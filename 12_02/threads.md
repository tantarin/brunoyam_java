‌Многопоточность в Java — это выполнение двух или более потоков одновременно для максимального использования центрального процесса.

При помощи многопоточности мы можем выделить в приложении несколько потоков, которые будут выполнять различные задачи одновременно.

Обычные программы на Java работают синхронно: строчки кода выполняются одна за другой в главном потоке. Но можно создать несколько тредов и управлять ими. Допустим, один может просто ждать, пока выполнится другой, а может в это время что-то вычислять.

Когда запускается программа, начинает работать главный поток этой программы. От этого главного потока порождаются все остальные дочерние потоки.

Посмотрим, что покажет программа, если воспользоваться методом Thread.currentThread(), который возвращает ссылку на текущую нить:

```
public class Main{
    public static void main(String[] args){
        System.out.println(Thread.currentThread().getName());
    }
}
```

**Поток можно создать двумя способами:**

-унаследовать класс Thread,

-реализовать интерфейс Runnable. ‌

**1 способ:**

```
public class Main {
    public static void main(String[] args) {
        MyThread myThread = new MyThread();
        System.out.println("Main thread started...");
        myThread.start();
        System.out.println("Main thread finished...");
    }
}

class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Hello, I am " + Thread.currentThread().getName());
    }
}
```

Аналогично созданию одного потока мы можем запускать сразу несколько потоков:

```
public class Main {
    public static void main(String[] args) {
        System.out.println("Main thread started...");
        for(int i=1; i < 6; i++) {
            new MyThread("MyThread " + i).start();
        }
        System.out.println("Main thread finished...");
    }
}

class MyThread extends Thread {
    public MyThread(String name) {
        super(name);
    }

    @Override
    public void run() {
        System.out.println("Hello, I am " + Thread.currentThread().getName());
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

Второй вариант лучше — он более гибкий. Например, если бы MyThread уже наследовал какой-либо класс, то было бы невозможно пойти первым путём, так как Java не поддерживает множественное наследование.

**Синхронизация потоков Java**

В многопоточности Java присутствует асинхронное поведение. Если один поток записывает некоторые данные, а другой в это время их считывает, в приложении может возникнуть ошибка. Поэтому при необходимости доступа к общим ресурсам двум и более потоками используется синхронизация.

В Java есть свои методы для обеспечения синхронизации. Как только поток достигает синхронизированного блока, другие потоки должны ожидать, пока текущий не выйдет из синхронизированного блока. 

Синхронизация достигается в Java использованием зарезервированного слова synchronized. Вы можете использовать его в своих классах определяя синхронизированные методы или блоки.

Это можно написать следующим образом:

```
Synchronized(object)
{  
        //Блок команд для синхронизации
}
```

Давайте рассмотрим следующую задачу: есть банковский аккаунт с некоторым балансом, например, 50 руб. И есть 2 потока, которые хотят снимать оттуда одновременно по 10 руб. 

```
public class Main{
    public static void main(String[] args) {
        Account account = new Account();
        Thread one = new Thread(account);
        Thread two = new Thread(account);
        one.setName("Fred");
        two.setName("Lucy");
        one.start();
        two.start();
    }
}

class Account implements Runnable {
    private int balance = 50;
    public int getBalance() {
        return balance;
    }

    public void withdraw(int amount) { //снять баланс
        balance -= amount;
    }

    public void run() {
        for (int x = 0; x < 5; x++) {
            makeWithdrawal(10);
            if (getBalance() < 0) {
                System.out.println("account is overdrawn!");
            }
        }
    }

    private void makeWithdrawal(int amount) {
        if (getBalance() >= amount) {
            System.out.println(Thread.currentThread().getName()
                    + " is going to withdraw");
            try {
                Thread.sleep(500);
            } catch (InterruptedException ex) {
                ex.printStackTrace();
            }
            withdraw(amount);
            System.out.println(Thread.currentThread().getName()
                    + " completes the withdrawal. The balance is "
                    + getBalance());
        } else {
            System.out.println("Not enough in account for "
                    + Thread.currentThread().getName()
                    + " to withdraw " + getBalance());
        }
    }
}
```

Два потока в предыдущем примере находятся в состоянии гонок. Состояние гонок – это одновременный вызов в потоках исполнения одного и того же метода для того же самого объекта.

Каждый объект в Java имеет ассоциированный с ним монитор. Только один поток исполнения может в одно, и то же время владеть монитором. Все другие потоки исполнения, пытающиеся войти в заблокированный монитор, будут приостановлены до тех пор, пока первый поток не выйдет из монитора. Говорят, что они ожидают монитор. 

Когда выполнение кода доходит до оператора synchronized, монитор объекта счет блокируется, и на время его блокировки монопольный доступ к блоку кода имеет только один поток, который и произвел блокировку (Люси).

После окончания работы блока кода, монитор объекта счет освобождается и становится доступным для других потоков.

После освобождения монитора его захватывает другой поток, а все остальные потоки продолжают ожидать его освобождения.

**Ожидание завершения потоков**

Как правило, более распространенной ситуацией является случай, когда Main thread завершается самым последним. Для этого надо применить метод join(). В этом случае **текущий поток будет ожидать завершения потока, для которого вызван метод join**:

```
System.out.println("Main thread started...");
        MyThread t= new MyThread("MyThread ");
        t.start();
        try{
            t.join(); //main ожидает завершения потока MyThread
        }
        catch(InterruptedException e){

            System.out.printf("%s has been interrupted", t.getName());
        }
        System.out.println("Main thread finished...");
```



Иногда при взаимодействии потоков встает вопрос о извещении одних потоков о действиях других. Например, действия одного потока зависят от результата действий другого потока, и надо как-то известить один поток, что второй поток произвел некую работу. И для подобных ситуаций у класса Object определено ряд методов:

-wait(): освобождает монитор и переводит вызывающий поток в состояние ожидания до тех пор, пока другой поток не вызовет метод notify()

-notify(): продолжает работу потока, у которого ранее был вызван метод wait()

-notifyAll(): возобновляет работу всех потоков, у которых ранее был вызван метод wait()

**Задача**

Пока производитель не произвел продукт, потребитель не может его купить. Пусть производитель должен произвести 5 товаров, соответственно потребитель должен их все купить. Но при этом одновременно на складе может находиться не более 3 товаров.

```
public class Program {
  
    public static void main(String[] args) {
          
        Store store=new Store();
        Producer producer = new Producer(store);
        Consumer consumer = new Consumer(store);
        new Thread(producer).start();
        new Thread(consumer).start();
    }
}
// Класс Магазин, хранящий произведенные товары
class Store{
   private int product=0;
   public synchronized void get() {
      while (product<1) {
         try {
            wait();
         }
         catch (InterruptedException e) {
         }
      }
      product--;
      System.out.println("Покупатель купил 1 товар");
      System.out.println("Товаров на складе: " + product);
      notify();
   }
   public synchronized void put() {
       while (product>=3) {
         try {
            wait();
         }
         catch (InterruptedException e) { 
         } 
      }
      product++;
      System.out.println("Производитель добавил 1 товар");
      System.out.println("Товаров на складе: " + product);
      notify();
   }
}
// класс Производитель
class Producer implements Runnable{
  
    Store store;
    Producer(Store store){
       this.store=store; 
    }
    public void run(){
        for (int i = 1; i < 6; i++) {
            store.put();
        }
    }
}
// Класс Потребитель
class Consumer implements Runnable{
      
     Store store;
    Consumer(Store store){
       this.store=store; 
    }
    public void run(){
        for (int i = 1; i < 6; i++) {
            store.get();
        }
    }
}
```

Оба метода Store - put и get являются синхронизированными.

Рассмотрим еще пример. 

```

public class Main {
    public static void main(String[] args){

        List<Cat> catThreads = new ArrayList<>();

        int life = 9;

        Collections.addAll(catThreads,
                new Cat("Tom", life, "Thread Tom"),
                new Cat("Cleocatra", life, "Thread Cleocatra"),
                new Cat("Dupli", life, "Thread Dupli"),
                new Cat("Toodles", life, "Thread Toodles"));

        for(Cat cat : catThreads)
            cat.getThread().start();

        for(Cat cat : catThreads){
            try{
                cat.getThread().join();
            }catch (InterruptedException e){
                e.printStackTrace();
            }
        }

        System.out.println(String.format("Кот-победитель: %s!!!", Cat.cats.get(0)));
    }
}


class Cat implements Runnable{

    // Класс CopyOnWriteArrayList — тот же ArrayList, только потокобезопасный
    public static final List<Cat> cats = new CopyOnWriteArrayList<>();

    private String name;
    private volatile int life;
    private Thread thread;

    public Cat(String name, int life, String threadName) {

        this.name = name;           // Имя
        this.life = life;           // Количество жизни
        Cat.cats.add(this);         // Добавляем себя в List<Cat> cats
        thread = new Thread(this, threadName);   // Создаём поток этого кота и передаём ему ссылку на себя

        System.out.println(String.format("Кот %s создан. HP: %d", this.name, this.life));
    }

    public static synchronized void attack(Cat thisCat, Cat enemyCat) {

        if (thisCat.getLife() <= 0) { return; }

        // Если противник имеет жизни
        if (enemyCat.getLife() > 0) {
            // Отнимаем жизнь противника
            enemyCat.decrementLife();
            System.out.println(String.format("Кот %s атаковал кота %s. Жизни %<s: %d", thisCat.getName(), enemyCat.getName(), enemyCat.getLife()));

            // Если противник не имеет жизней
            if (enemyCat.getLife() <= 0) {
                // Удаляем противника из списка котов
                Cat.cats.remove(enemyCat);

                System.out.println(String.format("Кот %s покидает бой.", enemyCat.getName()));
                System.out.println(String.format("Оставшиеся коты: %s", Cat.cats));
                System.out.println(String.format("%s завершает свою работу.", enemyCat.getThread().getName()));
                // interrupt() — прервать работу треда
                enemyCat.getThread().interrupt();
            }
        }
    }

    // Точка входа в поток
    @Override
    public void run() {
        System.out.println(String.format("Кот %s идёт в бой.", name));

        // Пока котов больше 1
        while (Cat.cats.size() > 1){
            // Атакуем произвольного кота из оставшихся, кроме себя
            Cat.attack(this, getRandomEnemyCat(this));
        }
    }

    // Возвращает произвольный объект Cat из cats, кроме самого себя
    private Cat getRandomEnemyCat(Cat deleteThisCat) {

        // Создаём лист-копию из основного листа cats
        List<Cat> copyCats = new ArrayList<>(Cat.cats);
        // Удаляем текущего кота, чтобы он не выпал в качестве противника
        copyCats.remove(deleteThisCat);
        // Возвращаем произвольного кота из оставшихся с помощью класса util.java.Random
        return copyCats.get(new Random().nextInt(copyCats.size()));
    }

    public synchronized void decrementLife() { life--; }

    @Override
    public String toString() { return name; }

    public String getName() { return name; }
    public int getLife() { return life; }
    public Thread getThread() { return thread; }
}
```




