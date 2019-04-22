---
title: Настройка компоновщика для Blazor
author: guardrex
description: Сведения о том, как управлять компоновщиком для промежуточного языка (IL) при создании приложения Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: host-and-deploy/blazor/configure-linker
ms.openlocfilehash: 01e18498a16e86392755b02b92ffda929669cb7d
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614741"
---
# <a name="configure-the-linker-for-blazor"></a><span data-ttu-id="397dc-103">Настройка компоновщика для Blazor</span><span class="sxs-lookup"><span data-stu-id="397dc-103">Configure the Linker for Blazor</span></span>

<span data-ttu-id="397dc-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="397dc-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="397dc-105">Blazor выполняет компоновку для [промежуточного языка (IL)](/dotnet/standard/managed-code#intermediate-language--execution) при каждой сборке в режиме выпуска, чтобы удалить ненужный код из выходных сборок приложения.</span><span class="sxs-lookup"><span data-stu-id="397dc-105">Blazor performs [Intermediate Language (IL)](/dotnet/standard/managed-code#intermediate-language--execution) linking during each Release mode build to remove unnecessary IL from the app's output assemblies.</span></span>

<span data-ttu-id="397dc-106">Управлять компоновкой сборок можно одним из следующих способов:</span><span class="sxs-lookup"><span data-stu-id="397dc-106">Control assembly linking using either of the following approaches:</span></span>

* <span data-ttu-id="397dc-107">отключить компоновку глобально с помощью [свойства MSBuild](#disable-linking-with-a-msbuild-property);</span><span class="sxs-lookup"><span data-stu-id="397dc-107">Disable linking globally with a [MSBuild property](#disable-linking-with-a-msbuild-property).</span></span>
* <span data-ttu-id="397dc-108">управлять компоновкой каждой сборки с помощью [файла конфигурации](#control-linking-with-a-configuration-file).</span><span class="sxs-lookup"><span data-stu-id="397dc-108">Control linking on a per-assembly basis with a [configuration file](#control-linking-with-a-configuration-file).</span></span>

## <a name="disable-linking-with-a-msbuild-property"></a><span data-ttu-id="397dc-109">Отключение компоновки с помощью свойства MSBuild</span><span class="sxs-lookup"><span data-stu-id="397dc-109">Disable linking with a MSBuild property</span></span>

<span data-ttu-id="397dc-110">Компоновка включена по умолчанию в режиме выпуска при сборке приложения, включая публикацию.</span><span class="sxs-lookup"><span data-stu-id="397dc-110">Linking is enabled by default in Release mode when an app is built, which includes publishing.</span></span> <span data-ttu-id="397dc-111">Чтобы отключить компоновку для всех сборок, для свойства MSBuild `<BlazorLinkOnBuild>` задайте значение `false` в файле проекта:</span><span class="sxs-lookup"><span data-stu-id="397dc-111">To disable linking for all assemblies, set the `<BlazorLinkOnBuild>` MSBuild property to `false` in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a><span data-ttu-id="397dc-112">Управление компоновкой с помощью файла конфигурации</span><span class="sxs-lookup"><span data-stu-id="397dc-112">Control linking with a configuration file</span></span>

<span data-ttu-id="397dc-113">Чтобы управлять компоновкой каждой сборки, нужно предоставить XML-файл конфигурации и указать его как элемент MSBuild в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="397dc-113">Control linking on a per-assembly basis by providing an XML configuration file and specifying the file as a MSBuild item in the project file:</span></span>

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```

<span data-ttu-id="397dc-114">*Linker.XML*:</span><span class="sxs-lookup"><span data-stu-id="397dc-114">*Linker.xml*:</span></span>

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
      Fixes: https://github.com/aspnet/Blazor/issues/239
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

<span data-ttu-id="397dc-115">Дополнительные сведения см. на странице [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor) (Компоновщик IL. Синтаксис дескриптора XML).</span><span class="sxs-lookup"><span data-stu-id="397dc-115">For more information, see [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span></span>
