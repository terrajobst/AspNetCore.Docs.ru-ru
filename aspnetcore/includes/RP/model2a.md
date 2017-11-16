<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="8c3a4-101">Добавление строки подключения базы данных</span><span class="sxs-lookup"><span data-stu-id="8c3a4-101">Add a database connection string</span></span>

<span data-ttu-id="8c3a4-102">Добавьте строку подключения в файл *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="8c3a4-102">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="8c3a4-103">Регистрация контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="8c3a4-103">Register the database context</span></span>

<span data-ttu-id="8c3a4-104">Зарегистрируйте контекст базы данных в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) в файле *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="8c3a4-104">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>