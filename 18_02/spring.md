Spring Framework, или просто Spring — один из самых популярных фреймворков для создания веб-приложений на Java. Фреймворк — это что-то похожее на библиотеку.

Спринг — это просто набор каких-то классов и интерфейсов, которые уже написаны за вас. у спринга модульная структура. Это позволяет подключать только те модули, что нам нужны для нашего приложения и не подключать те, которыми мы заведомо не будем пользоваться.

Spring-фреймворк в первую очередь он знаменит своим механизмом внедрения зависимостей (Dependency Injection, DI). 

Рассмотрим пример. Есть классы Shop (магазин) и Seller (продавец). У класса Seller имеется поле типа Shop — магазин, в котором работает продавец.  

Вот мы и столкнулись с зависимостью: Seller зависит от Shop. Теперь задумаемся, как в объект Seller попадет объект Shop? Есть варианты: 

-Внедрить его через конструктор и сразу, при создании продавца, указывать магазин, в котором он работает: 

```
public class Seller { 
     
    private Shop shop; 
 
    public Seller(Shop shop) { 
        this.shop = shop; 
    } 
}
```

Создать сеттер и с помощью его вызова устанавливать продавцу магазин: 

```
public class Seller { 
 
    private Shop shop; 
 
    public void setShop(Shop shop) { 
        this.shop = shop; 
    } 
}
```

И таким образом мы затем используем: 

```
class Application { 
    public static void main(String[] args) { 
        Shop shop= new Shop(); 
        Seller seller = new Seller(shop); 
    } 
} 
```

Перечисленные два способа — это реализация Dependency Injection. И, наконец, мы подобрались к спрингу: он предоставляет еще один способ внедрять зависимости.

```
@Component 
public class Shop { 
} 
 
@Component 
public class Seller { 
 
    @Autowired 
    private Shop shop; 
} 
```

Аннотация Autowired попросила Spring в поле, которое она аннотирует, подставить значение. Эта операция называется «инжектнуть» (inject).   

Бин — создаваемый Spring-ом объект класса, который можно внедрить в качестве значения поля в другой объект. 

Представьте, что у нас приложение не из 2 классов, а на несколько порядков больше и управление зависимостями уже становится не самой тривиальной задачей. 
Поскольку Spring — это фреймворк, необходимо подключить его в свой проект. 

Если искать информацию про работу и установку фреймворка Spring, то часто можно встретить упоминание Spring Boot. Spring Boot — это дополнение к Spring, которое облегчает и ускоряет работу с ним. 

https://start.spring.io/

 В пункте Dependencies не забудьте добавить зависимость Web. После этого нажмите кнопку Generate и скачайте ZIP-архив. Распакуйте его в нужную папку на вашем компьютере, и у вас будет готов каркас будущего проекта. 
 Откройте установленную IDE и создайте новый проект. Назовите его удобным для себя образом. Выберите в меню пункт File и команду Open. Найдите файл DemoApplication.java в папке src/main/java/com/example/demo
 
 Теперь измените содержимое файла, добавив дополнительный метод hello() и аннотации @SpringBootApplication и @RestController, показанные в приведённом ниже коде. Вы можете просто скопировать и вставить код в текст файла:
 
 ```
 import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@SpringBootApplication
@RestController
public class DemoApplication {
                
                  public static void main(String[] args) {
                  SpringApplication.run(DemoApplication.class, args);
                  }
                  
                  @GetMapping("/hello")
                  public String hello(@RequestParam(value = "name", defaultValue = "World") String name) {
                  return String.format("Hello %s!", name);
                  }
                
              }
```

Метод hello(), который мы добавили, принимает параметр name с типом String, а затем объединяет этот параметр со словом «Hello» в коде. Это означает, что если вы зададите в запросе имя «Антон», то ответом будет «Hello Антон». Имя прописывается вручную в параметре defaultValue.

Аннотация @RestController сообщает Spring, что этот код описывает конечную точку, которая должна быть доступна через веб. Аннотация @GetMapping («/hello») указывает Spring, что надо использовать наш метод hello() для ответа на запросы, отправленные на адрес http://localhost:8080/hello. Наконец, @RequestParam указывает Spring, что в запросе должно быть значение name, а если его там нет, то использовать по умолчанию строку «World».

Теперь давайте соберём и запустим программу. Так как наша IDE уже готова к работе с фреймворком Spring, а сам проект создан в Spring Boot, который мы скачали с официального сайта, то сделать это просто.

mvnw spring-boot: run

Когда мы говорим о запросах, мы также подразумеваем HTTP-метод, который используется при отправке этого запроса. 

Каждый запрос представляет собой некий HTTP-метод. Например, когда мы переходим в браузере по адресу http://localhost:8080/greet/John/Smith, наш браузер отправляет GET-запрос на сервер.

Большая часть информационных систем обмениваются данными посредством HTTP-методов. Основными HTTP-методами являются – POST, GET, PUT, DELETE. Эти четыре запроса также называют CRUD-запросами.

POST-метод – используется при создании новых ресурсов или данных. Например, когда мы загружаем новые посты в соцсетях, чаще всего используется POST-запросы. POST-запрос может иметь тело запроса.

GET-метод – используется при получении данных. Например, при открытии любого веб-приложения, отправляется именно GET-запрос для получения данных и отображения их на странице. GET-запрос не имеет тела запроса.

PUT-метод – используется для обновления данных, а также может иметь тело запроса, как и POST.

DELETE-метод – используется для удаления данных.

***Создаем первое приложение***

Создадим дополнительные пакеты :

entity — для сущностей, в текущем проекте это будет класс Task;
controller — для классов контроллера;
repository — для слоя Repository, интерфейс с описанием общих методов, которые будут использоваться при взаимодействии с базой данных;
service — для классов с бизнес-логикой.

Создадим контроллер TaskController в пакете controller:

```
package com.myorganization.Organizer.controller;

import com.myorganization.Organizer.entity.Task;
import com.myorganization.Organizer.service.TaskService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@Controller
@RestController
public class TaskController {

    @Autowired
    private TaskService taskService;

    @GetMapping("/")
    public List<Task> getAll() {
        List<Task> taskList = taskService.getAll();
        return taskList;
    }

    @PostMapping("/add")
    public List<Task> addTask(@RequestBody Task task) {
        taskService.save(task);
        return task;
    }
    
    @GetMapping("/{username}")
    public User getUserByUsername(@PathVariable("username") String username) {
        return users.stream().filter(user -> user.getUsername().equals(username))
                .findFirst().get();
                
     @PutMapping("/{username}")
    public Post update(@PathVariable("username") String username, @RequestBody Post post) {
        users.stream().filter(user ->
                        user.getUsername().equals(username))
                .findAny()
                .ifPresent(user -> user.getPosts().add(post));
        return post;
        
     @DeleteMapping("/{username}")
    public String deleteUser(@PathVariable("username") String username) {
        users.stream().filter(user ->
                        user.getUsername().equals(username))
                        .findAny()
                                .ifPresent(users::remove);
        return "User with username: " + username + " has been deleted";
}
```

Создадим класс Task в пакете entity

```
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import java.util.Date;

@Entity
public class Task {

   @Id
   @GeneratedValue(strategy = GenerationType.AUTO)
   private Integer id;

   private Integer priorityId;

   private String description;

   private Date date;

   public Task() {
   }

   public Task(Integer id, Integer priorityId, String description, Date date) {
       this.id = id;
       this.priorityId = priorityId;
       this.description = description;
       this.date = date;
   }

   public Integer getId() {
       return id;
   }

   public void setId(Integer id) {
       this.id = id;
   }

   public Integer getPriorityId() {
       return priorityId;
   }

   public void setPriorityId(Integer priorityId) {
       this.priorityId = priorityId;
   }

   public String getDescription() {
       return description;
   }

   public void setDescription(String description) {
       this.description = description;
   }

   public Date getDate() {
       return date;
   }

   public void setDate(Date date) {
       this.date = date;
   }
}
```

Класс помечен аннотацией @Entity. Она указывает на то, что данный класс является сущностью и будет сохраняться в БД.

Данная сущность имеет первичный ключ поле id. Поле помечено аннотацией @Id и аннотацией @GeneratedValue, которая задает стратегию генерации первичного ключа как автоматическую генерацию, в этой сущности – целые числа в порядке возрастания.

Создадим интерфейс TaskRepository в пакете repository:

```
import com.myorganization.Organizer.entity.Task;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;


@Repository
public interface TaskRepository extends JpaRepository<Task, Integer> {

}
```

Класс унаследован от JpaRepository – интерфейса фреймворка Spring Data, предоставляющего набор стандартных методов JPA для работы с БД. Для данного приложения нет необходимости создавать дополнительные методы.

Создадим класс TaskService в пакете service:

```
import com.myorganization.Organizer.entity.Task;
import com.myorganization.Organizer.repository.TaskRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.domain.Sort;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class TaskService {

    @Autowired
    private TaskRepository taskRepository;

    public List<Task> getAll() {
        return taskRepository.findAll(Sort.by(Sort.Order.asc("date"),
                                      Sort.Order.desc("priorityId")));
    }

    public Task save(Task task) {
        return taskRepository.save(task);
    }

    public void delete(int id) {
        taskRepository.deleteById(id);
    }
}
```

Более правильно было бы добавить еще один интерфейс TaskService, который будет реализовывать существующий класс TaskService (изменим название на TaskServiceImpl), но в рамках этого приложения делать это не будем.





