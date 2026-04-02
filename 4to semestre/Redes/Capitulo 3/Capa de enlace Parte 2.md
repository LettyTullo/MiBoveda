## Codigos convulvionales 

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
