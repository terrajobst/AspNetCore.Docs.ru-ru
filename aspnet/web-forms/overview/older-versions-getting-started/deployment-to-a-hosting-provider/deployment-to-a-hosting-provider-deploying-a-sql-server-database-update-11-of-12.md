---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 'Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: развертывание обновления базы данных сервера SQL - 11 12 | Документация Майкрософт'
author: tdykstra
description: Этой серии руководств показано, как развернуть (публикации) ASP.NET проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: 2018b58207365a0a3829f1f359d46d765aad0a77
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836965"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a><span data-ttu-id="20d8d-103">Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: развертывание обновления базы данных сервера SQL - 11 12</span><span class="sxs-lookup"><span data-stu-id="20d8d-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a SQL Server Database Update - 11 of 12</span></span>
====================
<span data-ttu-id="20d8d-104">по [том Дайкстра](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="20d8d-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="20d8d-105">Загрузите начальный проект</span><span class="sxs-lookup"><span data-stu-id="20d8d-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="20d8d-106">Этой серии руководств показано, как развернуть (публикации) ASP.NET проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Studio 2012 RC или Visual Studio Express 2012 RC для Web.</span><span class="sxs-lookup"><span data-stu-id="20d8d-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="20d8d-107">Также можно использовать Visual Studio 2010, если установить обновление веб-публикации.</span><span class="sxs-lookup"><span data-stu-id="20d8d-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="20d8d-108">Введение в серии, см. в разделе [в первом учебнике серии](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="20d8d-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="20d8d-109">Учебник, в котором показаны компоненты развертывания, появившиеся после выпуска версии-КАНДИДАТА Visual Studio 2012, содержит сведения о развертывании выпусков SQL Server, отличных от SQL Server Compact и содержит сведения о развертывании веб-сайтов Azure для Windows, см. в разделе [веб-развертывание ASP.NET с помощью Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="20d8d-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Windows Azure Web Sites, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="20d8d-110">Обзор</span><span class="sxs-lookup"><span data-stu-id="20d8d-110">Overview</span></span>

<span data-ttu-id="20d8d-111">Этом руководстве показано, как развернуть обновление базы данных для всей базы данных SQL Server.</span><span class="sxs-lookup"><span data-stu-id="20d8d-111">This tutorial shows you how to deploy a database update to a full SQL Server database.</span></span> <span data-ttu-id="20d8d-112">Поскольку Code First Migrations выполняет всю работу по обновления базы данных, процесс идентичен почти что вы сделали для SQL Server Compact, в [развертывание обновления базы данных](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) руководства.</span><span class="sxs-lookup"><span data-stu-id="20d8d-112">Because Code First Migrations does all the work of updating the database, the process is almost identical to what you did for SQL Server Compact in the [Deploying a Database Update](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) tutorial.</span></span>

<span data-ttu-id="20d8d-113">Напоминание: Если что-то не работает, как работать с учебником, вы получаете сообщение об ошибке, не забудьте установить флажок [страница устранения неполадок](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="20d8d-113">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="adding-a-new-column-to-a-table"></a><span data-ttu-id="20d8d-114">Добавление нового столбца в таблицу</span><span class="sxs-lookup"><span data-stu-id="20d8d-114">Adding a New Column to a Table</span></span>

<span data-ttu-id="20d8d-115">В этом разделе руководства вносятся изменения базы данных и соответствующие изменения кода, затем проверить их в Visual Studio при подготовке к их развертывание в тестовой и рабочей средах.</span><span class="sxs-lookup"><span data-stu-id="20d8d-115">In this section of the tutorial you'll make a database change and corresponding code changes, then test them in Visual Studio in preparation for deploying them to the test and production environments.</span></span> <span data-ttu-id="20d8d-116">Изменение включает в себя добавление `OfficeHours` столбец `Instructor` сущности и отображение на новые сведения **преподавателей** веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="20d8d-116">The change involves adding an `OfficeHours` column to the `Instructor` entity and displaying the new information in the **Instructors** web page.</span></span>

<span data-ttu-id="20d8d-117">В проекте ContosoUniversity.DAL, откройте *Instructor.cs* и добавьте следующее свойство между `HireDate` и `Courses` свойства:</span><span class="sxs-lookup"><span data-stu-id="20d8d-117">In the ContosoUniversity.DAL project, open *Instructor.cs* and add the following property between the `HireDate` and `Courses` properties:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

<span data-ttu-id="20d8d-118">Обновите класс инициализатора, таким образом, чтобы он заполняет новый столбец с тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="20d8d-118">Update the initializer class so that it seeds the new column with test data.</span></span> <span data-ttu-id="20d8d-119">Откройте *Migrations\Configuration.cs* и замените блок кода, который начинается `var instructors = new List<Instructor>` с приведенным ниже кодом, включая новый столбец:</span><span class="sxs-lookup"><span data-stu-id="20d8d-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes the new column:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

<span data-ttu-id="20d8d-120">В проекте "ContosoUniversity", откройте *Instructors.aspx* и добавьте новое поле шаблона для часов office непосредственно перед закрывающим `</Columns>` тег в первом `GridView` управления:</span><span class="sxs-lookup"><span data-stu-id="20d8d-120">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field for office hours just before the closing `</Columns>` tag in the first `GridView` control:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

<span data-ttu-id="20d8d-121">Постройте решение.</span><span class="sxs-lookup"><span data-stu-id="20d8d-121">Build the solution.</span></span>

<span data-ttu-id="20d8d-122">Откройте **консоль диспетчера пакетов** окна, а также выберите ContosoUniversity.DAL как **проект по умолчанию**.</span><span class="sxs-lookup"><span data-stu-id="20d8d-122">Open the **Package Manager Console** window, and select ContosoUniversity.DAL as the **Default project**.</span></span>

<span data-ttu-id="20d8d-123">Введите следующие команды.</span><span class="sxs-lookup"><span data-stu-id="20d8d-123">Enter the following commands:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

<span data-ttu-id="20d8d-124">Запустите приложение и выберите **преподавателей** страницы.</span><span class="sxs-lookup"><span data-stu-id="20d8d-124">Run the application and select the **Instructors** page.</span></span> <span data-ttu-id="20d8d-125">На странице используется немного больше времени, чем обычно, чтобы загрузить, так как платформа Entity Framework повторно создает базу данных и заполняет ее тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="20d8d-125">The page takes a little longer than usual to load, because the Entity Framework re-creates the database and seeds it with test data.</span></span>

<span data-ttu-id="20d8d-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="20d8d-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span></span>

## <a name="deploying-the-database-update-to-the-test-environment"></a><span data-ttu-id="20d8d-127">Развертывание обновления базы данных в тестовую среду</span><span class="sxs-lookup"><span data-stu-id="20d8d-127">Deploying the Database Update to the Test Environment</span></span>

<span data-ttu-id="20d8d-128">При использовании Code First Migrations, способ развертывания изменений базы данных в SQL Server такое же, как SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="20d8d-128">When you use Code First Migrations, the method for deploying a database change to SQL Server is the same as for SQL Server Compact.</span></span> <span data-ttu-id="20d8d-129">Тем не менее, вам нужно изменить профиль публикации, так как он по-прежнему настроен для переноса из SQL Server Compact в SQL Server.</span><span class="sxs-lookup"><span data-stu-id="20d8d-129">However, you have to change the Test publish profile because it is still set up to migrate from SQL Server Compact to SQL Server.</span></span>

<span data-ttu-id="20d8d-130">Первым шагом является удаление преобразования строки подключения, которые вы создали в предыдущем учебном курсе.</span><span class="sxs-lookup"><span data-stu-id="20d8d-130">The first step is to remove the connection string transformations that you created in the previous tutorial.</span></span> <span data-ttu-id="20d8d-131">Это больше не требуется, так как вы зададите преобразований строк подключения в профиле публикации, как это делалось, прежде чем вы настроили **упаковка и публикация SQL** tab для перехода на SQL Server.</span><span class="sxs-lookup"><span data-stu-id="20d8d-131">These are no longer needed because you'll specify connection string transformations in the publish profile, as you did before you configured the **Package/Publish SQL** tab for migration to SQL Server.</span></span>

<span data-ttu-id="20d8d-132">Откройте *Web.Test.config* файл и удалите `connectionStrings` элемент.</span><span class="sxs-lookup"><span data-stu-id="20d8d-132">Open the *Web.Test.config* file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="20d8d-133">Единственное преобразование оставшиеся в *Web.Test.config* файл `Environment` значение в `appSettings` элемент.</span><span class="sxs-lookup"><span data-stu-id="20d8d-133">The only remaining transformation in the *Web.Test.config* file is for the `Environment` value in the `appSettings` element.</span></span>

<span data-ttu-id="20d8d-134">Теперь можно обновить профиль публикации и опубликовать в тестовую среду.</span><span class="sxs-lookup"><span data-stu-id="20d8d-134">Now you can update the publish profile and publish to the test environment.</span></span>

<span data-ttu-id="20d8d-135">Откройте **веб-публикации** мастера, а затем переключиться в режим **профиль** вкладки.</span><span class="sxs-lookup"><span data-stu-id="20d8d-135">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="20d8d-136">Выберите **теста** профиль публикации.</span><span class="sxs-lookup"><span data-stu-id="20d8d-136">Select the **Test** publish profile.</span></span>

<span data-ttu-id="20d8d-137">Выберите **параметры** вкладки.</span><span class="sxs-lookup"><span data-stu-id="20d8d-137">Select the **Settings** tab.</span></span>

<span data-ttu-id="20d8d-138">Нажмите кнопку **разрешить улучшения публикации новой базы данных**.</span><span class="sxs-lookup"><span data-stu-id="20d8d-138">Click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="20d8d-139">В поле Строка подключения для **SchoolContext**, то же самое значение, выбранное на *Web.Test.config* файл преобразования в предыдущем руководстве:</span><span class="sxs-lookup"><span data-stu-id="20d8d-139">In the connection string box for **SchoolContext**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

<span data-ttu-id="20d8d-140">Выберите **выполнить Code First Migrations (при запуске приложения)**.</span><span class="sxs-lookup"><span data-stu-id="20d8d-140">Select **Execute Code First Migrations (runs on application start)**.</span></span> <span data-ttu-id="20d8d-141">(В вашей версии Visual Studio, может быть флажок **применить Code First Migrations**.)</span><span class="sxs-lookup"><span data-stu-id="20d8d-141">(In your version of Visual Studio, the check box might be labeled **Apply Code First Migrations**.)</span></span>

<span data-ttu-id="20d8d-142">В поле Строка подключения для **DefaultConnection**, то же самое значение, выбранное на *Web.Test.config* файл преобразования в предыдущем руководстве:</span><span class="sxs-lookup"><span data-stu-id="20d8d-142">In the connection string box for **DefaultConnection**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

<span data-ttu-id="20d8d-143">Оставьте **обновления базы данных** очищен.</span><span class="sxs-lookup"><span data-stu-id="20d8d-143">Leave **Update database** cleared.</span></span>

<span data-ttu-id="20d8d-144">Нажмите кнопку **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="20d8d-144">Click **Publish**.</span></span>

<span data-ttu-id="20d8d-145">Visual Studio развертывает изменения в коде в тестовую среду и открывает в браузере домашней страницей университета Contoso.</span><span class="sxs-lookup"><span data-stu-id="20d8d-145">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="20d8d-146">Выберите страницу преподавателей.</span><span class="sxs-lookup"><span data-stu-id="20d8d-146">Select the Instructors page.</span></span>

<span data-ttu-id="20d8d-147">При выполнении приложения эту страницу, он пытается получить доступ к базе данных.</span><span class="sxs-lookup"><span data-stu-id="20d8d-147">When the application runs this page, it tries to access the database.</span></span> <span data-ttu-id="20d8d-148">Code First Migrations проверяет, является текущей базы данных и обнаруживает, что пока еще не примененные последней миграции.</span><span class="sxs-lookup"><span data-stu-id="20d8d-148">Code First Migrations checks if the database is current, and finds that the latest migration has not been applied yet.</span></span> <span data-ttu-id="20d8d-149">Code First Migrations применяется последней миграции, запускает `Seed` метод, а затем страница выполняется в обычном режиме.</span><span class="sxs-lookup"><span data-stu-id="20d8d-149">Code First Migrations applies the latest migration, runs the `Seed` method, and then the page runs normally.</span></span> <span data-ttu-id="20d8d-150">Появится новый столбец часов Office с помощью задания начальных значений данных.</span><span class="sxs-lookup"><span data-stu-id="20d8d-150">You see the new Office Hours column with the seeded data.</span></span>

<span data-ttu-id="20d8d-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="20d8d-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span></span>

## <a name="deploying-the-database-update-to-the-production-environment"></a><span data-ttu-id="20d8d-152">Развертывание обновления базы данных в рабочей среде</span><span class="sxs-lookup"><span data-stu-id="20d8d-152">Deploying the Database Update to the Production Environment</span></span>

<span data-ttu-id="20d8d-153">Необходимо также изменить профиль публикации для рабочей среды.</span><span class="sxs-lookup"><span data-stu-id="20d8d-153">You have to change the publish profile for the production environment also.</span></span> <span data-ttu-id="20d8d-154">В этом случае вы удалите существующий и создайте новую, импортировав обновленную PUBLISHSETTINGS-файла.</span><span class="sxs-lookup"><span data-stu-id="20d8d-154">In this case you'll remove the existing profile and create a new one by importing an updated .publishsettings file.</span></span> <span data-ttu-id="20d8d-155">Обновленный файл будет включать строку подключения для базы данных SQL Server Cytanium.</span><span class="sxs-lookup"><span data-stu-id="20d8d-155">The updated file will include the connection string for the SQL Server database at Cytanium.</span></span>

<span data-ttu-id="20d8d-156">Как можно было увидеть при развертывании в тестовую среду, вы больше не нужен преобразований строки подключения в *Web.Production.config* файл преобразования.</span><span class="sxs-lookup"><span data-stu-id="20d8d-156">As you saw when you deployed to the test environment, you no longer need connection string transforms in the *Web.Production.config* transformation file.</span></span> <span data-ttu-id="20d8d-157">Открыть этот файл и удалите `connectionStrings` элемент.</span><span class="sxs-lookup"><span data-stu-id="20d8d-157">Open that file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="20d8d-158">Остальные преобразования предназначены для `Environment` значение в `appSettings` элемент и `location` элемент, который ограничивает доступ к отчетам об ошибках Elmah.</span><span class="sxs-lookup"><span data-stu-id="20d8d-158">The remaining transformations are for the `Environment` value in the `appSettings` element and the `location` element that restricts access to Elmah error reports.</span></span>

<span data-ttu-id="20d8d-159">Перед созданием нового профиля публикации для рабочей среды, загрузить обновленные PUBLISHSETTINGS-файла так же, как ранее в [развертывание в рабочей среде](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) руководства.</span><span class="sxs-lookup"><span data-stu-id="20d8d-159">Before you create a new publish profile for production, download an updated .publishsettings file the same way you did earlier in the [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial.</span></span> <span data-ttu-id="20d8d-160">(В панели управления Cytanium щелкните **веб-сайтов**, а затем нажмите кнопку **contosouniversity.com** веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="20d8d-160">(In the Cytanium control panel, click **Web Sites**, and then click the **contosouniversity.com** website.</span></span> <span data-ttu-id="20d8d-161">Выберите **веб-публикаций** , а затем щелкните **загрузить профиль публикации веб-узла**.) Причина, по которой вы выполняете действия — Выбор строку подключения базы данных в файле PUBLISHSETTINGS.</span><span class="sxs-lookup"><span data-stu-id="20d8d-161">Select the **Web Publishing** tab, and then click **Download Publishing Profile for this web site**.) The reason you are doing this is to pick up the database connection string in the .publishsettings file.</span></span> <span data-ttu-id="20d8d-162">Первый раз, загруженный файл, так как используется SQL Server Compact и еще не создана база данных SQL Server в Cytanium еще был недоступен в строке подключения.</span><span class="sxs-lookup"><span data-stu-id="20d8d-162">The connection string wasn't available the first time you downloaded the file, because you were still using SQL Server Compact and hadn't created the SQL Server database at Cytanium yet.</span></span>

<span data-ttu-id="20d8d-163">Теперь можно обновить профиль публикации и публикации в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="20d8d-163">Now you can update the publish profile and publish to the production environment.</span></span>

<span data-ttu-id="20d8d-164">Откройте **веб-публикации** мастера, а затем переключиться в режим **профиль** вкладки.</span><span class="sxs-lookup"><span data-stu-id="20d8d-164">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="20d8d-165">Нажмите кнопку **Управление профилями**и удалите профиль производства.</span><span class="sxs-lookup"><span data-stu-id="20d8d-165">Click **Manage Profiles**, and then delete the Production profile.</span></span>

<span data-ttu-id="20d8d-166">Закрыть **веб-публикации** мастер должен сохранить это изменение.</span><span class="sxs-lookup"><span data-stu-id="20d8d-166">Close the **Publish Web** wizard to save this change.</span></span>

<span data-ttu-id="20d8d-167">Откройте **веб-публикации** мастер еще раз, а затем нажмите кнопку **импорта**.</span><span class="sxs-lookup"><span data-stu-id="20d8d-167">Open the **Publish Web** wizard again, and then click **Import**.</span></span>

<span data-ttu-id="20d8d-168">На **подключения** вкладке **URL-адрес назначения** с соответствующим значением, если вы используете временный URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="20d8d-168">On the **Connection** tab, change **Destination URL** to the appropriate value if you are using a temporary URL.</span></span>

<span data-ttu-id="20d8d-169">Нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="20d8d-169">Click **Next**.</span></span>

<span data-ttu-id="20d8d-170">На **параметры** щелкните **разрешить улучшения публикации новой базы данных**.</span><span class="sxs-lookup"><span data-stu-id="20d8d-170">On the **Settings** tab, click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="20d8d-171">В раскрывающемся списке строку подключения для **SchoolContext**, выберите строку подключения Cytanium.</span><span class="sxs-lookup"><span data-stu-id="20d8d-171">In the connection string drop-down list for **SchoolContext**, select the Cytanium connection string.</span></span>

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

<span data-ttu-id="20d8d-173">Выберите **выполнить Code First migrations (при запуске приложения)**.</span><span class="sxs-lookup"><span data-stu-id="20d8d-173">Select **Execute Code First migrations (runs on application start)**.</span></span>

<span data-ttu-id="20d8d-174">В раскрывающемся списке строку подключения для **DefaultConnection**, выберите строку подключения Cytanium.</span><span class="sxs-lookup"><span data-stu-id="20d8d-174">In the connection string drop-down list for **DefaultConnection**, select the Cytanium connection string.</span></span>

<span data-ttu-id="20d8d-175">Выберите **профиль** щелкните **Управление профилями**и переименуйте профиль из «contosouniversity.com - веб-развертывание» в «Production».</span><span class="sxs-lookup"><span data-stu-id="20d8d-175">Select the **Profile** tab, click **Manage Profiles**, and rename the profile from "contosouniversity.com - Web Deploy" to "Production".</span></span>

<span data-ttu-id="20d8d-176">Закройте профиль публикации, чтобы сохранить изменения, а затем открыть его снова.</span><span class="sxs-lookup"><span data-stu-id="20d8d-176">Close the publish profile to save the change, then open it again.</span></span>

<span data-ttu-id="20d8d-177">Нажмите кнопку **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="20d8d-177">Click **Publish**.</span></span> <span data-ttu-id="20d8d-178">(Для реальной рабочей веб-сайта, необходимо скопировать *приложения\_offline.htm* в рабочей среде и поместить его в папку проекта, перед публикацией, удалите его после завершения развертывания.)</span><span class="sxs-lookup"><span data-stu-id="20d8d-178">(For a real production website, you would copy *app\_offline.htm* to production and put it in your project folder before publishing, then remove it when deployment is complete.)</span></span>

<span data-ttu-id="20d8d-179">Visual Studio развертывает изменения в коде в тестовую среду и открывает в браузере домашней страницей университета Contoso.</span><span class="sxs-lookup"><span data-stu-id="20d8d-179">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="20d8d-180">Выберите страницу преподавателей.</span><span class="sxs-lookup"><span data-stu-id="20d8d-180">Select the Instructors page.</span></span>

<span data-ttu-id="20d8d-181">Code First Migrations обновляет базу данных так же, как и в тестовой среде.</span><span class="sxs-lookup"><span data-stu-id="20d8d-181">Code First Migrations updates the database the same way it did in the Test environment.</span></span> <span data-ttu-id="20d8d-182">Появится новый столбец часов Office с помощью задания начальных значений данных.</span><span class="sxs-lookup"><span data-stu-id="20d8d-182">You see the new Office Hours column with the seeded data.</span></span>

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

<span data-ttu-id="20d8d-184">Вы успешно развернули обновления приложения, включены изменения базы данных, база данных SQL Server.</span><span class="sxs-lookup"><span data-stu-id="20d8d-184">You have now successfully deployed an application update that included a database change, using a SQL Server database.</span></span>

## <a name="more-information"></a><span data-ttu-id="20d8d-185">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="20d8d-185">More Information</span></span>

<span data-ttu-id="20d8d-186">На этом завершается этой серии руководств по развертыванию веб-приложению ASP.NET для стороннего поставщика услуг размещения.</span><span class="sxs-lookup"><span data-stu-id="20d8d-186">This completes this series of tutorials on deploying an ASP.NET web application to a third-party hosting provider.</span></span> <span data-ttu-id="20d8d-187">Дополнительные сведения о всех подразделах, рассматриваемых в этих учебниках, см. в разделе [Карта содержимого развертывания ASP.NET](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) веб-сайте MSDN.</span><span class="sxs-lookup"><span data-stu-id="20d8d-187">For more information about any of the topics covered in these tutorials, see the [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) on the MSDN web site.</span></span>

## <a name="acknowledgements"></a><span data-ttu-id="20d8d-188">Благодарности</span><span class="sxs-lookup"><span data-stu-id="20d8d-188">Acknowledgements</span></span>

<span data-ttu-id="20d8d-189">Я бы хотел поблагодарить следующих специалистов, которые разработали вклад в содержимое этой серии руководств:</span><span class="sxs-lookup"><span data-stu-id="20d8d-189">I would like to thank the following people who made significant contributions to the content of this tutorial series:</span></span>

- [<span data-ttu-id="20d8d-190">Альберто Poblacion, обладатель звания MVP &amp; MCT, Испания</span><span class="sxs-lookup"><span data-stu-id="20d8d-190">Alberto Poblacion, MVP &amp; MCT, Spain</span></span>](https://mvp.support.microsoft.com/profile/Alberto)
- <span data-ttu-id="20d8d-191">Фергюсон Jarod, MVP разработки платформы данных, США</span><span class="sxs-lookup"><span data-stu-id="20d8d-191">Jarod Ferguson, Data Platform Development MVP, United States</span></span>
- <span data-ttu-id="20d8d-192">Harsh Миттал, Microsoft</span><span class="sxs-lookup"><span data-stu-id="20d8d-192">Harsh Mittal, Microsoft</span></span>
- [<span data-ttu-id="20d8d-193">Олсон Kristina, Microsoft</span><span class="sxs-lookup"><span data-stu-id="20d8d-193">Kristina Olson, Microsoft</span></span>](https://blogs.iis.net/krolson/default.aspx)
- [<span data-ttu-id="20d8d-194">Майк эти протоколы, Microsoft</span><span class="sxs-lookup"><span data-stu-id="20d8d-194">Mike Pope, Microsoft</span></span>](http://www.mikepope.com/blog/DisplayBlog.aspx)
- <span data-ttu-id="20d8d-195">Мохи Srivastava, Microsoft</span><span class="sxs-lookup"><span data-stu-id="20d8d-195">Mohit Srivastava, Microsoft</span></span>
- [<span data-ttu-id="20d8d-196">Raffaele Rialdi, Италия</span><span class="sxs-lookup"><span data-stu-id="20d8d-196">Raffaele Rialdi, Italy</span></span>](http://www.iamraf.net/)
- [<span data-ttu-id="20d8d-197">Рик Андерсон, Microsoft</span><span class="sxs-lookup"><span data-stu-id="20d8d-197">Rick Anderson, Microsoft</span></span>](https://blogs.msdn.com/b/rickandy/)
- <span data-ttu-id="20d8d-198">[Саид Хашими, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))</span><span class="sxs-lookup"><span data-stu-id="20d8d-198">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))</span></span>
- <span data-ttu-id="20d8d-199">[Скотт Хансельман](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))</span><span class="sxs-lookup"><span data-stu-id="20d8d-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))</span></span>
- <span data-ttu-id="20d8d-200">[Скотт Хантер, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))</span><span class="sxs-lookup"><span data-stu-id="20d8d-200">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))</span></span>
- [<span data-ttu-id="20d8d-201">Srđan Božović, Сербия</span><span class="sxs-lookup"><span data-stu-id="20d8d-201">Srđan Božović, Serbia</span></span>](http://msforge.net/blogs/zmajcek/)
- <span data-ttu-id="20d8d-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))</span><span class="sxs-lookup"><span data-stu-id="20d8d-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="20d8d-203">[Назад](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [Вперед](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="20d8d-203">[Previous](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
[Next](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span></span>
