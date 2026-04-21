La **Redundancia Cíclica (CRC)**, también conocida como **código polinómico**, es una técnica de detección de errores sumamente potente y ampliamente utilizada en la capa de enlace de datos. Su funcionamiento se basa en tratar las cadenas de bits como polinomios con coeficientes de únicamente "0" y "1".
1. Fundamento Teórico
- **Representación Polinómica:** Una trama de k bits se considera un polinomio de grado k−1. Por ejemplo, la cadena `110001` representa el polinomio x5+x4+1.
- **Aritmética Módulo 2:** Todos los cálculos se realizan en base 2 sin acarreos en la suma ni préstamos en la resta, lo que hace que ambas operaciones sean idénticas al **XOR (OR exclusivo)**
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
Analisis de G(x)