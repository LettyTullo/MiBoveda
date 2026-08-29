## MPLS

**MPLS** (siglas de _**MultiProtocol Label Switching**_ o **Conmutación Multiprotocolo de Etiquetas**) es una tecnología de red orientada a la conexión utilizada principalmente por los proveedores de servicios de Internet (ISP) para transportar y gestionar el tráfico a través de sus redes troncales.

Aunque funciona por encima de la capa de enlace y por debajo de la capa de red (conociéndose informalmente como un protocolo de **Capa 2.5**), se caracteriza por envolver los paquetes IP dentro de una cabecera de etiquetas para acelerar y flexibilizar el reenvío de datos.
# Principio de Funcionamiento: Conmutación vs. Reenvío
En el enrutamiento IP tradicional, los routers realizan un "reenvío" (_forwarding_), lo que implica buscar en una tabla la coincidencia de prefijo más largo para la dirección IP de destino de cada paquete. Este es un proceso de búsqueda que puede ser lento en redes de alta velocidad.

**MPLS introduce la conmutación (_switching_):**

- Cuando un paquete IP entra a la red MPLS, un enrutador de borde le añade una **etiqueta** en el frente.
- Los routers internos de la red MPLS ya no analizan la dirección IP de destino. En su lugar, utilizan el valor numérico de la etiqueta como un **índice directo** en una tabla interna de reenvío.
- Esto hace que encontrar la línea de salida correcta sea una búsqueda indexada extremadamente rápida y sencilla, optimizando el rendimiento del hardware.
# La Cabecera MPLS (4 bytes)
La cabecera de etiquetas se inserta justo antes de la cabecera IP. Tiene una longitud de **4 bytes** y consta de los siguientes cuatro campos:

1. **Etiqueta (20 bits):** Contiene el índice de conmutación. Estas etiquetas tienen únicamente un **significado local** entre routers adyacentes y se reasignan en cada salto para evitar conflictos de numeración.
2. **QoS (3 bits):** Indica la clase de servicio para dar prioridad al paquete (Calidad de Servicio).
3. **S (1 bit):** Bandera de apilamiento (_stacking_). Permite apilar múltiples etiquetas en un solo paquete; se pone en **1** para la etiqueta que está en el fondo de la pila y en **0** para las demás.
4. **TTL (8 bits):** Tiempo de vida (_Time to Live_). Funciona igual que en IP: se decrementa en cada router y, si llega a 0, el paquete se descarta para evitar bucles infinitos en caso de fallos de enrutamiento.
# Componentes Clave de una Red MPLS
El ecosistema MPLS define dispositivos y clasificaciones específicas para el manejo del tráfico:

- **LER (Label Edge Router / Enrutador de Etiquetas de Borde):** Se ubican en las fronteras de la red MPLS. Su función es examinar el paquete IP entrante, determinar qué ruta debe seguir, colocar la etiqueta adecuada (_push_) y, al salir de la red, retirar la etiqueta (_pop_) para entregar el paquete IP original intacto a la red destino.
- **LSR (Label Switched Router / Enrutador de Conmutación de Etiquetas):** Son los enrutadores internos de la red. Reciben el paquete etiquetado, consultan su tabla, reemplazan la etiqueta vieja por una nueva (_swap_) y reenvían el paquete por la línea de salida correspondiente.
- **FEC (Forwarding Equivalence Class / Clase de Equivalencia de Reenvío):** Es un grupo de flujos de datos que terminan en un destino común o comparten requerimientos de servicio. En lugar de asignar etiquetas individuales a cada conexión, el router agrupa múltiples flujos bajo una **misma etiqueta de FEC**, lo que permite una agregación eficiente del tráfico.
# Apilamiento de Etiquetas (_Label Stacking_)
Gracias al bit **S**, MPLS puede operar a varios niveles de jerarquía de forma simultánea. Si varios flujos que ya tienen etiquetas individuales deben seguir una ruta común a través de la red, el router puede agregar una **segunda etiqueta exterior** en el paquete (creando una pila). El tráfico viaja guiado por la etiqueta externa y, al finalizar el trayecto común, esta etiqueta es retirada, revelando la etiqueta interna original para que continúe su curso individual.

>[!tip] Ventajas y Aplicaciones de MPLS
>- **Independencia de protocolo (Multiprotocolo):** Al no formar parte de la capa de enlace ni de red, MPLS es independiente de ambas. Puede transportar tanto paquetes IP como no IP, o mover paquetes IP sobre infraestructuras físicas que no utilicen direccionamiento IP.
>- **Ingeniería de Tráfico:** Permite a los operadores calcular y establecer de antemano rutas específicas que eviten los enlaces congestionados, aprovechando la capacidad ociosa de la red de forma dinámica (algo que el enrutamiento IP tradicional por ruta más corta no puede hacer de forma eficiente).
>- **Redes Privadas Virtuales (VPN MPLS):** Los ISP utilizan MPLS para configurar túneles seguros para empresas. Al etiquetar y separar el tráfico de cada cliente, se garantiza el aislamiento de datos y se pueden ofrecer acuerdos de nivel de servicio (SLA) con ancho de banda y QoS garantizados a través de la red pública.

