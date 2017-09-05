---
title: "По промежуточного слоя ASP.NET Core"
author: rick-anderson
description: "Описание по промежуточного слоя и конвейер обработки запросов."
keywords: "ASP.NET Core, по промежуточного слоя, конвейера, делегат"
ms.author: riande
manager: wpickett
ms.date: 08/14/2017
ms.topic: article
ms.assetid: db9a86ab-46c2-40e0-baed-86e38c16af1f
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware
ms.openlocfilehash: 84c357ebbf28dffc4382f6c648921210e72ac854
ms.sourcegitcommit: 26166785ad181a8519cb008800d71d96499b0499
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/01/2017
---
# <a name="aspnet-core-middleware-fundamentals"></a>Принципы работы по промежуточного слоя ASP.NET Core

<a name=fundamentals-middleware></a>

По [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Стив Смит](http://ardalis.com)

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)

## <a name="what-is-middleware"></a>Новые возможности по промежуточного слоя

По промежуточного слоя — программное обеспечение, построенный в конвейер приложения для обработки запросов и ответов. Каждый компонент:

* Выбирает, нужно ли передать запрос к следующему компоненту в конвейере.
* Может выполнять работу, до и после вызова следующему компоненту в конвейере. 

Запрос делегаты используются для построения конвейера запросов. Делегаты запрос обработки каждого HTTP-запроса.

Запрос делегаты настраиваются с помощью [запуска](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [карты](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), и [используйте](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) методы расширения. Отдельный запрос делегат может быть указанную строку в качестве анонимного метода (называемые по промежуточного слоя в строке), или могут определяться в классе для повторного использования. Эти классы многократного использования и анонимные методы в строке *по промежуточного слоя*, или *компонентов по промежуточного слоя*. Каждый компонент по промежуточного слоя в конвейере отвечает за вызов следующему компоненту в конвейере и сокращенное вычисление цепочке при необходимости.

[Миграция модули HTTP с по промежуточного слоя](../migration/http-modules.md) объясняются различия между конвейеры запроса в ASP.NET Core и более ранние версии, а также дополнительные примеры по промежуточного слоя.

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a>Создание конвейера по промежуточного слоя с IApplicationBuilder

Конвейер обработки запросов ASP.NET Core состоит из последовательности делегатов запроса, называемый один за другим, как показано на этой диаграмме (поток выполнения следующим черной стрелки):

![При обработке шаблона запроса отображение запросов, поступающих, обработки через три middlewares и ответ, так как приложение. Каждый по промежуточного слоя выполняет логику и передает запрос на следующее по промежуточного слоя на операторе next(). После третьего по промежуточного слоя обрабатывает запрос, он выдан обратно через предыдущих двух middlewares дополнительную обработку после операторов next() каждого в свою очередь перед выходом из приложения в качестве ответа клиенту.](middleware/_static/request-delegate-pipeline.png)

Все делегаты могут выполнять операции, до и после следующего делегата. Делегат можно также принять решение не передавать запрос далее делегата, который называется сокращенного вычисления конвейер обработки запросов. Сокращенные вычисления часто является предпочтительным, так как он позволяет избежать ненужных работу. Например по промежуточного слоя статических файлов можно вернуть запрос статического файла и сокращенные оставшуюся часть конвейера. Делегаты обработки исключений должны вызываться в начале конвейера, поэтому они могут перехватывать исключения, возникающие в более поздних стадиях конвейера.

Простейший возможных приложения ASP.NET Core устанавливает делегат одного запроса, который обрабатывает все запросы. Этот вариант не содержит конвейер самого запроса. Вместо этого один анонимная функция вызывается в ответ на каждый запрос HTTP.

[!code-csharp[Main](middleware/sample/Middleware/Startup.cs)]

Первый [приложения. Запустите](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) делегата завершает конвейера.

Можно соединить в цепочку несколько делегатов запроса вместе с [приложения. Используйте](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions). `next` Представляет параметр Далее делегата в конвейере. (Помните, что краткой записи продаж по *не* вызов *Далее* параметров.) До и после следующего делегата обычно можно выполнять действия, как показано в этом примере.

[!code-csharp[Main](middleware/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> Не вызывайте `next.Invoke` после отправки ответа клиенту. Изменения `HttpResponse` после запуска ответ будет создано исключение. Например изменения, такие как установка заголовки, код состояния, и т.д., возникает исключение. Запись в тексте ответа после вызова метода `next`:
> - Может вызвать нарушение протокола. Например, записи более чем указанных выше `content-length`.
> - Может привести к повреждению формат текста сообщения. Например запись нижний колонтитул в HTML, CSS-файл.
>
> [HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) полезно подсказка для указания, если отправлены заголовки и/или записан в тексте.

## <a name="ordering"></a>Упорядочение

Порядок, в котором компонентов по промежуточного слоя, добавляются в `Configure` метод определяет порядок, в котором они вызываются при запросах и обратный порядок для ответа. Этот порядок важен для безопасности, производительности и функциональности.

Метод конфигурации (см. ниже) добавляет следующие компоненты по промежуточного слоя:

1. Обработка исключений и ошибок
2. Статические файлового сервера
3. Проверка подлинности
4. MVC

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

В представленном выше коде `UseExceptionHandler` — это первый компонент по промежуточного слоя, добавить в конвейер, таким образом, он перехватывает все исключения, возникающие в последующих вызовах.

По промежуточного слоя статических файлов называется ранних этапах конвейера, чтобы он мог обрабатывать запросы и краткой записи, минуя остальных компонентов. Предоставляет по промежуточного слоя статических файлов **не** проверки авторизации. Все файлы, обслуживаемых, включая те, в разделе *wwwroot*, имеющихся в открытом доступе. В разделе [работа с файлами статического](xref:fundamentals/static-files) подход для защиты статических файлов.

Если запрос не обрабатывается по промежуточного слоя статических файлов, оно передается по промежуточного слоя идентификаторов (`app.UseIdentity`), который выполняет проверку подлинности. Удостоверение не сокращенный запросы без проверки подлинности. Несмотря на то, что запросы проверки подлинности удостоверения авторизации (и отклонения) возникает только в том случае, после MVC выбирает конкретного контроллера и действия.

В следующем примере показано по промежуточного слоя, упорядочение, где обрабатываются запросы статических файлов по промежуточного слоя статических файлов до сжатия ответа по промежуточного слоя. Статические файлы, не сжимаются с порядком, по промежуточного слоя. Отклики MVC [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) могут быть сжаты.

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name=middleware-run-map-use></a>

### <a name="use-run-and-map"></a>Использование, выполнения и карты

Настройка конвейера HTTP с помощью `Use`, `Run`, и `Map`. `Use` Метод может краткой записи конвейера (то есть, в том случае, если он не вызывает `next` делегат запроса). `Run`является правилом, и некоторые компоненты по промежуточного слоя может привести `Run[Middleware]` методы, которые выполняются в конце конвейера.

`Map*`расширения используются как соглашение для ветвления в конвейер. [Карта](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) ветвей конвейера запросов на основе совпадений для данного пути запроса. Если путь запроса начинается с заданного пути, выполняется ветвь.

[!code-csharp[Main](middleware/sample/Chain/StartupMap.cs?name=snippet1)]

Ниже приведены запросы и ответы от `http://localhost:1234` с помощью предыдущего кода:

| Запрос | Ответ |
| --- | --- |
| localhost:1234 | Здравствуйте, из карты-делегата.  |
| localhost:1234 / map1 | Тест карты 1 |
| localhost:1234 / map2 | Тест карты 2 |
| localhost:1234 / map3 | Здравствуйте, из карты-делегата.  |

Когда `Map` — используется, соответствующие пути сегментами, удаляются из `HttpRequest.Path` и присоединяется к `HttpRequest.PathBase` для каждого запроса.

[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) ветвей конвейера запросов на основе результата заданного предиката. Любой предикат типа `Func<HttpContext, bool>` может использоваться для сопоставления в новой ветви конвейера запросов. В следующем примере предикат используется для определения наличия переменной строки запроса `branch`:

[!code-csharp[Main](middleware/sample/Chain/StartupMapWhen.cs?name=snippet1)]

Ниже приведены запросы и ответы от `http://localhost:1234` с помощью предыдущего кода:

| Запрос | Ответ |
| --- | --- |
| localhost:1234 | Здравствуйте, из карты-делегата.  |
| localhost:1234 /? ветвь = master | Использовать ветви = master|

`Map`поддерживает вложение, например:

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

`Map`можно сопоставить несколько сегментов одновременно, например:

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a>Встроенные по промежуточного слоя

ASP.NET Core поставляется со следующими компонентами по промежуточного слоя:

| По промежуточного слоя | Описание |
| ----- | ------- |
| [Проверка подлинности](xref:security/authentication/identity) | Обеспечивает поддержку проверки подлинности. |
| [CORS](xref:security/cors) | Настраивает общий доступ к ресурсам независимо от источника. |
| [Кэширование ответов](xref:performance/caching/middleware) | Предоставляет поддержку для кэширования ответов. |
| [Сжатие ответа](xref:performance/response-compression) | Поддерживает сжатие ответов. |
| [Маршрутизация](xref:fundamentals/routing) | Определяет и ограничивает запрос маршрутов. |
| [Сеанс](xref:fundamentals/app-state) | Предоставляет поддержку для управления пользовательских сеансов. |
| [Статические файлы](xref:fundamentals/static-files) | Обеспечивает поддержку для обслуживания статических файлов и просмотр каталогов. |
| [По промежуточного слоя перезаписи URL-адрес](xref:fundamentals/url-rewriting) | Предоставляет поддержку для перезаписи URL-адресов и перенаправления запросов. |

<a name=middleware-writing-middleware></a>

## <a name="writing-middleware"></a>Записи по промежуточного слоя

По промежуточного слоя обычно, содержащийся в классе, которое предоставляется с помощью метода расширения. Рассмотрим следующий по промежуточного слоя, который задает язык и региональные параметры для текущего запроса из строки запроса.

[!code-csharp[Main](middleware/sample/Culture/StartupCulture.cs?name=snippet1)]

Примечание: Приведенном выше примере используется для демонстрации создания компонентов по промежуточного слоя. В разделе [ глобализации и локализации](xref:fundamentals/localization) для поддержки встроенных локализации ASP.NET Core.

Можно проверить по промежуточного слоя, передав в язык и региональные параметры, например `http://localhost:7997/?culture=no`.

Следующий код перемещает делегат по промежуточного слоя для класса:

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddleware.cs)]

Следующий метод расширения предоставляет по промежуточного слоя через [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

В следующем коде вызывается по промежуточного слоя из `Configure`:

[!code-csharp[Main](middleware/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

По промежуточного слоя, должны соответствовать [явные зависимости принцип](http://deviq.com/explicit-dependencies-principle/) путем предоставления доступа к его зависимости в своем конструкторе. По промежуточного слоя, создается один раз за *существования приложения*. В разделе *зависимости на запрос* ниже, если вам нужно службы совместно с по промежуточного слоя внутри запроса.

Устранить их зависимости от внедрения зависимостей через параметры конструктора компонентов по промежуточного слоя. [`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary)может также принимать дополнительные параметры напрямую.

### <a name="per-request-dependencies"></a>Зависимости на запрос

Так как по промежуточного слоя создается при запуске приложения, а не для каждого запроса, *область* время существования службы, используемые по промежуточного слоя конструкторы не являются общими с другими типами зависимостей введенные во время каждого запроса. Если необходимо предоставить *область* службы между по промежуточного слоя и другими типами, добавьте эти службы, чтобы `Invoke` сигнатуры метода. `Invoke` Метод может принимать дополнительные параметры, которые заполняются внедрения зависимостей. Пример:

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
* [Миграция модули HTTP с по промежуточного слоя](../migration/http-modules.md)
* [Запуск приложения](startup.md)
* [Параметры запроса](request-features.md)
