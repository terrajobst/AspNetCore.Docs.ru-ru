---
uid: web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
title: Новые возможности в ASP.NET Web Pages 3.2 | Документы Microsoft
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/30/2014
ms.topic: article
ms.assetid: a652beff-8e6b-48ad-bfe4-3703f7ccf0a5
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
msc.type: authoredcontent
ms.openlocfilehash: 80421018e0508d430b6142cd3cee1727d1d17b7e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30896398"
---
<a name="whats-new-in-aspnet-web-pages-32"></a>Новые возможности веб-страниц ASP.NET версии 3.2
====================
по [Microsoft](https://github.com/microsoft)

В этом разделе описаны новые 3.2 веб-страниц ASP.NET, веб-страницы 3.2.2 и [веб-страницы 3.2.3 бета-версия 1](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)

## <a name="aspnet-web-pages-32"></a>Веб-страниц ASP.NET версии 3.2

Этот выпуск исправляет ошибку и содержит новую возможность.

## <a name="download"></a>Скачать

Компоненты среды выполнения выпущены в виде пакетов NuGet при галереи NuGet. Выполните все пакеты среды выполнения [семантического управления версиями](http://semver.org/) спецификации. Пакет 3.2 веб-страниц ASP.NET имеет следующую версию: &ldquo;3.2.0&rdquo;. Можно установить или обновить эти пакеты через [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebPages/). Этот выпуск также включает соответствующие локализованные пакеты в NuGet.

Можно установить или обновить выпущенных пакетов NuGet, используя консоль диспетчера пакетов NuGet:

[!code-console[Main](whats-new-in-aspnet-web-pages-32/samples/sample1.cmd)]

## <a name="new-feature-and-bug-fix"></a>Новые функции и исправления ошибки

Мы фиксированной одну ошибку и его расширение один дополнительный компонент в этой версии. Запрос можно найти одним и тем же [здесь](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC|v5.2%20RTM&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=Id&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed).

## <a name="aspnet-web-pages-322"></a>Веб-страниц ASP.NET 3.2.2

Этот выпуск передача вверх изменения в [ASP.NET Web Pages 3.2.1 бета-версии](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) которой обеспечивает значительное улучшение производительности в отрисовки страниц больших razor. В разделе[проблема Codeplex 585](https://aspnetwebstack.codeplex.com/workitem/585). Этот выпуск выравнивается MVC 5.2.2 пакеты, которые теперь будет зависеть от этой версии.

Мы работали с группой MSN визуализации больших страниц. Если страницы отображают более 80 объем данных в килобайтах, мы получим объектов в куче больших объектов. При использовании нескольких слоев макеты умножается этот эффект.

Результат на сервере — дополнительных ЦП, памяти и даже долго более длительного хранения приостанавливает во время [очистки Gen 2](https://msdn.microsoft.com/en-us/library/ms973837.aspx) в сборщике мусора.

Ниже приведена таблица демонстрации результатов анализа [perfview](https://channel9.msdn.com/Series/PerfView-Tutorial) для запуска. Удерживается константой около 68%, ЦП, а большие страницы подготавливаются. В таблице показаны почти полностью устранить номер поколения 2, что результатом является более высокому тарифу запроса и существенное снижение приостанавливает свою работу из-за сборки мусора.

| **Область** | **Прежде чем (3.2)** | **После (3.2.1)** | **Дельта %** |
| --- | --- | --- | --- |
| Всего запроса (число) | 26,986 | 32,591 | <font style="background-color: #4bacc6">20.80%</font> |
| Время трассировки (в секундах) | 196.20 | 198.60 | 1.20% |
| Запросов/сек | 137.53 | 164.10 | <font style="background-color: #4bacc6">19.30%</font> |
| Загрузка ЦП | 68.80% | 68.50% |  -0.40% |
| Образцы ЦП GC | 124,323 | 17,543 | <font style="background-color: #4bacc6">-85.90%</font> |
| Общие выделения (число) | 55,357,146 | 57,222,949 | 3.40% |
| Приостановка общего сборки Мусора (примеры) | 15,091 | 8,515 | <font style="background-color: #4bacc6">-43.60%</font> |
| Сборка Мусора Gen0 (число) | 403 | 1,216 | 201.70% |
| Сборка Мусора Gen1 (число) | 290 | 367 | 26.60% |
| Сборка Мусора Gen2 (число) | 229 | 2 | <font style="background-color: #00ff00">-99.10%</font> |
| ЦП / запроса (образцы req) | 19.73 | 16.47 | -16.50% |

| Выделение цветом: | <font style="background-color: #00ff00">Улучшения ядра</font> | <font style="background-color: #4bacc6">Положительное влияние на производительность</font> |
|---------------|-----------------------------------------------------------------|-------------------------------------------------------------------------------|
|               |                                                                 |                                                                               |

## <a name="aspnet-web-pages-323-beta1"></a>ASP.NET Web Pages 3.2.3 бета-версия 1

Этот выпуск содержит только исправления ошибок. Можно использовать [этот запрос](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) Чтобы просмотреть список проблем, исправленных в этом выпуске.
