<span data-ttu-id="b9d41-101">Для созданного кода базы данных удостоверений требуется [Entity Framework Core миграций](/ef/core/managing-schemas/migrations/).</span><span class="sxs-lookup"><span data-stu-id="b9d41-101">The generated Identity database code requires [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="b9d41-102">Создание миграции и обновления базы данных.</span><span class="sxs-lookup"><span data-stu-id="b9d41-102">Create a migration and update the database.</span></span> <span data-ttu-id="b9d41-103">Например, выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="b9d41-103">For example, run the following commands:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b9d41-104">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b9d41-104">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b9d41-105">В Visual Studio **консоль диспетчера пакетов**:</span><span class="sxs-lookup"><span data-stu-id="b9d41-105">In the Visual Studio **Package Manager Console**:</span></span>

```powershell
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b9d41-106">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="b9d41-106">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

---

<span data-ttu-id="b9d41-107">Параметр имени "креатеидентитисчема" для `Add-Migration` команды является произвольным.</span><span class="sxs-lookup"><span data-stu-id="b9d41-107">The "CreateIdentitySchema" name parameter for the `Add-Migration` command is arbitrary.</span></span> <span data-ttu-id="b9d41-108">`"CreateIdentitySchema"`Описывает миграцию.</span><span class="sxs-lookup"><span data-stu-id="b9d41-108">`"CreateIdentitySchema"` describes the migration.</span></span>
