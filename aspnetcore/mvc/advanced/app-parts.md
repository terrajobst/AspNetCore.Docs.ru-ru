---
title: "Части приложения в ASP.NET Core"
author: ardalis
description: "Сведения о том, как использовать части приложения, которые являются абстракциями по отношению к ресурсам приложения, для настройки приложения в целях обнаружения или запрета загрузки компонентов из сборки."
manager: wpickett
ms.author: riande
ms.date: 01/04/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 6b855f8725dacc89a7e0607224ef3c19ab9f5676
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="application-parts-in-aspnet-core"></a>Части приложения в ASP.NET Core

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

*Часть приложения* — это абстракция по отношению к ресурсам приложения, из которой могут быть обнаружены такие элементы MVC, как контроллеры, компоненты представлений или вспомогательные функции тегов. Одним из примеров части приложения является AssemblyPart, который содержит ссылку на сборку и предоставляет типы и ссылки на компиляцию. *Поставщики компонентов* работают с частями приложения для заполнения компонентов приложения ASP.NET Core MVC. Основной вариант использования частей приложения заключается в настройке приложения для обнаружения (или запрета загрузки) компонентов MVC из сборки.

## <a name="introducing-application-parts"></a>Общие сведения о частях приложения

Приложения MVC загружают свои компоненты из [частей приложения](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart). В частности, класс [AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) представляет часть приложения, поддерживаемую сборкой. Эти классы можно использовать для обнаружения и загрузки компонентов MVC, таких как контроллеры, компоненты представлений, вспомогательные функции тегов и источники компиляции Razor. [ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) отвечает за отслеживание частей приложения и поставщиков компонентов, доступных для приложения MVC. При настройке MVC можно взаимодействовать с классом `ApplicationPartManager` в `Startup`:

```csharp
// create an assembly part from a class's assembly
var assembly = typeof(Startup).GetTypeInfo().Assembly;
services.AddMvc()
    .AddApplicationPart(assembly);

// OR
var assembly = typeof(Startup).GetTypeInfo().Assembly;
var part = new AssemblyPart(assembly);
services.AddMvc()
    .ConfigureApplicationPartManager(apm => p.ApplicationParts.Add(part));
```

По умолчанию MVC выполняет поиск в дереве зависимостей и находит контроллеры (даже в других сборках). Часть приложения можно использовать для загрузки произвольной сборки (например, из подключаемого модуля, который не указывается во время компиляции).

С помощью частей приложения можно *запретить* поиск контроллеров в конкретной сборке или расположении. Чтобы контролировать части (или сборки), доступные для приложения, следует изменить коллекцию `ApplicationParts` класса `ApplicationPartManager`. Порядок записей в коллекции `ApplicationParts` не имеет значения. Очень важно полностью настроить класс `ApplicationPartManager` перед его использованием для конфигурации служб в контейнере. Например, необходимо полностью настроить класс `ApplicationPartManager` перед вызовом метода `AddControllersAsServices`. В противном случае контроллеры в частях приложения, добавленные после вызова этого метода, не будут затронуты (не будут зарегистрированы как службы), что может привести к неправильной работе приложения.

Если у вас есть сборка, содержащая контроллеры, которые не требуется использовать, удалите ее из `ApplicationPartManager`:

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p =>
    {
        var dependentLibrary = p.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           p.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

Помимо сборки проекта и зависимых сборок, `ApplicationPartManager` будет содержать части для `Microsoft.AspNetCore.Mvc.TagHelpers` и `Microsoft.AspNetCore.Mvc.Razor` по умолчанию.

## <a name="application-feature-providers"></a>Поставщики компонентов приложений

Поставщики компонентов приложений проверяют части приложения и предоставляют для них компоненты. Доступны встроенные поставщики компонентов для следующих компонентов MVC:

* [Контроллеры](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [Ссылка на метаданные](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [Вспомогательные функции тегов](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [Компоненты представлений](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

Поставщики компонентов наследуют от `IApplicationFeatureProvider<T>`, где `T` является типом компонента. Для всех перечисленных выше типов компонентов MVC можно реализовать собственные поставщики. Порядок поставщиков компонентов в коллекции `ApplicationPartManager.FeatureProviders` имеет важное значение, поскольку поставщики более поздних версий способны реагировать на действия поставщиков предыдущих версий.

### <a name="sample-generic-controller-feature"></a>Пример: компонент универсального контроллера

По умолчанию ASP.NET Core MVC игнорирует универсальные контроллеры (например, `SomeController<T>`). В этом примере используется поставщик компонентов контроллера, который запускается после поставщика по умолчанию и добавляет экземпляры универсального контроллера для указанного списка типов (определенного в `EntityTypes.Types`):

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

Типы сущностей:

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

Поставщик компонентов добавляется в `Startup`:

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p => 
        p.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

По умолчанию имена универсальных контроллеров, используемых для маршрутизации, будут иметь вид *GenericController`1[Widget]* вместо *Widget*. Для изменения имени в соответствии с универсальным типом, используемым контроллером, применяется следующий атрибут:

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

Класс `GenericController`:

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

При запросе совпадающего маршрута выводится следующий результат:

![Пример выходных данных из примера приложения: "Hello from a generic Sproket controller".](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a>Пример: отображение доступных компонентов

Для итерации по заполненным компонентам, доступным для приложения, можно запросить класс `ApplicationPartManager` через [внедрение зависимостей](../../fundamentals/dependency-injection.md) и использовать его для заполнения экземпляров соответствующих компонентов:

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

Пример выходных данных:

![Пример выходных данных из примера приложения](app-parts/_static/available-features.png)
