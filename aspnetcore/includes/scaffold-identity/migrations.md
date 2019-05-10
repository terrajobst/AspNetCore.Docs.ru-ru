<span data-ttu-id="4eaef-101">Созданный код базы данных удостоверений требует [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/).</span><span class="sxs-lookup"><span data-stu-id="4eaef-101">The generated Identity database code requires [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="4eaef-102">Создание миграции и обновления базы данных.</span><span class="sxs-lookup"><span data-stu-id="4eaef-102">Create a migration and update the database.</span></span> <span data-ttu-id="4eaef-103">Например выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="4eaef-103">For example, run the following commands:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4eaef-104">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4eaef-104">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4eaef-105">В Visual Studio **консоль диспетчера пакетов**:</span><span class="sxs-lookup"><span data-stu-id="4eaef-105">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4eaef-106">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="4eaef-106">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

---

<span data-ttu-id="4eaef-107">Параметр name «CreateIdentitySchema» для `Add-Migration` команда может быть произвольным.</span><span class="sxs-lookup"><span data-stu-id="4eaef-107">The "CreateIdentitySchema" name parameter for the `Add-Migration` command is arbitrary.</span></span> <span data-ttu-id="4eaef-108">`"CreateIdentitySchema"` содержит описание миграции.</span><span class="sxs-lookup"><span data-stu-id="4eaef-108">`"CreateIdentitySchema"` describes the migration.</span></span>
