---
title: "Размещение ASP.NET Core в контейнерах Docker"
author: rick-anderson
description: "Ссылки на ресурсы для изучения способов размещения приложений ASP.NET Core в контейнерах Docker."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/08/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/docker/index
ms.openlocfilehash: f6646a92e75b79d2193e9cbca7fa8ac8e26dc429
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="host-aspnet-core-in-docker-containers"></a><span data-ttu-id="4845d-103">Размещение ASP.NET Core в контейнерах Docker</span><span class="sxs-lookup"><span data-stu-id="4845d-103">Host ASP.NET Core in Docker containers</span></span>

<span data-ttu-id="4845d-104">В следующих статьях содержатся сведения о размещении приложений ASP.NET Core в Docker.</span><span class="sxs-lookup"><span data-stu-id="4845d-104">The following articles are available for learning about hosting ASP.NET Core apps in Docker:</span></span>

[<span data-ttu-id="4845d-105">Общие сведения о контейнерах и Docker</span><span class="sxs-lookup"><span data-stu-id="4845d-105">Introduction to Containers and Docker</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/index)  
<span data-ttu-id="4845d-106">Узнайте о том, что контейнеризация — это подход к разработке программного обеспечения, при котором приложение или служба, их зависимости и конфигурация упаковываются вместе в образ контейнера.</span><span class="sxs-lookup"><span data-stu-id="4845d-106">See how containerization is an approach to software development in which an application or service, its dependencies, and its configuration are packaged together as a container image.</span></span> <span data-ttu-id="4845d-107">Образ можно протестировать и затем развернуть на узле.</span><span class="sxs-lookup"><span data-stu-id="4845d-107">The image can be tested and then deployed to a host.</span></span>

[<span data-ttu-id="4845d-108">Что такое Docker?</span><span class="sxs-lookup"><span data-stu-id="4845d-108">What is Docker</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-defined)  
<span data-ttu-id="4845d-109">Узнайте, о том, что Docker — это проект с открытым исходным кодом для автоматизации развертывания приложений в виде переносимых автономных контейнеров, выполняемых в облаке или локальной среде.</span><span class="sxs-lookup"><span data-stu-id="4845d-109">Discover how Docker is an open-source project for automating the deployment of apps as portable, self-sufficient containers that can run on the cloud or on-premises.</span></span>

[<span data-ttu-id="4845d-110">Терминология Docker</span><span class="sxs-lookup"><span data-stu-id="4845d-110">Docker Terminology</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-terminology)  
<span data-ttu-id="4845d-111">Изучите термины и определения для технологии Docker.</span><span class="sxs-lookup"><span data-stu-id="4845d-111">Learn terms and definitions for Docker technology.</span></span>

[<span data-ttu-id="4845d-112">Контейнеры, образы и реестры Docker</span><span class="sxs-lookup"><span data-stu-id="4845d-112">Docker containers, images, and registries</span></span>](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-containers-images-registries)  
<span data-ttu-id="4845d-113">Узнайте о хранении образов контейнеров Docker в реестре образов для согласованного развертывания в средах.</span><span class="sxs-lookup"><span data-stu-id="4845d-113">Find out how Docker container images are stored in an image registry for consistent deployment across environments.</span></span>

[<span data-ttu-id="4845d-114">Создание образов Docker для приложений .NET Core</span><span class="sxs-lookup"><span data-stu-id="4845d-114">Building Docker Images for .NET Core Applications</span></span>](/dotnet/articles/core/docker/building-net-docker-images)  
<span data-ttu-id="4845d-115">Узнайте, как создавать и добавлять в Docker приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4845d-115">Learn how to build and dockerize an ASP.NET Core app.</span></span> <span data-ttu-id="4845d-116">Изучите образы Docker, поддерживаемые корпорацией Майкрософт, и ознакомьтесь с вариантами использования.</span><span class="sxs-lookup"><span data-stu-id="4845d-116">Explore Docker images maintained by Microsoft and examine use cases.</span></span>

[<span data-ttu-id="4845d-117">Инструменты Visual Studio для Docker</span><span class="sxs-lookup"><span data-stu-id="4845d-117">Visual Studio Tools for Docker</span></span>](xref:host-and-deploy/docker/visual-studio-tools-for-docker)  
<span data-ttu-id="4845d-118">Узнайте, как Visual Studio 2017 поддерживает создание, отладку и запуск приложений ASP.NET Core, предназначенных для .NET Framework или .NET Core, в Docker для Windows.</span><span class="sxs-lookup"><span data-stu-id="4845d-118">Discover how Visual Studio 2017 supports building, debugging, and running ASP.NET Core apps targeting either .NET Framework or .NET Core on Docker for Windows.</span></span> <span data-ttu-id="4845d-119">Поддерживаются контейнеры Windows и Linux.</span><span class="sxs-lookup"><span data-stu-id="4845d-119">Both Windows and Linux containers are supported.</span></span>

[<span data-ttu-id="4845d-120">Публикация в образ Docker</span><span class="sxs-lookup"><span data-stu-id="4845d-120">Publish to a Docker Image</span></span>](/azure/vs-azure-tools-docker-hosting-web-apps-in-docker)  
<span data-ttu-id="4845d-121">Узнайте, как использовать расширение Visual Studio Tools for Docker для развертывания приложения ASP.NET Core на узле Docker в Azure с помощью PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4845d-121">Find out how to use the Visual Studio Tools for Docker extension to deploy an ASP.NET Core app to a Docker host on Azure using PowerShell.</span></span>
