---
title: Обработка ошибок в ASP.NET Core
author: tdykstra
description: Узнайте, как обрабатывать ошибки в приложениях ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/05/2019
uid: fundamentals/error-handling
ms.openlocfilehash: d809c70b3fae6b2d21d5ec0871298d905b873d5d
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665367"
---
# <a name="handle-errors-in-aspnet-core"></a>Обработка ошибок в ASP.NET Core

Авторы: [Том Дикстра](https://github.com/tdykstra/) (Tom Dykstra), [Люк Лэтем](https://github.com/guardrex) (Luke Latham) и [Стив Смит](https://ardalis.com/) (Steve Smith)

В этой статье рассматриваются основные методы обработки ошибок в приложениях ASP.NET Core.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="developer-exception-page"></a>Страница со сведениями об исключении для разработчика

Чтобы настроить приложение для отображения страницы с подробными сведениями об исключениях запросов, используйте *страницу исключений для разработчика*. Эта страница доступна в пакете [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) в [метапакете Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Добавьте строку в метод `Startup.Configure`, если приложение выполняется в [среде](xref:fundamentals/environments) разработки:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_UseDeveloperExceptionPage)]

Поместите вызов к <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> перед другим ПО промежуточного слоя, где требуется перехватывать исключения.

> [!WARNING]
> Включать страницу исключений для разработчика следует **только тогда, когда приложение выполняется в среде разработки**. Подробные сведения об исключениях не должны быть общедоступными при выполнении приложения в рабочей среде. Дополнительные сведения о настройке среды см. в статье <xref:fundamentals/environments>.

Чтобы просмотреть страницу исключений для разработчика, запустите образец приложения, указав среду `Development`, и добавьте элемент `?throw=true` к базовому URL-адресу приложения. Страница содержит следующие сведения об исключении и запросе:

* Трассировка стека
* параметры строки запроса (при наличии);
* файлы cookie (при наличии);
* Заголовки

## <a name="configure-a-custom-exception-handling-page"></a>Настройка пользовательской страницы обработки исключений

Когда приложение не выполняется в среде разработки, вызовите метод расширения <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>, чтобы добавить ПО промежуточного слоя для обработки исключений. ПО промежуточного слоя выполняет такие функции:

* Перехватывает исключения.
* Ведет журнал исключений.
* Повторно выполняет запрос в альтернативном конвейере для указанной страницы или контроллера. Запрос не выполняется повторно, если запущен отклик.

В следующем примере приложения с помощью <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> добавляется ПО промежуточного слоя для обработки исключений в средах, не предназначенных для разработки. Метод расширения определяет страницу ошибки или контроллер в конечной точке `/Error` для повторного выполнения запросов после перехвата исключений и их регистрации в журнале.

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_UseExceptionHandler1)]

Шаблон приложения Razor Pages предоставляет страницу ошибки (*.cshtml*) и <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> класс (`ErrorModel`) в папке Pages.

В приложении MVC приведенный ниже метод обработчика ошибок добавлен в шаблон приложения MVC. Он отображается в контроллере Home.

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

Не следует добавлять в метод действия обработки ошибок атрибуты метода HTTP, например `HttpGet`. Из-за использования явных команд некоторые запросы могут не передаваться в метод. Разрешите анонимный доступ к методу, чтобы не прошедшие проверку подлинности пользователи могли открывать представление ошибок.

## <a name="access-the-exception"></a>Доступ к исключению

Используйте интерфейс <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature>, чтобы получить доступ к исключению или к пути исходного запроса в контроллере или на странице:

* Путь доступен из свойства <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature.Path>.
* Считайте метод <xref:System.Exception?displayProperty=fullName> из унаследованного свойства [IExceptionHandlerFeature.Error](xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature.Error).

```csharp
// using Microsoft.AspNetCore.Diagnostics;

var exceptionHandlerPathFeature = 
    HttpContext.Features.Get<IExceptionHandlerPathFeature>();
var path = exceptionHandlerPathFeature?.Path;
var error = exceptionHandlerPathFeature?.Error;
```

> [!WARNING]
> **Не** передавайте клиентам конфиденциальную информацию об ошибках из <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> или <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature>. Сохранение ошибок создает риски для безопасности.

## <a name="configure-custom-exception-handling-code"></a>Настройка пользовательского кода обработки исключений

Альтернативой предоставления конечной точки для обработки ошибок с помощью [пользовательской страницы обработки исключений](#configure-a-custom-exception-handling-page) является предоставление лямбда-функции методу <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>. Использование лямбда-функции с методом <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> позволяет получить доступ к ошибке до возврата ответа.

Пример приложения демонстрирует пользовательский код обработки исключений в методе `Startup.Configure`. Вызовите исключение с помощью ссылки **Выдать исключение** на странице указателя. Лямбда-функция выполняет такой код:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_UseExceptionHandler2)]

> [!WARNING]
> **Не** передавайте клиентам конфиденциальную информацию об ошибках из <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> или <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature>. Сохранение ошибок создает риски для безопасности.

## <a name="configure-status-code-pages"></a>Настройка страниц с кодами состояния

По умолчанию приложение ASP.NET Core не предоставляет страницы для кодов состояния HTTP, таких как код *404 Not Found* (не найдено). Приложение возвращает код состояния без текста ответа. Чтобы указать коды состояния, используйте ПО промежуточного слоя страниц с кодами состояния.

Это ПО промежуточного слоя доступно в пакете [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) в [метапакете Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

Добавьте строку в метод `Startup.Configure`.

```csharp
app.UseStatusCodePages();
```

Вызовите метод <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> перед ПО промежуточного слоя для обработки запросов (например ПО промежуточного слоя для статических файлов и ПО промежуточного слоя MVC).

По умолчанию это ПО промежуточного слоя добавляет текстовые обработчики для распространенных кодов состояния, например *404 Not Found* (не найдено).

```
Status Code: 404; Not Found
```

Это ПО промежуточного слоя поддерживает несколько методов расширения, которые позволяют настроить его поведение.

Перегрузка <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> принимает лямбда-выражение, которое можно использовать для отработки пользовательской логики обработки ошибок и записи ответа вручную.

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

Перегрузка <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> принимает тип содержимого и строку формата, которые можно использовать для настройки типа содержимого и текста ответа.

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

### <a name="redirect-and-re-execute-extension-methods"></a>Методы расширения для перенаправления и повторного выполнения

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:

* Отправляет клиенту код состояния *302 — Found*.
* Перенаправляет клиента к расположению, предоставленному в шаблоне URL-адреса.

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> обычно используется, если приложение:

* Должно перенаправлять клиент в другую конечную точку, что обычно бывает в случаях, когда другое приложение обрабатывает ошибку. Для веб-приложений в адресной строке браузера клиента отображается конечная точка перенаправления.
* Не должно сохранять и возвращать исходный код состояния с ответом первичного перенаправления.

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:

* Возвращает исходный код состояния клиенту.
* Позволяет создать текст ответа путем повторного выполнения конвейера запросов с использованием другого пути.

```csharp
app.UseStatusCodePagesWithReExecute("/Error/{0}");
```

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> обычно используется, если приложение:

* Обрабатывает запрос без перенаправления к другой конечной точке. Для веб-приложений в адресной строке браузера клиента отображается изначально запрошенная конечная точка.
* Сохраняет и возвращает исходный код состояния с ответом.

Шаблоны могут содержать заполнитель (`{0}`) для кода состояния. Шаблон должен начинаться с косой черты (`/`). При использовании заполнителя убедитесь, что конечная точка (страница или контроллер) могут обрабатывать сегмент пути. Например, страница Razor для ошибок должна принимать необязательное значение сегмента с директивой `@page`.

```cshtml
@page "{code?}"
```

Можно отключить страницы кодов состояния для конкретных запросов в методе обработчика Razor Pages или в контроллере MVC. Чтобы отключить страницы кодов состояния, попробуйте извлечь <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature> из коллекции запроса [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features) и отключить эту функцию, если она доступна.

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

Чтобы использовать перегрузку <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*>, которая указывает на конечную точку в приложении, создайте представление MVC или страницу Razor для конечной точки. Например, шаблон приложения Razor Pages позволяет создать следующую страницу и класс модели страницы:

*Error.cshtml*:

::: moniker range=">= aspnetcore-2.2"

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
    Swapping to the <strong>Development</strong> environment displays 
    detailed information about the error that occurred.
</p>
<p>
    <strong>The Development environment shouldn't be enabled for deployed 
    applications.</strong> It can result in displaying sensitive information 
    from exceptions to end users. For local debugging, enable the 
    <strong>Development</strong> environment by setting the 
    <strong>ASPNETCORE_ENVIRONMENT</strong> environment variable to 
    <strong>Development</strong> and restarting the app.
</p>
```

*Error.cshtml.cs*:

```csharp
[ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

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
    Swapping to <strong>Development</strong> environment will display more detailed 
    information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications
    </strong>, as it can result in sensitive information from exceptions being 
    displayed to end users. For local debugging, development environment can be 
    enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment 
    variable to <strong>Development</strong>, and restarting the application.
</p>
```

*Error.cshtml.cs*:

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, 
        NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

::: moniker-end

## <a name="exception-handling-code"></a>Код обработки исключений

Код на страницах обработки исключений может создавать исключения. Часто желательно, чтобы страницы ошибок в рабочей среде содержали только статическое содержимое.

Также обратите внимание, что после отправки заголовков для ответа:

* Приложение не может изменить код состояния ответа.
* Нельзя запустить страницу или обработчик исключений. Необходимо завершить ответ или прервать подключение.

## <a name="server-exception-handling"></a>Обработка исключений на сервере

Помимо логики обработки исключений в приложении, [реализация сервера](xref:fundamentals/servers/index) может выполнять ряд операций по обработке исключений. Если сервер перехватывает исключение перед отправкой заголовков ответа, он отсылает ответ *500 Internal Server Error* (внутренняя ошибка сервера) без текста ответа. Если сервер перехватывает исключение после отправки заголовков ответа, он закрывает соединение. Запросы, не обработанные приложением, обрабатываются сервером. Все исключения, возникшие при обработке запроса серверов, обрабатываются с помощью механизма обработки исключений на сервере. Пользовательские страницы ошибок приложений, ПО промежуточного слоя для обработки исключений и фильтры не влияют на этот процесс.

## <a name="startup-exception-handling"></a>Обработка исключений при запуске

Исключения, возникающие во время запуска приложения, могут обрабатываться только на уровне размещения. При помощи [веб-узла](xref:fundamentals/host/web-host) вы можете [настроить реакцию узла на ошибки при запуске](xref:fundamentals/host/web-host#detailed-errors) с помощью ключей `captureStartupErrors` и `detailedErrors`.

Узел может отображать страницу со сведениями о перехваченной ошибке при запуске, только если ошибка произошла после привязки адреса и порта узла. При сбое привязки по какой-либо причине:

* На уровне размещения в журнале фиксируется критическое исключение.
* Выполняется аварийное завершение процесса dotnet.
* Если приложение запущено на сервере [Kestrel](xref:fundamentals/servers/kestrel), страница со сведениями об ошибке не выводится.

При работе в службе [IIS](/iis) или [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) [модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module) возвращает ошибку *502.5 Process Failure* (ошибка процесса), если процесс невозможно запустить. Для получения дополнительной информации см. <xref:host-and-deploy/iis/troubleshoot>. Сведения об устранении неполадок при запуске в службе приложений Azure см. в статье <xref:host-and-deploy/azure-apps/troubleshoot>.

## <a name="aspnet-core-mvc-error-handling"></a>Обработка ошибок в ASP.NET Core MVC

В приложениях [MVC](xref:mvc/overview) есть дополнительные возможности обработки ошибок, например настройка фильтров исключений и проведение проверки модели.

### <a name="exception-filters"></a>Фильтры исключений

Фильтры исключений можно настраивать глобально либо для отдельных контроллеров, либо для действий в приложении MVC. Эти фильтры обрабатывают все необработанные исключения, которые возникают во время выполнения действия контроллера или другого фильтра. Иным образом они не вызываются. Для получения дополнительной информации см. <xref:mvc/controllers/filters#exception-filters>.

> [!TIP]
> Фильтры исключений полезны при перехвате исключений, которые возникают в действиях MVC. Но эти фильтры не так гибки, как ПО промежуточного слоя для обработки исключений. Мы рекомендуем использовать ПО промежуточного слоя. Используйте фильтры, только если ошибки нужно обрабатывать *по-разному* (в зависимости от выбранного действия MVC).

### <a name="handle-model-state-errors"></a>Обработка ошибок состояния модели

Перед вызовом каждого действия контроллера [модель проверяется](xref:mvc/models/validation). За проверку свойства [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) и соответствующую реакцию отвечает метод действия.

В некоторых приложениях соблюдается стандартное соглашение об обработке ошибок [проверки модели](xref:mvc/models/validation). Подходящим механизмом для реализации такой политики может быть [фильтр](xref:mvc/controllers/filters). Следует проверить, как выполняются действия при недопустимых состояниях модели. Для получения дополнительной информации см. <xref:mvc/controllers/testing>.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
