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
ms.openlocfilehash: f6b454da838308adddaaddb84073722f647af379
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/28/2017
---
# <a name="dependency-injection-into-controllers"></a>Внедрение зависимостей в контроллерах

<a name=dependency-injection-controllers></a>

По [Стив Смит](https://ardalis.com/)

Контроллеров MVC ASP.NET Core должен запросить явным образом с помощью конструкторов их зависимости. В некоторых случаях действия отдельных контроллера может потребоваться службы, и он не имеет смысл для запроса на уровне контроллера. В этом случае также можно запустить службу как параметр метода действия.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample)

## <a name="dependency-injection"></a>Внедрение зависимостей

Методика внедрения зависимости следует [принципом инверсии зависимостей](http://deviq.com/dependency-inversion-principle/), обеспечивая приложений может состоять из слабо связанных модулей. ASP.NET Core имеет встроенную поддержку [внедрения зависимостей](../../fundamentals/dependency-injection.md), что делает упрощает тестирование и поддержку приложений.

## <a name="constructor-injection"></a>Внедрение конструктора

Встроенная поддержка внедрения зависимостей на основе конструктора ASP.NET Core расширяется до контроллеров MVC. Просто добавляя тип службы в качестве параметра конструктора контроллер, ASP.NET Core будет пытаться разрешить этого типа, используя встроенный в контейнере службы. Службы обычно, но не всегда, задаются с помощью интерфейсов. Например если приложение имеет бизнес-логики, от которого зависит текущее время, можно ввести это служба, которая возвращает время (вместо жестко запрограммированного его), позволяющие тестов для передачи в реализации, использующие определенное время.

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


Реализация интерфейса такого рода, чтобы он использовал системные часы во время выполнения является тривиальным:

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


Это место можно использовать службу в нашем контроллера. В этом случае мы добавили некоторую логику для `HomeController` `Index` метод для отображения приветствия для пользователя на основе времени суток.

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

Если мы запустим приложение, мы скорее возникает ошибка:

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

Эта ошибка возникает, когда мы не настроены службы в `ConfigureServices` метод в нашем `Startup` класса. Для указания, что запросы для `IDateTime` должны разрешаться с использованием экземпляра `SystemDateTime`, добавьте выделенную строку в списке ниже, чтобы ваш `ConfigureServices` метод:

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> Этой конкретной службы могут быть реализованы с помощью любого из нескольких разных время существования параметров (`Transient`, `Scoped`, или `Singleton`). В разделе [внедрения зависимостей](../../fundamentals/dependency-injection.md) чтобы понять, как каждый из этих параметров области влияет на поведение службы.

После настройки службы, работающим с приложением и переход к домашней странице будет выведено время на сообщение должным образом:

![Приветствие сервера](dependency-injection/_static/server-greeting.png)

>[!TIP]
> В разделе [логику контроллера тестирования](testing.md) научиться явно запросить зависимости [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) контроллеры упрощает код для тестирования.

Внедрение встроенных зависимостей ASP.NET Core поддерживает наличие только один конструктор для классов запроса службы. При наличии более одного конструктора, можно получить исключение с сообщением:

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

Как говорится в сообщении об ошибке, можно устранить эту проблему, имеющий один конструктор. Вы также можете [заменить поддержка внедрения зависимостей по умолчанию на реализацию сторонних производителей](../../fundamentals/dependency-injection.md#replacing-the-default-services-container)многие, которые поддерживают несколько конструкторов.

## <a name="action-injection-with-fromservices"></a>Действие с FromServices вставкой

Иногда не требуется служба для более одного действия в контроллере. В этом случае он имеет смысл для добавления службы как параметр методу действия. Это можно сделать, пометив параметр с атрибутом `[FromServices]` как показано ниже:

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a>Доступ к параметрам из контроллера

Доступ к параметрам приложения или конфигурации из контроллера — это общий шаблон. Такой доступ следует использовать шаблон параметры, описанные в [конфигурации](../../fundamentals/configuration.md). Обычно следует не запросить параметры непосредственно из вашего контроллера с помощью внедрения зависимости. Оптимальный подход — запрос `IOptions<T>` экземпляра, где `T` класс конфигурации, необходимо.

Для работы с параметрами шаблона, необходимо создать класс, который представляет параметры, подобные следующему:

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

Вам нужно настроить приложение на использование модели параметры и добавить класс конфигурации для коллекции служб в `ConfigureServices`:

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> В списке выше, Настройка приложения, чтобы считать параметры из файла формата JSON. Можно также настроить параметры полностью в коде, как показано в приведенном выше коде комментариями. В разделе [конфигурации](../../fundamentals/configuration.md) для дальнейшей настройки.

После указания объект строго типизированных конфигурации (в этом случае `SampleWebSettings`) и добавить его в коллекцию служб, можно запросить его из любого контроллеру или методу действия, запросив экземпляр `IOptions<T>` (в этом случае `IOptions<SampleWebSettings>`) . В следующем коде показано, как один запрос параметров из контроллера:

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

Следующие параметры шаблона позволяет параметры и конфигурации, чтобы быть отделены друг от друга и обеспечивает следующие контроллера [Разделение областей ответственности](http://deviq.com/separation-of-concerns/), так как его не нужно знать способы и места для поиска параметров сведения. Также упрощает контроллера модульного теста [логику контроллера тестирования](testing.md), так как она не [статических напечатанными](http://deviq.com/static-cling/) или прямое создание экземпляров классов параметров в класс контроллера.
