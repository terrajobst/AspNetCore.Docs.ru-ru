---
ms.openlocfilehash: 698a127120f7f52672ceb927ff24eb1b02f91521
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320296"
---
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
