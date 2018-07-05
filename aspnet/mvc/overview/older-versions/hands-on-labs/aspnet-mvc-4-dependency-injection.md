---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: Внедрение зависимостей в ASP.NET MVC 4 | Документация Майкрософт
author: rick-anderson
description: 'Примечание: Этот практический семинар предполагается, что у вас есть базовые знания о ASP.NET MVC и ASP.NET MVC 4 фильтры. Если вы не использовали фильтров ASP.NET MVC 4 до, мы rec...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 715444a6fbf491d7b99918294cfd2d0d0216cd09
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388048"
---
# <a name="aspnet-mvc-4-dependency-injection"></a><span data-ttu-id="27edd-104">Внедрение зависимостей в ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="27edd-104">ASP.NET MVC 4 Dependency Injection</span></span>

<span data-ttu-id="27edd-105">по [Web Слышатся Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="27edd-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="27edd-106">Скачайте комплект учебных материалов по лагеря Web</span><span class="sxs-lookup"><span data-stu-id="27edd-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="27edd-107">Это Практическое лабораторное занятие предполагается, что у вас есть базовые знания о **ASP.NET MVC** и **фильтров ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="27edd-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC** and **ASP.NET MVC 4 filters**.</span></span> <span data-ttu-id="27edd-108">Если вы не использовали **фильтров ASP.NET MVC 4** раньше, мы рекомендуем к превышению **настраиваемые действия ASP.NET MVC фильтры** Практическое лабораторное занятие.</span><span class="sxs-lookup"><span data-stu-id="27edd-108">If you have not used **ASP.NET MVC 4 filters** before, we recommend you to go over **ASP.NET MVC Custom Action Filters** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="27edd-109">Все примеры кода и фрагменты кода включены в Web лагеря комплект обучающих материалов, доступных из в [выпуски Microsoft Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="27edd-109">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="27edd-110">Проект, относящиеся к этой лаборатории доступен в [внедрения зависимостей ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="27edd-110">The project specific to this lab is available at [ASP.NET MVC 4 Dependency Injection](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span></span>

<span data-ttu-id="27edd-111">В **объектно-ориентированного программирования** парадигма, совместной работы объектов в модели совместной работы которых содержится участники и потребителей.</span><span class="sxs-lookup"><span data-stu-id="27edd-111">In **Object Oriented Programming** paradigm, objects work together in a collaboration model where there are contributors and consumers.</span></span> <span data-ttu-id="27edd-112">Естественно эта модель связи приводит к возникновению ошибки зависимости между объектами и компонентами, становится сложно управлять при увеличении сложности.</span><span class="sxs-lookup"><span data-stu-id="27edd-112">Naturally, this communication model generates dependencies between objects and components, becoming difficult to manage when complexity increases.</span></span>

<span data-ttu-id="27edd-113">![Класс зависимостей и сложность модели](aspnet-mvc-4-dependency-injection/_static/image1.png "класса зависимости и сложность модели")</span><span class="sxs-lookup"><span data-stu-id="27edd-113">![Class dependencies and model complexity](aspnet-mvc-4-dependency-injection/_static/image1.png "Class dependencies and model complexity")</span></span>

<span data-ttu-id="27edd-114">*Класс зависимостей и сложность модели*</span><span class="sxs-lookup"><span data-stu-id="27edd-114">*Class dependencies and model complexity*</span></span>

<span data-ttu-id="27edd-115">Вы, вероятно, слышали о **шаблон фабрики** и разделение между интерфейсом и реализации, с помощью служб, где клиентские объекты часто отвечают за расположение службы.</span><span class="sxs-lookup"><span data-stu-id="27edd-115">You have probably heard about the **Factory Pattern** and the separation between the interface and the implementation using services, where the client objects are often responsible for service location.</span></span>

<span data-ttu-id="27edd-116">Шаблон внедрения зависимостей является конкретной реализацией Inversion of Control.</span><span class="sxs-lookup"><span data-stu-id="27edd-116">The Dependency Injection pattern is a particular implementation of Inversion of Control.</span></span> <span data-ttu-id="27edd-117">**Инверсии управления (IoC)** означает, что объекты не создавайте другие объекты, на которые они используют для выполнения своих задач.</span><span class="sxs-lookup"><span data-stu-id="27edd-117">**Inversion of Control (IoC)** means that objects do not create other objects on which they rely to do their work.</span></span> <span data-ttu-id="27edd-118">Вместо этого они получают объекты, которые им необходимы из внешнего источника (например, файл конфигурации xml).</span><span class="sxs-lookup"><span data-stu-id="27edd-118">Instead, they get the objects that they need from an outside source (for example, an xml configuration file).</span></span>

<span data-ttu-id="27edd-119">**Внедрение зависимостей (DI)** означает, что это сделать без вмешательства объекта, как правило, компонентом платформы, который передает параметры конструктора и задайте свойства.</span><span class="sxs-lookup"><span data-stu-id="27edd-119">**Dependency Injection (DI)** means that this is done without the object intervention, usually by a framework component that passes constructor parameters and set properties.</span></span>

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a><span data-ttu-id="27edd-120">Шаблон разработки внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="27edd-120">The Dependency Injection (DI) Design Pattern</span></span>

<span data-ttu-id="27edd-121">На высоком уровне, внедрение зависимостей призвано, класс клиента (например *golfer*) требуется что-нибудь, удовлетворяющий интерфейс (например *IClub*).</span><span class="sxs-lookup"><span data-stu-id="27edd-121">At a high level, the goal of Dependency Injection is that a client class (e.g. *the golfer*) needs something that satisfies an interface (e.g. *IClub*).</span></span> <span data-ttu-id="27edd-122">Безразлично, что такое конкретный тип (например *WedgeClub WoodClub IronClub,* или *PutterClub*), ему кто-то другой для обработки, (например хорошее *caddy*).</span><span class="sxs-lookup"><span data-stu-id="27edd-122">It doesn't care what the concrete type is (e.g. *WoodClub, IronClub, WedgeClub* or *PutterClub*), it wants someone else to handle that (e.g. a good *caddy*).</span></span> <span data-ttu-id="27edd-123">Сопоставитель зависимостей в ASP.NET MVC можно позволяют регистрировать логику зависимостей где-то еще (например контейнер или *контейнер треф*).</span><span class="sxs-lookup"><span data-stu-id="27edd-123">The Dependency Resolver in ASP.NET MVC can allow you to register your dependency logic somewhere else (e.g. a container or a *bag of clubs*).</span></span>

<span data-ttu-id="27edd-124">![Схема внедрения зависимостей](aspnet-mvc-4-dependency-injection/_static/image2.png "рисунке внедрения зависимостей")</span><span class="sxs-lookup"><span data-stu-id="27edd-124">![Dependency Injection diagram](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection illustration")</span></span>

<span data-ttu-id="27edd-125">*Внедрение зависимостей - аналогию Golf*</span><span class="sxs-lookup"><span data-stu-id="27edd-125">*Dependency Injection - Golf analogy*</span></span>

<span data-ttu-id="27edd-126">Ниже перечислены преимущества использования шаблон внедрения зависимостей и инверсии управления.</span><span class="sxs-lookup"><span data-stu-id="27edd-126">The advantages of using Dependency Injection pattern and Inversion of Control are the following:</span></span>

- <span data-ttu-id="27edd-127">Уменьшает взаимозависимости классов</span><span class="sxs-lookup"><span data-stu-id="27edd-127">Reduces class coupling</span></span>
- <span data-ttu-id="27edd-128">Увеличивает повторное использование кода</span><span class="sxs-lookup"><span data-stu-id="27edd-128">Increases code reusing</span></span>
- <span data-ttu-id="27edd-129">Повышает удобство поддержки кода</span><span class="sxs-lookup"><span data-stu-id="27edd-129">Improves code maintainability</span></span>
- <span data-ttu-id="27edd-130">Улучшает тестирования приложений</span><span class="sxs-lookup"><span data-stu-id="27edd-130">Improves application testing</span></span>

> [!NOTE]
> <span data-ttu-id="27edd-131">Внедрение зависимостей иногда сравнивается с абстрактным фабричного конструктивного шаблона, но есть незначительные различия между оба подхода.</span><span class="sxs-lookup"><span data-stu-id="27edd-131">Dependency Injection is sometimes compared with Abstract Factory Design Pattern, but there is a slight difference between both approaches.</span></span> <span data-ttu-id="27edd-132">DI имеет структуру, работа за устранить зависимости, вызов фабрики и зарегистрированных служб.</span><span class="sxs-lookup"><span data-stu-id="27edd-132">DI has a Framework working behind to solve dependencies by calling the factories and the registered services.</span></span>


<span data-ttu-id="27edd-133">Теперь, когда вы знаете шаблон внедрения зависимостей, вы узнаете в этом лабораторном занятии как применять их в ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="27edd-133">Now that you understand the Dependency Injection Pattern, you will learn throughout this lab how to apply it in ASP.NET MVC 4.</span></span> <span data-ttu-id="27edd-134">Сначала с помощью внедрения зависимости в **контроллеров** включена служба доступа к базе данных.</span><span class="sxs-lookup"><span data-stu-id="27edd-134">You will start using Dependency Injection in the **Controllers** to include a database access service.</span></span> <span data-ttu-id="27edd-135">После этого будет применить внедрение зависимостей для **представления** для использования службы и отображения сведений.</span><span class="sxs-lookup"><span data-stu-id="27edd-135">Next, you will apply Dependency Injection to the **Views** to consume a service and show information.</span></span> <span data-ttu-id="27edd-136">Наконец будут расширены внедрения Зависимостей в ASP.NET MVC 4 фильтры, внедрение пользовательского фильтра действий в решении.</span><span class="sxs-lookup"><span data-stu-id="27edd-136">Finally, you will extend the DI to ASP.NET MVC 4 Filters, injecting a custom action filter in the solution.</span></span>

<span data-ttu-id="27edd-137">В этом Практическое лабораторное занятие, вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="27edd-137">In this Hands-on Lab, you will learn how to:</span></span>

- <span data-ttu-id="27edd-138">Интеграция ASP.NET MVC 4 с Unity для внедрения зависимостей, использование пакетов NuGet</span><span class="sxs-lookup"><span data-stu-id="27edd-138">Integrate ASP.NET MVC 4 with Unity for Dependency Injection using NuGet Packages</span></span>
- <span data-ttu-id="27edd-139">С помощью внедрения зависимостей в контроллер ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="27edd-139">Use Dependency Injection inside an ASP.NET MVC Controller</span></span>
- <span data-ttu-id="27edd-140">С помощью внедрения зависимостей в представления ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="27edd-140">Use Dependency Injection inside an ASP.NET MVC View</span></span>
- <span data-ttu-id="27edd-141">С помощью внедрения зависимостей в ASP.NET MVC фильтра действий</span><span class="sxs-lookup"><span data-stu-id="27edd-141">Use Dependency Injection inside an ASP.NET MVC Action Filter</span></span>

> [!NOTE]
> <span data-ttu-id="27edd-142">Это лабораторное занятие использует пакет NuGet Unity.Mvc3 для разрешения зависимостей, но можно адаптировать любой платформой внедрения зависимостей для работы с ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="27edd-142">This Lab is using Unity.Mvc3 NuGet Package for dependency resolution, but it is possible to adapt any Dependency Injection Framework to work with ASP.NET MVC 4.</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="27edd-143">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="27edd-143">Prerequisites</span></span>

<span data-ttu-id="27edd-144">Необходимо иметь следующие элементы во укомплектовать данную лабораторию:</span><span class="sxs-lookup"><span data-stu-id="27edd-144">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="27edd-145">[Microsoft Visual Studio Express 2012 для Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) или руководителя (чтение [приложение А](#AppendixA) инструкции по его установке).</span><span class="sxs-lookup"><span data-stu-id="27edd-145">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="27edd-146">Установка</span><span class="sxs-lookup"><span data-stu-id="27edd-146">Setup</span></span>

<span data-ttu-id="27edd-147">**Установка фрагменты кода**</span><span class="sxs-lookup"><span data-stu-id="27edd-147">**Installing Code Snippets**</span></span>

<span data-ttu-id="27edd-148">Для удобства большая часть кода, который вы планируете управлять вдоль этой лаборатории доступен как фрагменты кода Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="27edd-148">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="27edd-149">Чтобы установить фрагменты кода запуска **.\Source\Setup\CodeSnippets.vsi** файла.</span><span class="sxs-lookup"><span data-stu-id="27edd-149">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="27edd-150">Если вы не знакомы с фрагментов кода Visual Studio и хотите узнать о способах их использования, можно ссылаться в приложение из этого документа &quot; [с помощью фрагментов кода в приложении б](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="27edd-150">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="27edd-151">Упражнения</span><span class="sxs-lookup"><span data-stu-id="27edd-151">Exercises</span></span>

<span data-ttu-id="27edd-152">Это Практическое лабораторное занятие включает по следующие упражнения:</span><span class="sxs-lookup"><span data-stu-id="27edd-152">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="27edd-153">Упражнение 1: Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="27edd-153">Exercise 1: Injecting a Controller</span></span>](#Exercise1)
2. [<span data-ttu-id="27edd-154">Упражнение 2: Добавление представления</span><span class="sxs-lookup"><span data-stu-id="27edd-154">Exercise 2: Injecting a View</span></span>](#Exercise2)
3. [<span data-ttu-id="27edd-155">Упражнение 3: Добавление фильтров</span><span class="sxs-lookup"><span data-stu-id="27edd-155">Exercise 3: Injecting Filters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="27edd-156">Каждого упражнения сопровождается **окончания** папку, содержащую полученное решение, должен быть получен после завершения упражнения.</span><span class="sxs-lookup"><span data-stu-id="27edd-156">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="27edd-157">Это решение можно использовать как руководство, если вам нужна дополнительная помощь при работе с примерами.</span><span class="sxs-lookup"><span data-stu-id="27edd-157">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="27edd-158">Предполагаемое время для выполнения этого лабораторного: **30 минут**.</span><span class="sxs-lookup"><span data-stu-id="27edd-158">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a><span data-ttu-id="27edd-159">Упражнение 1: Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="27edd-159">Exercise 1: Injecting a Controller</span></span>

<span data-ttu-id="27edd-160">В этом упражнении вы узнаете, как использовать внедрение зависимостей в ASP.NET MVC Controllers, интеграция Unity с помощью пакета NuGet.</span><span class="sxs-lookup"><span data-stu-id="27edd-160">In this exercise, you will learn how to use Dependency Injection in ASP.NET MVC Controllers by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="27edd-161">По этой причине службы будет включать в свои контроллеры MvcMusicStore для отделения логики от доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="27edd-161">For that reason, you will include services into your MvcMusicStore controllers to separate the logic from the data access.</span></span> <span data-ttu-id="27edd-162">Службы создаст новую зависимость в конструктор контроллера, который будет разрешаться с использованием внедрения зависимостей с помощью **Unity**.</span><span class="sxs-lookup"><span data-stu-id="27edd-162">The services will create a new dependency in the controller constructor, which will be resolved using Dependency Injection with the help of **Unity**.</span></span>

<span data-ttu-id="27edd-163">Этот подход будет показано, как для создания менее связанных приложений, которые являются более гибкими и простыми в обслуживании и тестировании.</span><span class="sxs-lookup"><span data-stu-id="27edd-163">This approach will show you how to generate less coupled applications, which are more flexible and easier to maintain and test.</span></span> <span data-ttu-id="27edd-164">Вы также узнаете, как интегрировать ASP.NET MVC с помощью Unity.</span><span class="sxs-lookup"><span data-stu-id="27edd-164">You will also learn how to integrate ASP.NET MVC with Unity.</span></span>

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a><span data-ttu-id="27edd-165">Сведения о службе StoreManager</span><span class="sxs-lookup"><span data-stu-id="27edd-165">About StoreManager Service</span></span>

<span data-ttu-id="27edd-166">Music Store MVC, теперь предоставляются в решении begin содержит службу, которая управляет данными Store контроллер с именем **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="27edd-166">The MVC Music Store provided in the begin solution now includes a service that manages the Store Controller data named **StoreService**.</span></span> <span data-ttu-id="27edd-167">Ниже Вы найдете реализация службы Store.</span><span class="sxs-lookup"><span data-stu-id="27edd-167">Below you will find the Store Service implementation.</span></span> <span data-ttu-id="27edd-168">Обратите внимание, что все методы возвращают сущностей модели.</span><span class="sxs-lookup"><span data-stu-id="27edd-168">Note that all the methods return Model entities.</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

<span data-ttu-id="27edd-169">**StoreController** из begin решение использует **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="27edd-169">**StoreController** from the begin solution now consumes **StoreService**.</span></span> <span data-ttu-id="27edd-170">Все ссылки на данные были удалены из **StoreController**и теперь можно изменить текущий поставщик доступа к данным без изменения любой метод, который потребляет **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="27edd-170">All the data references were removed from **StoreController**, and now possible to modify the current data access provider without changing any method that consumes **StoreService**.</span></span>

<span data-ttu-id="27edd-171">Вы найдете ниже, **StoreController** реализация имеет зависимость от **StoreService** внутри конструктора класса.</span><span class="sxs-lookup"><span data-stu-id="27edd-171">You will find below that the **StoreController** implementation has a dependency with **StoreService** inside the class constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="27edd-172">Зависимости, представленные в этом упражнении связана с **Inversion of Control** (IoC).</span><span class="sxs-lookup"><span data-stu-id="27edd-172">The dependency introduced in this exercise is related to **Inversion of Control** (IoC).</span></span>
> 
> <span data-ttu-id="27edd-173">**StoreController** конструктор класса получает **IStoreService** параметру типа, который необходим для выполнения вызовов службы от внутри класса.</span><span class="sxs-lookup"><span data-stu-id="27edd-173">The **StoreController** class constructor receives an **IStoreService** type parameter, which is essential to perform service calls from inside the class.</span></span> <span data-ttu-id="27edd-174">Тем не менее **StoreController** не реализован конструктор по умолчанию (с без параметров), любой контроллер для работы с ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="27edd-174">However, **StoreController** does not implement the default constructor (with no parameters) that any controller must have to work with ASP.NET MVC.</span></span>
> 
> <span data-ttu-id="27edd-175">Чтобы устранить зависимость, контроллер может создаваться фабрикой абстрактным (класс, который возвращает любой объект указанного типа).</span><span class="sxs-lookup"><span data-stu-id="27edd-175">To resolve the dependency, the controller has to be created by an abstract factory (a class that returns any object of the specified type).</span></span>


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="27edd-176">Вы получите ошибку при попытке создать StoreController без отправки объекта службы, так как не объявлен конструктор класса.</span><span class="sxs-lookup"><span data-stu-id="27edd-176">You will get an error when the class tries to create the StoreController without sending the service object, as there is no parameterless constructor declared.</span></span>


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a><span data-ttu-id="27edd-177">Задача 1 - Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="27edd-177">Task 1 - Running the Application</span></span>

<span data-ttu-id="27edd-178">В этой задаче будет выполняться приложения Begin, который включает в себя службу в контроллер Store отделяет доступа к данным от логики приложения.</span><span class="sxs-lookup"><span data-stu-id="27edd-178">In this task, you will run the Begin application, which includes the service into the Store Controller that separates the data access from the application logic.</span></span>

<span data-ttu-id="27edd-179">При запуске приложения, вы получите исключение, так как служба контроллера не передается в качестве параметра по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="27edd-179">When running the application, you will receive an exception, as the controller service is not passed as a parameter by default:</span></span>

1. <span data-ttu-id="27edd-180">Откройте **начать** решение находится в **внедрение Source\Ex01 Controller\Begin**.</span><span class="sxs-lookup"><span data-stu-id="27edd-180">Open the **Begin** solution located in **Source\Ex01-Injecting Controller\Begin**.</span></span>

   1. <span data-ttu-id="27edd-181">Необходимо будет загрузить некоторые отсутствующие пакеты NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="27edd-181">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="27edd-182">Чтобы сделать это, нажмите кнопку **проекта** меню и выберите **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="27edd-182">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="27edd-183">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="27edd-183">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="27edd-184">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="27edd-184">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="27edd-185">Одним из преимуществ использования NuGet является отсутствие поставлять все библиотеки в проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="27edd-185">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="27edd-186">С помощью NuGet Power Tools путем указания версий пакета в файле Packages.config можно для скачивания всех необходимых библиотек при первом запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="27edd-186">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="27edd-187">Вот почему необходимо выполните описанные выше действия, после открытия существующего решения из этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="27edd-187">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="27edd-188">Нажмите клавишу **Ctrl + F5** для запуска приложения без отладки.</span><span class="sxs-lookup"><span data-stu-id="27edd-188">Press **Ctrl + F5** to run the application without debugging.</span></span> <span data-ttu-id="27edd-189">Вы получите сообщение об ошибке &quot; **нет конструктора без параметров, определенных для данного объекта**&quot;:</span><span class="sxs-lookup"><span data-stu-id="27edd-189">You will get the error message &quot;**No parameterless constructor defined for this object**&quot;:</span></span>

    <span data-ttu-id="27edd-190">![Ошибка при выполнении приложения начать ASP.NET MVC](aspnet-mvc-4-dependency-injection/_static/image3.png "ошибка во время выполнения приложение начать ASP.NET MVC")</span><span class="sxs-lookup"><span data-stu-id="27edd-190">![Error while running ASP.NET MVC Begin Application](aspnet-mvc-4-dependency-injection/_static/image3.png "Error while running ASP.NET MVC Begin Application")</span></span>

    <span data-ttu-id="27edd-191">*Ошибка при выполнении приложения начать ASP.NET MVC*</span><span class="sxs-lookup"><span data-stu-id="27edd-191">*Error while running ASP.NET MVC Begin Application*</span></span>
3. <span data-ttu-id="27edd-192">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="27edd-192">Close the browser.</span></span>

<span data-ttu-id="27edd-193">В следующих шагах будет работать решение Music Store для внедрения зависимости, необходимые для этого контроллера.</span><span class="sxs-lookup"><span data-stu-id="27edd-193">In the following steps you will work on the Music Store Solution to inject the dependency this controller needs.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a><span data-ttu-id="27edd-194">Задача 2 - включая Unity в решение MvcMusicStore</span><span class="sxs-lookup"><span data-stu-id="27edd-194">Task 2 - Including Unity into MvcMusicStore Solution</span></span>

<span data-ttu-id="27edd-195">В этой задаче будет включать **Unity.Mvc3** пакет NuGet для решения.</span><span class="sxs-lookup"><span data-stu-id="27edd-195">In this task, you will include **Unity.Mvc3** NuGet Package to the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="27edd-196">Пакет Unity.Mvc3 был разработан для ASP.NET MVC 3, но она полностью совместима с ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="27edd-196">Unity.Mvc3 package was designed for ASP.NET MVC 3, but it is fully compatible with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="27edd-197">Unity — это контейнер внедрения зависимостей облегченный и расширяемый с дополнительной поддержкой для экземпляра и типа.</span><span class="sxs-lookup"><span data-stu-id="27edd-197">Unity is a lightweight, extensible dependency injection container with optional support for instance and type interception.</span></span> <span data-ttu-id="27edd-198">Это контейнер общего назначения для использования в приложении .NET любого типа.</span><span class="sxs-lookup"><span data-stu-id="27edd-198">It is a general-purpose container for use in any type of .NET application.</span></span> <span data-ttu-id="27edd-199">Он предоставляет общие возможности, в том числе механизмы внедрения зависимостей: создание объектов, абстракции требований, указав зависимости во время выполнения и гибкость, за счет отсрочки конфигурация компонента в контейнер.</span><span class="sxs-lookup"><span data-stu-id="27edd-199">It provides all the common features found in dependency injection mechanisms including: object creation, abstraction of requirements by specifying dependencies at runtime and flexibility, by deferring the component configuration to the container.</span></span>


1. <span data-ttu-id="27edd-200">Установка **Unity.Mvc3** пакет NuGet в **MvcMusicStore** проекта.</span><span class="sxs-lookup"><span data-stu-id="27edd-200">Install **Unity.Mvc3** NuGet Package in the **MvcMusicStore** project.</span></span> <span data-ttu-id="27edd-201">Чтобы сделать это, откройте **консоль диспетчера пакетов** из **представление** | **Other Windows**.</span><span class="sxs-lookup"><span data-stu-id="27edd-201">To do this, open the **Package Manager Console** from **View** | **Other Windows**.</span></span>
2. <span data-ttu-id="27edd-202">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="27edd-202">Run the following command.</span></span>

    <span data-ttu-id="27edd-203">PMC</span><span class="sxs-lookup"><span data-stu-id="27edd-203">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    <span data-ttu-id="27edd-204">![Установка пакета NuGet Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "установки пакета NuGet Unity.Mvc3")</span><span class="sxs-lookup"><span data-stu-id="27edd-204">![Installing Unity.Mvc3 NuGet Package](aspnet-mvc-4-dependency-injection/_static/image4.png "Installing Unity.Mvc3 NuGet Package")</span></span>

    <span data-ttu-id="27edd-205">*Установка пакета NuGet Unity.Mvc3*</span><span class="sxs-lookup"><span data-stu-id="27edd-205">*Installing Unity.Mvc3 NuGet Package*</span></span>
3. <span data-ttu-id="27edd-206">Один раз **Unity.Mvc3** установки пакета, просмотрите файлы и папки, она автоматически добавляется для упрощения конфигурации Unity.</span><span class="sxs-lookup"><span data-stu-id="27edd-206">Once the **Unity.Mvc3** package is installed, explore the files and folders it automatically adds in order to simplify Unity configuration.</span></span>

    <span data-ttu-id="27edd-207">![Установленный пакет Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 пакета")</span><span class="sxs-lookup"><span data-stu-id="27edd-207">![Unity.Mvc3 package installed](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 package installed")</span></span>

    <span data-ttu-id="27edd-208">*Установленный пакет Unity.Mvc3*</span><span class="sxs-lookup"><span data-stu-id="27edd-208">*Unity.Mvc3 package installed*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a><span data-ttu-id="27edd-209">Задача 3 - регистрация Unity в приложении Global.asax.cs\_запуск</span><span class="sxs-lookup"><span data-stu-id="27edd-209">Task 3 - Registering Unity in Global.asax.cs Application\_Start</span></span>

<span data-ttu-id="27edd-210">В этой задаче вы обновите **приложения\_запустить** метод находится в **Global.asax.cs** для вызова инициализатор загрузчика Unity и затем обновить регистрацию файл начального загрузчика Службы и контроллер, будет использоваться для внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="27edd-210">In this task, you will update the **Application\_Start** method located in **Global.asax.cs** to call the Unity Bootstrapper initializer and then, update the Bootstrapper file registering the Service and Controller you will use for Dependency Injection.</span></span>

1. <span data-ttu-id="27edd-211">Теперь будет подключить загрузчика, — это файл, который инициализирует контейнера Unity и Сопоставитель зависимостей.</span><span class="sxs-lookup"><span data-stu-id="27edd-211">Now, you will hook up the Bootstrapper which is the file that initializes the Unity container and Dependency Resolver.</span></span> <span data-ttu-id="27edd-212">Чтобы сделать это, откройте **Global.asax.cs** и добавьте следующий выделенный код в **приложения\_запустить** метод.</span><span class="sxs-lookup"><span data-stu-id="27edd-212">To do this, open **Global.asax.cs** and add the following highlighted code within the **Application\_Start** method.</span></span>

    <span data-ttu-id="27edd-213">(Code Snippet - *лаборатории внедрения зависимостей ASP.NET - Ex01 - инициализировать Unity*)</span><span class="sxs-lookup"><span data-stu-id="27edd-213">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Initialize Unity*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. <span data-ttu-id="27edd-214">Откройте **Bootstrapper.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="27edd-214">Open **Bootstrapper.cs** file.</span></span>
3. <span data-ttu-id="27edd-215">Включите следующие пространства имен: **MvcMusicStore.Services** и **MusicStore.Controllers**.</span><span class="sxs-lookup"><span data-stu-id="27edd-215">Include the following namespaces: **MvcMusicStore.Services** and **MusicStore.Controllers**.</span></span>

    <span data-ttu-id="27edd-216">(Code Snippet - *ASP.NET внедрения зависимостей лаборатории - Ex01 - загрузчик, добавление пространств имен*)</span><span class="sxs-lookup"><span data-stu-id="27edd-216">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. <span data-ttu-id="27edd-217">Замените **BuildUnityContainer** метод содержимого следующим кодом, который регистрирует Store контроллера и службы Store.</span><span class="sxs-lookup"><span data-stu-id="27edd-217">Replace **BuildUnityContainer** method's content with the following code that registers Store Controller and Store Service.</span></span>

    <span data-ttu-id="27edd-218">(Code Snippet - *ASP.NET зависимостей путем внедрения кода лаборатории - Ex01 - Store Register контроллера и службы*)</span><span class="sxs-lookup"><span data-stu-id="27edd-218">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller and Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="27edd-219">Задача 4 - запуск приложения</span><span class="sxs-lookup"><span data-stu-id="27edd-219">Task 4 - Running the Application</span></span>

<span data-ttu-id="27edd-220">В этой задаче будет выполняться приложение, чтобы убедиться, что его можно загрузить теперь после включения Unity.</span><span class="sxs-lookup"><span data-stu-id="27edd-220">In this task, you will run the application to verify that it can now be loaded after including Unity.</span></span>

1. <span data-ttu-id="27edd-221">Нажмите клавишу **F5** для запуска приложения, приложения должен загружаться без отображения сообщения об ошибке.</span><span class="sxs-lookup"><span data-stu-id="27edd-221">Press **F5** to run the application, the application should now load without showing any error message.</span></span>

    <span data-ttu-id="27edd-222">![Запуск приложения с помощью внедрения зависимостей](aspnet-mvc-4-dependency-injection/_static/image6.png "запуск приложения с помощью внедрения зависимостей")</span><span class="sxs-lookup"><span data-stu-id="27edd-222">![Running Application with Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Running Application with Dependency Injection")</span></span>

    <span data-ttu-id="27edd-223">*Запуск приложения с помощью внедрения зависимостей*</span><span class="sxs-lookup"><span data-stu-id="27edd-223">*Running Application with Dependency Injection*</span></span>
2. <span data-ttu-id="27edd-224">Перейдите к **/Store**.</span><span class="sxs-lookup"><span data-stu-id="27edd-224">Browse to **/Store**.</span></span> <span data-ttu-id="27edd-225">Это вызовет **StoreController**, который создается с помощью **Unity**.</span><span class="sxs-lookup"><span data-stu-id="27edd-225">This will invoke **StoreController**, which is now created using **Unity**.</span></span>

    <span data-ttu-id="27edd-226">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span><span class="sxs-lookup"><span data-stu-id="27edd-226">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span></span>

    <span data-ttu-id="27edd-227">*MVC Music Store*</span><span class="sxs-lookup"><span data-stu-id="27edd-227">*MVC Music Store*</span></span>
3. <span data-ttu-id="27edd-228">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="27edd-228">Close the browser.</span></span>

<span data-ttu-id="27edd-229">В следующих упражнениях вы узнаете, как расширить область внедрения зависимостей, чтобы использовать его внутри представлений MVC ASP.NET и фильтры действий.</span><span class="sxs-lookup"><span data-stu-id="27edd-229">In the following exercises you will learn how to extend the Dependency Injection scope to use it inside ASP.NET MVC Views and Action Filters.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a><span data-ttu-id="27edd-230">Упражнение 2: Добавление представления</span><span class="sxs-lookup"><span data-stu-id="27edd-230">Exercise 2: Injecting a View</span></span>

<span data-ttu-id="27edd-231">В этом упражнении вы узнаете, как использовать внедрение зависимостей в представления с новыми функциями ASP.NET MVC 4 для интеграции с Unity.</span><span class="sxs-lookup"><span data-stu-id="27edd-231">In this exercise, you will learn how to use Dependency Injection in a view with the new features of ASP.NET MVC 4 for Unity integration.</span></span> <span data-ttu-id="27edd-232">Для этого будет вызывать пользовательскую службу внутри представление Обзор Store, которое будет отображаться сообщение и изображение ниже.</span><span class="sxs-lookup"><span data-stu-id="27edd-232">In order to do that, you will call a custom service inside the Store Browse View, which will show a message and an image below.</span></span>

<span data-ttu-id="27edd-233">Затем следует интегрировать с помощью Unity и создать пользовательскую зависимость Сопоставитель для внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="27edd-233">Then, you will integrate the project with Unity and create a custom dependency resolver to inject the dependencies.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a><span data-ttu-id="27edd-234">Задача 1 - Создание представления, использующее службу</span><span class="sxs-lookup"><span data-stu-id="27edd-234">Task 1 - Creating a View that Consumes a Service</span></span>

<span data-ttu-id="27edd-235">В этой задаче вы создадите представление, которое выполняет вызов службы для создания новой зависимости.</span><span class="sxs-lookup"><span data-stu-id="27edd-235">In this task, you will create a view that performs a service call to generate a new dependency.</span></span> <span data-ttu-id="27edd-236">Служба состоит в простую службу обмена сообщениями, входят в это решение.</span><span class="sxs-lookup"><span data-stu-id="27edd-236">The service consists in a simple messaging service included in this solution.</span></span>

1. <span data-ttu-id="27edd-237">Откройте **начать** решение находится в **внедрение Source\Ex02 View\Begin** папки.</span><span class="sxs-lookup"><span data-stu-id="27edd-237">Open the **Begin** solution located in the **Source\Ex02-Injecting View\Begin** folder.</span></span> <span data-ttu-id="27edd-238">В противном случае можно продолжить использование **окончания** решение получен путем выполнения предыдущего упражнения.</span><span class="sxs-lookup"><span data-stu-id="27edd-238">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="27edd-239">Если вы открыли предоставленный **начать** решение, необходимо будет загрузить некоторые отсутствующие пакеты NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="27edd-239">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="27edd-240">Чтобы сделать это, нажмите кнопку **проекта** меню и выберите **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="27edd-240">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="27edd-241">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="27edd-241">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="27edd-242">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="27edd-242">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="27edd-243">Одним из преимуществ использования NuGet является отсутствие поставлять все библиотеки в проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="27edd-243">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="27edd-244">С помощью NuGet Power Tools путем указания версий пакета в файле Packages.config можно для скачивания всех необходимых библиотек при первом запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="27edd-244">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="27edd-245">Вот почему необходимо выполните описанные выше действия, после открытия существующего решения из этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="27edd-245">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="27edd-246">Дополнительные сведения см. в этой статье: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="27edd-246">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="27edd-247">Включить **MessageService.cs** и **IMessageService.cs** классы находятся в **источника \Assets** папку в **/службы**.</span><span class="sxs-lookup"><span data-stu-id="27edd-247">Include the **MessageService.cs** and the **IMessageService.cs** classes located in the **Source \Assets** folder in **/Services**.</span></span> <span data-ttu-id="27edd-248">Чтобы сделать это, щелкните правой кнопкой мыши **служб** папку и выберите **добавить существующий элемент**.</span><span class="sxs-lookup"><span data-stu-id="27edd-248">To do this, right-click **Services** folder and select **Add Existing Item**.</span></span> <span data-ttu-id="27edd-249">Перейдите в расположение файлов и включать их.</span><span class="sxs-lookup"><span data-stu-id="27edd-249">Browse to the files' location and include them.</span></span>

    <span data-ttu-id="27edd-250">![Добавление службы сообщений и интерфейс службы](aspnet-mvc-4-dependency-injection/_static/image8.png "Добавление службы сообщений и интерфейс службы")</span><span class="sxs-lookup"><span data-stu-id="27edd-250">![Adding Message Service and Service Interface](aspnet-mvc-4-dependency-injection/_static/image8.png "Adding Message Service and Service Interface")</span></span>

    <span data-ttu-id="27edd-251">*Добавление службы сообщений и интерфейс службы*</span><span class="sxs-lookup"><span data-stu-id="27edd-251">*Adding Message Service and Service Interface*</span></span>

    > [!NOTE]
    > <span data-ttu-id="27edd-252">**IMessageService** интерфейс определяет два свойства, реализуемый **MessageService** класса.</span><span class="sxs-lookup"><span data-stu-id="27edd-252">The **IMessageService** interface defines two properties implemented by the **MessageService** class.</span></span> <span data-ttu-id="27edd-253">Эти свойства —**сообщение** и **ImageUrl**-хранить сообщение и URL-адрес изображения для отображения.</span><span class="sxs-lookup"><span data-stu-id="27edd-253">These properties -**Message** and **ImageUrl**- store the message and the URL of the image to be displayed.</span></span>
3. <span data-ttu-id="27edd-254">Создать папку **/Pages** в проекте корневой папки, а затем добавьте существующий класс **MyBasePage.cs** из **Source\Assets**.</span><span class="sxs-lookup"><span data-stu-id="27edd-254">Create the folder **/Pages** in the project's root folder, and then add the existing class **MyBasePage.cs** from **Source\Assets**.</span></span> <span data-ttu-id="27edd-255">Основной страницы, который будет наследовать от имеет следующую структуру.</span><span class="sxs-lookup"><span data-stu-id="27edd-255">The base page you will inherit from has the following structure.</span></span>

    <span data-ttu-id="27edd-256">![Папка страниц](aspnet-mvc-4-dependency-injection/_static/image9.png "папка страниц")</span><span class="sxs-lookup"><span data-stu-id="27edd-256">![Pages folder](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages folder")</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. <span data-ttu-id="27edd-257">Откройте **Browse.cshtml** просмотра из **/Views/Store** папки и сделать ее от **MyBasePage.cs**.</span><span class="sxs-lookup"><span data-stu-id="27edd-257">Open **Browse.cshtml** view from **/Views/Store** folder, and make it inherit from **MyBasePage.cs**.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. <span data-ttu-id="27edd-258">В **Обзор** просмотреть, добавить вызов **MessageService** для отображения изображения и сообщение извлечено службой.</span><span class="sxs-lookup"><span data-stu-id="27edd-258">In the **Browse** view, add a call to **MessageService** to display an image and a message retrieved by the service.</span></span>
   <span data-ttu-id="27edd-259">(C#)</span><span class="sxs-lookup"><span data-stu-id="27edd-259">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a><span data-ttu-id="27edd-260">Задача 2 - включая арбитра пользовательскую зависимость и активатор страницы представления</span><span class="sxs-lookup"><span data-stu-id="27edd-260">Task 2 - Including a Custom Dependency Resolver and a Custom View Page Activator</span></span>

<span data-ttu-id="27edd-261">В предыдущей задаче вы добавили новую зависимость внутри представления для выполнения вызова службы внутри него.</span><span class="sxs-lookup"><span data-stu-id="27edd-261">In the previous task, you injected a new dependency inside a view to perform a service call inside it.</span></span> <span data-ttu-id="27edd-262">Теперь эту зависимость разрешит путем реализации интерфейсов внедрения зависимостей ASP.NET MVC **IViewPageActivator** и **IDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="27edd-262">Now, you will resolve that dependency by implementing the ASP.NET MVC Dependency Injection interfaces **IViewPageActivator** and **IDependencyResolver**.</span></span> <span data-ttu-id="27edd-263">В решении будет включать реализацию **IDependencyResolver** , обеспечит получение службы с помощью Unity.</span><span class="sxs-lookup"><span data-stu-id="27edd-263">You will include in the solution an implementation of **IDependencyResolver** that will deal with the service retrieval by using Unity.</span></span> <span data-ttu-id="27edd-264">То, будет включать другой пользовательской реализации **IViewPageActivator** интерфейс, который решит создавать представления.</span><span class="sxs-lookup"><span data-stu-id="27edd-264">Then, you will include another custom implementation of **IViewPageActivator** interface that will solve the creation of the views.</span></span>

> [!NOTE]
> <span data-ttu-id="27edd-265">С момента ASP.NET MVC 3 реализацию для внедрения зависимостей упрощена интерфейсы для регистрации службы.</span><span class="sxs-lookup"><span data-stu-id="27edd-265">Since ASP.NET MVC 3, the implementation for Dependency Injection had simplified the interfaces to register services.</span></span> <span data-ttu-id="27edd-266">**IDependencyResolver** и **IViewPageActivator** являются частью функции ASP.NET MVC 3 для внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="27edd-266">**IDependencyResolver** and **IViewPageActivator** are part of ASP.NET MVC 3 features for Dependency Injection.</span></span>
> 
> <span data-ttu-id="27edd-267">**-IDependencyResolver** интерфейс заменяет предыдущие IMvcServiceLocator.</span><span class="sxs-lookup"><span data-stu-id="27edd-267">**- IDependencyResolver** interface replaces the previous IMvcServiceLocator.</span></span> <span data-ttu-id="27edd-268">Объекты, реализующие IDependencyResolver должен возвращать экземпляр службы или службы коллекции.</span><span class="sxs-lookup"><span data-stu-id="27edd-268">Implementers of IDependencyResolver must return an instance of the service or a service collection.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> <span data-ttu-id="27edd-269">**-IViewPageActivator** интерфейс обеспечивает более точный контроль над как просмотреть страницы создаются посредством внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="27edd-269">**- IViewPageActivator** interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="27edd-270">Классы, реализующие **IViewPageActivator** интерфейс можно создавать экземпляры представления, используя информацию о контексте.</span><span class="sxs-lookup"><span data-stu-id="27edd-270">The classes that implement **IViewPageActivator** interface can create view instances using context information.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. <span data-ttu-id="27edd-271">Создать /**фабрик** папку в корневой папке проекта.</span><span class="sxs-lookup"><span data-stu-id="27edd-271">Create the /**Factories** folder in the project's root folder.</span></span>
2. <span data-ttu-id="27edd-272">Включить **CustomViewPageActivator.cs** в решение из **/источники/ресурсы/** для **фабрик** папки.</span><span class="sxs-lookup"><span data-stu-id="27edd-272">Include **CustomViewPageActivator.cs** to your solution from **/Sources/Assets/** to **Factories** folder.</span></span> <span data-ttu-id="27edd-273">Чтобы сделать это, щелкните правой кнопкой мыши **/Factories** папку, выберите **Add | Существующий элемент** , а затем выберите **CustomViewPageActivator.cs**.</span><span class="sxs-lookup"><span data-stu-id="27edd-273">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **CustomViewPageActivator.cs**.</span></span> <span data-ttu-id="27edd-274">Этот класс реализует **IViewPageActivator** интерфейс для хранения контейнера Unity.</span><span class="sxs-lookup"><span data-stu-id="27edd-274">This class implements the **IViewPageActivator** interface to hold the Unity Container.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="27edd-275">**CustomViewPageActivator** отвечает за управление созданием представления с помощью Unity-контейнера.</span><span class="sxs-lookup"><span data-stu-id="27edd-275">**CustomViewPageActivator** is responsible for managing the creation of a view by using a Unity container.</span></span>
3. <span data-ttu-id="27edd-276">Включить **UnityDependencyResolver.cs** файла из **/источники/активы** для **/Factories** папки.</span><span class="sxs-lookup"><span data-stu-id="27edd-276">Include **UnityDependencyResolver.cs** file from **/Sources/Assets** to **/Factories** folder.</span></span> <span data-ttu-id="27edd-277">Чтобы сделать это, щелкните правой кнопкой мыши **/Factories** папку, выберите **Add | Существующий элемент** , а затем выберите **UnityDependencyResolver.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="27edd-277">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **UnityDependencyResolver.cs** file.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="27edd-278">**UnityDependencyResolver** класс является пользовательских DependencyResolver для Unity.</span><span class="sxs-lookup"><span data-stu-id="27edd-278">**UnityDependencyResolver** class is a custom DependencyResolver for Unity.</span></span> <span data-ttu-id="27edd-279">Если службу не удается найти внутри контейнера Unity, является invocated базового сопоставителя.</span><span class="sxs-lookup"><span data-stu-id="27edd-279">When a service cannot be found inside the Unity container, the base resolver is invocated.</span></span>

<span data-ttu-id="27edd-280">В следующей задаче будет зарегистрирован обе реализации позволяет знать расположение службы и представления модели.</span><span class="sxs-lookup"><span data-stu-id="27edd-280">In the following task both implementations will be registered to let the model know the location of the services and the views.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a><span data-ttu-id="27edd-281">Задача 3 - регистрация для внедрения зависимости в пределах контейнера Unity</span><span class="sxs-lookup"><span data-stu-id="27edd-281">Task 3 - Registering for Dependency Injection within Unity container</span></span>

<span data-ttu-id="27edd-282">В этой задаче будет поместить все предыдущие друг с другом, чтобы сделать работать внедрения зависимостей, что.</span><span class="sxs-lookup"><span data-stu-id="27edd-282">In this task, you will put all the previous things together to make Dependency Injection work.</span></span>

<span data-ttu-id="27edd-283">Пока решение содержит следующие элементы:</span><span class="sxs-lookup"><span data-stu-id="27edd-283">Up to now your solution has the following elements:</span></span>

- <span data-ttu-id="27edd-284">Объект **Обзор** представление, которое наследует от **MyBaseClass с помощью модификатора** и использует **MessageService**.</span><span class="sxs-lookup"><span data-stu-id="27edd-284">A **Browse** View that inherits from **MyBaseClass** and consumes **MessageService**.</span></span>
- <span data-ttu-id="27edd-285">Промежуточный класс -**MyBaseClass с помощью модификатора**-с внедрения зависимостей, объявленным для интерфейса службы.</span><span class="sxs-lookup"><span data-stu-id="27edd-285">An intermediate class -**MyBaseClass**- that has dependency injection declared for the service interface.</span></span>
- <span data-ttu-id="27edd-286">Службы - **MessageService** - и его интерфейс **IMessageService**.</span><span class="sxs-lookup"><span data-stu-id="27edd-286">A service - **MessageService** - and its interface **IMessageService**.</span></span>
- <span data-ttu-id="27edd-287">Сопоставитель пользовательскую зависимость для Unity — **UnityDependencyResolver** -имеет дело с получения службы.</span><span class="sxs-lookup"><span data-stu-id="27edd-287">A custom dependency resolver for Unity - **UnityDependencyResolver** - that deals with service retrieval.</span></span>
- <span data-ttu-id="27edd-288">Активатор страницы представления - **CustomViewPageActivator** -, создает страницу.</span><span class="sxs-lookup"><span data-stu-id="27edd-288">A View Page activator - **CustomViewPageActivator** - that creates the page.</span></span>

<span data-ttu-id="27edd-289">Чтобы внедрить **Обзор** представление, вы теперь регистрирует Сопоставитель пользовательскую зависимость в контейнера Unity.</span><span class="sxs-lookup"><span data-stu-id="27edd-289">To inject **Browse** View, you will now register the custom dependency resolver in the Unity container.</span></span>

1. <span data-ttu-id="27edd-290">Откройте **Bootstrapper.cs** файла.</span><span class="sxs-lookup"><span data-stu-id="27edd-290">Open **Bootstrapper.cs** file.</span></span>
2. <span data-ttu-id="27edd-291">Регистрация экземпляра **MessageService** в контейнер Unity инициализировать службу:</span><span class="sxs-lookup"><span data-stu-id="27edd-291">Register an instance of **MessageService** into the Unity container to initialize the service:</span></span>

    <span data-ttu-id="27edd-292">(Code Snippet - *службу сообщения Register лаборатории - Ex02 - внедрения зависимостей ASP.NET*)</span><span class="sxs-lookup"><span data-stu-id="27edd-292">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. <span data-ttu-id="27edd-293">Добавьте ссылку на **MvcMusicStore.Factories** пространства имен.</span><span class="sxs-lookup"><span data-stu-id="27edd-293">Add a reference to **MvcMusicStore.Factories** namespace.</span></span>

    <span data-ttu-id="27edd-294">(Code Snippet - *пространство имен фабрик лаборатории - Ex02 - внедрения зависимостей ASP.NET*)</span><span class="sxs-lookup"><span data-stu-id="27edd-294">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Factories Namespace*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. <span data-ttu-id="27edd-295">Зарегистрировать **CustomViewPageActivator** как активатор страницы представления в контейнер Unity:</span><span class="sxs-lookup"><span data-stu-id="27edd-295">Register **CustomViewPageActivator** as a View Page activator into the Unity container:</span></span>

    <span data-ttu-id="27edd-296">(Code Snippet - *ASP.NET зависимостей путем внедрения кода лаборатории - Ex02 - Register CustomViewPageActivator*)</span><span class="sxs-lookup"><span data-stu-id="27edd-296">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. <span data-ttu-id="27edd-297">Замените экземпляр сопоставителя зависимостей по умолчанию ASP.NET MVC 4 **UnityDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="27edd-297">Replace ASP.NET MVC 4 default dependency resolver with an instance of **UnityDependencyResolver**.</span></span> <span data-ttu-id="27edd-298">Чтобы сделать это, замените **Initialise** метод содержимого следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="27edd-298">To do this, replace **Initialise** method content with the following code:</span></span>

    <span data-ttu-id="27edd-299">(Code Snippet - *ASP.NET зависимостей путем внедрения кода лаборатории - Ex02 - обновления Сопоставитель зависимостей*)</span><span class="sxs-lookup"><span data-stu-id="27edd-299">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Update Dependency Resolver*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="27edd-300">ASP.NET MVC предоставляет класс Сопоставитель зависимостей по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="27edd-300">ASP.NET MVC provides a default dependency resolver class.</span></span> <span data-ttu-id="27edd-301">Для работы с сопоставителей пользовательских зависимостей, которое мы создали для unity, должно быть заменено данным сопоставителем.</span><span class="sxs-lookup"><span data-stu-id="27edd-301">To work with custom dependency resolvers as the one we have created for unity, this resolver has to be replaced.</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="27edd-302">Задача 4 - запуск приложения</span><span class="sxs-lookup"><span data-stu-id="27edd-302">Task 4 - Running the Application</span></span>

<span data-ttu-id="27edd-303">В этой задаче будет выполняться приложение, чтобы убедиться, что браузер Store использует службу и показано изображение и сообщение, полученное:</span><span class="sxs-lookup"><span data-stu-id="27edd-303">In this task, you will run the application to verify that the Store Browser consumes the service and shows the image and the message retrieved:</span></span>

1. <span data-ttu-id="27edd-304">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="27edd-304">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="27edd-305">Нажмите кнопку **рок** в меню жанров и см. в разделе как **MessageService** был внедрен в представление и загружаются приветственное сообщение и изображения.</span><span class="sxs-lookup"><span data-stu-id="27edd-305">Click **Rock** within the Genres Menu and see how the **MessageService** was injected to the view and loaded the welcome message and the image.</span></span> <span data-ttu-id="27edd-306">В этом примере выполняется переход к &quot; **рок**&quot;:</span><span class="sxs-lookup"><span data-stu-id="27edd-306">In this example, we are entering to &quot;**Rock**&quot;:</span></span>

    <span data-ttu-id="27edd-307">![MVC Music Store - внедрения представления](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - внедрения представления")</span><span class="sxs-lookup"><span data-stu-id="27edd-307">![MVC Music Store - View Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - View Injection")</span></span>

    <span data-ttu-id="27edd-308">*MVC Music Store - внедрения представления*</span><span class="sxs-lookup"><span data-stu-id="27edd-308">*MVC Music Store - View Injection*</span></span>
3. <span data-ttu-id="27edd-309">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="27edd-309">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a><span data-ttu-id="27edd-310">Упражнение 3: Добавление фильтров действий</span><span class="sxs-lookup"><span data-stu-id="27edd-310">Exercise 3: Injecting Action Filters</span></span>

<span data-ttu-id="27edd-311">В предыдущей практической работу **настраиваемые фильтры действий** вы работали с настройки фильтров и внедрения.</span><span class="sxs-lookup"><span data-stu-id="27edd-311">In the previous Hands-On lab **Custom Action Filters** you have worked with filters customization and injection.</span></span> <span data-ttu-id="27edd-312">В этом упражнении вы узнаете, как внедрить фильтры с помощью внедрения зависимостей с помощью контейнера Unity.</span><span class="sxs-lookup"><span data-stu-id="27edd-312">In this exercise, you will learn how to inject filters with Dependency Injection by using the Unity container.</span></span> <span data-ttu-id="27edd-313">Чтобы сделать это, будет добавлен в решение Music Store пользовательского фильтра действий, будет отслеживать веб-узла.</span><span class="sxs-lookup"><span data-stu-id="27edd-313">To do that, you will add to the Music Store solution a custom action filter that will trace the activity of the site.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a><span data-ttu-id="27edd-314">Задача 1 - Включение отслеживания фильтра в решение</span><span class="sxs-lookup"><span data-stu-id="27edd-314">Task 1 - Including the Tracking Filter in the Solution</span></span>

<span data-ttu-id="27edd-315">В этой задаче будут включены в Music Store пользовательского фильтра действий для события трассировки.</span><span class="sxs-lookup"><span data-stu-id="27edd-315">In this task, you will include in the Music Store a custom action filter to trace events.</span></span> <span data-ttu-id="27edd-316">Основные понятия как пользовательского фильтра действий, всегда обрабатываются в предыдущем лаборатории &quot;пользовательских фильтров действий&quot;, необходимо просто добавить класс фильтра из папки Assets данную лабораторию и затем создать поставщик фильтра для Unity:</span><span class="sxs-lookup"><span data-stu-id="27edd-316">As custom action filter concepts are already treated in the previous Lab &quot;Custom Action Filters&quot;, you will just include the filter class from the Assets folder of this lab, and then create a Filter Provider for Unity:</span></span>

1. <span data-ttu-id="27edd-317">Откройте **начать** решение находится в **Source\Ex03 - внедрение Filter\Begin действие** папки.</span><span class="sxs-lookup"><span data-stu-id="27edd-317">Open the **Begin** solution located in the **Source\Ex03 - Injecting Action Filter\Begin** folder.</span></span> <span data-ttu-id="27edd-318">В противном случае можно продолжить использование **окончания** решение получен путем выполнения предыдущего упражнения.</span><span class="sxs-lookup"><span data-stu-id="27edd-318">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="27edd-319">Если вы открыли предоставленный **начать** решение, необходимо будет загрузить некоторые отсутствующие пакеты NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="27edd-319">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="27edd-320">Чтобы сделать это, нажмите кнопку **проекта** меню и выберите **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="27edd-320">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="27edd-321">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="27edd-321">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="27edd-322">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="27edd-322">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="27edd-323">Одним из преимуществ использования NuGet является отсутствие поставлять все библиотеки в проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="27edd-323">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="27edd-324">С помощью NuGet Power Tools путем указания версий пакета в файле Packages.config можно для скачивания всех необходимых библиотек при первом запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="27edd-324">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="27edd-325">Вот почему необходимо выполните описанные выше действия, после открытия существующего решения из этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="27edd-325">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="27edd-326">Дополнительные сведения см. в этой статье: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="27edd-326">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="27edd-327">Включить **TraceActionFilter.cs** файла из **/источники/активы** для **/фильтрует** папки.</span><span class="sxs-lookup"><span data-stu-id="27edd-327">Include **TraceActionFilter.cs** file from **/Sources/Assets** to **/Filters** folder.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > <span data-ttu-id="27edd-328">Этот фильтр настраиваемое действие выполняет трассировку ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="27edd-328">This custom action filter performs ASP.NET tracing.</span></span> <span data-ttu-id="27edd-329">Вы можете проверить &quot;локальный ASP.NET MVC 4 и динамические фильтры операций&quot; лаборатории для Дополнительные ссылки.</span><span class="sxs-lookup"><span data-stu-id="27edd-329">You can check &quot;ASP.NET MVC 4 local and Dynamic Action Filters&quot; Lab for more reference.</span></span>
3. <span data-ttu-id="27edd-330">Добавьте пустой класс **FilterProvider.cs** в проект в папке   **/фильтры.**</span><span class="sxs-lookup"><span data-stu-id="27edd-330">Add the empty class **FilterProvider.cs** to the project in the folder **/Filters.**</span></span>
4. <span data-ttu-id="27edd-331">Добавить **System.Web.Mvc** и **Microsoft.Practices.Unity** пространств имен в **FilterProvider.cs**.</span><span class="sxs-lookup"><span data-stu-id="27edd-331">Add the **System.Web.Mvc** and **Microsoft.Practices.Unity** namespaces in **FilterProvider.cs**.</span></span>

    <span data-ttu-id="27edd-332">(Code Snippet - *ASP.NET зависимостей путем внедрения кода лаборатории - Ex03 - фильтр поставщика добавление пространств имен*)</span><span class="sxs-lookup"><span data-stu-id="27edd-332">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. <span data-ttu-id="27edd-333">Сделайте соответствующие классы наследуют от **IFilterProvider** интерфейс.</span><span class="sxs-lookup"><span data-stu-id="27edd-333">Make the class inherit from **IFilterProvider** Interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. <span data-ttu-id="27edd-334">Добавить **IUnityContainer** свойство в **FilterProvider** класса, а затем создать конструктор класса, чтобы назначить контейнера.</span><span class="sxs-lookup"><span data-stu-id="27edd-334">Add a **IUnityContainer** property in the **FilterProvider** class, and then create a class constructor to assign the container.</span></span>

    <span data-ttu-id="27edd-335">(Code Snippet - *ASP.NET зависимостей путем внедрения кода лаборатории - Ex03 - фильтр поставщика конструктор*)</span><span class="sxs-lookup"><span data-stu-id="27edd-335">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Constructor*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="27edd-336">Конструктор класса поставщика фильтра не создаете **новый** объекта внутри.</span><span class="sxs-lookup"><span data-stu-id="27edd-336">The filter provider class constructor is not creating a **new** object inside.</span></span> <span data-ttu-id="27edd-337">Контейнера передается в качестве параметра, а зависимость решается путем Unity.</span><span class="sxs-lookup"><span data-stu-id="27edd-337">The container is passed as a parameter, and the dependency is solved by Unity.</span></span>
7. <span data-ttu-id="27edd-338">В **FilterProvider** , следует реализовать метод **GetFilters** из **IFilterProvider** интерфейс.</span><span class="sxs-lookup"><span data-stu-id="27edd-338">In the **FilterProvider** class, implement the method **GetFilters** from **IFilterProvider** interface.</span></span>

    <span data-ttu-id="27edd-339">(Code Snippet - *ASP.NET зависимостей путем внедрения кода лаборатории - Ex03 - фильтр поставщика GetFilters*)</span><span class="sxs-lookup"><span data-stu-id="27edd-339">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider GetFilters*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a><span data-ttu-id="27edd-340">Задача 2 - регистрация и включение фильтра</span><span class="sxs-lookup"><span data-stu-id="27edd-340">Task 2 - Registering and Enabling the Filter</span></span>

<span data-ttu-id="27edd-341">В этой задаче будет включить отслеживание сайта.</span><span class="sxs-lookup"><span data-stu-id="27edd-341">In this task, you will enable site tracking.</span></span> <span data-ttu-id="27edd-342">Чтобы сделать это, вы будете регистрировать фильтр в **Bootstrapper.cs BuildUnityContainer** метод для запуска трассировки:</span><span class="sxs-lookup"><span data-stu-id="27edd-342">To do that, you will register the filter in **Bootstrapper.cs BuildUnityContainer** method to start tracing:</span></span>

1. <span data-ttu-id="27edd-343">Откройте **Web.config** в корневую папку проекта и Включение отслеживания трассировки в группе System.Web.</span><span class="sxs-lookup"><span data-stu-id="27edd-343">Open **Web.config** located in the project root and enable trace tracking at System.Web group.</span></span>

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. <span data-ttu-id="27edd-344">Откройте **Bootstrapper.cs** в корневом каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="27edd-344">Open **Bootstrapper.cs** at project root.</span></span>
3. <span data-ttu-id="27edd-345">Добавьте ссылку на **MvcMusicStore.Filters** пространства имен.</span><span class="sxs-lookup"><span data-stu-id="27edd-345">Add a reference to the **MvcMusicStore.Filters** namespace.</span></span>

    <span data-ttu-id="27edd-346">(Code Snippet - *ASP.NET внедрения зависимостей лаборатории - Ex03 - загрузчик, добавление пространств имен*)</span><span class="sxs-lookup"><span data-stu-id="27edd-346">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. <span data-ttu-id="27edd-347">Выберите **BuildUnityContainer** метод и регистрация фильтра в контейнера Unity.</span><span class="sxs-lookup"><span data-stu-id="27edd-347">Select the **BuildUnityContainer** method and register the filter in the Unity Container.</span></span> <span data-ttu-id="27edd-348">Вам будет необходимо зарегистрировать у поставщика фильтров, а также действие фильтра.</span><span class="sxs-lookup"><span data-stu-id="27edd-348">You will have to register the filter provider as well as the action filter.</span></span>

    <span data-ttu-id="27edd-349">(Code Snippet - *ASP.NET зависимостей путем внедрения кода лаборатории - Ex03 - Register FilterProvider и ActionFilter*)</span><span class="sxs-lookup"><span data-stu-id="27edd-349">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider and ActionFilter*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="27edd-350">Задача 3 - запуск приложения</span><span class="sxs-lookup"><span data-stu-id="27edd-350">Task 3 - Running the Application</span></span>

<span data-ttu-id="27edd-351">В этой задаче будет запустить приложение и проверить, что настраиваемого фильтра действий является трассировка действия:</span><span class="sxs-lookup"><span data-stu-id="27edd-351">In this task, you will run the application and test that the custom action filter is tracing the activity:</span></span>

1. <span data-ttu-id="27edd-352">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="27edd-352">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="27edd-353">Нажмите кнопку **рок** меню жанров.</span><span class="sxs-lookup"><span data-stu-id="27edd-353">Click **Rock** within the Genres Menu.</span></span> <span data-ttu-id="27edd-354">Если вы хотите можно просмотреть дополнительные жанров.</span><span class="sxs-lookup"><span data-stu-id="27edd-354">You can browse to more genres if you want to.</span></span>

    <span data-ttu-id="27edd-355">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span><span class="sxs-lookup"><span data-stu-id="27edd-355">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span></span>

    <span data-ttu-id="27edd-356">*Приложение Music Store*</span><span class="sxs-lookup"><span data-stu-id="27edd-356">*Music Store*</span></span>
3. <span data-ttu-id="27edd-357">Перейдите к **/Trace.axd** для просмотра трассировки приложения странице, а затем выберите **Просмотр сведений о**.</span><span class="sxs-lookup"><span data-stu-id="27edd-357">Browse to **/Trace.axd** to see the Application Trace page, and then click **View Details**.</span></span>

    <span data-ttu-id="27edd-358">![Журнал трассировки приложения](aspnet-mvc-4-dependency-injection/_static/image12.png "журнал трассировки приложения")</span><span class="sxs-lookup"><span data-stu-id="27edd-358">![Application Trace Log](aspnet-mvc-4-dependency-injection/_static/image12.png "Application Trace Log")</span></span>

    <span data-ttu-id="27edd-359">*Журнал трассировки приложения*</span><span class="sxs-lookup"><span data-stu-id="27edd-359">*Application Trace Log*</span></span>

    <span data-ttu-id="27edd-360">![Трассировка приложения — сведения о запросе](aspnet-mvc-4-dependency-injection/_static/image13.png "трассировки приложения — сведения о запросе")</span><span class="sxs-lookup"><span data-stu-id="27edd-360">![Application Trace - Request Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - Request Details")</span></span>

    <span data-ttu-id="27edd-361">*Трассировка приложения — сведения о запросе*</span><span class="sxs-lookup"><span data-stu-id="27edd-361">*Application Trace - Request Details*</span></span>
4. <span data-ttu-id="27edd-362">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="27edd-362">Close the browser.</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="27edd-363">Сводка</span><span class="sxs-lookup"><span data-stu-id="27edd-363">Summary</span></span>

<span data-ttu-id="27edd-364">Выполнив этот практический семинар вы узнали, как использовать внедрение зависимостей в ASP.NET MVC 4, интеграция Unity с помощью пакета NuGet.</span><span class="sxs-lookup"><span data-stu-id="27edd-364">By completing this Hands-On Lab you have learned how to use Dependency Injection in ASP.NET MVC 4 by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="27edd-365">Чтобы добиться этого, вы использовали внедрение зависимостей в контроллеры, представления и фильтры действий.</span><span class="sxs-lookup"><span data-stu-id="27edd-365">To achieve that, you have used Dependency Injection inside controllers, views and action filters.</span></span>

<span data-ttu-id="27edd-366">Охваченных следующие понятия:</span><span class="sxs-lookup"><span data-stu-id="27edd-366">The following concepts were covered:</span></span>

- <span data-ttu-id="27edd-367">Функции внедрения зависимостей ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="27edd-367">ASP.NET MVC 4 Dependency Injection features</span></span>
- <span data-ttu-id="27edd-368">Интеграция с Unity с помощью пакета NuGet Unity.Mvc3</span><span class="sxs-lookup"><span data-stu-id="27edd-368">Unity integration using Unity.Mvc3 NuGet Package</span></span>
- <span data-ttu-id="27edd-369">Внедрение зависимостей в контроллеры</span><span class="sxs-lookup"><span data-stu-id="27edd-369">Dependency Injection in Controllers</span></span>
- <span data-ttu-id="27edd-370">Внедрение зависимостей в представления</span><span class="sxs-lookup"><span data-stu-id="27edd-370">Dependency Injection in Views</span></span>
- <span data-ttu-id="27edd-371">Внедрение зависимостей фильтров действий</span><span class="sxs-lookup"><span data-stu-id="27edd-371">Dependency injection of Action Filters</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="27edd-372">Приложение а. Установка Visual Studio Express 2012 для Web</span><span class="sxs-lookup"><span data-stu-id="27edd-372">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="27edd-373">Вы можете установить **Microsoft Visual Studio Express 2012 для Web** или другой &quot;Express&quot; версию с помощью **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="27edd-373">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="27edd-374">Приведенные ниже инструкции описывают действия, необходимые для установки *Visual studio Express 2012 для Web* с помощью *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="27edd-374">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="27edd-375">Перейдите к [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="27edd-375">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="27edd-376">Кроме того, если вы уже установили установщика веб-платформы, можно открыть его и выполните поиск продукта &quot; <em>Visual Studio Express 2012 для Web с пакетом Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="27edd-376">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="27edd-377">Щелкните **установить сейчас**.</span><span class="sxs-lookup"><span data-stu-id="27edd-377">Click on **Install Now**.</span></span> <span data-ttu-id="27edd-378">Если у вас нет **установщика веб-платформы** вы будете перенаправлены к сначала загрузить и установить его.</span><span class="sxs-lookup"><span data-stu-id="27edd-378">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="27edd-379">Один раз **установщика веб-платформы** открыт, нажмите кнопку **установить** для запуска программы установки.</span><span class="sxs-lookup"><span data-stu-id="27edd-379">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="27edd-380">![Установка Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "установка Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="27edd-380">![Install Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="27edd-381">*Установка Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="27edd-381">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="27edd-382">Прочтите лицензии и условия все продукты и нажмите кнопку **я принимаю** для продолжения.</span><span class="sxs-lookup"><span data-stu-id="27edd-382">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Принятие условий лицензии](aspnet-mvc-4-dependency-injection/_static/image15.png)

    <span data-ttu-id="27edd-384">*Принятие условий лицензии*</span><span class="sxs-lookup"><span data-stu-id="27edd-384">*Accepting the license terms*</span></span>
5. <span data-ttu-id="27edd-385">Подождите, пока не завершится процесс загрузки и установки.</span><span class="sxs-lookup"><span data-stu-id="27edd-385">Wait until the downloading and installation process completes.</span></span>

    ![Ход установки](aspnet-mvc-4-dependency-injection/_static/image16.png)

    <span data-ttu-id="27edd-387">*Ход выполнения установки*</span><span class="sxs-lookup"><span data-stu-id="27edd-387">*Installation progress*</span></span>
6. <span data-ttu-id="27edd-388">После завершения установки нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="27edd-388">When the installation completes, click **Finish**.</span></span>

    ![Установка завершена](aspnet-mvc-4-dependency-injection/_static/image17.png)

    <span data-ttu-id="27edd-390">*Установка завершена*</span><span class="sxs-lookup"><span data-stu-id="27edd-390">*Installation completed*</span></span>
7. <span data-ttu-id="27edd-391">Нажмите кнопку **выхода** закрыть установщик веб-платформы.</span><span class="sxs-lookup"><span data-stu-id="27edd-391">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="27edd-392">Чтобы открыть Visual Studio Express для Web, перейдите к **запустить** экрана и Займитесь написанием &quot; **VS Express**&quot;, нажмите кнопку на **VS Express для Web** Плитка.</span><span class="sxs-lookup"><span data-stu-id="27edd-392">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express для Web плитки](aspnet-mvc-4-dependency-injection/_static/image18.png)

    <span data-ttu-id="27edd-394">*VS Express для Web плитки*</span><span class="sxs-lookup"><span data-stu-id="27edd-394">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="27edd-395">Приложение б. Использование фрагментов кода</span><span class="sxs-lookup"><span data-stu-id="27edd-395">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="27edd-396">С помощью фрагментов кода у вас есть весь код, который требуется в вашем распоряжении.</span><span class="sxs-lookup"><span data-stu-id="27edd-396">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="27edd-397">Лаборатории документ поможет определить точно при их использовании, как показано на рисунке ниже.</span><span class="sxs-lookup"><span data-stu-id="27edd-397">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="27edd-398">![Фрагменты кода Visual Studio, чтобы вставить код в проект](aspnet-mvc-4-dependency-injection/_static/image19.png "фрагменты кода с помощью Visual Studio, чтобы вставить код в проект")</span><span class="sxs-lookup"><span data-stu-id="27edd-398">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-dependency-injection/_static/image19.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="27edd-399">*Фрагменты кода Visual Studio, чтобы вставить код в проект*</span><span class="sxs-lookup"><span data-stu-id="27edd-399">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="27edd-400">***Чтобы добавить фрагмент кода, с помощью клавиатуры (только C#)***</span><span class="sxs-lookup"><span data-stu-id="27edd-400">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="27edd-401">Поместите курсор в место вставки кода.</span><span class="sxs-lookup"><span data-stu-id="27edd-401">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="27edd-402">Начните вводить имя фрагмента (без пробелов и дефисов).</span><span class="sxs-lookup"><span data-stu-id="27edd-402">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="27edd-403">Посмотрите, как IntelliSense отображает, совпадающие с именами фрагменты.</span><span class="sxs-lookup"><span data-stu-id="27edd-403">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="27edd-404">Выберите правильный фрагмент (или продолжите ввод, пока не будет выделен весь фрагмент имени).</span><span class="sxs-lookup"><span data-stu-id="27edd-404">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="27edd-405">Нажмите клавишу Tab дважды, чтобы вставить фрагмент в положении курсора.</span><span class="sxs-lookup"><span data-stu-id="27edd-405">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="27edd-406">![Начните вводить имя фрагмента](aspnet-mvc-4-dependency-injection/_static/image20.png "начните вводить имя фрагмента")</span><span class="sxs-lookup"><span data-stu-id="27edd-406">![Start typing the snippet name](aspnet-mvc-4-dependency-injection/_static/image20.png "Start typing the snippet name")</span></span>

<span data-ttu-id="27edd-407">*Начните вводить имя фрагмента*</span><span class="sxs-lookup"><span data-stu-id="27edd-407">*Start typing the snippet name*</span></span>

<span data-ttu-id="27edd-408">![Нажмите клавишу Tab, чтобы выделить фрагмент в выделенный](aspnet-mvc-4-dependency-injection/_static/image21.png "нажмите клавишу Tab, чтобы выделить выделенный фрагмент")</span><span class="sxs-lookup"><span data-stu-id="27edd-408">![Press Tab to select the highlighted snippet](aspnet-mvc-4-dependency-injection/_static/image21.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="27edd-409">*Нажмите клавишу Tab, чтобы выделить выделенный фрагмент*</span><span class="sxs-lookup"><span data-stu-id="27edd-409">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="27edd-410">![Снова нажмите клавишу Tab и фрагмент будет расширяться](aspnet-mvc-4-dependency-injection/_static/image22.png "еще раз нажмите клавишу Tab и фрагмент будет расширяться")</span><span class="sxs-lookup"><span data-stu-id="27edd-410">![Press Tab again and the snippet will expand](aspnet-mvc-4-dependency-injection/_static/image22.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="27edd-411">*Снова нажмите клавишу Tab и фрагмент будет расширяться*</span><span class="sxs-lookup"><span data-stu-id="27edd-411">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="27edd-412">***Чтобы добавить фрагмент кода, с помощью мыши (C#, Visual Basic и XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="27edd-412">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="27edd-413">Щелкните правой кнопкой мыши место для вставки фрагмента кода.</span><span class="sxs-lookup"><span data-stu-id="27edd-413">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="27edd-414">Выберите **вставить фрагмент** следуют **Мои фрагменты кода**.</span><span class="sxs-lookup"><span data-stu-id="27edd-414">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="27edd-415">Выберите соответствующий фрагмент из списка, щелкнув его.</span><span class="sxs-lookup"><span data-stu-id="27edd-415">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="27edd-416">![Щелкните правой кнопкой мыши, где необходимо вставить фрагмент кода и выберите Вставить фрагмент](aspnet-mvc-4-dependency-injection/_static/image23.png "щелкните правой кнопкой мыши, где необходимо вставить фрагмент кода и выберите Вставить фрагмент")</span><span class="sxs-lookup"><span data-stu-id="27edd-416">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-dependency-injection/_static/image23.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="27edd-417">*Щелкните правой кнопкой мыши место для вставки фрагмента кода и выберите Вставить фрагмент*</span><span class="sxs-lookup"><span data-stu-id="27edd-417">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="27edd-418">![Выберите соответствующий фрагмент из списка, щелкнув по ней](aspnet-mvc-4-dependency-injection/_static/image24.png "выбрать соответствующий фрагмент из списка, щелкнув по ней")</span><span class="sxs-lookup"><span data-stu-id="27edd-418">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-dependency-injection/_static/image24.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="27edd-419">*Выберите соответствующий фрагмент из списка, щелкнув по ней*</span><span class="sxs-lookup"><span data-stu-id="27edd-419">*Pick the relevant snippet from the list, by clicking on it*</span></span>
