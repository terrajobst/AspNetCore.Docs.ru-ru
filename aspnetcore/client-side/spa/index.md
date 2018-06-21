---
title: Использование шаблонов одностраничных приложений с ASP.NET Core
author: SteveSandersonMS
description: Узнайте, как установить и начать использовать шаблоны проектов одностраничных приложений (SPA) в ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/index
ms.openlocfilehash: ab164ae5d2df47739ec04b32cd21835ffdf9f87f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291447"
---
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a><span data-ttu-id="83f4f-103">Использование шаблонов одностраничных приложений с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="83f4f-103">Use the Single Page Application templates with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="83f4f-104">Выпущенный пакет SDK для .NET Core 2.0.x содержит более ранние шаблоны проектов Angular, React и React с Redux.</span><span class="sxs-lookup"><span data-stu-id="83f4f-104">The released .NET Core 2.0.x SDK includes older project templates for Angular, React, and React with Redux.</span></span> <span data-ttu-id="83f4f-105">Эта документация не относится к этим шаблонам.</span><span class="sxs-lookup"><span data-stu-id="83f4f-105">This documentation isn't about those older project templates.</span></span> <span data-ttu-id="83f4f-106">Она относится к новейшим шаблонам Angular, React и React с Redux, которые можно установить в ASP.NET Core 2.0 вручную.</span><span class="sxs-lookup"><span data-stu-id="83f4f-106">This documentation is for the latest Angular, React, and React with Redux templates, which can be installed manually into ASP.NET Core 2.0.</span></span> <span data-ttu-id="83f4f-107">ASP.NET Core 2.1 включает обновленные шаблоны по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="83f4f-107">The templates are included by default with ASP.NET Core 2.1.</span></span>

::: moniker-end

## <a name="prerequisites"></a><span data-ttu-id="83f4f-108">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="83f4f-108">Prerequisites</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="83f4f-109">[Node.js](https://nodejs.org) 6 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="83f4f-109">[Node.js](https://nodejs.org), version 6 or later</span></span>

## <a name="installation"></a><span data-ttu-id="83f4f-110">Установка</span><span class="sxs-lookup"><span data-stu-id="83f4f-110">Installation</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="83f4f-111">Шаблоны уже установлены вместе с ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="83f4f-111">The templates are already installed with ASP.NET Core 2.1.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="83f4f-112">Если вы используете ASP.NET Core 2.0, то чтобы установить обновленные шаблоны Angular, React и React с Redux для ASP.NET Core, выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="83f4f-112">If you have ASP.NET Core 2.0, run the following command to install the updated ASP.NET Core templates for Angular, React, and React with Redux:</span></span>

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

::: moniker-end

## <a name="use-the-templates"></a><span data-ttu-id="83f4f-113">Использование шаблонов</span><span class="sxs-lookup"><span data-stu-id="83f4f-113">Use the templates</span></span>

* [<span data-ttu-id="83f4f-114">Использование шаблона проекта Angular</span><span class="sxs-lookup"><span data-stu-id="83f4f-114">Use the Angular project template</span></span>](xref:spa/angular)
* [<span data-ttu-id="83f4f-115">Использование шаблона проекта React</span><span class="sxs-lookup"><span data-stu-id="83f4f-115">Use the React project template</span></span>](xref:spa/react)
* [<span data-ttu-id="83f4f-116">Использование шаблона проекта React и Redux</span><span class="sxs-lookup"><span data-stu-id="83f4f-116">Use the React with Redux project template</span></span>](xref:spa/react-with-redux)
