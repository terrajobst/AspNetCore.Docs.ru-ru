---
title: "Вспомогательный модуль тега среды ASP.NET Core"
author: pkellner
description: "Вспомогательный тега среды ASP.NET Core определен, включая все свойства"
ms.author: riande
manager: wpickett
ms.date: 07/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 32646f1fdaf840f796da1ec573459157a41a86d1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="045d1-103">Вспомогательный модуль тега среды ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="045d1-103">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="045d1-104">По [Kellner Питер](http://peterkellner.net) и [Ateya Hisham Bin](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="045d1-104">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="045d1-105">Вспомогательный тега среды условно отрисовывает его вложенного содержимого на основании текущей среде размещения.</span><span class="sxs-lookup"><span data-stu-id="045d1-105">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="045d1-106">Его одного атрибута `names` является разделенный запятыми список среды кодов, если любой совпадают в текущей среде, запускающих заключено содержимое для отображения.</span><span class="sxs-lookup"><span data-stu-id="045d1-106">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="045d1-107">Атрибуты вспомогательного тега среды</span><span class="sxs-lookup"><span data-stu-id="045d1-107">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="045d1-108">имена</span><span class="sxs-lookup"><span data-stu-id="045d1-108">names</span></span>

<span data-ttu-id="045d1-109">Принимает только одно имя среды размещения или список разделенных запятыми имен среды, триггер подготовки к просмотру вложенного содержимого размещения.</span><span class="sxs-lookup"><span data-stu-id="045d1-109">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="045d1-110">Эти значения сравниваются с текущее значение, возвращаемое из статического свойства ASP.NET Core `HostingEnvironment.EnvironmentName`.</span><span class="sxs-lookup"><span data-stu-id="045d1-110">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="045d1-111">Это значение является одним из следующих: **промежуточной**; **Разработки** или **рабочей**.</span><span class="sxs-lookup"><span data-stu-id="045d1-111">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="045d1-112">Сравнение не учитывает регистр.</span><span class="sxs-lookup"><span data-stu-id="045d1-112">The comparison ignores case.</span></span>

<span data-ttu-id="045d1-113">Примером является допустимым `environment` модуль тег:</span><span class="sxs-lookup"><span data-stu-id="045d1-113">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="045d1-114">Включение и исключение атрибуты</span><span class="sxs-lookup"><span data-stu-id="045d1-114">include and exclude attributes</span></span>

<span data-ttu-id="045d1-115">Добавляет 2.x ASP.NET Core `include`  &  `exclude` атрибуты.</span><span class="sxs-lookup"><span data-stu-id="045d1-115">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="045d1-116">Эти атрибуты определяют визуализации на основе имен среды размещения включенный или исключенный вложенного содержимого.</span><span class="sxs-lookup"><span data-stu-id="045d1-116">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="045d1-117">Включить ASP.NET Core 2.0 и более поздних версий</span><span class="sxs-lookup"><span data-stu-id="045d1-117">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="045d1-118">`include` Свойство имеет такое же поведение из `names` атрибута в ASP.NET версии 1.0 Core.</span><span class="sxs-lookup"><span data-stu-id="045d1-118">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="045d1-119">исключить Core ASP.NET 2.0 и более поздних версий</span><span class="sxs-lookup"><span data-stu-id="045d1-119">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="045d1-120">Напротив `exclude` позволяет `EnvironmentTagHelper` отображения вложенного содержимого для всех имен среды размещения, за исключением ключи, указанный вами.</span><span class="sxs-lookup"><span data-stu-id="045d1-120">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="045d1-121">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="045d1-121">Additional resources</span></span>

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
