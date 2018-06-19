---
title: Razor Pages с EF Core в ASP.NET Core — миграции — 4 из 8
author: rick-anderson
description: В этом учебнике вы начинаете использовать функцию миграций EF Core для управления изменениями модели данных в приложении ASP.NET Core MVC.
manager: wpickett
ms.author: riande
ms.date: 10/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/migrations
ms.openlocfilehash: 690beaabeab098cf9b764730b1bf1bd04bf6b003
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
ms.locfileid: "32740079"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a><span data-ttu-id="11e67-103">Razor Pages с EF Core в ASP.NET Core — миграции — 4 из 8</span><span class="sxs-lookup"><span data-stu-id="11e67-103">Razor Pages with EF Core in ASP.NET Core - Migrations - 4 of 8</span></span>

<span data-ttu-id="11e67-104">Авторы: [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra), [Йон П. Смит](https://twitter.com/thereformedprog) (Jon P Smith) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="11e67-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="11e67-105">В этом учебнике используется функция миграций EF Core для управления изменениями модели данных.</span><span class="sxs-lookup"><span data-stu-id="11e67-105">In this tutorial, the EF Core migrations feature for managing data model changes is used.</span></span>

<span data-ttu-id="11e67-106">При возникновении проблем, которые вам не удается устранить, скачайте [готовое приложение для этого этапа](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="11e67-106">If you run into problems you can't solve, download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="11e67-107">При разработке нового приложения модель данных часто изменяется.</span><span class="sxs-lookup"><span data-stu-id="11e67-107">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="11e67-108">При каждом изменении нарушается синхронизация модели с базой данных.</span><span class="sxs-lookup"><span data-stu-id="11e67-108">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="11e67-109">Вы начали работу с этим учебником, настроив платформу Entity Framework для создания базы данных, если она еще не существует.</span><span class="sxs-lookup"><span data-stu-id="11e67-109">This tutorial started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="11e67-110">Каждый раз при изменении модели данных:</span><span class="sxs-lookup"><span data-stu-id="11e67-110">Each time the data model changes:</span></span>

* <span data-ttu-id="11e67-111">База данных удаляется.</span><span class="sxs-lookup"><span data-stu-id="11e67-111">The DB is dropped.</span></span>
* <span data-ttu-id="11e67-112">Платформа EF создает новую базу данных, соответствующую модели.</span><span class="sxs-lookup"><span data-stu-id="11e67-112">EF creates a new one that matches the model.</span></span>
* <span data-ttu-id="11e67-113">Приложение заполняет базу тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="11e67-113">The app seeds the DB with test data.</span></span>

<span data-ttu-id="11e67-114">Этот подход к обеспечению синхронизации базы данных с моделью данных хорошо работает до развертывания приложения в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="11e67-114">This approach to keeping the DB in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="11e67-115">Приложение, выполняющееся в рабочей среде, обычно содержит данные.</span><span class="sxs-lookup"><span data-stu-id="11e67-115">When the app is running in production, it's usually storing data that needs to be maintained.</span></span> <span data-ttu-id="11e67-116">Приложение не может запускаться с тестовой базой данных каждый раз при внесении изменений (например, при добавлении столбца).</span><span class="sxs-lookup"><span data-stu-id="11e67-116">The app can't start with a test DB each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="11e67-117">Функция миграций EF Core решает эту проблему, позволяя платформе EF Core обновить схему базы данных вместо создания новой базы.</span><span class="sxs-lookup"><span data-stu-id="11e67-117">The EF Core Migrations feature solves this problem by enabling EF Core to update the DB schema instead of creating a new DB.</span></span>

<span data-ttu-id="11e67-118">Вместо удаления и повторного создания базы данных при изменении модели функция миграций обновляет схему, сохраняя существующие данные.</span><span class="sxs-lookup"><span data-stu-id="11e67-118">Rather than dropping and recreating the DB when the data model changes, migrations updates the schema and retains existing data.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="11e67-119">Пакеты Entity Framework Core NuGet для миграций</span><span class="sxs-lookup"><span data-stu-id="11e67-119">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="11e67-120">Для работы с миграциями можно использовать **консоль диспетчера пакетов** (PMC) или интерфейс командной строки (CLI).</span><span class="sxs-lookup"><span data-stu-id="11e67-120">To work with migrations, use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span> <span data-ttu-id="11e67-121">Эти руководства демонстрируют использование команд интерфейса командной строки.</span><span class="sxs-lookup"><span data-stu-id="11e67-121">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="11e67-122">Дополнительные сведения о PMC [приведены в конце этого руководства](#pmc).</span><span class="sxs-lookup"><span data-stu-id="11e67-122">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="11e67-123">Средства EF Core для интерфейса командной строки (CLI) доступны в [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="11e67-123">The EF Core tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="11e67-124">Чтобы установить этот пакет, добавьте его в коллекцию `DotNetCliToolReference` в файле *CSPROJ*, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="11e67-124">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="11e67-125">**Примечание.** Для установки этого пакета необходимо отредактировать файл *CSPROJ*.</span><span class="sxs-lookup"><span data-stu-id="11e67-125">**Note:** This package must be installed by editing the *.csproj* file.</span></span> <span data-ttu-id="11e67-126">Для установки этого пакета нельзя использовать команду `install-package` или графический пользовательский интерфейс диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="11e67-126">The`install-package` command or the package manager GUI cannot be used to install this package.</span></span> <span data-ttu-id="11e67-127">Измените файл *CSPROJ*, щелкнув правой кнопкой мыши имя проекта в **обозревателе решений** и выбрав пункт **Изменить ContosoUniversity.csproj**.</span><span class="sxs-lookup"><span data-stu-id="11e67-127">Edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

<span data-ttu-id="11e67-128">В следующем примере разметки показан обновленный файл *CSPROJ* с выделенными средствами интерфейса командной строки EF Core:</span><span class="sxs-lookup"><span data-stu-id="11e67-128">The following markup shows the updated *.csproj* file with the EF Core CLI tools highlighted:</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
<span data-ttu-id="11e67-129">На момент написания руководства номера версий в предыдущем примере были актуальными.</span><span class="sxs-lookup"><span data-stu-id="11e67-129">The version numbers in the preceding example were current when the tutorial was written.</span></span> <span data-ttu-id="11e67-130">Используйте версию средств интерфейса командной строки EF Core, которая находится в других пакетах.</span><span class="sxs-lookup"><span data-stu-id="11e67-130">Use the same version for the EF Core CLI tools found in the other packages.</span></span>

## <a name="change-the-connection-string"></a><span data-ttu-id="11e67-131">Изменение строки подключения</span><span class="sxs-lookup"><span data-stu-id="11e67-131">Change the connection string</span></span>

<span data-ttu-id="11e67-132">В файле *appsettings.json* измените имя базы данных в строке подключения на ContosoUniversity2.</span><span class="sxs-lookup"><span data-stu-id="11e67-132">In the *appsettings.json* file, change the name of the DB in the connection string to ContosoUniversity2.</span></span>

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="11e67-133">Изменение имени базы данных в строке подключения приводит к выполнению первой миграции для создания новой базы данных.</span><span class="sxs-lookup"><span data-stu-id="11e67-133">Changing the DB name in the connection string causes the first migration to create a new DB.</span></span> <span data-ttu-id="11e67-134">Новая база данных создается в связи с тем, что база с указанным именем не существует.</span><span class="sxs-lookup"><span data-stu-id="11e67-134">A new DB is created because one with that name doesn't exist.</span></span> <span data-ttu-id="11e67-135">Для начала работы с миграциями изменять строку подключения необязательно.</span><span class="sxs-lookup"><span data-stu-id="11e67-135">Changing the connection string isn't required for getting started with migrations.</span></span>

<span data-ttu-id="11e67-136">Вместо изменения имени базы данных можно удалить ее.</span><span class="sxs-lookup"><span data-stu-id="11e67-136">An alternative to changing the DB name is deleting the DB.</span></span> <span data-ttu-id="11e67-137">Воспользуйтесь **обозревателем объектов SQL Server** (SSOX) или командой интерфейса командной строки `database drop`:</span><span class="sxs-lookup"><span data-stu-id="11e67-137">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>

 ```console
 dotnet ef database drop
 ```

<span data-ttu-id="11e67-138">Следующий раздел описывает выполнение команд интерфейса командной строки.</span><span class="sxs-lookup"><span data-stu-id="11e67-138">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="11e67-139">Создание первоначальной миграции</span><span class="sxs-lookup"><span data-stu-id="11e67-139">Create an initial migration</span></span>

<span data-ttu-id="11e67-140">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="11e67-140">Build the project.</span></span>

<span data-ttu-id="11e67-141">Откройте командное окно и перейдите в папку проекта.</span><span class="sxs-lookup"><span data-stu-id="11e67-141">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="11e67-142">Папка проекта содержит файл *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="11e67-142">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="11e67-143">Введите в командном окне следующее:</span><span class="sxs-lookup"><span data-stu-id="11e67-143">Enter the following in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="11e67-144">В командном окне отображается информация следующего вида:</span><span class="sxs-lookup"><span data-stu-id="11e67-144">The command window displays information similar to the following:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="11e67-145">Если миграция завершается сбоем с сообщением "*Нет доступа к файлу... ContosoUniversity.dll, так как он используется другим процессом.*"</span><span class="sxs-lookup"><span data-stu-id="11e67-145">If the migration fails with the message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*"</span></span> <span data-ttu-id="11e67-146">на экране:</span><span class="sxs-lookup"><span data-stu-id="11e67-146">is displayed:</span></span>

* <span data-ttu-id="11e67-147">Остановите IIS Express.</span><span class="sxs-lookup"><span data-stu-id="11e67-147">Stop IIS Express.</span></span>

   * <span data-ttu-id="11e67-148">Выйдите из среды Visual Studio и перезапустите ее, либо</span><span class="sxs-lookup"><span data-stu-id="11e67-148">Exit and restart Visual Studio, or</span></span>
   * <span data-ttu-id="11e67-149">Найдите значок IIS Express в панели задач Windows.</span><span class="sxs-lookup"><span data-stu-id="11e67-149">Find the IIS Express icon in the Windows System Tray.</span></span>
   * <span data-ttu-id="11e67-150">Щелкните значок IIS Express правой кнопкой мыши и выберите **ContosoUniversity > Остановить сайт**.</span><span class="sxs-lookup"><span data-stu-id="11e67-150">Right-click the IIS Express icon, and then click **ContosoUniversity > Stop Site**.</span></span>

<span data-ttu-id="11e67-151">Если отображается сообщение об ошибке "Ошибка сборки",</span><span class="sxs-lookup"><span data-stu-id="11e67-151">If the error message "Build failed."</span></span> <span data-ttu-id="11e67-152">выполните команду еще раз.</span><span class="sxs-lookup"><span data-stu-id="11e67-152">is displayed, run the command again.</span></span> <span data-ttu-id="11e67-153">Если вы получаете такую ошибку, оставьте комментарий в конце этого учебника.</span><span class="sxs-lookup"><span data-stu-id="11e67-153">If you get this error, leave a note at the bottom of this tutorial.</span></span>

### <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="11e67-154">Обзор методов Up и Down</span><span class="sxs-lookup"><span data-stu-id="11e67-154">Examine the Up and Down methods</span></span>

<span data-ttu-id="11e67-155">Команда EF Core `migrations add` создала код для создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="11e67-155">The EF Core command `migrations add` generated code to create the DB from.</span></span> <span data-ttu-id="11e67-156">Код миграции находится в файле *Migrations\<метка_времени>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="11e67-156">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="11e67-157">Метод `Up` класса `InitialCreate` создает таблицы базы данных, соответствующие наборам сущностей модели данных.</span><span class="sxs-lookup"><span data-stu-id="11e67-157">The `Up` method of the `InitialCreate` class creates the DB tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="11e67-158">Метод `Down` удаляет их, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="11e67-158">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

<span data-ttu-id="11e67-159">Функция миграций вызывает метод `Up`, чтобы реализовать изменения модели данных для миграции.</span><span class="sxs-lookup"><span data-stu-id="11e67-159">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="11e67-160">При вводе команды для отката обновления функция миграций вызывает метод `Down`.</span><span class="sxs-lookup"><span data-stu-id="11e67-160">When you enter a command to roll back the update, migrations calls the `Down` method.</span></span>

<span data-ttu-id="11e67-161">Приведенный выше код предназначен для первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="11e67-161">The preceding code is for the initial migration.</span></span> <span data-ttu-id="11e67-162">Этот код был создан при выполнении команды `migrations add InitialCreate`.</span><span class="sxs-lookup"><span data-stu-id="11e67-162">That code was created when the `migrations add InitialCreate` command was run.</span></span> <span data-ttu-id="11e67-163">Параметр имени миграции (в примере это "InitialCreate") используется в качестве имени файла.</span><span class="sxs-lookup"><span data-stu-id="11e67-163">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="11e67-164">В качестве имени миграции можно использовать любое допустимое имя файла.</span><span class="sxs-lookup"><span data-stu-id="11e67-164">The migration name can be any valid file name.</span></span> <span data-ttu-id="11e67-165">Рекомендуется выбрать слово или фразу, которые кратко описывают назначение миграции.</span><span class="sxs-lookup"><span data-stu-id="11e67-165">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="11e67-166">Например, миграция, обеспечивающая добавление таблицы кафедр, может называться "AddDepartmentTable".</span><span class="sxs-lookup"><span data-stu-id="11e67-166">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

<span data-ttu-id="11e67-167">Если создается первоначальная миграция и база данных существует:</span><span class="sxs-lookup"><span data-stu-id="11e67-167">If the initial migration is created and the DB exists:</span></span>

* <span data-ttu-id="11e67-168">Формируется код создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="11e67-168">The DB creation code is generated.</span></span>
* <span data-ttu-id="11e67-169">Выполнять код создания базы данных не нужно, поскольку база уже соответствует модели данных.</span><span class="sxs-lookup"><span data-stu-id="11e67-169">The DB creation code doesn't need to run because the DB already matches the data model.</span></span> <span data-ttu-id="11e67-170">В случае выполнения код создания базы данных не вносит никаких изменений, поскольку база уже соответствует модели данных.</span><span class="sxs-lookup"><span data-stu-id="11e67-170">If the DB creation code is run, it doesn't make any changes because the DB already matches the data model.</span></span>

<span data-ttu-id="11e67-171">При развертывании приложения в новой среде этот код необходимо выполнить для создания новой базы данных.</span><span class="sxs-lookup"><span data-stu-id="11e67-171">When the app is deployed to a new environment, the DB creation code must be run to create the DB.</span></span>

<span data-ttu-id="11e67-172">Ранее была изменена строка подключения для использования нового имени базы данных.</span><span class="sxs-lookup"><span data-stu-id="11e67-172">Previously the connection string was changed to use a new name for the DB.</span></span> <span data-ttu-id="11e67-173">Поскольку указанная база данных не существует, миграция создает ее.</span><span class="sxs-lookup"><span data-stu-id="11e67-173">The specified DB doesn't exist, so migrations creates the DB.</span></span>

### <a name="the-data-model-snapshot"></a><span data-ttu-id="11e67-174">Моментальный снимок модели данных</span><span class="sxs-lookup"><span data-stu-id="11e67-174">The data model snapshot</span></span>

<span data-ttu-id="11e67-175">Функция миграций создает *моментальный снимок* текущей схемы базы данных в *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="11e67-175">Migrations creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="11e67-176">При добавлении миграции EF определяет, что именно изменилось, сравнивая модель данных с файлом моментального снимка.</span><span class="sxs-lookup"><span data-stu-id="11e67-176">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span>

<span data-ttu-id="11e67-177">При удалении миграции используйте команду [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span><span class="sxs-lookup"><span data-stu-id="11e67-177">When deleting a migration, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span> <span data-ttu-id="11e67-178">`dotnet ef migrations remove` удаляет миграцию и гарантирует корректный сброс моментального снимка.</span><span class="sxs-lookup"><span data-stu-id="11e67-178">`dotnet ef migrations remove` deletes the migration and ensures the snapshot is correctly reset.</span></span>

<span data-ttu-id="11e67-179">Дополнительные сведения об использовании файла моментального снимка см. в разделе [Миграции Core EF в средах групп](/ef/core/managing-schemas/migrations/teams).</span><span class="sxs-lookup"><span data-stu-id="11e67-179">See [EF Core Migrations in Team Environments](/ef/core/managing-schemas/migrations/teams) for more information about how the snapshot file is used.</span></span>

## <a name="remove-ensurecreated"></a><span data-ttu-id="11e67-180">Удаление EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="11e67-180">Remove EnsureCreated</span></span>

<span data-ttu-id="11e67-181">Для ранней разработки использовалась команда `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="11e67-181">For early development, the `EnsureCreated` command was used.</span></span> <span data-ttu-id="11e67-182">В этом учебнике используются миграции.</span><span class="sxs-lookup"><span data-stu-id="11e67-182">In this tutorial, migrations is used.</span></span> <span data-ttu-id="11e67-183">`EnsureCreated` имеет следующие ограничения:</span><span class="sxs-lookup"><span data-stu-id="11e67-183">`EnsureCreated` has the following limitations:</span></span>

* <span data-ttu-id="11e67-184">Пропускает миграции и создает базу данных и схему.</span><span class="sxs-lookup"><span data-stu-id="11e67-184">Bypasses migrations and creates the DB and schema.</span></span>
* <span data-ttu-id="11e67-185">Не создает таблицу миграций.</span><span class="sxs-lookup"><span data-stu-id="11e67-185">Doesn't create a migrations table.</span></span>
* <span data-ttu-id="11e67-186">*Не может* использоваться с миграциями.</span><span class="sxs-lookup"><span data-stu-id="11e67-186">Can *not* be used with migrations.</span></span>
* <span data-ttu-id="11e67-187">Разработана для тестирования или быстрого создания прототипов, когда часто требуется удаление и повторное создание базы данных.</span><span class="sxs-lookup"><span data-stu-id="11e67-187">Is designed for testing or rapid prototyping where the DB is dropped and re-created frequently.</span></span>

<span data-ttu-id="11e67-188">Удалите следующую строку из `DbInitializer`:</span><span class="sxs-lookup"><span data-stu-id="11e67-188">Remove the following line from `DbInitializer`:</span></span>

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a><span data-ttu-id="11e67-189">Применение миграции к базе данных при разработке</span><span class="sxs-lookup"><span data-stu-id="11e67-189">Apply the migration to the DB in development</span></span>

<span data-ttu-id="11e67-190">Введите следующую команду в командном окне, чтобы создать базу данных и таблицы.</span><span class="sxs-lookup"><span data-stu-id="11e67-190">In the command window, enter the following to create the DB and tables.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="11e67-191">Примечание. Если команда `update` возвращает ошибку "Ошибка сборки",</span><span class="sxs-lookup"><span data-stu-id="11e67-191">Note: If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="11e67-192">выполните команду еще раз.</span><span class="sxs-lookup"><span data-stu-id="11e67-192">Run the command again.</span></span>
* <span data-ttu-id="11e67-193">Если ошибка повторится, выйдите из среды Visual Studio и затем выполните команду `update`.</span><span class="sxs-lookup"><span data-stu-id="11e67-193">If it fails again, exit Visual Studio and then run the `update` command.</span></span>
* <span data-ttu-id="11e67-194">Оставьте сообщение внизу страницы.</span><span class="sxs-lookup"><span data-stu-id="11e67-194">Leave a message at the bottom of the page.</span></span>

<span data-ttu-id="11e67-195">Выходные данные этой команды аналогичны команде `migrations add`.</span><span class="sxs-lookup"><span data-stu-id="11e67-195">The output from the command is similar to the `migrations add` command output.</span></span> <span data-ttu-id="11e67-196">Для предыдущей команды отображаются журналы команд SQL, которые настроены для базы данных.</span><span class="sxs-lookup"><span data-stu-id="11e67-196">In the preceding command, logs for the SQL commands that set up the DB are displayed.</span></span> <span data-ttu-id="11e67-197">В приведенном ниже примере выходных данных большинство журналов опущено:</span><span class="sxs-lookup"><span data-stu-id="11e67-197">Most of the logs are omitted in the following sample output:</span></span>

```text
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (467ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (20ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20170816151242_InitialCreate', N'2.0.0-rtm-26452');
Done.
```

<span data-ttu-id="11e67-198">Чтобы снизить уровень детализации сообщений журнала, можно изменить уровень ведения журнала в файле *appsettings.Development.json*.</span><span class="sxs-lookup"><span data-stu-id="11e67-198">To reduce the level of detail in log messages, change the log levels in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="11e67-199">Дополнительные сведения см. в статье [Введение в ведение журналов](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="11e67-199">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

<span data-ttu-id="11e67-200">Используйте **обозреватель объектов SQL Server** для проверки базы данных.</span><span class="sxs-lookup"><span data-stu-id="11e67-200">Use **SQL Server Object Explorer** to inspect the DB.</span></span> <span data-ttu-id="11e67-201">Обратите внимание на добавление таблицы `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="11e67-201">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="11e67-202">Таблица `__EFMigrationsHistory` используется для отслеживания миграций, которые были применены к базе данных.</span><span class="sxs-lookup"><span data-stu-id="11e67-202">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the DB.</span></span> <span data-ttu-id="11e67-203">Просмотрев данные в таблице `__EFMigrationsHistory`, вы увидите одну строку для первой миграции.</span><span class="sxs-lookup"><span data-stu-id="11e67-203">View the data in the `__EFMigrationsHistory` table, it shows one row for the first migration.</span></span> <span data-ttu-id="11e67-204">Последний журнал в предыдущем примере выходных данных интерфейса командной строки показывает инструкцию INSERT, создающую эту строку.</span><span class="sxs-lookup"><span data-stu-id="11e67-204">The last log in the preceding CLI output example shows the INSERT statement that creates this row.</span></span>

<span data-ttu-id="11e67-205">Запустите приложение и убедитесь, что все функции работают.</span><span class="sxs-lookup"><span data-stu-id="11e67-205">Run the app and verify that everything works.</span></span>

## <a name="applying-migrations-in-production"></a><span data-ttu-id="11e67-206">Применение миграций в рабочей среде</span><span class="sxs-lookup"><span data-stu-id="11e67-206">Applying migrations in production</span></span>

<span data-ttu-id="11e67-207">Для рабочих приложений **не рекомендуется** вызывать [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="11e67-207">We recommend production apps should **not** call [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="11e67-208">`Migrate` не следует вызывать из приложения в ферме серверов.</span><span class="sxs-lookup"><span data-stu-id="11e67-208">`Migrate` shouldn't be called from an app in server farm.</span></span> <span data-ttu-id="11e67-209">Например, если приложение было развернуто в облаке с горизонтальным масштабированием (выполняется несколько экземпляров приложения).</span><span class="sxs-lookup"><span data-stu-id="11e67-209">For example, if the app has been cloud deployed with scale-out (multiple instances of the app are running).</span></span>

<span data-ttu-id="11e67-210">Миграция базы данных должна выполняться контролируемым способом в рамках развертывания.</span><span class="sxs-lookup"><span data-stu-id="11e67-210">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="11e67-211">Подход к миграции рабочей базы данных включает следующее:</span><span class="sxs-lookup"><span data-stu-id="11e67-211">Production database migration approaches include:</span></span>

* <span data-ttu-id="11e67-212">Использование миграций для создания сценариев SQL и их использования в развертывании.</span><span class="sxs-lookup"><span data-stu-id="11e67-212">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="11e67-213">Выполнение `dotnet ef database update` из контролируемой среды.</span><span class="sxs-lookup"><span data-stu-id="11e67-213">Running `dotnet ef database update` from a controlled environment.</span></span>

<span data-ttu-id="11e67-214">Платформа EF Core использует таблицу `__MigrationsHistory` для определения необходимости выполнить какие-либо миграции.</span><span class="sxs-lookup"><span data-stu-id="11e67-214">EF Core uses the `__MigrationsHistory` table to see if any migrations need to run.</span></span> <span data-ttu-id="11e67-215">Если база данных находится в актуальном состоянии, миграция не выполняется.</span><span class="sxs-lookup"><span data-stu-id="11e67-215">If the DB is up to date, no migration is run.</span></span>

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="11e67-216">Сравнение интерфейса командной строки (CLI) и консоли диспетчера пакетов (PMC)</span><span class="sxs-lookup"><span data-stu-id="11e67-216">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="11e67-217">Средства EF Core для управления миграциями предоставляются в следующих источниках:</span><span class="sxs-lookup"><span data-stu-id="11e67-217">The EF Core tooling for managing migrations is available from:</span></span>

* <span data-ttu-id="11e67-218">Команды интерфейса командной строки .NET Core.</span><span class="sxs-lookup"><span data-stu-id="11e67-218">.NET Core CLI commands.</span></span>
* <span data-ttu-id="11e67-219">Командлеты PowerShell в окне **консоли диспетчера пакетов** (PMC) Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="11e67-219">The PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span>

<span data-ttu-id="11e67-220">В этом учебнике демонстрируется использование интерфейса командной строки, однако некоторые разработчики предпочитают использовать консоль диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="11e67-220">This tutorial shows how to use the CLI, some developers prefer using the PMC.</span></span>

<span data-ttu-id="11e67-221">Команды EF Core для команд консоли диспетчера пакетов находятся в пакете [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools).</span><span class="sxs-lookup"><span data-stu-id="11e67-221">The EF Core commands for the PMC are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="11e67-222">Этот пакет уже входит в состав метапакета [Microsoft.AspNetCore.All](xref:fundamentals/metapackage), поэтому устанавливать его не требуется.</span><span class="sxs-lookup"><span data-stu-id="11e67-222">This package is included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="11e67-223">**Важно!** Это не тот же пакет, который вы устанавливаете для интерфейса командной строки, изменив файл *CSPROJ*.</span><span class="sxs-lookup"><span data-stu-id="11e67-223">**Important:** This isn't the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="11e67-224">Его имя заканчивается на `Tools`, а имя пакета интерфейса командной строки — на `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="11e67-224">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="11e67-225">Дополнительные сведения о командах интерфейса командной строки см. в разделе [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="11e67-225">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="11e67-226">Дополнительные сведения о командах консоли диспетчера пакетов см. в разделе [Консоль диспетчера пакетов (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="11e67-226">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="11e67-227">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="11e67-227">Troubleshooting</span></span>

<span data-ttu-id="11e67-228">Скачайте [готовое приложение для этого этапа](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="11e67-228">Download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="11e67-229">Приложение создает следующее исключение:</span><span class="sxs-lookup"><span data-stu-id="11e67-229">The app generates the following exception:</span></span>

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="11e67-230">Решение. Выполните `dotnet ef database update`</span><span class="sxs-lookup"><span data-stu-id="11e67-230">Solution: Run `dotnet ef database update`</span></span>

<span data-ttu-id="11e67-231">Если команда `update` возвращает ошибку "Ошибка сборки",</span><span class="sxs-lookup"><span data-stu-id="11e67-231">If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="11e67-232">выполните команду еще раз.</span><span class="sxs-lookup"><span data-stu-id="11e67-232">Run the command again.</span></span>
* <span data-ttu-id="11e67-233">Оставьте сообщение внизу страницы.</span><span class="sxs-lookup"><span data-stu-id="11e67-233">Leave a message at the bottom of the page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="11e67-234">[Назад](xref:data/ef-rp/sort-filter-page)
> [Вперед](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="11e67-234">[Previous](xref:data/ef-rp/sort-filter-page)
[Next](xref:data/ef-rp/complex-data-model)</span></span>
