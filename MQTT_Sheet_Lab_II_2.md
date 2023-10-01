# MQTT_Lab_I
การทดลอง MQTT ตอนที่ 1  การใช้ ESP32 เป็น MQTT Subscriber

# 2. การทดลอง การรับปุ่มกดโดยใช้การ interrupt

## 2.1 ทดลองสร้างโปรเจคสำหรับการรับอินพุตแบบ interrupt

### 2.1.1 สร้าง projrct ใหม่ โดยไม่ต้องเลือก template

### 2.1.2 แก้ไข code ใน main.c ให้เป็นดังต่อไปนี้

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
#define LED_PIN 5

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

void app_main()
{
	// LED config as I/O port
    gpio_pad_select_gpio(LED_PIN);
    gpio_set_direction(LED_PIN, GPIO_MODE_INPUT_OUTPUT);

    // config Input No 21 for external interrupt input
    gpio_pad_select_gpio(INPUT_PIN);
    gpio_set_direction(INPUT_PIN, GPIO_MODE_INPUT);
    gpio_pulldown_en(INPUT_PIN);
    gpio_pullup_dis(INPUT_PIN);
    gpio_set_intr_type(INPUT_PIN, GPIO_INTR_POSEDGE);

    // FreeRTOS tas and queue
    interputQueue = xQueueCreate(10, sizeof(int));
    xTaskCreate(LED_Control_Task, "LED_Control_Task", 2048, NULL, 1, NULL);

    // install isr service and isr handler
    gpio_install_isr_service(0);
    gpio_isr_handler_add(INPUT_PIN, gpio_interrupt_handler, (void *)INPUT_PIN);
}

```

### 2.1.3 เชื่อมต่อวงจรตาม config ต่อไปนี้

```c
#define INPUT_PIN 21
#define LED_PIN 5
```

###  2.1.4 Build และรันโปรแกรม 

ตัวอย่างผลที่ได้ เมื่อกดปุ่ม button  

![Alt text](./Pictures/Picture-16.png)

## <<<งานที่ต้องทำ>>>

1. รับอินพุตจาก button สองตัว เพื่อควบคุม LED สองดวง โดยใช้การ interrupt ที่ดัดแปลงมาจากตัวอย่างในการทดลองนี้


 
##  [>> หัวข้อต่อไป >>](./MQTT_Sheet_Lab_II_3.md) 