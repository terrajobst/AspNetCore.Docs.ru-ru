---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'Веб-развертывание ASP.NET с помощью Visual Studio: развертывание обновления базы данных | Документация Майкрософт'
author: tdykstra
description: В этой серии руководств показано, как развернуть ASP.NET (публикации) веб-приложения, веб-приложениях службы приложений Azure или у стороннего поставщика размещения, Пол...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 88652a33d5367241fec4d442e9deb15d88a096c9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817627"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a><span data-ttu-id="44acb-103">Веб-развертывание ASP.NET с помощью Visual Studio: развертывание обновления базы данных</span><span class="sxs-lookup"><span data-stu-id="44acb-103">ASP.NET Web Deployment using Visual Studio: Deploying a Database Update</span></span>
====================
<span data-ttu-id="44acb-104">по [том Дайкстра](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="44acb-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="44acb-105">Загрузите начальный проект</span><span class="sxs-lookup"><span data-stu-id="44acb-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="44acb-106">В этой серии руководств показано, как развернуть ASP.NET (публикации) веб-приложения для веб-приложениях службы приложений Azure или стороннего поставщика услуг размещения, с помощью Visual Studio 2012 или Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="44acb-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="44acb-107">Сведения об этой серии см. в разделе [в первом учебнике серии](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="44acb-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="44acb-108">Обзор</span><span class="sxs-lookup"><span data-stu-id="44acb-108">Overview</span></span>

<span data-ttu-id="44acb-109">В этом руководстве внесенные изменения базы данных и изменения кода, тестирования изменений в Visual Studio, а затем развернуть обновление в средах тестирования, промежуточной и рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="44acb-109">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to the test, staging, and production environments.</span></span>

<span data-ttu-id="44acb-110">В этом руководстве сначала рассматривается для обновления базы данных, которая управляется Code First Migrations и позже он показывает, как обновить базу данных, используя поставщик dbDacFx.</span><span class="sxs-lookup"><span data-stu-id="44acb-110">The tutorial first shows how to update a database that is managed by Code First Migrations, and then later it shows how to update a database by using the dbDacFx provider.</span></span>

<span data-ttu-id="44acb-111">Напоминание: Если что-то не работает, как работать с учебником, вы получаете сообщение об ошибке, не забудьте установить флажок [страница устранения неполадок](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="44acb-111">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a><span data-ttu-id="44acb-112">Развертывание обновления базы данных с помощью Code First Migrations</span><span class="sxs-lookup"><span data-stu-id="44acb-112">Deploy a database update by using Code First Migrations</span></span>

<span data-ttu-id="44acb-113">В этом разделе, добавьте столбец даты рождения для `Person` базовый класс для `Student` и `Instructor` сущностей.</span><span class="sxs-lookup"><span data-stu-id="44acb-113">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="44acb-114">Затем вы обновите со страницей, отображающей данные instructor, чтобы он отображал нового столбца.</span><span class="sxs-lookup"><span data-stu-id="44acb-114">Then you update the page that displays instructor data so that it displays the new column.</span></span> <span data-ttu-id="44acb-115">Наконец необходимо выполнить развертывание изменений для тестирования, промежуточной и рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="44acb-115">Finally, you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-application-database"></a><span data-ttu-id="44acb-116">Добавить столбец в таблицу в базе данных приложения</span><span class="sxs-lookup"><span data-stu-id="44acb-116">Add a column to a table in the application database</span></span>

1. <span data-ttu-id="44acb-117">В *ContosoUniversity.DAL* откройте проект *Person.cs* и добавьте следующее свойство в конце `Person` класс (должно быть два закрывающей фигурных скобок после нее):</span><span class="sxs-lookup"><span data-stu-id="44acb-117">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    <span data-ttu-id="44acb-118">Затем обновите `Seed` метода, так что он содержит значение для нового столбца.</span><span class="sxs-lookup"><span data-stu-id="44acb-118">Next, update the `Seed` method so that it provides a value for the new column.</span></span> <span data-ttu-id="44acb-119">Откройте *Migrations\Configuration.cs* и замените блок кода, который начинается `var instructors = new List<Instructor>` с приведенным ниже кодом, который содержит информацию о дате рождения:</span><span class="sxs-lookup"><span data-stu-id="44acb-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. <span data-ttu-id="44acb-120">Выполните сборку решения, а затем откройте **консоль диспетчера пакетов** окна.</span><span class="sxs-lookup"><span data-stu-id="44acb-120">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="44acb-121">Убедитесь, что ContosoUniversity.DAL по-прежнему выбран в качестве **проект по умолчанию**.</span><span class="sxs-lookup"><span data-stu-id="44acb-121">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>
3. <span data-ttu-id="44acb-122">В **консоль диспетчера пакетов** выберите **ContosoUniversity.DAL** как **проект по умолчанию**, а затем введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="44acb-122">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    <span data-ttu-id="44acb-123">По завершении этой команды Visual Studio открывает файл класса, который определяет новый `DbMIgration` класса и в `Up` метода вы увидите код, создающий новый столбец.</span><span class="sxs-lookup"><span data-stu-id="44acb-123">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` class, and in the `Up` method you can see the code that creates the new column.</span></span> <span data-ttu-id="44acb-124">`Up` Метод создает столбец при внесении изменений и `Down` метод удаляет столбец, когда выполняется откат изменений.</span><span class="sxs-lookup"><span data-stu-id="44acb-124">The `Up` method creates the column when you are implementing the change, and the `Down` method deletes the column when you are rolling back the change.</span></span>

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. <span data-ttu-id="44acb-126">Выполните сборку решения, а затем введите следующую команду в **консоль диспетчера пакетов** окна (Убедитесь, что проект ContosoUniversity.DAL по-прежнему выбран):</span><span class="sxs-lookup"><span data-stu-id="44acb-126">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    <span data-ttu-id="44acb-127">Платформа Entity Framework выполняет `Up` метод, а затем выполняет `Seed` метод.</span><span class="sxs-lookup"><span data-stu-id="44acb-127">The Entity Framework runs the `Up` method and then runs the `Seed` method.</span></span>

### <a name="display-the-new-column-in-the-instructors-page"></a><span data-ttu-id="44acb-128">Отображение нового столбца на странице преподавателей</span><span class="sxs-lookup"><span data-stu-id="44acb-128">Display the new column in the Instructors page</span></span>

1. <span data-ttu-id="44acb-129">В проекте "ContosoUniversity", откройте *Instructors.aspx* и добавьте новое поле шаблона для отображения даты рождения.</span><span class="sxs-lookup"><span data-stu-id="44acb-129">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="44acb-130">Между для них назначения даты и office приема на работу, добавьте ее.</span><span class="sxs-lookup"><span data-stu-id="44acb-130">Add it between the ones for hire date and office assignment:</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    <span data-ttu-id="44acb-131">(Если отступов кода получает синхронизированы, можно нажать сочетание клавиш CTRL-K и CTRL-D для автоматически переформатирует файл.)</span><span class="sxs-lookup"><span data-stu-id="44acb-131">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>
2. <span data-ttu-id="44acb-132">Запустите приложение и нажмите кнопку **преподавателей** ссылку.</span><span class="sxs-lookup"><span data-stu-id="44acb-132">Run the application and click the **Instructors** link.</span></span>

    <span data-ttu-id="44acb-133">При загрузке страницы, вы увидите, что у него есть новое поле даты рождения.</span><span class="sxs-lookup"><span data-stu-id="44acb-133">When the page loads, you see that it has the new birth date field.</span></span>

    ![Странице "Instructors" с даты рождения](deploying-a-database-update/_static/image2.png)
3. <span data-ttu-id="44acb-135">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="44acb-135">Close the browser.</span></span>

### <a name="deploy-the-database-update"></a><span data-ttu-id="44acb-136">Развертывание обновления базы данных</span><span class="sxs-lookup"><span data-stu-id="44acb-136">Deploy the database update</span></span>

1. <span data-ttu-id="44acb-137">В **обозревателе решений** выберите проект ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="44acb-137">In **Solution Explorer** select the ContosoUniversity project.</span></span>
2. <span data-ttu-id="44acb-138">В **веб-публикация одним щелкните** панели инструментов нажмите кнопку **теста** профиль публикации, а затем нажмите кнопку **веб-публикации**.</span><span class="sxs-lookup"><span data-stu-id="44acb-138">In the **Web One Click Publish** toolbar, click the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="44acb-139">(Если отключена панели инструментов, выберите проект ContosoUniversity в **обозревателе решений**.)</span><span class="sxs-lookup"><span data-stu-id="44acb-139">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

    <span data-ttu-id="44acb-140">Visual Studio развертывает обновленное приложение и в браузере откроется домашняя страница.</span><span class="sxs-lookup"><span data-stu-id="44acb-140">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
3. <span data-ttu-id="44acb-141">Запустите **преподавателей** страницу, чтобы убедиться, что обновление было успешно развернуто.</span><span class="sxs-lookup"><span data-stu-id="44acb-141">Run the **Instructors** page to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="44acb-142">Когда приложение пытается получить доступ к базе данных для этой страницы, Code First обновляет схему базы данных и запускает `Seed` метод.</span><span class="sxs-lookup"><span data-stu-id="44acb-142">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="44acb-143">Когда отобразится страница, вы увидите ожидаемый **Дата рождения** столбец с датами в нем.</span><span class="sxs-lookup"><span data-stu-id="44acb-143">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>
4. <span data-ttu-id="44acb-144">В **веб-публикация одним щелкните** панели инструментов нажмите кнопку **промежуточной** профиль публикации, а затем нажмите кнопку **веб-публикации**.</span><span class="sxs-lookup"><span data-stu-id="44acb-144">In the **Web One Click Publish** toolbar, click the **Staging** publish profile, and then click **Publish Web**.</span></span>
5. <span data-ttu-id="44acb-145">Запустите **преподавателей** страницы в промежуточной среде, чтобы убедиться, что обновление было успешно развернуто.</span><span class="sxs-lookup"><span data-stu-id="44acb-145">Run the **Instructors** page in staging to verify that the update was successfully deployed.</span></span>
6. <span data-ttu-id="44acb-146">В **веб-публикация одним щелкните** панели инструментов нажмите кнопку **рабочей** профиль публикации, а затем нажмите кнопку **веб-публикации**.</span><span class="sxs-lookup"><span data-stu-id="44acb-146">In the **Web One Click Publish** toolbar, click the **Production** publish profile, and then click **Publish Web**.</span></span>
7. <span data-ttu-id="44acb-147">Запустите **преподавателей** страницы в рабочей среде, чтобы убедиться, что обновление было успешно развернуто.</span><span class="sxs-lookup"><span data-stu-id="44acb-147">Run the **Instructors** page in production to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="44acb-148">Для обновления приложения реальной рабочей среде, который включает в себя изменение базы данных также обычно выполняемые приложения в автономный режим во время развертывания с помощью *приложения\_offline.htm*, как показано в предыдущем учебном курсе.</span><span class="sxs-lookup"><span data-stu-id="44acb-148">For a real production application update that includes a database change you would also typically take the application offline during deployment by using *app\_offline.htm*, as you saw in the previous tutorial.</span></span>

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a><span data-ttu-id="44acb-149">Развертывание обновления базы данных, используя поставщик dbDacFx</span><span class="sxs-lookup"><span data-stu-id="44acb-149">Deploy a database update by using the dbDacFx provider</span></span>

<span data-ttu-id="44acb-150">В этом разделе вы добавите *комментарии* столбец *пользователя* таблицы в базе данных членства и создать страницу, которая позволяет отображать и редактировать комментарии для каждого пользователя.</span><span class="sxs-lookup"><span data-stu-id="44acb-150">In this section, you add a *Comments* column to the *User* table in the membership database and create a page that lets you display and edit comments for each user.</span></span> <span data-ttu-id="44acb-151">Затем необходимо выполнить развертывание изменений для тестирования, промежуточной и рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="44acb-151">Then you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-membership-database"></a><span data-ttu-id="44acb-152">Добавить столбец в таблицу в базе данных членства</span><span class="sxs-lookup"><span data-stu-id="44acb-152">Add a column to a table in the membership database</span></span>

1. <span data-ttu-id="44acb-153">В Visual Studio откройте **обозреватель объектов SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="44acb-153">In Visual Studio, open **SQL Server Object Explorer**.</span></span>
2. <span data-ttu-id="44acb-154">Разверните **(localdb) \v11.0**, разверните **баз данных**, разверните **aspnet ContosoUniversity** (не **aspnet-ContosoUniversity-Prod**) а затем разверните **таблиц**.</span><span class="sxs-lookup"><span data-stu-id="44acb-154">Expand **(localdb)\v11.0**, expand **Databases**, expand **aspnet-ContosoUniversity** (not **aspnet-ContosoUniversity-Prod**) and then expand **Tables**.</span></span>

    <span data-ttu-id="44acb-155">Если вы не видите **(localdb) \v11.0** под **SQL Server** узел, щелкните правой кнопкой мыши **SQL Server** узел и нажмите кнопку **добавить SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="44acb-155">If you don't see **(localdb)\v11.0** under the **SQL Server** node, right-click the **SQL Server** node and click **Add SQL Server**.</span></span> <span data-ttu-id="44acb-156">В **соединение с сервером** введите диалоговое окно *(localdb) \v11.0* как **имя сервера**, а затем нажмите кнопку **Connect**.</span><span class="sxs-lookup"><span data-stu-id="44acb-156">In the **Connect to Server** dialog box enter *(localdb)\v11.0* as the **Server name**, and then click **Connect**.</span></span>

    <span data-ttu-id="44acb-157">Если вы не видите **aspnet ContosoUniversity**, запустите проект и выполните вход с помощью *администратора* учетные данные (пароль *devpwd*) и обновите  **Обозреватель объектов SQL Server** окна.</span><span class="sxs-lookup"><span data-stu-id="44acb-157">If you don't see **aspnet-ContosoUniversity**, run the project and log in using the *admin* credentials (password is *devpwd*), and then refresh the **SQL Server Object Explorer** window.</span></span>
3. <span data-ttu-id="44acb-158">Щелкните правой кнопкой мыши **пользователей** таблицы, а затем нажмите кнопку **конструктор представлений**.</span><span class="sxs-lookup"><span data-stu-id="44acb-158">Right-click the **Users** table, and then click **View Designer**.</span></span>

    ![Конструктор представлений SSOX](deploying-a-database-update/_static/image3.png)
4. <span data-ttu-id="44acb-160">В конструкторе добавьте *комментарии* столбца и сделать его *nvarchar(128)* и допускает значения NULL и нажмите кнопку **обновления**.</span><span class="sxs-lookup"><span data-stu-id="44acb-160">In the designer, add a *Comments* column and make it *nvarchar(128)* and nullable, and then click **Update**.</span></span>

    ![Добавление столбца комментарии](deploying-a-database-update/_static/image4.png)
5. <span data-ttu-id="44acb-162">В **Просмотр обновлений базы данных** выберите **обновления базы данных**.</span><span class="sxs-lookup"><span data-stu-id="44acb-162">In the **Preview Database Updates** box, click **Update Database**.</span></span>

    ![Просмотр обновлений базы данных](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a><span data-ttu-id="44acb-164">Создание страницы для отображения и редактирования нового столбца</span><span class="sxs-lookup"><span data-stu-id="44acb-164">Create a page to display and edit the new column</span></span>

1. <span data-ttu-id="44acb-165">В **обозревателе решений**, щелкните правой кнопкой мыши **учетной записи** папка в проекте "ContosoUniversity", щелкните **добавить**, а затем нажмите кнопку **новый элемент** .</span><span class="sxs-lookup"><span data-stu-id="44acb-165">In **Solution Explorer**, right-click the **Account** folder in the ContosoUniversity project, click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="44acb-166">Создайте новый **страницу веб-формы с помощью главного** и назовите его *UserInfo.aspx*.</span><span class="sxs-lookup"><span data-stu-id="44acb-166">Create a new **Web Form Using Master Page** and name it *UserInfo.aspx*.</span></span> <span data-ttu-id="44acb-167">Примите имя по умолчанию *Site.Master* файл в качестве главной страницы.</span><span class="sxs-lookup"><span data-stu-id="44acb-167">Accept the default *Site.Master* file as the master page.</span></span>
3. <span data-ttu-id="44acb-168">Скопируйте следующую разметку в `MainContent` `Content` элемент (последние 3 `Content` элементы):</span><span class="sxs-lookup"><span data-stu-id="44acb-168">Copy the following markup into the `MainContent` `Content` element (the last of the 3 `Content` elements):</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. <span data-ttu-id="44acb-169">Щелкните правой кнопкой мыши *UserInfo.aspx* и нажмите кнопку **просмотреть в браузере**.</span><span class="sxs-lookup"><span data-stu-id="44acb-169">Right-click the *UserInfo.aspx* page and click **View in Browser**.</span></span>
5. <span data-ttu-id="44acb-170">Войти с помощью вашей *администратора* учетные данные пользователя (пароль *devpwd*) и добавить примечания для пользователя, чтобы убедиться, что страница работает правильно.</span><span class="sxs-lookup"><span data-stu-id="44acb-170">Log in with your *admin* user credentials (password is *devpwd*) and add some comments to a user to verify that the page works correctly.</span></span>

    ![Страница сведений о пользователе](deploying-a-database-update/_static/image6.png)
6. <span data-ttu-id="44acb-172">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="44acb-172">Close the browser.</span></span>

## <a name="deploy-the-database-update"></a><span data-ttu-id="44acb-173">Развертывание обновления базы данных</span><span class="sxs-lookup"><span data-stu-id="44acb-173">Deploy the database update</span></span>

<span data-ttu-id="44acb-174">Чтобы развернуть, используя поставщик dbDacFx, необходимо просто выбрать **обновления базы данных** параметр в профиле публикации.</span><span class="sxs-lookup"><span data-stu-id="44acb-174">To deploy by using the dbDacFx provider, you just need to select the **Update database** option in the publish profile.</span></span> <span data-ttu-id="44acb-175">Тем не менее, для первоначального развертывания при использовании этого параметра можно также настроить некоторые дополнительные скрипты SQL для выполнения: они по-прежнему в профиле, и необходимо предотвратить их повторный запуск.</span><span class="sxs-lookup"><span data-stu-id="44acb-175">However, for the initial deployment when you used this option you also configured some additional SQL scripts to run: those are still in the profile and you'll have to prevent them from running again.</span></span>

1. <span data-ttu-id="44acb-176">Откройте **веб-публикации** мастера, щелкнув правой кнопкой мыши проект ContosoUniversity команду **публикации**.</span><span class="sxs-lookup"><span data-stu-id="44acb-176">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="44acb-177">Выберите **теста** профиля.</span><span class="sxs-lookup"><span data-stu-id="44acb-177">Select the **Test** profile.</span></span>
3. <span data-ttu-id="44acb-178">Нажмите кнопку **параметры** вкладки.</span><span class="sxs-lookup"><span data-stu-id="44acb-178">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="44acb-179">В разделе **DefaultConnection**выберите **обновления базы данных**.</span><span class="sxs-lookup"><span data-stu-id="44acb-179">Under **DefaultConnection**, select **Update database**.</span></span>
5. <span data-ttu-id="44acb-180">Отключите эти дополнительные сценарии, которые настроены на выполнение для первоначального развертывания:</span><span class="sxs-lookup"><span data-stu-id="44acb-180">Disable the additional scripts that you configured to run for the initial deployment:</span></span>

    1. <span data-ttu-id="44acb-181">Нажмите кнопку **настроить обновление базы данных**.</span><span class="sxs-lookup"><span data-stu-id="44acb-181">Click **Configure database updates**.</span></span>
    2. <span data-ttu-id="44acb-182">В **Настройка обновления базы данных** диалоговом окне снимите флажки рядом с полем *Grant.sql* и *aspnet-data-dev.sql*.</span><span class="sxs-lookup"><span data-stu-id="44acb-182">In the **Configure Database Updates** dialog box, clear the check boxes next to *Grant.sql* and *aspnet-data-dev.sql*.</span></span>
    3. <span data-ttu-id="44acb-183">Нажмите кнопку **Закрыть**.</span><span class="sxs-lookup"><span data-stu-id="44acb-183">Click **Close**.</span></span>
6. <span data-ttu-id="44acb-184">Нажмите кнопку **предварительной версии** вкладки.</span><span class="sxs-lookup"><span data-stu-id="44acb-184">Click the **Preview** tab.</span></span>
7. <span data-ttu-id="44acb-185">В разделе **баз данных** и справа от **DefaultConnection**, нажмите кнопку **предварительной версии базы данных** ссылку.</span><span class="sxs-lookup"><span data-stu-id="44acb-185">Under **Databases** and to the right of **DefaultConnection**, click the **Preview database** link.</span></span>

    ![Предварительная версия базы данных](deploying-a-database-update/_static/image7.png)

    <span data-ttu-id="44acb-187">Окно предварительного просмотра приведен скрипт, который будет выполняться в целевой базе данных, чтобы сделать схемы базы данных соответствует схеме базы данных-источника.</span><span class="sxs-lookup"><span data-stu-id="44acb-187">The preview window shows the script that will be run in the destination database to make that database schema match the schema of the source database.</span></span> <span data-ttu-id="44acb-188">Скрипт включает команду ALTER TABLE, который добавляет новый столбец.</span><span class="sxs-lookup"><span data-stu-id="44acb-188">The script includes an ALTER TABLE command that adds the new column.</span></span>
8. <span data-ttu-id="44acb-189">Закрыть **предварительной версии базы данных** диалоговое окно, а затем нажмите кнопку **публикации**.</span><span class="sxs-lookup"><span data-stu-id="44acb-189">Close the **Database Preview** dialog box, and then click **Publish**.</span></span>

    <span data-ttu-id="44acb-190">Visual Studio развертывает обновленное приложение и в браузере откроется домашняя страница.</span><span class="sxs-lookup"><span data-stu-id="44acb-190">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
9. <span data-ttu-id="44acb-191">Откройте страницу сведений о пользователе (Добавить *Account/UserInfo.aspx* URL-адрес домашней страницы) чтобы проверить, что обновление было успешно развернуто.</span><span class="sxs-lookup"><span data-stu-id="44acb-191">Run the UserInfo page (add *Account/UserInfo.aspx* to the home page URL) to verify that the update was successfully deployed.</span></span> <span data-ttu-id="44acb-192">Необходимо выполнить вход, введя *администратора* и *devpwd*.</span><span class="sxs-lookup"><span data-stu-id="44acb-192">You'll have to log in by entering *admin* and *devpwd*.</span></span>

    <span data-ttu-id="44acb-193">Данные в таблицах не развертывается по умолчанию, и вы не настроили скрипта развертывания данных для запуска, поэтому вы не найдете комментарий, добавленный в разработке.</span><span class="sxs-lookup"><span data-stu-id="44acb-193">Data in tables is not deployed by default, and you didn't configure a data deployment script to run, so you won't find the comment that you added in development.</span></span> <span data-ttu-id="44acb-194">Теперь новый комментарий можно добавить в промежуточной среде, чтобы убедиться, что изменение было развернуто в базу данных, и страница работает правильно.</span><span class="sxs-lookup"><span data-stu-id="44acb-194">You can add a new comment now in staging to verify that the change was deployed to the database and the page works correctly.</span></span>
10. <span data-ttu-id="44acb-195">Выполните ту же процедуру для развертывания в промежуточной и рабочей.</span><span class="sxs-lookup"><span data-stu-id="44acb-195">Follow the same procedure to deploy to staging and production.</span></span>

    <span data-ttu-id="44acb-196">Не забудьте отключить дополнительные сценарии.</span><span class="sxs-lookup"><span data-stu-id="44acb-196">Don't forget to disable the extra scripts.</span></span> <span data-ttu-id="44acb-197">По сравнению с профиль тестирования отличается только что отключит только один скрипт в промежуточной и рабочей среде профили, так как они были настроены для запуска только *aspnet-prod-data.sql*.</span><span class="sxs-lookup"><span data-stu-id="44acb-197">The only difference compared to the Test profile is that you will disable only one script in the Staging and Production profiles because they were configured to run only *aspnet-prod-data.sql*.</span></span>

    <span data-ttu-id="44acb-198">Учетные данные для тестовых и производственных: администратора и prodpwd.</span><span class="sxs-lookup"><span data-stu-id="44acb-198">The credentials for staging and production are admin and prodpwd.</span></span>

    <span data-ttu-id="44acb-199">Для обновления приложения реальной рабочей среде, который включает в себя изменение базы данных также обычно выполняемые приложения в автономный режим во время развертывания, передав *приложения\_offline.htm* перед публикацией и его удаление После этого, как видно из [предыдущем учебном курсе](deploying-a-code-update.md).</span><span class="sxs-lookup"><span data-stu-id="44acb-199">For a real production application update that includes a database change you would also typically take the application offline during deployment by uploading *app\_offline.htm* before publishing and deleting it afterward, as you saw in [the previous tutorial](deploying-a-code-update.md).</span></span>

## <a name="summary"></a><span data-ttu-id="44acb-200">Сводка</span><span class="sxs-lookup"><span data-stu-id="44acb-200">Summary</span></span>

<span data-ttu-id="44acb-201">Теперь вы развернули, включены изменения базы данных с помощью Code First Migrations и поставщик dbDacFx обновления приложения.</span><span class="sxs-lookup"><span data-stu-id="44acb-201">You've now deployed an application update that included a database change using both Code First Migrations and the dbDacFx provider.</span></span>

![Странице "Instructors" с даты рождения](deploying-a-database-update/_static/image8.png)

![Страница сведений о пользователе](deploying-a-database-update/_static/image9.png)

<span data-ttu-id="44acb-204">Следующее руководство показано, как выполнить развертывание с помощью командной строки.</span><span class="sxs-lookup"><span data-stu-id="44acb-204">The next tutorial shows you how to execute deployments by using the command line.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="44acb-205">[Назад](deploying-a-code-update.md)
> [Вперед](command-line-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="44acb-205">[Previous](deploying-a-code-update.md)
[Next](command-line-deployment.md)</span></span>
