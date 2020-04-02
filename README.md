# Scheduler.Hangfire

Позволяет запускать фоновые задания.

  - Fire-and-forget jobs - выполняются только один раз, почти сразу после создания.
```csharp
BackgroundJob.Enqueue(() => Console.WriteLine("Simple!"));
```
  - Delayed jobs - Запланированные фоновые задания выполняются только через определенный промежуток времени.
```csharp
var jobId = BackgroundJob .Schedule (() => Console .WriteLine ( "Delayed!" ), TimeSpan .FromDays (7));
```

  - Recurring jobs - Следующий метод для выполнения любой повторяющейся задачи, используя выражения CRON .
```csharp
CronExpression expression = CronExpression.Parse("*/10 * * * * *", CronFormat.IncludeSeconds);
    recurringJobManager.AddOrUpdate(
    "Run every 10 second", 
    () => serviceProvider.GetService<IService>().CheckNotification(), 
    expression.ToString());
```
### CRON expression

    ┌───────────── second (optional)       0-59              * , - /                      
    │ ┌───────────── minute                0-59              * , - /                      
    │ │ ┌───────────── hour                0-23              * , - /                      
    │ │ │ ┌───────────── day of month      1-31              * , - / L W ?                
    │ │ │ │ ┌───────────── month           1-12 or JAN-DEC   * , - /                      
    │ │ │ │ │ ┌───────────── day of week   0-6  or SUN-SAT   * , - / # L ?                Both 0 and 7 means SUN
    │ │ │ │ │ │
    * * * * * *



  - Continuations - позволяет объединять несколько заданий.
```csharp
BackgroundJob.ContinueWith(jobId, () => Console.WriteLine("Продолжение!"));
```