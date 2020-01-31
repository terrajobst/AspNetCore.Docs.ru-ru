---
title: Веб-пакет SDK ASP.NET Core
author: Rick-Anderson
description: Общие сведения о Microsoft. NET. SDK. Web.
ms.author: riande
ms.date: 01/25/2020
no-loc:
- Blazor
uid: razor-pages/web-sdk
ms.openlocfilehash: 6a9d531efd2188aed525c949bb124914c31119db
ms.sourcegitcommit: fe41cff0b99f3920b727286944e5b652ca301640
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2020
ms.locfileid: "76869770"
---
# <a name="aspnet-core-web-sdk"></a><span data-ttu-id="bac64-103">Веб-пакет SDK ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bac64-103">ASP.NET Core Web SDK</span></span>

### <a name="overview"></a><span data-ttu-id="bac64-104">Обзор</span><span class="sxs-lookup"><span data-stu-id="bac64-104">Overview</span></span>

<span data-ttu-id="bac64-105">`Microsoft.NET.Sdk.Web` — это [пакет SDK проекта MSBuild](https://docs.microsoft.com/visualstudio/msbuild/how-to-use-project-sdk) для создания приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bac64-105">`Microsoft.NET.Sdk.Web` is an [MSBuild project SDK](https://docs.microsoft.com/visualstudio/msbuild/how-to-use-project-sdk) for building ASP.NET Core apps.</span></span> <span data-ttu-id="bac64-106">Можно создать ASP.NET Core приложение без этого пакета SDK, однако веб-пакет SDK будет следующим:</span><span class="sxs-lookup"><span data-stu-id="bac64-106">It's possible to build an ASP.NET Core app without this SDK, however, the Web SDK is:</span></span>

* <span data-ttu-id="bac64-107">Приспособлено к обеспечению первого класса.</span><span class="sxs-lookup"><span data-stu-id="bac64-107">Tailored towards providing a first-class experience.</span></span>
* <span data-ttu-id="bac64-108">Рекомендуемый целевой объект для большинства пользователей.</span><span class="sxs-lookup"><span data-stu-id="bac64-108">The recommended target for most users.</span></span>

<span data-ttu-id="bac64-109">Использование Web. SDK в проекте:</span><span class="sxs-lookup"><span data-stu-id="bac64-109">Use the Web.SDK in a project:</span></span>

  ```xml
  <Project Sdk="Microsoft.NET.Sdk.Web">
    <!-- omitted for brevity -->
  </Project>
  ```

<span data-ttu-id="bac64-110">Функции, доступные с помощью веб-пакета SDK:</span><span class="sxs-lookup"><span data-stu-id="bac64-110">Features enabled by using the Web SDK:</span></span>

* <span data-ttu-id="bac64-111">Проекты, предназначенные для .NET Core 3,0 или более поздней версии, неявно ссылаются на:</span><span class="sxs-lookup"><span data-stu-id="bac64-111">Projects targeting .NET Core 3.0 or later implicitly reference:</span></span>

  * <span data-ttu-id="bac64-112">[ASP.NET Core общая платформа](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bac64-112">The [ASP.NET Core shared framework](xref:fundamentals/metapackage-app).</span></span>
  * <span data-ttu-id="bac64-113">[Анализаторы](/visualstudio/extensibility/getting-started-with-roslyn-analyzers) , предназначенные для создания ASP.NET Core приложений.</span><span class="sxs-lookup"><span data-stu-id="bac64-113">[Analyzers](/visualstudio/extensibility/getting-started-with-roslyn-analyzers) designed for building ASP.NET Core apps.</span></span>
* <span data-ttu-id="bac64-114">Веб-пакет SDK импортирует целевые объекты MSBuild, которые позволяют использовать профили публикации и публикацию с помощью WebDeploy.</span><span class="sxs-lookup"><span data-stu-id="bac64-114">The Web SDK imports MSBuild targets that enable the use of publish profiles and publishing using WebDeploy.</span></span>

### <a name="properties"></a><span data-ttu-id="bac64-115">Свойства</span><span class="sxs-lookup"><span data-stu-id="bac64-115">Properties</span></span>

| <span data-ttu-id="bac64-116">Идентификаторы</span><span class="sxs-lookup"><span data-stu-id="bac64-116">Property</span></span> | <span data-ttu-id="bac64-117">Описание</span><span class="sxs-lookup"><span data-stu-id="bac64-117">Description</span></span> |
| -------- | ----------- |
| `DisableImplicitFrameworkReferences` | <span data-ttu-id="bac64-118">Отключает неявную ссылку на `Microsoft.AspNetCore.App` общую платформу.</span><span class="sxs-lookup"><span data-stu-id="bac64-118">Disables implicit reference to the `Microsoft.AspNetCore.App` shared framework.</span></span> |
| `DisableImplicitAspNetCoreAnalyzers` | <span data-ttu-id="bac64-119">Отключает неявную ссылку на анализаторы ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bac64-119">Disables implicit reference to ASP.NET Core analyzers.</span></span> |
| `DisableImplicitComponentsAnalyzers` | <span data-ttu-id="bac64-120">Отключает неявную ссылку на анализаторы компонентов Razor при создании приложений Blazor (сервера).</span><span class="sxs-lookup"><span data-stu-id="bac64-120">Disables implicit reference to Razor Components analyzers when building Blazor (server) applications.</span></span> |
