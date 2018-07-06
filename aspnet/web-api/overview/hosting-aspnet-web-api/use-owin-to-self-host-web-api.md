---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Использование OWIN для резидентного размещения веб-API 2 ASP.NET | Документация Майкрософт
author: rick-anderson
description: Этом руководстве показано, как разместить веб-API ASP.NET в консольном приложении, с помощью OWIN для резидентного размещения платформа веб-API. Откройте веб-интерфейс для .NET (OWIN) d...
ms.author: aspnetcontent
ms.date: 07/09/2013
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 9fba2774e3873d32115a14fa0c84b99466eda04f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830915"
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a><span data-ttu-id="6e780-104">Использование OWIN для резидентного размещения веб-API 2 ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6e780-104">Use OWIN to Self-Host ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="6e780-105">по [Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span><span class="sxs-lookup"><span data-stu-id="6e780-105">by [Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span></span>

> <span data-ttu-id="6e780-106">Этом руководстве показано, как разместить веб-API ASP.NET в консольном приложении, с помощью OWIN для резидентного размещения платформа веб-API.</span><span class="sxs-lookup"><span data-stu-id="6e780-106">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
> 
> <span data-ttu-id="6e780-107">[Открыть веб-интерфейс .NET](http://owin.org) (OWIN) определяет абстракции между веб-серверов .NET и веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="6e780-107">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="6e780-108">OWIN отделяет веб-приложения на сервер, который идеально OWIN для резидентного размещения веб-приложения в собственном процессе, за пределами служб IIS.</span><span class="sxs-lookup"><span data-stu-id="6e780-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6e780-109">Версии программного обеспечения, используемые в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="6e780-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="6e780-110">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (также работает с Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="6e780-110">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (also works with Visual Studio 2012)</span></span>
> - <span data-ttu-id="6e780-111">Веб-API 2</span><span class="sxs-lookup"><span data-stu-id="6e780-111">Web API 2</span></span>


> [!NOTE]
> <span data-ttu-id="6e780-112">Полный исходный код для выполнения инструкций этого руководства можно найти [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="6e780-112">You can find the complete source code for this tutorial at [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span></span>


## <a name="create-a-console-application"></a><span data-ttu-id="6e780-113">Создайте консольное приложение</span><span class="sxs-lookup"><span data-stu-id="6e780-113">Create a Console Application</span></span>

<span data-ttu-id="6e780-114">На **файл** меню, щелкните **New**, затем нажмите кнопку **проекта**.</span><span class="sxs-lookup"><span data-stu-id="6e780-114">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="6e780-115">Из **установленные шаблоны**, в разделе Visual C#, щелкните **Windows** и нажмите кнопку **консольное приложение**.</span><span class="sxs-lookup"><span data-stu-id="6e780-115">From **Installed Templates**, under Visual C#, click **Windows** and then click **Console Application**.</span></span> <span data-ttu-id="6e780-116">Присвойте проекту имя «OwinSelfhostSample» и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="6e780-116">Name the project "OwinSelfhostSample" and click **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="6e780-117">Добавьте веб-API и пакеты OWIN</span><span class="sxs-lookup"><span data-stu-id="6e780-117">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="6e780-118">Из **средства** меню, щелкните **диспетчер пакетов библиотеки**, затем нажмите кнопку **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="6e780-118">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span> <span data-ttu-id="6e780-119">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="6e780-119">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="6e780-120">Это установит пакет selfhost OWIN веб-API и все необходимые пакеты OWIN.</span><span class="sxs-lookup"><span data-stu-id="6e780-120">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="6e780-121">Настройка веб-API для резидентного размещения</span><span class="sxs-lookup"><span data-stu-id="6e780-121">Configure Web API for Self-Host</span></span>

<span data-ttu-id="6e780-122">В обозревателе решений щелкните правой кнопкой мыши проект и выберите **добавить** / **класс** для добавления нового класса.</span><span class="sxs-lookup"><span data-stu-id="6e780-122">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="6e780-123">Присвойте классу имя `Startup`.</span><span class="sxs-lookup"><span data-stu-id="6e780-123">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="6e780-124">Замените весь код шаблона в этом файле следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="6e780-124">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="6e780-125">Добавить контроллер веб-API</span><span class="sxs-lookup"><span data-stu-id="6e780-125">Add a Web API Controller</span></span>

<span data-ttu-id="6e780-126">Добавьте класс контроллера веб-API.</span><span class="sxs-lookup"><span data-stu-id="6e780-126">Next, add a Web API controller class.</span></span> <span data-ttu-id="6e780-127">В обозревателе решений щелкните правой кнопкой мыши проект и выберите **добавить** / **класс** для добавления нового класса.</span><span class="sxs-lookup"><span data-stu-id="6e780-127">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="6e780-128">Присвойте классу имя `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="6e780-128">Name the class `ValuesController`.</span></span>

<span data-ttu-id="6e780-129">Замените весь код шаблона в этом файле следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="6e780-129">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a><span data-ttu-id="6e780-130">Запустите хоста OWIN и сделать запрос с помощью HttpClient</span><span class="sxs-lookup"><span data-stu-id="6e780-130">Start the OWIN Host and Make a Request Using HttpClient</span></span>

<span data-ttu-id="6e780-131">Замените весь код шаблона в файле Program.cs следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="6e780-131">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a><span data-ttu-id="6e780-132">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="6e780-132">Running the Application</span></span>

<span data-ttu-id="6e780-133">Чтобы запустить приложение, нажмите клавишу F5 в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6e780-133">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="6e780-134">Этот вывод должен выглядеть так:</span><span class="sxs-lookup"><span data-stu-id="6e780-134">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="6e780-135">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="6e780-135">Additional Resources</span></span>

[<span data-ttu-id="6e780-136">Обзор проекта Katana</span><span class="sxs-lookup"><span data-stu-id="6e780-136">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="6e780-137">Размещение веб-API ASP.NET в рабочей роли Azure</span><span class="sxs-lookup"><span data-stu-id="6e780-137">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
