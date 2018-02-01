---
uid: mvc/overview/releases/mvc51-release-notes
title: "Новые возможности в ASP.NET MVC 5.1 | Документы Microsoft"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: be10486c9fd39738f44cdda4fedb409058017601
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-mvc-51"></a>Новые возможности в ASP.NET MVC 5.1
====================
по [Microsoft](https://github.com/microsoft)

В этом разделе описываются новые возможности в ASP.NET Web MVC 5.1.

- [Требования к программному обеспечению](#SoftwareRequirements)
- [Скачать](#download)
- [Документация](#documentation)
- [Новые возможности в ASP.NET MVC 5.1](#new-features)

    - [Атрибут улучшения маршрутизации](#AttributeRouting)
    - [Шаблоны редактора начальной загрузки поддерживаются](#Bootstrap)
    - [Поддержка перечислений в представлениях](#Enum)
    - [Ненавязчивой проверки для MinLength и MaxLength атрибутов](#Unobtrusive)
    - [Поддержка контекст «this» в ненавязчивого Ajax](#thisContext)
- [Известные проблемы и критические изменения](#KnownBreakingChanges)- [исправления ошибок](#bug-fixes)

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a>Требования к программному обеспечению

- Visual Studio 2012: Загрузите [ASP.NET и веб-средств 2013.1 для Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).
- Visual Studio 2013: Загрузите [Visual Studio 2013 с обновлением 1](https://go.microsoft.com/fwlink/?LinkId=390064). Это обновление требуется для изменения представления Razor ASP.NET MVC 5.1.

<a id="download"></a>
## <a name="download"></a>Скачать

Компоненты среды выполнения выпущены в виде пакетов NuGet при галереи NuGet. Выполните все пакеты среды выполнения [семантического управления версиями](http://semver.org/) спецификации. Последнюю версию пакета ASP.NET MVC 5.1 RTM имеет следующую версию: «5.1.2». Можно установить или обновить эти пакеты через [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/). Этот выпуск также включает соответствующие локализованные пакеты в NuGet.

Можно установить или обновить выпущенных пакетов NuGet, используя консоль диспетчера пакетов NuGet:

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Документация

Учебники и другие сведения о ASP.NET MVC 5.1 RTM доступны из веб-сайт ASP.NET (https://www.asp.net). 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a>Новые возможности в ASP.NET MVC 5.1

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a>Атрибут улучшения маршрутизации

 Выбор маршрута основан атрибут, маршрутизация теперь поддерживает ограничения, Включение управления версиями и заголовок. Многие аспекты маршруты атрибута теперь настраиваются через `IDirectRouteFactory` интерфейс и `RouteFactoryAttribute` класса. Префикс маршрута теперь можно расширять посредством `IRoutePrefix` интерфейс и `RoutePrefixAttribute` класса. 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a>Поддержка перечислений в представлениях

1. Новый `@Html.EnumDropDownListFor()` вспомогательные методы. Эту возможность можно использовать как и большинство вспомогательных методов HTML помнить, что выражение должно возвращать [перечисления](https://msdn.microsoft.com/en-us/library/cc138362.aspx) типа или [Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) где `T` является [перечисления](https://msdn.microsoft.com/en-us/library/cc138362.aspx) типа. Используйте `EnumHelper.IsValidForEnumHelper()` для проверки этих требований.
2. Новый `EnumHelper.GetSelectList()` методы, которые возвращают `IList<SelectListItem>`. Это полезно, когда необходимость в управлении список выбора до вызова метода, например, `@Html.DropDownListFor()`, или если требуется отобразить имена которого `@Html.EnumDropDownListFor()` показано.

В следующем коде показано эти API-интерфейсы.

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

Полный пример [здесь](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a>Шаблоны редактора начальной загрузки поддерживаются

Теперь допускается передача в атрибутах HTML в [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) как [анонимный объект](https://msdn.microsoft.com/en-us/library/bb397696.aspx).

Пример:

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a>Ненавязчивой проверки MinLengthAttribute и MaxLengthAttribute

Проверки на стороне клиента для строковых и массива типов теперь поддерживается для свойства декорированных [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) и [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) атрибуты.

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a>Поддержка контекст «this» в ненавязчивого Ajax

Функции обратного вызова (`OnBegin, OnComplete, OnFailure, OnSuccess`) теперь можно найти элемент вызова через `this` контекста. Пример:

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a>Известные проблемы и критические изменения

### <a name="attribute-routing"></a>Атрибут маршрутизации

Неоднозначности в маршрутизации соответствует атрибута теперь выдает ошибку вместо выбора первого совпадения.

Маршруты атрибутов не могут использовать `{controller}` параметра и с помощью `{action}` параметр маршрутов разместить на действия. Использует эти параметры большой вероятностью приведет к неоднозначности. 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>API MVC или веб-формирования шаблонов в проект 5.1 пакетов приводит к получению 5.0 пакетов для те, которые уже не существуют в проекте

Обновление пакетов NuGet для ASP.NET MVC 5.1 RTM не обновляет средства Visual Studio, такие как формирование шаблонов ASP.NET или шаблон проекта веб-приложения ASP.NET. Они используют предыдущую версию пакета среды выполнения ASP.NET (5.0.0.0). В результате формирование шаблонов ASP.NET установит предыдущую версию (5.0.0.0) необходимые пакеты, если они отсутствуют доступные в проектах. Формирование шаблонов ASP.NET в Visual Studio 2013 RTM или обновлением 1 не перезаписывает последние пакеты в проекте. Если вы используете формирование шаблонов ASP.NET после обновления пакеты проектов Web API 2.1 или ASP.NET MVC 5.1, убедитесь, что версии веб-API и ASP.NET MVC, согласованы. 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Выделение синтаксиса для представлений Razor в Visual Studio 2013

При обновлении до ASP.NET MVC 5.1 RTM без обновления Visual Studio 2013, не будет Поддержка редактора Visual Studio для выделения синтаксиса при изменении представлений Razor. Будет необходимо обновить Visual Studio 2013 для получения этой поддержки. 

### <a name="type-renames"></a>Переименование типа

Некоторые типы, используемые для расширения маршрутизации атрибута будут переименованы в 5.1 RTM.

| **Старое имя типа (5.1 версия-Кандидат)** | **Введите новое имя (5.1 RTM)** |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Исправления ошибок

Этот выпуск также включает несколько исправлений ошибок. Можно найти полный список здесь:

- [5.1.0 пакет](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 пакета](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

5.1.2 пакет содержит обновления IntelliSense, но без исправления ошибок.
