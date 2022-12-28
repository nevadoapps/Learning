# Object Oriented Programming with C# - Part 06

Table of contents:
- What is LINQ (Language Integrated Query)?
- Introduction to LINQ Queries
- Basic LINQ Query Operations
- Standard Query Operators [Reading](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/standard-query-operators-overview)

    - Sorting Data
    - Set Operations
    - Filtering Data
    - Quantifier Operations
    - Projection Operations
    - Partioning Data
    - Join Operations
    - Grouping Data
    - Generation Operations
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

    Filtering refers to the operation of restricting the result set to contain only those elements that satisfy a specified condition. It is also known as selection.

    | Method names | Description | C# query expression syntax | More information | 
    | -- | -- | -- | -- |
    | OfType |Selects values, depending on their ability to be cast to a specified type. |Not applicable. |Enumerable.OfType Queryable.OfType |
    | Where | Selects values that are based on a predicate function. | where | Enumerable.Where Queryable.Where | 

    - **Quantifier Operations**

    Quantifier operations return a Boolean value that indicates whether some or all of the elements in a sequence satisfy a condition.

    | Method names | Description | C# query expression syntax | More information | 
    | -- | -- | -- | -- |
    | All | Determines whether all the elements in a sequence satisfy a condition.	| Not applicable. | Enumerable.All Queryable.All |
    | Any | Determines whether any elements in a sequence satisfy a condition. | Not applicable. | Enumerable.Any Queryable.Any |
    | Contains | Determines whether a sequence contains a specified element. | Not applicable. | Enumerable.Contains Queryable.Contains |

    - **Projection Operations**

    Projection refers to the operation of transforming an object into a new form that often consists only of those properties that will be subsequently used. By using projection, you can construct a new type that is built from each object. You can project a property and perform a mathematical function on it. You can also project the original object without changing it.

    | Method names | Description | C# query expression syntax | More information | 
    | -- | -- | -- | -- |
    | Select | Projects values that are based on a transform function. | select | Enumerable.Select Queryable.Select |
    | SelectMany | Projects sequences of values that are based on a transform function and then flattens them into one sequence. | Use multiple from clauses | Enumerable.SelectMany Queryable.SelectMany |
    | Zip | Produces a sequence of tuples with elements from 2-3 specified sequences. | Not applicable. | Enumerable.Zip Queryable.Zip |

    - **Partioning Data**

    Partitioning in LINQ refers to the operation of dividing an input sequence into two sections, without rearranging the elements, and then returning one of the sections.

    | Method names | Description | C# query expression syntax | More information | 
    | -- | -- | -- | -- |
    |Skip | Skips elements up to a specified position in a sequence. | Not applicable. | Enumerable.Skip Queryable.Skip | 
    | SkipWhile | Skips elements based on a predicate function until an element does not satisfy the condition.	| Not applicable. |Enumerable.SkipWhile Queryable.SkipWhile |
    | Take | Takes elements up to a specified position in a sequence. | Not applicable.	| Enumerable.Take Queryable.Take |
    | TakeWhile	| Takes elements based on a predicate function until an element does not satisfy the condition.	| Not applicable. | Enumerable.TakeWhile Queryable.TakeWhile |
    | Chunk	| Splits the elements of a sequence into chunks of a specified maximum size. | Not applicable. | Enumerable.Chunk Queryable.Chunk |

    Example:
    ```csharp
    int chunkNumber = 1;
    foreach (int[] chunk in Enumerable.Range(0, 8).Chunk(3))
    {
        Console.WriteLine($"Chunk {chunkNumber++}:");
        foreach (int item in chunk)
        {
            Console.WriteLine($"    {item}");
        }

        Console.WriteLine();
    }
    // This code produces the following output:
    // Chunk 1:
    //    0
    //    1
    //    2
    //
    //Chunk 2:
    //    3
    //    4
    //    5
    //
    //Chunk 3:
    //    6
    //    7
    ```

    - **Join Operations**

    A join of two data sources is the association of objects in one data source with objects that share a common attribute in another data source.

    Joining is an important operation in queries that target data sources whose relationships to each other cannot be followed directly. In object-oriented programming, this could mean a correlation between objects that is not modeled, such as the backwards direction of a one-way relationship. An example of a one-way relationship is a Customer class that has a property of type City, but the City class does not have a property that is a collection of Customer objects. If you have a list of City objects and you want to find all the customers in each city, you could use a join operation to find them.

    The join methods provided in the LINQ framework are Join and GroupJoin. These methods perform equijoins, or joins that match two data sources based on equality of their keys. (For comparison, Transact-SQL supports join operators other than 'equals', for example the 'less than' operator.) In relational database terms, Join implements an inner join, a type of join in which only those objects that have a match in the other data set are returned. The GroupJoin method has no direct equivalent in relational database terms, but it implements a superset of inner joins and left outer joins. A left outer join is a join that returns each element of the first (left) data source, even if it has no correlated elements in the other data source.

    | Method names | Description | C# query expression syntax | More information | 
    | -- | -- | -- | -- |
    | Join | Joins two sequences based on key selector functions and extracts pairs of values. |join … in … on … equals … | Enumerable.Join Queryable.Join |
    | GroupJoin | Joins two sequences based on key selector functions and groups the resulting matches for each element. | join … in … on … equals … into …	 | Enumerable.GroupJoin Queryable.GroupJoin |

    Example:
    ```csharp
    class Product
    {
        public string Name { get; set; }
        public int CategoryId { get; set; }
    }

    class Category
    {
        public int Id { get; set; }
        public string CategoryName { get; set; }
    }

    public static void Example()
    {
        List<Product> products = new List<Product>
        {
            new Product { Name = "Cola", CategoryId = 0 },
            new Product { Name = "Tea", CategoryId = 0 },
            new Product { Name = "Apple", CategoryId = 1 },
            new Product { Name = "Kiwi", CategoryId = 1 },
            new Product { Name = "Carrot", CategoryId = 2 },
        };

        List<Category> categories = new List<Category>
        {
            new Category { Id = 0, CategoryName = "Beverage" },
            new Category { Id = 1, CategoryName = "Fruit" },
            new Category { Id = 2, CategoryName = "Vegetable" }
        };

        // Join products and categories based on CategoryId
        var query = from product in products
                    join category in categories on product.CategoryId equals category.Id
                    select new { product.Name, category.CategoryName };

        foreach (var item in query)
        {
            Console.WriteLine($"{item.Name} - {item.CategoryName}");
        }

        // This code produces the following output:
        //
        // Cola - Beverage
        // Tea - Beverage
        // Apple - Fruit
        // Kiwi - Fruit
        // Carrot - Vegetable
    }    
    ```
    Example - 02:
    ```csharp
    class Product
    {
        public string Name { get; set; }
        public int CategoryId { get; set; }
    }

    class Category
    {
        public int Id { get; set; }
        public string CategoryName { get; set; }
    }

    public static void Example()
    {
        List<Product> products = new List<Product>
        {
            new Product { Name = "Cola", CategoryId = 0 },
            new Product { Name = "Tea", CategoryId = 0 },
            new Product { Name = "Apple", CategoryId = 1 },
            new Product { Name = "Kiwi", CategoryId = 1 },
            new Product { Name = "Carrot", CategoryId = 2 },
        };

        List<Category> categories = new List<Category>
        {
            new Category { Id = 0, CategoryName = "Beverage" },
            new Category { Id = 1, CategoryName = "Fruit" },
            new Category { Id = 2, CategoryName = "Vegetable" }
        };

        // Join categories and product based on CategoryId and grouping result
        var productGroups = from category in categories
                            join product in products on category.Id equals product.CategoryId into productGroup
                            select productGroup;

        foreach (IEnumerable<Product> productGroup in productGroups)
        {
            Console.WriteLine("Group");
            foreach (Product product in productGroup)
            {
                Console.WriteLine($"{product.Name,8}");
            }
        }

        // This code produces the following output:
        //
        // Group
        //     Cola
        //      Tea
        // Group
        //    Apple
        //     Kiwi
        // Group
        //   Carrot
    }
    ```

    - **Grouping Data**

    Grouping refers to the operation of putting data into groups so that the elements in each group share a common attribute.

    | Method names | Description | C# query expression syntax | More information | 
    | -- | -- | -- | -- |
    | GroupBy | Groups elements that share a common attribute. Each group is represented by an IGrouping\<TKey,TElement> object. | group … by -or- group … by … into …	| Enumerable.GroupBy Queryable.GroupBy |
    | ToLookup | Inserts elements into a Lookup\<TKey,TElement> (a one-to-many dictionary) based on a key selector function. | Not applicable. | Enumerable.ToLookup |

    Example:
    ```csharp
    List<int> numbers = new List<int>() { 35, 44, 200, 84, 3987, 4, 199, 329, 446, 208 };  
    
    IEnumerable<IGrouping<int, int>> query = from number in numbers  
                                            group number by number % 2;  
    
    foreach (var group in query)  
    {  
        Console.WriteLine(group.Key == 0 ? "\nEven numbers:" : "\nOdd numbers:");  
        foreach (int i in group)  
            Console.WriteLine(i);  
    }  
    
    /* This code produces the following output:  
    
        Odd numbers:  
        35  
        3987  
        199  
        329  
    
        Even numbers:  
        44  
        200  
        84  
        4  
        446  
        208  
    */
    ```

    - **Generation Operations**

    Generation refers to creating a new sequence of values.

    The standard query operator methods that perform generation are listed in the following section.

    | Method names | Description | C# query expression syntax | More information | 
    | -- | -- | -- | -- |
    | DefaultIfEmpty | Replaces an empty collection with a default valued singleton collection.	| Not applicable. | Enumerable.DefaultIfEmpty Queryable.DefaultIfEmpty | 
    | Empty | Returns an empty collection. | Not applicable. | Enumerable.Empty |
    | Range | Generates a collection that contains a sequence of numbers. | Not applicable. | Enumerable.Range |
    | Repeat | Generates a collection that contains one repeated value.	| Not applicable. | Enumerable.Repeat |

    - **Element Operations**

    Element operations return a single, specific element from a sequence.

    The standard query operator methods that perform element operations are listed in the following section.  

    | Method names | Description | C# query expression syntax | More information | 
    | -- | -- | -- | -- |  
    | ElementAt | Returns the element at a specified index in a collection. | Not applicable. | Enumerable.ElementAt Queryable.ElementAt |
    | ElementAtOrDefault | Returns the element at a specified index in a collection or a default value if the index is out of range. | Not applicable. | Enumerable.ElementAtOrDefault Queryable.ElementAtOrDefault |
    | First | Returns the first element of a collection, or the first element that satisfies a condition. | Not applicable. |Enumerable.First Queryable.First |
    | FirstOrDefault | Returns the first element of a collection, or the first element that satisfies a condition. Returns a default value if no such element exists.	| Not applicable. | Enumerable.FirstOrDefault Queryable.FirstOrDefault Queryable.FirstOrDefault\<TSource>(IQueryable\<TSource>) |
    | Last |Returns the last element of a collection, or the last element that satisfies a condition. | Not applicable. | Enumerable.Last Queryable.Last | 
    | LastOrDefault | Returns the last element of a collection, or the last element that satisfies a condition. Returns a default value if no such element exists. | Not applicable. | Enumerable.LastOrDefault Queryable.LastOrDefault | 
    | Single | Returns the only element of a collection or the only element that satisfies a condition. Throws an InvalidOperationException if there is no element or more than one element to return. | Not applicable. | Enumerable.Single Queryable.Single | 
    | SingleOrDefault | Returns the only element of a collection or the only element that satisfies a condition. Returns a default value if there is no element to return. Throws an InvalidOperationException if there is more than one element to return.	| Not applicable.	 | Enumerable.SingleOrDefault Queryable.SingleOrDefault |

    - **Converting Data Types**

    Conversion methods change the type of input objects.

    Conversion operations in LINQ queries are useful in a variety of applications. Following are some examples:

    - The Enumerable.AsEnumerable method can be used to hide a type's custom implementation of a standard query operator.

    - The Enumerable.OfType method can be used to enable non-parameterized collections for LINQ querying.

    - The Enumerable.ToArray, Enumerable.ToDictionary, Enumerable.ToList, and Enumerable.ToLookup methods can be used to force immediate query execution instead of deferring it until the query is enumerated.

    | Method names | Description | C# query expression syntax | More information | 
    | -- | -- | -- | -- |  
    | AsEnumerable | Returns the input typed as IEnumerable\<T>. |Not applicable. | Enumerable.AsEnumerable | 
    | AsQueryable | Converts a (generic) IEnumerable to a (generic) IQueryable.	| Not applicable.	| Queryable.AsQueryable | 
    | Cast | Casts the elements of a collection to a specified type.	Use an explicitly typed range variable. For example: from string str in words | Enumerable.Cast Queryable.Cast |
    | OfType | Filters values, depending on their ability to be cast to a specified type. | Not applicable.	| Enumerable.OfType Queryable.OfType |
    | ToArray | Converts a collection to an array. | This method forces query execution. | Not applicable. | Enumerable.ToArray |
    | ToDictionary | Puts elements into a Dictionary\<TKey,TValue> based on a key selector function. This method forces query execution.	| Not applicable. | Enumerable.ToDictionary |
    | ToList | Converts a collection to a List\<T>. This method forces query execution.	| Not applicable. | Enumerable.ToList |
    | ToLookup | Puts elements into a Lookup\<TKey,TElement> (a one-to-many dictionary) based on a key selector function. This method forces query execution. | Not applicable.	| Enumerable.ToLookup |

    Example:
    ```csharp
    class Plant
    {
        public string Name { get; set; }
    }

    class CarnivorousPlant : Plant
    {
        public string TrapType { get; set; }
    }

    static void Cast()
    {
        Plant[] plants = new Plant[] {
            new CarnivorousPlant { Name = "Venus Fly Trap", TrapType = "Snap Trap" },
            new CarnivorousPlant { Name = "Pitcher Plant", TrapType = "Pitfall Trap" },
            new CarnivorousPlant { Name = "Sundew", TrapType = "Flypaper Trap" },
            new CarnivorousPlant { Name = "Waterwheel Plant", TrapType = "Snap Trap" }
        };

        var query = from CarnivorousPlant cPlant in plants
                    where cPlant.TrapType == "Snap Trap"
                    select cPlant;

        foreach (Plant plant in query)
            Console.WriteLine(plant.Name);

        /* This code produces the following output:

            Venus Fly Trap
            Waterwheel Plant
        */
    }
    ```

    - **Concatenation Operations**

    Concatenation refers to the operation of appending one sequence to another.

    The following illustration depicts a concatenation operation on two sequences of characters.    

    - **Aggregation Operations**

    An aggregation operation computes a single value from a collection of values. An example of an aggregation operation is calculating the average daily temperature from a month's worth of daily temperature values.

    | Method names | Description | C# query expression syntax | More information | 
    | -- | -- | -- | -- |  
    | Aggregate	| Performs a custom aggregation operation on the values of a collection. | Not applicable. | Enumerable.Aggregate Queryable.Aggregate |
    | Average | Calculates the average value of a collection of values.	|Not applicable. | Enumerable.Average Queryable.Average |
    | Count | Counts the elements in a collection, optionally only those elements that satisfy a predicate function. | Not applicable | Enumerable.Count Queryable.Count |
    | LongCount | Counts the elements in a large collection, optionally only those elements that satisfy a predicate function. | Not applicable. | Enumerable.LongCount Queryable.LongCount | 
    | Max or MaxBy | Determines the maximum value in a collection. | Not applicable. | Enumerable.Max Enumerable.MaxBy Queryable.Max Queryable.MaxBy |
    | Min or MinBy | Determines the minimum value in a collection. | Not applicable. | Enumerable.Min Enumerable.MinBy Queryable.Min Queryable.MinBy | 
    | Sum | Calculates the sum of the values in a collection. | Not applicable.	| Enumerable.Sum Queryable.Sum |
