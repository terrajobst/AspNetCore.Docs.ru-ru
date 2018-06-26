---
title: Обработка ошибок в ASP.NET Core
author: ardalis
description: Узнайте, как обрабатывать ошибки в приложениях ASP.NET Core.
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 11/30/2016
uid: fundamentals/error-handling
ms.openlocfilehash: 2fe46ecc32d61a7fafb2ad6e2a35456476608251
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273713"
---
# <a name="handle-errors-in-aspnet-core"></a>Обработка ошибок в ASP.NET Core

Авторы: [Стив Смит](https://ardalis.com/) (Steve Smith) и [Том Дакстра](https://github.com/tdykstra/) (Tom Dykstra)

В этой статье рассматриваются основные методы обработки ошибок в приложениях ASP.NET Core.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="the-developer-exception-page"></a>Страница со сведениями об исключении для разработчика

Чтобы настроить вывод в приложении страницы с подробными сведениями об исключениях, установите пакет NuGet `Microsoft.AspNetCore.Diagnostics` и добавьте строку в [метод Configure в классе Startup](xref:fundamentals/startup):

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

Поместите метод `UseDeveloperExceptionPage` перед каждым промежуточным слоем, в котором нужно перехватывать исключения, например `app.UseMvc`.

>[!WARNING]
> Включать страницу со сведениями об исключении для разработчика следует **только тогда, когда приложение выполняется в среде разработки**. Подробные сведения об исключениях не должны быть общедоступными при выполнении приложения в рабочей среде. [Дополнительные сведения о настройке сред](xref:fundamentals/environments).

Чтобы увидеть страницу со сведениями об исключении для разработчика, запустите образец приложения, указав среду `Development`, и добавьте элемент `?throw=true` к базовому URL-адресу приложения. Страница содержит несколько вкладок со сведениями об исключении и запросе. На первой вкладке приводится трассировка стека. 

![Трассировка стека](error-handling/_static/developer-exception-page.png)

На следующей вкладке представлены параметры строки запроса, если они имеются.

![Параметры строки запроса](error-handling/_static/developer-exception-page-query.png)

У этого запроса не было файлов cookie, но если бы они были, они бы отображались на вкладке **Cookies** (Файлы cookie). На последней вкладке можно просмотреть переданные заголовки.

![Заголовки](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a>Настройка пользовательской страницы обработки исключений

Если приложение выполняется не в среде `Development`, настройте страницу обработчика исключений.

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

В приложении Razor Pages шаблон Razor Pages [dotnet new](/dotnet/core/tools/dotnet-new) предоставляет страницу ошибок и класс модели страницы `ErrorModel` в папке *Pages*.

В приложении MVC не следует добавлять в метод действия обработки ошибок атрибуты метода HTTP, например `HttpGet`. Из-за использования явных команд некоторые запросы могут не передаваться в метод. Разрешите анонимный доступ к методу, чтобы не прошедшие проверку подлинности пользователи могли открывать представление ошибок.

Например, следующий метод обработчика ошибок предоставляется шаблоном MVC [dotnet new](/dotnet/core/tools/dotnet-new) и отображается в контроллере Home:

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configuring-status-code-pages"></a>Настройка страниц с кодами состояния

По умолчанию приложение не предоставляет специальных страниц для кодов состояния HTTP, таких как код *404 Не найдено*. Чтобы предоставить страницы кодов состояния, настройте ПО промежуточного слоя страниц кода состояния, добавив строку в метод `Startup.Configure`:

```csharp
app.UseStatusCodePages();
```

По умолчанию это ПО промежуточного слоя добавляет простые текстовые обработчики для распространенных кодов состояния, например 404:

![Страница 404](error-handling/_static/default-404-status-code.png)

ПО промежуточного слоя поддерживает несколько методов расширения. Один метод принимает лямбда-выражение:

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

Другой метод принимает строку формата и типа содержимого:

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

Имеются также методы расширения для перенаправления и повторного выполнения. Метод перенаправления отправляет клиенту код состояния 302:

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

Метод повторного выполнения возвращает исходный код состояния клиенту, а также выполняет обработчик для URL-адреса перенаправления:

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

Можно отключить страницы кодов состояния для конкретных запросов в методе обработчика Razor Pages или в контроллере MVC. Чтобы отключить страницы кодов состояния, попробуйте извлечь [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) из коллекции запроса [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) и отключить эту функцию, если она доступна:

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

Если вы используете перегрузку `UseStatusCodePages*`, которая указывает на конечную точку в приложении, создайте представление MVC или страницу Razor для конечной точки. Например, шаблон [dotnet new](/dotnet/core/tools/dotnet-new) для приложения Razor Pages создает следующую страницу и класс модели страницы:

*Error.cshtml*:

```cshtml
@page
@model ErrorModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to <strong>Development</strong> environment will display more detailed information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications</strong>, as it can result in sensitive information from exceptions being displayed to end users. For local debugging, development environment can be enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment variable to <strong>Development</strong>, and restarting the application.
</p>
```

*Error.cshtml.cs*:

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

## <a name="exception-handling-code"></a>Код обработки исключений

Код на страницах обработки исключений может создавать исключения. Часто желательно, чтобы страницы ошибок в рабочей среде содержали только статическое содержимое.

Кроме того, имейте в виду, что после отправки заголовков для ответа нельзя изменить код состояния ответа, а также невозможно выполнение страниц или обработчиков исключений. Необходимо завершить ответ или прервать подключение.

## <a name="server-exception-handling"></a>Обработка исключений на сервере

Помимо логики обработки исключений в приложении, [сервер](xref:fundamentals/servers/index), на котором размещается приложение, также выполняет ряд операций по обработке исключений. Если сервер перехватывает исключение перед отправкой заголовков, он отсылает ответ *500 Внутренняя ошибка сервера* без текста. Если сервер перехватывает исключение после отправки заголовков, он закрывает соединение. Запросы, не обработанные приложением, обрабатываются сервером. Все возникшие исключения обрабатываются с помощью механизма обработки исключений на сервере. Настроенные пользовательские страницы ошибок, промежуточный слой обработки исключений и фильтры не влияют на этот процесс.

## <a name="startup-exception-handling"></a>Обработка исключений при запуске

Исключения, возникающие во время запуска приложения, могут обрабатываться только на уровне размещения. При помощи [веб-узла](xref:fundamentals/host/web-host) вы можете [настроить реакцию узла на ошибки при запуске](xref:fundamentals/host/web-host#detailed-errors) с помощью ключей `captureStartupErrors` и `detailedErrors`.

Узел может отображать страницу со сведениями о перехваченной ошибке при запуске, только если ошибка произошла после привязки адреса и порта узла. Если выполнить привязку по какой-либо причине не удается, уровень размещения записывает в журнал критическое исключение, происходит аварийное завершение процесса dotnet и страница со сведениями об ошибке не выводится, когда приложение запущено на сервере [Kestrel](xref:fundamentals/servers/kestrel).

При работе в службе [IIS](/iis) или [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) возвращает ошибку *502.5 Сбой процесса*, если процесс невозможно запустить. Следуйте рекомендациям по устранению неполадок из раздела [Устранение неполадок ASP.NET Core в службах IIS](xref:host-and-deploy/iis/troubleshoot).

## <a name="aspnet-mvc-error-handling"></a>Обработка ошибок ASP.NET MVC

В приложениях [MVC](xref:mvc/overview) есть дополнительные возможности обработки ошибок, например настройка фильтров исключений и проведение проверки модели.

### <a name="exception-filters"></a>Фильтры исключений

Фильтры исключений можно настраивать глобально либо для отдельных контроллеров, либо для действий в приложении MVC. Эти фильтры обрабатывают все необработанные исключения, которые возникают во время выполнения действия контроллера или другого фильтра. Иным образом они не вызываются. Дополнительные сведения о фильтрах исключений см. в статье [Фильтры](xref:mvc/controllers/filters).

> [!TIP]
> Фильтры исключений хорошо подходят для перехвата исключений, которые происходят в действиях MVC, однако они не так гибки, как ПО промежуточного слоя для обработки ошибок. ПО промежуточного слоя предпочтительнее для общих случаев. Фильтры следует использовать только тогда, когда обработка ошибок должна осуществляться *по-разному* в зависимости от выбранного действия MVC.

### <a name="handling-model-state-errors"></a>Обработка ошибок состояния модели

[Проверка модели](xref:mvc/models/validation) проводится перед вызовом каждого действия контроллера. Метод действия отвечает за проверку свойства `ModelState.IsValid` и соответствующую реакцию.

В некоторых приложениях соблюдается стандартное соглашение об обработке ошибок проверки модели. В этом случае подходящим местом для реализации такой политики может быть [фильтр](xref:mvc/controllers/filters). Следует проверить, как выполняются действия при недопустимых состояниях модели. Дополнительные сведения см. в статье [Тестирование логики контроллера](xref:mvc/controllers/testing).
