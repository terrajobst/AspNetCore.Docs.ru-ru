---
title: Учебник. Использование ASP.NET MVC с EF Core. Функции миграций
description: В этом руководстве вы начинаете использовать функцию миграций EF Core для управления изменениями модели данных в приложении ASP.NET Core MVC.
author: rick-anderson
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/04/2019
ms.topic: tutorial
uid: data/ef-mvc/migrations
ms.openlocfilehash: ac924e7d6bee2f02ab11281a5c27f2c94a7183b3
ms.sourcegitcommit: 5e3797a02ff3c48bb8cb9ad4320bfd169ebe8aba
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2019
ms.locfileid: "56102998"
---
# <a name="tutorial-using-the-migrations-feature---aspnet-mvc-with-ef-core"></a><span data-ttu-id="e3c88-103">Учебник. Использование ASP.NET MVC с EF Core. Функции миграций</span><span class="sxs-lookup"><span data-stu-id="e3c88-103">Tutorial: Using the migrations feature - ASP.NET MVC with EF Core</span></span>

<span data-ttu-id="e3c88-104">В этом руководстве вы начинаете использовать функцию миграций EF Core для управления изменениями модели данных.</span><span class="sxs-lookup"><span data-stu-id="e3c88-104">In this tutorial, you start using the EF Core migrations feature for managing data model changes.</span></span> <span data-ttu-id="e3c88-105">В последующих руководствах вы добавите дополнительные миграции по мере изменения модели данных.</span><span class="sxs-lookup"><span data-stu-id="e3c88-105">In later tutorials, you'll add more migrations as you change the data model.</span></span>

<span data-ttu-id="e3c88-106">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="e3c88-106">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e3c88-107">Дополнительные сведения о миграциях</span><span class="sxs-lookup"><span data-stu-id="e3c88-107">Learn about migrations</span></span>
> * <span data-ttu-id="e3c88-108">Дополнительные сведения о пакетах миграции NuGet</span><span class="sxs-lookup"><span data-stu-id="e3c88-108">Learn about NuGet migration packages</span></span>
> * <span data-ttu-id="e3c88-109">Изменение строки подключения</span><span class="sxs-lookup"><span data-stu-id="e3c88-109">Change the connection string</span></span>
> * <span data-ttu-id="e3c88-110">Создание первоначальной миграции</span><span class="sxs-lookup"><span data-stu-id="e3c88-110">Create an initial migration</span></span>
> * <span data-ttu-id="e3c88-111">Обзор методов Up и Down</span><span class="sxs-lookup"><span data-stu-id="e3c88-111">Examine Up and Down methods</span></span>
> * <span data-ttu-id="e3c88-112">Дополнительные сведения о моментальном снимке модели данных</span><span class="sxs-lookup"><span data-stu-id="e3c88-112">Learn about the data model snapshot</span></span>
> * <span data-ttu-id="e3c88-113">Применение миграции</span><span class="sxs-lookup"><span data-stu-id="e3c88-113">Apply the migration</span></span>


## <a name="prerequisites"></a><span data-ttu-id="e3c88-114">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="e3c88-114">Prerequisites</span></span>

* [<span data-ttu-id="e3c88-115">Добавление сортировки, фильтрации и разбиения по страницам с использованием EF Core в приложении MVC ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e3c88-115">Add sorting, filtering, and paging with EF Core in an ASP.NET Core MVC app</span></span>](sort-filter-page.md)

## <a name="about-migrations"></a><span data-ttu-id="e3c88-116">Сведения о миграциях</span><span class="sxs-lookup"><span data-stu-id="e3c88-116">About migrations</span></span>

<span data-ttu-id="e3c88-117">При разработке нового приложения ваша модель данных часто меняется, и при каждом таком изменении она нарушает синхронизацию с базой данных.</span><span class="sxs-lookup"><span data-stu-id="e3c88-117">When you develop a new application, your data model changes frequently, and each time the model changes, it gets out of sync with the database.</span></span> <span data-ttu-id="e3c88-118">Вы начали работу с этими руководствами, настроив Entity Framework для создания базы данных, если она еще не существует.</span><span class="sxs-lookup"><span data-stu-id="e3c88-118">You started these tutorials by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="e3c88-119">Затем при каждом изменении модели данных — добавлении, удалении или изменении классов сущностей или изменении класса DbContext — вы можете удалить базу данных, после чего EF создает новую, которая соответствует данной модели, и заполняет ее тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="e3c88-119">Then each time you change the data model -- add, remove, or change entity classes or change your DbContext class -- you can delete the database and EF creates a new one that matches the model, and seeds it with test data.</span></span>

<span data-ttu-id="e3c88-120">Этот способ для обеспечения синхронизации базы данных с моделью данных хорошо работает до развертывания приложения в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="e3c88-120">This method of keeping the database in sync with the data model works well until you deploy the application to production.</span></span> <span data-ttu-id="e3c88-121">Когда приложение выполняется в рабочей среде, оно обычно хранит данные, которые вы хотите сохранить, и вам нежелательно терять все при каждом изменении, например при добавлении нового столбца.</span><span class="sxs-lookup"><span data-stu-id="e3c88-121">When the application is running in production it's usually storing data that you want to keep, and you don't want to lose everything each time you make a change such as adding a new column.</span></span> <span data-ttu-id="e3c88-122">Функция миграций EF Core решает эту проблему, позволяя EF обновить схему базы данных вместо создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="e3c88-122">The EF Core Migrations feature solves this problem by enabling EF to update the database schema instead of creating  a new database.</span></span>

## <a name="about-nuget-migration-packages"></a><span data-ttu-id="e3c88-123">Сведения о пакетах миграции NuGet</span><span class="sxs-lookup"><span data-stu-id="e3c88-123">About NuGet migration packages</span></span>

<span data-ttu-id="e3c88-124">Для работы с миграциями можно использовать **консоль диспетчера пакетов** (PMC) или интерфейс командной строки (CLI).</span><span class="sxs-lookup"><span data-stu-id="e3c88-124">To work with migrations, you can use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span>  <span data-ttu-id="e3c88-125">Эти руководства демонстрируют использование команд интерфейса командной строки.</span><span class="sxs-lookup"><span data-stu-id="e3c88-125">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="e3c88-126">Дополнительные сведения о PMC [приведены в конце этого руководства](#pmc).</span><span class="sxs-lookup"><span data-stu-id="e3c88-126">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="e3c88-127">Средства EF для интерфейса командной строки (CLI) доступны в [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="e3c88-127">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="e3c88-128">Чтобы установить этот пакет, добавьте его в коллекцию `DotNetCliToolReference` в файле *CSPROJ*, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="e3c88-128">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="e3c88-129">**Примечание.** Необходимо установить этот пакет, изменив файл *.csproj*. Использовать команду `install-package` или графический пользовательский интерфейс диспетчера пакетов нельзя.</span><span class="sxs-lookup"><span data-stu-id="e3c88-129">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span> <span data-ttu-id="e3c88-130">Вы можете изменить файл *CSPROJ*, щелкнув правой кнопкой мыши имя проекта в **обозревателе решений** и выбрав пункт **Изменить ContosoUniversity.csproj**.</span><span class="sxs-lookup"><span data-stu-id="e3c88-130">You can edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]

<span data-ttu-id="e3c88-131">(На момент написания руководства номера версий в этом примере были актуальными.)</span><span class="sxs-lookup"><span data-stu-id="e3c88-131">(The version numbers in this example were current when the tutorial was written.)</span></span>

## <a name="change-the-connection-string"></a><span data-ttu-id="e3c88-132">Изменение строки подключения</span><span class="sxs-lookup"><span data-stu-id="e3c88-132">Change the connection string</span></span>

<span data-ttu-id="e3c88-133">В файле *appsettings.json* измените имя базы данных в строке подключения на ContosoUniversity2 или другое имя, которое вы еще не использовали на компьютере, с которым работаете.</span><span class="sxs-lookup"><span data-stu-id="e3c88-133">In the *appsettings.json* file, change the name of the database in the connection string to ContosoUniversity2 or some other name that you haven't used on the computer you're using.</span></span>

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="e3c88-134">Это изменение настраивает проект, чтобы первая миграция создала базу данных.</span><span class="sxs-lookup"><span data-stu-id="e3c88-134">This change sets up the project so that the first migration will create a new database.</span></span> <span data-ttu-id="e3c88-135">Это необязательно для начала работы с миграциями, но позднее вы поймете, почему так стоит сделать.</span><span class="sxs-lookup"><span data-stu-id="e3c88-135">This isn't required to get started with migrations, but you'll see later why it's a good idea.</span></span>

> [!NOTE]
> <span data-ttu-id="e3c88-136">Вместо изменения имени базы данных можно удалить ее.</span><span class="sxs-lookup"><span data-stu-id="e3c88-136">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="e3c88-137">Воспользуйтесь **обозревателем объектов SQL Server** (SSOX) или командой интерфейса командной строки `database drop`:</span><span class="sxs-lookup"><span data-stu-id="e3c88-137">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```
> <span data-ttu-id="e3c88-138">Следующий раздел описывает выполнение команд интерфейса командной строки.</span><span class="sxs-lookup"><span data-stu-id="e3c88-138">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="e3c88-139">Создание первоначальной миграции</span><span class="sxs-lookup"><span data-stu-id="e3c88-139">Create an initial migration</span></span>

<span data-ttu-id="e3c88-140">Сохраните изменения и выполните сборку проекта.</span><span class="sxs-lookup"><span data-stu-id="e3c88-140">Save your changes and build the project.</span></span> <span data-ttu-id="e3c88-141">После этого откройте командное окно и в папку проекта.</span><span class="sxs-lookup"><span data-stu-id="e3c88-141">Then open a command window and navigate to the project folder.</span></span> <span data-ttu-id="e3c88-142">Вот как это можно сделать быстро:</span><span class="sxs-lookup"><span data-stu-id="e3c88-142">Here's a quick way to do that:</span></span>

* <span data-ttu-id="e3c88-143">Щелкните правой кнопкой мыши проект в **обозревателе решений** и выберите в контекстном меню пункт **Открыть папку в проводнике**.</span><span class="sxs-lookup"><span data-stu-id="e3c88-143">In **Solution Explorer**, right-click the project and choose **Open Folder in File Explorer** from the context menu.</span></span>

  ![Элемент меню "Открыть в проводнике"](migrations/_static/open-in-file-explorer.png)

* <span data-ttu-id="e3c88-145">Введите "cmd" в адресной строке и нажмите клавишу ВВОД.</span><span class="sxs-lookup"><span data-stu-id="e3c88-145">Enter "cmd" in the address bar and press Enter.</span></span>

  ![Открытие командного окна](migrations/_static/open-command-window.png)

<span data-ttu-id="e3c88-147">Введите в командном окне следующую команду:</span><span class="sxs-lookup"><span data-stu-id="e3c88-147">Enter the following command in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="e3c88-148">В командном окне отображаются следующие выходные данные:</span><span class="sxs-lookup"><span data-stu-id="e3c88-148">You see output like the following in the command window:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> <span data-ttu-id="e3c88-149">Если отображается сообщение об ошибке *No executable found matching command "dotnet-ef"* (Не найден исполняемый файл, соответствующий команде "dotnet-ef"), сведения об устранении проблемы см. в [этой записи блога](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/).</span><span class="sxs-lookup"><span data-stu-id="e3c88-149">If you see an error message *No executable found matching command "dotnet-ef"*, see [this blog post](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) for help troubleshooting.</span></span>

<span data-ttu-id="e3c88-150">Если отображается сообщение об ошибке "*Доступ к файлу... ContosoUniversity.dll невозможен, так как файл используется другим процессом*", найдите значок IIS Express в области уведомлений Windows, щелкните его правой кнопкой мыши, а затем выберите **ContosoUniversity > Остановить сайт**.</span><span class="sxs-lookup"><span data-stu-id="e3c88-150">If you see an error message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*", find the IIS Express icon in the Windows System Tray, and right-click it, then click **ContosoUniversity > Stop Site**.</span></span>

## <a name="examine-up-and-down-methods"></a><span data-ttu-id="e3c88-151">Обзор методов Up и Down</span><span class="sxs-lookup"><span data-stu-id="e3c88-151">Examine Up and Down methods</span></span>

<span data-ttu-id="e3c88-152">При выполнении команды `migrations add` система EF сформировала код, который создаст базу данных с нуля.</span><span class="sxs-lookup"><span data-stu-id="e3c88-152">When you executed the `migrations add` command, EF generated the code that will create the database from scratch.</span></span> <span data-ttu-id="e3c88-153">Этот код находится в файле *\<метка_времени>_InitialCreate.cs* внутри папки *Migrations*.</span><span class="sxs-lookup"><span data-stu-id="e3c88-153">This code is in the *Migrations* folder, in the file named *\<timestamp>_InitialCreate.cs*.</span></span> <span data-ttu-id="e3c88-154">Метод `Up` класса `InitialCreate` создает таблицы базы данных, которые соответствуют наборам сущностей модели данных, а метод `Down` удаляет их, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="e3c88-154">The `Up` method of the `InitialCreate` class creates the database tables that correspond to the data model entity sets, and the `Down` method deletes them, as shown in the following example.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

<span data-ttu-id="e3c88-155">Функция миграций вызывает метод `Up`, чтобы реализовать изменения модели данных для миграции.</span><span class="sxs-lookup"><span data-stu-id="e3c88-155">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="e3c88-156">При вводе команды для отката обновления функция миграций вызывает метод `Down`.</span><span class="sxs-lookup"><span data-stu-id="e3c88-156">When you enter a command to roll back the update, Migrations calls the `Down` method.</span></span>

<span data-ttu-id="e3c88-157">Этот пример кода предназначен для первоначальной миграции, созданной при вводе команды `migrations add InitialCreate`.</span><span class="sxs-lookup"><span data-stu-id="e3c88-157">This code is for the initial migration that was created when you entered the `migrations add InitialCreate` command.</span></span> <span data-ttu-id="e3c88-158">Параметр имени миграции (в примере это "InitialCreate") используется в качестве имени файла и может быть любым.</span><span class="sxs-lookup"><span data-stu-id="e3c88-158">The migration name parameter ("InitialCreate" in the example) is used for the file name and can be whatever you want.</span></span> <span data-ttu-id="e3c88-159">Рекомендуется выбрать слово или фразу, которые кратко описывают назначение миграции.</span><span class="sxs-lookup"><span data-stu-id="e3c88-159">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="e3c88-160">Например, последнюю миграцию можно назвать "AddDepartmentTable".</span><span class="sxs-lookup"><span data-stu-id="e3c88-160">For example, you might name a later migration "AddDepartmentTable".</span></span>

<span data-ttu-id="e3c88-161">Если вы создали первоначальную миграцию, когда база данных уже существовала, код для создания базы данных формируется, но выполнять его не требуется, так как база данных уже соответствует модели данных.</span><span class="sxs-lookup"><span data-stu-id="e3c88-161">If you created the initial migration when the database already exists, the database creation code is generated but it doesn't have to run because the database already matches the data model.</span></span> <span data-ttu-id="e3c88-162">Однако при развертывании приложения в другой среде, где база данных еще не существует, этот код будет выполняться для создания базы данных, поэтому рекомендуется сначала его протестировать.</span><span class="sxs-lookup"><span data-stu-id="e3c88-162">When you deploy the app to another environment where the database doesn't exist yet, this code will run to create your database, so it's a good idea to test it first.</span></span> <span data-ttu-id="e3c88-163">Вот почему ранее вы изменили имя базы данных в строке подключения — это позволяет функции миграций создать базу данных с нуля.</span><span class="sxs-lookup"><span data-stu-id="e3c88-163">That's why you changed the name of the database in the connection string earlier -- so that migrations can create a new one from scratch.</span></span>

## <a name="the-data-model-snapshot"></a><span data-ttu-id="e3c88-164">Моментальный снимок модели данных</span><span class="sxs-lookup"><span data-stu-id="e3c88-164">The data model snapshot</span></span>

<span data-ttu-id="e3c88-165">Функция миграций создает *моментальный снимок* текущей схемы базы данных в *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="e3c88-165">Migrations creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="e3c88-166">При добавлении миграции EF определяет, что именно изменилось, сравнивая модель данных с файлом моментального снимка.</span><span class="sxs-lookup"><span data-stu-id="e3c88-166">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span>

<span data-ttu-id="e3c88-167">При удалении миграции используйте команду [dotnet ef migrations remove](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).</span><span class="sxs-lookup"><span data-stu-id="e3c88-167">When deleting a migration, use the [dotnet ef migrations remove](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span> <span data-ttu-id="e3c88-168">`dotnet ef migrations remove` удаляет миграцию и гарантирует корректный сброс моментального снимка.</span><span class="sxs-lookup"><span data-stu-id="e3c88-168">`dotnet ef migrations remove` deletes the migration and ensures the snapshot is correctly reset.</span></span>

<span data-ttu-id="e3c88-169">Дополнительные сведения об использовании файла моментального снимка см. в разделе [Миграции Core EF в средах групп](/ef/core/managing-schemas/migrations/teams).</span><span class="sxs-lookup"><span data-stu-id="e3c88-169">See [EF Core Migrations in Team Environments](/ef/core/managing-schemas/migrations/teams) for more information about how the snapshot file is used.</span></span>

## <a name="apply-the-migration"></a><span data-ttu-id="e3c88-170">Применение миграции</span><span class="sxs-lookup"><span data-stu-id="e3c88-170">Apply the migration</span></span>

<span data-ttu-id="e3c88-171">Введите следующую команду в командном окне, чтобы создать базу данных и таблицы в ней.</span><span class="sxs-lookup"><span data-stu-id="e3c88-171">In the command window, enter the following command to create the database and tables in it.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="e3c88-172">Выходные данные команды аналогичны команде `migrations add`, за исключением того, что вы видите журналы для команд SQL, настраивающих базу данных.</span><span class="sxs-lookup"><span data-stu-id="e3c88-172">The output from the command is similar to the `migrations add` command, except that you see logs for the SQL commands that set up the database.</span></span> <span data-ttu-id="e3c88-173">В приведенном ниже примере выходных данных большинство журналов опущено.</span><span class="sxs-lookup"><span data-stu-id="e3c88-173">Most of the logs are omitted in the following sample output.</span></span> <span data-ttu-id="e3c88-174">Если вам не нужен такой уровень детализации сообщений журнала, можно изменить уровень ведения журнала в файле *appsettings.Development.json*.</span><span class="sxs-lookup"><span data-stu-id="e3c88-174">If you prefer not to see this level of detail in log messages, you can change the log level in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="e3c88-175">Для получения дополнительной информации см. <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="e3c88-175">For more information, see <xref:fundamentals/logging/index>.</span></span>

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

<span data-ttu-id="e3c88-176">Используйте **обозреватель объектов SQL Server** для проверки базы данных, как описано в первом руководстве.</span><span class="sxs-lookup"><span data-stu-id="e3c88-176">Use **SQL Server Object Explorer** to inspect the database as you did in the first tutorial.</span></span>  <span data-ttu-id="e3c88-177">Вы заметите добавление таблицы \_\_EFMigrationsHistory, отслеживающей миграции, которые были применены к базе данных.</span><span class="sxs-lookup"><span data-stu-id="e3c88-177">You'll notice the addition of an \_\_EFMigrationsHistory table that keeps track of which migrations have been applied to the database.</span></span> <span data-ttu-id="e3c88-178">Просмотрев данные в этой таблице, вы увидите одну строку для первой миграции.</span><span class="sxs-lookup"><span data-stu-id="e3c88-178">View the data in that table and you'll see one row for the first migration.</span></span> <span data-ttu-id="e3c88-179">(Последний журнал в предыдущем примере выходных данных интерфейса командной строки показывает оператор INSERT, создающий эту строку.)</span><span class="sxs-lookup"><span data-stu-id="e3c88-179">(The last log in the preceding CLI output example shows the INSERT statement that creates this row.)</span></span>

<span data-ttu-id="e3c88-180">Запустите приложение, чтобы убедиться, что все работает, как и раньше.</span><span class="sxs-lookup"><span data-stu-id="e3c88-180">Run the application to verify that everything still works the same as before.</span></span>

![Страница указателя учащихся](migrations/_static/students-index.png)

<a id="pmc"></a>

## <a name="compare-cli-and-pmc"></a><span data-ttu-id="e3c88-182">Сравнение CLI и PMC</span><span class="sxs-lookup"><span data-stu-id="e3c88-182">Compare CLI and PMC</span></span>

<span data-ttu-id="e3c88-183">Средства EF для управления миграциями доступны в виде команд интерфейса командной строки .NET Core или командлетов PowerShell в окне **консоли диспетчера пакетов** Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e3c88-183">The EF tooling for managing migrations is available from .NET Core CLI commands or from PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="e3c88-184">Это руководство описывает использование интерфейса командной строки, но вы можете использовать и консоль диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="e3c88-184">This tutorial shows how to use the CLI, but you can use the PMC if you prefer.</span></span>

<span data-ttu-id="e3c88-185">Команды EF для команд консоли диспетчера пакетов находятся в пакете [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools).</span><span class="sxs-lookup"><span data-stu-id="e3c88-185">The EF commands for the PMC commands are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="e3c88-186">Этот пакет входит в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), поэтому если приложение уже содержит ссылку на пакет `Microsoft.AspNetCore.App`, добавлять ссылку на пакет не нужно.</span><span class="sxs-lookup"><span data-stu-id="e3c88-186">This package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), so you don't need to add a package reference if your app has a package reference for `Microsoft.AspNetCore.App`.</span></span>

<span data-ttu-id="e3c88-187">**Внимание!** Это не тот же пакет, который вы устанавливаете для CLI, изменив файл *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="e3c88-187">**Important:** This isn't the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="e3c88-188">Его имя заканчивается на `Tools`, а имя пакета интерфейса командной строки — на `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="e3c88-188">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="e3c88-189">Дополнительные сведения о командах интерфейса командной строки см. в разделе [.NET Core CLI](/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="e3c88-189">For more information about the CLI commands, see [.NET Core CLI](/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="e3c88-190">Дополнительные сведения о командах консоли диспетчера пакетов см. в разделе [Консоль диспетчера пакетов (Visual Studio)](/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="e3c88-190">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="get-the-code"></a><span data-ttu-id="e3c88-191">Получение кода</span><span class="sxs-lookup"><span data-stu-id="e3c88-191">Get the code</span></span>

[<span data-ttu-id="e3c88-192">Скачайте или ознакомьтесь с готовым приложением.</span><span class="sxs-lookup"><span data-stu-id="e3c88-192">Download or view the completed application.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-step"></a><span data-ttu-id="e3c88-193">Дальнейшие действия</span><span class="sxs-lookup"><span data-stu-id="e3c88-193">Next step</span></span>

<span data-ttu-id="e3c88-194">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="e3c88-194">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e3c88-195">Дополнительные сведения о миграциях</span><span class="sxs-lookup"><span data-stu-id="e3c88-195">Learned about migrations</span></span>
> * <span data-ttu-id="e3c88-196">Дополнительные сведения о пакетах миграции NuGet</span><span class="sxs-lookup"><span data-stu-id="e3c88-196">Learned about NuGet migration packages</span></span>
> * <span data-ttu-id="e3c88-197">Изменение строки подключения</span><span class="sxs-lookup"><span data-stu-id="e3c88-197">Changed the connection string</span></span>
> * <span data-ttu-id="e3c88-198">Создание первоначальной миграции</span><span class="sxs-lookup"><span data-stu-id="e3c88-198">Created an initial migration</span></span>
> * <span data-ttu-id="e3c88-199">Обзор методов Up и Down</span><span class="sxs-lookup"><span data-stu-id="e3c88-199">Examined Up and Down methods</span></span>
> * <span data-ttu-id="e3c88-200">Дополнительные сведения о моментальном снимке модели данных</span><span class="sxs-lookup"><span data-stu-id="e3c88-200">Learned about the data model snapshot</span></span>
> * <span data-ttu-id="e3c88-201">Применение миграции</span><span class="sxs-lookup"><span data-stu-id="e3c88-201">Applied the migration</span></span>

<span data-ttu-id="e3c88-202">В следующем руководстве описаны более сложные вопросы, связанные с развертыванием модели данных.</span><span class="sxs-lookup"><span data-stu-id="e3c88-202">Advance to the next article to begin looking at more advanced topics about expanding the data model.</span></span> <span data-ttu-id="e3c88-203">Попутно вы создадите и примените дополнительные миграции.</span><span class="sxs-lookup"><span data-stu-id="e3c88-203">Along the way you'll create and apply additional migrations.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="e3c88-204">Создание и применение дополнительных миграций</span><span class="sxs-lookup"><span data-stu-id="e3c88-204">Create and apply additional migrations</span></span>](complex-data-model.md)
