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
    - **Diagrama de Trellis:** Es una representación temporal que permite observar la evolución de los estados a medida que se procesa la cadena de bits.
5. **Decodificación (Algoritmo de Viterbi):** Para recuperar el mensaje original en canales con ruido, se utiliza el **Algoritmo de Viterbi**. Este algoritmo busca la secuencia de estados con la **mayor probabilidad** de haber generado la secuencia de salida observada, manteniendo en cada paso el camino con el menor número de errores acumulados.

Se utilizan ampliamente en estándares como **GSM (telefonía móvil)**, comunicaciones por satélite y **IEEE 802.11 (Wi-Fi)**.