---
title: Компиляция файлов Razor в ASP.NET Core
author: rick-anderson
description: Узнайте, как происходит компиляция файлов Razor в приложении ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: cd096bba5eb580c0a606699a2bf7c36442fb56f7
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78652498"
---
# <a name="razor-file-compilation-in-aspnet-core"></a>Компиляция файлов Razor в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)

::: moniker range="= aspnetcore-1.1"

Файл Razor компилируется в среде выполнения при вызове связанного представления MVC. Публикация файла Razor во время сборки не поддерживается. При необходимости файлы Razor можно компилировать во время публикации и развертывать вместе с приложением &mdash; для этого используется средство предварительной компиляции.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Файл Razor компилируется в среде выполнения при вызове связанной страницы Razor или представления MVC. Публикация файла Razor во время сборки не поддерживается. При необходимости файлы Razor можно компилировать во время публикации и развертывать вместе с приложением &mdash; для этого используется средство предварительной компиляции.

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

Файл Razor компилируется в среде выполнения при вызове связанной страницы Razor или представления MVC. Файлы Razor компилируются и во время сборки, и во время публикации с помощью [пакета SDK для Razor](xref:razor-pages/sdk).

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

Файлы Razor с расширением *CSHTML* компилируются и во время сборки, и во время публикации с помощью [пакета SDK для Razor](xref:razor-pages/sdk). Компиляцию в среде выполнения при необходимости можно включить, настроив приложение.

::: moniker-end

## <a name="razor-compilation"></a>Компиляция Razor

::: moniker range=">= aspnetcore-3.0"

Компиляция файлов Razor во время сборки и публикации включена по умолчанию с помощью пакета SDK для Razor. Если эта возможность включена, компиляция в среде выполнения дополняет компиляцию во время сборки, позволяя обновлять файлы Razor при редактировании.

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

Компиляция файлов Razor во время сборки и публикации включена по умолчанию с помощью пакета SDK для Razor. Редактирование файлов Razor после их обновления поддерживается во время сборки. По умолчанию только скомпилированные файлы *Views.dll*, а не *.cshtml*, и сборки, необходимые для компиляции файлов Razor, развертываются с приложением.

> [!IMPORTANT]
> Средство предварительной компиляции является нерекомендуемым и будет удалено в ASP.NET Core 3.0. Мы советуем перейти на [пакет SDK для Razor](xref:razor-pages/sdk).
>
> Пакет SDK для Razor применяется только в том случае, если в файле проекта не заданы свойства предварительной компиляции. Например, если в *CSPROJ*-файле для свойства `MvcRazorCompileOnPublish` установлено значение `true`, пакет SDK для Razor будет отключен.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Если проект ориентирован на платформу .NET Framework, установите пакет NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

Если проект нацелен на .NET Core, никаких изменений не требуется.

В шаблонах проектов ASP.NET Core 2.x для свойства `MvcRazorCompileOnPublish` по умолчанию неявно задано значение `true`. Следовательно, этот элемент можно безопасно удалить из *CSPROJ*-файла.

> [!IMPORTANT]
> Средство предварительной компиляции является нерекомендуемым и будет удалено в ASP.NET Core 3.0. Мы советуем перейти на [пакет SDK для Razor](xref:razor-pages/sdk).
>
> Предварительная компиляция файлов Razor недоступна при выполнении [автономного развертывания](/dotnet/core/deploying/#self-contained-deployments-scd) в ASP.NET Core 2.0.

::: moniker-end

::: moniker range="= aspnetcore-1.1"

Задайте для свойства `MvcRazorCompileOnPublish` значение `true`и установите пакет NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/). Это показано в следующем примере *CSPROJ*:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

Подготовьте приложение к [развертыванию в зависимости от платформы](/dotnet/core/deploying/#framework-dependent-deployments-fdd) с помощью [команды публикации .NET Core CLI](/dotnet/core/tools/dotnet-publish). Например, выполните следующую команду в корневом элементе проекта:

```dotnetcli
dotnet publish -c Release
```

В случае успешной компиляции создается файл *\<<имя_проекта>.PrecompiledViews.dll*, содержащий скомпилированные файлы Razor. Например, следующий снимок экрана показывает содержимое файла *Index.cshtml* внутри *WebApplication1.PrecompiledViews.dll*:

![Представления Razor внутри библиотеки DLL](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="runtime-compilation"></a>компиляция среды выполнения.

::: moniker range="= aspnetcore-2.1"

Компиляция во время сборки дополняется компиляцией файлов Razor в среде выполнения. ASP.NET Core MVC будет перекомпилировать файлы Razor, когда содержимое файла *.cshtml* меняется.

::: moniker-end

::: moniker range="= aspnetcore-2.2"

Компиляция во время сборки дополняется компиляцией файлов Razor в среде выполнения. <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> возвращает или задает значение, определяющее, выполняется ли повторная компиляция и обновление файлов Razor (представления Razor и Razor Pages) при изменении файлов на диске.

Значение по умолчанию — `true` для:

* Если задана версия совместимости приложения <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> или более ранней версии
* Если задана версия совместимости приложения <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> или более поздняя версия и приложение находится в среде разработки <xref:Microsoft.AspNetCore.Hosting.HostingEnvironmentExtensions.IsDevelopment*>. Другими словами, файлы Razor не перекомпилируются вне среды разработки, если явным образом не задано <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange>.

Рекомендации и примеры настройки версии совместимости приложения см. в статье <xref:mvc/compatibility-version>.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

Чтобы включить компиляцию в среде выполнения для всех сред и режимов конфигурации, сделайте следующее:

1. установить пакет NuGet [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/).

1. обновите метод `Startup.ConfigureServices` в проекте, чтобы включить вызов `AddRazorRuntimeCompilation`. Пример:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddRazorPages()
            .AddRazorRuntimeCompilation();

        // code omitted for brevity
    }
    ```

### <a name="conditionally-enable-runtime-compilation"></a>Условное включение компиляции в среде выполнения

Компиляцию в среде выполнения можно включить так, чтобы она была доступна только для локальной рабочей среды. Подобное условное включение гарантирует, что опубликованные выходные данные:

* используют скомпилированные представления;
* имеют меньший размер;
* не включают наблюдатели файлов в рабочей среде.

Чтобы включить компиляцию в среде выполнения для конкретной среды и конкретного режима конфигурации, сделайте следующее:

1. Условно сошлитесь на пакет [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) с учетом активного значения `Configuration`:

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation" Version="3.1.0" Condition="'$(Configuration)' == 'Debug'" />
    ```

1. обновите метод `Startup.ConfigureServices` в проекте, чтобы включить вызов `AddRazorRuntimeCompilation`. Условно выполните `AddRazorRuntimeCompilation`, чтобы он выполнялся только в режиме отладки, если для переменной `ASPNETCORE_ENVIRONMENT` задано значение `Development`:

    ```csharp
    public IWebHostEnvironment Env { get; set; }

    public void ConfigureServices(IServiceCollection services)
    {
        IMvcBuilder builder = services.AddRazorPages();

    #if DEBUG
        if (Env.IsDevelopment())
        {
            builder.AddRazorRuntimeCompilation();
        }
    #endif

        // code omitted for brevity
    }
    ```

::: moniker-end

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
