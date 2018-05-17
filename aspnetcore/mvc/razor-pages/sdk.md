---
title: Пакет SDK для Razor в ASP.NET Core
author: Rick-Anderson
description: Сведения о применении в ASP.NET Core функции Razor Pages, которая делает создание кодов сценариев для страниц проще и эффективнее по сравнению с MVC.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/12/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: mvc/razor-pages/sdk
ms.openlocfilehash: 2cbebb12ccd1098e1950aa7eeb22fab4ffc689e6
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2018
---
# <a name="aspnet-core-razor-sdk"></a>Пакет SDK для Razor в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

[!INCLUDE[](~/includes/2.1.md)]

[!INCLUDE[](~/includes/2.1-SDK.md)] включает пакет SDK MSBuild `Microsoft.NET.Sdk.Razor` (пакет SDK Razor). Пакет SDK для Razor:

* Стандартизирует процесс создания, упаковки и публикации проектов, содержащих файлы [Razor](xref:mvc/views/razor), для проектов на основе MVC ASP.NET.
* Включает набор предопределенных целевых объектов, свойств и элементов, которые позволяют настраивать параметры компиляции файлов Razor.

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="using-the-razor-sdk"></a>Использование пакета SDK для Razor

Большинству веб-приложений нет необходимости явно ссылаться на пакет SDK для Razor. 

Использование пакета SDK для Razor для создания библиотеки классов с представлениями Razor или страницами Razor:

* Используйте `Microsoft.NET.Sdk.Razor` вместо `Microsoft.NET.Sdk`:
```xml
<Project SDK="Microsoft.NET.Sdk.Razor">
  ...
</Project>
```

* Обычно требуется ссылка на пакет `Microsoft.AspNetCore.Mvc` для получения дополнительных зависимостей, необходимых для построения и компиляции страниц Razor и представлений Razor. Как минимум, в проекте нужны ссылки на следующие пакеты:

    * `Microsoft.AspNetCore.Razor.Design` 
    * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
    
 Предыдущие пакеты включены в `Microsoft.AspNetCore.Mvc`. В приведенном ниже коде демонстрируется базовый файл *.csproj*, который использует пакет SDK для Razor, чтобы создавать файлы Razor для приложения Razor Pages ASP.NET Core:
    
 [!code-xml[Main](sdk/sample/RazorSDK.csproj)]

### <a name="properties"></a>Свойства

Следующие свойства управляют поведением пакета SDK для Razor в ходе создания проекта:

* `RazorCompileOnBuild`. Если имеет значение `true`, компилирует и создает сборку Razor при создании проекта. По умолчанию — `true`.
* `RazorCompileOnPublish`. Если имеет значение `true`, компилирует и создает сборку Razor при публикации проекта. По умолчанию — `true`.

Следующие свойства и элементы используются для настройки входных и выходных данных в пакете SDK для Razor:

| Элементы                                         | Описание:                                                                   |
| ------------                                  | -------------                                                                 |
| RazorGenerate                                 | Элементы (файлы *.cshtml*), которые являются входными данными для целей создания кода. |
| RazorCompile                                  | Элементы (CS-файлы), которые являются входными данными для целей компиляции Razor. Используйте этот ItemGroup, чтобы указать дополнительные файлы для компиляции в сборку Razor. |
| RazorAssemblyAttribute                        | Элементы, используемые для создания атрибутов для сборки Razor. Пример:  <br />`<RazorAssemblyAttribute ` <br />  `Include="System.Reflection.AssemblyMetadataAttribute"`<br />`  _Parameter1="BuildSource" _Parameter2="https://docs.asp.net/">` |
| RazorEmbeddedResource                         | Элементы, добавленные в качестве внедренных ресурсов к созданной сборке Razor |

| Свойство.                                      | Описание:                                                                   |
| ------------                                  | -------------                                                                 |
| RazorTargetName                               | Имя файла (без расширения) для сборки, созданной Razor. | 
| RazorOutputPath                               | Выходной каталог Razor.                                      |
| RazorCompileToolset                           | Используется для определения набора инструментов для построения сборки Razor. Допустимые значения: `Implicit` и `PrecompilationTool`. |
| EnableDefaultContentItems                     | При значении `true` включает определенные типы файлов, например файлы *.cshtml*, в качестве содержимого в проекте. При ссылке через Microsoft.NET.Sdk.Web также включает все файлы в *wwwroot* и файлы конфигурации.         |
| EnableDefaultRazorGenerateItems               | При значении `true` включает файлы *.cshtml* из элементов `Content` в элементы `RazorGenerate`. |
| GenerateRazorTargetAssemblyInfo               | При значении `true` создает файл *.cs* с атрибутами, заданными `RazorAssemblyAttribute`, и включает его в выходные данные компиляции. |
| EnableDefaultRazorTargetAssemblyInfoAttributes | При значении `true` добавляет стандартный набор атрибутов сборки в `RazorAssemblyAttribute`. |
| CopyRazorGenerateFilesToPublishDirectory       | При значении `true` копирует элементы RazorGenerate (файлы *.cshtml*) в каталог публикации. Обычно опубликованному приложению не нужны файлы Razor, если они участвуют в компиляции во время сборки или публикации. По умолчанию — `false`. |
| CopyRefAssembliesToPublishDirectory            | При значении `true` копирует элементы базовой сборки в каталог публикации. Обычно опубликованному приложению не нужны базовые сборки, если компиляция Razor происходит во время сборки или публикации. Установите значение `true`, если опубликованным приложениям требуется компиляция среды выполнения, например, в среде выполнения изменяются cshtml-файлы или используются встроенные представления. По умолчанию — `false`. |
| IncludeRazorContentInPack                      | При значении `true` все элементы содержимого Razor (файлы *.cshtml*) будут помечены для включения в создаваемый пакет NuGet. По умолчанию — `false`. |
| EmbedRazorGenerateSources | При значении `true` добавляет элементы RazorGenerate (*.cshtml*) в виде внедренных файлов к создаваемой сборке Razor. По умолчанию — `false`. |
| UseRazorBuildServer                           | При значении `true` использует серверный процесс постоянной сборки для разгрузки работы по созданию кода. По умолчанию используется значение `UseSharedCompilation`. |

### <a name="targets"></a>Целевые объекты
Пакет SDK для Razor определяет два основных целевых объекта:

* `RazorGenerate` создает файлы *.cs* из элементов RazorGenerate. Используйте свойство `RazorGenerateDependsOn`, чтобы указать дополнительные целевые объекты, которые могут выполняться до или после этого целевого объекта.
* `RazorCompile` компилирует созданные файлы *.cs* в сборку Razor. Используйте `RazorCompileDependsOn`, чтобы указать дополнительные целевые объекты, которые могут выполняться до или после этого целевого объекта.