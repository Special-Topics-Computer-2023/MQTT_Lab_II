# 64030313 นางสาวสุดารัตน์ ฆ้องอินตะ

## รับอินพุตจาก button สองตัว เพื่อควบคุม LED สองดวง

```c
#include <stdio.h>
#include <stdbool.h>
#include <unistd.h>

#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "freertos/queue.h"
#include "driver/gpio.h"
#include "esp_log.h"


#define INPUT_PIN 21
#define INPUT_PIN2 22
#define LED_PIN 5
#define LED_PIN2 4

int state = 0;
xQueueHandle interputQueue;

// interrupt service handler
static void IRAM_ATTR gpio_interrupt_handler(void *args)
{
    int pinNumber = (int)args;
    xQueueSendFromISR(interputQueue, &pinNumber, NULL);
}

// LED control task, received button prerssed from ISR
void LED_Control_Task(void *params)
{
    int pinNumber = 0;
    while (true)
    {
        if (xQueueReceive(interputQueue, &pinNumber, portMAX_DELAY))
        {
            gpio_set_level(LED_PIN, gpio_get_level(LED_PIN)==0);
            printf("GPIO %d was pressed. The state is %d\n", pinNumber,  gpio_get_level(LED_PIN));
        }
    }
}
void LED2_Control_Task(void *params)
{
    int pinNumber = 0;
    while (true)
    {
        if (xQueueReceive(interputQueue, &pinNumber, portMAX_DELAY))
        {
            gpio_set_level(LED_PIN2, gpio_get_level(LED_PIN2)==0);
            printf("GPIO %d was pressed. The state is %d\n", pinNumber,  gpio_get_level(LED_PIN2));
        }
    }
}
void app_main()
{
	// LED config as I/O port
    gpio_pad_select_gpio(LED_PIN);
    gpio_set_direction(LED_PIN, GPIO_MODE_INPUT_OUTPUT);
    gpio_pad_select_gpio(LED_PIN2);
    gpio_set_direction(LED_PIN2, GPIO_MODE_INPUT_OUTPUT);

    // config Input No 21 for external interrupt input
    gpio_pad_select_gpio(INPUT_PIN);
    gpio_set_direction(INPUT_PIN, GPIO_MODE_INPUT);
    gpio_pulldown_en(INPUT_PIN);
    gpio_pullup_dis(INPUT_PIN);
    gpio_set_intr_type(INPUT_PIN, GPIO_INTR_POSEDGE);
    gpio_pad_select_gpio(INPUT_PIN2);
    gpio_set_direction(INPUT_PIN2, GPIO_MODE_INPUT);
    gpio_pulldown_en(INPUT_PIN2);
    gpio_pullup_dis(INPUT_PIN2);
    gpio_set_intr_type(INPUT_PIN2, GPIO_INTR_POSEDGE);

    // FreeRTOS tas and queue
    interputQueue = xQueueCreate(10, sizeof(int));
    xTaskCreate(LED_Control_Task, "LED_Control_Task", 2048, NULL, 1, NULL);
    xTaskCreate(LED2_Control_Task, "LED_Control_Task", 2048, NULL, 1, NULL);

    // install isr service and isr handler
    gpio_install_isr_service(0);
    gpio_isr_handler_add(INPUT_PIN, gpio_interrupt_handler, (void *)INPUT_PIN);
    gpio_isr_handler_add(INPUT_PIN2, gpio_interrupt_handler, (void *)INPUT_PIN2);
}
```

## Terminal
![image](https://github.com/NamaoySudarat/MQTT_Lab_II/assets/115037574/966cd7fa-1e38-4488-90fd-657a3270e834)
