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
# <a name="aspnet-webhooks-receiver-dependencies"></a>Зависимости получателя ASP.NET веб-перехватчиков

Веб-перехватчики Microsoft ASP.NET разработан с помощью внедрения зависимостей в виду. Большинство зависимостей в системе могут быть заменены альтернативных реализаций, с помощью надежной подсистемы внедрения зависимостей.

См. в разделе [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) список зависимости получателя. Если зависимости не был зарегистрирован, используется реализация по умолчанию. См. в разделе [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) список реализации по умолчанию.
