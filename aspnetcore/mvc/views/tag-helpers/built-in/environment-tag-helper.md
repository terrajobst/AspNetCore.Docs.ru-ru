---
title: Вспомогательная функция тега среды в ASP.NET Core
author: pkellner
description: Определенная в ASP.NET Core вспомогательная функция тега среды, включая все свойства
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 379f58ed37329f047d53adf1dcfdfd2ad6a6ca4e
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64887679"
---
# <a name="environment-tag-helper-in-aspnet-core"></a>Вспомогательная функция тега среды в ASP.NET Core

Авторы: [Питер Кельнер (Peter Kellner)](http://peterkellner.net), [Хишам Бин Атейя (Hisham Bin Ateya)](https://twitter.com/hishambinateya) и [Люк Лэтэм (Luke Latham)](https://github.com/guardrex)

Вспомогательная функция тега среды условно отрисовывает заключенное в нее содержимое с учетом текущей [среды размещения](xref:fundamentals/environments). Единственный атрибут вспомогательной функции тега среды, `names`, — это разделенный запятыми список имен сред. Если одно из указанных имен среды соответствует текущей среде, включенное содержимое подготавливается к просмотру.

Общие сведения о вспомогательных функциях тегов см. здесь: <xref:mvc/views/tag-helpers/intro>.

## <a name="environment-tag-helper-attributes"></a>Атрибуты вспомогательной функции тега среды

### <a name="names"></a>имена

`names` принимает одно имя среды размещения или список разделенных запятыми имен сред размещения, которые запускают отрисовку включенного в функцию содержимого.

Значения среды сравниваются с текущим значением, возвращенным [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*). При сравнении регистр не учитывается.

В приведенном ниже примере используется вспомогательная функция тега среды. Содержимое подготавливается к просмотру в том случае, если среда размещения является промежуточной или рабочей:

```cshtml
<environment names="Staging,Production">
    <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

::: moniker range=">= aspnetcore-2.0"

## <a name="include-and-exclude-attributes"></a>Атрибуты include и exclude

Атрибуты `include` & `exclude` управляют отрисовкой включенного содержимого на основе имен включенных или исключенных сред размещения.

### <a name="include"></a>include

Свойство `include` ведет себя так же для атрибута `names`. Среда, указанная в значении атрибута `include`, должна соответствовать среде размещения приложения ([IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*)) для отрисовки содержимого тега `<environment>`.

```cshtml
<environment include="Staging,Production">
    <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude"></a>exclude

В отличие от атрибута `include`, содержимое тега `<environment>` отрисовывается, когда среда размещения не соответствует среде, указанной в значении атрибута `exclude`.

```cshtml
<environment exclude="Development">
    <strong>HostingEnvironment.EnvironmentName is not Development</strong>
</environment>
```

::: moniker-end

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/environments>
