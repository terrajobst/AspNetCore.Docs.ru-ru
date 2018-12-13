---
title: Средства диагностики производительности
author: mjrousos
description: Полезные инструменты для диагностики проблем с производительностью в приложениях ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 12/07/2018
uid: performance/diagnostic-tools
ms.openlocfilehash: 3093b7d646e4fa943334c7b1e70ddc007ab18780
ms.sourcegitcommit: 1ea1b4fc58055c62728143388562689f1ef96cb2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/13/2018
ms.locfileid: "53329207"
---
# <a name="performance-diagnostic-tools"></a>Средства диагностики производительности

По [Майк Роусос](https://github.com/mjrousos)

В этой статье перечислены инструменты для выявления проблем с производительностью в ASP.NET Core.

## <a name="visual-studio-diagnostic-tools"></a>Средства диагностики Visual Studio

[Средства профилирования и диагностики](/visualstudio/profiling) интегрирован в Visual Studio — хорошее место для начала расследования проблем с производительностью. Эти средства являются мощным и удобным механизмом для использования в среде разработки Visual Studio. Инструментарий обеспечивает анализ использования ЦП, использование памяти и событий производительности в приложениях ASP.NET Core.

Дополнительные сведения можно найти в [документация по Visual Studio](/visualstudio/profiling/profiling-overview).

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/app-insights-overview) приводятся подробные данные производительности для вашего приложения. Application Insights автоматически собирает данные о скорость ответа, частота сбоев, время отклика зависимостей и многое другое. Application Insights поддерживает ведение журнала пользовательских событий и конкретных метрик для приложения.

Application Insights можно использовать в различных средах:

* Оптимизирована для работы в Azure.
* Работает в рабочей среде, разработки и размещения.
* Работает локально из [Visual Studio](/azure/application-insights/app-insights-visual-studio) или в других средах размещения.

Дополнительные сведения см. в статье [Application Insights для ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="perfview"></a>PerfView

[PerfView](https://github.com/Microsoft/perfview) представляет собой средство анализа производительности, созданные командой .NET, в частности, для диагностики проблем с производительностью .NET. PerfView обеспечивает анализ ЦП использования памяти и сборки Мусора поведение, событий производительности и время по часам.

Дополнительные сведения о PerfView и как приступить к работе с [видео учебники по PerfView](http://channel9.msdn.com/Series/PerfView-Tutorial) или посредством считывания руководства пользователя, доступных в средстве или [на сайте GitHub](https://github.com/Microsoft/perfview).

## <a name="perfcollect"></a>PerfCollect

Хотя PerfView представляет собой средство анализа производительности полезно для сценариев .NET, оно работает только в Windows, поэтому его нельзя использовать для сбора данных трассировки из приложений ASP.NET Core, запущенных в средах Linux.

[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) — это сценарий bash, который использует собственный Linux, средства профилирования ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page) и [LTTng](https://lttng.org/)) для сбора данных трассировки в Linux, можно анализировать по PerfView. PerfCollect полезно в тех случаях, когда проблемы с производительностью отображаются в средах Linux, где PerfView не может использоваться непосредственно. Вместо этого PerfCollect можно собирать трассировки из приложений .NET Core, которые затем анализируются на компьютере Windows с помощью PerfView.

Дополнительные сведения о том, как установить и приступить к работе с PerfCollect [на сайте GitHub](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).
