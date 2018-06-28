---
title: Настройка модели удостоверения
author: ajcvickers
description: В этой статье описывается настройка базовой модели данных Entity Framework Core для ASP.NET Core Identity.
ms.author: avickers
ms.date: 04/12/2018
ms.prod: asp.net-core
uid: security/authentication/customize_identity_model
ms.openlocfilehash: b44b4fd0f24d245b969588a7226ea6aacbe2a722
ms.sourcegitcommit: 7003d27b607e529642ded0400aa48ae692a0e666
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036901"
---
# <a name="identity-model-customization"></a>Настройка модели удостоверения

По [Vickers Артуром](https://github.com/ajcvickers)

Удостоверение ASP.NET Core предоставляет платформу для управления и хранения учетных записей пользователей в приложениях ASP.NET Core. Удостоверение добавляется в проект при выборе «Отдельных учетных записей пользователей» в качестве механизма проверки подлинности. По умолчанию удостоверение используются для Entity Framework (EF) основной модели данных. В этой статье описывается настройка модели удостоверения.

<a name="identity-migrations"></a>

## <a name="identity-and-ef-core-migrations"></a>Удостоверение и EF Core миграции

Прежде чем изучать модели, полезно понять принципы работы идентификаторов с [миграций Core EF](/ef/core/managing-schemas/migrations/) для создания и обновления базы данных. На верхнем уровне выполняется.

1. Определение или обновление [модели данных в коде](/ef/core/modeling/).
1. Добавьте миграции преобразовать эту модель в изменения, которые могут быть применены к базе данных.
1. Проверьте, что миграция правильно представляет свои намерения.
1. Примените миграции для обновления базы данных должны быть синхронизированы с моделью.
1. Повторите шаги 1 – 4 для дальнейшего уточнения модели и синхронизировать базу данных.

Миграция добавляются и применяются с помощью:

* [Консоль диспетчера пакетов](/ef/core/miscellaneous/cli/powershell) (PMC), если вы используете Visual Studio.
* [Dotnet команды](/ef/core/miscellaneous/cli/dotnet) в командной строке.
* При запуске приложения, выбрав ссылку миграции на странице ошибки.

ASP.NET Core есть обработчик страницы ошибок во время разработки, который можно использовать для применения миграции при запуске приложения. Для создания приложений, часто бывает необходимо создавать скрипты SQL миграций и их развертывание в базу данных как часть управляемой развертывания приложения и базы данных.

Когда создается новое приложение с использованием удостоверения, уже выполнены действия 1 и 2. То есть начальную модель данных уже существует, и первоначальной миграции был добавлен в проект. Первоначальной миграции по-прежнему необходимо применить к базе данных. Первоначальной миграции можно сделать, запустив Update-Database (PMC), команда обновления (.NET Core CLI) dotnet ef базы данных или с помощью страницы ошибки при запуске приложения.

Описанные выше шаги должны повторяться при внесении изменений в модель.

<a name="identity-model"></a>

## <a name="the-identity-model"></a>Модель удостоверения

### <a name="entity-types"></a>Типы сущностей

Модель удостоверения состоит из семи типов сущностей.

* `User` -представляет пользователя
* `Role` -Представляет роль
* `UserClaim` -Представляет утверждения, которые пользователь обладал
* `UserToken` -Представляет маркер проверки подлинности для пользователя
* `UserLogin` — связывает пользователя с именем входа
* `RoleClaim` -Представляет утверждения, которые предоставляются всем пользователям в роли
* `UserRole` -присоединить сущность, которая связывает пользователей и ролей

### <a name="entity-type-relationships"></a>Связи между типами сущностей

Эти типы сущностей связаны друг с другом одним из следующих способов:

* Каждый `User` может иметь множество `UserClaims`
* Каждый `User` может иметь множество `UserLogins`
* Каждый `User` может иметь множество `UserTokens`
* Каждый `Role` может иметь множество связанных `RoleClaims`
* Каждый `User` может иметь множество связанных `Roles`и каждый `Role` могут быть связаны с большим числом пользователей
  * Это отношение многие ко многим, требующий Соединяемая таблица в базе данных. Соединяемая таблица представляется `UserRole` сущности.

### <a name="default-model-configuration"></a>Конфигурация модели по умолчанию

Удостоверение определяет ряд «контекст классы», которые наследуют от [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) для настройки и использования модели. Эта настройка выполняется с помощью [EF Core первый Fluent API Code](/ef/core/modeling/) в [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_) метод класса контекста. Конфигурация по умолчанию —:

```CSharp
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

    // Limit the size of the composite key columns due to common database restrictions
    b.Property(l => l.LoginProvider).HasMaxLength(128);
    b.Property(l => l.ProviderKey).HasMaxLength(128);

    // Maps to the AspNetUserLogins table
    b.ToTable("AspNetUserLogins");
});

builder.Entity<TUserToken>(b =>
{
    // Composite primary key consisting of the UserId, LoginProvider and Name
    b.HasKey(t => new { t.UserId, t.LoginProvider, t.Name });

    // Limit the size of the composite key columns due to common database restrictions
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

Удостоверение определяет типы CLR по умолчанию для каждого из перечисленных выше типов сущностей. Эти типы начинаются с «Удостоверение»: `IdentityUser`, `IdentityRole`, `IdentityUserClaim`, `IdentityUserToken`, `IdentityUserLogin`, `IdentityRoleClaim`, и `IdentityUserRole`.

Вместо непосредственного использования этих типов, типов можно использовать как базовые классы для типов приложения. `DbContext` Классов, определенных перечислением удостоверения, являются универсальными, таким образом, что для одного или нескольких типов сущностей в модели можно использовать различные типы среды CLR. Изменение этих универсальных типах также позволяют для типа первичного ключа пользователя.

При использовании идентификаторов с поддержкой для ролей, [IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext) должен использоваться класс:

```CSharp
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
public class IdentityDbContext<TUser, TRole, TKey>
    : IdentityDbContext<TUser, TRole, TKey, IdentityUserClaim<TKey>, IdentityUserRole<TKey>, IdentityUserLogin<TKey>, IdentityRoleClaim<TKey>, IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TRole : IdentityRole<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments
// The key type is defined by TKey
public abstract class IdentityDbContext<TUser, TRole, TKey, TUserClaim, TUserRole, TUserLogin, TRoleClaim, TUserToken>
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

Можно также использовать удостоверение не ролей (только утверждения), в этом случае `IdentityUserContext` должен использоваться класс:


```CSharp
// Uses the built-in non-role Identity types except with a custom User type
// Uses `string` as the key type
public class IdentityUserContext<TUser>
    : IdentityUserContext<TUser, string>
        where TUser : IdentityUser
{
}

// Uses the built-in non-role Identity types except with a custom User type
// The key type is defined by TKey
public class IdentityUserContext<TUser, TKey>
    : IdentityUserContext<TUser, TKey, IdentityUserClaim<TKey>, IdentityUserLogin<TKey>, IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments, with no roles
// The key type is defined by TKey
public abstract class IdentityUserContext<TUser, TKey, TUserClaim, TUserLogin, TUserToken>
    : DbContext
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
        where TUserClaim : IdentityUserClaim<TKey>
        where TUserLogin : IdentityUserLogin<TKey>
        where TUserToken : IdentityUserToken<TKey>
{
}
```

## <a name="customizing-the-model"></a>Настройка модели

Является отправной точкой для настройки модели является производным от типа соответствующего контекста; в предыдущем разделе. Этот тип контекста обычно вызывается `ApplicationDbContext` и созданных шаблонами ASP.NET Core.

Контекст используется для настройки модели двумя способами:
* Указывая типы сущностей и ключ для параметров универсального типа.
* Путем переопределения `OnModelCreating` изменение сопоставления этих типов.

При переопределении метода `OnModelCreating`, `base.OnModelCreating` должен вызываться сначала метод переопределения конфигурации должен быть вызван рядом. EF Core обычно применяется политика побеждает последний один для конфигурации. Например если `ToTable` для сущности типа вызванной сначала с именем одну таблицу и затем снова более поздней версии, и другое имя таблицы, а затем имя таблицы во втором вызове является тот, который используется.

### <a name="using-a-custom-user-type"></a>С помощью пользовательского типа пользователя

Чтобы использовать пользовательский тип пользователя, создайте тип и его наследуют `IdentityUser`. Обычно этот тип имени `ApplicationUser`. Этот тип обычно имеют дополнительные свойства не в базовом типе, в противном случае отсутствует значение не при его создании. Пример:

```CSharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

Затем используйте этот тип как универсальный аргумент для контекста:

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

Обновление `ConfigureServices` использовать новый `ApplicationUser` класса:

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Нет необходимости переопределять `OnModelCreating` здесь, так как основной EF сопоставит `CustomTag` свойство по соглашению. Однако базы данных необходимо обновить для получения нового `CustomTag` столбца. Чтобы сделать это, добавьте миграции и затем обновить базу данных, как описано в [удостоверений и миграции Core EF](#identity-migrations).

### <a name="changing-the-key-type"></a>Изменение типа ключа

Изменение типа столбца первичного ключа (PK) после создания базы данных, затруднительно для многих систем баз данных. Изменение первичного ключа обычно включает в себя удаление и повторное создание таблицы. Таким образом рекомендуется указать типы ключей в первоначальной миграции таким образом, что типы целевого объекта ключа создаются при создании базы данных.

Если базы данных будет создан, затем `Drop-Database` (PMC) или `dotnet ef database drop` (.NET Core CLI) может использоваться для его удаления.

После подтверждения, что база данных не существует, удалите первоначальной миграции с `Remove-Migration` (PMC) или `dotnet ef migrations remove` (.NET Core CLI).

Обновление `ApplicationDbContext` для использования другой базовый класс, указав новый тип ключа для `TKey`. Например, чтобы использовать `Guid` ключ:

```CSharp
public class ApplicationDbContext : IdentityDbContext<IdentityUser<Guid>, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

Обратите внимание, что универсальные классы `IdentityUser<TKey>` и `IdentityRole<TKey>` также должен быть указан для использования нового ключа типа. `ConfigureServices` должны быть обновлены для использования универсального пользователя:

```CSharp
services.AddDefaultIdentity<IdentityUser<Guid>>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Если в настраиваемом `ApplicationUser` — используется, обновить его, чтобы наследовать от `IdentityUser<TKey>`. Пример:

```CSharp
public class ApplicationUser : IdentityUser<Guid>
{
    public string CustomTag { get; set; }
}

public class ApplicationDbContext : IdentityDbContext<ApplicationUser, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

### <a name="adding-navigation-properties"></a>Добавление свойства навигации

Изменение конфигурации модели для связи может быть сложнее, чем других изменений. Чтобы заменить существующие связи, а не создавать дополнительные связи необходимо соблюдать осторожность. В частности измененные связи необходимо указать то же свойство внешнего ключа в качестве существующие связи. Например, связь между `Users` и `UserClaims` по умолчанию, указанный как:

```CSharp
builder.Entity<TUser>(b =>
{
    // Each User can have many UserClaims
    b.HasMany<TUserClaim>()
     .WithOne()
     .HasForeignKey(uc => uc.UserId)
     .IsRequired();
});
```

Внешний ключ для этого отношения указывается как `UserClaim.UserId` свойство. `HasMany` и `WithOne` вызывается без аргументов для создания связи без свойства навигации.

Свойство навигации, чтобы добавить `ApplicationUser` это даст возможность связанные `UserClaims` должно быть указано пользователем:

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

Обратите внимание, что `TKey` для `IdentityUserClaim<TKey>` — тип, указанный для первичного ключа пользователи — в данном случае `string` так как мы используем значения по умолчанию. Это **не** тип первичного ключа для `UserClaim` типа сущности.

Теперь, когда существует свойство навигации он должен быть настроен в `OnModelCreating`:

```CSharp
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

Обратите внимание, что связь настроен точно так, как он был ранее, только с помощью свойства навигации, указанного в вызове `HasMany`.

Свойства навигации существуют только в модели EF, а не в базе данных. Так как внешний ключ для связи не изменился, такое изменение модели не требуется обновление базы данных. Это можно проверить путем добавления к миграции после внесения изменений: `Up` и `Down` метода являются пустыми.

### <a name="adding-all-user-navigation-properties"></a>Добавление всех свойств навигации пользователя

С помощью раздела выше как рекомендации, ниже приведен пример, предназначенный для настройки свойств однонаправленный навигации для всех связей на пользователя:

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
    public virtual ICollection<IdentityUserLogin<string>> Logins { get; set; }
    public virtual ICollection<IdentityUserToken<string>> Tokens { get; set; }
    public virtual ICollection<IdentityUserRole<string>> UserRoles { get; set; }
}
```

```CSharp
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

### <a name="adding-user-and-role-navigation-properties"></a>Добавление свойств навигации пользователя и роли

С помощью раздела выше как рекомендации, ниже приведен пример, предназначенный для настройки свойств навигации для всех связей для пользователя и роли:

```CSharp
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

```CSharp
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

* В этом примере также включает `UserRole` присоединить сущность, которая необходима для перемещения по связи многие ко многим от пользователей к ролям.
* Не забудьте изменить типы свойств навигации в соответствии с тем `ApplicationXxx` типы теперь используются вместо `IdentityXxx` типов.
* Не забывайте использовать `ApplicationXxx` в универсальном `ApplicationContext` определения.

### <a name="adding-all-navigation-properties"></a>Добавление всех свойств навигации

С помощью раздела выше как рекомендации, ниже приведен пример, предназначенный для настройки свойств навигации для всех связей для всех типов сущностей:

```CSharp
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

```CSharp
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

### <a name="using-composite-keys"></a>С помощью составных ключей

Предыдущих разделах демонстрируется изменение типа ключа, используемого в модели удостоверения. Изменение модели идентификатора ключа с помощью составных ключей не поддерживается и не рекомендуется. С помощью составного ключа с удостоверением будет включать в себя изменение, как код диспетчера удостоверений взаимодействует с моделью, которая не поддерживается настройка и выходит за рамки настоящего документа.

### <a name="changing-tablecolumn-names-and-facets"></a>Изменение таблицы или столбца имен и аспекты

Чтобы изменить имена таблиц и столбцов, вызовите `base.OnModelCreating`, а затем добавьте конфигурацию, чтобы переопределить любые параметры по умолчанию. Например, для изменения имени все таблицы удостоверений:

```CSharp
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

Обратите внимание, что в этих примерах используются типы по умолчанию удостоверение. При использовании типа приложения, такие как `ApplicationUser` настройте типа вместо типа по умолчанию.

Ниже приведен пример, который изменяет некоторые имена столбцов:

```CSharp
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

Некоторые типы столбцов базы данных можно настроить с определенными _аспекты_ например строки максимально допустимую длину. Ниже приведен пример, в котором задает максимальной длины столбцов для нескольких свойств строки в модели:

```CSharp
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

### <a name="mapping-to-a-different-schema"></a>Сопоставление в другую схему

Схемы могут вести себя по-разному в другую базу данных поставщиков, но для SQL Server по умолчанию используется для создания всех таблиц в схеме «dbo». Это можно изменить, чтобы вместо этого создать таблицы в другую схему. Пример:

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

### <a name="lazy-loading"></a>«Неспешная» загрузка

В этом разделе добавляется поддержка прокси-серверов отложенную загрузку в модель удостоверения. Загрузка Lazy полезно, так как он позволяет использовать без убедитесь, что они загружаются свойства навигации.

Типы сущностей можно сделать подходит для отложенной загрузки несколькими способами, как описано в [EF основная документация](/ef/core/querying/related-data#lazy-loading). Для простоты мы будем использовать отложенную загрузку прокси-серверы, что требует:

* Установка [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) пакета.
* Вызов `.UseLazyLoadingProxies()` внутри `AddDbContext`.
* Типы сущностей открытый со свойствами навигации открытый виртуальный.

Ниже приведен пример вызова `.UseLazyLoadingProxies()`:

```CSharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
            .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Вернуться в приведенных выше примерах для добавления свойств навигации для типов сущностей.
