---
title: "Настройка HTTPS для разработчиков на ASP.NET Core"
author: Rick-Anderson
description: "Показано, как настроить поддержку протокола HTTPS для разработки приложений в ASP.NET Core 2.0."
keywords: ASP.NET Core, SSL, HTTPS
ms.author: riande
manager: wpickett
ms.date: 05/10/2017
ms.topic: article
ms.assetid: 94f2f1a4-7d46-45e2-a085-a57916e41724
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/https
ms.openlocfilehash: 7913758ffa045dcf884d73eed9bab223b8d2c3fe
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/22/2017
---
# <a name="setting-up-https-for-development-in-aspnet-core"></a><span data-ttu-id="0a329-104">Настройка HTTPS для разработчиков на ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0a329-104">Setting up HTTPS for development in ASP.NET Core</span></span>

> [!NOTE] 
> <span data-ttu-id="0a329-105">Этот раздел относится к ASP.NET Core 2.0 Предварительная версия 1</span><span class="sxs-lookup"><span data-stu-id="0a329-105">This topic applies to ASP.NET Core 2.0 Preview 1</span></span>

<span data-ttu-id="0a329-106">Можно настроить приложение для использования протокола HTTPS во время разработки для имитации HTTPS в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="0a329-106">You can configure your application to use HTTPS during development to simulate HTTPS in your production environment.</span></span> <span data-ttu-id="0a329-107">Включение HTTPS может потребоваться включить интеграцию с различными поставщиками удостоверений (например [Azure AD](https://azure.microsoft.com/services/active-directory) и [Azure AD B2C](https://azure.microsoft.com/services/active-directory-b2c/)).</span><span class="sxs-lookup"><span data-stu-id="0a329-107">Enabling HTTPS may be required to enable integration with various identity providers (like [Azure AD](https://azure.microsoft.com/services/active-directory) and [Azure AD B2C](https://azure.microsoft.com/services/active-directory-b2c/)).</span></span>

<a name="iisxpress"></a>

<span data-ttu-id="0a329-108">В Windows, если после установки Visual Studio или IIS Express, IIS Express разработки сертификат в хранилище сертификатов LocalMachine.</span><span class="sxs-lookup"><span data-stu-id="0a329-108">On Windows if you’ve installed Visual Studio or IIS Express, the IIS Express Development Certificate will be in your LocalMachine certificate store.</span></span> <span data-ttu-id="0a329-109">Можно обновить свойства проекта в Visual Studio для использования этого сертификата при выполнении за IIS Express.</span><span class="sxs-lookup"><span data-stu-id="0a329-109">You can update your project properties in Visual Studio to use this certificate when running behind IIS Express.</span></span>

   * <span data-ttu-id="0a329-110">В обозревателе решений щелкните правой кнопкой мыши проект и выберите **свойства**.</span><span class="sxs-lookup"><span data-stu-id="0a329-110">In Solution Explorer, right-click the project and select **Properties**.</span></span>
   * <span data-ttu-id="0a329-111">На левой панели выберите **отладки**.</span><span class="sxs-lookup"><span data-stu-id="0a329-111">On the left pane, select **Debug**.</span></span>
   * <span data-ttu-id="0a329-112">Проверьте **протокол SSL включен**</span><span class="sxs-lookup"><span data-stu-id="0a329-112">Check **Enable SSL**</span></span>
   * <span data-ttu-id="0a329-113">Скопируйте URL-адрес SSL и вставьте его в **URL-адрес приложения**</span><span class="sxs-lookup"><span data-stu-id="0a329-113">Copy the SSL URL and paste it into the **App URL**</span></span>

![Отладка вкладка Свойства веб-приложения](enforcing-ssl/_static/ssl.png)

<span data-ttu-id="0a329-115">Для разработки можно использовать сертификат IIS Express разработки, если он доступен или создать новый сертификат для целей разработки.</span><span class="sxs-lookup"><span data-stu-id="0a329-115">For development you can use the IIS Express Development Certificate if it is available, or create a new certificate for development purposes.</span></span> <span data-ttu-id="0a329-116">Сертификат разработки должна быть настроена в `appsettings.Development.json` так, чтобы он не используется в рабочей среде:</span><span class="sxs-lookup"><span data-stu-id="0a329-116">The development certificate should be configured in the `appsettings.Development.json` file so that it is not used in production:</span></span>

```json
{
  "Certificates": {
    "HTTPS": {
      "Source": "Store",
      "StoreLocation": "LocalMachine",
      "StoreName": "My",
      "Subject": "CN=localhost",
      "AllowInvalid": true
    }
  }
}
```

<span data-ttu-id="0a329-117">В этой конфигурации, в работе приложения вызовет исключение, указывающее «Нет сертификата с именем «HTTPS», найдено в конфигурации для текущей среды (производство)».</span><span class="sxs-lookup"><span data-stu-id="0a329-117">An app with this configuration running in production will throw an exception saying "No certificate named 'HTTPS' found in configuration for the current environment (Production)".</span></span> <span data-ttu-id="0a329-118">Для переключения [среды](xref:fundamentals/environments) для `Development`, задайте `ASPNETCORE_ENVIRONMENT` переменную среды, чтобы `Development`.</span><span class="sxs-lookup"><span data-stu-id="0a329-118">To switch the [environment](xref:fundamentals/environments) to `Development`, set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development`.</span></span>

<span data-ttu-id="0a329-119">Если у вас IIS Express разработки установки сертификата, можно создать разработки сертификат самостоятельно.</span><span class="sxs-lookup"><span data-stu-id="0a329-119">If you do not have the IIS Express Development Certificate installed, you can create a development certificate yourself.</span></span> <span data-ttu-id="0a329-120">В Windows можно создать сертификат разработки и добавить его в доверенное корневое хранилище для текущего пользователя, выполнив следующие команды PowerShell в командной строке с повышенными привилегиями:</span><span class="sxs-lookup"><span data-stu-id="0a329-120">On Windows you can create a development certificate and add it to the trusted root store for the current user by running the following PowerShell commands in an elevated prompt:</span></span>

```powershell
$cert = New-SelfSignedCertificate -Subject localhost -DnsName localhost -FriendlyName "ASP.NET Core Development" -KeyUsage DigitalSignature -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1") 
Export-Certificate -Cert $cert -FilePath cert.cer
Import-Certificate -FilePath cert.cer -CertStoreLocation cert:/CurrentUser/Root
```

<a name="OpenSSL"></a>

## <a name="kestrel-on--macos-and-linux"></a><span data-ttu-id="0a329-121">Kestrel на macOS и Linux</span><span class="sxs-lookup"><span data-stu-id="0a329-121">Kestrel on  macOS and Linux</span></span>

<span data-ttu-id="0a329-122">Вы можете настроить Kestrel для прослушивания по протоколу HTTPS, настроив нужный адрес IP-адрес, порт и сертификат конечной точки.</span><span class="sxs-lookup"><span data-stu-id="0a329-122">You can  configure Kestrel to listen over HTTPS by configuring an endpoint with the desired IP address, port, and certificate.</span></span> <span data-ttu-id="0a329-123">Сертификат может быть настроенного встроенного или на верхнем уровне `Certificates` раздела, а затем ссылаться по имени:</span><span class="sxs-lookup"><span data-stu-id="0a329-123">The certificate can be configured inline, or in the top-level `Certificates` section and then referenced by name:</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "LocalhostHttps": {
        "Address": "127.0.0.1",
        "Port": "43434",
        "Certificate": "HTTPS"
      }
    }
  }
}

```

<span data-ttu-id="0a329-124">MacOS и Linux можно создать самозаверяющий сертификат для использования HTTPS [OpenSSL](https://www.openssl.org/):</span><span class="sxs-lookup"><span data-stu-id="0a329-124">On macOS and Linux you can create a self-signed certificate for HTTPS using [OpenSSL](https://www.openssl.org/):</span></span>

```bash
openssl req -new -x509 -newkey rsa:2048 -keyout localhost.key -out localhost.cer -days 365 -subj /CN=localhost
openssl pkcs12 -export -out certificate.pfx -inkey localhost.key -in localhost.cer
```

<span data-ttu-id="0a329-125">Один раз `certificate.pfx` создан файл, настроить сертификат HTTPS в вашей `appsettings.Development.json` файла:</span><span class="sxs-lookup"><span data-stu-id="0a329-125">Once the `certificate.pfx` file has been generated, configure the HTTPS certificate in your `appsettings.Development.json` file:</span></span>

```json
{
  "Certificates": {
    "HTTPS": {
      "Source": "File",
      "Path": "certificate.pfx"
    }
  }
}
```

<span data-ttu-id="0a329-126">Также необходимо будет указать парольную фразу для сертификата, задав свойство конфигурации «Сертификатов: HTTPS:Password».</span><span class="sxs-lookup"><span data-stu-id="0a329-126">You will also need to specify the passphrase for the certificate by setting the “Certificates:HTTPS:Password” config property.</span></span> <span data-ttu-id="0a329-127">Пароли не должны храниться в виде обычного текста.</span><span class="sxs-lookup"><span data-stu-id="0a329-127">Passwords should not be stored in plain text.</span></span> <span data-ttu-id="0a329-128">В разделе [безопасного хранения из секретные данные во время разработки приложений](app-secrets.md) для соответствующую обработку сертификат парольную фразу.</span><span class="sxs-lookup"><span data-stu-id="0a329-128">See [Safe Storage of App Secrets During Development](app-secrets.md) for appropriate handling of the certificate passphrase.</span></span>

<span data-ttu-id="0a329-129">Можно macOS [добавить сертификат в цепочке ключей](https://support.apple.com/kb/PH20129?locale=en_US) и [изменить его параметры доверия](https://support.apple.com/kb/PH20127?locale=en_US&viewlocale=en_US) , чтобы он стал доверенным для протокола HTTPS во время разработки.</span><span class="sxs-lookup"><span data-stu-id="0a329-129">On macOS you can [add the certificate to your keychain](https://support.apple.com/kb/PH20129?locale=en_US) and [change its trust settings](https://support.apple.com/kb/PH20127?locale=en_US&viewlocale=en_US) so that it is trusted for HTTPS during development.</span></span> <span data-ttu-id="0a329-130">Чтобы добавить сертификат в цепочке ключей (эквивалент `CurrentUser/My` хранения в Windows) выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="0a329-130">To add the certificate to your keychain (the equivalent of the `CurrentUser/My` store on Windows) run the following command:</span></span>

```bash
security import certificate.pfx -k ~/Library/Keychains/login.keychain-db
```

<span data-ttu-id="0a329-131">Затем доверие к сертификату:</span><span class="sxs-lookup"><span data-stu-id="0a329-131">And then to trust the certificate:</span></span>

```bash
security add-trusted-cert localhost.cer
```

<span data-ttu-id="0a329-132">Затем можно настроить приложение для использования этого сертификата в разработке следующим образом:</span><span class="sxs-lookup"><span data-stu-id="0a329-132">You can then configure your app to use this certificate in development like this:</span></span>

```json
{
  "Certificates": {
    "HTTPS": {
      "Source": "Store",
      "StoreLocation": "CurrentUser",
      "StoreName": "My",
      "Subject": "CN=localhost",
      "AllowInvalid": true
    }
  }
}
```
