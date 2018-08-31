---
uid: visual-studio/overview/2017/optimize-build-perf
title: Оптимизировать производительность сборки для решения
author: AngelosP
description: Оптимизировать производительность сборки для решения
ms.author: riande
ms.date: 08/29/2018
msc.type: authoredcontent
ms.openlocfilehash: c1a5cf5e59374b4c0dd7150c5dd62fbde42af555
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312145"
---
# <a name="optimize-build-performance-for-solution"></a><span data-ttu-id="a01cc-103">Оптимизировать производительность сборки для решения</span><span class="sxs-lookup"><span data-stu-id="a01cc-103">Optimize build performance for solution</span></span>

<span data-ttu-id="a01cc-104">Visual Studio 2017 15.8 или более поздних версий имеют элемент меню: **построения** > **компиляции ASP.NET** > **оптимизация производительности сборки для решения**.</span><span class="sxs-lookup"><span data-stu-id="a01cc-104">Visual Studio 2017 15.8 or later include a menu item: **Build** > **ASP.NET Compilation** > **Optimize Build Performance for Solution**.</span></span>

![Снимок экрана нового пункта меню](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

<span data-ttu-id="a01cc-106">ASP.NET компилирует его представлений во время выполнения, это означает, что проект ASP.NET несет с собой копию компилятор.</span><span class="sxs-lookup"><span data-stu-id="a01cc-106">ASP.NET compiles its views at runtime, which means that an ASP.NET project carries with it a copy of the compiler.</span></span> <span data-ttu-id="a01cc-107">Тем не менее на компьютере разработчика при копию компилятор не соответствует копии Visual Studio, сборки влияет на производительность порядка 1 – 3 секунды каждой добавочной сборки.</span><span class="sxs-lookup"><span data-stu-id="a01cc-107">However on a developer machine when the copy of the compiler doesn't match Visual Studio's copy, build performance is impacted on the order of 1-3 seconds per incremental build.</span></span> <span data-ttu-id="a01cc-108">Эта функция обновляет копию проекта компилятора в соответствии с Visual Studio, что обычно ускоряет инкрементные сборки.</span><span class="sxs-lookup"><span data-stu-id="a01cc-108">This feature updates your project's copy of the compiler to match Visual Studio's, which usually speeds up incremental builds.</span></span>

<span data-ttu-id="a01cc-109">**Это применимо к ASP.NET Framework 4.7.1 или более поздней версии только проекты, он не применяется к ASP.NET Core.**</span><span class="sxs-lookup"><span data-stu-id="a01cc-109">**This is applicable to ASP.NET Framework 4.7.1 or later projects only, it does not apply to ASP.NET Core.**</span></span>
