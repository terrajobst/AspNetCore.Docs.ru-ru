---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/accessing-your-models-data-from-a-controller
title: "Доступ к данным вашей модели из контроллера (C#) | Документы Microsoft"
author: Rick-Anderson
description: "Этот учебник поможет узнать основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, являющийся..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 002ada5c-f114-47ab-a441-57dbdb728ea0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 5ee29dbc5b4566273592041d94458104e6e0f65e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="accessing-your-models-data-from-a-controller-c"></a>Доступ к данным вашей модели из контроллера (C#)
====================
По [Рик Андерсон](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Доступна обновленная версия этого учебника [здесь](../../../getting-started/introduction/getting-started.md) , с использованием ASP.NET MVC 5 и Visual Studio 2013. Он является более безопасны, выполните гораздо проще и показаны дополнительные возможности.
> 
> 
> Этот учебник поможет узнать основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, которой — это бесплатная версия Microsoft Visual Studio. Прежде чем начать, убедитесь, что вы установили необходимые компоненты, перечисленные ниже. Все из них можно установить, щелкнув по следующей ссылке: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Кроме того можно установить отдельно предварительные требования, используя следующие ссылки:
> 
> - [Необходимые компоненты Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Обновление средств ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(среда выполнения + средства поддержки)
> 
> Если вы используете Visual Studio 2010 вместо Visual Web Developer 2010, необходимо установить необходимые компоненты, щелкнув по следующей ссылке: [необходимых компонентов Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Проект Visual Web Developer с исходного кода C# доступна по следующему адресу. [Загрузка версии C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Если используется Visual Basic, перейдите в [Visual Basic-версии](../vb/intro-to-aspnet-mvc-3.md) этого учебника.

В этом разделе вы создадите новый `MoviesController` класса и написать код, который получает данные фильма и отображает его в браузере, с помощью шаблона представления. Убедитесь, что сборка приложения перед продолжением.

Щелкните правой кнопкой мыши *контроллеров* папки и создайте новый `MoviesController` контроллера. Выберите следующие параметры:

- Имя контроллера: **MoviesController**. (Это значение по умолчанию. )
- Шаблон: **контроллер с действиями чтения и записи и представлениями, использующий Entity Framework**.
- Класс модели: **фильма (MvcMovie.Models)**.
- Класс контекста данных: **MovieDBContext (MvcMovie.Models)**.
- Представления: **Razor (CSHTML)**. (По умолчанию).

[![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image2.png "AddScaffoldedMovieController")](accessing-your-models-data-from-a-controller/_static/image1.png)

Нажмите кнопку **Добавить**. Visual Web Developer создает следующие файлы и папки:

- *MoviesController.cs* файл в проекте *контроллеров* папки.
- Объект *фильмов* папки в проекте *представления* папки.
- *CREATE.cshtml Delete.cshtml, Details.cshtml, Edit.cshtml*, и *Index.cshtml* в новом *Views\Movies* папки.

[![NewMovieControllerScreenShot](accessing-your-models-data-from-a-controller/_static/image4.png "NewMovieControllerScreenShot")](accessing-your-models-data-from-a-controller/_static/image3.png)

Механизм формирования шаблонов ASP.NET MVC 3 автоматически создается CRUD (Создание, чтение, обновление и удаление) методы действий и представления для вас. Теперь у вас есть полнофункциональную веб-приложение, позволяющее создавать, список, изменение и удаление записей фильма.

Запустите приложение и перейдите к `Movies` контроллера путем добавления */Movies* URL-адрес в адресной строке браузера. Так как приложение полагается на маршрутизацию по умолчанию (определенный в *Global.asax* файл), запрос браузера `http://localhost:xxxxx/Movies` направляется по умолчанию `Index` метод действия `Movies` контроллера. Другими словами, запрос браузера `http://localhost:xxxxx/Movies` фактически является таким же, как запрос браузера `http://localhost:xxxxx/Movies/Index`. Результат — пустой список фильмов, так как вы их еще не добавили.

![](accessing-your-models-data-from-a-controller/_static/image5.png)

### <a name="creating-a-movie"></a>Создание фильма

Щелкните ссылку **Create New** (Создать). Введите некоторые сведения о фильма и нажмите кнопку **создать** кнопки.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Щелкнув **создать** кнопка вызывает отправку формы на сервер, где фильма сведения сохраняются в базе данных. Затем можно будет перенаправлен на */Movies* URL-адрес, где можно просмотреть только что созданный фильма в списке.

[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png "IndexWhenHarryMet")](accessing-your-models-data-from-a-controller/_static/image7.png)

Создайте еще несколько записей фильмов. Попробуйте воспользоваться ссылками **Изменить**, **Сведения** и **Удалить** — все они работают.

## <a name="examining-the-generated-code"></a>Изучение созданного кода

Откройте *Controllers\MoviesController.cs* файл и проверьте созданный `Index` метод. Часть контроллер фильма с `Index` метода показан ниже.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

В следующей строке из `MoviesController` класс создает экземпляры фильма контекста базы данных, как описано выше. Контекст базы данных фильма можно использовать для запроса, редактировать и удалять элементы.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Запрос на `Movies` возвращает все записи в `Movies` фильма базы данных, а затем передает результаты в `Index` представления.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Строго типизированные моделей и @model ключевое слово

Ранее в этом учебнике показано, как контроллер может передать объекты или данные шаблона представления с помощью `ViewBag` объекта. `ViewBag` Является динамическим объектом, который предоставляет удобный способ позднего связывания для передачи данных в представление.

ASP.NET MVC предоставляет также возможность передавать строго типизированные данные или объекты для шаблона представления. Это строго типизированными подход позволяет лучше проверка во время компиляции кода и обширные возможности IntelliSense в редакторе Visual Web Developer. Мы используем этот подход с `MoviesController` класса и *Index.cshtml* шаблона представления.

Обратите внимание на то, как код создает [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) объект при вызове `View` вспомогательный метод `Index` метода действия. Затем код передает это `Movies` список из контроллера в представление:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

Включив `@model` инструкции в верхней части представления файла шаблона, можно указать тип объекта, который ожидает представления. При создании контроллера фильма Visual Web Developer автоматически включаются следующие `@model` инструкции в верхней части *Index.cshtml* файла:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

Это `@model` директива позволяет получить доступ к списку фильмы, контроллера передается в представление, с помощью `Model` объекта со строгим типом. Например, в *Index.cshtml* шаблона, код выполняет цикл по фильмы, выполняя `foreach` инструкции через строго типизированный `Model` объекта:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml)]

Поскольку `Model` объект является типобезопасным (как `IEnumerable<Movie>` объекта), каждая `item` объект в цикле типизируется как `Movie`. Помимо прочих преимуществ это означает получение проверки кода во время компиляции и полная поддержка технологии IntelliSense в редакторе кода.

[![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image10.png "ModelIntellisene")](accessing-your-models-data-from-a-controller/_static/image9.png)

## <a name="working-with-sql-server-compact"></a>Работа с SQL Server Compact

Entity Framework Code First обнаружил, что указывает строку подключения базы данных, который был предоставлен `Movies` базы данных, которая не существует, поэтому Code First база данных создана автоматически. Убедитесь, что он был создан путем поиска в *приложения\_данные* папки. Если вы не видите *Movies.sdf* файла, нажмите кнопку **Показать все файлы** кнопку в **обозревателе решений** инструментов, нажмите кнопку **обновление** кнопку, а затем разверните *приложения\_данные* папки.

[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png "SDF_in_SolnExp")](accessing-your-models-data-from-a-controller/_static/image11.png)

Дважды щелкните *Movies.sdf* Открытие **обозревателя серверов**. Разверните **таблиц** папку для просмотра таблиц, которые были созданы в базе данных.

> [!NOTE]
> Если возникает сообщение об ошибке при двойном щелчке *Movies.sdf*, убедитесь, что вы установили [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(среда выполнения + средства поддержки). (Ссылки на программное обеспечение, см. список необходимых компонентов в первой части этого учебника ряда.) При установке выпуска теперь необходимо закрыть и снова открыть Visual Web Developer.


[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png "DB_explorer")](accessing-your-models-data-from-a-controller/_static/image13.png)

Существует две таблицы, один для `Movie` набора сущностей и затем `EdmMetadata` таблицы. `EdmMetadata` Таблицы используется платформой Entity Framework для определения, когда модель и база данных не синхронизированы.

Щелкните правой кнопкой мыши `Movies` таблицы и выберите **Показать таблицу данных** для просмотра данных, вы создали.

[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png "MoviesTable")](accessing-your-models-data-from-a-controller/_static/image15.png)

Щелкните правой кнопкой мыши `Movies` таблицы и выберите **изменить схему таблицы**.

[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png "EditTableSchema")](accessing-your-models-data-from-a-controller/_static/image17.png)

[![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image20.png "TableSchemaSM")](accessing-your-models-data-from-a-controller/_static/image19.png)

Обратите внимание как схема `Movies` таблицы сопоставляется `Movie` класса, созданного ранее. Entity Framework Code First автоматически создается эту схему на основе вашего `Movie` класса.

После завершения закройте соединение. (Если не закрывать соединение, может появиться ошибка очередном запуске проекта).

[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image22.png "CloseConnection")](accessing-your-models-data-from-a-controller/_static/image21.png)

Теперь у вас есть базы данных и страницу простого списка для отображения содержимого из него. В следующем уроке мы изучите остальной части кода формирования шаблонов и добавьте `SearchIndex` метод и `SearchIndex` представление, позволяющее искать фильмов в этой базе данных.

>[!div class="step-by-step"]
[Назад](adding-a-model.md)
[Вперед](examining-the-edit-methods-and-edit-view.md)
