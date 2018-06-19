---
title: Использование шаблонов одностраничных приложений с ASP.NET Core
author: SteveSandersonMS
description: Узнайте, как установить и начать использовать шаблоны проектов одностраничных приложений (SPA) в ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/index
ms.openlocfilehash: f7bb23e9001c7606c3e622bf4575a4debec56ccd
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/27/2018
ms.locfileid: "34555564"
---
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a>Использование шаблонов одностраничных приложений с ASP.NET Core

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> Выпущенный пакет SDK для .NET Core 2.0.x содержит более ранние шаблоны проектов Angular, React и React с Redux. Эта документация не относится к этим шаблонам. Она относится к новейшим шаблонам Angular, React и React с Redux, которые можно установить в ASP.NET Core 2.0 вручную. ASP.NET Core 2.1 включает обновленные шаблоны по умолчанию.

::: moniker-end

## <a name="prerequisites"></a>Предварительные требования

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* [Node.js](https://nodejs.org) 6 или более поздней версии

## <a name="installation"></a>Установка

::: moniker range=">= aspnetcore-2.1"

Шаблоны уже установлены вместе с ASP.NET Core 2.1.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Если вы используете ASP.NET Core 2.0, то чтобы установить обновленные шаблоны Angular, React и React с Redux для ASP.NET Core, выполните следующую команду.

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

::: moniker-end

## <a name="use-the-templates"></a>Использование шаблонов

* [Использование шаблона проекта Angular](xref:spa/angular)
* [Использование шаблона проекта React](xref:spa/react)
* [Использование шаблона проекта React и Redux](xref:spa/react-with-redux)
