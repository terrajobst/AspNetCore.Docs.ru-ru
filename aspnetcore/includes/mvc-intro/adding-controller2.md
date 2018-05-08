Замените содержимое файла *Controllers/HelloWorldController.cs* следующим:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_1)]

Каждый метод `public` в контроллере вызывается как конечная точка HTTP. В приведенном выше примере оба метода возвращают строку.  Обратите внимание на комментарии, предшествующие каждому методу.

Конечная точка HTTP — это адресуемый URL в веб-приложении, такой как `http://localhost:1234/HelloWorld`, который объединяет в себе используемый протокол (`HTTP`), сетевое расположение сервера с TCP-портом (`localhost:1234`), а также целевой URI (`HelloWorld`).

В первом комментарии указано, что этот метод [HTTP GET](https://www.w3schools.com/tags/ref_httpmethods.asp) вызывается путем добавления "/HelloWorld/" к базовому URL-адресу. Во втором комментарии указано, что этот метод [HTTP GET](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) вызывается путем добавления "/HelloWorld/Welcome/" к URL-адресу. Далее в этом руководстве вы научитесь использовать механизм формирования шаблонов для создания методов `HTTP POST`.

Запустите приложение в режиме без отладки и добавьте "HelloWorld" к пути в адресной строке. Метод `Index` возвращает строку.

![Окно браузера, в котором отображается ответ "This is my default action" (Это действие по умолчанию)](../../tutorials/first-mvc-app/adding-controller/_static/hell1.png)

MVC вызывает классы контроллера (и методы действия в них) в зависимости от входящего URL-адреса. [Логика маршрутизации URL-адресов](xref:mvc/controllers/routing), используемая моделью MVC по умолчанию, определяет вызываемый код на основе формата следующего вида:

`/[Controller]/[ActionName]/[Parameters]`

Формат маршрутизации задается в методе `Configure` в файле *Startup.cs*.

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

Если при запуске приложения не указаны сегменты URL-адреса, по умолчанию устанавливаются контроллер "Home" и метод "Index", которые заданы в выделенной выше строке шаблона.

Первый сегмент URL-адреса определяет класс контроллера, который будет выполняться. Таким образом, `localhost:xxxx/HelloWorld` сопоставляется с классом `HelloWorldController`. Вторая часть сегмента URL-адреса определяет метод действия для класса. Таким образом, `localhost:xxxx/HelloWorld/Index` выполняет метод `Index` класса `HelloWorldController`. Обратите внимание, что в этом случае достаточно перейти по адресу `localhost:xxxx/HelloWorld`, а метод `Index` вызывается по умолчанию. Это связано с тем, что если имя вызываемого метода не задано явно, для контроллера по умолчанию вызывается метод `Index`. В третьей части сегмента URL-адреса (`id`) указываются данные маршрута. Мы ознакомимся с ними далее в этом руководстве.

Перейдите по адресу `http://localhost:xxxx/HelloWorld/Welcome`. Выполняется метод `Welcome`, который возвращает строку "This is the Welcome action method..." (Это метод действия Welcome...). Для этого URL-адреса заданы контроллер `HelloWorld` и метод действия `Welcome`. Часть URL-адреса `[Parameters]` на данный момент еще не использовалась.

![Окно браузера, в котором отображается ответ "This is the Welcome action method" (Это метод действия Welcome)](../../tutorials/first-mvc-app/adding-controller/_static/welcome.png)

Измените код, чтобы передать сведения о параметрах из URL-адреса в контроллер. Например, `/HelloWorld/Welcome?name=Rick&numtimes=4`. Измените метод `Welcome`, включив два параметра, как показано в следующем коде. 

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_2)]

Предыдущий код:

* Использует функцию необязательного параметра C#, указывая, что параметр `numTimes` по умолчанию принимает значение 1, если ему не было передано значение.
* Использует `HtmlEncoder.Default.Encode` для защиты приложения от злонамеренного ввода данных (JavaScript). 
* Использует [интерполированные строки](/dotnet/articles/csharp/language-reference/keywords/interpolated-strings).

Запустите приложение и перейдите по адресу:

   `http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

(Замените xxxx на номер порта.) Вы можете попробовать различные значения `name` и `numtimes` в URL-адресе. [Система привязки модели MVC](xref:mvc/models/model-binding) автоматически сопоставляет именованные параметры из строк запроса в адресной строке с параметрами метода. Дополнительные сведения см. в разделе [Привязка модели](xref:mvc/models/model-binding).

![Окно браузера, в котором отображается ответ "Hello Rick" (Привет, Rick); NumTimes=4](../../tutorials/first-mvc-app/adding-controller/_static/rick4.png)

На показанном выше рисунке сегмент URL-адреса (`Parameters`) не используется, а параметры `name` и `numTimes` передаются как [строки запроса](https://wikipedia.org/wiki/Query_string). Вопросительный знак (`?`) в приведенном выше URL-адресе используется в качестве разделителя, после которого указываются строки запроса. Символ `&` разделяет строки запроса.

Замените метод `Welcome` следующим кодом:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_3)]

Запустите приложение и введите следующий URL-адрес: `http://localhost:xxx/HelloWorld/Welcome/3?name=Rick`

![Окно браузера, в котором отображается ответ "Hello Rick" (Привет, Rick); ID=3](../../tutorials/first-mvc-app/adding-controller/_static/rick_routedata.png)

На этот раз третий сегмент URL-адреса сопоставляется с параметром маршрута `id`. Метод `Welcome` содержит параметр `id`, который сопоставляется с шаблоном URL-адреса в методе `MapRoute`. Завершающий символ `?` (в `id?`) указывает, что параметр `id` является необязательным.

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

В этих примерах контроллер реализует работу представления и контроллера в модели MVC. Контроллер возвращает HTML напрямую. Как правило, это не требуется, поскольку в этом случае заметно усложняется написание и поддержка кода. Вместо этого обычно используется отдельный файл шаблона представления Razor для создания HTML-ответа. Это будет показано в следующем руководстве.
