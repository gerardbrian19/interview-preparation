---

### **1. Abstract vs. Virtual Methods – Definition and Usage**

#### **Abstract Methods:**

- **Definition:** Abstract methods are declared in an abstract class but do not have an implementation. They must be overridden in derived classes.
- **Usage:** When you want to enforce that all subclasses must provide their own implementation of a method.
- **Example in TypeScript:**

  ```typescript
  abstract class Vehicle {
    abstract move(): void; // No implementation
  }

  class Car extends Vehicle {
    move(): void {
      console.log("Car is moving");
    }
  }
  ```

#### **Virtual Methods (Polymorphism in TypeScript via Method Overriding):**

- **Definition:** Virtual methods are methods that have a base implementation but can be overridden in derived classes.
- **Usage:** When you want to provide a default behavior that can be customized by subclasses.
- **Example:**

  ```typescript
  class Animal {
    move(): void {
      console.log("Animal moves");
    }
  }

  class Dog extends Animal {
    move(): void {
      console.log("Dog runs");
    }
  }

  let animal: Animal = new Dog();
  animal.move(); // "Dog runs"
  ```

📌 **Key Difference:**

- **Abstract methods have no default implementation**, whereas **virtual methods have a default implementation that can be overridden**.

---

### **2. Fundamentals of OOP and Design Patterns**

#### **OOP Principles:**

1. **Encapsulation** – Hiding implementation details and exposing only necessary parts.

   ```typescript
   class User {
     private password: string; // Hidden from outside

     constructor(public name: string, password: string) {
       this.password = password;
     }
   }
   ```

2. **Inheritance** – Allows a class to inherit properties and methods from another class.

   ```typescript
   class Vehicle {
     move(): void {
       console.log("Moving...");
     }
   }

   class Car extends Vehicle {}
   ```

3. **Polymorphism** – Allows methods to be overridden and used dynamically.

   ```typescript
   class Shape {
     draw(): void {
       console.log("Drawing shape");
     }
   }

   class Circle extends Shape {
     draw(): void {
       console.log("Drawing circle");
     }
   }
   ```

4. **Abstraction** – Hides implementation details while exposing essential features.

   ```typescript
   abstract class Payment {
     abstract process(): void;
   }

   class CreditCardPayment extends Payment {
     process(): void {
       console.log("Processing Credit Card Payment");
     }
   }
   ```

#### **Common Design Patterns:**

- **Singleton** – Ensures a class has only one instance.

  ```typescript
  class Singleton {
    private static instance: Singleton;
    private constructor() {}

    static getInstance(): Singleton {
      if (!this.instance) {
        this.instance = new Singleton();
      }
      return this.instance;
    }
  }
  ```

- **Factory** – Creates objects without specifying the exact class.

  ```typescript
  class AnimalFactory {
    static createAnimal(type: string): Animal {
      if (type === "dog") return new Dog();
      else return new Cat();
    }
  }
  ```

- **Observer** – Notifies subscribers of changes.

  ```typescript
  class Subject {
    private observers: (() => void)[] = [];

    subscribe(observer: () => void) {
      this.observers.push(observer);
    }

    notify() {
      this.observers.forEach((observer) => observer());
    }
  }
  ```

---

### **3. Difference Between `const` and `readonly` in TypeScript**

| Feature               | `const` (for variables)                                              | `readonly` (for class properties)         |
| --------------------- | -------------------------------------------------------------------- | ----------------------------------------- |
| Scope                 | Block-scoped                                                         | Class-scoped                              |
| Usage                 | Used for variables                                                   | Used for class properties                 |
| Can it be reassigned? | ❌ No                                                                | ❌ No                                     |
| Can it be modified?   | Primitive values: ❌ No, Objects: ✅ Yes (but properties can change) | Objects: ✅ Yes (but only in constructor) |
| Example               | `const x = 5;`                                                       | `class A { readonly name = "John"; }`     |

✅ **Use `const`** for values that won’t change.  
✅ **Use `readonly`** for properties inside classes that should not be modified after initialization.

---

### **4. Pipes in Angular Dependency Injection**

- Pipes **transform** data in Angular templates.
- Custom pipes can be **injectable** by adding `@Injectable()`.

🔹 **Pure Pipes:**

- Stateless – runs only when inputs change.
- Example: Formatting a string.
  ```typescript
  @Pipe({ name: "capitalize" })
  export class CapitalizePipe implements PipeTransform {
    transform(value: string): string {
      return value.charAt(0).toUpperCase() + value.slice(1);
    }
  }
  ```

🔹 **Impure Pipes:**

- Runs on every change detection, even if inputs don't change.
- Example: Filtering a list dynamically.
  ```typescript
  @Pipe({ name: "filter", pure: false })
  export class FilterPipe implements PipeTransform {
    transform(items: any[], searchTerm: string): any[] {
      return items.filter((item) => item.includes(searchTerm));
    }
  }
  ```

📌 **Dependency Injection in Pipes:**  
If a pipe requires services, use `@Injectable()`.

```typescript
@Pipe({ name: "fetchData" })
@Injectable()
export class FetchDataPipe implements PipeTransform {
  constructor(private http: HttpClient) {}

  transform(url: string): Observable<any> {
    return this.http.get(url);
  }
}
```

---

### **5. ElasticSearch**

- **Definition:** A distributed search engine for full-text searches, analytics, and log monitoring.
- **Key Features:**
  - NoSQL
  - REST API-based
  - Fast indexing & searching
- **Basic Query Example:**
  ```json
  {
    "query": {
      "match": {
        "title": "Angular"
      }
    }
  }
  ```
- **Use Cases:**
  - Log monitoring (e.g., ELK Stack)
  - Full-text search (e.g., in e-commerce)

---

### **6. Software Architectural Design**

- **Definition:** The high-level structure of software, including components, relationships, and interactions.

🔹 **Common Architectures:**

1. **Monolithic:** One large application.
2. **Microservices:** Small, independent services.
3. **Layered Architecture:** Divides the system into Presentation, Business, and Data layers.
4. **MVC (Model-View-Controller):** Separates data (Model), UI (View), and logic (Controller).

🔹 **Best Practices:**

- Separation of Concerns
- Loose Coupling
- Scalability
- Maintainability

✅ **Example of Angular Software Architecture:**

- **Components:** UI logic
- **Services:** Business logic & API calls
- **Modules:** Organizes features
- **State Management:** NgRx for large apps

---
