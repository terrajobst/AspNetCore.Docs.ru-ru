---
title: "Непрерывное развертывание в Azure с помощью Visual Studio и Git"
author: rick-anderson
description: "Узнайте, как создать веб-приложение ASP.NET Core с помощью Visual Studio и развернуть его в службе приложений Azure, используя Git для непрерывного развертывания."
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 2707c7a8-2350-4304-9856-fda58e5c0a16
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/azure-continuous-deployment
ms.openlocfilehash: a9efad38b1c75bd3a186b4ec85861357ecf744b9
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a><span data-ttu-id="461b4-104">Непрерывное развертывание в Azure для ASP.NET Core с помощью Visual Studio и Git</span><span class="sxs-lookup"><span data-stu-id="461b4-104">Continuous deployment to Azure for ASP.NET Core, with Visual Studio and Git</span></span>

<span data-ttu-id="461b4-105">Автор: [Эрик Рейтан](https://github.com/Erikre) (Erik Reitan)</span><span class="sxs-lookup"><span data-stu-id="461b4-105">By [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="461b4-106">В этом руководстве показано, как создать веб-приложение ASP.NET Core с помощью Visual Studio и развернуть его из Visual Studio в службе приложений Azure, используя непрерывное развертывание.</span><span class="sxs-lookup"><span data-stu-id="461b4-106">This tutorial shows you how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span> 

<span data-ttu-id="461b4-107">См. также статью [Использование VSTS для сборки и публикации в веб-приложении Azure с помощью непрерывного развертывания](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic), в которой показано, как настроить рабочий процесс непрерывной поставки для [службы приложений Azure](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) с помощью Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="461b4-107">See also [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) using Visual Studio Team Services.</span></span> <span data-ttu-id="461b4-108">Непрерывная поставка Azure в Team Services упрощает настройку эффективного конвейера развертывания с целью публикации обновлений для приложения в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="461b4-108">Azure Continuous Delivery in Team Services simplifies setting up a robust deployment pipeline to publish updates for your app to Azure App Service.</span></span> <span data-ttu-id="461b4-109">Этот конвейер можно настроить на портале Azure для сборки, выполнения тестов, развертывания в промежуточном слоте и последующего развертывания в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="461b4-109">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot,  and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="461b4-110">Для работы с этим руководством требуется учетная запись Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="461b4-110">To complete this tutorial, you need a Microsoft Azure account.</span></span> <span data-ttu-id="461b4-111">Если у вас нет учетной записи, вы можете [активировать преимущества подписчика MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) или [зарегистрироваться для получения бесплатной версии](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="461b4-111">If you don't have an account, you can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="461b4-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="461b4-112">Prerequisites</span></span>

<span data-ttu-id="461b4-113">В этом руководстве предполагается, что вы уже установили следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="461b4-113">This tutorial assumes you have already installed the following:</span></span>

* [<span data-ttu-id="461b4-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="461b4-114">Visual Studio</span></span>](https://www.visualstudio.com)

* <span data-ttu-id="461b4-115">[ASP.NET Core](https://download.microsoft.com/download/F/6/E/F6ECBBCC-B02F-424E-8E03-D47E9FA631B7/DotNetCore.1.0.1-VS2015Tools.Preview2.0.3.exe) (среда выполнения и средства);</span><span class="sxs-lookup"><span data-stu-id="461b4-115">[ASP.NET Core](https://download.microsoft.com/download/F/6/E/F6ECBBCC-B02F-424E-8E03-D47E9FA631B7/DotNetCore.1.0.1-VS2015Tools.Preview2.0.3.exe) (runtime and tooling)</span></span>

* <span data-ttu-id="461b4-116">[Git](https://git-scm.com/downloads) для Windows.</span><span class="sxs-lookup"><span data-stu-id="461b4-116">[Git](https://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="461b4-117">Создание веб-приложения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="461b4-117">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="461b4-118">Запустите Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="461b4-118">Start Visual Studio.</span></span>

2. <span data-ttu-id="461b4-119">В меню **Файл** выберите пункт **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="461b4-119">From the **File** menu, select **New** > **Project**.</span></span>

3. <span data-ttu-id="461b4-120">Выберите шаблон проекта **Веб-приложение ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="461b4-120">Select the **ASP.NET Web Application** project template.</span></span> <span data-ttu-id="461b4-121">Он находится в разделе **Установленные** > **Шаблоны** > **Visual C#** > **Интернет**.</span><span class="sxs-lookup"><span data-stu-id="461b4-121">It appears under **Installed** > **Templates** > **Visual C#** > **Web**.</span></span> <span data-ttu-id="461b4-122">Задайте для проекта имя `SampleWebAppDemo`.</span><span class="sxs-lookup"><span data-stu-id="461b4-122">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="461b4-123">Выберите параметр **Создать репозиторий Git** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="461b4-123">Select the **Create new Git respository** option and click **OK**.</span></span>

   ![Диалоговое окно создания нового проекта](azure-continuous-deployment/_static/01-new-project.png)

4. <span data-ttu-id="461b4-125">В диалоговом окне **Новый проект ASP.NET** выберите **пустой** шаблон ASP.NET Core и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="461b4-125">In the **New ASP.NET Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![Диалоговое окно "Новый проект ASP.NET"](azure-continuous-deployment/_static/02-web-site-template.png)


### <a name="running-the-web-app-locally"></a><span data-ttu-id="461b4-127">Запуск веб-приложения в локальной системе</span><span class="sxs-lookup"><span data-stu-id="461b4-127">Running the web app locally</span></span>

1. <span data-ttu-id="461b4-128">После того как Visual Studio завершит создание приложения, запустите его, выбрав **Отладка** -> **Начать отладку**.</span><span class="sxs-lookup"><span data-stu-id="461b4-128">Once Visual Studio finishes creating the app, run the app by selecting **Debug** -> **Start Debugging**.</span></span> <span data-ttu-id="461b4-129">Вы также можете нажать клавишу **F5**.</span><span class="sxs-lookup"><span data-stu-id="461b4-129">As an alternative, you can press **F5**.</span></span>

   <span data-ttu-id="461b4-130">Инициализация Visual Studio и нового приложения может занять некоторое время.</span><span class="sxs-lookup"><span data-stu-id="461b4-130">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="461b4-131">По завершении запущенное приложение появится в браузере.</span><span class="sxs-lookup"><span data-stu-id="461b4-131">Once it is complete, the browser will show the running app.</span></span>

   ![Окно браузера с выполняющимся приложением, в котором выводится сообщение "Hello World!"](azure-continuous-deployment/_static/04-browser-runapp.png)

2. <span data-ttu-id="461b4-133">Посмотрев, как работает веб-приложение, закройте браузер и щелкните значок "Остановить отладку" на панели инструментов Visual Studio, чтобы остановить работу приложения.</span><span class="sxs-lookup"><span data-stu-id="461b4-133">After reviewing the running Web app, close the browser and click the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="461b4-134">Создание веб-приложения на портале Azure</span><span class="sxs-lookup"><span data-stu-id="461b4-134">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="461b4-135">Ниже приведены инструкции по созданию веб-приложения на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="461b4-135">The following steps will guide you through creating a web app in the Azure Portal.</span></span>

1. <span data-ttu-id="461b4-136">Войдите на [портал Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="461b4-136">Log in to the [Azure Portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="461b4-137">В левом верхнем углу страницы портала нажмите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="461b4-137">TAP **NEW** at the top left of the Portal</span></span>
3. <span data-ttu-id="461b4-138">Выберите **Интернет и мобильные устройства** > **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="461b4-138">TAP **Web + Mobile** > **Web App**</span></span>

    ![Портал Microsoft Azure > кнопка "Создать" > пункт "Интернет и мобильные устройства" в списке "Marketplace" > кнопка "Веб-приложение" в списке "Рекомендуемые приложения"](azure-continuous-deployment/_static/05-azure-newwebapp.png)

4.  <span data-ttu-id="461b4-140">В колонке **Веб-приложение** в поле **Имя службы приложений** введите уникальное значение.</span><span class="sxs-lookup"><span data-stu-id="461b4-140">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

    ![Колонка "Веб-приложение"](azure-continuous-deployment/_static/06-azure-newappblade.png)

    >[!NOTE]
    ><span data-ttu-id="461b4-142">**Имя службы приложений** должно быть уникальным.</span><span class="sxs-lookup"><span data-stu-id="461b4-142">The **App Service Name** name needs to be unique.</span></span> <span data-ttu-id="461b4-143">Это правило применяется при попытке ввести имя на портале.</span><span class="sxs-lookup"><span data-stu-id="461b4-143">The portal will enforce this rule when you attempt to enter the name.</span></span> <span data-ttu-id="461b4-144">После того как вы введете другое значение, вам потребуется подставить его вместо каждого экземпляра имени **SampleWebAppDemo**, который встретится вам в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="461b4-144">After you enter a different value, you'll need to substitute that value for each occurrence of **SampleWebAppDemo** that you see in this tutorial.</span></span>

    &nbsp;
    
    <span data-ttu-id="461b4-145">Кроме того, в колонке **Веб-приложение** выберите существующее **Расположение или план службы приложений** либо создайте собственный план или расположение.</span><span class="sxs-lookup"><span data-stu-id="461b4-145">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="461b4-146">При создании плана необходимо выбрать ценовую категорию, расположение и другие параметры.</span><span class="sxs-lookup"><span data-stu-id="461b4-146">If you create a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="461b4-147">Дополнительные сведения о планах службы приложений см. в статье [Подробный обзор планов службы приложений Azure](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).</span><span class="sxs-lookup"><span data-stu-id="461b4-147">For more information on App Service plans, [Azure App Service plans in-depth overview](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).</span></span>

5.  <span data-ttu-id="461b4-148">Нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="461b4-148">Click **Create**.</span></span> <span data-ttu-id="461b4-149">Azure подготовит и запустит веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="461b4-149">Azure will provision and start your web app.</span></span>

    ![Портал Azure: колонка "Основное" для приложения SampleWebAppDemo01](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="461b4-151">Включение публикации в Git для нового веб-приложения</span><span class="sxs-lookup"><span data-stu-id="461b4-151">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="461b4-152">Git — это распределенная система управления версиями, с помощью которой можно развернуть веб-приложение службы приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="461b4-152">Git is a distributed version control system that you can use to deploy your Azure App Service web app.</span></span> <span data-ttu-id="461b4-153">Вы будете сохранять создаваемый код веб-приложения в локальном репозитории Git и развертывать его в Azure, отправляя в удаленный репозиторий.</span><span class="sxs-lookup"><span data-stu-id="461b4-153">You'll store the code you write for your web app in a local Git repository, and you'll deploy your code to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="461b4-154">Войдите на [портал Azure](https://portal.azure.com), если вы этого еще не сделали.</span><span class="sxs-lookup"><span data-stu-id="461b4-154">Log into the [Azure Portal](https://portal.azure.com), if you're not already logged in.</span></span>

2. <span data-ttu-id="461b4-155">В нижней части области навигации нажмите кнопку **Обзор**.</span><span class="sxs-lookup"><span data-stu-id="461b4-155">Click **Browse**, located at the bottom of the navigation pane.</span></span>

3. <span data-ttu-id="461b4-156">Щелкните **Веб-приложения**, чтобы просмотреть список веб-приложений, связанных с вашей подпиской Azure.</span><span class="sxs-lookup"><span data-stu-id="461b4-156">Click **Web Apps** to view a list of the web apps associated with your Azure subscription.</span></span>

4. <span data-ttu-id="461b4-157">Выберите веб-приложение, созданное в предыдущем разделе этого руководства.</span><span class="sxs-lookup"><span data-stu-id="461b4-157">Select the web app you created in the previous section of this tutorial.</span></span>

5. <span data-ttu-id="461b4-158">Если колонка **Параметры** не отображается, выберите элемент **Параметры** в колонке **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="461b4-158">If the **Settings** blade is not shown, select **Settings** in the **Web App** blade.</span></span>

6. <span data-ttu-id="461b4-159">В колонке **Параметры** выберите элементы **Источник развертывания** > **Выбор источника** > **Локальный репозиторий Git**.</span><span class="sxs-lookup"><span data-stu-id="461b4-159">In the **Settings** blade, select **Deployment source** > **Choose Source** > **Local Git Repository**.</span></span>

   ![Колонка "Параметры" > колонка "Источник развертывания" > колонка "Выбор источника"](azure-continuous-deployment/_static/08-azure-localrepository.png)

7. <span data-ttu-id="461b4-161">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="461b4-161">Click **OK**.</span></span>

8. <span data-ttu-id="461b4-162">Если вы еще не настроили учетные данные развертывания для публикации веб-приложения или другого приложения службы приложений, сделайте это сейчас.</span><span class="sxs-lookup"><span data-stu-id="461b4-162">If you have not previously set up deployment credentials for publishing a web app or other App Service app, set them up now:</span></span>

   * <span data-ttu-id="461b4-163">Выберите **Параметры** > **Учетные данные развертывания**.</span><span class="sxs-lookup"><span data-stu-id="461b4-163">Click **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="461b4-164">Появится колонка **Установка учетных данных развертывания**.</span><span class="sxs-lookup"><span data-stu-id="461b4-164">The **Set deployment credentials** blade will be displayed.</span></span>

   * <span data-ttu-id="461b4-165">Создайте имя пользователя и пароль.</span><span class="sxs-lookup"><span data-stu-id="461b4-165">Create a user name and password.</span></span>  <span data-ttu-id="461b4-166">Этот пароль понадобится позднее при настройке Git.</span><span class="sxs-lookup"><span data-stu-id="461b4-166">You'll need this password later when setting up Git.</span></span>

   * <span data-ttu-id="461b4-167">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="461b4-167">Click **Save**.</span></span>

9. <span data-ttu-id="461b4-168">В колонке **Веб-приложение** выберите **Параметры** > **Свойства**.</span><span class="sxs-lookup"><span data-stu-id="461b4-168">In the **Web App** blade, click **Settings** > **Properties**.</span></span> <span data-ttu-id="461b4-169">URL-адрес удаленного репозитория Git, в котором будет развертываться приложение, приводится в поле **URL-адрес Git**.</span><span class="sxs-lookup"><span data-stu-id="461b4-169">The URL of the remote Git repository that you'll deploy to is shown under **GIT URL**.</span></span>

10. <span data-ttu-id="461b4-170">Скопируйте значение в поле **URL-адрес Git**, так как оно понадобится далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="461b4-170">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Портал Azure > колонка свойств приложения](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-your-web-app-to-azure-app-service"></a><span data-ttu-id="461b4-172">Публикация веб-приложения в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="461b4-172">Publish your web app to Azure App Service</span></span>

<span data-ttu-id="461b4-173">В этом разделе вы создадите локальный репозиторий Git с помощью Visual Studio и отправите код из него в Azure, чтобы развернуть веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="461b4-173">In this section, you will create a local Git repository using Visual Studio and push from that repository to Azure to deploy your web app.</span></span> <span data-ttu-id="461b4-174">Для этого потребуется выполнить указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="461b4-174">The steps involved include the following:</span></span>

   * <span data-ttu-id="461b4-175">Добавьте параметр удаленного репозитория, используя значение URL-адреса GIT, чтобы развернуть локальный репозиторий в Azure.</span><span class="sxs-lookup"><span data-stu-id="461b4-175">Add the remote repository setting using your GIT URL value, so you can deploy your local repository to Azure.</span></span>

   * <span data-ttu-id="461b4-176">Зафиксируйте изменения, внесенные в проект.</span><span class="sxs-lookup"><span data-stu-id="461b4-176">Commit your project changes.</span></span>

   * <span data-ttu-id="461b4-177">Отправьте изменения, внесенные в проект, из локального репозитория в удаленный репозиторий в Azure.</span><span class="sxs-lookup"><span data-stu-id="461b4-177">Push your project changes from your local repository to your remote repository on Azure.</span></span>

&nbsp;
   
1.  <span data-ttu-id="461b4-178">В **обозревателе решений** щелкните правой кнопкой мыши элемент **Решение "SampleWebAppDemo"** и выберите пункт **Зафиксировать**.</span><span class="sxs-lookup"><span data-stu-id="461b4-178">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="461b4-179">Откроется **Team Explorer**.</span><span class="sxs-lookup"><span data-stu-id="461b4-179">The **Team Explorer** will be displayed.</span></span>

    ![Вкладка "Подключение" в Team Explorer](azure-continuous-deployment/_static/10-team-explorer.png)

2.  <span data-ttu-id="461b4-181">В **Team Explorer** щелкните значок **Главная** и выберите **Параметры** > **Параметры репозитория**.</span><span class="sxs-lookup"><span data-stu-id="461b4-181">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

3.  <span data-ttu-id="461b4-182">В разделе **Удаленные** окна **Параметры репозитория** нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="461b4-182">In the **Remotes** section of the **Repository Settings** select **Add**.</span></span> <span data-ttu-id="461b4-183">Откроется диалоговое окно **Добавить удаленный объект**.</span><span class="sxs-lookup"><span data-stu-id="461b4-183">The **Add Remote** dialog box will be displayed.</span></span>

4.  <span data-ttu-id="461b4-184">В качестве **имени** удаленного объекта укажите **Azure-SampleApp**.</span><span class="sxs-lookup"><span data-stu-id="461b4-184">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

5.  <span data-ttu-id="461b4-185">В поле **Извлечь** укажите **URL-адрес Git**, который вы скопировали ранее на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="461b4-185">Set the value for **Fetch** to the **Git URL** that you copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="461b4-186">Обратите внимание на то, что этот URL-адрес должен заканчиваться на **.git**.</span><span class="sxs-lookup"><span data-stu-id="461b4-186">Note that this is the URL that ends with **.git**.</span></span>

    ![Диалоговое окно "Изменение удаленного объекта"](azure-continuous-deployment/_static/11-add-remote.png)

    >[!NOTE]
    ><span data-ttu-id="461b4-188">Удаленный репозиторий можно также указать в **командном окне**. Для этого откройте **командное окно**, перейдите в каталог проекта и введите команду.</span><span class="sxs-lookup"><span data-stu-id="461b4-188">As an alternative, you can specify the remote repository from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering the command.</span></span> <span data-ttu-id="461b4-189">Пример: `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`</span><span class="sxs-lookup"><span data-stu-id="461b4-189">For example:`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`</span></span>

6.  <span data-ttu-id="461b4-190">Щелкните значок **Главная** и выберите **Параметры** > **Глобальные параметры**.</span><span class="sxs-lookup"><span data-stu-id="461b4-190">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="461b4-191">Необходимо указать свое имя и адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="461b4-191">Make sure you have your name and your email address set.</span></span> <span data-ttu-id="461b4-192">Также может потребоваться выбрать команду **Обновить**.</span><span class="sxs-lookup"><span data-stu-id="461b4-192">You may also need to select **Update**.</span></span>

7.  <span data-ttu-id="461b4-193">Выберите **Главная** > **Изменения**, чтобы вернуться к представлению **Изменения**.</span><span class="sxs-lookup"><span data-stu-id="461b4-193">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

8.  <span data-ttu-id="461b4-194">Введите сообщение о фиксации, например **Начальная отправка № 1**, и щелкните **Зафиксировать**.</span><span class="sxs-lookup"><span data-stu-id="461b4-194">Enter a commit message, such as **Initial Push #1** and click **Commit**.</span></span> <span data-ttu-id="461b4-195">В результате будет создана локальная *фиксация*.</span><span class="sxs-lookup"><span data-stu-id="461b4-195">This action will create a *commit* locally.</span></span> <span data-ttu-id="461b4-196">Далее ее необходимо *синхронизировать* с Azure.</span><span class="sxs-lookup"><span data-stu-id="461b4-196">Next, you need to *sync* with Azure.</span></span>

    ![Вкладка "Подключение" в Team Explorer](azure-continuous-deployment/_static/12-initial-commit.png)

    >[!NOTE]
    ><span data-ttu-id="461b4-198">Изменения можно также фиксировать в **командном окне**. Для этого откройте **командное окно**, перейдите в каталог проекта и введите команду git.</span><span class="sxs-lookup"><span data-stu-id="461b4-198">As an alternative, you can commit your changes from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering the git commands.</span></span> <span data-ttu-id="461b4-199">Пример:</span><span class="sxs-lookup"><span data-stu-id="461b4-199">For example:</span></span>
    >
    >`git add .`
    >
    >`git commit -am "Initial Push #1"`

9.  <span data-ttu-id="461b4-200">Выберите **Главная** > **Синхронизировать** > **Действия** > **Открыть командную строку**.</span><span class="sxs-lookup"><span data-stu-id="461b4-200">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="461b4-201">Откроется командная строка, в которой будет выбран каталог проекта.</span><span class="sxs-lookup"><span data-stu-id="461b4-201">The command prompt will open to your project directory.</span></span>

10.  <span data-ttu-id="461b4-202">Введите в командном окне следующую команду:</span><span class="sxs-lookup"><span data-stu-id="461b4-202">Enter the following command in the command window:</span></span>

    `git push -u Azure-SampleApp master`

11.  <span data-ttu-id="461b4-203">Введите пароль из **учетных данных развертывания**, которые вы создали ранее на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="461b4-203">Enter your Azure **deployment credentials** password that you created earlier in Azure.</span></span>

    >[!NOTE]
    ><span data-ttu-id="461b4-204">В процессе ввода пароль не отображается.</span><span class="sxs-lookup"><span data-stu-id="461b4-204">Your password will not be visible as you enter it.</span></span>

    <span data-ttu-id="461b4-205">Эта команда запустит процесс отправки локальных файлов проекта в Azure.</span><span class="sxs-lookup"><span data-stu-id="461b4-205">This command will start the process of pushing your local project files to Azure.</span></span> <span data-ttu-id="461b4-206">Выходные данные приведенной выше команды заканчиваются сообщением о том, что развертывание прошло успешно.</span><span class="sxs-lookup"><span data-stu-id="461b4-206">The output from the above command ends with a message that deployment was successful.</span></span>
        
    ```
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
    * [new branch]      master -> master
    Branch master set up to track remote branch master from Azure-SampleApp.
    ```
    > [!NOTE]
    > <span data-ttu-id="461b4-207">Если вам нужно работать над проектом совместно, возможно, следует отправлять изменения в [GitHub](https://github.com) между операциями отправки в Azure.</span><span class="sxs-lookup"><span data-stu-id="461b4-207">If you need to collaborate on a project, you should consider pushing to [GitHub](https://github.com) in between pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="461b4-208">Проверка активного развертывания</span><span class="sxs-lookup"><span data-stu-id="461b4-208">Verify the Active Deployment</span></span>

<span data-ttu-id="461b4-209">Вы можете проверить, было ли веб-приложение успешно передано из локальной среды в Azure.</span><span class="sxs-lookup"><span data-stu-id="461b4-209">You can verify that you successfully transferred the web app from your local environment to Azure.</span></span> <span data-ttu-id="461b4-210">Успешное развертывание должно быть указано в списке.</span><span class="sxs-lookup"><span data-stu-id="461b4-210">You'll see the listed successful deployment.</span></span>

1. <span data-ttu-id="461b4-211">На [портале Azure](https://portal.azure.com) выберите веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="461b4-211">In the [Azure Portal](https://portal.azure.com), select your web app.</span></span> <span data-ttu-id="461b4-212">Затем выберите **Параметры** > **Непрерывное развертывание**.</span><span class="sxs-lookup"><span data-stu-id="461b4-212">Then, select **Settings** > **Continuous deployment**.</span></span>

   ![Портал Azure > колонка "Параметры" > колонка "Развертывания", содержащая успешное развертывание](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="461b4-214">Запуск приложения в Azure</span><span class="sxs-lookup"><span data-stu-id="461b4-214">Run the app in Azure</span></span>

<span data-ttu-id="461b4-215">Развернув веб-приложение в Azure, можно запустить его.</span><span class="sxs-lookup"><span data-stu-id="461b4-215">Now that you have deployed your web app to Azure, you can run the app.</span></span>

<span data-ttu-id="461b4-216">Это можно сделать двумя способами.</span><span class="sxs-lookup"><span data-stu-id="461b4-216">This can be done in two ways:</span></span>

* <span data-ttu-id="461b4-217">На портале Azure найдите колонку соответствующего веб-приложения и нажмите **Обзор**, чтобы открыть приложение в браузере по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="461b4-217">In the Azure Portal, locate the web app blade for your web app, and click **Browse** to view your app in your default browser.</span></span>

* <span data-ttu-id="461b4-218">Откройте браузер и введите URL-адрес веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="461b4-218">Open a browser and enter the URL for your web app.</span></span> <span data-ttu-id="461b4-219">Пример:</span><span class="sxs-lookup"><span data-stu-id="461b4-219">For example:</span></span>

  `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-your-web-app-and-republish"></a><span data-ttu-id="461b4-220">Обновление веб-приложения и повторная публикация</span><span class="sxs-lookup"><span data-stu-id="461b4-220">Update your web app and republish</span></span>

<span data-ttu-id="461b4-221">После внесения изменений в хранящийся локально код его можно опубликовать повторно.</span><span class="sxs-lookup"><span data-stu-id="461b4-221">After you make changes to your local code, you can republish.</span></span>

1.  <span data-ttu-id="461b4-222">В **обозревателе решений** Visual Studio откройте файл *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="461b4-222">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

2.  <span data-ttu-id="461b4-223">В методе `Configure` измените метод `Response.WriteAsync`, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="461b4-223">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

    ```aspx-cs
    await context.Response.WriteAsync("Hello World! Deploy to Azure.");
    ```
3.  <span data-ttu-id="461b4-224">Сохраните изменения в файле *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="461b4-224">Save changes to *Startup.cs*.</span></span>

4.  <span data-ttu-id="461b4-225">В **обозревателе решений** щелкните правой кнопкой мыши элемент **Решение "SampleWebAppDemo"** и выберите пункт **Зафиксировать**.</span><span class="sxs-lookup"><span data-stu-id="461b4-225">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="461b4-226">Откроется **Team Explorer**.</span><span class="sxs-lookup"><span data-stu-id="461b4-226">The **Team Explorer** will be displayed.</span></span>

5.  <span data-ttu-id="461b4-227">Введите сообщение о фиксации, например:</span><span class="sxs-lookup"><span data-stu-id="461b4-227">Enter a commit message, such as:</span></span>

    ```none
    Update #2
    ```

6.  <span data-ttu-id="461b4-228">Нажмите кнопку **Зафиксировать**, чтобы зафиксировать изменения проекта.</span><span class="sxs-lookup"><span data-stu-id="461b4-228">Press the **Commit** button to commit the project changes.</span></span>

7.  <span data-ttu-id="461b4-229">Выберите **Главная** > **Синхронизировать** > **Действия** > **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="461b4-229">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

>[!NOTE]
><span data-ttu-id="461b4-230">Изменения можно также отправлять в **командном окне**. Для этого откройте **командное окно**, перейдите в каталог проекта и введите команду git.</span><span class="sxs-lookup"><span data-stu-id="461b4-230">As an alternative, you can push your changes from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering a git command.</span></span> <span data-ttu-id="461b4-231">Пример:</span><span class="sxs-lookup"><span data-stu-id="461b4-231">For example:</span></span>
>
>`git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="461b4-232">Просмотр обновленного веб-приложения в Azure</span><span class="sxs-lookup"><span data-stu-id="461b4-232">View the updated web app in Azure</span></span>

<span data-ttu-id="461b4-233">Чтобы просмотреть обновленное веб-приложение, на портале Azure в колонке веб-приложения выберите **Обзор** или откройте браузер и введите URL-адрес веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="461b4-233">View your updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for your web app.</span></span> <span data-ttu-id="461b4-234">Пример:</span><span class="sxs-lookup"><span data-stu-id="461b4-234">For example:</span></span>

   `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a><span data-ttu-id="461b4-235">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="461b4-235">Additional Resources</span></span>

* [<span data-ttu-id="461b4-236">Публикация и развертывание</span><span class="sxs-lookup"><span data-stu-id="461b4-236">Publishing and Deployment</span></span>](index.md)

* [<span data-ttu-id="461b4-237">Проект Kudu</span><span class="sxs-lookup"><span data-stu-id="461b4-237">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
