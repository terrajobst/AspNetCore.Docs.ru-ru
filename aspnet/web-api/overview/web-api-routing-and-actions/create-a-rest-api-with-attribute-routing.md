---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: Создание REST API с помощью маршрутизации в ASP.NET Web API 2 с помощью атрибутов | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2013
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: da3ca8f89f823fcb2c4ab74af6ddf4f61d4e663a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829821"
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a>Создание REST API с помощью маршрутизации атрибутов в ASP.NET Web API 2
====================
по [Майк Уоссон](https://github.com/MikeWasson)

Веб-API 2 поддерживает новый тип маршрутизации, вызывается *маршрутизации с помощью атрибутов*. Общие сведения о маршрутизации с помощью атрибутов, см. в разделе [маршрутизации с помощью атрибутов в веб-API 2](attribute-routing-in-web-api-2.md). В этом руководстве используется маршрутизация с помощью атрибутов для создания API REST для коллекции книг. API-Интерфейс поддерживает следующие действия:

| Действие | Пример URI |
| --- | --- |
| Получение списка всех книг. | / api/книг |
| Получите книгу по идентификатору. | /API/Books/1 |
| Получение сведений о книге. | /API/Books/1/Details |
| Получение списка книг по жанру. | /API/Books/FANTASY |
| Получение списка книг по дате публикации. | /API/Books/Date/2013-02-16 /api/books/date/2013/02/16 (альтернативный способ) |
| Получение списка книг определенного автора. | /API/authors/1/Books |

Все методы являются только для чтения (HTTP-запросы GET).

Уровень данных мы будем использовать Entity Framework. Записи книги будет иметь следующие поля:

- ID
- Заголовок
- Жанра
- Дата публикации
- Цена
- Описание:
- AuthorID (внешний ключ к таблице Authors)

Большинство запросов тем не менее, API вернет подмножество этих данных ("Заголовок", "Автор" и "genre"). Чтобы получить полную запись клиента запросы `/api/books/{id}/details`.

## <a name="prerequisites"></a>Предварительные требования

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional или Enterprise edition.

## <a name="create-the-visual-studio-project"></a>Создание проекта Visual Studio

Запустить Visual Studio. Из **файл** меню, выберите **New** , а затем выберите **проекта**.

В **шаблоны** области выберите **установленные шаблоны** и разверните **Visual C#** узла. В разделе **Visual C#** выберите **Web**. В списке шаблонов проектов выберите **веб-приложение ASP.NET MVC 4**. Назовите проект &quot;BooksAPI&quot;.

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

В **новый проект ASP.NET** диалоговом окне выберите **пустой** шаблона. В разделе «Добавление папок и основные ссылки для» выберите **веб-API** флажок. Нажмите кнопку **Создание проекта**.

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

Это создает схему проекта, который настроен для функциональных возможностей веб-API.

### <a name="domain-models"></a>Модели домена

Добавьте классы для модели домена. В обозревателе решений щелкните правой кнопкой мыши папку Models. Выберите **добавить**, а затем выберите **класс**. Присвойте классу имя `Author`.

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

Замените код в Author.cs следующее:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

Теперь добавьте класс с именем `Book`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a>Добавить контроллер веб-API

На этом этапе мы добавим контроллер Web API, использующий Entity Framework в качестве уровня данных.

Для сборки проекта нажмите CTRL+SHIFT+B. Платформа Entity Framework использует отражение для обнаружения свойства моделей, поэтому для него требуется скомпилированной сборки для создания схемы базы данных.

В обозревателе решений щелкните правой кнопкой мыши папку Controllers. Выберите **добавить**, а затем выберите **контроллера**.

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

В **Добавление шаблона** диалоговое окно, выберите «веб-API 2 контроллер с действиями чтения и записи, использующий Entity Framework.»

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

В **Добавление контроллера** диалоговом окне для **имя контроллера**, введите &quot;BooksController&quot;. Выберите &quot;использовать асинхронные действия контроллера&quot; флажок. Для **класс модели**выберите &quot;книги&quot;. (Если вы не видите `Book` класс в списке в раскрывающемся списке, убедитесь, что при построении проекта.) Нажмите кнопку «+».

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

Нажмите кнопку **добавить** в **новый контекст данных** диалоговое окно.

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

Нажмите кнопку **добавить** в **Добавление контроллера** диалоговое окно. Формирование шаблонов добавляет класс с именем `BooksController` , определяющий контроллер API. Он также добавляет класс с именем `BooksAPIContext` в папку Models, который определяет контекста данных Entity Framework.

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a>Заполнение базы данных

В меню "Сервис" выберите **диспетчер пакетов библиотеки**, а затем выберите **консоль диспетчера пакетов**.

В окне консоли диспетчера пакетов введите следующую команду:

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

Эта команда создает папку Migrations и добавляет новый файл кода Configuration.cs. Откройте этот файл и добавьте следующий код, чтобы `Configuration.Seed` метод.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

В окне консоли диспетчера пакетов введите следующие команды.

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

Эти команды создают локальную базу данных и вызвать метод начальное значение для заполнения базы данных.

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a>Добавьте классы объектов передачи данных

Если запустить приложение сейчас и отправить запрос GET /api/books/1, ответ выглядит примерно следующим. (Я добавил отступами для удобства чтения).

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

Вместо этого я хочу этот запрос для возврата подмножества полей. Кроме того мне необходимо вернуть имя автора, а не идентификатор автора. Чтобы добиться этого, мы изменим методов контроллера для получения *объект передачи данных* (DTO) вместо модели EF. DTO — это объект, предназначенный только для переноса данных.

В обозревателе решений щелкните правой кнопкой мыши проект и выберите **добавить** | **новую папку**. Назовите папку &quot;DTO&quot;. Добавьте класс с именем `BookDto` в папку DTO, со следующим определением:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

Добавьте еще один класс с именем `BookDetailDto`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

Затем обновите `BooksController` класса для возвращения `BookDto` экземпляров. Мы будем использовать [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) метод проект `Book` экземпляры `BookDto` экземпляров. Вот обновленный код для класса контроллера.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> Я удалил `PutBook`, `PostBook`, и `DeleteBook` методов, так как они не нужны в этом руководстве.


Теперь Если вы запустите приложение и запросить /api/books/1, текст ответа должен выглядеть следующим образом:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a>Добавление атрибутов маршрутов

Затем мы преобразуем контроллер для использования маршрутизации с помощью атрибутов. Во-первых, добавьте **RoutePrefix** атрибута к контроллеру. Этот атрибут определяет начальное сегментов URI-адреса для всех методов в этом контроллере.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

Затем добавьте **[Route]** атрибуты к действиям контроллера, следующим образом:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

Шаблон маршрута для каждого метода контроллера — это префикс, а также строки, указанные в **маршрута** атрибута. Для `GetBook` метод, шаблон маршрута включает параметризованной строки &quot;{id: int}&quot;, выделяющему, если сегмент URI содержит целочисленное значение.

| Метод | Шаблон маршрута | Пример URI |
| --- | --- | --- |
| `GetBooks` | «api и книги» | `http://localhost/api/books` |
| `GetBook` | «api/books / {id: int}» | `http://localhost/api/books/5` |

## <a name="get-book-details"></a>Получение сведений о книге

Чтобы получить сведения о книге, клиент отправит запрос GET к `/api/books/{id}/details`, где *{id}* идентификатор книги.

Добавьте следующий метод в класс `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

Если вы запрашиваете `/api/books/1/details`, ответ выглядит следующим образом:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a>Получение книги по жанру

Для получения списка книг определенного жанра, клиент отправит запрос GET к `/api/books/genre`, где *жанр* имя жанр. (Например, `/api/books/fantasy`.)

Добавьте следующий метод в `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

Здесь мы при определении маршрута, который содержит параметр {жанр} в шаблоне URI. Обратите внимание на то, что веб-API способен различать эти два URI и перенаправлять их в различные методы:

`/api/books/1`

`/api/books/fantasy`

Это потому, что `GetBook` метод включает ограничение, что сегмент «id» должен быть целым числом:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

Если вы запрашивали /api/books/fantasy, ответ выглядит следующим образом:

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a>Получение книги по авторам

Чтобы получить список документации для данного автора, клиент отправит запрос GET к `/api/authors/id/books`, где *идентификатор* идентификатор автора.

Добавьте следующий метод в `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

В этом примере интересен поскольку &quot;книги&quot; является рассматривать дочерний ресурс &quot;авторы&quot;. Этот шаблон является довольно часто в API-интерфейсов RESTful.

Тильда (~) в шаблоне маршрута переопределяет префикс маршрута в **RoutePrefix** атрибута.

## <a name="get-books-by-publication-date"></a>Получение книги по дате публикации

Чтобы получить список документации по дате публикации, клиент отправит запрос GET к `/api/books/date/yyyy-mm-dd`, где *гггг мм дд* дата.

Вот один из способов сделать это:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

`{pubdate:datetime}` Параметр ограничен в соответствии с **DateTime** значение. Это работает, но это фактически более разрешительным, чем хотелось бы. Например эти URI также будет соответствовать маршрута:

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

Нет ничего плохого, позволяя эти URI. Тем не менее можно ограничить маршрут для определенного формата, добавление ограничения регулярных выражений в шаблон маршрута:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

Теперь только даты в виде &quot;гггг мм дд&quot; будут совпадать. Обратите внимание на то, что мы не используем регулярное выражение для проверки того, что у нас есть фактическая дата. Который обрабатывается при попытке преобразовать сегмент URI в веб-API **DateTime** экземпляра. Недопустимая дата такие как "2012-47-99' не сможет преобразовать, и клиент получит сообщение об ошибке 404.

Может также поддерживать разделитель косой черты (`/api/books/date/yyyy/mm/dd`), добавляя еще одну **[Route]** атрибута с другое регулярное выражение.

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

Нет сведений тонкое, но очень здесь. Второй шаблон маршрута присутствует подстановочный знак (\*) в начале параметра {pubdate}:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

Подсистема маршрутизации {pubdate} должен соответствовать остальной части URI. По умолчанию параметр шаблона соответствует один сегмент URI. В этом случае мы хотим {pubdate} охватывать несколько сегментов URI-адреса:

`/api/books/date/2013/06/17`

## <a name="controller-code"></a>Код контроллера

Ниже приведен полный код для класса BooksController.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a>Сводка

Маршрутизация с помощью атрибутов дает большей управляемости и большую гибкость при разработке URI для вашего API.
