---
title: "Методы и представления контроллера"
author: rick-anderson
description: "Работа с методами, представлениями контроллера и DataAnnotations"
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.assetid: c7313211-b271-4adf-bab8-8e72603cc0ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 8960853438d3227de3ef7c50936626149d8d5997
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="controller-methods-and-views"></a><span data-ttu-id="13b9f-104">Методы и представления контроллера</span><span class="sxs-lookup"><span data-stu-id="13b9f-104">Controller methods and views</span></span>

<span data-ttu-id="13b9f-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="13b9f-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="13b9f-106">Все готово для приложения Movie, но презентация далеко не идеальна.</span><span class="sxs-lookup"><span data-stu-id="13b9f-106">We have a good start to the movie app, but the presentation is not ideal.</span></span> <span data-ttu-id="13b9f-107">Мы не хотим видеть время (12:00:00 AM на рисунке ниже), а заголовок **ReleaseDate** (ДатаВыпуска) должен состоять из двух слов.</span><span class="sxs-lookup"><span data-stu-id="13b9f-107">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Представление индекса: заголовок "Release Date" (Дата выпуска) состоит из одного слова (без пробела), и рядом с каждой датой отображается время "12:00 AM".](working-with-sql/_static/m55.png)

<span data-ttu-id="13b9f-109">Откройте файл *Models/Movie.cs* и добавьте указанные ниже выделенные строки:</span><span class="sxs-lookup"><span data-stu-id="13b9f-109">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

<span data-ttu-id="13b9f-110">Щелкните правой кнопкой мыши строку, подчеркнутую волнистой красной линией, и выберите **> Быстрые действия и операции рефакторинга**.</span><span class="sxs-lookup"><span data-stu-id="13b9f-110">Right click on a red squiggly line **> Quick Actions and Refactorings**.</span></span>

  ![Контекстное меню с пунктом **Быстрые действия и рефакторинг**.](controller-methods-views/_static/qa.png)


<span data-ttu-id="13b9f-112">Выберите `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="13b9f-112">Tap `using System.ComponentModel.DataAnnotations;`</span></span>

  ![using System.ComponentModel.DataAnnotations в верхней части списка.](controller-methods-views/_static/da.png)

  <span data-ttu-id="13b9f-114">Visual Studio добавит `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="13b9f-114">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="13b9f-115">Удалим операторы `using`, которые не требуются.</span><span class="sxs-lookup"><span data-stu-id="13b9f-115">Let's remove the `using` statements that are not needed.</span></span> <span data-ttu-id="13b9f-116">По умолчанию они отображаются светло-серым шрифтом.</span><span class="sxs-lookup"><span data-stu-id="13b9f-116">They show up by default in a light grey font.</span></span> <span data-ttu-id="13b9f-117">Щелкните правой кнопкой мыши файл *Movie.cs* и выберите **> Удалить и сортировать операторы using**.</span><span class="sxs-lookup"><span data-stu-id="13b9f-117">Right click anywhere in the *Movie.cs* file **> Remove and Sort Usings**.</span></span>

![Удалить и сортировать операторы using](controller-methods-views/_static/rm.png)

<span data-ttu-id="13b9f-119">Обновленный код выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="13b9f-119">The updated code:</span></span>

[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="13b9f-120">[Назад](working-with-sql.md)
[Вперед](search.md)</span><span class="sxs-lookup"><span data-stu-id="13b9f-120">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
