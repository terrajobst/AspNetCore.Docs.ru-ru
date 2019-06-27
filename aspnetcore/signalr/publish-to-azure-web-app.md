---
title: Публикация ASP.NET Core SignalR приложения в службе приложений Azure
author: bradygaster
description: Узнайте, как опубликовать приложение ASP.NET Core SignalR в службе приложений Azure.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/26/2019
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 87a9c93add373b24e3c473912cdbfcc00bbebf7e
ms.sourcegitcommit: 9bb29f9ba6f0645ee8b9cabda07e3a5aa52cd659
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/26/2019
ms.locfileid: "67406102"
---
# <a name="publish-an-aspnet-core-signalr-app-to-azure-app-service"></a><span data-ttu-id="2bee1-103">Публикация ASP.NET Core SignalR приложения в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="2bee1-103">Publish an ASP.NET Core SignalR app to Azure App Service</span></span>

<span data-ttu-id="2bee1-104">По [Brady Gaster](https://twitter.com/bradygaster)</span><span class="sxs-lookup"><span data-stu-id="2bee1-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="2bee1-105">[Служба приложений Azure](/azure/app-service/app-service-web-overview) — [Microsoft облачные вычисления](https://azure.microsoft.com/) службу платформы для размещения веб-приложений, включая ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2bee1-105">[Azure App Service](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

> [!NOTE]
> <span data-ttu-id="2bee1-106">Эта статья относится к публикации приложения ASP.NET Core SignalR из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2bee1-106">This article refers to publishing an ASP.NET Core SignalR app from Visual Studio.</span></span> <span data-ttu-id="2bee1-107">Дополнительные сведения см. в разделе [SignalR службы для Azure](https://azure.microsoft.com/services/signalr-service).</span><span class="sxs-lookup"><span data-stu-id="2bee1-107">For more information, see [SignalR service for Azure](https://azure.microsoft.com/services/signalr-service).</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="2bee1-108">Публикация приложения</span><span class="sxs-lookup"><span data-stu-id="2bee1-108">Publish the app</span></span>

<span data-ttu-id="2bee1-109">В этой статье рассматриваются публикации с помощью средств в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2bee1-109">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="2bee1-110">Пользователи Visual Studio Code можно использовать [Azure CLI](/cli/azure) команды, чтобы публиковать приложения в Azure.</span><span class="sxs-lookup"><span data-stu-id="2bee1-110">Visual Studio Code users can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="2bee1-111">Дополнительные сведения см. в разделе [публикации приложения ASP.NET Core в Azure с помощью средств командной строки](/azure/app-service/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="2bee1-111">For more information, see [Publish an ASP.NET Core app to Azure with command line tools](/azure/app-service/app-service-web-get-started-dotnet).</span></span>

1. <span data-ttu-id="2bee1-112">В **обозревателе решений** щелкните правой кнопкой мыши проект и выберите **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="2bee1-112">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span>

1. <span data-ttu-id="2bee1-113">Убедитесь, что **службы приложений** и **создать** выбраны в **выберите целевой объект публикации** диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="2bee1-113">Confirm that **App Service** and **Create new** are selected in the **Pick a publish target** dialog.</span></span>

1. <span data-ttu-id="2bee1-114">Выберите **создать профиль** из **публикации** кнопку раскрывающегося вниз.</span><span class="sxs-lookup"><span data-stu-id="2bee1-114">Select **Create Profile** from the **Publish** button drop down.</span></span>

   <span data-ttu-id="2bee1-115">Введите сведения, описанные в следующей таблице в **создать службу приложений** диалогового окна и выберите **создать**.</span><span class="sxs-lookup"><span data-stu-id="2bee1-115">Enter the information described in the following table in the **Create App Service** dialog and select **Create**.</span></span>

   | <span data-ttu-id="2bee1-116">Элемент</span><span class="sxs-lookup"><span data-stu-id="2bee1-116">Item</span></span>               | <span data-ttu-id="2bee1-117">Описание</span><span class="sxs-lookup"><span data-stu-id="2bee1-117">Description</span></span> |
   | ------------------ | ----------- |
   | <span data-ttu-id="2bee1-118">**Name**</span><span class="sxs-lookup"><span data-stu-id="2bee1-118">**Name**</span></span>           | <span data-ttu-id="2bee1-119">Уникальное имя приложения.</span><span class="sxs-lookup"><span data-stu-id="2bee1-119">Unique name of the app.</span></span> |
   | <span data-ttu-id="2bee1-120">**Подписка**</span><span class="sxs-lookup"><span data-stu-id="2bee1-120">**Subscription**</span></span>   | <span data-ttu-id="2bee1-121">Подписка Azure, используемую приложением.</span><span class="sxs-lookup"><span data-stu-id="2bee1-121">Azure subscription that the app uses.</span></span> |
   | <span data-ttu-id="2bee1-122">**Группа ресурсов**</span><span class="sxs-lookup"><span data-stu-id="2bee1-122">**Resource Group**</span></span> | <span data-ttu-id="2bee1-123">Группа связанных ресурсов, к которым относится приложение.</span><span class="sxs-lookup"><span data-stu-id="2bee1-123">Group of related resources to which the app belongs.</span></span> |
   | <span data-ttu-id="2bee1-124">**План размещения**</span><span class="sxs-lookup"><span data-stu-id="2bee1-124">**Hosting Plan**</span></span>   | <span data-ttu-id="2bee1-125">Тарифный план для веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="2bee1-125">Pricing plan for the web app.</span></span> |

1. <span data-ttu-id="2bee1-126">Выберите **служба Azure SignalR** в **зависимости** > **добавить** стрелку раскрывающегося списка:</span><span class="sxs-lookup"><span data-stu-id="2bee1-126">Select the **Azure SignalR Service** in the **Dependencies** > **Add** drop-down list:</span></span>

   ![Область зависимостей, показывающий выбор служба Azure SignalR в раскрывающемся списке Добавить](publish-to-azure-web-app/_static/signalr-service-dependency.png)

1. <span data-ttu-id="2bee1-128">В **служба Azure SignalR** диалоговом окне выберите **создания нового экземпляра служба Azure SignalR**.</span><span class="sxs-lookup"><span data-stu-id="2bee1-128">In the **Azure SignalR Service** dialog, select **Create a new Azure SignalR Service instance**.</span></span>

1. <span data-ttu-id="2bee1-129">Укажите **имя**, **группы ресурсов**, и **расположение**.</span><span class="sxs-lookup"><span data-stu-id="2bee1-129">Provide a **Name**, **Resource Group**, and **Location**.</span></span> <span data-ttu-id="2bee1-130">Вернитесь к **служба Azure SignalR** диалогового окна и выберите **добавить**.</span><span class="sxs-lookup"><span data-stu-id="2bee1-130">Return to the **Azure SignalR Service** dialog and select **Add**.</span></span>

<span data-ttu-id="2bee1-131">Visual Studio выполняет следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="2bee1-131">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="2bee1-132">Создает профиль публикации содержащий параметры публикации.</span><span class="sxs-lookup"><span data-stu-id="2bee1-132">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="2bee1-133">Создает *веб-приложения Azure* с помощью указанных сведений.</span><span class="sxs-lookup"><span data-stu-id="2bee1-133">Creates an *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="2bee1-134">Публикует приложение.</span><span class="sxs-lookup"><span data-stu-id="2bee1-134">Publishes the app.</span></span>
* <span data-ttu-id="2bee1-135">Запускает браузер, который загружает веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="2bee1-135">Launches a browser, which loads the web app.</span></span>

<span data-ttu-id="2bee1-136">Формат URL-адреса приложения `{APP SERVICE NAME}.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="2bee1-136">The format of the app's URL is `{APP SERVICE NAME}.azurewebsites.net`.</span></span> <span data-ttu-id="2bee1-137">Например, приложение с именем `SignalRChatApp` имеет URL-адрес в `https://signalrchatapp.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="2bee1-137">For example, an app named `SignalRChatApp` has a URL of `https://signalrchatapp.azurewebsites.net`.</span></span>

<span data-ttu-id="2bee1-138">Если HTTP *502.2 - Неверный шлюз* ошибка возникает при развертывании приложение, предназначенное для предварительной версии выпуска .NET Core, см. в разделе [предварительной версии развертывание ASP.NET Core в службе приложений Azure](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service) ее устранения.</span><span class="sxs-lookup"><span data-stu-id="2bee1-138">If an HTTP *502.2 - Bad Gateway* error occurs when deploying an app that targets a preview .NET Core release, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service) to resolve it.</span></span>

## <a name="configure-the-app-in-azure-app-service"></a><span data-ttu-id="2bee1-139">Настройка приложения в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="2bee1-139">Configure the app in Azure App Service</span></span>

> [!NOTE]
> <span data-ttu-id="2bee1-140">*Этот раздел относится только к приложениям, не используется служба Azure SignalR.*</span><span class="sxs-lookup"><span data-stu-id="2bee1-140">*This section only applies to apps not using the Azure SignalR Service.*</span></span>
>
> <span data-ttu-id="2bee1-141">Если приложение использует служба Azure SignalR, службы приложений не требует конфигурации Affinity маршрутизации запросов приложений (ARR) и веб-сокеты, описанные в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="2bee1-141">If the app uses the Azure SignalR Service, the App Service doesn't require the configuration of Application Request Routing (ARR) Affinity and Web Sockets described in this section.</span></span> <span data-ttu-id="2bee1-142">Клиенты подключаются их веб-сокеты, чтобы служба Azure SignalR, не напрямую к приложению.</span><span class="sxs-lookup"><span data-stu-id="2bee1-142">Clients connect their Web Sockets to the Azure SignalR Service, not directly to the app.</span></span>

<span data-ttu-id="2bee1-143">Для приложений, размещенных без служба Azure SignalR включите:</span><span class="sxs-lookup"><span data-stu-id="2bee1-143">For apps hosted without the Azure SignalR Service, enable:</span></span>

* <span data-ttu-id="2bee1-144">[ARR сходства](https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity-cookie-(ARR-cookie)-for-Azure-web-apps.html) перенаправлять запросы от пользователя обратно в тот же экземпляр службы приложений.</span><span class="sxs-lookup"><span data-stu-id="2bee1-144">[ARR Affinity](https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity-cookie-(ARR-cookie)-for-Azure-web-apps.html) to route requests from a user back to the same App Service instance.</span></span> <span data-ttu-id="2bee1-145">Значение по умолчанию — **на**.</span><span class="sxs-lookup"><span data-stu-id="2bee1-145">The default setting is **On**.</span></span>
* <span data-ttu-id="2bee1-146">[Веб-сокеты](xref:fundamentals/websockets) чтобы разрешить веб-сокеты транспорт для функции.</span><span class="sxs-lookup"><span data-stu-id="2bee1-146">[Web Sockets](xref:fundamentals/websockets) to allow the Web Sockets transport to function.</span></span> <span data-ttu-id="2bee1-147">Значение по умолчанию — **Off**.</span><span class="sxs-lookup"><span data-stu-id="2bee1-147">The default setting is **Off**.</span></span>

1. <span data-ttu-id="2bee1-148">На портале Azure перейдите к веб-приложения в **службы приложений**.</span><span class="sxs-lookup"><span data-stu-id="2bee1-148">In the Azure portal, navigate to the web app in **App Services**.</span></span>
1. <span data-ttu-id="2bee1-149">Откройте **конфигурации** > **Общие параметры**.</span><span class="sxs-lookup"><span data-stu-id="2bee1-149">Open **Configuration** > **General settings**.</span></span>
1. <span data-ttu-id="2bee1-150">Задайте **веб-сокеты** для **на**.</span><span class="sxs-lookup"><span data-stu-id="2bee1-150">Set **Web sockets** to **On**.</span></span>
1. <span data-ttu-id="2bee1-151">Убедитесь, что **сходства ARR** присваивается **на**.</span><span class="sxs-lookup"><span data-stu-id="2bee1-151">Verify that **ARR affinity** is set to **On**.</span></span>

## <a name="app-service-plan-limits"></a><span data-ttu-id="2bee1-152">Ограничения плана службы приложений</span><span class="sxs-lookup"><span data-stu-id="2bee1-152">App Service Plan limits</span></span>

<span data-ttu-id="2bee1-153">Веб-сокеты и других транспортов ограничиваются зависимости от плана службы приложений выбран.</span><span class="sxs-lookup"><span data-stu-id="2bee1-153">Web Sockets and other transports are limited based on the App Service Plan selected.</span></span> <span data-ttu-id="2bee1-154">Дополнительные сведения см. в разделе *облачных служб Azure ограничивает* и *ограничения службы приложений* разделы [подписки Azure и ограничения служб, квоты и ограничения](/azure/azure-subscription-service-limits#app-service-limits) статья.</span><span class="sxs-lookup"><span data-stu-id="2bee1-154">For more information, see the *Azure Cloud Services limits* and *App Service limits* sections of the [Azure subscription and service limits, quotas, and constraints](/azure/azure-subscription-service-limits#app-service-limits) article.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2bee1-155">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="2bee1-155">Additional resources</span></span>

* [<span data-ttu-id="2bee1-156">Что такое служба Azure SignalR</span><span class="sxs-lookup"><span data-stu-id="2bee1-156">What is Azure SignalR Service?</span></span>](/azure/azure-signalr/signalr-overview)
* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="2bee1-157">Публикация приложения ASP.NET Core в Azure с помощью средств командной строки</span><span class="sxs-lookup"><span data-stu-id="2bee1-157">Publish an ASP.NET Core app to Azure with command line tools</span></span>](/azure/app-service/app-service-web-get-started-dotnet)
* [<span data-ttu-id="2bee1-158">Размещение и развертывание приложений ASP.NET Core Preview в Azure</span><span class="sxs-lookup"><span data-stu-id="2bee1-158">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
