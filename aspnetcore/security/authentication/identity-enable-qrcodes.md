---
title: Включение создания QR-код для приложения TOTP для проверки подлинности в ASP.NET Core
author: rick-anderson
description: Узнайте, как включение создания кода QR для приложений проверки подлинности TOTP, которые работают с ASP.NET Core двухфакторной проверки подлинности.
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: 437f354f71128a98bae9abdced291e04efc9f48e
ms.sourcegitcommit: fc7eb4243188950ae1f1b52669edc007e9d0798d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/07/2018
ms.locfileid: "51225386"
---
# <a name="enable-qr-code-generation-for-totp-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="2404a-103">Включение создания QR-код для приложения TOTP для проверки подлинности в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2404a-103">Enable QR Code generation for TOTP authenticator apps in ASP.NET Core</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="2404a-104">QR-коды требуется ASP.NET Core 2.0 или более поздней.</span><span class="sxs-lookup"><span data-stu-id="2404a-104">QR Codes requires ASP.NET Core 2.0 or later.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2404a-105">ASP.NET Core поставляется с поддержкой проверки подлинности приложений для отдельной проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="2404a-105">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="2404a-106">Два фактора проверки подлинности (2FA) проверки подлинности приложения с помощью по времени одноразовый пароль алгоритм (TOTP), являются отрасли, рекомендуемый подход для 2FA.</span><span class="sxs-lookup"><span data-stu-id="2404a-106">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="2404a-107">2FA с помощью TOTP предпочтительнее SMS 2FA.</span><span class="sxs-lookup"><span data-stu-id="2404a-107">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="2404a-108">Приложение для проверки подлинности предоставляет 6 to 8 цифр, какие пользователи должны ввести после подтверждения имени пользователя и пароля.</span><span class="sxs-lookup"><span data-stu-id="2404a-108">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="2404a-109">Обычно приложение для проверки подлинности устанавливается на смартфоне.</span><span class="sxs-lookup"><span data-stu-id="2404a-109">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="2404a-110">Шаблоны веб-приложений ASP.NET Core поддерживает структур проверки подлинности, но не обеспечивают поддержку создания QRCode.</span><span class="sxs-lookup"><span data-stu-id="2404a-110">The ASP.NET Core web app templates support authenticators, but don't provide support for QRCode generation.</span></span> <span data-ttu-id="2404a-111">Генераторы QRCode упростить настройку 2FA.</span><span class="sxs-lookup"><span data-stu-id="2404a-111">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="2404a-112">Этот документ поможет выполнить добавление [QR-код](https://wikipedia.org/wiki/QR_code) поколения на страницу настройки 2FA.</span><span class="sxs-lookup"><span data-stu-id="2404a-112">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="2404a-113">Добавление на страницу настройки 2FA QR-коды</span><span class="sxs-lookup"><span data-stu-id="2404a-113">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="2404a-114">В этих инструкциях используется *qrcode.js* из https://davidshimjs.github.io/qrcodejs/ репозитория.</span><span class="sxs-lookup"><span data-stu-id="2404a-114">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="2404a-115">Скачайте [библиотека javascript qrcode.js](https://davidshimjs.github.io/qrcodejs/) для `wwwroot\lib` в проекте.</span><span class="sxs-lookup"><span data-stu-id="2404a-115">Download the [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="2404a-116">Следуйте инструкциям в [удостоверений каркаса](xref:security/authentication/scaffold-identity) для создания */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2404a-116">Follow the instructions in [Scaffold Identity](xref:security/authentication/scaffold-identity) to generate */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span></span>
* <span data-ttu-id="2404a-117">В */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*, найдите `Scripts` в конце файла:</span><span class="sxs-lookup"><span data-stu-id="2404a-117">In */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*, locate the `Scripts` section at the end of the file:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <span data-ttu-id="2404a-118">В *Pages/Account/Manage/EnableAuthenticator.cshtml* (Razor Pages) или *Views/Manage/EnableAuthenticator.cshtml* (MVC), найдите `Scripts` в конце файла:</span><span class="sxs-lookup"><span data-stu-id="2404a-118">In *Pages/Account/Manage/EnableAuthenticator.cshtml* (Razor Pages) or *Views/Manage/EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="2404a-119">Обновление `Scripts` раздел, чтобы добавить ссылку на `qrcodejs` библиотеки, которые вы добавили и вызов, чтобы создать QR-код.</span><span class="sxs-lookup"><span data-stu-id="2404a-119">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="2404a-120">Он должен выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="2404a-120">It should look as follows:</span></span>

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

* <span data-ttu-id="2404a-121">Удаление абзаца, в которой представлены ссылки на эти инструкции.</span><span class="sxs-lookup"><span data-stu-id="2404a-121">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="2404a-122">Запустите приложение и убедитесь, что вы можете просканировать QR-код и проверку кода, который подтверждает средством проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="2404a-122">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="2404a-123">Изменение имени узла в QR-код</span><span class="sxs-lookup"><span data-stu-id="2404a-123">Change the site name in the QR Code</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="2404a-124">Имя сайта в QR-код берется из имени проекта, выбранный при первоначальном создании проекта.</span><span class="sxs-lookup"><span data-stu-id="2404a-124">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="2404a-125">Его можно изменить путем поиска `GenerateQrCodeUri(string email, string unformattedKey)` метод в */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2404a-125">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="2404a-126">Имя сайта в QR-код берется из имени проекта, выбранный при первоначальном создании проекта.</span><span class="sxs-lookup"><span data-stu-id="2404a-126">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="2404a-127">Его можно изменить путем поиска `GenerateQrCodeUri(string email, string unformattedKey)` метод в *Pages/Account/Manage/EnableAuthenticator.cshtml.cs* (Razor Pages) файла или *Controllers/ManageController.cs* файл (MVC).</span><span class="sxs-lookup"><span data-stu-id="2404a-127">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the *Pages/Account/Manage/EnableAuthenticator.cshtml.cs* (Razor Pages) file or the *Controllers/ManageController.cs* (MVC) file.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2404a-128">Код по умолчанию из шаблона выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="2404a-128">The default code from the template looks as follows:</span></span>

```csharp
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenicatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

<span data-ttu-id="2404a-129">Второй параметр в вызове `string.Format` — это имя узла, взятое из имя решения.</span><span class="sxs-lookup"><span data-stu-id="2404a-129">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="2404a-130">Его можно изменить в любое значение, но он всегда должен быть в кодировке URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="2404a-130">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="2404a-131">С помощью другой библиотеке QR-кода</span><span class="sxs-lookup"><span data-stu-id="2404a-131">Using a different QR Code library</span></span>

<span data-ttu-id="2404a-132">Библиотека QR-код можно заменить основной библиотеки.</span><span class="sxs-lookup"><span data-stu-id="2404a-132">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="2404a-133">HTML-код содержит `qrCode` библиотека предоставляет элемент, в который можно поместить QR-кода, какой бы механизм.</span><span class="sxs-lookup"><span data-stu-id="2404a-133">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="2404a-134">Правильно отформатированный URL-адрес для QR-код доступен в:</span><span class="sxs-lookup"><span data-stu-id="2404a-134">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="2404a-135">`AuthenticatorUri` Свойства модели.</span><span class="sxs-lookup"><span data-stu-id="2404a-135">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="2404a-136">`data-url` свойство в `qrCodeData` элемент.</span><span class="sxs-lookup"><span data-stu-id="2404a-136">`data-url` property in the `qrCodeData` element.</span></span>

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="2404a-137">Неравномерное распределение по времени клиента и сервера TOTP</span><span class="sxs-lookup"><span data-stu-id="2404a-137">TOTP client and server time skew</span></span>

<span data-ttu-id="2404a-138">Проверка подлинности TOTP (по времени одноразовый пароль) зависит от сервера и проверки подлинности устройства, наличие точного времени.</span><span class="sxs-lookup"><span data-stu-id="2404a-138">TOTP (Time-based One-Time Password) authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="2404a-139">Маркеры только продолжаться в течение 30 секунд.</span><span class="sxs-lookup"><span data-stu-id="2404a-139">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="2404a-140">Если имена входа 2FA TOTP не выполняются, проверьте время сервера точной и предпочтительно синхронизируется с точным службу NTP.</span><span class="sxs-lookup"><span data-stu-id="2404a-140">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>

::: moniker-end
