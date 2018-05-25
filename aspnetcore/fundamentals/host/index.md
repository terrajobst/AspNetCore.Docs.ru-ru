---
title: Размещение в ASP.NET Core
author: guardrex
description: Сведения о веб-узле ASP.NET Core и универсальном узле .NET, которые отвечают за запуск приложений и управление временем существования.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/index
ms.openlocfilehash: 7ad059e39866f59040c12b7ac15e9fa3405a9aad
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/17/2018
---
# <a name="host-in-aspnet-core"></a>Размещение в ASP.NET Core

Приложения .NET настраивают и запускают *узел*. Узел отвечает за запуск приложения и управление временем существования. Для использования доступны два API узла:

* [Веб-узел](xref:fundamentals/host/web-host) &ndash; подходит для размещения веб-приложений.
* [Универсальный узел](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 или более поздней версии) &ndash; подходит для размещения других приложений (например, приложений, которые выполняют фоновые задачи). В следующем выпуске универсальный узел позволит размещать любые приложения, включая веб-приложения. Со временем универсальный узел заменит веб-узел.

Сейчас разработчикам следует использовать [веб-узел](xref:fundamentals/host/web-host) на основе [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) для размещения приложений ASP.NET Core.
