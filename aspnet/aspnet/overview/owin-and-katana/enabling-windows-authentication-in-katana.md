---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: "Включение проверки подлинности Windows в Katana | Документы Microsoft"
author: MikeWasson
description: "В этой статье показано, как включить проверку подлинности Windows в Katana. Рассматриваются два сценария: с помощью служб IIS для размещения Katana, а с помощью HttpListener резидентной Kat..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 8a26d356f7abafba021199761f9a49dcb81765c5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="enabling-windows-authentication-in-katana"></a><span data-ttu-id="b56b0-104">Включение проверки подлинности Windows в Katana</span><span class="sxs-lookup"><span data-stu-id="b56b0-104">Enabling Windows Authentication in Katana</span></span>
====================
<span data-ttu-id="b56b0-105">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b56b0-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="b56b0-106">В этой статье показано, как включить проверку подлинности Windows в Katana.</span><span class="sxs-lookup"><span data-stu-id="b56b0-106">This article shows how to enable Windows Authentication in Katana.</span></span> <span data-ttu-id="b56b0-107">Рассматриваются два сценария: с помощью служб IIS для размещения Katana, а с помощью HttpListener резидентной Katana в пользовательском процессе.</span><span class="sxs-lookup"><span data-stu-id="b56b0-107">It covers two scenarios: Using IIS to host Katana, and using HttpListener to self-host Katana in a custom process.</span></span> <span data-ttu-id="b56b0-108">Спасибо, что Барри Dorrans, Дэвид Matson и Крис Ross рецензирование статьи.</span><span class="sxs-lookup"><span data-stu-id="b56b0-108">Thanks to Barry Dorrans, David Matson, and Chris Ross for reviewing this article.</span></span>


<span data-ttu-id="b56b0-109">Katana является реализацией Майкрософт [OWIN](http://owin.org/), откройте веб-интерфейс для .NET.</span><span class="sxs-lookup"><span data-stu-id="b56b0-109">Katana is Microsoft's implementation of [OWIN](http://owin.org/), the Open Web Interface for .NET.</span></span> <span data-ttu-id="b56b0-110">Можно читать введение OWIN и Katana [здесь](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="b56b0-110">You can read an introduction to OWIN and Katana [here](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="b56b0-111">Архитектура OWIN состоит из нескольких уровней:</span><span class="sxs-lookup"><span data-stu-id="b56b0-111">The OWIN architecture has several layers:</span></span>

- <span data-ttu-id="b56b0-112">Узел: Управляет процессом, в которой выполняется в конвейер OWIN.</span><span class="sxs-lookup"><span data-stu-id="b56b0-112">Host: Manages the process in which the OWIN pipeline runs.</span></span>
- <span data-ttu-id="b56b0-113">Сервер: Открывает к сетевому сокету и прослушивает запросы.</span><span class="sxs-lookup"><span data-stu-id="b56b0-113">Server: Opens a network socket and listens for requests.</span></span>
- <span data-ttu-id="b56b0-114">По промежуточного слоя: Обрабатывает HTTP-запроса и ответа.</span><span class="sxs-lookup"><span data-stu-id="b56b0-114">Middleware: Processes the HTTP request and response.</span></span>

<span data-ttu-id="b56b0-115">В настоящее время Katana предоставляет два сервера, которые поддерживают как встроенная проверка подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="b56b0-115">Katana currently provides two servers, both of which support Windows Integrated Authentication:</span></span>

- <span data-ttu-id="b56b0-116">**Microsoft.Owin.Host.SystemWeb**.</span><span class="sxs-lookup"><span data-stu-id="b56b0-116">**Microsoft.Owin.Host.SystemWeb**.</span></span> <span data-ttu-id="b56b0-117">Использует службы IIS с помощью конвейера ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b56b0-117">Uses IIS with the ASP.NET pipeline.</span></span>
- <span data-ttu-id="b56b0-118">**Microsoft.Owin.Host.HttpListener**.</span><span class="sxs-lookup"><span data-stu-id="b56b0-118">**Microsoft.Owin.Host.HttpListener**.</span></span> <span data-ttu-id="b56b0-119">Использует [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span><span class="sxs-lookup"><span data-stu-id="b56b0-119">Uses [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span></span> <span data-ttu-id="b56b0-120">Этот сервер в настоящее время является параметром по умолчанию, при размещении Katana.</span><span class="sxs-lookup"><span data-stu-id="b56b0-120">This server is currently the default option when self-hosting Katana.</span></span>

> [!NOTE]
> <span data-ttu-id="b56b0-121">Katana не предоставляет в настоящее время по промежуточного слоя OWIN для проверки подлинности Windows, так как эта функция уже доступна на серверах.</span><span class="sxs-lookup"><span data-stu-id="b56b0-121">Katana does not currently provide OWIN middleware for Windows Authentication, because this functionality is already available in the servers.</span></span>


## <a name="windows-authentication-in-iis"></a><span data-ttu-id="b56b0-122">Проверка подлинности Windows в службах IIS</span><span class="sxs-lookup"><span data-stu-id="b56b0-122">Windows Authentication in IIS</span></span>

<span data-ttu-id="b56b0-123">С помощью Microsoft.Owin.Host.SystemWeb, можно просто включите проверку подлинности Windows в службах IIS.</span><span class="sxs-lookup"><span data-stu-id="b56b0-123">Using Microsoft.Owin.Host.SystemWeb, you can simply enable Windows Authentication in IIS.</span></span>

<span data-ttu-id="b56b0-124">Давайте начнем с создания нового приложения ASP.NET с помощью шаблона проекта «Пустой веб-приложение ASP.NET».</span><span class="sxs-lookup"><span data-stu-id="b56b0-124">Let's start by creating a new ASP.NET application, using the "ASP.NET Empty Web Application" project template.</span></span>

![](enabling-windows-authentication-in-katana/_static/image1.png)

<span data-ttu-id="b56b0-125">Добавьте пакеты NuGet.</span><span class="sxs-lookup"><span data-stu-id="b56b0-125">Next, add NuGet packages.</span></span> <span data-ttu-id="b56b0-126">Из **средства** последовательно выберите пункты **диспетчер пакетов библиотеки**, а затем выберите **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="b56b0-126">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="b56b0-127">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="b56b0-127">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

<span data-ttu-id="b56b0-128">Теперь добавьте класс с именем `Startup` следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="b56b0-128">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

<span data-ttu-id="b56b0-129">Это все, что нужно создать приложение «Hello world» для OWIN, на сервере IIS.</span><span class="sxs-lookup"><span data-stu-id="b56b0-129">That's all you need to create a "Hello world" application for OWIN, running on IIS.</span></span> <span data-ttu-id="b56b0-130">Нажмите клавишу F5, чтобы начать отладку приложения.</span><span class="sxs-lookup"><span data-stu-id="b56b0-130">Press F5 to debug the application.</span></span> <span data-ttu-id="b56b0-131">Должно появиться сообщение «Hello World!»</span><span class="sxs-lookup"><span data-stu-id="b56b0-131">You should see "Hello World!"</span></span> <span data-ttu-id="b56b0-132">в окне браузера.</span><span class="sxs-lookup"><span data-stu-id="b56b0-132">in the browser window.</span></span>

![](enabling-windows-authentication-in-katana/_static/image2.png)

<span data-ttu-id="b56b0-133">Далее мы будем включите проверку подлинности Windows в IIS Express.</span><span class="sxs-lookup"><span data-stu-id="b56b0-133">Next, we'll enable Windows Authentication in IIS Express.</span></span> <span data-ttu-id="b56b0-134">Из **представление** последовательно выберите пункты **свойства**.</span><span class="sxs-lookup"><span data-stu-id="b56b0-134">From the **View** menu, select **Properties**.</span></span> <span data-ttu-id="b56b0-135">Щелкните имя проекта в обозревателе решений для просмотра свойств проекта.</span><span class="sxs-lookup"><span data-stu-id="b56b0-135">Click on the project name in Solution Explorer to view the project properties.</span></span>

<span data-ttu-id="b56b0-136">В **свойства** задайте **анонимную проверку подлинности** для **отключено** и задайте **проверки подлинности Windows** для  **Включить**.</span><span class="sxs-lookup"><span data-stu-id="b56b0-136">In the **Properties** window, set **Anonymous Authentication** to **Disabled** and set **Windows Authentication** to **Enabled**.</span></span>

![](enabling-windows-authentication-in-katana/_static/image3.png)

<span data-ttu-id="b56b0-137">При запуске приложения из Visual Studio, IIS Express потребуется учетные данные пользователя Windows.</span><span class="sxs-lookup"><span data-stu-id="b56b0-137">When you run the application from Visual Studio, IIS Express will require the user's Windows credentials.</span></span> <span data-ttu-id="b56b0-138">Это можно увидеть с помощью [Fiddler](http://fiddler2.com/home) или другой HTTP, средство отладки.</span><span class="sxs-lookup"><span data-stu-id="b56b0-138">You can see this by using [Fiddler](http://fiddler2.com/home) or another HTTP debugging tool.</span></span> <span data-ttu-id="b56b0-139">Ниже приведен пример ответа HTTP:</span><span class="sxs-lookup"><span data-stu-id="b56b0-139">Here is an example HTTP response:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

<span data-ttu-id="b56b0-140">Заголовки WWW-Authenticate в данном ответе указывают, что сервер поддерживает [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) протокола, который использует протокол Kerberos или NTLM.</span><span class="sxs-lookup"><span data-stu-id="b56b0-140">The WWW-Authenticate headers in this response indicate that the server supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocol, which uses either Kerberos or NTLM.</span></span>

<span data-ttu-id="b56b0-141">Позже, при развертывании приложения на сервере, выполните [эти действия](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) Включение проверки подлинности Windows в службах IIS на этом сервере.</span><span class="sxs-lookup"><span data-stu-id="b56b0-141">Later, when you deploy the application to a server, follow [these steps](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) to enable Windows Authentication in IIS on that server.</span></span>

## <a name="windows-authentication-in-httplistener"></a><span data-ttu-id="b56b0-142">Проверка подлинности Windows в HttpListener</span><span class="sxs-lookup"><span data-stu-id="b56b0-142">Windows Authentication in HttpListener</span></span>

<span data-ttu-id="b56b0-143">При использовании Microsoft.Owin.Host.HttpListener для самостоятельного размещения Katana проверку подлинности Windows можно включить непосредственно в **HttpListener** экземпляра.</span><span class="sxs-lookup"><span data-stu-id="b56b0-143">If you are using Microsoft.Owin.Host.HttpListener to self-host Katana, you can enable Windows Authentication directly on the **HttpListener** instance.</span></span>

<span data-ttu-id="b56b0-144">Во-первых создайте новое консольное приложение.</span><span class="sxs-lookup"><span data-stu-id="b56b0-144">First, create a new console application.</span></span> <span data-ttu-id="b56b0-145">Добавьте пакеты NuGet.</span><span class="sxs-lookup"><span data-stu-id="b56b0-145">Next, add NuGet packages.</span></span> <span data-ttu-id="b56b0-146">Из **средства** последовательно выберите пункты **диспетчер пакетов библиотеки**, а затем выберите **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="b56b0-146">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="b56b0-147">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="b56b0-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

<span data-ttu-id="b56b0-148">Теперь добавьте класс с именем `Startup` следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="b56b0-148">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

<span data-ttu-id="b56b0-149">Этот класс реализует тот же пример «Hello world», от и до, но также задает проверку подлинности Windows, как схемы проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="b56b0-149">This class implements the same "Hello world" example from before, but it also sets Windows Authentication as the authentication scheme.</span></span>

<span data-ttu-id="b56b0-150">Внутри `Main` функционировать, запуск конвейера OWIN:</span><span class="sxs-lookup"><span data-stu-id="b56b0-150">Inside the `Main` function, start the OWIN pipeline:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

<span data-ttu-id="b56b0-151">Можно отправить запрос в Fiddler, чтобы убедиться, что приложение использует проверку подлинности Windows:</span><span class="sxs-lookup"><span data-stu-id="b56b0-151">You can send a request in Fiddler to confirm that the application is using Windows Authentication:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a><span data-ttu-id="b56b0-152">См. также</span><span class="sxs-lookup"><span data-stu-id="b56b0-152">Related Topics</span></span>

[<span data-ttu-id="b56b0-153">Обзор проекта Katana</span><span class="sxs-lookup"><span data-stu-id="b56b0-153">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)

[<span data-ttu-id="b56b0-154">System.Net.HttpListener</span><span class="sxs-lookup"><span data-stu-id="b56b0-154">System.Net.HttpListener</span></span>](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[<span data-ttu-id="b56b0-155">Основные сведения об OWIN проверки подлинности форм в MVC 5</span><span class="sxs-lookup"><span data-stu-id="b56b0-155">Understanding OWIN Forms Authentication in MVC 5</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
