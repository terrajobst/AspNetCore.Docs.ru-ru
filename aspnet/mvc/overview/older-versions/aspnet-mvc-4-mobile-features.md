---
uid: mvc/overview/older-versions/aspnet-mvc-4-mobile-features
title: "Мобильные функции ASP.NET MVC 4 | Документы Microsoft"
author: Rick-Anderson
description: "Теперь имеется версии MVC 5 данного руководства с примерами кода на развертывание MVC 5 мобильного веб-приложения ASP.NET на веб-сайтов Azure."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 27dc4fc8-1b51-43b0-933f-fc1b52476523
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/aspnet-mvc-4-mobile-features
msc.type: authoredcontent
ms.openlocfilehash: f4e0e4eb558e0c7b9e94fc83ede986fa4c666739
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2018
---
<a name="aspnet-mvc-4-mobile-features"></a>Возможности ASP.NET MVC 4 для мобильных приложений
====================
По [Рик Андерсон](https://github.com/Rick-Anderson)

> Теперь имеется версии MVC 5 данного руководства с примерами кода в [развертывания ASP.NET MVC 5 мобильного веб-приложения на веб-сайтов Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/).


Этот учебник поможет узнать основные принципы работы с функциями мобильных устройств в ASP.NET MVC 4, веб-приложения. В этом учебнике, можно использовать [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) или Visual Web Developer 2010 Express пакетом обновления 1 (&quot;Visual Web Developer или VWD&quot;). Если у вас уже есть, можно использовать профессиональные версии Visual Studio.

Прежде чем начать, убедитесь, что вы установили необходимые компоненты, перечисленные ниже.

- [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) (рекомендуется) или Visual Studio Web Developer Express с пакетом обновления 1. Visual Studio 2012 содержит ASP.NET MVC 4. Если вы используете Visual Web Developer 2010, необходимо установить [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392).

Необходимо также эмулятор мобильных браузеров. Будет работать любой из следующих:

- [Эмулятор Windows 7 Phone](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). (Это эмулятор, используемый в большинстве снимков экрана в этом учебнике).
- Измените строку пользовательского агента для эмуляции iPhone. В разделе [это](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) запись в блоге.
- [Opera Mobile Emulator](http://www.opera.com/developer/tools/mobile/)
- [Apple Safari](http://www.apple.com/safari/download/) с агентом пользователя, задайте для iPhone. Инструкции по установке агента пользователя в Safari на «iPhone» см. в разделе [как предоставить Safari представьте это IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) в блоге Дэвида Alison.

Проекты Visual Studio с исходным кодом C# доступны по следующему адресу:

- [Загрузка начального проекта](https://go.microsoft.com/fwlink/?linkid=228307&amp;clcid=0x409)
- [Версия проекта](https://go.microsoft.com/fwlink/?linkid=228306&amp;clcid=0x409)

### <a name="what-youll-build"></a>Что мы создадим

В этом учебнике вы добавите мобильные функции простое приложение листинг конференции, которая предоставляется в [начальный проект](https://go.microsoft.com/fwlink/?LinkId=228307). На следующем рисунке показан страницы теги готового приложения как видно в [эмулятор Windows Phone 7](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). В разделе [клавиатуры сопоставления для Windows Phone Emulator](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx) для упрощения ввода с клавиатуры.

[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)

Можно использовать Internet Explorer версии 9 или 10, FireFox или Chrome при разработке мобильного приложения, задав [строку агента пользователя](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/). На следующем рисунке показана завершенная учебник, с помощью Internet Explorer, имитируя iPhone. Вы можете использовать инструменты разработчика F-12 Internet Explorer и [инструмента Fiddler](http://www.fiddler2.com/fiddler2/) для отладки приложения.

![](aspnet-mvc-4-mobile-features/_static/image3.png)

### <a name="skills-youll-learn"></a>Навыки, которые вы узнаете

Ниже приведен изучаемого материала.

- Использование HTML5 в шаблоны ASP.NET MVC 4 `viewport` атрибут и адаптивной отрисовки для улучшения отображения на мобильных устройствах.
- Инструкции по созданию мобильные особых представлений.
- Инструкции по созданию представления переключателя, переключение пользователей позволяет между представлением для мобильных устройств и настольных представление приложения.

### <a name="getting-started"></a>Начало работы

Загрузите приложение конференции листинг для начальный проект, используя следующую ссылку: [загрузить](https://go.microsoft.com/fwlink/?LinkId=228307). Затем в проводнике Windows щелкните правой кнопкой мыши *MvcMobile.zip* файл и выберите **свойства**. В **свойства MvcMobile.zip** диалогового окна выберите **Unblock** кнопки. (Разблокировке предотвращает предупреждение системы безопасности, которая возникает при попытке использования *.zip* файл, загруженный из Интернета.)

![p1_unBlock](aspnet-mvc-4-mobile-features/_static/image4.png)

Щелкните правой кнопкой мыши *MvcMobile.zip* файла и выберите **извлечь все** для распаковки файла. В Visual Studio откройте *MvcMobile.sln* файла.

Нажмите клавиши CTRL + F5 для запуска приложения, который будет отображаться в браузере рабочего стола. Запуск симулятора мобильных браузеров и скопируйте URL-адрес для конференции приложения в эмуляторе нажмите кнопку **Обзор тегом** ссылку. При использовании эмулятора Windows Phone, щелкните в строке URL-адреса и нажмите клавишу Pause, чтобы получить доступ с клавиатуры. На рисунке ниже показана *AllTags* представление (в выборе **Обзор тегом**).

[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)

Отображение удобочитаемым на мобильном устройстве. Выберите ссылку ASP.NET.

[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)

Просмотр тега ASP.NET очень загроможденной. Например **даты** столбец является очень трудно читать. Далее в этом учебнике вы создадите версии *AllTags* представления специально для браузеры для мобильных устройств и который сделает отображения для чтения.

Примечание: В данный момент есть ошибка в подсистеме мобильных кэширования. Для производственных приложений необходимо установить [основных DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) фрагментом пакета. В разделе [ASP.NET MVC 4 Mobile кэширование ошибки основных](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) подробные сведения для устранения этой проблемы.

## <a name="css-media-queries"></a>Медиа-запросами CSS

[Медиа-запросами CSS](http://www.w3.org/TR/css3-mediaqueries/) являются расширением CSS для типов носителей. Они позволяют создавать правила, которые переопределяют правила CSS по умолчанию для определенных обозревателей (агенты пользователя). Общие правила CSS, предназначенного браузеры для мобильных устройств является определение максимальный размер экрана. *Content\Site.css* файл, который создается при создании нового проекта ASP.NET MVC 4 Интернет содержит следующий запрос мультимедиа:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample1.css)]

Если 850 пикселей в ширину или окно браузера, он будет использовать правила CSS внутри этого блока носителя. Подобные запросы CSS мультимедиа можно использовать для предоставления более удобного отображения содержимого HTML на небольшой браузеры (например, браузеры для мобильных устройств), чем по умолчанию правила CSS, предназначенных для широкого отображается для браузеров настольных компьютеров.

## <a name="the-viewport-meta-tag"></a>Метатег окна просмотра

Большинство мобильных браузеров определяют ширину окна браузера виртуальный ( *просмотра*), значительно больше, чем фактическую ширину мобильного устройства. Это позволяет браузеры для мобильных устройств по размеру всей веб-страницы внутри виртуального экрана. Пользователи могут затем увеличение масштаба интересные содержимого. Тем не менее если значение ширины окна просмотра по ширине фактическом устройстве, масштабирования не требуется, так как содержимое помещается в мобильных браузеров.

Окно просмотра `<meta>` тег в файле разметки ASP.NET MVC 4 задает окно просмотра по ширине устройства. Строки кода окна просмотра `<meta>` тег в файле разметки ASP.NET MVC 4.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample2.html)]

## <a name="examining-the-effect-of-css-media-queries-and-the-viewport-meta-tag"></a>Изучение эффект CSS медиа-запросами и просмотра метатег

Откройте *представления\общие\\_Layout.cshtml* файл в редакторе и закомментируйте окна просмотра `<meta>` тег. В следующем примере показана строка комментарий.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample3.cshtml)]

Откройте *MvcMobile\Content\Site.css* в редакторе и измените максимальную ширину в запрос носителя для 0 пикселей. Это будет препятствовать правила CSS, используемых в браузеры для мобильных устройств. Следующая строка представляет запрос измененного носителя.

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample4.css)]

Сохраните изменения и перейти к приложению конференции в эмуляторе мобильных браузеров. Очень мала на следующем рисунке приведен результат удаления области просмотра `<meta>` тег. С помощью окна просмотра отсутствуют `<meta>` тег, браузер является уменьшение ширины окна просмотра по умолчанию (850 пикселей или шириной для большинства мобильных браузеров.)

[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)

Отменить внесенные изменения, раскомментируйте окна просмотра `<meta>` тег в файле макета и восстановить запрос носителя 850 пикселей *Site.css* файла. Сохранить изменения и обновить страницу в браузере мобильного проверка восстановления мобильной отображения.

Окно просмотра `<meta>` тег и запрос носителя CSS не являются специфичными для ASP.NET MVC 4, а также можно воспользоваться преимуществами этих функций в любой веб-приложения. Но теперь они встроены в файлы, которые создаются при создании нового проекта ASP.NET MVC 4.

Дополнительные сведения об окне просмотра `<meta>` тег см. в разделе [рассказ об два viewports — часть](http://www.quirksmode.org/mobile/viewports2.html).

В следующем разделе вы увидите, как предоставить определенным представлениям браузера mobile.

## <a name="overriding-views-layouts-and-partial-views"></a>Переопределения, представления, разделяемые представления и макеты

Значительные новая функция в ASP.NET MVC 4 — это простой механизм, позволяющий переопределить любого представления (включая макетов и частичных представлений) для мобильных браузеров, как правило, для отдельного браузера мобильных устройств или для любого конкретного браузера. Чтобы предоставить представления для мобильных устройств, можно скопировать файл представления и добавить *. Mobile* к имени файла. Например, для создания мобильных устройств *индекс* Просмотр, копирование *Views\Home\Index.cshtml* для *Views\Home\Index.Mobile.cshtml*.

В этом разделе мы создадим файл макета конкретных мобильных устройств.

Чтобы начать, скопируйте *представления\общие\\_Layout.cshtml* для *представления\общие\\_Layout.Mobile.cshtml*. Откройте  *\_Layout.Mobile.cshtml* и измените заголовок из **конференции MVC4** для **конференции (мобильный)**.

В каждом `Html.ActionLink` вызова, удалите «Просмотр по» в каждой связи *ActionLink*. Ниже приведен завершенного основной раздел файл макета для мобильных устройств.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample5.cshtml)]

Копировать *Views\Home\AllTags.cshtml* файл *Views\Home\AllTags.Mobile.cshtml*. Откройте новый файл и измените `<h2>` элемент «Теги» для «теги (M)»:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample6.html)]

Перейти на страницу "теги", используя браузер для настольных компьютеров и мобильных браузеров эмулятора. Эмулятор мобильных браузеров показано два внесенные изменения.

[![p2m_layoutTags.mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)

Напротив отображения рабочего стола не изменилась.

[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)

## <a name="browser-specific-views"></a>Браузер особых представлений

Помимо представления отдельных мобильный или рабочий стол можно создать представления для отдельного браузера. Например можно создать представления, которые специально предназначены для браузера iPhone. В этом разделе вы создадите макет для браузера iPhone и версию iPhone *AllTags* представления.

Откройте *Global.asax* и добавьте следующий код для файла `Application_Start` метод.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample7.cs)]

Этот код определяет новый режим отображения с именем «iPhone», которое совпадет с каждого входящего запроса. Если входящий запрос соответствует условию, заданных (то есть, если агент пользователя содержит строку «iPhone»), ASP.NET MVC будет искать представления, имя которого содержит суффикс «iPhone».

В коде, щелкните правой кнопкой мыши `DefaultDisplayMode`, выберите **Разрешить**, а затем выберите `using System.Web.WebPages;`. Это добавляет ссылку на `System.Web.WebPages` пространство имен, что, если `DisplayModes` и `DefaultDisplayMode` определенных типов.

[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)

Кроме того, можно просто вручную добавить следующую строку, чтобы `using` раздел файла.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample8.cs)]

Полное содержимое *Global.asax* файла показан ниже.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample9.cs)]

Сохраните изменения. Копировать *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* файл *MvcMobile\Views\Shared\\_Layout.iPhone.cshtml*. Откройте новый файл и измените `h1` заголовок из `Conference (Mobile)` для `Conference (iPhone)`.

Копировать *MvcMobile\Views\Home\AllTags.Mobile.cshtml* файл *MvcMobile\Views\Home\AllTags.iPhone.cshtml*. В новом файле, измените `<h2>` элемент из «теги (M)» для «Теги (iPhone)».

Запустите приложение. Запуск эмулятора мобильных браузеров, убедитесь, что его агент пользователя имеет значение «iPhone» и перейдите в *AllTags* представления. На следующем снимке экрана показано *AllTags* представление к просмотру в [Safari](http://www.apple.com/safari/download/) браузера. Safari для Windows можно загрузить [здесь](https://support.apple.com/kb/DL1531).

[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)

В этом разделе мы видели создание макетов мобильных устройств и представлений и создание макетов и представлений для конкретных устройств, таких как iPhone. В следующем разделе вы научитесь использовать jQuery Mobile более привлекательные представлений для мобильных устройств.

## <a name="using-jquery-mobile"></a>С помощью jQuery Mobile

[JQuery Mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html) библиотека предоставляет платформу пользовательского интерфейса, который работает на все основные браузеры для мобильных устройств. применяет jQuery Mobile *прогрессивном расширении* для браузеры для мобильных устройств, поддерживающих CSS и JavaScript. Последовательная усовершенствование позволяет всеми браузерами для отображения базовое содержимое веб-страницы, обеспечив при этом более мощных браузерах и устройствах иметь обширные возможности отображения. Файлы JavaScript и CSS, которые входят в состав jQuery Mobile стиля многие элементы в соответствии с браузеры для мобильных устройств без внесения изменений разметки.

В этом разделе вы установите *jQuery.Mobile.MVC* пакет NuGet, который устанавливает jQuery Mobile и графического представления переключателя.

Чтобы начать, удалите *Shared\\_Layout.Mobile.cshtml* и *Shared\\_Layout.iPhone.cshtml* файлы, которые были созданы ранее.

Переименуйте *Views\Home\AllTags.Mobile.cshtml* и *Views\Home\AllTags.iPhone.cshtml* файлов *Views\Home\AllTags.iPhone.cshtml.hide* и  *Views\Home\AllTags.Mobile.cshtml.hide*. Так как файлы больше не *.cshtml* расширения, они не будут использоваться средой выполнения ASP.NET MVC для подготовки к просмотру *AllTags* представления.

Установка *jQuery.Mobile.MVC* пакет NuGet таким образом:

1. Из **средства** последовательно выберите пункты **диспетчер пакетов библиотеки**, а затем выберите **консоль диспетчера пакетов**.

    [![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)
2. В **консоль диспетчера пакетов**, введите`Install-Package jQuery.Mobile.MVC -version 1.0.0`

На рисунке показаны файлы, добавленные и измененные в проект MvcMobile jQuery.Mobile.MVC пакет NuGet. Файлы, добавляемые имеют [Добавить] добавленным после имени файла. Изображение не содержит GIF и PNG-файлы добавлены *Content\images* папки.

![](aspnet-mvc-4-mobile-features/_static/image21.png)

Пакет NuGet jQuery.Mobile.MVC устанавливает следующие элементы:

- *Приложения\_Start\BundleMobileConfig.cs* файл, который необходим для ссылки на добавлены файлы JavaScript и CSS jQuery. Должно следовать приведенным ниже инструкциям и ссылаться мобильных пакета, определенные в этом файле.
- файлы jQuery Mobile CSS.
- Объект `ViewSwitcher` контроллера мини-приложения (*Controllers\ViewSwitcherController.cs*).
- файлы jQuery Mobile JavaScript.
- Файл jQuery Mobile стиля макета (*представления\общие\\_Layout.Mobile.cshtml*).
- Просмотреть переключатель частичного представления *(MvcMobile\Views\Shared\\_ViewSwitcher.cshtml*), предоставляющий ссылку в верхней части каждой страницы, чтобы перейти из рабочего стола на представление для мобильных устройств и наоборот.
- Несколько*.png* и *.gif* файлы изображений в *Content\images* папки.

Откройте *Global.asax* и добавьте следующий код в качестве последней строки файла `Application_Start` метод.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample10.cs)]

В следующем коде показано полное *Global.asax* файла.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample11.cs?highlight=26)]

> [!NOTE]
> Если вы используете Internet Explorer 9, и вы не видите `BundleMobileConfig` строка выше в желтой выделение, нажмите кнопку [кнопки представления совместимости](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![изображение кнопки «Просмотр в режиме совместимости» (отключено)] (http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg " Изображение кнопки «Просмотр в режиме совместимости» (отключено)") в IE, чтобы она стала измените с контуром ![изображение кнопки «Просмотр в режиме совместимости» (отключено)](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "изображение кнопки «Просмотр в режиме совместимости» (отключено) ") сплошной цвет ![изображение кнопки представления совместимости (on)](http://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "изображение кнопки представления совместимости (on)"). Вместо этого учебника можно просмотреть в FireFox или Chrome.


Откройте *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* и добавьте следующий код непосредственно после `Html.Partial` вызова:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample12.cshtml)]

Полный *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* файла показан ниже:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample13.cshtml)]

Построение приложения и в симулятор мобильных браузеров, перейдите к *AllTags* представления. Отобразится следующее:

[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)

> [!NOTE]
> Можно отлаживать мобильных конкретный код, [параметр строку агента пользователя](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) IE и хром iPhone и затем с помощью средств разработчика F-12. Если не отображается в браузере мобильного **Главная**, **динамика**, **тега**, и **даты** ссылки как кнопки, ссылки на jQuery Mobile сценарии и файлы CSS, скорее всего, не подходят.


Помимо изменения стиля, вы видите **отображение представление для мобильных устройств** и ссылку, которая позволяет переключиться с представлением для мобильных устройств, чтобы представление рабочего стола. Выберите **представление рабочего стола** отображается ссылка и представление рабочего стола.

[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)

Представление рабочего стола не позволяют напрямую перейти обратно в представление для мобильных устройств. Мы исправим это теперь. Откройте *представления\общие\\_Layout.cshtml* файла. Под страницы `body` элемента, добавьте следующий код, который выполняет визуализацию представления переключателя мини-приложения:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample14.cshtml)]

Обновить *AllTags* Просмотр в браузере мобильных устройств. Теперь можно переходить между представлениями для настольных компьютеров и мобильных устройств.

[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)

> [!NOTE]
> Отладка Примечание: можно добавить следующий код в конец одну\\_ViewSwitcher.cshtml могут помочь в отладке представления при на мобильное устройство с помощью браузера строку агента пользователя.
> 
> [!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample15.cs)]
> 
>  и следующий заголовок для добавления *представления\общие\\_Layout.cshtml* файла.  
> 
> [!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample16.html)]


Перейдите к *AllTags* страницы в браузер для настольных компьютеров. Переключатель вида мини-приложение не отображается в браузер на компьютере, поскольку он добавляется только к странице макета для мобильных устройств. Далее в этом учебнике вы увидите, как можно добавить мини переключателя в представлении рабочего стола.

[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)

## <a name="improving-the-speakers-list"></a>Улучшения в списке динамики

В мобильных браузеров выберите **динамики** ссылку. Так как представление для мобильных устройств (*AllSpeakers.Mobile.cshtml*), отображения по умолчанию динамики (*AllSpeakers.cshtml*) отображается с помощью мобильных режиме ( *\_ Layout.Mobile.cshtml*).

[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)

Глобально, представление по умолчанию (не для мобильных устройств) к просмотру в макете мобильных устройств можно отключить, установив `RequireConsistentDisplayMode` для `true` в *представления\\файл _ViewStart.cshtml* файл следующим образом:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample17.cshtml)]

При `RequireConsistentDisplayMode` равно `true`, мобильных макета (*\_Layout.Mobile.cshtml*) используется только для мобильных представлений. (Файл представления является формы ***ViewName**. Mobile.cshtml*). Может потребоваться задать `RequireConsistentDisplayMode` для `true` Если мобильных макет плохо работает с вашим представлениям не для мобильных устройств. На снимке экрана ниже показано как *динамики* при отображении страницы `RequireConsistentDisplayMode` равно `true`.

[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)

Согласованный режим отображения в представлении можно отключить, установив `RequireConsistentDisplayMode` для `false` в файле представления. Приведенный ниже код в *Views\Home\AllSpeakers.cshtml* файл задает `RequireConsistentDisplayMode` для `false`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample18.cshtml)]

## <a name="creating-a-mobile-speakers-view"></a>Создание представления динамики мобильных устройств

Как вы только что увидели *динамики* представление доступен для чтения, но ссылки невелики и трудно коснитесь на мобильном устройстве. В этом разделе вы создадите конкретного mobile *динамики* представления, который выглядит как современных мобильных приложений — он отображает большой, чтобы коснитесь ссылки и содержит поле поиска для быстрого поиска динамики.

Копировать *AllSpeakers.cshtml* для *AllSpeakers.Mobile.cshtml*. Откройте *AllSpeakers.Mobile.cshtml* файл и удалите `<h2>` элемента заголовка.

В `<ul>` , добавьте `data-role` атрибут и присвойте ему значение `listview`. Как и другие [ `data-*` атрибуты](http://html5doctor.com/html5-custom-data-attributes/), `data-role="listview"` делает проще большой список элементов для отвода. Вот как выглядит полная разметка.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample19.cshtml)]

Обновите браузер и мобильных устройств. Обновленное представление выглядит следующим образом:

[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)

Несмотря на то, что представление для мобильных устройств была улучшена, и громоздкими длинный список динамики. Чтобы устранить эту проблему, в `<ul>` , добавьте `data-filter` атрибут и присвойте ему значение `true`. Ниже показан код `ul` разметки.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample20.html)]

На следующем рисунке показаны поля фильтра поиска в верхней части страницы, полученный в результате `data-filter` атрибута.

[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)

При вводе каждой буквы в поле поиска jQuery Mobile фильтры списка, как показано на рисунке ниже.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)

## <a name="improving-the-tags-list"></a>Улучшение списка тегов

Значение по умолчанию, например *динамики* представление, *теги* представление доступен для чтения, но ссылки являются небольшой и трудно коснитесь на мобильном устройстве. В этом разделе можно исправить *теги* следующим образом: вы фиксированной *динамики* представления.

Удалить &quot;скрыть&quot; суффикс *Views\Home\AllTags.Mobile.cshtml.hide* файл, чтобы имя *Views\Home\AllTags.Mobile.cshtml*. Откройте переименованный файл и удалите `<h2>` элемента.

Добавить `data-role` и `data-filter` атрибуты `<ul>` тег, как показано ниже:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample21.html)]

На следующем рисунке показана страница теги, фильтрация по буквы `J`.

[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)

## <a name="improving-the-dates-list"></a>Улучшение списка дат

Можно улучшить *даты* просмотреть, как повышение *динамики* и *теги* представления, чтобы было проще использовать на мобильном устройстве.

Копировать *Views\Home\AllDates.cshtml* файл *Views\Home\AllDates.Mobile.cshtml*. Откройте новый файл и удалите `<h2>` элемента.

Добавить `data-role="listview"` для `<ul>` тег следующим образом:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample22.html)]

На следующем рисунке показано, что **даты** страницы выглядит как с `data-role` атрибут на месте.

[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png) заменить содержимое *Views\Home\AllDates.Mobile.cshtml* файла следующим кодом:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample23.cshtml)]

Этот код группирует все сеансы по дням. Разделитель списка создается для каждого нового дня, и в нем перечислены все сеансы для каждого дня в группе разделитель. Вот, что он будет выглядеть при запуске этого кода:

[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)

## <a name="improving-the-sessionstable-view"></a>Повышение SessionsTable представление

В этом разделе вы создадите представления мобильных сеансов. Изменения, вносимые мы будет более сложные, чем в других представлениях, которые мы создали.

В мобильных браузеров коснитесь **динамика** , а затем введите `Sc` в поле поиска.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)

Коснитесь **Скотт Хансельман** ссылку.

[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)

Как видите, отображение трудно читать на мобильный браузер. Столбец даты трудно читать и столбцом тегов из представления. Чтобы устранить эту проблему, скопируйте *Views\Home\SessionsTable.cshtml* для *Views\Home\SessionsTable.Mobile.cshtml*, а затем замените содержимое файла следующим кодом:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample24.cshtml)]

Код удаляет комнаты и теги столбцы и по вертикали, форматирует заголовок, динамика и даты, чтобы эта информация доступна для чтения на мобильных браузеров. На рисунке ниже отражает изменения в коде.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)

## <a name="improving-the-sessionbycode-view"></a>Повышение SessionByCode представление

Наконец, вы создадите mobile определенного вида *SessionByCode* представления. В мобильных браузеров коснитесь **динамика** , а затем введите `Sc` в поле поиска.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)

Коснитесь **Скотт Хансельман** ссылку. Отображаются Скотт Хансельман сеансы.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)

Выберите **Обзор MS Web стека из любовь** ссылку.

[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)

Представление рабочего стола по умолчанию неплохо, но его можно увеличить.

Копировать *Views\Home\SessionByCode.cshtml* для *Views\Home\SessionByCode.Mobile.cshtml* и замените содержимое *Views\Home\SessionByCode.Mobile.cshtml*файл с следующую разметку:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample25.cshtml)]

Новый разметка использует `data-role` атрибут, чтобы улучшить макет представления.

Обновите браузер и мобильных устройств. На рисунке отражает только что внесенные изменения в коде:

[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)

## <a name="wrapup-and-review"></a>Обзор и — Сводка

Этот учебник добавлены новые мобильные функции ASP.NET MVC 4 Developer Preview. Возможности мобильных устройств:

- Возможность переопределения макета, представления и частичные представления, как глобальный, так и в отдельном окне.
- Контроль над макетом и частичное переопределения с помощью принудительной `RequireConsistentDisplayMode` свойство.
- Переключатель вида мини-приложения для мобильных устройств представления не могут также отображаться в представлениях рабочего стола.
- Поддержка для поддержки определенных обозревателей, например iPhone браузер.

## <a name="see-also"></a>См. также

- [jQuery Mobile](http://jquerymobile.com) сайта.
- [jQuery Mobile Обзор](http://jquerymobile.com/demos/1.0b3/docs/about/intro.html)
- [Рекомендации приложений мобильных веб-технологий рекомендация W3C](http://www.w3.org/TR/mwabp/)
- [Кандидат с рекомендацией W3C для медиа-запросами](http://www.w3.org/TR/css3-mediaqueries/)
