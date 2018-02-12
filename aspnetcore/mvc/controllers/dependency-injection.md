---
title: "Внедрение зависимостей в контроллеры"
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 118f504311b58258b5a0510477280505135dd2d9
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="dependency-injection-into-controllers"></a><span data-ttu-id="b39b7-102">Внедрение зависимостей в контроллеры</span><span class="sxs-lookup"><span data-stu-id="b39b7-102">Dependency injection into controllers</span></span>

<a name="dependency-injection-controllers"></a>

<span data-ttu-id="b39b7-103">Автор: [Стив Смит](https://ardalis.com/) (Steve Smith)</span><span class="sxs-lookup"><span data-stu-id="b39b7-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="b39b7-104">Контроллеры ASP.NET Core MVC должны запрашивать свои зависимости явно с помощью конструкторов.</span><span class="sxs-lookup"><span data-stu-id="b39b7-104">ASP.NET Core MVC controllers should request their dependencies explicitly via their constructors.</span></span> <span data-ttu-id="b39b7-105">В некоторых случаях отдельные действия контроллера могут потребовать службу, которую не имеет смысла запрашивать на уровне контроллера.</span><span class="sxs-lookup"><span data-stu-id="b39b7-105">In some instances, individual controller actions may require a service, and it may not make sense to request at the controller level.</span></span> <span data-ttu-id="b39b7-106">В этой ситуации также можно внедрить службу в качестве параметра метода действия.</span><span class="sxs-lookup"><span data-stu-id="b39b7-106">In this case, you can also choose to inject a service as a parameter on the action method.</span></span>

<span data-ttu-id="b39b7-107">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b39b7-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="b39b7-108">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="b39b7-108">Dependency Injection</span></span>

<span data-ttu-id="b39b7-109">Методика внедрения зависимости следует [принципу инверсии зависимостей](http://deviq.com/dependency-inversion-principle/), позволяя приложениям состоять из слабо связанных модулей.</span><span class="sxs-lookup"><span data-stu-id="b39b7-109">Dependency injection is a technique that follows the [Dependency Inversion Principle](http://deviq.com/dependency-inversion-principle/), allowing for applications to be composed of loosely coupled modules.</span></span> <span data-ttu-id="b39b7-110">ASP.NET Core имеет встроенную поддержку [внедрения зависимостей](../../fundamentals/dependency-injection.md), что упрощает тестирование и обслуживание приложений.</span><span class="sxs-lookup"><span data-stu-id="b39b7-110">ASP.NET Core has built-in support for [dependency injection](../../fundamentals/dependency-injection.md), which makes applications easier to test and maintain.</span></span>

## <a name="constructor-injection"></a><span data-ttu-id="b39b7-111">Внедрение через конструктор</span><span class="sxs-lookup"><span data-stu-id="b39b7-111">Constructor Injection</span></span>

<span data-ttu-id="b39b7-112">Встроенная поддержка внедрения зависимостей на основе конструктора в ASP.NET Core распространяется на контроллеры MVC.</span><span class="sxs-lookup"><span data-stu-id="b39b7-112">ASP.NET Core's built-in support for constructor-based dependency injection extends to MVC controllers.</span></span> <span data-ttu-id="b39b7-113">Просто добавляя тип службы в контроллер в качестве параметра конструктора, ASP.NET Core будет пытаться разрешить этот тип с помощью встроенного контейнера службы.</span><span class="sxs-lookup"><span data-stu-id="b39b7-113">By simply adding a service type to your controller as a constructor parameter, ASP.NET Core will attempt to resolve that type using its built in service container.</span></span> <span data-ttu-id="b39b7-114">Службы обычно, хотя и не всегда, задаются с помощью интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="b39b7-114">Services are typically, but not always, defined using interfaces.</span></span> <span data-ttu-id="b39b7-115">Например, если приложение имеет бизнес-логику, зависящую от текущего времени, можно внедрить службу, которая возвращает время (вместо жесткого его задания), что позволяет тестам доходить до реализаций, использующих заданное время.</span><span class="sxs-lookup"><span data-stu-id="b39b7-115">For example, if your application has business logic that depends on the current time, you can inject a service that retrieves the time (rather than hard-coding it), which would allow your tests to pass in implementations that use a set time.</span></span>

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


<span data-ttu-id="b39b7-116">Реализация такого интерфейса, использующего системные часы во время выполнения, не представляет сложностей:</span><span class="sxs-lookup"><span data-stu-id="b39b7-116">Implementing an interface like this one so that it uses the system clock at runtime is trivial:</span></span>

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


<span data-ttu-id="b39b7-117">После этого мы можем использовать службу в своем контроллере.</span><span class="sxs-lookup"><span data-stu-id="b39b7-117">With this in place, we can use the service in our controller.</span></span> <span data-ttu-id="b39b7-118">В этом случае мы добавили некоторую логику в метод `HomeController` `Index` для отображения приветствия пользователю с учетом времени суток.</span><span class="sxs-lookup"><span data-stu-id="b39b7-118">In this case, we have added some logic to the `HomeController` `Index` method to display a greeting to the user based on the time of day.</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

<span data-ttu-id="b39b7-119">Если запустить приложение сейчас, скорее всего, возникнет ошибка:</span><span class="sxs-lookup"><span data-stu-id="b39b7-119">If we run the application now, we will most likely encounter an error:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

<span data-ttu-id="b39b7-120">Эта ошибка возникает, если мы не настроили службу в методе `ConfigureServices` своего класса `Startup`.</span><span class="sxs-lookup"><span data-stu-id="b39b7-120">This error occurs when we have not configured a service in the `ConfigureServices` method in our `Startup` class.</span></span> <span data-ttu-id="b39b7-121">Чтобы указать, что запросы для `IDateTime` должны разрешаться с использованием экземпляра `SystemDateTime`, добавьте выделенную ниже строку в свой метод `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b39b7-121">To specify that requests for `IDateTime` should be resolved using an instance of `SystemDateTime`, add the highlighted line in the listing below to your `ConfigureServices` method:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> <span data-ttu-id="b39b7-122">Эту конкретную службу можно реализовать с помощью любого из нескольких разных параметров времени существования (`Transient`, `Scoped` или `Singleton`).</span><span class="sxs-lookup"><span data-stu-id="b39b7-122">This particular service could be implemented using any of several different lifetime options (`Transient`, `Scoped`, or `Singleton`).</span></span> <span data-ttu-id="b39b7-123">Раздел [Внедрение зависимостей](../../fundamentals/dependency-injection.md) описывает, как каждый из этих параметров области влияет на поведение службы.</span><span class="sxs-lookup"><span data-stu-id="b39b7-123">See [Dependency Injection](../../fundamentals/dependency-injection.md) to understand how each of these scope options will affect the behavior of your service.</span></span>

<span data-ttu-id="b39b7-124">После настройки службы при запуске приложения и переходе на домашнюю страницу должно отображаться ожидаемое сообщение с учетом времени суток:</span><span class="sxs-lookup"><span data-stu-id="b39b7-124">Once the service has been configured, running the application and navigating to the home page should display the time-based message as expected:</span></span>

![Приветствие сервера](dependency-injection/_static/server-greeting.png)

>[!TIP]
> <span data-ttu-id="b39b7-126">Раздел [Тестирование логики контроллера](testing.md) описывает, как явный запрос зависимостей [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) в контроллерах упрощает тестирование кода.</span><span class="sxs-lookup"><span data-stu-id="b39b7-126">See [Testing Controller Logic](testing.md) to learn how to explicitly request dependencies [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) in controllers makes code easier to test.</span></span>

<span data-ttu-id="b39b7-127">Встроенная функция внедрения зависимостей ASP.NET Core поддерживает наличие лишь одного конструктора для классов, запрашивающих службы.</span><span class="sxs-lookup"><span data-stu-id="b39b7-127">ASP.NET Core's built-in dependency injection supports having only a single constructor for classes requesting services.</span></span> <span data-ttu-id="b39b7-128">При наличии более одного конструктора может возникнуть следующее исключение:</span><span class="sxs-lookup"><span data-stu-id="b39b7-128">If you have more than one constructor, you may get an exception stating:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

<span data-ttu-id="b39b7-129">Как сказано в сообщении об ошибке, вы можете устранить эту проблему, используя один конструктор.</span><span class="sxs-lookup"><span data-stu-id="b39b7-129">As the error message states, you can correct this problem having just a single constructor.</span></span> <span data-ttu-id="b39b7-130">Вы также можете [заменить поддержку внедрения зависимостей по умолчанию на стороннюю реализацию](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), многие из которых поддерживают несколько конструкторов.</span><span class="sxs-lookup"><span data-stu-id="b39b7-130">You can also [replace the default dependency injection support with a third party implementation](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), many of which support multiple constructors.</span></span>

## <a name="action-injection-with-fromservices"></a><span data-ttu-id="b39b7-131">Внедрение действий с помощью FromServices</span><span class="sxs-lookup"><span data-stu-id="b39b7-131">Action Injection with FromServices</span></span>

<span data-ttu-id="b39b7-132">Иногда служба требуется всего для одного действия в контроллере.</span><span class="sxs-lookup"><span data-stu-id="b39b7-132">Sometimes you don't need a service for more than one action within your controller.</span></span> <span data-ttu-id="b39b7-133">В этом случае имеет смысл внедрить службу в качестве параметра метода действия.</span><span class="sxs-lookup"><span data-stu-id="b39b7-133">In this case, it may make sense to inject the service as a parameter to the action method.</span></span> <span data-ttu-id="b39b7-134">Это можно сделать, пометив параметр с помощью атрибута `[FromServices]`, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="b39b7-134">This is done by marking the parameter with the attribute `[FromServices]` as shown here:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a><span data-ttu-id="b39b7-135">Доступ к параметрам из контроллера</span><span class="sxs-lookup"><span data-stu-id="b39b7-135">Accessing Settings from a Controller</span></span>

<span data-ttu-id="b39b7-136">Доступ к параметрам приложения или конфигурации из контроллера относится к типичным шаблонам.</span><span class="sxs-lookup"><span data-stu-id="b39b7-136">Accessing application or configuration settings from within a controller is a common pattern.</span></span> <span data-ttu-id="b39b7-137">Для такого доступа следует использовать шаблон параметров, описанный в разделе [Конфигурация](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="b39b7-137">This access should use the Options pattern described in [configuration](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="b39b7-138">В общем случае не следует запрашивать параметры непосредственно из своего контроллера с помощью внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="b39b7-138">You generally shouldn't request settings directly from your controller using dependency injection.</span></span> <span data-ttu-id="b39b7-139">Оптимальнее запросить экземпляр `IOptions<T>`, где `T` является нужным вам классом конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b39b7-139">A better approach is to request an `IOptions<T>` instance, where `T` is the configuration class you need.</span></span>

<span data-ttu-id="b39b7-140">Для работы с шаблоном параметров нужно создать класс, представляющий эти параметры, например следующий:</span><span class="sxs-lookup"><span data-stu-id="b39b7-140">To work with the options pattern, you need to create a class that represents the options, such as this one:</span></span>

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

<span data-ttu-id="b39b7-141">Затем вам нужно настроить приложение для использования этой модели параметров и добавить класс конфигурации в коллекцию служб в `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b39b7-141">Then you need to configure the application to use the options model and add your configuration class to the services collection in `ConfigureServices`:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> <span data-ttu-id="b39b7-142">В приведенном выше коде мы настраиваем приложение для считывания параметров из файла формата JSON.</span><span class="sxs-lookup"><span data-stu-id="b39b7-142">In the above listing, we are configuring the application to read the settings from a JSON-formatted file.</span></span> <span data-ttu-id="b39b7-143">Вы также можете полностью настроить параметры в коде, как показано выше.</span><span class="sxs-lookup"><span data-stu-id="b39b7-143">You can also configure the settings entirely in code, as is shown in the commented code above.</span></span> <span data-ttu-id="b39b7-144">Сведения о других параметрах конфигурации см. в разделе [Конфигурация](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="b39b7-144">See [Configuration](xref:fundamentals/configuration/index) for further configuration options.</span></span>

<span data-ttu-id="b39b7-145">После указания строго типизированного объекта конфигурации (в данном случае `SampleWebSettings`) и добавления его в коллекцию служб вы можете запросить его из любого контроллера или метода действия, запросив экземпляр `IOptions<T>` (в данном случае `IOptions<SampleWebSettings>`).</span><span class="sxs-lookup"><span data-stu-id="b39b7-145">Once you've specified a strongly-typed configuration object (in this case, `SampleWebSettings`) and added it to the services collection, you can request it from any Controller or Action method by requesting an instance of `IOptions<T>` (in this case, `IOptions<SampleWebSettings>`).</span></span> <span data-ttu-id="b39b7-146">Следующий код показывает, как запросить параметры из контроллера:</span><span class="sxs-lookup"><span data-stu-id="b39b7-146">The following code shows how one would request the settings from a controller:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

<span data-ttu-id="b39b7-147">Соблюдение шаблона параметров позволяет отделить параметры и конфигурацию друг от друга и обеспечивает [разделение функций](http://deviq.com/separation-of-concerns/) для контроллера, так как ему не нужно знать, где и как искать сведения о параметрах.</span><span class="sxs-lookup"><span data-stu-id="b39b7-147">Following the Options pattern allows settings and configuration to be decoupled from one another, and ensures the controller is following [separation of concerns](http://deviq.com/separation-of-concerns/), since it doesn't need to know how or where to find the settings information.</span></span> <span data-ttu-id="b39b7-148">Кроме того, это упрощает модульное тестирование контроллера ([Тестирование логики контроллера](testing.md)), так как отсутствует [статическое слипание](http://deviq.com/static-cling/) или прямое создание экземпляров для классов параметров в классе контроллера.</span><span class="sxs-lookup"><span data-stu-id="b39b7-148">It also makes the controller easier to unit test [Testing Controller Logic](testing.md), since there's no [static cling](http://deviq.com/static-cling/) or direct instantiation of settings classes within the controller class.</span></span>
