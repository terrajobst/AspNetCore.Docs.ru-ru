---
title: Соглашения для маршрутов и приложений Razor Pages в ASP.NET Core
author: guardrex
description: Узнайте, как использовать соглашения поставщика модели маршрутов и приложений для управления маршрутизацией, обнаружением и обработкой страниц.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/08/2019
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: c93f169c422d260f738faba4812861521f383e51
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/09/2019
ms.locfileid: "68914973"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a>Соглашения для маршрутов и приложений Razor Pages в ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

Узнайте, как использовать [соглашения поставщика модели маршрутов и приложений](xref:mvc/controllers/application-model#conventions) страниц для управления маршрутизацией, обнаружением и обработкой страниц в приложениях Razor Pages.

Когда вам необходимо настроить уникальные маршруты для отдельных страниц, используйте [соглашение AddPageRoute](#configure-a-page-route), описанное далее в этом разделе.

Чтобы указать маршрут страницы, добавить сегменты маршрута или добавить параметры в маршрут, используйте `@page` директиву страницы. Дополнительные сведения см. в разделе [Custom Routes](xref:razor-pages/index#custom-routes).

Существуют зарезервированные слова, которые нельзя использовать в качестве сегментов маршрутов или имен параметров. Дополнительные сведения см. в [разделе Маршрутизация: Зарезервированные](xref:fundamentals/routing#reserved-routing-names)имена маршрутов.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([как скачивать](xref:index#how-to-download-a-sample))

| Сценарий | Что демонстрирует пример |
| -------- | --------------------------- |
| [Соглашения для моделей](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li><li>IPageHandlerModelConvention</li></ul> | Добавление шаблона маршрута и заголовка к страницам приложения. |
| [Соглашения для действий с маршрутами страниц](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Добавление шаблона маршрута к страницам в папке и к одной странице. |
| [Соглашения для действий модели страниц](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (класс фильтра, лямбда-выражение или фабрика фильтров)</li></ul> | Добавление заголовка к страницам в папке, добавление заголовка к одной странице и настройка [фабрики фильтров](xref:mvc/controllers/filters#ifilterfactory) для добавления заголовка к страницам приложения. |

Razor Pages соглашения добавляются и настраиваются с <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> помощью <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> метода расширения в коллекции служб в `Startup` классе. Следующие примеры соглашений описаны далее в этом разделе:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
            {
                options.Conventions.Add( ... );
                options.Conventions.AddFolderRouteModelConvention(
                    "/OtherPages", model => { ... });
                options.Conventions.AddPageRouteModelConvention(
                    "/About", model => { ... });
                options.Conventions.AddPageRoute(
                    "/Contact", "TheContactPage/{text?}");
                options.Conventions.AddFolderApplicationModelConvention(
                    "/OtherPages", model => { ... });
                options.Conventions.AddPageApplicationModelConvention(
                    "/About", model => { ... });
                options.Conventions.ConfigureFilter(model => { ... });
                options.Conventions.ConfigureFilter( ... );
            });
}
```

## <a name="route-order"></a>Порядок маршрутов

Маршруты указывают <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> для обработки (сопоставление маршрутов).

| Номер            | Поведение |
| :--------------: | -------- |
| -1               | Маршрут обрабатывается до обработки других маршрутов. |
| 0                | Порядок не указан (значение по умолчанию). Не назначение `Order` (`Order = null`) по умолчанию присваивает `Order` маршруту значение 0 (нуль) для обработки. |
| 1, 2, &hellip; n | Указывает порядок обработки маршрутов. |

Обработка маршрутов устанавливается по соглашению:

* Маршруты обрабатываются в последовательном порядке (-1, 0, 1 &hellip; , 2, n).
* Если маршруты `Order`совпадают, сначала сопоставляются наиболее конкретный маршрут, за которым следуют менее конкретные маршруты.
* Если маршруты с `Order` одинаковым и одинаковым числом параметров соответствуют URL-адресу запроса, маршруты обрабатываются в том порядке, в котором <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>они добавляются в.

По возможности старайтесь не зависеть от установленного порядка обработки маршрутов. Как правило, маршрутизация выбирает правильный маршрут с сопоставлением URL-адресов. Если необходимо задать свойства маршрута `Order` для правильной маршрутизации запросов, схема маршрутизации приложения, скорее всего, выпутать клиентов и не будет поддерживать поддержку. Поиск упрощает схему маршрутизации приложения. Для примера приложения требуется явный порядок обработки маршрута, чтобы продемонстрировать несколько сценариев маршрутизации, использующих одно приложение. Однако следует попытаться избежать практической ситуации при настройке маршрута `Order` в рабочих приложениях.

Средства маршрутизации в Razor Pages и контроллере MVC имеют общую реализацию. Сведения о порядке маршрутов см. в разделе [маршрутизация к действиям контроллера: Упорядочивание маршрутов](xref:mvc/controllers/routing#ordering-attribute-routes)атрибутов.

## <a name="model-conventions"></a>Соглашения для моделей

Добавьте делегат для <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> , чтобы добавить [соглашения модели](xref:mvc/controllers/application-model#conventions) , применяемые к Razor Pages.

### <a name="add-a-route-model-convention-to-all-pages"></a>Добавление соглашения о модели маршрута ко всем страницам

Используйте <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> для создания и <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> добавления в коллекцию <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> экземпляров, которые применяются во время построения модели маршрутизации страниц.

В примере приложения шаблон маршрута `{globalTemplate?}` добавляется ко всем страницам приложения:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

::: moniker-end

Свойству <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> для <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> задано значение `1`. Это обеспечивает следующее поведение сопоставления маршрутов в примере приложения:

* Шаблон маршрута для `TheContactPage/{text?}` добавляется далее в разделе. Маршрут страницы контакта имеет порядок `null` по умолчанию (`Order = 0`), `{globalTemplate?}` поэтому он соответствует шаблону маршрута.
* Шаблон `{aboutTemplate?}` маршрута добавляется далее в разделе. Шаблону `{aboutTemplate?}` задано свойство `Order`, равное `2`. При запросе страницы About по адресу `/About/RouteDataValue` значение RouteDataValue загружается в `RouteData.Values["globalTemplate"]` (`Order = 1`), а не в `RouteData.Values["aboutTemplate"]` (`Order = 2`). Это происходит из-за значения свойства `Order`.
* Шаблон `{otherPagesTemplate?}` маршрута добавляется далее в разделе. Шаблону `{otherPagesTemplate?}` задано свойство `Order`, равное `2`. Когда любая страница в папке *pages/OtherPages* запрашивается с помощью параметра Route (например, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" загружается `RouteData.Values["globalTemplate"]` в`Order = 1`() и `RouteData.Values["otherPagesTemplate"]` не`Order = 2`() из-за установки свойства `Order` свойство.

Везде, где это возможно, `Order`не устанавливайте, `Order = 0`что приводит к. Чтобы выбрать правильный маршрут, используйте маршрутизацию.

Параметры Razor Pages, такие как добавление <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, добавляются при добавлении MVC в коллекцию служб в. `Startup.ConfigureServices` Пример см. в [образце приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

::: moniker-end

Запросите страницу About по адресу `localhost:5000/About/GlobalRouteValue` из примера и проверьте результат:

![Запрос страницы About с сегментом маршрута GlobalRouteValue. Отображенная страница показывает, что значение данных маршрута получено в методе OnGet страницы.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a>Добавление соглашения о модели приложений ко всем страницам

Используйте <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> для создания и <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> добавления в коллекцию <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> экземпляров, которые применяются во время построения модели приложения страницы.

Для демонстрации этих и других соглашений далее в этом разделе в пример приложения включен класс `AddHeaderAttribute`. Конструктор класса принимает строку `name` и массив строк `values`. Эти значения используются в его методе `OnResultExecuting` для задания заголовка ответа. Класс показан полностью в разделе [Соглашения для действий модели страниц](#page-model-action-conventions) далее.

Пример приложения использует класс `AddHeaderAttribute`, чтобы добавить заголовок `GlobalHeader` ко всем страницам в приложении:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

::: moniker-end

*Startup.cs*:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

::: moniker-end

Запросите страницу About по адресу `localhost:5000/About` из примера и посмотрите заголовки, чтобы проверить результат:

![Заголовки ответа страницы About, показывающие, что добавлен заголовок GlobalHeader.](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a>Добавление соглашения модели обработчиков ко всем страницам

Используйте <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> для создания и <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> добавления в коллекцию <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> экземпляров, которые применяются во время построения модели обработчика страницы.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

::: moniker-end

*Startup.cs*:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

::: moniker-end

## <a name="page-route-action-conventions"></a>Соглашения для действий с маршрутами страниц

Поставщик модели маршрута по умолчанию, производный <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> от, вызывает соглашения, которые предназначены для предоставления точек расширения для настройки маршрутов страниц.

### <a name="folder-route-model-convention"></a>Соглашение о модели маршрута папки

Используйте <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> для создания и добавления объекта <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> , который вызывает действие <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> для всех страниц в указанной папке.

Пример приложения использует <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> для добавления шаблона маршрута `{otherPagesTemplate?}` к страницам в папке *OtherPages*:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

::: moniker-end

Свойству <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> для <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> задано значение `2`. Это гарантирует, что шаблону `{globalTemplate?}` (установленному ранее в `1`разделе) присваивается приоритет для первой координаты значения данных маршрута, если указано одно значение маршрута. Если страница в папке *pages/OtherPages* запрашивается с помощью значения параметра маршрута (например `/OtherPages/Page1/RouteDataValue`,), то "RouteDataValue" загружается в`Order = 1` `RouteData.Values["globalTemplate"]` () и `RouteData.Values["otherPagesTemplate"]` не`Order = 2`() из-за установки свойства `Order` свойство.

Везде, где это возможно, `Order`не устанавливайте, `Order = 0`что приводит к. Чтобы выбрать правильный маршрут, используйте маршрутизацию.

Запросите страницу Page1 по адресу `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` из примера и проверьте результат:

![Страница Page1 в папке OtherPages запрашивается с сегментом маршрута GlobalRouteValue и OtherPagesRouteValue. Отображенная страница показывает, что значения данных маршрута получены в методе OnGet страницы.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a>Соглашение о модели маршрута страницы

Используйте <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> для создания и добавления объекта <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> , который вызывает действие <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> в для страницы с указанным именем.

Пример приложения использует `AddPageRouteModelConvention` для добавления шаблона маршрута `{aboutTemplate?}` к странице About:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

::: moniker-end

Свойству <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> для <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> задано значение `2`. Это гарантирует, что шаблону `{globalTemplate?}` (установленному ранее в `1`разделе) присваивается приоритет для первой координаты значения данных маршрута, если указано одно значение маршрута. Если страница `/About/RouteDataValue`about запрашивается со значением параметра маршрута, то "RouteDataValue" загружается в `RouteData.Values["globalTemplate"]` (`Order = 1`) `Order` и не `RouteData.Values["aboutTemplate"]` (`Order = 2`) из-за задания свойства.

Везде, где это возможно, `Order`не устанавливайте, `Order = 0`что приводит к. Чтобы выбрать правильный маршрут, используйте маршрутизацию.

Запросите страницу About по адресу `localhost:5000/About/GlobalRouteValue/AboutRouteValue` из примера и проверьте результат:

![Страница About запрашивается с сегментами маршрута для GlobalRouteValue и AboutRouteValue. Отображенная страница показывает, что значения данных маршрута получены в методе OnGet страницы.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

::: moniker range=">= aspnetcore-2.2"

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a>Использование преобразователя параметров для настройки маршрутов страниц

Маршруты страниц, созданные ASP.NET Core, можно настроить с помощью преобразователя параметров. Преобразователь параметров реализует `IOutboundParameterTransformer` и преобразует значения параметров. Например, пользовательский преобразователь параметра `SlugifyParameterTransformer` изменяет значение маршрута `SubscriptionManagement` на `subscription-management`.

Соглашение о модели маршрута страницыприменяетпреобразовательпараметровкименампапокиименфайловавтоматическисоздаваемыхмаршрутовстраницвприложении.`PageRouteTransformerConvention` Например, файл Razor Pages в */Пажес/субскриптионманажемент/виевалл.кштмл* будет иметь перезапись маршрута из `/SubscriptionManagement/ViewAll` в. `/subscription-management/view-all`

`PageRouteTransformerConvention`преобразует только автоматически создаваемые сегменты маршрута страницы, полученные из папки Razor Pages и имени файла. Он не преобразует сегменты маршрута, добавленные `@page` с помощью директивы. Это соглашение также не преобразует маршруты, <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>добавленные.

Регистрируется в качестве параметра в `Startup.ConfigureServices`: `PageRouteTransformerConvention`

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
            {
                options.Conventions.Add(
                    new PageRouteTransformerConvention(
                        new SlugifyParameterTransformer()));
            });
}

public class SlugifyParameterTransformer : IOutboundParameterTransformer
{
    public string TransformOutbound(object value)
    {
        if (value == null) { return null; }

        // Slugify value
        return Regex.Replace(value.ToString(), "([a-z])([A-Z])", "$1-$2").ToLower();
    }
}
```

::: moniker-end

## <a name="configure-a-page-route"></a>Настройка маршрута страницы

Используется <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> для настройки маршрута к странице по указанному пути страницы. Созданные ссылки на страницу будут использовать заданный вами маршрут. Для установления маршрута метод `AddPageRoute` использует `AddPageRouteModelConvention`.

Пример приложения создает маршрут к `/TheContactPage` для файла *Contact.cshtml*:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

::: moniker-end

На страницу Contact также можно перейти с помощью `/Contact` через маршрут по умолчанию.

Настраиваемый маршрут на страницу Contact в примере приложения позволяет использовать необязательный сегмент маршрута `text` (`{text?}`). Страница также включает этот необязательный сегмент в своей директиве `@page` на случай, если посетитель переходит на страницу по маршруту `/Contact`:

::: moniker range=">= aspnetcore-3.0"

[!code-cshtml[](razor-pages-conventions/samples/3.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

::: moniker-end

Обратите внимание, что URL-адрес, созданный для ссылки **Contact** на отображенной странице, отражает обновленный маршрут:

![Ссылка Contact на панели навигации в примере приложения](razor-pages-conventions/_static/contact-link.png)

![В ссылке Contact в показанном HTML видно, что href имеет значение "/TheContactPage".](razor-pages-conventions/_static/contact-link-source.png)

Перейдите на страницу Contact по ее обычному маршруту `/Contact` или по настроенному маршруту `/TheContactPage`. Если вы укажете дополнительный сегмент маршрута `text`, страница отобразит этот сегмент в HTML-кодировке:

![Пример в браузере Microsoft Edge с указанием необязательного сегмента маршрута "text" со значением "TextValue" в URL-адресе. Отображенная страница показывает значение сегмента "text".](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>Соглашения для действий модели страниц

Поставщик модели страницы по умолчанию, <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> реализующий соглашения о вызовах, которые предназначены для предоставления точек расширения для настройки моделей страниц. Эти соглашения полезны при создании и изменении для обнаружения и обработки страниц.

В примерах, приведенных в этом разделе, в примере `AddHeaderAttribute` приложения используется класс <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, который применяет заголовок ответа:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

::: moniker-end

Образец показывает использование соглашений для применения атрибута ко всем страницам в папке и к одной странице.

**Соглашение для модели приложений папки**

Используйте <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> для создания и добавления объекта <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> , который <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> вызывает действие для экземпляров для всех страниц в указанной папке.

Пример демонстрирует использование `AddFolderApplicationModelConvention` путем добавления заголовка `OtherPagesHeader` к страницам в папке *OtherPages* приложения:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

::: moniker-end

Запросите страницу Page1 по адресу `localhost:5000/OtherPages/Page1` из примера и посмотрите заголовки, чтобы проверить результат:

![Заголовки ответа страницы OtherPages/Page1, показывающие добавленный заголовок OtherPagesHeader.](razor-pages-conventions/_static/page1-otherpages-header.png)

**Соглашение для модели приложений страницы**

Используйте <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> для создания и добавления объекта <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> , который вызывает действие <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> в для страницы с указанным именем.

Пример демонстрирует использование `AddPageApplicationModelConvention` путем добавления заголовка `AboutHeader` на страницу About:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

::: moniker-end

Запросите страницу About по адресу `localhost:5000/About` из примера и посмотрите заголовки, чтобы проверить результат:

![Заголовки ответа страницы About, показывающие, что добавлен заголовок AboutHeader.](razor-pages-conventions/_static/about-page-about-header.png)

**Настройка фильтра**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>настраивает указанный фильтр для применения. Вы можете создать класс фильтра, но пример приложения показывает реализацию фильтра в лямбда-выражении, которое реализовано "за кулисами" в качестве фабрики, возвращающей фильтр:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet8)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

::: moniker-end

Модель приложений страницы используется для проверки относительного пути на наличие сегментов, ведущих к странице Page2 в папке *OtherPages*. Если условие выполняется, добавляется заголовок. Если нет, применяется `EmptyFilter`.

`EmptyFilter` является [фильтром действий](xref:mvc/controllers/filters#action-filters). Так как фильтры действий игнорируются Razor Pages, параметр `EmptyFilter` не действует так, как предполагалось, если `OtherPages/Page2`путь не содержит.

Запросите страницу Page2 по адресу `localhost:5000/OtherPages/Page2` из примера и посмотрите заголовки, чтобы проверить результат:

![В ответ для страницы Page2 добавлен заголовок OtherPagesPage2Header.](razor-pages-conventions/_static/page2-filter-header.png)

**Настройка фабрики фильтров**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>настраивает указанную фабрику для применения [фильтров](xref:mvc/controllers/filters) ко всем Razor Pages.

В примере приложения приведен образец использования [фабрики фильтров](xref:mvc/controllers/filters#ifilterfactory) путем добавления на страницы приложения заголовка `FilterFactoryHeader` с двумя значениями:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet9)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

::: moniker-end

*AddHeaderWithFactory.cs*:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

::: moniker-end

Запросите страницу About по адресу `localhost:5000/About` из примера и посмотрите заголовки, чтобы проверить результат:

![Заголовки ответов на странице About показывают два добавленных заголовка FilterFactoryHeader.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>Фильтры MVC и фильтр страниц (IPageFilter)

[Фильтры действий](xref:mvc/controllers/filters#action-filters) MVC игнорируются в Razor Pages, так как Razor Pages использует методы обработчиков. Другие типы фильтров MVC доступны для использования: [Авторизация](xref:mvc/controllers/filters#authorization-filters), [исключение](xref:mvc/controllers/filters#exception-filters), [ресурс](xref:mvc/controllers/filters#resource-filters)и [результат](xref:mvc/controllers/filters#result-filters). Дополнительные сведения см. в разделе [Фильтры](xref:mvc/controllers/filters).

Фильтр страницы (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) — это фильтр, который применяется к Razor Pages. Дополнительные сведения см. в разделе [Методы фильтрации для Razor Pages](xref:razor-pages/filter).

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>
