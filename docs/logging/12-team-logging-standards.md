# 12. Team Logging Standards

These standards are the recommended baseline for .NET application repositories. Apply stricter product, privacy, and regulatory controls where required.

## Required conventions

1. **Inject `Microsoft.Extensions.Logging.ILogger<T>`** into ordinary application classes. Use the class type as the category.
2. **Keep Serilog behind the Microsoft abstraction** for application code. Use `Serilog.ILogger` for bootstrap or Serilog-specific infrastructure where there is a clear need.
3. **Use structured message templates** with named properties. Do not use string interpolation or concatenation in logging calls.
4. **Use stable, descriptive property names**, such as `ExpenseId`, `PaymentId`, `Attempt`, `Provider`, and `ElapsedMs`.
5. **Pass exceptions as exception arguments**, not as interpolated message text.
6. **Choose levels by operational meaning**. Information for meaningful outcomes, Warning for handled degradation/retries, Error for failed operations, Critical for severe service-threatening conditions.
7. **Log at the layer with the best context** and avoid duplicate logs for one failure or business outcome.
8. **Never log secrets or unreviewed whole objects/request payloads.** Log only approved, necessary fields.
9. **Use TraceId/SpanId or explicit CorrelationId for logical correlation.** ThreadId is supplementary diagnostic metadata only.
10. **Configure providers, Serilog sinks, thresholds, and retention centrally** per application/environment. Do not create loggers per request.
11. **Set production framework category overrides** to control noise while retaining the categories needed for operations.
12. **Flush and dispose loggers cleanly** according to the integration and sink lifecycle.

## Preferred examples

```csharp
logger.LogInformation(
    "Expense {ExpenseId} approved by User {UserId}", expenseId, userId);

logger.LogWarning(
    "Payment {PaymentId} retry {Attempt} scheduled with provider {Provider}",
    paymentId, attempt, provider);

logger.LogError(exception,
    "Could not save Expense {ExpenseId}", expenseId);
```

## Avoid

```csharp
logger.LogInformation($"Approved expense {expenseId}"); // loses template properties
logger.LogError(exception.Message); // discards structured exception handling
logger.LogInformation("Request: {@Request}", request); // may expose excessive/sensitive data
```

## Review checklist

- Does the event answer a concrete diagnostic or operational question?
- Is the level appropriate, and is the event likely to be retained in production?
- Are changing values named properties with safe data types?
- Is the exception attached when relevant?
- Could any property contain a secret, personal data, or untrusted newline content?
- Will a trace/correlation or business identifier connect related events?
- Is this event duplicated at another layer or too frequent for its value?

## Lifetime note for logging wrappers

Direct `ILogger<T>` injection is the default. A wrapper is justified only when it enforces a meaningful application policy, such as a narrowly scoped audit API or standard domain fields. If introduced, choose its DI lifetime based on its own dependencies and state: a stateless wrapper may be singleton; a wrapper using request-scoped services/context must be scoped. Keep shared wrappers thread-safe. A wrapper that only renames forwarding methods adds indirection without useful behavior.
