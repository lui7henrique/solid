# (L) Liskov Substitution Principle

> When extending a class, remember that you should be able to pass objects of the subclass in place of objects of the parent class without breaking the client code.

The goal of this principle is that subclasses remain compatible with the behavior of the parent class. Subclasses should extend the behavior of the parent class and not replace it by something different.

If you follow this principle you will be able to replace a parent class by any of its subclasses without breaking the client code..

#

Imagine that we have an application that accepts orders. There are two possible states for an order: draft or confirmed. If an order was not confirmed, it cannot be payed.

In the following example we are breaking the substitution principle because the parent class has the method markAsPaid which does not throw any errors. On the contrary, the subclass DraftOrder throws an error in that method because draft orders cannot be payed. Replacing the parent class Order by its subclass DraftOrder may break the code if we were calling markAsPaid.

```typescript
class Order {
  id: number;
  items: string[];
  payed: boolean;

  // constructor

  markAsPaid(): void {
    this.payed = true;
  }
}

class DraftOrder extends Order {
  markAsPaid(): void {
    throw new Error("Draft orders can't be payed");
  }
}
```

We can improve this by making draft orders the parent class and confirmed orders the subclass. This way it is possible to replace the parent class by the subclass without breaking the code.

```typescript
class Order {
  id: number;
  items: string[];

  // constructor
}

class ConfirmedOrder extends Order {
  payed: boolean;

  // constructor

  markAsPaid(): void {
    this.payed = true;
  }
}
```

#

In others words, if the classes Cat and Dog extend the class Animal, then we would expect all of the functionality held within the Animal class to behave normally for a Cat and Dog object.

A classic example of a Liskov substitution violation is the “square & rectangle problem”. In this problem, it is posed that a Square class can inherit from a Rectangle class. On the face of it, this makes sense; both shapes have two sides, and both calculate their area by multiplying their sides by each other.

But the problem arises when we try to utilise some Rectangle functionality on a Square object. Let’s look at an example:

```javascript
class Rectangle {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }

  setHeight(newHeight) {
    this.height = newHeight;
  }
}

class Square extends Rectangle {}

const rectangle = new Rectangle(4, 5);
const square = new Square(4, 4);

console.log(`Height: ${rectangle.height}, Width: ${rectangle.width}`);
// Outputs 'Height: 4, Width: 5' (correct)

console.log(`Height: ${square.height}, Width: ${square.width}`); // Outputs 'Height: 4, Width: 4' (correct)

square.setHeight(5);
console.log(`Height: ${square.height}, Width: ${square.width}`); // Outputs 'Height: 5, Width: 4' (wrong)
```

In this example we initialise a Rectangle and Square, and output their dimensions. We then call the Rectangle.setHeight() on the Square object, and output its dimensions again. What we find is that the square now has a different height than its length, which of course makes for an invalid square.

This can be solved, using polymorphism, an if statement in the Rectangle class, or a variety of other methods. But the real cause of the issue is that Square is not a good child class of Rectangle, and that in reality, perhaps both shapes should inherit from a Shape class instead.
