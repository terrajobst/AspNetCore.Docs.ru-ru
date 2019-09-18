---
title: Развертывание приложения в службе приложений — DevOps с помощью ASP.NET Core и Azure
author: CamSoper
description: Развертывание приложения ASP.NET Core в службе приложений Azure, первым шагом для разработки и операций с ASP.NET Core и Azure.
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/deploy-to-app-service
ms.openlocfilehash: df41f296e9c4e1eff6e31d45b29ec30ee1e20cf4
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080441"
---
# <a name="deploy-an-app-to-app-service"></a><span data-ttu-id="c8dac-103">Развертывание приложения в службе приложений</span><span class="sxs-lookup"><span data-stu-id="c8dac-103">Deploy an app to App Service</span></span>

<span data-ttu-id="c8dac-104">[Служба приложений Azure](/azure/app-service/) Azure веб-платформы размещения.</span><span class="sxs-lookup"><span data-stu-id="c8dac-104">[Azure App Service](/azure/app-service/) is Azure's web hosting platform.</span></span> <span data-ttu-id="c8dac-105">Развертывание веб-приложения в службе приложений Azure можно сделать вручную или с помощью автоматизированного процесса.</span><span class="sxs-lookup"><span data-stu-id="c8dac-105">Deploying a web app to Azure App Service can be done manually or by an automated process.</span></span> <span data-ttu-id="c8dac-106">В этом разделе руководства рассматриваются методы развертывания, которые могут выполняться вручную или с помощью командной строки скрипта, или запускаются вручную с помощью Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c8dac-106">This section of the guide discusses deployment methods that can be triggered manually or by script using the command line, or triggered manually using Visual Studio.</span></span>

<span data-ttu-id="c8dac-107">В этом разделе вы будете выполнять следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="c8dac-107">In this section, you'll accomplish the following tasks:</span></span>

* <span data-ttu-id="c8dac-108">Скачайте и созданию примера приложения.</span><span class="sxs-lookup"><span data-stu-id="c8dac-108">Download and build the sample app.</span></span>
* <span data-ttu-id="c8dac-109">Создание приложения службы веб-приложение Azure с помощью Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="c8dac-109">Create an Azure App Service Web App using the Azure Cloud Shell.</span></span>
* <span data-ttu-id="c8dac-110">Развертывание примера приложения в Azure с помощью Git.</span><span class="sxs-lookup"><span data-stu-id="c8dac-110">Deploy the sample app to Azure using Git.</span></span>
* <span data-ttu-id="c8dac-111">Развертывание изменений в приложение, с помощью Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c8dac-111">Deploy a change to the app using Visual Studio.</span></span>
* <span data-ttu-id="c8dac-112">Добавьте промежуточный слот для веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="c8dac-112">Add a staging slot to the web app.</span></span>
* <span data-ttu-id="c8dac-113">Развертывание обновления в промежуточном слоте.</span><span class="sxs-lookup"><span data-stu-id="c8dac-113">Deploy an update to the staging slot.</span></span>
* <span data-ttu-id="c8dac-114">Переключить слоты промежуточной и рабочей.</span><span class="sxs-lookup"><span data-stu-id="c8dac-114">Swap the staging and production slots.</span></span>

## <a name="download-and-test-the-app"></a><span data-ttu-id="c8dac-115">Скачайте и тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="c8dac-115">Download and test the app</span></span>

<span data-ttu-id="c8dac-116">Приложение, используемое в этом руководстве, является предварительно созданные приложения ASP.NET Core, [простой веб-канала чтения](https://github.com/Azure-Samples/simple-feed-reader/).</span><span class="sxs-lookup"><span data-stu-id="c8dac-116">The app used in this guide is a pre-built ASP.NET Core app, [Simple Feed Reader](https://github.com/Azure-Samples/simple-feed-reader/).</span></span> <span data-ttu-id="c8dac-117">Это приложение Razor Pages, использующий `Microsoft.SyndicationFeed.ReaderWriter` API для получения канал RSS/Atom и отображения элементов новостей из списка.</span><span class="sxs-lookup"><span data-stu-id="c8dac-117">It's a Razor Pages app that uses the `Microsoft.SyndicationFeed.ReaderWriter` API to retrieve an RSS/Atom feed and display the news items in a list.</span></span>

<span data-ttu-id="c8dac-118">Вы можете просмотреть код, но важно понимать, что нет ничего особенного в отношении этого приложения.</span><span class="sxs-lookup"><span data-stu-id="c8dac-118">Feel free to review the code, but it's important to understand that there's nothing special about this app.</span></span> <span data-ttu-id="c8dac-119">Это просто простое приложение ASP.NET Core для иллюстративных целей.</span><span class="sxs-lookup"><span data-stu-id="c8dac-119">It's just a simple ASP.NET Core app for illustrative purposes.</span></span>

<span data-ttu-id="c8dac-120">Из командной оболочки Загрузите код, постройте проект и запустите его следующим образом.</span><span class="sxs-lookup"><span data-stu-id="c8dac-120">From a command shell, download the code, build the project, and run it as follows.</span></span>

> <span data-ttu-id="c8dac-121">*Примечание. Пользователи Linux и macOS должны внести соответствующие изменения в пути, например, используя косую черту`/`(), а не обратную косую черту (`\`).*</span><span class="sxs-lookup"><span data-stu-id="c8dac-121">*Note: Linux/macOS users should make appropriate changes for paths, e.g., using forward slash (`/`) rather than back slash (`\`).*</span></span>

1. <span data-ttu-id="c8dac-122">Клонируйте код на папку на локальном компьютере.</span><span class="sxs-lookup"><span data-stu-id="c8dac-122">Clone the code to a folder on your local machine.</span></span>

    ```console
    git clone https://github.com/Azure-Samples/simple-feed-reader/
    ```

2. <span data-ttu-id="c8dac-123">Изменить вашу рабочую папку для *чтения simple потоков* папку, которая была создана.</span><span class="sxs-lookup"><span data-stu-id="c8dac-123">Change your working folder to the *simple-feed-reader* folder that was created.</span></span>

    ```console
    cd .\simple-feed-reader\SimpleFeedReader
    ```

3. <span data-ttu-id="c8dac-124">Восстановите пакеты и выполните сборку решения.</span><span class="sxs-lookup"><span data-stu-id="c8dac-124">Restore the packages, and build the solution.</span></span>

    ```dotnetcli
    dotnet build
    ```

4. <span data-ttu-id="c8dac-125">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="c8dac-125">Run the app.</span></span>

    ```dotnetcli
    dotnet run
    ```

    ![Команда dotnet run прошла успешно](./media/deploying-to-app-service/dotnet-run.png)

5. <span data-ttu-id="c8dac-127">Откройте браузер и перейдите к `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="c8dac-127">Open a browser and navigate to `http://localhost:5000`.</span></span> <span data-ttu-id="c8dac-128">Приложение позволяет введите или вставьте URL-адрес канала синдикации и просмотреть список элементов новостей.</span><span class="sxs-lookup"><span data-stu-id="c8dac-128">The app allows you to type or paste a syndication feed URL and view a list of news items.</span></span>

     ![Приложения, отображая содержание RSS-канал](./media/deploying-to-app-service/app-in-browser.png)

6. <span data-ttu-id="c8dac-130">После проверки правильности работы приложения, выключить его, нажав клавишу **Ctrl**+**C** в командной оболочке.</span><span class="sxs-lookup"><span data-stu-id="c8dac-130">Once you're satisfied the app is working correctly, shut it down by pressing **Ctrl**+**C** in the command shell.</span></span>

## <a name="create-the-azure-app-service-web-app"></a><span data-ttu-id="c8dac-131">Создание веб-приложения службы приложений Azure</span><span class="sxs-lookup"><span data-stu-id="c8dac-131">Create the Azure App Service Web App</span></span>

<span data-ttu-id="c8dac-132">Чтобы развернуть приложение, необходимо создать службу приложений [веб-приложение](/azure/app-service/app-service-web-overview).</span><span class="sxs-lookup"><span data-stu-id="c8dac-132">To deploy the app, you'll need to create an App Service [Web App](/azure/app-service/app-service-web-overview).</span></span> <span data-ttu-id="c8dac-133">После создания веб-приложения вы развернете в него с локального компьютера с помощью Git.</span><span class="sxs-lookup"><span data-stu-id="c8dac-133">After creation of the Web App, you'll deploy to it from your local machine using Git.</span></span>

1. <span data-ttu-id="c8dac-134">Войдите в [Azure Cloud Shell](https://shell.azure.com/bash).</span><span class="sxs-lookup"><span data-stu-id="c8dac-134">Sign in to the [Azure Cloud Shell](https://shell.azure.com/bash).</span></span> <span data-ttu-id="c8dac-135">Примечание. При первом входе Cloud Shell запрос на создание учетной записи хранения для файлов конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c8dac-135">Note: When you sign in for the first time, Cloud Shell prompts to create a storage account for configuration files.</span></span> <span data-ttu-id="c8dac-136">Примите значения по умолчанию или задайте уникальное имя.</span><span class="sxs-lookup"><span data-stu-id="c8dac-136">Accept the defaults or provide a unique name.</span></span>

2. <span data-ttu-id="c8dac-137">Используйте Cloud Shell для выполнения следующих действий.</span><span class="sxs-lookup"><span data-stu-id="c8dac-137">Use the Cloud Shell for the following steps.</span></span>

    <span data-ttu-id="c8dac-138">1\.</span><span class="sxs-lookup"><span data-stu-id="c8dac-138">a.</span></span> <span data-ttu-id="c8dac-139">Объявите переменную для хранения имени веб приложения.</span><span class="sxs-lookup"><span data-stu-id="c8dac-139">Declare a variable to store your web app's name.</span></span> <span data-ttu-id="c8dac-140">Имя должно быть уникальным для использования в URL-адрес по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="c8dac-140">The name must be unique to be used in the default URL.</span></span> <span data-ttu-id="c8dac-141">С помощью `$RANDOM` функция Bash для создания имени, гарантирующее уникальность и результаты в формате `webappname99999`.</span><span class="sxs-lookup"><span data-stu-id="c8dac-141">Using the `$RANDOM` Bash function to construct the name guarantees uniqueness and results in the format `webappname99999`.</span></span>

    ```console
    webappname=mywebapp$RANDOM
    ```

    <span data-ttu-id="c8dac-142">2\.</span><span class="sxs-lookup"><span data-stu-id="c8dac-142">b.</span></span> <span data-ttu-id="c8dac-143">Создайте группу ресурсов.</span><span class="sxs-lookup"><span data-stu-id="c8dac-143">Create a resource group.</span></span> <span data-ttu-id="c8dac-144">Группы ресурсов позволяют объединять ресурсы Azure, которые администрируются как группа.</span><span class="sxs-lookup"><span data-stu-id="c8dac-144">Resource groups provide a means to aggregate Azure resources to be managed as a group.</span></span>

    ```azure-cli
    az group create --location centralus --name AzureTutorial
    ```

    <span data-ttu-id="c8dac-145">`az` Команда вызывает [Azure CLI](/cli/azure/).</span><span class="sxs-lookup"><span data-stu-id="c8dac-145">The `az` command invokes the [Azure CLI](/cli/azure/).</span></span> <span data-ttu-id="c8dac-146">Интерфейс командной строки можно выполнять локально, но использовать его в Cloud Shell экономит время и конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c8dac-146">The CLI can be run locally, but using it in the Cloud Shell saves time and configuration.</span></span>

    <span data-ttu-id="c8dac-147">В.</span><span class="sxs-lookup"><span data-stu-id="c8dac-147">c.</span></span> <span data-ttu-id="c8dac-148">Создайте план службы приложений на уровне S1.</span><span class="sxs-lookup"><span data-stu-id="c8dac-148">Create an App Service plan in the S1 tier.</span></span> <span data-ttu-id="c8dac-149">План службы приложений — это группа веб-приложений, которые совместно используют тот же уровень ценообразования.</span><span class="sxs-lookup"><span data-stu-id="c8dac-149">An App Service plan is a grouping of web apps that share the same pricing tier.</span></span> <span data-ttu-id="c8dac-150">Уровень на S1 не бесплатные, но это обязательно для функцию промежуточных слотов.</span><span class="sxs-lookup"><span data-stu-id="c8dac-150">The S1 tier isn't free, but it's required for the staging slots feature.</span></span>

    ```azure-cli
    az appservice plan create --name $webappname --resource-group AzureTutorial --sku S1
    ```

    <span data-ttu-id="c8dac-151">Г.</span><span class="sxs-lookup"><span data-stu-id="c8dac-151">d.</span></span> <span data-ttu-id="c8dac-152">Создайте ресурс веб-приложения, с помощью плана службы приложений в той же группе ресурсов.</span><span class="sxs-lookup"><span data-stu-id="c8dac-152">Create the web app resource using the App Service plan in the same resource group.</span></span>

    ```azure-cli
    az webapp create --name $webappname --resource-group AzureTutorial --plan $webappname
    ```

    <span data-ttu-id="c8dac-153">Д.</span><span class="sxs-lookup"><span data-stu-id="c8dac-153">e.</span></span> <span data-ttu-id="c8dac-154">Настройка учетных данных развертывания.</span><span class="sxs-lookup"><span data-stu-id="c8dac-154">Set the deployment credentials.</span></span> <span data-ttu-id="c8dac-155">Эти учетные данные развертывания применяются к веб-приложений в вашей подписке.</span><span class="sxs-lookup"><span data-stu-id="c8dac-155">These deployment credentials apply to all the web apps in your subscription.</span></span> <span data-ttu-id="c8dac-156">Не используйте специальные символы в имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="c8dac-156">Don't use special characters in the user name.</span></span>

    ```azure-cli
    az webapp deployment user set --user-name REPLACE_WITH_USER_NAME --password REPLACE_WITH_PASSWORD
    ```

    <span data-ttu-id="c8dac-157">f.</span><span class="sxs-lookup"><span data-stu-id="c8dac-157">f.</span></span> <span data-ttu-id="c8dac-158">Настройка веб-приложения, чтобы принять развертывание из локального Git и отображения *URL-адрес развертывания Git*.</span><span class="sxs-lookup"><span data-stu-id="c8dac-158">Configure the web app to accept deployments from local Git and display the *Git deployment URL*.</span></span> <span data-ttu-id="c8dac-159">**Запишите этот URL-адрес для ссылки на более поздней версии**.</span><span class="sxs-lookup"><span data-stu-id="c8dac-159">**Note this URL for reference later**.</span></span>

    ```azure-cli
    echo Git deployment URL: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --query url --output tsv)
    ```

    <span data-ttu-id="c8dac-160">ж.</span><span class="sxs-lookup"><span data-stu-id="c8dac-160">g.</span></span> <span data-ttu-id="c8dac-161">Отображение *веб-приложения URL-адрес*.</span><span class="sxs-lookup"><span data-stu-id="c8dac-161">Display the *web app URL*.</span></span> <span data-ttu-id="c8dac-162">Найдите этот URL-адрес, чтобы увидеть пустое веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="c8dac-162">Browse to this URL to see the blank web app.</span></span> <span data-ttu-id="c8dac-163">**Запишите этот URL-адрес для ссылки на более поздней версии**.</span><span class="sxs-lookup"><span data-stu-id="c8dac-163">**Note this URL for reference later**.</span></span>

    ```console
    echo Web app URL: http://$webappname.azurewebsites.net
    ```

3. <span data-ttu-id="c8dac-164">С помощью командной оболочки на локальном компьютере, перейдите к папке проекта веб приложения (например, `.\simple-feed-reader\SimpleFeedReader`).</span><span class="sxs-lookup"><span data-stu-id="c8dac-164">Using a command shell on your local machine, navigate to the web app's project folder (for example, `.\simple-feed-reader\SimpleFeedReader`).</span></span> <span data-ttu-id="c8dac-165">Выполните следующие команды, чтобы настроить Git для отправки в URL-адрес развертывания:</span><span class="sxs-lookup"><span data-stu-id="c8dac-165">Execute the following commands to set up Git to push to the deployment URL:</span></span>

    <span data-ttu-id="c8dac-166">1\.</span><span class="sxs-lookup"><span data-stu-id="c8dac-166">a.</span></span> <span data-ttu-id="c8dac-167">Добавьте URL-адрес удаленного в локальном репозитории.</span><span class="sxs-lookup"><span data-stu-id="c8dac-167">Add the remote URL to the local repository.</span></span>

    ```console
    git remote add azure-prod GIT_DEPLOYMENT_URL
    ```

    <span data-ttu-id="c8dac-168">2\.</span><span class="sxs-lookup"><span data-stu-id="c8dac-168">b.</span></span> <span data-ttu-id="c8dac-169">Push-локальной *master* ветви *azure prod* удаленного элемента *master* ветви.</span><span class="sxs-lookup"><span data-stu-id="c8dac-169">Push the local *master* branch to the *azure-prod* remote's *master* branch.</span></span>

    ```console
    git push azure-prod master
    ```

    <span data-ttu-id="c8dac-170">Вам предложат ввести учетные данные развертывания, созданную ранее.</span><span class="sxs-lookup"><span data-stu-id="c8dac-170">You'll be prompted for the deployment credentials you created earlier.</span></span> <span data-ttu-id="c8dac-171">Просмотрите выходные данные, в командной оболочке.</span><span class="sxs-lookup"><span data-stu-id="c8dac-171">Observe the output in the command shell.</span></span> <span data-ttu-id="c8dac-172">Azure выполняет сборку приложения ASP.NET Core удаленно.</span><span class="sxs-lookup"><span data-stu-id="c8dac-172">Azure builds the ASP.NET Core app remotely.</span></span>

4. <span data-ttu-id="c8dac-173">В браузере перейдите к *веб-приложения URL-адрес* и обратите внимание на то, приложение будет собрано и развернуто.</span><span class="sxs-lookup"><span data-stu-id="c8dac-173">In a browser, navigate to the *Web app URL* and note the app has been built and deployed.</span></span> <span data-ttu-id="c8dac-174">Дополнительные изменения можно было зафиксировать в локальном репозитории Git с `git commit`.</span><span class="sxs-lookup"><span data-stu-id="c8dac-174">Additional changes can be committed to the local Git repository with `git commit`.</span></span> <span data-ttu-id="c8dac-175">Эти изменения будут передаваться в Azure с предыдущим `git push` команды.</span><span class="sxs-lookup"><span data-stu-id="c8dac-175">These changes are pushed to Azure with the preceding `git push` command.</span></span>

## <a name="deployment-with-visual-studio"></a><span data-ttu-id="c8dac-176">Развертывание с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c8dac-176">Deployment with Visual Studio</span></span>

> <span data-ttu-id="c8dac-177">*Примечание. Этот раздел относится только к Windows. Пользователи Linux и macOS необходимо внести изменение, описано в шаге 2 ниже. Сохраните файл и зафиксируйте изменения в локальном репозитории с `git commit`. Наконец, отправьте изменения, с помощью `git push`, как показано в первом разделе.*</span><span class="sxs-lookup"><span data-stu-id="c8dac-177">*Note: This section applies to Windows only. Linux and macOS users should make the change described in step 2 below. Save the file, and commit the change to the local repository with `git commit`. Finally, push the change with `git push`, as in the first section.*</span></span>

<span data-ttu-id="c8dac-178">Приложение уже развернуто в командной оболочке.</span><span class="sxs-lookup"><span data-stu-id="c8dac-178">The app has already been deployed from the command shell.</span></span> <span data-ttu-id="c8dac-179">Давайте использовать интегрированные инструменты Visual Studio для развертывания обновления в приложение.</span><span class="sxs-lookup"><span data-stu-id="c8dac-179">Let's use Visual Studio's integrated tools to deploy an update to the app.</span></span> <span data-ttu-id="c8dac-180">По сути Visual Studio выполняет то же самое, что и средства командной строки, но в знакомый пользовательский Интерфейс Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c8dac-180">Behind the scenes, Visual Studio accomplishes the same thing as the command line tooling, but within Visual Studio's familiar UI.</span></span>

1. <span data-ttu-id="c8dac-181">Откройте *SimpleFeedReader.sln* в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c8dac-181">Open *SimpleFeedReader.sln* in Visual Studio.</span></span>
2. <span data-ttu-id="c8dac-182">В обозревателе решений откройте *Pages\Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c8dac-182">In Solution Explorer, open *Pages\Index.cshtml*.</span></span> <span data-ttu-id="c8dac-183">Изменение `<h2>Simple Feed Reader</h2>` для `<h2>Simple Feed Reader - V2</h2>`.</span><span class="sxs-lookup"><span data-stu-id="c8dac-183">Change `<h2>Simple Feed Reader</h2>` to `<h2>Simple Feed Reader - V2</h2>`.</span></span>
3. <span data-ttu-id="c8dac-184">Нажмите клавишу **Ctrl**+**Shift**+**B** для сборки приложения.</span><span class="sxs-lookup"><span data-stu-id="c8dac-184">Press **Ctrl**+**Shift**+**B** to build the app.</span></span>
4. <span data-ttu-id="c8dac-185">В обозревателе решений щелкните правой кнопкой мыши проект и нажмите кнопку **публикации**.</span><span class="sxs-lookup"><span data-stu-id="c8dac-185">In Solution Explorer, right-click on the project and click **Publish**.</span></span>

    ![Снимок экрана, показывающий щелкните правой кнопкой мыши, публикация](./media/deploying-to-app-service/publish.png)
5. <span data-ttu-id="c8dac-187">Visual Studio можно создать новый ресурс службы приложений, но это обновление будет публиковаться через существующее развертывание.</span><span class="sxs-lookup"><span data-stu-id="c8dac-187">Visual Studio can create a new App Service resource, but this update will be published over the existing deployment.</span></span> <span data-ttu-id="c8dac-188">В **выберите целевой объект публикации** диалоговом окне выберите **службы приложений** в списке слева, а затем выберите **выбрать существующее**.</span><span class="sxs-lookup"><span data-stu-id="c8dac-188">In the **Pick a publish target** dialog, select **App Service** from the list on the left, and then select **Select Existing**.</span></span> <span data-ttu-id="c8dac-189">Нажмите кнопку **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="c8dac-189">Click **Publish**.</span></span>
6. <span data-ttu-id="c8dac-190">В **службы приложений** диалоговом окне подтвердите, что Майкрософт или организационной учетной записи, используемой для создания подписки на Azure в правом верхнем углу.</span><span class="sxs-lookup"><span data-stu-id="c8dac-190">In the **App Service** dialog, confirm that the Microsoft or Organizational account used to create your Azure subscription is displayed in the upper right.</span></span> <span data-ttu-id="c8dac-191">Если это не так, нажмите кнопку раскрывающегося списка и добавьте его.</span><span class="sxs-lookup"><span data-stu-id="c8dac-191">If it's not, click the drop-down and add it.</span></span>
7. <span data-ttu-id="c8dac-192">Убедитесь, что правильный Azure **подписки** выбран.</span><span class="sxs-lookup"><span data-stu-id="c8dac-192">Confirm that the correct Azure **Subscription** is selected.</span></span> <span data-ttu-id="c8dac-193">Для **представление**выберите **группы ресурсов**.</span><span class="sxs-lookup"><span data-stu-id="c8dac-193">For **View**, select **Resource Group**.</span></span> <span data-ttu-id="c8dac-194">Разверните **AzureTutorial** группу ресурсов, а затем выберите существующее веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="c8dac-194">Expand the **AzureTutorial** resource group and then select the existing web app.</span></span> <span data-ttu-id="c8dac-195">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="c8dac-195">Click **OK**.</span></span>

    ![Диалоговое окно публикации службы приложений снимок экрана, показывающий](./media/deploying-to-app-service/publish-dialog.png)

<span data-ttu-id="c8dac-197">Visual Studio создает и развертывает приложение в Azure.</span><span class="sxs-lookup"><span data-stu-id="c8dac-197">Visual Studio builds and deploys the app to Azure.</span></span> <span data-ttu-id="c8dac-198">Перейдите к URL-адрес приложения.</span><span class="sxs-lookup"><span data-stu-id="c8dac-198">Browse to the web app URL.</span></span> <span data-ttu-id="c8dac-199">Убедитесь, что `<h2>` изменения элемента в реальном времени.</span><span class="sxs-lookup"><span data-stu-id="c8dac-199">Validate that the `<h2>` element modification is live.</span></span>

![Приложение с помощью измененный заголовок](./media/deploying-to-app-service/app-v2.png)

## <a name="deployment-slots"></a><span data-ttu-id="c8dac-201">Слоты развертывания</span><span class="sxs-lookup"><span data-stu-id="c8dac-201">Deployment slots</span></span>

<span data-ttu-id="c8dac-202">Слоты развертывания поддерживают промежуточного хранения изменений, не влияя на приложения, выполняемого в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="c8dac-202">Deployment slots support the staging of changes without impacting the app running in production.</span></span> <span data-ttu-id="c8dac-203">После проверки промежуточную версию приложения по качества работы команды можно переключать рабочий и промежуточный слоты.</span><span class="sxs-lookup"><span data-stu-id="c8dac-203">Once the staged version of the app is validated by a quality assurance team, the production and staging slots can be swapped.</span></span> <span data-ttu-id="c8dac-204">Приложение в промежуточной среде перемещается в рабочую среду, таким образом.</span><span class="sxs-lookup"><span data-stu-id="c8dac-204">The app in staging is promoted to production in this manner.</span></span> <span data-ttu-id="c8dac-205">Следующие действия создать промежуточный слот, развернуть некоторые изменения в него и переключите промежуточный слот с рабочей среде после проверки.</span><span class="sxs-lookup"><span data-stu-id="c8dac-205">The following steps create a staging slot, deploy some changes to it, and swap the staging slot with production after verification.</span></span>

1. <span data-ttu-id="c8dac-206">Войдите в [Azure Cloud Shell](https://shell.azure.com/bash), если еще не выполнил вход.</span><span class="sxs-lookup"><span data-stu-id="c8dac-206">Sign in to the [Azure Cloud Shell](https://shell.azure.com/bash), if not already signed in.</span></span>
2. <span data-ttu-id="c8dac-207">Создание промежуточного слота.</span><span class="sxs-lookup"><span data-stu-id="c8dac-207">Create the staging slot.</span></span>

    <span data-ttu-id="c8dac-208">1\.</span><span class="sxs-lookup"><span data-stu-id="c8dac-208">a.</span></span> <span data-ttu-id="c8dac-209">Создайте слот развертывания с именем *промежуточных*.</span><span class="sxs-lookup"><span data-stu-id="c8dac-209">Create a deployment slot with the name *staging*.</span></span>

    ```azure-cli
    az webapp deployment slot create --name $webappname --resource-group AzureTutorial --slot staging
    ```

    <span data-ttu-id="c8dac-210">2\.</span><span class="sxs-lookup"><span data-stu-id="c8dac-210">b.</span></span> <span data-ttu-id="c8dac-211">Настройте промежуточный слот, чтобы использовать развертывание из локального Git и get **промежуточных** URL-адрес развертывания.</span><span class="sxs-lookup"><span data-stu-id="c8dac-211">Configure the staging slot to use deployment from local Git and get the **staging** deployment URL.</span></span> <span data-ttu-id="c8dac-212">**Запишите этот URL-адрес для ссылки на более поздней версии**.</span><span class="sxs-lookup"><span data-stu-id="c8dac-212">**Note this URL for reference later**.</span></span>

    ```azure-cli
    echo Git deployment URL for staging: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --slot staging --query url --output tsv)
    ```

    <span data-ttu-id="c8dac-213">В.</span><span class="sxs-lookup"><span data-stu-id="c8dac-213">c.</span></span> <span data-ttu-id="c8dac-214">Отображать URL-адрес промежуточного слота.</span><span class="sxs-lookup"><span data-stu-id="c8dac-214">Display the staging slot's URL.</span></span> <span data-ttu-id="c8dac-215">Перейдите на URL-адрес, чтобы увидеть пустой промежуточный слот.</span><span class="sxs-lookup"><span data-stu-id="c8dac-215">Browse to the URL to see the empty staging slot.</span></span> <span data-ttu-id="c8dac-216">**Запишите этот URL-адрес для ссылки на более поздней версии**.</span><span class="sxs-lookup"><span data-stu-id="c8dac-216">**Note this URL for reference later**.</span></span>

    ```console
    echo Staging web app URL: http://$webappname-staging.azurewebsites.net
    ```

3. <span data-ttu-id="c8dac-217">В текстовом редакторе или Visual Studio, измените *Pages/Index.cshtml* еще раз, чтобы `<h2>` считывает элемент `<h2>Simple Feed Reader - V3</h2>` и сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="c8dac-217">In a text editor or Visual Studio, modify *Pages/Index.cshtml* again so that the `<h2>` element reads `<h2>Simple Feed Reader - V3</h2>` and save the file.</span></span>

4. <span data-ttu-id="c8dac-218">Зафиксировать файл в локальном репозитории Git, с помощью **изменения** страницы в Visual Studio *Team Explorer* вкладке или путем ввода следующих с помощью командной оболочки на локальном компьютере:</span><span class="sxs-lookup"><span data-stu-id="c8dac-218">Commit the file to the local Git repository, using either the **Changes** page in Visual Studio's *Team Explorer* tab, or by entering the following using the local machine's command shell:</span></span>

    ```console
    git commit -a -m "upgraded to V3"
    ```

5. <span data-ttu-id="c8dac-219">С помощью командной оболочки на локальном компьютере, добавьте URL-адрес промежуточного развертывания в качестве удаленного репозитория Git и отправьте зафиксированные изменения.</span><span class="sxs-lookup"><span data-stu-id="c8dac-219">Using the local machine's command shell, add the staging deployment URL as a Git remote and push the committed changes:</span></span>

    <span data-ttu-id="c8dac-220">1\.</span><span class="sxs-lookup"><span data-stu-id="c8dac-220">a.</span></span> <span data-ttu-id="c8dac-221">Добавьте удаленный URL-адрес для промежуточного хранения в локальном репозитории Git.</span><span class="sxs-lookup"><span data-stu-id="c8dac-221">Add the remote URL for staging to the local Git repository.</span></span>

    ```console
    git remote add azure-staging <Git_staging_deployment_URL>
    ```

    <span data-ttu-id="c8dac-222">2\.</span><span class="sxs-lookup"><span data-stu-id="c8dac-222">b.</span></span> <span data-ttu-id="c8dac-223">Push-локальной *master* ветви для *azure промежуточной* удаленного элемента *master* ветви.</span><span class="sxs-lookup"><span data-stu-id="c8dac-223">Push the local *master* branch to the *azure-staging* remote's *master* branch.</span></span>

    ```console
    git push azure-staging master
    ```

    <span data-ttu-id="c8dac-224">Подождите, пока Azure создает и развертывает приложение.</span><span class="sxs-lookup"><span data-stu-id="c8dac-224">Wait while Azure builds and deploys the app.</span></span>

6. <span data-ttu-id="c8dac-225">Чтобы убедиться, что V3 был развернут в промежуточном слоте, откройте два окна браузера.</span><span class="sxs-lookup"><span data-stu-id="c8dac-225">To verify that V3 has been deployed to the staging slot, open two browser windows.</span></span> <span data-ttu-id="c8dac-226">В одном окне перейдите к URL исходного веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="c8dac-226">In one window, navigate to the original web app URL.</span></span> <span data-ttu-id="c8dac-227">В другом окне перейдите к URL промежуточного веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="c8dac-227">In the other window, navigate to the staging web app URL.</span></span> <span data-ttu-id="c8dac-228">URL-адрес рабочей служит V2 приложения.</span><span class="sxs-lookup"><span data-stu-id="c8dac-228">The production URL serves V2 of the app.</span></span> <span data-ttu-id="c8dac-229">URL-адрес промежуточной служит V3 приложения.</span><span class="sxs-lookup"><span data-stu-id="c8dac-229">The staging URL serves V3 of the app.</span></span>

    ![Снимок экрана: сравнение окно браузера](./media/deploying-to-app-service/ready-to-swap.png)

7. <span data-ttu-id="c8dac-231">В Cloud Shell переключите промежуточный слот проверен и последующее вверх в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="c8dac-231">In the Cloud Shell, swap the verified/warmed-up staging slot into production.</span></span>

    ```azure-cli
    az webapp deployment slot swap --name $webappname --resource-group AzureTutorial --slot staging
    ```

8. <span data-ttu-id="c8dac-232">Проверьте, внесения переключения, обновив окно два браузера.</span><span class="sxs-lookup"><span data-stu-id="c8dac-232">Verify that the swap occurred by refreshing the two browser windows.</span></span>

    ![Сравнение окно браузера после замены](./media/deploying-to-app-service/swapped.png)

## <a name="summary"></a><span data-ttu-id="c8dac-234">Сводка</span><span class="sxs-lookup"><span data-stu-id="c8dac-234">Summary</span></span>

<span data-ttu-id="c8dac-235">В этом разделе были выполнены следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="c8dac-235">In this section, the following tasks were completed:</span></span>

* <span data-ttu-id="c8dac-236">Загружаются и построение примера приложения.</span><span class="sxs-lookup"><span data-stu-id="c8dac-236">Downloaded and built the sample app.</span></span>
* <span data-ttu-id="c8dac-237">Создать приложение службы веб-приложение Azure с помощью Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="c8dac-237">Created an Azure App Service Web App using the Azure Cloud Shell.</span></span>
* <span data-ttu-id="c8dac-238">Развернуть пример приложения в Azure с помощью Git.</span><span class="sxs-lookup"><span data-stu-id="c8dac-238">Deployed the sample app to Azure using Git.</span></span>
* <span data-ttu-id="c8dac-239">Развернуто изменение приложения с помощью Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c8dac-239">Deployed a change to the app using Visual Studio.</span></span>
* <span data-ttu-id="c8dac-240">Добавить промежуточный слот для веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="c8dac-240">Added a staging slot to the web app.</span></span>
* <span data-ttu-id="c8dac-241">Развернуть обновления в промежуточном слоте.</span><span class="sxs-lookup"><span data-stu-id="c8dac-241">Deployed an update to the staging slot.</span></span>
* <span data-ttu-id="c8dac-242">Поменять местами промежуточными и рабочими слотами.</span><span class="sxs-lookup"><span data-stu-id="c8dac-242">Swapped the staging and production slots.</span></span>

<span data-ttu-id="c8dac-243">В следующем разделе вы узнаете, как создать конвейер DevOps с конвейерами Azure.</span><span class="sxs-lookup"><span data-stu-id="c8dac-243">In the next section, you'll learn how to build a DevOps pipeline with Azure Pipelines.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="c8dac-244">Дополнительные материалы для чтения</span><span class="sxs-lookup"><span data-stu-id="c8dac-244">Additional reading</span></span>

* [<span data-ttu-id="c8dac-245">Обзор веб-приложений</span><span class="sxs-lookup"><span data-stu-id="c8dac-245">Web Apps overview</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="c8dac-246">Создание веб-приложения .NET Core с базой данных SQL в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="c8dac-246">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [<span data-ttu-id="c8dac-247">Настройка учетных данных развертывания для службы приложений Azure</span><span class="sxs-lookup"><span data-stu-id="c8dac-247">Configure deployment credentials for Azure App Service</span></span>](/azure/app-service/app-service-deployment-credentials)
* [<span data-ttu-id="c8dac-248">Настройка промежуточных сред в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="c8dac-248">Set up staging environments in Azure App Service</span></span>](/azure/app-service/web-sites-staged-publishing)
