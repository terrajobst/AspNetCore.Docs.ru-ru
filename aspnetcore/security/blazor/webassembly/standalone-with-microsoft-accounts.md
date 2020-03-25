---
title: Защита ASP.NET Core автономного приложения Blazor сборки с помощью учетных записей Майкрософт
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-microsoft-accounts
ms.openlocfilehash: be73bec971f96bd64afc735a1ea750d47c7bc383
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219263"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-microsoft-accounts"></a>Защита ASP.NET Core автономного приложения Blazor сборки с помощью учетных записей Майкрософт

[Хавьер Калварро Воронков](https://github.com/javiercn) и [Люк ЛаСаМ](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Чтобы создать изолированное приложение Blazor сборки, использующее [учетные записи Майкрософт с Azure Active Directory (AAD)](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal) для проверки подлинности:

1. [Создание клиента AAD и веб-приложения](/azure/active-directory/develop/v2-overview)

   Зарегистрируйте приложение AAD в области **Azure Active Directory** > **Регистрация приложений** портал Azure:

   1\. Укажите **имя** приложения (например, **Blazor клиента AAD**).<br>
   2\. В списке **Поддерживаемые типы учетных записей**выберите **учетные записи в любом организационном каталоге**.<br>
   3\. Оставьте в раскрывающемся списке **URI перенаправления** значение **веб**и укажите универсальный код ресурса (uri) перенаправления `https://localhost:5001/authentication/login-callback`.<br>
   4\. Отключите **разрешения** > установите флажок **предоставить доступ к администратору для OpenID Connect и offline_access разрешений** .<br>
   5\. Выберите **Зарегистрировать**.

   При **проверке Подлинности** > **конфигурации платформы** > **Web**:

   1\. Убедитесь, что **URI перенаправления** для `https://localhost:5001/authentication/login-callback` имеется.<br>
   2\. Для **неявного предоставления**установите флажки для **маркеров доступа** и **маркеров идентификации**.<br>
   3\. Остальные значения по умолчанию для приложения приемлемы для этого интерфейса.<br>
   4\. Нажмите кнопку **Сохранить**.

   Запишите идентификатор приложения (идентификатор клиента) (например, `11111111-1111-1111-1111-111111111111`).

1. Замените заполнители в следующей команде на записанные ранее сведения и выполните команду в командной оболочке:

   ```dotnetcli
   dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "common"
   ```

   Чтобы указать расположение выходных данных, которое создает папку проекта, если она не существует, включите параметр OUTPUT в команду с путем (например, `-o BlazorSample`). Имя папки также станет частью имени проекта.

После создания приложения вы сможете:

* Войдите в приложение, используя учетную запись Майкрософт.
* Запросите маркеры доступа для API-интерфейсов Майкрософт, используя тот же подход, что и для автономных Blazorных приложений, при условии, что приложение настроено правильно. Дополнительные сведения см. [в разделе Краткое руководство. Настройка приложения для предоставления веб-API](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis).

## <a name="authentication-package"></a>Пакет проверки подлинности

Когда приложение создается для использования рабочих или учебных учетных записей (`SingleOrg`), приложение автоматически получает ссылку на пакет для [библиотеки проверки подлинности Майкрософт](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`). Пакет предоставляет набор примитивов, которые помогают приложению проверять подлинность пользователей и получать маркеры для вызова защищенных интерфейсов API.

При добавлении проверки подлинности в приложение вручную добавьте пакет в файл проекта приложения:

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

Замените `{VERSION}` в предыдущей ссылке на пакет версией пакета `Microsoft.AspNetCore.Blazor.Templates`, показанного в статье <xref:blazor/get-started>.

Пакет `Microsoft.Authentication.WebAssembly.Msal` транзитивно добавляет пакет `Microsoft.AspNetCore.Components.WebAssembly.Authentication` в приложение.

## <a name="authentication-service-support"></a>Поддержка службы проверки подлинности

Поддержка проверки подлинности пользователей регистрируется в контейнере службы с помощью метода расширения `AddMsalAuthentication`, предоставленного пакетом `Microsoft.Authentication.WebAssembly.Msal`. Этот метод настраивает все службы, необходимые для взаимодействия приложения с поставщиком удостоверений (IP).

*Program.cs*:

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "{AUTHORITY}";
    authentication.ClientId = "{CLIENT ID}";
});
```

Метод `AddMsalAuthentication` принимает обратный вызов для настройки параметров, необходимых для проверки подлинности приложения. Значения, необходимые для настройки приложения, можно получить из конфигурации учетных записей Майкрософт при регистрации приложения.

## <a name="index-page"></a>Страница индексации

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

## <a name="app-component"></a>Компонент приложения

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a>Компонент Редиректтологин

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a>Компонент Логиндисплай

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a>Компонент проверки подлинности

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Краткое руководство. Регистрация приложения на платформе Microsoft Identity](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal)
* [Краткое руководство. Настройка приложения для предоставления веб-API](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)
