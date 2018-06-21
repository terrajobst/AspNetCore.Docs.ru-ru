---
title: Размещение в ASP.NET Core
author: guardrex
description: Сведения о веб-узле ASP.NET Core и универсальном узле .NET, которые отвечают за запуск приложений и управление временем существования.
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/index
ms.openlocfilehash: 365c679e789c07818c6eb007f40f6aef43b82c44
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276621"
---
# <a name="host-in-aspnet-core"></a>Размещение в ASP.NET Core

Приложения .NET настраивают и запускают *узел*. Узел отвечает за запуск приложения и управление временем существования. Для использования доступны два API узла:

* [Веб-узел](xref:fundamentals/host/web-host) &ndash; подходит для размещения веб-приложений.
* [Универсальный узел](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 или более поздней версии) &ndash; подходит для размещения других приложений (например, приложений, которые выполняют фоновые задачи). В следующем выпуске универсальный узел позволит размещать любые приложения, включая веб-приложения. Со временем универсальный узел заменит веб-узел.

Для размещения *веб-приложений* ASP.NET Core разработчикам следует использовать веб-узел на основе [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder). Если это *не веб-приложение*, для его размещения разработчикам следует использовать универсальный узел на основе [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).
