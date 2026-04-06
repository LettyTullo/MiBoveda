MODELO OSI, ITU-T
# SONET/SDH, PCM, T1
**SONET** (_Synchronous Optical Network_) y **SDH** (_Synchronous Digital Hierarchy_) son los protocolos de **capa física** más utilizados en los enlaces de fibra óptica de área extensa (WAN) que constituyen la red troncal de las redes de comunicaciones modernas

- **Distorcion por atenuacion:** Debido a que la atenuacion es diferente a distintas frecuencias, mayormente en señales analogicas
- **Distorcion por retardo:** La velocidad de propagacion varia con la frecuencia y es critico para señales digitales.

1. Código de Hamming
2. Código convolucionales binarios
3. Códigos Reed-Solomon
4. Códigos de comprobación de paridad de baja densidad

## Tecnicas de multiplexado y modulacion, incluyendo concepto

Los **LED** y los **ILD** son los dos tipos de fuentes de luz utilizados en los sistemas de transmisión por fibra óptica para convertir impulsos eléctricos en señales luminosas.

A continuación se explica qué es cada uno y sus diferencias principales según las fuentes:
# 1. LED (Diodo Emisor de Luz)
Es una fuente de luz que emite **luz incoherente**. Es la opción más sencilla y económica para transmisiones de datos.

- **Uso:** Se utiliza principalmente con **fibra multimodo**.
- **Ventajas:** Tiene una **larga vida útil**, es poco sensible a los cambios de temperatura y su costo es bajo.
- **Limitaciones:** Ofrece una **tasa de datos baja** y solo es adecuado para distancias cortas.
# 2. ILD (Diodo Láser de Inyección)
En las fuentes se le denomina comúnmente como **láser semiconductor**. A diferencia del LED, este emite **luz coherente**.

- **Uso:** Puede utilizarse tanto con fibra multimodo como con **fibra monomodo**.
- **Ventajas:** Permite una **tasa de datos alta** y es capaz de transmitir a **largas distancias**.
- **Limitaciones:** Es un dispositivo **caro**, tiene una vida útil más corta que el LED y presenta una **sensibilidad sustancial a la temperatura**, lo que suele requerir mecanismos de control más complejos
![[Pasted image 20260405212242.png]]
# **B8ZS** son las siglas de **Bipolar with 8-Zero Substitution** (Bipolar con Sustitución de 8 Ceros).
A continuación se explica su concepto y funcionamiento según las fuentes:
Concepto
Es una técnica de **scrambling** (mezclado) utilizada en la capa física para el **mejoramiento del clocking** o sincronización entre el emisor y el receptor. Su objetivo principal es resolver el problema de las **cadenas largas de ceros**, las cuales pueden causar que el receptor pierda el sincronismo al no haber transiciones en la señal
# **Bipolar-AMI** (_Alternate Mark Inversion_ o Inversión de Marca Alterna)
# NO RETURN TO ZERO LEVEL - INVERTED