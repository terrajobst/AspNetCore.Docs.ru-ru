---
title: "Вспомогательный модуль тега среды ASP.NET Core"
author: pkellner
description: "Вспомогательный тега среды ASP.Net Core определен, включая все свойства"
keywords: "ASP.NET Core, вспомогательная функция тегов"
ms.author: riande
manager: wpickett
ms.date: 07/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/EnvironmentTagHelper
ms.openlocfilehash: e16f942b4a42ef495c7a7daff6724ff071bb35a1
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/20/2017
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="8232b-104">Вспомогательный модуль тега среды ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8232b-104">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="8232b-105">По [Kellner Питер](http://peterkellner.net) и [Ateya Hisham Bin](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="8232b-105">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="8232b-106">Вспомогательный тега среды условно отрисовывает его вложенного содержимого на основании текущей среде размещения.</span><span class="sxs-lookup"><span data-stu-id="8232b-106">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="8232b-107">Его одного атрибута `names` является разделенный запятыми список среды кодов, если любой совпадают в текущей среде, запускающих заключено содержимое для отображения.</span><span class="sxs-lookup"><span data-stu-id="8232b-107">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="8232b-108">Атрибуты вспомогательного тега среды</span><span class="sxs-lookup"><span data-stu-id="8232b-108">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="8232b-109">имена</span><span class="sxs-lookup"><span data-stu-id="8232b-109">names</span></span>

<span data-ttu-id="8232b-110">Принимает только одно имя среды размещения или список разделенных запятыми имен среды, триггер подготовки к просмотру вложенного содержимого размещения.</span><span class="sxs-lookup"><span data-stu-id="8232b-110">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="8232b-111">Эти значения сравниваются с текущее значение, возвращаемое из статического свойства ASP.NET Core `HostingEnvironment.EnvironmentName`.</span><span class="sxs-lookup"><span data-stu-id="8232b-111">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="8232b-112">Это значение является одним из следующих: **промежуточной**; **Разработки** или **рабочей**.</span><span class="sxs-lookup"><span data-stu-id="8232b-112">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="8232b-113">Сравнение не учитывает регистр.</span><span class="sxs-lookup"><span data-stu-id="8232b-113">The comparison ignores case.</span></span>

<span data-ttu-id="8232b-114">Примером является допустимым `environment` модуль тег:</span><span class="sxs-lookup"><span data-stu-id="8232b-114">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="8232b-115">Включение и исключение атрибуты</span><span class="sxs-lookup"><span data-stu-id="8232b-115">include and exclude attributes</span></span>

<span data-ttu-id="8232b-116">Добавляет 2.x ASP.NET Core `include`  &  `exclude` атрибуты.</span><span class="sxs-lookup"><span data-stu-id="8232b-116">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="8232b-117">Эти атрибуты определяют визуализации на основе имен среды размещения включенный или исключенный вложенного содержимого.</span><span class="sxs-lookup"><span data-stu-id="8232b-117">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="8232b-118">Включить ASP.NET Core 2.0 и более поздних версий</span><span class="sxs-lookup"><span data-stu-id="8232b-118">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="8232b-119">`include` Свойство имеет такое же поведение из `names` атрибута в ASP.NET версии 1.0 Core.</span><span class="sxs-lookup"><span data-stu-id="8232b-119">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="8232b-120">исключить Core ASP.NET 2.0 и более поздних версий</span><span class="sxs-lookup"><span data-stu-id="8232b-120">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="8232b-121">Напротив `exclude` позволяет `EnvironmentTagHelper` отображения вложенного содержимого для всех имен среды размещения, за исключением ключи, указанный вами.</span><span class="sxs-lookup"><span data-stu-id="8232b-121">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="8232b-122">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="8232b-122">Additional resources</span></span>

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
