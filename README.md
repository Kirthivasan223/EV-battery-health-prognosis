# Battery Health Prognosis and Explosion Prevention in Electric Vehicles

A final year engineering project that builds an end-to-end, small-scale 
Battery Management System (BMS) for Li-ion EV batteries — combining 
real-time sensor data collection, a machine learning model for State of 
Health (SoH) prediction, IoT cloud integration, and an automated 
user alert system.


## Objective

- Deploy sensors to monitor Li-ion battery parameters at regular intervals
- Build a machine learning model to predict the State of Health (SoH) 
  of the battery
- Detect critical SoH levels, unsafe temperatures, and smoke presence
- Trigger automated user alerts when any parameter crosses a 
  safety threshold

---

## System Overview

The system operates in two integrated layers:

**Hardware Layer**
Sensors collect real-time readings of voltage, current, temperature, 
smoke levels, and internal resistance from a scaled-down Li-ion battery 
(3V). A Raspberry Pi acts as the central processing unit, transmitting 
this data to the cloud.

**Software Layer**
Voltage and current readings are fed into a trained Random Forest 
Regressor model to compute the battery's SoH. The SoH value, combined 
with temperature and smoke readings, is used to determine the State of 
Safety (SoS). If any parameter crosses a critical threshold, an 
automated alert email is sent to the user.


<img width="1883" height="712" alt="image" src="https://github.com/user-attachments/assets/9d959bb7-38be-484f-9d61-84e8cb0a6611" />

## Dataset

- **Source:** CALCE CS2 Dataset (publicly available battery cycling dataset)
- **Contents:** Current, voltage, and discharge capacity readings across 
  multiple charge/discharge cycles
- **Size:** 21,000+ data points used for training
- **Target Variable:** Discharge capacity, used to derive State of 
  Health (SoH)

---

## Machine Learning Model

**Algorithm:** Random Forest Regressor (Scikit-learn)

Random Forest was chosen over alternatives like RNN and Bayesian 
algorithms for the following reasons:
- Robust to noise and outliers in sensor data
- Faster training time while maintaining high accuracy
- Can be recalibrated for different battery chemistries and datasets

### Model Performance

| Metric | Score |
|---|---|
| R-squared (R²) | 0.9854 |
| Explained Variance Score | 0.9855 |
| Mean Absolute Error (MAE) | 0.70 |
| Mean Squared Error (MSE) | 9.09 |

An R² of 0.9854 indicates that the model explains 98.54% of the 
variance in battery SoH — a near-perfect fit on the test data.

---

## Hardware Components

| Parameter | Sensor Used |
|---|---|
| Voltage | LV 25-P |
| Current | ATO-AHKC-KAA (Non-invasive) |
| Temperature | MAX31856 |
| Pressure | Honeywell PX2 |
| Internal Resistance | YR-1070 Resistance Meter |
| Central Processing | Raspberry Pi |

The hardware setup is a scaled-down prototype of a real-world Tesla 
Model S BMS (100 kWh, 350–450V, 400A), implemented using a standard 
3V Li-ion battery for lab safety and feasibility.

---

## IoT Integration — ThingSpeak

- Sensor readings are transmitted from the Raspberry Pi to 
  **ThingSpeak**, an IoT cloud analytics platform
- The ML model's SoH predictions are also posted to ThingSpeak 
  for live visualisation
- ThingSpeak triggers automated **alert emails** to the user under 
  two conditions:
  - SoH drops below the critical threshold
  - Battery temperature exceeds the safe limit or smoke is detected

---

## Alert System

Two types of automated email alerts are generated:
1. **Low SoH Alert** — sent when the predicted State of Health falls 
   below the defined critical level
2. **Thermal/Smoke Alert** — sent when temperature crosses the danger 
   threshold or smoke is detected by the sensor

---

## Tech Stack

- **Language:** Python
- **ML Library:** Scikit-learn (Random Forest Regressor)
- **IoT Platform:** ThingSpeak
- **Hardware:** Raspberry Pi, Voltage/Current/Temperature/Smoke sensors
- **Dataset:** CALCE CS2

---

## Scope and Limitations

This is a scaled-down prototype intended to validate the concept. 
Real-world EV implementation would require industrial-grade sensors, 
higher voltage/current tolerances, and tighter hardware-software 
integration. The model can be recalibrated for different battery 
chemistries and form factors.

---

## References

Key papers that informed this work:
- Hu et al., *Health Prognosis for EV Battery Packs: A Data-Driven 
  Approach*, IEEE/ASME Transactions on Mechatronics, 2020
- Weng et al., *State-of-health monitoring via incremental capacity 
  peak tracking*, Applied Energy, 2016
- Xiong et al., *Review on Sensors Fault Diagnosis for Li-ion Batteries 
  in EVs*, IEEE IECA, 2018
