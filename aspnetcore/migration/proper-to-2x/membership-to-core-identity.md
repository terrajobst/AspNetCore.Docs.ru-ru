---
title: Перенос из проверки подлинности членства ASP.NET, в удостоверение ASP.NET Core 2.0
author: isaac2004
description: Сведения о переносе существующих приложений ASP.NET с использованием проверки подлинности членства в ASP.NET Core 2.0 Identity.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/10/2019
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: 3b708da13ff9f2887eee87ea17844312a4fe1b8d
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2019
ms.locfileid: "58264723"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a><span data-ttu-id="257ea-103">Перенос из проверки подлинности членства ASP.NET, в удостоверение ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="257ea-103">Migrate from ASP.NET Membership authentication to ASP.NET Core 2.0 Identity</span></span>

<span data-ttu-id="257ea-104">Автор [Айзек Левин](https://isaaclevin.com) (Isaac Levin)</span><span class="sxs-lookup"><span data-stu-id="257ea-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="257ea-105">В этой статье показано, перенос схемы базы данных для приложений ASP.NET с использованием проверки подлинности членства в ASP.NET Core 2.0 Identity.</span><span class="sxs-lookup"><span data-stu-id="257ea-105">This article demonstrates migrating the database schema for ASP.NET apps using Membership authentication to ASP.NET Core 2.0 Identity.</span></span>

> [!NOTE]
> <span data-ttu-id="257ea-106">В этом документе описываются действия, необходимые для переноса схемы базы данных для приложения на основе членства ASP.NET со схемой базы данных, используемый для удостоверения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="257ea-106">This document provides the steps needed to migrate the database schema for ASP.NET Membership-based apps to the database schema used for ASP.NET Core Identity.</span></span> <span data-ttu-id="257ea-107">Дополнительные сведения о переходе с проверки подлинности на основе членства ASP.NET на ASP.NET Identity см. в разделе [перенести существующее приложение из членства SQL в ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span><span class="sxs-lookup"><span data-stu-id="257ea-107">For more information about migrating from ASP.NET Membership-based authentication to ASP.NET Identity, see [Migrate an existing app from SQL Membership to ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span></span> <span data-ttu-id="257ea-108">Дополнительные сведения о ASP.NET Core Identity, см. в разделе [Общие сведения об Identity в ASP.NET Core](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="257ea-108">For more information about ASP.NET Core Identity, see [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity).</span></span>

## <a name="review-of-membership-schema"></a><span data-ttu-id="257ea-109">Обзор схемы членства</span><span class="sxs-lookup"><span data-stu-id="257ea-109">Review of Membership schema</span></span>

<span data-ttu-id="257ea-110">До ASP.NET 2.0 разработчики были заниматься созданием весь процесс проверки подлинности и авторизации для своих приложений.</span><span class="sxs-lookup"><span data-stu-id="257ea-110">Prior to ASP.NET 2.0, developers were tasked with creating the entire authentication and authorization process for their apps.</span></span> <span data-ttu-id="257ea-111">В ASP.NET 2.0 появилась членства, предоставляя стандартный решение по обработке безопасности в приложениях ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="257ea-111">With ASP.NET 2.0, Membership was introduced, providing a boilerplate solution to handling security within ASP.NET apps.</span></span> <span data-ttu-id="257ea-112">Теперь бы разработчики могли для начальной загрузки схемы в базу данных SQL Server с [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) команды.</span><span class="sxs-lookup"><span data-stu-id="257ea-112">Developers were now able to bootstrap a schema into a SQL Server database with the [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) command.</span></span> <span data-ttu-id="257ea-113">После выполнения этой команды, следующие таблицы были созданы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="257ea-113">After running this command, the following tables were created in the database.</span></span>

  ![Таблицы членства](identity/_static/membership-tables.png)

<span data-ttu-id="257ea-115">Чтобы перенести существующие приложения в удостоверение ASP.NET Core 2.0, данные в этих таблицах требует переноса таблиц, используемых новой схемы идентификации.</span><span class="sxs-lookup"><span data-stu-id="257ea-115">To migrate existing apps to ASP.NET Core 2.0 Identity, the data in these tables needs to be migrated to the tables used by the new Identity schema.</span></span>

## <a name="aspnet-core-identity-20-schema"></a><span data-ttu-id="257ea-116">ASP.NET Core Identity 2.0 схемы</span><span class="sxs-lookup"><span data-stu-id="257ea-116">ASP.NET Core Identity 2.0 schema</span></span>

<span data-ttu-id="257ea-117">ASP.NET Core 2.0 соответствует [удостоверений](/aspnet/identity/index) принцип, появившиеся в ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="257ea-117">ASP.NET Core 2.0 follows the [Identity](/aspnet/identity/index) principle introduced in ASP.NET 4.5.</span></span> <span data-ttu-id="257ea-118">На то, что принцип является общим, реализация между платформами, отличается, даже между версиями ASP.NET Core (см. в разделе [миграция проверки подлинности и удостоверения в ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span><span class="sxs-lookup"><span data-stu-id="257ea-118">Though the principle is shared, the implementation between the frameworks is different, even between versions of ASP.NET Core (see [Migrate authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span></span>

<span data-ttu-id="257ea-119">Самый быстрый способ просмотра схемы для удостоверения ASP.NET Core 2.0 является создание нового приложения ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="257ea-119">The fastest way to view the schema for ASP.NET Core 2.0 Identity is to create a new ASP.NET Core 2.0 app.</span></span> <span data-ttu-id="257ea-120">Выполните следующие действия в Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="257ea-120">Follow these steps in Visual Studio 2017:</span></span>

1. <span data-ttu-id="257ea-121">Выберите **Файл** > **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="257ea-121">Select **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="257ea-122">Создайте новый **веб-приложение ASP.NET Core** проект с именем *CoreIdentitySample*.</span><span class="sxs-lookup"><span data-stu-id="257ea-122">Create a new **ASP.NET Core Web Application** project named *CoreIdentitySample*.</span></span>
1. <span data-ttu-id="257ea-123">Выберите **ASP.NET Core 2.0** в раскрывающемся списке, а затем выберите **веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="257ea-123">Select **ASP.NET Core 2.0** in the dropdown and then select **Web Application**.</span></span> <span data-ttu-id="257ea-124">Этот шаблон создает [Razor Pages](xref:razor-pages/index) приложения.</span><span class="sxs-lookup"><span data-stu-id="257ea-124">This template produces a [Razor Pages](xref:razor-pages/index) app.</span></span> <span data-ttu-id="257ea-125">Перед нажатием кнопки **ОК**, нажмите кнопку **изменить способ проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="257ea-125">Before clicking **OK**, click **Change Authentication**.</span></span>
1. <span data-ttu-id="257ea-126">Выберите **учетные записи отдельных пользователей** для идентификации шаблонов.</span><span class="sxs-lookup"><span data-stu-id="257ea-126">Choose **Individual User Accounts** for the Identity templates.</span></span> <span data-ttu-id="257ea-127">Наконец, нажмите кнопку **ОК**, затем **ОК**.</span><span class="sxs-lookup"><span data-stu-id="257ea-127">Finally, click **OK**, then **OK**.</span></span> <span data-ttu-id="257ea-128">Visual Studio создает проект с помощью шаблона ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="257ea-128">Visual Studio creates a project using the ASP.NET Core Identity template.</span></span>
1. <span data-ttu-id="257ea-129">Выберите **средства** > **диспетчер пакетов NuGet** > **консоль диспетчера пакетов** открыть **консоль диспетчера пакетов** Окно (PMC).</span><span class="sxs-lookup"><span data-stu-id="257ea-129">Select **Tools** > **NuGet Package Manager** > **Package Manager Console** to open the **Package Manager Console** (PMC) window.</span></span>
1. <span data-ttu-id="257ea-130">Перейдите в корневую папку проекта в PMC и запустите [Entity Framework (EF) Core](/ef/core) `Update-Database` команды.</span><span class="sxs-lookup"><span data-stu-id="257ea-130">Navigate to the project root in PMC, and run the [Entity Framework (EF) Core](/ef/core) `Update-Database` command.</span></span>

    <span data-ttu-id="257ea-131">Удостоверение ASP.NET Core 2.0 использует EF Core для взаимодействия с базой данных, хранение данных проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="257ea-131">ASP.NET Core 2.0 Identity uses EF Core to interact with the database storing the authentication data.</span></span> <span data-ttu-id="257ea-132">Чтобы созданное приложение для работы должен быть базой данных для хранения этих данных.</span><span class="sxs-lookup"><span data-stu-id="257ea-132">In order for the newly created app to work, there needs to be a database to store this data.</span></span> <span data-ttu-id="257ea-133">После создания нового приложения, то самый быстрый способ проверить схему в среде базы данных является создание базы данных с помощью [миграций EF Core](/ef/core/managing-schemas/migrations/).</span><span class="sxs-lookup"><span data-stu-id="257ea-133">After creating a new app, the fastest way to inspect the schema in a database environment is to create the database using [EF Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="257ea-134">Этот процесс создает базу данных, либо локально или в другом месте, что повторяет эту схему.</span><span class="sxs-lookup"><span data-stu-id="257ea-134">This process creates a database, either locally or elsewhere, which mimics that schema.</span></span> <span data-ttu-id="257ea-135">В документации по предыдущей Дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="257ea-135">Review the preceding documentation for more information.</span></span>

    <span data-ttu-id="257ea-136">Команды EF Core используйте строку подключения для базы данных, указанной в *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="257ea-136">EF Core commands use the connection string for the database specified in *appsettings.json*.</span></span> <span data-ttu-id="257ea-137">Следующая строка подключения связанного с базой данных на *localhost* с именем *asp-net-core-identity*.</span><span class="sxs-lookup"><span data-stu-id="257ea-137">The following connection string targets a database on *localhost* named *asp-net-core-identity*.</span></span> <span data-ttu-id="257ea-138">В этом параметре, EF Core настроен на использование `DefaultConnection` строку подключения.</span><span class="sxs-lookup"><span data-stu-id="257ea-138">In this setting, EF Core is configured to use the `DefaultConnection` connection string.</span></span>

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
      }
    }
    ```

1. <span data-ttu-id="257ea-139">Выберите **представление** > **обозреватель объектов SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="257ea-139">Select **View** > **SQL Server Object Explorer**.</span></span> <span data-ttu-id="257ea-140">Разверните узел, соответствующий имени базы данных, указанному в `ConnectionStrings:DefaultConnection` свойство *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="257ea-140">Expand the node corresponding to the database name specified in the `ConnectionStrings:DefaultConnection` property of *appsettings.json*.</span></span>

    <span data-ttu-id="257ea-141">`Update-Database` Команда создала базы данных, указанной с помощью схемы и все данные, необходимые для инициализации приложения.</span><span class="sxs-lookup"><span data-stu-id="257ea-141">The `Update-Database` command created the database specified with the schema and any data needed for app initialization.</span></span> <span data-ttu-id="257ea-142">На следующем рисунке изображены структуру таблицы, созданного в предыдущих шагах.</span><span class="sxs-lookup"><span data-stu-id="257ea-142">The following image depicts the table structure that's created with the preceding steps.</span></span>

    ![Удостоверение таблиц](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a><span data-ttu-id="257ea-144">Перенос схемы</span><span class="sxs-lookup"><span data-stu-id="257ea-144">Migrate the schema</span></span>

<span data-ttu-id="257ea-145">Существуют небольшие отличия в структуры таблиц и полей для членства и удостоверения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="257ea-145">There are subtle differences in the table structures and fields for both Membership and ASP.NET Core Identity.</span></span> <span data-ttu-id="257ea-146">Шаблон значительно изменяется для проверки подлинности и авторизации с приложениями ASP.NET и ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="257ea-146">The pattern has changed substantially for authentication/authorization with ASP.NET and ASP.NET Core apps.</span></span> <span data-ttu-id="257ea-147">Основные объекты, которые по-прежнему используются с удостоверением *пользователей* и *ролей*.</span><span class="sxs-lookup"><span data-stu-id="257ea-147">The key objects that are still used with Identity are *Users* and *Roles*.</span></span> <span data-ttu-id="257ea-148">Ниже приведены таблицы сопоставления для *пользователей*, *ролей*, и *UserRoles*.</span><span class="sxs-lookup"><span data-stu-id="257ea-148">Here are mapping tables for *Users*, *Roles*, and *UserRoles*.</span></span>

### <a name="users"></a><span data-ttu-id="257ea-149">Пользователи</span><span class="sxs-lookup"><span data-stu-id="257ea-149">Users</span></span>

|<span data-ttu-id="257ea-150">*Удостоверение<br>(dbo. AspNetUsers)*</span><span class="sxs-lookup"><span data-stu-id="257ea-150">*Identity<br>(dbo.AspNetUsers)*</span></span>        ||<span data-ttu-id="257ea-151">*Членство<br>(dbo.aspnet_Users / dbo.aspnet_Membership)*</span><span class="sxs-lookup"><span data-stu-id="257ea-151">*Membership<br>(dbo.aspnet_Users / dbo.aspnet_Membership)*</span></span>||
|----------------------------------------|-----------------------------------------------------------|
|<span data-ttu-id="257ea-152">**Имя поля**</span><span class="sxs-lookup"><span data-stu-id="257ea-152">**Field Name**</span></span>                 |<span data-ttu-id="257ea-153">**Type**</span><span class="sxs-lookup"><span data-stu-id="257ea-153">**Type**</span></span>|<span data-ttu-id="257ea-154">**Имя поля**</span><span class="sxs-lookup"><span data-stu-id="257ea-154">**Field Name**</span></span>                                    |<span data-ttu-id="257ea-155">**Type**</span><span class="sxs-lookup"><span data-stu-id="257ea-155">**Type**</span></span>|
|`Id`                           |<span data-ttu-id="257ea-156">string</span><span class="sxs-lookup"><span data-stu-id="257ea-156">string</span></span>  |`aspnet_Users.UserId`                             |<span data-ttu-id="257ea-157">string</span><span class="sxs-lookup"><span data-stu-id="257ea-157">string</span></span>  |
|`UserName`                     |<span data-ttu-id="257ea-158">string</span><span class="sxs-lookup"><span data-stu-id="257ea-158">string</span></span>  |`aspnet_Users.UserName`                           |<span data-ttu-id="257ea-159">string</span><span class="sxs-lookup"><span data-stu-id="257ea-159">string</span></span>  |
|`Email`                        |<span data-ttu-id="257ea-160">string</span><span class="sxs-lookup"><span data-stu-id="257ea-160">string</span></span>  |`aspnet_Membership.Email`                         |<span data-ttu-id="257ea-161">string</span><span class="sxs-lookup"><span data-stu-id="257ea-161">string</span></span>  |
|`NormalizedUserName`           |<span data-ttu-id="257ea-162">string</span><span class="sxs-lookup"><span data-stu-id="257ea-162">string</span></span>  |`aspnet_Users.LoweredUserName`                    |<span data-ttu-id="257ea-163">string</span><span class="sxs-lookup"><span data-stu-id="257ea-163">string</span></span>  |
|`NormalizedEmail`              |<span data-ttu-id="257ea-164">string</span><span class="sxs-lookup"><span data-stu-id="257ea-164">string</span></span>  |`aspnet_Membership.LoweredEmail`                  |<span data-ttu-id="257ea-165">string</span><span class="sxs-lookup"><span data-stu-id="257ea-165">string</span></span>  |
|`PhoneNumber`                  |<span data-ttu-id="257ea-166">string</span><span class="sxs-lookup"><span data-stu-id="257ea-166">string</span></span>  |`aspnet_Users.MobileAlias`                        |<span data-ttu-id="257ea-167">string</span><span class="sxs-lookup"><span data-stu-id="257ea-167">string</span></span>  |
|`LockoutEnabled`               |<span data-ttu-id="257ea-168">bit</span><span class="sxs-lookup"><span data-stu-id="257ea-168">bit</span></span>     |`aspnet_Membership.IsLockedOut`                   |<span data-ttu-id="257ea-169">bit</span><span class="sxs-lookup"><span data-stu-id="257ea-169">bit</span></span>     |

> [!NOTE]
> <span data-ttu-id="257ea-170">Не все сопоставления полей похожи на отношения один к одному из членства в ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="257ea-170">Not all the field mappings resemble one-to-one relationships from Membership to ASP.NET Core Identity.</span></span> <span data-ttu-id="257ea-171">Приведенной выше таблице, используется схема по умолчанию авторизованного пользователя и сопоставляет его схемы удостоверения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="257ea-171">The preceding table takes the default Membership User schema and maps it to the ASP.NET Core Identity schema.</span></span> <span data-ttu-id="257ea-172">Другие настраиваемые поля, которые были использованы для членства должны быть добавлены вручную.</span><span class="sxs-lookup"><span data-stu-id="257ea-172">Any other custom fields that were used for Membership need to be mapped manually.</span></span> <span data-ttu-id="257ea-173">В этом сопоставлении нет отсутствует сопоставление для паролей, как требования к паролям и соли пароля не будут перемещаться между ними.</span><span class="sxs-lookup"><span data-stu-id="257ea-173">In this mapping, there's no map for passwords, as both password criteria and password salts don't migrate between the two.</span></span> <span data-ttu-id="257ea-174">**Рекомендуется оставлять пароль как null и предлагать пользователям сбрасывать свои пароли.**</span><span class="sxs-lookup"><span data-stu-id="257ea-174">**It's recommended to leave the password as null and to ask users to reset their passwords.**</span></span> <span data-ttu-id="257ea-175">В ASP.NET Core Identity `LockoutEnd` следует задавать на некоторую дату в будущем, если пользователь будет заблокирован. Это показано в сценарии миграции.</span><span class="sxs-lookup"><span data-stu-id="257ea-175">In ASP.NET Core Identity, `LockoutEnd` should be set to some date in the future if the user is locked out. This is shown in the migration script.</span></span>

### <a name="roles"></a><span data-ttu-id="257ea-176">Роли</span><span class="sxs-lookup"><span data-stu-id="257ea-176">Roles</span></span>

|<span data-ttu-id="257ea-177">*Удостоверение<br>(dbo. AspNetRoles)*</span><span class="sxs-lookup"><span data-stu-id="257ea-177">*Identity<br>(dbo.AspNetRoles)*</span></span>        ||<span data-ttu-id="257ea-178">*Членство<br>(dbo.aspnet_Roles)*</span><span class="sxs-lookup"><span data-stu-id="257ea-178">*Membership<br>(dbo.aspnet_Roles)*</span></span>||
|----------------------------------------|-----------------------------------|
|<span data-ttu-id="257ea-179">**Имя поля**</span><span class="sxs-lookup"><span data-stu-id="257ea-179">**Field Name**</span></span>                 |<span data-ttu-id="257ea-180">**Type**</span><span class="sxs-lookup"><span data-stu-id="257ea-180">**Type**</span></span>|<span data-ttu-id="257ea-181">**Имя поля**</span><span class="sxs-lookup"><span data-stu-id="257ea-181">**Field Name**</span></span>   |<span data-ttu-id="257ea-182">**Type**</span><span class="sxs-lookup"><span data-stu-id="257ea-182">**Type**</span></span>         |
|`Id`                           |<span data-ttu-id="257ea-183">string</span><span class="sxs-lookup"><span data-stu-id="257ea-183">string</span></span>  |`RoleId`         | <span data-ttu-id="257ea-184">string</span><span class="sxs-lookup"><span data-stu-id="257ea-184">string</span></span>          |
|`Name`                         |<span data-ttu-id="257ea-185">string</span><span class="sxs-lookup"><span data-stu-id="257ea-185">string</span></span>  |`RoleName`       | <span data-ttu-id="257ea-186">string</span><span class="sxs-lookup"><span data-stu-id="257ea-186">string</span></span>          |
|`NormalizedName`               |<span data-ttu-id="257ea-187">string</span><span class="sxs-lookup"><span data-stu-id="257ea-187">string</span></span>  |`LoweredRoleName`| <span data-ttu-id="257ea-188">string</span><span class="sxs-lookup"><span data-stu-id="257ea-188">string</span></span>          |

### <a name="user-roles"></a><span data-ttu-id="257ea-189">Роли пользователей</span><span class="sxs-lookup"><span data-stu-id="257ea-189">User Roles</span></span>

|<span data-ttu-id="257ea-190">*Identity<br>(dbo.AspNetUserRoles)*</span><span class="sxs-lookup"><span data-stu-id="257ea-190">*Identity<br>(dbo.AspNetUserRoles)*</span></span>||<span data-ttu-id="257ea-191">*Членство<br>(dbo.aspnet_UsersInRoles)*</span><span class="sxs-lookup"><span data-stu-id="257ea-191">*Membership<br>(dbo.aspnet_UsersInRoles)*</span></span>||
|------------------------------------|------------------------------------------|
|<span data-ttu-id="257ea-192">**Имя поля**</span><span class="sxs-lookup"><span data-stu-id="257ea-192">**Field Name**</span></span>           |<span data-ttu-id="257ea-193">**Type**</span><span class="sxs-lookup"><span data-stu-id="257ea-193">**Type**</span></span>  |<span data-ttu-id="257ea-194">**Имя поля**</span><span class="sxs-lookup"><span data-stu-id="257ea-194">**Field Name**</span></span>|<span data-ttu-id="257ea-195">**Type**</span><span class="sxs-lookup"><span data-stu-id="257ea-195">**Type**</span></span>                   |
|`RoleId`                 |<span data-ttu-id="257ea-196">string</span><span class="sxs-lookup"><span data-stu-id="257ea-196">string</span></span>    |`RoleId`      |<span data-ttu-id="257ea-197">string</span><span class="sxs-lookup"><span data-stu-id="257ea-197">string</span></span>                     |
|`UserId`                 |<span data-ttu-id="257ea-198">string</span><span class="sxs-lookup"><span data-stu-id="257ea-198">string</span></span>    |`UserId`      |<span data-ttu-id="257ea-199">string</span><span class="sxs-lookup"><span data-stu-id="257ea-199">string</span></span>                     |

<span data-ttu-id="257ea-200">Ссылаться на предыдущем сопоставление таблиц, при создании сценария миграции для *пользователей* и *ролей*.</span><span class="sxs-lookup"><span data-stu-id="257ea-200">Reference the preceding mapping tables when creating a migration script for *Users* and *Roles*.</span></span> <span data-ttu-id="257ea-201">В следующем примере предполагается, что у вас есть две базы данных на сервере базы данных.</span><span class="sxs-lookup"><span data-stu-id="257ea-201">The following example assumes you have two databases on a database server.</span></span> <span data-ttu-id="257ea-202">Одна база данных содержит существующую схему членства ASP.NET и данные.</span><span class="sxs-lookup"><span data-stu-id="257ea-202">One database contains the existing ASP.NET Membership schema and data.</span></span> <span data-ttu-id="257ea-203">Другой *CoreIdentitySample* база данных была создана, выполнив действия, описанные ранее.</span><span class="sxs-lookup"><span data-stu-id="257ea-203">The other *CoreIdentitySample* database was created using steps described earlier.</span></span> <span data-ttu-id="257ea-204">Комментарии — это встроенные вместе для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="257ea-204">Comments are included inline for more details.</span></span>

```sql
-- THIS SCRIPT NEEDS TO RUN FROM THE CONTEXT OF THE MEMBERSHIP DB
BEGIN TRANSACTION MigrateUsersAndRoles
USE aspnetdb

-- INSERT USERS
INSERT INTO CoreIdentitySample.dbo.AspNetUsers
            (Id,
             UserName,
             NormalizedUserName,
             PasswordHash,
             SecurityStamp,
             EmailConfirmed,
             PhoneNumber,
             PhoneNumberConfirmed,
             TwoFactorEnabled,
             LockoutEnd,
             LockoutEnabled,
             AccessFailedCount,
             Email,
             NormalizedEmail)
SELECT aspnet_Users.UserId,
       aspnet_Users.UserName,
       -- The NormalizedUserName value is upper case in ASP.NET Core Identity
       UPPER(aspnet_Users.UserName),
       -- Creates an empty password since passwords don't map between the 2 schemas
       '',
       /*
        The SecurityStamp token is used to verify the state of an account and
        is subject to change at any time. It should be initialized as a new ID.
       */
       NewID(),
       /*
        EmailConfirmed is set when a new user is created and confirmed via email.
        Users must have this set during migration to reset passwords.
       */
       1,
       aspnet_Users.MobileAlias,
       CASE
         WHEN aspnet_Users.MobileAlias IS NULL THEN 0
         ELSE 1
       END,
       -- 2FA likely wasn't setup in Membership for users, so setting as false.
       0,
       CASE
         -- Setting lockout date to time in the future (1,000 years)
         WHEN aspnet_Membership.IsLockedOut = 1 THEN Dateadd(year, 1000,
                                                     Sysutcdatetime())
         ELSE NULL
       END,
       aspnet_Membership.IsLockedOut,
       /*
        AccessFailedAccount is used to track failed logins. This is stored in
        Membership in multiple columns. Setting to 0 arbitrarily.
       */
       0,
       aspnet_Membership.Email,
       -- The NormalizedEmail value is upper case in ASP.NET Core Identity
       UPPER(aspnet_Membership.Email)
FROM   aspnet_Users
       LEFT OUTER JOIN aspnet_Membership
                    ON aspnet_Membership.ApplicationId =
                       aspnet_Users.ApplicationId
                       AND aspnet_Users.UserId = aspnet_Membership.UserId
       LEFT OUTER JOIN CoreIdentitySample.dbo.AspNetUsers
                    ON aspnet_Membership.UserId = AspNetUsers.Id
WHERE  AspNetUsers.Id IS NULL

-- INSERT ROLES
INSERT INTO CoreIdentitySample.dbo.AspNetRoles(Id, Name)
SELECT RoleId, RoleName
FROM aspnet_Roles;

-- INSERT USER ROLES
INSERT INTO CoreIdentitySample.dbo.AspNetUserRoles(UserId, RoleId)
SELECT UserId, RoleId
FROM aspnet_UsersInRoles;

IF @@ERROR <> 0
  BEGIN
    ROLLBACK TRANSACTION MigrateUsersAndRoles
    RETURN
  END

COMMIT TRANSACTION MigrateUsersAndRoles
```

<span data-ttu-id="257ea-205">После завершения приведенный выше сценарий созданного ранее приложения ASP.NET Core Identity заполняется авторизованных пользователей.</span><span class="sxs-lookup"><span data-stu-id="257ea-205">After completion of the preceding script, the ASP.NET Core Identity app created earlier is populated with Membership users.</span></span> <span data-ttu-id="257ea-206">Пользователи должны изменить свои пароли, прежде чем войти в.</span><span class="sxs-lookup"><span data-stu-id="257ea-206">Users need to change their passwords before logging in.</span></span>

> [!NOTE]
> <span data-ttu-id="257ea-207">Если система членства пользователей с именами пользователей, не соответствует адресу электронной почты, изменения требуются для приложения, созданного ранее, чтобы решить эту проблему.</span><span class="sxs-lookup"><span data-stu-id="257ea-207">If the Membership system had users with user names that didn't match their email address, changes are required to the app created earlier to accommodate this.</span></span> <span data-ttu-id="257ea-208">Шаблон по умолчанию ожидает `UserName` и `Email` должны совпадать.</span><span class="sxs-lookup"><span data-stu-id="257ea-208">The default template expects `UserName` and `Email` to be the same.</span></span> <span data-ttu-id="257ea-209">Для ситуаций, в которых они отличаются, процесс входа в систему необходимо изменить, чтобы использовать `UserName` вместо `Email`.</span><span class="sxs-lookup"><span data-stu-id="257ea-209">For situations in which they're different, the login process needs to be modified to use `UserName` instead of `Email`.</span></span>

<span data-ttu-id="257ea-210">В `PageModel` страницы входа, расположенный *Pages\Account\Login.cshtml.cs*, удалите `[EmailAddress]` из атрибута *электронной почты* свойство.</span><span class="sxs-lookup"><span data-stu-id="257ea-210">In the `PageModel` of the Login Page, located at *Pages\Account\Login.cshtml.cs*, remove the `[EmailAddress]` attribute from the *Email* property.</span></span> <span data-ttu-id="257ea-211">Переименуйте его в *UserName*.</span><span class="sxs-lookup"><span data-stu-id="257ea-211">Rename it to *UserName*.</span></span> <span data-ttu-id="257ea-212">Это требует изменения везде, где `EmailAddress` упоминается, в *представление* и *PageModel*.</span><span class="sxs-lookup"><span data-stu-id="257ea-212">This requires a change wherever `EmailAddress` is mentioned, in the *View* and *PageModel*.</span></span> <span data-ttu-id="257ea-213">Результат выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="257ea-213">The result looks like the following:</span></span>

 ![Фиксированного имени входа](identity/_static/fixed-login.png)

## <a name="next-steps"></a><span data-ttu-id="257ea-215">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="257ea-215">Next steps</span></span>

<span data-ttu-id="257ea-216">В этом руководстве вы узнали, как перенос пользователей из членства SQL в удостоверение ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="257ea-216">In this tutorial, you learned how to port users from SQL membership to ASP.NET Core 2.0 Identity.</span></span> <span data-ttu-id="257ea-217">Дополнительные сведения о удостоверения ASP.NET Core см. в разделе [Общие сведения об Identity](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="257ea-217">For more information regarding ASP.NET Core Identity, see [Introduction to Identity](xref:security/authentication/identity).</span></span>
