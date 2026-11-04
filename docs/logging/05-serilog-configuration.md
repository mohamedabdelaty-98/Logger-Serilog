# 5. Serilog Configuration

## Configure in code

Code configuration makes pipeline behavior explicit and is useful for bootstrap logging or small applications. Install only the packages needed by the chosen integration and sinks. The core package is `Serilog`; ASP.NET Core integration uses `Serilog.AspNetCore`; console and file destinations use `Serilog.Sinks.Console` and `Serilog.Sinks.File`.

```powershell
dotnet add package Serilog.AspNetCore
dotnet add package Serilog.Sinks.Console
dotnet add package Serilog.Sinks.File
```

Package versions should be compatible with the application's target framework. For current `Serilog.AspNetCore` guidance, use the major version aligned to the targeted ASP.NET Core/.NET generation as its project documentation directs.

## ASP.NET Core host integration

The current integration README demonstrates `builder.Services.AddSerilog()`:

```csharp
using Serilog;

Log.Logger = new LoggerConfiguration()
    .WriteTo.Console()
    .CreateLogger();

try
{
    Log.Information("Starting web application");
    var builder = WebApplication.CreateBuilder(args);
    builder.Services.AddSerilog();

    var app = builder.Build();
    app.MapGet("/health", () => Results.Ok());
    app.Run();
}
catch (Exception exception)
{
    Log.Fatal(exception, "Application terminated unexpectedly");
    throw;
}
finally
{
    await Log.CloseAndFlushAsync();
}
```

This bootstrap logger can capture startup failures before the host and DI container are fully built. Configure `AddSerilog()` overloads according to the selected Serilog.AspNetCore version and whether configuration/services should be supplied. Do not blindly copy older `UseSerilog()` examples without checking their version and host style.

## JSON configuration

For configuration-driven pipelines, install `Serilog.Settings.Configuration`, then add sink packages separately. An example `appsettings.json`:

```json
{
  "Serilog": {
    "Using": [ "Serilog.Sinks.Console", "Serilog.Sinks.File" ],
    "MinimumLevel": {
      "Default": "Information",
      "Override": {
        "Microsoft": "Warning",
        "Microsoft.AspNetCore": "Warning"
      }
    },
    "Enrich": [ "FromLogContext" ],
    "WriteTo": [
      { "Name": "Console" },
      {
        "Name": "File",
        "Args": {
          "path": "logs/app-.log",
          "rollingInterval": "Day",
          "retainedFileCountLimit": 14
        }
      }
    ],
    "Properties": {
      "Application": "ExpenseApi"
    }
  }
}
```

The configuration package discovers extension methods from installed sink/enricher assemblies. `Using` can help assembly discovery, though recent configuration approaches may discover assemblies automatically; keep it when needed for the app's deployment and package setup. Configuration binding syntax is package/API-specific; validate against the selected packages' current documentation.

```csharp
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddSerilog((services, configuration) => configuration
    .ReadFrom.Configuration(builder.Configuration)
    .ReadFrom.Services(services)
    .Enrich.FromLogContext());
```

If the exact overload differs in the installed package generation, follow that version's Serilog.AspNetCore README. Avoid maintaining two competing sources of truth for the same minimum levels in Microsoft `Logging` and Serilog `Serilog` sections.

## Shutdown and ownership

Flush and dispose the logger at application shutdown, especially when sinks buffer or send asynchronously. The host integration can manage logger lifetime when configured through its supported extension; a manually owned static bootstrap logger should be closed in `finally`. Do not create a new logger per request.

References: [Serilog configuration basics](https://github.com/serilog/serilog/wiki/Configuration-Basics), [ASP.NET Core integration](https://github.com/serilog/serilog-aspnetcore).
