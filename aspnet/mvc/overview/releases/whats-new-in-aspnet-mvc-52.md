---
uid: mvc/overview/releases/whats-new-in-aspnet-mvc-52
title: Новые возможности в ASP.NET MVC 5.2 | Документация Майкрософт
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 12/25/2014
ms.assetid: 97972587-2720-48b4-b158-f35f2e855fbf
msc.legacyurl: /mvc/overview/releases/whats-new-in-aspnet-mvc-52
msc.type: authoredcontent
ms.openlocfilehash: 76e1c13453c45a1b8ca71dd6435dcbfbd8bec66d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37801545"
---
<a name="whats-new-in-aspnet-mvc-52"></a>Новые возможности в ASP.NET MVC 5.2
====================
по [Microsoft](https://github.com/microsoft)

В этом разделе описываются новые возможности ASP.NET MVC 5.2, [Microsoft.AspNet.MVC 5.2.2](#52) и [бета-версия ASP.NET MVC 5.2.3](#mvc523Beta)

- [Требования к программному обеспечению](#softRequire)
- [Скачать](#download)
- [Документация](#documentation)
- [Новые возможности в ASP.NET MVC 5.2](#new-features)

    - [Атрибут улучшения маршрутизации](#attributerouting)
- [Известные проблемы и критические изменения](#knownbreakingchanges)
- [Исправления ошибок](#bug-fixes)
- [Microsoft.AspNet.MVC 5.2.2](#52)

<a id="softRequire"></a>
## <a name="software-requirements"></a>Требования к программному обеспечению

- Visual Studio 2012: Скачайте [ASP.NET и веб-инструменты 2013.1 для Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).
- Visual Studio 2013: Загрузки [Visual Studio 2013 с обновлением](https://go.microsoft.com/fwlink/?LinkId=390064) или более поздней версии. Это обновление требуется для изменения представления Razor ASP.NET MVC 5.2.

<a id="download"></a>
## <a name="download"></a>Скачать

Компоненты среды выполнения выпускаются в виде пакетов NuGet из коллекции NuGet. Выполните все пакеты среды выполнения [семантического управления версиями](http://semver.org/) спецификации. Последнюю версию пакета ASP.NET MVC 5.2 имеет следующую версию: «5.2.0». Вы можете установить или обновить эти пакеты с помощью [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/). Этот выпуск также включает соответствующие локализованные пакеты в NuGet.

Можно установить или обновить на выпущенных пакетов NuGet с помощью консоли диспетчера пакетов NuGet:

Microsoft.AspNet.Mvc Install-Package-версии не ниже 5.2.0

<a id="documentation"></a>
## <a name="documentation"></a>Документация

Учебники и другие сведения о ASP.NET MVC 5.2 доступны на веб-сайте ASP.NET ([https://www.asp.net/mvc](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-52"></a>Новые возможности в ASP.NET MVC 5.2

<a id="attributerouting"></a>
### <a name="attribute-routing-improvements"></a>Атрибут улучшения маршрутизации

Маршрутизация с помощью атрибутов теперь предоставляет точку расширения, вызывается IDirectRouteProvider, что позволяет полностью контролировать принципы обнаружения и настроить маршруты с атрибутами. IDirectRouteProvider отвечает за предоставление списка действия и контроллеры вместе с информацией о связанного маршрута для указания точности для этих действий требуется какие конфигурации маршрутизации. Реализация IDirectRouteProvider могут быть указаны при вызове MapAttributes/MapHttpAttributeRoutes.

Настройка IDirectRouteProvider будет простым путем расширения наших реализация по умолчанию, DefaultDirectRouteProvider. Этот класс предоставляет отдельный переопределяемый виртуальные методы, чтобы изменить логику для обнаружения атрибутов, создание записи маршрута и обнаружения префикс маршрута и префикс области.

С помощью нового атрибута маршрутизации расширяемость IDirectRouteProvider пользователь может сделайте следующее:

1. Поддерживает наследование атрибута маршрутов. Например в следующем сценарии контроллеры блог и Store при использовании атрибута соглашение для маршрутов, который определяется BaseController. 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample1.cs)]
2. Автоматически создать имена маршрутов для маршрутов на основе атрибутов. 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample2.cs)]
3. Изменение префиксов маршрутов в одном централизованном месте, прежде, чем маршруты добавляются в таблицу маршрутов.
4. Отфильтровывайте контроллеров, в которых требуется атрибут маршрута для поиска. Мы надеемся блога на 3 и 4 ближайшее время.

### <a name="facebook-fixes-for-changed-api-surface"></a>Facebook исправления для измененных поверхность API

Пакет MVC Facebook [было прервано](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=Facebook&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=All) из-за несколько API изменения, сделанные Facebook. Мы также выпустили новый пакет Facebook (Microsoft.AspNet.Facebook 1.0.0) для устранения этих проблем.

<a id="knownbreakingchanges"></a>
## <a name="known-issues-and-breaking-changes"></a>Известные проблемы и критические изменения

### <a name="scaffolding-mvcweb-api-into-a-project-with-520-packages-results-in-512-packages-for-ones-that-dont-already-exist-in-the-project"></a>Формирование шаблонов MVC и веб-API в проект без 5.2.0 пакетов приводит 5.1.2 пакеты для тех, которые еще не существуют в проекте

Обновление пакетов NuGet для ASP.NET MVC 5.2.0 не обновить инструменты Visual Studio, такие как формирование шаблонов ASP.NET или шаблон проекта веб-приложения ASP.NET. Они используют предыдущую версию пакетов среды выполнения ASP.NET (например 5.1.2 в обновлении 2). Таким образом формирование шаблонов ASP.NET установит предыдущую версию (например 5.1.2 в обновлении 2) необходимые пакеты, если они еще не доступны в проектах. Тем не менее формирование шаблонов ASP.NET в Visual Studio 2013 RTM или с обновлением 1 не перезаписывает последних пакетов в проектах. Если вы используете формирование шаблонов ASP.NET после обновления пакеты проектов веб-API 2.2 или ASP.NET MVC 5.2, убедитесь, что в версиях веб-API и ASP.NET MVC являются согласованными.

### <a name="microsoftjqueryunobtrusivevalidation-nuget-package-installation-fails-because-it-is-unable-to-find-a-version-of-microsoftjqueryunobtrusivevalidation-compatible-to-jquery-141"></a>Установка пакета Microsoft.jQuery.Unobtrusive.Validation NuGet завершается сбоем, так как не удается найти версию Microsoft.jQuery.Unobtrusive.Validation совместимость с jQuery 1.4.1

Microsoft.jQuery.Unobtrusive.Validation требует jQuery &gt;= 1.8 и jQuery.Validation &gt;= 1.8. But,jQuery.Validation (1,8) должен jQuery (&#8805; 1.3.2 &amp; &amp; &#8804; 1.6). Из-за этого когда диспетчер NuGet устанавливает JQuery 1.8 и jQuery.Validation 1.8, в то же время, происходит сбой. Если вы видите эту проблему, можно просто обновить версию jQuery.Validation для &gt; =  [1.8.0.1](https://www.nuget.org/packages/jQuery.Validation/1.8.0.1) с jQuery cap фиксированной во-первых, можно установить Microsoft.jQuery.Unobtrusive.Validation.

### <a name="the-jqueryvalidation-nuget-package-version-1130-does-not-recognize-some-international-email-addresses"></a>Jquery. Пакет nuget проверки версии 1.13.0 не распознает некоторые адреса электронной почты международного

версия пакета nuget jQuery.Validation 1.11.1 является последнюю известную версию, который распознает следующие допустимые адреса электронной почты. Возможно, все более поздние версии не сможет распознать их. Пример:

Стандарта интернационализации адресов (EAI) электронной почты (например, [ &#29992; &#25143; @domain.com ](mailto:&#29992;&#25143;@domain.com))   
 EAI + международное идентификаторы ресурсов (IRI) (например:., [ &#29992; &#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088; &#1092;](mailto:&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;))

Отображается в [https://github.com/jzaefferer/jquery-validation/issues/1222](https://github.com/jzaefferer/jquery-validation/issues/1222)

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Выделение синтаксиса для представления Razor в Visual Studio 2013

При обновлении до ASP.NET MVC 5.2 без обновления Visual Studio 2013, не будет поддержки редактора Visual Studio для выделения синтаксиса во время редактирования представлениями Razor. Необходимо будет обновить Visual Studio 2013 для получения этой поддержки.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Исправления ошибок и Minor обновления компонентов

Этот выпуск также включает несколько исправлений ошибок, а также дополнительный компонент обновления. Можно найти полный список здесь:

- [5.2 пакета](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<a id="52"></a>
## <a name="microsoftaspnetmvc-522"></a>Microsoft.AspNet.MVC 5.2.2

Этот выпуск не все новые функции и исправления ошибок в MVC. Мы внесли [изменить на веб-страницах](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) для значительного выигрыша в производительности и затем обновили все наши связанные пакеты, мы владельцем зависят от этой новой версии веб-страниц.

<a id="mvc523Beta"></a>
## <a name="aspnet-mvc-523-beta"></a>Бета-версия ASP.NET MVC 5.2.3

Читайте о выпуске [здесь](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Этот выпуск содержит только исправления ошибок. Можно использовать [этот запрос](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=MVC&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) для просмотра списка проблем, исправленных в этом выпуске.
