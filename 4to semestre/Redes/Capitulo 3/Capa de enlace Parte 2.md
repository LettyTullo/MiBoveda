## Codigos convulcionales 
Los **códigos convolucionales** son un método de corrección de errores que, a diferencia de los códigos de bloques, opera sobre una **cadena continua de bits** procesando una secuencia de entrada para generar una de salida.
A continuación se explica su funcionamiento paso a paso:
1. **Mantenimiento del estado (Memoria):** El codificador utiliza **registros de desplazamiento** para mantener un estado interno. Esto implica que la salida es una **función de los bits de entrada actuales y anteriores**, dotando al sistema de "memoria".
2. **Proceso de Codificación:** Cada vez que un bit entra al codificador, los valores de los registros se desplazan (normalmente a la derecha). Los bits de salida se generan realizando operaciones lineales, específicamente sumas **XOR (OR exclusivo)**, entre el bit de entrada y los bits almacenados en el estado interno.
3. **Definición de parámetros:** Estos códigos se especifican mediante dos valores:
    - **Tasa de código ($R = m/n$):** Es la relación entre la cantidad de bits que entran ($m$) y los que salen ($n$). Por ejemplo, en una tasa de 1/2, cada bit de entrada genera dos bits de salida.
    - **Longitud de restricción ($K$):** Es el número de bits de los que depende la salida; indica cuántos desplazamientos se requieren para que un bit de entrada deje de afectar a la salida.
4. **Representación lógica:** El funcionamiento se puede visualizar de tres formas:
    - **Tabla de transiciones:** Indica el estado siguiente y la salida según la entrada.
    - **Diagrama de estados:** Muestra gráficamente las transiciones entre los estados internos.
    - **Diagrama de Trellis:** Es una representación temporal que permite observar la evolución de los estados a medida que se procesa la cadena de bits
5. **Codificacion:** 
	- **Ingreso:** Un bit de información entra al codificador.
	- **Cálculo:** El sistema toma este nuevo bit y lo suma (usando compuertas lógicas XOR) junto con los bits anteriores que están almacenados temporalmente en su memoria (registros de desplazamiento).
	- **Salida:** Se generan múltiples bits de salida basados en las conexiones predefinidas del circuito.
	- **Desplazamiento:** El sistema hace un "tic" de reloj: descarta el bit más antiguo de su memoria, mueve los demás una posición y guarda el bit que acaba de entrar para usarlo en el siguiente cálculo.
6. **Decodificación (Algoritmo de Viterbi):** Para recuperar el mensaje original en canales con ruido, se utiliza el **Algoritmo de Viterbi**. Este algoritmo busca la secuencia de estados con la **mayor probabilidad** de haber generado la secuencia de salida observada, manteniendo en cada paso el camino con el menor número de errores acumulados.

Se utilizan ampliamente en estándares como **GSM (telefonía móvil)**, comunicaciones por satélite y **IEEE 802.11 (Wi-Fi)**.
## Algoritmo de Viterbi
El **algoritmo de Viterbi** es el método estándar utilizado para la **decodificación de códigos convolucionales**, específicamente diseñado para procesar señales en canales de comunicación que presentan ruido. Su función principal es determinar la secuencia de bits original con la mayor precisión posible a partir de una señal recibida que puede contener errores.
### Proceso de Funcionamiento
1. **Entrada:** Recibe el mensaje que ha sido codificado y posteriormente afectado por el ruido del canal.
2. **Análisis de Trayectorias:** El algoritmo recorre la secuencia observada paso a paso. Para cada paso y para cada posible **estado interno** del sistema, calcula qué secuencia de entrada habría producido la observación actual con el **menor número de errores acumulados**.
3. **Uso del Trellis:** Este proceso se visualiza mediante un **diagrama de Trellis**, donde se mantienen los caminos que tienen la **mayor probabilidad** de ser los correctos según la lógica del código convolucional utilizado.
4. **Selección del Mensaje:** Al finalizar el recorrido de toda la secuencia, el camino que presenta el menor número total de errores es seleccionado como el **mensaje más probable**.
### Decodificación de Decisión Suave (Soft Decision)
Una de las grandes ventajas de este algoritmo es su capacidad para implementar la **decodificación de decisión suave**. En lugar de decidir inmediatamente si un bit es "0" o "1" basándose en un umbral fijo, el algoritmo puede trabajar con la **incertidumbre** o probabilidad de la señal (por ejemplo, interpretando un voltaje intermedio como "muy probablemente un 1"). Esto permite una corrección de errores mucho más robusta que la decisión "dura" tradicional.
### Otras Aplicaciones
Aunque su uso principal es en redes (como en el estándar **IEEE 802.11**), el algoritmo de Viterbi tiene aplicaciones importantes en otros campos tecnológicos, tales como:
- **Reconocimiento y sintetización de voz**.
- **Bioinformática**.
- Comunicaciones satelitales y telefonía móvil GSM.
# Otros codigos de correccion
**Códigos Reed-Solomon**
Son códigos de bloques lineales que, a diferencia de los anteriores, no trabajan con bits individuales sino con **símbolos de múltiples bits** (como bytes).
- **Fortaleza:** Son excelentes para corregir **errores de ráfaga** (donde se dañan muchos bits consecutivos), ya que un error en una ráfaga de bits se trata simplemente como un error en un símbolo.
- **Aplicaciones:** Se utilizan masivamente en tecnologías como **DSL**, redes de cable, comunicaciones por satélite y en el almacenamiento físico como **CD, DVD y discos Blu-ray**.
**Códigos LDPC (Low-Density Parity Check)**
Estos códigos de comprobación de paridad de baja densidad se basan en una representación matricial donde cada bit de salida depende de solo una pequeña fracción de los bits de entrada.
- **Ventajas:** Son muy prácticos para tamaños de bloque grandes y ofrecen una capacidad de corrección que **supera a casi todos los demás códigos** en la práctica.
- **Aplicaciones modernas:** Se han vuelto fundamentales en estándares de alta velocidad como el **Ethernet de 10 Gbps**, redes eléctricas inteligentes y las versiones más recientes de **Wi-Fi (802.11)**
## Bits de paridad (Deteccion de errores)
La **detección de errores mediante bits de paridad** es uno de los métodos más sencillos y antiguos para asegurar la integridad de los datos transmitidos. Consiste en añadir un **solo bit de redundancia** al final de un mensaje para que la suma total de los bits con valor "1" sea par o impar.
A continuación se detalla su funcionamiento según las fuentes:
1. Tipos de Paridad
- **Paridad Par:** El bit de paridad se elige de tal manera que el número total de unos en el bloque (datos + paridad) sea un número par. Matemáticamente, esto equivale a realizar una operación **XOR** entre todos los bits de datos.
- **Paridad Impar:** El bit se selecciona para que el conteo total de unos sea un número impar
1. Ejemplo práctico
Si el mensaje original es `1110000` (tiene tres unos):
- En **paridad par**, se añade un `1` para que el total sea cuatro: `11100001`.
- En **paridad impar**, se añadiría un `0` para mantener el total en tres.
1. Capacidades y Limitaciones
- **Distancia de Hamming:** Este esquema tiene una distancia de Hamming de **2**, lo que permite detectar errores simples de un solo bit.
- **Detección selectiva:** Solo es capaz de detectar errores que afecten a un **número impar de bits** (1, 3, 5, etc.).
- **Fallo ante errores pares:** Si se producen dos errores (dos bits se invierten simultáneamente), la paridad seguirá pareciendo correcta y el receptor **no detectará el error**.
- **Ubicación del error:** No permite saber qué bit falló, solo indica que la trama está dañada.
1. Mejoras: Paridad en dos dimensiones (Intercalación)
Para combatir las **ráfagas de errores** (donde fallan muchos bits consecutivos), se utiliza la **intercalación**. En este método:
- Los datos se organizan en una matriz rectangular de N columnas.
- Se calcula un bit de paridad para cada fila y para cada columna.
- Esta técnica permite detectar ráfagas de error de longitud hasta N, ya que los bits erróneos se distribuyen entre diferentes columnas, afectando a múltiples sumas de paridad.
## Checksums o sumas de comprobacion 

Un **checksum** (o suma de comprobación) es un valor corto de redundancia que se añade a una unidad de datos para permitir que el receptor detecte errores ocurridos durante la transmisión.
### 1. ¿Cómo funciona?
El algoritmo trata los datos como una secuencia de palabras de una longitud fija $N$ (por ejemplo, 8 bits).
- **En el emisor:** Se realiza una suma de todas estas palabras de datos. En el caso del **checksum de Internet** (usado en IP, UDP y TCP), se utiliza aritmética de **complemento a uno**, donde los acarreos del final se vuelven a sumar al resultado. El valor resultante se suele colocar al final del mensaje.
- **En el receptor:** Se vuelve a calcular la suma de los datos recibidos. Si el resultado no coincide con el checksum enviado (o si la suma total incluyendo el checksum no da un valor específico, como cero), se determina que la trama contiene un error y se descarta.
### 2. Propiedades y capacidades
- **Detección de errores:** Es más eficaz que un simple bit de paridad, ya que puede detectar errores que no alteran la paridad total, como cuando bits en diferentes palabras cambian de forma que se cancelan en un conteo de paridad pero no en una suma.
- **Ráfagas de error:** Tiene la capacidad de detectar ráfagas de error de hasta $N$ bits.
- **Simplicidad:** Su principal ventaja es que es **fácil y eficiente de implementar en software**
### 3. Limitaciones
A pesar de su utilidad, el checksum tiene debilidades frente a ciertos fallos:
- **Errores sistemáticos:** Es vulnerable a la eliminación o adición de ceros y al intercambio de posición de las partes del mensaje, ya que la suma total no cambiaría.
- **Protección débil:** No protege bien contra el "empalme" de mensajes donde se combinan partes de dos paquetes diferentes.
## Redundancia ciclica (CRC)
La **Redundancia Cíclica (CRC)**, también conocida como **código polinómico**, es una técnica de detección de errores sumamente potente y ampliamente utilizada en la capa de enlace de datos. Su funcionamiento se basa en tratar las cadenas de bits como polinomios con coeficientes de únicamente "0" y "1".
1. Fundamento Teórico
- **Representación Polinómica:** Una trama de k bits se considera un polinomio de grado k−1. Por ejemplo, la cadena `110001` representa el polinomio x5+x4+1.
- **Aritmética Módulo 2:** Todos los cálculos se realizan en base 2 sin acarreos en la suma ni préstamos en la resta, lo que hace que ambas operaciones sean idénticas al **XOR (OR exclusivo)**.
- **Polinomio Generador (**G(x)**):** El emisor y el receptor deben acordar de antemano un polinomio divisor llamado generador, cuyos bits de mayor y menor orden deben ser 1.
1. Proceso de Cálculo (Paso a Paso)
**En el Emisor:**
2. Se determina el grado r del polinomio generador G(x).
3. Se añaden r bits en cero al final de la trama original.
4. Se divide esta nueva cadena entre G(x) utilizando la división larga módulo 2.
5. El **residuo** de esta división (que tiene r o menos bits) se resta de la trama con ceros, lo que equivale a reemplazar esos ceros por el residuo. El resultado es la trama transmitida, la cual ahora es **exactamente divisible** por G(x).
**En el Receptor:**
6. Se recibe la trama y se divide nuevamente por el mismo polinomio G(x).
7. Si el residuo es **cero**, se asume que no hubo errores de transmisión. Si el residuo es distinto de cero, la trama se rechaza por estar dañada