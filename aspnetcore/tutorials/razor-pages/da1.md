---
title: Изменение созданных страниц в приложении ASP.NET Core
author: rick-anderson
description: Сведения об изменении созданных страниц в приложении ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 7633c0a40764cc18a656f0497e3280e4067cb59f
ms.sourcegitcommit: 317f9be24db600499e79d25872d743af74bd86c0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045579"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="013f8-103">Изменение созданных страниц в приложении ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="013f8-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="013f8-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="013f8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="013f8-105">Все готово для приложения по работе с фильмами, но презентация далеко не идеальна.</span><span class="sxs-lookup"><span data-stu-id="013f8-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="013f8-106">Мы не хотим видеть время (12:00:00 AM на рисунке ниже), а заголовок **ReleaseDate** (ДатаВыпуска) должен состоять из двух слов: **Release Date**.</span><span class="sxs-lookup"><span data-stu-id="013f8-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![Приложение Movie с данными по фильмам, открытое в Chrome](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="013f8-108">Обновление созданного кода</span><span class="sxs-lookup"><span data-stu-id="013f8-108">Update the generated code</span></span>

<span data-ttu-id="013f8-109">Откройте файл *Models/Movie.cs* и добавьте указанные ниже выделенные строки кода:</span><span class="sxs-lookup"><span data-stu-id="013f8-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]

::: moniker-end

<span data-ttu-id="013f8-110">Щелкните правой кнопкой мыши строку, подчеркнутую волнистой красной линией, и выберите пункт **Быстрые действия и рефакторинг**.</span><span class="sxs-lookup"><span data-stu-id="013f8-110">Right click on a red squiggly line > **Quick Actions and Refactorings**.</span></span>

  ![Контекстное меню с пунктом **Быстрые действия и рефакторинг**.](da1/qa.png)

<span data-ttu-id="013f8-112">Выберите `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="013f8-112">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![using System.ComponentModel.DataAnnotations в верхней части списка.](da1/da.png)

  <span data-ttu-id="013f8-114">Visual Studio добавит `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="013f8-114">Visual Studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

[!INCLUDE [model1](~/includes/RP/da2.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="013f8-115">[Предыдущая статья: "Работа с SQL Server LocalDB"](xref:tutorials/razor-pages/sql)
> [Добавление поиска](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="013f8-115">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Add search](xref:tutorials/razor-pages/search)</span></span>
