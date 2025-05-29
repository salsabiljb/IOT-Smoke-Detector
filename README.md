# 🔥 FreeRTOS Smoke Detection & Response System

This project implements a **real-time smoke and gas detection system** using **FreeRTOS** on an **STM32F407** microcontroller. It integrates sensors, alert systems, and IoT connectivity (via ESP8266 and Blynk) to monitor air quality and respond to dangerous gas levels.

## 🎯 Objective

To design a real-time embedded system that:
- Detects smoke/gas via MQ135 sensor
- Activates fan, buzzer, LEDs, and LCD alerts when thresholds are exceeded
- Sends warnings to a mobile app (Blynk) via WiFi (ESP8266)
- Provides real-time feedback using FreeRTOS tasks and semaphores

---

## 🧰 Hardware Components

| Component         | Purpose                               |
|------------------|----------------------------------------|
| STM32F407VG      | Core MCU with FreeRTOS support         |
| MQ135            | Gas sensor (CO2, smoke, NH3, etc.)     |
| DC Fan + L298N   | Ventilation control via PWM            |
| LCD 16x2 (I2C)   | Displays gas levels and alerts         |
| Buzzer & LEDs    | Audio/Visual indicators                |
| ESP8266 (NodeMCU)| Sends data and alerts via Blynk Cloud  |

---

## 🔧 Software Stack

- **FreeRTOS**: Real-time task scheduling and semaphore sync
- **STM32CubeMX / STM32CubeIDE**: Configuration and C code development
- **Arduino IDE**: Programming the ESP8266 for Blynk integration
- **Blynk IoT Platform**: Real-time dashboard and email alerts

---

## 🔁 System Behavior

### Task Scheduling

| Task Name | Priority | Role |
|-----------|----------|------|
| `Task_1`  | 3 (High) | Reads gas levels, signals alert when >2000 PPM |
| `Task_2`  | 1        | Normal state handler (fan off, green LED on) |
| `Task_3`  | 2        | Alert handler (fan on, buzzer, red LED on) |

- **Binary Semaphore** used for synchronization
- Each task uses `vTaskDelay` or blocks on semaphore when not active

### State Transitions
- **Normal State**: <2000 PPM → Green LED, Fan Off
- **Warning State**: >2000 PPM → Red LED, Fan On, Buzzer, LCD Alert, Notification
- Transitions controlled via global flags (`a`, `b`, `state`) and `xSemaphoreGive/Take`

---

## 🌐 WiFi & Cloud

### ESP8266 (NodeMCU)
- Receives gas level from STM32 via UART
- Sends data to Blynk using `Blynk.virtualWrite()`
- Triggers email alerts using `Blynk.logEvent()` when value > 2000 PPM

### Blynk Dashboard
- Real-time gas monitoring
- Customizable widgets for data visualization
- Alerts via push/email on dangerous gas levels

---

## 📊 PWM Motor Control (Fan)

- Controlled via **L298N H-Bridge**
- PWM signal from `TIM1_CH1`
- Frequency: 1 MHz (Prescaler = 50, Period = 10)
- Duty Cycle scaled based on gas concentration

---

## 📟 LCD Display (I2C)

- Displays real-time gas level and `"DANGER"` alert
- Controlled via `PCF8574` I2C backpack
- Only 4 pins needed: VCC, GND, SDA, SCL

---

## 🧪 Testing & Results

- Gas sensor triggers alert system at ~2000 PPM
- Real-time fan control and UI feedback
- Reliable dashboard updates via Blynk
- Smooth task execution with FreeRTOS

---

## 📦 Demo Access

📁 **Note**: The full demo (including STM32 project files, ESP8266 code, and test assets) is too large to be pushed to the repository.  
🔗 You can download the zipped archive here: [📥 Google Drive Link](https://your-google-drive-link.com)

---

## 🚀 Future Improvements

- Add more gas sensors (MQ2, MQ5)
- Let users adjust threshold via dashboard
- AI-based prediction of danger based on trends
- Visualize gas history with graphs

---

## 🔒 License

This project is created for educational purposes and shared for open development and learning in embedded systems.

---

💡 Built with STM32, FreeRTOS, and IoT for Smarter Homes.
