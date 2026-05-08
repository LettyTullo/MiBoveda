## Tecnicas de calidad de servicio
# 1.  Intervalos de Inter-trama de Arbitraje (AIFS - Arbitration Inter Frame Space)
 Su función principal es permitir que el tráfico con diferentes niveles de prioridad compita por el acceso al canal de manera diferenciada, en lugar de que todas las estaciones esperen el mismo tiempo estándar (DIFS).
#  Funcionamiento y Propósito

En el mecanismo original de Wi-Fi, todas las tramas de datos debían esperar un intervalo **DIFS** antes de intentar transmitir. El estándar **802.11e** sustituye el uso de un DIFS único por los intervalos **AIFS**, los cuales varían en longitud según la categoría del tráfico:
- **Prioridad mediante el tiempo:** Cuanto más corto es el intervalo AIFS, mayor es la prioridad del tráfico, ya que la estación puede empezar su cuenta atrás de backoff antes que las demás.
- **Diferenciación de servicios:** El mecanismo define cuatro niveles de prioridad (clases de acceso), cada uno con sus propios parámetros de espera e inactividad.

>[!warning]  Tipos de intervalos AIFS definidos
>- **AIFS1 (Intervalo corto):** Es **más pequeño que el DIFS** (pero más largo que el SIFS). Se utiliza para mover el tráfico de **alta prioridad**, como la **voz**, a la cabecera de la línea de transmisión, permitiendo que se envíe antes que el tráfico normal.
>- **AIFS4 (Intervalo largo):** Es **mayor que el DIFS**. Se asigna al **tráfico de fondo** o de baja prioridad, el cual se aplaza hasta que el tráfico regular haya tenido su oportunidad de transmitir.

# Relación con la Ventana de Contención (CW)
Además de variar los tiempos de espera de inter-trama, el estándar 802.11e también define **nuevos valores de Ventana de Contención (CW - Contention Window)** asociados a cada nivel de AIFS. Esto significa que, además de esperar menos tiempo para empezar a contar, las tramas de alta prioridad suelen tener rangos de números aleatorios de backoff más pequeños, reduciendo aún más su retardo promedio en comparación con el tráfico de fondo.
# Jerarquía de los espacios de tiempo (de menor a mayor):

De acuerdo con los diagramas de las fuentes, la jerarquía de prioridades se establece así:
1. **SIFS:** El más corto, para respuestas inmediatas (ACK).
2. **AIFS1:** Para tramas de alta prioridad (voz).
3. **DIFS:** Para tramas normales de datos (DCF regular).
4. **AIFS4:** Para tramas de baja prioridad (fondo).
5. **EIFS:** El más largo, utilizado tras recibir tramas dañadas.
# Anomalía de la tasa de datos (o _rate anomaly_)
Es un problema de rendimiento que ocurre en las redes inalámbricas cuando estaciones que operan a diferentes velocidades (lentas y rápidas) comparten el mismo medio o espacio geográfico.

>[!example] 1. Causa de la anomalía
En el diseño original del mecanismo **CSMA/CA** ), el protocolo permitía que cada estación enviara **una sola trama** cada vez que ganaba acceso al canal.
> - Una estación lenta (por ejemplo, a 6 Mbps) tarda mucho más tiempo en transmitir su trama que una estación rápida (por ejemplo, a 54 Mbps).
> - Como resultado, la estación lenta ocupa el canal durante un periodo desproporcionadamente largo.

>[!amarillo] 2. Consecuencia: Degradación del rendimiento
>Debido a que la estación rápida debe esperar a que la lenta termine sus largas transmisiones, el emisor rápido se ve penalizado y su velocidad efectiva se reduce drásticamente, acercándose a la velocidad del emisor más lento.
> - **Ejemplo matemático de las fuentes:** Si una estación A transmite a **5 Mbps** y una estación B a **60 Mbps** de forma alternada (una trama cada una), ambas acabarán consiguiendo el mismo **throughput** o tasa de salida.
> - En este escenario, ambas estaciones transmitirán a una velocidad efectiva de solo **4,62 Mbps**, lo que perjudica claramente a la estación más rápida
# 2. Oportunidad de Transmisión (TXOP)

- **Propósito:** Resolver el problema conocido como **anomalía de velocidad (rate anomaly)**. En las redes originales, cada estación enviaba una sola trama por turno; si una estación era muy lenta (6 Mbps) y otra rápida (54 Mbps), la estación lenta ocupaba el canal mucho más tiempo, penalizando drásticamente el rendimiento de la estación rápida.

>[!important] Funcionamiento
>TXOP** permite que una estación, tras ganar el acceso al canal, lo utilice durante un **periodo de tiempo fijo** definido por el Punto de Acceso. Durante este intervalo, la estación puede transmitir **todas las tramas que pueda**.

>[!fire] **Beneficios:** 
>De esta forma, cada dispositivo obtiene la misma cantidad de tiempo de emisión en lugar del mismo número de tramas, lo que permite que las estaciones más rápidas consigan un mayor rendimiento sin ser "frenadas" por los dispositivos lentos.

## Servicios IEEE 802.11
![[Pasted image 20260508154505.png]]

## Tipos de tramas 802.11
