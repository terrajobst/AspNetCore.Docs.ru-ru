---
uid: signalr/overview/older-versions/dependency-injection
title: Внедрение зависимостей в SignalR 1.x | Документы Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/15/2013
ms.topic: article
ms.assetid: eaa206c4-edb3-487e-8fcb-54a3261fed36
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 6fd155adc9a0aa71b66db7a51062a51fb0c1feca
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
ms.locfileid: "26505493"
---
<a name="dependency-injection-in-signalr-1x"></a><span data-ttu-id="f70d8-102">Внедрение зависимостей в SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="f70d8-102">Dependency Injection in SignalR 1.x</span></span>
====================
<span data-ttu-id="f70d8-103">по [Mike Wasson](https://github.com/MikeWasson), [Патрик Флетчера](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="f70d8-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="f70d8-104">Внедрение зависимостей способ для удаления жестких зависимостей между объектами, упрощая процесс для замены зависимости объекта, либо для тестирования (с помощью макетов объектов) или для изменения поведения во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="f70d8-104">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="f70d8-105">Этого учебника показано, как выполнить внедрения зависимостей для концентраторов SignalR.</span><span class="sxs-lookup"><span data-stu-id="f70d8-105">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="f70d8-106">Также показано, как использовать контейнеры IoC с SignalR.</span><span class="sxs-lookup"><span data-stu-id="f70d8-106">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="f70d8-107">Контейнер IoC — это стандартная структура для внедрения зависимости.</span><span class="sxs-lookup"><span data-stu-id="f70d8-107">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="f70d8-108">Что такое внедрения зависимости</span><span class="sxs-lookup"><span data-stu-id="f70d8-108">What is Dependency Injection?</span></span>

<span data-ttu-id="f70d8-109">Этот раздел можно пропустите, если вы уже знакомы с внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="f70d8-109">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="f70d8-110">*Внедрение зависимостей* (DI) — это шаблон, где объекты, не отвечают за создание собственных зависимостей.</span><span class="sxs-lookup"><span data-stu-id="f70d8-110">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="f70d8-111">Ниже приведен простой пример, чтобы стимулировать DI.</span><span class="sxs-lookup"><span data-stu-id="f70d8-111">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="f70d8-112">Предположим, что имеется объект, который требуется для записи сообщений.</span><span class="sxs-lookup"><span data-stu-id="f70d8-112">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="f70d8-113">Можно определить интерфейс ведения журнала:</span><span class="sxs-lookup"><span data-stu-id="f70d8-113">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="f70d8-114">Объект, можно создать `ILogger` сообщений в журнале:</span><span class="sxs-lookup"><span data-stu-id="f70d8-114">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="f70d8-115">Это работает, но это не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="f70d8-115">This works, but it's not the best design.</span></span> <span data-ttu-id="f70d8-116">Если вы хотите заменить `FileLogger` с другим `ILogger` реализации, необходимо будет изменить `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="f70d8-116">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="f70d8-117">Supposing, много других объекты, используйте `FileLogger`, необходимо будет изменить их все.</span><span class="sxs-lookup"><span data-stu-id="f70d8-117">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="f70d8-118">Или если необходимо внести `FileLogger` является одноэлементным множеством, вам также потребуется внести изменения в приложении.</span><span class="sxs-lookup"><span data-stu-id="f70d8-118">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="f70d8-119">Лучшим подходом является «вставка» `ILogger` в объекте — например, с помощью аргумента конструктора:</span><span class="sxs-lookup"><span data-stu-id="f70d8-119">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="f70d8-120">Теперь объект не является обязательной при выборе которой `ILogger` для использования.</span><span class="sxs-lookup"><span data-stu-id="f70d8-120">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="f70d8-121">Вы можете swich `ILogger` реализации без изменения объектов, зависящих от нее.</span><span class="sxs-lookup"><span data-stu-id="f70d8-121">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="f70d8-122">Эта модель называется [внедрение конструктора](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="f70d8-122">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="f70d8-123">Другой подход заключается внедрения setter задаются зависимостей через свойство или метод задания.</span><span class="sxs-lookup"><span data-stu-id="f70d8-123">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="f70d8-124">Внедрение зависимостей простой в SignalR</span><span class="sxs-lookup"><span data-stu-id="f70d8-124">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="f70d8-125">Рассмотрим приложение чата из учебника [Приступая к работе с SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="f70d8-125">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="f70d8-126">Вот классу hub из этого приложения.</span><span class="sxs-lookup"><span data-stu-id="f70d8-126">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="f70d8-127">Предположим, что вы хотите хранить сообщения чата на сервере перед их отправкой.</span><span class="sxs-lookup"><span data-stu-id="f70d8-127">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="f70d8-128">Может Определите интерфейс, который абстрагирует эту функцию и использовать для вставки в интерфейс DI `ChatHub` класса.</span><span class="sxs-lookup"><span data-stu-id="f70d8-128">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="f70d8-129">Единственная проблема состоит в том, что приложение SignalR не создаются непосредственно концентраторов; SignalR создает их.</span><span class="sxs-lookup"><span data-stu-id="f70d8-129">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="f70d8-130">По умолчанию SignalR ожидает концентратора класса должен быть конструктор без параметров.</span><span class="sxs-lookup"><span data-stu-id="f70d8-130">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="f70d8-131">Тем не менее вы можно легко зарегистрировать функцию для создания экземпляров концентратора и использовать эту функцию для выполнения DI.</span><span class="sxs-lookup"><span data-stu-id="f70d8-131">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="f70d8-132">Зарегистрируйте эту функцию, вызвав **GlobalHost.DependencyResolver.Register**.</span><span class="sxs-lookup"><span data-stu-id="f70d8-132">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="f70d8-133">Теперь SignalR будет вызывать это анонимная функция, каждый раз, когда он должен создать `ChatHub` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="f70d8-133">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="f70d8-134">Контейнеры IoC</span><span class="sxs-lookup"><span data-stu-id="f70d8-134">IoC Containers</span></span>

<span data-ttu-id="f70d8-135">Предыдущий код хорошо работает для простых случаях.</span><span class="sxs-lookup"><span data-stu-id="f70d8-135">The previous code is fine for simple cases.</span></span> <span data-ttu-id="f70d8-136">Но по-прежнему должны были написать это:</span><span class="sxs-lookup"><span data-stu-id="f70d8-136">But you still had to write this:</span></span>

[!code-css[Main](dependency-injection/samples/sample8.css)]

<span data-ttu-id="f70d8-137">В сложных приложениях с много зависимостей может потребоваться запись большого объема кода это «привязки».</span><span class="sxs-lookup"><span data-stu-id="f70d8-137">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="f70d8-138">Этот код может быть трудно поддерживать, особенно в том случае, если зависимости являются вложенными.</span><span class="sxs-lookup"><span data-stu-id="f70d8-138">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="f70d8-139">Также будет сложно модульного теста.</span><span class="sxs-lookup"><span data-stu-id="f70d8-139">It is also hard to unit test.</span></span>

<span data-ttu-id="f70d8-140">Единственное решение — использовать контейнер IoC.</span><span class="sxs-lookup"><span data-stu-id="f70d8-140">One solution is to use an IoC container.</span></span> <span data-ttu-id="f70d8-141">Контейнер IoC — это программный компонент, который отвечает за управление зависимостями. Зарегистрировать типы с контейнером и затем использовать для создания объектов контейнера.</span><span class="sxs-lookup"><span data-stu-id="f70d8-141">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="f70d8-142">Контейнер автоматически решает, отношений зависимостей.</span><span class="sxs-lookup"><span data-stu-id="f70d8-142">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="f70d8-143">Многие контейнеры IoC также позволяют управлять вещей, как время существования объектов и области.</span><span class="sxs-lookup"><span data-stu-id="f70d8-143">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="f70d8-144">«IoC» означает «инверсии элемента управления», который является общий шаблон, когда платформа вызывает в код приложения.</span><span class="sxs-lookup"><span data-stu-id="f70d8-144">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="f70d8-145">Контейнер IoC создает объекты, который «инвертирует «обычного потока управления.</span><span class="sxs-lookup"><span data-stu-id="f70d8-145">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="f70d8-146">Использование контейнеров IoC в SignalR</span><span class="sxs-lookup"><span data-stu-id="f70d8-146">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="f70d8-147">Приложения разговора, возможно, слишком простой для использования преимуществ контейнер IoC.</span><span class="sxs-lookup"><span data-stu-id="f70d8-147">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="f70d8-148">Вместо этого, давайте взглянем на [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) образца.</span><span class="sxs-lookup"><span data-stu-id="f70d8-148">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="f70d8-149">Образец StockTicker определяет два основных класса:</span><span class="sxs-lookup"><span data-stu-id="f70d8-149">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="f70d8-150">`StockTickerHub`: Концентратора класс, который управляет клиентских подключений.</span><span class="sxs-lookup"><span data-stu-id="f70d8-150">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="f70d8-151">`StockTicker`: Одноэлементным. он содержит акций и периодически обновлять их.</span><span class="sxs-lookup"><span data-stu-id="f70d8-151">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="f70d8-152">`StockTickerHub`хранит ссылку на `StockTicker` одноэлементным, хотя `StockTicker` хранит ссылку на **IHubConnectionContext** для `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="f70d8-152">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="f70d8-153">Использует этот интерфейс для взаимодействия с `StockTickerHub` экземпляров.</span><span class="sxs-lookup"><span data-stu-id="f70d8-153">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="f70d8-154">(Дополнительные сведения см. в разделе [Server широковещательных пакетов с помощью ASP.NET SignalR](index.md).)</span><span class="sxs-lookup"><span data-stu-id="f70d8-154">(For more information, see [Server Broadcast with ASP.NET SignalR](index.md).)</span></span>

<span data-ttu-id="f70d8-155">Можно использовать контейнер IoC для немного untangle эти зависимости.</span><span class="sxs-lookup"><span data-stu-id="f70d8-155">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="f70d8-156">Во-первых давайте упростить `StockTickerHub` и `StockTicker` классы.</span><span class="sxs-lookup"><span data-stu-id="f70d8-156">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="f70d8-157">В следующем коде я закомментированы частей нам не нужен.</span><span class="sxs-lookup"><span data-stu-id="f70d8-157">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="f70d8-158">Удалите конструктор без параметров из `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="f70d8-158">Remove the parameterless constructor from `StockTicker`.</span></span> <span data-ttu-id="f70d8-159">Вместо этого мы всегда будем использовать DI для создания концентратора.</span><span class="sxs-lookup"><span data-stu-id="f70d8-159">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="f70d8-160">Для StockTicker удалите одноэлементный экземпляр.</span><span class="sxs-lookup"><span data-stu-id="f70d8-160">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="f70d8-161">Более поздней версии мы будем использовать для управления временем существования StockTicker контейнер IoC.</span><span class="sxs-lookup"><span data-stu-id="f70d8-161">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="f70d8-162">Кроме того сделайте открытый конструктор.</span><span class="sxs-lookup"><span data-stu-id="f70d8-162">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="f70d8-163">Далее мы можем выполнить рефакторинг кода путем создания интерфейса для `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="f70d8-163">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="f70d8-164">Мы будем использовать этот интерфейс для отделения `StockTickerHub` из `StockTicker` класса.</span><span class="sxs-lookup"><span data-stu-id="f70d8-164">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="f70d8-165">Visual Studio делает этот тип рефакторинга просто.</span><span class="sxs-lookup"><span data-stu-id="f70d8-165">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="f70d8-166">Откройте файл StockTicker.cs, щелкните правой кнопкой мыши `StockTicker` объявления класса и выберите **рефакторинг** ... **Извлечение интерфейса**.</span><span class="sxs-lookup"><span data-stu-id="f70d8-166">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="f70d8-167">В **извлечение интерфейса** диалоговое окно, нажмите кнопку **выделить все**.</span><span class="sxs-lookup"><span data-stu-id="f70d8-167">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="f70d8-168">Оставьте другие значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f70d8-168">Leave the other defaults.</span></span> <span data-ttu-id="f70d8-169">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="f70d8-169">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="f70d8-170">Visual Studio создает новый интерфейс с именем `IStockTicker`, а также изменяет `StockTicker` для наследования от `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="f70d8-170">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="f70d8-171">Откройте файл IStockTicker.cs и изменить интерфейс для **открытый**.</span><span class="sxs-lookup"><span data-stu-id="f70d8-171">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="f70d8-172">В `StockTickerHub` класса, измените два экземпляра `StockTicker` для `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="f70d8-172">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="f70d8-173">Создание `IStockTicker` интерфейса не является обязательным, но нужно показать, как DI может помочь сократить взаимозависимости между компонентами приложения.</span><span class="sxs-lookup"><span data-stu-id="f70d8-173">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="f70d8-174">Добавьте библиотеку Ninject</span><span class="sxs-lookup"><span data-stu-id="f70d8-174">Add the Ninject Library</span></span>

<span data-ttu-id="f70d8-175">Существует много контейнеры IoC открытым исходным кодом для .NET.</span><span class="sxs-lookup"><span data-stu-id="f70d8-175">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="f70d8-176">В этом учебнике, я буду использовать [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="f70d8-176">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="f70d8-177">(Другие популярные библиотеки включают в себя [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), и [StructureMap ](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="f70d8-177">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="f70d8-178">Используйте диспетчер пакетов NuGet для установки [Ninject библиотеки](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="f70d8-178">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="f70d8-179">В Visual Studio из **средства** меню выберите **диспетчер пакетов библиотеки** | **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="f70d8-179">In Visual Studio, from the **Tools** menu select **Library Package Manager** | **Package Manager Console**.</span></span> <span data-ttu-id="f70d8-180">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="f70d8-180">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="f70d8-181">Замените Сопоставитель зависимостей SignalR</span><span class="sxs-lookup"><span data-stu-id="f70d8-181">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="f70d8-182">Чтобы использовать Ninject в SignalR, создайте класс, производный от **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="f70d8-182">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="f70d8-183">Этот класс переопределяет **GetService** и **GetServices** методы **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="f70d8-183">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="f70d8-184">SignalR вызывает эти методы для создания различных объектов во время выполнения, включая экземпляры центра, а также различные службы, используются внутренними механизмами SignalR.</span><span class="sxs-lookup"><span data-stu-id="f70d8-184">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="f70d8-185">**GetService** метод создает один экземпляр типа.</span><span class="sxs-lookup"><span data-stu-id="f70d8-185">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="f70d8-186">Переопределите этот метод для вызова ядра Ninject **TryGet** метод.</span><span class="sxs-lookup"><span data-stu-id="f70d8-186">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="f70d8-187">Если этот метод возвращает значение null, вернутся к Сопоставитель по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f70d8-187">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="f70d8-188">**GetServices** метод создает коллекцию объектов определенного типа.</span><span class="sxs-lookup"><span data-stu-id="f70d8-188">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="f70d8-189">Переопределите этот метод, чтобы объединить результаты из Ninject с результатами из Сопоставитель по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f70d8-189">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="f70d8-190">Настройте привязки Ninject</span><span class="sxs-lookup"><span data-stu-id="f70d8-190">Configure Ninject Bindings</span></span>

<span data-ttu-id="f70d8-191">Теперь мы будем использовать Ninject для объявления типа привязки.</span><span class="sxs-lookup"><span data-stu-id="f70d8-191">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="f70d8-192">Откройте файл RegisterHubs.cs.</span><span class="sxs-lookup"><span data-stu-id="f70d8-192">Open the file RegisterHubs.cs.</span></span> <span data-ttu-id="f70d8-193">В `RegisterHubs.Start` метод, создать контейнер Ninject, который вызывает Ninject *ядра*.</span><span class="sxs-lookup"><span data-stu-id="f70d8-193">In the `RegisterHubs.Start` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="f70d8-194">Создайте экземпляр распознавателя нашей пользовательскую зависимость:</span><span class="sxs-lookup"><span data-stu-id="f70d8-194">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="f70d8-195">Создайте привязку для `IStockTicker` следующим образом:</span><span class="sxs-lookup"><span data-stu-id="f70d8-195">Create a binding for `IStockTicker` as follows:</span></span>

[!code-html[Main](dependency-injection/samples/sample17.html)]

<span data-ttu-id="f70d8-196">Этот код означает следующее.</span><span class="sxs-lookup"><span data-stu-id="f70d8-196">This code is saying two things.</span></span> <span data-ttu-id="f70d8-197">Во-первых, каждый раз, когда приложение должно `IStockTicker`, ядро должен создать экземпляр `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="f70d8-197">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="f70d8-198">Во-вторых, `StockTicker` класс должен быть созданный как одноэлементный объект.</span><span class="sxs-lookup"><span data-stu-id="f70d8-198">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="f70d8-199">Ninject создать один экземпляр объекта и возвращают один и тот же экземпляр для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="f70d8-199">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="f70d8-200">Создайте привязку для **IHubConnectionContext** следующим образом:</span><span class="sxs-lookup"><span data-stu-id="f70d8-200">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="f70d8-201">Этот код creatres анонимную функцию, которая возвращает **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="f70d8-201">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="f70d8-202">**WhenInjectedInto** метод указывает Ninject, чтобы использовать эту функцию только при создании `IStockTicker` экземпляров.</span><span class="sxs-lookup"><span data-stu-id="f70d8-202">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="f70d8-203">Причина заключается в том, что создает SignalR **IHubConnectionContext** экземпляров внутренне, и мы не хотим переопределить как SignalR создает их.</span><span class="sxs-lookup"><span data-stu-id="f70d8-203">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="f70d8-204">Эта функция применяется только к нашей `StockTicker` класса.</span><span class="sxs-lookup"><span data-stu-id="f70d8-204">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="f70d8-205">Передайте Сопоставитель зависимостей в **MapHubs** метод:</span><span class="sxs-lookup"><span data-stu-id="f70d8-205">Pass the dependency resolver into the **MapHubs** method:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="f70d8-206">Теперь использует сопоставителя, указанного в SignalR **MapHubs**, вместо Сопоставитель по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f70d8-206">Now SignalR will use the resolver specified in **MapHubs**, instead of the default resolver.</span></span>

<span data-ttu-id="f70d8-207">Ниже приведен полный код для `RegisterHubs.Start`.</span><span class="sxs-lookup"><span data-stu-id="f70d8-207">Here is the complete code listing for `RegisterHubs.Start`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="f70d8-208">Чтобы запустить приложение StockTicker в Visual Studio, нажмите клавишу F5.</span><span class="sxs-lookup"><span data-stu-id="f70d8-208">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="f70d8-209">В окне браузера перейдите к `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="f70d8-209">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="f70d8-210">Приложение имеет точно ту же функциональность, что перед.</span><span class="sxs-lookup"><span data-stu-id="f70d8-210">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="f70d8-211">(Описание см. в разделе [Server широковещательных пакетов с помощью ASP.NET SignalR](index.md).) Мы не было изменено поведение. только что внесли код удобнее для тестирования, обслуживания и развития.</span><span class="sxs-lookup"><span data-stu-id="f70d8-211">(For a description, see [Server Broadcast with ASP.NET SignalR](index.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
