---
title: Размещение и развертывание приложений Blazor на стороне сервера с помощью ASP.NET Core
author: guardrex
description: Узнайте, как выполнять размещение и развертывание приложения Blazor на стороне сервера с помощью ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/11/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 56a03ff583bf85497e2b3bacc70123845a046e3d
ms.sourcegitcommit: 040aedca220ed24ee1726e6886daf6906f95a028
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "67892699"
---
# <a name="host-and-deploy-blazor-server-side"></a>Размещение и развертывание Blazor на стороне сервера

Авторы: [Люк Лэтем](https://github.com/guardrex), [Рэйнер Стропек](https://www.timecockpit.com) и [Дэниэл Рот](https://github.com/danroth27)

## <a name="host-configuration-values"></a>Значения конфигурации узла

Серверные приложения, использующие [модель размещения на сервере](xref:blazor/hosting-models#server-side), могут принимать [значения конфигурации универсального узла](xref:fundamentals/host/generic-host#host-configuration).

## <a name="deployment"></a>Развертывание

При использовании [модели размещения на сервере](xref:blazor/hosting-models#server-side) Blazor работает на сервере в приложении ASP.NET Core. Обновление элементов пользовательского интерфейса, обработка событий и вызовы JavaScript обрабатываются через подключение [SignalR](xref:signalr/introduction).

Необходим веб-сервер, позволяющий размещать приложения ASP.NET Core. Visual Studio содержит шаблон проекта **Серверное приложение Blazor** (шаблон `blazorserverside` при использовании команды [dotnet new](/dotnet/core/tools/dotnet-new)).

## <a name="connection-scale-out"></a>Горизонтальное масштабирование подключений

Приложения Blazor, работающие на стороне сервера, требуют наличия одного активного подключения SignalR для каждого пользователя. В Рабочем развертывании Blazor на стороне сервера необходимо реализовать решение для поддержки нужного приложению числа одновременных подключений. [Служба Azure SignalR](/azure/azure-signalr/) управляет масштабированием подключений и является рекомендуемым решением для масштабирования приложений Blazor, работающих на стороне сервера. Дополнительные сведения можно найти по адресу: <xref:signalr/publish-to-azure-web-app>.

## <a name="signalr-configuration"></a>Конфигурация SignalR

Служба SignalR настраивается ASP.NET Core для самых распространенных сценариев приложений Blazor на стороне сервера. Подробные сведения о пользовательских и расширенных сценариях можно найти в статьях о SignalR в разделе [Дополнительные ресурсы](#additional-resources).

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:signalr/introduction>
* [Документация по службе Azure SignalR](/azure/azure-signalr/)
* [Краткое руководство. Создание чата с помощью службы SignalR](/azure/azure-signalr/signalr-quickstart-dotnet-core)
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [Развертывание предварительной версии ASP.NET Core в службе приложений Azure](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
