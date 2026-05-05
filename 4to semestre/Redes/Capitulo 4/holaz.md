- [*] **BSS - Basic Service Set:** Conjunto de Servicios Básicos
- [*] **AP - Access Point:** Punto de Acceso
- [*] **ESS - Extended Service Set:** Conjunto de Servicios Extendido
- [*] **SSID - Service Set Identifier:** Identificador de Conjunto de Servicio
- [I] **SSID - Service Set Identifier:** Es un identificador del ESS.

## Arquitectura de Red

- [I] **BSS (Basic Service Set):** Es la unidad fundamental de una red WiFi - 802.11. Consiste en estaciones inalámbricas que se comunican entre sí. Puede operar en modo de infraestructura (con un Punto de Acceso o AP) o en modo ad-hoc (sin AP).
- [I] **ESS (Extended Service Set):** Es una estructura de mayor escala formada por múltiples BSS conectados a través de un **Sistema de Distribución** (usualmente Ethernet). Esto permite que los usuarios se desplacen entre diferentes puntos de acceso sin perder la conectividad.
- [I] **SSID (Service Set Identifier):** Es el identificador de texto (nombre) que distingue a una red inalámbrica de otras.
- [I] **AP (Access Point):** Actúa como una estación base que coordina la comunicación dentro de un BSS y sirve de puente hacia el sistema de distribución cableado.

## Capas del Protocolo

	- **Subcapa MAC (Medium Access Control):** Una característica clave de la arquitectura 802.11 es que esta subcapa es **común** a todas las diferentes implementaciones de la capa física. Se encarga de gestionar el acceso al medio compartido y la formación de tramas.![[Pasted image 20260504154244.png]]
[[CSMA-CA]]


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
[[Tipos de WiFi]]

- **Capa Física (PHY):** Esta capa varía según el estándar utilizado (como 802.11a, b, g, n, etc.).
    - **Determinación de la tasa de bits:** La velocidad de transmisión (bitrate) se define mediante la combinación de la **técnica de modulación** y la **tasa de codificación** (que provee corrección de errores).
    - **Rate Adaptation - Adaptación de tasas:** Las velocidades se adaptan al medio (evita errores de transmisión)
    ![[Pasted image 20260504154830.png]]