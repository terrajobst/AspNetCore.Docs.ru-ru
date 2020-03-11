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
# <a name="migrate-from-microsoftextensionslogging-21-to-22-or-30"></a><span data-ttu-id="3d3f1-103">Миграция из Microsoft. Extensions. Logging 2,1 в 2,2 или 3,0</span><span class="sxs-lookup"><span data-stu-id="3d3f1-103">Migrate from Microsoft.Extensions.Logging 2.1 to 2.2 or 3.0</span></span>

<span data-ttu-id="3d3f1-104">В этой статье описаны общие шаги для миграции приложения non-ASP.NET Core, которое использует `Microsoft.Extensions.Logging` от 2,1 до 2,2 или 3,0.</span><span class="sxs-lookup"><span data-stu-id="3d3f1-104">This article outlines the common steps for migrating a non-ASP.NET Core application that uses `Microsoft.Extensions.Logging` from 2.1 to 2.2 or 3.0.</span></span>

## <a name="21-to-22"></a><span data-ttu-id="3d3f1-105">С версии 2.1 на 2.2</span><span class="sxs-lookup"><span data-stu-id="3d3f1-105">2.1 to 2.2</span></span>

<span data-ttu-id="3d3f1-106">Вручную создайте `ServiceCollection` и вызовите `AddLogging`.</span><span class="sxs-lookup"><span data-stu-id="3d3f1-106">Manually create `ServiceCollection` and call `AddLogging`.</span></span>

<span data-ttu-id="3d3f1-107">2,1. Пример:</span><span class="sxs-lookup"><span data-stu-id="3d3f1-107">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="3d3f1-108">2,2. Пример:</span><span class="sxs-lookup"><span data-stu-id="3d3f1-108">2.2 example:</span></span>

```csharp
var serviceCollection = new ServiceCollection();
serviceCollection.AddLogging(builder => builder.AddConsole());

using (var serviceProvider = serviceCollection.BuildServiceProvider())
using (var loggerFactory = serviceProvider.GetService<ILoggerFactory>())
{
    // use loggerFactory
}
```

## <a name="21-to-30"></a><span data-ttu-id="3d3f1-109">от 2,1 до 3,0</span><span class="sxs-lookup"><span data-stu-id="3d3f1-109">2.1 to 3.0</span></span>

<span data-ttu-id="3d3f1-110">В 3,0 используйте `LoggingFactory.Create`.</span><span class="sxs-lookup"><span data-stu-id="3d3f1-110">In 3.0, use `LoggingFactory.Create`.</span></span>

<span data-ttu-id="3d3f1-111">2,1. Пример:</span><span class="sxs-lookup"><span data-stu-id="3d3f1-111">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="3d3f1-112">3,0. Пример:</span><span class="sxs-lookup"><span data-stu-id="3d3f1-112">3.0 example:</span></span>

```csharp
using (var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole()))
{
    // use loggerFactory
}
```

## <a name="additional-resources"></a><span data-ttu-id="3d3f1-113">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="3d3f1-113">Additional resources</span></span>

<xref:fundamentals/logging/index>