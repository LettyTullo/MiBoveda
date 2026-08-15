## Control de congestion vs control de flujo
# Control de congestión (Congestion Control)

El control de congestión es un **problema global** que involucra a toda la red o a una parte de ella.

- **Objetivo:** Asegurar que la red sea capaz de transportar el tráfico ofrecido por todos los usuarios. Afecta el comportamiento de **todos los hosts y enrutadores**.
- **Causa:** Ocurre cuando se ofrece demasiado tráfico a la red, superando su capacidad de transporte interna. Esto provoca que las colas en los routers se llenen, aumente la latencia y se pierdan paquetes.
- **Mecanismo común:** Requiere la cooperación de las capas de red y transporte. Se utilizan técnicas como el **control de admisión**, **notificación explícita de congestión (ECN)**, y la gestión de la **ventana de congestión (cwnd)** en protocolos como TCP para reducir la carga en la red.
- **Ejemplo:** 1,000 computadoras conectadas a líneas de 1 Mbps. Si 500 de ellas intentan transferir archivos a las otras 500 simultáneamente, el tráfico total superará con creces lo que la red puede soportar, independientemente de la velocidad de los receptores.
# Control de flujo (Flow Control)

El control de flujo se refiere específicamente al tráfico entre un **emisor y un receptor concreto**.

- **Objetivo:** Garantizar que un emisor rápido no transmita datos más rápido de lo que un receptor lento puede absorberlos.
- **Causa:** Se debe a una limitación de recursos (como el espacio de búfer) en el **extremo receptor**, no en la red.
- **Mecanismo común:** Se implementa típicamente en la capa de transporte mediante protocolos de **ventana deslizante**, donde el receptor informa al emisor cuánto espacio libre queda en su búfer (ventana de recepción).
- **Ejemplo:** Una supercomputadora enviando un archivo de gran tamaño a una PC a través de una red de 100 Gbps. La red es capaz de manejar el tráfico, pero la PC (el receptor) solo puede procesar 1 Gbps, por lo que el emisor debe detenerse para dejarla "respirar".
