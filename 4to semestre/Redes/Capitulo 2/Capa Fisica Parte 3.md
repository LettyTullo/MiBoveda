## Digitalizacion de las seañales de voz
La **digitalización de las señales de voz** es el proceso de convertir datos analógicos en datos digitales para su transporte a través de redes modernas
# Codecs  
El término **códec** es la abreviatura de **"codificador-decodificador"**. Se trata de un dispositivo o proceso que realiza la conversión entre señales analógicas y bits digitales.
# La técnica PCM
La técnica estándar utilizada en el corazón del sistema telefónico y en los **codecs** (codificadores-decodificadores) es la denominada **PCM**. Este proceso consta de los siguientes pasos técnicos:
- **Muestreo:** Basándose en el **Teorema de Nyquist**, una señal se puede reconstruir totalmente si se muestrea a una frecuencia mayor al doble de su componente más alta. Para un canal de voz de 4,000 Hz, se toman **8,000 muestras por segundo** (una cada 125 μseg).
- **Cuantización:** Cada muestra de la amplitud de la señal (muestra PAM) se asigna a un valor digital, normalmente un número de **8 bits**.
- **Tasa de bits:** Al multiplicar las 8,000 muestras por los 8 bits de cada una, se obtiene la velocidad estándar de **64 kbps** para una llamada de voz sin comprimir.
# La Transportadora T1
La **portadora T1** (también denominada DS1) es una especificación técnica utilizada para transmitir múltiples canales multiplexados a través de un único circuito físico.
- **Capacidad y Uso:** Es el estándar utilizado principalmente en **Norteamérica y Japón**, con una tasa de bits total de **1.544 Mbps**.
- **Estructura de la Trama:** Una trama T1 se emite cada 125 μseg y consta de **193 bits** distribuidos así:
    - **24 canales de voz:** Cada canal aporta una muestra de 8 bits (24×8=192 bits).
    - **1 bit de encuadre:** Se añade un bit extra para sincronización, alcanzando los 193 bits totales por trama.
- **Señalización:** Las versiones antiguas utilizaban "señalización de bits robados", donde se usaba el bit menos significativo de cada muestra para control. Las versiones modernas ofrecen **canales claros** donde los 8 bits completos de cada canal están disponibles para datos, moviendo la señalización a un canal común separado.
Varias líneas T1 pueden combinarse mediante multiplexación por división en el tiempo (TDM). Esta jerarquía, conocida como el sistema de **Portadoras T** (T-Carrier), es un método para escalar la capacidad de transmisión combinando flujos de datos de menor velocidad en canales más potentes mediante la **multiplexación por división en el tiempo (TDM)**,.
### 1. El proceso de Multiplexación Bit a Bit
A diferencia de la trama T1 original, que combina 24 canales de voz organizándolos **byte por byte** (muestras de 8 bits), la multiplexación hacia niveles superiores (T2, T3 y T4) se realiza **bit a bit**,. Esto significa que el multiplexor toma un bit de cada flujo entrante de forma rotatoria para construir el nuevo flujo de alta velocidad.
### 2. ¿Por qué los números no parecen "sumar" exactos?
Si multiplicamos la velocidad de una T1 por cuatro ($1.544 \text{ Mbps} \times 4$), obtendríamos **6.176 Mbps**. Sin embargo, la velocidad de una **T2 es de 6.312 Mbps**.
Esta diferencia de velocidad (los bits "extra") se utiliza para **sobrecarga (overhead)**, específicamente para:
- **Encuadre (Framing):** Bits necesarios para que el receptor sepa dónde empieza y termina cada bloque de datos.
- **Sincronización y Recuperación:** Bits que permiten al sistema recuperarse rápidamente si la señal de la portadora se interrumpe o se pierde la sincronía entre el emisor y el receptor,.
### 3. La Escalera Jerárquica (Estándar de EE. UU. y Japón)
La progresión funciona de la siguiente manera,:
- **Nivel T2:** Combina **4 flujos T1** para alcanzar **6.312 Mbps**. Se utiliza principalmente de forma interna dentro de los sistemas de las compañías telefónicas,.
- **Nivel T3:** Combina **7 flujos T2** para alcanzar **44.736 Mbps**. Este nivel es muy común y es ampliamente alquilado por clientes corporativos que requieren gran ancho de banda.
- **Nivel T4:** Combina **6 flujos T3** para alcanzar **274.176 Mbps**. Se utiliza casi exclusivamente en el núcleo o _backbone_ del sistema telefónico y no es tan conocido por el público general.
# SONET/SDH
El multiplexado de las redes ópticas **SONET** (_Synchronous Optical Network_, estándar ANSI utilizado en EE. UU.) y **SDH** (_Synchronous Digital Hierarchy_, estándar de la UIT para el resto del mundo) es un método de transporte de datos en la capa física diseñado para unificar sistemas digitales incompatibles y permitir la transmisión a velocidades de gigabits por segundo.
### 1. Naturaleza Síncrona y Multiplexado TDM
A diferencia de otros sistemas, SONET/SDH utiliza un esquema de **Multiplexación por División en el Tiempo (TDM)** de naturaleza estrictamente **síncrona**.
- **Reloj Maestro:** Todos los emisores y receptores están vinculados a un reloj común con una precisión extrema (una parte en $10^9$).
- **Emisión Constante:** Los bits se envían a intervalos precisos y las tramas se emiten cada **125 $\mu$seg**, coincidiendo con la frecuencia de muestreo de los canales de voz PCM, incluso si no hay datos útiles que enviar (en cuyo caso se envían datos ficticios).
### 2. La Jerarquía de Tasas de Datos
SONET define una jerarquía basada en niveles de señal de transporte síncrono (STS) y portadoras ópticas (OC):
- **Unidad Básica (STS-1 / OC-1):** Tiene una velocidad de **51.84 Mbps**.
- **Señales STS-N:** Varias señales STS-1 se combinan para formar flujos de mayor velocidad. Por ejemplo, un multiplexor 3:1 puede combinar tres tributarios STS-1 de entrada en un único flujo **STS-3** de salida realizando el multiplexado **byte a byte**.
- **Escalabilidad:** Las velocidades comunes incluyen OC-3 (155.52 Mbps), OC-12 (622.08 Mbps), OC-48 (2.488 Gbps) y OC-192 (9.95 Gbps), llegando hasta OC-768 (casi 40 Gbps).
### 3. Estructura de la Trama SONET
La trama básica (STS-1) se organiza como un bloque de **810 bytes** dispuestos en un rectángulo de **90 columnas por 9 filas**.
- **Sobrecarga (Overhead):** Las primeras 3 columnas de cada fila se reservan para información de control (Sección y Línea).
- **SPE (Synchronous Payload Envelope):** Es el contenedor donde viajan los datos del usuario. Una característica inusual es que el SPE **no tiene una posición fija**; puede comenzar en cualquier lugar de la trama e incluso abarcar dos tramas consecutivas.
- **Punteros:** Se utilizan punteros en la sección de sobrecarga para indicar exactamente dónde comienza el primer byte del SPE, lo que otorga una gran flexibilidad al sistema para insertar datos en tiempo real sin esperar al inicio de una nueva trama física.

