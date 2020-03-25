---
title: Защита ASP.NET Core автономного приложения Blazor сборки с помощью библиотеки проверки подлинности
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-authentication-library
ms.openlocfilehash: ea50d94835b044f9c3d6a0561868f081d32cb62a
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219013"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-the-authentication-library"></a>Защита ASP.NET Core автономного приложения Blazor сборки с помощью библиотеки проверки подлинности

[Хавьер Калварро Воронков](https://github.com/javiercn) и [Люк ЛаСаМ](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

*Для Azure Active Directory (AAD) и Azure Active Directory B2C (AAD B2C) не следуйте указаниям в этом разделе. См. разделы, посвященные AAD и AAD B2C, в этом узле оглавления.*

Чтобы создать изолированное приложение Blazorной сборки, использующее библиотеку `Microsoft.AspNetCore.Components.WebAssembly.Authentication`, выполните в командной оболочке следующую команду:

```dotnetcli
dotnet new blazorwasm -au Individual
```

Чтобы указать расположение выходных данных, которое создает папку проекта, если она не существует, включите параметр OUTPUT в команду с путем (например, `-o BlazorSample`). Имя папки также станет частью имени проекта.

В Visual Studio [создайте Blazor приложение сборки](xref:blazor/get-started). Настройте **проверку подлинности** для **отдельных учетных записей пользователей** с помощью параметра **сохранить учетные записи пользователей в приложении** .

## <a name="authentication-package"></a>Пакет проверки подлинности

Когда приложение создается для использования отдельных учетных записей пользователей, приложение автоматически получает ссылку на пакет для `Microsoft.AspNetCore.Components.WebAssembly.Authentication` пакета в файле проекта приложения. Пакет предоставляет набор примитивов, которые помогают приложению проверять подлинность пользователей и получать маркеры для вызова защищенных интерфейсов API.

При добавлении проверки подлинности в приложение вручную добавьте пакет в файл проекта приложения:

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

Замените `{VERSION}` в предыдущей ссылке на пакет версией пакета `Microsoft.AspNetCore.Blazor.Templates`, показанного в статье <xref:blazor/get-started>.

## <a name="authentication-service-support"></a>Поддержка службы проверки подлинности

Поддержка проверки подлинности пользователей регистрируется в контейнере службы с помощью метода расширения `AddOidcAuthentication`, предоставленного пакетом `Microsoft.AspNetCore.Components.WebAssembly.Authentication`. Этот метод настраивает все службы, необходимые для взаимодействия приложения с поставщиком удостоверений (IP).

*Program.cs*:

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    options.ProviderOptions.Authority = "{AUTHORITY}";
    options.ProviderOptions.ClientId = "{CLIENT ID}";
});
```

Поддержка проверки подлинности для автономных приложений предлагается с помощью Open ID Connect (OIDC). Метод `AddOidcAuthentication` принимает обратный вызов для настройки параметров, необходимых для проверки подлинности приложения с помощью OIDC. Значения, необходимые для настройки приложения, можно получить из IP-адреса, совместимого с OIDC. Получите значения при регистрации приложения, которое обычно происходит на веб-портале.

## <a name="index-page"></a>Страница индексации

[!INCLUDE[](~/includes/blazor-security/index-page-authentication.md)]

## <a name="app-component"></a>Компонент приложения

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a>Компонент Редиректтологин

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a>Компонент Логиндисплай

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a>Компонент проверки подлинности

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]
