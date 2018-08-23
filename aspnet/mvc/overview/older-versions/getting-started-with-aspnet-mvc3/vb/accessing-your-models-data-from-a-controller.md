---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
title: Доступ к данным модели из контроллера (VB) | Документация Майкрософт
author: Rick-Anderson
description: Этом учебнике описываются основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, который является...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: cad00de1-3c68-4ff4-a436-54236d449459
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 526f3b532a0b5c1c5c6d0cf8a9e17899300ca237
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836089"
---
<a name="accessing-your-models-data-from-a-controller-vb"></a>Доступ к данным модели из контроллера (VB)
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

> Этом учебнике описываются основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, которой является бесплатной версии Microsoft Visual Studio. Перед началом работы убедитесь, что вы установили необходимые компоненты, перечисленные ниже. Все из них можно установить, щелкнув следующую ссылку: [установщика веб-платформы](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Кроме того можно установить по отдельности необходимых компонентов, с помощью следующих ссылок:
> 
> - [Необходимые компоненты для Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Обновление средств ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(среды выполнения и средства поддержки)
> 
> Если вы используете Visual Studio 2010 вместо Visual Web Developer 2010, необходимо установить необходимые компоненты, щелкнув следующую ссылку: [необходимых компонентов Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Проект Visual Web Developer с VB.NET исходный код доступен на следующей странице в этом разделе. [Загрузить версию VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Если вы предпочитаете C#, переключитесь в [C# версии](../cs/accessing-your-models-data-from-a-controller.md) работы с этим руководством.


В этом разделе вы создадите новый `MoviesController` класса и написать код, который извлекает данные фильма и отображает его в браузере с помощью шаблона представления. Обязательно выполните сборку своего приложения, прежде чем продолжить.

Щелкните правой кнопкой мыши *контроллеров* папки и создайте новый `MoviesController` контроллера. Выберите следующие параметры:

- Имя контроллера: **MoviesController**. (Это значение по умолчанию).
- Шаблон: **контроллер с действиями чтения и записи и представлениями, использующий Entity Framework**.
- Класс модели: **Movie (MvcMovie.Models)**.
- Класс контекста данных: **MovieDBContext (MvcMovie.Models)**.
- Представления: **Razor (CSHTML)**. (По умолчанию).

[![5addMovieController](accessing-your-models-data-from-a-controller/_static/image2.png)](accessing-your-models-data-from-a-controller/_static/image1.png)

Нажмите кнопку **Добавить**. Visual Web Developer создает следующие файлы и папки:

- *MoviesController.vb* файл в проекте *контроллеров* папки.
- Объект *фильмы* папку в проекте *представления* папки.
- *CREATE.vbhtml Delete.vbhtml, Details.vbhtml, Edit.vbhtml*, и *Index.vbhtml* в новом *Views\Movies* папки.

[![5_ScaffoldMovie](accessing-your-models-data-from-a-controller/_static/image4.png)](accessing-your-models-data-from-a-controller/_static/image3.png)

Механизм формирования шаблонов ASP.NET MVC 3 автоматически создается CRUD (Создание, чтение, обновление и удаление) методы действий и представления для вас. Теперь у вас есть полнофункциональное веб-приложение, которое служит для создания, перечисления, редактирования и удаления записей фильмов.

Запустите приложение и перейдите к `Movies` контроллер, добавляя */Movies* на URL-адрес в адресной строке браузера. Так как приложение полагается на маршрутизацию по умолчанию (определенный в *Global.asax* файл), запрос браузера `http://localhost:xxxxx/Movies` направляется по умолчанию `Index` метод действия `Movies` контроллера. Другими словами, запрос браузера `http://localhost:xxxxx/Movies` так же, как запрос браузера `http://localhost:xxxxx/Movies/Index`. Результатом является пустой список фильмов, так как вы их еще не добавили.

![](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="creating-a-movie"></a>Создание фильма

Щелкните ссылку **Create New** (Создать). Введите некоторые сведения о фильм, а затем нажмите кнопку **создать** кнопки.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Щелкнув **создать** кнопка вызывает отправку формы на сервер, где сведения о фильме сохраняются в базе данных. Затем вы перейдете к */Movies* URL-адрес, где вы увидите только что созданном фильме в листинг.

[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png)](accessing-your-models-data-from-a-controller/_static/image7.png)

Создайте еще несколько записей фильмов. Попробуйте воспользоваться ссылками **Изменить**, **Сведения** и **Удалить** — все они работают.

## <a name="examining-the-generated-code"></a>Изучение созданного кода

Откройте *Controllers\MoviesController.vb* файла и изучите созданный `Index` метод. Часть контроллера movie с `Index` метод приведен ниже.

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample1.vb)]

В следующей строке из `MoviesController` класс создает экземпляр контекста базы данных фильмов, как описано выше. Контекст базы данных movie позволяет запрашивать, изменять и удалять элементы.

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample2.vb)]

Запрос на `Movies` контроллер возвращает все записи в `Movies` таблицы в базе данных фильмов, а затем передает результат в `Index` представления.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Строго типизированные модели и @model ключевое слово

Ранее в этом учебнике вы видели, как контроллер может передать данные или объекты в шаблон представления с помощью `ViewBag` объекта. `ViewBag` Является динамическим объектом, который предоставляет удобный способ для передачи информации в представление с поздним связыванием.

ASP.NET MVC также предоставляет возможность передачи строго типизированных данных или объекты для шаблона представления. Это строго типизированными подход позволяет лучше проверка во время компиляции кода и расширить возможности IntelliSense в редакторе Visual Web Developer. Мы используем этот подход с `MoviesController` класс и *Index.vbhtml* шаблона представления.

Обратите внимание на то, как в коде создается [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) объекта при вызове `View` вспомогательный метод в `Index` метода действия. Затем код передает это `Movies` списка из контроллера в представление:

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample3.vb)]

Включив `@ModelType` инструкция в верхней части файла шаблона представления, можно указать тип объекта, который будет ожидаться представлением. При создании контроллера movie Visual Web Developer автоматически включает следующий `@model` инструкция в верхней части *Index.vbhtml* файла:

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.vbhtml)]

Это `@ModelType` директива позволяет просматривать список фильмов, который контроллер передал в представление с помощью `Model` строго типизированного объекта. Например, в *Index.vbhtml* шаблона, код перебирает фильмы во время работы `foreach` инструкции для строго типизированного `Model` объекта:

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.vbhtml)]

Так как `Model` строго типизированного объекта (как `IEnumerable<Movie>` объекта), каждая `item` объект в цикле типизируется как `Movie`. Помимо прочих преимуществ это означает, что получение проверки кода во время компиляции и полная поддержка IntelliSense в редакторе кода:

[![5_Intellisense](accessing-your-models-data-from-a-controller/_static/image10.png)](accessing-your-models-data-from-a-controller/_static/image9.png)

## <a name="working-with-sql-server-compact"></a>Работа с SQL Server Compact

Entity Framework Code First обнаружил, что указывает строку подключения базы данных, который был предоставлен `Movies` базы данных, которая не существует, поэтому Code First создает базу данных автоматически. Убедитесь, что он создан путем поиска *приложения\_данных* папки. Если вы не видите *Movies.sdf* щелкните **Показать все файлы** кнопку в **обозревателе решений** панели инструментов нажмите кнопку **обновить** кнопки, а затем разверните *приложения\_данных* папки.

[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png)](accessing-your-models-data-from-a-controller/_static/image11.png)

Дважды щелкните *Movies.sdf* открыть **обозревателя серверов**. Затем разверните **таблиц** папки для просмотра таблиц, которые будут созданы в базе данных.

> [!NOTE]
> Если отобразится сообщение об ошибке при двойном щелчке *Movies.sdf*, убедитесь, что вы установили **средств Visual Studio 2010 с пакетом обновления 1 для SQL Server Compact 4.0**. (Ссылки на программное обеспечение, см. список необходимых компонентов в первой части этой серии руководств.) При установке выпуска теперь необходимо закрыть и снова открыть Visual Web Developer.


[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png)](accessing-your-models-data-from-a-controller/_static/image13.png)

Существует две таблицы, одна для `Movie` набор сущностей и затем `EdmMetadata` таблицы. `EdmMetadata` Таблица используется платформой Entity Framework, чтобы определить, когда модель и база данных не синхронизированы.

Щелкните правой кнопкой мыши `Movies` таблицы и выберите **Показать таблицу данных** для просмотра данных, вы создали.

[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png)](accessing-your-models-data-from-a-controller/_static/image15.png)

Щелкните правой кнопкой мыши `Movies` таблицы и выберите **изменить схему таблицы**.

[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png)](accessing-your-models-data-from-a-controller/_static/image17.png)

![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image19.png)

Обратите внимание, что как схема `Movies` таблицы сопоставляется `Movie` класс, созданный ранее. Entity Framework Code First автоматически создается эта схема на основе вашего `Movie` класса.

Когда закончите, закройте соединение. (Если не закрыть соединение, может появиться ошибка очередном запуске проекта).

[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image21.png)](accessing-your-models-data-from-a-controller/_static/image20.png)

Теперь у вас есть базы данных и странице простого списка для отображения содержимого из него. В следующем учебном курсе мы просмотреть остаток шаблонный код и добавить `SearchIndex` метод и `SearchIndex` представление, которое дает возможность поиска фильмов в этой базе данных.

> [!div class="step-by-step"]
> [Назад](adding-a-model.md)
> [Вперед](examining-the-edit-methods-and-edit-view.md)
