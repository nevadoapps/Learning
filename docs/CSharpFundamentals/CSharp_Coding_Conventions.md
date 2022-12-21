## C# Coding Conventions

Coding conventions serve the following purposes:

- They create a consistent look to the code, so that readers can focus on content, not layout.
- They enable readers to understand the code more quickly by making assumptions based on previous experience.
- They facilitate copying, changing, and maintaining the code.
- They demonstrate C# best practices.


## Naming Conventions
- **Pascal Case**

    Use pascal casing ("PascalCasing") when naming class, record, or struct.

    Example:

    ```csharp
    public class DataService
    {
    }

    public record PhysicalAddress(
    string Street,
    string City,
    string StateOrProvince,
    string ZipCode);

    public record PhysicalAddress(
    string Street,
    string City,
    string StateOrProvince,
    string ZipCode);
    ```

    - When naming interface, use Pascal Casing in addition to prefixing the name with an **I**.

    ```csharp
    public interface IWorkerQueue
    {
    }
    ```
    - When naming public members of types, such as fields, properties, events, methods, and local functions, use pascal casing.
    ```csharp
    public class ExampleEvents
    {
        // A public field, these should be used sparingly
        public bool IsValid;

        // An init-only property
        public IWorkerQueue WorkerQueue { get; init; }

        // An event
        public event Action EventProcessing;

        // Method
        public void StartEventProcessing()
        {
            // Local function
            static int CountQueueItems() => WorkerQueue.Count;
            // ...
        }
    }
    ```
- **Camel Case**    
    Use camel casing ("camelCasing") when naming private or internal fields, and prefix them with _.

    ```csharp
    public class DataService
    {
        private IWorkerQueue _workerQueue;
    }
    ```
    - When working with static fields that are private or internal, use the s_ prefix and for thread static use t_.

    ```csharp
    public class DataService
    {
        private static IWorkerQueue s_workerQueue;

        [ThreadStatic]
        private static TimeSpan t_timeSpan;
    }
    ```
    - When writing method parameters, use camel casing.
    ```csharp
    public T SomeMethod<T>(int someNumber, bool isValid)
    {
    }
    ```
## new Operator

- Use one of the concise forms of object instantiation, as shown in the following declarations. The second example shows syntax that is available starting in C# 9.

    ```csharp
    var instance1 = new ExampleClass(); 

    ExampleClass instance2 = new(); //C# 9.0

    ExampleClass instance2 = new ExampleClass();
    ```

- Use object initializers to simplify object creation, as shown in the following example.

    ```csharp
    var instance3 = new ExampleClass { Name = "Desktop", ID = 37414,
    Location = "Redmond", Age = 2.3 };
    ```

## LINQ Queries

- Use meaningful names for query variables. The following example uses seattleCustomers for customers who are located in Seattle.

    ```csharp
    var seattleCustomers = from customer in customers
                       where customer.City == "Seattle"
                       select customer.Name;
    ```
- Use aliases to make sure that property names of anonymous types are correctly capitalized, using Pascal casing. 

    ```csharp
    var localDistributors =
        from customer in customers
        join distributor in distributors on customer.City equals distributor.City
        select new { Customer = customer, Distributor = distributor };
    ```
- Rename properties when the property names in the result would be ambiguous. For example, if your query returns a customer name and a distributor ID, instead of leaving them as Name and ID in the result, rename them to clarify that Name is the name of a customer, and ID is the ID of a distributor.   
    ```csharp
    var localDistributors2 =
        from customer in customers
        join distributor in distributors on customer.City equals distributor.City
        select new { CustomerName = customer.Name, DistributorID = distributor.ID };
    ```
- Use multiple from clauses instead of a join clause to access inner collections. For example, a collection of Student objects might each contain a collection of test scores. When the following query is executed, it returns each score that is over 90, along with the last name of the student who received the score.
    ```csharp
    var scoreQuery = from student in students
                    from score in student.Scores!
                    where score > 90
                    select new { Last = student.LastName, score };    
    ```