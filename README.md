# 🤖 Mobile Robot Controlled via STM32 and Android App

## 📘 Project Overview

This project implements the control of a **mobile robot** using an **STM32 microcontroller** and an **Android application** via Bluetooth (HC-05).  
The robot can move **forward**, **rotate right by 45°** or **rotate continuously to the right**, either as a short rotation or continuously while the command is active. 

All motion commands are executed with **PWM motor control**, **GPIO-based direction**, and **timer-based rotation** for accurate, non-blocking control.

> 🏫 This project was carried out as part of the **"Conception des Systèmes Électroniques 1"** course at **l'École des Mines de Saint-Étienne**.

---

## ⚙️ Features and Performance Parameters

| Feature | Implementation / Value |
|---------|------------------------|
| Forward motion | PWM-driven DC motors |
| Rotation | Right turn (Δ angle = +45° per short command, continuous rotation possible) |
| PWM frequency | 2 kHz |
| Timer base | 5 ms (TIM6) |
| Forward speed | 20 cm/s |
| Rotation precision | ±5° |
| Command interface | UART via Bluetooth module |
| Stop | Immediately disables motor outputs |

---

## 🧩 Hardware Overview

| Component | Description |
|------------|-------------|
| MCU | STM32L476RG (Nucleo-L476RG board) |
| Motors | 2x DC motors with L298N driver |
| Bluetooth | Bluetooth 2.0 module (HC-05) for communication with Android phone |
| Power | Ni-Mh or Ni-Cd battery, rechargeable via 11 V supply. Fully charged ~8 V, considered discharged at ~6 V (test point 7.2 V). 8 V powers DC motors, 6 V for servo, and regulated 3.3 V for Nucleo board. |
---

## 🔧 STM32CubeMX (.ioc) Configuration

| Peripheral | Configuration | Purpose |
|-------------|---------------|---------|
| TIM4 | PWM Generation, 2 kHz, Channels 1 & 2 | Controls left and right motor speed |
| TIM6 | Basic timer, 5 ms base | Handles rotation timing (interrupt-based) |
| GPIO | Output pins | Motor direction |
| USART2 | 9600 baud, 8N1, RX/TX enabled | Bluetooth communication |
| NVIC | TIM6 interrupt enabled | Stop motors after rotation duration |

### Pin Mapping Example

| Function | Pin | Mode |
|----------|-----|------|
| Left motor direction | PA0 | GPIO Output |
| Right motor direction | PA1 | GPIO Output |
| Left motor PWM | PB6 | TIM4_CH1 |
| Right motor PWM | PB7 | TIM4_CH2 |
| UART TX | PA2 | USART2_TX |
| UART RX | PA3 | USART2_RX |

---

## 📡 Communication Protocol

Commands received from Android via UART:

| Command | Action |
|---------|--------|
| `'F'` | Move forward at 20 cm/s |
| `'R'` | Rotate right (short: +45°; continuous while pressed) |
| `'S'` | Stop immediately |

---

## Contributors
. Hamza BELARBI
. Ali AHNANI 