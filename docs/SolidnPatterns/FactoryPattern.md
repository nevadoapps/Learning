Table of contents:
- Factory Method Pattern
    - What is Factory Method Pattern?
- Abstract Factory Pattern
    - What is Abstract Factory Pattern?


<details>
<summary>

**Factory Method Pattern**
</summary>
<p>

- **What is Factory Method Pattern?**

The Abstract Factory pattern is a creational design pattern that provides an interface for creating families of related or dependent objects without specifying their concrete classes. 

Here is an example of the factory method pattern in C#:

```csharp
public abstract class Creator
{
    public abstract Product FactoryMethod();
}

public class ConcreteCreatorA : Creator
{
    public override Product FactoryMethod()
    {
        return new ConcreteProductA();
    }
}

public class ConcreteCreatorB : Creator
{
    public override Product FactoryMethod()
    {
        return new ConcreteProductB();
    }
}

public abstract class Product
{
    public abstract void DoSomething();
}

public class ConcreteProductA : Product
{
    public override void DoSomething()
    {
        Console.WriteLine("I'm product A");
    }
}

public class ConcreteProductB : Product
{
    public override void DoSomething()
    {
        Console.WriteLine("I'm product B");
    }
}
```

To use the factory method pattern, the client code would create an instance of one of the concrete creators (e.g. ConcreteCreatorA or ConcreteCreatorB) and then call the FactoryMethod on that creator to create a new instance of the product. The client can then use the returned product as if it were a concrete object, without knowing or caring about the specific type of object that was created.

```csharp
Creator creator = new ConcreteCreatorA();
Product product = creator.FactoryMethod();
product.DoSomething();
```

Example - 02:
```csharp
public class Point
{
    private double x, y;
    public double X { get => x; }
    public double Y { get => y; }

    private Point(double x, double y)
    {
        this.x = x;
        this.y = y;
    }

    //Factory Method
    public static Point NewCartesionPoint(double x, double y)
    {
        return new Point(x, y);
    }

    //Factory Method
    public static Point NewPolarPoint(double rho, double theta)
    {
        return new Point(rho * Math.Cos(theta), theta * Math.Sin(rho));
    }

    public override string ToString()
    {
        return $"{nameof(x)}: {x}, {nameof(y)}: {y}";
    }
}

public class Program
{
    static void Main(string[] args)
    {
        var pointC = Point.NewCartesionPoint(1.0, 1.0);
        var pointP = Point.NewPolarPoint(4, 5);
        WriteLine($"PointC: {pointC}");
        WriteLine($"PointP: {pointP}");
    }
}
```

**Async Factory Method**
```csharp
internal class AsyncFactory
{
    static async Task Main(string[] args)
    {
        var result = await Foo.CreateAsync();
    }
}

public class Foo
{
    private Foo() { }

    private async Task<Foo> InitAsync()
    {

        WriteLine("InitAsync");
        await Task.Delay(1000);
        return this;
    }

    public static async Task<Foo> CreateAsync()
    {
        WriteLine("CreateAsync");
        var result = new Foo();
        return await result.InitAsync();
    }
}
```
</p>
</details>

<details>
<summary>

**Abstract Factory Pattern**
</summary>
<p>

The Abstract Factory pattern is often used to create objects that belong to a particular theme or look and feel, without specifying the actual objects that will be created.

The factory method pattern is a creational design pattern that provides an interface for creating objects in a super class, but allows subclasses to alter the type of objects that will be created.

The factory method pattern is also known as the virtual constructor pattern and is used to create objects without specifying the exact class of object that will be created. Instead, a factory method is used to create the object, which is then returned to the client. The client can use the returned object as if it were a concrete object, without knowing or caring about the specific type of object that was created.

Here are some potential use cases for the Abstract Factory pattern in C#:

1. Creating a UI toolkit: You can use the Abstract Factory pattern to create a UI toolkit that can be used to build applications with a consistent look and feel. The factory interface would define methods for creating various UI elements (e.g., buttons, text boxes, etc.), and concrete factory classes would implement these methods to create the specific UI elements for each supported platform (e.g., Windows, Mac, etc.).

2. Building a game engine: The Abstract Factory pattern can be used to create game objects (e.g., characters, weapons, etc.) that belong to a particular theme or setting. For example, you could have a factory interface for creating medieval-themed game objects and concrete factory classes for creating knights, archers, and other medieval-themed objects.

3. Implementing a database access layer: You can use the Abstract Factory pattern to create a database access layer that can be used to connect to different database management systems. The factory interface would define methods for creating database connection objects, and concrete factory classes would implement these methods to create the specific connection objects for each supported database system (e.g., Oracle, MySQL, etc.).

4. Creating a document processing system: The Abstract Factory pattern can be used to create document processing objects (e.g., text formatting objects, image rendering objects, etc.) that are optimized for different file formats. For example, you could have a factory interface for creating document processing objects and concrete factory classes for creating objects that are optimized for processing Word documents, PDF files, and other file formats.

Example - 1:
```csharp
public interface IHotDrink
{
    void Consume();
}

internal class Tea : IHotDrink
{
    public void Consume()
    {
        Console.WriteLine($"This tea is nice but I'd prefer it with milk");
    }
}

internal class Coffee : IHotDrink
{
    public void Consume()
    {
        Console.WriteLine($"It is great to have a cup of coffee");
    }
}

public interface IHotDrinkFactory
{
    IHotDrink Prepare(int amount);
}

internal class TeaFactory : IHotDrinkFactory
{
    public IHotDrink Prepare(int amount)
    {
        Console.WriteLine($"Preparing Tea with the following amount: {amount}");
        return new Tea();
    }
}

internal class CoffeeFactory : IHotDrinkFactory
{
    public IHotDrink Prepare(int amount)
    {
        Console.WriteLine($"Preparing Coffee with the following amount: {amount}");
        return new Coffee();
    }
}

#region First Approach that operates over Enum Values in AvailableDrink
//Which violates Open-Closed Principle
//It is because you have to modify enum for AvailableDrink in the case of new additional drink is available that would likely be served by the machine.

public class HotDrinkMachineWithEnum
{
    public enum AvailableDrink
    {
        Coffee,
        Tea
    }

    private Dictionary<AvailableDrink, IHotDrinkFactory> factories = new Dictionary<AvailableDrink, IHotDrinkFactory>();

    public HotDrinkMachineWithEnum()
    {
        foreach (var evalue in Enum.GetValues<AvailableDrink>())
        {
            var factory = (IHotDrinkFactory)Activator.CreateInstance(
                Type.GetType("NevadoApps.Learning.DesignPatterns." + Enum.GetName(typeof(AvailableDrink), evalue) + "Factory"));

            factories.Add(evalue, factory);
        }
    }

    public IHotDrink MakeDrink(AvailableDrink drink, int amount)
    {
        //return factories[drink].Prepare(amount);

        if (factories.TryGetValue(drink, out var factory))
            return factory.Prepare(amount);
        else
            return null;
    }
}
#endregion

#region Better Approach that does not violates Open-Closed principle
public class HotDrinkMachineWithoutEnum
{
    private List<(string, IHotDrinkFactory)> factories = new List<(string, IHotDrinkFactory)>();

    public HotDrinkMachineWithoutEnum()
    {
        foreach (var t in typeof(HotDrinkMachineWithoutEnum).Assembly.GetTypes())
        {
            if (typeof(IHotDrinkFactory).IsAssignableFrom(t) && !t.IsInterface)
            {
                var currentItem = t.Name.Replace("Factory", string.Empty);
                if (!factories.Contains((currentItem, (IHotDrinkFactory) Type.GetType(t.Name))))
                {
                    factories.Add((currentItem, (IHotDrinkFactory)Activator.CreateInstance(t))); 
                }
            }
        }
    }

    public IHotDrink MakeDrink()
    {
        Console.WriteLine($"Available drinks that you can order:");

        for (int i = 0; i < factories.Count; i++)
        {
            Console.WriteLine($"Index -> {i}, {factories[i].Item1}");
        }

        while(true)
        {
            Console.WriteLine($"Please select an index for the drink");
            var drinkIndexBl = int.TryParse(Console.ReadLine(), out int selectionIndex);

            if ((drinkIndexBl && factories.Count> selectionIndex && selectionIndex>=0))
            {
                Console.WriteLine($"Please write an amount for the drink");
                var drinkAmountBl = int.TryParse(Console.ReadLine(), out int selectionAmount);

                if (drinkAmountBl && selectionAmount > 0)
                    return factories[selectionIndex].Item2.Prepare(selectionAmount);
            }
        }
    }
}
#endregion


internal class AbstractFactory
{
    static void MainAbstractFactory(string[] args)
    {
        var machine1 = new HotDrinkMachineWithEnum();
        var drink1 = machine1.MakeDrink(HotDrinkMachineWithEnum.AvailableDrink.Coffee, 100);
        drink1.Consume();

        var machine2 = new HotDrinkMachineWithoutEnum();
        var drink2 = machine2.MakeDrink();
        drink2.Consume();
    }
}
```

Example - 2:
```csharp
public interface IAbstractFactory
{
    IAbstractProductA CreateProductA();
    IAbstractProductB CreateProductB();
}

public interface IAbstractProductA
{
    string UsefulFunctionA();
}

public interface IAbstractProductB
{
    string UsefulFunctionB();
    string AnotherUsefulFunctionB(IAbstractProductA collaborator);
}

public class ConcreteProductA1 : IAbstractProductA
{
    public string UsefulFunctionA()
    {
        return "The result of the product A1.";
    }
}

public class ConcreteProductA2 : IAbstractProductA
{
    public string UsefulFunctionA()
    {
        return "The result of the product A2.";
    }
}

public class ConcreteProductB1 : IAbstractProductB
{
    public string UsefulFunctionB()
    {
        return "The result of the product B1.";
    }

    public string AnotherUsefulFunctionB(IAbstractProductA collaborator)
    {
        var result = collaborator.UsefulFunctionA();

        return $"The result of the B1 collaborating with the ({result})";
    }
}

public class ConcreteProductB2 : IAbstractProductB
{
    public string UsefulFunctionB()
    {
        return "The result of the product B2.";
    }

    public string AnotherUsefulFunctionB(IAbstractProductA collaborator)
    {
        var result = collaborator.UsefulFunctionA();

        return $"The result of the B2 collaborating with the ({result})";
    }
}

public class ConcreteFactory1 : IAbstractFactory
{
    public IAbstractProductA CreateProductA()
    {
        return new ConcreteProductA1();
    }

    public IAbstractProductB CreateProductB()
    {
        return new ConcreteProductB1();
    }
}

public class ConcreteFactory2 : IAbstractFactory
{
    public IAbstractProductA CreateProductA()
    {
        return new ConcreteProductA2();
    }

    public IAbstractProductB CreateProductB()
    {
        return new ConcreteProductB2();
    }
}

```

To use the Abstract Factory, you can do the following

```csharp
IAbstractFactory factory = new ConcreteFactory1();
IAbstractProductA productA = factory.CreateProductA();
IAbstractProductB productB = factory.CreateProductB();

string result = productB.AnotherUsefulFunctionB(productA);

Console.WriteLine(result);
```
</p>
</details>