# .NET Team Documentation

Welcome to the team's practical engineering reference for building and operating .NET applications.

## .NET logging and Serilog

The logging guide explains the complete path from application `ILogger<T>` calls through Microsoft logging providers and Serilog's structured event pipeline to sinks and production observability systems. It covers logging fundamentals, levels and filtering, configuration, structured events, enrichers, request correlation, ASP.NET Core integration, and team standards.

It is written for developers learning .NET logging and experienced engineers who need a consistent implementation reference. Examples use ASP.NET Core and realistic business and operational scenarios.

## Recommended learning path

1. [Logging fundamentals](logging/01-logging-fundamentals.md)
2. [Microsoft logging abstraction](logging/02-microsoft-logging-abstraction.md)
3. [Providers and levels](logging/03-logging-providers-and-levels.md)
4. [Serilog architecture](logging/04-serilog-architecture.md)
5. [Configuration and event design](logging/05-serilog-configuration.md), [writing events](logging/06-writing-log-events.md), and [structured logging](logging/07-structured-logging.md)
6. [Enrichment and context](logging/08-enrichers-and-context.md), [sinks](logging/09-serilog-sinks.md), and [ASP.NET Core setup](logging/10-aspnetcore-implementation.md)
7. [Production practices](logging/11-best-practices.md) and [team standards](logging/12-team-logging-standards.md)

## Logging chapter index

- [Full logging guide and quick reference](logging/README.md)
- [01. Logging fundamentals](logging/01-logging-fundamentals.md)
- [02. Microsoft logging abstraction](logging/02-microsoft-logging-abstraction.md)
- [03. Providers and levels](logging/03-logging-providers-and-levels.md)
- [04. Serilog architecture](logging/04-serilog-architecture.md)
- [05. Serilog configuration](logging/05-serilog-configuration.md)
- [06. Writing log events](logging/06-writing-log-events.md)
- [07. Structured logging](logging/07-structured-logging.md)
- [08. Enrichers and context](logging/08-enrichers-and-context.md)
- [09. Serilog sinks](logging/09-serilog-sinks.md)
- [10. ASP.NET Core implementation](logging/10-aspnetcore-implementation.md)
- [11. Production best practices](logging/11-best-practices.md)
- [12. Team logging standards](logging/12-team-logging-standards.md)
- [References](logging/references.md)
