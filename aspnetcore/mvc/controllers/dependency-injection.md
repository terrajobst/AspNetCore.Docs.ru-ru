---
title: Внедрение зависимостей в контроллеры в ASP.NET Core
author: ardalis
description: Узнайте, как контроллеры MVC ASP.NET Core явным образом запрашивают зависимости с помощью конструкторов с внедрением зависимостей в ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: c3e26d294d51dc7044158b05c1ac39015c494610
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
---
# <a name="dependency-injection-into-controllers-in-aspnet-core"></a><span data-ttu-id="1d3ae-103">Внедрение зависимостей в контроллеры в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1d3ae-103">Dependency injection into controllers in ASP.NET Core</span></span>

<a name="dependency-injection-controllers"></a>

<span data-ttu-id="1d3ae-104">Автор: [Стив Смит](https://ardalis.com/) (Steve Smith)</span><span class="sxs-lookup"><span data-stu-id="1d3ae-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="1d3ae-105">Контроллеры ASP.NET Core MVC должны запрашивать свои зависимости явно с помощью конструкторов.</span><span class="sxs-lookup"><span data-stu-id="1d3ae-105">ASP.NET Core MVC controllers should request their dependencies explicitly via their constructors.</span></span> <span data-ttu-id="1d3ae-106">В некоторых случаях отдельные действия контроллера могут потребовать службу, которую не имеет смысла запрашивать на уровне контроллера.</span><span class="sxs-lookup"><span data-stu-id="1d3ae-106">In some instances, individual controller actions may require a service, and it may not make sense to request at the controller level.</span></span> <span data-ttu-id="1d3ae-107">В этой ситуации также можно внедрить службу в качестве параметра метода действия.</span><span class="sxs-lookup"><span data-stu-id="1d3ae-107">In this case, you can also choose to inject a service as a parameter on the action method.</span></span>

<span data-ttu-id="1d3ae-108">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1d3ae-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="1d3ae-109">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="1d3ae-109">Dependency Injection</span></span>

<span data-ttu-id="1d3ae-110">Методика внедрения зависимости следует [принципу инверсии зависимостей](http://deviq.com/dependency-inversion-principle/), позволяя приложениям состоять из слабо связанных модулей.</span><span class="sxs-lookup"><span data-stu-id="1d3ae-110">Dependency injection is a technique that follows the [Dependency Inversion Principle](http://deviq.com/dependency-inversion-principle/), allowing for applications to be composed of loosely coupled modules.</span></span> <span data-ttu-id="1d3ae-111">ASP.NET Core имеет встроенную поддержку [внедрения зависимостей](../../fundamentals/dependency-injection.md), что упрощает тестирование и обслуживание приложений.</span><span class="sxs-lookup"><span data-stu-id="1d3ae-111">ASP.NET Core has built-in support for [dependency injection](../../fundamentals/dependency-injection.md), which makes applications easier to test and maintain.</span></span>

## <a name="constructor-injection"></a><span data-ttu-id="1d3ae-112">Внедрение через конструктор</span><span class="sxs-lookup"><span data-stu-id="1d3ae-112">Constructor Injection</span></span>

<span data-ttu-id="1d3ae-113">Встроенная поддержка внедрения зависимостей на основе конструктора в ASP.NET Core распространяется на контроллеры MVC.</span><span class="sxs-lookup"><span data-stu-id="1d3ae-113">ASP.NET Core's built-in support for constructor-based dependency injection extends to MVC controllers.</span></span> <span data-ttu-id="1d3ae-114">Просто добавляя тип службы в контроллер в качестве параметра конструктора, ASP.NET Core будет пытаться разрешить этот тип с помощью встроенного контейнера службы.</span><span class="sxs-lookup"><span data-stu-id="1d3ae-114">By simply adding a service type to your controller as a constructor parameter, ASP.NET Core will attempt to resolve that type using its built in service container.</span></span> <span data-ttu-id="1d3ae-115">Службы обычно, хотя и не всегда, задаются с помощью интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="1d3ae-115">Services are typically, but not always, defined using interfaces.</span></span> <span data-ttu-id="1d3ae-116">Например, если приложение имеет бизнес-логику, зависящую от текущего времени, можно внедрить службу, которая возвращает время (вместо жесткого его задания), что позволяет тестам доходить до реализаций, использующих заданное время.</span><span class="sxs-lookup"><span data-stu-id="1d3ae-116">For example, if your application has business logic that depends on the current time, you can inject a service that retrieves the time (rather than hard-coding it), which would allow your tests to pass in implementations that use a set time.</span></span>

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


<span data-ttu-id="1d3ae-117">Реализация такого интерфейса, использующего системные часы во время выполнения, не представляет сложностей:</span><span class="sxs-lookup"><span data-stu-id="1d3ae-117">Implementing an interface like this one so that it uses the system clock at runtime is trivial:</span></span>

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


<span data-ttu-id="1d3ae-118">После этого мы можем использовать службу в своем контроллере.</span><span class="sxs-lookup"><span data-stu-id="1d3ae-118">With this in place, we can use the service in our controller.</span></span> <span data-ttu-id="1d3ae-119">В этом случае мы добавили некоторую логику в метод `HomeController` `Index` для отображения приветствия пользователю с учетом времени суток.</span><span class="sxs-lookup"><span data-stu-id="1d3ae-119">In this case, we have added some logic to the `HomeController` `Index` method to display a greeting to the user based on the time of day.</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

<span data-ttu-id="1d3ae-120">Если запустить приложение сейчас, скорее всего, возникнет ошибка:</span><span class="sxs-lookup"><span data-stu-id="1d3ae-120">If we run the application now, we will most likely encounter an error:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

<span data-ttu-id="1d3ae-121">Эта ошибка возникает, если мы не настроили службу в методе `ConfigureServices` своего класса `Startup`.</span><span class="sxs-lookup"><span data-stu-id="1d3ae-121">This error occurs when we have not configured a service in the `ConfigureServices` method in our `Startup` class.</span></span> <span data-ttu-id="1d3ae-122">Чтобы указать, что запросы для `IDateTime` должны разрешаться с использованием экземпляра `SystemDateTime`, добавьте выделенную ниже строку в свой метод `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1d3ae-122">To specify that requests for `IDateTime` should be resolved using an instance of `SystemDateTime`, add the highlighted line in the listing below to your `ConfigureServices` method:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> <span data-ttu-id="1d3ae-123">Эту конкретную службу можно реализовать с помощью любого из нескольких разных параметров времени существования (`Transient`, `Scoped` или `Singleton`).</span><span class="sxs-lookup"><span data-stu-id="1d3ae-123">This particular service could be implemented using any of several different lifetime options (`Transient`, `Scoped`, or `Singleton`).</span></span> <span data-ttu-id="1d3ae-124">Раздел [Внедрение зависимостей](../../fundamentals/dependency-injection.md) описывает, как каждый из этих параметров области влияет на поведение службы.</span><span class="sxs-lookup"><span data-stu-id="1d3ae-124">See [Dependency Injection](../../fundamentals/dependency-injection.md) to understand how each of these scope options will affect the behavior of your service.</span></span>

<span data-ttu-id="1d3ae-125">После настройки службы при запуске приложения и переходе на домашнюю страницу должно отображаться ожидаемое сообщение с учетом времени суток:</span><span class="sxs-lookup"><span data-stu-id="1d3ae-125">Once the service has been configured, running the application and navigating to the home page should display the time-based message as expected:</span></span>

![Приветствие сервера](dependency-injection/_static/server-greeting.png)

>[!TIP]
> <span data-ttu-id="1d3ae-127">См. раздел [Тестирование логики контроллера](testing.md), чтобы узнать, как явный запрос зависимостей [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) в контроллерах упрощает тестирование кода.</span><span class="sxs-lookup"><span data-stu-id="1d3ae-127">See [Test controller logic](testing.md) to learn how to explicitly request dependencies [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) in controllers makes code easier to test.</span></span>

<span data-ttu-id="1d3ae-128">Встроенная функция внедрения зависимостей ASP.NET Core поддерживает наличие лишь одного конструктора для классов, запрашивающих службы.</span><span class="sxs-lookup"><span data-stu-id="1d3ae-128">ASP.NET Core's built-in dependency injection supports having only a single constructor for classes requesting services.</span></span> <span data-ttu-id="1d3ae-129">При наличии более одного конструктора может возникнуть следующее исключение:</span><span class="sxs-lookup"><span data-stu-id="1d3ae-129">If you have more than one constructor, you may get an exception stating:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

<span data-ttu-id="1d3ae-130">Как сказано в сообщении об ошибке, вы можете устранить эту проблему, используя один конструктор.</span><span class="sxs-lookup"><span data-stu-id="1d3ae-130">As the error message states, you can correct this problem having just a single constructor.</span></span> <span data-ttu-id="1d3ae-131">Вы также можете [заменить поддержку внедрения зависимостей по умолчанию на стороннюю реализацию](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), многие из которых поддерживают несколько конструкторов.</span><span class="sxs-lookup"><span data-stu-id="1d3ae-131">You can also [replace the default dependency injection support with a third party implementation](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), many of which support multiple constructors.</span></span>

## <a name="action-injection-with-fromservices"></a><span data-ttu-id="1d3ae-132">Внедрение действий с помощью FromServices</span><span class="sxs-lookup"><span data-stu-id="1d3ae-132">Action Injection with FromServices</span></span>

<span data-ttu-id="1d3ae-133">Иногда служба требуется всего для одного действия в контроллере.</span><span class="sxs-lookup"><span data-stu-id="1d3ae-133">Sometimes you don't need a service for more than one action within your controller.</span></span> <span data-ttu-id="1d3ae-134">В этом случае имеет смысл внедрить службу в качестве параметра метода действия.</span><span class="sxs-lookup"><span data-stu-id="1d3ae-134">In this case, it may make sense to inject the service as a parameter to the action method.</span></span> <span data-ttu-id="1d3ae-135">Это можно сделать, пометив параметр с помощью атрибута `[FromServices]`, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="1d3ae-135">This is done by marking the parameter with the attribute `[FromServices]` as shown here:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a><span data-ttu-id="1d3ae-136">Доступ к параметрам из контроллера</span><span class="sxs-lookup"><span data-stu-id="1d3ae-136">Accessing Settings from a Controller</span></span>

<span data-ttu-id="1d3ae-137">Доступ к параметрам приложения или конфигурации из контроллера относится к типичным шаблонам.</span><span class="sxs-lookup"><span data-stu-id="1d3ae-137">Accessing application or configuration settings from within a controller is a common pattern.</span></span> <span data-ttu-id="1d3ae-138">Для такого доступа следует использовать шаблон параметров, описанный в разделе [Конфигурация](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="1d3ae-138">This access should use the Options pattern described in [configuration](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="1d3ae-139">В общем случае не следует запрашивать параметры непосредственно из своего контроллера с помощью внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="1d3ae-139">You generally shouldn't request settings directly from your controller using dependency injection.</span></span> <span data-ttu-id="1d3ae-140">Оптимальнее запросить экземпляр `IOptions<T>`, где `T` является нужным вам классом конфигурации.</span><span class="sxs-lookup"><span data-stu-id="1d3ae-140">A better approach is to request an `IOptions<T>` instance, where `T` is the configuration class you need.</span></span>

<span data-ttu-id="1d3ae-141">Для работы с шаблоном параметров нужно создать класс, представляющий эти параметры, например следующий:</span><span class="sxs-lookup"><span data-stu-id="1d3ae-141">To work with the options pattern, you need to create a class that represents the options, such as this one:</span></span>

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

<span data-ttu-id="1d3ae-142">Затем вам нужно настроить приложение для использования этой модели параметров и добавить класс конфигурации в коллекцию служб в `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1d3ae-142">Then you need to configure the application to use the options model and add your configuration class to the services collection in `ConfigureServices`:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> <span data-ttu-id="1d3ae-143">В приведенном выше коде мы настраиваем приложение для считывания параметров из файла формата JSON.</span><span class="sxs-lookup"><span data-stu-id="1d3ae-143">In the above listing, we are configuring the application to read the settings from a JSON-formatted file.</span></span> <span data-ttu-id="1d3ae-144">Вы также можете полностью настроить параметры в коде, как показано выше.</span><span class="sxs-lookup"><span data-stu-id="1d3ae-144">You can also configure the settings entirely in code, as is shown in the commented code above.</span></span> <span data-ttu-id="1d3ae-145">Сведения о других параметрах конфигурации см. в разделе [Конфигурация](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="1d3ae-145">See [Configuration](xref:fundamentals/configuration/index) for further configuration options.</span></span>

<span data-ttu-id="1d3ae-146">После указания строго типизированного объекта конфигурации (в данном случае `SampleWebSettings`) и добавления его в коллекцию служб вы можете запросить его из любого контроллера или метода действия, запросив экземпляр `IOptions<T>` (в данном случае `IOptions<SampleWebSettings>`).</span><span class="sxs-lookup"><span data-stu-id="1d3ae-146">Once you've specified a strongly-typed configuration object (in this case, `SampleWebSettings`) and added it to the services collection, you can request it from any Controller or Action method by requesting an instance of `IOptions<T>` (in this case, `IOptions<SampleWebSettings>`).</span></span> <span data-ttu-id="1d3ae-147">Следующий код показывает, как запросить параметры из контроллера:</span><span class="sxs-lookup"><span data-stu-id="1d3ae-147">The following code shows how one would request the settings from a controller:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

<span data-ttu-id="1d3ae-148">Соблюдение шаблона параметров позволяет отделить параметры и конфигурацию друг от друга и обеспечивает [разделение функций](http://deviq.com/separation-of-concerns/) для контроллера, так как ему не нужно знать, где и как искать сведения о параметрах.</span><span class="sxs-lookup"><span data-stu-id="1d3ae-148">Following the Options pattern allows settings and configuration to be decoupled from one another, and ensures the controller is following [separation of concerns](http://deviq.com/separation-of-concerns/), since it doesn't need to know how or where to find the settings information.</span></span> <span data-ttu-id="1d3ae-149">Кроме того, это упрощает модульное тестирование контроллера ([Тестирование логики контроллера](testing.md)), так как отсутствует [статическое слипание](http://deviq.com/static-cling/) или прямое создание экземпляров для классов параметров в классе контроллера.</span><span class="sxs-lookup"><span data-stu-id="1d3ae-149">It also makes the controller easier to unit test [Test controller logic](testing.md), since there's no [static cling](http://deviq.com/static-cling/) or direct instantiation of settings classes within the controller class.</span></span>
