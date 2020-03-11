---
title: Примеры проверки подлинности в ASP.NET Core
author: rick-anderson
description: Ссылки на примеры проверки подлинности в репозитории ASP.NET Core.
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: 3d7e28f6e501bd8bd3908ca4b314a63cee52ebe3
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78651658"
---
# <a name="authentication-samples-for-aspnet-core"></a>Примеры проверки подлинности в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

[Репозиторий ASP.NET Core](https://github.com/dotnet/AspNetCore) содержит следующие примеры проверки подлинности в папке *AspNetCore/src/Security/Samples* :

* [Преобразование утверждений](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/ClaimsTransformation)
* [Проверка подлинности файлов cookie](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/Cookies)
* [Поставщик настраиваемой политики — Иаусоризатионполиципровидер](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider)
* [Схемы и параметры динамической проверки подлинности](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/DynamicSchemes)
* [Внешние утверждения](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/Identity.ExternalClaims)
* [Выбор между файлами cookie и другой схемой проверки подлинности на основе запроса](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/PathSchemeSelection)
* [Разрешает доступ к статическим файлам](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>Запуск шаблонов

* Выберите [ветвь](https://github.com/dotnet/AspNetCore). Например `Tag:v3.0.0`.
* Клонировать или скачать [репозиторий ASP.NET Core](https://github.com/dotnet/AspNetCore).
* Убедитесь, что установлена версия [пакет SDK для .NET Core](https://www.microsoft.com/net/download/all) , соответствующая клону репозитория ASP.NET Core.
* Перейдите к примеру в *AspNetCore/src/Security/Samples* и запустите пример с `dotnet run`.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[Репозиторий ASP.NET Core](https://github.com/dotnet/AspNetCore) содержит следующие примеры проверки подлинности в папке *AspNetCore/src/Security/Samples* :

* [Преобразование утверждений](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [Проверка подлинности файлов cookie](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [Поставщик настраиваемой политики — Иаусоризатионполиципровидер](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [Схемы и параметры динамической проверки подлинности](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [Внешние утверждения](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [Выбор между файлами cookie и другой схемой проверки подлинности на основе запроса](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [Разрешает доступ к статическим файлам](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>Запуск шаблонов

* Выберите [ветвь](https://github.com/dotnet/AspNetCore). Например `release/2.2`.
* Клонировать или скачать [репозиторий ASP.NET Core](https://github.com/dotnet/AspNetCore).
* Убедитесь, что установлена версия [пакет SDK для .NET Core](https://www.microsoft.com/net/download/all) , соответствующая клону репозитория ASP.NET Core.
* Перейдите к примеру в *AspNetCore/src/Security/Samples* и запустите пример с `dotnet run`.

::: moniker-end
