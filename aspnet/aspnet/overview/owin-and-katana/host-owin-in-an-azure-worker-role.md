---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: Размещение OWIN в рабочей роли Azure | Документация Майкрософт
author: MikeWasson
description: Этом руководстве показано, как для саморазмещения OWIN в рабочей роли Microsoft Azure. Откройте веб-интерфейс для .NET (OWIN) определяет абстракции между .NET веб-сервер...
ms.author: riande
ms.date: 04/11/2014
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: dbf0964695dd2592d063b05c0778923edffe8e2e
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910781"
---
<a name="host-owin-in-an-azure-worker-role"></a><span data-ttu-id="bc67f-104">Размещение OWIN в рабочей роли Azure</span><span class="sxs-lookup"><span data-stu-id="bc67f-104">Host OWIN in an Azure Worker Role</span></span>
====================
<span data-ttu-id="bc67f-105">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bc67f-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="bc67f-106">Этом руководстве показано, как для саморазмещения OWIN в рабочей роли Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="bc67f-106">This tutorial shows how to self-host OWIN in a Microsoft Azure worker role.</span></span>
>
> <span data-ttu-id="bc67f-107">[Открыть веб-интерфейс .NET](http://owin.org/) (OWIN) определяет абстракции между веб-серверов .NET и веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="bc67f-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="bc67f-108">OWIN отделяет веб-приложения на сервер, который идеально OWIN для резидентного размещения веб-приложения в собственном процессе, за пределами IIS — например, внутри рабочей роли Azure.</span><span class="sxs-lookup"><span data-stu-id="bc67f-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
>
> <span data-ttu-id="bc67f-109">В этом руководстве вы узнаете, как для резидентного размещения приложений OWIN в рабочей роли Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="bc67f-109">In this tutorial, you'll learn how to self-host an OWIN applications inside a Microsoft Azure worker role.</span></span> <span data-ttu-id="bc67f-110">Дополнительные сведения о рабочих ролей, см. в разделе [модели выполнения Azure](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).</span><span class="sxs-lookup"><span data-stu-id="bc67f-110">To learn more about worker roles, see [Azure Execution Models](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="bc67f-111">Версии программного обеспечения, используемые в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="bc67f-111">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="bc67f-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="bc67f-112">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - [<span data-ttu-id="bc67f-113">Пакет Azure SDK для .NET 2.3</span><span class="sxs-lookup"><span data-stu-id="bc67f-113">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)
> - [<span data-ttu-id="bc67f-114">Microsoft.Owin.Selfhost 2.1.0</span><span class="sxs-lookup"><span data-stu-id="bc67f-114">Microsoft.Owin.Selfhost 2.1.0</span></span>](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="bc67f-115">Создание проекта Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="bc67f-115">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="bc67f-116">Запустите Visual Studio с правами администратора.</span><span class="sxs-lookup"><span data-stu-id="bc67f-116">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="bc67f-117">Для отладки приложения в локальной среде, с помощью эмулятора вычислений Azure требуются права администратора.</span><span class="sxs-lookup"><span data-stu-id="bc67f-117">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="bc67f-118">На **файл** меню, щелкните **New**, затем нажмите кнопку **проекта**.</span><span class="sxs-lookup"><span data-stu-id="bc67f-118">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="bc67f-119">Из **установленные шаблоны**, в разделе Visual C#, щелкните **Cloud** и нажмите кнопку **облачной службы Windows Azure**.</span><span class="sxs-lookup"><span data-stu-id="bc67f-119">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="bc67f-120">Присвойте проекту имя «AzureApp» и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="bc67f-120">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="bc67f-121">В **новая облачная служба Windows Azure** диалоговое окно, дважды щелкните **рабочей роли**.</span><span class="sxs-lookup"><span data-stu-id="bc67f-121">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="bc67f-122">Оставьте имя по умолчанию («WorkerRole1»).</span><span class="sxs-lookup"><span data-stu-id="bc67f-122">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="bc67f-123">Этот шаг добавляет рабочей роли в решение.</span><span class="sxs-lookup"><span data-stu-id="bc67f-123">This step adds a worker role to the solution.</span></span> <span data-ttu-id="bc67f-124">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="bc67f-124">Click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="bc67f-125">Решение Visual Studio, который создается содержит два проекта:</span><span class="sxs-lookup"><span data-stu-id="bc67f-125">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="bc67f-126">&quot;AzureApp&quot; определяет роли и конфигурации для приложения Azure.</span><span class="sxs-lookup"><span data-stu-id="bc67f-126">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="bc67f-127">&quot;WorkerRole1&quot; содержит код для рабочей роли.</span><span class="sxs-lookup"><span data-stu-id="bc67f-127">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="bc67f-128">Как правило приложение Azure может содержать несколько ролей, несмотря на то, что в этом руководстве используется одна роль.</span><span class="sxs-lookup"><span data-stu-id="bc67f-128">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a><span data-ttu-id="bc67f-129">Добавьте пакеты резидентного размещения OWIN</span><span class="sxs-lookup"><span data-stu-id="bc67f-129">Add the OWIN Self-Host Packages</span></span>

<span data-ttu-id="bc67f-130">Из **средства** меню, щелкните **диспетчер пакетов NuGet**, затем нажмите кнопку **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="bc67f-130">From the **Tools** menu, click **NuGet Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="bc67f-131">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="bc67f-131">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="bc67f-132">Добавить конечную точку HTTP</span><span class="sxs-lookup"><span data-stu-id="bc67f-132">Add an HTTP Endpoint</span></span>

<span data-ttu-id="bc67f-133">В обозревателе решений разверните проект AzureApp.</span><span class="sxs-lookup"><span data-stu-id="bc67f-133">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="bc67f-134">Разверните узел роли, щелкните правой кнопкой мыши WorkerRole1 и выберите **свойства**.</span><span class="sxs-lookup"><span data-stu-id="bc67f-134">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="bc67f-135">Нажмите кнопку **конечные точки**, а затем нажмите кнопку **добавить конечную точку**.</span><span class="sxs-lookup"><span data-stu-id="bc67f-135">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="bc67f-136">В **протокола** раскрывающегося списка выберите «http».</span><span class="sxs-lookup"><span data-stu-id="bc67f-136">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="bc67f-137">В **общий порт** и **частный порт**, введите 80.</span><span class="sxs-lookup"><span data-stu-id="bc67f-137">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="bc67f-138">Эти номера портов могут отличаться.</span><span class="sxs-lookup"><span data-stu-id="bc67f-138">These port numbers can be different.</span></span> <span data-ttu-id="bc67f-139">Общий порт — новые клиенты будут использовать при отправке запроса к роли.</span><span class="sxs-lookup"><span data-stu-id="bc67f-139">The public port is what clients use when they send a request to the role.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a><span data-ttu-id="bc67f-140">Создать класс запуска OWIN</span><span class="sxs-lookup"><span data-stu-id="bc67f-140">Create the OWIN Startup Class</span></span>

<span data-ttu-id="bc67f-141">В обозревателе решений щелкните правой кнопкой мыши проект WorkerRole1 и выберите **добавить** / **класс** для добавления нового класса.</span><span class="sxs-lookup"><span data-stu-id="bc67f-141">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="bc67f-142">Присвойте классу имя `Startup`.</span><span class="sxs-lookup"><span data-stu-id="bc67f-142">Name the class `Startup`.</span></span>

<span data-ttu-id="bc67f-143">Замените весь код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="bc67f-143">Replace all of the boilerplate code with the following:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

<span data-ttu-id="bc67f-144">`UseWelcomePage` Добавляет метод расширения простую HTML-страницу приложения, чтобы проверить, работает сайт.</span><span class="sxs-lookup"><span data-stu-id="bc67f-144">The `UseWelcomePage` extension method adds a simple HTML page to your application, to verify the site is working.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="bc67f-145">Запустить узел OWIN</span><span class="sxs-lookup"><span data-stu-id="bc67f-145">Start the OWIN Host</span></span>

<span data-ttu-id="bc67f-146">Откройте файл WorkerRole.cs.</span><span class="sxs-lookup"><span data-stu-id="bc67f-146">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="bc67f-147">Этот класс определяет код, выполняемый при рабочей роли запущено и остановлено.</span><span class="sxs-lookup"><span data-stu-id="bc67f-147">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="bc67f-148">Добавьте следующий оператор using:</span><span class="sxs-lookup"><span data-stu-id="bc67f-148">Add the following using statement:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="bc67f-149">Добавить **IDisposable** члена `WorkerRole` класса:</span><span class="sxs-lookup"><span data-stu-id="bc67f-149">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="bc67f-150">В `OnStart` метод, добавьте следующий код для запуска узла:</span><span class="sxs-lookup"><span data-stu-id="bc67f-150">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

<span data-ttu-id="bc67f-151">**WebApp.Start** метод начинает хоста OWIN.</span><span class="sxs-lookup"><span data-stu-id="bc67f-151">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="bc67f-152">Имя `Startup` класс является параметром типа метода.</span><span class="sxs-lookup"><span data-stu-id="bc67f-152">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="bc67f-153">По соглашению, главное приложение выполняет вызов `Configure` метод этого класса.</span><span class="sxs-lookup"><span data-stu-id="bc67f-153">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="bc67f-154">Переопределить `OnStop` для удаления  *\_приложения* экземпляр:</span><span class="sxs-lookup"><span data-stu-id="bc67f-154">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

<span data-ttu-id="bc67f-155">Ниже приведен полный код для WorkerRole.cs:</span><span class="sxs-lookup"><span data-stu-id="bc67f-155">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="bc67f-156">Выполните сборку решения и нажмите клавишу F5 для запуска приложения локально в эмуляторе вычислений Azure.</span><span class="sxs-lookup"><span data-stu-id="bc67f-156">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="bc67f-157">В зависимости от настроек брандмауэра может потребоваться предоставление эмулятору через брандмауэр.</span><span class="sxs-lookup"><span data-stu-id="bc67f-157">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

<span data-ttu-id="bc67f-158">Эмулятор вычислений назначает локальный IP-адрес конечной точки.</span><span class="sxs-lookup"><span data-stu-id="bc67f-158">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="bc67f-159">IP-адрес можно найти, просмотрев пользовательский Интерфейс эмулятора вычислений.</span><span class="sxs-lookup"><span data-stu-id="bc67f-159">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="bc67f-160">Щелкните правой кнопкой мыши значок эмулятора в задаче области уведомлений и выберите **Показать пользовательский Интерфейс эмулятора вычислений**.</span><span class="sxs-lookup"><span data-stu-id="bc67f-160">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="bc67f-161">Найти IP-адрес в группе развертывания служб, развертывания [id], сведения о службе.</span><span class="sxs-lookup"><span data-stu-id="bc67f-161">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="bc67f-162">Откройте веб-браузер и перейдите на http://<em>адрес</em>, где <em>адрес</em> IP-адрес, назначенный с помощью эмулятора вычислений; например, `http://127.0.0.1:80`.</span><span class="sxs-lookup"><span data-stu-id="bc67f-162">Open a web browser and navigate to http://<em>address</em>, where <em>address</em> is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80`.</span></span> <span data-ttu-id="bc67f-163">Вы должны увидеть страницу приветствия OWIN:</span><span class="sxs-lookup"><span data-stu-id="bc67f-163">You should see the OWIN welcome page:</span></span>

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="bc67f-164">Развертывание в Azure</span><span class="sxs-lookup"><span data-stu-id="bc67f-164">Deploy to Azure</span></span>

<span data-ttu-id="bc67f-165">Для выполнения этого шага необходимо иметь учетную запись Azure.</span><span class="sxs-lookup"><span data-stu-id="bc67f-165">For this step, you must have an Azure account.</span></span> <span data-ttu-id="bc67f-166">Если ее еще нет, можно создать бесплатную пробную учетную запись всего за несколько минут.</span><span class="sxs-lookup"><span data-stu-id="bc67f-166">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="bc67f-167">Дополнительные сведения см. в разделе [бесплатной пробной версии Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="bc67f-167">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="bc67f-168">В обозревателе решений щелкните правой кнопкой мыши проект AzureApp.</span><span class="sxs-lookup"><span data-stu-id="bc67f-168">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="bc67f-169">Нажмите **Публиковать**.</span><span class="sxs-lookup"><span data-stu-id="bc67f-169">Select **Publish**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image12.png)

<span data-ttu-id="bc67f-170">Если вы не вошли учетную запись Azure, щелкните **Sign In**.</span><span class="sxs-lookup"><span data-stu-id="bc67f-170">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="bc67f-171">После входа, выберите подписку и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="bc67f-171">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

<span data-ttu-id="bc67f-172">Введите имя для облачной службы и выбрать регион.</span><span class="sxs-lookup"><span data-stu-id="bc67f-172">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="bc67f-173">Нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="bc67f-173">Click **Create**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image17.png)

<span data-ttu-id="bc67f-174">Нажмите кнопку **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="bc67f-174">Click **Publish**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="bc67f-175">Окно журнала действий Azure показывает ход выполнения развертывания.</span><span class="sxs-lookup"><span data-stu-id="bc67f-175">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="bc67f-176">При развертывании приложения, перейдите к `http://appname.cloudapp.net/`, где *appname* имя облачной службы.</span><span class="sxs-lookup"><span data-stu-id="bc67f-176">When the app is deployed, browse to `http://appname.cloudapp.net/`, where *appname* is the name of your cloud service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bc67f-177">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="bc67f-177">Additional Resources</span></span>

- [<span data-ttu-id="bc67f-178">Обзор проекта Katana</span><span class="sxs-lookup"><span data-stu-id="bc67f-178">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)
- [<span data-ttu-id="bc67f-179">Проект Katana на GitHub</span><span class="sxs-lookup"><span data-stu-id="bc67f-179">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana/)
