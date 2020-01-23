---
title: Настройка компоновщика для ASP.NET Core Blazor
author: guardrex
description: Узнайте, как управлять компоновщиком для промежуточного языка (IL) при создании приложения Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/configure-linker
ms.openlocfilehash: cdcd62915b8f1bae26773ed91e55973527e158f6
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/17/2020
ms.locfileid: "76160279"
---
# <a name="configure-the-linker-for-aspnet-core-opno-locblazor"></a><span data-ttu-id="175f5-103">Настройка компоновщика для ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="175f5-103">Configure the Linker for ASP.NET Core Blazor</span></span>

<span data-ttu-id="175f5-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="175f5-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor<span data-ttu-id="175f5-105"> выполняет компоновку [промежуточного языка (IL)](/dotnet/standard/managed-code#intermediate-language--execution) во время сборки, чтобы затем удалить ненужные IL из выходных сборок приложения.</span><span class="sxs-lookup"><span data-stu-id="175f5-105"> performs [Intermediate Language (IL)](/dotnet/standard/managed-code#intermediate-language--execution) linking during a build to remove unnecessary IL from the app's output assemblies.</span></span>

<span data-ttu-id="175f5-106">Управлять компоновкой сборок можно одним из следующих способов:</span><span class="sxs-lookup"><span data-stu-id="175f5-106">Control assembly linking using either of the following approaches:</span></span>

* <span data-ttu-id="175f5-107">отключить компоновку глобально с помощью [свойства MSBuild](#disable-linking-with-a-msbuild-property);</span><span class="sxs-lookup"><span data-stu-id="175f5-107">Disable linking globally with a [MSBuild property](#disable-linking-with-a-msbuild-property).</span></span>
* <span data-ttu-id="175f5-108">управлять компоновкой каждой сборки с помощью [файла конфигурации](#control-linking-with-a-configuration-file).</span><span class="sxs-lookup"><span data-stu-id="175f5-108">Control linking on a per-assembly basis with a [configuration file](#control-linking-with-a-configuration-file).</span></span>

## <a name="disable-linking-with-a-msbuild-property"></a><span data-ttu-id="175f5-109">Отключение компоновки с помощью свойства MSBuild</span><span class="sxs-lookup"><span data-stu-id="175f5-109">Disable linking with a MSBuild property</span></span>

<span data-ttu-id="175f5-110">Компоновка включена по умолчанию при сборке приложения, включая публикацию.</span><span class="sxs-lookup"><span data-stu-id="175f5-110">Linking is enabled by default when an app is built, which includes publishing.</span></span> <span data-ttu-id="175f5-111">Чтобы отключить компоновку для всех сборок, для свойства MSBuild `BlazorLinkOnBuild` задайте значение `false` в файле проекта:</span><span class="sxs-lookup"><span data-stu-id="175f5-111">To disable linking for all assemblies, set the `BlazorLinkOnBuild` MSBuild property to `false` in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a><span data-ttu-id="175f5-112">Управление компоновкой с помощью файла конфигурации</span><span class="sxs-lookup"><span data-stu-id="175f5-112">Control linking with a configuration file</span></span>

<span data-ttu-id="175f5-113">Чтобы управлять компоновкой каждой сборки, нужно предоставить XML-файл конфигурации и указать его как элемент MSBuild в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="175f5-113">Control linking on a per-assembly basis by providing an XML configuration file and specifying the file as a MSBuild item in the project file:</span></span>

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```

<span data-ttu-id="175f5-114">*Linker.XML*:</span><span class="sxs-lookup"><span data-stu-id="175f5-114">*Linker.xml*:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--
  This file specifies which parts of the BCL or Blazor packages must not be
  stripped by the IL Linker even if they aren't referenced by user code.
-->
<linker>
  <assembly fullname="mscorlib">
    <!--
      Preserve the methods in WasmRuntime because its methods are called by 
      JavaScript client-side code to implement timers.
      Fixes: https://github.com/dotnet/blazor/issues/239
    -->
    <type fullname="System.Threading.WasmRuntime" />
  </assembly>
  <assembly fullname="System.Core">
    <!--
      System.Linq.Expressions* is required by Json.NET and any 
      expression.Compile caller. The assembly isn't stripped.
    -->
    <type fullname="System.Linq.Expressions*" />
  </assembly>
  <!--
    In this example, the app's entry point assembly is listed. The assembly
    isn't stripped by the IL Linker.
  -->
  <assembly fullname="MyCoolBlazorApp" />
</linker>
```

<span data-ttu-id="175f5-115">Дополнительные сведения см. на странице [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor) (Компоновщик IL. Синтаксис дескриптора XML).</span><span class="sxs-lookup"><span data-stu-id="175f5-115">For more information, see [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span></span>

### <a name="configure-the-linker-for-internationalization"></a><span data-ttu-id="175f5-116">Настройка компоновщика для интернационализации</span><span class="sxs-lookup"><span data-stu-id="175f5-116">Configure the linker for internationalization</span></span>

<span data-ttu-id="175f5-117">По умолчанию конфигурация компоновщика Blazor для приложений Blazor WebAssembly исключает сведения об интернационализации, кроме явно запрошенных языковых стандартов.</span><span class="sxs-lookup"><span data-stu-id="175f5-117">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="175f5-118">Удаление этих сборок уменьшает размер приложения.</span><span class="sxs-lookup"><span data-stu-id="175f5-118">Removing these assemblies minimizes the app's size.</span></span>

<span data-ttu-id="175f5-119">Чтобы указать, какие сборки I18N необходимо оставить, задайте свойство MSBuild `<MonoLinkerI18NAssemblies>` в файле проекта:</span><span class="sxs-lookup"><span data-stu-id="175f5-119">To control which I18N assemblies are retained, set the `<MonoLinkerI18NAssemblies>` MSBuild property in the project file:</span></span>

```xml
<PropertyGroup>
  <MonoLinkerI18NAssemblies>{all|none|REGION1,REGION2,...}</MonoLinkerI18NAssemblies>
</PropertyGroup>
```

| <span data-ttu-id="175f5-120">Значение региона</span><span class="sxs-lookup"><span data-stu-id="175f5-120">Region Value</span></span>     | <span data-ttu-id="175f5-121">Сборка для одного региона</span><span class="sxs-lookup"><span data-stu-id="175f5-121">Mono region assembly</span></span>    |
| ---------------- | ----------------------- |
| `all`            | <span data-ttu-id="175f5-122">Включены все сборки</span><span class="sxs-lookup"><span data-stu-id="175f5-122">All assemblies included</span></span> |
| `cjk`            | <span data-ttu-id="175f5-123">*I18N.CJK.dll*</span><span class="sxs-lookup"><span data-stu-id="175f5-123">*I18N.CJK.dll*</span></span>          |
| `mideast`        | <span data-ttu-id="175f5-124">*I18N.MidEast.dll*</span><span class="sxs-lookup"><span data-stu-id="175f5-124">*I18N.MidEast.dll*</span></span>      |
| <span data-ttu-id="175f5-125">`none` (по умолчанию)</span><span class="sxs-lookup"><span data-stu-id="175f5-125">`none` (default)</span></span> | <span data-ttu-id="175f5-126">Отсутствуют</span><span class="sxs-lookup"><span data-stu-id="175f5-126">None</span></span>                    |
| `other`          | <span data-ttu-id="175f5-127">*I18N.Other.dll*</span><span class="sxs-lookup"><span data-stu-id="175f5-127">*I18N.Other.dll*</span></span>        |
| `rare`           | <span data-ttu-id="175f5-128">*I18N.Rare.dll*</span><span class="sxs-lookup"><span data-stu-id="175f5-128">*I18N.Rare.dll*</span></span>         |
| `west`           | <span data-ttu-id="175f5-129">*I18N.West.dll*</span><span class="sxs-lookup"><span data-stu-id="175f5-129">*I18N.West.dll*</span></span>         |

<span data-ttu-id="175f5-130">Для разделения нескольких значений используйте запятую (например, `mideast,west`).</span><span class="sxs-lookup"><span data-stu-id="175f5-130">Use a comma to separate multiple values (for example, `mideast,west`).</span></span>

<span data-ttu-id="175f5-131">Дополнительные сведения см. на странице [I18N: библиотека платформы интернационализации Pnetlib (репозиторий mono/mono GitHub)](https://github.com/mono/mono/tree/master/mcs/class/I18N).</span><span class="sxs-lookup"><span data-stu-id="175f5-131">For more information, see [I18N: Pnetlib Internationalization Framework Libary (mono/mono GitHub repository)](https://github.com/mono/mono/tree/master/mcs/class/I18N).</span></span>
