---
title: "DI не зависимые сценарии для защиты данных в ASP.NET Core"
author: rick-anderson
description: "Узнайте, как для поддержки сценариев защиты данных, где невозможно или не хотите использовать службу, предоставляемую классом внедрения зависимостей."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/non-di-scenarios
ms.openlocfilehash: 1c84cfcf44086359a7d6900ca52781dc6f3b1b10
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="non-di-aware-scenarios-for-data-protection-in-aspnet-core"></a>DI не зависимые сценарии для защиты данных в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Система защиты данных ASP.NET Core — это обычно [добавлена в контейнер службы](xref:security/data-protection/consumer-apis/overview) и используемый зависимых компонентов с помощью внедрения зависимости (DI). Однако бывают случаи, когда это не нецелесообразно или нежелателен, особенно при импорте существующего приложения в системе.

Для поддержки следующих сценариев [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) пакет содержит конкретный тип [DataProtectionProvider](/dotnet/api/Microsoft.AspNetCore.DataProtection.DataProtectionProvider), который предоставляет простой способ использования защиты данных не полагаться на DI. `DataProtectionProvider` Тип реализует [IDataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionprovider). Создав `DataProtectionProvider` только требует предоставления [DirectoryInfo](/dotnet/api/system.io.directoryinfo) экземпляра для указания места хранения криптографических ключей поставщика, как показано в следующем образце кода:

[!code-none[Main](non-di-scenarios/_static/nodisample1.cs)]

По умолчанию `DataProtectionProvider` конкретный тип не шифровать необработанные материал ключа перед его сохранении в файловой системе. Это необходимо для поддержки сценариев, где точки разработчика для общей сетевой папке и системы защиты данных не может вывести автоматически механизм шифрования ключа соответствующие статических.

Кроме того `DataProtectionProvider` конкретный тип не [изолировать приложения](xref:security/data-protection/configuration/overview#per-application-isolation) по умолчанию. Все приложения, используя тот же каталог ключа могут совместно использовать полезных данных, при условии, что их [цели параметры](xref:security/data-protection/consumer-apis/purpose-strings) совпадают.

[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) конструктор принимает обратный дополнительной конфигурации, который можно использовать для настройки поведения системы. В следующем примере показано восстановление изоляции для явного вызова [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname). В примере также демонстрируется настройка системы для автоматического шифрования материализованного ключа с помощью Windows DPAPI. Если каталог указывает на общий ресурс UNC, вы можете распределять общий сертификат на всех соответствующих компьютерах и для настройки системы для использования шифрования на основе сертификатов с помощью вызова [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate).

[!code-none[Main](non-di-scenarios/_static/nodisample2.cs)]

> [!TIP]
> Экземпляры `DataProtectionProvider` сопряжено создать конкретный тип. Если приложение поддерживает несколько экземпляров этого типа, и они все используют один и тот же каталог хранилища ключей, может привести к снижению производительности приложения. Если вы используете `DataProtectionProvider` типа, мы рекомендуем создать такую один раз и использовать его максимальной. `DataProtectionProvider` Типа и все [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) экземпляры, созданные из этого являются потокобезопасными для нескольких клиентов.
