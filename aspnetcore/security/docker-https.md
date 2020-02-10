---
title: Размещение образов ASP.NET Core с помощью DOCKER по протоколу HTTPS
author: rick-anderson
description: Сведения о размещении образов ASP.NET Core с помощью DOCKER по протоколу HTTPS
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/05/2019
no-loc:
- Let's Encrypt
uid: security/docker-https
ms.openlocfilehash: 2f338e8883ca926c0f9a7ab339f58b088151cc87
ms.sourcegitcommit: 235623b6e5a5d1841139c82a11ac2b4b3f31a7a9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/08/2020
ms.locfileid: "77089205"
---
# <a name="hosting-aspnet-core-images-with-docker-over-https"></a><span data-ttu-id="0dd5c-103">Размещение образов ASP.NET Core с помощью DOCKER по протоколу HTTPS</span><span class="sxs-lookup"><span data-stu-id="0dd5c-103">Hosting ASP.NET Core images with Docker over HTTPS</span></span>

<span data-ttu-id="0dd5c-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0dd5c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0dd5c-105">По [умолчанию ASP.NET Core использует протокол HTTPS](/aspnet/core/security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="0dd5c-105">ASP.NET Core uses [HTTPS by default](/aspnet/core/security/enforcing-ssl).</span></span> <span data-ttu-id="0dd5c-106">[Протокол HTTPS](https://en.wikipedia.org/wiki/HTTPS) использует [Сертификаты](https://en.wikipedia.org/wiki/Public_key_certificate) для доверия, удостоверений и шифрования.</span><span class="sxs-lookup"><span data-stu-id="0dd5c-106">[HTTPS](https://en.wikipedia.org/wiki/HTTPS) relies on [certificates](https://en.wikipedia.org/wiki/Public_key_certificate) for trust, identity, and encryption.</span></span>

<span data-ttu-id="0dd5c-107">В этом документе объясняется, как запускать предварительно созданные образы контейнеров с помощью протокола HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0dd5c-107">This document explains how to run pre-built container images with HTTPS.</span></span>

<span data-ttu-id="0dd5c-108">Сценарии разработки см. [в статье Разработка приложений ASP.NET Core с помощью DOCKER по протоколу HTTPS](https://github.com/dotnet/dotnet-docker/blob/master/samples/run-aspnetcore-https-development.md) .</span><span class="sxs-lookup"><span data-stu-id="0dd5c-108">See [Developing ASP.NET Core Applications with Docker over HTTPS](https://github.com/dotnet/dotnet-docker/blob/master/samples/run-aspnetcore-https-development.md) for development scenarios.</span></span>

<span data-ttu-id="0dd5c-109">Для этого примера требуется [docker 17,06](https://docs.docker.com/release-notes/docker-ce) или более поздней версии [клиента DOCKER](https://www.docker.com/products/docker).</span><span class="sxs-lookup"><span data-stu-id="0dd5c-109">This sample requires [Docker 17.06](https://docs.docker.com/release-notes/docker-ce) or later of the [Docker client](https://www.docker.com/products/docker).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0dd5c-110">предварительные требования</span><span class="sxs-lookup"><span data-stu-id="0dd5c-110">Prerequisites</span></span>

<span data-ttu-id="0dd5c-111">Для выполнения некоторых инструкций в этом документе требуется [пакет SDK для .NET Core 2,2](https://www.microsoft.com/net/download) или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="0dd5c-111">The [.NET Core 2.2 SDK](https://www.microsoft.com/net/download) or later is required for some of the instructions in this document.</span></span>

## <a name="certificates"></a><span data-ttu-id="0dd5c-112">Сертификаты</span><span class="sxs-lookup"><span data-stu-id="0dd5c-112">Certificates</span></span>

<span data-ttu-id="0dd5c-113">Сертификат из [центра](https://wikipedia.org/wiki/Certificate_authority) сертификации необходим для [размещения в рабочей среде](https://blogs.msdn.microsoft.com/webdev/2017/11/29/configuring-https-in-asp-net-core-across-different-platforms/) для домена.</span><span class="sxs-lookup"><span data-stu-id="0dd5c-113">A certificate from a [certificate authority](https://wikipedia.org/wiki/Certificate_authority) is required for [production hosting](https://blogs.msdn.microsoft.com/webdev/2017/11/29/configuring-https-in-asp-net-core-across-different-platforms/) for a domain.</span></span> <span data-ttu-id="0dd5c-114">[Let's Encrypt](https://letsencrypt.org/) является центром сертификации, предлагающим бесплатные сертификаты.</span><span class="sxs-lookup"><span data-stu-id="0dd5c-114">[Let's Encrypt](https://letsencrypt.org/) is a certificate authority that offers free certificates.</span></span>

<span data-ttu-id="0dd5c-115">В этом документе используются [самозаверяющие сертификаты разработки](https://en.wikipedia.org/wiki/Self-signed_certificate) для размещения предварительно созданных образов с `localhost`.</span><span class="sxs-lookup"><span data-stu-id="0dd5c-115">This document uses [self-signed development certificates](https://en.wikipedia.org/wiki/Self-signed_certificate) for hosting pre-built images over `localhost`.</span></span> <span data-ttu-id="0dd5c-116">Инструкции аналогичны использованию рабочих сертификатов.</span><span class="sxs-lookup"><span data-stu-id="0dd5c-116">The instructions are similar to using production certificates.</span></span>

<span data-ttu-id="0dd5c-117">Для производственных сертификатов:</span><span class="sxs-lookup"><span data-stu-id="0dd5c-117">For production certs:</span></span>

* <span data-ttu-id="0dd5c-118">Средство `dotnet dev-certs` не является обязательным.</span><span class="sxs-lookup"><span data-stu-id="0dd5c-118">The `dotnet dev-certs` tool is not required.</span></span>
* <span data-ttu-id="0dd5c-119">Сертификаты не обязательно хранить в расположении, используемом в инструкциях.</span><span class="sxs-lookup"><span data-stu-id="0dd5c-119">Certificates do not need to be stored in the location used in the instructions.</span></span> <span data-ttu-id="0dd5c-120">Должно работать любое расположение, хотя не рекомендуется хранить сертификаты в каталоге сайта.</span><span class="sxs-lookup"><span data-stu-id="0dd5c-120">Any location should work, although storing certs within your site directory is not recommended.</span></span>

<span data-ttu-id="0dd5c-121">Инструкции, содержащиеся в следующем разделе, прикрепляют сертификаты к контейнерам с помощью параметра командной строки DOCKER `-v`.</span><span class="sxs-lookup"><span data-stu-id="0dd5c-121">The instructions contained in the following section volume mount certificates into containers using Docker's `-v` command-line option.</span></span> <span data-ttu-id="0dd5c-122">Вы можете добавить сертификаты в образы контейнеров с помощью команды `COPY` *Dockerfile*, но это не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="0dd5c-122">You could add certificates into container images with a `COPY` command in a *Dockerfile*, but it's not recommended.</span></span> <span data-ttu-id="0dd5c-123">Копирование сертификатов в образ не рекомендуется по следующим причинам:</span><span class="sxs-lookup"><span data-stu-id="0dd5c-123">Copying certificates into an image isn't recommended for the following reasons:</span></span>

* <span data-ttu-id="0dd5c-124">Использование одного и того же образа для тестирования с помощью сертификатов разработчика затрудняется.</span><span class="sxs-lookup"><span data-stu-id="0dd5c-124">It makes difficult to use the same image for testing with developer certificates.</span></span>
* <span data-ttu-id="0dd5c-125">Использование одного и того же образа для размещения с производственными сертификатами затрудняется.</span><span class="sxs-lookup"><span data-stu-id="0dd5c-125">It makes difficult to use the same image for Hosting with production certificates.</span></span>
* <span data-ttu-id="0dd5c-126">Существует значительный риск раскрытия сертификата.</span><span class="sxs-lookup"><span data-stu-id="0dd5c-126">There is significant risk of certificate disclosure.</span></span>

## <a name="running-pre-built-container-images-with-https"></a><span data-ttu-id="0dd5c-127">Запуск предварительно созданных образов контейнеров с помощью HTTPS</span><span class="sxs-lookup"><span data-stu-id="0dd5c-127">Running pre-built container images with HTTPS</span></span>

<span data-ttu-id="0dd5c-128">Используйте следующие инструкции для настройки операционной системы.</span><span class="sxs-lookup"><span data-stu-id="0dd5c-128">Use the following instructions for your operating system configuration.</span></span>

### <a name="windows-using-linux-containers"></a><span data-ttu-id="0dd5c-129">Windows с контейнерами Linux</span><span class="sxs-lookup"><span data-stu-id="0dd5c-129">Windows using Linux containers</span></span>

<span data-ttu-id="0dd5c-130">Создать сертификат и настроить локальный компьютер:</span><span class="sxs-lookup"><span data-stu-id="0dd5c-130">Generate certificate and configure local machine:</span></span>

```dotnetcli
dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

<span data-ttu-id="0dd5c-131">В предыдущих командах замените `{ password here }` паролем.</span><span class="sxs-lookup"><span data-stu-id="0dd5c-131">In the preceding commands, replace `{ password here }` with a password.</span></span>

<span data-ttu-id="0dd5c-132">Запустите образ контейнера с ASP.NET Core, настроенным для HTTPS:</span><span class="sxs-lookup"><span data-stu-id="0dd5c-132">Run the container image with ASP.NET Core configured for HTTPS:</span></span>

```console
docker pull mcr.microsoft.com/dotnet/core/samples:aspnetapp
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_Kestrel__Certificates__Default__Password="password" -e ASPNETCORE_Kestrel__Certificates__Default__Path=/https/aspnetapp.pfx -v %USERPROFILE%\.aspnet\https:/https/ mcr.microsoft.com/dotnet/core/samples:aspnetapp
```

<span data-ttu-id="0dd5c-133">Пароль должен совпадать с паролем, используемым для сертификата.</span><span class="sxs-lookup"><span data-stu-id="0dd5c-133">The password must match the password used for the certificate.</span></span>

### <a name="macos-or-linux"></a><span data-ttu-id="0dd5c-134">macOS или Linux</span><span class="sxs-lookup"><span data-stu-id="0dd5c-134">macOS or Linux</span></span>

<span data-ttu-id="0dd5c-135">Создать сертификат и настроить локальный компьютер:</span><span class="sxs-lookup"><span data-stu-id="0dd5c-135">Generate certificate and configure local machine:</span></span>

```dotnetcli
dotnet dev-certs https -ep ${HOME}/.aspnet/https/aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

<span data-ttu-id="0dd5c-136">`dotnet dev-certs https --trust` поддерживается только в macOS и Windows.</span><span class="sxs-lookup"><span data-stu-id="0dd5c-136">`dotnet dev-certs https --trust` is only supported on macOS and Windows.</span></span> <span data-ttu-id="0dd5c-137">Необходимо доверять сертификатам в Linux так, как это поддерживается вашим дистрибутив.</span><span class="sxs-lookup"><span data-stu-id="0dd5c-137">You need to trust certs on Linux in the way that is supported by your distro.</span></span> <span data-ttu-id="0dd5c-138">Вероятно, вам нужно доверять сертификату в браузере.</span><span class="sxs-lookup"><span data-stu-id="0dd5c-138">It is likely that you need to trust the certificate in your browser.</span></span>

<span data-ttu-id="0dd5c-139">В предыдущих командах замените `{ password here }` паролем.</span><span class="sxs-lookup"><span data-stu-id="0dd5c-139">In the preceding commands, replace `{ password here }` with a password.</span></span>

<span data-ttu-id="0dd5c-140">Запустите образ контейнера с ASP.NET Core, настроенным для HTTPS:</span><span class="sxs-lookup"><span data-stu-id="0dd5c-140">Run the container image with ASP.NET Core configured for HTTPS:</span></span>

```console
docker pull mcr.microsoft.com/dotnet/core/samples:aspnetapp
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_Kestrel__Certificates__Default__Password="password" -e ASPNETCORE_Kestrel__Certificates__Default__Path=/https/aspnetapp.pfx -v ${HOME}/.aspnet/https:/https/ mcr.microsoft.com/dotnet/core/samples:aspnetapp
```

<span data-ttu-id="0dd5c-141">Пароль должен совпадать с паролем, используемым для сертификата.</span><span class="sxs-lookup"><span data-stu-id="0dd5c-141">The password must match the password used for the certificate.</span></span>

### <a name="windows-using-windows-containers"></a><span data-ttu-id="0dd5c-142">Windows, использующих контейнеры Windows</span><span class="sxs-lookup"><span data-stu-id="0dd5c-142">Windows using Windows containers</span></span>

<span data-ttu-id="0dd5c-143">Создать сертификат и настроить локальный компьютер:</span><span class="sxs-lookup"><span data-stu-id="0dd5c-143">Generate certificate and configure local machine:</span></span>

```dotnetcli
dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p { password here }
dotnet dev-certs https --trust
```

<span data-ttu-id="0dd5c-144">В предыдущих командах замените `{ password here }` паролем.</span><span class="sxs-lookup"><span data-stu-id="0dd5c-144">In the preceding commands, replace `{ password here }` with a password.</span></span>

<span data-ttu-id="0dd5c-145">Запустите образ контейнера с ASP.NET Core, настроенным для HTTPS:</span><span class="sxs-lookup"><span data-stu-id="0dd5c-145">Run the container image with ASP.NET Core configured for HTTPS:</span></span>

```console
docker pull mcr.microsoft.com/dotnet/core/samples:aspnetapp
docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_Kestrel__Certificates__Default__Password="password" -e ASPNETCORE_Kestrel__Certificates__Default__Path=\https\aspnetapp.pfx -v %USERPROFILE%\.aspnet\https:C:\https\ mcr.microsoft.com/dotnet/core/samples:aspnetapp
```

<span data-ttu-id="0dd5c-146">Пароль должен совпадать с паролем, используемым для сертификата.</span><span class="sxs-lookup"><span data-stu-id="0dd5c-146">The password must match the password used for the certificate.</span></span>
