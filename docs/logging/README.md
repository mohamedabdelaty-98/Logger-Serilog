# .NET Logging and Serilog Team Guide

## Introduction

This guide establishes a shared understanding of application logging in .NET and a practical approach to using Microsoft.Extensions.Logging with Serilog in ASP.NET Core. It follows the event from the application call through filtering, enrichment, and output.

## Purpose and audience

Use this as both a learning path for developers new to .NET logging and a reference for engineers designing production logging. Examples use ASP.NET Core Web APIs and realistic expense, payment, database, and background-job scenarios.

## Prerequisites

Readers should know basic C#, dependency injection, and ASP.NET Core's `Program.cs` and configuration system. The guide assumes modern .NET; match package major versions to the app's target framework and verify exact package compatibility when adopting it.

## Recommended learning order

1. [Logging fundamentals](01-logging-fundamentals.md)
2. [Microsoft logging abstraction](02-microsoft-logging-abstraction.md)
3. [Providers and levels](03-logging-providers-and-levels.md)
4. [Serilog architecture](04-serilog-architecture.md)
5. [Serilog configuration](05-serilog-configuration.md)
6. [Writing log events](06-writing-log-events.md)
7. [Structured logging](07-structured-logging.md)
8. [Enrichers and context](08-enrichers-and-context.md)
9. [Sinks](09-serilog-sinks.md)
10. [ASP.NET Core implementation](10-aspnetcore-implementation.md)
11. [Production best practices](11-best-practices.md)
12. [Team standards](12-team-logging-standards.md)
13. [References](references.md)

## Contents

| Chapter | Covers |
|---|---|
| [01 — Fundamentals](01-logging-fundamentals.md) | Purpose, diagnostics, auditing, tracing, production use |
| [02 — Microsoft abstraction](02-microsoft-logging-abstraction.md) | `ILogger`, `ILogger<T>`, `ILoggerFactory`, DI, categories |
| [03 — Providers and levels](03-logging-providers-and-levels.md) | Destinations, providers, filtering, level mapping |
| [04 — Serilog architecture](04-serilog-architecture.md) | Events, pipeline, integration, components |
| [05 — Configuration](05-serilog-configuration.md) | Code and JSON configuration, startup and shutdown |
| [06 — Writing events](06-writing-log-events.md) | Templates, exceptions, event IDs, examples |
| [07 — Structured logging](07-structured-logging.md) | Properties, destructuring, queryability |
| [08 — Enrichment and context](08-enrichers-and-context.md) | Common properties, scopes, LogContext, correlation |
| [09 — Sinks](09-serilog-sinks.md) | Console, files, remote destinations, sink filtering |
| [10 — ASP.NET Core](10-aspnetcore-implementation.md) | End-to-end setup and request logging |
| [11 — Best practices](11-best-practices.md) | Operational, security, performance practices |
| [12 — Team standards](12-team-logging-standards.md) | Enforceable coding conventions |

## Quick reference

```csharp
logger.LogTrace("Cache lookup for {CacheKey}", cacheKey);
logger.LogDebug("Loaded expense {ExpenseId}", expenseId);
logger.LogInformation("Expense {ExpenseId} approved by User {UserId}", expenseId, userId);
logger.LogWarning("Payment provider retry {Attempt} for {PaymentId}", attempt, paymentId);
logger.LogError(exception, "Payment {PaymentId} failed", paymentId);
logger.LogCritical(exception, "Payment processing subsystem is unavailable");
```

| Concept | Meaning |
|---|---|
| Microsoft logging | Application-facing abstractions (`ILogger`, providers, levels, scopes) in `Microsoft.Extensions.Logging` |
| Serilog | A logging implementation and event pipeline with its own `Serilog.ILogger`, enrichers, filters, and sinks |
| Provider | An adapter integrating a logging implementation with the Microsoft logging abstraction |
| Sink | A Serilog output destination, such as console, file, or a telemetry platform |
| Template property | Named data in a message template, for example `{ExpenseId}` |

**Team default:** Inject `ILogger<T>`; use named template properties; log exceptions as exception arguments; configure levels and destinations centrally; never log secrets or sensitive payloads. See [team standards](12-team-logging-standards.md).
