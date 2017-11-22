---
title: "Добавление модели в приложение MVC ASP.NET Core"
author: rick-anderson
description: "Добавление модели в простое приложение ASP.NET Core."
keywords: ASP.NET Core,MVC,scaffold,scaffolding
ms.author: riande
manager: wpickett
ms.devlang: csharp
ms.date: 09/22/2017
ms.topic: get-started-article
ms.assetid: 8dc28498-eeee-1638-b903-b593059e9f39
ms.technology: aspnet
ms.prod: .net-core
uid: tutorials/first-mvc-app-mac/adding-model
ms.openlocfilehash: ff0b262bdf2685bd1bc410c30c12aa2d16f6dcda
ms.sourcegitcommit: 7d092cd99057bad9246c472a8a0a8cbc7ab9fa9b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/13/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

* Щелкните папку *Models* правой кнопкой мыши и выберите пункт **Добавить** > **Новый файл**. 
* В диалоговом окне **Новый файл** выполните следующие действия.

  * Выберите на левой панели пункт **Общие**.
  * Выберите в центральной области **Пустой класс**.
  * Назовите класс **Movie** и выберите **Создать**.

Добавьте в класс `Movie` следующие свойства:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

Поле `ID` является обязательным для первичного ключа базы данных.

Выполните сборку проекта, чтобы убедиться в отсутствии ошибок. Теперь в вашем приложении **M**VC есть **M**одель.

## <a name="prepare-the-project-for-scaffolding"></a>Подготовка проекта для формирования шаблонов

- Щелкните файл проекта правой кнопкой мыши и последовательно выберите параметры **Сервис > Изменить файл**.

  ![представление указанного выше шага](adding-model/_static/1.png)

- Добавьте в файл *MvcMovie.csproj* следующие выделенные пакеты NuGet:
             
  [!code-csharp[Main](../first-mvc-app-xplat/start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- Сохраните файл.

- Создайте файл *Models/MvcMovieContext.cs* и добавьте следующий класс `MvcMovieContext`: [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)].
   
- Откройте файл *Startup.cs* и добавьте две директивы using: [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)].

- Добавьте контекст базы данных в файл *Startup.cs*:

   [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  За счет этого Entity Framework будет знать, какие классы модели входят в модель данных. Вы определяете один *набор сущностей* объектов Movie, который будут представлен в базу данных как таблица Movie.

- Выполните сборку проекта, чтобы убедиться в отсутствии ошибок.

## <a name="scaffold-the-moviecontroller"></a>Формирование шаблонов для MovieController

Откройте окно терминала в папке проекта и выполните следующие команды.

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
Если возникает ошибка `No executable found matching command "dotnet-aspnet-codegenerator", verify`.

 * Вы находитесь в папке проекта. В папке проекта имеются файлы *Program.cs*, *Startup.cs* и *.csproj*.
 * Ваша версия — 1.1 или более поздняя. Запустите `dotnet`, чтобы узнать версию.
 * Вы добавили элемент `<DotNetCliToolReference>` в [файл MvcMovie.csproj](#prepare-the-project-for-scaffolding).
 
<!--
> [!NOTE]
> If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.
-->

Ядро формирование шаблонов создает следующее.

* Контроллер фильмов (*Controllers/MoviesController.cs*)
* Файлы представления Razor для страниц Create, Delete, Edit и Index (*Views/Movies/\*.cshtml*)

Автоматическое создание методов и представлений действия [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (создание, чтение, обновление и удаление) называется *формированием шаблонов*. Вскоре вы получите полнофункциональное веб-приложение, позволяющее управлять базой данных фильмов.

### <a name="add-the-files-to-visual-studio"></a>Добавление файлов в Visual Studio

* Добавьте файл *MovieController.cs* в проект Visual Studio.

  * Щелкните папку *Controllers* правой кнопкой мыши и выберите пункт **Добавить > Добавить файлы**.
  * Выберите файл *MovieController.cs*.

* Добавьте папку *Movies* и представления.

  * Щелкните папку *Views* правой кнопкой мыши и выберите пункт **Добавить > Добавить существующую папку**.
  * Перейдите к папку *Views*, выберите *Views\Movies* и нажмите кнопку **Открыть**.
  * В диалоговом окне **Выберите файлы для добавления из папки Movies** выберите вариант **Включить все** и нажмите кнопку **ОК**.

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

Теперь у вас есть база данных и страницы для отображения, редактирования, обновления и удаления данных. В следующем руководстве мы будем работать с базой данных.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro)
* [Глобализация и локализация](xref:fundamentals/localization)

>[!div class="step-by-step"]
[Назад: добавление представления](adding-view.md)
[Далее: работа с SQL](working-with-sql.md)  
