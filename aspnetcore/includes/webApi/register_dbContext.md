## <a name="register-the-database-context"></a>Регистрация контекста базы данных

На этом шаге контекст базы данных регистрируется с помощью контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection). Для контроллеров доступны службы (такие как контекст базы данных), зарегистрированные с помощью контейнера внедрения зависимостей (DI).

Зарегистрируйте контекст базы данных в контейнере службы с помощью встроенной поддержки [внедрения зависимостей](xref:fundamentals/dependency-injection). Замените содержимое файла *Startup.cs* следующим кодом:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup.cs?highlight=2,4,12-13)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Startup.cs?highlight=3,5,13-14)]
::: moniker-end

Предыдущий код:

* Удаляет неиспользуемый код.
* Указывает базу данных в памяти, которая внедряется в контейнер службы.