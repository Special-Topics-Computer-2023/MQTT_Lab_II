# 64030234 นางสาวกุลธิดา  เกษร

## 1.2.2 
- 1 ตั้งชื่อโปรเจคชื่อว่า esp32-mqtt-tcp
- 2 เลือก templete ชื่อว่า tcp
- 3 สร้างโปรเจค

## 1.2.4
- 1 build Project
- 2 แสดงผลลัพธ์ที่ได้จากการ build พบว่า ไม่มี error

 ## 1.2.5
  - 1 ตรวจสอบไฟล์โฟลเดอร์ build
  - 2 ตรวจสอบไฟล์โฟลเดอร์ esp idf component
  - 3 แสดงโค้ดตัวอย่างไฟล์ example

  ## เชื่อมต่อ MQTT Explorer
![เชื่อมต่อ MQTT Explorer](https://github.com/KUNTIDA234/MQTT_Lab_II/assets/115066215/13ec1584-47f8-4090-9a3f-21b3ff737cd1)

## publish 
![pub1](https://github.com/KUNTIDA234/MQTT_Lab_II/assets/115066215/b442814c-8266-4fd5-977b-8d8695996b2d)

## output terminal
![image](https://github.com/KUNTIDA234/MQTT_Lab_II/assets/115066215/0aafaf06-3e96-4b6b-a82e-79274f732c47)

## แก้ไขโค้ดเพื่อควบคุม led
```
#include <driver/gpio.h>
gpio_reset_pin(GPIO_NUM_23);
gpio_set_direction(GPIO_NUM_23, GPIO_MODE_OUTPUT);
```
```
   if(strncmp(event->topic, "/stu_234/lamp1", event->topic_len) == 0)  // if topic is "/stu_999/lamp1" then result = 0
                       {
                           ESP_LOGI(TAG, "event->topic = /stu_234/lamp1");
                           if(strncmp(event->data, "1", event->data_len) == 0) // if data is "1" then result = 0
                           {
                               ESP_LOGI(TAG, "Turn on LED");
                               // Todo: add code to turn on LED
                               gpio_set_level(GPIO_NUM_23, 1);
                           }
                           if(strncmp(event->data, "0", event->data_len) == 0) // if data is "0" then result = 0
                           {
                               ESP_LOGI(TAG, "Turn off LED");
                               // Todo: add code to turn off LED
                               gpio_set_level(GPIO_NUM_23, 0);

                           }
                       }
```
## แก้ไขโค้ดเพื่อควบคุม led 2 ดวง
```
msg_id = esp_mqtt_client_subscribe(client, "/stu_234/lamp2", 0);
ESP_LOGI(TAG, "sent subscribe successful, msg_id=%d", msg_id);
gpio_reset_pin(GPIO_NUM_22);
gpio_set_direction(GPIO_NUM_22, GPIO_MODE_OUTPUT);
```
```
   if(strncmp(event->topic, "/stu_234/lamp2", event->topic_len) == 0)  // if topic is "/stu_999/lamp1" then result = 0
                {
                    ESP_LOGI(TAG, "event->topic = /stu_234/lamp2");
                    if(strncmp(event->data, "1", event->data_len) == 0) // if data is "1" then result = 0
                    {
                        ESP_LOGI(TAG, "Turn on LED");
                        // Todo: add code to turn on LED
                        gpio_set_level(GPIO_NUM_22, 1);
                    }
                    if(strncmp(event->data, "0", event->data_len) == 0) // if data is "0" then result = 0
                    {
                        ESP_LOGI(TAG, "Turn off LED");
                        // Todo: add code to turn off LED
                        gpio_set_level(GPIO_NUM_22, 0);

                    }
                }

```
## publish 
![image](https://github.com/KUNTIDA234/MQTT_Lab_II/assets/115066215/c0041353-2c72-4738-b3ff-97c714f05297)

## output terminal
![image](https://github.com/KUNTIDA234/MQTT_Lab_II/assets/115066215/fcc99216-6389-4844-a2f3-d8e7cf6fd68d)
=======
<<<<<<<< HEAD:64030234/readme.md
========
# 64030234 นางสาวกุลธิดา  เกษร

## 1.2.2 
- 1 ตั้งชื่อโปรเจคชื่อว่า esp32-mqtt-tcp
- 2 เลือก templete ชื่อว่า tcp
- 3 สร้างโปรเจค

## 1.2.4
- 1 build Project
- 2 แสดงผลลัพธ์ที่ได้จากการ build พบว่า ไม่มี error

 ## 1.2.5
  - 1 ตรวจสอบไฟล์โฟลเดอร์ build
  - 2 ตรวจสอบไฟล์โฟลเดอร์ esp idf component
  - 3 แสดงโค้ดตัวอย่างไฟล์ example

  ## เชื่อมต่อ MQTT Explorer
  ![image](https://github.com/KUNTIDA234/MQTT_Lab_II/assets/115066215/76bc97c8-3743-4f23-95e8-e1fdc2917982)

## publish 
![image](https://github.com/KUNTIDA234/MQTT_Lab_II/assets/115066215/a31c1ab4-b3ae-46d8-9bbe-d33b74791144)

## output terminal
![image](https://github.com/KUNTIDA234/MQTT_Lab_II/assets/115066215/0aafaf06-3e96-4b6b-a82e-79274f732c47)

## แก้ไขโค้ดเพื่อควบคุม led
```
#include <driver/gpio.h>
gpio_reset_pin(GPIO_NUM_23);
gpio_set_direction(GPIO_NUM_23, GPIO_MODE_OUTPUT);
```
```
   if(strncmp(event->topic, "/stu_234/lamp1", event->topic_len) == 0)  // if topic is "/stu_999/lamp1" then result = 0
                       {
                           ESP_LOGI(TAG, "event->topic = /stu_234/lamp1");
                           if(strncmp(event->data, "1", event->data_len) == 0) // if data is "1" then result = 0
                           {
                               ESP_LOGI(TAG, "Turn on LED");
                               // Todo: add code to turn on LED
                               gpio_set_level(GPIO_NUM_23, 1);
                           }
                           if(strncmp(event->data, "0", event->data_len) == 0) // if data is "0" then result = 0
                           {
                               ESP_LOGI(TAG, "Turn off LED");
                               // Todo: add code to turn off LED
                               gpio_set_level(GPIO_NUM_23, 0);

                           }
                       }
```
## แก้ไขโค้ดเพื่อควบคุม led 2 ดวง
```
msg_id = esp_mqtt_client_subscribe(client, "/stu_234/lamp2", 0);
ESP_LOGI(TAG, "sent subscribe successful, msg_id=%d", msg_id);
gpio_reset_pin(GPIO_NUM_22);
gpio_set_direction(GPIO_NUM_22, GPIO_MODE_OUTPUT);
```
```
   if(strncmp(event->topic, "/stu_234/lamp2", event->topic_len) == 0)  // if topic is "/stu_999/lamp1" then result = 0
                {
                    ESP_LOGI(TAG, "event->topic = /stu_234/lamp2");
                    if(strncmp(event->data, "1", event->data_len) == 0) // if data is "1" then result = 0
                    {
                        ESP_LOGI(TAG, "Turn on LED");
                        // Todo: add code to turn on LED
                        gpio_set_level(GPIO_NUM_22, 1);
                    }
                    if(strncmp(event->data, "0", event->data_len) == 0) // if data is "0" then result = 0
                    {
                        ESP_LOGI(TAG, "Turn off LED");
                        // Todo: add code to turn off LED
                        gpio_set_level(GPIO_NUM_22, 0);

                    }
                }

```
## publish 
![image](https://github.com/KUNTIDA234/MQTT_Lab_II/assets/115066215/c0041353-2c72-4738-b3ff-97c714f05297)

## output terminal
![image](https://github.com/KUNTIDA234/MQTT_Lab_II/assets/115066215/fcc99216-6389-4844-a2f3-d8e7cf6fd68d)

# Repo Project

https://github.com/KUNTIDA234/esp32-mqtt-tcp
>>>>>>>> 3c99b4b165254e6474b17cc8579d9a915746594a:64030234/Lab1.md
>>>>>>> 3c99b4b165254e6474b17cc8579d9a915746594a
