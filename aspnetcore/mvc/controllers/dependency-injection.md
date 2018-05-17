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
# <a name="dependency-injection-into-controllers-in-aspnet-core"></a>Внедрение зависимостей в контроллеры в ASP.NET Core

<a name="dependency-injection-controllers"></a>

Автор: [Стив Смит](https://ardalis.com/) (Steve Smith)

Контроллеры ASP.NET Core MVC должны запрашивать свои зависимости явно с помощью конструкторов. В некоторых случаях отдельные действия контроллера могут потребовать службу, которую не имеет смысла запрашивать на уровне контроллера. В этой ситуации также можно внедрить службу в качестве параметра метода действия.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="dependency-injection"></a>Внедрение зависимостей

Методика внедрения зависимости следует [принципу инверсии зависимостей](http://deviq.com/dependency-inversion-principle/), позволяя приложениям состоять из слабо связанных модулей. ASP.NET Core имеет встроенную поддержку [внедрения зависимостей](../../fundamentals/dependency-injection.md), что упрощает тестирование и обслуживание приложений.

## <a name="constructor-injection"></a>Внедрение через конструктор

Встроенная поддержка внедрения зависимостей на основе конструктора в ASP.NET Core распространяется на контроллеры MVC. Просто добавляя тип службы в контроллер в качестве параметра конструктора, ASP.NET Core будет пытаться разрешить этот тип с помощью встроенного контейнера службы. Службы обычно, хотя и не всегда, задаются с помощью интерфейсов. Например, если приложение имеет бизнес-логику, зависящую от текущего времени, можно внедрить службу, которая возвращает время (вместо жесткого его задания), что позволяет тестам доходить до реализаций, использующих заданное время.

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


Реализация такого интерфейса, использующего системные часы во время выполнения, не представляет сложностей:

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


После этого мы можем использовать службу в своем контроллере. В этом случае мы добавили некоторую логику в метод `HomeController` `Index` для отображения приветствия пользователю с учетом времени суток.

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

Если запустить приложение сейчас, скорее всего, возникнет ошибка:

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

Эта ошибка возникает, если мы не настроили службу в методе `ConfigureServices` своего класса `Startup`. Чтобы указать, что запросы для `IDateTime` должны разрешаться с использованием экземпляра `SystemDateTime`, добавьте выделенную ниже строку в свой метод `ConfigureServices`:

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> Эту конкретную службу можно реализовать с помощью любого из нескольких разных параметров времени существования (`Transient`, `Scoped` или `Singleton`). Раздел [Внедрение зависимостей](../../fundamentals/dependency-injection.md) описывает, как каждый из этих параметров области влияет на поведение службы.

После настройки службы при запуске приложения и переходе на домашнюю страницу должно отображаться ожидаемое сообщение с учетом времени суток:

![Приветствие сервера](dependency-injection/_static/server-greeting.png)

>[!TIP]
> См. раздел [Тестирование логики контроллера](testing.md), чтобы узнать, как явный запрос зависимостей [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) в контроллерах упрощает тестирование кода.

Встроенная функция внедрения зависимостей ASP.NET Core поддерживает наличие лишь одного конструктора для классов, запрашивающих службы. При наличии более одного конструктора может возникнуть следующее исключение:

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

Как сказано в сообщении об ошибке, вы можете устранить эту проблему, используя один конструктор. Вы также можете [заменить поддержку внедрения зависимостей по умолчанию на стороннюю реализацию](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), многие из которых поддерживают несколько конструкторов.

## <a name="action-injection-with-fromservices"></a>Внедрение действий с помощью FromServices

Иногда служба требуется всего для одного действия в контроллере. В этом случае имеет смысл внедрить службу в качестве параметра метода действия. Это можно сделать, пометив параметр с помощью атрибута `[FromServices]`, как показано ниже:

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a>Доступ к параметрам из контроллера

Доступ к параметрам приложения или конфигурации из контроллера относится к типичным шаблонам. Для такого доступа следует использовать шаблон параметров, описанный в разделе [Конфигурация](xref:fundamentals/configuration/index). В общем случае не следует запрашивать параметры непосредственно из своего контроллера с помощью внедрения зависимостей. Оптимальнее запросить экземпляр `IOptions<T>`, где `T` является нужным вам классом конфигурации.

Для работы с шаблоном параметров нужно создать класс, представляющий эти параметры, например следующий:

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

Затем вам нужно настроить приложение для использования этой модели параметров и добавить класс конфигурации в коллекцию служб в `ConfigureServices`:

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> В приведенном выше коде мы настраиваем приложение для считывания параметров из файла формата JSON. Вы также можете полностью настроить параметры в коде, как показано выше. Сведения о других параметрах конфигурации см. в разделе [Конфигурация](xref:fundamentals/configuration/index).

После указания строго типизированного объекта конфигурации (в данном случае `SampleWebSettings`) и добавления его в коллекцию служб вы можете запросить его из любого контроллера или метода действия, запросив экземпляр `IOptions<T>` (в данном случае `IOptions<SampleWebSettings>`). Следующий код показывает, как запросить параметры из контроллера:

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

Соблюдение шаблона параметров позволяет отделить параметры и конфигурацию друг от друга и обеспечивает [разделение функций](http://deviq.com/separation-of-concerns/) для контроллера, так как ему не нужно знать, где и как искать сведения о параметрах. Кроме того, это упрощает модульное тестирование контроллера ([Тестирование логики контроллера](testing.md)), так как отсутствует [статическое слипание](http://deviq.com/static-cling/) или прямое создание экземпляров для классов параметров в классе контроллера.
