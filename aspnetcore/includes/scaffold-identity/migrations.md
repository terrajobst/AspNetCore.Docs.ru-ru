Для созданного кода базы данных удостоверений требуется [Entity Framework Core миграций](/ef/core/managing-schemas/migrations/). Создание миграции и обновления базы данных. Например, выполните следующие команды:

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

В **консоли диспетчера пакетов**Visual Studio:

```powershell
Install-Package Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

---

Параметр имени "Креатеидентитисчема" для команды `Add-Migration` является произвольным. `"CreateIdentitySchema"` описывает миграцию.
