---
title: "Используйте шаблон проекта реагирование на них с локализации"
author: SteveSandersonMS
description: "Сведения о начале работы с использованием шаблона проекта ASP.NET Core одной страницы приложений (SPA) для действия с Redux и создание-реагирование на них приложения."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react-with-redux
ms.openlocfilehash: f52f4a866b8bb2adeee19548df32f3e7a91761d1
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/23/2018
---
# <a name="use-the-react-with-redux-project-template"></a><span data-ttu-id="4cac6-103">Используйте шаблон проекта реагирование на них с локализации</span><span class="sxs-lookup"><span data-stu-id="4cac6-103">Use the React-with-Redux project template</span></span>

> [!NOTE]
> <span data-ttu-id="4cac6-104">В этой документации о шаблоне реагирование на них с локализации проекта отсутствует в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="4cac6-104">This documentation isn't about the React-with-Redux project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="4cac6-105">Дело новым шаблоном реагирование на них с локализации, к которому можно обновить вручную.</span><span class="sxs-lookup"><span data-stu-id="4cac6-105">It's about the newer React-with-Redux template to which you can update manually.</span></span> <span data-ttu-id="4cac6-106">Шаблон по умолчанию включены в ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="4cac6-106">The template is included in ASP.NET Core 2.1 by default.</span></span>

<span data-ttu-id="4cac6-107">Обновленный шаблон проекта реагирование на них с Redux предоставляет удобную начальную точку для приложений ASP.NET Core с помощью реагировать, Redux, и [создать реагирование на них приложении](https://github.com/facebookincubator/create-react-app) (CRA) соглашения для реализации полнофункционального клиентского пользовательского интерфейса (UI).</span><span class="sxs-lookup"><span data-stu-id="4cac6-107">The updated React-with-Redux project template provides a convenient starting point for ASP.NET Core apps using React, Redux, and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="4cac6-108">За исключением команду создания проекта все сведения о шаблоне реагирование на них с локализации совпадает шаблона реагирование на них.</span><span class="sxs-lookup"><span data-stu-id="4cac6-108">With the exception of the project creation command, all information about the React-with-Redux template is the same as the React template.</span></span> <span data-ttu-id="4cac6-109">Чтобы создать этот тип проекта, запустите `dotnet new reactredux` вместо `dotnet new react`.</span><span class="sxs-lookup"><span data-stu-id="4cac6-109">To create this project type, run `dotnet new reactredux` instead of `dotnet new react`.</span></span> <span data-ttu-id="4cac6-110">Дополнительные сведения о функции, общие для обоих шаблонов на основе реагирование на них см. в разделе [реагировать документации по шаблону](xref:spa/react).</span><span class="sxs-lookup"><span data-stu-id="4cac6-110">For more information about the functionality common to both React-based templates, see [React template documentation](xref:spa/react).</span></span>
