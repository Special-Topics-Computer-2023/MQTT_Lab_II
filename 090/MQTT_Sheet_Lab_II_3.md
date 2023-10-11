# MQTT_Lab_I
การทดลอง MQTT ตอนที่ 1  การใช้ ESP32 เป็น MQTT Subscriber

# 2. การทดลอง การรับปุ่มกดโดยใช้การ interrupt แล้ว publish ไปยัง MQTT

## 2.1 ทดลองสร้างโปรเจคสำหรับการรับอินพุตแบบ interrupt

### 2.1.1 เชื่อมต่อวงจรตาม config ต่อไปนี้

```c
INPUT_PIN  = GPIO21
LED_PIN    = GPIO5
```

### 2.1.2 กลับไปที่โปรเจคที่ทำในใบงานข้อที่ 1  (ถ้าทำบน git ให้สร้าง  branch ใหม่ เช่น  `Publish-button` 

#### 1. เพิ่ม code ถัดจาก  #include ตัวล่างสุด เพื่อใช้ gpio

```c
#include "driver/gpio.h"
```

####  2. เพิ่ม interrupt handler และ led task ไว้ถัดจากบรรทัด `static const char *TAG = "MQTT_EXAMPLE";`

```c
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

```

####  3. เพิ่ม code สำหรับติดตั้ง gpio และ interrupt ใน app_main()

ใน `void app_main(void)` เพิ่ม code ต่อไปนี้ใต้บรรทัด `ESP_ERROR_CHECK(esp_event_loop_create_default());` (ก่อน blovk ของ comment)

```c
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

```

### 2.1.3 Build และ Run โปรแกรม

1. ทดลองกดปุ่ม เพื่อดูว่ามีการตรวจจับและแสดง log ออกมาหรือไม่

2. ถ้าไม่แสดงผลเช่นเดียวกับการทดลองในเรื่องการรับปุ่มกดโดยใช้การ interrupt ให้แก้ไขให้ถูกต้อง



###  2.1.4 Publish สถานะการกดปุ่มขึ้นบน  MQTT Broker

#### 1.  เพิ่มตัวแปรสำหรับใช้งานกับ MQTT client เป็นตัวแปร global 

เนื่องจาก client ที่ใช้งานใน `static void mqtt_event_handler( ... )` เป็นตัวแปร local เราไม่สามารถใช้ในฟังก์ชัน `void LED_Control_Task(void *params)` ได้ จึงต้องทำสำเนาออกมาเป็นตัวแปร global

แก้ไข code ในส่วนต้นของโปรแกรม

```c
#define INPUT_PIN 21
#define LED_PIN 5

int state = 0;
xQueueHandle interputQueue;
esp_mqtt_client_handle_t mqtt_client;  //<-- เพิ่มบรรทัดนี้

```

#### 2. ในฟังก์ชัน static void mqtt_app_start(void)

ให้สำเนาตัวแปร local  (client) ไว้ในตัวแปร global  (mqtt_client)

```c
static void mqtt_app_start(void)
{
    ... 
    esp_mqtt_client_handle_t client = esp_mqtt_client_init(&mqtt_cfg);
    mqtt_client = client;  //<-- เพิ่มบรรทัดนี้ ถัดจากนั้นยังเหมือนเดิม
    ...
}
```

#### 3. publish ข้อมูลขึ้นบน MQTT Broker

แก้ไขฟังก์ชัน  LED_Control_Task ให้เป็นดังต่อไปนี้

```c
void LED_Control_Task(void *params)
{
    int pinNumber = 0;
    char data[3]; // <-- เพิ่มตัวแปรเก็บ string ที่จะส่ง
    while (true)
    {
        if (xQueueReceive(interputQueue, &pinNumber, portMAX_DELAY))
        {
            gpio_set_level(LED_PIN, gpio_get_level(LED_PIN)==0);
            printf("GPIO %d was pressed. The state is %d\n", pinNumber,  gpio_get_level(LED_PIN));
            sprintf(data,"%d",gpio_get_level(LED_PIN));  // <-- แปลงสถานะของ LED  (int) เป็น string
            esp_mqtt_client_publish(mqtt_client, "/stu_999/lamp1", data, 0, 0, 0); // <-- publish ไปยัง broker
        }
    }
}
```

###  2.1.5 ทดสอบการทำงาน

ถ้าโปรแกรมทุกอย่างถูกต้อง เมื่อกด button จะเกิดการ publish ส่งสถานะของ LED ขึ้นบน broker สังเกตุได้จาก MQTT Explorer

![Alt text](./Pictures/Picture-15.png)



## <<<งานที่ต้องทำ>>>

1. รับอินพุตจาก button สองตัว แบบ interrupt เพื่อควบคุม LED สองดวง แล้วส่งไปยัง MQTT Broker  ทดสอบโดยการแสดงผลบน MQTT Explorer (ดัดแปลงเพิ่มเติมจาก code ในใบงาน)
```
#include <stdio.h>
#include <stdbool.h>
#include <unistd.h>
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "freertos/queue.h"
#include "driver/gpio.h"
#include "esp_log.h"

#include "mqtt_client.h"

#define BUTTON1_PIN 21
#define BUTTON2_PIN 22
#define LED1_PIN 5
#define LED2_PIN 18

xQueueHandle buttonQueue;
static const char *TAG = "MQTT_EXAMPLE";

static esp_mqtt_client_handle_t mqtt_client;

int state = 0;
bool led1_state = false;
bool led2_state = false;

static void IRAM_ATTR button_interrupt_handler(void *args)
{
    int pinNumber = (int)args;
    xQueueSendFromISR(buttonQueue, &pinNumber, NULL);
}

void LED_Control_Task(void *params)
{
    int button1_pressed = 0;
    int button2_pressed = 0;
    int pinNumber = 0;

    while (true)
    {
        if (xQueueReceive(buttonQueue, &pinNumber, portMAX_DELAY))
        {
            if (pinNumber == BUTTON1_PIN)
            {
                button1_pressed = 1;
                led1_state = !led1_state;
                gpio_set_level(LED1_PIN, led1_state ? 1 : 0);
                ESP_LOGI(TAG, "Button 1 pressed. LED 1 state is %s", led1_state ? "ON" : "OFF");
            }
            else if (pinNumber == BUTTON2_PIN)
            {
                button2_pressed = 1;
                led2_state = !led2_state;
                gpio_set_level(LED2_PIN, led2_state ? 1 : 0);
                ESP_LOGI(TAG, "Button 2 pressed. LED 2 state is %s", led2_state ? "ON" : "OFF");
            }

            if (button1_pressed && button2_pressed)
            {
                button1_pressed = 0;
                button2_pressed = 0;

                // Publish LED states to MQTT when both buttons are pressed
                char payload[10];
                snprintf(payload, sizeof(payload), "%d%d", led1_state, led2_state);
                esp_mqtt_client_publish(mqtt_client, "/stu_090/leds", payload, 0, 0, 0);
                ESP_LOGI(TAG, "Published LED states to MQTT: %s", payload);
            }
        }
    }
}

static esp_err_t mqtt_event_handler_cb(esp_mqtt_event_handle_t event)
{
    esp_mqtt_client_handle_t client = event->client;
    int msg_id;

    switch (event->event_id)
    {
    case MQTT_EVENT_CONNECTED:
        ESP_LOGI(TAG, "MQTT_EVENT_CONNECTED");
        msg_id = esp_mqtt_client_subscribe(client, "/stu_090/leds", 0);
        ESP_LOGI(TAG, "Subscribed to /stu_090/leds, msg_id=%d", msg_id);
        break;
    case MQTT_EVENT_DATA:
        ESP_LOGI(TAG, "MQTT_EVENT_DATA");
        if (strncmp(event->topic, "/stu_090/leds", event->topic_len) == 0)
        {
            char data[10];
            strncpy(data, event->data, event->data_len);
            data[event->data_len] = '\0';
            ESP_LOGI(TAG, "Received data from /stu_090/leds: %s", data);

            if (strlen(data) == 2)
            {
                // Toggle LED states based on received data
                led1_state = (data[0] == '1');
                led2_state = (data[1] == '1');
                gpio_set_level(LED1_PIN, led1_state ? 1 : 0);
                gpio_set_level(LED2_PIN, led2_state ? 1 : 0);
                ESP_LOGI(TAG, "Updated LED states: LED1 %s, LED2 %s", led1_state ? "ON" : "OFF", led2_state ? "ON" : "OFF");
            }
        }
        break;
    default:
        break;
    }

    return ESP_OK;
}

void app_main(void)
{
    ESP_ERROR_CHECK(nvs_flash_init());
    ESP_ERROR_CHECK(esp_netif_init());
    ESP_ERROR_CHECK(esp_event_loop_create_default());

    gpio_pad_select_gpio(LED1_PIN);
    gpio_set_direction(LED1_PIN, GPIO_MODE_OUTPUT);
    gpio_pad_select_gpio(LED2_PIN);
    gpio_set_direction(LED2_PIN, GPIO_MODE_OUTPUT);

    buttonQueue = xQueueCreate(10, sizeof(int));
    xTaskCreate(LED_Control_Task, "LED_Control_Task", 2048, NULL, 10, NULL);

    gpio_install_isr_service(0);
    gpio_isr_handler_add(BUTTON1_PIN, button_interrupt_handler, (void *)BUTTON1_PIN);
    gpio_isr_handler_add(BUTTON2_PIN, button_interrupt_handler, (void *)BUTTON2_PIN);

    // Configure the MQTT client
    esp_mqtt_client_config_t mqtt_cfg = {
        .uri = CONFIG_BROKER_URL,
    };
    mqtt_client = esp_mqtt_client_init(&mqtt_cfg);
    esp_mqtt_client_register_event(mqtt_client, ESP_EVENT_ANY_ID, mqtt_event_handler_cb, NULL);
    esp_mqtt_client_start(mqtt_client);
}
```
** ถ้ากลัวผิดพลาด ให้แยก branch บน git แล้วแก้ได้โดยไม่ต้องกังวลว่า code จะเสียหาย

## >> จบการทดลอง  || 
