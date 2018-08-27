---
title: DevOps с помощью ASP.NET Core и Azure | Непрерывная интеграция и развертывание
author: CamSoper
description: Рекомендации по созданию сквозного решения конвейера DevOps для приложения ASP.NET Core, размещенного в Azure.
ms.author: scaddie
ms.date: 08/17/2018
uid: azure/devops/cicd
ms.openlocfilehash: e084a6115dc7e176c17b2b318233b7a003b39a83
ms.sourcegitcommit: 1cf65c25ed16495e27f35ded98b3952a30c68f36
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2018
ms.locfileid: "42909545"
---
# <a name="continuous-integration-and-deployment"></a><span data-ttu-id="6e0e7-103">Непрерывная интеграция и развертывание</span><span class="sxs-lookup"><span data-stu-id="6e0e7-103">Continuous integration and deployment</span></span>

<span data-ttu-id="6e0e7-104">В предыдущей главе вы создали локальный репозиторий Git для приложения для чтения простого веб-канала.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-104">In the previous chapter, you created a local Git repository for the Simple Feed Reader app.</span></span> <span data-ttu-id="6e0e7-105">В этой главе будут публиковать этого кода в репозитории GitHub, а также создание конвейера DevOps в Visual Studio Team Services (VSTS).</span><span class="sxs-lookup"><span data-stu-id="6e0e7-105">In this chapter, you'll publish that code to a GitHub repository and construct a Visual Studio Team Services (VSTS) DevOps pipeline.</span></span> <span data-ttu-id="6e0e7-106">Конвейер позволяет непрерывного выполнения сборки и развертывания приложения.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-106">The pipeline enables continuous builds and deployments of the app.</span></span> <span data-ttu-id="6e0e7-107">Любой фиксации в репозиторий GitHub активирует сборку и развертывание в промежуточный слот веб-приложения Azure.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-107">Any commit to the GitHub repository triggers a build and a deployment to the Azure Web App's staging slot.</span></span>

<span data-ttu-id="6e0e7-108">В этом разделе вы выполните следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-108">In this section, you'll complete the following tasks:</span></span>

* <span data-ttu-id="6e0e7-109">Публикация в код приложения на GitHub</span><span class="sxs-lookup"><span data-stu-id="6e0e7-109">Publish the app's code to GitHub</span></span>
* <span data-ttu-id="6e0e7-110">Отключить развертывание локального репозитория Git</span><span class="sxs-lookup"><span data-stu-id="6e0e7-110">Disconnect local Git deployment</span></span>
* <span data-ttu-id="6e0e7-111">Создание учетной записи VSTS</span><span class="sxs-lookup"><span data-stu-id="6e0e7-111">Create a VSTS account</span></span>
* <span data-ttu-id="6e0e7-112">Создание командного проекта в VSTS</span><span class="sxs-lookup"><span data-stu-id="6e0e7-112">Create a team project in VSTS</span></span>
* <span data-ttu-id="6e0e7-113">Создание определения построения</span><span class="sxs-lookup"><span data-stu-id="6e0e7-113">Create a build definition</span></span>
* <span data-ttu-id="6e0e7-114">Создать конвейер выпуска</span><span class="sxs-lookup"><span data-stu-id="6e0e7-114">Create a release pipeline</span></span>
* <span data-ttu-id="6e0e7-115">Зафиксировать изменения в GitHub и автоматически развернуть в Azure</span><span class="sxs-lookup"><span data-stu-id="6e0e7-115">Commit changes to GitHub and automatically deploy to Azure</span></span>
* <span data-ttu-id="6e0e7-116">Изучите конвейера VSTS DevOps</span><span class="sxs-lookup"><span data-stu-id="6e0e7-116">Examine the VSTS DevOps pipeline</span></span>

## <a name="publish-the-apps-code-to-github"></a><span data-ttu-id="6e0e7-117">Публикация в код приложения на GitHub</span><span class="sxs-lookup"><span data-stu-id="6e0e7-117">Publish the app's code to GitHub</span></span>

1. <span data-ttu-id="6e0e7-118">Откройте окно браузера и перейдите к `https://github.com`.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-118">Open a browser window, and navigate to `https://github.com`.</span></span>
1. <span data-ttu-id="6e0e7-119">Нажмите кнопку **+** раскрывающегося списка в заголовке, а затем выберите **новый репозиторий**:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-119">Click the **+** drop-down in the header, and select **New repository**:</span></span>

    ![Параметр новый репозиторий GitHub](media/cicd/github-new-repo.png)

1. <span data-ttu-id="6e0e7-121">Выберите свою учетную запись в **владельца** в раскрывающемся списке и введите *чтения simple потоков* в **имя репозитория** текстового поля.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-121">Select your account in the **Owner** drop-down, and enter *simple-feed-reader* in the **Repository name** textbox.</span></span>
1. <span data-ttu-id="6e0e7-122">Нажмите кнопку **Создать репозиторий** кнопки.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-122">Click the **Create repository** button.</span></span>
1. <span data-ttu-id="6e0e7-123">Откройте командную оболочку локального компьютера.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-123">Open your local machine's command shell.</span></span> <span data-ttu-id="6e0e7-124">Перейдите в каталог, в котором *чтения simple потоков* хранится репозиторий Git.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-124">Navigate to the directory in which the *simple-feed-reader* Git repository is stored.</span></span>
1. <span data-ttu-id="6e0e7-125">Переименуйте существующий *origin* удаленное подключение к *вышестоящего*.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-125">Rename the existing *origin* remote to *upstream*.</span></span> <span data-ttu-id="6e0e7-126">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-126">Execute the following command:</span></span>
    ```console
    git remote rename origin upstream
    ```
1. <span data-ttu-id="6e0e7-127">Добавьте новый *origin* удаленного указывает на копию репозитория на сайте GitHub.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-127">Add a new *origin* remote pointing to your copy of the repository on GitHub.</span></span> <span data-ttu-id="6e0e7-128">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-128">Execute the following command:</span></span>
    ```console
    git remote add origin https://github.com/<GitHub_username>/simple-feed-reader/
    ```
1. <span data-ttu-id="6e0e7-129">Публикация локального репозитория Git на только что созданный репозиторий GitHub.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-129">Publish your local Git repository to the newly created GitHub repository.</span></span> <span data-ttu-id="6e0e7-130">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-130">Execute the following command:</span></span>
    ```console
    git push -u origin master
    ```
1. <span data-ttu-id="6e0e7-131">Откройте окно браузера и перейдите к `https://github.com/<GitHub_username>/simple-feed-reader/`.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-131">Open a browser window, and navigate to `https://github.com/<GitHub_username>/simple-feed-reader/`.</span></span> <span data-ttu-id="6e0e7-132">Проверьте, что ваш код отображается в репозитории GitHub.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-132">Validate that your code appears in the GitHub repository.</span></span>

## <a name="disconnect-local-git-deployment"></a><span data-ttu-id="6e0e7-133">Отключить развертывание локального репозитория Git</span><span class="sxs-lookup"><span data-stu-id="6e0e7-133">Disconnect local Git deployment</span></span>

<span data-ttu-id="6e0e7-134">Удалите локальное развертывание Git, выполнив следующие действия.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-134">Remove the local Git deployment with the following steps.</span></span> <span data-ttu-id="6e0e7-135">VSTS заменяет и расширяет эту функциональность.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-135">VSTS both replaces and augments that functionality.</span></span>

1. <span data-ttu-id="6e0e7-136">Откройте [портала Azure](https://portal.azure.com/)и перейдите к *промежуточных (mywebapp\<unique_number\>/промежуточные)* веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-136">Open the [Azure portal](https://portal.azure.com/), and navigate to the *staging (mywebapp\<unique_number\>/staging)* Web App.</span></span> <span data-ttu-id="6e0e7-137">Веб-приложения можно быстро найти, введя *промежуточных* в поле поиска на портале:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-137">The Web App can be quickly located by entering *staging* in the portal's search box:</span></span>

    ![условие поиска для промежуточного веб-приложения](media/cicd/portal-search-box.png)

1. <span data-ttu-id="6e0e7-139">Нажмите кнопку **варианты развертывания**.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-139">Click **Deployment options**.</span></span> <span data-ttu-id="6e0e7-140">Появится новая панель.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-140">A new panel appears.</span></span> <span data-ttu-id="6e0e7-141">Нажмите кнопку **Disconnect** удалить локальную конфигурацию Git исходного элемента управления, добавленный в предыдущей главе.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-141">Click **Disconnect** to remove the local Git source control configuration that was added in the previous chapter.</span></span> <span data-ttu-id="6e0e7-142">Подтвердить операцию удаления, щелкнув **Да** кнопки.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-142">Confirm the removal operation by clicking the **Yes** button.</span></span>
1. <span data-ttu-id="6e0e7-143">Перейдите к *mywebapp < unique_number >* службы приложений.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-143">Navigate to the *mywebapp<unique_number>* App Service.</span></span> <span data-ttu-id="6e0e7-144">Напоминаем поле поиска на портале можно использовать для быстрого поиска в службе приложений.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-144">As a reminder, the portal's search box can be used to quickly locate the App Service.</span></span>
1. <span data-ttu-id="6e0e7-145">Нажмите кнопку **варианты развертывания**.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-145">Click **Deployment options**.</span></span> <span data-ttu-id="6e0e7-146">Появится новая панель.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-146">A new panel appears.</span></span> <span data-ttu-id="6e0e7-147">Нажмите кнопку **Disconnect** удалить локальную конфигурацию Git исходного элемента управления, добавленный в предыдущей главе.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-147">Click **Disconnect** to remove the local Git source control configuration that was added in the previous chapter.</span></span> <span data-ttu-id="6e0e7-148">Подтвердить операцию удаления, щелкнув **Да** кнопки.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-148">Confirm the removal operation by clicking the **Yes** button.</span></span>

## <a name="create-a-vsts-account"></a><span data-ttu-id="6e0e7-149">Создание учетной записи VSTS</span><span class="sxs-lookup"><span data-stu-id="6e0e7-149">Create a VSTS account</span></span>

1. <span data-ttu-id="6e0e7-150">Откройте браузер и перейдите к [страницу создания учетной записи VSTS](https://go.microsoft.com/fwlink/?LinkId=307137).</span><span class="sxs-lookup"><span data-stu-id="6e0e7-150">Open a browser, and navigate to the [VSTS account creation page](https://go.microsoft.com/fwlink/?LinkId=307137).</span></span>
1. <span data-ttu-id="6e0e7-151">Введите уникальное имя в **выберите запоминающееся имя** textbox для формирования URL-адрес для доступа к учетной записи VSTS.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-151">Type a unique name into the **Pick a memorable name** textbox to form the URL for accessing your VSTS account.</span></span>
1. <span data-ttu-id="6e0e7-152">Выберите **Git** "переключатель", так как код размещается в репозитории GitHub.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-152">Select the **Git** radio button, since the code is hosted in a GitHub repository.</span></span>
1. <span data-ttu-id="6e0e7-153">Нажмите кнопку **Continue** (Продолжить).</span><span class="sxs-lookup"><span data-stu-id="6e0e7-153">Click the **Continue** button.</span></span> <span data-ttu-id="6e0e7-154">После короткого ожидания, учетную запись и командный проект с именем *MyFirstProject*, создаются.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-154">After a short wait, an account and a team project, named *MyFirstProject*, are created.</span></span>

    ![Страница создания учетной записи VSTS](media/cicd/vsts-account-creation.png)

1. <span data-ttu-id="6e0e7-156">Откройте по электронной почте подтверждение, указывающее на то, что учетная запись VSTS и проекта будут готовы к использованию.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-156">Open the confirmation email indicating that the VSTS account and project are ready for use.</span></span> <span data-ttu-id="6e0e7-157">Нажмите кнопку **запустить проект** кнопки:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-157">Click the **Start your project** button:</span></span>

    ![Ваш проект кнопка "Пуск"](media/cicd/vsts-start-project.png)

1. <span data-ttu-id="6e0e7-159">Откроется в браузере  *\<account_name\>. visualstudio.com*.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-159">A browser opens to *\<account_name\>.visualstudio.com*.</span></span> <span data-ttu-id="6e0e7-160">Нажмите кнопку *MyFirstProject* ссылку, чтобы приступить к настройке конвейера DevOps проекта.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-160">Click the *MyFirstProject* link to begin configuring the project's DevOps pipeline.</span></span>

## <a name="configure-the-devops-pipeline"></a><span data-ttu-id="6e0e7-161">Настройка конвейера DevOps</span><span class="sxs-lookup"><span data-stu-id="6e0e7-161">Configure the DevOps pipeline</span></span>

<span data-ttu-id="6e0e7-162">Существуют состоит из трех шагов для завершения.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-162">There are three distinct steps to complete.</span></span> <span data-ttu-id="6e0e7-163">Выполнив действия, описанные в следующих трех разделов приводит работы конвейера DevOps.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-163">Completing the steps in the following three sections results in an operational DevOps pipeline.</span></span>

### <a name="grant-vsts-access-to-the-github-repository"></a><span data-ttu-id="6e0e7-164">Предоставление доступа VSTS в репозиторий GitHub</span><span class="sxs-lookup"><span data-stu-id="6e0e7-164">Grant VSTS access to the GitHub repository</span></span>

1. <span data-ttu-id="6e0e7-165">Разверните **или выполнить сборку кода из внешнего репозитория** accordion.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-165">Expand the **or build code from an external repository** accordion.</span></span> <span data-ttu-id="6e0e7-166">Нажмите кнопку **установки сборки** кнопки:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-166">Click the **Setup Build** button:</span></span>

    ![Кнопка настройки сборки](media/cicd/vsts-setup-build.png)

1. <span data-ttu-id="6e0e7-168">Выберите **GitHub** вариант **выбрать источник** раздел:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-168">Select the **GitHub** option from the **Select a source** section:</span></span>

    ![Выберите источник - GitHub](media/cicd/vsts-select-source.png)

1. <span data-ttu-id="6e0e7-170">Требуется авторизация для доступа к репозиторию GitHub VSTS.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-170">Authorization is required before VSTS can access your GitHub repository.</span></span> <span data-ttu-id="6e0e7-171">Введите *< GitHub_username > подключение GitHub* в **имя подключения** текстового поля.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-171">Enter *<GitHub_username> GitHub connection* in the **Connection name** textbox.</span></span> <span data-ttu-id="6e0e7-172">Пример:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-172">For example:</span></span>

    ![Имя подключения GitHub](media/cicd/vsts-repo-authz.png)

1. <span data-ttu-id="6e0e7-174">Если двухфакторная проверка подлинности будет включен в вашей учетной записи GitHub, личный маркер доступа не требуется.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-174">If two-factor authentication is enabled on your GitHub account, a personal access token is required.</span></span> <span data-ttu-id="6e0e7-175">В этом случае щелкните **авторизовать маркер личного доступа GitHub** ссылку.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-175">In that case, click the **Authorize with a GitHub personal access token** link.</span></span> <span data-ttu-id="6e0e7-176">См. в разделе [официальной документации создания маркера личного доступа GitHub](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) для справки.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-176">See the [official GitHub personal access token creation instructions](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) for help.</span></span> <span data-ttu-id="6e0e7-177">Только *репозитория* требуется область разрешений.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-177">Only the *repo* scope of permissions is needed.</span></span> <span data-ttu-id="6e0e7-178">В противном случае нажмите **авторизация выполняется с использованием OAuth** кнопки.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-178">Otherwise, click the **Authorize using OAuth** button.</span></span>
1. <span data-ttu-id="6e0e7-179">Когда появится запрос, войдите в учетную запись GitHub.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-179">When prompted, sign in to your GitHub account.</span></span> <span data-ttu-id="6e0e7-180">Затем выберите авторизовать для предоставления доступа к учетной записи VSTS.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-180">Then select Authorize to grant access to your VSTS account.</span></span> <span data-ttu-id="6e0e7-181">В случае успешного выполнения создается новая конечная точка службы.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-181">If successful, a new service endpoint is created.</span></span>
1. <span data-ttu-id="6e0e7-182">Нажмите кнопку с многоточием рядом с полем **репозитория** кнопки.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-182">Click the ellipsis button next to the **Repository** button.</span></span> <span data-ttu-id="6e0e7-183">Выберите *< GitHub_username > / чтения simple потоков* репозитория из списка.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-183">Select the *<GitHub_username>/simple-feed-reader* repository from the list.</span></span> <span data-ttu-id="6e0e7-184">Нажмите кнопку **выберите** кнопки.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-184">Click the **Select** button.</span></span>
1. <span data-ttu-id="6e0e7-185">Выберите *master* ветвь из **ветвь по умолчанию для сборок вручную и по расписанию** раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-185">Select the *master* branch from the **Default branch for manual and scheduled builds** drop-down.</span></span> <span data-ttu-id="6e0e7-186">Нажмите кнопку **Continue** (Продолжить).</span><span class="sxs-lookup"><span data-stu-id="6e0e7-186">Click the **Continue** button.</span></span> <span data-ttu-id="6e0e7-187">Откроется страница выбора шаблона.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-187">The template selection page appears.</span></span>

### <a name="create-the-build-definition"></a><span data-ttu-id="6e0e7-188">Создание определения сборки</span><span class="sxs-lookup"><span data-stu-id="6e0e7-188">Create the build definition</span></span>

1. <span data-ttu-id="6e0e7-189">На странице выбора шаблона, ввести *ASP.NET Core* в поле поиска:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-189">From the template selection page, enter *ASP.NET Core* in the search box:</span></span>

    ![ASP.NET Core поиска на странице шаблона](media/cicd/vsts-template-selection.png)

1. <span data-ttu-id="6e0e7-191">Результаты поиска шаблона отображаются.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-191">The template search results appear.</span></span> <span data-ttu-id="6e0e7-192">Наведите указатель мыши **ASP.NET Core** шаблона и нажмите кнопку **применить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-192">Hover over the **ASP.NET Core** template, and click the **Apply** button.</span></span>
1. <span data-ttu-id="6e0e7-193">**Задачи** появится вкладка определения построения.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-193">The **Tasks** tab of the build definition appears.</span></span> <span data-ttu-id="6e0e7-194">Перейдите на вкладку **Триггеры** .</span><span class="sxs-lookup"><span data-stu-id="6e0e7-194">Click the **Triggers** tab.</span></span>
1. <span data-ttu-id="6e0e7-195">Проверьте **непрерывной интеграции** поле.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-195">Check the **Enable continuous integration** box.</span></span> <span data-ttu-id="6e0e7-196">В разделе **фильтры ветвей** разделе, убедитесь, что **тип** раскрывающегося списка присваивается *Include*.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-196">Under the **Branch filters** section, confirm that the **Type** drop-down is set to *Include*.</span></span> <span data-ttu-id="6e0e7-197">Задайте **спецификация ветви** раскрывающийся список, чтобы *master*.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-197">Set the **Branch specification** drop-down to *master*.</span></span>

    ![Включение параметров непрерывной интеграции](media/cicd/vsts-enable-ci.png)

    <span data-ttu-id="6e0e7-199">Эти параметры предписывают построения для запуска при передаче любое изменение *master* ветвь репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-199">These settings cause a build to trigger when any change is pushed to the *master* branch of the GitHub repository.</span></span> <span data-ttu-id="6e0e7-200">Непрерывная интеграция тестируется в [фиксации изменений в GitHub и автоматическое развертывание в Azure](#commit-changes-to-github-and-automatically-deploy-to-azure) раздел.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-200">Continuous integration is tested in the [Commit changes to GitHub and automatically deploy to Azure](#commit-changes-to-github-and-automatically-deploy-to-azure) section.</span></span>

1. <span data-ttu-id="6e0e7-201">Нажмите кнопку **сохранить и поместить в очередь** и выберите **Сохранить** параметр:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-201">Click the **Save & queue** button, and select the **Save** option:</span></span>

    ![Кнопка "Сохранить"](media/cicd/vsts-save-build.png)

1. <span data-ttu-id="6e0e7-203">Появится следующее модальное диалоговое окно:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-203">The following modal dialog appears:</span></span>

    ![Сохраните определение сборки - модальное диалоговое окно](media/cicd/vsts-save-modal.png)

    <span data-ttu-id="6e0e7-205">Использовать папку по умолчанию из *\\*и нажмите кнопку **Сохранить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-205">Use the default folder of *\\*, and click the **Save** button.</span></span>

### <a name="create-the-release-pipeline"></a><span data-ttu-id="6e0e7-206">Создайте конвейер выпусков</span><span class="sxs-lookup"><span data-stu-id="6e0e7-206">Create the release pipeline</span></span>

1. <span data-ttu-id="6e0e7-207">Нажмите кнопку **выпуски** вкладке командного проекта.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-207">Click the **Releases** tab of your team project.</span></span> <span data-ttu-id="6e0e7-208">Нажмите кнопку **новый конвейер** кнопки.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-208">Click the **New pipeline** button.</span></span>

    ![Вкладка "выпуски" — Кнопка "Создать определение"](media/cicd/vsts-new-release-definition.png)

    <span data-ttu-id="6e0e7-210">Откроется панель выбора шаблона.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-210">The template selection pane appears.</span></span>

1. <span data-ttu-id="6e0e7-211">На странице выбора шаблона, ввести *службы приложений* в поле поиска:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-211">From the template selection page, enter *App Service* in the search box:</span></span>

    ![Поле поиска шаблона конвейер выпуска](media/cicd/vsts-release-template-search.png)

1. <span data-ttu-id="6e0e7-213">Результаты поиска шаблона отображаются.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-213">The template search results appear.</span></span> <span data-ttu-id="6e0e7-214">Наведите указатель мыши **развертывание службы приложений Azure с помощью слота** шаблона и нажмите кнопку **применить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-214">Hover over the **Azure App Service Deployment with Slot** template, and click the **Apply** button.</span></span> <span data-ttu-id="6e0e7-215">**Конвейера** появится вкладка конвейера выпуска.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-215">The **Pipeline** tab of the release pipeline appears.</span></span>

    ![Конвейер выпуска вкладку конвейера](media/cicd/vsts-release-definition-pipeline.png)

1. <span data-ttu-id="6e0e7-217">Нажмите кнопку **добавить** кнопку **артефакты** поле.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-217">Click the **Add** button in the **Artifacts** box.</span></span> <span data-ttu-id="6e0e7-218">**Добавление артефакта** появится панель:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-218">The **Add artifact** panel appears:</span></span>

    ![Конвейер выпуска - артефакта панель добавления](media/cicd/vsts-release-add-artifact.png)

1. <span data-ttu-id="6e0e7-220">Выберите **построения** плитку **типа источника** раздел.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-220">Select the **Build** tile from the **Source type** section.</span></span> <span data-ttu-id="6e0e7-221">Этот тип обеспечивает создание ссылок конвейер выпуска в определение сборки.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-221">This type allows for the linking of the release pipeline to the build definition.</span></span>
1. <span data-ttu-id="6e0e7-222">Выберите *MyFirstProject* из **проекта** раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-222">Select *MyFirstProject* from the **Project** drop-down.</span></span>
1. <span data-ttu-id="6e0e7-223">Выберите имя определения построения *MyFirstProject ASP.NET Core-CI*, из **источник (определение сборки)** раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-223">Select the build definition name, *MyFirstProject-ASP.NET Core-CI*, from the **Source (Build definition)** drop-down.</span></span>
1. <span data-ttu-id="6e0e7-224">Выберите *последнюю* из **версия по умолчанию** раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-224">Select *Latest* from the **Default version** drop-down.</span></span> <span data-ttu-id="6e0e7-225">Этот параметр создает артефакты, созданные последним запуском определения построения.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-225">This option builds the artifacts produced by the latest run of the build definition.</span></span>
1. <span data-ttu-id="6e0e7-226">Замените текст в **псевдоним источника** textbox с *Drop*.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-226">Replace the text in the **Source alias** textbox with *Drop*.</span></span>
1. <span data-ttu-id="6e0e7-227">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-227">Click the **Add** button.</span></span> <span data-ttu-id="6e0e7-228">**Артефакты** разделе обновлений для отображения изменений.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-228">The **Artifacts** section updates to display the changes.</span></span>
1. <span data-ttu-id="6e0e7-229">Щелкните значок с молнией, чтобы включить непрерывное развертывание на:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-229">Click the lightning bolt icon to enable continuous deployments:</span></span>

    ![Артефакты — значок с изображением молнии. конвейер выпуска](media/cicd/vsts-artifacts-lightning-bolt.png)

    <span data-ttu-id="6e0e7-231">Этот параметр включен развертывание происходит каждый раз, когда становится доступной новая сборка.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-231">With this option enabled, a deployment occurs each time a new build is available.</span></span>
1. <span data-ttu-id="6e0e7-232">Объект **триггер непрерывного развертывания** панели справа.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-232">A **Continuous deployment trigger** panel appears to the right.</span></span> <span data-ttu-id="6e0e7-233">Чтобы включить эту функцию, нажмите кнопку переключателя.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-233">Click the toggle button to enable the feature.</span></span> <span data-ttu-id="6e0e7-234">Это необязательно включить **триггер запроса на включение внесенных изменений**.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-234">It isn't necessary to enable the **Pull request trigger**.</span></span>
1. <span data-ttu-id="6e0e7-235">Нажмите кнопку **добавить** раскрывающееся меню в **фильтры ветви сборки** раздел.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-235">Click the **Add** drop-down in the **Build branch filters** section.</span></span> <span data-ttu-id="6e0e7-236">Выберите **ветвь по умолчанию определение построения** параметр.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-236">Choose the **Build Definition's default branch** option.</span></span> <span data-ttu-id="6e0e7-237">Этот фильтр вызывает выпуска для активации только для сборок из репозитория GitHub *master* ветви.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-237">This filter causes the release to trigger only for a build from the GitHub repository's *master* branch.</span></span>
1. <span data-ttu-id="6e0e7-238">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-238">Click the **Save** button.</span></span> <span data-ttu-id="6e0e7-239">Нажмите кнопку **ОК** кнопки в итоговом **Сохранить** модальное диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-239">Click the **OK** button in the resulting **Save** modal dialog.</span></span>
1. <span data-ttu-id="6e0e7-240">Нажмите кнопку **среду 1** поле.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-240">Click the **Environment 1** box.</span></span> <span data-ttu-id="6e0e7-241">**Среды** панели справа.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-241">An **Environment** panel appears to the right.</span></span> <span data-ttu-id="6e0e7-242">Изменение *среду 1* текста в **имя среды** textbox для *рабочей*.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-242">Change the *Environment 1* text in the **Environment name** textbox to *Production*.</span></span>

   ![Конвейер выпуска - текстовое имя среды](media/cicd/vsts-environment-name-textbox.png)

1. <span data-ttu-id="6e0e7-244">Нажмите кнопку **этапа 1, 2 задачи** ссылку в **рабочей** поле:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-244">Click the **1 phase, 2 tasks** link in the **Production** box:</span></span>

    ![Конвейер выпуска - link.png рабочей среды](media/cicd/vsts-production-link.png)

    <span data-ttu-id="6e0e7-246">**Задачи** появится вкладка среды.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-246">The **Tasks** tab of the environment appears.</span></span>
1. <span data-ttu-id="6e0e7-247">Нажмите кнопку **развернуть службу приложений Azure для слота** задачи.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-247">Click the **Deploy Azure App Service to Slot** task.</span></span> <span data-ttu-id="6e0e7-248">Его параметры отображаются на панели справа.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-248">Its settings appear in a panel to the right.</span></span>
1. <span data-ttu-id="6e0e7-249">Выберите подписку Azure, связанные со службой приложений из **подписки Azure** раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-249">Select the Azure subscription associated with the App Service from the **Azure subscription** drop-down.</span></span> <span data-ttu-id="6e0e7-250">После выбора нажмите кнопку **Authorize** кнопки.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-250">Once selected, click the **Authorize** button.</span></span>
1. <span data-ttu-id="6e0e7-251">Выберите *веб-приложения* из **тип приложения** раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-251">Select *Web App* from the **App type** drop-down.</span></span>
1. <span data-ttu-id="6e0e7-252">Выберите *mywebapp / < unique_number / >* из **имя службы приложений** раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-252">Select *mywebapp/<unique_number/>* from the **App service name** drop-down.</span></span>
1. <span data-ttu-id="6e0e7-253">Выберите *AzureTutorial* из **группы ресурсов** раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-253">Select *AzureTutorial* from the **Resource group** drop-down.</span></span>
1. <span data-ttu-id="6e0e7-254">Выберите *промежуточных* из **слот** раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-254">Select *staging* from the **Slot** drop-down.</span></span>
1. <span data-ttu-id="6e0e7-255">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-255">Click the **Save** button.</span></span>
1. <span data-ttu-id="6e0e7-256">Наведите указатель на имя конвейера выпуска по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-256">Hover over the default release pipeline name.</span></span> <span data-ttu-id="6e0e7-257">Щелкните значок карандаша, чтобы изменить его.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-257">Click the pencil icon to edit it.</span></span> <span data-ttu-id="6e0e7-258">Используйте *MyFirstProject-Core-компакт-ДИСК ASP.NET* как имя.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-258">Use *MyFirstProject-ASP.NET Core-CD* as the name.</span></span>

    ![Имя конвейера выпуска](media/cicd/vsts-release-definition-name.png)

1. <span data-ttu-id="6e0e7-260">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-260">Click the **Save** button.</span></span>

## <a name="commit-changes-to-github-and-automatically-deploy-to-azure"></a><span data-ttu-id="6e0e7-261">Зафиксировать изменения в GitHub и автоматически развернуть в Azure</span><span class="sxs-lookup"><span data-stu-id="6e0e7-261">Commit changes to GitHub and automatically deploy to Azure</span></span>

1. <span data-ttu-id="6e0e7-262">Откройте *SimpleFeedReader.sln* в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-262">Open *SimpleFeedReader.sln* in Visual Studio.</span></span>
1. <span data-ttu-id="6e0e7-263">В обозревателе решений откройте *Pages\Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-263">In Solution Explorer, open *Pages\Index.cshtml*.</span></span> <span data-ttu-id="6e0e7-264">Изменение `<h2>Simple Feed Reader - V3</h2>` для `<h2>Simple Feed Reader - V4</h2>`.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-264">Change `<h2>Simple Feed Reader - V3</h2>` to `<h2>Simple Feed Reader - V4</h2>`.</span></span>
1. <span data-ttu-id="6e0e7-265">Нажмите клавишу **Ctrl**+**Shift**+**B** для сборки приложения.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-265">Press **Ctrl**+**Shift**+**B** to build the app.</span></span>
1. <span data-ttu-id="6e0e7-266">Зафиксируйте файл в репозитории GitHub.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-266">Commit the file to the GitHub repository.</span></span> <span data-ttu-id="6e0e7-267">Использовать **изменения** страницы в Visual Studio *Team Explorer* вкладку или выполнить следующую процедуру с помощью командной оболочки на локальном компьютере:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-267">Use either the **Changes** page in Visual Studio's *Team Explorer* tab, or execute the following using the local machine's command shell:</span></span>

    ```console
    git commit -a -m "upgraded to V4"
    ```
1. <span data-ttu-id="6e0e7-268">Отправка изменений *master* ветви *origin* удаленный репозиторий GitHub:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-268">Push the change in the *master* branch to the *origin* remote of your GitHub repository:</span></span>

    ```console
    git push origin master
    ```

    <span data-ttu-id="6e0e7-269">Фиксация появляется в репозитории GitHub *master* ветви:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-269">The commit appears in the GitHub repository's *master* branch:</span></span>

    ![Фиксации GitHub в главной ветви](media/cicd/github-commit.png)

    <span data-ttu-id="6e0e7-271">Сборка запускается, поскольку непрерывной интеграции включена в определении сборки **триггеры** вкладке:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-271">The build is triggered, since continuous integration is enabled in the build definition's **Triggers** tab:</span></span>

    ![включить непрерывную интеграцию](media/cicd/enable-ci.png)

1. <span data-ttu-id="6e0e7-273">Перейдите к **в очереди** вкладке **сборки и выпуска** > **построения** страницы в VSTS.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-273">Navigate to the **Queued** tab of the **Build and Release** > **Builds** page in VSTS.</span></span> <span data-ttu-id="6e0e7-274">Построения в очереди показывает ветви и фиксации, запустившей сборки:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-274">The queued build shows the branch and commit that triggered the build:</span></span>

    ![построения в очереди](media/cicd/build-queued.png)

1. <span data-ttu-id="6e0e7-276">После успешного построения, развертывания в Azure.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-276">Once the build succeeds, a deployment to Azure occurs.</span></span> <span data-ttu-id="6e0e7-277">Перейдите к приложению в браузере.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-277">Navigate to the app in the browser.</span></span> <span data-ttu-id="6e0e7-278">Обратите внимание на то, что текст «V4» отображается в заголовке:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-278">Notice that the "V4" text appears in the heading:</span></span>

    ![обновленное приложение](media/cicd/updated-app-v4.png)

## <a name="examine-the-vsts-devops-pipeline"></a><span data-ttu-id="6e0e7-280">Изучите конвейера VSTS DevOps</span><span class="sxs-lookup"><span data-stu-id="6e0e7-280">Examine the VSTS DevOps pipeline</span></span>

### <a name="build-definition"></a><span data-ttu-id="6e0e7-281">Определение сборки</span><span class="sxs-lookup"><span data-stu-id="6e0e7-281">Build definition</span></span>

<span data-ttu-id="6e0e7-282">Определение сборки был создан с именем *MyFirstProject ASP.NET Core-CI*.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-282">A build definition was created with the name *MyFirstProject-ASP.NET Core-CI*.</span></span> <span data-ttu-id="6e0e7-283">По завершении построения создает *ZIP-файл* файла, включая ресурсы для публикации.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-283">Upon completion, the build produces a *.zip* file including the assets to be published.</span></span> <span data-ttu-id="6e0e7-284">Конвейер выпуска развертывает эти ресурсы в Azure.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-284">The release pipeline deploys those assets to Azure.</span></span>

<span data-ttu-id="6e0e7-285">Определение сборки **задачи** вкладке перечислены отдельных действий.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-285">The build definition's **Tasks** tab lists the individual steps being used.</span></span> <span data-ttu-id="6e0e7-286">Есть пять задач сборки.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-286">There are five build tasks.</span></span>

![задачи определения сборки](media/cicd/build-definition-tasks.png)

1. <span data-ttu-id="6e0e7-288">**Восстановление** &mdash; интерфейсов `dotnet restore` команду, чтобы восстановить пакеты NuGet приложения.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-288">**Restore** &mdash; Executes the `dotnet restore` command to restore the app's NuGet packages.</span></span> <span data-ttu-id="6e0e7-289">Пакет по умолчанию канал, используемый является nuget.org.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-289">The default package feed used is nuget.org.</span></span>
1. <span data-ttu-id="6e0e7-290">**Построение** &mdash; интерфейсов `dotnet build --configuration release` команду для компиляции в код приложения.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-290">**Build** &mdash; Executes the `dotnet build --configuration release` command to compile the app's code.</span></span> <span data-ttu-id="6e0e7-291">Это `--configuration` параметр используется для создания оптимизированной версии кода, который подходит для развертывания в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-291">This `--configuration` option is used to produce an optimized version of the code, which is suitable for deployment to a production environment.</span></span> <span data-ttu-id="6e0e7-292">Изменить *BuildConfiguration* переменных с определением сборки **переменных** вкладку, например, конфигурации отладки при необходимости.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-292">Modify the *BuildConfiguration* variable on the build definition's **Variables** tab if, for example, a debug configuration is needed.</span></span>
1. <span data-ttu-id="6e0e7-293">**Тест** &mdash; интерфейсов `dotnet test --configuration release --logger trx --results-directory <local_path_on_build_agent>` команду для запуска модульных тестов приложения.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-293">**Test** &mdash; Executes the `dotnet test --configuration release --logger trx --results-directory <local_path_on_build_agent>` command to run the app's unit tests.</span></span> <span data-ttu-id="6e0e7-294">Модульные тесты выполняются в рамках любого C# проект сопоставления `**/*Tests/*.csproj` маски.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-294">Unit tests are executed within any C# project matching the `**/*Tests/*.csproj` glob pattern.</span></span> <span data-ttu-id="6e0e7-295">Результаты теста сохраняются в *.trx* файл в расположении, заданном параметром `--results-directory` параметр.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-295">Test results are saved in a *.trx* file at the location specified by the `--results-directory` option.</span></span> <span data-ttu-id="6e0e7-296">Если не все тесты, сборка завершается ошибкой и не был развернут.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-296">If any tests fail, the build fails and isn't deployed.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6e0e7-297">Чтобы проверить работу модульные тесты, измените *SimpleFeedReader.Tests\Services\NewsServiceTests.cs* намеренно прервать один из тестов.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-297">To verify the unit tests work, modify *SimpleFeedReader.Tests\Services\NewsServiceTests.cs* to purposefully break one of the tests.</span></span> <span data-ttu-id="6e0e7-298">Например, измените `Assert.True(result.Count > 0);` для `Assert.False(result.Count > 0);` в `Returns_News_Stories_Given_Valid_Uri` метод.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-298">For example, change `Assert.True(result.Count > 0);` to `Assert.False(result.Count > 0);` in the `Returns_News_Stories_Given_Valid_Uri` method.</span></span> <span data-ttu-id="6e0e7-299">Зафиксируйте и отправьте изменения в GitHub.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-299">Commit and push the change to GitHub.</span></span> <span data-ttu-id="6e0e7-300">Сборка запускается и завершается сбоем.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-300">The build is triggered and fails.</span></span> <span data-ttu-id="6e0e7-301">Состояние конвейера сборки меняется на **сбой**.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-301">The build pipeline status changes to **failed**.</span></span> <span data-ttu-id="6e0e7-302">Снова вернуться изменений, фиксации и отправки.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-302">Revert the change, commit, and push again.</span></span> <span data-ttu-id="6e0e7-303">Сборка будет завершена.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-303">The build succeeds.</span></span>

1. <span data-ttu-id="6e0e7-304">**Публикация** &mdash; интерфейсов `dotnet publish --configuration release --output <local_path_on_build_agent>` команду, чтобы создать *ZIP-файл* файл с артефактами, которые должны быть развернуты.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-304">**Publish** &mdash; Executes the `dotnet publish --configuration release --output <local_path_on_build_agent>` command to produce a *.zip* file with the artifacts to be deployed.</span></span> <span data-ttu-id="6e0e7-305">`--output` Параметр указывает расположение публикации *ZIP-файл* файл.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-305">The `--output` option specifies the publish location of the *.zip* file.</span></span> <span data-ttu-id="6e0e7-306">Что расположение задается путем передачи [Предопределенная переменная](https://docs.microsoft.com/vsts/pipelines/build/variables) с именем `$(build.artifactstagingdirectory)`.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-306">That location is specified by passing a [predefined variable](https://docs.microsoft.com/vsts/pipelines/build/variables) named `$(build.artifactstagingdirectory)`.</span></span> <span data-ttu-id="6e0e7-307">Эту переменную при развертывании по локальному пути, такие как *c:\agent\_work\1\a*, на агенте построения.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-307">That variable expands to a local path, such as *c:\agent\_work\1\a*, on the build agent.</span></span>
1. <span data-ttu-id="6e0e7-308">**Публикация артефакта** &mdash; Publishes *ZIP-файл* файлы, созданные функцией **публикации** задачи.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-308">**Publish Artifact** &mdash; Publishes the *.zip* file produced by the **Publish** task.</span></span> <span data-ttu-id="6e0e7-309">Задача принимает *ZIP-файл* расположение в качестве параметра, которое предварительно определенных файла `$(build.artifactstagingdirectory)`.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-309">The task accepts the *.zip* file location as a parameter, which is the predefined variable `$(build.artifactstagingdirectory)`.</span></span> <span data-ttu-id="6e0e7-310">*ZIP-файл* опубликован файл в папку с именем *drop*.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-310">The *.zip* file is published as a folder named *drop*.</span></span>

<span data-ttu-id="6e0e7-311">Выберите определение сборки **Сводка** ссылку, чтобы просмотреть журнал сборок с определением:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-311">Click the build definition's **Summary** link to view a history of builds with the definition:</span></span>

![журнале определения сборки](media/cicd/build-definition-summary.png)

<span data-ttu-id="6e0e7-313">На соответствующей странице щелкните ссылку, соответствующую уникальный номер:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-313">On the resulting page, click the link corresponding to the unique build number:</span></span>

![Страница сводки определения построения](media/cicd/build-definition-completed.png)

<span data-ttu-id="6e0e7-315">Отображается сводка этой конкретной сборки.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-315">A summary of this specific build is displayed.</span></span> <span data-ttu-id="6e0e7-316">Нажмите кнопку **артефакты** вкладку и обратите внимание, *drop* указана папка, созданного посредством сборки:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-316">Click the **Artifacts** tab, and notice the *drop* folder produced by the build is listed:</span></span>

![Определение артефакты - папки сброса сборки](media/cicd/build-definition-artifacts.png)

<span data-ttu-id="6e0e7-318">Используйте **загрузить** и **Проводник** ссылки для проверки опубликованного артефакты.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-318">Use the **Download** and **Explore** links to inspect the published artifacts.</span></span>

### <a name="release-pipeline"></a><span data-ttu-id="6e0e7-319">Конвейер выпуска</span><span class="sxs-lookup"><span data-stu-id="6e0e7-319">Release pipeline</span></span>

<span data-ttu-id="6e0e7-320">Конвейер выпуска был создан с именем *MyFirstProject-Core-компакт-ДИСК ASP.NET*:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-320">A release pipeline was created with the name *MyFirstProject-ASP.NET Core-CD*:</span></span>

![Общие сведения о конвейере выпуска](media/cicd/release-definition-overview.png)

<span data-ttu-id="6e0e7-322">Два основных компонента конвейера выпуска **артефакты** и **сред**.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-322">The two major components of the release pipeline are the **Artifacts** and the **Environments**.</span></span> <span data-ttu-id="6e0e7-323">При нажатии блока в **артефакты** раздел показывает следующие панели:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-323">Clicking the box in the **Artifacts** section reveals the following panel:</span></span>

![артефакты конвейер выпуска](media/cicd/release-definition-artifacts.png)

<span data-ttu-id="6e0e7-325">**Источник (определение сборки)** значение представляет определение сборки, с которым связан этот конвейер выпуска.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-325">The **Source (Build definition)** value represents the build definition to which this release pipeline is linked.</span></span> <span data-ttu-id="6e0e7-326">*ZIP-файл* предоставляется файл, созданный при успешном выполнении определения построения *рабочей* среду для развертывания в Azure.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-326">The *.zip* file produced by a successful run of the build definition is provided to the *Production* environment for deployment to Azure.</span></span> <span data-ttu-id="6e0e7-327">Нажмите кнопку *этапа 1, 2 задачи* ссылку в *рабочей* "среды" для просмотра задач конвейера выпуска:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-327">Click the *1 phase, 2 tasks* link in the *Production* environment box to view the release pipeline tasks:</span></span>

![задачи конвейер выпуска](media/cicd/release-definition-tasks.png)

<span data-ttu-id="6e0e7-329">Конвейер выпуска состоит из двух задач: *развернуть службу приложений Azure для слота* и *управление службой приложений Azure — для переключения слотов*.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-329">The release pipeline consists of two tasks: *Deploy Azure App Service to Slot* and *Manage Azure App Service - Slot Swap*.</span></span> <span data-ttu-id="6e0e7-330">Щелкнув первая задача раскрывает конфигурация следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-330">Clicking the first task reveals the following task configuration:</span></span>

![Задача развертывания конвейер выпуска](media/cicd/release-definition-task1.png)

<span data-ttu-id="6e0e7-332">Подписка Azure, тип службы, имя веб-приложения, группу ресурсов и слот развертывания определяются в задачи развертывания.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-332">The Azure subscription, service type, web app name, resource group, and deployment slot are defined in the deployment task.</span></span> <span data-ttu-id="6e0e7-333">**Пакет или папка** содержит текстовое поле *ZIP-файл* путь к файлу, чтобы извлечь и развернуть для *промежуточных* слоте *mywebapp\<уникальный _Число\>*  веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-333">The **Package or folder** textbox holds the *.zip* file path to be extracted and deployed to the *staging* slot of the *mywebapp\<unique_number\>* web app.</span></span>

<span data-ttu-id="6e0e7-334">Щелкнув задачу замены слота раскрывает конфигурация следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="6e0e7-334">Clicking the slot swap task reveals the following task configuration:</span></span>

![выпуск конвейера слот переключения задач](media/cicd/release-definition-task2.png)

<span data-ttu-id="6e0e7-336">Предоставляются подписки, группы ресурсов, тип службы, имя веб-приложения и сведения о слот развертывания.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-336">The subscription, resource group, service type, web app name, and deployment slot details are provided.</span></span> <span data-ttu-id="6e0e7-337">**Переключить на рабочий слот** флажок.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-337">The **Swap with Production** checkbox is checked.</span></span> <span data-ttu-id="6e0e7-338">Следовательно, биты развертывания *промежуточных* меняются местами слот в рабочую среду.</span><span class="sxs-lookup"><span data-stu-id="6e0e7-338">Consequently, the bits deployed to the *staging* slot are swapped into the production environment.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="6e0e7-339">Дополнительные материалы для чтения</span><span class="sxs-lookup"><span data-stu-id="6e0e7-339">Additional reading</span></span>

* [<span data-ttu-id="6e0e7-340">Сборка приложения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6e0e7-340">Build your ASP.NET Core app</span></span>](https://docs.microsoft.com/vsts/build-release/apps/aspnet/build-aspnet-core)
* [<span data-ttu-id="6e0e7-341">Создание и развертывание в веб-приложение Azure</span><span class="sxs-lookup"><span data-stu-id="6e0e7-341">Build and deploy to an Azure Web App</span></span>](https://docs.microsoft.com/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp)
* [<span data-ttu-id="6e0e7-342">Определение процесса сборки CI для репозитория GitHub</span><span class="sxs-lookup"><span data-stu-id="6e0e7-342">Define a CI build process for your GitHub repository</span></span>](https://docs.microsoft.com/vsts/pipelines/build/ci-build-github)
