---
title: ПО промежуточного слоя ASP.NET Core
author: rick-anderson
description: Сведения о ПО промежуточного слоя ASP.NET Core и конвейере запросов.
ms.author: riande
ms.custom: mvc
ms.date: 02/17/2019
uid: fundamentals/middleware/index
ms.openlocfilehash: bac121441d6856ca79affe1a3130e5cbc76debd9
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665393"
---
# <a name="aspnet-core-middleware"></a>ПО промежуточного слоя ASP.NET Core

Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Стив Смит](https://ardalis.com/) (Steve Smith)

ПО промежуточного слоя — это программное обеспечение, выстраиваемое в виде конвейера приложения для обработки запросов и откликов. Каждый компонент:

* определяет, нужно ли передать запрос следующему компоненту в конвейере;
* может выполнять работу как до, так и после вызова следующего компонента в конвейере.

Для построения конвейера запросов используются делегаты запроса. Они обрабатывают каждый HTTP-запрос.

Для их настройки служат методы расширения <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> и <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>. Отдельный делегат запроса можно указать встроенным в качестве анонимного метода (называемого встроенным ПО промежуточного слоя) либо определить в многоразовом классе. Эти многоразовые классы и встроенные анонимные методы являются *ПО промежуточного слоя* или *компонентами промежуточного слоя*. Каждый компонент ПО промежуточного слоя в конвейере запросов отвечает за вызов следующего компонента в конвейере или замыкает конвейер. Когда промежуточный слой замыкает конвейер, он становится *терминальным промежуточным слоем*, так как препятствует обработке запроса дальнейшими компонентами промежуточного слоя.

Статья <xref:migration/http-modules> поясняет различия между конвейерами запросов в ASP.NET Core и ASP.NET 4.x, а также содержит дополнительные примеры ПО промежуточного слоя.

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a>Создание конвейера ПО промежуточного слоя с помощью IApplicationBuilder

Конвейер запросов ASP.NET Core состоит из последовательности делегатов запроса, вызываемых один за другим. На следующей схеме демонстрируется этот принцип. Поток выполнения показан черными стрелками.

![Шаблон обработки запросов, показывающий входящий запрос, обработку в трех компонентах промежуточного слоя и запрос, покидающий приложение. Каждый компонент промежуточного слоя выполняет свою логику и передает запрос следующему компоненту промежуточного слоя в операторе next(). После обработки в третьем компоненте промежуточного слоя запрос возвращается через два предыдущих компонента в обратном порядке для дополнительной обработки после их операторов next(), прежде чем он покинет приложение в качестве отклика клиенту.](index/_static/request-delegate-pipeline.png)

Каждый из делегатов может выполнять операции до и после следующего делегата. Делегаты обработки исключений должны вызываться в начале конвейера, чтобы перехватывать исключения, возникающие на более поздних этапах.

Простейшее приложение ASP.NET Core задает один делегат запроса, обрабатывающий все запросы. В этом случае конвейер запросов как таковой отсутствует. Вместо этого в ответ на каждый HTTP-запрос вызывается одна анонимная функция.

[!code-csharp[](index/snapshot/Middleware/Startup.cs?name=snippet1)]

Первый делегат <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> завершает конвейер.

Несколько делегатов запроса можно соединить в цепочку с помощью <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>. Параметр `next` представляет следующий делегат в конвейере. Замыкать конвейер можно*не* вызывая параметр *next*. Обычно действия можно выполнять как до, так и после следующего делегата, как показано в этом примере.

[!code-csharp[](index/snapshot/Chain/Startup.cs?name=snippet1)]

Если делегат не передает запрос следующему делегату, это называется *замыканием конвейера запросов*. Замыкание часто является предпочтительным, так как позволяет избежать ненужной работы. Например, [компонент промежуточного слоя для статических файлов](xref:fundamentals/static-files) может выступать в роли *терминального промежуточного слоя*, обрабатывая запрос статического файла и замыкая оставшуюся часть конвейера. Компоненты промежуточного слоя, предшествующие терминальному промежуточному слою, по-прежнему обрабатывают код после их инструкций `next.Invoke`. Но учитывайте следующее предупреждение о попытке записи в ответ, который уже был отправлен.

> [!WARNING]
> Не вызывайте `next.Invoke` после отправки отклика клиенту. Изменения <xref:Microsoft.AspNetCore.Http.HttpResponse> после запуска отклика приведут к возникновению исключения. Например, таким изменением может быть задание HTTP-заголовков и кода состояния. Запись в тело отклика после вызова `next`:
>
> * может вызвать нарушение протокола, например, при записи больше указанного значения `Content-Length`;
> * может привести к нарушению формата, например, при записи нижнего колонтитула HTML в CSS-файл.
>
> <xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> удобно использовать для обозначения того, были ли отправлены заголовки или выполнена запись в тело отклика.

## <a name="order"></a>Номер

Порядок, в котором компоненты промежуточного слоя добавляются в метод `Startup.Configure`, определяет порядок их вызова при запросах и обратный порядок для отклика. Соблюдение этого порядка крайне важно для обеспечения безопасности, производительности и функциональности.

Метод `Startup.Configure` добавляет компоненты ПО промежуточного слоя для распространенных сценариев приложений:

::: moniker range=">= aspnetcore-2.0"

1. Обработка исключений/ошибок
1. Протокол HTTP Strict Transport Security.
1. Перенаправление HTTPS
1. Статический файловый сервер
1. Применение политик в отношении файлов cookie.
1. Проверка подлинности
1. Сеанс
1. MVC

```csharp
public void Configure(IApplicationBuilder app)
{
    if (env.IsDevelopment())
    {
        // When the app runs in the Development environment:
        //   Use the Developer Exception Page to report app runtime errors.
        //   Use the Database Error Page to report database runtime errors.
        app.UseDeveloperExceptionPage();
        app.UseDatabaseErrorPage();
    }
    else
    {
        // When the app doesn't run in the Development environment:
        //   Enable the Exception Handler Middleware to catch exceptions
        //     thrown in the following middlewares.
        //   Use the HTTP Strict Transport Security Protocol (HSTS)
        //     Middleware.
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    // Use HTTPS Redirection Middleware to redirect HTTP requests to HTTPS.
    app.UseHttpsRedirection();

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Use Cookie Policy Middleware to conform to EU General Data 
    // Protection Regulation (GDPR) regulations.
    app.UseCookiePolicy();

    // Authenticate before the user accesses secure resources.
    app.UseAuthentication();

    // If the app uses session state, call Session Middleware after Cookie 
    // Policy Middleware and before MVC Middleware.
    app.UseSession();

    // Add MVC to the request pipeline.
    app.UseMvc();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

1. Обработка исключений/ошибок
1. Статические файлы
1. Проверка подлинности
1. Сеанс
1. MVC

```csharp
public void Configure(IApplicationBuilder app)
{
    // Enable the Exception Handler Middleware to catch exceptions
    //   thrown in the following middlewares.
    app.UseExceptionHandler("/Home/Error");

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Authenticate before you access secure resources.
    app.UseIdentity();

    // If the app uses session state, call UseSession before 
    // MVC Middleware.
    app.UseSession();

    // Add MVC to the request pipeline.
    app.UseMvcWithDefaultRoute();
}
```

::: moniker-end

В предыдущем примере кода каждый метод расширения ПО промежуточного слоя представляется в <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> с использованием пространства имен <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName>.

<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> — это первый компонент промежуточного слоя, добавленный в конвейер. Таким образом, обработчик исключений ПО промежуточного слоя перехватывает все исключения, возникающие в последующих вызовах.

Компонент промежуточного слоя для статических файлов вызывается на раннем этапе конвейера, чтобы он мог обработать запросы и выполнить замыкание, минуя остальные компоненты. Этот компонент **не** выполняет проверки авторизации. Все обрабатываемые им файлы, включая расположенные в *wwwroot*, находятся в открытом доступе. Сведения о защите статических файлов см. в статье <xref:fundamentals/static-files>.

::: moniker range=">= aspnetcore-2.0"

Если запрос не обрабатывается компонентом промежуточного слоя для статических файлов, он передается в компонент промежуточного слоя для проверки подлинности (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), который выполняет проверку подлинности. Этот компонент не замыкает запросы, не прошедшие проверку подлинности. Хотя ПО промежуточного слоя для проверки подлинности проверяет подлинность запросов, авторизация (и отклонение) выполняются только после того, как MVC выберет указанную страницу Razor Page или контроллер MVC и действие.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Если запрос не обрабатывается компонентом промежуточного слоя для статических файлов, он передается в компонент промежуточного слоя для удостоверений (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), который выполняет проверку подлинности. Этот компонент не замыкает запросы, не прошедшие проверку подлинности. Хотя он проверяет подлинность запросов, авторизация (и отклонение) выполняются только после того, как MVC выберет указанный контроллер и действие.

::: moniker-end

Следующий пример показывает порядок компонентов промежуточного слоя, где запросы для статических файлов обрабатываются компонентом для статических файлов до компонента для сжатия откликов. Статические файлы не сжимаются с этим порядком ПО промежуточного слоя. Отклики MVC из <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> можно сжать.

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files not compressed by Static File Middleware.
    app.UseStaticFiles();
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

### <a name="use-run-and-map"></a>Методы Use, Run и Map

Для настройки конвейера HTTP служат методы `Use`, `Run` и `Map`. Метод `Use` может замыкать конвейер (это происходит, если он не вызывает делегат запроса `next`). `Run` является соглашением, и некоторые компоненты промежуточного слоя могут предоставлять методы `Run[Middleware]`, которые выполняются в конце конвейера.

Расширения <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> используются в качестве соглашения для ветвления конвейера. `Map*` осуществляет ветвление конвейера запросов на основе совпадений для заданного пути запроса. Если путь запроса начинается с заданного пути, данная ветвь выполняется.

[!code-csharp[](index/snapshot/Chain/StartupMap.cs?name=snippet1)]

Ниже приведены запросы и отклики `http://localhost:1234` на базе предыдущего кода.

| Запрос             | Ответ                     |
| ------------------- | ---------------------------- |
| localhost:1234      | Hello from non-Map delegate. |
| localhost:1234/map1 | Map Test 1                   |
| localhost:1234/map2 | Map Test 2                   |
| localhost:1234/map3 | Hello from non-Map delegate. |

Когда используется `Map`, соответствующие сегменты путей удаляются из `HttpRequest.Path` и добавляются к `HttpRequest.PathBase` для каждого запроса.

[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) осуществляет ветвление конвейера запросов на основе результата заданного предиката. Любой предикат типа `Func<HttpContext, bool>` можно использовать для сопоставления запросов с новой ветвью конвейера. В следующем примере предикат служит для определения наличия переменной строки запроса `branch`.

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?name=snippet1)]

Ниже приведены запросы и отклики `http://localhost:1234` на базе предыдущего кода.

| Запрос                       | Ответ                     |
| ----------------------------- | ---------------------------- |
| localhost:1234                | Hello from non-Map delegate. |
| localhost:1234/?branch=master | Branch used = master         |

`Map` поддерживает вложение, например:

```csharp
app.Map("/level1", level1App => {
    level1App.Map("/level2a", level2AApp => {
        // "/level1/level2a" processing
    });
    level1App.Map("/level2b", level2BApp => {
        // "/level1/level2b" processing
    });
});
   ```

`Map` также может сопоставить несколько сегментов одновременно:

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?name=snippet1&highlight=13)]

## <a name="built-in-middleware"></a>Встроенное ПО промежуточного слоя

ASP.NET Core содержит следующие компоненты промежуточного слоя. В столбце *Порядок* указаны сведения о размещении ПО промежуточного слоя в конвейере обработки запросов и условия, в соответствии с которыми ПО промежуточного слоя может прервать обработку запроса. Если промежуточный слой замыкает конвейер обработки запроса и препятствует обработке запроса дальнейшими компонентами промежуточного слоя, он называется *терминальным промежуточным слоем*. Дополнительные сведения о замыкании конвейера см. в разделе [Создание конвейера ПО промежуточного слоя с помощью IApplicationBuilder](#create-a-middleware-pipeline-with-iapplicationbuilder).

| ПО промежуточного слоя | Описание | Номер |
| ---------- | ----------- | ----- |
| [Authentication](xref:security/authentication/identity) | Обеспечивает поддержку проверки подлинности. | Ставится перед тем, как потребуется `HttpContext.User`. Является конечным для обратных вызовов OAuth. |
| [Cookie Policy](xref:security/gdpr) | Позволяет отслеживать согласие пользователей на хранение личных сведений и применять минимальные стандарты для полей файлов cookie, таких как `secure` и `SameSite`. | Перед ПО промежуточного слоя, которое использует файлы cookie. Примеры Authentication, Session, MVC (TempData). |
| [CORS](xref:security/cors) | Настраивает общий доступ к ресурсам независимо от источника. | Ставится перед компонентами, использующими CORS. |
| [Обработка исключений](xref:fundamentals/error-handling) | Обрабатывает исключения. | Ставится перед компонентами, выдающими ошибки. |
| [Forwarded Headers](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | Пересылает заголовки, переданные через прокси-сервер, в текущий запрос. | Перед компонентами, использующими обновленные поля. Например: схема, узел, IP-адрес клиента, метод. |
| [Проверка работоспособности](xref:host-and-deploy/health-checks) | Проверяет работоспособность приложения ASP.NET Core и его зависимостей, таких как проверка доступности базы данных. | Является конечным, если запрос соответствует конечной точке проверки работоспособности. |
| [HTTP Method Override](/dotnet/api/microsoft.aspnetcore.builder.httpmethodoverrideextensions) | Разрешает входящий запрос POST для переопределения этого метода. | Ставится перед компонентами, использующими обновленный метод. |
| [HTTPS Redirection](xref:security/enforcing-ssl#require-https) | Перенаправление всех запросов HTTP на HTTPS (ASP.NET Core 2.1 или более поздней версии). | Ставится перед компонентами, использующими URL-адрес. |
| [HTTP Strict Transport Security (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | ПО промежуточного слоя для усовершенствования безопасности, которое добавляет специальный заголовок ответа (ASP.NET Core 2.1 или более поздней версии). | Перед отправкой ответов и после компонентов, изменяющих запросы. Примеры Forwarded Headers, URL Rewriting. |
| [MVC](xref:mvc/overview) | Обрабатывает запросы с помощью MVC/Razor Pages (ASP.NET Core 2.0 или более поздней версии). | Является конечным, если запрос соответствует маршруту. |
| [OWIN](xref:fundamentals/owin) | Взаимодействие с приложениями, серверами и ПО промежуточного слоя на основе OWIN. | Является конечным, если ПО промежуточного слоя OWIN полностью обрабатывает запрос. |
| [Кэширование ответов](xref:performance/caching/middleware) | Обеспечивает поддержку для кэширования откликов. | Ставится перед компонентами, требующими кэширование. |
| [Сжатие откликов](xref:performance/response-compression) | Обеспечивает поддержку для сжатия откликов. | Ставится перед компонентами, требующими сжатие. |
| [Localization](xref:fundamentals/localization) | Обеспечивает поддержку локализации. | Ставится перед компонентами, для которых важна локализация. |
| [Routing](xref:fundamentals/routing) | Определяет и ограничивает маршруты запросов. | Является конечным для совпадающих маршрутов. |
| [Session](xref:fundamentals/app-state) | Обеспечивает поддержку для управления пользовательскими сеансами. | Ставится перед компонентами, требующими сеанс. |
| [Static Files](xref:fundamentals/static-files) | Обеспечивает поддержку для обработки статических файлов и просмотра каталогов. | Является конечным, если запрос соответствует файлу. |
| [URL Rewriting](xref:fundamentals/url-rewriting) | Обеспечивает поддержку для переопределения URL-адресов и перенаправления запросов. | Ставится перед компонентами, использующими URL-адрес. |
| [WebSockets](xref:fundamentals/websockets) | Обеспечивает поддержку протокола WebSockets. | Ставится перед компонентами, которым нужно принимать запросы WebSocket. |

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/middleware/write>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
