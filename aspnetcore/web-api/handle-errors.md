---
title: Обработка ошибок в веб-API ASP.NET Core
author: pranavkm
description: Сведения об обработке ошибок с помощью веб-API ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: prkrishn
ms.custom: mvc
ms.date: 09/25/2019
uid: web-api/handle-errors
ms.openlocfilehash: 9c5dd2f89e7351f386d1f0633c831952dc58e568
ms.sourcegitcommit: 994da92edb0abf856b1655c18880028b15a28897
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2019
ms.locfileid: "71278735"
---
# <a name="handle-errors-in-aspnet-core-web-apis"></a>Обработка ошибок в веб-API ASP.NET Core

В этой статье описывается обработка и настройка обработки ошибок с помощью веб-API ASP.NET Core.

[Просмотрите или скачайте пример кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/handle-errors/samples) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="developer-exception-page"></a>Страница со сведениями об исключении для разработчика

Страница [со сведениями об исключении для разработчика](xref:fundamentals/error-handling) — это полезное средство, с помощью которого можно получить подробные трассировки стека для ошибок сервера.

На странице со сведениями об исключении для разработчика ответ отображается в виде обычного текста, если клиент не принимает выходные данные в формате HTML. Например:

```
> curl https://localhost:5001/weatherforecast
System.ArgumentException: count
   at errorhandling.Controllers.WeatherForecastController.Get(Int32 x) in D:\work\Samples\samples\aspnetcore\mvc\errorhandling\Controllers\WeatherForecastController.cs:line 35
   at lambda_method(Closure , Object , Object[] )
   at Microsoft.Extensions.Internal.ObjectMethodExecutor.Execute(Object target, Object[] parameters)
...
```

> [!WARNING]
> Включать страницу исключений для разработчика следует **только тогда, когда приложение выполняется в среде разработки**. Подробные сведения об исключениях не должны быть общедоступными при выполнении приложения в рабочей среде. Дополнительные сведения о настройке среды см. в статье <xref:fundamentals/environments>.

## <a name="exception-handler"></a>Обработчик исключений

В средах, не относящихся к разработке, для получения полезных данных об ошибках можно использовать [ПО промежуточного слоя для обработки исключений](xref:fundamentals/error-handling).

1. В `Startup.Configure` вызовите <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>, чтобы использовать ПО промежуточного слоя:

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_UseExceptionHandler&highlight=9)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_UseExceptionHandler&highlight=9)]

    ::: moniker-end

1. Настройте действие контроллера для ответа на маршрут `/error`:

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Controllers/ErrorController.cs?name=snippet_ErrorController)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Controllers/ErrorController.cs?name=snippet_ErrorController)]

    ::: moniker-end

Предыдущее действие `Error` отправляет клиенту полезные данные, соответствующие [RFC7807](https://tools.ietf.org/html/rfc7807).

ПО промежуточного слоя для обработки исключений также может предоставлять более подробные данные, согласованные с содержимым, в локальной среде разработки. Чтобы создать согласованный формат полезных данных в среде разработки и рабочей среде, сделайте следующее:

1. В `Startup.Configure` зарегистрируйте экземпляры ПО промежуточного слоя обработки исключений для конкретной среды:

    ::: moniker range=">= aspnetcore-3.0"

    ```csharp
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseExceptionHandler("/error-local-development");
        }
        else
        {
            app.UseExceptionHandler("/error");
        }
    }
    ```

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseExceptionHandler("/error-local-development");
        }
        else
        {
            app.UseExceptionHandler("/error");
        }
    }
    ```

    ::: moniker-end

    В приведенном выше коде ПО промежуточного слоя регистрируется с помощью внедрения зависимостей.

    * Маршрут `/error-local-development` в среде разработки.
    * Маршрут `/error` в средах, не имеющих отношения к разработке.
    
1. Примените маршрутизацию атрибутов к действиям контроллера:

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Controllers/ErrorController.cs?name=snippet_ErrorControllerEnvironmentSpecific)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Controllers/ErrorController.cs?name=snippet_ErrorControllerEnvironmentSpecific)]

    ::: moniker-end

## <a name="use-exceptions-to-modify-the-response"></a>Изменение ответа с помощью исключений

Содержимое ответа можно изменить за пределами контроллера. В веб-API ASP.NET 4.x один из способов это сделать — использовать тип <xref:System.Web.Http.HttpResponseException>. ASP.NET Core не содержит эквивалентный тип. Поддержку `HttpResponseException` можно добавить, сделав следующее:

1. Создайте известный тип исключения с именем `HttpResponseException`:

    [!code-csharp[](handle-errors/samples/3.x/Exceptions/HttpResponseException.cs?name=snippet_HttpResponseException)]

1. Создайте фильтр действий с именем `HttpResponseExceptionFilter`:

    [!code-csharp[](handle-errors/samples/3.x/Filters/HttpResponseExceptionFilter.cs?name=snippet_HttpResponseExceptionFilter)]

1. В `Startup.ConfigureServices` добавьте фильтр действий в коллекцию фильтров:

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.2"
    
    [!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.1"

    [!code-csharp[](handle-errors/samples/2.x/2.1/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

## <a name="validation-failure-error-response"></a>Ответ в случае ошибки при сбое проверки

В контроллерах веб-API MVC отвечает с помощью типа ответа <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> при сбое проверки модели. MVC использует результаты <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> для создания ответа в случае ошибки при сбое проверки. В следующем примере в `Startup.ConfigureServices` для изменения типа ответа по умолчанию на <xref:Microsoft.AspNetCore.Mvc.SerializableError> используется фабрика:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=4-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=5-14)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](handle-errors/samples/2.x/2.1/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=6-15)]

::: moniker-end

## <a name="client-error-response"></a>Ответ при ошибке клиента

*Результат ошибки* определяется как результат с кодом состояния HTTP 400 или выше. В контроллерах веб-API платформа MVC преобразует результат ошибки в результат с <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.

::: moniker range=">= aspnetcore-3.0"

Ответ при ошибке можно настроить одним из следующих способов:

1. [Реализация ProblemDetailsFactory](#implement-problemdetailsfactory).
1. [Использование ApiBehaviorOptions.ClientErrorMapping](#use-apibehavioroptionsclienterrormapping).

### <a name="implement-problemdetailsfactory"></a>Реализация ProblemDetailsFactory

MVC создает все экземпляры <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> и <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> с помощью `Microsoft.AspNetCore.Mvc.ProblemDetailsFactory`. Сюда относятся ответы при ошибках клиента, ответы в случае ошибок при сбое проверки, а также вспомогательные методы `Microsoft.AspNetCore.Mvc.ControllerBase.Problem` и <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem>.

Чтобы настроить ответ с подробными сведениями о проблемах, зарегистрируйте пользовательскую реализацию `ProblemDetailsFactory` в `Startup.ConfigureServices`:

```csharp
public void ConfigureServices(IServiceCollection serviceCollection)
{
    services.AddControllers();
    services.AddTransient<ProblemDetailsFactory, CustomProblemDetailsFactory>();
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

Ответ при ошибке можно настроить, как описано в разделе [Использование ApiBehaviorOptions.ClientErrorMapping](#use-apibehavioroptionsclienterrormapping).

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="use-apibehavioroptionsclienterrormapping"></a>Использование ApiBehaviorOptions.ClientErrorMapping

Используйте свойство <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*>, чтобы настроить содержимое ответа `ProblemDetails`. Например, следующий код в `type` обновляет свойство `Startup.ConfigureServices` для ответов 404:

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8-9)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=9-10)]

::: moniker-end
