---
uid: webhooks/receiving/dependencies
title: Веб-перехватчиков ASP.NET приемника зависимостей | Документы Microsoft
author: rick-anderson
description: Приемник зависимости и внедрение зависимостей в ASP.NET веб-привязок.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: f9726c746c8934594e26f2871f9b867c192374bb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
ms.locfileid: "26529913"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="a9fe9-103">Веб-перехватчиков ASP.NET приемника зависимостей</span><span class="sxs-lookup"><span data-stu-id="a9fe9-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="a9fe9-104">Microsoft ASP.NET веб-перехватчиков разработан с внедрение зависимостей в виду.</span><span class="sxs-lookup"><span data-stu-id="a9fe9-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="a9fe9-105">Большинство зависимостей в системе можно заменить альтернативных реализаций, использующих механизм внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="a9fe9-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="a9fe9-106">См. в разделе [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) список зависимостей получателя.</span><span class="sxs-lookup"><span data-stu-id="a9fe9-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="a9fe9-107">Если зависимости не был зарегистрирован, используется реализация по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a9fe9-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="a9fe9-108">См. в разделе [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) список реализации по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a9fe9-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
