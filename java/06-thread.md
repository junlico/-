## Create Thread

1. Extends `Thead`

   ```java
   public class Main {
       public static void main(String[] args) {
           Thread t = new MyThread();
           t.start();
       }
   }

   class MyThread extends Thread {
       @Override
       public void run() {
           System.out.println("start new thread!");
       }
   }
   ```

2. `Runnable` instance

   ```java
   public class Main {
       public static void main(String[] args) {
           Thread t = new Thread(new MyRunnable());
           t.start();
       }
   }

   class MyRunnable implements Runnable {
       @Override
       public void run() {
           System.out.println("start new thread!");
       }
   }
   ```

   lambda

   ```java
   public class Main {
       public static void main(String[] args) {
           Thread t = new Thread(() -> {
               System.out.println("start new thread!");
           });
           t.start();
       }
   }
   ```

## Synchronized

Atomic Access don't need synchronized, (single line)

- Reads and writes are atomic for `reference` variables and for most `primitive` variables (all types except long and double).
- Reads and writes are atomic for all variables declared `volatile` (including long and double variables).

```java
public class Counter {
    private int count = 0;

    public void add(int n) {
        synchronized(this) {
            count += n;
        }
    }

    public synchronized void dec(int n) {
        count -= n;
    }

    public int getCount() {
        return count;
    }
}
```

## Dead lock

Avoid dead lock by using the same order of locks

## wait, notify

```java
class TaskQueue {
    Queue<String> queue = new LinkedList<>();

    public synchronized void addTask(String s) {
        this.queue.add(s);
        this.notifyAll();
    }

    public synchronized String getTask() throws InterruptedException {
        while (queue.isEmpty()) {
            this.wait();
        }

        return queue.remove();
    }
}
```

## ReentrantLock

simplify `synchronized`

```java
public class Counter {
    private final Lock lock = new ReentrantLock();
    private int count = 0;

    public void add(int n) {
        lock.lock();
        try {
            count += n;
        } finally {
            lock.unlock();
        }
    }
}
```

### tryLock

```java
if (lock.tryLock(1, TimeUnit.SECONDS)) {
    try {
        ...
    } finally {
        lock.unlock();
    }
}
```

didn't get lock longer than 1 sec, `tryLock()` will return false, so not wait forever, avoid dead lock.

## Condition

```java
class TaskQueue {
    private final Lock lock = new ReentrantLock();
    private final Condition condition = lock.newCondition();
    private final Queue<String> queue = new LinkedList<>();

    public void addTask(String s) {
        lock.lock();
        try {
            queue.add(s);
            condition.signalAll();
        } finally {
            lock.unlock();
        }
    }

    public String getTask() throws InterruptedException {
        lock.lock();
        try {
            while (queue.isEmpty()) {
                condition.await();
            }
            return queue.remove();
        } finally {
            lock.unlock();
        }
    }
}
```

## ReadWriteLock

Allow multi-threads read, only one thread write.

```java
class Counter {
    private final ReadWriteLock readWriteLock = new ReentrantReadWriteLock();
    private final Lock readLock = readWriteLock.readLock();
    private final Lock writeLock = readWriteLock.writeLock();
    private final int[] counts = new int[10];

    public void inc(int index) {
        writeLock.lock();
        try {
            counts[index] += 1;
        } finally {
            writeLock.unlock();
        }
    }

    public int[] getCount() {
        readLock.lock();
        try {
            return Arrays.copyOf(counts, counts.length);
        } finally {
            readLock.unlock();
        }
    }
}
```

## StampedLock

```java
class Point {
    private final StampedLock stampedLock = new StampedLock();

    private double x;
    private double y;

    public void move(double deltaX, double deltaY) {
        long stamp = stampedLock.writeLock();
        try {
            x += deltaX;
            y += deltaY;
        } finally {
            stampedLock.unlockWrite(stamp);
        }
    }

    public double distanceFromOrigin() {
        long stamp = stampedLock.tryOptimisticRead();

        double currentX = x;
        double currentY = y;

        if (!stampedLock.validate(stamp)) {
            stamp = stampedLock.readLock();
            try {
                currentX = x;
                currentY = y;
            } finally {
                stampedLock.unlockRead(stamp);
            }
        }

        return Math.sqrt(currentX * currentX + currentY * currentY);
    }
}
```
