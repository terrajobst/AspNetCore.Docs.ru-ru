---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: SignalR горизонтального масштабирования с Azure Service Bus (SignalR 1.x) | Документы Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: b48a7b04701b69f68a492c0f7e08da4a37a92a48
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "28036444"
---
<a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a><span data-ttu-id="46942-102">SignalR горизонтального масштабирования с Azure Service Bus (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="46942-102">SignalR Scaleout with Azure Service Bus (SignalR 1.x)</span></span>
====================
<span data-ttu-id="46942-103">по [Mike Wasson](https://github.com/MikeWasson), [Патрик Флетчера](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="46942-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="46942-104">В этом учебнике будет развертывание приложения с SignalR для веб-роли Windows Azure, используя на задней стороне Service Bus для распределения сообщений для каждого экземпляра роли.</span><span class="sxs-lookup"><span data-stu-id="46942-104">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="46942-105">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="46942-105">Prerequisites:</span></span>

- <span data-ttu-id="46942-106">Учетная запись Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="46942-106">A Windows Azure account.</span></span>
- <span data-ttu-id="46942-107">[Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="46942-107">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="46942-108">Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="46942-108">Visual Studio 2012.</span></span>

<span data-ttu-id="46942-109">Задняя панель шины службы также совместим с [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), версия 1.1.</span><span class="sxs-lookup"><span data-stu-id="46942-109">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="46942-110">Однако он не совместим с версией 1.0 Service Bus for Windows Server.</span><span class="sxs-lookup"><span data-stu-id="46942-110">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="46942-111">Цены</span><span class="sxs-lookup"><span data-stu-id="46942-111">Pricing</span></span>

<span data-ttu-id="46942-112">На задней стороне Service Bus использует разделы для отправки сообщений.</span><span class="sxs-lookup"><span data-stu-id="46942-112">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="46942-113">Последнюю информацию, см. [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="46942-113">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="46942-114">На момент написания этой статьи можно отправить 1 000 000 сообщений в месяц для меньше $1.</span><span class="sxs-lookup"><span data-stu-id="46942-114">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="46942-115">Задняя панель отправляет сообщение служебной шины для каждого вызова метода концентратора SignalR.</span><span class="sxs-lookup"><span data-stu-id="46942-115">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="46942-116">Существуют также некоторые управляющие сообщения для подключения, отключения, присоединяемый или создание групп и т. д.</span><span class="sxs-lookup"><span data-stu-id="46942-116">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="46942-117">В большинстве приложений большая часть трафика сообщений будет вызовы методов концентратора.</span><span class="sxs-lookup"><span data-stu-id="46942-117">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="46942-118">Обзор</span><span class="sxs-lookup"><span data-stu-id="46942-118">Overview</span></span>

<span data-ttu-id="46942-119">Прежде чем мы приступим к подробный учебник, ниже приведен краткий обзор того, что сделать.</span><span class="sxs-lookup"><span data-stu-id="46942-119">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="46942-120">Используйте портал Windows Azure для создания нового пространства имен Service Bus.</span><span class="sxs-lookup"><span data-stu-id="46942-120">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="46942-121">Добавьте эти пакеты NuGet для приложения:</span><span class="sxs-lookup"><span data-stu-id="46942-121">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="46942-122">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="46942-122">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="46942-123">Microsoft.AspNet.SignalR.ServiceBus</span><span class="sxs-lookup"><span data-stu-id="46942-123">Microsoft.AspNet.SignalR.ServiceBus</span></span>](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. <span data-ttu-id="46942-124">Создайте приложение SignalR.</span><span class="sxs-lookup"><span data-stu-id="46942-124">Create a SignalR application.</span></span>
4. <span data-ttu-id="46942-125">Добавьте следующий код Global.asax для настройки на задней стороне:</span><span class="sxs-lookup"><span data-stu-id="46942-125">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="46942-126">Для каждого приложения выберите другое значение для «YourAppName».</span><span class="sxs-lookup"><span data-stu-id="46942-126">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="46942-127">Не используйте то же значение для нескольких приложений.</span><span class="sxs-lookup"><span data-stu-id="46942-127">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="46942-128">Создание служб Azure</span><span class="sxs-lookup"><span data-stu-id="46942-128">Create the Azure Services</span></span>

<span data-ttu-id="46942-129">Создание облачной службы, как описано в [Создание и развертывание облачной службы](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="46942-129">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="46942-130">Следуйте указаниям в разделе «как: создать облачную службу с помощью быстрого создания».</span><span class="sxs-lookup"><span data-stu-id="46942-130">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="46942-131">В этом учебнике необходимо отправить сертификат.</span><span class="sxs-lookup"><span data-stu-id="46942-131">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="46942-132">Создайте новое пространство имен Service Bus, как описано в [как для использования службы подписок и разделов шины](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="46942-132">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="46942-133">Следуйте указаниям в разделе «Создание службы пространство имен».</span><span class="sxs-lookup"><span data-stu-id="46942-133">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="46942-134">Убедитесь, выбираемых в том же регионе для облачной службы и пространство имен служебной шины.</span><span class="sxs-lookup"><span data-stu-id="46942-134">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="46942-135">Создание проекта Visual Studio</span><span class="sxs-lookup"><span data-stu-id="46942-135">Create the Visual Studio Project</span></span>

<span data-ttu-id="46942-136">Запустите Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="46942-136">Start Visual Studio.</span></span> <span data-ttu-id="46942-137">Из **файл** меню, нажмите кнопку **новый проект**.</span><span class="sxs-lookup"><span data-stu-id="46942-137">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="46942-138">В **новый проект** диалогового окна разверните **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="46942-138">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="46942-139">В разделе **установленные шаблоны**выберите **облака** , а затем выберите **облачных служб Windows Azure**.</span><span class="sxs-lookup"><span data-stu-id="46942-139">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="46942-140">Оставьте значение по умолчанию .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="46942-140">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="46942-141">Имя приложения ChatService и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="46942-141">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="46942-142">В **новой облачной службы Windows Azure** диалоговое окно, выберите веб-роль ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="46942-142">In the **New Windows Azure Cloud Service** dialog, select ASP.NET MVC 4 Web Role.</span></span> <span data-ttu-id="46942-143">Нажмите кнопку со стрелкой вправо (**&gt;**) чтобы добавить роль в решение.</span><span class="sxs-lookup"><span data-stu-id="46942-143">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="46942-144">Наведите указатель мыши новой роли, поэтому отображается значок карандаша.</span><span class="sxs-lookup"><span data-stu-id="46942-144">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="46942-145">Щелкните этот значок, чтобы переименовать роль.</span><span class="sxs-lookup"><span data-stu-id="46942-145">Click this icon to rename the role.</span></span> <span data-ttu-id="46942-146">Имя роли «SignalRChat» и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="46942-146">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="46942-147">В **нового проекта ASP.NET MVC 4** мастера выберите **веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="46942-147">In the **New ASP.NET MVC 4 Project** wizard, select **Internet Application**.</span></span> <span data-ttu-id="46942-148">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="46942-148">Click **OK**.</span></span> <span data-ttu-id="46942-149">Мастер проекта создает два проекта:</span><span class="sxs-lookup"><span data-stu-id="46942-149">The project wizard creates two projects:</span></span>

- <span data-ttu-id="46942-150">ChatService: В этот проект является приложением Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="46942-150">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="46942-151">Он определяет роли Azure и других параметров конфигурации.</span><span class="sxs-lookup"><span data-stu-id="46942-151">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="46942-152">SignalRChat: Этот проект является проект ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="46942-152">SignalRChat: This project is your ASP.NET MVC 4 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="46942-153">Создание приложения разговора SignalR</span><span class="sxs-lookup"><span data-stu-id="46942-153">Create the SignalR Chat Application</span></span>

<span data-ttu-id="46942-154">Чтобы создать приложения разговора, выполните шаги в учебнике [Приступая к работе с SignalR и MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="46942-154">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="46942-155">Установите необходимые библиотеки с помощью NuGet.</span><span class="sxs-lookup"><span data-stu-id="46942-155">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="46942-156">Из **средства** последовательно выберите пункты **диспетчер пакетов библиотеки**, а затем выберите **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="46942-156">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="46942-157">В **консоль диспетчера пакетов** окно, введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="46942-157">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="46942-158">Используйте `-ProjectName` параметр для установки пакетов проекта ASP.NET MVC, а не в проекте Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="46942-158">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="46942-159">Настройка объединительной плате</span><span class="sxs-lookup"><span data-stu-id="46942-159">Configure the Backplane</span></span>

<span data-ttu-id="46942-160">В файле Global.asax приложения добавьте следующий код:</span><span class="sxs-lookup"><span data-stu-id="46942-160">In your application's Global.asax file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="46942-161">Теперь вам нужно получить строки подключения service bus.</span><span class="sxs-lookup"><span data-stu-id="46942-161">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="46942-162">На портале Azure выберите пространство имен служебной шины, созданный и щелкните значок ключа доступа.</span><span class="sxs-lookup"><span data-stu-id="46942-162">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="46942-163">Скопируйте строку подключения в буфер обмена, а затем вставьте его в *connectionString* переменной.</span><span class="sxs-lookup"><span data-stu-id="46942-163">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="46942-164">Развертывание в Azure</span><span class="sxs-lookup"><span data-stu-id="46942-164">Deploy to Azure</span></span>

<span data-ttu-id="46942-165">В обозревателе решений откройте **ролей** папку внутри проекта ChatService.</span><span class="sxs-lookup"><span data-stu-id="46942-165">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

<span data-ttu-id="46942-166">Щелкните правой кнопкой мыши роль SignalRChat и выберите **свойства**.</span><span class="sxs-lookup"><span data-stu-id="46942-166">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="46942-167">Выберите **конфигурации** вкладки. В разделе **экземпляров** выберите 2.</span><span class="sxs-lookup"><span data-stu-id="46942-167">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="46942-168">Можно также задать размер виртуальной Машины **чрезвычайно малая**.</span><span class="sxs-lookup"><span data-stu-id="46942-168">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="46942-169">Сохраните изменения.</span><span class="sxs-lookup"><span data-stu-id="46942-169">Save the changes.</span></span>

<span data-ttu-id="46942-170">В обозревателе решений щелкните правой кнопкой мыши проект ChatService.</span><span class="sxs-lookup"><span data-stu-id="46942-170">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="46942-171">Нажмите **Публиковать**.</span><span class="sxs-lookup"><span data-stu-id="46942-171">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="46942-172">Если это ваш первый время публикации в Windows Azure, необходимо загрузить учетные данные.</span><span class="sxs-lookup"><span data-stu-id="46942-172">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="46942-173">В **публикации** мастера, нажмите кнопку «Вход для загрузки учетных данных».</span><span class="sxs-lookup"><span data-stu-id="46942-173">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="46942-174">Появится запрос для входа на портал Windows Azure и загрузите файл параметров публикации.</span><span class="sxs-lookup"><span data-stu-id="46942-174">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="46942-175">Нажмите кнопку **импорта** и выберите загруженный файл параметров публикации.</span><span class="sxs-lookup"><span data-stu-id="46942-175">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="46942-176">Нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="46942-176">Click **Next**.</span></span> <span data-ttu-id="46942-177">В **параметры публикации** диалогового окна в разделе **облачной службы**, выберите ранее созданную облачную службу.</span><span class="sxs-lookup"><span data-stu-id="46942-177">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="46942-178">Нажмите кнопку **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="46942-178">Click **Publish**.</span></span> <span data-ttu-id="46942-179">Он может занять несколько минут, чтобы развернуть приложение и запустить виртуальные машины.</span><span class="sxs-lookup"><span data-stu-id="46942-179">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="46942-180">Теперь при запуске приложения разговора экземпляры роли, взаимодействуют через Azure Service Bus с помощью раздела Service Bus.</span><span class="sxs-lookup"><span data-stu-id="46942-180">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="46942-181">Раздел является очередью сообщений, которая позволяет нескольким подписчикам.</span><span class="sxs-lookup"><span data-stu-id="46942-181">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="46942-182">Объединительной плате автоматически создает раздел и подписки.</span><span class="sxs-lookup"><span data-stu-id="46942-182">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="46942-183">Для просмотра подписок и действие сообщения, откройте портал Azure, выберите пространство имен служебной шины и выберите команду «Разделы».</span><span class="sxs-lookup"><span data-stu-id="46942-183">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="46942-184">Он может занять несколько минут, действие сообщения, отображаются в панели мониторинга.</span><span class="sxs-lookup"><span data-stu-id="46942-184">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="46942-185">SignalR управляет временем существования раздела.</span><span class="sxs-lookup"><span data-stu-id="46942-185">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="46942-186">При условии, что приложение развертывается, не пытайтесь вручную удалить разделы или измените параметры в разделе.</span><span class="sxs-lookup"><span data-stu-id="46942-186">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>
