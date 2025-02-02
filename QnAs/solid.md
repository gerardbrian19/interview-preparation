The **SOLID principles** are a set of five design principles that help developers write maintainable, scalable, and flexible code. They were introduced by **Robert C. Martin (Uncle Bob)** and are widely used in object-oriented programming (OOP).

### **SOLID Acronym:**
1. **S** – **Single Responsibility Principle (SRP)**
2. **O** – **Open/Closed Principle (OCP)**
3. **L** – **Liskov Substitution Principle (LSP)**
4. **I** – **Interface Segregation Principle (ISP)**
5. **D** – **Dependency Inversion Principle (DIP)**

---

### **1. Single Responsibility Principle (SRP)**
- **Definition**: A class (or function) should have only one reason to change.
- **Explanation**: Each class should have **only one responsibility** and **one purpose**.
- **Example (Bad)**
  ```typescript
  class InvoiceService {
    calculateTotal(invoice: Invoice) { /* logic */ }
    saveToDatabase(invoice: Invoice) { /* logic */ }
    sendEmailNotification(invoice: Invoice) { /* logic */ }
  }
  ```
  - This class does **too many things** (calculation, saving, and sending emails).
  
- **Example (Good)**
  ```typescript
  class InvoiceCalculator { calculateTotal(invoice: Invoice) { /* logic */ } }
  class InvoiceRepository { saveToDatabase(invoice: Invoice) { /* logic */ } }
  class EmailService { sendEmail(invoice: Invoice) { /* logic */ } }
  ```
  - Each class now has **only one responsibility**.

---

### **2. Open/Closed Principle (OCP)**
- **Definition**: A class should be **open for extension but closed for modification**.
- **Explanation**: You should be able to add new functionality **without modifying existing code**.
- **Example (Bad)**: Adding a new discount type requires modifying existing code.
  ```typescript
  class DiscountService {
    getDiscount(type: string, price: number) {
      if (type === "VIP") return price * 0.9;
      if (type === "Student") return price * 0.8;
      return price;
    }
  }
  ```
  - **Problem**: Every time a new discount type is added, you have to modify this class.

- **Example (Good)**
  ```typescript
  interface DiscountStrategy {
    apply(price: number): number;
  }

  class VipDiscount implements DiscountStrategy {
    apply(price: number) { return price * 0.9; }
  }

  class StudentDiscount implements DiscountStrategy {
    apply(price: number) { return price * 0.8; }
  }

  class DiscountService {
    getDiscount(strategy: DiscountStrategy, price: number) {
      return strategy.apply(price);
    }
  }
  ```
  - **Now, adding new discount types doesn’t require modifying existing code.**

---

### **3. Liskov Substitution Principle (LSP)**
- **Definition**: Subclasses should be able to replace their parent class **without breaking the program**.
- **Explanation**: A subclass should behave like its parent class **without unexpected behavior**.
- **Example (Bad)**
  ```typescript
  class Rectangle {
    constructor(public width: number, public height: number) {}
    setWidth(width: number) { this.width = width; }
    setHeight(height: number) { this.height = height; }
    getArea() { return this.width * this.height; }
  }

  class Square extends Rectangle {
    setWidth(width: number) { 
      this.width = width;
      this.height = width;
    }
    setHeight(height: number) { 
      this.width = height;
      this.height = height;
    }
  }
  ```
  - **Problem**: `Square` modifies the behavior of `Rectangle`, which breaks the expected logic.
  - If you expect a `Rectangle`, but get a `Square`, calling `setWidth` will unexpectedly modify both width and height.

- **Example (Good)**
  ```typescript
  interface Shape {
    getArea(): number;
  }

  class Rectangle implements Shape {
    constructor(public width: number, public height: number) {}
    getArea() { return this.width * this.height; }
  }

  class Square implements Shape {
    constructor(public side: number) {}
    getArea() { return this.side * this.side; }
  }
  ```
  - **Now, both `Rectangle` and `Square` follow the same behavior.**

---

### **4. Interface Segregation Principle (ISP)**
- **Definition**: Clients should not be forced to depend on interfaces they don’t use.
- **Explanation**: **Break large interfaces** into **smaller, more specific** ones.
- **Example (Bad)**
  ```typescript
  interface Worker {
    work(): void;
    eat(): void;
  }

  class Developer implements Worker {
    work() { console.log("Coding..."); }
    eat() { console.log("Eating lunch..."); }
  }

  class Robot implements Worker {
    work() { console.log("Building stuff..."); }
    eat() { throw new Error("Robots don't eat!"); } // ❌ Breaks the contract
  }
  ```
  - **Problem**: Robots don’t eat, but they are forced to implement `eat()`.

- **Example (Good)**
  ```typescript
  interface Workable { work(): void; }
  interface Eatable { eat(): void; }

  class Developer implements Workable, Eatable {
    work() { console.log("Coding..."); }
    eat() { console.log("Eating lunch..."); }
  }

  class Robot implements Workable {
    work() { console.log("Building stuff..."); }
  }
  ```
  - **Now, classes implement only the interfaces they need.**

---

### **5. Dependency Inversion Principle (DIP)**
- **Definition**: Depend on abstractions (interfaces), not concrete implementations.
- **Explanation**: **High-level modules** should not depend on **low-level modules**. Both should depend on **abstractions**.
- **Example (Bad)**
  ```typescript
  class MySQLDatabase {
    save(data: string) { console.log("Saving to MySQL..."); }
  }

  class UserService {
    constructor(private db: MySQLDatabase) {}

    saveUser(user: string) {
      this.db.save(user);
    }
  }
  ```
  - **Problem**: `UserService` is tightly coupled to `MySQLDatabase`. Changing to `PostgreSQL` requires modifying `UserService`.

- **Example (Good)**
  ```typescript
  interface Database {
    save(data: string): void;
  }

  class MySQLDatabase implements Database {
    save(data: string) { console.log("Saving to MySQL..."); }
  }

  class PostgreSQLDatabase implements Database {
    save(data: string) { console.log("Saving to PostgreSQL..."); }
  }

  class UserService {
    constructor(private db: Database) {}

    saveUser(user: string) {
      this.db.save(user);
    }
  }
  ```
  - **Now, `UserService` works with any database implementation without modification.**

---