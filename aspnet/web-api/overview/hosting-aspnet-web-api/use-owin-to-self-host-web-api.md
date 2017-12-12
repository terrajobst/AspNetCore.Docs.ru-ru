---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: "Используйте OWIN для самостоятельного размещения ASP.NET Web API 2 | Документы Microsoft"
author: rick-anderson
description: "Этот учебник показывает, как для размещения в консольном приложении, с помощью OWIN для самостоятельного размещения платформа веб-API ASP.NET Web API. Открыть веб-интерфейс для .NET (OWIN) d..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2013
ms.topic: article
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: fda0db8155c3303907331a690af35f619b589154
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a><span data-ttu-id="7bf5a-104">Используйте OWIN для самостоятельного размещения ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="7bf5a-104">Use OWIN to Self-Host ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="7bf5a-105">по [Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span><span class="sxs-lookup"><span data-stu-id="7bf5a-105">by [Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span></span>

> <span data-ttu-id="7bf5a-106">Этот учебник показывает, как для размещения в консольном приложении, с помощью OWIN для самостоятельного размещения платформа веб-API ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="7bf5a-106">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
> 
> <span data-ttu-id="7bf5a-107">[Открыть веб-интерфейс .NET](http://owin.org) (OWIN) определяет абстракцию между .NET веб-серверов и веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="7bf5a-107">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="7bf5a-108">OWIN разрывает связь веб-приложения с сервера, благодаря чему идеально подходит для размещения веб-приложения в свой собственный процесс, за пределами служб IIS на собственном сервере OWIN.</span><span class="sxs-lookup"><span data-stu-id="7bf5a-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="7bf5a-109">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="7bf5a-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="7bf5a-110">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (также работает с Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="7bf5a-110">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (also works with Visual Studio 2012)</span></span>
> - <span data-ttu-id="7bf5a-111">Веб-API 2</span><span class="sxs-lookup"><span data-stu-id="7bf5a-111">Web API 2</span></span>


> [!NOTE]
> <span data-ttu-id="7bf5a-112">Можно найти полный исходный код для этого учебника в [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="7bf5a-112">You can find the complete source code for this tutorial at [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span></span>


## <a name="create-a-console-application"></a><span data-ttu-id="7bf5a-113">Создайте консольное приложение</span><span class="sxs-lookup"><span data-stu-id="7bf5a-113">Create a Console Application</span></span>

<span data-ttu-id="7bf5a-114">На **файл** меню, нажмите кнопку **New**, нажмите кнопку **проекта**.</span><span class="sxs-lookup"><span data-stu-id="7bf5a-114">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="7bf5a-115">Из **установленные шаблоны**, в Visual C#, нажмите кнопку **Windows** и нажмите кнопку **консольное приложение**.</span><span class="sxs-lookup"><span data-stu-id="7bf5a-115">From **Installed Templates**, under Visual C#, click **Windows** and then click **Console Application**.</span></span> <span data-ttu-id="7bf5a-116">Назовите проект «OwinSelfhostSample» и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="7bf5a-116">Name the project "OwinSelfhostSample" and click **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="7bf5a-117">Добавление веб-API и пакеты OWIN</span><span class="sxs-lookup"><span data-stu-id="7bf5a-117">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="7bf5a-118">Из **средства** меню, нажмите кнопку **диспетчер пакетов библиотеки**, нажмите кнопку **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="7bf5a-118">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span> <span data-ttu-id="7bf5a-119">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="7bf5a-119">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="7bf5a-120">Будет выполнена установка пакета selfhost WebAPI OWIN и все необходимые пакеты OWIN.</span><span class="sxs-lookup"><span data-stu-id="7bf5a-120">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="7bf5a-121">Настройка веб-API для резидентного размещения</span><span class="sxs-lookup"><span data-stu-id="7bf5a-121">Configure Web API for Self-Host</span></span>

<span data-ttu-id="7bf5a-122">В обозревателе решений щелкните правой кнопкой мыши проект и выберите **добавить** / **класса** для добавления нового класса.</span><span class="sxs-lookup"><span data-stu-id="7bf5a-122">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="7bf5a-123">Присвойте классу имя `Startup`.</span><span class="sxs-lookup"><span data-stu-id="7bf5a-123">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="7bf5a-124">Замените весь код шаблона в этом файле следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="7bf5a-124">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="7bf5a-125">Добавить контроллер Web API</span><span class="sxs-lookup"><span data-stu-id="7bf5a-125">Add a Web API Controller</span></span>

<span data-ttu-id="7bf5a-126">Добавьте класс контроллера веб-API.</span><span class="sxs-lookup"><span data-stu-id="7bf5a-126">Next, add a Web API controller class.</span></span> <span data-ttu-id="7bf5a-127">В обозревателе решений щелкните правой кнопкой мыши проект и выберите **добавить** / **класса** для добавления нового класса.</span><span class="sxs-lookup"><span data-stu-id="7bf5a-127">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="7bf5a-128">Присвойте классу имя `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="7bf5a-128">Name the class `ValuesController`.</span></span>

<span data-ttu-id="7bf5a-129">Замените весь код шаблона в этом файле следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="7bf5a-129">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a><span data-ttu-id="7bf5a-130">Запустить узел OWIN и выполните запрос с помощью HttpClient</span><span class="sxs-lookup"><span data-stu-id="7bf5a-130">Start the OWIN Host and Make a Request Using HttpClient</span></span>

<span data-ttu-id="7bf5a-131">Замените весь код шаблона в файле Program.cs следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="7bf5a-131">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a><span data-ttu-id="7bf5a-132">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="7bf5a-132">Running the Application</span></span>

<span data-ttu-id="7bf5a-133">Чтобы запустить приложение, нажмите клавишу F5 в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7bf5a-133">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="7bf5a-134">Этот вывод должен выглядеть так:</span><span class="sxs-lookup"><span data-stu-id="7bf5a-134">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="7bf5a-135">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="7bf5a-135">Additional Resources</span></span>

[<span data-ttu-id="7bf5a-136">Общие сведения о проекте Katana</span><span class="sxs-lookup"><span data-stu-id="7bf5a-136">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="7bf5a-137">Размещения веб-API ASP.NET в рабочей роли Azure</span><span class="sxs-lookup"><span data-stu-id="7bf5a-137">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
