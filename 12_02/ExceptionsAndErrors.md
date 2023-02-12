![](/images/exc.jpeg)

Исключения делятся на несколько классов, но все они имеют общего предка — класс Throwable. Его потомками являются подклассы Exception и Error. Исключения (Exceptions) являются результатом проблем в программе, которые в принципе решаемы и предсказуемы.

Ошибки (Errors) представляют собой более серьёзные проблемы, которые, согласно спецификации Java, не следует пытаться обрабатывать в собственной программе, поскольку они связаны с проблемами уровня JVM. Например, исключения такого рода возникают, если закончилась память, доступная виртуальной машине.

В Java все исключения делятся на два типа: контролируемые исключения (checked) и неконтролируемые исключения (unchecked), к которым относятся ошибки (Errors) и исключения времени выполнения (RuntimeExceptions, потомок класса Exception).

Контролируемые исключения представляют собой ошибки, которые можно и нужно обрабатывать в программе, к этому типу относятся все потомки класса Exception (но не RuntimeException).

**В Java есть пять ключевых слов для работы с исключениями:**

-try — данное ключевое слово используется для отметки начала блока кода, который потенциально может привести к ошибке.

-catch — ключевое слово для отметки начала блока кода, предназначенного для перехвата и обработки исключений.

-finally — ключевое слово для отметки начала блока кода, которой является дополнительным. Этот блок помещается после последнего блока ‘catch’. Управление обычно передаётся в блок ‘finally’ в любом случае.

-throw — служит для генерации исключений.

-throws — ключевое слово, которое прописывается в сигнатуре метода, и обозначающее что метод потенциально может выбросить исключение с указанным типом.

Когда исключение передано, выполнение метода направляется по нелинейному пути. Это может стать источником проблем. Например, при входе метод открывает файл и закрывает при выходе. Чтобы закрытие файла не было пропущено из-за обработки исключения, был предложен механизм finally.

Ключевое слово finally создаёт блок кода, который будет выполнен после завершения блока try/catch, но перед кодом, следующим за ним. Блок будет выполнен, независимо от того, передано исключение или нет. Оператор finally не обязателен, однако каждый оператор try требует наличия либо catch, либо finally. Код в блоке finally будет выполнен всегда.

В Java 7 стала доступна новая конструкция, с помощью которой можно перехватывать несколько исключений одним блоком catch:

```
try {  
 ... 
} catch( IOException | SQLException ex ) {  
  logger.log(ex); 
  throw ex; 
}
```

```
class JavaExceptionExample{
    public static void main(String args[]){
        try{
            int data=100/0;
        }catch(ArithmeticException e){System.out.println(e);}
        //rest code of the program   
        System.out.println("rest of the code...");
    }
}  
```

**Создать собственное исключение**

```
public class ExcClass extends Exception {
 
    private String someString;
 
    public ExcClass (String string) {
        this.someString = string;
        System.out.println("Exception ExcClass");
    }
 
    public void myOwnExceptionMsg() {
        System.err.println("This is exception message for string: " + someString);
    }
}
 
public class TestExc {
 
    public static void main(String[] args) {
        try {
            String s = "SomeString";
            throw new ExcClass(s);
        } catch (ExcClass ex) {
            ex.myOwnExceptionMsg();
        }
    }
}
```

Пример:

```
import java.util.Scanner;

// Создание собственного класса исключения TriangleException
// Создание собственного класса Triangle, реализующего треугольник по его сторонам
class TriangleException extends Exception
{
  // переопределить метод toString(), описывающий исключение
  public String toString()
  {
    return "Error. Bad sides of triangle.";
  }
}

// объявить класс треугольник
class Triangle
{
  // стороны треугольника
  private double a,b,c;

  // конструктор по умолчанию
  public Triangle()
  {
    // равносторонний треугольник с длиной стороны 1
    a = b = c = 1;
  }

  // параметризированный конструктор
  public Triangle(double _a, double _b, double _c)
  {
    // использование класса исключения TriangleException
    try {
      // можно ли из сторон _a, _b, _c создать треугольник
      if (((_a+_b)<_c)||((_a+_c)<_b)||((_b+_c)<_a))
        throw new TriangleException();
    }
    catch(TriangleException e)
    {
      System.out.println("Exception: "+e.toString());
      return;
    }

    // если стороны треугольника введены корректно,
    // записать их во внутренние переменные класса
    a = _a;
    b = _b;
    c = _c;
  }

  // метод возвращающий площадь треугольника
  public double getArea()
  {
    // если стороны треугольника имеют корректные размеры,
    // то проверку на корень из отрицательного числа делать не нужно
    double p, s;
    p = (a+b+c)/2; // полупериметр
    s = Math.sqrt(p*(p-a)*(p-b)*(p-c)); // формула Герона
    return s;
  }
}

public class Train04 {
  // функция main() тестирует работу классов Triangle и TriangleException
  public static void main(String[] args) {
    double a, b, c;
    Scanner in = new Scanner(System.in);

    // ввод a, b, c
    System.out.print("a = ");
    a = in.nextDouble();
    System.out.print("b = ");
    b = in.nextDouble();
    System.out.print("c = ");
    c = in.nextDouble();

    // вычисление площади
    Triangle tr = new Triangle(a,b,c);
    double area = tr.getArea();
    System.out.println("area = " + area);

    in.close();
  }
}
```

Еще пример:

```
class QuadraticEquation {
    double a,b,c,x1,x2;

    // конструктор
    QuadraticEquation(double a, double b, double c) {
        this.a = a;
        this.b = b;
        this.c = c;
    }

    // метод, который решает квадратное уравнение
    void Solution() {
        double d; // дискриминант

        try {
            d = b*b - 4*a*c;

            // учесть деление на 0
            if (a==0)
                throw new ArithmeticException("Деление на 0."); // создать исключение

            // учесть корень из отрицательного числа
            if (d<0)
                throw new ArithmeticException("Уравнение не имеет корней."); // создать исключение

            x1 = (-b - Math.sqrt(d))/(2*a);
            x2 = (-b + Math.sqrt(d))/(2*a);

            System.out.println("x1 = " + x1);
            System.out.println("x2 = " + x2);
        }
        catch (ArithmeticException e) {
            System.out.println(e); // вывести исключение, созданное в блоке try
        }
    }
}
```

Пример исполтзования throws:

```
class NegativeNumberException extends Exception {
    private double value;

    // конструктор класса
    NegativeNumberException(double _value) {
        value = _value;
    }

    // переопределенный метод toString() класса Throwable,
    // этот метод виводит информацию об исключении типа NegativeNumberException
    public String toString() {
        String msg = "Exception: " + value + " is a negative number!!!";
        return msg;
    }
}

// демонстрация исключения
class DemoThrows {
    int SumItems(int A[]) throws NegativeNumberException {
        int i, sum=0;
        for (i=0; i<10; i++) {
            // если индекс за пределами массива, то сгенерировать исключение
            if (i>A.length)
                throw new ArrayIndexOutOfBoundsException("Индекс за пределами массива.");
            if (A[i]<0)
                throw new NegativeNumberException((double)A[i]);
            sum += A[i];
        }
        return sum;
    }
}

public class Train04 {
    public static void main(String[] args) {
        // вызов метода SumItems из класса DemoThrows
        int M[] = { 1, -2, 3, 4, 5, 6, 7 };
        DemoThrows dt = new DemoThrows(); // создать екземпляр класса DemoThrows
        int summ=0;

        try {
            // вызвать метод, в котором генерируется
            // исключение NegativeNumberException
            summ = dt.SumItems(M);
        }
        catch (NegativeNumberException e) {
            System.out.println("Отрицательное число: "+e);
        }
        catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("Перехвачено исключение: " + e);
        }
        finally {
            System.out.println("Summ = " + summ);
        }
    }
}
```


