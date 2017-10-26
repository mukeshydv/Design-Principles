# Object Oriented Design Principles

A good design is most important step of Software Development life cycle, and to have a good design we must follow some set of guidelines. According to Robert Martin in "Agile Software Development: Principles, Patterns, and Practices" there are 3 main reasons of bad design that should be avoided:

1. **Rigidity** - System is hard to change, beacause it affects many part of the system.
2. **Fragility** - Change to a part affects an unexpected part of the system.
3. **Immobility** - A module is hard to reuse in other System because it cannot be disentangled from the current System.


## SOLID Principles
To solve these problems there are principles called **SOLID**. It consist of 5 principle:

1. **S** - Single Responsibility Principle
2. **O** - Open Close Principle
3. **L** - Liskov's Substitution Principle
4. **I** - Interface Segregation Principle
5. **D** - Dependency Inversion Principle

We will talk about each of them with exmaple.

**Note: I'm writing the examples in Java beacause everyone is familier with it. It can be easily converted into Swift.**

### 1. Single Responsibility Principle

*`A Class should have only one reason to change`*

Like we have a class for `Report` Management like following:

```java
class Report {
  public void setTitle(String title) {
    // Set the title
  }
  
  public void setContent(String content) {
    // Set content
  }
  
  public String getTitle() {
    // return the title
  }
  
  public String getContent() {
    // return content
  }
  
  public void printReport() {
    // Print report
  }
}
```

At first it looks good, but this class can be change for two reason. First, the contents of report can change and second the printing logic can change. So these are two different responsibilities one is business logic and second one is representation logic and they must be seperated according to SRP.

There are chances that if we change the business logic the representational logic will also change.

```java
class Report {
  public void setTitle(String title) {
    // Set the title
  }
  
  public void setContent(String content) {
    // Set content
  }
  
  public String getTitle() {
    // return the title
  }
  
  public String getContent() {
    // return content
  }
}

interface PrintableReport {
  public void printReport(Report report);
}

// Different representation logics

class TextPrint implements PrintableReport {
  public void printReport(Report report) {
    System.out.print(report.getTitle() + report.getContent());
  }
}

class HTMLPrint implements PrintableReport {
  public void printReport(Report report) {
    System.out.print("<html><head><title>" + report.getTitle() + "</title></head><body>" + report.getContent() + "</body></html>");
  }
}
```

Now the Business logic and Representational logic are in different class. It will minimize the coupling and it is bad design to couple two different logics, also we can add new Representational logic easily without modifying our business logic.

### 2. Open Close Principle

*`Classes should be open for extension, but closed for modification`*

This principle states that any new functionality can be added in a system with minimum modification in the current code. Means the system should be open to additions but it should also closed for modification (less modification).

A class is closed when it is compiled or shared as library but it is also open since any new class can use it as parent and add new functionality.

Let's assume we have an `AreaCalculator` class which calculates the sum of areas of different shape like this:

```java
class AreaCalculator { 
 public double area(Object[] shapes) {
  double area = 0;
  for (Object shape in shapes) {
    if (shape instanceof Square) {
      Square square = (Square)shape;
      area += Math.sqrt(square.height);
    }

    if (shape instanceof Triangle) {
      Triangle triangle = (Triangle)shape;
      double totalHalf = (triangle.firstSide + triangle.secondSide + triangle.thirdSide) / 2;
      area += Math.sqrt(totalHalf * (totalHalf - triangle.firstSide) * 
       (totalHalf - triangle.secondSide) * (totalHalf - triangle.thirdSide));
    }

    if (shape instanceof Circle) {
      Circle circle = (Circle)shape;
      area += circle.radius * circle.radius * Math.PI;
    }
  }
  return area;
 }
}

class Square {
  public double height;
}

class Circle {
  public double radius;
}

class Triangle {
  public double firstSide;
  public double secondSide;
  public double thirdSide;
}
```

So in above example what if we have to add one more Shape? we have to change the `AreaCalculator` class. So this class doesn't follow OCP.

Let's see the following code:

```java
class AreaCalculator {
  public double area(Shape[] shapes) {
    double area = 0;
    for (Shape shape in shapes) {
     area += shape.area();
    }
    return area;
  }
}

interface Shape {
  public double area();
}

class Square implements Shape {
 double height;
 
 public override double area() {
   return Math.sqrt(height);
 }
}

class Circle implements Shape {
  double radius;

  public override double area() {
   return radius * radius * Math.PI;
  }
}

class Triangle implements Shape { 
 double firstSide;
 double secondSide;
 double thirdSide;

 public override double area() {
   double totalHalf = (firstSide + secondSide + thirdSide) / 2;
   return Math.sqrt(totalHalf * (totalHalf - firstSide) * 
    (totalHalf - secondSide) * (totalHalf - thirdSide));
 }
}
```

Now, In this code if we have to add a new Shape we just need to create a class and implement the `Shape` interface. There is no modification needed in `AreaCalculator` class, so it is close to modification as well as open to extension.
