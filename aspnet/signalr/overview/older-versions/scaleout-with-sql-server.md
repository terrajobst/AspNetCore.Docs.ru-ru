---
uid: signalr/overview/older-versions/scaleout-with-sql-server
title: Масштабирование SignalR с помощью SQL Server (SignalR 1.x) | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: riande
ms.date: 05/01/2013
ms.assetid: 1dca7967-8296-444a-9533-837eb284e78c
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 682aa837ed991cbf5d78dcb304e2c1bce905c52c
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287537"
---
<a name="signalr-scaleout-with-sql-server-signalr-1x"></a><span data-ttu-id="ecacd-102">Масштабирование SignalR с помощью SQL Server (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="ecacd-102">SignalR Scaleout with SQL Server (SignalR 1.x)</span></span>
====================
<span data-ttu-id="ecacd-103">по [Майк Уоссон](https://github.com/MikeWasson), [Патрик Флетчера](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="ecacd-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="ecacd-104">В этом руководстве будет использовать SQL Server, чтобы распределить сообщения в приложении SignalR, которое развертывается в два отдельных экземпляра IIS.</span><span class="sxs-lookup"><span data-stu-id="ecacd-104">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="ecacd-105">Можно также открыть этот пример на одном тестовом компьютере, но чтобы получить полные результаты, необходимо выполнить развертывание приложений SignalR на два или несколько серверов.</span><span class="sxs-lookup"><span data-stu-id="ecacd-105">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="ecacd-106">Также необходимо установить SQL Server на одном из серверов или на отдельном выделенном сервере.</span><span class="sxs-lookup"><span data-stu-id="ecacd-106">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="ecacd-107">Другой вариант — для работы с учебником, с помощью виртуальных машин в Azure.</span><span class="sxs-lookup"><span data-stu-id="ecacd-107">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="ecacd-108">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="ecacd-108">Prerequisites</span></span>

<span data-ttu-id="ecacd-109">Microsoft SQL Server 2005 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="ecacd-109">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="ecacd-110">Задняя панель поддерживает настольных и серверных выпусков SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ecacd-110">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="ecacd-111">Он не поддерживает SQL Server Compact Edition или базы данных SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="ecacd-111">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="ecacd-112">(Если приложение размещено в Azure, рекомендуется использовать на задней стороне служебной шины.)</span><span class="sxs-lookup"><span data-stu-id="ecacd-112">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="ecacd-113">Обзор</span><span class="sxs-lookup"><span data-stu-id="ecacd-113">Overview</span></span>

<span data-ttu-id="ecacd-114">Прежде чем мы перейдем к подробный учебник, ниже приведен краткий обзор. ваши действия.</span><span class="sxs-lookup"><span data-stu-id="ecacd-114">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="ecacd-115">Создание новой пустой базы данных.</span><span class="sxs-lookup"><span data-stu-id="ecacd-115">Create a new empty database.</span></span> <span data-ttu-id="ecacd-116">Задняя панель создаст необходимые таблицы в этой базе данных.</span><span class="sxs-lookup"><span data-stu-id="ecacd-116">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="ecacd-117">Добавьте следующие пакеты NuGet приложения:</span><span class="sxs-lookup"><span data-stu-id="ecacd-117">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="ecacd-118">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="ecacd-118">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="ecacd-119">Microsoft.AspNet.SignalR.SqlServer</span><span class="sxs-lookup"><span data-stu-id="ecacd-119">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="ecacd-120">Создайте приложение SignalR.</span><span class="sxs-lookup"><span data-stu-id="ecacd-120">Create a SignalR application.</span></span>
4. <span data-ttu-id="ecacd-121">Добавьте следующий код в Global.asax, чтобы настроить на задней стороне:</span><span class="sxs-lookup"><span data-stu-id="ecacd-121">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

## <a name="configure-the-database"></a><span data-ttu-id="ecacd-122">Настройка базы данных</span><span class="sxs-lookup"><span data-stu-id="ecacd-122">Configure the Database</span></span>

<span data-ttu-id="ecacd-123">Решите, будет ли приложение использовать проверку подлинности Windows или проверка подлинности SQL Server для доступа к базе данных.</span><span class="sxs-lookup"><span data-stu-id="ecacd-123">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="ecacd-124">В любом случае убедитесь, что пользователь базы данных имеет разрешения на вход, создавать схемы и создание таблиц.</span><span class="sxs-lookup"><span data-stu-id="ecacd-124">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="ecacd-125">Создайте новую базу данных для использования объединительной платы.</span><span class="sxs-lookup"><span data-stu-id="ecacd-125">Create a new database for the backplane to use.</span></span> <span data-ttu-id="ecacd-126">Можно выбрать любое имя базы данных.</span><span class="sxs-lookup"><span data-stu-id="ecacd-126">You can give the database any name.</span></span> <span data-ttu-id="ecacd-127">Не нужно создавать любой таблицы в базе данных; Задняя панель создаст необходимые таблицы.</span><span class="sxs-lookup"><span data-stu-id="ecacd-127">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="ecacd-128">Включение компонента Service Broker</span><span class="sxs-lookup"><span data-stu-id="ecacd-128">Enable Service Broker</span></span>

<span data-ttu-id="ecacd-129">Рекомендуется, чтобы включить компонент Service Broker для базы данных объединительной платы.</span><span class="sxs-lookup"><span data-stu-id="ecacd-129">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="ecacd-130">Компонент Service Broker обеспечивает встроенную поддержку для обмена сообщениями и очереди в SQL Server, что позволяет более эффективно получать обновления задней панели.</span><span class="sxs-lookup"><span data-stu-id="ecacd-130">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="ecacd-131">(Однако задней панели также работает без компонента Service Broker).</span><span class="sxs-lookup"><span data-stu-id="ecacd-131">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="ecacd-132">Чтобы проверить, включен ли компонент Service Broker, запросите **—\_broker\_включена** столбца в **sys.databases** представления каталога.</span><span class="sxs-lookup"><span data-stu-id="ecacd-132">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="ecacd-133">Чтобы включить компонент Service Broker, используйте следующий запрос SQL:</span><span class="sxs-lookup"><span data-stu-id="ecacd-133">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="ecacd-134">Если этот запрос принимать участие во взаимоблокировке, убедитесь, что нет подключения к базе данных приложений.</span><span class="sxs-lookup"><span data-stu-id="ecacd-134">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>

<span data-ttu-id="ecacd-135">Если трассировка включена, данные трассировки также будет показано, включен ли компонент Service Broker.</span><span class="sxs-lookup"><span data-stu-id="ecacd-135">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="ecacd-136">Создание приложения SignalR</span><span class="sxs-lookup"><span data-stu-id="ecacd-136">Create a SignalR Application</span></span>

<span data-ttu-id="ecacd-137">Создайте приложение SignalR, выполнив одно из следующих руководств:</span><span class="sxs-lookup"><span data-stu-id="ecacd-137">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="ecacd-138">Начало работы с SignalR</span><span class="sxs-lookup"><span data-stu-id="ecacd-138">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="ecacd-139">Начало работы с SignalR и MVC 4</span><span class="sxs-lookup"><span data-stu-id="ecacd-139">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="ecacd-140">Затем мы изменим приложение чата для поддержки горизонтального масштабирования с помощью SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ecacd-140">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="ecacd-141">Во-первых добавьте пакет SignalR.SqlServer NuGet в проект.</span><span class="sxs-lookup"><span data-stu-id="ecacd-141">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="ecacd-142">В Visual Studio из **средства** меню, выберите **диспетчер пакетов NuGet**, а затем выберите **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="ecacd-142">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="ecacd-143">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="ecacd-143">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="ecacd-144">Затем откройте файл Global.asax.</span><span class="sxs-lookup"><span data-stu-id="ecacd-144">Next, open the Global.asax file.</span></span> <span data-ttu-id="ecacd-145">Добавьте следующий код, чтобы **приложения\_запустить** метод:</span><span class="sxs-lookup"><span data-stu-id="ecacd-145">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="ecacd-146">Развертывание и запуск приложения</span><span class="sxs-lookup"><span data-stu-id="ecacd-146">Deploy and Run the Application</span></span>

<span data-ttu-id="ecacd-147">Подготовка экземпляров Windows Server для развертывания приложения SignalR.</span><span class="sxs-lookup"><span data-stu-id="ecacd-147">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="ecacd-148">Добавьте роль IIS.</span><span class="sxs-lookup"><span data-stu-id="ecacd-148">Add the IIS role.</span></span> <span data-ttu-id="ecacd-149">Включить функции «Разработка приложений», включая протокол WebSocket.</span><span class="sxs-lookup"><span data-stu-id="ecacd-149">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="ecacd-150">Также службы управления (см. в разделе «Средства управления»).</span><span class="sxs-lookup"><span data-stu-id="ecacd-150">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="ecacd-151">**Установить Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="ecacd-151">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="ecacd-152">Если запустить диспетчер служб IIS, он предложит вам установить веб-платформы Майкрософт или вы можете [загрузить intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="ecacd-152">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="ecacd-153">В установщике платформы найдите веб-развертывания и установить Web Deploy 3.0</span><span class="sxs-lookup"><span data-stu-id="ecacd-153">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="ecacd-154">Убедитесь, что веб-службы управления.</span><span class="sxs-lookup"><span data-stu-id="ecacd-154">Check that the Web Management Service is running.</span></span> <span data-ttu-id="ecacd-155">В противном случае запустите службу.</span><span class="sxs-lookup"><span data-stu-id="ecacd-155">If not, start the service.</span></span> <span data-ttu-id="ecacd-156">(Если вы не видите веб-службы управления в списке служб Windows, убедитесь, что вы установили службу управления, при добавлении роли IIS.)</span><span class="sxs-lookup"><span data-stu-id="ecacd-156">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="ecacd-157">Наконец откройте порт 8172 для TCP.</span><span class="sxs-lookup"><span data-stu-id="ecacd-157">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="ecacd-158">Это порт, который используется средством веб-развертывания.</span><span class="sxs-lookup"><span data-stu-id="ecacd-158">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="ecacd-159">Теперь все готово для развертывания проекта Visual Studio с компьютера разработки на сервер.</span><span class="sxs-lookup"><span data-stu-id="ecacd-159">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="ecacd-160">В обозревателе решений щелкните правой кнопкой мыши решение и нажмите кнопку **публикации**.</span><span class="sxs-lookup"><span data-stu-id="ecacd-160">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="ecacd-161">Дополнительные сведения о веб-развертывания, см. в разделе [веб-развертывания содержимого карты для Visual Studio и ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="ecacd-161">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="ecacd-162">При развертывании приложения на двух серверах, можно открыть каждый экземпляр в отдельном окне браузера и см. в разделе, что каждый из них сообщения SignalR от другого.</span><span class="sxs-lookup"><span data-stu-id="ecacd-162">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="ecacd-163">(Конечно, в рабочей среде, два сервера бы сидеть балансировщиком нагрузки.)</span><span class="sxs-lookup"><span data-stu-id="ecacd-163">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="ecacd-164">После запуска приложения, вы увидите, что SignalR автоматически создала таблиц в базе данных:</span><span class="sxs-lookup"><span data-stu-id="ecacd-164">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="ecacd-165">SignalR управляет таблицами.</span><span class="sxs-lookup"><span data-stu-id="ecacd-165">SignalR manages the tables.</span></span> <span data-ttu-id="ecacd-166">До тех пор, пока развертывается приложение, не удалять строки, изменить таблицу и т. д.</span><span class="sxs-lookup"><span data-stu-id="ecacd-166">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
