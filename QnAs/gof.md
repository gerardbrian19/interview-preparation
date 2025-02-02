The **Gang of Four (GoF) design patterns** are a set of 23 software design patterns introduced in the book *Design Patterns: Elements of Reusable Object-Oriented Software* by Erich Gamma, Richard Helm, Ralph Johnson, and John Vlissides.

These patterns are categorized into **Creational, Structural, and Behavioral** patterns. Below is how they can be implemented in **TypeScript**.

---

## **1. Creational Patterns**  
These patterns focus on **object creation mechanisms** to enhance flexibility and reuse.

### **1.1 Factory Method**
Encapsulates object creation in a method, allowing subclasses to decide which class to instantiate.

```typescript
abstract class Product {
  abstract use(): string;
}

class ConcreteProductA extends Product {
  use(): string {
    return "Using Product A";
  }
}

class ConcreteProductB extends Product {
  use(): string {
    return "Using Product B";
  }
}

class ProductFactory {
  static createProduct(type: string): Product {
    if (type === "A") return new ConcreteProductA();
    if (type === "B") return new ConcreteProductB();
    throw new Error("Invalid product type");
  }
}

// Usage
const product = ProductFactory.createProduct("A");
console.log(product.use());
```

---

### **1.2 Abstract Factory**
Provides an interface for creating families of related objects.

```typescript
interface Button {
  render(): void;
}

class WindowsButton implements Button {
  render() {
    console.log("Render Windows button");
  }
}

class MacButton implements Button {
  render() {
    console.log("Render Mac button");
  }
}

interface GUIFactory {
  createButton(): Button;
}

class WindowsFactory implements GUIFactory {
  createButton(): Button {
    return new WindowsButton();
  }
}

class MacFactory implements GUIFactory {
  createButton(): Button {
    return new MacButton();
  }
}

// Usage
const factory: GUIFactory = new MacFactory();
const button = factory.createButton();
button.render();
```

---

### **1.3 Singleton**
Ensures a class has only **one instance** and provides a global point of access.

```typescript
class Singleton {
  private static instance: Singleton;

  private constructor() {}

  static getInstance(): Singleton {
    if (!Singleton.instance) {
      Singleton.instance = new Singleton();
    }
    return Singleton.instance;
  }

  log() {
    console.log("Singleton instance");
  }
}

// Usage
const singleton1 = Singleton.getInstance();
const singleton2 = Singleton.getInstance();
console.log(singleton1 === singleton2); // true
```

---

## **2. Structural Patterns**  
These patterns deal with **object composition** to create flexible and reusable structures.

### **2.1 Adapter**
Allows incompatible interfaces to work together.

```typescript
class LegacyPrinter {
  printText(text: string) {
    console.log(`Printing: ${text}`);
  }
}

class NewPrinter {
  print(text: string) {
    console.log(`Modern printing: ${text}`);
  }
}

// Adapter to make NewPrinter compatible with LegacyPrinter
class PrinterAdapter extends LegacyPrinter {
  private newPrinter = new NewPrinter();

  printText(text: string) {
    this.newPrinter.print(text);
  }
}

// Usage
const printer: LegacyPrinter = new PrinterAdapter();
printer.printText("Hello, Adapter!");
```

---

### **2.2 Decorator**
Adds behavior to objects dynamically.

```typescript
interface Coffee {
  cost(): number;
}

class SimpleCoffee implements Coffee {
  cost(): number {
    return 5;
  }
}

class MilkDecorator implements Coffee {
  constructor(private coffee: Coffee) {}

  cost(): number {
    return this.coffee.cost() + 2;
  }
}

// Usage
const coffee = new SimpleCoffee();
const milkCoffee = new MilkDecorator(coffee);
console.log(milkCoffee.cost()); // 7
```

---

### **2.3 Proxy**
Controls access to another object.

```typescript
interface Service {
  request(): void;
}

class RealService implements Service {
  request() {
    console.log("Executing real request...");
  }
}

class ProxyService implements Service {
  private realService: RealService;

  constructor() {
    this.realService = new RealService();
  }

  request() {
    console.log("Proxy: Checking access before making request...");
    this.realService.request();
  }
}

// Usage
const proxy = new ProxyService();
proxy.request();
```

---

## **3. Behavioral Patterns**  
These patterns focus on **communication between objects**.

### **3.1 Observer**
Defines a dependency between objects where one changes state and notifies dependents.

```typescript
interface Observer {
  update(data: string): void;
}

class ConcreteObserver implements Observer {
  update(data: string) {
    console.log(`Received update: ${data}`);
  }
}

class Subject {
  private observers: Observer[] = [];

  addObserver(observer: Observer) {
    this.observers.push(observer);
  }

  notify(data: string) {
    this.observers.forEach((observer) => observer.update(data));
  }
}

// Usage
const subject = new Subject();
const observer1 = new ConcreteObserver();
subject.addObserver(observer1);
subject.notify("New Data!");
```

---

### **3.2 Strategy**
Encapsulates interchangeable algorithms.

```typescript
interface PaymentStrategy {
  pay(amount: number): void;
}

class CreditCardPayment implements PaymentStrategy {
  pay(amount: number) {
    console.log(`Paid ${amount} using Credit Card`);
  }
}

class PayPalPayment implements PaymentStrategy {
  pay(amount: number) {
    console.log(`Paid ${amount} using PayPal`);
  }
}

class PaymentProcessor {
  constructor(private strategy: PaymentStrategy) {}

  processPayment(amount: number) {
    this.strategy.pay(amount);
  }
}

// Usage
const payment = new PaymentProcessor(new CreditCardPayment());
payment.processPayment(100);
```

---

### **3.3 Command**
Encapsulates a request as an object.

```typescript
interface Command {
  execute(): void;
}

class Light {
  turnOn() {
    console.log("Light is On");
  }
  turnOff() {
    console.log("Light is Off");
  }
}

class LightOnCommand implements Command {
  constructor(private light: Light) {}

  execute() {
    this.light.turnOn();
  }
}

// Usage
const light = new Light();
const lightOnCommand = new LightOnCommand(light);
lightOnCommand.execute();
```

---

## **Summary**
| Category        | Pattern Types |
|----------------|--------------|
| **Creational** | Factory Method, Abstract Factory, Singleton |
| **Structural** | Adapter, Decorator, Proxy |
| **Behavioral** | Observer, Strategy, Command |