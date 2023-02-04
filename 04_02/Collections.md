Collection — это фреймворк, который создан для сохранения и манипуляции объектами.

![](/images/col_3.png)

**ArrayList:**
Самая распространенная коллекция. По сути, это массив с динамически расширяемым размером. Работа по управлению размером массива лежит на коллекции.
У нее быстрый перебор и быстрый поиск по индексу.

**Linked List:**
Это коллекция, в которой каждый элемент имеет ссылку на предыдущий и следующий элементы. По этим ссылкам можно переходить от одного элемента к другому.
При добавлении элемента просто меняются ссылки на предыдущий и следующий элементы.

**Set**

Множество - это такой же способ хранения данных, как массив или список. Но особенность множества в том, что оно может хранить только уникальные значения.

Есть три основные виды множеств - HashSet, LinkedHashSet и TreeSet. Чаще всего используется HashSet. 

-HashSet хранит элементы в произвольном порядке, но зато быстро ищет. Подходит, если порядок Вам не важен, но важна скорость. Более того, для оптимизации поиска, HashSet будет хранить элементы так, как ему удобно.
-LinkedHashSet будет хранить элементы в порядке добавления, но зато работает медленнее.
-TreeSet хранит элементы отсортированными.

```
public class Test {
 
    public static void main(String[] args) {
 
        HashSet<Integer> myHashSet = new HashSet<Integer>();
 
        myHashSet.add(1);
        myHashSet.add(2);
        myHashSet.add(3);
 
        System.out.println("Does myHashSet contain '1'? " + myHashSet.contains(1));
        System.out.println("Does myHashSet contain '11'? " + myHashSet.contains(11));
        
        HashSet<String> set=new HashSet();  
         set.add("One");    
         set.add("Two");    
         set.add("Three");   
         set.add("Four");  
         set.add("Five");  
         Iterator<String> i=set.iterator();  
         while(i.hasNext())  
         {  
         System.out.println(i.next());  
         } 
 
    }
}


```
retainAll(collection) — удаляет из коллекции указанную в скобках коллекцию. Обратите внимание: retainAll, не removeAll;

```
Set<String> s1;
Set<String> s2;
s1.retainAll(s2); // s1 now contains only elements in both sets
 ```

**TreeSet**

```
Set<String> treeSet = new TreeSet<>(Comparator.comparing(String::length));
```
