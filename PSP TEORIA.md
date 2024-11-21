

### **¿Qué es `Thread` en Java?**

En Java, `Thread` es una clase que representa un hilo de ejecución. Un hilo es una secuencia de ejecución dentro de un proceso. Los programas en Java generalmente ejecutan varios hilos, permitiendo que diferentes tareas se realicen en paralelo.

**Uso de `Thread`:**
La clase `Thread` es utilizada para crear y gestionar hilos directamente. Puedes crear un hilo de dos formas:

1. **Extender la clase `Thread`**:
   Esta es la forma en la que se crea un hilo directamente, sobrescribiendo el método `run()` para definir la tarea que se va a ejecutar en el hilo.

### **Ejemplo de `Thread`** (extendiendo la clase):

```java
class MiHilo extends Thread {
    @Override
    public void run() {
        // Tarea que realiza el hilo
        System.out.println("Hola desde el hilo " + Thread.currentThread().getName());
    }
}

public class EjemploThread {
    public static void main(String[] args) {
        // Crear e iniciar el hilo
        MiHilo hilo1 = new MiHilo();
        hilo1.start();  // Llama al método run() de MiHilo
        
        // Crear e iniciar otro hilo
        MiHilo hilo2 = new MiHilo();
        hilo2.start();  // Llama al método run() de MiHilo
    }
}
```

En este ejemplo, `MiHilo` extiende de la clase `Thread` y sobrescribe el método `run()`. Al invocar `start()`, el hilo comienza su ejecución y automáticamente ejecuta el método `run()`.

**Ventajas de usar `Thread` directamente:**
- Es una forma sencilla de crear y gestionar un hilo si no necesitas implementar múltiples comportamientos.
- Ideal para tareas sencillas que no requieren mucha reutilización de la lógica de ejecución.

---

### **¿Qué es `Runnable` en Java?**

`Runnable` es una interfaz en Java que define un solo método, `run()`. Esta interfaz se utiliza para representar tareas que pueden ejecutarse en un hilo. A diferencia de la clase `Thread`, que es una clase concreta, `Runnable` es una interfaz que permite separar la lógica de la tarea de la gestión del hilo. Es comúnmente utilizada para pasar tareas a los hilos.

**Uso de `Runnable`:**
En lugar de extender la clase `Thread`, puedes implementar la interfaz `Runnable`, lo que permite que la clase tenga una estructura más flexible y sea compatible con otras jerarquías de clases (ya que Java permite extender solo una clase, pero puedes implementar múltiples interfaces).

### **Ejemplo de `Runnable`** (implementando la interfaz):

```java
class MiTarea implements Runnable {
    @Override
    public void run() {
        // Tarea que realiza el hilo
        System.out.println("Hola desde el hilo " + Thread.currentThread().getName());
    }
}

public class EjemploRunnable {
    public static void main(String[] args) {
        // Crear una instancia de MiTarea (que implementa Runnable)
        MiTarea tarea = new MiTarea();
        
        // Crear un hilo que ejecutará la tarea
        Thread hilo1 = new Thread(tarea);
        hilo1.start();  // Ejecuta el método run() de MiTarea
        
        // Crear otro hilo que ejecutará la misma tarea
        Thread hilo2 = new Thread(tarea);
        hilo2.start();  // Ejecuta el método run() de MiTarea
    }
}
```

En este ejemplo, la clase `MiTarea` implementa la interfaz `Runnable` y su método `run()` contiene la tarea que se va a ejecutar en el hilo. Posteriormente, la clase `Thread` se utiliza para ejecutar el objeto `Runnable`.

**Ventajas de usar `Runnable`:**
- `Runnable` es más flexible porque permite que una clase extienda otras clases además de implementarlo (ya que Java solo permite heredar de una clase, pero puedes implementar varias interfaces).
- Es más adecuado para tareas que puedan ser compartidas entre varios hilos, ya que la tarea no está atada a una implementación de `Thread`.
- Recomendado cuando la tarea que se va a ejecutar puede ser reutilizada en diferentes hilos sin la necesidad de extender `Thread`.

---

### **¿Cuándo usar `Thread` y cuándo usar `Runnable`?**

1. **Usar `Thread`**:
   - Si solo necesitas un hilo simple y no necesitas compartir la lógica de ejecución entre diferentes hilos.
   - Si la tarea a ejecutar no será reutilizada o compartida en otros hilos.
   - Si necesitas crear un hilo con la clase extendida y agregar funcionalidades adicionales a `Thread`.

2. **Usar `Runnable`**:
   - Si quieres separar la lógica de la tarea de la gestión del hilo (más flexible).
   - Si tu clase ya extiende otra clase y solo quieres implementar la tarea en un hilo (Java permite implementar múltiples interfaces).
   - Si la tarea será compartida entre diferentes hilos, lo que facilita la reutilización del código.

---

### **Comparativa Visual:**

| Característica      | `Thread`                                   | `Runnable`                                 |
|---------------------|--------------------------------------------|--------------------------------------------|
| **Herencia**        | Extiende la clase `Thread`                 | Implementa la interfaz `Runnable`          |
| **Flexibilidad**    | Menos flexible (solo puedes extender una clase) | Más flexible (puedes implementar múltiples interfaces) |
| **Uso principal**   | Para tareas simples donde no se reutiliza la lógica de ejecución | Para tareas más reutilizables, con flexibilidad de herencia |
| **Reutilización**   | No reutilizable (por estar atado a `Thread`) | Reutilizable, se puede pasar a varios hilos |

---

### **¿Qué es un semáforo?**

En programación concurrente, un **semáforo** es una técnica de sincronización utilizada para controlar el acceso a recursos compartidos por múltiples hilos. El semáforo mantiene un contador que representa el número de recursos disponibles, y su principal función es controlar cuántos hilos pueden acceder a una sección crítica del código en un momento dado.

#### Tipos de semáforos:

1. **Semáforo binario (o semáforo de un solo bit)**:
   - Tiene dos valores posibles: 0 o 1.
   - Se utiliza para permitir o bloquear el acceso a un recurso.
   - Es equivalente a un **mutex** (mutual exclusion), utilizado para garantizar que solo un hilo pueda ejecutar una sección crítica a la vez.

2. **Semáforo general**:
   - Tiene un valor entero mayor a 1.
   - Controla el número de hilos que pueden acceder a la sección crítica simultáneamente.
   - Es útil cuando se tiene un número limitado de recursos que pueden ser compartidos entre múltiples hilos (por ejemplo, un conjunto de impresoras o conexiones de red).

### **¿Cómo funciona un semáforo en Java?**

Java proporciona la clase `Semaphore` en el paquete `java.util.concurrent` para trabajar con semáforos. Esta clase permite controlar el acceso a un recurso limitado utilizando métodos como `acquire()` y `release()`.

- **`acquire()`**: Un hilo llama a este método para adquirir un permiso del semáforo. Si el semáforo tiene permisos disponibles (su contador es mayor que 0), el hilo obtiene uno y continúa. Si no hay permisos disponibles (el contador es 0), el hilo se bloquea hasta que un permiso esté disponible.
  
- **`release()`**: Un hilo llama a este método para liberar un permiso del semáforo. Esto incrementa el contador del semáforo, lo que puede permitir que otro hilo continúe.

#### Ejemplo de semáforo en Java:

```java
import java.util.concurrent.Semaphore;

public class SemaforoEjemplo {
    private static Semaphore semaforo = new Semaphore(2); // Solo dos hilos pueden acceder a la sección crítica a la vez

    public static void main(String[] args) {
        for (int i = 0; i < 5; i++) {
            new Thread(new Tarea()).start(); // Crear e iniciar 5 hilos
        }
    }

    static class Tarea implements Runnable {
        @Override
        public void run() {
            try {
                semaforo.acquire(); // Intentar adquirir un permiso
                System.out.println(Thread.currentThread().getName() + " ha comenzado la tarea.");
                Thread.sleep(2000); // Simula trabajo que toma tiempo
                System.out.println(Thread.currentThread().getName() + " ha terminado la tarea.");
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            } finally {
                semaforo.release(); // Liberar el permiso
            }
        }
    }
}
```

En este ejemplo, el semáforo tiene un contador inicial de 2, lo que significa que solo dos hilos pueden ejecutar la tarea al mismo tiempo. Los demás hilos deben esperar su turno hasta que uno de los hilos termine y libere un permiso.

---

### **Métodos importantes relacionados con hilos en Java**

Aquí están algunos de los métodos más utilizados para trabajar con hilos en Java:

1. **`run()`**:
   - **Descripción**: Es el método que contiene la tarea que el hilo debe ejecutar. Este método es definido en la clase `Thread` o en la interfaz `Runnable`.
   - **Cuándo se usa**: Cuando se crea un hilo, el sistema de hilos ejecuta el código dentro de `run()`. **No debe llamarse directamente**, ya que la llamada directa a `run()` no creará un hilo nuevo. Debe ser invocado a través del método `start()` de la clase `Thread`.
   
   **Ejemplo**:
   ```java
   class MiHilo implements Runnable {
       @Override
       public void run() {
           System.out.println("El hilo está ejecutando la tarea.");
       }
   }
   ```

2. **`start()`**:
   - **Descripción**: Este método inicia la ejecución de un hilo. Cuando se llama a `start()`, el hilo entra en el estado **Runnable** y el sistema operativo comienza a ejecutar el código dentro del método `run()`.
   - **Cuándo se usa**: Se llama una vez al crear el hilo, para iniciar la ejecución de la tarea de forma concurrente.
   - **Importante**: No debes llamar directamente al método `run()`, ya que eso no crea un nuevo hilo, simplemente ejecutaría `run()` en el hilo actual.

   **Ejemplo**:
   ```java
   Thread hilo = new Thread(new MiHilo());
   hilo.start(); // Inicia la ejecución del hilo
   ```

3. **`sleep(long millis)`**:
   - **Descripción**: Este método estático de la clase `Thread` pone en pausa el hilo durante el tiempo especificado en milisegundos.
   - **Cuándo se usa**: Se utiliza cuando quieres que un hilo "duerma" o espere durante un período determinado. Puede lanzar una excepción `InterruptedException` si el hilo es interrumpido mientras está dormido.
   
   **Ejemplo**:
   ```java
   try {
       Thread.sleep(1000); // Hilo duerme 1 segundo
   } catch (InterruptedException e) {
       Thread.currentThread().interrupt();
   }
   ```

4. **`join()`**:
   - **Descripción**: Este método se utiliza para **esperar** a que otro hilo termine su ejecución. Si un hilo A llama a `join()` en un hilo B, el hilo A se bloquea hasta que el hilo B termine su ejecución.
   - **Cuándo se usa**: Se usa cuando necesitas esperar que un hilo termine antes de continuar con otro hilo o acción.
   
   **Ejemplo**:
   ```java
   Thread hilo1 = new Thread(() -> System.out.println("Hilo 1"));
   hilo1.start();
   try {
       hilo1.join(); // Espera a que hilo1 termine
   } catch (InterruptedException e) {
       Thread.currentThread().interrupt();
   }
   System.out.println("Hilo 1 ha terminado.");
   ```

5. **`interrupt()`**:
   - **Descripción**: Este método solicita que el hilo sea interrumpido. Si el hilo está en estado de espera (por ejemplo, está en `sleep()` o `wait()`), se lanzará una `InterruptedException`.
   - **Cuándo se usa**: Se usa para interrumpir la ejecución de un hilo. Sin embargo, los hilos no se terminan de forma automática; es necesario que el hilo interrumpido verifique regularmente si ha sido interrumpido (a través del método `isInterrupted()` o manejando `InterruptedException`).
   
   **Ejemplo**:
   ```java
   Thread hilo = new Thread(() -> {
       try {
           Thread.sleep(5000);
       } catch (InterruptedException e) {
           System.out.println("Hilo interrumpido");
       }
   });
   hilo.start();
   hilo.interrupt(); // Interrumpe el hilo
   ```

---

### **Resumen**

- **`Thread`**: Es una clase que representa un hilo en ejecución. Se puede crear un hilo extendiendo `Thread` o implementando `Runnable`.
- **`Runnable`**: Es una interfaz que permite definir tareas que pueden ser ejecutadas por un hilo.
- **Métodos importantes**:
  - **`run()`**: Define la tarea que ejecutará el hilo.
  - **`start()`**: Inicia un hilo y llama a `run()`.
  - **`sleep()`**: Suspende la ejecución de un hilo durante un tiempo determinado.
  - **`join()`**: Permite que un hilo espere a que otro termine.
  - **`interrupt()`**: Solicita que un hilo sea interrumpido.

