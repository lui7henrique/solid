# Open-closed Principle

> Classes should be open for extension but closed for modification.

The idea behind this principle is that existing classes should be extended but not modified. By modifying existing classes you risk breaking code which was already tested and reviewed.

The main benefit of this principle is that you can add new features without touching old code. This way you won't break the current usage of those original classes.

#

In the following bad example we can see how the `Order` class is calculating shipping costs for each of the existing shipping methods.

```typescript
class Order {
  id: number;
  items: string[];
  shipping: string;

  // constructor

  getTotalCost(): number {
    // calculates total cost
  }

  getShippingCosts(): number {
    const totalCost = this.getTotalCost();

    if (this.shipping === "ground") {
      return totalCost > 50 ? 0 : 5.95;
    }

    if (this.shipping === "air") {
      return 10.95;
    }

    return 0;
  }
}
```

If we wanted to add a new shipping method, we would have to modify the `Order` class. Following the Open/Closed Principle we can solve this by creating an interface and a class that implements it for each shipping method.

```typescript
class Order {
  id: number;
  items: string[];
  shipping: Shipping;

  // constructor

  getTotalCost(): number {
    // calculates total cost
  }
}

interface Shipping {
  getShippingCosts(totalCost: number): number;
}

class Ground implements Shipping {
  getShippingCosts(totalCost: number): number {
    return totalCost > 50 ? 0 : 5.95;
  }
}

class Air implements Shipping {
  getShippingCosts(): number {
    return 10.95;
  }
}

class PickUpInStore implements Shipping {
  getShippingCosts(): number {
    return 0;
  }
}
```
