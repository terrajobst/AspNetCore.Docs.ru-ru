---
title: Использование шаблонов одностраничных приложений с ASP.NET Core
author: SteveSandersonMS
description: Узнайте, как установить и начать использовать шаблоны проектов одностраничных приложений (SPA) в ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/index
ms.openlocfilehash: f7bb23e9001c7606c3e622bf4575a4debec56ccd
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/27/2018
---
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a><span data-ttu-id="5313c-103">Использование шаблонов одностраничных приложений с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5313c-103">Use the Single Page Application templates with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="5313c-104">Выпущенный пакет SDK для .NET Core 2.0.x содержит более ранние шаблоны проектов Angular, React и React с Redux.</span><span class="sxs-lookup"><span data-stu-id="5313c-104">The released .NET Core 2.0.x SDK includes older project templates for Angular, React, and React with Redux.</span></span> <span data-ttu-id="5313c-105">Эта документация не относится к этим шаблонам.</span><span class="sxs-lookup"><span data-stu-id="5313c-105">This documentation isn't about those older project templates.</span></span> <span data-ttu-id="5313c-106">Она относится к новейшим шаблонам Angular, React и React с Redux, которые можно установить в ASP.NET Core 2.0 вручную.</span><span class="sxs-lookup"><span data-stu-id="5313c-106">This documentation is for the latest Angular, React, and React with Redux templates, which can be installed manually into ASP.NET Core 2.0.</span></span> <span data-ttu-id="5313c-107">ASP.NET Core 2.1 включает обновленные шаблоны по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="5313c-107">The templates are included by default with ASP.NET Core 2.1.</span></span>

::: moniker-end

## <a name="prerequisites"></a><span data-ttu-id="5313c-108">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="5313c-108">Prerequisites</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="5313c-109">[Node.js](https://nodejs.org) 6 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="5313c-109">[Node.js](https://nodejs.org), version 6 or later</span></span>

## <a name="installation"></a><span data-ttu-id="5313c-110">Установка</span><span class="sxs-lookup"><span data-stu-id="5313c-110">Installation</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="5313c-111">Шаблоны уже установлены вместе с ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="5313c-111">The templates are already installed with ASP.NET Core 2.1.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="5313c-112">Если вы используете ASP.NET Core 2.0, то чтобы установить обновленные шаблоны Angular, React и React с Redux для ASP.NET Core, выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="5313c-112">If you have ASP.NET Core 2.0, run the following command to install the updated ASP.NET Core templates for Angular, React, and React with Redux:</span></span>

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

::: moniker-end

## <a name="use-the-templates"></a><span data-ttu-id="5313c-113">Использование шаблонов</span><span class="sxs-lookup"><span data-stu-id="5313c-113">Use the templates</span></span>

* [<span data-ttu-id="5313c-114">Использование шаблона проекта Angular</span><span class="sxs-lookup"><span data-stu-id="5313c-114">Use the Angular project template</span></span>](xref:spa/angular)
* [<span data-ttu-id="5313c-115">Использование шаблона проекта React</span><span class="sxs-lookup"><span data-stu-id="5313c-115">Use the React project template</span></span>](xref:spa/react)
* [<span data-ttu-id="5313c-116">Использование шаблона проекта React и Redux</span><span class="sxs-lookup"><span data-stu-id="5313c-116">Use the React with Redux project template</span></span>](xref:spa/react-with-redux)
