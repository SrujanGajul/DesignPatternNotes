
# Anti-Pattern: Inheritance Misuse

## Scenario
We need to create a `Car` class that can use different types of engines, such as a `PetrolEngine`, `ElectricEngine`, and potentially more in the future.

## Using Inheritance (Anti-Pattern)

If we misuse inheritance to achieve this, we might end up with a structure like the following:

```csharp
// Base class for Car
public class Car {
    public void StartEngine() {
        // Default implementation (if any)
    }
}

// Derived class for PetrolCar
public class PetrolCar : Car {
    public override void StartEngine() {
        // Specific implementation for petrol engine
        Console.WriteLine("Starting petrol engine...");
    }
}

// Derived class for ElectricCar
public class ElectricCar : Car {
    public override void StartEngine() {
        // Specific implementation for electric engine
        Console.WriteLine("Starting electric engine...");
    }
}

// Potentially more derived classes for other types of engines...
```

### Problems with This Approach

- **Tight Coupling**:
    Each new type of engine requires a new derived class, tightly coupling the `Car` hierarchy to specific engine implementations.

- **Code Duplication**:
    Common functionalities across different car types may need to be duplicated or re-implemented in each derived class.

- **Rigid Structure**:
    The class hierarchy becomes rigid, making it difficult to add new engine types or change the behavior without modifying multiple classes.

- **Violation of Single Responsibility Principle**:
    Each derived class is responsible for both the general behavior of the car and the specific behavior of the engine, leading to a violation of the Single Responsibility Principle.

- **Scalability Issues**:
    As the number of engine types grows, the class hierarchy becomes more complex and harder to maintain.

## Composition (Preferred Solution)

To avoid these issues, we should use composition. Here’s how the same scenario can be handled using composition:

```csharp
public interface IEngine {
    void Start();
}

public class PetrolEngine : IEngine {
    public void Start() {
        Console.WriteLine("Starting petrol engine...");
    }
}

public class ElectricEngine : IEngine {
    public void Start() {
        Console.WriteLine("Starting electric engine...");
    }
}

public class Car {
    private IEngine engine;

    public Car(IEngine engine) {
        this.engine = engine;
    }

    public void StartEngine() {
        engine.Start();
    }
}
```

### Benefits of Composition

- **Loose Coupling**:
    The `Car` class is loosely coupled with the `IEngine` interface. Any changes to the engine implementations do not affect the `Car` class.

- **No Code Duplication**:
    Common car functionalities remain in the `Car` class. Only engine-specific behaviors are encapsulated in their respective classes.

- **Flexible Structure**:
    Adding new engine types (like `HybridEngine`) is straightforward and does not require changes to the `Car` class.

- **Adherence to Single Responsibility Principle**:
    The `Car` class handles car-specific behaviors, while engine-specific behaviors are handled by the respective engine classes.

- **Scalability**:
    The system can easily scale with new engine types without the need for an extensive class hierarchy.

## Summary

- **Inheritance (Anti-Pattern)**: Leads to tight coupling, code duplication, rigid structure, and scalability issues.
- **Composition (Preferred)**: Promotes loose coupling, reduces code duplication, maintains flexibility, adheres to the Single Responsibility Principle, and scales easily.

By using composition over inheritance in this scenario, we design a more maintainable, flexible, and scalable system.



---
---
---


> For the above thing if we see clearly, we are creating a new type of car for everytype of engine which is not a correct way. What if there are different types of cars? Lets say sedan, hatchback, SUV and soon... Now we have to create those types of subclasses for everytype of engine and type of car combination. This creates the Subclass explosion. For this we have a pattern which asks us to use the composition instead of the inheritance. The pattern is Bridge pattern.
>
> So we need to use inheritance only when there is "is-a" relationship between class but not when we need to reuse the code.
>
> If you really want to reuse the code use the compositon by delegating the code to the composed variable i.e., in the above case Engine classes.