---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: "Внедрение зависимостей в ASP.NET MVC 4 | Документы Microsoft"
author: rick-anderson
description: "Примечание: Это Практическое лабораторное занятие предполагается наличие базовых знаний о фильтрах ASP.NET MVC и ASP.NET MVC 4. Если вы не использовали фильтров ASP.NET MVC 4 перед мы rec..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: af4967f642ba4615f3392c0c404d2ec62edaaae8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4-dependency-injection"></a><span data-ttu-id="e7cd8-104">Внедрение зависимостей в ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="e7cd8-104">ASP.NET MVC 4 Dependency Injection</span></span>
====================
<span data-ttu-id="e7cd8-105">по [Web лагеря команды](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="e7cd8-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> [!NOTE]
> <span data-ttu-id="e7cd8-106">Это Практическое лабораторное занятие предполагает иметь базовые знания об **ASP.NET MVC** и **фильтров ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-106">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC** and **ASP.NET MVC 4 filters**.</span></span> <span data-ttu-id="e7cd8-107">Если вы не использовали **фильтров ASP.NET MVC 4** раньше, мы рекомендуем перейти **настраиваемые действия ASP.NET MVC фильтры** Практическое лабораторное занятие.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-107">If you have not used **ASP.NET MVC 4 filters** before, we recommend you to go over **ASP.NET MVC Custom Action Filters** Hands-on Lab.</span></span>
> 
> <span data-ttu-id="e7cd8-108">Все образцы кода и фрагменты кода включаются в Web лагеря комплект учебных материалов, доступных в [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).</span><span class="sxs-lookup"><span data-stu-id="e7cd8-108">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).</span></span>


<span data-ttu-id="e7cd8-109">В **объектно-ориентированное программирование** парадигме, совместной работы объектов в модели совместная работа которых содержится участников и пользователей.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-109">In **Object Oriented Programming** paradigm, objects work together in a collaboration model where there are contributors and consumers.</span></span> <span data-ttu-id="e7cd8-110">Естественно эта модель связи приводит к возникновению ошибки зависимости между объектами и компонентами, становится трудно управлять при увеличении сложности.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-110">Naturally, this communication model generates dependencies between objects and components, becoming difficult to manage when complexity increases.</span></span>

<span data-ttu-id="e7cd8-111">![Класс зависимости и сложности модели](aspnet-mvc-4-dependency-injection/_static/image1.png "класса зависимости и сложности модели")</span><span class="sxs-lookup"><span data-stu-id="e7cd8-111">![Class dependencies and model complexity](aspnet-mvc-4-dependency-injection/_static/image1.png "Class dependencies and model complexity")</span></span>

<span data-ttu-id="e7cd8-112">*Класс зависимости и сложность модели*</span><span class="sxs-lookup"><span data-stu-id="e7cd8-112">*Class dependencies and model complexity*</span></span>

<span data-ttu-id="e7cd8-113">Возможно, вы слышали о **шаблон фабрики** и разделения между интерфейсом и реализации с помощью служб, где клиентские объекты часто отвечают за расположение службы.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-113">You have probably heard about the **Factory Pattern** and the separation between the interface and the implementation using services, where the client objects are often responsible for service location.</span></span>

<span data-ttu-id="e7cd8-114">Шаблон внедрения зависимостей — конкретной реализации инверсии элемента управления.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-114">The Dependency Injection pattern is a particular implementation of Inversion of Control.</span></span> <span data-ttu-id="e7cd8-115">**Инверсии управления (IoC)** означает, что объекты не создавать другие объекты, на которые они используют для выполнения своих задач.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-115">**Inversion of Control (IoC)** means that objects do not create other objects on which they rely to do their work.</span></span> <span data-ttu-id="e7cd8-116">Вместо этого они получить объекты, которые им необходимы из внешнего источника (например, файл конфигурации xml).</span><span class="sxs-lookup"><span data-stu-id="e7cd8-116">Instead, they get the objects that they need from an outside source (for example, an xml configuration file).</span></span>

<span data-ttu-id="e7cd8-117">**Внедрения зависимости (DI)** означает, что это осуществляется без вмешательства объекта, обычно компонентом платформы, который передает параметры конструктора и задать свойства.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-117">**Dependency Injection (DI)** means that this is done without the object intervention, usually by a framework component that passes constructor parameters and set properties.</span></span>

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a><span data-ttu-id="e7cd8-118">Шаблон разработки (DI) внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="e7cd8-118">The Dependency Injection (DI) Design Pattern</span></span>

<span data-ttu-id="e7cd8-119">На высоком уровне внедрения зависимостей предназначена, класс клиента (например *golfer*) требуется нечто, удовлетворяющий интерфейс (например *IClub*).</span><span class="sxs-lookup"><span data-stu-id="e7cd8-119">At a high level, the goal of Dependency Injection is that a client class (e.g. *the golfer*) needs something that satisfies an interface (e.g. *IClub*).</span></span> <span data-ttu-id="e7cd8-120">Не проверяет, имеет конкретный тип (например *WedgeClub WoodClub IronClub,* или *PutterClub*), ему кто-то другой для обработки (например хорошо *caddy*).</span><span class="sxs-lookup"><span data-stu-id="e7cd8-120">It doesn't care what the concrete type is (e.g. *WoodClub, IronClub, WedgeClub* or *PutterClub*), it wants someone else to handle that (e.g. a good *caddy*).</span></span> <span data-ttu-id="e7cd8-121">Сопоставитель зависимостей в ASP.NET MVC позволяет зарегистрировать где-нибудь еще логику зависимостей (например контейнера или *контейнера треф*).</span><span class="sxs-lookup"><span data-stu-id="e7cd8-121">The Dependency Resolver in ASP.NET MVC can allow you to register your dependency logic somewhere else (e.g. a container or a *bag of clubs*).</span></span>

<span data-ttu-id="e7cd8-122">![Диаграмма внедрения зависимостей](aspnet-mvc-4-dependency-injection/_static/image2.png "рисунке внедрения зависимости")</span><span class="sxs-lookup"><span data-stu-id="e7cd8-122">![Dependency Injection diagram](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection illustration")</span></span>

<span data-ttu-id="e7cd8-123">*Внедрение зависимостей - аналогию Гольф*</span><span class="sxs-lookup"><span data-stu-id="e7cd8-123">*Dependency Injection - Golf analogy*</span></span>

<span data-ttu-id="e7cd8-124">Ниже перечислены преимущества использования шаблона внедрения зависимостей и инверсии элемента управления.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-124">The advantages of using Dependency Injection pattern and Inversion of Control are the following:</span></span>

- <span data-ttu-id="e7cd8-125">Уменьшает соединения классов</span><span class="sxs-lookup"><span data-stu-id="e7cd8-125">Reduces class coupling</span></span>
- <span data-ttu-id="e7cd8-126">Увеличивает повторное использование кода</span><span class="sxs-lookup"><span data-stu-id="e7cd8-126">Increases code reusing</span></span>
- <span data-ttu-id="e7cd8-127">Повышает удобство поддержки кода</span><span class="sxs-lookup"><span data-stu-id="e7cd8-127">Improves code maintainability</span></span>
- <span data-ttu-id="e7cd8-128">Повышает тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="e7cd8-128">Improves application testing</span></span>

> [!NOTE]
> <span data-ttu-id="e7cd8-129">Внедрение зависимостей, иногда сравнивается с абстрактный фабричного конструктивного шаблона, но есть небольшие различия между оба подхода.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-129">Dependency Injection is sometimes compared with Abstract Factory Design Pattern, but there is a slight difference between both approaches.</span></span> <span data-ttu-id="e7cd8-130">DI установлена платформа Framework над за тем, чтобы устранить зависимости, для вызова фабрики и зарегистрированных служб.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-130">DI has a Framework working behind to solve dependencies by calling the factories and the registered services.</span></span>


<span data-ttu-id="e7cd8-131">Теперь, когда вы знаете шаблон внедрения зависимостей, вы узнаете в этой лаборатории как они применяются в ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-131">Now that you understand the Dependency Injection Pattern, you will learn throughout this lab how to apply it in ASP.NET MVC 4.</span></span> <span data-ttu-id="e7cd8-132">Сначала с помощью внедрения зависимости в **контроллеров** служба доступа к базе данных.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-132">You will start using Dependency Injection in the **Controllers** to include a database access service.</span></span> <span data-ttu-id="e7cd8-133">Затем примените внедрения зависимостей для **представления** для использования службы и отображения сведений.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-133">Next, you will apply Dependency Injection to the **Views** to consume a service and show information.</span></span> <span data-ttu-id="e7cd8-134">Наконец можно расширить DI в ASP.NET MVC 4 фильтры, добавление пользовательского фильтра действий в решении.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-134">Finally, you will extend the DI to ASP.NET MVC 4 Filters, injecting a custom action filter in the solution.</span></span>

<span data-ttu-id="e7cd8-135">В этой лаборатории практических вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="e7cd8-135">In this Hands-on Lab, you will learn how to:</span></span>

- <span data-ttu-id="e7cd8-136">Интеграция ASP.NET MVC 4 с помощью Unity для внедрения зависимости, с помощью пакетов NuGet</span><span class="sxs-lookup"><span data-stu-id="e7cd8-136">Integrate ASP.NET MVC 4 with Unity for Dependency Injection using NuGet Packages</span></span>
- <span data-ttu-id="e7cd8-137">Используют внедрение зависимостей внутри контроллера ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="e7cd8-137">Use Dependency Injection inside an ASP.NET MVC Controller</span></span>
- <span data-ttu-id="e7cd8-138">Используют внедрение зависимостей в представлении ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="e7cd8-138">Use Dependency Injection inside an ASP.NET MVC View</span></span>
- <span data-ttu-id="e7cd8-139">Используют внедрение зависимостей в ASP.NET MVC фильтра действий</span><span class="sxs-lookup"><span data-stu-id="e7cd8-139">Use Dependency Injection inside an ASP.NET MVC Action Filter</span></span>

> [!NOTE]
> <span data-ttu-id="e7cd8-140">Эта лаборатория использует Unity.Mvc3 пакет NuGet для разрешения зависимостей, но можно адаптировать любую платформу внедрения зависимостей для работы с ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-140">This Lab is using Unity.Mvc3 NuGet Package for dependency resolution, but it is possible to adapt any Dependency Injection Framework to work with ASP.NET MVC 4.</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="e7cd8-141">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="e7cd8-141">Prerequisites</span></span>

<span data-ttu-id="e7cd8-142">Необходимо иметь следующие элементы для этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-142">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="e7cd8-143">[Microsoft Visual Studio Express 2012 для Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) или выше (чтение [приложение A](#AppendixA) инструкции по его установке).</span><span class="sxs-lookup"><span data-stu-id="e7cd8-143">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="e7cd8-144">Установка</span><span class="sxs-lookup"><span data-stu-id="e7cd8-144">Setup</span></span>

<span data-ttu-id="e7cd8-145">**Установка фрагменты кода**</span><span class="sxs-lookup"><span data-stu-id="e7cd8-145">**Installing Code Snippets**</span></span>

<span data-ttu-id="e7cd8-146">Для удобства большая часть кода, который вы планируете управлять вдоль этой лаборатории доступна как фрагменты кода Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-146">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="e7cd8-147">Чтобы установить фрагменты кода запуска **.\Source\Setup\CodeSnippets.vsi** файла.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-147">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="e7cd8-148">Если вы не знакомы с фрагментов кода Visual Studio и хотите узнать о способах их использования, можно ссылаться в приложение из этого документа &quot; [с помощью фрагментов кода в приложении б.](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-148">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="e7cd8-149">Упражнения</span><span class="sxs-lookup"><span data-stu-id="e7cd8-149">Exercises</span></span>

<span data-ttu-id="e7cd8-150">Это Практическое лабораторное занятие состоит в выполнении следующих упражнений.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-150">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="e7cd8-151">Упражнение 1: Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="e7cd8-151">Exercise 1: Injecting a Controller</span></span>](#Exercise1)
2. [<span data-ttu-id="e7cd8-152">Упражнение 2: Добавление представления</span><span class="sxs-lookup"><span data-stu-id="e7cd8-152">Exercise 2: Injecting a View</span></span>](#Exercise2)
3. [<span data-ttu-id="e7cd8-153">Упражнение 3: Добавление фильтров</span><span class="sxs-lookup"><span data-stu-id="e7cd8-153">Exercise 3: Injecting Filters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="e7cd8-154">Каждого упражнения сопровождается **окончания** папку, содержащую полученное в результате решение, должен быть получен после завершения выполнения упражнений.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-154">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="e7cd8-155">Это решение можно использовать как руководство, если вам нужна дополнительная помощь при выполнении упражнений.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-155">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="e7cd8-156">Предполагаемое время для выполнения этого занятия: **30 минут**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-156">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a><span data-ttu-id="e7cd8-157">Упражнение 1: Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="e7cd8-157">Exercise 1: Injecting a Controller</span></span>

<span data-ttu-id="e7cd8-158">В этом упражнении вы узнаете, как использовать внедрение зависимостей в ASP.NET MVC Controllers интегрируя Unity с помощью пакета NuGet.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-158">In this exercise, you will learn how to use Dependency Injection in ASP.NET MVC Controllers by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="e7cd8-159">По этой причине будут включены в контроллерах MvcMusicStore для отделения логики доступа к данным служб.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-159">For that reason, you will include services into your MvcMusicStore controllers to separate the logic from the data access.</span></span> <span data-ttu-id="e7cd8-160">В конструктор контроллера, который будет разрешен с помощью внедрения зависимости с помощью служб создается новая зависимость **Unity**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-160">The services will create a new dependency in the controller constructor, which will be resolved using Dependency Injection with the help of **Unity**.</span></span>

<span data-ttu-id="e7cd8-161">Такой подход будет показано, как создать менее связанных приложений, которые являются более гибкими и простыми в обслуживании и тестировании.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-161">This approach will show you how to generate less coupled applications, which are more flexible and easier to maintain and test.</span></span> <span data-ttu-id="e7cd8-162">Также вы узнаете, как интегрировать ASP.NET MVC с помощью Unity.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-162">You will also learn how to integrate ASP.NET MVC with Unity.</span></span>

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a><span data-ttu-id="e7cd8-163">О службе StoreManager</span><span class="sxs-lookup"><span data-stu-id="e7cd8-163">About StoreManager Service</span></span>

<span data-ttu-id="e7cd8-164">MVC Music Store, предоставляемые в решении begin теперь включает службу, которая управляет контроллера хранилища данных с именем **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-164">The MVC Music Store provided in the begin solution now includes a service that manages the Store Controller data named **StoreService**.</span></span> <span data-ttu-id="e7cd8-165">Ниже Вы найдете реализации службы хранилища.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-165">Below you will find the Store Service implementation.</span></span> <span data-ttu-id="e7cd8-166">Обратите внимание, что все методы возвращают сущности модели.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-166">Note that all the methods return Model entities.</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

<span data-ttu-id="e7cd8-167">**StoreController** из начала решение использует **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-167">**StoreController** from the begin solution now consumes **StoreService**.</span></span> <span data-ttu-id="e7cd8-168">Ссылки на данные были удалены из **StoreController**и теперь можно изменить текущий поставщик доступа к данным без изменения любой метод, который использует **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-168">All the data references were removed from **StoreController**, and now possible to modify the current data access provider without changing any method that consumes **StoreService**.</span></span>

<span data-ttu-id="e7cd8-169">Вы найдете ниже, **StoreController** реализация имеет зависимость с **StoreService** внутри конструктора класса.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-169">You will find below that the **StoreController** implementation has a dependency with **StoreService** inside the class constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="e7cd8-170">Зависимости, представленные в этом упражнении, связанные с **инверсии управления** (IoC).</span><span class="sxs-lookup"><span data-stu-id="e7cd8-170">The dependency introduced in this exercise is related to **Inversion of Control** (IoC).</span></span>
> 
> <span data-ttu-id="e7cd8-171">**StoreController** конструктор класса принимает **IStoreService** параметру типа, который необходим для выполнения вызовов службы от внутри класса.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-171">The **StoreController** class constructor receives an **IStoreService** type parameter, which is essential to perform service calls from inside the class.</span></span> <span data-ttu-id="e7cd8-172">Тем не менее **StoreController** не реализует конструктор по умолчанию (с без параметров), любой контроллер необходимы для работы с ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-172">However, **StoreController** does not implement the default constructor (with no parameters) that any controller must have to work with ASP.NET MVC.</span></span>
> 
> <span data-ttu-id="e7cd8-173">Чтобы устранить зависимость, контроллер должна создаваться фабрикой абстрактным (класс, который возвращает любой объект указанного типа).</span><span class="sxs-lookup"><span data-stu-id="e7cd8-173">To resolve the dependency, the controller has to be created by an abstract factory (a class that returns any object of the specified type).</span></span>


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="e7cd8-174">Вы получите ошибку при попытке создать StoreController без отправки объекта службы, как отсутствует конструктор без параметров объявлен класс.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-174">You will get an error when the class tries to create the StoreController without sending the service object, as there is no parameterless constructor declared.</span></span>


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a><span data-ttu-id="e7cd8-175">Задача 1. запуск приложения</span><span class="sxs-lookup"><span data-stu-id="e7cd8-175">Task 1 - Running the Application</span></span>

<span data-ttu-id="e7cd8-176">В этой задаче будет запускаться приложение Begin, который включает в себя контроллер хранилища, который отделяет доступа к данным от логики приложения службы.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-176">In this task, you will run the Begin application, which includes the service into the Store Controller that separates the data access from the application logic.</span></span>

<span data-ttu-id="e7cd8-177">При запуске приложения, получит сообщение об ошибке, как служба контроллера не передается как параметр по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="e7cd8-177">When running the application, you will receive an exception, as the controller service is not passed as a parameter by default:</span></span>

1. <span data-ttu-id="e7cd8-178">Откройте **начать** решение находится в **вводится Source\Ex01 Controller\Begin**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-178">Open the **Begin** solution located in **Source\Ex01-Injecting Controller\Begin**.</span></span>

    1. <span data-ttu-id="e7cd8-179">Необходимо загрузить несколько отсутствующих пакетов NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-179">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="e7cd8-180">Чтобы сделать это, нажмите кнопку **проекта** и выбрать пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-180">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="e7cd8-181">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-181">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="e7cd8-182">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-182">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e7cd8-183">Одним из преимуществ использования NuGet является у вас не указан для отправки всех библиотек в вашем проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-183">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="e7cd8-184">С помощью NuGet Power Tools указав версий пакета в файле Packages.config можно для загрузки всех необходимых библиотек первый раз, при запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-184">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="e7cd8-185">Вот почему необходимо будет выполнить эти шаги после открытия существующего решения в этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-185">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="e7cd8-186">Нажмите клавишу **сочетание клавиш Ctrl + F5** для запуска приложения без отладки.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-186">Press **Ctrl + F5** to run the application without debugging.</span></span> <span data-ttu-id="e7cd8-187">Вы получите сообщение об ошибке &quot; **нет конструктора без параметров, определенные для этого объекта**&quot;:</span><span class="sxs-lookup"><span data-stu-id="e7cd8-187">You will get the error message &quot;**No parameterless constructor defined for this object**&quot;:</span></span>

    <span data-ttu-id="e7cd8-188">![Ошибка при выполнении приложения начать ASP.NET MVC](aspnet-mvc-4-dependency-injection/_static/image3.png "ошибка во время выполнения приложения начать ASP.NET MVC")</span><span class="sxs-lookup"><span data-stu-id="e7cd8-188">![Error while running ASP.NET MVC Begin Application](aspnet-mvc-4-dependency-injection/_static/image3.png "Error while running ASP.NET MVC Begin Application")</span></span>

    <span data-ttu-id="e7cd8-189">*Ошибка при выполнении приложения начать ASP.NET MVC*</span><span class="sxs-lookup"><span data-stu-id="e7cd8-189">*Error while running ASP.NET MVC Begin Application*</span></span>
3. <span data-ttu-id="e7cd8-190">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-190">Close the browser.</span></span>

<span data-ttu-id="e7cd8-191">В следующих шагах вы будете работать решения хранилища музыки для внедрения зависимости, необходимые для этого контроллера.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-191">In the following steps you will work on the Music Store Solution to inject the dependency this controller needs.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a><span data-ttu-id="e7cd8-192">Задача 2 - включая Unity в решение MvcMusicStore</span><span class="sxs-lookup"><span data-stu-id="e7cd8-192">Task 2 - Including Unity into MvcMusicStore Solution</span></span>

<span data-ttu-id="e7cd8-193">В этой задаче будет включать **Unity.Mvc3** пакет NuGet для решения.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-193">In this task, you will include **Unity.Mvc3** NuGet Package to the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="e7cd8-194">Unity.Mvc3 пакет, предназначенный для ASP.NET MVC 3, но он является полностью совместимым с ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-194">Unity.Mvc3 package was designed for ASP.NET MVC 3, but it is fully compatible with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="e7cd8-195">Unity — например, контейнер внедрения зависимостей компактную, расширяемую с дополнительной поддержки и типа.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-195">Unity is a lightweight, extensible dependency injection container with optional support for instance and type interception.</span></span> <span data-ttu-id="e7cd8-196">Он является универсальным контейнером для использования в любом приложении .NET.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-196">It is a general-purpose container for use in any type of .NET application.</span></span> <span data-ttu-id="e7cd8-197">Он предоставляет общие функции, найден в том числе механизмах внедрения зависимостей: создание объектов, абстракции требований путем указания зависимости во время выполнения и гибкость, откладывая конфигурации компонента в контейнер.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-197">It provides all the common features found in dependency injection mechanisms including: object creation, abstraction of requirements by specifying dependencies at runtime and flexibility, by deferring the component configuration to the container.</span></span>


1. <span data-ttu-id="e7cd8-198">Установка **Unity.Mvc3** пакет NuGet в **MvcMusicStore** проекта.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-198">Install **Unity.Mvc3** NuGet Package in the **MvcMusicStore** project.</span></span> <span data-ttu-id="e7cd8-199">Чтобы сделать это, откройте **консоль диспетчера пакетов** из **представление** | **другие окна**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-199">To do this, open the **Package Manager Console** from **View** | **Other Windows**.</span></span>
2. <span data-ttu-id="e7cd8-200">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="e7cd8-200">Run the following command.</span></span>

    <span data-ttu-id="e7cd8-201">PMC</span><span class="sxs-lookup"><span data-stu-id="e7cd8-201">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    <span data-ttu-id="e7cd8-202">![Установка пакета NuGet Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "Установка пакета NuGet Unity.Mvc3")</span><span class="sxs-lookup"><span data-stu-id="e7cd8-202">![Installing Unity.Mvc3 NuGet Package](aspnet-mvc-4-dependency-injection/_static/image4.png "Installing Unity.Mvc3 NuGet Package")</span></span>

    <span data-ttu-id="e7cd8-203">*Установка пакета NuGet Unity.Mvc3*</span><span class="sxs-lookup"><span data-stu-id="e7cd8-203">*Installing Unity.Mvc3 NuGet Package*</span></span>
3. <span data-ttu-id="e7cd8-204">Один раз **Unity.Mvc3** пакет установлен, просмотрите файлы и папки, автоматически добавляет для упрощения конфигурации Unity.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-204">Once the **Unity.Mvc3** package is installed, explore the files and folders it automatically adds in order to simplify Unity configuration.</span></span>

    <span data-ttu-id="e7cd8-205">![Установлен пакет Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 пакета")</span><span class="sxs-lookup"><span data-stu-id="e7cd8-205">![Unity.Mvc3 package installed](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 package installed")</span></span>

    <span data-ttu-id="e7cd8-206">*Установлен пакет Unity.Mvc3*</span><span class="sxs-lookup"><span data-stu-id="e7cd8-206">*Unity.Mvc3 package installed*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a><span data-ttu-id="e7cd8-207">Задача 3 - регистрация Unity в приложении Global.asax.cs\_запуск</span><span class="sxs-lookup"><span data-stu-id="e7cd8-207">Task 3 - Registering Unity in Global.asax.cs Application\_Start</span></span>

<span data-ttu-id="e7cd8-208">В этой задаче будет обновлена **приложения\_запустить** метод находится в **Global.asax.cs** вызов инициализатора загрузчика Unity и затем обновить регистрации файла загрузчика Службы и контроллер используется для внедрения зависимости.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-208">In this task, you will update the **Application\_Start** method located in **Global.asax.cs** to call the Unity Bootstrapper initializer and then, update the Bootstrapper file registering the Service and Controller you will use for Dependency Injection.</span></span>

1. <span data-ttu-id="e7cd8-209">Теперь будет подключать загрузчика, хранящийся в файле, который инициализирует контейнера Unity и сопоставителя зависимостей.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-209">Now, you will hook up the Bootstrapper which is the file that initializes the Unity container and Dependency Resolver.</span></span> <span data-ttu-id="e7cd8-210">Чтобы сделать это, откройте **Global.asax.cs** и добавьте следующий выделенный код в **приложения\_запустить** метод.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-210">To do this, open **Global.asax.cs** and add the following highlighted code within the **Application\_Start** method.</span></span>

    <span data-ttu-id="e7cd8-211">(Фрагмент - кода *Unity инициализировать лаборатории внедрения зависимостей ASP.NET - Ex01 -*)</span><span class="sxs-lookup"><span data-stu-id="e7cd8-211">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Initialize Unity*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. <span data-ttu-id="e7cd8-212">Откройте **Bootstrapper.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-212">Open **Bootstrapper.cs** file.</span></span>
3. <span data-ttu-id="e7cd8-213">Включите следующие пространства имен: **MvcMusicStore.Services** и **MusicStore.Controllers**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-213">Include the following namespaces: **MvcMusicStore.Services** and **MusicStore.Controllers**.</span></span>

    <span data-ttu-id="e7cd8-214">(Фрагмент - кода *ASP.NET внедрения зависимостей лаборатории - Ex01 - загрузчика добавление пространств имен*)</span><span class="sxs-lookup"><span data-stu-id="e7cd8-214">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. <span data-ttu-id="e7cd8-215">Замените **BuildUnityContainer** метод содержимого со следующим кодом, который регистрирует контроллера хранилища и службы хранилища.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-215">Replace **BuildUnityContainer** method's content with the following code that registers Store Controller and Store Service.</span></span>

    <span data-ttu-id="e7cd8-216">(Фрагмент - кода *ASP.NET зависимостей внедрения лаборатории - Ex01 - хранилище регистра контроллера и службы*)</span><span class="sxs-lookup"><span data-stu-id="e7cd8-216">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller and Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="e7cd8-217">Задача 4. запуск приложения</span><span class="sxs-lookup"><span data-stu-id="e7cd8-217">Task 4 - Running the Application</span></span>

<span data-ttu-id="e7cd8-218">В этой задаче будет выполняться приложение, чтобы убедиться, что теперь загрузки после включения Unity.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-218">In this task, you will run the application to verify that it can now be loaded after including Unity.</span></span>

1. <span data-ttu-id="e7cd8-219">Нажмите клавишу **F5** для запуска приложения, приложение теперь следует загрузить без отображения сообщения об ошибке.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-219">Press **F5** to run the application, the application should now load without showing any error message.</span></span>

    <span data-ttu-id="e7cd8-220">![Запуск приложения с помощью внедрения зависимости](aspnet-mvc-4-dependency-injection/_static/image6.png "запуск приложения с помощью внедрения зависимости")</span><span class="sxs-lookup"><span data-stu-id="e7cd8-220">![Running Application with Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Running Application with Dependency Injection")</span></span>

    <span data-ttu-id="e7cd8-221">*Запуск приложения с помощью внедрения зависимости*</span><span class="sxs-lookup"><span data-stu-id="e7cd8-221">*Running Application with Dependency Injection*</span></span>
2. <span data-ttu-id="e7cd8-222">Перейдите к **или сохранений**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-222">Browse to **/Store**.</span></span> <span data-ttu-id="e7cd8-223">Это вызовет **StoreController**, который создается с помощью **Unity**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-223">This will invoke **StoreController**, which is now created using **Unity**.</span></span>

    <span data-ttu-id="e7cd8-224">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span><span class="sxs-lookup"><span data-stu-id="e7cd8-224">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span></span>

    <span data-ttu-id="e7cd8-225">*MVC Music Store*</span><span class="sxs-lookup"><span data-stu-id="e7cd8-225">*MVC Music Store*</span></span>
3. <span data-ttu-id="e7cd8-226">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-226">Close the browser.</span></span>

<span data-ttu-id="e7cd8-227">В следующих упражнениях вы узнаете, как расширить область внедрения зависимостей, чтобы использовать его внутри представления ASP.NET MVC и фильтры действий.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-227">In the following exercises you will learn how to extend the Dependency Injection scope to use it inside ASP.NET MVC Views and Action Filters.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a><span data-ttu-id="e7cd8-228">Упражнение 2: Добавление представления</span><span class="sxs-lookup"><span data-stu-id="e7cd8-228">Exercise 2: Injecting a View</span></span>

<span data-ttu-id="e7cd8-229">В этом упражнении вы узнаете, как использовать внедрение зависимостей в представлении с новыми функциями ASP.NET MVC 4 для интеграции Unity.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-229">In this exercise, you will learn how to use Dependency Injection in a view with the new features of ASP.NET MVC 4 for Unity integration.</span></span> <span data-ttu-id="e7cd8-230">Чтобы сделать это, будет вызываться пользовательская служба внутри представления обзор хранилища, который будет отображать сообщение и образ ниже.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-230">In order to do that, you will call a custom service inside the Store Browse View, which will show a message and an image below.</span></span>

<span data-ttu-id="e7cd8-231">Затем будет интегрировать проекта Unity и создать пользовательскую зависимость Сопоставитель для внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-231">Then, you will integrate the project with Unity and create a custom dependency resolver to inject the dependencies.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a><span data-ttu-id="e7cd8-232">Задача 1. Создание представления, использующее службу</span><span class="sxs-lookup"><span data-stu-id="e7cd8-232">Task 1 - Creating a View that Consumes a Service</span></span>

<span data-ttu-id="e7cd8-233">В этой задаче вы создадите представление, которое выполняет вызов службы для создания новых зависимостей.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-233">In this task, you will create a view that performs a service call to generate a new dependency.</span></span> <span data-ttu-id="e7cd8-234">Служба состоит в простой службы обмена сообщениями, включенные в это решение.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-234">The service consists in a simple messaging service included in this solution.</span></span>

1. <span data-ttu-id="e7cd8-235">Откройте **начать** решение находится в **вводится Source\Ex02 View\Begin** папки.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-235">Open the **Begin** solution located in the **Source\Ex02-Injecting View\Begin** folder.</span></span> <span data-ttu-id="e7cd8-236">В противном случае можно продолжить использование **окончания** решения получен путем выполнения в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-236">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="e7cd8-237">Если вы открыли указанных **начать** решение, необходимо будет загрузить несколько отсутствующих пакетов NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-237">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="e7cd8-238">Чтобы сделать это, нажмите кнопку **проекта** и выбрать пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-238">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="e7cd8-239">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-239">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="e7cd8-240">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-240">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e7cd8-241">Одним из преимуществ использования NuGet является у вас не указан для отправки всех библиотек в вашем проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-241">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="e7cd8-242">С помощью NuGet Power Tools указав версий пакета в файле Packages.config можно для загрузки всех необходимых библиотек первый раз, при запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-242">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="e7cd8-243">Вот почему необходимо будет выполнить эти шаги после открытия существующего решения в этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-243">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
    > 
    > <span data-ttu-id="e7cd8-244">Дополнительные сведения см. в разделе этой статьи: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="e7cd8-244">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="e7cd8-245">Включить **MessageService.cs** и **IMessageService.cs** классы находятся в **источника \Assets** папки в **/службы**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-245">Include the **MessageService.cs** and the **IMessageService.cs** classes located in the **Source \Assets** folder in **/Services**.</span></span> <span data-ttu-id="e7cd8-246">Для этого щелкните правой кнопкой мыши **службы** папку и выберите **Добавление существующего элемента**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-246">To do this, right-click **Services** folder and select **Add Existing Item**.</span></span> <span data-ttu-id="e7cd8-247">Перейдите в расположение файлов и включить их.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-247">Browse to the files' location and include them.</span></span>

    <span data-ttu-id="e7cd8-248">![Добавление службы сообщений и интерфейс службы](aspnet-mvc-4-dependency-injection/_static/image8.png "Добавление службы сообщений и интерфейс службы")</span><span class="sxs-lookup"><span data-stu-id="e7cd8-248">![Adding Message Service and Service Interface](aspnet-mvc-4-dependency-injection/_static/image8.png "Adding Message Service and Service Interface")</span></span>

    <span data-ttu-id="e7cd8-249">*Добавление службы сообщений и интерфейс службы*</span><span class="sxs-lookup"><span data-stu-id="e7cd8-249">*Adding Message Service and Service Interface*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e7cd8-250">**IMessageService** интерфейс определяет два свойства, реализуемый **MessageService** класса.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-250">The **IMessageService** interface defines two properties implemented by the **MessageService** class.</span></span> <span data-ttu-id="e7cd8-251">Эти свойства -**сообщение** и **ImageUrl**-сохранить сообщение и URL-адрес изображения для отображения.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-251">These properties -**Message** and **ImageUrl**- store the message and the URL of the image to be displayed.</span></span>
3. <span data-ttu-id="e7cd8-252">Создать папку **/страницы** в проекте корневой папки, а затем добавьте существующий класс **MyBasePage.cs** из **Source\Assets**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-252">Create the folder **/Pages** in the project's root folder, and then add the existing class **MyBasePage.cs** from **Source\Assets**.</span></span> <span data-ttu-id="e7cd8-253">Базовой страницы, который будет наследовать имеет следующую структуру.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-253">The base page you will inherit from has the following structure.</span></span>

    <span data-ttu-id="e7cd8-254">![Папка страниц](aspnet-mvc-4-dependency-injection/_static/image9.png "папок")</span><span class="sxs-lookup"><span data-stu-id="e7cd8-254">![Pages folder](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages folder")</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. <span data-ttu-id="e7cd8-255">Откройте **Browse.cshtml** просмотра из **/представления/Store** папки и сделать его наследуют **MyBasePage.cs**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-255">Open **Browse.cshtml** view from **/Views/Store** folder, and make it inherit from **MyBasePage.cs**.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. <span data-ttu-id="e7cd8-256">В **Обзор** Просмотр, добавьте вызов **MessageService** для отображения изображения и сообщения, полученные службой.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-256">In the **Browse** view, add a call to **MessageService** to display an image and a message retrieved by the service.</span></span>
<span data-ttu-id="e7cd8-257">(C#)</span><span class="sxs-lookup"><span data-stu-id="e7cd8-257">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a><span data-ttu-id="e7cd8-258">Задача 2 - включая Сопоставитель пользовательскую зависимость и активатор страницы представления</span><span class="sxs-lookup"><span data-stu-id="e7cd8-258">Task 2 - Including a Custom Dependency Resolver and a Custom View Page Activator</span></span>

<span data-ttu-id="e7cd8-259">В предыдущей задаче введенный новой зависимости внутри представления для выполнения вызова служб внутри него.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-259">In the previous task, you injected a new dependency inside a view to perform a service call inside it.</span></span> <span data-ttu-id="e7cd8-260">Теперь будет разрешить зависимость путем реализации интерфейсов внедрения зависимостей MVC ASP.NET **IViewPageActivator** и **IDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-260">Now, you will resolve that dependency by implementing the ASP.NET MVC Dependency Injection interfaces **IViewPageActivator** and **IDependencyResolver**.</span></span> <span data-ttu-id="e7cd8-261">В решение будет включать реализацию **IDependencyResolver** , будет работать с Получение службы с помощью Unity.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-261">You will include in the solution an implementation of **IDependencyResolver** that will deal with the service retrieval by using Unity.</span></span> <span data-ttu-id="e7cd8-262">Затем следует включить другую пользовательскую реализацию **IViewPageActivator** интерфейс, который предстоит решить создания представлений.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-262">Then, you will include another custom implementation of **IViewPageActivator** interface that will solve the creation of the views.</span></span>

> [!NOTE]
> <span data-ttu-id="e7cd8-263">С момента ASP.NET MVC 3 реализацию для внедрения зависимостей упрощен интерфейсы для регистрации службы.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-263">Since ASP.NET MVC 3, the implementation for Dependency Injection had simplified the interfaces to register services.</span></span> <span data-ttu-id="e7cd8-264">**IDependencyResolver** и **IViewPageActivator** являются частью функции ASP.NET MVC 3 для внедрения зависимости.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-264">**IDependencyResolver** and **IViewPageActivator** are part of ASP.NET MVC 3 features for Dependency Injection.</span></span>
> 
> <span data-ttu-id="e7cd8-265">**-IDependencyResolver** интерфейс заменяет предыдущие IMvcServiceLocator.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-265">**- IDependencyResolver** interface replaces the previous IMvcServiceLocator.</span></span> <span data-ttu-id="e7cd8-266">Объекты, реализующие IDependencyResolver должен возвращать экземпляр службы или службы коллекции.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-266">Implementers of IDependencyResolver must return an instance of the service or a service collection.</span></span>
>
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> <span data-ttu-id="e7cd8-267">**-IViewPageActivator** интерфейс обеспечивает более точное управление как просмотр страниц создаются через внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-267">**- IViewPageActivator** interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="e7cd8-268">Классы, реализующие **IViewPageActivator** интерфейса можно создавать экземпляры представление, используя сведения о контексте.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-268">The classes that implement **IViewPageActivator** interface can create view instances using context information.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. <span data-ttu-id="e7cd8-269">Создать /**фабрик** папку в корневой папке проекта.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-269">Create the /**Factories** folder in the project's root folder.</span></span>
2. <span data-ttu-id="e7cd8-270">Включить **CustomViewPageActivator.cs** решение из **/источники/активы/** для **фабрик** папки.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-270">Include **CustomViewPageActivator.cs** to your solution from **/Sources/Assets/** to **Factories** folder.</span></span> <span data-ttu-id="e7cd8-271">Чтобы сделать это, щелкните правой кнопкой мыши **/Factories** выберите **добавить | Существующий элемент** , а затем выберите **CustomViewPageActivator.cs**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-271">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **CustomViewPageActivator.cs**.</span></span> <span data-ttu-id="e7cd8-272">Этот класс реализует **IViewPageActivator** интерфейс для хранения контейнера Unity.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-272">This class implements the **IViewPageActivator** interface to hold the Unity Container.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="e7cd8-273">**CustomViewPageActivator** отвечает за управление созданием представления с помощью контейнера Unity.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-273">**CustomViewPageActivator** is responsible for managing the creation of a view by using a Unity container.</span></span>
3. <span data-ttu-id="e7cd8-274">Включить **UnityDependencyResolver.cs** файл из **/источники/активы** для **/Factories** папки.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-274">Include **UnityDependencyResolver.cs** file from **/Sources/Assets** to **/Factories** folder.</span></span> <span data-ttu-id="e7cd8-275">Чтобы сделать это, щелкните правой кнопкой мыши **/Factories** выберите **добавить | Существующий элемент** , а затем выберите **UnityDependencyResolver.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-275">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **UnityDependencyResolver.cs** file.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="e7cd8-276">**UnityDependencyResolver** класс является пользовательской DependencyResolver для Unity.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-276">**UnityDependencyResolver** class is a custom DependencyResolver for Unity.</span></span> <span data-ttu-id="e7cd8-277">Если службу не удается найти внутри контейнера Unity, является invocated базового сопоставителя.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-277">When a service cannot be found inside the Unity container, the base resolver is invocated.</span></span>

<span data-ttu-id="e7cd8-278">В следующей задаче будет зарегистрирован обе реализации для модели известно расположение службы и представления.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-278">In the following task both implementations will be registered to let the model know the location of the services and the views.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a><span data-ttu-id="e7cd8-279">Задача 3. Регистрация для внедрения зависимости в пределах контейнера Unity</span><span class="sxs-lookup"><span data-stu-id="e7cd8-279">Task 3 - Registering for Dependency Injection within Unity container</span></span>

<span data-ttu-id="e7cd8-280">В этой задаче будет помещено все предыдущие о вместе, чтобы сделать работать внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-280">In this task, you will put all the previous things together to make Dependency Injection work.</span></span>

<span data-ttu-id="e7cd8-281">До сих решения содержит следующие элементы:</span><span class="sxs-lookup"><span data-stu-id="e7cd8-281">Up to now your solution has the following elements:</span></span>

- <span data-ttu-id="e7cd8-282">Объект **Обзор** представление, которое наследует от **MyBaseClass с помощью модификатора** и использует **MessageService**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-282">A **Browse** View that inherits from **MyBaseClass** and consumes **MessageService**.</span></span>
- <span data-ttu-id="e7cd8-283">Промежуточный класс -**MyBaseClass с помощью модификатора**-с внедрения зависимостей, объявленным для интерфейса службы.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-283">An intermediate class -**MyBaseClass**- that has dependency injection declared for the service interface.</span></span>
- <span data-ttu-id="e7cd8-284">Службе - **MessageService** - и интерфейс **IMessageService**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-284">A service - **MessageService** - and its interface **IMessageService**.</span></span>
- <span data-ttu-id="e7cd8-285">Сопоставитель пользовательскую зависимость для Unity - **UnityDependencyResolver** -, реагирует на получение службы.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-285">A custom dependency resolver for Unity - **UnityDependencyResolver** - that deals with service retrieval.</span></span>
- <span data-ttu-id="e7cd8-286">Активатор страницы представления - **CustomViewPageActivator** -, создающее страницу.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-286">A View Page activator - **CustomViewPageActivator** - that creates the page.</span></span>

<span data-ttu-id="e7cd8-287">Для вставки **Обзор** представления, теперь зарегистрируйте Сопоставитель пользовательскую зависимость в контейнере Unity.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-287">To inject **Browse** View, you will now register the custom dependency resolver in the Unity container.</span></span>

1. <span data-ttu-id="e7cd8-288">Откройте **Bootstrapper.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-288">Open **Bootstrapper.cs** file.</span></span>
2. <span data-ttu-id="e7cd8-289">Регистрация экземпляра **MessageService** в Unity контейнер для инициализации службы:</span><span class="sxs-lookup"><span data-stu-id="e7cd8-289">Register an instance of **MessageService** into the Unity container to initialize the service:</span></span>

    <span data-ttu-id="e7cd8-290">(Фрагмент - кода *службу сообщение Register лаборатории - Ex02 - внедрения зависимостей ASP.NET*)</span><span class="sxs-lookup"><span data-stu-id="e7cd8-290">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. <span data-ttu-id="e7cd8-291">Добавьте ссылку на **MvcMusicStore.Factories** пространства имен.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-291">Add a reference to **MvcMusicStore.Factories** namespace.</span></span>

    <span data-ttu-id="e7cd8-292">(Фрагмент - кода *пространство имен фабрики лаборатории - Ex02 - внедрения зависимостей ASP.NET*)</span><span class="sxs-lookup"><span data-stu-id="e7cd8-292">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Factories Namespace*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. <span data-ttu-id="e7cd8-293">Зарегистрировать **CustomViewPageActivator** как активатор страницы представления в Unity контейнер:</span><span class="sxs-lookup"><span data-stu-id="e7cd8-293">Register **CustomViewPageActivator** as a View Page activator into the Unity container:</span></span>

    <span data-ttu-id="e7cd8-294">(Фрагмент - кода *ASP.NET зависимостей внедрения лаборатории - Ex02 - Register CustomViewPageActivator*)</span><span class="sxs-lookup"><span data-stu-id="e7cd8-294">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. <span data-ttu-id="e7cd8-295">Замените экземпляр по умолчанию сопоставителем зависимостей ASP.NET MVC 4 **UnityDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-295">Replace ASP.NET MVC 4 default dependency resolver with an instance of **UnityDependencyResolver**.</span></span> <span data-ttu-id="e7cd8-296">Для этого замените **Initialise** метод содержимого с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="e7cd8-296">To do this, replace **Initialise** method content with the following code:</span></span>

    <span data-ttu-id="e7cd8-297">(Фрагмент - кода *ASP.NET зависимостей внедрения лаборатории - Ex02 - обновления Сопоставитель зависимостей*)</span><span class="sxs-lookup"><span data-stu-id="e7cd8-297">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Update Dependency Resolver*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="e7cd8-298">ASP.NET MVC предоставляет класс Сопоставитель зависимостей по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-298">ASP.NET MVC provides a default dependency resolver class.</span></span> <span data-ttu-id="e7cd8-299">Для работы с распознаватели пользовательскую зависимость, которая создана unity, должно быть заменено этим распознавателем.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-299">To work with custom dependency resolvers as the one we have created for unity, this resolver has to be replaced.</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="e7cd8-300">Задача 4. запуск приложения</span><span class="sxs-lookup"><span data-stu-id="e7cd8-300">Task 4 - Running the Application</span></span>

<span data-ttu-id="e7cd8-301">В этой задаче будет выполняться приложение, чтобы убедиться, что обозреватель хранилища использует службу и показывает образа и получить сообщение:</span><span class="sxs-lookup"><span data-stu-id="e7cd8-301">In this task, you will run the application to verify that the Store Browser consumes the service and shows the image and the message retrieved:</span></span>

1. <span data-ttu-id="e7cd8-302">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-302">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="e7cd8-303">Нажмите кнопку **рок** в меню жанров и см. в разделе как **MessageService** подставленный к представлению и загрузке приветственное сообщение и изображения.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-303">Click **Rock** within the Genres Menu and see how the **MessageService** was injected to the view and loaded the welcome message and the image.</span></span> <span data-ttu-id="e7cd8-304">В этом примере вводится для &quot; **рок**&quot;:</span><span class="sxs-lookup"><span data-stu-id="e7cd8-304">In this example, we are entering to &quot;**Rock**&quot;:</span></span>

    <span data-ttu-id="e7cd8-305">![Внедрение представления MVC Music Store -](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - представление внедрения")</span><span class="sxs-lookup"><span data-stu-id="e7cd8-305">![MVC Music Store - View Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - View Injection")</span></span>

    <span data-ttu-id="e7cd8-306">*MVC Music Store - представление внедрения*</span><span class="sxs-lookup"><span data-stu-id="e7cd8-306">*MVC Music Store - View Injection*</span></span>
3. <span data-ttu-id="e7cd8-307">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-307">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a><span data-ttu-id="e7cd8-308">Упражнение 3: Добавление фильтров действий</span><span class="sxs-lookup"><span data-stu-id="e7cd8-308">Exercise 3: Injecting Action Filters</span></span>

<span data-ttu-id="e7cd8-309">В предыдущей практической **настраиваемые фильтры действий** вы работали с путем внедрения кода и настройки фильтров.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-309">In the previous Hands-On lab **Custom Action Filters** you have worked with filters customization and injection.</span></span> <span data-ttu-id="e7cd8-310">В этом упражнении вы узнаете, как ввести фильтры с внедрения зависимостей с помощью контейнера Unity.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-310">In this exercise, you will learn how to inject filters with Dependency Injection by using the Unity container.</span></span> <span data-ttu-id="e7cd8-311">Чтобы сделать это, вы добавите в решение Music Store пользовательского фильтра действий, будет отслеживать действия узла.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-311">To do that, you will add to the Music Store solution a custom action filter that will trace the activity of the site.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a><span data-ttu-id="e7cd8-312">Задача 1 - включение фильтра отслеживания в решении</span><span class="sxs-lookup"><span data-stu-id="e7cd8-312">Task 1 - Including the Tracking Filter in the Solution</span></span>

<span data-ttu-id="e7cd8-313">В этой задаче будут включены в Music Store пользовательского фильтра действий для событий трассировки.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-313">In this task, you will include in the Music Store a custom action filter to trace events.</span></span> <span data-ttu-id="e7cd8-314">Основные понятия как пользовательского фильтра действий, всегда обрабатываются в предыдущем лаборатории &quot;пользовательских фильтров действий&quot;, необходимо просто добавить класс фильтра из папки Assets этой лабораторной и затем создать поставщик фильтра для Unity:</span><span class="sxs-lookup"><span data-stu-id="e7cd8-314">As custom action filter concepts are already treated in the previous Lab &quot;Custom Action Filters&quot;, you will just include the filter class from the Assets folder of this lab, and then create a Filter Provider for Unity:</span></span>

1. <span data-ttu-id="e7cd8-315">Откройте **начать** решение находится в **Source\Ex03 - Filter\Begin действие вводится** папки.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-315">Open the **Begin** solution located in the **Source\Ex03 - Injecting Action Filter\Begin** folder.</span></span> <span data-ttu-id="e7cd8-316">В противном случае можно продолжить использование **окончания** решения получен путем выполнения в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-316">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="e7cd8-317">Если вы открыли указанных **начать** решение, необходимо будет загрузить несколько отсутствующих пакетов NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-317">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="e7cd8-318">Чтобы сделать это, нажмите кнопку **проекта** и выбрать пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-318">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="e7cd8-319">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-319">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="e7cd8-320">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-320">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e7cd8-321">Одним из преимуществ использования NuGet является у вас не указан для отправки всех библиотек в вашем проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-321">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="e7cd8-322">С помощью NuGet Power Tools указав версий пакета в файле Packages.config можно для загрузки всех необходимых библиотек первый раз, при запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-322">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="e7cd8-323">Вот почему необходимо будет выполнить эти шаги после открытия существующего решения в этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-323">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
    > 
    > <span data-ttu-id="e7cd8-324">Дополнительные сведения см. в разделе этой статьи: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="e7cd8-324">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="e7cd8-325">Включить **TraceActionFilter.cs** файл из **/источники/активы** для **/фильтрует** папки.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-325">Include **TraceActionFilter.cs** file from **/Sources/Assets** to **/Filters** folder.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > <span data-ttu-id="e7cd8-326">Этот фильтр настраиваемое действие выполняет трассировку ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-326">This custom action filter performs ASP.NET tracing.</span></span> <span data-ttu-id="e7cd8-327">Вы можете проверить &quot;локальный ASP.NET MVC 4 и динамические фильтры действий&quot; лаборатории для получения дополнительной справки.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-327">You can check &quot;ASP.NET MVC 4 local and Dynamic Action Filters&quot; Lab for more reference.</span></span>
3. <span data-ttu-id="e7cd8-328">Добавление пустой класс **FilterProvider.cs** в проект в папке **или фильтры.**</span><span class="sxs-lookup"><span data-stu-id="e7cd8-328">Add the empty class **FilterProvider.cs** to the project in the folder **/Filters.**</span></span>
4. <span data-ttu-id="e7cd8-329">Добавить **System.Web.Mvc** и **Microsoft.Practices.Unity** пространства имен в **FilterProvider.cs**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-329">Add the **System.Web.Mvc** and **Microsoft.Practices.Unity** namespaces in **FilterProvider.cs**.</span></span>

    <span data-ttu-id="e7cd8-330">(Фрагмент - кода *ASP.NET зависимостей внедрения лаборатории - Ex03 - фильтр поставщика добавление пространств имен*)</span><span class="sxs-lookup"><span data-stu-id="e7cd8-330">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. <span data-ttu-id="e7cd8-331">Сделайте класс наследовал от **IFilterProvider** интерфейса.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-331">Make the class inherit from **IFilterProvider** Interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. <span data-ttu-id="e7cd8-332">Добавить **IUnityContainer** свойство в **FilterProvider** класса, а затем создать конструктор класса, чтобы назначить контейнера.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-332">Add a **IUnityContainer** property in the **FilterProvider** class, and then create a class constructor to assign the container.</span></span>

    <span data-ttu-id="e7cd8-333">(Фрагмент - кода *ASP.NET зависимостей внедрения лаборатории - Ex03 - фильтра Конструктор поставщика*)</span><span class="sxs-lookup"><span data-stu-id="e7cd8-333">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Constructor*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="e7cd8-334">Конструктор класса поставщика фильтра не создавая **новый** внутри объекта.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-334">The filter provider class constructor is not creating a **new** object inside.</span></span> <span data-ttu-id="e7cd8-335">Контейнера передается в качестве параметра, а зависимость решается путем Unity.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-335">The container is passed as a parameter, and the dependency is solved by Unity.</span></span>
7. <span data-ttu-id="e7cd8-336">В **FilterProvider** , следует реализовать метод **GetFilters** из **IFilterProvider** интерфейса.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-336">In the **FilterProvider** class, implement the method **GetFilters** from **IFilterProvider** interface.</span></span>

    <span data-ttu-id="e7cd8-337">(Фрагмент - кода *ASP.NET зависимостей внедрения лаборатории - Ex03 - фильтр поставщика GetFilters*)</span><span class="sxs-lookup"><span data-stu-id="e7cd8-337">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider GetFilters*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a><span data-ttu-id="e7cd8-338">Задача 2 - регистрация и включение фильтра</span><span class="sxs-lookup"><span data-stu-id="e7cd8-338">Task 2 - Registering and Enabling the Filter</span></span>

<span data-ttu-id="e7cd8-339">В этой задаче будет включить отслеживание сайта.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-339">In this task, you will enable site tracking.</span></span> <span data-ttu-id="e7cd8-340">Для этого потребуется зарегистрировать фильтр в **Bootstrapper.cs BuildUnityContainer** метода для запуска трассировки:</span><span class="sxs-lookup"><span data-stu-id="e7cd8-340">To do that, you will register the filter in **Bootstrapper.cs BuildUnityContainer** method to start tracing:</span></span>

1. <span data-ttu-id="e7cd8-341">Откройте **Web.config** находится в корневой каталог проекта, а также включение трассировки отслеживания в группе System.Web.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-341">Open **Web.config** located in the project root and enable trace tracking at System.Web group.</span></span>

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. <span data-ttu-id="e7cd8-342">Откройте **Bootstrapper.cs** корне проекта.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-342">Open **Bootstrapper.cs** at project root.</span></span>
3. <span data-ttu-id="e7cd8-343">Добавьте ссылку на **MvcMusicStore.Filters** пространства имен.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-343">Add a reference to the **MvcMusicStore.Filters** namespace.</span></span>

    <span data-ttu-id="e7cd8-344">(Фрагмент - кода *ASP.NET внедрения зависимостей лаборатории - Ex03 - загрузчика добавление пространств имен*)</span><span class="sxs-lookup"><span data-stu-id="e7cd8-344">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. <span data-ttu-id="e7cd8-345">Выберите **BuildUnityContainer** метод и зарегистрировать фильтр в контейнере Unity.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-345">Select the **BuildUnityContainer** method and register the filter in the Unity Container.</span></span> <span data-ttu-id="e7cd8-346">Необходимо зарегистрировать поставщик фильтра, а также действие фильтра.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-346">You will have to register the filter provider as well as the action filter.</span></span>

    <span data-ttu-id="e7cd8-347">(Фрагмент - кода *ASP.NET зависимостей внедрения лаборатории - Ex03 - Register FilterProvider и ActionFilter*)</span><span class="sxs-lookup"><span data-stu-id="e7cd8-347">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider and ActionFilter*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="e7cd8-348">Задача 3. запуск приложения</span><span class="sxs-lookup"><span data-stu-id="e7cd8-348">Task 3 - Running the Application</span></span>

<span data-ttu-id="e7cd8-349">В этой задаче будет запустите приложение и проверить, что пользовательского фильтра действий трассировка действия:</span><span class="sxs-lookup"><span data-stu-id="e7cd8-349">In this task, you will run the application and test that the custom action filter is tracing the activity:</span></span>

1. <span data-ttu-id="e7cd8-350">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-350">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="e7cd8-351">Нажмите кнопку **рок** жанров меню.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-351">Click **Rock** within the Genres Menu.</span></span> <span data-ttu-id="e7cd8-352">Можно указать несколько жанров, если вы хотите.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-352">You can browse to more genres if you want to.</span></span>

    <span data-ttu-id="e7cd8-353">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span><span class="sxs-lookup"><span data-stu-id="e7cd8-353">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span></span>

    <span data-ttu-id="e7cd8-354">*Music Store*</span><span class="sxs-lookup"><span data-stu-id="e7cd8-354">*Music Store*</span></span>
3. <span data-ttu-id="e7cd8-355">Перейдите к **/Trace.axd** для просмотра трассировки приложения и нажмите кнопку **Просмотр сведений о**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-355">Browse to **/Trace.axd** to see the Application Trace page, and then click **View Details**.</span></span>

    <span data-ttu-id="e7cd8-356">![Журнал трассировки приложения](aspnet-mvc-4-dependency-injection/_static/image12.png "журнал трассировки приложения")</span><span class="sxs-lookup"><span data-stu-id="e7cd8-356">![Application Trace Log](aspnet-mvc-4-dependency-injection/_static/image12.png "Application Trace Log")</span></span>

    <span data-ttu-id="e7cd8-357">*Журнал трассировки приложения*</span><span class="sxs-lookup"><span data-stu-id="e7cd8-357">*Application Trace Log*</span></span>

    <span data-ttu-id="e7cd8-358">![Трассировка приложения - сведения о запросе](aspnet-mvc-4-dependency-injection/_static/image13.png "трассировки приложения - сведения о запросе")</span><span class="sxs-lookup"><span data-stu-id="e7cd8-358">![Application Trace - Request Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - Request Details")</span></span>

    <span data-ttu-id="e7cd8-359">*Трассировка приложения - сведения о запросе*</span><span class="sxs-lookup"><span data-stu-id="e7cd8-359">*Application Trace - Request Details*</span></span>
4. <span data-ttu-id="e7cd8-360">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-360">Close the browser.</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="e7cd8-361">Сводка</span><span class="sxs-lookup"><span data-stu-id="e7cd8-361">Summary</span></span>

<span data-ttu-id="e7cd8-362">Выполнив это Практическое лабораторное занятие изучено используют внедрение зависимостей в ASP.NET MVC 4, интегрируя Unity с помощью пакета NuGet.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-362">By completing this Hands-On Lab you have learned how to use Dependency Injection in ASP.NET MVC 4 by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="e7cd8-363">Чтобы добиться этого, вы использовали внедрения зависимости внутри контроллеров, представлений и фильтры действий.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-363">To achieve that, you have used Dependency Injection inside controllers, views and action filters.</span></span>

<span data-ttu-id="e7cd8-364">Были освещены следующие понятия:</span><span class="sxs-lookup"><span data-stu-id="e7cd8-364">The following concepts were covered:</span></span>

- <span data-ttu-id="e7cd8-365">Возможности внедрения зависимостей ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="e7cd8-365">ASP.NET MVC 4 Dependency Injection features</span></span>
- <span data-ttu-id="e7cd8-366">Интеграция с Unity с помощью пакета NuGet Unity.Mvc3</span><span class="sxs-lookup"><span data-stu-id="e7cd8-366">Unity integration using Unity.Mvc3 NuGet Package</span></span>
- <span data-ttu-id="e7cd8-367">Внедрение зависимостей в контроллерах</span><span class="sxs-lookup"><span data-stu-id="e7cd8-367">Dependency Injection in Controllers</span></span>
- <span data-ttu-id="e7cd8-368">Внедрение зависимостей в представлениях</span><span class="sxs-lookup"><span data-stu-id="e7cd8-368">Dependency Injection in Views</span></span>
- <span data-ttu-id="e7cd8-369">Внедрение зависимостей фильтров действий</span><span class="sxs-lookup"><span data-stu-id="e7cd8-369">Dependency injection of Action Filters</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="e7cd8-370">Приложение а. Установка Visual Studio Express 2012 для Web</span><span class="sxs-lookup"><span data-stu-id="e7cd8-370">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="e7cd8-371">Можно установить **Microsoft Visual Studio Express 2012 для Web** или другой &quot;Express&quot; версию, используя  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="e7cd8-371">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="e7cd8-372">Следующие инструкции описывают действия, необходимые для установки *Visual studio Express 2012 для Web* с помощью *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-372">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="e7cd8-373">Последовательно выберите пункты [ [https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="e7cd8-373">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="e7cd8-374">Кроме того, если вы уже установили установщика веб-платформы, можно открыть его и выполните поиск продукта &quot; *Visual Studio Express 2012 для Web с пакетом Windows Azure SDK*&quot;.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-374">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="e7cd8-375">Щелкните **установить сейчас**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-375">Click on **Install Now**.</span></span> <span data-ttu-id="e7cd8-376">Если у вас **Web Platform Installer** вы будете перенаправлены, чтобы загрузить и установить ее сначала.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-376">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="e7cd8-377">Один раз **Web Platform Installer** открыто, нажмите кнопку **установить** для запуска программы установки.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-377">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="e7cd8-378">![Установка Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "установка Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="e7cd8-378">![Install Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="e7cd8-379">*Установка Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="e7cd8-379">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="e7cd8-380">Чтение всех продуктов лицензий и условия и нажмите кнопку **принимаю** для продолжения.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-380">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Принятие условий лицензионного соглашения](aspnet-mvc-4-dependency-injection/_static/image15.png)

    <span data-ttu-id="e7cd8-382">*Принятие условий лицензионного соглашения*</span><span class="sxs-lookup"><span data-stu-id="e7cd8-382">*Accepting the license terms*</span></span>
5. <span data-ttu-id="e7cd8-383">Дождитесь завершения процесса загрузки и установки.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-383">Wait until the downloading and installation process completes.</span></span>

    ![Ход установки](aspnet-mvc-4-dependency-injection/_static/image16.png)

    <span data-ttu-id="e7cd8-385">*Ход выполнения установки*</span><span class="sxs-lookup"><span data-stu-id="e7cd8-385">*Installation progress*</span></span>
6. <span data-ttu-id="e7cd8-386">По завершении установки нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-386">When the installation completes, click **Finish**.</span></span>

    ![Установка завершена](aspnet-mvc-4-dependency-injection/_static/image17.png)

    <span data-ttu-id="e7cd8-388">*Установка завершена*</span><span class="sxs-lookup"><span data-stu-id="e7cd8-388">*Installation completed*</span></span>
7. <span data-ttu-id="e7cd8-389">Нажмите кнопку **выхода** закрыть установщик веб-платформы.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-389">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="e7cd8-390">Чтобы открыть Visual Studio Express для Web, перейдите к **запустить** экрана и начинается запись &quot; **VS Express**&quot;, выберите команду **VS Express для Web** Плитка.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-390">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express для Web плитки](aspnet-mvc-4-dependency-injection/_static/image18.png)

    <span data-ttu-id="e7cd8-392">*VS Express для Web плитки*</span><span class="sxs-lookup"><span data-stu-id="e7cd8-392">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="e7cd8-393">Приложение б. фрагменты кода</span><span class="sxs-lookup"><span data-stu-id="e7cd8-393">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="e7cd8-394">С помощью фрагментов кода у вас есть весь код, который требуется под рукой.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-394">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="e7cd8-395">Документ лаборатории сообщает только при их использовании, как показано на следующем рисунке.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-395">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="e7cd8-396">![Фрагменты кода Visual Studio для вставки кода в проекте](aspnet-mvc-4-dependency-injection/_static/image19.png "фрагменты кода с помощью Visual Studio вставьте код в проект")</span><span class="sxs-lookup"><span data-stu-id="e7cd8-396">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-dependency-injection/_static/image19.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="e7cd8-397">*Фрагменты кода Visual Studio для вставки кода в проекте*</span><span class="sxs-lookup"><span data-stu-id="e7cd8-397">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="e7cd8-398">***Добавление фрагмента кода, с помощью клавиатуры (только C#)***</span><span class="sxs-lookup"><span data-stu-id="e7cd8-398">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="e7cd8-399">Поместите курсор в место вставки кода.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-399">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="e7cd8-400">Начните вводить имя фрагмента (без пробелов и дефисы).</span><span class="sxs-lookup"><span data-stu-id="e7cd8-400">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="e7cd8-401">Смотрите, как IntelliSense отображает, совпадающие с именами фрагменты.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-401">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="e7cd8-402">Выберите правильный фрагмент (или продолжите ввод, пока не будет выделен весь фрагмент имени).</span><span class="sxs-lookup"><span data-stu-id="e7cd8-402">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="e7cd8-403">Нажмите клавишу Tab дважды, чтобы вставить фрагмент в положении курсора.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-403">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="e7cd8-404">![Начните вводить имя фрагмента](aspnet-mvc-4-dependency-injection/_static/image20.png "начните вводить имя фрагмента")</span><span class="sxs-lookup"><span data-stu-id="e7cd8-404">![Start typing the snippet name](aspnet-mvc-4-dependency-injection/_static/image20.png "Start typing the snippet name")</span></span>

<span data-ttu-id="e7cd8-405">*Начните вводить имя фрагмента*</span><span class="sxs-lookup"><span data-stu-id="e7cd8-405">*Start typing the snippet name*</span></span>

<span data-ttu-id="e7cd8-406">![Нажмите клавишу Tab, чтобы выделить выделенный фрагмент](aspnet-mvc-4-dependency-injection/_static/image21.png "нажмите клавишу Tab, чтобы выделить выделенный фрагмент")</span><span class="sxs-lookup"><span data-stu-id="e7cd8-406">![Press Tab to select the highlighted snippet](aspnet-mvc-4-dependency-injection/_static/image21.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="e7cd8-407">*Нажмите клавишу Tab, чтобы выделить выделенный фрагмент*</span><span class="sxs-lookup"><span data-stu-id="e7cd8-407">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="e7cd8-408">![Снова нажмите клавишу Tab и фрагмент будет расширен](aspnet-mvc-4-dependency-injection/_static/image22.png "будет расширен, снова нажмите клавишу Tab и фрагмента кода")</span><span class="sxs-lookup"><span data-stu-id="e7cd8-408">![Press Tab again and the snippet will expand](aspnet-mvc-4-dependency-injection/_static/image22.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="e7cd8-409">*Снова нажмите клавишу Tab и фрагмент будет расширен*</span><span class="sxs-lookup"><span data-stu-id="e7cd8-409">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="e7cd8-410">***Добавление фрагмента кода, с помощью мыши (C#, Visual Basic и XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-410">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="e7cd8-411">Щелкните правой кнопкой мыши место вставки фрагмента кода.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-411">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="e7cd8-412">Выберите **вставить фрагмент** следуют **Мои фрагменты кода**.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-412">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="e7cd8-413">Выберите соответствующий фрагмент из списка, щелкнув по ней.</span><span class="sxs-lookup"><span data-stu-id="e7cd8-413">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="e7cd8-414">![Щелкните правой кнопкой мыши место для вставки фрагмента кода и выбора вставить фрагмент](aspnet-mvc-4-dependency-injection/_static/image23.png "место для вставки фрагмента кода и выбора вставить фрагмент кода щелкните правой кнопкой мыши")</span><span class="sxs-lookup"><span data-stu-id="e7cd8-414">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-dependency-injection/_static/image23.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="e7cd8-415">*Щелкните правой кнопкой мыши в место вставки фрагмента кода и выберите Вставить фрагмент*</span><span class="sxs-lookup"><span data-stu-id="e7cd8-415">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="e7cd8-416">![Выберите из списка, соответствующего фрагмента, щелкнув его](aspnet-mvc-4-dependency-injection/_static/image24.png "выбрать соответствующий фрагмент из списка, щелкнув по ней")</span><span class="sxs-lookup"><span data-stu-id="e7cd8-416">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-dependency-injection/_static/image24.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="e7cd8-417">*Выберите соответствующий фрагмент из списка, щелкнув по ней*</span><span class="sxs-lookup"><span data-stu-id="e7cd8-417">*Pick the relevant snippet from the list, by clicking on it*</span></span>
