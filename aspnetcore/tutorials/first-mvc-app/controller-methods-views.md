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
ms.openlocfilehash: 3f84d242a41bc482110d87ff342fa5b5d8c870ff
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729857"
---
# <a name="controller-methods-and-views-in-aspnet-core"></a><span data-ttu-id="6b518-103">Методы и представления контроллера в приложении ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6b518-103">Controller methods and views in ASP.NET Core</span></span>

<span data-ttu-id="6b518-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="6b518-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6b518-105">Все готово для приложения по работе с фильмами, но презентация далеко не идеальна.</span><span class="sxs-lookup"><span data-stu-id="6b518-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="6b518-106">Мы не хотим видеть время (12:00:00 AM на рисунке ниже), а заголовок **ReleaseDate** (ДатаВыпуска) должен состоять из двух слов.</span><span class="sxs-lookup"><span data-stu-id="6b518-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Представление индекса: заголовок "Release Date" (Дата выпуска) состоит из одного слова (без пробела), и рядом с каждой датой отображается время "12:00 AM".](working-with-sql/_static/m55.png)

<span data-ttu-id="6b518-108">Откройте файл *Models/Movie.cs* и добавьте указанные ниже выделенные строки:</span><span class="sxs-lookup"><span data-stu-id="6b518-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="6b518-109">[!code-csharp[](start-mvc/sample/MvcMovie21/Models/MovieDateFixed.cs?name=snippet_1&highlight=2,3,12-13,17)]</span><span class="sxs-lookup"><span data-stu-id="6b518-109">[!code-csharp[](start-mvc/sample/MvcMovie21/Models/MovieDateFixed.cs?name=snippet_1&highlight=2,3,12-13,17)]</span></span>
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="6b518-110">[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="6b518-110">[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]</span></span>
::: moniker-end

[!INCLUDE [adding-model](~/includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="6b518-111">[Назад](working-with-sql.md)
> [Вперед](search.md)</span><span class="sxs-lookup"><span data-stu-id="6b518-111">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
