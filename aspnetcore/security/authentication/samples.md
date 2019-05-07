---
title: Примеры проверки подлинности для ASP.NET Core
author: rick-anderson
description: Ссылки на репозитории с примерами проверки подлинности в ASP.NET Core.
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
# <a name="authentication-samples-for-aspnet-core"></a>Примеры проверки подлинности в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

[Репозиторий ASP.NET Core](https://github.com/aspnet/AspNetCore) содержит следующие примеры проверки подлинности в папке *AspNetCore/src/Security/samples*:

* [Преобразование утверждений](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [Проверка подлинности с помощью cookies](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [Провайдер пользовательской политики — IAuthorizationPolicyProvider](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [Схемы динамической проверки подлинности и параметры](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [Внешние утверждения](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [Выбор между cookie и другой схемой проверки подлинности на основе запроса](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [Огранение доступа к статическим файлам](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>Запуск примеров

* Выберите [ветвь](https://github.com/aspnet/AspNetCore). Например `release/2.2` 
* Склонируйте или скачайте [репозиторий ASP.NET Core](https://github.com/aspnet/AspNetCore).
* Проверьте, что у вас установлен [пакет SDK для .NET Core](https://www.microsoft.com/net/download/all) версии, которая соответствует репозиторию ASP.NET Core.
* Перейдите к примеру в *AspNetCore/src/Security/samples* и запустите его с помощью `dotnet run`.
