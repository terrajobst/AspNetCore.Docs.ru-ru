---
title: "Внедрение зависимостей в контроллерах"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: bc8b4ba3-e9ba-48fd-b1eb-cd48ff6bc7a1
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 4c632f521cf314bcf8c84f40c52a580a26a5ceee
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/01/2017
---
# <a name="dependency-injection-into-controllers"></a><span data-ttu-id="ba562-103">Внедрение зависимостей в контроллерах</span><span class="sxs-lookup"><span data-stu-id="ba562-103">Dependency injection into controllers</span></span>

<a name=dependency-injection-controllers></a>

<span data-ttu-id="ba562-104">По [Стив Смит](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="ba562-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="ba562-105">Контроллеров MVC ASP.NET Core должен запросить явным образом с помощью конструкторов их зависимости.</span><span class="sxs-lookup"><span data-stu-id="ba562-105">ASP.NET Core MVC controllers should request their dependencies explicitly via their constructors.</span></span> <span data-ttu-id="ba562-106">В некоторых случаях действия отдельных контроллера может потребоваться службы, и он не имеет смысл для запроса на уровне контроллера.</span><span class="sxs-lookup"><span data-stu-id="ba562-106">In some instances, individual controller actions may require a service, and it may not make sense to request at the controller level.</span></span> <span data-ttu-id="ba562-107">В этом случае также можно запустить службу как параметр метода действия.</span><span class="sxs-lookup"><span data-stu-id="ba562-107">In this case, you can also choose to inject a service as a parameter on the action method.</span></span>

<span data-ttu-id="ba562-108">[Просмотреть или загрузить образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([загрузке](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ba562-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="ba562-109">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="ba562-109">Dependency Injection</span></span>

<span data-ttu-id="ba562-110">Методика внедрения зависимости следует [принципом инверсии зависимостей](http://deviq.com/dependency-inversion-principle/), обеспечивая приложений может состоять из слабо связанных модулей.</span><span class="sxs-lookup"><span data-stu-id="ba562-110">Dependency injection is a technique that follows the [Dependency Inversion Principle](http://deviq.com/dependency-inversion-principle/), allowing for applications to be composed of loosely coupled modules.</span></span> <span data-ttu-id="ba562-111">ASP.NET Core имеет встроенную поддержку [внедрения зависимостей](../../fundamentals/dependency-injection.md), что делает упрощает тестирование и поддержку приложений.</span><span class="sxs-lookup"><span data-stu-id="ba562-111">ASP.NET Core has built-in support for [dependency injection](../../fundamentals/dependency-injection.md), which makes applications easier to test and maintain.</span></span>

## <a name="constructor-injection"></a><span data-ttu-id="ba562-112">Внедрение конструктора</span><span class="sxs-lookup"><span data-stu-id="ba562-112">Constructor Injection</span></span>

<span data-ttu-id="ba562-113">Встроенная поддержка внедрения зависимостей на основе конструктора ASP.NET Core расширяется до контроллеров MVC.</span><span class="sxs-lookup"><span data-stu-id="ba562-113">ASP.NET Core's built-in support for constructor-based dependency injection extends to MVC controllers.</span></span> <span data-ttu-id="ba562-114">Просто добавляя тип службы в качестве параметра конструктора контроллер, ASP.NET Core будет пытаться разрешить этого типа, используя встроенный в контейнере службы.</span><span class="sxs-lookup"><span data-stu-id="ba562-114">By simply adding a service type to your controller as a constructor parameter, ASP.NET Core will attempt to resolve that type using its built in service container.</span></span> <span data-ttu-id="ba562-115">Службы обычно, но не всегда, задаются с помощью интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="ba562-115">Services are typically, but not always, defined using interfaces.</span></span> <span data-ttu-id="ba562-116">Например если приложение имеет бизнес-логики, от которого зависит текущее время, можно ввести это служба, которая возвращает время (вместо жестко запрограммированного его), позволяющие тестов для передачи в реализации, использующие определенное время.</span><span class="sxs-lookup"><span data-stu-id="ba562-116">For example, if your application has business logic that depends on the current time, you can inject a service that retrieves the time (rather than hard-coding it), which would allow your tests to pass in implementations that use a set time.</span></span>

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


<span data-ttu-id="ba562-117">Реализация интерфейса такого рода, чтобы он использовал системные часы во время выполнения является тривиальным:</span><span class="sxs-lookup"><span data-stu-id="ba562-117">Implementing an interface like this one so that it uses the system clock at runtime is trivial:</span></span>

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


<span data-ttu-id="ba562-118">Это место можно использовать службу в нашем контроллера.</span><span class="sxs-lookup"><span data-stu-id="ba562-118">With this in place, we can use the service in our controller.</span></span> <span data-ttu-id="ba562-119">В этом случае мы добавили некоторую логику для `HomeController` `Index` метод для отображения приветствия для пользователя на основе времени суток.</span><span class="sxs-lookup"><span data-stu-id="ba562-119">In this case, we have added some logic to the `HomeController` `Index` method to display a greeting to the user based on the time of day.</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

<span data-ttu-id="ba562-120">Если мы запустим приложение, мы скорее возникает ошибка:</span><span class="sxs-lookup"><span data-stu-id="ba562-120">If we run the application now, we will most likely encounter an error:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

<span data-ttu-id="ba562-121">Эта ошибка возникает, когда мы не настроены службы в `ConfigureServices` метод в нашем `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="ba562-121">This error occurs when we have not configured a service in the `ConfigureServices` method in our `Startup` class.</span></span> <span data-ttu-id="ba562-122">Для указания, что запросы для `IDateTime` должны разрешаться с использованием экземпляра `SystemDateTime`, добавьте выделенную строку в списке ниже, чтобы ваш `ConfigureServices` метод:</span><span class="sxs-lookup"><span data-stu-id="ba562-122">To specify that requests for `IDateTime` should be resolved using an instance of `SystemDateTime`, add the highlighted line in the listing below to your `ConfigureServices` method:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> <span data-ttu-id="ba562-123">Этой конкретной службы могут быть реализованы с помощью любого из нескольких разных время существования параметров (`Transient`, `Scoped`, или `Singleton`).</span><span class="sxs-lookup"><span data-stu-id="ba562-123">This particular service could be implemented using any of several different lifetime options (`Transient`, `Scoped`, or `Singleton`).</span></span> <span data-ttu-id="ba562-124">В разделе [внедрения зависимостей](../../fundamentals/dependency-injection.md) чтобы понять, как каждый из этих параметров области влияет на поведение службы.</span><span class="sxs-lookup"><span data-stu-id="ba562-124">See [Dependency Injection](../../fundamentals/dependency-injection.md) to understand how each of these scope options will affect the behavior of your service.</span></span>

<span data-ttu-id="ba562-125">После настройки службы, работающим с приложением и переход к домашней странице будет выведено время на сообщение должным образом:</span><span class="sxs-lookup"><span data-stu-id="ba562-125">Once the service has been configured, running the application and navigating to the home page should display the time-based message as expected:</span></span>

![Приветствие сервера](dependency-injection/_static/server-greeting.png)

>[!TIP]
> <span data-ttu-id="ba562-127">В разделе [логику контроллера тестирования](testing.md) научиться явно запросить зависимости [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) контроллеры упрощает код для тестирования.</span><span class="sxs-lookup"><span data-stu-id="ba562-127">See [Testing Controller Logic](testing.md) to learn how to explicitly request dependencies [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) in controllers makes code easier to test.</span></span>

<span data-ttu-id="ba562-128">Внедрение встроенных зависимостей ASP.NET Core поддерживает наличие только один конструктор для классов запроса службы.</span><span class="sxs-lookup"><span data-stu-id="ba562-128">ASP.NET Core's built-in dependency injection supports having only a single constructor for classes requesting services.</span></span> <span data-ttu-id="ba562-129">При наличии более одного конструктора, можно получить исключение с сообщением:</span><span class="sxs-lookup"><span data-stu-id="ba562-129">If you have more than one constructor, you may get an exception stating:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

<span data-ttu-id="ba562-130">Как говорится в сообщении об ошибке, можно устранить эту проблему, имеющий один конструктор.</span><span class="sxs-lookup"><span data-stu-id="ba562-130">As the error message states, you can correct this problem having just a single constructor.</span></span> <span data-ttu-id="ba562-131">Вы также можете [заменить поддержка внедрения зависимостей по умолчанию на реализацию сторонних производителей](../../fundamentals/dependency-injection.md#replacing-the-default-services-container)многие, которые поддерживают несколько конструкторов.</span><span class="sxs-lookup"><span data-stu-id="ba562-131">You can also [replace the default dependency injection support with a third party implementation](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), many of which support multiple constructors.</span></span>

## <a name="action-injection-with-fromservices"></a><span data-ttu-id="ba562-132">Действие с FromServices вставкой</span><span class="sxs-lookup"><span data-stu-id="ba562-132">Action Injection with FromServices</span></span>

<span data-ttu-id="ba562-133">Иногда не требуется служба для более одного действия в контроллере.</span><span class="sxs-lookup"><span data-stu-id="ba562-133">Sometimes you don't need a service for more than one action within your controller.</span></span> <span data-ttu-id="ba562-134">В этом случае он имеет смысл для добавления службы как параметр методу действия.</span><span class="sxs-lookup"><span data-stu-id="ba562-134">In this case, it may make sense to inject the service as a parameter to the action method.</span></span> <span data-ttu-id="ba562-135">Это можно сделать, пометив параметр с атрибутом `[FromServices]` как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="ba562-135">This is done by marking the parameter with the attribute `[FromServices]` as shown here:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a><span data-ttu-id="ba562-136">Доступ к параметрам из контроллера</span><span class="sxs-lookup"><span data-stu-id="ba562-136">Accessing Settings from a Controller</span></span>

<span data-ttu-id="ba562-137">Доступ к параметрам приложения или конфигурации из контроллера — это общий шаблон.</span><span class="sxs-lookup"><span data-stu-id="ba562-137">Accessing application or configuration settings from within a controller is a common pattern.</span></span> <span data-ttu-id="ba562-138">Такой доступ следует использовать шаблон параметры, описанные в [конфигурации](../../fundamentals/configuration.md).</span><span class="sxs-lookup"><span data-stu-id="ba562-138">This access should use the Options pattern described in [configuration](../../fundamentals/configuration.md).</span></span> <span data-ttu-id="ba562-139">Обычно следует не запросить параметры непосредственно из вашего контроллера с помощью внедрения зависимости.</span><span class="sxs-lookup"><span data-stu-id="ba562-139">You generally should not request settings directly from your controller using dependency injection.</span></span> <span data-ttu-id="ba562-140">Оптимальный подход — запрос `IOptions<T>` экземпляра, где `T` класс конфигурации, необходимо.</span><span class="sxs-lookup"><span data-stu-id="ba562-140">A better approach is to request an `IOptions<T>` instance, where `T` is the configuration class you need.</span></span>

<span data-ttu-id="ba562-141">Для работы с параметрами шаблона, необходимо создать класс, который представляет параметры, подобные следующему:</span><span class="sxs-lookup"><span data-stu-id="ba562-141">To work with the options pattern, you need to create a class that represents the options, such as this one:</span></span>

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

<span data-ttu-id="ba562-142">Вам нужно настроить приложение на использование модели параметры и добавить класс конфигурации для коллекции служб в `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ba562-142">Then you need to configure the application to use the options model and add your configuration class to the services collection in `ConfigureServices`:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> <span data-ttu-id="ba562-143">В списке выше, Настройка приложения, чтобы считать параметры из файла формата JSON.</span><span class="sxs-lookup"><span data-stu-id="ba562-143">In the above listing, we are configuring the application to read the settings from a JSON-formatted file.</span></span> <span data-ttu-id="ba562-144">Можно также настроить параметры полностью в коде, как показано в приведенном выше коде комментариями.</span><span class="sxs-lookup"><span data-stu-id="ba562-144">You can also configure the settings entirely in code, as is shown in the commented code above.</span></span> <span data-ttu-id="ba562-145">В разделе [конфигурации](../../fundamentals/configuration.md) для дальнейшей настройки.</span><span class="sxs-lookup"><span data-stu-id="ba562-145">See [Configuration](../../fundamentals/configuration.md) for further configuration options.</span></span>

<span data-ttu-id="ba562-146">После указания объект строго типизированных конфигурации (в этом случае `SampleWebSettings`) и добавить его в коллекцию служб, можно запросить его из любого контроллеру или методу действия, запросив экземпляр `IOptions<T>` (в этом случае `IOptions<SampleWebSettings>`) .</span><span class="sxs-lookup"><span data-stu-id="ba562-146">Once you've specified a strongly-typed configuration object (in this case, `SampleWebSettings`) and added it to the services collection, you can request it from any Controller or Action method by requesting an instance of `IOptions<T>` (in this case, `IOptions<SampleWebSettings>`).</span></span> <span data-ttu-id="ba562-147">В следующем коде показано, как один запрос параметров из контроллера:</span><span class="sxs-lookup"><span data-stu-id="ba562-147">The following code shows how one would request the settings from a controller:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

<span data-ttu-id="ba562-148">Следующие параметры шаблона позволяет параметры и конфигурации, чтобы быть отделены друг от друга и обеспечивает следующие контроллера [Разделение областей ответственности](http://deviq.com/separation-of-concerns/), так как его не нужно знать способы и места для поиска параметров сведения.</span><span class="sxs-lookup"><span data-stu-id="ba562-148">Following the Options pattern allows settings and configuration to be decoupled from one another, and ensures the controller is following [separation of concerns](http://deviq.com/separation-of-concerns/), since it doesn't need to know how or where to find the settings information.</span></span> <span data-ttu-id="ba562-149">Также упрощает контроллера модульного теста [логику контроллера тестирования](testing.md), так как она не [статических напечатанными](http://deviq.com/static-cling/) или прямое создание экземпляров классов параметров в класс контроллера.</span><span class="sxs-lookup"><span data-stu-id="ba562-149">It also makes the controller easier to unit test [Testing Controller Logic](testing.md), since there is no [static cling](http://deviq.com/static-cling/) or direct instantiation of settings classes within the controller class.</span></span>
