---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: Учебник. Передача сообщений с сервера с помощью SignalR 2 | Документация Майкрософт
author: tdykstra
description: Этом руководстве показано, как создать веб-приложения, использующего ASP.NET SignalR 2 для предоставления широковещательных функциональные возможности сервера.
ms.author: bradyg
ms.date: 01/02/2019
ms.topic: tutorial
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: a243c78c7d552f1c82a88c6083871fcd16538618
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837433"
---
# <a name="tutorial-server-broadcast-with-signalr-2"></a><span data-ttu-id="10abe-103">Учебник. Передача сообщений с помощью SignalR 2 сервера</span><span class="sxs-lookup"><span data-stu-id="10abe-103">Tutorial: Server broadcast with SignalR 2</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="10abe-104">Этом руководстве показано, как создать веб-приложения, использующего ASP.NET SignalR 2 для предоставления широковещательных функциональные возможности сервера.</span><span class="sxs-lookup"><span data-stu-id="10abe-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span> <span data-ttu-id="10abe-105">Рассылка сервера означает, что сервер запускается связям клиентам.</span><span class="sxs-lookup"><span data-stu-id="10abe-105">Server broadcast means that the server starts the communications sent to clients.</span></span>

<span data-ttu-id="10abe-106">Приложение, которое вы создадите в этом руководстве имитирует биржевые сводки, типичный сценарий для широковещательных функциональные возможности сервера.</span><span class="sxs-lookup"><span data-stu-id="10abe-106">The application that you'll create in this tutorial simulates a stock ticker, a typical scenario for server broadcast functionality.</span></span> <span data-ttu-id="10abe-107">Периодически сервер случайным образом обновляет акций и рассылка обновлений для всех подключенных клиентов.</span><span class="sxs-lookup"><span data-stu-id="10abe-107">Periodically, the server randomly updates stock prices and broadcast the updates to all connected clients.</span></span> <span data-ttu-id="10abe-108">В браузере, цифры и символы в **изменить** и **%** динамически изменять столбцы в ответ на уведомления от сервера.</span><span class="sxs-lookup"><span data-stu-id="10abe-108">In the browser, the numbers and symbols in the **Change** and **%** columns dynamically change in response to notifications from the server.</span></span> <span data-ttu-id="10abe-109">Если открыть дополнительные браузеров на том же URL-адрес, все они Показывать те же данные и те же изменения в данные одновременно.</span><span class="sxs-lookup"><span data-stu-id="10abe-109">If you open additional browsers to the same URL, they all show the same data and the same changes to the data simultaneously.</span></span>

![Создание веб-](tutorial-server-broadcast-with-signalr/_static/image1.png)

<span data-ttu-id="10abe-111">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="10abe-111">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="10abe-112">Создание проекта</span><span class="sxs-lookup"><span data-stu-id="10abe-112">Create the project</span></span>
> * <span data-ttu-id="10abe-113">Настройте серверный код</span><span class="sxs-lookup"><span data-stu-id="10abe-113">Set up the server code</span></span>
> * <span data-ttu-id="10abe-114">Проверьте серверный код</span><span class="sxs-lookup"><span data-stu-id="10abe-114">Examine the server code</span></span>
> * <span data-ttu-id="10abe-115">Настройка кода клиента</span><span class="sxs-lookup"><span data-stu-id="10abe-115">Set up the client code</span></span>
> * <span data-ttu-id="10abe-116">Изучите код клиента</span><span class="sxs-lookup"><span data-stu-id="10abe-116">Examine the client code</span></span>
> * <span data-ttu-id="10abe-117">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="10abe-117">Test the application</span></span>
> * <span data-ttu-id="10abe-118">Включение ведения журнала</span><span class="sxs-lookup"><span data-stu-id="10abe-118">Enable logging</span></span>

> [!IMPORTANT]
> <span data-ttu-id="10abe-119">Если вы не хотите работать шаги по созданию приложения, можно установить пакет SignalR.Sample в новом проекте пустое веб-приложение ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="10abe-119">If you don't want to work through the steps of building the application, you can install the SignalR.Sample package in a new Empty ASP.NET Web Application project.</span></span> <span data-ttu-id="10abe-120">Если вы установите пакет NuGet без выполнения действия в этом руководстве, необходимо выполнить инструкции в *readme.txt* файл.</span><span class="sxs-lookup"><span data-stu-id="10abe-120">If you install the NuGet package without performing the steps in this tutorial, you must follow the instructions in the *readme.txt* file.</span></span> <span data-ttu-id="10abe-121">Для запуска пакета, необходимо добавить OWIN startup класса какие вызовы `ConfigureSignalR` метод в установленном пакете.</span><span class="sxs-lookup"><span data-stu-id="10abe-121">To run the package you need to add an OWIN startup class which calls the `ConfigureSignalR` method in the installed package.</span></span> <span data-ttu-id="10abe-122">Вы получите ошибку, если вы не добавили класс запуска OWIN.</span><span class="sxs-lookup"><span data-stu-id="10abe-122">You will receive an error if you do not add the OWIN startup class.</span></span> <span data-ttu-id="10abe-123">См. в разделе [установить образец StockTicker](#install-the-stockticker-sample) этой статьи.</span><span class="sxs-lookup"><span data-stu-id="10abe-123">See the [Install the StockTicker sample](#install-the-stockticker-sample) section of this article.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="10abe-124">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="10abe-124">Prerequisites</span></span>

 * <span data-ttu-id="10abe-125">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) с **ASP.NET и веб-разработка** рабочей нагрузки.</span><span class="sxs-lookup"><span data-stu-id="10abe-125">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="10abe-126">Создание проекта</span><span class="sxs-lookup"><span data-stu-id="10abe-126">Create the project</span></span>

<span data-ttu-id="10abe-127">В этом разделе показано, как использовать Visual Studio 2017 для создания пустой веб-приложения ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="10abe-127">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application.</span></span>

1. <span data-ttu-id="10abe-128">В Visual Studio создайте веб-приложения ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="10abe-128">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Создание веб-](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. <span data-ttu-id="10abe-130">В **новый веб-приложение ASP.NET - SignalR.StockTicker** окне оставьте **пустой** затем выберите **ОК**.</span><span class="sxs-lookup"><span data-stu-id="10abe-130">In the **New ASP.NET Web Application - SignalR.StockTicker** window, leave **Empty** selected and select **OK**.</span></span>

## <a name="set-up-the-server-code"></a><span data-ttu-id="10abe-131">Настройте серверный код</span><span class="sxs-lookup"><span data-stu-id="10abe-131">Set up the server code</span></span>

<span data-ttu-id="10abe-132">В этом разделе настраивается код, который выполняется на сервере.</span><span class="sxs-lookup"><span data-stu-id="10abe-132">In this section, you set up the code that runs on the server.</span></span>

### <a name="create-the-stock-class"></a><span data-ttu-id="10abe-133">Создайте класс акции</span><span class="sxs-lookup"><span data-stu-id="10abe-133">Create the Stock class</span></span>

<span data-ttu-id="10abe-134">Начните с создания *Stock* класс, который используется для хранения и передачи информации о акций модели.</span><span class="sxs-lookup"><span data-stu-id="10abe-134">You begin by creating the *Stock* model class that you'll use to store and transmit information about a stock.</span></span>

1. <span data-ttu-id="10abe-135">В **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите **добавить** > **класс**.</span><span class="sxs-lookup"><span data-stu-id="10abe-135">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="10abe-136">Назовите класс *Stock* и добавьте его в проект.</span><span class="sxs-lookup"><span data-stu-id="10abe-136">Name the class *Stock* and add it to the project.</span></span>

1. <span data-ttu-id="10abe-137">Замените код в *Stock.cs* файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="10abe-137">Replace the code in the *Stock.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    <span data-ttu-id="10abe-138">Два свойства, которые вы укажете при создании акций, `Symbol` (например, MSFT корпорации Майкрософт) и `Price`.</span><span class="sxs-lookup"><span data-stu-id="10abe-138">The two properties that you'll set when you create stocks are `Symbol` (for example, MSFT for Microsoft) and `Price`.</span></span> <span data-ttu-id="10abe-139">Другие свойства зависят от того, как и когда будет установлено `Price`.</span><span class="sxs-lookup"><span data-stu-id="10abe-139">The other properties depend on how and when you set `Price`.</span></span> <span data-ttu-id="10abe-140">В первый раз, можно задать `Price`, значение возвращает распространяется `DayOpen`.</span><span class="sxs-lookup"><span data-stu-id="10abe-140">The first time you set `Price`, the value gets propagated to `DayOpen`.</span></span> <span data-ttu-id="10abe-141">После этого при задании `Price`, приложение вычисляет `Change` и `PercentChange` значения свойств, основанное на различии между `Price` и `DayOpen`.</span><span class="sxs-lookup"><span data-stu-id="10abe-141">After that, when you set `Price`, the app calculates the `Change` and `PercentChange` property values based on the difference between `Price` and `DayOpen`.</span></span>

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a><span data-ttu-id="10abe-142">Создание классов StockTickerHub и StockTicker</span><span class="sxs-lookup"><span data-stu-id="10abe-142">Create the StockTickerHub and StockTicker classes</span></span>

<span data-ttu-id="10abe-143">Вы используете API концентратора SignalR для обработки взаимодействия клиентом и сервером.</span><span class="sxs-lookup"><span data-stu-id="10abe-143">You'll use the SignalR Hub API to handle server-to-client interaction.</span></span> <span data-ttu-id="10abe-144">Объект `StockTickerHub` класс, производный от `SignalRHub` класс, обрабатывающий получение подключений и вызовы методов из клиентов.</span><span class="sxs-lookup"><span data-stu-id="10abe-144">A `StockTickerHub` class that derives from the `SignalRHub` class will handle receiving connections and method calls from clients.</span></span> <span data-ttu-id="10abe-145">Также необходимо поддерживать биржевых данных и запустите `Timer` объекта.</span><span class="sxs-lookup"><span data-stu-id="10abe-145">You also need to maintain stock data and run a `Timer` object.</span></span> <span data-ttu-id="10abe-146">`Timer` Объект периодически будет активировать обновление цены, независимо от клиентских подключений.</span><span class="sxs-lookup"><span data-stu-id="10abe-146">The `Timer` object will periodically trigger price updates independent of client connections.</span></span> <span data-ttu-id="10abe-147">Эти функции нельзя поместить в `Hub` класса, так как концентраторы являются временными.</span><span class="sxs-lookup"><span data-stu-id="10abe-147">You can't put these functions in a `Hub` class, because Hubs are transient.</span></span> <span data-ttu-id="10abe-148">Приложение создает `Hub` экземпляр класса для каждой задачи на концентраторе, такие как подключения и вызовы от клиента к серверу.</span><span class="sxs-lookup"><span data-stu-id="10abe-148">The app creates a `Hub` class instance for each task on the hub, like connections and calls from the client to the server.</span></span> <span data-ttu-id="10abe-149">Поэтому механизм, который отслеживает биржевых данных, затем обновляет цены и передает изменения цены должен выполняться в отдельном классе.</span><span class="sxs-lookup"><span data-stu-id="10abe-149">So the mechanism that keeps stock data, updates prices, and broadcasts the price updates has to run in a separate class.</span></span> <span data-ttu-id="10abe-150">Этот класс будет назван `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="10abe-150">You'll name the class `StockTicker`.</span></span>

![Широковещательная рассылка из StockTicker](tutorial-server-broadcast-with-signalr/_static/image3.png)

<span data-ttu-id="10abe-152">Требуется только один экземпляр `StockTicker` класса для запуска на сервере, поэтому вам потребуется настроить ссылку из каждого `StockTickerHub` экземпляр одноэлементного `StockTicker` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="10abe-152">You only want one instance of the `StockTicker` class to run on the server, so you'll need to set up a reference from each `StockTickerHub` instance to the singleton `StockTicker` instance.</span></span> <span data-ttu-id="10abe-153">`StockTicker` Класс имеет для широковещательной передачи клиентов, так как он имеет биржевых данных и триггеров обновлений, но `StockTicker` не `Hub` класса.</span><span class="sxs-lookup"><span data-stu-id="10abe-153">The `StockTicker` class has to broadcast to clients because it has the stock data and triggers updates, but `StockTicker` isn't a `Hub` class.</span></span> <span data-ttu-id="10abe-154">`StockTicker` Класс должен получить ссылку на объект контекста подключения концентратора SignalR.</span><span class="sxs-lookup"><span data-stu-id="10abe-154">The `StockTicker` class has to get a reference to the SignalR Hub connection context object.</span></span> <span data-ttu-id="10abe-155">Он этого можно использовать объект контекста подключения SignalR для рассылки клиентам.</span><span class="sxs-lookup"><span data-stu-id="10abe-155">It can then use the SignalR connection context object to broadcast to clients.</span></span>

#### <a name="create-stocktickerhubcs"></a><span data-ttu-id="10abe-156">Create StockTickerHub.cs</span><span class="sxs-lookup"><span data-stu-id="10abe-156">Create StockTickerHub.cs</span></span>

1. <span data-ttu-id="10abe-157">В **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите **добавить** > **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="10abe-157">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="10abe-158">В **Добавление нового элемента — SignalR.StockTicker**выберите **установленные** > **Visual C#**   >  **Web**  >  **SignalR** , а затем выберите **класс концентратора SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="10abe-158">In **Add New Item - SignalR.StockTicker**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="10abe-159">Назовите класс *StockTickerHub* и добавьте его в проект.</span><span class="sxs-lookup"><span data-stu-id="10abe-159">Name the class *StockTickerHub* and add it to the project.</span></span>

    <span data-ttu-id="10abe-160">На этом шаге создается *StockTickerHub.cs* файл класса.</span><span class="sxs-lookup"><span data-stu-id="10abe-160">This step creates the *StockTickerHub.cs* class file.</span></span> <span data-ttu-id="10abe-161">Одновременно он добавляет набор файлов сценариев и ссылки на сборки, поддерживающий SignalR в проект.</span><span class="sxs-lookup"><span data-stu-id="10abe-161">Simultaneously, it adds a set of script files and assembly references that supports SignalR to the project.</span></span>

1. <span data-ttu-id="10abe-162">Замените код в *StockTickerHub.cs* файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="10abe-162">Replace the code in the *StockTickerHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. <span data-ttu-id="10abe-163">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="10abe-163">Save the file.</span></span>

<span data-ttu-id="10abe-164">Приложение использует [концентратора](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) класс определяет методы, которые клиенты могут вызывать на сервере.</span><span class="sxs-lookup"><span data-stu-id="10abe-164">The app uses the [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) class to define methods the clients can call on the server.</span></span> <span data-ttu-id="10abe-165">При определении одного метода: `GetAllStocks()`.</span><span class="sxs-lookup"><span data-stu-id="10abe-165">You're defining one method: `GetAllStocks()`.</span></span> <span data-ttu-id="10abe-166">Когда клиент сначала производит подключение к серверу, он вызывает этот метод, чтобы получить список всех акций с их текущие цены.</span><span class="sxs-lookup"><span data-stu-id="10abe-166">When a client initially connects to the server, it will call this method to get a list of all of the stocks with their current prices.</span></span> <span data-ttu-id="10abe-167">Метод может выполняться синхронно и возвращать `IEnumerable<Stock>` так, как он возвращает данные из памяти.</span><span class="sxs-lookup"><span data-stu-id="10abe-167">The method can run synchronously and return `IEnumerable<Stock>` because it's returning data from memory.</span></span>

<span data-ttu-id="10abe-168">Если метод должен получить данные, выполнив то, что будет включать в себя оповещения, такие как поиск в базе данных или вызов веб-службы, следует указать `Task<IEnumerable<Stock>>` как возвращаемое значение асинхронной обработки.</span><span class="sxs-lookup"><span data-stu-id="10abe-168">If the method had to get the data by doing something that would involve waiting, like a database lookup or a web service call, you would specify `Task<IEnumerable<Stock>>` as the return value to enable asynchronous processing.</span></span> <span data-ttu-id="10abe-169">Дополнительные сведения см. в разделе [ASP.NET руководство по API концентраторов SignalR - Server - когда следует выполнить асинхронно](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span><span class="sxs-lookup"><span data-stu-id="10abe-169">For more information, see [ASP.NET SignalR Hubs API Guide - Server - When to execute asynchronously](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span></span>

<span data-ttu-id="10abe-170">`HubName` Атрибут указывает, как приложение будет ссылаться в концентратор в код JavaScript на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="10abe-170">The `HubName` attribute specifies how the app will reference the Hub in JavaScript code on the client.</span></span> <span data-ttu-id="10abe-171">Имя по умолчанию на стороне клиента, если вы не используете этот атрибут является версией camelCase имя класса, который в данном случае было бы `stockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="10abe-171">The default name on the client if you don't use this attribute, is a camelCase version of the class name, which in this case would be `stockTickerHub`.</span></span>

<span data-ttu-id="10abe-172">Как можно будет увидеть позже при создании `StockTicker` класс, приложение создает одноэлементный экземпляр этого класса в его статическое `Instance` свойство.</span><span class="sxs-lookup"><span data-stu-id="10abe-172">As you'll see later when you create the `StockTicker` class, the app creates a singleton instance of that class in its static `Instance` property.</span></span> <span data-ttu-id="10abe-173">Этот одноэлементный экземпляр `StockTicker` находится в памяти независимо от того, сколько клиентов подключить или отключить.</span><span class="sxs-lookup"><span data-stu-id="10abe-173">That singleton instance of `StockTicker` is in memory no matter how many clients connect or disconnect.</span></span> <span data-ttu-id="10abe-174">Этот экземпляр является то, что `GetAllStocks()` метод используется для возврата текущего биржевых данных.</span><span class="sxs-lookup"><span data-stu-id="10abe-174">That instance is what the `GetAllStocks()` method uses to return current stock information.</span></span>

#### <a name="create-stocktickercs"></a><span data-ttu-id="10abe-175">Создание StockTicker.cs</span><span class="sxs-lookup"><span data-stu-id="10abe-175">Create StockTicker.cs</span></span>

1. <span data-ttu-id="10abe-176">В **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите **добавить** > **класс**.</span><span class="sxs-lookup"><span data-stu-id="10abe-176">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="10abe-177">Назовите класс *StockTicker* и добавьте его в проект.</span><span class="sxs-lookup"><span data-stu-id="10abe-177">Name the class *StockTicker* and add it to the project.</span></span>

1. <span data-ttu-id="10abe-178">Замените код в *StockTicker.cs* файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="10abe-178">Replace the code in the *StockTicker.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

<span data-ttu-id="10abe-179">Так как все потоки будут выполняться один и тот же экземпляр кода StockTicker, StockTicker класс должен быть поточно ориентированными.</span><span class="sxs-lookup"><span data-stu-id="10abe-179">Since all threads will be running the same instance of StockTicker code, the StockTicker class has to be thread-safe.</span></span>

### <a name="examine-the-server-code"></a><span data-ttu-id="10abe-180">Проверьте серверный код</span><span class="sxs-lookup"><span data-stu-id="10abe-180">Examine the server code</span></span>

<span data-ttu-id="10abe-181">Если посмотреть на код на сервере, он поможет вам понять, как работает приложение.</span><span class="sxs-lookup"><span data-stu-id="10abe-181">If you examine the server code, it will help you understand how the app works.</span></span>

#### <a name="storing-the-singleton-instance-in-a-static-field"></a><span data-ttu-id="10abe-182">Хранение одноэлементного экземпляра в статическом поле</span><span class="sxs-lookup"><span data-stu-id="10abe-182">Storing the singleton instance in a static field</span></span>

<span data-ttu-id="10abe-183">Этот код инициализирует статические `_instance` поля, поддерживающего `Instance` свойство с помощью экземпляра класса.</span><span class="sxs-lookup"><span data-stu-id="10abe-183">The code initializes the static `_instance` field that backs the `Instance` property with an instance of the class.</span></span> <span data-ttu-id="10abe-184">Поскольку конструктор является закрытым, он является единственным экземпляром класса, который можно создать приложение.</span><span class="sxs-lookup"><span data-stu-id="10abe-184">Because the constructor is private, it's the only instance of the class that the app can create.</span></span> <span data-ttu-id="10abe-185">Приложение использует [отложенной инициализации](/dotnet/framework/performance/lazy-initialization) для `_instance` поля.</span><span class="sxs-lookup"><span data-stu-id="10abe-185">The app uses [Lazy initialization](/dotnet/framework/performance/lazy-initialization) for the `_instance` field.</span></span> <span data-ttu-id="10abe-186">Они не используются для повышения производительности.</span><span class="sxs-lookup"><span data-stu-id="10abe-186">It's not for performance reasons.</span></span> <span data-ttu-id="10abe-187">Это, чтобы убедиться в том, что создание экземпляра является поточно ориентированной.</span><span class="sxs-lookup"><span data-stu-id="10abe-187">It's to make sure the instance creation is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

<span data-ttu-id="10abe-188">Каждый раз, клиент подключается к серверу, новый экземпляр класса StockTickerHub, работающих в отдельном потоке Получает одноэлементный экземпляр StockTicker из `StockTicker.Instance` статическое свойство, как показано ранее в `StockTickerHub` класса.</span><span class="sxs-lookup"><span data-stu-id="10abe-188">Each time a client connects to the server, a new instance of the StockTickerHub class running in a separate thread gets the StockTicker singleton instance from the `StockTicker.Instance` static property, as you saw earlier in the `StockTickerHub` class.</span></span>

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a><span data-ttu-id="10abe-189">Хранение данных акций в руководство.</span><span class="sxs-lookup"><span data-stu-id="10abe-189">Storing stock data in a ConcurrentDictionary</span></span>

<span data-ttu-id="10abe-190">Конструктор инициализирует `_stocks` коллекции с демонстрационными данными акций, и `GetAllStocks` возвращает акций.</span><span class="sxs-lookup"><span data-stu-id="10abe-190">The constructor initializes the `_stocks` collection with some sample stock data, and `GetAllStocks` returns the stocks.</span></span> <span data-ttu-id="10abe-191">Как было показано ранее, эта коллекция акций возвращается по `StockTickerHub.GetAllStocks`, являющийся серверного метода в `Hub` класс, который клиенты могут вызывать.</span><span class="sxs-lookup"><span data-stu-id="10abe-191">As you saw earlier, this collection of stocks is returned by `StockTickerHub.GetAllStocks`, which is a server method in the `Hub` class that clients can call.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

<span data-ttu-id="10abe-192">Представляет собой коллекцию акции [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) тип потокобезопасности.</span><span class="sxs-lookup"><span data-stu-id="10abe-192">The stocks collection is defined as a [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) type for thread safety.</span></span> <span data-ttu-id="10abe-193">В качестве альтернативы можно использовать [словарь](https://msdn.microsoft.com/library/xfhwa508.aspx) объекта и явным образом блокировка словаря, при внесении изменений в него.</span><span class="sxs-lookup"><span data-stu-id="10abe-193">As an alternative, you could use a [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) object and explicitly lock the dictionary when you make changes to it.</span></span>

<span data-ttu-id="10abe-194">Для этого примера приложения, достаточно для хранения данных приложений в памяти и потери данных, когда приложение освобождает `StockTicker` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="10abe-194">For this sample application, it's OK to store application data in memory and to lose the data when the app disposes of the  `StockTicker` instance.</span></span> <span data-ttu-id="10abe-195">В реальном приложении необходимо изменить с помощью хранилища данных серверной части, например базу данных.</span><span class="sxs-lookup"><span data-stu-id="10abe-195">In a real application, you would work with a back-end data store like a database.</span></span>

#### <a name="periodically-updating-stock-prices"></a><span data-ttu-id="10abe-196">Периодическое обновление акций</span><span class="sxs-lookup"><span data-stu-id="10abe-196">Periodically updating stock prices</span></span>

<span data-ttu-id="10abe-197">Запускается конструктор `Timer` объект, который периодически вызывает методы, которые обновляют акций на основе случайной выборки.</span><span class="sxs-lookup"><span data-stu-id="10abe-197">The constructor starts up a `Timer` object that periodically calls methods that update stock prices on a random basis.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 <span data-ttu-id="10abe-198">`Timer` вызовы `UpdateStockPrices`, который передает значения NULL в параметре state.</span><span class="sxs-lookup"><span data-stu-id="10abe-198">`Timer` calls `UpdateStockPrices`, which passes in null in the state parameter.</span></span> <span data-ttu-id="10abe-199">Перед обновлением цены, приложение принимает блокировку на `_updateStockPricesLock` объекта.</span><span class="sxs-lookup"><span data-stu-id="10abe-199">Before updating prices, the app takes a lock on the `_updateStockPricesLock` object.</span></span> <span data-ttu-id="10abe-200">Код проверяет, если другой поток уже обновляется цены, и затем вызывает `TryUpdateStockPrice` на каждой из них в списке.</span><span class="sxs-lookup"><span data-stu-id="10abe-200">The code checks if another thread is already updating prices, and then it calls `TryUpdateStockPrice` on each stock in the list.</span></span> <span data-ttu-id="10abe-201">`TryUpdateStockPrice` Метод решает, следует ли изменить цену акций и какой объем, чтобы изменить его.</span><span class="sxs-lookup"><span data-stu-id="10abe-201">The `TryUpdateStockPrice` method decides whether to change the stock price, and how much to change it.</span></span> <span data-ttu-id="10abe-202">Если изменяется акций, приложение вызывает `BroadcastStockPrice` рассылать изменения цены акций для всех подключенных клиентов.</span><span class="sxs-lookup"><span data-stu-id="10abe-202">If the stock price changes, the app calls `BroadcastStockPrice` to broadcast the stock price change to all connected clients.</span></span>

<span data-ttu-id="10abe-203">`_updatingStockPrices` Флаг назначенному [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) чтобы убедиться, что он является потокобезопасным.</span><span class="sxs-lookup"><span data-stu-id="10abe-203">The `_updatingStockPrices` flag designated [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) to make sure it is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

<span data-ttu-id="10abe-204">В реальном приложении `TryUpdateStockPrice` метод будет вызывать веб-службы для поиска цены.</span><span class="sxs-lookup"><span data-stu-id="10abe-204">In a real application, the `TryUpdateStockPrice` method would call a web service to look up the price.</span></span> <span data-ttu-id="10abe-205">В этом коде приложение использует генератор случайных чисел для внесения изменений случайным образом.</span><span class="sxs-lookup"><span data-stu-id="10abe-205">In this code, the app uses a random number generator to make changes randomly.</span></span>

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a><span data-ttu-id="10abe-206">Получение контекста SignalR, таким образом, чтобы класс StockTicker могут производить широковещательную рассылку для клиентов</span><span class="sxs-lookup"><span data-stu-id="10abe-206">Getting the SignalR context so that the StockTicker class can broadcast to clients</span></span>

<span data-ttu-id="10abe-207">Поскольку изменения цен здесь приходят в `StockTicker` объекта, это объект, которому требуется вызывать `updateStockPrice` метод на все подключенные клиенты.</span><span class="sxs-lookup"><span data-stu-id="10abe-207">Because the price changes originate here in the `StockTicker` object, it's the object that needs to call an `updateStockPrice` method on all connected clients.</span></span> <span data-ttu-id="10abe-208">В `Hub` класс, у вас есть API для вызова методов клиента, но `StockTicker` не является производным от `Hub` класса и не имеет ссылки на любое `Hub` объекта.</span><span class="sxs-lookup"><span data-stu-id="10abe-208">In a `Hub` class, you have an API for calling client methods, but `StockTicker` doesn't derive from the `Hub` class and doesn't have a reference to any `Hub` object.</span></span> <span data-ttu-id="10abe-209">Чтобы вещать на подключенных клиентов `StockTicker` класс должен получить экземпляр контекста SignalR для `StockTickerHub` класса и использовать его для вызова методов на клиентах.</span><span class="sxs-lookup"><span data-stu-id="10abe-209">To broadcast to connected clients, the `StockTicker` class has to get the SignalR context instance for the `StockTickerHub` class and use that to call methods on clients.</span></span>

<span data-ttu-id="10abe-210">Код получает ссылку на контекст SignalR, когда он создает одноэлементный экземпляр класса, которые ссылаются на передается конструктору, и конструктор поместит его в `Clients` свойство.</span><span class="sxs-lookup"><span data-stu-id="10abe-210">The code gets a reference to the SignalR context when it creates the singleton class instance, passes that reference to the constructor, and the constructor puts it in the `Clients` property.</span></span>

<span data-ttu-id="10abe-211">Есть две причины, почему вы хотите получить контекст только один раз: получение контекста является ресурсоемкие задачи и его после гарантирует приложение сохраняет последовательности сообщений, отправляемых клиентам.</span><span class="sxs-lookup"><span data-stu-id="10abe-211">There are two reasons why you want to get the context only once: getting the context is an expensive task, and getting it once makes sure the app preserves the intended order of messages sent to the clients.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

<span data-ttu-id="10abe-212">Начало `Clients` свойство контекста и поместив его `StockTickerClient` свойство позволяет написать код для вызова методов клиента, который выглядит так же, как это происходит в `Hub` класса.</span><span class="sxs-lookup"><span data-stu-id="10abe-212">Getting the `Clients` property of the context and putting it in the `StockTickerClient` property lets you write code to call client methods that looks the same as it would in a `Hub` class.</span></span> <span data-ttu-id="10abe-213">Например, чтобы вещать на все клиенты можно написать `Clients.All.updateStockPrice(stock)`.</span><span class="sxs-lookup"><span data-stu-id="10abe-213">For instance, to broadcast to all clients you can write `Clients.All.updateStockPrice(stock)`.</span></span>

<span data-ttu-id="10abe-214">`updateStockPrice` Метод, который вы вызываете в `BroadcastStockPrice` еще не существует.</span><span class="sxs-lookup"><span data-stu-id="10abe-214">The `updateStockPrice` method that you're calling in `BroadcastStockPrice` doesn't exist yet.</span></span> <span data-ttu-id="10abe-215">Она будет добавлена позже при написании кода, который выполняется на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="10abe-215">You'll add it later when you write code that runs on the client.</span></span> <span data-ttu-id="10abe-216">Можно ссылаться на `updateStockPrice` здесь потому что `Clients.All` является динамической, что означает, что приложение будет вычислено выражение во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="10abe-216">You can refer to `updateStockPrice` here because `Clients.All` is dynamic, which means the app will evaluate the expression at runtime.</span></span> <span data-ttu-id="10abe-217">При выполнении этого вызова метода, SignalR будет отправлять имя метода и значение параметра на клиенте, и если клиент имеет метод с именем `updateStockPrice`, приложение вызывает этот метод и передать значение параметра.</span><span class="sxs-lookup"><span data-stu-id="10abe-217">When this method call executes, SignalR will send the method name and the parameter value to the client, and if the client has a method named `updateStockPrice`, the app will call that method and pass the parameter value to it.</span></span>

<span data-ttu-id="10abe-218">`Clients.All` означает, что отправки всем клиентам.</span><span class="sxs-lookup"><span data-stu-id="10abe-218">`Clients.All` means send to all clients.</span></span> <span data-ttu-id="10abe-219">SignalR предоставляет другие параметры, чтобы указать, какие клиенты или группы клиентов для отправки.</span><span class="sxs-lookup"><span data-stu-id="10abe-219">SignalR gives you other options to specify which clients or groups of clients to send to.</span></span> <span data-ttu-id="10abe-220">Дополнительные сведения см. в разделе [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="10abe-220">For more information, see [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span></span>

### <a name="register-the-signalr-route"></a><span data-ttu-id="10abe-221">Регистрация маршрутов SignalR</span><span class="sxs-lookup"><span data-stu-id="10abe-221">Register the SignalR route</span></span>

<span data-ttu-id="10abe-222">Сервер должен знать, какой URL-адрес для перехвата и направить SignalR.</span><span class="sxs-lookup"><span data-stu-id="10abe-222">The server needs to know which URL to intercept and direct to SignalR.</span></span> <span data-ttu-id="10abe-223">Чтобы сделать это, добавьте класс запуска OWIN:</span><span class="sxs-lookup"><span data-stu-id="10abe-223">To do that, add an OWIN startup class:</span></span>

1. <span data-ttu-id="10abe-224">В **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите **добавить** > **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="10abe-224">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="10abe-225">В **Добавление нового элемента — SignalR.StockTicker** выберите **установленные** > **Visual C#**   >  **Web** и затем выберите **класс запуска OWIN**.</span><span class="sxs-lookup"><span data-stu-id="10abe-225">In **Add New Item - SignalR.StockTicker** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="10abe-226">Назовите класс *запуска* и выберите **ОК**.</span><span class="sxs-lookup"><span data-stu-id="10abe-226">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="10abe-227">Замените код по умолчанию в *Startup.cs* файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="10abe-227">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

<span data-ttu-id="10abe-228">Теперь вы завершили настройку серверный код.</span><span class="sxs-lookup"><span data-stu-id="10abe-228">You have now finished setting up the server code.</span></span> <span data-ttu-id="10abe-229">В следующем разделе настроим клиента.</span><span class="sxs-lookup"><span data-stu-id="10abe-229">In the next section, you'll set up the client.</span></span>

## <a name="set-up-the-client-code"></a><span data-ttu-id="10abe-230">Настройка кода клиента</span><span class="sxs-lookup"><span data-stu-id="10abe-230">Set up the client code</span></span>

<span data-ttu-id="10abe-231">В этом разделе настраивается код, который выполняется на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="10abe-231">In this section, you set up the code that runs on the client.</span></span>

### <a name="create-the-html-page-and-javascript-file"></a><span data-ttu-id="10abe-232">Создание HTML-страницы и файл JavaScript</span><span class="sxs-lookup"><span data-stu-id="10abe-232">Create the HTML page and JavaScript file</span></span>

<span data-ttu-id="10abe-233">HTML-страницу, отобразятся данные и данные будут организованы в файл JavaScript.</span><span class="sxs-lookup"><span data-stu-id="10abe-233">The HTML page will display the data and the JavaScript file will organize the data.</span></span>

#### <a name="create-stocktickerhtml"></a><span data-ttu-id="10abe-234">Создание StockTicker.html</span><span class="sxs-lookup"><span data-stu-id="10abe-234">Create StockTicker.html</span></span>

<span data-ttu-id="10abe-235">Во-первых мы добавим HTML-клиента.</span><span class="sxs-lookup"><span data-stu-id="10abe-235">First, you'll add the HTML client.</span></span>

1. <span data-ttu-id="10abe-236">В **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите **добавить** > **HTML-страницу**.</span><span class="sxs-lookup"><span data-stu-id="10abe-236">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="10abe-237">Назовите файл *StockTicker* и выберите **ОК**.</span><span class="sxs-lookup"><span data-stu-id="10abe-237">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="10abe-238">Замените код по умолчанию в *StockTicker.html* файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="10abe-238">Replace the default code in the *StockTicker.html* file with this code:</span></span>

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    <span data-ttu-id="10abe-239">HTML-код создается таблица с пятью столбцами, строку заголовка и строку данных, имеющего одну ячейку, которая охватывает все пять столбцов.</span><span class="sxs-lookup"><span data-stu-id="10abe-239">The HTML creates a table with five columns, a header row, and a data row with a single cell that spans all five columns.</span></span> <span data-ttu-id="10abe-240">Строки данных показывает «идет загрузка...» мгновенно при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="10abe-240">The data row shows "loading..." momentarily when the app starts.</span></span> <span data-ttu-id="10abe-241">Код JavaScript будет удалить эту строку и добавьте в строках месте биржевых данных, получаемых с сервера.</span><span class="sxs-lookup"><span data-stu-id="10abe-241">JavaScript code will remove that row and add in its place rows with stock data retrieved from the server.</span></span>

    <span data-ttu-id="10abe-242">Укажите теги сценария:</span><span class="sxs-lookup"><span data-stu-id="10abe-242">The script tags specify:</span></span>

    * <span data-ttu-id="10abe-243">Файл скрипта jQuery.</span><span class="sxs-lookup"><span data-stu-id="10abe-243">The jQuery script file.</span></span>

    * <span data-ttu-id="10abe-244">Файл скрипта SignalR core.</span><span class="sxs-lookup"><span data-stu-id="10abe-244">The SignalR core script file.</span></span>

    * <span data-ttu-id="10abe-245">Файл скрипта учетные записи-посредники SignalR.</span><span class="sxs-lookup"><span data-stu-id="10abe-245">The SignalR proxies script file.</span></span>

    * <span data-ttu-id="10abe-246">Файл сценария StockTicker, будет создано позже.</span><span class="sxs-lookup"><span data-stu-id="10abe-246">A StockTicker script file that you'll create later.</span></span>

    <span data-ttu-id="10abe-247">Приложение динамически создает файл скрипта учетные записи-посредники SignalR.</span><span class="sxs-lookup"><span data-stu-id="10abe-247">The app dynamically generates the SignalR proxies script file.</span></span> <span data-ttu-id="10abe-248">Он указывает URL-адрес «/ signalr/концентраторы» и определяет методы прокси-сервера для методов применительно к классу Hub в данном случае для `StockTickerHub.GetAllStocks`.</span><span class="sxs-lookup"><span data-stu-id="10abe-248">It specifies the "/signalr/hubs" URL and defines proxy methods for the methods on the Hub class, in this case, for `StockTickerHub.GetAllStocks`.</span></span> <span data-ttu-id="10abe-249">При желании можно создать этот файл JavaScript вручную с помощью [служебные программы SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/).</span><span class="sxs-lookup"><span data-stu-id="10abe-249">If you prefer, you can generate this JavaScript file manually by using [SignalR Utilities](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/).</span></span> <span data-ttu-id="10abe-250">Не забудьте отключить создание динамического файла в `MapHubs` вызов метода.</span><span class="sxs-lookup"><span data-stu-id="10abe-250">Don't forget to disable dynamic file creation in the `MapHubs` method call.</span></span>

1. <span data-ttu-id="10abe-251">В **обозревателе решений**, разверните **сценариев**.</span><span class="sxs-lookup"><span data-stu-id="10abe-251">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="10abe-252">Библиотеки скрипта для jQuery и SignalR, отображаются в проект.</span><span class="sxs-lookup"><span data-stu-id="10abe-252">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="10abe-253">Диспетчер пакетов будет установить более позднюю версию сценариев SignalR.</span><span class="sxs-lookup"><span data-stu-id="10abe-253">The package manager will install a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="10abe-254">Обновите ссылки на скрипты в блоке кода в соответствии с версии файлов скриптов в проекте.</span><span class="sxs-lookup"><span data-stu-id="10abe-254">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

1. <span data-ttu-id="10abe-255">В **обозревателе решений**, щелкните правой кнопкой мыши *StockTicker.html*, а затем выберите **задать в качестве начальной страницы**.</span><span class="sxs-lookup"><span data-stu-id="10abe-255">In **Solution Explorer**, right-click *StockTicker.html*, and then select **Set as Start Page**.</span></span>

#### <a name="create-stocktickerjs"></a><span data-ttu-id="10abe-256">Создание StockTicker.js</span><span class="sxs-lookup"><span data-stu-id="10abe-256">Create StockTicker.js</span></span>

<span data-ttu-id="10abe-257">Теперь создайте файл JavaScript.</span><span class="sxs-lookup"><span data-stu-id="10abe-257">Now create the JavaScript file.</span></span>

1. <span data-ttu-id="10abe-258">В **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите **добавить** > **файл JavaScript**.</span><span class="sxs-lookup"><span data-stu-id="10abe-258">In **Solution Explorer**, right-click the project and select **Add** > **JavaScript File**.</span></span>

1. <span data-ttu-id="10abe-259">Назовите файл *StockTicker* и выберите **ОК**.</span><span class="sxs-lookup"><span data-stu-id="10abe-259">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="10abe-260">Добавьте следующий код в *StockTicker.js* файла:</span><span class="sxs-lookup"><span data-stu-id="10abe-260">Add this code to the *StockTicker.js* file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a><span data-ttu-id="10abe-261">Изучите код клиента</span><span class="sxs-lookup"><span data-stu-id="10abe-261">Examine the client code</span></span>

<span data-ttu-id="10abe-262">Если вы изучите код клиента, поможет вам узнать, как клиентский код взаимодействует с серверный код, чтобы сделать работу приложения.</span><span class="sxs-lookup"><span data-stu-id="10abe-262">If you examine the client code, it will help you learn how the client code interacts with the server code to make the app work.</span></span>

#### <a name="starting-the-connection"></a><span data-ttu-id="10abe-263">При подключении</span><span class="sxs-lookup"><span data-stu-id="10abe-263">Starting the connection</span></span>

<span data-ttu-id="10abe-264">`$.connection` ссылается на прокси-серверы SignalR.</span><span class="sxs-lookup"><span data-stu-id="10abe-264">`$.connection` refers to the SignalR proxies.</span></span> <span data-ttu-id="10abe-265">Код получает ссылку на прокси-сервер для `StockTickerHub` класса и помещает его `ticker` переменной.</span><span class="sxs-lookup"><span data-stu-id="10abe-265">The code gets a reference to the proxy for the `StockTickerHub` class and puts it in the `ticker` variable.</span></span> <span data-ttu-id="10abe-266">Имя прокси-сервера — это имя, установленное с `HubName` атрибут:</span><span class="sxs-lookup"><span data-stu-id="10abe-266">The proxy name is the name that was set by the `HubName` attribute:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

<span data-ttu-id="10abe-267">После определения всех переменных и функций, в последней строке кода в файле инициализирует подключения SignalR путем вызова SignalR `start` функции.</span><span class="sxs-lookup"><span data-stu-id="10abe-267">After you define all the variables and functions, the last line of code in the file initializes the SignalR connection by calling the SignalR `start` function.</span></span> <span data-ttu-id="10abe-268">`start` Функция выполняется асинхронно и возвращает [объект jQuery отложенный](http://api.jquery.com/category/deferred-object/).</span><span class="sxs-lookup"><span data-stu-id="10abe-268">The `start` function executes asynchronously and returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/).</span></span> <span data-ttu-id="10abe-269">Можно вызвать функцию Готово для указания функции, вызываемой по завершении асинхронного действия приложения.</span><span class="sxs-lookup"><span data-stu-id="10abe-269">You can call the done function to specify the function to call when the app finishes the asynchronous action.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a><span data-ttu-id="10abe-270">Получение всех акции</span><span class="sxs-lookup"><span data-stu-id="10abe-270">Getting all the stocks</span></span>

<span data-ttu-id="10abe-271">`init` Вызовы функций `getAllStocks` функции на сервере и на основании данных, возвращаемый сервером для обновления таблицы акций.</span><span class="sxs-lookup"><span data-stu-id="10abe-271">The `init` function calls the `getAllStocks` function on the server and uses the information that the server returns to update the stock table.</span></span> <span data-ttu-id="10abe-272">Обратите внимание, что, по умолчанию, необходимо использовать camelCasing на клиенте, несмотря на то, что имя метода в стиле Pascal на сервере.</span><span class="sxs-lookup"><span data-stu-id="10abe-272">Notice that, by default, you have to use camelCasing on the client even though the method name is pascal-cased on the server.</span></span> <span data-ttu-id="10abe-273">CamelCasing правило применяется только к методам, не объекты.</span><span class="sxs-lookup"><span data-stu-id="10abe-273">The camelCasing rule only applies to methods, not objects.</span></span> <span data-ttu-id="10abe-274">Например, ссылке на `stock.Symbol` и `stock.Price`, а не `stock.symbol` или `stock.price`.</span><span class="sxs-lookup"><span data-stu-id="10abe-274">For example, you refer to `stock.Symbol` and `stock.Price`, not `stock.symbol` or `stock.price`.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

<span data-ttu-id="10abe-275">В `init` метод, приложение создает HTML для строки таблицы для каждого объекта акций, полученный от сервера путем вызова `formatStock` для свойства форматирования `stock` объекта, а затем путем вызова `supplant` для замещения местозаполнителей в `rowTemplate` переменной с `stock` значений свойств объектов.</span><span class="sxs-lookup"><span data-stu-id="10abe-275">In the `init` method, the app creates HTML for a table row for each stock object received from the server by calling `formatStock` to format properties of the `stock` object, and then by calling `supplant` to replace placeholders in the `rowTemplate` variable with the `stock` object property values.</span></span> <span data-ttu-id="10abe-276">Затем полученный HTML добавляется к таблице акций.</span><span class="sxs-lookup"><span data-stu-id="10abe-276">The resulting HTML is then appended to the stock table.</span></span>

> [!NOTE]
> <span data-ttu-id="10abe-277">Вы вызываете `init` , передав его в виде `callback` функцию, которая выполняется после асинхронного `start` функции завершения.</span><span class="sxs-lookup"><span data-stu-id="10abe-277">You call `init` by passing it in as a `callback` function that executes after the asynchronous `start` function finishes.</span></span> <span data-ttu-id="10abe-278">Если вызван `init` как отдельный оператор JavaScript после вызова метода `start`, функция будет ошибкой, так как оно будет выполняться немедленно, не дожидаясь функции запуска для завершения подключения к.</span><span class="sxs-lookup"><span data-stu-id="10abe-278">If you called `init` as a separate JavaScript statement after calling `start`, the function would fail because it would run immediately without waiting for the start function to finish establishing the connection.</span></span> <span data-ttu-id="10abe-279">В этом случае `init` функция попытается вызвать `getAllStocks` функцию, прежде чем приложение устанавливает соединение с сервером.</span><span class="sxs-lookup"><span data-stu-id="10abe-279">In that case, the `init` function would try to call the `getAllStocks` function before the app establishes a server connection.</span></span>

#### <a name="getting-updated-stock-prices"></a><span data-ttu-id="10abe-280">Получение обновленной акций.</span><span class="sxs-lookup"><span data-stu-id="10abe-280">Getting updated stock prices</span></span>

<span data-ttu-id="10abe-281">Когда сервер изменяет цены акции, он вызывает `updateStockPrice` на подключенных клиентов.</span><span class="sxs-lookup"><span data-stu-id="10abe-281">When the server changes a stock's price, it calls the `updateStockPrice` on connected clients.</span></span> <span data-ttu-id="10abe-282">Приложение добавляет функцию свойству клиентского объекта `stockTicker` прокси-сервера, чтобы сделать его доступным для вызовов с сервера.</span><span class="sxs-lookup"><span data-stu-id="10abe-282">The app adds the function to the client property of the `stockTicker` proxy to make it available to calls from the server.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

<span data-ttu-id="10abe-283">`updateStockPrice` Форматы функция stock-объектом получил от сервера в таблицу строк так же, как в `init` функции.</span><span class="sxs-lookup"><span data-stu-id="10abe-283">The `updateStockPrice` function formats a stock object received from the server into a table row the same way as in the `init` function.</span></span> <span data-ttu-id="10abe-284">Вместо того чтобы добавить строку в таблицу, он находит акции текущей строки в таблице и заменяет эту строку на новую.</span><span class="sxs-lookup"><span data-stu-id="10abe-284">Instead of appending the row to the table, it finds the stock's current row in the table and replaces that row with the new one.</span></span>

## <a name="test-the-application"></a><span data-ttu-id="10abe-285">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="10abe-285">Test the application</span></span>

<span data-ttu-id="10abe-286">Можно протестировать приложение, чтобы убедиться, что он работает.</span><span class="sxs-lookup"><span data-stu-id="10abe-286">You can test the app to make sure it's working.</span></span> <span data-ttu-id="10abe-287">Вы увидите все окна браузера отображения динамической таблицы акций акций постоянно меняется.</span><span class="sxs-lookup"><span data-stu-id="10abe-287">You'll see all browser windows display the live stock table with stock prices fluctuating.</span></span>

1. <span data-ttu-id="10abe-288">На панели инструментов, включите **отладки скриптов** и затем нажмите кнопку воспроизведения, чтобы запустить приложение в режиме отладки.</span><span class="sxs-lookup"><span data-stu-id="10abe-288">In the toolbar, turn on **Script Debugging** and then select the play button to run the app in Debug mode.</span></span>

    ![Снимок экрана: Включение режим отладки и выбрав play пользователя.](tutorial-server-broadcast-with-signalr/_static/image4.png)

    <span data-ttu-id="10abe-290">Откроется окно браузера на отображение **Live Stock таблицы**.</span><span class="sxs-lookup"><span data-stu-id="10abe-290">A browser window will open displaying the **Live Stock Table**.</span></span> <span data-ttu-id="10abe-291">Таблицы акций изначально отображает строки «загрузка...», затем, через некоторое время, приложение показывает начальные данные акций и запустите акций для изменения.</span><span class="sxs-lookup"><span data-stu-id="10abe-291">The stock table initially shows the "loading..." line, then, after a short time, the app shows the initial stock data, and then the stock prices start to change.</span></span>

1. <span data-ttu-id="10abe-292">Скопируйте URL-адрес из браузера, откройте два других браузеров и вставьте URL-адреса в панели адрес.</span><span class="sxs-lookup"><span data-stu-id="10abe-292">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

    <span data-ttu-id="10abe-293">Исходное отображение биржевых совпадает с первым браузером, и изменения происходят одновременно.</span><span class="sxs-lookup"><span data-stu-id="10abe-293">The initial stock display is the same as the first browser and changes happen simultaneously.</span></span>

1. <span data-ttu-id="10abe-294">Закройте все браузеры, открыть обозреватель и перейдите к тем же URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="10abe-294">Close all browsers, open a new browser, and go to the same URL.</span></span>

    <span data-ttu-id="10abe-295">Одноэлементный объект StockTicker продолжали работать на сервере.</span><span class="sxs-lookup"><span data-stu-id="10abe-295">The StockTicker singleton object continued to run in the server.</span></span> <span data-ttu-id="10abe-296">**Live Stock таблицы** показывает, продолжают акций для изменения.</span><span class="sxs-lookup"><span data-stu-id="10abe-296">The **Live Stock Table** shows that the stocks have continued to change.</span></span> <span data-ttu-id="10abe-297">Вы не видите первоначальной таблицы с нуля изменить данные.</span><span class="sxs-lookup"><span data-stu-id="10abe-297">You don't see the initial table with zero change figures.</span></span>

1. <span data-ttu-id="10abe-298">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="10abe-298">Close the browser.</span></span>

## <a name="enable-logging"></a><span data-ttu-id="10abe-299">Включение ведения журнала</span><span class="sxs-lookup"><span data-stu-id="10abe-299">Enable logging</span></span>

<span data-ttu-id="10abe-300">SignalR имеет встроенные функции, которую можно включить на стороне клиента для устранения неисправностей.</span><span class="sxs-lookup"><span data-stu-id="10abe-300">SignalR has a built-in logging function that you can enable on the client to aid in troubleshooting.</span></span> <span data-ttu-id="10abe-301">В этом разделе включите ведение журнала и примеры, показывающие, как журналы скажет, какой из следующих методов транспорта использует SignalR см. в разделе:</span><span class="sxs-lookup"><span data-stu-id="10abe-301">In this section, you enable logging and see examples that show how logs tell you which of the following transport methods SignalR is using:</span></span>

* <span data-ttu-id="10abe-302">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), поддерживается IIS 8 и современных обозревателей.</span><span class="sxs-lookup"><span data-stu-id="10abe-302">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), supported by IIS 8 and current browsers.</span></span>

* <span data-ttu-id="10abe-303">[Сервер отправил события](http://en.wikipedia.org/wiki/Server-sent_events), поддерживаемых браузеров, отличных от Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="10abe-303">[Server-sent events](http://en.wikipedia.org/wiki/Server-sent_events), supported by browsers other than Internet Explorer.</span></span>

* <span data-ttu-id="10abe-304">[Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), поддерживаемых обозревателем Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="10abe-304">[Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), supported by Internet Explorer.</span></span>

* <span data-ttu-id="10abe-305">[AJAX, длинный опрос](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), поддерживаемый всеми браузерами.</span><span class="sxs-lookup"><span data-stu-id="10abe-305">[Ajax long polling](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), supported by all browsers.</span></span>

<span data-ttu-id="10abe-306">Для любого соединения SignalR выбирает лучший метод транспорта, сервер и клиент поддерживают.</span><span class="sxs-lookup"><span data-stu-id="10abe-306">For any given connection, SignalR chooses the best transport method that both the server and the client support.</span></span>

1. <span data-ttu-id="10abe-307">Откройте *StockTicker.js*.</span><span class="sxs-lookup"><span data-stu-id="10abe-307">Open *StockTicker.js*.</span></span>

1. <span data-ttu-id="10abe-308">Добавление выделенной строкой кода, чтобы включить ведение журнала непосредственно перед код, который инициализирует подключение в конце файла:</span><span class="sxs-lookup"><span data-stu-id="10abe-308">Add this highlighted line of code to enable logging immediately before the code that initializes the connection at the end of the file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. <span data-ttu-id="10abe-309">Нажмите клавишу **F5**, чтобы запустить проект.</span><span class="sxs-lookup"><span data-stu-id="10abe-309">Press **F5** to run the project.</span></span>

1. <span data-ttu-id="10abe-310">Откройте окно инструментов разработчика в браузере и выберите консоль, чтобы просмотреть журналы.</span><span class="sxs-lookup"><span data-stu-id="10abe-310">Open your browser's developer tools window, and select the Console to see the logs.</span></span> <span data-ttu-id="10abe-311">Может потребоваться обновить страницу, чтобы просмотреть журналы SignalR согласования метода транспорта для нового соединения.</span><span class="sxs-lookup"><span data-stu-id="10abe-311">You might have to refresh the page to see the logs of SignalR negotiating the transport method for a new connection.</span></span>

    * <span data-ttu-id="10abe-312">Если вы используете Internet Explorer 10 в Windows 8 (IIS 8), метод транспорта является **WebSockets**.</span><span class="sxs-lookup"><span data-stu-id="10abe-312">If you're running Internet Explorer 10 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

    * <span data-ttu-id="10abe-313">Если вы используете Internet Explorer 10 в Windows 7 (IIS 7.5), метод транспорта является **iframe**.</span><span class="sxs-lookup"><span data-stu-id="10abe-313">If you're running Internet Explorer 10 on Windows 7 (IIS 7.5), the transport method is **iframe**.</span></span>

    * <span data-ttu-id="10abe-314">Если вы используете Firefox 19 в Windows 8 (IIS 8), метод транспорта является **WebSockets**.</span><span class="sxs-lookup"><span data-stu-id="10abe-314">If you're running Firefox 19 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

        > [!TIP]
        > <span data-ttu-id="10abe-315">В обозревателе Firefox установите надстройка Firebug, чтобы получать окно консоли.</span><span class="sxs-lookup"><span data-stu-id="10abe-315">In Firefox, install the Firebug add-in to get a Console window.</span></span>

    * <span data-ttu-id="10abe-316">Если вы используете Firefox 19 в Windows 7 (IIS 7.5), метод транспорта является **сервер отправил** события.</span><span class="sxs-lookup"><span data-stu-id="10abe-316">If you're running Firefox 19 on Windows 7 (IIS 7.5), the transport method is **server-sent** events.</span></span>

## <a name="install-the-stockticker-sample"></a><span data-ttu-id="10abe-317">Установка образца StockTicker</span><span class="sxs-lookup"><span data-stu-id="10abe-317">Install the StockTicker sample</span></span>

<span data-ttu-id="10abe-318">[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) устанавливает приложение StockTicker.</span><span class="sxs-lookup"><span data-stu-id="10abe-318">The [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installs the StockTicker application.</span></span> <span data-ttu-id="10abe-319">Пакет NuGet включает больше возможностей, чем упрощенная версия, которая создана с нуля.</span><span class="sxs-lookup"><span data-stu-id="10abe-319">The NuGet package includes more features than the simplified version that you created from scratch.</span></span> <span data-ttu-id="10abe-320">В этом разделе руководства вы установите пакет NuGet и просмотрите новые функции и код, который реализует их.</span><span class="sxs-lookup"><span data-stu-id="10abe-320">In this section of the tutorial, you install the NuGet package and review the new features and the code that implements them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="10abe-321">Если установить пакет без выполнения на предыдущих шагах этого учебника, необходимо добавить класс запуска OWIN в проект.</span><span class="sxs-lookup"><span data-stu-id="10abe-321">If you install the package without performing the earlier steps of this tutorial, you must add an OWIN startup class to your project.</span></span> <span data-ttu-id="10abe-322">Этот файл readme.txt для пакета NuGet описывает этот шаг.</span><span class="sxs-lookup"><span data-stu-id="10abe-322">This readme.txt file for the NuGet package explains this step.</span></span>

### <a name="install-the-signalrsample-nuget-package"></a><span data-ttu-id="10abe-323">Установите пакет SignalR.Sample NuGet</span><span class="sxs-lookup"><span data-stu-id="10abe-323">Install the SignalR.Sample NuGet package</span></span>

1. <span data-ttu-id="10abe-324">В **обозревателе решений** щелкните проект правой кнопкой мыши и выберите **Управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="10abe-324">In **Solution Explorer**, right-click the project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="10abe-325">В **диспетчер пакетов NuGet: SignalR.StockTicker**выберите **Обзор**.</span><span class="sxs-lookup"><span data-stu-id="10abe-325">In **NuGet Package manager: SignalR.StockTicker**, select **Browse**.</span></span>

1. <span data-ttu-id="10abe-326">Из **источник пакета**выберите **nuget.org**.</span><span class="sxs-lookup"><span data-stu-id="10abe-326">From **Package source**, select **nuget.org**.</span></span>

1. <span data-ttu-id="10abe-327">Введите *SignalR.Sample* в поле поиска и выберите **Microsoft.AspNet.SignalR.Sample** > **установить**.</span><span class="sxs-lookup"><span data-stu-id="10abe-327">Enter *SignalR.Sample* in the search box and select **Microsoft.AspNet.SignalR.Sample** > **Install**.</span></span>

1. <span data-ttu-id="10abe-328">В **обозревателе решений**, разверните *SignalR.Sample* папки.</span><span class="sxs-lookup"><span data-stu-id="10abe-328">In **Solution Explorer**, expand the *SignalR.Sample* folder.</span></span>

    <span data-ttu-id="10abe-329">Установка пакета SignalR.Sample создана папка и ее содержимое.</span><span class="sxs-lookup"><span data-stu-id="10abe-329">Installing the SignalR.Sample package created the folder and its contents.</span></span>

1. <span data-ttu-id="10abe-330">В *SignalR.Sample* папку, щелкните правой кнопкой мыши *StockTicker.html*, а затем выберите **задать в качестве начальной страницы**.</span><span class="sxs-lookup"><span data-stu-id="10abe-330">In the *SignalR.Sample* folder, right-click *StockTicker.html*, and then select **Set As Start Page**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="10abe-331">Установка SignalR.Sample NuGet пакета может изменить версию jQuery, у вас есть в вашей *сценариев* папки.</span><span class="sxs-lookup"><span data-stu-id="10abe-331">Installing The SignalR.Sample NuGet package might change the version of jQuery that you have in your *Scripts* folder.</span></span> <span data-ttu-id="10abe-332">Новый *StockTicker.html* файл пакета установки в *SignalR.Sample* папки будут синхронизированы с версии jQuery, которая устанавливает пакет, но если вы хотите запустить исходную *StockTicker.html* файл еще раз, необходимо сначала обновить ссылку на jQuery в тег сценария.</span><span class="sxs-lookup"><span data-stu-id="10abe-332">The new *StockTicker.html* file that the package installs in the *SignalR.Sample* folder will be in sync with the jQuery version that the package installs, but if you want to run your original *StockTicker.html* file again, you might have to update the jQuery reference in the script tag first.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="10abe-333">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="10abe-333">Run the application</span></span>

 <span data-ttu-id="10abe-334">В таблице, который вы видели в первом приложении было полезных функций.</span><span class="sxs-lookup"><span data-stu-id="10abe-334">The table that you saw in the first app had useful features.</span></span> <span data-ttu-id="10abe-335">Приложение полной биржевые сводки показывает новые функции: горизонтальной прокруткой окно, отображающее биржевых данных и акции, если изменить цвет, как растет и делятся.</span><span class="sxs-lookup"><span data-stu-id="10abe-335">The full stock ticker application shows new features: a horizontally scrolling window that shows the stock data and stocks that change color as they rise and fall.</span></span>

1. <span data-ttu-id="10abe-336">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="10abe-336">Press **F5** to run the app.</span></span>

     <span data-ttu-id="10abe-337">При первом запуске приложения, «Марка» «закрыто», и вы увидите создается статическая таблица и окно биржевых котировок, которое не прокрутки.</span><span class="sxs-lookup"><span data-stu-id="10abe-337">When you run the app for the first time, the "market" is "closed" and you see a static table and a ticker window that isn't scrolling.</span></span>

1. <span data-ttu-id="10abe-338">Выберите **Open Market**.</span><span class="sxs-lookup"><span data-stu-id="10abe-338">Select **Open Market**.</span></span>

    ![Снимок экрана: live биржевых котировок.](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * <span data-ttu-id="10abe-340">**Live биржевых котировок акций** поле начинает прокручиваться по горизонтали, и сервер запускается периодически рассылать изменения цены акций на основе случайной выборки.</span><span class="sxs-lookup"><span data-stu-id="10abe-340">The **Live Stock Ticker** box starts to scroll horizontally, and the server starts to periodically broadcast stock price changes on a random basis.</span></span>

    * <span data-ttu-id="10abe-341">Каждый раз изменяет цены акций, приложение обновляет оба **Live Stock таблицы** и **Live биржевых котировок акций**.</span><span class="sxs-lookup"><span data-stu-id="10abe-341">Each time a stock price changes, the app updates both the **Live Stock Table** and the **Live Stock Ticker**.</span></span>

    * <span data-ttu-id="10abe-342">При изменении цены акции является положительным, в приложении отображается акции с зеленым фоном.</span><span class="sxs-lookup"><span data-stu-id="10abe-342">When a stock's price change is positive, the app shows the stock with a green background.</span></span>

    * <span data-ttu-id="10abe-343">Если изменение является отрицательным, в приложении отображается акции с красным фоном.</span><span class="sxs-lookup"><span data-stu-id="10abe-343">When the change is negative, the app shows the stock with a red background.</span></span>

1. <span data-ttu-id="10abe-344">Выберите **закрыть рынка**.</span><span class="sxs-lookup"><span data-stu-id="10abe-344">Select **Close Market**.</span></span>

    * <span data-ttu-id="10abe-345">Таблицы обновляет stop.</span><span class="sxs-lookup"><span data-stu-id="10abe-345">The table updates stop.</span></span>

    * <span data-ttu-id="10abe-346">Биржевых котировок останавливает прокрутки.</span><span class="sxs-lookup"><span data-stu-id="10abe-346">The ticker stops scrolling.</span></span>

1. <span data-ttu-id="10abe-347">Выберите **сбросить**.</span><span class="sxs-lookup"><span data-stu-id="10abe-347">Select **Reset**.</span></span>

    * <span data-ttu-id="10abe-348">Выполняется сброс всех биржевых данных.</span><span class="sxs-lookup"><span data-stu-id="10abe-348">All stock data is reset.</span></span>

    * <span data-ttu-id="10abe-349">Приложение восстанавливает начальное состояние до изменения цен к работе.</span><span class="sxs-lookup"><span data-stu-id="10abe-349">The app restores the initial state before price changes started.</span></span>

1. <span data-ttu-id="10abe-350">Скопируйте URL-адрес из браузера, откройте два других браузеров и вставьте URL-адреса в панели адрес.</span><span class="sxs-lookup"><span data-stu-id="10abe-350">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="10abe-351">Вы видите те же данные, динамически обновляются в то же время, в каждом браузере.</span><span class="sxs-lookup"><span data-stu-id="10abe-351">You see the same data dynamically updated at the same time in each browser.</span></span>

1. <span data-ttu-id="10abe-352">При выборе любого из элементов управления, все браузеры одинаково реагировать на в то же время.</span><span class="sxs-lookup"><span data-stu-id="10abe-352">When you select any of the controls, all browsers respond the same way at the same time.</span></span>

### <a name="live-stock-ticker-display"></a><span data-ttu-id="10abe-353">Динамическое отображение биржевых котировок акций</span><span class="sxs-lookup"><span data-stu-id="10abe-353">Live Stock Ticker display</span></span>

<span data-ttu-id="10abe-354">**Live биржевых котировок акций** отображается неупорядоченный список в `<div>` элемент в формате в одну строку от стилей CSS.</span><span class="sxs-lookup"><span data-stu-id="10abe-354">The **Live Stock Ticker** display is an unordered list in a `<div>` element formatted into a single line by CSS styles.</span></span> <span data-ttu-id="10abe-355">Инициализирует приложение и обновляет биржевых котировок так же, как таблицы:, заменив заполнители в `<li>` строка шаблона и динамическое добавление `<li>` элементы `<ul>` элемент.</span><span class="sxs-lookup"><span data-stu-id="10abe-355">The app initializes and updates the ticker the same way as the table: by replacing placeholders in an `<li>` template string and dynamically adding the `<li>` elements to the `<ul>` element.</span></span> <span data-ttu-id="10abe-356">Приложение включает прокрутку с помощью jQuery `animate` функции для изменения margin-left неупорядоченного списка в пределах `<div>`.</span><span class="sxs-lookup"><span data-stu-id="10abe-356">The app includes  scrolling by using the jQuery `animate` function to vary the margin-left of the unordered list within the `<div>`.</span></span>

#### <a name="signalrsample-stocktickerhtml"></a><span data-ttu-id="10abe-357">SignalR.Sample StockTicker.html</span><span class="sxs-lookup"><span data-stu-id="10abe-357">SignalR.Sample StockTicker.html</span></span>

<span data-ttu-id="10abe-358">Биржевые сводки HTML-код:</span><span class="sxs-lookup"><span data-stu-id="10abe-358">The stock ticker HTML code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a><span data-ttu-id="10abe-359">SignalR.Sample StockTicker.css</span><span class="sxs-lookup"><span data-stu-id="10abe-359">SignalR.Sample StockTicker.css</span></span>

<span data-ttu-id="10abe-360">Биржевые сводки код CSS:</span><span class="sxs-lookup"><span data-stu-id="10abe-360">The stock ticker CSS code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a><span data-ttu-id="10abe-361">SignalR.Sample SignalR.StockTicker.js</span><span class="sxs-lookup"><span data-stu-id="10abe-361">SignalR.Sample SignalR.StockTicker.js</span></span>

<span data-ttu-id="10abe-362">Прокрутите кода jQuery, что позволяет:</span><span class="sxs-lookup"><span data-stu-id="10abe-362">The jQuery code that makes it scroll:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a><span data-ttu-id="10abe-363">Дополнительные методы на сервере, который клиент может вызывать</span><span class="sxs-lookup"><span data-stu-id="10abe-363">Additional methods on the server that the client can call</span></span>

<span data-ttu-id="10abe-364">Чтобы добавить гибкости в приложение, существуют дополнительные методы, которые приложение может вызывать.</span><span class="sxs-lookup"><span data-stu-id="10abe-364">To add flexibility to the app, there are additional methods the app can call.</span></span>

#### <a name="signalrsample-stocktickerhubcs"></a><span data-ttu-id="10abe-365">SignalR.Sample StockTickerHub.cs</span><span class="sxs-lookup"><span data-stu-id="10abe-365">SignalR.Sample StockTickerHub.cs</span></span>

<span data-ttu-id="10abe-366">`StockTickerHub` Класс определяет четыре дополнительные методы, которые клиент может вызывать:</span><span class="sxs-lookup"><span data-stu-id="10abe-366">The `StockTickerHub` class defines four additional methods that the client can call:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

<span data-ttu-id="10abe-367">Приложение вызывает `OpenMarket`, `CloseMarket`, и `Reset` в ответ на кнопки в верхней части страницы.</span><span class="sxs-lookup"><span data-stu-id="10abe-367">The app calls `OpenMarket`, `CloseMarket`, and `Reset` in response to the buttons at the top of the page.</span></span> <span data-ttu-id="10abe-368">Они показывают шаблон из одного клиента, активируя изменение в состоянии, немедленно распространяются на все клиенты.</span><span class="sxs-lookup"><span data-stu-id="10abe-368">They demonstrate the pattern of one client triggering a change in state immediately propagated to all clients.</span></span> <span data-ttu-id="10abe-369">Каждый из этих методов вызывает метод в `StockTicker` класс, который вызывает изменение состояния рынка и затем осуществляет широковещательную передачу новое состояние.</span><span class="sxs-lookup"><span data-stu-id="10abe-369">Each of these methods calls a method in the `StockTicker` class that causes the market state change and then broadcasts the new state.</span></span>

#### <a name="signalrsample-stocktickercs"></a><span data-ttu-id="10abe-370">SignalR.Sample StockTicker.cs</span><span class="sxs-lookup"><span data-stu-id="10abe-370">SignalR.Sample StockTicker.cs</span></span>

<span data-ttu-id="10abe-371">В `StockTicker` класс, приложение сохраняет сведения о состоянии рынка с `MarketState` свойство, которое возвращает `MarketState` значение перечисления:</span><span class="sxs-lookup"><span data-stu-id="10abe-371">In the `StockTicker` class, the app maintains the state of the market with a `MarketState` property that returns a `MarketState` enum value:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

<span data-ttu-id="10abe-372">Каждый из методов, изменяющих состояние рынка делать это внутри блок lock поскольку `StockTicker` класс должен быть поточно ориентированные:</span><span class="sxs-lookup"><span data-stu-id="10abe-372">Each of the methods that change the market state do so inside a lock block because the `StockTicker` class has to be thread-safe:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

<span data-ttu-id="10abe-373">Чтобы убедиться, что этот код является поточно ориентированной, `_marketState` поля, поддерживающего `MarketState` свойстве, назначенном `volatile`:</span><span class="sxs-lookup"><span data-stu-id="10abe-373">To make sure this code is thread-safe, the `_marketState` field that backs the `MarketState` property designated `volatile`:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

<span data-ttu-id="10abe-374">`BroadcastMarketStateChange` И `BroadcastMarketReset` методы похожи на BroadcastStockPrice метод, который вы уже видели, за исключением того, они вызывают различные методы, определенные на стороне клиента:</span><span class="sxs-lookup"><span data-stu-id="10abe-374">The `BroadcastMarketStateChange` and `BroadcastMarketReset` methods are similar to the BroadcastStockPrice method that you already saw, except they call different methods defined at the client:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a><span data-ttu-id="10abe-375">Дополнительные функции на клиенте, который сервер может обратиться</span><span class="sxs-lookup"><span data-stu-id="10abe-375">Additional functions on the client that the server can call</span></span>

<span data-ttu-id="10abe-376">`updateStockPrice` Функция теперь обрабатывает в таблице и отображение биржевых котировок и использует `jQuery.Color` начинает мигать, красного и зеленого цвета.</span><span class="sxs-lookup"><span data-stu-id="10abe-376">The `updateStockPrice` function now handles both the table and the ticker display, and it uses `jQuery.Color` to flash red and green colors.</span></span>

<span data-ttu-id="10abe-377">Новые функции в *SignalR.StockTicker.js* Включение и отключение кнопок, исходя из состояния рынка.</span><span class="sxs-lookup"><span data-stu-id="10abe-377">New functions in *SignalR.StockTicker.js* enable and disable the buttons based on market state.</span></span> <span data-ttu-id="10abe-378">Они также остановить или запустить **Live биржевых котировок акций** горизонтальная прокрутка.</span><span class="sxs-lookup"><span data-stu-id="10abe-378">They also stop or start the **Live Stock Ticker** horizontal scrolling.</span></span> <span data-ttu-id="10abe-379">Так как многие функции добавляются к `ticker.client`, приложение использует [jQuery расширить функции](http://api.jquery.com/jQuery.extend/) их добавления.</span><span class="sxs-lookup"><span data-stu-id="10abe-379">Since many functions are being added to `ticker.client`, the app uses the [jQuery extend function](http://api.jquery.com/jQuery.extend/) to add them.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a><span data-ttu-id="10abe-380">Программа установки дополнительного клиента после подключения к</span><span class="sxs-lookup"><span data-stu-id="10abe-380">Additional client setup after establishing the connection</span></span>

<span data-ttu-id="10abe-381">После того как клиент устанавливает соединение, он имеет некоторые проделать дополнительную работу:</span><span class="sxs-lookup"><span data-stu-id="10abe-381">After the client establishes the connection, it has some additional work to do:</span></span>

* <span data-ttu-id="10abe-382">Проверить, является ли рынке открытым или закрытым для вызова соответствующего `marketOpened` или `marketClosed` функции.</span><span class="sxs-lookup"><span data-stu-id="10abe-382">Find out if the market is open or closed to call the appropriate `marketOpened` or `marketClosed` function.</span></span>

* <span data-ttu-id="10abe-383">Подключите вызовы методов сервера кнопок.</span><span class="sxs-lookup"><span data-stu-id="10abe-383">Attach the server method calls to the buttons.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

<span data-ttu-id="10abe-384">Методы сервера не отправлены кнопки до, после того как приложение устанавливает соединение.</span><span class="sxs-lookup"><span data-stu-id="10abe-384">The server methods aren't wired up to the buttons until after the app establishes the connection.</span></span> <span data-ttu-id="10abe-385">Это так, что код не может вызывать методы сервера, прежде чем они доступны.</span><span class="sxs-lookup"><span data-stu-id="10abe-385">It's so the code can't call the server methods before they're available.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="10abe-386">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="10abe-386">Additional resources</span></span>

<span data-ttu-id="10abe-387">В этом руководстве вы узнали, как запрограммировать приложение SignalR, которое передает сообщения с сервера все подключенные клиенты.</span><span class="sxs-lookup"><span data-stu-id="10abe-387">In this tutorial you've learned how to program a SignalR application that broadcasts messages from the server to all connected clients.</span></span> <span data-ttu-id="10abe-388">Теперь можно выполнить рассылку сообщения на периодической основе и в ответ на уведомления из любого клиента.</span><span class="sxs-lookup"><span data-stu-id="10abe-388">Now you can broadcast messages on a periodic basis and in response to notifications from any client.</span></span> <span data-ttu-id="10abe-389">Концепция многопоточных одноэлементный экземпляр можно использовать для поддержания состояния сервера в многопользовательских online game сценариях.</span><span class="sxs-lookup"><span data-stu-id="10abe-389">You can use the concept of multi-threaded singleton instance to maintain server state in multi-player online game scenarios.</span></span> <span data-ttu-id="10abe-390">Например, см. в разделе [ShootR игры на основании SignalR](https://github.com/NTaylorMullen/ShootR).</span><span class="sxs-lookup"><span data-stu-id="10abe-390">For an example, see [the ShootR game based on SignalR](https://github.com/NTaylorMullen/ShootR).</span></span>

<span data-ttu-id="10abe-391">Учебники, которые показывают связи peer-to-peer сценариев, см. в разделе [начало работы с SignalR](introduction-to-signalr.md) и [обновления в реальном времени с SignalR](tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="10abe-391">For tutorials that show peer-to-peer communication scenarios, see [Getting Started with SignalR](introduction-to-signalr.md) and [Real-Time Updating with SignalR](tutorial-high-frequency-realtime-with-signalr.md).</span></span>

<span data-ttu-id="10abe-392">Дополнительные сведения о SignalR см. следующие ресурсы:</span><span class="sxs-lookup"><span data-stu-id="10abe-392">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="10abe-393">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="10abe-393">ASP.NET SignalR</span></span>](../../index.md)
* [<span data-ttu-id="10abe-394">Проект SignalR</span><span class="sxs-lookup"><span data-stu-id="10abe-394">SignalR Project</span></span>](http://signalr.net/)
* [<span data-ttu-id="10abe-395">SignalR GitHub и примерами</span><span class="sxs-lookup"><span data-stu-id="10abe-395">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)
* [<span data-ttu-id="10abe-396">Вики-сайте SignalR</span><span class="sxs-lookup"><span data-stu-id="10abe-396">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="10abe-397">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="10abe-397">Next steps</span></span>

<span data-ttu-id="10abe-398">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="10abe-398">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="10abe-399">Проект создан</span><span class="sxs-lookup"><span data-stu-id="10abe-399">Created the project</span></span>
> * <span data-ttu-id="10abe-400">Настройте серверный код</span><span class="sxs-lookup"><span data-stu-id="10abe-400">Set up the server code</span></span>
> * <span data-ttu-id="10abe-401">Проверить код на сервере</span><span class="sxs-lookup"><span data-stu-id="10abe-401">Examined the server code</span></span>
> * <span data-ttu-id="10abe-402">Настройка кода клиента</span><span class="sxs-lookup"><span data-stu-id="10abe-402">Set up the client code</span></span>
> * <span data-ttu-id="10abe-403">Проверить код клиента</span><span class="sxs-lookup"><span data-stu-id="10abe-403">Examined the client code</span></span>
> * <span data-ttu-id="10abe-404">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="10abe-404">Tested the application</span></span>
> * <span data-ttu-id="10abe-405">Ведение журнала включено</span><span class="sxs-lookup"><span data-stu-id="10abe-405">Enabled logging</span></span>

<span data-ttu-id="10abe-406">Перейдите к следующей статье, чтобы научиться создавать в режиме реального времени веб-приложения, использующего ASP.NET SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="10abe-406">Advance to the next article to learn how to create a real-time web application that uses ASP.NET SignalR 2.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="10abe-407">Создание в режиме реального времени веб-приложения с помощью SignalR</span><span class="sxs-lookup"><span data-stu-id="10abe-407">Create real-time web app with SignalR</span></span>](real-time-web-applications-with-signalr.md)