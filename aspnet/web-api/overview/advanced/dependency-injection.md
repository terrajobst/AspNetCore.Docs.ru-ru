---
uid: web-api/overview/advanced/dependency-injection
title: Внедрение зависимостей в ASP.NET Web API 2 | Документация Майкрософт
author: MikeWasson
description: Этом руководстве показано, как внедрить зависимости в контроллере веб-API ASP.NET. Версии программного обеспечения, используемые в руководстве Web API 2 Unity Application Block...
ms.author: riande
ms.date: 01/20/2014
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 318a2f1c587feb360212a390bb5de7bdc127513d
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341830"
---
<a name="dependency-injection-in-aspnet-web-api-2"></a>Внедрение зависимостей в ASP.NET Web API 2
====================
по [Майк Уоссон](https://github.com/MikeWasson)

[Скачать завершенный проект](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> Этом руководстве показано, как внедрить зависимости в контроллере веб-API ASP.NET.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемые в этом руководстве
> 
> 
> - Веб-API 2
> - [Unity Application Block](https://www.nuget.org/packages/Unity/)
> - Entity Framework 6 (версии 5 также работает)


## <a name="what-is-dependency-injection"></a>Что такое внедрение зависимостей?

*Зависимость* — это любой объект, который требуется другому объекту. Например, довольно часто для определения [репозитория](http://martinfowler.com/eaaCatalog/repository.html) , служащий для обработки доступа к данным. Давайте рассмотрим пример. Во-первых мы определим модель предметной области:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

Ниже приведен простой репозиторий класс, который хранит элементы в базе данных, использующий Entity Framework.

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Теперь давайте определим контроллер веб-API, который поддерживает запросы GET для `Product` сущностей. (Я пропускают POST и другие методы для простоты.) Ниже приведен первой попытки.

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Обратите внимание, что зависит от класса контроллера `ProductRepository`, и мы сообщаем создать контроллер `ProductRepository` экземпляра. Тем не менее рекомендуется явно указывать зависимость таким образом, это по следующим причинам.

- Если вы хотите заменить `ProductRepository` другой реализацией, необходимо также изменить класс контроллера.
- Если `ProductRepository` имеет зависимости, их необходимо настроить в контроллере. Для большого проекта с несколькими контроллерами код конфигурации становится разбиты на проект.
- Трудно модульного теста, так как контроллер, жестко заданные для запроса базы данных. Для модульного теста следует использовать репозитории макет или заглушки, что невозможно при помощи конструктора currect.

Можно устранить эти проблемы путем *внедрение* репозитория в контроллер. Во-первых, рефакторинг `ProductRepository` класс в интерфейсе:

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Затем укажите `IProductRepository` качестве параметра конструктора:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

В этом примере используется [внедрение через конструктор](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Можно также использовать *внедрения setter*, где можно устанавливать для зависимости через метод или свойство.

Но теперь имеется проблема, поскольку приложение не создает контроллер напрямую. Веб-API создает контроллер, когда он направляет запрос и веб-API не знает ничего о `IProductRepository`. Это, когда вступает в дело в сопоставителе зависимостей веб-API.

## <a name="the-web-api-dependency-resolver"></a>Сопоставитель зависимостей веб-API

Веб-API определяет **IDependencyResolver** интерфейс для разрешения зависимостей. Вот определение интерфейса:

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

**IDependencyScope** интерфейс содержит два метода:

- **GetService** создает один экземпляр типа.
- **GetServices** создает коллекцию объектов указанного типа.

**IDependencyResolver** наследует метод **IDependencyScope** и добавляет **BeginScope** метод. Я расскажу об областях далее в этом руководстве.

Когда веб-API создает экземпляр контроллера, в первую очередь вызывает **IDependencyResolver.GetService**, передав тип контроллера. Этот обработчик расширяемости можно использовать для создания контроллера, разрешение всех зависимостей. Если **GetService** возвращает значение null, ищет конструктор класса контроллера в веб-API.

## <a name="dependency-resolution-with-the-unity-container"></a>Разрешение зависимостей для контейнера Unity

Несмотря на то, что можно написать полное **IDependencyResolver** реализация с нуля, интерфейс действительно предназначена для работы в качестве моста между веб-API и существующие контейнеры IoC.

IoC-контейнер — это программный компонент, который отвечает за управление зависимостями. Зарегистрируйте типов в контейнере и затем использовать для создания объектов контейнера. Контейнер автоматически определяет отношение зависимость. Кроме того, множество контейнеров инверсии управления позволяют управлять времени существования объектов и области.

> [!NOTE]
> «IoC» означает «инверсии элемента управления», который является общий шаблон, когда платформа вызывает в код приложения. Контейнер IoC создает объектов, который «инвертирует» обычного потока управления.


В этом учебнике мы будем использовать [Unity](https://msdn.microsoft.com/library/ff647202.aspx) из Microsoft Patterns &amp; рекомендации. (Включить других популярных библиотек [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), и [StructureMap ](http://structuremap.github.io/documentation/).) Можно использовать диспетчер пакетов NuGet для установки Unity. Из **средства** меню в Visual Studio, выберите пункт **диспетчер пакетов NuGet**, а затем выберите **консоль диспетчера пакетов**. В окне консоли диспетчера пакетов введите следующую команду:

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

Вот реализация **IDependencyResolver** , который служит оболочкой контейнера Unity.

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> Если **GetService** метод не может разрешить тип, он должен вернуть **null**. Если **GetServices** метод не может разрешить тип, она должна возвращать пустой объект коллекции. Не создают исключения для неизвестных типов.


## <a name="configuring-the-dependency-resolver"></a>Настройка сопоставителя зависимостей

Установите средство разрешения зависимостей на **DependencyResolver** свойства глобального **HttpConfiguration** объекта.

В следующем коде регистрах `IProductRepository` интерфейса с помощью Unity, а затем создает `UnityResolver`.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a>Область зависимостей и временем существования контроллера

Контроллеры создаются отдельно для каждого запроса. Для управления временем существования объектов, **IDependencyResolver** использует концепцию *область*.

Сопоставитель зависимостей подключен к **HttpConfiguration** объект имеет глобальную область. Когда веб-API создает контроллер, он вызывает **BeginScope**. Этот метод возвращает **IDependencyScope** , представляющий дочерней области.

Веб-API, затем вызывает **GetService** в дочерней области для создания контроллера. По завершении запроса вызывает веб-API **Dispose** в дочерней области. Используйте **Dispose** метод для удаления зависимостей контроллера.

Как реализовать **BeginScope** зависит от контейнера IoC. Для Unity область соответствует в дочернем контейнере:

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

Большинство IoC-контейнеры имеют аналогичные эквиваленты.
