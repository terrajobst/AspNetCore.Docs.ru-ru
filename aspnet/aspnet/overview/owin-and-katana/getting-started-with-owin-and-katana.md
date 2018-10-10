---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Приступая к работе с OWIN и Katana | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: riande
ms.date: 09/27/2013
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 9920861da0e67d9304a944cacfb8ff8685267cd6
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913181"
---
<a name="getting-started-with-owin-and-katana"></a><span data-ttu-id="06731-102">Приступая к работе с OWIN и Katana</span><span class="sxs-lookup"><span data-stu-id="06731-102">Getting Started with OWIN and Katana</span></span>
====================
<span data-ttu-id="06731-103">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="06731-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="06731-104">[Откройте веб-интерфейс для .NET (OWIN)](http://owin.org/) абстракцию между веб-серверов .NET и веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="06731-104">[Open Web Interface for .NET (OWIN)](http://owin.org/) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="06731-105">Отделив веб-сервера из приложения, OWIN упрощает создание по промежуточного слоя для разработки веб-приложений .NET.</span><span class="sxs-lookup"><span data-stu-id="06731-105">By decoupling the web server from the application, OWIN makes it easier to create middleware for .NET web development.</span></span> <span data-ttu-id="06731-106">Кроме того, OWIN упрощает порт веб-приложений на другие узлы&#8212;к примеру, Резидентное размещение в службе Windows или другой процесс.</span><span class="sxs-lookup"><span data-stu-id="06731-106">Also, OWIN makes it easier to port web applications to other hosts&#8212;for example, self-hosting in a Windows service or other process.</span></span>

<span data-ttu-id="06731-107">OWIN — это спецификация принадлежащих сообщества, не реализацию.</span><span class="sxs-lookup"><span data-stu-id="06731-107">OWIN is a community-owned specification, not an implementation.</span></span> <span data-ttu-id="06731-108">Проект Katana — это набор компонентов OWIN с открытым кодом, разработанная корпорацией Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="06731-108">The Katana project is a set of open-source OWIN components developed by Microsoft.</span></span> <span data-ttu-id="06731-109">Общие сведения о OWIN и Katana, см. в разделе [Обзор проекта Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="06731-109">For a general overview of both OWIN and Katana, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="06731-110">В этой статье я перейти непосредственно в код, чтобы приступить к работе.</span><span class="sxs-lookup"><span data-stu-id="06731-110">In this article, I will jump right into code to get started.</span></span>

<span data-ttu-id="06731-111">В этом руководстве используется [версии-кандидата Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566), но можно также использовать Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="06731-111">This tutorial uses [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), but you can also use Visual Studio 2012.</span></span> <span data-ttu-id="06731-112">Некоторые действия различаются в Visual Studio 2012, который я примечание ниже.</span><span class="sxs-lookup"><span data-stu-id="06731-112">A few of the steps are different in Visual Studio 2012, which I note below.</span></span>

## <a name="host-owin-in-iis"></a><span data-ttu-id="06731-113">Размещение OWIN в IIS</span><span class="sxs-lookup"><span data-stu-id="06731-113">Host OWIN in IIS</span></span>

<span data-ttu-id="06731-114">В этом разделе будут размещены OWIN в службах IIS.</span><span class="sxs-lookup"><span data-stu-id="06731-114">In this section, we'll host OWIN in IIS.</span></span> <span data-ttu-id="06731-115">Этот параметр обеспечивает гибкость и возможность компоновки конвейер OWIN вместе с набор зрелой функций IIS.</span><span class="sxs-lookup"><span data-stu-id="06731-115">This option gives you the flexibility and composability of an OWIN pipeline together with the mature feature set of IIS.</span></span> <span data-ttu-id="06731-116">При использовании этого параметра приложение OWIN, выполняется в конвейере запросов ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="06731-116">Using this option, the OWIN application runs in the ASP.NET request pipeline.</span></span>

<span data-ttu-id="06731-117">Во-первых создайте новый проект веб-приложения ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="06731-117">First, create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="06731-118">(В Visual Studio 2012, используйте тип проекта пустое веб-приложение ASP.NET).</span><span class="sxs-lookup"><span data-stu-id="06731-118">(In Visual Studio 2012, use the ASP.NET Empty Web Application project type.)</span></span>

![](getting-started-with-owin-and-katana/_static/image1.png)

<span data-ttu-id="06731-119">В **новый проект ASP.NET** диалоговом окне выберите **пустой** шаблона.</span><span class="sxs-lookup"><span data-stu-id="06731-119">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span>

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a><span data-ttu-id="06731-120">Добавьте пакеты NuGet</span><span class="sxs-lookup"><span data-stu-id="06731-120">Add NuGet Packages</span></span>

<span data-ttu-id="06731-121">Затем добавьте необходимые пакеты NuGet.</span><span class="sxs-lookup"><span data-stu-id="06731-121">Next, add the required NuGet packages.</span></span> <span data-ttu-id="06731-122">Из **средства** меню, выберите **диспетчер пакетов NuGet**, а затем выберите **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="06731-122">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="06731-123">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="06731-123">In the Package Manager Console window, type the following command:</span></span>

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a><span data-ttu-id="06731-124">Добавьте класс запуска</span><span class="sxs-lookup"><span data-stu-id="06731-124">Add a Startup Class</span></span>

<span data-ttu-id="06731-125">Добавьте класс запуска OWIN.</span><span class="sxs-lookup"><span data-stu-id="06731-125">Next, add an OWIN startup class.</span></span> <span data-ttu-id="06731-126">В обозревателе решений щелкните правой кнопкой мыши проект и выберите **добавить**, а затем выберите **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="06731-126">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span> <span data-ttu-id="06731-127">В **Добавление нового элемента** диалоговом окне выберите **класс запуска Owin**.</span><span class="sxs-lookup"><span data-stu-id="06731-127">In the **Add New Item** dialog, select **Owin Startup class**.</span></span> <span data-ttu-id="06731-128">Дополнительные сведения о настройке класс startup см. в разделе [определение класса запуска OWIN](owin-startup-class-detection.md).</span><span class="sxs-lookup"><span data-stu-id="06731-128">For more info on configuring the startup class, see [OWIN Startup Class Detection](owin-startup-class-detection.md).</span></span>

![](getting-started-with-owin-and-katana/_static/image4.png)

<span data-ttu-id="06731-129">Добавьте следующий код в метод `Startup1.Configuration`:</span><span class="sxs-lookup"><span data-stu-id="06731-129">Add the following code to the `Startup1.Configuration` method:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

<span data-ttu-id="06731-130">Этот код добавляет просто блок по промежуточного слоя в конвейер OWIN, реализованную как функцию, которая получает **Microsoft.Owin.IOwinContext** экземпляра.</span><span class="sxs-lookup"><span data-stu-id="06731-130">This code adds a simple piece of middleware to the OWIN pipeline, implemented as a function that receives a **Microsoft.Owin.IOwinContext** instance.</span></span> <span data-ttu-id="06731-131">Когда сервер получает HTTP-запроса, в конвейер OWIN вызывает по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="06731-131">When the server receives an HTTP request, the OWIN pipeline invokes the middleware.</span></span> <span data-ttu-id="06731-132">Задает тип содержимого для ответа по промежуточного слоя и записывает текст ответа.</span><span class="sxs-lookup"><span data-stu-id="06731-132">The middleware sets the content type for the response and writes the response body.</span></span>

> [!NOTE]
> <span data-ttu-id="06731-133">Шаблон класса запуска OWIN доступен в Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="06731-133">The OWIN Startup class template is available in Visual Studio 2013.</span></span> <span data-ttu-id="06731-134">Если вы используете Visual Studio 2012, просто добавьте пустой класс с именем `Startup1`и вставьте в него следующий код:</span><span class="sxs-lookup"><span data-stu-id="06731-134">If you are using Visual Studio 2012, just add a new empty class named `Startup1`, and paste in the following code:</span></span>


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a><span data-ttu-id="06731-135">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="06731-135">Run the Application</span></span>

<span data-ttu-id="06731-136">Нажмите клавишу F5, чтобы начать отладку.</span><span class="sxs-lookup"><span data-stu-id="06731-136">Press F5 to begin debugging.</span></span> <span data-ttu-id="06731-137">Visual Studio открывает окно браузера, чтобы `http://localhost:*port*/`.</span><span class="sxs-lookup"><span data-stu-id="06731-137">Visual Studio will open a browser window to `http://localhost:*port*/`.</span></span> <span data-ttu-id="06731-138">Страница должна выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="06731-138">The page should look like the following:</span></span>

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a><span data-ttu-id="06731-139">OWIN резидентной в консольном приложении</span><span class="sxs-lookup"><span data-stu-id="06731-139">Self-Host OWIN in a Console Application</span></span>

<span data-ttu-id="06731-140">Его можно легко преобразовать это приложение из размещение в службах IIS для размещения на собственном сервере в пользовательском процессе.</span><span class="sxs-lookup"><span data-stu-id="06731-140">It's easy to convert this application from IIS hosting to self-hosting in a custom process.</span></span> <span data-ttu-id="06731-141">С помощью размещение в службах IIS, IIS действует как HTTP-сервер и как процесс, на котором размещена служба.</span><span class="sxs-lookup"><span data-stu-id="06731-141">With IIS hosting, IIS acts as both the HTTP server and as the process that hosts the service.</span></span> <span data-ttu-id="06731-142">С вариантом с размещением, приложение создает процесс и использует **HttpListener** класс как HTTP-сервера.</span><span class="sxs-lookup"><span data-stu-id="06731-142">With self-hosting, your application creates the process and uses the **HttpListener** class as the HTTP server.</span></span>

<span data-ttu-id="06731-143">В Visual Studio создайте новое консольное приложение.</span><span class="sxs-lookup"><span data-stu-id="06731-143">In Visual Studio, create a new console application.</span></span> <span data-ttu-id="06731-144">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="06731-144">In the Package Manager Console window, type the following command:</span></span>

`Install-Package Microsoft.Owin.SelfHost -Pre`

<span data-ttu-id="06731-145">Добавление `Startup1` класс из части 1 этого руководства в проект.</span><span class="sxs-lookup"><span data-stu-id="06731-145">Add a `Startup1` class from part 1 of this tutorial to the project.</span></span> <span data-ttu-id="06731-146">Не нужно изменять данный класс.</span><span class="sxs-lookup"><span data-stu-id="06731-146">You don't need to modify this class.</span></span>

<span data-ttu-id="06731-147">Реализация приложения `Main` метод следующим образом.</span><span class="sxs-lookup"><span data-stu-id="06731-147">Implement the application's `Main` method as follows.</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

<span data-ttu-id="06731-148">При запуске консольного приложения, сервер начинает прослушивать `http://localhost:9000`.</span><span class="sxs-lookup"><span data-stu-id="06731-148">When you run the console application, the server starts listening to `http://localhost:9000`.</span></span> <span data-ttu-id="06731-149">Если перейти по этому адресу в веб-браузере, вы увидите, что на странице «Hello world».</span><span class="sxs-lookup"><span data-stu-id="06731-149">If you navigate to this address in a web browser, you will see the "Hello world" page.</span></span>

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a><span data-ttu-id="06731-150">Добавление диагностики OWIN</span><span class="sxs-lookup"><span data-stu-id="06731-150">Add OWIN Diagnostics</span></span>

<span data-ttu-id="06731-151">Пакет Microsoft.Owin.Diagnostics содержит промежуточный слой, который перехватывает необработанные исключения и отображает HTML-страницу с сообщением об ошибке.</span><span class="sxs-lookup"><span data-stu-id="06731-151">The Microsoft.Owin.Diagnostics package contains middleware that catches unhandled exceptions and displays an HTML page with error details.</span></span> <span data-ttu-id="06731-152">Этот страничных функций как ошибку в странице ASP.NET, иногда называется "[желтый экраном смерти](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span><span class="sxs-lookup"><span data-stu-id="06731-152">This page functions much like the ASP.NET error page that is sometimes called the "[yellow screen of death](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span></span> <span data-ttu-id="06731-153">Как YSOD страницы ошибки Katana полезно использовать во время разработки, но рекомендуется его отключить в рабочем режиме.</span><span class="sxs-lookup"><span data-stu-id="06731-153">Like the YSOD, the Katana error page is useful during development, but it's a good practice to disable it in production mode.</span></span>

<span data-ttu-id="06731-154">Чтобы установить пакет диагностики в проекте, введите следующую команду в окне консоли диспетчера пакетов:</span><span class="sxs-lookup"><span data-stu-id="06731-154">To install the Diagnostics package in your project, type the following command in the Package Manager Console window:</span></span>

`install-package Microsoft.Owin.Diagnostics –Pre`

<span data-ttu-id="06731-155">Измените код в ваш `Startup1.Configuration` метод следующим образом:</span><span class="sxs-lookup"><span data-stu-id="06731-155">Change the code in your `Startup1.Configuration` method as follows:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

<span data-ttu-id="06731-156">Используйте сочетание клавиш CTRL + F5 для запуска приложения без отладки, таким образом, чтобы Visual Studio не произойдет останов на исключение.</span><span class="sxs-lookup"><span data-stu-id="06731-156">Now use CTRL+F5 to run the application without debugging, so that Visual Studio will not break on the exception.</span></span> <span data-ttu-id="06731-157">Приложение работает так же, как и раньше, пока вы переходите `http://localhost/fail`, после чего приложение выдает исключение.</span><span class="sxs-lookup"><span data-stu-id="06731-157">The application behaves the same as before, until you navigate to `http://localhost/fail`, at which point the application throws the exception.</span></span> <span data-ttu-id="06731-158">Промежуточное по страницы ошибки будет перехватить исключение и отображать HTML-страницу с информацией об ошибке.</span><span class="sxs-lookup"><span data-stu-id="06731-158">The error page middleware will catch the exception and display an HTML page with information about the error.</span></span> <span data-ttu-id="06731-159">Можно щелкнуть вкладки, чтобы просмотреть стек, строка запроса, файлы cookie, заголовка запроса и переменных среды OWIN.</span><span class="sxs-lookup"><span data-stu-id="06731-159">You can click the tabs to see the stack, query string, cookies, request header, and OWIN environment variables.</span></span>

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a><span data-ttu-id="06731-160">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="06731-160">Next Steps</span></span>

- [<span data-ttu-id="06731-161">Определение класса запуска OWIN</span><span class="sxs-lookup"><span data-stu-id="06731-161">OWIN Startup Class Detection</span></span>](owin-startup-class-detection.md)
- [<span data-ttu-id="06731-162">Использование OWIN для резидентного размещения веб-API ASP.NET</span><span class="sxs-lookup"><span data-stu-id="06731-162">Use OWIN to Self-Host ASP.NET Web API</span></span>](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [<span data-ttu-id="06731-163">Использование OWIN для резидентного размещения SignalR</span><span class="sxs-lookup"><span data-stu-id="06731-163">Use OWIN to Self-Host SignalR</span></span>](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
