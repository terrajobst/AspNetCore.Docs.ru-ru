---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Вспомогательный модуль с помощью ASP.NET Web Pages Twitter | Документация Майкрософт
author: tfitzmac
description: Этот раздел и приложения показано, как добавить вспомогательный модуль Twitter для WebMatrix 3 проекта. Он содержит код вспомогательный модуль Twitter и показан способ вызова вспомогательного приложения...
ms.author: aspnetcontent
ms.date: 02/07/2014
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 1824b677a7ba96ea6fc5119610725a30d472764e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817659"
---
<a name="twitter-helper-with-aspnet-web-pages"></a>Вспомогательное приложение Twitter с помощью веб-страниц ASP.NET
====================
по [Tom FitzMacken](https://github.com/tfitzmac)

> Этот раздел и приложения показано, как добавить вспомогательный модуль Twitter для WebMatrix 3 проекта. Он содержит код вспомогательный модуль Twitter и демонстрируется вызов вспомогательных методов.
> 
> Этот код для файла Twitter.cshtml была разработана **Tian Pan** корпорации Майкрософт.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемые в этом руководстве
> 
> 
> - Веб-страниц ASP.NET (Razor) 3
>   
> 
> Этот учебник также работает с ASP.NET Web Pages 2.


## <a name="introduction"></a>Вступление

В этом разделе показано, как добавить вспомогательный модуль Twitter для приложения и используйте синтаксис Razor для вызова вспомогательных методов. Вспомогательный модуль Twitter упрощает процесс включения кнопки Twitter и мини-приложения в приложении. Для использования мини-приложения Twitter, например временную шкалу пользователя или результатов поиска по хэштегу, необходимо сначала создать [мини-приложение в Twitter](https://twitter.com/settings/widgets). После создания мини-приложение, вы получите идентификатор мини-приложения. При вызове вспомогательные методы, которые показывают мини-приложения можно передать в качестве параметра этот идентификатор мини-приложения.

Эта статья написана для Twitter API версии 1.1. Путем непосредственного добавления Twitter вспомогательного кода в проект, можно обновить вспомогательный код, при изменении Twitter API.

Сведения об установке WebMatrix, см. в разделе [введение в ASP.NET Web Pages 2 - Приступая к работе](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).

## <a name="add-twitter-helper-to-your-project"></a>Добавьте вспомогательный модуль Twitter в проект

Чтобы добавить вспомогательный модуль Twitter, во-первых, добавьте папку с именем **приложения\_кода** в проект. Затем создайте файл с именем **Twitter.cshtml**.

![Папка App_Code](twitter-helper/_static/image1.png)

Замените код по умолчанию в Twitter.cshtml следующим кодом.

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a>Вызывайте методы Twitter из веб-страниц

Приведенный ниже показано, как использовать методы вспомогательный модуль Twitter из страницы в проекте. В проекте необходимо заменить значения параметров со значениями, которые имеют отношение к вашим потребностям. Можно использовать идентификаторы предоставленный мини-приложение для просмотра, как методы работают, но вам потребуется создать собственные мини-приложения для вашего проекта.

Не все параметры, показанные ниже, являются обязательными. Необязательные параметры позволяют настроить способ отображения кнопки или мини-приложения. Например кнопки выполните требуется только имя пользователя, выполните, но примере показано, как включить количество подписчиков, а также как указать размер кнопки и языка.

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a>Просмотреть результаты

Этот код создает следующие кнопки и мини-приложения. Эти кнопки и мини-приложения могут полностью работоспособно, не снимки экрана. Кнопки выполните отображается на испанском языке, поскольку было присвоено параметра language **es**.

### <a name="follow-button"></a>Выполните кнопки

[Выполните @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`

### <a name="tweet-button"></a>Кнопка твита

[Твит](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`

### <a name="user-timeline-profile"></a>Временная шкала пользователя (профиль)

[Твиты по @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="favorites"></a>Избранное

[Избранные Твитов @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="list"></a>Список

[Твиты из @Microsoft/MS \_потребителя\_делений](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="search"></a>Поиск

[О &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`
