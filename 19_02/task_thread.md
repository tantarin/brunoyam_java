```
public class Main {
    public static void main(String[] args) {
        Object lock = new Object();
        new StepThread(lock).start();
        new StepThread(lock).start();
    }
}

class StepThread extends Thread {
    private Object lock;

    public StepThread(Object lock) {
        this.lock = lock;
    }

    @Override
    public void run() {
        int i = 0;
        while (i < 6) {
            synchronized (lock) {
                System.out.println(getName());
                lock.notify();
                try {
                    lock.wait();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
            i++;
        }
    }
}
```
