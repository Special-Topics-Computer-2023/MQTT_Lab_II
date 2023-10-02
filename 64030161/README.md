# 64030161 Rachata Supanurak MQTT-II lab1

## สร้าง Project

![image](https://github.com/RachataS/MQTT_Lab_II/assets/115066261/8726a8ee-4f11-4c6b-bdd5-56f84fea075a)

เลือก template เป็น MQTT tcp
<img width="1582" alt="Screenshot 2566-10-02 at 09 45 18" src="https://github.com/RachataS/MQTT_Lab_II/assets/115066261/aeb22253-de1b-45f4-ac96-04587a56dc91">

## build project 

กดรูปถังเพื่อ build project
<img width="1582" alt="Screenshot 2566-10-02 at 09 47 40" src="https://github.com/RachataS/MQTT_Lab_II/assets/115066261/d66d1c4c-e70a-4c62-97ac-1b3824ac28fa">

## รันโปรแกรม

ผลลับธ์จะบอกว่าเชื่อมต่อกับ MQTT ได้แล้ว โดยจะเชื่อมต่อกับ MQTT_EVENT_DATA

```css
entry 0x40080698
I (27) boot: ESP-IDF v4.4.5-dirty 2nd stage bootloader
I (27) boot: compile time 09:49:05
I (27) boot: chip revision: v3.0
I (31) boot.esp32: SPI Speed      : 40MHz
I (36) boot.esp32: SPI Mode       : DIO
I (40) boot.esp32: SPI Flash Size : 4MB
I (45) boot: Enabling RNG early entropy source...
I (50) boot: Partition Table:
I (54) boot: ## Label            Usage          Type ST Offset   Length
I (61) boot:  0 nvs              WiFi data        01 02 00009000 00006000
I (68) boot:  1 phy_init         RF data          01 01 0000f000 00001000
I (76) boot:  2 factory          factory app      00 00 00010000 00100000
I (83) boot: End of partition table
I (88) esp_image: segment 0: paddr=00010020 vaddr=3f400020 size=1b780h (112512) map
I (137) esp_image: segment 1: paddr=0002b7a8 vaddr=3ffb0000 size=0307ch ( 12412) load
I (142) esp_image: segment 2: paddr=0002e82c vaddr=40080000 size=017ech (  6124) load
I (145) esp_image: segment 3: paddr=00030020 vaddr=400d0020 size=8ecb4h (584884) map
I (363) esp_image: segment 4: paddr=000becdc vaddr=400817ec size=130cch ( 78028) load
I (405) boot: Loaded app from partition at offset 0x10000
I (405) boot: Disabling RNG early entropy source...
I (416) cpu_start: Pro cpu up.
I (417) cpu_start: Starting app cpu, entry point is 0x4008117c
0x4008117c: call_start_cpu1 at /Users/rachatasupanurak/esp/esp-idf/components/esp_system/port/cpu_start.c:147

I (0) cpu_start: App cpu up.
I (433) cpu_start: Pro cpu start user code
I (433) cpu_start: cpu freq: 160000000
I (433) cpu_start: Application information:
I (438) cpu_start: Project name:     MQTT-II
I (442) cpu_start: App version:      927b033-dirty
I (448) cpu_start: Compile time:     Oct  2 2023 10:05:53
I (454) cpu_start: ELF file SHA256:  a368101e1a28e31e...
I (460) cpu_start: ESP-IDF:          v4.4.5-dirty
I (465) cpu_start: Min chip rev:     v0.0
I (470) cpu_start: Max chip rev:     v3.99 
I (475) cpu_start: Chip rev:         v3.0
I (480) heap_init: Initializing. RAM available for dynamic allocation:
I (487) heap_init: At 3FFAE6E0 len 00001920 (6 KiB): DRAM
I (493) heap_init: At 3FFB72F8 len 00028D08 (163 KiB): DRAM
I (499) heap_init: At 3FFE0440 len 00003AE0 (14 KiB): D/IRAM
I (505) heap_init: At 3FFE4350 len 0001BCB0 (111 KiB): D/IRAM
I (512) heap_init: At 400948B8 len 0000B748 (45 KiB): IRAM
I (519) spi_flash: detected chip: generic
I (523) spi_flash: flash io: dio
I (528) cpu_start: Starting scheduler on PRO CPU.
I (0) cpu_start: Starting scheduler on APP CPU.
I (538) MQTT_EXAMPLE: [APP] Startup..
I (538) MQTT_EXAMPLE: [APP] Free memory: 280724 bytes
I (548) MQTT_EXAMPLE: [APP] IDF version: v4.4.5-dirty
I (598) wifi:wifi driver task: 3ffbf600, prio:23, stack:6656, core=0
I (598) system_api: Base MAC address is not set
I (598) system_api: read default base MAC address from EFUSE
I (618) wifi:wifi firmware version: 0f80fa0
I (618) wifi:wifi certification version: v7.0
I (618) wifi:config NVS flash: enabled
I (618) wifi:config nano formating: disabled
I (628) wifi:Init data frame dynamic rx buffer num: 32
I (628) wifi:Init management frame dynamic rx buffer num: 32
I (638) wifi:Init management short buffer num: 32
I (638) wifi:Init dynamic tx buffer num: 32
I (648) wifi:Init static rx buffer size: 1600
I (648) wifi:Init static rx buffer num: 10
I (648) wifi:Init dynamic rx buffer num: 32
I (658) wifi_init: rx ba win: 6
I (658) wifi_init: tcpip mbox: 32
I (668) wifi_init: udp mbox: 6
I (668) wifi_init: tcp mbox: 6
I (668) wifi_init: tcp tx win: 5744
I (678) wifi_init: tcp rx win: 5744
I (678) wifi_init: tcp mss: 1440
I (688) wifi_init: WiFi IRAM OP enabled
I (688) wifi_init: WiFi RX IRAM OP enabled
I (698) example_connect: Connecting to TP-Link_E275...
I (698) phy_init: phy_version 4670,719f9f6,Feb 18 2021,17:07:07
I (798) wifi:mode : sta (24:d7:eb:0e:cb:20)
I (798) wifi:enable tsf
I (808) example_connect: Waiting for IP(s)
I (3218) wifi:new:<5,0>, old:<1,0>, ap:<255,255>, sta:<5,0>, prof:1
I (3958) wifi:state: init -> auth (b0)
I (3968) wifi:state: auth -> assoc (0)
I (3978) wifi:new:<5,2>, old:<5,0>, ap:<255,255>, sta:<5,2>, prof:1
I (3978) wifi:state: assoc -> run (10)
I (4008) wifi:connected with TP-Link_E275, aid = 4, channel 5, 40D, bssid = 60:a4:b7:62:e2:75
I (4008) wifi:security: WPA2-PSK, phy: bgn, rssi: -61
I (4008) wifi:pm start, type: 1

I (4078) wifi:AP's beacon interval = 102400 us, DTIM period = 1
I (4078) wifi:new:<5,0>, old:<5,2>, ap:<255,255>, sta:<5,0>, prof:1
I (5588) example_connect: Got IPv6 event: Interface "example_connect: sta" address: fe80:0000:0000:0000:26d7:ebff:fe0e:cb20, type: ESP_IP6_ADDR_IS_LINK_LOCAL
I (5598) wifi:<ba-add>idx:0 (ifx:0, 60:a4:b7:62:e2:75), tid:0, ssn:2, winSize:64
I (8518) esp_netif_handlers: example_connect: sta ip: 192.168.0.123, mask: 255.255.255.0, gw: 192.168.0.1
I (8518) example_connect: Got IPv4 event: Interface "example_connect: sta" address: 192.168.0.123
I (8528) example_connect: Connected to example_connect: sta
I (8528) example_connect: - IPv4 address: 192.168.0.123
I (8538) example_connect: - IPv6 address: fe80:0000:0000:0000:26d7:ebff:fe0e:cb20, type: ESP_IP6_ADDR_IS_LINK_LOCAL
I (8548) MQTT_EXAMPLE: Other event id:7
I (9818) MQTT_EXAMPLE: MQTT_EVENT_CONNECTED
I (9828) MQTT_EXAMPLE: sent publish successful, msg_id=52314
I (9828) MQTT_EXAMPLE: sent subscribe successful, msg_id=14598
I (9828) MQTT_EXAMPLE: sent subscribe successful, msg_id=12410
I (9838) MQTT_EXAMPLE: sent unsubscribe successful, msg_id=3036
I (10448) MQTT_EXAMPLE: MQTT_EVENT_PUBLISHED, msg_id=52314
I (10848) MQTT_EXAMPLE: MQTT_EVENT_SUBSCRIBED, msg_id=14598
I (10858) MQTT_EXAMPLE: sent publish successful, msg_id=0
I (11488) MQTT_EXAMPLE: MQTT_EVENT_SUBSCRIBED, msg_id=12410
I (11488) MQTT_EXAMPLE: sent publish successful, msg_id=0
I (11488) MQTT_EXAMPLE: MQTT_EVENT_UNSUBSCRIBED, msg_id=3036
I (11878) MQTT_EXAMPLE: MQTT_EVENT_DATA
TOPIC=/topic/qos0
DATA=data
I (12278) MQTT_EXAMPLE: MQTT_EVENT_DATA
TOPIC=/topic/qos0
DATA=data
```

เมื่อแก้ไขโค้ดใน case MQTT_EVENT_CONNECTED จะได้ output ดังนี้
```css
I (5528) esp_netif_handlers: example_connect: sta ip: 192.168.0.123, mask: 255.255.255.0, gw: 192.168.0.1
I (5528) example_connect: Got IPv4 event: Interface "example_connect: sta" address: 192.168.0.123
I (5588) example_connect: Got IPv6 event: Interface "example_connect: sta" address: fe80:0000:0000:0000:26d7:ebff:fe0e:cb20, type: ESP_IP6_ADDR_IS_LINK_LOCAL
I (5588) example_connect: Connected to example_connect: sta
I (5598) example_connect: - IPv4 address: 192.168.0.123
I (5598) example_connect: - IPv6 address: fe80:0000:0000:0000:26d7:ebff:fe0e:cb20, type: ESP_IP6_ADDR_IS_LINK_LOCAL
I (5608) wifi:<ba-add>idx:0 (ifx:0, 60:a4:b7:62:e2:75), tid:0, ssn:2, winSize:64
I (5628) MQTT_EXAMPLE: Other event id:7
I (6798) MQTT_EXAMPLE: sent unsubscribe successful, msg_id=24690
I (6798) MQTT_EXAMPLE: sent subscribe successful, msg_id=35432
I (7298) MQTT_EXAMPLE: MQTT_EVENT_UNSUBSCRIBED, msg_id=24690
I (7598) MQTT_EXAMPLE: MQTT_EVENT_SUBSCRIBED, msg_id=35432
I (7598) MQTT_EXAMPLE: sent publish successful, msg_id=0
```
เมื่อส่งข้อมูลใน MQTT Explorer ดังนี้

<img width="384" alt="Screenshot 2566-10-02 at 10 17 55" src="https://github.com/RachataS/MQTT_Lab_II/assets/115066261/e24750aa-b044-437b-b232-5da8e1159aca">

จะเห็นผลลัพธ์ใน terminal ดังนี้

<img width="453" alt="Screenshot 2566-10-02 at 10 18 46" src="https://github.com/RachataS/MQTT_Lab_II/assets/115066261/45a22b1d-0b52-489c-a7c2-eba3bda0580e">

ส่งค่า 0 1 เพื่อทดสอบคำสั่งสำหรับให้ LED ติด - ดับ

<img width="379" alt="Screenshot 2566-10-02 at 10 28 58" src="https://github.com/RachataS/MQTT_Lab_II/assets/115066261/3867c1e0-b84e-464f-8b1e-26598a10abb5">

จะเห็นผลลัพธ์ใน terminal ดังนี้

<img width="486" alt="Screenshot 2566-10-02 at 10 29 17" src="https://github.com/RachataS/MQTT_Lab_II/assets/115066261/3623bce3-458e-49b7-93d2-4017106b5ef2">

## เพิ่มโค้ดเพื่อให้สามารถควบคุมไฟ ติด ดับได้

define เพื่อเก็บค่่า pin
```css
#define LED1 23
```

reset pin และ ตั้งค่า pin ให้เป็น output ใน app_main
```css
     gpio_reset_pin(LED1);
    gpio_set_direction(LED1, GPIO_MODE_OUTPUT);
```

แก้ไข code ใน case MQTT_EVENT_DATA ใน switch case เพื่อควบคุม LED
```css
    case MQTT_EVENT_DATA:
        ESP_LOGI(TAG, "MQTT_EVENT_DATA");
        printf("TOPIC=%.*s\r\n", event->topic_len, event->topic);
        printf("DATA=%.*s\r\n", event->data_len, event->data);

        if (strncmp(event->topic, "/stu_161/lamp1", event->topic_len) == 0) // if topic is "/stu_999/lamp1" then result = 0
        {
            ESP_LOGI(TAG, "event->topic = /stu_161/lamp1");
            if (strncmp(event->data, "1", event->data_len) == 0) // if data is "1" then result = 0
            {
                ESP_LOGI(TAG, "Turn on LED");
                gpio_set_level(LED1, 1);
            }
            if (strncmp(event->data, "0", event->data_len) == 0) // if data is "0" then result = 0
            {
                ESP_LOGI(TAG, "Turn off LED");
                gpio_set_level(LED1, 0);
            }
        }
        break;
```
เมื่อส่ง 1 ใน MQTT Explorer ไฟจะติดและแสลงผลดังนี้

<img width="399" alt="Screenshot 2566-10-02 at 10 50 09" src="https://github.com/RachataS/MQTT_Lab_II/assets/115066261/42ba52fd-7175-4a4a-a5fa-a29a50283eb7">

<img width="397" alt="Screenshot 2566-10-02 at 10 50 33" src="https://github.com/RachataS/MQTT_Lab_II/assets/115066261/a766c9fc-746e-40b7-8a45-c5e78e384865">

เมื่อส่ง 0 ใน MQTT Explorer ไฟจดับและแสลงผลดังนี้

<img width="400" alt="Screenshot 2566-10-02 at 10 51 36" src="https://github.com/RachataS/MQTT_Lab_II/assets/115066261/67b8ec1e-0bcd-4304-a3bd-9dc1fc1313be">

<img width="412" alt="Screenshot 2566-10-02 at 10 51 51" src="https://github.com/RachataS/MQTT_Lab_II/assets/115066261/052624f9-6bfb-4f74-b48c-a82887d98747">

## แก้ไขโค้ดเพื่อให้ควบคุม LED 2 ดวง

define เพื่อเก็บค่่า pin
```css
#define LED1 23
#define LED2 22
```

reset pin และ ตั้งค่า pin ให้เป็น output ใน app_main
```css
     gpio_reset_pin(LED1);
    gpio_reset_pin(LED2);
    gpio_set_direction(LED1, GPIO_MODE_OUTPUT);
    gpio_set_direction(LED2, GPIO_MODE_OUTPUT);
```

เพิ่ม subscribe ใน case MQTT_EVENT_CONNECTED
```css
msg_id = esp_mqtt_client_subscribe(client, "/stu_161/lamp2", 0);
```

เพิ่ม code ใน case MQTT_EVENT_DATA ใน switch case เพื่อควบคุม LED 2 ดวง
```css
 if (strncmp(event->topic, "/stu_161/lamp2", event->topic_len) == 0) // if topic is "/stu_999/lamp1" then result = 0
        {
            ESP_LOGI(TAG, "event->topic = /stu_161/lamp2");
            if (strncmp(event->data, "1", event->data_len) == 0) // if data is "1" then result = 0
            {
                ESP_LOGI(TAG, "Turn on LED2");
                gpio_set_level(LED2, 1);
            }
            if (strncmp(event->data, "0", event->data_len) == 0) // if data is "0" then result = 0
            {
                ESP_LOGI(TAG, "Turn off LED2");
                gpio_set_level(LED2, 0);
            }
        }
```

เมื่อส่งค่าไปใน topic lamp2 เป็น 1 จะแสดงผลใน terminal และไฟจะติด

<img width="385" alt="Screenshot 2566-10-02 at 11 19 58" src="https://github.com/RachataS/MQTT_Lab_II/assets/115066261/3c83746f-2862-4ebe-b3d2-b2b15d6b55cd">

<img width="384" alt="Screenshot 2566-10-02 at 11 21 02" src="https://github.com/RachataS/MQTT_Lab_II/assets/115066261/2adb9ad9-9c24-4d79-8054-c5828037c606">

repo - https://github.com/RachataS/MQTT-II.git

[>> Lab2 >>](./lab2.md) 
