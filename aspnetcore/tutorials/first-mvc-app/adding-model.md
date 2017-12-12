---
title: "Добавление модели в приложение MVC ASP.NET Core"
author: rick-anderson
description: "Добавление модели в простое приложение ASP.NET Core."
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 03/30/2017
ms.topic: get-started-article
ms.assetid: 8dc28498-00ee-4d66-b903-b593059e9f39
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: a29bab9cf0712936fa9c3f2b4bb3b275a46fe6f6
ms.sourcegitcommit: e641c5794525f983485621860926d8ab4e7360c8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/23/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

Примечание. Шаблоны ASP.NET Core 2.0 содержат папку *Models*.

В обозревателе решений щелкните проект **MvcMovie** правой кнопкой мыши и выберите пункт **Добавить** > **Новая папка**. Присвойте папке имя *Models*.

Щелкните правой кнопкой мыши папку *Models* и выберите пункт **Добавить** > **Класс**. Добавьте класс **Movie** и следующие свойства:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

Поле `ID` является обязательным для первичного ключа базы данных. 

Выполните сборку проекта, чтобы убедиться в отсутствии ошибок. Теперь в вашем приложении **M**VC есть **м**одель.

## <a name="scaffolding-a-controller"></a>Формирование шаблонов контроллера

В **обозревателе решений** щелкните папку *Контроллеры* правой кнопкой мыши и выберите пункт **Добавить > Контроллер**.

![представление указанного выше шага](adding-model/_static/add_controller.png)

В диалоговом окне **Добавление зависимостей MVC** установите переключатель в положение **Минимальный набор зависимостей** и нажмите кнопку **Добавить**.

![представление указанного выше шага](adding-model/_static/add_depend.png)

В Visual Studio добавляются зависимости, необходимые для формирования шаблонов контроллера, но сам контроллер не создается. Контроллер будет создан при следующем выполнении команды **Добавить > Контроллер**. 

В **обозревателе решений** щелкните папку *Контроллеры* правой кнопкой мыши и выберите пункт **Добавить > Контроллер**.

![представление указанного выше шага](adding-model/_static/add_controller.png)

В диалоговом окне **Добавление шаблона** выберите **Контроллер MVC с представлениями, использующий Entity Framework > Добавить**.

![Диалоговое окно "Добавление шаблона"](adding-model/_static/add_scaffold2.png)

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
* файлы представления Razor для страниц Create, Delete, Details, Edit и Index (*Views/Movies/&ast;.cshtml*).

Автоматическое создание контекста базы данных и методов и представлений действий [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (создание, чтение, обновление и удаление) называется *формированием шаблонов*. Вскоре вы получите полнофункциональное веб-приложение, позволяющее управлять базой данных фильмов.

Если запустить приложение и щелкнуть ссылку **Mvc Movie**, возникнет ошибка наподобие следующей:

```
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

В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![Меню PMC](adding-model/_static/pmc.png)

В PMC введите следующие команды:

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

**Примечание**. Если при выполнении команды `Install-Package` возникнет ошибка, откройте диспетчер пакетов NuGet и найдите пакет `Microsoft.EntityFrameworkCore.Tools`. Это позволит установить пакет или убедиться, что он установлен. Если же у вас возникают проблемы с PMC, вы можете [воспользоваться командной строкой](#cli).

Команда `Add-Migration` формирует код для создания схемы исходной базы данных. Схема создается на основе модели, указанной в `DbContext` (в файле *Data/MvcMovieContext.cs*). Аргумент `Initial` используется для присвоения имен миграциям. Можно использовать любое имя, но обычно выбирается имя, описывающее миграцию. Дополнительные сведения см. в статье [Введение в миграции](xref:data/ef-mvc/migrations#introduction-to-migrations).

Команда `Update-Database` выполняет метод `Up` в файле *Migrations/\<метка_времени>_Initial.cs*, который создает базу данных.

<a name="cli"></a> Описанные выше действия можно выполнить с помощью интерфейса командной строки (CLI) вместо PMC.

* Добавьте [средства EF Core](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) в файл *CSPROJ*.
* В консоли (в каталоге проекта) выполните следующие команды:

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```     
  

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model4.md)]

![Контекстное меню Intellisense элемента модели со списком доступных свойств: идентификатор, цена, дата выпуска и название](adding-model/_static/ints.png)

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro)
* [Глобализация и локализация](xref:fundamentals/localization)

>[!div class="step-by-step"]
[Назад: добавление представления](adding-view.md)
[Далее: работа с SQL](working-with-sql.md)  
