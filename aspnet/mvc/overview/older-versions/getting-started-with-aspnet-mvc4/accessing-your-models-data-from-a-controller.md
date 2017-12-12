---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: "Доступ к вашей модели данных от контроллера | Документы Microsoft"
author: Rick-Anderson
description: "Примечание: Обновленную версию этого учебника доступен здесь, использующий ASP.NET MVC 5 и Visual Studio 2013. Это более безопасный, гораздо проще выполните и демонстрационных..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 82b4521279dcd9b9dc5a8e81b3a0d87ab26d46ac
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="accessing-your-models-data-from-a-controller"></a>Доступ к вашей модели данных из контроллера
====================
По [Рик Андерсон](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Доступна обновленная версия этого учебника [здесь](../../getting-started/introduction/getting-started.md) , с использованием ASP.NET MVC 5 и Visual Studio 2013. Он является более безопасны, выполните гораздо проще и показаны дополнительные возможности.


В этом разделе вы создадите новый `MoviesController` класса и написать код, который получает данные фильма и отображает его в браузере, с помощью шаблона представления.

**Постройте приложение** перед переходом к следующему шагу.

Щелкните правой кнопкой мыши *контроллеров* папки и создайте новый `MoviesController` контроллера. Следующие параметры не будут отображаться до построения приложения. Выберите следующие параметры:

- Имя контроллера: **MoviesController**. (Это значение по умолчанию. )
- Шаблон: **контроллер MVC с действиями чтения и записи и представлениями, использующий Entity Framework**.
- Класс модели: **фильма (MvcMovie.Models)**.
- Класс контекста данных: **MovieDBContext (MvcMovie.Models)**.
- Представления: **Razor (CSHTML)**. (По умолчанию).

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

Нажмите кнопку **Добавить**. Экспресс-выпуска Visual Studio создает следующие файлы и папки:

- *MoviesController.cs* файл в проекте *контроллеров* папки.
- Объект *фильмов* папки в проекте *представления* папки.
- *CREATE.cshtml Delete.cshtml, Details.cshtml, Edit.cshtml*, и *Index.cshtml* в новом *Views\Movies* папки.

ASP.NET MVC 4 автоматически создается CRUD (Создание, чтение, обновление и удаление) методы действий и представления автоматически (автоматическое создание CRUD методы действий и представления, называется формирование шаблонов). Теперь у вас есть полнофункциональную веб-приложение, позволяющее создавать, список, изменение и удаление записей фильма.

Запустите приложение и перейдите к `Movies` контроллера путем добавления */Movies* URL-адрес в адресной строке браузера. Так как приложение полагается на маршрутизацию по умолчанию (определенный в *Global.asax* файл), запрос браузера `http://localhost:xxxxx/Movies` направляется по умолчанию `Index` метод действия `Movies` контроллера. Другими словами, запрос браузера `http://localhost:xxxxx/Movies` фактически является таким же, как запрос браузера `http://localhost:xxxxx/Movies/Index`. Результат — пустой список фильмов, так как вы их еще не добавили.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a>Создание фильма

Щелкните ссылку **Create New** (Создать). Введите некоторые сведения о фильма и нажмите кнопку **создать** кнопки.

![](accessing-your-models-data-from-a-controller/_static/image3.png)

Щелкнув **создать** кнопка вызывает отправку формы на сервер, где фильма сведения сохраняются в базе данных. Затем можно будет перенаправлен на */Movies* URL-адрес, где можно просмотреть только что созданный фильма в списке.

![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")

Создайте еще несколько записей фильмов. Попробуйте воспользоваться ссылками **Изменить**, **Сведения** и **Удалить** — все они работают.

## <a name="examining-the-generated-code"></a>Изучение созданного кода

Откройте *Controllers\MoviesController.cs* файл и проверьте созданный `Index` метод. Часть контроллер фильма с `Index` метода показан ниже.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

В следующей строке из `MoviesController` класс создает экземпляры фильма контекста базы данных, как описано выше. Контекст базы данных фильма можно использовать для запроса, редактировать и удалять элементы.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Запрос на `Movies` возвращает все записи в `Movies` фильма базы данных, а затем передает результаты в `Index` представления.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Строго типизированные моделей и @model ключевое слово

Ранее в этом учебнике показано, как контроллер может передать объекты или данные шаблона представления с помощью `ViewBag` объекта. `ViewBag` Является динамическим объектом, который предоставляет удобный способ позднего связывания для передачи данных в представление.

ASP.NET MVC предоставляет также возможность передавать строго типизированные данные или объекты для шаблона представления. Это строго типизированными подход позволяет лучше проверка во время компиляции кода и обширные возможности IntelliSense в редакторе Visual Studio. Механизм формирования шаблонов в Visual Studio используется этот подход с `MoviesController` класс и представление шаблонов при создании методов и представления.

В *Controllers\MoviesController.cs* Проверьте созданный файл `Details` метод. Часть контроллер фильма с `Details` метода показан ниже.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

Если `Movie` находится экземпляр `Movie` модель передается в представлении сведений. Изучение содержимого *Views\Movies\Details.cshtml* файла.

Включив `@model` инструкции в верхней части представления файла шаблона, можно указать тип объекта, который ожидает представления. При создании контроллера movie Visual Studio автоматически включает следующий оператор `@model` в начало файла *Details.cshtml*:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

Эта директива `@model` обеспечивает доступ к фильму, который контроллер передал в представление с использованием строго типизированного объекта `Model`. Например, в *Details.cshtml* шаблона, код передает каждого фильма поля для `DisplayNameFor` и [DisplayFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) вспомогательных методов HTML со строгой типизацией `Model` объекта. Создание и редактирование методов и шаблонов представлений также передать объект модели фильма.

Изучите *Index.cshtml* Просмотр шаблона и `Index` метод в *MoviesController.cs* файла. Обратите внимание на то, как код создает [ `List` ](https://msdn.microsoft.com/en-us/library/6sh2ey19.aspx) объект при вызове `View` вспомогательный метод `Index` метода действия. Затем код передает это `Movies` список из контроллера в представление:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

При создании контроллера фильм, экспресс-выпуска Visual Studio автоматически включаются следующие `@model` инструкции в верхней части *Index.cshtml* файла:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Это `@model` директива позволяет получить доступ к списку фильмы, контроллера передается в представление, с помощью `Model` объекта со строгим типом. Например, в *Index.cshtml* шаблона, код выполняет цикл по фильмы, выполняя `foreach` инструкции через строго типизированный `Model` объекта:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

Поскольку `Model` объект является типобезопасным (как `IEnumerable<Movie>` объекта), каждая `item` объект в цикле типизируется как `Movie`. Помимо прочих преимуществ это означает получение проверки кода во время компиляции и полная поддержка технологии IntelliSense в редакторе кода.

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a>Работа с SQL Server LocalDB

Entity Framework Code First обнаружил, что указывает строку подключения базы данных, который был предоставлен `Movies` базы данных, которая не существует, поэтому Code First база данных создана автоматически. Убедитесь, что он был создан путем поиска в *приложения\_данные* папки. Если вы не видите *Movies.mdf* файла, нажмите кнопку **Показать все файлы** кнопку в **обозревателе решений** инструментов, нажмите кнопку **обновление** кнопку, а затем разверните *приложения\_данные* папки.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Дважды щелкните *Movies.mdf* Открытие **ОБОЗРЕВАТЕЛЬ баз данных**, затем разверните **таблиц** папку для просмотра фильмов таблицы.

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")

> [!NOTE]
> Если не отображается в обозревателе базы данных, из **средства** последовательно выберите пункты **подключение к базе данных**, затем отменить **Выбор источника данных** диалогового окна. Это заставит откройте обозреватель баз данных.


> [!NOTE]
> Если вы используете VWD или Visual Studio 2010 и появляется сообщение об ошибке похожи на любую из следующих действий:
> 
> - Базы данных "C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. MDF "не удается открыть, поскольку она имеет версию 706. Данный сервер поддерживает версию 655 и более ранних версий. Переход на предыдущую версию не поддерживается.
> - &quot;InvalidOperation исключение было обработано пользовательским кодом&quot; переданном объекте SqlConnection не указан исходный каталог.
> 
> Необходимо установить [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) и [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0). Проверьте `MovieDBContext` строку подключения, заданную на предыдущей странице.


Щелкните правой кнопкой мыши `Movies` таблицы и выберите **Показать таблицу данных** для просмотра данных, вы создали.

![](accessing-your-models-data-from-a-controller/_static/image8.png)

Щелкните правой кнопкой мыши `Movies` таблицы и выберите **Открыть определение таблицы** для просмотра структуры, Entity Framework Code First создаются таблицы.

![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")

![](accessing-your-models-data-from-a-controller/_static/image10.png)

Обратите внимание как схема `Movies` таблицы сопоставляется `Movie` класса, созданного ранее. Entity Framework Code First автоматически создается эту схему на основе вашего `Movie` класса.

После завершения закрыть подключение, щелкните правой кнопкой мыши *MovieDBContext* и выбрав **закрыть подключение**. (Если не закрывать соединение, может появиться ошибка очередном запуске проекта).

![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")

Теперь у вас есть базы данных и страницу простого списка для отображения содержимого из него. В следующем уроке мы изучите остальной части кода формирования шаблонов и добавьте `SearchIndex` метод и `SearchIndex` представление, позволяющее искать фильмов в этой базе данных.

>[!div class="step-by-step"]
[Назад](adding-a-model.md)
[Вперед](examining-the-edit-methods-and-edit-view.md)
