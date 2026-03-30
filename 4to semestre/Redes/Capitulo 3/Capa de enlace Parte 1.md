La capa de enlace de datos es la responsable del envío de **tramas** * de información en un enlace simple. Además maneja los errores de transmisión y regula el flujo de datos.
 - [I] **Tramas:** Es la unidad básica de información que se intercambia en la capa de enlace de datos. Es la organización de los bits en unidades discretas y enteras (tramas) para lograr una comunicación fiable entre dos máquinas directamente conectadas.

==Los bits se entregan exactamente en el mismo orden en el que se envían==

>[!important] Qué hace la capa de enlace de datos
>Acepta los paquetes de la [[3. Capa de Red|Capa de Red]], los encapsula en tramas y los envía usando la [[1. Capa física |Capa Física]]. La recepción es el proceso opuesto. Su principal función es proporcionar servicios a la capa de red.

**Corresponde** a la capa de enlace de datos **detectar y**, si es posible, **corregir los errores** obtenidos de la capa física, ya que esta trata de enviar los bits a la capa de enlace de datos sin procesarlos y entregarlos a destino. La capa física envía los bits con bits de redundancia, para reducir la tasa de errores, pero no garantiza que lleguen sin error a destino.

## Problemas de diseño de la capa de enlace de datos

>[!warning] Atención
>Usa los servicios de la capa física inferior para enviar y recibir bits a través de canales de comunicación poco fiables que pueden perder datos.
>**Funciones:**
>1. Proporcionar una **interfaz de servicio bien definida** con la capa de red
>2. **Identificación de las tramas** en la secuencia de bytes como segmentos autocontenidos.
>3. **Detección y corrección de errores** de transmisión.
>4. **Control de flujo**: no permitir que receptores más lentos sean sobrepasados por transmisores más rápidos.
>5. Direccionamiento a nivel del enlace de datos (**direccionamiento físico**) - inicio y fin del enlace.

## Servicios prestados a la [[3. Capa de Red|Capa de Red]]
El principal servicio de la capa de enlace es transferir datos desde la capa de red en la máquina emisora u origen hasta la máquina receptora o destino. En la máquina origen hay un proceso en la que la capa de red pasa paquetes a la capa de enlace de datos para su transmisión a destino.
### Servicio sin conexión
#### Sin confirmación de recepción
- La maquina de origen envía tramas independientes sin que la maquina destino lo reconozca. -
	Apropiada cuando la tasa de errores es muy baja. Ejemplo: Ethernet.
#### Con confirmación de recepción
- Consiste en que la máquina origen envía los datos a la máquina destino, pudiendo reconocer el destino si una trama ha llegado correctamente o se ha perdido.
	Cuando la tasa de errores es importante o en canales inestables (poco fiables). Ejemplo: Redes Wi-Fi (802.11).
### Servicio orientado a la conexión con confirmación de recepción
- Las máquinas de origen y destino establecen una conexión antes del envío de datos. Cada trama está numerada, la capa de enlace garantiza el orden y que cada trama será recibida una vez.
	Ejemplo: Redes [[Clasificación por Alcance Geográfico#WAN (Wide Area Network)|WAN]]


El diseño debe facilitar al receptor la localización del inicio de nuevas tramas utilizando **poco ancho de banda** del canal. Los métodos comunes de encuadre son:
## Conteo de caracteres o Bytes
Se usa un campo al inicio de la trama indicando el número de bytes de la misma.

>[!warning] Desventajas
>- Puede **perderse el sincronismo** por culpa de un error causando distorsión.
>	Si la cuenta de bytes es 5 en la segunda trama, se convierte en 7 debido a un solo bit.
>- [i] Rara vez se utiliza por sí solo por esta razón

![[Pasted image 20260327160201.png|400]]
## Marcar Bytes con relleno de Bytes - byte stuffing
Resuelve el problema de desincronización tras un error de transmisión haciendo que cada trama comience y termine con bytes especiales (banderas -> FLAG). 

> [!important] La bandera se adiciona **al inicio y al final** de la trama
![[Pasted image 20260327160815.png]]

Desventaja: está ligado al uso de bytes (8 bits) si o si.

- [I] **Byte stuffing:** consiste en agregar un byte de escape especial (ESC) cuando cualquiera de los bytes banderas aparezcan coincidentemente como secuencia dentro de la trama. La capa de enlace de datos en el extremo receptor elimina los bytes ESC antes de entregar los datos a la capa de red.
	Si el byte ESC también se encuentra dentro de la secuencia de bytes de la trama, se añade otro byte ESC o FLAG identificador.

## Bits de bandera con relleno de bits - bit stuffing
Las tramas pueden contener un número arbitrario de bits formados por unidades de **cualquier tamaño**. El relleno no permite que se interpongan los bits de relleno con los bits de los datos.

- [I] **Bit stuffing:** es la técnica que permite que los datos del usuario contengan cualquier combinación de "1" y "0" sin que el receptor los confunda con las señales de control que marcan el inicio o el fin de una trama. Cada trama comienza y termina con un patrón especial de bits

Se desarrolló para el protocolo HDLC (High Level Data Link Control).
Cada trama comienza y termina con un patrón de bits igual a $01111110_2$ (es el estándar más común) que es lo mismo que $0x7E_{16}$. Este patrón es una bandera, por lo que si en la Capa de Enlace de Datos del emisor se encuentran 5 bits "1", se introduce automáticamente un bit "0" en los bits salientes.

>[!info] Funcionamiento
>- Emisor: Agrega un 0 luego de 5 bits "1" seguidos (*stuffing*)
>	- Datos originales: 01111110
>	- Datos con relleno: 011111**0**10
>- Receptor: (*destuffing*)
>	- si ve **5** bits "1" seguidos de un "0", lo elimina porque sabe que **es el relleno**.
>	- si ve **6** bits "1" seguidos de un "0", sabe que es el FLAG de **fin de trama**.
>	- si ve **7** bits "1" se considera **error de transmisión**.
## Violaciones de codificación de la capa física
Se usa un atajo de la capa física, violaciones de codificación que serían **caracteres no válidos** para delimitar las tramas.
Al tenerse señales reservadas
- Es fácil encontrar el inicio y fin de cada trama.
- Sin necesidad de rellenar mucho los datos (no hay problemas de longitud).
- [I] **Preámbulo:** es un patrón bien definido que se encuentra al inicio de cada trama, muy utilizado en Wi-Fi (802.11) y Ethernet (802.3). Puede ser bastante largo (72 bits para 802.11). Va seguido de un campo de conteo en la cabecera que se utiliza para localizar el final de la trama. (se mezclan los métodos)
## Control de Flujo
El control de caudal es un **[[2. Capa de Enlace de Datos#Problemas de diseño de la capa de enlace de datos|problema de diseño]]** importante que presenta la capa de enlace de datos y en capas superiores. ==Un emisor que quiere transmitir tramas más rápido de lo que un receptor puede aceptarlas== (cuando el emisor funciona en una máquina rápida y el receptor en una máquina lenta y de gama baja).
## Tipos de control de flujo

1. **Basado en retroalimentación:** El receptor devuelve el mensaje o información al emisor dándole la posibilidad de enviar más datos o el receptor le dice al emisor en qué fase está.
2. **Basado en la velocidad (tasas de bits):** Se usa un mecanismo definido que limita la velocidad a la que los remitentes pueden transmitir datos, sin usar la retroalimentación del receptor. (Capa de enlace o superiores).

- [I] **NIC (Tarjetas de interfaz de red):** funcionan a velocidad de cable, pueden gestionar tramas tan rápido como llegan al enlace.
## Control de errores
Los errores inalámbricos y los bucles locales envejecidos tienen tasas de error que son órdenes de magnitud mayores.
Las estrategias tomadas para el manejo de errores utilizan información adicional añadidas a los datos enviados.
- [i] Los errores se deben a:
	- Distorsión
	- Ruido
	- Atenuación

>[!important] Estrategias de control de errores
>Ambas se basan en **añadir la suficiente información redundante** para que el **receptor** pueda **deducir**:
>1. Lo enviado. [[Control de errores (Detección y corrección)#Códigos de corrección|Códigos de corrección]]
>2. Si se ha producido un error y solicitar una retransmisión. (no identifica dónde se produjo el error). [[Control de errores (Detección y corrección)#Códigos de detección|Códigos de detección]]
### Tipo de error
Como ningún modelo es 100% efectivo, hay que considerar el tipo de error producido.
1. Variaciones en **ruido térmico:** producen errores uniformemente distribuidos.
2. **Errores impulsivos:** producen errores de ráfaga (son más difíciles de corregir que los errores aislados).
## Códigos de corrección (FEC - Forward Error Connection)
- Se utilizan en canales ruidosos porque las retransmisiones tienen la misma probabilidad de error que la primera transmisión.
- Usos: [[2. Capa Física]], [[2. Capa de Enlace de Datos]] y capas superiores.

>[!tip] Los diferentes tipos de corrección de errores son:
>1. Código de Hamming
>2. Código convolucionales binarios
>3. Códigos Reed-Solomon
>4. Códigos de comprobación de paridad de baja densidad

- **Código de bloques:** los **r** bits de comprobación se calculan en función a los **m** bits de datos asociados.
- **Código sistemático:** los **m** bits de datos se envían a la vez que los de control, en lugar de codificarse antes de los enviados.
- **Código lineal:** los **r** bits de control se calculan como una función lineal de los **m** bits de datos.

==El más utilizado es el OR exclusivo o XOR o la **suma de módulo 2**==

- [I] **Codeword (palabra codificada):** unidad de n bits que contiene datos y bits de comprobación.
- [I] **Tasa de codificación:** es la fracción de la codificación que contiene información no redundante ($m/n$).
	- ($1/2$) en canal ruidoso
	- ($1$) en canales de alta calidad

## Códigos de detección

# Codigo de Hamming 
- [I] **Distancia de hamming:** es la cantidad mínima de bits cambiados para pasar de una palabra codificada válida a otra cualquiera. La **distancia de un código completo** es la menor de todas las distancias entre una palabra y otra.
	Un código con distancia mínima de Hamming **d** puede detectar palabras codificadas (codewords) con hasta **s** bits con error, donde $d_{min}\gt{s}$
	**Nota rápida:** Si la pregunta te pidiera solo **detectar** 1 error (saber que algo está mal pero no poder arreglarlo), la fórmula es simplemente d + 1.  Como pide **corregir**, necesitas ese espacio "extra" se representa como dmin>=2d+1 para saber cuál era la palabra original correcta.

>[!info] Funcionamiento de control de errores
>- Toma un mensaje de longitud **m**
>- Le agrega **r** bits de redundancia
>- Se obtiene una **palabra codificada** de longitud **n** $$n=m+r$$

| **Detección de errores**                                                                                                       | **Corrección de errores**                                                                                                |
| ------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------ |
| $d_{\text{min}}$ : distancia de Hamming<br>$s$ : cantidad de bits con error **máximos a detectar**<br>$$d_\text{{min}}\gt{s}$$ | $d_{\text{min}}$ : distancia de Hamming<br>$t$ : cantidad de bits con error **máximos a corregir**<br>$$d_{min}\gt{2t}$$ |
| ![[Pasted image 20260329172902.png\|250]]                                                                                      | ![[Pasted image 20260329172830.png\|300]]                                                                                |

---
## Corrección de errores de un solo bit
Se presenta en el código de Hamming
==ESTUDIAR PORQUE EL PROFE DIJO QUE PUEDE PREGUNTAR ALGO ASI==
>[!info] Se tiene **m** bits de mensaje y **r** bits de verificación y se quiere corregir todos los errores de **un bit**.
>- Mensajes posibles: $2^m$
>- Cant de codewords: $(m+r+1)$
>	Para indicar la ausencia de error (una codeword válida)
>	Para indicar la ubicación del error ($m+r$posibilidades)
>- Cant de codewords: $2^n=2^{m+r}$ palabras codificadas
>$$2^m*(m+r+1)\leq{2^{m+r}=2^m*2^r}$$
>$$(m+r+1)\leq{2^r}$$


# pasos a seguir para el codigo de hamming
1- dividir el mensaje en bloques
2- Hallar los bits de paridad de cada bloque 
3- Aplicar el codigo de hamming a cada bloque 
4- Transmitir los datos columna por columna  