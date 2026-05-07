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

# Evolución del Estándar IEEE 802.11 (Wi-Fi)

> [!abstract] Resumen de Evolución
> 
> Las variantes del estándar IEEE 802.11 han evolucionado para ofrecer **mayores velocidades** y **mejor eficiencia** mediante cambios en las bandas de frecuencia, técnicas de modulación y el uso de múltiples antenas.
# Detalle de Versiones

> [!info] 1. 802.11b (Lanzada en 1999)
> 
> Fue la primera variante en alcanzar un gran éxito comercial.
> 
> - **Frecuencia:** Opera en la banda de 2.4 GHz (más propensa a interferencias pero con mayor alcance).
>     
> - **Velocidad máxima:** Hasta 11 Mbps.
>     
> - **Técnica:** Utiliza espectro ensanchado (**DSSS**) con secuencias de Barker y modulación **CCK** (Complementary code Keying). Con modulacion mas altas con el mismo ancho de banda. 


> [!todo] 2. 802.11a (Lanzada en 1999)
> 
> Utiliza una tecnología de modulación más avanzada. Soporta tasa de bits elevadas, esta menos usada
> 
> - **Frecuencia:** Opera exclusivamente en la banda de 5 GHz.
>     
> - **Velocidad máxima:** Hasta 54 Mbps.
>     
> - **Técnica:** Introdujo la multiplexación por división de frecuencias ortogonales (**OFDM**).
>     
> - _Nota:_ Su alcance es menor porque los 5 GHz no penetran tan bien las paredes. Señales de multiples carriers a diferentes frecuencias. Hasta 48 subcarriers modulados usando BPSK, QPSK, 16-QAM, o 64-QAM

> [!success] 3. 802.11g (Lanzada en 2003)
> 
> Buscó combinar las tecnicas de codificacion de capa fisica utilizados en 'a', y 'b' para proveer servicio a una variedad de tasas de bits
> 
> - **Frecuencia:** Vuelve a la banda de 2.4 GHz.
>     
> - **Velocidad máxima:** Hasta 54 Mbps.
>     
> - **Técnica:** Utiliza **OFDM** (como la "a") pero en la banda de la "b". 
>     
> - **Compatibilidad:** Totalmente compatible con dispositivos 802.11b.
> - Utiliza ERP-OFDM (6 a 54 Mbps). Dividiendo el canal en múltiples subportadoras que envían datos en paralelo, lo que permite aprovechar mejor el espectro y resistir interferencias. 
> - Tambien ERP-PBCC (22 y 33 Mbps). (Código Convolucional Binario de Paquetes). Es un esquema de codificación y modulación opcional dentro del estándar. Se diseñó para ofrecer velocidades intermedias (**22 y 33 Mbps**) que fueran superiores a las de 802.11b pero utilizando métodos de procesamiento de señal distintos al OFDM.

> [!warning] 4. 802.11n (Wi-Fi 4 - Lanzada en 2009)
> 
> Salto tecnológico importante para superar los 100 Mbps.
> 
> - **Frecuencia:** Puede operar tanto en 2.4 GHz como en 5 GHz.
>     
> - **Velocidad máxima:** Hasta 600 Mbps.
>     
> - **Innovaciones clave:**
>     
>     - **MIMO (Multiple Input Multiple Output):** Usa múltiples antenas (hasta 4 flujos).
>         
>     - **Channel Bonding:** Une dos canales separados no solapados de 20 MHz para formar uno de **40 MHz**.
>         

> [!tip] 5. 802.11ac (Wi-Fi 5 - Lanzada en 2013)
> 
> Estándar más extendido actualmente en dispositivos móviles modernos.
> 
> - **Frecuencia:** Opera únicamente en la banda de 5 GHz.
>     
> - **Velocidad máxima:** Hasta 6.93 Gbps (Gigabit Wi-Fi).
>     
> - **Innovaciones clave:**
>     
>     - **Canales más anchos:** Soporta **80 MHz y 160 MHz**. Hasta ocho flujos usando la banda de 5Ghz
>         
>     - **MU-MIMO (Multi User):** El punto de acceso habla con hasta cuatro clientes al mismo tiempo.
>         
>     - **Modulación 256-QAM:** Transporta más bits por símbolo.
>         

## Resumen Comparativo

|**Estándar**|**Año**|**Frecuencia**|**Velocidad Máx.**|**Técnica Principal**|
|---|---|---|---|---|
|**802.11b**|1999|2.4 GHz|11 Mbps|DSSS / CCK|
|**802.11a**|1999|5 GHz|54 Mbps|OFDM|
|**802.11g**|2003|2.4 GHz|54 Mbps|OFDM|
|**802.11n**|2009|2.4 / 5 GHz|600 Mbps|MIMO OFDM|
|**802.11ac**|2013|5 GHz|6.93 Gbps|MU-MIMO|

# Protocolo CSMA/CA (IEEE 802.11)

> [!abstract] Concepto Clave
> 
> El mecanismo **CSMA/CA** (Carrier Sense Multiple Access with Collision Avoidance) es el control de acceso al medio para redes Wi-Fi. A diferencia de Ethernet, Wi-Fi **no puede detectar colisiones** durante la transmisión porque las radios son **semidúplex** y la señal propia opaca a las demás (potencia 1.000.000 a 1).

## 1. Modos de Operación

# 1. Función de Coordinación Distribuida (DCF - Distributed Coordination Function)

Es el modo de operación **estándar y más común**, donde cada estación actúa de forma independiente sin un control centralizado.

> [!info]- Sus caracteristicas principales:
>- **Acceso basado en contención:** Las estaciones compiten por el canal utilizando un algoritmo de **backoff exponencial binario** similar al de Ethernet, pero con un inicio anticipado para evitar colisiones.
>- **Detección de canal dual:**
>**Física:** La estación escucha el medio para detectar señales de radio.
>**Virtual (NAV):** Utiliza el **Vector de Asignación de Red (NAV)**, un temporizador interno basado en el campo "duración" de las tramas de otras estaciones, para saber cuánto tiempo estará ocupado el canal sin necesidad de escucharlo físicamente.
>- **Intervalos entre tramas (IFS):** Utiliza huecos de tiempo como **DIFS** (para tramas normales) y **SIFS** (el más corto, para dar prioridad a respuestas inmediatas como el ACK).
>- **Confirmación explícita (ACK):** Dado que las colisiones no pueden detectarse físicamente mientras se transmite, el receptor debe enviar siempre una trama **ACK** para confirmar la recepción correcta; de lo contrario, el emisor asume una colisión.
>- **Mecanismo opcional RTS/CTS:** Permite reservar el canal mediante el intercambio de tramas de "petición para enviar" y "listo para enviar", lo cual ayuda a mitigar el **problema de la estación escondida** en tramas largas.
# Intervalos entre Tramas (IFS)
Establecen prioridades según el tiempo de espera:

|**Sigla**|**Nombre**|**Tiempo Típico**|**Uso Principal**|
|---|---|---|---|
|**SIFS**|Short IFS|10 μseg|Prioridad alta: **ACK** o respuesta **CTS**.|
|**PIFS**|PCF IFS|30 μseg|Utilizado en modo centralizado (PCF).|
|**DIFS**|DCF IFS|50 μseg|Intervalo estándar para **tramas de datos**.|
|**EIFS**|Extended IFS|Variable|Se usa tras recibir una **trama dañada**.|

# Algoritmo de Backoff (Retroceso)

Si el canal está ocupado, la estación espera un **DIFS** y luego:
1. Elige un número de ranuras inactivas (**slots**) al azar en el rango `[1, CW]`.
2. **CW (Contention Window):** Inicia en CWmin = 15.
3. El contador descuenta slots si el canal está libre; si hay tráfico, se pausa y reanuda tras otro DIFS.
4. Al llegar a **cero**, se transmite.
5. **Si falla (no hay ACK):** El valor de CW se duplica ($31, 63, ...$ hasta $CW_{max} = 1023$).
# Algoritmo de IFS + Backoff
Inicialmente se espera un DIFS
- Si el canal esta libre, se transmite
- Si no:  -  Se espera hasta que el medio este libre
		- Se espera un IFS + algoritmo de backoff
![[Pasted image 20260507183508.png]]
# Mecanismo Opcional RTS/CTS
Soluciona el problema de la **"estación escondida"**:
- **RTS (Request to Send):** El emisor solicita permiso e indica la duración.
- **CTS (Clear to Send):** El receptor confirma que el canal está libre.
- **Resultado:** Todas las estaciones que oyen el RTS o CTS actualizan su NAV y guardan silencio.
- _Nota:_ Solo se usa para **tramas muy largas** por el overhead que genera.