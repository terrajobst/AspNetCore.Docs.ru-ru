---
uid: webhooks/receiving/dependencies
title: Зависимости получателя ASP.NET веб-перехватчиков | Документация Майкрософт
author: rick-anderson
description: Зависимости получателя и внедрение зависимостей в ASP.NET веб-перехватчики.
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: 05dcfac121e7974fd83c5b3736616479574944a1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817841"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="a752a-103">Зависимости получателя ASP.NET веб-перехватчиков</span><span class="sxs-lookup"><span data-stu-id="a752a-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="a752a-104">Веб-перехватчики Microsoft ASP.NET разработан с помощью внедрения зависимостей в виду.</span><span class="sxs-lookup"><span data-stu-id="a752a-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="a752a-105">Большинство зависимостей в системе могут быть заменены альтернативных реализаций, с помощью надежной подсистемы внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="a752a-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="a752a-106">См. в разделе [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) список зависимости получателя.</span><span class="sxs-lookup"><span data-stu-id="a752a-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="a752a-107">Если зависимости не был зарегистрирован, используется реализация по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a752a-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="a752a-108">См. в разделе [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) список реализации по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a752a-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
