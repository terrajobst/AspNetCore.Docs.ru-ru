---
title: Использование шаблонов одностраничных приложений с ASP.NET Core
author: SteveSandersonMS
description: Узнайте, как установить и начать использовать шаблоны проектов одностраничных приложений (SPA) в ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/index
ms.openlocfilehash: ab164ae5d2df47739ec04b32cd21835ffdf9f87f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291447"
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
