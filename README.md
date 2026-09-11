# 🚑 ESP32 Patient Monitor & Fall Alert System

[![Hardware](https://img.shields.io/badge/Hardware-ESP32%20%7C%20MAX30102%20%7C%20MPU6050-blue.svg)](#componentes)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PlatformIO](https://img.shields.io/badge/PlatformIO-Compatible-orange.svg)](https://platformio.org/)

Un sistema portátil, económico y de código abierto basado en el microcontrolador **ESP32** para el monitoreo remoto en tiempo real de signos vitales (SpO2 y Frecuencia Cardíaca) y detección automática de caídas mediante un acelerómetro de 6 ejes.

Diseñado con enfoque académico y social para **adultos mayores**, pacientes en cuidado domiciliario o centros de salud con recursos limitados en Colombia y Latinoamérica.

---

## 🎯 Motivación y Relevancia Social

El envejecimiento acelerado de la población y el déficit de cuidadores formales exigen soluciones tecnológicas de **bajo costo** y **alta accesibilidad**:
- **Detección temprana:** Las caídas no atendidas a tiempo representan una de las principales causas de complicación en salud para adultos mayores.
- **Sin costos de suscripción:** Utiliza la red WiFi doméstica/institucional y la API gratuita de **Telegram Bot API**, eliminando el uso de SIM cards o servicios en la nube de pago.
- **Contexto clínico:** Desarrollado bajo la perspectiva de apoyo técnico al paciente en entornos IPS y salud pública.

---

## 🛠️ Lista de Materiales (BOM)

| Componente | Función | Precio Aprox. (COP) |
| :--- | :--- | :---: |
| **ESP32 DevKit V1** | Microcontrolador principal (Dual Core + WiFi + Bluetooth) | ~$30.000 |
| **MAX30102** | Sensor fotopletismográfico (SpO2 y Frecuencia Cardíaca) | ~$22.000 |
| **MPU6050** | Acelerómetro y Giroscopio de 6 ejes (Detección de caídas) | ~$12.000 |
| **Buzzer Activo 5V** | Alarma sonora local para pánico o confirmación | ~$4.000 |
| **Módulo TP4056 + LiPo 3.7V** | Sistema de alimentación autónomo y recargable (1000mAh) | ~$25.000 |
| **Protoboard / Cableado** | Interconexión de componentes | ~$8.000 |
| **Caja / Enclosure 3D** | Protección y ergonomía para uso portátil | Variable |
| **TOTAL ESTIMADO** | | **~$101.000 COP (~$25 USD)** |

---

## 📐 Esquema de Conexionado (Pinout)

Ambos sensores (**MAX30102** y **MPU6050**) comparten el bus I2C del ESP32 gracias a que poseen diferentes direcciones I2C en el bus (`0x57` para MAX30102 y `0x68` para MPU6050).

| Componente | Pin del Componente | Pin ESP32 DevKit V1 | Notas |
| :--- | :--- | :--- | :--- |
| **MAX30102** | VCC | 3.3V | Voltaje lógico de 3.3V |
| | GND | GND | Tierra común |
| | SDA | GPIO 21 | Bus I2C (Compartido) |
| | SCL | GPIO 22 | Bus I2C (Compartido) |
| **MPU6050** | VCC | 3.3V / 5V | Regulador a bordes compatible |
| | GND | GND | Tierra común |
| | SDA | GPIO 21 | Bus I2C (Compartido) |
| | SCL | GPIO 22 | Bus I2C (Compartido) |
| **Buzzer Activo**| VCC (+) | GPIO 25 | Control digital (PWM / HIGH) |
| | GND (-) | GND | Tierra común |

---

## 🧮 Algoritmo de Detección de Caídas

El sistema analiza el **Módulo Vectorial de Aceleración Total ($A_T$)** producido por las fuerzas $X, Y, Z$:

$$A_T = \sqrt{a_x^2 + a_y^2 + a_z^2}$$

El algoritmo evalúa tres fases críticas consecutivas:
1. **Caída Libre (Free-fall):** $A_T < 0.4\,g$ durante más de 100 ms.
2. **Impacto Severo:** $A_T > 2.8\,g$ inmediatamente después de la caída libre.
3. **Inmovilidad Posterior:** Ausencia de aceleración abrupta en los 3 segundos posteriores al impacto.

Si el patrón cumple estos umbrales, el sistema activa el Buzzer local y envía un mensaje prioritario de emergencia a Telegram.

---

## 📱 Configuración del Bot de Telegram

1. Abre Telegram y busca a `@BotFather`.
2. Envía el comando `/newbot` y sigue las instrucciones para obtener el **Bot Token**.
3. Busca tu Bot recién creado en Telegram y presiona `Start`.
4. Obtén tu **Chat ID** usando el bot `@userinfobot`.
5. Coloca estas credenciales en el archivo `config.h` del firmware.

---

## ⚠️ Descargo de Responsabilidad (Academic Disclaimer)

Este dispositivo es un prototipo desarrollado con fines puramente **académicos, educativos y de investigación personal**. No cuenta con certificación de organismo regulador sanitario (INVIMA, FDA, CE) ni pretende sustituir equipos médicos profesionales de diagnóstico o monitoreo hemodinámico crítico.

---

## 👨‍💻 Autor

**Seykarim R. Mestre Zalabata**  
*Ingeniero Electrónico | Innovación en Salud & IoT Territorial*  
Valledupar, Cesar, Colombia  
[![GitHub](https://img.shields.io/badge/GitHub-Seykarim-181717?style=flat&logo=github)](https://github.com/)
