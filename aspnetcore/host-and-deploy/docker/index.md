---
title: Размещение ASP.NET Core в контейнерах Docker
author: rick-anderson
description: Ссылки на ресурсы для изучения способов размещения приложений ASP.NET Core в контейнерах Docker.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/08/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/docker/index
ms.openlocfilehash: 12a179287ec302994380e0faf4b843596f8c2f4e
ms.sourcegitcommit: d45d766504c2c5aad2453f01f089bc6b696b5576
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2018
ms.locfileid: "30280099"
---
# <a name="host-aspnet-core-in-docker-containers"></a>Размещение ASP.NET Core в контейнерах Docker

В следующих статьях содержатся сведения о размещении приложений ASP.NET Core в Docker.

[Общие сведения о контейнерах и Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/index)  
Узнайте о том, что контейнеризация — это подход к разработке программного обеспечения, при котором приложение или служба, их зависимости и конфигурация упаковываются вместе в образ контейнера. Образ можно протестировать и затем развернуть на узле.

[Что такое Docker?](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-defined)  
Узнайте, о том, что Docker — это проект с открытым исходным кодом для автоматизации развертывания приложений в виде переносимых автономных контейнеров, выполняемых в облаке или локальной среде.

[Терминология Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-terminology)  
Изучите термины и определения для технологии Docker.

[Контейнеры, образы и реестры Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-containers-images-registries)  
Узнайте о хранении образов контейнеров Docker в реестре образов для согласованного развертывания в средах.

[Создание образов Docker для приложений .NET Core](/dotnet/articles/core/docker/building-net-docker-images)  
Узнайте, как создавать и добавлять в Docker приложение ASP.NET Core. Изучите образы Docker, поддерживаемые корпорацией Майкрософт, и ознакомьтесь с вариантами использования.

[Инструменты Visual Studio для Docker](xref:host-and-deploy/docker/visual-studio-tools-for-docker)  
Узнайте, как Visual Studio 2017 поддерживает создание, отладку и запуск приложений ASP.NET Core, предназначенных для .NET Framework или .NET Core, в Docker для Windows. Поддерживаются контейнеры Windows и Linux.

[Публикация в образ Docker](/azure/vs-azure-tools-docker-hosting-web-apps-in-docker)  
Узнайте, как использовать расширение Visual Studio Tools for Docker для развертывания приложения ASP.NET Core на узле Docker в Azure с помощью PowerShell.

[Настройка ASP.NET Core для работы с прокси-серверами и подсистемами балансировки нагрузки](xref:host-and-deploy/proxy-load-balancer)  
Для приложений, размещенных за прокси-серверами и подсистемами балансировки нагрузки, может потребоваться дополнительная настройка. При передаче запросов через прокси-сервер сведения об исходном запросе, например схема и IP-адрес клиента, часто бывают скрыты. Иногда необходимо вручную переслать некоторые сведения о запросе в приложение.
