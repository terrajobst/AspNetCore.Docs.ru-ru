---
title: Миграция из Microsoft. Extensions. Logging 2,1 в 2,2 или 3,0
author: pakrym
description: Узнайте, как перенести приложение non-ASP.NET Core, использующее Microsoft. Extensions. Logging от 2,1 до 2,2 или 3,0.
ms.author: pakrym
ms.custom: mvc
ms.date: 01/04/2019
uid: migration/logging-nonaspnetcore
ms.openlocfilehash: 2519ddc02cee5978483bcaef4341a52aad3ba2a6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78651880"
---
# <a name="migrate-from-microsoftextensionslogging-21-to-22-or-30"></a>Миграция из Microsoft. Extensions. Logging 2,1 в 2,2 или 3,0

В этой статье описаны общие шаги для миграции приложения non-ASP.NET Core, которое использует `Microsoft.Extensions.Logging` от 2,1 до 2,2 или 3,0.

## <a name="21-to-22"></a>С версии 2.1 на 2.2

Вручную создайте `ServiceCollection` и вызовите `AddLogging`.

2,1. Пример:

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

2,2. Пример:

```csharp
var serviceCollection = new ServiceCollection();
serviceCollection.AddLogging(builder => builder.AddConsole());

using (var serviceProvider = serviceCollection.BuildServiceProvider())
using (var loggerFactory = serviceProvider.GetService<ILoggerFactory>())
{
    // use loggerFactory
}
```

## <a name="21-to-30"></a>от 2,1 до 3,0

В 3,0 используйте `LoggingFactory.Create`.

2,1. Пример:

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

3,0. Пример:

```csharp
using (var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole()))
{
    // use loggerFactory
}
```

## <a name="additional-resources"></a>Дополнительные ресурсы

<xref:fundamentals/logging/index>