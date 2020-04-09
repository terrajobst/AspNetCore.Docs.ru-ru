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
# <a name="hosting-aspnet-core-images-with-docker-compose-over-https"></a><span data-ttu-id="cb905-103">Хостинг ASP.NET основных изображений с Docker Составить над HTTPS</span><span class="sxs-lookup"><span data-stu-id="cb905-103">Hosting ASP.NET Core images with Docker Compose over HTTPS</span></span>


<span data-ttu-id="cb905-104">ASP.NET Core использует [HTTPS по умолчанию.](/aspnet/core/security/enforcing-ssl)</span><span class="sxs-lookup"><span data-stu-id="cb905-104">ASP.NET Core uses [HTTPS by default](/aspnet/core/security/enforcing-ssl).</span></span> <span data-ttu-id="cb905-105">[HTTPS](https://en.wikipedia.org/wiki/HTTPS) полагается на [сертификаты](https://en.wikipedia.org/wiki/Public_key_certificate) доверия, идентификации и шифрования.</span><span class="sxs-lookup"><span data-stu-id="cb905-105">[HTTPS](https://en.wikipedia.org/wiki/HTTPS) relies on [certificates](https://en.wikipedia.org/wiki/Public_key_certificate) for trust, identity, and encryption.</span></span>

<span data-ttu-id="cb905-106">В этом документе объясняется, как запускать предварительно построенные изображения контейнеров с помощью HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cb905-106">This document explains how to run pre-built container images with HTTPS.</span></span>

<span data-ttu-id="cb905-107">Для сценариев разработки сценариев разработки событий можно ознакомиться с [ASP.NET основных приложений с Docker по сценариям разработки.](https://github.com/dotnet/dotnet-docker/blob/master/samples/run-aspnetcore-https-development.md)</span><span class="sxs-lookup"><span data-stu-id="cb905-107">See [Developing ASP.NET Core Applications with Docker over HTTPS](https://github.com/dotnet/dotnet-docker/blob/master/samples/run-aspnetcore-https-development.md) for development scenarios.</span></span>

<span data-ttu-id="cb905-108">Этот образец требует [Докер 17,06](https://docs.docker.com/release-notes/docker-ce) или более поздний [клиент Docker](https://www.docker.com/products/docker).</span><span class="sxs-lookup"><span data-stu-id="cb905-108">This sample requires [Docker 17.06](https://docs.docker.com/release-notes/docker-ce) or later of the [Docker client](https://www.docker.com/products/docker).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cb905-109">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="cb905-109">Prerequisites</span></span>

<span data-ttu-id="cb905-110">Для некоторых инструкций в этом документе требуется [sDK .NET Core 2.2](https://dotnet.microsoft.com/download) или позже.</span><span class="sxs-lookup"><span data-stu-id="cb905-110">The [.NET Core 2.2 SDK](https://dotnet.microsoft.com/download) or later is required for some of the instructions in this document.</span></span>

## <a name="certificates"></a><span data-ttu-id="cb905-111">Сертификаты</span><span class="sxs-lookup"><span data-stu-id="cb905-111">Certificates</span></span>

<span data-ttu-id="cb905-112">Сертификат от [сертификата требуется](https://wikipedia.org/wiki/Certificate_authority) для [производства хостинга](https://blogs.msdn.microsoft.com/webdev/2017/11/29/configuring-https-in-asp-net-core-across-different-platforms/) для домена.</span><span class="sxs-lookup"><span data-stu-id="cb905-112">A certificate from a [certificate authority](https://wikipedia.org/wiki/Certificate_authority) is required for [production hosting](https://blogs.msdn.microsoft.com/webdev/2017/11/29/configuring-https-in-asp-net-core-across-different-platforms/) for a domain.</span></span> <span data-ttu-id="cb905-113">[Let's Encrypt](https://letsencrypt.org/)является сертификат оморганом, который предлагает бесплатные сертификаты.</span><span class="sxs-lookup"><span data-stu-id="cb905-113">[Let's Encrypt](https://letsencrypt.org/) is a certificate authority that offers free certificates.</span></span>

<span data-ttu-id="cb905-114">В этом документе используются [самоподписанные сертификаты разработки](https://wikipedia.org/wiki/Self-signed_certificate) для размещения заранее построенных изображений. `localhost`</span><span class="sxs-lookup"><span data-stu-id="cb905-114">This document uses [self-signed development certificates](https://wikipedia.org/wiki/Self-signed_certificate) for hosting pre-built images over `localhost`.</span></span> <span data-ttu-id="cb905-115">Инструкции аналогичны использованию производственных сертификатов.</span><span class="sxs-lookup"><span data-stu-id="cb905-115">The instructions are similar to using production certificates.</span></span>

<span data-ttu-id="cb905-116">Для производственных сертификатов:</span><span class="sxs-lookup"><span data-stu-id="cb905-116">For production certificates:</span></span>

* <span data-ttu-id="cb905-117">Инструмент `dotnet dev-certs` не требуется.</span><span class="sxs-lookup"><span data-stu-id="cb905-117">The `dotnet dev-certs` tool is not required.</span></span>
* <span data-ttu-id="cb905-118">Сертификаты не должны храниться в месте, используемом в инструкциях.</span><span class="sxs-lookup"><span data-stu-id="cb905-118">Certificates don't need to be stored in the location used in the instructions.</span></span> <span data-ttu-id="cb905-119">Храните сертификаты в любом месте за пределами каталога сайта.</span><span class="sxs-lookup"><span data-stu-id="cb905-119">Store the certificates in any location outside the site directory.</span></span>

<span data-ttu-id="cb905-120">Инструкции, содержащиеся в следующем разделе объем крепления `volumes` сертификатов в контейнеры с использованием имущества в *docker-compose.yml.*</span><span class="sxs-lookup"><span data-stu-id="cb905-120">The instructions contained in the following section volume mount certificates into containers using the `volumes` property in *docker-compose.yml.*</span></span> <span data-ttu-id="cb905-121">Можно добавить сертификаты в `COPY` изображения контейнеров с командой в *Dockerfile,* но это не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="cb905-121">You could add certificates into container images with a `COPY` command in a *Dockerfile*, but it's not recommended.</span></span> <span data-ttu-id="cb905-122">Копирование сертификатов в изображение не рекомендуется по следующим причинам:</span><span class="sxs-lookup"><span data-stu-id="cb905-122">Copying certificates into an image isn't recommended for the following reasons:</span></span>

* <span data-ttu-id="cb905-123">Это затрудняет использование одного и того же изображения для тестирования с помощью сертификатов разработчика.</span><span class="sxs-lookup"><span data-stu-id="cb905-123">It makes it difficult to use the same image for testing with developer certificates.</span></span>
* <span data-ttu-id="cb905-124">Это затрудняет использование одного и того же изображения для хостинга с производственными сертификатами.</span><span class="sxs-lookup"><span data-stu-id="cb905-124">It makes it difficult to use the same image for Hosting with production certificates.</span></span>
* <span data-ttu-id="cb905-125">Существует значительный риск раскрытия сертификата.</span><span class="sxs-lookup"><span data-stu-id="cb905-125">There is significant risk of certificate disclosure.</span></span>

## <a name="starting-a-container-with-https-support-using-docker-compose"></a><span data-ttu-id="cb905-126">Запуск контейнера с поддержкой https с помощью докера</span><span class="sxs-lookup"><span data-stu-id="cb905-126">Starting a container with https support using docker compose</span></span>

<span data-ttu-id="cb905-127">Используйте следующие инструкции для конфигурации операционной системы.</span><span class="sxs-lookup"><span data-stu-id="cb905-127">Use the following instructions for your operating system configuration.</span></span>

### <a name="windows-using-linux-containers"></a><span data-ttu-id="cb905-128">Windows с использованием linux контейнеров</span><span class="sxs-lookup"><span data-stu-id="cb905-128">Windows using Linux containers</span></span>

<span data-ttu-id="cb905-129">Создание сертификата и настройка локальной машины:</span><span class="sxs-lookup"><span data-stu-id="cb905-129">Generate certificate and configure local machine:</span></span>

```dotnetcli
dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

<span data-ttu-id="cb905-130">В предыдущих командах `{ password here }` замените пароль.</span><span class="sxs-lookup"><span data-stu-id="cb905-130">In the preceding commands, replace `{ password here }` with a password.</span></span>

<span data-ttu-id="cb905-131">Создайте файл _docker-compose.debug.yml_ со следующим содержанием:</span><span class="sxs-lookup"><span data-stu-id="cb905-131">Create a _docker-compose.debug.yml_ file with the following content:</span></span>

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
<span data-ttu-id="cb905-132">Пароль, указанный в файле сочинения докера, должен соответствовать паролю, используемому для сертификата.</span><span class="sxs-lookup"><span data-stu-id="cb905-132">The password specified in the docker compose file must match the password used for the certificate.</span></span>

<span data-ttu-id="cb905-133">Запустите контейнер с ASP.NET Core, настроенном для HTTPS:</span><span class="sxs-lookup"><span data-stu-id="cb905-133">Start the container with ASP.NET Core configured for HTTPS:</span></span>

```console
docker-compose -f "docker-compose.debug.yml" up -d
```

### <a name="macos-or-linux"></a><span data-ttu-id="cb905-134">macOS или Linux</span><span class="sxs-lookup"><span data-stu-id="cb905-134">macOS or Linux</span></span>

<span data-ttu-id="cb905-135">Создание сертификата и настройка локальной машины:</span><span class="sxs-lookup"><span data-stu-id="cb905-135">Generate certificate and configure local machine:</span></span>

```dotnetcli
dotnet dev-certs https -ep ${HOME}/.aspnet/https/aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

<span data-ttu-id="cb905-136">`dotnet dev-certs https --trust`поддерживается только на macOS и Windows.</span><span class="sxs-lookup"><span data-stu-id="cb905-136">`dotnet dev-certs https --trust` is only supported on macOS and Windows.</span></span> <span data-ttu-id="cb905-137">Вы должны доверять сертификатам на Linux таким образом, чтобы это поддерживалось вашим дистро.</span><span class="sxs-lookup"><span data-stu-id="cb905-137">You need to trust certificates on Linux in the way that is supported by your distro.</span></span> <span data-ttu-id="cb905-138">Вполне вероятно, что вам нужно доверять сертификат в вашем браузере.</span><span class="sxs-lookup"><span data-stu-id="cb905-138">It is likely that you need to trust the certificate in your browser.</span></span>

<span data-ttu-id="cb905-139">В предыдущих командах `{ password here }` замените пароль.</span><span class="sxs-lookup"><span data-stu-id="cb905-139">In the preceding commands, replace `{ password here }` with a password.</span></span>

<span data-ttu-id="cb905-140">Создайте файл _docker-compose.debug.yml_ со следующим содержанием:</span><span class="sxs-lookup"><span data-stu-id="cb905-140">Create a _docker-compose.debug.yml_ file with the following content:</span></span>

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
<span data-ttu-id="cb905-141">Пароль, указанный в файле сочинения докера, должен соответствовать паролю, используемому для сертификата.</span><span class="sxs-lookup"><span data-stu-id="cb905-141">The password specified in the docker compose file must match the password used for the certificate.</span></span>

<span data-ttu-id="cb905-142">Запустите контейнер с ASP.NET Core, настроенном для HTTPS:</span><span class="sxs-lookup"><span data-stu-id="cb905-142">Start the container with ASP.NET Core configured for HTTPS:</span></span>

```console
docker-compose -f "docker-compose.debug.yml" up -d
```

### <a name="windows-using-windows-containers"></a><span data-ttu-id="cb905-143">Windows с использованием контейнеров Windows</span><span class="sxs-lookup"><span data-stu-id="cb905-143">Windows using Windows containers</span></span>

<span data-ttu-id="cb905-144">Создание сертификата и настройка локальной машины:</span><span class="sxs-lookup"><span data-stu-id="cb905-144">Generate certificate and configure local machine:</span></span>

```dotnetcli
dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

<span data-ttu-id="cb905-145">В предыдущих командах `{ password here }` замените пароль.</span><span class="sxs-lookup"><span data-stu-id="cb905-145">In the preceding commands, replace `{ password here }` with a password.</span></span>

<span data-ttu-id="cb905-146">Создайте файл _docker-compose.debug.yml_ со следующим содержанием:</span><span class="sxs-lookup"><span data-stu-id="cb905-146">Create a _docker-compose.debug.yml_ file with the following content:</span></span>

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
<span data-ttu-id="cb905-147">Пароль, указанный в файле сочинения докера, должен соответствовать паролю, используемому для сертификата.</span><span class="sxs-lookup"><span data-stu-id="cb905-147">The password specified in the docker compose file must match the password used for the certificate.</span></span>

<span data-ttu-id="cb905-148">Запустите контейнер с ASP.NET Core, настроенном для HTTPS:</span><span class="sxs-lookup"><span data-stu-id="cb905-148">Start the container with ASP.NET Core configured for HTTPS:</span></span>

```console
docker-compose -f "docker-compose.debug.yml" up -d
```
