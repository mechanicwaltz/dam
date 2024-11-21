# MAIN: 
```
package org.example;

import java.util.Random;
import java.util.concurrent.Semaphore;

//Clase principal que contiene los ejercicios
public class Main {
    public static void main(String[] args) {
        //ej01();
        //ej02();
        //ej03();
        ej04();
    }

    public void ej01() {
        //Como queremos que sean 5 mensajes, hacemos un bucle de 5 iteraciones
        for (int i = 0; i < 5; i++) {
            //Creamos un hilo con la clase imprimirMensaje y se enchufa.
            Thread hilo = new Thread(new imprimirMensaje());
            hilo.start();
            System.out.println("Hilo " + (i + 1) + " creado");
        }
    }

    public static void ej02() {
        //Se inicializa el contador a 0.
        int contador = 0;
        // Se crea un bucle de 10 iteraciones para crear 10 hilos
        for (int i = 0; i < 10; i++) {
            // En cada interacción, se crea un hilo con la clase contadorCompartidoSinSemaforo con el valor actual del contador
            Thread hilo = new Thread(new contadorCompartidoSinSemaforo(contador));
            // Se arranca el hilo y se incrementa el contador a 100.
            hilo.start();
            contador += 100;
            // Se imprime un mensaje con el número de hilo creado y el valor del contador
            System.out.println("Hilo " + (i + 1) + " creado, contador: " + contador);
        }
    }

    public static void ej03() {
        int contador = 0;
        for (int i = 0; i < 10; i++) {
            Thread hilo = new Thread(new contadorCompartidoConSemaforo(contador));
            hilo.start();
            contador += 100;
            System.out.println("Hilo " + (i + 1) + " creado, contador: " + contador);
        }
    }
    public static void ej04(){
        Task task = new Task();
        Thread thread1 = new Thread(new Runnable() {
            @Override
            public void run() {
                task.waitForTask();
            }
        });

        // Crear el segundo hilo que realizará la tarea
        Thread thread2 = new Thread(new Runnable() {
            @Override
            public void run() {
                task.performTask();
            }
        });

        // Iniciar ambos hilos
        thread1.start();
        thread2.start();
    }
}


    class imprimirMensaje implements Runnable {
        //Se implementa la interfaz Runnable con el Override de run (obligatorio)
        //Dentro del run, se hace un random de 0 a 5 segundos, que usamos para poner a dormir los hilos
        //Se imprime un mensaje con el tiempo que se ha dormido el hilo
        @Override
        public void run() {
            try {
                Random r = new Random();
                int retraso = r.nextInt(5000);
                Thread.sleep(retraso);
                System.out.println("Holi, soy un mensaje mandado despues de  " + retraso + " milisegundos");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }


    }


     class contadorCompartidoSinSemaforo implements Runnable {
        private int contador;

        //Constructor que recibe el valor del contador
        public contadorCompartidoSinSemaforo(int contador) {
            this.contador = contador;
        }

        //Override de run, donde se incrementa el contador en 100 y se imprime el valor del contador hasta que el contador llega a 1000.
        //Se pone un sleep de 1 segundo para simular trabajo.
        //T0do esto metido en un bloque try catch para evitar errores.
        @Override
        public void run() {
            try {
                while (contador < 1000) {
                    contador += 100;
                    System.out.println("Contador actualizado: " + contador);
                    Thread.sleep(1000); // Añadir un pequeño retraso para simular trabajo
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

     class contadorCompartidoConSemaforo implements Runnable {
        //Variable estática para compartir el valor del contador entre los hilos
        private static int contador;
        //Semaforo para sincronizar el acceso al contador
        private static Semaphore semaforo = new Semaphore(1);

        //Constructor que recibe el valor inicial del contador
        public contadorCompartidoConSemaforo(int contador) {
            this.contador = contador;
        }

        @Override
        public void run() {
            try {
                while (true) {
                    //Adquirimos el semáforo para acceder al contador
                    semaforo.acquire();
                    // Si el contador llega a 1000, liberamos el semáforo y salimos del bucle
                    if (contador >= 1000) {
                        semaforo.release();
                        break;
                    }
                    //Incrementamos el contador en 100
                    contador += 100;
                    //Liberamos el semáforo
                    semaforo.release();
                    //Añadir un pequeño retraso para simular trabajo
                    Thread.sleep(1000);
                }
            } catch (InterruptedException e) {

                e.printStackTrace();
            }
        }
    }
```

# CLASE TASK:

```
package org.example;

public class Task { // Objeto de bloqueo para sincronizar los hilos
    private final Object lock = new Object();

    // Método que hace que el primer hilo espere hasta que se complete la tarea
    public void waitForTask() {
        synchronized (lock) {
            try {
                System.out.println("Thread 1: Waiting for the task to be completed...");
                // Esperar a que se notifique que la tarea ha sido completada
                lock.wait();
                System.out.println("Thread 1: Task completed! Continuing execution...");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
    public void performTask() {
        synchronized (lock) {
            try {
                System.out.println("Thread 2: Performing the task...");
                // Simular una tarea durmiendo el hilo durante 3 segundos
                Thread.sleep(3000);
                System.out.println("Thread 2: Task done! Notifying...");
                // Notificar al primer hilo que la tarea ha sido completada
                lock.notify();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    /*
  
```

  ## Explicación del código:
### Clase Main:
#### Método main:
- Crea una instancia de la clase Task.
- Crea dos hilos: uno que espera a que se complete la tarea (thread1) y otro que realiza la tarea (thread2).
- Inicia ambos hilos.
#### Clase Task:
- Atributo lock:
Un objeto utilizado para sincronizar los hilos.
- Método waitForTask:
Sincroniza en el objeto lock.
Imprime un mensaje indicando que está esperando.
Llama a lock.wait() para esperar a que se complete la tarea.
Una vez notificado, imprime un mensaje indicando que la tarea se ha completado.
- Método performTask:
Sincroniza en el objeto lock.
Imprime un mensaje indicando que está realizando la tarea.
Simula una tarea durmiendo el hilo durante 3 segundos.
Imprime un mensaje indicando que la tarea ha terminado.
Llama a lock.notify() para notificar al primer hilo que la tarea se ha completado. */
}
