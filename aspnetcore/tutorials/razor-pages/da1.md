---
title: "Изменение созданных страниц"
author: rick-anderson
description: "Изменение созданных страниц для улучшения отображения."
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/da1
ms.openlocfilehash: a1bb1ab1e4fac9c634f4048947ac3f934af3d625
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="update-the-generated-pages"></a><span data-ttu-id="bb43a-103">Изменение созданных страниц</span><span class="sxs-lookup"><span data-stu-id="bb43a-103">Update the generated pages</span></span>

<span data-ttu-id="bb43a-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="bb43a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bb43a-105">Все готово для приложения по работе с фильмами, но презентация далеко не идеальна.</span><span class="sxs-lookup"><span data-stu-id="bb43a-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="bb43a-106">Мы не хотим видеть время (12:00:00 AM на рисунке ниже), а заголовок **ReleaseDate** (ДатаВыпуска) должен состоять из двух слов: **Release Date**.</span><span class="sxs-lookup"><span data-stu-id="bb43a-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![Приложение Movie с данными по фильмам, открытое в Chrome](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="bb43a-108">Обновление созданного кода</span><span class="sxs-lookup"><span data-stu-id="bb43a-108">Update the generated code</span></span>

<span data-ttu-id="bb43a-109">Откройте файл *Models/Movie.cs* и добавьте указанные ниже выделенные строки кода:</span><span class="sxs-lookup"><span data-stu-id="bb43a-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

<span data-ttu-id="bb43a-110">Щелкните правой кнопкой мыши строку, подчеркнутую волнистой красной линией, и выберите пункт **Быстрые действия и рефакторинг**.</span><span class="sxs-lookup"><span data-stu-id="bb43a-110">Right click on a red squiggly line > ** Quick Actions and Refactorings**.</span></span>

  ![Контекстное меню с пунктом **Быстрые действия и рефакторинг**.](da1/qa.png)

<span data-ttu-id="bb43a-112">Выберите `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="bb43a-112">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![using System.ComponentModel.DataAnnotations в верхней части списка.](da1/da.png)

  <span data-ttu-id="bb43a-114">Visual Studio добавит `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="bb43a-114">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

[!INCLUDE[model1](../../includes/RP/da2.md)]

>[!div class="step-by-step"]
<span data-ttu-id="bb43a-115">[Назад: работа с SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Добавление поиска](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="bb43a-115">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Adding Search](xref:tutorials/razor-pages/search)</span></span>
