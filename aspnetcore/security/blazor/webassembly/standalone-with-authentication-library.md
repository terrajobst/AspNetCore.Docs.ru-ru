---
title: Защищайте Blazor автономное приложение ASP.NET Core WebAssembly с библиотекой аутентификации
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-authentication-library
ms.openlocfilehash: 893fff10df37e1c2be549604f4cb83cd20049108
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/08/2020
ms.locfileid: "80977045"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-the-authentication-library"></a>Защищайте Blazor автономное приложение ASP.NET Core WebAssembly с библиотекой аутентификации

[Хавьер Кальварро Нельсон](https://github.com/javiercn) и Люк [Лэйтам](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

*Для Active Directory Azure (AAD) и Azure Active Directory B2C (AAD B2C) не следуйте указаниям в этой теме. В этой таблице узлов содержимого смотрите темы AAD и AAD B2C.*

Чтобы создать Blazor автономное приложение WebAssembly, используюваее `Microsoft.AspNetCore.Components.WebAssembly.Authentication` библиотеку, выполните следующую команду в командной оболочке:

```dotnetcli
dotnet new blazorwasm -au Individual
```

Чтобы указать местона вывода, которое создает папку проекта, если она не существует, включите опцию вывода в команду с путем (например, `-o BlazorSample`). Имя папки также становится частью названия проекта.

В Visual Studio [создайте приложение Blazor WebAssembly](xref:blazor/get-started). Установите **аутентификацию** для **индивидуальных учетных записей пользователей** с помощью опции приложения **для пользователей Магазина.**

## <a name="authentication-package"></a>Пакет аутентификации

Когда приложение создается для использования индивидуальных учетных записей `Microsoft.AspNetCore.Components.WebAssembly.Authentication` пользователей, приложение автоматически получает ссылку на пакет для пакета в файле проекта приложения. Пакет предоставляет набор примитивов, которые помогают приложению аутентифицировать пользователей и получать токены для вызова защищенных AIS.

При добавлении аутентификации в приложение вручную добавьте пакет в файл проекта приложения:

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

Замените `{VERSION}` в предыдущем пакете ссылку на версию `Microsoft.AspNetCore.Blazor.Templates` пакета, показанную <xref:blazor/get-started> в статье.

## <a name="authentication-service-support"></a>Поддержка службы аутентификации

Поддержка аутентификации пользователей регистрируется `AddOidcAuthentication` в сервисном `Microsoft.AspNetCore.Components.WebAssembly.Authentication` контейнере с методом расширения, предусмотренным пакетом. Этот метод настраивает все службы, необходимые для взаимодействия приложения с поставщиком идентификационных данных (IP).

*Program.cs*:

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    options.ProviderOptions.Authority = "{AUTHORITY}";
    options.ProviderOptions.ClientId = "{CLIENT ID}";
});
```

Поддержка аутентификации автономных приложений предлагается с помощью Open ID Connect (OIDC). Метод `AddOidcAuthentication` принимает обратный вызов для настройки параметров, необходимых для проверки подлинности приложения с помощью OIDC. Значения, необходимые для настройки приложения, могут быть получены с IP-адреса, совместимым с OIDC. Получить значения при регистрации приложения, которое обычно происходит в их интернет-портале.

## <a name="access-token-scopes"></a>Области маркеров доступа

Шаблон Blazor WebAssembly не настраивает приложение автоматически, чтобы запросить токен доступа для защищенного API. Чтобы предоставить токен как часть потока ввоза, добавьте область к `OidcProviderOptions`примку токенов по умолчанию:

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultScopes.Add("{SCOPE URI}");
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
> options.ProviderOptions.DefaultScopes.Add(
>     "{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}");
> ```

Для получения дополнительной информации см. <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>.

## <a name="imports-file"></a>Файл импорта

[!INCLUDE[](~/includes/blazor-security/imports-file-standalone.md)]

## <a name="index-page"></a>Страница индексации

[!INCLUDE[](~/includes/blazor-security/index-page-authentication.md)]

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
