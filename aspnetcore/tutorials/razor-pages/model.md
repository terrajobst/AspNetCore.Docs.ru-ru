---
title: Добавление модели в приложение Razor Pages в ASP.NET Core
author: rick-anderson
description: Узнайте, как добавлять классы для управления фильмами в базе данных с использованием Entity Framework Core (EF Core).
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: 41a88e06afbe6e7accd03ff7b39aa69e15e0c0b4
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325817"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a>Добавление модели в приложение Razor Pages в ASP.NET Core

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Добавление модели данных

В обозревателе решений щелкните проект **RazorPagesMovie** правой кнопкой мыши и выберите пункт **Добавить** > **Новая папка**. Присвойте папке имя *Models*.

Щелкните правой кнопкой мыши папку *Models*. Выберите **Добавить** > **Класс**. Назначьте классу имя **Movie** и замените содержимое класса `Movie` следующим кодом:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]

## <a name="scaffold-the-movie-model"></a>Создание модели фильма

В этом разделе создается модель фильма. То есть средство формирования шаблонов создает страницы для операций создания, чтения, обновления и удаления для модели фильма.

Создайте папку *Pages/Movies*:

* В **Обозревателе решений** щелкните правой кнопкой мыши на папку *Pages* > **Добавить** > **Новая папка**.
* Назовите папку *Movies*

В **Обозревателе решений** щелкните правой кнопкой мыши на папку *Pages/Movies* > **Добавить** > **Создать шаблонный элемент**.

![Изображение из предыдущих инструкций.](model/_static/sca.png)

В диалоговом окне **Добавить шаблон** выберите **Razor Pages с помощью Entity Framework (CRUD)** > **Добавить**.

![Изображение из предыдущих инструкций.](model/_static/add_scaffold.png)

Заполните поля в диалоговом окне **Добавить Razor Pages с помощью Entity Framework (CRUD)**:

* В раскрывающемся списке **Класс модели** выберите **Фильм (RazorPagesMovie.Models)**.
* В строке **Класс контекста данных** нажмите на значок плюса **+** и примите сгенерированное имя **RazorPagesMovie.Models.RazorPagesMovieContext**.
* Нажмите **Добавить**.

![Изображение из предыдущих инструкций.](model/_static/arp.png)

В процессе формирования шаблонов создаются и обновляются указанные ниже файлы.

### <a name="files-created"></a>Создаваемые файлы

* *Pages/Movies*: Create, Delete, Details, Edit, Index. Эти страницы подробно описываются в следующем учебнике.
* *Data/RazorPagesMovieContext.cs*

### <a name="file-updated"></a>Обновляемые файлы

* *Startup.cs*. Изменения в этом файле подробно описываются в следующем разделе.
* *appsettings.json*. Добавляется строка подключения, используемая для подключения к локальной базе данных.

## <a name="examine-the-context-registered-with-dependency-injection"></a>Проверка контекста, зарегистрированного с помощью внедрения зависимостей

ASP.NET Core поддерживает [внедрение зависимостей](xref:fundamentals/dependency-injection). С помощью внедрения зависимостей службы (например, контекст базы данных EF Core) регистрируются во время запуска приложения. Затем компоненты, которые используют эти службы (например, Razor Pages), обращаются к ним через параметры конструктора. Код конструктора, который получает экземпляр контекста базы данных, приведен далее в этом руководстве.

Средство формирования шаблонов автоматически создает контекст базы данных и регистрирует его с использованием контейнера внедрения зависимостей.

Проверьте метод `Startup.ConfigureServices`. Средством формирования шаблонов была добавлена выделенная строка:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

Контекст базы данных — это класс main, который координирует функциональные возможности EF Core для заданной модели данных. Контекст данных получен из [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). Контекст данных указывает сущности, которые включаются в модель данных. В этом проекте соответствующий класс называется `RazorPagesMovieContext`.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Data/RazorPagesMovieContext.cs)]

Представленный выше код создает свойство [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) для набора сущностей. В терминологии Entity Framework набор сущностей обычно соответствует таблице базы данных. Сущность соответствует строке в таблице.

Имя строки подключения передается в контекст путем вызова метода для объекта [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions). При локальной разработке [система конфигурации ASP.NET Core](xref:fundamentals/configuration/index) считывает строку подключения из файла *appsettings.json*.

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

Кроме того, можно использовать следующие команды .NET Core CLI из папки проекта:

```console
dotnet ef migrations add Initial
dotnet ef database update
```

Не обращайте внимание на следующее предупреждающее сообщение, эту проблему вы решите в одном из следующих руководств:

```console
Microsoft.EntityFrameworkCore.Model.Validation[30000]
      No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.
```

Команда `Add-Migration` формирует код для создания схемы исходной базы данных. Схема создается на основе модели, указанной в `RazorPagesMovieContext` (в файле *Data/RazorPagesMovieContext.cs*). Аргумент `Initial` используется для присвоения имен миграциям. Можно использовать любое имя, но обычно выбирается имя, описывающее миграцию. Дополнительные сведения см. в статье [Введение в миграции](xref:data/ef-mvc/migrations#introduction-to-migrations).

Команда `Update-Database` выполняет метод `Up` в файле *Migrations/{time-stamp}_InitialCreate.cs*, который создает базу данных.

Если возникает ошибка.

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

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

```console
Microsoft.EntityFrameworkCore.Model.Validation[30000]
      No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'
```

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
