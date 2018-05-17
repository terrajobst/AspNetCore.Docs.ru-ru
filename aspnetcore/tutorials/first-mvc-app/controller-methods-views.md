---
title: Методы и представления контроллера в приложении ASP.NET Core
author: rick-anderson
description: Узнайте, как работать с методами, представлениями и DataAnnotations контроллера в ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 6fe0a0e71079bebcbd3a76abee0f2917f562e766
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="controller-methods-and-views-in-aspnet-core"></a><span data-ttu-id="584d9-103">Методы и представления контроллера в приложении ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="584d9-103">Controller methods and views in ASP.NET Core</span></span>

<span data-ttu-id="584d9-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="584d9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="584d9-105">Все готово для приложения по работе с фильмами, но презентация далеко не идеальна.</span><span class="sxs-lookup"><span data-stu-id="584d9-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="584d9-106">Мы не хотим видеть время (12:00:00 AM на рисунке ниже), а заголовок **ReleaseDate** (ДатаВыпуска) должен состоять из двух слов.</span><span class="sxs-lookup"><span data-stu-id="584d9-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Представление индекса: заголовок "Release Date" (Дата выпуска) состоит из одного слова (без пробела), и рядом с каждой датой отображается время "12:00 AM".](working-with-sql/_static/m55.png)

<span data-ttu-id="584d9-108">Откройте файл *Models/Movie.cs* и добавьте указанные ниже выделенные строки:</span><span class="sxs-lookup"><span data-stu-id="584d9-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

<span data-ttu-id="584d9-109">Щелкните правой кнопкой мыши строку, подчеркнутую волнистой красной линией, и выберите **> Быстрые действия и операции рефакторинга**.</span><span class="sxs-lookup"><span data-stu-id="584d9-109">Right click on a red squiggly line **> Quick Actions and Refactorings**.</span></span>

  ![Контекстное меню с пунктом **Быстрые действия и рефакторинг**.](controller-methods-views/_static/qa.png)


<span data-ttu-id="584d9-111">Выберите `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="584d9-111">Tap `using System.ComponentModel.DataAnnotations;`</span></span>

  ![using System.ComponentModel.DataAnnotations в верхней части списка.](controller-methods-views/_static/da.png)

  <span data-ttu-id="584d9-113">Visual Studio добавит `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="584d9-113">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="584d9-114">Удалим операторы `using`, которые не требуются.</span><span class="sxs-lookup"><span data-stu-id="584d9-114">Let's remove the `using` statements that are not needed.</span></span> <span data-ttu-id="584d9-115">По умолчанию они отображаются светло-серым шрифтом.</span><span class="sxs-lookup"><span data-stu-id="584d9-115">They show up by default in a light grey font.</span></span> <span data-ttu-id="584d9-116">Щелкните правой кнопкой мыши файл *Movie.cs* и выберите **> Удалить и сортировать операторы using**.</span><span class="sxs-lookup"><span data-stu-id="584d9-116">Right click anywhere in the *Movie.cs* file **> Remove and Sort Usings**.</span></span>

![Удалить и сортировать операторы using](controller-methods-views/_static/rm.png)

<span data-ttu-id="584d9-118">Обновленный код выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="584d9-118">The updated code:</span></span>

[!code-csharp[](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE [adding-model](../../includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="584d9-119">[Назад](working-with-sql.md)
> [Вперед](search.md)</span><span class="sxs-lookup"><span data-stu-id="584d9-119">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
