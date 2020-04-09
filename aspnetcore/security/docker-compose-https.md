---
title: Хостинг ASP.NET основное изображение в контейнере с помощью докера, сочиняемого с помощью HTTPS
author: ravipal
description: Узнайте, как размещать ASP.NET основные изображения с Docker Составьте над HTTPS
monikerRange: '>= aspnetcore-2.1'
ms.author: ravipal
ms.custom: mvc
ms.date: 03/28/2020
no-loc:
- Let's Encrypt
uid: security/docker-compose-https
ms.openlocfilehash: 616ccf906e98534ffda08c0c2b6d0a171f063cc1
ms.sourcegitcommit: d03905aadf5ceac39fff17706481af7f6c130411
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2020
ms.locfileid: "80381812"
---
# <a name="hosting-aspnet-core-images-with-docker-compose-over-https"></a>Хостинг ASP.NET основных изображений с Docker Составить над HTTPS


ASP.NET Core использует [HTTPS по умолчанию.](/aspnet/core/security/enforcing-ssl) [HTTPS](https://en.wikipedia.org/wiki/HTTPS) полагается на [сертификаты](https://en.wikipedia.org/wiki/Public_key_certificate) доверия, идентификации и шифрования.

В этом документе объясняется, как запускать предварительно построенные изображения контейнеров с помощью HTTPS.

Для сценариев разработки сценариев разработки событий можно ознакомиться с [ASP.NET основных приложений с Docker по сценариям разработки.](https://github.com/dotnet/dotnet-docker/blob/master/samples/run-aspnetcore-https-development.md)

Этот образец требует [Докер 17,06](https://docs.docker.com/release-notes/docker-ce) или более поздний [клиент Docker](https://www.docker.com/products/docker).

## <a name="prerequisites"></a>Предварительные требования

Для некоторых инструкций в этом документе требуется [sDK .NET Core 2.2](https://dotnet.microsoft.com/download) или позже.

## <a name="certificates"></a>Сертификаты

Сертификат от [сертификата требуется](https://wikipedia.org/wiki/Certificate_authority) для [производства хостинга](https://blogs.msdn.microsoft.com/webdev/2017/11/29/configuring-https-in-asp-net-core-across-different-platforms/) для домена. [Let's Encrypt](https://letsencrypt.org/)является сертификат оморганом, который предлагает бесплатные сертификаты.

В этом документе используются [самоподписанные сертификаты разработки](https://wikipedia.org/wiki/Self-signed_certificate) для размещения заранее построенных изображений. `localhost` Инструкции аналогичны использованию производственных сертификатов.

Для производственных сертификатов:

* Инструмент `dotnet dev-certs` не требуется.
* Сертификаты не должны храниться в месте, используемом в инструкциях. Храните сертификаты в любом месте за пределами каталога сайта.

Инструкции, содержащиеся в следующем разделе объем крепления `volumes` сертификатов в контейнеры с использованием имущества в *docker-compose.yml.* Можно добавить сертификаты в `COPY` изображения контейнеров с командой в *Dockerfile,* но это не рекомендуется. Копирование сертификатов в изображение не рекомендуется по следующим причинам:

* Это затрудняет использование одного и того же изображения для тестирования с помощью сертификатов разработчика.
* Это затрудняет использование одного и того же изображения для хостинга с производственными сертификатами.
* Существует значительный риск раскрытия сертификата.

## <a name="starting-a-container-with-https-support-using-docker-compose"></a>Запуск контейнера с поддержкой https с помощью докера

Используйте следующие инструкции для конфигурации операционной системы.

### <a name="windows-using-linux-containers"></a>Windows с использованием linux контейнеров

Создание сертификата и настройка локальной машины:

```dotnetcli
dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

В предыдущих командах `{ password here }` замените пароль.

Создайте файл _docker-compose.debug.yml_ со следующим содержанием:

```json
version: '3.4'

services:
  webapp:
    image: mcr.microsoft.com/dotnet/core/samples:aspnetapp
    ports:
      - 80
      - 443
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=https://+:443;http://+:80
      - ASPNETCORE_Kestrel__Certificates__Default__Password=password
      - ASPNETCORE_Kestrel__Certificates__Default__Path=/https/aspnetapp.pfx
    volumes:
      - ~/.aspnet/https:/https:ro
```
Пароль, указанный в файле сочинения докера, должен соответствовать паролю, используемому для сертификата.

Запустите контейнер с ASP.NET Core, настроенном для HTTPS:

```console
docker-compose -f "docker-compose.debug.yml" up -d
```

### <a name="macos-or-linux"></a>macOS или Linux

Создание сертификата и настройка локальной машины:

```dotnetcli
dotnet dev-certs https -ep ${HOME}/.aspnet/https/aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

`dotnet dev-certs https --trust`поддерживается только на macOS и Windows. Вы должны доверять сертификатам на Linux таким образом, чтобы это поддерживалось вашим дистро. Вполне вероятно, что вам нужно доверять сертификат в вашем браузере.

В предыдущих командах `{ password here }` замените пароль.

Создайте файл _docker-compose.debug.yml_ со следующим содержанием:

```json
version: '3.4'

services:
  webapp:
    image: mcr.microsoft.com/dotnet/core/samples:aspnetapp
    ports:
      - 80
      - 443
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=https://+:443;http://+:80
      - ASPNETCORE_Kestrel__Certificates__Default__Password=password
      - ASPNETCORE_Kestrel__Certificates__Default__Path=/https/aspnetapp.pfx
    volumes:
      - ~/.aspnet/https:/https:ro
```
Пароль, указанный в файле сочинения докера, должен соответствовать паролю, используемому для сертификата.

Запустите контейнер с ASP.NET Core, настроенном для HTTPS:

```console
docker-compose -f "docker-compose.debug.yml" up -d
```

### <a name="windows-using-windows-containers"></a>Windows с использованием контейнеров Windows

Создание сертификата и настройка локальной машины:

```dotnetcli
dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

В предыдущих командах `{ password here }` замените пароль.

Создайте файл _docker-compose.debug.yml_ со следующим содержанием:

```json
version: '3.4'

services:
  webapp:
    image: mcr.microsoft.com/dotnet/core/samples:aspnetapp
    ports:
      - 80
      - 443
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=https://+:443;http://+:80
      - ASPNETCORE_Kestrel__Certificates__Default__Password=password
      - ASPNETCORE_Kestrel__Certificates__Default__Path=C:\https\aspnetapp.pfx
    volumes:
      - ${USERPROFILE}\.aspnet\https:C:\https:ro
```
Пароль, указанный в файле сочинения докера, должен соответствовать паролю, используемому для сертификата.

Запустите контейнер с ASP.NET Core, настроенном для HTTPS:

```console
docker-compose -f "docker-compose.debug.yml" up -d
```
