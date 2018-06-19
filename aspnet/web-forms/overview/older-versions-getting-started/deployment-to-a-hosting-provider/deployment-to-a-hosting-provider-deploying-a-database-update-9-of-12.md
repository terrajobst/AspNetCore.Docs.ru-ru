---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: 'Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: развертывание обновления базы данных - 9 12 | Документы Microsoft'
author: tdykstra
description: Ряд учебниках показано развертывание ASP.NET (публикации) проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: 0a00f9d3ed284ebbc1d83c1b5696436e5ba00f4b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30884890"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a><span data-ttu-id="44d67-103">Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: развертывание обновления базы данных - 9, 12</span><span class="sxs-lookup"><span data-stu-id="44d67-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a Database Update - 9 of 12</span></span>
====================
<span data-ttu-id="44d67-104">по [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="44d67-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="44d67-105">Загрузите начальный проект</span><span class="sxs-lookup"><span data-stu-id="44d67-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="44d67-106">Ряд учебниках показано развертывание ASP.NET (публикации) проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Studio 2012 RC или Visual Studio Express 2012 RC для Web.</span><span class="sxs-lookup"><span data-stu-id="44d67-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="44d67-107">Также можно использовать Visual Studio 2010, при установке обновления публикации Web.</span><span class="sxs-lookup"><span data-stu-id="44d67-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="44d67-108">Введение ряда см. в разделе [в первом учебнике ряда](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="44d67-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="44d67-109">Учебник, в котором показаны возможности развертывания, появившиеся после выпуска версии-КАНДИДАТА Visual Studio 2012, показано, как развернуть выпусках SQL Server, отличных от SQL Server Compact и показано, как для развертывания на веб-приложениях службы приложений Azure, в разделе [развертывания веб-приложения ASP.NET с помощью Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="44d67-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Azure App Service Web Apps, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="44d67-110">Обзор</span><span class="sxs-lookup"><span data-stu-id="44d67-110">Overview</span></span>

<span data-ttu-id="44d67-111">В этом учебнике внести изменение базы данных или изменение связанного кода, тестирования изменений в Visual Studio, а затем развернуть обновление для тестирования и в производственной среде.</span><span class="sxs-lookup"><span data-stu-id="44d67-111">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to both the test and production environments.</span></span>

<span data-ttu-id="44d67-112">Напоминание: Если вы получаете сообщение об ошибке или что-то не работает, как пройти учебник, не забудьте проверить [страницу устранения неполадок](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="44d67-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="adding-a-new-column-to-a-table"></a><span data-ttu-id="44d67-113">Добавление нового столбца в таблицу</span><span class="sxs-lookup"><span data-stu-id="44d67-113">Adding a New Column to a Table</span></span>

<span data-ttu-id="44d67-114">В этом разделе добавьте столбец даты рождения для `Person` базовый класс для `Student` и `Instructor` сущности.</span><span class="sxs-lookup"><span data-stu-id="44d67-114">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="44d67-115">Обновите страницу, отображающую инструктора данных, чтобы он отображал нового столбца.</span><span class="sxs-lookup"><span data-stu-id="44d67-115">Then you update the page that displays instructor data so that it displays the new column.</span></span>

<span data-ttu-id="44d67-116">В *ContosoUniversity.DAL* откройте проект *Person.cs* и добавьте следующее свойство в конце `Person` класса (должно быть две фигурные скобки после его закрытия):</span><span class="sxs-lookup"><span data-stu-id="44d67-116">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

<span data-ttu-id="44d67-117">Затем обновите Seed-метод, чтобы он предоставляет значение для нового столбца.</span><span class="sxs-lookup"><span data-stu-id="44d67-117">Next, update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="44d67-118">Откройте *Migrations\Configuration.cs* и заменить блок кода, который начинается `var instructors = new List<Instructor>` с следующий блок кода, который содержит информацию о дате рождения:</span><span class="sxs-lookup"><span data-stu-id="44d67-118">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

<span data-ttu-id="44d67-119">В проекте ContosoUniversity откройте *Instructors.aspx* и добавить новое поле шаблон для отображения даты рождения.</span><span class="sxs-lookup"><span data-stu-id="44d67-119">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="44d67-120">Добавьте его от тех, для назначения найма даты и office:</span><span class="sxs-lookup"><span data-stu-id="44d67-120">Add it between the ones for hire date and office assignment:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

<span data-ttu-id="44d67-121">(Если отступа кода получает синхронизированы, можно нажать сочетание клавиш CTRL-K и CTRL + D автоматическое переформатирование файл.)</span><span class="sxs-lookup"><span data-stu-id="44d67-121">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>

<span data-ttu-id="44d67-122">Постройте решение, а затем откройте **консоль диспетчера пакетов** окна.</span><span class="sxs-lookup"><span data-stu-id="44d67-122">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="44d67-123">Убедитесь, что ContosoUniversity.DAL по-прежнему выбран в качестве **проекта по умолчанию**.</span><span class="sxs-lookup"><span data-stu-id="44d67-123">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>

<span data-ttu-id="44d67-124">В **консоль диспетчера пакетов** выберите **ContosoUniversity.DAL** как **проекта по умолчанию**, а затем введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="44d67-124">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

<span data-ttu-id="44d67-125">По завершении этой команды Visual Studio открывает файл класса, который определяет новый `DbMIgration` класса и в `Up` метода можно просмотреть код, который создает новый столбец.</span><span class="sxs-lookup"><span data-stu-id="44d67-125">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` class, and in the `Up` method you can see the code that creates the new column.</span></span>

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

<span data-ttu-id="44d67-127">Постройте решение, а затем введите следующую команду в **консоль диспетчера пакетов** окна (Убедитесь, что по-прежнему выбран проект ContosoUniversity.DAL):</span><span class="sxs-lookup"><span data-stu-id="44d67-127">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

<span data-ttu-id="44d67-128">После завершения команды, запустите приложение и выберите страницу инструкторов.</span><span class="sxs-lookup"><span data-stu-id="44d67-128">When the command finishes, run the application and select the Instructors page.</span></span> <span data-ttu-id="44d67-129">При загрузке страницы, вы видите, что она имеет новый поля «Дата рождения».</span><span class="sxs-lookup"><span data-stu-id="44d67-129">When the page loads, you see that it has the new birth date field.</span></span>

<span data-ttu-id="44d67-130">[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="44d67-130">[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)</span></span>

## <a name="deploying-the-database-update-to-the-test-environment"></a><span data-ttu-id="44d67-131">Развертывание обновления базы данных в тестовой среде</span><span class="sxs-lookup"><span data-stu-id="44d67-131">Deploying the Database Update to the Test Environment</span></span>

<span data-ttu-id="44d67-132">В **обозревателе решений** выберите ContosoUniversity проект.</span><span class="sxs-lookup"><span data-stu-id="44d67-132">In **Solution Explorer** select the ContosoUniversity project.</span></span>

<span data-ttu-id="44d67-133">В **веб-публикация одним щелкните** панели инструментов выберите **тест** профиль публикации, а затем нажмите кнопку **веб-публикация**.</span><span class="sxs-lookup"><span data-stu-id="44d67-133">In the **Web One Click Publish** toolbar, select the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="44d67-134">(Если отключена панели инструментов, выберите проект ContosoUniversity в **обозревателе решений**.)</span><span class="sxs-lookup"><span data-stu-id="44d67-134">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

<span data-ttu-id="44d67-135">Visual Studio развертывает обновленное приложение, и в браузере будет открыта на домашнюю страницу.</span><span class="sxs-lookup"><span data-stu-id="44d67-135">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span> <span data-ttu-id="44d67-136">Запустите страницу инструкторов, чтобы убедиться, что обновление было успешно развернуто.</span><span class="sxs-lookup"><span data-stu-id="44d67-136">Run the Instructors page to verify that the update was successfully deployed.</span></span> <span data-ttu-id="44d67-137">Когда приложение пытается получить доступ к базе данных для этой страницы, Code First обновляет схему базы данных и запускает `Seed` метод.</span><span class="sxs-lookup"><span data-stu-id="44d67-137">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="44d67-138">При отображении страницы, вы видите ожидаемых **Дата рождения** столбец с датами в нем.</span><span class="sxs-lookup"><span data-stu-id="44d67-138">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>

<span data-ttu-id="44d67-139">[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="44d67-139">[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)</span></span>

## <a name="deploying-the-database-update-to-the-production-environment"></a><span data-ttu-id="44d67-140">Развертывание обновления базы данных в рабочей среде</span><span class="sxs-lookup"><span data-stu-id="44d67-140">Deploying the Database Update to the Production Environment</span></span>

<span data-ttu-id="44d67-141">Теперь можно развернуть в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="44d67-141">You can now deploy to production.</span></span> <span data-ttu-id="44d67-142">Единственное отличие заключается в том, что будет использоваться *приложения\_offline.htm* для запрещения доступа к узлу и таким образом обновляет базу данных при развертывании изменений.</span><span class="sxs-lookup"><span data-stu-id="44d67-142">The only difference is that you'll use *app\_offline.htm* to prevent users from accessing the site and thus updating the database while you're deploying changes.</span></span> <span data-ttu-id="44d67-143">Для рабочего развертывания выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="44d67-143">For production deployment perform the following steps:</span></span>

- <span data-ttu-id="44d67-144">Отправка *приложения\_offline.htm* файла рабочего сайта.</span><span class="sxs-lookup"><span data-stu-id="44d67-144">Upload the *app\_offline.htm* file to the production site.</span></span>
- <span data-ttu-id="44d67-145">В Visual Studio, выберите профиль на производственных **веб-публикация одним щелкните** инструментов и выберите команду **веб-публикация**.</span><span class="sxs-lookup"><span data-stu-id="44d67-145">In Visual Studio, choose the Production profile in the **Web One Click Publish** toolbar and click **Publish Web**.</span></span>
- <span data-ttu-id="44d67-146">Удалить *приложения\_offline.htm* файла из рабочего сайта.</span><span class="sxs-lookup"><span data-stu-id="44d67-146">Delete the *app\_offline.htm* file from the production site.</span></span>

> [!NOTE]
> <span data-ttu-id="44d67-147">Приложение во время использования в рабочей среде следует реализация плана резервного копирования.</span><span class="sxs-lookup"><span data-stu-id="44d67-147">While your application is in use in the production environment you should be implementing a backup plan.</span></span> <span data-ttu-id="44d67-148">То есть, вы должны периодическое копирование *School-Prod.sdf* и *aspnet Prod.sdf* файлы с рабочего сайта безопасное хранилище, а должен указывать нескольких поколений таких Создание резервных копий.</span><span class="sxs-lookup"><span data-stu-id="44d67-148">That is, you should be periodically copying the *School-Prod.sdf* and *aspnet-Prod.sdf* files from the production site to a secure storage location, and you should be keeping several generations of such backups.</span></span> <span data-ttu-id="44d67-149">При обновлении базы данных, следует сделать резервную копию из непосредственно перед изменением.</span><span class="sxs-lookup"><span data-stu-id="44d67-149">When you update the database, you should make a backup copy from immediately before the change.</span></span> <span data-ttu-id="44d67-150">Затем Если допущена ошибка и не обнаружит его до, после его развертывания в рабочей среде, по-прежнему можно восстановление базы данных в состояние, в котором она находилась до его был поврежден.</span><span class="sxs-lookup"><span data-stu-id="44d67-150">Then, if you make a mistake and don't discover it until after you have deployed it to production, you will still be able to recover the database to the state it was in before it became corrupted.</span></span>


<span data-ttu-id="44d67-151">Когда Visual Studio открывает URL-адрес домашней страницы в браузере, *приложения\_offline.htm* -страница.</span><span class="sxs-lookup"><span data-stu-id="44d67-151">When Visual Studio opens the home page URL in the browser, the *app\_offline.htm* page is displayed.</span></span> <span data-ttu-id="44d67-152">После удаления *приложения\_offline.htm* файл, можно перейти на домашнюю страницу еще раз, чтобы проверить успешность развертывания обновления.</span><span class="sxs-lookup"><span data-stu-id="44d67-152">After you delete the *app\_offline.htm* file, you can browse to your home page again to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="44d67-153">[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="44d67-153">[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)</span></span>

<span data-ttu-id="44d67-154">Теперь, после развертывания обновления, включенные изменения базы данных для рабочего и тестового приложения.</span><span class="sxs-lookup"><span data-stu-id="44d67-154">You've now deployed an application update that included a database change to both test and production.</span></span> <span data-ttu-id="44d67-155">Далее учебнике показано, как перенести базу данных из SQL Server Compact в SQL Server Express и SQL Server.</span><span class="sxs-lookup"><span data-stu-id="44d67-155">The next tutorial shows you how to migrate your database from SQL Server Compact to SQL Server Express and SQL Server.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="44d67-156">[Назад](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [Вперед](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="44d67-156">[Previous](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
[Next](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)</span></span>
