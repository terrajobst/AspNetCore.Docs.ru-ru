---
title: Части приложения в ASP.NET Core
author: rick-anderson
description: Совместное использование контроллеров, представлений, Razor Pages и других ресурсов с помощью частей приложений в ASP.NET Core
ms.author: riande
ms.date: 11/10/2019
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 92c408adfad8af3dfd2572b625ae28ef24f64948
ms.sourcegitcommit: a7bbe3890befead19440075b05b9674351f98872
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2019
ms.locfileid: "73905763"
---
# <a name="share-controllers-views-razor-pages-and-more-with-application-parts-in-aspnet-core"></a>Совместное использование контроллеров, представлений, Razor Pages и других ресурсов с помощью частей приложений в ASP.NET Core

::: moniker range=">= aspnetcore-3.0"

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([как скачивать](xref:index#how-to-download-a-sample))

*Часть приложения* — это абстракция для ресурсов приложения. Части приложений позволяют ASP.NET Core обнаруживать контроллеры, компоненты представлений, вспомогательные функции тегов, Razor Pages, источники компиляции Razor и другие ресурсы. [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) — это часть приложения. `AssemblyPart` инкапсулирует ссылку на сборку и предоставляет типы и ссылки на компиляцию.

*Поставщики компонентов* работают с частями приложения для заполнения компонентов приложения ASP.NET Core. Основной вариант использования частей приложения заключается в настройке приложения для обнаружения (или запрета загрузки) компонентов ASP.NET Core из сборки. Например, может потребоваться совместное использование общих функциональных возможностей несколькими приложениями. С помощью частей приложения можно совместно использовать сборку (библиотека DLL), содержащую контроллеры, представления, Razor Pages, источники компиляции Razor, вспомогательные функции тегов и другие ресурсы, с несколькими приложениями. Желательно предоставить общий доступ к сборке для дублирования кода в нескольких проектах.

Приложения ASP.NET Core загружают компоненты из <xref:System.Web.WebPages.ApplicationPart>. Класс <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> представляет часть приложения, поддерживаемую сборкой.

## <a name="load-aspnet-core-features"></a>Загрузка компонентов ASP.NET Core

Используйте классы `ApplicationPart` и `AssemblyPart` для обнаружения и загрузки компонентов ASP.NET Core (контроллеры, компоненты представления и т. д.). <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> отслеживает доступные части приложения и поставщики компонентов. Функция `ApplicationPartManager` настраивается в `Startup.ConfigureServices`.

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

Следующий код предоставляет альтернативный подход к настройке `ApplicationPartManager` с использованием `AssemblyPart`:

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

Предыдущие два примера кода загружают `SharedController` из сборки. `SharedController` не находится в проекте приложения. См. пример [решения WebAppParts](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) для загрузки.

### <a name="include-views"></a>Включение представлений

Чтобы включить представления в сборку, выполните следующие действия.

* Добавьте следующую разметку в файл общего проекта:

  ```csproj
  <ItemGroup>
      <EmbeddedResource Include="Views\**\*.cshtml" />
  </ItemGroup>
  ```

* Добавьте <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> в <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:

  [!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

### <a name="prevent-loading-resources"></a>Запрет загрузки ресурсов

Части приложения можно использовать для *запрета* загрузки ресурсов в определенной сборке или расположении. Добавьте или удалите элементы коллекции <xref:Microsoft.AspNetCore.Mvc.ApplicationParts>, чтобы скрыть ресурсы или сделать их доступными. Порядок записей в коллекции `ApplicationParts` не имеет значения. Настройте класс `ApplicationPartManager` перед его использованием для конфигурации служб в контейнере. Например, настройте класс `ApplicationPartManager` перед вызовом метода `AddControllersAsServices`. Вызовите метод `Remove` в коллекции `ApplicationParts`, чтобы удалить ресурс.

В следующем коде для удаления `MyDependentLibrary` из приложения используется <xref:Microsoft.AspNetCore.Mvc.ApplicationParts>: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]

`ApplicationPartManager` содержит части для:

* сборки приложения и зависимых сборок;
* `Microsoft.AspNetCore.Mvc.TagHelpers`.
* `Microsoft.AspNetCore.Mvc.Razor`.

## <a name="application-feature-providers"></a>Поставщики компонентов приложений

Поставщики компонентов приложений проверяют части приложения и предоставляют для них компоненты. Доступны встроенные поставщики компонентов для следующих компонентов ASP.NET Core:

* [Контроллеры](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [Вспомогательные функции тегов](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [Компоненты представлений](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

Поставщики компонентов наследуют от <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, где `T` является типом компонента. Поставщики компонентов можно реализовать для любого из перечисленных выше типов компонентов. Порядок поставщиков компонентов в `ApplicationPartManager.FeatureProviders` может повлиять на поведение во время выполнения. Поставщики, добавленные позднее, могут реагировать на действия, выполняемые ранее добавленными поставщиками.

### <a name="generic-controller-feature"></a>компонент универсального контроллера

ASP.NET Core игнорирует [универсальные контроллеры](/dotnet/csharp/programming-guide/generics/generic-classes). Универсальный контроллер содержит параметр типа (например, `MyController<T>`). В следующем примере добавляются экземпляры универсального контроллера для указанного списка типов:

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

Типы определены в файле `EntityTypes.Types`.

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

Поставщик компонентов добавляется в `Startup`:

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

Имена универсальных контроллеров, используемых для маршрутизации, будут иметь вид *GenericController`1[Widget]* вместо *Widget*. Для изменения имени в соответствии с универсальным типом, используемым контроллером, применяется следующий атрибут:

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

Класс `GenericController`:

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

Например, запрос URL-адреса `https://localhost:5001/Sprocket` приводит к следующему ответу:

```text
Hello from a generic Sprocket controller.
```

### <a name="display-available-features"></a>отображение доступных компонентов

Компоненты, доступные для приложения, можно перечислить путем запроса `ApplicationPartManager` с помощью [внедрения зависимостей](../../fundamentals/dependency-injection.md):

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

В [примере для скачивания](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) используется приведенный выше код для отображения компонентов приложения:

```text
Controllers:
    - FeaturesController
    - HomeController
    - HelloController
    - GenericController`1
    - GenericController`1
Tag Helpers:
    - PrerenderTagHelper
    - AnchorTagHelper
    - CacheTagHelper
    - DistributedCacheTagHelper
    - EnvironmentTagHelper
    - Additional Tag Helpers omitted for brevity.
View Components:
    - SampleViewComponent
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([как скачивать](xref:index#how-to-download-a-sample))

*Часть приложения* — это абстракция для ресурсов приложения. Части приложений позволяют ASP.NET Core обнаруживать контроллеры, компоненты представлений, вспомогательные функции тегов, Razor Pages, источники компиляции Razor и другие ресурсы. [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) — это часть приложения. `AssemblyPart` инкапсулирует ссылку на сборку и предоставляет типы и ссылки на компиляцию.

*Поставщики компонентов* работают с частями приложения для заполнения компонентов приложения ASP.NET Core. Основной вариант использования частей приложения заключается в настройке приложения для обнаружения (или запрета загрузки) компонентов ASP.NET Core из сборки. Например, может потребоваться совместное использование общих функциональных возможностей несколькими приложениями. С помощью частей приложения можно совместно использовать сборку (библиотека DLL), содержащую контроллеры, представления, Razor Pages, источники компиляции Razor, вспомогательные функции тегов и другие ресурсы, с несколькими приложениями. Желательно предоставить общий доступ к сборке для дублирования кода в нескольких проектах.

Приложения ASP.NET Core загружают компоненты из <xref:System.Web.WebPages.ApplicationPart>. Класс <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> представляет часть приложения, поддерживаемую сборкой.

## <a name="load-aspnet-core-features"></a>Загрузка компонентов ASP.NET Core

Используйте классы `ApplicationPart` и `AssemblyPart` для обнаружения и загрузки компонентов ASP.NET Core (контроллеры, компоненты представления и т. д.). <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> отслеживает доступные части приложения и поставщики компонентов. Функция `ApplicationPartManager` настраивается в `Startup.ConfigureServices`.

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

Следующий код предоставляет альтернативный подход к настройке `ApplicationPartManager` с использованием `AssemblyPart`:

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

Предыдущие два примера кода загружают `SharedController` из сборки. `SharedController` не находится в проекте приложения. См. пример [решения WebAppParts](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) для загрузки.

### <a name="include-views"></a>Включение представлений

Чтобы включить представления в сборку, выполните следующие действия.

* Добавьте следующую разметку в файл общего проекта:

  ```csproj
  <ItemGroup>
      <EmbeddedResource Include="Views\**\*.cshtml" />
  </ItemGroup>
  ```

* Добавьте <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> в <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:

  [!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

### <a name="prevent-loading-resources"></a>Запрет загрузки ресурсов

Части приложения можно использовать для *запрета* загрузки ресурсов в определенной сборке или расположении. Добавьте или удалите элементы коллекции <xref:Microsoft.AspNetCore.Mvc.ApplicationParts>, чтобы скрыть ресурсы или сделать их доступными. Порядок записей в коллекции `ApplicationParts` не имеет значения. Настройте класс `ApplicationPartManager` перед его использованием для конфигурации служб в контейнере. Например, настройте класс `ApplicationPartManager` перед вызовом метода `AddControllersAsServices`. Вызовите метод `Remove` в коллекции `ApplicationParts`, чтобы удалить ресурс.

В следующем коде для удаления `MyDependentLibrary` из приложения используется <xref:Microsoft.AspNetCore.Mvc.ApplicationParts>: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]

`ApplicationPartManager` содержит части для:

* сборки приложения и зависимых сборок;
* `Microsoft.AspNetCore.Mvc.TagHelpers`.
* `Microsoft.AspNetCore.Mvc.Razor`.

## <a name="application-feature-providers"></a>Поставщики компонентов приложений

Поставщики компонентов приложений проверяют части приложения и предоставляют для них компоненты. Доступны встроенные поставщики компонентов для следующих компонентов ASP.NET Core:

* [Контроллеры](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [Вспомогательные функции тегов](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [Компоненты представлений](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

Поставщики компонентов наследуют от <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, где `T` является типом компонента. Поставщики компонентов можно реализовать для любого из перечисленных выше типов компонентов. Порядок поставщиков компонентов в `ApplicationPartManager.FeatureProviders` может повлиять на поведение во время выполнения. Поставщики, добавленные позднее, могут реагировать на действия, выполняемые ранее добавленными поставщиками.

### <a name="generic-controller-feature"></a>компонент универсального контроллера

ASP.NET Core игнорирует [универсальные контроллеры](/dotnet/csharp/programming-guide/generics/generic-classes). Универсальный контроллер содержит параметр типа (например, `MyController<T>`). В следующем примере добавляются экземпляры универсального контроллера для указанного списка типов:

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

Типы определены в файле `EntityTypes.Types`.

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

Поставщик компонентов добавляется в `Startup`:

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

Имена универсальных контроллеров, используемых для маршрутизации, будут иметь вид *GenericController`1[Widget]* вместо *Widget*. Для изменения имени в соответствии с универсальным типом, используемым контроллером, применяется следующий атрибут:

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

Класс `GenericController`:

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

Например, запрос URL-адреса `https://localhost:5001/Sprocket` приводит к следующему ответу:

```text
Hello from a generic Sprocket controller.
```

### <a name="display-available-features"></a>отображение доступных компонентов

Компоненты, доступные для приложения, можно перечислить путем запроса `ApplicationPartManager` с помощью [внедрения зависимостей](../../fundamentals/dependency-injection.md):

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

В [примере для скачивания](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) используется приведенный выше код для отображения компонентов приложения:

```text
Controllers:
    - FeaturesController
    - HomeController
    - HelloController
    - GenericController`1
    - GenericController`1
Tag Helpers:
    - PrerenderTagHelper
    - AnchorTagHelper
    - CacheTagHelper
    - DistributedCacheTagHelper
    - EnvironmentTagHelper
    - Additional Tag Helpers omitted for brevity.
View Components:
    - SampleViewComponent
```

::: moniker-end
