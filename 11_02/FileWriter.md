 **Работа с потоками данных. IO vs. NIO**
 
 В Java основной функционал работы с потоками сосредоточен в классах из пакета java.io.
 
Объект, из которого можно считать данные, называется потоком ввода, а объект, в который можно записывать данные, - потоком вывода. Например, если надо считать содержание файла, то применяется поток ввода, а если надо записать в файл - то поток вывода.

В основе всех классов, управляющих потоками байтов, находятся два абстрактных класса: InputStream (представляющий потоки ввода) и OutputStream (представляющий потоки вывода).

Работают с символами: Writer и Reader. 

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



