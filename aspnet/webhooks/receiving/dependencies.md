---
uid: webhooks/receiving/dependencies
title: Зависимости получателя ASP.NET веб-перехватчиков | Документация Майкрософт
author: rick-anderson
description: Зависимости получателя и внедрение зависимостей в ASP.NET веб-перехватчики.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.technology: ''
ms.openlocfilehash: cf45e3d2e45e4b7882b42d9aa0931e18c08f76b5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401191"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="d5068-103">Зависимости получателя ASP.NET веб-перехватчиков</span><span class="sxs-lookup"><span data-stu-id="d5068-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="d5068-104">Веб-перехватчики Microsoft ASP.NET разработан с помощью внедрения зависимостей в виду.</span><span class="sxs-lookup"><span data-stu-id="d5068-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="d5068-105">Большинство зависимостей в системе могут быть заменены альтернативных реализаций, с помощью надежной подсистемы внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="d5068-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="d5068-106">См. в разделе [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) список зависимости получателя.</span><span class="sxs-lookup"><span data-stu-id="d5068-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="d5068-107">Если зависимости не был зарегистрирован, используется реализация по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d5068-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="d5068-108">См. в разделе [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) список реализации по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d5068-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
