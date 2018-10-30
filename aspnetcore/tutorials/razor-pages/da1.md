---
title: Изменение созданных страниц в приложении ASP.NET Core
author: rick-anderson
description: Сведения об изменении созданных страниц в приложении ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: f47a68840a6307b69bc92a7b157037d91dce5422
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477219"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a>Изменение созданных страниц в приложении ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Все готово для приложения по работе с фильмами, но презентация далеко не идеальна. Мы не хотим видеть время (12:00:00 AM на рисунке ниже), а заголовок **ReleaseDate** (ДатаВыпуска) должен состоять из двух слов: **Release Date**.

![Приложение Movie с данными по фильмам, открытое в Chrome](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Обновление созданного кода

Откройте файл *Models/Movie.cs* и добавьте указанные ниже выделенные строки кода:

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]

::: moniker-end

Щелкните правой кнопкой мыши строку, подчеркнутую волнистой красной линией, и выберите пункт **Быстрые действия и рефакторинг**.

  ![Контекстное меню с пунктом **Быстрые действия и рефакторинг**.](da1/qa.png)

Выберите `using System.ComponentModel.DataAnnotations;`.

  ![using System.ComponentModel.DataAnnotations в верхней части списка.](da1/da.png)

  Visual Studio добавит `using System.ComponentModel.DataAnnotations;`.

[!INCLUDE [model1](~/includes/RP/da2.md)]

> [!div class="step-by-step"]
> [Предыдущая статья: "Работа с SQL Server LocalDB"](xref:tutorials/razor-pages/sql)
> [Следующая статья: "Добавление поиска"](xref:tutorials/razor-pages/search)
