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

