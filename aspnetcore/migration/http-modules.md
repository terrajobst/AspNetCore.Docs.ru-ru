---
title: Перенос обработчики и модули HTTP на по промежуточного слоя ASP.NET Core
author: rick-anderson
description: ''
ms.author: tdykstra
ms.date: 12/07/2016
uid: migration/http-modules
ms.openlocfilehash: 84381210910c66a7d121120b8c6b0f046cae8c4f
ms.sourcegitcommit: a1283d486ac1dcedfc7ea302e1cc882833e2c515
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2019
ms.locfileid: "67207808"
---
# <a name="migrate-http-handlers-and-modules-to-aspnet-core-middleware"></a>Перенос обработчики и модули HTTP на по промежуточного слоя ASP.NET Core

В этой статье показано, как перенос существующего ASP.NET [HTTP-модулей и обработчиков в system.webserver](/iis/configuration/system.webserver/) на ASP.NET Core [по промежуточного слоя](xref:fundamentals/middleware/index).

## <a name="modules-and-handlers-revisited"></a>Модули и обработчики revisited

Прежде чем переходить к по промежуточного слоя ASP.NET Core, сначала освежим в работе модулей и обработчиков HTTP:

![Обработчик модулей](http-modules/_static/moduleshandlers.png)

**Существуют следующие обработчики**

* Классы, реализующие [IHttpHandler](/dotnet/api/system.web.ihttphandler)

* Позволяет обрабатывать запросы с помощью указанного имени файла или расширение, например *отчетов*

* [Настроить](/iis/configuration/system.webserver/handlers/) в *Web.config*

**Модули являются:**

* Классы, реализующие [IHttpModule](/dotnet/api/system.web.ihttpmodule)

* Вызывается для каждого запроса

* Возможность краткой записи (прекратить дальнейшую обработку запроса)

* Возможность добавить в HTTP-ответа, или создать свои собственные

* [Настроить](/iis/configuration/system.webserver/modules/) в *Web.config*

**Порядок, в котором модули обрабатывают входящие запросы определяется:**

1. [Жизненного цикла приложения](https://msdn.microsoft.com/library/ms227673.aspx), который является серии события, инициируемые ASP.NET: [BeginRequest](/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](/dotnet/api/system.web.httpapplication.authenticaterequest)и т. д. Каждый модуль, можно создать обработчик для одного или нескольких событий.

2. Для того же события, порядок, в котором они настроены в *Web.config*.

В дополнение к модулям, можно добавить обработчики событий жизненного цикла вашего *Global.asax.cs* файл. Эти обработчики запустите после обработчиков в настроенные модули.

## <a name="from-handlers-and-modules-to-middleware"></a>Обработчики и модули на по промежуточного слоя

**По промежуточного слоя, проще, чем модулей и обработчиков HTTP:**

* Модули, обработчиков *Global.asax.cs*, *Web.config* (за исключением конфигурации IIS) и жизненного цикла приложения будут удалены

* Роли модули и обработчики были выполнены на по промежуточного слоя

* По промежуточного слоя настраиваются с помощью кода, а не в *Web.config*

* [Ветвление конвейера](xref:fundamentals/middleware/index#use-run-and-map) позволяет отправлять запросы к по промежуточного слоя, основываясь на не только URL, но и от заголовков запроса, строки запроса, и т.д.

**По промежуточного слоя очень похожи на модули:**

* Вызывается в принципе, для каждого запроса

* Возможность краткой записи запроса, по [неправильно передает запрос следующему компоненту промежуточного слоя](#http-modules-shortcircuiting-middleware)

* Возможность создавать свои собственные HTTP-ответа

**По промежуточного слоя и модули обрабатываются в другом порядке.**

* Порядок по промежуточного слоя основан на порядке, в котором их вставки в конвейер запросов, хотя порядок модулей главным образом основан на [жизненного цикла приложения](https://msdn.microsoft.com/library/ms227673.aspx) события

* Порядок по промежуточного слоя для ответов обратна из того, что для запросов, хотя порядок модулей одинаков для запросов и ответов

* См. в разделе [Создание конвейера по промежуточного слоя с помощью IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)

![ПО промежуточного слоя](http-modules/_static/middleware.png)

Обратите внимание на то, как в приведенном выше рисунке, по промежуточного слоя проверки подлинности сокращено запроса.

## <a name="migrating-module-code-to-middleware"></a>Перенос кода модуля на по промежуточного слоя

Существующий модуль HTTP будет выглядеть примерно следующим образом:

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

Как показано в [по промежуточного слоя](xref:fundamentals/middleware/index) страницы, по промежуточного слоя ASP.NET Core — это класс, который предоставляет `Invoke` метод ведения `HttpContext` и возвращая `Task`. Новый по промежуточного слоя будет выглядеть следующим образом:

<a name="http-modules-usemiddleware"></a>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

Предыдущий шаблон по промежуточного слоя, сделанный в из раздела [написание по промежуточного слоя](xref:fundamentals/middleware/write).

*MyMiddlewareExtensions* вспомогательный класс упрощает для настройки по промежуточного слоя в вашей `Startup` класса. `UseMyMiddleware` Метод добавляет класс по промежуточного слоя в конвейер запросов. Службы, необходимые для по промежуточного слоя получить внедрен в конструктор по промежуточного слоя.

<a name="http-modules-shortcircuiting-middleware"></a>

Модуль может вызвать завершение запроса, например, если пользователь не имеет разрешения:

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

По промежуточного слоя обрабатывает это, не вызвав `Invoke` на следующее по промежуточного слоя в конвейере. Имейте в виду, это не прервать полностью запрос, так как предыдущий по промежуточного слоя по-прежнему вызываться, когда ответ проходит обратно через конвейер.

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

При переносе функциональные возможности вашего модуля нового по промежуточного слоя, вы обнаружите, что ваш код не компилироваться, поскольку `HttpContext` класс существенно изменились в ASP.NET Core. [Впоследствии](#migrating-to-the-new-httpcontext), вы увидите, как перенести в новый HttpContext ASP.NET Core.

## <a name="migrating-module-insertion-into-the-request-pipeline"></a>Миграция вставки модуля в конвейер запросов

Модули HTTP обычно добавляются в конвейер запросов с помощью *Web.config*:

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

Преобразование с [Добавление нового по промежуточного слоя](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) в конвейер запросов в вашей `Startup` класса:

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

Точному в конвейере, задав новый по промежуточного слоя зависит от события, оно будет обработано как модуль (`BeginRequest`, `EndRequest`т. д.) и порядок их расположения в списке модулей в *Web.config*.

Как уже говорилось, не жизненного цикла приложения в ASP.NET Core и порядок, в котором ответы обрабатываются по промежуточного слоя отличается от порядка, используемых модулями. Это может сделать ваше решение упорядочивания более сложной задачей.

Если упорядочение становится проблемой, можно разделить на несколько компонентов по промежуточного слоя, которые могут быть упорядочены независимо друг от друга вашего модуля.

## <a name="migrating-handler-code-to-middleware"></a>Перенос кода обработчика для по промежуточного слоя

Обработчик HTTP выглядит примерно следующим образом:

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

В проекте ASP.NET Core следует преобразовать это по промежуточного слоя, аналогичную следующей:

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

Это по промежуточного слоя очень похож на по промежуточного слоя, соответствующий модулей. Это единственное реальное различие здесь отсутствует вызов к `_next.Invoke(context)`. Это имеет смысл, поскольку обработчик является в конце конвейера запросов, таким образом, не следующее по промежуточного слоя для вызова.

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a>Миграция вставки обработчик в конвейер запросов

Настройка обработчика HTTP-данных выполняется в *Web.config* и выглядит примерно следующим образом:

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

Можно преобразовать это, добавив новый обработчик по промежуточного слоя в конвейер запросов в вашей `Startup` класс, аналогичную по промежуточного слоя, преобразованные из модулей. Проблема этого подхода является то, что он будет отправлять все запросы по промежуточного слоя новый обработчик. Тем не менее требуется только доставку по промежуточного слоя запросов с помощью заданного модуля. Даст те же функциональные возможности, которые были с обработчиком HTTP.

Одним из решений является ветвление конвейера запросов с помощью данного расширения, с помощью `MapWhen` метода расширения. Для этого в том же `Configure` метод, где добавить другого по промежуточного слоя:

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

`MapWhen` принимает следующие параметры:

1. Лямбда-выражение, которое принимает `HttpContext` и возвращает `true` Если запрос должен вышли из строя ветвь. Это означает, что можно выполнять ветвление запросов не только на основании их расширения, но также в заголовки запросов, параметры строки запроса и т. д.

2. Лямбда-выражение, которое принимает `IApplicationBuilder` и добавляет по промежуточного слоя для ветви. Это означает, что можно добавить дополнительные по промежуточного слоя в ветвь перед обработчик по промежуточного слоя.

Промежуточного слоя, добавляемого в конвейер, прежде чем ветвь будет вызываться для всех запросов; ветвь не оказывает влияния на них.

## <a name="loading-middleware-options-using-the-options-pattern"></a>Возможности по промежуточного слоя с помощью шаблона параметров загрузки

Некоторые модули и обработчики имеют параметры конфигурации, которые хранятся в *Web.config*. Тем не менее, в ASP.NET Core новая модель конфигурации используется вместо *Web.config*.

Новый [система конфигурации](xref:fundamentals/configuration/index) предоставляет следующие параметры, чтобы решить эту проблему:

* Напрямую внедрить параметры в по промежуточного слоя, как показано в [разделу](#loading-middleware-options-through-direct-injection).

* Используйте [шаблон параметров](xref:fundamentals/configuration/options):

1. Создание класса, содержащего параметры по промежуточного слоя, например:

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2. Store со значениями

   Система конфигурации позволяет хранить параметр в любом нужные значения. Однако наиболее сайтов используйте *appsettings.json*, поэтому мы рассмотрим этот подход:

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

   *MyMiddlewareOptionsSection* вот имя раздела. Он не обязательно совпадает с именем класса параметров.

3. Связать со значениями с класс параметров

    Шаблон параметров использует платформой внедрения зависимостей ASP.NET Core, чтобы связать тип параметров (таких как `MyMiddlewareOptions`) с `MyMiddlewareOptions` объекта, имеющего параметры.

    Обновление вашей `Startup` класса:

   1. Если вы используете *appsettings.json*, добавьте его в построитель конфигурации в `Startup` конструктор:

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

   2. Настройка параметров службы:

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

   3. Свяжите параметры с вашего класса параметров:

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4. Вставить параметры в ваш конструктор по промежуточного слоя. Это похоже на добавление параметров в контроллер.

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

   [UseMiddleware](#http-modules-usemiddleware) метод расширения, который добавляет по промежуточного слоя для `IApplicationBuilder` берет на себя внедрения зависимостей.

   Это не ограничивается `IOptions` объектов. Таким образом могут внедряться и любой другой объект, требуется по промежуточного слоя.

## <a name="loading-middleware-options-through-direct-injection"></a>Загрузка параметры по промежуточного слоя посредством прямого внедрения

Шаблон параметров имеет то преимущество, что он создает ослабить связь между значениями параметров и их пользователей. После связывания класса параметров со значениями фактических параметров любого другого класса можно получить доступ к параметрам через платформой внедрения зависимостей. Нет необходимости передавать значения параметров.

Реализация делится Однако если вы хотите использовать же по промежуточного слоя дважды с различными параметрами. Например авторизации промежуточного слоя, используемое в других ветвях, позволяя разные роли. Два объекта различные варианты нельзя будет связать с одной параметры класса.

Решением является получение параметрических объектов со значениями фактических параметров в вашей `Startup` класса и передать их напрямую к каждому экземпляру по промежуточного слоя.

1. Добавьте второй ключ для *appsettings.json*

   Чтобы добавить второй набор параметров для *appsettings.json* файла следует использовать новый ключ для уникальной идентификации:

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2. Извлечь значения параметров и передавать их по промежуточного слоя. `Use...` Метода расширения (который добавляет по промежуточного слоя в конвейере) является логичным местом для передачи значений параметра: 

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

3. Включите по промежуточного слоя, чтобы воспользоваться параметра options. Обеспечьте перегрузку `Use...` метода расширения (который принимает параметр options и передает его `UseMiddleware`). Когда `UseMiddleware` вызывается с параметрами, он передает параметры в ваш конструктор по промежуточного слоя, когда он создает экземпляр объекта по промежуточного слоя.

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

   Обратите внимание на то, как это создает оболочку объект параметров в `OptionsWrapper` объекта. Этот код реализует `IOptions`, как требуется для конструктора по промежуточного слоя.

## <a name="migrating-to-the-new-httpcontext"></a>Переход на новый HttpContext

Вы уже видели, `Invoke` метод в по промежуточного слоя принимает параметр типа `HttpContext`:

```csharp
public async Task Invoke(HttpContext context)
```

`HttpContext` сильно изменился в ASP.NET Core. В этом разделе показано, как перевести наиболее часто используемые свойства [System.Web.HttpContext](/dotnet/api/system.web.httpcontext) к новому `Microsoft.AspNetCore.Http.HttpContext`.

### <a name="httpcontext"></a>HttpContext

**HttpContext.Items** преобразуется в:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

**Уникальный идентификатор запроса (эквивалент System.Web.HttpContext)**

Предоставляет уникальный идентификатор для каждого запроса. Очень полезно включить в журналах.

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a>HttpContext.Request

**HttpContext.Request.HttpMethod** преобразуется в:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

**HttpContext.Request.QueryString** преобразуется в:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

**HttpContext.Request.Url** и **HttpContext.Request.RawUrl** перевода:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

**HttpContext.Request.IsSecureConnection** преобразуется в:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

**HttpContext.Request.UserHostAddress** преобразуется в:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

**HttpContext.Request.Cookies** преобразуется в:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

**HttpContext.Request.RequestContext.RouteData** преобразуется в:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

**HttpContext.Request.Headers** преобразуется в:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

**HttpContext.Request.UserAgent** преобразуется в:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

**HttpContext.Request.UrlReferrer** преобразуется в:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

**HttpContext.Request.ContentType** преобразуется в:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

**HttpContext.Request.Form** преобразуется в:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> Чтение значений формы, только в том случае, если тип содержимого sub — *x-www-формы-urlencoded* или *данные формы*.

**HttpContext.Request.InputStream** преобразуется в:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> Используйте этот код только в обработчике тип по промежуточного слоя, в конце конвейера.
>
>Вы можете прочитать необработанном тексте, как показано выше только один раз в запрос. По промежуточного слоя, попытка чтения текста после первого чтения будет считывать пустым текстом.
>
>Это не относится к чтению формы, как показано выше, так как это делается из буфера.

### <a name="httpcontextresponse"></a>HttpContext.Response

**HttpContext.Response.Status** и **HttpContext.Response.StatusDescription** перевода:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

**HttpContext.Response.ContentEncoding** и **HttpContext.Response.ContentType** перевода:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

**HttpContext.Response.ContentType** на свой собственный также преобразуется в:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

**HttpContext.Response.Output** преобразуется в:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

**HttpContext.Response.TransmitFile**

Обслуживает файл рассматривается [здесь](../fundamentals/request-features.md#middleware-and-request-features).

**HttpContext.Response.Headers**

Отправляя заголовки ответа усложняется тем фактом, что если вы их после записи данных в текст ответа, они не отправляются.

Решение заключается в том, чтобы задать метод обратного вызова, который будет вызываться перед записью на ответ начинается справа. Лучше всего это делается в начале `Invoke` метод в по промежуточного слоя. Это этого метода обратного вызова, которая задает вашей заголовки ответа.

Следующий код задает метод обратного вызова, вызванный `SetHeaders`:

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

`SetHeaders` Метод обратного вызова будет выглядеть следующим образом:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

**HttpContext.Response.Cookies**

Файлы cookie передаются обозревателю в *Set-Cookie* заголовок ответа. В результате отправки файлов cookie требуется разрешение тому же обратному вызову, что и для отправки заголовков ответа:

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

`SetCookies` Метод обратного вызова будет выглядеть следующим образом:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Обработчики HTTP-данных и общие сведения о модули HTTP](/iis/configuration/system.webserver/)
* [Конфигурация](xref:fundamentals/configuration/index)
* [Запуск приложения](xref:fundamentals/startup)
* [ПО промежуточного слоя](xref:fundamentals/middleware/index)
