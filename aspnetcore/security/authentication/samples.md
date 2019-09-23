---
title: Примеры проверки подлинности в ASP.NET Core
author: rick-anderson
description: Ссылки на примеры проверки подлинности в репозитории ASP.NET Core.
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: d49aef198e926d88f1a6727f84b06f0861c8812d
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187297"
---
# <a name="authentication-samples-for-aspnet-core"></a><span data-ttu-id="62fc3-103">Примеры проверки подлинности в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="62fc3-103">Authentication samples for ASP.NET Core</span></span>

<span data-ttu-id="62fc3-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="62fc3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="62fc3-105">[Репозиторий ASP.NET Core](https://github.com/aspnet/AspNetCore) содержит следующие примеры проверки подлинности в папке *AspNetCore/src/Security/samples*:</span><span class="sxs-lookup"><span data-stu-id="62fc3-105">The [ASP.NET Core repository](https://github.com/aspnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="62fc3-106">Преобразование утверждений</span><span class="sxs-lookup"><span data-stu-id="62fc3-106">Claims transformation</span></span>](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="62fc3-107">Проверка подлинности с помощью cookie</span><span class="sxs-lookup"><span data-stu-id="62fc3-107">Cookie authentication</span></span>](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/Cookies)
* [<span data-ttu-id="62fc3-108">Поставщик пользовательской политики — IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="62fc3-108">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="62fc3-109">Схемы и параметры динамической проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="62fc3-109">Dynamic authentication schemes and options</span></span>](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="62fc3-110">Внешние утверждения</span><span class="sxs-lookup"><span data-stu-id="62fc3-110">External claims</span></span>](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="62fc3-111">Выбор между cookie и другой схемой проверки подлинности на основе запроса</span><span class="sxs-lookup"><span data-stu-id="62fc3-111">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="62fc3-112">Ограничение доступа к статическим файлам</span><span class="sxs-lookup"><span data-stu-id="62fc3-112">Restricts access to static files</span></span>](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="62fc3-113">Запуск примеров</span><span class="sxs-lookup"><span data-stu-id="62fc3-113">Run the samples</span></span>

* <span data-ttu-id="62fc3-114">Выберите [ветвь](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="62fc3-114">Select a [branch](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="62fc3-115">Например: `Tag:v3.0.0`</span><span class="sxs-lookup"><span data-stu-id="62fc3-115">For example, `Tag:v3.0.0`</span></span>
* <span data-ttu-id="62fc3-116">Клонируйте или скачайте [репозиторий ASP.NET Core](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="62fc3-116">Clone or download the [ASP.NET Core repository](https://github.com/aspnet/AspNetCore).</span></span>
* <span data-ttu-id="62fc3-117">Проверьте, что у вас установлен [пакет SDK для .NET Core](https://www.microsoft.com/net/download/all) версии, которая соответствует клону репозитория ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="62fc3-117">Verify you have installed the [.NET Core SDK](https://www.microsoft.com/net/download/all) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="62fc3-118">Перейдите к примеру в *AspNetCore/src/Security/samples* и запустите его с помощью `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="62fc3-118">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="62fc3-119">[Репозиторий ASP.NET Core](https://github.com/aspnet/AspNetCore) содержит следующие примеры проверки подлинности в папке *AspNetCore/src/Security/samples*:</span><span class="sxs-lookup"><span data-stu-id="62fc3-119">The [ASP.NET Core repository](https://github.com/aspnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="62fc3-120">Преобразование утверждений</span><span class="sxs-lookup"><span data-stu-id="62fc3-120">Claims transformation</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="62fc3-121">Проверка подлинности с помощью cookie</span><span class="sxs-lookup"><span data-stu-id="62fc3-121">Cookie authentication</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [<span data-ttu-id="62fc3-122">Поставщик пользовательской политики — IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="62fc3-122">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="62fc3-123">Схемы и параметры динамической проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="62fc3-123">Dynamic authentication schemes and options</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="62fc3-124">Внешние утверждения</span><span class="sxs-lookup"><span data-stu-id="62fc3-124">External claims</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="62fc3-125">Выбор между cookie и другой схемой проверки подлинности на основе запроса</span><span class="sxs-lookup"><span data-stu-id="62fc3-125">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="62fc3-126">Ограничение доступа к статическим файлам</span><span class="sxs-lookup"><span data-stu-id="62fc3-126">Restricts access to static files</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="62fc3-127">Запуск примеров</span><span class="sxs-lookup"><span data-stu-id="62fc3-127">Run the samples</span></span>

* <span data-ttu-id="62fc3-128">Выберите [ветвь](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="62fc3-128">Select a [branch](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="62fc3-129">Например: `release/2.2`</span><span class="sxs-lookup"><span data-stu-id="62fc3-129">For example, `release/2.2`</span></span>
* <span data-ttu-id="62fc3-130">Клонируйте или скачайте [репозиторий ASP.NET Core](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="62fc3-130">Clone or download the [ASP.NET Core repository](https://github.com/aspnet/AspNetCore).</span></span>
* <span data-ttu-id="62fc3-131">Проверьте, что у вас установлен [пакет SDK для .NET Core](https://www.microsoft.com/net/download/all) версии, которая соответствует клону репозитория ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="62fc3-131">Verify you have installed the [.NET Core SDK](https://www.microsoft.com/net/download/all) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="62fc3-132">Перейдите к примеру в *AspNetCore/src/Security/samples* и запустите его с помощью `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="62fc3-132">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end
