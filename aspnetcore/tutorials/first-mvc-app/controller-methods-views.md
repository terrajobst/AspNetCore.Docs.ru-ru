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
# <a name="controller-methods-and-views-in-aspnet-core"></a>Методы и представления контроллера в приложении ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Все готово для приложения по работе с фильмами, но презентация далеко не идеальна. Мы не хотим видеть время (12:00:00 AM на рисунке ниже), а заголовок **ReleaseDate** (ДатаВыпуска) должен состоять из двух слов.

![Представление индекса: заголовок "Release Date" (Дата выпуска) состоит из одного слова (без пробела), и рядом с каждой датой отображается время "12:00 AM".](working-with-sql/_static/m55.png)

Откройте файл *Models/Movie.cs* и добавьте указанные ниже выделенные строки:

[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

Щелкните правой кнопкой мыши строку, подчеркнутую волнистой красной линией, и выберите **> Быстрые действия и операции рефакторинга**.

  ![Контекстное меню с пунктом **Быстрые действия и рефакторинг**.](controller-methods-views/_static/qa.png)


Выберите `using System.ComponentModel.DataAnnotations;`

  ![using System.ComponentModel.DataAnnotations в верхней части списка.](controller-methods-views/_static/da.png)

  Visual Studio добавит `using System.ComponentModel.DataAnnotations;`.

Удалим операторы `using`, которые не требуются. По умолчанию они отображаются светло-серым шрифтом. Щелкните правой кнопкой мыши файл *Movie.cs* и выберите **> Удалить и сортировать операторы using**.

![Удалить и сортировать операторы using](controller-methods-views/_static/rm.png)

Обновленный код выглядит следующим образом:

[!code-csharp[](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE [adding-model](../../includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> [Назад](working-with-sql.md)
> [Вперед](search.md)  
