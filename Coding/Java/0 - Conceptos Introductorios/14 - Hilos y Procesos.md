### Hilos (Threads) en Java

Un hilo es una unidad ligera de ejecución dentro de un proceso. Todos los hilos de un proceso comparten el mismo espacio de direcciones, lo que les permite compartir datos y recursos de manera eficiente. En Java, los hilos se pueden crear y gestionar de varias maneras:

En Java, hay dos maneras principales de crear hilos:
**Implementando la interfaz `Runnable`**:

- Crea una clase que implemente la interfaz `Runnable` y sobrescribe el método `run()`.
- Crea una instancia de la clase `Thread` pasando como argumento una instancia de la clase que implementa `Runnable`.
- Llama al método `start()` en la instancia de `Thread`.

```java
public class MyRunnable implements Runnable {
    public void run() {
        // Código a ejecutar en el hilo
        System.out.println("Hilo en ejecución");
    }
}

public class Main {
    public static void main(String[] args) {
        MyRunnable myRunnable = new MyRunnable();
        Thread thread = new Thread(myRunnable);
        thread.start();
    }
}
```

**Extendiendo la clase `Thread`**:
- Crea una clase que extienda la clase `Thread` y sobrescribe el método `run()`.
- Crea una instancia de la clase y llama al método `start()`.

```java
public class MyThread extends Thread {
    public void run() {
        // Código a ejecutar en el hilo
        System.out.println("Hilo en ejecución");
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread myThread = new MyThread();
        myThread.start();
    }
}
```
### Procesos en Java

Un proceso es una instancia de un programa que se ejecuta de manera aislada y tiene su propio espacio de direcciones. Los procesos son más pesados que los hilos y se comunican entre sí mediante mecanismos de inter-procesos (IPC) como archivos, sockets o colas de mensajes. En Java, se pueden lanzar procesos usando la clase `ProcessBuilder` o la clase `Runtime`.


### Diferencias entre Hilos y Procesos

- **Espacio de Direcciones**: Los hilos comparten el mismo espacio de direcciones dentro de un proceso, mientras que los procesos tienen su propio espacio de direcciones.
- **Comunicación**: Los hilos se comunican de manera directa y eficiente al compartir memoria, mientras que los procesos requieren mecanismos de IPC.
- **Peso**: Crear y gestionar hilos es más ligero y rápido que hacerlo con procesos.
- **Seguridad y Estabilidad**: Los procesos son más seguros y estables porque están aislados; un fallo en un proceso no afecta a otros, mientras que un fallo en un hilo puede afectar a todo el proceso.