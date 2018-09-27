---
title: Настройка модели удостоверения в ASP.NET Core
author: ajcvickers
description: В этой статье описывается настройка базовой модели данных Entity Framework Core для ASP.NET Core Identity.
ms.author: avickers
ms.date: 09/24/2018
uid: security/authentication/customize_identity_model
ms.openlocfilehash: 90c867eeac0e64bfe77cc7a829d61e831a2fb8e1
ms.sourcegitcommit: 9bdba90b2c97a4016188434657194b2d7027d6e3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/27/2018
ms.locfileid: "47402259"
---
# <a name="identity-model-customization-in-aspnet-core"></a>Настройка модели удостоверения в ASP.NET Core

По [Артур Vickers](https://github.com/ajcvickers)

Удостоверение ASP.NET Core предоставляет платформу для управления и хранения учетных записей пользователей в приложениях ASP.NET Core. Удостоверение добавляется в проект при **учетные записи отдельных пользователей** выбран в качестве механизма проверки подлинности. По умолчанию удостоверение позволяет использовать из Entity Framework (EF) Core модели данных. В этой статье описывается настройка модели удостоверения.

## <a name="identity-and-ef-core-migrations"></a>Удостоверение и миграций EF Core

Прежде чем изучать модель, это полезно для понимания принципов работы удостоверений с [миграций EF Core](/ef/core/managing-schemas/migrations/) для создания и обновления базы данных. На верхнем уровне выполняется:

1. Определение или обновление [модели данных в коде](/ef/core/modeling/).
1. Добавьте миграцию для преобразования этой модели в изменения, которые могут быть применены к базе данных.
1. Проверьте, что миграция правильно представляет свои намерения.
1. Применение миграции для обновления базы данных должны быть синхронизированы с моделью.
1. Повторите шаги 1 – 4 для дополнительного уточнения модели и поддерживать синхронизацию базы данных.

Для добавления и применить миграции, используйте один из следующих подходов:

* **Консоль диспетчера пакетов** окна (PMC), если с помощью Visual Studio. Дополнительные сведения см. в разделе [инструменты EF Core PMC](/ef/core/miscellaneous/cli/powershell).
* .NET Core CLI при помощи командной строки. Дополнительные сведения см. в разделе [средства командной строки EF Core .NET](/ef/core/miscellaneous/cli/dotnet).
* Щелкнув **применить миграции** кнопку на странице ошибки при запуске приложения.

ASP.NET Core есть обработчик страницы ошибок во время разработки. Обработчик может применить миграции, при запуске приложения. Для рабочих приложений, бывает необходимо создавать скрипты SQL по миграции и развертывания изменений базы данных как часть управляемой развертывания приложения и базы данных.

Когда создается новое приложение с использованием удостоверения, шаги 1 и 2 выше уже были завершены. То есть начальную модель данных уже существует, и первоначальной миграции был добавлен в проект. Первоначальной миграции по-прежнему необходимо применить к базе данных. Первоначальной миграции могут применяться через одну из следующих подходов:

* Запустите `Update-Database` в PMC.
* Запустите `dotnet ef database update` в командной строке.
* Нажмите кнопку **применить миграции** кнопку на странице ошибки при запуске приложения.

Повторите предыдущие шаги, при внесении изменений в модель.

## <a name="the-identity-model"></a>Модель удостоверения

### <a name="entity-types"></a>Типы сущностей

Модель удостоверения состоит из следующих типов сущностей.

|Тип сущности|Описание                                                  |
|-----------|-------------------------------------------------------------|
|`User`     |Представляет пользователя.                                         |
|`Role`     |Представляет роль.                                           |
|`UserClaim`|Представляет утверждение, обладает пользователь.                    |
|`UserToken`|Представляет маркер проверки подлинности для пользователя.               |
|`UserLogin`|Связывает пользователя с именем входа.                              |
|`RoleClaim`|Представляет утверждение, эта роль присваивается всем пользователям в роли.|
|`UserRole` |Сущность соединения, которая связывает пользователей и ролей.               |

### <a name="entity-type-relationships"></a>Связи между типами сущностей

[Типы сущностей](#entity-types) связаны друг с другом, одним из следующих способов:

* Каждый `User` может иметь множество `UserClaims`.
* Каждый `User` может иметь множество `UserLogins`.
* Каждый `User` может иметь множество `UserTokens`.
* Каждый `Role` может иметь несколько связанных `RoleClaims`.
* Каждый `User` может иметь несколько связанных `Roles`и каждый `Role` можно связать со многими `Users`. Это отношение многие ко многим, нуждается в таблице соединения в базе данных. В таблице соединения, представленного `UserRole` сущности.

### <a name="default-model-configuration"></a>Конфигурация модели по умолчанию

Определяет удостоверение, многие *классы контекста* , наследуемым от <xref:Microsoft.EntityFrameworkCore.DbContext> Настройка и использование модели. Эта настройка выполняется с помощью [EF Core Fluent API для Code First](/ef/core/modeling/) в <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating*> метод класса контекста. По умолчанию используется:

```csharp
builder.Entity<TUser>(b =>
{
    // Primary key
    b.HasKey(u => u.Id);

    // Indexes for "normalized" username and email, to allow efficient lookups
    b.HasIndex(u => u.NormalizedUserName).HasName("UserNameIndex").IsUnique();
    b.HasIndex(u => u.NormalizedEmail).HasName("EmailIndex");

    // Maps to the AspNetUsers table
    b.ToTable("AspNetUsers");

    // A concurrency token for use with the optimistic concurrency checking
    b.Property(u => u.ConcurrencyStamp).IsConcurrencyToken();

    // Limit the size of columns to use efficient database types
    b.Property(u => u.UserName).HasMaxLength(256);
    b.Property(u => u.NormalizedUserName).HasMaxLength(256);
    b.Property(u => u.Email).HasMaxLength(256);
    b.Property(u => u.NormalizedEmail).HasMaxLength(256);

    // The relationships between User and other entity types
    // Note that these relationships are configured with no navigation properties

    // Each User can have many UserClaims
    b.HasMany<TUserClaim>().WithOne().HasForeignKey(uc => uc.UserId).IsRequired();

    // Each User can have many UserLogins
    b.HasMany<TUserLogin>().WithOne().HasForeignKey(ul => ul.UserId).IsRequired();

    // Each User can have many UserTokens
    b.HasMany<TUserToken>().WithOne().HasForeignKey(ut => ut.UserId).IsRequired();

    // Each User can have many entries in the UserRole join table
    b.HasMany<TUserRole>().WithOne().HasForeignKey(ur => ur.UserId).IsRequired();
});

builder.Entity<TUserClaim>(b =>
{
    // Primary key
    b.HasKey(uc => uc.Id);

    // Maps to the AspNetUserClaims table
    b.ToTable("AspNetUserClaims");
});

builder.Entity<TUserLogin>(b =>
{
    // Composite primary key consisting of the LoginProvider and the key to use
    // with that provider
    b.HasKey(l => new { l.LoginProvider, l.ProviderKey });

    // Limit the size of the composite key columns due to common DB restrictions
    b.Property(l => l.LoginProvider).HasMaxLength(128);
    b.Property(l => l.ProviderKey).HasMaxLength(128);

    // Maps to the AspNetUserLogins table
    b.ToTable("AspNetUserLogins");
});

builder.Entity<TUserToken>(b =>
{
    // Composite primary key consisting of the UserId, LoginProvider and Name
    b.HasKey(t => new { t.UserId, t.LoginProvider, t.Name });

    // Limit the size of the composite key columns due to common DB restrictions
    b.Property(t => t.LoginProvider).HasMaxLength(maxKeyLength);
    b.Property(t => t.Name).HasMaxLength(maxKeyLength);

    // Maps to the AspNetUserTokens table
    b.ToTable("AspNetUserTokens");
});

builder.Entity<TRole>(b =>
{
    // Primary key
    b.HasKey(r => r.Id);

    // Index for "normalized" role name to allow efficient lookups
    b.HasIndex(r => r.NormalizedName).HasName("RoleNameIndex").IsUnique();

    // Maps to the AspNetRoles table
    b.ToTable("AspNetRoles");

    // A concurrency token for use with the optimistic concurrency checking
    b.Property(r => r.ConcurrencyStamp).IsConcurrencyToken();

    // Limit the size of columns to use efficient database types
    b.Property(u => u.Name).HasMaxLength(256);
    b.Property(u => u.NormalizedName).HasMaxLength(256);

    // The relationships between Role and other entity types
    // Note that these relationships are configured with no navigation properties

    // Each Role can have many entries in the UserRole join table
    b.HasMany<TUserRole>().WithOne().HasForeignKey(ur => ur.RoleId).IsRequired();

    // Each Role can have many associated RoleClaims
    b.HasMany<TRoleClaim>().WithOne().HasForeignKey(rc => rc.RoleId).IsRequired();
});

builder.Entity<TRoleClaim>(b =>
{
    // Primary key
    b.HasKey(rc => rc.Id);

    // Maps to the AspNetRoleClaims table
    b.ToTable("AspNetRoleClaims");
});

builder.Entity<TUserRole>(b =>
{
    // Primary key
    b.HasKey(r => new { r.UserId, r.RoleId });

    // Maps to the AspNetUserRoles table
    b.ToTable("AspNetUserRoles");
});
```

### <a name="model-generic-types"></a>Универсальные типы модели

По умолчанию определяет удостоверение [среда CLR](/dotnet/standard/glossary#clr) типы (CLR) для каждого типа сущности в списке выше. Эти типы начинаются с *удостоверений*:

* `IdentityUser`
* `IdentityRole`
* `IdentityUserClaim`
* `IdentityUserToken`
* `IdentityUserLogin`
* `IdentityRoleClaim`
* `IdentityUserRole`

Вместо того чтобы непосредственно использовать эти типы, типы можно использовать как базовые классы для типов приложения. `DbContext` Классы, определенные по удостоверению универсальны, таким образом, можно использовать различные типы среды CLR для одного или нескольких типов сущностей в модели. Эти универсальные типы также позволяют `User` типа первичного ключа (PK) данных должен быть изменен.

При использовании удостоверений с поддержкой для ролей, <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext> должен использоваться класс. Пример:

```csharp
// Uses all the built-in Identity types
// Uses `string` as the key type
public class IdentityDbContext
    : IdentityDbContext<IdentityUser, IdentityRole, string>
{
}

// Uses the built-in Identity types except with a custom User type
// Uses `string` as the key type
public class IdentityDbContext<TUser>
    : IdentityDbContext<TUser, IdentityRole, string>
        where TUser : IdentityUser
{
}

// Uses the built-in Identity types except with custom User and Role types
// The key type is defined by TKey
public class IdentityDbContext<TUser, TRole, TKey> : IdentityDbContext<
    TUser, TRole, TKey, IdentityUserClaim<TKey>, IdentityUserRole<TKey>,
    IdentityUserLogin<TKey>, IdentityRoleClaim<TKey>, IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TRole : IdentityRole<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments
// The key type is defined by TKey
public abstract class IdentityDbContext<
    TUser, TRole, TKey, TUserClaim, TUserRole, TUserLogin, TRoleClaim, TUserToken>
    : IdentityUserContext<TUser, TKey, TUserClaim, TUserLogin, TUserToken>
         where TUser : IdentityUser<TKey>
         where TRole : IdentityRole<TKey>
         where TKey : IEquatable<TKey>
         where TUserClaim : IdentityUserClaim<TKey>
         where TUserRole : IdentityUserRole<TKey>
         where TUserLogin : IdentityUserLogin<TKey>
         where TRoleClaim : IdentityRoleClaim<TKey>
         where TUserToken : IdentityUserToken<TKey>
```

Можно также использовать удостоверение без ролей (только утверждения), в этом случае <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUserContext`1> должен использоваться класс:

```csharp
// Uses the built-in non-role Identity types except with a custom User type
// Uses `string` as the key type
public class IdentityUserContext<TUser>
    : IdentityUserContext<TUser, string>
        where TUser : IdentityUser
{
}

// Uses the built-in non-role Identity types except with a custom User type
// The key type is defined by TKey
public class IdentityUserContext<TUser, TKey> : IdentityUserContext<
    TUser, TKey, IdentityUserClaim<TKey>, IdentityUserLogin<TKey>,
    IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments, with no roles
// The key type is defined by TKey
public abstract class IdentityUserContext<
    TUser, TKey, TUserClaim, TUserLogin, TUserToken> : DbContext
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
        where TUserClaim : IdentityUserClaim<TKey>
        where TUserLogin : IdentityUserLogin<TKey>
        where TUserToken : IdentityUserToken<TKey>
{
}
```

## <a name="customize-the-model"></a>Настройка модели

Отправной точкой для настройки модели является производным от типа соответствующего контекста. См. в разделе [универсальных типов модели](#model-generic-types) раздел. Этот тип контекста обычно вызывается `ApplicationDbContext` и создается в шаблонах ASP.NET Core.

Контекст используется для настройки модели двумя способами:

* Предоставление сущности и типы ключей для параметров универсального типа.
* Переопределение `OnModelCreating` изменение сопоставления этих типов.

При переопределении метода `OnModelCreating`, `base.OnModelCreating` необходимо сначала вызвать; переопределения конфигурации должен быть вызван, далее. EF Core обычно имеет политику побеждает последний — один для конфигурации. Например если `ToTable` сначала вызывается метод для типа сущности с именем одной таблицы и затем снова позднее с помощью другое имя таблицы, имя таблицы во втором вызове будет использоваться.

### <a name="custom-user-data"></a>Пользовательские данные

[Пользовательские данные](xref:security/authentication/add-user-data) поддерживается путем наследования от `IdentityUser`. Имя этого типа обычно `ApplicationUser`:


```csharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

Используйте `ApplicationUser` тип как универсальный аргумент для контекста:

```csharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

Нет необходимости в Переопределите `OnModelCreating` в `ApplicationDbContext` класса. Сопоставляет EF Core `CustomTag` свойство по соглашению. Тем не менее, требуется обновить, чтобы создать новую базу данных `CustomTag` столбца. Чтобы создать столбец, добавить миграцию и затем обновить базу данных, как описано в разделе [удостоверений и миграций EF Core](#identity-and-ef-core-migrations).

Обновление `Startup.ConfigureServices` использовать новый `ApplicationUser` класса:

::: moniker range=">= aspnetcore-2.1"

```csharp
services.AddDefaultIdentity<ApplicationUser>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultUI();
```

В ASP.NET Core 2.1 или более поздней версии удостоверение предоставляется как библиотека классов Razor. Дополнительные сведения см. в разделе <xref:security/authentication/scaffold-identity>. Следовательно, предыдущий код требует вызова <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>. Если удостоверение шаблон был использован для добавления в проект файлы, удалите вызов `AddDefaultUI`. Дополнительные сведения:

* [Удостоверение шаблона](xref:security/authentication/scaffold-identity)
* [Добавление, скачивание и удаление пользовательские данные для удостоверения](xref:security/authentication/add-user-data)

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();
```

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext, Guid>()
        .AddDefaultTokenProviders();
```

::: moniker-end

### <a name="change-the-primary-key-type"></a>Изменение типа первичного ключа

Изменение типа данных столбца первичного ключа после создания базы данных является проблемой для многих систем баз данных. Изменение первичный ключ обычно включает в себя удаление и повторное создание таблицы. Таким образом типы ключей должны быть заданы в первоначальной миграции, при создании базы данных.

Выполните следующие действия, чтобы изменить тип первичного ключа.

1. Если база данных была создана перед изменением первичного ключа, запустите `Drop-Database` (PMC) или `dotnet ef database drop` (CLI) .NET Core для его удаления.
2. После подтверждения удаления базы данных, удалите первоначальной миграции с `Remove-Migration` (PMC) или `dotnet ef migrations remove` (.NET Core CLI).
3. Обновление `ApplicationDbContext` класса для наследования от <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext`3>. Укажите новый тип ключа для `TKey`. Например, чтобы использовать `Guid` тип ключа:

    ```csharp
    public class ApplicationDbContext
        : IdentityDbContext<IdentityUser<Guid>, IdentityRole<Guid>, Guid>
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }
    }
    ```

    ::: moniker range=">= aspnetcore-2.0"

    В приведенном выше коде универсальные классы <xref:Microsoft.AspNetCore.Identity.IdentityUser`1> и <xref:Microsoft.AspNetCore.Identity.IdentityRole`1> должен быть указан для использования нового типа ключа.

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    В приведенном выше коде универсальные классы <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUser`1> и <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityRole`1> должен быть указан для использования нового типа ключа.

    ::: moniker-end

    `Startup.ConfigureServices` должны быть обновлены для использования универсального пользователя:

    ::: moniker range=">= aspnetcore-2.1"

    ```csharp
    services.AddDefaultIdentity<IdentityUser<Guid>>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    ```csharp
    services.AddIdentity<IdentityUser<Guid>, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    ```csharp
    services.AddIdentity<IdentityUser<Guid>, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext, Guid>()
            .AddDefaultTokenProviders();
    ```

    ::: moniker-end

4. Если пользовательский `ApplicationUser` класс используется, обновите наследование от класса `IdentityUser`. Пример:

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Models/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    ::: moniker range=">= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    Обновление `ApplicationDbContext` для ссылки на пользовательский `ApplicationUser` класса:

    ```csharp
    public class ApplicationDbContext
        : IdentityDbContext<ApplicationUser, IdentityRole<Guid>, Guid>
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }
    }
    ```

    Зарегистрируйте класс контекста пользовательской базы данных, при добавлении службы удостоверений в `Startup.ConfigureServices`:

    ::: moniker range=">= aspnetcore-2.1"

    ```csharp
    services.AddDefaultIdentity<ApplicationUser>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultUI()
            .AddDefaultTokenProviders();
    ```

    Тип данных первичный ключ определяется путем анализа <xref:Microsoft.EntityFrameworkCore.DbContext> объекта.

    В ASP.NET Core 2.1 или более поздней версии удостоверение предоставляется как библиотека классов Razor. Дополнительные сведения см. в разделе <xref:security/authentication/scaffold-identity>. Следовательно, предыдущий код требует вызова <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>. Если удостоверение шаблон был использован для добавления в проект файлы, удалите вызов `AddDefaultUI`.

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    Тип данных первичный ключ определяется путем анализа <xref:Microsoft.EntityFrameworkCore.DbContext> объекта.

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext, Guid>()
            .AddDefaultTokenProviders();
    ```

    <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> Метод принимает `TKey` тип первичного ключа.

    ::: moniker-end

5. Если пользовательский `ApplicationRole` класс используется, обновите наследование от класса `IdentityRole<TKey>`. Пример:

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationRole.cs?name=snippet_ApplicationRole&highlight=4)]

    Обновление `ApplicationDbContext` для ссылки на пользовательский `ApplicationRole` класса. Например, следующий класс ссылается на пользовательский `ApplicationUser` и пользовательское `ApplicationRole`:

    ::: moniker range=">= aspnetcore-2.1"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    Зарегистрируйте класс контекста пользовательской базы данных, при добавлении службы удостоверений в `Startup.ConfigureServices`:

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=13-16)]

    Тип данных первичный ключ определяется путем анализа <xref:Microsoft.EntityFrameworkCore.DbContext> объекта.

    В ASP.NET Core 2.1 или более поздней версии удостоверение предоставляется как библиотека классов Razor. Дополнительные сведения см. в разделе <xref:security/authentication/scaffold-identity>. Следовательно, предыдущий код требует вызова <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>. Если удостоверение шаблон был использован для добавления в проект файлы, удалите вызов `AddDefaultUI`.

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    Зарегистрируйте класс контекста пользовательской базы данных, при добавлении службы удостоверений в `Startup.ConfigureServices`:

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    Тип данных первичный ключ определяется путем анализа <xref:Microsoft.EntityFrameworkCore.DbContext> объекта.

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    Зарегистрируйте класс контекста пользовательской базы данных, при добавлении службы удостоверений в `Startup.ConfigureServices`:

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> Метод принимает `TKey` тип первичного ключа.

    ::: moniker-end

### <a name="add-navigation-properties"></a>Добавление свойства навигации

Изменение конфигурации модели для связи может оказаться сложнее, чем вносить другие изменения. Замените существующие связи, а не новые, создать дополнительные связи необходимо соблюдать осторожность. В частности измененные связи необходимо указать же свойства внешнего ключа (FK) как существующую связь. Например, связь между `Users` и `UserClaims` является, по умолчанию задан следующим образом:

```csharp
builder.Entity<TUser>(b =>
{
    // Each User can have many UserClaims
    b.HasMany<TUserClaim>()
     .WithOne()
     .HasForeignKey(uc => uc.UserId)
     .IsRequired();
});
```

Для этого отношения внешнего ключа указывается как `UserClaim.UserId` свойство. `HasMany` и `WithOne` можно вызвать без аргументов для создания связи без свойства навигации.

Добавьте свойство навигации, чтобы `ApplicationUser` , позволяющий связанные `UserClaims` ссылка от пользователя:

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

`TKey` Для `IdentityUserClaim<TKey>` — это тип, указанный для первичного ключа пользователей. В этом случае `TKey` является `string` так, как используются значения по умолчанию. Он имеет **не** тип первичного ключа для `UserClaim` типа сущности.

Теперь, когда существует свойство навигации, должно быть настроено в `OnModelCreating`:

```csharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();
        });
    }
}
```

Обратите внимание на то, что связь настроен так же, как ранее, только с помощью свойства навигации, указанного в вызове `HasMany`.

Свойства навигации существуют только в модель EF, а не в базе данных. Так как для связи внешнего ключа не изменилось, такое изменение модели не требует обновления базы данных. Это можно проверить путем добавления миграции после внесения изменений. `Up` И `Down` методы являются пустыми.

### <a name="add-all-user-navigation-properties"></a>Добавить все свойства навигации пользователя

Используя в качестве рекомендации в разделе выше, в следующем примере настраивается свойства односторонней навигации для всех отношений пользователя:

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
    public virtual ICollection<IdentityUserLogin<string>> Logins { get; set; }
    public virtual ICollection<IdentityUserToken<string>> Tokens { get; set; }
    public virtual ICollection<IdentityUserRole<string>> UserRoles { get; set; }
}
```

```csharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne()
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne()
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne()
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });
    }
}
```

### <a name="add-user-and-role-navigation-properties"></a>Добавление свойства навигации пользователя и роли

Используя в качестве рекомендации в разделе выше, в следующем примере настраивается свойства навигации для всех отношений на пользователя и роли:

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
    public virtual ICollection<IdentityUserLogin<string>> Logins { get; set; }
    public virtual ICollection<IdentityUserToken<string>> Tokens { get; set; }
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationRole : IdentityRole
{
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationUserRole : IdentityUserRole<string>
{
    public virtual ApplicationUser User { get; set; }
    public virtual ApplicationRole Role { get; set; }
}
```

```csharp
public class ApplicationDbContext
    : IdentityDbContext<
        ApplicationUser, ApplicationRole, string,
        IdentityUserClaim<string>, ApplicationUserRole, IdentityUserLogin<string>,
        IdentityRoleClaim<string>, IdentityUserToken<string>>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne()
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne()
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.User)
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });

        modelBuilder.Entity<ApplicationRole>(b =>
        {
            // Each Role can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.Role)
                .HasForeignKey(ur => ur.RoleId)
                .IsRequired();
        });

    }
}
```

Примечания.

* Этот пример также включает `UserRole` сущности, который необходим для перемещения по связи многие ко многим от пользователей к ролям соединения.
* Не забудьте изменить типы свойства навигации, соответствующим образом `ApplicationXxx` вместо теперь используются типы `IdentityXxx` типов.
* Не забывайте использовать `ApplicationXxx` в универсальном `ApplicationContext` определения.

### <a name="add-all-navigation-properties"></a>Добавить все свойства навигации

Используя в качестве рекомендации в разделе выше, в следующем примере настраивается свойства навигации для всех связей для всех типов сущностей:

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<ApplicationUserClaim> Claims { get; set; }
    public virtual ICollection<ApplicationUserLogin> Logins { get; set; }
    public virtual ICollection<ApplicationUserToken> Tokens { get; set; }
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationRole : IdentityRole
{
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
    public virtual ICollection<ApplicationRoleClaim> RoleClaims { get; set; }
}

public class ApplicationUserRole : IdentityUserRole<string>
{
    public virtual ApplicationUser User { get; set; }
    public virtual ApplicationRole Role { get; set; }
}

public class ApplicationUserClaim : IdentityUserClaim<string>
{
    public virtual ApplicationUser User { get; set; }
}

public class ApplicationUserLogin : IdentityUserLogin<string>
{
    public virtual ApplicationUser User { get; set; }
}

public class ApplicationRoleClaim : IdentityRoleClaim<string>
{
    public virtual ApplicationRole Role { get; set; }
}

public class ApplicationUserToken : IdentityUserToken<string>
{
    public virtual ApplicationUser User { get; set; }
}
```

```csharp
public class ApplicationDbContext
    : IdentityDbContext<
        ApplicationUser, ApplicationRole, string,
        ApplicationUserClaim, ApplicationUserRole, ApplicationUserLogin,
        ApplicationRoleClaim, ApplicationUserToken>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne(e => e.User)
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne(e => e.User)
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne(e => e.User)
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.User)
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });

        modelBuilder.Entity<ApplicationRole>(b =>
        {
            // Each Role can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.Role)
                .HasForeignKey(ur => ur.RoleId)
                .IsRequired();

            // Each Role can have many associated RoleClaims
            b.HasMany(e => e.RoleClaims)
                .WithOne(e => e.Role)
                .HasForeignKey(rc => rc.RoleId)
                .IsRequired();
        });
    }
}
```

### <a name="use-composite-keys"></a>Используйте составные ключи

Предыдущих разделах демонстрируется изменение типа ключа, используемого в модели удостоверения. Изменение ключа модели удостоверения с помощью составных ключей не поддерживается и не рекомендуется. С помощью составного ключа с удостоверением включает в себя изменение, как код Identity manager взаимодействует с моделью. Такая настройка выходит за рамки настоящего документа.

### <a name="change-tablecolumn-names-and-facets"></a>Изменения имен таблицы или столбца и аспекты

Чтобы изменить имена таблиц и столбцов, вызовите `base.OnModelCreating`. Затем добавьте конфигурацию, чтобы переопределить любые параметры по умолчанию. Например, для изменения имени всех таблиц удостоверений:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.ToTable("MyUsers");
    });

    modelBuilder.Entity<IdentityUserClaim<string>>(b =>
    {
        b.ToTable("MyUserClaims");
    });

    modelBuilder.Entity<IdentityUserLogin<string>>(b =>
    {
        b.ToTable("MyUserLogins");
    });

    modelBuilder.Entity<IdentityUserToken<string>>(b =>
    {
        b.ToTable("MyUserTokens");
    });

    modelBuilder.Entity<IdentityRole>(b =>
    {
        b.ToTable("MyRoles");
    });

    modelBuilder.Entity<IdentityRoleClaim<string>>(b =>
    {
        b.ToTable("MyRoleClaims");
    });

    modelBuilder.Entity<IdentityUserRole<string>>(b =>
    {
        b.ToTable("MyUserRoles");
    });
}
```

Эти примеры используют типы удостоверений по умолчанию. При использовании типа приложения, такие как `ApplicationUser`, настройка этого типа вместо типа по умолчанию.

В следующем примере изменяется некоторые имена столбцов:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.Property(e => e.Email).HasColumnName("EMail");
    });

    modelBuilder.Entity<IdentityUserClaim<string>>(b =>
    {
        b.Property(e => e.ClaimType).HasColumnName("CType");
        b.Property(e => e.ClaimValue).HasColumnName("CValue");
    });
}
```

Некоторые типы столбцов базы данных можно настроить с определенными *аспекты* (например, максимально `string` допустимая длина). В следующем примере задается максимальная длина столбца для нескольких `string` свойства в модели:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.Property(u => u.UserName).HasMaxLength(128);
        b.Property(u => u.NormalizedUserName).HasMaxLength(128);
        b.Property(u => u.Email).HasMaxLength(128);
        b.Property(u => u.NormalizedEmail).HasMaxLength(128);
    });

    modelBuilder.Entity<IdentityUserToken<string>>(b =>
    {
        b.Property(t => t.LoginProvider).HasMaxLength(128);
        b.Property(t => t.Name).HasMaxLength(128);
    });
}
```

### <a name="map-to-a-different-schema"></a>Сопоставление в другую схему

Схемы может вести себя по-разному для поставщиков базы данных. Для SQL Server по умолчанию является создание всех таблиц в *dbo* схемы. Таблицы могут создаваться в другой схеме. Пример:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

::: moniker range=">= aspnetcore-2.1"

### <a name="lazy-loading"></a>Отложенная загрузка

В этом разделе добавляется поддержка прокси отложенной загрузки в модель удостоверения. Lazy загрузка полезен в тех случаях, так как он позволяет использовать без убедитесь, что они при загрузке свойства навигации.

Типы сущностей можно сделать для отложенной загрузки несколькими способами, как описано в разделе [документации по EF Core](/ef/core/querying/related-data#lazy-loading). Для простоты используйте прокси отложенной загрузки, которая требует:

* Установка [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) пакета.
* Вызов <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseLazyLoadingProxies*> внутри <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext*>.
* Типы общей сущности с `public virtual` свойства навигации.

В следующем примере показан вызов `UseLazyLoadingProxies` в `Startup.ConfigureServices`:

```csharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
              .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

См. приведенные выше примеры, инструкции по добавлению свойств навигации в типы сущностей.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:security/authentication/scaffold-identity>

::: moniker-end
