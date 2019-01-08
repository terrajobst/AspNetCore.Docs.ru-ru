---
title: Перенос из Microsoft.Extensions.Logging в 2.1 и 2.2 или 3.0
author: pakrym
description: Узнайте, как перенести приложение ASP.NET Core, которое использует Microsoft.Extensions.Logging из 2.1, 2.2 или 3.0.
ms.author: pakrym
ms.custom: mvc
ms.date: 01/04/2019
uid: migration/logging-nonaspnetcore
ms.openlocfilehash: 2519ddc02cee5978483bcaef4341a52aad3ba2a6
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099474"
---
# <a name="migrate-from-microsoftextensionslogging-21-to-22-or-30"></a>Перенос из Microsoft.Extensions.Logging в 2.1 и 2.2 или 3.0

В этой статье приведены общие шаги по переносу приложений ASP.NET Core, который использует `Microsoft.Extensions.Logging` из 2.1, 2.2 или 3.0.

## <a name="21-to-22"></a>Чтобы 2.1 и 2.2

Вручную создайте `ServiceCollection` и вызвать `AddLogging`.

Пример 2.1.

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

2,2 пример.

```csharp
var serviceCollection = new ServiceCollection();
serviceCollection.AddLogging(builder => builder.AddConsole());

using (var serviceProvider = serviceCollection.BuildServiceProvider())
using (var loggerFactory = serviceProvider.GetService<ILoggerFactory>())
{
    // use loggerFactory
}
```

## <a name="21-to-30"></a>2.1 до 3.0

В 3.0, используйте `LoggingFactory.Create`.

Пример 2.1.

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

Пример 3.0.

```csharp
using (var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole()))
{
    // use loggerFactory
}
```

## <a name="additional-resources"></a>Дополнительные ресурсы

<xref:fundamentals/logging/index>