# นางสาวกุลธิดา  เกษร


1.รับอินพุตจาก button สองตัว แบบ interrupt เพื่อควบคุม LED สองดวง แล้วส่งไปยัง MQTT Broker ทดสอบโดยการแสดงผลบน MQTT Explorer (ดัดแปลงเพิ่มเติมจาก code ในใบงาน)

```
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

#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "freertos/semphr.h"
#include "freertos/queue.h"

#include "lwip/sockets.h"
#include "lwip/dns.h"
#include "lwip/netdb.h"

#include "esp_log.h"
#include "mqtt_client.h"

#include "driver/gpio.h"
#include "esp_rom_gpio.h"
static const char *TAG = "MQTT_EXAMPLE";

#define INPUT_PIN 21
#define INPUT_PIN_2 13
#define LED_PIN 23
#define LED_PIN_2 26
int state = 0;
int state_2 = 0;
QueueHandle_t interputQueue;
esp_mqtt_client_handle_t mqtt_client;

// interrupt service handler
static void IRAM_ATTR gpio_interrupt_handler(void *args) {
	int pinNumber = (int) args;
	xQueueSendFromISR(interputQueue, &pinNumber, NULL);
}

// LED control task, received button prerssed from ISR
void LED_Control_Task(void *params) {
	int pinNumber = 0;
	char data[3];
	while (true) {
		if (xQueueReceive(interputQueue, &pinNumber, portMAX_DELAY)) {
			gpio_set_level(LED_PIN, gpio_get_level(LED_PIN) == 0);
			printf("GPIO %d was pressed. The state is %d\n", pinNumber,
					gpio_get_level(LED_PIN));
			sprintf(data, "%d", gpio_get_level(LED_PIN)); // <-- แปลงสถานะของ LED  (int) เป็น string
			esp_mqtt_client_publish(mqtt_client, "/stu_234/lamp1", data, 0, 0,
					0); // <-- publish ไปยัง broker
		}
	}
}

// LED control task, received button prerssed from ISR
void LED_Control_Task_2(void *params) {
	int pinNumber = 0;
	char data[3];
	while (true) {
		if (xQueueReceive(interputQueue, &pinNumber, portMAX_DELAY)) {
			gpio_set_level(LED_PIN_2, gpio_get_level(LED_PIN_2) == 0);
			printf("GPIO %d was pressed. The state is %d\n", pinNumber,
					gpio_get_level(LED_PIN_2));
			sprintf(data, "%d", gpio_get_level(LED_PIN_2)); // <-- แปลงสถานะของ LED  (int) เป็น string
			esp_mqtt_client_publish(mqtt_client, "/stu_234/lamp2", data, 0, 0,
					0); // <-- publish ไปยัง broker
		}
	}
}

static void log_error_if_nonzero(const char *message, int error_code) {
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
static void mqtt_event_handler(void *handler_args, esp_event_base_t base,
		int32_t event_id, void *event_data) {
	ESP_LOGD(TAG, "Event dispatched from event loop base=%s, event_id=%d", base,
			event_id);
	esp_mqtt_event_handle_t event = event_data;
	esp_mqtt_client_handle_t client = event->client;
	int msg_id;
	switch ((esp_mqtt_event_id_t) event_id) {
	case MQTT_EVENT_CONNECTED:
		ESP_LOGI(TAG, "MQTT_EVENT_CONNECTED");
		msg_id = esp_mqtt_client_publish(client, "/topic/qos1", "data_3", 0, 1,
				0);
		ESP_LOGI(TAG, "sent publish successful, msg_id=%d", msg_id);

		msg_id = esp_mqtt_client_subscribe(client, "/topic/qos0", 0);
		ESP_LOGI(TAG, "sent subscribe successful, msg_id=%d", msg_id);

		msg_id = esp_mqtt_client_subscribe(client, "/topic/qos1", 1);
		ESP_LOGI(TAG, "sent subscribe successful, msg_id=%d", msg_id);

		msg_id = esp_mqtt_client_unsubscribe(client, "/topic/qos1");
		ESP_LOGI(TAG, "sent unsubscribe successful, msg_id=%d", msg_id);

		msg_id = esp_mqtt_client_subscribe(client, "/stu_234/lamp1", 0);
		ESP_LOGI(TAG, "sent subscribe successful, msg_id=%d", msg_id);

		msg_id = esp_mqtt_client_subscribe(client, "/stu_234/lamp2", 0);
		ESP_LOGI(TAG, "sent subscribe successful, msg_id=%d", msg_id);

		break;
	case MQTT_EVENT_DISCONNECTED:
		ESP_LOGI(TAG, "MQTT_EVENT_DISCONNECTED");
		break;

	case MQTT_EVENT_SUBSCRIBED:
		ESP_LOGI(TAG, "MQTT_EVENT_SUBSCRIBED, msg_id=%d", event->msg_id);
		msg_id = esp_mqtt_client_publish(client, "/topic/qos0", "data", 0, 0,
				0);
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

		if (strncmp(event->topic, "/stu_234/lamp1", event->topic_len) == 0) // if topic is "/stu_999/lamp1" then result = 0
				{
			ESP_LOGI(TAG, "event->topic = /stu_234/lamp1");
			if (strncmp(event->data, "1", event->data_len) == 0) // if data is "1" then result = 0
					{
				ESP_LOGI(TAG, "Turn on LED");
				gpio_set_level(23, 1);
			}
			if (strncmp(event->data, "0", event->data_len) == 0) // if data is "0" then result = 0
					{
				ESP_LOGI(TAG, "Turn off LED");
				gpio_set_level(23, 0);
			}
		}
		if (strncmp(event->topic, "/stu_234/lamp2", event->topic_len) == 0) // if topic is "/stu_999/lamp1" then result = 0
				{
			ESP_LOGI(TAG, "event->topic = /stu_234/lamp2");
			if (strncmp(event->data, "1", event->data_len) == 0) // if data is "1" then result = 0
					{
				ESP_LOGI(TAG, "Turn on LED");
				gpio_set_level(26, 1);
			}
			if (strncmp(event->data, "0", event->data_len) == 0) // if data is "0" then result = 0
					{
				ESP_LOGI(TAG, "Turn off LED");
				gpio_set_level(26, 0);
			}
		}
		break;
	case MQTT_EVENT_ERROR:
		ESP_LOGI(TAG, "MQTT_EVENT_ERROR");
		if (event->error_handle->error_type == MQTT_ERROR_TYPE_TCP_TRANSPORT) {
			log_error_if_nonzero("reported from esp-tls",
					event->error_handle->esp_tls_last_esp_err);
			log_error_if_nonzero("reported from tls stack",
					event->error_handle->esp_tls_stack_err);
			log_error_if_nonzero("captured as transport's socket errno",
					event->error_handle->esp_transport_sock_errno);
			ESP_LOGI(TAG, "Last errno string (%s)",
					strerror(event->error_handle->esp_transport_sock_errno));

		}
		break;
	default:
		ESP_LOGI(TAG, "Other event id:%d", event->event_id);
		break;
	}
}

static void mqtt_app_start(void) {
	esp_mqtt_client_config_t mqtt_cfg = { .broker.address.uri =
	CONFIG_BROKER_URL, };
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
	mqtt_client = client;
	/* The last argument may be used to pass data to the event handler, in this example mqtt_event_handler */
	esp_mqtt_client_register_event(client, ESP_EVENT_ANY_ID, mqtt_event_handler,
	NULL);
	esp_mqtt_client_start(client);
}

void app_main(void) {
	gpio_reset_pin(23);
	gpio_reset_pin(26);

	gpio_set_direction(23, GPIO_MODE_OUTPUT);
	gpio_set_direction(26, GPIO_MODE_OUTPUT);

	// LED config as I/O port
	esp_rom_gpio_pad_select_gpio(LED_PIN);
	gpio_set_direction(LED_PIN, GPIO_MODE_INPUT_OUTPUT);

	esp_rom_gpio_pad_select_gpio(LED_PIN_2);
	gpio_set_direction(LED_PIN_2, GPIO_MODE_INPUT_OUTPUT);

	// config Input No 21 for external interrupt input
	esp_rom_gpio_pad_select_gpio(INPUT_PIN);
	gpio_set_direction(INPUT_PIN, GPIO_MODE_INPUT);
	gpio_pulldown_en(INPUT_PIN);
	gpio_pullup_dis(INPUT_PIN);
	gpio_set_intr_type(INPUT_PIN, GPIO_INTR_POSEDGE);

	// config Input No 13 for external interrupt input
	esp_rom_gpio_pad_select_gpio(INPUT_PIN_2);
	gpio_set_direction(INPUT_PIN_2, GPIO_MODE_INPUT);
	gpio_pulldown_en(INPUT_PIN_2);
	gpio_pullup_dis(INPUT_PIN_2);
	gpio_set_intr_type(INPUT_PIN_2, GPIO_INTR_POSEDGE);

	// FreeRTOS tas and queue
	interputQueue = xQueueCreate(10, sizeof(int));
	xTaskCreate(LED_Control_Task, "LED_Control_Task", 2048, NULL, 1, NULL);
	xTaskCreate(LED_Control_Task_2, "LED_Control_Task_2", 2048, NULL, 1, NULL);

	// install isr service and isr handler
	gpio_install_isr_service(0);
	gpio_isr_handler_add(INPUT_PIN, gpio_interrupt_handler, (void*) INPUT_PIN);
	gpio_isr_handler_add(INPUT_PIN_2, gpio_interrupt_handler,
			(void*) INPUT_PIN_2);
	ESP_LOGI(TAG, "[APP] Startup..");
	ESP_LOGI(TAG, "[APP] Free memory: %d bytes", esp_get_free_heap_size());
	ESP_LOGI(TAG, "[APP] IDF version: %s", esp_get_idf_version());

	esp_log_level_set("*", ESP_LOG_INFO);
	esp_log_level_set("mqtt_client", ESP_LOG_VERBOSE);
	esp_log_level_set("MQTT_EXAMPLE", ESP_LOG_VERBOSE);
	esp_log_level_set("TRANSPORT_BASE", ESP_LOG_VERBOSE);
	esp_log_level_set("esp-tls", ESP_LOG_VERBOSE);
	esp_log_level_set("TRANSPORT", ESP_LOG_VERBOSE);
	esp_log_level_set("outbox", ESP_LOG_VERBOSE);

	ESP_ERROR_CHECK(nvs_flash_init());
	ESP_ERROR_CHECK(esp_netif_init());
	ESP_ERROR_CHECK(esp_event_loop_create_default());

	/* This helper function configures Wi-Fi or Ethernet, as selected in menuconfig.
	 * Read "Establishing Wi-Fi or Ethernet Connection" section in
	 * examples/protocols/README.md for more information about this function.
	 */
	ESP_ERROR_CHECK(example_connect());

	mqtt_app_start();
}

```
# terminal
![image](https://github.com/KUNTIDA234/MQTT_Lab_II/assets/115066215/aac5213e-4741-4c13-a5f4-9db9d34aabbe)

# MQTT Explorer
![image](https://github.com/KUNTIDA234/MQTT_Lab_II/assets/115066215/842be319-648e-4b60-b830-1caef2b92404)

# Repo Project

https://github.com/KUNTIDA234/esp32-mqtt-lab3 
