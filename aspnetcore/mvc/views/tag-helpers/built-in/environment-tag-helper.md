---
title: "Вспомогательный модуль тега среды ASP.NET Core"
author: pkellner
description: "Вспомогательный тега среды ASP.NET Core определен, включая все свойства"
keywords: "ASP.NET Core, вспомогательная функция тегов"
ms.author: riande
manager: wpickett
ms.date: 07/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 2639e4d7494e752462a1a2cb0648042a2d2d06ec
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="environment-tag-helper-in-aspnet-core"></a>Вспомогательный модуль тега среды ASP.NET Core

По [Kellner Питер](http://peterkellner.net) и [Ateya Hisham Bin](https://twitter.com/hishambinateya)

Вспомогательный тега среды условно отрисовывает его вложенного содержимого на основании текущей среде размещения. Его одного атрибута `names` является разделенный запятыми список среды кодов, если любой совпадают в текущей среде, запускающих заключено содержимое для отображения.

## <a name="environment-tag-helper-attributes"></a>Атрибуты вспомогательного тега среды

### <a name="names"></a>имена

Принимает только одно имя среды размещения или список разделенных запятыми имен среды, триггер подготовки к просмотру вложенного содержимого размещения.

Эти значения сравниваются с текущее значение, возвращаемое из статического свойства ASP.NET Core `HostingEnvironment.EnvironmentName`.  Это значение является одним из следующих: **промежуточной**; **Разработки** или **рабочей**. Сравнение не учитывает регистр.

Примером является допустимым `environment` модуль тег:

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a>Включение и исключение атрибуты

Добавляет 2.x ASP.NET Core `include`  &  `exclude` атрибуты. Эти атрибуты определяют визуализации на основе имен среды размещения включенный или исключенный вложенного содержимого.

### <a name="include-aspnet-core-20-and-later"></a>Включить ASP.NET Core 2.0 и более поздних версий

`include` Свойство имеет такое же поведение из `names` атрибута в ASP.NET версии 1.0 Core.

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a>исключить Core ASP.NET 2.0 и более поздних версий

Напротив `exclude` позволяет `EnvironmentTagHelper` отображения вложенного содержимого для всех имен среды размещения, за исключением ключи, указанный вами.

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
