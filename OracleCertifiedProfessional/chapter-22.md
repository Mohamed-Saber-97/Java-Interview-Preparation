# Concurrency: Part I

## Runtime Organization for Thread Execution

![](/images/RunTimeDataAreas.png "Runtime Data Areas")

- JVM stack: A JVM stack (also known as execution stack, call stack, or frame stack) is created for each thread when the thread starts, and is used for bookkeeping method executions in the thread. This is also where all local variables for each active method invocation are stored. Note that each thread takes care of its own exception handling, and thus does not affect other threads.
- Program counter (PC): This register is created for each thread when the thread starts, and stores the address of the JVM instruction currently being executed
- Heap: This shared memory space is where objects are created, stored, and garbage collected
- Method area: This is created when the JVM starts. It stores the runtime constant pool, field and method information, static variables, and method bytecode for each of the classes and interfaces loaded by the JVM
- Runtime constant pool: In addition to storing the constants defined in each class and interface, this also stores references to all methods and fields. The JVM uses the information in this pool to find the actual address of a method or a field in memory

## Creating Threads

- Implementing the task performed by a thread is achieved in one of two ways:
    - Implementing the functional interface `java.lang.Runnable`
    - Extending the `java.lang.Thread` class

### Implementing the `Runnable` Interface

```java

@FunctionalInterface
public interface Runnable {
    void run();
}
```

```java
void test() {
    Thread thread = new Thread(() -> System.out.println("Harmonious threads create beautiful applications."));
    thread.start();//Call returns immediately
}
```

- `Thread(Runnable threadTarget)`, `Thread(Runnable threadTarget, String threadName)`
- `void start()` new thread will begin execution as a child thread of the current thread. throws an `IllegalThreadStateException` if the thread is already started, or it has already completed execution
- `void run()`
- `static Thread currentThread()`
- `final String getName()`
- `final void setName(String name)`
- `final void setDaemon(boolean flag)`
- `final boolean isDaemon()`

## Thread Lifecycle

### Objects, Monitors, and Locks

- Java, each object has a monitor associated with it—including arrays. A thread can lock and unlock a monitor, but only one thread at a time can acquire the lock on a monitor.
- `static boolean holdsLock(Object obj)`

### Thread States

![](/images/ThreadStates.png "Thread States")

- `final boolean isAlive()`
- `Thread.State getState()`
- `final int getPriority()`
- `final void setPriority(int newPriority)`
- `static void yield()` current thread to temporarily pause its execution and, thereby, allow other threads to execute
- `static void sleep(long millisec) throws InterruptedException`
- `static void sleep(long millis, int nanos) throws InterruptedException`
- `final void join() throws InterruptedException`
- `final void join(long millisec) throws InterruptedException`
- `static Map<Thread, StackTraceElement[]> getAllStackTraces()`
