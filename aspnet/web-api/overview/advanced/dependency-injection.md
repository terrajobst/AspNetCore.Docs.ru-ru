---
uid: web-api/overview/advanced/dependency-injection
title: "Внедрение зависимостей в ASP.NET Web API 2 | Документы Microsoft"
author: MikeWasson
description: "Этот учебник показывает, как для вставки зависимости на контроллере веб-API ASP.NET. Версии программного обеспечения, используемые в блоке Unity приложения учебника Web API 2..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: b4cf39c59ed257b0014dbdbecef3eb7bc48f410d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="dependency-injection-in-aspnet-web-api-2"></a><span data-ttu-id="0baa1-104">Внедрение зависимостей в ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="0baa1-104">Dependency Injection in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="0baa1-105">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0baa1-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="0baa1-106">Загрузка завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="0baa1-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> <span data-ttu-id="0baa1-107">Этот учебник показывает, как для вставки зависимости на контроллере веб-API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0baa1-107">This tutorial shows how to inject dependencies into your ASP.NET Web API controller.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="0baa1-108">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="0baa1-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="0baa1-109">Веб-API 2</span><span class="sxs-lookup"><span data-stu-id="0baa1-109">Web API 2</span></span>
> - [<span data-ttu-id="0baa1-110">Блок приложения Unity</span><span class="sxs-lookup"><span data-stu-id="0baa1-110">Unity Application Block</span></span>](https://www.nuget.org/packages/Unity/)
> - <span data-ttu-id="0baa1-111">Entity Framework 6 (версия 5 также работает)</span><span class="sxs-lookup"><span data-stu-id="0baa1-111">Entity Framework 6 (version 5 also works)</span></span>


## <a name="what-is-dependency-injection"></a><span data-ttu-id="0baa1-112">Что такое внедрения зависимости</span><span class="sxs-lookup"><span data-stu-id="0baa1-112">What is Dependency Injection?</span></span>

<span data-ttu-id="0baa1-113">Объект *зависимостей* — это любой объект, который требует другой объект.</span><span class="sxs-lookup"><span data-stu-id="0baa1-113">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="0baa1-114">Например, общие для определения [репозитория](http://martinfowler.com/eaaCatalog/repository.html) , обрабатывает доступ к данным.</span><span class="sxs-lookup"><span data-stu-id="0baa1-114">For example, it's common to define a [repository](http://martinfowler.com/eaaCatalog/repository.html) that handles data access.</span></span> <span data-ttu-id="0baa1-115">Давайте рассмотрим пример.</span><span class="sxs-lookup"><span data-stu-id="0baa1-115">Let's illustrate with an example.</span></span> <span data-ttu-id="0baa1-116">Во-первых мы определим модель домена:</span><span class="sxs-lookup"><span data-stu-id="0baa1-116">First, we'll define a domain model:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="0baa1-117">Ниже приведен простой репозиторий класс, который хранит элементы в базе данных, использующий Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0baa1-117">Here is a simple repository class that stores items in a database, using Entity Framework.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="0baa1-118">Теперь давайте определить контроллер веб-API, который поддерживает запросы GET для `Product` сущностей.</span><span class="sxs-lookup"><span data-stu-id="0baa1-118">Now let's define a Web API controller that supports GET requests for `Product` entities.</span></span> <span data-ttu-id="0baa1-119">(Я пропускают POST, а также другие методы для простоты.) Вот первой попытки.</span><span class="sxs-lookup"><span data-stu-id="0baa1-119">(I'm leaving out POST and other methods for simplicity.) Here is a first attempt:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="0baa1-120">Обратите внимание, что зависит от класса контроллера `ProductRepository`, и мы позволяя создать контроллер `ProductRepository` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="0baa1-120">Notice that the controller class depends on `ProductRepository`, and we are letting the controller create the `ProductRepository` instance.</span></span> <span data-ttu-id="0baa1-121">Однако это рекомендуется жестко зависимостей таким образом, по следующим причинам.</span><span class="sxs-lookup"><span data-stu-id="0baa1-121">However, it's a bad idea to hard code the dependency in this way, for several reasons.</span></span>

- <span data-ttu-id="0baa1-122">Если вы хотите заменить `ProductRepository` с другой реализации, необходимо также изменить класс контроллера.</span><span class="sxs-lookup"><span data-stu-id="0baa1-122">If you want to replace `ProductRepository` with a different implementation, you also need to modify the controller class.</span></span>
- <span data-ttu-id="0baa1-123">Если `ProductRepository` имеет зависимости, необходимо настроить в контроллере.</span><span class="sxs-lookup"><span data-stu-id="0baa1-123">If the `ProductRepository` has dependencies, you must configure these inside the controller.</span></span> <span data-ttu-id="0baa1-124">Для больших проектов с несколькими контроллерами код конфигурации становится в разных частях проекта.</span><span class="sxs-lookup"><span data-stu-id="0baa1-124">For a large project with multiple controllers, your configuration code becomes scattered across your project.</span></span>
- <span data-ttu-id="0baa1-125">Трудно модульный тест, так как данный контроллер является жестко запрограммировано на запрос в базу данных.</span><span class="sxs-lookup"><span data-stu-id="0baa1-125">It is hard to unit test, because the controller is hard-coded to query the database.</span></span> <span data-ttu-id="0baa1-126">Для модульного теста следует использовать репозитории макетов или заглушки, что невозможно при помощи конструктора currect.</span><span class="sxs-lookup"><span data-stu-id="0baa1-126">For a unit test, you should use a mock or stub repository, which is not possible with the currect design.</span></span>

<span data-ttu-id="0baa1-127">Можно устранить эти проблемы с *вводится* репозитория в контроллер.</span><span class="sxs-lookup"><span data-stu-id="0baa1-127">We can address these problems by *injecting* the repository into the controller.</span></span> <span data-ttu-id="0baa1-128">Во-первых, рефакторинг `ProductRepository` класс в интерфейсе:</span><span class="sxs-lookup"><span data-stu-id="0baa1-128">First, refactor the `ProductRepository` class into an interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="0baa1-129">Затем укажите `IProductRepository` как параметр конструктора:</span><span class="sxs-lookup"><span data-stu-id="0baa1-129">Then provide the `IProductRepository` as a constructor parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="0baa1-130">В этом примере используется [внедрение конструктора](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="0baa1-130">This example uses [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="0baa1-131">Можно также использовать *внедрения setter*, где установить зависимость через метод или свойство.</span><span class="sxs-lookup"><span data-stu-id="0baa1-131">You can also use *setter injection*, where you set the dependency through a setter method or property.</span></span>

<span data-ttu-id="0baa1-132">Но теперь имеется проблема, поскольку приложение не создает контроллер напрямую.</span><span class="sxs-lookup"><span data-stu-id="0baa1-132">But now there is a problem, because your application doesn't create the controller directly.</span></span> <span data-ttu-id="0baa1-133">Создает веб-API контроллер, когда он направляет запрос и веб-API ничего не знает о `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="0baa1-133">Web API creates the controller when it routes the request, and Web API doesn't know anything about `IProductRepository`.</span></span> <span data-ttu-id="0baa1-134">Это происходит, когда вступает в дело Сопоставитель зависимостей веб-API.</span><span class="sxs-lookup"><span data-stu-id="0baa1-134">This is where the Web API dependency resolver comes in.</span></span>

## <a name="the-web-api-dependency-resolver"></a><span data-ttu-id="0baa1-135">Сопоставитель зависимостей веб-API</span><span class="sxs-lookup"><span data-stu-id="0baa1-135">The Web API Dependency Resolver</span></span>

<span data-ttu-id="0baa1-136">Определяет веб-API **IDependencyResolver** интерфейс для разрешении зависимостей.</span><span class="sxs-lookup"><span data-stu-id="0baa1-136">Web API defines the **IDependencyResolver** interface for resolving dependencies.</span></span> <span data-ttu-id="0baa1-137">Вот определение интерфейса:</span><span class="sxs-lookup"><span data-stu-id="0baa1-137">Here is the definition of the interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="0baa1-138">**IDependencyScope** интерфейса есть два метода:</span><span class="sxs-lookup"><span data-stu-id="0baa1-138">The **IDependencyScope** interface has two methods:</span></span>

- <span data-ttu-id="0baa1-139">**GetService** создает один экземпляр типа.</span><span class="sxs-lookup"><span data-stu-id="0baa1-139">**GetService** creates one instance of a type.</span></span>
- <span data-ttu-id="0baa1-140">**Метод GetServices** создает коллекцию объектов определенного типа.</span><span class="sxs-lookup"><span data-stu-id="0baa1-140">**GetServices** creates a collection of objects of a specified type.</span></span>

<span data-ttu-id="0baa1-141">**IDependencyResolver** наследует метод **IDependencyScope** и добавляет **BeginScope** метод.</span><span class="sxs-lookup"><span data-stu-id="0baa1-141">The **IDependencyResolver** method inherits **IDependencyScope** and adds the **BeginScope** method.</span></span> <span data-ttu-id="0baa1-142">Мы поговорим об областях далее в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="0baa1-142">I'll talk about scopes later in this tutorial.</span></span>

<span data-ttu-id="0baa1-143">Если веб-API создает экземпляр контроллера, он сначала вызывает **IDependencyResolver.GetService**, передав тип контроллера.</span><span class="sxs-lookup"><span data-stu-id="0baa1-143">When Web API creates a controller instance, it first calls **IDependencyResolver.GetService**, passing in the controller type.</span></span> <span data-ttu-id="0baa1-144">Этот обработчик расширения можно использовать для создания контроллера, разрешаются любые зависимости.</span><span class="sxs-lookup"><span data-stu-id="0baa1-144">You can use this extensibility hook to create the controller, resolving any dependencies.</span></span> <span data-ttu-id="0baa1-145">Если **GetService** возвращает значение null, веб-API ищет конструктор в класс контроллера.</span><span class="sxs-lookup"><span data-stu-id="0baa1-145">If **GetService** returns null, Web API looks for a parameterless constructor on the controller class.</span></span>

## <a name="dependency-resolution-with-the-unity-container"></a><span data-ttu-id="0baa1-146">Разрешение зависимостей с контейнером Unity</span><span class="sxs-lookup"><span data-stu-id="0baa1-146">Dependency Resolution with the Unity Container</span></span>

<span data-ttu-id="0baa1-147">Несмотря на то, что можно написать полный **IDependencyResolver** действительно реализацию с нуля, интерфейс предназначен для работы в качестве моста между веб-API и существующие контейнеры IoC.</span><span class="sxs-lookup"><span data-stu-id="0baa1-147">Although you could write a complete **IDependencyResolver** implementation from scratch, the interface is really designed to act as bridge between Web API and existing IoC containers.</span></span>

<span data-ttu-id="0baa1-148">Контейнер IoC — это программный компонент, который отвечает за управление зависимостями.</span><span class="sxs-lookup"><span data-stu-id="0baa1-148">An IoC container is a software component that is responsible for managing dependencies.</span></span> <span data-ttu-id="0baa1-149">Зарегистрировать типы с контейнером и затем использовать для создания объектов контейнера.</span><span class="sxs-lookup"><span data-stu-id="0baa1-149">You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="0baa1-150">Контейнер автоматически решает, отношений зависимостей.</span><span class="sxs-lookup"><span data-stu-id="0baa1-150">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="0baa1-151">Многие контейнеры IoC также позволяют управлять вещей, как время существования объектов и области.</span><span class="sxs-lookup"><span data-stu-id="0baa1-151">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="0baa1-152">«IoC» означает «инверсии элемента управления», который является общий шаблон, когда платформа вызывает в код приложения.</span><span class="sxs-lookup"><span data-stu-id="0baa1-152">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="0baa1-153">Контейнер IoC создает объекты, который «инвертирует «обычного потока управления.</span><span class="sxs-lookup"><span data-stu-id="0baa1-153">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


<span data-ttu-id="0baa1-154">В этом учебнике мы будем использовать [Unity](https://msdn.microsoft.com/en-us/library/ff647202.aspx) из шаблонов Microsoft &amp; рекомендации.</span><span class="sxs-lookup"><span data-stu-id="0baa1-154">For this tutorial, we'll use [Unity](https://msdn.microsoft.com/en-us/library/ff647202.aspx) from Microsoft Patterns &amp; Practices.</span></span> <span data-ttu-id="0baa1-155">(Другие популярные библиотеки включают в себя [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), и [StructureMap ](http://docs.structuremap.net/).) Установка Unity можно использовать диспетчер пакетов NuGet.</span><span class="sxs-lookup"><span data-stu-id="0baa1-155">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), and [StructureMap](http://docs.structuremap.net/).) You can use NuGet Package Manager to install Unity.</span></span> <span data-ttu-id="0baa1-156">Из **средства** в Visual Studio, выберите пункт меню **диспетчер пакетов библиотеки**, а затем выберите **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="0baa1-156">From the **Tools** menu in Visual Studio, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="0baa1-157">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="0baa1-157">In the Package Manager Console window, type the following command:</span></span>

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

<span data-ttu-id="0baa1-158">Здесь — это реализация **IDependencyResolver** -оболочки контейнера Unity.</span><span class="sxs-lookup"><span data-stu-id="0baa1-158">Here is an implementation of **IDependencyResolver** that wraps a Unity container.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="0baa1-159">Если **GetService** метод не может разрешить тип, он должен возвращать **null**.</span><span class="sxs-lookup"><span data-stu-id="0baa1-159">If the **GetService** method cannot resolve a type, it should return **null**.</span></span> <span data-ttu-id="0baa1-160">Если **GetServices** метод не может разрешить тип, он должен возвращать пустой объект коллекции.</span><span class="sxs-lookup"><span data-stu-id="0baa1-160">If the **GetServices** method cannot resolve a type, it should return an empty collection object.</span></span> <span data-ttu-id="0baa1-161">Не следует создавать исключения для неизвестных типов.</span><span class="sxs-lookup"><span data-stu-id="0baa1-161">Don't throw exceptions for unknown types.</span></span>


## <a name="configuring-the-dependency-resolver"></a><span data-ttu-id="0baa1-162">Настройка сопоставителя зависимостей</span><span class="sxs-lookup"><span data-stu-id="0baa1-162">Configuring the Dependency Resolver</span></span>

<span data-ttu-id="0baa1-163">Установить средство разрешения зависимостей для **DependencyResolver** свойство глобального **HttpConfiguration** объекта.</span><span class="sxs-lookup"><span data-stu-id="0baa1-163">Set the dependency resolver on the **DependencyResolver** property of the global **HttpConfiguration** object.</span></span>

<span data-ttu-id="0baa1-164">В следующем примере кода регистры `IProductRepository` взаимодействовать с Unity, а затем создает `UnityResolver`.</span><span class="sxs-lookup"><span data-stu-id="0baa1-164">The following code registers the `IProductRepository` interface with Unity and then creates a `UnityResolver`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a><span data-ttu-id="0baa1-165">Область зависимостей и временем существования контроллера</span><span class="sxs-lookup"><span data-stu-id="0baa1-165">Dependency Scope and Controller Lifetime</span></span>

<span data-ttu-id="0baa1-166">Контроллеры создаются для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="0baa1-166">Controllers are created per request.</span></span> <span data-ttu-id="0baa1-167">Для управления временем жизни объектов, **IDependencyResolver** использует концепцию *область*.</span><span class="sxs-lookup"><span data-stu-id="0baa1-167">To manage object lifetimes, **IDependencyResolver** uses the concept of a *scope*.</span></span>

<span data-ttu-id="0baa1-168">Сопоставитель зависимостей присоединяется к **HttpConfiguration** объект имеет глобальную область.</span><span class="sxs-lookup"><span data-stu-id="0baa1-168">The dependency resolver attached to the **HttpConfiguration** object has global scope.</span></span> <span data-ttu-id="0baa1-169">Если веб-API будет создан контроллер, он вызывает **BeginScope**.</span><span class="sxs-lookup"><span data-stu-id="0baa1-169">When Web API creates a controller, it calls **BeginScope**.</span></span> <span data-ttu-id="0baa1-170">Этот метод возвращает **IDependencyScope** , представляющий дочерней области.</span><span class="sxs-lookup"><span data-stu-id="0baa1-170">This method returns an **IDependencyScope** that represents a child scope.</span></span>

<span data-ttu-id="0baa1-171">Затем вызывает веб-API **GetService** в дочерней области для создания контроллера.</span><span class="sxs-lookup"><span data-stu-id="0baa1-171">Web API then calls **GetService** on the child scope to create the controller.</span></span> <span data-ttu-id="0baa1-172">При запросе веб-API вызывает **Dispose** в дочерней области.</span><span class="sxs-lookup"><span data-stu-id="0baa1-172">When request is complete, Web API calls **Dispose** on the child scope.</span></span> <span data-ttu-id="0baa1-173">Используйте **Dispose** метод для удаления зависимостей контроллера.</span><span class="sxs-lookup"><span data-stu-id="0baa1-173">Use the **Dispose** method to dispose of the controller's dependencies.</span></span>

<span data-ttu-id="0baa1-174">Реализация **BeginScope** зависит от контейнер IoC.</span><span class="sxs-lookup"><span data-stu-id="0baa1-174">How you implement **BeginScope** depends on the IoC container.</span></span> <span data-ttu-id="0baa1-175">Область для Unity, соответствует дочернего контейнера:</span><span class="sxs-lookup"><span data-stu-id="0baa1-175">For Unity, scope corresponds to a child container:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

<span data-ttu-id="0baa1-176">Большинство IoC контейнеры имеют аналогичные эквиваленты.</span><span class="sxs-lookup"><span data-stu-id="0baa1-176">Most IoC containers have similar equivalents.</span></span>
