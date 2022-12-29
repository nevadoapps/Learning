Table of contents:
- What is Specification Pattern?
- Further reading
  - [Specification Design Pattern in C# - 01](https://codingsight.com/specification-design-pattern-in-c/)
  - [Specification Design Pattern in C# - 02](https://www.c-sharpcorner.com/article/the-specification-pattern-in-c-sharp/)


## What is Specification Pattern?

The specification pattern is a particular software design pattern, whereby business rules can be combined by chaining the business rules together using boolean logic. The pattern is frequently used in the context of domain-driven design.

The specification pattern is a design pattern that allows you to separate the definition of a complex query from its execution. It's often used in the context of data access and can be used to build flexible and reusable queries that can be combined to form more complex queries.

In C#, you can implement the specification pattern by creating an interface that defines the structure of a specification, and then implementing this interface for each specific specification that you want to define. The interface typically includes a method that takes an input object and returns a boolean value indicating whether the input object satisfies the specification or not.

Example - 01:
```csharp
public enum ColorEnum
{
    Black,
    Blue,
    White,
    Yellow,
    Red
}

public enum SizeEnum
{
    Small,
    Medium,
    Large
}

public enum ShapeTypeEnum
{
    Rectangle,
    Square,
    Circle,
    Triangle
}

public class Shape
{
    public string Name { get; set; }
    public ColorEnum Color { get; set; }
    public SizeEnum Size { get; set; }
    public ShapeTypeEnum ShapeType { get; set; }

    public override string ToString()
    {
        return $"Name {this.Name}, Color: {this.Color}, Size: {this.Size}, Shape Type: {this.ShapeType}";
    }
}

public interface ISpecification<T>
{
    bool IsStatisfied(T item);
}

public class ColorSpecification : ISpecification<Shape>
{
    private ColorEnum color;

    public ColorSpecification(ColorEnum color)
    {
        this.color = color; 
    }

    public bool IsStatisfied(Shape item)
    {
        if (item is null)
            return false;

        if (item.Color == this.color) 
            return true;

        return false;
    }
}

public class SizeSpecification: ISpecification<Shape>
{
    private SizeEnum size;

    public SizeSpecification(SizeEnum size)
    {
        this.size = size;
    }

    public bool IsStatisfied(Shape item)
    {
        if (item is null) return false;

        if (item.Size == this.size) return true;

        return false;
    }
}

public class ShapeTypeSpecification: ISpecification<Shape>
{
    private ShapeTypeEnum shapeType;

    public ShapeTypeSpecification(ShapeTypeEnum shapeType)
    {
        this.shapeType = shapeType;
    }

    public bool IsStatisfied(Shape item)
    {
        if (item is null) return false;

        if (item.ShapeType == this.shapeType) return true;

        return false;
    }
}

public class GroupSpecification<T>: ISpecification<T>
{
    private IList<ISpecification<T>> _specs;

    public GroupSpecification(List<ISpecification<T>> specs)
    {
        if (!specs.Any())
            throw new ArgumentNullException(nameof(specs));

        _specs = specs;
    }

    public bool IsStatisfied(T item)
    {
        foreach (var spec in _specs)
        {
            if (!spec.IsStatisfied(item)) return false;
        }

        return true;
    }
}

public interface IFilter<T>
{
    IEnumerable<T> Filter(IEnumerable<T> shapes, ISpecification<T> spec);
}

public class ShapeFilter: IFilter<Shape>
{
    public IEnumerable<Shape> Filter(IEnumerable<Shape> shapes, ISpecification<Shape> spec)
    {
        foreach (var shape in shapes)
        {
            if (spec.IsStatisfied(shape))
                yield return shape;
        }
    }
}

class Program
{
    static void Main(string[] args)
    {
        var shapes = new List<Shape>()
        {
            new Shape() { Color = ColorEnum.Black, Name = "HSY101", ShapeType = ShapeTypeEnum.Square, Size = SizeEnum.Small },
            new Shape() { Color = ColorEnum.Blue, Name = "HSY102", ShapeType = ShapeTypeEnum.Triangle, Size = SizeEnum.Medium },
            new Shape() { Color = ColorEnum.Yellow, Name = "HSY103", ShapeType = ShapeTypeEnum.Rectangle, Size = SizeEnum.Large },
            new Shape() { Color = ColorEnum.Blue, Name = "HSY104", ShapeType = ShapeTypeEnum.Circle, Size = SizeEnum.Medium },
            new Shape() { Color = ColorEnum.White, Name = "HSY105", ShapeType = ShapeTypeEnum.Square, Size = SizeEnum.Small },
            new Shape() { Color = ColorEnum.Black, Name = "HSY106", ShapeType = ShapeTypeEnum.Circle, Size = SizeEnum.Medium },
            new Shape() { Color = ColorEnum.Blue, Name = "HSY107", ShapeType = ShapeTypeEnum.Triangle, Size = SizeEnum.Large },
            new Shape() { Color = ColorEnum.White, Name = "HSY108", ShapeType = ShapeTypeEnum.Triangle, Size = SizeEnum.Small },
            new Shape() { Color = ColorEnum.Blue, Name = "HSY109", ShapeType = ShapeTypeEnum.Square, Size = SizeEnum.Medium },
            new Shape() { Color = ColorEnum.White, Name = "HSY110", ShapeType = ShapeTypeEnum.Triangle, Size = SizeEnum.Small },
            new Shape() { Color = ColorEnum.Red, Name = "HSY111", ShapeType = ShapeTypeEnum.Circle, Size = SizeEnum.Medium }
        };

        var sf = new ShapeFilter();

        foreach (var p in sf.Filter(shapes, new GroupSpecification<Shape>(new List<ISpecification<Shape>>()
                                                            {
                                                                new ColorSpecification(ColorEnum.White),
                                                                new SizeSpecification(SizeEnum.Small)
                                                            })))
        {
            Console.WriteLine($"Item is: {p.ToString()}");
        }

    }
}
```

Example - 02:
```csharp
public interface ISpecification<T>
{
    bool IsSatisfiedBy(T obj);
}

public class PropertyValueSpecification : ISpecification<MyObject>
{
    private readonly string _propertyName;
    private readonly object _expectedValue;

    public PropertyValueSpecification(string propertyName, object expectedValue)
    {
        _propertyName = propertyName;
        _expectedValue = expectedValue;
    }

    public bool IsSatisfiedBy(MyObject obj)
    {
        return obj.GetType().GetProperty(_propertyName).GetValue(obj) == _expectedValue;
    }
}

var specification = new PropertyValueSpecification("Color", "Red");
var redObjects = repository.Find(specification);
```
Example - 03:
```csharp
using System;
using System.Collections.Generic;
using static System.Console;

namespace DotNetDesignPatternDemos.SOLID.OCP
{
  public enum Color
  {
    Red, Green, Blue
  }

  public enum Size
  {
    Small, Medium, Large, Yuge
  }

  public class Product
  {
    public string Name;
    public Color Color;
    public Size Size;

    public Product(string name, Color color, Size size)
    {
      Name = name ?? throw new ArgumentNullException(paramName: nameof(name));
      Color = color;
      Size = size;
    }
  }

  public class ProductFilter
  {
    // let's suppose we don't want ad-hoc queries on products
    public IEnumerable<Product> FilterByColor(IEnumerable<Product> products, Color color)
    {
      foreach (var p in products)
        if (p.Color == color)
          yield return p;
    }
    
    public static IEnumerable<Product> FilterBySize(IEnumerable<Product> products, Size size)
    {
      foreach (var p in products)
        if (p.Size == size)
          yield return p;
    }

    public static IEnumerable<Product> FilterBySizeAndColor(IEnumerable<Product> products, Size size, Color color)
    {
      foreach (var p in products)
        if (p.Size == size && p.Color == color)
          yield return p;
    } // state space explosion
      // 3 criteria = 7 methods

    // OCP = open for extension but closed for modification
  }

  // we introduce two new interfaces that are open for extension

  public interface ISpecification<T>
  {
    bool IsSatisfied(Product p);
  }

  public interface IFilter<T>
  {
    IEnumerable<T> Filter(IEnumerable<T> items, ISpecification<T> spec);
  }

  public class ColorSpecification : ISpecification<Product>
  {
    private Color color;

    public ColorSpecification(Color color)
    {
      this.color = color;
    }

    public bool IsSatisfied(Product p)
    {
      return p.Color == color;
    }
  }

  public class SizeSpecification : ISpecification<Product>
  {
    private Size size;

    public SizeSpecification(Size size)
    {
      this.size = size;
    }

    public bool IsSatisfied(Product p)
    {
      return p.Size == size;
    }
  }

  // combinator
  public class AndSpecification<T> : ISpecification<T>
  {
    private ISpecification<T> first, second;

    public AndSpecification(ISpecification<T> first, ISpecification<T> second)
    {
      this.first = first ?? throw new ArgumentNullException(paramName: nameof(first));
      this.second = second ?? throw new ArgumentNullException(paramName: nameof(second));
    }

    public bool IsSatisfied(Product p)
    {
      return first.IsSatisfied(p) && second.IsSatisfied(p);
    }
  }

  public class BetterFilter : IFilter<Product>
  {
    public IEnumerable<Product> Filter(IEnumerable<Product> items, ISpecification<Product> spec)
    {
      foreach (var i in items)
        if (spec.IsSatisfied(i))
          yield return i;
    }
  }

  public class Demo
  {
    static void Main(string[] args)
    {
      var apple = new Product("Apple", Color.Green, Size.Small);
      var tree = new Product("Tree", Color.Green, Size.Large);
      var house = new Product("House", Color.Blue, Size.Large);

      Product[] products = {apple, tree, house};

      var pf = new ProductFilter();
      WriteLine("Green products (old):");
      foreach (var p in pf.FilterByColor(products, Color.Green))
        WriteLine($" - {p.Name} is green");

      // ^^ BEFORE

      // vv AFTER
      var bf = new BetterFilter();
      WriteLine("Green products (new):");
      foreach (var p in bf.Filter(products, new ColorSpecification(Color.Green)))
        WriteLine($" - {p.Name} is green");

      WriteLine("Large products");
      foreach (var p in bf.Filter(products, new SizeSpecification(Size.Large)))
        WriteLine($" - {p.Name} is large");

      WriteLine("Large blue items");
      foreach (var p in bf.Filter(products,
        new AndSpecification<Product>(new ColorSpecification(Color.Blue), new SizeSpecification(Size.Large)))
      )
      {
        WriteLine($" - {p.Name} is big and blue");
      }
    }
  }
}

```