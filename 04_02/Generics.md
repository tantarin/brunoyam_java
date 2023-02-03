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
  
  
