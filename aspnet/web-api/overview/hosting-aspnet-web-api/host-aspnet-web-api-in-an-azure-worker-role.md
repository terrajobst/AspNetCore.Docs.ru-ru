---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Размещение ASP.NET Web API 2 в рабочей роли Azure | Документация Майкрософт
author: MikeWasson
description: Этом руководстве показано, как разместить веб-API ASP.NET в рабочей роли Azure, с помощью OWIN для резидентного размещения платформа веб-API. Откройте веб-интерфейс для .NET (OWIN) de...
ms.author: aspnetcontent
ms.date: 04/02/2014
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: c53256b8a72a377f51b9fbac7944657cb6d4c6e4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803854"
---
<a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a><span data-ttu-id="e4ec2-104">Размещение ASP.NET Web API 2 в рабочей роли Azure</span><span class="sxs-lookup"><span data-stu-id="e4ec2-104">Host ASP.NET Web API 2 in an Azure Worker Role</span></span>
====================
<span data-ttu-id="e4ec2-105">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e4ec2-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="e4ec2-106">Этом руководстве показано, как разместить веб-API ASP.NET в рабочей роли Azure, с помощью OWIN для резидентного размещения платформа веб-API.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-106">This tutorial shows how to host ASP.NET Web API in an Azure Worker Role, using OWIN to self-host the Web API framework.</span></span>
> 
> <span data-ttu-id="e4ec2-107">[Открыть веб-интерфейс .NET](http://owin.org/) (OWIN) определяет абстракции между веб-серверов .NET и веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="e4ec2-108">OWIN отделяет веб-приложения на сервер, который идеально OWIN для резидентного размещения веб-приложения в собственном процессе, за пределами IIS — например, внутри рабочей роли Azure.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
> 
> <span data-ttu-id="e4ec2-109">В этом руководстве вы используете пакет Microsoft.Owin.Host.HttpListener, предоставляющую HTTP-сервер, который используется для резидентного размещения приложения OWIN.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-109">In this tutorial, you'll use the Microsoft.Owin.Host.HttpListener package, which provides an HTTP server that be used to self-host OWIN applications.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="e4ec2-110">Версии программного обеспечения, используемые в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="e4ec2-110">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="e4ec2-111">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="e4ec2-111">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="e4ec2-112">Веб-API 2</span><span class="sxs-lookup"><span data-stu-id="e4ec2-112">Web API 2</span></span>
> - [<span data-ttu-id="e4ec2-113">Пакет Azure SDK для .NET 2.3</span><span class="sxs-lookup"><span data-stu-id="e4ec2-113">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)


## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="e4ec2-114">Создание проекта Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="e4ec2-114">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="e4ec2-115">Запустите Visual Studio с правами администратора.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-115">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="e4ec2-116">Для отладки приложения в локальной среде, с помощью эмулятора вычислений Azure требуются права администратора.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-116">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="e4ec2-117">На **файл** меню, щелкните **New**, затем нажмите кнопку **проекта**.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-117">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="e4ec2-118">Из **установленные шаблоны**, в разделе Visual C#, щелкните **Cloud** и нажмите кнопку **облачной службы Windows Azure**.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-118">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="e4ec2-119">Присвойте проекту имя «AzureApp» и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-119">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="e4ec2-120">В **новая облачная служба Windows Azure** диалоговое окно, дважды щелкните **рабочей роли**.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-120">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="e4ec2-121">Оставьте имя по умолчанию («WorkerRole1»).</span><span class="sxs-lookup"><span data-stu-id="e4ec2-121">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="e4ec2-122">Этот шаг добавляет рабочей роли в решение.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-122">This step adds a worker role to the solution.</span></span> <span data-ttu-id="e4ec2-123">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-123">Click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="e4ec2-124">Решение Visual Studio, который создается содержит два проекта:</span><span class="sxs-lookup"><span data-stu-id="e4ec2-124">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="e4ec2-125">&quot;AzureApp&quot; определяет роли и конфигурации для приложения Azure.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-125">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="e4ec2-126">&quot;WorkerRole1&quot; содержит код для рабочей роли.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-126">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="e4ec2-127">Как правило приложение Azure может содержать несколько ролей, несмотря на то, что в этом руководстве используется одна роль.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-127">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="e4ec2-128">Добавьте веб-API и пакеты OWIN</span><span class="sxs-lookup"><span data-stu-id="e4ec2-128">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="e4ec2-129">Из **средства** меню, щелкните **диспетчер пакетов библиотеки**, затем нажмите кнопку **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-129">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="e4ec2-130">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="e4ec2-130">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="e4ec2-131">Добавить конечную точку HTTP</span><span class="sxs-lookup"><span data-stu-id="e4ec2-131">Add an HTTP Endpoint</span></span>

<span data-ttu-id="e4ec2-132">В обозревателе решений разверните проект AzureApp.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-132">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="e4ec2-133">Разверните узел роли, щелкните правой кнопкой мыши WorkerRole1 и выберите **свойства**.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-133">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="e4ec2-134">Нажмите кнопку **конечные точки**, а затем нажмите кнопку **добавить конечную точку**.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-134">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="e4ec2-135">В **протокола** раскрывающегося списка выберите «http».</span><span class="sxs-lookup"><span data-stu-id="e4ec2-135">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="e4ec2-136">В **общий порт** и **частный порт**, введите 80.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-136">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="e4ec2-137">Эти номера портов могут отличаться.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-137">These port numbers can be different.</span></span> <span data-ttu-id="e4ec2-138">Общий порт — новые клиенты будут использовать при отправке запроса к роли.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-138">The public port is what clients use when they send a request to the role.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="e4ec2-139">Настройка веб-API для резидентного размещения</span><span class="sxs-lookup"><span data-stu-id="e4ec2-139">Configure Web API for Self-Host</span></span>

<span data-ttu-id="e4ec2-140">В обозревателе решений щелкните правой кнопкой мыши проект WorkerRole1 и выберите **добавить** / **класс** для добавления нового класса.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-140">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="e4ec2-141">Присвойте классу имя `Startup`.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-141">Name the class `Startup`.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="e4ec2-142">Замените весь код шаблона в этом файле следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="e4ec2-142">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="e4ec2-143">Добавить контроллер веб-API</span><span class="sxs-lookup"><span data-stu-id="e4ec2-143">Add a Web API Controller</span></span>

<span data-ttu-id="e4ec2-144">Добавьте класс контроллера веб-API.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-144">Next, add a Web API controller class.</span></span> <span data-ttu-id="e4ec2-145">Щелкните правой кнопкой мыши проект WorkerRole1 и выберите **добавить** / **класс**.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-145">Right-click the WorkerRole1 project and select **Add** / **Class**.</span></span> <span data-ttu-id="e4ec2-146">Имя класса TestController.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-146">Name the class TestController.</span></span> <span data-ttu-id="e4ec2-147">Замените весь код шаблона в этом файле следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="e4ec2-147">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="e4ec2-148">Для простоты этот контроллер определяет только два метода GET, возвращающие обычного текста.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-148">For simplicity, this controller just defines two GET methods that return plain text.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="e4ec2-149">Запустить узел OWIN</span><span class="sxs-lookup"><span data-stu-id="e4ec2-149">Start the OWIN Host</span></span>

<span data-ttu-id="e4ec2-150">Откройте файл WorkerRole.cs.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-150">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="e4ec2-151">Этот класс определяет код, выполняемый при рабочей роли запущено и остановлено.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-151">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="e4ec2-152">Добавьте следующий оператор using:</span><span class="sxs-lookup"><span data-stu-id="e4ec2-152">Add the following using statement:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="e4ec2-153">Добавить **IDisposable** члена `WorkerRole` класса:</span><span class="sxs-lookup"><span data-stu-id="e4ec2-153">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

<span data-ttu-id="e4ec2-154">В `OnStart` метод, добавьте следующий код для запуска узла:</span><span class="sxs-lookup"><span data-stu-id="e4ec2-154">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

<span data-ttu-id="e4ec2-155">**WebApp.Start** метод начинает хоста OWIN.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-155">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="e4ec2-156">Имя `Startup` класс является параметром типа метода.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-156">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="e4ec2-157">По соглашению, главное приложение выполняет вызов `Configure` метод этого класса.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-157">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="e4ec2-158">Переопределить `OnStop` для удаления  *\_приложения* экземпляр:</span><span class="sxs-lookup"><span data-stu-id="e4ec2-158">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="e4ec2-159">Ниже приведен полный код для WorkerRole.cs:</span><span class="sxs-lookup"><span data-stu-id="e4ec2-159">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

<span data-ttu-id="e4ec2-160">Выполните сборку решения и нажмите клавишу F5 для запуска приложения локально в эмуляторе вычислений Azure.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-160">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="e4ec2-161">В зависимости от настроек брандмауэра может потребоваться предоставление эмулятору через брандмауэр.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-161">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="e4ec2-162">Если вы получаете исключение, как показано ниже, см. раздел [этой записи блога](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) решение этой проблемы.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-162">If you get an exception like the following, please see [this blog post](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) for a workaround.</span></span> <span data-ttu-id="e4ec2-163">«Не удалось загрузить файл или сборку "Microsoft.Owin, Version = 2.0.2.0, язык и региональные параметры = neutral, PublicKeyToken = 31bf3856ad364e35" или одну из ее зависимостей.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-163">"Could not load file or assembly 'Microsoft.Owin, Version=2.0.2.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies.</span></span> <span data-ttu-id="e4ec2-164">Определение манифеста расположены сборки не соответствует ссылки на сборку.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-164">The located assembly's manifest definition does not match the assembly reference.</span></span> <span data-ttu-id="e4ec2-165">(Исключение из HRESULT: 0x80131040)»</span><span class="sxs-lookup"><span data-stu-id="e4ec2-165">(Exception from HRESULT: 0x80131040)"</span></span>


<span data-ttu-id="e4ec2-166">Эмулятор вычислений назначает локальный IP-адрес конечной точки.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-166">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="e4ec2-167">IP-адрес можно найти, просмотрев пользовательский Интерфейс эмулятора вычислений.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-167">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="e4ec2-168">Щелкните правой кнопкой мыши значок эмулятора в задаче области уведомлений и выберите **Показать пользовательский Интерфейс эмулятора вычислений**.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-168">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

<span data-ttu-id="e4ec2-169">Найти IP-адрес в группе развертывания служб, развертывания [id], сведения о службе.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-169">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="e4ec2-170">Откройте веб-браузер и перейдите на http://<em>адрес</em>/test/1, где <em>адрес</em> IP-адрес, назначенный с помощью эмулятора вычислений; например, `http://127.0.0.1:80/test/1`.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-170">Open a web browser and navigate to http://<em>address</em>/test/1, where <em>address</em> is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80/test/1`.</span></span> <span data-ttu-id="e4ec2-171">Вы увидите ответ от контроллера веб-API:</span><span class="sxs-lookup"><span data-stu-id="e4ec2-171">You should see the response from the Web API controller:</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="e4ec2-172">Развертывание в Azure</span><span class="sxs-lookup"><span data-stu-id="e4ec2-172">Deploy to Azure</span></span>

<span data-ttu-id="e4ec2-173">Для выполнения этого шага необходимо иметь учетную запись Azure.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-173">For this step, you must have an Azure account.</span></span> <span data-ttu-id="e4ec2-174">Если ее еще нет, можно создать бесплатную пробную учетную запись всего за несколько минут.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-174">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="e4ec2-175">Дополнительные сведения см. в разделе [бесплатной пробной версии Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="e4ec2-175">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="e4ec2-176">В обозревателе решений щелкните правой кнопкой мыши проект AzureApp.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-176">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="e4ec2-177">Нажмите **Публиковать**.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-177">Select **Publish**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="e4ec2-178">Если вы не вошли учетную запись Azure, щелкните **Sign In**.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-178">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

<span data-ttu-id="e4ec2-179">После входа, выберите подписку и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-179">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

<span data-ttu-id="e4ec2-180">Введите имя для облачной службы и выбрать регион.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-180">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="e4ec2-181">Нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-181">Click **Create**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="e4ec2-182">Нажмите кнопку **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-182">Click **Publish**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

<span data-ttu-id="e4ec2-183">Окно журнала действий Azure показывает ход выполнения развертывания.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-183">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="e4ec2-184">При развертывании приложения, перейдите к http://appname.cloudapp.net/test/1.</span><span class="sxs-lookup"><span data-stu-id="e4ec2-184">When the app is deployed, browse to http://appname.cloudapp.net/test/1.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a><span data-ttu-id="e4ec2-185">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="e4ec2-185">Additional Resources</span></span>

- [<span data-ttu-id="e4ec2-186">Обзор проекта Katana</span><span class="sxs-lookup"><span data-stu-id="e4ec2-186">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [<span data-ttu-id="e4ec2-187">Проект Katana на GitHub</span><span class="sxs-lookup"><span data-stu-id="e4ec2-187">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana)
