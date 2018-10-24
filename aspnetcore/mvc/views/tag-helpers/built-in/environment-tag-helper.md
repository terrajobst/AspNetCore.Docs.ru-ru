---
title: Вспомогательная функция тега среды в ASP.NET Core
author: pkellner
description: Определенная в ASP.NET Core вспомогательная функция тега среды, включая все свойства
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 379f58ed37329f047d53adf1dcfdfd2ad6a6ca4e
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325241"
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="62210-103">Вспомогательная функция тега среды в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="62210-103">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="62210-104">Авторы: [Питер Кельнер (Peter Kellner)](http://peterkellner.net), [Хишам Бин Атейя (Hisham Bin Ateya)](https://twitter.com/hishambinateya) и [Люк Лэтэм (Luke Latham)](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="62210-104">By [Peter Kellner](http://peterkellner.net), [Hisham Bin Ateya](https://twitter.com/hishambinateya), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="62210-105">Вспомогательная функция тега среды условно отрисовывает заключенное в нее содержимое с учетом текущей [среды размещения](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="62210-105">The Environment Tag Helper conditionally renders its enclosed content based on the current [hosting environment](xref:fundamentals/environments).</span></span> <span data-ttu-id="62210-106">Единственный атрибут вспомогательной функции тега среды, `names`, — это разделенный запятыми список имен сред.</span><span class="sxs-lookup"><span data-stu-id="62210-106">The Environment Tag Helper's single attribute, `names`, is a comma-separated list of environment names.</span></span> <span data-ttu-id="62210-107">Если одно из указанных имен среды соответствует текущей среде, включенное содержимое подготавливается к просмотру.</span><span class="sxs-lookup"><span data-stu-id="62210-107">If any of the provided environment names match the current environment, the enclosed content is rendered.</span></span>

<span data-ttu-id="62210-108">Общие сведения о вспомогательных функциях тегов см. в разделе <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="62210-108">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="62210-109">Атрибуты вспомогательной функции тега среды</span><span class="sxs-lookup"><span data-stu-id="62210-109">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="62210-110">имена</span><span class="sxs-lookup"><span data-stu-id="62210-110">names</span></span>

<span data-ttu-id="62210-111">`names` принимает одно имя среды размещения или список разделенных запятыми имен сред размещения, которые запускают отрисовку включенного в функцию содержимого.</span><span class="sxs-lookup"><span data-stu-id="62210-111">`names` accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="62210-112">Значения среды сравниваются с текущим значением, возвращенным [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="62210-112">Environment values are compared to the current value returned by [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*).</span></span> <span data-ttu-id="62210-113">При сравнении регистр не учитывается.</span><span class="sxs-lookup"><span data-stu-id="62210-113">The comparison ignores case.</span></span>

<span data-ttu-id="62210-114">В приведенном ниже примере используется вспомогательная функция тега среды.</span><span class="sxs-lookup"><span data-stu-id="62210-114">The following example uses an Environment Tag Helper.</span></span> <span data-ttu-id="62210-115">Содержимое подготавливается к просмотру в том случае, если среда размещения является промежуточной или рабочей:</span><span class="sxs-lookup"><span data-stu-id="62210-115">The content is rendered if the hosting environment is Staging or Production:</span></span>

```cshtml
<environment names="Staging,Production">
    <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

::: moniker range=">= aspnetcore-2.0"

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="62210-116">Атрибуты include и exclude</span><span class="sxs-lookup"><span data-stu-id="62210-116">include and exclude attributes</span></span>

<span data-ttu-id="62210-117">Атрибуты `include` & `exclude` управляют отрисовкой включенного содержимого на основе имен включенных или исключенных сред размещения.</span><span class="sxs-lookup"><span data-stu-id="62210-117">`include` & `exclude` attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include"></a><span data-ttu-id="62210-118">include</span><span class="sxs-lookup"><span data-stu-id="62210-118">include</span></span>

<span data-ttu-id="62210-119">Свойство `include` ведет себя так же для атрибута `names`.</span><span class="sxs-lookup"><span data-stu-id="62210-119">The `include` property exhibits similar behavior to the `names` attribute.</span></span> <span data-ttu-id="62210-120">Среда, указанная в значении атрибута `include`, должна соответствовать среде размещения приложения ([IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*)) для отрисовки содержимого тега `<environment>`.</span><span class="sxs-lookup"><span data-stu-id="62210-120">An environment listed in the `include` attribute value must match the app's hosting environment ([IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*)) to render the content of the `<environment>` tag.</span></span>

```cshtml
<environment include="Staging,Production">
    <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude"></a><span data-ttu-id="62210-121">exclude</span><span class="sxs-lookup"><span data-stu-id="62210-121">exclude</span></span>

<span data-ttu-id="62210-122">В отличие от атрибута `include`, содержимое тега `<environment>` отрисовывается, когда среда размещения не соответствует среде, указанной в значении атрибута `exclude`.</span><span class="sxs-lookup"><span data-stu-id="62210-122">In contrast to the `include` attribute, the content of the `<environment>` tag is rendered when the hosting environment doesn't match an environment listed in the `exclude` attribute value.</span></span>

```cshtml
<environment exclude="Development">
    <strong>HostingEnvironment.EnvironmentName is not Development</strong>
</environment>
```

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="62210-123">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="62210-123">Additional resources</span></span>

* <xref:fundamentals/environments>
