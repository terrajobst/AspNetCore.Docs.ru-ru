---
uid: webhooks/receiving/dependencies
title: Зависимости получателя ASP.NET веб-перехватчиков | Документация Майкрософт
author: rick-anderson
description: Зависимости получателя и внедрение зависимостей в ASP.NET веб-перехватчики.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: c44cfe3ed310aa728a989b108c410e8786e4f514
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835681"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="f22fd-103">Зависимости получателя ASP.NET веб-перехватчиков</span><span class="sxs-lookup"><span data-stu-id="f22fd-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="f22fd-104">Веб-перехватчики Microsoft ASP.NET разработан с помощью внедрения зависимостей в виду.</span><span class="sxs-lookup"><span data-stu-id="f22fd-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="f22fd-105">Большинство зависимостей в системе могут быть заменены альтернативных реализаций, с помощью надежной подсистемы внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="f22fd-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="f22fd-106">См. в разделе [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) список зависимости получателя.</span><span class="sxs-lookup"><span data-stu-id="f22fd-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="f22fd-107">Если зависимости не был зарегистрирован, используется реализация по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f22fd-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="f22fd-108">См. в разделе [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) список реализации по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f22fd-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
