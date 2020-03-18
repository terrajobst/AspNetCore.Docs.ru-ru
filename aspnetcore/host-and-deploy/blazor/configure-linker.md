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
ms.openlocfilehash: 263b85a3213c1da233e4c96095faaf39d0a8e13f
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78648610"
---
# <a name="configure-the-linker-for-aspnet-core-blazor"></a>Настройка компоновщика для ASP.NET Core Blazor

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor выполняет компоновку [промежуточного языка (IL)](/dotnet/standard/managed-code#intermediate-language--execution) во время сборки, чтобы затем удалить ненужные IL из выходных сборок приложения.

Управлять компоновкой сборок можно одним из следующих способов:

* отключить компоновку глобально с помощью [свойства MSBuild](#disable-linking-with-a-msbuild-property);
* управлять компоновкой каждой сборки с помощью [файла конфигурации](#control-linking-with-a-configuration-file).

## <a name="disable-linking-with-a-msbuild-property"></a>Отключение компоновки с помощью свойства MSBuild

Компоновка включена по умолчанию при сборке приложения, включая публикацию. Чтобы отключить компоновку для всех сборок, для свойства MSBuild `BlazorLinkOnBuild` задайте значение `false` в файле проекта:

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a>Управление компоновкой с помощью файла конфигурации

Чтобы управлять компоновкой каждой сборки, нужно предоставить XML-файл конфигурации и указать его как элемент MSBuild в файле проекта.

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```

*Linker.XML*:

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

Дополнительные сведения см. на странице [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor) (Компоновщик IL. Синтаксис дескриптора XML).

### <a name="configure-the-linker-for-internationalization"></a>Настройка компоновщика для интернационализации

По умолчанию конфигурация компоновщика Blazor для приложений Blazor WebAssembly исключает сведения об интернационализации, кроме явно запрошенных языковых стандартов. Удаление этих сборок уменьшает размер приложения.

Чтобы указать, какие сборки I18N необходимо оставить, задайте свойство MSBuild `<MonoLinkerI18NAssemblies>` в файле проекта:

```xml
<PropertyGroup>
  <MonoLinkerI18NAssemblies>{all|none|REGION1,REGION2,...}</MonoLinkerI18NAssemblies>
</PropertyGroup>
```

| Значение региона     | Сборка для одного региона    |
| ---------------- | ----------------------- |
| `all`            | Включены все сборки |
| `cjk`            | *I18N.CJK.dll*          |
| `mideast`        | *I18N.MidEast.dll*      |
| `none` (по умолчанию) | Отсутствуют                    |
| `other`          | *I18N.Other.dll*        |
| `rare`           | *I18N.Rare.dll*         |
| `west`           | *I18N.West.dll*         |

Для разделения нескольких значений используйте запятую (например, `mideast,west`).

Дополнительные сведения см. на странице [I18N: библиотека платформы интернационализации Pnetlib (репозиторий mono/mono GitHub)](https://github.com/mono/mono/tree/master/mcs/class/I18N).
