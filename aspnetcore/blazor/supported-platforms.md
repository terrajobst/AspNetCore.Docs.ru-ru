---
title: Поддерживаемые платформы ASP.NET Core Блазор
author: guardrex
description: Сведения о поддерживаемых платформах для ASP.NET Core Блазор.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: blazor/supported-platforms
ms.openlocfilehash: 8730417f772c84ebcccc449a5826126aa5c64abb
ms.sourcegitcommit: e5a74f882c14eaa0e5639ff082355e130559ba83
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/20/2019
ms.locfileid: "71168166"
---
# <a name="aspnet-core-blazor-supported-platforms"></a>Поддерживаемые платформы ASP.NET Core Блазор

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="browser-requirements"></a>Требования к браузеру

### <a name="blazor-webassembly"></a>Blazor WebAssembly

| Браузер                          | Версия               |
| -------------------------------- | :-------------------: |
| Microsoft Edge                   | Текущие               |
| Mozilla Firefox                  | Текущие               |
| Google Chrome, включая Android | Текущие               |
| Safari, включая iOS            | Текущие               |
| Microsoft Internet Explorer      | Не поддерживается&dagger; |

&dagger;Microsoft Internet Explorer не поддерживает [сборку](https://webassembly.org).

### <a name="blazor-server"></a>Blazor Server

| Браузер                          | Версия    |
| -------------------------------- | :--------: |
| Microsoft Edge                   | Текущие    |
| Mozilla Firefox                  | Текущие    |
| Google Chrome, включая Android | Текущие    |
| Safari, включая iOS            | Текущие    |
| Microsoft Internet Explorer      | стр&dagger; |

&dagger;Требуются дополнительные заполнения (например, обещания можно добавить через пакет [PolyFill.IO](https://polyfill.io/v3/) ).

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:blazor/hosting-models>
