---
title: Пакет SDK для Razor в ASP.NET Core
author: Rick-Anderson
description: Сведения о применении в ASP.NET Core функции Razor Pages, которая делает создание кодов сценариев для страниц проще и эффективнее по сравнению с MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 08/23/2019
no-loc:
- Blazor
uid: razor-pages/sdk
ms.openlocfilehash: 2fbdf95d02d7918236981c7fee8ebcbedf5c55e1
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963263"
---
# <a name="aspnet-core-razor-sdk"></a>Пакет SDK для Razor в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)

## <a name="overview"></a>Обзор

[!INCLUDE[](~/includes/2.1-SDK.md)] включает пакет SDK для `Microsoft.NET.Sdk.Razor` MSBuild (пакет SDK для Razor). Пакет SDK для Razor:

::: moniker range=">= aspnetcore-3.0"

* Требуется для сборки, упаковки и публикации проектов, содержащих файлы [Razor](xref:mvc/views/razor) , для ASP.NET Core проектов на основе MVC или [Blazor](xref:blazor/index) .
* Включает набор предопределенных целевых объектов, свойств и элементов, позволяющих настраивать компиляцию файлов Razor ( *. cshtml* или *. Razor*).

Пакет SDK для Razor включает `Content` элементы с `Include`ными атрибутами, заданными `**\*.cshtml` и `**\*.razor` шаблонами глобализации. Публикуются соответствующие файлы.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* Стандартизирует процесс создания, упаковки и публикации проектов, содержащих файлы [Razor](xref:mvc/views/razor), для проектов на основе MVC ASP.NET.
* Включает набор предопределенных целевых объектов, свойств и элементов, которые позволяют настраивать параметры компиляции файлов Razor.

Пакет SDK для Razor включает элемент `Content` с атрибутом `Include`, установленным в шаблон глобализации `**\*.cshtml`. Публикуются соответствующие файлы.

::: moniker-end

## <a name="prerequisites"></a>Необходимые компоненты

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="use-the-razor-sdk"></a>Использование пакета SDK для Razor

Большинству веб-приложений не требуется явная ссылка на пакет SDK для Razor.

::: moniker range=">= aspnetcore-3.0"

Чтобы использовать пакет SDK для Razor для построения библиотек классов, содержащих представления Razor или Razor Pages, мы рекомендуем начать с шаблона проекта РКЛ для библиотеки классов Razor. Для РКЛ, который используется для создания файлов Blazor ( *. Razor*), минимально требуется ссылка на пакет [Microsoft. AspNetCore. Components](https://www.nuget.org/packages/Microsoft.AspNetCore.Components) . РКЛ, который используется для построения представлений Razor или страниц (*CSHTML* -файлов), минимально требует целевых объектов `netcoreapp3.0` или более поздней версии и имеет `FrameworkReference` в файле проекта [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app) .

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Использование пакета SDK для Razor для создания библиотеки классов с представлениями Razor или страницами Razor:

* Используйте `Microsoft.NET.Sdk.Razor` вместо `Microsoft.NET.Sdk`:

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    <!-- omitted for brevity -->
  </Project>
  ```

* Как правило, ссылка на пакет `Microsoft.AspNetCore.Mvc` необходима для получения дополнительных зависимостей, необходимых для построения и компиляции Razor Pages и представлений Razor. Проект должен добавлять ссылки на пакеты в:

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`
    
  Пакет `Microsoft.AspNetCore.Razor.Design` предоставляет задачи компиляции Razor и целевые объекты для проекта.

  Предыдущие пакеты включены в `Microsoft.AspNetCore.Mvc`. В следующей разметке показан файл проекта, который использует пакет SDK для Razor для создания файлов Razor для приложения ASP.NET Core Razor Pages.
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]
  
::: moniker-end

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> Пакеты `Microsoft.AspNetCore.Razor.Design` и `Microsoft.AspNetCore.Mvc.Razor.Extensions` включены в [метапакет Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app). Однако ссылка на пакет без версии `Microsoft.AspNetCore.App` предоставляет метапакет приложению, которое не включает последнюю версию `Microsoft.AspNetCore.Razor.Design`. Проекты должны ссылаться на последовательную версию `Microsoft.AspNetCore.Razor.Design` (или `Microsoft.AspNetCore.Mvc`), чтобы были включены последние исправления времени сборки для Razor. Дополнительные сведения см. в [этой статье об ошибке на GitHub](https://github.com/aspnet/Razor/issues/2553).

::: moniker-end

### <a name="properties"></a>Свойства

Следующие свойства управляют поведением пакета SDK для Razor в ходе создания проекта:

* `RazorCompileOnBuild` &ndash; при `true` компилирует и выдает сборку Razor в ходе сборки проекта. По умолчанию — `true`.
* `RazorCompileOnPublish` &ndash; при `true` компилирует и выдает сборку Razor в процессе публикации проекта. По умолчанию — `true`.

Свойства и элементы в следующей таблице используются для настройки входных и выходных данных в пакете SDK для Razor.

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> Начиная с ASP.NET Core 3,0, представления MVC или Razor Pages не обслуживаются по умолчанию, если свойства MSBuild `RazorCompileOnBuild` или `RazorCompileOnPublish` в файле проекта отключены. Приложения должны добавить явную ссылку на пакет [Microsoft. AspNetCore. MVC. Razor. рунтимекомпилатион](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation) , если приложение использует компиляцию с помощью среды выполнения для обработки *CSHTML* файлов.

::: moniker-end

| Элементы | Описание |
| ----- | ----------- |
| `RazorGenerate` | Элементы Item (*CSHTML* -файлы), которые являются входными данными для создания кода. |
| `RazorComponent` | Элементы элементов (*Razor* -файлы), которые являются входными данными для создания кода компонента Razor. |
| `RazorCompile` | Элементы Item (*CS* -файлы), входные в целевые объекты компиляции Razor. Используйте этот `ItemGroup`, чтобы указать дополнительные файлы для компиляции в сборку Razor. |
| `RazorTargetAssemblyAttribute` | Элементы, используемые для создания атрибутов для сборки Razor. Пример:  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | Элементы Item добавляются как внедренные ресурсы в созданную сборку Razor. |

| свойство; | Описание |
| -------- | ----------- |
| `RazorTargetName` | Имя файла (без расширения) для сборки, созданной Razor. | 
| `RazorOutputPath` | Выходной каталог Razor. |
| `RazorCompileToolset` | Используется для определения набора инструментов для построения сборки Razor. Допустимые значения: `Implicit`, `RazorSDK` и `PrecompilationTool`. |
| [енабледефаултконтентитемс](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | Значение по умолчанию — `true`. При `true` включает файлы *Web. config*, *. JSON*и *. cshtml* в качестве содержимого в проекте. При ссылке через `Microsoft.NET.Sdk.Web` файлы также включаются в файлы *wwwroot* и config. |
| `EnableDefaultRazorGenerateItems` | При значении `true` включает файлы *.cshtml* из элементов `Content` в элементы `RazorGenerate`. |
| `GenerateRazorTargetAssemblyInfo` | При `true` создает файл *. CS* , содержащий атрибуты, заданные `RazorAssemblyAttribute`, и включает файл в выходные данные компиляции. |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | При значении `true` добавляет стандартный набор атрибутов сборки в `RazorAssemblyAttribute`. |
| `CopyRazorGenerateFilesToPublishDirectory` | Если `true`, копирует файлы `RazorGenerate` ( *. cshtml*) в каталог публикации. Как правило, файлы Razor не требуются для опубликованного приложения, если они участвуют в компиляции во время сборки или во время публикации. По умолчанию — `false`. |
| `CopyRefAssembliesToPublishDirectory` | При значении `true` копирует элементы базовой сборки в каталог публикации. Как правило, ссылочные сборки не требуются для опубликованного приложения, если компиляция Razor происходит во время сборки или во время публикации. Задайте значение `true`, если для опубликованного приложения требуется компиляция среды выполнения. Например, установите значение `true`, если приложение изменяет файлы *. cshtml* во время выполнения или использует внедренные представления. По умолчанию — `false`. |
| `IncludeRazorContentInPack` | Если `true`, все элементы содержимого Razor (*CSHTML* -файлы) помечены для включения в созданный пакет NuGet. По умолчанию — `false`. |
| `EmbedRazorGenerateSources` | При значении `true` добавляет элементы RazorGenerate ( *.cshtml*) в виде внедренных файлов к создаваемой сборке Razor. По умолчанию — `false`. |
| `UseRazorBuildServer` | При значении `true` использует серверный процесс постоянной сборки для разгрузки работы по созданию кода. По умолчанию используется значение `UseSharedCompilation`. |
| `GenerateMvcApplicationPartsAssemblyAttributes` | Если `true`, пакет SDK создает дополнительные атрибуты, используемые MVC во время выполнения для выполнения обнаружения частей приложения. |

Дополнительные сведения о свойствах см. в статье [MSBuild Properties](/visualstudio/msbuild/msbuild-properties) (Свойства MSBuild).

### <a name="targets"></a>Целевые объекты

Пакет SDK для Razor определяет два основных целевых объекта:

* `RazorGenerate` &ndash; код создает *CS* -файлы из элементов `RazorGenerate`. Используйте свойство `RazorGenerateDependsOn`, чтобы указать дополнительные целевые объекты, которые могут выполняться до или после этого целевого объекта.
* `RazorCompile` &ndash; компилирует созданные файлы *. CS* в сборку Razor. Используйте `RazorCompileDependsOn`, чтобы указать дополнительные целевые объекты, которые могут выполняться до или после этого целевого объекта.
* `RazorComponentGenerate` &ndash; код создает файлы *. CS* для элементов `RazorComponent`. Используйте свойство `RazorComponentGenerateDependsOn`, чтобы указать дополнительные целевые объекты, которые могут выполняться до или после этого целевого объекта.

### <a name="runtime-compilation-of-razor-views"></a>Компиляция среды выполнения представлений Razor

* По умолчанию пакет SDK для Razor не публикует базовые сборки, необходимые для компиляции среды выполнения. Это приведет к сбою компиляции, если модель приложения зависит от компиляции среды выполнения &mdash; например, приложение использует внедренные представления или меняет представления после публикации. Установите для `CopyRefAssembliesToPublishDirectory` значение `true`, чтобы продолжить публикацию базовых сборок.

* Для веб-приложения убедитесь, что приложение предназначено для пакета SDK `Microsoft.NET.Sdk.Web`.

## <a name="razor-language-version"></a>Версия языка Razor

При использовании пакета SDK `Microsoft.NET.Sdk.Web` версия языка Razor выводится из целевой версии платформы приложения. Для проектов, предназначенных для пакета SDK `Microsoft.NET.Sdk.Razor`, или в редких случаях, когда приложению требуется версия языка Razor, отличная от выводимого значения, можно настроить версию, задав свойство `<RazorLangVersion>` в файле проекта приложения:

```xml
<PropertyGroup>
  <RazorLangVersion>{VERSION}</RazorLangVersion>
</PropertyGroup>
```

Языковая версия Razor тесно интегрирована с версией среды выполнения, для которой она была создана. Нацеливание на языковую версию, не предназначенную для среды выполнения, не поддерживается и, скорее всего, приведет к ошибкам сборки.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Дополнения к формату CSPROJ для .NET Core](/dotnet/core/tools/csproj)
* [Общие элементы проектов MSBuild](/visualstudio/msbuild/common-msbuild-project-items)
