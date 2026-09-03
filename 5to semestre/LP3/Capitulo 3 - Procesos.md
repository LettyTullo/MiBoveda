# ¿Qué es un proceso en Linux?

Un **proceso** es formalmente una **instancia en ejecución de un programa**. Por ejemplo, si tienes abiertas dos terminales ejecutando el mismo programa de consola, tendrás dos procesos distintos corriendo en tu sistema.

Cada proceso se caracteriza por:

- **Identificador de Proceso (PID):** Un número único de 16 bits asignado secuencialmente por el kernel de Linux para identificar a cada proceso activo.
- **Identificador del Padre (PPID):** Todo proceso en Linux (con excepción del proceso especial `init`) tiene un proceso padre que lo creó.
- **El Árbol de Procesos:** La estructura de procesos es jerárquica y tiene forma de árbol. La raíz de este árbol es el proceso especial **`init` (PID 1)**, que es el primer proceso que se inicia al arrancar el sistema operativo y el encargado de adoptar y limpiar a los procesos huérfanos.
# Funciones y Llamadas al Sistema de Procesos
#### 1. Consulta de Identidad

- **`getpid()`**
    - **Cabecera:** `<unistd.h>` (el tipo devuelto es `pid_t`, definido en `<sys/types.h>`).
    - **Propósito y utilidad:** Devuelve el PID único del proceso actual en ejecución. Sirve para que un programa se identifique a sí mismo o deje constancia de su identidad en archivos de registros (_logs_).
- **`getppid()`**
    - **Cabecera:** `<unistd.h>`.
    - **Propósito y utilidad:** Devuelve el PPID del proceso padre que creó al proceso actual. Sirve para rastrear de qué proceso proviene la ejecución o para comunicarse con él.

#### 2. Creación y Ejecución de Programas

- **`system()`**
    - **Cabecera:** `<stdlib.h>`.
    - **Propósito y utilidad:** Toma como argumento una cadena de caracteres que representa un comando y crea un subproceso que ejecuta el shell estándar (`/bin/sh`) para interpretar e iniciar dicho comando. Sirve para ejecutar de manera rápida y sencilla un comando externo. Sin embargo, se desaconseja en aplicaciones avanzadas porque es ineficiente, depende del shell del sistema y acarrea serios riesgos de seguridad.
- **`fork()`**
    - **Cabecera:** `<unistd.h>`.
    - **Propósito y utilidad:** Duplica el proceso actual, creando un nuevo proceso denominado "hijo". El proceso hijo es una copia exacta del proceso "padre" en cuanto a memoria virtual, descriptores de archivos abiertos y recursos.
    - **Para qué sirve:** Es el mecanismo fundamental en Linux para generar multitarea. Permite bifurcar el flujo de ejecución: al retornar la función, el padre recibe el PID del hijo recién creado, mientras que el hijo recibe un `0`, permitiendo diferenciar en el código quién ejecuta qué sección.
- **Familia `exec*()`** (como `execvp()`, `execv()`, `execl()`, `execlp()`, `execve()`, `execle()`)
    - **Cabecera:** `<unistd.h>`.
    - **Propósito y utilidad:** Reemplaza el programa que se está ejecutando en el proceso actual por un programa nuevo. Si tiene éxito, el proceso deja de correr el código original, carga el nuevo ejecutable desde el principio y nunca retorna a la llamada.
    - **Para qué sirve:** Permite que un proceso hijo (creado previamente con `fork()`) se transforme en una instancia de un programa completamente diferente. Las letras en sus nombres indican variaciones en cómo reciben sus parámetros:
        - **`p` (PATH):** Busca el programa en las rutas del sistema especificadas en la variable de entorno `PATH` (ej. `execvp`, `execlp`).
        - **`v` (Vector):** Recibe los argumentos del comando en forma de un arreglo de punteros a caracteres terminado en `NULL` (ej. `execv`, `execvp`, `execve`).
        - **`l` (List):** Recibe los argumentos como una lista fija de variables de longitud variable separadas por comas en C (ej. `execl`, `execlp`, `execle`).
        - **`e` (Environment):** Permite especificar de forma explícita un arreglo de variables de entorno para el nuevo programa (ej. `execve`, `execle`).

#### 3. Sincronización y Espera de Hijos

- **`wait()`**
    - **Cabecera:** `<sys/wait.h>`.
    - **Propósito y utilidad:** Bloquea al proceso padre hasta que uno de sus procesos hijos termina su ejecución (o se produce un error).
    - **Para qué sirve:** Evita condiciones de carrera y la acumulación de **procesos zombie**. Al completarse, recupera el estado de salida del hijo para que el sistema operativo pueda liberar definitivamente sus recursos de la tabla de procesos.
- **`waitpid()`**
    - **Cabecera:** `<sys/wait.h>`.
    - **Propósito y utilidad:** Es una versión más flexible de `wait()`. Permite esperar por un proceso hijo específico utilizando su PID como argumento.
    - **Para qué sirve:** Sirve para sincronizar de forma precisa un hijo determinado. También admite banderas como **`WNOHANG`**, lo que permite que la función opere de forma _no bloqueante_: si ningún hijo ha terminado en ese momento, la función retorna inmediatamente un `0` en lugar de detener la ejecución del padre.
- **`wait3()` y `wait4()`**
    - **Cabecera:** `<sys/wait.h>` (o `<sys/resource.h>`).
    - **Propósito y utilidad:** Cumplen el mismo rol de esperar la terminación de procesos hijos, pero añaden información detallada sobre el uso de recursos y estadísticas de uso de CPU que consumió el hijo durante su vida.
- **Macros de Análisis de Estado (`WIFEXITED`, `WEXITSTATUS`, `WTERMSIG`)**
    - **Cabecera:** `<sys/wait.h>`.
    - **Propósito y utilidad:** Se utilizan para interpretar el entero de estado (_status_) que devuelven las llamadas `wait()` o `waitpid()`.
        - **`WIFEXITED(status)`:** Devuelve verdadero si el proceso hijo finalizó de manera normal (utilizando `exit()` o retornando de `main`).
        - **`WEXITSTATUS(status)`:** Si el hijo terminó normalmente, extrae el código de salida devuelto por el hijo (un entero entre 0 y 127).
        - **`WTERMSIG(status)`:** Si el hijo finalizó anormalmente debido a una señal no manejada, esta macro extrae el número de señal que causó su terminación
#### 4. Finalización de Procesos

- **`exit()`**
    - **Cabecera:** `<stdlib.h>`.
    - **Propósito y utilidad:** Termina inmediatamente de manera normal el proceso que realiza la llamada.
    - **Para qué sirve:** Retorna un código numérico de salida al proceso padre (por convención, `0` indica éxito y valores distintos de cero indican algún error). Durante su cierre, la biblioteca estándar se encarga de vaciar los búferes de salida de E/S, cerrar descriptores de archivos y liberar la memoria dinámica asignada al proceso.
- **`_exit()`**
    - **Cabecera:** `<unistd.h>`.
    - **Propósito y utilidad:** Termina el proceso actual de manera inmediata.
    - **Para qué sirve:** A diferencia de `exit()`, finaliza el proceso sin ejecutar tareas de limpieza de la biblioteca estándar de C (como vaciar los búferes de `stdio` o ejecutar manejadores registrados). Es la función recomendada para terminar el subproceso hijo si una llamada a `execvp()` u otra de la familia falla, evitando alterar los búferes del padre.
- **`abort()`**
    - **Cabecera:** `<stdlib.h>`.
    - **Propósito y utilidad:** Envía de forma automática la señal `SIGABRT` al propio proceso para forzar su terminación anormal.
    - **Para qué sirve:** Se utiliza en situaciones de errores internos de software graves o fallos irrecuperables. Al provocar un fin anormal, el sistema operativo genera un volcado de memoria en un archivo central (_core file_), que resulta muy útil para realizar depuraciones post-mortem.
#### 5. Comunicación y Control

- **`kill()`**
    - **Cabecera:** `<sys/types.h>` y `<signal.h>`.
    - **Propósito y utilidad:** Envía una señal específica a un proceso de destino identificado por su PID.
    - **Para qué sirve:** Permite la comunicación asíncrona interprocesos. Se puede usar para solicitar que un proceso termine de manera ordenada (`SIGTERM`), forzar su finalización inmediata e inevitable (`SIGKILL`), o enviarle señales de propósito general de la aplicación (`SIGUSR1`, `SIGUSR2`).
- **`sigaction()`**
    - **Cabecera:** `<signal.h>`.
    - **Propósito y utilidad:** Configura o modifica cómo reacciona el proceso ante la recepción de una señal específica.
    - **Para qué sirve:** Permite cambiar la disposición (_disposition_) de una señal para:
        1. Ignorarla por completo (`SIG_IGN`).
        2. Restaurar el comportamiento por defecto del sistema (`SIG_DFL`).
        3. Instalar un manejador personalizado (_signal-handler_), que es una función de tu programa que se ejecutará asíncronamente en el momento exacto en que la señal sea entregada.
- **`nice()`**
    - **Cabecera:** `<unistd.h>`.
    - **Propósito y utilidad:** Incrementa o decrementa el valor de "niceness" (amabilidad) del proceso actual.
    - **Para qué sirve:** Controla de manera programática la prioridad con la que el kernel de Linux planifica la ejecución de tu proceso. Un valor de amabilidad más alto reduce la prioridad de ejecución del proceso para que consuma menos CPU y no ralentice el sistema. Solo procesos con privilegios de root pueden asignar valores negativos de niceness para aumentar su prioridad.
# Detalle Clave: El tipo de variable `sig_atomic_t`

Cuando trabajas con procesos y señales usando `sigaction()`, es sumamente peligroso modificar variables globales comunes dentro de tus manejadores de señales asínconos. Si una señal interrumpe al programa principal a mitad de una instrucción de escritura de varios pasos, la memoria puede corromperse.

Por ello, el sistema proporciona el tipo especial **`volatile sig_atomic_t`**. Las variables declaradas con este tipo garantizan que cualquier lectura o escritura se realice en **una sola instrucción de máquina indivisible (atómica)**, asegurando que tus variables compartidas entre el flujo principal y tus manejadores de señales no se corrompan.