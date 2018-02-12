---
title: "ПО промежуточного слоя ASP.NET Core"
author: rick-anderson
description: "Сведения о ПО промежуточного слоя ASP.NET Core и конвейере запросов."
manager: wpickett
ms.author: riande
ms.date: 01/22/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/middleware
ms.openlocfilehash: 697a3a8e475dda0d48a11dbfad8fbb0603aa1bf8
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="aspnet-core-middleware-fundamentals"></a>Основы ПО промежуточного слоя ASP.NET Core

<a name="fundamentals-middleware"></a>

Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Стив Смит](https://ardalis.com/) (Steve Smith)

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-middleware"></a>Что такое ПО промежуточного слоя?

ПО промежуточного слоя — это программное обеспечение, выстраиваемое в виде конвейера приложения ASP.NET для обработки запросов и откликов. Каждый компонент:

* определяет, нужно ли передать запрос следующему компоненту в конвейере;
* может выполнять работу как до, так и после вызова следующего компонента в конвейере. 

Для построения конвейера запросов используются делегаты запроса. Они обрабатывают каждый HTTP-запрос.

Для их настройки служат методы расширения [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) и [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions). Отдельный делегат запроса можно указать встроенным в качестве анонимного метода (называемого встроенным ПО промежуточного слоя) либо определить в многоразовом классе. Эти многоразовые классы и встроенные анонимные методы являются *ПО промежуточного слоя* или *компонентами промежуточного слоя*. Каждый компонент промежуточного слоя представляет собой конвейер запросов, отвечающий за вызов следующего компонента в конвейере и замыкающий цепочку, если это необходимо.

Статья о [миграции модулей HTTP на ПО промежуточного слоя](../migration/http-modules.md) поясняет различия между конвейерами запросов в ASP.NET Core и предыдущих версиях, а также содержит дополнительные примеры ПО промежуточного слоя.

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a>Создание конвейера ПО промежуточного слоя с помощью IApplicationBuilder

Конвейер запросов ASP.NET Core состоит из последовательности делегатов запроса, вызываемых один за другим, как показано на этой схеме (поток выполнения обозначен черными стрелками).

![Шаблон обработки запросов, показывающий входящий запрос, обработку в трех компонентах промежуточного слоя и запрос, покидающий приложение. Каждый компонент промежуточного слоя выполняет свою логику и передает запрос следующему компоненту промежуточного слоя в операторе next(). После обработки в третьем компоненте промежуточного слоя запрос возвращается через два предыдущих компонента для дополнительной обработки после операторов next(), прежде чем он покинет приложение в качестве отклика клиенту.](middleware/_static/request-delegate-pipeline.png)

Каждый из делегатов может выполнять операции до и после следующего делегата. Делегат также может принять решение не передавать запрос следующему делегату, что называется замыканием конвейера запросов. Замыкание часто является предпочтительным, так как позволяет избежать ненужной работы. Например, компонент промежуточного слоя для статических файлов может возвратить запрос статического файла и выполнить замыкание, чтобы обойти оставшуюся часть конвейера. Делегаты обработки исключений должны вызываться в начале конвейера, чтобы они могли перехватывать исключения, возникающие в более поздних стадиях.

Простейшее приложение ASP.NET Core задает один делегат запроса, обрабатывающий все запросы. В этом случае конвейер запросов как таковой отсутствует. Вместо этого в ответ на каждый HTTP-запрос вызывается одна анонимная функция.

[!code-csharp[Main](middleware/sample/Middleware/Startup.cs)]

Первый делегат [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) завершает конвейер.

Несколько делегатов запроса можно соединить в цепочку с помощью [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions). Параметр `next` представляет следующий делегат в конвейере. (Помните, что замкнуть конвейер, *не* вызывая параметр *next*, невозможно.) Обычно действия можно выполнять как до, так и после следующего делегата, как показано в этом примере.

[!code-csharp[Main](middleware/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> Не вызывайте `next.Invoke` после отправки отклика клиенту. Изменения `HttpResponse` после запуска отклика приведут к возникновению исключения. Например, таким изменением может быть задание заголовков, кода состояние и т. п. Запись в тело отклика после вызова `next`:
> - может вызвать нарушение протокола, например, при записи больше указанного значения `content-length`;
> - может привести к нарушению формата, например, при записи нижнего колонтитула HTML в CSS-файл.
>
> [HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) удобное использовать для обозначения того, были ли отправлены заголовки и (или) выполнена запись в тело отклика.

## <a name="ordering"></a>Упорядочение

Порядок, в котором компоненты промежуточного слоя добавляются в метод `Configure`, определяет порядок их вызова при запросах и обратный порядок для отклика. Соблюдение этого порядка крайне важно для обеспечения безопасности, производительности и функциональности.

Метод Configure (см. ниже) добавляет следующие компоненты промежуточного слоя.

1. Обработка исключений/ошибок
2. Статический файловый сервер
3. Проверка подлинности
4. MVC

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)


```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseAuthentication();               // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseIdentity();                     // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

-----------

В представленном выше коде `UseExceptionHandler` — это первый компонент промежуточного слоя, добавленный в конвейер, поэтому он перехватывает все исключения, возникающие в последующих вызовах.

Компонент промежуточного слоя для статических файлов вызывается на раннем этапе конвейера, чтобы он мог обработать запросы и выполнить замыкание, минуя остальные компоненты. Этот компонент **не** выполняет проверки авторизации. Все обрабатываемые им файлы, включая расположенные в *wwwroot*, находятся в открытом доступе. Сведения о защите статических файлов см. в статье [Работа со статическими файлами](xref:fundamentals/static-files).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)


Если запрос не обрабатывается компонентом промежуточного слоя для статических файлов, он передается в компонент промежуточного слоя для удостоверений (`app.UseAuthentication`), который выполняет проверку подлинности. Этот компонент не замыкает запросы, не прошедшие проверку подлинности. Хотя он проверяет подлинность запросов, авторизация (и отклонение) выполняются только после того, как MVC выберет указанную страницу Razor Page или указанный контроллер и действие.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Если запрос не обрабатывается компонентом промежуточного слоя для статических файлов, он передается в компонент промежуточного слоя для удостоверений (`app.UseIdentity`), который выполняет проверку подлинности. Этот компонент не замыкает запросы, не прошедшие проверку подлинности. Хотя он проверяет подлинность запросов, авторизация (и отклонение) выполняются только после того, как MVC выберет указанный контроллер и действие.

-----------

Следующий пример показывает порядок компонентов промежуточного слоя, где запросы для статических файлов обрабатываются компонентом для статических файлов до компонента для сжатия откликов. При таком порядке сжатие статических файлов не выполняется. Отклики MVC из [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) могут сжиматься.

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name="middleware-run-map-use"></a>

### <a name="use-run-and-map"></a>Методы Use, Run и Map

Для настройки конвейера HTTP служат методы `Use`, `Run` и `Map`. Метод `Use` может замыкать конвейер (это происходит, если он не вызывает делегат запроса `next`). `Run` является соглашением, и некоторые компоненты промежуточного слоя могут предоставлять методы `Run[Middleware]`, которые выполняются в конце конвейера.

Расширения `Map*` используются в качестве соглашения для ветвления конвейера. [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) осуществляет ветвление конвейера запросов на основе совпадений для заданного пути запроса. Если путь запроса начинается с заданного пути, данная ветвь выполняется.

[!code-csharp[Main](middleware/sample/Chain/StartupMap.cs?name=snippet1)]

Ниже приведены запросы и отклики `http://localhost:1234` на базе предыдущего кода.

| Запрос | Ответ |
| --- | --- |
| localhost:1234 | Hello from non-Map delegate.  |
| localhost:1234/map1 | Map Test 1 |
| localhost:1234/map2 | Map Test 2 |
| localhost:1234/map3 | Hello from non-Map delegate.  |

Когда используется `Map`, соответствующие сегменты путей удаляются из `HttpRequest.Path` и добавляются к `HttpRequest.PathBase` для каждого запроса.

[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) осуществляет ветвление конвейера запросов на основе результата заданного предиката. Любой предикат типа `Func<HttpContext, bool>` можно использовать для сопоставления запросов с новой ветвью конвейера. В следующем примере предикат служит для определения наличия переменной строки запроса `branch`.

[!code-csharp[Main](middleware/sample/Chain/StartupMapWhen.cs?name=snippet1)]

Ниже приведены запросы и отклики `http://localhost:1234` на базе предыдущего кода.

| Запрос | Ответ |
| --- | --- |
| localhost:1234 | Hello from non-Map delegate.  |
| localhost:1234/?branch=master | Branch used = master|

`Map` поддерживает вложение, например:

```csharp
app.Map("/level1", level1App => {
       level1App.Map("/level2a", level2AApp => {
           // "/level1/level2a"
           //...
       });
       level1App.Map("/level2b", level2BApp => {
           // "/level1/level2b"
           //...
       });
   });
   ```

`Map` также может сопоставить несколько сегментов одновременно, например:

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a>Встроенное ПО промежуточного слоя

ASP.NET Core содержит следующие компоненты промежуточного слоя, а также описание порядка их добавления.

| ПО промежуточного слоя | Описание: | Номер |
| ---------- | ----------- | ----- |
| [Проверка подлинности](xref:security/authentication/identity) | Обеспечивает поддержку проверки подлинности. | Ставится перед тем, как потребуется `HttpContext.User`. Является конечным для обратных вызовов OAuth. |
| [CORS](xref:security/cors) | Настраивает общий доступ к ресурсам независимо от источника. | Ставится перед компонентами, использующими CORS. |
| [Диагностика](xref:fundamentals/error-handling) | Настраивает диагностику. | Ставится перед компонентами, выдающими ошибки. |
| [ForwardedHeaders/HttpOverrides](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | Пересылает заголовки, переданные через прокси-сервер, в текущий запрос. | Ставится перед компонентами, использующими обновленные поля (например: Scheme, Host, ClientIP, Method). |
| [Кэширование ответов](xref:performance/caching/middleware) | Обеспечивает поддержку для кэширования откликов. | Ставится перед компонентами, требующими кэширование. |
| [Сжатие откликов](xref:performance/response-compression) | Обеспечивает поддержку для сжатия откликов. | Ставится перед компонентами, требующими сжатие. |
| [RequestLocalization](xref:fundamentals/localization) | Обеспечивает поддержку локализации. | Ставится перед компонентами, для которых важна локализация. |
| [Маршрутизация](xref:fundamentals/routing) | Определяет и ограничивает маршруты запросов. | Является конечным для совпадающих маршрутов. |
| [Сеанс](xref:fundamentals/app-state) | Обеспечивает поддержку для управления пользовательскими сеансами. | Ставится перед компонентами, требующими сеанс. |
| [Статические файлы](xref:fundamentals/static-files) | Обеспечивает поддержку для обработки статических файлов и просмотра каталогов. | Является конечным, если запрос соответствует файлам. |
| [Переопределение адреса URL](xref:fundamentals/url-rewriting) | Обеспечивает поддержку для переопределения URL-адресов и перенаправления запросов. | Ставится перед компонентами, использующими URL-адрес. |
| [WebSockets](xref:fundamentals/websockets) | Обеспечивает поддержку протокола WebSockets. | Ставится перед компонентами, которым нужно принимать запросы WebSocket. |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a>Написание ПО промежуточного слоя

ПО промежуточного слоя обычно инкапсулируется в класс и предоставляется с помощью метода расширения. Рассмотрим следующее ПО промежуточного слоя, которое задает язык и региональные параметры для текущего запроса из строки запроса.

[!code-csharp[Main](middleware/sample/Culture/StartupCulture.cs?name=snippet1)]

Примечание. Приведенный выше пример кода демонстрирует создание компонента промежуточного слоя. Сведения о встроенной поддержке локализации ASP.NET Core см. в разделе [ Глобализация и локализация](xref:fundamentals/localization).

Это ПО промежуточного слоя можно проверить, передав язык и региональные параметры, например `http://localhost:7997/?culture=no`.

Следующий код перемещает делегат ПО промежуточного слоя в класс.

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddleware.cs)]

Следующий метод расширения предоставляет ПО промежуточного слоя посредством [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder).

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

Следующий код вызывает ПО промежуточного слоя из `Configure`.

[!code-csharp[Main](middleware/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

ПО промежуточного слоя должно соответствовать [принципу явных зависимостей](http://deviq.com/explicit-dependencies-principle/), предоставляя свои зависимости в своем конструкторе. ПО промежуточного слоя создается один раз за *время существования приложения*. В разделе *Зависимости отдельных запросов* ниже приведены сведения о том, как использовать службы совместно с ПО промежуточного слоя внутри запроса.

Компоненты промежуточного слоя могут разрешать свои зависимости, возникшие в результате введения зависимостей, за счет параметров конструктора. [`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) также может принимать дополнительные параметры напрямую.

### <a name="per-request-dependencies"></a>Зависимости отдельных запросов

Так как ПО промежуточного слоя создается при запуске приложения, а не для отдельных запросов, службы времени существования *scoped*, используемые конструкторами ПО промежуточного слоя, не являются общими с другими типами, возникшими в результате введения зависимостей, в каждом из запросов. Если необходимо предоставить службу *scoped* для совместного использования ПО промежуточного слоя и другими типами, добавьте ее в сигнатуру метода `Invoke`. Метод `Invoke` может принимать дополнительные параметры, заполняемые при введении зависимостей. Пример:

```c#
public class MyMiddleware
{
    private readonly RequestDelegate _next;

    public MyMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="resources"></a>Ресурсы

* [Пример кода, используемый в этом документе](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)
* [Миграция с модулей HTTP на ПО промежуточного слоя](../migration/http-modules.md)
* [Запуск приложения](startup.md)
* [Параметры запроса](request-features.md)
