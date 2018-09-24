---
title: Изменение созданных страниц в приложении ASP.NET Core
author: rick-anderson
description: Сведения об изменении созданных страниц в приложении ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 78490be1cfa3018c465cb1e8125918404a7e4525
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011616"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="4e5d3-103">Изменение созданных страниц в приложении ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4e5d3-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="4e5d3-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="4e5d3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4e5d3-105">Все готово для приложения по работе с фильмами, но презентация далеко не идеальна.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="4e5d3-106">Мы не хотим видеть время (12:00:00 AM на рисунке ниже), а заголовок **ReleaseDate** (ДатаВыпуска) должен состоять из двух слов: **Release Date**.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![Приложение Movie с данными по фильмам, открытое в Chrome](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="4e5d3-108">Обновление созданного кода</span><span class="sxs-lookup"><span data-stu-id="4e5d3-108">Update the generated code</span></span>

<span data-ttu-id="4e5d3-109">Откройте файл *Models/Movie.cs* и добавьте указанные ниже выделенные строки кода:</span><span class="sxs-lookup"><span data-stu-id="4e5d3-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]

::: moniker-end

<span data-ttu-id="4e5d3-110">Щелкните правой кнопкой мыши строку, подчеркнутую волнистой красной линией, и выберите пункт **Быстрые действия и рефакторинг**.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-110">Right click on a red squiggly line > **Quick Actions and Refactorings**.</span></span>

  ![Контекстное меню с пунктом **Быстрые действия и рефакторинг**.](da1/qa.png)

<span data-ttu-id="4e5d3-112">Выберите `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-112">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![using System.ComponentModel.DataAnnotations в верхней части списка.](da1/da.png)

  <span data-ttu-id="4e5d3-114">Visual Studio добавит `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="4e5d3-114">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

[!INCLUDE [model1](~/includes/RP/da2.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="4e5d3-115">[Предыдущая статья: "Работа с SQL Server LocalDB"](xref:tutorials/razor-pages/sql)
> [Добавление поиска](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="4e5d3-115">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Add search](xref:tutorials/razor-pages/search)</span></span>
