Файл application.properties:

```
spring.h2.console.enabled=true
spring.datasource.url=jdbc:h2:mem:demodb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=secret
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
```

файл data.sql:

```
INSERT INTO CUSTOMER (name, surname, email) VALUES
 ('Lokesh', 'Gupta', 'abc@gmail.com'),
 ('Deja', 'Vu', 'xyz@email.com'),
 ('Caption', 'America', 'cap@marvel.com');
```

файл schema.sql:

```
DROP TABLE IF EXISTS CUSTOMER;

CREATE TABLE CUSTOMER (
       id INT AUTO_INCREMENT  PRIMARY KEY,
       name VARCHAR(250) NOT NULL,
       surname VARCHAR(250) NOT NULL,
       email VARCHAR(250) DEFAULT NULL
);
```
