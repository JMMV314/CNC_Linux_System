# Control Centralizado de Ruteadoras CNC con ESP32 y Raspberry Pi

Este proyecto busca **automatizar y centralizar el control de múltiples ruteadoras CNC** utilizadas para la producción de PCB.  
Cada ruteadora está controlada por un **ESP32**, que se comunica mediante **Wi-Fi** con una **Raspberry Pi**, la cual actúa como **nodo maestro y gateway hacia un servidor en la nube (AWS)**.

---

## Descripción general

Actualmente, el control de cada ruteadora CNC se realiza de forma independiente, ya sea:
- Mediante una **tarjeta SD** con el archivo G-code, o
- A través de una **conexión serial directa** desde un PC.

Esto requiere intervención humana y múltiples equipos, aumentando el costo y el riesgo de error.

El presente sistema busca eliminar esa dependencia, permitiendo que:
- Una sola **Raspberry Pi** gestione todas las máquinas,
- Se controle la producción mediante una **interfaz táctil**, y
- Se registren automáticamente los tiempos de operación y estados.

---

## Componentes del sistema

### Hardware
- **Raspberry Pi 4B / 5** — Nodo maestro y gateway.
- **ESP32-C6 (x10)** — Controladores locales de las ruteadoras.
- **Ruteadoras CNC** — Equipos de producción.
- **Red Wi-Fi local** — Comunicación entre Raspberry y ESP32.
- **Servidor AWS (EC2 / IoT Core / S3)** — Almacenamiento y monitoreo remoto.

### Software
- **Sistema operativo:** Raspberry Pi OS (Linux)
- **Lenguajes:** Python (control y UI), C++ (firmware ESP32)
- **Protocolo de comunicación:** MQTT / TCP sockets
- **Base de datos local:** SQLite / JSON logs
- **Interfaz:** Touchscreen local con dashboard

---

## ⚙️ Arquitectura del sistema

```mermaid
flowchart TD
    A[Raspberry Pi (Main Control App)] -->|Wi-Fi| B1[ESP32 #1 - CNC Router 1]
    A -->|Wi-Fi| B2[ESP32 #2 - CNC Router 2]
    A -->|Wi-Fi| B3[ESP32 #3 - CNC Router 3]
    A -->|AWS API| C[Cloud Server]
    
    B1 --> D1[CNC Router 1]
    B2 --> D2[CNC Router 2]
    B3 --> D3[CNC Router 3]
