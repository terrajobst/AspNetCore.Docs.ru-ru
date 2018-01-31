---
title: "Методы и представления контроллера"
author: rick-anderson
description: "Работа с методами, представлениями контроллера и DataAnnotations"
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 200f02f9815966653b3b46918737c60d11f11d5a
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="controller-methods-and-views"></a><span data-ttu-id="c0e37-103">Методы и представления контроллера</span><span class="sxs-lookup"><span data-stu-id="c0e37-103">Controller methods and views</span></span>

<span data-ttu-id="c0e37-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="c0e37-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c0e37-105">Все готово для приложения по работе с фильмами, но презентация далеко не идеальна.</span><span class="sxs-lookup"><span data-stu-id="c0e37-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="c0e37-106">Мы не хотим видеть время (12:00:00 AM на рисунке ниже), а заголовок **ReleaseDate** (ДатаВыпуска) должен состоять из двух слов.</span><span class="sxs-lookup"><span data-stu-id="c0e37-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Представление индекса: заголовок "Release Date" (Дата выпуска) состоит из одного слова (без пробела), и рядом с каждой датой отображается время "12:00 AM".](working-with-sql/_static/m55.png)

<span data-ttu-id="c0e37-108">Откройте файл *Models/Movie.cs* и добавьте указанные ниже выделенные строки:</span><span class="sxs-lookup"><span data-stu-id="c0e37-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

<span data-ttu-id="c0e37-109">Щелкните правой кнопкой мыши строку, подчеркнутую волнистой красной линией, и выберите **> Быстрые действия и операции рефакторинга**.</span><span class="sxs-lookup"><span data-stu-id="c0e37-109">Right click on a red squiggly line **> Quick Actions and Refactorings**.</span></span>

  ![Контекстное меню с пунктом **Быстрые действия и рефакторинг**.](controller-methods-views/_static/qa.png)


<span data-ttu-id="c0e37-111">Выберите `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="c0e37-111">Tap `using System.ComponentModel.DataAnnotations;`</span></span>

  ![using System.ComponentModel.DataAnnotations в верхней части списка.](controller-methods-views/_static/da.png)

  <span data-ttu-id="c0e37-113">Visual Studio добавит `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="c0e37-113">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="c0e37-114">Удалим операторы `using`, которые не требуются.</span><span class="sxs-lookup"><span data-stu-id="c0e37-114">Let's remove the `using` statements that are not needed.</span></span> <span data-ttu-id="c0e37-115">По умолчанию они отображаются светло-серым шрифтом.</span><span class="sxs-lookup"><span data-stu-id="c0e37-115">They show up by default in a light grey font.</span></span> <span data-ttu-id="c0e37-116">Щелкните правой кнопкой мыши файл *Movie.cs* и выберите **> Удалить и сортировать операторы using**.</span><span class="sxs-lookup"><span data-stu-id="c0e37-116">Right click anywhere in the *Movie.cs* file **> Remove and Sort Usings**.</span></span>

![Удалить и сортировать операторы using](controller-methods-views/_static/rm.png)

<span data-ttu-id="c0e37-118">Обновленный код выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="c0e37-118">The updated code:</span></span>

[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="c0e37-119">[Назад](working-with-sql.md)
[Вперед](search.md)</span><span class="sxs-lookup"><span data-stu-id="c0e37-119">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
