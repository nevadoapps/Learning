# Object Oriented Programming with C# - Part 05
Table of contents:
- Understanding Indexer Methods
- Understanding Operator Methods
- Understanding Custom Type Conversions
- Understanding Extension Methods
- Understanding Anonymous Types


## Understanding Indeer Methods

Indexers allow instance of a class or struct to be indexed just like arrays. The indexed value can be set or retrieved without explicitly specifying a type of instance member. Indexers resemble **properties** except that their accessors take parameters.

Example:
```csharp
    internal class Program
    {
        static void Main(string[] args) 
        {
            GenericCollection<Person06> collection = new();

            collection.Add(new Person06() { Id = 1, Name = "HSY01"});
            collection.Add(new Person06() { Id = 2, Name = "HSY02" });
            collection.Add(new Person06() { Id = 3, Name = "HSY03" });
            collection.Add(new Person06() { Id = 4, Name = "HSY05" });
            collection.Add(new Person06() { Id = 5, Name = "HSY05" });

            Console.WriteLine($"Collection Count: {collection.Count()}");

            foreach (var colItem in collection)
            {
                Console.WriteLine($"Id: {colItem.Id}, Name: {colItem.Name}");
            }

            var result = collection.Where(f => f.Name == "HSY05").FirstOrDefault();

            if (result is null)
                Console.WriteLine("Result is null which is expected");
            else
                Console.WriteLine($"Found - > Id: {result.Id}, Name: {result.Name}");

            var value1 = (Person06)collection.Get(0);
            if (collection.Get(0).Equals(value1))
                Console.WriteLine("(Over Get Method) - > Two objects are equal");
            else
                Console.WriteLine("(Over Get Method) - > Two objects are not equal which is expected result");

            var iValue1 = (Person06)collection[0];
            var iValue2 = (Person06)collection[0];
            
            if (iValue1.Equals(iValue2))
                Console.WriteLine("(Over Indexer Method) - > Two objects are equal");
            else
                Console.WriteLine("(Over Indexer Method) - > Two objects are not equal which is expected result");
        }
    }

    internal class Person06
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }

    internal class GenericCollection<T>: IEquatable<T>, IEnumerable<T> where T: class
    {
        private List<T> list;

        public GenericCollection() 
        { 
            list = new List<T>();
        }

        public T this[int index]
        {
            get { return (T)list[index]; }
            set { list[index] = value; }
        }

        public void Add(T item)
        {
            list.Add(item);
        }

        public void Remove(T item)
        {
            list.Remove(item);
        }

        public T Get(int index)
        {
            return list[index];
        }

        public IEnumerator<T> GetEnumerator()
        {
            return this.list.GetEnumerator();
        }

        IEnumerator IEnumerable.GetEnumerator()
        {
            return GetEnumerator();
        }

        public bool Equals(T? other)
        {
            if (other == null) return false;
            
            var properties = typeof(T).GetProperties();

            foreach ( var property in properties)
            {
                var value1 = property.GetValue(this);
                var value2 = property.GetValue(other);

                if (!Equals(value1, value2)) return false;
            }

            return true;
        }
    }
}
```