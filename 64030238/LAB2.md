# 64030238 Jasda Singhapooti

# LAB2
![image](https://github.com/JASDA0000/MQTT_Lab_II/assets/103983336/b2e35c72-21e0-4c88-bfd3-825ee4ea0fb6)
![image](https://github.com/JASDA0000/MQTT_Lab_II/assets/103983336/334886a0-e1c7-4618-9c64-0908e1f46553)

```c
#include <stdio.h>
#include <stdbool.h>
#include <unistd.h>

#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "freertos/queue.h"
#include "driver/gpio.h"
#include "esp_log.h"

#define BUTTON1_PIN 21
#define BUTTON2_PIN 22
#define LED1_PIN 5
#define LED2_PIN 4

int state = 0;
xQueueHandle interputQueue1;
xQueueHandle interputQueue2;

// interrupt service handler
static void IRAM_ATTR gpio_interrupt_handler(void *args)
{
    int pinNumber = (int)args;
    xQueueSendFromISR(interputQueue1, &pinNumber, NULL);
}

static void IRAM_ATTR gpio_interrupt_handler2(void *args)
{
    int pinNumber = (int)args;
    xQueueSendFromISR(interputQueue2, &pinNumber, NULL);
}

// LED control task, received button prerssed from ISR
void LED_Control_Task(void *params)
{
    int pinNumber = 0;
    while (true)
    {
        if (xQueueReceive(interputQueue1, &pinNumber, portMAX_DELAY))
        {
            gpio_set_level(LED1_PIN, gpio_get_level(LED1_PIN) == 0);
            printf("GPIO %d was pressed. The state is %d\n", pinNumber, gpio_get_level(LED1_PIN));
        }
    }
}

void LED2_Control_Task(void *params)
{
    int pinNumber = 0;
    while (true)
    {
        if (xQueueReceive(interputQueue2, &pinNumber, portMAX_DELAY))
        {
            gpio_set_level(LED2_PIN, gpio_get_level(LED2_PIN) == 0);
            printf("GPIO %d was pressed. The state is %d\n", pinNumber, gpio_get_level(LED1_PIN));
        }
    }
}

void app_main()
{
    // LED config as I/O port
    gpio_pad_select_gpio(LED1_PIN);
    gpio_set_direction(LED1_PIN, GPIO_MODE_INPUT_OUTPUT);
    gpio_pad_select_gpio(LED2_PIN);
    gpio_set_direction(LED2_PIN, GPIO_MODE_INPUT_OUTPUT);

    // config Input No 21 for external interrupt input
    gpio_pad_select_gpio(BUTTON1_PIN);
    gpio_set_direction(BUTTON1_PIN, GPIO_MODE_INPUT);
    gpio_pulldown_en(BUTTON1_PIN);
    gpio_pullup_dis(BUTTON1_PIN);
    gpio_set_intr_type(BUTTON1_PIN, GPIO_INTR_POSEDGE);

    gpio_pad_select_gpio(BUTTON2_PIN);
    gpio_set_direction(BUTTON2_PIN, GPIO_MODE_INPUT);
    gpio_pulldown_en(BUTTON2_PIN);
    gpio_pullup_dis(BUTTON2_PIN);
    gpio_set_intr_type(BUTTON2_PIN, GPIO_INTR_POSEDGE);

    // FreeRTOS tas and queue
    interputQueue1 = xQueueCreate(10, sizeof(int));
    interputQueue2 = xQueueCreate(10, sizeof(int));
    xTaskCreate(LED_Control_Task, "LED_Control_Task", 2048, NULL, 1, NULL);
    xTaskCreate(LED2_Control_Task, "LED_Control_Task", 2048, NULL, 1, NULL);

    // install isr service and isr handler
    gpio_install_isr_service(0);
    gpio_isr_handler_add(BUTTON1_PIN, gpio_interrupt_handler, (void *)BUTTON1_PIN);
    gpio_isr_handler_add(BUTTON2_PIN, gpio_interrupt_handler2, (void *)BUTTON2_PIN);
}
```
