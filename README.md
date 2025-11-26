#🛠️ Cómo compilar y ejecutar el programa

Para ejecutar el programa principal filosofos.cpp:

- Se debe compilar el archivo usando un compilador de C++ compatible con C++11.
- Se revisa que las lineas de comando no tenga errores para la ejecucio del programa .

- Es necesario habilitar la librería de hilos (pthread) para que el programa funcione correctamente.

- Una vez compilado, se obtiene un ejecutable llamado filosofos, que al abrirse inicia la simulación de los filósofos durante el tiempo definido en el código (por ejemplo, 21 segundos).

- Al finalizar, el programa muestra en pantalla cuántas veces comió cada filósofo.

En resumen: primero se compila el archivo principal respetando el estándar C++11 y el soporte para hilos, y luego se ejecuta el resultado para ver la simulación y poder verificar si esta bien .

#🧩Herramientas de sincronización utilizadas

###1.  Uso de mutex para controlar el acceso a los tenedores

En el programa se crean cinco mutex, uno por cada tenedor:

> mutex tenedor[N];


Cada filósofo debe bloquear (lock) un tenedor antes de usarlo y liberarlo (unlock) después de comer.
Esto garantiza que ningún filósofo pueda usar el mismo tenedor al mismo tiempo, evitando condiciones de carrera y conflictos entre hilos.

###b ) Mutex adicional para controlar la salida en pantalla

Se utiliza un mutex para asegurar que los mensajes en la consola aparezcan ordenados:

> mutex pantalla;


Se aplica junto con lock_guard:

> lock_guard<mutex> lock(pantalla);


Esto permite que solo un hilo pueda imprimir en la consola a la vez, evitando que los mensajes se mezclen o aparezcan desordenados.
###3. Estrategia para prevenir el deadlock: inversión del orden de toma de tenedores

El deadlock ocurre cuando todos los filósofos toman un tenedor y esperan eternamente por el otro.
Para evitarlo, el programa implementa una estrategia clásica:

> bool invertir = (id == N - 1);


- Los filósofos 0 a 3 toman primero el tenedor izquierdo y luego el derecho.

- El último filósofo (el número 4) toma primero el derecho y luego el izquierdo.

- Esta inversión rompe la cadena circular que provoca deadlock, asegurando que al menos un filósofo siempre pueda avanzar.

###4. Uso de sleep_for para permitir alternancia y evitar saturación

El programa utiliza pausas controladas:

> this_thread::sleep_for(chrono::milliseconds(...));


Esto simula los tiempos de pensar y comer y evita que los hilos compitan de manera incesante por los recursos, permitiendo una ejecución más ordenada.
