
Класс Optional появился в Java 8. Задачей этого класса является предоставление решений на уровне типа-обертки для обеспечения удобства обработки возможных null-значений.

Пустой элемент optional — это основной способ избежать исключения Null Pointer Exception при использовании API Optional.

В потоке Optional любой null будет преобразован в пустой Optional. Пустой элемент Optional больше не будет обрабатываться. Вот как мы можем избежать исключения NullPointerException.

Существует три способа инициализации объекта Optional:

- Optional.of(T)
 
- Optional.ofNullable(T)

- Optional.empty()

```
String name = "brunoyam";
    Optional<String> opt = Optional.of(name);
    assertTrue(opt.isPresent());
```

Представим, что у нас есть класс UserRepository, который работает с хранилищем наших пользователей, в данном примере это будет обычная HashMap.

Допустим, мы хотим найти пользователя в базе данных по идентификатору. После чего нам требуется вывести на консоль длину имени пользователя.

```
final Person person = personRepository.findById(1L);
final String firstName = person.getFirstName();
System.out.println("Длина твоего имени: " + firstName.length());
```

Выполнив этот код, мы можем получить исключение:

```
Exception in thread "main" java.lang.NullPointerException: Cannot invoke "dev.struchkov.example.optional.Person.getFirstName()" because "person" is null
	at dev.struchkov.example.optional.Main.test(Main.java:18)
	at dev.struchkov.example.optional.Main.main(Main.java:13)
```

Дело в том, что мы не учли того факта, что пользователя с идентификатором 1L может не быть в нашей HashMap. И у нас нет варианта, кроме как вернуть null.

```
public class PersonRepository {

    private final Map<Long, Person> persons;

    public PersonRepository(Map<Long, Person> persons) {
        this.persons = persons;
    }

    public Person findById(Long id) {
        return persons.get(id);
    }
    
}
```

Во время вызова метода null-объекта вы получите NullPointerException.

```
final Person person = personRepository.findById(1L);
if (person != null) {
    final String firstName = person.getFirstName();
    System.out.println("Длина твоего имени: " + firstName.length());
}
```

Допустим, что пользователь с таким идентификатором все же есть, выполняем код:

```
Exception in thread "main" java.lang.NullPointerException: Cannot invoke "String.length()" because "firstName" is null
	at dev.struchkov.example.optional.Main.test(Main.java:20)
	at dev.struchkov.example.optional.Main.main(Main.java:13)
 ```
 
 И мы снова получили NullPointerException, только на этот раз проблема оказалась в поле firstName, теперь оно null, и уже вызов метода length() выбросил исключение. Добавим еще одну проверку:
 
 ```
 final User user = userRepository.findById(1L);
if (user != null) {
	final String firstName = user.getFirstName();
	if (firstName != null) {
		System.out.println("Длина твоего имени: " + firstName.length());
	}
}
```

Перепишем наш пример с использованием Optional и посмотрим, что изменится:

```

import java.util.Map;
import java.util.Optional;

public class PersonRepository {

    private final Map<Long, Person> persons;

    public PersonRepository(Map<Long, Person> persons) {
        this.persons = persons;
    }

    public Optional<Person> findById(Long id) {
        return Optional.ofNullable(persons.get(id));
    }

}


final Optional<Person> optPerson = personRepository.findById(1L);
if (optPerson.isPresent()) {
    final Person person = optPerson.get();
    final String firstName = person.getFirstName();
    if (firstName != null) {
    	System.out.println("Длина твоего имени: " + firstName.length());
    }
}
```

Вы точно знаете что метод findById() возвращает контейнер, в котором объекта может не быть. 

Здесь мы с вами увидели два основных метода: isPresent() возвращает true, если внутри есть объект, а метод get() возвращает этот объект.

```
final Optional<Person> optPerson = personRepository.findById(1L);
final Optional<String> optFirstName = optPerson.map(user -> user.getFirstName());

optFirstName.ifPresent(
	firstName -> System.out.println("Длина твоего имени: " + firstName.length())
);
```

```
personRepository.findById(1L)
        .map(User::getFirstName)
        .ifPresent(
                firstName -> System.out.println("Длина твоего имени: " + firstName.length())
        );
 ```

Метод Optional.orElse возвращает переданное значение, если Optional пустой

```
 Optional<String> name = Optional.of("John");
    System.out.println(name.orElse("blank")); //output John
 
    Optional<Object> empty = Optional.empty();
    System.out.println(empty.orElse("blank")); //output blank
 ```
