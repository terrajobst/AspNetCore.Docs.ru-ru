---
title: Вспомогательная функция тега частичного представления в ASP.NET Core
author: scottaddie
description: Сведения о вспомогательной функции тега частичного представления в ASP.NET и роли каждого из его атрибутов в отрисовке частичного представления.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 04/13/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/partial-tag-helper
ms.openlocfilehash: 670663b963f4207da793afff44d55b85ba58b7f8
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2018
---
# <a name="partial-tag-helper-in-aspnet-core"></a><span data-ttu-id="56f52-103">Вспомогательная функция тега частичного представления в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56f52-103">Partial Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="56f52-104">Автор: [Скотт Адди](https://github.com/scottaddie) (Scott Addie)</span><span class="sxs-lookup"><span data-stu-id="56f52-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

<span data-ttu-id="56f52-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="56f52-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="overview"></a><span data-ttu-id="56f52-106">Обзор</span><span class="sxs-lookup"><span data-stu-id="56f52-106">Overview</span></span>

<span data-ttu-id="56f52-107">Вспомогательная функция тега частичного представления используется для отрисовки [частичного представления](xref:mvc/views/partial) в приложениях Razor Pages и MVC.</span><span class="sxs-lookup"><span data-stu-id="56f52-107">The Partial Tag Helper is used for rendering a [partial view](xref:mvc/views/partial) in Razor Pages and MVC apps.</span></span> <span data-ttu-id="56f52-108">Примечания.</span><span class="sxs-lookup"><span data-stu-id="56f52-108">Consider that it:</span></span>

* <span data-ttu-id="56f52-109">Требуется ASP.NET Core 2.1 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="56f52-109">Requires ASP.NET Core 2.1 or later.</span></span>
* <span data-ttu-id="56f52-110">Это альтернатива [синтаксиса вспомогательной функции HTML](xref:mvc/views/partial#referencing-a-partial-view).</span><span class="sxs-lookup"><span data-stu-id="56f52-110">Is an alternative to [HTML Helper syntax](xref:mvc/views/partial#referencing-a-partial-view).</span></span>
* <span data-ttu-id="56f52-111">Отрисовка частичного представления выполняется асинхронно.</span><span class="sxs-lookup"><span data-stu-id="56f52-111">Renders the partial view asynchronously.</span></span>

<span data-ttu-id="56f52-112">Параметры вспомогательной функции HTML для отрисовки частичного представления:</span><span class="sxs-lookup"><span data-stu-id="56f52-112">The HTML Helper options for rendering a partial view include:</span></span>

* [<span data-ttu-id="56f52-113">@await Html.PartialAsync</span><span class="sxs-lookup"><span data-stu-id="56f52-113">@await Html.PartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync)
* [<span data-ttu-id="56f52-114">@await Html.RenderPartialAsync</span><span class="sxs-lookup"><span data-stu-id="56f52-114">@await Html.RenderPartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync)
* [@Html.Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial)
* [@Html.RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial)

<span data-ttu-id="56f52-115">В примерах в этом документе используется модель *Product*:</span><span class="sxs-lookup"><span data-stu-id="56f52-115">The *Product* model is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Models/Product.cs)]

<span data-ttu-id="56f52-116">Далее следуют атрибуты вспомогательной функции тега частичного представления.</span><span class="sxs-lookup"><span data-stu-id="56f52-116">An inventory of the Partial Tag Helper attributes follows.</span></span>

## <a name="name"></a><span data-ttu-id="56f52-117">имя</span><span class="sxs-lookup"><span data-stu-id="56f52-117">name</span></span>

<span data-ttu-id="56f52-118">Атрибут `name` является обязательным.</span><span class="sxs-lookup"><span data-stu-id="56f52-118">The `name` attribute is required.</span></span> <span data-ttu-id="56f52-119">Она указывает имя или путь для отображения частичного представления, которое будет отрисовано.</span><span class="sxs-lookup"><span data-stu-id="56f52-119">It indicates the name or the path of the partial view to be rendered.</span></span> <span data-ttu-id="56f52-120">Если указано имя частичного представления, запускается процесс [обнаружения представления](xref:mvc/views/overview#view-discovery).</span><span class="sxs-lookup"><span data-stu-id="56f52-120">When a partial view name is provided, the [view discovery](xref:mvc/views/overview#view-discovery) process is initiated.</span></span> <span data-ttu-id="56f52-121">Этот процесс пропускается, если предоставлен явный путь.</span><span class="sxs-lookup"><span data-stu-id="56f52-121">That process is bypassed when an explicit path is provided.</span></span>

<span data-ttu-id="56f52-122">В приведенной ниже разметке используется явный путь, указывающий, что файл *_ProductPartial.cshtml* необходимо загрузить из папки *Shared*.</span><span class="sxs-lookup"><span data-stu-id="56f52-122">The following markup uses an explicit path, indicating that *_ProductPartial.cshtml* is to be loaded from the *Shared* folder.</span></span> <span data-ttu-id="56f52-123">При использовании атрибута [for](#for) модель передается в частичное представление для привязки.</span><span class="sxs-lookup"><span data-stu-id="56f52-123">Using the [for](#for) attribute, a model is passed to the partial view for binding.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Name)]

## <a name="for"></a><span data-ttu-id="56f52-124">for</span><span class="sxs-lookup"><span data-stu-id="56f52-124">for</span></span>

<span data-ttu-id="56f52-125">Атрибут `for` назначает [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) для сравнения с текущей моделью.</span><span class="sxs-lookup"><span data-stu-id="56f52-125">The `for` attribute assigns a [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) to be evaluated against the current model.</span></span> <span data-ttu-id="56f52-126">Класс `ModelExpression` выводит синтаксис `@Model.`.</span><span class="sxs-lookup"><span data-stu-id="56f52-126">A `ModelExpression` infers the `@Model.` syntax.</span></span> <span data-ttu-id="56f52-127">Например, можно использовать `for="Product"` вместо `for="@Model.Product"`.</span><span class="sxs-lookup"><span data-stu-id="56f52-127">For example, `for="Product"` can be used instead of `for="@Model.Product"`.</span></span> <span data-ttu-id="56f52-128">Это поведение вывода по умолчанию переопределяется с помощью символа `@` для определения встроенного выражения.</span><span class="sxs-lookup"><span data-stu-id="56f52-128">This default inference behavior is overridden by using the `@` symbol to define an inline expression.</span></span> <span data-ttu-id="56f52-129">Атрибут `for` нельзя использовать с атрибутом [model](#model).</span><span class="sxs-lookup"><span data-stu-id="56f52-129">The `for` attribute can't be used with the [model](#model) attribute.</span></span>

<span data-ttu-id="56f52-130">Приведенная ниже разметка загружает *_ProductPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="56f52-130">The following markup loads *_ProductPartial.cshtml*:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_For)]

<span data-ttu-id="56f52-131">Частичное представление привязывается к свойству `Product` соответствующей модели страницы:</span><span class="sxs-lookup"><span data-stu-id="56f52-131">The partial view is bound to the associated page model's `Product` property:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Product.cshtml.cs?highlight=8)]

## <a name="model"></a><span data-ttu-id="56f52-132">model</span><span class="sxs-lookup"><span data-stu-id="56f52-132">model</span></span>

<span data-ttu-id="56f52-133">Атрибут `model` присваивает экземпляр модели для передачи в частичное представление.</span><span class="sxs-lookup"><span data-stu-id="56f52-133">The `model` attribute assigns a model instance to pass to the partial view.</span></span> <span data-ttu-id="56f52-134">Атрибут `model` нельзя использовать с атрибутом [for](#for).</span><span class="sxs-lookup"><span data-stu-id="56f52-134">The `model` attribute can't be used with the [for](#for) attribute.</span></span>

<span data-ttu-id="56f52-135">В следующей разметке создается экземпляр нового объекта `Product`, который передается атрибуту `model` для привязки:</span><span class="sxs-lookup"><span data-stu-id="56f52-135">In the following markup, a new `Product` object is instantiated and passed to the `model` attribute for binding:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Model)]

## <a name="view-data"></a><span data-ttu-id="56f52-136">view-data</span><span class="sxs-lookup"><span data-stu-id="56f52-136">view-data</span></span>

<span data-ttu-id="56f52-137">Атрибут `view-data` присваивает [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) для передачи в частичное представление.</span><span class="sxs-lookup"><span data-stu-id="56f52-137">The `view-data` attribute assigns a [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) to pass to the partial view.</span></span> <span data-ttu-id="56f52-138">В следующей разметке вся коллекция ViewData становится доступной для частичного представления:</span><span class="sxs-lookup"><span data-stu-id="56f52-138">The following markup makes the entire ViewData collection accessible to the partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_ViewData&highlight=5-)]

<span data-ttu-id="56f52-139">В приведенном выше коде для значения ключа `IsNumberReadOnly` устанавливается `true`, и значение добавляется в коллекцию ViewData.</span><span class="sxs-lookup"><span data-stu-id="56f52-139">In the preceding code, the `IsNumberReadOnly` key value is set to `true` and added to the ViewData collection.</span></span> <span data-ttu-id="56f52-140">Следовательно, `ViewData["IsNumberReadOnly"]` становится доступно в следующем частичном представлении:</span><span class="sxs-lookup"><span data-stu-id="56f52-140">Consequently, `ViewData["IsNumberReadOnly"]` is made accessible within the following partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Shared/_ProductViewDataPartial.cshtml?highlight=5)]

<span data-ttu-id="56f52-141">В этом примере значение `ViewData["IsNumberReadOnly"]` определяет, будет ли поле *Number* отображаться только для просмотра.</span><span class="sxs-lookup"><span data-stu-id="56f52-141">In this example, the value of `ViewData["IsNumberReadOnly"]` determines whether the *Number* field is displayed as read only.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="56f52-142">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="56f52-142">Additional resources</span></span>

* [<span data-ttu-id="56f52-143">Частичные представления</span><span class="sxs-lookup"><span data-stu-id="56f52-143">Partial views</span></span>](xref:mvc/views/partial)
* [<span data-ttu-id="56f52-144">Нестрого типизированные данные (ViewData и ViewBag)</span><span class="sxs-lookup"><span data-stu-id="56f52-144">Weakly typed data (ViewData and ViewBag)</span></span>](xref:mvc/views/overview#weakly-typed-data-viewdata-and-viewbag)
