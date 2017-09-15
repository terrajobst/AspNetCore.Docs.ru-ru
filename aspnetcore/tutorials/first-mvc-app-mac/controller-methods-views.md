---
title: "Методы и представления контроллера в приложении ASP.NET Core MVC"
author: rick-anderson
description: "Работа с методами, представлениями контроллера и DataAnnotations"
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: c7313211-babe-babe-babe-8e72603cc0ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/controller-methods-views
ms.openlocfilehash: 846a78a09cdde0cfdbcf35bb899c1822ebe8fea2
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="controller-methods-and-views-in-an-aspnet-core-mvc-app"></a>Методы и представления контроллера в приложении ASP.NET Core MVC

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Все готово для приложения Movie, но презентация далеко не идеальна. Мы не хотим видеть время (12:00:00 AM на рисунке ниже), а заголовок **ReleaseDate** (ДатаВыпуска) должен состоять из двух слов.

![Представление индекса: заголовок "Release Date" (Дата выпуска) состоит из одного слова (без пробела), и рядом с каждой датой отображается время "12:00 AM".](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

Откройте файл *Models/Movie.cs* и добавьте указанные ниже выделенные строки:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

Выполните сборку и запуск приложения.

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
[Предыдущая статья — "Работа с SQLite"](working-with-sql.md)
[Следующая статья — "Добавление поиска"](search.md)
