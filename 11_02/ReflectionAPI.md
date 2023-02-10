 Рефлексия позволяет исследовать информацию о полях, методах и конструкторах классов.
 
 ```
 class Sample
{
    String s;
    public Sample() {
        s = "Java Reflection API";
    }

    public void method1() {
        System.out.println("Информация в строке — " + s);
    }

    public void method2(int x) {
        System.out.println("Целое число — " + x);
    }

    private void method3() {
        System.out.println("Вызов приватного метода");
    }
}

class Exercise

{
    public static void main(String args[]) throws Exception
    {

        Sample obj = new Sample();

        Class cls = obj.getClass();
        System.out.println("Имя класса — " + cls.getName());

        Constructor constructor = cls.getConstructor();
        System.out.println("Имя конструктора — " + constructor.getName());
        System.out.println("Это публичные методы классов: ");

        Method[] methods = cls.getMethods();

        for (Method method : methods) {
            System.out.println(method.getName());
        }

        Method methodcall1 = cls.getDeclaredMethod("method2", int.class);

        methodcall1.invoke(obj, 25);

        Field field = cls.getDeclaredField("s");

        field.setAccessible(true);

        field.set(obj, "Brunoyam");

        Method methodcall2 = cls.getDeclaredMethod("method1");

        methodcall2.invoke(obj);

        Method methodcall3 = cls.getDeclaredMethod("method3");

        methodcall3.setAccessible(true);
        methodcall3.invoke(obj);
    }
}
```
