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

The factory method pattern is a creational design pattern that provides an interface for creating objects in a super class, but allows subclasses to alter the type of objects that will be created.

The factory method pattern is also known as the virtual constructor pattern and is used to create objects without specifying the exact class of object that will be created. Instead, a factory method is used to create the object, which is then returned to the client. The client can use the returned object as if it were a concrete object, without knowing or caring about the specific type of object that was created.

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

</p>
</details>