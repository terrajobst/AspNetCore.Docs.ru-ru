---
title: Вспомогательная функция тега среды в ASP.NET Core
author: pkellner
description: Определенная в ASP.NET Core вспомогательная функция тега среды, включая все свойства
ms.author: riande
ms.date: 07/14/2017
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 4a283a3a03aa6cac228ec6effd02e3f1095be260
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342228"
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="a4da9-103">Вспомогательная функция тега среды в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a4da9-103">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="a4da9-104">Авторы: [Питер Кельнер (Peter Kellner)](http://peterkellner.net) и [Хишам Бин Атея](https://twitter.com/hishambinateya) (Hisham Bin Ateya)</span><span class="sxs-lookup"><span data-stu-id="a4da9-104">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="a4da9-105">Вспомогательная функция тега среды условно отрисовывает заключенное в нее содержимое с учетом текущей среды размещения.</span><span class="sxs-lookup"><span data-stu-id="a4da9-105">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="a4da9-106">Ее единственным атрибутом `names` является разделенный запятыми список имен сред, который при наличии совпадения с текущей средой запустит отрисовку содержимого.</span><span class="sxs-lookup"><span data-stu-id="a4da9-106">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="a4da9-107">Атрибуты вспомогательной функции тега среды</span><span class="sxs-lookup"><span data-stu-id="a4da9-107">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="a4da9-108">имена</span><span class="sxs-lookup"><span data-stu-id="a4da9-108">names</span></span>

<span data-ttu-id="a4da9-109">Принимает одно имя среды размещения или список разделенных запятыми имен сред размещения, которые запускают отрисовку включенного в функцию содержимого.</span><span class="sxs-lookup"><span data-stu-id="a4da9-109">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="a4da9-110">Эти значения сравниваются с текущим значением, возвращаемым из статического свойства ASP.NET Core `HostingEnvironment.EnvironmentName`.</span><span class="sxs-lookup"><span data-stu-id="a4da9-110">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="a4da9-111">Это значение является одним из следующих: **Staging**, **Development** или **Production**.</span><span class="sxs-lookup"><span data-stu-id="a4da9-111">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="a4da9-112">При сравнении регистр не учитывается.</span><span class="sxs-lookup"><span data-stu-id="a4da9-112">The comparison ignores case.</span></span>

<span data-ttu-id="a4da9-113">Пример допустимой функции тега `environment`:</span><span class="sxs-lookup"><span data-stu-id="a4da9-113">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="a4da9-114">Атрибуты include и exclude</span><span class="sxs-lookup"><span data-stu-id="a4da9-114">include and exclude attributes</span></span>

<span data-ttu-id="a4da9-115">В ASP.NET Core 2.x добавлены атрибуты `include` & `exclude`.</span><span class="sxs-lookup"><span data-stu-id="a4da9-115">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="a4da9-116">Эти атрибуты управляют отрисовкой включенного содержимого на основе имен включенных или исключенных сред размещения.</span><span class="sxs-lookup"><span data-stu-id="a4da9-116">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="a4da9-117">включить ASP.NET Core 2.0 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="a4da9-117">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="a4da9-118">Свойство `include` действует аналогично атрибуту `names` в ASP.NET Core 1.0.</span><span class="sxs-lookup"><span data-stu-id="a4da9-118">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="a4da9-119">исключить ASP.NET Core 2.0 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="a4da9-119">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="a4da9-120">Напротив, свойство `exclude` позволяет `EnvironmentTagHelper` отрисовывать включенное в функцию содержимое для всех имен сред размещения, за исключением указанных.</span><span class="sxs-lookup"><span data-stu-id="a4da9-120">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="a4da9-121">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a4da9-121">Additional resources</span></span>

* <xref:fundamentals/environments>
