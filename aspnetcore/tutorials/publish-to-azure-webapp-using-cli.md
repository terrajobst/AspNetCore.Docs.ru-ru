---
title: Публикация приложения ASP.NET Core в Azure с использованием средств командной строки
author: camsoper
description: Сведения о публикации приложения ASP.NET Core в службе приложений Azure с помощью клиента командной строки Git.
ms.author: casoper
ms.custom: mvc
ms.date: 11/03/2017
services: multiple
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 526ceef469d473706f39cdc3ee645280e99315b1
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2018
ms.locfileid: "38126964"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-command-line-tools"></a><span data-ttu-id="c1c0a-103">Публикация приложения ASP.NET Core в Azure с использованием средств командной строки</span><span class="sxs-lookup"><span data-stu-id="c1c0a-103">Publish an ASP.NET Core app to Azure with command line tools</span></span>

<span data-ttu-id="c1c0a-104">Автор [Кэм Сопер (Cam Soper)](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="c1c0a-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="c1c0a-105">В этом руководстве рассказывается, как создавать и развертывать приложение ASP.NET Core в службе приложений Microsoft Azure с помощью программ командной строки.</span><span class="sxs-lookup"><span data-stu-id="c1c0a-105">This tutorial will show you how to build and deploy an ASP.NET Core app to Microsoft Azure App Service using command line tools.</span></span> <span data-ttu-id="c1c0a-106">После завершения у вас будет веб-приложение Razor Pages, созданное в ASP.NET Core и размещенное в качестве веб-приложения службы приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="c1c0a-106">When finished, you'll have a Razor Pages web app built in ASP.NET Core hosted as an Azure App Service Web App.</span></span> <span data-ttu-id="c1c0a-107">Это руководство написано с использованием программ командной строки Windows, но оно также может применяться в средах MacOS и Linux.</span><span class="sxs-lookup"><span data-stu-id="c1c0a-107">This tutorial is written using Windows command line tools, but can be applied to macOS and Linux environments, as well.</span></span>

<span data-ttu-id="c1c0a-108">В этом руководстве вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="c1c0a-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c1c0a-109">создать веб-сайт службы приложений Azure с помощью Azure CLI;</span><span class="sxs-lookup"><span data-stu-id="c1c0a-109">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="c1c0a-110">развернуть приложение ASP.NET Core в службе приложений Azure, используя программу командной строки Git.</span><span class="sxs-lookup"><span data-stu-id="c1c0a-110">Deploy an ASP.NET Core app to Azure App Service using the Git command line tool</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c1c0a-111">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="c1c0a-111">Prerequisites</span></span>

<span data-ttu-id="c1c0a-112">Для работы с этим руководством вам понадобится:</span><span class="sxs-lookup"><span data-stu-id="c1c0a-112">To complete this tutorial, you'll need:</span></span>

* <span data-ttu-id="c1c0a-113">[Подписка на Microsoft Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="c1c0a-113">A [Microsoft Azure subscription](https://azure.microsoft.com/free/)</span></span>
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="c1c0a-114">Клиент командной строки [Git](https://www.git-scm.com/).</span><span class="sxs-lookup"><span data-stu-id="c1c0a-114">[Git](https://www.git-scm.com/) command line client</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="c1c0a-115">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="c1c0a-115">Create a web app</span></span>

<span data-ttu-id="c1c0a-116">Создайте каталог для веб-приложения, приложение ASP.NET Core Razor Pages, а затем запустите веб-сайт локально.</span><span class="sxs-lookup"><span data-stu-id="c1c0a-116">Create a new directory for the web app, create a new ASP.NET Core Razor Pages app, and then run the website locally.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="c1c0a-117">Windows</span><span class="sxs-lookup"><span data-stu-id="c1c0a-117">Windows</span></span>](#tab/windows)

::: moniker range=">= aspnetcore-2.1"

```console
REM Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the app
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
REM Create a new ASP.NET Core Razor Pages app
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the app
dotnet run
```

::: moniker-end

# <a name="othertabother"></a>[<span data-ttu-id="c1c0a-118">Другое</span><span class="sxs-lookup"><span data-stu-id="c1c0a-118">Other</span></span>](#tab/other)

::: moniker range=">= aspnetcore-2.1"

```bash
# Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the app
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```bash
# Create a new ASP.NET Core Razor Pages app
dotnet new razor -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the app
dotnet run
```

::: moniker-end

---

![Данные, выводимые в командной строке](publish-to-azure-webapp-using-cli/_static/new_prj.png)

<span data-ttu-id="c1c0a-120">Протестируйте приложение, перейдя по адресу `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="c1c0a-120">Test the app by browsing to `http://localhost:5000`.</span></span>

![Веб-сайт, работающий локально](publish-to-azure-webapp-using-cli/_static/app_test.png)

## <a name="create-the-azure-app-service-instance"></a><span data-ttu-id="c1c0a-122">Создание экземпляра службы приложений Azure</span><span class="sxs-lookup"><span data-stu-id="c1c0a-122">Create the Azure App Service instance</span></span>

<span data-ttu-id="c1c0a-123">Используя [Azure Cloud Shell](/azure/cloud-shell/quickstart), создайте группу ресурсов, план и веб-приложение службы приложений.</span><span class="sxs-lookup"><span data-stu-id="c1c0a-123">Using the [Azure Cloud Shell](/azure/cloud-shell/quickstart), create a resource group, App Service plan, and an App Service web app.</span></span>

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

<span data-ttu-id="c1c0a-124">Перед развертыванием настройте учетные данные для развертывания на уровне учетной записи, используя следующую команду:</span><span class="sxs-lookup"><span data-stu-id="c1c0a-124">Before deployment, set the account-level deployment credentials using the following command:</span></span>

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

<span data-ttu-id="c1c0a-125">URL-адрес развертывания необходим для развертывания приложения с использованием Git.</span><span class="sxs-lookup"><span data-stu-id="c1c0a-125">A deployment URL is needed to deploy the app using Git.</span></span> <span data-ttu-id="c1c0a-126">Получите URL-адрес следующим образом.</span><span class="sxs-lookup"><span data-stu-id="c1c0a-126">Retrieve the URL like this.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```

<span data-ttu-id="c1c0a-127">Запишите отображаемый URL-адрес, заканчивающийся на `.git`.</span><span class="sxs-lookup"><span data-stu-id="c1c0a-127">Note the displayed URL ending in `.git`.</span></span> <span data-ttu-id="c1c0a-128">Он используется на следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="c1c0a-128">It's used in the next step.</span></span>

## <a name="deploy-the-app-using-git"></a><span data-ttu-id="c1c0a-129">Развертывание приложения с помощью Git</span><span class="sxs-lookup"><span data-stu-id="c1c0a-129">Deploy the app using Git</span></span>

<span data-ttu-id="c1c0a-130">Все готово к развертыванию с локального компьютера с помощью Git.</span><span class="sxs-lookup"><span data-stu-id="c1c0a-130">You're ready to deploy from your local machine using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="c1c0a-131">Любые предупреждения из Git об окончаниях строк можно игнорировать, не опасаясь.</span><span class="sxs-lookup"><span data-stu-id="c1c0a-131">It's safe to ignore any warnings from Git about line endings.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="c1c0a-132">Windows</span><span class="sxs-lookup"><span data-stu-id="c1c0a-132">Windows</span></span>](#tab/windows)

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

# <a name="othertabother"></a>[<span data-ttu-id="c1c0a-133">Другое</span><span class="sxs-lookup"><span data-stu-id="c1c0a-133">Other</span></span>](#tab/other)

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

<span data-ttu-id="c1c0a-134">Git предлагает ввести учетные данные развертывания, заданные ранее.</span><span class="sxs-lookup"><span data-stu-id="c1c0a-134">Git prompts for the deployment credentials that were set earlier.</span></span> <span data-ttu-id="c1c0a-135">После проверки подлинности приложение будет перенесено в удаленное местоположение, скомпилировано и развернуто.</span><span class="sxs-lookup"><span data-stu-id="c1c0a-135">After authenticating, the app will be pushed to the remote location, built, and deployed.</span></span>

![Выходные данные развертывания Git](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-app"></a><span data-ttu-id="c1c0a-137">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="c1c0a-137">Test the app</span></span>

<span data-ttu-id="c1c0a-138">Протестируйте приложение, перейдя по адресу `https://<web app name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="c1c0a-138">Test the app by browsing to `https://<web app name>.azurewebsites.net`.</span></span> <span data-ttu-id="c1c0a-139">Для отображения адреса в Cloud Shell (или Azure CLI) используйте следующую команду:</span><span class="sxs-lookup"><span data-stu-id="c1c0a-139">To display the address in the Cloud Shell (or Azure CLI), use the following:</span></span>

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![Приложение, работающее в Azure](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a><span data-ttu-id="c1c0a-141">Очистка</span><span class="sxs-lookup"><span data-stu-id="c1c0a-141">Clean up</span></span>

<span data-ttu-id="c1c0a-142">После проверки приложения, кода и ресурсов удалите веб-приложение и план, удалив группу ресурсов.</span><span class="sxs-lookup"><span data-stu-id="c1c0a-142">When finished testing the app and inspecting the code and resources, delete the web app and plan by deleting the resource group.</span></span>

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a><span data-ttu-id="c1c0a-143">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="c1c0a-143">Next steps</span></span>

<span data-ttu-id="c1c0a-144">В этом руководстве вы узнали, как:</span><span class="sxs-lookup"><span data-stu-id="c1c0a-144">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c1c0a-145">создать веб-сайт службы приложений Azure с помощью Azure CLI;</span><span class="sxs-lookup"><span data-stu-id="c1c0a-145">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="c1c0a-146">развернуть приложение ASP.NET Core в службе приложений Azure, используя программу командной строки Git.</span><span class="sxs-lookup"><span data-stu-id="c1c0a-146">Deploy an ASP.NET Core app to Azure App Service using the Git command line tool</span></span>

<span data-ttu-id="c1c0a-147">Теперь научитесь развертывать имеющееся веб-приложение, использующее Cosmos DB, с помощью командной строки.</span><span class="sxs-lookup"><span data-stu-id="c1c0a-147">Next, learn to use the command line to deploy an existing web app that uses CosmosDB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c1c0a-148">Развертывание приложения в Azure из командной строки с помощью .NET Core</span><span class="sxs-lookup"><span data-stu-id="c1c0a-148">Deploy to Azure from the command line with .NET Core</span></span>](/dotnet/azure/dotnet-quickstart-xplat)
