#****Cómo compilar y ejecutar****

Para compilar el código en C++ se sugiere utilizar g++ con soporte para C++11 o versiones posteriores:

> g++ -std=c++11 filosofos. cpp -o filosofos -pthread

- std=c++11: habilita las funciones avanzadas de C++ (hilos y mutex).

- pthread: permite el uso de hilos en sistemas Linux/Unix.

Para correr la simulación:

### ./filosofos

La simulación tiene una duración de 10 segundos y al finalizar indica cuántas veces comió cada filósofo.

Herramientas de sincronización

> std::mutex

Se encarga de proteger los recursos compartidos para evitar el acceso simultáneo de múltiples hilos.

En este programa:

> > tenedor[N]: mutex correspondiente a cada tenedor de los filósofos.

> > pantalla: mutex que asegura que los mensajes se impriman de manera ordenada.

luego esta : 

> -lock_guard

Automáticamente bloquea un mutex al iniciar un bloque y lo libera al finalizar.

Se aplica para asegurar que los mensajes se muestren de forma organizada en la consola.

#****Estrategias de concurrencia****
Prevención de deadlock

- Los filósofos del 1 al N-1: primero toman el tenedor de la izquierda y luego el de la derecha.

- El filósofo N: invierte el orden, tomando primero el tenedor derecho y luego el izquierdo.

Esta lógica interrumpe el ciclo de espera circular y previene que todos los filósofos queden atrapados.
