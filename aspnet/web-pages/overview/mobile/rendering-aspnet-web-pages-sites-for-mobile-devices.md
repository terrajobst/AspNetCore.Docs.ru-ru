---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Визуализации ASP.NET Web Pages сайтов (Razor) для мобильных устройств | Документы Microsoft
author: tfitzmac
description: 'В этой статье описывается создание страниц на сайте ASP.NET Web Pages (Razor), будет отображаться соответствующим образом на мобильных устройствах. Вы узнаете: как вы...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: 641798c5b835be959d02dd0d854b61ca21d83016
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a>Подготовка к просмотру веб-страниц (Razor) сайтов ASP.NET для мобильных устройств
====================
по [Tom FitzMacken](https://github.com/tfitzmac)

> В этой статье описывается создание страниц на сайте ASP.NET Web Pages (Razor), будет отображаться соответствующим образом на мобильных устройствах.
> 
> Что вы узнаете следующее.
> 
> - Как использовать соглашение об именовании позволяет указать, что страница разработан специально для мобильных устройств.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемая в этом учебнике
> 
> 
> - Веб-страниц ASP.NET (Razor) 3
>   
> 
> Этот учебник также работает с веб-страницы ASP.NET 2.


Веб-страниц ASP.NET позволяет создавать настраиваемого отображения для отображения содержимого для мобильных устройств или на других устройствах.

— Самый простой способ создать страницу конкретного устройства в узел веб-страниц ASP.NET с помощью имени файла шаблона, следующим образом: <em>FileName.</em> <em>Mobile</em><em>.cshtml</em>. Можно создать две версии страницы (например, один с именем <em>MyFile.cshtml</em> и один с именем <em>MyFile.Mobile.cshtml</em>). Во время выполнения, когда мобильное устройство запрашивает <em>MyFile.cshtml</em>, ASP.NET отображает содержимое, полученное от <em>MyFile.Mobile.cshtml</em>. В противном случае <em>MyFile.cshtml</em> подготавливается к просмотру.

Приведенный ниже показано, как включить подготовки мобильных устройств, добавив страницу содержимого для мобильных устройств. *Page1.cshtml* включает содержимое, а также боковая панель навигации. *Page1.Mobile.cshtml* с тем же содержимым, но пропускает боковой панели.

1. На сайте веб-страниц ASP.NET, создайте файл с именем *Page1.cshtml* и замените следующую текущего содержимого.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. Создайте файл с именем *Page1.Mobile.cshtml* и заменить существующее содержимое следующей разметкой. Обратите внимание, что версия мобильной страницы пропускает раздел навигации для повышения подготовки к просмотру на экране меньшего размера.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. Запустить браузер для настольных компьютеров и перейдите к папке *Page1.cshtml*. ![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)
4. Мобильный браузер (или его эмулятор) и перейдите к *Page1.cshtml*. (Обратите внимание, что не следует включать *.mobile.* как часть URL-адреса). Несмотря на то, что этот запрос поступает *Page1.cshtml*, ASP.NET отображает *Page1.Mobile.cshtml*.

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> Для проверки страниц для мобильных устройств, можно использовать симулятор мобильного устройства, запускаемую на настольном компьютере. Это средство позволяет тестировать веб-страницы, как они выглядели на мобильных устройствах (то есть, обычно с намного меньше область). Является одним из примеров симулятора [надстройки переключателя агента пользователя](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) для Mozilla Firefox, который позволяет моделировать различные браузеры для мобильных устройств с рабочего стола версии Firefox.


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Дополнительные ресурсы


[Эмулятор Windows Phone](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)
