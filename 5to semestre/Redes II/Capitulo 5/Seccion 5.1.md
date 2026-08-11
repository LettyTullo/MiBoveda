>[!amarillo] Funciones principales de la capa de red
>- **Enrutamiento de paquetes:** Su función fundamental es determinar la ruta que deben seguir los paquetes desde la máquina de origen hasta la de destino. Para ello, debe conocer la **topología de la red** (enrutadores y enlaces) y calcular las rutas óptimas.
>- **Interconexión de redes (Internetworking):** Permite la comunicación entre dispositivos situados en redes o subredes autónomas distintas, lidiando con la heterogeneidad de las tecnologías de red.
>- **Gestión del tráfico y congestión:** Debe evitar sobrecargar ciertas líneas o enrutadores mientras otros permanecen ociosos. Cuando hay demasiados paquetes en una parte de la red, se produce congestión, y la capa de red debe aplicar mecanismos para mitigarla.
>- **Garantía de Calidad de Servicio (QoS):** Gestiona parámetros como el ancho de banda, el retardo, la fluctuación (jitter) y la pérdida de paquetes para satisfacer los requisitos de las aplicaciones.
>- **Direccionamiento uniforme:** Proporciona a la capa de transporte un plan de numeración uniforme para identificar dispositivos, incluso si se atraviesan diferentes tipos de redes (LAN o WAN)

# Aspectos de diseño 

1. **Conmutación de paquetes de almacenamiento y reenvío (Store-and-Forward):** Un host envía un paquete al router más cercano, el cual lo almacena hasta recibirlo por completo y verificar su suma de comprobación (control de errores); solo entonces lo reenvía al siguiente salto.

![[Screenshot 2026-08-10 203803.png|470]]


>[!danger] Explicacion
> **Hosts (H1 y H2) y Procesos (P1 y P2):** El origen y el destino de la comunicación. El proceso P1 en el Host H1 fragmenta la información en bloques independientes llamados **paquetes** para enviarlos al proceso P2 en el Host H2.
> - **Equipos del ISP (_ISP's equipment_):** Es la subred del proveedor de servicios de Internet, compuesta por los routers internos (A, B, C, D y E) interconectados por enlaces de comunicación.
> - **Router de acceso (F) y LAN:** El router F actúa como la pasarela local que entrega los paquetes a la red de área local (LAN) donde reside H2.

2. **Servicios proporcionados a la capa de transporte**:

>[!Rosado]- Servicio sin Conexión (Red de Datagramas)
En este modelo, inspirado en el sistema de telegramas, cada paquete se inyecta en la red de forma individual
>- **Funcionamiento:** Cada paquete (datagrama) lleva la **dirección de destino completa**.
>- **Enrutamiento:** Los routers deciden de forma independiente el camino para cada paquete, lo que significa que paquetes de un mismo mensaje podrían seguir rutas distintas y llegar en desorden.
>- **Configuración:** No se requiere ninguna configuración previa antes de enviar los datos.
>- **Ejemplo:** El protocolo **IP**, que es la base de Internet, funciona bajo este esquema.

![[Screenshot 2026-08-10 205108.png|558]]


3. Servicio Orientado a la Conexión (Red de Circuitos Virtuales - CV)

Este modelo imita el funcionamiento del sistema telefónico tradicional.

- **Funcionamiento:** Antes de enviar datos, se establece una ruta fija desde el router de origen hasta el de destino, denominada **Circuito Virtual (CV)**.
- **Identificadores:** Los paquetes no llevan la dirección completa, sino un **identificador o etiqueta corta** que indica a qué CV pertenecen.
- **Enrutamiento:** La decisión de ruta se toma una sola vez al establecer la conexión; todos los paquetes subsiguientes siguen esa misma trayectoria.
- **Ejemplo:** Tecnologías como ATM, Frame Relay y **MPLS** utilizan este enfoque
2. **Implementación del enrutamiento:** El algoritmo de enrutamiento decide por qué línea de salida enviar un paquete entrante. Los algoritmos pueden ser **estáticos** (calculados de antemano) o **adaptativos** (se ajustan a cambios en la topología y el tráfico).
3. **Internetworking y fragmentación:** Al conectar redes heterogéneas, surgen diferencias en los tamaños máximos de paquetes permitidos. Si un paquete es demasiado grande para la siguiente red, la capa de red debe **fragmentarlo** en unidades menores y luego reensamblarlas en el destino.
4. **Control de congestión y admisión:** El diseño debe incluir estrategias como el **aprovisionamiento de red** (diseño robusto), el **control de admisión** (rechazar nuevo tráfico si la red está llena) y la **regulación del tráfico** (pedir a los hosts que disminuyan la velocidad).
5. **Software-Defined Networking (SDN):** Una tendencia moderna de diseño es la separación del **plano de control** (el software que decide las rutas) del **plano de datos** (el hardware que reenvía los paquetes), permitiendo una gestión de red más flexible y programable