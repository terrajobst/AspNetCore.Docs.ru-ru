---
uid: signalr/overview/performance/scaleout-with-redis
title: SignalR горизонтального масштабирования с помощью Redis | Документы Microsoft
author: MikeWasson
description: Версии программного обеспечения используется в этом разделе Visual Studio 2013 .NET 4.5 SignalR предыдущие версии версии 2 в этом разделе сведения о предыдущих версиях...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 2ef161f35e69ef4a754d2740199166ee48c3fbab
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
ms.locfileid: "28042694"
---
<a name="signalr-scaleout-with-redis"></a><span data-ttu-id="86ce3-103">SignalR горизонтального масштабирования с Redis</span><span class="sxs-lookup"><span data-stu-id="86ce3-103">SignalR Scaleout with Redis</span></span>
====================
<span data-ttu-id="86ce3-104">по [Mike Wasson](https://github.com/MikeWasson), [Патрик Флетчера](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="86ce3-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="86ce3-105">Версии программного обеспечения, используемого в этом разделе</span><span class="sxs-lookup"><span data-stu-id="86ce3-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="86ce3-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="86ce3-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="86ce3-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="86ce3-107">.NET 4.5</span></span>
> - <span data-ttu-id="86ce3-108">SignalR версии 2</span><span class="sxs-lookup"><span data-stu-id="86ce3-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="86ce3-109">Предыдущие версии этого раздела</span><span class="sxs-lookup"><span data-stu-id="86ce3-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="86ce3-110">Сведения о более ранних версий SignalR см. в разделе [более ранних версий SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="86ce3-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="86ce3-111">Вопросы и комментарии</span><span class="sxs-lookup"><span data-stu-id="86ce3-111">Questions and comments</span></span>
> 
> <span data-ttu-id="86ce3-112">Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить в комментарии в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="86ce3-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="86ce3-113">Если у вас есть вопросы, которые не связаны непосредственно для работы с учебником, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="86ce3-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="86ce3-114">В этом учебнике используется [Redis](http://redis.io/) для распределения сообщений между SignalR приложения, которое развертывается на два отдельных экземпляра IIS.</span><span class="sxs-lookup"><span data-stu-id="86ce3-114">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="86ce3-115">Redis представляет хранилищу ключей и значений в памяти.</span><span class="sxs-lookup"><span data-stu-id="86ce3-115">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="86ce3-116">Оно также поддерживает системы обмена сообщениями с моделью, публикации или подписки.</span><span class="sxs-lookup"><span data-stu-id="86ce3-116">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="86ce3-117">На задней стороне SignalR Redis использует функцию pub/sub для пересылки сообщений на другие серверы.</span><span class="sxs-lookup"><span data-stu-id="86ce3-117">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="86ce3-118">В этом учебнике используется три сервера:</span><span class="sxs-lookup"><span data-stu-id="86ce3-118">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="86ce3-119">Два сервера под управлением Windows, который будет использоваться для развертывания приложения SignalR.</span><span class="sxs-lookup"><span data-stu-id="86ce3-119">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="86ce3-120">Один сервер под управлением Linux, который будет использоваться для выполнения Redis.</span><span class="sxs-lookup"><span data-stu-id="86ce3-120">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="86ce3-121">Снимки экрана, в этом учебнике я использовал Ubuntu 12.04 TLS.</span><span class="sxs-lookup"><span data-stu-id="86ce3-121">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="86ce3-122">При отсутствии трех физических серверов для использования, можно создать виртуальные машины в Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="86ce3-122">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="86ce3-123">Другой вариант — создать виртуальные машины в Azure.</span><span class="sxs-lookup"><span data-stu-id="86ce3-123">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="86ce3-124">Несмотря на то, что в этом учебнике используется официальный реализацию Redis, имеется также [Windows порт из Redis](https://github.com/MSOpenTech/redis) из MSOpenTech.</span><span class="sxs-lookup"><span data-stu-id="86ce3-124">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="86ce3-125">Установка и настройка различаются, а в противном случае действия одинаковы.</span><span class="sxs-lookup"><span data-stu-id="86ce3-125">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="86ce3-126">SignalR горизонтального масштабирования с помощью Redis не поддерживает кластеры Redis.</span><span class="sxs-lookup"><span data-stu-id="86ce3-126">SignalR scaleout with Redis does not support Redis clusters.</span></span>


## <a name="overview"></a><span data-ttu-id="86ce3-127">Обзор</span><span class="sxs-lookup"><span data-stu-id="86ce3-127">Overview</span></span>

<span data-ttu-id="86ce3-128">Прежде чем мы приступим к подробный учебник, ниже приведен краткий обзор того, что сделать.</span><span class="sxs-lookup"><span data-stu-id="86ce3-128">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="86ce3-129">Установка Redis и запуск сервера Redis.</span><span class="sxs-lookup"><span data-stu-id="86ce3-129">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="86ce3-130">Добавьте эти пакеты NuGet для приложения:</span><span class="sxs-lookup"><span data-stu-id="86ce3-130">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="86ce3-131">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="86ce3-131">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="86ce3-132">Microsoft.AspNet.SignalR.Redis</span><span class="sxs-lookup"><span data-stu-id="86ce3-132">Microsoft.AspNet.SignalR.Redis</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. <span data-ttu-id="86ce3-133">Создайте приложение SignalR.</span><span class="sxs-lookup"><span data-stu-id="86ce3-133">Create a SignalR application.</span></span>
4. <span data-ttu-id="86ce3-134">Добавьте следующий код в файле Startup.cs для настройки на задней стороне:</span><span class="sxs-lookup"><span data-stu-id="86ce3-134">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="86ce3-135">Ubuntu на узле Hyper-V</span><span class="sxs-lookup"><span data-stu-id="86ce3-135">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="86ce3-136">С помощью Windows Hyper-V, можно легко создать Виртуальной машине Ubuntu на Windows Server.</span><span class="sxs-lookup"><span data-stu-id="86ce3-136">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="86ce3-137">Загрузите ISO-образ Ubuntu от [http://www.ubuntu.com](http://www.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="86ce3-137">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="86ce3-138">В Hyper-V Добавление новой виртуальной Машины.</span><span class="sxs-lookup"><span data-stu-id="86ce3-138">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="86ce3-139">В **подключить виртуальный жесткий диск** шаг, выберите **создать виртуальный жесткий диск**.</span><span class="sxs-lookup"><span data-stu-id="86ce3-139">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="86ce3-140">В **параметры установки** шаг, выберите **файл образа (.iso)**, нажмите кнопку **Обзор**и перейдите к установке Ubuntu ISO.</span><span class="sxs-lookup"><span data-stu-id="86ce3-140">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="86ce3-141">Установить Redis</span><span class="sxs-lookup"><span data-stu-id="86ce3-141">Install Redis</span></span>

<span data-ttu-id="86ce3-142">Следуйте указаниям в [http://redis.io/download](http://redis.io/download) загрузить и построить Redis.</span><span class="sxs-lookup"><span data-stu-id="86ce3-142">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="86ce3-143">Это создает двоичные файлы Redis `src` каталога.</span><span class="sxs-lookup"><span data-stu-id="86ce3-143">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="86ce3-144">По умолчанию Redis не требует пароля.</span><span class="sxs-lookup"><span data-stu-id="86ce3-144">By default, Redis does not require a password.</span></span> <span data-ttu-id="86ce3-145">Чтобы указать пароль, изменить `redis.conf` файл, расположенный в корневом каталоге исходного кода.</span><span class="sxs-lookup"><span data-stu-id="86ce3-145">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="86ce3-146">(Сделать резервную копию файла, перед внесением изменений!) Добавьте следующую директиву для `redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="86ce3-146">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="86ce3-147">Теперь запустите сервер Redis:</span><span class="sxs-lookup"><span data-stu-id="86ce3-147">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="86ce3-148">Открытый порт 6379, который Redis порта по умолчанию прослушивает.</span><span class="sxs-lookup"><span data-stu-id="86ce3-148">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="86ce3-149">(Можно изменить номер порта в файле конфигурации).</span><span class="sxs-lookup"><span data-stu-id="86ce3-149">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="86ce3-150">Создание приложения SignalR</span><span class="sxs-lookup"><span data-stu-id="86ce3-150">Create the SignalR Application</span></span>

<span data-ttu-id="86ce3-151">Создайте приложение SignalR, выполните одно из этих учебников.</span><span class="sxs-lookup"><span data-stu-id="86ce3-151">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="86ce3-152">Приступая к работе с SignalR 2.0</span><span class="sxs-lookup"><span data-stu-id="86ce3-152">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="86ce3-153">Приступая к работе с SignalR 2.0 и MVC 5</span><span class="sxs-lookup"><span data-stu-id="86ce3-153">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="86ce3-154">Затем мы изменим приложения разговора для поддержки горизонтального масштабирования с помощью Redis.</span><span class="sxs-lookup"><span data-stu-id="86ce3-154">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="86ce3-155">Сначала добавьте пакет SignalR.Redis NuGet в проект.</span><span class="sxs-lookup"><span data-stu-id="86ce3-155">First, add the SignalR.Redis NuGet package to your project.</span></span> <span data-ttu-id="86ce3-156">В Visual Studio из **средства** последовательно выберите пункты **диспетчер пакетов библиотеки**, а затем выберите **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="86ce3-156">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="86ce3-157">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="86ce3-157">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="86ce3-158">Затем откройте файл файле Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="86ce3-158">Next, open the Startup.cs file.</span></span> <span data-ttu-id="86ce3-159">Добавьте следующий код в **конфигурации** метод:</span><span class="sxs-lookup"><span data-stu-id="86ce3-159">Add the following code to the **Configuration** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="86ce3-160">«сервер» является имя сервера, на котором выполняется Redis.</span><span class="sxs-lookup"><span data-stu-id="86ce3-160">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="86ce3-161">*порт* — номер порта</span><span class="sxs-lookup"><span data-stu-id="86ce3-161">*port* is the port number</span></span>
- <span data-ttu-id="86ce3-162">«пароль» — это пароль, который определен в файле redis.conf.</span><span class="sxs-lookup"><span data-stu-id="86ce3-162">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="86ce3-163">«Имя_приложения» — это любая строка.</span><span class="sxs-lookup"><span data-stu-id="86ce3-163">"AppName" is any string.</span></span> <span data-ttu-id="86ce3-164">SignalR создает Redis pub/sub канал с таким именем.</span><span class="sxs-lookup"><span data-stu-id="86ce3-164">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="86ce3-165">Пример:</span><span class="sxs-lookup"><span data-stu-id="86ce3-165">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="86ce3-166">Развертывание и запуск приложения</span><span class="sxs-lookup"><span data-stu-id="86ce3-166">Deploy and Run the Application</span></span>

<span data-ttu-id="86ce3-167">Подготовьте экземпляры сервера Windows для развертывания приложения SignalR.</span><span class="sxs-lookup"><span data-stu-id="86ce3-167">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="86ce3-168">Добавьте роль IIS.</span><span class="sxs-lookup"><span data-stu-id="86ce3-168">Add the IIS role.</span></span> <span data-ttu-id="86ce3-169">Включить функции «Разработка приложений», включая протокол WebSocket.</span><span class="sxs-lookup"><span data-stu-id="86ce3-169">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="86ce3-170">Также можно включить службу управления (в области «Средства управления»).</span><span class="sxs-lookup"><span data-stu-id="86ce3-170">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="86ce3-171">**Установка веб-развертывание 3.0.**</span><span class="sxs-lookup"><span data-stu-id="86ce3-171">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="86ce3-172">При выполнении диспетчера служб IIS, система предложит установить веб-платформы Майкрософт или вы можете [загрузки intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="86ce3-172">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="86ce3-173">В установщике платформы поиск веб-развертывания и установка Web Deploy 3.0</span><span class="sxs-lookup"><span data-stu-id="86ce3-173">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="86ce3-174">Проверьте, что запущена служба управления.</span><span class="sxs-lookup"><span data-stu-id="86ce3-174">Check that the Web Management Service is running.</span></span> <span data-ttu-id="86ce3-175">В противном случае запустите службу.</span><span class="sxs-lookup"><span data-stu-id="86ce3-175">If not, start the service.</span></span> <span data-ttu-id="86ce3-176">(Если вы не видите управления веб-службы в списке служб Windows, убедитесь, что установлена служба управления, при добавлении роли IIS.)</span><span class="sxs-lookup"><span data-stu-id="86ce3-176">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="86ce3-177">Управление веб-службы по умолчанию прослушивает TCP-порт 8172.</span><span class="sxs-lookup"><span data-stu-id="86ce3-177">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="86ce3-178">В брандмауэре Windows создайте новое правило входящего трафика, разрешающее трафик TCP через порт 8172.</span><span class="sxs-lookup"><span data-stu-id="86ce3-178">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="86ce3-179">Дополнительные сведения см. в разделе [Настройка правил брандмауэра](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="86ce3-179">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="86ce3-180">(При размещении виртуальных машин в Azure, это можно сделать непосредственно на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="86ce3-180">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="86ce3-181">В разделе [Настройка конечных точек для виртуальной машины](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span><span class="sxs-lookup"><span data-stu-id="86ce3-181">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="86ce3-182">Теперь вы готовы к развертыванию проекта Visual Studio с компьютера разработки на сервер.</span><span class="sxs-lookup"><span data-stu-id="86ce3-182">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="86ce3-183">В обозревателе решений щелкните правой кнопкой мыши решение и выберите **публикации**.</span><span class="sxs-lookup"><span data-stu-id="86ce3-183">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="86ce3-184">Подробную документацию по веб-развертывания см. в разделе [Web Карта содержимого развертывания для Visual Studio и ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="86ce3-184">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="86ce3-185">При развертывании приложения на двух серверах, можно открыть каждый экземпляр в отдельном окне браузера и в разделе, чтобы они могли получать сообщений SignalR от другого.</span><span class="sxs-lookup"><span data-stu-id="86ce3-185">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="86ce3-186">(Конечно, в рабочей среде, два сервера будет разместить в подсистему балансировки нагрузки.)</span><span class="sxs-lookup"><span data-stu-id="86ce3-186">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="86ce3-187">Если вы хотите, чтобы сообщения, отправленные Redis, можно использовать **redis cli** клиента, которая устанавливается вместе с Redis.</span><span class="sxs-lookup"><span data-stu-id="86ce3-187">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
