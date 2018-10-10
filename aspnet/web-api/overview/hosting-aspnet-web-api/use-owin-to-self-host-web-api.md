---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Использование OWIN для резидентного размещения веб-API 2 ASP.NET | Документация Майкрософт
author: rick-anderson
description: Этом руководстве показано, как разместить веб-API ASP.NET в консольном приложении, с помощью OWIN для резидентного размещения платформа веб-API. Откройте веб-интерфейс для .NET (OWIN) d...
ms.author: riande
ms.date: 07/09/2013
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 06fd13fe9b12d172d615ae76a71d246a89f5386d
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910490"
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a><span data-ttu-id="dba09-104">Использование OWIN для резидентного размещения веб-API 2 ASP.NET</span><span class="sxs-lookup"><span data-stu-id="dba09-104">Use OWIN to Self-Host ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="dba09-105">по [Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span><span class="sxs-lookup"><span data-stu-id="dba09-105">by [Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span></span>

> <span data-ttu-id="dba09-106">Этом руководстве показано, как разместить веб-API ASP.NET в консольном приложении, с помощью OWIN для резидентного размещения платформа веб-API.</span><span class="sxs-lookup"><span data-stu-id="dba09-106">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
>
> <span data-ttu-id="dba09-107">[Открыть веб-интерфейс .NET](http://owin.org) (OWIN) определяет абстракции между веб-серверов .NET и веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="dba09-107">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="dba09-108">OWIN отделяет веб-приложения на сервер, который идеально OWIN для резидентного размещения веб-приложения в собственном процессе, за пределами служб IIS.</span><span class="sxs-lookup"><span data-stu-id="dba09-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="dba09-109">Версии программного обеспечения, используемые в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="dba09-109">Software versions used in the tutorial</span></span>
>
>
> - <span data-ttu-id="dba09-110">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (также работает с Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="dba09-110">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (also works with Visual Studio 2012)</span></span>
> - <span data-ttu-id="dba09-111">Веб-API 2</span><span class="sxs-lookup"><span data-stu-id="dba09-111">Web API 2</span></span>


> [!NOTE]
> <span data-ttu-id="dba09-112">Полный исходный код для выполнения инструкций этого руководства можно найти [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="dba09-112">You can find the complete source code for this tutorial at [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span></span>


## <a name="create-a-console-application"></a><span data-ttu-id="dba09-113">Создайте консольное приложение</span><span class="sxs-lookup"><span data-stu-id="dba09-113">Create a Console Application</span></span>

<span data-ttu-id="dba09-114">На **файл** меню, щелкните **New**, затем нажмите кнопку **проекта**.</span><span class="sxs-lookup"><span data-stu-id="dba09-114">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="dba09-115">Из **установленные шаблоны**, в разделе Visual C#, щелкните **Windows** и нажмите кнопку **консольное приложение**.</span><span class="sxs-lookup"><span data-stu-id="dba09-115">From **Installed Templates**, under Visual C#, click **Windows** and then click **Console Application**.</span></span> <span data-ttu-id="dba09-116">Присвойте проекту имя «OwinSelfhostSample» и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="dba09-116">Name the project "OwinSelfhostSample" and click **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="dba09-117">Добавьте веб-API и пакеты OWIN</span><span class="sxs-lookup"><span data-stu-id="dba09-117">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="dba09-118">Из **средства** меню, щелкните **диспетчер пакетов NuGet**, затем нажмите кнопку **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="dba09-118">From the **Tools** menu, click **NuGet Package Manager**, then click **Package Manager Console**.</span></span> <span data-ttu-id="dba09-119">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="dba09-119">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="dba09-120">Это установит пакет selfhost OWIN веб-API и все необходимые пакеты OWIN.</span><span class="sxs-lookup"><span data-stu-id="dba09-120">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="dba09-121">Настройка веб-API для резидентного размещения</span><span class="sxs-lookup"><span data-stu-id="dba09-121">Configure Web API for Self-Host</span></span>

<span data-ttu-id="dba09-122">В обозревателе решений щелкните правой кнопкой мыши проект и выберите **добавить** / **класс** для добавления нового класса.</span><span class="sxs-lookup"><span data-stu-id="dba09-122">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="dba09-123">Присвойте классу имя `Startup`.</span><span class="sxs-lookup"><span data-stu-id="dba09-123">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="dba09-124">Замените весь код шаблона в этом файле следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="dba09-124">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="dba09-125">Добавить контроллер веб-API</span><span class="sxs-lookup"><span data-stu-id="dba09-125">Add a Web API Controller</span></span>

<span data-ttu-id="dba09-126">Добавьте класс контроллера веб-API.</span><span class="sxs-lookup"><span data-stu-id="dba09-126">Next, add a Web API controller class.</span></span> <span data-ttu-id="dba09-127">В обозревателе решений щелкните правой кнопкой мыши проект и выберите **добавить** / **класс** для добавления нового класса.</span><span class="sxs-lookup"><span data-stu-id="dba09-127">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="dba09-128">Присвойте классу имя `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="dba09-128">Name the class `ValuesController`.</span></span>

<span data-ttu-id="dba09-129">Замените весь код шаблона в этом файле следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="dba09-129">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a><span data-ttu-id="dba09-130">Запустите хоста OWIN и сделать запрос с помощью HttpClient</span><span class="sxs-lookup"><span data-stu-id="dba09-130">Start the OWIN Host and Make a Request Using HttpClient</span></span>

<span data-ttu-id="dba09-131">Замените весь код шаблона в файле Program.cs следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="dba09-131">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a><span data-ttu-id="dba09-132">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="dba09-132">Running the Application</span></span>

<span data-ttu-id="dba09-133">Чтобы запустить приложение, нажмите клавишу F5 в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dba09-133">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="dba09-134">Этот вывод должен выглядеть так:</span><span class="sxs-lookup"><span data-stu-id="dba09-134">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="dba09-135">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="dba09-135">Additional Resources</span></span>

[<span data-ttu-id="dba09-136">Обзор проекта Katana</span><span class="sxs-lookup"><span data-stu-id="dba09-136">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="dba09-137">Размещение веб-API ASP.NET в рабочей роли Azure</span><span class="sxs-lookup"><span data-stu-id="dba09-137">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
