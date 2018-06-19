---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Сайт с помощью CAPTCHA, чтобы предотвратить использование вашего веб-ASP.NET Razor программы-роботы) | Документы Microsoft
author: microsoft
description: В этой статье объясняется, как использовать ReCaptcha (меры безопасности), чтобы предотвратить выполнение задач в веб-страниц ASP.NET (Razor) автоматические программы (программы-роботы) мы...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2012
ms.topic: article
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: 75e80a3e7ebe787852152404bf2e0bf88a1a6a56
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
ms.locfileid: "26529923"
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>Чтобы предотвратить использование вашего веб-ASP.NET Razor программы-роботы с помощью CAPTCHA) сайта
====================
по [Microsoft](https://github.com/microsoft)

> В этой статье описывается использование ReCaptcha (меры безопасности), чтобы предотвратить выполнение задач в на веб-сайт ASP.NET Web Pages (Razor) автоматических программ (программы-роботы).
> 
> **Что вы узнаете следующее.** 
> 
> - Как добавить тест CAPTCHA в веб-узла.
> 
> Существуют следующие функции ASP.NET, представленные в статье:
> 
> - `ReCaptcha` Вспомогательные.
> 
> > [!NOTE]
> > Информация в этой статье относится к веб-страниц ASP.NET 1.0 и 2 веб-страниц.


## <a name="about-captchas"></a>О CAPTCHAs

Каждый раз, предоставляется возможность регистрации пользователей на сайте или даже просто введите имя и URL-адрес (как для блог комментарий), может появиться наводнения фиктивное имен. Они часто влево на автоматические программы (программы-роботы), пытающиеся оставьте URL-адреса в каждом веб-сайта, можно найти. (Общие причины является для учета URL-адреса товаров для продажи).

Вы можете помочь убедитесь в том, что пользователь является человек, а не программой компьютера с помощью *CAPTCHA* для проверки пользователей при их регистрации или в противном случае введите свое имя и сайта. CAPTCHA означает теста полностью автоматизировать открытый Тьюринга о компьютерах и люди расстоянии друг от друга. — CAPTCHA *запрос ответ* теста, в котором пользователю будет предложено сделать что-нибудь легко лицу для выполнения, но трудно автоматизированной программой для выполнения. Наиболее распространенный тип CAPTCHA, где некоторые искаженный буквы в разделе и будет предложено ввести их. (Искажение должен усложняют программы-роботы расшифровать буквы.)

## <a name="adding-a-recaptcha-test"></a>Добавление теста ReCaptcha

На страницах ASP.NET, можно использовать `ReCaptcha` вспомогательный метод для отображения тест CAPTCHA, который основан на службе ReCaptcha ([http://recaptcha.net](http://recaptcha.net)). `ReCaptcha` Вспомогательный объект отображает изображение два искаженный слов, которые пользователи должны ввести правильно перед проверкой страницы. Ответ пользователя проверяется службой ReCaptcha.Net.

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. Регистрация веб-сайта в ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). После завершения регистрации, вы сможете получить открытый ключ и закрытый ключ.
2. Добавить библиотека вспомогательных методов ASP.NET Web веб-сайта, как описано в [Установка помощников в узел веб-страниц ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), если это еще не сделано.
3. Если у вас еще нет  *\_AppStart.cshtml* в корневой папке веб-сайта, создайте файл с именем  *\_AppStart.cshtml*.
4. Добавьте следующие `Recaptcha` вспомогательный параметры в  *\_AppStart.cshtml* файла: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. Задать `PublicKey` и `PrivateKey` свойства с помощью собственных открытый и закрытый ключи.
6. Сохранить  *\_AppStart.cshtml* файл и закройте его.
7. В корневой папке веб-сайта, создайте новую страницу с именем *Recaptcha.cshtml*.
8. Замените существующее содержимое следующим кодом: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. Запустите *Recaptcha.cshtml* страницы в браузере. Если `PrivateKey` значение допустимо, на странице отображается элемент управления ReCaptcha и кнопки. Если вы не задали ключи глобально доступны в  *\_AppStart.html*, страница может отображаться ошибка. 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. Введите слова для теста. Если вы прошли проверку ReCaptcha, появится сообщение об ошибке. В противном случае отображается сообщение об ошибке, и отобразится ReCaptcha элемента управления.

> [!NOTE]
> Если компьютер находится в домене, которая использует прокси-сервер, может потребоваться настроить `defaultproxy` элемент *Web.config* файла. В следующем примере показан *Web.config* файл с `defaultproxy` настроены для включения должна работать служба ReCaptcha элемента.
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Дополнительные ресурсы


- [Настройка поведения сайта для веб-страниц узлов ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)
- [ReCaptcha сайта](https://www.google.com/recaptcha)
