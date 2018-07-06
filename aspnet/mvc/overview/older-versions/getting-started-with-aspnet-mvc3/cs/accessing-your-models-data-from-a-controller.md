---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/accessing-your-models-data-from-a-controller
title: Доступ к данным модели из контроллера (C#) | Документация Майкрософт
author: Rick-Anderson
description: Этом учебнике описываются основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, который является...
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: 002ada5c-f114-47ab-a441-57dbdb728ea0
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: a1a39cf06b7ab9109117e89051c8adf47062a926
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805617"
---
<a name="accessing-your-models-data-from-a-controller-c"></a>Доступ к данным модели из контроллера (C#)
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Обновленную версию этого учебника доступен [здесь](../../../getting-started/introduction/getting-started.md) , использующий ASP.NET MVC 5 и Visual Studio 2013. Она более безопасные, гораздо проще следовать и показаны дополнительные возможности.
> 
> 
> Этом учебнике описываются основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, которой является бесплатной версии Microsoft Visual Studio. Перед началом работы убедитесь, что вы установили необходимые компоненты, перечисленные ниже. Все из них можно установить, щелкнув следующую ссылку: [установщика веб-платформы](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Кроме того можно установить по отдельности необходимых компонентов, с помощью следующих ссылок:
> 
> - [Необходимые компоненты для Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Обновление средств ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(среды выполнения и средства поддержки)
> 
> Если вы используете Visual Studio 2010 вместо Visual Web Developer 2010, необходимо установить необходимые компоненты, щелкнув следующую ссылку: [необходимых компонентов Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Проект Visual Web Developer с исходным кодом C# — прилагаются в этом разделе. [Загрузить версию C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Если вы предпочитаете Visual Basic, переключитесь в [версии Visual Basic](../vb/intro-to-aspnet-mvc-3.md) работы с этим руководством.

В этом разделе вы создадите новый `MoviesController` класса и написать код, который извлекает данные фильма и отображает его в браузере с помощью шаблона представления. Обязательно выполните сборку своего приложения, прежде чем продолжить.

Щелкните правой кнопкой мыши *контроллеров* папки и создайте новый `MoviesController` контроллера. Выберите следующие параметры:

- Имя контроллера: **MoviesController**. (Это значение по умолчанию. )
- Шаблон: **контроллер с действиями чтения и записи и представлениями, использующий Entity Framework**.
- Класс модели: **Movie (MvcMovie.Models)**.
- Класс контекста данных: **MovieDBContext (MvcMovie.Models)**.
- Представления: **Razor (CSHTML)**. (По умолчанию).

[![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image2.png "AddScaffoldedMovieController")](accessing-your-models-data-from-a-controller/_static/image1.png)

Нажмите кнопку **Добавить**. Visual Web Developer создает следующие файлы и папки:

- *MoviesController.cs* файл в проекте *контроллеров* папки.
- Объект *фильмы* папку в проекте *представления* папки.
- *CREATE.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, и *Index.cshtml* в новом *Views\Movies* папки.

[![NewMovieControllerScreenShot](accessing-your-models-data-from-a-controller/_static/image4.png "NewMovieControllerScreenShot")](accessing-your-models-data-from-a-controller/_static/image3.png)

Механизм формирования шаблонов ASP.NET MVC 3 автоматически создается CRUD (Создание, чтение, обновление и удаление) методы действий и представления для вас. Теперь у вас есть полнофункциональное веб-приложение, которое служит для создания, перечисления, редактирования и удаления записей фильмов.

Запустите приложение и перейдите к `Movies` контроллер, добавляя */Movies* на URL-адрес в адресной строке браузера. Так как приложение полагается на маршрутизацию по умолчанию (определенный в *Global.asax* файл), запрос браузера `http://localhost:xxxxx/Movies` направляется по умолчанию `Index` метод действия `Movies` контроллера. Другими словами, запрос браузера `http://localhost:xxxxx/Movies` так же, как запрос браузера `http://localhost:xxxxx/Movies/Index`. Результатом является пустой список фильмов, так как вы их еще не добавили.

![](accessing-your-models-data-from-a-controller/_static/image5.png)

### <a name="creating-a-movie"></a>Создание фильма

Щелкните ссылку **Create New** (Создать). Введите некоторые сведения о фильм, а затем нажмите кнопку **создать** кнопки.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Щелкнув **создать** кнопка вызывает отправку формы на сервер, где сведения о фильме сохраняются в базе данных. Затем вы перейдете к */Movies* URL-адрес, где вы увидите только что созданном фильме в листинг.

[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png "IndexWhenHarryMet")](accessing-your-models-data-from-a-controller/_static/image7.png)

Создайте еще несколько записей фильмов. Попробуйте воспользоваться ссылками **Изменить**, **Сведения** и **Удалить** — все они работают.

## <a name="examining-the-generated-code"></a>Изучение созданного кода

Откройте *Controllers\MoviesController.cs* файла и изучите созданный `Index` метод. Часть контроллера movie с `Index` метод приведен ниже.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

В следующей строке из `MoviesController` класс создает экземпляр контекста базы данных фильмов, как описано выше. Контекст базы данных movie позволяет запрашивать, изменять и удалять элементы.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Запрос на `Movies` контроллер возвращает все записи в `Movies` таблицы в базе данных фильмов, а затем передает результат в `Index` представления.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Строго типизированные модели и @model ключевое слово

Ранее в этом учебнике вы видели, как контроллер может передать данные или объекты в шаблон представления с помощью `ViewBag` объекта. `ViewBag` Является динамическим объектом, который предоставляет удобный способ для передачи информации в представление с поздним связыванием.

ASP.NET MVC также предоставляет возможность передачи строго типизированных данных или объекты для шаблона представления. Это строго типизированными подход позволяет лучше проверка во время компиляции кода и расширить возможности IntelliSense в редакторе Visual Web Developer. Мы используем этот подход с `MoviesController` класс и *Index.cshtml* шаблона представления.

Обратите внимание на то, как в коде создается [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) объекта при вызове `View` вспомогательный метод в `Index` метода действия. Затем код передает это `Movies` списка из контроллера в представление:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

Включив `@model` инструкция в верхней части файла шаблона представления, можно указать тип объекта, который будет ожидаться представлением. При создании контроллера movie Visual Web Developer автоматически включает следующий `@model` инструкция в верхней части *Index.cshtml* файла:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

Это `@model` директива позволяет просматривать список фильмов, который контроллер передал в представление с помощью `Model` строго типизированного объекта. Например, в *Index.cshtml* шаблона, код перебирает фильмы во время работы `foreach` инструкции для строго типизированного `Model` объекта:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml)]

Так как `Model` строго типизированного объекта (как `IEnumerable<Movie>` объекта), каждая `item` объект в цикле типизируется как `Movie`. Помимо прочих преимуществ это означает, что получение проверки кода во время компиляции и полная поддержка IntelliSense в редакторе кода:

[![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image10.png "ModelIntellisene")](accessing-your-models-data-from-a-controller/_static/image9.png)

## <a name="working-with-sql-server-compact"></a>Работа с SQL Server Compact

Entity Framework Code First обнаружил, что указывает строку подключения базы данных, который был предоставлен `Movies` базы данных, которая не существует, поэтому Code First создает базу данных автоматически. Убедитесь, что он создан путем поиска *приложения\_данных* папки. Если вы не видите *Movies.sdf* щелкните **Показать все файлы** кнопку в **обозревателе решений** панели инструментов нажмите кнопку **обновить** кнопки, а затем разверните *приложения\_данных* папки.

[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png "SDF_in_SolnExp")](accessing-your-models-data-from-a-controller/_static/image11.png)

Дважды щелкните *Movies.sdf* открыть **обозревателя серверов**. Затем разверните **таблиц** папки для просмотра таблиц, которые будут созданы в базе данных.

> [!NOTE]
> Если отобразится сообщение об ошибке при двойном щелчке *Movies.sdf*, убедитесь, что вы установили [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(среды выполнения и средства поддержки). (Ссылки на программное обеспечение, см. список необходимых компонентов в первой части этой серии руководств.) При установке выпуска теперь необходимо закрыть и снова открыть Visual Web Developer.


[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png "DB_explorer")](accessing-your-models-data-from-a-controller/_static/image13.png)

Существует две таблицы, одна для `Movie` набор сущностей и затем `EdmMetadata` таблицы. `EdmMetadata` Таблица используется платформой Entity Framework, чтобы определить, когда модель и база данных не синхронизированы.

Щелкните правой кнопкой мыши `Movies` таблицы и выберите **Показать таблицу данных** для просмотра данных, вы создали.

[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png "MoviesTable")](accessing-your-models-data-from-a-controller/_static/image15.png)

Щелкните правой кнопкой мыши `Movies` таблицы и выберите **изменить схему таблицы**.

[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png "EditTableSchema")](accessing-your-models-data-from-a-controller/_static/image17.png)

[![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image20.png "TableSchemaSM")](accessing-your-models-data-from-a-controller/_static/image19.png)

Обратите внимание, что как схема `Movies` таблицы сопоставляется `Movie` класс, созданный ранее. Entity Framework Code First автоматически создается эта схема на основе вашего `Movie` класса.

Когда закончите, закройте соединение. (Если не закрыть соединение, может появиться ошибка очередном запуске проекта).

[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image22.png "CloseConnection")](accessing-your-models-data-from-a-controller/_static/image21.png)

Теперь у вас есть базы данных и странице простого списка для отображения содержимого из него. В следующем учебном курсе мы просмотреть остаток шаблонный код и добавить `SearchIndex` метод и `SearchIndex` представление, которое дает возможность поиска фильмов в этой базе данных.

> [!div class="step-by-step"]
> [Назад](adding-a-model.md)
> [Вперед](examining-the-edit-methods-and-edit-view.md)
