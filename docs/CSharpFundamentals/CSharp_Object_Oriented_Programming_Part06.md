# Object Oriented Programming with C# - Part 06

Table of contents:
- What is LINQ (Language Integrated Query)?
- Introduction to LINQ Queries
- Basic LINQ Query Operations
- Standard Query Operators
    - Sorting Data
    - Set Operations
    - Filtering Data
    - Quantifier Operations
    - Projection Operations
    - Partioning Operations
    - Join Operations
    - Grouping Data
    - Generation Operations
    - Equality Operatios
    - Element Operations
    - Converting Data Types
    - Concatenation Operations
    - Aggregation Operations


## What is LINQ (Language Integrated Query)?

Language Integrated Query (LINQ) is the name for a set of technologies based on the integration of query capabilities directly into to C# language.

With LINQ, a query is a first-class language construct, just like classes, methods, events. You write queries against strongly typed collections of objects by using language keywords and familiar operators.

The LINQ family of technologies provides a consistent query experience for objects (LINQ to Objects), relational databases (LINQ to SQL), and XML (LINQ to XML).

he most visible "language-integrated" part of LINQ is the query expression. Query expressions are written in a declarative query syntax. By using query syntax, you can perform filtering, ordering, and grouping operations on data sources with a minimum of code. You use the same basic query expression patterns to query and transform data in SQL databases, ADO.NET Datasets, XML documents and streams, and .NET collections.

**Notes:**

You can write LINQ queries in C# for SQL Server databases, XML documents, ADO.NET Datasets, and any collection of objects that supports **IEnumerable** or the generic **IEnumerable\<T>** interface. LINQ support is also provided by third parties for many Web services and other database implementations.

Example:
```csharp
static void Main(string[] args)
{
    //Example
    int[] scores = { 43, 54, 75, 23, 89, 90, 78 };

    IEnumerable<int> scoreQuery = from score in scores
                                    where score > 50
                                    select score;

    foreach(var score in scoreQuery)
    {
        Console.WriteLine($"Score - Example: {score}");
    }

    Console.WriteLine("******************************");

    //Converting to List - First Way

    List<int> list = (scoreQuery).ToList();

    foreach(var score2 in list)
    {
        Console.WriteLine($"Score - First Way: {score2}");
    }

    Console.WriteLine("******************************");

    //Converting to List - Second Way

    List<int> scoreQuery2 = (from score in scores
                                    where score > 50
                                    select score).ToList();

    foreach (var score in scoreQuery)
    {
        Console.WriteLine($"Score - Second Way: {score}");
    }
}
```

**Query Exression Overview**

- Query expressions can be used to query and to transform data from any LINQ-enabled data source. For example, a single query can retrieve data from a SQL database, and produce an XML stream as output.
- Query expressions are easy to grasp because they use many familiar C# language constructs.
- The variables in a query expression are all strongly typed, although in many cases you do not have to provide the type explicitly because the compiler can infer it. For more information, see [Type relationships in LINQ query operations](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/type-relationships-in-linq-query-operations).
- A query is not executed until you iterate over the query variable, for example, in a foreach statement. For more information, see [Introduction to LINQ queries](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/introduction-to-linq-queries).
- At compile time, query expressions are converted to Standard Query Operator method calls according to the rules set forth in the C# specification. Any query that can be expressed by using query syntax can also be expressed by using method syntax. However, in most cases query syntax is more readable and concise. For more information, see [C# language specification and Standard query operators overview](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/language-specification/expressions#1118-query-expressions).
- As a rule when you write LINQ queries, we recommend that you use query syntax whenever possible and method syntax whenever necessary. There is no semantic or performance difference between the two different forms. Query expressions are often more readable than equivalent expressions written in method syntax.
- Some query operations, such as Count or Max, have no equivalent query expression clause and must therefore be expressed as a method call. Method syntax can be combined with query syntax in various ways. For more information, see [Query syntax and method syntax in LINQ](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq).
 - Query expressions can be compiled to expression trees or to delegates, depending on the type that the query is applied to. **IEnumerable\<T>** queries are compiled to delegates. **IQueryable** and **IQueryable\<T>** queries are compiled to expression trees. For more information, see Expression trees.

 ## Introduction to LINQ Queries

 A query is an expression that retrieves data from a data source. Queries are usually expressed in a specialized query language. 

 All LINQ query operations consist of three distinct actions:
 1. Obtain the data source
 2. Create the query
 3. Execute the query

 **The Data Source**
 In the previous example, because the data source is an array, it implicitly supports the generic **IEnumerable\<T>** interface. This fact means it can be queried with LINQ. A query is executed in a foreach statement, and foreach requires **IEnumerable** or **IEnumerable\<T>**. Types that support IEnumerable<T> or a derived interface such as the generic **IQueryable\<T>** are called queryable types.

 A queryable type requires no modification or special treatment to serve as a LINQ data source. If the source data is not already in memory as a queryable type, the LINQ provider must represent it as such.

 Example:
 ```csharp
// Create a data source from an XML document.
// using System.Xml.Linq;
XElement contacts = XElement.Load(@"c:\myContactList.xml");

//or 

Northwnd db = new Northwnd(@"c:\northwnd.mdf");

// Query for customers in London.
IQueryable<Customer> custQuery =
    from cust in db.Customers
    where cust.City == "London"
    select cust;
 ```
**The Query**
The query specifies what information to retrieve from the data source or sources. Optionally, a query also specifies how that information should be sorted, grouped, and shaped before it is returned. A query is stored in a query variable and initialized with a query expression. To make it easier to write queries, C# has introduced new query syntax.

The query expression contains three clauses: **from**, **where** and **select**.
The from clause specifies the data source, the where clause applies the filter, and the select clause specifies the type of the returned elements. 

**Query Execution**

he query variable itself only stores the query commands. The actual execution of the query is deferred until you iterate over the query variable in a foreach statement. This concept is referred to as deferred execution.

The foreach statement is also where the query results are retrieved. 

**Forcing Immediate Execution**

Queries that perform aggregation functions over a range of source elements must first iterate over those elements. Examples of such queries are Count, Max, Average, and First. These execute without an explicit foreach statement because the query itself must use foreach in order to return a result. Note also that these types of queries return a single value, not an IEnumerable collection. The following query returns a count of the even numbers in the source array:

Example:
```csharp
var evenNumQuery =
    from num in numbers
    where (num % 2) == 0
    select num;

int evenNumCount = evenNumQuery.Count();
```

To force immediate execution of any query and cache its results, you can call the ToList or ToArray methods.

Example:
```csharp
List<int> numQuery2 =
    (from num in numbers
     where (num % 2) == 0
     select num).ToList();

// or like this:
// numQuery3 is still an int[]

var numQuery3 =
    (from num in numbers
     where (num % 2) == 0
     select num).ToArray();
```

**Basic LINQ Query Operations**

- **Obtaining a Data Source**

  In a LINQ Query, the first step is to specify the data source. In C# as in most programming languages a variable must be declared before it can be used. In a LINQ query, the **from** clause comes first in order to introduce the data source (customers) and the range variable (cust).

  ```csharp
  var queryAllCustomers = from cust in customers select cust;
  ```

  The range variable is like the iteration variable in a foreach loop except that no actual iteration occurs in a query expression. When the query is executed, the range variable will serve as a reference to each successive element in customers. Because the compiler can infer the type of cust, you do not have to specify it explicitly. Additional range variables can be introduced by a **let** clause. For more information, see let clause.

    - **let clause**

    In a query expression, it is sometimes useful to store the result of a sub-expression in order to use it in subsequent clauses. You can do this with the **let** keyword, which creates a new range variable and initializes it with the result of the expression you supply. Once initialized with a value, the range variable cannot be used to store another value. However, if the range variable holds a queryable type, it can be queried.

    Example:

    ```csharp
    class LetSample1
    {
        static void Main()
        {
            string[] strings =
            {
                "A penny saved is a penny earned.",
                "The early bird catches the worm.",
                "The pen is mightier than the sword."
            };

            // Split the sentence into an array of words
            // and select those whose first letter is a vowel.
            var earlyBirdQuery =
                from sentence in strings
                let words = sentence.Split(' ')
                from word in words
                let w = word.ToLower()
                where w[0] == 'a' || w[0] == 'e'
                    || w[0] == 'i' || w[0] == 'o'
                    || w[0] == 'u'
                select word;

            // Execute the query.
            foreach (var v in earlyBirdQuery)
            {
                Console.WriteLine("\"{0}\" starts with a vowel", v);
            }

            // Keep the console window open in debug mode.
            Console.WriteLine("Press any key to exit.");
            Console.ReadKey();
        }
    }
    /* Output:
        "A" starts with a vowel
        "is" starts with a vowel
        "a" starts with a vowel
        "earned." starts with a vowel
        "early" starts with a vowel
        "is" starts with a vowel
    */
    ```

- **Filtering**

Probably the most common query operation is to apply a filter in the form of a Boolean expression. The filter causes the query to return only those elements for which the expression is true. The result is produced by using the **where** clause. The filter in effect specifies which elements to exclude from the source sequence. In the following example, only those customers who have an address in London are returned.

Example:
```csharp
var queryLondonCustomers = from cust in customers
                           where cust.City == "London"
                           select cust;
```

- **Ordering**

Often it is convenient to sort the returned data. The **orderby** clause will cause the elements in the returned sequence to be sorted according to the default comparer for the type being sorted. For example, the following query can be extended to sort the results based on the Name property. Because **Name** is a string, the default comparer performs an alphabetical sort from A to Z.

```csharp
var queryLondonCustomers3 =
    from cust in customers
    where cust.City == "London"
    orderby cust.Name ascending
    select cust;
```
To order the results in reverse order, from Z to A, use the **orderby…descending** clause.

```csharp
class OrderbySample2
{
    // The element type of the data source.
    public class Student
    {
        public string First { get; set; }
        public string Last { get; set; }
        public int ID { get; set; }
    }

    public static List<Student> GetStudents()
    {
        // Use a collection initializer to create the data source. Note that each element
        //  in the list contains an inner sequence of scores.
        List<Student> students = new List<Student>
        {
           new Student {First="Svetlana", Last="Omelchenko", ID=111},
           new Student {First="Claire", Last="O'Donnell", ID=112},
           new Student {First="Sven", Last="Mortensen", ID=113},
           new Student {First="Cesar", Last="Garcia", ID=114},
           new Student {First="Debra", Last="Garcia", ID=115}
        };

        return students;
    }
    static void Main(string[] args)
    {
        // Create the data source.
        List<Student> students = GetStudents();

        // Create the query.
        IEnumerable<Student> sortedStudents =
            from student in students
            orderby student.Last ascending, student.First ascending
            select student;

        // Execute the query.
        Console.WriteLine("sortedStudents:");
        foreach (Student student in sortedStudents)
            Console.WriteLine(student.Last + " " + student.First);

        // Now create groups and sort the groups. The query first sorts the names
        // of all students so that they will be in alphabetical order after they are
        // grouped. The second orderby sorts the group keys in alpha order.
        var sortedGroups =
            from student in students
            orderby student.Last, student.First
            group student by student.Last[0] into newGroup
            orderby newGroup.Key
            select newGroup;

        // Execute the query.
        Console.WriteLine(Environment.NewLine + "sortedGroups:");
        foreach (var studentGroup in sortedGroups)
        {
            Console.WriteLine(studentGroup.Key);
            foreach (var student in studentGroup)
            {
                Console.WriteLine("   {0}, {1}", student.Last, student.First);
            }
        }

        // Keep the console window open in debug mode
        Console.WriteLine("Press any key to exit.");
        Console.ReadKey();
    }
}
/* Output:
sortedStudents:
Garcia Cesar
Garcia Debra
Mortensen Sven
O'Donnell Claire
Omelchenko Svetlana

sortedGroups:
G
   Garcia, Cesar
   Garcia, Debra
M
   Mortensen, Sven
O
   O'Donnell, Claire
   Omelchenko, Svetlana
*/
```

**Grouping**

The **group** clause enables you to group your results based on a key that you specify. For example you could specify that the results should be grouped by the City so that all customers from London or Paris are in individual groups. In this case, cust.City is the key.

```csharp
// queryCustomersByCity is an IEnumerable<IGrouping<string, Customer>>
  var queryCustomersByCity =
      from cust in customers
      group cust by cust.City;

  // customerGroup is an IGrouping<string, Customer>
  foreach (var customerGroup in queryCustomersByCity)
  {
      Console.WriteLine(customerGroup.Key);
      foreach (Customer customer in customerGroup)
      {
          Console.WriteLine("    {0}", customer.Name);
      }
  }
```

When you end a query with a **group** clause, your results take the form of a list of lists. Each element in the list is an object that has a **Key** member and a list of elements that are grouped under that key. When you iterate over a query that produces a sequence of groups, you must use a nested **foreach** loop. The outer loop iterates over each group, and the inner loop iterates over each group's members.

If you must refer to the results of a group operation, you can use the **into** keyword to create an identifier that can be queried further. The following query returns only those groups that contain more than two customers:

```csharp
// custQuery is an IEnumerable<IGrouping<string, Customer>>
var custQuery =
    from cust in customers
    group cust by cust.City into custGroup
    where custGroup.Count() > 2
    orderby custGroup.Key
    select custGroup;
```

**Joining**

Join operations create associations between sequences that are not explicitly modeled in the data sources. For example you can perform a join to find all the customers and distributors who have the same location. In LINQ the **join** clause always works against object collections instead of database tables directly.

```csharp
var innerJoinQuery =
    from cust in customers
    join dist in distributors on cust.City equals dist.City
    select new { CustomerName = cust.Name, DistributorName = dist.Name };
```

In LINQ, you do not have to use **join** as often as you do in SQL, because foreign keys in LINQ are represented in the object model as properties that hold a collection of items. For example, a Customer object contains a collection of Order objects. Rather than performing a join, you access the orders by using dot notation:

```csharp
from order in Customer.Orders...
```

**Selecting**

The select clause produces the results of the query and specifies the "shape" or type of each returned element. For example, you can specify whether your results will consist of complete Customer objects, just one member, a subset of members, or some completely different result type based on a computation or new object creation. When the select clause produces something other than a copy of the source element, the operation is called a projection. The use of projections to transform data is a powerful capability of LINQ query expressions. 

- ## Standard Query Operators

    - **Sorting Data**

    A sorting operation orders the elements of a sequence based on one or more attributes. The first sort criterion performs a primary sort on the elements. By specifying a second sort criterion, you can sort the elements within each primary sort group.

    | Method Name | Description | C# Query Expression Syntax | More Information | 
    | -- | -- |-- |-- |
    | OrderBy	| Sorts values in ascending order.	| orderby | Enumerable.OrderBy Queryable.OrderBy |
    | OrderByDescending	| Sorts values in descending order.	| orderby … descending	| Enumerable.OrderByDescending Queryable.OrderByDescending |
    | ThenBy	| Performs a secondary sort in ascending order.	|orderby …, |Enumerable.ThenBy  Queryable.ThenBy | 
    ThenByDescending	| Performs a secondary sort in descending order.	| orderby …, … descending	| Enumerable.ThenByDescending Queryable.ThenByDescending | 
    | Reverse	| Reverses the order of the elements in a collection.	|Not applicable.	|Enumerable.Reverse Queryable.Reverse |

    Example:

    The following example demonstrates how to use the orderby clause in a LINQ query to sort the strings in an array by string length, in ascending order.

    ```csharp
    string[] words = { "the", "quick", "brown", "fox", "jumps" };  
    
    IEnumerable<string> query = from word in words  
                                orderby word.Length  
                                select word;  
    
    foreach (string str in query)  
        Console.WriteLine(str);  
    
    /* This code produces the following output:  
    
        the  
        fox  
        quick  
        brown  
        jumps  
    */

    string[] words = { "the", "quick", "brown", "fox", "jumps" };  
    
    IEnumerable<string> query = from word in words  
                                orderby word.Substring(0, 1) descending  
                                select word;  
    
    foreach (string str in query)  
        Console.WriteLine(str);  
    
    /* This code produces the following output:  
    
        the  
        quick  
        jumps  
        fox  
        brown  
    */
    ```

    - **Set Operations**

    Set operations in LINQ refer to query operations that produce a result set that is based on the presence or absence of equivalent elements within the same or separate collections (or sets).

    | Method names | Description | C# query expression syntax | More information | 
    | -- | -- | -- | -- |
    | Distinct or DistinctBy | Removes duplicate values from a collection.	|Not applicable. | Enumerable.Distinct Enumerable.DistinctBy Queryable.Distinct Queryable.DistinctBy |
    | Except or ExceptBy |Returns the set difference, which means the elements of one collection that do not appear in a second collection.	| Not applicable. |Enumerable.Except Enumerable.ExceptBy Queryable.Except Queryable.ExceptBy |
    | Intersect or IntersectBy | Returns the set intersection, which means elements that appear in each of two collections.	| Not applicable. | Enumerable.Intersect Enumerable.IntersectBy Queryable.Intersect Queryable.IntersectBy |
    | Union or UnionBy | Returns the set union, which means unique elements that appear in either of two collections. | Not applicable.	| Enumerable.Union Enumerable.UnionBy Queryable.Union Queryable.UnionBy |

    Example (Base):

    ```csharp
    namespace SolarSystem;

    record Planet(
        string Name,
        PlanetType Type,
        int OrderFromSun)
    {
        public static readonly Planet Mercury =
            new(nameof(Mercury), PlanetType.Rock, 1);

        public static readonly Planet Venus =
            new(nameof(Venus), PlanetType.Rock, 2);

        public static readonly Planet Earth =
            new(nameof(Earth), PlanetType.Rock, 3);

        public static readonly Planet Mars =
            new(nameof(Mars), PlanetType.Rock, 4);

        public static readonly Planet Jupiter =
            new(nameof(Jupiter), PlanetType.Gas, 5);

        public static readonly Planet Saturn =
            new(nameof(Saturn), PlanetType.Gas, 6);

        public static readonly Planet Uranus =
            new(nameof(Uranus), PlanetType.Liquid, 7);

        public static readonly Planet Neptune =
            new(nameof(Neptune), PlanetType.Liquid, 8);

        // Yes, I know... not technically a planet anymore
        public static readonly Planet Pluto =
            new(nameof(Pluto), PlanetType.Ice, 9);
    }

    enum PlanetType
    {
        Rock,
        Ice,
        Gas,
        Liquid
    };    

    ```

    - **Distinct** and **DistinctBy**

    The DistinctBy is an alternative approach to Distinct that takes a keySelector. The keySelector is used as the comparative discriminator of the source type. Consider the following planet array:

    Example: 
    ```csharp
    Planet[] planets =
    {
        Planet.Mercury,
        Planet.Venus,
        Planet.Earth,
        Planet.Mars,
        Planet.Jupiter,
        Planet.Saturn,
        Planet.Uranus,
        Planet.Neptune,
        Planet.Pluto
    };

    foreach (Planet planet in planets.DistinctBy(p => p.Type))
    {
        Console.WriteLine(planet);
    }

    // This code produces the following output:
    //     Planet { Name = Mercury, Type = Rock, OrderFromSun = 1 }
    //     Planet { Name = Jupiter, Type = Gas, OrderFromSun = 5 }
    //     Planet { Name = Uranus, Type = Liquid, OrderFromSun = 7 }
    //     Planet { Name = Pluto, Type = Ice, OrderFromSun = 9 }
    ```

    - **Except** and **ExceptBy**

    The following example depicts the behavior of Enumerable.Except. The returned sequence contains only the elements from the first input sequence that are not in the second input sequence.

    Example:
    ```csharp
    string[] planets1 = { "Mercury", "Venus", "Earth", "Jupiter" };
    string[] planets2 = { "Mercury", "Earth", "Mars", "Jupiter" };

    IEnumerable<string> query = from planet in planets1.Except(planets2)
                                select planet;

    foreach (var str in query)
    {
        Console.WriteLine(str);
    }

    /* This code produces the following output:
    *
    * Venus
    */
    ```

    - **Intersect** and **IntersectBy**

    The IntersectBy method is an alternative approach to Intersect that takes two sequences of possibly heterogenous types and a keySelector. The keySelector is used as the comparative discriminator of the second collection's type. Consider the following planet arrays:

    Example:
    ```csharp
    Planet[] firstFivePlanetsFromTheSun =
    {
        Planet.Mercury,
        Planet.Venus,
        Planet.Earth,
        Planet.Mars,
        Planet.Jupiter
    };

    Planet[] lastFivePlanetsFromTheSun =
    {
        Planet.Mars,
        Planet.Jupiter,
        Planet.Saturn,
        Planet.Uranus,
        Planet.Neptune
    };

    foreach (Planet planet in
        firstFivePlanetsFromTheSun.IntersectBy(
            lastFivePlanetsFromTheSun, planet => planet))
    {
        Console.WriteLine(planet);
    }

    // This code produces the following output:
    //     Planet { Name = Mars, Type = Rock, OrderFromSun = 4 }
    //     Planet { Name = Jupiter, Type = Gas, OrderFromSun = 5 }    
    ```

    - **Union** and **UnionBy**

    The UnionBy method is an alternative approach to Union that takes two sequences of the same type and a keySelector. The keySelector is used as the comparative discriminator of the source type. Consider the following planet arrays:

    Example:
    ```csharp
    Planet[] firstFivePlanetsFromTheSun =
    {
        Planet.Mercury,
        Planet.Venus,
        Planet.Earth,
        Planet.Mars,
        Planet.Jupiter
    };

    Planet[] lastFivePlanetsFromTheSun =
    {
        Planet.Mars,
        Planet.Jupiter,
        Planet.Saturn,
        Planet.Uranus,
        Planet.Neptune
    };

    foreach (Planet planet in
        firstFivePlanetsFromTheSun.UnionBy(
            lastFivePlanetsFromTheSun, planet => planet))
    {
        Console.WriteLine(planet);
    }

    // This code produces the following output:
    //     Planet { Name = Mercury, Type = Rock, OrderFromSun = 1 }
    //     Planet { Name = Venus, Type = Rock, OrderFromSun = 2 }
    //     Planet { Name = Earth, Type = Rock, OrderFromSun = 3 }
    //     Planet { Name = Mars, Type = Rock, OrderFromSun = 4 }
    //     Planet { Name = Jupiter, Type = Gas, OrderFromSun = 5 }
    //     Planet { Name = Saturn, Type = Gas, OrderFromSun = 6 }
    //     Planet { Name = Uranus, Type = Liquid, OrderFromSun = 7 }
    //     Planet { Name = Neptune, Type = Liquid, OrderFromSun = 8 }    
    ```

    - **Filtering Data**
    - **Quantifier Operations**
    - **Projection Operations**
    - **Partioning Operations**
    - **Join Operations**
    - **Grouping Data**
    - **Generation Operations**
    - **Equality Operatios**
    - **Element Operations**
    - **Converting Data Types**
    - **Concatenation Operations**
    - **Aggregation Operations**