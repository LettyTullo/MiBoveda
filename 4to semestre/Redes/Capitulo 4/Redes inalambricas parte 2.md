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
Fórmulas del TXOP

Las fórmulas permiten comparar el rendimiento o **VTH (Throughput - Tasa de salida)** del sistema cuando se utiliza esta técnica frente a cuando no se utiliza.
1. **Escenario SIN TXOP (Mecanismo tradicional)**
Cuando las estaciones operan de forma alternada enviando una trama a la vez, ambas acaban obteniendo la misma tasa de salida, independientemente de su velocidad nominal. La fórmula para calcular el rendimiento combinado o individual (VTH​) de dos estaciones (A y B) es:

VA​1​+VB​1​=VTH​1​

- **Ejemplo:** Si la estación A transmite a 5 Mbps y la B a 60 Mbps, al aplicar la fórmula, ambas estaciones operarán a una velocidad efectiva de solo **4,62 Mbps**, perjudicando severamente a la estación más rápida.

2. Escenario CON TXOP

Con el uso de **TXOP**, las tasas de salida no son iguales porque cada una aprovecha su tiempo de emisión según su capacidad técnica. La fórmula para determinar el rendimiento de una estación específica bajo este esquema es:

VTH_A​=NVA​​

Donde:

- VA​: Es la tasa de bits nominal de la estación.
- N: Es la cantidad de estaciones que están compitiendo y compartiendo el tiempo de forma equitativa.
- **Ejemplo:** Siguiendo el caso anterior con 2 estaciones (N=2):
    - VTH_A​: 5 Mbps / 2 = **2,5 Mbps**.
    - VTH_B​: 60 Mbps / 2 = **30 Mbps**.
## Servicios IEEE 802.11
![[Pasted image 20260508154505.png|506]]

## Tipos de tramas 802.11

El estándar 802.11 define **tres clases principales** de tramas para gestionar la comunicación en el aire:
# 1. Tramas de Administración (Management):
Se utilizan para establecer y mantener la comunicación entre las estaciones y los **Puntos de Acceso (AP)**. Incluyen:
    - **Beacons (Balizas):** Emitidas periódicamente por el AP para anunciar la presencia de la red (SSID), sincronizar relojes y avisar sobre datos pendientes en modo de ahorro de energía.
    - **Tramas de Asociación y Autenticación:** Permiten que un dispositivo se registre en un AP y demuestre sus credenciales antes de transmitir datos.
# Tramas de Control:
Son tramas cortas que ayudan en la entrega de las tramas de datos y gestionan el acceso al medio ruidoso. Subtipos clave son:
    - **RTS (Request to Send) y CTS (Clear to Send):** Utilizadas opcionalmente para reservar el canal y evitar colisiones por "estaciones escondidas".
    - **ACK (Acuse de recibo):** Enviadas inmediatamente después de recibir una trama de datos para confirmar su llegada.
    - **PS-Poll (Power Save-Poll):** Usadas por estaciones en modo ahorro de energía para solicitar al AP la entrega de datos almacenados. 
    - **Contention-Free (CF) - end:** Anuncia fin del periodo de contencion libre en (PCF)
# Tramas de Datos:
Transportan la carga útil real de la red (como paquetes IP). Se caracterizan por poder contener hasta **4 direcciones MAC** para gestionar el tráfico que pasa a través del sistema de distribución inalámbrico.
![[Pasted image 20260508155530.png|529]]

>[!amarillo]  Alugunos campos:
>**Duracion:** En microsegundos, tiempo de transmision de la trama +ACK
>**Secuencia**: 12 bits de secuencia de la trama + 4 bits de numero de fragmento
>**Check sequence:** CRC de 32 bits.

>[!danger] Campos TO DS y FROM DS
>Los campos **To DS** (Para el DS) y **From DS** (Desde el DS) son subcampos de un solo bit. Su función principal es indicar el origen y destino de la trama en relación con el **Sistema de Distribución (DS)**, que es la red (normalmente cableada) que interconecta los Puntos de Acceso (AP).
La combinación de estos dos bits es fundamental, ya que determina el significado de las hasta **cuatro direcciones MAC** que puede contener la trama de datos:
>- **To DS = 0, From DS = 0**: Indica una comunicación directa entre estaciones en modo **ad hoc**, sin la intervención de un Punto de Acceso. En este escenario, la Dirección 1 es el destino final y la Dirección 2 es el origen.
>- **To DS = 0, From DS = 1**: Significa que la trama proviene del Sistema de Distribución; es decir, el **AP la está enviando a una estación** cliente. Aquí, la Dirección 1 es el destino (la estación) y la Dirección 2 es el AP emisor.
>- **To DS = 1, From DS = 0**: Indica que la trama va hacia el Sistema de Distribución, enviada por una **estación cliente hacia un AP**. La Dirección 1 es el AP receptor y la Dirección 2 es el origen (la estación).
>- **To DS = 1, From DS = 1**: Se utiliza únicamente en configuraciones de **puente inalámbrico** (WDS), donde un AP envía tramas directamente a otro AP. Este es el único caso donde se utilizan las **cuatro direcciones MAC** para identificar al AP receptor, al AP emisor, al destino final y al origen real de los datos.

[[Conmutacion en la capa de enlace]]