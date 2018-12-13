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
# <a name="performance-diagnostic-tools"></a><span data-ttu-id="f51c7-103">Средства диагностики производительности</span><span class="sxs-lookup"><span data-stu-id="f51c7-103">Performance Diagnostic Tools</span></span>

<span data-ttu-id="f51c7-104">По [Майк Роусос](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="f51c7-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="f51c7-105">В этой статье перечислены инструменты для выявления проблем с производительностью в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f51c7-105">This article lists tools for diagnosing performance issues in ASP.NET Core.</span></span>

## <a name="visual-studio-diagnostic-tools"></a><span data-ttu-id="f51c7-106">Средства диагностики Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f51c7-106">Visual Studio Diagnostic Tools</span></span>

<span data-ttu-id="f51c7-107">[Средства профилирования и диагностики](/visualstudio/profiling) интегрирован в Visual Studio — хорошее место для начала расследования проблем с производительностью.</span><span class="sxs-lookup"><span data-stu-id="f51c7-107">The [profiling and diagnostic tools](/visualstudio/profiling) built into Visual Studio are a good place to start investigating performance issues.</span></span> <span data-ttu-id="f51c7-108">Эти средства являются мощным и удобным механизмом для использования в среде разработки Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f51c7-108">These tools are powerful and convenient to use from the Visual Studio development environment.</span></span> <span data-ttu-id="f51c7-109">Инструментарий обеспечивает анализ использования ЦП, использование памяти и событий производительности в приложениях ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f51c7-109">The tooling allows analysis of CPU usage, memory usage, and performance events in ASP.NET Core apps.</span></span>

<span data-ttu-id="f51c7-110">Дополнительные сведения можно найти в [документация по Visual Studio](/visualstudio/profiling/profiling-overview).</span><span class="sxs-lookup"><span data-stu-id="f51c7-110">More information is available in [Visual Studio documentation](/visualstudio/profiling/profiling-overview).</span></span>

## <a name="application-insights"></a><span data-ttu-id="f51c7-111">Application Insights</span><span class="sxs-lookup"><span data-stu-id="f51c7-111">Application Insights</span></span>

<span data-ttu-id="f51c7-112">[Application Insights](/azure/application-insights/app-insights-overview) приводятся подробные данные производительности для вашего приложения.</span><span class="sxs-lookup"><span data-stu-id="f51c7-112">[Application Insights](/azure/application-insights/app-insights-overview) provides in-depth performance data for your app.</span></span> <span data-ttu-id="f51c7-113">Application Insights автоматически собирает данные о скорость ответа, частота сбоев, время отклика зависимостей и многое другое.</span><span class="sxs-lookup"><span data-stu-id="f51c7-113">Application Insights automatically collects data on response rates, failure rates, dependency response times, and more.</span></span> <span data-ttu-id="f51c7-114">Application Insights поддерживает ведение журнала пользовательских событий и конкретных метрик для приложения.</span><span class="sxs-lookup"><span data-stu-id="f51c7-114">Application Insights supports logging custom events and metrics specific to your app.</span></span>

<span data-ttu-id="f51c7-115">Application Insights можно использовать в различных средах:</span><span class="sxs-lookup"><span data-stu-id="f51c7-115">Application Insights can be used in a variety environments:</span></span>

* <span data-ttu-id="f51c7-116">Оптимизирована для работы в Azure.</span><span class="sxs-lookup"><span data-stu-id="f51c7-116">Optimized to work in Azure.</span></span>
* <span data-ttu-id="f51c7-117">Работает в рабочей среде, разработки и размещения.</span><span class="sxs-lookup"><span data-stu-id="f51c7-117">Works in production, development, and staging.</span></span>
* <span data-ttu-id="f51c7-118">Работает локально из [Visual Studio](/azure/application-insights/app-insights-visual-studio) или в других средах размещения.</span><span class="sxs-lookup"><span data-stu-id="f51c7-118">Works locally from [Visual Studio](/azure/application-insights/app-insights-visual-studio) or in other hosting environments.</span></span>

<span data-ttu-id="f51c7-119">Дополнительные сведения см. в статье [Application Insights для ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="f51c7-119">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="perfview"></a><span data-ttu-id="f51c7-120">PerfView</span><span class="sxs-lookup"><span data-stu-id="f51c7-120">PerfView</span></span>

<span data-ttu-id="f51c7-121">[PerfView](https://github.com/Microsoft/perfview) представляет собой средство анализа производительности, созданные командой .NET, в частности, для диагностики проблем с производительностью .NET.</span><span class="sxs-lookup"><span data-stu-id="f51c7-121">[PerfView](https://github.com/Microsoft/perfview) is a performance analysis tool created by the .NET team specifically for diagnosing .NET performance issues.</span></span> <span data-ttu-id="f51c7-122">PerfView обеспечивает анализ ЦП использования памяти и сборки Мусора поведение, событий производительности и время по часам.</span><span class="sxs-lookup"><span data-stu-id="f51c7-122">PerfView allows analysis of CPU usage, memory and GC behavior, performance events, and wall clock time.</span></span>

<span data-ttu-id="f51c7-123">Дополнительные сведения о PerfView и как приступить к работе с [видео учебники по PerfView](http://channel9.msdn.com/Series/PerfView-Tutorial) или посредством считывания руководства пользователя, доступных в средстве или [на сайте GitHub](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="f51c7-123">You can learn more about PerfView and how to get started with [PerfView video tutorials](http://channel9.msdn.com/Series/PerfView-Tutorial) or by reading the user's guide available in the tool or [on GitHub](https://github.com/Microsoft/perfview).</span></span>

## <a name="perfcollect"></a><span data-ttu-id="f51c7-124">PerfCollect</span><span class="sxs-lookup"><span data-stu-id="f51c7-124">PerfCollect</span></span>

<span data-ttu-id="f51c7-125">Хотя PerfView представляет собой средство анализа производительности полезно для сценариев .NET, оно работает только в Windows, поэтому его нельзя использовать для сбора данных трассировки из приложений ASP.NET Core, запущенных в средах Linux.</span><span class="sxs-lookup"><span data-stu-id="f51c7-125">While PerfView is a useful performance analysis tool for .NET scenarios, it only runs on Windows so you can't use it to collect traces from ASP.NET Core apps running in Linux environments.</span></span>

<span data-ttu-id="f51c7-126">[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) — это сценарий bash, который использует собственный Linux, средства профилирования ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page) и [LTTng](https://lttng.org/)) для сбора данных трассировки в Linux, можно анализировать по PerfView.</span><span class="sxs-lookup"><span data-stu-id="f51c7-126">[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) is a bash script that uses native Linux profiling tools ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page) and [LTTng](https://lttng.org/)) to collect traces on Linux that can be analyzed by PerfView.</span></span> <span data-ttu-id="f51c7-127">PerfCollect полезно в тех случаях, когда проблемы с производительностью отображаются в средах Linux, где PerfView не может использоваться непосредственно.</span><span class="sxs-lookup"><span data-stu-id="f51c7-127">PerfCollect is useful when performance problems show up in Linux environments where PerfView can't be used directly.</span></span> <span data-ttu-id="f51c7-128">Вместо этого PerfCollect можно собирать трассировки из приложений .NET Core, которые затем анализируются на компьютере Windows с помощью PerfView.</span><span class="sxs-lookup"><span data-stu-id="f51c7-128">Instead, PerfCollect can collect traces from .NET Core apps that are then analyzed on a Windows computer using PerfView.</span></span>

<span data-ttu-id="f51c7-129">Дополнительные сведения о том, как установить и приступить к работе с PerfCollect [на сайте GitHub](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).</span><span class="sxs-lookup"><span data-stu-id="f51c7-129">More information about how to install and get started with PerfCollect is available [on GitHub](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).</span></span>
