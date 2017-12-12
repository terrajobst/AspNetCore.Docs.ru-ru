---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: "Разместить OWIN в рабочей роли Azure | Документы Microsoft"
author: MikeWasson
description: "Этот учебник демонстрирует резидентной OWIN в рабочей роли Microsoft Azure. Откройте веб-интерфейс .NET (OWIN) определяет абстракцию между .NET веб-сервера..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/11/2014
ms.topic: article
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 647514ae5a92b9d729179327fb97bd8005b0a4b2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="host-owin-in-an-azure-worker-role"></a><span data-ttu-id="f4226-104">Узел OWIN в рабочей роли Azure</span><span class="sxs-lookup"><span data-stu-id="f4226-104">Host OWIN in an Azure Worker Role</span></span>
====================
<span data-ttu-id="f4226-105">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f4226-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="f4226-106">Этот учебник демонстрирует резидентной OWIN в рабочей роли Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="f4226-106">This tutorial shows how to self-host OWIN in a Microsoft Azure worker role.</span></span>
> 
> <span data-ttu-id="f4226-107">[Открыть веб-интерфейс .NET](http://owin.org/) (OWIN) определяет абстракцию между .NET веб-серверов и веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="f4226-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="f4226-108">OWIN разрывает связь веб-приложения с сервера, что делает OWIN идеальной для резидентного размещения веб-приложения в свой собственный процесс, за пределами служб IIS — например, внутри рабочей роли Azure.</span><span class="sxs-lookup"><span data-stu-id="f4226-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
> 
> <span data-ttu-id="f4226-109">В этом учебнике вы узнаете, как для резидентных приложений OWIN в рабочей роли Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="f4226-109">In this tutorial, you'll learn how to self-host an OWIN applications inside a Microsoft Azure worker role.</span></span> <span data-ttu-id="f4226-110">Дополнительные сведения о рабочих ролей см. в разделе [модели выполнения Azure](https://azure.microsoft.com/en-us/documentation/articles/fundamentals-application-models/#CloudServices).</span><span class="sxs-lookup"><span data-stu-id="f4226-110">To learn more about worker roles, see [Azure Execution Models](https://azure.microsoft.com/en-us/documentation/articles/fundamentals-application-models/#CloudServices).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f4226-111">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="f4226-111">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="f4226-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="f4226-112">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [<span data-ttu-id="f4226-113">Azure SDK для .NET 2.3</span><span class="sxs-lookup"><span data-stu-id="f4226-113">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/en-us/downloads/)
> - [<span data-ttu-id="f4226-114">Microsoft.Owin.Selfhost 2.1.0</span><span class="sxs-lookup"><span data-stu-id="f4226-114">Microsoft.Owin.Selfhost 2.1.0</span></span>](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="f4226-115">Создание проекта Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="f4226-115">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="f4226-116">Запустите Visual Studio с правами администратора.</span><span class="sxs-lookup"><span data-stu-id="f4226-116">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="f4226-117">Для отладки приложения локально, с помощью эмулятора вычислений Azure требуются права администратора.</span><span class="sxs-lookup"><span data-stu-id="f4226-117">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="f4226-118">На **файл** меню, нажмите кнопку **New**, нажмите кнопку **проекта**.</span><span class="sxs-lookup"><span data-stu-id="f4226-118">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="f4226-119">Из **установленные шаблоны**, в Visual C#, нажмите кнопку **облака** и нажмите кнопку **облачных служб Windows Azure**.</span><span class="sxs-lookup"><span data-stu-id="f4226-119">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="f4226-120">Назовите проект «AzureApp» и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="f4226-120">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="f4226-121">В **новой облачной службы Windows Azure** диалоговое окно, дважды щелкните **рабочей роли**.</span><span class="sxs-lookup"><span data-stu-id="f4226-121">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="f4226-122">Оставьте имя по умолчанию («WorkerRole1»).</span><span class="sxs-lookup"><span data-stu-id="f4226-122">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="f4226-123">Этот шаг добавляет рабочей роли в решение.</span><span class="sxs-lookup"><span data-stu-id="f4226-123">This step adds a worker role to the solution.</span></span> <span data-ttu-id="f4226-124">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="f4226-124">Click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="f4226-125">Решение Visual Studio, которая создается содержит два проекта:</span><span class="sxs-lookup"><span data-stu-id="f4226-125">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="f4226-126">&quot;AzureApp&quot; определяет роли и конфигурации для приложения Azure.</span><span class="sxs-lookup"><span data-stu-id="f4226-126">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="f4226-127">&quot;WorkerRole1&quot; содержит код для рабочей роли.</span><span class="sxs-lookup"><span data-stu-id="f4226-127">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="f4226-128">Как правило приложение Azure может содержать несколько ролей, несмотря на то, что в этом учебнике используется одна роль.</span><span class="sxs-lookup"><span data-stu-id="f4226-128">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a><span data-ttu-id="f4226-129">Добавление пакетов резидентной OWIN</span><span class="sxs-lookup"><span data-stu-id="f4226-129">Add the OWIN Self-Host Packages</span></span>

<span data-ttu-id="f4226-130">Из **средства** меню, нажмите кнопку **диспетчер пакетов библиотеки**, нажмите кнопку **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="f4226-130">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="f4226-131">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="f4226-131">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="f4226-132">Добавить конечную точку HTTP</span><span class="sxs-lookup"><span data-stu-id="f4226-132">Add an HTTP Endpoint</span></span>

<span data-ttu-id="f4226-133">В обозревателе решений разверните проект AzureApp.</span><span class="sxs-lookup"><span data-stu-id="f4226-133">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="f4226-134">Разверните узел роли, щелкните правой кнопкой мыши WorkerRole1 и выберите **свойства**.</span><span class="sxs-lookup"><span data-stu-id="f4226-134">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="f4226-135">Нажмите кнопку **конечные точки**, а затем нажмите кнопку **добавить конечную точку**.</span><span class="sxs-lookup"><span data-stu-id="f4226-135">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="f4226-136">В **протокола** раскрывающемся списке выберите «http».</span><span class="sxs-lookup"><span data-stu-id="f4226-136">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="f4226-137">В **общий порт** и **частный порт**, введите 80.</span><span class="sxs-lookup"><span data-stu-id="f4226-137">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="f4226-138">Эти номера портов могут быть разными.</span><span class="sxs-lookup"><span data-stu-id="f4226-138">These port numbers can be different.</span></span> <span data-ttu-id="f4226-139">Открытый порт является то, что клиенты будут использовать при отправке запроса к роли.</span><span class="sxs-lookup"><span data-stu-id="f4226-139">The public port is what clients use when they send a request to the role.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a><span data-ttu-id="f4226-140">Создайте класс запуска OWIN</span><span class="sxs-lookup"><span data-stu-id="f4226-140">Create the OWIN Startup Class</span></span>

<span data-ttu-id="f4226-141">В обозревателе решений щелкните правой кнопкой мыши проект WorkerRole1 и выберите **добавить** / **класса** для добавления нового класса.</span><span class="sxs-lookup"><span data-stu-id="f4226-141">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="f4226-142">Присвойте классу имя `Startup`.</span><span class="sxs-lookup"><span data-stu-id="f4226-142">Name the class `Startup`.</span></span>

<span data-ttu-id="f4226-143">Замените все стандартный код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="f4226-143">Replace all of the boilerplate code with the following:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

<span data-ttu-id="f4226-144">`UseWelcomePage` Метод расширения добавляет простой HTML-страницы для приложения, чтобы проверить работы сайта.</span><span class="sxs-lookup"><span data-stu-id="f4226-144">The `UseWelcomePage` extension method adds a simple HTML page to your application, to verify the site is working.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="f4226-145">Запустить узел OWIN</span><span class="sxs-lookup"><span data-stu-id="f4226-145">Start the OWIN Host</span></span>

<span data-ttu-id="f4226-146">Откройте файл WorkerRole.cs.</span><span class="sxs-lookup"><span data-stu-id="f4226-146">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="f4226-147">Этот класс определяет код, который выполняется при остановке и запуске рабочей роли.</span><span class="sxs-lookup"><span data-stu-id="f4226-147">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="f4226-148">Добавьте следующий код с помощью инструкции:</span><span class="sxs-lookup"><span data-stu-id="f4226-148">Add the following using statement:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="f4226-149">Добавить **IDisposable** члена `WorkerRole` класса:</span><span class="sxs-lookup"><span data-stu-id="f4226-149">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="f4226-150">В `OnStart` метода, добавьте следующий код для запуска узла:</span><span class="sxs-lookup"><span data-stu-id="f4226-150">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

<span data-ttu-id="f4226-151">**WebApp.Start** метод запускает узел OWIN.</span><span class="sxs-lookup"><span data-stu-id="f4226-151">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="f4226-152">Имя `Startup` класса является параметром типа метода.</span><span class="sxs-lookup"><span data-stu-id="f4226-152">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="f4226-153">По соглашению, узел будет вызывать `Configure` метода этого класса.</span><span class="sxs-lookup"><span data-stu-id="f4226-153">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="f4226-154">Переопределить `OnStop` для удаления  *\_приложения* экземпляр:</span><span class="sxs-lookup"><span data-stu-id="f4226-154">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

<span data-ttu-id="f4226-155">Ниже приведен полный код для WorkerRole.cs:</span><span class="sxs-lookup"><span data-stu-id="f4226-155">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="f4226-156">Выполните сборку решения и нажмите клавишу F5 для запуска приложения локально в эмуляторе вычислений Azure.</span><span class="sxs-lookup"><span data-stu-id="f4226-156">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="f4226-157">В зависимости от настроек брандмауэра может потребоваться разрешить эмулятор через брандмауэр.</span><span class="sxs-lookup"><span data-stu-id="f4226-157">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

<span data-ttu-id="f4226-158">Эмулятор вычислений назначает локальный IP-адрес конечной точки.</span><span class="sxs-lookup"><span data-stu-id="f4226-158">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="f4226-159">IP-адрес можно найти, просмотрев пользовательский Интерфейс эмулятора вычислений.</span><span class="sxs-lookup"><span data-stu-id="f4226-159">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="f4226-160">Щелкните правой кнопкой мыши значок эмулятора в задаче панели области уведомлений и выберите **Показать пользовательский Интерфейс эмулятора вычислений**.</span><span class="sxs-lookup"><span data-stu-id="f4226-160">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="f4226-161">Поиск IP-адреса в развертываниях службы развертывания [id], сведения о службе.</span><span class="sxs-lookup"><span data-stu-id="f4226-161">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="f4226-162">Откройте веб-браузер и перейдите к http://*адрес*, где *адрес* IP-адрес, назначенный эмулятор вычислений; например, `http://127.0.0.1:80`.</span><span class="sxs-lookup"><span data-stu-id="f4226-162">Open a web browser and navigate to http://*address*, where *address* is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80`.</span></span> <span data-ttu-id="f4226-163">Вы увидите страницу приветствия OWIN:</span><span class="sxs-lookup"><span data-stu-id="f4226-163">You should see the OWIN welcome page:</span></span>

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="f4226-164">Развертывание в Azure</span><span class="sxs-lookup"><span data-stu-id="f4226-164">Deploy to Azure</span></span>

<span data-ttu-id="f4226-165">Для выполнения этого шага необходимо иметь учетную запись Azure.</span><span class="sxs-lookup"><span data-stu-id="f4226-165">For this step, you must have an Azure account.</span></span> <span data-ttu-id="f4226-166">Если у вас еще нет один, можно создать бесплатную пробную учетную запись всего за несколько минут.</span><span class="sxs-lookup"><span data-stu-id="f4226-166">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="f4226-167">Дополнительные сведения см. в разделе [бесплатная пробная версия Microsoft](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="f4226-167">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="f4226-168">В обозревателе решений щелкните правой кнопкой мыши проект AzureApp.</span><span class="sxs-lookup"><span data-stu-id="f4226-168">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="f4226-169">Нажмите **Публиковать**.</span><span class="sxs-lookup"><span data-stu-id="f4226-169">Select **Publish**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image12.png)

<span data-ttu-id="f4226-170">Если вы не вошли учетную запись Azure, щелкните **входа**.</span><span class="sxs-lookup"><span data-stu-id="f4226-170">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="f4226-171">После входа, выберите подписку и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="f4226-171">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

<span data-ttu-id="f4226-172">Введите имя для облачной службы и выберите область.</span><span class="sxs-lookup"><span data-stu-id="f4226-172">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="f4226-173">Нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="f4226-173">Click **Create**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image17.png)

<span data-ttu-id="f4226-174">Нажмите кнопку **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="f4226-174">Click **Publish**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="f4226-175">В окне Журнал действий Azure отображается ход выполнения развертывания.</span><span class="sxs-lookup"><span data-stu-id="f4226-175">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="f4226-176">При развертывании приложения, перейдите к `http://appname.cloudapp.net/`, где *appname* имя облачной службы.</span><span class="sxs-lookup"><span data-stu-id="f4226-176">When the app is deployed, browse to `http://appname.cloudapp.net/`, where *appname* is the name of your cloud service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f4226-177">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="f4226-177">Additional Resources</span></span>

- [<span data-ttu-id="f4226-178">Общие сведения о проекте Katana</span><span class="sxs-lookup"><span data-stu-id="f4226-178">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)
- [<span data-ttu-id="f4226-179">Katana проекта на GitHub</span><span class="sxs-lookup"><span data-stu-id="f4226-179">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana/)
