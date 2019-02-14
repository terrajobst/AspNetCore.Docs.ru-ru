---
title: Компиляция и предварительная компиляция файлов Razor в ASP.NET Core
author: rick-anderson
description: Узнайте о преимуществах предварительной компиляции файлов Razor и о том, как это сделать в приложении ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: c4e8f722fdf3d3f64807cc35ff9f349af7f32abd
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248190"
---
# <a name="razor-file-compilation-in-aspnet-core"></a>Компиляция файлов Razor в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

::: moniker range="= aspnetcore-1.1"

Файл Razor компилируется в среде выполнения при вызове связанного представления MVC. Публикация файла Razor во время сборки не поддерживается. При необходимости файлы Razor можно компилировать во время публикации и развертывать вместе с приложением &mdash; для этого используется средство предварительной компиляции.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Файл Razor компилируется в среде выполнения при вызове связанной страницы Razor или представления MVC. Публикация файла Razor во время сборки не поддерживается. При необходимости файлы Razor можно компилировать во время публикации и развертывать вместе с приложением &mdash; для этого используется средство предварительной компиляции.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Файл Razor компилируется в среде выполнения при вызове связанной страницы Razor или представления MVC. Файлы Razor компилируются и во время сборки, и во время публикации с помощью [пакета SDK для Razor](xref:razor-pages/sdk).

::: moniker-end

## <a name="precompilation-considerations"></a>Особенности предварительной компиляции

Ниже перечислены побочные эффекты предварительной компиляции файлов Razor:

* уменьшение размера публикуемого пакета;
* уменьшение времени запуска сервера;
* отсутствие возможности редактирования файлов Razor по причине отсутствия связанного содержимого в публикуемом пакете.

## <a name="deploy-precompiled-files"></a>Развертывание предварительно скомпилированных файлов

::: moniker range=">= aspnetcore-2.1"

Компиляция файлов Razor во время сборки и публикации включена по умолчанию с помощью пакета SDK для Razor. Редактирование файлов Razor после их обновления поддерживается во время сборки. По умолчанию только скомпилированный файл *Views.dll*, а не *CSHTML*-файлы, развертываются вместе с приложением.

> [!IMPORTANT]
> Средство предварительной компиляции будет удалено в ASP.NET Core 3.0. Мы советуем перейти на [пакет SDK для Razor](xref:razor-pages/sdk).
>
> Пакет SDK для Razor применяется только в том случае, если в файле проекта не заданы свойства предварительной компиляции. Например, если в *CSPROJ*-файле для свойства `MvcRazorCompileOnPublish` установлено значение `true`, пакет SDK для Razor будет отключен.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Если проект ориентирован на платформу .NET Framework, установите пакет NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

Если проект нацелен на .NET Core, никаких изменений не требуется.

В шаблонах проектов ASP.NET Core 2.x для свойства `MvcRazorCompileOnPublish` по умолчанию неявно задано значение `true`. Следовательно, этот элемент можно безопасно удалить из *CSPROJ*-файла.

> [!IMPORTANT]
> Средство предварительной компиляции будет удалено в ASP.NET Core 3.0. Мы советуем перейти на [пакет SDK для Razor](xref:razor-pages/sdk).
>
> Предварительная компиляция файлов Razor недоступна при выполнении [автономного развертывания](/dotnet/core/deploying/#self-contained-deployments-scd) в ASP.NET Core 2.0.

::: moniker-end

::: moniker range="= aspnetcore-1.1"

Задайте для свойства `MvcRazorCompileOnPublish` значение `true`и установите пакет NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/). Это показано в следующем примере *CSPROJ*:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

Подготовьте приложение к [развертыванию в зависимости от платформы](/dotnet/core/deploying/#framework-dependent-deployments-fdd) с помощью [команды публикации .NET Core CLI](/dotnet/core/tools/dotnet-publish). Например, выполните следующую команду в корневом элементе проекта:

```console
dotnet publish -c Release
```

В случае успешного выполнения компиляции создается файл *<имя_проекта>.PrecompiledViews.dll*, содержащий скомпилированные файлы Razor. Например, следующий снимок экрана показывает содержимое файла *Index.cshtml* внутри *WebApplication1.PrecompiledViews.dll*:

![Представления Razor внутри библиотеки DLL](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="recompile-razor-files-on-change"></a>Перекомпилирование файлов Razor при изменении

<xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> возвращает или задает значение, определяющее, выполняется ли повторная компиляция и обновление файлов Razor (представления Razor и Razor Pages) при изменении файлов на диске.

Если задано значение `true`, [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) отслеживает изменения в файлах Razor в настроенных экземплярах <xref:Microsoft.Extensions.FileProviders.IFileProvider>.

Значение по умолчанию — `true` для:

* приложений ASP.NET Core 2.1 или более ранней версии;
* приложений ASP.NET Core 2.2 или более поздней версии в среде разработки.

<xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> связан с параметром совместимости и может вести себя по-разному в зависимости от настроенной версии совместимости для приложения. Настройте приложения, задав для <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> приоритет над значением, которое содержится в версии совместимости приложения.

Если версия совместимости приложения имеет значение <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> или более ранее, то для <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> задается значение `true` (если не задано явно).

Если версия совместимости приложения имеет значение <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> или более позднее, то для <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> задается значение `false` (если не используется среда разработки или значение не задано явным образом).

Рекомендации и примеры настройки версии совместимости приложения см. в статье <xref:mvc/compatibility-version>.

## <a name="additional-resources"></a>Дополнительные ресурсы

::: moniker range="= aspnetcore-1.1"

* <xref:mvc/views/overview>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <xref:razor-pages/index>
* <xref:mvc/views/overview>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>

::: moniker-end
