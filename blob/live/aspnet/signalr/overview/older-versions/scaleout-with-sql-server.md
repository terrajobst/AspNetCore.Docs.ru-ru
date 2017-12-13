---
uid: signalr/overview/older-versions/scaleout-with-sql-server
title: "SignalR горизонтального масштабирования с SQL Server (SignalR 1.x) | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 1dca7967-8296-444a-9533-837eb284e78c
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 7a589f5c6a5196444c3c616b39f501f3a7512471
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="signalr-scaleout-with-sql-server-signalr-1x"></a><span data-ttu-id="3b025-102">SignalR горизонтального масштабирования с SQL Server (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="3b025-102">SignalR Scaleout with SQL Server (SignalR 1.x)</span></span>
====================
<span data-ttu-id="3b025-103">по [Mike Wasson](https://github.com/MikeWasson), [Патрик Флетчера](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="3b025-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="3b025-104">В этом учебнике будет использоваться SQL Server для распределения сообщений между SignalR приложения, которое развертывается в двух разных экземплярах служб IIS.</span><span class="sxs-lookup"><span data-stu-id="3b025-104">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="3b025-105">Этот учебник можно также запустить на одном тестовом компьютере, но чтобы получить результат, необходимо развернуть приложение SignalR на несколько серверов.</span><span class="sxs-lookup"><span data-stu-id="3b025-105">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="3b025-106">Также необходимо установить SQL Server на одном из серверов или на отдельном выделенном сервере.</span><span class="sxs-lookup"><span data-stu-id="3b025-106">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="3b025-107">Другой вариант — для работы с учебником, с использованием виртуальных машин в Azure.</span><span class="sxs-lookup"><span data-stu-id="3b025-107">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="3b025-108">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="3b025-108">Prerequisites</span></span>

<span data-ttu-id="3b025-109">Microsoft SQL Server 2005 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="3b025-109">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="3b025-110">Задняя панель поддерживает выпуски настольных компьютеров и серверов SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3b025-110">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="3b025-111">Он не поддерживает SQL Server Compact Edition или базы данных SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="3b025-111">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="3b025-112">(Если приложение размещено в Azure, рекомендуется на задней стороне служебной шины вместо.)</span><span class="sxs-lookup"><span data-stu-id="3b025-112">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="3b025-113">Обзор</span><span class="sxs-lookup"><span data-stu-id="3b025-113">Overview</span></span>

<span data-ttu-id="3b025-114">Прежде чем мы приступим к подробный учебник, ниже приведен краткий обзор того, что сделать.</span><span class="sxs-lookup"><span data-stu-id="3b025-114">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="3b025-115">Создайте новую пустую базу данных.</span><span class="sxs-lookup"><span data-stu-id="3b025-115">Create a new empty database.</span></span> <span data-ttu-id="3b025-116">Объединительной плате будет создать необходимые таблицы в этой базе данных.</span><span class="sxs-lookup"><span data-stu-id="3b025-116">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="3b025-117">Добавьте эти пакеты NuGet для приложения:</span><span class="sxs-lookup"><span data-stu-id="3b025-117">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="3b025-118">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="3b025-118">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="3b025-119">Microsoft.AspNet.SignalR.SqlServer</span><span class="sxs-lookup"><span data-stu-id="3b025-119">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="3b025-120">Создайте приложение SignalR.</span><span class="sxs-lookup"><span data-stu-id="3b025-120">Create a SignalR application.</span></span>
4. <span data-ttu-id="3b025-121">Добавьте следующий код Global.asax для настройки на задней стороне:</span><span class="sxs-lookup"><span data-stu-id="3b025-121">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

## <a name="configure-the-database"></a><span data-ttu-id="3b025-122">Настройка базы данных</span><span class="sxs-lookup"><span data-stu-id="3b025-122">Configure the Database</span></span>

<span data-ttu-id="3b025-123">Решите, будет ли приложение использовать проверку подлинности Windows или проверка подлинности SQL Server для доступа к базе данных.</span><span class="sxs-lookup"><span data-stu-id="3b025-123">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="3b025-124">Независимо от того убедитесь, что пользователь базы данных имеет разрешения на вход, создании схем и таблиц.</span><span class="sxs-lookup"><span data-stu-id="3b025-124">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="3b025-125">Создайте новую базу данных для использования объединительной платы.</span><span class="sxs-lookup"><span data-stu-id="3b025-125">Create a new database for the backplane to use.</span></span> <span data-ttu-id="3b025-126">Можно присвоить любое имя базы данных.</span><span class="sxs-lookup"><span data-stu-id="3b025-126">You can give the database any name.</span></span> <span data-ttu-id="3b025-127">Не нужно создавать любой таблицы в базе данных. Задняя панель будет создать необходимые таблицы.</span><span class="sxs-lookup"><span data-stu-id="3b025-127">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="3b025-128">Включение компонента Service Broker</span><span class="sxs-lookup"><span data-stu-id="3b025-128">Enable Service Broker</span></span>

<span data-ttu-id="3b025-129">Рекомендуется включить компонент Service Broker для базы данных объединительной платы.</span><span class="sxs-lookup"><span data-stu-id="3b025-129">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="3b025-130">Компонент Service Broker обеспечивает собственную поддержку для обмена сообщениями и очередей в SQL Server, что позволяет более эффективно получать обновления объединительной плате.</span><span class="sxs-lookup"><span data-stu-id="3b025-130">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="3b025-131">(Однако объединительной платы также работает без компонента Service Broker).</span><span class="sxs-lookup"><span data-stu-id="3b025-131">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="3b025-132">Чтобы проверить, включен ли компонент Service Broker, запрос **—\_broker\_включен** столбца в **sys.databases** представления каталога.</span><span class="sxs-lookup"><span data-stu-id="3b025-132">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="3b025-133">Чтобы включить компонент Service Broker, используйте следующий SQL-запрос:</span><span class="sxs-lookup"><span data-stu-id="3b025-133">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="3b025-134">Если взаимоблокировка, убедитесь, что этот запрос нет приложений, подключенных к базе данных.</span><span class="sxs-lookup"><span data-stu-id="3b025-134">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>


<span data-ttu-id="3b025-135">Если трассировка включена, данные трассировки также будет показано, включен ли компонент Service Broker.</span><span class="sxs-lookup"><span data-stu-id="3b025-135">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="3b025-136">Создание приложения SignalR</span><span class="sxs-lookup"><span data-stu-id="3b025-136">Create a SignalR Application</span></span>

<span data-ttu-id="3b025-137">Создайте приложение SignalR, выполните одно из этих учебников.</span><span class="sxs-lookup"><span data-stu-id="3b025-137">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="3b025-138">Приступая к работе с SignalR</span><span class="sxs-lookup"><span data-stu-id="3b025-138">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="3b025-139">Приступая к работе с SignalR и MVC 4</span><span class="sxs-lookup"><span data-stu-id="3b025-139">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="3b025-140">Затем мы изменим приложения разговора для поддержки горизонтального масштабирования с SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3b025-140">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="3b025-141">Сначала добавьте пакет SignalR.SqlServer NuGet в проект.</span><span class="sxs-lookup"><span data-stu-id="3b025-141">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="3b025-142">В Visual Studio из **средства** последовательно выберите пункты **диспетчер пакетов библиотеки**, а затем выберите **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="3b025-142">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="3b025-143">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="3b025-143">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="3b025-144">Затем откройте файл Global.asax.</span><span class="sxs-lookup"><span data-stu-id="3b025-144">Next, open the Global.asax file.</span></span> <span data-ttu-id="3b025-145">Добавьте следующий код в **приложения\_запустить** метод:</span><span class="sxs-lookup"><span data-stu-id="3b025-145">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="3b025-146">Развертывание и запуск приложения</span><span class="sxs-lookup"><span data-stu-id="3b025-146">Deploy and Run the Application</span></span>

<span data-ttu-id="3b025-147">Подготовьте экземпляры сервера Windows для развертывания приложения SignalR.</span><span class="sxs-lookup"><span data-stu-id="3b025-147">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="3b025-148">Добавьте роль IIS.</span><span class="sxs-lookup"><span data-stu-id="3b025-148">Add the IIS role.</span></span> <span data-ttu-id="3b025-149">Включить функции «Разработка приложений», включая протокол WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3b025-149">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="3b025-150">Также можно включить службу управления (в области «Средства управления»).</span><span class="sxs-lookup"><span data-stu-id="3b025-150">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="3b025-151">**Установка веб-развертывание 3.0.**</span><span class="sxs-lookup"><span data-stu-id="3b025-151">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="3b025-152">При выполнении диспетчера служб IIS, система предложит установить веб-платформы Майкрософт или вы можете [загрузки intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="3b025-152">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="3b025-153">В установщике платформы поиск веб-развертывания и установка Web Deploy 3.0</span><span class="sxs-lookup"><span data-stu-id="3b025-153">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="3b025-154">Проверьте, что запущена служба управления.</span><span class="sxs-lookup"><span data-stu-id="3b025-154">Check that the Web Management Service is running.</span></span> <span data-ttu-id="3b025-155">В противном случае запустите службу.</span><span class="sxs-lookup"><span data-stu-id="3b025-155">If not, start the service.</span></span> <span data-ttu-id="3b025-156">(Если вы не видите управления веб-службы в списке служб Windows, убедитесь, что установлена служба управления, при добавлении роли IIS.)</span><span class="sxs-lookup"><span data-stu-id="3b025-156">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="3b025-157">Наконец необходимо откройте порт 8172 протокол TCP.</span><span class="sxs-lookup"><span data-stu-id="3b025-157">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="3b025-158">Это порт, который используется средством веб-развертывания.</span><span class="sxs-lookup"><span data-stu-id="3b025-158">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="3b025-159">Теперь вы готовы к развертыванию проекта Visual Studio с компьютера разработки на сервер.</span><span class="sxs-lookup"><span data-stu-id="3b025-159">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="3b025-160">В обозревателе решений щелкните правой кнопкой мыши решение и выберите **публикации**.</span><span class="sxs-lookup"><span data-stu-id="3b025-160">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="3b025-161">Подробную документацию по веб-развертывания см. в разделе [Web Карта содержимого развертывания для Visual Studio и ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="3b025-161">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="3b025-162">При развертывании приложения на двух серверах, можно открыть каждый экземпляр в отдельном окне браузера и в разделе, чтобы они могли получать сообщений SignalR от другого.</span><span class="sxs-lookup"><span data-stu-id="3b025-162">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="3b025-163">(Конечно, в рабочей среде, два сервера будет разместить в подсистему балансировки нагрузки.)</span><span class="sxs-lookup"><span data-stu-id="3b025-163">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="3b025-164">После запуска приложения, можно увидеть, что SignalR автоматически создал таблиц в базе данных:</span><span class="sxs-lookup"><span data-stu-id="3b025-164">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="3b025-165">SignalR управляет таблиц.</span><span class="sxs-lookup"><span data-stu-id="3b025-165">SignalR manages the tables.</span></span> <span data-ttu-id="3b025-166">При условии, что приложение развертывается, не удалять строки, изменить таблицу и пр.</span><span class="sxs-lookup"><span data-stu-id="3b025-166">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
