Созданный код идентификатора базы данных требует [Entity Framework Core миграции](/ef/core/managing-schemas/migrations/). Создание миграции и обновить базу данных. Например выполните следующие команды:

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

------

Имя параметра «CreateIdentitySchema» для `Add-Migration` команда может быть произвольным. `"CreateIdentitySchema"` содержит описание миграции.