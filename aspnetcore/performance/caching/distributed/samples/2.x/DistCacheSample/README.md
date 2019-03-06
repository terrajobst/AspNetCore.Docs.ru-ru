# <a name="aspnet-core-distributed-cache-sample"></a>Пример распределенного кэша ASP.NET Core

В этом примере описывается использование распределенного кэша. В этом примере демонстрируется сценарий, описанный в [работа с распределенным кэшем в ASP.NET Core](https://docs.microsoft.com/aspnet/core/performance/caching/distributed) раздела.

В рабочей среде в примере приложения настроен для использования распределенного кэша SQL Server. Чтобы перенастроить приложение для использования распределенного кэша Redis, измените директиву препроцессора в верхней части *Startup.cs* файл, используемый Redis (`#define Redis // SQLServer`). Дополнительные сведения см. в разделе [директивы препроцессора в образце кода](https://docs.microsoft.com/aspnet/core/#preprocessor-directives-in-sample-code).
