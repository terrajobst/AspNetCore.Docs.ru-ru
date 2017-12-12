---
title: "Razor страниц маршрута и приложения соглашение о возможностях ASP.NET Core"
author: guardrex
description: "Узнайте, как маршрут и приложения модель поставщика соглашение о функции помогают маршрутизации страницы управления обнаружения и обработки."
keywords: "ASP.NET Core, страниц Razor, соглашения, AddFolderRouteModelConvention, AddPageRouteModelConvention, AddPageRoute, AddFolderApplicationModelConvention, AddPageApplicationModelConvention ConfigureFilter, фильтры"
ms.author: riande
manager: wpickett
ms.date: 10/23/2017
ms.topic: article
ms.assetid: 6b60514c-81ad-485b-bb22-9b71416dff08
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/razor-pages/razor-pages-convention-features
ms.openlocfilehash: 81fe5198e25c4275f5cf0a123536a9130be8c1d9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="razor-pages-route-and-app-convention-features-in-aspnet-core"></a>Razor страниц маршрута и приложения соглашение о возможностях ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

Сведения об использовании страницы маршрута и приложения модель соглашение о функции поставщика для управления страницы маршрутизации, обнаружения и обработки в приложениях страниц Razor. Если нужно будет настроить маршруты настраиваемой страницы для отдельных страниц настройки маршрутизации на страницы с [AddPageRoute соглашение](#configure-a-page-route) описано далее в этом разделе.

Используйте [пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-convention-features/sample) ([загрузке](xref:tutorials/index#how-to-download-a-sample)) для изучения функций, описанных в этом разделе.

| Функции | В образце показано... |
| -------- | --------------------------- |
| [Модель соглашения маршрута и приложения](#add-route-and-app-model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li></ul> | Добавление шаблона маршрута и заголовок страницы приложения. |
| [Соглашения о действие маршрута страницы](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Добавление шаблона маршрута к страницам в папке и на одной странице. |
| [Страница модели действия соглашения](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (класс фильтра, лямбда-выражение или фильтр фабрики)</li></ul> | Добавление заголовка страницы в папке, добавление заголовка на одной странице и настройка [заводского](xref:mvc/controllers/filters#ifilterfactory) для добавления заголовка для страницы приложения. |
| [Поставщик модели по умолчанию страницы приложения](#replace-the-default-page-app-model-provider) | Замена поставщика модели страницы по умолчанию, изменение соглашения об именовании обработчика. |

## <a name="add-route-and-app-model-conventions"></a>Добавить соглашения модели маршрута и приложения

Добавьте делегат для [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) для добавления маршрута и приложения соглашения о модели, применимые к страниц Razor.

**Добавить соглашение модели маршрут ко всем страницам**

Используйте [соглашения](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) для создания и добавления [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) в коллекцию [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) экземпляров, которые применяются во время модель маршрута и страницы построение.

Добавляет в пример приложения `{globalTemplate?}` шаблон маршрута для всех страниц в приложении:

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

> [!NOTE]
> `Order` Свойство `AttributeRouteModel` задано значение `0` (ноль). Это гарантирует, что этот шаблон имеет преимущество для первой позиции значение данных маршрута при указано значение один маршрут. Например, в этом примере добавляется `{aboutTemplate?}` шаблон маршрута, далее в этом разделе. `{aboutTemplate?}` Шаблон предоставить `Order` из `1`. При запросе на страницу About `/About/RouteDataValue`, «RouteDataValue» загружается в `RouteData.Values["globalTemplate"]` (`Order = 0`) и не `RouteData.Values["aboutTemplate"]` (`Order = 1`) из-за параметра `Order` свойство.

*Файла Startup.cs*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet1)]

Запрос о страницу образцов в `localhost:5000/About/GlobalRouteValue` и проверьте результат:

![О программе страница запрашивается с GlobalRouteValue сегмент маршрута. Отображаемой страницы показывает, что значение данных маршрута захватывается в метод OnGet страницы.](razor-pages-convention-features/_static/about-page-global-template.png)

**Добавить соглашение о модели приложения ко всем страницам**

Используйте [соглашения](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) для создания и добавления [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) в коллекцию [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) экземпляров, которые применяются во время маршрутизации и страницы построение модели.

Чтобы продемонстрировать это и другие соглашения далее в этом разделе, включает пример приложения `AddHeaderAttribute` класса. Конструктор классов принимает `name` строки и `values` массив строк. Эти значения используются в его `OnResultExecuting` метод, чтобы задать заголовок ответа. Полный класс отображается в [страницы модели действия соглашения](#page-model-action-conventions) далее в разделе.

Образцы использования приложения `AddHeaderAttribute` класса, чтобы добавить верхний колонтитул `GlobalHeader`, чтобы все страницы в приложении:

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Файла Startup.cs*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet2)]

Запрос о страницу образцов в `localhost:5000/About` и проверять заголовки, чтобы просмотреть результат:

![Заголовки ответа страницы о программе показывают, что GlobalHeader был добавлен.](razor-pages-convention-features/_static/about-page-global-header.png)

## <a name="page-route-action-conventions"></a>Соглашения о действие маршрута страницы

Поставщик модели маршрута по умолчанию, производный от [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider) вызывает соглашения, которые предназначены для предоставления точек расширения для настройки маршрутов страницы.

**Соглашение о модели маршрута папки**

Используйте [AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) для создания и добавления [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) , вызывающий действие на [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) для всех страниц в разделе Указанная папка.

В образце приложения используется `AddFolderRouteModelConvention` добавление `{otherPagesTemplate?}` шаблон маршрута к страницам *OtherPages* папки:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet3)]

> [!NOTE]
> `Order` Свойство `AttributeRouteModel` равно `1`. Это гарантирует, что шаблон для `{globalTemplate?}` (set выше) имеет преимущество для значение первого данных маршрута позиции, если указано значение один маршрут. Если в запросе страницы Page1 `/OtherPages/Page1/RouteDataValue`, «RouteDataValue» загружается в `RouteData.Values["globalTemplate"]` (`Order = 0`) и не `RouteData.Values["otherPagesTemplate"]` (`Order = 1`) из-за параметра `Order` свойство.

Запрос страницы Page1 образец `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` и проверьте результат:

![Page1 в папке OtherPages запрашивается сегментом маршрута GlobalRouteValue и OtherPagesRouteValue. Отображаемой страницы показывает, что значения маршрута данные будут записаны в метод OnGet страницы.](razor-pages-convention-features/_static/otherpages-page1-global-and-otherpages-templates.png)

**Соглашение о модели маршрута страницы**

Используйте [AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) для создания и добавления [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) , вызывающий действие на [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) для страницы с указанным имя.

В образце приложения используется `AddPageRouteModelConvention` добавление `{aboutTemplate?}` шаблона маршрута к странице о программе:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet4)]

> [!NOTE]
> `Order` Свойство `AttributeRouteModel` равно `1`. Это гарантирует, что шаблон для `{globalTemplate?}` (set выше) имеет преимущество для значение первого данных маршрута позиции, если указано значение один маршрут. Если в запросе страницы о `/About/RouteDataValue`, «RouteDataValue» загружается в `RouteData.Values["globalTemplate"]` (`Order = 0`) и не `RouteData.Values["aboutTemplate"]` (`Order = 1`) из-за параметра `Order` свойство.

Запрос о страницу образцов в `localhost:5000/About/GlobalRouteValue/AboutRouteValue` и проверьте результат:

![Страница запрашивается с сегментами маршрута для GlobalRouteValue и AboutRouteValue. Отображаемой страницы показывает, что значения маршрута данные будут записаны в метод OnGet страницы.](razor-pages-convention-features/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a>Настройка маршрутов страницы

Используйте [AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) для настройки маршрута к странице на путь к указанной странице. Созданные ссылки на страницу использовать на заданный маршрут. `AddPageRoute`использует `AddPageRouteModelConvention` для установления маршрута.

Пример приложения создается маршрут к `/TheContactPage` для *Contact.cshtml*:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet5)]

Обратитесь к странице также может быть достигнута по `/Contact` через своего маршрута по умолчанию.

Обеспечивает настраиваемый маршрут в пример приложения обратитесь к странице необязательный `text` сегмента маршрута (`{text?}`). На этой странице также доступны это необязательно-сегмент в его `@page` директив в случае, если посетитель обращается к странице в его `/Contact` маршрута:

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Contact.cshtml?highlight=1)]

Обратите внимание, что созданный URL-адрес для **контакт** ссылку в отображаемой странице отражает обновленные маршрута:

![Образец приложения обратитесь к ссылки на панели навигации](razor-pages-convention-features/_static/contact-link.png)

![Проверка связи контакта в HTML указывает, присвоено значение атрибута href "/ TheContactPage"](razor-pages-convention-features/_static/contact-link-source.png)

Посетите страницу контакта на либо его обычным маршрута `/Contact`, или настраиваемый маршрут `/TheContactPage`. Если указать дополнительный `text` сегмента маршрута, на странице отображается сегмент HTML-кодирование необходимо указать:

![Пример браузера Edge указания маршрута Необязательный «текст» сегмент «Текст» в URL-АДРЕСЕ. Отображаемой страницы отображается значение сегмента «текст».](razor-pages-convention-features/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>Страница модели действия соглашения

Поставщик модели страницы по умолчанию, который реализует [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider) вызывает соглашения, которые предназначены для предоставления точек расширения для настройки модели страницы. Эти правила полезны при построении и изменения обнаружения страницы и функции обработки.

В примерах этого раздела примера приложения используется `AddHeaderAttribute` класса, который является [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute), которое применяет заголовок ответа:

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/AddHeader.cs?name=snippet1)]

С помощью соглашений, в образце показано применение атрибута для всех страниц в папке и на одной странице.

**Соглашение о модели приложения папки**

Используйте [AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) для создания и добавления [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) , вызывающий действие на [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) экземпляров для все страницы в указанной папке.

В образце показано использование `AddFolderApplicationModelConvention` путем добавления заголовка `OtherPagesHeader`, к страницам внутри *OtherPages* папку приложения:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet6)]

Запрос страницы Page1 образец `localhost:5000/OtherPages/Page1` и проверять заголовки, чтобы просмотреть результат:

![Заголовки ответа страницы OtherPages/Page1 показывают, что OtherPagesHeader был добавлен.](razor-pages-convention-features/_static/page1-otherpages-header.png)

**Соглашение о модели приложения страницы**

Используйте [AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention) для создания и добавления [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) , вызывающий действие на [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) для страницы с именем speciifed.

В образце показано использование `AddPageApplicationModelConvention` путем добавления заголовка `AboutHeader`, о программе страницу:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet7)]

Запрос о страницу образцов в `localhost:5000/About` и проверять заголовки, чтобы просмотреть результат:

![Заголовки ответа страницы о программе показывают, что AboutHeader был добавлен.](razor-pages-convention-features/_static/about-page-about-header.png)

**Настройка фильтра**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) настраивает указанный фильтр для применения. Можно реализовать класс фильтра, но в примере приложения показано, как для реализации фильтра в лямбда-выражения, который реализован как фабрика, которая возвращает фильтр фоновый:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet8)]

Модель приложения страница используется для проверки относительный путь для сегментов, которые приводят к странице страница 2 в *OtherPages* папки. Если условие выполняется успешно, добавляется заголовок. Если нет, `EmptyFilter` применяется.

`EmptyFilter`— [фильтр действий](xref:mvc/controllers/filters#action-filters). Поскольку фильтры действий, учитываются страниц Razor `EmptyFilter` нет ops должным образом, если путь не содержит `OtherPages/Page2`.

Запрос страницы страница 2 образца `localhost:5000/OtherPages/Page2` и проверять заголовки, чтобы просмотреть результат:

![OtherPagesPage2Header добавляется в ответ для страница 2.](razor-pages-convention-features/_static/page2-filter-header.png)

**Настройка фабрики фильтра**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) настраивает указанного фабрики для применения [фильтры](xref:mvc/controllers/filters) для всех страниц Razor.

Пример приложения предоставляет пример использования [заводского](xref:mvc/controllers/filters#ifilterfactory) путем добавления заголовка `FilterFactoryHeader`, с двумя значениями для страницы приложения:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs*:

[!code-csharp[Main](razor-pages-convention-features/sample/Factories/AddHeaderWithFactory.cs?name=snippet1)]

Запрос о страницу образцов в `localhost:5000/About` и проверять заголовки, чтобы просмотреть результат:

![Заголовки ответа страницы About показывают, что были добавлены два заголовка FilterFactoryHeader.](razor-pages-convention-features/_static/about-page-filter-factory-header.png)

## <a name="replace-the-default-page-app-model-provider"></a>Замените поставщик модели приложения страницы по умолчанию

Использует страниц Razor `IPageApplicationModelProvider` интерфейс для создания [DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider). Можно наследовать от поставщика модели по умолчанию, чтобы предоставить свою собственную логику реализации для обработчика обнаружения и обработки. Реализация по умолчанию ([ссылки на источник](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) устанавливает соглашения для *неименованные* и *с именем* обработчик именования, как описано ниже.

**По умолчанию неименованный методы обработчика**

Методы обработчика HTTP-команды (методы обработчика «неименованный») соглашению: `On<HTTP verb>[Async]` (добавления `Async` необязательно, но рекомендуется для асинхронных методов).

| Метод неименованные обработчика     | Операция                      |
| -------------------------- | ------------------------------ |
| `OnGet`/`OnGetAsync`       | Инициализируйте состояние страницы.     |
| `OnPost`/`OnPostAsync`     | Обрабатывает запросы POST.          |
| `OnDelete`/`OnDeleteAsync` | Обработка запросов на удаление &#8224;. |
| `OnPut`/`OnPutAsync`       | Обрабатывать запросы PUT &#8224;.    |
| `OnPatch`/`OnPatchAsync`   | Обрабатывать запросы PATCH &#8224;.  |

&#8224; Используется для выполнения вызовов API для страницы.

**По умолчанию с именем методы обработчика**

Методы обработчика, предусмотренные разработчиком (» с именем «методы обработчика) следуйте аналогичным именах. Имя обработчика появляется после команды HTTP, а также между командой HTTP и `Async`: `On<HTTP verb><handler name>[Async]` (добавления `Async` необязательно, но рекомендуется для асинхронных методов). Например методы, которые обрабатывают сообщения может занять именования, показанные в следующей таблице.

| Пример с именем метода обработчика             | Пример операции        |
| ---------------------------------------- | ------------------------ |
| `OnGetMessage`/`OnGetMessageAsync`       | Получите сообщение.        |
| `OnPostMessage`/`OnPostMessageAsync`     | ОТПРАВЬТЕ сообщение.          |
| `OnDeleteMessage`/`OnDeleteMessageAsync` | Удалите сообщение &#8224;. |
| `OnPutMessage`/`OnPutMessageAsync`       | ПОМЕСТИТЕ сообщение &#8224;.    |
| `OnPatchMessage`/`OnPatchMessageAsync`   | Исправление для сообщения &#8224;.  |

&#8224; Используется для выполнения вызовов API для страницы.

**Настройка имен метод обработчика**

Предположим, нужно изменить способ именуются методы обработчика именованные и неименованные. Альтернативные схемы именования — избежать, начиная с «On» имена методов и использовать первый сегмент word, чтобы определить команду HTTP. Можно внести другие изменения, например преобразования команды для удаления, PUT и PATCH для БЛОГА. Такая схема предоставляет имена методов, показанные в следующей таблице.

| Метод обработчика                       | Операция                      |
| ------------------------------------ | ------------------------------ |
| `Get`                                | Инициализируйте состояние страницы.     |
| `Post`/`PostAsync`                   | Обрабатывает запросы POST.          |
| `Delete`/`DeleteAsync`               | Обработка запросов на удаление &#8224;. |
| `Put`/`PutAsync`                     | Обрабатывать запросы PUT &#8224;.    |
| `Patch`/`PatchAsync`                 | Обрабатывать запросы PATCH &#8224;.  |
| `GetMessage`                         | Получите сообщение.              |
| `PostMessage`/`PostMessageAsync`     | ОТПРАВЬТЕ сообщение.                |
| `DeleteMessage`/`DeleteMessageAsync` | ОТПРАВЬТЕ сообщение для удаления.      |
| `PutMessage`/`PutMessageAsync`       | ОТПРАВЬТЕ сообщение для размещения.         |
| `PatchMessage`/`PatchMessageAsync`   | ОТПРАВЬТЕ сообщение в исправления.       |

&#8224; Используется для выполнения вызовов API для страницы.

Чтобы установить эту схему, наследуют `DefaultPageApplicationModelProvider` класса и переопределить [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel) метод для предоставления пользовательской логики для разрешения [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) имена обработчиков. В образце приложения показано, как это делается его `CustomPageApplicationModelProvider` класса:

[!code-csharp[Main](razor-pages-convention-features/sample/CustomPageApplicationModelProvider.cs?name=snippet1&highlight=1-2,45-46,64-68,78-85,87,92,106)]

Особенности класса:

* Этот класс наследует от `DefaultPageApplicationModelProvider`.
* `TryParseHandlerMethod` Обрабатывает обработчик для определения команду HTTP (`httpMethod`) и имя именованного обработчика (`handlerName`) при создании `PageHandlerModel`.
  * `Async` Постфиксного игнорируется, если он имеется.
  * Используется для синтаксического анализа HTTP-команду от имени метода.
  * Если имя метода (без `Async`) — совпадает с именем команды HTTP, отсутствует именованный обработчик. `handlerName` Равно `null`, и имя метода `Get`, `Post`, `Delete`, `Put`, или `Patch`.
  * Если имя метода (без `Async`) длиннее, чем наименованием глагола HTTP именованный обработчик. Для `handlerName` задано значение `<method name (less 'Async', if present)>`. Например «GetMessage» и «GetMessageAsync» дают имя обработчика «GetMessage».
  * DELETE, PUT и PATCH HTTP-команды, преобразуются в POST.

Зарегистрировать `CustomPageApplicationModelProvider` в `Startup` класса:

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet10)]

Файл кода программной *Index.cshtml.cs* показано, как изменяются соглашения об именовании метод обычным обработчик для страниц в приложении. Обычный «Вкл.» префикс именования использоваться со страницами Razor удаляется. Метод, который инициализирует состояние страницы теперь называется `Get`. Вы увидите это соглашение, оканчивающимися приложения при открытии любого файла кода для любой из страниц.

Каждый из других методов начинаются с команду HTTP, которая описывает его обработки. Два метода, которые начинаются с `Delete` обычно будет рассматриваться как удалить HTTP-команды, но логика `TryParseHandlerMethod` явно задает команды POST для обоих обработчиков.

Обратите внимание, что `Async` является необязательным между `DeleteAllMessages` и `DeleteMessageAsync`. Они оба асинхронные методы, но вы можете использовать `Async` постфиксный или нет, мы рекомендуем сделать. `DeleteAllMessages`Здесь используется для демонстрации, однако рекомендуется давать такой метод `DeleteAllMessagesAsync`. Он не влияет на обработку образец реализации, но использование `Async` постфиксные вызовы указать на то, что это асинхронный метод.

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/Index.cshtml.cs?name=snippet1&highlight=1,6,16,29)]

Обратите внимание, в имена обработчиков *Index.cshtml* соответствует `DeleteAllMessages` и `DeleteMessageAsync` методы обработчика:

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Index.cshtml?range=29-60&highlight=7-8,24-25)]

`Async`в поле имя метода обработчика `DeleteMessageAsync` помещает добавляются по `TryParseHandlerMethod` для сопоставления обработчика запроса POST для метода. `asp-page-handler` Имя `DeleteMessage` соответствует методу обработчика `DeleteMessageAsync`.

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>Фильтры MVC и фильтр страницы (IPageFilter)

MVC [фильтры действий](xref:mvc/controllers/filters#action-filters) игнорируются, страниц Razor, так как страниц Razor использовать методы обработчика. Другие типы фильтров MVC доступны для использования: [авторизации](xref:mvc/controllers/filters#authorization-filters), [исключение](xref:mvc/controllers/filters#exception-filters), [ресурсов](xref:mvc/controllers/filters#resource-filters), и [результат](xref:mvc/controllers/filters#result-filters). Дополнительные сведения см. в разделе [фильтры](xref:mvc/controllers/filters) раздела.

Фильтр страницы ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) представляет собой фильтр, применяется к страницам Razor. Он похожими на выполнение метода обработки страницы. Он позволяет обрабатывать пользовательский код на стадии выполнения метода обработчика страницы. Ниже приведен пример из примера приложения:

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/ReplaceRouteValueFilterAttribute.cs?name=snippet1)]

Этот фильтр проверяет наличие `globalTemplate` маршрутизации в «Заменяющее_значение» значение «TriggerValue» и операции замены.

`ReplaceRouteValueFilter` Атрибут может применяться непосредственно к `PageModel` в коде:

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/OtherPages/Page3.cshtml.cs?range=10-12&highlight=1)]

Запрос страницы Page3 из примера приложения с на `localhost:5000/OtherPages/Page3/TriggerValue`. Обратите внимание на то, как фильтр заменяет значение маршрута:

![Запрос на OtherPages/Page3 результатами TriggerValue маршрута сегмента в фильтре, заменив Заменяющее_значение значения маршрута.](razor-pages-convention-features/_static/otherpages-page3-filter-replacement-value.png)

## <a name="see-also"></a>См. также

* [Правила авторизации страниц Razor](xref:security/authorization/razor-pages-authorization)
