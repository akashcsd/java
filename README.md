https://github.com/jrandj/java-11?tab=readme-ov-file#Java-SE-11-Programmer-I
### **Summary of Key Concepts for Java Interfaces**

**1. Defining an Interface:**
*   An interface is an abstract data type that declares a list of abstract methods and constant variables.
*   Declared using the `interface` keyword.
*   **Implicit Modifiers:** Interfaces have modifiers automatically added by the compiler.
    *   Interfaces are implicitly `abstract`.
    *   Interface variables are implicitly `public`, `static`, and `final`.
    *   Interface methods without a body are implicitly `abstract` and `public`.
*   Interfaces can include `default` and `static` methods (Java 8+), and `private` and `private static` methods (Java 9+), though these are outside the scope of the 1Z0-815 exam.
*   Interface variables (constants) must be initialized at declaration.

**2. Implementing an Interface:**
*   A class uses the `implements` keyword to declare that it will provide implementations for the abstract methods of an interface.
*   A class can implement multiple interfaces, separated by commas.
*   All abstract methods of the implemented interfaces must be overridden in the concrete class. The access modifier in the implementing class must be `public`.
*   **Covariant Return Types:** When overriding methods from an interface, the return type in the implementing class can be a subtype of the return type declared in the interface.

**3. Extending Interfaces:**
*   An interface can extend one or more other interfaces using the `extends` keyword.
*   Unlike classes, interfaces can extend multiple interfaces because they don't have constructors and aren't part of instance initialization.

**4. Conflicting Modifiers:**
*   Explicitly adding modifiers that conflict with implicit modifiers will result in a compile-time error.
    *   E.g., an interface cannot be `final` or `private`.
    *   Interface methods cannot be `private` or `protected`.
    *   Interface variables cannot be `private` or `protected`, and cannot be `abstract`.

**5. Differences between Interfaces and Abstract Classes:**
*   **Implicit Modifiers:** Only interfaces use implicit modifiers.
*   **Constructors:** Interfaces do not have constructors and are not part of instance initialization. Abstract classes can have constructors.
*   **Multiple Inheritance:** Classes can implement multiple interfaces (multiple inheritance of type), but can only extend one abstract class (single inheritance of state and behavior).
*   **Method Access:** An unimplemented method in an abstract class without an access modifier is `package-private`, whereas an unimplemented method in an interface without an access modifier is `public`.

**6. Polymorphism and Interfaces:**
*   An object can be referred to by an interface type.
*   **Casting:** An interface reference can be cast to any reference that inherits the interface, but this might lead to `ClassCastException` at runtime if the actual object type doesn't support the cast. The compiler will prevent casts to completely unrelated types (e.g., casting `Canine` to `String` if `String` doesn't implement `Canine`).
*   **`instanceof` Operator:**
    *   The compiler generally allows `instanceof` checks between a reference type and an interface, even if the reference type doesn't directly implement the interface, because a subclass might.
    *   An exception occurs if the left-side reference is a `final` class that does not implement the interface; in this case, the compiler will report an error.

**7. Duplicate Interface Method Declarations:**
*   If a class implements multiple interfaces with identical method signatures (name and parameters), a single overriding method in the class can satisfy both interface requirements.
*   If the method names are the same but parameters differ, it's considered method overloading, and the class must implement all overloaded versions.
*   If the method signatures are the same but return types are incompatible (not covariant), the class cannot compile.

### **Organized List of Interface Rules (for 1Z0-815 Exam)**

**Interface Definition Rules:**
1.  Interfaces cannot be instantiated.
2.  Top-level interfaces cannot be `protected` or `private`. They must be `public` or `package-private`.
3.  Interfaces are implicitly `abstract` and cannot be marked `final`.
4.  Interfaces may include zero or more abstract methods.
5.  An interface can `extend` any number of interfaces.
6.  An interface reference may be cast to any reference that inherits the interface (runtime exception possible).
7.  The compiler reports `instanceof` errors with an interface only if the left-side reference is a `final` class that does not implement the interface.
8.  (For 1Z0-816) Interface methods with a body must be `default`, `private`, `static`, or `private static`.

**Abstract Interface Method Rules:**
1.  Abstract methods can only be defined in abstract classes or interfaces.
2.  Abstract methods cannot be declared `private` or `final`.
3.  Abstract methods must not provide a method body in the type they are declared.
4.  Implementing an abstract method in a subclass follows method overriding rules (including covariant return types, exception declarations).
5.  Interface methods without a body are implicitly `abstract` and `public`.

**Interface Variable Rules:**
1.  Interface variables are implicitly `public`, `static`, and `final`.
2.  Interface variables must be initialized at declaration.

**Key Takeaway:** Think of an interface as a specialized abstract class that defines a contract (a set of rules) for implementing classes. It primarily focuses on defining behavior without implementation details, supports multiple inheritance of type, and relies heavily on implicit modifiers.
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### 1. Multiple Inheritance of Type (Behavior)

*   **Abstract Classes:** A class can only **extend one** abstract class. This is Java's way of enforcing single inheritance for class hierarchy. If a class `A` extends `B`, it cannot also extend `C`. This limits a class to inheriting implementations and state from only one parent.
*   **Interfaces:** A class can **implement multiple** interfaces. This allows a class to inherit *types* (i.e., contracts for behavior) from many different sources. For example, a `Penguin` class might implement `Swimmer`, `Walker`, and `Eater` interfaces, declaring that it can perform all those actions.

    *   **Use Case:** This is crucial for defining diverse capabilities for an object without being constrained by a single class hierarchy. A `Car` might implement `Driveable` and `Lockable`. A `Smartphone` might implement `Callable`, `Connectable`, and `Photographable`.

Here's a conceptual diagram illustrating how a single class can implement multiple interfaces: 

### 2. Defining Contracts Purely for Behavior

*   **Abstract Classes:** Can have both abstract methods (no implementation) and concrete methods (with implementation), as well as instance variables and constructors. They represent a "partially implemented" class hierarchy.
*   **Interfaces:** Historically, interfaces could *only* contain abstract methods and public static final variables. This means they are purely about defining a contract for behavior. A class implementing an interface *must* provide an implementation for all its abstract methods. While Java 8+ added default and static methods, and Java 9+ added private methods, the core purpose remains to define a set of behaviors.

    *   **Use Case:** When you want to ensure that a class can perform certain actions, regardless of its underlying implementation or what it extends. For example, the `Comparable` interface guarantees that an object can be compared to another of its kind, while `Serializable` indicates an object can be written to a byte stream.

### 3. Decoupling and Flexibility

*   **Abstract Classes:** Create a strong "is-a" relationship and coupling within a class hierarchy. If you change the abstract class, all subclasses might be affected.
*   **Interfaces:** Promote weaker coupling. A class "is-a" type of the interfaces it implements, but it doesn't necessarily share an implementation hierarchy. This makes your code more flexible and easier to refactor or extend in the future. You can swap out implementations easily as long as they adhere to the interface.

    *   **Use Case:** Dependency Injection, where you program against interfaces rather than concrete implementations. This allows for easier testing and changing of components.

### 4. No Constructors and No Instance State

*   **Abstract Classes:** Can have constructors and instance variables, meaning they can have and manage state.
*   **Interfaces:** Do not have constructors and cannot have instance variables (only `public static final` constants). They define behavior without dictating any internal state or how that state is managed.

    *   **Use Case:** When you need a contract that *only* specifies methods, without any assumptions about the internal data or construction process of the implementing classes. This makes interfaces very lightweight and versatile.

### 5. Backward Compatibility (Default Methods in Java 8+)

*   **Interfaces (with Default Methods):** The introduction of `default` methods in Java 8 was a significant enhancement. It allows you to add new methods to an existing interface without breaking all the classes that already implement it. These classes automatically inherit the default implementation but can override it if needed.
*   **Abstract Classes:** Adding a new abstract method to an abstract class would break all existing concrete subclasses, forcing them to implement the new method.

    *   **Use Case:** This feature was crucial for evolving the Java Collections Framework (e.g., adding `forEach` to `Iterable`) without requiring millions of existing Java classes to be updated.

### In essence:

*   **Use an Abstract Class when:**
    *   You want to provide a common base implementation for subclasses.
    *   You need to define common state (instance variables) that all subclasses will share.
    *   You want to create a strong "is-a" hierarchy where subclasses genuinely extend the base class's essence.
    *   You only need a single inheritance path.

*   **Use an Interface when:**
    *   You want to define a contract for behavior that multiple unrelated classes can adhere to.
    *   You need to enable a class to have multiple "types" or capabilities (multiple inheritance of behavior).
    *   You want to achieve high decoupling and flexibility in your design.
    *   You don't care about the internal state or implementation details of the classes that fulfill the contract.
    *   You need to add new methods to existing contracts without breaking backward compatibility.

Both are powerful tools in Java, and often, you'll see them used together, with a class extending an abstract class and implementing multiple interfaces.
