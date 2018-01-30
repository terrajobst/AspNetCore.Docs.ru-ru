---
title: "Непрерывное развертывание в Azure с помощью Visual Studio и Git"
author: rick-anderson
description: "Сведения о создании веб-приложения ASP.NET Core с помощью Visual Studio и его развертывании в службе приложений Azure с использованием Git для непрерывного развертывания."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: 78f4eed188323f2f43fafbb69d3fca9b59129ad2
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a><span data-ttu-id="13b44-103">Непрерывное развертывание в Azure ASP.NET Core с Visual Studio и Git</span><span class="sxs-lookup"><span data-stu-id="13b44-103">Continuous deployment to Azure for ASP.NET Core with Visual Studio and Git</span></span>

<span data-ttu-id="13b44-104">Автор: [Эрик Рейтан](https://github.com/Erikre) (Erik Reitan)</span><span class="sxs-lookup"><span data-stu-id="13b44-104">By [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="13b44-105">Этого учебника показано, как создать веб-приложение ASP.NET Core с помощью Visual Studio и развернуть ее на службе приложений Azure из Visual Studio с помощью непрерывного развертывания.</span><span class="sxs-lookup"><span data-stu-id="13b44-105">This tutorial shows how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span>

<span data-ttu-id="13b44-106">См. также статью [Использование VSTS для сборки и публикации в веб-приложении Azure с помощью непрерывного развертывания](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), в которой показано, как настроить рабочий процесс непрерывной поставки для [службы приложений Azure](/azure/app-service/app-service-web-overview) с помощью Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="13b44-106">See also [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](/azure/app-service/app-service-web-overview) using Visual Studio Team Services.</span></span> <span data-ttu-id="13b44-107">Azure непрерывной доставки в Team Services упрощает настройку конвейера высокого уровня надежности системы, для публикации обновлений для приложений, размещенных в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="13b44-107">Azure Continuous Delivery in Team Services simplifies setting up a robust deployment pipeline to publish updates for apps hosted in Azure App Service.</span></span> <span data-ttu-id="13b44-108">Конвейер можно настроить на портале Azure для создания, выполнения тестов, развертывания промежуточный слот и затем развернуть в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="13b44-108">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot, and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="13b44-109">Для работы с этим учебником требуется учетная запись Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="13b44-109">To complete this tutorial, a Microsoft Azure account is required.</span></span> <span data-ttu-id="13b44-110">Чтобы получить учетную запись, [активировать преимущества для подписчиков MSDN](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) или [зарегистрироваться для получения бесплатной пробной версии](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="13b44-110">To obtain an account, [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="13b44-111">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="13b44-111">Prerequisites</span></span>

<span data-ttu-id="13b44-112">В этом учебнике предполагается, что установлено следующее программное обеспечение:</span><span class="sxs-lookup"><span data-stu-id="13b44-112">This tutorial assumes the following software is installed:</span></span>

* [<span data-ttu-id="13b44-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="13b44-113">Visual Studio</span></span>](https://www.visualstudio.com)
* <span data-ttu-id="13b44-114">[Пакет SDK для .NET core](https://www.microsoft.com/net/download/core) (среда выполнения и средства)</span><span class="sxs-lookup"><span data-stu-id="13b44-114">[.NET Core SDK](https://www.microsoft.com/net/download/core) (runtime and tooling)</span></span>
* <span data-ttu-id="13b44-115">[Git](https://git-scm.com/downloads) для Windows.</span><span class="sxs-lookup"><span data-stu-id="13b44-115">[Git](https://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="13b44-116">Создание веб-приложения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="13b44-116">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="13b44-117">Запустите Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="13b44-117">Start Visual Studio.</span></span>

1. <span data-ttu-id="13b44-118">В меню **Файл** выберите пункт **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="13b44-118">From the **File** menu, select **New** > **Project**.</span></span>

1. <span data-ttu-id="13b44-119">Выберите шаблон проекта **Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="13b44-119">Select the **ASP.NET Core Web Application** project template.</span></span> <span data-ttu-id="13b44-120">Он находится в разделе **Installed** (Установлено) > **Templates** (Шаблоны) > **Visual C#** > **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="13b44-120">It appears under **Installed** > **Templates** > **Visual C#** > **.NET Core**.</span></span> <span data-ttu-id="13b44-121">Задайте для проекта имя `SampleWebAppDemo`.</span><span class="sxs-lookup"><span data-stu-id="13b44-121">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="13b44-122">Выберите параметр **Создать репозиторий Git** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="13b44-122">Select the **Create new Git respository** option and click **OK**.</span></span>

   ![Диалоговое окно создания нового проекта](azure-continuous-deployment/_static/01-new-project.png)

1. <span data-ttu-id="13b44-124">В диалоговом окне **New ASP.NET Core Project** (Новый проект ASP.NET Core) выберите **пустой** шаблон и щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="13b44-124">In the **New ASP.NET Core Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![Диалоговое окно "Новый проект ASP.NET"](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> <span data-ttu-id="13b44-126">Последний выпуск .NET Core является версия 2.0.</span><span class="sxs-lookup"><span data-stu-id="13b44-126">The most recent release of .NET Core is 2.0.</span></span>

### <a name="running-the-web-app-locally"></a><span data-ttu-id="13b44-127">Запуск веб-приложения в локальной системе</span><span class="sxs-lookup"><span data-stu-id="13b44-127">Running the web app locally</span></span>

1. <span data-ttu-id="13b44-128">После того как Visual Studio завершит создание приложения, запустите его, выбрав **Отладка** > **Начать отладку**.</span><span class="sxs-lookup"><span data-stu-id="13b44-128">Once Visual Studio finishes creating the app, run the app by selecting **Debug** > **Start Debugging**.</span></span> <span data-ttu-id="13b44-129">В качестве альтернативы нажать **F5**.</span><span class="sxs-lookup"><span data-stu-id="13b44-129">As an alternative, press **F5**.</span></span>

   <span data-ttu-id="13b44-130">Инициализация Visual Studio и нового приложения может занять некоторое время.</span><span class="sxs-lookup"><span data-stu-id="13b44-130">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="13b44-131">После завершения браузер показано выполняющееся приложение.</span><span class="sxs-lookup"><span data-stu-id="13b44-131">Once it's complete, the browser shows the running app.</span></span>

   ![Окно браузера с выполняющимся приложением, в котором выводится сообщение "Hello World!"](azure-continuous-deployment/_static/04-browser-runapp.png)

1. <span data-ttu-id="13b44-133">Ознакомившись с запущенной веб-приложения, закройте браузер и щелкните значок «Остановить отладку» на панели инструментов Visual Studio, чтобы остановить выполнение приложения.</span><span class="sxs-lookup"><span data-stu-id="13b44-133">After reviewing the running Web app, close the browser and select the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="13b44-134">Создание веб-приложения на портале Azure</span><span class="sxs-lookup"><span data-stu-id="13b44-134">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="13b44-135">Следующие шаги создания веб-приложения на портале Azure:</span><span class="sxs-lookup"><span data-stu-id="13b44-135">The following steps create a web app in the Azure Portal:</span></span>

1. <span data-ttu-id="13b44-136">Войдите на [портал Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="13b44-136">Log in to the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="13b44-137">Выберите **NEW** в верхней левой части портала интерфейса.</span><span class="sxs-lookup"><span data-stu-id="13b44-137">Select **NEW** at the top left of the portal interface.</span></span>

1. <span data-ttu-id="13b44-138">Выберите **Интернет + мобильные устройства** > **веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="13b44-138">Select **Web + Mobile** > **Web App**.</span></span>

   ![Портал Microsoft Azure > кнопка "Создать" > пункт "Интернет и мобильные устройства" в списке "Marketplace" > кнопка "Веб-приложение" в списке "Рекомендуемые приложения"](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. <span data-ttu-id="13b44-140">В колонке **Веб-приложение** в поле **Имя службы приложений** введите уникальное значение.</span><span class="sxs-lookup"><span data-stu-id="13b44-140">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

   ![Колонка "Веб-приложение"](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > <span data-ttu-id="13b44-142">**Имя приложения службы** имя должно быть уникальным.</span><span class="sxs-lookup"><span data-stu-id="13b44-142">The **App Service Name** name must be unique.</span></span> <span data-ttu-id="13b44-143">Портал применяет это правило, если указано имя.</span><span class="sxs-lookup"><span data-stu-id="13b44-143">The portal enforces this rule when the name is provided.</span></span> <span data-ttu-id="13b44-144">Если предоставить другое значение, замените это значение для каждого вхождения **SampleWebAppDemo** в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="13b44-144">If providing a different value, substitute that value for each occurrence of **SampleWebAppDemo** in this tutorial.</span></span>

   <span data-ttu-id="13b44-145">Кроме того, в колонке **Веб-приложение** выберите существующее **Расположение или план службы приложений** либо создайте собственный план или расположение.</span><span class="sxs-lookup"><span data-stu-id="13b44-145">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="13b44-146">Если создается новый план, выберите ценовую категорию, расположение и другие параметры.</span><span class="sxs-lookup"><span data-stu-id="13b44-146">If creating a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="13b44-147">Дополнительные сведения о планах службы приложений см. в разделе [исчерпывающий обзор планы службы приложений Azure](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span><span class="sxs-lookup"><span data-stu-id="13b44-147">For more information on App Service plans, see [Azure App Service plans in-depth overview](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span></span>

1. <span data-ttu-id="13b44-148">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="13b44-148">Select **Create**.</span></span> <span data-ttu-id="13b44-149">Подготовка Azure и запустить веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="13b44-149">Azure will provision and start the web app.</span></span>

   ![Портал Azure: колонка "Основное" для приложения SampleWebAppDemo01](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="13b44-151">Включение публикации в Git для нового веб-приложения</span><span class="sxs-lookup"><span data-stu-id="13b44-151">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="13b44-152">Git — это система управления распределенных версии, который может использоваться для развертывания веб-приложение службы приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="13b44-152">Git is a distributed version control system that can be used to deploy an Azure App Service web app.</span></span> <span data-ttu-id="13b44-153">Кода веб-приложения хранятся в локальном репозитории, а код развертывается в Azure с помощью принудительной отправки в удаленном репозитории.</span><span class="sxs-lookup"><span data-stu-id="13b44-153">Web app code is stored in a local Git repository, and the code is deployed to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="13b44-154">Войдите на [портал Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="13b44-154">Log into the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="13b44-155">Выберите **службы приложений** для просмотра списка приложений служб, связанных с подпиской Azure.</span><span class="sxs-lookup"><span data-stu-id="13b44-155">Select **App Services** to view a list of the app services associated with the Azure subscription.</span></span>

1. <span data-ttu-id="13b44-156">Выберите веб-приложения, созданные в предыдущем разделе этого учебника.</span><span class="sxs-lookup"><span data-stu-id="13b44-156">Select the web app created in the previous section of this tutorial.</span></span>

1. <span data-ttu-id="13b44-157">В колонке **Deployment** (Развертывание) выберите **Deployment options** (Параметры развертывания) > **Выбор источника** > **Локальный репозиторий Git**.</span><span class="sxs-lookup"><span data-stu-id="13b44-157">In the **Deployment** blade, select **Deployment options** > **Choose Source** > **Local Git Repository**.</span></span>

   ![Колонка "Параметры" > колонка "Источник развертывания" > колонка "Выбор источника"](azure-continuous-deployment/_static/deployment-options.png)

1. <span data-ttu-id="13b44-159">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="13b44-159">Select **OK**.</span></span>

1. <span data-ttu-id="13b44-160">Если учетные данные развертывания для публикации веб-приложение или другое приложение службы приложений ранее не задан, настройте их сейчас:</span><span class="sxs-lookup"><span data-stu-id="13b44-160">If deployment credentials for publishing a web app or other App Service app haven't previously been set up, set them up now:</span></span>

   * <span data-ttu-id="13b44-161">Выберите **параметры** > **учетные данные развертывания**.</span><span class="sxs-lookup"><span data-stu-id="13b44-161">Select **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="13b44-162">**Задать учетные данные развертывания** колонке отображается.</span><span class="sxs-lookup"><span data-stu-id="13b44-162">The **Set deployment credentials** blade is displayed.</span></span>
   * <span data-ttu-id="13b44-163">Создайте имя пользователя и пароль.</span><span class="sxs-lookup"><span data-stu-id="13b44-163">Create a user name and password.</span></span> <span data-ttu-id="13b44-164">Сохраните пароль для последующего использования при настройке Git.</span><span class="sxs-lookup"><span data-stu-id="13b44-164">Save the password for later use when setting up Git.</span></span>
   * <span data-ttu-id="13b44-165">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="13b44-165">Select **Save**.</span></span>

1. <span data-ttu-id="13b44-166">В **веб-приложения** колонке выберите **параметры** > **свойства**.</span><span class="sxs-lookup"><span data-stu-id="13b44-166">In the **Web App** blade, select **Settings** > **Properties**.</span></span> <span data-ttu-id="13b44-167">URL-адрес в удаленный репозиторий Git, для развертывания отображается в разделе **URL-адрес GIT**.</span><span class="sxs-lookup"><span data-stu-id="13b44-167">The URL of the remote Git repository to deploy to is shown under **GIT URL**.</span></span>

1. <span data-ttu-id="13b44-168">Скопируйте значение в поле **URL-адрес Git**, так как оно понадобится далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="13b44-168">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Портал Azure > колонка свойств приложения](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a><span data-ttu-id="13b44-170">Опубликовать веб-приложения в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="13b44-170">Publish the web app to Azure App Service</span></span>

<span data-ttu-id="13b44-171">В этом разделе создайте локальный репозиторий Git с помощью Visual Studio и push из этого репозитория в Azure для развертывания веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="13b44-171">In this section, create a local Git repository using Visual Studio and push from that repository to Azure to deploy the web app.</span></span> <span data-ttu-id="13b44-172">Для этого потребуется выполнить указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="13b44-172">The steps involved include the following:</span></span>

* <span data-ttu-id="13b44-173">Добавьте параметр удаленный репозиторий, используя значение URL-адрес GIT, чтобы можно было развернуть локальный репозиторий в Azure.</span><span class="sxs-lookup"><span data-stu-id="13b44-173">Add the remote repository setting using the GIT URL value, so the local repository can be deployed to Azure.</span></span>
* <span data-ttu-id="13b44-174">Зафиксируйте изменения проекта.</span><span class="sxs-lookup"><span data-stu-id="13b44-174">Commit project changes.</span></span>
* <span data-ttu-id="13b44-175">Отправьте изменения проекта из локального репозитория в удаленном репозитории в Azure.</span><span class="sxs-lookup"><span data-stu-id="13b44-175">Push project changes from the local repository to the remote repository on Azure.</span></span>

1. <span data-ttu-id="13b44-176">В **обозревателе решений** щелкните правой кнопкой мыши элемент **Решение "SampleWebAppDemo"** и выберите пункт **Зафиксировать**.</span><span class="sxs-lookup"><span data-stu-id="13b44-176">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="13b44-177">**Team Explorer** отображается.</span><span class="sxs-lookup"><span data-stu-id="13b44-177">The **Team Explorer** is displayed.</span></span>

   ![Вкладка "Подключение" в Team Explorer](azure-continuous-deployment/_static/10-team-explorer.png)

1. <span data-ttu-id="13b44-179">В **Team Explorer** щелкните значок **Главная** и выберите **Параметры** > **Параметры репозитория**.</span><span class="sxs-lookup"><span data-stu-id="13b44-179">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

1. <span data-ttu-id="13b44-180">В **удаленные элементы** раздел **параметров репозитория**выберите **добавить**.</span><span class="sxs-lookup"><span data-stu-id="13b44-180">In the **Remotes** section of the **Repository Settings**, select **Add**.</span></span> <span data-ttu-id="13b44-181">**Добавьте удаленный** диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="13b44-181">The **Add Remote** dialog box is displayed.</span></span>

1. <span data-ttu-id="13b44-182">В качестве **имени** удаленного объекта укажите **Azure-SampleApp**.</span><span class="sxs-lookup"><span data-stu-id="13b44-182">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

1. <span data-ttu-id="13b44-183">Задайте значение **выборки** для **URL-адрес Git** , скопированные из Azure ранее в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="13b44-183">Set the value for **Fetch** to the **Git URL** that copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="13b44-184">Обратите внимание на то, что этот URL-адрес должен заканчиваться на **.git**.</span><span class="sxs-lookup"><span data-stu-id="13b44-184">Note that this is the URL that ends with **.git**.</span></span>

   ![Диалоговое окно "Изменение удаленного объекта"](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > <span data-ttu-id="13b44-186">Кроме того, укажите удаленный репозиторий, из **командное окно** , открыв **командное окно**, изменения в каталог проекта и ввести команду.</span><span class="sxs-lookup"><span data-stu-id="13b44-186">As an alternative, specify the remote repository from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the command.</span></span> <span data-ttu-id="13b44-187">Пример</span><span class="sxs-lookup"><span data-stu-id="13b44-187">Example:</span></span>
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. <span data-ttu-id="13b44-188">Щелкните значок **Главная** и выберите **Параметры** > **Глобальные параметры**.</span><span class="sxs-lookup"><span data-stu-id="13b44-188">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="13b44-189">Убедитесь, что заданы имя и адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="13b44-189">Confirm that the name and email address are set.</span></span> <span data-ttu-id="13b44-190">Выберите **обновление** при необходимости.</span><span class="sxs-lookup"><span data-stu-id="13b44-190">Select **Update** if required.</span></span>

1. <span data-ttu-id="13b44-191">Выберите **Главная** > **Изменения**, чтобы вернуться к представлению **Изменения**.</span><span class="sxs-lookup"><span data-stu-id="13b44-191">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

1. <span data-ttu-id="13b44-192">Введите сообщение фиксации, например **начальной Push #1** и выберите **фиксации**.</span><span class="sxs-lookup"><span data-stu-id="13b44-192">Enter a commit message, such as **Initial Push #1** and select **Commit**.</span></span> <span data-ttu-id="13b44-193">Это действие создает *фиксации* локально.</span><span class="sxs-lookup"><span data-stu-id="13b44-193">This action creates a *commit* locally.</span></span>

   ![Вкладка "Подключение" в Team Explorer](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > <span data-ttu-id="13b44-195">В качестве альтернативы фиксации меняется с **командное окно** , открыв **командное окно**, изменения в каталог проекта и ввод команд git.</span><span class="sxs-lookup"><span data-stu-id="13b44-195">As an alternative, commit changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the git commands.</span></span> <span data-ttu-id="13b44-196">Пример</span><span class="sxs-lookup"><span data-stu-id="13b44-196">Example:</span></span>
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. <span data-ttu-id="13b44-197">Выберите **Главная** > **Синхронизировать** > **Действия** > **Открыть командную строку**.</span><span class="sxs-lookup"><span data-stu-id="13b44-197">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="13b44-198">Откроется командная строка в каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="13b44-198">The command prompt opens to the project directory.</span></span>

1. <span data-ttu-id="13b44-199">Введите в командном окне следующую команду:</span><span class="sxs-lookup"><span data-stu-id="13b44-199">Enter the following command in the command window:</span></span>

   `git push -u Azure-SampleApp master`

1. <span data-ttu-id="13b44-200">Введите Azure **учетные данные развертывания** пароль, созданный ранее в Azure.</span><span class="sxs-lookup"><span data-stu-id="13b44-200">Enter the Azure **deployment credentials** password created earlier in Azure.</span></span>

   <span data-ttu-id="13b44-201">Эта команда запускает процесс передачи файлов локального проекта в Azure.</span><span class="sxs-lookup"><span data-stu-id="13b44-201">This command starts the process of pushing the local project files to Azure.</span></span> <span data-ttu-id="13b44-202">Выходные данные этой команды оканчивается сообщение о том, что развертывание успешно.</span><span class="sxs-lookup"><span data-stu-id="13b44-202">The output from the above command ends with a message that the deployment was successful.</span></span>

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > <span data-ttu-id="13b44-203">Если требуется совместная работа над проектом, распределенного запроса передает [GitHub](https://github.com) до отправки в Azure.</span><span class="sxs-lookup"><span data-stu-id="13b44-203">If collaboration on the project is required, consider pushing to [GitHub](https://github.com) before pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="13b44-204">Проверка активного развертывания</span><span class="sxs-lookup"><span data-stu-id="13b44-204">Verify the Active Deployment</span></span>

<span data-ttu-id="13b44-205">Проверьте успешность выполнения передачи приложения web от локальной среды в Azure.</span><span class="sxs-lookup"><span data-stu-id="13b44-205">Verify that the web app transfer from the local environment to Azure is successful.</span></span>

<span data-ttu-id="13b44-206">В [портала Azure](https://portal.azure.com), выберите веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="13b44-206">In the [Azure Portal](https://portal.azure.com), select the web app.</span></span> <span data-ttu-id="13b44-207">Выберите **развертывания** > **варианты развертывания**.</span><span class="sxs-lookup"><span data-stu-id="13b44-207">Select **Deployment** > **Deployment options**.</span></span>

![Портал Azure > колонка "Параметры" > колонка "Развертывания", содержащая успешное развертывание](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="13b44-209">Запуск приложения в Azure</span><span class="sxs-lookup"><span data-stu-id="13b44-209">Run the app in Azure</span></span>

<span data-ttu-id="13b44-210">Теперь, когда веб-приложение развертывается в Azure, запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="13b44-210">Now that the web app is deployed to Azure, run the app.</span></span>

<span data-ttu-id="13b44-211">Это можно сделать двумя способами:</span><span class="sxs-lookup"><span data-stu-id="13b44-211">This can be accomplished in two ways:</span></span>

* <span data-ttu-id="13b44-212">На портале Azure найдите колонка веб приложения для веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="13b44-212">In the Azure Portal, locate the web app blade for the web app.</span></span> <span data-ttu-id="13b44-213">Выберите **Обзор** для просмотра приложения в браузере по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="13b44-213">Select **Browse** to view the app in the default browser.</span></span>
* <span data-ttu-id="13b44-214">Откройте браузер и введите URL-адрес для веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="13b44-214">Open a browser and enter the URL for the web app.</span></span> <span data-ttu-id="13b44-215">Пример: `http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="13b44-215">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="update-the-web-app-and-republish"></a><span data-ttu-id="13b44-216">Обновить веб-приложение и повторно опубликуйте</span><span class="sxs-lookup"><span data-stu-id="13b44-216">Update the web app and republish</span></span>

<span data-ttu-id="13b44-217">После внесения изменений в локальный код, повторной публикации:</span><span class="sxs-lookup"><span data-stu-id="13b44-217">After making changes to the local code, republish:</span></span>

1. <span data-ttu-id="13b44-218">В **обозревателе решений** Visual Studio откройте файл *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="13b44-218">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

1. <span data-ttu-id="13b44-219">В методе `Configure` измените метод `Response.WriteAsync`, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="13b44-219">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. <span data-ttu-id="13b44-220">Сохранить изменения в *файла Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="13b44-220">Save the changes to *Startup.cs*.</span></span>

1. <span data-ttu-id="13b44-221">В **обозревателе решений** щелкните правой кнопкой мыши элемент **Решение "SampleWebAppDemo"** и выберите пункт **Зафиксировать**.</span><span class="sxs-lookup"><span data-stu-id="13b44-221">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="13b44-222">**Team Explorer** отображается.</span><span class="sxs-lookup"><span data-stu-id="13b44-222">The **Team Explorer** is displayed.</span></span>

1. <span data-ttu-id="13b44-223">Например, введите сообщение фиксации `Update #2`.</span><span class="sxs-lookup"><span data-stu-id="13b44-223">Enter a commit message, such as `Update #2`.</span></span>

1. <span data-ttu-id="13b44-224">Нажмите кнопку **Зафиксировать**, чтобы зафиксировать изменения проекта.</span><span class="sxs-lookup"><span data-stu-id="13b44-224">Press the **Commit** button to commit the project changes.</span></span>

1. <span data-ttu-id="13b44-225">Выберите **Главная** > **Синхронизировать** > **Действия** > **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="13b44-225">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

> [!NOTE]
> <span data-ttu-id="13b44-226">Кроме того, отправьте изменения из **командное окно** , открыв **командное окно**, изменения в каталог проекта и ввода команд git.</span><span class="sxs-lookup"><span data-stu-id="13b44-226">As an alternative, push the changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering a git command.</span></span> <span data-ttu-id="13b44-227">Пример</span><span class="sxs-lookup"><span data-stu-id="13b44-227">Example:</span></span>
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="13b44-228">Просмотр обновленного веб-приложения в Azure</span><span class="sxs-lookup"><span data-stu-id="13b44-228">View the updated web app in Azure</span></span>

<span data-ttu-id="13b44-229">Просмотреть обновленные веб-приложения, выбрав **Обзор** из колонки веб приложения на портале Azure или откройте браузер и введите URL-адрес для веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="13b44-229">View the updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for the web app.</span></span> <span data-ttu-id="13b44-230">Пример: `http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="13b44-230">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="additional-resources"></a><span data-ttu-id="13b44-231">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="13b44-231">Additional resources</span></span>

* [<span data-ttu-id="13b44-232">Для создания и публикации в Azure веб-приложение с непрерывным развертыванием с помощью VSTS</span><span class="sxs-lookup"><span data-stu-id="13b44-232">Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment</span></span>](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic)
* [<span data-ttu-id="13b44-233">Проект Kudu</span><span class="sxs-lookup"><span data-stu-id="13b44-233">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
