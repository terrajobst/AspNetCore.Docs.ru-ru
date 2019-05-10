---
title: Без поддержки Внедрения зависимые сценарии для защиты данных в ASP.NET Core
author: rick-anderson
description: Узнайте, как для поддержки сценариев защиты данных, где нельзя или не хотите использовать службу, предоставляемую внедрения зависимостей.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/configuration/non-di-scenarios
ms.openlocfilehash: 34354c8443f6ae00bcce6ad9bdb6c11aaaa25bf8
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897311"
---
# <a name="non-di-aware-scenarios-for-data-protection-in-aspnet-core"></a>Без поддержки Внедрения зависимые сценарии для защиты данных в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Система защиты данных в ASP.NET Core — это обычно [добавлены в контейнер службы](xref:security/data-protection/consumer-apis/overview) и используемый зависимые компоненты посредством внедрения зависимостей (DI). Однако бывают случаи, когда это не невозможно или нежелательно, особенно в том случае, при импорте в системе в существующее приложение.

Для поддержки следующих сценариев [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) пакет содержит конкретный тип, [DataProtectionProvider](/dotnet/api/Microsoft.AspNetCore.DataProtection.DataProtectionProvider), который предлагает простой способ использования защиты данных без использования внедрения Зависимостей. `DataProtectionProvider` Введите реализует [IDataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionprovider). Создав `DataProtectionProvider` только необходимо указать [DirectoryInfo](/dotnet/api/system.io.directoryinfo) экземпляре указывает хранения криптографических ключей поставщика, как показано в следующем примере кода:

[!code-none[](non-di-scenarios/_static/nodisample1.cs)]

По умолчанию `DataProtectionProvider` конкретный тип не шифровать необработанные материал ключа перед их сохранением в файловой системе. Это необходимо для поддержки сценариев, где точки разработчиков в общей сетевой папке и систему защиты данных не может вывести автоматически механизм соответствующие неактивных ключей шифрования.

Кроме того `DataProtectionProvider` конкретный тип не [изолировать приложения](xref:security/data-protection/configuration/overview#per-application-isolation) по умолчанию. Все приложения, используя тот же каталог ключей могут совместно использовать полезные данные, при условии их [назначения параметров](xref:security/data-protection/consumer-apis/purpose-strings) совпадают.

[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) конструктор принимает необязательная конфигурация обратного вызова, который может использоваться для настройки поведения системы. Следующий пример демонстрирует восстановление изоляция с помощью явного вызова [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname). В примере также демонстрируется настройка системы для автоматического шифрования постоянных ключей, с помощью Windows DPAPI. Если каталог указывает на общий ресурс UNC, вы можете распределить всех соответствующих машин сертификат с общим доступом и настроить систему для использования шифрования на основе сертификата с помощью вызова [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate).

[!code-none[](non-di-scenarios/_static/nodisample2.cs)]

> [!TIP]
> Экземпляры `DataProtectionProvider` конкретный тип затратные при создании. Если приложение поддерживает несколько экземпляров этого типа, и они все используют один и тот же каталог хранилища ключей, может привести к снижению производительности приложения. Если вы используете `DataProtectionProvider` типа, мы рекомендуем создать этот тип один раз и использовать его как можно больше. `DataProtectionProvider` Типа и все [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) экземпляров, созданных из нее являются поточно ориентированными для нескольких клиентов.
