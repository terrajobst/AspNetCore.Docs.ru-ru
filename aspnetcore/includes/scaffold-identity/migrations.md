Созданный код базы данных удостоверений требует [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/). Создание миграции и обновления базы данных. Например выполните следующие команды:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

В Visual Studio **консоль диспетчера пакетов**:

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

---

Параметр name «CreateIdentitySchema» для `Add-Migration` команда может быть произвольным. `"CreateIdentitySchema"` содержит описание миграции.
