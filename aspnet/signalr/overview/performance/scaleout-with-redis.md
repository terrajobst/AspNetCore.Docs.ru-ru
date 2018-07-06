---
uid: signalr/overview/performance/scaleout-with-redis
title: Масштабирование SignalR с помощью Redis | Документация Майкрософт
author: MikeWasson
description: Версии программного обеспечения используется в этом разделе 4.5 .NET SignalR для Visual Studio 2013 версии 2, предыдущие версии в этом разделе сведения о более ранних версиях...
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 8c125070aa2bc8f0070d9933b77e09a5c84b797a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831010"
---
<a name="signalr-scaleout-with-redis"></a><span data-ttu-id="ab3b8-103">Масштабирование SignalR с помощью Redis</span><span class="sxs-lookup"><span data-stu-id="ab3b8-103">SignalR Scaleout with Redis</span></span>
====================
<span data-ttu-id="ab3b8-104">по [Майк Уоссон](https://github.com/MikeWasson), [Патрик Флетчера](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="ab3b8-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="ab3b8-105">Версии программного обеспечения, используемого в этом разделе</span><span class="sxs-lookup"><span data-stu-id="ab3b8-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="ab3b8-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="ab3b8-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="ab3b8-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="ab3b8-107">.NET 4.5</span></span>
> - <span data-ttu-id="ab3b8-108">SignalR версии 2</span><span class="sxs-lookup"><span data-stu-id="ab3b8-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="ab3b8-109">Предыдущие версии этого раздела</span><span class="sxs-lookup"><span data-stu-id="ab3b8-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="ab3b8-110">Сведения о более ранних версий SignalR, см. в разделе [более старых версий SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="ab3b8-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="ab3b8-111">Вопросы и комментарии</span><span class="sxs-lookup"><span data-stu-id="ab3b8-111">Questions and comments</span></span>
> 
> <span data-ttu-id="ab3b8-112">Оставьте свои отзывы на том, как вам понравилось, и этот учебник и что можно улучшить в комментариях в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="ab3b8-113">Если у вас есть вопросы, которые не имеют отношения к руководству, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="ab3b8-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="ab3b8-114">В этом руководстве вы воспользуетесь [Redis](http://redis.io/) чтобы распределить сообщения в приложении SignalR, которое развертывается на два отдельных экземпляра IIS.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-114">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="ab3b8-115">Redis — это хранилище ключ значение в памяти.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-115">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="ab3b8-116">Она также поддерживает систему обмена сообщениями с помощью модели публикации и подписки.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-116">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="ab3b8-117">На задней стороне SignalR Redis используется функция pub/sub для перенаправления сообщений к другим серверам.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-117">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="ab3b8-118">В этом учебнике будет использоваться на трех серверах:</span><span class="sxs-lookup"><span data-stu-id="ab3b8-118">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="ab3b8-119">Два сервера под управлением Windows, который будет использоваться для развертывания приложения SignalR.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-119">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="ab3b8-120">Один сервер под управлением Linux, который будет использоваться для выполнения Redis.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-120">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="ab3b8-121">Снимки экрана в этом руководстве я использовал Ubuntu 12.04 TLS.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-121">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="ab3b8-122">Если у вас нет трех физических серверов для использования, можно создать виртуальные машины в Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-122">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="ab3b8-123">Другой вариант — для создания виртуальных машин в Azure.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-123">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="ab3b8-124">Несмотря на то, что в этом учебнике используется официальный реализация Redis, имеется также [Windows порт из Redis](https://github.com/MSOpenTech/redis) из MSOpenTech.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-124">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="ab3b8-125">Установка и настройка различаются, но в противном случае действия одинаковы.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-125">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="ab3b8-126">Масштабирование SignalR с помощью Redis не поддерживает кластеры Redis.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-126">SignalR scaleout with Redis does not support Redis clusters.</span></span>


## <a name="overview"></a><span data-ttu-id="ab3b8-127">Обзор</span><span class="sxs-lookup"><span data-stu-id="ab3b8-127">Overview</span></span>

<span data-ttu-id="ab3b8-128">Прежде чем мы перейдем к подробный учебник, ниже приведен краткий обзор. ваши действия.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-128">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="ab3b8-129">Установите Redis и запустить сервер Redis.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-129">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="ab3b8-130">Добавьте следующие пакеты NuGet приложения:</span><span class="sxs-lookup"><span data-stu-id="ab3b8-130">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="ab3b8-131">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="ab3b8-131">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="ab3b8-132">Microsoft.AspNet.SignalR.Redis</span><span class="sxs-lookup"><span data-stu-id="ab3b8-132">Microsoft.AspNet.SignalR.Redis</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. <span data-ttu-id="ab3b8-133">Создайте приложение SignalR.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-133">Create a SignalR application.</span></span>
4. <span data-ttu-id="ab3b8-134">Добавьте следующий код в Startup.cs для настройки на задней стороне:</span><span class="sxs-lookup"><span data-stu-id="ab3b8-134">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="ab3b8-135">Ubuntu в Hyper-V</span><span class="sxs-lookup"><span data-stu-id="ab3b8-135">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="ab3b8-136">С помощью Windows Hyper-V, можно легко создать виртуальной Машины Ubuntu в Windows Server.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-136">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="ab3b8-137">Скачайте ISO-образ Ubuntu из [ http://www.ubuntu.com ](http://www.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="ab3b8-137">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="ab3b8-138">В Hyper-V добавьте новую виртуальную Машину.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-138">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="ab3b8-139">В **подключить виртуальный жесткий диск** выберите **создать виртуальный жесткий диск**.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-139">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="ab3b8-140">В **параметры установки** выберите **файл образа (.iso)**, нажмите кнопку **Обзор**и перейдите к Ubuntu установочный ISO-образ.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-140">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="ab3b8-141">Установите Redis</span><span class="sxs-lookup"><span data-stu-id="ab3b8-141">Install Redis</span></span>

<span data-ttu-id="ab3b8-142">Следуйте инструкциям из статьи [ http://redis.io/download ](http://redis.io/download) для скачивания и сборки Redis.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-142">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="ab3b8-143">Это создает двоичные файлы Redis `src` каталога.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-143">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="ab3b8-144">По умолчанию Redis не требует пароля.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-144">By default, Redis does not require a password.</span></span> <span data-ttu-id="ab3b8-145">Чтобы указать пароль, изменить `redis.conf` файл, который находится в корневой каталог исходного кода.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-145">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="ab3b8-146">(Перед внесением изменений убедитесь в резервную копию файла!) Добавьте следующую директиву для `redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="ab3b8-146">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="ab3b8-147">Теперь запустите сервер Redis:</span><span class="sxs-lookup"><span data-stu-id="ab3b8-147">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="ab3b8-148">Открытие порта 6379, который является портом по умолчанию, Redis прослушивает.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-148">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="ab3b8-149">(Можно изменить номер порта в файле конфигурации.)</span><span class="sxs-lookup"><span data-stu-id="ab3b8-149">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="ab3b8-150">Создание приложения SignalR</span><span class="sxs-lookup"><span data-stu-id="ab3b8-150">Create the SignalR Application</span></span>

<span data-ttu-id="ab3b8-151">Создайте приложение SignalR, выполнив одно из следующих руководств:</span><span class="sxs-lookup"><span data-stu-id="ab3b8-151">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="ab3b8-152">Начало работы с SignalR 2.0</span><span class="sxs-lookup"><span data-stu-id="ab3b8-152">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="ab3b8-153">Начало работы с SignalR 2.0 и MVC 5</span><span class="sxs-lookup"><span data-stu-id="ab3b8-153">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="ab3b8-154">Затем мы изменим приложение чата для поддержки горизонтального масштабирования с помощью Redis.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-154">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="ab3b8-155">Во-первых добавьте пакет SignalR.Redis NuGet в проект.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-155">First, add the SignalR.Redis NuGet package to your project.</span></span> <span data-ttu-id="ab3b8-156">В Visual Studio из **средства** меню, выберите **диспетчер пакетов библиотеки**, а затем выберите **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-156">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="ab3b8-157">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="ab3b8-157">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="ab3b8-158">Затем откройте файл Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-158">Next, open the Startup.cs file.</span></span> <span data-ttu-id="ab3b8-159">Добавьте следующий код, чтобы **конфигурации** метод:</span><span class="sxs-lookup"><span data-stu-id="ab3b8-159">Add the following code to the **Configuration** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="ab3b8-160">«server» — имя сервера, на котором выполняется Redis.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-160">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="ab3b8-161">*порт* — номер порта</span><span class="sxs-lookup"><span data-stu-id="ab3b8-161">*port* is the port number</span></span>
- <span data-ttu-id="ab3b8-162">«password» — это пароль, который определен в файле redis.conf.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-162">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="ab3b8-163">«AppName» — любая строка.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-163">"AppName" is any string.</span></span> <span data-ttu-id="ab3b8-164">SignalR создает канал Redis pub/sub с таким именем.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-164">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="ab3b8-165">Пример:</span><span class="sxs-lookup"><span data-stu-id="ab3b8-165">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="ab3b8-166">Развертывание и запуск приложения</span><span class="sxs-lookup"><span data-stu-id="ab3b8-166">Deploy and Run the Application</span></span>

<span data-ttu-id="ab3b8-167">Подготовка экземпляров Windows Server для развертывания приложения SignalR.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-167">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="ab3b8-168">Добавьте роль IIS.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-168">Add the IIS role.</span></span> <span data-ttu-id="ab3b8-169">Включить функции «Разработка приложений», включая протокол WebSocket.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-169">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="ab3b8-170">Также службы управления (см. в разделе «Средства управления»).</span><span class="sxs-lookup"><span data-stu-id="ab3b8-170">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="ab3b8-171">**Установить Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="ab3b8-171">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="ab3b8-172">Если запустить диспетчер служб IIS, он предложит вам установить веб-платформы Майкрософт или вы можете [загрузить intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="ab3b8-172">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="ab3b8-173">В установщике платформы найдите веб-развертывания и установить Web Deploy 3.0</span><span class="sxs-lookup"><span data-stu-id="ab3b8-173">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="ab3b8-174">Убедитесь, что веб-службы управления.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-174">Check that the Web Management Service is running.</span></span> <span data-ttu-id="ab3b8-175">В противном случае запустите службу.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-175">If not, start the service.</span></span> <span data-ttu-id="ab3b8-176">(Если вы не видите веб-службы управления в списке служб Windows, убедитесь, что вы установили службу управления, при добавлении роли IIS.)</span><span class="sxs-lookup"><span data-stu-id="ab3b8-176">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="ab3b8-177">Веб-служба управления по умолчанию прослушивает TCP-порт 8172.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-177">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="ab3b8-178">В брандмауэре Windows создайте новое входящее правило разрешает TCP-трафик через порт 8172.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-178">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="ab3b8-179">Дополнительные сведения см. в разделе [Настройка правил брандмауэра](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="ab3b8-179">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="ab3b8-180">(Если вы размещаете виртуальные машины в Azure, вы это можно сделать непосредственно на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-180">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="ab3b8-181">См. в разделе [Настройка конечных точек для виртуальной машины](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span><span class="sxs-lookup"><span data-stu-id="ab3b8-181">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="ab3b8-182">Теперь все готово для развертывания проекта Visual Studio с компьютера разработки на сервер.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-182">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="ab3b8-183">В обозревателе решений щелкните правой кнопкой мыши решение и нажмите кнопку **публикации**.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-183">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="ab3b8-184">Дополнительные сведения о веб-развертывания, см. в разделе [веб-развертывания содержимого карты для Visual Studio и ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="ab3b8-184">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="ab3b8-185">При развертывании приложения на двух серверах, можно открыть каждый экземпляр в отдельном окне браузера и см. в разделе, что каждый из них сообщения SignalR от другого.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-185">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="ab3b8-186">(Конечно, в рабочей среде, два сервера бы сидеть балансировщиком нагрузки.)</span><span class="sxs-lookup"><span data-stu-id="ab3b8-186">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="ab3b8-187">Если вы хотите просмотреть сообщения, отправляемые к Redis, можно использовать **redis-cli** клиент, который устанавливается вместе с Redis.</span><span class="sxs-lookup"><span data-stu-id="ab3b8-187">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
