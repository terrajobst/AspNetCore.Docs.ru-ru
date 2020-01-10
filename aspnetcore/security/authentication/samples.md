---
title: Примеры проверки подлинности в ASP.NET Core
author: rick-anderson
description: Ссылки на примеры проверки подлинности в репозитории ASP.NET Core.
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: 3d7e28f6e501bd8bd3908ca4b314a63cee52ebe3
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828676"
---
# <a name="authentication-samples-for-aspnet-core"></a>Примеры проверки подлинности в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

::: moniker range=">= aspnetcore-3.0"

[Репозиторий ASP.NET Core](https://github.com/dotnet/AspNetCore) содержит следующие примеры проверки подлинности в папке *AspNetCore/src/Security/samples*:

* [Преобразование утверждений](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/ClaimsTransformation)
* [Проверка подлинности с помощью cookie](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/Cookies)
* [Поставщик пользовательской политики — IAuthorizationPolicyProvider](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider)
* [Схемы и параметры динамической проверки подлинности](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/DynamicSchemes)
* [Внешние утверждения](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/Identity.ExternalClaims)
* [Выбор между cookie и другой схемой проверки подлинности на основе запроса](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/PathSchemeSelection)
* [Ограничение доступа к статическим файлам](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>Запуск примеров

* Выберите [ветвь](https://github.com/dotnet/AspNetCore). Например: `Tag:v3.0.0`
* Клонируйте или скачайте [репозиторий ASP.NET Core](https://github.com/dotnet/AspNetCore).
* Проверьте, что у вас установлен [пакет SDK для .NET Core](https://www.microsoft.com/net/download/all) версии, которая соответствует клону репозитория ASP.NET Core.
* Перейдите к примеру в *AspNetCore/src/Security/samples* и запустите его с помощью `dotnet run`.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[Репозиторий ASP.NET Core](https://github.com/dotnet/AspNetCore) содержит следующие примеры проверки подлинности в папке *AspNetCore/src/Security/samples*:

* [Преобразование утверждений](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [Проверка подлинности с помощью cookie](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [Поставщик пользовательской политики — IAuthorizationPolicyProvider](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [Схемы и параметры динамической проверки подлинности](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [Внешние утверждения](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [Выбор между cookie и другой схемой проверки подлинности на основе запроса](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [Ограничение доступа к статическим файлам](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>Запуск примеров

* Выберите [ветвь](https://github.com/dotnet/AspNetCore). Например: `release/2.2`
* Клонируйте или скачайте [репозиторий ASP.NET Core](https://github.com/dotnet/AspNetCore).
* Проверьте, что у вас установлен [пакет SDK для .NET Core](https://www.microsoft.com/net/download/all) версии, которая соответствует клону репозитория ASP.NET Core.
* Перейдите к примеру в *AspNetCore/src/Security/samples* и запустите его с помощью `dotnet run`.

::: moniker-end
