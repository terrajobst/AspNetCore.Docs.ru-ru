---
title: Настройка проверки подлинности Windows в ASP.NET Core
author: scottaddie
description: Узнайте, как настроить проверку подлинности Windows в ASP.NET Core, используя IIS Express, службы IIS, HTTP.sys и WebListener.
ms.author: riande
ms.custom: mvc
ms.date: 11/01/2018
uid: security/authentication/windowsauth
ms.openlocfilehash: 87fcab75555c1dae0b2815c30d79fd4615df9660
ms.sourcegitcommit: 85f2939af7a167b9694e1d2093277ffc9a741b23
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/02/2018
ms.locfileid: "50968297"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>Настройка проверки подлинности Windows в ASP.NET Core

Авторы: [Стив Смит](https://ardalis.com) (Steve Smith) и [Скотт Эдди](https://twitter.com/Scott_Addie) (Scott Addie)

Проверка подлинности Windows можно настроить для приложений ASP.NET Core, размещенных в IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), или [WebListener](xref:fundamentals/servers/weblistener).

## <a name="windows-authentication"></a>Проверка подлинности Windows

Проверка подлинности Windows зависит от операционной системы для проверки подлинности пользователей, приложений ASP.NET Core. Можно использовать проверку подлинности Windows, когда сервер работает в корпоративной сети с помощью удостоверения домена Active Directory или других учетных записей Windows для идентификации пользователей. Проверка подлинности Windows является наилучшим образом подходит для среды интрасети, в которых пользователи, клиентские приложения и веб-серверы принадлежат к тому же домену Windows.

[Дополнительные сведения о проверке подлинности Windows и его установки для служб IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a>Включить проверку подлинности Windows в приложении ASP.NET Core

Шаблон веб-приложения Visual Studio можно настроить для поддержки проверки подлинности Windows.

### <a name="use-the-windows-authentication-app-template"></a>Использовать шаблон приложения проверки подлинности Windows

В Visual Studio:

1. Создайте новое веб-приложение ASP.NET Core.
1. Выберите веб-приложение из списка шаблонов.
1. Выберите **изменить способ проверки подлинности** и выберите **проверки подлинности Windows**.

Запустите приложение. Имя пользователя отображается в правом верхнем углу приложения.

![Снимок экрана обозревателя проверки подлинности Windows](windowsauth/_static/browser-screenshot.png)

Для разработки с использованием IIS Express шаблон содержит все настройки, необходимой для использования проверки подлинности Windows. Ниже показано, как вручную настроить приложение ASP.NET Core для проверки подлинности Windows.

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a>Параметры Visual Studio для Windows и анонимная проверка подлинности

В проект Visual Studio **свойства** страницы **Отладка** вкладка предоставляет флажки для проверки подлинности Windows и анонимную проверку подлинности.

![Снимок экрана обозревателя проверки подлинности Windows](windowsauth/_static/vs-auth-property-menu.png)

Кроме того, эти свойства можно задать в *launchSettings.json* файла:

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a>Включить проверку подлинности Windows со службами IIS

Службы IIS используют [модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) для размещения приложений ASP.NET Core. В службах IIS, приложение не настроена проверка подлинности Windows. Ниже показано, как использовать диспетчер служб IIS для настройки приложения ASP.NET Core для использования проверки подлинности Windows.

### <a name="iis-configuration"></a>Конфигурация IIS

Включение службы роли IIS для проверки подлинности Windows. Дополнительные сведения см. в разделе [включить проверку подлинности Windows в службы роли IIS (см. шаг 2)](xref:host-and-deploy/iis/index#iis-configuration).

По умолчанию по промежуточного слоя для интеграции IIS настроен для автоматической проверки подлинности запросов. Дополнительные сведения см. в разделе [размещение ASP.NET Core в Windows со службами IIS: параметры служб IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).

Модуль ASP.NET Core настроен на пересылку токена проверки подлинности Windows в приложение по умолчанию. Дополнительные сведения см. в разделе [Справочник по конфигурации модуля ASP.NET Core: атрибуты элемента aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

### <a name="create-a-new-iis-site"></a>Создание нового сайта IIS

Укажите имя и папку и разрешите ему создать новый пул приложений.

### <a name="customize-authentication"></a>Настройки проверки подлинности

Откройте средства проверки подлинности для сайта.

![Меню проверки подлинности IIS](windowsauth/_static/iis-authentication-menu.png)

Отключить анонимную проверку подлинности и включить проверку подлинности Windows.

![Параметры проверки подлинности IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a>Опубликовать проект в папке сайта IIS

С помощью Visual Studio или .NET Core CLI, опубликуйте приложение в папку назначения.

![Диалоговое окно публикации Visual Studio](windowsauth/_static/vs-publish-app.png)

Дополнительные сведения о [публикация в службах IIS](xref:host-and-deploy/iis/index).

Запустите приложение, чтобы убедиться, что проверка подлинности Windows работает.

::: moniker range=">= aspnetcore-2.0"

## <a name="enable-windows-authentication-with-httpsys"></a>Включить проверку подлинности Windows с использованием HTTP.sys

Несмотря на то, что Kestrel не поддерживает проверку подлинности Windows, можно использовать [HTTP.sys](xref:fundamentals/servers/httpsys) для поддержки сценариев резидентных в Windows. В следующем примере настраивается узел веб-приложения для использования HTTP.sys с проверкой подлинности Windows:

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

> [!NOTE]
> HTTP.sys делегирует задачи в проверку подлинности в режиме ядра с помощью протокола проверки подлинности Kerberos. Проверка подлинности в режиме пользователя не поддерживается с Kerberos и HTTP.sys. Необходимо использовать учетную запись компьютера для расшифровки маркера/билета Kerberos, полученного из Active Directory и переадресованного клиентом на сервер для проверки подлинности пользователя. Зарегистрируйте имя субъекта-службы (SPN) для узла, а не пользователя приложения.

> [!NOTE]
> HTTP.sys не поддерживается на сервере Nano Server версии 1709 или более поздней версии. Чтобы использовать проверку подлинности Windows и HTTP.sys с сервером Nano Server, используйте [контейнера Server Core (microsoft/windowsservercore)](https://hub.docker.com/r/microsoft/windowsservercore/). Дополнительные сведения о Server Core см. в разделе [Какова вариант установки Server Core в Windows Server?](/windows-server/administration/server-core/what-is-server-core).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

## <a name="enable-windows-authentication-with-weblistener"></a>Включить проверку подлинности Windows с помощью WebListener

Несмотря на то, что Kestrel не поддерживает проверку подлинности Windows, можно использовать [WebListener](xref:fundamentals/servers/weblistener) для поддержки сценариев резидентных в Windows. В следующем примере настраивается узел веб-приложения для использования WebListener с проверкой подлинности Windows:

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

> [!NOTE]
> WebListener делегирует задачи в проверку подлинности в режиме ядра с помощью протокола проверки подлинности Kerberos. Проверка подлинности в режиме пользователя не поддерживается с Kerberos и WebListener. Необходимо использовать учетную запись компьютера для расшифровки маркера/билета Kerberos, полученного из Active Directory и переадресованного клиентом на сервер для проверки подлинности пользователя. Зарегистрируйте имя субъекта-службы (SPN) для узла, а не пользователя приложения.

::: moniker-end

## <a name="work-with-windows-authentication"></a>Работа с проверкой подлинности Windows

Состояние конфигурации анонимный доступ определяет способ, которым `[Authorize]` и `[AllowAnonymous]` атрибуты используются в приложении. Следующих двух разделах объясняется, как обрабатывать запрещенные и разрешенные конфигурации состояния анонимный доступ.

### <a name="disallow-anonymous-access"></a>Запретить анонимный доступ

Если включена проверка подлинности Windows и анонимный доступ отключен, `[Authorize]` и `[AllowAnonymous]` атрибуты никак не влияют. Если IIS сайта (или сервера HTTP.sys или WebListener) настроен на запрет анонимного доступа, никогда не достигнут приложения. По этой причине `[AllowAnonymous]` атрибут не применяется.

### <a name="allow-anonymous-access"></a>Разрешить анонимный доступ

Если включены как проверка подлинности Windows, так и анонимный доступ, используйте `[Authorize]` и `[AllowAnonymous]` атрибуты. `[Authorize]` Атрибут позволяет защищать частей приложения, которые действительно требует проверки подлинности Windows. `[AllowAnonymous]` Атрибут переопределения `[Authorize]` атрибут использования в приложениях, в которых разрешен анонимный доступ. См. в разделе [простой авторизации](xref:security/authorization/simple) сведения об использовании атрибута.

В ASP.NET Core 2.x, `[Authorize]` атрибута требуется дополнительная конфигурация *Startup.cs* бросить вызов анонимные запросы для проверки подлинности Windows. Рекомендуемая конфигурация зависит от немного используется веб-сервера.

> [!NOTE]
> По умолчанию пользователи, у которых отсутствует разрешение на доступ к странице, откроется пустой ответ HTTP 403. [StatusCodePages по промежуточного слоя](xref:fundamentals/error-handling#configure-status-code-pages) можно настроить для предоставления пользователям более комфортные «Отказано в доступе».

#### <a name="iis"></a>IIS

Если используется IIS, добавьте следующий код в `ConfigureServices` метод:

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a>HTTP.sys

Если в файле HTTP.sys, добавьте следующий код в `ConfigureServices` метод:

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a>Олицетворение

ASP.NET Core не реализует олицетворения. Приложения запускаются с удостоверением приложения для всех запросов, с помощью удостоверения пула или процесс приложения. Если вам нужно явно выполнять действия от имени пользователя, используйте `WindowsIdentity.RunImpersonated`. Запустить одно действие в этом контексте, а затем закройте контекста.

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

Обратите внимание, что `RunImpersonated` не поддерживает асинхронные операции и не должны использоваться для сложных сценариев. Например упаковки всего запросов или по промежуточного слоя цепочек не поддерживается и не рекомендуется.
