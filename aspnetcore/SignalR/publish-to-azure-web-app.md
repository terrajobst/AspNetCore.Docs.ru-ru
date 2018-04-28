---
title: Публикация ASP.NET Core приложений SignalR на веб-приложение Azure
author: rachelappel
description: Публикация ASP.NET Core приложений SignalR на веб-приложение Azure
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/20/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: fd7d38ad47d9004db2ae7b5858dc22609943f601
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/28/2018
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a><span data-ttu-id="569b5-103">Публикация ASP.NET Core приложений SignalR на веб-приложение Azure</span><span class="sxs-lookup"><span data-stu-id="569b5-103">Publish an ASP.NET Core SignalR app to an Azure Web App</span></span>

<span data-ttu-id="569b5-104">[Azure веб-приложения](/azure/app-service/app-service-web-overview) — [Microsoft облачные вычисления](https://azure.microsoft.com/) платформы службы для размещения веб-приложений, включая ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="569b5-104">[Azure Web App](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="569b5-105">Публикация приложения</span><span class="sxs-lookup"><span data-stu-id="569b5-105">Publish the app</span></span>

<span data-ttu-id="569b5-106">Visual Studio предоставляет встроенные средства для публикации для веб-приложение Azure.</span><span class="sxs-lookup"><span data-stu-id="569b5-106">Visual Studio provides built-in tools for publishing to an Azure Web App.</span></span> <span data-ttu-id="569b5-107">Можно использовать Visual Studio код пользователя [Azure CLI](/cli/azure) команд для публикации приложения в Azure.</span><span class="sxs-lookup"><span data-stu-id="569b5-107">Visual Studio Code user can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="569b5-108">В этой статье рассказывается о публикации с использованием средства в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="569b5-108">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="569b5-109">Чтобы опубликовать приложение с помощью Azure CLI, в разделе [публикации приложения ASP.NET Core в Azure с помощью средства командной строки](xref:tutorials/publish-to-azure-webapp-using-cli).</span><span class="sxs-lookup"><span data-stu-id="569b5-109">To publish an app using Azure CLI, see [Publish an ASP.NET Core app to Azure with command line tools](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

<span data-ttu-id="569b5-110">Правой кнопкой мыши проект в **обозревателе решений** и выберите **публикации**.</span><span class="sxs-lookup"><span data-stu-id="569b5-110">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span> <span data-ttu-id="569b5-111">Убедитесь, что **создать новый** возврата **выбрать место назначения публикации** диалогового окна, а затем выберите **публикации**.</span><span class="sxs-lookup"><span data-stu-id="569b5-111">Confirm that **Create new** is checked in the **Pick a publish target** dialog, and select **Publish**.</span></span>

![Подбор публикации целевой](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

<span data-ttu-id="569b5-113">Введите следующие сведения в **Создание приложения службы** диалоговое окно и выбрать **создать**.</span><span class="sxs-lookup"><span data-stu-id="569b5-113">Enter the following information in the **Create App Service** dialog and select **Create**.</span></span>

| <span data-ttu-id="569b5-114">Элемент</span><span class="sxs-lookup"><span data-stu-id="569b5-114">Item</span></span> | <span data-ttu-id="569b5-115">Описание</span><span class="sxs-lookup"><span data-stu-id="569b5-115">Description</span></span> |
| ---- | ----------- |
| <span data-ttu-id="569b5-116">**Имя приложения**</span><span class="sxs-lookup"><span data-stu-id="569b5-116">**App name**</span></span> | <span data-ttu-id="569b5-117">Уникальное имя приложения.</span><span class="sxs-lookup"><span data-stu-id="569b5-117">A unique name of the app.</span></span> |
| <span data-ttu-id="569b5-118">**Подписки**</span><span class="sxs-lookup"><span data-stu-id="569b5-118">**Subscription**</span></span> | <span data-ttu-id="569b5-119">Подписка Azure, который использует приложение.</span><span class="sxs-lookup"><span data-stu-id="569b5-119">The Azure subscription that the app uses.</span></span> |
| <span data-ttu-id="569b5-120">**Группа ресурсов**</span><span class="sxs-lookup"><span data-stu-id="569b5-120">**Resource Group**</span></span> | <span data-ttu-id="569b5-121">Группа связанных ресурсов, к которым принадлежит приложения.</span><span class="sxs-lookup"><span data-stu-id="569b5-121">The group of related resources to which the app belongs.</span></span>  |
| <span data-ttu-id="569b5-122">**План размещения**</span><span class="sxs-lookup"><span data-stu-id="569b5-122">**Hosting Plan**</span></span> | <span data-ttu-id="569b5-123">Ценовой план для веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="569b5-123">The pricing plan for the web app.</span></span> |

![Создание приложения службы](publish-to-azure-web-app/_static/create-app-service-dialog.png)

<span data-ttu-id="569b5-125">Visual Studio выполняет следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="569b5-125">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="569b5-126">Создает профиль публикации содержащего параметры публикации.</span><span class="sxs-lookup"><span data-stu-id="569b5-126">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="569b5-127">Создает или использует существующий *веб-приложения Azure* с помощью указанных сведений.</span><span class="sxs-lookup"><span data-stu-id="569b5-127">Creates or uses an existing *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="569b5-128">Публикация приложения.</span><span class="sxs-lookup"><span data-stu-id="569b5-128">Publishes the app.</span></span>
* <span data-ttu-id="569b5-129">Запускает браузер с загружен опубликованных веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="569b5-129">Launches a browser, with the published web app loaded.</span></span>

<span data-ttu-id="569b5-130">Обратите внимание на формат URL-адреса для приложения является *.azurewebsites {имя приложения} .net*.</span><span class="sxs-lookup"><span data-stu-id="569b5-130">Notice the format of the URL for the app is *{app name}.azurewebsites.net*.</span></span> <span data-ttu-id="569b5-131">Например, приложение с именем `SignalRChattR` URL-адрес, который выглядит как `https://signalrchattr.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="569b5-131">For example, an app named `SignalRChattR` has a URL that looks like `https://signalrchattr.azurewebsites.net`.</span></span>

<span data-ttu-id="569b5-132">См. в случае ошибки HTTP 502.2 [развертывание ASP.NET Core предварительного выпуска в службу приложений Azure](xref:host-and-deploy/azure-apps/index) ее устранения.</span><span class="sxs-lookup"><span data-stu-id="569b5-132">If an HTTP 502.2 error occurs, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index) to resolve it.</span></span>

## <a name="configure-signalr-web-app"></a><span data-ttu-id="569b5-133">Настройка SignalR веб-приложения</span><span class="sxs-lookup"><span data-stu-id="569b5-133">Configure SignalR web app</span></span>

<span data-ttu-id="569b5-134">Приложения ASP.NET Core SignalR, которые опубликованы как веб-приложение Azure должно быть [сходства ARR](https://en.wikipedia.org/wiki/Application_Request_Routing) включена.</span><span class="sxs-lookup"><span data-stu-id="569b5-134">ASP.NET Core SignalR apps that are published as an Azure Web App must have [ARR Affinity](https://en.wikipedia.org/wiki/Application_Request_Routing) enabled.</span></span> <span data-ttu-id="569b5-135">[WebSockets](xref:fundamentals/websockets) для должна быть включена, разрешить передачи WebSockets функции.</span><span class="sxs-lookup"><span data-stu-id="569b5-135">[WebSockets](xref:fundamentals/websockets) should be enabled, to allow the WebSockets transport to function.</span></span>

<span data-ttu-id="569b5-136">На портале Azure перейдите к **параметры приложения** для веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="569b5-136">In the Azure portal, navigate to **App Settings** for your web app.</span></span> <span data-ttu-id="569b5-137">Задать **WebSockets** для **на**и проверьте **сходства ARR** — **на**.</span><span class="sxs-lookup"><span data-stu-id="569b5-137">Set **WebSockets** to **On**, and verify **ARR Affinity** is **On**.</span></span>

![Параметры Azure веб-приложения на портале Azure](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 <span data-ttu-id="569b5-139">WebSockets и других транспортов [ограничены зависят от того, план служб приложений](/azure/azure-subscription-service-limits#app-service-limits).</span><span class="sxs-lookup"><span data-stu-id="569b5-139">WebSockets and other transports [are limited based on the App Service Plan](/azure/azure-subscription-service-limits#app-service-limits).</span></span>

## <a name="related-resources"></a><span data-ttu-id="569b5-140">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="569b5-140">Related resources</span></span>

* [<span data-ttu-id="569b5-141">Публикация приложения ASP.NET Core в Azure с помощью средств командной строки</span><span class="sxs-lookup"><span data-stu-id="569b5-141">Publish an ASP.NET Core app to Azure with command line tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli?tabs=windows)
* [<span data-ttu-id="569b5-142">Публикация приложения ASP.NET Core в Azure с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="569b5-142">Publish an ASP.NET Core app to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)
* [<span data-ttu-id="569b5-143">Размещать и развертывать приложения ASP.NET Core Preview в Azure</span><span class="sxs-lookup"><span data-stu-id="569b5-143">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
