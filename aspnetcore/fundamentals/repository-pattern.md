---
title: Реализация шаблона репозитория с помощью ASP.NET Core
author: ardalis
description: Узнайте, как реализовать конструктивный шаблон для приложения репозитория в приложении ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2018
uid: fundamentals/repository-pattern
ms.openlocfilehash: d64c3d090f62c580ebc05a6bbc2744a4508bb9bc
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342695"
---
# <a name="repository-pattern-with-aspnet-core"></a><span data-ttu-id="608cd-103">Реализация шаблона репозитория с помощью ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="608cd-103">Repository pattern with ASP.NET Core</span></span>

<span data-ttu-id="608cd-104">Авторы: [Стив Смит](https://ardalis.com/) (Steve Smith), [Скотт Эдди](https://scottaddie.com) (Scott Addie) и [Люк Латам](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="608cd-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="608cd-105">*Шаблон репозитория* — это конструктивный шаблон, который позволяет изолировать доступ к данным с помощью абстракций интерфейса.</span><span class="sxs-lookup"><span data-stu-id="608cd-105">The *repository pattern* is a design pattern that isolates data access behind interface abstractions.</span></span> <span data-ttu-id="608cd-106">Подключение к базе данных и обработка объектов хранилища данных осуществляется с помощью методов, предоставляемых в реализации интерфейса.</span><span class="sxs-lookup"><span data-stu-id="608cd-106">Connecting to the database and manipulating data storage objects is performed through methods provided by the interface's implementation.</span></span> <span data-ttu-id="608cd-107">Поэтому для работы с базой данных (например, для подключений, команд и читателей) не нужно вызывать код.</span><span class="sxs-lookup"><span data-stu-id="608cd-107">Consequently, there's no need for calling code to deal with database concerns, such as connections, commands, and readers.</span></span>

<span data-ttu-id="608cd-108">Преимущества реализации шаблона репозитория с помощью ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="608cd-108">Implementation of the repository pattern with ASP.NET Core has the following benefits:</span></span>

* <span data-ttu-id="608cd-109">Упрощена организация приложения. Нет прямой взаимозависимости между уровнями бизнес-логики и доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="608cd-109">Organization of the app is less complex with no direct interdependency between the business and data access layers.</span></span>
* <span data-ttu-id="608cd-110">Проще повторно использовать код доступа к базе данных, так как им централизованно управляют один или несколько репозиториев.</span><span class="sxs-lookup"><span data-stu-id="608cd-110">It's easier to reuse database access code because the code is centrally managed by one or more repositories.</span></span>
* <span data-ttu-id="608cd-111">Бизнес-часть кода можно проверять отдельно от уровня базы данных (с помощью модульных тестов).</span><span class="sxs-lookup"><span data-stu-id="608cd-111">The business domain can be independently unit tested from the database layer.</span></span>

<span data-ttu-id="608cd-112">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="608cd-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="608cd-113">В [примере приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) используется шаблон репозитория, который позволяет выполнить инициализацию и вывести список имен персонажей фильма.</span><span class="sxs-lookup"><span data-stu-id="608cd-113">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) uses the repository pattern to initialize and display a list of movie character names.</span></span> <span data-ttu-id="608cd-114">В приложении используется [Entity Framework Core](/ef/core/) и класс `ApplicationDbContext`, чтобы обеспечить сохраняемость данных. Но в инфраструктуре базы данных нельзя четко определить точку доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="608cd-114">The app uses [Entity Framework Core](/ef/core/) and an `ApplicationDbContext` class for its data persistence, but the database infrastructure isn't apparent where the data is accessed.</span></span> <span data-ttu-id="608cd-115">Доступ к данным и объекты базы данных абстрагируются за [репозиторием](https://martinfowler.com/eaaCatalog/repository.html).</span><span class="sxs-lookup"><span data-stu-id="608cd-115">Data access and database objects are abstracted behind a [repository](https://martinfowler.com/eaaCatalog/repository.html).</span></span>

## <a name="repository-interface"></a><span data-ttu-id="608cd-116">Интерфейс репозитория</span><span class="sxs-lookup"><span data-stu-id="608cd-116">Repository interface</span></span>

<span data-ttu-id="608cd-117">Интерфейс репозитория определяет свойства и методы для реализации.</span><span class="sxs-lookup"><span data-stu-id="608cd-117">A repository interface defines the properties and methods for implementation.</span></span> <span data-ttu-id="608cd-118">В примере приложения реализуется интерфейс репозитория `ICharacterRepository` для данных персонажей фильма.</span><span class="sxs-lookup"><span data-stu-id="608cd-118">In the sample app, the repository interface for movie character data is `ICharacterRepository`.</span></span> <span data-ttu-id="608cd-119">`ICharacterRepository` определяет методы `ListAll` и `Add`, необходимые для работы с экземплярами `Character` в приложении:</span><span class="sxs-lookup"><span data-stu-id="608cd-119">`ICharacterRepository` defines the `ListAll` and `Add` methods required to work with `Character` instances in the app:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="608cd-120">`Character` определяется следующим образом.</span><span class="sxs-lookup"><span data-stu-id="608cd-120">`Character` is defined as:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

## <a name="repository-concrete-type"></a><span data-ttu-id="608cd-121">Конкретный тип репозитория</span><span class="sxs-lookup"><span data-stu-id="608cd-121">Repository concrete type</span></span>

<span data-ttu-id="608cd-122">Этот интерфейс реализуется с помощью конкретного типа.</span><span class="sxs-lookup"><span data-stu-id="608cd-122">The interface is implemented by a concrete type.</span></span> <span data-ttu-id="608cd-123">В примере приложения `CharacterRepository` управляет контекстом базы данных и реализует методы `ListAll` и `Add`:</span><span class="sxs-lookup"><span data-stu-id="608cd-123">In the sample app, `CharacterRepository` manages the database context and implements the `ListAll` and `Add` methods:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

## <a name="register-the-repository-service"></a><span data-ttu-id="608cd-124">Регистрация службы репозитория</span><span class="sxs-lookup"><span data-stu-id="608cd-124">Register the repository service</span></span>

<span data-ttu-id="608cd-125">Контекст репозитория и базы данных регистрируются с помощью контейнера службы в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="608cd-125">The repository and database context are registered with the service container in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="608cd-126">В примере приложения настраивается `ApplicationDbContext` для вызова метода расширения [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext).</span><span class="sxs-lookup"><span data-stu-id="608cd-126">In the sample app, `ApplicationDbContext` is configured with the call to the extension method [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext).</span></span> <span data-ttu-id="608cd-127">`ICharacterRepository` регистрируется как ограниченная служба:</span><span class="sxs-lookup"><span data-stu-id="608cd-127">`ICharacterRepository` is registered as a scoped service:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,18)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,12)]

::: moniker-end

## <a name="inject-an-instance-of-the-repository"></a><span data-ttu-id="608cd-128">Внедрение экземпляра репозитория</span><span class="sxs-lookup"><span data-stu-id="608cd-128">Inject an instance of the repository</span></span>

<span data-ttu-id="608cd-129">В классе, где требуется доступ к базе данных, экземпляр репозитория запрашивается через конструктор и назначается скрытому полю для использования методами класса.</span><span class="sxs-lookup"><span data-stu-id="608cd-129">In a class where database access is required, an instance of the repository is requested via the constructor and assigned to a private field for use by class methods.</span></span> <span data-ttu-id="608cd-130">В примере приложения `ICharacterRepository` используется для:</span><span class="sxs-lookup"><span data-stu-id="608cd-130">In the sample app, `ICharacterRepository` is used to:</span></span>

* <span data-ttu-id="608cd-131">заполнения базы данных при отсутствии персонажей;</span><span class="sxs-lookup"><span data-stu-id="608cd-131">Populate the database if no characters exist.</span></span>
* <span data-ttu-id="608cd-132">получения списка персонажей для отображения.</span><span class="sxs-lookup"><span data-stu-id="608cd-132">Obtain a list of the characters for display.</span></span>

<span data-ttu-id="608cd-133">Обратите внимание, что вызывающий код взаимодействует только с реализацией интерфейса `CharacterRepository`.</span><span class="sxs-lookup"><span data-stu-id="608cd-133">Notice how the calling code only interacts with the interface's implementation, `CharacterRepository`.</span></span> <span data-ttu-id="608cd-134">В вызывающем коде не используется `ApplicationDbContext` напрямую:</span><span class="sxs-lookup"><span data-stu-id="608cd-134">Calling code doesn't use the `ApplicationDbContext` directly:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Pages/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Controllers/HomeController.cs?name=snippet1)]

::: moniker-end

## <a name="generic-repository-interface"></a><span data-ttu-id="608cd-135">Универсальный интерфейс репозитория</span><span class="sxs-lookup"><span data-stu-id="608cd-135">Generic repository interface</span></span>

<span data-ttu-id="608cd-136">В этой статье и представленном в ней примере приложения показана простейшая реализация шаблона репозитория, где для каждого бизнес-объекта создается отдельный репозиторий.</span><span class="sxs-lookup"><span data-stu-id="608cd-136">This topic and its sample app demonstrate the simplest implementation of the repository pattern, where one repository is created for each business object.</span></span> <span data-ttu-id="608cd-137">Если в приложении используется большое число объектов, можно уменьшить объем кода, необходимого для реализации шаблона репозитория, с помощью *универсального интерфейса репозитория*.</span><span class="sxs-lookup"><span data-stu-id="608cd-137">If the app grows beyond a few objects, a *generic repository interface* may reduce the amount of code required to implement the repository pattern.</span></span> <span data-ttu-id="608cd-138">Дополнительные сведения см. в статье об [универсальном интерфейсе для шаблона репозитория на сайте DevIQ](http://deviq.com/repository-pattern/).</span><span class="sxs-lookup"><span data-stu-id="608cd-138">For more information, see [DevIQ: Repository Pattern: Generic Repository Interface](http://deviq.com/repository-pattern/).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="608cd-139">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="608cd-139">Additional resources</span></span>

* [<span data-ttu-id="608cd-140">DevIQ: шаблон репозитория</span><span class="sxs-lookup"><span data-stu-id="608cd-140">DevIQ: Repository Pattern</span></span>](https://deviq.com/repository-pattern/)
* [<span data-ttu-id="608cd-141">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="608cd-141">Dependency injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="608cd-142">Внедрение зависимостей в представления</span><span class="sxs-lookup"><span data-stu-id="608cd-142">Dependency injection into views</span></span>](xref:mvc/views/dependency-injection)
* [<span data-ttu-id="608cd-143">Внедрение зависимостей в контроллеры</span><span class="sxs-lookup"><span data-stu-id="608cd-143">Dependency injection into controllers</span></span>](xref:mvc/controllers/dependency-injection)
* [<span data-ttu-id="608cd-144">Внедрение зависимостей в обработчики требований</span><span class="sxs-lookup"><span data-stu-id="608cd-144">Dependency injection in requirement handlers</span></span>](xref:security/authorization/dependencyinjection)
* [<span data-ttu-id="608cd-145">Контейнеры с инверсией управления и шаблон внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="608cd-145">Inversion of Control Containers and the Dependency Injection Pattern</span></span>](https://www.martinfowler.com/articles/injection.html)
