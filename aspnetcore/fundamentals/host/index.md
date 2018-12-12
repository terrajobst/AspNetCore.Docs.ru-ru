---
title: Веб-узел и универсальный узел в ASP.NET Core
author: guardrex
description: Сведения о веб-узле ASP.NET Core и универсальном узле .NET, которые отвечают за запуск приложений и управление временем существования.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 08/28/2018
uid: fundamentals/host/index
ms.openlocfilehash: 43a68f67368ce37a1fb4032579c6c4e4c05d2719
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284582"
---
# <a name="web-host-and-generic-host-in-aspnet-core"></a>Веб-узел и универсальный узел в ASP.NET Core

Приложения .NET настраивают и запускают *узел*. Узел отвечает за запуск приложения и управление временем существования. Для использования доступны два API узла:

* [Веб-узел](xref:fundamentals/host/web-host) &ndash; подходит для размещения веб-приложений.
* [Универсальный узел](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 или более поздней версии) &ndash; подходит для размещения других приложений (например, приложений, которые выполняют фоновые задачи). В следующем выпуске универсальный узел позволит размещать любые приложения, включая веб-приложения. Со временем универсальный узел заменит веб-узел.

Для размещения *веб-приложений* ASP.NET Core разработчикам следует использовать веб-узел на основе <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>. Если это *не веб-приложения*, для их размещения разработчикам следует использовать универсальный узел на основе <xref:Microsoft.Extensions.Hosting.HostBuilder>.

<xref:fundamentals/host/hosted-services>  
Узнайте, как реализовать фоновые задачи с размещенными службами в ASP.NET Core.

<xref:fundamentals/configuration/platform-specific-configuration>  
Узнайте, как расширить приложение ASP.NET Core при помощи сборки, заданной по ссылке или без ссылок, используя реализацию <xref:Microsoft.AspNetCore.Hosting.IHostingStartup>.
