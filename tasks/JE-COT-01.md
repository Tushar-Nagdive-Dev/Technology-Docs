
### **Common Task 1: Employee Management System**
You are building an Employee Management System. The system should store employee details, calculate salaries, and manage departments.

1. **Basic:**
   - How would you create a class `Employee` with attributes like `id`, `name`, and `salary`?
   - Write a method to display the details of an employee.
   - How would you use a constructor to initialize an `Employee` object?

2. **Intermediate:**
   - How would you use inheritance to create a `Manager` class that extends `Employee` and adds a `department` attribute?
   - Implement method overriding to calculate a bonus for the `Manager` class.
   - How would you use `ArrayList` to store and retrieve a list of employees?

3. **Advanced:**
   - How would you use `Comparable` or `Comparator` to sort employees by salary?
   - Implement a `Department` class that uses a `HashMap` to map employees to their respective departments.
   - How would you handle exceptions when reading employee data from a file?

---

### **Common Task 2: Online Shopping Cart**
You are developing an online shopping cart system where users can add products, apply discounts, and checkout.

1. **Basic:**
   - Create a `Product` class with attributes like `productId`, `name`, `price`, and `quantity`.
   - Write a method to calculate the total cost of all products in the cart.
   - How would you use a `for` loop to display all products in the cart?

2. **Intermediate:**
   - How would you use interfaces to implement discount strategies (e.g., `FlatDiscount`, `PercentageDiscount`)?
   - Implement a `ShoppingCart` class that uses a `LinkedList` to store products.
   - How would you use `enum` to define product categories (e.g., `ELECTRONICS`, `CLOTHING`)?

3. **Advanced:**
   - How would you use multithreading to simulate multiple users adding products to the cart simultaneously?
   - Implement a `Checkout` class that uses `synchronized` methods to ensure thread-safe operations.
   - How would you use Java Streams to filter products based on price or category?

---

### **Common Task 3: Banking Application**
You are building a banking application that allows users to create accounts, deposit/withdraw money, and check balances.

1. **Basic:**
   - Create an `Account` class with attributes like `accountNumber`, `accountHolderName`, and `balance`.
   - Write methods to deposit and withdraw money, ensuring the balance cannot go negative.
   - How would you use encapsulation to protect the `balance` attribute?

2. **Intermediate:**
   - How would you use polymorphism to handle different account types (e.g., `SavingsAccount`, `CurrentAccount`)?
   - Implement a `Transaction` class to log all transactions (deposit, withdrawal) with timestamps.
   - How would you use `ArrayList` to store all accounts and retrieve them by account number?

3. **Advanced:**
   - How would you use Java’s `java.time` API to calculate the interest for a `SavingsAccount` based on the account creation date?
   - Implement a `Bank` class that uses a `ConcurrentHashMap` to handle multiple accounts in a thread-safe manner.
   - How would you use custom exceptions to handle insufficient balance or invalid account numbers?

---

### **Common Task 4: Library Management System**
You are developing a library management system to manage books, members, and borrowing records.

1. **Basic:**
   - Create a `Book` class with attributes like `bookId`, `title`, `author`, and `isAvailable`.
   - Write a method to check if a book is available for borrowing.
   - How would you use a `switch` statement to handle different menu options (e.g., add book, borrow book)?

2. **Intermediate:**
   - How would you use composition to create a `Library` class that contains a list of `Book` objects?
   - Implement a `Member` class and use aggregation to associate members with borrowed books.
   - How would you use `HashSet` to ensure no duplicate books are added to the library?

3. **Advanced:**
   - How would you use Java’s `Stream` API to find all books by a specific author?
   - Implement a `BorrowRecord` class that uses `LocalDate` to track borrowing and return dates.
   - How would you use serialization to save and load the library’s data to/from a file?

---

### **Common Task 5: Social Media Platform**
You are building a social media platform where users can create profiles, post messages, and follow other users.

1. **Basic:**
   - Create a `User` class with attributes like `userId`, `username`, and `email`.
   - Write a method to validate the email format using regular expressions.
   - How would you use a `while` loop to allow users to post multiple messages?

2. **Intermediate:**
   - How would you use a `HashMap` to store followers for each user?
   - Implement a `Post` class and use composition to associate posts with users.
   - How would you use `LinkedList` to display a user’s feed (list of posts)?

3. **Advanced:**
   - How would you use Java’s `ExecutorService` to handle multiple users posting simultaneously?
   - Implement a `Notification` class that uses `Observer` design pattern to notify followers of new posts.
   - How would you use Java’s `Reflection` API to dynamically inspect user profiles?

---

### **Common Task 6: Flight Booking System**
You are developing a flight booking system where users can search for flights, book tickets, and view bookings.

1. **Basic:**
   - Create a `Flight` class with attributes like `flightNumber`, `source`, `destination`, and `seatsAvailable`.
   - Write a method to check if a flight has available seats.
   - How would you use a `do-while` loop to allow users to retry booking if seats are unavailable?

2. **Intermediate:**
   - How would you use `HashMap` to store flight details and retrieve them by flight number?
   - Implement a `Ticket` class and use aggregation to associate tickets with flights and users.
   - How would you use `enum` to define flight classes (e.g., `ECONOMY`, `BUSINESS`)?

3. **Advanced:**
   - How would you use Java’s `Optional` class to handle cases where a flight is not found?
   - Implement a `BookingSystem` class that uses `ConcurrentHashMap` to handle concurrent bookings.
   - How would you use Java’s `Lambda Expressions` to filter flights based on user preferences?

