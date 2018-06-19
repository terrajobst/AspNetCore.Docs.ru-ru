---
title: DI не зависимые сценарии для защиты данных в ASP.NET Core
author: rick-anderson
description: Узнайте, как для поддержки сценариев защиты данных, где невозможно или не хотите использовать службу, предоставляемую классом внедрения зависимостей.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/configuration/non-di-scenarios
ms.openlocfilehash: d878bd20489876f919f2a8e0149f3f000cbf72d8
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2018
ms.locfileid: "29727250"
---
# <a name="non-di-aware-scenarios-for-data-protection-in-aspnet-core"></a>DI не зависимые сценарии для защиты данных в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Система защиты данных ASP.NET Core — это обычно [добавлена в контейнер службы](xref:security/data-protection/consumer-apis/overview) и используемый зависимых компонентов с помощью внедрения зависимости (DI). Однако бывают случаи, когда это не нецелесообразно или нежелателен, особенно при импорте существующего приложения в системе.

Для поддержки следующих сценариев [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) пакет содержит конкретный тип [DataProtectionProvider](/dotnet/api/Microsoft.AspNetCore.DataProtection.DataProtectionProvider), который предоставляет простой способ использования защиты данных не полагаться на DI. `DataProtectionProvider` Тип реализует [IDataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionprovider). Создав `DataProtectionProvider` только требует предоставления [DirectoryInfo](/dotnet/api/system.io.directoryinfo) экземпляра для указания места хранения криптографических ключей поставщика, как показано в следующем образце кода:

[!code-none[](non-di-scenarios/_static/nodisample1.cs)]

По умолчанию `DataProtectionProvider` конкретный тип не шифровать необработанные материал ключа перед его сохранении в файловой системе. Это необходимо для поддержки сценариев, где точки разработчика для общей сетевой папке и системы защиты данных не может вывести автоматически механизм шифрования ключа соответствующие статических.

Кроме того `DataProtectionProvider` конкретный тип не [изолировать приложения](xref:security/data-protection/configuration/overview#per-application-isolation) по умолчанию. Все приложения, используя тот же каталог ключа могут совместно использовать полезных данных, при условии, что их [цели параметры](xref:security/data-protection/consumer-apis/purpose-strings) совпадают.

[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) конструктор принимает обратный дополнительной конфигурации, который можно использовать для настройки поведения системы. В следующем примере показано восстановление изоляции для явного вызова [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname). В примере также демонстрируется настройка системы для автоматического шифрования материализованного ключа с помощью Windows DPAPI. Если каталог указывает на общий ресурс UNC, вы можете распределять общий сертификат на всех соответствующих компьютерах и для настройки системы для использования шифрования на основе сертификатов с помощью вызова [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate).

[!code-none[](non-di-scenarios/_static/nodisample2.cs)]

> [!TIP]
> Экземпляры `DataProtectionProvider` сопряжено создать конкретный тип. Если приложение поддерживает несколько экземпляров этого типа, и они все используют один и тот же каталог хранилища ключей, может привести к снижению производительности приложения. Если вы используете `DataProtectionProvider` типа, мы рекомендуем создать такую один раз и использовать его максимальной. `DataProtectionProvider` Типа и все [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) экземпляры, созданные из этого являются потокобезопасными для нескольких клиентов.
