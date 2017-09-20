---
title: "Публикация приложения ASP.NET Core в Azure с помощью Visual Studio"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 09/01/2017
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: 0c0ec1c7c1408b0460c594a47a3e5738cd170d5f
ms.sourcegitcommit: d022d4b96795ee473fa3847a1d8a8c7430423a86
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="a1164-103">Публикация веб-приложения ASP.NET Core в службе приложений Azure с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a1164-103">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="a1164-104">Авторы: [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT), [Сезар Блум Сильвейра (Cesar Blum Silveira)](https://github.com/cesarbs) и [Рейчел Аппель (Rachel Appel)](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="a1164-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

## <a name="set-up-the-development-environment"></a><span data-ttu-id="a1164-105">Настройка среды разработки</span><span class="sxs-lookup"><span data-stu-id="a1164-105">Set up the development environment</span></span>

* <span data-ttu-id="a1164-106">Установите последнюю версию [пакета Azure SDK для Visual Studio](https://www.visualstudio.com/vs/azure-tools/).</span><span class="sxs-lookup"><span data-stu-id="a1164-106">Install the latest [Azure SDK for Visual Studio](https://www.visualstudio.com/vs/azure-tools/).</span></span> <span data-ttu-id="a1164-107">При установке пакета SDK будет установлена Visual Studio, если это еще не сделано.</span><span class="sxs-lookup"><span data-stu-id="a1164-107">The SDK installs Visual Studio if you don't already have it.</span></span>

* <span data-ttu-id="a1164-108">Проверьте свою [учетную запись Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="a1164-108">Verify your [Azure account](https://portal.azure.com/).</span></span> <span data-ttu-id="a1164-109">Вы можете [открыть бесплатную учетную запись Azure](https://azure.microsoft.com/pricing/free-trial/) или [активировать преимущества для подписчиков Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="a1164-109">You can [open a free Azure account](https://azure.microsoft.com/pricing/free-trial/) or [Activate Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="a1164-110">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="a1164-110">Create a web app</span></span>

<span data-ttu-id="a1164-111">На начальной странице Visual Studio последовательно выберите **Файл > Создать > Проект…**</span><span class="sxs-lookup"><span data-stu-id="a1164-111">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![меню "Файл"](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="a1164-113">В диалоговом окне **Создание проекта** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="a1164-113">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="a1164-114">В левой области выберите **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="a1164-114">In the left pane, select **.NET Core**.</span></span>

* <span data-ttu-id="a1164-115">В центральной области выберите **Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="a1164-115">In the center pane, select **ASP.NET Core Web Application**.</span></span>

* <span data-ttu-id="a1164-116">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="a1164-116">Select **OK**.</span></span>

![Диалоговое окно создания нового проекта](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="a1164-118">В диалоговом окне **Создание веб-приложения ASP.NET Core** сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="a1164-118">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="a1164-119">Выберите **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="a1164-119">Select **Web Application**.</span></span>

* <span data-ttu-id="a1164-120">Выберите **Изменить способ проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="a1164-120">Select **Change Authentication**.</span></span>

![Диалоговое окно создания нового проекта](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="a1164-122">Появится диалоговое окно **Изменение способа проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="a1164-122">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="a1164-123">Выберите **Учетные записи отдельных пользователей**.</span><span class="sxs-lookup"><span data-stu-id="a1164-123">Select **Individual User Accounts**.</span></span>

* <span data-ttu-id="a1164-124">Нажмите кнопку **ОК**, чтобы вернуться на страницу **Создание веб-приложения ASP.NET Core**, а затем снова нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="a1164-124">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Диалоговое окно "Новый способ веб-проверки подлинности ASP.NET Core"](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="a1164-126">Visual Studio создает решение.</span><span class="sxs-lookup"><span data-stu-id="a1164-126">Visual Studio creates the solution.</span></span>

## <a name="run-the-app-locally"></a><span data-ttu-id="a1164-127">Локальный запуск приложения</span><span class="sxs-lookup"><span data-stu-id="a1164-127">Run the app locally</span></span>

* <span data-ttu-id="a1164-128">Выберите **Отладка**, а затем **Запустить без отладки**, чтобы запустить приложение локально.</span><span class="sxs-lookup"><span data-stu-id="a1164-128">Choose **Debug** then **Start Without Debugging** to run the app locally.</span></span>

* <span data-ttu-id="a1164-129">Щелкните ссылки **Сведения** и **Контакты**, чтобы проверить работу веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="a1164-129">Click the **About** and **Contact** links to verify the web application works.</span></span>

![Веб-приложение откроется в localhost.](publish-to-azure-webapp-using-vs/_static/show.png)

* <span data-ttu-id="a1164-131">Выберите **Зарегистрироваться** и зарегистрируйте нового пользователя.</span><span class="sxs-lookup"><span data-stu-id="a1164-131">Select **Register** and register a new user.</span></span> <span data-ttu-id="a1164-132">Можно использовать вымышленный адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="a1164-132">You can use a fictitious email address.</span></span> <span data-ttu-id="a1164-133">После отправки на странице отображается следующая ошибка.</span><span class="sxs-lookup"><span data-stu-id="a1164-133">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="a1164-134">*"Внутренняя ошибка сервера: не удалось выполнить операцию базы данных при обработке запроса. Исключения SQL: не удается открыть базу данных. Применение имеющихся миграций для контекста базы данных приложения, возможно, позволит решить проблему."*</span><span class="sxs-lookup"><span data-stu-id="a1164-134">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>

* <span data-ttu-id="a1164-135">Выберите **Применить миграции** и после обновления страницы перезагрузите ее.</span><span class="sxs-lookup"><span data-stu-id="a1164-135">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Внутренняя ошибка сервера: сбой операции из базы данных при обработке запроса.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="a1164-139">Приложение отобразит адрес электронной почты, который использовался для регистрации нового пользователя, и ссылку **Выйти**.</span><span class="sxs-lookup"><span data-stu-id="a1164-139">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Веб-приложение откроется в Microsoft Edge.](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="a1164-142">Развертывание приложения в Azure</span><span class="sxs-lookup"><span data-stu-id="a1164-142">Deploy the app to Azure</span></span>

<span data-ttu-id="a1164-143">Закройте веб-страницу, вернитесь в Visual Studio и выберите **Остановить отладку** в меню **Отладка**.</span><span class="sxs-lookup"><span data-stu-id="a1164-143">Close the web page, return to Visual Studio, and select **Stop Debugging** from the **Debug** menu.</span></span>

<span data-ttu-id="a1164-144">В обозревателе решений щелкните правой кнопкой мыши проект и выберите команду **Опубликовать…**</span><span class="sxs-lookup"><span data-stu-id="a1164-144">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Открытое контекстное меню с выделенной ссылкой "Опубликовать"](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="a1164-146">В диалоговом окне **Публикация** выберите **Служба приложений Microsoft Azure** и щелкните **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="a1164-146">In the **Publish** dialog, select **Microsoft Azure App Service** and click **Publish**.</span></span>

![Диалоговое окно публикации](publish-to-azure-webapp-using-vs/_static/maas1.png)

* <span data-ttu-id="a1164-148">Присвойте приложению уникальное имя.</span><span class="sxs-lookup"><span data-stu-id="a1164-148">Name the app a unique name.</span></span> 

* <span data-ttu-id="a1164-149">Выберите подписку.</span><span class="sxs-lookup"><span data-stu-id="a1164-149">Select a subscription.</span></span>

* <span data-ttu-id="a1164-150">Щелкните **Создать…** рядом с группой ресурсов и введите имя для новой группы ресурсов.</span><span class="sxs-lookup"><span data-stu-id="a1164-150">Select **New...** for the resource group and enter a name for the new resource group.</span></span>

* <span data-ttu-id="a1164-151">Щелкните **Создать…** рядом с планом службы приложений и выберите ближайшее к вам расположение.</span><span class="sxs-lookup"><span data-stu-id="a1164-151">Select **New...** for the app service plan and select a location near you.</span></span> <span data-ttu-id="a1164-152">Вы можете сохранить имя, созданное по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a1164-152">You can keep the name that is generated by default.</span></span>

![Диалоговое окно "Служба приложений"](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="a1164-154">Выберите вкладку **Службы**, чтобы создать базу данных.</span><span class="sxs-lookup"><span data-stu-id="a1164-154">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="a1164-155">Выберите зеленый значок **+**, чтобы создать базу данных SQL.</span><span class="sxs-lookup"><span data-stu-id="a1164-155">Select the green **+** icon to create a new SQL Database</span></span>

![Новая база данных SQL](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="a1164-157">Щелкните **Создать…** в диалоговом окне **Настройка базы данных SQL**, чтобы создать базу данных.</span><span class="sxs-lookup"><span data-stu-id="a1164-157">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Новый сервер и база данных SQL](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="a1164-159">Появится диалоговое окно **Настройка SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="a1164-159">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="a1164-160">Введите имя пользователя и пароль администратора, а затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="a1164-160">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="a1164-161">Не забудьте имя пользователя и пароль, созданные на этом шаге.</span><span class="sxs-lookup"><span data-stu-id="a1164-161">Don't forget the user name and password you create in this step.</span></span> <span data-ttu-id="a1164-162">Вы можете сохранить **Имя сервера** по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a1164-162">You can keep the default **Server Name**.</span></span> 

* <span data-ttu-id="a1164-163">Введите имена базы данных и строки подключения.</span><span class="sxs-lookup"><span data-stu-id="a1164-163">Enter names for the database and connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="a1164-164">"admin" не может использоваться в качестве имени пользователя администратора.</span><span class="sxs-lookup"><span data-stu-id="a1164-164">"admin" is not allowed as the administrator user name.</span></span>

![Диалоговое окно "Настройка SQL Server"](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="a1164-166">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="a1164-166">Select **OK**.</span></span>

<span data-ttu-id="a1164-167">Visual Studio вернет диалоговое окно **Создание службы приложений**.</span><span class="sxs-lookup"><span data-stu-id="a1164-167">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="a1164-168">Нажмите кнопку **Создать** в диалоговом окне **Создание службы приложений**.</span><span class="sxs-lookup"><span data-stu-id="a1164-168">Select **Create** on the **Create App Service** dialog.</span></span>

![Диалоговое окно "Настройка базы данных SQL"](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* <span data-ttu-id="a1164-170">Щелкните ссылку **Параметры** в диалоговом окне **Публикация**.</span><span class="sxs-lookup"><span data-stu-id="a1164-170">Click the **Settings** link in the **Publish** dialog.</span></span>

![Диалоговое окно "Публикация": панель "Подключение"](publish-to-azure-webapp-using-vs/_static/pubc.png)

<span data-ttu-id="a1164-172">На странице **Параметры** диалогового окна **Публикация**</span><span class="sxs-lookup"><span data-stu-id="a1164-172">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="a1164-173">Разверните раздел **Базы данных** и установите флажок **Использовать эту строку подключения во время выполнения**.</span><span class="sxs-lookup"><span data-stu-id="a1164-173">Expand **Databases** and check **Use this connection string at runtime**.</span></span>

  * <span data-ttu-id="a1164-174">Разверните раздел **Миграции Entity Framework** и установите флажок **Использовать эту миграцию при публикации**.</span><span class="sxs-lookup"><span data-stu-id="a1164-174">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="a1164-175">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="a1164-175">Select **Save**.</span></span> <span data-ttu-id="a1164-176">Visual Studio вернется в диалоговое окно **Публикация**.</span><span class="sxs-lookup"><span data-stu-id="a1164-176">Visual Studio returns to the **Publish** dialog.</span></span> 

![Диалоговое окно "Публикация": панель "Параметры"](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="a1164-178">Нажмите кнопку **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="a1164-178">Click **Publish**.</span></span> <span data-ttu-id="a1164-179">Visual Studio опубликует приложение в Azure и запустит облачное приложение в браузере.</span><span class="sxs-lookup"><span data-stu-id="a1164-179">Visual Studio will publish your app to Azure and launch the cloud app in your browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="a1164-180">Тестирование приложения в Azure</span><span class="sxs-lookup"><span data-stu-id="a1164-180">Test your app in Azure</span></span>

* <span data-ttu-id="a1164-181">Протестируйте ссылки **О программе** и **Контакт**.</span><span class="sxs-lookup"><span data-stu-id="a1164-181">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="a1164-182">Регистрация нового пользователя</span><span class="sxs-lookup"><span data-stu-id="a1164-182">Register a new user</span></span>

![Веб-приложение, открытое в Microsoft Edge в службе приложений Azure](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="a1164-184">Обновление приложения</span><span class="sxs-lookup"><span data-stu-id="a1164-184">Update the app</span></span>

* <span data-ttu-id="a1164-185">Измените страницу Razor *Pages/About.cshtml* и ее содержимое.</span><span class="sxs-lookup"><span data-stu-id="a1164-185">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="a1164-186">Например, вы можете изменить абзац на "Hello ASP.NET Core!":</span><span class="sxs-lookup"><span data-stu-id="a1164-186">For example, you can modify the paragraph to say "Hello ASP.NET Core!":</span></span>

    [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]

* <span data-ttu-id="a1164-187">Щелкните правой кнопкой мыши проект и снова выберите пункт **Опубликовать...**.</span><span class="sxs-lookup"><span data-stu-id="a1164-187">Right-click on the project and select **Publish...** again.</span></span>

![Открытое контекстное меню с выделенной ссылкой "Опубликовать"](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="a1164-189">После публикации приложения убедитесь, что внесенные изменения доступны в Azure.</span><span class="sxs-lookup"><span data-stu-id="a1164-189">After the app is published, verify the changes you made are available on Azure.</span></span>

![Убедитесь, что задача завершена.](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="a1164-191">Очистка</span><span class="sxs-lookup"><span data-stu-id="a1164-191">Clean up</span></span>

<span data-ttu-id="a1164-192">Завершив тестирование приложения, перейдите на [портал Azure](https://portal.azure.com/) и удалите приложение.</span><span class="sxs-lookup"><span data-stu-id="a1164-192">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="a1164-193">Выберите пункт **Группы ресурсов**, а затем созданную группу ресурсов.</span><span class="sxs-lookup"><span data-stu-id="a1164-193">Select **Resource groups**, then select the resource group you created.</span></span>

![Портал Azure: пункт "Группы ресурсов" в меню боковой панели](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="a1164-195">На странице **Группы ресурсов** выберите **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="a1164-195">In the **Resource groups** page, select **Delete**.</span></span>

![Портал Azure: страница "Группы ресурсов"](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="a1164-197">Введите имя группы ресурсов и выберите **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="a1164-197">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="a1164-198">Ваше приложение и все ресурсы, созданные при работе с этим руководством, удалены из Azure.</span><span class="sxs-lookup"><span data-stu-id="a1164-198">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="a1164-199">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="a1164-199">Next steps</span></span>

* [<span data-ttu-id="a1164-200">Начало работы с ASP.NET Core MVC и Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a1164-200">Getting started with ASP.NET Core MVC and Visual Studio</span></span>](first-mvc-app/start-mvc.md)

* [<span data-ttu-id="a1164-201">Введение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a1164-201">Introduction to ASP.NET Core</span></span>](../index.md)

* [<span data-ttu-id="a1164-202">Основы</span><span class="sxs-lookup"><span data-stu-id="a1164-202">Fundamentals</span></span>](../fundamentals/index.md)
