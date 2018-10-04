---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: Доступ к данным модели из контроллера | Документация Майкрософт
author: Rick-Anderson
description: 'Примечание: Обновленную версию этого учебника доступен здесь, использующий ASP.NET MVC 5 и Visual Studio 2013. Это более безопасное и гораздо проще выполнить и демонстрационных версий...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 6d4d2a5f30e55cc876632f9f1bf73ab534a702ad
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577409"
---
<a name="accessing-your-models-data-from-a-controller"></a>Доступ к данным модели из контроллера
====================
по [Рик Андерсон]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > Обновленную версию этого учебника доступен [здесь](../../getting-started/introduction/getting-started.md) , использующий ASP.NET MVC 5 и Visual Studio 2013. Она более безопасные, гораздо проще следовать и показаны дополнительные возможности.


В этом разделе вы создадите новый `MoviesController` класса и написать код, который извлекает данные фильма и отображает его в браузере с помощью шаблона представления.

**Постройте приложение** перед переходом к следующему шагу.

Щелкните правой кнопкой мыши *контроллеров* папки и создайте новый `MoviesController` контроллера. Перечисленные ниже параметры не будут отображаться до построения приложения. Выберите следующие параметры:

- Имя контроллера: **MoviesController**. (Это значение по умолчанию. )
- Шаблон: **контроллер MVC с действиями чтения и записи и представлениями, использующий Entity Framework**.
- Класс модели: **Movie (MvcMovie.Models)**.
- Класс контекста данных: **MovieDBContext (MvcMovie.Models)**.
- Представления: **Razor (CSHTML)**. (По умолчанию).

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

Нажмите кнопку **Добавить**. Visual Studio Express создает следующие файлы и папки:

- *MoviesController.cs* файл в проекте *контроллеров* папки.
- Объект *фильмы* папку в проекте *представления* папки.
- *CREATE.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, и *Index.cshtml* в новом *Views\Movies* папки.

ASP.NET MVC 4, автоматически создается CRUD (Создание, чтение, обновление и удаление) методы действий и представления для вас (автоматическое создание действия CRUD-методы и представления называется формированием шаблонов). Теперь у вас есть полнофункциональное веб-приложение, которое служит для создания, перечисления, редактирования и удаления записей фильмов.

Запустите приложение и перейдите к `Movies` контроллер, добавляя */Movies* на URL-адрес в адресной строке браузера. Так как приложение полагается на маршрутизацию по умолчанию (определенный в *Global.asax* файл), запрос браузера `http://localhost:xxxxx/Movies` направляется по умолчанию `Index` метод действия `Movies` контроллера. Другими словами, запрос браузера `http://localhost:xxxxx/Movies` так же, как запрос браузера `http://localhost:xxxxx/Movies/Index`. Результатом является пустой список фильмов, так как вы их еще не добавили.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a>Создание фильма

Щелкните ссылку **Create New** (Создать). Введите некоторые сведения о фильм, а затем нажмите кнопку **создать** кнопки.

![](accessing-your-models-data-from-a-controller/_static/image3.png)

Щелкнув **создать** кнопка вызывает отправку формы на сервер, где сведения о фильме сохраняются в базе данных. Затем вы перейдете к */Movies* URL-адрес, где вы увидите только что созданном фильме в листинг.

![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")

Создайте еще несколько записей фильмов. Попробуйте воспользоваться ссылками **Изменить**, **Сведения** и **Удалить** — все они работают.

## <a name="examining-the-generated-code"></a>Изучение созданного кода

Откройте *Controllers\MoviesController.cs* файла и изучите созданный `Index` метод. Часть контроллера movie с `Index` метод приведен ниже.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

В следующей строке из `MoviesController` класс создает экземпляр контекста базы данных фильмов, как описано выше. Контекст базы данных movie позволяет запрашивать, изменять и удалять элементы.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Запрос на `Movies` контроллер возвращает все записи в `Movies` таблицы в базе данных фильмов, а затем передает результат в `Index` представления.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Строго типизированные модели и @model ключевое слово

Ранее в этом учебнике вы видели, как контроллер может передать данные или объекты в шаблон представления с помощью `ViewBag` объекта. `ViewBag` Является динамическим объектом, который предоставляет удобный способ для передачи информации в представление с поздним связыванием.

ASP.NET MVC также предоставляет возможность передачи строго типизированных данных или объекты для шаблона представления. Это строго типизированными подход позволяет лучше проверка во время компиляции кода и расширить возможности IntelliSense в редакторе Visual Studio. Такой подход используется механизм формирования шаблонов в Visual Studio `MoviesController` класс и представление шаблонов при создании методов и представлений.

В *Controllers\MoviesController.cs* изучите созданный файл `Details` метод. Часть контроллера movie с `Details` метод приведен ниже.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

Если `Movie` находится экземпляр `Movie` модель передается в представление Details. Изучение содержимого *Views\Movies\Details.cshtml* файла.

Включив `@model` инструкция в верхней части файла шаблона представления, можно указать тип объекта, который будет ожидаться представлением. При создании контроллера movie Visual Studio автоматически включает следующий оператор `@model` в начало файла *Details.cshtml*:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

Эта директива `@model` обеспечивает доступ к фильму, который контроллер передал в представление с использованием строго типизированного объекта `Model`. Например, в *Details.cshtml* шаблона, код передает каждое поле фильма `DisplayNameFor` и [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) вспомогательных методов HTML со строго типизированным `Model` объекта. Методы создания и редактирования и шаблоны представлений также передать объект модели фильма.

Изучите *Index.cshtml* шаблон представления и `Index` метод в *MoviesController.cs* файла. Обратите внимание на то, как в коде создается [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) объекта при вызове `View` вспомогательный метод в `Index` метода действия. Затем код передает это `Movies` списка из контроллера в представление:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

При создании контроллера movie Visual Studio Express автоматически включает следующий `@model` инструкция в верхней части *Index.cshtml* файла:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Это `@model` директива позволяет просматривать список фильмов, который контроллер передал в представление с помощью `Model` строго типизированного объекта. Например, в *Index.cshtml* шаблона, код перебирает фильмы во время работы `foreach` инструкции для строго типизированного `Model` объекта:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

Так как `Model` строго типизированного объекта (как `IEnumerable<Movie>` объекта), каждая `item` объект в цикле типизируется как `Movie`. Помимо прочих преимуществ это означает, что получение проверки кода во время компиляции и полная поддержка IntelliSense в редакторе кода:

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a>Работа с SQL Server LocalDB

Entity Framework Code First обнаружил, что указывает строку подключения базы данных, который был предоставлен `Movies` базы данных, которая не существует, поэтому Code First создает базу данных автоматически. Убедитесь, что он создан путем поиска *приложения\_данных* папки. Если вы не видите *Movies.mdf* щелкните **Показать все файлы** кнопку в **обозревателе решений** панели инструментов нажмите кнопку **обновить** кнопки, а затем разверните *приложения\_данных* папки.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Дважды щелкните *Movies.mdf* открыть **ОБОЗРЕВАТЕЛЬ баз данных**, затем разверните **таблиц** папки для просмотра таблицы фильмы.

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")

> [!NOTE]
> Если не отображается в обозревателе базы данных, из **средства** меню, выберите **подключение к базе данных**, затем отменить **Выбор источника данных** диалоговое окно. Это приведет к принудительной откройте обозреватель баз данных.


> [!NOTE]
> Если вы используете материалов по VWD или Visual Studio 2010 и появляется сообщение об ошибке, аналогичную одно из следующих расположений:
> 
> - Базы данных "C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. MDF "не удается открыть, поскольку она имеет версию 706. Данный сервер поддерживает версию 655 и более ранних версий. Переход на предыдущую версию не поддерживается.
> - &quot;Исключение InvalidOperation было обработано пользовательским кодом&quot; переданном объекте SqlConnection не указан исходный каталог.
> 
> Необходимо установить [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) и [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0). Проверьте `MovieDBContext` строка подключения, указанная на предыдущей странице.


Щелкните правой кнопкой мыши `Movies` таблицы и выберите **Показать таблицу данных** для просмотра данных, вы создали.

![](accessing-your-models-data-from-a-controller/_static/image8.png)

Щелкните правой кнопкой мыши `Movies` таблицы и выберите **Открыть определение таблицы** для просмотра таблицы структуры, Entity Framework Code First создана автоматически.

![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")

![](accessing-your-models-data-from-a-controller/_static/image10.png)

Обратите внимание, что как схема `Movies` таблицы сопоставляется `Movie` класс, созданный ранее. Entity Framework Code First автоматически создается эта схема на основе вашего `Movie` класса.

Когда вы закончите, закрыть соединение, щелкнув правой кнопкой мыши *MovieDBContext* и выбрав **закрыть подключение**. (Если не закрыть соединение, может появиться ошибка очередном запуске проекта).

![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")

Теперь у вас есть базы данных и странице простого списка для отображения содержимого из него. В следующем учебном курсе мы просмотреть остаток шаблонный код и добавить `SearchIndex` метод и `SearchIndex` представление, которое дает возможность поиска фильмов в этой базе данных.

> [!div class="step-by-step"]
> [Назад](adding-a-model.md)
> [Вперед](examining-the-edit-methods-and-edit-view.md)
