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

- **Propósito:** Resolver el problema conocido como **anomalía de velocidad (rate anomaly)**.

>[!important] Funcionamiento
>TXOP permite que una estación, tras ganar el acceso al canal, lo utilice durante un **periodo de tiempo fijo definido por el Punto de Acceso. Durante este intervalo, la estación puede transmitir todas las tramas que pueda.

>[!fire] **Beneficios:** 
>De esta forma, cada dispositivo obtiene la misma cantidad de tiempo de emisión en lugar del mismo número de tramas, lo que permite que las estaciones más rápidas consigan un mayor rendimiento sin ser "frenadas" por los dispositivos lentos.

# Fórmulas del TXOP 

### **1. Escenario SIN TXOP (Mecanismo Tradicional)
En el acceso tradicional, las estaciones compiten por el canal y envían **una trama a la vez**. Esto provoca que todas las estaciones acaben obteniendo la misma tasa de salida, independientemente de su velocidad nominal (problema del "nodo lento").
### Fórmula de Rendimiento Combinado

Para dos estaciones ($A$ y $B$), la tasa efectiva ($V_{TH}$) se calcula mediante la media armónica:

$$\frac{1}{V_{TH}} = \frac{1}{V_A} + \frac{1}{V_B}$$

> [!EXAMPLE] Ejemplo Práctico
> 
> Si la **Estación A** transmite a **5 Mbps** ($V_A$) y la **Estación B** a **60 Mbps** ($V_B$):
> 
> $$\frac{1}{V_{TH}} = \frac{1}{5} + \frac{1}{60} = \frac{12 + 1}{60} = \frac{13}{60}$$
> 
> $$V_{TH} = \frac{60}{13} \approx 4,62 \text{ Mbps}$$
> 
> **Resultado:** Ambas estaciones operan a **4,62 Mbps**. La estación rápida ($B$) se ve severamente perjudicada por la lentitud de la estación $A$.
### 2. Escenario CON TXOP

Con **TXOP**, en lugar de transmitir una sola trama, las estaciones tienen asignado un **tiempo de uso del canal**. Esto permite que cada estación aproveche su velocidad nominal durante ese tiempo.
### Fórmula de Rendimiento por Estación
El rendimiento de una estación específica bajo este esquema se basa en la división equitativa del tiempo de transmisión:

$$VTH_A = \frac{V_A}{N}$$

Donde:

- **$V_A$**: Tasa de bits nominal de la estación.
- **$N$**: Cantidad de estaciones compitiendo por el tiempo de forma equitativa.

> [!SUCCESS] Ejemplo Práctico (N = 2)
> 
> Siguiendo el caso anterior con las mismas dos estaciones:
> 
> - **Para Estación A (lenta):**
>     
>     $$VTH_A = \frac{5 \text{ Mbps}}{2} = 2,5 \text{ Mbps}$$
>     
> - **Para Estación B (rápida):**
>     
>     $$VTH_B = \frac{60 \text{ Mbps}}{2} = 30 \text{ Mbps}$$
>     
> 
> **Conclusión:** Aunque la estación lenta reduce su rendimiento a la mitad, la estación rápida logra un _throughput_ mucho mayor (**30 Mbps** frente a **4,62 Mbps**), mejorando la eficiencia global de la red.

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

>[!amarillo]  Estructura y Campos:
>- **Control de Trama (2 bytes):** Contiene los subcampos que definen el comportamiento de la trama (ver detalle abajo).
>- **Duración (2 bytes):** Indica el tiempo en microsegundos que la trama y su respectivo **ACK** ocuparán el canal. Se usa para actualizar el **NAV** de otras estaciones.
>- **Direcciones (Dirección 1 a 4, 6 bytes cada una):** Las tramas Wi-Fi pueden usar hasta 4 direcciones **MAC** para identificar el origen y destino final, además de los **AP**  intermedios.
>- **Secuencia (2 bytes):** Consta de 12 bits para el número de secuencia (detectar duplicados) y 4 bits para el número de fragmento.
>- **Datos (0 a 2312 bytes):** Contiene el paquete de capas superiores. 
>- **Secuencia de Verificación (**Check Sequence**, 4 bytes):** Un código **CRC** de 32 bits para detectar errores de transmisión.

# Subcampos del Control de Trama:

1. **Versión (2 bits):** Actualmente fijada en `00`.
2. **Tipo (2 bits):** Gestión (`00`), Control (`01`) o Datos (`10`).
3. **Subtipo (4 bits):** Define la función específica (ej. trama de datos regular o **QoS** - **Quality of Service** - Calidad de Servicio).
4. **To DS (Hacia el Sistema de Distribución):** Indica si la trama se dirige hacia la red cableada.
5. **From DS (Desde el Sistema de Distribución):** Indica si la trama proviene de la red cableada.
6. **Más fragmentos:** Indica que la trama actual es parte de una ráfaga y hay más fragmentos por venir.
7. **Retry (Reintento):** Indica que la trama es una retransmisión de una que no fue confirmada.
8. **Gestión de Energía:** Avisa al **AP** que la estación entrará en modo reposo 
9. **Más datos:** El emisor indica que tiene más tramas pendientes para el receptor.
10. **Protegido:** Indica que el cuerpo de la trama está cifrado.
11. **Order (Orden):** Indica que las tramas deben entregarse estrictamente en el orden recibido.

>[!danger] Campos TO DS y FROM DS
>Los campos **To DS** (Para el DS) y **From DS** (Desde el DS) son subcampos de un solo bit. Su función principal es indicar el origen y destino de la trama en relación con el **Sistema de Distribución (DS)**, que es la red (normalmente cableada) que interconecta los Puntos de Acceso (AP).
La combinación de estos dos bits es fundamental, ya que determina el significado de las hasta **cuatro direcciones MAC** que puede contener la trama de datos:
>- **To DS = 0, From DS = 0**: Indica una comunicación directa entre estaciones en modo **ad hoc**, sin la intervención de un Punto de Acceso. En este escenario, la Dirección 1 es el destino final y la Dirección 2 es el origen.
>- **To DS = 0, From DS = 1**: Significa que la trama proviene del Sistema de Distribución; es decir, el **AP la está enviando a una estación** cliente. Aquí, la Dirección 1 es el destino (la estación) y la Dirección 2 es el AP emisor.
>- **To DS = 1, From DS = 0**: Indica que la trama va hacia el Sistema de Distribución, enviada por una **estación cliente hacia un AP**. La Dirección 1 es el AP receptor y la Dirección 2 es el origen (la estación).
>- **To DS = 1, From DS = 1**: Se utiliza únicamente en configuraciones de **puente inalámbrico** (WDS), donde un AP envía tramas directamente a otro AP. Este es el único caso donde se utilizan las **cuatro direcciones MAC** para identificar al AP receptor, al AP emisor, al destino final y al origen real de los datos.

[[Conmutacion en la capa de enlace]]
