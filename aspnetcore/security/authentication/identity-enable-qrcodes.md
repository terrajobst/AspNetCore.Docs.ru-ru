---
title: Включение создания QR-кода для приложений TOTP Authenticator в ASP.NET Core
author: rick-anderson
description: Узнайте, как включить создание QR-кода для приложений TOTP Authenticator, работающих с ASP.NET Core двухфакторной проверки подлинности.
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: a7fdc86b3fe94e714e5147c89a32fce13757d1c1
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78654160"
---
# <a name="enable-qr-code-generation-for-totp-authenticator-apps-in-aspnet-core"></a>Включение создания QR-кода для приложений TOTP Authenticator в ASP.NET Core

::: moniker range="<= aspnetcore-2.0"

Для QR-кодов требуется ASP.NET Core 2,0 или более поздней версии.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

ASP.NET Core поставляется с поддержкой приложений проверки подлинности для индивидуальной проверки подлинности. Два фактора проверки подлинности (2FA) проверки подлинности приложения с помощью по времени одноразовый пароль алгоритм (TOTP), являются отрасли, рекомендуемый подход для 2FA. 2FA с помощью TOTP предпочтительнее SMS 2FA. Приложение для проверки подлинности предоставляет код из 6 – 8 цифр, который пользователи должны ввести после подтверждения имени пользователя и пароля. Обычно приложение для проверки подлинности устанавливается на смарт-телефон.

Шаблоны веб-приложений ASP.NET Core поддерживают средства проверки подлинности, но не обеспечивают поддержку создания Кркоде. Кркоденые генераторы упрощают настройку 2FA. В этом документе рассматривается добавление QR- [кода](https://wikipedia.org/wiki/QR_code) на страницу настройки 2FA.

Двухфакторная проверка подлинности не выполняется с помощью внешнего поставщика проверки подлинности, например [Google](xref:security/authentication/google-logins) или [Facebook](xref:security/authentication/facebook-logins). Внешние имена входа защищены любым механизмом, предоставляемым внешним поставщиком входа. Рассмотрим, например, для поставщика проверки подлинности [Майкрософт](xref:security/authentication/microsoft-logins) требуется аппаратный ключ или другой подход 2FA. Если шаблоны по умолчанию применяют "Local" 2FA, то пользователям потребуется выполнить два подхода 2FA, что не является часто используемым сценарием.

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a>Добавление QR-кодов на страницу настройки 2FA

Эти инструкции используют *кркоде. js* из репозитория https://davidshimjs.github.io/qrcodejs/.

* Скачайте [библиотеку JavaScript кркоде. js](https://davidshimjs.github.io/qrcodejs/) в папку `wwwroot\lib` проекта.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* Следуйте инструкциям в разделе [удостоверение формирования шаблонов](xref:security/authentication/scaffold-identity) , чтобы создать */Ареас/идентити/Пажес/аккаунт/манаже/енаблеаусентикатор.кштмл*.
* В */Ареас/идентити/Пажес/аккаунт/манаже/енаблеаусентикатор.кштмл*в конце файла выберите раздел `Scripts`:

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* В окне *страницы/учетная запись/управление/енаблеаусентикатор. cshtml* (Razor Pages) или *views/Manage/енаблеаусентикатор. cshtml* (MVC) в конце файла выберите раздел `Scripts`.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* Обновите раздел `Scripts`, чтобы добавить ссылку на добавленную библиотеку `qrcodejs` и вызов для создания QR-кода. Он должен выглядеть следующим образом:

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")

    <script type="text/javascript" src="~/lib/qrcode.js"></script>
    <script type="text/javascript">
        new QRCode(document.getElementById("qrCode"),
            {
                text: "@Html.Raw(Model.AuthenticatorUri)",
                width: 150,
                height: 150
            });
    </script>
}
```

* Удалите абзац со ссылкой на эти инструкции.

Запустите приложение и убедитесь, что вы можете просмотреть QR-код и проверить код, который проверяется средством проверки подлинности.

## <a name="change-the-site-name-in-the-qr-code"></a>Изменение имени сайта в QR-коде

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Имя сайта в QR-коде берется из имени проекта, выбранного при первоначальном создании проекта. Его можно изменить, выполнив поиск метода `GenerateQrCodeUri(string email, string unformattedKey)` в */Ареас/идентити/Пажес/аккаунт/манаже/енаблеаусентикатор.кштмл.КС*.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Имя сайта в QR-коде берется из имени проекта, выбранного при первоначальном создании проекта. Его можно изменить, выполнив поиск метода `GenerateQrCodeUri(string email, string unformattedKey)` в файле *pages/Account/Manage/енаблеаусентикатор. cshtml. CS* (Razor Pages) или в файле *Controllers/манажеконтроллер. CS* (MVC).

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

Код по умолчанию из шаблона выглядит следующим образом:

```csharp
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenticatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

Вторым параметром в вызове `string.Format` является имя вашего сайта, взятое из имени решения. Его можно изменить на любое значение, но оно всегда должно быть в кодировке URL-адреса.

## <a name="using-a-different-qr-code-library"></a>Использование другой библиотеки QR-кодов

Библиотеку QR-кода можно заменить предпочтительной библиотекой. HTML содержит элемент `qrCode`, в который можно поместить QR-код любым механизмом, предоставляемым библиотекой.

URL-адрес в правильно отформатированном коде QR доступен в:

* Свойство `AuthenticatorUri` модели.
* `data-url` свойство в элементе `qrCodeData`.

## <a name="totp-client-and-server-time-skew"></a>Отклонение времени клиента и сервера TOTP

TOTP (одноразовый пароль на основе времени) зависит от сервера и устройства проверки подлинности с точным временем. Маркеры только последний раз в течение 30 секунд. При сбое имен входа TOTP 2FA убедитесь, что время сервера точное и желательно синхронизироваться с точной службой NTP.

::: moniker-end
