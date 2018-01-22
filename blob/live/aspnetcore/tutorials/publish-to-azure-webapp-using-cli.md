---
title: "Публикация приложения ASP.NET Core в Azure с использованием программ командной строки | Документация Майкрософт"
description: "Сведения о публикации приложения ASP.NET Core в службе приложений Azure с помощью клиента командной строки Git."
services: multiple
author: camsoper
ms.author: casoper
manager: wpickett
ms.date: 11/03/2017
ms.topic: get-started-article
ms.prod: asp.net-core
ms.technology: aspnet
ms.custom: mvc
ms.devlang: dotnet
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 4797260f95443954e86aae1614140c0caa5ca8bd
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="deploy-an-aspnet-core-application-to-azure-app-service-from-the-command-line"></a><span data-ttu-id="f00af-103">Развертывание приложения ASP.NET Core в службе приложений Azure из командной строки</span><span class="sxs-lookup"><span data-stu-id="f00af-103">Deploy an ASP.NET Core application to Azure App Service from the command line</span></span>

<span data-ttu-id="f00af-104">Автор [Кэм Сопер (Cam Soper)](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="f00af-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

<span data-ttu-id="f00af-105">В этом руководстве рассказывается, как создавать и развертывать приложение ASP.NET Core в службе приложений Microsoft Azure с помощью программ командной строки.</span><span class="sxs-lookup"><span data-stu-id="f00af-105">This tutorial will show you how to build and deploy an ASP.NET Core application to Microsoft Azure App Service using command line tools.</span></span>  <span data-ttu-id="f00af-106">После завершения у вас будет веб-приложение, созданное в ASP.NET MVC Core, размещенное в качестве веб-приложения службы приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="f00af-106">When finished, you'll have a web application built in ASP.NET MVC Core hosted as an Azure App Service Web App.</span></span>  <span data-ttu-id="f00af-107">Это руководство написано с использованием программ командной строки Windows, но оно также может применяться в средах MacOS и Linux.</span><span class="sxs-lookup"><span data-stu-id="f00af-107">This tutorial is written using Windows command line tools, but can be applied to macOS and Linux environments, as well.</span></span>  

<span data-ttu-id="f00af-108">В этом руководстве вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="f00af-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f00af-109">создать веб-сайт службы приложений Azure с помощью Azure CLI;</span><span class="sxs-lookup"><span data-stu-id="f00af-109">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="f00af-110">развернуть приложение ASP.NET Core в службе приложений Azure, используя программу командной строки Git.</span><span class="sxs-lookup"><span data-stu-id="f00af-110">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f00af-111">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="f00af-111">Prerequisites</span></span>

<span data-ttu-id="f00af-112">Для работы с этим руководством вам понадобится:</span><span class="sxs-lookup"><span data-stu-id="f00af-112">To complete this tutorial, you'll need:</span></span>

* <span data-ttu-id="f00af-113">[Подписка на Microsoft Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="f00af-113">A [Microsoft Azure subscription](https://azure.microsoft.com/free/)</span></span>
* [<span data-ttu-id="f00af-114">.NET Core</span><span class="sxs-lookup"><span data-stu-id="f00af-114">.NET Core</span></span>](https://www.microsoft.com/net/download/core)
* <span data-ttu-id="f00af-115">Клиент командной строки [Git](https://www.git-scm.com/).</span><span class="sxs-lookup"><span data-stu-id="f00af-115">[Git](https://www.git-scm.com/) command line client</span></span>

## <a name="create-a-web-application"></a><span data-ttu-id="f00af-116">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="f00af-116">Create a web application</span></span>

<span data-ttu-id="f00af-117">Создайте каталог для веб-приложения, создайте приложение ASP.NET Core MVC, а затем запустите веб-сайт локально.</span><span class="sxs-lookup"><span data-stu-id="f00af-117">Create a new directory for the web application, create a new ASP.NET Core MVC application, and then run the website locally.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="f00af-118">Windows</span><span class="sxs-lookup"><span data-stu-id="f00af-118">Windows</span></span>](#tab/windows)
```cmd
REM Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the application
dotnet run
```

# <a name="othertabother"></a>[<span data-ttu-id="f00af-119">Другое</span><span class="sxs-lookup"><span data-stu-id="f00af-119">Other</span></span>](#tab/other)
```bash
# Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the application
dotnet run
```
---

![Данные, выводимые в командной строке](publish-to-azure-webapp-using-cli/_static/new_prj.png)

<span data-ttu-id="f00af-121">Проверьте приложение, перейдя по адресу http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="f00af-121">Test the application by browsing to http://localhost:5000.</span></span>

![Веб-сайт, работающий локально](publish-to-azure-webapp-using-cli/_static/app_test.png)


## <a name="create-the-azure-app-service-instance"></a><span data-ttu-id="f00af-123">Создание экземпляра службы приложений Azure</span><span class="sxs-lookup"><span data-stu-id="f00af-123">Create the Azure App Service instance</span></span>

<span data-ttu-id="f00af-124">Используя [Azure Cloud Shell](/azure/cloud-shell/quickstart), создайте группу ресурсов, план и веб-приложение службы приложений.</span><span class="sxs-lookup"><span data-stu-id="f00af-124">Using the [Azure Cloud Shell](/azure/cloud-shell/quickstart), create a resource group, App Service plan, and an App Service web app.</span></span>

```azurecli-interactive
# Generate a unique Web App name
let randomNum=$RANDOM*$RANDOM
webappname=tutorialApp$randomNum

# Create the DotNetAzureTutorial resource group
az group create --name DotNetAzureTutorial --location EastUS

# Create an App Service plan.
az appservice plan create --name $webappname --resource-group DotNetAzureTutorial --sku FREE

# Create the Web App
az webapp create --name $webappname --resource-group DotNetAzureTutorial --plan $webappname
```

<span data-ttu-id="f00af-125">Перед развертыванием настройте учетные данные для развертывания на уровне учетной записи, используя следующую команду:</span><span class="sxs-lookup"><span data-stu-id="f00af-125">Before deployment, set the account-level deployment credentials using the following command:</span></span>

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

<span data-ttu-id="f00af-126">URL-адрес развертывания необходим для развертывания приложения с использованием Git.</span><span class="sxs-lookup"><span data-stu-id="f00af-126">A deployment URL is needed to deploy the application using Git.</span></span>  <span data-ttu-id="f00af-127">Получите URL-адрес следующим образом.</span><span class="sxs-lookup"><span data-stu-id="f00af-127">Retrieve the URL like this.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```
<span data-ttu-id="f00af-128">Запишите отображаемый URL-адрес, заканчивающийся на `.git`.</span><span class="sxs-lookup"><span data-stu-id="f00af-128">Note the displayed URL ending in `.git`.</span></span> <span data-ttu-id="f00af-129">Он используется на следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="f00af-129">It's used in the next step.</span></span>

## <a name="deploy-the-application-using-git"></a><span data-ttu-id="f00af-130">Развертывание приложения с помощью Git</span><span class="sxs-lookup"><span data-stu-id="f00af-130">Deploy the application using Git</span></span>

<span data-ttu-id="f00af-131">Все готово к развертыванию с локального компьютера с помощью Git.</span><span class="sxs-lookup"><span data-stu-id="f00af-131">You're ready to deploy from your local machine using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="f00af-132">Любые предупреждения из Git об окончаниях строк можно игнорировать, не опасаясь.</span><span class="sxs-lookup"><span data-stu-id="f00af-132">It's safe to ignore any warnings from Git about line endings.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="f00af-133">Windows</span><span class="sxs-lookup"><span data-stu-id="f00af-133">Windows</span></span>](#tab/windows)
```cmd
REM Initialize the local Git repository
git init

REM Add the contents of the working directory to the repo
git add --all

REM Commit the changes to the local repo
git commit -a -m "Initial commit"

REM Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

REM Push the local repository to the remote
git push azure master
```

# <a name="othertabother"></a>[<span data-ttu-id="f00af-134">Другое</span><span class="sxs-lookup"><span data-stu-id="f00af-134">Other</span></span>](#tab/other)
```bash
# Initialize the local Git repository
git init

# Add the contents of the working directory to the repo
git add --all

# Commit the changes to the local repo
git commit -a -m "Initial commit"

# Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

# Push the local repository to the remote
git push azure master
```
---

<span data-ttu-id="f00af-135">Git предложит ввести учетные данные развертывания, заданные ранее.</span><span class="sxs-lookup"><span data-stu-id="f00af-135">Git will prompt for the deployment credentials that were set earlier.</span></span>  <span data-ttu-id="f00af-136">После проверки подлинности приложение будет перенесено в удаленное местоположение, создано и развернуто.</span><span class="sxs-lookup"><span data-stu-id="f00af-136">After authenticating, the application will be pushed to the remote location, built, and deployed.</span></span>

![Выходные данные развертывания Git](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-application"></a><span data-ttu-id="f00af-138">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="f00af-138">Test the application</span></span>

<span data-ttu-id="f00af-139">Проверьте приложение, перейдя по адресу `https://<web app name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="f00af-139">Test the application by browsing to `https://<web app name>.azurewebsites.net`.</span></span>  <span data-ttu-id="f00af-140">Для отображения адреса в Cloud Shell (или Azure CLI) используйте следующую команду:</span><span class="sxs-lookup"><span data-stu-id="f00af-140">To display the address in the Cloud Shell (or Azure CLI), use the following:</span></span>

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![Приложение, работающее в Azure](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a><span data-ttu-id="f00af-142">Очистка</span><span class="sxs-lookup"><span data-stu-id="f00af-142">Clean up</span></span>

<span data-ttu-id="f00af-143">После проверки приложения, кода и ресурсов удалите веб-приложение и план, удалив группу ресурсов.</span><span class="sxs-lookup"><span data-stu-id="f00af-143">When finished testing the app and inspecting the code and resources, delete the web app and plan by deleting the resource group.</span></span>

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a><span data-ttu-id="f00af-144">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="f00af-144">Next steps</span></span>

<span data-ttu-id="f00af-145">В этом руководстве вы узнали, как:</span><span class="sxs-lookup"><span data-stu-id="f00af-145">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f00af-146">создать веб-сайт службы приложений Azure с помощью Azure CLI;</span><span class="sxs-lookup"><span data-stu-id="f00af-146">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="f00af-147">развернуть приложение ASP.NET Core в службе приложений Azure, используя программу командной строки Git.</span><span class="sxs-lookup"><span data-stu-id="f00af-147">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

<span data-ttu-id="f00af-148">Теперь научитесь развертывать имеющееся веб-приложение, использующее Cosmos DB, с помощью командной строки.</span><span class="sxs-lookup"><span data-stu-id="f00af-148">Next, learn to use the command line to deploy an existing web app that uses CosmosDB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="f00af-149">Развертывание приложения в Azure из командной строки с помощью .NET Core</span><span class="sxs-lookup"><span data-stu-id="f00af-149">Deploy to Azure from the command line with .NET Core</span></span>](/dotnet/azure/dotnet-quickstart-xplat)
