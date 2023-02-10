Сериализация представляет процесс записи состояния объекта в поток, соответственно процесс извлечения или восстановления состояния объекта из потока называется десериализацией. Сериализация очень удобна, когда идет работа со сложными объектами.

Сериализовать можно только те объекты, которые реализуют интерфейс Serializable. Этот интерфейс не определяет никаких методов, просто он служит указателем системе, что объект, реализующий его, может быть сериализован.

Для сериализации объектов в поток используется класс ObjectOutputStream. Он записывает данные в поток.

```
import java.io.*;
 
public class Program {
 
    public static void main(String[] args) {
         
        try(ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("person.dat")))
        {
            Person p = new Person("Sam", 33, 178, true);
            oos.writeObject(p);
        }
        catch(Exception ex){
             
            System.out.println(ex.getMessage());
        } 
    } 
}
class Person implements Serializable{
      
    private String name;
    private int age;
    private double height;
    private boolean married;
      
    Person(String n, int a, double h, boolean m){
          
        name=n;
        age=a;
        height=h;
        married=m;
    }
    String getName() {return name;}
    int getAge(){ return age;}
    double getHeight(){return height;}
    boolean getMarried(){return married;}
}
```

Больше всего впечатляет то, что весь процесс независим от JVM (виртуальной машины Java), то есть объект может быть сериализован на одной платформе и десериализован на совершенно другой платформе.

Класс ObjectOutputStream содержит много методов записи для осуществления записи различных типов данных, но среди них в особенности выделяется один метод:

```
public final void writeObject(Object x) throws IOException
```

Вышеуказанный метод осуществляет сериализацию Объекта и отправляет его в поток выходных данных. Аналогично, класс ObjectInputStream содержит следующий метод осуществления десериализации объекта:

```
public final Object readObject() throws IOException, ClassNotFoundException
```

Этим методом извлекается следующий Объект из потока данных и осуществляется его десериализация. Возвращенное значение - это Объект, поэтому необходимо привести его к соответствующему типу данных.

```
public class Employee implements java.io.Serializable {
   public String name;
   public String address;
   public transient int SSN;
   public int number;
   
   public void mailCheck() {
      System.out.println("Отправка чека на " + name + " " + address);
   }
}
```

Обратите внимание, что для успешного выполнения сериализации класса должны быть выполнены два условия:

-Класс должен реализовывать интерфейс java.io.Serializable.

-Все поля в классе должны быть сериализуемыми. Если поле не сериализуемо, оно должно быть помечено как промежуточное.

Если вам интересно узнать, можно ли выполнить сериализацию стандартного класса Java, проверьте документацию по данному классу.

В Java при сериализации объекта в файл установленным архитектурным требованием Java является присвоение файлу расширения .ser.

```
import java.io.*;
public class SerializeDemo {

   public static void main(String [] args) {
      Employee e = new Employee();
      e.name = "Анастасия Крот";
      e.address = "Москва, Россия";
      e.SSN = 11122333;
      e.number = 101;
      
      try {
         FileOutputStream fileOut =
         new FileOutputStream("/tmp/employee.ser");
         ObjectOutputStream out = new ObjectOutputStream(fileOut);
         out.writeObject(e);
         out.close();
         fileOut.close();
         System.out.printf("Сериализованные данные сохраняются в /tmp/employee.ser");
      } catch (IOException i) {
         i.printStackTrace();
      }
   }
}
```

