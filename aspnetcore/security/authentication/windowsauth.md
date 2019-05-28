---
title: Настройка проверки подлинности Windows в ASP.NET Core
author: scottaddie
description: Узнайте, как настроить проверку подлинности Windows в ASP.NET Core, используя IIS Express, службы IIS и HTTP.sys.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 04/03/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: 46c9e054b6e9228a709f93c6b73772708f6c6eb0
ms.sourcegitcommit: b8ed594ab9f47fa32510574f3e1b210cff000967
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2019
ms.locfileid: "66251225"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>Настройка проверки подлинности Windows в ASP.NET Core

По [Scott Addie](https://twitter.com/Scott_Addie) и [Люк Лэтем](https://github.com/guardrex)

[Проверка подлинности Windows](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) можно настроить для приложений ASP.NET Core, размещенных с [IIS](xref:host-and-deploy/iis/index) или [HTTP.sys](xref:fundamentals/servers/httpsys).

Проверка подлинности Windows зависит от операционной системы для проверки подлинности пользователей, приложений ASP.NET Core. Можно использовать проверку подлинности Windows, когда сервер работает в корпоративной сети с помощью удостоверения домена Active Directory или учетных записей Windows для идентификации пользователей. Проверка подлинности Windows является наилучшим образом подходит для среды интрасети, где пользователи, клиентские приложения и веб-серверы принадлежат к тому же домену Windows.

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a>Включить проверку подлинности Windows в приложении ASP.NET Core

**Веб-приложение** шаблон, доступный через Visual Studio или .NET Core CLI можно настроить для поддержки проверки подлинности Windows.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="use-the-windows-authentication-app-template-for-a-new-project"></a>Шаблон приложения проверки подлинности Windows можно использовать для нового проекта

В Visual Studio сделайте следующее:

1. Создайте новый проект.
1. Выберите **Новое веб-приложение ASP.NET Core**. Выберите **Далее**.
1. Введите имя в **имя_проекта** поля. Подтвердите **расположение** запись правильно, или укажите расположение для проекта. Выберите **Создать**.
1. Выберите **изменение** под **проверки подлинности**.
1. В **изменить способ проверки подлинности** выберите **проверки подлинности Windows**. Нажмите кнопку **ОК**.
1. Выберите **Веб-приложение**.
1. Выберите **Создать**.

Запустите приложение. Имя пользователя отображается в готовом для просмотра приложения пользовательского интерфейса.

### <a name="manual-configuration-for-an-existing-project"></a>Ручная настройка для существующего проекта

Свойства проекта позволяют включить проверку подлинности Windows и отключить анонимную проверку подлинности:

1. Щелкните правой кнопкой мыши проект в Visual Studio **обозревателе решений** и выберите **свойства**.
1. Выберите вкладку **Отладка**.
1. Снимите флажок для **Включение анонимной проверки подлинности**.
1. Установите флажок для **включить проверку подлинности Windows**.

Кроме того, можно настроить свойства в `iisSettings` узел *launchSettings.json* файла:

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

Используйте **проверки подлинности Windows** шаблон приложения.

Выполнение [команды dotnet new](/dotnet/core/tools/dotnet-new) с `webapp` аргумент (веб-приложения ASP.NET Core) и `--auth Windows` переключения:

```console
dotnet new webapp --auth Windows
```

---

При изменении существующего проекта, убедитесь, что файл проекта содержит ссылку на пакет для [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) **или** [ Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) пакет NuGet.

## <a name="enable-windows-authentication-with-iis"></a>Включить проверку подлинности Windows со службами IIS

Службы IIS используют [модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module) для размещения приложений ASP.NET Core. Проверка подлинности Windows настроена для IIS с помощью *web.config* файл. В следующих разделах показано как:

* Укажите локальный *web.config* файл, который активирует проверку подлинности Windows на сервере, при развертывании приложения.
* Использование диспетчера служб IIS для настройки *web.config* файл приложения ASP.NET Core, которое уже развернуто на сервер.

### <a name="iis-configuration"></a>Конфигурация IIS

Если вы еще не сделали, включении служб IIS для размещения приложений ASP.NET Core. Дополнительные сведения см. в разделе <xref:host-and-deploy/iis/index>.

Включение службы роли IIS для проверки подлинности Windows. Дополнительные сведения см. в разделе [включить проверку подлинности Windows в службы роли IIS (см. шаг 2)](xref:host-and-deploy/iis/index#iis-configuration).

[По промежуточного слоя для интеграции IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) настраивается для автоматической проверки подлинности запросов по умолчанию. Дополнительные сведения см. в разделе [размещение ASP.NET Core в Windows со службами IIS: Параметры служб IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).

Модуль ASP.NET Core настроен на пересылку токена проверки подлинности Windows в приложение по умолчанию. Дополнительные сведения см. в разделе [Справочник по конфигурации модуля ASP.NET Core: Атрибуты элемента aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

### <a name="create-a-new-iis-site"></a>Создание нового сайта IIS

Укажите имя и папку и разрешите ему создать новый пул приложений.

### <a name="enable-windows-authentication-for-the-app-in-iis"></a>Включить проверку подлинности Windows для приложения в IIS

Используйте **либо** из следующих подходов:

* [Настройка на стороне разработки перед публикацией приложения](#development-side-configuration-with-a-local-webconfig-file) (*рекомендуется*)
* [Настройка на стороне сервера после публикации приложения](#server-side-configuration-with-the-iis-manager)

#### <a name="development-side-configuration-with-a-local-webconfig-file"></a>Настройка на стороне разработки с помощью файла web.config локального

Выполните следующие действия **перед** вы [публикация и развертывание проекта](#publish-and-deploy-your-project-to-the-iis-site-folder).

Добавьте следующий *web.config* файл в корневую папку проекта:

[!code-xml[](windowsauth/sample_snapshot/web_2.config)]

При публикации проекта с помощью пакета SDK (без `<IsTransformWebConfigDisabled>` свойство значение `true` в файле проекта), опубликованной *web.config* файл включает `<location><system.webServer><security><authentication>` раздел. Дополнительные сведения о `<IsTransformWebConfigDisabled>` свойство, см. в разделе <xref:host-and-deploy/iis/index#webconfig-file>.

#### <a name="server-side-configuration-with-the-iis-manager"></a>Настройка на стороне сервера с помощью диспетчера IIS

Выполните следующие действия **после** вы [публикация и развертывание проекта](#publish-and-deploy-your-project-to-the-iis-site-folder).

1. В диспетчере IIS выберите сайт IIS, в разделе **сайты** узел **подключений** боковой панели.
1. Дважды щелкните **проверки подлинности** в **IIS** области.
1. Выберите **анонимную проверку подлинности**. Выберите **отключить** в **действия** боковой панели.
1. Выберите **проверки подлинности Windows**. Выберите **включить** в **действия** боковой панели.

При этих действий, IIS Manager вносит изменения в приложения *web.config* файл. Объект `<system.webServer><security><authentication>` добавляется узел с обновленными параметрами для `anonymousAuthentication` и `windowsAuthentication`:

[!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

`<system.webServer>` Добавлен в раздел *web.config* файл диспетчером IIS находится за пределами приложения `<location>` добавлен с помощью пакета SDK .NET Core, когда приложение публикуется раздел. Поскольку раздел добавляется вне `<location>` узел, параметры, наследуются [дочерние приложения](xref:host-and-deploy/iis/index#sub-applications) с текущим приложением. Чтобы запретить наследование, переместите добавленного `<security>` разделе внутри `<location><system.webServer>` раздел, в котором пакет SDK.

Когда диспетчер IIS используется для добавления конфигурации IIS, оно влияет только на приложения *web.config* файл на сервере. Последующие развертывания приложения может перезаписать параметры на сервере, если копия сервера *web.config* заменяется проекта *web.config* файл. Используйте **либо** из следующих методов для управления параметрами:

* Используйте диспетчер служб IIS, чтобы присвоить параметрам в *web.config* файл после файл перезаписывается при развертывании.
* Добавить *файл web.config* приложение локально с параметрами. Дополнительные сведения см. в разделе [Настройка на стороне разработки](#development-side-configuration-with-a-local-webconfig-file) раздел.

### <a name="publish-and-deploy-your-project-to-the-iis-site-folder"></a>Публикация и развертывание проекта в папку сайта IIS

С помощью Visual Studio или .NET Core CLI, публикация и развертывание приложения в папке назначения.

Дополнительные сведения о размещении в службах IIS публикации и развертывания, см. в разделах:

* [dotnet publish](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>

Запустите приложение, чтобы убедиться, что проверка подлинности Windows работает.

## <a name="enable-windows-authentication-with-httpsys"></a>Включить проверку подлинности Windows с использованием HTTP.sys

Несмотря на то, что Kestrel не поддерживает проверку подлинности Windows, можно использовать [HTTP.sys](xref:fundamentals/servers/httpsys) для поддержки сценариев резидентных в Windows. В следующем примере настраивается узел веб-приложения для использования HTTP.sys с проверкой подлинности Windows:

[!code-csharp[](windowsauth/sample_snapshot/Program.cs?highlight=9-14)]

> [!NOTE]
> HTTP.sys делегирует задачи в проверку подлинности в режиме ядра с помощью протокола проверки подлинности Kerberos. Проверка подлинности в режиме пользователя не поддерживается с Kerberos и HTTP.sys. Необходимо использовать учетную запись компьютера для расшифровки маркера/билета Kerberos, полученного из Active Directory и переадресованного клиентом на сервер для проверки подлинности пользователя. Зарегистрируйте имя субъекта-службы (SPN) для узла, а не пользователя приложения.

> [!NOTE]
> HTTP.sys не поддерживается на сервере Nano Server версии 1709 или более поздней версии. Чтобы использовать проверку подлинности Windows и HTTP.sys с сервером Nano Server, используйте [контейнера Server Core (microsoft/windowsservercore)](https://hub.docker.com/r/microsoft/windowsservercore/). Дополнительные сведения о Server Core см. в разделе [Какова вариант установки Server Core в Windows Server?](/windows-server/administration/server-core/what-is-server-core).

## <a name="work-with-windows-authentication"></a>Работа с проверкой подлинности Windows

Состояние конфигурации анонимный доступ определяет способ, которым `[Authorize]` и `[AllowAnonymous]` атрибуты используются в приложении. Следующих двух разделах объясняется, как обрабатывать запрещенные и разрешенные конфигурации состояния анонимный доступ.

### <a name="disallow-anonymous-access"></a>Запретить анонимный доступ

Если включена проверка подлинности Windows и анонимный доступ отключен, `[Authorize]` и `[AllowAnonymous]` атрибуты никак не влияют. Если IIS сайта (или HTTP.sys) настроен на запрет анонимного доступа, никогда не достигнут приложения. По этой причине `[AllowAnonymous]` атрибут не применяется.

### <a name="allow-anonymous-access"></a>Разрешить анонимный доступ

Если включены как проверка подлинности Windows, так и анонимный доступ, используйте `[Authorize]` и `[AllowAnonymous]` атрибуты. `[Authorize]` Атрибут позволяет защищать частей приложения, которые действительно требует проверки подлинности Windows. `[AllowAnonymous]` Атрибут переопределения `[Authorize]` атрибут использования в приложениях, в которых разрешен анонимный доступ. См. в разделе [простой авторизации](xref:security/authorization/simple) сведения об использовании атрибута.

В ASP.NET Core 2.x, `[Authorize]` атрибута требуется дополнительная конфигурация *Startup.cs* бросить вызов анонимные запросы для проверки подлинности Windows. Рекомендуемая конфигурация зависит от немного используется веб-сервера.

> [!NOTE]
> По умолчанию пользователи, у которых отсутствует разрешение на доступ к странице, откроется пустой ответ HTTP 403. [StatusCodePages по промежуточного слоя](xref:fundamentals/error-handling#usestatuscodepages) можно настроить для предоставления пользователям более комфортные «Отказано в доступе».

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

ASP.NET Core не реализует олицетворения. Приложения запускаются с удостоверение приложения для всех запросов, с помощью удостоверения пула или процесс приложения. Если вам нужно явно выполнять действия от имени пользователя, используйте [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) в [терминалов встроенного ПО промежуточного слоя](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) в `Startup.Configure`. Запустить одно действие в этом контексте, а затем закройте контекста.

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

`RunImpersonated` не поддерживает асинхронные операции и не должны использоваться для сложных сценариев. Например упаковки всего запросов или по промежуточного слоя цепочек не поддерживается и не рекомендуется.

### <a name="claims-transformations"></a>Преобразования утверждений

При размещении в режиме в процессе IIS <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> не вызывается внутри для инициализации пользователя. Таким образом, реализация <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation>, используемая для преобразования утверждений после каждой проверки подлинности, не активируется по умолчанию. Дополнительные сведения и пример кода, который активирует преобразования утверждений, при размещении в процессе, см. в разделе <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.
