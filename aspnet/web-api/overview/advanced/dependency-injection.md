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
ms.openlocfilehash: 7f64cc83e36c80b0ffd53edfc629557c0847b200
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="dependency-injection-in-aspnet-web-api-2"></a>Внедрение зависимостей в ASP.NET Web API 2
====================
по [Mike Wasson](https://github.com/MikeWasson)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> Этот учебник показывает, как для вставки зависимости на контроллере веб-API ASP.NET.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемая в этом учебнике
> 
> 
> - Веб-API 2
> - [Блок приложения Unity](https://www.nuget.org/packages/Unity/)
> - Entity Framework 6 (версия 5 также работает)


## <a name="what-is-dependency-injection"></a>Что такое внедрения зависимости

Объект *зависимостей* — это любой объект, который требует другой объект. Например, общие для определения [репозитория](http://martinfowler.com/eaaCatalog/repository.html) , обрабатывает доступ к данным. Давайте рассмотрим пример. Во-первых мы определим модель домена:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

Ниже приведен простой репозиторий класс, который хранит элементы в базе данных, использующий Entity Framework.

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Теперь давайте определить контроллер веб-API, который поддерживает запросы GET для `Product` сущностей. (Я пропускают POST, а также другие методы для простоты.) Вот первой попытки.

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Обратите внимание, что зависит от класса контроллера `ProductRepository`, и мы позволяя создать контроллер `ProductRepository` экземпляра. Однако это рекомендуется жестко зависимостей таким образом, по следующим причинам.

- Если вы хотите заменить `ProductRepository` с другой реализации, необходимо также изменить класс контроллера.
- Если `ProductRepository` имеет зависимости, необходимо настроить в контроллере. Для больших проектов с несколькими контроллерами код конфигурации становится в разных частях проекта.
- Трудно модульный тест, так как данный контроллер является жестко запрограммировано на запрос в базу данных. Для модульного теста следует использовать репозитории макетов или заглушки, что невозможно при помощи конструктора currect.

Можно устранить эти проблемы с *вводится* репозитория в контроллер. Во-первых, рефакторинг `ProductRepository` класс в интерфейсе:

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Затем укажите `IProductRepository` как параметр конструктора:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

В этом примере используется [внедрение конструктора](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Можно также использовать *внедрения setter*, где установить зависимость через метод или свойство.

Но теперь имеется проблема, поскольку приложение не создает контроллер напрямую. Создает веб-API контроллер, когда он направляет запрос и веб-API ничего не знает о `IProductRepository`. Это происходит, когда вступает в дело Сопоставитель зависимостей веб-API.

## <a name="the-web-api-dependency-resolver"></a>Сопоставитель зависимостей веб-API

Определяет веб-API **IDependencyResolver** интерфейс для разрешении зависимостей. Вот определение интерфейса:

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

**IDependencyScope** интерфейса есть два метода:

- **GetService** создает один экземпляр типа.
- **Метод GetServices** создает коллекцию объектов определенного типа.

**IDependencyResolver** наследует метод **IDependencyScope** и добавляет **BeginScope** метод. Мы поговорим об областях далее в этом учебнике.

Если веб-API создает экземпляр контроллера, он сначала вызывает **IDependencyResolver.GetService**, передав тип контроллера. Этот обработчик расширения можно использовать для создания контроллера, разрешаются любые зависимости. Если **GetService** возвращает значение null, веб-API ищет конструктор в класс контроллера.

## <a name="dependency-resolution-with-the-unity-container"></a>Разрешение зависимостей с контейнером Unity

Несмотря на то, что можно написать полный **IDependencyResolver** действительно реализацию с нуля, интерфейс предназначен для работы в качестве моста между веб-API и существующие контейнеры IoC.

Контейнер IoC — это программный компонент, который отвечает за управление зависимостями. Зарегистрировать типы с контейнером и затем использовать для создания объектов контейнера. Контейнер автоматически решает, отношений зависимостей. Многие контейнеры IoC также позволяют управлять вещей, как время существования объектов и области.

> [!NOTE]
> «IoC» означает «инверсии элемента управления», который является общий шаблон, когда платформа вызывает в код приложения. Контейнер IoC создает объекты, который «инвертирует «обычного потока управления.


В этом учебнике мы будем использовать [Unity](https://msdn.microsoft.com/library/ff647202.aspx) из шаблонов Microsoft &amp; рекомендации. (Другие популярные библиотеки включают в себя [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), и [StructureMap ](http://docs.structuremap.net/).) Установка Unity можно использовать диспетчер пакетов NuGet. Из **средства** в Visual Studio, выберите пункт меню **диспетчер пакетов библиотеки**, а затем выберите **консоль диспетчера пакетов**. В окне консоли диспетчера пакетов введите следующую команду:

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

Здесь — это реализация **IDependencyResolver** -оболочки контейнера Unity.

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> Если **GetService** метод не может разрешить тип, он должен возвращать **null**. Если **GetServices** метод не может разрешить тип, он должен возвращать пустой объект коллекции. Не следует создавать исключения для неизвестных типов.


## <a name="configuring-the-dependency-resolver"></a>Настройка сопоставителя зависимостей

Установить средство разрешения зависимостей для **DependencyResolver** свойство глобального **HttpConfiguration** объекта.

В следующем примере кода регистры `IProductRepository` взаимодействовать с Unity, а затем создает `UnityResolver`.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a>Область зависимостей и временем существования контроллера

Контроллеры создаются для каждого запроса. Для управления временем жизни объектов, **IDependencyResolver** использует концепцию *область*.

Сопоставитель зависимостей присоединяется к **HttpConfiguration** объект имеет глобальную область. Если веб-API будет создан контроллер, он вызывает **BeginScope**. Этот метод возвращает **IDependencyScope** , представляющий дочерней области.

Затем вызывает веб-API **GetService** в дочерней области для создания контроллера. При запросе веб-API вызывает **Dispose** в дочерней области. Используйте **Dispose** метод для удаления зависимостей контроллера.

Реализация **BeginScope** зависит от контейнер IoC. Область для Unity, соответствует дочернего контейнера:

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

Большинство IoC контейнеры имеют аналогичные эквиваленты.
