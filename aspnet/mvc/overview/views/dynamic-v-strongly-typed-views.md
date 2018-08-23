---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Динамические и Строго типизированные представления | Документация Майкрософт
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/27/2011
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 41a47f0f6e5fd900e8bf35c37ed98566fe416ace
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828651"
---
<a name="dynamic-v-strongly-typed-views"></a>Динамические и Строго типизированные представления
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

Существует три способа для передачи данных из контроллера в представление в ASP.NET MVC 3:

1. В виде строго типизированной модели объекта.
2. Как динамический тип (с помощью @model динамические)
3. Использование ViewBag

Я написал простое приложение MVC 3 верхней блога для сравнения и сравните динамические и строго типизированные представления. Контроллер начинается с простой список блогов:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

Щелкните правой кнопкой мыши в методе IndexNotStonglyTyped() и добавить представление Razor.

[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)

Убедитесь, что **создать строго типизированное представление** флажок не установлен. Результирующее представление не содержат множество:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

Поскольку мы используем динамический и не строго типизированного представления, intellisense не помогают. Ниже приведен полный код:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)

Теперь мы добавим строго типизированного представления. Добавьте следующий код в контроллер:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


Обратите внимание, что это точно тем же возвращаемое View(topBlogs); вызывать как не строго типизированного представления. Щелкните правой кнопкой мыши внутри элемента *StonglyTypedIndex()* и выберите **Добавление представления**. На этот раз выберите **блог** класс модели и выберите **списка** как шаблон каркаса.

[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)

В новый шаблон представления мы обеспечить поддержку intellisense.

[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)

Можно загрузить проект c# [здесь](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).
