---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Вспомогательный модуль с веб-страниц ASP.NET Twitter | Документы Microsoft
author: tfitzmac
description: Этого раздела и приложения показано, как добавить в проект WebMatrix 3 вспомогательный модуль Twitter. Он содержит код вспомогательный модуль Twitter и показан вызов вспомогательного...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 07d9c4d485c42b78a42c54c9740af5f67cb44763
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/25/2018
---
<a name="twitter-helper-with-aspnet-web-pages"></a>Вспомогательное приложение Twitter с помощью веб-страниц ASP.NET
====================
по [Tom FitzMacken](https://github.com/tfitzmac)

> Этого раздела и приложения показано, как добавить в проект WebMatrix 3 вспомогательный модуль Twitter. Он содержит код вспомогательный модуль Twitter и показано, как вызвать вспомогательных методов.
> 
> Этот код для файла Twitter.cshtml был разработан **Tian панорамирование** корпорации Майкрософт.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемая в этом учебнике
> 
> 
> - Веб-страниц ASP.NET (Razor) 3
>   
> 
> Этот учебник также работает с веб-страницы ASP.NET 2.


## <a name="introduction"></a>Вступление

В этом разделе показано, как добавить вспомогательный модуль Twitter в приложение и использовать синтаксис Razor для вызова вспомогательных методов. Вспомогательный модуль Twitter упрощает для включения кнопки Twitter и мини-приложения в приложении. Для использования в Twitter мини-приложении, например шкалы пользователя или результатах поиска для хэштегом, необходимо сначала создать [мини-приложение в Twitter](https://twitter.com/settings/widgets). После создания мини-приложения, вы получите идентификатор мини-приложения. При вызове вспомогательные методы, которые показывают мини-приложения можно передать в качестве параметра этот идентификатор мини-приложения.

В этом разделе была написана для версии 1.1 Twitter API. Непосредственное добавление Twitter вспомогательного кода в проект, можно обновить вспомогательного кода при изменении Twitter API.

Сведения об установке WebMatrix см. в разделе [введение в ASP.NET Web Pages 2 — начало работы](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).

## <a name="add-twitter-helper-to-your-project"></a>Добавьте в проект вспомогательный модуль Twitter

Чтобы добавить вспомогательный модуль Twitter, сначала добавьте папку с именем **приложения\_код** в проект. Создайте файл с именем **Twitter.cshtml**.

![Папки App_Code](twitter-helper/_static/image1.png)

Замените код по умолчанию в Twitter.cshtml следующим кодом.

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a>Вызов методов Twitter из веб-страниц

Приведенный ниже показано, как использовать методы вспомогательный модуль Twitter со страницы в проекте. В проекте необходимо заменить значения параметров со значениями, которые относятся к вашим потребностям. Идентификаторы предоставленный мини-приложения можно использовать для изучения того, как методы работают, но требуется создавать собственные мини-приложения для проекта.

Не все параметры, приведенные ниже, являются обязательными. Необязательные параметры используются для настройки отображения кнопки и мини-приложения. Например кнопка выполните требуется только имя пользователя, выполните, но в примере показано входят: номер последователи и как указать размер кнопки и языка.

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a>Просмотреть результаты

Этот код создает следующие кнопки и мини-приложения. Эти кнопки и мини-приложений могут полностью функциональные, не снимки экрана. Кнопка выполните отображается на испанском языке, так как имеет значение параметра language **es**.

### <a name="follow-button"></a>Выполните кнопки

[Выполните @aspnet)](https://twitter.com/aspnet)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>

### <a name="tweet-button"></a>Кнопка твит

[Твиты](https://twitter.com/share)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>

### <a name="user-timeline-profile"></a>Временная шкала пользователя (профиль)

[Твиты @aspnet](https://twitter.com/aspnet)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>

### <a name="favorites"></a>Избранное

[Избранного Твиты по @Microsoft](https://twitter.com/Microsoft/favorites)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>

### <a name="list"></a>Список

[Твиты из @Microsoft/MS \_потребителя\_делений](https://twitter.com/microsoft/ms-consumer-brands/)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>

### <a name="search"></a>Поиск

[Твитов о &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>
