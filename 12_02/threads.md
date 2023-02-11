‌Обычные программы на Java работают синхронно: строчки кода выполняются одна за другой в главном потоке. Но можно создать несколько тредов и управлять ими. Допустим, один может просто ждать, пока выполнится другой, а может в это время что-то вычислять.

Поток можно создать двумя способами:

-унаследовать класс Thread,

-реализовать интерфейс Runnable. ‌

**1 способ:**

```
class MyThread extends Thread{
    @Override
    public void run(){
        System.out.println("Hello, I’m " + Thread.currentThread());
    }
}
public class Main{
    public static void main(String[] args){
        MyThread myThread = new MyThread();
        myThread.start();
    }
}
```

Вся логика нового треда выполняется в методе run(), а запускается он методом start().

Обратите внимание: если на экземпляре класса Thread вместо метода start() вызвать run(), то код, написанный для другого потока, отлично выполнится, но выполнит его тот же тред, который и вызвал этот метод, а новый запущен не будет! Поэтому нужно пользоваться методом start().

**2 способ:**

```
class MyThread implements Runnable{
    @Override
    public void run(){
        System.out.print("Hello, I’m " + Thread.currentThread().getName());
    }
}

public class Main{
    public static void main(String[] args){
        // Первый параметр: экземпляр Runnable
        // Второй параметр: своё имя (необязательно) 
        Thread myThread = new Thread(new MyThread(), "Leo");
        myThread.start();
    }
}
```

Посмотрим, что покажет программа, если воспользоваться методом Thread.currentThread(), который возвращает ссылку на текущую нить:

```
public class Main{
    public static void main(String[] args){
        System.out.println(Thread.currentThread().getName());
    }
}
```





