---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Использование CAPTCHA, чтобы предотвратить использование веб-ASP.NET Razor программами-роботами) сайта | Документация Майкрософт
author: microsoft
description: В этой статье описывается использование ReCaptcha (меры безопасности), чтобы предотвратить выполнение задач в веб-страниц ASP.NET (Razor) автоматическими программами (программами-роботами) мы...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2012
ms.topic: article
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: cfdc8561f8ee4b97cc1ef2111113846a1c579d5c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389076"
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>Использование CAPTCHA, чтобы предотвратить использование веб-ASP.NET Razor программами-роботами) сайта
====================
по [Microsoft](https://github.com/microsoft)

> В этой статье описываются способы использования ReCaptcha (меры безопасности), чтобы предотвратить выполнение задач в на веб-сайте ASP.NET Web Pages (Razor) автоматическими программами (программами-роботами).
> 
> **Вы узнаете, как:** 
> 
> - Как добавить на сайт тест CAPTCHA.
> 
> Ниже приведены функции ASP.NET, представленные в этой статье.
> 
> - `ReCaptcha` Вспомогательный.
> 
> > [!NOTE]
> > Сведения в этой статье относятся к веб-страниц ASP.NET 1.0 и Web Pages 2.


## <a name="about-captchas"></a>О CAPTCHAs

Любое время, вы сотрудники могут легко зарегистрировать на сайте, или даже просто введите имя и URL-адрес (как и для блог комментарий), то можно получить поток ненастоящие. Они часто остаются с автоматическими программами (программами-роботами), которые пытаются привести каждого веб-узла, который можно найти URL-адреса. (Распространенный мотив — post URL-адреса продуктов для продажи.)

Помочь убедитесь в том, что пользователь является человек, а не программой компьютера с помощью *CAPTCHA* для проверки подлинности пользователей при регистрации или в противном случае введите свое имя и сайта. CAPTCHA означает теста полностью автоматизировать общих Тьюринга о компьютерах и люди друг от друга. — Тест CAPTCHA *ответ на запрос* теста, в котором пользователю предлагается сделать нечто, легко лицу для выполнения, но трудно сделать автоматизированной программой. Это наиболее распространенный тип CAPTCHA, где некоторые искаженные буквы см. в разделе и будет предложено ввести их. (Искажение предполагается затруднить программ-роботов расшифровать буквы.)

## <a name="adding-a-recaptcha-test"></a>Добавление теста ReCaptcha

На страницах ASP.NET, можно использовать `ReCaptcha` вспомогательный метод для отрисовки тест CAPTCHA, основанный на службе ReCaptcha ([http://recaptcha.net](http://recaptcha.net)). `ReCaptcha` Вспомогательный объект отображает изображение два искаженные слов, которые пользователям нужно правильно ввести перед проверкой страницы. Ответ пользователя проверяется службой ReCaptcha.Net.

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. Регистрация веб-сайта в ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). После завершения регистрации вы получите открытый ключ и закрытый ключ.
2. Добавьте библиотеку ASP.NET на веб-сайт, как описано в [Установка вспомогательных функций на сайте веб-страниц ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), если у вас еще нет.
3. Если у вас нет  *\_AppStart.cshtml* в корневой папке веб-сайта, создайте файл с именем  *\_AppStart.cshtml*.
4. Добавьте следующий `Recaptcha` вспомогательные параметры в  *\_AppStart.cshtml* файла: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. Задайте `PublicKey` и `PrivateKey` свойства с помощью собственных открытых и закрытых ключей.
6. Сохранить  *\_AppStart.cshtml* файл и закройте его.
7. В корневой папке веб-сайта, создайте новую страницу с именем *Recaptcha.cshtml*.
8. Замените существующее содержимое следующим: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. Запустите *Recaptcha.cshtml* страницу в браузере. Если `PrivateKey` значение является допустимым, на странице отображается элемент управления ReCaptcha и кнопки. Если вы не задали ключи глобально в  *\_AppStart.html*, на странице будет отображаться ошибка. 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. Введите слова для теста. Если вы прошли Испытание на ReCaptcha, вы увидите сообщение об ошибке. В противном случае отображается сообщение об ошибке и отобразится элемент управления ReCaptcha.

> [!NOTE]
> Если компьютер находится в домене, который использует прокси-сервер, может потребоваться настроить `defaultproxy` элемент *Web.config* файл. В следующем примере показан *Web.config* файл с `defaultproxy` элемент, настроены для включения ReCaptcha для работы службы.
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Дополнительные ресурсы


- [Настройка поведения сайта для веб-страниц узлов ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)
- [ReCaptcha сайта](https://www.google.com/recaptcha)
