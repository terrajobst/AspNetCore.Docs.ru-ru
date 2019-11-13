---
title: Публикация приложения ASP.NET Core SignalR в службе приложений Azure
author: bradygaster
description: Узнайте, как опубликовать приложение ASP.NET Core SignalR в службе приложений Azure.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: d03a007ca883b3d0391b848e3e92c90469ee640a
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963932"
---
# <a name="publish-an-aspnet-core-opno-locsignalr-app-to-azure-app-service"></a><span data-ttu-id="cf0cd-103">Публикация приложения ASP.NET Core SignalR в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="cf0cd-103">Publish an ASP.NET Core SignalR app to Azure App Service</span></span>

<span data-ttu-id="cf0cd-104">По [Брейди Гастер](https://twitter.com/bradygaster)</span><span class="sxs-lookup"><span data-stu-id="cf0cd-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="cf0cd-105">[Служба приложений Azure](/azure/app-service/app-service-web-overview) — это служба платформы [облачных вычислений Майкрософт](https://azure.microsoft.com/) для размещения веб-приложений, включая ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cf0cd-105">[Azure App Service](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

> [!NOTE]
> <span data-ttu-id="cf0cd-106">В этой статье описывается публикация приложения ASP.NET Core SignalR из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cf0cd-106">This article refers to publishing an ASP.NET Core SignalR app from Visual Studio.</span></span> <span data-ttu-id="cf0cd-107">Дополнительные сведения см. в статье [SignalR Service for Azure](https://azure.microsoft.com/services/signalr-service).</span><span class="sxs-lookup"><span data-stu-id="cf0cd-107">For more information, see [SignalR service for Azure](https://azure.microsoft.com/services/signalr-service).</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="cf0cd-108">Публикация приложения</span><span class="sxs-lookup"><span data-stu-id="cf0cd-108">Publish the app</span></span>

<span data-ttu-id="cf0cd-109">В этой статье описывается публикация с помощью средств в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cf0cd-109">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="cf0cd-110">Visual Studio Code пользователи могут использовать команды [Azure CLI](/cli/azure) для публикации приложений в Azure.</span><span class="sxs-lookup"><span data-stu-id="cf0cd-110">Visual Studio Code users can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="cf0cd-111">Дополнительные сведения см. в статье [публикация ASP.NET Core приложения в Azure с помощью средств командной строки](/azure/app-service/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="cf0cd-111">For more information, see [Publish an ASP.NET Core app to Azure with command line tools](/azure/app-service/app-service-web-get-started-dotnet).</span></span>

1. <span data-ttu-id="cf0cd-112">В **обозревателе решений** щелкните правой кнопкой мыши проект и выберите **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="cf0cd-112">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span>

1. <span data-ttu-id="cf0cd-113">Убедитесь, что в диалоговом окне **Выбор целевого объекта публикации** выбран параметр **служба приложений** и **создать новые** .</span><span class="sxs-lookup"><span data-stu-id="cf0cd-113">Confirm that **App Service** and **Create new** are selected in the **Pick a publish target** dialog.</span></span>

1. <span data-ttu-id="cf0cd-114">Выберите **создать профиль** из раскрывающегося списка кнопка " **опубликовать** ".</span><span class="sxs-lookup"><span data-stu-id="cf0cd-114">Select **Create Profile** from the **Publish** button drop down.</span></span>

   <span data-ttu-id="cf0cd-115">Введите сведения, описанные в следующей таблице в диалоговом окне **Создание службы приложений** , и выберите **создать**.</span><span class="sxs-lookup"><span data-stu-id="cf0cd-115">Enter the information described in the following table in the **Create App Service** dialog and select **Create**.</span></span>

   | <span data-ttu-id="cf0cd-116">Элемент</span><span class="sxs-lookup"><span data-stu-id="cf0cd-116">Item</span></span>               | <span data-ttu-id="cf0cd-117">Описание</span><span class="sxs-lookup"><span data-stu-id="cf0cd-117">Description</span></span> |
   | ------------------ | ----------- |
   | <span data-ttu-id="cf0cd-118">**Name**</span><span class="sxs-lookup"><span data-stu-id="cf0cd-118">**Name**</span></span>           | <span data-ttu-id="cf0cd-119">Уникальное имя приложения.</span><span class="sxs-lookup"><span data-stu-id="cf0cd-119">Unique name of the app.</span></span> |
   | <span data-ttu-id="cf0cd-120">**Подписка**</span><span class="sxs-lookup"><span data-stu-id="cf0cd-120">**Subscription**</span></span>   | <span data-ttu-id="cf0cd-121">Подписка Azure, используемая приложением.</span><span class="sxs-lookup"><span data-stu-id="cf0cd-121">Azure subscription that the app uses.</span></span> |
   | <span data-ttu-id="cf0cd-122">**Группа ресурсов**</span><span class="sxs-lookup"><span data-stu-id="cf0cd-122">**Resource Group**</span></span> | <span data-ttu-id="cf0cd-123">Группа связанных ресурсов, к которым принадлежит приложение.</span><span class="sxs-lookup"><span data-stu-id="cf0cd-123">Group of related resources to which the app belongs.</span></span> |
   | <span data-ttu-id="cf0cd-124">**План размещения**</span><span class="sxs-lookup"><span data-stu-id="cf0cd-124">**Hosting Plan**</span></span>   | <span data-ttu-id="cf0cd-125">Тарифный план для веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="cf0cd-125">Pricing plan for the web app.</span></span> |

1. <span data-ttu-id="cf0cd-126">Выберите **службу SignalR Azure** в раскрывающемся списке **зависимости** > **добавить** :</span><span class="sxs-lookup"><span data-stu-id="cf0cd-126">Select the **Azure SignalR Service** in the **Dependencies** > **Add** drop-down list:</span></span>

   ![Область зависимостей, отображающая выбор Azure [! Операцион. Служба NO-LOC (SignalR)] в раскрывающемся списке "Добавить"](publish-to-azure-web-app/_static/signalr-service-dependency.png)

1. <span data-ttu-id="cf0cd-128">В диалоговом окне **службы SignalR Azure** выберите **создать новый экземпляр службы SignalR Azure**.</span><span class="sxs-lookup"><span data-stu-id="cf0cd-128">In the **Azure SignalR Service** dialog, select **Create a new Azure SignalR Service instance**.</span></span>

1. <span data-ttu-id="cf0cd-129">Укажите **имя**, **группу ресурсов**и **Расположение**.</span><span class="sxs-lookup"><span data-stu-id="cf0cd-129">Provide a **Name**, **Resource Group**, and **Location**.</span></span> <span data-ttu-id="cf0cd-130">Вернитесь в диалоговое окно **службы SignalR Azure** и нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="cf0cd-130">Return to the **Azure SignalR Service** dialog and select **Add**.</span></span>

<span data-ttu-id="cf0cd-131">Visual Studio выполняет следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="cf0cd-131">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="cf0cd-132">Создает профиль публикации, содержащий параметры публикации.</span><span class="sxs-lookup"><span data-stu-id="cf0cd-132">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="cf0cd-133">Создает *веб-приложение Azure* с предоставленными сведениями.</span><span class="sxs-lookup"><span data-stu-id="cf0cd-133">Creates an *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="cf0cd-134">Публикует приложение.</span><span class="sxs-lookup"><span data-stu-id="cf0cd-134">Publishes the app.</span></span>
* <span data-ttu-id="cf0cd-135">Запускает браузер, который загружает веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="cf0cd-135">Launches a browser, which loads the web app.</span></span>

<span data-ttu-id="cf0cd-136">Формат URL-адреса приложения `{APP SERVICE NAME}.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="cf0cd-136">The format of the app's URL is `{APP SERVICE NAME}.azurewebsites.net`.</span></span> <span data-ttu-id="cf0cd-137">Например, приложение с именем `SignalRChatApp` имеет URL-адрес `https://signalrchatapp.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="cf0cd-137">For example, an app named `SignalRChatApp` has a URL of `https://signalrchatapp.azurewebsites.net`.</span></span>

<span data-ttu-id="cf0cd-138">Если при развертывании приложения, предназначенного для предварительной версии .NET Core, возникает ошибка HTTP *502,2-Bad Gateway* , см. статью [развертывание ASP.NET Core предварительной версии в службе приложений Azure](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service) .</span><span class="sxs-lookup"><span data-stu-id="cf0cd-138">If an HTTP *502.2 - Bad Gateway* error occurs when deploying an app that targets a preview .NET Core release, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service) to resolve it.</span></span>

## <a name="configure-the-app-in-azure-app-service"></a><span data-ttu-id="cf0cd-139">Настройка приложения в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="cf0cd-139">Configure the app in Azure App Service</span></span>

> [!NOTE]
> <span data-ttu-id="cf0cd-140">*Этот раздел относится только к приложениям, которые не используют службу SignalR Azure.*</span><span class="sxs-lookup"><span data-stu-id="cf0cd-140">*This section only applies to apps not using the Azure SignalR Service.*</span></span>
>
> <span data-ttu-id="cf0cd-141">Если приложение использует службу SignalR Azure, служба приложений не требует настройки сходства маршрутизации запросов приложений (ARR) и веб-сокетов, описанных в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="cf0cd-141">If the app uses the Azure SignalR Service, the App Service doesn't require the configuration of Application Request Routing (ARR) Affinity and Web Sockets described in this section.</span></span> <span data-ttu-id="cf0cd-142">Клиенты подключают свои веб-сокеты к службе SignalR Azure, а не напрямую к приложению.</span><span class="sxs-lookup"><span data-stu-id="cf0cd-142">Clients connect their Web Sockets to the Azure SignalR Service, not directly to the app.</span></span>

<span data-ttu-id="cf0cd-143">Для приложений, размещенных без службы Azure SignalR, включите:</span><span class="sxs-lookup"><span data-stu-id="cf0cd-143">For apps hosted without the Azure SignalR Service, enable:</span></span>

* <span data-ttu-id="cf0cd-144">[Сопоставление arr](https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity-cookie-(ARR-cookie)-for-Azure-web-apps.html) для маршрутизации запросов от пользователя к тому же экземпляру службы приложений.</span><span class="sxs-lookup"><span data-stu-id="cf0cd-144">[ARR Affinity](https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity-cookie-(ARR-cookie)-for-Azure-web-apps.html) to route requests from a user back to the same App Service instance.</span></span> <span data-ttu-id="cf0cd-145">Значение по умолчанию — **On**.</span><span class="sxs-lookup"><span data-stu-id="cf0cd-145">The default setting is **On**.</span></span>
* <span data-ttu-id="cf0cd-146">[Веб-сокеты](xref:fundamentals/websockets) для обеспечения функционирования транспорта веб-сокетов.</span><span class="sxs-lookup"><span data-stu-id="cf0cd-146">[Web Sockets](xref:fundamentals/websockets) to allow the Web Sockets transport to function.</span></span> <span data-ttu-id="cf0cd-147">Значение по умолчанию — **Off**.</span><span class="sxs-lookup"><span data-stu-id="cf0cd-147">The default setting is **Off**.</span></span>

1. <span data-ttu-id="cf0cd-148">В портал Azure перейдите к веб-приложению в **службах приложений**.</span><span class="sxs-lookup"><span data-stu-id="cf0cd-148">In the Azure portal, navigate to the web app in **App Services**.</span></span>
1. <span data-ttu-id="cf0cd-149">Откройте **конфигурацию** > **Общие параметры**.</span><span class="sxs-lookup"><span data-stu-id="cf0cd-149">Open **Configuration** > **General settings**.</span></span>
1. <span data-ttu-id="cf0cd-150">Установите для **веб-сокетов** значение **вкл**.</span><span class="sxs-lookup"><span data-stu-id="cf0cd-150">Set **Web sockets** to **On**.</span></span>
1. <span data-ttu-id="cf0cd-151">Убедитесь, что для параметра **сходство arr** установлено значение **вкл**.</span><span class="sxs-lookup"><span data-stu-id="cf0cd-151">Verify that **ARR affinity** is set to **On**.</span></span>

## <a name="app-service-plan-limits"></a><span data-ttu-id="cf0cd-152">Ограничения плана службы приложений</span><span class="sxs-lookup"><span data-stu-id="cf0cd-152">App Service Plan limits</span></span>

<span data-ttu-id="cf0cd-153">Веб-сокеты и другие транспорты ограничены в зависимости от выбранного плана службы приложений.</span><span class="sxs-lookup"><span data-stu-id="cf0cd-153">Web Sockets and other transports are limited based on the App Service Plan selected.</span></span> <span data-ttu-id="cf0cd-154">Дополнительные сведения см. в разделе *ограничения облачных служб Azure* и *ограничения службы приложений* статьи [Подписка Azure и ограничения службы, квоты и ограничения](/azure/azure-subscription-service-limits#app-service-limits) .</span><span class="sxs-lookup"><span data-stu-id="cf0cd-154">For more information, see the *Azure Cloud Services limits* and *App Service limits* sections of the [Azure subscription and service limits, quotas, and constraints](/azure/azure-subscription-service-limits#app-service-limits) article.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cf0cd-155">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="cf0cd-155">Additional resources</span></span>

* <span data-ttu-id="cf0cd-156">[Что такое служба SignalR Azure?](/azure/azure-signalr/signalr-overview)</span><span class="sxs-lookup"><span data-stu-id="cf0cd-156">[What is Azure SignalR Service?](/azure/azure-signalr/signalr-overview)</span></span>
* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="cf0cd-157">Публикация ASP.NET Core приложения в Azure с помощью средств командной строки</span><span class="sxs-lookup"><span data-stu-id="cf0cd-157">Publish an ASP.NET Core app to Azure with command line tools</span></span>](/azure/app-service/app-service-web-get-started-dotnet)
* [<span data-ttu-id="cf0cd-158">Размещение и развертывание приложений ASP.NET Core Preview в Azure</span><span class="sxs-lookup"><span data-stu-id="cf0cd-158">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
