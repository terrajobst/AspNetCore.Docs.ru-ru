---
title: "Работа с SQL Server LocalDB и ASP.NET Core"
author: rick-anderson
description: "Описывается работа с SQL Server LocalDB и ASP.NET Core."
keywords: ASP.NET Core, Razor Pages, Razor, MVC, SQL, LocalDB
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 42fa98886f3e87e79ea1ea4a2223a79319676006
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/28/2017
---
# <a name="working-with-sql-server-localdb-and-aspnet-core"></a>Работа с SQL Server LocalDB и ASP.NET Core

Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Джо Одетт](https://twitter.com/joeaudette) (Joe Audette) 

Объект `MovieContext` обрабатывает задачу подключения к базе данных и сопоставления объектов `Movie` с записями базы данных. Контекст базы данных регистрируется с помощью контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection) в методе `ConfigureServices` в файле *Startup.cs*:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=6-7)]

Система [конфигурации](xref:fundamentals/configuration) ASP.NET Core считывает `ConnectionString`. Для разработки на локальном уровне она получает строку подключения из файла *appsettings.json*:

[!code-json[Main](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

При развертывании приложения на тестовом или рабочем сервере вы можете использовать переменную среды или другой способ настройки строки подключения к реальному серверу SQL Server. Дополнительные сведения см. в статье [Конфигурация](xref:fundamentals/configuration).

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

LocalDB — это упрощенная версия ядра СУБД SQL Server Express, предназначенная для разработки программ. LocalDB запускается по требованию в пользовательском режиме, поэтому настройки не слишком сложны. По умолчанию база данных LocalDB создает файлы \*.mdf в каталоге *C:/Users/\<пользователь\>*.

<a name="ssox"></a>
* В меню **Вид** откройте **обозреватель объектов SQL Server** (SSOX).

  ![Меню "Вид"](sql/_static/ssox.png)

* Щелкните правой кнопкой мыши таблицу `Movie` и выберите пункт **Конструктор представлений**.

  ![Контекстное меню, открытое на таблице Movie](sql/_static/design.png)

  ![Таблица Movie, открытая в конструкторе](sql/_static/dv.png)

Обратите внимание на значок с изображением ключа рядом с `ID`. По умолчанию EF создает свойство с именем `ID` для первичного ключа.

* Щелкните правой кнопкой мыши таблицу `Movie` и выберите пункт **Просмотреть данные**.

  ![Открытая таблица Movie с отображением данных](sql/_static/vd22.png)

## <a name="seed-the-database"></a>Заполнение базы данных

Создайте класс `SeedData` в папке *Models*. Замените сгенерированный код следующим кодом:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

Если в базе данных есть фильмы, возвращается инициализатор заполнения и фильмы не добавляются.

```csharp
if (context.Movies.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a>Добавление инициализатора заполнения

Добавьте инициализатор заполнения в конец метода `Main` в файле *Program.cs*:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Program.cs?highlight=6,17-32)]

Тестирование приложения

* Удалите все записи из базы данных. Это можно сделать с помощью ссылок удаления в браузере или из [SSOX](xref:tutorials/razor-pages/new-field#ssox).
* Необходимо выполнить инициализацию (вызывать методы в классе `Startup`), чтобы запустить метод заполнения. Для этого следует остановить и перезапустить IIS Express. Воспользуйтесь одним из перечисленных ниже подходов.

  * Щелкните правой кнопкой мыши значок IIS Express в области уведомлений и выберите **Выход** или **Остановить сайт**.

    ![Значок IIS Express в области уведомлений](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Контекстное меню](sql/_static/stopIIS.png)

   * Если среда Visual Studio была запущена в режиме без отладки, нажмите клавишу F5 для запуска в режиме отладки.
   * Если среда Visual Studio была запущена в режиме отладки, остановите отладчик и нажмите клавишу F5.
   
В приложении отображаются заполненные данные.

![Приложение Movie с данными по фильмам, открытое в Chrome](sql/_static/m55.png)

В следующем учебнике будет улучшено представление данных.

>[!div class="step-by-step"]
[Предыдущая тема. Шаблонные страницы Razor](xref:tutorials/razor-pages/page)
[Следующая тема. Обновление страниц](xref:tutorials/razor-pages/da1)
