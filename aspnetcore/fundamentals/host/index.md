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
ms.openlocfilehash: 7f8ccff7e3da93d6e617505ac93fafc3a82ed880
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252013"
---
# <a name="host-in-aspnet-core"></a>Размещение в ASP.NET Core

Приложения .NET настраивают и запускают *узел*. Узел отвечает за запуск приложения и управление временем существования. Для использования доступны два API узла:

* [Веб-узел](xref:fundamentals/host/web-host) &ndash; подходит для размещения веб-приложений.
* [Универсальный узел](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 или более поздней версии) &ndash; подходит для размещения других приложений (например, приложений, которые выполняют фоновые задачи). В следующем выпуске универсальный узел позволит размещать любые приложения, включая веб-приложения. Со временем универсальный узел заменит веб-узел.

Для размещения *веб-приложений* ASP.NET Core разработчикам следует использовать веб-узел на основе [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder). Если это *не веб-приложение*, для его размещения разработчикам следует использовать универсальный узел на основе [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).
