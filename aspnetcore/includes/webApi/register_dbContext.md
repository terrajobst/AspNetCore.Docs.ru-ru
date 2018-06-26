## <a name="register-the-database-context"></a><span data-ttu-id="ca31e-101">Регистрация контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="ca31e-101">Register the database context</span></span>

<span data-ttu-id="ca31e-102">На этом шаге контекст базы данных регистрируется с помощью контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="ca31e-102">In this step, the database context is registered with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="ca31e-103">Для контроллеров доступны службы (такие как контекст базы данных), зарегистрированные с помощью контейнера внедрения зависимостей (DI).</span><span class="sxs-lookup"><span data-stu-id="ca31e-103">Services (such as the DB context) that are registered with the dependency injection (DI) container are available to the controllers.</span></span>

<span data-ttu-id="ca31e-104">Зарегистрируйте контекст базы данных в контейнере службы с помощью встроенной поддержки [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="ca31e-104">Register the DB context with the service container using the built-in support for [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="ca31e-105">Замените содержимое файла *Startup.cs* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="ca31e-105">Replace the contents of the *Startup.cs* file with the following code:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Startup.cs?highlight=3,5,13-14)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup.cs?highlight=2,4,12-13)]

::: moniker-end  

<span data-ttu-id="ca31e-106">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="ca31e-106">The preceding code:</span></span>

* <span data-ttu-id="ca31e-107">Удаляет неиспользуемый код.</span><span class="sxs-lookup"><span data-stu-id="ca31e-107">Removes the unused code.</span></span>
* <span data-ttu-id="ca31e-108">Указывает базу данных в памяти, которая внедряется в контейнер службы.</span><span class="sxs-lookup"><span data-stu-id="ca31e-108">Specifies an in-memory database is injected into the service container.</span></span>
