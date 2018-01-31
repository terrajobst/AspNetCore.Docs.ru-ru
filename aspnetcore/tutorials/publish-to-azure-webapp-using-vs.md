---
title: "Публикация приложения ASP.NET Core в Azure с помощью Visual Studio"
author: rick-anderson
description: "Сведения о публикации приложения ASP.NET Core в службе приложений Azure с помощью Visual Studio."
manager: wpickett
ms.author: riande
ms.date: 12/16/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: 14d8dd0a5e6a99bacce3bf50b0468b20e0dddb96
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="52bba-103">Публикация веб-приложения ASP.NET Core в службе приложений Azure с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="52bba-103">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="52bba-104">Авторы: [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT), [Сезар Блум Сильвейра (Cesar Blum Silveira)](https://github.com/cesarbs) и [Рейчел Аппель (Rachel Appel)](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="52bba-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="52bba-105">Если вы работаете на компьютере Mac, см. раздел [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) (Публикация в Azure из Visual Studio для компьютера Mac).</span><span class="sxs-lookup"><span data-stu-id="52bba-105">See [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) if you are working on a Mac.</span></span>

## <a name="set-up"></a><span data-ttu-id="52bba-106">Настройка</span><span class="sxs-lookup"><span data-stu-id="52bba-106">Set up</span></span>

* <span data-ttu-id="52bba-107">Создайте [бесплатную учетную запись Azure](https://aka.ms/K5y5yh), если у вас ее нет.</span><span class="sxs-lookup"><span data-stu-id="52bba-107">Open a [free Azure account](https://aka.ms/K5y5yh) if you don't have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="52bba-108">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="52bba-108">Create a web app</span></span>

<span data-ttu-id="52bba-109">На начальной странице Visual Studio последовательно выберите **Файл > Создать > Проект…**</span><span class="sxs-lookup"><span data-stu-id="52bba-109">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![меню "Файл"](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="52bba-111">В диалоговом окне **Создание проекта** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="52bba-111">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="52bba-112">В левой области выберите **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="52bba-112">In the left pane, select **.NET Core**.</span></span>
* <span data-ttu-id="52bba-113">В центральной области выберите **Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="52bba-113">In the center pane, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="52bba-114">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="52bba-114">Select **OK**.</span></span>

![Диалоговое окно создания нового проекта](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="52bba-116">В диалоговом окне **Создание веб-приложения ASP.NET Core** сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="52bba-116">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="52bba-117">Выберите **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="52bba-117">Select **Web Application**.</span></span>
* <span data-ttu-id="52bba-118">Выберите **Изменить способ проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="52bba-118">Select **Change Authentication**.</span></span>

![Диалоговое окно создания нового проекта](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="52bba-120">Появится диалоговое окно **Изменение способа проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="52bba-120">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="52bba-121">Выберите **Учетные записи отдельных пользователей**.</span><span class="sxs-lookup"><span data-stu-id="52bba-121">Select **Individual User Accounts**.</span></span>
* <span data-ttu-id="52bba-122">Нажмите кнопку **ОК**, чтобы вернуться на страницу **Создание веб-приложения ASP.NET Core**, а затем снова нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="52bba-122">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Диалоговое окно "Новый способ веб-проверки подлинности ASP.NET Core"](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="52bba-124">Visual Studio создает решение.</span><span class="sxs-lookup"><span data-stu-id="52bba-124">Visual Studio creates the solution.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="52bba-125">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="52bba-125">Run the app</span></span>

* <span data-ttu-id="52bba-126">Нажмите клавиши CTRL+F5, чтобы запустить проект.</span><span class="sxs-lookup"><span data-stu-id="52bba-126">Press CTRL+F5 to run the project.</span></span>
* <span data-ttu-id="52bba-127">Протестируйте ссылки **О программе** и **Контакт**.</span><span class="sxs-lookup"><span data-stu-id="52bba-127">Test the **About** and **Contact** links.</span></span>

![Веб-приложение откроется в localhost.](publish-to-azure-webapp-using-vs/_static/show.png)

### <a name="register-a-user"></a><span data-ttu-id="52bba-129">Регистрация пользователя</span><span class="sxs-lookup"><span data-stu-id="52bba-129">Register a user</span></span>

* <span data-ttu-id="52bba-130">Выберите **Зарегистрироваться** и зарегистрируйте нового пользователя.</span><span class="sxs-lookup"><span data-stu-id="52bba-130">Select **Register** and register a new user.</span></span> <span data-ttu-id="52bba-131">Можно использовать вымышленный адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="52bba-131">You can use a fictitious email address.</span></span> <span data-ttu-id="52bba-132">После отправки на странице отображается следующая ошибка.</span><span class="sxs-lookup"><span data-stu-id="52bba-132">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="52bba-133">*"Внутренняя ошибка сервера: не удалось выполнить операцию базы данных при обработке запроса. Исключения SQL: не удается открыть базу данных. Применение имеющихся миграций для контекста базы данных приложения, возможно, позволит решить проблему."*</span><span class="sxs-lookup"><span data-stu-id="52bba-133">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>
* <span data-ttu-id="52bba-134">Выберите **Применить миграции** и после обновления страницы перезагрузите ее.</span><span class="sxs-lookup"><span data-stu-id="52bba-134">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Внутренняя ошибка сервера: сбой операции из базы данных при обработке запроса.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="52bba-138">Приложение отобразит адрес электронной почты, который использовался для регистрации нового пользователя, и ссылку **Выйти**.</span><span class="sxs-lookup"><span data-stu-id="52bba-138">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Веб-приложение откроется в Microsoft Edge.](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="52bba-141">Развертывание приложения в Azure</span><span class="sxs-lookup"><span data-stu-id="52bba-141">Deploy the app to Azure</span></span>

<span data-ttu-id="52bba-142">В обозревателе решений щелкните правой кнопкой мыши проект и выберите команду **Опубликовать…**</span><span class="sxs-lookup"><span data-stu-id="52bba-142">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Открытое контекстное меню с выделенной ссылкой "Опубликовать"](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="52bba-144">В диалоговом окне **Публикация**:</span><span class="sxs-lookup"><span data-stu-id="52bba-144">In the **Publish** dialog:</span></span>

* <span data-ttu-id="52bba-145">Выберите **Служба приложений Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="52bba-145">Select **Microsoft Azure App Service**.</span></span>
* <span data-ttu-id="52bba-146">Выберите значок шестеренки и затем пункт **Создать профиль**.</span><span class="sxs-lookup"><span data-stu-id="52bba-146">Select the gear icon and then select **Create Profile**.</span></span>
* <span data-ttu-id="52bba-147">Выберите **Создать профиль**.</span><span class="sxs-lookup"><span data-stu-id="52bba-147">Select **Create Profile**.</span></span>

![Диалоговое окно публикации](publish-to-azure-webapp-using-vs/_static/maas1.png)

### <a name="create-azure-resources"></a><span data-ttu-id="52bba-149">Создание ресурсов Azure</span><span class="sxs-lookup"><span data-stu-id="52bba-149">Create Azure resources</span></span>

<span data-ttu-id="52bba-150">Отображается диалоговое окно **Создание приложения службы**:</span><span class="sxs-lookup"><span data-stu-id="52bba-150">The **Create App Service** dialog appears:</span></span>

* <span data-ttu-id="52bba-151">Введите свою подписку.</span><span class="sxs-lookup"><span data-stu-id="52bba-151">Enter your subscription.</span></span>
* <span data-ttu-id="52bba-152">Заполняются поля ввода **Имя приложения**, **Группа ресурсов** и **План службы приложений**.</span><span class="sxs-lookup"><span data-stu-id="52bba-152">The **App Name**, **Resource Group**, and **App Service Plan** entry fields are populated.</span></span> <span data-ttu-id="52bba-153">Вы можете сохранить эти имена или изменить их.</span><span class="sxs-lookup"><span data-stu-id="52bba-153">You can keep these names or change them.</span></span>

![Диалоговое окно "Служба приложений"](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="52bba-155">Выберите вкладку **Службы**, чтобы создать базу данных.</span><span class="sxs-lookup"><span data-stu-id="52bba-155">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="52bba-156">Выберите зеленый значок **+**, чтобы создать базу данных SQL.</span><span class="sxs-lookup"><span data-stu-id="52bba-156">Select the green **+** icon to create a new SQL Database</span></span>

![Новая база данных SQL](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="52bba-158">Щелкните **Создать…** в диалоговом окне **Настройка базы данных SQL**, чтобы создать базу данных.</span><span class="sxs-lookup"><span data-stu-id="52bba-158">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Новый сервер и база данных SQL](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="52bba-160">Появится диалоговое окно **Настройка SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="52bba-160">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="52bba-161">Введите имя пользователя и пароль администратора, а затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="52bba-161">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="52bba-162">Вы можете сохранить **Имя сервера** по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="52bba-162">You can keep the default **Server Name**.</span></span> 

> [!NOTE]
> <span data-ttu-id="52bba-163">"admin" запрещено использовать в качестве имени пользователя администратора.</span><span class="sxs-lookup"><span data-stu-id="52bba-163">"admin" isn't allowed as the administrator user name.</span></span>

![Диалоговое окно "Настройка SQL Server"](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="52bba-165">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="52bba-165">Select **OK**.</span></span>

<span data-ttu-id="52bba-166">Visual Studio вернет диалоговое окно **Создание службы приложений**.</span><span class="sxs-lookup"><span data-stu-id="52bba-166">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="52bba-167">Нажмите кнопку **Создать** в диалоговом окне **Создание службы приложений**.</span><span class="sxs-lookup"><span data-stu-id="52bba-167">Select **Create** on the **Create App Service** dialog.</span></span>

![Диалоговое окно "Настройка базы данных SQL"](publish-to-azure-webapp-using-vs/_static/conf_final.png)

<span data-ttu-id="52bba-169">Visual Studio создает веб-приложение и SQL Server в Azure.</span><span class="sxs-lookup"><span data-stu-id="52bba-169">Visual Studio creates the Web app and SQL Server on Azure.</span></span> <span data-ttu-id="52bba-170">Это может занять несколько минут.</span><span class="sxs-lookup"><span data-stu-id="52bba-170">This step can take a few minutes.</span></span> <span data-ttu-id="52bba-171">Сведения о созданных ресурсах см. в разделе [Дополнительные ресурсы](#additonal-resources).</span><span class="sxs-lookup"><span data-stu-id="52bba-171">For information on the resources created, see [Additonal resources](#additonal-resources).</span></span>

<span data-ttu-id="52bba-172">После завершения развертывания выберите **Параметры**:</span><span class="sxs-lookup"><span data-stu-id="52bba-172">When deployment completes, select **Settings**:</span></span>

![Диалоговое окно "Настройка SQL Server"](publish-to-azure-webapp-using-vs/_static/set.png)

<span data-ttu-id="52bba-174">На странице **Параметры** диалогового окна **Публикация**</span><span class="sxs-lookup"><span data-stu-id="52bba-174">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="52bba-175">Разверните раздел **Базы данных** и установите флажок **Использовать эту строку подключения во время выполнения**.</span><span class="sxs-lookup"><span data-stu-id="52bba-175">Expand **Databases** and check **Use this connection string at runtime**.</span></span>
  * <span data-ttu-id="52bba-176">Разверните раздел **Миграции Entity Framework** и установите флажок **Использовать эту миграцию при публикации**.</span><span class="sxs-lookup"><span data-stu-id="52bba-176">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="52bba-177">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="52bba-177">Select **Save**.</span></span> <span data-ttu-id="52bba-178">Visual Studio вернется в диалоговое окно **Публикация**.</span><span class="sxs-lookup"><span data-stu-id="52bba-178">Visual Studio returns to the **Publish** dialog.</span></span> 

![Диалоговое окно "Публикация": панель "Параметры"](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="52bba-180">Нажмите кнопку **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="52bba-180">Click **Publish**.</span></span> <span data-ttu-id="52bba-181">Visual Studio публикует приложение в Azure.</span><span class="sxs-lookup"><span data-stu-id="52bba-181">Visual Studio publishs your app to Azure.</span></span> <span data-ttu-id="52bba-182">По завершении развертывания приложение открывается в браузере.</span><span class="sxs-lookup"><span data-stu-id="52bba-182">When the depoyment completes, the app is opened in a browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="52bba-183">Тестирование приложения в Azure</span><span class="sxs-lookup"><span data-stu-id="52bba-183">Test your app in Azure</span></span>

* <span data-ttu-id="52bba-184">Протестируйте ссылки **О программе** и **Контакт**.</span><span class="sxs-lookup"><span data-stu-id="52bba-184">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="52bba-185">Регистрация нового пользователя</span><span class="sxs-lookup"><span data-stu-id="52bba-185">Register a new user</span></span>

![Веб-приложение, открытое в Microsoft Edge в службе приложений Azure](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="52bba-187">Обновление приложения</span><span class="sxs-lookup"><span data-stu-id="52bba-187">Update the app</span></span>

* <span data-ttu-id="52bba-188">Измените страницу Razor *Pages/About.cshtml* и ее содержимое.</span><span class="sxs-lookup"><span data-stu-id="52bba-188">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="52bba-189">Например, вы можете изменить абзац на "Hello ASP.NET Core!": [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="52bba-189">For example, you can modify the paragraph to say "Hello ASP.NET Core!": [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span></span>

* <span data-ttu-id="52bba-190">Щелкните правой кнопкой мыши проект и снова выберите пункт **Опубликовать...**.</span><span class="sxs-lookup"><span data-stu-id="52bba-190">Right-click on the project and select **Publish...** again.</span></span>

![Открытое контекстное меню с выделенной ссылкой "Опубликовать"](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="52bba-192">После публикации приложения убедитесь, что внесенные изменения доступны в Azure.</span><span class="sxs-lookup"><span data-stu-id="52bba-192">After the app is published, verify the changes you made are available on Azure.</span></span>

![Убедитесь, что задача завершена.](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="52bba-194">Очистка</span><span class="sxs-lookup"><span data-stu-id="52bba-194">Clean up</span></span>

<span data-ttu-id="52bba-195">Завершив тестирование приложения, перейдите на [портал Azure](https://portal.azure.com/) и удалите приложение.</span><span class="sxs-lookup"><span data-stu-id="52bba-195">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="52bba-196">Выберите пункт **Группы ресурсов**, а затем созданную группу ресурсов.</span><span class="sxs-lookup"><span data-stu-id="52bba-196">Select **Resource groups**, then select the resource group you created.</span></span>

![Портал Azure: пункт "Группы ресурсов" в меню боковой панели](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="52bba-198">На странице **Группы ресурсов** выберите **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="52bba-198">In the **Resource groups** page, select **Delete**.</span></span>

![Портал Azure: страница "Группы ресурсов"](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="52bba-200">Введите имя группы ресурсов и выберите **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="52bba-200">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="52bba-201">Ваше приложение и все ресурсы, созданные при работе с этим руководством, удалены из Azure.</span><span class="sxs-lookup"><span data-stu-id="52bba-201">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="52bba-202">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="52bba-202">Next steps</span></span>

* [<span data-ttu-id="52bba-203">Непрерывное развертывание в Azure с помощью Visual Studio и Git</span><span class="sxs-lookup"><span data-stu-id="52bba-203">Continuous Deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)

## <a name="additonal-resources"></a><span data-ttu-id="52bba-204">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="52bba-204">Additonal resources</span></span>

* <span data-ttu-id="52bba-205">[служба приложений Azure](https://docs.microsoft.com/azure/app-service/app-service-web-overview);</span><span class="sxs-lookup"><span data-stu-id="52bba-205">[Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-web-overview)</span></span>
* [<span data-ttu-id="52bba-206">Группа ресурсов Azure</span><span class="sxs-lookup"><span data-stu-id="52bba-206">Azure resource groups</span></span>](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups)
* [<span data-ttu-id="52bba-207">База данных SQL Azure</span><span class="sxs-lookup"><span data-stu-id="52bba-207">Azure SQL Database</span></span>](https://docs.microsoft.com/azure/sql-database/)
