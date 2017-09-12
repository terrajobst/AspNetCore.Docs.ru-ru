---
title: "Настроить проверку подлинности в ASP.NET Core"
author: ardalis
description: "Как настроить проверку подлинности в ASP.NET Core"
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 7/5/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-548ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/windowsauth
ms.openlocfilehash: aa401f956d74680efd3964203af3e8866b129887
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>Настроить проверку подлинности в ASP.NET Core

По [Стив Смит](https://ardalis.com)

Проверка подлинности Windows можно настроить для приложений ASP.NET Core, размещенной в IIS или WebListener.

## <a name="what-is-windows-authentication"></a>Что такое проверка подлинности Windows

Проверка подлинности Windows зависит от операционной системы, для проверки подлинности пользователей приложения ASP.NET Core. При запуске сервера в корпоративной сети с помощью удостоверения домена Active Directory или других учетных записей Windows для идентификации пользователей, можно использовать проверку подлинности Windows. Проверка подлинности Windows является наиболее безопасный способ проверки подлинности наиболее подходящих для среды интрасети, которой принадлежит пользователей, клиентские приложения и веб-серверов в том же домене Windows.

[Дополнительные сведения о проверке подлинности Windows и его установки для служб IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).

## <a name="enabling-windows-authentication-in-an-aspnet-core-application"></a>Включение проверки подлинности Windows в приложении ASP.NET Core

Шаблон веб-приложения Visual Studio можно настроить для поддержки проверки подлинности Windows.

### <a name="using-the-windows-authentication-app-template"></a>С помощью шаблона приложения проверки подлинности Windows

В Visual Studio:
* Создайте новое веб-приложение ASP.NET Core. 
* В списке шаблонов выберите веб-приложение.
* Нажмите кнопку "Изменить проверку подлинности" и выберите **проверки подлинности Windows**. 

Запустите приложение. Имя пользователя отображается в верхней правой части приложения.

![Снимок экрана обозревателя проверки подлинности Windows](windowsauth/_static/browser-screenshot.png)

Для разработки с помощью IIS Express шаблон содержит все настройки, необходимой для использования проверки подлинности Windows. В следующем разделе показано, как настроить приложение ASP.NET Core вручную для проверки подлинности Windows.

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a>Параметры Visual Studio для Windows и анонимная проверка подлинности

Страницы свойств Visual Studio вкладку «Отладка» предоставляет флажки для проверки подлинности Windows и анонимную проверку подлинности.

![Снимок экрана обозревателя проверки подлинности Windows](windowsauth/_static/vs-auth-property-menu.png)

Можно также настроить эти свойства в `launchSettings.json` файла:

```json
{
  "iisSettings": {
    "windowsAuthentication": true,
    "anonymousAuthentication": false,
    "iisExpress": {
      "applicationUrl": "http://localhost:52171/",
      "sslPort": 0
    }
  } // additional options trimmed
}
```

## <a name="enabling-windows-authentication-with-iis"></a>Включение проверки подлинности Windows в службах IIS

Службы IIS используют [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) (ANCM) для размещения приложений ASP.NET Core. ANCM потоки проверки подлинности Windows для служб IIS по умолчанию. Настройка проверки подлинности Windows осуществляется в службах IIS, не проекта приложения. Следующие разделы показывают, как использовать диспетчер служб IIS для настройки приложения ASP.NET Core, чтобы использовать проверку подлинности Windows:

### <a name="create-a-new-iis-site"></a>Создать новый сайт IIS

Укажите имя и папка и разрешить его, чтобы создать новый пул приложений.

### <a name="customize-authentication"></a>Настройки проверки подлинности

Откройте меню проверки подлинности для сайта.

![Меню проверки подлинности IIS](windowsauth/_static/iis-authentication-menu.png)

Отключите анонимную проверку подлинности и включить проверку подлинности Windows.

![Параметры проверки подлинности IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a>Опубликовать проект в папку узла IIS

С помощью Visual Studio или .NET Core (CLI), *публикации* приложения в папку назначения.

![Диалоговое окно публикации Visual Studio](windowsauth/_static/vs-publish-app.png)

Дополнительные сведения о [публикация в службах IIS](xref:publishing/iis).

Запустите приложение, чтобы убедиться, что проверка подлинности Windows работает.

## <a name="enabling-windows-authentication-with-weblistener"></a>Включение проверки подлинности Windows с WebListener

Несмотря на то, что Kestrel не поддерживает проверку подлинности Windows, можно использовать [WebListener](xref:fundamentals/servers/weblistener) для поддержки сценариев резидентных в Windows. В следующем примере настраивается узел веб-приложения для WebListener с проверкой подлинности Windows:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var host = new WebHostBuilder()
            .UseWebListener(options =>
            {
                options.ListenerSettings.Authentication.Schemes = 
                    AuthenticationSchemes.Negotiate | AuthenticationSchemes.NTLM;
                options.ListenerSettings.Authentication.AllowAnonymous = false;
            })
            .UseContentRoot(Directory.GetCurrentDirectory())
            .UseStartup<Startup>()
            .Build();

        host.Run();
    }
}
```

## <a name="working-with-windows-authentication"></a>Работа с проверкой подлинности Windows

Если ваше приложение использует проверку подлинности Windows и анонимный доступ, можно использовать ``[Authorize]`` и ``[AllowAnonymous]`` атрибуты. Приложения, имеющие не требует анонимного включена ``[Authorize]``; приложение рассматривается как требование проверки подлинности, анонимные запросы будут отвергнуты. Обратите внимание, если узел IIS настроен **не** разрешает анонимный доступ ``[AllowAnonymous]`` атрибут не **не** Разрешить анонимные запросы. ``[AllowAnonymous]`` Переопределения атрибутов ``[Authorize]`` атрибут для использования в приложениях, которые разрешить анонимный доступ.

### <a name="impersonation"></a>Олицетворение

Олицетворение ASP.NET Core не реализован. Приложения работают с удостоверением приложения для всех запросов, используя удостоверение пула или процесс приложения. Если вам нужно явно выполнять действия от имени пользователя, используйте ``WindowsIdentity.RunImpersonated``. Запустить одно действие в этом контексте, а затем закройте контекст. Обратите внимание, что ``RunImpersonated`` не поддерживает асинхронные и не должны использоваться для сложных сценариев. Например упаковки все запросы или цепочки по промежуточного слоя не поддерживается или рекомендуемые.
