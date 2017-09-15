# <a name="adding-a-view-to-an-aspnet-core-mvc-app"></a>Добавление представления в приложение MVC ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этом разделе вы измените класс `HelloWorldController`, чтобы использовать файлы шаблонов представления Razor для отчетливой инкапсуляции процесса создания HTML-ответов для клиента.

Файл шаблона представления создается с помощью Razor. Шаблоны представления на основе Razor имеют расширение файла *.cshtml*. Они реализуют удобный способ для создания выходных данных HTML с помощью C#.

На данный момент метод `Index` возвращает строку с сообщением, которое жестко задано в классе контроллера. В классе `HelloWorldController` замените метод `Index` следующим кодом:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_4)]

Предыдущий код возвращает объект `View`. В нем используется шаблон представления для создания HTML-ответа браузеру. Методы контроллера (также называются методами действия), такие как приведенный выше метод `Index`, обычно возвращают [IActionResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iactionresult) (или производный от `ActionResult` класс), а не примитивные типы, такие как строки.
