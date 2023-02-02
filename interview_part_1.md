1. Что такое конструктор?
2. Какие существуют модификаторы доступа?
3. На какие основные группы можно поделить типы данных?
4. Какие примитивные типы вы знаете?
5. **Что Вы знаете о массивах?**

В Java массивы являются объектами.
Массив  — это контейнер для хранения однотипных значений, количество которых было задано заранее.
Пример создания массива со строковыми значениями:
```
String[] strArray = {"Java","is","the","best","language"};
```
Если создается пустой массив с некоторым количеством ячеек, то всем элементам массива будут присваиваться значения по умолчанию. Например:
```
int[] arr = new int[10];
```
Так, для массива с элементами типа boolean начальные значения будут равны false, для массивов с числовыми значениями — 0, с элементами типа char — \u0000. Для массива типа класса (объекты) — null.
После объявления массива определенного размера сам размер нельзя изменить. Чтобы добавить новый элемент в массив, необходимо создать новый массив большего размера и скопировать в него все элементы со старого.

7. **Что вы знаете о классах оболочках?**

В java есть примитивные типы данных (такие, как int, short, boolean и т.д.).  У примитивных типов есть объекты-аналоги - так называемые "классы оболочки", или "wrapper" (с англ. "обертка, упаковка").

![](/images/Wrap_1.png "MarineGEO logo")

Класс называется "оболочкой" потому, что он, по сути, копирует то, что уже существует, но добавляет новые возможности для работы с привычными типами.
Но зачем они нужны? Если есть обычный int, short, ..., зачем нам Short и Integer? Или, может, оставить только классы оболочки, и не пользоваться примитивами, раз у них функций больше? 
Например, обычный int занимает меньше места, и если нет необходимости проводить над ним особые операции, Ваш компьютер будет работать быстрее.
В свою очередь, с помощью класса-оболочки Integer можно выполнять специальные операции - например, перевести текст в число:
```
    String s="200";    
    int i=Integer.parseInt(s);
```
Кроме того, есть ситуации, когда нельзя использовать объекты, или наоборот, когда можно использовать только объекты.