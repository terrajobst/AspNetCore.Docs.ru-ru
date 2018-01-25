---
title: "Используйте шаблон проекта реагирование на них с локализации"
author: SteveSandersonMS
description: "Сведения о начале работы с использованием шаблона проекта ASP.NET Core одностраничного приложения (SPA) версии кандидата для действия с Redux и создание-реагирование на них приложения."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/06/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react-with-redux
ms.openlocfilehash: 71a8702b9f6e9a2b40f150026f1955450e0fa7a3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="use-the-react-with-redux-project-template-release-candidate"></a><span data-ttu-id="6c3bd-103">Используйте действия с локализации шаблон проекта (версия-кандидат)</span><span class="sxs-lookup"><span data-stu-id="6c3bd-103">Use the React-with-Redux project template (release candidate)</span></span>

> [!NOTE]
> <span data-ttu-id="6c3bd-104">В этой документации не о выпущенных реагирование на них с локализации шаблона проекта.</span><span class="sxs-lookup"><span data-stu-id="6c3bd-104">This documentation isn't about the released React-with-Redux project template.</span></span> <span data-ttu-id="6c3bd-105">**Данная документация является о версии-кандидате реагирование на них с локализации шаблона.**</span><span class="sxs-lookup"><span data-stu-id="6c3bd-105">**This documentation is about the release candidate of the React-with-Redux template.**</span></span> <span data-ttu-id="6c3bd-106">Мы надеемся, что для отправки в начале 2018 выпущенной версии.</span><span class="sxs-lookup"><span data-stu-id="6c3bd-106">We hope to ship the released version in early 2018.</span></span>

<span data-ttu-id="6c3bd-107">Обновленный шаблон проекта реагирование на них с Redux предоставляет удобную начальную точку для приложений ASP.NET Core с помощью реагировать, Redux, и [создать реагирование на них приложении](https://github.com/facebookincubator/create-react-app) (CRA) соглашения для реализации полнофункционального клиентского пользовательского интерфейса (UI).</span><span class="sxs-lookup"><span data-stu-id="6c3bd-107">The updated React-with-Redux project template provides a convenient starting point for ASP.NET Core apps using React, Redux, and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="6c3bd-108">За исключением команду создания проекта все сведения о шаблоне реагирование на них с локализации совпадает шаблона реагирование на них.</span><span class="sxs-lookup"><span data-stu-id="6c3bd-108">With the exception of the project creation command, all information about the React-with-Redux template is the same as the React template.</span></span> <span data-ttu-id="6c3bd-109">Чтобы создать этот тип проекта, запустите `dotnet new reactredux` вместо `dotnet new react`.</span><span class="sxs-lookup"><span data-stu-id="6c3bd-109">To create this project type, run `dotnet new reactredux` instead of `dotnet new react`.</span></span> <span data-ttu-id="6c3bd-110">Дополнительные сведения о функции, общие для обоих шаблонов на основе реагирование на них см. в разделе [реагировать документации по шаблону](xref:spa/react).</span><span class="sxs-lookup"><span data-stu-id="6c3bd-110">For more information about the functionality common to both React-based templates, see [React template documentation](xref:spa/react).</span></span>
