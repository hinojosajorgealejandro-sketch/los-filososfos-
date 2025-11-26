# 🧩 Problema de los Filósofos Comensales  
## Implementación en C++ con Threads y Mutex

## 📌 Descripción
Este programa simula el problema clásico de concurrencia conocido como **Los Filósofos Comensales**, utilizando:

- Hilos (`std::thread`)
- Mutex para exclusión mutua
- Impresión sincronizada con `lock_guard`
- Estrategia para evitar *deadlock*

---

# 🧠 Código completo

```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <chrono>
using namespace std;

const int N = 5;
mutex tenedor[N];
mutex pantalla;        // para imprimir ordenado
bool correr = true;    // controla el tiempo de la simulación
int comidas[N] = {0};

void filosofo(int id)
{
    int izq = id;
    int der = (id + 1) % N;

    bool invertir = (id == N - 1); // el último invierte el orden

    while (correr)
    {
        // Pensando
        {
            lock_guard<mutex> lock(pantalla);
            cout << "Filosofo " << id+1 << " esta pensando..." << endl;
        }
        this_thread::sleep_for(chrono::milliseconds(400));

        {
            lock_guard<mutex> lock(pantalla);
            cout << "Filosofo " << id+1 << " quiere comer." << endl;
        }

        // Tomar tenedores
        if (!invertir)
        {
            tenedor[izq].lock();
            tenedor[der].lock();
        }
        else
        {
            tenedor[der].lock();
            tenedor[izq].lock();
        }

        // Comiendo
        {
            lock_guard<mutex> lock(pantalla);
            cout << "Filosofo " << id+1 << " esta comiendo..." << endl;
        }
        comidas[id]++;
        this_thread::sleep_for(chrono::milliseconds(500));

        // Soltar tenedores
        tenedor[izq].unlock();
        tenedor[der].unlock();
    }
}

int main()
{
    thread hilos[N];

    for (int i = 0; i < N; i++)
        hilos[i] = thread(filosofo, i);

    this_thread::sleep_for(chrono::seconds(10));
    correr = false;

    for (int i = 0; i < N; i++)
        hilos[i].join();

    cout << "\n===== RESULTADOS =====\n";
    for (int i = 0; i < N; i++)
        cout << "Filosofo " << i+1 << " comio " << comidas[i] << " veces.\n";

    return 0;
}
