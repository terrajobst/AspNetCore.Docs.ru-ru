---
title: Поддерживаемые платформы ASP.NET Core Blazor
author: guardrex
description: Сведения о поддерживаемых платформах для ASP.NET Core Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/supported-platforms
ms.openlocfilehash: 505974280b5c96ec2bcae42c6e076ab67a15bb07
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/17/2020
ms.locfileid: "76160136"
---
# <a name="aspnet-core-opno-locblazor-supported-platforms"></a>Поддерживаемые платформы ASP.NET Core Blazor

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="browser-requirements"></a>Требования к браузеру

### <a name="opno-locblazor-webassembly"></a>Blazor WebAssembly

| Браузер                          | {2&gt;Version&lt;2}               |
| -------------------------------- | :-------------------: |
| Microsoft Edge                   | Current               |
| Mozilla Firefox                  | Current               |
| Google Chrome, включая Android | Current               |
| Safari, включая iOS            | Current               |
| Microsoft Internet Explorer      | Не поддерживается&dagger; |

&dagger;Microsoft Internet Explorer не поддерживает [сборку](https://webassembly.org).

### <a name="opno-locblazor-server"></a>Blazor Server

| Браузер                          | {2&gt;Version&lt;2}    |
| -------------------------------- | :--------: |
| Microsoft Edge                   | Current    |
| Mozilla Firefox                  | Current    |
| Google Chrome, включая Android | Current    |
| Safari, включая iOS            | Current    |
| Microsoft Internet Explorer      | 11&dagger; |

требуются &dagger;дополнительные заполнения (например, обещания можно добавить через пакет [PolyFill.IO](https://polyfill.io/v3/) ).

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:blazor/hosting-models>
