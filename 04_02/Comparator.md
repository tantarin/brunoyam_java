**Интерфейс Comparable**

```
public class House implements Comparable<House>{
    int area;
    int price;
    String city;
    boolean hasFurniture;
 
    public House(int area, int price, String city, boolean hasFurniture)
    {
        this.area = area;
        this.price = price;
        this.city = city;
        this.hasFurniture = hasFurniture;
    }
 
    @Override
    public String toString() {
        final StringBuffer sb = new StringBuffer("House{");
            sb.append("area=").append(area);
            sb.append(", price=").append(price);
            sb.append(", city='").append(city).append('\'');
            sb.append(", hasFurniture=").append(hasFurniture);
            sb.append('}');
        return sb.toString();
    }
 
    public int compareTo(House anotherHouse)
    {
        if (this.area == anotherHouse.area) {
            return 0;
        } else if (this.area < anotherHouse.area) {
            return -1;
        } else {
            return 1;
        }
    }
}
```

```
public class Test {
 
    public static void main(String[] args) {
 
       TreeSet<House> myHouseArrayList = new TreeSet<House>();
 
        House firstHouse = new House(100, 120000, "Tokyo", true);
        House secondHouse = new House(40, 70000, "Oxford", true);
        House thirdHouse = new House(70, 180000, "Paris", false);
 
        myHouseArrayList.add(firstHouse);
        myHouseArrayList.add(secondHouse);
        myHouseArrayList.add(thirdHouse);
 
        for (House h: myHouseArrayList) {
            System.out.println(h);
        }
    }
}
```

**Интерфейс Comparator**

Например, у нас уже есть класс House. Давайте создадим отдельный класс, которые будут выполнять функцию сравнения - PriceComparator:

```

public class PriceComparator implements Comparator<House> {
 
    public int compare(House h1, House h2) {
        if (h1.price == h2.price) {
            return 0;
        }
        if (h1.price > h2.price) {
            return 1;
        }
        else {
            return -1;
        }
    }
}


4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
public class Test {
 
    public static void main(String[] args) {
 
       ArrayList<House> myHouseArrayList = new ArrayList<House>();
 
        House firstHouse = new House(100, 120000, "Tokyo", true);
        House secondHouse = new House(40, 70000, "Oxford", true);
        House thirdHouse = new House(70, 180000, "Paris", false);
 
        myHouseArrayList.add(firstHouse);
        myHouseArrayList.add(secondHouse);
        myHouseArrayList.add(thirdHouse);
 
        for (House h: myHouseArrayList) {
            System.out.println(h);
        }
 
        PriceComparator myPriceComparator = new PriceComparator();
        myHouseArrayList.sort(myPriceComparator);
 
        System.out.println("Sorted: ");
        for (House h: myHouseArrayList) {
            System.out.println(h);
        }
    }
}
```
