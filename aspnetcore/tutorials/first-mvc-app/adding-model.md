---
title: Добавление модели в приложение MVC ASP.NET Core
author: rick-anderson
description: Добавление модели в простое приложение ASP.NET Core.
ms.author: riande
ms.date: 12/8/2017
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 28a63498bc1a3c7b6ad6be038209dacdb49e44ee
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960672"
---
[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model1.md)]

Щелкните правой кнопкой мыши папку *Models* и выберите пункт **Добавить** > **Класс**. Добавьте класс **Movie** и следующие свойства:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

Поле `ID` является обязательным для первичного ключа базы данных. 

Выполните сборку проекта, чтобы убедиться в отсутствии ошибок. Теперь в вашем приложении **M**VC есть **м**одель.

## <a name="scaffolding-a-controller"></a>Формирование шаблонов контроллера

::: moniker range=">= aspnetcore-2.1"

В **Обозревателе решений** щелкните правой кнопкой мыши папку *Контроллеры* и выберите **Добавить > Создать шаблонный элемент**.

![представление указанного выше шага](adding-model/_static/add_controller21.png)

В диалоговом окне **Добавление шаблона** выберите **Контроллер MVC с представлениями, использующий Entity Framework > Добавить**.

![Диалоговое окно "Добавление шаблона"](adding-model/_static/add_scaffold21.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

В **обозревателе решений** щелкните папку *Контроллеры* правой кнопкой мыши и выберите пункт **Добавить > Контроллер**.

![представление указанного выше шага](adding-model/_static/add_controller.png)

Если открывается диалоговое окно **Добавление зависимостей MVC**:

* [Обновите Visual Studio до последней версии](https://www.visualstudio.com/downloads/). Это диалоговое окно отображает версии Visual Studio, предшествующие 15.5.
* Если обновить не удается, выберите **ДОБАВИТЬ** и снова выполните шаги по добавлению контроллера.

В диалоговом окне **Добавление шаблона** выберите **Контроллер MVC с представлениями, использующий Entity Framework > Добавить**.

![Диалоговое окно "Добавление шаблона"](adding-model/_static/add_scaffold2.png)

::: moniker-end

Выполните необходимые действия в диалоговом окне **Добавление контроллера**:

* **Класс модели:** *Movie (MvcMovie.Models)*
* **Класс контекста данных:** щелкните значок **+** и добавьте класс **MvcMovie.Models.MvcMovieContext** по умолчанию.

![Добавление контекста данных](adding-model/_static/dc.png)

* **Представление:** оставьте флажки, установленные по умолчанию.
* **Имя контроллера:** оставьте имя по умолчанию (*MoviesController*).
* Нажмите кнопку **Добавить**.

![Диалоговое окно "Добавление контроллера"](adding-model/_static/add_controller2.png)

Visual Studio создаст следующие компоненты:

* [класс контекста базы данных](xref:data/ef-mvc/intro#create-the-database-context) Entity Framework Core (*Data/MvcMovieContext.cs*);
* контроллер фильмов (*Controllers/MoviesController.cs*);
* файлы представления Razor для страниц Create, Delete, Details, Edit и Index (<em>Views/Movies/&ast;.cshtml</em>).

Автоматическое создание контекста базы данных и методов и представлений действий [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (создание, чтение, обновление и удаление) называется *формированием шаблонов*. Вскоре вы получите полнофункциональное веб-приложение, позволяющее управлять базой данных фильмов.

Если запустить приложение и щелкнуть ссылку **Mvc Movie**, возникает ошибка наподобие следующей:

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

Вам необходимо создать базу данных. Для этого вы используете функцию [миграций](xref:data/ef-mvc/migrations) EF Core. С помощью миграций можно создать базу данных, соответствующую модели данных, и обновлять схему базы данных при изменении модели.

## <a name="add-ef-tooling-and-perform-initial-migration"></a>Добавление средств EF и выполнение первоначальной миграции

В этом разделе консоль диспетчера пакетов (PMC) используется для выполнения следующих действий:

* Добавления пакета средств Entity Framework Core. Этот пакет необходим для добавления миграций и обновления базы данных.
* Добавления первоначальной миграции.
* Обновления базы данных с помощью первоначальной миграции.

В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.

<!-- following image shared with uid: tutorials/razor-pages/model --> ![Меню PMC](adding-model/_static/pmc.png)

В PMC введите следующие команды:

::: moniker range=">= aspnetcore-2.1"
``` PMC
Add-Migration Initial
Update-Database
```

Не обращайте внимание на сообщение об ошибке, мы исправим это в следующем руководстве:

*Microsoft.EntityFrameworkCore.Model.Validation[30000]*  
      *Для десятичного столбца Price в типе сущности Movie не указан тип. Это приведет к тому, что значения будут усекаться без вмешательства пользователя, если они не помещаются в значения точности и масштаба по умолчанию. Явно укажите тип столбца SQL Server, который может вместить все значения, с помощью 'ForHasColumnType()'.*

::: moniker-end
::: moniker range="<= aspnetcore-2.0"

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

**Примечание**. Если при выполнении команды `Install-Package` возникнет ошибка, откройте диспетчер пакетов NuGet и найдите пакет `Microsoft.EntityFrameworkCore.Tools`. Это позволит установить пакет или убедиться, что он установлен. Если же у вас возникают проблемы с PMC, вы можете [воспользоваться командной строкой](#cli).

::: moniker-end

Команда `Add-Migration` формирует код для создания схемы исходной базы данных. Схема создается на основе модели, указанной в `DbContext` (в файле *Data/MvcMovieContext.cs*). Аргумент `Initial` используется для присвоения имен миграциям. Можно использовать любое имя, но обычно выбирается имя, описывающее миграцию. Дополнительные сведения см. в статье [Введение в миграции](xref:data/ef-mvc/migrations#introduction-to-migrations).

Команда `Update-Database` выполняет метод `Up` в файле *Migrations/\<метка_времени>_Initial.cs*, который создает базу данных.

<a name="cli"></a> Описанные выше действия можно выполнить с помощью интерфейса командной строки (CLI) вместо PMC.

* Добавьте [средства EF Core](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) в файл *CSPROJ*.
* В консоли (в каталоге проекта) выполните следующие команды:

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```

  Если вы запускаете приложение и возникает ошибка:

  ```text
  SqlException: Cannot open database "Movie" requested by the login.
  The login failed.
  Login failed for user 'user name'.
  ```

Возможно, вы не выполнили `dotnet ef database update`.

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model3.md)]

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]
::: moniker-end

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model4.md)]

![Контекстное меню Intellisense элемента модели со списком доступных свойств: идентификатор, цена, дата выпуска и название](adding-model/_static/ints.png)

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro)
* [Глобализация и локализация](xref:fundamentals/localization)

> [!div class="step-by-step"]
> [Назад: добавление представления](adding-view.md)
> [Далее: работа с SQL](working-with-sql.md)  
