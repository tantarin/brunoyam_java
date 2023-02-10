
Интерфейс java.util.Stream представляет собой последовательность элементов, над которой можно производить различные операции.

Операции над стримами бывают или промежуточными (intermediate) или конечными (terminal). Конечные операции возвращают результат определенного типа, а промежуточные операции возвращают тот же стрим. Таким образом вы можете строить цепочки из несколько операций над одним и тем же стримом.

У стрима может быть сколько угодно вызовов промежуточных операций и последним вызов конечной операции. При этом все промежуточные операции выполняются лениво и пока не будет вызвана конечная операция никаких действий на самом деле не происходит.

Потоки не могут быть использованы повторно. Как только была вызвана какая-нибудь конечная операция, поток закрывается.

**Какие существуют способы создания стрима?**

```
Stream<String> fromCollection = Arrays.asList("x", "y", "z").stream();

Stream<String> fromValues = Stream.of("x", "y", "z");

Stream<String> fromArray = Arrays.stream(new String[]{"x", "y", "z"});
```

Коллекции позволяют работать с элементами по отдельности, тогда как стримы так делать не позволяют, но вместо этого предоставляют возможность выполнять функции над данными как над одним целым.

Метод collect() является конечной операцией, которая используется для представление результата в виде коллекции или какой-либо другой структуры данных.

Метод map() является промежуточной операцией, которая заданным образом преобразует каждый элемент стрима.

mapToInt(), mapToDouble(), mapToLong() - аналоги map(), возвращающие соответствующий числовой стрим (то есть стрим из числовых примитивов):

```
Stream
    .of("12", "22", "4", "444", "123")
    .mapToInt(Integer::parseInt)
    .toArray(); //[12, 22, 4, 444, 123]
 ```

Метод count() возвращает количество элементов в потоке данных:

```
import java.util.stream.Stream;
import java.util.Optional;
import java.util.*;
public class Program {
 
    public static void main(String[] args) {
         
        ArrayList<String> names = new ArrayList<String>();
        names.addAll(Arrays.asList(new String[]{"Tom", "Sam", "Bob", "Alice"}));
        System.out.println(names.stream().count());  // 4
         
        // количество элементов с длиной не больше 3 символов
        System.out.println(names.stream().filter(n->n.length()<=3).count());  // 3
    } 
}
```

allMatch, anyMatch, noneMatch

```
import java.util.stream.Stream;
import java.util.Optional;
import java.util.ArrayList;
import java.util.Arrays;
public class Program {
 
    public static void main(String[] args) {
         
        ArrayList<String> names = new ArrayList<String>();
        names.addAll(Arrays.asList(new String[]{"Tom", "Sam", "Bob", "Alice"}));
         
        // есть ли в потоке строка, длина которой больше 3
        boolean any = names.stream().anyMatch(s->s.length()>3);
        System.out.println(any);    // true
         
        // все ли строки имеют длину в 3 символа
        boolean all = names.stream().allMatch(s->s.length()==3);
        System.out.println(all);    // false
         
        // НЕТ ЛИ в потоке строки "Bill". Если нет, то true, если есть, то false
        boolean none = names.stream().noneMatch(s->s=="Bill");
        System.out.println(none);   // true
    } 
}
```

Как можно вывести на экран уникальные квадраты чисел используя метод map()?

```
Stream
    .of(1, 2, 3, 2, 1)
    .map(s -> s * s)
    .distinct()
    .forEach(System.out::println);
 ```
 
 Как вывести на экран количество пустых строк с помощью метода filter()?
 
 ```
 System.out.println(
    Stream
        .of("Hello", "", ", ", "world", "!")
        .filter(String::isEmpty)
        .count());
 ```
 
 

