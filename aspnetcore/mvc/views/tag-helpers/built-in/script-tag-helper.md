---
title: Вспомогательная функция тега сценария в ASP.NET Core
author: rick-anderson
ms.author: riande
description: Обнаруживайте атрибуты вспомогательной функции тега сценария ASP.NET Core и роль, которую играет каждый атрибут в расширении поведения тега сценария HTML.
ms.custom: mvc
ms.date: 12/18/2018
uid: mvc/views/tag-helpers/builtin-th/script-tag-helper
ms.openlocfilehash: 5f2fb8a45048804afa8aff2989cd53489e45a33b
ms.sourcegitcommit: fae6f0e253f9d62d8f39de5884d2ba2b4b2a6050
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2019
ms.locfileid: "71256468"
---
# <a name="script-tag-helper-in-aspnet-core"></a><span data-ttu-id="e7715-103">Вспомогательная функция тега сценария в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e7715-103">Script Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="e7715-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="e7715-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e7715-105">[Вспомогательная функция тега сценария](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) создает ссылку на первичный или резервный файл сценария.</span><span class="sxs-lookup"><span data-stu-id="e7715-105">The [Script Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) generates a link to a primary or fall back script file.</span></span> <span data-ttu-id="e7715-106">Обычно первичный файл сценария находится в [сети доставки содержимого](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).</span><span class="sxs-lookup"><span data-stu-id="e7715-106">Typically the primary script file is on a [Content Delivery Network](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).</span></span>

[!INCLUDE[](~/includes/cdn.md)]

<span data-ttu-id="e7715-107">Вспомогательная функция тега сценария позволяет указать CDN для файла сценария и резервную копию, если CDN недоступен.</span><span class="sxs-lookup"><span data-stu-id="e7715-107">The Script Tag Helper allows you to specify a CDN for the script file and a fallback when the CDN is not available.</span></span> <span data-ttu-id="e7715-108">Вспомогательная функция тега сценария обеспечивает высокую производительность CDN с надежностью локального размещения.</span><span class="sxs-lookup"><span data-stu-id="e7715-108">The Script Tag Helper provides the performance advantage of a CDN with the robustness of local hosting.</span></span>

<span data-ttu-id="e7715-109">В следующей разметке Razor показан элемент `script` файла макета, созданного с помощью шаблона веб-приложения ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="e7715-109">The following Razor markup shows the `script` element of a layout file created with the ASP.NET Core web app template:</span></span>

[!code-html[](link-tag-helper/sample/_Layout.cshtml?name=snippet2)]

<span data-ttu-id="e7715-110">Следующий код похож на отображаемый HTML из предыдущего кода (в среде, не являющейся средой разработки):</span><span class="sxs-lookup"><span data-stu-id="e7715-110">The following is similar to the rendered HTML from the preceding code (in a non-Development environment):</span></span>

[!code-csharp[](link-tag-helper/sample/HtmlPage2.html)]

<span data-ttu-id="e7715-111">В приведенном выше коде вспомогательная функция тега сценария создала второй элемент сценария (`<script>  (window.jQuery || document.write(`), который проверяет на наличие `window.jQuery`.</span><span class="sxs-lookup"><span data-stu-id="e7715-111">In the preceding code, the Script Tag Helper generated the second script ( `<script>  (window.jQuery || document.write(`) element, which tests for `window.jQuery`.</span></span> <span data-ttu-id="e7715-112">Если `window.jQuery` не найден, `document.write(` выполняется и создает сценарий.</span><span class="sxs-lookup"><span data-stu-id="e7715-112">If `window.jQuery` is not found, `document.write(` runs and creates a script</span></span> 

## <a name="commonly-used-script-tag-helper-attributes"></a><span data-ttu-id="e7715-113">Часто используемые атрибуты вспомогательной функции тега сценария</span><span class="sxs-lookup"><span data-stu-id="e7715-113">Commonly used Script Tag Helper attributes</span></span>

<span data-ttu-id="e7715-114">Все атрибуты, свойства и методы вспомогательной функции тега сценария см. в статье [ScriptTagHelper Class](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) (Класс ScriptTagHelper).</span><span class="sxs-lookup"><span data-stu-id="e7715-114">See [Script Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) for all the Script Tag Helper attributes, properties, and methods.</span></span>

### <a name="href"></a><span data-ttu-id="e7715-115">href</span><span class="sxs-lookup"><span data-stu-id="e7715-115">href</span></span>

<span data-ttu-id="e7715-116">Предпочтительный адрес связанного ресурса.</span><span class="sxs-lookup"><span data-stu-id="e7715-116">Preferred address of the linked resource.</span></span> <span data-ttu-id="e7715-117">Адрес передается в созданный код HTML во всех случаях.</span><span class="sxs-lookup"><span data-stu-id="e7715-117">The address is passed thought to the generated HTML in all cases.</span></span>

### <a name="asp-fallback-href"></a><span data-ttu-id="e7715-118">asp-fallback-href</span><span class="sxs-lookup"><span data-stu-id="e7715-118">asp-fallback-href</span></span>

<span data-ttu-id="e7715-119">URL-адрес таблицы стилей CSS, к которой можно перейти в случае сбоя основного URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="e7715-119">The URL of a CSS stylesheet to fallback to in the case the primary URL fails.</span></span>

### <a name="asp-fallback-test-class"></a><span data-ttu-id="e7715-120">asp-fallback-test-class</span><span class="sxs-lookup"><span data-stu-id="e7715-120">asp-fallback-test-class</span></span>

<span data-ttu-id="e7715-121">Имя класса, определенное в таблице стилей для использования в качестве теста резервного экземпляра.</span><span class="sxs-lookup"><span data-stu-id="e7715-121">The class name defined in the stylesheet to use for the fallback test.</span></span> <span data-ttu-id="e7715-122">Дополнительные сведения можно найти по адресу: <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestClass>.</span><span class="sxs-lookup"><span data-stu-id="e7715-122">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestClass>.</span></span>

### <a name="asp-fallback-test-property"></a><span data-ttu-id="e7715-123">asp-fallback-test-property</span><span class="sxs-lookup"><span data-stu-id="e7715-123">asp-fallback-test-property</span></span>

<span data-ttu-id="e7715-124">Имя свойства CSS, используемое для теста резервного экземпляра.</span><span class="sxs-lookup"><span data-stu-id="e7715-124">The CSS property name to use for the fallback test.</span></span> <span data-ttu-id="e7715-125">Дополнительные сведения можно найти по адресу: <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestProperty>.</span><span class="sxs-lookup"><span data-stu-id="e7715-125">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestProperty>.</span></span>

### <a name="asp-fallback-test-value"></a><span data-ttu-id="e7715-126">asp-fallback-test-value</span><span class="sxs-lookup"><span data-stu-id="e7715-126">asp-fallback-test-value</span></span>

<span data-ttu-id="e7715-127">Значение свойства CSS, используемое для теста резервного экземпляра.</span><span class="sxs-lookup"><span data-stu-id="e7715-127">The CSS property value to use for the fallback test.</span></span> <span data-ttu-id="e7715-128">Дополнительные сведения можно найти по адресу: <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>.</span><span class="sxs-lookup"><span data-stu-id="e7715-128">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>.</span></span>

### <a name="asp-fallback-test-value"></a><span data-ttu-id="e7715-129">asp-fallback-test-value</span><span class="sxs-lookup"><span data-stu-id="e7715-129">asp-fallback-test-value</span></span>

<span data-ttu-id="e7715-130">Значение свойства CSS, используемое для теста резервного экземпляра.</span><span class="sxs-lookup"><span data-stu-id="e7715-130">The CSS property value to use for the fallback test.</span></span> <span data-ttu-id="e7715-131">Дополнительные сведения см. в разделе <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>.</span><span class="sxs-lookup"><span data-stu-id="e7715-131">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue></span></span>

## <a name="additional-resources"></a><span data-ttu-id="e7715-132">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="e7715-132">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
