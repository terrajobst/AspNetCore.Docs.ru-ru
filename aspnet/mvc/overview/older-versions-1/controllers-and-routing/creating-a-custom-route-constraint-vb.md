---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: Создание пользовательского ограничения маршрута (Visual Basic) | Документация Майкрософт
author: StephenWalther
description: Стивен Вальтер демонстрирует, как можно создать ограничение настраиваемый маршрут. Мы реализуем простой пользовательский ограничение, которое запрещает маршрут соответствует w...
ms.author: aspnetcontent
ms.date: 02/16/2009
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 72389d11467cbf7baea4cc9452266edb8ab81125
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840479"
---
<a name="creating-a-custom-route-constraint-vb"></a><span data-ttu-id="bb996-104">Создание пользовательского ограничения маршрута (VB)</span><span class="sxs-lookup"><span data-stu-id="bb996-104">Creating a Custom Route Constraint (VB)</span></span>
====================
<span data-ttu-id="bb996-105">по [Стивен Вальтер](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="bb996-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="bb996-106">Стивен Вальтер демонстрирует, как можно создать ограничение настраиваемый маршрут.</span><span class="sxs-lookup"><span data-stu-id="bb996-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="bb996-107">Мы реализуем простой пользовательский ограничение, которое предотвращает маршрут соответствует при запрос браузера с удаленного компьютера.</span><span class="sxs-lookup"><span data-stu-id="bb996-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>


<span data-ttu-id="bb996-108">Целью данного учебника — продемонстрировать, как можно создать ограничение настраиваемый маршрут.</span><span class="sxs-lookup"><span data-stu-id="bb996-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="bb996-109">Настраиваемый маршрут ограничение позволяет предотвратить маршрут соответствует, если только некоторые пользовательские условия.</span><span class="sxs-lookup"><span data-stu-id="bb996-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="bb996-110">В этом руководстве мы создадим ограничения маршрута Localhost.</span><span class="sxs-lookup"><span data-stu-id="bb996-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="bb996-111">Ограничения маршрута Localhost соответствует только запросы, поступающие с локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="bb996-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="bb996-112">Удаленные запросы из Интернета не совпадают.</span><span class="sxs-lookup"><span data-stu-id="bb996-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="bb996-113">Реализуйте ограничение настраиваемый маршрут, реализовав интерфейс IRouteConstraint.</span><span class="sxs-lookup"><span data-stu-id="bb996-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="bb996-114">Это очень простой интерфейс, который описывает один метод:</span><span class="sxs-lookup"><span data-stu-id="bb996-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

<span data-ttu-id="bb996-115">Метод возвращает значение типа Boolean.</span><span class="sxs-lookup"><span data-stu-id="bb996-115">The method returns a Boolean value.</span></span> <span data-ttu-id="bb996-116">Если вы возвращаете False, маршрут, связанный с ограничением не будет соответствовать запрос браузера.</span><span class="sxs-lookup"><span data-stu-id="bb996-116">If you return False, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="bb996-117">Ограничение Localhost содержится в листинге 1.</span><span class="sxs-lookup"><span data-stu-id="bb996-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="bb996-118">**В листинге 1 - LocalhostConstraint.vb**</span><span class="sxs-lookup"><span data-stu-id="bb996-118">**Listing 1 - LocalhostConstraint.vb**</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

<span data-ttu-id="bb996-119">Ограничение в листинге 1 использует свойство IsLocal класса HttpRequest.</span><span class="sxs-lookup"><span data-stu-id="bb996-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="bb996-120">Это свойство возвращает значение true, если IP-адрес запроса является либо 127.0.0.1 или если IP-адрес запроса совпадает с IP-адрес сервера.</span><span class="sxs-lookup"><span data-stu-id="bb996-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="bb996-121">Можно использовать пользовательские ограничения маршрута, указанным в файле Global.asax.</span><span class="sxs-lookup"><span data-stu-id="bb996-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="bb996-122">Файл Global.asax в листинге 2 использует ограничение Localhost, чтобы запретить запросе страницы администрирования, если только они сделать запрос с локального сервера.</span><span class="sxs-lookup"><span data-stu-id="bb996-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="bb996-123">Например запрос /Admin/DeleteAll завершится ошибкой при внесении с удаленного сервера.</span><span class="sxs-lookup"><span data-stu-id="bb996-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="bb996-124">**В листинге 2 - Global.asax**</span><span class="sxs-lookup"><span data-stu-id="bb996-124">**Listing 2 - Global.asax**</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

<span data-ttu-id="bb996-125">Ограничение Localhost используется в определении маршрута администратора.</span><span class="sxs-lookup"><span data-stu-id="bb996-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="bb996-126">Этот маршрут, не будет соответствовать запроса удаленного обозревателя.</span><span class="sxs-lookup"><span data-stu-id="bb996-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="bb996-127">Учтите, тем не менее, что другие маршруты, указанные в файле Global.asax может соответствовать тот же запрос.</span><span class="sxs-lookup"><span data-stu-id="bb996-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="bb996-128">Важно понимать, что ограничение предотвращает сопоставление запроса определенного маршрута, и не все маршруты, определенные в файле Global.asax.</span><span class="sxs-lookup"><span data-stu-id="bb996-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="bb996-129">Обратите внимание на то, что маршрут по умолчанию был закомментирован из файла Global.asax в листинге 2.</span><span class="sxs-lookup"><span data-stu-id="bb996-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="bb996-130">Если маршрут по умолчанию, маршрут по умолчанию будет соответствовать запросы для администратора контроллера.</span><span class="sxs-lookup"><span data-stu-id="bb996-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="bb996-131">В этом случае удаленные пользователи по-прежнему может заставить действия администратора контроллера, несмотря на то, что их запросы не соответствуют маршруту администратора.</span><span class="sxs-lookup"><span data-stu-id="bb996-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="bb996-132">Назад</span><span class="sxs-lookup"><span data-stu-id="bb996-132">Previous</span></span>](creating-a-route-constraint-vb.md)
