---
title: Вспомогательная функция тега частичного представления в ASP.NET Core
author: scottaddie
description: Сведения о вспомогательной функции тега частичного представления в ASP.NET и роли каждого из его атрибутов в отрисовке частичного представления.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 07/25/2018
uid: mvc/views/tag-helpers/builtin-th/partial-tag-helper
ms.openlocfilehash: bcaeb3ba286feb55c0755c5baee4b7608710e647
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207788"
---
# <a name="partial-tag-helper-in-aspnet-core"></a><span data-ttu-id="dc841-103">Вспомогательная функция тега частичного представления в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dc841-103">Partial Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="dc841-104">Автор: [Скотт Адди](https://github.com/scottaddie) (Scott Addie)</span><span class="sxs-lookup"><span data-stu-id="dc841-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="dc841-105">Общие сведения о вспомогательных функциях тегов см. здесь: <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="dc841-105">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

<span data-ttu-id="dc841-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="dc841-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview"></a><span data-ttu-id="dc841-107">Обзор</span><span class="sxs-lookup"><span data-stu-id="dc841-107">Overview</span></span>

<span data-ttu-id="dc841-108">Вспомогательная функция тега частичного представления используется для отрисовки [частичного представления](xref:mvc/views/partial) в приложениях Razor Pages и MVC.</span><span class="sxs-lookup"><span data-stu-id="dc841-108">The Partial Tag Helper is used for rendering a [partial view](xref:mvc/views/partial) in Razor Pages and MVC apps.</span></span> <span data-ttu-id="dc841-109">Примечания.</span><span class="sxs-lookup"><span data-stu-id="dc841-109">Consider that it:</span></span>

* <span data-ttu-id="dc841-110">Требуется ASP.NET Core 2.1 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="dc841-110">Requires ASP.NET Core 2.1 or later.</span></span>
* <span data-ttu-id="dc841-111">Это альтернатива [синтаксиса вспомогательной функции HTML](xref:mvc/views/partial#reference-a-partial-view).</span><span class="sxs-lookup"><span data-stu-id="dc841-111">Is an alternative to [HTML Helper syntax](xref:mvc/views/partial#reference-a-partial-view).</span></span>
* <span data-ttu-id="dc841-112">Отрисовка частичного представления выполняется асинхронно.</span><span class="sxs-lookup"><span data-stu-id="dc841-112">Renders the partial view asynchronously.</span></span>

<span data-ttu-id="dc841-113">Параметры вспомогательной функции HTML для отрисовки частичного представления:</span><span class="sxs-lookup"><span data-stu-id="dc841-113">The HTML Helper options for rendering a partial view include:</span></span>

* [<span data-ttu-id="dc841-114">@await Html.PartialAsync</span><span class="sxs-lookup"><span data-stu-id="dc841-114">@await Html.PartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync)
* [<span data-ttu-id="dc841-115">@await Html.RenderPartialAsync</span><span class="sxs-lookup"><span data-stu-id="dc841-115">@await Html.RenderPartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync)
* [@Html.Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial)
* [@Html.RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial)

<span data-ttu-id="dc841-116">В примерах в этом документе используется модель *Product*:</span><span class="sxs-lookup"><span data-stu-id="dc841-116">The *Product* model is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Models/Product.cs)]

<span data-ttu-id="dc841-117">Далее следуют атрибуты вспомогательной функции тега частичного представления.</span><span class="sxs-lookup"><span data-stu-id="dc841-117">An inventory of the Partial Tag Helper attributes follows.</span></span>

## <a name="name"></a><span data-ttu-id="dc841-118">имя</span><span class="sxs-lookup"><span data-stu-id="dc841-118">name</span></span>

<span data-ttu-id="dc841-119">Атрибут `name` является обязательным.</span><span class="sxs-lookup"><span data-stu-id="dc841-119">The `name` attribute is required.</span></span> <span data-ttu-id="dc841-120">Она указывает имя или путь для отображения частичного представления, которое будет отрисовано.</span><span class="sxs-lookup"><span data-stu-id="dc841-120">It indicates the name or the path of the partial view to be rendered.</span></span> <span data-ttu-id="dc841-121">Если указано имя частичного представления, запускается процесс [обнаружения представления](xref:mvc/views/overview#view-discovery).</span><span class="sxs-lookup"><span data-stu-id="dc841-121">When a partial view name is provided, the [view discovery](xref:mvc/views/overview#view-discovery) process is initiated.</span></span> <span data-ttu-id="dc841-122">Этот процесс пропускается, если предоставлен явный путь.</span><span class="sxs-lookup"><span data-stu-id="dc841-122">That process is bypassed when an explicit path is provided.</span></span> <span data-ttu-id="dc841-123">Все допустимые значения `name` см. в разделе [Обнаружение частичного представления](xref:mvc/views/partial#partial-view-discovery).</span><span class="sxs-lookup"><span data-stu-id="dc841-123">For all acceptable `name` values, see [Partial view discovery](xref:mvc/views/partial#partial-view-discovery).</span></span>

<span data-ttu-id="dc841-124">В приведенной ниже разметке используется явный путь, указывающий, что файл *_ProductPartial.cshtml* необходимо загрузить из папки *Shared*.</span><span class="sxs-lookup"><span data-stu-id="dc841-124">The following markup uses an explicit path, indicating that *_ProductPartial.cshtml* is to be loaded from the *Shared* folder.</span></span> <span data-ttu-id="dc841-125">При использовании атрибута [for](#for) модель передается в частичное представление для привязки.</span><span class="sxs-lookup"><span data-stu-id="dc841-125">Using the [for](#for) attribute, a model is passed to the partial view for binding.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Name)]

## <a name="for"></a><span data-ttu-id="dc841-126">for</span><span class="sxs-lookup"><span data-stu-id="dc841-126">for</span></span>

<span data-ttu-id="dc841-127">Атрибут `for` назначает [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) для сравнения с текущей моделью.</span><span class="sxs-lookup"><span data-stu-id="dc841-127">The `for` attribute assigns a [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) to be evaluated against the current model.</span></span> <span data-ttu-id="dc841-128">Класс `ModelExpression` выводит синтаксис `@Model.`.</span><span class="sxs-lookup"><span data-stu-id="dc841-128">A `ModelExpression` infers the `@Model.` syntax.</span></span> <span data-ttu-id="dc841-129">Например, можно использовать `for="Product"` вместо `for="@Model.Product"`.</span><span class="sxs-lookup"><span data-stu-id="dc841-129">For example, `for="Product"` can be used instead of `for="@Model.Product"`.</span></span> <span data-ttu-id="dc841-130">Это поведение вывода по умолчанию переопределяется с помощью символа `@` для определения встроенного выражения.</span><span class="sxs-lookup"><span data-stu-id="dc841-130">This default inference behavior is overridden by using the `@` symbol to define an inline expression.</span></span> <span data-ttu-id="dc841-131">Атрибут `for` нельзя использовать с атрибутом [model](#model).</span><span class="sxs-lookup"><span data-stu-id="dc841-131">The `for` attribute can't be used with the [model](#model) attribute.</span></span>

<span data-ttu-id="dc841-132">Приведенная ниже разметка загружает *_ProductPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="dc841-132">The following markup loads *_ProductPartial.cshtml*:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_For)]

<span data-ttu-id="dc841-133">Частичное представление привязывается к свойству `Product` соответствующей модели страницы:</span><span class="sxs-lookup"><span data-stu-id="dc841-133">The partial view is bound to the associated page model's `Product` property:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Product.cshtml.cs?highlight=8)]

## <a name="model"></a><span data-ttu-id="dc841-134">model</span><span class="sxs-lookup"><span data-stu-id="dc841-134">model</span></span>

<span data-ttu-id="dc841-135">Атрибут `model` присваивает экземпляр модели для передачи в частичное представление.</span><span class="sxs-lookup"><span data-stu-id="dc841-135">The `model` attribute assigns a model instance to pass to the partial view.</span></span> <span data-ttu-id="dc841-136">Атрибут `model` нельзя использовать с атрибутом [for](#for).</span><span class="sxs-lookup"><span data-stu-id="dc841-136">The `model` attribute can't be used with the [for](#for) attribute.</span></span>

<span data-ttu-id="dc841-137">В следующей разметке создается экземпляр нового объекта `Product`, который передается атрибуту `model` для привязки:</span><span class="sxs-lookup"><span data-stu-id="dc841-137">In the following markup, a new `Product` object is instantiated and passed to the `model` attribute for binding:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Model)]

## <a name="view-data"></a><span data-ttu-id="dc841-138">view-data</span><span class="sxs-lookup"><span data-stu-id="dc841-138">view-data</span></span>

<span data-ttu-id="dc841-139">Атрибут `view-data` присваивает [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) для передачи в частичное представление.</span><span class="sxs-lookup"><span data-stu-id="dc841-139">The `view-data` attribute assigns a [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) to pass to the partial view.</span></span> <span data-ttu-id="dc841-140">В следующей разметке вся коллекция ViewData становится доступной для частичного представления:</span><span class="sxs-lookup"><span data-stu-id="dc841-140">The following markup makes the entire ViewData collection accessible to the partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_ViewData&highlight=5-)]

<span data-ttu-id="dc841-141">В приведенном выше коде для значения ключа `IsNumberReadOnly` устанавливается `true`, и значение добавляется в коллекцию ViewData.</span><span class="sxs-lookup"><span data-stu-id="dc841-141">In the preceding code, the `IsNumberReadOnly` key value is set to `true` and added to the ViewData collection.</span></span> <span data-ttu-id="dc841-142">Следовательно, `ViewData["IsNumberReadOnly"]` становится доступно в следующем частичном представлении:</span><span class="sxs-lookup"><span data-stu-id="dc841-142">Consequently, `ViewData["IsNumberReadOnly"]` is made accessible within the following partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Shared/_ProductViewDataPartial.cshtml?highlight=5)]

<span data-ttu-id="dc841-143">В этом примере значение `ViewData["IsNumberReadOnly"]` определяет, будет ли поле *Number* отображаться только для просмотра.</span><span class="sxs-lookup"><span data-stu-id="dc841-143">In this example, the value of `ViewData["IsNumberReadOnly"]` determines whether the *Number* field is displayed as read only.</span></span>

## <a name="migrate-from-an-html-helper"></a><span data-ttu-id="dc841-144">Перенос из вспомогательного метода HTML</span><span class="sxs-lookup"><span data-stu-id="dc841-144">Migrate from an HTML Helper</span></span>

<span data-ttu-id="dc841-145">Рассмотрим следующий пример асинхронного вспомогательного метода HTML.</span><span class="sxs-lookup"><span data-stu-id="dc841-145">Consider the following asynchronous HTML Helper example.</span></span> <span data-ttu-id="dc841-146">Выполняется итерация и отображение коллекции продуктов.</span><span class="sxs-lookup"><span data-stu-id="dc841-146">A collection of products is iterated and displayed.</span></span> <span data-ttu-id="dc841-147">Для первого параметра метода `PartialAsync` загружается частичное представление *_ProductPartial.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="dc841-147">Per the `PartialAsync` method's first parameter, the *_ProductPartial.cshtml* partial view is loaded.</span></span> <span data-ttu-id="dc841-148">Экземпляр модели `Product` передается в частичное представление для привязки.</span><span class="sxs-lookup"><span data-stu-id="dc841-148">An instance of the `Product` model is passed to the partial view for binding.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Products.cshtml?name=snippet_HtmlHelper&highlight=3)]

<span data-ttu-id="dc841-149">Следующая вспомогательная функция тега частичного представления обеспечивает такую же асинхронную подготовку к просмотру, что и вспомогательный метод HTML `PartialAsync`.</span><span class="sxs-lookup"><span data-stu-id="dc841-149">The following Partial Tag Helper achieves the same asynchronous rendering behavior as the `PartialAsync` HTML Helper.</span></span> <span data-ttu-id="dc841-150">Атрибуту `model` назначен экземпляр модели `Product` для привязки к частичному представлению.</span><span class="sxs-lookup"><span data-stu-id="dc841-150">The `model` attribute is assigned a `Product` model instance for binding to the partial view.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Products.cshtml?name=snippet_TagHelper&highlight=3)]

## <a name="additional-resources"></a><span data-ttu-id="dc841-151">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="dc841-151">Additional resources</span></span>

* <xref:mvc/views/partial>
* <xref:mvc/views/overview#weakly-typed-data-viewdata-viewdata-attribute-and-viewbag>
