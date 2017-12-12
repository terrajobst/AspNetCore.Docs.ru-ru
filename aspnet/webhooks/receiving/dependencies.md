---
uid: webhooks/receiving/dependencies
title: "Веб-перехватчиков ASP.NET приемника зависимостей | Документы Microsoft"
author: rick-anderson
description: "Приемник зависимости и внедрение зависимостей в ASP.NET веб-привязок."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: f9726c746c8934594e26f2871f9b867c192374bb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-receiver-dependencies"></a>Веб-перехватчиков ASP.NET приемника зависимостей

Microsoft ASP.NET веб-перехватчиков разработан с внедрение зависимостей в виду. Большинство зависимостей в системе можно заменить альтернативных реализаций, использующих механизм внедрения зависимостей.

См. в разделе [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) список зависимостей получателя. Если зависимости не был зарегистрирован, используется реализация по умолчанию. См. в разделе [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) список реализации по умолчанию.
