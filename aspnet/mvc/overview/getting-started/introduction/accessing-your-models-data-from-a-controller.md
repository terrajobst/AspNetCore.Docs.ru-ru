---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Доступ к вашей модели данных от контроллера | Документы Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: d3dfa079c334e04f368531456ec2ec4e9728f893
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873561"
---
<a name="accessing-your-models-data-from-a-controller"></a>Доступ к вашей модели данных из контроллера
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

В этом разделе вы создадите новый `MoviesController` класса и написать код, который получает данные фильма и отображает его в браузере, с помощью шаблона представления.

**Постройте приложение** перед переходом к следующему шагу. Если не построить приложение, вы получите ошибку при добавлении контроллера.

В обозревателе решений щелкните правой кнопкой мыши *контроллеров* папку и нажмите кнопку **добавить**, затем **контроллера**.

![](accessing-your-models-data-from-a-controller/_static/image1.png)

В **Добавление формирования шаблонов** диалоговое окно, нажмите кнопку **контроллер MVC 5 с представлениями, использующий Entity Framework**, а затем нажмите кнопку **добавить**.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- Выберите **фильма (MvcMovie.Models)** для класса модели.
- Выберите **MovieDBContext (MvcMovie.Models)** для класса контекста данных.
- Имя контроллера введите **MoviesController**.

  На следующем рисунке показано диалоговое окно завершенной.  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

Нажмите кнопку **Добавить**. (Если появляется ошибка, возможно, не была построена приложения перед началом добавления контроллера.) Visual Studio создает следующие файлы и папки:

- *MoviesController.cs* файла в *контроллеров* папки.
- Объект *Views\Movies* папки.
- *CREATE.cshtml Delete.cshtml, Details.cshtml, Edit.cshtml*, и *Index.cshtml* в новом *Views\Movies* папки.

Visual Studio автоматически создается [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (Создание, чтение, обновление и удаление) методы действий и представления автоматически (автоматическое создание CRUD методы действий и представления, называется формирование шаблонов). Теперь у вас есть полнофункциональную веб-приложение, позволяющее создавать, список, изменение и удаление записей фильма.

Запустите приложение и щелкнуть **фильма MVC** ссылки (или перейти к `Movies` контроллера путем добавления */Movies* URL-адрес в адресной строке браузера). Так как приложение полагается на маршрутизацию по умолчанию (определенные в *приложения\_Start\RouteConfig.cs* файл), запрос браузера `http://localhost:xxxxx/Movies` направляется по умолчанию `Index` методдействия`Movies` контроллера. Другими словами, запрос браузера `http://localhost:xxxxx/Movies` фактически является таким же, как запрос браузера `http://localhost:xxxxx/Movies/Index`. Результат — пустой список фильмов, так как вы их еще не добавили.

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a>Создание фильма

Щелкните ссылку **Create New** (Создать). Введите некоторые сведения о фильма и нажмите кнопку **создать** кнопки.


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> Вы не сможете ввести в поле «Цена» десятичной точки и запятые. Поддержка проверки jQuery для языков, кроме английского, используйте запятую (&quot;,&quot;) для десятичной запятой и форматы дат локализованной США, необходимо включить *globalize.js* и конкретных  *cultures/globalize.cultures.js* файла (из [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) и JavaScript, чтобы использовать `Globalize.parseFloat`. Я покажу, как это сделать в следующем уроке. А пока вводите целые числа, такие как 10.


Щелкнув **создать** кнопка вызывает отправку формы на сервер, где фильма сведения сохраняются в базе данных. Затем можно будет перенаправлен на */Movies* URL-адрес, где можно просмотреть только что созданный фильма в списке.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Создайте еще несколько записей фильмов. Попробуйте воспользоваться ссылками **Изменить**, **Сведения** и **Удалить** — все они работают.

## <a name="examining-the-generated-code"></a>Изучение созданного кода

Откройте *Controllers\MoviesController.cs* файл и проверьте созданный `Index` метод. Часть контроллер фильма с `Index` метода показан ниже.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Запрос на `Movies` возвращает все записи в `Movies` таблицы, а затем передает результаты в `Index` представления. В следующей строке из `MoviesController` класс создает экземпляры фильма контекста базы данных, как описано выше. Контекст базы данных фильма можно использовать для запроса, редактировать и удалять элементы.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a>Строго типизированные моделей и @model ключевое слово

Ранее в этом учебнике показано, как контроллер может передать объекты или данные шаблона представления с помощью `ViewBag` объекта. `ViewBag` Является динамическим объектом, который предоставляет удобный способ позднего связывания для передачи данных в представление.

MVC также предоставляет возможность передавать *строго* типизированные объекты шаблона представления. Такой подход строго типизированные позволяет лучше во время компиляции выполняется проверка кода и расширенные [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) в редакторе Visual Studio. Этот подход используется механизм формирования шаблонов в Visual Studio (т. е передачи *строго* типизированную модель) с `MoviesController` класса и представление шаблонов при создании методов и представления.

В *Controllers\MoviesController.cs* Проверьте созданный файл `Details` метод. `Details` Метода показан ниже.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

`id` Параметра обычно передается в качестве данных о маршруте, например `http://localhost:1234/movies/details/1` установит контроллера к контроллеру фильма действие `details` и `id` значение 1. Можно также передавать в идентификаторе со строкой запроса следующим образом:

`http://localhost:1234/movies/details?id=1`

Если `Movie` находится экземпляр `Movie` модели передается `Details` представления:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

Изучение содержимого *Views\Movies\Details.cshtml* файла:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

Включив `@model` инструкции в верхней части представления файла шаблона, можно указать тип объекта, который ожидает представления. При создании контроллера movie Visual Studio автоматически включает следующий оператор `@model` в начало файла *Details.cshtml*:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Эта директива `@model` обеспечивает доступ к фильму, который контроллер передал в представление с использованием строго типизированного объекта `Model`. Например, в *Details.cshtml* шаблона, код передает каждого фильма поля для `DisplayNameFor` и [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) вспомогательных методов HTML со строгой типизацией `Model` объекта. `Create` И `Edit` методы и просмотреть шаблоны также передать объект модели фильма.

Изучите *Index.cshtml* Просмотр шаблона и `Index` метод в *MoviesController.cs* файла. Обратите внимание на то, как код создает [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) объект при вызове `View` вспомогательный метод `Index` метода действия. Затем код передает это `Movies` список `Index` метода действия к представлению:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

При создании контроллера фильм, Visual Studio автоматически включается следующее `@model` инструкции в верхней части *Index.cshtml* файла:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

Это `@model` директива позволяет получить доступ к списку фильмы, контроллера передается в представление, с помощью `Model` объекта со строгим типом. Например, в *Index.cshtml* шаблона, код выполняет цикл по фильмы, выполняя `foreach` инструкции через строго типизированный `Model` объекта:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

Поскольку `Model` объект является типобезопасным (как `IEnumerable<Movie>` объекта), каждая `item` объект в цикле типизируется как `Movie`. Помимо прочих преимуществ это означает получение проверки кода во время компиляции и полная поддержка технологии IntelliSense в редакторе кода.

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a>Работа с SQL Server LocalDB

Entity Framework Code First обнаружил, что указывает строку подключения базы данных, который был предоставлен `Movies` базы данных, которая не существует, поэтому Code First база данных создана автоматически. Убедитесь, что он был создан путем поиска в *приложения\_данные* папки. Если вы не видите *Movies.mdf* файла, нажмите кнопку **Показать все файлы** кнопку в **обозревателе решений** инструментов, нажмите кнопку **обновление** кнопку, а затем разверните *приложения\_данные* папки.

![](accessing-your-models-data-from-a-controller/_static/image9.png)

Дважды щелкните *Movies.mdf* Открытие **ОБОЗРЕВАТЕЛЯ СЕРВЕРОВ**, затем разверните **таблиц** папку для просмотра фильмов таблицы. Обратите внимание на значок ключа рядом с идентификатором. По умолчанию EF сделает свойство с именем идентификатор первичного ключа. Дополнительные сведения о EF и MVC см. Учебник отлично Tom Dykstra на [MVC и EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")

Щелкните правой кнопкой мыши `Movies` таблицы и выберите **Показать таблицу данных** для просмотра данных, вы создали.

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

Щелкните правой кнопкой мыши `Movies` таблицы и выберите **Открыть определение таблицы** для просмотра структуры, Entity Framework Code First создаются таблицы.

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

Обратите внимание как схема `Movies` таблицы сопоставляется `Movie` класса, созданного ранее. Entity Framework Code First автоматически создается эту схему на основе вашего `Movie` класса.

После завершения закрыть подключение, щелкните правой кнопкой мыши *MovieDBContext* и выбрав **закрыть подключение**. (Если не закрывать соединение, может появиться ошибка очередном запуске проекта).

![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")

Теперь у вас есть база данных и страницы для отображения, редактирования, обновления и удаления данных. В следующем уроке мы изучите остальной части кода формирования шаблонов и добавьте `SearchIndex` метод и `SearchIndex` представление, позволяющее искать фильмов в этой базе данных. Дополнительные сведения об использовании платформы Entity Framework с MVC см. в разделе [Создание модели данных Entity Framework для приложения ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

> [!div class="step-by-step"]
> [Назад](creating-a-connection-string.md)
> [Вперед](examining-the-edit-methods-and-edit-view.md)
