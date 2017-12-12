---
title: "Настроить проверку подлинности в ASP.NET Core"
author: ardalis
description: "В этой статье описывается настройка проверки подлинности Windows в ASP.NET Core, используя IIS Express, службы IIS, HTTP.sys и WebListener."
keywords: "ASP.NET Core, проверка подлинности Windows, Authorize атрибут, атрибут AllowAnonymous"
ms.author: riande
manager: wpickett
ms.date: 10/24/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-548ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/windowsauth
ms.openlocfilehash: 703f924d049a267cb8bee22e63e6669b13099c53
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="configure-windows-authentication-in-an-aspnet-core-app"></a>Настройка проверки подлинности Windows в приложении ASP.NET Core

По [Стив Смит](https://ardalis.com) и [Скотт Addie](https://twitter.com/Scott_Addie)

Проверка подлинности Windows можно настроить для приложения ASP.NET Core, размещенные в IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), или [WebListener](xref:fundamentals/servers/weblistener).

## <a name="what-is-windows-authentication"></a>Что такое проверка подлинности Windows

Проверка подлинности Windows зависит от операционной системы, для проверки подлинности пользователей приложения ASP.NET Core. При запуске сервера в корпоративной сети с помощью удостоверения домена Active Directory или других учетных записей Windows для идентификации пользователей, можно использовать проверку подлинности Windows. Проверка подлинности Windows наилучшим образом подходит для среды интрасети, в которых пользователей, клиентские приложения и веб-серверы принадлежат к одному домену Windows.

[Дополнительные сведения о проверке подлинности Windows и его установки для служб IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a>Включение проверки подлинности Windows в приложении ASP.NET Core

Шаблон веб-приложения Visual Studio можно настроить для поддержки проверки подлинности Windows.

### <a name="use-the-windows-authentication-app-template"></a>Используйте шаблон приложения проверки подлинности Windows

В Visual Studio:
1. Создайте новое веб-приложение ASP.NET Core. 
1. В списке шаблонов выберите веб-приложение.
1. Выберите **изменить аутентификацию** и выберите пункт **проверки подлинности Windows**. 

Запустите приложение. Имя пользователя отображается в верхней правой части приложения.

![Снимок экрана обозревателя проверки подлинности Windows](windowsauth/_static/browser-screenshot.png)

Для разработки с помощью IIS Express шаблон содержит все настройки, необходимой для использования проверки подлинности Windows. Ниже показано, как вручную настроить приложение ASP.NET Core для проверки подлинности Windows.

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a>Параметры Visual Studio для Windows и анонимная проверка подлинности

Проект Visual Studio **свойства** страницы **отладки** вкладка предоставляет флажки для проверки подлинности Windows и анонимную проверку подлинности.

![Снимок экрана обозревателя проверки подлинности Windows](windowsauth/_static/vs-auth-property-menu.png)

Кроме того, можно настроить эти свойства в *launchSettings.json* файла:

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a>Включение проверки подлинности Windows в службах IIS

Службы IIS используют [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) (ANCM) для размещения приложений ASP.NET Core. ANCM потоки проверки подлинности Windows для служб IIS по умолчанию. Настройка проверки подлинности Windows осуществляется в службах IIS, не проекта приложения. Ниже показано, как использовать диспетчер служб IIS для настройки приложения ASP.NET Core, чтобы использовать проверку подлинности Windows.

### <a name="create-a-new-iis-site"></a>Создать новый сайт IIS

Укажите имя и папка и разрешить его, чтобы создать новый пул приложений.

### <a name="customize-authentication"></a>Настройки проверки подлинности

Откройте меню проверки подлинности для сайта.

![Меню проверки подлинности IIS](windowsauth/_static/iis-authentication-menu.png)

Отключите анонимную проверку подлинности и включить проверку подлинности Windows.

![Параметры проверки подлинности IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a>Опубликовать проект в папку узла IIS

С помощью Visual Studio или .NET Core CLI, опубликуйте приложение в папку назначения.

![Диалоговое окно публикации Visual Studio](windowsauth/_static/vs-publish-app.png)

Дополнительные сведения о [публикация в службах IIS](xref:publishing/iis).

Запустите приложение, чтобы убедиться, что проверка подлинности Windows работает.

## <a name="enable-windows-authentication-with-httpsys-or-weblistener"></a>Включение проверки подлинности Windows в HTTP.sys или WebListener

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Несмотря на то, что Kestrel не поддерживает проверку подлинности Windows, можно использовать [HTTP.sys](xref:fundamentals/servers/httpsys) для поддержки сценариев резидентных в Windows. В следующем примере настраивается узел веб-приложения для HTTP.sys с проверкой подлинности Windows:

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Несмотря на то, что Kestrel не поддерживает проверку подлинности Windows, можно использовать [WebListener](xref:fundamentals/servers/weblistener) для поддержки сценариев резидентных в Windows. В следующем примере настраивается узел веб-приложения для WebListener с проверкой подлинности Windows:

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

---

## <a name="work-with-windows-authentication"></a>Использование проверки подлинности Windows

Состояние конфигурации анонимный доступ определяет, каким образом `[Authorize]` и `[AllowAnonymous]` атрибуты используются в приложении. Следующих двух разделах объясняется, как обрабатывать конфигурации запрещенных и разрешенных состояний анонимный доступ.

### <a name="disallow-anonymous-access"></a>Запретить анонимный доступ

Если включена проверка подлинности Windows и анонимный доступ отключен, `[Authorize]` и `[AllowAnonymous]` атрибуты никак не влияют. Если сайт IIS (или HTTP.sys или WebListener сервера) настроен для запрещения анонимный доступ, запрос никогда не достигает приложения. По этой причине `[AllowAnonymous]` не применяется атрибут.

### <a name="allow-anonymous-access"></a>Разрешить анонимный доступ

При включении проверки подлинности Windows и анонимный доступ, используйте `[Authorize]` и `[AllowAnonymous]` атрибуты. `[Authorize]` Атрибут позволяет защищать части приложения, которые действительно требуется проверка подлинности Windows. `[AllowAnonymous]` Переопределения атрибутов `[Authorize]` атрибута для использования в приложениях, в которых разрешен анонимный доступ. В разделе [простой авторизации](xref:security/authorization/simple) сведения об использовании атрибутов.

В ASP.NET Core 2.x `[Authorize]` атрибут требует дополнительной настройки в *файла Startup.cs* проверку анонимные запросы на проверку подлинности Windows. Рекомендуемая конфигурация немного различается в зависимости от используемого веб-сервера.

#### <a name="iis"></a>IIS

Если с помощью IIS, добавьте следующий код в `ConfigureServices` метод: 

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a>HTTP.sys

Если с помощью файла HTTP.sys, добавьте следующий код в `ConfigureServices` метод:

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a>Олицетворение

ASP.NET Core не реализует олицетворения. Приложения работают с удостоверением приложения для всех запросов, используя удостоверение пула или процесс приложения. Если вам нужно явно выполнять действия от имени пользователя, используйте `WindowsIdentity.RunImpersonated`. Запустить одно действие в этом контексте, а затем закройте контекст.

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

Обратите внимание, что `RunImpersonated` не поддерживает асинхронные операции и не должны использоваться для сложных сценариев. Например упаковки все запросы или цепочки по промежуточного слоя не поддерживается и не рекомендуется.

---
