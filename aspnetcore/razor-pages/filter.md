---
title: Методы фильтрации для Razor Pages в ASP.NET Core
author: Rick-Anderson
description: Описание способов создания методов фильтрации Razor Pages в ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 2/18/2020
uid: razor-pages/filter
ms.openlocfilehash: a60b17685c6f836de7c0afcc5b89a9894fb8b28f
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447235"
---
# <a name="filter-methods-for-razor-pages-in-aspnet-core"></a>Методы фильтрации для Razor Pages в ASP.NET Core

::: moniker range=">= aspnetcore-3.0"

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)

Фильтры Razor Pages [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) и [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) разрешают Razor Pages выполнять код до и после запуска обработчика Razor Pages. Фильтры страницы Razor похожи на [фильтры действий MVC ASP.NET Core](xref:mvc/controllers/filters#action-filters), но их нельзя применять к методам обработчика отдельной страницы.

Фильтры страницы Razor:

* Запускают код после выбора метода обработчика, но до привязки модели.
* Запускают код до выполнения метода обработчика, но после привязки модели.
* Запускают код после выполнения метода обработчика.
* Могут реализовываться глобально или на странице.
* Не могут применяться к методам обработчика для конкретной страницы.
* Может иметь зависимости конструктора, заполненные путем [внедрения зависимостей](xref:fundamentals/dependency-injection) (DI). Дополнительные сведения см. в разделе [сервицефилтераттрибуте](/aspnet/core/mvc/controllers/filters#servicefilterattribute) и [типефилтераттрибуте](/aspnet/core/mvc/controllers/filters#typefilterattribute).

Хотя конструкторы страниц и по промежуточного слоя позволяют выполнять пользовательский код до выполнения метода обработчика, только фильтры страниц Razor обеспечивают доступ к <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.HttpContext> и странице. По промежуточного слоя имеет доступ к `HttpContext`, но не к "контексту страницы". Фильтры имеют <xref:Microsoft.AspNetCore.Mvc.Filters.FilterContext> производный параметр, который обеспечивает доступ к `HttpContext`. Например, образец [Применение атрибута фильтра](#ifa) добавляет заголовок к ответу. Это невозможно сделать с помощью конструкторов или ПО промежуточного слоя.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/filter/3.1sample) ([как скачивать](xref:index#how-to-download-a-sample))

Фильтры страницы Razor предоставляют следующие методы, которые могут применяться глобально или на уровне страницы:

* Синхронные методы:

  * [OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0). Вызывается после выбора метода обработчика, но до привязки модели.
  * [OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0). Вызывается до выполнения метода обработчика, но после привязки модели.
  * [OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0). Вызывается после выполнения метода обработчика, но до результата действия.

* Асинхронные методы:

  * [OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0). Вызывается асинхронно после выбора метода обработчика, но до привязки модели.
  * [OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0). Вызывается асинхронно до вызова метода обработчика, но после привязки модели.

Реализуйте синхронный **или** асинхронный интерфейс фильтра, но **не** оба варианта. Платформа сначала проверяет, реализует ли фильтр асинхронный интерфейс. Если да, вызывается он. В противном случае вызываются методы синхронного интерфейса. Если реализуются оба интерфейса, вызываются только асинхронные методы. Это же правило применяется к переопределению на страницах — реализуйте синхронную или асинхронную версию переопределения, но не обе сразу.

## <a name="implement-razor-page-filters-globally"></a>Реализации фильтров страницы Razor глобально

В следующем коде реализуется фильтр `IAsyncPageFilter`:

[!code-csharp[Main](filter/3.1sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

В приведенном выше коде `ProcessUserAgent.Write` является пользовательским кодом, который работает со строкой агента пользователя.

В следующем коде включается `SampleAsyncPageFilter` в классе `Startup`:

[!code-csharp[Main](filter/3.1sample/PageFilter/Startup.cs?name=snippet2)]

Следующий код вызывает <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*>, чтобы применить `SampleAsyncPageFilter` только к страницам в */Movies*:

[!code-csharp[Main](filter/3.1sample/PageFilter/Startup2.cs?name=snippet2)]

В следующем коде реализуется синхронный метод `IPageFilter`:

[!code-csharp[Main](filter/3.1sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

В следующем коде включается `SamplePageFilter`:

[!code-csharp[Main](filter/3.1sample/PageFilter/StartupSync.cs?name=snippet2)]

## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a>Реализация фильтров страницы Razor путем переопределения методов фильтра

Следующий код переопределяет асинхронные фильтры страниц Razor:

[!code-csharp[Main](filter/3.1sample/PageFilter/Pages/Index.cshtml.cs?name=snippet)]

<a name="ifa"></a>

## <a name="implement-a-filter-attribute"></a>Реализация атрибута фильтра

Встроенный фильтр, основанный на атрибутах <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter.OnResultExecutionAsync*> фильтре, может быть подклассом. Следующий фильтр добавляет заголовок к ответу:

[!code-csharp[Main](filter/3.1sample/PageFilter/Filters/AddHeaderAttribute.cs)]

В следующем коде применяется атрибут `AddHeader`:

[!code-csharp[Main](filter/3.1sample/PageFilter/Pages/Movies/Test.cshtml.cs)]

Для изучения заголовков используйте средство разработчика браузера. В **заголовке ответа** отображается `author: Rick`.

Инструкции по переопределению порядка см. в разделе [Переопределение порядка по умолчанию](xref:mvc/controllers/filters#overriding-the-default-order).

Инструкции по замыканию конвейера фильтров из фильтра см. в разделе [Отмена и замыкание](xref:mvc/controllers/filters#cancellation-and-short-circuiting).

<a name="auth"></a>

## <a name="authorize-filter-attribute"></a>Атрибут фильтра Authorize

Атрибут [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) можно применить к `PageModel`:

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)

Фильтры Razor Pages [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) и [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) разрешают Razor Pages выполнять код до и после запуска обработчика Razor Pages. Фильтры страницы Razor похожи на [фильтры действий MVC ASP.NET Core](xref:mvc/controllers/filters#action-filters), но их нельзя применять к методам обработчика отдельной страницы.

Фильтры страницы Razor:

* Запускают код после выбора метода обработчика, но до привязки модели.
* Запускают код до выполнения метода обработчика, но после привязки модели.
* Запускают код после выполнения метода обработчика.
* Могут реализовываться глобально или на странице.
* Не могут применяться к методам обработчика для конкретной страницы.

Код может быть запущен до выполнения метода обработчика с помощью конструктора страницы или ПО промежуточного слоя, но только фильтры страницы Razor имеют доступ к [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext). Фильтры имеют производный параметр [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0), который предоставляет доступ к `HttpContext`. Например, образец [Применение атрибута фильтра](#ifa) добавляет заголовок к ответу. Это невозможно сделать с помощью конструкторов или ПО промежуточного слоя.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/filter/sample/PageFilter) ([как скачивать](xref:index#how-to-download-a-sample))

Фильтры страницы Razor предоставляют следующие методы, которые могут применяться глобально или на уровне страницы:

* Синхронные методы:

  * [OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0). Вызывается после выбора метода обработчика, но до привязки модели.
  * [OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0). Вызывается до выполнения метода обработчика, но после привязки модели.
  * [OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0). Вызывается после выполнения метода обработчика, но до результата действия.

* Асинхронные методы:

  * [OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0). Вызывается асинхронно после выбора метода обработчика, но до привязки модели.
  * [OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0). Вызывается асинхронно до вызова метода обработчика, но после привязки модели.

> [!NOTE]
> Реализуйте **либо** синхронный, либо асинхронный вариант интерфейса фильтра, но не оба варианта. Платформа сначала проверяет, реализует ли фильтр асинхронный интерфейс. Если да, вызывается он. В противном случае вызываются методы синхронного интерфейса. Если реализуются оба интерфейса, вызываются только асинхронные методы. Это же правило применяется к переопределению на страницах — реализуйте синхронную или асинхронную версию переопределения, но не обе сразу.

## <a name="implement-razor-page-filters-globally"></a>Реализации фильтров страницы Razor глобально

В следующем коде реализуется фильтр `IAsyncPageFilter`:

[!code-csharp[Main](filter/sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

В приведенном выше коде [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) не является обязательным. Он используется в образце для предоставления сведений о трассировке для приложения.

В следующем коде включается `SampleAsyncPageFilter` в классе `Startup`:

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet2&highlight=11)]

В следующем коде демонстрируется полный класс `Startup`:

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet1)]

Следующий код вызывает `AddFolderApplicationModelConvention` для применения `SampleAsyncPageFilter` только к страницам в */subFolder*:

[!code-csharp[Main](filter/sample/PageFilter/Startup2.cs?name=snippet2)]

В следующем коде реализуется синхронный метод `IPageFilter`:

[!code-csharp[Main](filter/sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

В следующем коде включается `SamplePageFilter`:

[!code-csharp[Main](filter/sample/PageFilter/StartupSync.cs?name=snippet2&highlight=11)]

## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a>Реализация фильтров страницы Razor путем переопределения методов фильтра

В следующем коде переопределяются синхронные фильтры страницы Razor:

[!code-csharp[Main](filter/sample/PageFilter/Pages/Index.cshtml.cs)]

<a name="ifa"></a>

## <a name="implement-a-filter-attribute"></a>Реализация атрибута фильтра

Встроенный фильтр на основе атрибутов [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) может быть разделен на подклассы. Следующий фильтр добавляет заголовок к ответу:

[!code-csharp[Main](filter/sample/PageFilter/Filters/AddHeaderAttribute.cs)]

В следующем коде применяется атрибут `AddHeader`:

[!code-csharp[Main](filter/sample/PageFilter/Pages/Contact.cshtml.cs?name=snippet1)]

Инструкции по переопределению порядка см. в разделе [Переопределение порядка по умолчанию](xref:mvc/controllers/filters#overriding-the-default-order).

Инструкции по замыканию конвейера фильтров из фильтра см. в разделе [Отмена и замыкание](xref:mvc/controllers/filters#cancellation-and-short-circuiting). 

<a name="auth"></a>

## <a name="authorize-filter-attribute"></a>Атрибут фильтра Authorize

Атрибут [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) можно применить к `PageModel`:

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]

::: moniker-end
