---
title: "Совместное использование файлов cookie между приложениями"
author: rick-anderson
description: "В этом документе объясняется, как стек защиты данных поддерживает совместное использование файлов cookie проверки подлинности между ASP.NET 4.x и приложений ASP.NET Core."
keywords: "Совместное использование Core,ASP.NET,cookies,Interop,cookie ASP.NET"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 9a7aae45-8e21-4c54-950c-3c29df6c1082
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/compatibility/cookie-sharing
ms.openlocfilehash: e92cc81f9362787b7b4bfb44ba26b82182826a59
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="sharing-cookies-between-applications"></a>Совместное использование файлов cookie между приложениями

Веб-сайтов обычно состоят из многих отдельных веб-приложениях, все работает вместе harmoniously. Если разработчику приложения для обеспечения эффективной работы единый вход, они часто понадобятся все различные веб-приложения на узле для совместного использования билетов проверки подлинности между друг с другом.

Для поддержки этого сценария, стека защиты данных позволяет совместное использование проверки подлинности файла cookie Katana и ASP.NET Core билетов проверки подлинности файла cookie.

## <a name="sharing-authentication-cookies-between-applications"></a>Совместное использование файлов cookie проверки подлинности между приложениями

Для совместного использования файлов cookie проверки подлинности между двумя приложениями ASP.NET Core, настройка каждого приложения, которое следует совместно использовать файлы cookie следующим образом.

В вашей настройки метода, используйте CookieAuthenticationOptions, чтобы настроить службу защиты данных файлов cookie и AuthenticationScheme для сопоставления ASP.NET 4.x.

Если вы используете удостоверений:

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
{
    options.Cookies.ApplicationCookie.AuthenticationScheme = "ApplicationCookie";
    var protectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"));
    options.Cookies.ApplicationCookie.DataProtectionProvider = protectionProvider;
    options.Cookies.ApplicationCookie.TicketDataFormat = new TicketDataFormat(protectionProvider.CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", "Cookies", "v2"));
});
```

Если вы используете файлы cookie напрямую:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
});
```
   
`DataProtectionProvider` Требует `Microsoft.AspNetCore.DataProtection.Extensions` пакет NuGet.

При использовании таким образом DirectoryInfo должны указывать на расположение хранилища ключей, специально предназначенные для файлов cookie проверки подлинности. Файл cookie проверки подлинности по промежуточного слоя будет использовать явно предоставленных реализации DataProtectionProvider, который теперь является изолированной из системы защиты данных, используемые в других частях приложения. Имя приложения учитывается (намеренно так, как вы пытаетесь получить несколько приложений, для совместного использования полезных данных).

>[!WARNING]
>Следует подумать о настройке DataProtectionProvider таким образом, что они шифруются при хранении, как в примере ниже.
>
>
>  ```csharp
>  app.UseCookieAuthentication(new CookieAuthenticationOptions
>  {
>      DataProtectionProvider = DataProtectionProvider.Create(
>          new DirectoryInfo(@"c:\shared-auth-ticket-keys\"),
>          configure =>
>          {
>              configure.ProtectKeysWithCertificate("thumbprint");
>          })
>  });
>  ```

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-applications"></a>Совместное использование файлов cookie проверки подлинности между ASP.NET 4.x и приложения ASP.NET Core

Для создания файлов cookie проверки подлинности, которые совместимы с по промежуточного слоя ASP.NET Core файла cookie проверки подлинности можно настроить приложения ASP.NET 4.x, которые используют Katana промежуточного по проверки подлинности файла cookie. Это позволяет поэтапное обновление отдельных приложений большой узел одновременно обеспечивая smooth единого входа на взаимодействие на сайте.

>[!TIP]
> Чтобы узнать, использует ли текущее приложение Katana промежуточного по проверки подлинности файла cookie, существование вызов UseCookieAuthentication в Startup.Auth.cs проекта. Проекты веб-приложений ASP.NET 4.x создан в Visual Studio 2013 и более поздней версии, по умолчанию используют промежуточного по проверки подлинности файла cookie Katana.

> [!NOTE]
> Приложение ASP.NET 4.x необходимо использовать .NET Framework 4.5.1 или более поздней версии, в противном случае необходимые пакеты NuGet не удастся установить.

Для совместного использования файлов cookie проверки подлинности между 4.x приложений ASP.NET и приложений ASP.NET Core, настроить приложение ASP.NET Core, как упоминалось выше, а затем настройте приложений ASP.NET 4.x, выполнив следующие действия.

1.  Установка пакета Microsoft.Owin.Security.Interop в каждое приложение ASP.NET 4.x.

2.   В Startup.Auth.cs найдите вызов UseCookieAuthentication, который обычно будет выглядеть следующим образом.

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        // ...
    });
    ```
    
3.  Измените вызов UseCookieAuthentication следующим образом, изменения CookieName должно соответствовать имени, используемого ASP.NET Core файла cookie проверки подлинности по промежуточного слоя, предоставления экземпляра DataProtectionProvider, который был инициализирован в расположение хранилища ключей и значение CookieManager взаимодействия диспетчер chunkingcookiemanager, который, поэтому совместим неверный формат.

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        AuthenticationType = DefaultAuthenticationTypes.ApplicationCookie,
        CookieName = ".AspNetCore.Cookies",
        // CookieName = ".AspNetCore.ApplicationCookie", (if you're using identity)
        // CookiePath = "...", (if necessary)
        // ...
        TicketDataFormat = new AspNetTicketDataFormat(
            new DataProtectorShim(
                DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
                .CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware",
                "Cookies", "v2"))),
        CookieManager = new ChunkingCookieManager()
    });
    ```
    Чтобы она указывала на том же месте хранения, указывает приложению ASP.NET Core и должны быть настроены с одинаковыми параметрами имеет DirectoryInfo.

ASP.NET 4.x и приложения ASP.NET Core теперь настроены для совместного использования файлов cookie проверки подлинности.

> [!NOTE]
> Вам потребуется, чтобы убедиться в том, что система идентификаторов для каждого приложения указывает на одной и той же базе данных пользователя. В противном случае система идентификации вызовет сбои во время выполнения при попытке соответствует сведениям в файл cookie проверки подлинности со сведениями в своей базе данных.
