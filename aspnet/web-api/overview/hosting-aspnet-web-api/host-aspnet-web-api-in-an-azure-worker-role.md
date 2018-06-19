---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Размещения ASP.NET Web API 2 в рабочей роли Azure | Документы Microsoft
author: MikeWasson
description: Этого учебника показано, как разместить веб-API ASP.NET в рабочей роли Azure, с помощью OWIN для самостоятельного размещения платформа веб-API. Открыть веб-интерфейс .NET (OWIN) de...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/02/2014
ms.topic: article
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 7ba1dc850e2f9d9c88e6ddf263a796e1867a98be
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873655"
---
<a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a><span data-ttu-id="60daf-104">Размещения ASP.NET Web API 2 в рабочей роли Azure</span><span class="sxs-lookup"><span data-stu-id="60daf-104">Host ASP.NET Web API 2 in an Azure Worker Role</span></span>
====================
<span data-ttu-id="60daf-105">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="60daf-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="60daf-106">Этого учебника показано, как разместить веб-API ASP.NET в рабочей роли Azure, с помощью OWIN для самостоятельного размещения платформа веб-API.</span><span class="sxs-lookup"><span data-stu-id="60daf-106">This tutorial shows how to host ASP.NET Web API in an Azure Worker Role, using OWIN to self-host the Web API framework.</span></span>
> 
> <span data-ttu-id="60daf-107">[Открыть веб-интерфейс .NET](http://owin.org/) (OWIN) определяет абстракцию между .NET веб-серверов и веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="60daf-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="60daf-108">OWIN разрывает связь веб-приложения с сервера, что делает OWIN идеальной для резидентного размещения веб-приложения в свой собственный процесс, за пределами служб IIS — например, внутри рабочей роли Azure.</span><span class="sxs-lookup"><span data-stu-id="60daf-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
> 
> <span data-ttu-id="60daf-109">В этом учебнике будет использоваться Microsoft.Owin.Host.HttpListener пакета, который предоставляет HTTP-сервер используется для резидентного размещения приложения OWIN.</span><span class="sxs-lookup"><span data-stu-id="60daf-109">In this tutorial, you'll use the Microsoft.Owin.Host.HttpListener package, which provides an HTTP server that be used to self-host OWIN applications.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="60daf-110">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="60daf-110">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="60daf-111">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="60daf-111">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="60daf-112">Веб-API 2</span><span class="sxs-lookup"><span data-stu-id="60daf-112">Web API 2</span></span>
> - [<span data-ttu-id="60daf-113">Azure SDK для .NET 2.3</span><span class="sxs-lookup"><span data-stu-id="60daf-113">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)


## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="60daf-114">Создание проекта Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="60daf-114">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="60daf-115">Запустите Visual Studio с правами администратора.</span><span class="sxs-lookup"><span data-stu-id="60daf-115">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="60daf-116">Для отладки приложения локально, с помощью эмулятора вычислений Azure требуются права администратора.</span><span class="sxs-lookup"><span data-stu-id="60daf-116">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="60daf-117">На **файл** меню, нажмите кнопку **New**, нажмите кнопку **проекта**.</span><span class="sxs-lookup"><span data-stu-id="60daf-117">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="60daf-118">Из **установленные шаблоны**, в Visual C#, нажмите кнопку **облака** и нажмите кнопку **облачных служб Windows Azure**.</span><span class="sxs-lookup"><span data-stu-id="60daf-118">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="60daf-119">Назовите проект «AzureApp» и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="60daf-119">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="60daf-120">В **новой облачной службы Windows Azure** диалоговое окно, дважды щелкните **рабочей роли**.</span><span class="sxs-lookup"><span data-stu-id="60daf-120">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="60daf-121">Оставьте имя по умолчанию («WorkerRole1»).</span><span class="sxs-lookup"><span data-stu-id="60daf-121">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="60daf-122">Этот шаг добавляет рабочей роли в решение.</span><span class="sxs-lookup"><span data-stu-id="60daf-122">This step adds a worker role to the solution.</span></span> <span data-ttu-id="60daf-123">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="60daf-123">Click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="60daf-124">Решение Visual Studio, которая создается содержит два проекта:</span><span class="sxs-lookup"><span data-stu-id="60daf-124">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="60daf-125">&quot;AzureApp&quot; определяет роли и конфигурации для приложения Azure.</span><span class="sxs-lookup"><span data-stu-id="60daf-125">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="60daf-126">&quot;WorkerRole1&quot; содержит код для рабочей роли.</span><span class="sxs-lookup"><span data-stu-id="60daf-126">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="60daf-127">Как правило приложение Azure может содержать несколько ролей, несмотря на то, что в этом учебнике используется одна роль.</span><span class="sxs-lookup"><span data-stu-id="60daf-127">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="60daf-128">Добавление веб-API и пакеты OWIN</span><span class="sxs-lookup"><span data-stu-id="60daf-128">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="60daf-129">Из **средства** меню, нажмите кнопку **диспетчер пакетов библиотеки**, нажмите кнопку **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="60daf-129">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="60daf-130">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="60daf-130">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="60daf-131">Добавить конечную точку HTTP</span><span class="sxs-lookup"><span data-stu-id="60daf-131">Add an HTTP Endpoint</span></span>

<span data-ttu-id="60daf-132">В обозревателе решений разверните проект AzureApp.</span><span class="sxs-lookup"><span data-stu-id="60daf-132">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="60daf-133">Разверните узел роли, щелкните правой кнопкой мыши WorkerRole1 и выберите **свойства**.</span><span class="sxs-lookup"><span data-stu-id="60daf-133">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="60daf-134">Нажмите кнопку **конечные точки**, а затем нажмите кнопку **добавить конечную точку**.</span><span class="sxs-lookup"><span data-stu-id="60daf-134">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="60daf-135">В **протокола** раскрывающемся списке выберите «http».</span><span class="sxs-lookup"><span data-stu-id="60daf-135">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="60daf-136">В **общий порт** и **частный порт**, введите 80.</span><span class="sxs-lookup"><span data-stu-id="60daf-136">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="60daf-137">Эти номера портов могут быть разными.</span><span class="sxs-lookup"><span data-stu-id="60daf-137">These port numbers can be different.</span></span> <span data-ttu-id="60daf-138">Открытый порт является то, что клиенты будут использовать при отправке запроса к роли.</span><span class="sxs-lookup"><span data-stu-id="60daf-138">The public port is what clients use when they send a request to the role.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="60daf-139">Настройка веб-API для резидентного размещения</span><span class="sxs-lookup"><span data-stu-id="60daf-139">Configure Web API for Self-Host</span></span>

<span data-ttu-id="60daf-140">В обозревателе решений щелкните правой кнопкой мыши проект WorkerRole1 и выберите **добавить** / **класса** для добавления нового класса.</span><span class="sxs-lookup"><span data-stu-id="60daf-140">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="60daf-141">Присвойте классу имя `Startup`.</span><span class="sxs-lookup"><span data-stu-id="60daf-141">Name the class `Startup`.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="60daf-142">Замените весь код шаблона в этом файле следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="60daf-142">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="60daf-143">Добавить контроллер Web API</span><span class="sxs-lookup"><span data-stu-id="60daf-143">Add a Web API Controller</span></span>

<span data-ttu-id="60daf-144">Добавьте класс контроллера веб-API.</span><span class="sxs-lookup"><span data-stu-id="60daf-144">Next, add a Web API controller class.</span></span> <span data-ttu-id="60daf-145">Щелкните правой кнопкой мыши проект WorkerRole1 и выберите **добавить** / **класса**.</span><span class="sxs-lookup"><span data-stu-id="60daf-145">Right-click the WorkerRole1 project and select **Add** / **Class**.</span></span> <span data-ttu-id="60daf-146">Имя класса TestController.</span><span class="sxs-lookup"><span data-stu-id="60daf-146">Name the class TestController.</span></span> <span data-ttu-id="60daf-147">Замените весь код шаблона в этом файле следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="60daf-147">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="60daf-148">Для простоты этот контроллер просто определяет два метода GET, которые возвращают обычного текста.</span><span class="sxs-lookup"><span data-stu-id="60daf-148">For simplicity, this controller just defines two GET methods that return plain text.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="60daf-149">Запустить узел OWIN</span><span class="sxs-lookup"><span data-stu-id="60daf-149">Start the OWIN Host</span></span>

<span data-ttu-id="60daf-150">Откройте файл WorkerRole.cs.</span><span class="sxs-lookup"><span data-stu-id="60daf-150">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="60daf-151">Этот класс определяет код, который выполняется при остановке и запуске рабочей роли.</span><span class="sxs-lookup"><span data-stu-id="60daf-151">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="60daf-152">Добавьте следующий код с помощью инструкции:</span><span class="sxs-lookup"><span data-stu-id="60daf-152">Add the following using statement:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="60daf-153">Добавить **IDisposable** члена `WorkerRole` класса:</span><span class="sxs-lookup"><span data-stu-id="60daf-153">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

<span data-ttu-id="60daf-154">В `OnStart` метода, добавьте следующий код для запуска узла:</span><span class="sxs-lookup"><span data-stu-id="60daf-154">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

<span data-ttu-id="60daf-155">**WebApp.Start** метод запускает узел OWIN.</span><span class="sxs-lookup"><span data-stu-id="60daf-155">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="60daf-156">Имя `Startup` класса является параметром типа метода.</span><span class="sxs-lookup"><span data-stu-id="60daf-156">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="60daf-157">По соглашению, узел будет вызывать `Configure` метода этого класса.</span><span class="sxs-lookup"><span data-stu-id="60daf-157">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="60daf-158">Переопределить `OnStop` для удаления  *\_приложения* экземпляр:</span><span class="sxs-lookup"><span data-stu-id="60daf-158">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="60daf-159">Ниже приведен полный код для WorkerRole.cs:</span><span class="sxs-lookup"><span data-stu-id="60daf-159">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

<span data-ttu-id="60daf-160">Выполните сборку решения и нажмите клавишу F5 для запуска приложения локально в эмуляторе вычислений Azure.</span><span class="sxs-lookup"><span data-stu-id="60daf-160">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="60daf-161">В зависимости от настроек брандмауэра может потребоваться разрешить эмулятор через брандмауэр.</span><span class="sxs-lookup"><span data-stu-id="60daf-161">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="60daf-162">Если вы получаете исключение, как показано ниже, см. в разделе [этой записи блога](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) для решения этой проблемы.</span><span class="sxs-lookup"><span data-stu-id="60daf-162">If you get an exception like the following, please see [this blog post](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) for a workaround.</span></span> <span data-ttu-id="60daf-163">«Не удалось загрузить файл или сборку ' Microsoft.Owin, версия = 2.0.2.0, язык и региональные параметры = neutral, PublicKeyToken = 31bf3856ad364e35 "или одну из ее зависимостей.</span><span class="sxs-lookup"><span data-stu-id="60daf-163">"Could not load file or assembly 'Microsoft.Owin, Version=2.0.2.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies.</span></span> <span data-ttu-id="60daf-164">Определение расположения сборки манифеста не соответствует ссылке сборки.</span><span class="sxs-lookup"><span data-stu-id="60daf-164">The located assembly's manifest definition does not match the assembly reference.</span></span> <span data-ttu-id="60daf-165">(Исключение из HRESULT: 0x80131040)»</span><span class="sxs-lookup"><span data-stu-id="60daf-165">(Exception from HRESULT: 0x80131040)"</span></span>


<span data-ttu-id="60daf-166">Эмулятор вычислений назначает локальный IP-адрес конечной точки.</span><span class="sxs-lookup"><span data-stu-id="60daf-166">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="60daf-167">IP-адрес можно найти, просмотрев пользовательский Интерфейс эмулятора вычислений.</span><span class="sxs-lookup"><span data-stu-id="60daf-167">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="60daf-168">Щелкните правой кнопкой мыши значок эмулятора в задаче панели области уведомлений и выберите **Показать пользовательский Интерфейс эмулятора вычислений**.</span><span class="sxs-lookup"><span data-stu-id="60daf-168">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

<span data-ttu-id="60daf-169">Поиск IP-адреса в развертываниях службы развертывания [id], сведения о службе.</span><span class="sxs-lookup"><span data-stu-id="60daf-169">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="60daf-170">Откройте веб-браузер и перейдите к http://<em>адрес</em>/test/1, где <em>адрес</em> IP-адрес, назначенный эмулятор вычислений; например, `http://127.0.0.1:80/test/1`.</span><span class="sxs-lookup"><span data-stu-id="60daf-170">Open a web browser and navigate to http://<em>address</em>/test/1, where <em>address</em> is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80/test/1`.</span></span> <span data-ttu-id="60daf-171">Вы увидите ответа от контроллера веб-API:</span><span class="sxs-lookup"><span data-stu-id="60daf-171">You should see the response from the Web API controller:</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="60daf-172">Развертывание в Azure</span><span class="sxs-lookup"><span data-stu-id="60daf-172">Deploy to Azure</span></span>

<span data-ttu-id="60daf-173">Для выполнения этого шага необходимо иметь учетную запись Azure.</span><span class="sxs-lookup"><span data-stu-id="60daf-173">For this step, you must have an Azure account.</span></span> <span data-ttu-id="60daf-174">Если у вас еще нет один, можно создать бесплатную пробную учетную запись всего за несколько минут.</span><span class="sxs-lookup"><span data-stu-id="60daf-174">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="60daf-175">Дополнительные сведения см. в разделе [бесплатная пробная версия Microsoft](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="60daf-175">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="60daf-176">В обозревателе решений щелкните правой кнопкой мыши проект AzureApp.</span><span class="sxs-lookup"><span data-stu-id="60daf-176">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="60daf-177">Нажмите **Публиковать**.</span><span class="sxs-lookup"><span data-stu-id="60daf-177">Select **Publish**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="60daf-178">Если вы не вошли учетную запись Azure, щелкните **входа**.</span><span class="sxs-lookup"><span data-stu-id="60daf-178">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

<span data-ttu-id="60daf-179">После входа, выберите подписку и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="60daf-179">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

<span data-ttu-id="60daf-180">Введите имя для облачной службы и выберите область.</span><span class="sxs-lookup"><span data-stu-id="60daf-180">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="60daf-181">Нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="60daf-181">Click **Create**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="60daf-182">Нажмите кнопку **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="60daf-182">Click **Publish**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

<span data-ttu-id="60daf-183">В окне Журнал действий Azure отображается ход выполнения развертывания.</span><span class="sxs-lookup"><span data-stu-id="60daf-183">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="60daf-184">При развертывании приложения, перейдите к http://appname.cloudapp.net/test/1.</span><span class="sxs-lookup"><span data-stu-id="60daf-184">When the app is deployed, browse to http://appname.cloudapp.net/test/1.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a><span data-ttu-id="60daf-185">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="60daf-185">Additional Resources</span></span>

- [<span data-ttu-id="60daf-186">Обзор проекта Katana</span><span class="sxs-lookup"><span data-stu-id="60daf-186">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [<span data-ttu-id="60daf-187">Katana проекта на GitHub</span><span class="sxs-lookup"><span data-stu-id="60daf-187">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana)
