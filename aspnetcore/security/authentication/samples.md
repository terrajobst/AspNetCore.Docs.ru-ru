---
title: Примеры проверки подлинности в ASP.NET Core
author: rick-anderson
description: Ссылки на примеры проверки подлинности в репозитории ASP.NET Core.
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: efa177245faceddad4eb80de9e6f6d38e1a4261c
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022410"
---
# <a name="authentication-samples-for-aspnet-core"></a><span data-ttu-id="083b3-103">Примеры проверки подлинности в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="083b3-103">Authentication samples for ASP.NET Core</span></span>

<span data-ttu-id="083b3-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="083b3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="083b3-105">[Репозиторий ASP.NET Core](https://github.com/aspnet/AspNetCore) содержит следующие примеры проверки подлинности в папке *AspNetCore/src/Security/samples*:</span><span class="sxs-lookup"><span data-stu-id="083b3-105">The [ASP.NET Core repository](https://github.com/aspnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="083b3-106">Преобразование утверждений</span><span class="sxs-lookup"><span data-stu-id="083b3-106">Claims transformation</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="083b3-107">Проверка подлинности с помощью cookie</span><span class="sxs-lookup"><span data-stu-id="083b3-107">Cookie authentication</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [<span data-ttu-id="083b3-108">Поставщик пользовательской политики — IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="083b3-108">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="083b3-109">Схемы и параметры динамической проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="083b3-109">Dynamic authentication schemes and options</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="083b3-110">Внешние утверждения</span><span class="sxs-lookup"><span data-stu-id="083b3-110">External claims</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="083b3-111">Выбор между cookie и другой схемой проверки подлинности на основе запроса</span><span class="sxs-lookup"><span data-stu-id="083b3-111">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="083b3-112">Ограничение доступа к статическим файлам</span><span class="sxs-lookup"><span data-stu-id="083b3-112">Restricts access to static files</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="083b3-113">Запуск примеров</span><span class="sxs-lookup"><span data-stu-id="083b3-113">Run the samples</span></span>

* <span data-ttu-id="083b3-114">Выберите [ветвь](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="083b3-114">Select a [branch](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="083b3-115">Например: `Tag:v3.0.0`</span><span class="sxs-lookup"><span data-stu-id="083b3-115">For example, `Tag:v3.0.0`</span></span>
* <span data-ttu-id="083b3-116">Клонируйте или скачайте [репозиторий ASP.NET Core](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="083b3-116">Clone or download the [ASP.NET Core repository](https://github.com/aspnet/AspNetCore).</span></span>
* <span data-ttu-id="083b3-117">Проверьте, что у вас установлен [пакет SDK для .NET Core](https://www.microsoft.com/net/download/all) версии, которая соответствует клону репозитория ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="083b3-117">Verify you have installed the [.NET Core SDK](https://www.microsoft.com/net/download/all) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="083b3-118">Перейдите к примеру в *AspNetCore/src/Security/samples* и запустите его с помощью `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="083b3-118">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="083b3-119">[Репозиторий ASP.NET Core](https://github.com/aspnet/AspNetCore) содержит следующие примеры проверки подлинности в папке *AspNetCore/src/Security/samples*:</span><span class="sxs-lookup"><span data-stu-id="083b3-119">The [ASP.NET Core repository](https://github.com/aspnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="083b3-120">Преобразование утверждений</span><span class="sxs-lookup"><span data-stu-id="083b3-120">Claims transformation</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="083b3-121">Проверка подлинности с помощью cookie</span><span class="sxs-lookup"><span data-stu-id="083b3-121">Cookie authentication</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [<span data-ttu-id="083b3-122">Поставщик пользовательской политики — IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="083b3-122">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="083b3-123">Схемы и параметры динамической проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="083b3-123">Dynamic authentication schemes and options</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="083b3-124">Внешние утверждения</span><span class="sxs-lookup"><span data-stu-id="083b3-124">External claims</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="083b3-125">Выбор между cookie и другой схемой проверки подлинности на основе запроса</span><span class="sxs-lookup"><span data-stu-id="083b3-125">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="083b3-126">Ограничение доступа к статическим файлам</span><span class="sxs-lookup"><span data-stu-id="083b3-126">Restricts access to static files</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="083b3-127">Запуск примеров</span><span class="sxs-lookup"><span data-stu-id="083b3-127">Run the samples</span></span>

* <span data-ttu-id="083b3-128">Выберите [ветвь](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="083b3-128">Select a [branch](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="083b3-129">Например: `release/2.2`</span><span class="sxs-lookup"><span data-stu-id="083b3-129">For example, `release/2.2`</span></span>
* <span data-ttu-id="083b3-130">Клонируйте или скачайте [репозиторий ASP.NET Core](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="083b3-130">Clone or download the [ASP.NET Core repository](https://github.com/aspnet/AspNetCore).</span></span>
* <span data-ttu-id="083b3-131">Проверьте, что у вас установлен [пакет SDK для .NET Core](https://www.microsoft.com/net/download/all) версии, которая соответствует клону репозитория ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="083b3-131">Verify you have installed the [.NET Core SDK](https://www.microsoft.com/net/download/all) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="083b3-132">Перейдите к примеру в *AspNetCore/src/Security/samples* и запустите его с помощью `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="083b3-132">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end
