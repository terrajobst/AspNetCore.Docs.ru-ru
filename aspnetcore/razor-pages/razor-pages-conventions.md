---
title: Соглашения для маршрутов и приложений Razor Pages в ASP.NET Core
author: guardrex
description: Узнайте, как использовать соглашения поставщика модели маршрутов и приложений для управления маршрутизацией, обнаружением и обработкой страниц.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/07/2019
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: 4e07b5803adbce94982584212fa65afbfd427b64
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64893511"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a>Соглашения для маршрутов и приложений Razor Pages в ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

Узнайте, как использовать [соглашения поставщика модели маршрутов и приложений](xref:mvc/controllers/application-model#conventions) страниц для управления маршрутизацией, обнаружением и обработкой страниц в приложениях Razor Pages.

Когда вам необходимо настроить уникальные маршруты для отдельных страниц, используйте [соглашение AddPageRoute](#configure-a-page-route), описанное далее в этом разделе.

Чтобы указать маршрута страницы, добавление сегментами маршрута или добавить параметры для маршрута, используйте страницы `@page` директива. Дополнительные сведения см. в разделе [пользовательские маршруты](xref:razor-pages/index#custom-routes).

Существуют зарезервированные слова, которые нельзя использовать в качестве сегментами маршрута или имена параметров. Дополнительные сведения см. в разделе [маршрутизации: Зарезервированные имена маршрутизации](xref:fundamentals/routing#reserved-routing-names).

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([как скачивать](xref:index#how-to-download-a-sample))

| Сценарий | Что демонстрирует пример |
| -------- | --------------------------- |
| [Соглашения для моделей](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li><li>IPageHandlerModelConvention</li></ul> | Добавление шаблона маршрута и заголовка к страницам приложения. |
| [Соглашения для действий с маршрутами страниц](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Добавление шаблона маршрута к страницам в папке и к одной странице. |
| [Соглашения для действий модели страниц](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (класс фильтра, лямбда-выражение или фабрика фильтров)</li></ul> | Добавление заголовка к страницам в папке, добавление заголовка к одной странице и настройка [фабрики фильтров](xref:mvc/controllers/filters#ifilterfactory) для добавления заголовка к страницам приложения. |

Соглашения о страницах Razor добавляются и настроить с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> метод расширения для <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> в сбор службы `Startup` класса. Следующие примеры соглашений описаны далее в этом разделе:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
            {
                options.Conventions.Add( ... );
                options.Conventions.AddFolderRouteModelConvention("/OtherPages", model => { ... });
                options.Conventions.AddPageRouteModelConvention("/About", model => { ... });
                options.Conventions.AddPageRoute("/Contact", "TheContactPage/{text?}");
                options.Conventions.AddFolderApplicationModelConvention("/OtherPages", model => { ... });
                options.Conventions.AddPageApplicationModelConvention("/About", model => { ... });
                options.Conventions.ConfigureFilter(model => { ... });
                options.Conventions.ConfigureFilter( ... );
            });
}
```

## <a name="route-order"></a>Порядок маршрута

Маршруты указывают <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> для обработки (сопоставление маршрутов).

| Номер            | Поведение |
| :--------------: | -------- |
| -1               | Маршрут обрабатывается перед обработкой других маршрутов. |
| 0                | Порядок не указан (значение по умолчанию). Не назначая `Order` (`Order = null`) по умолчанию маршрут `Order` 0 (ноль) для обработки. |
| 1, 2, &hellip; n | Указывает порядок обработки маршрута. |

Обработка маршрутов устанавливается в соответствии с соглашением:

* Маршруты обрабатываются в последовательном порядке (-1, 0, 1, 2, &hellip; n).
* Когда маршруты имеют одинаковые `Order`, наиболее конкретный маршрут соответствует после менее определенных маршрутов.
* Когда маршруты с тем же `Order` и URL-адрес запроса соответствует одинаковое число параметров, маршруты обрабатываются в порядке, в котором они добавляются к <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.

По возможности избегайте в зависимости от установленного маршрута обработки заказа. Как правило Маршрутизация выбирает соответствующий маршрут с соответствующими URL-адрес. Если необходимо задать маршрут `Order` свойства для маршрутизации запросов правильно, маршрутизации схему приложения, вероятно, заблуждение клиентов и уязвимости для поддержания. Поиск для упрощения маршрутизации схемы приложения. Пример приложения требуется явный маршрут обработки заказа, чтобы продемонстрировать несколько сценариев маршрутизации, с помощью одного приложения. Тем не менее, стоит пытаться избежать практика параметр маршрута `Order` в рабочих приложениях.

Средства маршрутизации в Razor Pages и контроллере MVC имеют общую реализацию. Информация на порядок маршрута в разделах MVC доступна на [Маршрутизация к действиям контроллера: Упорядочение маршрутов на основе атрибутов](xref:mvc/controllers/routing#ordering-attribute-routes).

## <a name="model-conventions"></a>Соглашения для моделей

Добавьте делегат для <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> добавление [модели соглашения](xref:mvc/controllers/application-model#conventions) , применяемые к Razor Pages.

### <a name="add-a-route-model-convention-to-all-pages"></a>Добавление соглашения для модели маршрутов ко всем страницам

Используйте <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> для создания и добавления <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> в коллекцию <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> экземпляров, которые применяются при маршрута страницы модели конструкции.

В примере приложения шаблон маршрута `{globalTemplate?}` добавляется ко всем страницам приложения:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

Свойству <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> для <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> задано значение `1`. Это гарантирует следующий маршрут, соответствующий поведения в примере приложения:

* Шаблон маршрута для `TheContactPage/{text?}` добавляется в разделе ниже. Обратитесь к странице маршрут имеет порядок по умолчанию `null` (`Order = 0`), чтобы он соответствовал перед `{globalTemplate?}` шаблон маршрута.
* `{aboutTemplate?}` Далее в этом разделе добавляется шаблон маршрута. Шаблону `{aboutTemplate?}` задано свойство `Order`, равное `2`. При запросе страницы About по адресу `/About/RouteDataValue` значение RouteDataValue загружается в `RouteData.Values["globalTemplate"]` (`Order = 1`), а не в `RouteData.Values["aboutTemplate"]` (`Order = 2`). Это происходит из-за значения свойства `Order`.
* `{otherPagesTemplate?}` Далее в этом разделе добавляется шаблон маршрута. Шаблону `{otherPagesTemplate?}` задано свойство `Order`, равное `2`. При любой страницы в *страниц/OtherPages* папку запрашивается с параметром маршрута (например, `/OtherPages/Page1/RouteDataValue`), «RouteDataValue» загружается в `RouteData.Values["globalTemplate"]` (`Order = 1`) и не `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) из-за параметра `Order` свойство.

Везде, где это возможно, не устанавливайте `Order`, что приводит к `Order = 0`. Используют маршрутов, чтобы выбрать правильный маршрут.

Параметры страницы Razor, такие как добавление <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, добавляются при добавлении в коллекцию служб в MVC `Startup.ConfigureServices`. Пример см. в [образце приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

Запросите страницу About по адресу `localhost:5000/About/GlobalRouteValue` из примера и проверьте результат:

![Запрос страницы About с сегментом маршрута GlobalRouteValue. Отображенная страница показывает, что значение данных маршрута получено в методе OnGet страницы.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a>Добавить соглашение для модели приложений ко всем страницам

Используйте <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> для создания и добавления <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> в коллекцию <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> экземпляров, которые применяются при странице конструкции модели приложения.

Для демонстрации этих и других соглашений далее в этом разделе в пример приложения включен класс `AddHeaderAttribute`. Конструктор класса принимает строку `name` и массив строк `values`. Эти значения используются в его методе `OnResultExecuting` для задания заголовка ответа. Класс показан полностью в разделе [Соглашения для действий модели страниц](#page-model-action-conventions) далее.

Пример приложения использует класс `AddHeaderAttribute`, чтобы добавить заголовок `GlobalHeader` ко всем страницам в приложении:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

Запросите страницу About по адресу `localhost:5000/About` из примера и посмотрите заголовки, чтобы проверить результат:

![Заголовки ответа страницы About, показывающие, что добавлен заголовок GlobalHeader.](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a>Добавление обработчика модели соглашение ко всем страницам

Используйте <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> для создания и добавления <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> в коллекцию <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> экземпляров, которые применяются при странице обработчик построения модели.

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a>Соглашения для действий с маршрутами страниц

Поставщик модели маршрутов по умолчанию, который является производным от <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> вызывает соглашения, которые предназначены для предоставления точек расширения для настройки маршрутов страниц.

### <a name="folder-route-model-convention"></a>Соглашение для модели маршрутов папки

Используйте <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> для создания и добавления <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> , вызывающий действие <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> для всех страниц в указанной папке.

Пример приложения использует <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> для добавления шаблона маршрута `{otherPagesTemplate?}` к страницам в папке *OtherPages*:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

Свойству <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> для <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> задано значение `2`. Это гарантирует, что шаблон для `{globalTemplate?}` (ранее в этом разделе, чтобы задать `1`) имеет преимущество — для первого значение данных маршрута при указании одного значения маршрута предоставляется. Если страница в *страниц/OtherPages* папку запрашивается со значением параметра маршрута (например, `/OtherPages/Page1/RouteDataValue`), «RouteDataValue» загружается в `RouteData.Values["globalTemplate"]` (`Order = 1`) и не `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) из-за параметра `Order` свойство.

Везде, где это возможно, не устанавливайте `Order`, что приводит к `Order = 0`. Используют маршрутов, чтобы выбрать правильный маршрут.

Запросите страницу Page1 по адресу `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` из примера и проверьте результат:

![Страница Page1 в папке OtherPages запрашивается с сегментом маршрута GlobalRouteValue и OtherPagesRouteValue. Отображенная страница показывает, что значения данных маршрута получены в методе OnGet страницы.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a>Соглашение для модели маршрутов страницы

Используйте <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> для создания и добавления <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> , вызывающий действие <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> для страницы с указанным именем.

Пример приложения использует `AddPageRouteModelConvention` для добавления шаблона маршрута `{aboutTemplate?}` к странице About:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

Свойству <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> для <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> задано значение `2`. Это гарантирует, что шаблон для `{globalTemplate?}` (ранее в этом разделе, чтобы задать `1`) имеет преимущество — для первого значение данных маршрута при указании одного значения маршрута предоставляется. При запросе страницы About со значением параметра маршрута в `/About/RouteDataValue`, «RouteDataValue» загружается в `RouteData.Values["globalTemplate"]` (`Order = 1`) и не `RouteData.Values["aboutTemplate"]` (`Order = 2`) из-за параметра `Order` свойство.

Везде, где это возможно, не устанавливайте `Order`, что приводит к `Order = 0`. Используют маршрутов, чтобы выбрать правильный маршрут.

Запросите страницу About по адресу `localhost:5000/About/GlobalRouteValue/AboutRouteValue` из примера и проверьте результат:

![Страница About запрашивается с сегментами маршрута для GlobalRouteValue и AboutRouteValue. Отображенная страница показывает, что значения данных маршрута получены в методе OnGet страницы.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

::: moniker range=">= aspnetcore-2.2"

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a>Использовать параметр преобразователя для настройки маршрутов страниц

Страница маршруты, созданный ASP.NET Core можно настроить, используя параметр преобразователя. Преобразователь параметров реализует `IOutboundParameterTransformer` и преобразует значения параметров. Например, пользовательский преобразователь параметра `SlugifyParameterTransformer` изменяет значение маршрута `SubscriptionManagement` на `subscription-management`.

`PageRouteTransformerConvention` Соглашение для модели маршрутов страницы применяется параметр преобразователя от сегментов имени файлов и папок автоматически созданные маршруты в приложении. Например, файл страницы Razor Pages в */Pages/SubscriptionManagement/ViewAll.cshtml* бы маршрута из переписать `/SubscriptionManagement/ViewAll` для `/subscription-management/view-all`.

`PageRouteTransformerConvention` только преобразует автоматически созданный сегменты маршрута страницы, полученные из папки Razor Pages и имя файла. Он не преобразует сегментами маршрута, добавленные с помощью `@page` директива. Соглашение также не преобразует маршруты, добавленные с <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.

`PageRouteTransformerConvention` Зарегистрирован в качестве параметра в `Startup.ConfigureServices`:

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

Используйте <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> для настройки маршрута на страницу по указанному пути страницы. Созданные ссылки на страницу будут использовать заданный вами маршрут. Для установления маршрута метод `AddPageRoute` использует `AddPageRouteModelConvention`.

Пример приложения создает маршрут к `/TheContactPage` для файла *Contact.cshtml*:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

На страницу Contact также можно перейти с помощью `/Contact` через маршрут по умолчанию.

Настраиваемый маршрут на страницу Contact в примере приложения позволяет использовать необязательный сегмент маршрута `text` (`{text?}`). Страница также включает этот необязательный сегмент в своей директиве `@page` на случай, если посетитель переходит на страницу по маршруту `/Contact`:

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

Обратите внимание, что URL-адрес, созданный для ссылки **Contact** на отображенной странице, отражает обновленный маршрут:

![Ссылка Contact на панели навигации в примере приложения](razor-pages-conventions/_static/contact-link.png)

![В ссылке Contact в показанном HTML видно, что href имеет значение "/TheContactPage".](razor-pages-conventions/_static/contact-link-source.png)

Перейдите на страницу Contact по ее обычному маршруту `/Contact` или по настроенному маршруту `/TheContactPage`. Если вы укажете дополнительный сегмент маршрута `text`, страница отобразит этот сегмент в HTML-кодировке:

![Пример в браузере Microsoft Edge с указанием необязательного сегмента маршрута "text" со значением "TextValue" в URL-адресе. Отображенная страница показывает значение сегмента "text".](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>Соглашения для действий модели страниц

Поставщик модели страниц по умолчанию, который реализует <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> вызывает соглашения, которые предназначены для предоставления точек расширения для настройки моделей страниц. Эти соглашения полезны при создании и изменении для обнаружения и обработки страниц.

Примеры в этом разделе, пример приложения использует `AddHeaderAttribute` класс, являющийся <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, который применяет заголовок ответа:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

Образец показывает использование соглашений для применения атрибута ко всем страницам в папке и к одной странице.

**Соглашение для модели приложений папки**

Используйте <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> для создания и добавления <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> , вызывающий действие <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> экземпляры для всех страниц в указанной папке.

Пример демонстрирует использование `AddFolderApplicationModelConvention` путем добавления заголовка `OtherPagesHeader` к страницам в папке *OtherPages* приложения:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

Запросите страницу Page1 по адресу `localhost:5000/OtherPages/Page1` из примера и посмотрите заголовки, чтобы проверить результат:

![Заголовки ответа страницы OtherPages/Page1, показывающие добавленный заголовок OtherPagesHeader.](razor-pages-conventions/_static/page1-otherpages-header.png)

**Соглашение для модели приложений страницы**

Используйте <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> для создания и добавления <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> , вызывающий действие <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> для страницы с указанным именем.

Пример демонстрирует использование `AddPageApplicationModelConvention` путем добавления заголовка `AboutHeader` на страницу About:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

Запросите страницу About по адресу `localhost:5000/About` из примера и посмотрите заголовки, чтобы проверить результат:

![Заголовки ответа страницы About, показывающие, что добавлен заголовок AboutHeader.](razor-pages-conventions/_static/about-page-about-header.png)

**Настройка фильтра**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> Настраивает указанный фильтр для применения. Вы можете создать класс фильтра, но пример приложения показывает реализацию фильтра в лямбда-выражении, которое реализовано "за кулисами" в качестве фабрики, возвращающей фильтр:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

Модель приложений страницы используется для проверки относительного пути на наличие сегментов, ведущих к странице Page2 в папке *OtherPages*. Если условие выполняется, добавляется заголовок. Если нет, применяется `EmptyFilter`.

`EmptyFilter` является [фильтром действий](xref:mvc/controllers/filters#action-filters). Поскольку в Razor Pages фильтры действий не учитываются, `EmptyFilter` является "холостой командой", как и задумано, если путь не содержит `OtherPages/Page2`.

Запросите страницу Page2 по адресу `localhost:5000/OtherPages/Page2` из примера и посмотрите заголовки, чтобы проверить результат:

![В ответ для страницы Page2 добавлен заголовок OtherPagesPage2Header.](razor-pages-conventions/_static/page2-filter-header.png)

**Настройка фабрики фильтров**

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> Настраивает указанную фабрику на применение [фильтры](xref:mvc/controllers/filters) ко всем страницам Razor.

В примере приложения приведен образец использования [фабрики фильтров](xref:mvc/controllers/filters#ifilterfactory) путем добавления на страницы приложения заголовка `FilterFactoryHeader` с двумя значениями:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*:

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

Запросите страницу About по адресу `localhost:5000/About` из примера и посмотрите заголовки, чтобы проверить результат:

![Заголовки ответов на странице About показывают два добавленных заголовка FilterFactoryHeader.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>Фильтры MVC и фильтр страниц (IPageFilter)

[Фильтры действий](xref:mvc/controllers/filters#action-filters) MVC игнорируются в Razor Pages, так как Razor Pages использует методы обработчиков. Для использования доступны другие типы фильтров MVC: [Авторизация](xref:mvc/controllers/filters#authorization-filters), [исключение](xref:mvc/controllers/filters#exception-filters), [ресурсов](xref:mvc/controllers/filters#resource-filters), и [результат](xref:mvc/controllers/filters#result-filters). Дополнительные сведения см. в разделе [Фильтры](xref:mvc/controllers/filters).

Фильтр страниц (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) представляет собой фильтр, применяемый для Razor Pages. Дополнительные сведения см. в разделе [Методы фильтрации для Razor Pages](xref:razor-pages/filter).

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>
