---
title: Защита ASP.NET Core автономного приложения Blazor сборки с помощью Azure Active Directory
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-azure-active-directory
ms.openlocfilehash: 76bcac29d86a236e56c0eaea24a694c4845ecbcf
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083587"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-azure-active-directory"></a>Защита ASP.NET Core автономного приложения Blazor сборки с помощью Azure Active Directory

[Хавьер Калварро Воронков](https://github.com/javiercn) и [Люк ЛаСаМ](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Чтобы создать изолированное приложение Blazor сборки, использующее [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) для проверки подлинности:

[Создание клиента AAD и веб-приложения](/azure/active-directory/develop/v2-overview):

Зарегистрируйте приложение AAD в области **Azure Active Directory** > **Регистрация приложений** портал Azure:

1. Укажите **имя** приложения (например, **Blazor клиента AAD**).
1. Выберите **Поддерживаемые типы учетных записей**. Вы можете выбрать **учетные записи в этом каталоге организации только** для этого интерфейса.
1. Оставьте в раскрывающемся списке **URI перенаправления** значение **веб**и укажите универсальный код ресурса (uri) перенаправления `https://localhost:5001/authentication/login-callback`.
1. Отключите **разрешения** > установите флажок **предоставить доступ к администратору для OpenID Connect и offline_access разрешений** .
1. Выберите **Зарегистрировать**.

При **проверке Подлинности** > **конфигурации платформы** > **Web**:

1. Убедитесь, что **URI перенаправления** для `https://localhost:5001/authentication/login-callback` имеется.
1. Для **неявного предоставления**установите флажки для **маркеров доступа** и **маркеров идентификации**.
1. Остальные значения по умолчанию для приложения приемлемы для этого интерфейса.
1. Нажмите кнопку **Сохранить**.

Запишите следующие сведения:

* Идентификатор приложения (идентификатор клиента) (например, `11111111-1111-1111-1111-111111111111`)
* Идентификатор каталога (идентификатор клиента) (например, `22222222-2222-2222-2222-222222222222`)

Замените заполнители в следующей команде на записанные ранее сведения и выполните команду в командной оболочке:

```dotnetcli
dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "{TENANT ID}"
```

Чтобы указать расположение выходных данных, которое создает папку проекта, если она не существует, включите параметр OUTPUT в команду с путем (например, `-o BlazorSample`). Имя папки также станет частью имени проекта.

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
    authentication.Authority = "https://login.microsoftonline.com/{TENANT ID}";
    authentication.ClientId = "{CLIENT ID}";
});
```

Метод `AddMsalAuthentication` принимает обратный вызов для настройки параметров, необходимых для проверки подлинности приложения. Значения, необходимые для настройки приложения, можно получить из конфигурации AAD на портале Azure при регистрации приложения.

Шаблон Blazor-сборки не автоматически настраивает приложение для запроса маркера доступа для безопасного API. Чтобы создать маркер в рамках потока входа, добавьте область в область маркера доступа по умолчанию `MsalProviderOptions`:

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}");
});
```

> [!NOTE]
> Область маркера доступа по умолчанию должна иметь формат `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` (например, `11111111-1111-1111-1111-111111111111/API.Access`). Если для параметра области (как показано на портале Azure) предоставлена схема или схема и узел, то *клиентское приложение* создает необработанное исключение при получении от *приложения API сервера*ответа *401 с несанкционированным* доступом.

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

* <xref:security/authentication/azure-active-directory/index>
