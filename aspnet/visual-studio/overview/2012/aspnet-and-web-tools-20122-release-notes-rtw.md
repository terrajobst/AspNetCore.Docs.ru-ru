---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw
title: ASP.NET и веб-инструменты 2012.2 заметки о выпуске | Документация Майкрософт
author: rick-anderson
description: Заметки о выпуске ASP.NET и веб-инструментами 2012.2.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/14/2013
ms.topic: article
ms.assetid: 9534e58b-1d15-4f1d-b04c-10c79b9d8227
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw
msc.type: content
ms.openlocfilehash: fe26f1f312415d97d71d2ab75d04877dc4a9cfbb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362402"
---
<a name="aspnet-and-web-tools-20122-release-notes"></a>ASP.NET и веб-инструменты 2012.2 заметки о выпуске
====================
> В этом документе описывается выпуск ASP.NET и веб-инструментами 2012.2. Это обновление для веб-средства Visual Studio и ASP.NET.


- [Замечания по установке](#_Installation)
- [Документация](#_Documentation)
- [Поддержка](#_Support)
- [Требования к программному обеспечению](#_Software_Requirements)
- [Новые возможности в ASP.NET и веб-инструменты 2012.2](#_New_Features_in)

    - [инструментарий](#_Tooling);
    - [Веб-публикации](#_Web_Publishing)
    - [Шаблоны ASP.NET MVC](#_Templates)
    - [ASP.NET Web API](#_ASP.NET_Web_API)

    - [ASP.NET SignalR](#_ASP.NET_SignalR)
    - [ASP.NET Friendly URLs](#_ASP.NET_Friendly_URLs)
- [Известные проблемы и критические изменения](#_Known_Issues_and)

<a id="_Installation"></a>
## <a name="installation-notes"></a>Замечания по установке

ASP.NET и веб-инструментами 2012.2 для Visual Studio 2012 можно установить с помощью [установщик веб-платформы](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2). Это обновление для Visual Studio 2012 или Visual Studio Express 2012 для Web, который обязателен. Если у вас установлена среда Visual Studio, устанавливается Visual Studio Express 2012 для Web.

Можно также установить ASP.NET и веб-инструментами 2012.2 вручную. Необходимо иметь Visual Studio 2012 или Visual Studio Express 2012 для Web установлен. Затем используйте следующие инструкции: 

1. Скачайте [ASP.NET и Web платформ 2012.2](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe) установщика из центра загрузки.
2. При запуске запроса нажмите кнопку. Можно также сохранить файл, чтобы запустить его позже.
3. Проверьте версию Visual Studio, потребуется обновить. Это можно сделать, запустив Visual Studio, вы хотите обновить. Затем щелкните пункт меню справки.   
    ![](aspnet-and-web-tools-20122-release-notes-rtw/_static/image1.jpg)
4. Если вы видите элемент меню &quot;о Microsoft Visual Studio 2012 для Web&quot; Загрузите [веб-разработчиков инструментами 2012.2 — Visual Studio Express 2012 для Web](https://go.microsoft.com/fwlink/?LinkID=282228). В противном случае загрузите [веб-разработчиков инструментами 2012.2 — Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228).
5. При запуске запроса нажмите кнопку. Можно также сохранить файл, чтобы запустить его позже.

> [!NOTE]
> ASP.NET и веб-инструментами 2012.2 выпуск не поддерживает SQL Server Data Tools. SQL Server и баз данных SQL Windows Azure предоставляет широкий набор средств, включающей разработки вне сети на основе проекта, сравнение схем и улучшенной базе данных возможности развертывания базы данных. Дополнительные сведения или установить SQL Server Data Tools можно [ https://go.microsoft.com/fwlink/?LinkID=237127 ](https://go.microsoft.com/fwlink/?LinkID=237127).

<a id="_Documentation"></a>
## <a name="documentation"></a>Документация

Учебники и другие сведения о ASP.NET и веб-инструментами 2012.2 доступны из веб-сайт ASP.NET ( https://www.asp.net).

<a id="_Support"></a>
## <a name="support"></a>Поддержка

ASP.NET и веб-инструментами 2012.2 официально выпущена и поддерживается. Можно использовать обычный поддержки канала. Вы также можете опубликовать вопросы на форумах ASP.NET ([https://forums.asp.net/](https://forums.asp.net/)), где участники сообщества ASP.NET помогают друг другу, предоставляя полезные сведения.

<a id="_Software_Requirements"></a>
## <a name="software-requirements"></a>Требования к программному обеспечению

ASP.NET и веб-инструментами 2012.2 требуется Visual Studio 2012 или Visual Studio Express 2012 для Web.

<a id="_New_Features_in"></a>
## <a name="new-features-in-aspnet-and-web-tools-20122"></a>Новые возможности в ASP.NET и веб-инструменты 2012.2

В этом разделе описываются функции, которые были введены в выпуске ASP.NET и веб-инструментами 2012.2.

<a id="_Tooling"></a>
### <a name="tooling"></a>Инструментарий

- Инспектор страниц 

    - Поддерживает сопоставление выбора JavaScript, позволяя инспектор страниц для сопоставления элементов, которые были динамически добавлены на страницу обратно на соответствующий код JavaScript.
    - Возможность просмотра обновлений CSS в режиме реального времени.
    - Дополнительные сведения в статье [Автосинхронизация CSS и JavaScript, сопоставление выбора в инспекторе страниц](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx).
- Редактор 

    - Поддерживает выделение синтаксиса для CoffeeScript, Mustache, Handlebars и JsRender.
    - Редактор HTML предоставляет поддержку технологии Intellisense для маскирования привязок.
    - МЕНЬШЕ редактирования и компилятора поддержки, чтобы включить создание динамических CSS, с помощью меньше.
    - Вставить JSON как класс .NET. Вставьте JSON в C# или VB.NET файл кода, а Visual Studio с помощью данной команды вставьте специальные автоматически создаст классы .NET, которые получены из JSON.
- Поддержка мобильного эмулятора добавляет обработчиков расширений, позволяя эмуляторы независимых производителей можно установить как VSIX. Установленных эмуляторы будут отображаться в раскрывающемся списке F5, чтобы разработчики можно предварительно просмотреть свои веб-сайты на различных мобильных устройств. Дополнительные сведения об этой функции в запись в блоге Скотта Хансельмана [новой BrowserStack интеграции с Visual Studio](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx).

<a id="_Web_Publishing"></a>
### <a name="web-publishing"></a>Веб-публикации

- Проекты веб-сайт теперь имеют те же возможности публикации проектов веб-приложений, включая публикацию веб-сайтов Azure для Windows.
- Выборочная публикация &#8211; для одного или нескольких файлов можно выполнить следующие действия (после публикации в конечную точку веб-развертывания): 

    - Опубликуйте выбранные файлы.
    - Увидеть разницу между локальным файлом и удаленный файл.
    - Обновление локального файла с удаленного файла или обновление удаленного файла с локальным файлом.

<a id="_Templates"></a>
### <a name="aspnet-mvc-templates"></a>Шаблоны ASP.NET MVC

- Новый шаблон приложения Facebook упрощает создание простых приложений Facebook Canvas. Несколько простых шагов можно создать приложение Facebook, который получает данные от пользователя, вошедшего в систему и интегрируется со своими друзьями. Шаблон включает новую библиотеку для обеспечения проверки всех предназначенную для построения приложений Facebook, включая проверку подлинности, разрешения, доступ к данным Facebook и многое другое. Дополнительные сведения об использовании шаблона приложения Facebook см. в разделе [ https://go.microsoft.com/fwlink/?LinkID=269921 ](https://go.microsoft.com/fwlink/?LinkID=269921).
- Новый шаблон одностраничного приложения MVC разработчики могут создавать интерактивные клиентские веб-приложения с использованием HTML 5, CSS 3 и популярных Knockout и jQuery библиотеки JavaScript, веб-API ASP.NET. Шаблон включает приложение списка «задач», демонстрирующее распространенные приемы для создания приложения JavaScript HTML5, в котором используется серверный API RESTful. Дополнительные сведения в [ https://www.asp.net/single-page-application ](../../../single-page-application/index.md).
- Теперь можно создать файл VSIX, который добавляет новые шаблоны в диалоговом окне Новый проект ASP.NET MVC. Узнайте подробности здесь: [https://go.microsoft.com/fwlink/?LinkId=275019](https://go.microsoft.com/fwlink/?LinkId=275019)
- Пакет FixedDisplayModes &#8211; шаблоны проектов MVC были обновлены для включения нового пакета NuGet «FixedDisplayModes», который позволит обойти ошибку в MVC 4. Дополнительные сведения о исправления, содержащиеся в пакете см. в этой записи блога ([https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)) от команды MVC.

<a id="_ASP.NET_Web_API"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

Веб-API ASP.NET были дополнены несколько новых возможностей:

- ASP.NET Web API OData
- Трассировка веб-API ASP.NET
- Страница справки веб-API ASP.NET

#### <a name="aspnet-web-api-odata"></a>ASP.NET Web API OData

OData веб-API ASP.NET обеспечивает гибкость, необходимые для создания конечных точек OData с широкими возможностями бизнес-логики для любого источника данных. С помощью ASP.NET Web API OData можно управлять объем семантику OData, который требуется предоставить. Доступна также из NuGet и входит в состав шаблонов проектов ASP.NET MVC 4 ASP.NET Web API OData ([http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata)).

ASP.NET Web API OData в настоящее время поддерживает следующие функции:

- Включите семантику запроса OData, применив атрибут [Queryable].
- Легко проверять запросы OData и ограничить набор поддерживаемых параметров запроса, операторы и функции.
- Параметр привязки к ODataQueryOptions непосредственно, для получения представления дерева абстрактного синтаксиса запроса, который затем можно проверить и применен к IEnumerable или IQueryable.
- Включите разбиение на страницы службы и связи нового поколения страницы путем задания пределов результат по атрибуту [Queryable].
- Запрос как встроенный число общее число сопоставления с ресурсами с помощью $inlinecount.
- Распространение значений null для элемента управления.
- Операторы/All в $filter.
- Определить модель EDM в соответствии с соглашением, или явно настроить модель, так же, как для Entity Framework Code-First.
- Наборы сущностей предоставляют путем наследования от EntitySetController.
- Простой, настраиваемый в отношении предоставление доступа к свойствам навигации, управления ссылками и реализации действия OData.
- Упрощенное маршрутизации с помощью метода расширения MapODataRoute.
- Поддержка управления версиями, предоставляя несколько моделей EDM.
- Предоставьте сервисного документа и $metadata, вы можете создать клиентов (.NET, Windows Phone, Windows Store, и т.д.) для веб-API.
- Поддержка форматов verbose OData Atom, JSON и JSON.
- Создание, обновление, частично обновление (PATCH) и удаление сущностей.
- Запрашивается и обрабатывается связи между сущностями.
- Создание ссылок отношений, которые привязать к маршрутов.
- Сложные типы.
- Наследование типа сущности.
- Свойства коллекции.
- Перечисления.
- Действия OData.
- Основаны на ту же основу, что службы данных WCF, а именно ODataLib ([http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata)).

Дополнительные сведения об ASP.NET Web API OData см. в разделе [ https://go.microsoft.com/fwlink/?LinkId=271141 ](https://go.microsoft.com/fwlink/?LinkId=271141).

#### <a name="aspnet-web-api-tracing"></a>Трассировка веб-API ASP.NET

Веб-API ASP.NET трассировки данные трассировки из веб-API-интерфейсы интегрируется с трассировки .NET. Теперь включен по умолчанию в шаблоне проекта веб-API. Трассировка данных для вашего веб-API-интерфейсы отправляется в окно вывода и предоставляются с помощью IntelliTrace. ASP.NET Web API трассировки позволяет отправлять данные трассировки о веб-API, если они размещены на платформе Windows Azure за счет интеграции с [диагностики Windows Azure](https://msdn.microsoft.com/library/windowsazure/hh411529.aspx). Можно также установить и включить ASP.NET Web API трассировку в любом приложении, с помощью пакета NuGet для трассировки ASP.NET Web API ([http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing)).

Дополнительные сведения о настройке и использовании ASP.NET Web API трассировки см. в разделе [ https://go.microsoft.com/fwlink/?LinkID=269874 ](https://go.microsoft.com/fwlink/?LinkID=269874).

#### <a name="aspnet-web-api-help-page"></a>Страница справки веб-API ASP.NET

ASP.NET Web страницу справки API теперь включен по умолчанию в шаблоне проекта веб-API. Веб-API справки страницы ASP.NET автоматически создает документацию для веб-API, включая конечные точки HTTP, поддерживаемых методов HTTP, параметров и пример запроса и ответа полезных данных сообщений. Документация по автоматически извлекаются из комментариев в коде. Можно также добавить страницу справки API ASP.NET Web в любое приложение ASP.NET Web API справки страницы пакета NuGet ([http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage)).

Дополнительные сведения по установке и настройке см. страницу справки ASP.NET Web API [ https://go.microsoft.com/fwlink/?LinkId=271140 ](https://go.microsoft.com/fwlink/?LinkId=271140).

<a id="_ASP.NET_SignalR"></a>
### <a name="aspnet-signalr"></a>ASP.NET SignalR

ASP.NET SignalR упрощает добавление функциональности в режиме реального времени веб-приложения ASP.NET с помощью WebSockets, если он доступен и автоматически добравшись до других методов при его исчезновении.

Дополнительные сведения об использовании ASP.NET SignalR см. в разделе [ https://go.microsoft.com/fwlink/?LinkId=271271 ](https://go.microsoft.com/fwlink/?LinkId=271271).

<a id="_ASP.NET_Friendly_URLs"></a>
### <a name="aspnet-friendly-urls"></a>ASP.NET Friendly URLs

ASP.NET FriendlyURLs упрощает для разработчиков форм для создания, очистки поиска URL-адреса (без расширения .aspx). Он немного до не требует настройки и может использоваться с существующими приложениями ASP.NET v4.0. Функция FriendlyURLs также облегчает разработчикам добавлять поддержка мобильных устройств для их приложений, поддерживая переключение между представлениями настольных и мобильных.

Дополнительные сведения об установке и использовании ASP.NET Friendly URLs см. в разделе [ http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx ](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx).

<a id="_Known_Issues_and"></a>
## <a name="known-issues-and-breaking-changes"></a>Известные проблемы и критические изменения

В этом разделе описываются известные проблемы и критические изменения, которые находятся в выпуске ASP.NET и веб-инструментами 2012.2.

### <a name="installation-issues"></a>Проблемы установки

#### <a name="out-of-order-installs-of-visual-studio-2012"></a>Неупорядоченные устанавливает Visual Studio 2012

Установка дополнительных SKU для Visual Studio 2012, после установки ASP.NET и веб-инструментами 2012.2 потребует операции восстановления. Рассмотрим следующую последовательность:

1. Установка Visual Studio 2012 Express для Web
2. Установка ASP.NET и веб-инструменты 2012.2
3. Установка Visual Studio 2012 Professional, Premium или Ultimate

Шаг 2 приведет только установка обновлений для Express для Web. Чтобы убедиться, что установлен во время выполнения шага 3 дополнительных SKU содержит обновления необходимо будет восстановить ASP.NET и веб-инструментами 2012.2, чтобы установить обновления для установки последнего номера SKU. Это также относится к номера SKU на шаге 1 и 3 в обратном порядке.

#### <a name="installing-microsoft-aspnet-and-web-tools-20122-when-visual-studio-is-open"></a>Установка Microsoft ASP.NET и веб-инструментами 2012.2, при открытии Visual Studio

Если во время установки Microsoft ASP.NET и веб-инструментами 2012.2 открыт VS, Visual Studio может оказаться в неисправном состоянии. Рекомендуется, пользователи должны закрыть все экземпляры Visual Studio перед продолжением установки.

#### <a name="canceling-aspnet-and-web-tools-20122-setup-in-the-middle-of-installation"></a>Отмена установки ASP.NET и веб-инструментами 2012.2 середине установки

Отмена ASP.NET и веб-инструментами 2012.2 установки в процессе установки будет закрыто Visual Studio в неисправном состоянии. Для решения этой проблемы выполните следующие действия: 

- Go для установки и удаления программ
- Удалите Microsoft ASP.NET и веб-инструментами 2012.2, при его наличии.
- Переустановите Microsoft ASP.NET и веб-инструменты 2012.2

#### <a name="after-uninstalling-aspnet-and-web-tools-20122-the-aspnet-mvc-4-templates-and-razor-v2-web-site-templates-are-missing"></a>После удаления ASP.NET и веб-инструментами 2012.2 ASP.NET MVC 4 отсутствуют шаблоны и шаблоны веб-узел Razor v2

При удалении ASP.NET и веб-инструментами 2012.2 будет также удаляет все ASP.NET MVC 4 и шаблоны веб-узел Razor v2 из Visual Studio 2012.

Обойти это восстановить установку Visual Studio 2012 для переустановки ASP.NET MVC 4 и шаблоны веб-узел Razor v2.

### <a name="tooling-issues"></a>Проблемы инструментов

#### <a name="nuget-error-reported-during-project-creation"></a>Ошибка NuGet во время создания проекта

После установки ASP.NET и веб-инструментами 2012.2 может появиться следующая ошибка при создании проекта MVC 4

![](aspnet-and-web-tools-20122-release-notes-rtw/_static/image1.png)

ASP.NET и веб-инструментами 2012.2 поставляется NuGet 2.1 и обновит расширения в Visual Studio 2012. В некоторых случаях установщик VSIX не удастся правильно обновить VSIX. Следующие действия пользователям для решения этой проблемы.

1. Запустите Visual Studio 2012 с правами администратора
2. Последовательно выберите пункты Сервис -&gt;расширения и обновления и удаления NuGet.
3. Закройте Visual Studio
4. Перейдите к папке установки ASP.NET и веб-инструментами 2012.2:

    1. Для Visual Studio 2012: **программы Files\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio 2012**
    2. Для Visual Studio Express 2012 для Web: **Program Files\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio Express 2012 для Web**
5. Дважды щелкните NuGet.Tools.vsix переустановить NuGet

### <a name="web-api-issues"></a>Проблемы веб-API

#### <a name="parsing-issues-in-filter-and-datetime-literals"></a>Анализ проблем в $filter и литералами даты и времени

Средство синтаксического анализа OData URI не сможет правильно проанализирована литералами частичного даты и времени. Например, $filter = начальным eq "2012-12-31T12:00" не сможет правильно проанализирована. Решение — использовать полную литерал, $filter = начальным eq "2012-12-31T12:00:00".

#### <a name="odata-doesnt-support-case-insensitive-property-names"></a>OData не поддерживает имена свойств, без учета регистра.

OData не поддерживает имена свойств, без учета регистра в запросы OData и пути odata. См. в разделе рабочие элементы:

- [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)
- [http://aspnetwebstack.codeplex.com/workitem/704](http://aspnetwebstack.codeplex.com/workitem/704)

Если пользователи используют другой регистр символов на стороне клиента javascript и на стороне сервера, они, скорее всего, возникает эта ошибка. Эта проблема присуща протокола odata. Однако многие пользователи сообщает эту проблему. Чтобы обойти ее, пользователям нужно исправить их регистр в URL-адрес.

#### <a name="default-odata-routing-conventions-doesnt-support-postput-on-navigation-property"></a>Соглашения о маршрутизации OData по умолчанию не поддерживает POST или PUT для свойства навигации.

Соглашения о маршрутизации OData по умолчанию не поддерживает POST или PUT для свойства навигации. См. в разделе рабочий элемент [ http://aspnetwebstack.codeplex.com/workitem/366 ](http://aspnetwebstack.codeplex.com/workitem/366). Мы отсутствует соглашению часто используемые в соглашения по умолчанию.

Чтобы обойти ее, пользователи должны расширить новое соглашение маршрутизации для его поддержки.

### <a name="facebook-template-issues"></a>Проблемы с шаблонами Facebook

#### <a name="facebook-application-template-only-works-using-net-45"></a>Шаблон приложения Facebook работает только с помощью .NET 4.5

.NET 4.5 необходимо выбрать в раскрывающемся списке в диалоговом окне "новый проект" для отображения шаблона приложения Facebook в ASP.NET MVC 4 framework.

#### <a name="real-time-update-controller"></a>В режиме реального времени обновить контроллер

Данный шаблон приложения Facebook позволяет пользователю легко создать контроллер Web API для обработки обновлений в реальном времени от Facebook. Если на компьютере разработки находится за NAT, контроллер может не работать без дальнейшей настройки сети. Дополнительные сведения см. здесь: [http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)

#### <a name="query-string-values-conflict-with-facebook-oauth-parameters"></a>Запрос, что строковые значения конфликтуют с параметрами Facebook OAuth

Следующие поля конфликтуют с помощью вызова диалогового окна Facebook OAuth резервного URL-адрес. Не добавляйте собственные значения строки запроса со следующими именами: код "," Ошибка "," Ошибка\_описание "," Ошибка\_причина.

#### <a name="using-page-inspector-with-facebook-template"></a>Использование инспектора страниц с помощью шаблона Facebook

Нельзя использовать функцию инспектора страниц в Visual Studio 2012 при отладке приложения Facebook. Инспектор страниц в настоящее время не поддерживает элементы IFRAME.

### <a name="single-page-application-template-issues"></a>Проблемы с шаблонами одностраничного приложения

#### <a name="with-jquery-19knockout-221-update-when-running-default-mvc-spa-project-new-todo-item-edit-enter-focus-event-is-not-handled-properly"></a>С JQuery, введите 1.9/Knockout 2.2.1 обновления, при запуске проекта MVC SPA по умолчанию, создания изменения элемента todo событие фокус не обрабатывается правильно.

С JQuery 1.9/Knockout 2.2.1 обновления, при запуске проекта MVC SPA по умолчанию, создания изменения элемента todo введите больше не фокус на поле ввода нового элемента todo после ввода нового элемента todo в список todo.

Ссылка на инструкции по решению [ http://knockoutjs.com/documentation/hasfocus-binding.html ](http://knockoutjs.com/documentation/hasfocus-binding.html)и внести аналогичные исправления в следующем примере кода:

Файл todo.model.js  
 функция todolist(data), добавьте следующие:  
 **self.isSelected = ko.observable(false);**

функция todoList.prototype.addTodo, добавьте следующий текст blacked:  
 **self.isSelected(true);**  
 self.newTodoTitle(&quot;&quot;);

Файл index.cshtml, добавьте следующий текст blacked:  
 &lt;формы привязки к данным =&quot;отправить: addTodo&quot;&gt;  
 &lt;входной класс =&quot;addTodo&quot; тип =&quot;текст&quot; привязки к данным =&quot;значение: newTodoTitle, заполнителя: «Тип здесь, чтобы добавить информацию», blurOnEnter: true, **hasfocus: isSelected**, событие: {размытия: addTodo}&quot; /&gt;  
 &lt;/form&gt;
