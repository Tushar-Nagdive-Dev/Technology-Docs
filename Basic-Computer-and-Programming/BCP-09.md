
## 📘 MODULE 8: Object-Oriented Programming in Java

*Object-Oriented = Organize code around **objects**, not just logic*

---

## 🧠 8.1 What is OOP?

OOP is a programming paradigm that uses **“objects”** to design applications.

### ✅ Real-World Example:

A **Car** has:

* Properties: color, speed, fuel
* Behaviors: drive(), stop(), refuel()

In Java:

```java
class Car {
    String color;
    int speed;

    void drive() {
        System.out.println("Car is driving...");
    }
}
```

---

## 🧠 8.2 Core Principles of OOP

| Principle         | Meaning                               | Real Example                       |
| ----------------- | ------------------------------------- | ---------------------------------- |
| **Encapsulation** | Wrap data + methods in a class        | Capsule = data hidden inside       |
| **Abstraction**   | Hide complexity, show only essentials | Drive a car without knowing engine |
| **Inheritance**   | Reuse behavior from another class     | Son inherits traits from father    |
| **Polymorphism**  | One action, many forms                | Brake in car vs bicycle vs train   |

---

## 🧠 8.3 Classes and Objects

### ✅ A **class** is a blueprint

```java
class Student {
    String name;
    int age;

    void study() {
        System.out.println(name + " is studying.");
    }
}
```

### ✅ An **object** is a real-world instance

```java
public class Main {
    public static void main(String[] args) {
        Student s1 = new Student();
        s1.name = "Tushar";
        s1.age = 25;
        s1.study();  // Tushar is studying.
    }
}
```

---

## 🧠 8.4 Encapsulation (Use of `private`)

```java
class BankAccount {
    private double balance = 0;

    public void deposit(double amount) {
        balance += amount;
    }

    public double getBalance() {
        return balance;
    }
}
```

🔒 Protects `balance` from direct access.

---

## 🧠 8.5 Inheritance – “is-a” Relationship

```java
class Animal {
    void makeSound() {
        System.out.println("Animal makes sound");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("Dog barks");
    }
}
```

---

## 🧠 8.6 Polymorphism (Overriding)

```java
class Animal {
    void makeSound() {
        System.out.println("Animal sound");
    }
}

class Cat extends Animal {
    void makeSound() {
        System.out.println("Meow");
    }
}
```

---

## 🧠 8.7 Abstraction (with `abstract` class or `interface`)

```java
abstract class Shape {
    abstract void draw();
}

class Circle extends Shape {
    void draw() {
        System.out.println("Drawing circle");
    }
}
```

---

## 📝 Exercises

### ✅ Q1: Create a class `Person` with `name`, `age`, and a method `greet()`. Then create two objects and call their methods.

---

### ✅ Q2: Create an `Animal` class and `Dog` class that inherits from `Animal`. Add a method `bark()` to `Dog`.

---

### ✅ Q3: Use encapsulation to protect a variable `password` in a `User` class. Allow it to be set/get via methods only.

---

### ✅ Q4: Use method overriding to change behavior of a method `work()` in a `Programmer` class that extends `Employee`.

---

### ✅ Q5: Create an interface `Vehicle` with method `move()`. Implement it in `Car` and `Bike` classes.

---

```java
// Q1: Person class with name, age, and greet method
class Person {
    String name;
    int age;

    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    void greet() {
        System.out.println("Hello, I'm " + name + " and I'm " + age + " years old.");
    }
}

// Q2: Animal class and Dog class with inheritance
class Animal {
    String species;

    Animal(String species) {
        this.species = species;
    }
}

class Dog extends Animal {
    Dog(String species) {
        super(species);
    }

    void bark() {
        System.out.println("Woof! I'm a " + species);
    }
}

// Q3: User class with encapsulated password
class User {
    private String password;

    void setPassword(String password) {
        this.password = password;
    }

    String getPassword() {
        return password;
    }
}

// Q4: Employee class and Programmer class with method overriding
class Employee {
    void work() {
        System.out.println("Employee is working.");
    }
}

class Programmer extends Employee {
    @Override
    void work() {
        System.out.println("Programmer is coding.");
    }
}

// Q5: Vehicle interface and Car/Bike classes
interface Vehicle {
    void move();
}

class Car implements Vehicle {
    public void move() {
        System.out.println("Car is driving on the road.");
    }
}

class Bike implements Vehicle {
    public void move() {
        System.out.println("Bike is pedaling on the path.");
    }
}

// Main class to test all exercises
public class OOPExercises {
    public static void main(String[] args) {
        // Q1: Create two Person objects and call greet
        System.out.println("Q1: Person class");
        Person person1 = new Person("Alice", 30);
        Person person2 = new Person("Bob", 25);
        person1.greet();
        person2.greet();

        // Q2: Create Dog object and call bark
        System.out.println("\nQ2: Dog class");
        Dog dog = new Dog("Golden Retriever");
        dog.bark();

        // Q3: User class with encapsulated password
        System.out.println("\nQ3: User class");
        User user = new User();
        user.setPassword("secure123");
        System.out.println("User password: " + user.getPassword());

        // Q4: Programmer class with overridden work method
        System.out.println("\nQ4: Programmer class");
        Employee employee = new Employee();
        Programmer programmer = new Programmer();
        employee.work();
        programmer.work();

        // Q5: Car and Bike implementing Vehicle interface
        System.out.println("\nQ5: Vehicle interface");
        Car car = new Car();
        Bike bike =  new Bike();
        car.move();
        bike.move();
    }
}
```
