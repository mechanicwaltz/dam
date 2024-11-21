# Ejercicio 4: Productor y consumidor sencillo con espera activa
Crea un programa en el que un hilo (el productor) genere números aleatorios y otro hilo (el consumidor) los imprima. Usa una bandera compartida para indicar si hay un número disponible y emplea sincronización con synchronized para evitar conflictos.
```
import java.util.Random;

public class ProductorConsumidor {
    private static int numero;
    private static boolean disponible = false; // Indica si hay un número listo

    public static void main(String[] args) {
        Object lock = new Object(); // Objeto de sincronización

        Thread productor = new Thread(() -> {
            Random rand = new Random();
            for (int i = 0; i < 5; i++) {
                synchronized (lock) {
                    while (disponible) { // Esperar si el consumidor no ha procesado
                        try {
                            lock.wait();
                        } catch (InterruptedException e) {
                            Thread.currentThread().interrupt();
                        }
                    }
                    numero = rand.nextInt(100); // Genera un número aleatorio
                    System.out.println("Productor generó: " + numero);
                    disponible = true; // Marca el número como disponible
                    lock.notify(); // Notifica al consumidor
                }
            }
        });

        Thread consumidor = new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                synchronized (lock) {
                    while (!disponible) { // Esperar hasta que haya un número
                        try {
                            lock.wait();
                        } catch (InterruptedException e) {
                            Thread.currentThread().interrupt();
                        }
                    }
                    System.out.println("Consumidor procesó: " + numero);
                    disponible = false; // Marca como procesado
                    lock.notify(); // Notifica al productor
                }
            }
        });

        productor.start();
        consumidor.start();
    }
}
```
---


# Ejercicio 5: Hilos que trabajan en paralelo
Crea 5 hilos que impriman números del 1 al 10, pero que cada uno lo haga con un retardo distinto (entre 1 y 3 segundos por número). Al final, todos deben terminar de ejecutar.

```
import java.util.Random;

public class HilosParalelo {
    public static void main(String[] args) {
        for (int i = 1; i <= 5; i++) {
            final int hiloId = i; // Identificador del hilo
            Thread hilo = new Thread(() -> {
                Random rand = new Random();
                for (int j = 1; j <= 10; j++) {
                    System.out.println("Hilo " + hiloId + ": " + j);
                    try {
                        Thread.sleep(rand.nextInt(2000) + 1000); // Pausa entre 1 y 3 segundos
                    } catch (InterruptedException e) {
                        Thread.currentThread().interrupt();
                    }
                }
            });
            hilo.start();
        }
    }
}

```
---

# Ejercicio 6: Semáforo para gestionar acceso
Simula un programa en el que 3 hilos representan autos intentando entrar en un estacionamiento con solo 2 plazas. Usa Semaphore para gestionar el acceso de los autos.

```
import java.util.concurrent.Semaphore;

public class Estacionamiento {
    public static void main(String[] args) {
        Semaphore plazas = new Semaphore(2); // Estacionamiento con 2 plazas

        for (int i = 1; i <= 3; i++) {
            final int autoId = i;
            Thread auto = new Thread(() -> {
                try {
                    System.out.println("Auto " + autoId + " intentando entrar.");
                    plazas.acquire(); // Intentar ocupar una plaza
                    System.out.println("Auto " + autoId + " estacionado.");
                    Thread.sleep(2000); // Simula tiempo estacionado
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                } finally {
                    System.out.println("Auto " + autoId + " salió del estacionamiento.");
                    plazas.release(); // Libera la plaza
                }
            });
            auto.start();
        }
    }
}

```

---
# Ejercicio 7: Hilos en cascada
Crea un programa donde tres hilos impriman mensajes en orden. El hilo 1 imprime su mensaje, luego el hilo 2 y, por último, el hilo 3. Usa los métodos wait() y notify() para coordinar la ejecución.

```
public class HilosCascada {
    public static void main(String[] args) {
        Object lock = new Object();

        Thread hilo1 = new Thread(() -> {
            synchronized (lock) {
                System.out.println("Hilo 1: Hola");
                lock.notify();
            }
        });

        Thread hilo2 = new Thread(() -> {
            synchronized (lock) {
                try {
                    lock.wait();
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
                System.out.println("Hilo 2: ¿Cómo estás?");
                lock.notify();
            }
        });

        Thread hilo3 = new Thread(() -> {
            synchronized (lock) {
                try {
                    lock.wait();
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
                System.out.println("Hilo 3: Adiós");
            }
        });

        hilo1.start();
        hilo2.start();
        hilo3.start();
    }
}

```
---
# Ejercicio 8: Carreras entre hilos
Crea un programa en el que dos hilos compiten para llegar a un objetivo. Cada hilo avanza en pasos aleatorios (entre 1 y 10 unidades) en cada iteración. El programa debe mostrar quién gana la carrera.

```
import java.util.Random;

public class CarreraHilos {
    public static void main(String[] args) {
        final int objetivo = 100; // Distancia que deben recorrer
        Object lock = new Object(); // Objeto para sincronización

        Thread corredor1 = new Thread(new Corredor("Corredor 1", objetivo, lock));
        Thread corredor2 = new Thread(new Corredor("Corredor 2", objetivo, lock));

        // Iniciar los hilos
        corredor1.start();
        corredor2.start();
    }
}

class Corredor implements Runnable {
    private final String nombre;
    private final int objetivo;
    private final Object lock;
    private int distancia = 0;

    public Corredor(String nombre, int objetivo, Object lock) {
        this.nombre = nombre;
        this.objetivo = objetivo;
        this.lock = lock;
    }

    @Override
    public void run() {
        Random rand = new Random();
        while (distancia < objetivo) {
            int paso = rand.nextInt(10) + 1; // Avanza entre 1 y 10 unidades
            distancia += paso;
            System.out.println(nombre + " avanzó " + paso + " unidades. Total: " + distancia);

            if (distancia >= objetivo) {
                synchronized (lock) {
                    System.out.println("¡" + nombre + " ha ganado la carrera!");
                    System.exit(0); // Termina el programa cuando hay un ganador
                }
            }

            try {
                Thread.sleep(500); // Pausa para simular el tiempo entre pasos
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
    }
}

```
---

# Ejercicio 9: Hilos con prioridad
Crea un programa donde se creen 5 hilos con diferentes prioridades.
Cada hilo debe imprimir su prioridad y un mensaje indicando que está ejecutándose.
Observa cómo la prioridad afecta el orden de ejecución de los hilos.

```
package org.example;

public class Principal {
    public static void main(String[] args) {
        // Crear 5 hilos con diferentes prioridades
        for (int i = 1; i <= 5; i++) {
            // Crear un hilo con la tarea PrioridadTarea y establecer su prioridad
            Thread hilo = new Thread(new PrioridadTarea(i));
            hilo.setPriority(i);
            hilo.start();
        }
    }
}

class PrioridadTarea implements Runnable {
    private int prioridad;

    // Constructor que recibe la prioridad del hilo
    public PrioridadTarea(int prioridad) {
        this.prioridad = prioridad;
    }

    @Override
    public void run() {
        // Imprimir la prioridad del hilo y un mensaje indicando que está ejecutándose
        System.out.println("Hilo con prioridad " + prioridad + " está ejecutándose.");
    }
}
```
----

# Ejercicio 10: Productor-Consumidor
Implementa el problema del productor-consumidor usando una cola compartida.
Crea un hilo productor que genere números aleatorios y los coloque en la cola.
Crea un hilo consumidor que tome los números de la cola y los imprima.
Usa los métodos wait() y notify() para coordinar la producción y el consumo.

```
package org.example;

import java.util.LinkedList;
import java.util.Queue;

public class Principal {
    public static void main(String[] args) {
        // Crear una instancia de la cola compartida
        ColaCompartida colaCompartida = new ColaCompartida();

        // Crear el hilo productor y el hilo consumidor
        Thread productor = new Thread(new Productor(colaCompartida));
        Thread consumidor = new Thread(new Consumidor(colaCompartida));

        // Iniciar ambos hilos
        productor.start();
        consumidor.start();
    }
}

class ColaCompartida {
    private final Queue<Integer> cola = new LinkedList<>();
    private final int LIMITE = 10;
    private final Object bloqueo = new Object();

    // Método para producir un valor y añadirlo a la cola
    public void producir(int valor) throws InterruptedException {
        synchronized (bloqueo) {
            // Esperar si la cola está llena
            while (cola.size() == LIMITE) {
                bloqueo.wait();
            }
            // Añadir el valor a la cola
            cola.add(valor);
            // Notificar al consumidor que hay un nuevo valor en la cola
            bloqueo.notify();
        }
    }

    // Método para consumir un valor de la cola
    public int consumir() throws InterruptedException {
        synchronized (bloqueo) {
            // Esperar si la cola está vacía
            while (cola.isEmpty()) {
                bloqueo.wait();
            }
            // Obtener el valor de la cola
            int valor = cola.poll();
            // Notificar al productor que hay espacio en la cola
            bloqueo.notify();
            return valor;
        }
    }
}

class Productor implements Runnable {
    private final ColaCompartida colaCompartida;

    // Constructor que recibe la cola compartida
    public Productor(ColaCompartida colaCompartida) {
        this.colaCompartida = colaCompartida;
    }

    @Override
    public void run() {
        try {
            // Producir 20 valores aleatorios
            for (int i = 0; i < 20; i++) {
                int valor = (int) (Math.random() * 100);
                colaCompartida.producir(valor);
                System.out.println("Producido: " + valor);
                // Dormir el hilo por 500 milisegundos
                Thread.sleep(500);
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

class Consumidor implements Runnable {
    private final ColaCompartida colaCompartida;

    // Constructor que recibe la cola compartida
    public Consumidor(ColaCompartida colaCompartida) {
        this.colaCompartida = colaCompartida;
    }

    @Override
    public void run() {
        try {
            // Consumir 20 valores de la cola
            for (int i = 0; i < 20; i++) {
                int valor = colaCompartida.consumir();
                System.out.println("Consumido: " + valor);
                // Dormir el hilo por 1000 milisegundos
                Thread.sleep(1000);
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

---

# Dilema de los filósofos

```
package org.example;

import java.util.concurrent.Semaphore;

public class Principal {
    public static void main(String[] args) {
        Filosofo[] filosofos = new Filosofo[5];
        Semaphore[] tenedores = new Semaphore[5];

        for (int i = 0; i < tenedores.length; i++) {
            tenedores[i] = new Semaphore(1);
        }

        for (int i = 0; i < filosofos.length; i++) {
            filosofos[i] = new Filosofo(i, tenedores[i], tenedores[(i + 1) % tenedores.length]);
            new Thread(filosofos[i]).start();
        }
    }
}

class Filosofo implements Runnable {
    private final int id;
    private final Semaphore tenedorIzquierdo;
    private final Semaphore tenedorDerecho;

    public Filosofo(int id, Semaphore tenedorIzquierdo, Semaphore tenedorDerecho) {
        this.id = id;
        this.tenedorIzquierdo = tenedorIzquierdo;
        this.tenedorDerecho = tenedorDerecho;
    }

    @Override
    public void run() {
        try {
            while (true) {
                pensar();
                recogerTenedores();
                comer();
                soltarTenedores();
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    private void pensar() throws InterruptedException {
        System.out.println("Filósofo " + id + " está pensando.");
        Thread.sleep((int) (Math.random() * 1000));
    }

    private void recogerTenedores() throws InterruptedException {
        tenedorIzquierdo.acquire();
        tenedorDerecho.acquire();
        System.out.println("Filósofo " + id + " recogió los tenedores.");
    }

    private void comer() throws InterruptedException {
        System.out.println("Filósofo " + id + " está comiendo.");
        Thread.sleep((int) (Math.random() * 1000));
    }

    private void soltarTenedores() {
        tenedorIzquierdo.release();
        tenedorDerecho.release();
        System.out.println("Filósofo " + id + " soltó los tenedores.");
    }
}
```
---
