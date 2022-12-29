Table of contents:
- What is SOLID principles?
- (**S**)ingle Responsibility Principle
- (**O**)pen-Closed Principle
- (**L**)iskov Substitution Principle
- (**I**)nterface Segregation Principle
- (**D**)ependency Inversion Principle 

## (**S**)ingle Responsibility Principle

The Single Responsibility Principle in C# states that “Each software module or class should have only one reason to change“. In other words, we can say that each module or class should have only one responsibility to do.

A module or function should be responsbile for a single purpose.

Example:

```csharp
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.IO;
using static System.Console;

namespace DotNetDesignPatternDemos.SOLID.SRP
{
  // just stores a couple of journal entries and ways of
  // working with them
  public class Journal
  {
    private readonly List<string> entries = new List<string>();

    private static int count = 0;

    public int AddEntry(string text)
    {
      entries.Add($"{++count}: {text}");
      return count; // memento pattern!
    }

    public void RemoveEntry(int index)
    {
      entries.RemoveAt(index);
    }

    public override string ToString()
    {
      return string.Join(Environment.NewLine, entries);
    }

    // breaks single responsibility principle
    public void Save(string filename, bool overwrite = false)
    {
      File.WriteAllText(filename, ToString());
    }

    public void Load(string filename)
    {
      
    }

    public void Load(Uri uri)
    {
      
    }
  }

  // handles the responsibility of persisting objects
  public class Persistence
  {
    public void SaveToFile(Journal journal, string filename, bool overwrite = false)
    {
      if (overwrite || !File.Exists(filename))
        File.WriteAllText(filename, journal.ToString());
    }
  }

  public class Demo
  {
    static void Main(string[] args)
    {
      var j = new Journal();
      j.AddEntry("I cried today.");
      j.AddEntry("I ate a bug.");
      WriteLine(j);

      var p = new Persistence();
      var filename = @"c:\temp\journal.txt";
      p.SaveToFile(j, filename);
      Process.Start(filename);
    }
  }
}

```

## (**O**)pen-Closed Principle

The Open-Closed Principle states that “software entities such as modules, classes, functions, etc. should be open for extension, but closed for modification“.

Let us understand the above definition in simple words. Here we need to understand two things. The first thing is Open for extension and the second thing is Closed for modification. The Open for extension means we need to design the software modules/classes in such a way that the new responsibilities or functionalities should be added easily when new requirements come. On the other hand, Closed for modification means, we should not modify the class/module until we find some bugs.

The reason for this is, we have already developed a class/module and it has gone through the unit testing phase. So we should not have to change this as it affects the existing functionalities. In simple words, we can say that we should develop one module/class in such a way that it should allow its behavior to be extended without altering its source code.

As per the Open-Closed principle, Instead of MODIFYING, we should go for **EXTENSION**.

Example:

```csharp
public class Invoice
{        
    public double GetInvoiceDiscount(double amount, InvoiceType invoiceType)
    {
        double finalAmount = 0;
        if (invoiceType == InvoiceType.FinalInvoice)
        {
            finalAmount = amount - 100;
        }
        else if (invoiceType == InvoiceType.ProposedInvoice)
        {
            finalAmount = amount - 50;
        }
        return finalAmount;
    }
}
public enum InvoiceType
{
    FinalInvoice,
    ProposedInvoice
};
```

The right approach that meets the Open-Closed principle is;

```csharp
public class Invoice
{
    public virtual double GetInvoiceDiscount(double amount)
    {
        return amount - 10;
    }
}

public class FinalInvoice : Invoice
{
    public override double GetInvoiceDiscount(double amount)
    {
        return base.GetInvoiceDiscount(amount) - 50;
    }
}
public class ProposedInvoice : Invoice
{
    public override double GetInvoiceDiscount(double amount)
    {
        return base.GetInvoiceDiscount(amount) - 40;
    }
}
public class RecurringInvoice : Invoice
{
    public override double GetInvoiceDiscount(double amount)
    {
        return base.GetInvoiceDiscount(amount) - 30;
    }
}
```

- Issue: The problem with the below example is that if we want to add another new invoice type, then we need to add one more "else if" condition in the same "GetInvoiceDiscount" method, in other words, we need to modify the Invoice class.

## (**L**)iskov Substitution Principle

Subtypes must be substitutable for their base type.

The behavior of a program needs to remain unchanged when you substitute one of its components for another component that has been defined by a supertype of the primer object.

## (**I**)nterface Segregation Principle

Creating huge interfaces will cause dependencies to occur while you are building concrete object, but these are harmful to the system architecture.

## (**D**)ependency Inversion Principle

The most flexible systems are the ones where object dependencies only refer to abstractions.