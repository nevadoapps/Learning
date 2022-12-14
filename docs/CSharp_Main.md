# Learning C# and Object Oriented Programming
Table of Contents:
1. The anatomy of a simple C# program

<details>
    <summary>##The anatomy of a simple C# program
</summary>
<p>
C# demands that all program logic be contained within a type definition. (type is a general term referring to a member of the set, class, interface, structure, enumeration, delegate).

Unlike many other languages, in C#, it is not possible to craete global functions or global points of data. Rather, all data members and all methods must be contained within a type definition.

<quote>**Note:** C# is a case-sensitive programming language. Therefore, Main is not the same as main, and Readline is not the same as Readline.

Be aware that all C# keywords are lowercase, e.g., public, lock, class, dynamic while namespaces, types and member names begin (by convention) with initial Capital latter and have capitalized the first letter of any embedded words, e.g., Console.WriteLine, System.Windows, MessageBox, System.Data.SqlClient.
</quote>

<quote>**Sample C# Method**

```csharp
class Program
{
    static void Main(string[] args)
    {
        //Comment
        Console.WriteLine($"This is my first line of code");
        Console.ReadLine();
    }
}
```
</p>