# Object Oriented Design Principles

A good design is and most important step of Software Development life cycle, and to have a good design we must follow some set of guidelines. According to Robert Martin in "Agile Software Development: Principles, Patterns, and Practices" there are 3 main reasons of bad design that should be avoided:

1. **Rigidity** - System is hard to change, beacause it affects many part of the system.
2. **Fragility** - Change to a part affects an unexpected part of the system.
3. **Immobility** - A module is hard to reuse in other System because it cannot be disentangled from the current System.

To solve these problems there are principles called **SOLID**. It consist of 5 principle:

1. **S** - Single Responsibility Principle
2. **O** - Open Close Principle
3. **L** - Liskov's Substitution Principle
4. **I** - Interface Segregation Principle
5. **D** - Dependency Inversion Principle

We will talk about each of them with exmaple.

**Note: I'm writing the examples in Java beacause everyone is familier with it. It can be easily converted into Swift.**

### 1. Single Responsibility Principle

*A Class should have only one reason to change*

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

At first it looks good, but this class can be change for two reason. First, the contents of report can change and second the printing login can change. So these are two different responsibilities one is business login and second one is representation login and they must be seperated according to SRP.

There are chances that if we change the business logic the representational login will also change.

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

Now the Business login and Representational logic are in different class. It will minimize the coupling and it is bad design to couple two different logics.
