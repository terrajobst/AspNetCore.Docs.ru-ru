---
title: Включить создание QR-код для TOTP приложения для проверки подлинности в ASP.NET Core
author: rick-anderson
description: Узнайте, как включить QR создание кода для приложения для проверки подлинности TOTP, работающие с двухфакторной проверки подлинности ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/24/2017
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: b0d8f104119340b97bd65f1826bb921ca875acf8
ms.sourcegitcommit: 1faf2525902236428dae6a59e375519bafd5d6d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/28/2018
ms.locfileid: "37089975"
---
# <a name="enable-qr-code-generation-for-totp-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="f09ff-103">Включить создание QR-код для TOTP приложения для проверки подлинности в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f09ff-103">Enable QR Code generation for TOTP authenticator apps in ASP.NET Core</span></span>

<span data-ttu-id="f09ff-104">ASP.NET Core поставляется с поддержки проверки подлинности приложений для отдельных проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="f09ff-104">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="f09ff-105">Два фактора проверки подлинности (2FA) приложения для проверки подлинности, с помощью синхронизированного одноразовый пароль алгоритм (TOTP), — это рекомендованный подход для 2FA отрасли.</span><span class="sxs-lookup"><span data-stu-id="f09ff-105">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="f09ff-106">2FA с помощью TOTP предпочтительнее SMS 2FA.</span><span class="sxs-lookup"><span data-stu-id="f09ff-106">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="f09ff-107">Приложение проверки подлинности предоставляет 6 – 8 цифр, какие пользователи должны ввести после подтверждения свое имя пользователя и пароль.</span><span class="sxs-lookup"><span data-stu-id="f09ff-107">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="f09ff-108">Обычно приложение проверки подлинности устанавливается на смартфоне.</span><span class="sxs-lookup"><span data-stu-id="f09ff-108">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="f09ff-109">Веб-приложения ASP.NET Core шаблонов поддерживает атрибуты проверки подлинности, но не обеспечивают поддержку создания QRCode.</span><span class="sxs-lookup"><span data-stu-id="f09ff-109">The ASP.NET Core web app templates support authenticators, but don't provide support for QRCode generation.</span></span> <span data-ttu-id="f09ff-110">Генераторы QRCode простота установки 2FA.</span><span class="sxs-lookup"><span data-stu-id="f09ff-110">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="f09ff-111">В этом документе подробно рассматривается добавление [QR-код](https://wikipedia.org/wiki/QR_code) поколения на страницу настройки 2FA.</span><span class="sxs-lookup"><span data-stu-id="f09ff-111">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="f09ff-112">Добавление на страницу настройки 2FA QR-коды</span><span class="sxs-lookup"><span data-stu-id="f09ff-112">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="f09ff-113">В этих инструкциях используется *qrcode.js* из https://davidshimjs.github.io/qrcodejs/ репозитория.</span><span class="sxs-lookup"><span data-stu-id="f09ff-113">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="f09ff-114">Загрузить [библиотека javascript qrcode.js](https://davidshimjs.github.io/qrcodejs/) для `wwwroot\lib` в папке проекта.</span><span class="sxs-lookup"><span data-stu-id="f09ff-114">Download the [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

* <span data-ttu-id="f09ff-115">В *Pages\Account\Manage\EnableAuthenticator.cshtml* (страниц Razor) или *Views\Manage\EnableAuthenticator.cshtml* (MVC), найдите `Scripts` в конце файла:</span><span class="sxs-lookup"><span data-stu-id="f09ff-115">In *Pages\Account\Manage\EnableAuthenticator.cshtml* (Razor Pages) or *Views\Manage\EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="f09ff-116">Обновление `Scripts` раздел, чтобы добавить ссылку на `qrcodejs` библиотеки, добавленные и вызова для создания QR-код.</span><span class="sxs-lookup"><span data-stu-id="f09ff-116">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="f09ff-117">Он должен выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="f09ff-117">It should look as follows:</span></span>

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

* <span data-ttu-id="f09ff-118">Удаление абзаца, в которой представлены ссылки на эти инструкции.</span><span class="sxs-lookup"><span data-stu-id="f09ff-118">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="f09ff-119">Запустите приложение и убедитесь, что сканирование QR-кода и проверить код, который подтверждает средством проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="f09ff-119">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="f09ff-120">Изменение имени узла в QR-кода</span><span class="sxs-lookup"><span data-stu-id="f09ff-120">Change the site name in the QR Code</span></span>

<span data-ttu-id="f09ff-121">Имя узла в QR-кода берется из имени проекта, выбранный при первоначальном создании проекта.</span><span class="sxs-lookup"><span data-stu-id="f09ff-121">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="f09ff-122">Его можно изменить путем поиска `GenerateQrCodeUri(string email, string unformattedKey)` метод в *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* (страниц Razor) файла или *Controllers\ManageController.cs* файла (MVC).</span><span class="sxs-lookup"><span data-stu-id="f09ff-122">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* (Razor Pages) file or the *Controllers\ManageController.cs* (MVC) file.</span></span>

<span data-ttu-id="f09ff-123">Код по умолчанию на основе шаблона выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="f09ff-123">The default code from the template looks as follows:</span></span>

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

<span data-ttu-id="f09ff-124">Второй параметр в вызове `string.Format` — это имя узла, взяты из имя вашего решения.</span><span class="sxs-lookup"><span data-stu-id="f09ff-124">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="f09ff-125">Его можно изменить в любое значение, но он всегда должен быть в кодировке URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="f09ff-125">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="f09ff-126">С помощью другой библиотекой QR-кода</span><span class="sxs-lookup"><span data-stu-id="f09ff-126">Using a different QR Code library</span></span>

<span data-ttu-id="f09ff-127">Библиотека QR-кода можно заменить предпочтительные библиотеки.</span><span class="sxs-lookup"><span data-stu-id="f09ff-127">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="f09ff-128">В HTML `qrCode` библиотека предоставляет элемент, в который можно поместить QR-кода с любой механизм.</span><span class="sxs-lookup"><span data-stu-id="f09ff-128">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="f09ff-129">Правильно отформатированный URL-адрес для QR-код доступен в:</span><span class="sxs-lookup"><span data-stu-id="f09ff-129">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="f09ff-130">`AuthenticatorUri` Свойства модели.</span><span class="sxs-lookup"><span data-stu-id="f09ff-130">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="f09ff-131">`data-url` свойство в `qrCodeData` элемент.</span><span class="sxs-lookup"><span data-stu-id="f09ff-131">`data-url` property in the `qrCodeData` element.</span></span>

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="f09ff-132">Отклонение по времени клиента и сервера TOTP</span><span class="sxs-lookup"><span data-stu-id="f09ff-132">TOTP client and server time skew</span></span>

<span data-ttu-id="f09ff-133">Проверка подлинности TOTP (на основе времени одноразового пароля) зависит от сервера и проверки подлинности устройства, необходимости точного времени.</span><span class="sxs-lookup"><span data-stu-id="f09ff-133">TOTP (Time-based One-Time Password) authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="f09ff-134">Токены только последние 30 секунд.</span><span class="sxs-lookup"><span data-stu-id="f09ff-134">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="f09ff-135">Если имена входа 2FA TOTP не удается, проверьте время сервера точными и предпочтительно синхронизированные точные службе NTP.</span><span class="sxs-lookup"><span data-stu-id="f09ff-135">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>
