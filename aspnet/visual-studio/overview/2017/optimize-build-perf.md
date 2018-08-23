---
uid: visual-studio/overview/2017/optimize-build-perf
title: Оптимизировать производительность сборки для решения
author: tfitzmac
description: Оптимизировать производительность сборки для решения
ms.author: riande
ms.date: 08/22/2018
msc.type: authoredcontent
ms.openlocfilehash: 19f190835e7477e69db470b74edac9e211fd9158
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/22/2018
ms.locfileid: "41909947"
---
# <a name="optimize-build-performance-for-solution"></a>Оптимизировать производительность сборки для решения
Visual Studio 2017 15.8 и позже добавить новый элемент меню в разделе **сборки > компиляции ASP.NET > Оптимизация производительности сборки для решения**.

![Снимок экрана нового пункта меню](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

ASP.NET компилирует его представлений во время выполнения, это означает, что проект ASP.NET несет с собой копию компилятор. Тем не менее на компьютере разработчика при копию компилятор не соответствует копии Visual Studio, производительность построения это повлияет на порядка 1 – 3 секунды каждой добавочной сборки. Эта функция будет обновить копию проекта компилятора в соответствии с Visual Studio которая должна ускорить выполнение инкрементные сборки.

Это применимо только в проекты ASP.NET Framework, он не применяется к ASP.NET Core.
