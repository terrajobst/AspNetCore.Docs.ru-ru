---
title: Веб-пакет SDK для ASP.NET Core
author: Rick-Anderson
description: Общие сведения о пакете Microsoft.NET.Sdk.Web.
ms.author: riande
ms.date: 01/25/2020
no-loc:
- Blazor
uid: razor-pages/web-sdk
ms.openlocfilehash: 6a9d531efd2188aed525c949bb124914c31119db
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78648208"
---
# <a name="aspnet-core-web-sdk"></a><span data-ttu-id="cc51a-103">Веб-пакет SDK для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cc51a-103">ASP.NET Core Web SDK</span></span>

### <a name="overview"></a><span data-ttu-id="cc51a-104">Обзор</span><span class="sxs-lookup"><span data-stu-id="cc51a-104">Overview</span></span>

<span data-ttu-id="cc51a-105">`Microsoft.NET.Sdk.Web` — это [пакет SDK проекта MSBuild](https://docs.microsoft.com/visualstudio/msbuild/how-to-use-project-sdk) для разработки приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cc51a-105">`Microsoft.NET.Sdk.Web` is an [MSBuild project SDK](https://docs.microsoft.com/visualstudio/msbuild/how-to-use-project-sdk) for building ASP.NET Core apps.</span></span> <span data-ttu-id="cc51a-106">Приложения ASP.NET Core можно создавать и без этого пакета SDK, однако веб-пакет SDK дает следующие преимущества:</span><span class="sxs-lookup"><span data-stu-id="cc51a-106">It's possible to build an ASP.NET Core app without this SDK, however, the Web SDK is:</span></span>

* <span data-ttu-id="cc51a-107">обеспечивает максимальное удобство разработки;</span><span class="sxs-lookup"><span data-stu-id="cc51a-107">Tailored towards providing a first-class experience.</span></span>
* <span data-ttu-id="cc51a-108">рекомендуется для большинства пользователей.</span><span class="sxs-lookup"><span data-stu-id="cc51a-108">The recommended target for most users.</span></span>

<span data-ttu-id="cc51a-109">Используйте Web.SDK в проекте:</span><span class="sxs-lookup"><span data-stu-id="cc51a-109">Use the Web.SDK in a project:</span></span>

  ```xml
  <Project Sdk="Microsoft.NET.Sdk.Web">
    <!-- omitted for brevity -->
  </Project>
  ```

<span data-ttu-id="cc51a-110">Возможности, обеспечиваемые веб-пакетом SDK:</span><span class="sxs-lookup"><span data-stu-id="cc51a-110">Features enabled by using the Web SDK:</span></span>

* <span data-ttu-id="cc51a-111">Проекты, предназначенные для .NET Core 3.0 или более поздней версии, имеют неявные ссылки на следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="cc51a-111">Projects targeting .NET Core 3.0 or later implicitly reference:</span></span>

  * <span data-ttu-id="cc51a-112">[общая платформа ASP.NET Core](xref:fundamentals/metapackage-app);</span><span class="sxs-lookup"><span data-stu-id="cc51a-112">The [ASP.NET Core shared framework](xref:fundamentals/metapackage-app).</span></span>
  * <span data-ttu-id="cc51a-113">[анализаторы](/visualstudio/extensibility/getting-started-with-roslyn-analyzers), предназначенные для создания приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cc51a-113">[Analyzers](/visualstudio/extensibility/getting-started-with-roslyn-analyzers) designed for building ASP.NET Core apps.</span></span>
* <span data-ttu-id="cc51a-114">Веб-пакет SDK импортирует целевые объекты MSBuild, которые позволяют использовать профили публикации и выполнять публикацию с помощью WebDeploy.</span><span class="sxs-lookup"><span data-stu-id="cc51a-114">The Web SDK imports MSBuild targets that enable the use of publish profiles and publishing using WebDeploy.</span></span>

### <a name="properties"></a><span data-ttu-id="cc51a-115">Свойства</span><span class="sxs-lookup"><span data-stu-id="cc51a-115">Properties</span></span>

| <span data-ttu-id="cc51a-116">Свойство.</span><span class="sxs-lookup"><span data-stu-id="cc51a-116">Property</span></span> | <span data-ttu-id="cc51a-117">Описание</span><span class="sxs-lookup"><span data-stu-id="cc51a-117">Description</span></span> |
| -------- | ----------- |
| `DisableImplicitFrameworkReferences` | <span data-ttu-id="cc51a-118">Отключает неявную ссылку на общую платформу `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="cc51a-118">Disables implicit reference to the `Microsoft.AspNetCore.App` shared framework.</span></span> |
| `DisableImplicitAspNetCoreAnalyzers` | <span data-ttu-id="cc51a-119">Отключает неявную ссылку на анализаторы ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cc51a-119">Disables implicit reference to ASP.NET Core analyzers.</span></span> |
| `DisableImplicitComponentsAnalyzers` | <span data-ttu-id="cc51a-120">Отключает неявную ссылку на анализаторы компонентов Razor при сборке приложений Blazor (серверных).</span><span class="sxs-lookup"><span data-stu-id="cc51a-120">Disables implicit reference to Razor Components analyzers when building Blazor (server) applications.</span></span> |