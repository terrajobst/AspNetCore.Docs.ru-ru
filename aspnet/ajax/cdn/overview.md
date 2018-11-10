---
uid: ajax/cdn/overview
title: Сеть доставки содержимого Microsoft Ajax | Документация Майкрософт
author: rick-anderson
description: ''
ms.author: riande
ms.date: 10/14/2017
ms.assetid: 8935bf14-ca6d-4a4e-9dbe-b96ce74cef49
msc.legacyurl: /ajax/cdn
msc.type: content
ms.openlocfilehash: 0da6955a6062571d917fb8c9651417fe834ad34f
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912660"
---
<a name="microsoft-ajax-content-delivery-network"></a>Сеть доставки содержимого Microsoft Ajax
====================
> [!WARNING]
> Рабочие приложения не должен принимать жесткие зависимости на ресурсов CDN. Приложения должны проверить ссылки на средства CDN и использовать резервный ресурс, если CDN недоступен. 
>
> Сети доставки Содержимого Microsoft Ajax есть соглашение кода с использованием Azure CDN.
>
> Используйте [проблема GitHub](https://github.com/aspnet/Docs/issues/5832) сообщить о проблемах с помощью сети доставки Содержимого Microsoft Ajax.

## <a name="table-of-contents"></a>Содержание

**[переименован в ajax.aspnetcdn.com AJAX.Microsoft.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**  
**[Поддержка .vsdoc Visual Studio](#Visual_Studio_vsdoc_Support_19)**  
**[С помощью ASP.NET Ajax из сети CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**  
**[С помощью jQuery из сети CDN](#Using_jQuery_from_the_CDN_21)**  
**[С помощью jQuery пользовательского интерфейса из сети CDN](#Using_jQuery_UI_from_the_CDN_22)**  
**[Файлы в сети доставки Содержимого](#Third-Party_Files_on_the_CDN_23)**  
  
 [Выпусков jQuery в сети доставки Содержимого](#jQuery_Releases_on_the_CDN_0)  
 [Перенос выпусков jQuery, в сети доставки Содержимого](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [jQuery выпуски пользовательского интерфейса в сети доставки Содержимого](#jQuery_UI_Releases_on_the_CDN_2)  
 [jQuery выпуски проверки в сети доставки Содержимого](#jQuery_Validation_Releases_on_the_CDN_3)  
 [jQuery Mobile выпусков в сети доставки Содержимого](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [jQuery шаблоны выпусков в сети доставки Содержимого](#jQuery_Templates_Releases_on_the_CDN_5)  
 [jQuery цикла выпусков в сети доставки Содержимого](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [jQuery DataTables выпусков в сети доставки Содержимого](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [Выпуски Modernizr в сети доставки Содержимого](#Modernizr_Releases_on_the_CDN_8)  
 [Выпуски JSHint в сети доставки Содержимого](#JSHint_Releases_on_the_CDN_10)  
 [Выпуски Knockout в сети доставки Содержимого](#Knockout_Releases_on_the_CDN_11)  
 [Глобализация выпусков в сети доставки Содержимого](#Globalize_Releases_on_the_CDN_12)  
 [Ответ выпусков в сети доставки Содержимого](#Respond_Releases_on_the_CDN_13)  
 [Выпуски начальной загрузки в сети доставки Содержимого](#Bootstrap_Releases_on_the_CDN_14)  
 [Выпуски TouchCarousel начальной загрузки в сети доставки Содержимого](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [Выпуски Hammer.js в сети доставки Содержимого](#Hammerjs_Releases_on_the_CDN_19)  
 [Веб-форм ASP.NET и Ajax выпусков в сети доставки Содержимого](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [Освобождает ASP.NET MVC в сети доставки Содержимого](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [Освобождает ASP.NET SignalR в сети доставки Содержимого](#ASPNET_SignalR_Releases_on_the_CDN_17)

Microsoft Ajax доставки содержимого сети (CDN) размещает популярных сторонних библиотек JavaScript, например jQuery и позволяет легко добавлять их к веб-приложениям. Например, можно запустить с помощью jQuery, которая размещается в этой сети доставки Содержимого путем простого добавления &lt;скрипт&gt; тег на страницу, которая указывает ajax.aspnetcdn.com.

Используя преимущества сети CDN, может значительно повысить производительность приложений Ajax. Содержимое CDN кэшируется на серверах, расположенных по всему миру. Кроме того сеть позволяет браузерам повторно использовать кэшированные сторонних файлов JavaScript для веб-сайтов, которые находятся в разных доменах.

CDN поддерживает SSL (HTTPS), при необходимости для обслуживания веб-страницы, используя протокол SSL.

CDN размещает следующие библиотеки сценария третьих лиц, которых были переданы и пользователя есть лицензия, владельцами этих библиотек:

- jQuery (www.jquery.com)
- пользовательский Интерфейс (www.jqueryui.com) jQuery
- jQuery Mobile (www.jquerymobile.com)
- jQuery Validation (www.jquery.com)
- Подключаемый модуль jQuery Cycle (www.malsup.com/jquery/cycle/)
- jQuery DataTables (http://datatables.net/)

Сети доставки Содержимого Microsoft Ajax также включает в себя следующие библиотеки, которые были переданы корпорацией Майкрософт:

- ASP.NET Ajax
- Файлы JavaScript в ASP.NET MVC
- Файлы ASP.NET SignalR JavaScript

Microsoft не предъявляет прав собственности на любые сторонние библиотеки, размещенных в этой сети CDN. Владельцам авторских прав, библиотек Лицензирование эти библиотеки для вас. Все права, которые необходимо загрузить и использовать такие библиотеки предоставляются исключительно с владельцев авторских прав. Так как это не библиотеки корпорации Майкрософт, корпорация Майкрософт предоставляет не сопровождаются никакими лицензии права интеллектуальной собственности (включая не подразумеваемых патентные права) для сторонних библиотек, размещенных в этой сети CDN.

Если вы хотите отправить библиотеки JavaScript и библиотеки является одним из верхней библиотеки JavaScript (как указано на http://trends.builtwith.com) или расширений или подключаемых модулей для этих библиотек, которые являются (a) популярных; или (б) полезно для использования в ASP.NET, а затем обратитесь в службу AjaxCDNSubmission@Microsoft.com.

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a>переименован в ajax.aspnetcdn.com AJAX.Microsoft.com

CDN позволяет использовать имя домена microsoft.com, а также был изменен для использования имени домена aspnetcdn.com. Это изменение было внесено для повышения производительности, поскольку при обращении к домена microsoft.com браузер отправляет все файлы cookie из этого домена по каналу связи с каждым запросом. Путем переименования домена имя, отличное от microsoft.com производительности можно увеличить, возможную на 25%. Обратите внимание на то, ajax.microsoft.com будет продолжать работать, но рекомендуется ajax.aspnetcdn.com.

- Старый формат: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js
- Новый формат: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a>Поддержка .vsdoc Visual Studio

Использовать файлы .vsdoc должным образом с помощью Visual Studio 2008, необходимо убедиться, что у вас есть VS 2008 SP1 установлены и было установлено исправление для vsdoc файлов. Их можно получить здесь:

- [Скачайте Visual Studio 2008 SP1](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "скачать Visual Studio 2008 с пакетом обновления 1")
- [Загрузки .vsdoc исправления для Visual Studio 2008 SP1](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "загрузки .vsdoc исправления для Visual Studio 2008 с пакетом обновления 1")

Visual Studio 2010 поддерживает файлы .vsdoc без любые дополнительные исправления.

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a>С помощью ASP.NET Ajax из сети CDN

При использовании ASP.NET 4, можно перенаправлять все запросы на сценарии ASP.NET framework в CDN. Получение сценариев из сети CDN, вместо локального веб-сервера может существенно повысить производительность общедоступных веб-сайтов ASP.NET.

Используйте свойство ScriptManager EnableCDN для перенаправления всех запросов скриптов ASP.NET framework в Microsoft Ajax CDN:

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a>С помощью jQuery из сети CDN

Можно использовать сценарии jQuery, размещенной в сети доставки Содержимого в веб-приложения путем добавления следующего элемента сценария на страницу:

[!code-html[Main](overview/samples/sample2.html)]

CDN также включает в себя минифицированные версию jQuery скрипт, который можно получить с помощью следующего элемента:

[!code-html[Main](overview/samples/sample3.html)]

Чтобы разрешить страницу, чтобы возврат к загрузке jQuery из локальный путь на свой веб-сайт, если CDN недоступен, добавьте следующий элемент сразу после элемента, ссылающегося на CDN:

[!code-html[Main](overview/samples/sample4.html)]

На следующей странице образец использует CDN версию библиотеки jQuery (с резервным подключением локальную копию) для отображения содержимого элемента div, при нажатии кнопки.

[!code-html[Main](overview/samples/sample5.html)]

Можно узнать больше о jQuery и загрузить локальную копию jQuery, посетив [jQuery](http://jquery.com/) веб-сайта.

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a>С помощью jQuery пользовательского интерфейса из сети CDN

CDN также содержит библиотеки jQuery UI. Библиотека пользовательского интерфейса jQuery включает широкий набор мини-приложений и эффекты, которые можно использовать в приложениях ASP.NET. Например следующая страница иллюстрирует, как jQuery Datepicker пользовательского интерфейса в контексте приложения веб-форм ASP.NET можно использовать для отображения всплывающего календаря:

[!code-aspx[Main](overview/samples/sample6.aspx)]

При перемещении фокуса в текстовое поле, с помощью клавиатуры, отображается календарь:

![Всплывающего календаря, созданных с помощью Datepicker](overview/_static/image1.png)

Обратите внимание на то, что должен включать три файла из сети CDN в приведенном выше коде:

- Библиотека jQuery &mdash; библиотеки jQuery UI зависит от библиотеки jQuery. Библиотека jQuery необходимо добавить на страницу перед добавлением библиотеки jQuery UI.
- Библиотека пользовательского интерфейса jQuery &mdash; библиотеки jQuery UI содержит все эффекты пользовательского интерфейса jQuery и мини-приложений, таких как Datepicker мини-приложения, используемые в странице выше.
- Тема пользовательского интерфейса jQuery &mdash; пользовательский Интерфейс jQuery поддерживает различные темы. На странице выше ссылка на CSS-файл для импорта Redmond темы.

Все темы пользовательского интерфейса standard jQuery размещаются в сети доставки Содержимого. [Эта страница содержит](jquery-ui/cdnjqueryui1910.md "пользовательский Интерфейс jQuery 1.8.10 в сети доставки Содержимого Microsoft Ajax") для просмотра эскизов для каждой темы.

Дополнительные сведения о библиотеке пользовательского интерфейса jQuery, посетите официальный [веб-сайт пользовательского интерфейса jQuery](http://jQueryUI.com "веб-сайт пользовательского интерфейса jQuery").

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a>Файлы в сети доставки Содержимого

CDN размещает некоторые из наиболее популярных библиотек JavaScript третьих лиц. Microsoft не предъявляет прав собственности на любые сторонние библиотеки, размещенных в этой сети CDN. Владельцам авторских прав, библиотек Лицензирование эти библиотеки для вас. Все права, которые необходимо загрузить и использовать такие библиотеки предоставляются исключительно с владельцев авторских прав. Так как это не библиотеки корпорации Майкрософт, корпорация Майкрософт предоставляет не сопровождаются никакими лицензии права интеллектуальной собственности (включая не подразумеваемых патентные права) для сторонних библиотек, размещенных в этой сети CDN.

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a>Выпусков jQuery в сети доставки Содержимого

Следующие версии jQuery размещаются в сети доставки Содержимого:

#### <a name="jquery-version-331"></a>версия jQuery 3.3.1
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map

#### <a name="jquery-version-321"></a>jQuery версии 3.2.1
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map

#### <a name="jquery-version-320"></a>jQuery версии 3.2.0

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map

#### <a name="jquery-version-311"></a>версия jQuery 3.1.1

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map

#### <a name="jquery-version-310"></a>jQuery версии 3.1.0

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map

#### <a name="jquery-version-300"></a>jQuery версии 3.0.0

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map

#### <a name="jquery-version-224"></a>версия jQuery 2.2.4

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map

#### <a name="jquery-version-223"></a>jQuery версии 2.2.3

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map

#### <a name="jquery-version-222"></a>jQuery версии 2.2.2

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map

#### <a name="jquery-version-221"></a>версия jQuery 2.2.1

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map

#### <a name="jquery-version-220"></a>jQuery версии 2.2.0

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map

#### <a name="jquery-version-214"></a>jQuery версия 2.1.4

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map

#### <a name="jquery-version-213"></a>jQuery версии 2.1.3

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map

#### <a name="jquery-version-212"></a>jQuery версии 2.1.2

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js

#### <a name="jquery-version-211"></a>версия jQuery 2.1.1

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map

#### <a name="jquery-version-210"></a>jQuery версии 2.1.0

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map

#### <a name="jquery-version-203"></a>jQuery версии 2.0.3

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map

#### <a name="jquery-version-202"></a>jQuery версии 2.0.2

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map

#### <a name="jquery-version-201"></a>jQuery версии 2.0.1

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map

#### <a name="jquery-version-200"></a>jQuery версии 2.0.0

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map

#### <a name="jquery-version-1124"></a>версия jQuery 1.12.4

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map

#### <a name="jquery-version-1123"></a>версия jQuery 1.12.3

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map

#### <a name="jquery-version-1122"></a>версия jQuery 1.12.2

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map

#### <a name="jquery-version-1121"></a>версия jQuery 1.12.1

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map

#### <a name="jquery-version-1120"></a>версия jQuery 1.12.0

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map

#### <a name="jquery-version-1113"></a>версия jQuery 1.11.3

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map

#### <a name="jquery-version-1112"></a>версия jQuery 1.11.2

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map

#### <a name="jquery-version-1111"></a>версия jQuery 1.11.1

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map

#### <a name="jquery-version-1110"></a>jQuery версии 1.11.0

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map

#### <a name="jquery-version-1102"></a>версия jQuery 1.10.2

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map

#### <a name="jquery-version-1101"></a>версия jQuery 1.10.1

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map

#### <a name="jquery-version-1100"></a>версия jQuery 1.10.0

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map

#### <a name="jquery-version-191"></a>версия jQuery 1.9.1

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map

#### <a name="jquery-version-190"></a>версия jQuery 1.9.0

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map

#### <a name="jquery-version-183"></a>версия jQuery 1.8.3

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js

#### <a name="jquery-version-182"></a>версия jQuery 1.8.2

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js

#### <a name="jquery-version-181"></a>jQuery версии 1.8.1

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js

#### <a name="jquery-version-180"></a>jQuery версии 1.8.0

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js

#### <a name="jquery-version-172"></a>версия jQuery 1.7.2

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js

#### <a name="jquery-version-171"></a>версия jQuery 1.7.1

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js

#### <a name="jquery-version-17"></a>версия jQuery 1.7

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js

#### <a name="jquery-version-164"></a>версия jQuery 1.6.4

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js

#### <a name="jquery-version-163"></a>версия jQuery 1.6.3

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js

#### <a name="jquery-version-162"></a>версия jQuery 1.6.2

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js

#### <a name="jquery-version-161"></a>jQuery версии 1.6.1

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js

#### <a name="jquery-version-16"></a>jQuery версии 1.6

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js

#### <a name="jquery-version-152"></a>версия jQuery 1.5.2

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js

#### <a name="jquery-version-151"></a>версия jQuery 1.5.1

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js

#### <a name="jquery-version-15"></a>версии jQuery 1.5

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js

#### <a name="jquery-version-144"></a>версия jQuery 1.4.4

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js

#### <a name="jquery-version-143"></a>версия jQuery 1.4.3

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js

#### <a name="jquery-version-142"></a>версия jQuery 1.4.2

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js

#### <a name="jquery-version-141"></a>версия jQuery 1.4.1

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js

#### <a name="jquery-version-14"></a>jQuery версии 1.4

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js

#### <a name="jquery-version-132"></a>jQuery версия 1.3.2

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a>Перенос выпусков jQuery, в сети доставки Содержимого

Следующие версии jQuery миграции размещаются в сети доставки Содержимого:

#### <a name="jquery-migrate-version-300"></a>jQuery миграции версии 3.0.0

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js

#### <a name="jquery-migrate-version-121"></a>jQuery версии 1.2.1 "Миграция"

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js

jQuery версии 1.2.0 "Миграция"

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js

#### <a name="jquery-migrate-version-111"></a>jQuery версии 1.1.1 "Миграция"

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js

#### <a name="jquery-migrate-version-110"></a>jQuery версии 1.1.0 "Миграция"

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js

#### <a name="jquery-migrate-version-100"></a>jQuery версии 1.0.0 "Миграция"

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a>jQuery выпуски пользовательского интерфейса в сети доставки Содержимого

Следующие версии библиотеки jQuery UI размещаются в этой сети доставки Содержимого. Щелкните каждый ссылку, чтобы просмотреть фактический список файлов.

- [пользовательский Интерфейс jQuery 1.12.1](jquery-ui/cdnjqueryui1121.md "пользовательский Интерфейс jQuery 1.12.1 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.12.0](jquery-ui/cdnjqueryui1120.md "пользовательский Интерфейс jQuery 1.12.0 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.11.4](jquery-ui/cdnjqueryui1114.md "пользовательский Интерфейс jQuery 1.11.4 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.11.3](jquery-ui/cdnjqueryui1113.md "пользовательский Интерфейс jQuery 1.11.3 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.11.2](jquery-ui/cdnjqueryui1112.md "пользовательский Интерфейс jQuery 1.11.2 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.11.1](jquery-ui/cdnjqueryui1111.md "пользовательский Интерфейс jQuery 1.11.1 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс 1.11.0 jQuery](jquery-ui/cdnjqueryui1110.md "пользовательский Интерфейс jQuery 1.11.0 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.10.4](jquery-ui/cdnjqueryui1104.md "пользовательский Интерфейс jQuery 1.10.4 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.10.3](jquery-ui/cdnjqueryui1103.md "пользовательский Интерфейс jQuery 1.10.3 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.10.2](jquery-ui/cdnjqueryui1102.md "пользовательский Интерфейс jQuery 1.10.2 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.10.1](jquery-ui/cdnjqueryui1101.md "пользовательский Интерфейс jQuery 1.10.1 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.10.0](jquery-ui/cdnjqueryui1100.md "пользовательский Интерфейс jQuery 1.10.0 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.9.2](jquery-ui/cdnjqueryui192.md "пользовательский Интерфейс jQuery 1.9.2 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.9.1](jquery-ui/cdnjqueryui191.md "пользовательский Интерфейс jQuery 1.9.1 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.9.0](jquery-ui/cdnjqueryui190.md "пользовательский Интерфейс jQuery 1.9.0 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.8.24](jquery-ui/cdnjqueryui1824.md "пользовательский Интерфейс jQuery 1.8.24 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.8.23](jquery-ui/cdnjqueryui1823.md "пользовательский Интерфейс jQuery 1.8.23 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.8.22](jquery-ui/cdnjqueryui1822.md "пользовательский Интерфейс jQuery 1.8.22 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.8.21](jquery-ui/cdnjqueryui1821.md "пользовательский Интерфейс jQuery 1.8.21 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.8.20](jquery-ui/cdnjqueryui1820.md "пользовательский Интерфейс jQuery 1.8.20 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.8.19](jquery-ui/cdnjqueryui1819.md "пользовательский Интерфейс jQuery 1.8.19 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.8.18](jquery-ui/cdnjqueryui1818.md "пользовательский Интерфейс jQuery 1.8.18 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.8.17](jquery-ui/cdnjqueryui1817.md "пользовательский Интерфейс jQuery 1.8.17 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.8.16](jquery-ui/cdnjqueryui1816.md "пользовательский Интерфейс jQuery 1.8.16 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.8.15](jquery-ui/cdnjqueryui1815.md "пользовательский Интерфейс jQuery 1.8.15 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.8.14](jquery-ui/cdnjqueryui1814.md "пользовательский Интерфейс jQuery 1.8.14 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.8.13](jquery-ui/cdnjqueryui1813.md "пользовательский Интерфейс jQuery 1.8.13 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.8.12](jquery-ui/cdnjqueryui1812.md "пользовательский Интерфейс jQuery 1.8.12 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.8.11](jquery-ui/cdnjqueryui1811.md "пользовательский Интерфейс jQuery 1.8.11 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.8.10](jquery-ui/cdnjqueryui1910.md "пользовательский Интерфейс jQuery 1.8.10 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.8.9](jquery-ui/cdnjqueryui189.md "пользовательский Интерфейс jQuery 1.8.9 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.8.8](jquery-ui/cdnjqueryui188.md "пользовательский Интерфейс jQuery 1.8.8 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.8.7](jquery-ui/cdnjqueryui187.md "пользовательский Интерфейс jQuery 1.8.7 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.8.6](jquery-ui/cdnjqueryui186.md "пользовательский Интерфейс jQuery 1.8.6 в сети доставки Содержимого Microsoft Ajax")
- [пользовательский Интерфейс jQuery 1.8.5](jquery-ui/cdnjqueryui185.md "пользовательский Интерфейс jQuery 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a>jQuery выпуски проверки в сети доставки Содержимого

Следующие версии библиотеки проверки jQuery размещаются в этой сети доставки Содержимого. Щелкните каждый ссылку, чтобы просмотреть фактический список файлов.

- [Подключаемый модуль jQuery Validate 1.17.0](jquery-validate/cdnjqueryvalidate1170.md "проверки 1.17.0 jQuery")
- [Подключаемый модуль jQuery Validate версии 1.16.0](jquery-validate/cdnjqueryvalidate1160.md "jQuery Validate 1.16.0")
- [Подключаемый модуль jQuery Validate 1.15.1](jquery-validate/cdnjqueryvalidate1151.md "jQuery Validate 1.15.1")
- [Подключаемый модуль jQuery Validate 1.15.0](jquery-validate/cdnjqueryvalidate1150.md "jQuery Validate 1.15.0")
- [Подключаемый модуль jQuery Validate 1.14.0](jquery-validate/cdnjqueryvalidate1140.md "jQuery Validate 1.14.0")
- [Подключаемый модуль jQuery Validate 1.13.1](jquery-validate/cdnjqueryvalidate1131.md "jQuery Validate 1.13.1")
- [Подключаемый модуль jQuery Validate 1.13.0](jquery-validate/cdnjqueryvalidate1130.md "проверки 1.13.0 jQuery")
- [Подключаемый модуль jQuery Validate 1.12.0](jquery-validate/cdnjqueryvalidate1120.md "jQuery 1.12.0 проверки")
- [Подключаемый модуль jQuery Validate 1.11.1](jquery-validate/cdnjqueryvalidate1111.md "jQuery 1.11.1 проверки")
- [Подключаемый модуль jQuery Validate 1.11.0](jquery-validate/cdnjqueryvalidate111.md "проверки 1.11.0 jQuery")
- [Подключаемый модуль jQuery Validate 1.10.0](jquery-validate/cdnjqueryvalidate110.md "jQuery 1.10.0 проверки")
- [Подключаемый модуль jQuery Validate 1.9](jquery-validate/cdnjqueryvalidate19.md "jquery.validate версии 1.9")
- [Подключаемый модуль jQuery Validate 1.8.1](jquery-validate/cdnjqueryvalidate181.md "jquery.validate версии 1.8.1")
- [Подключаемый модуль jQuery Validate 1.8](jquery-validate/cdnjqueryvalidate18.md "jquery.validate версии 1.8")
- [Подключаемый модуль jQuery Validate 1.7](jquery-validate/cdnjqueryvalidate17.md "jquery.validate версии 1.7")
- [Подключаемый модуль jQuery Validate 1.6](jquery-validate/cdnjqueryvalidate16.md "подключаемый модуль jQuery Validate 1.6")
- [Подключаемый модуль jQuery Validate 1.5.5](jquery-validate/cdnjqueryvalidate155.md "подключаемый модуль jQuery Validate 1.5.5.")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a>jQuery Mobile выпусков в сети доставки Содержимого

Следующие версии библиотеки jQuery мобильных размещаются в этой сети доставки Содержимого. Щелкните каждый ссылку, чтобы просмотреть фактический список файлов.

- [jQuery Mobile 1.4.5](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 в сети доставки Содержимого Microsoft Ajax")
- [jQuery Mobile 1.4.2](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 в сети доставки Содержимого Microsoft Ajax")
- [jQuery Mobile 1.4.1](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 в сети доставки Содержимого Microsoft Ajax")
- [jQuery Mobile 1.4.0](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 в сети доставки Содержимого Microsoft Ajax")
- [jQuery Mobile 1.3.2](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 в сети доставки Содержимого Microsoft Ajax")
- [jQuery Mobile 1.3.1](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 в сети доставки Содержимого Microsoft Ajax")
- [jQuery Mobile 1.3.0](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 в сети доставки Содержимого Microsoft Ajax")
- [jQuery Mobile 1.2.0](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 в сети доставки Содержимого Microsoft Ajax")
- [jQuery Mobile 1.1.2](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 в сети доставки Содержимого Microsoft Ajax")
- [jQuery Mobile 1.1.1](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 в сети доставки Содержимого Microsoft Ajax")
- [jQuery Mobile 1.1.0](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 в сети доставки Содержимого Microsoft Ajax")
- [jQuery Mobile 1.1.0 версия-Кандидат 2](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 в сети доставки Содержимого Microsoft Ajax")
- [jQuery Mobile 1.0.1](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 в сети доставки Содержимого Microsoft Ajax")
- [jQuery Mobile 1.0](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 в сети доставки Содержимого Microsoft Ajax")
- [jQuery Mobile 1.0 версии-Кандидата 2](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 в сети доставки Содержимого Microsoft Ajax")
- [jQuery Mobile 1.0 версии-Кандидата 1](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 в сети доставки Содержимого Microsoft Ajax")
- [jQuery Mobile 1.0 бета-версия 3](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 бета-версии 3 в сети доставки Содержимого Microsoft Ajax")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a>jQuery шаблоны выпусков в сети доставки Содержимого

Следующие версии подключаемого модуля jQuery шаблоны размещаются в этой сети доставки Содержимого. Щелкните каждый ссылку, чтобы просмотреть фактический список файлов.

- [jQuery шаблоны бета-версия 1](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery шаблоны бета-версии 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a>jQuery цикла выпусков в сети доставки Содержимого

Следующие версии подключаемого модуля цикл jQuery размещаются в этой сети доставки Содержимого. Щелкните каждый ссылку, чтобы просмотреть фактический список файлов.

- [jQuery цикла 2.99](jquery-cycle/cdnjquerycycle299.md "jQuery цикла 2.99")
- [jQuery цикла 2.94](jquery-cycle/cdnjquerycycle294.md "jQuery 2.94 цикла")
- [jQuery 2,88 цикла](jquery-cycle/cdnjquerycycle288.md "jQuery 2,88 цикла")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a>jQuery DataTables выпусков в сети доставки Содержимого

В следующих выпусках подключаемый модуль jQuery DataTables размещаются в этой сети доставки Содержимого. Щелкните каждый ссылку, чтобы просмотреть фактический список файлов.

- [jQuery DataTables 1.10.5](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [jQuery DataTables 1.10.4](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [jQuery DataTables 1.9.4](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [jQuery DataTables 1.9.3](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [jQuery DataTables 1.9.2](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [jQuery DataTables 1.9.1](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [jQuery DataTables 1.9.0](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [jQuery DataTables 1.8.2](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a>Выпуски Modernizr в сети доставки Содержимого

Следующие выпуски [Modernizr](http://www.modernizr.com "Modernizr") размещаются в сети доставки Содержимого:

- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-3.5.0.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a>Выпуски JSHint в сети доставки Содержимого

Следующие выпуски [JSHint](http://www.jshint.com "JSHint") размещаются в сети доставки Содержимого:

- https://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a>Выпуски Knockout в сети доставки Содержимого

Следующие выпуски [Knockout](http://www.knockoutjs.com "Knockout") размещаются в сети доставки Содержимого:

- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a>Глобализация выпусков в сети доставки Содержимого

Следующие выпуски [Globalize](https://github.com/jquery/globalize "Globalize") размещаются в сети доставки Содержимого:

#### <a name="globalize-version-100"></a>Глобализация версии 1.0.0

- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js

#### <a name="globalize-version-011"></a>Глобализация версии 0.1.1

- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js

    - Все языки и региональные параметры
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js

    - Замените «{языка и региональных параметров — код}» с кодом нужного языка и региональных параметров, например Microsoft globalize.culture.en GB.js== файлов в CDN == эти библиотеки были отправлены корпорацией Майкрософт.

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a>Ответ выпусков в сети доставки Содержимого

Следующие выпуски [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") ответ размещаются в сети доставки Содержимого:

#### <a name="respond-version-142"></a>Ответ версии 1.4.2

- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js

#### <a name="respond-version-141"></a>Ответ версии 1.4.1

- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js

#### <a name="respond-version-140"></a>Ответ версии 1.4.0

- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js

#### <a name="respond-version-130"></a>Ответ версии 1.3.0

- https://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js

#### <a name="respond-version-120"></a>Ответ версии 1.2.0

- https://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a>Выпуски начальной загрузки в сети доставки Содержимого

Следующие выпуски [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap размещаются в сети доставки Содержимого:

#### <a name="bootstrap-version-411"></a>Начальной загрузки версии 4.1.1

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-400"></a>Начальной загрузки версии 4.0.0

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-337"></a>Начальной загрузки версии 3.3.7

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-336"></a>Начальной загрузки версии 3.3.6

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-335"></a>Начальной загрузки версии 3.3.5

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-334"></a>Начальной загрузки версии 3.3.4

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-332"></a>Начальной загрузки версии 3.3.2

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-331"></a>Начальной загрузки версии 3.3.1

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-330"></a>Начальной загрузки версии 3.3.0

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-320"></a>Начальной загрузки версии 3.2.0

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-311"></a>Начальной загрузки версии 3.1.1

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-310"></a>Начальной загрузки версии 3.1.0

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-303"></a>Начальной загрузки версии 3.0.3

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-302"></a>Начальной загрузки версии 3.0.2

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-301"></a>Начальной загрузки версии 3.0.1

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-300"></a>Начальной загрузки версии 3.0.0

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-232"></a>Начальной загрузки версии 2.3.2

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png

#### <a name="bootstrap-version-231"></a>Начальной загрузки версии 2.3.1

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a>Выпуски TouchCarousel начальной загрузки в сети доставки Содержимого

Следующие выпуски [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel выпуски размещаются в сети доставки Содержимого:

#### <a name="bootstrap-touchcarousel-version-080"></a>TouchCarousel начальной загрузки версии 0.8.0

- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css
- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a>Выпуски Hammer.js в сети доставки Содержимого

Следующие выпуски [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js выпуски размещаются в сети доставки Содержимого:

#### <a name="hammerjs-version-204"></a>Hammer.js версии 2.0.4

- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a>Веб-форм ASP.NET и Ajax выпусков в сети доставки Содержимого

Следующие версии ASP.NET Ajax Library размещаются в сети доставки Содержимого. Щелкните каждый ссылку, чтобы просмотреть фактический список файлов.

- [Версии веб-форм ASP.NET и Ajax 4.5.2](cdnajax452.md "веб-форм ASP.NET и Ajax 4.5.2")
- [Версии веб-форм ASP.NET и Ajax 4](cdnajax4.md "веб-форм ASP.NET и Ajax 4")
- [Ajax для ASP.NET версии 3.5](cdnajax35.md "Ajax для ASP.NET 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a>Освобождает ASP.NET MVC в сети доставки Содержимого

Следующие файлы ASP.NET MVC JavaScript, размещенных в этой сети доставки Содержимого:

#### <a name="aspnet-mvc-523"></a>ASP.NET MVC 5.2.3

- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-51"></a>ASP.NET MVC 5.1

- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-50"></a>ASP.NET MVC 5.0

- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-40"></a>ASP.NET MVC 4.0

- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-30"></a>ASP.NET MVC 3.0

- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js
- https://ajax.aspnetcdn.com/ajax/jquery.unobtrusive-ajax/3.2.5/jquery.unobtrusive-ajax.js
- https://ajax.aspnetcdn.com/ajax/jquery.unobtrusive-ajax/3.2.5/jquery.unobtrusive-ajax.min.js 
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js
- https://ajax.aspnetcdn.com/ajax/jquery.validation.unobtrusive/3.2.10/jquery.validate.unobtrusive.js 
- https://ajax.aspnetcdn.com/ajax/jquery.validation.unobtrusive/3.2.10/jquery.validate.unobtrusive.min.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-20"></a>ASP.NET MVC 2.0

- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-10"></a>ASP.NET MVC 1.0

- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a>Освобождает ASP.NET SignalR в сети доставки Содержимого

Следующие файлы ASP.NET SignalR JavaScript размещаются в этой сети доставки Содержимого:

#### <a name="aspnet-signalr-222"></a>ASP.NET SignalR 2.2.2

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js

#### <a name="aspnet-signalr-221"></a>ASP.NET SignalR 2.2.1

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js

#### <a name="aspnet-signalr-220"></a>ASP.NET SignalR 2.2.0

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js

#### <a name="aspnet-signalr-210"></a>ASP.NET SignalR 2.1.0

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js

#### <a name="aspnet-signalr-203"></a>ASP.NET SignalR 2.0.3

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js

#### <a name="aspnet-signalr-202"></a>ASP.NET SignalR 2.0.2

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js

#### <a name="aspnet-signalr-201"></a>ASP.NET SignalR 2.0.1

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js

#### <a name="aspnet-signalr-200"></a>ASP.NET SignalR 2.0.0

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js

#### <a name="aspnet-signalr-113"></a>ASP.NET SignalR 1.1.3

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js

#### <a name="aspnet-signalr-112"></a>ASP.NET SignalR 1.1.2

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js

#### <a name="aspnet-signalr-111"></a>ASP.NET SignalR 1.1.1

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js

#### <a name="aspnet-signalr-110"></a>ASP.NET SignalR 1.1.0

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js

#### <a name="aspnet-signalr-101"></a>ASP.NET SignalR 1.0.1

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js

Сведения об условиях использования сети CDN, см. в разделе [Microsoft Ajax CDN условия использования](https://www.asp.net/terms-of-use "Microsoft Ajax CDN условия использования").
