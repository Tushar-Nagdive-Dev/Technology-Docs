## 📘 MODULE 3: Java Setup & Your First Program

---

## 🧠 3.1 Setting Up Java (JDK) and an IDE

### ✅ Step 1: Install Java JDK

We’ll use the latest stable **Java 17+**.

🔗 [Download JDK](https://www.oracle.com/java/technologies/javase-jdk17-downloads.html)
Or use: [https://adoptium.net](https://adoptium.net)

✅ After install, run in terminal (Mac/Linux) or CMD (Windows):

```bash
java -version
javac -version
```

You should see something like:

```
java version "17.0.12"
javac 17.0.12
```

---

### ✅ Step 2: Install an IDE

Choose one of the following:

* **VS Code** (Lightweight, good for beginners)
* **IntelliJ IDEA CE** (Highly recommended)
* **Eclipse IDE**

👉 For now, let's proceed with **VS Code**.

Install:

* [VS Code](https://code.visualstudio.com)
* Install Extension: `Java Extension Pack` (by Microsoft)

---

## 🧠 3.2 Your First Java Program: HelloWorld

### ✅ File: `HelloWorld.java`

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, Tushar!");
    }
}
```

---

### ✅ Step-by-Step Explanation:

| Line                                       | Explanation                                              |
| ------------------------------------------ | -------------------------------------------------------- |
| `public class HelloWorld {`                | Defines a class named `HelloWorld` (Java is class-based) |
| `public static void main(String[] args) {` | Entry point of the program — code execution starts here  |
| `System.out.println("Hello, Tushar!");`    | Prints text to the console                               |
| `}`                                        | Closes the method and class                              |

---

## 🧪 How to Compile and Run

### ✅ Compile:

```bash
javac HelloWorld.java
```

This creates a file `HelloWorld.class`

### ✅ Run:

```bash
java HelloWorld
```

✅ Output:

```
Hello, Tushar!
```

---

## ⚠️ Common Mistakes

| Mistake                            | Fix                               |
| ---------------------------------- | --------------------------------- |
| File name doesn’t match class name | Must be `HelloWorld.java`         |
| Forgetting `main()` method         | Add `public static void main…`    |
| Missing semicolon `;`              | Every Java statement ends with it |

---

## 📝 Practice Exercises

### ✅ Q1: Modify the program to print:

```
Welcome to Java Programming, Tushar!
You're going to be a Java Expert.
```

### ✅ Q2: Create a new file `Introduction.java` that prints your name, age, and favorite language.

---
```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Welcome to Java Programming, Tushar!");
        System.out.println("You're going to be a Java Expert.");
    }
}
```

```java
public class Introduction {
    public static void main(String[] args) {
        String name = "Tushar";
        int age = 25; // Replace with your actual age
        String favoriteLanguage = "Java"; // Replace with your favorite language
        System.out.println("Name: " + name);
        System.out.println("Age: " + age);
        System.out.println("Favorite Language: " + favoriteLanguage);
    }
}
```
