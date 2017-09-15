---
title: "Методы и представления контроллера"
author: rick-anderson
description: "Работа с методами, представлениями контроллера и DataAnnotations"
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: c7313211-bb71-4adf-babe-8e72603cc0ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/controller-methods-views
ms.openlocfilehash: 87516295c6e8c49b492312afda80c92c968e778b
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="controller-methods-and-views"></a><span data-ttu-id="a4bd0-104">Методы и представления контроллера</span><span class="sxs-lookup"><span data-stu-id="a4bd0-104">Controller methods and views</span></span>

<span data-ttu-id="a4bd0-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="a4bd0-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a4bd0-106">Все готово для приложения Movie, но презентация далеко не идеальна.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-106">We have a good start to the movie app, but the presentation is not ideal.</span></span> <span data-ttu-id="a4bd0-107">Мы не хотим видеть время (12:00:00 AM на рисунке ниже), а заголовок **ReleaseDate** (ДатаВыпуска) должен состоять из двух слов.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-107">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Представление индекса: заголовок "Release Date" (Дата выпуска) состоит из одного слова (без пробела), и рядом с каждой датой отображается время "12:00 AM".](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

<span data-ttu-id="a4bd0-109">Откройте файл *Models/Movie.cs* и добавьте указанные ниже выделенные строки:</span><span class="sxs-lookup"><span data-stu-id="a4bd0-109">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

<span data-ttu-id="a4bd0-110">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]</span><span class="sxs-lookup"><span data-stu-id="a4bd0-110">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]</span></span>

<span data-ttu-id="a4bd0-111">Выполните сборку и запуск приложения.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-111">Build and run the app.</span></span>

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="a4bd0-112">[Предыдущая статья — "Работа с SQLite"](working-with-sql.md)
[Следующая статья — "Добавление поиска"](search.md)</span><span class="sxs-lookup"><span data-stu-id="a4bd0-112">[Previous - Working with SQLite](working-with-sql.md)
[Next - Add search](search.md)</span></span>  