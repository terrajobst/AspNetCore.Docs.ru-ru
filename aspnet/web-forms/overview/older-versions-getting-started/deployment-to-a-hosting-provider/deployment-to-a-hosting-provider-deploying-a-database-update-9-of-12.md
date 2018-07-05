---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: 'Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: развертывание обновления базы данных - 9, 12 | Документация Майкрософт'
author: tdykstra
description: Этой серии руководств показано, как развернуть (публикации) ASP.NET проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: e94281e36192c91a04392735af318bbc517b0521
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382463"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a><span data-ttu-id="50907-103">Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: развертывание обновления базы данных - 9, 12</span><span class="sxs-lookup"><span data-stu-id="50907-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a Database Update - 9 of 12</span></span>
====================
<span data-ttu-id="50907-104">по [том Дайкстра](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="50907-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="50907-105">Загрузите начальный проект</span><span class="sxs-lookup"><span data-stu-id="50907-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="50907-106">Этой серии руководств показано, как развернуть (публикации) ASP.NET проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Studio 2012 RC или Visual Studio Express 2012 RC для Web.</span><span class="sxs-lookup"><span data-stu-id="50907-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="50907-107">Также можно использовать Visual Studio 2010, если установить обновление веб-публикации.</span><span class="sxs-lookup"><span data-stu-id="50907-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="50907-108">Введение в серии, см. в разделе [в первом учебнике серии](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="50907-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="50907-109">Учебник, в котором показаны компоненты развертывания, появившиеся после выпуска версии-КАНДИДАТА Visual Studio 2012, содержит сведения о развертывании выпусков SQL Server, отличных от SQL Server Compact и содержит сведения о развертывании веб-приложения службы приложений Azure, см. в разделе [веб-развертывание ASP.NET с помощью Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="50907-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Azure App Service Web Apps, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="50907-110">Обзор</span><span class="sxs-lookup"><span data-stu-id="50907-110">Overview</span></span>

<span data-ttu-id="50907-111">В этом руководстве внесенные изменения базы данных и изменения кода, тестирования изменений в Visual Studio, а затем развернуть обновление в рабочей и тестовой среды.</span><span class="sxs-lookup"><span data-stu-id="50907-111">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to both the test and production environments.</span></span>

<span data-ttu-id="50907-112">Напоминание: Если что-то не работает, как работать с учебником, вы получаете сообщение об ошибке, не забудьте установить флажок [страница устранения неполадок](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="50907-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="adding-a-new-column-to-a-table"></a><span data-ttu-id="50907-113">Добавление нового столбца в таблицу</span><span class="sxs-lookup"><span data-stu-id="50907-113">Adding a New Column to a Table</span></span>

<span data-ttu-id="50907-114">В этом разделе, добавьте столбец даты рождения для `Person` базовый класс для `Student` и `Instructor` сущностей.</span><span class="sxs-lookup"><span data-stu-id="50907-114">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="50907-115">Затем вы обновите со страницей, отображающей данные instructor, чтобы он отображал нового столбца.</span><span class="sxs-lookup"><span data-stu-id="50907-115">Then you update the page that displays instructor data so that it displays the new column.</span></span>

<span data-ttu-id="50907-116">В *ContosoUniversity.DAL* откройте проект *Person.cs* и добавьте следующее свойство в конце `Person` класс (должно быть два закрывающей фигурных скобок после нее):</span><span class="sxs-lookup"><span data-stu-id="50907-116">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

<span data-ttu-id="50907-117">Затем обновите метод заполнения таким образом, чтобы он предоставлял значение нового столбца.</span><span class="sxs-lookup"><span data-stu-id="50907-117">Next, update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="50907-118">Откройте *Migrations\Configuration.cs* и замените блок кода, который начинается `var instructors = new List<Instructor>` с приведенным ниже кодом, который содержит информацию о дате рождения:</span><span class="sxs-lookup"><span data-stu-id="50907-118">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

<span data-ttu-id="50907-119">В проекте "ContosoUniversity", откройте *Instructors.aspx* и добавьте новое поле шаблона для отображения даты рождения.</span><span class="sxs-lookup"><span data-stu-id="50907-119">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="50907-120">Между для них назначения даты и office приема на работу, добавьте ее.</span><span class="sxs-lookup"><span data-stu-id="50907-120">Add it between the ones for hire date and office assignment:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

<span data-ttu-id="50907-121">(Если отступов кода получает синхронизированы, можно нажать сочетание клавиш CTRL-K и CTRL-D для автоматически переформатирует файл.)</span><span class="sxs-lookup"><span data-stu-id="50907-121">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>

<span data-ttu-id="50907-122">Выполните сборку решения, а затем откройте **консоль диспетчера пакетов** окна.</span><span class="sxs-lookup"><span data-stu-id="50907-122">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="50907-123">Убедитесь, что ContosoUniversity.DAL по-прежнему выбран в качестве **проект по умолчанию**.</span><span class="sxs-lookup"><span data-stu-id="50907-123">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>

<span data-ttu-id="50907-124">В **консоль диспетчера пакетов** выберите **ContosoUniversity.DAL** как **проект по умолчанию**, а затем введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="50907-124">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

<span data-ttu-id="50907-125">По завершении этой команды Visual Studio открывает файл класса, который определяет новый `DbMIgration` класса и в `Up` метода вы увидите код, создающий новый столбец.</span><span class="sxs-lookup"><span data-stu-id="50907-125">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` class, and in the `Up` method you can see the code that creates the new column.</span></span>

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

<span data-ttu-id="50907-127">Выполните сборку решения, а затем введите следующую команду в **консоль диспетчера пакетов** окна (Убедитесь, что проект ContosoUniversity.DAL по-прежнему выбран):</span><span class="sxs-lookup"><span data-stu-id="50907-127">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

<span data-ttu-id="50907-128">После завершения выполнения команды, запустите приложение и выберите страницу преподавателей.</span><span class="sxs-lookup"><span data-stu-id="50907-128">When the command finishes, run the application and select the Instructors page.</span></span> <span data-ttu-id="50907-129">При загрузке страницы, вы увидите, что у него есть новое поле даты рождения.</span><span class="sxs-lookup"><span data-stu-id="50907-129">When the page loads, you see that it has the new birth date field.</span></span>

<span data-ttu-id="50907-130">[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="50907-130">[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)</span></span>

## <a name="deploying-the-database-update-to-the-test-environment"></a><span data-ttu-id="50907-131">Развертывание обновления базы данных в тестовую среду</span><span class="sxs-lookup"><span data-stu-id="50907-131">Deploying the Database Update to the Test Environment</span></span>

<span data-ttu-id="50907-132">В **обозревателе решений** выберите проект ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="50907-132">In **Solution Explorer** select the ContosoUniversity project.</span></span>

<span data-ttu-id="50907-133">В **веб-публикация одним щелкните** панели инструментов, выберите **теста** профиль публикации, а затем нажмите кнопку **веб-публикации**.</span><span class="sxs-lookup"><span data-stu-id="50907-133">In the **Web One Click Publish** toolbar, select the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="50907-134">(Если отключена панели инструментов, выберите проект ContosoUniversity в **обозревателе решений**.)</span><span class="sxs-lookup"><span data-stu-id="50907-134">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

<span data-ttu-id="50907-135">Visual Studio развертывает обновленное приложение и в браузере откроется домашняя страница.</span><span class="sxs-lookup"><span data-stu-id="50907-135">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span> <span data-ttu-id="50907-136">Откройте страницу преподавателей, чтобы убедиться, что обновление было успешно развернуто.</span><span class="sxs-lookup"><span data-stu-id="50907-136">Run the Instructors page to verify that the update was successfully deployed.</span></span> <span data-ttu-id="50907-137">Когда приложение пытается получить доступ к базе данных для этой страницы, Code First обновляет схему базы данных и запускает `Seed` метод.</span><span class="sxs-lookup"><span data-stu-id="50907-137">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="50907-138">Когда отобразится страница, вы увидите ожидаемый **Дата рождения** столбец с датами в нем.</span><span class="sxs-lookup"><span data-stu-id="50907-138">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>

<span data-ttu-id="50907-139">[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="50907-139">[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)</span></span>

## <a name="deploying-the-database-update-to-the-production-environment"></a><span data-ttu-id="50907-140">Развертывание обновления базы данных в рабочей среде</span><span class="sxs-lookup"><span data-stu-id="50907-140">Deploying the Database Update to the Production Environment</span></span>

<span data-ttu-id="50907-141">Теперь можно развернуть в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="50907-141">You can now deploy to production.</span></span> <span data-ttu-id="50907-142">Единственное отличие заключается в том, что вы используете *приложения\_offline.htm* чтобы запретить пользователям доступ к сайту и обновление базы данных таким образом, при развертывании изменений.</span><span class="sxs-lookup"><span data-stu-id="50907-142">The only difference is that you'll use *app\_offline.htm* to prevent users from accessing the site and thus updating the database while you're deploying changes.</span></span> <span data-ttu-id="50907-143">Для развертывания в рабочей среде выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="50907-143">For production deployment perform the following steps:</span></span>

- <span data-ttu-id="50907-144">Отправка *приложения\_offline.htm* файл к рабочему сайту.</span><span class="sxs-lookup"><span data-stu-id="50907-144">Upload the *app\_offline.htm* file to the production site.</span></span>
- <span data-ttu-id="50907-145">В Visual Studio, выберите профиль производства в **веб-публикация одним щелкните** панели инструментов и щелкните **веб-публикации**.</span><span class="sxs-lookup"><span data-stu-id="50907-145">In Visual Studio, choose the Production profile in the **Web One Click Publish** toolbar and click **Publish Web**.</span></span>
- <span data-ttu-id="50907-146">Удалить *приложения\_offline.htm* файл с рабочего сайта.</span><span class="sxs-lookup"><span data-stu-id="50907-146">Delete the *app\_offline.htm* file from the production site.</span></span>

> [!NOTE]
> <span data-ttu-id="50907-147">Когда приложение используется в рабочей среде следует реализация плана резервного копирования.</span><span class="sxs-lookup"><span data-stu-id="50907-147">While your application is in use in the production environment you should be implementing a backup plan.</span></span> <span data-ttu-id="50907-148">То есть вы должны периодически скопировать *School-Prod.sdf* и *aspnet Prod.sdf* файлы с рабочего сайта в место безопасного хранения, и должен сохранение нескольких поколений ОС, таких резервные копии.</span><span class="sxs-lookup"><span data-stu-id="50907-148">That is, you should be periodically copying the *School-Prod.sdf* and *aspnet-Prod.sdf* files from the production site to a secure storage location, and you should be keeping several generations of such backups.</span></span> <span data-ttu-id="50907-149">При обновлении базы данных, следует создайте резервную копию из непосредственно перед изменением.</span><span class="sxs-lookup"><span data-stu-id="50907-149">When you update the database, you should make a backup copy from immediately before the change.</span></span> <span data-ttu-id="50907-150">Затем Если вы сделаете ошибку и не обнаружит его до, после его развертывания в рабочей среде, по-прежнему можно восстановить базу данных к состоянию, в котором он находился перед его был поврежден.</span><span class="sxs-lookup"><span data-stu-id="50907-150">Then, if you make a mistake and don't discover it until after you have deployed it to production, you will still be able to recover the database to the state it was in before it became corrupted.</span></span>


<span data-ttu-id="50907-151">Когда Visual Studio открывает URL-адрес домашней страницы в браузере, *приложения\_offline.htm* откроется страница.</span><span class="sxs-lookup"><span data-stu-id="50907-151">When Visual Studio opens the home page URL in the browser, the *app\_offline.htm* page is displayed.</span></span> <span data-ttu-id="50907-152">После удаления *приложения\_offline.htm* файл, можно перейти к домашней странице еще раз, чтобы проверить, что обновление успешно развернут.</span><span class="sxs-lookup"><span data-stu-id="50907-152">After you delete the *app\_offline.htm* file, you can browse to your home page again to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="50907-153">[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="50907-153">[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)</span></span>

<span data-ttu-id="50907-154">Теперь вы развернули обновления приложения, включены изменения базы данных для тестирования и производственная среда.</span><span class="sxs-lookup"><span data-stu-id="50907-154">You've now deployed an application update that included a database change to both test and production.</span></span> <span data-ttu-id="50907-155">Следующее руководство показано, как перенести базу данных из SQL Server Compact в SQL Server Express и SQL Server.</span><span class="sxs-lookup"><span data-stu-id="50907-155">The next tutorial shows you how to migrate your database from SQL Server Compact to SQL Server Express and SQL Server.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="50907-156">[Назад](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [Вперед](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="50907-156">[Previous](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
[Next](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)</span></span>
