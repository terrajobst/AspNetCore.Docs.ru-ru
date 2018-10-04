---
uid: mvc/overview/older-versions/aspnet-mvc-4-mobile-features
title: Мобильные функции ASP.NET MVC 4 | Документация Майкрософт
author: Rick-Anderson
description: Сейчас MVC 5, версия этого руководства с примерами кода в развертывание MVC 5 мобильных веб-приложения ASP.NET на веб-сайтах Azure.
ms.author: riande
ms.date: 08/15/2012
ms.assetid: 27dc4fc8-1b51-43b0-933f-fc1b52476523
msc.legacyurl: /mvc/overview/older-versions/aspnet-mvc-4-mobile-features
msc.type: authoredcontent
ms.openlocfilehash: 8b82b8b9b1ee6646072931da889c643afb34d474
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578163"
---
<a name="aspnet-mvc-4-mobile-features"></a>Возможности ASP.NET MVC 4 для мобильных приложений
====================
по [Рик Андерсон]((https://twitter.com/RickAndMSFT))

> Теперь есть версия этого руководства с примерами кода в MVC 5 [развертывание MVC 5 мобильных веб-приложения ASP.NET на веб-сайтов Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/).


Этом учебнике описываются основы работы с мобильными функциями в ASP.NET MVC 4, веб-приложения. Для этого руководства можно использовать [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) или Visual Web Developer 2010 Express пакетом обновления 1 (&quot;Visual Web Developer или VWD&quot;). Профессиональные версии Visual Studio можно использовать, если у вас уже есть.

Перед началом работы убедитесь, что вы установили необходимые компоненты, перечисленные ниже.

- [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) (рекомендуется) или Visual Studio Web Developer Express с пакетом обновления 1. Visual Studio 2012 содержит ASP.NET MVC 4. Если вы используете Visual Web Developer 2010, необходимо установить [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392).

Необходимо также эмулятор браузера мобильного устройства. Подойдет любой из следующих:

- [Эмулятор Windows 7 Phone](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). (Это эмулятор, который используется в большинстве на снимках экрана в этом руководстве).
- Измените строку агента пользователя для эмуляции iPhone. См. в разделе [это](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) запись в блоге.
- [Эмулятор Opera Mobile](http://www.opera.com/developer/tools/mobile/)
- [Apple Safari](http://www.apple.com/safari/download/) с агентом пользователя, задайте для iPhone. Инструкции по настройке агента пользователя в Safari «iPhone», см. в разделе [как предоставить Safari представьте это IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) в блоге Дэвида Alison.

Проекты Visual Studio с исходным кодом C# доступны в этой статье прилагаются:

- [Скачивание начального проекта](https://go.microsoft.com/fwlink/?linkid=228307&amp;clcid=0x409)
- [Скачивание полного проекта](https://go.microsoft.com/fwlink/?linkid=228306&amp;clcid=0x409)

### <a name="what-youll-build"></a>Что вы создадите

В этом учебнике вы добавите функции для мобильных устройств в простое приложение списка конференции, которая предоставляется в [начальный проект](https://go.microsoft.com/fwlink/?LinkId=228307). На следующем рисунке показаны страницы теги готового приложения, как показано на [эмулятора телефона Windows 7](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). См. в разделе [клавиатуры сопоставление для Windows Phone Emulator](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx) для упрощения ввода с клавиатуры.

[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)

Можно использовать Internet Explorer версии 9 или 10, FireFox или Chrome для разработки своего мобильного приложения, задав [строку агента пользователя](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/). Ниже показаны результаты всей работы с помощью Internet Explorer, эмуляция iPhone. Вы можете использовать инструменты разработчика F-12 Internet Explorer и [инструмента Fiddler](http://www.fiddler2.com/fiddler2/) для отладки приложения.

![](aspnet-mvc-4-mobile-features/_static/image3.png)

### <a name="skills-youll-learn"></a>Навыки, которые вы узнаете

Вот, вы узнаете, как:

- Использование HTML5 в шаблоны ASP.NET MVC 4 `viewport` атрибут и адаптивной отрисовки для улучшения отображения на мобильных устройствах.
- Как создать представления.
- Как создать представление переключателя, переключения пользователей позволяет между представлением для мобильных устройств и представление рабочего стола приложения.

### <a name="getting-started"></a>Начало работы

Загрузите приложение списка конференции для начального проекта, используя следующую ссылку: [загрузить](https://go.microsoft.com/fwlink/?LinkId=228307). Щелкните в обозревателе Windows щелкните правой кнопкой мыши *MvcMobile.zip* файл и выберите **свойства**. В **свойства MvcMobile.zip** диалоговое окно, выберите **Unblock** кнопки. (Разблокировка устраняет предупреждение системы безопасности, возникающее при попытке использовать *ZIP-файл* файл, загруженный из Интернета.)

![p1_unBlock](aspnet-mvc-4-mobile-features/_static/image4.png)

Щелкните правой кнопкой мыши *MvcMobile.zip* файл и выберите **извлечь все** Чтобы распаковать файл. В Visual Studio откройте *MvcMobile.sln* файла.

Нажмите клавиши CTRL + F5, чтобы запустить приложение, которое будет отображаться в браузер настольного компьютера. Запустите эмулятор браузера мобильного устройства, скопируйте URL-адрес приложения конференции в эмуляторе и нажмите кнопку **поиск по тегу** ссылку. Если вы используете эмулятор Windows Phone, щелкните в строке URL-адрес и нажмите клавишу Pause, чтобы получить доступ с клавиатуры. На рисунке ниже показано *AllTags* представления (Выбор **поиск по тегу**).

[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)

Отображение очень удобным для чтения на мобильном устройстве. Выберите ссылку ASP.NET.

[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)

Представление тегов ASP.NET — в этом месте нарастает беспорядок. Например **даты** столбец представляет собой непростую задачу для чтения. Далее в этом руководстве вы создадите версию *AllTags* представления специально для браузеров мобильных устройств и, чтобы отображение для чтения.

Примечание: В настоящее время ошибка в модуле мобильных кэширования. Для создания приложений, необходимо установить [основных DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) ценная пакета. См. в разделе [ASP.NET MVC 4 Mobile кэширование ошибки основных](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) подробные исправления.

## <a name="css-media-queries"></a>Медиа-запросов CSS

[Медиа-запросов CSS](http://www.w3.org/TR/css3-mediaqueries/) представляют собой расширение для CSS для типов мультимедиа. Они позволяют создавать правила, которые переопределяют правила CSS по умолчанию для определенных браузеров (агенты пользователей). Общие правила CSS, предназначенного для браузеров мобильных устройств является определение максимальный размер экрана. *Content\Site.css* файл, который создается при создании нового проекта ASP.NET MVC 4 Internet содержит следующий запрос мультимедиа:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample1.css)]

Если окно браузера 850 пикселей в ширину или меньше, оно будет использовать правила CSS внутри этого блока мультимедиа. Подобные запросы мультимедиа CSS можно использовать для предоставления более удобного отображения содержимого HTML на небольших браузеры (например, браузеры мобильных устройств), чем правила CSS по умолчанию, которые предназначены для широкого отображается для браузеров настольных компьютеров.

## <a name="the-viewport-meta-tag"></a>Окно просмотра мета-тег

Большинство мобильных браузеров определить ширину окна виртуального браузера ( *просмотра*) это значительно больше, чем фактическую ширину мобильного устройства. Это позволяет браузеры мобильных устройств в соответствии со всей веб-страницы внутри виртуального дисплея. Пользователи затем можно увеличить масштаб содержимого. Тем не менее если значение ширины окна просмотра для ширины самого устройства, не масштабирования необходим, так как содержимое размещается в браузере мобильного устройства.

Окно просмотра `<meta>` тег в файле макета ASP.NET MVC 4 задает окно просмотра по ширине устройства. В следующей строке показано окно просмотра `<meta>` тег в файле макета ASP.NET MVC 4.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample2.html)]

## <a name="examining-the-effect-of-css-media-queries-and-the-viewport-meta-tag"></a>Изучение эффекта CSS медиа-запросами и просмотра мета-тег

Откройте *Views\Shared\\_Layout.cshtml* файл в редакторе и закомментируйте окна просмотра `<meta>` тега. В следующей разметке показан закомментированную строку.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample3.cshtml)]

Откройте *MvcMobile\Content\Site.css* в редакторе и измените максимальную ширину в запросе носителя на 0 пикселов. Это будет препятствовать правила CSS, используемых в браузерах мобильных устройств. В следующей строке показано запрос измененный носителя:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample4.css)]

Сохраните изменения и перейдите к приложению конференции в эмулятор браузера мобильного устройства. Мало места на следующем рисунке приведен результат удаления окна просмотра `<meta>` тега. С помощью окна просмотра отсутствуют `<meta>` тег, браузер является уменьшение масштаба до ширины окна просмотра по умолчанию (850 пикселей или шириной для большинства мобильных обозревателей.)

[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)

Отменить изменения, раскомментируйте окна просмотра `<meta>` тег в файле макета и восстановить запрос носителя 850 пикселей в *Site.css* файл. Сохраните изменения и обновите браузер мобильного устройства для проверки того, что отображение мобильной восстановлен.

Окно просмотра `<meta>` тег и медиа-запросов CSS не являются специфичными для ASP.NET MVC 4, и можно воспользоваться преимуществами этих функций в любое веб-приложение. Но теперь встроены в файлы, которые создаются при создании нового проекта ASP.NET MVC 4.

Дополнительные сведения об области просмотра `<meta>` тег, см. в разделе [сказка о двух viewports — часть 2](http://www.quirksmode.org/mobile/viewports2.html).

В следующем разделе вы увидите, как обеспечить представления для браузеров мобильных устройств.

## <a name="overriding-views-layouts-and-partial-views"></a>Переопределение представлений, макетов и частичных представлений

Значительные новая функция в ASP.NET MVC 4 — это простой механизм, который позволяет переопределить любое представление (включая макеты и частичные представления) для браузеров мобильных устройств, как правило, для отдельного браузера мобильных или для любого конкретного браузера. Чтобы предоставить представление мобильных обозревателей, можно скопировать файл представления и добавить *. Mobile* к имени файла. Например, чтобы создать мобильное *индекс* Просмотр, копирование *Views\Home\Index.cshtml* для *Views\Home\Index.Mobile.cshtml*.

В этом разделе вы создадите файл макета для мобильных обозревателей.

Сначала скопируйте *Views\Shared\\_Layout.cshtml* для *Views\Shared\\_Layout.Mobile.cshtml*. Откройте  *\_Layout.Mobile.cshtml* и измените заголовок с **конференции MVC4** для **конференции (мобильный)**.

В каждом `Html.ActionLink` вызывать, удалите «Поиск по» в каждой связи *ActionLink*. В следующем коде показано завершенное основной раздел файла макета для мобильных устройств.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample5.cshtml)]

Копировать *Views\Home\AllTags.cshtml* файл *Views\Home\AllTags.Mobile.cshtml*. Откройте новый файл и измените `<h2>` элемент с «Tags» на «Tags (M)»:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample6.html)]

Перейдите на страницу тегов, используя классический браузер и эмулятор браузера мобильного устройства. Эмулятор браузера мобильного устройства показывает два изменения, внесенные.

[![p2m_layoutTags.mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)

Напротив отображения рабочего стола не изменилась.

[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)

## <a name="browser-specific-views"></a>Представлений для браузера

Помимо представления для мобильных устройств и настольных компьютеров можно создать представления для отдельных браузеров. Например можно создать представления, которые предназначены специально для браузера iPhone. В этом разделе вы создадите макет для браузера iPhone и версию *AllTags* представления.

Откройте *Global.asax* файл и добавьте следующий код, чтобы `Application_Start` метод.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample7.cs)]

Этот код определяет новый режим отображения с именем «iPhone», которая будет сопоставлена с каждого входящего запроса. Если входящий запрос удовлетворяет условию, которое вы определили (то есть, если агент пользователя содержит строку «iPhone»), ASP.NET MVC будет искать представления, имя которого содержит суффикс «iPhone».

В коде, щелкните правой кнопкой мыши `DefaultDisplayMode`, выберите **устранить**, а затем выберите `using System.Web.WebPages;`. Это действие добавляет ссылку к `System.Web.WebPages` пространство имен, которое там, где `DisplayModes` и `DefaultDisplayMode` определенных типов.

[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)

Кроме того, можно просто вручную добавить следующую строку `using` раздел файла.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample8.cs)]

Полное содержимое *Global.asax* файла приведен ниже.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample9.cs)]

Сохраните изменения. Копировать *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* файл *MvcMobile\Views\Shared\\_Layout.iPhone.cshtml*. Откройте новый файл и измените `h1` заголовком от `Conference (Mobile)` для `Conference (iPhone)`.

Копировать *MvcMobile\Views\Home\AllTags.Mobile.cshtml* файл *MvcMobile\Views\Home\AllTags.iPhone.cshtml*. В новом файле измените `<h2>` элемент из «Tags (M)» на «Tags (iPhone)».

Запустите приложение. Запустите эмулятор браузера мобильного устройства, убедитесь, что его агента пользователя задано значение «iPhone» и перейдите к *AllTags* представления. На следующем снимке экрана показан *AllTags* представления, визуализированного на [Safari](http://www.apple.com/safari/download/) браузера. Safari для Windows можно загрузить [здесь](https://support.apple.com/kb/DL1531).

[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)

В этом разделе мы рассмотрели способы создания мобильных макеты и представления и как создавать макеты и представления для конкретных устройств, таких как iPhone. В следующем разделе вы узнаете, как использовать jQuery Mobile более привлекательные представлений для мобильных устройств.

## <a name="using-jquery-mobile"></a>С помощью jQuery Mobile

[JQuery Mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html) библиотека предоставляет платформа пользовательского интерфейса, который работает на всех основных браузеров мобильных устройств. jQuery Mobile применяется *прогрессивное Совершенствование* для браузеров мобильных устройств, которые поддерживают CSS и JavaScript. Прогрессивное Совершенствование позволяет все обозреватели для отображения основных содержимого веб-страницы, предоставляя более мощные браузеры и устройства для более широкие отображались. Файлы JavaScript и CSS, которые входят в состав jQuery Mobile стиля многие элементы в соответствии с браузеры мобильных устройств без изменения разметки.

В этом разделе вы установите *jQuery.Mobile.MVC* пакета NuGet, который устанавливает jQuery Mobile и мини-приложение переключателя.

Чтобы начать, удалите *Shared\\_Layout.Mobile.cshtml* и *Shared\\_Layout.iPhone.cshtml* файлы, созданные ранее.

Переименуйте *Views\Home\AllTags.Mobile.cshtml* и *Views\Home\AllTags.iPhone.cshtml* файлы *Views\Home\AllTags.iPhone.cshtml.hide* и  *Views\Home\AllTags.Mobile.cshtml.hide*. Так как файлы больше не имеют *.cshtml* расширения, они не будут использоваться средой выполнения ASP.NET MVC для подготовки к просмотру *AllTags* представления.

Установка *jQuery.Mobile.MVC* пакет NuGet, таким образом:

1. Из **средства** меню, выберите **диспетчер пакетов библиотеки**, а затем выберите **консоль диспетчера пакетов**.

    [![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)
2. В **консоль диспетчера пакетов**, введите `Install-Package jQuery.Mobile.MVC -version 1.0.0`

На следующем рисунке показана файлы добавления и изменения в проект MvcMobile jQuery.Mobile.MVC пакетом NuGet. Файлы, которые добавляются имеют [добавьте] добавленным после имени файла. Образ не содержит файл GIF и PNG-файлов добавляется *Content\images* папки.

![](aspnet-mvc-4-mobile-features/_static/image21.png)

Пакет NuGet jQuery.Mobile.MVC устанавливает следующее:

- *Приложения\_Start\BundleMobileConfig.cs* файл, который требуется для ссылки на добавлены файлы CSS и JavaScript jQuery. Необходимо выполните приведенные ниже инструкции и ссылаться на пакет мобильных устройств, определенные в этом файле.
- файлы jQuery Mobile CSS.
- Объект `ViewSwitcher` контроллера мини-приложения (*Controllers\ViewSwitcherController.cs*).
- файлы JavaScript для мобильных jQuery.
- Файл jQuery Mobile стиля макета (*Views\Shared\\_Layout.Mobile.cshtml*).
- Частичное представление переключателя *(MvcMobile\Views\Shared\\_ViewSwitcher.cshtml*), предоставляющий ссылку в верхней части каждой страницы, чтобы перейти из рабочего стола представление для мобильных устройств и наоборот.
- Несколько<em>.png</em> и <em>.gif</em> файлы изображений в <em>Content\images</em> папки.

Откройте *Global.asax* файл и добавьте следующий код в последней строке из `Application_Start` метод.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample10.cs)]

В следующем коде показан полный *Global.asax* файл.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample11.cs?highlight=26)]

> [!NOTE]
> Если вы используете Internet Explorer 9 и вы не видите `BundleMobileConfig` строку выше в выделение желтым цветом, нажмите кнопку [кнопка просмотра в режиме совместимости](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![изображение кнопки «Просмотр в режиме совместимости» (отключено)](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg " Изображение кнопки «Просмотр в режиме совместимости» (отключено)") в IE, чтобы изменить контур значок ![изображение кнопки «Просмотр в режиме совместимости» (отключено)](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "изображение кнопки «Просмотр в режиме совместимости» (отключено) ") сплошной ![изображение кнопки просмотра в режиме совместимости (on)](http://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "изображение кнопки просмотра в режиме совместимости (on)"). Вместо этого учебника можно просмотреть в FireFox или Chrome.


Откройте *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* файл и добавьте следующий код непосредственно после `Html.Partial` вызова:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample12.cshtml)]

Полный *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* файла приведен ниже:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample13.cshtml)]

При создании приложения и в вашей эмулятор браузера мобильного устройства, перейдите к *AllTags* представления. Вы увидите следующее:

[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)

> [!NOTE]
> Можно отлаживать мобильных конкретный код, [параметр строку агента пользователя](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) IE или Chrome для iPhone, а затем с помощью средств разработчика F-12. Если не отображается в браузере мобильного **Главная**, **говорящего**, **тега**, и **даты** связи, как кнопки, ссылки на jQuery Mobile сценарии и файлы CSS, скорее всего, не подходят.


Помимо изменения стиля, вы увидите **отображение мобильного представления** и ссылку, которая позволяет переключать представление для мобильных устройств, чтобы представление рабочего стола. Выберите **представление рабочего стола** отображается ссылка, а представление рабочего стола.

[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)

Представление рабочего стола не дает возможности для перехода непосредственно к представление для мобильных устройств. Теперь мы исправим это. Откройте *Views\Shared\\_Layout.cshtml* файла. Под страницы `body` элемента, добавьте следующий код, который отображает представление переключателя мини-приложения:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample14.cshtml)]

Обновить *AllTags* представление в браузере мобильного устройства. Теперь можно переходить между представлениями desktop и mobile.

[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)

> [!NOTE]
> Отладка Примечание: можно добавить следующий код в конец Views\Shared\\_ViewSwitcher.cshtml для отладки представления, настроенное с помощью строки агента пользователя браузера на мобильном устройстве.
> 
> [!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample15.cs)]
> 
>  и следующий заголовок для добавления *Views\Shared\\_Layout.cshtml* файла.  
> 
> [!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample16.html)]


Перейдите к *AllTags* страницу в классическом браузере. Представление переключателя мини-приложения не отображаются в классическом браузере, поскольку он добавляется только к странице макета для мобильных устройств. Далее в этом руководстве вы увидите, как можно добавить мини-приложение переключателя представление рабочего стола.

[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)

## <a name="improving-the-speakers-list"></a>Улучшение списка докладчиков

В браузере мобильного устройства, выберите **динамики** ссылку. Так как нет мобильного представления (*AllSpeakers.Mobile.cshtml*), отображения для говорящих по умолчанию (*AllSpeakers.cshtml*) визуализируется с использованием представления мобильного макета ( *\_ Layout.Mobile.cshtml*).

[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)

Вы можете глобально отключить представление по умолчанию (не для мобильных устройств) из отображения в мобильном макете, задав `RequireConsistentDisplayMode` для `true` в *представления\\_ViewStart.cshtml* файл следующим образом:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample17.cshtml)]

Когда `RequireConsistentDisplayMode` присваивается `true`, то мобильный макет (<em>\_Layout.Mobile.cshtml</em>) используется только для мобильных представлений. (То есть файл представления имеет форму <em>** ViewName</em><em>. Mobile.cshtml</em>.) Вы можете задать `RequireConsistentDisplayMode` для `true` Если макет для мобильных устройств не работает с представлений не для мобильных устройств. Снимок экрана ниже показано как <em>динамики</em> страница отображается при значении `RequireConsistentDisplayMode` присваивается `true`.

[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)

Согласованный режим отображения в представлении можно отключить, установив `RequireConsistentDisplayMode` для `false` в файле представления. Следующая разметка в *Views\Home\AllSpeakers.cshtml* файле задает `RequireConsistentDisplayMode` для `false`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample18.cshtml)]

## <a name="creating-a-mobile-speakers-view"></a>Создание представления мобильных докладчиков

Как вы только что увидели, *динамики* удобно для чтения, но ссылки невелики и трудно на мобильном устройстве. В этом разделе вы создадите мобильных обозревателей *динамики* представления, который выглядит, как современные мобильные приложения — отображает большой, чтобы коснитесь ссылки и содержит поле поиска, чтобы быстро находить докладчиков.

Копировать *AllSpeakers.cshtml* для *AllSpeakers.Mobile.cshtml*. Откройте *AllSpeakers.Mobile.cshtml* файл и удалите `<h2>` элемента заголовка.

В `<ul>` , добавьте `data-role` атрибут и присвойте ему значение `listview`. Как и другие [ `data-*` атрибуты](http://html5doctor.com/html5-custom-data-attributes/), `data-role="listview"` делает большой список элементы проще для отвода. Вот как выглядит полная разметка:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample19.cshtml)]

Обновите браузер мобильного устройства. Обновленное представление выглядит следующим образом:

[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)

Несмотря на то, что представление для мобильных устройств была улучшена, трудно перемещаться по длинному списку докладчиков. Чтобы исправить это, в `<ul>` , добавьте `data-filter` атрибут и присвойте ему значение `true`. В коде ниже показан `ul` разметки.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample20.html)]

На следующем рисунке показано поле фильтра поиска в верхней части страницы, полученный в результате `data-filter` атрибута.

[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)

При вводе каждой буквы в поле поиска, jQuery Mobile фильтрует отображаемого списка, как показано на рисунке ниже.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)

## <a name="improving-the-tags-list"></a>Улучшение списка тегов

Значение по умолчанию, такие как *динамики* представлении *теги* удобно для чтения, но ссылки слишком малого и трудно на мобильном устройстве. В этом разделе вы исправите *теги* следующим образом: устранены *динамики* представления.

Удалить &quot;скрыть&quot; суффикс *Views\Home\AllTags.Mobile.cshtml.hide* файл, чтобы имя *Views\Home\AllTags.Mobile.cshtml*. Откройте переименованный файл и удалите `<h2>` элемент.

Добавить `data-role` и `data-filter` атрибуты `<ul>` тег, как показано ниже:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample21.html)]

На следующем рисунке показана страница теги, фильтрация по буквы `J`.

[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)

## <a name="improving-the-dates-list"></a>Улучшение списка дат

Вы можете улучшить *даты* просматривать как повышают уровень *динамики* и *теги* просматривает, так как это проще в использовании на мобильном устройстве.

Копировать *Views\Home\AllDates.cshtml* файл *Views\Home\AllDates.Mobile.cshtml*. Откройте новый файл и удалите `<h2>` элемент.

Добавить `data-role="listview"` для `<ul>` тег следующим образом:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample22.html)]

На следующем рисунке показано, что **даты** как выглядит страница с `data-role` атрибут на месте.

[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png) Замените содержимое файла *Views\Home\AllDates.Mobile.cshtml* файла следующим кодом:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample23.cshtml)]

Этот код группирует все сеансы по дням. Он создает разделитель списка для каждого нового дня, и в нем перечислены все сеансы для каждого дня в списке разделитель. Вот, что получится при выполнении этого кода:

[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)

## <a name="improving-the-sessionstable-view"></a>Улучшение представления SessionsTable

В этом разделе вы создадите представление мобильных обозревателей сеансов. Изменения, вносимые мы будет более сложный, чем в других представлениях, которые мы создали.

В браузере мобильного устройства коснитесь **говорящего** кнопку, а затем введите `Sc` в поле поиска.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)

Коснитесь **(Scott hanselman)** ссылку.

[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)

Как вы видите, отображение трудно читать в браузере мобильного устройства. Столбец дат трудно читать, и столбец "теги" выходит за пределы представления. Чтобы устранить эту проблему, скопируйте *Views\Home\SessionsTable.cshtml* для *Views\Home\SessionsTable.Mobile.cshtml*, а затем замените содержимое файла следующим кодом:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample24.cshtml)]

Код удаляет комнаты и теги столбцов и по вертикали, форматирует заголовок, говорящего и даты, чтобы все эти данные можно было читать в браузере мобильного устройства. На рисунке ниже приведены изменения в коде.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)

## <a name="improving-the-sessionbycode-view"></a>Улучшение представления SessionByCode

Наконец, вы создадите представление мобильных обозревателей *SessionByCode* представления. В браузере мобильного устройства коснитесь **говорящего** кнопку, а затем введите `Sc` в поле поиска.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)

Коснитесь **(Scott hanselman)** ссылку. Скотт Хансельман сеансы будут отображены.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)

Выберите **обзором MS Web стека из любовь** ссылку.

[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)

Представление рабочего стола по умолчанию работает, но его можно увеличить.

Копировать *Views\Home\SessionByCode.cshtml* для *Views\Home\SessionByCode.Mobile.cshtml* и замените содержимое файла *Views\Home\SessionByCode.Mobile.cshtml*файл, используя следующую разметку:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample25.cshtml)]

Новая разметка использует `data-role` атрибут для улучшения макета представления.

Обновите браузер мобильного устройства. Следующий рисунок отражает только что внесенные изменения в коде:

[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)

## <a name="wrapup-and-review"></a>Обзор и — Сводка

Этот учебник представила новые мобильные функции ASP.NET MVC 4 Developer Preview. Мобильные возможности:

- Возможность переопределения макета, представлений и частичных представлений, как глобально, так и для отдельного представления.
- Контроль над макетом и частичное переопределение с помощью принудительного применения `RequireConsistentDisplayMode` свойство.
- Просмотреть переключатель мини-приложения для мобильных устройств представлений также могут отобразиться в представлениях рабочего стола.
- Поддержка для поддержки определенных браузеров, таких как браузер iPhone.

## <a name="see-also"></a>См. также

- [jQuery Mobile](http://jquerymobile.com) сайта.
- [jQuery Mobile Обзор](http://jquerymobile.com/demos/1.0b3/docs/about/intro.html)
- [Рекомендации от W3C по мобильных веб-технологий приложения](http://www.w3.org/TR/mwabp/)
- [W3c по запросам мультимедиа](http://www.w3.org/TR/css3-mediaqueries/)
