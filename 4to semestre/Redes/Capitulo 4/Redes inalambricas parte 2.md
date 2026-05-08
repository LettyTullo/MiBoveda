## Tecnicas de calidad de servicio

 # Intervalos de Inter-trama de Arbitraje (AIFS - Arbitration Inter Frame Space)
 son una técnica de **Calidad de Servicio (QoS)** introducida por el estándar **IEEE 802.11e** en 2005. Su función principal es permitir que el tráfico con diferentes niveles de prioridad compita por el acceso al canal de manera diferenciada, en lugar de que todas las estaciones esperen el mismo tiempo estándar (DIFS).

A continuación se detallan sus características y funcionamiento según las fuentes:

### 1. Funcionamiento y Propósito

En el mecanismo original de Wi-Fi, todas las tramas de datos debían esperar un intervalo **DIFS** antes de intentar transmitir. El estándar **802.11e** sustituye el uso de un DIFS único por los intervalos **AIFS**, los cuales varían en longitud según la categoría del tráfico:

- **Prioridad mediante el tiempo:** Cuanto más corto es el intervalo AIFS, mayor es la prioridad del tráfico, ya que la estación puede empezar su cuenta atrás de backoff antes que las demás.
- **Diferenciación de servicios:** El mecanismo define cuatro niveles de prioridad (clases de acceso), cada uno con sus propios parámetros de espera e inactividad.

### 2. Tipos de intervalos AIFS definidos

Las fuentes destacan dos ejemplos claros de cómo se ajustan estos tiempos para gestionar la prioridad:

- **AIFS1 (Intervalo corto):** Es **más pequeño que el DIFS** (pero más largo que el SIFS). Se utiliza para mover el tráfico de **alta prioridad**, como la **voz**, a la cabecera de la línea de transmisión, permitiendo que se envíe antes que el tráfico normal.
- **AIFS4 (Intervalo largo):** Es **mayor que el DIFS**. Se asigna al **tráfico de fondo** o de baja prioridad, el cual se aplaza hasta que el tráfico regular haya tenido su oportunidad de transmitir.

### 3. Relación con la Ventana de Contención (CW)

Además de variar los tiempos de espera de inter-trama, el estándar 802.11e también define **nuevos valores de Ventana de Contención (CW - Contention Window)** asociados a cada nivel de AIFS. Esto significa que, además de esperar menos tiempo para empezar a contar, las tramas de alta prioridad suelen tener rangos de números aleatorios de backoff más pequeños, reduciendo aún más su retardo promedio en comparación con el tráfico de fondo.

### Jerarquía de los espacios de tiempo (de menor a mayor):

De acuerdo con los diagramas de las fuentes, la jerarquía de prioridades se establece así:

1. **SIFS:** El más corto, para respuestas inmediatas (ACK).
2. **AIFS1:** Para tramas de alta prioridad (voz).
3. **DIFS:** Para tramas normales de datos (DCF regular).
4. **AIFS4:** Para tramas de baja prioridad (fondo).
5. **EIFS:** El más largo, utilizado tras recibir tramas dañadas.
# Oportunidad de Transmisión (TXOP)

El concepto de **TXOP** fue introducido con el estándar **802.11e** en 2005 como una técnica avanzada de Calidad de Servicio.
- **Propósito:** Resolver el problema conocido como **anomalía de velocidad (rate anomaly)**. En las redes originales, cada estación enviaba una sola trama por turno; si una estación era muy lenta (6 Mbps) y otra rápida (54 Mbps), la estación lenta ocupaba el canal mucho más tiempo, penalizando drásticamente el rendimiento de la estación rápida.

>[!important] Funcionamiento
>TXOP** permite que una estación, tras ganar el acceso al canal, lo utilice durante un **periodo de tiempo fijo** definido por el Punto de Acceso. Durante este intervalo, la estación puede transmitir **todas las tramas que pueda**.

>[!fire] **Beneficios:** 
>De esta forma, cada dispositivo obtiene la misma cantidad de tiempo de emisión en lugar del mismo número de tramas, lo que permite que las estaciones más rápidas consigan un mayor rendimiento sin ser "frenadas" por los dispositivos lentos.

