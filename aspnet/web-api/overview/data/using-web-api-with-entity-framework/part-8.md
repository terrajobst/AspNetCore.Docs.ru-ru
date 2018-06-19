---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Отобразить сведения об элементе | Документы Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 94863e94f2a8b3f1ce8a8fb85d877bc0768f3d8a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868091"
---
<a name="display-item-details"></a>Отобразить сведения о элемента
====================
по [Mike Wasson](https://github.com/MikeWasson)

[Загрузка завершенного проекта](https://github.com/MikeWasson/BookService)

В этом разделе будет добавлена возможность просмотра сведений для каждой книги. В файле app.js добавьте следующий код для модели представления:

[!code-javascript[Main](part-8/samples/sample1.js)]

В Views/Home/Index.cshtml добавьте элемент привязки данных ссылку сведения:

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

Данный код привязывает обработчик щелчка для &lt;&gt; элемент `getBookDetail` функции в модели представления.

В том же файле Замените следующие комментариев:

[!code-html[Main](part-8/samples/sample3.html)]

следующим кодом:

[!code-html[Main](part-8/samples/sample4.html)]

Эта разметка создает таблицу, которая является привязкой к данным свойствам `detail` наблюдаемый в модели представления.

«&lt;!--Ko--&gt; &quot; синтаксис позволяет включить привязку маскирования вне элемента DOM. В этом случае `if` привязка передает в этом разделе разметки для отображения только если `details` отлично от null.

[!code-html[Main](part-8/samples/sample5.html)]

Теперь, если запустить приложение и выберите один из &quot;сведений&quot; ссылки приложения будут выведены подробности книги.

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> [Назад](part-7.md)
> [Вперед](part-9.md)
