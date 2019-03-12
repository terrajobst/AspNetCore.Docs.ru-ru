---
title: Razor Pages с EF Core в ASP.NET Core — миграции — 4 из 8
author: rick-anderson
description: В этом учебнике вы начинаете использовать функцию миграций EF Core для управления изменениями модели данных в приложении ASP.NET Core MVC.
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/migrations
ms.openlocfilehash: ef79aca700068aadf9b5b2b689213b6e78a5d729
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346441"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a><span data-ttu-id="1641d-103">Razor Pages с EF Core в ASP.NET Core — миграции — 4 из 8</span><span class="sxs-lookup"><span data-stu-id="1641d-103">Razor Pages with EF Core in ASP.NET Core - Migrations - 4 of 8</span></span>

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="1641d-104">Авторы: [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra), [Йон П. Смит](https://twitter.com/thereformedprog) (Jon P Smith) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="1641d-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

<span data-ttu-id="1641d-105">В этом учебнике используется функция миграций EF Core для управления изменениями модели данных.</span><span class="sxs-lookup"><span data-stu-id="1641d-105">In this tutorial, the EF Core migrations feature for managing data model changes is used.</span></span>

<span data-ttu-id="1641d-106">При возникновении проблем, которые вам не удается устранить, скачайте [готовое приложение](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="1641d-106">If you run into problems you can't solve, download the [completed app](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span>

<span data-ttu-id="1641d-107">При разработке нового приложения модель данных часто изменяется.</span><span class="sxs-lookup"><span data-stu-id="1641d-107">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="1641d-108">При каждом изменении нарушается синхронизация модели с базой данных.</span><span class="sxs-lookup"><span data-stu-id="1641d-108">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="1641d-109">Вы начали работу с этим учебником, настроив платформу Entity Framework для создания базы данных, если она еще не существует.</span><span class="sxs-lookup"><span data-stu-id="1641d-109">This tutorial started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="1641d-110">Каждый раз при изменении модели данных:</span><span class="sxs-lookup"><span data-stu-id="1641d-110">Each time the data model changes:</span></span>

* <span data-ttu-id="1641d-111">База данных удаляется.</span><span class="sxs-lookup"><span data-stu-id="1641d-111">The DB is dropped.</span></span>
* <span data-ttu-id="1641d-112">Платформа EF создает новую базу данных, соответствующую модели.</span><span class="sxs-lookup"><span data-stu-id="1641d-112">EF creates a new one that matches the model.</span></span>
* <span data-ttu-id="1641d-113">Приложение заполняет базу тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="1641d-113">The app seeds the DB with test data.</span></span>

<span data-ttu-id="1641d-114">Этот подход к обеспечению синхронизации базы данных с моделью данных хорошо работает до развертывания приложения в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="1641d-114">This approach to keeping the DB in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="1641d-115">Приложение, выполняющееся в рабочей среде, обычно содержит данные.</span><span class="sxs-lookup"><span data-stu-id="1641d-115">When the app is running in production, it's usually storing data that needs to be maintained.</span></span> <span data-ttu-id="1641d-116">Приложение не может запускаться с тестовой базой данных каждый раз при внесении изменений (например, при добавлении столбца).</span><span class="sxs-lookup"><span data-stu-id="1641d-116">The app can't start with a test DB each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="1641d-117">Функция миграций EF Core решает эту проблему, позволяя платформе EF Core обновить схему базы данных вместо создания новой базы.</span><span class="sxs-lookup"><span data-stu-id="1641d-117">The EF Core Migrations feature solves this problem by enabling EF Core to update the DB schema instead of creating a new DB.</span></span>

<span data-ttu-id="1641d-118">Вместо удаления и повторного создания базы данных при изменении модели функция миграций обновляет схему, сохраняя существующие данные.</span><span class="sxs-lookup"><span data-stu-id="1641d-118">Rather than dropping and recreating the DB when the data model changes, migrations updates the schema and retains existing data.</span></span>

## <a name="drop-the-database"></a><span data-ttu-id="1641d-119">Удаление базы данных</span><span class="sxs-lookup"><span data-stu-id="1641d-119">Drop the database</span></span>

<span data-ttu-id="1641d-120">Воспользуйтесь **обозревателем объектов SQL Server** (SSOX) или командой `database drop`:</span><span class="sxs-lookup"><span data-stu-id="1641d-120">Use **SQL Server Object Explorer** (SSOX) or the `database drop` command:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1641d-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1641d-121">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1641d-122">Выполните следующие команды в **консоли диспетчера пакетов** (PMC):</span><span class="sxs-lookup"><span data-stu-id="1641d-122">In the **Package Manager Console** (PMC), run the following command:</span></span>

```PMC
Drop-Database
```

<span data-ttu-id="1641d-123">Чтобы просмотреть справочную информацию, выполните команду `Get-Help about_EntityFrameworkCore` в PMC.</span><span class="sxs-lookup"><span data-stu-id="1641d-123">Run `Get-Help about_EntityFrameworkCore` from the PMC to get help information.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1641d-124">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="1641d-124">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1641d-125">Откройте командное окно и перейдите в папку проекта.</span><span class="sxs-lookup"><span data-stu-id="1641d-125">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="1641d-126">Папка проекта содержит файл *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="1641d-126">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="1641d-127">Введите в командном окне следующее:</span><span class="sxs-lookup"><span data-stu-id="1641d-127">Enter the following in the command window:</span></span>

 ```console
 dotnet ef database drop
 ```

------

## <a name="create-an-initial-migration-and-update-the-db"></a><span data-ttu-id="1641d-128">Создание первоначальной миграции и обновление базы данных</span><span class="sxs-lookup"><span data-stu-id="1641d-128">Create an initial migration and update the DB</span></span>

<span data-ttu-id="1641d-129">Выполните построение проекта и создайте первую миграцию.</span><span class="sxs-lookup"><span data-stu-id="1641d-129">Build the project and create the first migration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1641d-130">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1641d-130">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1641d-131">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="1641d-131">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

------

### <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="1641d-132">Обзор методов Up и Down</span><span class="sxs-lookup"><span data-stu-id="1641d-132">Examine the Up and Down methods</span></span>

<span data-ttu-id="1641d-133">Команда EF Core `migrations add` создала код для создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="1641d-133">The EF Core `migrations add` command  generated code to create the DB.</span></span> <span data-ttu-id="1641d-134">Код миграции находится в файле *Migrations\<метка_времени>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="1641d-134">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="1641d-135">Метод `Up` класса `InitialCreate` создает таблицы базы данных, соответствующие наборам сущностей модели данных.</span><span class="sxs-lookup"><span data-stu-id="1641d-135">The `Up` method of the `InitialCreate` class creates the DB tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="1641d-136">Метод `Down` удаляет их, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="1641d-136">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu21/Migrations/20180626224812_InitialCreate.cs?range=7-24,77-88)]

<span data-ttu-id="1641d-137">Функция миграций вызывает метод `Up`, чтобы реализовать изменения модели данных для миграции.</span><span class="sxs-lookup"><span data-stu-id="1641d-137">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="1641d-138">При вводе команды для отката обновления функция миграций вызывает метод `Down`.</span><span class="sxs-lookup"><span data-stu-id="1641d-138">When you enter a command to roll back the update, migrations calls the `Down` method.</span></span>

<span data-ttu-id="1641d-139">Приведенный выше код предназначен для первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="1641d-139">The preceding code is for the initial migration.</span></span> <span data-ttu-id="1641d-140">Этот код был создан при выполнении команды `migrations add InitialCreate`.</span><span class="sxs-lookup"><span data-stu-id="1641d-140">That code was created when the `migrations add InitialCreate` command was run.</span></span> <span data-ttu-id="1641d-141">Параметр имени миграции (в примере это "InitialCreate") используется в качестве имени файла.</span><span class="sxs-lookup"><span data-stu-id="1641d-141">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="1641d-142">В качестве имени миграции можно использовать любое допустимое имя файла.</span><span class="sxs-lookup"><span data-stu-id="1641d-142">The migration name can be any valid file name.</span></span> <span data-ttu-id="1641d-143">Рекомендуется выбрать слово или фразу, которые кратко описывают назначение миграции.</span><span class="sxs-lookup"><span data-stu-id="1641d-143">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="1641d-144">Например, миграция, обеспечивающая добавление таблицы кафедр, может называться "AddDepartmentTable".</span><span class="sxs-lookup"><span data-stu-id="1641d-144">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

<span data-ttu-id="1641d-145">Если создается первоначальная миграция и база данных существует:</span><span class="sxs-lookup"><span data-stu-id="1641d-145">If the initial migration is created and the DB exists:</span></span>

* <span data-ttu-id="1641d-146">Формируется код создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="1641d-146">The DB creation code is generated.</span></span>
* <span data-ttu-id="1641d-147">Выполнять код создания базы данных не нужно, поскольку база уже соответствует модели данных.</span><span class="sxs-lookup"><span data-stu-id="1641d-147">The DB creation code doesn't need to run because the DB already matches the data model.</span></span> <span data-ttu-id="1641d-148">В случае выполнения код создания базы данных не вносит никаких изменений, поскольку база уже соответствует модели данных.</span><span class="sxs-lookup"><span data-stu-id="1641d-148">If the DB creation code is run, it doesn't make any changes because the DB already matches the data model.</span></span>

<span data-ttu-id="1641d-149">При развертывании приложения в новой среде этот код необходимо выполнить для создания новой базы данных.</span><span class="sxs-lookup"><span data-stu-id="1641d-149">When the app is deployed to a new environment, the DB creation code must be run to create the DB.</span></span>

<span data-ttu-id="1641d-150">Предыдущая база данных была удалена и не существует, поэтому функция миграций создает новую базу данных.</span><span class="sxs-lookup"><span data-stu-id="1641d-150">Previously the DB was dropped and doesn't exist, so migrations creates the new DB.</span></span>

### <a name="the-data-model-snapshot"></a><span data-ttu-id="1641d-151">Моментальный снимок модели данных</span><span class="sxs-lookup"><span data-stu-id="1641d-151">The data model snapshot</span></span>

<span data-ttu-id="1641d-152">Функция миграций создает *моментальный снимок* текущей схемы базы данных в *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="1641d-152">Migrations create a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="1641d-153">При добавлении миграции EF определяет, что именно изменилось, сравнивая модель данных с файлом моментального снимка.</span><span class="sxs-lookup"><span data-stu-id="1641d-153">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span>

<span data-ttu-id="1641d-154">Чтобы удалить миграцию, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="1641d-154">To delete a migration, use the following command:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1641d-155">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1641d-155">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1641d-156">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="1641d-156">Remove-Migration</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1641d-157">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="1641d-157">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet ef migrations remove
```

<span data-ttu-id="1641d-158">Дополнительные сведения см. в разделе [dotnet ef migrations remove](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span><span class="sxs-lookup"><span data-stu-id="1641d-158">For more information, see  [dotnet ef migrations remove](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span></span>

------

<span data-ttu-id="1641d-159">Команда remove migrations удаляет миграцию и гарантирует корректный сброс моментального снимка.</span><span class="sxs-lookup"><span data-stu-id="1641d-159">The remove migrations command deletes the migration and ensures the snapshot is correctly reset.</span></span>

### <a name="remove-ensurecreated-and-test-the-app"></a><span data-ttu-id="1641d-160">Удаление EnsureCreated и тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="1641d-160">Remove EnsureCreated and test the app</span></span>

<span data-ttu-id="1641d-161">Для ранней разработки использовалась команда `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="1641d-161">For early development, `EnsureCreated` was used.</span></span> <span data-ttu-id="1641d-162">В этом учебнике используются миграции.</span><span class="sxs-lookup"><span data-stu-id="1641d-162">In this tutorial, migrations are used.</span></span> <span data-ttu-id="1641d-163">`EnsureCreated` имеет следующие ограничения:</span><span class="sxs-lookup"><span data-stu-id="1641d-163">`EnsureCreated` has the following limitations:</span></span>

* <span data-ttu-id="1641d-164">Пропускает миграции и создает базу данных и схему.</span><span class="sxs-lookup"><span data-stu-id="1641d-164">Bypasses migrations and creates the DB and schema.</span></span>
* <span data-ttu-id="1641d-165">Не создает таблицу миграций.</span><span class="sxs-lookup"><span data-stu-id="1641d-165">Doesn't create a migrations table.</span></span>
* <span data-ttu-id="1641d-166">*Не может* использоваться с миграциями.</span><span class="sxs-lookup"><span data-stu-id="1641d-166">Can *not* be used with migrations.</span></span>
* <span data-ttu-id="1641d-167">Разработана для тестирования или быстрого создания прототипов, когда часто требуется удаление и повторное создание базы данных.</span><span class="sxs-lookup"><span data-stu-id="1641d-167">Is designed for testing or rapid prototyping where the DB is dropped and re-created frequently.</span></span>

<span data-ttu-id="1641d-168">Удалите следующую строку из `DbInitializer`:</span><span class="sxs-lookup"><span data-stu-id="1641d-168">Remove the following line from `DbInitializer`:</span></span>

```csharp
context.Database.EnsureCreated();
```

<span data-ttu-id="1641d-169">Запустите приложение и убедитесь, что база заполняется данными.</span><span class="sxs-lookup"><span data-stu-id="1641d-169">Run the app and verify the DB is seeded.</span></span>

### <a name="inspect-the-database"></a><span data-ttu-id="1641d-170">Проверка базы данных</span><span class="sxs-lookup"><span data-stu-id="1641d-170">Inspect the database</span></span>

<span data-ttu-id="1641d-171">Используйте **обозреватель объектов SQL Server** для проверки базы данных.</span><span class="sxs-lookup"><span data-stu-id="1641d-171">Use **SQL Server Object Explorer** to inspect the DB.</span></span> <span data-ttu-id="1641d-172">Обратите внимание на добавление таблицы `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="1641d-172">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="1641d-173">Таблица `__EFMigrationsHistory` используется для отслеживания миграций, которые были применены к базе данных.</span><span class="sxs-lookup"><span data-stu-id="1641d-173">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the DB.</span></span> <span data-ttu-id="1641d-174">Просмотрев данные в таблице `__EFMigrationsHistory`, вы увидите одну строку для первой миграции.</span><span class="sxs-lookup"><span data-stu-id="1641d-174">View the data in the `__EFMigrationsHistory` table, it shows one row for the first migration.</span></span> <span data-ttu-id="1641d-175">Последний журнал в предыдущем примере выходных данных интерфейса командной строки показывает инструкцию INSERT, создающую эту строку.</span><span class="sxs-lookup"><span data-stu-id="1641d-175">The last log in the preceding CLI output example shows the INSERT statement that creates this row.</span></span>

<span data-ttu-id="1641d-176">Запустите приложение и убедитесь, что все функции работают.</span><span class="sxs-lookup"><span data-stu-id="1641d-176">Run the app and verify that everything works.</span></span>

## <a name="applying-migrations-in-production"></a><span data-ttu-id="1641d-177">Применение миграций в рабочей среде</span><span class="sxs-lookup"><span data-stu-id="1641d-177">Applying migrations in production</span></span>

<span data-ttu-id="1641d-178">Для рабочих приложений **не рекомендуется** вызывать [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="1641d-178">We recommend production apps should **not** call [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="1641d-179">`Migrate` не следует вызывать из приложения в ферме серверов.</span><span class="sxs-lookup"><span data-stu-id="1641d-179">`Migrate` shouldn't be called from an app in server farm.</span></span> <span data-ttu-id="1641d-180">Например, если приложение было развернуто в облаке с горизонтальным масштабированием (выполняется несколько экземпляров приложения).</span><span class="sxs-lookup"><span data-stu-id="1641d-180">For example, if the app has been cloud deployed with scale-out (multiple instances of the app are running).</span></span>

<span data-ttu-id="1641d-181">Миграция базы данных должна выполняться контролируемым способом в рамках развертывания.</span><span class="sxs-lookup"><span data-stu-id="1641d-181">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="1641d-182">Подход к миграции рабочей базы данных включает следующее:</span><span class="sxs-lookup"><span data-stu-id="1641d-182">Production database migration approaches include:</span></span>

* <span data-ttu-id="1641d-183">Использование миграций для создания сценариев SQL и их использования в развертывании.</span><span class="sxs-lookup"><span data-stu-id="1641d-183">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="1641d-184">Выполнение `dotnet ef database update` из контролируемой среды.</span><span class="sxs-lookup"><span data-stu-id="1641d-184">Running `dotnet ef database update` from a controlled environment.</span></span>

<span data-ttu-id="1641d-185">Платформа EF Core использует таблицу `__MigrationsHistory` для определения необходимости выполнить какие-либо миграции.</span><span class="sxs-lookup"><span data-stu-id="1641d-185">EF Core uses the `__MigrationsHistory` table to see if any migrations need to run.</span></span> <span data-ttu-id="1641d-186">Если база данных находится в актуальном состоянии, миграция не выполняется.</span><span class="sxs-lookup"><span data-stu-id="1641d-186">If the DB is up-to-date, no migration is run.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="1641d-187">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="1641d-187">Troubleshooting</span></span>

<span data-ttu-id="1641d-188">Скачайте [готовое приложение](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="1641d-188">Download the [completed app](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="1641d-189">Приложение создает следующее исключение:</span><span class="sxs-lookup"><span data-stu-id="1641d-189">The app generates the following exception:</span></span>

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="1641d-190">Решение: Запуск `dotnet ef database update`</span><span class="sxs-lookup"><span data-stu-id="1641d-190">Solution: Run `dotnet ef database update`</span></span>

### <a name="additional-resources"></a><span data-ttu-id="1641d-191">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="1641d-191">Additional resources</span></span>

* [<span data-ttu-id="1641d-192">Версия руководства на YouTube</span><span class="sxs-lookup"><span data-stu-id="1641d-192">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=OWSUuMLKTJo)
* <span data-ttu-id="1641d-193">[Интерфейс командной строки .NET Core](/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="1641d-193">[.NET Core CLI](/ef/core/miscellaneous/cli/dotnet).</span></span>
* [<span data-ttu-id="1641d-194">Консоль диспетчера пакетов (Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="1641d-194">Package Manager Console (Visual Studio)</span></span>](/ef/core/miscellaneous/cli/powershell)

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="1641d-195">[Назад](xref:data/ef-rp/sort-filter-page)
> [Вперед](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="1641d-195">[Previous](xref:data/ef-rp/sort-filter-page)
[Next](xref:data/ef-rp/complex-data-model)</span></span>
