---
title: "Включение создания QR-код для приложения для проверки подлинности в ASP.NET Core"
author: rick-anderson
description: "Включение создания QR-код для приложения для проверки подлинности в ASP.NET Core"
keywords: "Ядро ASP.NET, MVC, создание QR-кода, средства проверки подлинности, 2FA"
ms.author: riande
manager: wpickett
ms.date: 07/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: fcadf9ca0ad66bb0fd56efc248fc7534965b48b3
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/28/2017
---
# <a name="enabling-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="b7678-104">Включение создания QR-код для приложения для проверки подлинности в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b7678-104">Enabling QR Code generation for authenticator apps in ASP.NET Core</span></span>

<span data-ttu-id="b7678-105">Примечание: Этот раздел относится к ASP.NET Core 2.x со страницами Razor.</span><span class="sxs-lookup"><span data-stu-id="b7678-105">Note: This topic applies to ASP.NET Core 2.x with Razor Pages.</span></span>

<span data-ttu-id="b7678-106">ASP.NET Core поставляется с поддержки проверки подлинности приложений для отдельных проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="b7678-106">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="b7678-107">Два фактора проверки подлинности (2FA) приложения для проверки подлинности, с помощью синхронизированного одноразовый пароль алгоритм (TOTP), являются рекомендуется approch для 2FA отрасли.</span><span class="sxs-lookup"><span data-stu-id="b7678-107">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approch for 2FA.</span></span> <span data-ttu-id="b7678-108">2FA с помощью TOTP предпочтительнее SMS 2FA.</span><span class="sxs-lookup"><span data-stu-id="b7678-108">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="b7678-109">Приложение проверки подлинности предоставляет 6 – 8 цифр, какие пользователи должны ввести после подтверждения свое имя пользователя и пароль.</span><span class="sxs-lookup"><span data-stu-id="b7678-109">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="b7678-110">Обычно приложение проверки подлинности устанавливается на смартфоне.</span><span class="sxs-lookup"><span data-stu-id="b7678-110">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="b7678-111">Веб-приложения ASP.NET Core шаблонов поддерживает атрибуты проверки подлинности, но не обеспечивают поддержку создания QRCode.</span><span class="sxs-lookup"><span data-stu-id="b7678-111">The ASP.NET Core web app templates support authenticators, but do not provide support for QRCode generation.</span></span> <span data-ttu-id="b7678-112">Генераторы QRCode простота установки 2FA.</span><span class="sxs-lookup"><span data-stu-id="b7678-112">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="b7678-113">В этом документе подробно рассматривается добавление [QR-код](https://wikipedia.org/wiki/QR_code) поколения на страницу настройки 2FA.</span><span class="sxs-lookup"><span data-stu-id="b7678-113">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="b7678-114">Добавление на страницу настройки 2FA QR-коды</span><span class="sxs-lookup"><span data-stu-id="b7678-114">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="b7678-115">В этих инструкциях используется *qrcode.js* из репозитория https://davidshimjs.github.io/qrcodejs/.</span><span class="sxs-lookup"><span data-stu-id="b7678-115">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="b7678-116">Загрузить [библиотека javascript qrcode.js](https://davidshimjs.github.io/qrcodejs/) для `wwwroot\lib` в папке проекта.</span><span class="sxs-lookup"><span data-stu-id="b7678-116">Download the  [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

* <span data-ttu-id="b7678-117">В *Pages\Account\Manage\EnableAuthenticator.cshtml* файла, найдите `Scripts` в конце файла:</span><span class="sxs-lookup"><span data-stu-id="b7678-117">In the *Pages\Account\Manage\EnableAuthenticator.cshtml* file, locate the `Scripts` section at the end of the file:</span></span>

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="b7678-118">Обновление `Scripts` раздел, чтобы добавить ссылку на `qrcodejs` библиотеки, добавленные и вызова для создания QR-код.</span><span class="sxs-lookup"><span data-stu-id="b7678-118">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="b7678-119">Он должен выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="b7678-119">It should look as follows:</span></span>

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

* <span data-ttu-id="b7678-120">Удаление абзаца, в которой представлены ссылки на эти инструкции.</span><span class="sxs-lookup"><span data-stu-id="b7678-120">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="b7678-121">Запустите приложение и убедитесь, что сканирование QR-кода и проверить код, который подтверждает средством проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="b7678-121">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="b7678-122">Изменение имени узла в QR-кода</span><span class="sxs-lookup"><span data-stu-id="b7678-122">Change the site name in the QR Code</span></span>

<span data-ttu-id="b7678-123">Имя узла в QR-кода берется из имени проекта, выбранный при первоначальном создании проекта.</span><span class="sxs-lookup"><span data-stu-id="b7678-123">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="b7678-124">Его можно изменить путем поиска `GenerateQrCodeUri(string email, string unformattedKey)` метод в *EnableAuthenticator.cshtml.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="b7678-124">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in  the *EnableAuthenticator.cshtml.cs* file.</span></span> 

<span data-ttu-id="b7678-125">Код по умолчанию на основе шаблона выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="b7678-125">The default code from the template looks as follows:</span></span>

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

<span data-ttu-id="b7678-126">Второй параметр в вызове `string.Format` — это имя узла, взяты из имя вашего решения.</span><span class="sxs-lookup"><span data-stu-id="b7678-126">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="b7678-127">Его можно изменить в любое значение, но он всегда должен быть в кодировке URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="b7678-127">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="b7678-128">С помощью другой библиотекой QR-кода</span><span class="sxs-lookup"><span data-stu-id="b7678-128">Using a different QR Code library</span></span>

<span data-ttu-id="b7678-129">Библиотека QR-кода можно заменить предпочтительные библиотеки.</span><span class="sxs-lookup"><span data-stu-id="b7678-129">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="b7678-130">В HTML `qrCode` библиотека предоставляет элемент, в который можно поместить QR-кода с любой механизм.</span><span class="sxs-lookup"><span data-stu-id="b7678-130">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="b7678-131">Правильно отформатированный URL-адрес для QR-код доступен в:</span><span class="sxs-lookup"><span data-stu-id="b7678-131">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="b7678-132">`AuthenticatorUri`Свойства модели.</span><span class="sxs-lookup"><span data-stu-id="b7678-132">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="b7678-133">`data-url`свойство в `qrCodeData` элемент.</span><span class="sxs-lookup"><span data-stu-id="b7678-133">`data-url` property in the `qrCodeData` element.</span></span> 

<span data-ttu-id="b7678-134">Используйте `@Html.Raw` для доступа к свойству модели в представлении (в противном случае двойным шифрованием амперсанды в URL-адресе и будет пропущен параметр метки QR-код).</span><span class="sxs-lookup"><span data-stu-id="b7678-134">Use `@Html.Raw` to access the model property in a view (otherwise the ampersands in the url will be double encoded and the label parameter of the QR Code will be ignored).</span></span>
