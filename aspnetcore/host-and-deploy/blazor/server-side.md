---
title: Размещение и развертывание Blazor на стороне сервера
author: guardrex
description: Узнайте, как выполнять размещение и развертывание приложения Blazor на стороне сервера с помощью ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 940020ee44d72d50395aad64bc924413c1bbecfb
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614756"
---
# <a name="host-and-deploy-blazor-server-side"></a>Размещение и развертывание Blazor на стороне сервера

Авторы: [Люк Лэтем](https://github.com/guardrex), [Рэйнер Стропек](https://www.timecockpit.com) и [Дэниэл Рот](https://github.com/danroth27)

## <a name="host-configuration-values"></a>Значения конфигурации узла

Серверные приложения, использующие [модель размещения на сервере](xref:blazor/hosting-models#server-side-hosting-model), могут принимать [значения конфигурации универсального узла](xref:fundamentals/host/generic-host#host-configuration).

## <a name="deployment"></a>Развертывание

При использовании [модели размещения на сервере](xref:blazor/hosting-models#server-side-hosting-model) Blazor работает на сервере в приложении ASP.NET Core. Обновление элементов пользовательского интерфейса, обработка событий и вызовы JavaScript обрабатываются через подключение [SignalR](xref:signalr/introduction).

Приложение входит в состав приложения ASP.NET Core в публикуемых выходных данных; следовательно, два приложения развертываются вместе. Веб-сервер, позволяющий размещать приложения ASP.NET Core, является обязательным. Для развертывания на стороне сервера Visual Studio включает шаблон проекта **Razor Components** (шаблон `razorcomponents` при использовании команды [dotnet new](/dotnet/core/tools/dotnet-new)).

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

Дополнительные сведения о размещении приложения ASP.NET Core и его развертывании см. в разделе <xref:host-and-deploy/index>.

Подробнее о развертывании в службе приложений Azure см. в <xref:tutorials/publish-to-azure-webapp-using-vs>.
