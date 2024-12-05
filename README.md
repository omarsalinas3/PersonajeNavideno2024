# Unidad-III-Principios-IOT
Practica final, instrumento de evaluación (ROBIDAD)

# INSTRUMENTO DE EVALUACIÓN
# Personaje ROBIDAD
## Nombre del personaje
ROBIDAD

## Creador
Omar Salinas Salinas
## Explicacion del funcionamiento
El personaje (ROBIDAD) cuando detecta movimiento a una distancia de 10 cm prende los leds que tiene por el cuerpo, ademas mueve el cuerpo media vuelta (180°), produce una melodia navideña y muestra un mensaje en el pecho diciendo "Felices Fiestas", todo esto durante 10 segundos despues de detectar moviemiento y luego se detiene y comienza nuevamente la lectura de la distancia.

## Materiales a utlizar
| Material         | Imagen | Cantidad | Precio  |
|------------------|-------------------------------------------------------------------------------------------------------------|----------|---------|
| Placa Arduino Uno | <img src="https://github.com/user-attachments/assets/39048c81-c2a8-47e7-b1f0-efc059c6aeee" width="60"/> | 1 | $80.00 |
| Sensor Ultrasonico | <img src="https://www.330ohms.com/cdn/shop/products/photo_A_OS-03261_SensorUltrasonico_HC-SR04_01_1200x1200.png?v=1598042103" width="100"/> | 1 | $40.00 |
| Servomotor SG90 | <img src="https://github.com/user-attachments/assets/8ae1aa9c-0251-4731-b013-a7b8b73f5ba7" width="100"/> | 2 | $40.00 |
| LEDs RGB | <img src="https://github.com/user-attachments/assets/0ef372bf-1c11-4ae0-9dfb-b34800260e96" width="100"/> | 2 | $2.00 |
| Pantalla OLED | <img src="https://github.com/user-attachments/assets/58cc6ea6-59d0-4d65-a39e-90c917803234" width="100"/> | 1 | $60.00 |
| Buzzer | <img src="https://github.com/user-attachments/assets/cd8d664c-87e8-4462-ad53-9b355c68a740" width="100"/> | 1 | $10.00 |
| Jumpers y cables | <img src="https://github.com/user-attachments/assets/a280353d-bdbf-47d8-9919-6c51b14fe28b" width="100"/> | 1 pack | $30.00 |
| Resistencias | <img src="https://github.com/user-attachments/assets/328da7ee-7586-4beb-8869-fc11694266de" width="100"/> | 2 | $15.00 |
| Carton | <img src="https://github.com/user-attachments/assets/3ebfd4ba-f5f6-4d3a-84a9-6060d9243c37" width="100"/> | 1 | $42.00 |

## Software a utilizar
Thonny

## Dibujo del personaje
![Imagen de WhatsApp 2024-09-27 a las 08 07 59_b0c7d3ef](https://github.com/user-attachments/assets/b509fe30-0bac-4deb-93d2-32d9b9260347)



# PRACTICA FINAL
<h1>Video demostración del funcionamiento</h1>

<h2>Fotos de la elaboración del código</h2>
![Imagen de WhatsApp 2024-12-04 a las 20 00 13_a691e4e0](https://github.com/user-attachments/assets/ea3330d4-6889-4b79-a2c8-f2ccc97a34f7)
![Imagen de WhatsApp 2024-12-04 a las 20 00 15_9aa5f94e](https://github.com/user-attachments/assets/0563b3ac-c340-4764-9040-e6283b7b2d0f)


<h2>Código en MicroPython</h2>
from machine import Pin, PWM, SoftI2C
from time import sleep, ticks_us, ticks_diff
from ssd1306 import SSD1306_I2C

import random

# Configuración de pines
TRIGGER_PIN = 5   # Pin de trigger del sensor ultrasónico
ECHO_PIN = 18     # Pin de echo del sensor ultrasónico
BUZZER_PIN = 16   # Pin del buzzer
MOTOR_PIN1 = 12   # Pin para el motor a pasos
MOTOR_PIN2 = 13
MOTOR_PIN3 = 14
MOTOR_PIN4 = 27

# Pines para el primer LED RGB (LED 1)
RED_PIN1 = 26
GREEN_PIN1 = 25
BLUE_PIN1 = 23

# Pines para el segundo LED RGB (LED 2)
RED_PIN2 = 15
GREEN_PIN2 = 2
BLUE_PIN2 = 4

# Configuración de pines PWM para el primer LED RGB
red_pwm1 = PWM(Pin(RED_PIN1), freq=1000, duty=0)
green_pwm1 = PWM(Pin(GREEN_PIN1), freq=1000, duty=0)
blue_pwm1 = PWM(Pin(BLUE_PIN1), freq=1000, duty=0)

# Configuración de pines PWM para el segundo LED RGB
red_pwm2 = PWM(Pin(RED_PIN2), freq=1000, duty=0)
green_pwm2 = PWM(Pin(GREEN_PIN2), freq=1000, duty=0)
blue_pwm2 = PWM(Pin(BLUE_PIN2), freq=1000, duty=0)

# Configuración del buzzer (PWM)
buzzer = PWM(Pin(BUZZER_PIN), freq=1, duty=0)  # Inicializa el buzzer con una frecuencia baja para evitar que emita sonido constante

# Configuración de pines de trigger y echo
trigger = Pin(TRIGGER_PIN, Pin.OUT)
echo = Pin(ECHO_PIN, Pin.IN)

# Configuración de la pantalla OLED
i2c = SoftI2C(scl=Pin(22), sda=Pin(21))  # Ajusta los pines SCL y SDA según tu configuración
oled = SSD1306_I2C(128, 64, i2c)

# Función para leer la distancia del sensor ultrasónico
def leer_distancia():
    trigger.value(0)
    sleep(0.000002)
    trigger.value(1)
    sleep(0.00001)
    trigger.value(0)
    
    # Inicializar las variables
    pulse_start = 0
    pulse_end = 0
    
    while echo.value() == 0:
        pulse_start = ticks_us()
    
    while echo.value() == 1:
        pulse_end = ticks_us()
    
    pulse_duration = ticks_diff(pulse_end, pulse_start)
    distancia = pulse_duration * 0.0343 / 2  # Convertir a centímetros
    return distancia

# Función para reproducir "Jingle Bells" en el buzzer y cambiar el color de los LEDs
def reproducir_melodia():
    notas = [
        392, 392, 392, 392, 392, 392, 392, 494, 261, 294, 330, 
        349, 349, 349, 349, 349, 349, 392, 392, 392, 392, 349, 
        349, 392, 349, 294, 261
    ]
    duraciones = [
        400, 400, 800, 400, 400, 800, 400, 400, 400, 400, 400, 
        800, 400, 400, 800, 400, 400, 800, 400, 400, 800, 400, 
        400, 400, 400, 400
    ]
    
    # Diccionario para asignar colores a las notas
    colores = {
        261: 'rojo',  # Do (baja frecuencia)
        294: 'amarillo',  # Re
        330: 'verde',  # Mi
        349: 'rojo',  # Fa
        392: 'amarillo',  # Sol (alta frecuencia)
        494: 'verde',  # La
    }

    for nota, duracion in zip(notas, duraciones):
        # Cambiar el color de los LEDs según la nota
        color = colores.get(nota, 'amarillo')  # Si no se encuentra, por defecto 'amarillo'
        cambiar_color_leds(color)
        
        buzzer.freq(nota)  # Establecer la frecuencia de la nota
        buzzer.duty(512)   # Activar el buzzer
        sleep(duracion / 1000)  # Esperar la duración de la nota
        buzzer.duty(0)     # Apagar el buzzer
        sleep(0.05)        # Pausa pequeña entre notas

# Función para mover el motor a pasos
def mover_motor():
    secuencia = [
        [1, 0, 0, 1],
        [1, 0, 0, 0],
        [1, 1, 0, 0],
        [0, 1, 0, 0],
        [0, 1, 1, 0],
        [0, 0, 1, 0],
        [0, 0, 1, 1],
        [0, 0, 0, 1]
    ]
    
    def paso(secuencia):
        for i in range(4):
            Pin([MOTOR_PIN1, MOTOR_PIN2, MOTOR_PIN3, MOTOR_PIN4][i], Pin.OUT).value(secuencia[i])
    
    # Mueve el motor en un sentido (reduciendo los pasos para que no dé una vuelta completa)
    for _ in range(256):  # Menos pasos para que no sea una vuelta completa
        for paso_secuencia in secuencia:
            paso(paso_secuencia)
            sleep(0.005)  # Aumenta la velocidad reduciendo el tiempo de espera entre los pasos
    
    # Mueve el motor en el sentido contrario (también con menos pasos)
    for _ in range(256):  # Menos pasos para que no sea una vuelta completa
        for paso_secuencia in reversed(secuencia):
            paso(paso_secuencia)
            sleep(0.005)  # Aumenta la velocidad reduciendo el tiempo de espera entre los pasos


# Función para cambiar el color de ambos LEDs RGB en función de la canción
def cambiar_color_leds(color):
    if color == 'rojo':
        red_pwm1.duty(512)
        green_pwm1.duty(0)
        blue_pwm1.duty(0)
        red_pwm2.duty(512)
        green_pwm2.duty(0)
        blue_pwm2.duty(0)
    elif color == 'amarillo':
        red_pwm1.duty(512)
        green_pwm1.duty(512)
        blue_pwm1.duty(0)
        red_pwm2.duty(512)
        green_pwm2.duty(512)
        blue_pwm2.duty(0)
    elif color == 'verde':
        red_pwm1.duty(0)
        green_pwm1.duty(512)
        blue_pwm1.duty(0)
        red_pwm2.duty(0)
        green_pwm2.duty(512)
        blue_pwm2.duty(0)

# Función para dibujar un árbol de Navidad más grande
def dibujar_arbol():
    # Limpiar la pantalla
    oled.fill(0)

    # Dibujar el árbol (forma triangular más grande)
    # Parte superior
    oled.pixel(64, 8, 1)
    oled.pixel(63, 9, 1)
    oled.pixel(65, 9, 1)
    oled.pixel(62, 10, 1)
    oled.pixel(66, 10, 1)
    oled.pixel(61, 11, 1)
    oled.pixel(67, 11, 1)
    oled.pixel(60, 12, 1)
    oled.pixel(68, 12, 1)
    oled.pixel(59, 13, 1)
    oled.pixel(69, 13, 1)
    oled.pixel(58, 14, 1)
    oled.pixel(70, 14, 1)
    oled.pixel(57, 15, 1)
    oled.pixel(71, 15, 1)
    oled.pixel(56, 16, 1)
    oled.pixel(72, 16, 1)
    oled.pixel(55, 17, 1)
    oled.pixel(73, 17, 1)
    oled.pixel(54, 18, 1)
    oled.pixel(74, 18, 1)
    oled.pixel(53, 19, 1)
    oled.pixel(75, 19, 1)
    oled.pixel(52, 20, 1)
    oled.pixel(76, 20, 1)
    oled.pixel(51, 21, 1)
    oled.pixel(77, 21, 1)

    # Parte media (más ancha)
    oled.pixel(50, 22, 1)
    oled.pixel(78, 22, 1)
    oled.pixel(49, 23, 1)
    oled.pixel(79, 23, 1)
    oled.pixel(48, 24, 1)
    oled.pixel(80, 24, 1)
    oled.pixel(47, 25, 1)
    oled.pixel(81, 25, 1)
    oled.pixel(46, 26, 1)
    oled.pixel(82, 26, 1)
    oled.pixel(45, 27, 1)
    oled.pixel(83, 27, 1)

    # Parte inferior
    oled.pixel(44, 28, 1)
    oled.pixel(84, 28, 1)
    oled.pixel(43, 29, 1)
    oled.pixel(85, 29, 1)
    oled.pixel(42, 30, 1)
    oled.pixel(86, 30, 1)

    # Trunk del árbol
    oled.pixel(63, 32, 1)
    oled.pixel(64, 32, 1)
    oled.pixel(65, 32, 1)

    oled.show()

# Función principal
def main():
    while True:
        distancia = leer_distancia()
        oled.fill(0)  # Limpiar la pantalla
        oled.text('Distancia: {:.2f} cm'.format(distancia), 0, 0)
        oled.show()

        if distancia < 10:
            # Reproducir melodía y cambiar LED y motor
            reproducir_melodia()
            mover_motor()
            dibujar_arbol()
        
        sleep(1)

# Ejecutar la función principal
main()
<h2>Flujo de Node-Red</h2>
[
    {
        "id": "2bec6c482b64de8f",
        "type": "tab",
        "label": "Personaje Navideño",
        "disabled": false,
        "info": "",
        "env": []
    },
    {
        "id": "9ea9fb3eed8e7b41",
        "type": "inject",
        "z": "2bec6c482b64de8f",
        "name": "Inject_1",
        "props": [
            {
                "p": "payload"
            },
            {
                "p": "topic",
                "vt": "str"
            }
        ],
        "repeat": "",
        "crontab": "",
        "once": false,
        "onceDelay": 0.1,
        "topic": "gds0642/ceoy/screen",
        "payload": "true",
        "payloadType": "bool",
        "x": 250,
        "y": 360,
        "wires": [
            [
                "261e77de67a36009"
            ]
        ]
    },
    {
        "id": "261e77de67a36009",
        "type": "ui_switch",
        "z": "2bec6c482b64de8f",
        "name": "",
        "label": "Pantalla",
        "tooltip": "",
        "group": "0fea946a726fbd8e",
        "order": 1,
        "width": 0,
        "height": 0,
        "passthru": true,
        "decouple": "false",
        "topic": "topic",
        "topicType": "msg",
        "style": "",
        "onvalue": "true",
        "onvalueType": "bool",
        "onicon": "",
        "oncolor": "",
        "offvalue": "false",
        "offvalueType": "bool",
        "officon": "",
        "offcolor": "",
        "animate": false,
        "className": "",
        "x": 460,
        "y": 380,
        "wires": [
            [
                "ba1f66507ded8dbf"
            ]
        ]
    },
    {
        "id": "ad201cf6ee945c9a",
        "type": "inject",
        "z": "2bec6c482b64de8f",
        "name": "Inject_0",
        "props": [
            {
                "p": "payload"
            },
            {
                "p": "topic",
                "vt": "str"
            }
        ],
        "repeat": "",
        "crontab": "",
        "once": false,
        "onceDelay": 0.1,
        "topic": "gds0642/ceoy/screen",
        "payload": "false",
        "payloadType": "bool",
        "x": 250,
        "y": 420,
        "wires": [
            [
                "261e77de67a36009"
            ]
        ]
    },
    {
        "id": "ba1f66507ded8dbf",
        "type": "mqtt out",
        "z": "2bec6c482b64de8f",
        "name": "Pantalla",
        "topic": "gds0642/ceoy/screen",
        "qos": "2",
        "retain": "false",
        "respTopic": "",
        "contentType": "",
        "userProps": "",
        "correl": "",
        "expiry": "",
        "broker": "e0dd5710309997b4",
        "x": 620,
        "y": 380,
        "wires": []
    },
    {
        "id": "0fea946a726fbd8e",
        "type": "ui_group",
        "name": "Pantalla",
        "tab": "49c174f3097eae0a",
        "order": 2,
        "disp": true,
        "width": "6",
        "collapse": false,
        "className": ""
    },
    {
        "id": "e0dd5710309997b4",
        "type": "mqtt-broker",
        "name": "",
        "broker": "broker.emqx.io",
        "port": "1883",
        "clientid": "",
        "autoConnect": true,
        "usetls": false,
        "protocolVersion": "4",
        "keepalive": "60",
        "cleansession": true,
        "autoUnsubscribe": true,
        "birthTopic": "",
        "birthQos": "0",
        "birthRetain": "false",
        "birthPayload": "",
        "birthMsg": {},
        "closeTopic": "",
        "closeQos": "0",
        "closeRetain": "false",
        "closePayload": "",
        "closeMsg": {},
        "willTopic": "",
        "willQos": "0",
        "willRetain": "false",
        "willPayload": "",
        "willMsg": {},
        "userProps": "",
        "sessionExpiry": ""
    },
    {
        "id": "49c174f3097eae0a",
        "type": "ui_tab",
        "name": "Personaje Navideño",
        "icon": "dashboard",
        "order": 4,
        "disabled": false,
        "hidden": false
    }
]
<h2>Link de video en TikTok</h2>

<h6>Coevaluacion a Cristian Efraín Oropeza Yepiz<h6>
Durante la elabolación del proyecto...


# DOCUMENTACIÓN
## Ejercicios en clase


## Lecciones de Netacad

