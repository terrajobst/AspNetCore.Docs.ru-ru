---
title: "Страниц Razor с основными EF - Migrations - 4, 8."
author: rick-anderson
description: "В этом учебнике сначала с помощью EF Core миграции для управления изменений модели данных в приложении ASP.NET Core MVC."
manager: wpickett
ms.author: riande
ms.date: 10/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/migrations
ms.openlocfilehash: e89d95702cb94556bc6e5dc73253c51acaa11578
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="migrations---ef-core-with-razor-pages-tutorial-4-of-8"></a><span data-ttu-id="478b2-103">Миграция - Core EF учебнику страниц Razor (4, 8)</span><span class="sxs-lookup"><span data-stu-id="478b2-103">Migrations - EF Core with Razor Pages tutorial (4 of 8)</span></span>

<span data-ttu-id="478b2-104">По [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="478b2-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="478b2-105">В этом учебнике функция миграции EF основных компонентов для управления изменений модели данных используется.</span><span class="sxs-lookup"><span data-stu-id="478b2-105">In this tutorial, the EF Core migrations feature for managing data model changes is used.</span></span>

<span data-ttu-id="478b2-106">Если возникли проблемы, не удается устранить, загрузите [завершенное приложение для данного этапа](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="478b2-106">If you run into problems you can't solve, download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="478b2-107">При разработке нового приложения, данные изменений в модели часто.</span><span class="sxs-lookup"><span data-stu-id="478b2-107">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="478b2-108">Каждый раз изменения модели, модель получает синхронизации с базой данных.</span><span class="sxs-lookup"><span data-stu-id="478b2-108">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="478b2-109">Этот учебник, запущенную настройка платформы Entity Framework для создания базы данных, если он не существует.</span><span class="sxs-lookup"><span data-stu-id="478b2-109">This tutorial started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="478b2-110">Каждый раз, изменений в модели данных:</span><span class="sxs-lookup"><span data-stu-id="478b2-110">Each time the data model changes:</span></span>

* <span data-ttu-id="478b2-111">Базы данных может быть удален.</span><span class="sxs-lookup"><span data-stu-id="478b2-111">The DB is dropped.</span></span>
* <span data-ttu-id="478b2-112">EF создает новый, который соответствует модели.</span><span class="sxs-lookup"><span data-stu-id="478b2-112">EF creates a new one that matches the model.</span></span>
* <span data-ttu-id="478b2-113">Приложение заполняет DB тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="478b2-113">The app seeds the DB with test data.</span></span>

<span data-ttu-id="478b2-114">Такой подход для синхронизации базы данных с моделью данных работает хорошо, пока развертывание приложения в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="478b2-114">This approach to keeping the DB in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="478b2-115">Когда приложение выполняется в производственной среде, он обычно хранит данные, которые необходимо сохранять.</span><span class="sxs-lookup"><span data-stu-id="478b2-115">When the app is running in production, it's usually storing data that needs to be maintained.</span></span> <span data-ttu-id="478b2-116">Приложение не может начинаться с тестирования базы данных при каждом изменении (например, Добавление нового столбца).</span><span class="sxs-lookup"><span data-stu-id="478b2-116">The app can't start with a test DB each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="478b2-117">Средство миграции Core EF решает эту проблему, позволяя EF основных компонентов для обновления схемы базы данных вместо создания новой базы данных.</span><span class="sxs-lookup"><span data-stu-id="478b2-117">The EF Core Migrations feature solves this problem by enabling EF Core to update the DB schema instead of creating a new DB.</span></span>

<span data-ttu-id="478b2-118">Вместо удаления и повторного создания базы данных, когда изменений в модели данных, миграция обновляет схему и сохраняет существующие данные.</span><span class="sxs-lookup"><span data-stu-id="478b2-118">Rather than dropping and recreating the DB when the data model changes, migrations updates the schema and retains existing data.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="478b2-119">Пакеты Entity Framework Core NuGet для миграций</span><span class="sxs-lookup"><span data-stu-id="478b2-119">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="478b2-120">Для работы с миграций, используйте **консоль диспетчера пакетов** (PMC) или интерфейса командной строки (CLI).</span><span class="sxs-lookup"><span data-stu-id="478b2-120">To work with migrations, use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span> <span data-ttu-id="478b2-121">Эти учебники демонстрируют использование команды CLI.</span><span class="sxs-lookup"><span data-stu-id="478b2-121">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="478b2-122">Сведения о PMC находится в [конце этого учебника](#pmc).</span><span class="sxs-lookup"><span data-stu-id="478b2-122">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="478b2-123">EF основные инструменты для командной строки (CLI), представлено в [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="478b2-123">The EF Core tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="478b2-124">Чтобы установить этот пакет, добавьте его в `DotNetCliToolReference` коллекции в *.csproj* файла, как показано.</span><span class="sxs-lookup"><span data-stu-id="478b2-124">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="478b2-125">**Примечание:** этот пакет должен быть установлен путем редактирования *.csproj* файла.</span><span class="sxs-lookup"><span data-stu-id="478b2-125">**Note:** This package must be installed by editing the *.csproj* file.</span></span> <span data-ttu-id="478b2-126">`install-package` Команды или диспетчер пакетов графического интерфейса пользователя не может использоваться для установки этого пакета.</span><span class="sxs-lookup"><span data-stu-id="478b2-126">The`install-package` command or the package manager GUI cannot be used to install this package.</span></span> <span data-ttu-id="478b2-127">Изменить *.csproj* файла, щелкнув правой кнопкой мыши имя проекта в **обозревателе решений** и выбрав **изменить ContosoUniversity.csproj**.</span><span class="sxs-lookup"><span data-stu-id="478b2-127">Edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

<span data-ttu-id="478b2-128">В следующем примере показана обновленный *.csproj* файла с помощью средств EF Core CLI выделяются:</span><span class="sxs-lookup"><span data-stu-id="478b2-128">The following markup shows the updated *.csproj* file with the EF Core CLI tools highlighted:</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
<span data-ttu-id="478b2-129">Номера версий в предыдущем примере были действительны, во время написания учебника.</span><span class="sxs-lookup"><span data-stu-id="478b2-129">The version numbers in the preceding example were current when the tutorial was written.</span></span> <span data-ttu-id="478b2-130">Для CLI EF основных средств, имеющихся в другие пакеты используют ту же версию.</span><span class="sxs-lookup"><span data-stu-id="478b2-130">Use the same version for the EF Core CLI tools found in the other packages.</span></span>

## <a name="change-the-connection-string"></a><span data-ttu-id="478b2-131">Измените строку подключения</span><span class="sxs-lookup"><span data-stu-id="478b2-131">Change the connection string</span></span>

<span data-ttu-id="478b2-132">В *appsettings.json* файл, измените имя базы данных в строке соединения на ContosoUniversity2.</span><span class="sxs-lookup"><span data-stu-id="478b2-132">In the *appsettings.json* file, change the name of the DB in the connection string to ContosoUniversity2.</span></span>

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="478b2-133">Изменение имени базы данных в строке подключения приводит к первой миграции для создания новой базы данных.</span><span class="sxs-lookup"><span data-stu-id="478b2-133">Changing the DB name in the connection string causes the first migration to create a new DB.</span></span> <span data-ttu-id="478b2-134">Новые базы данных создается, поскольку его с таким именем не существует.</span><span class="sxs-lookup"><span data-stu-id="478b2-134">A new DB is created because one with that name doesn't exist.</span></span> <span data-ttu-id="478b2-135">Изменение строки подключения не требуется для начала работы с миграции.</span><span class="sxs-lookup"><span data-stu-id="478b2-135">Changing the connection string isn't required for getting started with migrations.</span></span>

<span data-ttu-id="478b2-136">Вместо изменения имени базы данных удаление базы данных.</span><span class="sxs-lookup"><span data-stu-id="478b2-136">An alternative to changing the DB name is deleting the DB.</span></span> <span data-ttu-id="478b2-137">Используйте **обозреватель объектов SQL Server** (SSOX) или `database drop` команду CLI:</span><span class="sxs-lookup"><span data-stu-id="478b2-137">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>

 ```console
 dotnet ef database drop
 ```

<span data-ttu-id="478b2-138">Следующий раздел объясняет, как для выполнения команд CLI.</span><span class="sxs-lookup"><span data-stu-id="478b2-138">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="478b2-139">Создание первоначальной миграции</span><span class="sxs-lookup"><span data-stu-id="478b2-139">Create an initial migration</span></span>

<span data-ttu-id="478b2-140">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="478b2-140">Build the project.</span></span>

<span data-ttu-id="478b2-141">Откройте окно командной строки и перейдите в папку проекта.</span><span class="sxs-lookup"><span data-stu-id="478b2-141">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="478b2-142">Папка проекта содержит *файла Startup.cs* файл.</span><span class="sxs-lookup"><span data-stu-id="478b2-142">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="478b2-143">В командной строке введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="478b2-143">Enter the following in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="478b2-144">В командном окне отображается информация, аналогично приведенным ниже:</span><span class="sxs-lookup"><span data-stu-id="478b2-144">The command window displays information similar to the following:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="478b2-145">Если происходит сбой с сообщением «*не может получить доступ к файла... ContosoUniversity.dll, так как он используется другим процессом.* "</span><span class="sxs-lookup"><span data-stu-id="478b2-145">If the migration fails with the message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*"</span></span> <span data-ttu-id="478b2-146">отображается:</span><span class="sxs-lookup"><span data-stu-id="478b2-146">is displayed:</span></span>

* <span data-ttu-id="478b2-147">Остановить IIS Express.</span><span class="sxs-lookup"><span data-stu-id="478b2-147">Stop IIS Express.</span></span>

   * <span data-ttu-id="478b2-148">Закройте и перезапустите Visual Studio или</span><span class="sxs-lookup"><span data-stu-id="478b2-148">Exit and restart Visual Studio, or</span></span>
   * <span data-ttu-id="478b2-149">Значок IIS Express поиска на панели задач Windows.</span><span class="sxs-lookup"><span data-stu-id="478b2-149">Find the IIS Express icon in the Windows System Tray.</span></span>
   * <span data-ttu-id="478b2-150">Щелкните правой кнопкой мыши значок IIS Express, а затем нажмите кнопку **ContosoUniversity > Остановить узел**.</span><span class="sxs-lookup"><span data-stu-id="478b2-150">Right-click the IIS Express icon, and then click **ContosoUniversity > Stop Site**.</span></span>

<span data-ttu-id="478b2-151">Если сообщение об ошибке ««Сбой построения.</span><span class="sxs-lookup"><span data-stu-id="478b2-151">If the error message "Build failed."</span></span> <span data-ttu-id="478b2-152">отображается, выполните команду еще раз.</span><span class="sxs-lookup"><span data-stu-id="478b2-152">is displayed, run the command again.</span></span> <span data-ttu-id="478b2-153">Если эта ошибка появляется, оставьте комментарий в нижней части этого учебника.</span><span class="sxs-lookup"><span data-stu-id="478b2-153">If you get this error, leave a note at the bottom of this tutorial.</span></span>

### <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="478b2-154">Изучите вверх и вниз методы</span><span class="sxs-lookup"><span data-stu-id="478b2-154">Examine the Up and Down methods</span></span>

<span data-ttu-id="478b2-155">Команда EF Core `migrations add` созданного кода для создания из базы данных.</span><span class="sxs-lookup"><span data-stu-id="478b2-155">The EF Core command `migrations add` generated code to create the DB from.</span></span> <span data-ttu-id="478b2-156">Этот код миграции находится в *миграций\<timestamp > _InitialCreate.cs* файл.</span><span class="sxs-lookup"><span data-stu-id="478b2-156">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="478b2-157">`Up` Метод `InitialCreate` класс создает таблицы базы данных, которые соответствуют наборов сущностей модели данных.</span><span class="sxs-lookup"><span data-stu-id="478b2-157">The `Up` method of the `InitialCreate` class creates the DB tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="478b2-158">`Down` Метод удаляет их, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="478b2-158">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

<span data-ttu-id="478b2-159">Миграция вызовы `Up` метод для реализации изменений модели данных для миграции.</span><span class="sxs-lookup"><span data-stu-id="478b2-159">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="478b2-160">При вводе команды для отката обновления, вызывает миграции `Down` метод.</span><span class="sxs-lookup"><span data-stu-id="478b2-160">When you enter a command to roll back the update, migrations calls the `Down` method.</span></span>

<span data-ttu-id="478b2-161">Предыдущий код предназначен для первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="478b2-161">The preceding code is for the initial migration.</span></span> <span data-ttu-id="478b2-162">Этот код создан `migrations add InitialCreate` была выполнена команда.</span><span class="sxs-lookup"><span data-stu-id="478b2-162">That code was created when the `migrations add InitialCreate` command was run.</span></span> <span data-ttu-id="478b2-163">Для имени файла используется параметр name миграции («InitialCreate» в примере).</span><span class="sxs-lookup"><span data-stu-id="478b2-163">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="478b2-164">Имя миграции, может быть любое допустимое имя файла.</span><span class="sxs-lookup"><span data-stu-id="478b2-164">The migration name can be any valid file name.</span></span> <span data-ttu-id="478b2-165">Рекомендуется выбрать слово или фразу, в которой говорится, что выполняется в процессе переноса.</span><span class="sxs-lookup"><span data-stu-id="478b2-165">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="478b2-166">Например миграции, которая добавлена таблица department может иметь название «AddDepartmentTable».</span><span class="sxs-lookup"><span data-stu-id="478b2-166">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

<span data-ttu-id="478b2-167">Если создается первоначальной миграции и выходит из базы данных:</span><span class="sxs-lookup"><span data-stu-id="478b2-167">If the initial migration is created and the DB exits:</span></span>

* <span data-ttu-id="478b2-168">Создается код создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="478b2-168">The DB creation code is generated.</span></span>
* <span data-ttu-id="478b2-169">Создание кода БД не нужно выполнить, так как базы данных уже соответствует модели данных.</span><span class="sxs-lookup"><span data-stu-id="478b2-169">The DB creation code doesn't need to run because the DB already matches the data model.</span></span> <span data-ttu-id="478b2-170">При выполнении кода создания DB не вносите никаких изменений, так как базы данных, уже соответствует модели данных.</span><span class="sxs-lookup"><span data-stu-id="478b2-170">If the The DB creation code is run, it doesn't make any changes because the DB already matches the data model.</span></span>

<span data-ttu-id="478b2-171">При развертывании приложения в новую среду создания кода БД необходимо выполнить для создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="478b2-171">When the app is deployed to a new environment, the DB creation code must be run to create the DB.</span></span>

<span data-ttu-id="478b2-172">Ранее строки подключения была изменена для использования нового имени для базы данных.</span><span class="sxs-lookup"><span data-stu-id="478b2-172">Previously the connection string was changed to use a new name for the DB.</span></span> <span data-ttu-id="478b2-173">Указанной базы данных не существует, поэтому миграция создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="478b2-173">The specified DB doesn't exist, so migrations creates the DB.</span></span>

### <a name="examine-the-data-model-snapshot"></a><span data-ttu-id="478b2-174">Изучите снимок модели данных</span><span class="sxs-lookup"><span data-stu-id="478b2-174">Examine the data model snapshot</span></span>

<span data-ttu-id="478b2-175">Миграция создает *моментального снимка* текущей схемы базы данных в *Migrations/SchoolContextModelSnapshot.cs*:</span><span class="sxs-lookup"><span data-stu-id="478b2-175">Migrations creates a *snapshot* of the current DB schema in *Migrations/SchoolContextModelSnapshot.cs*:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

<span data-ttu-id="478b2-176">Поскольку текущая схема базы данных представлен в коде, EF Core не должны взаимодействовать с базы данных для создания миграции.</span><span class="sxs-lookup"><span data-stu-id="478b2-176">Because the current DB schema is represented in code, EF Core doesn't have to interact with the DB to create migrations.</span></span> <span data-ttu-id="478b2-177">При добавлении миграции EF Core определяет, что изменилось, сравнивая модели данных в файле моментального снимка.</span><span class="sxs-lookup"><span data-stu-id="478b2-177">When you add a migration, EF Core determines what changed by comparing the data model to the snapshot file.</span></span> <span data-ttu-id="478b2-178">EF Core взаимодействует с базы данных, только когда требуется обновление базы данных.</span><span class="sxs-lookup"><span data-stu-id="478b2-178">EF Core interacts with the DB only when it has to update the DB.</span></span>

<span data-ttu-id="478b2-179">Файл моментальных снимков должен быть синхронизирован с миграций, которые он создан.</span><span class="sxs-lookup"><span data-stu-id="478b2-179">The snapshot file must be in sync with the migrations that created it.</span></span> <span data-ttu-id="478b2-180">Нельзя удалить миграции, удалив файл  *\<timestamp > _\<migrationname > .cs*.</span><span class="sxs-lookup"><span data-stu-id="478b2-180">A migration can't be removed by deleting the file named *\<timestamp>_\<migrationname>.cs*.</span></span> <span data-ttu-id="478b2-181">Если этот файл удаляется, оставшиеся операции миграции не синхронизированы с файлом моментального снимка базы данных.</span><span class="sxs-lookup"><span data-stu-id="478b2-181">If that file is deleted, the remaining migrations are out of sync with the DB snapshot file.</span></span> <span data-ttu-id="478b2-182">Чтобы удалить последний переноса добавлен, используйте [dotnet ef миграции удалить](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) команды.</span><span class="sxs-lookup"><span data-stu-id="478b2-182">To delete the last migration added, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span>

## <a name="remove-ensurecreated"></a><span data-ttu-id="478b2-183">Удалить EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="478b2-183">Remove EnsureCreated</span></span>

<span data-ttu-id="478b2-184">Для раннего разработки `EnsureCreated` была использована.</span><span class="sxs-lookup"><span data-stu-id="478b2-184">For early development, the `EnsureCreated` command was used.</span></span> <span data-ttu-id="478b2-185">В этом учебнике используется миграции.</span><span class="sxs-lookup"><span data-stu-id="478b2-185">In this tutorial, migrations is used.</span></span> <span data-ttu-id="478b2-186">`EnsureCreated`имеет следующие ограничения:</span><span class="sxs-lookup"><span data-stu-id="478b2-186">`EnsureCreated` has the following limitations:</span></span>

* <span data-ttu-id="478b2-187">Пропускает миграции и создает базы данных и схемы.</span><span class="sxs-lookup"><span data-stu-id="478b2-187">Bypasses migrations and creates the DB and schema.</span></span>
* <span data-ttu-id="478b2-188">Не приводит к созданию таблицы миграций.</span><span class="sxs-lookup"><span data-stu-id="478b2-188">Doesn't create a migrations table.</span></span>
* <span data-ttu-id="478b2-189">Можно *не* использоваться с миграции.</span><span class="sxs-lookup"><span data-stu-id="478b2-189">Can *not* be used with migrations.</span></span>
* <span data-ttu-id="478b2-190">Предназначен для тестирования или быстрого создания прототипов. она где базы данных будет удалена и создана заново часто.</span><span class="sxs-lookup"><span data-stu-id="478b2-190">Is designed for testing or rapid prototyping where the DB is dropped and re-created frequently.</span></span>

<span data-ttu-id="478b2-191">Удалите следующую строку из `DbInitializer`:</span><span class="sxs-lookup"><span data-stu-id="478b2-191">Remove the following line from `DbInitializer`:</span></span>

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a><span data-ttu-id="478b2-192">Применить к базе данных. в разработке миграции</span><span class="sxs-lookup"><span data-stu-id="478b2-192">Apply the migration to the DB in development</span></span>

<span data-ttu-id="478b2-193">В окне командной строки введите следующую команду, чтобы создать базы данных и таблицы.</span><span class="sxs-lookup"><span data-stu-id="478b2-193">In the command window, enter the following to create the DB and tables.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="478b2-194">Примечание: Если `update` команда возвращает ошибку «Ошибка построения.»:</span><span class="sxs-lookup"><span data-stu-id="478b2-194">Note: If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="478b2-195">Выполните команду еще раз.</span><span class="sxs-lookup"><span data-stu-id="478b2-195">Run the command again.</span></span>
* <span data-ttu-id="478b2-196">Если ошибка повториться, выйдите из Visual Studio, а затем запустите `update` команды.</span><span class="sxs-lookup"><span data-stu-id="478b2-196">If it fails again, exit Visual Studio and then run the `update` command.</span></span>
* <span data-ttu-id="478b2-197">Оставьте сообщение в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="478b2-197">Leave a message at the bottom of the page.</span></span>

<span data-ttu-id="478b2-198">Выходные данные команды аналогичен `migrations add` выходные данные команды.</span><span class="sxs-lookup"><span data-stu-id="478b2-198">The output from the command is similar to the `migrations add` command output.</span></span> <span data-ttu-id="478b2-199">В предыдущей команде отображаются журналы для команды SQL, которые настроены в базе данных.</span><span class="sxs-lookup"><span data-stu-id="478b2-199">In the preceding command, logs for the SQL commands that set up the DB are displayed.</span></span> <span data-ttu-id="478b2-200">Большинство журналов опущены в следующий результат:</span><span class="sxs-lookup"><span data-stu-id="478b2-200">Most of the logs are omitted in the following sample output:</span></span>

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

<span data-ttu-id="478b2-201">Чтобы снизить уровень детализации сообщений журнала, можно изменить уровни журнала в *appsettings. Development.JSON* файл.</span><span class="sxs-lookup"><span data-stu-id="478b2-201">To reduce the level of detail in log messages, can change the log levels in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="478b2-202">Дополнительные сведения см. в разделе [введение к ведению журнала](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="478b2-202">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

<span data-ttu-id="478b2-203">Используйте **обозреватель объектов SQL Server** для проверки базы данных.</span><span class="sxs-lookup"><span data-stu-id="478b2-203">Use **SQL Server Object Explorer** to inspect the DB.</span></span> <span data-ttu-id="478b2-204">Обратите внимание на добавленную `__EFMigrationsHistory` таблицы.</span><span class="sxs-lookup"><span data-stu-id="478b2-204">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="478b2-205">`__EFMigrationsHistory` Таблица отслеживает отслеживания объекта миграций, которые были применены к базе данных.</span><span class="sxs-lookup"><span data-stu-id="478b2-205">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the DB.</span></span> <span data-ttu-id="478b2-206">Просмотр данных в `__EFMigrationsHistory` таблицу, она показана одна строка для первой миграции.</span><span class="sxs-lookup"><span data-stu-id="478b2-206">View the data in the `__EFMigrationsHistory` table, it shows one row for the first migration.</span></span> <span data-ttu-id="478b2-207">Последний журнал в предыдущем примере вывода CLI показывает инструкции INSERT, которая создает эту строку.</span><span class="sxs-lookup"><span data-stu-id="478b2-207">The last log in the preceding CLI output example shows the INSERT statement that creates this row.</span></span>

<span data-ttu-id="478b2-208">Запустите приложение и убедитесь, что все работает.</span><span class="sxs-lookup"><span data-stu-id="478b2-208">Run the app and verify that everything works.</span></span>

## <a name="appling-migrations-in-production"></a><span data-ttu-id="478b2-209">Применение миграции в рабочей среде</span><span class="sxs-lookup"><span data-stu-id="478b2-209">Appling migrations in production</span></span>

<span data-ttu-id="478b2-210">Мы рекомендуем производственных приложений следует **не** вызвать [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="478b2-210">We recommend production apps should **not** call [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="478b2-211">`Migrate`не следует вызывать из приложения в ферме серверов.</span><span class="sxs-lookup"><span data-stu-id="478b2-211">`Migrate` shouldn't be called from an app in server farm.</span></span> <span data-ttu-id="478b2-212">Например, если приложение было облака, развернутых с помощью масштабирования (запущено несколько экземпляров приложения).</span><span class="sxs-lookup"><span data-stu-id="478b2-212">For example, if the app has been cloud deployed with scale-out (multiple instances of the app are running).</span></span>

<span data-ttu-id="478b2-213">Миграция базы данных должно выполняться как часть развертывания и контролируемым образом.</span><span class="sxs-lookup"><span data-stu-id="478b2-213">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="478b2-214">Следующие подходы к переносу рабочей базы данных.</span><span class="sxs-lookup"><span data-stu-id="478b2-214">Production database migration approaches include:</span></span>

* <span data-ttu-id="478b2-215">Создание сценариев SQL и использование скриптов SQL в среде с помощью миграций.</span><span class="sxs-lookup"><span data-stu-id="478b2-215">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="478b2-216">Под управлением `dotnet ef database update` из управляемой среды.</span><span class="sxs-lookup"><span data-stu-id="478b2-216">Running `dotnet ef database update` from a controlled environment.</span></span>

<span data-ttu-id="478b2-217">EF Core использует `__MigrationsHistory` таблицу для просмотра, если любой миграции необходимо выполнить.</span><span class="sxs-lookup"><span data-stu-id="478b2-217">EF Core uses the `__MigrationsHistory` table to see if any migrations need to run.</span></span> <span data-ttu-id="478b2-218">Если базы данных находится в актуальном состоянии, перенос не выполняется.</span><span class="sxs-lookup"><span data-stu-id="478b2-218">If the DB is up to date, no migration is run.</span></span>

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="478b2-219">Интерфейс командной строки (CLI) vs. Консоль диспетчера пакетов (PMC)</span><span class="sxs-lookup"><span data-stu-id="478b2-219">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="478b2-220">EF основные средства для управления миграции можно найти:</span><span class="sxs-lookup"><span data-stu-id="478b2-220">The EF Core tooling for managing migrations is available from:</span></span>

* <span data-ttu-id="478b2-221">Команды .NET core CLI.</span><span class="sxs-lookup"><span data-stu-id="478b2-221">.NET Core CLI commands.</span></span>
* <span data-ttu-id="478b2-222">Командлеты PowerShell в Visual Studio **консоль диспетчера пакетов** (PMC) окна.</span><span class="sxs-lookup"><span data-stu-id="478b2-222">The PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span>

<span data-ttu-id="478b2-223">Этот учебник показывает, как использовать CLI, некоторые разработчики предпочитают с помощью PMC.</span><span class="sxs-lookup"><span data-stu-id="478b2-223">This tutorial shows how to use the CLI, some developers prefer using the PMC.</span></span>

<span data-ttu-id="478b2-224">EF основные команды для PMC находятся в [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) пакета.</span><span class="sxs-lookup"><span data-stu-id="478b2-224">The EF Core commands for the PMC are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="478b2-225">Этот пакет включен в [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, поэтому устанавливать его не требуется.</span><span class="sxs-lookup"><span data-stu-id="478b2-225">This package is included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="478b2-226">**Важно:** это не тот же пакет, как установить для CLI, изменив *.csproj* файла.</span><span class="sxs-lookup"><span data-stu-id="478b2-226">**Important:** This isn't the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="478b2-227">Имя данного объекта заканчивается на `Tools`, в отличие от имени пакета CLI, который заканчивается на `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="478b2-227">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="478b2-228">Дополнительные сведения о командах CLI см. в разделе [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="478b2-228">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="478b2-229">Дополнительные сведения о командах PMC см. в разделе [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="478b2-229">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="478b2-230">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="478b2-230">Troubleshooting</span></span>

<span data-ttu-id="478b2-231">Загрузить [завершенное приложение для данного этапа](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span><span class="sxs-lookup"><span data-stu-id="478b2-231">Download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="478b2-232">Приложение создает следующее исключение:</span><span class="sxs-lookup"><span data-stu-id="478b2-232">The app generates the following exception:</span></span>

```text
`SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="478b2-233">Решение: запустите`dotnet ef database update`</span><span class="sxs-lookup"><span data-stu-id="478b2-233">Solution: Run `dotnet ef database update`</span></span>

<span data-ttu-id="478b2-234">Если `update` команда возвращает ошибку «Ошибка построения.»:</span><span class="sxs-lookup"><span data-stu-id="478b2-234">If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="478b2-235">Выполните команду еще раз.</span><span class="sxs-lookup"><span data-stu-id="478b2-235">Run the command again.</span></span>
* <span data-ttu-id="478b2-236">Оставьте сообщение в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="478b2-236">Leave a message at the bottom of the page.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="478b2-237">[Назад](xref:data/ef-rp/sort-filter-page)
[Вперед](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="478b2-237">[Previous](xref:data/ef-rp/sort-filter-page)
[Next](xref:data/ef-rp/complex-data-model)</span></span>
