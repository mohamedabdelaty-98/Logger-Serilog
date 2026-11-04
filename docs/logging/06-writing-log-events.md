# 6. Writing Log Events

## Event anatomy

A structured log event generally carries a timestamp, severity, source/category, message template, named property values, and optional exception. The rendered message is for people; the properties make events filterable and aggregatable by machines.

```csharp
logger.LogInformation(
    "Expense {ExpenseId} approved by User {UserId}", expenseId, userId);
```

Use stable, descriptive property names. Keep the template text stable across calls; place changing values in template arguments. Avoid string interpolation because it constructs a final string and loses the template's named data at the logging API boundary.

```csharp
// Avoid: values have already been merged into an unstructured string.
logger.LogInformation($"Expense {expenseId} approved by User {userId}");
```

## Levels in practice

- Trace: detailed internal state useful for targeted diagnosis.
- Debug: developer-facing details such as a cache miss decision.
- Information: successful business or operational milestones.
- Warning: recoverable issue, retry, or unexpected input handled safely.
- Error: failed operation, with exception when available.
- Critical: severe condition requiring urgent response.

Do not use Error for expected business outcomes such as an invalid user request unless the system itself failed. A rejected payment can be an Information event if it is a normal outcome; a payment gateway outage is a Warning while retrying and Error when the operation fails permanently.

## Exceptions

Pass exceptions through the exception parameter so the provider can preserve type, stack, and details. Do not interpolate `exception.Message` into the template as a substitute.

```csharp
try
{
    await paymentGateway.ProcessAsync(payment, cancellationToken);
}
catch (HttpRequestException exception)
{
    logger.LogError(exception,
        "Payment provider call failed for {PaymentId}", payment.Id);
    throw;
}
```

Log an exception at the layer that can add useful context and make the failure actionable. Avoid logging and rethrowing the same exception at every layer, which creates duplicate stack traces. Preserve the original exception when rethrowing (`throw;`).

## Event IDs and high-volume code

For recurring important events, stable `EventId` values can support categorization. For hot paths or libraries, Microsoft's `LoggerMessage` source generator provides compile-time logging methods with reduced allocations and diagnostics. Use it where performance or consistent event definitions justify the extra declaration; ordinary extension methods are fine for most application logging.

```csharp
internal static partial class ExpenseLog
{
    [LoggerMessage(EventId = 2101, Level = LogLevel.Information,
        Message = "Expense {ExpenseId} approved by User {UserId}")]
    internal static partial void ExpenseApproved(
        ILogger logger, Guid expenseId, Guid userId);
}
```

## Real event examples

```csharp
logger.LogInformation("Expense {ExpenseId} created", expenseId);
logger.LogWarning("Payment {PaymentId} retry {Attempt} scheduled", paymentId, attempt);
logger.LogError(exception, "Could not save expense {ExpenseId}", expenseId);
logger.LogInformation("Background job {JobName} completed in {ElapsedMs} ms",
    jobName, elapsedMilliseconds);
```

Serilog direct calls use `Log.Information("... {Property}", value)` and analogous methods. Through Microsoft's abstraction use `logger.LogInformation(...)`. Both support message templates; keep one consistent app-facing convention.

Reference: [Serilog writing log events](https://github.com/serilog/serilog/wiki/Writing-Log-Events).
