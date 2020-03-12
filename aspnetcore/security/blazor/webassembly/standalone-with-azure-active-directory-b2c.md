---
title: Защита ASP.NET Core автономного приложения Blazor сборки с помощью Azure Active Directory B2C
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-azure-active-directory-b2c
ms.openlocfilehash: 0ea42943c908d8cf9d083c1cfc568c1835588ce9
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083659"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-azure-active-directory-b2c"></a>Защита ASP.NET Core автономного приложения Blazor сборки с помощью Azure Active Directory B2C

[Хавьер Калварро Воронков](https://github.com/javiercn) и [Люк ЛаСаМ](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Чтобы создать изолированное приложение Blazor сборки, использующее [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) для проверки подлинности:

1. Следуйте указаниям в следующих разделах, чтобы создать клиент и зарегистрировать веб-приложение на портале Azure.

   * [Создайте клиент AAD B2C](/azure/active-directory-b2c/tutorial-create-tenant) &ndash; запишите следующие сведения:

     1 \. AAD B2C экземпляр (например, `https://contoso.b2clogin.com/`, который включает замыкающую косую черту)<br>
     2 \. Домен клиента AAD B2C (например, `contoso.onmicrosoft.com`)

   * [Зарегистрируйте веб-приложение](/azure/active-directory-b2c/tutorial-register-applications) &ndash; сделайте следующее при регистрации приложения:

     1 \. Задайте для параметра **веб-приложение или веб-API** значение **Да**.<br>
     2 \. Установите для параметра **Разрешить неявный поток** значение **Да**.<br>
     3 \. Добавьте **URL-адрес ответа** `https://localhost:5001/authentication/login-callback`.

     Запишите идентификатор приложения (идентификатор клиента) (например, `11111111-1111-1111-1111-111111111111`).

   * [Создание потоков пользователя](/azure/active-directory-b2c/tutorial-create-user-flows) & ndash; Создайте пользовательский поток регистрации и входа.

     Запишите имя потока пользователя для регистрации и входа, созданное для приложения (например, `B2C_1_signupsignin`).

1. Замените заполнители в следующей команде на записанные ранее сведения и выполните команду в командной оболочке:

   ```dotnetcli
   dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --client-id "{CLIENT ID}" --domain "{DOMAIN}" -ssp "{SIGN UP OR SIGN IN POLICY}"
   ```

   Чтобы указать расположение выходных данных, которое создает папку проекта, если она не существует, включите параметр OUTPUT в команду с путем (например, `-o BlazorSample`). Имя папки также станет частью имени проекта.

## <a name="authentication-package"></a>Пакет проверки подлинности

При создании приложения для использования отдельной учетной записи B2C (`IndividualB2C`) приложение автоматически получает ссылку на пакет для [библиотеки проверки подлинности Майкрософт](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`). Пакет предоставляет набор примитивов, которые помогают приложению проверять подлинность пользователей и получать маркеры для вызова защищенных интерфейсов API.

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
    authentication.Authority = 
        "{AAD B2C INSTANCE}{DOMAIN}/{SIGN UP OR SIGN IN POLICY}";
    authentication.ClientId = "{CLIENT ID}";
    authentication.ValidateAuthority = false;
});
```

Метод `AddMsalAuthentication` принимает обратный вызов для настройки параметров, необходимых для проверки подлинности приложения. Значения, необходимые для настройки приложения, можно получить из конфигурации AAD на портале Azure при регистрации приложения.

Шаблон Blazor-сборки не автоматически настраивает приложение для запроса маркера доступа для безопасного API. Чтобы создать маркер в рамках потока входа, добавьте область в область маркера доступа по умолчанию `MsalProviderOptions`:

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{API ID URI}/{SCOPE}");
});
```

## <a name="index-page"></a>Главная страница

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

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

* <xref:security/authentication/azure-ad-b2c>
* [Руководство по созданию клиента Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-create-tenant)
