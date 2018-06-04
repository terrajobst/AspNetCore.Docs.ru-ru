---
title: Соглашения для маршрутов и приложений Razor Pages в ASP.NET Core
author: guardrex
description: Узнайте, как использовать соглашения поставщика модели маршрутов и приложений для управления маршрутизацией, обнаружением и обработкой страниц.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 04/12/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/razor-pages/razor-pages-conventions
ms.openlocfilehash: eba3422fbf46ac181a783b7f8cc605c2a549b4b7
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729747"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a>Соглашения для маршрутов и приложений Razor Pages в ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

Узнайте, как использовать [соглашения поставщика модели маршрутов и приложений](xref:mvc/controllers/application-model#conventions) страниц для управления маршрутизацией, обнаружением и обработкой страниц в приложениях Razor Pages. Когда вам необходимо настроить уникальные маршруты для отдельных страниц, используйте [соглашение AddPageRoute](#configure-a-page-route), описанное далее в этом разделе.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-conventions/sample/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

::: moniker range="= aspnetcore-2.0"
| Сценарий | Что демонстрирует пример |
| -------- | --------------------------- |
| [Соглашения для моделей](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li></ul> | Добавление шаблона маршрута и заголовка к страницам приложения. |
| [Соглашения для действий с маршрутами страниц](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Добавление шаблона маршрута к страницам в папке и к одной странице. |
| [Соглашения для действий модели страниц](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (класс фильтра, лямбда-выражение или фабрика фильтров)</li></ul> | Добавление заголовка к страницам в папке, добавление заголовка к одной странице и настройка [фабрики фильтров](xref:mvc/controllers/filters#ifilterfactory) для добавления заголовка к страницам приложения. |
| [Поставщик модели приложений страниц по умолчанию](#replace-the-default-page-app-model-provider) | Замена поставщика модели страниц по умолчанию для изменения соглашений по именованию обработчиков. |
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
| Сценарий | Что демонстрирует пример |
| -------- | --------------------------- |
| [Соглашения для моделей](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li><li>IPageHandlerModelConvention</li></ul> | Добавление шаблона маршрута и заголовка к страницам приложения. |
| [Соглашения для действий с маршрутами страниц](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Добавление шаблона маршрута к страницам в папке и к одной странице. |
| [Соглашения для действий модели страниц](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (класс фильтра, лямбда-выражение или фабрика фильтров)</li></ul> | Добавление заголовка к страницам в папке, добавление заголовка к одной странице и настройка [фабрики фильтров](xref:mvc/controllers/filters#ifilterfactory) для добавления заголовка к страницам приложения. |
| [Поставщик модели приложений страниц по умолчанию](#replace-the-default-page-app-model-provider) | Замена поставщика модели страниц по умолчанию для изменения соглашений по именованию обработчиков. |
::: moniker-end

Соглашения Razor Pages добавляются и настраиваются с помощью метода расширения [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) в [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc) в коллекции служб в классе `Startup`. Следующие примеры соглашений описаны далее в этом разделе:

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

## <a name="model-conventions"></a>Соглашения для моделей

Добавьте делегат для [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention), чтобы добавить [соглашения для модели](xref:mvc/controllers/application-model#conventions), применяемые к Razor Pages.

**Добавление соглашения для модели маршрутов ко всем страницам**

Используйте свойство [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions), чтобы создать и добавить [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) в коллекцию экземпляров [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention), которые применяются при формировании модели маршрутов страниц.

В примере приложения шаблон маршрута `{globalTemplate?}` добавляется ко всем страницам приложения:

[!code-csharp[](razor-pages-conventions/sample/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

> [!NOTE]
> Свойству `Order` для `AttributeRouteModel` задано значение `-1`. Это гарантирует, что этот шаблон получит приоритет для первой позиции значения данных маршрута, когда предоставляется одно значение маршрута, а также что он будет иметь приоритет над автоматически созданными маршрутами Razor Pages. Проиллюстрировать это можно следующим образом. Далее в этом разделе показан пример, где добавляется шаблон маршрута `{aboutTemplate?}`. Шаблону `{aboutTemplate?}` задано свойство `Order`, равное `1`. При запросе страницы About по адресу `/About/RouteDataValue` значение RouteDataValue загружается в `RouteData.Values["globalTemplate"]` (`Order = -1`), а не в `RouteData.Values["aboutTemplate"]` (`Order = 1`). Это происходит из-за значения свойства `Order`.

Параметры Razor Pages, такие как добавление [соглашений](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions), добавляются при добавлении MVC в коллекцию службы в `Startup.ConfigureServices`. Пример см. в [образце приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-conventions/sample/).

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet1)]

Запросите страницу About по адресу `localhost:5000/About/GlobalRouteValue` из примера и проверьте результат:

![Запрос страницы About с сегментом маршрута GlobalRouteValue. Отображенная страница показывает, что значение данных маршрута получено в методе OnGet страницы.](razor-pages-conventions/_static/about-page-global-template.png)

**Добавление соглашения для модели приложений ко всем страницам**

Используйте свойство [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions), чтобы создать и добавить [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) в коллекцию экземпляров [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention), которые применяются при формировании модели приложения страницы.

Для демонстрации этих и других соглашений далее в этом разделе в пример приложения включен класс `AddHeaderAttribute`. Конструктор класса принимает строку `name` и массив строк `values`. Эти значения используются в его методе `OnResultExecuting` для задания заголовка ответа. Класс показан полностью в разделе [Соглашения для действий модели страниц](#page-model-action-conventions) далее.

Пример приложения использует класс `AddHeaderAttribute`, чтобы добавить заголовок `GlobalHeader` ко всем страницам в приложении:

[!code-csharp[](razor-pages-conventions/sample/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet2)]

Запросите страницу About по адресу `localhost:5000/About` из примера и посмотрите заголовки, чтобы проверить результат:

![Заголовки ответа страницы About, показывающие, что добавлен заголовок GlobalHeader.](razor-pages-conventions/_static/about-page-global-header.png)

::: moniker range=">= aspnetcore-2.1"
**Добавление соглашения для модели обработчика ко всем страницам**

Используйте свойство [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions), чтобы создать и добавить [IPageHandlerModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipagehandlermodelconvention) в коллекцию экземпляров [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention), которые применяются при формировании модели обработчика страницы.

```csharp
public class GlobalPageHandlerModelConvention 
    : IPageHandlerModelConvention
{
    public void Apply(PageHandlerModel model)
    {
        ...
    }
}
```

`Startup.ConfigureServices`:

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add(new GlobalPageHandlerModelConvention());
        });
```
::: moniker-end

## <a name="page-route-action-conventions"></a>Соглашения для действий с маршрутами страниц

Поставщик модели маршрутов по умолчанию, который является производным от [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider), использует соглашения, создающие точки расширения для настройки маршрутов страниц.

**Соглашение для модели маршрутов папки**

Используйте метод [AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention), чтобы создать и добавить интерфейс [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention), вызывающий действие в классе [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) для всех страниц в указанной папке.

Пример приложения использует `AddFolderRouteModelConvention` для добавления шаблона маршрута `{otherPagesTemplate?}` к страницам в папке *OtherPages*:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet3)]

> [!NOTE]
> Свойству `Order` для `AttributeRouteModel` задано значение `1`. Это гарантирует, что при указании одного значения маршрута приоритетно выбирается значение данных маршрута из шаблона `{globalTemplate?}`, заданного ранее в этом разделе. При запросе страницы Page1 по адресу `/OtherPages/Page1/RouteDataValue` значение RouteDataValue загружается в `RouteData.Values["globalTemplate"]` (`Order = -1`), а не в `RouteData.Values["otherPagesTemplate"]` (`Order = 1`). Это происходит из-за значения свойства `Order`.

Запросите страницу Page1 по адресу `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` из примера и проверьте результат:

![Страница Page1 в папке OtherPages запрашивается с сегментом маршрута GlobalRouteValue и OtherPagesRouteValue. Отображенная страница показывает, что значения данных маршрута получены в методе OnGet страницы.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

**Соглашение для модели маршрутов страницы**

Используйте метод [AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention), чтобы создать и добавить интерфейс [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention), вызывающий действие в классе [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) для страницы с указанным именем.

Пример приложения использует `AddPageRouteModelConvention` для добавления шаблона маршрута `{aboutTemplate?}` к странице About:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet4)]

> [!NOTE]
> Свойству `Order` для `AttributeRouteModel` задано значение `1`. Это гарантирует, что при указании одного значения маршрута приоритетно выбирается значение данных маршрута из шаблона `{globalTemplate?}`, заданного ранее в этом разделе. При запросе страницы About по адресу `/About/RouteDataValue` значение RouteDataValue загружается в `RouteData.Values["globalTemplate"]` (`Order = -1`), а не в `RouteData.Values["aboutTemplate"]` (`Order = 1`). Это происходит из-за значения свойства `Order`.

Запросите страницу About по адресу `localhost:5000/About/GlobalRouteValue/AboutRouteValue` из примера и проверьте результат:

![Страница About запрашивается с сегментами маршрута для GlobalRouteValue и AboutRouteValue. Отображенная страница показывает, что значения данных маршрута получены в методе OnGet страницы.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a>Настройка маршрута страницы

Используйте метод [AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute), чтобы настроить маршрут к странице по указанному пути страницы. Созданные ссылки на страницу будут использовать заданный вами маршрут. Для установления маршрута метод `AddPageRoute` использует `AddPageRouteModelConvention`.

Пример приложения создает маршрут к `/TheContactPage` для файла *Contact.cshtml*:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet5)]

На страницу Contact также можно перейти с помощью `/Contact` через маршрут по умолчанию.

Настраиваемый маршрут на страницу Contact в примере приложения позволяет использовать необязательный сегмент маршрута `text` (`{text?}`). Страница также включает этот необязательный сегмент в своей директиве `@page` на случай, если посетитель переходит на страницу по маршруту `/Contact`:

[!code-cshtml[](razor-pages-conventions/sample/Pages/Contact.cshtml?highlight=1)]

Обратите внимание, что URL-адрес, созданный для ссылки **Contact** на отображенной странице, отражает обновленный маршрут:

![Ссылка Contact на панели навигации в примере приложения](razor-pages-conventions/_static/contact-link.png)

![В ссылке Contact в показанном HTML видно, что href имеет значение "/TheContactPage".](razor-pages-conventions/_static/contact-link-source.png)

Перейдите на страницу Contact по ее обычному маршруту `/Contact` или по настроенному маршруту `/TheContactPage`. Если вы укажете дополнительный сегмент маршрута `text`, страница отобразит этот сегмент в HTML-кодировке:

![Пример в браузере Microsoft Edge с указанием необязательного сегмента маршрута "text" со значением "TextValue" в URL-адресе. Отображенная страница показывает значение сегмента "text".](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>Соглашения для действий модели страниц

Поставщик модели страниц по умолчанию, который реализует [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider), использует соглашения, создающие точки расширения для настройки моделей страниц. Эти соглашения полезны при создании и изменении для обнаружения и обработки страниц.

В примерах этого раздела образец приложения использует класс `AddHeaderAttribute`, который является [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute) и применяет заголовок ответа:

[!code-csharp[](razor-pages-conventions/sample/Filters/AddHeader.cs?name=snippet1)]

Образец показывает использование соглашений для применения атрибута ко всем страницам в папке и к одной странице.

**Соглашение для модели приложений папки**

Используйте метод [AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention), чтобы создать и добавить интерфейс [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention), вызывающий действие в экземплярах [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) для всех страниц в указанной папке.

Пример демонстрирует использование `AddFolderApplicationModelConvention` путем добавления заголовка `OtherPagesHeader` к страницам в папке *OtherPages* приложения:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet6)]

Запросите страницу Page1 по адресу `localhost:5000/OtherPages/Page1` из примера и посмотрите заголовки, чтобы проверить результат:

![Заголовки ответа страницы OtherPages/Page1, показывающие добавленный заголовок OtherPagesHeader.](razor-pages-conventions/_static/page1-otherpages-header.png)

**Соглашение для модели приложений страницы**

Используйте метод [AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention), чтобы создать и добавить интерфейс [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention), вызывающий действие в классе [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) для страницы с указанным именем.

Пример демонстрирует использование `AddPageApplicationModelConvention` путем добавления заголовка `AboutHeader` на страницу About:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet7)]

Запросите страницу About по адресу `localhost:5000/About` из примера и посмотрите заголовки, чтобы проверить результат:

![Заголовки ответа страницы About, показывающие, что добавлен заголовок AboutHeader.](razor-pages-conventions/_static/about-page-about-header.png)

**Настройка фильтра**

Для настройки применяемого фильтра используется метод [ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter). Вы можете создать класс фильтра, но пример приложения показывает реализацию фильтра в лямбда-выражении, которое реализовано "за кулисами" в качестве фабрики, возвращающей фильтр:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet8)]

Модель приложений страницы используется для проверки относительного пути на наличие сегментов, ведущих к странице Page2 в папке *OtherPages*. Если условие выполняется, добавляется заголовок. Если нет, применяется `EmptyFilter`.

`EmptyFilter` является [фильтром действий](xref:mvc/controllers/filters#action-filters). Поскольку в Razor Pages фильтры действий не учитываются, `EmptyFilter` является "холостой командой", как и задумано, если путь не содержит `OtherPages/Page2`.

Запросите страницу Page2 по адресу `localhost:5000/OtherPages/Page2` из примера и посмотрите заголовки, чтобы проверить результат:

![В ответ для страницы Page2 добавлен заголовок OtherPagesPage2Header.](razor-pages-conventions/_static/page2-filter-header.png)

**Настройка фабрики фильтров**

Метод [ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) позволяет настроить указанную фабрику на применение [фильтров](xref:mvc/controllers/filters) ко всем страницам Razor Pages.

В примере приложения приведен образец использования [фабрики фильтров](xref:mvc/controllers/filters#ifilterfactory) путем добавления на страницы приложения заголовка `FilterFactoryHeader` с двумя значениями:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*:

[!code-csharp[](razor-pages-conventions/sample/Factories/AddHeaderWithFactory.cs?name=snippet1)]

Запросите страницу About по адресу `localhost:5000/About` из примера и посмотрите заголовки, чтобы проверить результат:

![Заголовки ответов на странице About показывают два добавленных заголовка FilterFactoryHeader.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="replace-the-default-page-app-model-provider"></a>Замена поставщика модели приложений страниц по умолчанию

В Razor Pages используется интерфейс `IPageApplicationModelProvider` для создания [DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider). Вы можете наследовать его от поставщика модели по умолчанию, чтобы создать собственную логику реализации для обнаружения и обработки обработчиков. Реализация по умолчанию ([ссылка на исходный код](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) устанавливает соглашения для именования *неименованных* и *именованных* обработчиков, как описано ниже.

**Методы неименованных обработчиков по умолчанию**

Методы обработчиков для HTTP-команд (методы неименованных обработчиков) следуют соглашению `On<HTTP verb>[Async]` (добавление `Async` является необязательным, но рекомендуется для асинхронных методов).

| Метод неименованных обработчиков     | Операция                      |
| -------------------------- | ------------------------------ |
| `OnGet`/`OnGetAsync`       | Инициализация состояния страницы.     |
| `OnPost`/`OnPostAsync`     | Обработка запросов POST.          |
| `OnDelete`/`OnDeleteAsync` | Обработка запросов DELETE &#8224;. |
| `OnPut`/`OnPutAsync`       | Обработка запросов PUT &#8224;.    |
| `OnPatch`/`OnPatchAsync`   | Обработка запросов PATCH &#8224;.  |

&#8224; Используется для вызовов API к странице.

**Методы именованных обработчиков по умолчанию**

Для методов обработчиков, предоставляемых разработчиком (методов именованных обработчиков), действует похожее соглашение. Имя обработчика отображается после HTTP-команды или между HTTP-командой и `Async`: `On<HTTP verb><handler name>[Async]` (добавление `Async` является необязательным, но рекомендуется для асинхронных методов). Например, к методам, которые обрабатывают сообщения, может применяться именование, показанное в следующей таблице.

| Пример метода именованных обработчиков             | Пример операции        |
| ---------------------------------------- | ------------------------ |
| `OnGetMessage`/`OnGetMessageAsync`       | Получение сообщения.        |
| `OnPostMessage`/`OnPostMessageAsync`     | Отправка (POST) сообщения.          |
| `OnDeleteMessage`/`OnDeleteMessageAsync` | Удаление (DELETE) сообщения &#8224;. |
| `OnPutMessage`/`OnPutMessageAsync`       | Размещение (PUT) сообщения &#8224;.    |
| `OnPatchMessage`/`OnPatchMessageAsync`   | Обновление (PATCH) сообщения &#8224;.  |

&#8224; Используется для вызовов API к странице.

**Настройка имен методов обработчиков**

Предположим, вы хотите изменить способ именования методов именованных и неименованных обработчиков. Ваша схема должна не допускать слово "On" в начале имен методов и использовать сегмент с первым словом для определения HTTP-команды. Вы можете внести и другие изменения, например преобразовать команды для операций DELETE, PUT и PATCH в POST. Такая схема позволяет получить имена методов, показанные в следующей таблице.

| Метод обработчиков                       | Операция                      |
| ------------------------------------ | ------------------------------ |
| `Get`                                | Инициализация состояния страницы.     |
| `Post`/`PostAsync`                   | Обработка запросов POST.          |
| `Delete`/`DeleteAsync`               | Обработка запросов DELETE &#8224;. |
| `Put`/`PutAsync`                     | Обработка запросов PUT &#8224;.    |
| `Patch`/`PatchAsync`                 | Обработка запросов PATCH &#8224;.  |
| `GetMessage`                         | Получение сообщения.              |
| `PostMessage`/`PostMessageAsync`     | Отправка (POST) сообщения.                |
| `DeleteMessage`/`DeleteMessageAsync` | Отправка (POST) сообщения для удаления.      |
| `PutMessage`/`PutMessageAsync`       | Отправка (POST) сообщения для размещения.         |
| `PatchMessage`/`PatchMessageAsync`   | Отправка (POST) сообщения для обновления.       |

&#8224; Используется для вызовов API к странице.

Чтобы создать эту схему, используйте наследование от класса `DefaultPageApplicationModelProvider` и переопределите метод [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel), чтобы задать свою логику для разрешения имен обработчиков [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel). Пример приложения показывает реализацию такой схемы в классе `CustomPageApplicationModelProvider`:

[!code-csharp[](razor-pages-conventions/sample/CustomPageApplicationModelProvider.cs?name=snippet1&highlight=1-2,45-46,64-68,78-85,87,92,106)]

Особенности класса:

* Класс наследует от `DefaultPageApplicationModelProvider`.
* `TryParseHandlerMethod` анализирует обработчик для определения HTTP-команды (`httpMethod`) и имени именованного обработчика (`handlerName`) при создании `PageHandlerModel`.
  * Если имеется постфикс `Async`, он игнорируется.
  * Для выявления HTTP-команды по имени метода используется учет регистра.
  * Если имя метода (без `Async`) совпадает с именем HTTP-команды, значит, именованный обработчик отсутствует. `handlerName` получает значение `null`, а имя метода — это `Get`, `Post`, `Delete`, `Put` или `Patch`.
  * Если имя метода (без `Async`) длиннее имени HTTP-команды, значит, здесь присутствует именованный обработчик. Для `handlerName` задано значение `<method name (less 'Async', if present)>`. Например, при анализе "GetMessage" и "GetMessageAsync" будет получено одно и то же имя обработчика "GetMessage".
  * HTTP-команды DELETE, PUT и PATCH преобразуются в POST.

Зарегистрируйте `CustomPageApplicationModelProvider` в классе `Startup`:

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet10)]

В модели страниц в файле *Index.cshtml.cs* показано, как изменяются обычные соглашения об именовании методов обработчиков для страниц в приложении. Обычный префикс именования "On", используемый в Razor Pages, удаляется. Метод, который инициализирует состояние страницы, теперь называется `Get`. Если вы откроете любую модель для любой из страниц, вы увидите, что это соглашение используется по всему приложению.

Имена всех остальных методов начинаются с HTTP-команды, которая описывает его вариант обработки. Два метода, которые начинаются с `Delete`, обычно рассматриваются как HTTP-команды DELETE, но логика в `TryParseHandlerMethod` явно задает команду POST для обоих обработчиков.

Обратите внимание, что постфикс `Async` является необязательным для `DeleteAllMessages` и `DeleteMessageAsync`. Оба этих метода являются асинхронными, но вы можете не использовать для них постфикс `Async`, хотя мы рекомендуем использовать его. Метод `DeleteAllMessages` показан здесь для примера, но рекомендуем присваивать ему имя `DeleteAllMessagesAsync`. Он не влияет на обработку реализации примера, но наличие постфикса `Async` указывает на то, что это асинхронный метод.

[!code-csharp[](razor-pages-conventions/sample/Pages/Index.cshtml.cs?name=snippet1&highlight=1,6,16,29)]

Обратите внимание, что имена обработчиков, приведенные в *Index.cshtml*, совпадают с методами обработчиков `DeleteAllMessages` и `DeleteMessageAsync`:

[!code-cshtml[](razor-pages-conventions/sample/Pages/Index.cshtml?range=29-60&highlight=7-8,24-25)]

`Async` в имени метода обработчика `DeleteMessageAsync` отбрасывается методом `TryParseHandlerMethod` для сопоставления методу обработчиков запросов POST. Имя `DeleteMessage` из `asp-page-handler` сопоставляется методу обработчика `DeleteMessageAsync`.

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>Фильтры MVC и фильтр страниц (IPageFilter)

[Фильтры действий](xref:mvc/controllers/filters#action-filters) MVC игнорируются в Razor Pages, так как Razor Pages использует методы обработчиков. Доступны для использования другие типы фильтров MVC: фильтры [авторизации](xref:mvc/controllers/filters#authorization-filters), [исключений](xref:mvc/controllers/filters#exception-filters), [ресурсов](xref:mvc/controllers/filters#resource-filters) и [результатов](xref:mvc/controllers/filters#result-filters). Дополнительные сведения см. в разделе [Фильтры](xref:mvc/controllers/filters).

Фильтр страниц ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) — это фильтр, применяемый для Razor Pages. Дополнительные сведения см. в разделе [Методы фильтрации для Razor Pages](xref:mvc/razor-pages/filter).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Соглашения об авторизации Razor Pages](xref:security/authorization/razor-pages-authorization)
