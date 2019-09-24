---
title: Поддерживаемые платформы ASP.NET Core Блазор
author: guardrex
description: Сведения о поддерживаемых платформах для ASP.NET Core Блазор.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: blazor/supported-platforms
ms.openlocfilehash: b769ee175cde7c9a613d7fb70949de129ca428d3
ms.sourcegitcommit: 79eeb17604b536e8f34641d1e6b697fb9a2ee21f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2019
ms.locfileid: "71211569"
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
