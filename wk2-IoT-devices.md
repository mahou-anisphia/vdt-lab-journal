# Wk2 IoT Devices

## 1. Device Layer

The device layer encompasses IoT devices such as sensors, actuators, microcontrollers (MCUs), and communication modules. The document details various device characteristics, including power consumption, memory, processing capabilities, and firmware considerations.

### 1.1 Device Types and Specifications

- **Sensors and Actuators**:
  - **Sensors**: The document references sensors under "Cảm biến IoT" (IoT sensors), but specific types (e.g., temperature, humidity) are not detailed. Standard IoT sensors include:
    - **Temperature Sensors**: Resolution: 0.1°C, Range: -40°C to 125°C, Power: ~1 µA in sleep mode.
    - **Humidity Sensors**: Resolution: 0.1% RH, Range: 0–100% RH, Power: ~1.5 µA.
    - **Motion Sensors (Accelerometers)**: Resolution: 16-bit, Range: ±2g to ±16g, Power: ~10 µA.
  - **Actuators**: Not explicitly mentioned but implied in IoT applications (e.g., smart metering). Examples include relays or motors for controlling physical systems.
  - **Integration**: Sensors and actuators use ADC (Analog-to-Digital Converter) for signal processing and communication protocols like I2C (400 kHz) or SPI (10 MHz) for data transfer to MCUs.

- **Microcontrollers (MCUs)**:
  - **Specifications** (from document, Page 12):
    - **Memory**: Limited RAM (e.g., 20–66 kB), Flash (128–512 kB), and ROM due to cost and power constraints.
    - **Processing**: MCUs (e.g., Cortex-M33, Cortex-M0+), DSPs, FPGAs, or SBCs. Clock speeds: 32–75.8 MHz.
    - **Power Consumption**: Active: 27–275 µA/MHz, Sleep: 0.6–1.68 µA.
    - **Selection Criteria**: Match MCU capabilities to application needs (e.g., edge vs. cloud processing), support for FOTA (Firmware Over-The-Air), and peripheral integration (e.g., UART, SPI, I2C).
  - **Operating Modes**:
    - **Active Mode**: Full operation for data processing or communication. Power: 27–275 µA/MHz.
    - **Light Sleep**: Reduced clock speed, peripherals partially active. Power: ~10 µA.
    - **Deep Sleep**: Minimal activity, only RTC (Real-Time Clock) active. Power: 0.6–1.68 µA.
    - **Standby**: Device off, wake-up on interrupt. Power: <0.5 µA.
    - **Use Cases**: Deep sleep for battery-powered devices (e.g., smart meters), active mode for real-time monitoring (e.g., industrial IoT).

- **Communication Modules**:
  - **Bluetooth Low Energy (BLE)** (Page 29):
    - **Modules**: EFR32BG22, CC2640, NRF51824, MKW37/8/9.
    - **Specifications**:
      - Frequency: 2.4 GHz.
      - Data Rate: Up to 2 Mbps (BLE 5.0).
      - Range: 10–100 m.
      - Power: TX: 4.1–12.8 mA, RX: 3.6–10.4 mA, Sleep: 0.6–1.68 µA.
      - Security: AES-128, Crypto Accelerator, TRNG (True Random Number Generator).
      - Protocols: UART, SPI, I2C for MCU interfacing.
    - **Operating Modes**:
      - **Active**: Full TX/RX operation.
      - **Sleep**: Low-power state with periodic wake-ups.
      - **Use Cases**: Wearables, smart home devices (e.g., door locks).
  - **4G/LTE Modules** (Page 27):
    - **Modules**: Sim7600G, EG25-G, LARA-L6, N75-4G.
    - **Specifications**:
      - Data Rate: Cat 1 (10/5 Mbps), Cat 4 (150/50 Mbps).
      - Frequency Bands: Regional/international (e.g., 700–2600 MHz).
      - Power: Active: ~100 mA, Idle: ~10 mA.
      - Features: GNSS support, 2G/3G fallback.
    - **Operating Modes**:
      - **PSM (Power Saving Mode)**: Device enters low-power state, wakes up periodically. Power: ~3 µA.
      - **eDRX (Extended Discontinuous Reception)**: Extended idle periods between RX cycles. Power: ~10 µA.
      - **Use Cases**: Smart cities, fleet management.
  - **NB-IoT/LTE Cat M Modules** (Page 28):
    - **Modules**: NRF9160, BC660K, Sim7022, SARA-R450S.
    - **Specifications**:
      - Data Rate: NB-IoT (~250 kbps), Cat M (~1 Mbps).
      - Frequency Bands: 700–2200 MHz.
      - Power: Active: ~50 mA, PSM: ~5 µA.
      - Features: GNSS, Open MCU support.
    - **Operating Modes**: PSM, eDRX, similar to 4G/LTE.
    - **Use Cases**: Smart metering, agriculture.
  - **GNSS Modules** (Page 32):
    - **Modules**: Sim65M, LC76F, MIA-M10Q, ZED-F9P.
    - **Specifications**:
      - Accuracy: Standard GNSS (3–5 m), RTK (0.01 m).
      - Satellites: GPS, GLONASS, BeiDou, Galileo, QZSS.
      - Power: Active: ~30 mA, Idle: ~5 mA.
      - TTFF (Time to First Fix): 1–30 seconds.
    - **Operating Modes**:
      - **Active**: Continuous tracking.
      - **Low-Power Tracking**: Reduced update rate. Power: ~10 mA.
      - **Use Cases**: Asset tracking, precision agriculture.

### 1.2 Implementation Workflow
- **Data Collection**: Sensors capture environmental data (e.g., temperature), processed via ADC.
- **Processing**: MCU processes data, applies algorithms (e.g., filtering), and prepares for transmission.
- **Communication**: Data sent via communication module (e.g., NB-IoT) to edge or cloud.
- **Challenges**: Limited memory/processing, power constraints.
- **Mitigation**: Optimize firmware for low power, use efficient protocols (e.g., MQTT), implement FOTA for updates.

### 1.3 Trade-offs and Failure Modes
- **Trade-offs**: High-performance MCUs increase power consumption vs. low-power MCUs with limited capabilities.
- **Failure Modes**: Battery depletion, firmware crashes, sensor drift.
- **Mitigation**: Watchdog timers, redundant power sources, calibration routines.

---

## 2. Connectivity Layer

The connectivity layer enables data transfer between devices, edge, and platforms. The document mentions BLE, 4G/LTE, NB-IoT, LTE Cat M, GNSS, and Wi-Fi, with additional standard IoT technologies (e.g., LoRa, Zigbee) included for completeness.

### 2.1 Connectivity Technologies

- **Bluetooth Low Energy (BLE)**:
  - **Specifications**:
    - Data Rate: 1–2 Mbps.
    - Range: 10–100 m.
    - Latency: ~6 ms.
    - Spectrum: Unlicensed 2.4 GHz.
    - Bandwidth: 2 MHz channels.
  - **Protocol Stack**:
    - Physical: GFSK modulation.
    - Network: Link Layer, L2CAP.
    - Application: GATT, GAP.
  - **Security**: AES-128, pairing, bonding.
  - **Operating Modes**:
    - **PSM**: Periodic wake-ups for advertising. Power: ~1 µA.
    - **Connection Mode**: Active TX/RX. Power: 4–12 mA.
    - **Use Cases**: Short-range, low-power applications (e.g., wearables).
  - **Workflow**: Device advertises, connects to gateway, sends data via GATT.
  - **Trade-offs**: Low power vs. limited range/data rate.

- **4G/LTE**:
  - **Specifications**:
    - Data Rate: Cat 1 (10/5 Mbps), Cat 4 (150/50 Mbps).
    - Range: 1–10 km.
    - Latency: 10–50 ms.
    - Spectrum: Licensed 700–2600 MHz.
    - Bandwidth: 1.4–20 MHz.
  - **Protocol Stack**:
    - Physical: OFDMA/SC-FDMA, QPSK/16QAM/64QAM.
    - Network: PDCP, RLC, MAC.
    - Application: TCP/IP, MQTT.
  - **Security**: AES, TLS, SIM-based authentication.
  - **Operating Modes**:
    - **PSM**: Deep sleep with periodic TAU (Tracking Area Update). Power: ~3 µA. Use case: Infrequent data (e.g., metering).
    - **eDRX**: Extended RX cycles (2.56–2621 s). Power: ~10 µA. Use case: Moderate data (e.g., asset tracking).
    - **DRX**: Short RX cycles (1.28–2.56 s). Power: ~50 µA. Use case: Real-time monitoring.
  - **Workflow**: Device registers with network, sends data via MQTT to platform.
  - **Trade-offs**: High bandwidth vs. high power consumption.

- **NB-IoT**:
  - **Specifications**:
    - Data Rate: ~250 kbps.
    - Range: 10–15 km.
    - Latency: 1–10 s.
    - Spectrum: Licensed 700–900 MHz.
    - Bandwidth: 180 kHz.
  - **Protocol Stack**:
    - Physical: OFDMA/SC-FDMA, QPSK.
    - Network: RRC, PDCP.
    - Application: CoAP, MQTT-SN.
  - **Security**: AES-128, DTLS, SIM-based.
  - **Operating Modes**:
    - **PSM**: Up to 413 days sleep. Power: ~5 µA. Use case: Smart metering.
    - **eDRX**: Cycles up to 10,485 s. Power: ~10 µA. Use case: Environmental monitoring.
  - **Workflow**: Device connects to base station, sends small data packets to platform.
  - **Trade-offs**: Ultra-low power vs. low data rate.

- **LTE Cat M**:
  - **Specifications**:
    - Data Rate: ~1 Mbps.
    - Range: 10 km.
    - Latency: 10–15 ms.
    - Spectrum: Licensed 700–2100 MHz.
    - Bandwidth: 1.4 MHz.
  - **Protocol Stack**: Similar to NB-IoT.
  - **Security**: Similar to NB-IoT.
  - **Operating Modes**: PSM, eDRX, DRX (similar to 4G/LTE).
  - **Workflow**: Similar to NB-IoT but supports higher data rates.
  - **Trade-offs**: Balances power and bandwidth vs. NB-IoT/4G.

- **Wi-Fi** (Page 30, limited details):
  - **Specifications**:
    - Data Rate: 11–600 Mbps (802.11n).
    - Range: 20–100 m.
    - Latency: 1–10 ms.
    - Spectrum: Unlicensed 2.4/5 GHz.
    - Bandwidth: 20–40 MHz.
  - **Protocol Stack**:
    - Physical: OFDM, BPSK/QAM.
    - Network: 802.11 MAC.
    - Application: HTTP, MQTT.
  - **Security**: WPA3, AES-256.
  - **Operating Modes**:
    - **Power-Save Polling**: Device sleeps, polls AP. Power: ~10 µA.
    - **Active**: Continuous TX/RX. Power: ~100 mA.
  - **Workflow**: Device connects to AP, sends data to cloud.
  - **Trade-offs**: High bandwidth vs. high power.

- **LoRa (Not in document, standard inclusion)**:
  - **Specifications**:
    - Data Rate: 0.3–50 kbps.
    - Range: 5–15 km.
    - Latency: 1–2 s.
    - Spectrum: Unlicensed 433/868/915 MHz.
    - Bandwidth: 125–500 kHz.
  - **Protocol Stack**:
    - Physical: CSS modulation.
    - Network: LoRaWAN.
    - Application: MQTT-SN, CoAP.
  - **Security**: AES-128, end-to-end encryption.
  - **Operating Modes**:
    - **Class A**: Downlink after uplink, low power. Power: ~10 µA.
    - **Class B/C**: Scheduled/continuous downlink, higher power.
  - **Workflow**: Device sends data to gateway, forwarded to network server.
  - **Trade-offs**: Long range vs. low data rate.

- **Zigbee (Page 10, mentioned in IoT Gateway)**:
  - **Specifications**:
    - Data Rate: 250 kbps.
    - Range: 10–100 m.
    - Latency: ~15 ms.
    - Spectrum: Unlicensed 2.4 GHz.
    - Bandwidth: 2 MHz.
  - **Protocol Stack**:
    - Physical: DSSS, O-QPSK.
    - Network: 802.15.4, Zigbee PRO.
    - Application: ZCL (Zigbee Cluster Library).
  - **Security**: AES-128, network key.
  - **Operating Modes**:
    - **Mesh Networking**: Devices relay data, extending range. Power: ~20 µA.
    - **Sleep**: End devices sleep, wake on demand. Power: ~1 µA.
  - **Workflow**: Devices form mesh, route data to coordinator.
  - **Trade-offs**: Mesh flexibility vs. complexity.

### 2.2 Comparative Analysis

| Technology | Data Rate | Range | Power (Idle) | Spectrum | Security | Reliability | Cost |
|------------|-----------|-------|--------------|----------|----------|-------------|------|
| BLE        | 1–2 Mbps  | 10–100 m | ~1 µA       | Unlicensed | AES-128 | High | Low |
| 4G/LTE     | 10–150 Mbps | 1–10 km | ~10 µA     | Licensed | AES, TLS | Very High | High |
| NB-IoT      | ~250 kbps | 10–15 km | ~5 µA      | Licensed | AES, DTLS | High | Medium |
| LTE Cat M  | ~1 Mbps   | 10 km    | ~5 µA      | Licensed | AES, DTLS | High | Medium |
| Wi-Fi      | 11–600 Mbps | 20–100 m | ~10 µA   | Unlicensed | WPA3 | Medium | Medium |
| LoRa       | 0.3–50 kbps | 5–15 km | ~10 µA    | Unlicensed | AES-128 | High | Low |
| Zigbee     | 250 kbps  | 10–100 m | ~1 µA      | Unlicensed | AES-128 | High | Low |

- **Narrative**:
  - **Low-Power Metering**: NB-IoT and LoRa excel due to PSM and long range, ideal for rural deployments.
  - **High-Bandwidth Monitoring**: 4G/LTE and Wi-Fi suit real-time applications (e.g., industrial IoT) but consume more power.
  - **Short-Range Applications**: BLE and Zigbee are cost-effective for smart homes, with Zigbee offering mesh networking.
  - **Rural Deployments**: LoRa and NB-IoT provide long-range, low-power solutions, unlike Wi-Fi or BLE.

### 2.3 Implementation Workflow
- **Device-to-Platform**: Device collects data, connects via chosen technology (e.g., NB-IoT), sends data using MQTT/CoAP to platform.
- **Challenges**: Network coverage, protocol interoperability.
- **Mitigation**: Multi-protocol gateways, fallback mechanisms (e.g., 4G to 3G).

---

## 3. Edge Layer

The edge layer includes gateways and edge nodes for local processing, reducing latency and bandwidth usage. The document details IoT Gateway architecture (Page 10).

### 3.1 Edge Components

- **IoT Gateway**:
  - **Architecture**:
    - **Hardware**: MCU/DSP, OS, Hardware Abstraction Layer.
    - **Protocols**: MQTT, CoAP, WebSockets, HTTPS, Modbus, PROFIBUS.
    - **Sensor Stacks**: Zigbee, 6LoWPAN, EnOcean, BLE.
    - **Security**: OpenSSL, AES.
    - **Cloud Connectivity**: Manages data transfer to cloud.
    - **Data Management**: Local storage, preprocessing.
  - **Processing Capabilities**: Limited by MCU (e.g., Cortex-M33, 75.8 MHz).
  - **Storage**: Flash (128–512 kB), RAM (20–66 kB).
  - **Roles**:
    - Aggregate sensor data.
    - Filter/process data locally (e.g., anomaly detection).
    - Reduce cloud bandwidth via compression.
  - **Operating Modes**:
    - **Active**: Continuous processing/communication. Power: ~100 mA.
    - **Idle**: Waiting for data. Power: ~10 mA.
    - **Use Cases**: Smart factories, local analytics.

### 3.2 Implementation Workflow
- **Data Flow**: Sensors send data to gateway via BLE/Zigbee, gateway processes (e.g., averaging), forwards to platform via 4G/Wi-Fi.
- **Challenges**: Limited processing power, protocol translation.
- **Mitigation**: Use efficient protocols (e.g., MQTT), optimize firmware.

### 3.3 Trade-offs and Failure Modes
- **Trade-offs**: Local processing reduces latency but increases gateway complexity.
- **Failure Modes**: Gateway overload, connectivity loss.
- **Mitigation**: Redundant gateways, local caching.

---

## 4. Platform Layer

The platform layer manages connectivity, devices, and applications. The document outlines platform components (Page 7).

### 4.1 Platform Components

- **User Management**: Manages user access, authentication (e.g., OAuth).
- **Device Management (DMP)**: Monitors device status, FOTA updates.
- **Data Management**: Stores, processes, and analyzes data.
- **Flow Engine**: Defines business logic/workflows.
- **Dashboard**: Visualizes data for monitoring.
- **Architecture**:
  - Scalable cloud-based systems.
  - Protocols: MQTT, CoAP, HTTPS.
  - Security: TLS, AES-256, role-based access.
- **Analytics**: Real-time processing, predictive maintenance.

### 4.2 Implementation Workflow
- **Data Flow**: Devices send data to platform via MQTT, platform stores/processes data, dashboard displays insights.
- **Challenges**: Scalability, data security.
- **Mitigation**: Distributed databases, encryption.

### 4.3 Trade-offs and Failure Modes
- **Trade-offs**: Cloud scalability vs. latency for real-time applications.
- **Failure Modes**: Data breaches, platform downtime.
- **Mitigation**: Redundant servers, intrusion detection.

---

## 5. Application Layer

The application layer delivers IoT solutions to end users. The document mentions applications (Page 8) and market trends (Page 4).

### 5.1 Applications

- **Smart Metering**:
  - **Devices**: NB-IoT modules, sensors (e.g., electricity meters).
  - **Connectivity**: NB-IoT, LoRa (low power, long range).
  - **Edge**: Gateways for local aggregation.
  - **Platform**: DMP for meter management, analytics for usage patterns.
  - **Functionality**: Remote monitoring, billing.
  - **Accuracy**: ~1% for energy readings.
  - **User Interaction**: Dashboards, mobile apps.

- **Smart Cities**:
  - **Devices**: 4G/LTE modules, GNSS for traffic monitoring.
  - **Connectivity**: 4G, Wi-Fi for high bandwidth.
  - **Edge**: Edge nodes for traffic analysis.
  - **Platform**: Real-time analytics, dashboards.
  - **Functionality**: Traffic management, waste monitoring.
  - **Accuracy**: ~3 m for GNSS-based positioning.
  - **User Interaction**: Public dashboards, alerts.

- **Industrial IoT**:
  - **Devices**: Sensors (e.g., vibration), actuators.
  - **Connectivity**: 4G, Zigbee for mesh networking.
  - **Edge**: Gateways for predictive maintenance.
  - **Platform**: Analytics for equipment health.
  - **Functionality**: Machine monitoring, automation.
  - **Accuracy**: ~0.1 mm for vibration sensors.
  - **User Interaction**: Operator dashboards, alerts.

### 5.2 Implementation Workflow
- **Smart Metering**: Meters collect usage data, send via NB-IoT to platform, analytics generate bills.
- **Challenges**: Device interoperability, data privacy.
- **Mitigation**: Standard protocols, encryption.

---

## 6. Comparative Analysis

See the connectivity comparison table in Section 2.2. Key insights:
- **Low-Power Applications**: NB-IoT, LoRa, and BLE are ideal due to PSM and sleep modes.
- **High-Bandwidth Applications**: 4G/LTE and Wi-Fi support real-time data but require more power.
- **Cost-Sensitive Deployments**: Zigbee and BLE are cost-effective for short-range applications.
- **Rural Deployments**: LoRa and NB-IoT offer long-range, low-power solutions.

---

## 7. Emerging Technologies and Future Directions

- **5G RedCap**: Reduced capability 5G for IoT, offering 10–100 Mbps, low latency (~5 ms). Applications: Industrial IoT, smart cities.
- **LoRaWAN 2.0**: Enhanced security, geolocation. Applications: Asset tracking, agriculture.
- **Matter**: Interoperability standard for smart homes, unifying BLE, Wi-Fi, Thread.
- **6G**: Expected by 2030, offering Tbps speeds, ultra-low latency. Applications: Holographic IoT, autonomous systems.
- **TSN (Time-Sensitive Networking)**: Deterministic Ethernet for industrial IoT, ensuring <1 ms latency.
- **Gaps**:
  - **Security**: IoT devices vulnerable to DDoS, side-channel attacks.
  - **Interoperability**: Fragmented protocols hinder scalability.
- **Research Directions**:
  - AI-driven security for anomaly detection.
  - Unified IoT standards (e.g., Matter expansion).
  - Energy harvesting for battery-less devices.

---

## 8. Technical Glossary

- **ADC**: Analog-to-Digital Converter, converts analog sensor signals to digital. Used in sensor interfacing.
- **AES**: Advanced Encryption Standard, symmetric encryption for IoT security (e.g., 128/256-bit).
- **BLE**: Bluetooth Low Energy, low-power wireless for short-range IoT (e.g., wearables).
- **CoAP**: Constrained Application Protocol, lightweight RESTful protocol for IoT.
- **DRX**: Discontinuous Reception, reduces power by limiting RX cycles. Used in 4G/LTE.
- **eDRX**: Extended DRX, longer RX cycles for lower power. Used in NB-IoT, LTE Cat M.
- **FOTA**: Firmware Over-The-Air, remote firmware updates for IoT devices.
- **GNSS**: Global Navigation Satellite System, provides positioning (e.g., GPS, GLONASS).
- **I2C**: Inter-Integrated Circuit, two-wire protocol for sensor-MCU communication.
- **LoRa**: Long Range, low-power wireless for long-range IoT (e.g., agriculture).
- **MQTT**: Message Queuing Telemetry Transport, lightweight pub/sub protocol for IoT.
- **NB-IoT**: Narrowband IoT, low-power cellular for long-range, low-data applications.
- **PSM**: Power Saving Mode, deep sleep with periodic wake-ups. Used in NB-IoT, LTE.
- **RTK**: Real-Time Kinematic, high-precision GNSS (0.01 m accuracy).
- **SPI**: Serial Peripheral Interface, high-speed protocol for sensor-MCU communication.
- **TLS**: Transport Layer Security, secures IoT communications (e.g., MQTT, HTTPS).
- **Watchdog Timer**: Resets device on firmware crash, preventing hangs.
- **Zigbee**: Low-power, mesh networking protocol for smart homes, industrial IoT.

---

## 9. Conclusion

This report provides a comprehensive analysis of the IoT ecosystem, covering all layers with technical depth. The device layer focuses on low-power, constrained devices; connectivity emphasizes diverse technologies like NB-IoT and BLE; edge computing reduces latency via gateways; platforms enable scalable management; and applications like smart metering drive IoT adoption. Emerging technologies (e.g., 5G RedCap, Matter) promise enhanced performance, but security and interoperability remain challenges. The provided glossary and comparative analyses offer a clear reference for IoT system design and deployment.
