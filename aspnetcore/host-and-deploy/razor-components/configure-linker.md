---
title: Настройка компоновщика для Blazor
author: guardrex
description: Сведения о том, как управлять компоновщиком для промежуточного языка (IL) при создании приложения Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/20/2019
uid: host-and-deploy/razor-components/configure-linker
ms.openlocfilehash: 7c53e7912ec3b0ae471ea38777f874f55a32487d
ms.sourcegitcommit: 0945078a09c372f17e9b003758ed87e99c2449f4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/21/2019
ms.locfileid: "56647945"
---
# <a name="configure-the-linker-for-blazor"></a>Настройка компоновщика для Blazor

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Blazor выполняет компоновку для [промежуточного языка (IL)](/dotnet/standard/managed-code#intermediate-language--execution) при каждой сборке в режиме выпуска, чтобы удалить ненужный код из выходных сборок.

Управлять компоновкой сборок можно одним из следующих способов:

* отключить связывание глобально с помощью свойства MSBuild;
* управлять компоновкой каждой сборки с помощью файла конфигурации.

## <a name="disable-linking-with-an-msbuild-property"></a>Отключение компоновки с помощью свойства MSBuild

Компоновка включена по умолчанию в режиме выпуска при сборке приложения, включая публикацию. Чтобы отключить компоновку для всех сборок, для свойства MSBuild `<BlazorLinkOnBuild>` задайте значение `false` в файле проекта:

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a>Управление компоновкой с помощью файла конфигурации

Вы можете управлять компоновкой каждой сборки. Для этого нужно предоставить XML-файл конфигурации и указать его как элемент MSBuild в файле проекта.

Ниже приведен пример файла конфигурации (*Linker.xml*):

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

Дополнительные сведения о формате файла конфигурации см. в разделе [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor) (Компоновщик IL. Синтаксис дескриптора XML).

Укажите файл конфигурации в файле проекта с помощью элемента `BlazorLinkerDescriptor`:

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```
