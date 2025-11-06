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

Here's an image that illustrates the relationship between an interface, an abstract class, and a concrete class: 
