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
