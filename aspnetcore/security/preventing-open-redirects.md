---
title: Предотвращение атак с открытым перенаправлением в ASP.NET Core
author: ardalis
description: Показывает, как предотвратить атаки открытого перенаправления в приложении ASP.NET Core
ms.author: riande
ms.date: 07/07/2017
uid: security/preventing-open-redirects
ms.openlocfilehash: 9d8cac8708fe9aeadba5af1287362a20df7f6bfe
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78652222"
---
# <a name="prevent-open-redirect-attacks-in-aspnet-core"></a>Предотвращение атак с открытым перенаправлением в ASP.NET Core

Если веб-приложение перенаправляет пользователя на URL-адрес, указанный по запросу, например в строке запроса или через данные формы, этот URL-адрес может быть подделан и вести на внешний вредоносный сайт. Такое изменение данных называется атакой открытого перенаправления.

Каждый раз, когда логика вашего приложения выполняет перенаправление на определенный URL-адрес, нужно убедиться, что этот адрес не был изменен. ASP.NET Core содержит встроенные функциональные возможности, помогающие защитить приложения от атак методом открытого перенаправления/переадресации.

## <a name="what-is-an-open-redirect-attack"></a>Что такое атака с открытым перенаправлением?

Веб-приложения часто перенаправляют пользователей на страницу входа при доступе к ресурсам, требующим проверки подлинности. Перенаправление обычно включает параметр `returnUrl` QueryString, чтобы пользователь мог вернуться к первоначально запрошенному URL-адресу после успешного входа. После проверки подлинности пользователь перенаправляется на URL-адрес, по которому они были изначально запрошены.

Поскольку конечный URL-адрес указан в строке запроса, злоумышленник может незаконно изменить ее. Измененный запрос позволит сайту перенаправить пользователя на внешний вредоносный сайт. Этот прием называется атакой открытого перенаправления.

### <a name="an-example-attack"></a>Пример атаки

Злоумышленник может разработать атаку, которая позволит ему получить доступ к учетным или конфиденциальным данным пользователя. Чтобы начать атаку, злоумышленник убедится в том, что пользователь должен щелкнуть ссылку на страницу входа на ваш сайт с `returnUrl` значением строки запроса, добавленным к URL-адресу. Например, рассмотрим приложение на `contoso.com`, которое содержит страницу входа в `http://contoso.com/Account/LogOn?returnUrl=/Home/About`. Атака включает следующие шаги:

1. Пользователь щелкает вредоносную ссылку на `http://contoso.com/Account/LogOn?returnUrl=http://contoso1.com/Account/LogOn` (второй URL-адрес — contoso**1**. com, а не "contoso.com").
2. Пользователь успешно входит в систему.
3. Пользователь перенаправляется (сайт) на `http://contoso1.com/Account/LogOn` (вредоносный сайт, который выглядит точно так же, как и реальный сайт).
4. Пользователь пытается войти еще раз (предоставляя вредоносному сайту свои учетные данные) и перенаправляется на настоящий сайт.

Пользователь скорее всего думает, что ему просто не удалось войти с первой попытки, и не подозревает, что его учетные данные скомпрометированы.

![Процесс атаки открытого перенаправления](preventing-open-redirects/_static/open-redirection-attack-process.png)

Помимо страниц для входа, некоторые сайты предоставляют страницы перенаправления или конечные точки. Представьте, что приложение содержит страницу с открытым перенаправлением, `/Home/Redirect`. Злоумышленник может создать, например, ссылку в сообщении электронной почты с `[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`. Обычный пользователь будет просматривать URL-адрес и видеть, что он начинается с имени вашего сайта. Увидев, что этот URL-адрес начинается с названия вашего веб-сайта, обычный пользователь, ни о чем не подозревая, щелкнет ссылку. Открытое перенаправление затем переведет его на фишинговый сайт, который выглядит в точности как ваш, и пользователь, скорее всего, введет на нем свои учетные данные.

## <a name="protecting-against-open-redirect-attacks"></a>Защита от открытого перенаправления

При разработке веб-приложений следует считать все предоставляемые пользователями данные не заслуживающими доверия. Если приложение содержит функции, перенаправляющие пользователя на основе содержимого URL-адреса, перенаправление должно осуществляться только локально внутри приложения (или строго на известный URL, а не на любой адрес в строке запроса).

### <a name="localredirect"></a>локалредирект

Используйте вспомогательный метод `LocalRedirect` из базового класса `Controller`:

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

`LocalRedirect` выдаст исключение, если указан нелокальный URL-адрес. В противном случае он ведет себя так же, как метод `Redirect`.

### <a name="islocalurl"></a>ислокалурл

Используйте метод [ислокалурл](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper.islocalurl#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) для проверки URL-адресов перед перенаправлением:

В следующем примере показано, как проверить, является ли URL-адрес локальным, перед перенаправлением.

```csharp
private IActionResult RedirectToLocal(string returnUrl)
{
    if (Url.IsLocalUrl(returnUrl))
    {
        return Redirect(returnUrl);
    }
    else
    {
        return RedirectToAction(nameof(HomeController.Index), "Home");
    }
}
```

Метод `IsLocalUrl` защищает пользователей от случайного перенаправления к вредоносному сайту. Рекомендуем фиксировать сведения об URL-адресе в ситуациях, когда вместо ожидаемого локального URL предоставляется нелокальный. Это поможет в диагностике использующих перенаправление атак.
