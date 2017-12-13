---
uid: signalr/overview/older-versions/scaleout-with-redis
title: "SignalR горизонтального масштабирования с помощью Redis (SignalR 1.x) | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 6abecf80-8ffa-41ba-b0d9-1d9edbe7687b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: c0d6fd421dad02298326d1975ae68d1e7cc78d8c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="signalr-scaleout-with-redis-signalr-1x"></a><span data-ttu-id="b0c48-102">SignalR горизонтального масштабирования с помощью Redis (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="b0c48-102">SignalR Scaleout with Redis (SignalR 1.x)</span></span>
====================
<span data-ttu-id="b0c48-103">по [Mike Wasson](https://github.com/MikeWasson), [Патрик Флетчера](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="b0c48-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="b0c48-104">В этом учебнике используется [Redis](http://redis.io/) для распределения сообщений между SignalR приложения, которое развертывается на два отдельных экземпляра IIS.</span><span class="sxs-lookup"><span data-stu-id="b0c48-104">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="b0c48-105">Redis представляет хранилищу ключей и значений в памяти.</span><span class="sxs-lookup"><span data-stu-id="b0c48-105">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="b0c48-106">Оно также поддерживает системы обмена сообщениями с моделью, публикации или подписки.</span><span class="sxs-lookup"><span data-stu-id="b0c48-106">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="b0c48-107">На задней стороне SignalR Redis использует функцию pub/sub для пересылки сообщений на другие серверы.</span><span class="sxs-lookup"><span data-stu-id="b0c48-107">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="b0c48-108">В этом учебнике используется три сервера:</span><span class="sxs-lookup"><span data-stu-id="b0c48-108">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="b0c48-109">Два сервера под управлением Windows, который будет использоваться для развертывания приложения SignalR.</span><span class="sxs-lookup"><span data-stu-id="b0c48-109">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="b0c48-110">Один сервер под управлением Linux, который будет использоваться для выполнения Redis.</span><span class="sxs-lookup"><span data-stu-id="b0c48-110">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="b0c48-111">Снимки экрана, в этом учебнике я использовал Ubuntu 12.04 TLS.</span><span class="sxs-lookup"><span data-stu-id="b0c48-111">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="b0c48-112">При отсутствии трех физических серверов для использования, можно создать виртуальные машины в Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="b0c48-112">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="b0c48-113">Другой вариант — создать виртуальные машины в Azure.</span><span class="sxs-lookup"><span data-stu-id="b0c48-113">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="b0c48-114">Несмотря на то, что в этом учебнике используется официальный реализацию Redis, имеется также [Windows порт из Redis](https://github.com/MSOpenTech/redis) из MSOpenTech.</span><span class="sxs-lookup"><span data-stu-id="b0c48-114">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="b0c48-115">Установка и настройка различаются, а в противном случае действия одинаковы.</span><span class="sxs-lookup"><span data-stu-id="b0c48-115">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="b0c48-116">SignalR горизонтального масштабирования с помощью Redis не поддерживает кластеры Redis.</span><span class="sxs-lookup"><span data-stu-id="b0c48-116">SignalR scaleout with Redis does not support Redis clusters.</span></span>


## <a name="overview"></a><span data-ttu-id="b0c48-117">Обзор</span><span class="sxs-lookup"><span data-stu-id="b0c48-117">Overview</span></span>

<span data-ttu-id="b0c48-118">Прежде чем мы приступим к подробный учебник, ниже приведен краткий обзор того, что сделать.</span><span class="sxs-lookup"><span data-stu-id="b0c48-118">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="b0c48-119">Установка Redis и запуск сервера Redis.</span><span class="sxs-lookup"><span data-stu-id="b0c48-119">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="b0c48-120">Добавьте эти пакеты NuGet для приложения:</span><span class="sxs-lookup"><span data-stu-id="b0c48-120">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="b0c48-121">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="b0c48-121">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="b0c48-122">Microsoft.AspNet.SignalR.Redis</span><span class="sxs-lookup"><span data-stu-id="b0c48-122">Microsoft.AspNet.SignalR.Redis</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. <span data-ttu-id="b0c48-123">Создайте приложение SignalR.</span><span class="sxs-lookup"><span data-stu-id="b0c48-123">Create a SignalR application.</span></span>
4. <span data-ttu-id="b0c48-124">Добавьте следующий код Global.asax для настройки на задней стороне:</span><span class="sxs-lookup"><span data-stu-id="b0c48-124">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="b0c48-125">Ubuntu на узле Hyper-V</span><span class="sxs-lookup"><span data-stu-id="b0c48-125">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="b0c48-126">С помощью Windows Hyper-V, можно легко создать Виртуальной машине Ubuntu на Windows Server.</span><span class="sxs-lookup"><span data-stu-id="b0c48-126">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="b0c48-127">Загрузите ISO-образ Ubuntu от [http://www.ubuntu.com](http://www.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="b0c48-127">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="b0c48-128">В Hyper-V Добавление новой виртуальной Машины.</span><span class="sxs-lookup"><span data-stu-id="b0c48-128">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="b0c48-129">В **подключить виртуальный жесткий диск** шаг, выберите **создать виртуальный жесткий диск**.</span><span class="sxs-lookup"><span data-stu-id="b0c48-129">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="b0c48-130">В **параметры установки** шаг, выберите **файл образа (.iso)**, нажмите кнопку **Обзор**и перейдите к установке Ubuntu ISO.</span><span class="sxs-lookup"><span data-stu-id="b0c48-130">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="b0c48-131">Установить Redis</span><span class="sxs-lookup"><span data-stu-id="b0c48-131">Install Redis</span></span>

<span data-ttu-id="b0c48-132">Следуйте указаниям в [http://redis.io/download](http://redis.io/download) загрузить и построить Redis.</span><span class="sxs-lookup"><span data-stu-id="b0c48-132">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="b0c48-133">Это создает двоичные файлы Redis `src` каталога.</span><span class="sxs-lookup"><span data-stu-id="b0c48-133">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="b0c48-134">По умолчанию Redis не требует пароля.</span><span class="sxs-lookup"><span data-stu-id="b0c48-134">By default, Redis does not require a password.</span></span> <span data-ttu-id="b0c48-135">Чтобы указать пароль, изменить `redis.conf` файл, расположенный в корневом каталоге исходного кода.</span><span class="sxs-lookup"><span data-stu-id="b0c48-135">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="b0c48-136">(Сделать резервную копию файла, перед внесением изменений!) Добавьте следующую директиву для `redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="b0c48-136">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="b0c48-137">Теперь запустите сервер Redis:</span><span class="sxs-lookup"><span data-stu-id="b0c48-137">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="b0c48-138">Открытый порт 6379, который Redis порта по умолчанию прослушивает.</span><span class="sxs-lookup"><span data-stu-id="b0c48-138">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="b0c48-139">(Можно изменить номер порта в файле конфигурации).</span><span class="sxs-lookup"><span data-stu-id="b0c48-139">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="b0c48-140">Создание приложения SignalR</span><span class="sxs-lookup"><span data-stu-id="b0c48-140">Create the SignalR Application</span></span>

<span data-ttu-id="b0c48-141">Создайте приложение SignalR, выполните одно из этих учебников.</span><span class="sxs-lookup"><span data-stu-id="b0c48-141">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="b0c48-142">Приступая к работе с SignalR</span><span class="sxs-lookup"><span data-stu-id="b0c48-142">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="b0c48-143">Приступая к работе с SignalR и MVC 4</span><span class="sxs-lookup"><span data-stu-id="b0c48-143">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="b0c48-144">Затем мы изменим приложения разговора для поддержки горизонтального масштабирования с помощью Redis.</span><span class="sxs-lookup"><span data-stu-id="b0c48-144">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="b0c48-145">Сначала добавьте пакет SignalR.Redis NuGet в проект.</span><span class="sxs-lookup"><span data-stu-id="b0c48-145">First, add the SignalR.Redis NuGet package to your project.</span></span> <span data-ttu-id="b0c48-146">В Visual Studio из **средства** последовательно выберите пункты **диспетчер пакетов библиотеки**, а затем выберите **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="b0c48-146">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="b0c48-147">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="b0c48-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="b0c48-148">Затем откройте файл Global.asax.</span><span class="sxs-lookup"><span data-stu-id="b0c48-148">Next, open the Global.asax file.</span></span> <span data-ttu-id="b0c48-149">Добавьте следующий код в **приложения\_запустить** метод:</span><span class="sxs-lookup"><span data-stu-id="b0c48-149">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="b0c48-150">«сервер» является имя сервера, на котором выполняется Redis.</span><span class="sxs-lookup"><span data-stu-id="b0c48-150">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="b0c48-151">*порт* — номер порта</span><span class="sxs-lookup"><span data-stu-id="b0c48-151">*port* is the port number</span></span>
- <span data-ttu-id="b0c48-152">«пароль» — это пароль, который определен в файле redis.conf.</span><span class="sxs-lookup"><span data-stu-id="b0c48-152">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="b0c48-153">«Имя_приложения» — это любая строка.</span><span class="sxs-lookup"><span data-stu-id="b0c48-153">"AppName" is any string.</span></span> <span data-ttu-id="b0c48-154">SignalR создает Redis pub/sub канал с таким именем.</span><span class="sxs-lookup"><span data-stu-id="b0c48-154">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="b0c48-155">Пример:</span><span class="sxs-lookup"><span data-stu-id="b0c48-155">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="b0c48-156">Развертывание и запуск приложения</span><span class="sxs-lookup"><span data-stu-id="b0c48-156">Deploy and Run the Application</span></span>

<span data-ttu-id="b0c48-157">Подготовьте экземпляры сервера Windows для развертывания приложения SignalR.</span><span class="sxs-lookup"><span data-stu-id="b0c48-157">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="b0c48-158">Добавьте роль IIS.</span><span class="sxs-lookup"><span data-stu-id="b0c48-158">Add the IIS role.</span></span> <span data-ttu-id="b0c48-159">Включить функции «Разработка приложений», включая протокол WebSocket.</span><span class="sxs-lookup"><span data-stu-id="b0c48-159">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="b0c48-160">Также можно включить службу управления (в области «Средства управления»).</span><span class="sxs-lookup"><span data-stu-id="b0c48-160">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="b0c48-161">**Установка веб-развертывание 3.0.**</span><span class="sxs-lookup"><span data-stu-id="b0c48-161">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="b0c48-162">При выполнении диспетчера служб IIS, система предложит установить веб-платформы Майкрософт или вы можете [загрузки intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="b0c48-162">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="b0c48-163">В установщике платформы поиск веб-развертывания и установка Web Deploy 3.0</span><span class="sxs-lookup"><span data-stu-id="b0c48-163">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="b0c48-164">Проверьте, что запущена служба управления.</span><span class="sxs-lookup"><span data-stu-id="b0c48-164">Check that the Web Management Service is running.</span></span> <span data-ttu-id="b0c48-165">В противном случае запустите службу.</span><span class="sxs-lookup"><span data-stu-id="b0c48-165">If not, start the service.</span></span> <span data-ttu-id="b0c48-166">(Если вы не видите управления веб-службы в списке служб Windows, убедитесь, что установлена служба управления, при добавлении роли IIS.)</span><span class="sxs-lookup"><span data-stu-id="b0c48-166">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="b0c48-167">Управление веб-службы по умолчанию прослушивает TCP-порт 8172.</span><span class="sxs-lookup"><span data-stu-id="b0c48-167">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="b0c48-168">В брандмауэре Windows создайте новое правило входящего трафика, разрешающее трафик TCP через порт 8172.</span><span class="sxs-lookup"><span data-stu-id="b0c48-168">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="b0c48-169">Дополнительные сведения см. в разделе [Настройка правил брандмауэра](https://technet.microsoft.com/en-us/library/dd448559(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="b0c48-169">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/en-us/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="b0c48-170">(При размещении виртуальных машин в Azure, это можно сделать непосредственно на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="b0c48-170">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="b0c48-171">В разделе [Настройка конечных точек для виртуальной машины](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-set-up-endpoints/).)</span><span class="sxs-lookup"><span data-stu-id="b0c48-171">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="b0c48-172">Теперь вы готовы к развертыванию проекта Visual Studio с компьютера разработки на сервер.</span><span class="sxs-lookup"><span data-stu-id="b0c48-172">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="b0c48-173">В обозревателе решений щелкните правой кнопкой мыши решение и выберите **публикации**.</span><span class="sxs-lookup"><span data-stu-id="b0c48-173">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="b0c48-174">Подробную документацию по веб-развертывания см. в разделе [Web Карта содержимого развертывания для Visual Studio и ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="b0c48-174">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="b0c48-175">При развертывании приложения на двух серверах, можно открыть каждый экземпляр в отдельном окне браузера и в разделе, чтобы они могли получать сообщений SignalR от другого.</span><span class="sxs-lookup"><span data-stu-id="b0c48-175">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="b0c48-176">(Конечно, в рабочей среде, два сервера будет разместить в подсистему балансировки нагрузки.)</span><span class="sxs-lookup"><span data-stu-id="b0c48-176">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="b0c48-177">Если вы хотите, чтобы сообщения, отправленные Redis, можно использовать **redis cli** клиента, которая устанавливается вместе с Redis.</span><span class="sxs-lookup"><span data-stu-id="b0c48-177">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
