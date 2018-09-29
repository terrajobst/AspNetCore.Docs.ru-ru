---
title: Публикация ASP.NET Core SignalR приложение веб-приложении Azure
author: tdykstra
description: Публикация ASP.NET Core SignalR приложение веб-приложении Azure
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/20/2018
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: a6a0e44f5c67fefdac6bd26b3772c23e75f8bfc1
ms.sourcegitcommit: 32f5ee0690604d451f61e9a5c28881c9fcf85738
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2018
ms.locfileid: "47454730"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a><span data-ttu-id="bd816-103">Публикация ASP.NET Core SignalR приложения для веб-приложение Azure</span><span class="sxs-lookup"><span data-stu-id="bd816-103">Publish an ASP.NET Core SignalR app to an Azure Web App</span></span>

<span data-ttu-id="bd816-104">[Веб-приложение Azure](/azure/app-service/app-service-web-overview) — [Microsoft облачные вычисления](https://azure.microsoft.com/) службу платформы для размещения веб-приложений, включая ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bd816-104">[Azure Web App](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

> [!NOTE]
> <span data-ttu-id="bd816-105">Эта статья относится к публикации приложения ASP.NET Core SignalR из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bd816-105">This article refers to publishing an ASP.NET Core SignalR app from Visual Studio.</span></span> <span data-ttu-id="bd816-106">Посетите [SignalR службы для Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) Дополнительные сведения об использовании SignalR в Azure.</span><span class="sxs-lookup"><span data-stu-id="bd816-106">Visit [SignalR service for Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) for more information about using SignalR on Azure.</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="bd816-107">Публикация приложения</span><span class="sxs-lookup"><span data-stu-id="bd816-107">Publish the app</span></span>

<span data-ttu-id="bd816-108">Visual Studio предоставляет встроенные средства для публикации в веб-приложение Azure.</span><span class="sxs-lookup"><span data-stu-id="bd816-108">Visual Studio provides built-in tools for publishing to an Azure Web App.</span></span> <span data-ttu-id="bd816-109">Visual Studio Code пользователь может использовать [Azure CLI](/cli/azure) команды, чтобы публиковать приложения в Azure.</span><span class="sxs-lookup"><span data-stu-id="bd816-109">Visual Studio Code user can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="bd816-110">В этой статье рассматриваются публикации с помощью средств в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bd816-110">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="bd816-111">Чтобы опубликовать приложение с помощью Azure CLI, см. в разделе [публикации приложения ASP.NET Core в Azure с помощью средств командной строки](/azure/app-service/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="bd816-111">To publish an app using Azure CLI, see [Publish an ASP.NET Core app to Azure with command line tools](/azure/app-service/app-service-web-get-started-dotnet).</span></span>

<span data-ttu-id="bd816-112">Щелкните правой кнопкой мыши проект в **обозревателе решений** и выберите **публикации**.</span><span class="sxs-lookup"><span data-stu-id="bd816-112">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span> <span data-ttu-id="bd816-113">Убедитесь, что **создать** возвратом **выберите целевой объект публикации** диалоговое окно, а затем выберите **публикации**.</span><span class="sxs-lookup"><span data-stu-id="bd816-113">Confirm that **Create new** is checked in the **Pick a publish target** dialog, and select **Publish**.</span></span>

![Выбор целевого объекта публикации](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

<span data-ttu-id="bd816-115">Введите следующие сведения в **создать службу приложений** диалогового окна и выберите **создать**.</span><span class="sxs-lookup"><span data-stu-id="bd816-115">Enter the following information in the **Create App Service** dialog and select **Create**.</span></span>

| <span data-ttu-id="bd816-116">Элемент</span><span class="sxs-lookup"><span data-stu-id="bd816-116">Item</span></span> | <span data-ttu-id="bd816-117">Описание</span><span class="sxs-lookup"><span data-stu-id="bd816-117">Description</span></span> |
| ---- | ----------- |
| <span data-ttu-id="bd816-118">**Имя приложения**</span><span class="sxs-lookup"><span data-stu-id="bd816-118">**App name**</span></span> | <span data-ttu-id="bd816-119">Уникальное имя приложения.</span><span class="sxs-lookup"><span data-stu-id="bd816-119">A unique name of the app.</span></span> |
| <span data-ttu-id="bd816-120">**Подписка**</span><span class="sxs-lookup"><span data-stu-id="bd816-120">**Subscription**</span></span> | <span data-ttu-id="bd816-121">Подписка Azure, используемую приложением.</span><span class="sxs-lookup"><span data-stu-id="bd816-121">The Azure subscription that the app uses.</span></span> |
| <span data-ttu-id="bd816-122">**Группа ресурсов**</span><span class="sxs-lookup"><span data-stu-id="bd816-122">**Resource Group**</span></span> | <span data-ttu-id="bd816-123">Группа связанных ресурсов, к которым относится приложение.</span><span class="sxs-lookup"><span data-stu-id="bd816-123">The group of related resources to which the app belongs.</span></span>  |
| <span data-ttu-id="bd816-124">**План размещения**</span><span class="sxs-lookup"><span data-stu-id="bd816-124">**Hosting Plan**</span></span> | <span data-ttu-id="bd816-125">Тарифный план для веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="bd816-125">The pricing plan for the web app.</span></span> |

![Создание службы приложений](publish-to-azure-web-app/_static/create-app-service-dialog.png)

<span data-ttu-id="bd816-127">Visual Studio выполняет следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="bd816-127">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="bd816-128">Создает профиль публикации содержащий параметры публикации.</span><span class="sxs-lookup"><span data-stu-id="bd816-128">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="bd816-129">Создает или использует существующий *веб-приложения Azure* с помощью указанных сведений.</span><span class="sxs-lookup"><span data-stu-id="bd816-129">Creates or uses an existing *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="bd816-130">Публикует приложение.</span><span class="sxs-lookup"><span data-stu-id="bd816-130">Publishes the app.</span></span>
* <span data-ttu-id="bd816-131">Запускает браузер, с помощью опубликованного веб-приложения, которые загружены.</span><span class="sxs-lookup"><span data-stu-id="bd816-131">Launches a browser, with the published web app loaded.</span></span>

<span data-ttu-id="bd816-132">Обратите внимание на формат URL-адрес для приложения — *{имя_приложения} .azurewebsites .net*.</span><span class="sxs-lookup"><span data-stu-id="bd816-132">Notice the format of the URL for the app is *{app name}.azurewebsites.net*.</span></span> <span data-ttu-id="bd816-133">Например, приложение с именем `SignalRChattR` имеет URL-адрес, который выглядит как `https://signalrchattr.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="bd816-133">For example, an app named `SignalRChattR` has a URL that looks like `https://signalrchattr.azurewebsites.net`.</span></span>

<span data-ttu-id="bd816-134">Если возникает ошибка HTTP 502.2, см. в разделе [предварительной версии развертывание ASP.NET Core в службе приложений Azure](xref:host-and-deploy/azure-apps/index) ее устранения.</span><span class="sxs-lookup"><span data-stu-id="bd816-134">If an HTTP 502.2 error occurs, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index) to resolve it.</span></span>

## <a name="configure-signalr-web-app"></a><span data-ttu-id="bd816-135">Настройка веб-приложения SignalR</span><span class="sxs-lookup"><span data-stu-id="bd816-135">Configure SignalR web app</span></span>

<span data-ttu-id="bd816-136">Приложения ASP.NET Core SignalR, которые опубликованы как веб-приложение Azure необходим [сходства ARR](https://en.wikipedia.org/wiki/Application_Request_Routing) включена.</span><span class="sxs-lookup"><span data-stu-id="bd816-136">ASP.NET Core SignalR apps that are published as an Azure Web App must have [ARR Affinity](https://en.wikipedia.org/wiki/Application_Request_Routing) enabled.</span></span> <span data-ttu-id="bd816-137">[WebSockets](xref:fundamentals/websockets) должна быть включена, чтобы разрешить WebSockets транспорт для функции.</span><span class="sxs-lookup"><span data-stu-id="bd816-137">[WebSockets](xref:fundamentals/websockets) should be enabled, to allow the WebSockets transport to function.</span></span>

<span data-ttu-id="bd816-138">На портале Azure перейдите к **параметры приложения** для веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="bd816-138">In the Azure portal, navigate to **App Settings** for your web app.</span></span> <span data-ttu-id="bd816-139">Задайте **WebSockets** для **на**и проверьте **ARR сходства** является **на**.</span><span class="sxs-lookup"><span data-stu-id="bd816-139">Set **WebSockets** to **On**, and verify **ARR Affinity** is **On**.</span></span>

![Параметры в Azure веб-приложения на портале Azure](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 <span data-ttu-id="bd816-141">WebSockets и других транспортов [ограничены зависят от того, план службы приложений](/azure/azure-subscription-service-limits#app-service-limits).</span><span class="sxs-lookup"><span data-stu-id="bd816-141">WebSockets and other transports [are limited based on the App Service Plan](/azure/azure-subscription-service-limits#app-service-limits).</span></span>

## <a name="related-resources"></a><span data-ttu-id="bd816-142">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="bd816-142">Related resources</span></span>

* [<span data-ttu-id="bd816-143">Публикация приложения ASP.NET Core в Azure с помощью средств командной строки</span><span class="sxs-lookup"><span data-stu-id="bd816-143">Publish an ASP.NET Core app to Azure with command line tools</span></span>](/azure/app-service/app-service-web-get-started-dotnet)
* [<span data-ttu-id="bd816-144">Публикация приложения ASP.NET Core в Azure с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bd816-144">Publish an ASP.NET Core app to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)
* [<span data-ttu-id="bd816-145">Размещение и развертывание приложений ASP.NET Core Preview в Azure</span><span class="sxs-lookup"><span data-stu-id="bd816-145">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
