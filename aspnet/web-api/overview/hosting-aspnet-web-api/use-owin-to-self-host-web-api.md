---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Использование OWIN для резидентного размещения веб-API ASP.NET | Документация Майкрософт
author: rick-anderson
description: Этом руководстве показано, как разместить веб-API ASP.NET в консольном приложении, с помощью OWIN для резидентного размещения платформа веб-API. Откройте веб-интерфейс для .NET (OWIN) d...
ms.author: riande
ms.date: 07/09/2013
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 59ce24aa47ca590fbe9b617dbbe8bc6b3711849e
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667392"
---
<a name="use-owin-to-self-host-aspnet-web-api"></a><span data-ttu-id="6d768-104">Использование OWIN для резидентного размещения веб-API ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6d768-104">Use OWIN to Self-Host ASP.NET Web API</span></span> 
====================

> <span data-ttu-id="6d768-105">Этом руководстве показано, как разместить веб-API ASP.NET в консольном приложении, с помощью OWIN для резидентного размещения платформа веб-API.</span><span class="sxs-lookup"><span data-stu-id="6d768-105">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
>
> <span data-ttu-id="6d768-106">[Открыть веб-интерфейс .NET](http://owin.org) (OWIN) определяет абстракции между веб-серверов .NET и веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="6d768-106">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="6d768-107">OWIN отделяет веб-приложения на сервер, который идеально OWIN для резидентного размещения веб-приложения в собственном процессе, за пределами служб IIS.</span><span class="sxs-lookup"><span data-stu-id="6d768-107">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6d768-108">Версии программного обеспечения, используемые в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="6d768-108">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="6d768-109">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="6d768-109">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/) 
> - <span data-ttu-id="6d768-110">Веб-API 5.2.7</span><span class="sxs-lookup"><span data-stu-id="6d768-110">Web API 5.2.7</span></span>


> [!NOTE]
> <span data-ttu-id="6d768-111">Полный исходный код для выполнения инструкций этого руководства можно найти [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="6d768-111">You can find the complete source code for this tutorial at [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span></span>


## <a name="create-a-console-application"></a><span data-ttu-id="6d768-112">Создание консольного приложения</span><span class="sxs-lookup"><span data-stu-id="6d768-112">Create a console application</span></span>

<span data-ttu-id="6d768-113">На **файл** меню **New**, а затем выберите **проекта**.</span><span class="sxs-lookup"><span data-stu-id="6d768-113">On the **File** menu,  **New**, then select **Project**.</span></span> <span data-ttu-id="6d768-114">Из **установленные**в разделе **Visual C#** выберите **Windows Desktop** , а затем выберите **консольное приложение (.Net Framework)**.</span><span class="sxs-lookup"><span data-stu-id="6d768-114">From **Installed**, under **Visual C#**, select **Windows Desktop** and then select **Console App (.Net Framework)**.</span></span> <span data-ttu-id="6d768-115">Присвойте проекту имя «OwinSelfhostSample» и выберите **ОК**.</span><span class="sxs-lookup"><span data-stu-id="6d768-115">Name the project "OwinSelfhostSample" and select **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image7.png)](use-owin-to-self-host-web-api/_static/image7.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="6d768-116">Добавьте пакеты веб-API и OWIN</span><span class="sxs-lookup"><span data-stu-id="6d768-116">Add the Web API and OWIN packages</span></span>

<span data-ttu-id="6d768-117">Из **средства** меню, выберите **диспетчер пакетов NuGet**, а затем выберите **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="6d768-117">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="6d768-118">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="6d768-118">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="6d768-119">Это установит пакет selfhost OWIN веб-API и все необходимые пакеты OWIN.</span><span class="sxs-lookup"><span data-stu-id="6d768-119">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="6d768-120">Настройка веб-API для резидентного размещения</span><span class="sxs-lookup"><span data-stu-id="6d768-120">Configure Web API for self-host</span></span>

<span data-ttu-id="6d768-121">В обозревателе решений щелкните правой кнопкой мыши проект и выберите **добавить** / **класс** для добавления нового класса.</span><span class="sxs-lookup"><span data-stu-id="6d768-121">In Solution Explorer, right-click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="6d768-122">Присвойте классу имя `Startup`.</span><span class="sxs-lookup"><span data-stu-id="6d768-122">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="6d768-123">Замените весь код шаблона в этом файле следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="6d768-123">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="6d768-124">Добавление контроллера веб-API</span><span class="sxs-lookup"><span data-stu-id="6d768-124">Add a Web API controller</span></span>

<span data-ttu-id="6d768-125">Добавьте класс контроллера веб-API.</span><span class="sxs-lookup"><span data-stu-id="6d768-125">Next, add a Web API controller class.</span></span> <span data-ttu-id="6d768-126">В обозревателе решений щелкните правой кнопкой мыши проект и выберите **добавить** / **класс** для добавления нового класса.</span><span class="sxs-lookup"><span data-stu-id="6d768-126">In Solution Explorer, right-click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="6d768-127">Присвойте классу имя `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="6d768-127">Name the class `ValuesController`.</span></span>

<span data-ttu-id="6d768-128">Замените весь код шаблона в этом файле следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="6d768-128">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-with-httpclient"></a><span data-ttu-id="6d768-129">Запустите хоста OWIN и сделать запрос с помощью HttpClient</span><span class="sxs-lookup"><span data-stu-id="6d768-129">Start the OWIN Host and make a request with HttpClient</span></span>

<span data-ttu-id="6d768-130">Замените весь код шаблона в файле Program.cs следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="6d768-130">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="run-the-application"></a><span data-ttu-id="6d768-131">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="6d768-131">Run the application</span></span>

<span data-ttu-id="6d768-132">Чтобы запустить приложение, нажмите клавишу F5 в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6d768-132">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="6d768-133">Этот вывод должен выглядеть так:</span><span class="sxs-lookup"><span data-stu-id="6d768-133">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="6d768-134">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="6d768-134">Additional resources</span></span>

[<span data-ttu-id="6d768-135">Обзор проекта Katana</span><span class="sxs-lookup"><span data-stu-id="6d768-135">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="6d768-136">Размещение веб-API ASP.NET в рабочей роли Azure</span><span class="sxs-lookup"><span data-stu-id="6d768-136">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
