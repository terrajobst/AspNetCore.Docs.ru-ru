---
title: "Настройка удостоверения ASP.NET Core"
author: AdrienTorris
description: "Значения по умолчанию ASP.NET Core Identity понять и настройки различных свойств Identity для использования пользовательских значений."
keywords: "Проверку подлинности ASP.NET Core, удостоверение,"
ms.author: scaddie
manager: wpickett
ms.date: 09/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: 2861ca474e7e82da81943966394a92040ce96ab8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="configure-identity"></a>Настройка удостоверения

ASP.NET Core Identity имеет некоторые виды поведения по умолчанию, которые можно легко переопределить в вашем приложении `Startup` класса.

## <a name="passwords-policy"></a>Политики паролей

По умолчанию удостоверение требует, что пароли содержат символ верхнего регистра, строчные буквы, цифры и не алфавитно-цифровой символ. Существуют также некоторые другие ограничения. Если вы хотите упростить ограничения для пароля, можно сделать это в `Startup` класса приложения.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core 2.0 добавлен `RequiredUniqueChars` свойство. В противном случае параметры совпадают с ASP.NET Core 1.x.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

`IdentityOptions.Password`имеет следующие свойства:
* `RequireDigit`: Необходимо указать число от 0-9 и пароль. По умолчанию — true.
* `RequiredLength`: Минимальная длина пароля. По умолчанию — 6.
* `RequireNonAlphanumeric`: Требуется не буквенно-цифровых символов в пароле. По умолчанию — true.
* `RequireUppercase`: Требуется верхний регистр символов в пароле. По умолчанию — true.
* `RequireLowercase`: Должен стоять символ нижнего регистра в пароле. По умолчанию — true.
* `RequiredUniqueChars`: Требует количества уникальных символов в пароле. По умолчанию используется значение 1.


## <a name="users-lockout"></a>Блокировки пользователя

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

`IdentityOptions.Lockout`имеет следующие свойства:
* `DefaultLockoutTimeSpan`: Количество времени, пользователь будет заблокирован при возникновении блокировки. Значение по умолчанию 5 минут.
* `MaxFailedAccessAttempts`: Число неудачных попыток доступа до пользователя будет заблокирована, если включена блокировка. По умолчанию — 5.
* `AllowedForNewUsers`: Определяет, если новый пользователь может быть заблокирован. По умолчанию — true.


## <a name="sign-in-settings"></a>Параметры входа

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

`IdentityOptions.SignIn`имеет следующие свойства:
* `RequireConfirmedEmail`: Требует подтверждения электронной почты для входа. По умолчанию используется значение false.
* `RequireConfirmedPhoneNumber`: Требуется номер телефона, подтвержденной для входа. По умолчанию используется значение false.


## <a name="user-validation-settings"></a>Параметры проверки пользователя

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

`IdentityOptions.User`имеет следующие свойства:
* `RequireUniqueEmail`: Требуется каждому пользователю иметь уникальный адрес электронной почты. По умолчанию используется значение false.
* `AllowedUserNameCharacters`: Допустимые символы в имени пользователя. Значение по умолчанию abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+.

## <a name="applications-cookie-settings"></a>Параметры файлов cookie приложения

Все параметры файла cookie приложения как политика паролей, можно изменить в `Startup` класса.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

В разделе `ConfigureServices` в `Startup` класса, можно настроить куки-файл приложения.

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

--- 

`CookieAuthenticationOptions`имеет следующие свойства:
* `Cookie.Name`: Имя файла cookie. Значение по умолчанию. AspNetCore.Cookies.
* `Cookie.HttpOnly`: Если значение равно true, куки-файл недоступен из скриптов на стороне клиента. По умолчанию — true.
* `ExpireTimeSpan`: Определяет, сколько времени хранится билет проверки подлинности в файл cookie будет оставаться действительным с момента его создания. По умолчанию — 14 дней.
* `LoginPath`: Если пользователь не авторизован, они будут перенаправляться к этому пути с именем входа. Значение по умолчанию или учетной записи или имени входа.
* `LogoutPath`: При выходе пользователя он будет перенаправлен по этому пути. Значение по умолчанию или учетной записи и выхода из системы.
* `AccessDeniedPath`: Если пользователь не может выполнить проверку авторизации, они будут перенаправляться к этому пути. Значение по умолчанию или учетной записи или доступ запрещен.
* `SlidingExpiration`: Если значение равно true, новый файл cookie будет выдан с новым окончанием срока действия текущего файла cookie при более половины срока. По умолчанию — true.
* `ReturnUrlParameter`: ReturnUrlParameter определяет имя параметра строки запроса, который добавляется по промежуточного слоя, когда код состояния 401 несанкционированный изменяется на перенаправление 302 пути входа.
* `AuthenticationScheme`: Это относится только к ASP.NET Core 1.x. Логическое имя для проверки подлинности конкретной схемы.
* `AutomaticAuthenticate`: Этот флаг относится только к ASP.NET Core 1.x. Если значение равно true, файл cookie проверки подлинности запустите при каждом запросе и попытка проверки и воссоздания любому участнику сериализованный она создана.

