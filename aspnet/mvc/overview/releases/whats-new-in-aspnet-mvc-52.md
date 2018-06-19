---
uid: mvc/overview/releases/whats-new-in-aspnet-mvc-52
title: Новые возможности в ASP.NET MVC 5.2 | Документы Microsoft
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 97972587-2720-48b4-b158-f35f2e855fbf
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/whats-new-in-aspnet-mvc-52
msc.type: authoredcontent
ms.openlocfilehash: 94f6131fdb86d1694c1f563c5f6806f119c71266
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
ms.locfileid: "26504103"
---
<a name="whats-new-in-aspnet-mvc-52"></a>Новые возможности в ASP.NET MVC 5.2
====================
по [Microsoft](https://github.com/microsoft)

В этом разделе описываются новые возможности в ASP.NET MVC версии 5.2 [Microsoft.AspNet.MVC 5.2.2](#52) и [бета-версия ASP.NET MVC 5.2.3](#mvc523Beta)

- [Требования к программному обеспечению](#softRequire)
- [Скачать](#download)
- [Документация](#documentation)
- [Новые возможности в ASP.NET MVC 5.2](#new-features)

    - [Атрибут улучшения маршрутизации](#attributerouting)
- [Известные проблемы и критические изменения](#knownbreakingchanges)
- [Исправления ошибок](#bug-fixes)
- [5.2.2 Microsoft.AspNet.MVC](#52)

<a id="softRequire"></a>
## <a name="software-requirements"></a>Требования к программному обеспечению

- Visual Studio 2012: Загрузите [ASP.NET и веб-средств 2013.1 для Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).
- Visual Studio 2013: Загрузите [Visual Studio 2013 с обновлением](https://go.microsoft.com/fwlink/?LinkId=390064) или более поздней версии. Это обновление требуется для изменения представления Razor ASP.NET MVC 5.2.

<a id="download"></a>
## <a name="download"></a>Скачать

Компоненты среды выполнения выпущены в виде пакетов NuGet при галереи NuGet. Выполните все пакеты среды выполнения [семантического управления версиями](http://semver.org/) спецификации. Последнюю версию пакета ASP.NET MVC 5.2 имеет следующую версию: «5.2.0». Можно установить или обновить эти пакеты через [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/). Этот выпуск также включает соответствующие локализованные пакеты в NuGet.

Можно установить или обновить выпущенных пакетов NuGet, используя консоль диспетчера пакетов NuGet:

Установка пакета Microsoft.AspNet.Mvc-версия 5.2.0

<a id="documentation"></a>
## <a name="documentation"></a>Документация

Учебники и другие сведения о ASP.NET MVC 5.2 доступны из веб-сайт ASP.NET ([https://www.asp.net/mvc](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-52"></a>Новые возможности в ASP.NET MVC 5.2

<a id="attributerouting"></a>
### <a name="attribute-routing-improvements"></a>Атрибут улучшения маршрутизации

Атрибут маршрутизации теперь предоставляет точку расширения, вызывается IDirectRouteProvider, что позволяет полностью контролировать способа обнаружения и настройки маршрутов с атрибутами. IDirectRouteProvider отвечает за предоставление списка контроллеров вместе с информацией о связанного маршрута для указания точно конфигурации маршрутизации, которые является полезными для тех действий, и действия. Реализация IDirectRouteProvider может быть указан при вызове MapAttributes/MapHttpAttributeRoutes.

Настройка IDirectRouteProvider будет простым, расширяя реализацию по умолчанию DefaultDirectRouteProvider. Этот класс предоставляет отдельный overridable виртуальные методы для изменения логики для обнаружения атрибутов, создание записи маршрута и обнаружение префикс маршрута и префикс области.

Новый атрибут маршрутизации расширяемость IDirectRouteProvider пользователь может выполнять следующее:

1. Поддерживает наследование атрибута маршрутов. Например в следующем сценарии контроллеры блога и хранилища используется соглашение о маршрут атрибута, которое определяется BaseController. 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample1.cs)]
2. Автоматически создать имена маршрутов для маршрутов с атрибутами. 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample2.cs)]
3. Измените префиксов маршрутов в одном централизованном месте, прежде чем маршруты добавляются в таблицу маршрутизации.
4. Фильтрация контроллеров, для которых нужно маршрутизации атрибута для поиска. Мы надеемся, что в блоге на 3 и 4 первой.

### <a name="facebook-fixes-for-changed-api-surface"></a>Facebook исправления для измененных область API

Пакет MVC Facebook [было прервано](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=Facebook&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=All) из-за несколько API изменения, сделанные Facebook. Мы также реализуем новый пакет Facebook (Microsoft.AspNet.Facebook 1.0.0) для устранения этих проблем.

<a id="knownbreakingchanges"></a>
## <a name="known-issues-and-breaking-changes"></a>Известные проблемы и критические изменения

### <a name="scaffolding-mvcweb-api-into-a-project-with-520-packages-results-in-512-packages-for-ones-that-dont-already-exist-in-the-project"></a>API MVC или веб-формирования шаблонов в проект с 5.2.0 результаты пакеты в 5.1.2 пакеты для те, которые уже не существуют в проекте

Обновление пакетов NuGet для ASP.NET MVC 5.2.0 не обновляет средства Visual Studio, такие как формирование шаблонов ASP.NET или шаблон проекта веб-приложения ASP.NET. Они используют предыдущую версию пакета среды выполнения ASP.NET (например 5.1.2 в обновлении 2). В результате формирование шаблонов ASP.NET установит предыдущую версию (например 5.1.2 в обновлении 2) необходимые пакеты, если они отсутствуют доступные в проектах. Формирование шаблонов ASP.NET в Visual Studio 2013 RTM или обновлением 1 не перезаписывает последние пакеты в проекте. Если вы используете формирование шаблонов ASP.NET после обновления пакеты проектов Web API 2.2 или ASP.NET MVC 5.2, убедитесь, что версии веб-API и ASP.NET MVC согласованы.

### <a name="microsoftjqueryunobtrusivevalidation-nuget-package-installation-fails-because-it-is-unable-to-find-a-version-of-microsoftjqueryunobtrusivevalidation-compatible-to-jquery-141"></a>Установка пакета Microsoft.jQuery.Unobtrusive.Validation NuGet завершается ошибкой, так как не удается найти версию Microsoft.jQuery.Unobtrusive.Validation совместимость с jQuery 1.4.1

JQuery требуется Microsoft.jQuery.Unobtrusive.Validation &gt;= 1.8 и jQuery.Validation &gt;= 1.8. But,jQuery.Validation (1.8) должен jQuery (&#8805; 1.3.2 &amp; &amp; &#8804; 1.6). По этой причине при установке NuGet JQuery 1.8 и jQuery.Validation 1.8 одновременно, происходит сбой. Если вы видите эту проблему, можно просто обновить версию jQuery.Validation для &gt; =  [1.8.0.1](https://www.nuget.org/packages/jQuery.Validation/1.8.0.1) содержит ограничение jQuery фиксированной во-первых, можно для установки Microsoft.jQuery.Unobtrusive.Validation.

### <a name="the-jqueryvalidation-nuget-package-version-1130-does-not-recognize-some-international-email-addresses"></a>Jquery. Версия пакета nuget проверки 1.13.0 не распознает некоторые адреса электронной почты международные

версия пакета nuget jQuery.Validation 1.11.1 является последнюю известную версию, который распознает следующие допустимых адресов электронной почты. Все более поздней версии может быть не может распознать их. Пример:

Стандарта интернационализации адресов (EAI) электронной почты (например, [&#29992; &#25143;@domain.com ](mailto:&#29992;&#25143;@domain.com))   
 EAI + международное идентификаторов ресурсов (IRI) (например., [&#29992; &#25143; @&#1076; &#1086; &#1084; &#1077; &#1085;. &#1088; &#1092; ](mailto:&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;))

Проблема формируется на [https://github.com/jzaefferer/jquery-validation/issues/1222](https://github.com/jzaefferer/jquery-validation/issues/1222)

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Выделение синтаксиса для представлений Razor в Visual Studio 2013

При обновлении ASP.NET MVC 5.2 без обновления Visual Studio 2013 не будет Поддержка редактора Visual Studio для выделения синтаксиса при изменении представлений Razor. Будет необходимо обновить Visual Studio 2013 для получения этой поддержки.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Исправления и обновления вспомогательных компонентов

Этот выпуск включает несколько исправлений ошибок, а также дополнительный компонент обновления. Можно найти полный список здесь:

- [5.2 пакета](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<a id="52"></a>
## <a name="microsoftaspnetmvc-522"></a>5.2.2 Microsoft.AspNet.MVC

Этот выпуск не имеет все новые функции и исправления ошибок в MVC. Мы внесли [изменения в веб-страницах](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) для значительного увеличения производительности и впоследствии обновить все зависимые пакеты, мы владельцем зависят от этой новой версии веб-страниц.

<a id="mvc523Beta"></a>
## <a name="aspnet-mvc-523-beta"></a>Бета-версия ASP.NET MVC 5.2.3

Можно прочитать о выпуске [здесь](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Этот выпуск содержит только исправления ошибок. Можно использовать [этот запрос](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=MVC&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) Чтобы просмотреть список проблем, исправленных в этом выпуске.
