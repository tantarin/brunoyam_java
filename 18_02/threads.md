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
public class Main {
    public static void main(String[] args) throws InterruptedException {
        BankAccount ba = new BankAccount(100);
        var t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                ba.deposit(100);
            }

        });

        var t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                ba.withdraw(100);
            }
        });

        t1.start();
        t2.start();

        t1.join();
        t2.join();

        System.out.println(ba);
    }
}

class BankAccount {
    private long balance;

    public BankAccount(long balance) {
        this.balance = balance;
    }

    public void withdraw(long amount) {
        long newBalance = balance - amount;
        balance = newBalance;
    }

    public void deposit(long amount) {
        long newBalance = balance + amount;
        balance = newBalance;
    }

    @Override
    public String toString() {
        return String.valueOf(balance);
    }
}
```

В коде выше мы создали экземпляр класса BankAccount с начальным балансом 100. Затем создали два потока, один из которых увеличивает счет, а другой наоборот — снимает средства со счета. Оба они выполняют свою операцию внутри цикла по 1000 раз.
Хоть и ожидается, что баланс измениться не должен, но это не так, причем конечное значение будет меняться при каждом запуске программы: иногда отрицательный, а иногда положительный, но он практически никогда не будет соответствовать первоначальному балансу.

Переменная balance используется совместно в обоих потоках. Когда один поток изменяет/обновляет переменную, то для этого он выполняет несколько операций: в начале читает, а затем записывает.
Если поток читает до того, как другой поток закончит свою запись, то вот тут все и выходит из-под контроля.

Когда поток считывает и записывает общую переменную, мы должны защитить ее блокировкой, чтобы никакой другой поток не мог получить к ней доступ во время этих операций.
Область в коде, которая считывает и записывает в переменную, называется критическим разделом. 

Два потока в предыдущем примере находятся в состоянии гонок. Это состояние возникает, когда два или более потока из одного процесса одновременно обращаются к одной и той же ячейке памяти, при этом какие-то из них считывают из нее данные, а какие-то записывают, что и приводит к разным результатам от запуска к запуску программы.

Его можно предотвратить, сохраняя критические значения внутри специального блока кода, называемого синхронизированным (synchronized block). Для его создания служит ключевое слово synchronized с объектом блокировки (в качестве аргумента у этого слова). 

```
    public synchronized void withdraw(long amount) {
        long newBalance = balance - amount;
        balance = newBalance;
    }

    public synchronized void deposit(long amount) {
        long newBalance = balance + amount;
        balance = newBalance;
    }
```


***volatile***

Поток создается с чистой рабочей памятью, и должен перед использованием загрузить все необходимые переменные из основного хранилища (можно сказать что он имеет некий кэш).

Любая переменная сначала создается в основном хранилище и лишь затем копируется в рабочую память потоков, которые будут ее применять.

Если переменная объявлена, как volatile, то ее чтение и запись будет производиться из\в основное хранилище.
    
Для решения проблемы состояния гонки можно использовать ключевое слово volatile. Если поле является final, то в volatile нет необходимости. Это происходит потому, что final-поля никогда не меняются, а значит не могут создать озвученных ранее проблем. Хотя ключевое слово volatile может быть хорошим решением, его чрезмерное использование может вызвать проблемы, т. к. оно запрещает кэширование данных процессором, что, безусловно, снижает производительность программы и препятствует дальнейшей оптимизации.

В Java операции чтения и записи полей всех типов, кроме long и double, являются атомарными. Например, если ты в одном потоке меняешь значение переменной int, а в другом потоке читаешь значение этой переменной, ты получишь либо ее старое значение, либо новое — то, которое получилось после изменения в потоке 1. Никаких «промежуточных вариантов» там появиться не может. Соответственно, в этих случаях может возникнуть проблема. Один поток записывает какое-то 64-битное значение в переменную Х, и делает он это «в два захода». В то же время второй поток пытается прочитать значение этой переменной, причем делает это как раз посередине, когда первые 32 бита уже записаны, а вторые — еще нет. В результате он читает промежуточное, некорректное значение, и получается ошибка. 

Если мы объявляем в нашей программе какую-то переменную, со словом volatile это означает, что:

1) Она всегда будет атомарно читаться и записываться. Даже если это 64-битные double или long.

2) Java-машина не будет помещать ее в кэш. Так что ситуация, когда 10 потоков работают со своими локальными копиями исключена.

Однако с long и double это не работает.

Ключевое слово synchronized само по себе устраняет проблему видимости. Это означает, что переменная всегда считывается или записывается в основную память, в то время как volatile обеспечивает только считывание из основной памяти.

```
public class Main {
    private static volatile int MY_INT = 0;
    public static void main(String[] args) {
        new ChangeListener().start();
        new ChangeMaker().start();
    }
    static class ChangeListener extends Thread {
        @Override
        public void run() {
            int local_value = MY_INT;
            while ( local_value < 5){
                if( local_value!= MY_INT){
                    System.out.println("Got Change for MY_INT : " + MY_INT);
                    local_value= MY_INT;
                }
            }
        }
    }
    static class ChangeMaker extends Thread{
        @Override
        public void run() {
            int local_value = MY_INT;
            while (MY_INT <5){
                local_value++;
                System.out.println("Incrementing MY_INT to " + local_value);
                MY_INT = local_value;
                try {
                    Thread.sleep(500);
                } catch (InterruptedException e) { e.printStackTrace(); }
            }
        }
    }
}
```

Каждый объект в Java имеет ассоциированный с ним монитор. Только один поток исполнения может в одно, и то же время владеть монитором. Все другие потоки исполнения, пытающиеся войти в заблокированный монитор, будут приостановлены до тех пор, пока первый поток не выйдет из монитора. Говорят, что они ожидают монитор. 

Когда выполнение кода доходит до оператора synchronized, монитор объекта счет блокируется, и на время его блокировки монопольный доступ к блоку кода имеет только один поток, который и произвел блокировку (Люси).

После окончания работы блока кода, монитор объекта счет освобождается и становится доступным для других потоков.

После освобождения монитора его захватывает другой поток, а все остальные потоки продолжают ожидать его освобождения.

**Ожидание завершения потоков**

Как правило, более распространенной ситуацией является случай, когда Main thread завершается самым последним. Для этого надо применить метод join(). В этом случае **текущий поток будет ожидать завершения потока, для которого вызван метод join**:

```
public static void main(String[] args) {
         
    System.out.println("Main thread started...");
    for(int i=1; i < 6; i++)
        new JThread("JThread " + i).start();
    System.out.println("Main thread finished...");
}
```

Main thread завершался до дочернего потока. Как правило, более распространенной ситуацией является случай, когда Main thread завершается самым последним. Для этого надо применить метод join(). В этом случае текущий поток будет ожидать завершения потока, для которого вызван метод join:

```
public static void main(String[] args) {
         
    System.out.println("Main thread started...");
    JThread t= new JThread("JThread ");
    t.start();
    try{
        t.join(); 
    }
    catch(InterruptedException e){
     
        System.out.printf("%s has been interrupted", t.getName());
    }
    System.out.println("Main thread finished...");
}
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

***yield***

Когда мы вызываем метод yield у потока, он фактически говорит другим потокам: «Так, ребята, я никуда особо не тороплюсь, так что если кому-то из вас важно получить время процессора — берите, мне не срочно».

```
public class ThreadExample extends Thread {

   public ThreadExample() {
       this.start();
   }

   public void run() {

       System.out.println(Thread.currentThread().getName() + " уступает свое место другим");
       Thread.yield();
       System.out.println(Thread.currentThread().getName() + " has finished executing.");
   }

   public static void main(String[] args) {
       new ThreadExample();
       new ThreadExample();
       new ThreadExample();
   }
}
```


