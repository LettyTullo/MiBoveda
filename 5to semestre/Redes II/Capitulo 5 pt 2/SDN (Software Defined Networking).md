Las redes definidas por software (**SDN**) representan un cambio radical en la arquitectura de red al desacoplar el control lógico del hardware físico.

>[!tip] El problema
>Tradicionalmente, el software que ejecuta los algoritmos de enrutamiento (como OSPF o BGP) ha estado **integrado verticalmente** con el hardware propietario de los routers. Esto impedía a los operadores modificar el comportamiento de los protocolos o innovar de forma ágil. Además, la ingeniería de tráfico se realizaba de manera indirecta y manual mediante la alteración de parámetros toscos de configuración que interactuaban de forma impredecible.

>[!success] La idea central de SDN es separar físicamente la red en dos componentes independientes:
>- **Plano de control:** El software y la lógica que selecciona las rutas y decide el reenvío de tráfico.
>- **Plano de datos:** La tecnología basada en hardware encargada de realizar búsquedas rápidas en los paquetes y ejecutar el reenvío.

# El plano de control centralizado

En SDN, la lógica de control se ejecuta fuera de los routers en un **programa lógicamente centralizado** llamado **controlador**, escrito en lenguajes de alto nivel como Python, Java, Golang o C. El controlador calcula las rutas globalmente e inyecta de forma remota las tablas de reenvío en los dispositivos de red.
- **Antecedentes:** Plataformas como **RCP** (2003) utilizaron BGP de forma centralizada para control de tráfico, y **Ethane** (2007) centralizó la autenticación de hosts, aunque requería conmutadores personalizados.
- **OpenFlow (2008):** Fue la primera interfaz estándar para comunicar al controlador con los switches. Expone una **tabla de coincidencia-acción** basada en TCAM. OpenFlow 1.0 utilizaba una única tabla, mientras que OpenFlow 1.3 añadió cadenas de tablas y operaciones complejas. Aunque abrió el mercado, hoy se considera una interfaz limitada.
- **Proyectos como CORD:** Llevan este control programable a redes celulares 5G utilizando hardware básico desagregado y software de código abierto.
# El plano de datos programable
Los enfoques modernos permiten programar el hardware físico de manera independiente de los protocolos estándar.
- **Arquitectura RMT (Reconfigurable Match Tables):** Es una canalización de procesamiento de paquetes inspirada en RISC. Consta de un **analizador programable** (_parser_) para formatos de cabeceras personalizados, **etapas de coincidencia con estado** para cálculos aritméticos y decisiones rápidas de descarte, y un **deparser** que vuelve a escribir los valores modificados en el paquete.
- **Lenguaje P4:** Lenguaje de alto nivel utilizado para programar estos procesadores de paquetes independientes del protocolo (como el chip Barefoot Tofino).
- **Procesamiento Ingress/Egress:** Permite analizar los tiempos de espera en las colas individuales, aplicar encapsulaciones personalizadas y gestionar activamente colas de salida (como RED) empleando metadatos.
- **Open vSwitch (OVS):** Es una implementación de software switch de código abierto que opera en el kernel de Linux, clave para conectar máquinas virtuales en los hipervisores de los centros de datos.

### 5. Telemetría de red programable

Se considera una de las aplicaciones estrella de SDN. Supera las estadísticas agregadas tradicionales de IPFIX y el alto costo de almacenamiento de la captura masiva de paquetes:

- **INT (In-band Network Telemetry):** Permite que los paquetes de usuario recolecten y transporten información del estado de la red (como la latencia exacta experimentada en cada salto) a lo largo de su ruta.
- **Consultas MapReduce:** Permite a los operadores programar y particionar consultas de tráfico de alto nivel directamente en el hardware y software de conmutación.
- **Tráfico cifrado:** Ante la creciente encriptación en Internet, la telemetría programable ayuda a deducir de manera indirecta parámetros como la calidad de video analizando estadísticas físicas como el tamaño y tiempo entre llegadas de paquetes.

### 6. Beneficios de SDN

- **Innovación veloz:** El desarrollo se produce a la velocidad del software en lugar de depender de actualizaciones de hardware.
- **Reducción de costos:** Permite simplificar y abaratar el equipamiento físico utilizando conmutadores más genéricos.
- **Algoritmos avanzados:** Facilita el uso de algoritmos de control de enrutamiento más complejos y exigentes computacionalmente.
- **Lanzamiento rápido:** Reduce drásticamente los ciclos de lanzamiento de nuevos servicios de red.

🧩 Si lo deseas, puedo diseñar un cuestionario de práctica interactivo en un archivo descargable para que pongas a prueba tus conocimientos sobre SDN y arquitecturas de red antes de tu examen.