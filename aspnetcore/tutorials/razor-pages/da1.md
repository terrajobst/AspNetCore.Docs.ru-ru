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
# <a name="update-the-generated-pages"></a>Изменение созданных страниц

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Все готово для приложения по работе с фильмами, но презентация далеко не идеальна. Мы не хотим видеть время (12:00:00 AM на рисунке ниже), а заголовок **ReleaseDate** (ДатаВыпуска) должен состоять из двух слов: **Release Date**.

![Приложение Movie с данными по фильмам, открытое в Chrome](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Обновление созданного кода

Откройте файл *Models/Movie.cs* и добавьте указанные ниже выделенные строки кода:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

Щелкните правой кнопкой мыши строку, подчеркнутую волнистой красной линией, и выберите пункт **Быстрые действия и рефакторинг**.

  ![Контекстное меню с пунктом **Быстрые действия и рефакторинг**.](da1/qa.png)

Выберите `using System.ComponentModel.DataAnnotations;`.

  ![using System.ComponentModel.DataAnnotations в верхней части списка.](da1/da.png)

  Visual Studio добавит `using System.ComponentModel.DataAnnotations;`.

[!INCLUDE[model1](../../includes/RP/da2.md)]

>[!div class="step-by-step"]
[Назад: работа с SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Добавление поиска](xref:tutorials/razor-pages/search)
