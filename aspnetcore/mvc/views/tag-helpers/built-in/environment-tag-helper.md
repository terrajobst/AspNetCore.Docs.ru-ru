---
title: Вспомогательная функция тега среды в ASP.NET Core
author: pkellner
description: Определенная в ASP.NET Core вспомогательная функция тега среды, включая все свойства
ms.author: riande
ms.date: 07/14/2017
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 4a283a3a03aa6cac228ec6effd02e3f1095be260
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342228"
---
# <a name="environment-tag-helper-in-aspnet-core"></a>Вспомогательная функция тега среды в ASP.NET Core

Авторы: [Питер Кельнер (Peter Kellner)](http://peterkellner.net) и [Хишам Бин Атея](https://twitter.com/hishambinateya) (Hisham Bin Ateya)

Вспомогательная функция тега среды условно отрисовывает заключенное в нее содержимое с учетом текущей среды размещения. Ее единственным атрибутом `names` является разделенный запятыми список имен сред, который при наличии совпадения с текущей средой запустит отрисовку содержимого.

## <a name="environment-tag-helper-attributes"></a>Атрибуты вспомогательной функции тега среды

### <a name="names"></a>имена

Принимает одно имя среды размещения или список разделенных запятыми имен сред размещения, которые запускают отрисовку включенного в функцию содержимого.

Эти значения сравниваются с текущим значением, возвращаемым из статического свойства ASP.NET Core `HostingEnvironment.EnvironmentName`.  Это значение является одним из следующих: **Staging**, **Development** или **Production**. При сравнении регистр не учитывается.

Пример допустимой функции тега `environment`:

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a>Атрибуты include и exclude

В ASP.NET Core 2.x добавлены атрибуты `include` & `exclude`. Эти атрибуты управляют отрисовкой включенного содержимого на основе имен включенных или исключенных сред размещения.

### <a name="include-aspnet-core-20-and-later"></a>включить ASP.NET Core 2.0 и более поздние версии

Свойство `include` действует аналогично атрибуту `names` в ASP.NET Core 1.0.

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a>исключить ASP.NET Core 2.0 и более поздние версии

Напротив, свойство `exclude` позволяет `EnvironmentTagHelper` отрисовывать включенное в функцию содержимое для всех имен сред размещения, за исключением указанных.

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/environments>
