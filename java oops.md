Here are some **Java OOP (Object-Oriented Programming) concept questions** to assess a candidate’s understanding of fundamental OOP principles such as inheritance, polymorphism, abstraction, encapsulation, and more:

---

### 1. **Encapsulation: Data Hiding and Access Modifiers**

```java
public class Employee {
    private String name;
    private int age;
    private double salary;

    public Employee(String name, int age, double salary) {
        this.name = name;
        this.age = age;
        this.salary = salary;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public double getSalary() {
        return salary;
    }

    public void setSalary(double salary) {
        this.salary = salary;
    }
}
```

**Question:**
- Can you explain the concept of encapsulation? How is it implemented in the above code?
- What would happen if we removed the `private` keyword from the fields?
- How could we improve this code to prevent negative salary or age values?

**What to Look For:**
- The candidate should be able to explain encapsulation as the bundling of data and methods that operate on the data, while restricting direct access to the data (fields).
- They should suggest input validation in setter methods to improve data safety.

---

### 2. **Inheritance and Method Overriding**

```java
class Animal {
    public void sound() {
        System.out.println("Some sound");
    }
}

class Dog extends Animal {
    @Override
    public void sound() {
        System.out.println("Bark");
    }
}

class Cat extends Animal {
    @Override
    public void sound() {
        System.out.println("Meow");
    }
}

public class TestInheritance {
    public static void main(String[] args) {
        Animal dog = new Dog();
        dog.sound();

        Animal cat = new Cat();
        cat.sound();
    }
}
```

**Question:**
- Explain the concept of inheritance in the above code.
- What is method overriding? How is it being used here?
- Can you add a new `Bird` class that extends `Animal` and overrides the `sound()` method to print "Chirp"?

**What to Look For:**
- The candidate should explain how the `Dog` and `Cat` classes inherit from the `Animal` class and override its `sound()` method.
- They should know that method overriding allows a subclass to provide its specific implementation for a method already defined in the parent class.

---

### 3. **Polymorphism: Runtime vs Compile-Time**

```java
class Shape {
    public void draw() {
        System.out.println("Drawing a shape");
    }
}

class Circle extends Shape {
    @Override
    public void draw() {
        System.out.println("Drawing a circle");
    }
}

class Square extends Shape {
    @Override
    public void draw() {
        System.out.println("Drawing a square");
    }
}

public class TestPolymorphism {
    public static void main(String[] args) {
        Shape shape1 = new Circle();
        Shape shape2 = new Square();

        shape1.draw();
        shape2.draw();
    }
}
```

**Question:**
- What is polymorphism in OOP, and how is it demonstrated in this example?
- What’s the difference between compile-time and runtime polymorphism? Which one is being used here?
- Can you add another method `calculateArea()` to `Shape`, which is specific for each subclass?

**What to Look For:**
- The candidate should explain runtime polymorphism (method overriding and dynamic method dispatch).
- They should distinguish between compile-time (method overloading) and runtime (method overriding) polymorphism.

---

### 4. **Abstraction with Abstract Classes**

```java
abstract class Vehicle {
    public abstract void startEngine();

    public void stopEngine() {
        System.out.println("Engine stopped");
    }
}

class Car extends Vehicle {
    @Override
    public void startEngine() {
        System.out.println("Car engine started");
    }
}

class Motorcycle extends Vehicle {
    @Override
    public void startEngine() {
        System.out.println("Motorcycle engine started");
    }
}

public class TestAbstraction {
    public static void main(String[] args) {
        Vehicle car = new Car();
        car.startEngine();
        car.stopEngine();

        Vehicle bike = new Motorcycle();
        bike.startEngine();
    }
}
```

**Question:**
- What is abstraction in OOP, and how is it implemented in the above code?
- Why can’t we instantiate the `Vehicle` class directly?
- How could you refactor this code if you needed to add a `Truck` class that starts the engine differently?

**What to Look For:**
- The candidate should explain abstraction as hiding the implementation details and showing only the essential features (e.g., the abstract method `startEngine()`).
- They should mention that `abstract` classes cannot be instantiated directly and can suggest how to implement a `Truck` class with its own `startEngine()` method.

---

### 5. **Interfaces and Multiple Inheritance**

```java
interface Flyable {
    void fly();
}

interface Swimmable {
    void swim();
}

class Duck implements Flyable, Swimmable {
    @Override
    public void fly() {
        System.out.println("Duck is flying");
    }

    @Override
    public void swim() {
        System.out.println("Duck is swimming");
    }
}

public class TestInterface {
    public static void main(String[] args) {
        Duck duck = new Duck();
        duck.fly();
        duck.swim();
    }
}
```

**Question:**
- Explain the role of interfaces in Java. Why does Java use interfaces to simulate multiple inheritance?
- What is the difference between an interface and an abstract class?
- Can you add a `Quackable` interface that requires the `quack()` method and implement it in `Duck`?

**What to Look For:**
- The candidate should explain that interfaces allow classes to inherit multiple behaviors, unlike abstract classes which allow inheritance of shared structure/behavior but not multiple inheritance.
- They should know that interfaces define a contract that implementing classes must fulfill.

---

### 6. **Constructor Overloading**

```java
class Person {
    private String name;
    private int age;

    public Person() {
        this.name = "Unknown";
        this.age = 0;
    }

    public Person(String name) {
        this.name = name;
        this.age = 0;
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public void display() {
        System.out.println("Name: " + name + ", Age: " + age);
    }
}

public class TestConstructors {
    public static void main(String[] args) {
        Person person1 = new Person();
        Person person2 = new Person("John");
        Person person3 = new Person("Jane", 25);

        person1.display();
        person2.display();
        person3.display();
    }
}
```

**Question:**
- What is constructor overloading, and how is it being used in the `Person` class?
- Why do we need multiple constructors in some cases?
- What would happen if we didn’t provide a default constructor in the `Person` class?

**What to Look For:**
- The candidate should explain that constructor overloading allows creating objects in multiple ways, providing flexibility.
- They should know the importance of providing a default constructor in some situations (like when no-argument instances are required).

---

### 7. **Super Keyword in Inheritance**

```java
class Animal {
    protected String name;

    public Animal(String name) {
        this.name = name;
    }

    public void makeSound() {
        System.out.println("Animal sound");
    }
}

class Dog extends Animal {
    public Dog(String name) {
        super(name);
    }

    @Override
    public void makeSound() {
        super.makeSound();
        System.out.println("Bark");
    }

    public void displayName() {
        System.out.println("Dog's name is: " + super.name);
    }
}

public class TestSuper {
    public static void main(String[] args) {
        Dog dog = new Dog("Buddy");
        dog.makeSound();
        dog.displayName();
    }
}
```

**Question:**
- What is the `super` keyword, and how is it used in the `Dog` class?
- Why is it necessary to call the `super()` constructor in the `Dog` constructor?
- Can you explain how `super` is used for both methods and fields in the above code?

**What to Look For:**
- The candidate should understand the `super` keyword is used to refer to the parent class’s constructor, methods, and fields.
- They should explain the necessity of calling `super()` to ensure the parent class is properly initialized.

---

### 8. **Static Methods and Fields**

```java
class Calculator {
    public static int add(int a, int b) {
        return a + b;
    }

    public static int subtract(int a, int b) {
        return a - b;
    }
}

public class TestStatic {
    public static void main(String[] args) {
        System.out.println("Addition: " + Calculator.add(10, 5));
        System.out.println("Subtraction: " + Calculator.subtract(10, 5));
    }
}
```

**Question:**
- What are static methods and fields in Java? How is it implemented in the `Calculator` class?
- Why is it possible to call `
