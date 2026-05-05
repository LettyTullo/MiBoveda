## Redes LAN inalámbricas WiFi (IEEE 802.11)
Las redes LAN inalámbricas, conocidas comúnmente como **Wi-Fi**, se rigen por el estándar **IEEE 802.11** y están diseñadas para permitir la conectividad de dispositivos móviles mediante señales de radio de corto alcance. Opera en bandas de frecuencia sin licencia (bandas ISM), principalmente en **2.4 GHz** y **5 GHz**
# 1. Arquitectura de la Red
La estructura lógica de una red 802.11 se compone de los siguientes elementos:
- **BSS (Basic Service Set):** Es el bloque fundamental, consistente en un grupo de estaciones que se comunican entre sí.
- **ESS (Extended Service Set):** Es un conjunto de varios BSS interconectados a través de un **Sistema de Distribución (DS)**, generalmente una red Ethernet cableada. Para el usuario, el ESS aparece como una única red lógica identificada por un **SSID** (nombre de la red).
- **Modos de operación:**
    - **Modo Infraestructura:** Cada estación se asocia con un **Punto de Acceso (AP)** central que retransmite todos los paquetes.
    - **Modo Ad Hoc:** Las estaciones se comunican directamente entre sí sin necesidad de un AP
>[!info] Conceptos importantes 
>**El Access Point (AP):** Es un dispositivo que actúa como una **estación base** para la red inalámbrica. En una red en modo infraestructura, el AP es el centro de la comunicación; todos los clientes (como portátiles o teléfonos) se asocian a él, y cualquier mensaje entre clientes o hacia Internet debe pasar obligatoriamente por el AP
>**SSID (Service Set Identifier):** Es el identificador de texto (nombre) que distingue a una red inalámbrica de otras.

## Capas del protocolo
**Subcapa MAC (Medium Access Control):** Una característica clave de la arquitectura 802.11 es que esta subcapa es **común** a todas las diferentes implementaciones de la capa física. Se encarga de gestionar el acceso al medio compartido y la formación de tramas.

|                                  | IEEE 802.11n | IEEE 802.11ac    | IEEE 502.11ax    |
| -------------------------------- | ------------ | ---------------- | ---------------- |
| Generación                       | 4 - 2009     | 5 - 2013         | 6 - 2019         |
| Frecuencia                       | 2,4 / 5 GHz  | 5 GHz            | 2,4 / 5 GHz      |
| Ancho del canal                  | 20 / 40 MHZ  | 20/40/80/160 MHz | 20/40/80/160 MHz |
| Compatibilidad                   | 802.11a/b/g  | 802.11a/n        | 802.11a/b/g/n/ac |
| Modulación                       | OFDM         | OFDM             | OFDMA            |
| Codificación máxima              | 64 - QAM     | 256 - QAM        | 1024 - QAM       |
| Tasa de transmisión máx          | 600 Mbps     | 6,933 Gbps       | 9,607 Gbps       |
| MIMO                             | MIMO         | MU-MIMO downlink | MU - MIMO ambos  |
| Máximo de cadenas de transmisión | 4            | 8                | 8                |
**Capa Física (PHY):** Esta capa varía según el estándar utilizado (como 802.11a, b, g, n, etc.).
- **Determinación de la tasa de bits:** La velocidad de transmisión (bitrate) se define mediante la combinación de la **técnica de modulación** y la **tasa de codificación** (que provee corrección de errores).
- **Rate Adaptation - Adaptación de tasas:** Las velocidades se adaptan al medio (evita errores de transmisión)
# Tipos de WIFI
