<!--
[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](../../tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

Предыдущий метод `Index`:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_1stSearch)]

Обновленный метод `Index` с параметром `id`:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_SearchID)]

Теперь можно передать заголовок поиска в качестве данных маршрута (сегмент URL-адреса) вместо значения строки запроса.

![Представление Index, в URL-адрес которого добавлено слово ghost, возвращает два фильма: Ghostbusters и Ghostbusters 2](../../tutorials/first-mvc-app/search/_static/g2.png)

Тем не менее пользователи вряд ли будут каждый раз изменять URL-адрес для поиска фильмов. Итак, теперь вам необходимо добавить пользовательский интерфейс для удобства фильтрации фильмов. Если вы изменили сигнатуру метода `Index` для тестирования передачи параметра `ID` с привязкой к маршруту, измените ее снова, чтобы она снова принимала параметр `searchString`:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_1stSearch)]

Откройте файл *Views/Movies/Index.cshtml* и добавьте разметку `<form>`, которая выделена ниже:

[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexForm1.cshtml?highlight=10-16&range=4-21)]

Тег HTML `<form>` использует [вспомогательную функцию тега Form](../../mvc/views/working-with-forms.md), чтобы при отправке формы строка фильтра передавалась в действие `Index` контроллера movies. Сохраните изменения и протестируйте фильтр.

![Представление Index со словом ghost в текстовом поле фильтра по названию](../../tutorials/first-mvc-app/search/_static/filter.png)

Вопреки ожиданиям, перегрузка `[HttpPost]` для метода `Index` отсутствует. Она не нужна, поскольку метод не изменяет состояние приложения и просто выполняет фильтрацию данных.

Можно добавить следующий метод `[HttpPost] Index`.

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_SearchPost)]

Параметр `notUsed` используется для создания перегрузки метода `Index`. Это мы обсудим далее в этом учебнике.

При добавлении этого метода вызывающий метод действия будет сопоставлять метод `[HttpPost] Index`, а метод `[HttpPost] Index` будет выполняться, как показано на рисунке ниже.

![Окно браузера с ответом приложения From HttpPost Index: фильтр по слову ghost](../../tutorials/first-mvc-app/search/_static/fo.png)

Тем не менее при добавлении этой версии `[HttpPost]` метода `Index` существует ограничение на общую реализацию. Допустим, вам необходимо добавить в закладки конкретный поиск или отправить друзьям ссылку, по которой они могут просмотреть аналогичный отфильтрованный список фильмов. Обратите внимание, что URL-адрес запроса HTTP POST совпадает с URL-адресом запроса GET (localhost:xxxxx/Movies/Index) — в URL-адресе отсутствуют сведения о поиске. Данные строки поиска отправляются на сервер в виде [значения поля формы](https://developer.mozilla.org/docs/Web/Guide/HTML/Forms/Sending_and_retrieving_form_data). Вы можете проверить это с помощью средств разработчика для браузера или [инструмента Fiddler](http://www.telerik.com/fiddler). На рисунке ниже показаны средства разработчика для браузера Chrome:

![Вкладка "Сеть" средств разработчика в Microsoft Edge с телом запроса со значением searchString, равным ghost](../../tutorials/first-mvc-app/search/_static/f12_rb.png)

В теле запроса отображается параметр поиска и маркер [XSRF](../../security/anti-request-forgery.md). Обратите внимание, что, как описывается в предыдущем руководстве, [вспомогательная функция тега Form](../../mvc/views/working-with-forms.md) создает маркер защиты от подделки [XSRF](../../security/anti-request-forgery.md). Поскольку мы не изменяем данные, проверять маркер безопасности в методе контроллера не нужно.

Так как параметр поиска находится в теле запроса, а не в URL-адресе, эти сведения о поиске нельзя добавить в закладки или открыть для общего доступа. Чтобы исправить это, необходимо указать запрос как `HTTP GET`.