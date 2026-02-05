# SOLID

As the project codebase grows, it becomes more and more difficult to manage, but that is the problem, the SOLID principles are meant to solve. 

The term SOLID is an acronym for five design principles intended to make the software codebase more understandable, flexible and maintainable.

Let us learn each principle with some examples so that we can understand how we can implement these priniciples in our codebase.


## 1. Single-Responsibility Principle

It states:

    A class should have only one reason to change.

Bad design:
```java
class Invoice {
    int amount;

    int calculateTotal() { return amount + 100; }
    void printInvoice() { System.out.println("Printing..."); }
    void saveToDB() { System.out.println("Saving to DB"); }
}
```

This class calculates total, prints invoice, and saves to db as well, which are too many responsiblities for a single class.

Good design

```java
class Invoice {
    int amount;
    int calculateTotal() { return amount + 100; }
}

class InvoicePrinter {
    void print(Invoice invoice) { System.out.println("Printing..."); }
}

class InvoiceRepository {
    void save(Invoice invoice) { System.out.println("Saving to DB"); }
}
```
Now each class has one job, which makes it easier for testing and maintenance.


## 2. Open-closed Principle

Definition:

    Objects or entities should be open for extension but closed for modification.

Meaning: Add new features without changing existing code.

Bad design
```java
class AreaCalculator {
    double calculate(String shape) {
        if(shape.equals("circle")) return 3.14 * 2 * 2;
        if(shape.equals("rectangle")) return 4 * 5;
        return 0;
    }
}
```
This is bad design because, if we want to add a new shape, we must modify this class.

Good design

```java
interface Shape {
    double area();
}

class Circle implements Shape {
    public double area() { return Math.PI * 2 * 2; }
}

class Rectangle implements Shape {
    public double area() { return 4 * 5; }
}
```

Add new shape:

```java
class Triangle implements Shape {
    public double area() { return 0.5 * 4 * 5; }
}
```

Now no existing code was changed, but the logic to calculate the area was included in its shape class only.

## 3. Liskov Substitution Principle

Definition:

    Objects of a subclass should replace parent class objects without breaking behavior.

Bad example

```java
class Bird {
    void fly() {}
}

class Ostrich extends Bird {
    void fly() { throw new UnsupportedOperationException(); }
}
```

Ostrich cannot fly and this violates Liskov Substitution principle.

Good design

```java
class Bird {
    void eat() {}
}

class FlyingBird extends Bird {
    void fly() {}
}

class Sparrow extends FlyingBird {}
class Ostrich extends Bird {}
```

Now substitution works correctly.


## 4. Interface Segregation Principle

Definition:

Clients should not be forced to implement methods they don’t use.

Bad design

```java
interface Worker {
    void work();
    void eat();
}

class Robot implements Worker {
    public void work() {}
    public void eat() {}
}
```
But robots dont eat, but was forced to implement the eat class.

Good design

```java
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}

class Human implements Workable, Eatable {
    public void work() {}
    public void eat() {}
}

class Robot implements Workable {
    public void work() {}
}
```

Now we have small, focused interfaces, and classes can implement only those they need.

## 5. Dependency Inversion Principle

Definition:

    High-level modules should not depend on low-level modules. Both should depend on abstractions (interfaces).

Bad design

```java
class WiredKeyboard {
    void type() {}
}

class Computer {
    WiredKeyboard keyboard = new WiredKeyboard(); // tightly coupled
}
```

If type of keyboard changes we will have to change the computer class as well.

Good design

```java
interface Keyboard {
    void type();
}

class WiredKeyboard implements Keyboard {
    public void type() {}
}

class WirelessKeyboard implements Keyboard {
    public void type() {}
}

class Computer {
    Keyboard keyboard;

    Computer(Keyboard keyboard) {
        this.keyboard = keyboard;
    }
}
```

Now any keyboard can be used, the computer class, the code is more flexible and there is loose coupling.

## References

[Digital Ocean Article on SOLID Principles](https://www.digitalocean.com/community/conceptual_articles/s-o-l-i-d-the-first-five-principles-of-object-oriented-design)

[SOLID Design Principles Youtube Playlist ](https://www.youtube.com/playlist?list=PL6n9fhu94yhXjG1w2blMXUzyDrZ_eyOme)