---
uid: signalr/overview/performance/scaleout-with-sql-server
title: Масштабирование SignalR с помощью SQL Server | Документация Майкрософт
author: MikeWasson
description: Версии программного обеспечения используется в этом разделе 4.5 .NET SignalR для Visual Studio 2013 версии 2, предыдущие версии в этом разделе сведения о более ранних версиях...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 7d9acfc8ed2dd692f36528a8a9700d92d86a4203
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910451"
---
<a name="signalr-scaleout-with-sql-server"></a><span data-ttu-id="e2a26-103">Масштабирование SignalR с помощью SQL Server</span><span class="sxs-lookup"><span data-stu-id="e2a26-103">SignalR Scaleout with SQL Server</span></span>
====================
<span data-ttu-id="e2a26-104">по [Майк Уоссон](https://github.com/MikeWasson), [Патрик Флетчера](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="e2a26-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="e2a26-105">Версии программного обеспечения, используемого в этом разделе</span><span class="sxs-lookup"><span data-stu-id="e2a26-105">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="e2a26-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="e2a26-106">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="e2a26-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="e2a26-107">.NET 4.5</span></span>
> - <span data-ttu-id="e2a26-108">SignalR версии 2</span><span class="sxs-lookup"><span data-stu-id="e2a26-108">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="e2a26-109">Предыдущие версии этого раздела</span><span class="sxs-lookup"><span data-stu-id="e2a26-109">Previous versions of this topic</span></span>
>
> <span data-ttu-id="e2a26-110">Сведения о более ранних версий SignalR, см. в разделе [более старых версий SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="e2a26-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="e2a26-111">Вопросы и комментарии</span><span class="sxs-lookup"><span data-stu-id="e2a26-111">Questions and comments</span></span>
>
> <span data-ttu-id="e2a26-112">Оставьте свои отзывы на том, как вам понравилось, и этот учебник и что можно улучшить в комментариях в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="e2a26-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="e2a26-113">Если у вас есть вопросы, которые не имеют отношения к руководству, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="e2a26-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="e2a26-114">В этом руководстве будет использовать SQL Server, чтобы распределить сообщения в приложении SignalR, которое развертывается в два отдельных экземпляра IIS.</span><span class="sxs-lookup"><span data-stu-id="e2a26-114">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="e2a26-115">Можно также открыть этот пример на одном тестовом компьютере, но чтобы получить полные результаты, необходимо выполнить развертывание приложений SignalR на два или несколько серверов.</span><span class="sxs-lookup"><span data-stu-id="e2a26-115">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="e2a26-116">Также необходимо установить SQL Server на одном из серверов или на отдельном выделенном сервере.</span><span class="sxs-lookup"><span data-stu-id="e2a26-116">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="e2a26-117">Другой вариант — для работы с учебником, с помощью виртуальных машин в Azure.</span><span class="sxs-lookup"><span data-stu-id="e2a26-117">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="e2a26-118">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="e2a26-118">Prerequisites</span></span>

<span data-ttu-id="e2a26-119">Microsoft SQL Server 2005 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="e2a26-119">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="e2a26-120">Задняя панель поддерживает настольных и серверных выпусков SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e2a26-120">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="e2a26-121">Он не поддерживает SQL Server Compact Edition или базы данных SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="e2a26-121">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="e2a26-122">(Если приложение размещено в Azure, рекомендуется использовать на задней стороне служебной шины.)</span><span class="sxs-lookup"><span data-stu-id="e2a26-122">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="e2a26-123">Обзор</span><span class="sxs-lookup"><span data-stu-id="e2a26-123">Overview</span></span>

<span data-ttu-id="e2a26-124">Прежде чем мы перейдем к подробный учебник, ниже приведен краткий обзор. ваши действия.</span><span class="sxs-lookup"><span data-stu-id="e2a26-124">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="e2a26-125">Создание новой пустой базы данных.</span><span class="sxs-lookup"><span data-stu-id="e2a26-125">Create a new empty database.</span></span> <span data-ttu-id="e2a26-126">Задняя панель создаст необходимые таблицы в этой базе данных.</span><span class="sxs-lookup"><span data-stu-id="e2a26-126">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="e2a26-127">Добавьте следующие пакеты NuGet приложения:</span><span class="sxs-lookup"><span data-stu-id="e2a26-127">Add these NuGet packages to your application:</span></span>

    - [<span data-ttu-id="e2a26-128">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="e2a26-128">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="e2a26-129">Microsoft.AspNet.SignalR.SqlServer</span><span class="sxs-lookup"><span data-stu-id="e2a26-129">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="e2a26-130">Создайте приложение SignalR.</span><span class="sxs-lookup"><span data-stu-id="e2a26-130">Create a SignalR application.</span></span>
4. <span data-ttu-id="e2a26-131">Добавьте следующий код в Startup.cs для настройки на задней стороне:</span><span class="sxs-lookup"><span data-stu-id="e2a26-131">Add the following code to Startup.cs to configure the backplane:</span></span>

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

   <span data-ttu-id="e2a26-132">Этот код настраивает задней панели со значениями по умолчанию для [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) и [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="e2a26-132">This code configures the backplane with the default values for [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="e2a26-133">Дополнительные сведения об изменении этих значений см. в разделе [производительность SignalR: метрики горизонтального масштабирования](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="e2a26-133">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span>

## <a name="configure-the-database"></a><span data-ttu-id="e2a26-134">Настройка базы данных</span><span class="sxs-lookup"><span data-stu-id="e2a26-134">Configure the Database</span></span>

<span data-ttu-id="e2a26-135">Решите, будет ли приложение использовать проверку подлинности Windows или проверка подлинности SQL Server для доступа к базе данных.</span><span class="sxs-lookup"><span data-stu-id="e2a26-135">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="e2a26-136">В любом случае убедитесь, что пользователь базы данных имеет разрешения на вход, создавать схемы и создание таблиц.</span><span class="sxs-lookup"><span data-stu-id="e2a26-136">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="e2a26-137">Создайте новую базу данных для использования объединительной платы.</span><span class="sxs-lookup"><span data-stu-id="e2a26-137">Create a new database for the backplane to use.</span></span> <span data-ttu-id="e2a26-138">Можно выбрать любое имя базы данных.</span><span class="sxs-lookup"><span data-stu-id="e2a26-138">You can give the database any name.</span></span> <span data-ttu-id="e2a26-139">Не нужно создавать любой таблицы в базе данных; Задняя панель создаст необходимые таблицы.</span><span class="sxs-lookup"><span data-stu-id="e2a26-139">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="e2a26-140">Включение компонента Service Broker</span><span class="sxs-lookup"><span data-stu-id="e2a26-140">Enable Service Broker</span></span>

<span data-ttu-id="e2a26-141">Рекомендуется, чтобы включить компонент Service Broker для базы данных объединительной платы.</span><span class="sxs-lookup"><span data-stu-id="e2a26-141">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="e2a26-142">Компонент Service Broker обеспечивает встроенную поддержку для обмена сообщениями и очереди в SQL Server, что позволяет более эффективно получать обновления задней панели.</span><span class="sxs-lookup"><span data-stu-id="e2a26-142">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="e2a26-143">(Однако задней панели также работает без компонента Service Broker).</span><span class="sxs-lookup"><span data-stu-id="e2a26-143">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="e2a26-144">Чтобы проверить, включен ли компонент Service Broker, запросите **—\_broker\_включена** столбца в **sys.databases** представления каталога.</span><span class="sxs-lookup"><span data-stu-id="e2a26-144">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="e2a26-145">Чтобы включить компонент Service Broker, используйте следующий запрос SQL:</span><span class="sxs-lookup"><span data-stu-id="e2a26-145">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="e2a26-146">Если этот запрос принимать участие во взаимоблокировке, убедитесь, что нет подключения к базе данных приложений.</span><span class="sxs-lookup"><span data-stu-id="e2a26-146">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>


<span data-ttu-id="e2a26-147">Если трассировка включена, данные трассировки также будет показано, включен ли компонент Service Broker.</span><span class="sxs-lookup"><span data-stu-id="e2a26-147">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="e2a26-148">Создание приложения SignalR</span><span class="sxs-lookup"><span data-stu-id="e2a26-148">Create a SignalR Application</span></span>

<span data-ttu-id="e2a26-149">Создайте приложение SignalR, выполнив одно из следующих руководств:</span><span class="sxs-lookup"><span data-stu-id="e2a26-149">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="e2a26-150">Начало работы с SignalR 2.0</span><span class="sxs-lookup"><span data-stu-id="e2a26-150">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="e2a26-151">Начало работы с SignalR 2.0 и MVC 5</span><span class="sxs-lookup"><span data-stu-id="e2a26-151">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="e2a26-152">Затем мы изменим приложение чата для поддержки горизонтального масштабирования с помощью SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e2a26-152">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="e2a26-153">Во-первых добавьте пакет SignalR.SqlServer NuGet в проект.</span><span class="sxs-lookup"><span data-stu-id="e2a26-153">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="e2a26-154">В Visual Studio из **средства** меню, выберите **диспетчер пакетов NuGet**, а затем выберите **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="e2a26-154">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="e2a26-155">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="e2a26-155">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="e2a26-156">Затем откройте файл Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="e2a26-156">Next, open the Startup.cs file.</span></span> <span data-ttu-id="e2a26-157">Добавьте следующий код, чтобы **Настройка** метод:</span><span class="sxs-lookup"><span data-stu-id="e2a26-157">Add the following code to the **Configure** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="e2a26-158">Развертывание и запуск приложения</span><span class="sxs-lookup"><span data-stu-id="e2a26-158">Deploy and Run the Application</span></span>

<span data-ttu-id="e2a26-159">Подготовка экземпляров Windows Server для развертывания приложения SignalR.</span><span class="sxs-lookup"><span data-stu-id="e2a26-159">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="e2a26-160">Добавьте роль IIS.</span><span class="sxs-lookup"><span data-stu-id="e2a26-160">Add the IIS role.</span></span> <span data-ttu-id="e2a26-161">Включить функции «Разработка приложений», включая протокол WebSocket.</span><span class="sxs-lookup"><span data-stu-id="e2a26-161">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="e2a26-162">Также службы управления (см. в разделе «Средства управления»).</span><span class="sxs-lookup"><span data-stu-id="e2a26-162">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="e2a26-163">**Установить Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="e2a26-163">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="e2a26-164">Если запустить диспетчер служб IIS, он предложит вам установить веб-платформы Майкрософт или вы можете [загрузить intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="e2a26-164">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="e2a26-165">В установщике платформы найдите веб-развертывания и установить Web Deploy 3.0</span><span class="sxs-lookup"><span data-stu-id="e2a26-165">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="e2a26-166">Убедитесь, что веб-службы управления.</span><span class="sxs-lookup"><span data-stu-id="e2a26-166">Check that the Web Management Service is running.</span></span> <span data-ttu-id="e2a26-167">В противном случае запустите службу.</span><span class="sxs-lookup"><span data-stu-id="e2a26-167">If not, start the service.</span></span> <span data-ttu-id="e2a26-168">(Если вы не видите веб-службы управления в списке служб Windows, убедитесь, что вы установили службу управления, при добавлении роли IIS.)</span><span class="sxs-lookup"><span data-stu-id="e2a26-168">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="e2a26-169">Наконец откройте порт 8172 для TCP.</span><span class="sxs-lookup"><span data-stu-id="e2a26-169">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="e2a26-170">Это порт, который используется средством веб-развертывания.</span><span class="sxs-lookup"><span data-stu-id="e2a26-170">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="e2a26-171">Теперь все готово для развертывания проекта Visual Studio с компьютера разработки на сервер.</span><span class="sxs-lookup"><span data-stu-id="e2a26-171">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="e2a26-172">В обозревателе решений щелкните правой кнопкой мыши решение и нажмите кнопку **публикации**.</span><span class="sxs-lookup"><span data-stu-id="e2a26-172">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="e2a26-173">Дополнительные сведения о веб-развертывания, см. в разделе [веб-развертывания содержимого карты для Visual Studio и ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="e2a26-173">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="e2a26-174">При развертывании приложения на двух серверах, можно открыть каждый экземпляр в отдельном окне браузера и см. в разделе, что каждый из них сообщения SignalR от другого.</span><span class="sxs-lookup"><span data-stu-id="e2a26-174">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="e2a26-175">(Конечно, в рабочей среде, два сервера бы сидеть балансировщиком нагрузки.)</span><span class="sxs-lookup"><span data-stu-id="e2a26-175">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="e2a26-176">После запуска приложения, вы увидите, что SignalR автоматически создала таблиц в базе данных:</span><span class="sxs-lookup"><span data-stu-id="e2a26-176">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="e2a26-177">SignalR управляет таблицами.</span><span class="sxs-lookup"><span data-stu-id="e2a26-177">SignalR manages the tables.</span></span> <span data-ttu-id="e2a26-178">До тех пор, пока развертывается приложение, не удалять строки, изменить таблицу и т. д.</span><span class="sxs-lookup"><span data-stu-id="e2a26-178">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
