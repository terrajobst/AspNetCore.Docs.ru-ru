---
title: Настройка модели удостоверения
author: ajcvickers
description: В этой статье описывается настройка базовой модели данных Entity Framework Core для ASP.NET Core Identity.
monikerRange: '>= aspnetcore-2.1'
ms.author: avickers
ms.date: 04/12/2018
uid: security/authentication/customize_identity_model
ms.openlocfilehash: 41ea125414c5997ee36f4e312beba4ff318a4a8d
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077684"
---
# <a name="identity-model-customization"></a><span data-ttu-id="3cec9-103">Настройка модели удостоверения</span><span class="sxs-lookup"><span data-stu-id="3cec9-103">Identity model customization</span></span>

<span data-ttu-id="3cec9-104">По [Vickers Артуром](https://github.com/ajcvickers)</span><span class="sxs-lookup"><span data-stu-id="3cec9-104">By [Arthur Vickers](https://github.com/ajcvickers)</span></span>

<span data-ttu-id="3cec9-105">Удостоверение ASP.NET Core предоставляет платформу для управления и хранения учетных записей пользователей в приложениях ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3cec9-105">ASP.NET Core Identity provides a framework for managing and storing user accounts in ASP.NET Core applications.</span></span> <span data-ttu-id="3cec9-106">Удостоверение добавляется в проект при выборе «Отдельных учетных записей пользователей» в качестве механизма проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="3cec9-106">Identity is added to your project when "Individual User Accounts" is selected as the authentication mechanism.</span></span> <span data-ttu-id="3cec9-107">По умолчанию удостоверение используются для Entity Framework (EF) основной модели данных.</span><span class="sxs-lookup"><span data-stu-id="3cec9-107">By default, Identity makes use of an Entity Framework (EF) Core data model.</span></span> <span data-ttu-id="3cec9-108">В этой статье описывается настройка модели удостоверения.</span><span class="sxs-lookup"><span data-stu-id="3cec9-108">This article describes how to customize the Identity model.</span></span>

<a name="identity-migrations"></a>

## <a name="identity-and-ef-core-migrations"></a><span data-ttu-id="3cec9-109">Удостоверение и EF Core миграции</span><span class="sxs-lookup"><span data-stu-id="3cec9-109">Identity and EF Core Migrations</span></span>

<span data-ttu-id="3cec9-110">Прежде чем изучать модели, полезно понять принципы работы идентификаторов с [миграций Core EF](/ef/core/managing-schemas/migrations/) для создания и обновления базы данных.</span><span class="sxs-lookup"><span data-stu-id="3cec9-110">Before examining the model, it's useful to understand how Identity works with [EF Core Migrations](/ef/core/managing-schemas/migrations/) to create and update a database.</span></span> <span data-ttu-id="3cec9-111">На верхнем уровне выполняется.</span><span class="sxs-lookup"><span data-stu-id="3cec9-111">At the top level, the process is:</span></span>

1. <span data-ttu-id="3cec9-112">Определение или обновление [модели данных в коде](/ef/core/modeling/).</span><span class="sxs-lookup"><span data-stu-id="3cec9-112">Define or update a [data model in code](/ef/core/modeling/).</span></span>
1. <span data-ttu-id="3cec9-113">Добавьте миграции преобразовать эту модель в изменения, которые могут быть применены к базе данных.</span><span class="sxs-lookup"><span data-stu-id="3cec9-113">Add a migration to translate this model into changes that can be applied to the database.</span></span>
1. <span data-ttu-id="3cec9-114">Проверьте, что миграция правильно представляет свои намерения.</span><span class="sxs-lookup"><span data-stu-id="3cec9-114">Check that the migration correctly represents your intentions.</span></span>
1. <span data-ttu-id="3cec9-115">Примените миграции для обновления базы данных должны быть синхронизированы с моделью.</span><span class="sxs-lookup"><span data-stu-id="3cec9-115">Apply the migration to update the database to be in sync with the model.</span></span>
1. <span data-ttu-id="3cec9-116">Повторите шаги 1 – 4 для дальнейшего уточнения модели и синхронизировать базу данных.</span><span class="sxs-lookup"><span data-stu-id="3cec9-116">Repeat steps 1-4 to further refine the model and keep the database in sync.</span></span>

<span data-ttu-id="3cec9-117">Миграция добавляются и применяются с помощью:</span><span class="sxs-lookup"><span data-stu-id="3cec9-117">Migrations are added and applied using:</span></span>

* <span data-ttu-id="3cec9-118">[Консоль диспетчера пакетов](/ef/core/miscellaneous/cli/powershell) (PMC), если вы используете Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3cec9-118">The [Package Manager Console](/ef/core/miscellaneous/cli/powershell) (PMC) if you are using Visual Studio.</span></span>
* <span data-ttu-id="3cec9-119">[Dotnet команды](/ef/core/miscellaneous/cli/dotnet) в командной строке.</span><span class="sxs-lookup"><span data-stu-id="3cec9-119">The [dotnet commands](/ef/core/miscellaneous/cli/dotnet) on the command line.</span></span>
* <span data-ttu-id="3cec9-120">При запуске приложения, выбрав ссылку миграции на странице ошибки.</span><span class="sxs-lookup"><span data-stu-id="3cec9-120">Selecting the migrations link on the error page when the application is run.</span></span>

<span data-ttu-id="3cec9-121">ASP.NET Core есть обработчик страницы ошибок во время разработки, который можно использовать для применения миграции при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="3cec9-121">ASP.NET Core has a development-time error page handler that can be used to apply migrations when the application is run.</span></span> <span data-ttu-id="3cec9-122">Для создания приложений, часто бывает необходимо создавать скрипты SQL миграций и их развертывание в базу данных как часть управляемой развертывания приложения и базы данных.</span><span class="sxs-lookup"><span data-stu-id="3cec9-122">For production applications, it is often more appropriate to generate SQL scripts from the migrations and deploy these to the database as part of a controlled application and database deployment.</span></span>

<span data-ttu-id="3cec9-123">Когда создается новое приложение с использованием удостоверения, уже выполнены действия 1 и 2.</span><span class="sxs-lookup"><span data-stu-id="3cec9-123">When a new application using Identity is created, steps 1 and 2 above have already been completed.</span></span> <span data-ttu-id="3cec9-124">То есть начальную модель данных уже существует, и первоначальной миграции был добавлен в проект.</span><span class="sxs-lookup"><span data-stu-id="3cec9-124">That is, the initial data model already exists, and the initial migration has been added to the project.</span></span> <span data-ttu-id="3cec9-125">Первоначальной миграции по-прежнему необходимо применить к базе данных.</span><span class="sxs-lookup"><span data-stu-id="3cec9-125">The initial migration still needs to be applied to the database.</span></span> <span data-ttu-id="3cec9-126">Первоначальной миграции можно сделать, запустив Update-Database (PMC), команда обновления (.NET Core CLI) dotnet ef базы данных или с помощью страницы ошибки при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="3cec9-126">The initial migration can be done by running the Update-Database (PMC), the dotnet ef database update (.NET Core CLI) command, or by using the error page when the application is run.</span></span>

<span data-ttu-id="3cec9-127">Описанные выше шаги должны повторяться при внесении изменений в модель.</span><span class="sxs-lookup"><span data-stu-id="3cec9-127">The preceding steps need to be repeated as changes are made to the model.</span></span>

<a name="identity-model"></a>

## <a name="the-identity-model"></a><span data-ttu-id="3cec9-128">Модель удостоверения</span><span class="sxs-lookup"><span data-stu-id="3cec9-128">The Identity model</span></span>

### <a name="entity-types"></a><span data-ttu-id="3cec9-129">Типы сущностей</span><span class="sxs-lookup"><span data-stu-id="3cec9-129">Entity types</span></span>

<span data-ttu-id="3cec9-130">Модель удостоверения состоит из семи типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="3cec9-130">The Identity model consists of seven entity types:</span></span>

* <span data-ttu-id="3cec9-131">`User` -представляет пользователя</span><span class="sxs-lookup"><span data-stu-id="3cec9-131">`User` - represents the user</span></span>
* <span data-ttu-id="3cec9-132">`Role` -Представляет роль</span><span class="sxs-lookup"><span data-stu-id="3cec9-132">`Role` - represents a role</span></span>
* <span data-ttu-id="3cec9-133">`UserClaim` -Представляет утверждения, которые пользователь обладал</span><span class="sxs-lookup"><span data-stu-id="3cec9-133">`UserClaim` - represents a claim that a user possess</span></span>
* <span data-ttu-id="3cec9-134">`UserToken` -Представляет маркер проверки подлинности для пользователя</span><span class="sxs-lookup"><span data-stu-id="3cec9-134">`UserToken` - represents an authentication token for a user</span></span>
* <span data-ttu-id="3cec9-135">`UserLogin` — связывает пользователя с именем входа</span><span class="sxs-lookup"><span data-stu-id="3cec9-135">`UserLogin` - associates a user with a login</span></span>
* <span data-ttu-id="3cec9-136">`RoleClaim` -Представляет утверждения, которые предоставляются всем пользователям в роли</span><span class="sxs-lookup"><span data-stu-id="3cec9-136">`RoleClaim` - represents a claim that is granted to all users within a role</span></span>
* <span data-ttu-id="3cec9-137">`UserRole` -присоединить сущность, которая связывает пользователей и ролей</span><span class="sxs-lookup"><span data-stu-id="3cec9-137">`UserRole` - join entity that associates users and roles</span></span>

### <a name="entity-type-relationships"></a><span data-ttu-id="3cec9-138">Связи между типами сущностей</span><span class="sxs-lookup"><span data-stu-id="3cec9-138">Entity type relationships</span></span>

<span data-ttu-id="3cec9-139">Эти типы сущностей связаны друг с другом одним из следующих способов:</span><span class="sxs-lookup"><span data-stu-id="3cec9-139">These entity types are related to each other in the following ways:</span></span>

* <span data-ttu-id="3cec9-140">Каждый `User` может иметь множество `UserClaims`</span><span class="sxs-lookup"><span data-stu-id="3cec9-140">Each `User` can have many `UserClaims`</span></span>
* <span data-ttu-id="3cec9-141">Каждый `User` может иметь множество `UserLogins`</span><span class="sxs-lookup"><span data-stu-id="3cec9-141">Each `User` can have many `UserLogins`</span></span>
* <span data-ttu-id="3cec9-142">Каждый `User` может иметь множество `UserTokens`</span><span class="sxs-lookup"><span data-stu-id="3cec9-142">Each `User` can have many `UserTokens`</span></span>
* <span data-ttu-id="3cec9-143">Каждый `Role` может иметь множество связанных `RoleClaims`</span><span class="sxs-lookup"><span data-stu-id="3cec9-143">Each `Role` can have many associated `RoleClaims`</span></span>
* <span data-ttu-id="3cec9-144">Каждый `User` может иметь множество связанных `Roles`и каждый `Role` могут быть связаны с большим числом пользователей</span><span class="sxs-lookup"><span data-stu-id="3cec9-144">Each `User` can have many associated `Roles`, and each `Role` can be associated with many Users</span></span>
  * <span data-ttu-id="3cec9-145">Это отношение многие ко многим, требующий Соединяемая таблица в базе данных.</span><span class="sxs-lookup"><span data-stu-id="3cec9-145">This is a many-to-many relationship, which requires a join table in the database.</span></span> <span data-ttu-id="3cec9-146">Соединяемая таблица представляется `UserRole` сущности.</span><span class="sxs-lookup"><span data-stu-id="3cec9-146">The join table is represented by the `UserRole` entity.</span></span>

### <a name="default-model-configuration"></a><span data-ttu-id="3cec9-147">Конфигурация модели по умолчанию</span><span class="sxs-lookup"><span data-stu-id="3cec9-147">Default model configuration</span></span>

<span data-ttu-id="3cec9-148">Удостоверение определяет ряд «контекст классы», которые наследуют от [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) для настройки и использования модели.</span><span class="sxs-lookup"><span data-stu-id="3cec9-148">Identity defines a variety of "context classes" which inherit from [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) to configure and use the model.</span></span> <span data-ttu-id="3cec9-149">Эта настройка выполняется с помощью [EF Core первый Fluent API Code](/ef/core/modeling/) в [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_) метод класса контекста.</span><span class="sxs-lookup"><span data-stu-id="3cec9-149">This configuration is done using the [EF Core Code First Fluent API](/ef/core/modeling/) in the [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_) method of the context class.</span></span> <span data-ttu-id="3cec9-150">Конфигурация по умолчанию —:</span><span class="sxs-lookup"><span data-stu-id="3cec9-150">The default configuration is:</span></span>

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

### <a name="model-generic-types"></a><span data-ttu-id="3cec9-151">Универсальные типы модели</span><span class="sxs-lookup"><span data-stu-id="3cec9-151">Model generic types</span></span>

<span data-ttu-id="3cec9-152">Удостоверение определяет типы CLR по умолчанию для каждого из перечисленных выше типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="3cec9-152">Identity defines default CLR types for each of the entity types listed above.</span></span> <span data-ttu-id="3cec9-153">Эти типы начинаются с «Удостоверение»: `IdentityUser`, `IdentityRole`, `IdentityUserClaim`, `IdentityUserToken`, `IdentityUserLogin`, `IdentityRoleClaim`, и `IdentityUserRole`.</span><span class="sxs-lookup"><span data-stu-id="3cec9-153">These types are all prefixed with "Identity": `IdentityUser`, `IdentityRole`, `IdentityUserClaim`, `IdentityUserToken`, `IdentityUserLogin`, `IdentityRoleClaim`, and `IdentityUserRole`.</span></span>

<span data-ttu-id="3cec9-154">Вместо непосредственного использования этих типов, типов можно использовать как базовые классы для типов приложения.</span><span class="sxs-lookup"><span data-stu-id="3cec9-154">Rather than using these types directly, the types can be used as base classes for the application's own types.</span></span> <span data-ttu-id="3cec9-155">`DbContext` Классов, определенных перечислением удостоверения, являются универсальными, таким образом, что для одного или нескольких типов сущностей в модели можно использовать различные типы среды CLR.</span><span class="sxs-lookup"><span data-stu-id="3cec9-155">The `DbContext` classes defined by Identity are generic such that different CLR types can be used for one or more of the entity types in the model.</span></span> <span data-ttu-id="3cec9-156">Изменение этих универсальных типах также позволяют для типа первичного ключа пользователя.</span><span class="sxs-lookup"><span data-stu-id="3cec9-156">These generic types also allow for the type of the User primary key to be changed.</span></span>

<span data-ttu-id="3cec9-157">При использовании идентификаторов с поддержкой для ролей, [IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext) должен использоваться класс:</span><span class="sxs-lookup"><span data-stu-id="3cec9-157">When using Identity with support for roles, an [IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext) class should be used:</span></span>

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

<span data-ttu-id="3cec9-158">Можно также использовать удостоверение не ролей (только утверждения), в этом случае `IdentityUserContext` должен использоваться класс:</span><span class="sxs-lookup"><span data-stu-id="3cec9-158">It is also possible to use Identity without roles (only claims), in which case an `IdentityUserContext` class should be used:</span></span>


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

## <a name="customizing-the-model"></a><span data-ttu-id="3cec9-159">Настройка модели</span><span class="sxs-lookup"><span data-stu-id="3cec9-159">Customizing the model</span></span>

<span data-ttu-id="3cec9-160">Является отправной точкой для настройки модели является производным от типа соответствующего контекста; в предыдущем разделе.</span><span class="sxs-lookup"><span data-stu-id="3cec9-160">The starting point for customizing the model is to derive from the appropriate context type; see the preceding section.</span></span> <span data-ttu-id="3cec9-161">Этот тип контекста обычно вызывается `ApplicationDbContext` и созданных шаблонами ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3cec9-161">This context type is customarily called `ApplicationDbContext` and is created by the ASP.NET Core templates.</span></span>

<span data-ttu-id="3cec9-162">Контекст используется для настройки модели двумя способами:</span><span class="sxs-lookup"><span data-stu-id="3cec9-162">The context is used to configure the model in two ways:</span></span>
* <span data-ttu-id="3cec9-163">Указывая типы сущностей и ключ для параметров универсального типа.</span><span class="sxs-lookup"><span data-stu-id="3cec9-163">By supplying entity and key types for the generic type parameters.</span></span>
* <span data-ttu-id="3cec9-164">Путем переопределения `OnModelCreating` изменение сопоставления этих типов.</span><span class="sxs-lookup"><span data-stu-id="3cec9-164">By overriding `OnModelCreating` to modify the mapping of these types.</span></span>

<span data-ttu-id="3cec9-165">При переопределении метода `OnModelCreating`, `base.OnModelCreating` должен вызываться сначала метод переопределения конфигурации должен быть вызван рядом.</span><span class="sxs-lookup"><span data-stu-id="3cec9-165">When overriding `OnModelCreating`, `base.OnModelCreating` should be called first, the overriding configuration should be called next.</span></span> <span data-ttu-id="3cec9-166">EF Core обычно применяется политика побеждает последний один для конфигурации.</span><span class="sxs-lookup"><span data-stu-id="3cec9-166">EF Core generally has a last-one-wins policy for configuration.</span></span> <span data-ttu-id="3cec9-167">Например если `ToTable` для сущности типа вызванной сначала с именем одну таблицу и затем снова более поздней версии, и другое имя таблицы, а затем имя таблицы во втором вызове является тот, который используется.</span><span class="sxs-lookup"><span data-stu-id="3cec9-167">For example, if the `ToTable` for an entity type is called first with one table name and then again later with a different table name, then the table name in the second call is the one that is used.</span></span>

### <a name="using-a-custom-user-type"></a><span data-ttu-id="3cec9-168">С помощью пользовательского типа пользователя</span><span class="sxs-lookup"><span data-stu-id="3cec9-168">Using a custom User type</span></span>

<span data-ttu-id="3cec9-169">Чтобы использовать пользовательский тип пользователя, создайте тип и его наследуют `IdentityUser`.</span><span class="sxs-lookup"><span data-stu-id="3cec9-169">To use a custom User type, create the type and have it inherit from `IdentityUser`.</span></span> <span data-ttu-id="3cec9-170">Обычно этот тип имени `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="3cec9-170">It is customary to name this type `ApplicationUser`.</span></span> <span data-ttu-id="3cec9-171">Этот тип обычно имеют дополнительные свойства не в базовом типе, в противном случае отсутствует значение не при его создании.</span><span class="sxs-lookup"><span data-stu-id="3cec9-171">This type will typically have additional properties not in the base type, otherwise there would be no value in creating it.</span></span> <span data-ttu-id="3cec9-172">Пример:</span><span class="sxs-lookup"><span data-stu-id="3cec9-172">For example:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

<span data-ttu-id="3cec9-173">Затем используйте этот тип как универсальный аргумент для контекста:</span><span class="sxs-lookup"><span data-stu-id="3cec9-173">Next use this type as a generic argument for the context:</span></span>

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

<span data-ttu-id="3cec9-174">Обновление `ConfigureServices` использовать новый `ApplicationUser` класса:</span><span class="sxs-lookup"><span data-stu-id="3cec9-174">Update `ConfigureServices` to use the new `ApplicationUser` class:</span></span>

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="3cec9-175">Нет необходимости переопределять `OnModelCreating` здесь, так как основной EF сопоставит `CustomTag` свойство по соглашению.</span><span class="sxs-lookup"><span data-stu-id="3cec9-175">There is no need to override `OnModelCreating` here because EF Core will map the `CustomTag` property by convention.</span></span> <span data-ttu-id="3cec9-176">Однако базы данных необходимо обновить для получения нового `CustomTag` столбца.</span><span class="sxs-lookup"><span data-stu-id="3cec9-176">However, the database will need to be updated to get a new `CustomTag` column.</span></span> <span data-ttu-id="3cec9-177">Чтобы сделать это, добавьте миграции и затем обновить базу данных, как описано в [удостоверений и миграции Core EF](#identity-migrations).</span><span class="sxs-lookup"><span data-stu-id="3cec9-177">To do this, add a migration, and then update the database as described in  [Identity and EF Core Migrations](#identity-migrations).</span></span>

### <a name="changing-the-key-type"></a><span data-ttu-id="3cec9-178">Изменение типа ключа</span><span class="sxs-lookup"><span data-stu-id="3cec9-178">Changing the key type</span></span>

<span data-ttu-id="3cec9-179">Изменение типа столбца первичного ключа (PK) после создания базы данных, затруднительно для многих систем баз данных.</span><span class="sxs-lookup"><span data-stu-id="3cec9-179">Changing the type of a primary key (PK) column after the database has been created is problematic on many database systems.</span></span> <span data-ttu-id="3cec9-180">Изменение первичного ключа обычно включает в себя удаление и повторное создание таблицы.</span><span class="sxs-lookup"><span data-stu-id="3cec9-180">Changing the PK typically involves dropping and re-creating the table.</span></span> <span data-ttu-id="3cec9-181">Таким образом рекомендуется указать типы ключей в первоначальной миграции таким образом, что типы целевого объекта ключа создаются при создании базы данных.</span><span class="sxs-lookup"><span data-stu-id="3cec9-181">Therefore, it is recommended that key types be specified in the initial migration such that the target key types are created when the database is created.</span></span>

<span data-ttu-id="3cec9-182">Если базы данных будет создан, затем `Drop-Database` (PMC) или `dotnet ef database drop` (.NET Core CLI) может использоваться для его удаления.</span><span class="sxs-lookup"><span data-stu-id="3cec9-182">If the database has been created, then `Drop-Database` (PMC) or `dotnet ef database drop` (.NET Core CLI) can be used to delete it.</span></span>

<span data-ttu-id="3cec9-183">После подтверждения, что база данных не существует, удалите первоначальной миграции с `Remove-Migration` (PMC) или `dotnet ef migrations remove` (.NET Core CLI).</span><span class="sxs-lookup"><span data-stu-id="3cec9-183">Once it is confirmed that the database doesn't exist,  remove the initial migration with `Remove-Migration` (PMC) or `dotnet ef migrations remove` (.NET Core CLI).</span></span>

<span data-ttu-id="3cec9-184">Обновление `ApplicationDbContext` для использования другой базовый класс, указав новый тип ключа для `TKey`.</span><span class="sxs-lookup"><span data-stu-id="3cec9-184">Update the `ApplicationDbContext` to use a different base class, specifying the new key type for `TKey`.</span></span> <span data-ttu-id="3cec9-185">Например, чтобы использовать `Guid` ключ:</span><span class="sxs-lookup"><span data-stu-id="3cec9-185">For example, to use a `Guid` key:</span></span>

```CSharp
public class ApplicationDbContext : IdentityDbContext<IdentityUser<Guid>, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

<span data-ttu-id="3cec9-186">Обратите внимание, что универсальные классы `IdentityUser<TKey>` и `IdentityRole<TKey>` также должен быть указан для использования нового ключа типа.</span><span class="sxs-lookup"><span data-stu-id="3cec9-186">Notice that the generic classes `IdentityUser<TKey>` and `IdentityRole<TKey>` must also be specified to use the new key type.</span></span> <span data-ttu-id="3cec9-187">`ConfigureServices` должны быть обновлены для использования универсального пользователя:</span><span class="sxs-lookup"><span data-stu-id="3cec9-187">`ConfigureServices` must be updated to use the generic user:</span></span>

```CSharp
services.AddDefaultIdentity<IdentityUser<Guid>>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="3cec9-188">Если в настраиваемом `ApplicationUser` — используется, обновить его, чтобы наследовать от `IdentityUser<TKey>`.</span><span class="sxs-lookup"><span data-stu-id="3cec9-188">If a custom `ApplicationUser` is being used, update it to inherit from `IdentityUser<TKey>`.</span></span> <span data-ttu-id="3cec9-189">Пример:</span><span class="sxs-lookup"><span data-stu-id="3cec9-189">For example:</span></span>

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

### <a name="adding-navigation-properties"></a><span data-ttu-id="3cec9-190">Добавление свойства навигации</span><span class="sxs-lookup"><span data-stu-id="3cec9-190">Adding navigation properties</span></span>

<span data-ttu-id="3cec9-191">Изменение конфигурации модели для связи может быть сложнее, чем других изменений.</span><span class="sxs-lookup"><span data-stu-id="3cec9-191">Changing the model configuration for relationships can be a more difficult than making other changes.</span></span> <span data-ttu-id="3cec9-192">Чтобы заменить существующие связи, а не создавать дополнительные связи необходимо соблюдать осторожность.</span><span class="sxs-lookup"><span data-stu-id="3cec9-192">Care must be taken to replace the existing relationships rather than create a new additional relationships.</span></span> <span data-ttu-id="3cec9-193">В частности измененные связи необходимо указать то же свойство внешнего ключа в качестве существующие связи.</span><span class="sxs-lookup"><span data-stu-id="3cec9-193">In particular, the changed relationship must specify the same foreign key property as the existing relationship.</span></span> <span data-ttu-id="3cec9-194">Например, связь между `Users` и `UserClaims` по умолчанию, указанный как:</span><span class="sxs-lookup"><span data-stu-id="3cec9-194">For example, the relationship between `Users` and `UserClaims` is by default specified as:</span></span>

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

<span data-ttu-id="3cec9-195">Внешний ключ для этого отношения указывается как `UserClaim.UserId` свойство.</span><span class="sxs-lookup"><span data-stu-id="3cec9-195">The foreign key for this relationship is specified as the `UserClaim.UserId` property.</span></span> <span data-ttu-id="3cec9-196">`HasMany` и `WithOne` вызывается без аргументов для создания связи без свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="3cec9-196">`HasMany` and `WithOne` are called without arguments to create the relationship without navigation properties.</span></span>

<span data-ttu-id="3cec9-197">Свойство навигации, чтобы добавить `ApplicationUser` это даст возможность связанные `UserClaims` должно быть указано пользователем:</span><span class="sxs-lookup"><span data-stu-id="3cec9-197">Add a navigation property to `ApplicationUser` that will allow associated `UserClaims` to be referenced from the user:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

<span data-ttu-id="3cec9-198">Обратите внимание, что `TKey` для `IdentityUserClaim<TKey>` — тип, указанный для первичного ключа пользователи — в данном случае `string` так как мы используем значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3cec9-198">Note that the `TKey` for `IdentityUserClaim<TKey>` is type specified for the primary key of users--in this case `string` since we're using the defaults.</span></span> <span data-ttu-id="3cec9-199">Это **не** тип первичного ключа для `UserClaim` типа сущности.</span><span class="sxs-lookup"><span data-stu-id="3cec9-199">It is **not** the primary key type for the `UserClaim` entity type.</span></span>

<span data-ttu-id="3cec9-200">Теперь, когда существует свойство навигации он должен быть настроен в `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="3cec9-200">Now that the navigation property exists it must be configured in `OnModelCreating`:</span></span>

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

<span data-ttu-id="3cec9-201">Обратите внимание, что связь настроен точно так, как он был ранее, только с помощью свойства навигации, указанного в вызове `HasMany`.</span><span class="sxs-lookup"><span data-stu-id="3cec9-201">Notice that relationship is configured exactly as it was before, only with a navigation property specified in the call to `HasMany`.</span></span>

<span data-ttu-id="3cec9-202">Свойства навигации существуют только в модели EF, а не в базе данных.</span><span class="sxs-lookup"><span data-stu-id="3cec9-202">The navigation properties only exist in the EF model, not the database.</span></span> <span data-ttu-id="3cec9-203">Так как внешний ключ для связи не изменился, такое изменение модели не требуется обновление базы данных.</span><span class="sxs-lookup"><span data-stu-id="3cec9-203">Because the foreign key for the relationship has not changed, this kind of model change does not require the database to be updated.</span></span> <span data-ttu-id="3cec9-204">Это можно проверить путем добавления к миграции после внесения изменений: `Up` и `Down` метода являются пустыми.</span><span class="sxs-lookup"><span data-stu-id="3cec9-204">This can be checked by adding a migration after making the change: the `Up` and `Down` methods are empty.</span></span>

### <a name="adding-all-user-navigation-properties"></a><span data-ttu-id="3cec9-205">Добавление всех свойств навигации пользователя</span><span class="sxs-lookup"><span data-stu-id="3cec9-205">Adding all User navigation properties</span></span>

<span data-ttu-id="3cec9-206">С помощью раздела выше как рекомендации, ниже приведен пример, предназначенный для настройки свойств однонаправленный навигации для всех связей на пользователя:</span><span class="sxs-lookup"><span data-stu-id="3cec9-206">Using the section above as guidance, here is an example that configures unidirectional navigation properties for all relationships on User:</span></span>

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

### <a name="adding-user-and-role-navigation-properties"></a><span data-ttu-id="3cec9-207">Добавление свойств навигации пользователя и роли</span><span class="sxs-lookup"><span data-stu-id="3cec9-207">Adding User and Role navigation properties</span></span>

<span data-ttu-id="3cec9-208">С помощью раздела выше как рекомендации, ниже приведен пример, предназначенный для настройки свойств навигации для всех связей для пользователя и роли:</span><span class="sxs-lookup"><span data-stu-id="3cec9-208">Using the section above as guidance, here is an example that configures navigation properties for all relationships on User and Role:</span></span>

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

<span data-ttu-id="3cec9-209">Примечания.</span><span class="sxs-lookup"><span data-stu-id="3cec9-209">Notes:</span></span>

* <span data-ttu-id="3cec9-210">В этом примере также включает `UserRole` присоединить сущность, которая необходима для перемещения по связи многие ко многим от пользователей к ролям.</span><span class="sxs-lookup"><span data-stu-id="3cec9-210">This example also includes the `UserRole` join entity, which is needed to navigate the many-to-many relationship from Users to Roles.</span></span>
* <span data-ttu-id="3cec9-211">Не забудьте изменить типы свойств навигации в соответствии с тем `ApplicationXxx` типы теперь используются вместо `IdentityXxx` типов.</span><span class="sxs-lookup"><span data-stu-id="3cec9-211">Remember to change the types of the navigation properties to reflect that `ApplicationXxx` types are now being used instead of `IdentityXxx` types.</span></span>
* <span data-ttu-id="3cec9-212">Не забывайте использовать `ApplicationXxx` в универсальном `ApplicationContext` определения.</span><span class="sxs-lookup"><span data-stu-id="3cec9-212">Remember to use the `ApplicationXxx` in the generic `ApplicationContext` definition.</span></span>

### <a name="adding-all-navigation-properties"></a><span data-ttu-id="3cec9-213">Добавление всех свойств навигации</span><span class="sxs-lookup"><span data-stu-id="3cec9-213">Adding all navigation properties</span></span>

<span data-ttu-id="3cec9-214">С помощью раздела выше как рекомендации, ниже приведен пример, предназначенный для настройки свойств навигации для всех связей для всех типов сущностей:</span><span class="sxs-lookup"><span data-stu-id="3cec9-214">Using the section above as guidance, here is an example that configures navigation properties for all relationships on all entity types:</span></span>

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

### <a name="using-composite-keys"></a><span data-ttu-id="3cec9-215">С помощью составных ключей</span><span class="sxs-lookup"><span data-stu-id="3cec9-215">Using composite keys</span></span>

<span data-ttu-id="3cec9-216">Предыдущих разделах демонстрируется изменение типа ключа, используемого в модели удостоверения.</span><span class="sxs-lookup"><span data-stu-id="3cec9-216">The preceding sections demonstrated changing the type of key used in the Identity model.</span></span> <span data-ttu-id="3cec9-217">Изменение модели идентификатора ключа с помощью составных ключей не поддерживается и не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="3cec9-217">Changing the Identity key model to use composite keys is not supported or recommended.</span></span> <span data-ttu-id="3cec9-218">С помощью составного ключа с удостоверением будет включать в себя изменение, как код диспетчера удостоверений взаимодействует с моделью, которая не поддерживается настройка и выходит за рамки настоящего документа.</span><span class="sxs-lookup"><span data-stu-id="3cec9-218">Using a composite key with Identity would involve changing how the Identity manager code interacts with the model, which is an unsupported customization and beyond the scope of this document.</span></span>

### <a name="changing-tablecolumn-names-and-facets"></a><span data-ttu-id="3cec9-219">Изменение таблицы или столбца имен и аспекты</span><span class="sxs-lookup"><span data-stu-id="3cec9-219">Changing table/column names and facets</span></span>

<span data-ttu-id="3cec9-220">Чтобы изменить имена таблиц и столбцов, вызовите `base.OnModelCreating`, а затем добавьте конфигурацию, чтобы переопределить любые параметры по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3cec9-220">To change the names of tables and columns, call `base.OnModelCreating`, and then add configuration to override any of the defaults.</span></span> <span data-ttu-id="3cec9-221">Например, для изменения имени все таблицы удостоверений:</span><span class="sxs-lookup"><span data-stu-id="3cec9-221">For example, to change the name of all the identity tables:</span></span>

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

<span data-ttu-id="3cec9-222">Обратите внимание, что в этих примерах используются типы по умолчанию удостоверение.</span><span class="sxs-lookup"><span data-stu-id="3cec9-222">Note that these examples use the default Identity types.</span></span> <span data-ttu-id="3cec9-223">При использовании типа приложения, такие как `ApplicationUser` настройте типа вместо типа по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3cec9-223">If you are using an application type, such as `ApplicationUser` then configure that type instead of the default type.</span></span>

<span data-ttu-id="3cec9-224">Ниже приведен пример, который изменяет некоторые имена столбцов:</span><span class="sxs-lookup"><span data-stu-id="3cec9-224">Here is an example that changes some column names:</span></span>

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

<span data-ttu-id="3cec9-225">Некоторые типы столбцов базы данных можно настроить с определенными _аспекты_ например строки максимально допустимую длину.</span><span class="sxs-lookup"><span data-stu-id="3cec9-225">Some types of database columns can be configured with certain _facets_ such as the maximum string length allowed.</span></span> <span data-ttu-id="3cec9-226">Ниже приведен пример, в котором задает максимальной длины столбцов для нескольких свойств строки в модели:</span><span class="sxs-lookup"><span data-stu-id="3cec9-226">Here is an example that sets column max lengths for several string properties in the model:</span></span>

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

### <a name="mapping-to-a-different-schema"></a><span data-ttu-id="3cec9-227">Сопоставление в другую схему</span><span class="sxs-lookup"><span data-stu-id="3cec9-227">Mapping to a different schema</span></span>

<span data-ttu-id="3cec9-228">Схемы могут вести себя по-разному в другую базу данных поставщиков, но для SQL Server по умолчанию используется для создания всех таблиц в схеме «dbo».</span><span class="sxs-lookup"><span data-stu-id="3cec9-228">Schemas can behave differently in different database providers, but for SQL Server, the default is to create all tables in the "dbo" schema.</span></span> <span data-ttu-id="3cec9-229">Это можно изменить, чтобы вместо этого создать таблицы в другую схему.</span><span class="sxs-lookup"><span data-stu-id="3cec9-229">This can be changed to instead create the tables in a different schema.</span></span> <span data-ttu-id="3cec9-230">Пример:</span><span class="sxs-lookup"><span data-stu-id="3cec9-230">For example:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

### <a name="lazy-loading"></a><span data-ttu-id="3cec9-231">«Неспешная» загрузка</span><span class="sxs-lookup"><span data-stu-id="3cec9-231">Lazy loading</span></span>

<span data-ttu-id="3cec9-232">В этом разделе добавляется поддержка прокси-серверов отложенную загрузку в модель удостоверения.</span><span class="sxs-lookup"><span data-stu-id="3cec9-232">In this section support for lazy-loading proxies in the Identity model is added.</span></span> <span data-ttu-id="3cec9-233">Загрузка Lazy полезно, так как он позволяет использовать без убедитесь, что они загружаются свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="3cec9-233">Lazy-loading is useful since it allows navigation properties to be used without first ensuring they are loaded.</span></span>

<span data-ttu-id="3cec9-234">Типы сущностей можно сделать подходит для отложенной загрузки несколькими способами, как описано в [EF основная документация](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="3cec9-234">Entity types can be made suitable for lazy-loading in several ways, as described in the [EF Core documentation](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="3cec9-235">Для простоты мы будем использовать отложенную загрузку прокси-серверы, что требует:</span><span class="sxs-lookup"><span data-stu-id="3cec9-235">For simplicity, we will use lazy-loading proxies, which requires:</span></span>

* <span data-ttu-id="3cec9-236">Установка [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) пакета.</span><span class="sxs-lookup"><span data-stu-id="3cec9-236">Installation of the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package.</span></span>
* <span data-ttu-id="3cec9-237">Вызов `.UseLazyLoadingProxies()` внутри `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="3cec9-237">A call to `.UseLazyLoadingProxies()` inside `AddDbContext`.</span></span>
* <span data-ttu-id="3cec9-238">Типы сущностей открытый со свойствами навигации открытый виртуальный.</span><span class="sxs-lookup"><span data-stu-id="3cec9-238">Public entity types with public virtual navigation properties.</span></span>

<span data-ttu-id="3cec9-239">Ниже приведен пример вызова `.UseLazyLoadingProxies()`:</span><span class="sxs-lookup"><span data-stu-id="3cec9-239">Here is an example of calling `.UseLazyLoadingProxies()`:</span></span>

```CSharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
            .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="3cec9-240">Вернуться в приведенных выше примерах для добавления свойств навигации для типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="3cec9-240">Refer back to the examples above for adding navigation properties to the entity types.</span></span>
