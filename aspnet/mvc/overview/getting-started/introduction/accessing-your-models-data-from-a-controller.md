---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Доступ к данным модели из контроллера | Документация Майкрософт
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 16f4a61d048e90b7962f038e90e0721883726c44
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835739"
---
<a name="accessing-your-models-data-from-a-controller"></a>Доступ к данным модели из контроллера
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

В этом разделе вы создадите новый `MoviesController` класса и написать код, который извлекает данные фильма и отображает его в браузере с помощью шаблона представления.

**Постройте приложение** перед переходом к следующему шагу. Если вы не создадите приложение, вы получите ошибку при добавлении контроллера.

В обозревателе решений щелкните правой кнопкой мыши *контроллеров* папку и нажмите кнопку **добавить**, затем **контроллера**.

![](accessing-your-models-data-from-a-controller/_static/image1.png)

В **Добавление шаблона** диалоговом окне щелкните **контроллер MVC 5 с представлениями, использующий Entity Framework**, а затем нажмите кнопку **добавить**.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- Выберите **Movie (MvcMovie.Models)** класс модели.
- Выберите **MovieDBContext (MvcMovie.Models)** для класса контекста данных.
- Имя контроллера введите **MoviesController**.

  На следующем рисунке показано завершенное диалоговое окно.  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

Нажмите кнопку **Добавить**. (Если отобразится сообщение об ошибке, возможно, не была построена приложения перед началом добавления контроллера.) Visual Studio создает следующие файлы и папки:

- *MoviesController.cs* файл *контроллеров* папки.
- Объект *Views\Movies* папки.
- *CREATE.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, и *Index.cshtml* в новом *Views\Movies* папки.

Visual Studio автоматически создается [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (Создание, чтение, обновление и удаление) методы действий и представления для вас (автоматическое создание действия CRUD-методы и представления называется формированием шаблонов). Теперь у вас есть полнофункциональное веб-приложение, которое служит для создания, перечисления, редактирования и удаления записей фильмов.

Запустите приложение и щелкнуть **MVC Movie** ссылку (или перейдите к `Movies` контроллер, добавляя */Movies* на URL-адрес в адресной строке браузера). Так как приложение полагается на маршрутизацию по умолчанию (определенный в *приложения\_Start\RouteConfig.cs* файл), запрос браузера `http://localhost:xxxxx/Movies` направляется по умолчанию `Index` метод действия `Movies` контроллера. Другими словами, запрос браузера `http://localhost:xxxxx/Movies` так же, как запрос браузера `http://localhost:xxxxx/Movies/Index`. Результатом является пустой список фильмов, так как вы их еще не добавили.

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a>Создание фильма

Щелкните ссылку **Create New** (Создать). Введите некоторые сведения о фильм, а затем нажмите кнопку **создать** кнопки.


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> Вы не сможете вводить десятичные точки или запятые в поле «Цена». Поддержка проверки jQuery для английского, используйте запятую (&quot;,&quot;) для десятичного разделителя и форматов даты неанглийские США, необходимо включить *globalize.js* и конкретными  *cultures/globalize.cultures.js* файла (из [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) и JavaScript, чтобы использовать `Globalize.parseFloat`. Я покажу, как это сделать в следующем учебном курсе. А пока вводите целые числа, такие как 10.


Щелкнув **создать** кнопка вызывает отправку формы на сервер, где сведения о фильме сохраняются в базе данных. Затем вы перейдете к */Movies* URL-адрес, где вы увидите только что созданном фильме в листинг.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Создайте еще несколько записей фильмов. Попробуйте воспользоваться ссылками **Изменить**, **Сведения** и **Удалить** — все они работают.

## <a name="examining-the-generated-code"></a>Изучение созданного кода

Откройте *Controllers\MoviesController.cs* файла и изучите созданный `Index` метод. Часть контроллера movie с `Index` метод приведен ниже.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Запрос на `Movies` контроллер возвращает все записи в `Movies` таблицу и затем передает результаты в `Index` представления. В следующей строке из `MoviesController` класс создает экземпляр контекста базы данных фильмов, как описано выше. Контекст базы данных movie позволяет запрашивать, изменять и удалять элементы.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a>Строго типизированные модели и @model ключевое слово

Ранее в этом учебнике вы видели, как контроллер может передать данные или объекты в шаблон представления с помощью `ViewBag` объекта. `ViewBag` Является динамическим объектом, который предоставляет удобный способ для передачи информации в представление с поздним связыванием.

MVC также предоставляет возможность передавать *строго* типизированные объекты шаблона представления. Такой подход строго типизированные позволяет лучше компиляции во время проверки кода и более широкие [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) в редакторе Visual Studio. Этот подход используется механизм формирования шаблонов в Visual Studio (то есть передача *строго* типизированной модели) с `MoviesController` класс и представление шаблонов при создании методов и представлений.

В *Controllers\MoviesController.cs* изучите созданный файл `Details` метод. `Details` Метод приведен ниже.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

`id` Параметр обычно передается в качестве данных маршрута, например `http://localhost:1234/movies/details/1` задаст контроллер к контроллеру фильмов, действие `details` и `id` 1. Можно также передавать в идентификаторе со строкой запроса следующим образом:

`http://localhost:1234/movies/details?id=1`

Если `Movie` находится экземпляр `Movie` модель передается `Details` представления:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

Изучение содержимого *Views\Movies\Details.cshtml* файла:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

Включив `@model` инструкция в верхней части файла шаблона представления, можно указать тип объекта, который будет ожидаться представлением. При создании контроллера movie Visual Studio автоматически включает следующий оператор `@model` в начало файла *Details.cshtml*:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Эта директива `@model` обеспечивает доступ к фильму, который контроллер передал в представление с использованием строго типизированного объекта `Model`. Например, в *Details.cshtml* шаблона, код передает каждое поле фильма `DisplayNameFor` и [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) вспомогательных методов HTML со строго типизированным `Model` объекта. `Create` И `Edit` методы и шаблоны представлений также передать объект модели фильма.

Изучите *Index.cshtml* шаблон представления и `Index` метод в *MoviesController.cs* файла. Обратите внимание на то, как в коде создается [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) объекта при вызове `View` вспомогательный метод в `Index` метода действия. Затем код передает это `Movies` список `Index` метода действия в представление:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

При создании контроллера movie Visual Studio автоматически включает следующий `@model` инструкция в верхней части *Index.cshtml* файла:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

Это `@model` директива позволяет просматривать список фильмов, который контроллер передал в представление с помощью `Model` строго типизированного объекта. Например, в *Index.cshtml* шаблона, код перебирает фильмы во время работы `foreach` инструкции для строго типизированного `Model` объекта:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

Так как `Model` строго типизированного объекта (как `IEnumerable<Movie>` объекта), каждая `item` объект в цикле типизируется как `Movie`. Помимо прочих преимуществ это означает, что получение проверки кода во время компиляции и полная поддержка IntelliSense в редакторе кода:

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a>Работа с SQL Server LocalDB

Entity Framework Code First обнаружил, что указывает строку подключения базы данных, который был предоставлен `Movies` базы данных, которая не существует, поэтому Code First создает базу данных автоматически. Убедитесь, что он создан путем поиска *приложения\_данных* папки. Если вы не видите *Movies.mdf* щелкните **Показать все файлы** кнопку в **обозревателе решений** панели инструментов нажмите кнопку **обновить** кнопки, а затем разверните *приложения\_данных* папки.

![](accessing-your-models-data-from-a-controller/_static/image9.png)

Дважды щелкните *Movies.mdf* открыть **ОБОЗРЕВАТЕЛЯ СЕРВЕРОВ**, затем разверните **таблиц** папки для просмотра таблицы фильмы. Обратите внимание на значок ключа рядом с идентификатором. По умолчанию EF сделает свойство с именем идентификатор первичного ключа. Дополнительные сведения о EF и MVC, см. в разделе отличную руководстве том Дайкстра на [MVC и EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")

Щелкните правой кнопкой мыши `Movies` таблицы и выберите **Показать таблицу данных** для просмотра данных, вы создали.

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

Щелкните правой кнопкой мыши `Movies` таблицы и выберите **Открыть определение таблицы** для просмотра таблицы структуры, Entity Framework Code First создана автоматически.

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

Обратите внимание, что как схема `Movies` таблицы сопоставляется `Movie` класс, созданный ранее. Entity Framework Code First автоматически создается эта схема на основе вашего `Movie` класса.

Когда вы закончите, закрыть соединение, щелкнув правой кнопкой мыши *MovieDBContext* и выбрав **закрыть подключение**. (Если не закрыть соединение, может появиться ошибка очередном запуске проекта).

![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")

Теперь у вас есть база данных и страницы для отображения, редактирования, обновления и удаления данных. В следующем учебном курсе мы просмотреть остаток шаблонный код и добавить `SearchIndex` метод и `SearchIndex` представление, которое дает возможность поиска фильмов в этой базе данных. Дополнительные сведения об использовании Entity Framework с MVC см. в разделе [Создание модели данных Entity Framework для приложения ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

> [!div class="step-by-step"]
> [Назад](creating-a-connection-string.md)
> [Вперед](examining-the-edit-methods-and-edit-view.md)
