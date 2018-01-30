---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: "Предотвращение атак путем перенаправления Open (C#) | Документы Microsoft"
author: jongalloway
description: "В этом учебнике описано, как может помешать атак путем перенаправления с открытым в приложениях ASP.NET MVC. Этот учебник описывает изменения, которые были сделаны..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: 17944c0600a174176e3e9940f414b34f0835b800
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
<a name="preventing-open-redirection-attacks-c"></a>Предотвращение атак путем перенаправления Open (C#)
====================
по [Джон Гэллоуэй](https://github.com/jongalloway)

> В этом учебнике описано, как может помешать атак путем перенаправления с открытым в приложениях ASP.NET MVC. Этот учебник описываются изменения, внесенные в AccountController в ASP.NET MVC 3 и показано, как применять эти изменения в существующие ASP.NET 1.0 MVC и 2 приложений.


## <a name="what-is-an-open-redirection-attack"></a>Что такое Атака перенаправления открыть?

Все веб-приложения, которая перенаправляет на URL-адреса, указанного с помощью запроса, например данные строки запроса или формы потенциально могут быть подменены для перенаправления пользователей на внешний, вредоносные URL-адрес. Это изменение данных называется атака откройте перенаправления.

Каждый раз, когда указанный URL-адрес перенаправляет логики приложения, необходимо убедиться, что URL-адрес перенаправления не было изменено. Имя входа, используемое в AccountController по умолчанию для MVC ASP.NET 1.0 и ASP.NET MVC 2 уязвим для атак путем перенаправления открыть. К счастью можно легко обновить существующие приложения, чтобы воспользоваться исправлениями из предварительной версии ASP.NET MVC 3.

Чтобы понять уязвимость, давайте взглянем на как работает перенаправления входа в проекте веб-приложение ASP.NET MVC 2 по умолчанию. В этом приложении попытке посетите действия контроллера, в котором есть атрибут [Authorize] будет перенаправлять неавторизованных пользователей к представлению /Account/LogOn. Это применимо к /Account/LogOn должен включать параметр строки запроса returnUrl, чтобы пользователь могут быть возвращены первоначально запрошенному URL-адресу после их успешного входа.

На снимке экрана ниже мы видим, попытка доступа к представлению /Account/ChangePassword, если не выполнен результатам при перенаправлении /Account/LogOn? ReturnUrl = % 2fAccount % 2fChangePassword % 2f.

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

**На рисунке 01**: страницы входа с помощью открытые перенаправления

Так как параметр строки запроса ReturnUrl не проверяется, злоумышленник может изменить его внедрить любой URL-адрес в качестве параметра провести атаку откройте перенаправления. Чтобы продемонстрировать это, можно изменить параметр ReturnUrl [http://bing.com](http://bing.com), таким образом, полученный URL-адрес входа или учетной записи и входа в систему? ReturnUrl = http://www.bing.com/. После успешного входа в систему на сайт, то будут перенаправлены в [http://bing.com](http://bing.com). Так как это перенаправление является недопустимым, он может вместо точки вредоносный сайт, в которой предпринимается попытка сбить с толку пользователя.

### <a name="a-more-complex-open-redirection-attack"></a>Более сложные откройте Атака перенаправления

Атак путем перенаправления откройте особенно опасны, поскольку злоумышленник знает, что мы пытаемся войти определенного веб-узла, что делает нам уязвимым для [фишинга](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx). Например злоумышленник может отправить вредоносные сообщения электронной почты для пользователей веб-сайта при попытке записать свои пароли. Давайте посмотрим, как это будет работать на сайте обновление NerdDinner. (Обратите внимание, на действующем сайте обновление NerdDinner был обновлен для защиты от атак путем перенаправления с открытым.)

Во-первых он отправляет нам ссылку на страницу входа на обновление NerdDinner, который включает перенаправление на своей странице поддельного:

[http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

Обратите внимание, что URL-адрес возврата указывает nerddiner.com, в которой отсутствует «n» из word dinner. В этом примере это домен, он управляет. Если мы обращаются ссылку выше, мы будут представлены допустимые NerdDinner.com страницы входа.

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

**На рисунке 02**: обновление NerdDinner страницу входа с помощью открытые перенаправления

Когда мы правильно войти, действия входа ASP.NET MVC AccountController перенаправляет нам URL-адрес, указанный в параметре строки запроса returnUrl. В данном случае это URL-адрес, он укажет, что является [http://nerddiner.com/Account/LogOn](http://nerddiner.com/Account/LogOn). Если мы очень watchful, это скорее всего мы не заметить этого, особенно потому, что он был осторожность, чтобы убедиться, что их поддельного страница выглядит так же, как страницы законный входа. Эта страница входа — это сообщение Ошибка, запрос, что мы войти снова. Неловкий (США), мы ввели пароль.

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

**На рисунке 03**: экран входа подделать обновление NerdDinner

Когда мы заново, нашей имя пользователя и пароль, на страницу входа поддельного сохраняет сведения и отправляет нам на законных NerdDinner.com веб-узел. На этом этапе NerdDinner.com сайта уже проверку подлинности (США), так можно перенаправить на страницу входа поддельного непосредственно на эту страницу. Конечным результатом является злоумышленник получает нашей имя пользователя и пароль, что мы не знают, что мы предоставляем его для них.

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a>Просмотрев уязвимого кода в действии входа AccountController

Ниже приведен код для действия входа в приложение ASP.NET MVC 2. Обратите внимание, что после успешного входа, возвращает ли контроллер перенаправление на returnUrl. Вы увидите, проверка не выполняется для параметра returnUrl.

**1 — ASP.NET MVC 2 входа действие в список`AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

Теперь давайте взглянем на изменения в ASP.NET MVC 3 входа действия. Этот код был изменен для проверки параметра returnUrl путем вызова нового метода System.Web.Mvc.Url вспомогательный класс с именем `IsLocalUrl()`.

**2 — ASP.NET MVC 3 входа действие в список`AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

Были внесены изменения для проверки возвращаемого параметра URL-адреса путем вызова нового метода в классе вспомогательный System.Web.Mvc.Url `IsLocalUrl()`.

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a>Защита MVC 2 и ASP.NET MVC 1.0 приложений

Мы можно воспользоваться преимуществами изменения ASP.NET MVC 3 в наши существующие ASP.NET MVC 1.0 и 2 приложения путем добавления вспомогательный метод IsLocalUrl() и выполняется обновление действия входа для проверки параметра returnUrl.

Метод UrlHelper IsLocalUrl(), просто вызов метода в System.Web.WebPages, как эта проверка также используется приложениями веб-страниц ASP.NET.

**Листинг 3 – метод IsLocalUrl() из ASP.NET MVC 3 UrlHelper`class`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

Метод IsUrlLocalToHost содержит логики проверки, как показано в листинге 4.

**Листинг 4 – метод IsUrlLocalToHost() класса System.Web.WebPages RequestExtensions**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

В нашем ASP.NET MVC 1.0 или 2 приложение мы добавим метод IsLocalUrl() AccountController, но вы еще настоятельно рекомендуется, чтобы добавить отдельный вспомогательный класс, если это возможно. Мы сделает два небольшие изменения для версии ASP.NET MVC 3 IsLocalUrl(), чтобы она работала внутри AccountController. Во-первых мы изменим его из открытого метода в закрытый метод, так как открытые методы контроллеры доступны как действия контроллера. Во-вторых мы изменим проверки URL-адрес узла для узла приложения. Что вызова использует локальный RequestContext в класс UrlHelper. Вместо этого. RequestContext.HttpContext.Request.Url.Host, мы воспользуемся ею. Request.Url.Host. Следующий код показывает измененный метод IsLocalUrl() для использования с классом контроллер в ASP.NET MVC 1.0 и 2 приложений.

**Список 5 — метод IsLocalUrl() которого изменяется для использования с классом контроллер MVC**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

Теперь, когда метод IsLocalUrl() на месте, мы можно вызвать из наших действия входа для проверки параметра returnUrl, как показано в следующем коде.

**Перечисление 6 – Updated входа метод, который проверяет параметр returnUrl**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

Теперь можно проверить атака откройте перенаправления, попробуйте выполнить вход с использованием URL-адрес внешнего возврата. Воспользуемся/Account/входа? ReturnUrl = http://www.bing.com/ еще раз.

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

**На рисунке 04**: для проверки обновленного действия входа в систему

После успешного входа в систему, мы перенаправляется на действие контроллера Home/Index, а не внешнего URL-адреса.

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

**На рисунке 05**: Откройте перенаправления атаки изменило

## <a name="summary"></a>Сводка

Атак путем перенаправления откройте может возникнуть, когда перенаправления URL-адреса, передаются как параметры в URL-адрес для приложения. Шаблон включает в себя код, чтобы обеспечить защиту от ASP.NET MVC 3 откройте атак путем перенаправления. Можно добавить этот код с внесения некоторых изменений в ASP.NET MVC 1.0 и 2 приложений. Чтобы защититься от атак путем перенаправления с открытым, при входе в ASP.NET версии 1.0 и 2 приложения, добавьте метод IsLocalUrl() и проверки параметра returnUrl в действии входа в систему.
