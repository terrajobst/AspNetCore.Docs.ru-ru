---
title: "Публикация приложения ASP.NET Core в Azure с помощью Visual Studio"
author: rick-anderson
description: "Сведения о публикации приложения ASP.NET Core в службе приложений Azure с помощью Visual Studio."
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 12/16/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: e8de630c4fa8cff5395f7246b91384d4725e4ca6
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2018
---
<span data-ttu-id="cffe1-104">/ru-ru</span><span class="sxs-lookup"><span data-stu-id="cffe1-104">/en-us</span></span>

# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="cffe1-105">Публикация веб-приложения ASP.NET Core в службе приложений Azure с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cffe1-105">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="cffe1-106">Авторы: [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT), [Сезар Блум Сильвейра (Cesar Blum Silveira)](https://github.com/cesarbs) и [Рейчел Аппель (Rachel Appel)](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="cffe1-106">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="cffe1-107">Если вы работаете на компьютере Mac, см. раздел [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) (Публикация в Azure из Visual Studio для компьютера Mac).</span><span class="sxs-lookup"><span data-stu-id="cffe1-107">See [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) if you are working on a Mac.</span></span>

## <a name="set-up"></a><span data-ttu-id="cffe1-108">Настройка</span><span class="sxs-lookup"><span data-stu-id="cffe1-108">Set up</span></span>

* <span data-ttu-id="cffe1-109">Создайте [бесплатную учетную запись Azure](https://aka.ms/K5y5yh), если у вас ее нет.</span><span class="sxs-lookup"><span data-stu-id="cffe1-109">Open a [free Azure account](https://aka.ms/K5y5yh) if you do not have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="cffe1-110">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="cffe1-110">Create a web app</span></span>

<span data-ttu-id="cffe1-111">На начальной странице Visual Studio последовательно выберите **Файл > Создать > Проект…**</span><span class="sxs-lookup"><span data-stu-id="cffe1-111">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![меню "Файл"](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="cffe1-113">В диалоговом окне **Создание проекта** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="cffe1-113">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="cffe1-114">В левой области выберите **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="cffe1-114">In the left pane, select **.NET Core**.</span></span>
* <span data-ttu-id="cffe1-115">В центральной области выберите **Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="cffe1-115">In the center pane, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="cffe1-116">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="cffe1-116">Select **OK**.</span></span>

![Диалоговое окно создания нового проекта](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="cffe1-118">В диалоговом окне **Создание веб-приложения ASP.NET Core** сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="cffe1-118">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="cffe1-119">Выберите **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="cffe1-119">Select **Web Application**.</span></span>
* <span data-ttu-id="cffe1-120">Выберите **Изменить способ проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="cffe1-120">Select **Change Authentication**.</span></span>

![Диалоговое окно создания нового проекта](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="cffe1-122">Появится диалоговое окно **Изменение способа проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="cffe1-122">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="cffe1-123">Выберите **Учетные записи отдельных пользователей**.</span><span class="sxs-lookup"><span data-stu-id="cffe1-123">Select **Individual User Accounts**.</span></span>
* <span data-ttu-id="cffe1-124">Нажмите кнопку **ОК**, чтобы вернуться на страницу **Создание веб-приложения ASP.NET Core**, а затем снова нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="cffe1-124">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Диалоговое окно "Новый способ веб-проверки подлинности ASP.NET Core"](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="cffe1-126">Visual Studio создает решение.</span><span class="sxs-lookup"><span data-stu-id="cffe1-126">Visual Studio creates the solution.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="cffe1-127">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="cffe1-127">Run the app</span></span>

* <span data-ttu-id="cffe1-128">Нажмите клавиши CTRL+F5, чтобы запустить проект.</span><span class="sxs-lookup"><span data-stu-id="cffe1-128">Press CTRL+F5 to run the project.</span></span>
* <span data-ttu-id="cffe1-129">Протестируйте ссылки **О программе** и **Контакт**.</span><span class="sxs-lookup"><span data-stu-id="cffe1-129">Test the **About** and **Contact** links.</span></span>

![Веб-приложение откроется в localhost.](publish-to-azure-webapp-using-vs/_static/show.png)

### <a name="register-a-user"></a><span data-ttu-id="cffe1-131">Регистрация пользователя</span><span class="sxs-lookup"><span data-stu-id="cffe1-131">Register a user</span></span>

* <span data-ttu-id="cffe1-132">Выберите **Зарегистрироваться** и зарегистрируйте нового пользователя.</span><span class="sxs-lookup"><span data-stu-id="cffe1-132">Select **Register** and register a new user.</span></span> <span data-ttu-id="cffe1-133">Можно использовать вымышленный адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="cffe1-133">You can use a fictitious email address.</span></span> <span data-ttu-id="cffe1-134">После отправки на странице отображается следующая ошибка.</span><span class="sxs-lookup"><span data-stu-id="cffe1-134">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="cffe1-135">*"Внутренняя ошибка сервера: не удалось выполнить операцию базы данных при обработке запроса. Исключения SQL: не удается открыть базу данных. Применение имеющихся миграций для контекста базы данных приложения, возможно, позволит решить проблему."*</span><span class="sxs-lookup"><span data-stu-id="cffe1-135">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>
* <span data-ttu-id="cffe1-136">Выберите **Применить миграции** и после обновления страницы перезагрузите ее.</span><span class="sxs-lookup"><span data-stu-id="cffe1-136">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Внутренняя ошибка сервера: сбой операции из базы данных при обработке запроса.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="cffe1-140">Приложение отобразит адрес электронной почты, который использовался для регистрации нового пользователя, и ссылку **Выйти**.</span><span class="sxs-lookup"><span data-stu-id="cffe1-140">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Веб-приложение откроется в Microsoft Edge.](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="cffe1-143">Развертывание приложения в Azure</span><span class="sxs-lookup"><span data-stu-id="cffe1-143">Deploy the app to Azure</span></span>

<span data-ttu-id="cffe1-144">В обозревателе решений щелкните правой кнопкой мыши проект и выберите команду **Опубликовать…**</span><span class="sxs-lookup"><span data-stu-id="cffe1-144">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Открытое контекстное меню с выделенной ссылкой "Опубликовать"](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="cffe1-146">В диалоговом окне **Публикация**:</span><span class="sxs-lookup"><span data-stu-id="cffe1-146">In the **Publish** dialog:</span></span>

* <span data-ttu-id="cffe1-147">Выберите **Служба приложений Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="cffe1-147">Select **Microsoft Azure App Service**.</span></span>
* <span data-ttu-id="cffe1-148">Выберите значок шестеренки и затем пункт **Создать профиль**.</span><span class="sxs-lookup"><span data-stu-id="cffe1-148">Select the gear icon and then select **Create Profile**.</span></span>
* <span data-ttu-id="cffe1-149">Выберите **Создать профиль**.</span><span class="sxs-lookup"><span data-stu-id="cffe1-149">Select **Create Profile**.</span></span>

![Диалоговое окно публикации](publish-to-azure-webapp-using-vs/_static/maas1.png)

### <a name="create-azure-resources"></a><span data-ttu-id="cffe1-151">Создание ресурсов Azure</span><span class="sxs-lookup"><span data-stu-id="cffe1-151">Create Azure resources</span></span>

<span data-ttu-id="cffe1-152">Отображается диалоговое окно **Создание приложения службы**:</span><span class="sxs-lookup"><span data-stu-id="cffe1-152">The **Create App Service** dialog appears:</span></span>

* <span data-ttu-id="cffe1-153">Введите свою подписку.</span><span class="sxs-lookup"><span data-stu-id="cffe1-153">Enter your subscription.</span></span>
* <span data-ttu-id="cffe1-154">Заполняются поля ввода **Имя приложения**, **Группа ресурсов** и **План службы приложений**.</span><span class="sxs-lookup"><span data-stu-id="cffe1-154">The **App Name**, **Resource Group**, and **App Service Plan** entry fields are populated.</span></span> <span data-ttu-id="cffe1-155">Вы можете сохранить эти имена или изменить их.</span><span class="sxs-lookup"><span data-stu-id="cffe1-155">You can keep these names or change them.</span></span>

![Диалоговое окно "Служба приложений"](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="cffe1-157">Выберите вкладку **Службы**, чтобы создать базу данных.</span><span class="sxs-lookup"><span data-stu-id="cffe1-157">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="cffe1-158">Выберите зеленый значок **+**, чтобы создать базу данных SQL.</span><span class="sxs-lookup"><span data-stu-id="cffe1-158">Select the green **+** icon to create a new SQL Database</span></span>

![Новая база данных SQL](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="cffe1-160">Щелкните **Создать…** в диалоговом окне **Настройка базы данных SQL**, чтобы создать базу данных.</span><span class="sxs-lookup"><span data-stu-id="cffe1-160">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Новый сервер и база данных SQL](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="cffe1-162">Появится диалоговое окно **Настройка SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="cffe1-162">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="cffe1-163">Введите имя пользователя и пароль администратора, а затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="cffe1-163">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="cffe1-164">Вы можете сохранить **Имя сервера** по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="cffe1-164">You can keep the default **Server Name**.</span></span> 

> [!NOTE]
> <span data-ttu-id="cffe1-165">"admin" не может использоваться в качестве имени пользователя администратора.</span><span class="sxs-lookup"><span data-stu-id="cffe1-165">"admin" is not allowed as the administrator user name.</span></span>

![Диалоговое окно "Настройка SQL Server"](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="cffe1-167">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="cffe1-167">Select **OK**.</span></span>

<span data-ttu-id="cffe1-168">Visual Studio вернет диалоговое окно **Создание службы приложений**.</span><span class="sxs-lookup"><span data-stu-id="cffe1-168">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="cffe1-169">Нажмите кнопку **Создать** в диалоговом окне **Создание службы приложений**.</span><span class="sxs-lookup"><span data-stu-id="cffe1-169">Select **Create** on the **Create App Service** dialog.</span></span>

![Диалоговое окно "Настройка базы данных SQL"](publish-to-azure-webapp-using-vs/_static/conf_final.png)

<span data-ttu-id="cffe1-171">Visual Studio создает веб-приложение и SQL Server в Azure.</span><span class="sxs-lookup"><span data-stu-id="cffe1-171">Visual Studio creates the Web app and SQL Server on Azure.</span></span> <span data-ttu-id="cffe1-172">Это может занять несколько минут.</span><span class="sxs-lookup"><span data-stu-id="cffe1-172">This step can take a few minutes.</span></span> <span data-ttu-id="cffe1-173">Сведения о созданных ресурсах см. в разделе [Дополнительные ресурсы](#additonal-resources).</span><span class="sxs-lookup"><span data-stu-id="cffe1-173">For information on the resources created, see [Additonal resources](#additonal-resources).</span></span>

<span data-ttu-id="cffe1-174">После завершения развертывания выберите **Параметры**:</span><span class="sxs-lookup"><span data-stu-id="cffe1-174">When deployment completes, select **Settings**:</span></span>

![Диалоговое окно "Настройка SQL Server"](publish-to-azure-webapp-using-vs/_static/set.png)

<span data-ttu-id="cffe1-176">На странице **Параметры** диалогового окна **Публикация**</span><span class="sxs-lookup"><span data-stu-id="cffe1-176">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="cffe1-177">Разверните раздел **Базы данных** и установите флажок **Использовать эту строку подключения во время выполнения**.</span><span class="sxs-lookup"><span data-stu-id="cffe1-177">Expand **Databases** and check **Use this connection string at runtime**.</span></span>
  * <span data-ttu-id="cffe1-178">Разверните раздел **Миграции Entity Framework** и установите флажок **Использовать эту миграцию при публикации**.</span><span class="sxs-lookup"><span data-stu-id="cffe1-178">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="cffe1-179">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="cffe1-179">Select **Save**.</span></span> <span data-ttu-id="cffe1-180">Visual Studio вернется в диалоговое окно **Публикация**.</span><span class="sxs-lookup"><span data-stu-id="cffe1-180">Visual Studio returns to the **Publish** dialog.</span></span> 

![Диалоговое окно "Публикация": панель "Параметры"](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="cffe1-182">Нажмите кнопку **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="cffe1-182">Click **Publish**.</span></span> <span data-ttu-id="cffe1-183">Visual Studio публикует приложение в Azure.</span><span class="sxs-lookup"><span data-stu-id="cffe1-183">Visual Studio publishs your app to Azure.</span></span> <span data-ttu-id="cffe1-184">По завершении развертывания приложение открывается в браузере.</span><span class="sxs-lookup"><span data-stu-id="cffe1-184">When the depoyment completes, the app is opened in a browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="cffe1-185">Тестирование приложения в Azure</span><span class="sxs-lookup"><span data-stu-id="cffe1-185">Test your app in Azure</span></span>

* <span data-ttu-id="cffe1-186">Протестируйте ссылки **О программе** и **Контакт**.</span><span class="sxs-lookup"><span data-stu-id="cffe1-186">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="cffe1-187">Регистрация нового пользователя</span><span class="sxs-lookup"><span data-stu-id="cffe1-187">Register a new user</span></span>

![Веб-приложение, открытое в Microsoft Edge в службе приложений Azure](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="cffe1-189">Обновление приложения</span><span class="sxs-lookup"><span data-stu-id="cffe1-189">Update the app</span></span>

* <span data-ttu-id="cffe1-190">Измените страницу Razor *Pages/About.cshtml* и ее содержимое.</span><span class="sxs-lookup"><span data-stu-id="cffe1-190">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="cffe1-191">Например, вы можете изменить абзац на "Hello ASP.NET Core!": [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="cffe1-191">For example, you can modify the paragraph to say "Hello ASP.NET Core!": [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span></span>

* <span data-ttu-id="cffe1-192">Щелкните правой кнопкой мыши проект и снова выберите пункт **Опубликовать...**.</span><span class="sxs-lookup"><span data-stu-id="cffe1-192">Right-click on the project and select **Publish...** again.</span></span>

![Открытое контекстное меню с выделенной ссылкой "Опубликовать"](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="cffe1-194">После публикации приложения убедитесь, что внесенные изменения доступны в Azure.</span><span class="sxs-lookup"><span data-stu-id="cffe1-194">After the app is published, verify the changes you made are available on Azure.</span></span>

![Убедитесь, что задача завершена.](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="cffe1-196">Очистка</span><span class="sxs-lookup"><span data-stu-id="cffe1-196">Clean up</span></span>

<span data-ttu-id="cffe1-197">Завершив тестирование приложения, перейдите на [портал Azure](https://portal.azure.com/) и удалите приложение.</span><span class="sxs-lookup"><span data-stu-id="cffe1-197">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="cffe1-198">Выберите пункт **Группы ресурсов**, а затем созданную группу ресурсов.</span><span class="sxs-lookup"><span data-stu-id="cffe1-198">Select **Resource groups**, then select the resource group you created.</span></span>

![Портал Azure: пункт "Группы ресурсов" в меню боковой панели](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="cffe1-200">На странице **Группы ресурсов** выберите **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="cffe1-200">In the **Resource groups** page, select **Delete**.</span></span>

![Портал Azure: страница "Группы ресурсов"](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="cffe1-202">Введите имя группы ресурсов и выберите **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="cffe1-202">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="cffe1-203">Ваше приложение и все ресурсы, созданные при работе с этим руководством, удалены из Azure.</span><span class="sxs-lookup"><span data-stu-id="cffe1-203">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="cffe1-204">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="cffe1-204">Next steps</span></span>

* [<span data-ttu-id="cffe1-205">Непрерывное развертывание в Azure с помощью Visual Studio и Git</span><span class="sxs-lookup"><span data-stu-id="cffe1-205">Continuous Deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)

## <a name="additonal-resources"></a><span data-ttu-id="cffe1-206">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="cffe1-206">Additonal resources</span></span>

* <span data-ttu-id="cffe1-207">[служба приложений Azure](https://docs.microsoft.com/en-us/azure/app-service/app-service-web-overview);</span><span class="sxs-lookup"><span data-stu-id="cffe1-207">[Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/app-service-web-overview)</span></span>
* [<span data-ttu-id="cffe1-208">Группа ресурсов Azure</span><span class="sxs-lookup"><span data-stu-id="cffe1-208">Azure resource groups</span></span>](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview#resource-groups)
* [<span data-ttu-id="cffe1-209">База данных SQL Azure</span><span class="sxs-lookup"><span data-stu-id="cffe1-209">Azure SQL Database</span></span>](https://docs.microsoft.com/en-us/azure/sql-database/)