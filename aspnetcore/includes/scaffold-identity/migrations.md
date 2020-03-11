<span data-ttu-id="b6d72-101">Для созданного кода базы данных удостоверений требуется [Entity Framework Core миграций](/ef/core/managing-schemas/migrations/).</span><span class="sxs-lookup"><span data-stu-id="b6d72-101">The generated Identity database code requires [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="b6d72-102">Создание миграции и обновления базы данных.</span><span class="sxs-lookup"><span data-stu-id="b6d72-102">Create a migration and update the database.</span></span> <span data-ttu-id="b6d72-103">Например, выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="b6d72-103">For example, run the following commands:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="b6d72-104">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b6d72-104">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b6d72-105">В **консоли диспетчера пакетов**Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="b6d72-105">In the Visual Studio **Package Manager Console**:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-cli"></a>[<span data-ttu-id="b6d72-106">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="b6d72-106">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

---

<span data-ttu-id="b6d72-107">Параметр имени "Креатеидентитисчема" для команды `Add-Migration` является произвольным.</span><span class="sxs-lookup"><span data-stu-id="b6d72-107">The "CreateIdentitySchema" name parameter for the `Add-Migration` command is arbitrary.</span></span> <span data-ttu-id="b6d72-108">`"CreateIdentitySchema"` описывает миграцию.</span><span class="sxs-lookup"><span data-stu-id="b6d72-108">`"CreateIdentitySchema"` describes the migration.</span></span>
