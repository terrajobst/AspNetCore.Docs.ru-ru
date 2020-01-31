---
title: Веб-пакет SDK ASP.NET Core
author: Rick-Anderson
description: Общие сведения о Microsoft. NET. SDK. Web.
ms.author: riande
ms.date: 01/25/2020
no-loc:
- Blazor
uid: razor-pages/web-sdk
ms.openlocfilehash: 6a9d531efd2188aed525c949bb124914c31119db
ms.sourcegitcommit: fe41cff0b99f3920b727286944e5b652ca301640
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2020
ms.locfileid: "76869770"
---
# <a name="aspnet-core-web-sdk"></a>Веб-пакет SDK ASP.NET Core

### <a name="overview"></a>Обзор

`Microsoft.NET.Sdk.Web` — это [пакет SDK проекта MSBuild](https://docs.microsoft.com/visualstudio/msbuild/how-to-use-project-sdk) для создания приложений ASP.NET Core. Можно создать ASP.NET Core приложение без этого пакета SDK, однако веб-пакет SDK будет следующим:

* Приспособлено к обеспечению первого класса.
* Рекомендуемый целевой объект для большинства пользователей.

Использование Web. SDK в проекте:

  ```xml
  <Project Sdk="Microsoft.NET.Sdk.Web">
    <!-- omitted for brevity -->
  </Project>
  ```

Функции, доступные с помощью веб-пакета SDK:

* Проекты, предназначенные для .NET Core 3,0 или более поздней версии, неявно ссылаются на:

  * [ASP.NET Core общая платформа](xref:fundamentals/metapackage-app).
  * [Анализаторы](/visualstudio/extensibility/getting-started-with-roslyn-analyzers) , предназначенные для создания ASP.NET Core приложений.
* Веб-пакет SDK импортирует целевые объекты MSBuild, которые позволяют использовать профили публикации и публикацию с помощью WebDeploy.

### <a name="properties"></a>Свойства

| Идентификаторы | Описание |
| -------- | ----------- |
| `DisableImplicitFrameworkReferences` | Отключает неявную ссылку на `Microsoft.AspNetCore.App` общую платформу. |
| `DisableImplicitAspNetCoreAnalyzers` | Отключает неявную ссылку на анализаторы ASP.NET Core. |
| `DisableImplicitComponentsAnalyzers` | Отключает неявную ссылку на анализаторы компонентов Razor при создании приложений Blazor (сервера). |
