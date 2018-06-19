---
title: Перенести из проверки подлинности членства в ASP.NET 2.0 удостоверению ASP.NET Core
author: isaac2004
description: Дополнительные сведения о миграции существующих приложений ASP.NET, с помощью проверки подлинности членства ASP.NET Core 2.0 Identity.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: f0d1099bfda01d036831350e0888ae3830ad3d58
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/08/2018
ms.locfileid: "33851546"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a><span data-ttu-id="ac387-103">Перенести из проверки подлинности членства в ASP.NET 2.0 удостоверению ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ac387-103">Migrate from ASP.NET Membership authentication to ASP.NET Core 2.0 Identity</span></span>

<span data-ttu-id="ac387-104">Автор [Айзек Левин](https://isaaclevin.com) (Isaac Levin)</span><span class="sxs-lookup"><span data-stu-id="ac387-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="ac387-105">В этой статье демонстрируется миграция схемы базы данных для приложений ASP.NET, с помощью проверки подлинности членства ASP.NET Core 2.0 Identity.</span><span class="sxs-lookup"><span data-stu-id="ac387-105">This article demonstrates migrating the database schema for ASP.NET apps using Membership authentication to ASP.NET Core 2.0 Identity.</span></span>

> [!NOTE]
> <span data-ttu-id="ac387-106">Этот документ содержит шаги, необходимые для переноса схемы базы данных для приложения на основе членства в ASP.NET в схему базы данных, используемый для ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="ac387-106">This document provides the steps needed to migrate the database schema for ASP.NET Membership-based apps to the database schema used for ASP.NET Core Identity.</span></span> <span data-ttu-id="ac387-107">Дополнительные сведения о миграции с проверки подлинности на основе членства ASP.NET для ASP.NET Identity см. в разделе [провести миграцию существующего приложения с членства SQL в ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span><span class="sxs-lookup"><span data-stu-id="ac387-107">For more information about migrating from ASP.NET Membership-based authentication to ASP.NET Identity, see [Migrate an existing app from SQL Membership to ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span></span> <span data-ttu-id="ac387-108">Дополнительные сведения об ASP.NET Core Identity см. в разделе [введение в ASP.NET Core удостоверения](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="ac387-108">For more information about ASP.NET Core Identity, see [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity).</span></span>

## <a name="review-of-membership-schema"></a><span data-ttu-id="ac387-109">Проверка членства схемы</span><span class="sxs-lookup"><span data-stu-id="ac387-109">Review of Membership schema</span></span>

<span data-ttu-id="ac387-110">До ASP.NET 2.0 разработчики были заниматься созданием весь процесс проверки подлинности и авторизации для своих приложений.</span><span class="sxs-lookup"><span data-stu-id="ac387-110">Prior to ASP.NET 2.0, developers were tasked with creating the entire authentication and authorization process for their apps.</span></span> <span data-ttu-id="ac387-111">В ASP.NET 2.0 появилась членства, предоставления стандартного решения к обработке безопасности в приложениях ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ac387-111">With ASP.NET 2.0, Membership was introduced, providing a boilerplate solution to handling security within ASP.NET apps.</span></span> <span data-ttu-id="ac387-112">Теперь разработчики могли начальной загрузки схемы в базу данных SQL Server с [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) команды.</span><span class="sxs-lookup"><span data-stu-id="ac387-112">Developers were now able to bootstrap a schema into a SQL Server database with the [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) command.</span></span> <span data-ttu-id="ac387-113">После выполнения этой команды, следующие таблицы были созданы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="ac387-113">After running this command, the following tables were created in the database.</span></span>

  ![Таблицы членства](identity/_static/membership-tables.png)

<span data-ttu-id="ac387-115">Чтобы перенести существующие приложения ASP.NET Core 2.0 Identity, данные в этих таблицах необходимо перенести таблиц, используемых в новую схему удостоверений.</span><span class="sxs-lookup"><span data-stu-id="ac387-115">To migrate existing apps to ASP.NET Core 2.0 Identity, the data in these tables needs to be migrated to the tables used by the new Identity schema.</span></span>

## <a name="aspnet-core-identity-20-schema"></a><span data-ttu-id="ac387-116">Схема ASP.NET Core Identity 2.0</span><span class="sxs-lookup"><span data-stu-id="ac387-116">ASP.NET Core Identity 2.0 schema</span></span>

<span data-ttu-id="ac387-117">Соответствует ASP.NET Core 2.0 [удостоверение](/aspnet/identity/index) принцип, появившиеся в ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="ac387-117">ASP.NET Core 2.0 follows the [Identity](/aspnet/identity/index) principle introduced in ASP.NET 4.5.</span></span> <span data-ttu-id="ac387-118">Хотя общий принцип, реализация между платформами отличается, даже между версиями ASP.NET Core (см. [перенести проверку подлинности и удостоверение в ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span><span class="sxs-lookup"><span data-stu-id="ac387-118">Though the principle is shared, the implementation between the frameworks is different, even between versions of ASP.NET Core (see [Migrate authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span></span>

<span data-ttu-id="ac387-119">Самый быстрый способ просмотра схемы для ASP.NET Core 2.0 Identity является создание нового приложения ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="ac387-119">The fastest way to view the schema for ASP.NET Core 2.0 Identity is to create a new ASP.NET Core 2.0 app.</span></span> <span data-ttu-id="ac387-120">Выполните следующие действия в Visual Studio 2017 г.</span><span class="sxs-lookup"><span data-stu-id="ac387-120">Follow these steps in Visual Studio 2017:</span></span>

* <span data-ttu-id="ac387-121">Выберите **Файл** > **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="ac387-121">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="ac387-122">Создайте новый **веб-приложения ASP.NET Core**и назовите проект *CoreIdentitySample*.</span><span class="sxs-lookup"><span data-stu-id="ac387-122">Create a new **ASP.NET Core Web Application**, and name the project *CoreIdentitySample*.</span></span>
* <span data-ttu-id="ac387-123">Выберите в раскрывающемся списке **ASP.NET Core 2.0**, а затем **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="ac387-123">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span> <span data-ttu-id="ac387-124">Этот шаблон создает [страниц Razor](xref:mvc/razor-pages/index) приложения.</span><span class="sxs-lookup"><span data-stu-id="ac387-124">This template produces a [Razor Pages](xref:mvc/razor-pages/index) app.</span></span> <span data-ttu-id="ac387-125">Перед нажатием кнопки **ОК**, нажмите кнопку **изменить аутентификацию**.</span><span class="sxs-lookup"><span data-stu-id="ac387-125">Before clicking **OK**, click **Change Authentication**.</span></span>
* <span data-ttu-id="ac387-126">Выберите **отдельных учетных записей пользователей** для идентификаторов шаблонов.</span><span class="sxs-lookup"><span data-stu-id="ac387-126">Choose **Individual User Accounts** for the Identity templates.</span></span> <span data-ttu-id="ac387-127">Наконец, нажмите кнопку **ОК**, затем **ОК**.</span><span class="sxs-lookup"><span data-stu-id="ac387-127">Finally, click **OK**, then **OK**.</span></span> <span data-ttu-id="ac387-128">Visual Studio создает проект с помощью шаблона ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="ac387-128">Visual Studio creates a project using the ASP.NET Core Identity template.</span></span>

<span data-ttu-id="ac387-129">Использует удостоверения ASP.NET Core 2.0 [Entity Framework Core](/ef/core) для взаимодействия с базой данных, хранение данных проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="ac387-129">ASP.NET Core 2.0 Identity uses [Entity Framework Core](/ef/core) to interact with the database storing the authentication data.</span></span> <span data-ttu-id="ac387-130">Чтобы вновь созданного приложения для работы необходимо поддерживать базу данных для хранения этих данных.</span><span class="sxs-lookup"><span data-stu-id="ac387-130">In order for the newly created app to work, there needs to be a database to store this data.</span></span> <span data-ttu-id="ac387-131">После создания нового приложения, то самый быстрый способ проверить схему в среде базы данных является создание базы данных, с помощью миграций Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ac387-131">After creating a new app, the fastest way to inspect the schema in a database environment is to create the database using Entity Framework migrations.</span></span> <span data-ttu-id="ac387-132">Этот процесс создает базу данных, либо локально или в других местах, который имитирует этой схемы.</span><span class="sxs-lookup"><span data-stu-id="ac387-132">This process creates a database, either locally or elsewhere, which mimics that schema.</span></span> <span data-ttu-id="ac387-133">Предыдущий документации для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="ac387-133">Review the preceding documentation for more information.</span></span>

<span data-ttu-id="ac387-134">Чтобы создать базу данных со схемой ASP.NET Core Identity, запустите `Update-Database` в Visual Studio **консоль диспетчера пакетов** окна (PMC)&mdash;расположен по адресу **средства**  >  **Диспетчера пакетов NuGet** > **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="ac387-134">To create a database with the ASP.NET Core Identity schema, run the `Update-Database` command in Visual Studio's **Package Manager Console** (PMC) window&mdash;it's located at **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="ac387-135">PMC поддерживает выполнение команды Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ac387-135">PMC supports running Entity Framework commands.</span></span>

<span data-ttu-id="ac387-136">Команды Entity Framework использовать строку подключения для базы данных, указанной в *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="ac387-136">Entity Framework commands use the connection string for the database specified in *appsettings.json*.</span></span> <span data-ttu-id="ac387-137">Следующая строка подключения связанного с базой данных на *localhost* с именем *asp net-core идентификаторов*.</span><span class="sxs-lookup"><span data-stu-id="ac387-137">The following connection string targets a database on *localhost* named *asp-net-core-identity*.</span></span> <span data-ttu-id="ac387-138">В этом параметре Entity Framework настроен на использование `DefaultConnection` строку подключения.</span><span class="sxs-lookup"><span data-stu-id="ac387-138">In this setting, Entity Framework is configured to use the `DefaultConnection` connection string.</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

<span data-ttu-id="ac387-139">Эта команда создает базы данных, указанной с помощью схемы и все данные, необходимые для инициализации приложения.</span><span class="sxs-lookup"><span data-stu-id="ac387-139">This command builds the database specified with the schema and any data needed for app initialization.</span></span> <span data-ttu-id="ac387-140">На следующей иллюстрации изображена структура таблицы, создаваемой с описанные выше шаги.</span><span class="sxs-lookup"><span data-stu-id="ac387-140">The following image depicts the table structure that's created with the preceding steps.</span></span>

   ![Удостоверение таблиц](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a><span data-ttu-id="ac387-142">Перенос схемы</span><span class="sxs-lookup"><span data-stu-id="ac387-142">Migrate the schema</span></span>

<span data-ttu-id="ac387-143">Существуют небольшие отличия в структуры таблиц и полей для членства и ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="ac387-143">There are subtle differences in the table structures and fields for both Membership and ASP.NET Core Identity.</span></span> <span data-ttu-id="ac387-144">Шаблон отличается большей части для проверки подлинности и авторизации с приложениями ASP.NET и ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ac387-144">The pattern has changed substantially for authentication/authorization with ASP.NET and ASP.NET Core apps.</span></span> <span data-ttu-id="ac387-145">Объекты ключей, по-прежнему используются с удостоверением, *пользователей* и *ролей*.</span><span class="sxs-lookup"><span data-stu-id="ac387-145">The key objects that are still used with Identity are *Users* and *Roles*.</span></span> <span data-ttu-id="ac387-146">Ниже приведены таблицы сопоставления для *пользователей*, *ролей*, и *UserRoles*.</span><span class="sxs-lookup"><span data-stu-id="ac387-146">Here are mapping tables for *Users*, *Roles*, and *UserRoles*.</span></span>

### <a name="users"></a><span data-ttu-id="ac387-147">Пользователи</span><span class="sxs-lookup"><span data-stu-id="ac387-147">Users</span></span>

| <span data-ttu-id="ac387-148">*Identity(AspNetUsers)*</span><span class="sxs-lookup"><span data-stu-id="ac387-148">*Identity(AspNetUsers)*</span></span> |   | <span data-ttu-id="ac387-149">*Membership(aspnet_Users/aspnet_Membership)*</span><span class="sxs-lookup"><span data-stu-id="ac387-149">*Membership(aspnet_Users/aspnet_Membership)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="ac387-150">**Имя поля**</span><span class="sxs-lookup"><span data-stu-id="ac387-150">**Field Name**</span></span> | <span data-ttu-id="ac387-151">**Type**</span><span class="sxs-lookup"><span data-stu-id="ac387-151">**Type**</span></span>  |   <span data-ttu-id="ac387-152">**Имя поля**</span><span class="sxs-lookup"><span data-stu-id="ac387-152">**Field Name**</span></span> | <span data-ttu-id="ac387-153">**Type**</span><span class="sxs-lookup"><span data-stu-id="ac387-153">**Type**</span></span>  |
|`Id` | <span data-ttu-id="ac387-154">string</span><span class="sxs-lookup"><span data-stu-id="ac387-154">string</span></span> | `aspnet_Users.UserId` | <span data-ttu-id="ac387-155">string</span><span class="sxs-lookup"><span data-stu-id="ac387-155">string</span></span>
|`UserName` | <span data-ttu-id="ac387-156">string</span><span class="sxs-lookup"><span data-stu-id="ac387-156">string</span></span> | `aspnet_Users.UserName` | <span data-ttu-id="ac387-157">string</span><span class="sxs-lookup"><span data-stu-id="ac387-157">string</span></span>
|`Email` | <span data-ttu-id="ac387-158">string</span><span class="sxs-lookup"><span data-stu-id="ac387-158">string</span></span> | `aspnet_Membership.Email` | <span data-ttu-id="ac387-159">string</span><span class="sxs-lookup"><span data-stu-id="ac387-159">string</span></span>
|`NormalizedUserName` | <span data-ttu-id="ac387-160">string</span><span class="sxs-lookup"><span data-stu-id="ac387-160">string</span></span> | `aspnet_Users.LoweredUserName` | <span data-ttu-id="ac387-161">string</span><span class="sxs-lookup"><span data-stu-id="ac387-161">string</span></span>
|`NormalizedEmail` | <span data-ttu-id="ac387-162">string</span><span class="sxs-lookup"><span data-stu-id="ac387-162">string</span></span> | `aspnet_Membership.LoweredEmail` | <span data-ttu-id="ac387-163">string</span><span class="sxs-lookup"><span data-stu-id="ac387-163">string</span></span>
|`PhoneNumber` | <span data-ttu-id="ac387-164">string</span><span class="sxs-lookup"><span data-stu-id="ac387-164">string</span></span> | `aspnet_Users.MobileAlias` | <span data-ttu-id="ac387-165">string</span><span class="sxs-lookup"><span data-stu-id="ac387-165">string</span></span>
|`LockoutEnabled` | <span data-ttu-id="ac387-166">bit</span><span class="sxs-lookup"><span data-stu-id="ac387-166">bit</span></span> | `aspnet_Membership.IsLockedOut` | <span data-ttu-id="ac387-167">bit</span><span class="sxs-lookup"><span data-stu-id="ac387-167">bit</span></span>

> [!NOTE]
> <span data-ttu-id="ac387-168">Не все сопоставления полей напоминают отношения один к одному из членства в ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="ac387-168">Not all the field mappings resemble one-to-one relationships from Membership to ASP.NET Core Identity.</span></span> <span data-ttu-id="ac387-169">Приведенной выше таблице используется схема по умолчанию авторизованного пользователя и сопоставляет ее схемы ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="ac387-169">The preceding table takes the default Membership User schema and maps it to the ASP.NET Core Identity schema.</span></span> <span data-ttu-id="ac387-170">Другие настраиваемые поля, которые были использованы для членства должны быть добавлены вручную.</span><span class="sxs-lookup"><span data-stu-id="ac387-170">Any other custom fields that were used for Membership need to be mapped manually.</span></span> <span data-ttu-id="ac387-171">В этом сопоставлении файл сопоставления отсутствует пароли, как требования к паролям и соли пароль не проводить миграцию между двумя.</span><span class="sxs-lookup"><span data-stu-id="ac387-171">In this mapping, there's no map for passwords, as both password criteria and password salts don't migrate between the two.</span></span> <span data-ttu-id="ac387-172">**Рекомендуется оставить поле пароля как значения null и предлагать пользователям сбрасывать свои пароли.**</span><span class="sxs-lookup"><span data-stu-id="ac387-172">**It's recommended to leave the password as null and to ask users to reset their passwords.**</span></span> <span data-ttu-id="ac387-173">В ASP.NET Identity Core `LockoutEnd` должно задаваться на некоторую дату в будущем, если пользователь заблокирован. Это показано в сценарии миграции.</span><span class="sxs-lookup"><span data-stu-id="ac387-173">In ASP.NET Core Identity, `LockoutEnd` should be set to some date in the future if the user is locked out. This is shown in the migration script.</span></span>

### <a name="roles"></a><span data-ttu-id="ac387-174">Роли</span><span class="sxs-lookup"><span data-stu-id="ac387-174">Roles</span></span>

| <span data-ttu-id="ac387-175">*Identity(AspNetRoles)*</span><span class="sxs-lookup"><span data-stu-id="ac387-175">*Identity(AspNetRoles)*</span></span> |   | <span data-ttu-id="ac387-176">*Membership(aspnet_Roles)*</span><span class="sxs-lookup"><span data-stu-id="ac387-176">*Membership(aspnet_Roles)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="ac387-177">**Имя поля**</span><span class="sxs-lookup"><span data-stu-id="ac387-177">**Field Name**</span></span> | <span data-ttu-id="ac387-178">**Type**</span><span class="sxs-lookup"><span data-stu-id="ac387-178">**Type**</span></span>  |   <span data-ttu-id="ac387-179">**Имя поля**</span><span class="sxs-lookup"><span data-stu-id="ac387-179">**Field Name**</span></span> | <span data-ttu-id="ac387-180">**Type**</span><span class="sxs-lookup"><span data-stu-id="ac387-180">**Type**</span></span>  |
|`Id` | <span data-ttu-id="ac387-181">string</span><span class="sxs-lookup"><span data-stu-id="ac387-181">string</span></span> | `RoleId` | <span data-ttu-id="ac387-182">string</span><span class="sxs-lookup"><span data-stu-id="ac387-182">string</span></span>
|`Name` | <span data-ttu-id="ac387-183">string</span><span class="sxs-lookup"><span data-stu-id="ac387-183">string</span></span> | `RoleName` | <span data-ttu-id="ac387-184">string</span><span class="sxs-lookup"><span data-stu-id="ac387-184">string</span></span>
|`NormalizedName` | <span data-ttu-id="ac387-185">string</span><span class="sxs-lookup"><span data-stu-id="ac387-185">string</span></span> | `LoweredRoleName` | <span data-ttu-id="ac387-186">string</span><span class="sxs-lookup"><span data-stu-id="ac387-186">string</span></span>

### <a name="user-roles"></a><span data-ttu-id="ac387-187">Роли пользователей</span><span class="sxs-lookup"><span data-stu-id="ac387-187">User Roles</span></span>

| <span data-ttu-id="ac387-188">*Identity(AspNetUserRoles)*</span><span class="sxs-lookup"><span data-stu-id="ac387-188">*Identity(AspNetUserRoles)*</span></span> |   | <span data-ttu-id="ac387-189">*Membership(aspnet_UsersInRoles)*</span><span class="sxs-lookup"><span data-stu-id="ac387-189">*Membership(aspnet_UsersInRoles)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="ac387-190">**Имя поля**</span><span class="sxs-lookup"><span data-stu-id="ac387-190">**Field Name**</span></span> | <span data-ttu-id="ac387-191">**Type**</span><span class="sxs-lookup"><span data-stu-id="ac387-191">**Type**</span></span>  |   <span data-ttu-id="ac387-192">**Имя поля**</span><span class="sxs-lookup"><span data-stu-id="ac387-192">**Field Name**</span></span> | <span data-ttu-id="ac387-193">**Type**</span><span class="sxs-lookup"><span data-stu-id="ac387-193">**Type**</span></span>  |
|`RoleId` | <span data-ttu-id="ac387-194">string</span><span class="sxs-lookup"><span data-stu-id="ac387-194">string</span></span> | `RoleId` | <span data-ttu-id="ac387-195">string</span><span class="sxs-lookup"><span data-stu-id="ac387-195">string</span></span>
|`UserId` | <span data-ttu-id="ac387-196">string</span><span class="sxs-lookup"><span data-stu-id="ac387-196">string</span></span> | `UserId` | <span data-ttu-id="ac387-197">string</span><span class="sxs-lookup"><span data-stu-id="ac387-197">string</span></span>

<span data-ttu-id="ac387-198">Ссылаться на предыдущем таблицы сопоставления при создании сценария миграции для *пользователей* и *ролей*.</span><span class="sxs-lookup"><span data-stu-id="ac387-198">Reference the preceding mapping tables when creating a migration script for *Users* and *Roles*.</span></span> <span data-ttu-id="ac387-199">В следующем примере предполагается, что у вас есть две базы данных на сервере базы данных.</span><span class="sxs-lookup"><span data-stu-id="ac387-199">The following example assumes you have two databases on a database server.</span></span> <span data-ttu-id="ac387-200">Одна база данных содержит существующие членства ASP.NET схему и данные.</span><span class="sxs-lookup"><span data-stu-id="ac387-200">One database contains the existing ASP.NET Membership schema and data.</span></span> <span data-ttu-id="ac387-201">Другие базы данных был создан с помощью действия, описанные ранее.</span><span class="sxs-lookup"><span data-stu-id="ac387-201">The other database was created using steps described earlier.</span></span> <span data-ttu-id="ac387-202">Комментарии — это встроенные вместе для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="ac387-202">Comments are included inline for more details.</span></span>

```sql
-- THIS SCRIPT NEEDS TO RUN FROM THE CONTEXT OF THE MEMBERSHIP DB
BEGIN TRANSACTION MigrateUsersAndRoles
use aspnetdb

-- INSERT USERS
INSERT INTO coreidentity.dbo.aspnetusers
            (id,
             username,
             normalizedusername,
             passwordhash,
             securitystamp,
             emailconfirmed,
             phonenumber,
             phonenumberconfirmed,
             twofactorenabled,
             lockoutend,
             lockoutenabled,
             accessfailedcount,
             email,
             normalizedemail)
SELECT aspnet_users.userid,
       aspnet_users.username,
       aspnet_users.loweredusername,
       --Creates an empty password since passwords don't map between the two schemas
       '',
       --Security Stamp is a token used to verify the state of an account and is subject to change at any time. It should be intialized as a new ID.
       NewID(),
       --EmailConfirmed is set when a new user is created and confirmed via email. Users must have this set during migration to ensure they're able to reset passwords.
       1,
       aspnet_users.mobilealias,
       CASE
         WHEN aspnet_Users.MobileAlias is null THEN 0
         ELSE 1
       END,
       --2-factor Auth likely wasn't setup in Membership for users, so setting as false.
       0,
       CASE
         --Setting lockout date to time in the future (1000 years)
         WHEN aspnet_membership.islockedout = 1 THEN Dateadd(year, 1000,
                                                     Sysutcdatetime())
         ELSE NULL
       END,
       aspnet_membership.islockedout,
       --AccessFailedAccount is used to track failed logins. This is stored in membership in multiple columns. Setting to 0 arbitrarily.
       0,
       aspnet_membership.email,
       aspnet_membership.loweredemail
FROM   aspnet_users
       LEFT OUTER JOIN aspnet_membership
                    ON aspnet_membership.applicationid =
                       aspnet_users.applicationid
                       AND aspnet_users.userid = aspnet_membership.userid
       LEFT OUTER JOIN coreidentity.dbo.aspnetusers
                    ON aspnet_membership.userid = aspnetusers.id
WHERE  aspnetusers.id IS NULL

-- INSERT ROLES
INSERT INTO coreIdentity.dbo.aspnetroles(id,name)
SELECT roleId,rolename
FROM aspnet_roles;

-- INSERT USER ROLES
INSERT INTO coreidentity.dbo.aspnetuserroles(userid,roleid)
SELECT userid,roleid
FROM aspnet_usersinroles;

IF @@ERROR <> 0
  BEGIN
    ROLLBACK TRANSACTION MigrateUsersAndRoles
    RETURN
  END

COMMIT TRANSACTION MigrateUsersAndRoles
```

<span data-ttu-id="ac387-203">После выполнения этого скрипта ASP.NET Core Identity приложения, созданного ранее заполняется авторизованных пользователей.</span><span class="sxs-lookup"><span data-stu-id="ac387-203">After completion of this script, the ASP.NET Core Identity app created earlier is populated with Membership users.</span></span> <span data-ttu-id="ac387-204">Пользователи должны изменить свой пароль для входа.</span><span class="sxs-lookup"><span data-stu-id="ac387-204">Users need to change their passwords before logging in.</span></span>

> [!NOTE]
> <span data-ttu-id="ac387-205">Если система членства пользователей с именами пользователей, которые не соответствуют свой адрес электронной почты, требуются изменения в приложении, созданном ранее, чтобы решить эту проблему.</span><span class="sxs-lookup"><span data-stu-id="ac387-205">If the Membership system had users with user names that didn't match their email address, changes are required to the app created earlier to accommodate this.</span></span> <span data-ttu-id="ac387-206">Шаблон по умолчанию ожидает `UserName` и `Email` должны совпадать.</span><span class="sxs-lookup"><span data-stu-id="ac387-206">The default template expects `UserName` and `Email` to be the same.</span></span> <span data-ttu-id="ac387-207">В ситуациях, в которых они отличаются, процесс входа в систему необходимо изменить для использования `UserName` вместо `Email`.</span><span class="sxs-lookup"><span data-stu-id="ac387-207">For situations in which they're different, the login process needs to be modified to use `UserName` instead of `Email`.</span></span>

<span data-ttu-id="ac387-208">В `PageModel` страницы входа, расположенный в *Pages\Account\Login.cshtml.cs*, удалите `[EmailAddress]` атрибута из *электронной почты* свойство.</span><span class="sxs-lookup"><span data-stu-id="ac387-208">In the `PageModel` of the Login Page, located at *Pages\Account\Login.cshtml.cs*, remove the `[EmailAddress]` attribute from the *Email* property.</span></span> <span data-ttu-id="ac387-209">Переименуйте его в *UserName*.</span><span class="sxs-lookup"><span data-stu-id="ac387-209">Rename it to *UserName*.</span></span> <span data-ttu-id="ac387-210">Это требует изменения везде, где `EmailAddress` упоминается, в *представление* и *PageModel*.</span><span class="sxs-lookup"><span data-stu-id="ac387-210">This requires a change wherever `EmailAddress` is mentioned, in the *View* and *PageModel*.</span></span> <span data-ttu-id="ac387-211">Результат выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ac387-211">The result looks like the following:</span></span>

 ![Фиксированный входа](identity/_static/fixed-login.png)

## <a name="next-steps"></a><span data-ttu-id="ac387-213">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="ac387-213">Next steps</span></span>

<span data-ttu-id="ac387-214">В этом учебнике вы узнали, как перенести пользователей членства SQL с ASP.NET Core 2.0 Identity.</span><span class="sxs-lookup"><span data-stu-id="ac387-214">In this tutorial, you learned how to port users from SQL membership to ASP.NET Core 2.0 Identity.</span></span> <span data-ttu-id="ac387-215">Дополнительные сведения о ASP.NET Core Identity см. в разделе [Общие сведения об идентификации](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="ac387-215">For more information regarding ASP.NET Core Identity, see [Introduction to Identity](xref:security/authentication/identity).</span></span>
