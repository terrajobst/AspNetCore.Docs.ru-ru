---
title: Использование соглашений веб-API
author: pranavkm
description: Сведения о соглашениях веб-API в ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: pranavkm
ms.custom: mvc
ms.date: 11/13/2018
uid: web-api/advanced/conventions
ms.openlocfilehash: ede9a46c160cf6a49aa93da710af0bf0b8f59acc
ms.sourcegitcommit: c4572be5ebb301013a5698caf9b5572b76cb2e34
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/30/2018
ms.locfileid: "52710079"
---
# <a name="use-web-api-conventions"></a><span data-ttu-id="14501-103">Использование соглашений веб-API</span><span class="sxs-lookup"><span data-stu-id="14501-103">Use web API conventions</span></span>

<span data-ttu-id="14501-104">В ASP.NET Core 2.2 добавлен способ извлечения распространенной [документации по API](xref:tutorials/web-api-help-pages-using-swagger) и ее применения к нескольким действиям, контроллеру или всем контроллерам в рамках сборки.</span><span class="sxs-lookup"><span data-stu-id="14501-104">ASP.NET Core 2.2 introduces a way to extract common [API documentation](xref:tutorials/web-api-help-pages-using-swagger) and apply it to multiple actions, controllers, or all controllers within an assembly.</span></span> <span data-ttu-id="14501-105">Соглашения веб-API заменяют применение атрибута [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute) к отдельным действиям.</span><span class="sxs-lookup"><span data-stu-id="14501-105">Web API conventions are a substitute for decorating individual actions with [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute).</span></span> <span data-ttu-id="14501-106">Это позволяет определять наиболее распространенные стандартные типы возвращаемых значений и коды состояния, возвращаемые из действия, а также способ выбора метода соглашения, который применяется к действию.</span><span class="sxs-lookup"><span data-stu-id="14501-106">It allows you to define the most common "conventional" return types and status codes that you return from your action with a way to select the convention method that applies to an action.</span></span>

<span data-ttu-id="14501-107">По умолчанию в состав ASP.NET Core MVC 2.2 входит набор стандартных соглашений `Microsoft.AspNetCore.Mvc.DefaultApiConventions`.</span><span class="sxs-lookup"><span data-stu-id="14501-107">By default, ASP.NET Core MVC 2.2 ships with a set of default conventions, `Microsoft.AspNetCore.Mvc.DefaultApiConventions`.</span></span> <span data-ttu-id="14501-108">В основу соглашений положен контроллер, который формируется в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="14501-108">The conventions are based on the controller that ASP.NET Core scaffolds.</span></span> <span data-ttu-id="14501-109">Если ваши действия соответствуют схеме, являющейся результатом формирования шаблонов, вы сможете успешно использовать соглашения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="14501-109">If your actions follow the pattern that scaffolding produces, you should be successful using the default conventions.</span></span>

<span data-ttu-id="14501-110">Во время выполнения <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> понимает соглашения.</span><span class="sxs-lookup"><span data-stu-id="14501-110">At runtime, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> understands conventions.</span></span> <span data-ttu-id="14501-111">`ApiExplorer` является абстракцией MVC для взаимодействия с генераторами документов OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="14501-111">`ApiExplorer` is MVC's abstraction to communicate with Open API document generators.</span></span> <span data-ttu-id="14501-112">Атрибуты из примененного соглашения связываются с действием и включаются в документацию Swagger для действия.</span><span class="sxs-lookup"><span data-stu-id="14501-112">Attributes from the applied convention get associated with an action and are included in the action's Swagger documentation.</span></span> <span data-ttu-id="14501-113">Анализаторы API также понимают соглашения.</span><span class="sxs-lookup"><span data-stu-id="14501-113">API analyzers also understand conventions.</span></span> <span data-ttu-id="14501-114">Если ваше действие является нестандартным (например, возвращает код состояния, который не задокументирован примененным соглашением), оно выводит предупреждение, позволяя задокументировать его.</span><span class="sxs-lookup"><span data-stu-id="14501-114">If your action is unconventional (for example, it returns a status code that isn't documented by the applied convention), it produces a warning, encouraging you to document it.</span></span>

<span data-ttu-id="14501-115">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="14501-115">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="apply-web-api-conventions"></a><span data-ttu-id="14501-116">Применение соглашений веб-API</span><span class="sxs-lookup"><span data-stu-id="14501-116">Apply web API conventions</span></span>

<span data-ttu-id="14501-117">Существует три способа применения соглашения.</span><span class="sxs-lookup"><span data-stu-id="14501-117">There are three ways to apply a convention.</span></span> <span data-ttu-id="14501-118">Соглашения не являются составными.</span><span class="sxs-lookup"><span data-stu-id="14501-118">Conventions don't compose.</span></span> <span data-ttu-id="14501-119">Каждое действие может быть связано только с одним соглашением.</span><span class="sxs-lookup"><span data-stu-id="14501-119">Each action may be associated with exactly one convention.</span></span> <span data-ttu-id="14501-120">Более конкретные соглашения (см. ниже) имеют приоритет над менее определенными.</span><span class="sxs-lookup"><span data-stu-id="14501-120">More specific conventions (detailed below) take precedence over less specific ones.</span></span> <span data-ttu-id="14501-121">Если к действию применяются два или более соглашения с одинаковым приоритетом, выбор осуществляется недетерминированным образом.</span><span class="sxs-lookup"><span data-stu-id="14501-121">The selection is non-deterministic when two or more conventions of the same priority apply to an action.</span></span> <span data-ttu-id="14501-122">Существуют следующие варианты применения соглашения к действию (от наиболее конкретного до наименее конкретного):</span><span class="sxs-lookup"><span data-stu-id="14501-122">The following options exist to apply a convention to an action, from the most specific to the least specific:</span></span>

1. <span data-ttu-id="14501-123">`Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; Применяется к отдельным действиям и указывает тип соглашения и применимый метод соглашения.</span><span class="sxs-lookup"><span data-stu-id="14501-123">`Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; Applies to individual actions and specifies the convention type and the convention method that applies.</span></span> <span data-ttu-id="14501-124">В следующем примере показано применение метода соглашения `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` к действию `Update`:</span><span class="sxs-lookup"><span data-stu-id="14501-124">In the following sample, the convention method `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` is applied to the `Update` action:</span></span>

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventionmethod&highlight=2-3)]

1. <span data-ttu-id="14501-125">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute`, примененный к контроллеру &mdash;, применяет тип соглашения ко всем действиям в контроллере.</span><span class="sxs-lookup"><span data-stu-id="14501-125">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applied to a controller &mdash; Applies the convention type to all actions on the controller.</span></span> <span data-ttu-id="14501-126">Методы соглашения дополняются подсказками, которые определяют, к каким действиям они будут применены (сведения в рамках соглашений о создании).</span><span class="sxs-lookup"><span data-stu-id="14501-126">Convention methods are decorated with hints that determine which actions it would apply to (details as part of authoring conventions).</span></span> <span data-ttu-id="14501-127">Пример:</span><span class="sxs-lookup"><span data-stu-id="14501-127">For example:</span></span>

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventiontypeattribute)]

1. <span data-ttu-id="14501-128">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute`, примененный к сборке &mdash;, применяет тип соглашения ко всем контроллерам в текущей сборке.</span><span class="sxs-lookup"><span data-stu-id="14501-128">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applied to an assembly &mdash; Applies the convention type to all controllers in the current assembly.</span></span> <span data-ttu-id="14501-129">Пример:</span><span class="sxs-lookup"><span data-stu-id="14501-129">For example:</span></span>

    [!code-csharp[](conventions/sample/Startup.cs?name=apiconventiontypeattribute)]

## <a name="create-web-api-conventions"></a><span data-ttu-id="14501-130">Создание соглашений веб-API</span><span class="sxs-lookup"><span data-stu-id="14501-130">Create web API conventions</span></span>

<span data-ttu-id="14501-131">Соглашение — это статический тип с методами.</span><span class="sxs-lookup"><span data-stu-id="14501-131">A convention is a static type with methods.</span></span> <span data-ttu-id="14501-132">Эти методы помечаются атрибутами `[ProducesResponseType]` или `[ProducesDefaultResponseType]`.</span><span class="sxs-lookup"><span data-stu-id="14501-132">These methods are annotated with `[ProducesResponseType]` or `[ProducesDefaultResponseType]` attributes.</span></span>

```csharp
public static class MyAppConventions
{
    [ProducesResponseType(200)]
    [ProducesResponseType(404)]
    public static void Find(int id)
    {
    }
}
```

<span data-ttu-id="14501-133">Результатом применения этого соглашения к сборке является метод соглашения, применяемый к любому действию с именем `Find` и имеющий только один параметр `id` до тех пор, пока не появятся другие более конкретные атрибуты метаданных.</span><span class="sxs-lookup"><span data-stu-id="14501-133">Applying this convention to an assembly results in the convention method applying to any action with the name `Find` and having exactly one parameter named `id`, as long as they don't have other more specific metadata attributes.</span></span>

<span data-ttu-id="14501-134">Помимо `[ProducesResponseType]` и `[ProducesDefaultResponseType]` к методу соглашения, определяющему методы, к которым они применяются, можно применять `[ApiConventionNameMatch]` и `[ApiConventionTypeMatch]`.</span><span class="sxs-lookup"><span data-stu-id="14501-134">In addition to `[ProducesResponseType]` and `[ProducesDefaultResponseType]`, `[ApiConventionNameMatch]` and `[ApiConventionTypeMatch]` can be applied to the convention method that determines the methods they apply to.</span></span> <span data-ttu-id="14501-135">Пример:</span><span class="sxs-lookup"><span data-stu-id="14501-135">For example:</span></span>

```csharp
[ProducesResponseType(200)]
[ProducesResponseType(404)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

* <span data-ttu-id="14501-136">Параметр `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix`, примененный к методу, указывает, что соглашение может соответствовать любому действию до тех пор, пока у него есть префикс "Find".</span><span class="sxs-lookup"><span data-stu-id="14501-136">The `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` option applied to the method, indicates that the convention can match any action as long as it's prefixed with "Find".</span></span> <span data-ttu-id="14501-137">Это такие методы, как `Find`, `FindPet` или `FindById`.</span><span class="sxs-lookup"><span data-stu-id="14501-137">This includes methods such as `Find`, `FindPet`, or `FindById`.</span></span>
* <span data-ttu-id="14501-138">`Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix`, примененный к параметру, указывает, что соглашение может соответствовать методам только с одним параметром, заканчивающимся в идентификаторе суффикса.</span><span class="sxs-lookup"><span data-stu-id="14501-138">The `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` applied to the parameter indicates that the convention can match methods with exactly one parameter ending in the suffix identifier.</span></span> <span data-ttu-id="14501-139">Это такие параметры, как `id` или `petId`.</span><span class="sxs-lookup"><span data-stu-id="14501-139">This includes parameters such as `id` or `petId`.</span></span> <span data-ttu-id="14501-140">`ApiConventionTypeMatch` может аналогичным образом применяться к типам для ограничения типа параметра.</span><span class="sxs-lookup"><span data-stu-id="14501-140">`ApiConventionTypeMatch` can be similarly applied to types to constrain the type of the parameter.</span></span> <span data-ttu-id="14501-141">Аргумент `params[]` можно использовать для указания остальных параметров, которым не требуется явное сопоставление.</span><span class="sxs-lookup"><span data-stu-id="14501-141">A `params[]` argument can be used to indicate remaining parameters that don't need to be explicitly matched.</span></span>
