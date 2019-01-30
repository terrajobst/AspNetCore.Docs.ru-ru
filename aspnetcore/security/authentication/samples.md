---
title: Примеры проверки подлинности для ASP.NET Core
author: rick-anderson
description: Ссылки на примеры проверки подлинности в ASP.NET Core репозитории.
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: 7b3c911d60ad4737ebd12ce6f7628ad624b11658
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236682"
---
# <a name="authentication-samples-for-aspnet-core"></a>Примеры проверки подлинности для ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

[Репозитория ASP.NET Core](https://github.com/aspnet/AspNetCore) содержит следующие примеры проверки подлинности *AspNetCore/src/Security/samples* папку:

* [Преобразование заявок](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [Файл cookie проверки подлинности](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [Поставщик настраиваемой политики — IAuthorizationPolicyProvider](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [Схемы динамической проверки подлинности и параметры](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [Внешние утверждений](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [Выбор между файл cookie и другую схему проверки подлинности на основе запроса](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [Ограничивает доступ к статическим файлам](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>Выполнение примеров

* Выберите [ветви](https://github.com/aspnet/AspNetCore). Например `release/2.2` 
* Клонируйте или скачайте [репозитория ASP.NET Core](https://github.com/aspnet/AspNetCore).
* Проверка установки [пакет SDK для .NET Core](https://www.microsoft.com/net/download/all) версию, которая соответствует клон репозитория ASP.NET Core.
* Перейдите к выборке в *AspNetCore/src/Security/samples* и запуск примера с `dotnet run`.
