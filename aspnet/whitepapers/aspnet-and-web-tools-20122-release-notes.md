---
uid: whitepapers/aspnet-and-web-tools-20122-release-notes
title: "ASP.NET и веб-средств 2012.2 заметки о выпуске | Документы Microsoft"
author: rick-anderson
description: "Заметки о выпуске для ASP.NET и 2012.2 средства Web."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/14/2013
ms.topic: article
ms.assetid: bdb18d02-9f61-4676-836d-6fdea94f9282
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-and-web-tools-20122-release-notes
msc.type: content
ms.openlocfilehash: 52559a47f86e572f873d4eaaab50e87eb51722fd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-and-web-tools-20122-release-notes"></a>ASP.NET и веб-инструменты 2012.2 заметки о выпуске
====================
> В этом документе описывается выпуск ASP.NET и 2012.2 средства Web. Это обновление для веб-средства Visual Studio и ASP.NET.


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

ASP.NET и 2012.2 Web Tools для Visual Studio 2012 можно установить с помощью [установщика веб-платформы](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2). Это обновление для Visual Studio 2012 или Visual Studio Express 2012 для Web, который является обязательным. Если у вас установлено приложение Visual Studio, будет установлен Visual Studio Express 2012 для Web.

Можно также установить ASP.NET и 2012.2 средства Web вручную. Необходимо иметь Visual Studio 2012 или Visual Studio Express 2012 для установки веб-узла. Затем выполните следующие действия: 

1. Загрузить [ASP.NET и платформы 2012.2 Web](https://download.microsoft.com/download/6/5/6/6562AFBE-9503-4E64-970C-1427133FCD73/AspNetWebTools2012Setup.exe) программы установки из центра загрузки Майкрософт.
2. При запуске запроса нажмите кнопку. Кроме того, можно сохранить файл для последующего запуска.
3. Проверьте версию Visual Studio, потребуется обновить. Это можно сделать, запустив Visual Studio, вы хотите обновить. Выберите меню «Справка».   
    ![](aspnet-and-web-tools-20122-release-notes/_static/image1.jpg)
4. Если пункт меню отображается &quot;о Microsoft Visual Studio 2012 для Web&quot; Загрузите [2012.2 средств разработчика веб - Visual Studio Express 2012 для Web](https://go.microsoft.com/fwlink/?LinkID=282228). В противном случае загрузите [2012.2 средств разработчика веб - Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228).
5. При запуске запроса нажмите кнопку. Кроме того, можно сохранить файл для последующего запуска.

> [!NOTE]
> ASP.NET и 2012.2 средства Web выпуска не включает SQL Server Data Tools. SQL Server и баз данных SQL Windows Azure предоставляет богатый набор средств, включая разработки резервного проекта вне сети, сравнение схем и возможности развертывания расширенные базы данных базы данных. Дополнительные сведения или установить SQL Server Data Tools можно [https://go.microsoft.com/fwlink/?LinkID=237127](https://go.microsoft.com/fwlink/?LinkID=237127).

<a id="_Documentation"></a>
## <a name="documentation"></a>Документация

Учебники и другие сведения о ASP.NET и Web 2012.2 средства доступны из веб-сайт ASP.NET (https://www.asp.net).

<a id="_Support"></a>
## <a name="support"></a>Поддержка

ASP.NET Web Tools 2012.2 официально выпущена и поддерживается. Можно использовать обычные поддержки канала. Вы можете опубликовать вопросы на форумах ASP.NET ([https://forums.asp.net/](https://forums.asp.net/)), где члены сообщества ASP.NET друг другу, предоставляя полезные сведения.

<a id="_Software_Requirements"></a>
## <a name="software-requirements"></a>Требования к программному обеспечению

ASP.NET и 2012.2 средства Web требуется Visual Studio 2012 или Visual Studio Express 2012 для Web.

<a id="_New_Features_in"></a>
## <a name="new-features-in-aspnet-and-web-tools-20122"></a>Новые возможности в ASP.NET и веб-инструменты 2012.2

В этом разделе описаны функции, которые были введены в версии ASP.NET и 2012.2 средства Web.

<a id="_Tooling"></a>
### <a name="tooling"></a>Инструментарий

- Инспектор страниц 

    - Поддерживает сопоставление JavaScript выделение, позволяя инспектор страниц для сопоставления элементов, добавленных на страницу обратно в соответствующий код JavaScript динамически.
    - Возможность просмотреть обновления CSS в режиме реального времени.
    - Дополнительные сведения см. в статье [Автосинхронизации CSS и JavaScript Выбор сопоставления в инспекторе страниц](https://blogs.msdn.com/b/webdev/archive/2012/12/14/css-auto-sync-and-javascript-selection-mapping-in-page-inspector.aspx).
- Редактор 

    - Поддерживает подсветку синтаксиса для CoffeeScript, Mustache, рули и JsRender.
    - Редактор HTML предоставляет Intellisense для маскирования привязок.
    - МЕНЬШЕ редактирования и компилятор поддерживает включение построения динамических CSS, с помощью меньше.
    - Вставить JSON как класс .NET. Вставить JSON в C# или VB.NET файл кода, а Visual Studio с помощью данной команды специальные вставьте автоматически создаст классы .NET, получены из JSON.
- Поддержка мобильных эмуляторов добавляет обработчиков расширений, чтобы эмуляторы сторонних разработчиков, которые можно установить как расширение VSIX. Установленные эмуляторы будут отображаться в раскрывающемся списке F5, чтобы разработчики можно выполнить предварительный просмотр своих веб-сайтов на различных мобильных устройств. Дополнительные сведения о эту функцию в запись в блоге Скотт Хансельман [новый BrowserStack интеграцию с Visual Studio](http://www.hanselman.com/blog/CrossBrowserDebuggingIntegratedIntoVisualStudioWithBrowserStack.aspx).

<a id="_Web_Publishing"></a>
### <a name="web-publishing"></a>Веб-публикации

- Проекты веб-сайта теперь имеют те же возможности публикации, что проектов веб-приложений, включая публикацию веб-сайтов Azure для Windows.
- Функция выборочной публикации &#8211; для одного или нескольких файлов (после публикации в конечную точку веб-развертывания) можно выполнить следующие действия: 

    - Опубликуйте выбранные файлы.
    - См. в отличие от локального файла удаленного файла.
    - Обновить локальный файл с удаленного файла или обновление удаленного файла с локального файла.

<a id="_Templates"></a>
### <a name="aspnet-mvc-templates"></a>Шаблоны ASP.NET MVC

- Новый шаблон приложения Facebook делает создание простой приложений Facebook Canvas. Несколько простых шагов можно создать приложение Facebook, который получает данные от пользователя, вошедшего в систему и интегрируется с друзьями. Шаблон включает новую библиотеку для обеспечения проверки всех по проектированию приложений Facebook, включая проверку подлинности, разрешения, доступ к данным Facebook и многое другое. Дополнительные сведения об использовании шаблона приложения Facebook. в разделе [https://go.microsoft.com/fwlink/?LinkID=269921](https://go.microsoft.com/fwlink/?LinkID=269921).
- Новый шаблон приложения MVC разработчики могут создавать интерактивные клиентские веб-приложения с использованием HTML 5, CSS 3 популярных маскирования и библиотеки jQuery JavaScript, веб-API ASP.NET. Шаблон включает список приложение «todo», демонстрирующее распространенные приемы создания приложения JavaScript HTML5, использующий серверный API RESTful. Дополнительные сведения по [https://www.asp.net/single-page-application](../single-page-application/index.md).
- Теперь можно создать файл VSIX, который добавляет новые шаблоны диалоговое окно нового проекта ASP.NET MVC. Узнайте, как здесь: [https://go.microsoft.com/fwlink/?LinkId=275019](https://go.microsoft.com/fwlink/?LinkId=275019)
- Пакет FixedDisplayModes &#8211; Шаблоны проектов MVC были обновлены для включения новый пакет NuGet «FixedDisplayModes», который содержит обходной путь для ошибок в MVC 4. Дополнительные сведения о исправления, содержащиеся в пакете см. в этой записи блога ([https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)) от службы технической поддержки MVC.

<a id="_ASP.NET_Web_API"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

Веб-API ASP.NET были добавлены несколько новых возможностей:

- OData веб-API ASP.NET
- Трассировка ASP.NET Web API
- Страница справки веб-API ASP.NET

#### <a name="aspnet-web-api-odata"></a>OData веб-API ASP.NET

OData веб-API ASP.NET обеспечивает гибкость, необходимо создать конечные точки OData с форматированным бизнес-логики для любого источника данных. С помощью ASP.NET Web API OData управлять объемом семантику OData, которому требуется предоставить доступ. Входит в состав шаблонов проектов ASP.NET MVC 4 ASP.NET Web API OData, а также доступна из NuGet ([http://www.nuget.org/packages/microsoft.aspnet.webapi.odata](http://www.nuget.org/packages/microsoft.aspnet.webapi.odata)).

ASP.NET Web API OData в настоящее время поддерживает следующие функции:

- Включите семантику запроса OData, применив атрибут [Queryable].
- Простой способ проверки запросов OData и ограничить набор поддерживаемых параметров запроса, операторы и функции.
- Привязка параметра к ODataQueryOptions непосредственно, для получения представления дерева абстрактного синтаксиса запроса, который затем можно проверить и применен к устройству IQueryable или IEnumerable.
- Включите разбиение на страницы службы и связи нового поколения страницы путем ограничения результатов для атрибута [Queryable].
- Запрос встроенных количество общее число сопоставления ресурсов с помощью $inlinecount.
- Распространение значения null для элемента управления.
- Операторы/All в $filter.
- Определить модель EDM с соглашением, или явно настроить модель, так же, как для Entity Framework сначала код.
- Наборы сущностей предъявляет путем наследования от EntitySetController.
- Простой, настраиваемый соглашения для предоставления доступа к свойствам навигации, управления ссылками и реализации действия OData.
- Упрощенное Маршрутизация с помощью метода расширения MapODataRoute.
- Поддержка управления версиями, предоставляя несколько моделей EDM.
- Предоставьте сервисного документа и $metadata, можно создать клиентов (.NET, Windows Phone, магазина Windows, т. д.) для веб-API.
- Поддержка OData Atom, JSON и JSON verbose форматов.
- Создание, обновление, частично обновление (PATCH) и удаление сущностей.
- Запросы и управлять ими связей между сущностями.
- Создание ссылок на связи, привязать к вашей маршрутов.
- Сложные типы.
- Наследование типа сущности.
- Свойства коллекции.
- Перечислимые типы.
- Действия OData.
- Построено тому же принципу, как службы данных WCF, а именно ODataLib ([http://www.nuget.org/packages/microsoft.data.odata](http://www.nuget.org/packages/microsoft.data.odata)).

Дополнительные сведения о ASP.NET Web API OData. в разделе [https://go.microsoft.com/fwlink/?LinkId=271141](https://go.microsoft.com/fwlink/?LinkId=271141).

#### <a name="aspnet-web-api-tracing"></a>Трассировка ASP.NET Web API

Веб-API ASP.NET трассировки данные трассировки из веб-API-интерфейсы интегрируется с трассировку .NET. Теперь включен по умолчанию в шаблоне проекта веб-API. Трассировка запросов для вашего веб-API отправляется в окно вывода и становятся доступными через IntelliTrace. ASP.NET Web API Tracing позволяет трассировке сведения о веб-API при размещении в Windows Azure через интеграцию с [диагностики Windows Azure](https://msdn.microsoft.com/library/windowsazure/hh411529.aspx). Можно также установить и включить в любом приложении, с помощью пакета NuGet для трассировки ASP.NET Web API ASP.NET Web API Tracing ([http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing](http://www.nuget.org/packages/microsoft.aspnet.webapi.tracing)).

Дополнительные сведения о настройке и использовании ASP.NET Web API Tracing разделе [https://go.microsoft.com/fwlink/?LinkID=269874](https://go.microsoft.com/fwlink/?LinkID=269874).

#### <a name="aspnet-web-api-help-page"></a>Страница справки веб-API ASP.NET

Страница справки API ASP.NET Web теперь включено по умолчанию в шаблоне проекта веб-API. Страница справки API ASP.NET Web автоматически создает документацию для веб-API, включая конечные точки HTTP, поддерживаемых методов HTTP, параметров и пример запроса и ответного сообщения полезных данных. Документация автоматически извлекается из комментариев в код. ASP.NET Web API страница справки также можно добавить к любому приложению, используя ASP.NET Web API справки страницы пакета NuGet ([http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage](http://www.nuget.org/packages/microsoft.aspnet.webapi.helppage)).

Дополнительные сведения о настройке и настройка веб-API справки страницы ASP.NET см. [https://go.microsoft.com/fwlink/?LinkId=271140](https://go.microsoft.com/fwlink/?LinkId=271140).

<a id="_ASP.NET_SignalR"></a>
### <a name="aspnet-signalr"></a>ASP.NET SignalR

ASP.NET SignalR упрощает добавление функциональности в реальном времени веб-приложению ASP.NET, с помощью WebSockets, если он доступен и автоматически падающий с другими методами, если это не так.

Дополнительные сведения об использовании ASP.NET SignalR разделе [https://go.microsoft.com/fwlink/?LinkId=271271](https://go.microsoft.com/fwlink/?LinkId=271271).

<a id="_ASP.NET_Friendly_URLs"></a>
### <a name="aspnet-friendly-urls"></a>ASP.NET Friendly URLs

ASP.NET FriendlyURLs позволяет очень легко для веб-форм разработчиков для создания очистки поиска URL-адреса (без расширения .aspx). Он практически к не требует настройки и может использоваться с существующими приложениями ASP.NET v4.0. Функция FriendlyURLs также упрощает разработчикам добавлять поддержка мобильных устройств к приложениям, поддерживая переключение между представлениями для настольных компьютеров и мобильных устройств.

Дополнительные сведения об установке и использовании ASP.NET Friendly URLs разделе [http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx).

<a id="_Known_Issues_and"></a>
## <a name="known-issues-and-breaking-changes"></a>Известные проблемы и критические изменения

В этом разделе описываются известные проблемы и критические изменения в выпуске ASP.NET и 2012.2 средства Web.

### <a name="installation-issues"></a>Проблемы установки

#### <a name="out-of-order-installs-of-visual-studio-2012"></a>По порядку установки Visual Studio 2012

Установка дополнительных SKU для Visual Studio 2012, после установки ASP.NET и 2012.2 средства Web потребуется операции восстановления. Рассмотрим следующую последовательность:

1. Установка Visual Studio 2012 Express для Web
2. Установка ASP.NET и веб-инструменты 2012.2
3. Установка Visual Studio 2012 Professional, Premium или Ultimate

Шаг 2 только приведет к установке обновлений для Express для Web. Убедитесь, что установлен во время выполнения шага 3 дополнительных SKU содержит обновления потребуется исправление ASP.NET и 2012.2 средства Web для установки обновлений для установки последнего SKU. Это также применимо, если меняется SKU на шаге 1 и 3.

#### <a name="installing-microsoft-aspnet-and-web-tools-20122-when-visual-studio-is-open"></a>Установка Microsoft ASP.NET и 2012.2 средства Web, при открытии Visual Studio

При VS открыта во время установки Microsoft ASP.NET и 2012.2 средства Web, Visual Studio может оказаться в неработоспособном состоянии. Рекомендуется, пользователи должны закрыть все экземпляры Visual Studio перед продолжением установки.

#### <a name="canceling-aspnet-and-web-tools-20122-setup-in-the-middle-of-installation"></a>Отмена установки ASP.NET и 2012.2 средства Web во время установки

Отмена ASP.NET и 2012.2 средства Web установки во время установки будет закрыто Visual Studio в неверном состоянии. Для устранения этой проблемы выполните следующие действия: 

- Нажмите Установка и удаление программ
- Удалите Microsoft ASP.NET и 2012.2 средства Web, при его наличии.
- Переустановите Microsoft ASP.NET и веб-инструменты 2012.2

#### <a name="after-uninstalling-aspnet-and-web-tools-20122-the-aspnet-mvc-4-templates-and-razor-v2-web-site-templates-are-missing"></a>После удаления ASP.NET и веб-2012.2 средств ASP.NET MVC 4 отсутствуют шаблоны и Razor v2 веб-сайта

При удалении ASP.NET и 2012.2 средства веб все ASP.NET MVC 4 и шаблоны веб-сайтов Razor v2 будет также удален из Visual Studio 2012.

Это решение подходит для исправления установки Visual Studio 2012 для переустановки ASP.NET MVC 4 и шаблоны веб-сайтов Razor v2.

### <a name="tooling-issues"></a>Вопросы работы с проектами

#### <a name="nuget-error-reported-during-project-creation"></a>NuGet ошибки, обнаруженной во время создания проекта

После установки ASP.NET и 2012.2 средства Web может появиться следующая ошибка при создании проекта MVC 4

![](aspnet-and-web-tools-20122-release-notes/_static/image1.png)

ASP.NET и 2012.2 средства Web поставляется NuGet 2.1 и будет выполнено обновление расширения в Visual Studio 2012. В некоторых случаях не удастся правильно обновить VSIX установщик VSIX. Следующие шаги позволит вам для решения этой проблемы:

1. Запустите Visual Studio 2012 с правами администратора
2. Последовательно выберите пункты Сервис -&gt;расширения и обновления и удаления в NuGet.
3. Закройте Visual Studio
4. Перейдите к папке установки ASP.NET и 2012.2 средства Web:

    1. Для Visual Studio 2012: **программы Files\Microsoft ASP.NET\ASP.NET веб-Stack\Visual Studio 2012**
    2. Для Visual Studio Express 2012 для Web: **Program Files\Microsoft ASP.NET\ASP.NET Web Stack\Visual Studio Express 2012 для Web**
5. Дважды щелкните NuGet.Tools.vsix для переустановки NuGet

### <a name="web-api-issues"></a>Вопросы веб-API

#### <a name="parsing-issues-in-filter-and-datetime-literals"></a>Анализ проблем в $filter и литералы даты и времени

Средство синтаксического анализа OData URI не удается выполнить анализ литералами даты и времени частичного должным образом. Например, $filter = начальным eq "2012-12-31T12:00" не удается правильно проанализирована. Обходной путь заключается в использовании полного литерала $filter = начальным eq "2012-12-31T12:00:00".

#### <a name="odata-doesnt-support-case-insensitive-property-names"></a>OData не поддерживает имена свойств, без учета регистра.

OData не поддерживает имена свойств, без учета регистра в запросах OData и пути odata. Рабочие элементы в разделе:

- [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366)
- [http://aspnetwebstack.codeplex.com/workitem/704](http://aspnetwebstack.codeplex.com/workitem/704)

Если пользователи имеют другой регистр символов в javascript на стороне клиента и на стороне сервера, они, скорее всего, эта проблема возникает. Эта проблема присуща протокол odata. Однако многие пользователи сообщает эту проблему. Чтобы обойти ее, пользователям необходимо исправить их вариантов в URL-адрес.

#### <a name="default-odata-routing-conventions-doesnt-support-postput-on-navigation-property"></a>Соглашения маршрутизации OData по умолчанию не поддерживает POST или PUT для свойства навигации.

Соглашения маршрутизации OData по умолчанию не поддерживает POST или PUT для свойства навигации. Рабочий элемент в разделе [http://aspnetwebstack.codeplex.com/workitem/366](http://aspnetwebstack.codeplex.com/workitem/366). Мы это часто используемые соглашение в отношении соглашений по умолчанию отсутствует.

Чтобы обойти ее, пользователям необходимо расширить новое соглашение о маршрутизации для его поддержки.

### <a name="facebook-template-issues"></a>Проблемы с шаблонами Facebook

#### <a name="facebook-application-template-only-works-using-net-45"></a>Шаблон приложения Facebook работает только с помощью .NET 4.5

.NET 4.5 необходимо выбрать в раскрывающемся списке framework в диалоговом окне "новый проект" для отображения шаблона приложения Facebook в ASP.NET MVC 4.

#### <a name="real-time-update-controller"></a>В режиме реального времени обновления контроллера

Шаблон приложения Facebook позволяет пользователям легко создать контроллер Web API для обработки обновлений в реальном времени от Facebook. Если ваш компьютер для разработки за NAT, контроллер могут работать без дальнейшей настройки сети. Дополнительные сведения см. здесь: [http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook](http://facebook.stackoverflow.com/questions/5259467/can-a-computer-behind-a-nat-router-receive-realtime-updates-from-facebook)

#### <a name="query-string-values-conflict-with-facebook-oauth-parameters"></a>Запрос, что строковые значения конфликтуют с параметрами Facebook OAuth

Следующие поля конфликтуют с вызова диалогового окна Facebook OAuth резервное URL-адрес. Не следует добавлять собственные значения строки запроса со следующими именами: код "," Ошибка "," Ошибка\_описание, ошибка\_причины.

#### <a name="using-page-inspector-with-facebook-template"></a>С помощью инспектора страниц с помощью шаблона Facebook

Нельзя использовать функцию инспектор страниц в Visual Studio 2012 при отладке приложения Facebook. Инспектор страниц в настоящее время не поддерживает рамки.

### <a name="single-page-application-template-issues"></a>Выдает шаблон одностраничного приложения

#### <a name="with-jquery-19knockout-221-update-when-running-default-mvc-spa-project-new-todo-item-edit-enter-focus-event-is-not-handled-properly"></a>С помощью JQuery, введите 1.9/маскирования 2.2.1 обновления, при запуске проекта MVC SPA по умолчанию, новые изменения элемента todo события фокуса правильно не обрабатывается.

1.9 JQuery/маскирования 2.2.1 обновлений при запуске проекта MVC SPA по умолчанию, новые изменения элемента todo укажите, больше не фокус обратно в новое поле ввода элемента todo после ввода нового элемента todo в список todo.

Для ссылки на инструкции по решению [http://knockoutjs.com/documentation/hasfocus-binding.html](http://knockoutjs.com/documentation/hasfocus-binding.html)и внести исправления примерно в следующем примере кода:

Файл todo.model.js  
 функция todolist(data), добавьте следующий:  
 **self.isSelected = ko.observable(false);**

функция todoList.prototype.addTodo, добавьте следующий текст blacked:  
 **self.isSelected(true);**  
 self.newTodoTitle(&quot;&quot;);

Файл index.cshtml, добавьте следующий текст blacked:  
 &lt;формы связывания данных =&quot;отправки: addTodo&quot;&gt;  
 &lt;ввода класса =&quot;addTodo&quot; тип =&quot;текст&quot; привязка к данным =&quot;значение: newTodoTitle, заполнитель: «Тип здесь Добавление», blurOnEnter: true, **активировансчетчик: isSelected**, события: {размытие: addTodo}&quot; /&gt;  
 &lt;/ FROM&gt;
