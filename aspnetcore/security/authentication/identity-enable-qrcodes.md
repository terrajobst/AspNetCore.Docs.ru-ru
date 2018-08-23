---
title: Включение создания QR-код для приложения TOTP для проверки подлинности в ASP.NET Core
author: rick-anderson
description: Узнайте, как включение создания кода QR для приложений проверки подлинности TOTP, которые работают с ASP.NET Core двухфакторной проверки подлинности.
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: 4535efdde7340436c6a508848bff86e103df570e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838495"
---
# <a name="enable-qr-code-generation-for-totp-authenticator-apps-in-aspnet-core"></a>Включение создания QR-код для приложения TOTP для проверки подлинности в ASP.NET Core

::: moniker range="<= aspnetcore-2.0"

QR-коды требуется ASP.NET Core 2.0 или более поздней.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

ASP.NET Core поставляется с поддержкой проверки подлинности приложений для отдельной проверки подлинности. Два фактора проверки подлинности (2FA) проверки подлинности приложения с помощью по времени одноразовый пароль алгоритм (TOTP), являются отрасли, рекомендуемый подход для 2FA. 2FA с помощью TOTP предпочтительнее SMS 2FA. Приложение для проверки подлинности предоставляет 6 to 8 цифр, какие пользователи должны ввести после подтверждения имени пользователя и пароля. Обычно приложение для проверки подлинности устанавливается на смартфоне.

Шаблоны веб-приложений ASP.NET Core поддерживает структур проверки подлинности, но не обеспечивают поддержку создания QRCode. Генераторы QRCode упростить настройку 2FA. Этот документ поможет выполнить добавление [QR-код](https://wikipedia.org/wiki/QR_code) поколения на страницу настройки 2FA.

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a>Добавление на страницу настройки 2FA QR-коды

В этих инструкциях используется *qrcode.js* из https://davidshimjs.github.io/qrcodejs/ репозитория.

* Скачайте [библиотека javascript qrcode.js](https://davidshimjs.github.io/qrcodejs/) для `wwwroot\lib` в проекте.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* Следуйте инструкциям в [удостоверений каркаса](xref:security/authentication/scaffold-identity) для создания */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.
* В */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*, найдите `Scripts` в конце файла:

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* В *Pages/Account/Manage/EnableAuthenticator.cshtml* (Razor Pages) или *Views/Manage/EnableAuthenticator.cshtml* (MVC), найдите `Scripts` в конце файла:

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* Обновление `Scripts` раздел, чтобы добавить ссылку на `qrcodejs` библиотеки, которые вы добавили и вызов, чтобы создать QR-код. Он должен выглядеть следующим образом:

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

* Удаление абзаца, в которой представлены ссылки на эти инструкции.

Запустите приложение и убедитесь, что вы можете просканировать QR-код и проверку кода, который подтверждает средством проверки подлинности.

## <a name="change-the-site-name-in-the-qr-code"></a>Изменение имени узла в QR-код

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Имя сайта в QR-код берется из имени проекта, выбранный при первоначальном создании проекта. Его можно изменить путем поиска `GenerateQrCodeUri(string email, string unformattedKey)` метод в */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Имя сайта в QR-код берется из имени проекта, выбранный при первоначальном создании проекта. Его можно изменить путем поиска `GenerateQrCodeUri(string email, string unformattedKey)` метод в *Pages/Account/Manage/EnableAuthenticator.cshtml.cs* (Razor Pages) файла или *Controllers/ManageController.cs* файл (MVC).

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

Код по умолчанию из шаблона выглядит следующим образом:

```c#
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenicatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

Второй параметр в вызове `string.Format` — это имя узла, взятое из имя решения. Его можно изменить в любое значение, но он всегда должен быть в кодировке URL-адрес.

## <a name="using-a-different-qr-code-library"></a>С помощью другой библиотеке QR-кода

Библиотека QR-код можно заменить основной библиотеки. HTML-код содержит `qrCode` библиотека предоставляет элемент, в который можно поместить QR-кода, какой бы механизм.

Правильно отформатированный URL-адрес для QR-код доступен в:

* `AuthenticatorUri` Свойства модели.
* `data-url` свойство в `qrCodeData` элемент.

## <a name="totp-client-and-server-time-skew"></a>Неравномерное распределение по времени клиента и сервера TOTP

Проверка подлинности TOTP (по времени одноразовый пароль) зависит от сервера и проверки подлинности устройства, наличие точного времени. Маркеры только продолжаться в течение 30 секунд. Если имена входа 2FA TOTP не выполняются, проверьте время сервера точной и предпочтительно синхронизируется с точным службу NTP.

::: moniker-end
