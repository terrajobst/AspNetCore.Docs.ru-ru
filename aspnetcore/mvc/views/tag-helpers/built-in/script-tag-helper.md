---
title: Вспомогательная функция тега сценария в ASP.NET Core
author: rick-anderson
ms.author: riande
description: Обнаруживайте атрибуты вспомогательной функции тега сценария ASP.NET Core и роль, которую играет каждый атрибут в расширении поведения тега сценария HTML.
ms.custom: mvc
ms.date: 12/02/2019
uid: mvc/views/tag-helpers/builtin-th/script-tag-helper
ms.openlocfilehash: a037abb6a454e6d06305e7d7f6ecad0c2a0ca717
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2020
ms.locfileid: "77171845"
---
# <a name="script-tag-helper-in-aspnet-core"></a><span data-ttu-id="991f4-103">Вспомогательная функция тега сценария в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="991f4-103">Script Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="991f4-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="991f4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="991f4-105">[Вспомогательная функция тега сценария](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) создает ссылку на первичный или резервный файл сценария.</span><span class="sxs-lookup"><span data-stu-id="991f4-105">The [Script Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) generates a link to a primary or fall back script file.</span></span> <span data-ttu-id="991f4-106">Обычно первичный файл сценария находится в [сети доставки содержимого](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).</span><span class="sxs-lookup"><span data-stu-id="991f4-106">Typically the primary script file is on a [Content Delivery Network](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).</span></span>

[!INCLUDE[](~/includes/cdn.md)]

<span data-ttu-id="991f4-107">Вспомогательная функция тега сценария позволяет указать CDN для файла сценария и резервную копию, если CDN недоступен.</span><span class="sxs-lookup"><span data-stu-id="991f4-107">The Script Tag Helper allows you to specify a CDN for the script file and a fallback when the CDN is not available.</span></span> <span data-ttu-id="991f4-108">Вспомогательная функция тега сценария обеспечивает высокую производительность CDN с надежностью локального размещения.</span><span class="sxs-lookup"><span data-stu-id="991f4-108">The Script Tag Helper provides the performance advantage of a CDN with the robustness of local hosting.</span></span>

<span data-ttu-id="991f4-109">В следующей разметке Razor показан элемент `script` с резервной копией:</span><span class="sxs-lookup"><span data-stu-id="991f4-109">The following Razor markup shows a `script` element with a fallback:</span></span>

```html
<script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-3.3.1.min.js"
        asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
        asp-fallback-test="window.jQuery"
        crossorigin="anonymous"
        integrity="sha384-tsQFqpEReu7ZLhBV2VZlAu7zcOV+rXbYlF2cqB8txI/8aZajjp4Bqd+V6D5IgvKT">
</script>
```

<span data-ttu-id="991f4-110">Не используйте атрибут [defer](https://developer.mozilla.org/docs/Web/HTML/Element/script) элемента `<script>`, чтобы отложить загрузку скрипта CDN.</span><span class="sxs-lookup"><span data-stu-id="991f4-110">Don't use the `<script>` element's [defer](https://developer.mozilla.org/docs/Web/HTML/Element/script) attribute to defer loading the CDN script.</span></span> <span data-ttu-id="991f4-111">Вспомогательная функция тега скрипта обрабатывает код JavaScript, который сразу же выполняет выражение [asp-fallback-test](#asp-fallback-test).</span><span class="sxs-lookup"><span data-stu-id="991f4-111">The Script Tag Helper renders JavaScript that immediately executes the [asp-fallback-test](#asp-fallback-test) expression.</span></span> <span data-ttu-id="991f4-112">Это выражение дает сбой, если загрузка скрипта CDN отложена.</span><span class="sxs-lookup"><span data-stu-id="991f4-112">The expression fails if loading the CDN script is deferred.</span></span>

## <a name="commonly-used-script-tag-helper-attributes"></a><span data-ttu-id="991f4-113">Часто используемые атрибуты вспомогательной функции тега сценария</span><span class="sxs-lookup"><span data-stu-id="991f4-113">Commonly used Script Tag Helper attributes</span></span>

<span data-ttu-id="991f4-114">Все атрибуты, свойства и методы вспомогательной функции тега сценария см. в статье [ScriptTagHelper Class](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) (Класс ScriptTagHelper).</span><span class="sxs-lookup"><span data-stu-id="991f4-114">See [Script Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) for all the Script Tag Helper attributes, properties, and methods.</span></span>

### <a name="asp-fallback-test"></a><span data-ttu-id="991f4-115">asp-fallback-test</span><span class="sxs-lookup"><span data-stu-id="991f4-115">asp-fallback-test</span></span>

<span data-ttu-id="991f4-116">Метод скрипта, определенный в основном скрипте, для использования в тесте резервного экземпляра.</span><span class="sxs-lookup"><span data-stu-id="991f4-116">The script method defined in the primary script to use for the fallback test.</span></span> <span data-ttu-id="991f4-117">Для получения дополнительной информации см. <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper.FallbackTestExpression>.</span><span class="sxs-lookup"><span data-stu-id="991f4-117">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper.FallbackTestExpression>.</span></span>

### <a name="asp-fallback-src"></a><span data-ttu-id="991f4-118">asp-fallback-src</span><span class="sxs-lookup"><span data-stu-id="991f4-118">asp-fallback-src</span></span>

<span data-ttu-id="991f4-119">URL-адрес тега Script, на который можно перейти в случае сбоя основного URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="991f4-119">The URL of a Script tag to fallback to in the case the primary one fails.</span></span> <span data-ttu-id="991f4-120">Для получения дополнительной информации см. <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper.FallbackSrc>.</span><span class="sxs-lookup"><span data-stu-id="991f4-120">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper.FallbackSrc>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="991f4-121">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="991f4-121">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
