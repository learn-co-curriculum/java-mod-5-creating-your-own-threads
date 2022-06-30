# Creating Your Own Threads

## Learning Goals

- Create custom threads in Java.
- Create simple multithreaded programs.

## Creating Custom Threads in Java

We can create and run custom threads from the main thread. Java gives us two
main ways of creating custom threads:

- Extending the `Thread` class and overriding its `run` method.
- Implementing the `Runnable` interface and passing it to the `Thread` class
  constructor.

### Subclassing the `Thread` Class

In this case, we extend the `Thread` class and override the `run` method. We can
initialize threads as instances of the extended class.

```java
class ExtendedThread extends Thread {
    @Override
    public void run() {
        String msg = String.format("This is %s", getName());
        System.out.println(msg);
    }
}

class Example {
    public static void main(String[] args) {
        Thread t = new ExtendedThread();

        System.out.println("Name: " + t.getName());
        System.out.println("ID: " + t.getId());
        System.out.println("Alive: " + t.isAlive());
        System.out.println("Priority: " + t.getPriority());
        System.out.println("Daemon: " + t.isDaemon());
    }
}
```

```plaintext
Name: Thread-0
ID: 15
Alive: false
Priority: 5
Daemon: false
```

### Implementing the `Runnable` Interface

In this case, we have to implement the `Runnable` interface and override its
`run` method. In order to create a thread, we have to pass the implementation to
the `Thread` constructor.

```java
class RunnableThread implements Runnable {
    @Override
    public void run() {
        String threadName = Thread.currentThread().getName();
        String msg = String.format("This is %s", threadName);
        System.out.println(msg);
    }
}

class Example {
    public static void main(String[] args) {
        Thread t = new Thread(new RunnableThread(), "custom-thread");

        System.out.println("Name: " + t.getName());
        System.out.println("ID: " + t.getId());
        System.out.println("Alive: " + t.isAlive());
        System.out.println("Priority: " + t.getPriority());
        System.out.println("Daemon: " + t.isDaemon());
    }
}
```

```plaintext
Name: custom-thread
ID: 15
Alive: false
Priority: 5
Daemon: false
```

The `Runnable` interface is a functional interface which means we can use lambda
expressions for our `run` method implementation. The following code will give
the exact same output as the previous example.

```java
class Example {
    public static void main(String[] args) {
        Thread t = new Thread(() -> {
            String threadName = Thread.currentThread().getName();
            String msg = String.format("This is %s", threadName);
            System.out.println(msg);
        }, "custom-thread");

        System.out.println("Name: " + t.getName());
        System.out.println("ID: " + t.getId());
        System.out.println("Alive: " + t.isAlive());
        System.out.println("Priority: " + t.getPriority());
        System.out.println("Daemon: " + t.isDaemon());
    }
}
```

Note that you can pass a custom name when calling the `Thread` constructor. Both
of the threads have the `Alive` status set to `false`. Let’s look at how we can
start the threads and run the code defined in the `run` methods.

## Starting Threads

We can use the `start` method on thread instances to start a thread. The `run`
method isn’t immediately invoked when the `start` method is called. The `run`
method is invoked automatically by the JVM.

If you start multiple threads, the order of execution isn’t guaranteed either.
The statements inside a single thread does execute sequentially though.

```java
class ExtendedThread extends Thread {
    @Override
    public void run() {
        String msg = String.format("This is %s", getName());
        System.out.println(msg);
    }
}

class RunnableThread implements Runnable {
    @Override
    public void run() {
        String threadName = Thread.currentThread().getName();
        String msg = String.format("This is %s", threadName);
        System.out.println(msg);
    }
}

class Example {
    public static void main(String[] args) {
        Thread t1 = new ExtendedThread();
        t1.setName("Extended Thread");
        Thread t2 = new Thread(new RunnableThread(), "Runnable Thread");

        t1.start();
        t2.start();
        System.out.println("Done");
    }
}
```

```plaintext
Done
This is Extended Thread
This is Runnable Thread
```

This is just one of the possible outputs of the above code. Do not rely on the
execution order of statements across different threads unless you’ve taken
special measures to handle any shared data.

## Conclusion

We’ve learned how to create and start our own custom threads. In the next
lessons, we’ll learn more about how to manage the threads and also look at how
to safely manage shared data among threads.
