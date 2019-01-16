---
title: Использование шаблона проекта React и Redux с ASP.NET Core
author: SteveSandersonMS
description: Сведения о начале работы с шаблоном проекта одностраничного приложения (SPA) ASP.NET Core для React с Redux и create-react-app.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/react-with-redux
ms.openlocfilehash: c1aedcf1e14a9e7b339b60dd02c4267cd5945a49
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341624"
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a><span data-ttu-id="9b72a-103">Использование шаблона проекта React и Redux с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9b72a-103">Use the React-with-Redux project template with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="9b72a-104">Эта документация не относятся к шаблоне проекта React с Redux, включенные в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="9b72a-104">This documentation doesn't pertain to the React-with-Redux project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="9b72a-105">Это имеет отношение к новой React с Redux шаблон, который можно обновить вручную.</span><span class="sxs-lookup"><span data-stu-id="9b72a-105">It pertains to the newer React-with-Redux template that you can update manually.</span></span> <span data-ttu-id="9b72a-106">Шаблон ASP.NET Core 2.1 или более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="9b72a-106">The template is available in ASP.NET Core 2.1 or later.</span></span>

::: moniker-end

<span data-ttu-id="9b72a-107">Обновленный шаблон проекта React и Redux служит удобной отправной точкой для создания приложений ASP.NET Core на основе конвенций React, Redux и [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA), позволяющих реализовать полноценный пользовательский интерфейс на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="9b72a-107">The updated React-with-Redux project template provides a convenient starting point for ASP.NET Core apps using React, Redux, and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="9b72a-108">За исключением команд создания проекта, все сведения о шаблоне React и Redux совпадают с данными о шаблоне React.</span><span class="sxs-lookup"><span data-stu-id="9b72a-108">With the exception of the project creation command, all information about the React-with-Redux template is the same as the React template.</span></span> <span data-ttu-id="9b72a-109">Чтобы создать проект такого типа, выполните `dotnet new reactredux` вместо `dotnet new react`.</span><span class="sxs-lookup"><span data-stu-id="9b72a-109">To create this project type, run `dotnet new reactredux` instead of `dotnet new react`.</span></span> <span data-ttu-id="9b72a-110">Дополнительные сведения о функциональности обоих шаблонов на основе React вы найдете в [документации по шаблону React](xref:spa/react).</span><span class="sxs-lookup"><span data-stu-id="9b72a-110">For more information about the functionality common to both React-based templates, see [React template documentation](xref:spa/react).</span></span>

<span data-ttu-id="9b72a-111">Сведения о настройке React с Redux составные части приложения в IIS, см. в разделе [2.1 ReactRedux шаблона: Не удается использовать SPA в службах IIS (aspnet/Templating &num;555)](https://github.com/aspnet/Templating/issues/555).</span><span class="sxs-lookup"><span data-stu-id="9b72a-111">For information on configuring a React-with-Redux sub-application in IIS, see [ReactRedux Template 2.1: Unable to use SPA on IIS (aspnet/Templating &num;555)](https://github.com/aspnet/Templating/issues/555).</span></span>
