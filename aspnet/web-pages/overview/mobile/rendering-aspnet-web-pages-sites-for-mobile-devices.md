---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Отрисовки ASP.NET Web Pages (Razor) сайтов для мобильных устройств | Документация Майкрософт
author: tfitzmac
description: 'В этой статье описывается создание страниц на сайте веб-страниц ASP.NET (Razor), будут отображаться правильно на мобильных устройствах. Вы узнаете, как: как вы...'
ms.author: aspnetcontent
ms.date: 02/17/2014
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: dcc44e0d78814ef9a8537b01efa17259a12e9c76
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816354"
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a>Подготовка к просмотру ASP.NET Web Pages (Razor) сайтов для мобильных устройств
====================
по [Tom FitzMacken](https://github.com/tfitzmac)

> В этой статье описывается создание страниц на сайте веб-страниц ASP.NET (Razor), будут отображаться правильно на мобильных устройствах.
> 
> Вы узнаете, как:
> 
> - Как использовать соглашения об именовании позволяет указать, что страницы разработан специально для мобильных устройств.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемые в этом руководстве
> 
> 
> - Веб-страниц ASP.NET (Razor) 3
>   
> 
> Этот учебник также работает с ASP.NET Web Pages 2.


Веб-страницы ASP.NET позволяет создавать пользовательские отображение для отображения содержимого на мобильный или других устройств.

Самый простой способ создать страницу конкретного устройства, на сайте веб-страниц ASP.NET, — с помощью шаблона именования файлов следующим образом: <em>FileName.</em> <em>Mobile</em><em>.cshtml</em>. Можно создать две версии страницы (например, один с именем <em>MyFile.cshtml</em> и один с именем <em>MyFile.Mobile.cshtml</em>). Во время выполнения, когда мобильное устройство запрашивает <em>MyFile.cshtml</em>, ASP.NET отображает содержимое из <em>MyFile.Mobile.cshtml</em>. В противном случае <em>MyFile.cshtml</em> визуализации.

Приведенный ниже показано, как включить отрисовку мобильных устройств, добавив страницу содержимого для мобильных устройств. *Page1.cshtml* содержит содержимое, а также боковая панель навигации. *Page1.Mobile.cshtml* с тем же содержимым, но пропускает боковой панели.

1. На сайте веб-страниц ASP.NET, создайте файл с именем *Page1.cshtml* и замените текущее содержимое следующей разметкой.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. Создайте файл с именем *Page1.Mobile.cshtml* и замените существующее содержимое следующей разметкой. Обратите внимание на то, что мобильную версию страницы пропускает раздел перехода для повышения подготовки к просмотру на экране меньшего размера.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. Запустите классический браузер и перейдите к *Page1.cshtml*. ![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)
4. Запустите браузер мобильного устройства (или эмулятор мобильных устройств) и перейдите к *Page1.cshtml*. (Обратите внимание, что отсутствует *.mobile.* как часть URL-адрес.) Несмотря на то, что этот запрос поступает в *Page1.cshtml*, ASP.NET отображает *Page1.Mobile.cshtml*.

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> Чтобы протестировать страницах для мобильных устройств, можно использовать имитатор мобильных устройств, который выполняется на настольном компьютере. Эта программа предназначена для тестирования веб-страниц, так как они выглядели на мобильных устройствах (то есть обычно намного меньше с область отображается). Одним из примеров симулятор является [надстройки переключателя между агентом пользователя](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) для Mozilla Firefox, которая позволяет моделировать различные браузеры для мобильных устройств из версии Firefox для настольного компьютера.


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Дополнительные ресурсы


[Эмулятор Windows Phone](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)
