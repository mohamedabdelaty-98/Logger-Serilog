# References

The guide paraphrases and connects concepts from the following primary documentation. Package APIs and compatibility evolve; check the documentation for the version used by each application before adopting code.

## Microsoft Learn

- [.NET logging overview](https://learn.microsoft.com/en-us/dotnet/core/extensions/logging/overview) — abstractions, categories, levels, templates, configuration.
- [Logging providers in .NET](https://learn.microsoft.com/en-us/dotnet/core/extensions/logging/providers) — providers, host defaults, provider registration.
- [Logging in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/logging/) — web app logging and providers.
- [`ILogger` API](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.logging.ilogger) — interface reference.
- [High-performance logging](https://learn.microsoft.com/en-us/dotnet/core/extensions/logging/high-performance-logging) — `LoggerMessage` and performance patterns.
- [Compile-time logging source generation](https://learn.microsoft.com/en-us/dotnet/core/extensions/logging/source-generation) — `LoggerMessageAttribute` usage.

## Serilog project documentation

- [Configuration Basics](https://github.com/serilog/serilog/wiki/Configuration-Basics)
- [Writing Log Events](https://github.com/serilog/serilog/wiki/Writing-Log-Events)
- [Structured Data](https://github.com/serilog/serilog/wiki/Structured-Data)
- [Provided Sinks](https://github.com/serilog/serilog/wiki/Provided-Sinks)
- [Enrichment](https://github.com/serilog/serilog/wiki/Enrichment)
- [Serilog.AspNetCore README](https://github.com/serilog/serilog-aspnetcore)
- [Serilog thread enricher](https://github.com/serilog/serilog-enrichers-thread)

## Microsoft and Serilog terminology crosswalk

| Microsoft Extensions.Logging | Serilog |
|---|---|
| `ILogger<T>` category | `SourceContext` property when bridged through Serilog |
| Provider | Integration adapter/provider |
| Provider destination | Sink |
| `LogLevel.Trace` | `LogEventLevel.Verbose` |
| `LogLevel.Critical` | `LogEventLevel.Fatal` |
| `BeginScope()` | Often represented through Serilog log context when integration is configured |
