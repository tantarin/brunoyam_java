```
List<String> list = new ArrayList<>(3);   //Это «угадывание» называется type inference — выведение типа, а оператор «<>» — это diamond operator.
//error
list.add(new Integer(1));
```

Создаем дженерик класс:

```
class Box<T> { // обозначение типа - T
   // переменная с типом T
   private T item;
 
   public void putItem(T item) { //параметр метода типа T
      this.item = item;
   }

   public T getItem() { // возвращает объект типа T
     return item;
   }
}
```

Примитивные типы и массивы с дженериками не используются, то есть нельзя создать Box<int> или Box<int[]>, но можно — Box<Integer> или Box<List<Integer>>.
  
Давайте рассмотрим пример:

```
abstract class Garbage{
   public abstract double getWeight();
}

class Paper extends Garbage{
   @Override
   public double getWeight() {
       return 0.01;
   }
}

class Plastic extends Garbage{
   @Override
   public double getWeight() {
       return  0.3;
   }
}
   
class Box<T extends Garbage> {

   private T item;

   public double getItemWeight() {
       // не скомпилируется
       return item == null ? 0 : item.getWeight();
   }
//... остальные методы
}
```
  
Здесь T extends Garbage означает, что в качестве T можно подставить Garbage или любой класс-наследник Garbage. Из уже известных нам классов это могут быть, например, Paper или Plastic.
   
Но можно ограничить тип и снизу. Это называется lower bounding и выглядит так:
   
```
List<? super Garbage> example3 = new ArrayList<Garbage>();
```
   
**Wildcards**
   
 В Java есть и специальный символ для обозначения неизвестного типа — «?». Его принято называть wildcard, дословно — «дикая карта».

```
// Java program to demonstrate Upper Bounded Wildcards

import java.util.Arrays;
import java.util.List;

class WildcardDemo {
	public static void main(String[] args)
	{

		// Upper Bounded Integer List
		List<Integer> list1 = Arrays.asList(4, 5, 6, 7);

		// printing the sum of elements in list
		System.out.println("Total sum is:" + sum(list1));

		// Double list
		List<Double> list2 = Arrays.asList(4.1, 5.1, 6.1);

		// printing the sum of elements in list
		System.out.print("Total sum is:" + sum(list2));
	}

	private static double sum(List<? extends Number> list)
	{
		double sum = 0.0;
		for (Number i : list) {
			sum += i.doubleValue();
		}

		return sum;
	}
}
```

![](/images/wild.jpg)
