## <a name="call-the-web-api-with-jquery"></a>Вызов веб-API с помощью jQuery

В этом разделе добавляется HTML-страница, которая использует jQuery для вызова веб-API. jQuery запускает запрос и вносит на страницу подробные сведения из ответа API.

Настройте в проекте обслуживание статических файлов и включение сопоставления файлов по умолчанию. Для этого необходимо вызвать методы расширения [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) и [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) в *Startup.Configure*. Дополнительные сведения см. в разделе [Работа со статическими файлами в ASP.NET Core](xref:fundamentals/static-files).

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup2.cs?name=snippet_Configure&highlight=3-4)]

Добавьте HTML-файл с именем *index.html* в каталог *wwwroot* проекта. Замените его содержимое следующей разметкой:

[!code-html[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/index.html)]

Добавьте файл JavaScript с именем *site.js* в каталог *wwwroot* проекта. Замените его содержимое следующим кодом:

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

Может потребоваться изменение параметров запуска проекта ASP.NET Core для локального тестирования HTML-страницы. Откройте *launchSettings.json* в каталоге *Свойства* в проекте. Удалите свойство `launchUrl`, чтобы приложение открылось через *index.html* &mdash; файл проекта по умолчанию.

Для получения jQuery можно использовать следующие способы. В предыдущем фрагменте кода библиотека загружается из CDN. В этом примере приводится полный набор функций CRUD для вызова API с помощью jQuery. В этом примере используются дополнительные компоненты, расширяющие возможности разработчика. Ниже приводятся объяснения о вызовах API.

### <a name="get-a-list-of-to-do-items"></a>Получение списка элементов задач

Чтобы получить список элементов задач, отправьте HTTP-запрос GET к */api/todo*.

Функция JQuery [ajax](https://api.jquery.com/jquery.ajax/) отправляет запрос AJAX к API, который возвращает JSON, представляющий объект или массив. Эта функция может обрабатывать все виды взаимодействия с HTTP, отправляя HTTP-запросы в указанный `url`. `GET` используется в качестве `type`. В случае успешного запроса используется функция обратного вызова `success`. При обратном вызове в модель DOM вносятся данные о задачах.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a>Добавление элемента задачи

Чтобы добавить элемент задачи, отправьте HTTP-запрос POST к */api/todo/*. Текст запроса должен содержать объект задачи. Функция [ajax](https://api.jquery.com/jquery.ajax/) использует `POST` для вызова API. В тексте запросов `POST` и `PUT` содержатся данные, отправляемые в API. API ожидает текста запроса JSON. Для параметров `accepts` и `contentType` указывается `application/json`, чтобы классифицировать тип носителя при получении и отправке соответственно. Данные преобразуются в объект JSON с помощью [`JSON.stringify`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify). Если интерфейс API возвращает код состояния успешного выполнения, вызывается функция `getData` для обновления HTML-таблицы.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a>Обновление элемента задачи

Обновление элемента задачи похоже на добавление, поскольку в обеих операциях используется текст запроса. Единственная разница между ними заключается в изменении `url` для добавления уникального идентификатора элемента, а `type` — `PUT`.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a>Удаление элемента задачи

Чтобы удалить элемент задачи, установите для `type` в вызове AJAX значение `DELETE` и укажите уникальный идентификатор элемента в URL-адресе.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]
