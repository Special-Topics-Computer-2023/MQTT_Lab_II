## สร้างโปรเจกต์ (1.2.2)
![image](https://github.com/NamaoySudarat/MQTT_Lab_II/assets/115037574/7861c5b8-1377-407c-949e-e36f16b188a6)
1. ตั้งชื่อโปรเจกต์
2. เลือก templates
3. กดปุ่ม finish เพื่อสร้างโปรเจกต์

## Build โปรแกรม (1.2.4)
![image-1](https://github.com/NamaoySudarat/MQTT_Lab_II/assets/115037574/fd977f5e-d4e2-4606-b99a-16b8ca8bd7c8)
1. ปุ่ม Build
2. ผลลัพธ์หลังการ Build โปรแกรม

## ตรวจสอบไฟล์ (1.2.5)
![image-2](https://github.com/NamaoySudarat/MQTT_Lab_II/assets/115037574/8a4ad374-3aa4-49d3-bb1e-df1f0da6b462)
![image-3](https://github.com/NamaoySudarat/MQTT_Lab_II/assets/115037574/bbc9adf9-df4d-4fe4-83cc-e5152199ef49)
1. ตรวจไฟล์ใน folder build
2. ตรวจ esp idf components
3. ไฟล์ตัวอย่าง

## เชื่อมต่อ MQTT
![image-6](https://github.com/NamaoySudarat/MQTT_Lab_II/assets/115037574/3a82b028-e712-4526-8734-b60b1d42d46c)

# publish
![image-8](https://github.com/NamaoySudarat/MQTT_Lab_II/assets/115037574/6e71c86d-f8b0-4ec3-9fe1-0e4965718da5)

## Output ใน terminal หลังจากส่งมาจาก MQTT Explorer
![image-7](https://github.com/NamaoySudarat/MQTT_Lab_II/assets/115037574/7320f063-7f3e-45e2-b17c-a45a28993a3b)

## แก้ไข code เพื่อควบคุม LED
- เพิ่ม code นี้ เพื่อใช้งานขา GPIO
```
#include <driver/gpio.h>
```
- กำหนดค่าของขา GPIO
```
gpio_reset_pin(GPIO_NUM_23);
gpio_set_direction(GPIO_NUM_23, GPIO_MODE_OUTPUT);
```
- เพิ่ม code ควบคุม LED
```
if(strncmp(event->topic, "/stu_313/lamp1", event->topic_len) == 0)  // if topic is "/stu_999/lamp1" then result = 0
{
    ESP_LOGI(TAG, "event->topic = /stu_313/lamp1");
    if(strncmp(event->data, "1", event->data_len) == 0) // if data is "1" then result = 0
    {
        ESP_LOGI(TAG, "Turn on LED");
        gpio_set_level(GPIO_NUM_23, 1);
    }
    if(strncmp(event->data, "0", event->data_len) == 0) // if data is "0" then result = 0
    {
        ESP_LOGI(TAG, "Turn off LED");
        gpio_set_level(GPIO_NUM_23, 0);
    }
}
```
## ทดลอง Publish เพื่อควบคุม LED
![image-9](https://github.com/NamaoySudarat/MQTT_Lab_II/assets/115037574/bfa2da28-c460-4ac3-89e5-c4cdc819ed9e)

## Output จาก terminal
![image-10](https://github.com/NamaoySudarat/MQTT_Lab_II/assets/115037574/58097570-5af0-4bc7-9aeb-f5f77eaa8757)

## แก้ไข code เพื่อควบคุม LED 2 ดวง
- เพิ่มเพื่อ subscribe topic `/stu_313/lamp2`
```
msg_id = esp_mqtt_client_subscribe(client, "/stu_313/lamp2", 0);
ESP_LOGI(TAG, "sent subscribe successful, msg_id=%d", msg_id);
```
- กำหนดค่าของขา GPIO
```
gpio_reset_pin(GPIO_NUM_22);
gpio_set_direction(GPIO_NUM_22, GPIO_MODE_OUTPUT);
```
- เพิ่ม code ควบคุม LED ของดวงที่ 2
```
if(strncmp(event->topic, "/stu_313/lamp2", event->topic_len) == 0)
{
    ESP_LOGI(TAG, "event->topic = /stu_313/lamp2");
    if(strncmp(event->data, "1", event->data_len) == 0)
    {
        ESP_LOGI(TAG, "Turn on LED2");
        gpio_set_level(GPIO_NUM_22, 1);
    }
    if(strncmp(event->data, "0", event->data_len) == 0) 
    {
        ESP_LOGI(TAG, "Turn off LED2");
        gpio_set_level(GPIO_NUM_22, 0);
    }
}
```

## ทดลอง Publish เพื่อควบคุม LED ทั้ง 2 ดวง
![image-15](https://github.com/NamaoySudarat/MQTT_Lab_II/assets/115037574/c6286de3-2a0e-41d0-b244-4daadfa0c5aa)
![image-16](https://github.com/NamaoySudarat/MQTT_Lab_II/assets/115037574/93e22fd6-10cc-4558-b705-428c2755717e)

## Output จาก terminal
![image-17](https://github.com/NamaoySudarat/MQTT_Lab_II/assets/115037574/de7211f0-f741-44aa-8646-948a091012ea)
![image-18](https://github.com/NamaoySudarat/MQTT_Lab_II/assets/115037574/2bcf8ef2-9e82-4ab8-9a2b-5f5ec122d61c)
