---
title: "Страниц Razor с основными EF - Migrations - 4, 8."
author: rick-anderson
description: "В этом учебнике сначала с помощью EF Core миграции для управления изменений модели данных в приложении ASP.NET Core MVC."
keywords: "ASP.NET Core,Entity Framework Core,миграция"
ms.author: riande
manager: wpickett
ms.date: 10/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/migrations
ms.openlocfilehash: 8549fc522bcd05a5755a449cd6d4b655f00d998b
ms.sourcegitcommit: 6e46abd65973dea796d364a514de9ec2e3e1c1ed
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/02/2017
---
# <a name="migrations---ef-core-with-razor-pages-tutorial-4-of-8"></a><span data-ttu-id="21de2-104">Миграция - Core EF учебнику страниц Razor (4, 8)</span><span class="sxs-lookup"><span data-stu-id="21de2-104">Migrations - EF Core with Razor Pages tutorial (4 of 8)</span></span>

<span data-ttu-id="21de2-105">По [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="21de2-105">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="21de2-106">В этом учебнике функция миграции EF основных компонентов для управления изменений модели данных используется.</span><span class="sxs-lookup"><span data-stu-id="21de2-106">In this tutorial, the EF Core migrations feature for managing data model changes is used.</span></span>

<span data-ttu-id="21de2-107">Если возникли проблемы, не удается устранить, загрузите [завершенное приложение для данного этапа](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="21de2-107">If you run into problems you can't solve, download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="21de2-108">При разработке нового приложения, данные изменений в модели часто.</span><span class="sxs-lookup"><span data-stu-id="21de2-108">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="21de2-109">Каждый раз изменения модели, модель получает синхронизации с базой данных.</span><span class="sxs-lookup"><span data-stu-id="21de2-109">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="21de2-110">Этот учебник, запущенную настройка платформы Entity Framework для создания базы данных, если он не существует.</span><span class="sxs-lookup"><span data-stu-id="21de2-110">This tutorial started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="21de2-111">Каждый раз, изменений в модели данных:</span><span class="sxs-lookup"><span data-stu-id="21de2-111">Each time the data model changes:</span></span>

* <span data-ttu-id="21de2-112">Базы данных может быть удален.</span><span class="sxs-lookup"><span data-stu-id="21de2-112">The DB is dropped.</span></span>
* <span data-ttu-id="21de2-113">EF создает новый, который соответствует модели.</span><span class="sxs-lookup"><span data-stu-id="21de2-113">EF creates a new one that matches the model.</span></span>
* <span data-ttu-id="21de2-114">Приложение заполняет DB тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="21de2-114">The app seeds the DB with test data.</span></span>

<span data-ttu-id="21de2-115">Такой подход для синхронизации базы данных с моделью данных работает хорошо, пока развертывание приложения в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="21de2-115">This approach to keeping the DB in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="21de2-116">Когда приложение выполняется в производственной среде, он обычно хранит данные, которые необходимо сохранять.</span><span class="sxs-lookup"><span data-stu-id="21de2-116">When the app is running in production, it is usually storing data that needs to be maintained.</span></span> <span data-ttu-id="21de2-117">Приложение не может начинаться с тестирования базы данных при каждом изменении (например, Добавление нового столбца).</span><span class="sxs-lookup"><span data-stu-id="21de2-117">The app can't start with a test DB each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="21de2-118">Средство миграции Core EF решает эту проблему, позволяя EF основных компонентов для обновления схемы базы данных вместо создания новой базы данных.</span><span class="sxs-lookup"><span data-stu-id="21de2-118">The EF Core Migrations feature solves this problem by enabling EF Core to update the DB schema instead of creating a new DB.</span></span>

<span data-ttu-id="21de2-119">Вместо удаления и повторного создания базы данных, когда изменений в модели данных, миграция обновляет схему и сохраняет существующие данные.</span><span class="sxs-lookup"><span data-stu-id="21de2-119">Rather than dropping and recreating the DB when the data model changes, migrations updates the schema and retains existing data.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="21de2-120">Пакеты Entity Framework Core NuGet для миграций</span><span class="sxs-lookup"><span data-stu-id="21de2-120">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="21de2-121">Для работы с миграций, используйте **консоль диспетчера пакетов** (PMC) или интерфейса командной строки (CLI).</span><span class="sxs-lookup"><span data-stu-id="21de2-121">To work with migrations, use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span> <span data-ttu-id="21de2-122">Эти учебники демонстрируют использование команды CLI.</span><span class="sxs-lookup"><span data-stu-id="21de2-122">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="21de2-123">Сведения о PMC находится в [конце этого учебника](#pmc).</span><span class="sxs-lookup"><span data-stu-id="21de2-123">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="21de2-124">EF основные инструменты для командной строки (CLI), представлено в [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="21de2-124">The EF Core tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="21de2-125">Чтобы установить этот пакет, добавьте его в `DotNetCliToolReference` коллекции в *.csproj* файла, как показано.</span><span class="sxs-lookup"><span data-stu-id="21de2-125">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="21de2-126">**Примечание:** этот пакет должен быть установлен путем редактирования *.csproj* файла.</span><span class="sxs-lookup"><span data-stu-id="21de2-126">**Note:** This package must be installed by editing the *.csproj* file.</span></span> <span data-ttu-id="21de2-127">`install-package` Команды или диспетчер пакетов графического интерфейса пользователя не может использоваться для установки этого пакета.</span><span class="sxs-lookup"><span data-stu-id="21de2-127">The`install-package` command or the package manager GUI cannot be used to install this package.</span></span> <span data-ttu-id="21de2-128">Изменить *.csproj* файла, щелкнув правой кнопкой мыши имя проекта в **обозревателе решений** и выбрав **изменить ContosoUniversity.csproj**.</span><span class="sxs-lookup"><span data-stu-id="21de2-128">Edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

<span data-ttu-id="21de2-129">В следующем примере показана обновленный *.csproj* файла с помощью средств EF Core CLI выделяются:</span><span class="sxs-lookup"><span data-stu-id="21de2-129">The following markup shows the updated *.csproj* file with the EF Core CLI tools highlighted:</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
<span data-ttu-id="21de2-130">Номера версий в предыдущем примере были действительны, во время написания учебника.</span><span class="sxs-lookup"><span data-stu-id="21de2-130">The version numbers in the preceding example were current when the tutorial was written.</span></span> <span data-ttu-id="21de2-131">Для CLI EF основных средств, имеющихся в другие пакеты используют ту же версию.</span><span class="sxs-lookup"><span data-stu-id="21de2-131">Use the same version for the EF Core CLI tools found in the other packages.</span></span>

## <a name="change-the-connection-string"></a><span data-ttu-id="21de2-132">Измените строку подключения</span><span class="sxs-lookup"><span data-stu-id="21de2-132">Change the connection string</span></span>

<span data-ttu-id="21de2-133">В *appsettings.json* файл, измените имя базы данных в строке соединения на ContosoUniversity2.</span><span class="sxs-lookup"><span data-stu-id="21de2-133">In the *appsettings.json* file, change the name of the DB in the connection string to ContosoUniversity2.</span></span>

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="21de2-134">Изменение имени базы данных в строке подключения приводит к первой миграции для создания новой базы данных.</span><span class="sxs-lookup"><span data-stu-id="21de2-134">Changing the DB name in the connection string causes the first migration to create a new DB.</span></span> <span data-ttu-id="21de2-135">Новые базы данных создается, поскольку он с таким именем не существует.</span><span class="sxs-lookup"><span data-stu-id="21de2-135">A new DB is created because one with that name does not exist.</span></span> <span data-ttu-id="21de2-136">Изменение строки подключения не требуется для начала работы с миграции.</span><span class="sxs-lookup"><span data-stu-id="21de2-136">Changing the connection string isn't required for getting started with migrations.</span></span>

<span data-ttu-id="21de2-137">Вместо изменения имени базы данных удаление базы данных.</span><span class="sxs-lookup"><span data-stu-id="21de2-137">An alternative to changing the DB name is deleting the DB.</span></span> <span data-ttu-id="21de2-138">Используйте **обозреватель объектов SQL Server** (SSOX) или `database drop` команду CLI:</span><span class="sxs-lookup"><span data-stu-id="21de2-138">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>

 ```console
 dotnet ef database drop
 ```

<span data-ttu-id="21de2-139">Следующий раздел объясняет, как для выполнения команд CLI.</span><span class="sxs-lookup"><span data-stu-id="21de2-139">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="21de2-140">Создание первоначальной миграции</span><span class="sxs-lookup"><span data-stu-id="21de2-140">Create an initial migration</span></span>

<span data-ttu-id="21de2-141">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="21de2-141">Build the project.</span></span>

<span data-ttu-id="21de2-142">Откройте окно командной строки и перейдите в папку проекта.</span><span class="sxs-lookup"><span data-stu-id="21de2-142">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="21de2-143">Папка проекта содержит *файла Startup.cs* файл.</span><span class="sxs-lookup"><span data-stu-id="21de2-143">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="21de2-144">В командной строке введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="21de2-144">Enter the following in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="21de2-145">В командном окне отображается информация, аналогично приведенным ниже:</span><span class="sxs-lookup"><span data-stu-id="21de2-145">The command window displays information similar to the following:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="21de2-146">Если происходит сбой с сообщением «*не может получить доступ к файла... ContosoUniversity.dll, так как он используется другим процессом.* "</span><span class="sxs-lookup"><span data-stu-id="21de2-146">If the migration fails with the message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*"</span></span> <span data-ttu-id="21de2-147">отображается:</span><span class="sxs-lookup"><span data-stu-id="21de2-147">is displayed:</span></span>

* <span data-ttu-id="21de2-148">Остановить IIS Express.</span><span class="sxs-lookup"><span data-stu-id="21de2-148">Stop IIS Express.</span></span>

   * <span data-ttu-id="21de2-149">Закройте и перезапустите Visual Studio или</span><span class="sxs-lookup"><span data-stu-id="21de2-149">Exit and restart Visual Studio, or</span></span>
   * <span data-ttu-id="21de2-150">Значок IIS Express поиска на панели задач Windows.</span><span class="sxs-lookup"><span data-stu-id="21de2-150">Find the IIS Express icon in the Windows System Tray.</span></span>
   * <span data-ttu-id="21de2-151">Щелкните правой кнопкой мыши значок IIS Express, а затем нажмите кнопку **ContosoUniversity > Остановить узел**.</span><span class="sxs-lookup"><span data-stu-id="21de2-151">Right-click the IIS Express icon, and then click **ContosoUniversity > Stop Site**.</span></span>

<span data-ttu-id="21de2-152">Если сообщение об ошибке ««Сбой построения.</span><span class="sxs-lookup"><span data-stu-id="21de2-152">If the error message "Build failed."</span></span> <span data-ttu-id="21de2-153">отображается, выполните команду еще раз.</span><span class="sxs-lookup"><span data-stu-id="21de2-153">is displayed, run the command again.</span></span> <span data-ttu-id="21de2-154">Если эта ошибка появляется, оставьте комментарий в нижней части этого учебника.</span><span class="sxs-lookup"><span data-stu-id="21de2-154">If you get this error, leave a note at the bottom of this tutorial.</span></span>

### <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="21de2-155">Изучите вверх и вниз методы</span><span class="sxs-lookup"><span data-stu-id="21de2-155">Examine the Up and Down methods</span></span>

<span data-ttu-id="21de2-156">Команда EF Core `migrations add` созданного кода для создания из базы данных.</span><span class="sxs-lookup"><span data-stu-id="21de2-156">The EF Core command `migrations add` generated code to create the DB from.</span></span> <span data-ttu-id="21de2-157">Этот код миграции находится в *миграций\<timestamp > _InitialCreate.cs* файл.</span><span class="sxs-lookup"><span data-stu-id="21de2-157">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="21de2-158">`Up` Метод `InitialCreate` класс создает таблицы базы данных, которые соответствуют наборов сущностей модели данных.</span><span class="sxs-lookup"><span data-stu-id="21de2-158">The `Up` method of the `InitialCreate` class creates the DB tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="21de2-159">`Down` Метод удаляет их, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="21de2-159">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

<span data-ttu-id="21de2-160">Миграция вызовы `Up` метод для реализации изменений модели данных для миграции.</span><span class="sxs-lookup"><span data-stu-id="21de2-160">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="21de2-161">При вводе команды для отката обновления, вызывает миграции `Down` метод.</span><span class="sxs-lookup"><span data-stu-id="21de2-161">When you enter a command to roll back the update, migrations calls the `Down` method.</span></span>

<span data-ttu-id="21de2-162">Предыдущий код предназначен для первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="21de2-162">The preceding code is for the initial migration.</span></span> <span data-ttu-id="21de2-163">Этот код создан `migrations add InitialCreate` была выполнена команда.</span><span class="sxs-lookup"><span data-stu-id="21de2-163">That code was created when the `migrations add InitialCreate` command was run.</span></span> <span data-ttu-id="21de2-164">Для имени файла используется параметр name миграции («InitialCreate» в примере).</span><span class="sxs-lookup"><span data-stu-id="21de2-164">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="21de2-165">Имя миграции, может быть любое допустимое имя файла.</span><span class="sxs-lookup"><span data-stu-id="21de2-165">The migration name can be any valid file name.</span></span> <span data-ttu-id="21de2-166">Рекомендуется выбрать слово или фразу, в которой говорится, что выполняется в процессе переноса.</span><span class="sxs-lookup"><span data-stu-id="21de2-166">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="21de2-167">Например миграции, которая добавлена таблица department может иметь название «AddDepartmentTable».</span><span class="sxs-lookup"><span data-stu-id="21de2-167">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

<span data-ttu-id="21de2-168">Если создается первоначальной миграции и выходит из базы данных:</span><span class="sxs-lookup"><span data-stu-id="21de2-168">If the initial migration is created and the DB exits:</span></span>

* <span data-ttu-id="21de2-169">Создается код создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="21de2-169">The DB creation code is generated.</span></span>
* <span data-ttu-id="21de2-170">Создание кода БД не нужно выполнить, так как базы данных уже соответствует модели данных.</span><span class="sxs-lookup"><span data-stu-id="21de2-170">The DB creation code doesn't need to run because the DB already matches the data model.</span></span> <span data-ttu-id="21de2-171">При выполнении кода создания DB не вносите никаких изменений, так как базы данных, уже соответствует модели данных.</span><span class="sxs-lookup"><span data-stu-id="21de2-171">If the The DB creation code is run, it doesn't make any changes because the DB already matches the data model.</span></span>

<span data-ttu-id="21de2-172">При развертывании приложения в новую среду создания кода БД необходимо выполнить для создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="21de2-172">When the app is deployed to a new environment, the DB creation code must be run to create the DB.</span></span>

<span data-ttu-id="21de2-173">Ранее строки подключения была изменена для использования нового имени для базы данных.</span><span class="sxs-lookup"><span data-stu-id="21de2-173">Previously the connection string was changed to use a new name for the DB.</span></span> <span data-ttu-id="21de2-174">Указанной базы данных не существует, поэтому миграция создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="21de2-174">The specified DB doesn't exist, so migrations creates the DB.</span></span>

### <a name="examine-the-data-model-snapshot"></a><span data-ttu-id="21de2-175">Изучите снимок модели данных</span><span class="sxs-lookup"><span data-stu-id="21de2-175">Examine the data model snapshot</span></span>

<span data-ttu-id="21de2-176">Миграция создает *моментального снимка* текущей схемы базы данных в *Migrations/SchoolContextModelSnapshot.cs*:</span><span class="sxs-lookup"><span data-stu-id="21de2-176">Migrations creates a *snapshot* of the current DB schema in *Migrations/SchoolContextModelSnapshot.cs*:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

<span data-ttu-id="21de2-177">Поскольку текущая схема базы данных представлен в коде, EF Core не должны взаимодействовать с базы данных для создания миграции.</span><span class="sxs-lookup"><span data-stu-id="21de2-177">Because the current DB schema is represented in code, EF Core doesn't have to interact with the DB to create migrations.</span></span> <span data-ttu-id="21de2-178">При добавлении миграции EF Core определяет, что изменилось, сравнивая модели данных в файле моментального снимка.</span><span class="sxs-lookup"><span data-stu-id="21de2-178">When you add a migration, EF Core determines what changed by comparing the data model to the snapshot file.</span></span> <span data-ttu-id="21de2-179">EF Core взаимодействует с базы данных, только когда требуется обновление базы данных.</span><span class="sxs-lookup"><span data-stu-id="21de2-179">EF Core interacts with the DB only when it has to update the DB.</span></span>

<span data-ttu-id="21de2-180">Файл моментальных снимков должен быть синхронизирован с миграций, которые он создан.</span><span class="sxs-lookup"><span data-stu-id="21de2-180">The snapshot file must be in sync with the migrations that created it.</span></span> <span data-ttu-id="21de2-181">Нельзя удалить миграции, удалив файл  *\<timestamp > _\<migrationname > .cs*.</span><span class="sxs-lookup"><span data-stu-id="21de2-181">A migration can't be removed by deleting the file named *\<timestamp>_\<migrationname>.cs*.</span></span> <span data-ttu-id="21de2-182">Если этот файл удаляется, оставшиеся операции миграции не синхронизированы с файлом моментального снимка базы данных.</span><span class="sxs-lookup"><span data-stu-id="21de2-182">If that file is deleted, the remaining migrations are out of sync with the DB snapshot file.</span></span> <span data-ttu-id="21de2-183">Чтобы удалить последний переноса добавлен, используйте [dotnet ef миграции удалить](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) команды.</span><span class="sxs-lookup"><span data-stu-id="21de2-183">To delete the last migration added, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span>

## <a name="remove-ensurecreated"></a><span data-ttu-id="21de2-184">Удалить EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="21de2-184">Remove EnsureCreated</span></span>

<span data-ttu-id="21de2-185">Для раннего разработки `EnsureCreated` была использована.</span><span class="sxs-lookup"><span data-stu-id="21de2-185">For early development, the `EnsureCreated` command was used.</span></span> <span data-ttu-id="21de2-186">В этом учебнике используется миграции.</span><span class="sxs-lookup"><span data-stu-id="21de2-186">In this tutorial, migrations is used.</span></span> <span data-ttu-id="21de2-187">`EnsureCreated`имеет следующие limatitions:</span><span class="sxs-lookup"><span data-stu-id="21de2-187">`EnsureCreated` has the following limatitions:</span></span>

* <span data-ttu-id="21de2-188">Пропускает миграции и создает базы данных и схемы.</span><span class="sxs-lookup"><span data-stu-id="21de2-188">Bypasses migrations and creates the DB and schema.</span></span>
* <span data-ttu-id="21de2-189">Не создает таблицы миграций.</span><span class="sxs-lookup"><span data-stu-id="21de2-189">Does not create a migrations table.</span></span>
* <span data-ttu-id="21de2-190">Можно *не* использоваться с миграции.</span><span class="sxs-lookup"><span data-stu-id="21de2-190">Can *not* be used with migrations.</span></span>
* <span data-ttu-id="21de2-191">Предназначен для тестирования или быстрого создания прототипов. она где базы данных будет удалена и создана заново часто.</span><span class="sxs-lookup"><span data-stu-id="21de2-191">Is designed for testing or rapid prototyping where the DB is dropped and re-created frequently.</span></span>

<span data-ttu-id="21de2-192">Удалите следующую строку из `DbInitializer`:</span><span class="sxs-lookup"><span data-stu-id="21de2-192">Remove the following line from `DbInitializer`:</span></span>

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a><span data-ttu-id="21de2-193">Применить к базе данных. в разработке миграции</span><span class="sxs-lookup"><span data-stu-id="21de2-193">Apply the migration to the DB in development</span></span>

<span data-ttu-id="21de2-194">В окне командной строки введите следующую команду, чтобы создать базы данных и таблицы.</span><span class="sxs-lookup"><span data-stu-id="21de2-194">In the command window, enter the following to create the DB and tables.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="21de2-195">Примечание: Если `update` команда возвращает ошибку «Ошибка построения.»:</span><span class="sxs-lookup"><span data-stu-id="21de2-195">Note: If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="21de2-196">Выполните команду еще раз.</span><span class="sxs-lookup"><span data-stu-id="21de2-196">Run the command again.</span></span>
* <span data-ttu-id="21de2-197">Если ошибка повториться, выйдите из Visual Studio, а затем запустите `update` команды.</span><span class="sxs-lookup"><span data-stu-id="21de2-197">If it fails again, exit Visual Studio and then run the `update` command.</span></span>
* <span data-ttu-id="21de2-198">Оставьте сообщение в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="21de2-198">Leave a message at the bottom of the page.</span></span>

<span data-ttu-id="21de2-199">Выходные данные команды аналогичен `migrations add` выходные данные команды.</span><span class="sxs-lookup"><span data-stu-id="21de2-199">The output from the command is similar to the `migrations add` command output.</span></span> <span data-ttu-id="21de2-200">В предыдущей команде отображаются журналы для команды SQL, которые настроены в базе данных.</span><span class="sxs-lookup"><span data-stu-id="21de2-200">In the preceding command, logs for the SQL commands that set up the DB are displayed.</span></span> <span data-ttu-id="21de2-201">Большинство журналов опущены в следующий результат:</span><span class="sxs-lookup"><span data-stu-id="21de2-201">Most of the logs are omitted in the following sample output:</span></span>

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

<span data-ttu-id="21de2-202">Чтобы снизить уровень детализации сообщений журнала, можно изменить уровни журнала в *appsettings. Development.JSON* файл.</span><span class="sxs-lookup"><span data-stu-id="21de2-202">To reduce the level of detail in log messages, can change the log levels in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="21de2-203">Дополнительные сведения см. в разделе [введение к ведению журнала](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="21de2-203">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

<span data-ttu-id="21de2-204">Используйте **обозреватель объектов SQL Server** для проверки базы данных.</span><span class="sxs-lookup"><span data-stu-id="21de2-204">Use **SQL Server Object Explorer** to inspect the DB.</span></span> <span data-ttu-id="21de2-205">Обратите внимание на добавленную `__EFMigrationsHistory` таблицы.</span><span class="sxs-lookup"><span data-stu-id="21de2-205">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="21de2-206">`__EFMigrationsHistory` Таблица отслеживает отслеживания объекта миграций, которые были применены к базе данных.</span><span class="sxs-lookup"><span data-stu-id="21de2-206">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the DB.</span></span> <span data-ttu-id="21de2-207">Просмотр данных в `__EFMigrationsHistory` таблицу, она показана одна строка для первой миграции.</span><span class="sxs-lookup"><span data-stu-id="21de2-207">View the data in the `__EFMigrationsHistory` table, it shows one row for the first migration.</span></span> <span data-ttu-id="21de2-208">Последний журнал в предыдущем примере вывода CLI показывает инструкции INSERT, которая создает эту строку.</span><span class="sxs-lookup"><span data-stu-id="21de2-208">The last log in the preceding CLI output example shows the INSERT statement that creates this row.</span></span>

<span data-ttu-id="21de2-209">Запустите приложение и убедитесь, что все работает.</span><span class="sxs-lookup"><span data-stu-id="21de2-209">Run the app and verify that everything works.</span></span>

## <a name="appling-migrations-in-production"></a><span data-ttu-id="21de2-210">Применение миграции в рабочей среде</span><span class="sxs-lookup"><span data-stu-id="21de2-210">Appling migrations in production</span></span>

<span data-ttu-id="21de2-211">Мы рекомендуем производственных приложений следует **не** вызвать [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="21de2-211">We recommend production apps should **not** call [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="21de2-212">`Migrate`не должен вызываться из приложения в ферме серверов.</span><span class="sxs-lookup"><span data-stu-id="21de2-212">`Migrate` should not be called from an app in server farm.</span></span> <span data-ttu-id="21de2-213">Например, если приложение было облака, развернутых с помощью масштабирования (запущено несколько экземпляров приложения).</span><span class="sxs-lookup"><span data-stu-id="21de2-213">For example, if the app has been cloud deployed with scale-out (multiple instances of the app are running).</span></span>

<span data-ttu-id="21de2-214">Миграция базы данных должно выполняться как часть развертывания и контролируемым образом.</span><span class="sxs-lookup"><span data-stu-id="21de2-214">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="21de2-215">Следующие подходы к переносу рабочей базы данных.</span><span class="sxs-lookup"><span data-stu-id="21de2-215">Production database migration approaches include:</span></span>

* <span data-ttu-id="21de2-216">Создание сценариев SQL и использование скриптов SQL в среде с помощью миграций.</span><span class="sxs-lookup"><span data-stu-id="21de2-216">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="21de2-217">Под управлением `dotnet ef database update` из управляемой среды.</span><span class="sxs-lookup"><span data-stu-id="21de2-217">Running `dotnet ef database update` from a controlled environment.</span></span>

<span data-ttu-id="21de2-218">EF Core использует `__MigrationsHistory` таблицу для просмотра, если любой миграции необходимо выполнить.</span><span class="sxs-lookup"><span data-stu-id="21de2-218">EF Core uses the `__MigrationsHistory` table to see if any migrations need to run.</span></span> <span data-ttu-id="21de2-219">Если базы данных находится в актуальном состоянии, перенос не выполняется.</span><span class="sxs-lookup"><span data-stu-id="21de2-219">If the DB is up to date, no migration is run.</span></span>

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="21de2-220">Интерфейс командной строки (CLI) vs. Консоль диспетчера пакетов (PMC)</span><span class="sxs-lookup"><span data-stu-id="21de2-220">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="21de2-221">EF основные средства для управления миграции можно найти:</span><span class="sxs-lookup"><span data-stu-id="21de2-221">The EF Core tooling for managing migrations is available from:</span></span>

* <span data-ttu-id="21de2-222">Команды .NET core CLI.</span><span class="sxs-lookup"><span data-stu-id="21de2-222">.NET Core CLI commands.</span></span>
* <span data-ttu-id="21de2-223">Командлеты PowerShell в Visual Studio **консоль диспетчера пакетов** (PMC) окна.</span><span class="sxs-lookup"><span data-stu-id="21de2-223">The PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span>

<span data-ttu-id="21de2-224">Этот учебник показывает, как использовать CLI, некоторые разработчики предпочитают с помощью PMC.</span><span class="sxs-lookup"><span data-stu-id="21de2-224">This tutorial shows how to use the CLI, some developers prefer using the PMC.</span></span>

<span data-ttu-id="21de2-225">EF основные команды для PMC находятся в [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) пакета.</span><span class="sxs-lookup"><span data-stu-id="21de2-225">The EF Core commands for the PMC are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="21de2-226">Этот пакет включен в [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, поэтому устанавливать его не требуется.</span><span class="sxs-lookup"><span data-stu-id="21de2-226">This package is included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="21de2-227">**Важно:** это не тот же пакет, как установить для CLI, изменив *.csproj* файла.</span><span class="sxs-lookup"><span data-stu-id="21de2-227">**Important:** This is not the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="21de2-228">Имя данного объекта заканчивается на `Tools`, в отличие от имени пакета CLI, который заканчивается на `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="21de2-228">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="21de2-229">Дополнительные сведения о командах CLI см. в разделе [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="21de2-229">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="21de2-230">Дополнительные сведения о командах PMC см. в разделе [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="21de2-230">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="21de2-231">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="21de2-231">Troubleshooting</span></span>

<span data-ttu-id="21de2-232">Загрузить [завершенное приложение для данного этапа](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="21de2-232">Download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="21de2-233">Приложение создает следующее исключение:</span><span class="sxs-lookup"><span data-stu-id="21de2-233">The app generates the following exception:</span></span>

```text
`SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="21de2-234">Решение: запустите`dotnet ef database update`</span><span class="sxs-lookup"><span data-stu-id="21de2-234">Solution: Run `dotnet ef database update`</span></span>

<span data-ttu-id="21de2-235">Если `update` команда возвращает ошибку «Ошибка построения.»:</span><span class="sxs-lookup"><span data-stu-id="21de2-235">If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="21de2-236">Выполните команду еще раз.</span><span class="sxs-lookup"><span data-stu-id="21de2-236">Run the command again.</span></span>
* <span data-ttu-id="21de2-237">Оставьте сообщение в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="21de2-237">Leave a message at the bottom of the page.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="21de2-238">[Назад](xref:data/ef-rp/sort-filter-page)
[Вперед](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="21de2-238">[Previous](xref:data/ef-rp/sort-filter-page)
[Next](xref:data/ef-rp/complex-data-model)</span></span>
