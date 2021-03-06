---
title: Веб-пакет SDK для ASP.NET Core
author: Rick-Anderson
description: Общие сведения о пакете Microsoft.NET.Sdk.Web.
ms.author: riande
ms.date: 01/25/2020
no-loc:
- Blazor
uid: razor-pages/web-sdk
ms.openlocfilehash: 6a9d531efd2188aed525c949bb124914c31119db
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2020
ms.locfileid: "78648208"
---
# <a name="aspnet-core-web-sdk"></a>Веб-пакет SDK для ASP.NET Core

### <a name="overview"></a>Обзор

`Microsoft.NET.Sdk.Web` — это [пакет SDK проекта MSBuild](https://docs.microsoft.com/visualstudio/msbuild/how-to-use-project-sdk) для разработки приложений ASP.NET Core. Приложения ASP.NET Core можно создавать и без этого пакета SDK, однако веб-пакет SDK дает следующие преимущества:

* обеспечивает максимальное удобство разработки;
* рекомендуется для большинства пользователей.

Используйте Web.SDK в проекте:

  ```xml
  <Project Sdk="Microsoft.NET.Sdk.Web">
    <!-- omitted for brevity -->
  </Project>
  ```

Возможности, обеспечиваемые веб-пакетом SDK:

* Проекты, предназначенные для .NET Core 3.0 или более поздней версии, имеют неявные ссылки на следующие компоненты:

  * [общая платформа ASP.NET Core](xref:fundamentals/metapackage-app);
  * [анализаторы](/visualstudio/extensibility/getting-started-with-roslyn-analyzers), предназначенные для создания приложений ASP.NET Core.
* Веб-пакет SDK импортирует целевые объекты MSBuild, которые позволяют использовать профили публикации и выполнять публикацию с помощью WebDeploy.

### <a name="properties"></a>Свойства

| Свойство | Description |
| -------- | ----------- |
| `DisableImplicitFrameworkReferences` | Отключает неявную ссылку на общую платформу `Microsoft.AspNetCore.App`. |
| `DisableImplicitAspNetCoreAnalyzers` | Отключает неявную ссылку на анализаторы ASP.NET Core. |
| `DisableImplicitComponentsAnalyzers` | Отключает неявную ссылку на анализаторы компонентов Razor при сборке приложений Blazor (серверных). |
