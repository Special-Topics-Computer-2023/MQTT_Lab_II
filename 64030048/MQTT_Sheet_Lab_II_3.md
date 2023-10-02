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

** ถ้ากลัวผิดพลาด ให้แยก branch บน git แล้วแก้ได้โดยไม่ต้องกังวลว่า code จะเสียหาย

## >> จบการทดลอง  || 