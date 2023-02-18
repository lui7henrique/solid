# (S) Single Responsibility Principle in TypeScript

> A class should have just one reason to change.

Every class in our code should be responsible for just a single part of the application. By following this principle we reduce the complexity of our code.

If a class is responsible of multiple parts of our app, it will have to be changed frequently. Therefore, changing one part of the class increases the risk of breaking other parts of itself. The solution is to divide it into multiple classes, each one with one responsibility.

#

In the following bad example we can see how the `Student` class has two responsibilities: managing the student data and the course data.

```typescript
class Student {
  id: number;
  name: string;
  courseId: number;
  courseName: string;
  courseSubjects: string[];

  // constructor

  getCourseSubjects(): string {
    return this.courseSubjects.join(", ");
  }
}
```

Following the Single Responsibility Principle we can improve this by moving the course data to its own class.

```typescript
class Student {
  id: number;
  name: string;
  course: Course;

  // constructor
}

class Course {
  id: number;
  name: string;
  subjects: string[];

  // constructor

  getCourseSubjects(): string {
    return this.subjects.join(", ");
  }
}
```
