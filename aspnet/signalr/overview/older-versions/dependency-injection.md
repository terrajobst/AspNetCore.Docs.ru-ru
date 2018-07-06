---
uid: signalr/overview/older-versions/dependency-injection
title: Внедрение зависимостей в SignalR 1.x | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 05/15/2013
ms.assetid: eaa206c4-edb3-487e-8fcb-54a3261fed36
msc.legacyurl: /signalr/overview/older-versions/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: ff96b4958652dac5109648974770abf3a9b6ae0c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802400"
---
<a name="dependency-injection-in-signalr-1x"></a><span data-ttu-id="520d1-102">Внедрение зависимостей в SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="520d1-102">Dependency Injection in SignalR 1.x</span></span>
====================
<span data-ttu-id="520d1-103">по [Майк Уоссон](https://github.com/MikeWasson), [Патрик Флетчера](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="520d1-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="520d1-104">Внедрение зависимостей является способ удаления жестких зависимостей между объектами, упрощая для замены зависимости объекта, либо для тестирования (с использованием макетов объектов) или для изменения поведения во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="520d1-104">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="520d1-105">Этом руководстве показано, как выполнить внедрения зависимостей для концентраторов SignalR.</span><span class="sxs-lookup"><span data-stu-id="520d1-105">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="520d1-106">Также показано, как использовать контейнеры IoC с SignalR.</span><span class="sxs-lookup"><span data-stu-id="520d1-106">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="520d1-107">IoC-контейнер — это стандартная структура для внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="520d1-107">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="520d1-108">Что такое внедрение зависимостей?</span><span class="sxs-lookup"><span data-stu-id="520d1-108">What is Dependency Injection?</span></span>

<span data-ttu-id="520d1-109">Этот раздел можно пропустите, если вы уже знакомы с помощью внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="520d1-109">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="520d1-110">*Внедрение зависимостей* (DI) — это шаблон, где объекты не отвечают за создание собственных зависимостей.</span><span class="sxs-lookup"><span data-stu-id="520d1-110">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="520d1-111">Ниже приведен простой пример для внедрения Зависимостей.</span><span class="sxs-lookup"><span data-stu-id="520d1-111">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="520d1-112">Предположим, что у вас есть объект, который требуется для регистрации сообщений.</span><span class="sxs-lookup"><span data-stu-id="520d1-112">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="520d1-113">Можно определить интерфейс ведения журнала:</span><span class="sxs-lookup"><span data-stu-id="520d1-113">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="520d1-114">В объекте, можно создать `ILogger` запись сообщений в журнал:</span><span class="sxs-lookup"><span data-stu-id="520d1-114">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="520d1-115">Это работает, но это не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="520d1-115">This works, but it's not the best design.</span></span> <span data-ttu-id="520d1-116">Если вы хотите заменить `FileLogger` с другим `ILogger` реализации, нужно будет только изменить `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="520d1-116">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="520d1-117">Допустим, что много других объектов используйте `FileLogger`, необходимо изменить все из них.</span><span class="sxs-lookup"><span data-stu-id="520d1-117">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="520d1-118">Или если необходимо внести `FileLogger` является одноэлементным множеством, также необходимо внести изменения в приложении.</span><span class="sxs-lookup"><span data-stu-id="520d1-118">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="520d1-119">Лучшим подходом является «внедрение» `ILogger` в объект — например, с помощью аргумента конструктора:</span><span class="sxs-lookup"><span data-stu-id="520d1-119">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="520d1-120">Теперь объект не несет ответственности за выбора `ILogger` для использования.</span><span class="sxs-lookup"><span data-stu-id="520d1-120">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="520d1-121">Вы можете swich `ILogger` реализации без изменения объекты, зависящие от нее.</span><span class="sxs-lookup"><span data-stu-id="520d1-121">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="520d1-122">Такая модель называется [внедрение через конструктор](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="520d1-122">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="520d1-123">Другой шаблон — внедрение метод задания, где можно устанавливать для зависимости через метод или свойство.</span><span class="sxs-lookup"><span data-stu-id="520d1-123">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="520d1-124">Внедрение простого зависимостей в SignalR</span><span class="sxs-lookup"><span data-stu-id="520d1-124">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="520d1-125">Рассмотрим приложение чата из учебника [начало работы с SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="520d1-125">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="520d1-126">Ниже показан класс центра из этого приложения:</span><span class="sxs-lookup"><span data-stu-id="520d1-126">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="520d1-127">Предположим, что вам нужно хранить сообщения чата на сервере перед их отправкой.</span><span class="sxs-lookup"><span data-stu-id="520d1-127">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="520d1-128">Можно определить интерфейс, который абстрагирует эту функцию и использовать внедрение Зависимостей для вставки интерфейса в `ChatHub` класса.</span><span class="sxs-lookup"><span data-stu-id="520d1-128">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="520d1-129">Единственная проблема состоит, что приложение SignalR непосредственно не создать концентраторы; SignalR создаст их самостоятельно.</span><span class="sxs-lookup"><span data-stu-id="520d1-129">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="520d1-130">По умолчанию SignalR ожидает, что для класса концентратора конструктор без параметров.</span><span class="sxs-lookup"><span data-stu-id="520d1-130">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="520d1-131">Тем не менее можно легко зарегистрировать функцию для создания экземпляров концентраторов и использовать эту функцию для выполнения внедрения Зависимостей.</span><span class="sxs-lookup"><span data-stu-id="520d1-131">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="520d1-132">Зарегистрируйте функцию с помощью метода **GlobalHost.DependencyResolver.Register**.</span><span class="sxs-lookup"><span data-stu-id="520d1-132">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="520d1-133">Теперь SignalR будет вызывать это анонимная функция, каждый раз, когда необходимо создать `ChatHub` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="520d1-133">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="520d1-134">Контейнеры IoC</span><span class="sxs-lookup"><span data-stu-id="520d1-134">IoC Containers</span></span>

<span data-ttu-id="520d1-135">Предыдущий код годится для простых случаях.</span><span class="sxs-lookup"><span data-stu-id="520d1-135">The previous code is fine for simple cases.</span></span> <span data-ttu-id="520d1-136">Но все равно требовалось написать следующее:</span><span class="sxs-lookup"><span data-stu-id="520d1-136">But you still had to write this:</span></span>

[!code-css[Main](dependency-injection/samples/sample8.css)]

<span data-ttu-id="520d1-137">В сложных приложениях с много зависимостей может потребоваться написать немало этот код «подключения».</span><span class="sxs-lookup"><span data-stu-id="520d1-137">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="520d1-138">Этот код может быть трудно поддерживать, особенно в том случае, если зависимости являются вложенными.</span><span class="sxs-lookup"><span data-stu-id="520d1-138">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="520d1-139">Это также трудно модульного теста.</span><span class="sxs-lookup"><span data-stu-id="520d1-139">It is also hard to unit test.</span></span>

<span data-ttu-id="520d1-140">Одним из решений является использование IoC-контейнер.</span><span class="sxs-lookup"><span data-stu-id="520d1-140">One solution is to use an IoC container.</span></span> <span data-ttu-id="520d1-141">IoC-контейнер — это программный компонент, который отвечает за управление зависимостями. Зарегистрируйте типов в контейнере и затем использовать для создания объектов контейнера.</span><span class="sxs-lookup"><span data-stu-id="520d1-141">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="520d1-142">Контейнер автоматически определяет отношение зависимость.</span><span class="sxs-lookup"><span data-stu-id="520d1-142">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="520d1-143">Кроме того, множество контейнеров инверсии управления позволяют управлять времени существования объектов и области.</span><span class="sxs-lookup"><span data-stu-id="520d1-143">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="520d1-144">«IoC» означает «инверсии элемента управления», который является общий шаблон, когда платформа вызывает в код приложения.</span><span class="sxs-lookup"><span data-stu-id="520d1-144">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="520d1-145">Контейнер IoC создает объектов, который «инвертирует» обычного потока управления.</span><span class="sxs-lookup"><span data-stu-id="520d1-145">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="520d1-146">Использование контейнеров IoC в SignalR</span><span class="sxs-lookup"><span data-stu-id="520d1-146">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="520d1-147">Приложения чата, вероятно, слишком простой использовать преимущества контейнер IoC.</span><span class="sxs-lookup"><span data-stu-id="520d1-147">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="520d1-148">Вместо этого давайте взглянем на [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) образца.</span><span class="sxs-lookup"><span data-stu-id="520d1-148">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="520d1-149">Образец StockTicker определяет два основных класса:</span><span class="sxs-lookup"><span data-stu-id="520d1-149">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="520d1-150">`StockTickerHub`: Класс концентратора, который управляет клиентских подключений.</span><span class="sxs-lookup"><span data-stu-id="520d1-150">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="520d1-151">`StockTicker`: Одноэлементным. он содержит цены акций и периодически обновлять их.</span><span class="sxs-lookup"><span data-stu-id="520d1-151">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="520d1-152">`StockTickerHub` хранит ссылку на `StockTicker` одноэлементным множеством, хотя `StockTicker` хранит ссылку на **IHubConnectionContext** для `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="520d1-152">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="520d1-153">Использует этот интерфейс для взаимодействия с `StockTickerHub` экземпляров.</span><span class="sxs-lookup"><span data-stu-id="520d1-153">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="520d1-154">(Дополнительные сведения см. в разделе [передача сообщений с сервера с помощью SignalR](index.md).)</span><span class="sxs-lookup"><span data-stu-id="520d1-154">(For more information, see [Server Broadcast with ASP.NET SignalR](index.md).)</span></span>

<span data-ttu-id="520d1-155">Можно использовать контейнер IoC для немного клубка эти зависимости.</span><span class="sxs-lookup"><span data-stu-id="520d1-155">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="520d1-156">Во-первых давайте упростим `StockTickerHub` и `StockTicker` классы.</span><span class="sxs-lookup"><span data-stu-id="520d1-156">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="520d1-157">В следующем коде я закомментирован частей, нам не нужен.</span><span class="sxs-lookup"><span data-stu-id="520d1-157">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="520d1-158">Удалите конструктор без параметров из `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="520d1-158">Remove the parameterless constructor from `StockTicker`.</span></span> <span data-ttu-id="520d1-159">Вместо этого всегда используется DI создайте концентратор.</span><span class="sxs-lookup"><span data-stu-id="520d1-159">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="520d1-160">Для StockTicker удалите одноэлементный экземпляр.</span><span class="sxs-lookup"><span data-stu-id="520d1-160">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="520d1-161">Позже мы будем использовать контейнер IoC, управление временем существования StockTicker.</span><span class="sxs-lookup"><span data-stu-id="520d1-161">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="520d1-162">Кроме того не открытый конструктор.</span><span class="sxs-lookup"><span data-stu-id="520d1-162">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="520d1-163">Далее мы можем выполнить рефакторинг кода путем создания интерфейса для `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="520d1-163">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="520d1-164">Мы будем использовать этот интерфейс для отделения `StockTickerHub` из `StockTicker` класса.</span><span class="sxs-lookup"><span data-stu-id="520d1-164">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="520d1-165">Visual Studio делает этот вид рефакторинга просто.</span><span class="sxs-lookup"><span data-stu-id="520d1-165">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="520d1-166">Откройте файл StockTicker.cs, щелкните правой кнопкой мыши `StockTicker` объявление класса и выберите **рефакторинг** ... **Извлечение интерфейса**.</span><span class="sxs-lookup"><span data-stu-id="520d1-166">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="520d1-167">В **извлечение интерфейса** диалоговое окно, нажмите кнопку **выбрать все**.</span><span class="sxs-lookup"><span data-stu-id="520d1-167">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="520d1-168">Оставьте остальные значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="520d1-168">Leave the other defaults.</span></span> <span data-ttu-id="520d1-169">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="520d1-169">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="520d1-170">Visual Studio создает новый интерфейс с именем `IStockTicker`, а также изменяет `StockTicker` для наследования от `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="520d1-170">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="520d1-171">Откройте файл IStockTicker.cs и изменить интерфейс для **открытый**.</span><span class="sxs-lookup"><span data-stu-id="520d1-171">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="520d1-172">В `StockTickerHub` измените два экземпляра `StockTicker` для `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="520d1-172">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="520d1-173">Создание `IStockTicker` интерфейса не является обязательным, но я хотел показать, как внедрение Зависимостей позволяет сократить взаимозависимости между компонентами приложения.</span><span class="sxs-lookup"><span data-stu-id="520d1-173">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="520d1-174">Добавьте библиотеку Ninject</span><span class="sxs-lookup"><span data-stu-id="520d1-174">Add the Ninject Library</span></span>

<span data-ttu-id="520d1-175">Существует множество контейнеров инверсии управления открытым исходным кодом для .NET.</span><span class="sxs-lookup"><span data-stu-id="520d1-175">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="520d1-176">В этом учебнике я использую [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="520d1-176">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="520d1-177">(Включить других популярных библиотек [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), и [StructureMap ](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="520d1-177">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="520d1-178">С помощью диспетчера пакетов NuGet для установки [Ninject библиотеки](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="520d1-178">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="520d1-179">В Visual Studio из **средства** меню выберите **диспетчер пакетов библиотеки** | **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="520d1-179">In Visual Studio, from the **Tools** menu select **Library Package Manager** | **Package Manager Console**.</span></span> <span data-ttu-id="520d1-180">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="520d1-180">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="520d1-181">Замените Сопоставитель зависимостей SignalR</span><span class="sxs-lookup"><span data-stu-id="520d1-181">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="520d1-182">Чтобы использовать Ninject в SignalR, создайте класс, производный от **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="520d1-182">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="520d1-183">Этот класс переопределяет **GetService** и **GetServices** методы **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="520d1-183">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="520d1-184">SignalR эти методы вызываются для создания различных объектов во время выполнения, включая экземпляры центра, а также различные службы, используемые внутри SignalR.</span><span class="sxs-lookup"><span data-stu-id="520d1-184">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="520d1-185">**GetService** метод создает один экземпляр типа.</span><span class="sxs-lookup"><span data-stu-id="520d1-185">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="520d1-186">Переопределите этот метод для вызова ядра Ninject **TryGet** метод.</span><span class="sxs-lookup"><span data-stu-id="520d1-186">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="520d1-187">Если этот метод возвращает значение null, переключиться на Сопоставитель по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="520d1-187">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="520d1-188">**GetServices** метод создает коллекцию объектов указанного типа.</span><span class="sxs-lookup"><span data-stu-id="520d1-188">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="520d1-189">Переопределите этот метод, чтобы объединить результаты из Ninject с результатами из Сопоставитель по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="520d1-189">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="520d1-190">Настройка привязок Ninject</span><span class="sxs-lookup"><span data-stu-id="520d1-190">Configure Ninject Bindings</span></span>

<span data-ttu-id="520d1-191">Теперь мы будем использовать для объявления привязки типов Ninject.</span><span class="sxs-lookup"><span data-stu-id="520d1-191">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="520d1-192">Откройте файл RegisterHubs.cs.</span><span class="sxs-lookup"><span data-stu-id="520d1-192">Open the file RegisterHubs.cs.</span></span> <span data-ttu-id="520d1-193">В `RegisterHubs.Start` метод, создайте контейнер Ninject, который вызывает Ninject *ядра*.</span><span class="sxs-lookup"><span data-stu-id="520d1-193">In the `RegisterHubs.Start` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="520d1-194">Создайте экземпляр сопоставителя наших пользовательскую зависимость:</span><span class="sxs-lookup"><span data-stu-id="520d1-194">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="520d1-195">Создать привязку для `IStockTicker` следующим образом:</span><span class="sxs-lookup"><span data-stu-id="520d1-195">Create a binding for `IStockTicker` as follows:</span></span>

[!code-html[Main](dependency-injection/samples/sample17.html)]

<span data-ttu-id="520d1-196">Этот код говорит две вещи.</span><span class="sxs-lookup"><span data-stu-id="520d1-196">This code is saying two things.</span></span> <span data-ttu-id="520d1-197">Во-первых, каждый раз, когда приложение должно `IStockTicker`, ядро следует создать экземпляр `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="520d1-197">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="520d1-198">Во-вторых, `StockTicker` класс должен быть созданный объект в виде одноэлементного объекта.</span><span class="sxs-lookup"><span data-stu-id="520d1-198">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="520d1-199">Ninject будет создать один экземпляр объекта и возвращают один и тот же экземпляр для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="520d1-199">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="520d1-200">Создать привязку для **IHubConnectionContext** следующим образом:</span><span class="sxs-lookup"><span data-stu-id="520d1-200">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="520d1-201">Этот код creatres анонимную функцию, которая возвращает **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="520d1-201">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="520d1-202">**WhenInjectedInto** метод указывает Ninject, чтобы использовать эту функцию только в том случае, при создании `IStockTicker` экземпляров.</span><span class="sxs-lookup"><span data-stu-id="520d1-202">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="520d1-203">Причина в том, что создает SignalR **IHubConnectionContext** экземпляров на внутреннем уровне и мы не хотим переопределить как SignalR создает их.</span><span class="sxs-lookup"><span data-stu-id="520d1-203">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="520d1-204">Эта функция применяется только к нашей `StockTicker` класса.</span><span class="sxs-lookup"><span data-stu-id="520d1-204">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="520d1-205">Передайте в сопоставителе зависимостей в **MapHubs** метод:</span><span class="sxs-lookup"><span data-stu-id="520d1-205">Pass the dependency resolver into the **MapHubs** method:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="520d1-206">Теперь SignalR будет использовать сопоставителя, указанного в **MapHubs**, вместо Сопоставитель по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="520d1-206">Now SignalR will use the resolver specified in **MapHubs**, instead of the default resolver.</span></span>

<span data-ttu-id="520d1-207">Ниже приведен полный код для `RegisterHubs.Start`.</span><span class="sxs-lookup"><span data-stu-id="520d1-207">Here is the complete code listing for `RegisterHubs.Start`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="520d1-208">Чтобы запустить приложение StockTicker в Visual Studio, нажмите клавишу F5.</span><span class="sxs-lookup"><span data-stu-id="520d1-208">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="520d1-209">В окне браузера перейдите к `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="520d1-209">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="520d1-210">Приложение имеет ровно ту же функциональность, что перед.</span><span class="sxs-lookup"><span data-stu-id="520d1-210">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="520d1-211">(Описание, см. в разделе [передача сообщений с сервера с помощью SignalR](index.md).) Мы еще не изменили поведение; просто упростилась код для тестирования, поддержки и развиваться.</span><span class="sxs-lookup"><span data-stu-id="520d1-211">(For a description, see [Server Broadcast with ASP.NET SignalR](index.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
