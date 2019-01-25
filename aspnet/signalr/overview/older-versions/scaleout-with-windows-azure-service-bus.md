---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: Масштабирование SignalR с помощью Azure Service Bus (SignalR 1.x) | Документация Майкрософт
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 77186f43b38a8423a1cbd4cf42723c5b9ccdd953
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837914"
---
<a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a><span data-ttu-id="8ad3d-102">Масштабирование SignalR с помощью Azure Service Bus (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="8ad3d-102">SignalR Scaleout with Azure Service Bus (SignalR 1.x)</span></span>
====================
<span data-ttu-id="8ad3d-103">по [Майк Уоссон](https://github.com/MikeWasson), [Патрик Флетчера](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="8ad3d-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="8ad3d-104">В этом руководстве вы развернете приложении SignalR для веб-роли Windows Azure, использование объединительной платы служебной шины для распределения сообщений для каждого экземпляра роли.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-104">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="8ad3d-105">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="8ad3d-105">Prerequisites:</span></span>

- <span data-ttu-id="8ad3d-106">Учетная запись Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-106">A Windows Azure account.</span></span>
- <span data-ttu-id="8ad3d-107">[Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="8ad3d-107">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="8ad3d-108">Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-108">Visual Studio 2012.</span></span>

<span data-ttu-id="8ad3d-109">На задней стороне шины службы также совместима с [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), версии 1.1.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-109">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="8ad3d-110">Тем не менее не совместима с версией 1.0 Service Bus for Windows Server.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-110">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="8ad3d-111">Цены</span><span class="sxs-lookup"><span data-stu-id="8ad3d-111">Pricing</span></span>

<span data-ttu-id="8ad3d-112">На задней стороне служебная шина использует разделы для отправки сообщений.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-112">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="8ad3d-113">Сведения о последних ценах, см. в разделе [служебной шины](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="8ad3d-113">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="8ad3d-114">Во время написания данной статьи вы можете отправлять 1 000 000 сообщений в месяц для меньше, чем 1 долл. США.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-114">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="8ad3d-115">Задняя панель отправляет сообщение служебной шины для каждого вызова метода концентратора SignalR.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-115">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="8ad3d-116">Кроме того, существуют также некоторые сообщения управления для подключения, отключения, присоединяемый или создание групп и т. д.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-116">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="8ad3d-117">В большинстве приложений большая часть трафика сообщений будет вызовы методов концентратора.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-117">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="8ad3d-118">Обзор</span><span class="sxs-lookup"><span data-stu-id="8ad3d-118">Overview</span></span>

<span data-ttu-id="8ad3d-119">Прежде чем мы перейдем к подробный учебник, ниже приведен краткий обзор. ваши действия.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-119">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="8ad3d-120">Создать новое пространство имен служебной шины с помощью портала Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-120">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="8ad3d-121">Добавьте следующие пакеты NuGet приложения:</span><span class="sxs-lookup"><span data-stu-id="8ad3d-121">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="8ad3d-122">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="8ad3d-122">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="8ad3d-123">Microsoft.AspNet.SignalR.ServiceBus</span><span class="sxs-lookup"><span data-stu-id="8ad3d-123">Microsoft.AspNet.SignalR.ServiceBus</span></span>](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. <span data-ttu-id="8ad3d-124">Создайте приложение SignalR.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-124">Create a SignalR application.</span></span>
4. <span data-ttu-id="8ad3d-125">Добавьте следующий код в Global.asax, чтобы настроить на задней стороне:</span><span class="sxs-lookup"><span data-stu-id="8ad3d-125">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="8ad3d-126">Для каждого приложения выберите другое значение для «YourAppName».</span><span class="sxs-lookup"><span data-stu-id="8ad3d-126">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="8ad3d-127">Не используйте то же значение в нескольких приложениях.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-127">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="8ad3d-128">Создание служб Azure</span><span class="sxs-lookup"><span data-stu-id="8ad3d-128">Create the Azure Services</span></span>

<span data-ttu-id="8ad3d-129">Создание облачной службы, как описано в разделе [как создать и развернуть облачную службу](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="8ad3d-129">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="8ad3d-130">Выполните действия, описанные в разделе «как: Создание облачной службы с помощью функции быстрого создания».</span><span class="sxs-lookup"><span data-stu-id="8ad3d-130">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="8ad3d-131">В этом руководстве вы не обязательно должны передать сертификат.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-131">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="8ad3d-132">Создайте новое пространство имен служебной шины, как описано в [как для использования разделов и подписок служебной шины](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="8ad3d-132">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="8ad3d-133">Выполните действия, описанные в разделе «Создание имен службы».</span><span class="sxs-lookup"><span data-stu-id="8ad3d-133">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="8ad3d-134">Убедитесь в том, что выберите тот же регион для облачной службы и пространство имен служебной шины.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-134">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="8ad3d-135">Создание проекта Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8ad3d-135">Create the Visual Studio Project</span></span>

<span data-ttu-id="8ad3d-136">Запустите Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-136">Start Visual Studio.</span></span> <span data-ttu-id="8ad3d-137">Из **файл** меню, щелкните **новый проект**.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-137">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="8ad3d-138">В **новый проект** диалоговом окне разверните **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-138">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="8ad3d-139">В разделе **установленные шаблоны**выберите **Cloud** , а затем выберите **облачной службы Windows Azure**.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-139">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="8ad3d-140">Оставьте по умолчанию .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-140">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="8ad3d-141">Присвойте имя приложению ChatService, а затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-141">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="8ad3d-142">В **новая облачная служба Windows Azure** диалоговое окно, выберите веб-роль ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-142">In the **New Windows Azure Cloud Service** dialog, select ASP.NET MVC 4 Web Role.</span></span> <span data-ttu-id="8ad3d-143">Нажмите кнопку со стрелкой вправо (**&gt;**) чтобы добавить роль в решение.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-143">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="8ad3d-144">Наведите указатель мыши новую роль, поэтому отображается значок с изображением карандаша.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-144">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="8ad3d-145">Щелкните этот значок, чтобы переименовать роль.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-145">Click this icon to rename the role.</span></span> <span data-ttu-id="8ad3d-146">Имя роли «SignalRChat» и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-146">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="8ad3d-147">В **создания проекта ASP.NET MVC 4** мастера выберите **веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-147">In the **New ASP.NET MVC 4 Project** wizard, select **Internet Application**.</span></span> <span data-ttu-id="8ad3d-148">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-148">Click **OK**.</span></span> <span data-ttu-id="8ad3d-149">Мастер проекта создает два проекта:</span><span class="sxs-lookup"><span data-stu-id="8ad3d-149">The project wizard creates two projects:</span></span>

- <span data-ttu-id="8ad3d-150">ChatService: Этот проект является приложением Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-150">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="8ad3d-151">Он определяет роли Azure и других параметров конфигурации.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-151">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="8ad3d-152">SignalRChat: Этот проект является проекта ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-152">SignalRChat: This project is your ASP.NET MVC 4 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="8ad3d-153">Создание приложения чата SignalR</span><span class="sxs-lookup"><span data-stu-id="8ad3d-153">Create the SignalR Chat Application</span></span>

<span data-ttu-id="8ad3d-154">Чтобы создать приложение чата, следуйте указаниям в этом руководстве [начало работы с SignalR и MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="8ad3d-154">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="8ad3d-155">Используйте NuGet для установки необходимых библиотек.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-155">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="8ad3d-156">Из **средства** меню, выберите **диспетчер пакетов NuGet**, а затем выберите **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-156">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="8ad3d-157">В **консоль диспетчера пакетов** окно, введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="8ad3d-157">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="8ad3d-158">Используйте `-ProjectName` параметр для установки пакетов проекта ASP.NET MVC, а не проекта Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-158">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="8ad3d-159">Настройка задней панели</span><span class="sxs-lookup"><span data-stu-id="8ad3d-159">Configure the Backplane</span></span>

<span data-ttu-id="8ad3d-160">В файле Global.asax приложения добавьте следующий код:</span><span class="sxs-lookup"><span data-stu-id="8ad3d-160">In your application's Global.asax file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="8ad3d-161">Теперь необходимо получить строку подключения service bus.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-161">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="8ad3d-162">На портале Azure выберите пространство имен служебной шины, который вы создали и щелкните значок ключа доступа.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-162">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="8ad3d-163">Скопируйте строку подключения в буфер обмена, а затем вставьте его в *connectionString* переменной.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-163">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="8ad3d-164">Развертывание в Azure</span><span class="sxs-lookup"><span data-stu-id="8ad3d-164">Deploy to Azure</span></span>

<span data-ttu-id="8ad3d-165">В обозревателе решений разверните **ролей** папку внутри проекта ChatService.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-165">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

<span data-ttu-id="8ad3d-166">Щелкните правой кнопкой мыши роль SignalRChat и выберите **свойства**.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-166">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="8ad3d-167">Выберите **конфигурации** вкладки. В разделе **экземпляров** выберите 2.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-167">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="8ad3d-168">Можно также задать размер виртуальной Машины **очень мелкий**.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-168">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="8ad3d-169">Сохраните изменения.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-169">Save the changes.</span></span>

<span data-ttu-id="8ad3d-170">В обозревателе решений щелкните правой кнопкой мыши проект ChatService.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-170">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="8ad3d-171">Нажмите **Публиковать**.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-171">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="8ad3d-172">Если это ваш первый время публикации в Windows Azure, необходимо загрузить учетные данные.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-172">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="8ad3d-173">В **публикации** мастера, нажмите кнопку «Вход для загрузки учетных данных».</span><span class="sxs-lookup"><span data-stu-id="8ad3d-173">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="8ad3d-174">Это предложит Войдите на портал Windows Azure и загрузите файл параметров публикации.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-174">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="8ad3d-175">Нажмите кнопку **импорта** и выберите загруженный файл параметров публикации.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-175">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="8ad3d-176">Нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-176">Click **Next**.</span></span> <span data-ttu-id="8ad3d-177">В **параметры публикации** диалогового окна в разделе **облачной службы**, выберите облачную службу, созданную ранее.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-177">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="8ad3d-178">Нажмите кнопку **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-178">Click **Publish**.</span></span> <span data-ttu-id="8ad3d-179">Может занять несколько минут, чтобы развернуть приложение и запустить виртуальные машины.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-179">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="8ad3d-180">Теперь при запуске приложения чата, экземпляры ролей, взаимодействуют через служебную шину Azure, используя раздел служебной шины.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-180">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="8ad3d-181">Тема — это очередь сообщений, которая позволяет нескольким подписчикам.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-181">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="8ad3d-182">Задняя панель автоматически создает раздела и подписки.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-182">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="8ad3d-183">Чтобы увидеть подписки и действие сообщения, откройте портал Azure, выберите пространство имен служебной шины и щелкните «Разделы».</span><span class="sxs-lookup"><span data-stu-id="8ad3d-183">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="8ad3d-184">Берет на себя занять несколько минут для действия сообщения для отображения на панели мониторинга.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-184">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="8ad3d-185">SignalR управляет временем существования раздела.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-185">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="8ad3d-186">До тех пор, пока развертывается приложение, не следует пытаться вручную удалить разделы или изменить параметры в разделе.</span><span class="sxs-lookup"><span data-stu-id="8ad3d-186">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>
