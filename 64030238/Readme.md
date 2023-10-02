## 64030238 Jasda Singhapooti

#ลิ้ง Github https://github.com/JASDA0000/MQTT_II
#
1.สร้าง Project
2.เลือก Template
3.ฺBuild Project

## 1.2.4
Build Project
![image](https://github.com/JASDA0000/MQTT_Lab_II/assets/103983336/ca804668-78b5-4cc1-8913-32a2a32ecc97)
## 1.2.5
ไฟล์ต่างๆที่โปรแกรมเรียกใข้
![image](https://github.com/JASDA0000/MQTT_Lab_II/assets/103983336/424b48ec-f314-4dc1-8ddb-8aad686e76e7)

แก้ไข SSID และ Password ให้ตรงกับ WIFI
![image](https://github.com/JASDA0000/MQTT_Lab_II/assets/103983336/1d31ba96-1275-41aa-85e2-369959c0d7e6)

Build and Run 
```
I (4990) example_connect: Got IPv4 event: Interface "example_netif_sta" address: 192.168.0.102
I (5640) example_connect: Got IPv6 event: Interface "example_netif_sta" address: fe80:0000:0000:0000:26d7:ebff:fe0e:c8d0, type: ESP_IP6_ADDR_IS_LINK_LOCAL
I (5640) example_common: Connected to example_netif_sta
I (5650) wifi:<ba-add>idx:0 (ifx:0, 60:a4:b7:62:e2:75), tid:0, ssn:2, winSize:64
I (5650) example_common: - IPv4 address: 192.168.0.102,
I (5660) example_common: - IPv6 address: fe80:0000:0000:0000:26d7:ebff:fe0e:c8d0, type: ESP_IP6_ADDR_IS_LINK_LOCAL
I (5670) MQTT_EXAMPLE: Other event id:7
I (6930) MQTT_EXAMPLE: MQTT_EVENT_CONNECTED
I (6930) MQTT_EXAMPLE: sent publish successful, msg_id=16315
I (6930) MQTT_EXAMPLE: sent subscribe successful, msg_id=49034
I (6940) MQTT_EXAMPLE: sent subscribe successful, msg_id=16421
I (6940) MQTT_EXAMPLE: sent unsubscribe successful, msg_id=35276
I (7340) MQTT_EXAMPLE: MQTT_EVENT_PUBLISHED, msg_id=16315
I (13790) MQTT_EXAMPLE: MQTT_EVENT_SUBSCRIBED, msg_id=49034
I (13790) MQTT_EXAMPLE: sent publish successful, msg_id=0
I (14410) MQTT_EXAMPLE: MQTT_EVENT_SUBSCRIBED, msg_id=16421
I (14410) MQTT_EXAMPLE: sent publish successful, msg_id=0
I (14420) MQTT_EXAMPLE: MQTT_EVENT_UNSUBSCRIBED, msg_id=35276
I (17170) MQTT_EXAMPLE: MQTT_EVENT_DATA
TOPIC=/topic/qos0
DATA=data
I (17580) MQTT_EXAMPLE: MQTT_EVENT_DATA
TOPIC=/topic/qos0
DATA=data
```
ส่งข้อมูลจาก MQTT Exploer
![image](https://github.com/JASDA0000/MQTT_Lab_II/assets/103983336/80879efb-e4a0-4e8e-9933-275b7766a168)

MQTT Exloer เป็น 0
![image](https://github.com/JASDA0000/MQTT_Lab_II/assets/103983336/49563416-8d33-4ddb-a799-59fde86d5df7)
ไฟดับ
![image](https://github.com/JASDA0000/MQTT_Lab_II/assets/103983336/c5367b82-810b-41be-a179-0abb9111f014)
MQTT Exloer เป็น 1
![image](https://github.com/JASDA0000/MQTT_Lab_II/assets/103983336/e7d1e253-49e2-41b5-a6da-6f79198573e9)
ไฟติด
![image](https://github.com/JASDA0000/MQTT_Lab_II/assets/103983336/d0aad315-4466-4771-931d-ca040644bcd3)

```c++
/* MQTT (over TCP) Example

   This example code is in the Public Domain (or CC0 licensed, at your option.)

   Unless required by applicable law or agreed to in writing, this
   software is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR
   CONDITIONS OF ANY KIND, either express or implied.
*/

#include <stdio.h>
#include <stdint.h>
#include <stddef.h>
#include <string.h>
#include "esp_wifi.h"
#include "esp_system.h"
#include "nvs_flash.h"
#include "esp_event.h"
#include "esp_netif.h"
#include "protocol_examples_common.h"
#include "driver/gpio.h"
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "freertos/semphr.h"
#include "freertos/queue.h"

#include "lwip/sockets.h"
#include "lwip/dns.h"
#include "lwip/netdb.h"

#include "esp_log.h"
#include "mqtt_client.h"

#define LED_1 22
#define LED_2 23
static const char *TAG = "MQTT_EXAMPLE";


static void log_error_if_nonzero(const char *message, int error_code)
{
    if (error_code != 0) {
        ESP_LOGE(TAG, "Last error %s: 0x%x", message, error_code);
    }
}

/*
 * @brief Event handler registered to receive MQTT events
 *
 *  This function is called by the MQTT client event loop.
 *
 * @param handler_args user data registered to the event.
 * @param base Event base for the handler(always MQTT Base in this example).
 * @param event_id The id for the received event.
 * @param event_data The data for the event, esp_mqtt_event_handle_t.
 */
static void mqtt_event_handler(void *handler_args, esp_event_base_t base, int32_t event_id, void *event_data)
{
    ESP_LOGD(TAG, "Event dispatched from event loop base=%s, event_id=%d", base, event_id);
    esp_mqtt_event_handle_t event = event_data;
    esp_mqtt_client_handle_t client = event->client;
    int msg_id;
    switch ((esp_mqtt_event_id_t)event_id) {
    case MQTT_EVENT_CONNECTED:
    	msg_id = esp_mqtt_client_unsubscribe(client, "/topic/qos1");
    	ESP_LOGI(TAG, "sent unsubscribe successful, msg_id=%d", msg_id);

    	        // เพิ่มบรรทัดต่อไปนี้ โดยตั้งขื่อเป็น stu-<เลข 3 ตัวท้ายของรหัสนักศึกษา>
    	msg_id = esp_mqtt_client_subscribe(client, "/stu_238/lamp1", 0);
    	msg_id = esp_mqtt_client_subscribe(client, "/stu_238/lamp2", 0);
    	ESP_LOGI(TAG, "sent subscribe successful, msg_id=%d", msg_id);
    	break;
    case MQTT_EVENT_DISCONNECTED:
        ESP_LOGI(TAG, "MQTT_EVENT_DISCONNECTED");
        break;

    case MQTT_EVENT_SUBSCRIBED:
        ESP_LOGI(TAG, "MQTT_EVENT_SUBSCRIBED, msg_id=%d", event->msg_id);
        msg_id = esp_mqtt_client_publish(client, "/topic/qos0", "data", 0, 0, 0);
        ESP_LOGI(TAG, "sent publish successful, msg_id=%d", msg_id);
        break;
    case MQTT_EVENT_UNSUBSCRIBED:
        ESP_LOGI(TAG, "MQTT_EVENT_UNSUBSCRIBED, msg_id=%d", event->msg_id);
        break;
    case MQTT_EVENT_PUBLISHED:
        ESP_LOGI(TAG, "MQTT_EVENT_PUBLISHED, msg_id=%d", event->msg_id);
        break;
    case MQTT_EVENT_DATA:
    	ESP_LOGI(TAG, "MQTT_EVENT_DATA");
    	        printf("TOPIC=%.*s\r\n", event->topic_len, event->topic);
    	        printf("DATA=%.*s\r\n", event->data_len, event->data);

    	        if(strncmp(event->topic, "/stu_238/lamp1", event->topic_len) == 0)  // if topic is "/stu_999/lamp1" then result = 0
    	        {
    	            ESP_LOGI(TAG, "event->topic = /stu_238/lamp1");
    	            if(strncmp(event->data, "1", event->data_len) == 0) // if data is "1" then result = 0
    	            {
    	                ESP_LOGI(TAG, "Turn on LED");
    	                gpio_set_level(LED_1, 1);
    	            }
    	            if(strncmp(event->data, "0", event->data_len) == 0) // if data is "0" then result = 0
    	            {
    	                ESP_LOGI(TAG, "Turn off LED");
    	                gpio_set_level(LED_1, 0);
    	            }
    	        }
    	        if(strncmp(event->topic, "/stu_238/lamp2", event->topic_len) == 0)  // if topic is "/stu_999/lamp1" then result = 0
    	            	        {
    	            	            ESP_LOGI(TAG, "event->topic = /stu_238/lamp2");
    	            	            if(strncmp(event->data, "1", event->data_len) == 0) // if data is "1" then result = 0
    	            	            {
    	            	                ESP_LOGI(TAG, "Turn on LED");
    	            	                gpio_set_level(LED_2, 1);
    	            	            }
    	            	            if(strncmp(event->data, "0", event->data_len) == 0) // if data is "0" then result = 0
    	            	            {
    	            	                ESP_LOGI(TAG, "Turn off LED");
    	            	                gpio_set_level(LED_2, 0);
    	            	            }
    	            	        }
    	        break;

    case MQTT_EVENT_ERROR:
        ESP_LOGI(TAG, "MQTT_EVENT_ERROR");
        if (event->error_handle->error_type == MQTT_ERROR_TYPE_TCP_TRANSPORT) {
            log_error_if_nonzero("reported from esp-tls", event->error_handle->esp_tls_last_esp_err);
            log_error_if_nonzero("reported from tls stack", event->error_handle->esp_tls_stack_err);
            log_error_if_nonzero("captured as transport's socket errno",  event->error_handle->esp_transport_sock_errno);
            ESP_LOGI(TAG, "Last errno string (%s)", strerror(event->error_handle->esp_transport_sock_errno));

        }
        break;
    default:
        ESP_LOGI(TAG, "Other event id:%d", event->event_id);
        break;
    }
}

static void mqtt_app_start(void)
{
    esp_mqtt_client_config_t mqtt_cfg = {
        .broker.address.uri = CONFIG_BROKER_URL,
    };
#if CONFIG_BROKER_URL_FROM_STDIN
    char line[128];

    if (strcmp(mqtt_cfg.broker.address.uri, "FROM_STDIN") == 0) {
        int count = 0;
        printf("Please enter url of mqtt broker\n");
        while (count < 128) {
            int c = fgetc(stdin);
            if (c == '\n') {
                line[count] = '\0';
                break;
            } else if (c > 0 && c < 127) {
                line[count] = c;
                ++count;
            }
            vTaskDelay(10 / portTICK_PERIOD_MS);
        }
        mqtt_cfg.broker.address.uri = line;
        printf("Broker url: %s\n", line);
    } else {
        ESP_LOGE(TAG, "Configuration mismatch: wrong broker url");
        abort();
    }
#endif /* CONFIG_BROKER_URL_FROM_STDIN */

    esp_mqtt_client_handle_t client = esp_mqtt_client_init(&mqtt_cfg);
    /* The last argument may be used to pass data to the event handler, in this example mqtt_event_handler */
    esp_mqtt_client_register_event(client, ESP_EVENT_ANY_ID, mqtt_event_handler, NULL);
    esp_mqtt_client_start(client);
}

void app_main(void)
{
//    ESP_LOGI(TAG, "[APP] Startup..");
//    ESP_LOGI(TAG, "[APP] Free memory: %d bytes", esp_get_free_heap_size());
//    ESP_LOGI(TAG, "[APP] IDF version: %s", esp_get_idf_version());
//
//    esp_log_level_set("*", ESP_LOG_INFO);
//    esp_log_level_set("mqtt_client", ESP_LOG_VERBOSE);
//    esp_log_level_set("MQTT_EXAMPLE", ESP_LOG_VERBOSE);
//    esp_log_level_set("TRANSPORT_BASE", ESP_LOG_VERBOSE);
//    esp_log_level_set("esp-tls", ESP_LOG_VERBOSE);
//    esp_log_level_set("TRANSPORT", ESP_LOG_VERBOSE);
//    esp_log_level_set("outbox", ESP_LOG_VERBOSE);
//
//    ESP_ERROR_CHECK(nvs_flash_init());
//    ESP_ERROR_CHECK(esp_netif_init());
//    ESP_ERROR_CHECK(esp_event_loop_create_default());
//
//    /* This helper function configures Wi-Fi or Ethernet, as selected in menuconfig.
//     * Read "Establishing Wi-Fi or Ethernet Connection" section in
//     * examples/protocols/README.md for more information about this function.
//     */
//    ESP_ERROR_CHECK(example_connect());
//
//    mqtt_app_start();
		gpio_reset_pin(LED_1);
		gpio_reset_pin(LED_2);
		gpio_set_direction(LED_1, GPIO_MODE_OUTPUT);
		gpio_set_direction(LED_2, GPIO_MODE_OUTPUT);
		ESP_ERROR_CHECK(nvs_flash_init());
	    ESP_ERROR_CHECK(esp_netif_init());
	    ESP_ERROR_CHECK(esp_event_loop_create_default());

	    ESP_ERROR_CHECK(example_connect());

	    mqtt_app_start();  //<< กด ctrl+click เพื่อตามไปที่ฟังก์ชัน
}
```

![image](https://github.com/JASDA0000/MQTT_Lab_II/assets/103983336/af84b0d6-17e7-4b37-acee-64645bd3ee94)
MQTT Exploer ถ้าต้องการตั้ง LED1 เปลี่ยน Topic เป็น stu_238/lamp1 ส่วน LED2 เปลี่ยนเป็น stu_238/lamp2
![image](https://github.com/JASDA0000/MQTT_Lab_II/assets/103983336/74d690a3-8428-42bb-b2a4-bf82484088eb)

