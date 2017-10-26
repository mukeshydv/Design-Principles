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
  @override
  public void printReport(Report report) {
    System.out.print(report.getTitle() + report.getContent());
  }
}

class HTMLPrint implements PrintableReport {
  @override
  public void printReport(Report report) {
    System.out.print("<html><head><title>" + report.getTitle() + "</title></head><body>" + report.getContent() + "</body></html>");
  }
}
```
This principle also helps to identify the classes of the system and designing class diagrams such that change in any functionality only affects the class diagram of classes responsible for that functionality (Reducing the rework).


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
 
 @override
 public double area() {
   return Math.sqrt(height);
 }
}

class Circle implements Shape {
  double radius;

  @override
  public double area() {
   return radius * radius * Math.PI;
  }
}

class Triangle implements Shape { 
 double firstSide;
 double secondSide;
 double thirdSide;

 @override
 public double area() {
   double totalHalf = (firstSide + secondSide + thirdSide) / 2;
   return Math.sqrt(totalHalf * (totalHalf - firstSide) * 
    (totalHalf - secondSide) * (totalHalf - thirdSide));
 }
}
```

Now, In this code if we have to add a new Shape we just need to create a class and implement the `Shape` interface. There is no modification needed in `AreaCalculator` class, so it is close to modification as well as open to extension.


### 3. Liskov's Substitution Principle

*`Let q(x) be a property provable about objects of x of type T. Then q(y) should be provable for objects y of type S where S is a subtype of T.`*

All it is saying that, Any method which is using a reference of base class can use any child class object without knowing it.

or

In other words, A subclass should override the methods of parent class in such way that does not break the functionality from a client point of view.

or

If a method is using the Base class then the reference to the Base class can be replaced with a Derived class without affecting the functionality of the method.


A classic problem of Likov's Substitution Principle is Rectangle and Square, as we know a Square `is-a` Rectangle so we will try to establish an `IS-A` relationship using inheritance like below:

```java
public class Rectangle {
    private int length;
    private int breadth;
    
    public int getLength() {
        return length;
    }
    
    public void setLength(int length) {
        this.length = length;
    }
    
    public int getBreadth() {
        return breadth;
    }
    
    public void setBreadth(int breadth) {
        this.breadth = breadth;
    }
    
    public int getArea() {
        return this.length * this.breadth;
    }
}

// Here we're trying to have is-a relationship.
public class Square extends Rectangle {

    // Breadth and Height is same in Square
    
    @Override
    public void setBreadth(int breadth) {
        super.setBreadth(breadth);
        super.setLength(breadth);
    }
    
    @Override
    public void setLength(int length) {
        super.setLength(length);
        super.setBreadth(length);
    }
}
```

Now let's assume we have a client method `calculateArea` which takes `Rectangle` as parameter and have test cases based on `Rectangle` class like following.

```java
public void calculateArea(Rectangle r) {
        r.setBreadth(2);
        r.setLength(3);
        
        // From the code, the expected behavior is that 
        // the area of the rectangle is equal to 6.
        
        assert r.getArea() == 6 : "Error in calculating area";
}
```

What will happen if we pass a `Square` object to this method? The test case will fail as it will give us an output 9 for `Square`. So here the Likov's Substitution Principle violates.

So here cannot be an `is-a` relationship between `Square` and `Rectangle`.
What is the problem with `is-a` relationship here?
1. `Square` doesn't need Height and Breadth, as the sides of Square are equal so it is a wastage of memory.
2. To work appropriately the client (In this case `calculateArea` method) need to know the details of derive class `Square`, for this we have to change the client code which breaks the `Open Close Principle`.

So this principle make sure that new Derive classes are extending the Base class without changing their behaviour. 


### 4. Interface Segregation Principle

*`A client should never be forced to implement an interface that it doesn’t use or clients shouldn’t be forced to depend on methods they do not use.`*

In OOD we provide abstraction to module using interfaces, so we create an interface for a module and an implementation class. But now if we have to create an another module which only have some of the functionality as of previous module then we are forced to implement the whole interface and add some dummy implementation or throw Exception. This is known as `fat interface`.

So the Interface Segregation Principle states that in place of using one fat interface we create multiple small interfaces for submodules.

Let's take an exmaple of a Smart printer which can print, fax and scan. So we have system like this:

```java
interface ISmartPrinter {
    void print();
    void fax();
    void scan();
}

class SmartPrinter implements ISmartPrinter {

    public void print() {
         // Printing code.
    }
    
    public void fax() {
         // Fax Code.
    }
    
    public void scan() {
         // Scanning code.
    }
}
```

Now suppose we also have a not so good printer which can only print, so we're forced to implement the whole interface like:

```java
class EconomicPrinter implements ISmartPrinter {
    public void print() {
        //Yes I can print.
    }
    
    public void fax() {
        throw new NotSupportedException();
    }
    
    public void scan() {
        throw new NotSupportedException();
    }
}
```

This is not good.

So what we can do is apply ISP and devide the fat interface `ISmartPrinter` into three smaller interfaces, `IPrinter`, `IFax` and `IScanner` like this:

```java
interface IPrinter {
    void print();
}

interface IFax {
    void fax();
}

interface IScanner {
    void scan();
}


class SmartPrinter implements IPrinter, IFax, IScanner {

    public void print() {
         // Printing code.
    }
    
    public void fax() {
         // Fax Code.
    }
    
    public void scan() {
         // Scanning code.
    }
}


class EconomicPrinter implements IPrinter {
    public void print() {
        //Yes I can print.
    }
}
```

Keep in mind that using ISP can result in many small interfaces in code, but it will reduce the complexity, increase testability and also improve code quailty. Smaller interfaces are easier to implement, improving flexibility and the possibility of reuse. But ISP can also be used based on experience, like identifying the areas which are more likely to have an extension in future.
