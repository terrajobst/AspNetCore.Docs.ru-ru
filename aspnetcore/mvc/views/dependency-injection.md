---
title: "Внедрение зависимостей в представления"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 80fb9e43-e4db-4af2-b2a8-e1364a712f69
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/dependency-injection
ms.openlocfilehash: 05d64858dd70b45a1e2bb90a86ab3cbdc85264b1
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="dependency-injection-into-views"></a><span data-ttu-id="7c25a-103">Внедрение зависимостей в представления</span><span class="sxs-lookup"><span data-stu-id="7c25a-103">Dependency injection into views</span></span>

<span data-ttu-id="7c25a-104">По [Стив Смит](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="7c25a-104">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="7c25a-105">Поддерживает ASP.NET Core [внедрения зависимостей](xref:fundamentals/dependency-injection) в представления.</span><span class="sxs-lookup"><span data-stu-id="7c25a-105">ASP.NET Core supports [dependency injection](xref:fundamentals/dependency-injection) into views.</span></span> <span data-ttu-id="7c25a-106">Это может быть полезно для представления службы, такие как локализации или данные, необходимые только для заполнения представления элементов.</span><span class="sxs-lookup"><span data-stu-id="7c25a-106">This can be useful for view-specific services, such as localization or data required only for populating view elements.</span></span> <span data-ttu-id="7c25a-107">Можно попытаться ведение [Разделение областей ответственности](http://deviq.com/separation-of-concerns) между контроллерами и представлениями.</span><span class="sxs-lookup"><span data-stu-id="7c25a-107">You should try to maintain [separation of concerns](http://deviq.com/separation-of-concerns) between your controllers and views.</span></span> <span data-ttu-id="7c25a-108">Большая часть данных, которые отображаются в представлениях необходимо передать из контроллера.</span><span class="sxs-lookup"><span data-stu-id="7c25a-108">Most of the data your views display should be passed in from the controller.</span></span>

[<span data-ttu-id="7c25a-109">Просмотреть или скачать образец кода</span><span class="sxs-lookup"><span data-stu-id="7c25a-109">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample)

## <a name="a-simple-example"></a><span data-ttu-id="7c25a-110">Простой пример</span><span class="sxs-lookup"><span data-stu-id="7c25a-110">A Simple Example</span></span>

<span data-ttu-id="7c25a-111">Служба может вводить в представлении с помощью `@inject` директивы.</span><span class="sxs-lookup"><span data-stu-id="7c25a-111">You can inject a service into a view using the `@inject` directive.</span></span> <span data-ttu-id="7c25a-112">Можно представить `@inject` как добавление свойства в представлении, а также заполнение свойства с помощью DI.</span><span class="sxs-lookup"><span data-stu-id="7c25a-112">You can think of `@inject` as adding a property to your view, and populating the property using DI.</span></span>

<span data-ttu-id="7c25a-113">Синтаксис для `@inject`:`@inject <type> <name>`</span><span class="sxs-lookup"><span data-stu-id="7c25a-113">The syntax for `@inject`: `@inject <type> <name>`</span></span>

<span data-ttu-id="7c25a-114">Пример `@inject` в действии:</span><span class="sxs-lookup"><span data-stu-id="7c25a-114">An example of `@inject` in action:</span></span>

<span data-ttu-id="7c25a-115">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]</span><span class="sxs-lookup"><span data-stu-id="7c25a-115">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]</span></span>

<span data-ttu-id="7c25a-116">Это представление отображает список `ToDoItem` экземпляров, а также сводку Общая статистика.</span><span class="sxs-lookup"><span data-stu-id="7c25a-116">This view displays a list of `ToDoItem` instances, along with a summary showing overall statistics.</span></span> <span data-ttu-id="7c25a-117">Сводка заполняется из внедренного `StatisticsService`.</span><span class="sxs-lookup"><span data-stu-id="7c25a-117">The summary is populated from the injected `StatisticsService`.</span></span> <span data-ttu-id="7c25a-118">Эта служба регистрируется для внедрения зависимости в `ConfigureServices` в *файла Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7c25a-118">This service is registered for dependency injection in `ConfigureServices` in *Startup.cs*:</span></span>

<span data-ttu-id="7c25a-119">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]</span><span class="sxs-lookup"><span data-stu-id="7c25a-119">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]</span></span>

<span data-ttu-id="7c25a-120">`StatisticsService` Выполняет некоторые вычисления на наборе `ToDoItem` экземпляров, которые он обращается к через репозитория:</span><span class="sxs-lookup"><span data-stu-id="7c25a-120">The `StatisticsService` performs some calculations on the set of `ToDoItem` instances, which it accesses via a repository:</span></span>

<span data-ttu-id="7c25a-121">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,26)]</span><span class="sxs-lookup"><span data-stu-id="7c25a-121">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,26)]</span></span>

<span data-ttu-id="7c25a-122">Репозитории примеров использует находящуюся в памяти коллекцию.</span><span class="sxs-lookup"><span data-stu-id="7c25a-122">The sample repository uses an in-memory collection.</span></span> <span data-ttu-id="7c25a-123">Реализация, показанный выше (который работает на всех данных в памяти) не рекомендуется для больших, удаленный доступ к которым осуществляется наборов данных.</span><span class="sxs-lookup"><span data-stu-id="7c25a-123">The implementation shown above (which operates on all of the data in memory) is not recommended for large, remotely accessed data sets.</span></span>

<span data-ttu-id="7c25a-124">Образец отображаются данные из модели, привязанный к представлению и службы, вставленными в представлении:</span><span class="sxs-lookup"><span data-stu-id="7c25a-124">The sample displays data from the model bound to the view and the service injected into the view:</span></span>

![Для просмотра списка число элементов, выполнить элементов, средний приоритет и список задач с их уровни приоритета и логические значения, указывающее, завершения.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a><span data-ttu-id="7c25a-126">Заполнение поиск данных</span><span class="sxs-lookup"><span data-stu-id="7c25a-126">Populating Lookup Data</span></span>

<span data-ttu-id="7c25a-127">Внедрение представления можно использовать для заполнения параметров в элементы пользовательского интерфейса, например раскрывающиеся списки.</span><span class="sxs-lookup"><span data-stu-id="7c25a-127">View injection can be useful to populate options in UI elements, such as dropdown lists.</span></span> <span data-ttu-id="7c25a-128">Рассмотрим форму профиль пользователя, содержащий параметры для указания пол, состояние и другие параметры.</span><span class="sxs-lookup"><span data-stu-id="7c25a-128">Consider a user profile form that includes options for specifying gender, state, and other preferences.</span></span> <span data-ttu-id="7c25a-129">Отрисовка формы с помощью стандартный подход MVC потребует контроллера для запроса службы доступа к данным для каждого из этих наборов параметров и затем заполнение модели или `ViewBag` каждый набор параметров для привязки.</span><span class="sxs-lookup"><span data-stu-id="7c25a-129">Rendering such a form using a standard MVC approach would require the controller to request data access services for each of these sets of options, and then populate a model or `ViewBag` with each set of options to be bound.</span></span>

<span data-ttu-id="7c25a-130">Альтернативный подход внедряет службы непосредственно в представлении, чтобы получить параметры.</span><span class="sxs-lookup"><span data-stu-id="7c25a-130">An alternative approach injects services directly into the view to obtain the options.</span></span> <span data-ttu-id="7c25a-131">Это сводит к минимуму объем кода, необходимого для контроллера, переместив этой логики конструкций элемента представления в самого представления.</span><span class="sxs-lookup"><span data-stu-id="7c25a-131">This minimizes the amount of code required by the controller, moving this view element construction logic into the view itself.</span></span> <span data-ttu-id="7c25a-132">Действие контроллера для отображения в форме редактирования профиля нужно только передать экземпляр профиля в форме:</span><span class="sxs-lookup"><span data-stu-id="7c25a-132">The controller action to display a profile editing form only needs to pass the form the profile instance:</span></span>

<span data-ttu-id="7c25a-133">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]</span><span class="sxs-lookup"><span data-stu-id="7c25a-133">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]</span></span>

<span data-ttu-id="7c25a-134">HTML-форму, используемую для обновления этих параметров включает в себя раскрывающиеся списки для трех свойств:</span><span class="sxs-lookup"><span data-stu-id="7c25a-134">The HTML form used to update these preferences includes dropdown lists for three of the properties:</span></span>

![Обновите представление профиля с формой ввода имя, пол, состояние и любимого цвета.](dependency-injection/_static/updateprofile.png)

<span data-ttu-id="7c25a-136">Эти списки заполняются службы, который был вставлен в представлении:</span><span class="sxs-lookup"><span data-stu-id="7c25a-136">These lists are populated by a service that has been injected into the view:</span></span>

<span data-ttu-id="7c25a-137">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]</span><span class="sxs-lookup"><span data-stu-id="7c25a-137">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]</span></span>

<span data-ttu-id="7c25a-138">`ProfileOptionsService` — Это служба уровня пользовательского интерфейса обеспечивает только данные, необходимые для этой формы:</span><span class="sxs-lookup"><span data-stu-id="7c25a-138">The `ProfileOptionsService` is a UI-level service designed to provide just the data needed for this form:</span></span>

<span data-ttu-id="7c25a-139">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]</span><span class="sxs-lookup"><span data-stu-id="7c25a-139">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]</span></span>

>[!TIP]
> <span data-ttu-id="7c25a-140">Не забудьте регистрации будет запрашивать через внедрение зависимостей в типы `ConfigureServices` метод в *файла Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="7c25a-140">Don't forget to register types you will request through dependency injection in the  `ConfigureServices` method in *Startup.cs*.</span></span>

## <a name="overriding-services"></a><span data-ttu-id="7c25a-141">Переопределение служб</span><span class="sxs-lookup"><span data-stu-id="7c25a-141">Overriding Services</span></span>

<span data-ttu-id="7c25a-142">Помимо добавления новых служб, этот метод можно использовать для переопределения добавленного ранее службы на странице.</span><span class="sxs-lookup"><span data-stu-id="7c25a-142">In addition to injecting new services, this technique can also be used to override previously injected services on a page.</span></span> <span data-ttu-id="7c25a-143">На следующем рисунке показаны все доступные поля на странице, используемый в первом примере:</span><span class="sxs-lookup"><span data-stu-id="7c25a-143">The figure below shows all of the fields available on the page used in the first example:</span></span>

![Контекстные меню IntelliSense на типизированный Html, компонент, StatsService и URL-адрес поля со списком символа @](dependency-injection/_static/razor-fields.png)

<span data-ttu-id="7c25a-145">Как видите, поля по умолчанию включают `Html`, `Component`, и `Url` (а также `StatsService` , мы введенного).</span><span class="sxs-lookup"><span data-stu-id="7c25a-145">As you can see, the default fields include `Html`, `Component`, and `Url` (as well as the `StatsService` that we injected).</span></span> <span data-ttu-id="7c25a-146">Если для экземпляра необходимо заменить на собственный вспомогательных методов HTML по умолчанию, это можно легко сделать с помощью `@inject`:</span><span class="sxs-lookup"><span data-stu-id="7c25a-146">If for instance you wanted to replace the default HTML Helpers with your own, you could easily do so using `@inject`:</span></span>

<span data-ttu-id="7c25a-147">[!code-html[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]</span><span class="sxs-lookup"><span data-stu-id="7c25a-147">[!code-html[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]</span></span>

<span data-ttu-id="7c25a-148">Если вы хотите расширить существующие службы, можно просто использовать этот метод при наследовании или упаковки существующую вашу реализацию.</span><span class="sxs-lookup"><span data-stu-id="7c25a-148">If you want to extend existing services, you can simply use this technique while inheriting from or wrapping the existing implementation with your own.</span></span>

## <a name="see-also"></a><span data-ttu-id="7c25a-149">См. также</span><span class="sxs-lookup"><span data-stu-id="7c25a-149">See Also</span></span>

* <span data-ttu-id="7c25a-150">Блог Timms Simon: [начало поиск данных в представлении](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span><span class="sxs-lookup"><span data-stu-id="7c25a-150">Simon Timms Blog: [Getting Lookup Data Into Your View](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span></span>
