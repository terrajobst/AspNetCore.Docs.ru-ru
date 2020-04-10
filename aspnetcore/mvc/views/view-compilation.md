---
title: Компиляция файлов Razor в ASP.NET Core
author: rick-anderson
description: Узнайте, как происходит компиляция файлов Razor в приложении ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 4/8/2020
uid: mvc/views/view-compilation
ms.openlocfilehash: 7f329ffb4c63e8699663f49720145984bb8802fd
ms.sourcegitcommit: 9a46e78c79d167e5fa0cddf89c1ef584e5fe1779
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/09/2020
ms.locfileid: "80994611"
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="2f37b-103">Компиляция файлов Razor в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2f37b-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="2f37b-104">Автор: [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2f37b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2f37b-105">Файлы Razor с расширением *CSHTML* компилируются и во время сборки, и во время публикации с помощью [пакета SDK для Razor](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="2f37b-105">Razor files with a *.cshtml* extension are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span> <span data-ttu-id="2f37b-106">Компиляцию в среде выполнения при необходимости можно включить, настроив приложение.</span><span class="sxs-lookup"><span data-stu-id="2f37b-106">Runtime compilation may be optionally enabled by configuring your application.</span></span>

## <a name="razor-compilation"></a><span data-ttu-id="2f37b-107">Компиляция Razor</span><span class="sxs-lookup"><span data-stu-id="2f37b-107">Razor compilation</span></span>

<span data-ttu-id="2f37b-108">Компиляция файлов Razor во время сборки и публикации включена по умолчанию с помощью пакета SDK для Razor.</span><span class="sxs-lookup"><span data-stu-id="2f37b-108">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="2f37b-109">Если эта возможность включена, компиляция в среде выполнения дополняет компиляцию во время сборки, позволяя обновлять файлы Razor при редактировании.</span><span class="sxs-lookup"><span data-stu-id="2f37b-109">When enabled, runtime compilation complements build-time compilation, allowing Razor files to be updated if they are edited.</span></span>

## <a name="runtime-compilation"></a><span data-ttu-id="2f37b-110">компиляция среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="2f37b-110">Runtime compilation</span></span>

<span data-ttu-id="2f37b-111">Чтобы включить компиляцию в среде выполнения для всех сред и режимов конфигурации, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="2f37b-111">To enable runtime compilation for all environments and configuration modes:</span></span>

1. <span data-ttu-id="2f37b-112">Установите пакет [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) NuGet.</span><span class="sxs-lookup"><span data-stu-id="2f37b-112">Install the [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) NuGet package.</span></span>

1. <span data-ttu-id="2f37b-113">обновите метод `Startup.ConfigureServices` в проекте, чтобы включить вызов `AddRazorRuntimeCompilation`.</span><span class="sxs-lookup"><span data-stu-id="2f37b-113">Update the project's `Startup.ConfigureServices` method to include a call to `AddRazorRuntimeCompilation`.</span></span> <span data-ttu-id="2f37b-114">Пример:</span><span class="sxs-lookup"><span data-stu-id="2f37b-114">For example:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddRazorPages()
            .AddRazorRuntimeCompilation();

        // code omitted for brevity
    }
    ```

### <a name="conditionally-enable-runtime-compilation"></a><span data-ttu-id="2f37b-115">Условное включение компиляции в среде выполнения</span><span class="sxs-lookup"><span data-stu-id="2f37b-115">Conditionally enable runtime compilation</span></span>

<span data-ttu-id="2f37b-116">Компиляцию в среде выполнения можно включить так, чтобы она была доступна только для локальной рабочей среды.</span><span class="sxs-lookup"><span data-stu-id="2f37b-116">Runtime compilation can be enabled such that it's only available for local development.</span></span> <span data-ttu-id="2f37b-117">Подобное условное включение гарантирует, что опубликованные выходные данные:</span><span class="sxs-lookup"><span data-stu-id="2f37b-117">Conditionally enabling in this manner ensures that the published output:</span></span>

* <span data-ttu-id="2f37b-118">используют скомпилированные представления;</span><span class="sxs-lookup"><span data-stu-id="2f37b-118">Uses compiled views.</span></span>
* <span data-ttu-id="2f37b-119">имеют меньший размер;</span><span class="sxs-lookup"><span data-stu-id="2f37b-119">Is smaller in size.</span></span>
* <span data-ttu-id="2f37b-120">не включают наблюдатели файлов в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="2f37b-120">Doesn't enable file watchers in production.</span></span>

<span data-ttu-id="2f37b-121">Чтобы включить компиляцию в среде выполнения для конкретной среды и конкретного режима конфигурации, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="2f37b-121">To enable runtime compilation based on the environment and configuration mode:</span></span>

1. <span data-ttu-id="2f37b-122">Условно сошлитесь на пакет [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) с учетом активного значения `Configuration`:</span><span class="sxs-lookup"><span data-stu-id="2f37b-122">Conditionally reference the [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) package based on the active `Configuration` value:</span></span>

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation" Version="3.1.0" Condition="'$(Configuration)' == 'Debug'" />
    ```

1. <span data-ttu-id="2f37b-123">обновите метод `Startup.ConfigureServices` в проекте, чтобы включить вызов `AddRazorRuntimeCompilation`.</span><span class="sxs-lookup"><span data-stu-id="2f37b-123">Update the project's `Startup.ConfigureServices` method to include a call to `AddRazorRuntimeCompilation`.</span></span> <span data-ttu-id="2f37b-124">Условно выполните `AddRazorRuntimeCompilation`, чтобы он выполнялся только в режиме отладки, если для переменной `ASPNETCORE_ENVIRONMENT` задано значение `Development`:</span><span class="sxs-lookup"><span data-stu-id="2f37b-124">Conditionally execute `AddRazorRuntimeCompilation` such that it only runs in Debug mode when the `ASPNETCORE_ENVIRONMENT` variable is set to `Development`:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="2f37b-125">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="2f37b-125">Additional resources</span></span>

* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="2f37b-126">Файл Razor компилируется в среде выполнения при вызове связанной страницы Razor или представления MVC.</span><span class="sxs-lookup"><span data-stu-id="2f37b-126">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="2f37b-127">Файлы Razor компилируются и во время сборки, и во время публикации с помощью [пакета SDK для Razor](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="2f37b-127">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span>

## <a name="razor-compilation"></a><span data-ttu-id="2f37b-128">Компиляция Razor</span><span class="sxs-lookup"><span data-stu-id="2f37b-128">Razor compilation</span></span>

<span data-ttu-id="2f37b-129">Компиляция файлов Razor во время сборки и публикации включена по умолчанию с помощью пакета SDK для Razor.</span><span class="sxs-lookup"><span data-stu-id="2f37b-129">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="2f37b-130">Редактирование файлов Razor после их обновления поддерживается во время сборки.</span><span class="sxs-lookup"><span data-stu-id="2f37b-130">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="2f37b-131">По умолчанию только скомпилированные файлы *Views.dll*, а не *.cshtml*, и сборки, необходимые для компиляции файлов Razor, развертываются с приложением.</span><span class="sxs-lookup"><span data-stu-id="2f37b-131">By default, only the compiled *Views.dll* and no *.cshtml* files or references assemblies required to compile Razor files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2f37b-132">Средство предварительной компиляции является нерекомендуемым и будет удалено в ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="2f37b-132">The precompilation tool has been deprecated, and will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="2f37b-133">Мы советуем перейти на [пакет SDK для Razor](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="2f37b-133">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="2f37b-134">Пакет SDK для Razor применяется только в том случае, если в файле проекта не заданы свойства предварительной компиляции.</span><span class="sxs-lookup"><span data-stu-id="2f37b-134">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="2f37b-135">Например, если в *CSPROJ*-файле для свойства `MvcRazorCompileOnPublish` установлено значение `true`, пакет SDK для Razor будет отключен.</span><span class="sxs-lookup"><span data-stu-id="2f37b-135">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>

## <a name="runtime-compilation"></a><span data-ttu-id="2f37b-136">компиляция среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="2f37b-136">Runtime compilation</span></span>

<span data-ttu-id="2f37b-137">Компиляция во время сборки дополняется компиляцией файлов Razor в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="2f37b-137">Build-time compilation is supplemented by runtime compilation of Razor files.</span></span> <span data-ttu-id="2f37b-138">ASP.NET Core MVC будет перекомпилировать файлы Razor, когда содержимое файла *.cshtml* меняется.</span><span class="sxs-lookup"><span data-stu-id="2f37b-138">ASP.NET Core MVC will recompile Razor files when the contents of a *.cshtml* file change.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2f37b-139">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="2f37b-139">Additional resources</span></span>

* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>

::: moniker-end