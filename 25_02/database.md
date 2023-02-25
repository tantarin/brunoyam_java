https://disk.yandex.ru/i/nBB-Hg71ua1tOg

Добавим POM:

```
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <scope>runtime</scope>
        </dependency>
```

Зависимость h2 (эйч ту) займется эмуляцией базы данных без скачиваний, установок и сложных конфигураций.

Создаем в rescources 2 файла: data.sql и schema.sql

```
schema.sql
INSERT INTO TBL_EMPLOYEES (first_name, last_name, email) VALUES
                                                             ('Lokesh', 'Gupta', 'abc@gmail.com'),
                                                             ('Deja', 'Vu', 'xyz@email.com'),
                                                             ('Caption', 'America', 'cap@marvel.com');
                                                             
DROP TABLE IF EXISTS TBL_EMPLOYEES;

data.sql
CREATE TABLE TBL_EMPLOYEES (
                               id INT AUTO_INCREMENT  PRIMARY KEY,
                               first_name VARCHAR(250) NOT NULL,
                               last_name VARCHAR(250) NOT NULL,
                               email VARCHAR(250) DEFAULT NULL
);                                                             
```

В application.properties:

```
spring.h2.console.enabled=true
spring.datasource.url=jdbc:h2:mem:demodb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=secret
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
```

Затем переходим на: http://localhost:8080/h2-console


