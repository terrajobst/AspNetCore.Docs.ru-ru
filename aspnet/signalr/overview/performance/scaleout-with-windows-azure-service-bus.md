---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: Масштабирование SignalR с помощью Azure Service Bus | Документация Майкрософт
author: MikeWasson
description: Версии программного обеспечения используется в этой версии Visual Studio 2013 .NET 4.5 SignalR раздел 2 предыдущие версии этого раздела для SignalR 1.x версии этого раздела...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 5cdb9b5eb6d3f5ebd5c96e4b0d89926c18bddadd
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287615"
---
<a name="signalr-scaleout-with-azure-service-bus"></a><span data-ttu-id="aad2d-103">Масштабирование SignalR с помощью Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="aad2d-103">SignalR Scaleout with Azure Service Bus</span></span>
====================
<span data-ttu-id="aad2d-104">по [Майк Уоссон](https://github.com/MikeWasson), [Патрик Флетчера](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="aad2d-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="aad2d-105">В этом руководстве вы развернете приложении SignalR для веб-роли Windows Azure, использование объединительной платы служебной шины для распределения сообщений для каждого экземпляра роли.</span><span class="sxs-lookup"><span data-stu-id="aad2d-105">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span> <span data-ttu-id="aad2d-106">(Можно также использовать задняя служебной шины с [веб-приложения в службе приложений Azure](https://docs.microsoft.com/azure/app-service-web/).)</span><span class="sxs-lookup"><span data-stu-id="aad2d-106">(You can also use the Service Bus backplane with [web apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="aad2d-107">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="aad2d-107">Prerequisites:</span></span>

- <span data-ttu-id="aad2d-108">Учетная запись Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="aad2d-108">A Windows Azure account.</span></span>
- <span data-ttu-id="aad2d-109">[Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="aad2d-109">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="aad2d-110">Visual Studio 2012 или 2013.</span><span class="sxs-lookup"><span data-stu-id="aad2d-110">Visual Studio 2012 or 2013.</span></span>

<span data-ttu-id="aad2d-111">На задней стороне шины службы также совместима с [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), версии 1.1.</span><span class="sxs-lookup"><span data-stu-id="aad2d-111">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="aad2d-112">Тем не менее не совместима с версией 1.0 Service Bus for Windows Server.</span><span class="sxs-lookup"><span data-stu-id="aad2d-112">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="aad2d-113">Цены</span><span class="sxs-lookup"><span data-stu-id="aad2d-113">Pricing</span></span>

<span data-ttu-id="aad2d-114">На задней стороне служебная шина использует разделы для отправки сообщений.</span><span class="sxs-lookup"><span data-stu-id="aad2d-114">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="aad2d-115">Сведения о последних ценах, см. в разделе [служебной шины](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="aad2d-115">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="aad2d-116">Во время написания данной статьи вы можете отправлять 1 000 000 сообщений в месяц для меньше, чем 1 долл. США.</span><span class="sxs-lookup"><span data-stu-id="aad2d-116">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="aad2d-117">Задняя панель отправляет сообщение служебной шины для каждого вызова метода концентратора SignalR.</span><span class="sxs-lookup"><span data-stu-id="aad2d-117">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="aad2d-118">Кроме того, существуют также некоторые сообщения управления для подключения, отключения, присоединяемый или создание групп и т. д.</span><span class="sxs-lookup"><span data-stu-id="aad2d-118">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="aad2d-119">В большинстве приложений большая часть трафика сообщений будет вызовы методов концентратора.</span><span class="sxs-lookup"><span data-stu-id="aad2d-119">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="aad2d-120">Обзор</span><span class="sxs-lookup"><span data-stu-id="aad2d-120">Overview</span></span>

<span data-ttu-id="aad2d-121">Прежде чем мы перейдем к подробный учебник, ниже приведен краткий обзор. ваши действия.</span><span class="sxs-lookup"><span data-stu-id="aad2d-121">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="aad2d-122">Создать новое пространство имен служебной шины с помощью портала Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="aad2d-122">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="aad2d-123">Добавьте следующие пакеты NuGet приложения:</span><span class="sxs-lookup"><span data-stu-id="aad2d-123">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="aad2d-124">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="aad2d-124">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - <span data-ttu-id="aad2d-125">[Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) или [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span><span class="sxs-lookup"><span data-stu-id="aad2d-125">[Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) or [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span></span>
3. <span data-ttu-id="aad2d-126">Создайте приложение SignalR.</span><span class="sxs-lookup"><span data-stu-id="aad2d-126">Create a SignalR application.</span></span>
4. <span data-ttu-id="aad2d-127">Добавьте следующий код в Startup.cs для настройки на задней стороне:</span><span class="sxs-lookup"><span data-stu-id="aad2d-127">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="aad2d-128">Этот код настраивает задней панели со значениями по умолчанию для [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) и [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="aad2d-128">This code configures the backplane with the default values for [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="aad2d-129">Дополнительные сведения об изменении этих значений см. в разделе [SignalR производительности: Метрики горизонтального масштабирования](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="aad2d-129">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span>

<span data-ttu-id="aad2d-130">Для каждого приложения выберите другое значение для «YourAppName».</span><span class="sxs-lookup"><span data-stu-id="aad2d-130">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="aad2d-131">Не используйте то же значение в нескольких приложениях.</span><span class="sxs-lookup"><span data-stu-id="aad2d-131">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="aad2d-132">Создание служб Azure</span><span class="sxs-lookup"><span data-stu-id="aad2d-132">Create the Azure Services</span></span>

<span data-ttu-id="aad2d-133">Создание облачной службы, как описано в разделе [как создать и развернуть облачную службу](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="aad2d-133">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="aad2d-134">Выполните действия, описанные в разделе «как: Создание облачной службы с помощью функции быстрого создания».</span><span class="sxs-lookup"><span data-stu-id="aad2d-134">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="aad2d-135">В этом руководстве вы не обязательно должны передать сертификат.</span><span class="sxs-lookup"><span data-stu-id="aad2d-135">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="aad2d-136">Создайте новое пространство имен служебной шины, как описано в [как для использования разделов и подписок служебной шины](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="aad2d-136">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="aad2d-137">Выполните действия, описанные в разделе «Создание имен службы».</span><span class="sxs-lookup"><span data-stu-id="aad2d-137">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="aad2d-138">Убедитесь в том, что выберите тот же регион для облачной службы и пространство имен служебной шины.</span><span class="sxs-lookup"><span data-stu-id="aad2d-138">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="aad2d-139">Создание проекта Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aad2d-139">Create the Visual Studio Project</span></span>

<span data-ttu-id="aad2d-140">Запустите Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="aad2d-140">Start Visual Studio.</span></span> <span data-ttu-id="aad2d-141">Из **файл** меню, щелкните **новый проект**.</span><span class="sxs-lookup"><span data-stu-id="aad2d-141">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="aad2d-142">В **новый проект** диалоговом окне разверните **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="aad2d-142">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="aad2d-143">В разделе **установленные шаблоны**выберите **Cloud** , а затем выберите **облачной службы Windows Azure**.</span><span class="sxs-lookup"><span data-stu-id="aad2d-143">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="aad2d-144">Оставьте по умолчанию .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="aad2d-144">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="aad2d-145">Присвойте имя приложению ChatService, а затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="aad2d-145">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="aad2d-146">В **новая облачная служба Windows Azure** диалоговое окно, выберите веб-роли ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="aad2d-146">In the **New Windows Azure Cloud Service** dialog, select ASP.NET Web Role.</span></span> <span data-ttu-id="aad2d-147">Нажмите кнопку со стрелкой вправо (**&gt;**) чтобы добавить роль в решение.</span><span class="sxs-lookup"><span data-stu-id="aad2d-147">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="aad2d-148">Наведите указатель мыши новую роль, поэтому отображается значок с изображением карандаша.</span><span class="sxs-lookup"><span data-stu-id="aad2d-148">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="aad2d-149">Щелкните этот значок, чтобы переименовать роль.</span><span class="sxs-lookup"><span data-stu-id="aad2d-149">Click this icon to rename the role.</span></span> <span data-ttu-id="aad2d-150">Имя роли «SignalRChat» и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="aad2d-150">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="aad2d-151">В **новый проект ASP.NET** диалоговом окне выберите **MVC**и нажмите кнопку ОК.</span><span class="sxs-lookup"><span data-stu-id="aad2d-151">In the **New ASP.NET Project** dialog, select **MVC**, and click OK.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="aad2d-152">Мастер проекта создает два проекта:</span><span class="sxs-lookup"><span data-stu-id="aad2d-152">The project wizard creates two projects:</span></span>

- <span data-ttu-id="aad2d-153">ChatService: Этот проект является приложением Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="aad2d-153">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="aad2d-154">Он определяет роли Azure и других параметров конфигурации.</span><span class="sxs-lookup"><span data-stu-id="aad2d-154">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="aad2d-155">SignalRChat: Этот проект является проекта ASP.NET MVC 5.</span><span class="sxs-lookup"><span data-stu-id="aad2d-155">SignalRChat: This project is your ASP.NET MVC 5 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="aad2d-156">Создание приложения чата SignalR</span><span class="sxs-lookup"><span data-stu-id="aad2d-156">Create the SignalR Chat Application</span></span>

<span data-ttu-id="aad2d-157">Чтобы создать приложение чата, следуйте указаниям в этом руководстве [начало работы с SignalR и MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="aad2d-157">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="aad2d-158">Используйте NuGet для установки необходимых библиотек.</span><span class="sxs-lookup"><span data-stu-id="aad2d-158">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="aad2d-159">Из **средства** меню, выберите **диспетчер пакетов NuGet**, а затем выберите **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="aad2d-159">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="aad2d-160">В **консоль диспетчера пакетов** окно, введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="aad2d-160">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="aad2d-161">Используйте `-ProjectName` параметр для установки пакетов проекта ASP.NET MVC, а не проекта Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="aad2d-161">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="aad2d-162">Настройка задней панели</span><span class="sxs-lookup"><span data-stu-id="aad2d-162">Configure the Backplane</span></span>

<span data-ttu-id="aad2d-163">В файле Startup.cs приложения добавьте следующий код:</span><span class="sxs-lookup"><span data-stu-id="aad2d-163">In your application's Startup.cs file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="aad2d-164">Теперь необходимо получить строку подключения service bus.</span><span class="sxs-lookup"><span data-stu-id="aad2d-164">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="aad2d-165">На портале Azure выберите пространство имен служебной шины, который вы создали и щелкните значок ключа доступа.</span><span class="sxs-lookup"><span data-stu-id="aad2d-165">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

<span data-ttu-id="aad2d-166">Скопируйте строку подключения в буфер обмена, а затем вставьте его в *connectionString* переменной.</span><span class="sxs-lookup"><span data-stu-id="aad2d-166">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="aad2d-167">Развертывание в Azure</span><span class="sxs-lookup"><span data-stu-id="aad2d-167">Deploy to Azure</span></span>

<span data-ttu-id="aad2d-168">В обозревателе решений разверните **ролей** папку внутри проекта ChatService.</span><span class="sxs-lookup"><span data-stu-id="aad2d-168">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="aad2d-169">Щелкните правой кнопкой мыши роль SignalRChat и выберите **свойства**.</span><span class="sxs-lookup"><span data-stu-id="aad2d-169">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="aad2d-170">Выберите **конфигурации** вкладки. В разделе **экземпляров** выберите 2.</span><span class="sxs-lookup"><span data-stu-id="aad2d-170">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="aad2d-171">Можно также задать размер виртуальной Машины **очень мелкий**.</span><span class="sxs-lookup"><span data-stu-id="aad2d-171">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="aad2d-172">Сохраните изменения.</span><span class="sxs-lookup"><span data-stu-id="aad2d-172">Save the changes.</span></span>

<span data-ttu-id="aad2d-173">В обозревателе решений щелкните правой кнопкой мыши проект ChatService.</span><span class="sxs-lookup"><span data-stu-id="aad2d-173">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="aad2d-174">Нажмите **Публиковать**.</span><span class="sxs-lookup"><span data-stu-id="aad2d-174">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="aad2d-175">Если это ваш первый время публикации в Windows Azure, необходимо загрузить учетные данные.</span><span class="sxs-lookup"><span data-stu-id="aad2d-175">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="aad2d-176">В **публикации** мастера, нажмите кнопку «Вход для загрузки учетных данных».</span><span class="sxs-lookup"><span data-stu-id="aad2d-176">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="aad2d-177">Это предложит Войдите на портал Windows Azure и загрузите файл параметров публикации.</span><span class="sxs-lookup"><span data-stu-id="aad2d-177">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="aad2d-178">Нажмите кнопку **импорта** и выберите загруженный файл параметров публикации.</span><span class="sxs-lookup"><span data-stu-id="aad2d-178">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="aad2d-179">Нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="aad2d-179">Click **Next**.</span></span> <span data-ttu-id="aad2d-180">В **параметры публикации** диалогового окна в разделе **облачной службы**, выберите облачную службу, созданную ранее.</span><span class="sxs-lookup"><span data-stu-id="aad2d-180">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="aad2d-181">Нажмите кнопку **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="aad2d-181">Click **Publish**.</span></span> <span data-ttu-id="aad2d-182">Может занять несколько минут, чтобы развернуть приложение и запустить виртуальные машины.</span><span class="sxs-lookup"><span data-stu-id="aad2d-182">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="aad2d-183">Теперь при запуске приложения чата, экземпляры ролей, взаимодействуют через служебную шину Azure, используя раздел служебной шины.</span><span class="sxs-lookup"><span data-stu-id="aad2d-183">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="aad2d-184">Тема — это очередь сообщений, которая позволяет нескольким подписчикам.</span><span class="sxs-lookup"><span data-stu-id="aad2d-184">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="aad2d-185">Задняя панель автоматически создает раздела и подписки.</span><span class="sxs-lookup"><span data-stu-id="aad2d-185">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="aad2d-186">Чтобы увидеть подписки и действие сообщения, откройте портал Azure, выберите пространство имен служебной шины и щелкните «Разделы».</span><span class="sxs-lookup"><span data-stu-id="aad2d-186">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="aad2d-187">Берет на себя занять несколько минут для действия сообщения для отображения на панели мониторинга.</span><span class="sxs-lookup"><span data-stu-id="aad2d-187">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

<span data-ttu-id="aad2d-188">SignalR управляет временем существования раздела.</span><span class="sxs-lookup"><span data-stu-id="aad2d-188">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="aad2d-189">До тех пор, пока развертывается приложение, не следует пытаться вручную удалить разделы или изменить параметры в разделе.</span><span class="sxs-lookup"><span data-stu-id="aad2d-189">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="aad2d-190">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="aad2d-190">Troubleshooting</span></span>

<span data-ttu-id="aad2d-191">**System.InvalidOperationException «Только поддерживаемые IsolationLevel — «IsolationLevel.Serializable».»**</span><span class="sxs-lookup"><span data-stu-id="aad2d-191">**System.InvalidOperationException "The only supported IsolationLevel is 'IsolationLevel.Serializable'."**</span></span>

<span data-ttu-id="aad2d-192">Эта ошибка может возникать, если другой уровень транзакции для операции `Serializable`.</span><span class="sxs-lookup"><span data-stu-id="aad2d-192">This error can occur if the transaction level for an operation is set to something other than `Serializable`.</span></span> <span data-ttu-id="aad2d-193">Убедитесь, что никакие операции выполняются с другие уровни транзакций.</span><span class="sxs-lookup"><span data-stu-id="aad2d-193">Verify that no operations are being performed with other transaction levels.</span></span>
