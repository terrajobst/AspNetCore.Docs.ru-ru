---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: Создание ограничения маршрута Custom (C#) | Документы Microsoft
author: StephenWalther
description: Стивен Вальтер показано, как можно создать ограничение настраиваемый маршрут. Мы реализуем простой пользовательский ограничение, которое блокирует маршрут соответствует w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 4c120a102b117433b6774f2ea7800f1c4a609f8b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874347"
---
<a name="creating-a-custom-route-constraint-c"></a><span data-ttu-id="5ce76-104">Создание ограничения маршрута Custom (C#)</span><span class="sxs-lookup"><span data-stu-id="5ce76-104">Creating a Custom Route Constraint (C#)</span></span>
====================
<span data-ttu-id="5ce76-105">по [Стивен Вальтер](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="5ce76-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="5ce76-106">Стивен Вальтер показано, как можно создать ограничение настраиваемый маршрут.</span><span class="sxs-lookup"><span data-stu-id="5ce76-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="5ce76-107">Мы реализовали простой пользовательский ограничение, которое предотвращает маршрут соответствует при запрос браузера с удаленного компьютера.</span><span class="sxs-lookup"><span data-stu-id="5ce76-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>


<span data-ttu-id="5ce76-108">Целью данного учебника является демонстрация как можно создать ограничение настраиваемый маршрут.</span><span class="sxs-lookup"><span data-stu-id="5ce76-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="5ce76-109">Настраиваемый маршрут ограничение позволяет предотвратить сравниваемыми, если совпадают некоторые настраиваемого условия маршрута.</span><span class="sxs-lookup"><span data-stu-id="5ce76-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="5ce76-110">В этом учебнике мы создания ограничения маршрута Localhost.</span><span class="sxs-lookup"><span data-stu-id="5ce76-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="5ce76-111">Ограничения маршрута Localhost соответствует только запросы от локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="5ce76-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="5ce76-112">Удаленные запросы из через Интернет не соответствуют.</span><span class="sxs-lookup"><span data-stu-id="5ce76-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="5ce76-113">Реализовать ограничение настраиваемый маршрут, реализовав интерфейс IRouteConstraint.</span><span class="sxs-lookup"><span data-stu-id="5ce76-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="5ce76-114">Это очень простой интерфейс, который описывает один метод:</span><span class="sxs-lookup"><span data-stu-id="5ce76-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

<span data-ttu-id="5ce76-115">Метод возвращает логическое значение.</span><span class="sxs-lookup"><span data-stu-id="5ce76-115">The method returns a Boolean value.</span></span> <span data-ttu-id="5ce76-116">Если возвращается значение false, маршрута, связанного с ограничением не соответствует запросу браузера.</span><span class="sxs-lookup"><span data-stu-id="5ce76-116">If you return false, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="5ce76-117">Ограничение Localhost содержится в листинге 1.</span><span class="sxs-lookup"><span data-stu-id="5ce76-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="5ce76-118">**Листинг 1 - LocalhostConstraint.cs**</span><span class="sxs-lookup"><span data-stu-id="5ce76-118">**Listing 1 - LocalhostConstraint.cs**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

<span data-ttu-id="5ce76-119">Ограничение в список 1 использует свойство IsLocal класса HttpRequest.</span><span class="sxs-lookup"><span data-stu-id="5ce76-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="5ce76-120">Это свойство возвращает значение true, если IP-адрес запроса либо 127.0.0.1 или если IP-адрес запроса будет таким же, как IP-адрес сервера.</span><span class="sxs-lookup"><span data-stu-id="5ce76-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="5ce76-121">Можно использовать пользовательские ограничения маршрута, определен в файле Global.asax.</span><span class="sxs-lookup"><span data-stu-id="5ce76-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="5ce76-122">Файл Global.asax в списке 2 использует ограничение Localhost чтобы запретить запросе страницы администрирования, если они не выполняете запрос с локального сервера.</span><span class="sxs-lookup"><span data-stu-id="5ce76-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="5ce76-123">Например запрос /Admin/DeleteAll завершится ошибкой при внесении с удаленного сервера.</span><span class="sxs-lookup"><span data-stu-id="5ce76-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="5ce76-124">**Листинг 2 - Global.asax**</span><span class="sxs-lookup"><span data-stu-id="5ce76-124">**Listing 2 - Global.asax**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

<span data-ttu-id="5ce76-125">Ограничение Localhost используется в определении маршрута администратора.</span><span class="sxs-lookup"><span data-stu-id="5ce76-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="5ce76-126">Запрос удаленной браузера, не будет соответствовать этот маршрут.</span><span class="sxs-lookup"><span data-stu-id="5ce76-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="5ce76-127">Однако поймите, что тот же запрос может совпадать с других маршрутов, указанных в файле Global.asax.</span><span class="sxs-lookup"><span data-stu-id="5ce76-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="5ce76-128">Важно понимать, что ограничение не позволяет сопоставить запрос на конкретный маршрут и не все маршруты, определенный в файле Global.asax.</span><span class="sxs-lookup"><span data-stu-id="5ce76-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="5ce76-129">Обратите внимание, что маршрут по умолчанию был закомментирован из файла Global.asax в списке 2.</span><span class="sxs-lookup"><span data-stu-id="5ce76-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="5ce76-130">При включении маршрута по умолчанию маршрута по умолчанию будет соответствовать запросов для администрирования контроллера.</span><span class="sxs-lookup"><span data-stu-id="5ce76-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="5ce76-131">В этом случае удаленные пользователи по-прежнему удалось вызвать действий администратора контроллера, несмотря на то, что их запросы не соответствует маршруту администратора.</span><span class="sxs-lookup"><span data-stu-id="5ce76-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5ce76-132">[Назад](creating-a-route-constraint-cs.md)
> [Вперед](asp-net-mvc-controller-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="5ce76-132">[Previous](creating-a-route-constraint-cs.md)
[Next](asp-net-mvc-controller-overview-vb.md)</span></span>
