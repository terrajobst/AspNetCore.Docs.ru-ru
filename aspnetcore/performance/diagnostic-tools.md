---
title: Средства диагностики производительности
author: mjrousos
description: Полезные инструменты для диагностики проблем с производительностью в приложениях ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 12/07/2018
uid: performance/diagnostic-tools
ms.openlocfilehash: 0fb9944d1082090f4f58f641a26ee96286920456
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210006"
---
# <a name="performance-diagnostic-tools"></a>Средства диагностики производительности

По [Майк Роусос](https://github.com/mjrousos)

В этой статье перечислены инструменты для выявления проблем с производительностью в ASP.NET Core.

## <a name="visual-studio-diagnostic-tools"></a>Средства диагностики Visual Studio

[Средства профилирования и диагностики](/visualstudio/profiling) интегрирован в Visual Studio — хорошее место для начала расследования проблем с производительностью. Эти средства являются мощным и удобным механизмом для использования в среде разработки Visual Studio. Инструментарий обеспечивает анализ использования ЦП, использование памяти и событий производительности в приложениях ASP.NET Core. Встроенные, делает легко профилирования во время разработки.

Дополнительные сведения можно найти в [документация по Visual Studio](/visualstudio/profiling/profiling-overview).

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/app-insights-overview) приводятся подробные данные производительности для вашего приложения. Application Insights автоматически собирает данные о скорость ответа, частота сбоев, время отклика зависимостей и многое другое. Application Insights поддерживает ведение журнала пользовательских событий и конкретных метрик для приложения.

Azure Application Insights предоставляет несколько способов предоставить insights на отслеживаемых приложений:

- [Схема сопоставления приложений](/azure/application-insights/app-insights-app-map) — помогает выявлять узкие или сбоя горячих точек всех компонентов распределенных приложений.
- [Колонке метрик на портале Application Insights](/azure/application-insights/app-insights-metrics-explorer?toc=/azure/azure-monitor/toc.json) показано измеряются значения и значения числа событий.
- [Колонку "производительность" на портале Application Insights](/azure/application-insights/app-insights-tutorial-performance):

  - Показывает сведения о производительности для различных операций в отслеживаемом приложении.
  - Позволяет детализации в единую операцию для проверки всех частей и зависимости, способствующих длительного времени.
  - Profiler может вызываться из здесь, чтобы собирать производительности трассировки по запросу.

- [Azure Application Insights Profiler](/azure/azure-monitor/app/profiler) позволяет обычных и по запросу профилирование приложений для .NET.  Azure на портале должна появиться записи трассировок производительности с помощью стеков вызовов и критических путей. Файлы трассировки можно также загрузить для более глубокого анализа с помощью PerfView.

Application Insights можно использовать в различных средах:

- Оптимизирована для работы в Azure.
- Работает в рабочей среде, разработки и размещения.
- Работает локально из [Visual Studio](/azure/application-insights/app-insights-visual-studio) или в других средах размещения.

Дополнительные сведения см. в статье [Application Insights для ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="perfview"></a>PerfView

[PerfView](https://github.com/Microsoft/perfview) представляет собой средство анализа производительности, созданные командой .NET, в частности, для диагностики проблем с производительностью .NET. PerfView обеспечивает анализ ЦП использования памяти и сборки Мусора поведение, событий производительности и время по часам.

Дополнительные сведения о PerfView и как приступить к работе с [видео учебники по PerfView](http://channel9.msdn.com/Series/PerfView-Tutorial) или посредством считывания руководства пользователя, доступных в средстве или [на сайте GitHub](https://github.com/Microsoft/perfview).

## <a name="windows-performance-toolkit"></a>Windows Performance Toolkit

[Windows Performance Toolkit](/windows-hardware/test/wpt/) (WPT) состоит из двух компонентов: Windows Performance Recorder (WPR) и Windows Performance Analyzer (WPA). Средства создают подробные производительности профили операционных систем Windows и приложений. WPT имеет более широкие способы визуализации данных, но сбор данных обладает меньшими возможностями, чем в PerfView.

## <a name="perfcollect"></a>PerfCollect

Хотя PerfView представляет собой средство анализа производительности полезно для сценариев .NET, оно работает только в Windows, поэтому его нельзя использовать для сбора данных трассировки из приложений ASP.NET Core, запущенных в средах Linux.

[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) — это сценарий bash, который использует собственный Linux, средства профилирования ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page) и [LTTng](https://lttng.org/)) для сбора данных трассировки в Linux, можно анализировать по PerfView. PerfCollect полезно в тех случаях, когда проблемы с производительностью отображаются в средах Linux, где PerfView не может использоваться непосредственно. Вместо этого PerfCollect можно собирать трассировки из приложений .NET Core, которые затем анализируются на компьютере Windows с помощью PerfView.

Дополнительные сведения о том, как установить и приступить к работе с PerfCollect [на сайте GitHub](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).

## <a name="other-third-party-performance-tools"></a>Другие средства сторонних производительности

Ниже перечислены некоторые средства повышения производительности независимых производителей, которые полезны в исследования производительности приложений .NET Core.

- [MiniProfiler](https://miniprofiler.com/)
- dotTrace и dotMemory от компании JetBrains
- VTune корпорации Intel
