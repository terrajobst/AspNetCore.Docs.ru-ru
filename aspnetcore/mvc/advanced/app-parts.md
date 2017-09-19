---
title: "Частей приложения в ASP.NET Core"
author: ardalis
description: "Сведения об использовании частей приложения, которые являются abstrations ресурсами приложения, чтобы настроить приложение для обнаружения или избегать загрузки компонентов из сборки."
keywords: "ASP.NET Core часть приложения, часть приложения"
ms.author: riande
manager: wpickett
ms.date: 01/04/2017
ms.topic: article
ms.assetid: b355a48e-a15c-4d58-b69c-899963613a98
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 77d3a58d58493bf1b0b760ab9037d2778ba23441
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/19/2017
---
# <a name="application-parts-in-aspnet-core"></a>Частей приложения в ASP.NET Core

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample)

*Часть приложения* — это абстракция над ресурсы приложения, из которого MVC такими функциями, как контроллеров, просмотр компонентов или вспомогательных функций тегов могут быть обнаружены. Одним из примеров части приложения является AssemblyPart, который содержит ссылку на сборку и предоставляет типы и ссылки на компиляцию. *Функции поставщиков* работают с частей приложения для заполнения компонентов приложения ASP.NET Core MVC. Является позволяют настроить приложения для обнаружения (или избежать загрузки) основного варианта использования для частей приложения MVC компоненты из сборки.

## <a name="introducing-application-parts"></a>Знакомство с приложением частей приложения

Приложения MVC загрузить их компоненты из [частей приложения](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart). В частности [AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) класс представляет часть приложения, поддерживаемый сборки. Эти классы можно использовать для обнаружения и загрузки MVC функции, например контроллеров, просмотр компонентов, вспомогательных функций тегов и razor компиляции источников. [ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) отвечает за отслеживание частей приложения и функции поставщиков, доступных для приложения MVC. Вы можете взаимодействовать с `ApplicationPartManager` в `Startup` при настройке MVC:

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

По умолчанию MVC поиск в дереве зависимостей и поиск контроллеров (даже в других сборок). Попытка загрузить сборку произвольные (например, от подключаемого модуля, который не обращается во время компиляции), можно использовать часть приложения.

Можно использовать частей приложения для *избежать* поиск контроллеров в конкретной сборке или в другом расположении. Можно выбрать, какие части (или сборок) доступны для приложения, изменив `ApplicationParts` коллекцию `ApplicationPartManager`. Порядок записей в `ApplicationParts` сбора не имеет значения. Очень важно, чтобы полностью настроить `ApplicationPartManager` перед его использованием для настройки служб в контейнере. Например, необходимо полностью настроить `ApplicationPartManager` перед вызовом `AddControllersAsServices`. Несоблюдение этого, означает, что в частях приложения добавить контроллеры после вызова метода не будут затронуты (не получить зарегистрирована как службы) что может привести неправильное bevavior приложения.

Если у вас есть к сборке, содержащей контроллеры, вы не хотите использовать, удалите его из `ApplicationPartManager`:

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

Помимо сборки проекта и его зависимых сборок `ApplicationPartManager` будет включать части для `Microsoft.AspNetCore.Mvc.TagHelpers` и `Microsoft.AspNetCore.Mvc.Razor` по умолчанию.

## <a name="application-feature-providers"></a>Поставщики компонентов приложений

Поставщики функций приложения проверьте частей приложения и предоставляет функциональные возможности для этих частей. Нет встроенной функции поставщиков для следующих компонентов MVC:

* [Контроллеры](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [Ссылка на метаданные](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [Вспомогательные функции тегов](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [Просмотр компонентов](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

Наследовать поставщиков функций `IApplicationFeatureProvider<T>`, где `T` является типом функции. Вы можете реализовать собственные поставщики для любого типа в MVC функции, перечисленные выше функции. Порядок поставщиков функций в `ApplicationPartManager.FeatureProviders` коллекции может быть важно, поскольку поставщики более поздних версий можно реагировать на действия, производимые предыдущие поставщики.

### <a name="sample-generic-controller-feature"></a>Образец: Функция универсального контроллера

По умолчанию ASP.NET Core MVC игнорирует универсального контроллеров (например, `SomeController<T>`). В этом примере используется поставщик функций контроллера, который выполняется после поставщика по умолчанию и добавляет экземпляры универсального контроллера для указанного списка типов (определенные в `EntityTypes.Types`):

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

Типы сущностей:

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

Поставщик функций добавляется в `Startup`:

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p => 
        p.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

По умолчанию контроллера, универсальных имен, используемых для маршрутизации будет иметь вид *GenericController "1 [мини-приложение]* вместо *мини-приложение*. Измените имя, чтобы они соответствовали универсального типа, используемое контроллером используется следующий атрибут:

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

`GenericController` Класса:

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

Результат, когда запрашивается совпадающий маршрут:

![Считывает пример выходных данных из примера приложения «Hello из универсального контроллера Sproket».](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a>Образца: Доступные функции для отображения

Можно выполнять итерацию заполненных функции, доступные для приложения, запрашивая `ApplicationPartManager` через [внедрения зависимостей](../../fundamentals/dependency-injection.md) и использовать его для заполнения экземпляры соответствующих компонентов:

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

Пример выходных данных:

![Пример выходных данных из примера приложения](app-parts/_static/available-features.png)
