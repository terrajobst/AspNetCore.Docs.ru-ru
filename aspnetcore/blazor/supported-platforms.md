---
title: Поддерживаемые платформы ASP.NET Core Blazor
author: guardrex
description: Сведения о поддерживаемых платформах для ASP.NET Core Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
no-loc:
- Blazor
uid: blazor/supported-platforms
ms.openlocfilehash: de51296cc8785474e1c1406cfd5d4e5bd4050172
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/12/2019
ms.locfileid: "73962728"
---
# <a name="aspnet-core-opno-locblazor-supported-platforms"></a>Поддерживаемые платформы ASP.NET Core Blazor

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="browser-requirements"></a>Требования к браузеру

### <a name="opno-locblazor-webassembly"></a>Blazorная сборка

| Браузер                          | Version               |
| -------------------------------- | :-------------------: |
| Microsoft Edge                   | Текущие               |
| Mozilla Firefox                  | Текущие               |
| Google Chrome, включая Android | Текущие               |
| Safari, включая iOS            | Текущие               |
| Microsoft Internet Explorer      | Не поддерживается&dagger; |

&dagger;Microsoft Internet Explorer не поддерживает [сборку](https://webassembly.org).

### <a name="opno-locblazor-server"></a>Сервер Blazor

| Браузер                          | Version    |
| -------------------------------- | :--------: |
| Microsoft Edge                   | Текущие    |
| Mozilla Firefox                  | Текущие    |
| Google Chrome, включая Android | Текущие    |
| Safari, включая iOS            | Текущие    |
| Microsoft Internet Explorer      | 11&dagger; |

требуются &dagger;Additionalные заливки (например, обещания можно добавить через пакет [PolyFill.IO](https://polyfill.io/v3/) ).

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:blazor/hosting-models>
