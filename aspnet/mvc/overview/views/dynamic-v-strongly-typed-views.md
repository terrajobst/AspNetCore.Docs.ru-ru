---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Динамические v. Строго типизированные представления | Документы Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2011
ms.topic: article
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 8a96d43e04a0a50d5176c10c26aa49918a0e56ef
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "26504093"
---
<a name="dynamic-v-strongly-typed-views"></a>Динамические v. Строго типизированные представления
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

Для передачи данных из контроллера в представление в ASP.NET MVC 3 тремя способами:

1. В виде строго типизированных модели объекта.
2. Как динамического типа (с помощью @model динамические)
3. Использование ViewBag

Я написал простого приложения MVC 3 верхней блога для сравнения и сравните динамические и строго типизированные представления. Контроллер начинается с простой список блогов:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

Щелкните правой кнопкой мыши в методе IndexNotStonglyTyped() и добавьте представления Razor.

[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)

Убедитесь, что **создать строго типизированное представление** флажок не установлен. Полученное представление не содержит много:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

Так как мы используем динамические и строго типизированного представления, технология intellisense не помогают. Ниже приведен полный код:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)

Теперь мы добавим строго типизированного представления. Добавьте следующий код на контроллер:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


Обратите внимание, что это именно же возвращаемого View(topBlogs); Вызовите как не строго типизированного представления. Щелкните правой кнопкой мыши внутри *StonglyTypedIndex()* и выберите **добавить представление**. На этот раз выберите **блог** класс модели и выберите **списка** как шаблон формирования.

[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)

В новый шаблон представления мы получить поддержку intellisense.

[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)

Можно загрузить проект c# [здесь](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).
