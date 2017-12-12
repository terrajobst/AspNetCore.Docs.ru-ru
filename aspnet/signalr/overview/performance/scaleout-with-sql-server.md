---
uid: signalr/overview/performance/scaleout-with-sql-server
title: "SignalR горизонтального масштабирования с SQL Server | Документы Microsoft"
author: MikeWasson
description: "Версии программного обеспечения используется в этом разделе Visual Studio 2013 .NET 4.5 SignalR предыдущие версии версии 2 в этом разделе сведения о предыдущих версиях..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 5bf625a1ef8cc8ceab0014fadfab0c8a23dbc8da
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="signalr-scaleout-with-sql-server"></a><span data-ttu-id="4da20-103">SignalR горизонтального масштабирования с SQL Server</span><span class="sxs-lookup"><span data-stu-id="4da20-103">SignalR Scaleout with SQL Server</span></span>
====================
<span data-ttu-id="4da20-104">по [Mike Wasson](https://github.com/MikeWasson), [Патрик Флетчера](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="4da20-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="4da20-105">Версии программного обеспечения, используемого в этом разделе</span><span class="sxs-lookup"><span data-stu-id="4da20-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="4da20-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="4da20-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="4da20-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="4da20-107">.NET 4.5</span></span>
> - <span data-ttu-id="4da20-108">SignalR версии 2</span><span class="sxs-lookup"><span data-stu-id="4da20-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="4da20-109">Предыдущие версии этого раздела</span><span class="sxs-lookup"><span data-stu-id="4da20-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="4da20-110">Сведения о более ранних версий SignalR см. в разделе [более ранних версий SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="4da20-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="4da20-111">Вопросы и комментарии</span><span class="sxs-lookup"><span data-stu-id="4da20-111">Questions and comments</span></span>
> 
> <span data-ttu-id="4da20-112">Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить в комментарии в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="4da20-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="4da20-113">Если у вас есть вопросы, которые не связаны непосредственно для работы с учебником, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="4da20-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="4da20-114">В этом учебнике будет использоваться SQL Server для распределения сообщений между SignalR приложения, которое развертывается в двух разных экземплярах служб IIS.</span><span class="sxs-lookup"><span data-stu-id="4da20-114">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="4da20-115">Этот учебник можно также запустить на одном тестовом компьютере, но чтобы получить результат, необходимо развернуть приложение SignalR на несколько серверов.</span><span class="sxs-lookup"><span data-stu-id="4da20-115">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="4da20-116">Также необходимо установить SQL Server на одном из серверов или на отдельном выделенном сервере.</span><span class="sxs-lookup"><span data-stu-id="4da20-116">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="4da20-117">Другой вариант — для работы с учебником, с использованием виртуальных машин в Azure.</span><span class="sxs-lookup"><span data-stu-id="4da20-117">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="4da20-118">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="4da20-118">Prerequisites</span></span>

<span data-ttu-id="4da20-119">Microsoft SQL Server 2005 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="4da20-119">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="4da20-120">Задняя панель поддерживает выпуски настольных компьютеров и серверов SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4da20-120">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="4da20-121">Он не поддерживает SQL Server Compact Edition или базы данных SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="4da20-121">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="4da20-122">(Если приложение размещено в Azure, рекомендуется на задней стороне служебной шины вместо.)</span><span class="sxs-lookup"><span data-stu-id="4da20-122">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="4da20-123">Обзор</span><span class="sxs-lookup"><span data-stu-id="4da20-123">Overview</span></span>

<span data-ttu-id="4da20-124">Прежде чем мы приступим к подробный учебник, ниже приведен краткий обзор того, что сделать.</span><span class="sxs-lookup"><span data-stu-id="4da20-124">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="4da20-125">Создайте новую пустую базу данных.</span><span class="sxs-lookup"><span data-stu-id="4da20-125">Create a new empty database.</span></span> <span data-ttu-id="4da20-126">Объединительной плате будет создать необходимые таблицы в этой базе данных.</span><span class="sxs-lookup"><span data-stu-id="4da20-126">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="4da20-127">Добавьте эти пакеты NuGet для приложения:</span><span class="sxs-lookup"><span data-stu-id="4da20-127">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="4da20-128">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="4da20-128">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="4da20-129">Microsoft.AspNet.SignalR.SqlServer</span><span class="sxs-lookup"><span data-stu-id="4da20-129">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="4da20-130">Создайте приложение SignalR.</span><span class="sxs-lookup"><span data-stu-id="4da20-130">Create a SignalR application.</span></span>
4. <span data-ttu-id="4da20-131">Добавьте следующий код в файле Startup.cs для настройки на задней стороне:</span><span class="sxs-lookup"><span data-stu-id="4da20-131">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

 <span data-ttu-id="4da20-132">Этот код настраивает объединительной плате со значениями по умолчанию для [TableCount](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) и [MaxQueueLength](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="4da20-132">This code configures the backplane with the default values for [TableCount](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="4da20-133">Сведения об изменении этих значений см. в разделе [производительности SignalR: метрики горизонтального масштабирования](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="4da20-133">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span> 

## <a name="configure-the-database"></a><span data-ttu-id="4da20-134">Настройка базы данных</span><span class="sxs-lookup"><span data-stu-id="4da20-134">Configure the Database</span></span>

<span data-ttu-id="4da20-135">Решите, будет ли приложение использовать проверку подлинности Windows или проверка подлинности SQL Server для доступа к базе данных.</span><span class="sxs-lookup"><span data-stu-id="4da20-135">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="4da20-136">Независимо от того убедитесь, что пользователь базы данных имеет разрешения на вход, создании схем и таблиц.</span><span class="sxs-lookup"><span data-stu-id="4da20-136">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="4da20-137">Создайте новую базу данных для использования объединительной платы.</span><span class="sxs-lookup"><span data-stu-id="4da20-137">Create a new database for the backplane to use.</span></span> <span data-ttu-id="4da20-138">Можно присвоить любое имя базы данных.</span><span class="sxs-lookup"><span data-stu-id="4da20-138">You can give the database any name.</span></span> <span data-ttu-id="4da20-139">Не нужно создавать любой таблицы в базе данных. Задняя панель будет создать необходимые таблицы.</span><span class="sxs-lookup"><span data-stu-id="4da20-139">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="4da20-140">Включение компонента Service Broker</span><span class="sxs-lookup"><span data-stu-id="4da20-140">Enable Service Broker</span></span>

<span data-ttu-id="4da20-141">Рекомендуется включить компонент Service Broker для базы данных объединительной платы.</span><span class="sxs-lookup"><span data-stu-id="4da20-141">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="4da20-142">Компонент Service Broker обеспечивает собственную поддержку для обмена сообщениями и очередей в SQL Server, что позволяет более эффективно получать обновления объединительной плате.</span><span class="sxs-lookup"><span data-stu-id="4da20-142">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="4da20-143">(Однако объединительной платы также работает без компонента Service Broker).</span><span class="sxs-lookup"><span data-stu-id="4da20-143">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="4da20-144">Чтобы проверить, включен ли компонент Service Broker, запрос **—\_broker\_включен** столбца в **sys.databases** представления каталога.</span><span class="sxs-lookup"><span data-stu-id="4da20-144">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="4da20-145">Чтобы включить компонент Service Broker, используйте следующий SQL-запрос:</span><span class="sxs-lookup"><span data-stu-id="4da20-145">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="4da20-146">Если взаимоблокировка, убедитесь, что этот запрос нет приложений, подключенных к базе данных.</span><span class="sxs-lookup"><span data-stu-id="4da20-146">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>


<span data-ttu-id="4da20-147">Если трассировка включена, данные трассировки также будет показано, включен ли компонент Service Broker.</span><span class="sxs-lookup"><span data-stu-id="4da20-147">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="4da20-148">Создание приложения SignalR</span><span class="sxs-lookup"><span data-stu-id="4da20-148">Create a SignalR Application</span></span>

<span data-ttu-id="4da20-149">Создайте приложение SignalR, выполните одно из этих учебников.</span><span class="sxs-lookup"><span data-stu-id="4da20-149">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="4da20-150">Приступая к работе с SignalR 2.0</span><span class="sxs-lookup"><span data-stu-id="4da20-150">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="4da20-151">Приступая к работе с SignalR 2.0 и MVC 5</span><span class="sxs-lookup"><span data-stu-id="4da20-151">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="4da20-152">Затем мы изменим приложения разговора для поддержки горизонтального масштабирования с SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4da20-152">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="4da20-153">Сначала добавьте пакет SignalR.SqlServer NuGet в проект.</span><span class="sxs-lookup"><span data-stu-id="4da20-153">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="4da20-154">В Visual Studio из **средства** последовательно выберите пункты **диспетчер пакетов библиотеки**, а затем выберите **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="4da20-154">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="4da20-155">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="4da20-155">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="4da20-156">Затем откройте файл файле Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="4da20-156">Next, open the Startup.cs file.</span></span> <span data-ttu-id="4da20-157">Добавьте следующий код в **Настройка** метод:</span><span class="sxs-lookup"><span data-stu-id="4da20-157">Add the following code to the **Configure** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="4da20-158">Развертывание и запуск приложения</span><span class="sxs-lookup"><span data-stu-id="4da20-158">Deploy and Run the Application</span></span>

<span data-ttu-id="4da20-159">Подготовьте экземпляры сервера Windows для развертывания приложения SignalR.</span><span class="sxs-lookup"><span data-stu-id="4da20-159">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="4da20-160">Добавьте роль IIS.</span><span class="sxs-lookup"><span data-stu-id="4da20-160">Add the IIS role.</span></span> <span data-ttu-id="4da20-161">Включить функции «Разработка приложений», включая протокол WebSocket.</span><span class="sxs-lookup"><span data-stu-id="4da20-161">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="4da20-162">Также можно включить службу управления (в области «Средства управления»).</span><span class="sxs-lookup"><span data-stu-id="4da20-162">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="4da20-163">**Установка веб-развертывание 3.0.**</span><span class="sxs-lookup"><span data-stu-id="4da20-163">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="4da20-164">При выполнении диспетчера служб IIS, система предложит установить веб-платформы Майкрософт или вы можете [загрузки intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="4da20-164">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="4da20-165">В установщике платформы поиск веб-развертывания и установка Web Deploy 3.0</span><span class="sxs-lookup"><span data-stu-id="4da20-165">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="4da20-166">Проверьте, что запущена служба управления.</span><span class="sxs-lookup"><span data-stu-id="4da20-166">Check that the Web Management Service is running.</span></span> <span data-ttu-id="4da20-167">В противном случае запустите службу.</span><span class="sxs-lookup"><span data-stu-id="4da20-167">If not, start the service.</span></span> <span data-ttu-id="4da20-168">(Если вы не видите управления веб-службы в списке служб Windows, убедитесь, что установлена служба управления, при добавлении роли IIS.)</span><span class="sxs-lookup"><span data-stu-id="4da20-168">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="4da20-169">Наконец необходимо откройте порт 8172 протокол TCP.</span><span class="sxs-lookup"><span data-stu-id="4da20-169">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="4da20-170">Это порт, который используется средством веб-развертывания.</span><span class="sxs-lookup"><span data-stu-id="4da20-170">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="4da20-171">Теперь вы готовы к развертыванию проекта Visual Studio с компьютера разработки на сервер.</span><span class="sxs-lookup"><span data-stu-id="4da20-171">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="4da20-172">В обозревателе решений щелкните правой кнопкой мыши решение и выберите **публикации**.</span><span class="sxs-lookup"><span data-stu-id="4da20-172">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="4da20-173">Подробную документацию по веб-развертывания см. в разделе [Web Карта содержимого развертывания для Visual Studio и ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="4da20-173">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="4da20-174">При развертывании приложения на двух серверах, можно открыть каждый экземпляр в отдельном окне браузера и в разделе, чтобы они могли получать сообщений SignalR от другого.</span><span class="sxs-lookup"><span data-stu-id="4da20-174">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="4da20-175">(Конечно, в рабочей среде, два сервера будет разместить в подсистему балансировки нагрузки.)</span><span class="sxs-lookup"><span data-stu-id="4da20-175">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="4da20-176">После запуска приложения, можно увидеть, что SignalR автоматически создал таблиц в базе данных:</span><span class="sxs-lookup"><span data-stu-id="4da20-176">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="4da20-177">SignalR управляет таблиц.</span><span class="sxs-lookup"><span data-stu-id="4da20-177">SignalR manages the tables.</span></span> <span data-ttu-id="4da20-178">При условии, что приложение развертывается, не удалять строки, изменить таблицу и пр.</span><span class="sxs-lookup"><span data-stu-id="4da20-178">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
