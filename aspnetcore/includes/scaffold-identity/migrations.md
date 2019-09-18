Для созданного кода базы данных удостоверений требуется [Entity Framework Core миграций](/ef/core/managing-schemas/migrations/). Создание миграции и обновления базы данных. Например, выполните следующие команды:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

В Visual Studio **консоль диспетчера пакетов**:

```powershell
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

---

Параметр имени "креатеидентитисчема" для `Add-Migration` команды является произвольным. `"CreateIdentitySchema"`Описывает миграцию.
