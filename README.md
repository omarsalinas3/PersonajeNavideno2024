# Unidad-I-Aplicaciones-IOT

# INSTRUMENTO DE EVALUACIÓN ORDINARIO
ASIGNATURA: Aplicaciones de IoT
UNIDAD: I. Adquisición y Procesamiento de Datos


## Creador
Omar Salinas Salinas-
Cristian Efraín Oropeza Yepiz
## NetAcad Python Fundamentals 2
https://drive.google.com/drive/folders/1O_VhORLl-EkqEuYkoz_RiCApS8D4UADs?usp=sharing

## Ejercicios 1 y 2:


https://drive.google.com/drive/folders/1223LKWGqDJ9OyhAWGqKYZsncF8CWj-gw?usp=sharing



## Diagramas
# Importar módulos necesarios
import network
from umqtt.simple import MQTTClient
from machine import Pin
from time import sleep
from hcsr04 import HCSR04

# Propiedades para conectar a un cliente MQTT
MQTT_BROKER = "192.168.137.237"
MQTT_USER = ""
MQTT_PASSWORD = ""
MQTT_CLIENT_ID = ""
MQTT_TOPIC = "ceoy/ejercicio"
MQTT_PORT = 1883

# Crear el objeto que controla el sensor HCSR04
sensor = HCSR04(trigger_pin=15, echo_pin=4, echo_timeout_us=24000)

# Función para conectar a Wi-Fi
def conectar_wifi():
    print("Conectando a Wi-Fi...", end="")
    sta_if = network.WLAN(network.STA_IF)
    sta_if.active(True)
    sta_if.connect('OmarNamekusei', 'linux123')
    while not sta_if.isconnected():
        print(".", end="")
        sleep(0.3)
    print("Wi-Fi conectada!")

# Función para conectar al broker MQTT
def conectar_broker():
    client = MQTTClient(MQTT_CLIENT_ID, MQTT_BROKER, port=MQTT_PORT, 
                        user=MQTT_USER, password=MQTT_PASSWORD, keepalive=0)
    client.connect()
    print(f"Conectado a {MQTT_BROKER}, en el tópico {MQTT_TOPIC}")
    return client

# Conectar a Wi-Fi
conectar_wifi()

# Conectar al broker MQTT
client = conectar_broker()

distancia_anterior = 0

# Ciclo infinito
while True:
    client.check_msg()  # Comprobar si hay mensajes entrantes

    # Medir la distancia usando el sensor ultrasónico
    distancia = int(sensor.distance_cm())
    
    # Si la distancia ha cambiado, publicarla en el broker MQTT
    if distancia != distancia_anterior:
        print(f"La distancia es {distancia} cms.")
        client.publish(MQTT_TOPIC, str(distancia))  # Publicar distancia
        distancia_anterior = distancia  # Actualizar la distancia anterior
    
    sleep(2)  # Esperar 2 segundos antes de medir nuevamente




# Ejercicio 3 


![Imagen de WhatsApp 2025-02-06 a las 22 38 52_1e428208](https://github.com/user-attachments/assets/aa7a6edb-75e0-4100-8226-38d5c5d5b2d9)


![Imagen de WhatsApp 2025-02-06 a las 22 38 52_6c60320c](https://github.com/user-attachments/assets/91ec8b3a-b898-40e3-9da6-ce81c6ca411d)

![Imagen de WhatsApp 2025-02-06 a las 22 38 53_9d5c4524](https://github.com/user-attachments/assets/f7b9e7b2-5230-4f88-bcc4-da29bf982cf5)

# Ejercicio 4 



