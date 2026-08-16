La **Calidad de Servicio (QoS)** se define como un conjunto de mecanismos y técnicas diseñados para proporcionar garantías de rendimiento superiores al modelo de "mejor esfuerzo" (_best effort_) convencional de Internet,. Su objetivo principal es satisfacer los requisitos específicos de diferentes aplicaciones para asegurar una experiencia de usuario óptima,.
# Parámetros clave de la QoS
Las necesidades de cada **flujo** (tráfico de un origen a un destino específico) se miden mediante cuatro parámetros fundamentales,:
- **Ancho de banda:** La capacidad de transmisión necesaria para el flujo,.
- **Retardo (Latencia):** El tiempo que tardan los paquetes en atravesar la red desde el origen al destino,.
- **Variación del retardo (****Jitter****):** La desviación estándar en los tiempos de llegada de los paquetes; es crítica para aplicaciones de audio y video,.
- **Pérdida de paquetes:** La tasa de paquetes que no llegan a su destino, lo cual puede degradar severamente el rendimiento,.
# Aspectos para su implementación
Para garantizar la calidad de servicio, la red debe abordar cuatro retos técnicos,:
1. **Requerimientos de la aplicación:** Identificar qué necesita cada tipo de tráfico (ej. la telefonía es sensible al retardo pero no requiere mucho ancho de banda),.
2. **Regulación del tráfico (Modelado):** Controlar la tasa promedio y las ráfagas de los flujos que entran a la red mediante algoritmos como la **cubeta con goteo** (_leaky bucket_) o la **cubeta con tokens**,,.
3. **Reserva de recursos:** Asignar ancho de banda, espacio de búfer y ciclos de CPU en los enrutadores para flujos específicos,.
4. **Control de admisión:** Decidir si la red puede aceptar de forma segura más tráfico sin comprometer las garantías ya existentes