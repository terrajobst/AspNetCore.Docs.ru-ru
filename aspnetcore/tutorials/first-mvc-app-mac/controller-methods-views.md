---
title: Методы и представления контроллера в приложении ASP.NET Core MVC
author: rick-anderson
description: Работа с методами, представлениями контроллера и DataAnnotations
manager: wpickett
ms.author: riande
ms.date: 04/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-mac/controller-methods-views
ms.openlocfilehash: 7a6a965d99742e7e06e6da82999dc60264cac6c8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30897882"
---
# <a name="controller-methods-and-views-in-an-aspnet-core-mvc-app"></a><span data-ttu-id="b3bf3-103">Методы и представления контроллера в приложении ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="b3bf3-103">Controller methods and views in an ASP.NET Core MVC app</span></span>

<span data-ttu-id="b3bf3-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="b3bf3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b3bf3-105">Все готово для приложения по работе с фильмами, но презентация далеко не идеальна.</span><span class="sxs-lookup"><span data-stu-id="b3bf3-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="b3bf3-106">Мы не хотим видеть время (12:00:00 AM на рисунке ниже), а заголовок **ReleaseDate** (ДатаВыпуска) должен состоять из двух слов.</span><span class="sxs-lookup"><span data-stu-id="b3bf3-106">We don't want to see the time (12:00:00 AM in the following image) and **ReleaseDate** should be two words.</span></span>

![Представление индекса: заголовок "Release Date" (Дата выпуска) состоит из одного слова (без пробела), и рядом с каждой датой отображается время "12:00 AM".](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

<span data-ttu-id="b3bf3-108">Откройте файл *Models/Movie.cs* и добавьте указанные ниже выделенные строки:</span><span class="sxs-lookup"><span data-stu-id="b3bf3-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

<span data-ttu-id="b3bf3-109">Выполните сборку и запуск приложения.</span><span class="sxs-lookup"><span data-stu-id="b3bf3-109">Build and run the app.</span></span>

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE [adding-model](../../includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="b3bf3-110">[Предыдущая статья — "Работа с SQLite"](working-with-sql.md)
> [Следующая статья — "Добавление поиска"](search.md)</span><span class="sxs-lookup"><span data-stu-id="b3bf3-110">[Previous - Working with SQLite](working-with-sql.md)
[Next - Add search](search.md)</span></span>
