---
title: "Настройка удостоверения ASP.NET Core"
author: AdrienTorris
description: "Значения по умолчанию ASP.NET Core Identity понять и настройки различных свойств Identity для использования пользовательских значений."
ms.author: scaddie
manager: wpickett
ms.date: 01/11/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: d3a13d1cef3417522460b44c52c1361c3e9d1162
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="configure-identity"></a>Настройка удостоверения

ASP.NET Core Identity имеет общие поведения в приложениях, например политика паролей, время блокировки и параметры файлов cookie, которые можно легко переопределить в вашем приложении `Startup` класса.

## <a name="passwords-policy"></a>Политики паролей

По умолчанию удостоверение требует, что пароли содержат символ верхнего регистра, строчные буквы, цифры и не алфавитно-цифровой символ. Существуют также некоторые другие ограничения. Чтобы упростить ограничения для пароля, измените `ConfigureServices` метод `Startup` класса приложения.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core 2.0 добавлен `RequiredUniqueChars` свойство. В противном случае параметры совпадают с ASP.NET Core 1.x.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

`IdentityOptions.Password`имеет следующие свойства:

| Свойство.                | Описание:                       | По умолчанию |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | Необходимо указать число от 0-9 и пароль. | true |
| `RequiredLength`        | Минимальная длина пароля. | 6 |
| `RequireNonAlphanumeric`| Требуется не буквенно-цифровых символов в пароле. | true |
| `RequireUppercase`      | Требуется верхний регистр символов в пароле. | true |
| `RequireLowercase`      | Должен стоять символ нижнего регистра в пароле. | true |
| `RequiredUniqueChars`   | Требует количества уникальных символов в пароле. | 1 |


## <a name="users-lockout"></a>Блокировки пользователя

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

`IdentityOptions.Lockout`имеет следующие свойства:

| Свойство.                | Описание:                       | По умолчанию |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | Количество времени, пользователь будет заблокирован при возникновении блокировки.  | 5 минут  |
| `MaxFailedAccessAttempts` | Число неудачных попыток доступа до пользователя будет заблокирована, если включена блокировка.  | 5 |
| `AllowedForNewUsers` | Определяет, если новый пользователь может быть заблокирован.  | true |

## <a name="sign-in-settings"></a>Параметры входа

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

`IdentityOptions.SignIn`имеет следующие свойства:

| Свойство.                | Описание:                       | По умолчанию |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | Требует подтверждения электронной почты для входа. | False  |
| `RequireConfirmedPhoneNumber` |  Требуется номер телефона, подтвержденной для входа. | False  |

## <a name="user-validation-settings"></a>Параметры проверки пользователя

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

`IdentityOptions.User`имеет следующие свойства:

| Свойство.                | Описание:                       | По умолчанию |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | Требуется иметь уникальный адрес электронной почты пользователя. | False  |
| `AllowedUserNameCharacters`  | Допустимые символы в имени пользователя. | abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+ |


## <a name="applications-cookie-settings"></a>Параметры файлов cookie приложения

Все параметры файла cookie приложения как политика паролей, можно изменить в `Startup` класса.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

В разделе `ConfigureServices` в `Startup` класса, можно настроить куки-файл приложения.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

`CookieAuthenticationOptions`имеет следующие свойства:

| Свойство.                | Описание:                       | По умолчанию |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | Имя файла cookie.  | .AspNetCore.Cookies.  |
| `Cookie.HttpOnly`  | Если значение равно true, куки-файл недоступен из скриптов на стороне клиента.  |  true |
| `ExpireTimeSpan`  | Определяет, сколько времени хранится билет проверки подлинности в файл cookie будет оставаться действительным с момента его создания.  | 14 дней  |
| `LoginPath`  | Если пользователь не авторизован, они будут перенаправляться по этому пути, имени входа. | /Account/Login  |
| `LogoutPath`  | При выходе пользователя он будет перенаправлен по этому пути.  | /Account/Logout  |
| `AccessDeniedPath`  | При сбое проверка авторизации пользователя они будут перенаправляться к этому пути.  |   |
| `SlidingExpiration`  | Если значение равно true, новый файл cookie будет выдан с новым окончанием срока действия текущего файла cookie при более половины срока.  | /Account/AccessDenied |
| `ReturnUrlParameter`  | Определяет имя параметра строки запроса, который добавляется по промежуточного слоя, когда код состояния 401 несанкционированный изменяется на перенаправление 302 пути входа.  |  true |
| `AuthenticationScheme`  | Это относится только к ASP.NET Core 1.x. Логическое имя для проверки подлинности конкретной схемы. |  |
| `AutomaticAuthenticate`  | Этот флаг относится только к ASP.NET Core 1.x. Если значение равно true, файл cookie проверки подлинности запустите при каждом запросе и попытка проверки и воссоздания любому участнику сериализованный она создана.  |  |
