---
title: Добавление модели в приложение Razor Pages в ASP.NET Core
author: rick-anderson
description: Узнайте, как добавлять классы для управления фильмами в базе данных с использованием Entity Framework Core (EF Core).
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: 508cca07fa96c20e228d2c55c9fb101f7fc3cb02
ms.sourcegitcommit: 79b756ea03eae77a716f500ef88253ee9b1464d2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/22/2018
ms.locfileid: "36327556"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a>Добавление модели в приложение Razor Pages в ASP.NET Core

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Добавление модели данных

В обозревателе решений щелкните проект **RazorPagesMovie** правой кнопкой мыши и выберите пункт **Добавить** > **Новая папка**. Присвойте папке имя *Models*.

Щелкните правой кнопкой мыши папку *Models*. Выберите **Добавить** > **Класс**. Добавьте класс **Movie** и следующие свойства:

Замените содержимое класса `Movie` следующим кодом:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]

## <a name="scaffold-the-movie-model"></a>Создание модели фильма

В этом разделе создается модель фильма. То есть средство формирования шаблонов создает страницы для операций создания, чтения, обновления и удаления для модели фильма.

Создайте папку *Pages/Movies*:

* В **Обозревателе решений** щелкните правой кнопкой мыши на папку *Pages* > **Добавить** > **Новая папка**.
* Назовите папку *Movies*

В **Обозревателе решений** щелкните правой кнопкой мыши на папку *Pages/Movies* > **Добавить** > **Создать шаблонный элемент**.

![Изображение из предыдущих инструкций.](model/_static/sca.png)

В диалоговом окне **Добавить шаблон** выберите **Razor Pages с помощью Entity Framework (CRUD)** > **ДОБАВИТЬ**.

![Изображение из предыдущих инструкций.](model/_static/add_scaffold.png)

Заполните поля в диалоговом окне **Добавить Razor Pages с помощью Entity Framework (CRUD)**:

* В раскрывающемся списке **Класс модели** выберите **Фильм (RazorPagesMovie.Models)**.
* В строке **Класс контекста данных** нажмите на значок плюса **+** и примите сгенерированное имя **RazorPagesMovie.Models.RazorPagesMovieContext**.
* В раскрывающемся списке **Класс контекста данных**  выберите **RazorPagesMovie.Models.RazorPagesMovieContext**
* Нажмите **Добавить**.

![Изображение из предыдущих инструкций.](model/_static/arp.png)

<a name="pmc"></a>
## <a name="perform-initial-migration"></a>Выполнение первоначальной миграции

В этом разделе консоль диспетчера пакетов (PMC) используется для выполнения следующих действий.

* Добавления первоначальной миграции.
* Обновления базы данных с помощью первоначальной миграции.

В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.

  ![Меню PMC](../first-mvc-app/adding-model/_static/pmc.png)

В PMC введите следующие команды:

```PMC
Add-Migration Initial
Update-Database
```

Кроме того, можно использовать следующие команды .NET Core CLI:

```console
dotnet ef migrations add Initial
dotnet ef database update
```

Не обращайте внимание на следующее предупреждающее сообщение, эту проблемы мы решим в следующем руководстве:

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

Команда `Add-Migration` формирует код для создания схемы исходной базы данных. Схема создается на основе модели, указанной в `RazorPagesMovieContext` (в файле *Data/RazorPagesMovieContext.cs*). Аргумент `Initial` используется для присвоения имен миграциям. Можно использовать любое имя, но обычно выбирается имя, описывающее миграцию. Дополнительные сведения см. в статье [Введение в миграции](xref:data/ef-mvc/migrations#introduction-to-migrations).

Команда `Update-Database` выполняет метод `Up` в файле *Migrations/{time-stamp}_InitialCreate.cs*, который создает базу данных.

Если возникает ошибка.

SqlException: не удается открыть базу данных RazorPagesMovieContext-GUID, запрошенную при входе. Сбой при входе.
Сбой при входе в систему пользователя user name.

Вы пропустили [шаг миграции](#pmc).
::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Добавление модели данных

В обозревателе решений щелкните проект **RazorPagesMovie** правой кнопкой мыши и выберите пункт **Добавить** > **Новая папка**. Присвойте папке имя *Models*.

Щелкните правой кнопкой мыши папку *Models*. Выберите **Добавить** > **Класс**. Добавьте класс **Movie** и следующие свойства:

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a>Добавление строки подключения базы данных

Добавьте строку подключения в файл *appsettings.json*.

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a>Регистрация контекста базы данных

Зарегистрируйте контекст базы данных в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) в [методе ConfigureServices класса Startup](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

Выполните сборку проекта, чтобы убедиться в отсутствии ошибок.

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>Добавление средств формирования шаблонов и выполнение первоначальной миграции

В этом разделе консоль диспетчера пакетов (PMC) используется для выполнения следующих действий.

* Добавление веб-пакета создания кода для Visual Studio. Этот пакет необходим для работы ядра формирования шаблонов.
* Добавление первоначальной миграции.
* Обновления базы данных с помощью первоначальной миграции.

В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.

  ![Меню PMC](../first-mvc-app/adding-model/_static/pmc.png)

В PMC введите следующие команды:

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

Кроме того, можно использовать следующие команды .NET Core CLI:

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

Не обращайте внимание на следующее сообщение:

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

Мы исправим это в следующем руководстве.

Команда `Install-Package` устанавливает средства, необходимые для запуска ядра формирования шаблонов.

Команда `Add-Migration` формирует код для создания схемы исходной базы данных. Схема создается на основе модели, указанной в `DbContext` (в файле *Models/MovieContext.cs*). Аргумент `Initial` используется для присвоения имен миграциям. Можно использовать любое имя, но обычно выбирается имя, описывающее миграцию. Дополнительные сведения см. в статье [Введение в миграции](xref:data/ef-mvc/migrations#introduction-to-migrations).

Команда `Update-Database` выполняет метод `Up` в файле *Migrations/{time-stamp}_InitialCreate.cs*, который создает базу данных.

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a>Тестирование приложения

* Запустите приложение и добавьте `/Movies` к URL-адресу в браузере (`http://localhost:port/movies`).
* Протестируйте ссылку **Создать**.

  ![Страница "Создать"](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* Протестируйте ссылки **Изменить**, **Сведения** и **Удалить**.

Если появляется исключение SQL, выполните миграции и обновите базу данных.

В следующем учебнике рассматриваются файлы, созданные с помощью формирования шаблонов.

> [!div class="step-by-step"]
> [Предыдущая статья — "Начало работы"](xref:tutorials/razor-pages/razor-pages-start)
> [Следующая статья — "Сформированные страницы Razor Pages"](xref:tutorials/razor-pages/page)    
