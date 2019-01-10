---
title: Совместимая версия для ASP.NET Core MVC
author: rick-anderson
description: Сведения о том, как класс Startup в ASP.NET Core настраивает службы и конвейер запросов приложения.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/10/2018
uid: mvc/compatibility-version
ms.openlocfilehash: 63243d99c7cb74a7e594cd309a808455c6611fc0
ms.sourcegitcommit: 68a3081dd175d6518d1bfa31b4712bd8a2dd3864
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/18/2018
ms.locfileid: "53577855"
---
# <a name="compatibility-version-for-aspnet-core-mvc"></a><span data-ttu-id="29570-103">Совместимая версия для ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="29570-103">Compatibility version for ASP.NET Core MVC</span></span>

<span data-ttu-id="29570-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="29570-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="29570-105">Метод <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> позволяет приложению принимать или отклонять потенциально критические изменения в поведении, появившиеся в ASP.NET Core MVC 2.1 или более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="29570-105">The <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> method allows an app to opt-in or opt-out of potentially breaking behavior changes introduced in ASP.NET Core MVC 2.1 or later.</span></span> <span data-ttu-id="29570-106">Эти потенциально критические изменения обычно затрагивают поведение подсистем MVC и способы вызова **кода** средой выполнения.</span><span class="sxs-lookup"><span data-stu-id="29570-106">These potentially breaking behavior changes are generally in how the MVC subsystem behaves and how **your code** is called by the runtime.</span></span> <span data-ttu-id="29570-107">Если принять эти изменения, вы сможете использовать актуальное поведение, которое сохранится в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="29570-107">By opting in, you get the latest behavior, and the long-term behavior of ASP.NET Core.</span></span>

<span data-ttu-id="29570-108">Следующий код задает режим совместимости в ASP.NET Core 2.2:</span><span class="sxs-lookup"><span data-stu-id="29570-108">The following code sets the compatibility mode to ASP.NET Core 2.2:</span></span>

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup.cs?name=snippet1)]

<span data-ttu-id="29570-109">Мы рекомендуем протестировать приложение с помощью последней версии (`CompatibilityVersion.Version_2_2`).</span><span class="sxs-lookup"><span data-stu-id="29570-109">We recommend you test your app using the latest version (`CompatibilityVersion.Version_2_2`).</span></span> <span data-ttu-id="29570-110">Мы полагаем, что в большинстве приложений использование последней версии не приведет к критически важным изменениям в поведении.</span><span class="sxs-lookup"><span data-stu-id="29570-110">We anticipate that most apps won't have breaking behavior changes using the latest version.</span></span>

<span data-ttu-id="29570-111">Приложения, вызывающие `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`, защищены от потенциальных критически важных изменений в поведении, появившихся в MVC ASP.NET 2.1 и более поздних версий 2.x.</span><span class="sxs-lookup"><span data-stu-id="29570-111">Apps that call `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` are protected from potentially breaking behavior changes introduced in the ASP.NET Core 2.1 MVC and later 2.x versions.</span></span> <span data-ttu-id="29570-112">Особенности защиты:</span><span class="sxs-lookup"><span data-stu-id="29570-112">This protection:</span></span>

* <span data-ttu-id="29570-113">Не применяется ко всем изменениям в версии 2.1 и более поздних версиях. Она направлена только на потенциально критические изменения в поведении среды выполнения ASP.NET Core в подсистеме MVC.</span><span class="sxs-lookup"><span data-stu-id="29570-113">Does not apply to all 2.1 and later changes, it's targeted to potentially breaking ASP.NET Core runtime behavior changes in the MVC subsystem.</span></span>
* <span data-ttu-id="29570-114">Не распространяется на следующую основную версию.</span><span class="sxs-lookup"><span data-stu-id="29570-114">Does not extend to the next major version.</span></span>

<span data-ttu-id="29570-115">По умолчанию для приложений ASP.NET Core 2.1 и более поздних версий 2.x, которые **не** вызывают `SetCompatibilityVersion`, предусмотрена совместимость с 2.0.</span><span class="sxs-lookup"><span data-stu-id="29570-115">The default compatibility for ASP.NET Core 2.1 and later 2.x apps that do **not** call `SetCompatibilityVersion` is 2.0 compatibility.</span></span> <span data-ttu-id="29570-116">Отсутствие вызова `SetCompatibilityVersion` равноценно вызову `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.</span><span class="sxs-lookup"><span data-stu-id="29570-116">That is, not calling `SetCompatibilityVersion` is the same as calling `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.</span></span>

<span data-ttu-id="29570-117">Следующий код задает режим совместимости в ASP.NET Core 2.2, за исключением следующего поведения:</span><span class="sxs-lookup"><span data-stu-id="29570-117">The following code sets the compatibility mode to ASP.NET Core 2.2, except for the following behaviors:</span></span>

* [<span data-ttu-id="29570-118">AllowCombiningAuthorizeFilters</span><span class="sxs-lookup"><span data-stu-id="29570-118">AllowCombiningAuthorizeFilters</span></span>](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)
* [<span data-ttu-id="29570-119">InputFormatterExceptionPolicy</span><span class="sxs-lookup"><span data-stu-id="29570-119">InputFormatterExceptionPolicy</span></span>](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup2.cs?name=snippet1)]

<span data-ttu-id="29570-120">Если в приложении возникают критические изменения в поведении, использование параметров совместимости имеет следующие преимущества:</span><span class="sxs-lookup"><span data-stu-id="29570-120">For apps that encounter breaking behavior changes, using the appropriate compatibility switches:</span></span>

* <span data-ttu-id="29570-121">Позволяет использовать последний выпуск и отказаться от конкретных критических изменений.</span><span class="sxs-lookup"><span data-stu-id="29570-121">Allows you to use the latest release and opt out of specific breaking behavior changes.</span></span>
* <span data-ttu-id="29570-122">Дает вам время обновить приложение, чтобы оно работало с последними изменениями.</span><span class="sxs-lookup"><span data-stu-id="29570-122">Gives you time to update your app so it works with the latest changes.</span></span>

<span data-ttu-id="29570-123">В комментариях к классу [MvcOptions](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs) подробно объясняется, что изменилось и почему изменения выгодны для большинства пользователей.</span><span class="sxs-lookup"><span data-stu-id="29570-123">The [MvcOptions](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs) class source comments have a good explanation of what changed and why the changes are an improvement for most users.</span></span>

<span data-ttu-id="29570-124">Позднее мы выпустим [версию ASP.NET Core 3.0](https://github.com/aspnet/Home/wiki/Roadmap).</span><span class="sxs-lookup"><span data-stu-id="29570-124">At some future date, there will be an [ASP.NET Core 3.0 version](https://github.com/aspnet/Home/wiki/Roadmap).</span></span> <span data-ttu-id="29570-125">Старое поведение, поддерживаемое параметрами совместимости, не сохранится в версии 3.0.</span><span class="sxs-lookup"><span data-stu-id="29570-125">Old behaviors supported by compatibility switches will be removed in the 3.0 version.</span></span> <span data-ttu-id="29570-126">Мы думаем, что это полезные изменения почти для всех пользователей.</span><span class="sxs-lookup"><span data-stu-id="29570-126">We feel these are positive changes benefitting nearly all users.</span></span> <span data-ttu-id="29570-127">Большинство приложений сможет воспользоваться этими изменениями уже сейчас, а другие успеют обновиться.</span><span class="sxs-lookup"><span data-stu-id="29570-127">By introducing these changes now, most apps can benefit now, and the others will have time to update their apps.</span></span>
