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
