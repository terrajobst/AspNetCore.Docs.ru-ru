---
title: Защищайте Blazor автономное приложение ASP.NET Core WebAssembly с учетными записями Майкрософт
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-microsoft-accounts
ms.openlocfilehash: 8c409651b3338c2baeae497bef43b994823a20f9
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/08/2020
ms.locfileid: "80977084"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-microsoft-accounts"></a>Защищайте Blazor автономное приложение ASP.NET Core WebAssembly с учетными записями Майкрософт

[Хавьер Кальварро Нельсон](https://github.com/javiercn) и Люк [Лэйтам](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Для создания Blazor автономного приложения WebAssembly, использующее [учетные записи Майкрософт с помощью Active Directory (AAD) для](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal) проверки подлинности:

1. [Создание арендатора AAD и веб-приложения](/azure/active-directory/develop/v2-overview)

   Зарегистрируйте приложение AAD в зоне**регистрации приложений** **Azure Active Directory** > на портале Azure:

   1\. Укажите **имя** приложения (например, ** Blazor Client AAD).**<br>
   2\. В **поддерживаемых типах учетных записей**выберите **учетные записи в любом каталоге организации.**<br>
   3\. Оставьте **перенаправить URI** упасть набор в **Интернет**, `https://localhost:5001/authentication/login-callback`и обеспечить перенаправление URI .<br>
   4\. Отключите > **админ-концентратор** **Разрешений**Гранта для проверки открытых и offline_access разрешений.<br>
   5\. Выберите **Зарегистрировать**.

   В**настройках платформы** >  **аутентификации** > **Web**:

   1\. Подтвердите, `https://localhost:5001/authentication/login-callback` что **redirect URI** присутствует.<br>
   2\. Для **неявного гранта**выберите флажки для **токенов Access** и **токенов ID.**<br>
   3\. Остальные по умолчанию для приложения приемлемы для этого опыта.<br>
   4\. Нажмите кнопку **Сохранить**.

   Запись идентификатора приложения (например, `11111111-1111-1111-1111-111111111111`id клиента).

1. Замените заполнителей в следующей команде информацией, записанной ранее, и выполните команду в командном корпусе:

   ```dotnetcli
   dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "common"
   ```

   Чтобы указать местона вывода, которое создает папку проекта, если она не существует, включите опцию вывода в команду с путем (например, `-o BlazorSample`). Имя папки также становится частью названия проекта.

После создания приложения, вы должны быть в состоянии:

* Войти в приложение с помощью учетной записи Майкрософт.
* Запрос токенов доступа для AI Microsoft с Blazor использованием того же подхода, что и для автономных приложений, при условии, что вы правильно настроили приложение. Для получения дополнительной информации [см.](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)

## <a name="authentication-package"></a>Пакет аутентификации

Когда приложение создается для использования рабочих`SingleOrg`или школьных учетных записей ( ),`Microsoft.Authentication.WebAssembly.Msal`приложение автоматически получает ссылку на пакет для [библиотеки подлинности Майкрософт](/azure/active-directory/develop/msal-overview) (). Пакет предоставляет набор примитивов, которые помогают приложению аутентифицировать пользователей и получать токены для вызова защищенных AIS.

При добавлении аутентификации в приложение вручную добавьте пакет в файл проекта приложения:

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

Замените `{VERSION}` в предыдущем пакете ссылку на версию `Microsoft.AspNetCore.Blazor.Templates` пакета, показанную <xref:blazor/get-started> в статье.

Пакет `Microsoft.Authentication.WebAssembly.Msal` транзитно добавляет `Microsoft.AspNetCore.Components.WebAssembly.Authentication` пакет в приложение.

## <a name="authentication-service-support"></a>Поддержка службы аутентификации

Поддержка аутентификации пользователей регистрируется `AddMsalAuthentication` в сервисном `Microsoft.Authentication.WebAssembly.Msal` контейнере с методом расширения, предусмотренным пакетом. Этот метод настраивает все службы, необходимые для взаимодействия приложения с поставщиком идентификационных данных (IP).

*Program.cs*:

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "{AUTHORITY}";
    authentication.ClientId = "{CLIENT ID}";
});
```

Метод `AddMsalAuthentication` принимает обратный вызов для настройки параметров, необходимых для проверки подлинности приложения. Значения, необходимые для настройки приложения, могут быть получены из конфигурации учетных записей Майкрософт при регистрации приложения.

## <a name="access-token-scopes"></a>Области маркеров доступа

Шаблон Blazor WebAssembly не настраивает приложение автоматически, чтобы запросить токен доступа для защищенного API. Чтобы предоставить токен как часть потока ввоза, добавьте область к области `MsalProviderOptions`маркеров доступа по умолчанию:

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

> [!NOTE]
> Если портал Azure предоставляет область URI и **приложение выбрасывает необработанное исключение,** когда получает *401 несанкционированный* ответ от API, попробуйте использовать область URI, которая не включает схему и узел. Например, портал Azure может предоставить один из следующих форматов URI:
>
> * `https://{ORGANIZATION}.onmicrosoft.com/{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
> * `api://{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
>
> Поставка области URI без схемы и хозяина:
>
> ```csharp
> options.ProviderOptions.DefaultAccessTokenScopes.Add(
>     "{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}");
> ```

Для получения дополнительной информации см. <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>.

## <a name="imports-file"></a>Файл импорта

[!INCLUDE[](~/includes/blazor-security/imports-file-standalone.md)]

## <a name="index-page"></a>Страница индексации

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

## <a name="app-component"></a>Компонент приложения

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a>ПеренаправлениеКомпонентToLogin

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a>Компонент LoginDisplay

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a>Компонент аутентификации

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Запрос дополнительных токенов доступа](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
* [Быстрый запуск: Зарегистрируйте приложение на платформе идентификации Майкрософт](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal)
* [Быстрый запуск: Настройка приложения для разоблачения веб-AIS](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)
