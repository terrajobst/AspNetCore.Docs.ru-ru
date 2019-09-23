---
title: Форматирование данных отклика в веб-API ASP.NET Core
author: ardalis
description: Сведения о форматировании данных отклика в веб-API ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 05/29/2019
uid: web-api/advanced/formatting
ms.openlocfilehash: 8bee4efdae5341ddab5bd3aec278ecfef37f0c08
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082349"
---
<!-- DO NOT EDIT BEFORE https://github.com/aspnet/AspNetCore.Docs/pull/12077 MERGES -->
# <a name="format-response-data-in-aspnet-core-web-api"></a>Форматирование данных отклика в веб-API ASP.NET Core

Автор: [Стив Смит](https://ardalis.com/) (Steve Smith)

ASP.NET Core MVC имеет встроенную поддержку для форматирования данных отклика с использованием фиксированных форматов или с учетом спецификаций клиента.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting/sample) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="format-specific-action-results"></a>Результаты действий для конкретного формата

Некоторые типы результатов действий характерны для определенного формата, например `JsonResult` и `ContentResult`. Действия могут возвращать определенные результаты, которые всегда форматируются определенным образом. Например, при возврате `JsonResult` независимо от параметров клиента возвращаются данные в формате JSON. Аналогичным образом, при возврате `ContentResult` возвращаются строковые данные в формате обычного текста (как и в случае возврата простой строки).

> [!NOTE]
> Действию не требуется возвращать какой-либо определенный тип; MVC поддерживает любое значение, возвращаемое объектом. Если действие возвращает реализацию `IActionResult` и контроллер наследует от `Controller`, разработчики имеют множество вспомогательных методов, соответствующих различным вариантам. Результаты из действий, возвращающих объекты, которые не являются типами `IActionResult`, будут сериализованы с помощью соответствующей реализации `IOutputFormatter`.

Чтобы возвратить данные в определенном формате из контроллера, наследующего от базового класса `Controller`, используйте встроенный вспомогательный метод `Json` для возврата JSON и `Content` для обычного текста. Метод действия должен возвращать конкретный тип результата (например, `JsonResult`) или `IActionResult`.

Возврат данных в формате JSON:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

Пример отклика из этого действия:

![Вкладка "Сеть" средств разработчика в Microsoft Edge, где в поле "Тип содержимого" для отклика указано значение application/json](formatting/_static/json-response.png)

Обратите внимание, что как в списке сетевых запросов, так и в разделе "Заголовки ответа" для отклика указан тип содержимого `application/json`. Кроме того, обратите внимание на список параметров, предоставленный браузером (в данном случае Microsoft Edge) в заголовке Accept в разделе "Заголовки запроса". Сейчас этот заголовок следует игнорировать, учитывая приведенные ниже данные о нем.

Чтобы возвратить данные в формате обычного текста, используйте `ContentResult` и вспомогательный метод `Content`:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

Отклик из этого действия:

![Вкладка "Сеть" средств разработчика в Microsoft Edge, где в поле "Тип содержимого" для отклика указано значение text/plain](formatting/_static/text-response.png)

Обратите внимание, что в этом случае возвращаемый `Content-Type` имеет значение `text/plain`. Аналогичного результата можно добиться с помощью одного только строкового типа отклика:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> Для нетривиальных действий с несколькими типами возвращаемого значения или возвращаемыми параметрами (например, различными кодами состояния HTTP в зависимости от выполненных операций) рекомендуется использовать `IActionResult` в качестве типа возвращаемого значения.

## <a name="content-negotiation"></a>Согласование содержимого

Согласование *содержимого* происходит, когда клиент задает [заголовок Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html). Для ASP.NET Core MVC по умолчанию используется формат JSON. Согласование содержимого реализуется `ObjectResult`. Оно также встроено в результаты действия с определенным кодом состояния, возвращаемые из вспомогательных методов (которые все основаны на `ObjectResult`). Вы также можете возвратить тип модели (класс, определенный в качестве типа передачи данных), при этом платформа автоматически использует для него оболочку `ObjectResult`.

Следующий метод действия использует вспомогательные методы `Ok` и `NotFound`:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

Если только не был запрошен иной формат и сервер способен возвратить данные в нем, возвращается отклик в формате JSON. Вы можете использовать такое средство, как [Fiddler](https://www.telerik.com/fiddler), чтобы создать запрос, содержащий заголовок Accept, и указать другой формат. В этом случае, если на сервере есть *модуль форматирования*, способный создать отклик в запрошенном формате, результат возвращается в формате, предпочитаемом клиентом.

![Консоль Fiddler, показывающая созданный вручную запрос GET, в котором заголовок Accept имеет значение application/xml](formatting/_static/fiddler-composer.png)

На предыдущем снимке экрана средство Fiddler Composer используется для создания запроса, для чего указано значение `Accept: application/xml`. По умолчанию ASP.NET Core MVC поддерживает только JSON, поэтому даже при указании другого формата возвращаемый результат все равно будет иметь формат JSON. Сведения о добавлении дополнительных модулей форматирования приведены в следующем разделе.

Действия контроллера могут возвращать объекты POCO, при этом ASP.NET Core MVC автоматически создает `ObjectResult`, используемый в качестве оболочки для такого объекта. Клиент получает отформатированный сериализованный объект (по умолчанию используется формат JSON, но можно настроить XML или другие форматы). Если возвращается объект `null`, платформа возвращает отклик `204 No Content`.

Возвращение типа объекта:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

В этом примере запрос допустимого псевдонима автора получает отклик "200 OK" с данными об авторе. Запрос недопустимого псевдонима получает отклик "204 Нет содержимого". Ниже приведены снимки экрана, показывающие отклик в форматах XML и JSON.

### <a name="content-negotiation-process"></a>Процесс согласования содержимого

*Согласование* содержимого выполняется только при наличии заголовка `Accept` в запросе. Если запрос содержит заголовок Accept, платформа перечисляет типы мультимедиа в заголовке Accept в порядке предпочтения и попытается найти модуль форматирования, способный создать отклик в одном из форматов, указанных в заголовке Accept. Если модуль форматирования, способный удовлетворить запрос клиента, не найден, платформа пытается найти первый модуль форматирования, способный создать отклик (если только разработчик не настроил параметр `MvcOptions` для возврата отклика "406 Неприемлемо"). Если в запросе указан XML, однако модуль форматирования XML не настроен, используется модуль форматирования JSON. В общем случае если модуль форматирования, обеспечивающий требуемый формат, не настроен, используется первый модуль форматирования, способный отформатировать данный объект. Если заголовок не указан, для сериализации отклика используется первый модуль форматирования, способный обработать возвращаемый объект. В этом случае никакое согласование не выполняется — используемый формат определяется сервером.

> [!NOTE]
> Если заголовок Accept содержит `*/*`, он игнорируется, если только `RespectBrowserAcceptHeader` не имеет значение true в `MvcOptions`.

### <a name="browsers-and-content-negotiation"></a>Браузеры и согласование содержимого

В отличие от типичных клиентов API веб-браузеры, как правило, предоставляют заголовки `Accept` с обширным набором форматов, включая подстановочные знаки. По умолчанию, когда платформа обнаруживает поступающий из браузера запрос, она игнорирует заголовок `Accept` и возвращает содержимое в формате по умолчанию, настроенном в приложении (JSON, если не указано иное). Это обеспечивает более согласованное взаимодействие при использовании различных браузеров для работы с API.

Если вы хотите, чтобы приложение учитывало заголовки Accept в браузере, это можно настроить в конфигурации MVC, задав для `RespectBrowserAcceptHeader` значение `true` в методе `ConfigureServices` в файле *Startup.cs*.

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a>Настройка модулей форматирования

Если приложение должно поддерживать дополнительные форматы, кроме стандартного JSON, можно добавить пакеты NuGet и настроить MVC для их поддержки. Существуют отдельные модули форматирования для ввода и вывода. Модули форматирования ввода используются [привязкой модели](xref:mvc/models/model-binding); модули форматирования вывода используются для форматирования откликов. Можно также настроить [пользовательские модули форматирования](xref:web-api/advanced/custom-formatters).

::: moniker range=">= aspnetcore-3.0"

### <a name="configure-systemtextjson-based-formatters"></a>Настройка форматировщиков на основе System.Text.Json

Функции для форматировщиков на основе `System.Text.Json` можно настроить с помощью `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions`.

```csharp
services.AddControllers().AddJsonOptions(options =>
{
    // Use the default property (Pascal) casing.
    options.SerializerOptions.PropertyNamingPolicy = null;

    // Configure a custom converter.
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

Параметры сериализации выходных данных для отдельных действий можно настроить с помощью `JsonResult`. Например:

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerOptions
    {
        options.WriteIndented = true,
    });
}
```

### <a name="add-newtonsoftjson-based-json-format-support"></a>Добавление поддержки формата JSON на основе Newtonsoft.Json

До выпуска версии ASP.NET Core 3.0 в MVC по умолчанию использовались форматировщики JSON, реализованные с помощью пакета `Newtonsoft.Json`. В ASP.NET Core 3.0 или более поздней версии форматировщики JSON по умолчанию основаны на `System.Text.Json`. Чтобы добавить поддержку форматировщиков и функций на основе `Newtonsoft.Json`, установите пакет NuGet [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) и настройте его в `Startup.ConfigureServices`.

```csharp
services.AddControllers()
    .AddNewtonsoftJson();
```

Возможно, некоторые функции не будут оптимально работать с форматировщиками на основе `System.Text.Json` и будут требовать ссылки на форматировщики на основе `Newtonsoft.Json` для выпуска ASP.NET Core 3.0. Продолжайте работать с форматировщиками на основе `Newtonsoft.Json`, если при использовании приложения ASP.NET Core 3.0 или более поздней версии:

* Применяются атрибуты `Newtonsoft.Json` (например, `[JsonProperty]` или `[JsonIgnore]`), настраиваются параметры сериализации или работа приложения основана на функциях, которые предоставляет `Newtonsoft.Json`.
* Настраивается `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`. В ASP.NET Core более ранних версий, чем версия 3.0, `JsonResult.SerializerSettings` принимает экземпляр `JsonSerializerSettings`, который относится к `Newtonsoft.Json`.
* Создается документация [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>).

Функции для форматировщиков на основе `Newtonsoft.Json` можно настроить с помощью `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`:

```csharp
services.AddControllers().AddNewtonsoftJson(options =>
{
    // Use the default property (Pascal) casing
    options.SerializerSettings.ContractResolver = new DefautlContractResolver();

    // Configure a custom converter
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

Параметры сериализации выходных данных для отдельных действий можно настроить с помощью `JsonResult`. Например:

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerSettings
    {
        options.Formatting = Formatting.Indented,
    });
}
```

::: moniker-end

### <a name="add-xml-format-support"></a>Добавление поддержки формата XML

::: moniker range="<= aspnetcore-2.2"

Чтобы включить поддержку форматирования XML в ASP.NET Core версии 2.2 или более ранней, установите пакет NuGet [Microsoft.AspNetCore.Mvc.Formatters.Xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/).

::: moniker-end

Форматировщики XML, реализованные с помощью `System.Xml.Serialization.XmlSerializer`, можно настроить путем вызова <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*> в `Startup.ConfigureServices`:

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

А форматировщики XML, реализованные с помощью `System.Runtime.Serialization.DataContractSerializer`, можно настроить путем вызова <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlDataContractSerializerFormatters*> в `Startup.ConfigureServices`:

```csharp
services.AddMvc()
    .AddXmlDataContractSerializerFormatters();
```

После добавления поддержки для форматирования XML методы контроллера должны возвращать формат, соответствующий заголовку `Accept` запроса, как показано в следующем примере Fiddler:

![Консоль Fiddler: на вкладке "Raw" (Необработанные) для запроса показано, что заголовок Accept имеет значение application/xml. На вкладке "Raw" (Необработанные) для запроса показано, что заголовок Content-Type имеет значение application/xml.](formatting/_static/xml-response.png)

На вкладке "Inspectors" (Инспекторы) видно, что необработанный запрос GET был выполнен с использованием набора заголовков `Accept: application/xml`. В области откликов отображается заголовок `Content-Type: application/xml`, а объект `Author` сериализуется в формат XML.

На вкладке "Composer" (Редактор) можно изменить запрос, указав `application/json` в заголовке `Accept`. Выполните запрос, при этом отклик будет иметь формат JSON:

![Консоль Fiddler: на вкладке "Raw" (Необработанные) для запроса показано, что заголовок Accept имеет значение application/json. На вкладке "Raw" (Необработанные) для запроса показано, что заголовок Content-Type имеет значение application/json.](formatting/_static/json-response-fiddler.png)

На этом снимке экрана видно, что запрос задает заголовок `Accept: application/json`, а отклик указывает то же значение в `Content-Type`. Объект `Author` отображается в тексте ответа в формате JSON.

### <a name="forcing-a-particular-format"></a>Принудительное использование определенного формата

Если вы хотите ограничить форматы отклика для конкретного действия, можно применить фильтр `[Produces]`. Фильтр `[Produces]` указывает форматы отклика для определенного действия (или контроллера). Как и большинство [фильтров](xref:mvc/controllers/filters), его можно применить к действию, контроллеру или глобальной области.

```csharp
[Produces("application/json")]
public class AuthorsController
```

Фильтр `[Produces]` принуждает все действия в `AuthorsController` возвращать отклики в формате JSON, даже если для приложения настроены другие модули форматирования, а клиент предоставляет заголовок `Accept`, запрашивающий другой доступный формат. Раздел [Фильтры](xref:mvc/controllers/filters) содержит дополнительные сведения, включая способы глобального применения фильтров.

### <a name="special-case-formatters"></a>Специальные модули форматирования

Встроенные модули форматирования реализуют некоторые специальные возможности. По умолчанию типы возвращаемых значений `string` форматируются как *text/plain* (*text/html*, если того требует заголовок `Accept`). Это поведение можно отключить, удалив `TextOutputFormatter`. Удалить модули форматирования можно в методе `Configure` в файле *Startup.cs* (см. ниже). Действия, у которых типом возвращаемого объекта является модель, возвращают отклик "204 Нет содержимого" при возврате `null`. Это поведение можно отключить, удалив `HttpNoContentOutputFormatter`. Приведенный ниже код удаляет `TextOutputFormatter` и `HttpNoContentOutputFormatter`.

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

Без `TextOutputFormatter` типы возвращаемых значений `string` возвращают, например, отклик "406 Неприемлемо". Обратите внимание, что если модуль форматирования XML существует, он форматирует типы возвращаемых значений `string`, когда `TextOutputFormatter` удален.

Без `HttpNoContentOutputFormatter` объекты со значением null форматируются с помощью настроенного модуля форматирования. Например, модуль форматирования JSON просто возвращает отклик с текстом `null`, а модуль форматирования XML — пустой элемент XML с заданным атрибутом `xsi:nil="true"`.

## <a name="response-format-url-mappings"></a>Сопоставления URL-адреса для формата отклика

Клиенты могут запрашивать определенный формат в URL-адресе, например в строке запроса или части пути, либо используя расширение файла определенного формата, такое как XML или JSON. Сопоставление из пути запроса должно быть указано в маршруте, используемом API. Например:

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

Этот маршрут позволяет задать запрошенный формат в качестве дополнительного расширения файла. Атрибут `[FormatFilter]` проверяет наличие значения формата в `RouteData` и сопоставляет этот формат отклика с соответствующим модулем форматирования при создании отклика.

|           Маршрут            |             Formatter              |
|----------------------------|------------------------------------|
|   `/products/GetById/5`    |    Модуль форматирования вывода по умолчанию    |
| `/products/GetById/5.json` | Модуль форматирования JSON (если настроен) |
| `/products/GetById/5.xml`  | Модуль форматирования XML (если настроен)  |
