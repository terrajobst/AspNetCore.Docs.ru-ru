---
title: Добавление модели в приложение MVC ASP.NET Core
author: rick-anderson
description: Добавление модели в простое приложение ASP.NET Core.
ms.author: riande
ms.date: 02/25/2019
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 0580c2cb7717a89d0d16cd3f431a63db78fa27cb
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64887429"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a>Добавление модели в приложение MVC ASP.NET Core

Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra)

В этом разделе мы добавим классы для управления фильмами в базе данных. Эти классы будут представлять уровень **м**одели в приложении **M**VC.

Эти классы используются в [Entity Framework Core](/ef/core) (EF Core) для работы с базой данных. EF Core —это платформа объектно реляционного сопоставления (ORM), которая позволяет упростить необходимый код доступа к данным.

Создаваемые вами классы моделей называются классами POCO (от **P**lain **O**ld **C**LR **O** — старые добрые объекты CLR), так как они не зависят от EF Core. Эти классы просто определяют свойства данных, которые будут храниться в базе данных.

Работая с этим учебником, вы напишете классы моделей, а затем EF Core создаст базу данных. Другой вариант, который здесь не рассматривается: создание классов моделей из существующей базы данных. Дополнительные сведения об этом подходе см. в разделе [ASP.NET Core — существующая база данных](/ef/core/get-started/aspnetcore/existing-db).

## <a name="add-a-data-model-class"></a>Добавление класса модели данных

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Щелкните правой кнопкой мыши папку *Models* и выберите пункт **Добавить** > **Класс**. Присвойте классу имя **Movie**.

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio для Mac](#tab/visual-studio-code+visual-studio-mac)

* Добавьте класс в папку *Models* с именем *Movie.cs*.

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]
[!INCLUDE [model 2](~/includes/mvc-intro/model2.md)]

---

## <a name="scaffold-the-movie-model"></a>Создание модели фильма

В этом разделе создается модель фильма. То есть средство формирования шаблонов создает страницы для операций создания, чтения, обновления и удаления для модели фильма.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

В **Обозревателе решений** щелкните правой кнопкой мыши папку *Контроллеры* и выберите **Добавить > Создать шаблонный элемент**.

![представление указанного выше шага](adding-model/_static/add_controller21.png)

В диалоговом окне **Добавление шаблона** выберите **Контроллер MVC с представлениями, использующий Entity Framework > Добавить**.

![Диалоговое окно "Добавление шаблона"](adding-model/_static/add_scaffold21.png)

Выполните необходимые действия в диалоговом окне **Добавление контроллера**:

* **Класс модели:** *Movie (MvcMovie.Models)*
* **Класс контекста данных:** щелкните значок **+** и добавьте класс **MvcMovie.Models.MvcMovieContext** по умолчанию.

![Добавление контекста данных](adding-model/_static/dc.png)

* **Представления:** оставьте флажки, установленные по умолчанию
* **Имя контроллера:** оставьте имя по умолчанию (*MoviesController*)
* Нажмите **Добавить**

![Диалоговое окно "Добавление контроллера"](adding-model/_static/add_controller2.png)

Visual Studio создаст следующие компоненты:

* [класс контекста базы данных](xref:data/ef-mvc/intro#create-the-database-context) Entity Framework Core (*Data/MvcMovieContext.cs*);
* контроллер фильмов (*Controllers/MoviesController.cs*);
* файлы представления Razor для страниц Create, Delete, Details, Edit и Index (*Views/Movies/\*.cshtml*).

Автоматическое создание контекста базы данных и методов и представлений действий [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (создание, чтение, обновление и удаление) называется *формированием шаблонов*.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).
* Установка средства формирования шаблонов:

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* Выполните следующую команду:

  ```console
     dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

* Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).
* Установка средства формирования шаблонов:

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* Выполните следующую команду:

  ```console
     dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<!-- End of VS tabs                  -->

Если запустить приложение и щелкнуть ссылку **Mvc Movie**, возникает ошибка наподобие следующей:

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString
```

Вам необходимо создать базу данных. Для этого вы используете функцию [миграций](xref:data/ef-mvc/migrations) EF Core. С помощью миграций можно создать базу данных, соответствующую модели данных, и обновлять схему базы данных при изменении модели.

<a name="pmc"></a>

## <a name="initial-migration"></a>Первоначальная миграция

В этом разделе выполняются следующие задачи:

* Добавления первоначальной миграции.
* Обновления базы данных с помощью первоначальной миграции.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов** (PMC).

   ![Меню PMC](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

1. В PMC введите следующие команды:

   ```console
   Add-Migration Initial
   Update-Database
   ```

   Команда `Add-Migration` формирует код для создания схемы исходной базы данных.

   Схема базы данных создается на основе модели, указанной в классе `MvcMovieContext` (в файле *Data/MvcMovieContext.cs*). Аргумент `Initial` — это имя миграции. Можно использовать любое имя, но по соглашению используется имя, которое описывает миграцию. Для получения дополнительной информации см. <xref:data/ef-mvc/migrations>.

   Команда `Update-Database` выполняет метод `Up` в файле *Migrations/{time-stamp}_InitialCreate.cs*, который создает базу данных.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio для Mac](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

Команда `ef migrations add InitialCreate` формирует код для создания схемы исходной базы данных.

Схема базы данных создается на основе модели, указанной в классе `MvcMovieContext` (в файле *Data/MvcMovieContext.cs*). Аргумент `InitialCreate` — это имя миграции. Можно использовать любое имя, но по соглашению выбирается имя, которое описывает миграцию.

---

## <a name="examine-the-context-registered-with-dependency-injection"></a>Проверка контекста, зарегистрированного с помощью внедрения зависимостей

ASP.NET Core поддерживает [внедрение зависимостей](xref:fundamentals/dependency-injection). С помощью внедрения зависимостей службы (например, контекст базы данных EF Core) регистрируются во время запуска приложения. Затем компоненты, которые используют эти службы (например, Razor Pages), обращаются к ним через параметры конструктора. Код конструктора, который получает экземпляр контекста базы данных, приведен далее в этом руководстве.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Средство формирования шаблонов автоматически создает контекст базы данных и регистрирует его с использованием контейнера внедрения зависимостей.

Рассмотрим следующий метод `Startup.ConfigureServices`. Средством формирования шаблонов была добавлена выделенная строка:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

`MvcMovieContext` координирует функции EF Core (Create, Read, Update, Delete и т. д.) для модели `Movie`. Контекст данных (`MvcMovieContext`) получен из [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). Контекст данных указывает сущности, которые включаются в модель данных:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Data/MvcMovieContext.cs)]

Представленный выше код создает свойство [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) для набора сущностей. В терминологии Entity Framework набор сущностей обычно соответствует таблице базы данных. Сущность соответствует строке в таблице.

Имя строки подключения передается в контекст путем вызова метода для объекта [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions). При локальной разработке [система конфигурации ASP.NET Core](xref:fundamentals/configuration/index) считывает строку подключения из файла *appsettings.json*.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio для Mac](#tab/visual-studio-code+visual-studio-mac)

Вы создаете контекст базы данных и регистрируете его с использованием контейнера внедрения зависимостей.

---

<a name="test"></a>

### <a name="test-the-app"></a>Тестирование приложения

* Запустите приложение и добавьте `/Movies` к URL-адресу в браузере (`http://localhost:port/movies`).

Если вы получите исключение базы данных, аналогичное следующему:

```console
SqlException: Cannot open database "MvcMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

Вы пропустили [шаг миграции](#pmc).

* Протестируйте ссылку **Создать**. Введите и отправьте данные.

  > [!NOTE]
  > В поле `Price` нельзя вводить десятичные запятые. Чтобы обеспечить поддержку [проверки jQuery](https://jqueryvalidation.org/) для других языков, кроме английского, используйте вместо десятичной точки запятую (,), а для отображения данных в форматах для других языков, кроме английского, выполните глобализацию приложения. Инструкции по глобализации см. на [сайте GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).

* Протестируйте ссылки **Изменить**, **Сведения** и **Удалить**.

Проверьте класс `Startup`:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

Выделенный выше код демонстрирует добавление контекста базы данных фильмов в контейнер [внедрения зависимостей](xref:fundamentals/dependency-injection):

* `services.AddDbContext<MvcMovieContext>(options =>` задает используемую базу данных и строку подключения.
* `=>` — это [лямбда-оператор](/dotnet/articles/csharp/language-reference/operators/lambda-operator)

Откройте файл *Controllers/MoviesController.cs* и изучите его конструктор:

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

Этот конструктор применяет [внедрение зависимостей](xref:fundamentals/dependency-injection) для внедрения контекста базы данных (`MvcMovieContext`) в контроллер. Контекст базы данных используется в каждом методе [создания, чтения, обновления и удаления](https://wikipedia.org/wiki/Create,_read,_update_and_delete) в контроллере.

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a>Строго типизированные модели и ключевое слово @model

Ранее в этом руководстве вы ознакомились с передачей данных или объектов из контроллера в представление с использованием словаря `ViewData`. Словарь `ViewData` представляет собой динамический объект, который реализует удобный механизм позднего связывания для передачи информации в представление.

Модель MVC также поддерживает передачу строго типизированных объектов модели в представление. Такой подход обеспечивает более эффективную проверку кода во время компиляции. При создании методов и представлений в этом подходе используется механизм формирования шаблонов (то есть передачи строго типизированной модели) с представлениями и классами `MoviesController`.

Изучите созданный метод `Details` в файле *Controllers/MoviesController.cs*:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

Параметр `id` обычно передается в качестве данных маршрута. Например, `https://localhost:5001/movies/details/1` задает:

* Контроллер `movies` (первый сегмент URL-адреса).
* Действие `details` (второй сегмент URL-адреса).
* Идентификатор 1 (последний сегмент URL-адреса).

Также можно передать `id` с помощью строки запроса следующим образом:

`https://localhost:5001/movies/details?id=1`

Параметр `id` определяется как [тип, допускающий значение NULL](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`), в случае, если не указано значение идентификатора.

[Лямбда-выражение](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) передается в `FirstOrDefaultAsync` для выбора записей фильмов, которые соответствуют данным маршрута или значению строки запроса.

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

Если фильм найден, экземпляр модели `Movie` передается в представление `Details`:

```csharp
return View(movie);
   ```

Изучите содержимое файла *Views/Movies/Details.cshtml*:

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

Указав оператор `@model` в начале файла представления, вы можете задать тип объекта, который будет ожидаться представлением. При создании контроллера movie следующий оператор `@model` был автоматически добавлен в начало файла *Details.cshtml*:

```HTML
@model MvcMovie.Models.Movie
   ```

Эта директива `@model` обеспечивает доступ к фильму, который контроллер передал в представление с использованием строго типизированного объекта `Model`. Например, в представлении *Details.cshtml* код передает каждое поле фильма во вспомогательные функции HTML `DisplayNameFor` и `DisplayFor` со строго типизированным объектом `Model`. Методы `Create` и `Edit` и представления также передают объект модели `Movie`.

Изучите представление *Index.cshtml* и метод `Index` в контроллере Movies. Обратите внимание на то, как в коде создается объект `List` при вызове метода `View`. Код передает список `Movies` из метода действия `Index` в представление:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

При создании контроллера movies механизм формирования шаблонов автоматически включает следующий оператор `@model` в начало файла *Index.cshtml*:

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

Эта директива `@model` обеспечивает доступ к списку фильмов, который контроллер передал в представление с использованием строго типизированного объекта `Model`. Например, в представлении *Index.cshtml* код циклически перебирает фильмы с использованием оператора `foreach` для строго типизированного объекта `Model`:

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

Поскольку объект `Model` является строго типизированным (как и объект `IEnumerable<Movie>`), каждый элемент в цикле получает тип `Movie`. Помимо прочих преимуществ, это означает, что выполняется проверка кода во время компиляции:

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro)
* [Глобализация и локализация](xref:fundamentals/localization)

> [!div class="step-by-step"]
> [Назад: добавление представления](adding-view.md)
> [Далее: работа с SQL](working-with-sql.md)
