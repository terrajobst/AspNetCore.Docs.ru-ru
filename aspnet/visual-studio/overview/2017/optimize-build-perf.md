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
# <a name="optimize-build-performance-for-solution"></a>Оптимизировать производительность сборки для решения

Visual Studio 2017 15.8 или более поздних версий имеют элемент меню: **построения** > **компиляции ASP.NET** > **оптимизация производительности сборки для решения**.

![Снимок экрана нового пункта меню](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

ASP.NET компилирует его представлений во время выполнения, это означает, что проект ASP.NET несет с собой копию компилятор. Тем не менее на компьютере разработчика при копию компилятор не соответствует копии Visual Studio, сборки влияет на производительность порядка 1 – 3 секунды каждой добавочной сборки. Эта функция обновляет копию проекта компилятора в соответствии с Visual Studio, что обычно ускоряет инкрементные сборки.

**Это применимо к ASP.NET Framework 4.7.1 или более поздней версии только проекты, он не применяется к ASP.NET Core.**
