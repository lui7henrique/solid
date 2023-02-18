# (I) Dependency inversion principle

> Depend upon abstractions, not concretions.

In our applications we can differentiate between two types of classes:

- Low level classes which do operations like reading from a database or saving a file.
- High level classes which implement the business logic and use those low level classes.

What this principle proposes is that high level classes depend on interfaces instead of concrete implementations. This is easier to understand with an example.

#

In the following bad example we have the class that saves orders in a database. The class depends directly on the low level class.
`OrderService`
`OrderService`
`MySQLDatabase`

If in the future we wanted to change the database that we are using we would have to modify the class.
`OrderService`

```typescript
class OrderService {
  database: MySQLDatabase;

  // constructor

  save(order: Order): void {
    if (order.id === undefined) {
      this.database.insert(order);
    } else {
      this.database.update(order);
    }
  }
}

class MySQLDatabase {
  insert(order: Order) {
    // insert
  }

  update(order: Order) {
    // update
  }
}
```

We can improve this by creating an interface and make the class dependant of it. This way, we are inverting the dependency. Now the high level class depends on an abstraction instead of a low level class. `OrderService`

```typescript
class OrderService {
  database: Database;

  // constructor

  save(order: Order): void {
    this.database.save(order);
  }
}

interface Database {
  save(order: Order): void;
}

class MySQLDatabase implements Database {
  save(order: Order) {
    if (order.id === undefined) {
      // insert
    } else {
      // update
    }
  }
}
```

Now we can add new databases without modifying the class `OrderService`
