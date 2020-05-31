# Table of Contents

1. [Constructor](#constructor)
2. [Overload](#overload)
3. [Inheritance](#inheritance)
4. [Polymorphism](#polymorphism)
5. [Abstraction](#abstraction)
6. [Interface](#interface)
7. [static](#static)

| Benefits of OOP |                                               |
| --------------- | --------------------------------------------- |
| Encapsulation   | Reduce complexity + increase reusability      |
| Abstraction     | Reduce complexity + isolate impact of changes |
| Inheritance     | Eliminate redundant code                      |
| Polymorphism    | Refactor ugly switch/case statements          |

## Constructor

```java
class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public Person(String name) {
        this(name, 18);
    }

    public Person() {
        this("Unnamed");
    }
}
```

## Overload

```java
class Person {
    private String name;

    public void setName(String name) {
        this.name = name;
    }

    public void setName(String firstName, String lastName) {
        String name = firstName + " " + lastName;
        this.setName(name);
    }
}
```

## Inheritance

```java
class Person {
    protected String name;
    protected int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {...}
    public void setName(String name) {...}
    public int getAge() {...}
    public void setAge(int age) {...}
}
```

```java
class Student extends Person {
    private int score;

    public Student(String name, int age, int score) {
        super(name, age);
        this.score = score;
    }

    public int getScore() {...}
    public void setScore(int score) {...}
}
```

### Upcasting

```java
Student s = new Student(...);
Person p = s;  // upcasting, ok
Object o1 = p; // upcasting, ok
Object o2 = s; // upcasting, ok
```

### Downcasting

```java
Person p1 = new Student(...);
Person p2 = new Person(...);
Student s1 = (Student) p1; // ok
Student s2 = (Student) p2; // runtime error! ClassCastException
```

Using `instanceof` to avoid error

```java
Person p = new Student(...);
if (p instanceof Student) {
    Student s = (Student) p;
}
```

Since Java 14, enable compiler with `--source 14` and `--enable-preview`, directly casting type

```java
Object obj = "hello";
if (obj instanceof String s) {
    System.our.println(s.toUpperCase()); // HELLO
}
```

## Polymorphism

Override: same function signature, same return type
Overload: different function signature

```java
class Person {
    public void run() {
        System.out.println("Person.run");
    }
}

class Student extends Person {
    @Override
    public void run() {
        System.out.println("Student.run");
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Income[] incomes = new Income[] {
            new Income(3000),
            new Salary(7500),
            new SpecialAllowance(15000)
        };

        System.out.println(totalTax(incomes));
    }

    public static double totalTax(Income... incomes) {
        double total = 0;

        for (Income income: incomes) {
            total += income.getTax();
        }

        return total;
    }
}

class Income {
    protected double income;

    public Income(double income) {
        this.income = income;
    }

    public double getTax() {
        return income * 0.1;
    }
}

class Salary extends Income {
    public Salary(double income) {
        super(income);
    }

    @Override
    public double getTax() {
        if (income <= 5000) {
            return 0;
        }

        return (income - 5000) * 0.2;
    }
}

class SpecialAllowance extends Income {
    public SpecialAllowance(double income) {
        super(income);
    }

    @Override
    public double getTax() {
        return 0;
    }
}
```

### Call `super` method

```java
class Person {
    protected String name;

    public String hello() {
        return "Hello, " + name;
    }
}

class Student extends Person {
    @Override
    public String hello() {
        return super.hello() + "!";
    }
}
```

### `final`, don't allow `Override`

```java
class Person {
    protected String name;

    public final String hello() {
        return "Hello, " + name;
    }
}

class Student extends Person {
    // compile error
    @Override
    public String hello() {}
}
```

```java
final class Person {}

// compile error
class Student extends Person {}
```

```java
class Person {
    // once instantiated, can't change
    public final String name;

    public Person(String name) {
        this.name = name;
    }
}
```

## Abstraction

```java
abstract class Person {
    public abstract void run();
}

class Student {
    @Override
    public void run() {...}
}
```

## Interface

if a abstract class doesn't have any fields,

```java
abstract class Person {
    public abstract void run();
    public abstract String getName();
}
```

we can change it to `interface`

```java
interface Person {
    void run();
    String getName();
}
```

```java
class Student implements Person {
    private String name;

    public Student(String name) {
        this.name = name;
    }

    @Override
    public void run() {
        System.out.println(this.name + " run");
    }

    @Override
    public String getName() {
        return this.name;
    }
}
```

In Java, a class could **ONLY** extends one class, but could implements multiple `interface`

```java
class Student implements Person, Hello {}
```

### Interface inheritance

```java
interface Hello {
    void hello();
}

interface Person extends Hello {
    void run();
    String getName();
}
```

### Interface default

```java
public class Main {
    public static void main(String[] args) {
        Person p = new Student("John");
        p.run(); // John run
    }
}

interface Person {
    String getName();
    default void run() {
        System.out.println(getName() + " run");
    }
}

class Student implements Person {
    private String name;

    public Student(String name) {
        this.name = name;
    }

    public String getName() {
        return this.name;
    }
}
```

## static

`static` share one memory address in class

### static field

```java
public class Main {
    public static void main(String[] args) {
        Person p1 = new Person("John", 20);
        System.out.println(p1.number); // 1

        Person p2 = new Person("Jane", 21);
        System.out.println(p2.number); // 2
    }
}


class Person {
    public String name;
    public int age;
    public static int number = 0;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
        number++;
    }
}
```

**NOT** recommend to use `instance.static_field` to access, better using `Person.static_field`, e.g. `Person.number`;

### static method

Can **NOT** use `this` in `static method`

```java
public class Main {
    public static void main(String[] args) {
        Person.setName(99);
        System.out.println(Person.number);
    }
}

class Person {
    public static int number;

    public static void setNumber(int value) {
        number = value;
    }
}
```

### static in interface

```java
public interface Person {
    public static final int MALE = 1;
    public static final int FEMALE = 2;
}
```

since fields in `interface` could only be `public static final`, could also write as

```java
public interface Person {
    int MALE = 1;
    int FEMALE = 2;
}
```
