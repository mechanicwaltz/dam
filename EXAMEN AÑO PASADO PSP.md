# CLASE ALMACEN:
```
import java.util.concurrent.Semaphore; // Importamos la clase Semaphore para gestionar la sincronización.

class Almacen {
    // Variables para almacenar la cantidad actual de verduras y cítricos.
    private int verduras = 0;
    private int citricos = 0;

    // Capacidad máxima que el almacén puede almacenar.
    private final int capacidadAlmacen;

    // Semáforo para garantizar exclusión mutua en la sección crítica.
    private final Semaphore mutex = new Semaphore(1);

    // Semáforos para controlar el espacio disponible para cada tipo de producto.
    private final Semaphore espacioVerduras;
    private final Semaphore espacioCitricos;

    // Semáforo para controlar la disponibilidad de productos para consumir.
    private final Semaphore productos;

    // Constructor que inicializa el almacén con la capacidad máxima.
    public Almacen(int capacidad) {
        this.capacidadAlmacen = capacidad;
        this.espacioVerduras = new Semaphore(capacidad); // Espacio disponible para verduras.
        this.espacioCitricos = new Semaphore(capacidad); // Espacio disponible para cítricos.
        this.productos = new Semaphore(0); // Inicialmente, no hay productos.
    }

    // Método para producir verduras.
    public void producirVerduras(int cantidad) throws InterruptedException {
        espacioVerduras.acquire(cantidad); // Bloquea hasta que haya espacio suficiente para las verduras.
        mutex.acquire(); // Entra en la sección crítica.
        try {
            verduras += cantidad; // Añade la cantidad de verduras al almacén.
            System.out.println("Producidas " + cantidad + " verduras. Total: " + verduras);
        } finally {
            mutex.release(); // Sale de la sección crítica.
        }
        productos.release(cantidad); // Indica que hay más productos disponibles para consumir.
    }

    // Método para producir cítricos.
    public void producirCitricos(int cantidad) throws InterruptedException {
        espacioCitricos.acquire(cantidad); // Bloquea hasta que haya espacio suficiente para cítricos.
        mutex.acquire(); // Entra en la sección crítica.
        try {
            citricos += cantidad; // Añade la cantidad de cítricos al almacén.
            System.out.println("Producidos " + cantidad + " cítricos. Total: " + citricos);
        } finally {
            mutex.release(); // Sale de la sección crítica.
        }
        productos.release(cantidad); // Indica que hay más productos disponibles para consumir.
    }

    // Método para consumir productos (verduras y cítricos).
    public void consumir(int verdurasConsumir, int citricosConsumir) throws InterruptedException {
        productos.acquire(verdurasConsumir + citricosConsumir); // Bloquea hasta que haya suficientes productos disponibles.
        mutex.acquire(); // Entra en la sección crítica.
        try {
            while (verduras < verdurasConsumir || citricos < citricosConsumir) {
                // Si no hay suficientes productos, espera y reintenta.
                mutex.release();
                productos.release(verdurasConsumir + citricosConsumir);
                Thread.sleep(100);
                productos.acquire(verdurasConsumir + citricosConsumir);
                mutex.acquire();
            }
            verduras -= verdurasConsumir; // Resta las verduras consumidas.
            citricos -= citricosConsumir; // Resta los cítricos consumidos.
            System.out.println("Consumidas " + verdurasConsumir + " verduras y " + citricosConsumir + " cítricos.");
        } finally {
            mutex.release(); // Sale de la sección crítica.
            espacioVerduras.release(verdurasConsumir); // Libera espacio para más verduras.
            espacioCitricos.release(citricosConsumir); // Libera espacio para más cítricos.
        }
    }
}
```

# CLASE PRODUCTOR CITRICOS:
```
import java.util.Random; // Importamos la clase Random para generar números aleatorios.

class ProductorCitricos implements Runnable {
    private final Almacen almacen; // Almacén compartido.

    // Constructor que recibe el almacén.
    public ProductorCitricos(Almacen almacen) {
        this.almacen = almacen;
    }

    @Override
    public void run() {
        Random rand = new Random(); // Generador de números aleatorios.
        int iteraciones = 5 + rand.nextInt(6); // Genera un número aleatorio entre 5 y 10.

        try {
            for (int i = 0; i < iteraciones; i++) {
                int cantidad = 1 + rand.nextInt(10); // Genera un número aleatorio entre 1 y 10.
                almacen.producirCitricos(cantidad); // Produce cítricos en el almacén.
                Thread.sleep(500); // Pausa de 0.5 segundos.
            }
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt(); // Maneja interrupciones del hilo.
        }
    }
}
```

# CLASE PRODUCTOR VERDURAS
```
import java.util.Random; // Importamos la clase Random para generar números aleatorios.

class ProductorVerduras implements Runnable {
    private final Almacen almacen; // Almacén compartido.

    // Constructor que recibe el almacén.
    public ProductorVerduras(Almacen almacen) {
        this.almacen = almacen;
    }

    @Override
    public void run() {
        Random rand = new Random(); // Generador de números aleatorios.
        int iteraciones = 5 + rand.nextInt(6); // Genera un número aleatorio entre 5 y 10.

        try {
            for (int i = 0; i < iteraciones; i++) {
                int cantidad = 1 + rand.nextInt(10); // Genera un número aleatorio entre 1 y 10.
                almacen.producirVerduras(cantidad); // Produce verduras en el almacén.
                Thread.sleep(500); // Pausa de 0.5 segundos.
            }
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt(); // Maneja interrupciones del hilo.
        }
    }
}
```

# CLASE CONSUMIDOR
```
import java.util.Random; // Importamos la clase Random para generar números aleatorios.

class Consumidor implements Runnable {
    private final Almacen almacen; // Almacén compartido.

    // Constructor que recibe el almacén.
    public Consumidor(Almacen almacen) {
        this.almacen = almacen;
    }

    @Override
    public void run() {
        Random rand = new Random(); // Generador de números aleatorios.
        int iteraciones = 5 + rand.nextInt(6); // Genera un número aleatorio entre 5 y 10.

        try {
            for (int i = 0; i < iteraciones; i++) {
                int verdurasConsumir = 1 + rand.nextInt(10); // Genera un número aleatorio entre 1 y 10.
                int citricosConsumir = 1 + rand.nextInt(10); // Genera un número aleatorio entre 1 y 10.
                almacen.consumir(verdurasConsumir, citricosConsumir); // Consume productos del almacén.
                Thread.sleep(500); // Pausa de 0.5 segundos.
            }
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt(); // Maneja interrupciones del hilo.
        }
    }
}
```

# CLASE MERCADO 
```
public class Mercado {
    public static void main(String[] args) {
        // Establece valores predeterminados
        int productoresVerduras = 3;  // Número de productores de verduras
        int productoresCitricos = 3;  // Número de productores de cítricos
        int consumidores = 4;         // Número de consumidores
        int capacidad = 20;           // Capacidad máxima del almacén por tipo de producto

        // Crea el objeto compartido "Almacen"
        Almacen almacen = new Almacen(capacidad);

        // Array para almacenar los hilos
        Thread[] threads = new Thread[productoresVerduras + productoresCitricos + consumidores];

        // Crea los hilos de productores de verduras
        for (int i = 0; i < productoresVerduras; i++) {
            threads[i] = new Thread(new ProductorVerduras(almacen));
        }

        // Crea los hilos de productores de cítricos
        for (int i = 0; i < productoresCitricos; i++) {
            threads[productoresVerduras + i] = new Thread(new ProductorCitricos(almacen));
        }

        // Crea los hilos de consumidores
        for (int i = 0; i < consumidores; i++) {
            threads[productoresVerduras + productoresCitricos + i] = new Thread(new Consumidor(almacen));
        }

        // Inicia todos los hilos
        for (Thread thread : threads) {
            thread.start();
        }

        // Espera a que todos los hilos terminen
        for (Thread thread : threads) {
            try {
                thread.join();
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt(); // Maneja interrupciones del hilo
            }
        }

        System.out.println("Simulación completada."); // Mensaje final al completar la simulación
    }
}
```
