---
title: Пакет SDK для Razor в ASP.NET Core
author: Rick-Anderson
description: Сведения о применении в ASP.NET Core функции Razor Pages, которая делает создание кодов сценариев для страниц проще и эффективнее по сравнению с MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/25/2018
uid: razor-pages/sdk
ms.openlocfilehash: de51c9443e639cd64c234b6975cf7252bb7a2b9a
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2019
ms.locfileid: "58751026"
---
# <a name="aspnet-core-razor-sdk"></a>Пакет SDK для Razor в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

[!INCLUDE[](~/includes/2.1-SDK.md)] Включает в себя `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK). Пакет SDK для Razor:

* Стандартизирует процесс создания, упаковки и публикации проектов, содержащих файлы [Razor](xref:mvc/views/razor), для проектов на основе MVC ASP.NET.
* Включает набор предопределенных целевых объектов, свойств и элементов, которые позволяют настраивать параметры компиляции файлов Razor.

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="using-the-razor-sdk"></a>Использование пакета SDK для Razor

Большинство веб-приложений не требуется явно ссылаться на пакет SDK Razor.

Использование пакета SDK для Razor для создания библиотеки классов с представлениями Razor или страницами Razor:

* Используйте `Microsoft.NET.Sdk.Razor` вместо `Microsoft.NET.Sdk`:

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    ...
  </Project>
  ```

* Как правило, ссылки на пакет `Microsoft.AspNetCore.Mvc` требуется для получения дополнительные зависимости, необходимые для создания и компиляции Razor Pages и представлениями Razor. Как минимум проект следует добавить ссылки на пакеты для:

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`
    
  `Microsoft.AspNetCore.Razor.Design` Пакет содержит задачи компиляции Razor и целевых объектов для проекта.

  Предыдущие пакеты включены в `Microsoft.AspNetCore.Mvc`. В следующей разметке показан файл проекта, который использует пакет SDK для Razor для создания файлов Razor для приложения ASP.NET Core Razor Pages:
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> `Microsoft.AspNetCore.Razor.Design` И `Microsoft.AspNetCore.Mvc.Razor.Extensions` пакеты входят в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Тем не менее тем меньше версии `Microsoft.AspNetCore.App` ссылки на пакет предоставляет метапакет к приложению, которое не включает последнюю версию `Microsoft.AspNetCore.Razor.Design`. Проекты должны ссылаться на согласованность версий `Microsoft.AspNetCore.Razor.Design` (или `Microsoft.AspNetCore.Mvc`), чтобы включаются последние исправления во время сборки для Razor. Дополнительные сведения см. в разделе [проблема GitHub](https://github.com/aspnet/Razor/issues/2553).

::: moniker-end

### <a name="properties"></a>Свойства

Следующие свойства управляют поведением пакета SDK для Razor в ходе создания проекта:

* `RazorCompileOnBuild` &ndash; Когда `true`, компилирует и сборка Razor как часть сборки проекта. По умолчанию — `true`.
* `RazorCompileOnPublish` &ndash; Когда `true`, компиляция и сборка Razor как часть публикации проекта. По умолчанию — `true`.

Свойства и элементы в следующей таблице используются для настройки входных и выходных данных в пакет SDK для Razor.

| Элементы | Описание |
| ----- | ----------- |
| `RazorGenerate` | Элементы (файлы *.cshtml*), которые являются входными данными для целей создания кода. |
| `RazorCompile` | Элементы Item (*.cs* файлы), являются входными значениями для целевых сред компиляции Razor. Используйте этот элемент ItemGroup, чтобы указать дополнительные файлы для компиляции в сборку Razor. |
| `RazorTargetAssemblyAttribute` | Элементы, используемые для создания атрибутов для сборки Razor. Пример:  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | Элемент элементы добавляются в виде внедренных ресурсов на созданную сборку Razor. |

| Свойство | Описание |
| -------- | ----------- |
| `RazorTargetName` | Имя файла (без расширения) для сборки, созданной Razor. | 
| `RazorOutputPath` | Выходной каталог Razor. |
| `RazorCompileToolset` | Используется для определения набора инструментов для построения сборки Razor. Допустимые значения: `Implicit`, `RazorSDK` и `PrecompilationTool`. |
| [EnableDefaultContentItems](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | Значение по умолчанию — `true`. Когда `true`, включает в себя *web.config*, *.json*, и *.cshtml* файлы содержимого в проекте. Когда я ссылаюсь через `Microsoft.NET.Sdk.Web`, файлы в разделе *wwwroot* и файлы конфигурации, также будут включены. |
| `EnableDefaultRazorGenerateItems` | При значении `true` включает файлы *.cshtml* из элементов `Content` в элементы `RazorGenerate`. |
| `GenerateRazorTargetAssemblyInfo` | Когда `true`, приводит к возникновению ошибки *.cs* файл, содержащий атрибуты, указанные фильтром `RazorAssemblyAttribute` и файл в выходных данных компиляции. |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | При значении `true` добавляет стандартный набор атрибутов сборки в `RazorAssemblyAttribute`. |
| `CopyRazorGenerateFilesToPublishDirectory` | Когда `true`, копии `RazorGenerate` элементы (*.cshtml*) файлы в каталог публикации. Как правило файлы Razor не требуются для опубликованного приложения, если они участвуют в компиляции во время сборки или во время публикации. По умолчанию — `false`. |
| `CopyRefAssembliesToPublishDirectory` | При значении `true` копирует элементы базовой сборки в каталог публикации. Как правило ссылочные сборки не являются обязательными для опубликованного приложения случае во время сборки или публикации во время компиляции Razor. Значение `true` Если опубликованного приложения требует компиляции во время выполнения. Например, значение равно `true` Если приложение изменяет *.cshtml* файлы во время выполнения или использует внедренные представления. По умолчанию — `false`. |
| `IncludeRazorContentInPack` | Когда `true`, все элементы содержимого Razor (*.cshtml* файлы) отмечены для включения в создаваемый пакет NuGet. По умолчанию — `false`. |
| `EmbedRazorGenerateSources` | При значении `true` добавляет элементы RazorGenerate (*.cshtml*) в виде внедренных файлов к создаваемой сборке Razor. По умолчанию — `false`. |
| `UseRazorBuildServer` | При значении `true` использует серверный процесс постоянной сборки для разгрузки работы по созданию кода. По умолчанию используется значение `UseSharedCompilation`. |

Дополнительные сведения о свойствах см. в статье [MSBuild Properties](/visualstudio/msbuild/msbuild-properties) (Свойства MSBuild).

### <a name="targets"></a>Целевые объекты

Пакет SDK для Razor определяет два основных целевых объекта:

* `RazorGenerate` &ndash; Код создает *.cs* файлов из `RazorGenerate` элементы item. Используйте свойство `RazorGenerateDependsOn`, чтобы указать дополнительные целевые объекты, которые могут выполняться до или после этого целевого объекта.
* `RazorCompile` &ndash; Компилирует созданный *.cs* файлы в сборку Razor. Используйте `RazorCompileDependsOn`, чтобы указать дополнительные целевые объекты, которые могут выполняться до или после этого целевого объекта.

### <a name="runtime-compilation-of-razor-views"></a>Компиляция среды выполнения представлений Razor

* По умолчанию пакет SDK для Razor не публикует базовые сборки, необходимые для компиляции среды выполнения. Это приведет к сбою компиляции, если модель приложения зависит от компиляции среды выполнения &mdash; например, приложение использует внедренные представления или меняет представления после публикации. Установите для `CopyRefAssembliesToPublishDirectory` значение `true`, чтобы продолжить публикацию базовых сборок.

* Для веб-приложения, убедитесь в приложение предназначено для `Microsoft.NET.Sdk.Web` пакета SDK.
