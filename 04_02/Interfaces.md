
Интерфейс это конструкция языка Java, в рамках которой принято описывать абстрактные публичные (abstract public) методы и статические константы (final static).

С помощью интерфейса можно указать, что именно должен выполнять класс его реализующий.

Интерфейсы - это один из механизмов реализации принципа полиморфизма "один интерфейс, несколько методов". 

Переменные интерфейса являются public static final по умолчанию и эти модификаторы необязательны при их объявлении. Например, в следующем примере объявлены переменные RIGHT, LEFT, UP, DOWN без каких-либо модификаторов. Но они будут public static final.

```
public interface Moveable {
    int RIGHT = 1;
    int LEFT = 2;
    int UP = 3;
    int DOWN = 4;

    void moveRight();

    void moveLeft();
}
```
Чтобы указать, что данный класс реализует интерфейс, в строке объявления класса указываем ключевое слово implements и имя интерфейса. 

Класс реализующий интерфейс должен содержать полный набор методов, определенных в этом интерфейсе.

Если класс реализует интерфейс, но не полностью реализует определенные в нем методы, он должен быть объявлен как abstract. 

Тип интерфейса можно указывать при объявлении переменных, содержащих ссылки на объекты, классы которых реализуют этот интерфейс. 

```
Moveable moveable = new Transport();
Transport transport = new Transport();
Moveable robot = new Robot();
 ```
 
Один класс может реализовать любое количество интерфейсов. Для указания того, что класс реализует несколько интерфейсов, после ключевого слова implements через запятую перечисляются нужные интерфейсы. 

Интерфейс может наследоваться от другого интерфейса через ключевое слово extends. Один интерфейс, в отличие от классов, может расширять несколько интерфейсов.
```
public interface Football extends Sport, TVProgram {
    void homeTeamScored(int points);

    void visitingTeamScored(int points);

    void endOfQuarter(int quarter);
}
```

**Интерфейсы маркеры**

Интерфейсы маркеры - это интерфейсы, у которых не определены ни методы, ни переменные. Реализация этих интерфейсов придает классу определенные свойства. Например, интерфейсы Cloneable и Serializable, отвечающие за клонирование и сохранение объекта в информационном потоке, являются интерфейсами маркерами. Если класс реализует интерфейс Cloneable, это говорит о том, что объекты этого класса могут быть клонированы.

**Методы по умолчанию в интерфейсах**

В JDK 8 в интерфейсы ввели методы по умолчанию - это методы, у которых есть реализация. Другое их название - методы расширения. Классы, реализующие интерфейсы, не обязаны переопределять такие методы, но могут если это необходимо. Методы по умолчанию определяются с ключевым словом default.

```
public interface SomeInterface {
    default String defaultMethod() {
        return "Объект типа String по умолчанию";
    }
}
```

**Статические методы интерфейса**

В версии JDK 8, в интерфейсы добавлена еще одна возможность - определять в нем статические методы. Статические методы интерфейса, как и класса, можно вызывать независимо от любого объекта. Для вызова статического метода достаточно указать имя интерфейса и через точку имя самого метода.

```
MyIf.staticMethod()
```

**Функциональный интерфейс**

Функциональный интерфейс — это интерфейс, который содержит ровно один абстрактный метод, то есть описание метода без тела. Статические методы и методы по умолчанию при этом не в счёт, их в функциональном интерфейсе может быть сколько угодно.

```
@FunctionalInterface
public interface ToIntBiFunction<T, U> {

   /**
    * Applies this function to the given arguments.
    *
    * @param t the first function argument
    * @param u the second function argument
    * @return the function result
    */
   int applyAsInt(T t, U u);
}
```

```
//Определяем свой функциональный интерфейс
@FunctionalInterface
interface MyPredicate {
    boolean test(Integer value);
}

public class Tester {
    public static void main(String[] args) throws Exception {
        MyPredicate myPredicate = x -> x > 0;
        System.out.println(myPredicate.test(10));   //true

        //Аналогично, но используется встроенный функциональный интерфейс java.util.function.Predicate
        Predicate<Integer> predicate = x -> x > 0;
        System.out.println(predicate.test(-10));    //false
    }
}
```

**Лямбда**

По сути, это анонимный (без имени) класс или метод. 

Не будь лямбд, вызывать метод processTwoNumbers каждый раз приходилось бы так:

```
ToIntBiFunction<Integer, Integer> biFunction = new ToIntBiFunction<>() {
    @Override
    public int applyAsInt(Integer a, Integer b) {
        return a + b;
    }
};

processTwoNumbers(1, 2, biFunction);
```

biFunction в примере создана с использованием анонимных классов. Без этого нам пришлось бы создавать класс, реализующий интерфейс ToIntBiFunction, и объявлять в этом классе метод applyAsInt. А с анонимным классом мы всё это сделали на лету.

А наша лямбда будет такой:

```
ToIntBiFunction<Integer, Integer> biFunction = (a, b) -> a + b;

processTwoNumbers(1, 2, (a, b) -> a+b);
```

Где применяют лямбды?

Допустим, нужно отсортировать коллекцию по последней букве каждого слова:

```
List<String> colors = Arrays.asList("Black", "White", "Red");
Collections.sort(colors, (o1, o2) -> {
   String o1LastLetter = o1.substring(o1.length() - 1);
   String o2LastLetter = o2.substring(o2.length() - 1);
   return o1LastLetter.compareTo(o2LastLetter);
});
```


