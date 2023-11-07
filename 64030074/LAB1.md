# 64030074 Thani Paknam
## 1.2.2
1. สร้าง Project
2. เลือก Template
3. Build Project
## 1.2.4
Build Project
1. ![image](https://github.com/tnpn2545/MQTT_Lab_II/assets/115066414/86fff9f3-aa98-45a6-9e39-e1427157cf31)
ไฟล์ต่างๆที่โปรแกรม Build ออกมา
2. ![image](https://github.com/tnpn2545/MQTT_Lab_II/assets/115066414/379ddc25-06bb-442a-adc0-0c074c0c06fa)
Build And Run
```
I (715) wifi_init: WiFi RX IRAM OP enabled
I (715) phy_init: phy_version 4670,719f9f6,Feb 18 2021,17:07:07
I (815) wifi:mode : sta (94:b5:55:f6:f1:00)
I (815) wifi:enable tsf
I (825) example_connect: Connecting to Solomon...
I (825) example_connect: Waiting for IP(s)
I (3235) wifi:new:<6,0>, old:<1,0>, ap:<255,255>, sta:<6,0>, prof:1
I (4305) wifi:state: init -> auth (b0)
I (4325) wifi:state: auth -> assoc (0)
I (4325) wifi:state: assoc -> run (10)
I (4415) wifi:connected with Solomon, aid = 2, channel 6, BW20, bssid = 26:cf:2d:34:1d:98
I (4415) wifi:security: WPA2-PSK, phy: bgn, rssi: -62
I (4415) wifi:pm start, type: 1

I (4445) wifi:<ba-add>idx:0 (ifx:0, 26:cf:2d:34:1d:98), tid:0, ssn:2, winSize:64
I (4475) wifi:AP's beacon interval = 102400 us, DTIM period = 1
I (5425) esp_netif_handlers: example_netif_sta ip: 172.20.10.3, mask: 255.255.255.240, gw: 172.20.10.1
I (5425) example_connect: Got IPv4 event: Interface "example_netif_sta" address: 172.20.10.3
I (5605) example_connect: Got IPv6 event: Interface "example_netif_sta" address: fe80:0000:0000:0000:96b5:55ff:fef6:f100, type: ESP_IP6_ADDR_IS_LINK_LOCAL
I (5605) example_common: Connected to example_netif_sta
I (5615) example_common: - IPv4 address: 172.20.10.3,
I (5615) example_common: - IPv6 address: fe80:0000:0000:0000:96b5:55ff:fef6:f100, type: ESP_IP6_ADDR_IS_LINK_LOCAL
I (5635) MQTT_EXAMPLE: Other event id:7
I (6455) MQTT_EXAMPLE: MQTT_EVENT_CONNECTED
I (6465) MQTT_EXAMPLE: sent publish successful, msg_id=60349
I (6465) MQTT_EXAMPLE: sent subscribe successful, msg_id=54623
I (6465) MQTT_EXAMPLE: sent subscribe successful, msg_id=25982
I (6475) MQTT_EXAMPLE: sent unsubscribe successful, msg_id=29002
I (6845) MQTT_EXAMPLE: MQTT_EVENT_PUBLISHED, msg_id=60349
I (7225) MQTT_EXAMPLE: MQTT_EVENT_SUBSCRIBED, msg_id=54623
I (7235) MQTT_EXAMPLE: sent publish successful, msg_id=0
I (7235) MQTT_EXAMPLE: MQTT_EVENT_SUBSCRIBED, msg_id=25982
I (7235) MQTT_EXAMPLE: sent publish successful, msg_id=0
I (7245) MQTT_EXAMPLE: MQTT_EVENT_UNSUBSCRIBED, msg_id=29002
I (7555) MQTT_EXAMPLE: MQTT_EVENT_DATA
TOPIC=/topic/qos0
DATA=data
I (7935) MQTT_EXAMPLE: MQTT_EVENT_DATA
TOPIC=/topic/qos0
DATA=data
```
## งานที่ต้องทำ
ติดตามข้อมูลจาก MQTT
![image](https://github.com/tnpn2545/MQTT_Lab_II/assets/115066414/9e2525f9-683a-407c-b2be-340408679737)
ข้อมูลใน MQTT Exploer
![image](https://github.com/tnpn2545/MQTT_Lab_II/assets/115066414/da2059e9-6468-4a0c-bae4-5b0ce9d2b45b)
เพิ่ม code ให้สามารถ On-Off LED ได้จริง เมื่อสั่งการจาก MQTT Explorer
```c++
 #include "driver/gpio.h"
```
```c++
 #define LED1 23
```
```c++
if(strncmp(event->data, "1", event->data_len) == 0) // if data is "1" then result = 0
{
  ESP_LOGI(TAG, "Turn on LED");
  gpio_set_level(LED1, 1);
}
if(strncmp(event->data, "0", event->data_len) == 0) // if data is "0" then result = 0
{
  ESP_LOGI(TAG, "Turn off LED");
  gpio_set_level(LED1, 0);
}
```
