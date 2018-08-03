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
# <a name="repository-pattern-with-aspnet-core"></a>Реализация шаблона репозитория с помощью ASP.NET Core

Авторы: [Стив Смит](https://ardalis.com/) (Steve Smith), [Скотт Эдди](https://scottaddie.com) (Scott Addie) и [Люк Латам](https://github.com/guardrex) (Luke Latham)

*Шаблон репозитория* — это конструктивный шаблон, который позволяет изолировать доступ к данным с помощью абстракций интерфейса. Подключение к базе данных и обработка объектов хранилища данных осуществляется с помощью методов, предоставляемых в реализации интерфейса. Поэтому для работы с базой данных (например, для подключений, команд и читателей) не нужно вызывать код.

Преимущества реализации шаблона репозитория с помощью ASP.NET Core:

* Упрощена организация приложения. Нет прямой взаимозависимости между уровнями бизнес-логики и доступа к данным.
* Проще повторно использовать код доступа к базе данных, так как им централизованно управляют один или несколько репозиториев.
* Бизнес-часть кода можно проверять отдельно от уровня базы данных (с помощью модульных тестов).

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

В [примере приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) используется шаблон репозитория, который позволяет выполнить инициализацию и вывести список имен персонажей фильма. В приложении используется [Entity Framework Core](/ef/core/) и класс `ApplicationDbContext`, чтобы обеспечить сохраняемость данных. Но в инфраструктуре базы данных нельзя четко определить точку доступа к данным. Доступ к данным и объекты базы данных абстрагируются за [репозиторием](https://martinfowler.com/eaaCatalog/repository.html).

## <a name="repository-interface"></a>Интерфейс репозитория

Интерфейс репозитория определяет свойства и методы для реализации. В примере приложения реализуется интерфейс репозитория `ICharacterRepository` для данных персонажей фильма. `ICharacterRepository` определяет методы `ListAll` и `Add`, необходимые для работы с экземплярами `Character` в приложении:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

`Character` определяется следующим образом.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

## <a name="repository-concrete-type"></a>Конкретный тип репозитория

Этот интерфейс реализуется с помощью конкретного типа. В примере приложения `CharacterRepository` управляет контекстом базы данных и реализует методы `ListAll` и `Add`:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

## <a name="register-the-repository-service"></a>Регистрация службы репозитория

Контекст репозитория и базы данных регистрируются с помощью контейнера службы в `Startup.ConfigureServices`. В примере приложения настраивается `ApplicationDbContext` для вызова метода расширения [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext). `ICharacterRepository` регистрируется как ограниченная служба:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,18)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,12)]

::: moniker-end

## <a name="inject-an-instance-of-the-repository"></a>Внедрение экземпляра репозитория

В классе, где требуется доступ к базе данных, экземпляр репозитория запрашивается через конструктор и назначается скрытому полю для использования методами класса. В примере приложения `ICharacterRepository` используется для:

* заполнения базы данных при отсутствии персонажей;
* получения списка персонажей для отображения.

Обратите внимание, что вызывающий код взаимодействует только с реализацией интерфейса `CharacterRepository`. В вызывающем коде не используется `ApplicationDbContext` напрямую:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Pages/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Controllers/HomeController.cs?name=snippet1)]

::: moniker-end

## <a name="generic-repository-interface"></a>Универсальный интерфейс репозитория

В этой статье и представленном в ней примере приложения показана простейшая реализация шаблона репозитория, где для каждого бизнес-объекта создается отдельный репозиторий. Если в приложении используется большое число объектов, можно уменьшить объем кода, необходимого для реализации шаблона репозитория, с помощью *универсального интерфейса репозитория*. Дополнительные сведения см. в статье об [универсальном интерфейсе для шаблона репозитория на сайте DevIQ](http://deviq.com/repository-pattern/).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [DevIQ: шаблон репозитория](https://deviq.com/repository-pattern/)
* [Внедрение зависимостей](xref:fundamentals/dependency-injection)
* [Внедрение зависимостей в представления](xref:mvc/views/dependency-injection)
* [Внедрение зависимостей в контроллеры](xref:mvc/controllers/dependency-injection)
* [Внедрение зависимостей в обработчики требований](xref:security/authorization/dependencyinjection)
* [Контейнеры с инверсией управления и шаблон внедрения зависимостей](https://www.martinfowler.com/articles/injection.html)
