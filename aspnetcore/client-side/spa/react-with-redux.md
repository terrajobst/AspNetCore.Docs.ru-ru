---
title: Использование шаблона проекта React и Redux с ASP.NET Core
author: SteveSandersonMS
description: Сведения о начале работы с шаблоном проекта одностраничного приложения (SPA) ASP.NET Core для React с Redux и create-react-app.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/react-with-redux
ms.openlocfilehash: dab3d20865250aae548bff4614e631dd7c73b46f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291487"
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a><span data-ttu-id="7a46d-103">Использование шаблона проекта React и Redux с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7a46d-103">Use the React-with-Redux project template with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="7a46d-104">Информация о шаблоне проекта React и Redux, который включен в ASP.NET Core 2.0, в этой документации отсутствует.</span><span class="sxs-lookup"><span data-stu-id="7a46d-104">This documentation isn't about the React-with-Redux project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="7a46d-105">Речь идет о более новом шаблоне React и Redux, который можно обновить вручную.</span><span class="sxs-lookup"><span data-stu-id="7a46d-105">It's about the newer React-with-Redux template to which you can update manually.</span></span> <span data-ttu-id="7a46d-106">Этот шаблон по умолчанию включен в ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="7a46d-106">The template is included in ASP.NET Core 2.1 by default.</span></span>

::: moniker-end

<span data-ttu-id="7a46d-107">Обновленный шаблон проекта React и Redux служит удобной отправной точкой для создания приложений ASP.NET Core на основе конвенций React, Redux и [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA), позволяющих реализовать полноценный пользовательский интерфейс на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="7a46d-107">The updated React-with-Redux project template provides a convenient starting point for ASP.NET Core apps using React, Redux, and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="7a46d-108">За исключением команд создания проекта, все сведения о шаблоне React и Redux совпадают с данными о шаблоне React.</span><span class="sxs-lookup"><span data-stu-id="7a46d-108">With the exception of the project creation command, all information about the React-with-Redux template is the same as the React template.</span></span> <span data-ttu-id="7a46d-109">Чтобы создать проект такого типа, выполните `dotnet new reactredux` вместо `dotnet new react`.</span><span class="sxs-lookup"><span data-stu-id="7a46d-109">To create this project type, run `dotnet new reactredux` instead of `dotnet new react`.</span></span> <span data-ttu-id="7a46d-110">Дополнительные сведения о функциональности обоих шаблонов на основе React вы найдете в [документации по шаблону React](xref:spa/react).</span><span class="sxs-lookup"><span data-stu-id="7a46d-110">For more information about the functionality common to both React-based templates, see [React template documentation](xref:spa/react).</span></span>
