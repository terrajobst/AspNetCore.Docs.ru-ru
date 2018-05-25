---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: Веб-страницы ASP.NET (Razor) часто задаваемые вопросы | Документы Microsoft
author: tfitzmac
description: В этой статье перечислены некоторые часто задаваемые вопросы о веб-страниц ASP.NET (Razor) и WebMatrix. Версии программного обеспечения, используемых в учебника по ASP.NET Web Pages (р...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: 60cc4ca364923cb131d5e91cd7b6307b1e68644b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-pages-razor-faq"></a>Веб-страницы ASP.NET (Razor) часто задаваемые вопросы
====================
по [Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > WebMatrix не рекомендуется использовать в интегрированной среде разработки для веб-страниц ASP.NET. Используйте [Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio) или [кода Visual Studio](https://code.visualstudio.com/).
>
> В этой статье перечислены некоторые часто задаваемые вопросы о веб-страниц ASP.NET (Razor) и WebMatrix.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемая в этом учебнике
> 
> 
> - Веб-страниц ASP.NET (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>   
> 
> Этот учебник также работает с веб-страницы ASP.NET 2, WebMatrix 2 и Visual Studio 2012.


- [Какова разница между веб-страниц ASP.NET, веб-форм ASP.NET и ASP.NET MVC?](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [Зачем мне WebMatrix для работы с веб-страницы?](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [Можно использовать элементы управления веб-форм ASP.NET на веб-страницы](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [Можно развернуть узел веб-страниц ASP.NET без использования WebMatrix](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [Нужно ли использовать вспомогательное приложение WebSecurity для поддержки имен входа?](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [Поддерживает ли веб-страниц ASP.NET HTML5?](#Does_ASP.NET_Web_Pages_support_HTML5)
- [Можно ли использовать JavaScript и jQuery с веб-страницы?](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [Дополнительные ресурсы](#AdditionalResources)

Ответы на вопросы об ошибках и других проблем см [руководство по устранению неполадок ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001).

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a>Какова разница между веб-страниц ASP.NET, веб-форм ASP.NET и ASP.NET MVC?

Все три условия технологии ASP.NET для создания динамических веб-приложений:

- Веб-страницы ASP.NET основное внимание уделяется Добавление HTML-страниц и простой и упрощенный синтаксис функции динамического кода (на стороне сервера) и доступ к базе данных.
- Веб-форм ASP.NET основана на модели объектов страницы и традиционные тип окна элементы управления (кнопок, списков, и т. д.). Web Forms использует модель на основе событий, который хорошо знаком тем, которые работали с разработки на базе клиента (Windows forms).
- ASP.NET MVC в ASP.NET реализует шаблон model-view-controller для ASP.NET. Основное внимание уделяется «Разделение областей ответственности» (обработки данных и слои пользовательского интерфейса).

Все три платформы полностью поддерживаются и продолжить разработку группой разработки ASP.NET. Как правило, выбор Framework, используйте зависит от фона и опыт работы с ASP.NET.

В частности веб-страницы ASP.NET была разработана для упрощения для тех, кто уже знаком HTML Добавление страницы их обработку на сервере. Это хороший вариант для учащихся любителей, обычно пользователи, не знакомы с программированием. Он также может быть хорошим выбором для разработчиков, имеющих опыт работы с не ASP.NET веб-технологий.

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a>Зачем мне WebMatrix для работы с веб-страницы?

Нет. WebMatrix не рекомендуется использовать в интегрированной среде разработки для веб-страниц ASP.NET. Используйте [Visual Studio](program-asp-net-web-pages-in-visual-studio.md) или [кода Visual Studio](https://code.visualstudio.com/).

Если вы не хотите использовать Visual Studio или кода Visual Studio, можно установить компонент продукты по отдельности с помощью [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx). Необходимы следующие продукты:

- Microsoft .NET Framework 4.5
- ASP.NET MVC 5 (которая устанавливает платформу веб-страниц ASP.NET)
- IIS Express (веб-сервер)
- Microsoft SQL Server Compact 4.0 (база данных)

Можно использовать текстовый редактор для изменения *.cshtml* (или *.vbhtml*) страниц.

Управление базами данных SQL Server Compact (*.sdf* файлов) без средство немного сложнее. Containds средств Visual Studio для управления *.sdf* баз данных. Можно также выполнять команды SQL в коде для выполнения многих задач управления SQL Server.

Чтобы проверить *.cshtml* страниц без использования интегрированной среды разработки (IDE), можно развернуть их к активному серверу. (См. [можно выполнить установку на сайт веб-страниц ASP.NET без использования WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))

### <a name="running-iis-express-without-using-an-ide"></a>Под управлением IIS Express без использования интегрированной среды разработки

При установке на компьютере, что веб-сервер IIS Express, можно использовать для проверки страниц. Можно запустить из командной строки IIS Express и связать его с конкретный номер порта. Затем указать этот порт при запросе *.cshtml* файлы в браузере.

В Windows, откройте командную строку с правами администратора и измените на *C:\Program Files\IIS Express.* (Для 64-разрядных системах папку использовать *Express \IIS C:\Program Files (x86).)* Затем введите следующую команду, используя фактический путь к узлу:

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

Можно использовать любой номер порта, который уже не защищены другим процессом. (Номера портов выше 1024 обычно свободны.) Для `path` , используйте путь к папке веб-сайта где *.cshtml* файлов.

После запуска эту команду, чтобы настроить IIS Express для обслуживания страниц можно откройте браузер и перейдите к *.cshtml* файла. Используйте URL-адреса следующим образом:

`http://localhost:35896/default.cshtml`

Для получения справки по параметрам командной строки IIS Express, введите `iisexpress.exe /?` из командной строки.

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a>Можно использовать элементы управления веб-форм ASP.NET на веб-страницы

Нет. Элементы управления Web Forms как [флажок](https://msdn.microsoft.com/library/system.web.ui.webcontrols.checkbox) управления [проверяющие элементы управления](https://msdn.microsoft.com/library/bwd43d0x)и [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview) управления только для работы на страницах Web Forms (*.aspx* файлов). Эти элементы управления требуют страницы веб-форм.

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a>Можно развернуть узел веб-страниц ASP.NET без использования WebMatrix

Да. Вы можете вручную скопировать файлы веб-сайта на сервере (обычно по протоколу FTP). При выполнении ручного копирования, также необходимо скопировать файлы, которые поддерживают SQL Server Compact (база данных). Дополнительные сведения см. в записи блога [развертывание веб-страниц приложений без средство](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317).

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a>Нужно ли использовать вспомогательное приложение WebSecurity для поддержки имен входа?

Нет. `SimpleMembership` Поставщика, который входит в состав веб-страниц ASP.NET — один из вариантов. Также доступны поставщики безопасности, которые являются частью ASP.NET (который вы может использоваться для работы с веб-форм). Например можно использовать проверку подлинности форм в ASP.NET Web Pages так же, как и в веб-форм. Для проверки подлинности форм одним из примеров использования, см. в статье технической поддержки Майкрософт [How To Implement Forms-Based проверки подлинности в приложении ASP.NET с помощью C# .NET](https://support.microsoft.com/kb/301240). Загрузить пример [версии ASP.NET «входа &amp; пароль](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm).

Сведения о том, как использовать проверку подлинности Windows, см. в записи блога [проверки подлинности Windows с помощью веб-страницах ASP.NET](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298).

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a>Поддерживает ли веб-страниц ASP.NET HTML5?

Да. Страницы, создание с веб-страниц ASP.NET (*.cshtml* или *.vbhtml* страниц), по существу HTML-страницы, содержащие код, выполняемый на сервере, перед отображением страницы. При условии, что браузер пользователя поддерживает HTML5, можно использовать элементы HTML5 в *.cshtml* или *.vbhtml* страницы.

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a>Можно ли использовать JavaScript и jQuery с веб-страницы?

Конечно. Страницы, создание с веб-страниц ASP.NET (*.cshtml* или *.vbhtml* страниц), просто HTML-страницы с использованием серверного кода в них. Таким образом, что-либо обычный HTML-странице, можно сделать с помощью JavaScript и jQuery можно также сделать в *.cshtml* или *.vbhtml* страницы.

**Начального сайта** шаблона в WebMatrix содержит ряд библиотеки jQuery. Если создать сайт с помощью этого шаблона, *сценариев* папка содержит библиотеку jQuery core (*jquery 1.6.2.js)* и библиотеки для проверки jQuery (*jquery.validate.js*и т. д.).

Ниже приведены некоторые записи в блогах, демонстрирующие способы использования jQuery с веб-страниц ASP.NET.

- [Добавление в веб-страниц ASP.NET с помощью WebMatrix jQuery богу](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/) по Рэйчел Аппель
- [5 минут: WebMatrix + jQuery UI + json + jQuery шаблоны](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/) по Jonas Eriksson
- [WebMatrix и формы jQuery](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms) , Майк Бринд

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Дополнительные ресурсы


[Руководство по устранению неполадок веб-страниц ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)

[Форум по ASP.NET Web Pages и WebMatrix](https://forums.asp.net/1224.aspx/1?WebMatrix) веб-сайта ASP.NET
