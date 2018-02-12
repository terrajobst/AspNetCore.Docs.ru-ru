---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: "ASP.NET веб-развертывания с помощью Visual Studio: развертывание обновления базы данных | Документы Microsoft"
author: tdykstra
description: "Этот учебник ряд показано развертывание ASP.NET (публикации) веб-приложения для веб-приложениях службы приложений Azure или стороннего поставщика услуг размещения, Пол..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 8e875a4282df78ec647579e74c3fbeabd2495fc2
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a><span data-ttu-id="26005-103">ASP.NET веб-развертывания с помощью Visual Studio: развертывание обновления базы данных</span><span class="sxs-lookup"><span data-stu-id="26005-103">ASP.NET Web Deployment using Visual Studio: Deploying a Database Update</span></span>
====================
<span data-ttu-id="26005-104">По [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="26005-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="26005-105">Загрузите начальный проект</span><span class="sxs-lookup"><span data-stu-id="26005-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="26005-106">Этот учебник ряд показано развертывание ASP.NET (публикации) веб-приложения для веб-приложениях службы приложений Azure или стороннего поставщика услуг размещения, с помощью Visual Studio 2012 или Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="26005-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="26005-107">Сведения о последовательности см. в разделе [в первом учебнике ряда](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="26005-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="26005-108">Обзор</span><span class="sxs-lookup"><span data-stu-id="26005-108">Overview</span></span>

<span data-ttu-id="26005-109">В этом учебнике внести изменение базы данных или изменение связанного кода, тестирования изменений в Visual Studio, а затем развернуть обновление для тестирования, промежуточного хранения и рабочей среды.</span><span class="sxs-lookup"><span data-stu-id="26005-109">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to the test, staging, and production environments.</span></span>

<span data-ttu-id="26005-110">Учебник сначала показано, как для обновления базы данных, который управляется миграции Code First и нажмите Далее показано, как обновить базу данных, используя поставщик dbDacFx.</span><span class="sxs-lookup"><span data-stu-id="26005-110">The tutorial first shows how to update a database that is managed by Code First Migrations, and then later it shows how to update a database by using the dbDacFx provider.</span></span>

<span data-ttu-id="26005-111">Напоминание: Если вы получаете сообщение об ошибке или что-то не работает, как пройти учебник, не забудьте проверить [страницу устранения неполадок](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="26005-111">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a><span data-ttu-id="26005-112">Развертывание обновления базы данных с помощью Code First Migrations</span><span class="sxs-lookup"><span data-stu-id="26005-112">Deploy a database update by using Code First Migrations</span></span>

<span data-ttu-id="26005-113">В этом разделе добавьте столбец даты рождения для `Person` базовый класс для `Student` и `Instructor` сущности.</span><span class="sxs-lookup"><span data-stu-id="26005-113">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="26005-114">Обновите страницу, отображающую инструктора данных, чтобы он отображал нового столбца.</span><span class="sxs-lookup"><span data-stu-id="26005-114">Then you update the page that displays instructor data so that it displays the new column.</span></span> <span data-ttu-id="26005-115">Наконец необходимо выполнить развертывание изменений для тестирования, промежуточного хранения и рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="26005-115">Finally, you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-application-database"></a><span data-ttu-id="26005-116">Добавить столбец в таблицу в базе данных приложения</span><span class="sxs-lookup"><span data-stu-id="26005-116">Add a column to a table in the application database</span></span>

1. <span data-ttu-id="26005-117">В *ContosoUniversity.DAL* откройте проект *Person.cs* и добавьте следующее свойство в конце `Person` класса (должно быть две фигурные скобки после его закрытия):</span><span class="sxs-lookup"><span data-stu-id="26005-117">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    <span data-ttu-id="26005-118">Затем обновите `Seed` метода, так что он содержит значение для нового столбца.</span><span class="sxs-lookup"><span data-stu-id="26005-118">Next, update the `Seed` method so that it provides a value for the new column.</span></span> <span data-ttu-id="26005-119">Откройте *Migrations\Configuration.cs* и заменить блок кода, который начинается `var instructors = new List<Instructor>` с следующий блок кода, который содержит информацию о дате рождения:</span><span class="sxs-lookup"><span data-stu-id="26005-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. <span data-ttu-id="26005-120">Постройте решение, а затем откройте **консоль диспетчера пакетов** окна.</span><span class="sxs-lookup"><span data-stu-id="26005-120">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="26005-121">Убедитесь, что ContosoUniversity.DAL по-прежнему выбран в качестве **проекта по умолчанию**.</span><span class="sxs-lookup"><span data-stu-id="26005-121">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>
3. <span data-ttu-id="26005-122">В **консоль диспетчера пакетов** выберите **ContosoUniversity.DAL** как **проекта по умолчанию**, а затем введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="26005-122">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    <span data-ttu-id="26005-123">По завершении этой команды Visual Studio открывает файл класса, который определяет новый `DbMIgration` класса и в `Up` метода можно просмотреть код, который создает новый столбец.</span><span class="sxs-lookup"><span data-stu-id="26005-123">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` class, and in the `Up` method you can see the code that creates the new column.</span></span> <span data-ttu-id="26005-124">`Up` Метод создает столбец при внесении изменений и `Down` метод удаляет столбец, если выполняется откат изменений.</span><span class="sxs-lookup"><span data-stu-id="26005-124">The `Up` method creates the column when you are implementing the change, and the `Down` method deletes the column when you are rolling back the change.</span></span>

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. <span data-ttu-id="26005-126">Постройте решение, а затем введите следующую команду в **консоль диспетчера пакетов** окна (Убедитесь, что по-прежнему выбран проект ContosoUniversity.DAL):</span><span class="sxs-lookup"><span data-stu-id="26005-126">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    <span data-ttu-id="26005-127">Платформа Entity Framework выполняется `Up` метода, а затем выполняется `Seed` метод.</span><span class="sxs-lookup"><span data-stu-id="26005-127">The Entity Framework runs the `Up` method and then runs the `Seed` method.</span></span>

### <a name="display-the-new-column-in-the-instructors-page"></a><span data-ttu-id="26005-128">Отображение нового столбца на странице инструкторов</span><span class="sxs-lookup"><span data-stu-id="26005-128">Display the new column in the Instructors page</span></span>

1. <span data-ttu-id="26005-129">В проекте ContosoUniversity откройте *Instructors.aspx* и добавить новое поле шаблон для отображения даты рождения.</span><span class="sxs-lookup"><span data-stu-id="26005-129">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="26005-130">Добавьте его от тех, для назначения найма даты и office:</span><span class="sxs-lookup"><span data-stu-id="26005-130">Add it between the ones for hire date and office assignment:</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    <span data-ttu-id="26005-131">(Если отступа кода получает синхронизированы, можно нажать сочетание клавиш CTRL-K и CTRL + D автоматическое переформатирование файл.)</span><span class="sxs-lookup"><span data-stu-id="26005-131">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>
2. <span data-ttu-id="26005-132">Запустите приложение и нажмите кнопку **инструкторов** ссылку.</span><span class="sxs-lookup"><span data-stu-id="26005-132">Run the application and click the **Instructors** link.</span></span>

    <span data-ttu-id="26005-133">При загрузке страницы, вы видите, что она имеет новый поля «Дата рождения».</span><span class="sxs-lookup"><span data-stu-id="26005-133">When the page loads, you see that it has the new birth date field.</span></span>

    ![Страница инструкторов с Дата рождения](deploying-a-database-update/_static/image2.png)
3. <span data-ttu-id="26005-135">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="26005-135">Close the browser.</span></span>

### <a name="deploy-the-database-update"></a><span data-ttu-id="26005-136">Развертывание обновления базы данных</span><span class="sxs-lookup"><span data-stu-id="26005-136">Deploy the database update</span></span>

1. <span data-ttu-id="26005-137">В **обозревателе решений** выберите ContosoUniversity проект.</span><span class="sxs-lookup"><span data-stu-id="26005-137">In **Solution Explorer** select the ContosoUniversity project.</span></span>
2. <span data-ttu-id="26005-138">В **веб-публикация одним щелкните** инструментов, нажмите кнопку **тест** профиль публикации, а затем нажмите кнопку **веб-публикация**.</span><span class="sxs-lookup"><span data-stu-id="26005-138">In the **Web One Click Publish** toolbar, click the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="26005-139">(Если отключена панели инструментов, выберите проект ContosoUniversity в **обозревателе решений**.)</span><span class="sxs-lookup"><span data-stu-id="26005-139">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

    <span data-ttu-id="26005-140">Visual Studio развертывает обновленное приложение, и в браузере будет открыта на домашнюю страницу.</span><span class="sxs-lookup"><span data-stu-id="26005-140">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
3. <span data-ttu-id="26005-141">Запустите **инструкторов** страницу, чтобы убедиться, что обновление было успешно развернуто.</span><span class="sxs-lookup"><span data-stu-id="26005-141">Run the **Instructors** page to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="26005-142">Когда приложение пытается получить доступ к базе данных для этой страницы, Code First обновляет схему базы данных и запускает `Seed` метод.</span><span class="sxs-lookup"><span data-stu-id="26005-142">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="26005-143">При отображении страницы, вы видите ожидаемых **Дата рождения** столбец с датами в нем.</span><span class="sxs-lookup"><span data-stu-id="26005-143">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>
4. <span data-ttu-id="26005-144">В **веб-публикация одним щелкните** инструментов, нажмите кнопку **промежуточной** профиль публикации, а затем нажмите кнопку **веб-публикация**.</span><span class="sxs-lookup"><span data-stu-id="26005-144">In the **Web One Click Publish** toolbar, click the **Staging** publish profile, and then click **Publish Web**.</span></span>
5. <span data-ttu-id="26005-145">Запустите **инструкторов** страницы размещения, чтобы убедиться, что обновление было успешно развернуто.</span><span class="sxs-lookup"><span data-stu-id="26005-145">Run the **Instructors** page in staging to verify that the update was successfully deployed.</span></span>
6. <span data-ttu-id="26005-146">В **веб-публикация одним щелкните** инструментов, нажмите кнопку **рабочей** профиль публикации, а затем нажмите кнопку **веб-публикация**.</span><span class="sxs-lookup"><span data-stu-id="26005-146">In the **Web One Click Publish** toolbar, click the **Production** publish profile, and then click **Publish Web**.</span></span>
7. <span data-ttu-id="26005-147">Запустите **инструкторов** страницы в эксплуатацию, чтобы убедиться, что обновление было успешно развернуто.</span><span class="sxs-lookup"><span data-stu-id="26005-147">Run the **Instructors** page in production to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="26005-148">Для обновления приложения рабочей, который включает в себя изменение базы данных будет обычно также перевести приложение в автономный режим во время развертывания с помощью *приложения\_offline.htm*, как показано в предыдущем учебнике.</span><span class="sxs-lookup"><span data-stu-id="26005-148">For a real production application update that includes a database change you would also typically take the application offline during deployment by using *app\_offline.htm*, as you saw in the previous tutorial.</span></span>

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a><span data-ttu-id="26005-149">Развертывание обновления базы данных с помощью поставщик dbDacFx</span><span class="sxs-lookup"><span data-stu-id="26005-149">Deploy a database update by using the dbDacFx provider</span></span>

<span data-ttu-id="26005-150">В этом разделе добавьте *комментарии* столбец *пользователя* таблицы в базе данных членства и создание страницы, которая позволяет отображать и изменять комментарии для каждого пользователя.</span><span class="sxs-lookup"><span data-stu-id="26005-150">In this section, you add a *Comments* column to the *User* table in the membership database and create a page that lets you display and edit comments for each user.</span></span> <span data-ttu-id="26005-151">Затем необходимо выполнить развертывание изменений для тестирования, промежуточного хранения и рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="26005-151">Then you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-membership-database"></a><span data-ttu-id="26005-152">Добавить столбец в таблицу в базе данных членства</span><span class="sxs-lookup"><span data-stu-id="26005-152">Add a column to a table in the membership database</span></span>

1. <span data-ttu-id="26005-153">В Visual Studio откройте **обозреватель объектов SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="26005-153">In Visual Studio, open **SQL Server Object Explorer**.</span></span>
2. <span data-ttu-id="26005-154">Разверните **(localdb) \v11.0**, разверните **баз данных**, разверните **aspnet ContosoUniversity** (не **aspnet ContosoUniversity одной рабочей**) а затем разверните **таблиц**.</span><span class="sxs-lookup"><span data-stu-id="26005-154">Expand **(localdb)\v11.0**, expand **Databases**, expand **aspnet-ContosoUniversity** (not **aspnet-ContosoUniversity-Prod**) and then expand **Tables**.</span></span>

    <span data-ttu-id="26005-155">Если вы не видите **(localdb) \v11.0** под **SQL Server** узел, щелкните правой кнопкой мыши **SQL Server** и выберите команду **добавить SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="26005-155">If you don't see **(localdb)\v11.0** under the **SQL Server** node, right-click the **SQL Server** node and click **Add SQL Server**.</span></span> <span data-ttu-id="26005-156">В **соединение с сервером** введите диалоговое окно *(localdb) \v11.0* как **имя сервера**, а затем нажмите кнопку **Connect**.</span><span class="sxs-lookup"><span data-stu-id="26005-156">In the **Connect to Server** dialog box enter *(localdb)\v11.0* as the **Server name**, and then click **Connect**.</span></span>

    <span data-ttu-id="26005-157">Если вы не видите **aspnet ContosoUniversity**, запустите проект и выполнить вход с помощью *администратора* учетные данные (пароль *devpwd*) и обновите  **Обозреватель объектов SQL Server** окна.</span><span class="sxs-lookup"><span data-stu-id="26005-157">If you don't see **aspnet-ContosoUniversity**, run the project and log in using the *admin* credentials (password is *devpwd*), and then refresh the **SQL Server Object Explorer** window.</span></span>
3. <span data-ttu-id="26005-158">Щелкните правой кнопкой мыши **пользователей** , а затем выберите пункт **конструктор представлений**.</span><span class="sxs-lookup"><span data-stu-id="26005-158">Right-click the **Users** table, and then click **View Designer**.</span></span>

    ![Конструктор представлений SSOX](deploying-a-database-update/_static/image3.png)
4. <span data-ttu-id="26005-160">В конструкторе добавьте *комментарии* столбца и сделать его *nvarchar(128)* и допускает значения NULL и нажмите кнопку **обновление**.</span><span class="sxs-lookup"><span data-stu-id="26005-160">In the designer, add a *Comments* column and make it *nvarchar(128)* and nullable, and then click **Update**.</span></span>

    ![Добавление столбца комментарии](deploying-a-database-update/_static/image4.png)
5. <span data-ttu-id="26005-162">В **Просмотр обновлений базы данных** щелкните **обновление базы данных**.</span><span class="sxs-lookup"><span data-stu-id="26005-162">In the **Preview Database Updates** box, click **Update Database**.</span></span>

    ![Просмотр обновлений базы данных](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a><span data-ttu-id="26005-164">Создание страницы для отображения и редактирования нового столбца</span><span class="sxs-lookup"><span data-stu-id="26005-164">Create a page to display and edit the new column</span></span>

1. <span data-ttu-id="26005-165">В **обозревателе решений**, щелкните правой кнопкой мыши **учетной записи** щелкните папку в проекте ContosoUniversity **добавить**и нажмите кнопку **новый элемент** .</span><span class="sxs-lookup"><span data-stu-id="26005-165">In **Solution Explorer**, right-click the **Account** folder in the ContosoUniversity project, click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="26005-166">Создайте новый **страницу веб-формы с помощью главного** и назовите его *UserInfo.aspx*.</span><span class="sxs-lookup"><span data-stu-id="26005-166">Create a new **Web Form Using Master Page** and name it *UserInfo.aspx*.</span></span> <span data-ttu-id="26005-167">Примите имя по умолчанию *Site.Master* файл в качестве главной страницы.</span><span class="sxs-lookup"><span data-stu-id="26005-167">Accept the default *Site.Master* file as the master page.</span></span>
3. <span data-ttu-id="26005-168">Скопируйте следующую разметку в `MainContent` `Content` элемент (последний 3 `Content` элементы):</span><span class="sxs-lookup"><span data-stu-id="26005-168">Copy the following markup into the `MainContent` `Content` element (the last of the 3 `Content` elements):</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. <span data-ttu-id="26005-169">Щелкните правой кнопкой мыши *UserInfo.aspx* и нажмите кнопку **Просмотр в браузере**.</span><span class="sxs-lookup"><span data-stu-id="26005-169">Right-click the *UserInfo.aspx* page and click **View in Browser**.</span></span>
5. <span data-ttu-id="26005-170">Входа в систему на *администратора* учетные данные (пароль *devpwd*) и добавить примечания к пользователю, чтобы убедиться, что страница работает правильно.</span><span class="sxs-lookup"><span data-stu-id="26005-170">Log in with your *admin* user credentials (password is *devpwd*) and add some comments to a user to verify that the page works correctly.</span></span>

    ![Страница сведений о пользователях](deploying-a-database-update/_static/image6.png)
6. <span data-ttu-id="26005-172">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="26005-172">Close the browser.</span></span>

## <a name="deploy-the-database-update"></a><span data-ttu-id="26005-173">Развертывание обновления базы данных</span><span class="sxs-lookup"><span data-stu-id="26005-173">Deploy the database update</span></span>

<span data-ttu-id="26005-174">Развертывание с помощью поставщик dbDacFx, необходимо просто выбрать **обновление базы данных** параметра в профиле публикации.</span><span class="sxs-lookup"><span data-stu-id="26005-174">To deploy by using the dbDacFx provider, you just need to select the **Update database** option in the publish profile.</span></span> <span data-ttu-id="26005-175">Однако для первоначального развертывания при использовании этого параметра можно также настроить некоторые дополнительные скрипты SQL для выполнения: они по-прежнему в профиле, и необходимо предотвратить еще раз.</span><span class="sxs-lookup"><span data-stu-id="26005-175">However, for the initial deployment when you used this option you also configured some additional SQL scripts to run: those are still in the profile and you'll have to prevent them from running again.</span></span>

1. <span data-ttu-id="26005-176">Откройте **веб-публикация** мастера, щелкнув правой кнопкой мыши проект ContosoUniversity команду **публикации**.</span><span class="sxs-lookup"><span data-stu-id="26005-176">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="26005-177">Выберите **тест** профиля.</span><span class="sxs-lookup"><span data-stu-id="26005-177">Select the **Test** profile.</span></span>
3. <span data-ttu-id="26005-178">Нажмите кнопку **параметры** вкладки.</span><span class="sxs-lookup"><span data-stu-id="26005-178">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="26005-179">В разделе **DefaultConnection**выберите **обновление базы данных**.</span><span class="sxs-lookup"><span data-stu-id="26005-179">Under **DefaultConnection**, select **Update database**.</span></span>
5. <span data-ttu-id="26005-180">Отключите эти дополнительные сценарии, настроенные на выполнение для первоначального развертывания:</span><span class="sxs-lookup"><span data-stu-id="26005-180">Disable the additional scripts that you configured to run for the initial deployment:</span></span>

    1. <span data-ttu-id="26005-181">Нажмите кнопку **настроить обновление базы данных**.</span><span class="sxs-lookup"><span data-stu-id="26005-181">Click **Configure database updates**.</span></span>
    2. <span data-ttu-id="26005-182">В **Настройка обновления базы данных** диалоговое окно, снимите флажки рядом с *Grant.sql* и *aspnet данных dev.sql*.</span><span class="sxs-lookup"><span data-stu-id="26005-182">In the **Configure Database Updates** dialog box, clear the check boxes next to *Grant.sql* and *aspnet-data-dev.sql*.</span></span>
    3. <span data-ttu-id="26005-183">Нажмите кнопку **Закрыть**.</span><span class="sxs-lookup"><span data-stu-id="26005-183">Click **Close**.</span></span>
6. <span data-ttu-id="26005-184">Нажмите кнопку **предварительного просмотра** вкладки.</span><span class="sxs-lookup"><span data-stu-id="26005-184">Click the **Preview** tab.</span></span>
7. <span data-ttu-id="26005-185">В разделе **баз данных** и справа от **DefaultConnection**, нажмите кнопку **предварительной версии базы данных** ссылку.</span><span class="sxs-lookup"><span data-stu-id="26005-185">Under **Databases** and to the right of **DefaultConnection**, click the **Preview database** link.</span></span>

    ![Предварительная версия базы данных](deploying-a-database-update/_static/image7.png)

    <span data-ttu-id="26005-187">В окне просмотра отображается скрипт, который будет выполняться в целевой базы данных, чтобы сделать схемы базы данных совпадает со схемой базы данных-источника.</span><span class="sxs-lookup"><span data-stu-id="26005-187">The preview window shows the script that will be run in the destination database to make that database schema match the schema of the source database.</span></span> <span data-ttu-id="26005-188">Скрипт включает команду ALTER TABLE, который добавляет новый столбец.</span><span class="sxs-lookup"><span data-stu-id="26005-188">The script includes an ALTER TABLE command that adds the new column.</span></span>
8. <span data-ttu-id="26005-189">Закрыть **предварительной версии базы данных** диалоговое окно, а затем щелкните **публикации**.</span><span class="sxs-lookup"><span data-stu-id="26005-189">Close the **Database Preview** dialog box, and then click **Publish**.</span></span>

    <span data-ttu-id="26005-190">Visual Studio развертывает обновленное приложение, и в браузере будет открыта на домашнюю страницу.</span><span class="sxs-lookup"><span data-stu-id="26005-190">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
9. <span data-ttu-id="26005-191">Откройте страницу сведений о пользователях (Добавить *Account/UserInfo.aspx* URL-адрес домашней страницы) чтобы проверить, что обновление было успешно развернуто.</span><span class="sxs-lookup"><span data-stu-id="26005-191">Run the UserInfo page (add *Account/UserInfo.aspx* to the home page URL) to verify that the update was successfully deployed.</span></span> <span data-ttu-id="26005-192">Вам потребуется войти, введя *администратора* и *devpwd*.</span><span class="sxs-lookup"><span data-stu-id="26005-192">You'll have to log in by entering *admin* and *devpwd*.</span></span>

    <span data-ttu-id="26005-193">Данные в таблицах не разворачивается по умолчанию и настроило сценарий развертывания данных для запуска, поэтому вы не найдете комментарий, который вы добавили в разработке.</span><span class="sxs-lookup"><span data-stu-id="26005-193">Data in tables is not deployed by default, and you didn't configure a data deployment script to run, so you won't find the comment that you added in development.</span></span> <span data-ttu-id="26005-194">Можно добавить новый комментарий теперь в промежуточной, чтобы проверить, был развертывание изменений в базе данных, и страница работает правильно.</span><span class="sxs-lookup"><span data-stu-id="26005-194">You can add a new comment now in staging to verify that the change was deployed to the database and the page works correctly.</span></span>
10. <span data-ttu-id="26005-195">Выполните ту же процедуру для развертывания в промежуточной и производственной сред.</span><span class="sxs-lookup"><span data-stu-id="26005-195">Follow the same procedure to deploy to staging and production.</span></span>

    <span data-ttu-id="26005-196">Не забудьте отключить дополнительных сценариев.</span><span class="sxs-lookup"><span data-stu-id="26005-196">Don't forget to disable the extra scripts.</span></span> <span data-ttu-id="26005-197">По сравнению с тестовый профиль отличается только что отключит только один сценарий в промежуточной и рабочей профили, так как они были настроены для запуска только *aspnet prod-data.sql*.</span><span class="sxs-lookup"><span data-stu-id="26005-197">The only difference compared to the Test profile is that you will disable only one script in the Staging and Production profiles because they were configured to run only *aspnet-prod-data.sql*.</span></span>

    <span data-ttu-id="26005-198">Учетные данные для промежуточных и производственных: admin и prodpwd.</span><span class="sxs-lookup"><span data-stu-id="26005-198">The credentials for staging and production are admin and prodpwd.</span></span>

    <span data-ttu-id="26005-199">Для обновления приложения рабочей, который включает в себя изменение базы данных обычно также может потребоваться приложение в автономный режим во время развертывания путем передачи *приложения\_offline.htm* перед публикацией и удалением После этого, как показано в [работу с предыдущим учебником](deploying-a-code-update.md).</span><span class="sxs-lookup"><span data-stu-id="26005-199">For a real production application update that includes a database change you would also typically take the application offline during deployment by uploading *app\_offline.htm* before publishing and deleting it afterward, as you saw in [the previous tutorial](deploying-a-code-update.md).</span></span>

## <a name="summary"></a><span data-ttu-id="26005-200">Сводка</span><span class="sxs-lookup"><span data-stu-id="26005-200">Summary</span></span>

<span data-ttu-id="26005-201">Теперь, когда развертывание обновления приложения, включенные с помощью Code First Migrations и поставщик dbDacFx изменение базы данных.</span><span class="sxs-lookup"><span data-stu-id="26005-201">You've now deployed an application update that included a database change using both Code First Migrations and the dbDacFx provider.</span></span>

![Страница инструкторов с Дата рождения](deploying-a-database-update/_static/image8.png)

![Страница сведений о пользователях](deploying-a-database-update/_static/image9.png)

<span data-ttu-id="26005-204">Далее учебника показано, как выполнить развертывание с помощью командной строки.</span><span class="sxs-lookup"><span data-stu-id="26005-204">The next tutorial shows you how to execute deployments by using the command line.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="26005-205">[Назад](deploying-a-code-update.md)
[Вперед](command-line-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="26005-205">[Previous](deploying-a-code-update.md)
[Next](command-line-deployment.md)</span></span>
