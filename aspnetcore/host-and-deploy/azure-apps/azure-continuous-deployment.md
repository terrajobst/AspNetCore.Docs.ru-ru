---
title: Непрерывное развертывание в Azure с помощью Visual Studio и Git с помощью ASP.NET Core
author: rick-anderson
description: Сведения о создании веб-приложения ASP.NET Core с помощью Visual Studio и его развертывании в службе приложений Azure с использованием Git для непрерывного развертывания.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: 4de1893e8c1f7f2f4d9af7278a110067ea777c61
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="continuous-deployment-to-azure-with-visual-studio-and-git-with-aspnet-core"></a><span data-ttu-id="d60d8-103">Непрерывное развертывание в Azure с помощью Visual Studio и Git с помощью ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d60d8-103">Continuous deployment to Azure with Visual Studio and Git with ASP.NET Core</span></span>

<span data-ttu-id="d60d8-104">Автор: [Эрик Рейтан](https://github.com/Erikre) (Erik Reitan)</span><span class="sxs-lookup"><span data-stu-id="d60d8-104">By [Erik Reitan](https://github.com/Erikre)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="d60d8-105">Этого учебника показано, как создать веб-приложение ASP.NET Core с помощью Visual Studio и развернуть ее на службе приложений Azure из Visual Studio с помощью непрерывного развертывания.</span><span class="sxs-lookup"><span data-stu-id="d60d8-105">This tutorial shows how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span>

<span data-ttu-id="d60d8-106">См. также статью [Использование VSTS для сборки и публикации в веб-приложении Azure с помощью непрерывного развертывания](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), в которой показано, как настроить рабочий процесс непрерывной поставки для [службы приложений Azure](/azure/app-service/app-service-web-overview) с помощью Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="d60d8-106">See also [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](/azure/app-service/app-service-web-overview) using Visual Studio Team Services.</span></span> <span data-ttu-id="d60d8-107">Azure непрерывной доставки в Team Services упрощает настройку конвейера высокого уровня надежности системы, для публикации обновлений для приложений, размещенных в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="d60d8-107">Azure Continuous Delivery in Team Services simplifies setting up a robust deployment pipeline to publish updates for apps hosted in Azure App Service.</span></span> <span data-ttu-id="d60d8-108">Конвейер можно настроить на портале Azure для создания, выполнения тестов, развертывания промежуточный слот и затем развернуть в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="d60d8-108">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot, and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="d60d8-109">Для работы с этим учебником требуется учетная запись Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="d60d8-109">To complete this tutorial, a Microsoft Azure account is required.</span></span> <span data-ttu-id="d60d8-110">Чтобы получить учетную запись, [активировать преимущества для подписчиков MSDN](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) или [зарегистрироваться для получения бесплатной пробной версии](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="d60d8-110">To obtain an account, [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d60d8-111">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="d60d8-111">Prerequisites</span></span>

<span data-ttu-id="d60d8-112">В этом учебнике предполагается, что установлено следующее программное обеспечение:</span><span class="sxs-lookup"><span data-stu-id="d60d8-112">This tutorial assumes the following software is installed:</span></span>

* [<span data-ttu-id="d60d8-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d60d8-113">Visual Studio</span></span>](https://www.visualstudio.com)
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="d60d8-114">[Git](https://git-scm.com/downloads) для Windows.</span><span class="sxs-lookup"><span data-stu-id="d60d8-114">[Git](https://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="d60d8-115">Создание веб-приложения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d60d8-115">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="d60d8-116">Запустите Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d60d8-116">Start Visual Studio.</span></span>

1. <span data-ttu-id="d60d8-117">В меню **Файл** выберите пункт **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="d60d8-117">From the **File** menu, select **New** > **Project**.</span></span>

1. <span data-ttu-id="d60d8-118">Выберите шаблон проекта **Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="d60d8-118">Select the **ASP.NET Core Web Application** project template.</span></span> <span data-ttu-id="d60d8-119">Он находится в разделе **Installed** (Установлено) > **Templates** (Шаблоны) > **Visual C#** > **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="d60d8-119">It appears under **Installed** > **Templates** > **Visual C#** > **.NET Core**.</span></span> <span data-ttu-id="d60d8-120">Задайте для проекта имя `SampleWebAppDemo`.</span><span class="sxs-lookup"><span data-stu-id="d60d8-120">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="d60d8-121">Выберите **создать новый репозиторий Git** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="d60d8-121">Select the **Create new Git repository** option and click **OK**.</span></span>

   ![Диалоговое окно создания нового проекта](azure-continuous-deployment/_static/01-new-project.png)

1. <span data-ttu-id="d60d8-123">В диалоговом окне **New ASP.NET Core Project** (Новый проект ASP.NET Core) выберите **пустой** шаблон и щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="d60d8-123">In the **New ASP.NET Core Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![Диалоговое окно "Новый проект ASP.NET"](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> <span data-ttu-id="d60d8-125">Последний выпуск .NET Core является версия 2.0.</span><span class="sxs-lookup"><span data-stu-id="d60d8-125">The most recent release of .NET Core is 2.0.</span></span>

### <a name="running-the-web-app-locally"></a><span data-ttu-id="d60d8-126">Запуск веб-приложения в локальной системе</span><span class="sxs-lookup"><span data-stu-id="d60d8-126">Running the web app locally</span></span>

1. <span data-ttu-id="d60d8-127">После того как Visual Studio завершит создание приложения, запустите его, выбрав **Отладка** > **Начать отладку**.</span><span class="sxs-lookup"><span data-stu-id="d60d8-127">Once Visual Studio finishes creating the app, run the app by selecting **Debug** > **Start Debugging**.</span></span> <span data-ttu-id="d60d8-128">В качестве альтернативы нажать **F5**.</span><span class="sxs-lookup"><span data-stu-id="d60d8-128">As an alternative, press **F5**.</span></span>

   <span data-ttu-id="d60d8-129">Инициализация Visual Studio и нового приложения может занять некоторое время.</span><span class="sxs-lookup"><span data-stu-id="d60d8-129">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="d60d8-130">После завершения браузер показано выполняющееся приложение.</span><span class="sxs-lookup"><span data-stu-id="d60d8-130">Once it's complete, the browser shows the running app.</span></span>

   ![Окно браузера с выполняющимся приложением, в котором выводится сообщение "Hello World!"](azure-continuous-deployment/_static/04-browser-runapp.png)

1. <span data-ttu-id="d60d8-132">Ознакомившись с запущенной веб-приложения, закройте браузер и щелкните значок «Остановить отладку» на панели инструментов Visual Studio, чтобы остановить выполнение приложения.</span><span class="sxs-lookup"><span data-stu-id="d60d8-132">After reviewing the running Web app, close the browser and select the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="d60d8-133">Создание веб-приложения на портале Azure</span><span class="sxs-lookup"><span data-stu-id="d60d8-133">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="d60d8-134">Следующие шаги создания веб-приложения на портале Azure:</span><span class="sxs-lookup"><span data-stu-id="d60d8-134">The following steps create a web app in the Azure Portal:</span></span>

1. <span data-ttu-id="d60d8-135">Войдите на [портал Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d60d8-135">Log in to the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="d60d8-136">Выберите **NEW** в верхней левой части портала интерфейса.</span><span class="sxs-lookup"><span data-stu-id="d60d8-136">Select **NEW** at the top left of the portal interface.</span></span>

1. <span data-ttu-id="d60d8-137">Выберите **Интернет + мобильные устройства** > **веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="d60d8-137">Select **Web + Mobile** > **Web App**.</span></span>

   ![Портал Microsoft Azure > кнопка "Создать" > пункт "Интернет и мобильные устройства" в списке "Marketplace" > кнопка "Веб-приложение" в списке "Рекомендуемые приложения"](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. <span data-ttu-id="d60d8-139">В колонке **Веб-приложение** в поле **Имя службы приложений** введите уникальное значение.</span><span class="sxs-lookup"><span data-stu-id="d60d8-139">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

   ![Колонка "Веб-приложение"](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > <span data-ttu-id="d60d8-141">**Имя приложения службы** имя должно быть уникальным.</span><span class="sxs-lookup"><span data-stu-id="d60d8-141">The **App Service Name** name must be unique.</span></span> <span data-ttu-id="d60d8-142">Портал применяет это правило, если указано имя.</span><span class="sxs-lookup"><span data-stu-id="d60d8-142">The portal enforces this rule when the name is provided.</span></span> <span data-ttu-id="d60d8-143">Если предоставить другое значение, замените это значение для каждого вхождения **SampleWebAppDemo** в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="d60d8-143">If providing a different value, substitute that value for each occurrence of **SampleWebAppDemo** in this tutorial.</span></span>

   <span data-ttu-id="d60d8-144">Кроме того, в колонке **Веб-приложение** выберите существующее **Расположение или план службы приложений** либо создайте собственный план или расположение.</span><span class="sxs-lookup"><span data-stu-id="d60d8-144">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="d60d8-145">Если создается новый план, выберите ценовую категорию, расположение и другие параметры.</span><span class="sxs-lookup"><span data-stu-id="d60d8-145">If creating a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="d60d8-146">Дополнительные сведения о планах службы приложений см. в разделе [исчерпывающий обзор планы службы приложений Azure](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span><span class="sxs-lookup"><span data-stu-id="d60d8-146">For more information on App Service plans, see [Azure App Service plans in-depth overview](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span></span>

1. <span data-ttu-id="d60d8-147">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="d60d8-147">Select **Create**.</span></span> <span data-ttu-id="d60d8-148">Подготовка Azure и запустить веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="d60d8-148">Azure will provision and start the web app.</span></span>

   ![Портал Azure: колонка "Основное" для приложения SampleWebAppDemo01](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="d60d8-150">Включение публикации в Git для нового веб-приложения</span><span class="sxs-lookup"><span data-stu-id="d60d8-150">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="d60d8-151">Git — это система управления распределенных версии, который может использоваться для развертывания веб-приложение службы приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="d60d8-151">Git is a distributed version control system that can be used to deploy an Azure App Service web app.</span></span> <span data-ttu-id="d60d8-152">Кода веб-приложения хранятся в локальном репозитории, а код развертывается в Azure с помощью принудительной отправки в удаленном репозитории.</span><span class="sxs-lookup"><span data-stu-id="d60d8-152">Web app code is stored in a local Git repository, and the code is deployed to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="d60d8-153">Войдите на [портал Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d60d8-153">Log into the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="d60d8-154">Выберите **службы приложений** для просмотра списка приложений служб, связанных с подпиской Azure.</span><span class="sxs-lookup"><span data-stu-id="d60d8-154">Select **App Services** to view a list of the app services associated with the Azure subscription.</span></span>

1. <span data-ttu-id="d60d8-155">Выберите веб-приложения, созданные в предыдущем разделе этого учебника.</span><span class="sxs-lookup"><span data-stu-id="d60d8-155">Select the web app created in the previous section of this tutorial.</span></span>

1. <span data-ttu-id="d60d8-156">В колонке **Deployment** (Развертывание) выберите **Deployment options** (Параметры развертывания) > **Выбор источника** > **Локальный репозиторий Git**.</span><span class="sxs-lookup"><span data-stu-id="d60d8-156">In the **Deployment** blade, select **Deployment options** > **Choose Source** > **Local Git Repository**.</span></span>

   ![Колонка "Параметры" > колонка "Источник развертывания" > колонка "Выбор источника"](azure-continuous-deployment/_static/deployment-options.png)

1. <span data-ttu-id="d60d8-158">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="d60d8-158">Select **OK**.</span></span>

1. <span data-ttu-id="d60d8-159">Если учетные данные развертывания для публикации веб-приложение или другое приложение службы приложений ранее не задан, настройте их сейчас:</span><span class="sxs-lookup"><span data-stu-id="d60d8-159">If deployment credentials for publishing a web app or other App Service app haven't previously been set up, set them up now:</span></span>

   * <span data-ttu-id="d60d8-160">Выберите **параметры** > **учетные данные развертывания**.</span><span class="sxs-lookup"><span data-stu-id="d60d8-160">Select **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="d60d8-161">**Задать учетные данные развертывания** колонке отображается.</span><span class="sxs-lookup"><span data-stu-id="d60d8-161">The **Set deployment credentials** blade is displayed.</span></span>
   * <span data-ttu-id="d60d8-162">Создайте имя пользователя и пароль.</span><span class="sxs-lookup"><span data-stu-id="d60d8-162">Create a user name and password.</span></span> <span data-ttu-id="d60d8-163">Сохраните пароль для последующего использования при настройке Git.</span><span class="sxs-lookup"><span data-stu-id="d60d8-163">Save the password for later use when setting up Git.</span></span>
   * <span data-ttu-id="d60d8-164">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="d60d8-164">Select **Save**.</span></span>

1. <span data-ttu-id="d60d8-165">В **веб-приложения** колонке выберите **параметры** > **свойства**.</span><span class="sxs-lookup"><span data-stu-id="d60d8-165">In the **Web App** blade, select **Settings** > **Properties**.</span></span> <span data-ttu-id="d60d8-166">URL-адрес в удаленный репозиторий Git, для развертывания отображается в разделе **URL-адрес GIT**.</span><span class="sxs-lookup"><span data-stu-id="d60d8-166">The URL of the remote Git repository to deploy to is shown under **GIT URL**.</span></span>

1. <span data-ttu-id="d60d8-167">Скопируйте значение в поле **URL-адрес Git**, так как оно понадобится далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="d60d8-167">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Портал Azure > колонка свойств приложения](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a><span data-ttu-id="d60d8-169">Опубликовать веб-приложения в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="d60d8-169">Publish the web app to Azure App Service</span></span>

<span data-ttu-id="d60d8-170">В этом разделе создайте локальный репозиторий Git с помощью Visual Studio и push из этого репозитория в Azure для развертывания веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="d60d8-170">In this section, create a local Git repository using Visual Studio and push from that repository to Azure to deploy the web app.</span></span> <span data-ttu-id="d60d8-171">Для этого потребуется выполнить указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="d60d8-171">The steps involved include the following:</span></span>

* <span data-ttu-id="d60d8-172">Добавьте параметр удаленный репозиторий, используя значение URL-адрес GIT, чтобы можно было развернуть локальный репозиторий в Azure.</span><span class="sxs-lookup"><span data-stu-id="d60d8-172">Add the remote repository setting using the GIT URL value, so the local repository can be deployed to Azure.</span></span>
* <span data-ttu-id="d60d8-173">Зафиксируйте изменения проекта.</span><span class="sxs-lookup"><span data-stu-id="d60d8-173">Commit project changes.</span></span>
* <span data-ttu-id="d60d8-174">Отправьте изменения проекта из локального репозитория в удаленном репозитории в Azure.</span><span class="sxs-lookup"><span data-stu-id="d60d8-174">Push project changes from the local repository to the remote repository on Azure.</span></span>

1. <span data-ttu-id="d60d8-175">В **обозревателе решений** щелкните правой кнопкой мыши элемент **Решение "SampleWebAppDemo"** и выберите пункт **Зафиксировать**.</span><span class="sxs-lookup"><span data-stu-id="d60d8-175">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="d60d8-176">**Team Explorer** отображается.</span><span class="sxs-lookup"><span data-stu-id="d60d8-176">The **Team Explorer** is displayed.</span></span>

   ![Вкладка "Подключение" в Team Explorer](azure-continuous-deployment/_static/10-team-explorer.png)

1. <span data-ttu-id="d60d8-178">В **Team Explorer** щелкните значок **Главная** и выберите **Параметры** > **Параметры репозитория**.</span><span class="sxs-lookup"><span data-stu-id="d60d8-178">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

1. <span data-ttu-id="d60d8-179">В **удаленные элементы** раздел **параметров репозитория**выберите **добавить**.</span><span class="sxs-lookup"><span data-stu-id="d60d8-179">In the **Remotes** section of the **Repository Settings**, select **Add**.</span></span> <span data-ttu-id="d60d8-180">**Добавьте удаленный** диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="d60d8-180">The **Add Remote** dialog box is displayed.</span></span>

1. <span data-ttu-id="d60d8-181">В качестве **имени** удаленного объекта укажите **Azure-SampleApp**.</span><span class="sxs-lookup"><span data-stu-id="d60d8-181">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

1. <span data-ttu-id="d60d8-182">Задайте значение **выборки** для **URL-адрес Git** , скопированные из Azure ранее в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="d60d8-182">Set the value for **Fetch** to the **Git URL** that copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="d60d8-183">Обратите внимание на то, что этот URL-адрес должен заканчиваться на **.git**.</span><span class="sxs-lookup"><span data-stu-id="d60d8-183">Note that this is the URL that ends with **.git**.</span></span>

   ![Диалоговое окно "Изменение удаленного объекта"](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > <span data-ttu-id="d60d8-185">Кроме того, укажите удаленный репозиторий, из **командное окно** , открыв **командное окно**, изменения в каталог проекта и ввести команду.</span><span class="sxs-lookup"><span data-stu-id="d60d8-185">As an alternative, specify the remote repository from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the command.</span></span> <span data-ttu-id="d60d8-186">Пример</span><span class="sxs-lookup"><span data-stu-id="d60d8-186">Example:</span></span>
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. <span data-ttu-id="d60d8-187">Щелкните значок **Главная** и выберите **Параметры** > **Глобальные параметры**.</span><span class="sxs-lookup"><span data-stu-id="d60d8-187">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="d60d8-188">Убедитесь, что заданы имя и адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="d60d8-188">Confirm that the name and email address are set.</span></span> <span data-ttu-id="d60d8-189">Выберите **обновление** при необходимости.</span><span class="sxs-lookup"><span data-stu-id="d60d8-189">Select **Update** if required.</span></span>

1. <span data-ttu-id="d60d8-190">Выберите **Главная** > **Изменения**, чтобы вернуться к представлению **Изменения**.</span><span class="sxs-lookup"><span data-stu-id="d60d8-190">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

1. <span data-ttu-id="d60d8-191">Введите сообщение фиксации, например **начальной Push #1** и выберите **фиксации**.</span><span class="sxs-lookup"><span data-stu-id="d60d8-191">Enter a commit message, such as **Initial Push #1** and select **Commit**.</span></span> <span data-ttu-id="d60d8-192">Это действие создает *фиксации* локально.</span><span class="sxs-lookup"><span data-stu-id="d60d8-192">This action creates a *commit* locally.</span></span>

   ![Вкладка "Подключение" в Team Explorer](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > <span data-ttu-id="d60d8-194">В качестве альтернативы фиксации меняется с **командное окно** , открыв **командное окно**, изменения в каталог проекта и ввод команд git.</span><span class="sxs-lookup"><span data-stu-id="d60d8-194">As an alternative, commit changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the git commands.</span></span> <span data-ttu-id="d60d8-195">Пример</span><span class="sxs-lookup"><span data-stu-id="d60d8-195">Example:</span></span>
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. <span data-ttu-id="d60d8-196">Выберите **Главная** > **Синхронизировать** > **Действия** > **Открыть командную строку**.</span><span class="sxs-lookup"><span data-stu-id="d60d8-196">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="d60d8-197">Откроется командная строка в каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="d60d8-197">The command prompt opens to the project directory.</span></span>

1. <span data-ttu-id="d60d8-198">Введите в командном окне следующую команду:</span><span class="sxs-lookup"><span data-stu-id="d60d8-198">Enter the following command in the command window:</span></span>

   `git push -u Azure-SampleApp master`

1. <span data-ttu-id="d60d8-199">Введите Azure **учетные данные развертывания** пароль, созданный ранее в Azure.</span><span class="sxs-lookup"><span data-stu-id="d60d8-199">Enter the Azure **deployment credentials** password created earlier in Azure.</span></span>

   <span data-ttu-id="d60d8-200">Эта команда запускает процесс передачи файлов локального проекта в Azure.</span><span class="sxs-lookup"><span data-stu-id="d60d8-200">This command starts the process of pushing the local project files to Azure.</span></span> <span data-ttu-id="d60d8-201">Выходные данные этой команды оканчивается сообщение о том, что развертывание успешно.</span><span class="sxs-lookup"><span data-stu-id="d60d8-201">The output from the above command ends with a message that the deployment was successful.</span></span>

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > <span data-ttu-id="d60d8-202">Если требуется совместная работа над проектом, распределенного запроса передает [GitHub](https://github.com) до отправки в Azure.</span><span class="sxs-lookup"><span data-stu-id="d60d8-202">If collaboration on the project is required, consider pushing to [GitHub](https://github.com) before pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="d60d8-203">Проверка активного развертывания</span><span class="sxs-lookup"><span data-stu-id="d60d8-203">Verify the Active Deployment</span></span>

<span data-ttu-id="d60d8-204">Проверьте успешность выполнения передачи приложения web от локальной среды в Azure.</span><span class="sxs-lookup"><span data-stu-id="d60d8-204">Verify that the web app transfer from the local environment to Azure is successful.</span></span>

<span data-ttu-id="d60d8-205">В [портала Azure](https://portal.azure.com), выберите веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="d60d8-205">In the [Azure Portal](https://portal.azure.com), select the web app.</span></span> <span data-ttu-id="d60d8-206">Выберите **развертывания** > **варианты развертывания**.</span><span class="sxs-lookup"><span data-stu-id="d60d8-206">Select **Deployment** > **Deployment options**.</span></span>

![Портал Azure > колонка "Параметры" > колонка "Развертывания", содержащая успешное развертывание](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="d60d8-208">Запуск приложения в Azure</span><span class="sxs-lookup"><span data-stu-id="d60d8-208">Run the app in Azure</span></span>

<span data-ttu-id="d60d8-209">Теперь, когда веб-приложение развертывается в Azure, запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="d60d8-209">Now that the web app is deployed to Azure, run the app.</span></span>

<span data-ttu-id="d60d8-210">Это можно сделать двумя способами:</span><span class="sxs-lookup"><span data-stu-id="d60d8-210">This can be accomplished in two ways:</span></span>

* <span data-ttu-id="d60d8-211">На портале Azure найдите колонка веб приложения для веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="d60d8-211">In the Azure Portal, locate the web app blade for the web app.</span></span> <span data-ttu-id="d60d8-212">Выберите **Обзор** для просмотра приложения в браузере по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d60d8-212">Select **Browse** to view the app in the default browser.</span></span>
* <span data-ttu-id="d60d8-213">Откройте браузер и введите URL-адрес для веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="d60d8-213">Open a browser and enter the URL for the web app.</span></span> <span data-ttu-id="d60d8-214">Пример: `http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="d60d8-214">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="update-the-web-app-and-republish"></a><span data-ttu-id="d60d8-215">Обновить веб-приложение и повторно опубликуйте</span><span class="sxs-lookup"><span data-stu-id="d60d8-215">Update the web app and republish</span></span>

<span data-ttu-id="d60d8-216">После внесения изменений в локальный код, повторной публикации:</span><span class="sxs-lookup"><span data-stu-id="d60d8-216">After making changes to the local code, republish:</span></span>

1. <span data-ttu-id="d60d8-217">В **обозревателе решений** Visual Studio откройте файл *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d60d8-217">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

1. <span data-ttu-id="d60d8-218">В методе `Configure` измените метод `Response.WriteAsync`, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="d60d8-218">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. <span data-ttu-id="d60d8-219">Сохранить изменения в *файла Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d60d8-219">Save the changes to *Startup.cs*.</span></span>

1. <span data-ttu-id="d60d8-220">В **обозревателе решений** щелкните правой кнопкой мыши элемент **Решение "SampleWebAppDemo"** и выберите пункт **Зафиксировать**.</span><span class="sxs-lookup"><span data-stu-id="d60d8-220">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="d60d8-221">**Team Explorer** отображается.</span><span class="sxs-lookup"><span data-stu-id="d60d8-221">The **Team Explorer** is displayed.</span></span>

1. <span data-ttu-id="d60d8-222">Например, введите сообщение фиксации `Update #2`.</span><span class="sxs-lookup"><span data-stu-id="d60d8-222">Enter a commit message, such as `Update #2`.</span></span>

1. <span data-ttu-id="d60d8-223">Нажмите кнопку **Зафиксировать**, чтобы зафиксировать изменения проекта.</span><span class="sxs-lookup"><span data-stu-id="d60d8-223">Press the **Commit** button to commit the project changes.</span></span>

1. <span data-ttu-id="d60d8-224">Выберите **Главная** > **Синхронизировать** > **Действия** > **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="d60d8-224">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

> [!NOTE]
> <span data-ttu-id="d60d8-225">Кроме того, отправьте изменения из **командное окно** , открыв **командное окно**, изменения в каталог проекта и ввода команд git.</span><span class="sxs-lookup"><span data-stu-id="d60d8-225">As an alternative, push the changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering a git command.</span></span> <span data-ttu-id="d60d8-226">Пример</span><span class="sxs-lookup"><span data-stu-id="d60d8-226">Example:</span></span>
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="d60d8-227">Просмотр обновленного веб-приложения в Azure</span><span class="sxs-lookup"><span data-stu-id="d60d8-227">View the updated web app in Azure</span></span>

<span data-ttu-id="d60d8-228">Просмотреть обновленные веб-приложения, выбрав **Обзор** из колонки веб приложения на портале Azure или откройте браузер и введите URL-адрес для веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="d60d8-228">View the updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for the web app.</span></span> <span data-ttu-id="d60d8-229">Пример: `http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="d60d8-229">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d60d8-230">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="d60d8-230">Additional resources</span></span>

* [<span data-ttu-id="d60d8-231">Для создания и публикации в Azure веб-приложение с непрерывным развертыванием с помощью VSTS</span><span class="sxs-lookup"><span data-stu-id="d60d8-231">Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment</span></span>](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic)
* [<span data-ttu-id="d60d8-232">Проект Kudu</span><span class="sxs-lookup"><span data-stu-id="d60d8-232">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
