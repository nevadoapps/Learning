Table of contents:
- What is IConfiguration?
- What is IOptions\<T>, IOptionsSnapshot\<T>, IOptionsMonitor\<T>?
- How to bind & register configuration as service in AppSettings.json to custom class with different ways?
- [Further Reading](https://learn.microsoft.com/en-us/dotnet/core/extensions/options)

**What is IConfiguration?**

IConfiguration is an interface in the Microsoft.Extensions.Configuration namespace of the Microsoft.Extensions.Configuration.Abstractions package in ASP.NET Core. It represents a set of key-value application configuration properties.

You can use the IConfiguration interface to bind strongly typed objects to configuration data, such as JSON files or environment variables. You can also use it to access configuration data from various sources, such as JSON files, the command line, and environment variables.

To implement IConfiguration in your program, you will need to do the following:

1. Add the Microsoft.Extensions.Configuration.Abstractions package to your project.
2. In your Program class, add a field of type IConfiguration and a constructor that accepts an IConfiguration object.
3. In the Main method of your Program class, create an IConfiguration object using the ConfigurationBuilder class. You can use the AddJsonFile method to add a JSON configuration file as a source, and the Build method to build the configuration object.
4. Pass the IConfiguration object to the constructor of your Program class.

Here is an example of how you might implement IConfiguration in the Program class of a .NET Core console application:

```csharp
using Microsoft.Extensions.Configuration;

namespace MyApp
{
    public class Program
    {
        private readonly IConfiguration _config;

        public Program(IConfiguration config)
        {
            _config = config;
        }

        public static void Main(string[] args)
        {
            var config = new ConfigurationBuilder()
                .AddJsonFile("appsettings.json")
                .Build();

            var program = new Program(config);
            // ...
        }
    }
}
```
You can then use the _config field in your Program class to access configuration data. For example:
```csharp
string connectionString = _config.GetConnectionString("DefaultConnection");
```

**What is IOptions\<T>, IOptionsSnapshot\<T>, IOptionsMonitor\<T>?**

- **Options interfaces**
    - **IOptions\<TOptions>**:
        - Does not support:
            - Reading of configuration data after the app has started.
            - Named options
        - Is registered as a Singleton and can be injected into any service lifetime.
    - **IOptionsSnapshot\<TOptions>**:
        - Is useful in scenarios where options should be recomputed on every injection resolution, in scoped or transient lifetimes. For more information, see Use IOptionsSnapshot to read updated data.
        - Is registered as Scoped and therefore cannot be injected into a Singleton service. 
        - Supports named options
    - **IOptionsMonitor\<TOptions>**:
        - Is used to retrieve options and manage options notifications for **TOptions** instances.
        - Is registered as a Singleton and can be injected into any service lifetime.
        - Supports:
            - Change notifications
            - Named options
            - Reloadable configuration
            - Selective options invalidation (IOptionsMonitorCache\<TOptions>)
- **Options interfaces benefits**

Using a generic wrapper type gives you the ability to decouple the lifetime of the option from the DI container. The **IOptions\<TOptions>**.Value interface provides a layer of abstraction, including generic constraints, on your options type. This provides the following benefits:

- The evaluation of the T configuration instance is deferred to the accessing of **IOptions\<TOptions>**.Value, rather than when it is injected. This is important because you can consume the T option from various places and choose the lifetime semantics without changing anything about T.
- When registering options of type T, you do not need to explicitly register the T type. This is a convenience when you're authoring a library with simple defaults, and you don't want to force the caller to register options into the DI container with a specific lifetime.
- From the perspective of the API, it allows for constraints on the type T (in this case, T is constrained to a reference type).   

**Use IOptionsSnapshot to read updated data**

When you use **IOptionsSnapshot\<TOptions>**, options are computed once per request when accessed and are cached for the lifetime of the request. Changes to the configuration are read after the app starts when using configuration providers that support reading updated configuration values.

The difference between IOptionsMonitor and IOptionsSnapshot is that:
- IOptionsMonitor is a **singleton** service that retrieves current option values at any time, which is especially useful in singleton dependencies.
- IOptionsSnapshot is a scoped service and provides a snapshot of the options at the time the **IOptionsSnapshot\<T>** object is constructed. Options snapshots are designed for use with transient and scoped dependencies.

Example:
```csharp
using Microsoft.Extensions.Options;

namespace ConsoleJson.Example;

public sealed class ScopedService
{
    private readonly TransientFaultHandlingOptions _options;

    public ScopedService(IOptionsSnapshot<TransientFaultHandlingOptions> options) =>
        _options = options.Value;

    public void DisplayValues()
    {
        Console.WriteLine($"TransientFaultHandlingOptions.Enabled={_options.Enabled}");
        Console.WriteLine($"TransientFaultHandlingOptions.AutoRetryDelay={_options.AutoRetryDelay}");
    }
}
```

**How to bind & register configuration as service from AppSettings.json to custom class with different ways?**

- **First Way**