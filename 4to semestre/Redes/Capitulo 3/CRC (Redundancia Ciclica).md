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
## Analisis de G(x) - Errores de transmision 
Si ocurre un error durante la transmisión, el receptor no recibe T(x), sino una señal distorsionada que se representa como T(x)+E(x), donde E(x) **es el polinomio de error**. En este polinomio, cada bit 1 corresponde a un bit que ha sido invertido durante la transmisión.
Todos los errores de E(x) que sean divisibles entre G(x) no seran detectados.
# Análisis según el tipo de error
La capacidad del CRC para detectar fallos depende de las propiedades de G(x):

- **Errores de un solo bit:** Si G(x) tiene más de un término (lo cual incluye siempre el término xn y el término x0), nunca podrá dividir a un error de tipo E(x)=xi, por lo que **detectará todos los errores de un solo bit**.
![[Pasted image 20260421130530.png]]


- **Errores de dos bits:** Para detectar todos los errores dobles aislados, G(x) no debe dividir a xt+1 para ningún valor de t hasta la longitud máxima de la trama.
![[Pasted image 20260421130836.png|697]]

- **Número impar de bits con error:** Si se incluye (x+1) **como factor de** G(x), el código podrá detectar todos los errores que afecten a un número impar de bits. Si al dividir G(x) entre x+1 el resto es cero, significa que todos los errores con numero impar de bits seran detectados.
- **Errores de ráfaga (Burst errors):**
    - Una ráfaga de error de **longitud** L≤r (donde r es el grado del polinomio generador) será **detectada siempre**, siempre que G(x) incluya un término independiente x0.
    - Si la longitud de la ráfaga es exactamente r+1, la probabilidad de que el error no se detecte es de 1/2r−1.
    - Para ráfagas más largas de r+1 bits, la probabilidad de que el error pase desapercibido es de 1/2r
![[Pasted image 20260421131114.png]]


[[Ventanas corredizas]]