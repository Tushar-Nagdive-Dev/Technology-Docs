## **Employee Management System**

### **1. Basic Requirements**

#### **1.1 Create an `Employee` Class**
- The `Employee` class will have attributes like `id`, `name`, and `salary`.
- It will include a constructor to initialize these attributes and a method to display employee details.

```java
class Employee {
    private int id;
    private String name;
    private double salary;

    // Constructor to initialize Employee object
    public Employee(int id, String name, double salary) {
        this.id = id;
        this.name = name;
        this.salary = salary;
    }

    // Method to display employee details
    public void displayDetails() {
        System.out.println("Employee ID: " + id);
        System.out.println("Name: " + name);
        System.out.println("Salary: $" + salary);
    }

    // Getters and setters (optional)
    public int getId() { return id; }
    public String getName() { return name; }
    public double getSalary() { return salary; }
}
```

#### **1.2 Example Usage**
```java
public class Main {
    public static void main(String[] args) {
        // Create an Employee object
        Employee emp = new Employee(101, "John Doe", 50000);

        // Display employee details
        emp.displayDetails();
    }
}
```

---

### **2. Intermediate Requirements**

#### **2.1 Create a `Manager` Class Using Inheritance**
- The `Manager` class will extend the `Employee` class and add a `department` attribute.
- It will override the `displayDetails` method to include the department.

```java
class Manager extends Employee {
    private String department;

    // Constructor to initialize Manager object
    public Manager(int id, String name, double salary, String department) {
        super(id, name, salary); // Call superclass constructor
        this.department = department;
    }

    // Override displayDetails method
    @Override
    public void displayDetails() {
        super.displayDetails(); // Call superclass method
        System.out.println("Department: " + department);
    }

    // Method to calculate bonus
    public double calculateBonus() {
        return getSalary() * 0.1; // 10% bonus
    }
}
```

#### **2.2 Use `ArrayList` to Store Employees**
- Create an `ArrayList` to store multiple employees and managers.

```java
import java.util.ArrayList;

public class Main {
    public static void main(String[] args) {
        // Create a list of employees
        ArrayList<Employee> employees = new ArrayList<>();

        // Add employees and managers to the list
        employees.add(new Employee(101, "John Doe", 50000));
        employees.add(new Manager(102, "Jane Smith", 70000, "HR"));

        // Display details of all employees
        for (Employee emp : employees) {
            emp.displayDetails();
            if (emp instanceof Manager) {
                System.out.println("Bonus: $" + ((Manager) emp).calculateBonus());
            }
            System.out.println();
        }
    }
}
```

---

### **3. Advanced Requirements**

#### **3.1 Sort Employees by Salary Using `Comparable`**
- Implement the `Comparable` interface in the `Employee` class to sort employees by salary.

```java
class Employee implements Comparable<Employee> {
    private int id;
    private String name;
    private double salary;

    // Constructor and other methods...

    @Override
    public int compareTo(Employee other) {
        return Double.compare(this.salary, other.salary);
    }
}
```

#### **3.2 Sort Employees Using `Collections.sort`**
```java
import java.util.Collections;

public class Main {
    public static void main(String[] args) {
        ArrayList<Employee> employees = new ArrayList<>();
        employees.add(new Employee(101, "John Doe", 50000));
        employees.add(new Employee(102, "Jane Smith", 70000));
        employees.add(new Employee(103, "Alice Johnson", 60000));

        // Sort employees by salary
        Collections.sort(employees);

        // Display sorted employees
        for (Employee emp : employees) {
            emp.displayDetails();
        }
    }
}
```

#### **3.3 Implement a `Department` Class Using `HashMap`**
- Use a `HashMap` to map employees to their respective departments.

```java
import java.util.HashMap;

class Department {
    private String name;
    private HashMap<Integer, Employee> employees;

    public Department(String name) {
        this.name = name;
        this.employees = new HashMap<>();
    }

    // Add employee to department
    public void addEmployee(Employee emp) {
        employees.put(emp.getId(), emp);
    }

    // Display all employees in the department
    public void displayEmployees() {
        System.out.println("Department: " + name);
        for (Employee emp : employees.values()) {
            emp.displayDetails();
        }
    }
}
```

#### **3.4 Example Usage**
```java
public class Main {
    public static void main(String[] args) {
        Department hr = new Department("HR");
        hr.addEmployee(new Employee(101, "John Doe", 50000));
        hr.addEmployee(new Manager(102, "Jane Smith", 70000, "HR"));

        hr.displayEmployees();
    }
}
```

#### **3.5 Handle Exceptions When Reading Employee Data from a File**
- Use `try-catch` blocks to handle exceptions when reading data from a file.

```java
import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        try {
            File file = new File("employees.txt");
            Scanner scanner = new Scanner(file);

            while (scanner.hasNextLine()) {
                String line = scanner.nextLine();
                System.out.println(line);
            }

            scanner.close();
        } catch (FileNotFoundException e) {
            System.out.println("File not found: " + e.getMessage());
        }
    }
}
```

---

### **Summary of Key Concepts**
1. **Basic:**
   - Created an `Employee` class with attributes and methods.
   - Used a constructor to initialize objects.
2. **Intermediate:**
   - Used inheritance to create a `Manager` class.
   - Overrode methods and used `ArrayList` to manage employees.
3. **Advanced:**
   - Implemented `Comparable` to sort employees by salary.
   - Used `HashMap` to map employees to departments.
   - Handled file I/O exceptions.
