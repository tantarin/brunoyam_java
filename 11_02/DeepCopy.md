 По умолчанию, клонирование в Java является поверхностным, т.е. Object class не знает о структуре класса, которого он копирует.
 
Если класс имеет только члены примитивных типов, то будет создана совершенно новая копия объекта и возвращена ссылка на этот объект.
Если класс содержит не только члены примитивных типов, а и любого другого типа класса, тогда копируются ссылки на объекты этих классов. Следовательно, оба объекта будут иметь одинаковые ссылки.

**Глубокое клонирование требует выполнения следующих правил:**

Нет необходимости копировать отдельно примитивные данные;

Все классы-члены в оригинальном классе должны поддерживать клонирование. Для каждого члена класса должен вызываться super.clone() при переопределении метода clone();

Если какой-либо член класса не поддерживает клонирование, то в методе клонирования необходимо создать новый экземпляр этого класса и скопировать каждый его член со всеми атрибутами в новый объект класса, по одному.

**Для клонирования объекта в Java можно воспользоваться тремя способами:**

-Переопределение метода clone() и реализация интерфейса Cloneable().

-Использование конструктора копирования.

-Использовать для клонирования механизм сериализации.

Реализация по умолчанию Object.clone() метод возвращает Неглубокое копирование.

При неглубоком копировании, если значение поля является примитивным типом, оно копирует свое значение; в противном случае, если значение поля является ссылкой на объект, оно копирует ссылку и, следовательно, ссылается на тот же объект. Теперь, если один из этих объектов изменен, изменение будет видно в другом.

В Глубоком копирование объекты, на которые есть ссылки, не являются общими; вместо этого новые объекты создаются для любых объектов, на которые есть ссылки.

```
import java.util.Arrays;
import java.util.HashMap;
import java.util.HashSet;
import java.util.Set;
import java.util.Map;
 
// `Subject` также должен реализовать `Cloneable`!!
class Subject implements Cloneable
{
    private Set<String> subjects;
 
    public Subject()
    {
        subjects = new HashSet<>(
                Arrays.asList("Maths", "Science", "English", "History")
        );
    }
 
    @Override
    public Subject clone() throws CloneNotSupportedException {
        Subject obj = (Subject)super.clone();
        obj.subjects = new HashSet<>(this.subjects);
 
        return obj;
    }
 
    @Override
    public String toString() {
        return subjects.toString();
    }
 
    public Set<String> getSubjects() {
        return subjects;
    }
}
 
// Обратите внимание, что `Student` реализует интерфейс `Cloneable`
class Student implements Cloneable
{
    private String name;        // неизменяемое поле
    private int age;            // примитивное поле
 
    private Subject subjects;
    private Map<String, Integer> map;
 
    public Student(String name, int age)
    {
        this.name = name;
        this.age = age;
 
        map = new HashMap<String, Integer>() {{
            put(name, age);
        }};
 
        subjects = new Subject();
    }
 
    @Override
    public String toString()
    {
        return Arrays.asList(name, String.valueOf(age),
                subjects.toString()).toString();
    }
 
    @Override
    public Object clone() throws CloneNotSupportedException {
        Student student = (Student) super.clone();
 
        // примитивные поля типа int не копируются, так как их содержимое
        // уже скопировано
 
        // Строка неизменна
 
        // вызов `clone()` для объекта `Subject`
        student.subjects = this.subjects.clone();
 
        // создаем новый экземпляр `Hashmap`
        student.map = new HashMap<>(this.map);
 
        return student;
    }
 
    public Set<String> getSubjects() { return subjects.getSubjects(); }
    public Map<String, Integer> getMap() { return map; }
 
    // включаем оставшиеся геттеры и сеттеры
}
 
// Демонстрация глубокой копии объектов с помощью метода `clone()`
class Main
{
    // Вспомогательный метод для сравнения двух объектов. Он печатает мелкую копию
    // если оба объекта имеют один и тот же объект; в противном случае он печатает глубокую копию
    public static void compare(Object ob1, Object ob2)
    {
        if (ob1 == ob2) {
            System.out.println("Shallow Copy");
        }
        else {
            System.out.println("Deep Copy");
        }
    };
 
    public static void main(String[] args)
    {
        Student student = new Student("Jon Snow", 22);
 
        Student clone = null;
        try {
            clone = (Student) student.clone();
            System.out.println("Cloned Object: " + clone + '\n');
        } catch (CloneNotSupportedException ex) {
            ex.printStackTrace();
        }
 
        compare(student.getSubjects(), clone.getSubjects());
        compare(student.getMap(), clone.getMap());
 
        // любые изменения, внесенные в карту клона, не отразятся на карте ученика
        clone.getMap().put("John Cena", 40);
        System.out.println(student.getMap());
    }
}
```
