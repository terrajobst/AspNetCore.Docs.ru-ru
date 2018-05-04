---
title: Перенести обработчики HTTP-данных и модули в по промежуточного слоя ASP.NET Core
author: rick-anderson
description: ''
manager: wpickett
ms.author: tdykstra
ms.date: 12/07/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/http-modules
ms.openlocfilehash: cbdef871ffc3269e3118d23ed20306a71b9df030
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="migrate-http-handlers-and-modules-to-aspnet-core-middleware"></a>Перенести обработчики HTTP-данных и модули в по промежуточного слоя ASP.NET Core

По [Мэтт Perdeck](https://www.linkedin.com/in/mattperdeck)

В этой статье показано, как выполнить миграцию существующих ASP.NET [HTTP-модули и обработчики в system.webserver](/iis/configuration/system.webserver/) для ASP.NET Core [по промежуточного слоя](xref:fundamentals/middleware/index).

## <a name="modules-and-handlers-revisited"></a>Модули и обработчики продукта

Прежде чем переходить к по промежуточного слоя ASP.NET Core давайте сначала освежить работу модулей и обработчиков HTTP:

![Обработчик модулей](http-modules/_static/moduleshandlers.png)

**Существуют следующие обработчики.**

   * Классы, реализующие [IHttpHandler](/dotnet/api/system.web.ihttphandler)

   * Используется для обработки запросов с заданным именем файла или расширения, такие как *.report*

   * [Настроить](/iis/configuration/system.webserver/handlers/) в *Web.config*

**Модули являются:**

   * Классы, реализующие [IHttpModule](/dotnet/api/system.web.ihttpmodule)

   * Вызывается для каждого запроса

   * Возможность краткой записи (прекратить дальнейшую обработку запроса)

   * Возможность добавить в HTTP-ответ или создать свои собственные

   * [Настроить](/iis/configuration/system.webserver/modules/) в *Web.config*

**Порядок, в котором модули обработки входящих запросов определяется:**

   1. [Жизненного цикла приложения](https://msdn.microsoft.com/library/ms227673.aspx), который является ряда событий, произошедших в ASP.NET: [BeginRequest](/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](/dotnet/api/system.web.httpapplication.authenticaterequest)и т. д. Каждый модуль можно создать обработчик для одного или нескольких событий.

   2. Для одного события в порядке, в котором они настроены в *Web.config*.

В дополнение к модули, можно добавить обработчики событий жизненного цикла для вашего *Global.asax.cs* файла. Эти обработчики запускать после обработчики в настроенные модули.

## <a name="from-handlers-and-modules-to-middleware"></a>Из обработчиков и модулей с по промежуточного слоя

**По промежуточного слоя, проще, чем модулей и обработчиков HTTP:**

   * Модули, обработчиками, *Global.asax.cs*, *Web.config* (за исключением конфигурации IIS) и жизненного цикла приложения будут удалены

   * Роли модули и обработчики было выполнено по промежуточного слоя

   * По промежуточного слоя, настроенные с помощью кода, а не в *Web.config*

   * [Ветвление конвейера](xref:fundamentals/middleware/index#middleware-run-map-use) позволяет отправлять запросы для определенного по промежуточного слоя, основанный на не только URL, но также и от заголовки запроса, строки запроса, и т. д.

**По промежуточного слоя очень похожи на модули:**

   * Вызывается в принципе для каждого запроса

   * Возможность краткой записи запроса, по [неправильно передает запрос на следующее по промежуточного слоя](#http-modules-shortcircuiting-middleware)

   * Возможность создавать свои собственные HTTP-ответа.

**По промежуточного слоя и модули, обрабатываются в другом порядке:**

   * Порядок по промежуточного слоя основан на порядке, в котором их вставки в конвейер запросов, хотя порядок модулей главным образом основан на [жизненного цикла приложения](https://msdn.microsoft.com/library/ms227673.aspx) события

   * Порядок по промежуточного слоя для ответов — обратное из того, что для запросов, а порядок модулей является одинаковым для запросов и ответов

   * В разделе [создания конвейера по промежуточного слоя с IApplicationBuilder](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder)

![ПО промежуточного слоя](http-modules/_static/middleware.png)

Обратите внимание на то, как в приведенном выше рисунке промежуточного по проверки подлинности short-circuited запроса.

## <a name="migrating-module-code-to-middleware"></a>Перенос кода модуля в по промежуточного слоя

Существующий модуль HTTP будет выглядеть следующим образом:

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

Как показано в [по промежуточного слоя](xref:fundamentals/middleware/index) , по промежуточного слоя ASP.NET Core используется класс, предоставляющий `Invoke` метод ведения `HttpContext` и возвращая `Task`. Новый по промежуточного слоя будет выглядеть следующим образом:

<a name="http-modules-usemiddleware"></a>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

Предыдущий шаблона по промежуточного слоя взят из раздела [записи по промежуточного слоя](xref:fundamentals/middleware/index#middleware-writing-middleware).

*MyMiddlewareExtensions* вспомогательный класс для упрощения настройки по промежуточного слоя в вашей `Startup` класса. `UseMyMiddleware` Метод добавляет по промежуточного слоя класса конвейера запросов. Службы, необходимые по промежуточного слоя получить введенный в конструкторе по промежуточного слоя.

<a name="http-modules-shortcircuiting-middleware"></a>

Модуль может вызвать завершение запроса, например, если пользователь не имеет разрешения:

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

Обрабатывает это по промежуточного слоя, не вызвав `Invoke` на следующее по промежуточного слоя в конвейере. Имейте в виду, что это не завершить полностью запроса, из-за предыдущих middlewares по-прежнему вызываться, когда ответ проходит обратно через конвейер.

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

При переносе функциональные возможности своего модуля новый по промежуточного слоя, может оказаться, что код не будет компилироваться из-за `HttpContext` класса в ASP.NET Core значительно изменились. [Позже на](#migrating-to-the-new-httpcontext), будет показано, как выполнить миграцию на новый HttpContext ASP.NET Core.

## <a name="migrating-module-insertion-into-the-request-pipeline"></a>Миграция вставки модуля в конвейер обработки запросов

HTTP-модули обычно добавляются конвейера запросов, используя *Web.config*:

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

Преобразование с [Добавление нового по промежуточного слоя](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder) в конвейер обработки запросов в вашей `Startup` класса:

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

Точному конвейера, где вставить новый по промежуточного слоя зависит от события, он обрабатывается как модуль (`BeginRequest`, `EndRequest`, т. д.) и порядок их расположения в список модулей в *Web.config*.

Как уже указывалось, не жизненного цикла приложения в ASP.NET Core и порядок обработки ответов по промежуточного слоя отличается от порядка, используемого в модулях. Это может принять решение о упорядочивания более сложной задачей.

Если порядок становится проблемой, модуль может разбиваться несколько компонентов по промежуточного слоя, которые могут быть упорядочены независимо друг от друга.

## <a name="migrating-handler-code-to-middleware"></a>Перенос кода обработчика в по промежуточного слоя

Обработчик HTTP-данных выглядит следующим образом:

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

В проекте ASP.NET Core следует преобразовать это по промежуточного слоя, аналогичный следующему:

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

Это по промежуточного слоя очень похожа на соответствующий модули по промежуточного слоя. Единственная разница заключается в следующем Вот вызов `_next.Invoke(context)`. Это имеет смысл, поскольку обработчик в конце конвейера запросов, таким образом, не следующее по промежуточного слоя для вызова.

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a>Миграция вставки обработчик в конвейер обработки запросов

Настройка обработчика HTTP-данных выполняется в *Web.config* и выглядит примерно так:

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

Можно было преобразовать путем добавления нового обработчика по промежуточного слоя в конвейере вашей `Startup` класса, аналогично по промежуточного слоя, преобразованные из модулей. Проблема с такого подхода заключается в том, что он отправит все запросы по промежуточного слоя новый обработчик. Тем не менее необходимо только запросы расширений для доступа по промежуточного слоя. В этом случае получат вы те же функциональные возможности, которые были с обработчиком HTTP.

Одно из решений — создать ветвь конвейера запросов с использованием данного расширения с помощью `MapWhen` метода расширения. Для этого в том же `Configure` метод, который добавлять другого по промежуточного слоя:

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

`MapWhen` принимает следующие параметры:

1. Лямбда-выражения, принимающего `HttpContext` и возвращает `true` Если ветвь сбоя запроса. Это означает, что можно создать ветвь запросов не только на основании их расширения, но и заголовки запроса, параметров строки запроса, и т. д.

2. Лямбда-выражения, принимающего `IApplicationBuilder` и добавляет по промежуточного слоя для ветви. Это означает, что можно добавить дополнительные по промежуточного слоя в ветвь перед обработчика по промежуточного слоя.

По промежуточного слоя, добавить в конвейер, прежде чем ветвь будет вызываться для всех запросов; ветвь не оказывает влияния на них.

## <a name="loading-middleware-options-using-the-options-pattern"></a>Возможности по промежуточного слоя, с помощью шаблона параметров загрузки

Некоторые модули и обработчики имеют параметры конфигурации, которые хранятся в *Web.config*. Однако в ASP.NET Core новая модель конфигурации используются вместо *Web.config*.

Новый [система конфигурации](xref:fundamentals/configuration/index) предоставляет следующие параметры, чтобы устранить эту проблему:

* Напрямую внедрить параметры в по промежуточного слоя, как показано в [разделу](#loading-middleware-options-through-direct-injection).

* Используйте [параметры шаблона](xref:fundamentals/configuration/options):

1. Создание класса, содержащего параметры по промежуточного слоя, например:

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2. Хранить значения параметров

   Система конфигурации позволяет хранить значения параметров в любом месте требуется. Однако наиболее сайтов используйте *appsettings.json*, поэтому мы будем такой подход:

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

   *MyMiddlewareOptionsSection* вот имя раздела. Он не должен совпадать с именем класса параметров.

3. Связать со значениями параметров в классе

    Параметры шаблона используется платформа внедрения зависимостей ASP.NET Core сопоставление параметров типа (например, `MyMiddlewareOptions`) с `MyMiddlewareOptions` объекта, имеющего параметры.

    Обновление вашего `Startup` класса:

   1. Если вы используете *appsettings.json*, добавьте его в конструктор конфигурации в `Startup` конструктор:

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

   2. Настройка параметров службы:

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

   3. Свяжите варианты с классе параметров:

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4. Вставить параметры в ваш конструктор по промежуточного слоя. Это похоже на добавление параметров в контроллере.

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

   [UseMiddleware](#http-modules-usemiddleware) метод расширения, который добавляет по промежуточного слоя для `IApplicationBuilder` берет на себя внедрения зависимостей.

   Это не ограничивается `IOptions` объектов. Любой другой объект, требуется по промежуточного слоя могут быть добавлены таким способом.

## <a name="loading-middleware-options-through-direct-injection"></a>Загрузка параметры по промежуточного слоя посредством прямого внедрения

Шаблон параметров имеет то преимущество, что он создает свободные взаимозависимость между значениями параметров и их пользователями. После класс параметров связан с фактические параметры значений и любого другого класса могут получить доступ к параметры на платформе внедрения зависимостей. Нет необходимости передавать значения параметров.

Это разбивает то, что если вы хотите использовать одинаковые по промежуточного слоя дважды с различными параметрами. Например авторизации промежуточного слоя, используемое в различных ветвях, позволяя различным ролям. Два различных параметров объекта невозможно связать с одной параметры класса.

Решением является получить объекты параметров со значениями фактические параметры в вашей `Startup` класса и передавать их непосредственно в каждый экземпляр по промежуточного слоя.

1. Добавьте второй ключ *appsettings.json*

   Чтобы добавить второй набор параметров для *appsettings.json* файла следует использовать новый ключ для его однозначной идентификации:

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2. Получать значения параметров и их передачи в по промежуточного слоя. `Use...` Метода расширения (которая добавляет по промежуточного слоя в конвейере) логично передать значения параметров: 

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

3. Включение по промежуточного слоя, чтобы воспользоваться параметр options. Предоставить перегрузку `Use...` метода расширения (которая принимает параметр параметры и передает их в `UseMiddleware`). Когда `UseMiddleware` вызывается с параметрами, он передает параметры по промежуточного слоя конструктор при инициализации соответствующего объекта по промежуточного слоя.

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

   Обратите внимание на то, как это создает оболочку для объекта параметров в `OptionsWrapper` объекта. Это реализуется `IOptions`, как требуется для конструктора по промежуточного слоя.

## <a name="migrating-to-the-new-httpcontext"></a>Переход на новый интерфейс HttpContext

Вы уже видели раньше, `Invoke` метод в по промежуточного слоя принимает параметр типа `HttpContext`:

```csharp
public async Task Invoke(HttpContext context)
```

`HttpContext` значительно изменилась в ASP.NET Core. В этом разделе показано, как преобразовать наиболее часто используемые свойства [System.Web.HttpContext](/dotnet/api/system.web.httpcontext) к новому `Microsoft.AspNetCore.Http.HttpContext`.

### <a name="httpcontext"></a>HttpContext

**HttpContext.Items** преобразуется в:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

**Уникальный идентификатор запроса (аналог не System.Web.HttpContext)**

Предоставляет уникальный идентификатор для каждого запроса. Удобно использовать для включения в журналах.

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a>HttpContext.Request

**HttpContext.Request.HttpMethod** преобразуется в:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

**HttpContext.Request.QueryString** преобразуется в:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

**HttpContext.Request.Url** и **HttpContext.Request.RawUrl** перевести на:

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
> Чтение значений формы, только если тип содержимого sub *x-www-формы-urlencoded* или *данные формы*.

**HttpContext.Request.InputStream** преобразуется в:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> Этот код можно используйте только в обработчике типа по промежуточного слоя, в конце конвейера.
>
>Можно считывать необработанный текст, как показано выше только один раз для каждого запроса. Попытка чтения после первого чтения тела запроса по промежуточного слоя будет считывать пустой текст.
>
>Это не применимо к чтению формы, как показано выше, из-за этого из буфера.

### <a name="httpcontextresponse"></a>HttpContext.Response

**HttpContext.Response.Status** и **HttpContext.Response.StatusDescription** перевести на:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

**HttpContext.Response.ContentEncoding** и **HttpContext.Response.ContentType** перевести на:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

**HttpContext.Response.ContentType** на свой собственный также преобразуется в:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

**HttpContext.Response.Output** преобразуется в:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

**HttpContext.Response.TransmitFile**

Обслуживающий файл рассматривается [здесь](../fundamentals/request-features.md#middleware-and-request-features).

**HttpContext.Response.Headers**

Отправка заголовки ответа, осложняется тем, что если задать ничего произошло после записи в текст ответа, они не отправляется.

Решение заключается в том, чтобы задать метод обратного вызова, который будет вызываться перед записью на ответ начинается справа. Лучше всего для этого в начале `Invoke` метод в по промежуточного слоя. Это этот метод обратного вызова, который задает заголовки ответа, к.

Следующий код задает метод обратного вызова вызывается `SetHeaders`:

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

`SetHeaders` Метод обратного вызова будет выглядеть следующим образом:

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

**HttpContext.Response.Cookies**

Файлы cookie передаваться в браузере к *Set-Cookie* заголовок ответа. В результате для отправки файлов cookie требуется тому же обратному вызову, что использовались для отправки заголовков ответа:

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
