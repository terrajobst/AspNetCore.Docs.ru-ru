---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: Внедрение зависимостей в ASP.NET MVC 4 | Документы Microsoft
author: rick-anderson
description: 'Примечание: Это Практическое лабораторное занятие предполагается наличие базовых знаний о фильтрах ASP.NET MVC и ASP.NET MVC 4. Если вы не использовали фильтров ASP.NET MVC 4 перед мы rec...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: e6c24d03039f0e6005948a73348589627c9df2df
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877662"
---
# <a name="aspnet-mvc-4-dependency-injection"></a><span data-ttu-id="c03ab-104">Внедрение зависимостей в ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="c03ab-104">ASP.NET MVC 4 Dependency Injection</span></span>

<span data-ttu-id="c03ab-105">По [Web лагеря команды](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="c03ab-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="c03ab-106">Загрузите комплект учебных материалов лагеря Web</span><span class="sxs-lookup"><span data-stu-id="c03ab-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="c03ab-107">Это Практическое лабораторное занятие предполагает иметь базовые знания об **ASP.NET MVC** и **фильтров ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC** and **ASP.NET MVC 4 filters**.</span></span> <span data-ttu-id="c03ab-108">Если вы не использовали **фильтров ASP.NET MVC 4** раньше, мы рекомендуем перейти **настраиваемые действия ASP.NET MVC фильтры** Практическое лабораторное занятие.</span><span class="sxs-lookup"><span data-stu-id="c03ab-108">If you have not used **ASP.NET MVC 4 filters** before, we recommend you to go over **ASP.NET MVC Custom Action Filters** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="c03ab-109">Все образцы кода и фрагменты кода включаются в Web лагеря комплект учебных материалов, доступные в [выпуски Microsoft Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="c03ab-109">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="c03ab-110">Проект для этой лаборатории доступен на [внедрения зависимостей ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="c03ab-110">The project specific to this lab is available at [ASP.NET MVC 4 Dependency Injection](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span></span>

<span data-ttu-id="c03ab-111">В **объектно-ориентированное программирование** парадигме, совместной работы объектов в модели совместная работа которых содержится участников и пользователей.</span><span class="sxs-lookup"><span data-stu-id="c03ab-111">In **Object Oriented Programming** paradigm, objects work together in a collaboration model where there are contributors and consumers.</span></span> <span data-ttu-id="c03ab-112">Естественно эта модель связи приводит к возникновению ошибки зависимости между объектами и компонентами, становится трудно управлять при увеличении сложности.</span><span class="sxs-lookup"><span data-stu-id="c03ab-112">Naturally, this communication model generates dependencies between objects and components, becoming difficult to manage when complexity increases.</span></span>

<span data-ttu-id="c03ab-113">![Класс зависимости и сложности модели](aspnet-mvc-4-dependency-injection/_static/image1.png "класса зависимости и сложности модели")</span><span class="sxs-lookup"><span data-stu-id="c03ab-113">![Class dependencies and model complexity](aspnet-mvc-4-dependency-injection/_static/image1.png "Class dependencies and model complexity")</span></span>

<span data-ttu-id="c03ab-114">*Класс зависимости и сложность модели*</span><span class="sxs-lookup"><span data-stu-id="c03ab-114">*Class dependencies and model complexity*</span></span>

<span data-ttu-id="c03ab-115">Возможно, вы слышали о **шаблон фабрики** и разделения между интерфейсом и реализации с помощью служб, где клиентские объекты часто отвечают за расположение службы.</span><span class="sxs-lookup"><span data-stu-id="c03ab-115">You have probably heard about the **Factory Pattern** and the separation between the interface and the implementation using services, where the client objects are often responsible for service location.</span></span>

<span data-ttu-id="c03ab-116">Шаблон внедрения зависимостей — конкретной реализации инверсии элемента управления.</span><span class="sxs-lookup"><span data-stu-id="c03ab-116">The Dependency Injection pattern is a particular implementation of Inversion of Control.</span></span> <span data-ttu-id="c03ab-117">**Инверсии управления (IoC)** означает, что объекты не создавать другие объекты, на которые они используют для выполнения своих задач.</span><span class="sxs-lookup"><span data-stu-id="c03ab-117">**Inversion of Control (IoC)** means that objects do not create other objects on which they rely to do their work.</span></span> <span data-ttu-id="c03ab-118">Вместо этого они получить объекты, которые им необходимы из внешнего источника (например, файл конфигурации xml).</span><span class="sxs-lookup"><span data-stu-id="c03ab-118">Instead, they get the objects that they need from an outside source (for example, an xml configuration file).</span></span>

<span data-ttu-id="c03ab-119">**Внедрения зависимости (DI)** означает, что это осуществляется без вмешательства объекта, обычно компонентом платформы, который передает параметры конструктора и задать свойства.</span><span class="sxs-lookup"><span data-stu-id="c03ab-119">**Dependency Injection (DI)** means that this is done without the object intervention, usually by a framework component that passes constructor parameters and set properties.</span></span>

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a><span data-ttu-id="c03ab-120">Шаблон разработки (DI) внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="c03ab-120">The Dependency Injection (DI) Design Pattern</span></span>

<span data-ttu-id="c03ab-121">На высоком уровне внедрения зависимостей предназначена, класс клиента (например *golfer*) требуется нечто, удовлетворяющий интерфейс (например *IClub*).</span><span class="sxs-lookup"><span data-stu-id="c03ab-121">At a high level, the goal of Dependency Injection is that a client class (e.g. *the golfer*) needs something that satisfies an interface (e.g. *IClub*).</span></span> <span data-ttu-id="c03ab-122">Не проверяет, имеет конкретный тип (например *WedgeClub WoodClub IronClub,* или *PutterClub*), ему кто-то другой для обработки (например хорошо *caddy*).</span><span class="sxs-lookup"><span data-stu-id="c03ab-122">It doesn't care what the concrete type is (e.g. *WoodClub, IronClub, WedgeClub* or *PutterClub*), it wants someone else to handle that (e.g. a good *caddy*).</span></span> <span data-ttu-id="c03ab-123">Сопоставитель зависимостей в ASP.NET MVC позволяет зарегистрировать где-нибудь еще логику зависимостей (например контейнера или *контейнера треф*).</span><span class="sxs-lookup"><span data-stu-id="c03ab-123">The Dependency Resolver in ASP.NET MVC can allow you to register your dependency logic somewhere else (e.g. a container or a *bag of clubs*).</span></span>

<span data-ttu-id="c03ab-124">![Диаграмма внедрения зависимостей](aspnet-mvc-4-dependency-injection/_static/image2.png "рисунке внедрения зависимости")</span><span class="sxs-lookup"><span data-stu-id="c03ab-124">![Dependency Injection diagram](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection illustration")</span></span>

<span data-ttu-id="c03ab-125">*Внедрение зависимостей - аналогию Гольф*</span><span class="sxs-lookup"><span data-stu-id="c03ab-125">*Dependency Injection - Golf analogy*</span></span>

<span data-ttu-id="c03ab-126">Ниже перечислены преимущества использования шаблона внедрения зависимостей и инверсии элемента управления.</span><span class="sxs-lookup"><span data-stu-id="c03ab-126">The advantages of using Dependency Injection pattern and Inversion of Control are the following:</span></span>

- <span data-ttu-id="c03ab-127">Уменьшает соединения классов</span><span class="sxs-lookup"><span data-stu-id="c03ab-127">Reduces class coupling</span></span>
- <span data-ttu-id="c03ab-128">Увеличивает повторное использование кода</span><span class="sxs-lookup"><span data-stu-id="c03ab-128">Increases code reusing</span></span>
- <span data-ttu-id="c03ab-129">Повышает удобство поддержки кода</span><span class="sxs-lookup"><span data-stu-id="c03ab-129">Improves code maintainability</span></span>
- <span data-ttu-id="c03ab-130">Повышает тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="c03ab-130">Improves application testing</span></span>

> [!NOTE]
> <span data-ttu-id="c03ab-131">Внедрение зависимостей, иногда сравнивается с абстрактный фабричного конструктивного шаблона, но есть небольшие различия между оба подхода.</span><span class="sxs-lookup"><span data-stu-id="c03ab-131">Dependency Injection is sometimes compared with Abstract Factory Design Pattern, but there is a slight difference between both approaches.</span></span> <span data-ttu-id="c03ab-132">DI установлена платформа Framework над за тем, чтобы устранить зависимости, для вызова фабрики и зарегистрированных служб.</span><span class="sxs-lookup"><span data-stu-id="c03ab-132">DI has a Framework working behind to solve dependencies by calling the factories and the registered services.</span></span>


<span data-ttu-id="c03ab-133">Теперь, когда вы знаете шаблон внедрения зависимостей, вы узнаете в этой лаборатории как они применяются в ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="c03ab-133">Now that you understand the Dependency Injection Pattern, you will learn throughout this lab how to apply it in ASP.NET MVC 4.</span></span> <span data-ttu-id="c03ab-134">Сначала с помощью внедрения зависимости в **контроллеров** служба доступа к базе данных.</span><span class="sxs-lookup"><span data-stu-id="c03ab-134">You will start using Dependency Injection in the **Controllers** to include a database access service.</span></span> <span data-ttu-id="c03ab-135">Затем примените внедрения зависимостей для **представления** для использования службы и отображения сведений.</span><span class="sxs-lookup"><span data-stu-id="c03ab-135">Next, you will apply Dependency Injection to the **Views** to consume a service and show information.</span></span> <span data-ttu-id="c03ab-136">Наконец можно расширить DI в ASP.NET MVC 4 фильтры, добавление пользовательского фильтра действий в решении.</span><span class="sxs-lookup"><span data-stu-id="c03ab-136">Finally, you will extend the DI to ASP.NET MVC 4 Filters, injecting a custom action filter in the solution.</span></span>

<span data-ttu-id="c03ab-137">В этой лаборатории практических вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="c03ab-137">In this Hands-on Lab, you will learn how to:</span></span>

- <span data-ttu-id="c03ab-138">Интеграция ASP.NET MVC 4 с помощью Unity для внедрения зависимости, с помощью пакетов NuGet</span><span class="sxs-lookup"><span data-stu-id="c03ab-138">Integrate ASP.NET MVC 4 with Unity for Dependency Injection using NuGet Packages</span></span>
- <span data-ttu-id="c03ab-139">Используют внедрение зависимостей внутри контроллера ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="c03ab-139">Use Dependency Injection inside an ASP.NET MVC Controller</span></span>
- <span data-ttu-id="c03ab-140">Используют внедрение зависимостей в представлении ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="c03ab-140">Use Dependency Injection inside an ASP.NET MVC View</span></span>
- <span data-ttu-id="c03ab-141">Используют внедрение зависимостей в ASP.NET MVC фильтра действий</span><span class="sxs-lookup"><span data-stu-id="c03ab-141">Use Dependency Injection inside an ASP.NET MVC Action Filter</span></span>

> [!NOTE]
> <span data-ttu-id="c03ab-142">Эта лаборатория использует Unity.Mvc3 пакет NuGet для разрешения зависимостей, но можно адаптировать любую платформу внедрения зависимостей для работы с ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="c03ab-142">This Lab is using Unity.Mvc3 NuGet Package for dependency resolution, but it is possible to adapt any Dependency Injection Framework to work with ASP.NET MVC 4.</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="c03ab-143">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="c03ab-143">Prerequisites</span></span>

<span data-ttu-id="c03ab-144">Необходимо иметь следующие элементы для этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="c03ab-144">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="c03ab-145">[Microsoft Visual Studio Express 2012 для Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) или выше (чтение [приложение A](#AppendixA) инструкции по его установке).</span><span class="sxs-lookup"><span data-stu-id="c03ab-145">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="c03ab-146">Установка</span><span class="sxs-lookup"><span data-stu-id="c03ab-146">Setup</span></span>

<span data-ttu-id="c03ab-147">**Установка фрагменты кода**</span><span class="sxs-lookup"><span data-stu-id="c03ab-147">**Installing Code Snippets**</span></span>

<span data-ttu-id="c03ab-148">Для удобства большая часть кода, который вы планируете управлять вдоль этой лаборатории доступна как фрагменты кода Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c03ab-148">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="c03ab-149">Чтобы установить фрагменты кода запуска **.\Source\Setup\CodeSnippets.vsi** файла.</span><span class="sxs-lookup"><span data-stu-id="c03ab-149">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="c03ab-150">Если вы не знакомы с фрагментов кода Visual Studio и хотите узнать о способах их использования, можно ссылаться в приложение из этого документа &quot; [с помощью фрагментов кода в приложении б](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="c03ab-150">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="c03ab-151">Упражнения</span><span class="sxs-lookup"><span data-stu-id="c03ab-151">Exercises</span></span>

<span data-ttu-id="c03ab-152">Это Практическое лабораторное занятие состоит в выполнении следующих упражнений.</span><span class="sxs-lookup"><span data-stu-id="c03ab-152">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="c03ab-153">Упражнение 1: Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="c03ab-153">Exercise 1: Injecting a Controller</span></span>](#Exercise1)
2. [<span data-ttu-id="c03ab-154">Упражнение 2: Добавление представления</span><span class="sxs-lookup"><span data-stu-id="c03ab-154">Exercise 2: Injecting a View</span></span>](#Exercise2)
3. [<span data-ttu-id="c03ab-155">Упражнение 3: Добавление фильтров</span><span class="sxs-lookup"><span data-stu-id="c03ab-155">Exercise 3: Injecting Filters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="c03ab-156">Каждого упражнения сопровождается **окончания** папку, содержащую полученное в результате решение, должен быть получен после завершения выполнения упражнений.</span><span class="sxs-lookup"><span data-stu-id="c03ab-156">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="c03ab-157">Это решение можно использовать как руководство, если вам нужна дополнительная помощь при выполнении упражнений.</span><span class="sxs-lookup"><span data-stu-id="c03ab-157">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="c03ab-158">Предполагаемое время для выполнения этого занятия: **30 минут**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-158">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a><span data-ttu-id="c03ab-159">Упражнение 1: Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="c03ab-159">Exercise 1: Injecting a Controller</span></span>

<span data-ttu-id="c03ab-160">В этом упражнении вы узнаете, как использовать внедрение зависимостей в ASP.NET MVC Controllers интегрируя Unity с помощью пакета NuGet.</span><span class="sxs-lookup"><span data-stu-id="c03ab-160">In this exercise, you will learn how to use Dependency Injection in ASP.NET MVC Controllers by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="c03ab-161">По этой причине будут включены в контроллерах MvcMusicStore для отделения логики доступа к данным служб.</span><span class="sxs-lookup"><span data-stu-id="c03ab-161">For that reason, you will include services into your MvcMusicStore controllers to separate the logic from the data access.</span></span> <span data-ttu-id="c03ab-162">В конструктор контроллера, который будет разрешен с помощью внедрения зависимости с помощью служб создается новая зависимость **Unity**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-162">The services will create a new dependency in the controller constructor, which will be resolved using Dependency Injection with the help of **Unity**.</span></span>

<span data-ttu-id="c03ab-163">Такой подход будет показано, как создать менее связанных приложений, которые являются более гибкими и простыми в обслуживании и тестировании.</span><span class="sxs-lookup"><span data-stu-id="c03ab-163">This approach will show you how to generate less coupled applications, which are more flexible and easier to maintain and test.</span></span> <span data-ttu-id="c03ab-164">Также вы узнаете, как интегрировать ASP.NET MVC с помощью Unity.</span><span class="sxs-lookup"><span data-stu-id="c03ab-164">You will also learn how to integrate ASP.NET MVC with Unity.</span></span>

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a><span data-ttu-id="c03ab-165">О службе StoreManager</span><span class="sxs-lookup"><span data-stu-id="c03ab-165">About StoreManager Service</span></span>

<span data-ttu-id="c03ab-166">MVC Music Store, предоставляемые в решении begin теперь включает службу, которая управляет контроллера хранилища данных с именем **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-166">The MVC Music Store provided in the begin solution now includes a service that manages the Store Controller data named **StoreService**.</span></span> <span data-ttu-id="c03ab-167">Ниже Вы найдете реализации службы хранилища.</span><span class="sxs-lookup"><span data-stu-id="c03ab-167">Below you will find the Store Service implementation.</span></span> <span data-ttu-id="c03ab-168">Обратите внимание, что все методы возвращают сущности модели.</span><span class="sxs-lookup"><span data-stu-id="c03ab-168">Note that all the methods return Model entities.</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

<span data-ttu-id="c03ab-169">**StoreController** из начала решение использует **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-169">**StoreController** from the begin solution now consumes **StoreService**.</span></span> <span data-ttu-id="c03ab-170">Ссылки на данные были удалены из **StoreController**и теперь можно изменить текущий поставщик доступа к данным без изменения любой метод, который использует **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-170">All the data references were removed from **StoreController**, and now possible to modify the current data access provider without changing any method that consumes **StoreService**.</span></span>

<span data-ttu-id="c03ab-171">Вы найдете ниже, **StoreController** реализация имеет зависимость с **StoreService** внутри конструктора класса.</span><span class="sxs-lookup"><span data-stu-id="c03ab-171">You will find below that the **StoreController** implementation has a dependency with **StoreService** inside the class constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="c03ab-172">Зависимости, представленные в этом упражнении, связанные с **инверсии управления** (IoC).</span><span class="sxs-lookup"><span data-stu-id="c03ab-172">The dependency introduced in this exercise is related to **Inversion of Control** (IoC).</span></span>
> 
> <span data-ttu-id="c03ab-173">**StoreController** конструктор класса принимает **IStoreService** параметру типа, который необходим для выполнения вызовов службы от внутри класса.</span><span class="sxs-lookup"><span data-stu-id="c03ab-173">The **StoreController** class constructor receives an **IStoreService** type parameter, which is essential to perform service calls from inside the class.</span></span> <span data-ttu-id="c03ab-174">Тем не менее **StoreController** не реализует конструктор по умолчанию (с без параметров), любой контроллер необходимы для работы с ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c03ab-174">However, **StoreController** does not implement the default constructor (with no parameters) that any controller must have to work with ASP.NET MVC.</span></span>
> 
> <span data-ttu-id="c03ab-175">Чтобы устранить зависимость, контроллер должна создаваться фабрикой абстрактным (класс, который возвращает любой объект указанного типа).</span><span class="sxs-lookup"><span data-stu-id="c03ab-175">To resolve the dependency, the controller has to be created by an abstract factory (a class that returns any object of the specified type).</span></span>


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="c03ab-176">Вы получите ошибку при попытке создать StoreController без отправки объекта службы, как отсутствует конструктор без параметров объявлен класс.</span><span class="sxs-lookup"><span data-stu-id="c03ab-176">You will get an error when the class tries to create the StoreController without sending the service object, as there is no parameterless constructor declared.</span></span>


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a><span data-ttu-id="c03ab-177">Задача 1. запуск приложения</span><span class="sxs-lookup"><span data-stu-id="c03ab-177">Task 1 - Running the Application</span></span>

<span data-ttu-id="c03ab-178">В этой задаче будет запускаться приложение Begin, который включает в себя контроллер хранилища, который отделяет доступа к данным от логики приложения службы.</span><span class="sxs-lookup"><span data-stu-id="c03ab-178">In this task, you will run the Begin application, which includes the service into the Store Controller that separates the data access from the application logic.</span></span>

<span data-ttu-id="c03ab-179">При запуске приложения, получит сообщение об ошибке, как служба контроллера не передается как параметр по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="c03ab-179">When running the application, you will receive an exception, as the controller service is not passed as a parameter by default:</span></span>

1. <span data-ttu-id="c03ab-180">Откройте **начать** решение находится в **вводится Source\Ex01 Controller\Begin**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-180">Open the **Begin** solution located in **Source\Ex01-Injecting Controller\Begin**.</span></span>

   1. <span data-ttu-id="c03ab-181">Необходимо загрузить несколько отсутствующих пакетов NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="c03ab-181">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="c03ab-182">Чтобы сделать это, нажмите кнопку **проекта** и выбрать пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-182">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="c03ab-183">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="c03ab-183">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="c03ab-184">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-184">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="c03ab-185">Одним из преимуществ использования NuGet является у вас не указан для отправки всех библиотек в вашем проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="c03ab-185">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="c03ab-186">С помощью NuGet Power Tools указав версий пакета в файле Packages.config можно для загрузки всех необходимых библиотек первый раз, при запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="c03ab-186">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="c03ab-187">Вот почему необходимо будет выполнить эти шаги после открытия существующего решения в этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="c03ab-187">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="c03ab-188">Нажмите клавишу **сочетание клавиш Ctrl + F5** для запуска приложения без отладки.</span><span class="sxs-lookup"><span data-stu-id="c03ab-188">Press **Ctrl + F5** to run the application without debugging.</span></span> <span data-ttu-id="c03ab-189">Вы получите сообщение об ошибке &quot; **нет конструктора без параметров, определенные для этого объекта**&quot;:</span><span class="sxs-lookup"><span data-stu-id="c03ab-189">You will get the error message &quot;**No parameterless constructor defined for this object**&quot;:</span></span>

    <span data-ttu-id="c03ab-190">![Ошибка при выполнении приложения начать ASP.NET MVC](aspnet-mvc-4-dependency-injection/_static/image3.png "ошибка во время выполнения приложения начать ASP.NET MVC")</span><span class="sxs-lookup"><span data-stu-id="c03ab-190">![Error while running ASP.NET MVC Begin Application](aspnet-mvc-4-dependency-injection/_static/image3.png "Error while running ASP.NET MVC Begin Application")</span></span>

    <span data-ttu-id="c03ab-191">*Ошибка при выполнении приложения начать ASP.NET MVC*</span><span class="sxs-lookup"><span data-stu-id="c03ab-191">*Error while running ASP.NET MVC Begin Application*</span></span>
3. <span data-ttu-id="c03ab-192">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="c03ab-192">Close the browser.</span></span>

<span data-ttu-id="c03ab-193">В следующих шагах вы будете работать решения хранилища музыки для внедрения зависимости, необходимые для этого контроллера.</span><span class="sxs-lookup"><span data-stu-id="c03ab-193">In the following steps you will work on the Music Store Solution to inject the dependency this controller needs.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a><span data-ttu-id="c03ab-194">Задача 2 - включая Unity в решение MvcMusicStore</span><span class="sxs-lookup"><span data-stu-id="c03ab-194">Task 2 - Including Unity into MvcMusicStore Solution</span></span>

<span data-ttu-id="c03ab-195">В этой задаче будет включать **Unity.Mvc3** пакет NuGet для решения.</span><span class="sxs-lookup"><span data-stu-id="c03ab-195">In this task, you will include **Unity.Mvc3** NuGet Package to the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="c03ab-196">Unity.Mvc3 пакет, предназначенный для ASP.NET MVC 3, но он является полностью совместимым с ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="c03ab-196">Unity.Mvc3 package was designed for ASP.NET MVC 3, but it is fully compatible with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="c03ab-197">Unity — например, контейнер внедрения зависимостей компактную, расширяемую с дополнительной поддержки и типа.</span><span class="sxs-lookup"><span data-stu-id="c03ab-197">Unity is a lightweight, extensible dependency injection container with optional support for instance and type interception.</span></span> <span data-ttu-id="c03ab-198">Он является универсальным контейнером для использования в любом приложении .NET.</span><span class="sxs-lookup"><span data-stu-id="c03ab-198">It is a general-purpose container for use in any type of .NET application.</span></span> <span data-ttu-id="c03ab-199">Он предоставляет общие функции, найден в том числе механизмах внедрения зависимостей: создание объектов, абстракции требований путем указания зависимости во время выполнения и гибкость, откладывая конфигурации компонента в контейнер.</span><span class="sxs-lookup"><span data-stu-id="c03ab-199">It provides all the common features found in dependency injection mechanisms including: object creation, abstraction of requirements by specifying dependencies at runtime and flexibility, by deferring the component configuration to the container.</span></span>


1. <span data-ttu-id="c03ab-200">Установка **Unity.Mvc3** пакет NuGet в **MvcMusicStore** проекта.</span><span class="sxs-lookup"><span data-stu-id="c03ab-200">Install **Unity.Mvc3** NuGet Package in the **MvcMusicStore** project.</span></span> <span data-ttu-id="c03ab-201">Чтобы сделать это, откройте **консоль диспетчера пакетов** из **представление** | **другие окна**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-201">To do this, open the **Package Manager Console** from **View** | **Other Windows**.</span></span>
2. <span data-ttu-id="c03ab-202">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="c03ab-202">Run the following command.</span></span>

    <span data-ttu-id="c03ab-203">PMC</span><span class="sxs-lookup"><span data-stu-id="c03ab-203">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    <span data-ttu-id="c03ab-204">![Установка пакета NuGet Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "Установка пакета NuGet Unity.Mvc3")</span><span class="sxs-lookup"><span data-stu-id="c03ab-204">![Installing Unity.Mvc3 NuGet Package](aspnet-mvc-4-dependency-injection/_static/image4.png "Installing Unity.Mvc3 NuGet Package")</span></span>

    <span data-ttu-id="c03ab-205">*Установка пакета NuGet Unity.Mvc3*</span><span class="sxs-lookup"><span data-stu-id="c03ab-205">*Installing Unity.Mvc3 NuGet Package*</span></span>
3. <span data-ttu-id="c03ab-206">Один раз **Unity.Mvc3** пакет установлен, просмотрите файлы и папки, автоматически добавляет для упрощения конфигурации Unity.</span><span class="sxs-lookup"><span data-stu-id="c03ab-206">Once the **Unity.Mvc3** package is installed, explore the files and folders it automatically adds in order to simplify Unity configuration.</span></span>

    <span data-ttu-id="c03ab-207">![Установлен пакет Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 пакета")</span><span class="sxs-lookup"><span data-stu-id="c03ab-207">![Unity.Mvc3 package installed](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 package installed")</span></span>

    <span data-ttu-id="c03ab-208">*Установлен пакет Unity.Mvc3*</span><span class="sxs-lookup"><span data-stu-id="c03ab-208">*Unity.Mvc3 package installed*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a><span data-ttu-id="c03ab-209">Задача 3 - регистрация Unity в приложении Global.asax.cs\_запуск</span><span class="sxs-lookup"><span data-stu-id="c03ab-209">Task 3 - Registering Unity in Global.asax.cs Application\_Start</span></span>

<span data-ttu-id="c03ab-210">В этой задаче будет обновлена **приложения\_запустить** метод находится в **Global.asax.cs** вызов инициализатора загрузчика Unity и затем обновить регистрации файла загрузчика Службы и контроллер используется для внедрения зависимости.</span><span class="sxs-lookup"><span data-stu-id="c03ab-210">In this task, you will update the **Application\_Start** method located in **Global.asax.cs** to call the Unity Bootstrapper initializer and then, update the Bootstrapper file registering the Service and Controller you will use for Dependency Injection.</span></span>

1. <span data-ttu-id="c03ab-211">Теперь будет подключать загрузчика, хранящийся в файле, который инициализирует контейнера Unity и сопоставителя зависимостей.</span><span class="sxs-lookup"><span data-stu-id="c03ab-211">Now, you will hook up the Bootstrapper which is the file that initializes the Unity container and Dependency Resolver.</span></span> <span data-ttu-id="c03ab-212">Чтобы сделать это, откройте **Global.asax.cs** и добавьте следующий выделенный код в **приложения\_запустить** метод.</span><span class="sxs-lookup"><span data-stu-id="c03ab-212">To do this, open **Global.asax.cs** and add the following highlighted code within the **Application\_Start** method.</span></span>

    <span data-ttu-id="c03ab-213">(Фрагмент - кода *Unity инициализировать лаборатории внедрения зависимостей ASP.NET - Ex01 -*)</span><span class="sxs-lookup"><span data-stu-id="c03ab-213">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Initialize Unity*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. <span data-ttu-id="c03ab-214">Откройте **Bootstrapper.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="c03ab-214">Open **Bootstrapper.cs** file.</span></span>
3. <span data-ttu-id="c03ab-215">Включите следующие пространства имен: **MvcMusicStore.Services** и **MusicStore.Controllers**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-215">Include the following namespaces: **MvcMusicStore.Services** and **MusicStore.Controllers**.</span></span>

    <span data-ttu-id="c03ab-216">(Фрагмент - кода *ASP.NET внедрения зависимостей лаборатории - Ex01 - загрузчика добавление пространств имен*)</span><span class="sxs-lookup"><span data-stu-id="c03ab-216">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. <span data-ttu-id="c03ab-217">Замените **BuildUnityContainer** метод содержимого со следующим кодом, который регистрирует контроллера хранилища и службы хранилища.</span><span class="sxs-lookup"><span data-stu-id="c03ab-217">Replace **BuildUnityContainer** method's content with the following code that registers Store Controller and Store Service.</span></span>

    <span data-ttu-id="c03ab-218">(Фрагмент - кода *ASP.NET зависимостей внедрения лаборатории - Ex01 - хранилище регистра контроллера и службы*)</span><span class="sxs-lookup"><span data-stu-id="c03ab-218">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller and Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="c03ab-219">Задача 4. запуск приложения</span><span class="sxs-lookup"><span data-stu-id="c03ab-219">Task 4 - Running the Application</span></span>

<span data-ttu-id="c03ab-220">В этой задаче будет выполняться приложение, чтобы убедиться, что теперь загрузки после включения Unity.</span><span class="sxs-lookup"><span data-stu-id="c03ab-220">In this task, you will run the application to verify that it can now be loaded after including Unity.</span></span>

1. <span data-ttu-id="c03ab-221">Нажмите клавишу **F5** для запуска приложения, приложение теперь следует загрузить без отображения сообщения об ошибке.</span><span class="sxs-lookup"><span data-stu-id="c03ab-221">Press **F5** to run the application, the application should now load without showing any error message.</span></span>

    <span data-ttu-id="c03ab-222">![Запуск приложения с помощью внедрения зависимости](aspnet-mvc-4-dependency-injection/_static/image6.png "запуск приложения с помощью внедрения зависимости")</span><span class="sxs-lookup"><span data-stu-id="c03ab-222">![Running Application with Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Running Application with Dependency Injection")</span></span>

    <span data-ttu-id="c03ab-223">*Запуск приложения с помощью внедрения зависимости*</span><span class="sxs-lookup"><span data-stu-id="c03ab-223">*Running Application with Dependency Injection*</span></span>
2. <span data-ttu-id="c03ab-224">Перейдите к **или сохранений**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-224">Browse to **/Store**.</span></span> <span data-ttu-id="c03ab-225">Это вызовет **StoreController**, который создается с помощью **Unity**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-225">This will invoke **StoreController**, which is now created using **Unity**.</span></span>

    <span data-ttu-id="c03ab-226">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span><span class="sxs-lookup"><span data-stu-id="c03ab-226">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span></span>

    <span data-ttu-id="c03ab-227">*MVC Music Store*</span><span class="sxs-lookup"><span data-stu-id="c03ab-227">*MVC Music Store*</span></span>
3. <span data-ttu-id="c03ab-228">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="c03ab-228">Close the browser.</span></span>

<span data-ttu-id="c03ab-229">В следующих упражнениях вы узнаете, как расширить область внедрения зависимостей, чтобы использовать его внутри представления ASP.NET MVC и фильтры действий.</span><span class="sxs-lookup"><span data-stu-id="c03ab-229">In the following exercises you will learn how to extend the Dependency Injection scope to use it inside ASP.NET MVC Views and Action Filters.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a><span data-ttu-id="c03ab-230">Упражнение 2: Добавление представления</span><span class="sxs-lookup"><span data-stu-id="c03ab-230">Exercise 2: Injecting a View</span></span>

<span data-ttu-id="c03ab-231">В этом упражнении вы узнаете, как использовать внедрение зависимостей в представлении с новыми функциями ASP.NET MVC 4 для интеграции Unity.</span><span class="sxs-lookup"><span data-stu-id="c03ab-231">In this exercise, you will learn how to use Dependency Injection in a view with the new features of ASP.NET MVC 4 for Unity integration.</span></span> <span data-ttu-id="c03ab-232">Чтобы сделать это, будет вызываться пользовательская служба внутри представления обзор хранилища, который будет отображать сообщение и образ ниже.</span><span class="sxs-lookup"><span data-stu-id="c03ab-232">In order to do that, you will call a custom service inside the Store Browse View, which will show a message and an image below.</span></span>

<span data-ttu-id="c03ab-233">Затем будет интегрировать проекта Unity и создать пользовательскую зависимость Сопоставитель для внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="c03ab-233">Then, you will integrate the project with Unity and create a custom dependency resolver to inject the dependencies.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a><span data-ttu-id="c03ab-234">Задача 1. Создание представления, использующее службу</span><span class="sxs-lookup"><span data-stu-id="c03ab-234">Task 1 - Creating a View that Consumes a Service</span></span>

<span data-ttu-id="c03ab-235">В этой задаче вы создадите представление, которое выполняет вызов службы для создания новых зависимостей.</span><span class="sxs-lookup"><span data-stu-id="c03ab-235">In this task, you will create a view that performs a service call to generate a new dependency.</span></span> <span data-ttu-id="c03ab-236">Служба состоит в простой службы обмена сообщениями, включенные в это решение.</span><span class="sxs-lookup"><span data-stu-id="c03ab-236">The service consists in a simple messaging service included in this solution.</span></span>

1. <span data-ttu-id="c03ab-237">Откройте **начать** решение находится в **вводится Source\Ex02 View\Begin** папки.</span><span class="sxs-lookup"><span data-stu-id="c03ab-237">Open the **Begin** solution located in the **Source\Ex02-Injecting View\Begin** folder.</span></span> <span data-ttu-id="c03ab-238">В противном случае можно продолжить использование **окончания** решения получен путем выполнения в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="c03ab-238">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="c03ab-239">Если вы открыли указанных **начать** решение, необходимо будет загрузить несколько отсутствующих пакетов NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="c03ab-239">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="c03ab-240">Чтобы сделать это, нажмите кнопку **проекта** и выбрать пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-240">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="c03ab-241">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="c03ab-241">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="c03ab-242">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-242">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="c03ab-243">Одним из преимуществ использования NuGet является у вас не указан для отправки всех библиотек в вашем проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="c03ab-243">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="c03ab-244">С помощью NuGet Power Tools указав версий пакета в файле Packages.config можно для загрузки всех необходимых библиотек первый раз, при запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="c03ab-244">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="c03ab-245">Вот почему необходимо будет выполнить эти шаги после открытия существующего решения в этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="c03ab-245">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="c03ab-246">Дополнительные сведения см. в разделе этой статьи: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="c03ab-246">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="c03ab-247">Включить **MessageService.cs** и **IMessageService.cs** классы находятся в **источника \Assets** папки в **/службы**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-247">Include the **MessageService.cs** and the **IMessageService.cs** classes located in the **Source \Assets** folder in **/Services**.</span></span> <span data-ttu-id="c03ab-248">Для этого щелкните правой кнопкой мыши **службы** папку и выберите **Добавление существующего элемента**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-248">To do this, right-click **Services** folder and select **Add Existing Item**.</span></span> <span data-ttu-id="c03ab-249">Перейдите в расположение файлов и включить их.</span><span class="sxs-lookup"><span data-stu-id="c03ab-249">Browse to the files' location and include them.</span></span>

    <span data-ttu-id="c03ab-250">![Добавление службы сообщений и интерфейс службы](aspnet-mvc-4-dependency-injection/_static/image8.png "Добавление службы сообщений и интерфейс службы")</span><span class="sxs-lookup"><span data-stu-id="c03ab-250">![Adding Message Service and Service Interface](aspnet-mvc-4-dependency-injection/_static/image8.png "Adding Message Service and Service Interface")</span></span>

    <span data-ttu-id="c03ab-251">*Добавление службы сообщений и интерфейс службы*</span><span class="sxs-lookup"><span data-stu-id="c03ab-251">*Adding Message Service and Service Interface*</span></span>

    > [!NOTE]
    > <span data-ttu-id="c03ab-252">**IMessageService** интерфейс определяет два свойства, реализуемый **MessageService** класса.</span><span class="sxs-lookup"><span data-stu-id="c03ab-252">The **IMessageService** interface defines two properties implemented by the **MessageService** class.</span></span> <span data-ttu-id="c03ab-253">Эти свойства -**сообщение** и **ImageUrl**-сохранить сообщение и URL-адрес изображения для отображения.</span><span class="sxs-lookup"><span data-stu-id="c03ab-253">These properties -**Message** and **ImageUrl**- store the message and the URL of the image to be displayed.</span></span>
3. <span data-ttu-id="c03ab-254">Создать папку **/страницы** в проекте корневой папки, а затем добавьте существующий класс **MyBasePage.cs** из **Source\Assets**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-254">Create the folder **/Pages** in the project's root folder, and then add the existing class **MyBasePage.cs** from **Source\Assets**.</span></span> <span data-ttu-id="c03ab-255">Базовой страницы, который будет наследовать имеет следующую структуру.</span><span class="sxs-lookup"><span data-stu-id="c03ab-255">The base page you will inherit from has the following structure.</span></span>

    <span data-ttu-id="c03ab-256">![Папка страниц](aspnet-mvc-4-dependency-injection/_static/image9.png "папок")</span><span class="sxs-lookup"><span data-stu-id="c03ab-256">![Pages folder](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages folder")</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. <span data-ttu-id="c03ab-257">Откройте **Browse.cshtml** просмотра из **/представления/Store** папки и сделать его наследуют **MyBasePage.cs**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-257">Open **Browse.cshtml** view from **/Views/Store** folder, and make it inherit from **MyBasePage.cs**.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. <span data-ttu-id="c03ab-258">В **Обзор** Просмотр, добавьте вызов **MessageService** для отображения изображения и сообщения, полученные службой.</span><span class="sxs-lookup"><span data-stu-id="c03ab-258">In the **Browse** view, add a call to **MessageService** to display an image and a message retrieved by the service.</span></span>
   <span data-ttu-id="c03ab-259">(C#)</span><span class="sxs-lookup"><span data-stu-id="c03ab-259">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a><span data-ttu-id="c03ab-260">Задача 2 - включая Сопоставитель пользовательскую зависимость и активатор страницы представления</span><span class="sxs-lookup"><span data-stu-id="c03ab-260">Task 2 - Including a Custom Dependency Resolver and a Custom View Page Activator</span></span>

<span data-ttu-id="c03ab-261">В предыдущей задаче введенный новой зависимости внутри представления для выполнения вызова служб внутри него.</span><span class="sxs-lookup"><span data-stu-id="c03ab-261">In the previous task, you injected a new dependency inside a view to perform a service call inside it.</span></span> <span data-ttu-id="c03ab-262">Теперь будет разрешить зависимость путем реализации интерфейсов внедрения зависимостей MVC ASP.NET **IViewPageActivator** и **IDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-262">Now, you will resolve that dependency by implementing the ASP.NET MVC Dependency Injection interfaces **IViewPageActivator** and **IDependencyResolver**.</span></span> <span data-ttu-id="c03ab-263">В решение будет включать реализацию **IDependencyResolver** , будет работать с Получение службы с помощью Unity.</span><span class="sxs-lookup"><span data-stu-id="c03ab-263">You will include in the solution an implementation of **IDependencyResolver** that will deal with the service retrieval by using Unity.</span></span> <span data-ttu-id="c03ab-264">Затем следует включить другую пользовательскую реализацию **IViewPageActivator** интерфейс, который предстоит решить создания представлений.</span><span class="sxs-lookup"><span data-stu-id="c03ab-264">Then, you will include another custom implementation of **IViewPageActivator** interface that will solve the creation of the views.</span></span>

> [!NOTE]
> <span data-ttu-id="c03ab-265">С момента ASP.NET MVC 3 реализацию для внедрения зависимостей упрощен интерфейсы для регистрации службы.</span><span class="sxs-lookup"><span data-stu-id="c03ab-265">Since ASP.NET MVC 3, the implementation for Dependency Injection had simplified the interfaces to register services.</span></span> <span data-ttu-id="c03ab-266">**IDependencyResolver** и **IViewPageActivator** являются частью функции ASP.NET MVC 3 для внедрения зависимости.</span><span class="sxs-lookup"><span data-stu-id="c03ab-266">**IDependencyResolver** and **IViewPageActivator** are part of ASP.NET MVC 3 features for Dependency Injection.</span></span>
> 
> <span data-ttu-id="c03ab-267">**-IDependencyResolver** интерфейс заменяет предыдущие IMvcServiceLocator.</span><span class="sxs-lookup"><span data-stu-id="c03ab-267">**- IDependencyResolver** interface replaces the previous IMvcServiceLocator.</span></span> <span data-ttu-id="c03ab-268">Объекты, реализующие IDependencyResolver должен возвращать экземпляр службы или службы коллекции.</span><span class="sxs-lookup"><span data-stu-id="c03ab-268">Implementers of IDependencyResolver must return an instance of the service or a service collection.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> <span data-ttu-id="c03ab-269">**-IViewPageActivator** интерфейс обеспечивает более точное управление как просмотр страниц создаются через внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="c03ab-269">**- IViewPageActivator** interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="c03ab-270">Классы, реализующие **IViewPageActivator** интерфейса можно создавать экземпляры представление, используя сведения о контексте.</span><span class="sxs-lookup"><span data-stu-id="c03ab-270">The classes that implement **IViewPageActivator** interface can create view instances using context information.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. <span data-ttu-id="c03ab-271">Создать /**фабрик** папку в корневой папке проекта.</span><span class="sxs-lookup"><span data-stu-id="c03ab-271">Create the /**Factories** folder in the project's root folder.</span></span>
2. <span data-ttu-id="c03ab-272">Включить **CustomViewPageActivator.cs** решение из **/источники/активы/** для **фабрик** папки.</span><span class="sxs-lookup"><span data-stu-id="c03ab-272">Include **CustomViewPageActivator.cs** to your solution from **/Sources/Assets/** to **Factories** folder.</span></span> <span data-ttu-id="c03ab-273">Чтобы сделать это, щелкните правой кнопкой мыши **/Factories** выберите **добавить | Существующий элемент** , а затем выберите **CustomViewPageActivator.cs**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-273">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **CustomViewPageActivator.cs**.</span></span> <span data-ttu-id="c03ab-274">Этот класс реализует **IViewPageActivator** интерфейс для хранения контейнера Unity.</span><span class="sxs-lookup"><span data-stu-id="c03ab-274">This class implements the **IViewPageActivator** interface to hold the Unity Container.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="c03ab-275">**CustomViewPageActivator** отвечает за управление созданием представления с помощью контейнера Unity.</span><span class="sxs-lookup"><span data-stu-id="c03ab-275">**CustomViewPageActivator** is responsible for managing the creation of a view by using a Unity container.</span></span>
3. <span data-ttu-id="c03ab-276">Включить **UnityDependencyResolver.cs** файл из **/источники/активы** для **/Factories** папки.</span><span class="sxs-lookup"><span data-stu-id="c03ab-276">Include **UnityDependencyResolver.cs** file from **/Sources/Assets** to **/Factories** folder.</span></span> <span data-ttu-id="c03ab-277">Чтобы сделать это, щелкните правой кнопкой мыши **/Factories** выберите **добавить | Существующий элемент** , а затем выберите **UnityDependencyResolver.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="c03ab-277">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **UnityDependencyResolver.cs** file.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="c03ab-278">**UnityDependencyResolver** класс является пользовательской DependencyResolver для Unity.</span><span class="sxs-lookup"><span data-stu-id="c03ab-278">**UnityDependencyResolver** class is a custom DependencyResolver for Unity.</span></span> <span data-ttu-id="c03ab-279">Если службу не удается найти внутри контейнера Unity, является invocated базового сопоставителя.</span><span class="sxs-lookup"><span data-stu-id="c03ab-279">When a service cannot be found inside the Unity container, the base resolver is invocated.</span></span>

<span data-ttu-id="c03ab-280">В следующей задаче будет зарегистрирован обе реализации для модели известно расположение службы и представления.</span><span class="sxs-lookup"><span data-stu-id="c03ab-280">In the following task both implementations will be registered to let the model know the location of the services and the views.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a><span data-ttu-id="c03ab-281">Задача 3. Регистрация для внедрения зависимости в пределах контейнера Unity</span><span class="sxs-lookup"><span data-stu-id="c03ab-281">Task 3 - Registering for Dependency Injection within Unity container</span></span>

<span data-ttu-id="c03ab-282">В этой задаче будет помещено все предыдущие о вместе, чтобы сделать работать внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="c03ab-282">In this task, you will put all the previous things together to make Dependency Injection work.</span></span>

<span data-ttu-id="c03ab-283">До сих решения содержит следующие элементы:</span><span class="sxs-lookup"><span data-stu-id="c03ab-283">Up to now your solution has the following elements:</span></span>

- <span data-ttu-id="c03ab-284">Объект **Обзор** представление, которое наследует от **MyBaseClass с помощью модификатора** и использует **MessageService**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-284">A **Browse** View that inherits from **MyBaseClass** and consumes **MessageService**.</span></span>
- <span data-ttu-id="c03ab-285">Промежуточный класс -**MyBaseClass с помощью модификатора**-с внедрения зависимостей, объявленным для интерфейса службы.</span><span class="sxs-lookup"><span data-stu-id="c03ab-285">An intermediate class -**MyBaseClass**- that has dependency injection declared for the service interface.</span></span>
- <span data-ttu-id="c03ab-286">Службе - **MessageService** - и интерфейс **IMessageService**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-286">A service - **MessageService** - and its interface **IMessageService**.</span></span>
- <span data-ttu-id="c03ab-287">Сопоставитель пользовательскую зависимость для Unity - **UnityDependencyResolver** -, реагирует на получение службы.</span><span class="sxs-lookup"><span data-stu-id="c03ab-287">A custom dependency resolver for Unity - **UnityDependencyResolver** - that deals with service retrieval.</span></span>
- <span data-ttu-id="c03ab-288">Активатор страницы представления - **CustomViewPageActivator** -, создающее страницу.</span><span class="sxs-lookup"><span data-stu-id="c03ab-288">A View Page activator - **CustomViewPageActivator** - that creates the page.</span></span>

<span data-ttu-id="c03ab-289">Для вставки **Обзор** представления, теперь зарегистрируйте Сопоставитель пользовательскую зависимость в контейнере Unity.</span><span class="sxs-lookup"><span data-stu-id="c03ab-289">To inject **Browse** View, you will now register the custom dependency resolver in the Unity container.</span></span>

1. <span data-ttu-id="c03ab-290">Откройте **Bootstrapper.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="c03ab-290">Open **Bootstrapper.cs** file.</span></span>
2. <span data-ttu-id="c03ab-291">Регистрация экземпляра **MessageService** в Unity контейнер для инициализации службы:</span><span class="sxs-lookup"><span data-stu-id="c03ab-291">Register an instance of **MessageService** into the Unity container to initialize the service:</span></span>

    <span data-ttu-id="c03ab-292">(Фрагмент - кода *службу сообщение Register лаборатории - Ex02 - внедрения зависимостей ASP.NET*)</span><span class="sxs-lookup"><span data-stu-id="c03ab-292">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. <span data-ttu-id="c03ab-293">Добавьте ссылку на **MvcMusicStore.Factories** пространства имен.</span><span class="sxs-lookup"><span data-stu-id="c03ab-293">Add a reference to **MvcMusicStore.Factories** namespace.</span></span>

    <span data-ttu-id="c03ab-294">(Фрагмент - кода *пространство имен фабрики лаборатории - Ex02 - внедрения зависимостей ASP.NET*)</span><span class="sxs-lookup"><span data-stu-id="c03ab-294">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Factories Namespace*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. <span data-ttu-id="c03ab-295">Зарегистрировать **CustomViewPageActivator** как активатор страницы представления в Unity контейнер:</span><span class="sxs-lookup"><span data-stu-id="c03ab-295">Register **CustomViewPageActivator** as a View Page activator into the Unity container:</span></span>

    <span data-ttu-id="c03ab-296">(Фрагмент - кода *ASP.NET зависимостей внедрения лаборатории - Ex02 - Register CustomViewPageActivator*)</span><span class="sxs-lookup"><span data-stu-id="c03ab-296">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. <span data-ttu-id="c03ab-297">Замените экземпляр по умолчанию сопоставителем зависимостей ASP.NET MVC 4 **UnityDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-297">Replace ASP.NET MVC 4 default dependency resolver with an instance of **UnityDependencyResolver**.</span></span> <span data-ttu-id="c03ab-298">Для этого замените **Initialise** метод содержимого с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="c03ab-298">To do this, replace **Initialise** method content with the following code:</span></span>

    <span data-ttu-id="c03ab-299">(Фрагмент - кода *ASP.NET зависимостей внедрения лаборатории - Ex02 - обновления Сопоставитель зависимостей*)</span><span class="sxs-lookup"><span data-stu-id="c03ab-299">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Update Dependency Resolver*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="c03ab-300">ASP.NET MVC предоставляет класс Сопоставитель зависимостей по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="c03ab-300">ASP.NET MVC provides a default dependency resolver class.</span></span> <span data-ttu-id="c03ab-301">Для работы с распознаватели пользовательскую зависимость, которая создана unity, должно быть заменено этим распознавателем.</span><span class="sxs-lookup"><span data-stu-id="c03ab-301">To work with custom dependency resolvers as the one we have created for unity, this resolver has to be replaced.</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="c03ab-302">Задача 4. запуск приложения</span><span class="sxs-lookup"><span data-stu-id="c03ab-302">Task 4 - Running the Application</span></span>

<span data-ttu-id="c03ab-303">В этой задаче будет выполняться приложение, чтобы убедиться, что обозреватель хранилища использует службу и показывает образа и получить сообщение:</span><span class="sxs-lookup"><span data-stu-id="c03ab-303">In this task, you will run the application to verify that the Store Browser consumes the service and shows the image and the message retrieved:</span></span>

1. <span data-ttu-id="c03ab-304">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="c03ab-304">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="c03ab-305">Нажмите кнопку **рок** в меню жанров и см. в разделе как **MessageService** подставленный к представлению и загрузке приветственное сообщение и изображения.</span><span class="sxs-lookup"><span data-stu-id="c03ab-305">Click **Rock** within the Genres Menu and see how the **MessageService** was injected to the view and loaded the welcome message and the image.</span></span> <span data-ttu-id="c03ab-306">В этом примере вводится для &quot; **рок**&quot;:</span><span class="sxs-lookup"><span data-stu-id="c03ab-306">In this example, we are entering to &quot;**Rock**&quot;:</span></span>

    <span data-ttu-id="c03ab-307">![Внедрение представления MVC Music Store -](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - представление внедрения")</span><span class="sxs-lookup"><span data-stu-id="c03ab-307">![MVC Music Store - View Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - View Injection")</span></span>

    <span data-ttu-id="c03ab-308">*MVC Music Store - представление внедрения*</span><span class="sxs-lookup"><span data-stu-id="c03ab-308">*MVC Music Store - View Injection*</span></span>
3. <span data-ttu-id="c03ab-309">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="c03ab-309">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a><span data-ttu-id="c03ab-310">Упражнение 3: Добавление фильтров действий</span><span class="sxs-lookup"><span data-stu-id="c03ab-310">Exercise 3: Injecting Action Filters</span></span>

<span data-ttu-id="c03ab-311">В предыдущей практической **настраиваемые фильтры действий** вы работали с путем внедрения кода и настройки фильтров.</span><span class="sxs-lookup"><span data-stu-id="c03ab-311">In the previous Hands-On lab **Custom Action Filters** you have worked with filters customization and injection.</span></span> <span data-ttu-id="c03ab-312">В этом упражнении вы узнаете, как ввести фильтры с внедрения зависимостей с помощью контейнера Unity.</span><span class="sxs-lookup"><span data-stu-id="c03ab-312">In this exercise, you will learn how to inject filters with Dependency Injection by using the Unity container.</span></span> <span data-ttu-id="c03ab-313">Чтобы сделать это, вы добавите в решение Music Store пользовательского фильтра действий, будет отслеживать действия узла.</span><span class="sxs-lookup"><span data-stu-id="c03ab-313">To do that, you will add to the Music Store solution a custom action filter that will trace the activity of the site.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a><span data-ttu-id="c03ab-314">Задача 1 - включение фильтра отслеживания в решении</span><span class="sxs-lookup"><span data-stu-id="c03ab-314">Task 1 - Including the Tracking Filter in the Solution</span></span>

<span data-ttu-id="c03ab-315">В этой задаче будут включены в Music Store пользовательского фильтра действий для событий трассировки.</span><span class="sxs-lookup"><span data-stu-id="c03ab-315">In this task, you will include in the Music Store a custom action filter to trace events.</span></span> <span data-ttu-id="c03ab-316">Основные понятия как пользовательского фильтра действий, всегда обрабатываются в предыдущем лаборатории &quot;пользовательских фильтров действий&quot;, необходимо просто добавить класс фильтра из папки Assets этой лабораторной и затем создать поставщик фильтра для Unity:</span><span class="sxs-lookup"><span data-stu-id="c03ab-316">As custom action filter concepts are already treated in the previous Lab &quot;Custom Action Filters&quot;, you will just include the filter class from the Assets folder of this lab, and then create a Filter Provider for Unity:</span></span>

1. <span data-ttu-id="c03ab-317">Откройте **начать** решение находится в **Source\Ex03 - Filter\Begin действие вводится** папки.</span><span class="sxs-lookup"><span data-stu-id="c03ab-317">Open the **Begin** solution located in the **Source\Ex03 - Injecting Action Filter\Begin** folder.</span></span> <span data-ttu-id="c03ab-318">В противном случае можно продолжить использование **окончания** решения получен путем выполнения в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="c03ab-318">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="c03ab-319">Если вы открыли указанных **начать** решение, необходимо будет загрузить несколько отсутствующих пакетов NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="c03ab-319">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="c03ab-320">Чтобы сделать это, нажмите кнопку **проекта** и выбрать пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-320">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="c03ab-321">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="c03ab-321">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="c03ab-322">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-322">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="c03ab-323">Одним из преимуществ использования NuGet является у вас не указан для отправки всех библиотек в вашем проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="c03ab-323">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="c03ab-324">С помощью NuGet Power Tools указав версий пакета в файле Packages.config можно для загрузки всех необходимых библиотек первый раз, при запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="c03ab-324">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="c03ab-325">Вот почему необходимо будет выполнить эти шаги после открытия существующего решения в этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="c03ab-325">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="c03ab-326">Дополнительные сведения см. в разделе этой статьи: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="c03ab-326">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="c03ab-327">Включить **TraceActionFilter.cs** файл из **/источники/активы** для **/фильтрует** папки.</span><span class="sxs-lookup"><span data-stu-id="c03ab-327">Include **TraceActionFilter.cs** file from **/Sources/Assets** to **/Filters** folder.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > <span data-ttu-id="c03ab-328">Этот фильтр настраиваемое действие выполняет трассировку ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c03ab-328">This custom action filter performs ASP.NET tracing.</span></span> <span data-ttu-id="c03ab-329">Вы можете проверить &quot;локальный ASP.NET MVC 4 и динамические фильтры действий&quot; лаборатории для получения дополнительной справки.</span><span class="sxs-lookup"><span data-stu-id="c03ab-329">You can check &quot;ASP.NET MVC 4 local and Dynamic Action Filters&quot; Lab for more reference.</span></span>
3. <span data-ttu-id="c03ab-330">Добавление пустой класс **FilterProvider.cs** в проект в папке **или фильтры.**</span><span class="sxs-lookup"><span data-stu-id="c03ab-330">Add the empty class **FilterProvider.cs** to the project in the folder **/Filters.**</span></span>
4. <span data-ttu-id="c03ab-331">Добавить **System.Web.Mvc** и **Microsoft.Practices.Unity** пространства имен в **FilterProvider.cs**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-331">Add the **System.Web.Mvc** and **Microsoft.Practices.Unity** namespaces in **FilterProvider.cs**.</span></span>

    <span data-ttu-id="c03ab-332">(Фрагмент - кода *ASP.NET зависимостей внедрения лаборатории - Ex03 - фильтр поставщика добавление пространств имен*)</span><span class="sxs-lookup"><span data-stu-id="c03ab-332">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. <span data-ttu-id="c03ab-333">Сделайте класс наследовал от **IFilterProvider** интерфейса.</span><span class="sxs-lookup"><span data-stu-id="c03ab-333">Make the class inherit from **IFilterProvider** Interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. <span data-ttu-id="c03ab-334">Добавить **IUnityContainer** свойство в **FilterProvider** класса, а затем создать конструктор класса, чтобы назначить контейнера.</span><span class="sxs-lookup"><span data-stu-id="c03ab-334">Add a **IUnityContainer** property in the **FilterProvider** class, and then create a class constructor to assign the container.</span></span>

    <span data-ttu-id="c03ab-335">(Фрагмент - кода *ASP.NET зависимостей внедрения лаборатории - Ex03 - фильтра Конструктор поставщика*)</span><span class="sxs-lookup"><span data-stu-id="c03ab-335">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Constructor*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="c03ab-336">Конструктор класса поставщика фильтра не создавая **новый** внутри объекта.</span><span class="sxs-lookup"><span data-stu-id="c03ab-336">The filter provider class constructor is not creating a **new** object inside.</span></span> <span data-ttu-id="c03ab-337">Контейнера передается в качестве параметра, а зависимость решается путем Unity.</span><span class="sxs-lookup"><span data-stu-id="c03ab-337">The container is passed as a parameter, and the dependency is solved by Unity.</span></span>
7. <span data-ttu-id="c03ab-338">В **FilterProvider** , следует реализовать метод **GetFilters** из **IFilterProvider** интерфейса.</span><span class="sxs-lookup"><span data-stu-id="c03ab-338">In the **FilterProvider** class, implement the method **GetFilters** from **IFilterProvider** interface.</span></span>

    <span data-ttu-id="c03ab-339">(Фрагмент - кода *ASP.NET зависимостей внедрения лаборатории - Ex03 - фильтр поставщика GetFilters*)</span><span class="sxs-lookup"><span data-stu-id="c03ab-339">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider GetFilters*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a><span data-ttu-id="c03ab-340">Задача 2 - регистрация и включение фильтра</span><span class="sxs-lookup"><span data-stu-id="c03ab-340">Task 2 - Registering and Enabling the Filter</span></span>

<span data-ttu-id="c03ab-341">В этой задаче будет включить отслеживание сайта.</span><span class="sxs-lookup"><span data-stu-id="c03ab-341">In this task, you will enable site tracking.</span></span> <span data-ttu-id="c03ab-342">Для этого потребуется зарегистрировать фильтр в **Bootstrapper.cs BuildUnityContainer** метода для запуска трассировки:</span><span class="sxs-lookup"><span data-stu-id="c03ab-342">To do that, you will register the filter in **Bootstrapper.cs BuildUnityContainer** method to start tracing:</span></span>

1. <span data-ttu-id="c03ab-343">Откройте **Web.config** находится в корневой каталог проекта, а также включение трассировки отслеживания в группе System.Web.</span><span class="sxs-lookup"><span data-stu-id="c03ab-343">Open **Web.config** located in the project root and enable trace tracking at System.Web group.</span></span>

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. <span data-ttu-id="c03ab-344">Откройте **Bootstrapper.cs** корне проекта.</span><span class="sxs-lookup"><span data-stu-id="c03ab-344">Open **Bootstrapper.cs** at project root.</span></span>
3. <span data-ttu-id="c03ab-345">Добавьте ссылку на **MvcMusicStore.Filters** пространства имен.</span><span class="sxs-lookup"><span data-stu-id="c03ab-345">Add a reference to the **MvcMusicStore.Filters** namespace.</span></span>

    <span data-ttu-id="c03ab-346">(Фрагмент - кода *ASP.NET внедрения зависимостей лаборатории - Ex03 - загрузчика добавление пространств имен*)</span><span class="sxs-lookup"><span data-stu-id="c03ab-346">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. <span data-ttu-id="c03ab-347">Выберите **BuildUnityContainer** метод и зарегистрировать фильтр в контейнере Unity.</span><span class="sxs-lookup"><span data-stu-id="c03ab-347">Select the **BuildUnityContainer** method and register the filter in the Unity Container.</span></span> <span data-ttu-id="c03ab-348">Необходимо зарегистрировать поставщик фильтра, а также действие фильтра.</span><span class="sxs-lookup"><span data-stu-id="c03ab-348">You will have to register the filter provider as well as the action filter.</span></span>

    <span data-ttu-id="c03ab-349">(Фрагмент - кода *ASP.NET зависимостей внедрения лаборатории - Ex03 - Register FilterProvider и ActionFilter*)</span><span class="sxs-lookup"><span data-stu-id="c03ab-349">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider and ActionFilter*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="c03ab-350">Задача 3. запуск приложения</span><span class="sxs-lookup"><span data-stu-id="c03ab-350">Task 3 - Running the Application</span></span>

<span data-ttu-id="c03ab-351">В этой задаче будет запустите приложение и проверить, что пользовательского фильтра действий трассировка действия:</span><span class="sxs-lookup"><span data-stu-id="c03ab-351">In this task, you will run the application and test that the custom action filter is tracing the activity:</span></span>

1. <span data-ttu-id="c03ab-352">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="c03ab-352">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="c03ab-353">Нажмите кнопку **рок** жанров меню.</span><span class="sxs-lookup"><span data-stu-id="c03ab-353">Click **Rock** within the Genres Menu.</span></span> <span data-ttu-id="c03ab-354">Можно указать несколько жанров, если вы хотите.</span><span class="sxs-lookup"><span data-stu-id="c03ab-354">You can browse to more genres if you want to.</span></span>

    <span data-ttu-id="c03ab-355">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span><span class="sxs-lookup"><span data-stu-id="c03ab-355">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span></span>

    <span data-ttu-id="c03ab-356">*Приложение Music Store*</span><span class="sxs-lookup"><span data-stu-id="c03ab-356">*Music Store*</span></span>
3. <span data-ttu-id="c03ab-357">Перейдите к **/Trace.axd** для просмотра трассировки приложения и нажмите кнопку **Просмотр сведений о**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-357">Browse to **/Trace.axd** to see the Application Trace page, and then click **View Details**.</span></span>

    <span data-ttu-id="c03ab-358">![Журнал трассировки приложения](aspnet-mvc-4-dependency-injection/_static/image12.png "журнал трассировки приложения")</span><span class="sxs-lookup"><span data-stu-id="c03ab-358">![Application Trace Log](aspnet-mvc-4-dependency-injection/_static/image12.png "Application Trace Log")</span></span>

    <span data-ttu-id="c03ab-359">*Журнал трассировки приложения*</span><span class="sxs-lookup"><span data-stu-id="c03ab-359">*Application Trace Log*</span></span>

    <span data-ttu-id="c03ab-360">![Трассировка приложения - сведения о запросе](aspnet-mvc-4-dependency-injection/_static/image13.png "трассировки приложения - сведения о запросе")</span><span class="sxs-lookup"><span data-stu-id="c03ab-360">![Application Trace - Request Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - Request Details")</span></span>

    <span data-ttu-id="c03ab-361">*Трассировка приложения - сведения о запросе*</span><span class="sxs-lookup"><span data-stu-id="c03ab-361">*Application Trace - Request Details*</span></span>
4. <span data-ttu-id="c03ab-362">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="c03ab-362">Close the browser.</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="c03ab-363">Сводка</span><span class="sxs-lookup"><span data-stu-id="c03ab-363">Summary</span></span>

<span data-ttu-id="c03ab-364">Выполнив это Практическое лабораторное занятие изучено используют внедрение зависимостей в ASP.NET MVC 4, интегрируя Unity с помощью пакета NuGet.</span><span class="sxs-lookup"><span data-stu-id="c03ab-364">By completing this Hands-On Lab you have learned how to use Dependency Injection in ASP.NET MVC 4 by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="c03ab-365">Чтобы добиться этого, вы использовали внедрения зависимости внутри контроллеров, представлений и фильтры действий.</span><span class="sxs-lookup"><span data-stu-id="c03ab-365">To achieve that, you have used Dependency Injection inside controllers, views and action filters.</span></span>

<span data-ttu-id="c03ab-366">Были освещены следующие понятия:</span><span class="sxs-lookup"><span data-stu-id="c03ab-366">The following concepts were covered:</span></span>

- <span data-ttu-id="c03ab-367">Возможности внедрения зависимостей ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="c03ab-367">ASP.NET MVC 4 Dependency Injection features</span></span>
- <span data-ttu-id="c03ab-368">Интеграция с Unity с помощью пакета NuGet Unity.Mvc3</span><span class="sxs-lookup"><span data-stu-id="c03ab-368">Unity integration using Unity.Mvc3 NuGet Package</span></span>
- <span data-ttu-id="c03ab-369">Внедрение зависимостей в контроллерах</span><span class="sxs-lookup"><span data-stu-id="c03ab-369">Dependency Injection in Controllers</span></span>
- <span data-ttu-id="c03ab-370">Внедрение зависимостей в представлениях</span><span class="sxs-lookup"><span data-stu-id="c03ab-370">Dependency Injection in Views</span></span>
- <span data-ttu-id="c03ab-371">Внедрение зависимостей фильтров действий</span><span class="sxs-lookup"><span data-stu-id="c03ab-371">Dependency injection of Action Filters</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="c03ab-372">Приложение а. Установка Visual Studio Express 2012 для Web</span><span class="sxs-lookup"><span data-stu-id="c03ab-372">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="c03ab-373">Можно установить **Microsoft Visual Studio Express 2012 для Web** или другой &quot;Express&quot; версию, используя **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-373">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="c03ab-374">Следующие инструкции описывают действия, необходимые для установки *Visual studio Express 2012 для Web* с помощью *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="c03ab-374">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="c03ab-375">Последовательно выберите пункты [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="c03ab-375">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="c03ab-376">Кроме того, если вы уже установили установщика веб-платформы, можно открыть его и выполните поиск продукта &quot; <em>Visual Studio Express 2012 для Web с пакетом Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="c03ab-376">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="c03ab-377">Щелкните **установить сейчас**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-377">Click on **Install Now**.</span></span> <span data-ttu-id="c03ab-378">Если у вас **Web Platform Installer** вы будете перенаправлены, чтобы загрузить и установить ее сначала.</span><span class="sxs-lookup"><span data-stu-id="c03ab-378">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="c03ab-379">Один раз **Web Platform Installer** открыто, нажмите кнопку **установить** для запуска программы установки.</span><span class="sxs-lookup"><span data-stu-id="c03ab-379">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="c03ab-380">![Установка Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "установка Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="c03ab-380">![Install Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="c03ab-381">*Установка Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="c03ab-381">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="c03ab-382">Чтение всех продуктов лицензий и условия и нажмите кнопку **принимаю** для продолжения.</span><span class="sxs-lookup"><span data-stu-id="c03ab-382">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Принятие условий лицензионного соглашения](aspnet-mvc-4-dependency-injection/_static/image15.png)

    <span data-ttu-id="c03ab-384">*Принятие условий лицензионного соглашения*</span><span class="sxs-lookup"><span data-stu-id="c03ab-384">*Accepting the license terms*</span></span>
5. <span data-ttu-id="c03ab-385">Дождитесь завершения процесса загрузки и установки.</span><span class="sxs-lookup"><span data-stu-id="c03ab-385">Wait until the downloading and installation process completes.</span></span>

    ![Ход установки](aspnet-mvc-4-dependency-injection/_static/image16.png)

    <span data-ttu-id="c03ab-387">*Ход выполнения установки*</span><span class="sxs-lookup"><span data-stu-id="c03ab-387">*Installation progress*</span></span>
6. <span data-ttu-id="c03ab-388">По завершении установки нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-388">When the installation completes, click **Finish**.</span></span>

    ![Установка завершена](aspnet-mvc-4-dependency-injection/_static/image17.png)

    <span data-ttu-id="c03ab-390">*Установка завершена*</span><span class="sxs-lookup"><span data-stu-id="c03ab-390">*Installation completed*</span></span>
7. <span data-ttu-id="c03ab-391">Нажмите кнопку **выхода** закрыть установщик веб-платформы.</span><span class="sxs-lookup"><span data-stu-id="c03ab-391">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="c03ab-392">Чтобы открыть Visual Studio Express для Web, перейдите к **запустить** экрана и начинается запись &quot; **VS Express**&quot;, выберите команду **VS Express для Web** Плитка.</span><span class="sxs-lookup"><span data-stu-id="c03ab-392">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express для Web плитки](aspnet-mvc-4-dependency-injection/_static/image18.png)

    <span data-ttu-id="c03ab-394">*VS Express для Web плитки*</span><span class="sxs-lookup"><span data-stu-id="c03ab-394">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="c03ab-395">Приложение б. фрагменты кода</span><span class="sxs-lookup"><span data-stu-id="c03ab-395">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="c03ab-396">С помощью фрагментов кода у вас есть весь код, который требуется под рукой.</span><span class="sxs-lookup"><span data-stu-id="c03ab-396">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="c03ab-397">Документ лаборатории сообщает только при их использовании, как показано на следующем рисунке.</span><span class="sxs-lookup"><span data-stu-id="c03ab-397">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="c03ab-398">![Фрагменты кода Visual Studio для вставки кода в проекте](aspnet-mvc-4-dependency-injection/_static/image19.png "фрагменты кода с помощью Visual Studio вставьте код в проект")</span><span class="sxs-lookup"><span data-stu-id="c03ab-398">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-dependency-injection/_static/image19.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="c03ab-399">*Фрагменты кода Visual Studio для вставки кода в проекте*</span><span class="sxs-lookup"><span data-stu-id="c03ab-399">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="c03ab-400">***Добавление фрагмента кода, с помощью клавиатуры (только C#)***</span><span class="sxs-lookup"><span data-stu-id="c03ab-400">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="c03ab-401">Поместите курсор в место вставки кода.</span><span class="sxs-lookup"><span data-stu-id="c03ab-401">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="c03ab-402">Начните вводить имя фрагмента (без пробелов и дефисы).</span><span class="sxs-lookup"><span data-stu-id="c03ab-402">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="c03ab-403">Смотрите, как IntelliSense отображает, совпадающие с именами фрагменты.</span><span class="sxs-lookup"><span data-stu-id="c03ab-403">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="c03ab-404">Выберите правильный фрагмент (или продолжите ввод, пока не будет выделен весь фрагмент имени).</span><span class="sxs-lookup"><span data-stu-id="c03ab-404">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="c03ab-405">Нажмите клавишу Tab дважды, чтобы вставить фрагмент в положении курсора.</span><span class="sxs-lookup"><span data-stu-id="c03ab-405">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="c03ab-406">![Начните вводить имя фрагмента](aspnet-mvc-4-dependency-injection/_static/image20.png "начните вводить имя фрагмента")</span><span class="sxs-lookup"><span data-stu-id="c03ab-406">![Start typing the snippet name](aspnet-mvc-4-dependency-injection/_static/image20.png "Start typing the snippet name")</span></span>

<span data-ttu-id="c03ab-407">*Начните вводить имя фрагмента*</span><span class="sxs-lookup"><span data-stu-id="c03ab-407">*Start typing the snippet name*</span></span>

<span data-ttu-id="c03ab-408">![Нажмите клавишу Tab, чтобы выделить выделенный фрагмент](aspnet-mvc-4-dependency-injection/_static/image21.png "нажмите клавишу Tab, чтобы выделить выделенный фрагмент")</span><span class="sxs-lookup"><span data-stu-id="c03ab-408">![Press Tab to select the highlighted snippet](aspnet-mvc-4-dependency-injection/_static/image21.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="c03ab-409">*Нажмите клавишу Tab, чтобы выделить выделенный фрагмент*</span><span class="sxs-lookup"><span data-stu-id="c03ab-409">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="c03ab-410">![Снова нажмите клавишу Tab и фрагмент будет расширен](aspnet-mvc-4-dependency-injection/_static/image22.png "будет расширен, снова нажмите клавишу Tab и фрагмента кода")</span><span class="sxs-lookup"><span data-stu-id="c03ab-410">![Press Tab again and the snippet will expand](aspnet-mvc-4-dependency-injection/_static/image22.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="c03ab-411">*Снова нажмите клавишу Tab и фрагмент будет расширен*</span><span class="sxs-lookup"><span data-stu-id="c03ab-411">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="c03ab-412">***Добавление фрагмента кода, с помощью мыши (C#, Visual Basic и XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="c03ab-412">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="c03ab-413">Щелкните правой кнопкой мыши место вставки фрагмента кода.</span><span class="sxs-lookup"><span data-stu-id="c03ab-413">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="c03ab-414">Выберите **вставить фрагмент** следуют **Мои фрагменты кода**.</span><span class="sxs-lookup"><span data-stu-id="c03ab-414">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="c03ab-415">Выберите соответствующий фрагмент из списка, щелкнув по ней.</span><span class="sxs-lookup"><span data-stu-id="c03ab-415">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="c03ab-416">![Щелкните правой кнопкой мыши место для вставки фрагмента кода и выбора вставить фрагмент](aspnet-mvc-4-dependency-injection/_static/image23.png "место для вставки фрагмента кода и выбора вставить фрагмент кода щелкните правой кнопкой мыши")</span><span class="sxs-lookup"><span data-stu-id="c03ab-416">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-dependency-injection/_static/image23.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="c03ab-417">*Щелкните правой кнопкой мыши в место вставки фрагмента кода и выберите Вставить фрагмент*</span><span class="sxs-lookup"><span data-stu-id="c03ab-417">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="c03ab-418">![Выберите из списка, соответствующего фрагмента, щелкнув его](aspnet-mvc-4-dependency-injection/_static/image24.png "выбрать соответствующий фрагмент из списка, щелкнув по ней")</span><span class="sxs-lookup"><span data-stu-id="c03ab-418">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-dependency-injection/_static/image24.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="c03ab-419">*Выберите соответствующий фрагмент из списка, щелкнув по ней*</span><span class="sxs-lookup"><span data-stu-id="c03ab-419">*Pick the relevant snippet from the list, by clicking on it*</span></span>
