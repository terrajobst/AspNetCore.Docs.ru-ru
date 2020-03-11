---
title: Примеры проверки подлинности в ASP.NET Core
author: rick-anderson
description: Ссылки на примеры проверки подлинности в репозитории ASP.NET Core.
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: 3d7e28f6e501bd8bd3908ca4b314a63cee52ebe3
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78651658"
---
# <a name="authentication-samples-for-aspnet-core"></a><span data-ttu-id="ca082-103">Примеры проверки подлинности в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ca082-103">Authentication samples for ASP.NET Core</span></span>

<span data-ttu-id="ca082-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ca082-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ca082-105">[Репозиторий ASP.NET Core](https://github.com/dotnet/AspNetCore) содержит следующие примеры проверки подлинности в папке *AspNetCore/src/Security/Samples* :</span><span class="sxs-lookup"><span data-stu-id="ca082-105">The [ASP.NET Core repository](https://github.com/dotnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="ca082-106">Преобразование утверждений</span><span class="sxs-lookup"><span data-stu-id="ca082-106">Claims transformation</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="ca082-107">Проверка подлинности файлов cookie</span><span class="sxs-lookup"><span data-stu-id="ca082-107">Cookie authentication</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/Cookies)
* [<span data-ttu-id="ca082-108">Поставщик настраиваемой политики — Иаусоризатионполиципровидер</span><span class="sxs-lookup"><span data-stu-id="ca082-108">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="ca082-109">Схемы и параметры динамической проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="ca082-109">Dynamic authentication schemes and options</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="ca082-110">Внешние утверждения</span><span class="sxs-lookup"><span data-stu-id="ca082-110">External claims</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="ca082-111">Выбор между файлами cookie и другой схемой проверки подлинности на основе запроса</span><span class="sxs-lookup"><span data-stu-id="ca082-111">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="ca082-112">Разрешает доступ к статическим файлам</span><span class="sxs-lookup"><span data-stu-id="ca082-112">Restricts access to static files</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="ca082-113">Запуск шаблонов</span><span class="sxs-lookup"><span data-stu-id="ca082-113">Run the samples</span></span>

* <span data-ttu-id="ca082-114">Выберите [ветвь](https://github.com/dotnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="ca082-114">Select a [branch](https://github.com/dotnet/AspNetCore).</span></span> <span data-ttu-id="ca082-115">Например `Tag:v3.0.0`.</span><span class="sxs-lookup"><span data-stu-id="ca082-115">For example, `Tag:v3.0.0`</span></span>
* <span data-ttu-id="ca082-116">Клонировать или скачать [репозиторий ASP.NET Core](https://github.com/dotnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="ca082-116">Clone or download the [ASP.NET Core repository](https://github.com/dotnet/AspNetCore).</span></span>
* <span data-ttu-id="ca082-117">Убедитесь, что установлена версия [пакет SDK для .NET Core](https://www.microsoft.com/net/download/all) , соответствующая клону репозитория ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ca082-117">Verify you have installed the [.NET Core SDK](https://www.microsoft.com/net/download/all) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="ca082-118">Перейдите к примеру в *AspNetCore/src/Security/Samples* и запустите пример с `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="ca082-118">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ca082-119">[Репозиторий ASP.NET Core](https://github.com/dotnet/AspNetCore) содержит следующие примеры проверки подлинности в папке *AspNetCore/src/Security/Samples* :</span><span class="sxs-lookup"><span data-stu-id="ca082-119">The [ASP.NET Core repository](https://github.com/dotnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="ca082-120">Преобразование утверждений</span><span class="sxs-lookup"><span data-stu-id="ca082-120">Claims transformation</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="ca082-121">Проверка подлинности файлов cookie</span><span class="sxs-lookup"><span data-stu-id="ca082-121">Cookie authentication</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [<span data-ttu-id="ca082-122">Поставщик настраиваемой политики — Иаусоризатионполиципровидер</span><span class="sxs-lookup"><span data-stu-id="ca082-122">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="ca082-123">Схемы и параметры динамической проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="ca082-123">Dynamic authentication schemes and options</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="ca082-124">Внешние утверждения</span><span class="sxs-lookup"><span data-stu-id="ca082-124">External claims</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="ca082-125">Выбор между файлами cookie и другой схемой проверки подлинности на основе запроса</span><span class="sxs-lookup"><span data-stu-id="ca082-125">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="ca082-126">Разрешает доступ к статическим файлам</span><span class="sxs-lookup"><span data-stu-id="ca082-126">Restricts access to static files</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="ca082-127">Запуск шаблонов</span><span class="sxs-lookup"><span data-stu-id="ca082-127">Run the samples</span></span>

* <span data-ttu-id="ca082-128">Выберите [ветвь](https://github.com/dotnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="ca082-128">Select a [branch](https://github.com/dotnet/AspNetCore).</span></span> <span data-ttu-id="ca082-129">Например `release/2.2`.</span><span class="sxs-lookup"><span data-stu-id="ca082-129">For example, `release/2.2`</span></span>
* <span data-ttu-id="ca082-130">Клонировать или скачать [репозиторий ASP.NET Core](https://github.com/dotnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="ca082-130">Clone or download the [ASP.NET Core repository](https://github.com/dotnet/AspNetCore).</span></span>
* <span data-ttu-id="ca082-131">Убедитесь, что установлена версия [пакет SDK для .NET Core](https://www.microsoft.com/net/download/all) , соответствующая клону репозитория ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ca082-131">Verify you have installed the [.NET Core SDK](https://www.microsoft.com/net/download/all) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="ca082-132">Перейдите к примеру в *AspNetCore/src/Security/Samples* и запустите пример с `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="ca082-132">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end
