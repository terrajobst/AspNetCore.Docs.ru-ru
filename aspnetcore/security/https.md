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
ms.openlocfilehash: e06f4194d496b5b11aa867e66563bec317e735ff
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="setting-up-https-for-development-in-aspnet-core"></a>Настройка HTTPS для разработчиков на ASP.NET Core

> [!NOTE] 
> Этот раздел относится к ASP.NET Core 2.0 Предварительная версия 1

Можно настроить приложение для использования протокола HTTPS во время разработки для имитации HTTPS в рабочей среде. Включение HTTPS может потребоваться включить интеграцию с различными поставщиками удостоверений (например [Azure AD](https://azure.microsoft.com/services/active-directory) и [Azure AD B2C](https://azure.microsoft.com/services/active-directory-b2c)).

<a name="iisxpress"></a>

В Windows, если после установки Visual Studio или IIS Express, IIS Express разработки сертификат в хранилище сертификатов LocalMachine. Можно обновить свойства проекта в Visual Studio для использования этого сертификата при выполнении за IIS Express.

   * В обозревателе решений щелкните правой кнопкой мыши проект и выберите **свойства**.
   * На левой панели выберите **отладки**.
   * Проверьте **протокол SSL включен**
   * Скопируйте URL-адрес SSL и вставьте его в **URL-адрес приложения**

![Отладка вкладка Свойства веб-приложения](enforcing-ssl/_static/ssl.png)

Для разработки можно использовать сертификат IIS Express разработки, если он доступен или создать новый сертификат для целей разработки. Сертификат разработки должна быть настроена в `appsettings.Development.json` так, чтобы он не используется в рабочей среде:

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

В этой конфигурации, в работе приложения вызовет исключение, указывающее «Нет сертификата с именем «HTTPS», найдено в конфигурации для текущей среды (производство)». Для переключения [среды](xref:fundamentals/environments) для `Development`, задайте `ASPNETCORE_ENVIRONMENT` переменную среды, чтобы `Development`.

Если у вас IIS Express разработки установки сертификата, можно создать разработки сертификат самостоятельно. В Windows можно создать сертификат разработки и добавить его в доверенное корневое хранилище для текущего пользователя, выполнив следующие команды PowerShell в командной строке с повышенными привилегиями:

```powershell
$cert = New-SelfSignedCertificate -Subject localhost -DnsName localhost -FriendlyName "ASP.NET Core Development" -KeyUsage DigitalSignature -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1") 
Export-Certificate -Cert $cert -FilePath cert.cer
Import-Certificate -FilePath cert.cer -CertStoreLocation cert:/CurrentUser/Root
```

<a name="OpenSSL"></a>

## <a name="kestrel-on--macos-and-linux"></a>Kestrel на macOS и Linux

Вы можете настроить Kestrel для прослушивания по протоколу HTTPS, настроив нужный адрес IP-адрес, порт и сертификат конечной точки. Сертификат может быть настроенного встроенного или на верхнем уровне `Certificates` раздела, а затем ссылаться по имени:

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

MacOS и Linux можно создать самозаверяющий сертификат для использования HTTPS [OpenSSL](https://www.openssl.org/):

```bash
openssl req -new -x509 -newkey rsa:2048 -keyout localhost.key -out localhost.cer -days 365 -subj /CN=localhost
openssl pkcs12 -export -out certificate.pfx -inkey localhost.key -in localhost.cer
```

Один раз `certificate.pfx` создан файл, настроить сертификат HTTPS в вашей `appsettings.Development.json` файла:

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

Также необходимо будет указать парольную фразу для сертификата, задав свойство конфигурации «Сертификатов: HTTPS:Password». Пароли не должны храниться в виде обычного текста. В разделе [безопасного хранения из секретные данные во время разработки приложений](app-secrets.md) для соответствующую обработку сертификат парольную фразу.

Можно macOS [добавить сертификат в цепочке ключей](https://support.apple.com/kb/PH20129?locale=en_US) и [изменить его параметры доверия](https://support.apple.com/kb/PH20127?locale=en_US&viewlocale=en_US) , чтобы он стал доверенным для протокола HTTPS во время разработки. Чтобы добавить сертификат в цепочке ключей (эквивалент `CurrentUser/My` хранения в Windows) выполните следующую команду:

```bash
security import certificate.pfx -k ~/Library/Keychains/login.keychain-db
```

Затем доверие к сертификату:

```bash
security add-trusted-cert localhost.cer
```

Затем можно настроить приложение для использования этого сертификата в разработке следующим образом:

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
