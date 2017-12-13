---
uid: signalr/overview/advanced/dependency-injection
title: "Внедрение зависимостей в SignalR | Документы Microsoft"
author: MikeWasson
description: "Версии программного обеспечения используется в этом разделе Visual Studio 2013 .NET 4.5 SignalR предыдущие версии версии 2 в этом разделе сведения о предыдущих версиях..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 3732b5d0ea6de841a6c402bfd5ef4dfb7b7a9162
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="dependency-injection-in-signalr"></a><span data-ttu-id="00352-103">Внедрение зависимостей в SignalR</span><span class="sxs-lookup"><span data-stu-id="00352-103">Dependency Injection in SignalR</span></span>
====================
<span data-ttu-id="00352-104">по [Mike Wasson](https://github.com/MikeWasson), [Патрик Флетчера](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="00352-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="00352-105">Версии программного обеспечения, используемого в этом разделе</span><span class="sxs-lookup"><span data-stu-id="00352-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="00352-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="00352-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="00352-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="00352-107">.NET 4.5</span></span>
> - <span data-ttu-id="00352-108">SignalR версии 2</span><span class="sxs-lookup"><span data-stu-id="00352-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="00352-109">Предыдущие версии этого раздела</span><span class="sxs-lookup"><span data-stu-id="00352-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="00352-110">Сведения о более ранних версий SignalR см. в разделе [более ранних версий SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="00352-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="00352-111">Вопросы и комментарии</span><span class="sxs-lookup"><span data-stu-id="00352-111">Questions and comments</span></span>
> 
> <span data-ttu-id="00352-112">Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить в комментарии в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="00352-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="00352-113">Если у вас есть вопросы, которые не связаны непосредственно для работы с учебником, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="00352-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="00352-114">Внедрение зависимостей способ для удаления жестких зависимостей между объектами, упрощая процесс для замены зависимости объекта, либо для тестирования (с помощью макетов объектов) или для изменения поведения во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="00352-114">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="00352-115">Этого учебника показано, как выполнить внедрения зависимостей для концентраторов SignalR.</span><span class="sxs-lookup"><span data-stu-id="00352-115">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="00352-116">Также показано, как использовать контейнеры IoC с SignalR.</span><span class="sxs-lookup"><span data-stu-id="00352-116">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="00352-117">Контейнер IoC — это стандартная структура для внедрения зависимости.</span><span class="sxs-lookup"><span data-stu-id="00352-117">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="00352-118">Что такое внедрения зависимости</span><span class="sxs-lookup"><span data-stu-id="00352-118">What is Dependency Injection?</span></span>

<span data-ttu-id="00352-119">Этот раздел можно пропустите, если вы уже знакомы с внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="00352-119">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="00352-120">*Внедрение зависимостей* (DI) — это шаблон, где объекты, не отвечают за создание собственных зависимостей.</span><span class="sxs-lookup"><span data-stu-id="00352-120">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="00352-121">Ниже приведен простой пример, чтобы стимулировать DI.</span><span class="sxs-lookup"><span data-stu-id="00352-121">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="00352-122">Предположим, что имеется объект, который требуется для записи сообщений.</span><span class="sxs-lookup"><span data-stu-id="00352-122">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="00352-123">Можно определить интерфейс ведения журнала:</span><span class="sxs-lookup"><span data-stu-id="00352-123">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="00352-124">Объект, можно создать `ILogger` сообщений в журнале:</span><span class="sxs-lookup"><span data-stu-id="00352-124">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="00352-125">Это работает, но это не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="00352-125">This works, but it's not the best design.</span></span> <span data-ttu-id="00352-126">Если вы хотите заменить `FileLogger` с другим `ILogger` реализации, необходимо будет изменить `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="00352-126">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="00352-127">Supposing, много других объекты, используйте `FileLogger`, необходимо будет изменить их все.</span><span class="sxs-lookup"><span data-stu-id="00352-127">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="00352-128">Или если необходимо внести `FileLogger` является одноэлементным множеством, вам также потребуется внести изменения в приложении.</span><span class="sxs-lookup"><span data-stu-id="00352-128">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="00352-129">Лучшим подходом является «вставка» `ILogger` в объекте — например, с помощью аргумента конструктора:</span><span class="sxs-lookup"><span data-stu-id="00352-129">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="00352-130">Теперь объект не является обязательной при выборе которой `ILogger` для использования.</span><span class="sxs-lookup"><span data-stu-id="00352-130">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="00352-131">Вы можете swich `ILogger` реализации без изменения объектов, зависящих от нее.</span><span class="sxs-lookup"><span data-stu-id="00352-131">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="00352-132">Эта модель называется [внедрение конструктора](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="00352-132">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="00352-133">Другой подход заключается внедрения setter задаются зависимостей через свойство или метод задания.</span><span class="sxs-lookup"><span data-stu-id="00352-133">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="00352-134">Внедрение зависимостей простой в SignalR</span><span class="sxs-lookup"><span data-stu-id="00352-134">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="00352-135">Рассмотрим приложение чата из учебника [Приступая к работе с SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="00352-135">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="00352-136">Вот классу hub из этого приложения.</span><span class="sxs-lookup"><span data-stu-id="00352-136">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="00352-137">Предположим, что вы хотите хранить сообщения чата на сервере перед их отправкой.</span><span class="sxs-lookup"><span data-stu-id="00352-137">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="00352-138">Может Определите интерфейс, который абстрагирует эту функцию и использовать для вставки в интерфейс DI `ChatHub` класса.</span><span class="sxs-lookup"><span data-stu-id="00352-138">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="00352-139">Единственная проблема состоит в том, что приложение SignalR не создаются непосредственно концентраторов; SignalR создает их.</span><span class="sxs-lookup"><span data-stu-id="00352-139">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="00352-140">По умолчанию SignalR ожидает концентратора класса должен быть конструктор без параметров.</span><span class="sxs-lookup"><span data-stu-id="00352-140">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="00352-141">Тем не менее вы можно легко зарегистрировать функцию для создания экземпляров концентратора и использовать эту функцию для выполнения DI.</span><span class="sxs-lookup"><span data-stu-id="00352-141">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="00352-142">Зарегистрируйте эту функцию, вызвав **GlobalHost.DependencyResolver.Register**.</span><span class="sxs-lookup"><span data-stu-id="00352-142">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="00352-143">Теперь SignalR будет вызывать это анонимная функция, каждый раз, когда он должен создать `ChatHub` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="00352-143">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="00352-144">Контейнеры IoC</span><span class="sxs-lookup"><span data-stu-id="00352-144">IoC Containers</span></span>

<span data-ttu-id="00352-145">Предыдущий код хорошо работает для простых случаях.</span><span class="sxs-lookup"><span data-stu-id="00352-145">The previous code is fine for simple cases.</span></span> <span data-ttu-id="00352-146">Но по-прежнему должны были написать это:</span><span class="sxs-lookup"><span data-stu-id="00352-146">But you still had to write this:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

<span data-ttu-id="00352-147">В сложных приложениях с много зависимостей может потребоваться запись большого объема кода это «привязки».</span><span class="sxs-lookup"><span data-stu-id="00352-147">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="00352-148">Этот код может быть трудно поддерживать, особенно в том случае, если зависимости являются вложенными.</span><span class="sxs-lookup"><span data-stu-id="00352-148">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="00352-149">Также будет сложно модульного теста.</span><span class="sxs-lookup"><span data-stu-id="00352-149">It is also hard to unit test.</span></span>

<span data-ttu-id="00352-150">Единственное решение — использовать контейнер IoC.</span><span class="sxs-lookup"><span data-stu-id="00352-150">One solution is to use an IoC container.</span></span> <span data-ttu-id="00352-151">Контейнер IoC — это программный компонент, который отвечает за управление зависимостями. Зарегистрировать типы с контейнером и затем использовать для создания объектов контейнера.</span><span class="sxs-lookup"><span data-stu-id="00352-151">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="00352-152">Контейнер автоматически решает, отношений зависимостей.</span><span class="sxs-lookup"><span data-stu-id="00352-152">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="00352-153">Многие контейнеры IoC также позволяют управлять вещей, как время существования объектов и области.</span><span class="sxs-lookup"><span data-stu-id="00352-153">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="00352-154">«IoC» означает «инверсии элемента управления», который является общий шаблон, когда платформа вызывает в код приложения.</span><span class="sxs-lookup"><span data-stu-id="00352-154">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="00352-155">Контейнер IoC создает объекты, который «инвертирует «обычного потока управления.</span><span class="sxs-lookup"><span data-stu-id="00352-155">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="00352-156">Использование контейнеров IoC в SignalR</span><span class="sxs-lookup"><span data-stu-id="00352-156">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="00352-157">Приложения разговора, возможно, слишком простой для использования преимуществ контейнер IoC.</span><span class="sxs-lookup"><span data-stu-id="00352-157">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="00352-158">Вместо этого, давайте взглянем на [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) образца.</span><span class="sxs-lookup"><span data-stu-id="00352-158">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="00352-159">Образец StockTicker определяет два основных класса:</span><span class="sxs-lookup"><span data-stu-id="00352-159">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="00352-160">`StockTickerHub`: Концентратора класс, который управляет клиентских подключений.</span><span class="sxs-lookup"><span data-stu-id="00352-160">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="00352-161">`StockTicker`: Одноэлементным. он содержит акций и периодически обновлять их.</span><span class="sxs-lookup"><span data-stu-id="00352-161">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="00352-162">`StockTickerHub`хранит ссылку на `StockTicker` одноэлементным, хотя `StockTicker` хранит ссылку на **IHubConnectionContext** для `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="00352-162">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="00352-163">Использует этот интерфейс для взаимодействия с `StockTickerHub` экземпляров.</span><span class="sxs-lookup"><span data-stu-id="00352-163">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="00352-164">(Дополнительные сведения см. в разделе [Server широковещательных пакетов с помощью ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span><span class="sxs-lookup"><span data-stu-id="00352-164">(For more information, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span></span>

<span data-ttu-id="00352-165">Можно использовать контейнер IoC для немного untangle эти зависимости.</span><span class="sxs-lookup"><span data-stu-id="00352-165">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="00352-166">Во-первых давайте упростить `StockTickerHub` и `StockTicker` классы.</span><span class="sxs-lookup"><span data-stu-id="00352-166">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="00352-167">В следующем коде я закомментированы частей нам не нужен.</span><span class="sxs-lookup"><span data-stu-id="00352-167">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="00352-168">Удалите конструктор без параметров из `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="00352-168">Remove the parameterless constructor from `StockTickerHub`.</span></span> <span data-ttu-id="00352-169">Вместо этого мы всегда будем использовать DI для создания концентратора.</span><span class="sxs-lookup"><span data-stu-id="00352-169">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="00352-170">Для StockTicker удалите одноэлементный экземпляр.</span><span class="sxs-lookup"><span data-stu-id="00352-170">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="00352-171">Более поздней версии мы будем использовать для управления временем существования StockTicker контейнер IoC.</span><span class="sxs-lookup"><span data-stu-id="00352-171">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="00352-172">Кроме того сделайте открытый конструктор.</span><span class="sxs-lookup"><span data-stu-id="00352-172">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="00352-173">Далее мы можем выполнить рефакторинг кода путем создания интерфейса для `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="00352-173">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="00352-174">Мы будем использовать этот интерфейс для отделения `StockTickerHub` из `StockTicker` класса.</span><span class="sxs-lookup"><span data-stu-id="00352-174">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="00352-175">Visual Studio делает этот тип рефакторинга просто.</span><span class="sxs-lookup"><span data-stu-id="00352-175">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="00352-176">Откройте файл StockTicker.cs, щелкните правой кнопкой мыши `StockTicker` объявления класса и выберите **рефакторинг** ... **Извлечение интерфейса**.</span><span class="sxs-lookup"><span data-stu-id="00352-176">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="00352-177">В **извлечение интерфейса** диалоговое окно, нажмите кнопку **выделить все**.</span><span class="sxs-lookup"><span data-stu-id="00352-177">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="00352-178">Оставьте другие значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="00352-178">Leave the other defaults.</span></span> <span data-ttu-id="00352-179">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="00352-179">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="00352-180">Visual Studio создает новый интерфейс с именем `IStockTicker`, а также изменяет `StockTicker` для наследования от `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="00352-180">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="00352-181">Откройте файл IStockTicker.cs и изменить интерфейс для **открытый**.</span><span class="sxs-lookup"><span data-stu-id="00352-181">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="00352-182">В `StockTickerHub` класса, измените два экземпляра `StockTicker` для `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="00352-182">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="00352-183">Создание `IStockTicker` интерфейса не является обязательным, но нужно показать, как DI может помочь сократить взаимозависимости между компонентами приложения.</span><span class="sxs-lookup"><span data-stu-id="00352-183">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="00352-184">Добавьте библиотеку Ninject</span><span class="sxs-lookup"><span data-stu-id="00352-184">Add the Ninject Library</span></span>

<span data-ttu-id="00352-185">Существует много контейнеры IoC открытым исходным кодом для .NET.</span><span class="sxs-lookup"><span data-stu-id="00352-185">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="00352-186">В этом учебнике, я буду использовать [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="00352-186">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="00352-187">(Другие популярные библиотеки включают в себя [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), и [StructureMap ](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="00352-187">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="00352-188">Используйте диспетчер пакетов NuGet для установки [Ninject библиотеки](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="00352-188">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="00352-189">В Visual Studio из **средства** меню выберите **диспетчер пакетов библиотеки** | **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="00352-189">In Visual Studio, from the **Tools** menu select **Library Package Manager** | **Package Manager Console**.</span></span> <span data-ttu-id="00352-190">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="00352-190">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="00352-191">Замените Сопоставитель зависимостей SignalR</span><span class="sxs-lookup"><span data-stu-id="00352-191">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="00352-192">Чтобы использовать Ninject в SignalR, создайте класс, производный от **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="00352-192">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="00352-193">Этот класс переопределяет **GetService** и **GetServices** методы **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="00352-193">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="00352-194">SignalR вызывает эти методы для создания различных объектов во время выполнения, включая экземпляры центра, а также различные службы, используются внутренними механизмами SignalR.</span><span class="sxs-lookup"><span data-stu-id="00352-194">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="00352-195">**GetService** метод создает один экземпляр типа.</span><span class="sxs-lookup"><span data-stu-id="00352-195">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="00352-196">Переопределите этот метод для вызова ядра Ninject **TryGet** метод.</span><span class="sxs-lookup"><span data-stu-id="00352-196">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="00352-197">Если этот метод возвращает значение null, вернутся к Сопоставитель по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="00352-197">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="00352-198">**GetServices** метод создает коллекцию объектов определенного типа.</span><span class="sxs-lookup"><span data-stu-id="00352-198">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="00352-199">Переопределите этот метод, чтобы объединить результаты из Ninject с результатами из Сопоставитель по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="00352-199">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="00352-200">Настройте привязки Ninject</span><span class="sxs-lookup"><span data-stu-id="00352-200">Configure Ninject Bindings</span></span>

<span data-ttu-id="00352-201">Теперь мы будем использовать Ninject для объявления типа привязки.</span><span class="sxs-lookup"><span data-stu-id="00352-201">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="00352-202">Откройте класс приложения в файле Startup.cs (либо созданным вручную согласно инструкциям пакета `readme.txt`, или который был создан путем добавления в проект проверки подлинности).</span><span class="sxs-lookup"><span data-stu-id="00352-202">Open your application's Startup.cs class (that you either created manually as per the package instructions in `readme.txt`, or that was created by adding authentication to your project).</span></span> <span data-ttu-id="00352-203">В `Startup.Configuration` метод, создать контейнер Ninject, который вызывает Ninject *ядра*.</span><span class="sxs-lookup"><span data-stu-id="00352-203">In the `Startup.Configuration` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="00352-204">Создайте экземпляр распознавателя нашей пользовательскую зависимость:</span><span class="sxs-lookup"><span data-stu-id="00352-204">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="00352-205">Создайте привязку для `IStockTicker` следующим образом:</span><span class="sxs-lookup"><span data-stu-id="00352-205">Create a binding for `IStockTicker` as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

<span data-ttu-id="00352-206">Этот код означает следующее.</span><span class="sxs-lookup"><span data-stu-id="00352-206">This code is saying two things.</span></span> <span data-ttu-id="00352-207">Во-первых, каждый раз, когда приложение должно `IStockTicker`, ядро должен создать экземпляр `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="00352-207">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="00352-208">Во-вторых, `StockTicker` класс должен быть созданный как одноэлементный объект.</span><span class="sxs-lookup"><span data-stu-id="00352-208">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="00352-209">Ninject создать один экземпляр объекта и возвращают один и тот же экземпляр для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="00352-209">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="00352-210">Создайте привязку для **IHubConnectionContext** следующим образом:</span><span class="sxs-lookup"><span data-stu-id="00352-210">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="00352-211">Этот код creatres анонимную функцию, которая возвращает **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="00352-211">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="00352-212">**WhenInjectedInto** метод указывает Ninject, чтобы использовать эту функцию только при создании `IStockTicker` экземпляров.</span><span class="sxs-lookup"><span data-stu-id="00352-212">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="00352-213">Причина заключается в том, что создает SignalR **IHubConnectionContext** экземпляров внутренне, и мы не хотим переопределить как SignalR создает их.</span><span class="sxs-lookup"><span data-stu-id="00352-213">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="00352-214">Эта функция применяется только к нашей `StockTicker` класса.</span><span class="sxs-lookup"><span data-stu-id="00352-214">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="00352-215">Передайте Сопоставитель зависимостей в **MapSignalR** метод путем добавления конфигурацию концентратора:</span><span class="sxs-lookup"><span data-stu-id="00352-215">Pass the dependency resolver into the **MapSignalR** method by adding a hub configuration:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="00352-216">Добавьте в метод Startup.ConfigureSignalR в классе при запуске образца новый параметр:</span><span class="sxs-lookup"><span data-stu-id="00352-216">Update the Startup.ConfigureSignalR method in the sample's Startup class with the new parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="00352-217">Теперь использует сопоставителя, указанного в SignalR **MapSignalR**, вместо Сопоставитель по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="00352-217">Now SignalR will use the resolver specified in **MapSignalR**, instead of the default resolver.</span></span>

<span data-ttu-id="00352-218">Ниже приведен полный код для `Startup.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="00352-218">Here is the complete code listing for `Startup.Configuration`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

<span data-ttu-id="00352-219">Чтобы запустить приложение StockTicker в Visual Studio, нажмите клавишу F5.</span><span class="sxs-lookup"><span data-stu-id="00352-219">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="00352-220">В окне браузера перейдите к `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="00352-220">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="00352-221">Приложение имеет точно ту же функциональность, что перед.</span><span class="sxs-lookup"><span data-stu-id="00352-221">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="00352-222">(Описание см. в разделе [Server широковещательных пакетов с помощью ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) Мы не было изменено поведение. только что внесли код удобнее для тестирования, обслуживания и развития.</span><span class="sxs-lookup"><span data-stu-id="00352-222">(For a description, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
