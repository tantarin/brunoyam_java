 **Работа с потоками данных. IO vs. NIO**
 
В Java основной функционал работы с потоками сосредоточен в классах из пакета java.io.
 
IO (Input & Output) API — это Java API, которое облегчает разработчикам работу с потоками. Скажем, мы получили какие-то данные (например, фамилия, имя и отчество) и нам нужно записать их в файл — в этот момент и приходит время использовать java.io.

Разделяют два вида потоков ввода/вывода: байтовые и символьные.

Байтовые: java.io.InputStream, java.io.OutputStream;

Символьные: java.io.Reader, java.io.Writer;
 
Объект, из которого можно считать данные, называется потоком ввода, а объект, в который можно записывать данные, - потоком вывода. Например, если надо считать содержание файла, то применяется поток ввода, а если надо записать в файл - то поток вывода.

Класс OutputStream - это абстрактный класс, определяющий потоковый байтовый вывод.

В этой категории находятся классы, определяющие, куда направляются ваши данные: в массив байтов (но не напрямую в String; предполагается что вы сможете создать их из массива байтов), в файл или канал.

-BufferedOutputStream (Буферизированный выходной поток)

-ByteArrayOutputStream (Создает буфер в памяти. Все данные, посылаемые в этот поток, размещаются в созданном буфере)

-FileOutputStream(Отправка данных в файл на диске. Реализация класса OutputStream)

-ObjectOutputStream(Выходной поток для объектов)

Работа с файлами всегда несет за собой работу с исключениями: например, попытка создать новый файл, который уже существует, вызовет IOException. В данном случае работа приложения должна быть продолжена и пользователь должен получить уведомление о том, по какой причине файл не может быть создан. 

```
try {
	File.createTempFile("prefix", "");
} catch (IOException e) {
	// Handle IOException
}

public static File createTempFile(String prefix, String suffix) throws IOException {
...
}
```


Схема работы с потоком в упрощенном виде выглядит так:

-Создается экземпляр потока

-Поток открывается (для чтения или записи)

-Производится чтение из потока/запись в поток

-Поток закрывается

Reader и Writer — абстрактные классы. Для работы с файлами нам потребуются уже конкретные и это будут FileReader и FileWriter.

```
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
 
public class WriterReader 
{
    public static void main(String[] args) {
        writeText();
        readText();
    }
 
    private static void writeText() {
        String test = "TEST !!!"; // Эту строку мы посимвольно запишем в файл
 
        // Создание файлового потока для записи символов ак автозакрываемый ресурс
        // Нам не надо вызывать fw.close(), т.к. в данном случае он будет закрыт автоматически
        try (FileWriter fw = new FileWriter("text.txt")) {
            // Записываем посимвольно , обращаясь к каждому элементу строку (как к символу)
            for (int i = 0; i < test.length(); i++) {
                fw.write(test.charAt(i));
            }
        } catch (IOException ex) {
            ex.printStackTrace(System.out);
        }
    }
 
    private static void readText() {
        try (FileReader fr = new FileReader("text.txt")) {
            // Переменная для хранения строки
            StringBuilder sb = new StringBuilder();
            int code = -1;
            // Читаем посимвольно пока код считанного символа не станет равным -1
            while ((code = fr.read()) != -1) {
                // Вызов Character.toChars() преобразует int в char
                sb.append(Character.toChars(code));
            }
            System.out.println(sb.toString());
 
        } catch (IOException ex) {
            ex.printStackTrace(System.out);
        }
    }
}
```

После окончания блока try .. catch наш FileWriter автоматически закроется. Обратите внимание — если файл не закрыть, то не гарантируется, что он корректно будет записан. Именно НЕ ГАРАНТИРУЕТСЯ. Т.е. может записаться, а может и нет. 

**Работа с файлами**

Класс File, определенный в пакете java.io, не работает напрямую с потоками. Его задачей является управление информацией о файлах и каталогах.

```
import java.io.File;

public class Main {
    public static void main(String args[])
    {
        try {
            File f = new File("C:\\Users\\tanya\\untitled\\src\\ex.txt");
            if (f.createNewFile())
                System.out.println("File created");
            else
                System.out.println("File already exists");
        }
        catch (Exception e) {
            System.err.println(e);
        }
    }
}
```

Если хотите создать новый файл и одновременно записать какую-нибудь информацию в нем, можете использовать метод FileOutputStream.write. Этот метод автоматически создает новый файл и записывает в нем контент.

Метод FileOutputStream используется для записи байтов в файл. Если хотите записать символьную информацию, будет лучше использовать FileWriter.

```
import java.io.FileWriter;

public class Main {
    public static void main(String[] args) {
        try(FileWriter fileWriter = new FileWriter("C:\\Users\\tanya\\untitled\\src\\ex.txt")) {
            fileWriter.write("Brunoyam");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

**Чтение из файла**

```
public static void main(String[] args) throws Exception
{
 FileReader reader = new FileReader("c:/data.txt");
 FileWriter writer = new FileWriter("c:/result.txt");

 while (reader.ready()) //пока есть непрочитанные байты в потоке ввода
 {
  int data = reader.read(); //читаем один символ (char будет расширен до int)
  writer.write(data); //пишем один символ (int будет обрезан/сужен до char)
 }

 //закрываем потоки после использования
 reader.close();
 writer.close();
}
```

Чтобы прочесть содержимое файла используется класс BufferedReader. BufferedWriter пишет в файл с использованием буфера. А это рациональнее в плане работы с ресурсами.

Java класс BufferedReader читает текст из потока ввода символов, буферизуя прочитанные символы, чтобы обеспечить эффективное считывание символов, массивов и строк. Можно указать в конструкторе вторым параметром размер буфера.


```
// В качестве объекта передаем открытый файл на основе класса FileReader
BufferedReader br = new BufferedReader(file);

// Считываем данные
while((String line = br.readLine()) != null) {
	// Выводим каждую отдельную строку в консоль
	System.out.println(line);
}
```

Пример:

```
public class FileWritterExample {

	public static void main(String[] args) {
		String outputFileName = "file.txt";
		String[] array = { "one", "two", "three", "four" };

		try (BufferedWriter writter = new BufferedWriter(new FileWriter(outputFileName))) {
			for (String value : array) {
				writter.write(value + "\n");
			}
		}
        catch (IOException e) {
			e.printStackTrace();
		}
	}

}
```

**Считываем с консоли**

```
BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
        String fileName = reader.readLine();

        FileWriter writer = new FileWriter(fileName);
```

Считываем с консоли и записываем в файл:

```
public class ConsoleReaderExample {

	public static void main(String[] args) {
		String outputFileName = "file.txt";

		try (BufferedReader reader = new BufferedReader(new InputStreamReader(System.in))) {
			try (BufferedWriter writter = new BufferedWriter(new FileWriter(outputFileName))) {
				String line;
				while (!(line = reader.readLine()).equals("exit")) { // Прерывание цикла при написании строки exit
					writter.write(line);
				}
			}
		}
         catch (IOException e) {
			e.printStackTrace();
		}
	}

}
```

**Запись объектов в файл**

```
public class Main {

    public static void main(String[] args) {
        Employee emp = new Employee("Pankaj");

        emp.setAge(35);
        emp.setGender("Male");
        emp.setRole("CEO");
        System.out.println(emp);

        try {
            FileOutputStream fos = new FileOutputStream("EmployeeObject.ser");
            ObjectOutputStream oos = new ObjectOutputStream(fos);
            // write object to file
            oos.writeObject(emp);
            System.out.println("Done");


            FileInputStream is = new FileInputStream("EmployeeObject.ser");
            ObjectInputStream ois = new ObjectInputStream(is);
            Employee emp2 = (Employee) ois.readObject();

            ois.close();
            is.close();
            System.out.println(emp);
            oos.close();
            fos.close();
        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            throw new RuntimeException(e);
        }
    }

}


class Employee implements Serializable {

    private static final long serialVersionUID = -299482035708790407L;

    private String name;
    private String gender;
    private int age;

    private String role;
    // private transient String role;

    public Employee(String n) {
        this.name = n;
    }

    public String getGender() {
        return gender;
    }

    public void setGender(String gender) {
        this.gender = gender;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getRole() {
        return role;
    }

    public void setRole(String role) {
        this.role = role;
    }

    @Override
    public String toString() {
        return "Employee:: Name=" + this.name + " Age=" + this.age + " Gender=" + this.gender + " Role=" + this.role;
    }
}
```


Java NIO, или Java Non-blocking I/O (иногда — Java New I/O, “новый ввод-вывод”) предназначена для реализации высокопроизводительных операций ввода-вывода.

Files.walkFileTree()
Files.isSymbolicLink()
Files.readAttributes()


Files.write() - это рекомендуемый способ создания файла, так как вам не придется беспокоиться о закрытии ресурсов IO.

```
public class FilesWriteFileExample {

	public static void main(String[] args) {
		Path path = Paths.get("D:/data/test.txt");
		try {
			String str = "This is write file Example";
			byte[] bs = str.getBytes();
			Path writtenFilePath = Files.write(path, bs);
			System.out.println("Written content in file:\n"+ new String(Files.readAllBytes(writtenFilePath)));
		} catch (Exception e) {
			e.printStackTrace();
		}

	}

}
```


**Path**
Path представляет из себя путь в файловой системе. Он содержит имя файла и список каталогов, определяющих путь к нему.

```
Path relative = Paths.get("Main.java");
System.out.println("Файл: " + relative);
//получение файловой системы
System.out.println(relative.getFileSystem());
```

Paths — это совсем простой класс с единственным статическим методом get(). Его создали исключительно для того, чтобы из переданной строки или URI получить объект типа Path.

```
Path path = Paths.get("c:\\data\\file.txt");
```

Files — это утилитный класс, с помощью которого мы можем напрямую получать размер файла, копировать их, и не только.

```
Path path = Paths.get("files/file.txt");
boolean pathExists = Files.exists(path);
```

Java IO (input-output) является потокоориентированным, а Java NIO (new/non-blocking io) – буфер-ориентированным.

Когда дело доходит до чтения файлов, NIO.2 предлагает несколько способов сделать это — каждый со своими плюсами и минусами. Эти подходы заключаются в следующем:

-Чтение файла в байтовый массив

-Использование небуферизованных потоков

-Использование буферизованных потоков

```

Path filePath = Paths.get("C:", "b.txt");
 
if (Files.exists(filePath)) {
    try {
        List<String> lines = Files.readAllLines(filePath, StandardCharsets.UTF_8);
 
        for (String line : lines) {
            System.out.println(line);
        }
    } catch (IOException e) {
        throw new RuntimeException(e);
    }
}
```

Просто подготовьте список строк и передайте его для write метода.

```

Path filePath = Paths.get("/home/jstas/b.txt");
 
List<String> lines = new ArrayList<>();
lines.add("Lorem ipsum dolor sit amet, consectetur adipiscing elit.");
lines.add("Aliquam sit amet justo nec leo euismod porttitor.");
lines.add("Vestibulum id sagittis nulla, eu posuere sem.");
lines.add("Cras commodo, massa sed semper elementum, ligula orci malesuada tortor, sed iaculis ligula ligula et ipsum.");
 
try {
    Files.write(filePath, lines, StandardCharsets.UTF_8, StandardOpenOption.CREATE, StandardOpenOption.APPEND);
} catch (IOException e) {
    throw new RuntimeException(e);
}
```

Может быть не очень хорошая идея работать с байтовыми массивами, когда дело касается больших файлов. Тогда помогают потоки.

**try-with-resources**

Конструкцию try-with-resources ввели в Java 7. Она дает возможность объявлять один или несколько ресурсов в блоке try, которые будут закрыты автоматически без использования finally блока.

В качестве ресурса можно использовать любой объект, класс которого реализует интерфейс java.lang.AutoCloseable или java.io.Closable.

```
String src = "c:\\projects\\log.txt";
String dest = "c:\\projects\\copy.txt";

try(FileInputStream input = new FileInputStream(src);

FileOutputStream output = new FileOutputStream(dest))
{
   byte[] buffer = input.readAllBytes();
   output.write(buffer);
}
```



