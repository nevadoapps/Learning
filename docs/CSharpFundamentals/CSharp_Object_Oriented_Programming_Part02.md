# Object Oriented Programming with C# - Part 02
Table of contents: 
1. Exception Handling
2. Working with Interfaces
3. Understanding Object Lifetime

## 1. Exception Handling

Programming with structured exception handling involves the use of four interrelated entities:

- A class type that represents the details of the exception
- A member that throws an instance of the exception class to the caller under the correct circumstances
- A block of code on the caller's side that invokes the exception-prone number
- A block of code on the caller's side that will process (or catch) the exception, should it occur

The C# offers five keyword (try, catch, throw, finally, and when) that allow you throw and handle exception.

Example: (Simple Exception Handling)
```csharp
try 
{
    //Code block in which potentionally an exception needs to be handled.   
}
catch (Exception ex)
{
    //Catch block
}
finally 
{
    //Finally block in which will be executed whether an exception is caught or not.
}
```

- ## The System.Exception Base Class

All exceptions ultimately derive from the System.Exception base class, which in turn derives from System.Object.

- Core Members of the System.Exception Type

    | System.Exception Property | Meaning in Life |
    | -- | -- |
    | Data | This read-only property retrieves a collection of key-value pairs that provide additional, programmer-defined information about the exception. | 
    | InnerException | This read-only property can be used to obtain information about the previous exceptions that caused the current exception to occur. |
    | Message | This read-only property returns the textual description of a given error. |
    | Source | This property gets or sets the name of the assembly, or the project, that threw the current exception. | 
    | StackTrace | This read-only property contains a string that identifies the sequence of calls that triggered the exception. | 
    | TargetSite | This read-only property returns a MethodBase object, which describes numerous details about the method that threw the exception. |

- ## Catch Blocks

A **catch** block can specify the type of exception to catch. The type specification is called an **exception filter**. The exception type should be derived from Exception base class.

Multiple **catch** blocks with different exception classes can be chained together. The **catch** blocks are evaluated from top to botom in your code, but only one **catch** block is executed for each exception that is thrown.

- ## Finally Blocks

A finally block enables you to clean up actions that are performed in a **try** block.  If present, the **finally** block executes last, after the **try** block and any matched **catch** block. A **finally** block always runs, whether an exception is thrown or a **catch** block matching the exception type is found.

The **finally** block can be used to release resource such as file streams, database connections, and graphics handles without waiting for the garbage collector in the runtime to finalize the objects. ([Read - Using statement](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/using-statement))

Example:
```csharp
FileStream? file = null;
FileInfo fileinfo = new System.IO.FileInfo("./file.txt");
try
{
    file = fileinfo.OpenWrite();
    file.WriteByte(0xF);
}
finally
{
    // Check for null because OpenWrite might have failed.
    file?.Close();
}
```
- ## Catching multiple exceptions & Exception Filters
```csharp
int GetInt(int[] array, int index)
{
    try
    {
        return array[index];
    }
    catch (IndexOutOfRangeException e) when (index < 0) 
    {
        throw new ArgumentOutOfRangeException(
            "Parameter index cannot be negative.", e);
    }
    catch (IndexOutOfRangeException e)
    {
        throw new ArgumentOutOfRangeException(
            "Parameter index cannot be greater than the array size.", e);
    }
}
```

- ## System-Level Exceptions (System.SystemException)
Exceptions that are thrown by the .Net Core platform are called **system exceptions**.
These exceptions are generally regarded as nonrecoverable, fatal errors. Simply put, when an exception type derives from **System.SystemException**, you are able to determine that the .Net Core runtime is the entity that has thrown the exception, rather than the codebase of the executing application.

- ## Application-Level Exceptions (System.ApplicationException)
The only purpose of **System.ApplicationException** is to identify the source of the error. When you handle an exception deriving from **System.ApplicationException**, you can assume the exception was raised by the codebase of the executing application, rather than by the .Net Core base class libraries or .Net Core runtime engine.

## 2. Working with Interfaces



#3 3. Understanding Object Lifetime