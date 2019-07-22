---
title: Учебник. Создание веб-API с помощью ASP.NET Core
author: rick-anderson
description: Узнайте, как создать веб-API с помощью ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/11/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 95410cef9753fbb0eda6136320b59682e0553ea7
ms.sourcegitcommit: 040aedca220ed24ee1726e6886daf6906f95a028
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "67893220"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a>Учебник. Создание веб-API с помощью ASP.NET Core

Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Майк Уоссон](https://github.com/mikewasson)

В этом учебнике приводятся основные сведения о создании веб-API с помощью ASP.NET Core.

В этом руководстве вы узнаете, как:

> [!div class="checklist"]
> * Создание проекта веб-API.
> * Добавление класса модели.
> * Создание контекста базы данных.
> * Регистрация контекста базы данных.
> * Добавление контроллера.
> * Добавление методов CRUD.
> * Настройка маршрутизации и путей URL.
> * Указание возвращаемых значений.
> * Вызов веб-API с помощью Postman.
> * Вызов веб-API с помощью jQuery.

В конечном итоге вы получите веб-API, обеспечивающий управление элементами списка дел, хранящимися в реляционной базе данных.

## <a name="overview"></a>Обзор

В этом руководстве создается следующий API-интерфейс:

|API | ОПИСАНИЕ | Текст запроса | Текст ответа |
|--- | ---- | ---- | ---- |
|GET/api/todo | Получение всех элементов задач | Нет | Массив элементов задач|
|GET/api/todo/{id} | Получение объекта по идентификатору | Нет | Элемент задачи|
|POST/api/todo | Добавление нового элемента | Элемент задачи | Элемент задачи |
|PUT/api/todo/{id} | Обновление существующего элемента &nbsp; | Элемент задачи | Нет |
|DELETE/api/todo/{id} &nbsp; &nbsp; | Удаление элемента &nbsp; &nbsp; | Нет | Нет|

На следующем рисунке показана структура приложения.

![Клиент представлен прямоугольником слева. Он отправляет запрос и получает ответ от приложения (прямоугольник справа). В прямоугольнике приложения также расположены три прямоугольника, представляющих контроллер, модель и уровень доступа к данным. Запрос поступает на контроллер приложения, который выполняет операции чтения и записи между контроллером и уровнем доступа к данным. Модель сериализуется и возвращается клиенту в ответе.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a>Предварительные требования

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a>Создайте веб-проект.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* В меню **Файл** выберите пункт **Создать** > **Проект**.
* Выберите шаблон **Веб-приложение ASP.NET Core** и нажмите **Далее**.
* Назовите проект *TodoApi* и нажмите **Создать**.
* В диалоговом окне **Создание веб-приложения ASP.NET Core** убедитесь в том, что выбраны платформы **.NET Core** и **ASP.NET Core 2.2**. Выберите шаблон **API** и нажмите кнопку **Создать**. **Не** выбирайте **Включение поддержки Docker**.

![Диалоговое окно создания проекта VS](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

* Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Смените каталог (`cd`) на папку, в которой будет содержаться папка проекта.
* Выполните следующие команды:

   ```console
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  С помощью этих команд создается новый проект веб-API и открывается новый экземпляр Visual Studio Code в новой папке проекта.

* При появлении диалогового окна с запросом на добавление в проект необходимых ресурсов выберите **Да**.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

* Выберите **Файл** > **Новое решение**.

  ![Новое решение macOS](first-web-api-mac/_static/sln.png)

* Выберите **.NET Core** > **Приложение** > **API** > **Далее**.

  ![Диалоговое окно "Новый проект" в macOS](first-web-api-mac/_static/1.png)
  
* В диалоговом окне **Настройка нового веб-API ASP.NET Core** оставьте установленное по умолчанию значение для параметра **Целевая платформа**, то есть * *.NET Core 2.2*.

* Введите *TodoApi* в поле **Имя проекта** и выберите команду **Создать**.

  ![Диалоговое окно конфигурации](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a>Тестирование API

Шаблон проекта создает API `values`. Вызовите метод `Get` из браузера для тестирования приложения.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Нажмите клавиши CTRL+F5, чтобы запустить приложение. Visual Studio запустит браузер и перейдет к `https://localhost:<port>/api/values`, где `<port>` — это номер порта, выбранный случайным образом.

Если появится диалоговое окно с запросом о необходимости доверять сертификату IIS Express, выберите **Да**. В появляющемся следом диалоговом окне **Предупреждение системы безопасности** выберите **Да**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

Нажмите клавиши CTRL+F5, чтобы запустить приложение. В браузере перейдите по следующему URL-адресу: [https://localhost:5001/api/values](https://localhost:5001/api/values).

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

Выберите **Выполнить** > **Начать отладку**, чтобы запустить приложение. Visual Studio для Mac запустит браузер и перейдет к `https://localhost:<port>`, где `<port>` — это номер порта, выбранный случайным образом. Появится ошибка HTTP 404 (Не найдено). Добавьте `/api/values` к URL-адресу (измените URL-адрес на `https://localhost:<port>/api/values`).

---

Возвращается следующий файл JSON:

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a>Добавление класса модели

*Модель* — это набор классов, представляющих данные, которыми управляет приложение. Модель этого приложения содержит единственный класс `TodoItem`.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* В **обозревателе решений** щелкните проект правой кнопкой мыши. Выберите **Добавить** > **Новая папка**. Присвойте папке имя *Models*.

* Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Класс**. Присвойте классу имя *TodoItem* и выберите **Добавить**.

* Замените код шаблона следующим кодом:

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

* Добавьте папку с именем *Models*.

* Добавьте класс `TodoItem` в папку *Models*, используя следующий код:

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

* Щелкните проект правой кнопкой мыши. Выберите **Добавить** > **Новая папка**. Присвойте папке имя *Models*.

  ![Новая папка](first-web-api-mac/_static/folder.png)

* Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Новый файл** > **Общие** > **Пустой класс**.

* Назовите класс *TodoItem* и нажмите **Создать**.

* Замените код шаблона следующим кодом:

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

Свойство `Id` выступает в качестве уникального ключа реляционной базы данных.

Классы моделей можно размещать в любом месте проекта, но обычно для этого используется папка *Models*.

## <a name="add-a-database-context"></a>Добавление контекста базы данных

*Контекст базы данных* —это основной класс, который координирует функциональные возможности Entity Framework для модели данных. Этот класс является производным от класса `Microsoft.EntityFrameworkCore.DbContext`.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Щелкните папку *Models* правой кнопкой мыши и выберите **Добавить** > **Класс**. Назовите класс *TodoContext* и нажмите **Добавить**.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio для Mac](#tab/visual-studio-code+visual-studio-mac)

* Добавьте класс `TodoContext` в папку *Models*.

---

* Замените код шаблона следующим кодом:

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a>Регистрация контекста базы данных

В ASP.NET Core службы (такие как контекст базы данных) должны быть зарегистрированы с помощью контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection). Контейнер предоставляет службу контроллерам.

Обновите файл *Startup.cs*, используя следующий выделенный код:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

Предыдущий код:

* Удалите неиспользуемые объявления `using`.
* Добавляет контекст базы данных в контейнер внедрения зависимостей.
* Указывает, что контекст базы данных будет использовать базу данных в памяти.

## <a name="add-a-controller"></a>Добавление контроллера

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Щелкните папку *Controllers* правой кнопкой мыши.
* Выберите **Добавить** > **Новый объект**.
* В диалоговом окне **Добавить новый элемент** выберите шаблон **Класс контроллера API**.
* Присвойте классу имя *TodoController* и выберите **Добавить**.

  ![Диалоговое окно добавления элемента с контроллером в поле поиска и выбранным контроллером веб-API](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio для Mac](#tab/visual-studio-code+visual-studio-mac)

* В папке *Controllers* создайте класс с именем `TodoController`.

---

* Замените код шаблона следующим кодом:

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Предыдущий код:

* Определяет класс контроллера API без методов.
* Добавляет в класс атрибут [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute). Этот атрибут указывает, что контроллер отвечает на запросы веб-API. Дополнительные сведения о поведении, которое реализует этот атрибут, см. в <xref:web-api/index>.
* Использует внедрение зависимостей для внедрения контекста базы данных (`TodoContext`) в контроллер. Контекст базы данных используется в каждом методе [создания, чтения, обновления и удаления](https://wikipedia.org/wiki/Create,_read,_update_and_delete) в контроллере.
* Добавляет элемент `Item1` в базу данных, если она пуста. Этот код находится в конструкторе и выполняется каждый раз при обнаружении нового HTTP-запроса. Если вы удалите все элементы, конструктор создаст `Item1` при следующем вызове метода API. Поэтому может создаться впечатление, что удаление не было выполнено, хотя это не так.

## <a name="add-get-methods"></a>Добавление методов Get

Чтобы получить API, который извлекает элементы списка дел, добавьте следующие методы в класс `TodoController`:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

Эти методы реализуют две конечные точки GET:

* `GET /api/todo`
* `GET /api/todo/{id}`

Если приложение все еще выполняется, оно останавливается. Затем оно запускается снова с последними изменениями.

Протестируйте приложение, вызвав эти две конечные точки в браузере. Например:

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

При вызове `GetTodoItems` возвращается следующий ответ HTTP:

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a>Маршрутизация и пути URL

Атрибут [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) обозначает метод, который отвечает на запрос HTTP GET. Путь URL для каждого метода формируется следующим образом:

* Возьмите строку шаблона в атрибуте `Route` контроллера:

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* Замените `[controller]` именем контроллера (по соглашению это имя класса контроллера без суффикса "Controller"). В этом примере класс контроллера носит имя **Todo**, а сам контроллер, соответственно, — "todo". В ASP.NET Core [маршрутизация](xref:mvc/controllers/routing) реализуется без учета регистра символов.
* Если атрибут `[HttpGet]` имеет шаблон маршрута (например, `[HttpGet("products")]`), добавьте его к пути. В этом примере шаблон не используется. Дополнительные сведения см. в разделе [Маршрутизация атрибутов с помощью атрибутов Http[Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

В следующем методе `GetTodoItem` `"{id}"` — это переменная-заполнитель для уникального идентификатора элемента задачи. При вызове `GetTodoItem` параметру метода `id` присваивается значение `"{id}"` в URL-адресе.

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a>Возвращаемые значения

Возвращаемое значение имеет тип `GetTodoItems`, а метод `GetTodoItem` имеет тип [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type). ASP.NET Core автоматически сериализует объект в формат [JSON](https://www.json.org/) и записывает данные JSON в тело сообщения ответа. Код ответа для этого типа возвращаемого значения равен 200, что свидетельствует об отсутствии необработанных исключений. Необработанные исключения преобразуются в ошибки 5xx.

Типы возвращаемых значений `ActionResult` могут представлять широкий спектр кодов состояний HTTP. Например, метод `GetTodoItem` может возвращать два разных значения состояния:

* Если запрошенному идентификатору не соответствует ни один элемент, метод возвращает ошибку 404 ([Не найдено](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound)).
* В противном случае метод возвращает код 200 с телом ответа JSON. При возвращении `item` возвращается ответ HTTP 200.

## <a name="test-the-gettodoitems-method"></a>Тестирование метода GetTodoItems

В этом учебнике для тестирования веб-API используется Postman.

* Установка [Postman](https://www.getpostman.com/downloads/)
* Запустите веб-приложение.
* Запустите Postman.
* Отключение параметра **Проверка SSL-сертификата**
  
  * В меню **Файл > Параметры** (вкладка **Общие*) отключите параметр **Проверка SSL-сертификата**.
    > [!WARNING]
    > После тестирования контроллера снова включите проверку SSL-сертификата.

* Создайте новый запрос.
  * Укажите метод HTTP **GET**.
  * Укажите URL-адрес запроса `https://localhost:<port>/api/todo`. Например, `https://localhost:5001/api/todo`.
* Выберите режим **Представление с двумя областями** в Postman.
* Нажмите кнопку **Отправить**.

![Postman с запросом Get](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a>Добавление метода Create

Добавьте приведенный ниже метод `PostTodoItem`.

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Предыдущий код является методом HTTP POST, как указывает атрибут [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute). Этот метод получает значение элемента списка дел из текста HTTP-запроса.

Метод `CreatedAtAction`:

* В случае успеха возвращает код состояния HTTP 201. HTTP 201 представляет собой стандартный ответ для метода HTTP POST, создающий ресурс на сервере.
* Добавляет заголовок `Location` в ответ. Заголовок `Location` расположения указывает URI вновь созданной задачи. Дополнительные сведения см. в статье [10.2.2 201 "Создан ресурс"](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* Указывает действие `GetTodoItem` для создания URI заголовка `Location`. Ключевое слово `nameof` C# используется для предотвращения жесткого программирования имени действия в вызове `CreatedAtAction`.

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a>Тестирование метода PostTodoItem

* Выполните построение проекта.
* В Postman укажите метод HTTP `POST`.
* Откройте вкладку **Тело**.
* Установите переключатель **без обработки**.
* Задайте тип **JSON (приложение/json)** .
* В теле запроса введите код JSON для элемента списка дел:

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* Нажмите кнопку **Отправить**.

  ![Postman с запросом Create](first-web-api/_static/create.png)

  Если вы получаете ошибку 405 "Недопустимый метод", это может свидетельствовать о том, что после добавления метода `PostTodoItem` не была выполнена компиляция проекта.

### <a name="test-the-location-header-uri"></a>Тестирование URI заголовка расположения

* Перейдите на вкладку **Заголовки** в области **Ответ**.
* Скопируйте значение заголовка **Расположение**:

  ![Вкладка "Заголовки" в консоли Postman](first-web-api/_static/pmc2.png)

* Укажите метод GET.
* Вставьте URI (например, `https://localhost:5001/api/Todo/2`)
* Нажмите кнопку **Отправить**.

## <a name="add-a-puttodoitem-method"></a>Добавление метода PutTodoItem

Добавьте приведенный ниже метод `PutTodoItem`.

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

Страница `PutTodoItem` аналогична странице `PostTodoItem`, но использует запрос HTTP PUT. Ответ — [204 (Нет содержимого)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Согласно спецификации HTTP, запрос PUT требует, чтобы клиент отправлял всю обновленную сущность, а не только изменения. Чтобы обеспечить поддержку частичных обновлений, используйте [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).

Если возникнет ошибка вызова `PutTodoItem`, вызовите `GET`, чтобы в базе данных был один элемент.

### <a name="test-the-puttodoitem-method"></a>Тестирование метода PutTodoItem

Этот пример использует базу данных в памяти, которая должна быть инициирована при каждом запуске приложения. При выполнении вызова PUT в базе данных уже должен существовать какой-либо элемент. Для этого перед вызовом PUT выполните вызов GET, чтобы убедиться в наличии такого элемента в базе данных.

Обновите элемент списка дел с идентификатором = 1 и присвойте ему имя "feed fish":

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

На следующем рисунке показан процесс обновления Postman:

![Консоль Postman с ответом 204 (Нет содержимого)](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a>Добавление метода DeleteTodoItem

Добавьте приведенный ниже метод `DeleteTodoItem`.

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

`DeleteTodoItem`Ответ[ — 204 (нет содержимого)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

### <a name="test-the-deletetodoitem-method"></a>Тестирование метода DeleteTodoItem

Удалите элемент списка дел с помощью Postman:

* Укажите метод `DELETE`.
* Укажите URI удаляемого объекта, например `https://localhost:5001/api/todo/1`
* Нажмите кнопку **Отправить**

В этом примере приложения вы можете удалить все элементы. Однако в случае удаления последнего элемента в момент следующего вызова API конструктор класса модели создаст новый элемент.

## <a name="call-the-api-with-jquery"></a>Вызов API с помощью jQuery

В этом разделе добавляется HTML-страница, которая использует jQuery для вызова веб-API. jQuery запускает запрос и вносит на страницу подробные сведения из ответа API.

Настройте приложение для [обслуживания статических файлов](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) и [включения сопоставления файлов по умолчанию](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_), обновив *Startup.cs* следующим выделенным кодом:

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

::: moniker range=">= aspnetcore-2.2"
Создайте папку *wwwroot* в каталоге проекта.
::: moniker-end

Добавьте HTML-файл *index.html* в каталог *wwwroot*. Замените его содержимое следующей разметкой:

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

Добавьте файл JavaScript с именем *site.js* в каталог *wwwroot*. Замените его содержимое следующим кодом:

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

Может потребоваться изменение параметров запуска проекта ASP.NET Core для локального тестирования HTML-страницы:

* Откройте файл *Properties\launchSettings.json*.
* Удалите свойство `launchUrl`, чтобы приложение открылось через *index.html* &mdash; файл проекта по умолчанию.

Для получения jQuery можно использовать следующие способы. В предыдущем фрагменте кода библиотека загружается из CDN.

В этом примере вызываются все методы CRUD в API. Ниже приводится пояснение вызовов API.

### <a name="get-a-list-of-to-do-items"></a>Получение списка элементов задач

Функция jQuery [ajax](https://api.jquery.com/jquery.ajax/) отправляет запрос `GET` к API, который возвращает JSON, представляющий массив элементов списка дел. В случае успешного запроса используется функция обратного вызова `success`. При обратном вызове в модель DOM вносятся данные о задачах.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a>Добавление элемента задачи

Функция [ajax](https://api.jquery.com/jquery.ajax/) отправляет запрос `POST` с элементом списка дел в теле запроса. Для параметров `accepts` и `contentType` указывается `application/json`, чтобы указать тип носителя при получении и отправке. Элемент списка дел преобразуется в JSON с помощью [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify). Если интерфейс API возвращает код состояния успешного выполнения, вызывается функция `getData` для обновления HTML-таблицы.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a>Обновление элемента задачи

Обновление элемента списка дел выполняется аналогично его добавлению. `url` изменяется, чтобы добавить уникальный идентификатор для элемента, а в качестве `type` устанавливается `PUT`.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a>Удаление элемента задачи

Чтобы удалить элемент списка дел, установите для `type` в вызове AJAX значение `DELETE` и укажите уникальный идентификатор элемента в URL-адресе.

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

## <a name="additional-resources"></a>Дополнительные ресурсы

[Просмотреть или скачать пример кода для этого учебника](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples). См. раздел [Практическое руководство. Скачивание файла](xref:index#how-to-download-a-sample).

Дополнительные сведения см. в следующих ресурсах:

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/index>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [Версия руководства на YouTube](https://www.youtube.com/watch?v=TTkhEyGBfAk)

## <a name="next-steps"></a>Следующие шаги

В этом руководстве вы узнали, как:

> [!div class="checklist"]
> * Создание проекта веб-API.
> * Добавление класса модели.
> * Создание контекста базы данных.
> * Регистрация контекста базы данных.
> * Добавление контроллера.
> * Добавление методов CRUD.
> * Настройка маршрутизации и путей URL.
> * Указание возвращаемых значений.
> * Вызов веб-API с помощью Postman.
> * Вызов веб-API с помощью jQuery.

Перейдите к следующему учебнику, посвященному созданию страниц справки по API:

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
