Table of Contents:

1. Proposed way of injection services by Microsoft
2. Injecting services by IServiceInstaller and Reclection

## Proposed way of injection services by Microsoft

- Add a folder which will consists of Extension Methods

- Use the following template for each services to be registered to IServices collection

**Template:**
```csharp
public static class Configuration
{
    public static IServiceCollection Add<ServiceExtension> (this IServiceCollection services, IConfiguration configuration)
    {
        //Register desired services by using services.Add...
        //Eg.: 
        services.AddMvc();

        return services
    }
}

//program.cs
builder.Services.Add<ServiceExtension>(configuration);
```

Example:
Let's register Authentication and Authorization to the services over the Extension Method

```csharp
public static class Configuration
{
    public static IServiceCollection AddAuthenticationnAuthorization(this IServiceCollection servives, IConfiguration configuration) {
        services.AddAuthentication();
        services.AddAuthorization();

        return services;
    }
}

//program.cs

builder.Service.AddAuthenticationnAuthorization(builder.Configuration);
```
## Injecting services by IServiceInstaller and Reclection

The second method will ease the way to register a bunch of services to IServiceCollection without writing an extension method for each of them.

- First, create an Interface to be used in each Extension Methods

```csharp
public interface IServiceInstaller
{
    void Install(IServiceCollection services, IConfiguration configuration);
}
```
- Create a services installer class and method for each extension that you have on your mind. For example, we would like to create an Installer which install Authentication and Authorization services for the application.

```csharp
public class AuthenticationAuthorizationServiceInstaller: IServiceInstaller
{
    public void Install(IServiceCollection services, IConfiguration configuration) {
        services.AddAuthentication();
        services.AddAuthorization();
    }
}
```

- Call InstallerClass for each services, instantiate them and call Install Method. To make this simple, Reclection would be the easiest way to do so. 

Write an extension method which will query matched interfaces over assembly and instanciate them over Activator.

```csharp
public static class Configuration 
{
    public static IServiceCollection InstallServices(this IServiceCollection services, IConfiguration configuration, params Assembly[] assemblies) 
    {
        IEnumerable<IServiceInstaller> serviceInstaller = assemblies.SelectMany(a => a.DefinedTypes)
            .Where(IsAssignableTo<IServiceInstaller>)
            .Select(Activator.CreateInstance)
            .Cast<IServiceInstaller>();

        foreach (var installer in serviceInstaller)
        {
            Console.WriteLine($"AddInstallServices - Installing Service: {installer.GetType().Name} / {DateTime.Now}");
            installer.Install(services, configuration);
        }

        return services;

        static bool IsAssignableTo<T>(TypeInfo typeinfo)
        {
            return typeof(T).IsAssignableFrom(typeInfo) &&
                !typeof.IsInterface &&
                !typeof.IsAbstract;
        }
    }
}

- Lastly, you should call the extension method that we have prev. implemented called InstallServices in Program.cs that

```csharp
builder.Services.AddInstallServices(builder.Configuration, new Assembly[] { typeof(IServiceInstaller).Assembly });
```