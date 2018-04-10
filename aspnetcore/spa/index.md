---
title: Использование шаблонов одностраничных приложений с ASP.NET Core
author: SteveSandersonMS
description: Узнайте, как установить и начать использовать шаблоны проектов одностраничных приложений (SPA) в ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/index
ms.openlocfilehash: eda4817de007f3c3184b2ba6ed6c97989ff17da5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a><span data-ttu-id="e443d-103">Использование шаблонов одностраничных приложений с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e443d-103">Use the Single Page Application templates with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="e443d-104">Выпущенный пакет SDK для .NET Core 2.0.x содержит более ранние шаблоны проектов Angular, React и React с Redux.</span><span class="sxs-lookup"><span data-stu-id="e443d-104">The released .NET Core 2.0.x SDK includes older project templates for Angular, React, and React with Redux.</span></span> <span data-ttu-id="e443d-105">Эта документация не относится к этим шаблонам.</span><span class="sxs-lookup"><span data-stu-id="e443d-105">This documentation isn't about those older project templates.</span></span> <span data-ttu-id="e443d-106">Она относится к новейшим шаблонам Angular, React и React с Redux, которые можно установить в ASP.NET Core 2.0 вручную.</span><span class="sxs-lookup"><span data-stu-id="e443d-106">This documentation is for the latest Angular, React, and React with Redux templates, which can be installed manually into ASP.NET Core 2.0.</span></span> <span data-ttu-id="e443d-107">ASP.NET Core 2.1 включает обновленные шаблоны по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e443d-107">The templates are included by default with ASP.NET Core 2.1.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e443d-108">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="e443d-108">Prerequisites</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="e443d-109">[Node.js](https://nodejs.org) 6 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="e443d-109">[Node.js](https://nodejs.org), version 6 or later</span></span>

## <a name="installation"></a><span data-ttu-id="e443d-110">Установка</span><span class="sxs-lookup"><span data-stu-id="e443d-110">Installation</span></span>

<span data-ttu-id="e443d-111">Если вы используете ASP.NET Core 2.0, то чтобы установить обновленные шаблоны Angular, React и React с Redux для ASP.NET Core, выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="e443d-111">If you have ASP.NET Core 2.0, run the following command to install the updated ASP.NET Core templates for Angular, React, and React with Redux:</span></span>

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

## <a name="use-the-templates"></a><span data-ttu-id="e443d-112">Использование шаблонов</span><span class="sxs-lookup"><span data-stu-id="e443d-112">Use the templates</span></span>

- [<span data-ttu-id="e443d-113">Использование шаблона проекта Angular</span><span class="sxs-lookup"><span data-stu-id="e443d-113">Use the Angular project template</span></span>](xref:spa/angular)
- [<span data-ttu-id="e443d-114">Использование шаблона проекта React</span><span class="sxs-lookup"><span data-stu-id="e443d-114">Use the React project template</span></span>](xref:spa/react)
- [<span data-ttu-id="e443d-115">Использование шаблона проекта React и Redux</span><span class="sxs-lookup"><span data-stu-id="e443d-115">Use the React with Redux project template</span></span>](xref:spa/react-with-redux)
