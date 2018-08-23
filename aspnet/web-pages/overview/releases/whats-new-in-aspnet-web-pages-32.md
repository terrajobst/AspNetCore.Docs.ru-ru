---
uid: web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
title: Новые возможности в ASP.NET Web Pages 3.2 | Документация Майкрософт
author: microsoft
description: ''
ms.author: riande
ms.date: 06/30/2014
ms.assetid: a652beff-8e6b-48ad-bfe4-3703f7ccf0a5
msc.legacyurl: /web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
msc.type: authoredcontent
ms.openlocfilehash: 31b462a3116b29770f534fb95b69a29ae665487c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828764"
---
<a name="whats-new-in-aspnet-web-pages-32"></a>Новые возможности в веб-страниц ASP.NET 3.2
====================
по [Microsoft](https://github.com/microsoft)

В этом разделе описываются новые возможности для веб-страниц ASP.NET 3.2, веб-страниц 3.2.2 и [веб-страницы 3.2.3 бета-версии 1](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)

## <a name="aspnet-web-pages-32"></a>Веб-страниц ASP.NET 3.2

Этот выпуск исправляет ошибку и вводит новую возможность.

## <a name="download"></a>Скачать

Компоненты среды выполнения выпускаются в виде пакетов NuGet из коллекции NuGet. Выполните все пакеты среды выполнения [семантического управления версиями](http://semver.org/) спецификации. Пакет веб-страниц ASP.NET 3.2 имеет следующую версию: &ldquo;3.2.0&rdquo;. Вы можете установить или обновить эти пакеты с помощью [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebPages/). Этот выпуск также включает соответствующие локализованные пакеты в NuGet.

Можно установить или обновить на выпущенных пакетов NuGet с помощью консоли диспетчера пакетов NuGet:

[!code-console[Main](whats-new-in-aspnet-web-pages-32/samples/sample1.cmd)]

## <a name="new-feature-and-bug-fix"></a>Новая функция и исправление ошибки

Мы фиксированной одну вашу ошибку и внесенных одним незначительные улучшений в этом выпуске. Вы можете найти запрос, для того же [здесь](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC|v5.2%20RTM&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=Id&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed).

## <a name="aspnet-web-pages-322"></a>ASP.NET Web Pages 3.2.2

Этот выпуск сводное изменения [выпуска бета-версии ASP.NET Web Pages 3.2.1](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) который обеспечивает значительное улучшение производительности при отрисовке больших razor pages. См. в разделе[проблема Codeplex 585](https://aspnetwebstack.codeplex.com/workitem/585). Этот выпуск соответствует MVC 5.2.2 пакеты, которые теперь будет зависеть от этой версии.

Мы сотрудничаем с группой MSN над визуализацией больших страниц. При более чем 80 килобайт данных отображения страницы, мы получим объектов в куче больших объектов. При использовании нескольких уровней макеты умножается этот эффект.

Результат на сервере — дополнительные ЦП, длительное хранение в памяти и даже long приостанавливает работу во время [очистки Gen 2](https://msdn.microsoft.com/en-us/library/ms973837.aspx) сборщика мусора.

Ниже приведена таблица, демонстрирующий результаты анализа [perfview](https://channel9.msdn.com/Series/PerfView-Tutorial) для выполнения. Удерживается константы около 68%, ЦП, хотя большие страницы, отображаются. В таблице показаны почти полностью исключен номер поколения 2, что результатом является более высокому тарифу запроса и значительное снижение приостанавливается из-за сборки мусора.

| **Область** | **Прежде чем (3.2)** | **После (3.2.1)** | **% Изменений** |
| --- | --- | --- | --- |
| Общее число запросов (количество) | 26,986 | 32,591 | <font style="background-color: #4bacc6">20.80%</font> |
| Продолжительность трассировки (в секундах) | 196.20 | 198.60 | 1.20% |
| Запросов в секунду | 137.53 | 164.10 | <font style="background-color: #4bacc6">19.30%</font> |
| Загрузка ЦП | 68.80% | 68.50% |  -0.40% |
| Примеры ЦП сборки Мусора | 124,323 | 17,543 | <font style="background-color: #4bacc6">-85.90%</font> |
| Общий объем выделенной памяти (количество) | 55,357,146 | 57,222,949 | 3.40% |
| Общее GC Pause (примеры) | 15,091 | 8,515 | <font style="background-color: #4bacc6">-43.60%</font> |
| Сборщик Мусора Gen0 (количество) | 403 | 1,216 | 201.70% |
| Поколение 1 GC (количество) | 290 | 367 | 26.60% |
| Сборки Мусора поколения 2 (количество) | 229 | 2 | <font style="background-color: #00ff00">-99.10%</font> |
| ЦП / запроса (примеры/req) | 19.73 | 16.47 | -16.50% |

| Цветовое выделение синтаксиса: | <font style="background-color: #00ff00">Улучшения ядра</font> | <font style="background-color: #4bacc6">Положительное влияние на производительность</font> |
|---------------|-----------------------------------------------------------------|-------------------------------------------------------------------------------|
|               |                                                                 |                                                                               |

## <a name="aspnet-web-pages-323-beta1"></a>Веб-страницы 3.2.3 ASP.NET бета-версии 1

Этот выпуск содержит только исправления ошибок. Можно использовать [этот запрос](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) для просмотра списка проблем, исправленных в этом выпуске.
