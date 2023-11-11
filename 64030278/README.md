# Poramat Phuangmanee
# สร้างโปรเจค เลือก tcp
![image](https://github.com/TikPoramat2545/MQTT_Lab_II/assets/134470274/e12b22b3-7687-4597-b52a-b964296ceadd)

![image](https://github.com/TikPoramat2545/MQTT_Lab_II/assets/134470274/8afd0a2a-f8e0-4779-9d1f-3f73f095b4b0)

# build and run program
## output
```css
entry 0x4008064c
I (27) boot: ESP-IDF v5.0.2-dirty 2nd stage bootloader
I (27) boot: compile time 13:00:34
I (28) boot: chip revision: v3.0
I (31) boot.esp32: SPI Speed      : 40MHz
I (36) boot.esp32: SPI Mode       : DIO
I (40) boot.esp32: SPI Flash Size : 2MB
I (45) boot: Enabling RNG early entropy source...
I (50) boot: Partition Table:
I (54) boot: ## Label            Usage          Type ST Offset   Length
I (61) boot:  0 nvs              WiFi data        01 02 00009000 00006000
I (68) boot:  1 phy_init         RF data          01 01 0000f000 00001000
I (76) boot:  2 factory          factory app      00 00 00010000 00100000
I (83) boot: End of partition table
I (87) esp_image: segment 0: paddr=00010020 vaddr=3f400020 size=248c0h (149696) map
I (150) esp_image: segment 1: paddr=000348e8 vaddr=3ffb0000 size=033bch ( 13244) load
I (155) esp_image: segment 2: paddr=00037cac vaddr=40080000 size=0836ch ( 33644) load
I (170) esp_image: segment 3: paddr=00040020 vaddr=400d0020 size=9376ch (604012) map
I (388) esp_image: segment 4: paddr=000d3794 vaddr=4008836c size=0c92ch ( 51500) load
I (420) boot: Loaded app from partition at offset 0x10000
I (420) boot: Disabling RNG early entropy source...
I (432) cpu_start: Pro cpu up.
I (432) cpu_start: Starting app cpu, entry point is 0x400812b8
0x400812b8: xt_highint4 at C:/Espressif/frameworks/esp-idf-v5.0.2/components/esp_system/port/soc/esp32/highint_hdl.S:287

I (0) cpu_start: App cpu up.
I (448) cpu_start: Pro cpu start user code
I (448) cpu_start: cpu freq: 160000000 Hz
I (448) cpu_start: Application information:
I (453) cpu_start: Project name:     mqtt_tcp
I (458) cpu_start: App version:      39179a4-dirty
I (463) cpu_start: Compile time:     Nov  9 2023 13:05:54
I (470) cpu_start: ELF file SHA256:  d3abb6d95672bfd6...
Warning: checksum mismatch between flashed and built applications. Checksum of built application is b1555dbbccc0a7e16dc6b54901a1cf3dc537fa27151265df16a84dfd3bbeca93
I (475) cpu_start: ESP-IDF:          v5.0.2-dirty
I (481) cpu_start: Min chip rev:     v0.0
I (486) cpu_start: Max chip rev:     v3.99 
I (490) cpu_start: Chip rev:         v3.0
I (495) heap_init: Initializing. RAM available for dynamic allocation:
I (503) heap_init: At 3FFAE6E0 len 00001920 (6 KiB): DRAM
I (508) heap_init: At 3FFB7638 len 000289C8 (162 KiB): DRAM
I (515) heap_init: At 3FFE0440 len 00003AE0 (14 KiB): D/IRAM
I (521) heap_init: At 3FFE4350 len 0001BCB0 (111 KiB): D/IRAM
I (527) heap_init: At 40094C98 len 0000B368 (44 KiB): IRAM
I (535) spi_flash: detected chip: generic
I (538) spi_flash: flash io: dio
W (542) spi_flash: Detected size(4096k) larger than the size in the binary image header(2048k). Using the size in the binary image header.
I (556) cpu_start: Starting scheduler on PRO CPU.
I (0) cpu_start: Starting scheduler on APP CPU.
I (566) MQTT_EXAMPLE: [APP] Startup..
I (566) MQTT_EXAMPLE: [APP] Free memory: 281356 bytes
I (576) MQTT_EXAMPLE: [APP] IDF version: v5.0.2-dirty
I (626) example_connect: Start example_connect.
I (636) wifi:wifi driver task: 3ffbf30c, prio:23, stack:6656, core=0
I (636) system_api: Base MAC address is not set
I (636) system_api: read default base MAC address from EFUSE
I (656) wifi:wifi firmware version: 57982fe
I (656) wifi:wifi certification version: v7.0
I (656) wifi:config NVS flash: enabled
I (656) wifi:config nano formating: disabled
I (666) wifi:Init data frame dynamic rx buffer num: 32
I (666) wifi:Init management frame dynamic rx buffer num: 32
I (676) wifi:Init management short buffer num: 32
I (676) wifi:Init dynamic tx buffer num: 32
I (686) wifi:Init static rx buffer size: 1600
I (686) wifi:Init static rx buffer num: 10
I (696) wifi:Init dynamic rx buffer num: 32
I (696) wifi_init: rx ba win: 6
I (696) wifi_init: tcpip mbox: 32
I (706) wifi_init: udp mbox: 6
I (706) wifi_init: tcp mbox: 6
I (706) wifi_init: tcp tx win: 5744
I (716) wifi_init: tcp rx win: 5744
I (716) wifi_init: tcp mss: 1440
I (726) wifi_init: WiFi IRAM OP enabled
I (726) wifi_init: WiFi RX IRAM OP enabled
I (736) phy_init: phy_version 4670,719f9f6,Feb 18 2021,17:07:07
I (836) wifi:mode : sta (94:b5:55:f6:f5:90)
I (836) wifi:enable tsf
I (836) example_connect: Connecting to I Wanna Die...
I (846) example_connect: Waiting for IP(s)
I (3256) wifi:new:<6,0>, old:<1,0>, ap:<255,255>, sta:<6,0>, prof:1
I (4316) wifi:state: init -> auth (b0)
I (4336) wifi:state: auth -> assoc (0)
I (4346) wifi:state: assoc -> run (10)
I (4766) wifi:connected with I Wanna Die, aid = 1, channel 6, BW20, bssid = ee:bf:8f:d8:06:86
I (4766) wifi:security: WPA2-PSK, phy: bgn, rssi: -62
I (4776) wifi:pm start, type: 1

I (4786) wifi:<ba-add>idx:0 (ifx:0, ee:bf:8f:d8:06:86), tid:0, ssn:2, winSize:64
I (4826) wifi:AP's beacon interval = 102400 us, DTIM period = 1
I (5776) esp_netif_handlers: example_netif_sta ip: 172.20.10.8, mask: 255.255.255.240, gw: 172.20.10.1
I (5776) example_connect: Got IPv4 event: Interface "example_netif_sta" address: 172.20.10.8
I (6616) example_connect: Got IPv6 event: Interface "example_netif_sta" address: fe80:0000:0000:0000:96b5:55ff:fef6:f590, type: ESP_IP6_ADDR_IS_LINK_LOCAL
I (6616) example_common: Connected to example_netif_sta
I (6626) example_common: - IPv4 address: 172.20.10.8,
I (6626) example_common: - IPv6 address: fe80:0000:0000:0000:96b5:55ff:fef6:f590, type: ESP_IP6_ADDR_IS_LINK_LOCAL
I (6646) MQTT_EXAMPLE: Other event id:7
I (7606) MQTT_EXAMPLE: MQTT_EVENT_CONNECTED
I (7606) MQTT_EXAMPLE: sent publish successful, msg_id=36238
I (7606) MQTT_EXAMPLE: sent subscribe successful, msg_id=20293
I (7606) MQTT_EXAMPLE: sent subscribe successful, msg_id=28799
I (7616) MQTT_EXAMPLE: sent unsubscribe successful, msg_id=59043
I (8236) MQTT_EXAMPLE: MQTT_EVENT_PUBLISHED, msg_id=36238
I (8826) MQTT_EXAMPLE: MQTT_EVENT_SUBSCRIBED, msg_id=20293
I (8836) MQTT_EXAMPLE: sent publish successful, msg_id=0
I (8836) MQTT_EXAMPLE: MQTT_EVENT_SUBSCRIBED, msg_id=28799
I (8836) MQTT_EXAMPLE: sent publish successful, msg_id=0
I (8846) MQTT_EXAMPLE: MQTT_EVENT_UNSUBSCRIBED, msg_id=59043
I (9236) MQTT_EXAMPLE: MQTT_EVENT_DATA
TOPIC=/topic/qos0
DATA=data
I (9756) MQTT_EXAMPLE: MQTT_EVENT_DATA
TOPIC=/topic/qos0
DATA=data
I (10366) MQTT_EXAMPLE: MQTT_EVENT_DATA
TOPIC=/topic/qos0
DATA=data
```

เมื่อแก้ไขโค้ดใน case MQTT_EVENT_CONNECTED จะได้ output ดังนี้
```css
I (28326) esp_netif_handlers: example_netif_sta ip: 172.20.10.8, mask: 255.255.255.240, gw: 172.20.10.1
I (28326) example_connect: Got IPv4 event: Interface "example_netif_sta" address: 172.20.10.8
I (28616) example_connect: Got IPv6 event: Interface "example_netif_sta" address: fe80:0000:0000:0000:96b5:55ff:fef6:f590, type: ESP_IP6_ADDR_IS_LINK_LOCAL
I (28616) example_common: Connected to example_netif_sta
I (28626) example_common: - IPv4 address: 172.20.10.8,
I (28626) example_common: - IPv6 address: fe80:0000:0000:0000:96b5:55ff:fef6:f590, type: ESP_IP6_ADDR_IS_LINK_LOCAL
I (28646) MQTT_EXAMPLE: Other event id:7
I (30026) MQTT_EXAMPLE: MQTT_EVENT_CONNECTED
I (30026) MQTT_EXAMPLE: sent publish successful, msg_id=33877
I (30026) MQTT_EXAMPLE: sent subscribe successful, msg_id=20508
I (30036) MQTT_EXAMPLE: sent subscribe successful, msg_id=51637
I (30036) MQTT_EXAMPLE: sent unsubscribe successful, msg_id=26625
I (30046) MQTT_EXAMPLE: sent unsubscribe successful, msg_id=773
I (30056) MQTT_EXAMPLE: sent subscribe successful, msg_id=11123
I (30636) MQTT_EXAMPLE: MQTT_EVENT_PUBLISHED, msg_id=33877
I (31146) MQTT_EXAMPLE: MQTT_EVENT_SUBSCRIBED, msg_id=20508
I (31146) MQTT_EXAMPLE: sent publish successful, msg_id=0
I (31156) MQTT_EXAMPLE: MQTT_EVENT_SUBSCRIBED, msg_id=51637
I (31156) MQTT_EXAMPLE: sent publish successful, msg_id=0
I (31166) MQTT_EXAMPLE: MQTT_EVENT_UNSUBSCRIBED, msg_id=26625
I (31166) MQTT_EXAMPLE: MQTT_EVENT_UNSUBSCRIBED, msg_id=773
I (31176) MQTT_EXAMPLE: MQTT_EVENT_SUBSCRIBED, msg_id=11123
I (31186) MQTT_EXAMPLE: sent publish successful, msg_id=0
I (31586) MQTT_EXAMPLE: MQTT_EVENT_DATA
TOPIC=/topic/qos0
DATA=data
I (32276) MQTT_EXAMPLE: MQTT_EVENT_DATA
TOPIC=/topic/qos0
DATA=data
I (32276) MQTT_EXAMPLE: MQTT_EVENT_DATA
TOPIC=/topic/qos0
DATA=data
```
ทดสอบส่งข้อมูล
![image](https://github.com/TikPoramat2545/MQTT_Lab_II/assets/134470274/c5eb9726-3e53-4e74-b034-c20e8d5bcfcc)


Output
```css
TOPIC=/stu_278/lamp1
DATA=1
I (115736) MQTT_EXAMPLE: MQTT_EVENT_DATA
TOPIC=/topic/qos0
DATA=data
I (129456) MQTT_EXAMPLE: MQTT_EVENT_DATA
TOPIC=/stu_278/lamp1
DATA=Hello
```
แก้ไขโค้ดในการเปิดปิดไฟ
กำหนด define
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

        if (strncmp(event->topic, "/stu_278/lamp1", event->topic_len) == 0) // if topic is "/stu_999/lamp1" then result = 0
        {
            ESP_LOGI(TAG, "event->topic = /stu_278/lamp1");
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
เมื่อทดลองจะได้ผลดังนี้
![image](https://github.com/TikPoramat2545/MQTT_Lab_II/assets/134470274/30dd5acf-a319-4d3c-867f-6e2d86d56048)


Output
```css
I (11019) MQTT_EXAMPLE: MQTT_EVENT_DATA
TOPIC=/stu_278/lamp1
DATA=1
I (11019) MQTT_EXAMPLE: event->topic = /stu_207/lamp1
I (11019) MQTT_EXAMPLE: Turn on LED
I (17159) MQTT_EXAMPLE: MQTT_EVENT_DATA
TOPIC=/stu_278/lamp1
DATA=0
I (17159) MQTT_EXAMPLE: event->topic = /stu_278/lamp1
I (17159) MQTT_EXAMPLE: Turn off LED
```
แก้ไขโค้ดเพื่อให้ควบคุม LED 2 ดวง
define เพื่อเก็บค่า pin

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
msg_id = esp_mqtt_client_subscribe(client, "/stu_278/lamp2", 0);
```

เพิ่ม code ใน case MQTT_EVENT_DATA ใน switch case เพื่อควบคุม LED 2 ดวง

```css
 if (strncmp(event->topic, "/stu_278/lamp2", event->topic_len) == 0) // if topic is "/stu_999/lamp1" then result = 0
        {
            ESP_LOGI(TAG, "event->topic = /stu_278/lamp2");
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
ทดลองรันโปรแกรม

![image](https://github.com/TikPoramat2545/MQTT_Lab_II/assets/134470274/fc7a002b-5345-4f0d-96a5-61a303600488)



ทดลองเปิดปิดไฟ ใช้ /lamp1-2 ในการกำหนดดวงไฟ
```css
I (22539) MQTT_EXAMPLE: MQTT_EVENT_DATA
TOPIC=/stu_278/lamp1
DATA=1
I (22539) MQTT_EXAMPLE: event->topic = /stu_278/lamp1
I (22539) MQTT_EXAMPLE: Turn on LED1
I (24899) MQTT_EXAMPLE: MQTT_EVENT_DATA
TOPIC=/topic/qos0
DATA=data
I (32879) MQTT_EXAMPLE: MQTT_EVENT_DATA
TOPIC=/stu_207/lamp2
```
Repo : https://github.com/64030278-poramat/Mqtt-Lab-2
