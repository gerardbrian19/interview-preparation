Object-Oriented Programming (OOP) in TypeScript follows the four main principles of OOP: **Encapsulation, Inheritance, Polymorphism, and Abstraction**. TypeScript enhances OOP with strong typing and additional features like interfaces and access modifiers.

### **1. Encapsulation**
Encapsulation is about bundling data (properties) and methods that operate on the data into a single unit (class) and restricting direct access to some of the object's details.

#### **Example:**
```typescript
class Person {
  private name: string; // Private property, cannot be accessed directly from outside

  constructor(name: string) {
    this.name = name;
  }

  getName(): string {
    return this.name;
  }
}

const person = new Person("John");
console.log(person.getName()); // ✅ Allowed
// console.log(person.name); // ❌ Error: Property 'name' is private
```
👉 **`private` keyword restricts access to the property.**  
👉 **Encapsulation ensures data is accessed through defined methods (getters/setters).**

---

### **2. Inheritance**
Inheritance allows a class (child) to inherit properties and methods from another class (parent), promoting code reuse.

#### **Example:**
```typescript
class Animal {
  constructor(protected name: string) {}

  makeSound(): void {
    console.log("Some sound...");
  }
}

class Dog extends Animal {
  constructor(name: string) {
    super(name); // Calls the parent class constructor
  }

  makeSound(): void {
    console.log(`${this.name} barks!`);
  }
}

const dog = new Dog("Buddy");
dog.makeSound(); // Buddy barks!
```
👉 **`extends` keyword allows `Dog` to inherit from `Animal`.**  
👉 **`super()` calls the parent class constructor.**  
👉 **Method overriding is demonstrated with `makeSound()`.**

---

### **3. Polymorphism**
Polymorphism allows methods to have different implementations in different classes, even if they share the same name.

#### **Example:**
```typescript
class Bird {
  fly(): void {
    console.log("This bird can fly.");
  }
}

class Penguin extends Bird {
  fly(): void {
    console.log("Penguins cannot fly!");
  }
}

const birds: Bird[] = [new Bird(), new Penguin()];

birds.forEach((bird) => bird.fly());
```
👉 **The same method `fly()` behaves differently in `Bird` and `Penguin`.**  
👉 **This enables dynamic method behavior.**

---

### **4. Abstraction**
Abstraction allows defining a blueprint (abstract class or interface) without providing a full implementation, forcing subclasses to implement the required methods.

#### **Using Abstract Classes:**
```typescript
abstract class Vehicle {
  constructor(protected brand: string) {}

  abstract startEngine(): void; // Abstract method must be implemented in child classes

  getBrand(): string {
    return this.brand;
  }
}

class Car extends Vehicle {
  startEngine(): void {
    console.log(`${this.brand} engine started.`);
  }
}

const car = new Car("Toyota");
car.startEngine(); // Toyota engine started.
```
👉 **`abstract` classes cannot be instantiated directly.**  
👉 **Child classes must implement abstract methods.**

#### **Using Interfaces:**
```typescript
interface Shape {
  getArea(): number;
}

class Circle implements Shape {
  constructor(private radius: number) {}

  getArea(): number {
    return Math.PI * this.radius * this.radius;
  }
}

const circle = new Circle(5);
console.log(circle.getArea()); // 78.5398
```
👉 **`interface` defines a contract that classes must follow.**  
👉 **Unlike abstract classes, interfaces cannot contain implementation details.**

---

### **Bonus: Additional OOP Features in TypeScript**
1. **Access Modifiers:**
   - `public`: Accessible from anywhere (default).
   - `private`: Accessible only within the class.
   - `protected`: Accessible within the class and subclasses.

2. **Static Properties & Methods:**
   ```typescript
   class MathUtil {
     static PI = 3.14;
     static square(num: number): number {
       return num * num;
     }
   }

   console.log(MathUtil.PI); // 3.14
   console.log(MathUtil.square(4)); // 16
   ```
   👉 **Static properties/methods belong to the class, not an instance.**

3. **Getters and Setters:**
   ```typescript
   class Person {
     private _age: number = 0;

     get age(): number {
       return this._age;
     }

     set age(value: number) {
       if (value < 0) throw new Error("Age cannot be negative");
       this._age = value;
     }
   }

   const p = new Person();
   p.age = 25; // ✅ Allowed
   console.log(p.age); // 25
   ```
   👉 **Encapsulates property access with validation.**

---