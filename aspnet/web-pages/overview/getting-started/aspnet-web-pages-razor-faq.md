---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: Веб-страницы ASP.NET (Razor) часто задаваемые вопросы | Документация Майкрософт
author: tfitzmac
description: В этой статье перечислены некоторые часто задаваемые вопросы о веб-страниц ASP.NET (Razor) и WebMatrix. Версии программного обеспечения, используемые в учебника по ASP.NET Web Pages (R...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: 9546a5639da6baeadf9f01dfbe7da59d3dc4d6c6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374760"
---
<a name="aspnet-web-pages-razor-faq"></a>Веб-страницы ASP.NET (Razor) часто задаваемые вопросы
====================
по [Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > WebMatrix не рекомендуется использовать как интегрированную среду разработки для веб-страниц ASP.NET. Используйте [Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio) или [Visual Studio Code](https://code.visualstudio.com/).
>
> В этой статье перечислены некоторые часто задаваемые вопросы о веб-страниц ASP.NET (Razor) и WebMatrix.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемые в этом руководстве
> 
> 
> - Веб-страниц ASP.NET (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>   
> 
> Этот учебник также работает с веб-страниц ASP.NET 2, WebMatrix 2 и Visual Studio 2012.


- [Какова разница между веб-страниц ASP.NET, веб-форм ASP.NET и ASP.NET MVC?](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [Зачем мне WebMatrix для работы с веб-страниц?](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [Можно использовать элементы управления веб-форм ASP.NET на странице веб-страниц?](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [Можно развернуть на сайте веб-страниц ASP.NET без использования WebMatrix](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [Нужно ли использовать вспомогательное приложение WebSecurity для поддержки имен входа?](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [Поддерживает ли веб-страниц ASP.NET, HTML5?](#Does_ASP.NET_Web_Pages_support_HTML5)
- [Можно ли использовать JavaScript и jQuery с веб-страниц?](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [Дополнительные ресурсы](#AdditionalResources)

Вопросы об ошибках и другие проблемы, см. в разделе [руководство по устранению неполадок ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001).

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a>Какова разница между веб-страниц ASP.NET, веб-форм ASP.NET и ASP.NET MVC?

Все три — технологии ASP.NET для создания динамических веб-приложений:

- Веб-страницы ASP.NET посвящена Добавление HTML-страниц и простая и легкая синтаксис функции динамического кода (на стороне сервера) и доступ к базе данных.
- Веб-формы ASP.NET основана на модели объекта страницы и традиционное окно элементов управления (кнопки, списки, и т.д.). Веб-форм используется модель на основе событий, знаком тем, кто работал с разработки на основе клиента (Windows forms).
- ASP.NET MVC реализует шаблон model-view-controller для ASP.NET. Внимание уделяется «Разделение областей ответственности» (обработки данных и слои пользовательского интерфейса).

Все три платформы, полностью поддерживаются и продолжить разработку группой ASP.NET. Как правило, Выбор платформы для использования зависит от вашего рода и опыт работы с ASP.NET.

В частности, веб-страницы ASP.NET была разработана для упрощения для тех, кто уже знаком с HTML, чтобы добавить сервер обработки на соответствующие страницы. Он хорошо подходит для учащихся, любителей, люди в целом, кто не знаком с программированием. Он также может быть хорошим выбором для разработчиков, имеющих опыт работы с не ASP.NET веб-технологиями.

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a>Зачем мне WebMatrix для работы с веб-страниц?

Нет. WebMatrix не рекомендуется использовать как интегрированную среду разработки для веб-страниц ASP.NET. Используйте [Visual Studio](program-asp-net-web-pages-in-visual-studio.md) или [Visual Studio Code](https://code.visualstudio.com/).

Если вы не хотите использовать Visual Studio или Visual Studio Code, вы можете установить компонент продукты по отдельности, используя [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx). Необходимы следующие продукты:

- Microsoft .NET Framework 4.5
- ASP.NET MVC 5 (который устанавливается платформа веб-страниц ASP.NET)
- IIS Express (веб-сервер)
- Microsoft SQL Server Compact 4.0 (база данных)

Можно использовать текстовый редактор для редактирования *.cshtml* (или *.vbhtml*) страниц.

Управление базами данных SQL Server Compact (*.sdf* файлов) без средство немного сложнее. Containds средств Visual Studio для управления *.sdf* баз данных. Можно также выполнять команды SQL в коде для выполнения многих задач управления SQL Server.

Для тестирования *.cshtml* страниц без использования интегрированную среду разработки (IDE), можно развернуть их к активному серверу. (См. в разделе [можно ли развернуть на сайте веб-страниц ASP.NET без использования WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))

### <a name="running-iis-express-without-using-an-ide"></a>Под управлением IIS Express без использования интегрированной среды разработки

При установке на компьютер в качестве веб-сервер IIS Express, можно использовать для тестирования на страницах. Можно запустить из командной строки IIS Express и связать его с другой номер порта. Затем указать этот порт при запросе *.cshtml* файлы в браузере.

В Windows, откройте командную строку с правами администратора и измените на *C:\Program Files\IIS Express.* (Для 64-разрядных системах, использовать папку *C:\Program Files (x86) \IIS Express.)* Затем введите следующую команду, используя фактический путь к узлу:

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

Можно использовать любой номер порта, который уже не защищены другим процессом. (Номера портов выше 1024 обычно бесплатны.) Для `path` значение, используйте путь к папке веб-сайта где *.cshtml* файлов.

После выполнения этой команды, чтобы настроить IIS Express для обслуживания страниц, можно открыть браузер и перейдите к *.cshtml* файла. Используйте URL-адрес следующего вида:

`http://localhost:35896/default.cshtml`

Для получения справки по параметрам командной строки IIS Express, введите `iisexpress.exe /?` в командной строке.

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a>Можно использовать элементы управления веб-форм ASP.NET на странице веб-страниц?

Нет. Элементы управления Web Forms как [флажок](https://msdn.microsoft.com/library/system.web.ui.webcontrols.checkbox) управления [проверяющие элементы управления](https://msdn.microsoft.com/library/bwd43d0x)и [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview) управления работает только на страницах Web Forms (*.aspx* файлов). Эти элементы управления требуют страницы веб-форм.

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a>Можно развернуть на сайте веб-страниц ASP.NET без использования WebMatrix

Да. Можно вручную скопировать файлы веб-сайта на сервере (обычно с помощью FTP). При выполнении копирования вручную, необходимо также скопировать файлы, которые поддерживают SQL Server Compact (база данных). Дополнительные сведения см. в записи блога [развертывание веб-страниц приложений без такого средства,](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317).

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a>Нужно ли использовать вспомогательное приложение WebSecurity для поддержки имен входа?

Нет. `SimpleMembership` Поставщика, который входит в веб-страниц ASP.NET — один из вариантов. Также доступны поставщиков безопасности, которые являются частью ASP.NET (который вы может использоваться для работы с веб-форм). Например можно использовать проверку подлинности форм ASP.NET Web Pages так же, как в веб-форм. Один пример использования проверки подлинности форм, см. в разделе в статье службы поддержки Майкрософт [How To Implement Forms-Based проверки подлинности в приложении ASP.NET с помощью C# .NET](https://support.microsoft.com/kb/301240). Чтобы загрузить пример, см. в разделе [версии ASP.NET «имя входа &amp; пароль](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm).

Сведения о том, как использовать проверку подлинности Windows, см. в записи блога [проверки подлинности Windows с помощью ASP.NET Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298).

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a>Поддерживает ли веб-страниц ASP.NET, HTML5?

Да. Страницы, создаваемые с помощью ASP.NET Web Pages (*.cshtml* или *.vbhtml* страниц) являются по сути HTML-страниц, которые также содержат код, выполняемый на сервере, перед отображением страницы. До тех пор, пока браузер пользователя поддерживает HTML5, можно использовать такие элементы HTML5 в *.cshtml* или *.vbhtml* страницы.

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a>Можно ли использовать JavaScript и jQuery с веб-страниц?

Конечно. Страницы, создаваемые с помощью ASP.NET Web Pages (*.cshtml* или *.vbhtml* страниц), просто HTML-страниц с использованием серверного кода в них. Таким образом, что-то можно сделать в обычном HTML-страницу, с помощью JavaScript или jQuery можно также сделать в *.cshtml* или *.vbhtml* страницы.

**Начального сайта** шаблона в WebMatrix содержит ряд библиотеки jQuery. Если создать сайт с помощью этого шаблона, *сценарии* папка содержит библиотеку jQuery core (*jquery 1.6.2.js)* и библиотеки для проверки jQuery (*jQuery.Validate.js;* и т. д.).

Ниже приведены некоторые сообщения в блогах, которые иллюстрируют способы использования jQuery с ASP.NET Web Pages.

- [Добавление к веб-страниц ASP.NET с помощью WebMatrix jQuery богу](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/) с Рейчел Аппель
- [5 мин.: WebMatrix + пользовательский Интерфейс jQuery + json + jQuery шаблоны](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/) , Йонас Eriksson
- [WebMatrix и формы jQuery](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms) , Майк Бринд

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Дополнительные ресурсы


[Руководство по устранению неполадок веб-страниц ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)

[Форум по WebMatrix и ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) веб-сайта ASP.NET
