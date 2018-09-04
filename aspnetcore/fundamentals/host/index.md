---
title: Размещение в ASP.NET Core
author: guardrex
description: Сведения о веб-узле ASP.NET Core и универсальном узле .NET, которые отвечают за запуск приложений и управление временем существования.
ms.author: riande
ms.custom: mvc
ms.date: 08/28/2018
uid: fundamentals/host/index
ms.openlocfilehash: 9927722b5080beb94e5628d9e7b54e6d50a5bff8
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336054"
---
# <a name="host-in-aspnet-core"></a>Размещение в ASP.NET Core

Приложения .NET настраивают и запускают *узел*. Узел отвечает за запуск приложения и управление временем существования. Для использования доступны два API узла:

* [Веб-узел](xref:fundamentals/host/web-host) &ndash; подходит для размещения веб-приложений.
* [Универсальный узел](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 или более поздней версии) &ndash; подходит для размещения других приложений (например, приложений, которые выполняют фоновые задачи). В следующем выпуске универсальный узел позволит размещать любые приложения, включая веб-приложения. Со временем универсальный узел заменит веб-узел.

Для размещения *веб-приложений* ASP.NET Core разработчикам следует использовать веб-узел на основе <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>. Если это *не веб-приложения*, для их размещения разработчикам следует использовать универсальный узел на основе <xref:Microsoft.Extensions.Hosting.HostBuilder>.

<xref:fundamentals/host/hosted-services>  
Узнайте, как реализовать фоновые задачи с размещенными службами в ASP.NET Core.

<xref:fundamentals/configuration/platform-specific-configuration>  
Узнайте, как расширить приложение ASP.NET Core при помощи сборки, заданной по ссылке или без ссылок, используя реализацию <xref:Microsoft.AspNetCore.Hosting.IHostingStartup>.
