---
title: Включить создание QR-код для приложения для проверки подлинности в ASP.NET Core
author: rick-anderson
description: Узнайте, как включить QR создание кода для приложения, работающие с ASP.NET Core двухфакторной проверки подлинности для проверки подлинности.
manager: wpickett
ms.author: riande
ms.date: 09/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: 463c1c7b3aef624622e34943f1a7a518e658a037
ms.sourcegitcommit: 4e3497bda0c3e5011ffba3717eb61a1d46c61c15
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/14/2018
ms.locfileid: "35613038"
---
# <a name="enable-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="12e2e-103">Включить создание QR-код для приложения для проверки подлинности в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="12e2e-103">Enable QR Code generation for authenticator apps in ASP.NET Core</span></span>

<span data-ttu-id="12e2e-104">Примечание: Этот раздел относится к ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="12e2e-104">Note: This topic applies to ASP.NET Core 2.x</span></span>

<span data-ttu-id="12e2e-105">ASP.NET Core поставляется с поддержки проверки подлинности приложений для отдельных проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="12e2e-105">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="12e2e-106">Два фактора проверки подлинности (2FA) приложения для проверки подлинности, с помощью синхронизированного одноразовый пароль алгоритм (TOTP), — это рекомендованный подход для 2FA отрасли.</span><span class="sxs-lookup"><span data-stu-id="12e2e-106">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="12e2e-107">2FA с помощью TOTP предпочтительнее SMS 2FA.</span><span class="sxs-lookup"><span data-stu-id="12e2e-107">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="12e2e-108">Приложение проверки подлинности предоставляет 6 – 8 цифр, какие пользователи должны ввести после подтверждения свое имя пользователя и пароль.</span><span class="sxs-lookup"><span data-stu-id="12e2e-108">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="12e2e-109">Обычно приложение проверки подлинности устанавливается на смартфоне.</span><span class="sxs-lookup"><span data-stu-id="12e2e-109">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="12e2e-110">Веб-приложения ASP.NET Core шаблонов поддерживает атрибуты проверки подлинности, но не обеспечивают поддержку создания QRCode.</span><span class="sxs-lookup"><span data-stu-id="12e2e-110">The ASP.NET Core web app templates support authenticators, but don't provide support for QRCode generation.</span></span> <span data-ttu-id="12e2e-111">Генераторы QRCode простота установки 2FA.</span><span class="sxs-lookup"><span data-stu-id="12e2e-111">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="12e2e-112">В этом документе подробно рассматривается добавление [QR-код](https://wikipedia.org/wiki/QR_code) поколения на страницу настройки 2FA.</span><span class="sxs-lookup"><span data-stu-id="12e2e-112">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="12e2e-113">Добавление на страницу настройки 2FA QR-коды</span><span class="sxs-lookup"><span data-stu-id="12e2e-113">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="12e2e-114">В этих инструкциях используется *qrcode.js* из https://davidshimjs.github.io/qrcodejs/ репозитория.</span><span class="sxs-lookup"><span data-stu-id="12e2e-114">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="12e2e-115">Загрузить [библиотека javascript qrcode.js](https://davidshimjs.github.io/qrcodejs/) для `wwwroot\lib` в папке проекта.</span><span class="sxs-lookup"><span data-stu-id="12e2e-115">Download the [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

* <span data-ttu-id="12e2e-116">В *Pages\Account\Manage\EnableAuthenticator.cshtml* (страниц Razor) или *Views\Manage\EnableAuthenticator.cshtml* (MVC), найдите `Scripts` в конце файла:</span><span class="sxs-lookup"><span data-stu-id="12e2e-116">In *Pages\Account\Manage\EnableAuthenticator.cshtml* (Razor Pages) or *Views\Manage\EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="12e2e-117">Обновление `Scripts` раздел, чтобы добавить ссылку на `qrcodejs` библиотеки, добавленные и вызова для создания QR-код.</span><span class="sxs-lookup"><span data-stu-id="12e2e-117">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="12e2e-118">Он должен выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="12e2e-118">It should look as follows:</span></span>

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

* <span data-ttu-id="12e2e-119">Удаление абзаца, в которой представлены ссылки на эти инструкции.</span><span class="sxs-lookup"><span data-stu-id="12e2e-119">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="12e2e-120">Запустите приложение и убедитесь, что сканирование QR-кода и проверить код, который подтверждает средством проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="12e2e-120">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="12e2e-121">Изменение имени узла в QR-кода</span><span class="sxs-lookup"><span data-stu-id="12e2e-121">Change the site name in the QR Code</span></span>

<span data-ttu-id="12e2e-122">Имя узла в QR-кода берется из имени проекта, выбранный при первоначальном создании проекта.</span><span class="sxs-lookup"><span data-stu-id="12e2e-122">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="12e2e-123">Его можно изменить путем поиска `GenerateQrCodeUri(string email, string unformattedKey)` метод в *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* (страниц Razor) файла или *Controllers\ManageController.cs* файла (MVC).</span><span class="sxs-lookup"><span data-stu-id="12e2e-123">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* (Razor Pages) file or the *Controllers\ManageController.cs* (MVC) file.</span></span> 

<span data-ttu-id="12e2e-124">Код по умолчанию на основе шаблона выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="12e2e-124">The default code from the template looks as follows:</span></span>

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

<span data-ttu-id="12e2e-125">Второй параметр в вызове `string.Format` — это имя узла, взяты из имя вашего решения.</span><span class="sxs-lookup"><span data-stu-id="12e2e-125">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="12e2e-126">Его можно изменить в любое значение, но он всегда должен быть в кодировке URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="12e2e-126">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="12e2e-127">С помощью другой библиотекой QR-кода</span><span class="sxs-lookup"><span data-stu-id="12e2e-127">Using a different QR Code library</span></span>

<span data-ttu-id="12e2e-128">Библиотека QR-кода можно заменить предпочтительные библиотеки.</span><span class="sxs-lookup"><span data-stu-id="12e2e-128">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="12e2e-129">В HTML `qrCode` библиотека предоставляет элемент, в который можно поместить QR-кода с любой механизм.</span><span class="sxs-lookup"><span data-stu-id="12e2e-129">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="12e2e-130">Правильно отформатированный URL-адрес для QR-код доступен в:</span><span class="sxs-lookup"><span data-stu-id="12e2e-130">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="12e2e-131">`AuthenticatorUri` Свойства модели.</span><span class="sxs-lookup"><span data-stu-id="12e2e-131">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="12e2e-132">`data-url` свойство в `qrCodeData` элемент.</span><span class="sxs-lookup"><span data-stu-id="12e2e-132">`data-url` property in the `qrCodeData` element.</span></span> 

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="12e2e-133">Отклонение по времени клиента и сервера TOTP</span><span class="sxs-lookup"><span data-stu-id="12e2e-133">TOTP client and server time skew</span></span>

<span data-ttu-id="12e2e-134">Проверка подлинности TOTP (на основе времени одноразового пароля) зависит от сервера и проверки подлинности устройства, необходимости точного времени.</span><span class="sxs-lookup"><span data-stu-id="12e2e-134">TOTP (Time-based One-Time Password) authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="12e2e-135">Токены только последние 30 секунд.</span><span class="sxs-lookup"><span data-stu-id="12e2e-135">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="12e2e-136">Если имена входа 2FA TOTP не удается, проверьте время сервера точными и предпочтительно синхронизированные точные службе NTP.</span><span class="sxs-lookup"><span data-stu-id="12e2e-136">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>
