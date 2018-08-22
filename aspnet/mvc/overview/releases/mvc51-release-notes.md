---
uid: mvc/overview/releases/mvc51-release-notes
title: Новые возможности в ASP.NET MVC 5.1 | Документация Майкрософт
author: microsoft
description: ''
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 6cdf5ae42457ab10e30693a1b77b4aee65570be5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836986"
---
<a name="whats-new-in-aspnet-mvc-51"></a>Новые возможности в ASP.NET MVC 5.1
====================
по [Microsoft](https://github.com/microsoft)

В этом разделе описываются новые возможности ASP.NET Web MVC 5.1.

- [Требования к программному обеспечению](#SoftwareRequirements)
- [Скачать](#download)
- [Документация](#documentation)
- [Новые возможности в ASP.NET MVC 5.1](#new-features)

    - [Атрибут улучшения маршрутизации](#AttributeRouting)
    - [Поддержка начальной загрузки для редактора шаблонов](#Bootstrap)
    - [Поддержке нумераций в представлениях](#Enum)
    - [Малозаметная проверка для MinLength и MaxLength атрибутов](#Unobtrusive)
    - [Поддержка «this» контекст в ненавязчивого Ajax](#thisContext)
- [Известные проблемы и критические изменения](#KnownBreakingChanges)- [исправления ошибок](#bug-fixes)

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a>Требования к программному обеспечению

- Visual Studio 2012: Скачайте [ASP.NET и веб-инструменты 2013.1 для Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).
- Visual Studio 2013: Загрузки [Visual Studio 2013 с обновлением 1](https://go.microsoft.com/fwlink/?LinkId=390064). Это обновление требуется для изменения представления Razor ASP.NET MVC 5.1.

<a id="download"></a>
## <a name="download"></a>Скачать

Компоненты среды выполнения выпускаются в виде пакетов NuGet из коллекции NuGet. Выполните все пакеты среды выполнения [семантического управления версиями](http://semver.org/) спецификации. Последнюю версию пакета ASP.NET MVC 5.1 RTM имеет следующую версию: «5.1.2». Вы можете установить или обновить эти пакеты с помощью [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/). Этот выпуск также включает соответствующие локализованные пакеты в NuGet.

Можно установить или обновить на выпущенных пакетов NuGet с помощью консоли диспетчера пакетов NuGet:

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Документация

Учебники и другие сведения о ASP.NET MVC 5.1 RTM доступны на веб-сайте ASP.NET ( https://www.asp.net). 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a>Новые возможности в ASP.NET MVC 5.1

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a>Атрибут улучшения маршрутизации

 Выбор маршрута на основе атрибутов, маршрутизация теперь поддерживает ограничения, обеспечивая управление версиями и заголовок. Многие аспекты маршруты с атрибутами теперь настраиваются с помощью `IDirectRouteFactory` интерфейс и `RouteFactoryAttribute` класса. Префикс маршрута теперь можно расширять посредством `IRoutePrefix` интерфейс и `RoutePrefixAttribute` класса. 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a>Поддержке нумераций в представлениях

1. Новый `@Html.EnumDropDownListFor()` вспомогательные методы. Они должны использоваться как и большинство вспомогательных методов HTML, если выражение должно возвращать [перечисления](https://msdn.microsoft.com/en-us/library/cc138362.aspx) тип или [Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) где `T` является [перечисления](https://msdn.microsoft.com/en-us/library/cc138362.aspx) типа. Используйте `EnumHelper.IsValidForEnumHelper()` для проверки этих требований.
2. Новый `EnumHelper.GetSelectList()` методы, которые возвращают `IList<SelectListItem>`. Это полезно, когда необходимость в управлении в списке выбора, перед вызовом функции, например, `@Html.DropDownListFor()`, или если требуется отобразить имена которого `@Html.EnumDropDownListFor()` показано.

В следующем коде показано эти API-интерфейсы.

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

Полный пример можно просмотреть [здесь](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a>Поддержка начальной загрузки для редактора шаблонов

Теперь мы разрешаем передачи в атрибутах HTML в [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) как [анонимный объект](https://msdn.microsoft.com/en-us/library/bb397696.aspx).

Пример:

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a>Малозаметная проверка MinLengthAttribute и MaxLengthAttribute

Проверка на стороне клиента для строковых и массива типов будут поддерживаться, теперь для свойства с атрибутом [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) и [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) атрибуты.

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a>Поддержка «this» контекст в ненавязчивого Ajax

В функции обратного вызова (`OnBegin, OnComplete, OnFailure, OnSuccess`) теперь будут иметь возможность найти элемент вызов через `this` контекста. Пример:

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a>Известные проблемы и критические изменения

### <a name="attribute-routing"></a>Маршрутизация с помощью атрибутов

Неоднозначности в маршрутизации соответствует атрибута теперь сообщает ошибку вместо выбора первого совпадения.

Маршруты с атрибутами, запрещается использовать `{controller}` параметра и обратно с помощью `{action}` параметр на маршрутах уделено действия. Сценарии использования этих параметров большой вероятностью приведет к неоднозначности. 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Формирование шаблонов MVC и веб-API в проект с помощью 5.1 результатов пакеты в 5.0 пакеты для тех, которые еще не существуют в проекте

Обновление пакетов NuGet для ASP.NET MVC 5.1 RTM-версии не обновить инструменты Visual Studio, такие как формирование шаблонов ASP.NET или шаблон проекта веб-приложения ASP.NET. Они используют предыдущую версию пакетов среды выполнения ASP.NET (5.0.0.0). Таким образом формирование шаблонов ASP.NET установит предыдущую версию (5.0.0.0) необходимые пакеты, если они еще не доступны в проектах. Тем не менее формирование шаблонов ASP.NET в Visual Studio 2013 RTM или с обновлением 1 не перезаписывает последних пакетов в проектах. Если вы используете формирование шаблонов ASP.NET после обновления пакеты проектов Web API 2.1 или ASP.NET MVC 5.1, убедитесь, что в версиях веб-API и ASP.NET MVC являются согласованными. 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Выделение синтаксиса для представления Razor в Visual Studio 2013

Если вы обновляете до ASP.NET MVC 5.1 RTM-версии без обновления Visual Studio 2013, не будет поддержки редактора Visual Studio для выделения синтаксиса во время редактирования представлениями Razor. Необходимо будет обновить Visual Studio 2013 для получения этой поддержки. 

### <a name="type-renames"></a>Переименование типа

Некоторые типы, используемые для расширения маршрутизации атрибут переименовываются в окончательной версии 5.1.

| **Старое имя типа (5.1 версия-Кандидат)** | **Имя нового типа (5.1 RTM)** |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Исправления ошибок

Этот выпуск также включает несколько исправлений ошибок. Можно найти полный список здесь:

- [5.1.0 пакет](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 пакет](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

5.1.2 пакет содержит обновления IntelliSense, но не исправления ошибок.
