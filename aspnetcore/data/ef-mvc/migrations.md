---
title: ASP.NET Core MVC с EF Core — миграции — 4 из 10
author: rick-anderson
description: В этом руководстве вы начинаете использовать функцию миграций EF Core для управления изменениями модели данных в приложении ASP.NET Core MVC.
ms.author: tdykstra
ms.date: 03/15/2018
uid: data/ef-mvc/migrations
ms.openlocfilehash: d8b92aeedb252b93e1dc1aca424d26a377305da2
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273589"
---
# <a name="aspnet-core-mvc-with-ef-core---migrations---4-of-10"></a><span data-ttu-id="f4298-103">ASP.NET Core MVC с EF Core — миграции — 4 из 10</span><span class="sxs-lookup"><span data-stu-id="f4298-103">ASP.NET Core MVC with EF Core - Migrations - 4 of 10</span></span>

<span data-ttu-id="f4298-104">Авторы: [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="f4298-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f4298-105">На примере учебного веб-приложения "Университет Contoso" демонстрируется процесс создания веб-приложений ASP.NET Core MVC с помощью Entity Framework Core и Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f4298-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="f4298-106">Сведения о серии руководств см. в [первом руководстве серии](intro.md).</span><span class="sxs-lookup"><span data-stu-id="f4298-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="f4298-107">В этом руководстве вы начинаете использовать функцию миграций EF Core для управления изменениями модели данных.</span><span class="sxs-lookup"><span data-stu-id="f4298-107">In this tutorial, you start using the EF Core migrations feature for managing data model changes.</span></span> <span data-ttu-id="f4298-108">В последующих руководствах вы добавите дополнительные миграции по мере изменения модели данных.</span><span class="sxs-lookup"><span data-stu-id="f4298-108">In later tutorials, you'll add more migrations as you change the data model.</span></span>

## <a name="introduction-to-migrations"></a><span data-ttu-id="f4298-109">Введение в миграции</span><span class="sxs-lookup"><span data-stu-id="f4298-109">Introduction to migrations</span></span>

<span data-ttu-id="f4298-110">При разработке нового приложения ваша модель данных часто меняется, и при каждом таком изменении она нарушает синхронизацию с базой данных.</span><span class="sxs-lookup"><span data-stu-id="f4298-110">When you develop a new application, your data model changes frequently, and each time the model changes, it gets out of sync with the database.</span></span> <span data-ttu-id="f4298-111">Вы начали работу с этими руководствами, настроив Entity Framework для создания базы данных, если она еще не существует.</span><span class="sxs-lookup"><span data-stu-id="f4298-111">You started these tutorials by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="f4298-112">Затем при каждом изменении модели данных — добавлении, удалении или изменении классов сущностей или изменении класса DbContext — вы можете удалить базу данных, после чего EF создает новую, которая соответствует данной модели, и заполняет ее тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="f4298-112">Then each time you change the data model -- add, remove, or change entity classes or change your DbContext class -- you can delete the database and EF creates a new one that matches the model, and seeds it with test data.</span></span>

<span data-ttu-id="f4298-113">Этот способ для обеспечения синхронизации базы данных с моделью данных хорошо работает до развертывания приложения в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="f4298-113">This method of keeping the database in sync with the data model works well until you deploy the application to production.</span></span> <span data-ttu-id="f4298-114">Когда приложение выполняется в рабочей среде, оно обычно хранит данные, которые вы хотите сохранить, и вам нежелательно терять все при каждом изменении, например при добавлении нового столбца.</span><span class="sxs-lookup"><span data-stu-id="f4298-114">When the application is running in production it's usually storing data that you want to keep, and you don't want to lose everything each time you make a change such as adding a new column.</span></span> <span data-ttu-id="f4298-115">Функция миграций EF Core решает эту проблему, позволяя EF обновить схему базы данных вместо создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="f4298-115">The EF Core Migrations feature solves this problem by enabling EF to update the database schema instead of creating  a new database.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="f4298-116">Пакеты Entity Framework Core NuGet для миграций</span><span class="sxs-lookup"><span data-stu-id="f4298-116">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="f4298-117">Для работы с миграциями можно использовать **консоль диспетчера пакетов** (PMC) или интерфейс командной строки (CLI).</span><span class="sxs-lookup"><span data-stu-id="f4298-117">To work with migrations, you can use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span>  <span data-ttu-id="f4298-118">Эти руководства демонстрируют использование команд интерфейса командной строки.</span><span class="sxs-lookup"><span data-stu-id="f4298-118">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="f4298-119">Дополнительные сведения о PMC [приведены в конце этого руководства](#pmc).</span><span class="sxs-lookup"><span data-stu-id="f4298-119">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="f4298-120">Средства EF для интерфейса командной строки (CLI) доступны в [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="f4298-120">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="f4298-121">Чтобы установить этот пакет, добавьте его в коллекцию `DotNetCliToolReference` в файле *CSPROJ*, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="f4298-121">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="f4298-122">**Примечание**. Необходимо установить этот пакет, отредактировав файл *.csproj*; использовать команду `install-package` или графический пользовательский интерфейс диспетчера пакетов нельзя.</span><span class="sxs-lookup"><span data-stu-id="f4298-122">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span> <span data-ttu-id="f4298-123">Вы можете изменить файл *CSPROJ*, щелкнув правой кнопкой мыши имя проекта в **обозревателе решений** и выбрав пункт **Изменить ContosoUniversity.csproj**.</span><span class="sxs-lookup"><span data-stu-id="f4298-123">You can edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
<span data-ttu-id="f4298-124">(На момент написания руководства номера версий в этом примере были актуальными.)</span><span class="sxs-lookup"><span data-stu-id="f4298-124">(The version numbers in this example were current when the tutorial was written.)</span></span> 

## <a name="change-the-connection-string"></a><span data-ttu-id="f4298-125">Изменение строки подключения</span><span class="sxs-lookup"><span data-stu-id="f4298-125">Change the connection string</span></span>

<span data-ttu-id="f4298-126">В файле *appsettings.json* измените имя базы данных в строке подключения на ContosoUniversity2 или другое имя, которое вы еще не использовали на компьютере, с которым работаете.</span><span class="sxs-lookup"><span data-stu-id="f4298-126">In the *appsettings.json* file, change the name of the database in the connection string to ContosoUniversity2 or some other name that you haven't used on the computer you're using.</span></span>

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="f4298-127">Это изменение настраивает проект, чтобы первая миграция создала базу данных.</span><span class="sxs-lookup"><span data-stu-id="f4298-127">This change sets up the project so that the first migration will create a new database.</span></span> <span data-ttu-id="f4298-128">Это необязательно для начала работы с миграциями, но позднее вы поймете, почему так стоит сделать.</span><span class="sxs-lookup"><span data-stu-id="f4298-128">This isn't required to get started with migrations, but you'll see later why it's a good idea.</span></span>

> [!NOTE]
> <span data-ttu-id="f4298-129">Вместо изменения имени базы данных можно удалить ее.</span><span class="sxs-lookup"><span data-stu-id="f4298-129">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="f4298-130">Воспользуйтесь **обозревателем объектов SQL Server** (SSOX) или командой интерфейса командной строки `database drop`:</span><span class="sxs-lookup"><span data-stu-id="f4298-130">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```
> <span data-ttu-id="f4298-131">Следующий раздел описывает выполнение команд интерфейса командной строки.</span><span class="sxs-lookup"><span data-stu-id="f4298-131">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="f4298-132">Создание первоначальной миграции</span><span class="sxs-lookup"><span data-stu-id="f4298-132">Create an initial migration</span></span>

<span data-ttu-id="f4298-133">Сохраните изменения и выполните сборку проекта.</span><span class="sxs-lookup"><span data-stu-id="f4298-133">Save your changes and build the project.</span></span> <span data-ttu-id="f4298-134">После этого откройте командное окно и в папку проекта.</span><span class="sxs-lookup"><span data-stu-id="f4298-134">Then open a command window and navigate to the project folder.</span></span> <span data-ttu-id="f4298-135">Вот как это можно сделать быстро:</span><span class="sxs-lookup"><span data-stu-id="f4298-135">Here's a quick way to do that:</span></span>

* <span data-ttu-id="f4298-136">Щелкните правой кнопкой мыши проект в **обозревателе решений** и выберите пункт **Открыть в проводнике**.</span><span class="sxs-lookup"><span data-stu-id="f4298-136">In **Solution Explorer**, right-click the project and choose **Open in File Explorer** from the context menu.</span></span>

  ![Элемент меню "Открыть в проводнике"](migrations/_static/open-in-file-explorer.png)

* <span data-ttu-id="f4298-138">Введите "cmd" в адресной строке и нажмите клавишу ВВОД.</span><span class="sxs-lookup"><span data-stu-id="f4298-138">Enter "cmd" in the address bar and press Enter.</span></span>

  ![Открытие командного окна](migrations/_static/open-command-window.png)

<span data-ttu-id="f4298-140">Введите в командном окне следующую команду:</span><span class="sxs-lookup"><span data-stu-id="f4298-140">Enter the following command in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="f4298-141">В командном окне отображаются следующие выходные данные:</span><span class="sxs-lookup"><span data-stu-id="f4298-141">You see output like the following in the command window:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> <span data-ttu-id="f4298-142">Если отображается сообщение об ошибке *No executable found matching command "dotnet-ef"* (Не найден исполняемый файл, соответствующий команде "dotnet-ef"), сведения об устранении проблемы см. в [этой записи блога](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/).</span><span class="sxs-lookup"><span data-stu-id="f4298-142">If you see an error message *No executable found matching command "dotnet-ef"*, see [this blog post](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) for help troubleshooting.</span></span>

<span data-ttu-id="f4298-143">Если отображается сообщение об ошибке "*Доступ к файлу... ContosoUniversity.dll невозможен, так как файл используется другим процессом*", найдите значок IIS Express в области уведомлений Windows, щелкните его правой кнопкой мыши, а затем выберите **ContosoUniversity > Остановить сайт**.</span><span class="sxs-lookup"><span data-stu-id="f4298-143">If you see an error message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*", find the IIS Express icon in the Windows System Tray, and right-click it, then click **ContosoUniversity > Stop Site**.</span></span>

## <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="f4298-144">Обзор методов Up и Down</span><span class="sxs-lookup"><span data-stu-id="f4298-144">Examine the Up and Down methods</span></span>

<span data-ttu-id="f4298-145">При выполнении команды `migrations add` система EF сформировала код, который создаст базу данных с нуля.</span><span class="sxs-lookup"><span data-stu-id="f4298-145">When you executed the `migrations add` command, EF generated the code that will create the database from scratch.</span></span> <span data-ttu-id="f4298-146">Этот код находится в файле *\<метка_времени>_InitialCreate.cs* внутри папки *Migrations*.</span><span class="sxs-lookup"><span data-stu-id="f4298-146">This code is in the *Migrations* folder, in the file named *\<timestamp>_InitialCreate.cs*.</span></span> <span data-ttu-id="f4298-147">Метод `Up` класса `InitialCreate` создает таблицы базы данных, которые соответствуют наборам сущностей модели данных, а метод `Down` удаляет их, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="f4298-147">The `Up` method of the `InitialCreate` class creates the database tables that correspond to the data model entity sets, and the `Down` method deletes them, as shown in the following example.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

<span data-ttu-id="f4298-148">Функция миграций вызывает метод `Up`, чтобы реализовать изменения модели данных для миграции.</span><span class="sxs-lookup"><span data-stu-id="f4298-148">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="f4298-149">При вводе команды для отката обновления функция миграций вызывает метод `Down`.</span><span class="sxs-lookup"><span data-stu-id="f4298-149">When you enter a command to roll back the update, Migrations calls the `Down` method.</span></span>

<span data-ttu-id="f4298-150">Этот пример кода предназначен для первоначальной миграции, созданной при вводе команды `migrations add InitialCreate`.</span><span class="sxs-lookup"><span data-stu-id="f4298-150">This code is for the initial migration that was created when you entered the `migrations add InitialCreate` command.</span></span> <span data-ttu-id="f4298-151">Параметр имени миграции (в примере это "InitialCreate") используется в качестве имени файла и может быть любым.</span><span class="sxs-lookup"><span data-stu-id="f4298-151">The migration name parameter ("InitialCreate" in the example) is used for the file name and can be whatever you want.</span></span> <span data-ttu-id="f4298-152">Рекомендуется выбрать слово или фразу, которые кратко описывают назначение миграции.</span><span class="sxs-lookup"><span data-stu-id="f4298-152">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="f4298-153">Например, последнюю миграцию можно назвать "AddDepartmentTable".</span><span class="sxs-lookup"><span data-stu-id="f4298-153">For example, you might name a later migration "AddDepartmentTable".</span></span>

<span data-ttu-id="f4298-154">Если вы создали первоначальную миграцию, когда база данных уже существовала, код для создания базы данных формируется, но выполнять его не требуется, так как база данных уже соответствует модели данных.</span><span class="sxs-lookup"><span data-stu-id="f4298-154">If you created the initial migration when the database already exists, the database creation code is generated but it doesn't have to run because the database already matches the data model.</span></span> <span data-ttu-id="f4298-155">Однако при развертывании приложения в другой среде, где база данных еще не существует, этот код будет выполняться для создания базы данных, поэтому рекомендуется сначала его протестировать.</span><span class="sxs-lookup"><span data-stu-id="f4298-155">When you deploy the app to another environment where the database doesn't exist yet, this code will run to create your database, so it's a good idea to test it first.</span></span> <span data-ttu-id="f4298-156">Вот почему ранее вы изменили имя базы данных в строке подключения — это позволяет функции миграций создать базу данных с нуля.</span><span class="sxs-lookup"><span data-stu-id="f4298-156">That's why you changed the name of the database in the connection string earlier -- so that migrations can create a new one from scratch.</span></span>

## <a name="the-data-model-snapshot"></a><span data-ttu-id="f4298-157">Моментальный снимок модели данных</span><span class="sxs-lookup"><span data-stu-id="f4298-157">The data model snapshot</span></span>

<span data-ttu-id="f4298-158">Функция миграций создает *моментальный снимок* текущей схемы базы данных в *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="f4298-158">Migrations creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="f4298-159">При добавлении миграции EF определяет, что именно изменилось, сравнивая модель данных с файлом моментального снимка.</span><span class="sxs-lookup"><span data-stu-id="f4298-159">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span>

<span data-ttu-id="f4298-160">При удалении миграции используйте команду [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span><span class="sxs-lookup"><span data-stu-id="f4298-160">When deleting a migration, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span> <span data-ttu-id="f4298-161">`dotnet ef migrations remove` удаляет миграцию и гарантирует корректный сброс моментального снимка.</span><span class="sxs-lookup"><span data-stu-id="f4298-161">`dotnet ef migrations remove` deletes the migration and ensures the snapshot is correctly reset.</span></span>

<span data-ttu-id="f4298-162">Дополнительные сведения об использовании файла моментального снимка см. в разделе [Миграции Core EF в средах групп](/ef/core/managing-schemas/migrations/teams).</span><span class="sxs-lookup"><span data-stu-id="f4298-162">See [EF Core Migrations in Team Environments](/ef/core/managing-schemas/migrations/teams) for more information about how the snapshot file is used.</span></span>

## <a name="apply-the-migration-to-the-database"></a><span data-ttu-id="f4298-163">Применение миграции к базе данных</span><span class="sxs-lookup"><span data-stu-id="f4298-163">Apply the migration to the database</span></span>

<span data-ttu-id="f4298-164">Введите следующую команду в командном окне, чтобы создать базу данных и таблицы в ней.</span><span class="sxs-lookup"><span data-stu-id="f4298-164">In the command window, enter the following command to create the database and tables in it.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="f4298-165">Выходные данные команды аналогичны команде `migrations add`, за исключением того, что вы видите журналы для команд SQL, настраивающих базу данных.</span><span class="sxs-lookup"><span data-stu-id="f4298-165">The output from the command is similar to the `migrations add` command, except that you see logs for the SQL commands that set up the database.</span></span> <span data-ttu-id="f4298-166">В приведенном ниже примере выходных данных большинство журналов опущено.</span><span class="sxs-lookup"><span data-stu-id="f4298-166">Most of the logs are omitted in the following sample output.</span></span> <span data-ttu-id="f4298-167">Если вам не нужен такой уровень детализации сообщений журнала, можно изменить уровень ведения журнала в файле *appsettings.Development.json*.</span><span class="sxs-lookup"><span data-stu-id="f4298-167">If you prefer not to see this level of detail in log messages, you can change the log level in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="f4298-168">Дополнительные сведения см. в статье [Введение в ведение журналов](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="f4298-168">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

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

<span data-ttu-id="f4298-169">Используйте **обозреватель объектов SQL Server** для проверки базы данных, как описано в первом руководстве.</span><span class="sxs-lookup"><span data-stu-id="f4298-169">Use **SQL Server Object Explorer** to inspect the database as you did in the first tutorial.</span></span>  <span data-ttu-id="f4298-170">Вы заметите добавление таблицы __EFMigrationsHistory, отслеживающей миграции, которые были применены к базе данных.</span><span class="sxs-lookup"><span data-stu-id="f4298-170">You'll notice the addition of an __EFMigrationsHistory table that keeps track of which migrations have been applied to the database.</span></span> <span data-ttu-id="f4298-171">Просмотрев данные в этой таблице, вы увидите одну строку для первой миграции.</span><span class="sxs-lookup"><span data-stu-id="f4298-171">View the data in that table and you'll see one row for the first migration.</span></span> <span data-ttu-id="f4298-172">(Последний журнал в предыдущем примере выходных данных интерфейса командной строки показывает оператор INSERT, создающий эту строку.)</span><span class="sxs-lookup"><span data-stu-id="f4298-172">(The last log in the preceding CLI output example shows the INSERT statement that creates this row.)</span></span>

<span data-ttu-id="f4298-173">Запустите приложение, чтобы убедиться, что все работает, как и раньше.</span><span class="sxs-lookup"><span data-stu-id="f4298-173">Run the application to verify that everything still works the same as before.</span></span>

![Страница указателя учащихся](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="f4298-175">Сравнение интерфейса командной строки (CLI) и консоли диспетчера пакетов (PMC)</span><span class="sxs-lookup"><span data-stu-id="f4298-175">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="f4298-176">Средства EF для управления миграциями доступны в виде команд интерфейса командной строки .NET Core или командлетов PowerShell в окне **консоли диспетчера пакетов** Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f4298-176">The EF tooling for managing migrations is available from .NET Core CLI commands or from PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="f4298-177">Это руководство описывает использование интерфейса командной строки, но вы можете использовать и консоль диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="f4298-177">This tutorial shows how to use the CLI, but you can use the PMC if you prefer.</span></span>

<span data-ttu-id="f4298-178">Команды EF для команд консоли диспетчера пакетов находятся в пакете [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools).</span><span class="sxs-lookup"><span data-stu-id="f4298-178">The EF commands for the PMC commands are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="f4298-179">Этот пакет уже входит в состав метапакета [Microsoft.AspNetCore.All](xref:fundamentals/metapackage), поэтому устанавливать его не требуется.</span><span class="sxs-lookup"><span data-stu-id="f4298-179">This package is already included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="f4298-180">**Важно!** Это не тот же пакет, который вы устанавливаете для интерфейса командной строки, изменив файл *CSPROJ*.</span><span class="sxs-lookup"><span data-stu-id="f4298-180">**Important:** This isn't the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="f4298-181">Его имя заканчивается на `Tools`, а имя пакета интерфейса командной строки — на `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="f4298-181">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="f4298-182">Дополнительные сведения о командах интерфейса командной строки см. в разделе [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="f4298-182">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span> 

<span data-ttu-id="f4298-183">Дополнительные сведения о командах консоли диспетчера пакетов см. в разделе [Консоль диспетчера пакетов (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="f4298-183">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="summary"></a><span data-ttu-id="f4298-184">Сводка</span><span class="sxs-lookup"><span data-stu-id="f4298-184">Summary</span></span>

<span data-ttu-id="f4298-185">В этом руководстве вы узнали, как создать и применить первую миграцию.</span><span class="sxs-lookup"><span data-stu-id="f4298-185">In this tutorial, you've seen how to create and apply your first migration.</span></span> <span data-ttu-id="f4298-186">В следующем руководстве вы начнете рассматривать более сложные вопросы, развернув модель данных.</span><span class="sxs-lookup"><span data-stu-id="f4298-186">In the next tutorial, you'll begin looking at more advanced topics by expanding the data model.</span></span> <span data-ttu-id="f4298-187">Попутно вы создадите и примените дополнительные миграции.</span><span class="sxs-lookup"><span data-stu-id="f4298-187">Along the way you'll create and apply additional migrations.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f4298-188">[Назад](sort-filter-page.md)
> [Вперед](complex-data-model.md)</span><span class="sxs-lookup"><span data-stu-id="f4298-188">[Previous](sort-filter-page.md)
[Next](complex-data-model.md)</span></span>  
