---
uid: signalr/overview/advanced/dependency-injection
title: Внедрение зависимостей в SignalR | Документация Майкрософт
author: MikeWasson
description: Версии программного обеспечения используется в этом разделе 4.5 .NET SignalR для Visual Studio 2013 версии 2, предыдущие версии в этом разделе сведения о более ранних версиях...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 607738e7531eaf9ee9f6a24267b65e153cc4d599
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912875"
---
<a name="dependency-injection-in-signalr"></a><span data-ttu-id="9f910-103">Внедрение зависимостей в SignalR</span><span class="sxs-lookup"><span data-stu-id="9f910-103">Dependency Injection in SignalR</span></span>
====================
<span data-ttu-id="9f910-104">по [Майк Уоссон](https://github.com/MikeWasson), [Патрик Флетчера](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="9f910-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="9f910-105">Версии программного обеспечения, используемого в этом разделе</span><span class="sxs-lookup"><span data-stu-id="9f910-105">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="9f910-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="9f910-106">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="9f910-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="9f910-107">.NET 4.5</span></span>
> - <span data-ttu-id="9f910-108">SignalR версии 2</span><span class="sxs-lookup"><span data-stu-id="9f910-108">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="9f910-109">Предыдущие версии этого раздела</span><span class="sxs-lookup"><span data-stu-id="9f910-109">Previous versions of this topic</span></span>
>
> <span data-ttu-id="9f910-110">Сведения о более ранних версий SignalR, см. в разделе [более старых версий SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="9f910-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="9f910-111">Вопросы и комментарии</span><span class="sxs-lookup"><span data-stu-id="9f910-111">Questions and comments</span></span>
>
> <span data-ttu-id="9f910-112">Оставьте свои отзывы на том, как вам понравилось, и этот учебник и что можно улучшить в комментариях в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="9f910-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="9f910-113">Если у вас есть вопросы, которые не имеют отношения к руководству, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="9f910-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="9f910-114">Внедрение зависимостей является способ удаления жестких зависимостей между объектами, упрощая для замены зависимости объекта, либо для тестирования (с использованием макетов объектов) или для изменения поведения во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="9f910-114">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="9f910-115">Этом руководстве показано, как выполнить внедрения зависимостей для концентраторов SignalR.</span><span class="sxs-lookup"><span data-stu-id="9f910-115">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="9f910-116">Также показано, как использовать контейнеры IoC с SignalR.</span><span class="sxs-lookup"><span data-stu-id="9f910-116">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="9f910-117">IoC-контейнер — это стандартная структура для внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="9f910-117">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="9f910-118">Что такое внедрение зависимостей?</span><span class="sxs-lookup"><span data-stu-id="9f910-118">What is Dependency Injection?</span></span>

<span data-ttu-id="9f910-119">Этот раздел можно пропустите, если вы уже знакомы с помощью внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="9f910-119">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="9f910-120">*Внедрение зависимостей* (DI) — это шаблон, где объекты не отвечают за создание собственных зависимостей.</span><span class="sxs-lookup"><span data-stu-id="9f910-120">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="9f910-121">Ниже приведен простой пример для внедрения Зависимостей.</span><span class="sxs-lookup"><span data-stu-id="9f910-121">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="9f910-122">Предположим, что у вас есть объект, который требуется для регистрации сообщений.</span><span class="sxs-lookup"><span data-stu-id="9f910-122">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="9f910-123">Можно определить интерфейс ведения журнала:</span><span class="sxs-lookup"><span data-stu-id="9f910-123">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="9f910-124">В объекте, можно создать `ILogger` запись сообщений в журнал:</span><span class="sxs-lookup"><span data-stu-id="9f910-124">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="9f910-125">Это работает, но это не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="9f910-125">This works, but it's not the best design.</span></span> <span data-ttu-id="9f910-126">Если вы хотите заменить `FileLogger` с другим `ILogger` реализации, нужно будет только изменить `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="9f910-126">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="9f910-127">Допустим, что много других объектов используйте `FileLogger`, необходимо изменить все из них.</span><span class="sxs-lookup"><span data-stu-id="9f910-127">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="9f910-128">Или если необходимо внести `FileLogger` является одноэлементным множеством, также необходимо внести изменения в приложении.</span><span class="sxs-lookup"><span data-stu-id="9f910-128">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="9f910-129">Лучшим подходом является «внедрение» `ILogger` в объект — например, с помощью аргумента конструктора:</span><span class="sxs-lookup"><span data-stu-id="9f910-129">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="9f910-130">Теперь объект не несет ответственности за выбора `ILogger` для использования.</span><span class="sxs-lookup"><span data-stu-id="9f910-130">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="9f910-131">Вы можете swich `ILogger` реализации без изменения объекты, зависящие от нее.</span><span class="sxs-lookup"><span data-stu-id="9f910-131">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="9f910-132">Такая модель называется [внедрение через конструктор](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="9f910-132">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="9f910-133">Другой шаблон — внедрение метод задания, где можно устанавливать для зависимости через метод или свойство.</span><span class="sxs-lookup"><span data-stu-id="9f910-133">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="9f910-134">Внедрение простого зависимостей в SignalR</span><span class="sxs-lookup"><span data-stu-id="9f910-134">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="9f910-135">Рассмотрим приложение чата из учебника [начало работы с SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="9f910-135">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="9f910-136">Ниже показан класс центра из этого приложения:</span><span class="sxs-lookup"><span data-stu-id="9f910-136">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="9f910-137">Предположим, что вам нужно хранить сообщения чата на сервере перед их отправкой.</span><span class="sxs-lookup"><span data-stu-id="9f910-137">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="9f910-138">Можно определить интерфейс, который абстрагирует эту функцию и использовать внедрение Зависимостей для вставки интерфейса в `ChatHub` класса.</span><span class="sxs-lookup"><span data-stu-id="9f910-138">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="9f910-139">Единственная проблема состоит, что приложение SignalR непосредственно не создать концентраторы; SignalR создаст их самостоятельно.</span><span class="sxs-lookup"><span data-stu-id="9f910-139">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="9f910-140">По умолчанию SignalR ожидает, что для класса концентратора конструктор без параметров.</span><span class="sxs-lookup"><span data-stu-id="9f910-140">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="9f910-141">Тем не менее можно легко зарегистрировать функцию для создания экземпляров концентраторов и использовать эту функцию для выполнения внедрения Зависимостей.</span><span class="sxs-lookup"><span data-stu-id="9f910-141">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="9f910-142">Зарегистрируйте функцию с помощью метода **GlobalHost.DependencyResolver.Register**.</span><span class="sxs-lookup"><span data-stu-id="9f910-142">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="9f910-143">Теперь SignalR будет вызывать это анонимная функция, каждый раз, когда необходимо создать `ChatHub` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="9f910-143">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="9f910-144">Контейнеры IoC</span><span class="sxs-lookup"><span data-stu-id="9f910-144">IoC Containers</span></span>

<span data-ttu-id="9f910-145">Предыдущий код годится для простых случаях.</span><span class="sxs-lookup"><span data-stu-id="9f910-145">The previous code is fine for simple cases.</span></span> <span data-ttu-id="9f910-146">Но все равно требовалось написать следующее:</span><span class="sxs-lookup"><span data-stu-id="9f910-146">But you still had to write this:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

<span data-ttu-id="9f910-147">В сложных приложениях с много зависимостей может потребоваться написать немало этот код «подключения».</span><span class="sxs-lookup"><span data-stu-id="9f910-147">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="9f910-148">Этот код может быть трудно поддерживать, особенно в том случае, если зависимости являются вложенными.</span><span class="sxs-lookup"><span data-stu-id="9f910-148">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="9f910-149">Это также трудно модульного теста.</span><span class="sxs-lookup"><span data-stu-id="9f910-149">It is also hard to unit test.</span></span>

<span data-ttu-id="9f910-150">Одним из решений является использование IoC-контейнер.</span><span class="sxs-lookup"><span data-stu-id="9f910-150">One solution is to use an IoC container.</span></span> <span data-ttu-id="9f910-151">IoC-контейнер — это программный компонент, который отвечает за управление зависимостями. Зарегистрируйте типов в контейнере и затем использовать для создания объектов контейнера.</span><span class="sxs-lookup"><span data-stu-id="9f910-151">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="9f910-152">Контейнер автоматически определяет отношение зависимость.</span><span class="sxs-lookup"><span data-stu-id="9f910-152">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="9f910-153">Кроме того, множество контейнеров инверсии управления позволяют управлять времени существования объектов и области.</span><span class="sxs-lookup"><span data-stu-id="9f910-153">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="9f910-154">«IoC» означает «инверсии элемента управления», который является общий шаблон, когда платформа вызывает в код приложения.</span><span class="sxs-lookup"><span data-stu-id="9f910-154">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="9f910-155">Контейнер IoC создает объектов, который «инвертирует» обычного потока управления.</span><span class="sxs-lookup"><span data-stu-id="9f910-155">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="9f910-156">Использование контейнеров IoC в SignalR</span><span class="sxs-lookup"><span data-stu-id="9f910-156">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="9f910-157">Приложения чата, вероятно, слишком простой использовать преимущества контейнер IoC.</span><span class="sxs-lookup"><span data-stu-id="9f910-157">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="9f910-158">Вместо этого давайте взглянем на [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) образца.</span><span class="sxs-lookup"><span data-stu-id="9f910-158">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="9f910-159">Образец StockTicker определяет два основных класса:</span><span class="sxs-lookup"><span data-stu-id="9f910-159">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="9f910-160">`StockTickerHub`: Класс концентратора, который управляет клиентских подключений.</span><span class="sxs-lookup"><span data-stu-id="9f910-160">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="9f910-161">`StockTicker`: Одноэлементным. он содержит цены акций и периодически обновлять их.</span><span class="sxs-lookup"><span data-stu-id="9f910-161">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="9f910-162">`StockTickerHub` хранит ссылку на `StockTicker` одноэлементным множеством, хотя `StockTicker` хранит ссылку на **IHubConnectionContext** для `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="9f910-162">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="9f910-163">Использует этот интерфейс для взаимодействия с `StockTickerHub` экземпляров.</span><span class="sxs-lookup"><span data-stu-id="9f910-163">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="9f910-164">(Дополнительные сведения см. в разделе [передача сообщений с сервера с помощью SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span><span class="sxs-lookup"><span data-stu-id="9f910-164">(For more information, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span></span>

<span data-ttu-id="9f910-165">Можно использовать контейнер IoC для немного клубка эти зависимости.</span><span class="sxs-lookup"><span data-stu-id="9f910-165">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="9f910-166">Во-первых давайте упростим `StockTickerHub` и `StockTicker` классы.</span><span class="sxs-lookup"><span data-stu-id="9f910-166">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="9f910-167">В следующем коде я закомментирован частей, нам не нужен.</span><span class="sxs-lookup"><span data-stu-id="9f910-167">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="9f910-168">Удалите конструктор без параметров из `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="9f910-168">Remove the parameterless constructor from `StockTickerHub`.</span></span> <span data-ttu-id="9f910-169">Вместо этого всегда используется DI создайте концентратор.</span><span class="sxs-lookup"><span data-stu-id="9f910-169">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="9f910-170">Для StockTicker удалите одноэлементный экземпляр.</span><span class="sxs-lookup"><span data-stu-id="9f910-170">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="9f910-171">Позже мы будем использовать контейнер IoC, управление временем существования StockTicker.</span><span class="sxs-lookup"><span data-stu-id="9f910-171">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="9f910-172">Кроме того не открытый конструктор.</span><span class="sxs-lookup"><span data-stu-id="9f910-172">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="9f910-173">Далее мы можем выполнить рефакторинг кода путем создания интерфейса для `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="9f910-173">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="9f910-174">Мы будем использовать этот интерфейс для отделения `StockTickerHub` из `StockTicker` класса.</span><span class="sxs-lookup"><span data-stu-id="9f910-174">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="9f910-175">Visual Studio делает этот вид рефакторинга просто.</span><span class="sxs-lookup"><span data-stu-id="9f910-175">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="9f910-176">Откройте файл StockTicker.cs, щелкните правой кнопкой мыши `StockTicker` объявление класса и выберите **рефакторинг** ... **Извлечение интерфейса**.</span><span class="sxs-lookup"><span data-stu-id="9f910-176">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="9f910-177">В **извлечение интерфейса** диалоговое окно, нажмите кнопку **выбрать все**.</span><span class="sxs-lookup"><span data-stu-id="9f910-177">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="9f910-178">Оставьте остальные значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="9f910-178">Leave the other defaults.</span></span> <span data-ttu-id="9f910-179">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="9f910-179">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="9f910-180">Visual Studio создает новый интерфейс с именем `IStockTicker`, а также изменяет `StockTicker` для наследования от `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="9f910-180">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="9f910-181">Откройте файл IStockTicker.cs и изменить интерфейс для **открытый**.</span><span class="sxs-lookup"><span data-stu-id="9f910-181">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="9f910-182">В `StockTickerHub` измените два экземпляра `StockTicker` для `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="9f910-182">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="9f910-183">Создание `IStockTicker` интерфейса не является обязательным, но я хотел показать, как внедрение Зависимостей позволяет сократить взаимозависимости между компонентами приложения.</span><span class="sxs-lookup"><span data-stu-id="9f910-183">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="9f910-184">Добавьте библиотеку Ninject</span><span class="sxs-lookup"><span data-stu-id="9f910-184">Add the Ninject Library</span></span>

<span data-ttu-id="9f910-185">Существует множество контейнеров инверсии управления открытым исходным кодом для .NET.</span><span class="sxs-lookup"><span data-stu-id="9f910-185">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="9f910-186">В этом учебнике я использую [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="9f910-186">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="9f910-187">(Включить других популярных библиотек [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), и [StructureMap ](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="9f910-187">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="9f910-188">С помощью диспетчера пакетов NuGet для установки [Ninject библиотеки](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="9f910-188">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="9f910-189">В Visual Studio из **средства** меню выберите **диспетчер пакетов NuGet** > **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="9f910-189">In Visual Studio, from the **Tools** menu select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="9f910-190">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="9f910-190">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="9f910-191">Замените Сопоставитель зависимостей SignalR</span><span class="sxs-lookup"><span data-stu-id="9f910-191">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="9f910-192">Чтобы использовать Ninject в SignalR, создайте класс, производный от **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="9f910-192">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="9f910-193">Этот класс переопределяет **GetService** и **GetServices** методы **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="9f910-193">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="9f910-194">SignalR эти методы вызываются для создания различных объектов во время выполнения, включая экземпляры центра, а также различные службы, используемые внутри SignalR.</span><span class="sxs-lookup"><span data-stu-id="9f910-194">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="9f910-195">**GetService** метод создает один экземпляр типа.</span><span class="sxs-lookup"><span data-stu-id="9f910-195">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="9f910-196">Переопределите этот метод для вызова ядра Ninject **TryGet** метод.</span><span class="sxs-lookup"><span data-stu-id="9f910-196">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="9f910-197">Если этот метод возвращает значение null, переключиться на Сопоставитель по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="9f910-197">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="9f910-198">**GetServices** метод создает коллекцию объектов указанного типа.</span><span class="sxs-lookup"><span data-stu-id="9f910-198">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="9f910-199">Переопределите этот метод, чтобы объединить результаты из Ninject с результатами из Сопоставитель по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="9f910-199">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="9f910-200">Настройка привязок Ninject</span><span class="sxs-lookup"><span data-stu-id="9f910-200">Configure Ninject Bindings</span></span>

<span data-ttu-id="9f910-201">Теперь мы будем использовать для объявления привязки типов Ninject.</span><span class="sxs-lookup"><span data-stu-id="9f910-201">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="9f910-202">Откройте класс Startup.cs приложения (либо созданного вручную согласно инструкциям пакета в `readme.txt`, или который был создан путем добавления проверки подлинности в проект).</span><span class="sxs-lookup"><span data-stu-id="9f910-202">Open your application's Startup.cs class (that you either created manually as per the package instructions in `readme.txt`, or that was created by adding authentication to your project).</span></span> <span data-ttu-id="9f910-203">В `Startup.Configuration` метод, создайте контейнер Ninject, который вызывает Ninject *ядра*.</span><span class="sxs-lookup"><span data-stu-id="9f910-203">In the `Startup.Configuration` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="9f910-204">Создайте экземпляр сопоставителя наших пользовательскую зависимость:</span><span class="sxs-lookup"><span data-stu-id="9f910-204">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="9f910-205">Создать привязку для `IStockTicker` следующим образом:</span><span class="sxs-lookup"><span data-stu-id="9f910-205">Create a binding for `IStockTicker` as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

<span data-ttu-id="9f910-206">Этот код говорит две вещи.</span><span class="sxs-lookup"><span data-stu-id="9f910-206">This code is saying two things.</span></span> <span data-ttu-id="9f910-207">Во-первых, каждый раз, когда приложение должно `IStockTicker`, ядро следует создать экземпляр `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="9f910-207">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="9f910-208">Во-вторых, `StockTicker` класс должен быть созданный объект в виде одноэлементного объекта.</span><span class="sxs-lookup"><span data-stu-id="9f910-208">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="9f910-209">Ninject будет создать один экземпляр объекта и возвращают один и тот же экземпляр для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="9f910-209">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="9f910-210">Создать привязку для **IHubConnectionContext** следующим образом:</span><span class="sxs-lookup"><span data-stu-id="9f910-210">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="9f910-211">Этот код creatres анонимную функцию, которая возвращает **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="9f910-211">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="9f910-212">**WhenInjectedInto** метод указывает Ninject, чтобы использовать эту функцию только в том случае, при создании `IStockTicker` экземпляров.</span><span class="sxs-lookup"><span data-stu-id="9f910-212">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="9f910-213">Причина в том, что создает SignalR **IHubConnectionContext** экземпляров на внутреннем уровне и мы не хотим переопределить как SignalR создает их.</span><span class="sxs-lookup"><span data-stu-id="9f910-213">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="9f910-214">Эта функция применяется только к нашей `StockTicker` класса.</span><span class="sxs-lookup"><span data-stu-id="9f910-214">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="9f910-215">Передайте в сопоставителе зависимостей в **MapSignalR** метод путем добавления конфигурации концентратора:</span><span class="sxs-lookup"><span data-stu-id="9f910-215">Pass the dependency resolver into the **MapSignalR** method by adding a hub configuration:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="9f910-216">Обновите метод Startup.ConfigureSignalR в классе запуска образца, используя новый параметр:</span><span class="sxs-lookup"><span data-stu-id="9f910-216">Update the Startup.ConfigureSignalR method in the sample's Startup class with the new parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="9f910-217">Теперь SignalR будет использовать сопоставителя, указанного в **MapSignalR**, вместо Сопоставитель по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="9f910-217">Now SignalR will use the resolver specified in **MapSignalR**, instead of the default resolver.</span></span>

<span data-ttu-id="9f910-218">Ниже приведен полный код для `Startup.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="9f910-218">Here is the complete code listing for `Startup.Configuration`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

<span data-ttu-id="9f910-219">Чтобы запустить приложение StockTicker в Visual Studio, нажмите клавишу F5.</span><span class="sxs-lookup"><span data-stu-id="9f910-219">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="9f910-220">В окне браузера перейдите к `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="9f910-220">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="9f910-221">Приложение имеет ровно ту же функциональность, что перед.</span><span class="sxs-lookup"><span data-stu-id="9f910-221">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="9f910-222">(Описание, см. в разделе [передача сообщений с сервера с помощью SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) Мы еще не изменили поведение; просто упростилась код для тестирования, поддержки и развиваться.</span><span class="sxs-lookup"><span data-stu-id="9f910-222">(For a description, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
