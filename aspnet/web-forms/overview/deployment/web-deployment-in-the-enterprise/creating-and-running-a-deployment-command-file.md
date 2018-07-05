---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Файл команд, создание и запуск развертывания | Документация Майкрософт
author: jrjlee
description: В этом разделе описывается создание командный файл, который позволит выполнить развертывание с помощью файлов проекта Microsoft Build Engine (MSBuild) как один шаг, повторно...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: fc59920feb5eb48bc8150606b58a1ed4ba60ee92
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395377"
---
<a name="creating-and-running-a-deployment-command-file"></a><span data-ttu-id="48760-103">Создание и запуск командного файла развертывания</span><span class="sxs-lookup"><span data-stu-id="48760-103">Creating and Running a Deployment Command File</span></span>
====================
<span data-ttu-id="48760-104">по [Джейсон Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="48760-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="48760-105">Загрузить PDF-файл</span><span class="sxs-lookup"><span data-stu-id="48760-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="48760-106">В этом разделе описывается создание командный файл, который позволит выполнить развертывание с помощью файлов проекта Microsoft Build Engine (MSBuild) как один шаг, повторяемый процесс.</span><span class="sxs-lookup"><span data-stu-id="48760-106">This topic describes how to build a command file that will let you run a deployment using Microsoft Build Engine (MSBuild) project files as a single-step, repeatable process.</span></span>


<span data-ttu-id="48760-107">Этот раздел является частью серии учебников, исходя из требования к развертыванию enterprise вымышленной компании Fabrikam, Inc. В этой серии руководств используется пример решения&#x2014; [Contact Manager](the-contact-manager-solution.md) решение&#x2014;для представления веб-приложения с более реалистичные уровень сложности, включая приложения ASP.NET MVC 3, Windows Communication Служба Foundation (WCF) и проект базы данных.</span><span class="sxs-lookup"><span data-stu-id="48760-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager](the-contact-manager-solution.md) solution&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="48760-108">Метод развертывания в основе этих учебников основан на разбиение проекта файл подход, описанный в [основные сведения о процессе построения](understanding-the-build-process.md), в которой процесс построения управляется двумя файлы проекта&#x2014;один с Создание инструкции, которые применяются для каждой целевой среде и содержащего параметры построения и развертывания для конкретной среды.</span><span class="sxs-lookup"><span data-stu-id="48760-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Build Process](understanding-the-build-process.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="48760-109">Во время сборки файл проекта для конкретной среды объединяется в файл проекта независимой от среды, чтобы полный набор инструкций построения.</span><span class="sxs-lookup"><span data-stu-id="48760-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="process-overview"></a><span data-ttu-id="48760-110">Общие сведения о процессе</span><span class="sxs-lookup"><span data-stu-id="48760-110">Process Overview</span></span>

<span data-ttu-id="48760-111">В этом разделе вы узнаете, как создать и запустить командный файл, который использует эти файлы проекта для выполнения Повторяемое развертывание целевую среду.</span><span class="sxs-lookup"><span data-stu-id="48760-111">In this topic, you'll learn how to create and run a command file that uses these project files to perform a repeatable deployment to your target environment.</span></span> <span data-ttu-id="48760-112">По сути, командный файл просто должен содержать команды MSBuild:</span><span class="sxs-lookup"><span data-stu-id="48760-112">Essentially, the command file simply needs to contain an MSBuild command that:</span></span>

- <span data-ttu-id="48760-113">Сообщает MSBuild о необходимости выполнения независимой от среды *Publish.proj* файла.</span><span class="sxs-lookup"><span data-stu-id="48760-113">Tells MSBuild to execute the environment-agnostic *Publish.proj* file.</span></span>
- <span data-ttu-id="48760-114">Сообщает *Publish.proj* файл, файл, который содержит параметры проекта для конкретной среды и где ее найти.</span><span class="sxs-lookup"><span data-stu-id="48760-114">Tells the *Publish.proj* file which file contains the environment-specific project settings and where to find it.</span></span>

## <a name="create-an-msbuild-command"></a><span data-ttu-id="48760-115">Создание команды MSBuild</span><span class="sxs-lookup"><span data-stu-id="48760-115">Create an MSBuild Command</span></span>

<span data-ttu-id="48760-116">Как описано в разделе [основные сведения о процессе построения](understanding-the-build-process.md), файле проекта для конкретной среды&#x2014;к примеру, *Env Dev.proj*&#x2014;предназначена для импорта в независимой от среды *Publish.proj* файл во время сборки.</span><span class="sxs-lookup"><span data-stu-id="48760-116">As described in [Understanding the Build Process](understanding-the-build-process.md), the environment-specific project file&#x2014;for example, *Env-Dev.proj*&#x2014;is designed to be imported into the environment-agnostic *Publish.proj* file at build time.</span></span> <span data-ttu-id="48760-117">Вместе эти два файла предоставляют полный набор инструкций, которые задают MSBuild для построения и развертывания решения.</span><span class="sxs-lookup"><span data-stu-id="48760-117">Together, these two files provide a complete set of instructions that tell MSBuild how to build and deploy your solution.</span></span>

<span data-ttu-id="48760-118">*Publish.proj* файле используется **импорта** элемент для импорта файла проекта для конкретной среды.</span><span class="sxs-lookup"><span data-stu-id="48760-118">The *Publish.proj* file uses an **Import** element to import the environment-specific project file.</span></span>


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


<span data-ttu-id="48760-119">Таким образом при использовании MSBuild.exe для создания и развертывания решения диспетчера контактов, вам потребуется:</span><span class="sxs-lookup"><span data-stu-id="48760-119">As such, when you use MSBuild.exe to build and deploy the Contact Manager solution, you need to:</span></span>

- <span data-ttu-id="48760-120">Проведение MSBuild.exe *Publish.proj* файла.</span><span class="sxs-lookup"><span data-stu-id="48760-120">Run MSBuild.exe on the *Publish.proj* file.</span></span>
- <span data-ttu-id="48760-121">Укажите расположение файла проекта для конкретной среды, указав параметр командной строки с именем **TargetEnvPropsFile**.</span><span class="sxs-lookup"><span data-stu-id="48760-121">Specify the location of the environment-specific project file by supplying a command-line parameter named **TargetEnvPropsFile**.</span></span>

<span data-ttu-id="48760-122">Для этого команду MSBuild должен выглядеть примерно следующим образом:</span><span class="sxs-lookup"><span data-stu-id="48760-122">To do this, your MSBuild command should resemble this:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


<span data-ttu-id="48760-123">Здесь это простое действие, чтобы переместить повторяемых и один шаг развертывания.</span><span class="sxs-lookup"><span data-stu-id="48760-123">From here, it's a simple step to move to a repeatable, single-step deployment.</span></span> <span data-ttu-id="48760-124">Все, что нужно сделать — это добавить команду MSBuild CMD-файл.</span><span class="sxs-lookup"><span data-stu-id="48760-124">All you need to do is to add your MSBuild command to a .cmd file.</span></span> <span data-ttu-id="48760-125">В решение диспетчера контактов, в папку публикации включает в себя файл с именем *Dev.cmd публикации* , именно это и делает.</span><span class="sxs-lookup"><span data-stu-id="48760-125">In the Contact Manager solution, the Publish folder includes a file named *Publish-Dev.cmd* that does exactly this.</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="48760-126">**/Fl** коммутатора MSBuild использует для создания файла журнала с именем *msbuild.log* в рабочем каталоге, в котором был вызван MSBuild.exe.</span><span class="sxs-lookup"><span data-stu-id="48760-126">The **/fl** switch instructs MSBuild to create a log file named *msbuild.log* in the working directory in which MSBuild.exe was invoked.</span></span>


<span data-ttu-id="48760-127">Для развертывания или повторного развертывания решения диспетчера контактов, все что нужно сделать выполнить *Dev.cmd публикации* файл.</span><span class="sxs-lookup"><span data-stu-id="48760-127">To deploy or redeploy the Contact Manager solution, all you need to do is run the *Publish-Dev.cmd* file.</span></span> <span data-ttu-id="48760-128">При запуске файла будет MSBuild:</span><span class="sxs-lookup"><span data-stu-id="48760-128">When you run the file, MSBuild will:</span></span>

- <span data-ttu-id="48760-129">Построение всех проектов в решении.</span><span class="sxs-lookup"><span data-stu-id="48760-129">Build all the projects in the solution.</span></span>
- <span data-ttu-id="48760-130">Создание развертывания веб-пакетов для проектов веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="48760-130">Generate deployable web packages for the web application projects.</span></span>
- <span data-ttu-id="48760-131">Создание расширением DBSCHEMA и .deploymanifest файлов для проектов баз данных.</span><span class="sxs-lookup"><span data-stu-id="48760-131">Generate .dbschema and .deploymanifest files for the database projects.</span></span>
- <span data-ttu-id="48760-132">Развертывание веб-пакеты на веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="48760-132">Deploy the web packages to the web server.</span></span>
- <span data-ttu-id="48760-133">Развертывание базы данных на сервер базы данных.</span><span class="sxs-lookup"><span data-stu-id="48760-133">Deploy the database to the database server.</span></span>

## <a name="run-the-deployment"></a><span data-ttu-id="48760-134">Выполните развертывание</span><span class="sxs-lookup"><span data-stu-id="48760-134">Run the Deployment</span></span>

<span data-ttu-id="48760-135">При создании командного файла для целевой среды можно выполнить всего развертывания, просто запустив файл.</span><span class="sxs-lookup"><span data-stu-id="48760-135">When you've created a command file for your target environment, you should be able to complete the entire deployment by simply running the file.</span></span>

<span data-ttu-id="48760-136">**Чтобы развернуть решение диспетчера контактов для тестовой среды**</span><span class="sxs-lookup"><span data-stu-id="48760-136">**To deploy the Contact Manager solution to your test environment**</span></span>

1. <span data-ttu-id="48760-137">На рабочей станции разработчика, откройте проводник Windows и перейдите к расположению *Dev.cmd публикации* файл.</span><span class="sxs-lookup"><span data-stu-id="48760-137">On your developer workstation, open Windows Explorer, and then browse to the location of the *Publish-Dev.cmd* file.</span></span>
2. <span data-ttu-id="48760-138">Дважды щелкните файл, чтобы запустить его.</span><span class="sxs-lookup"><span data-stu-id="48760-138">Double-click the file to run it.</span></span>
3. <span data-ttu-id="48760-139">Если **открыть файл — предупреждение системы безопасности** появится диалоговое окно, нажмите кнопку **запуска**.</span><span class="sxs-lookup"><span data-stu-id="48760-139">If an **Open File – Security Warning** dialog box appears, click **Run**.</span></span>
4. <span data-ttu-id="48760-140">Если параметры конфигурации и тестовые серверы настроены правильно, окно командной строки будет отображаться **построение выполнено успешно** сообщение после завершения обработки файлов проекта MSBuild.</span><span class="sxs-lookup"><span data-stu-id="48760-140">If your configuration settings and test servers are set up correctly, the Command Prompt window will show a **Build succeeded** message when MSBuild has finished processing the project files.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. <span data-ttu-id="48760-141">Если это первый раз, вы развернули решение в этой среде, необходимо добавить тест web server учетную запись компьютера для **db\_datawriter** и **db\_datareader**роли на **ContactManager** базы данных.</span><span class="sxs-lookup"><span data-stu-id="48760-141">If this is the first time you've deployed the solution to this environment, you'll need to add the test web server machine account to the **db\_datawriter** and **db\_datareader** roles on the **ContactManager** database.</span></span> <span data-ttu-id="48760-142">Эта процедура описана в [Настройка сервера базы данных для развертывания веб-публикаций](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="48760-142">This procedure is described in [Configure a Database Server for Web Deploy Publishing](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="48760-143">Необходимо назначить эти разрешения, при создании базы данных.</span><span class="sxs-lookup"><span data-stu-id="48760-143">You only need to assign these permissions when you create the database.</span></span> <span data-ttu-id="48760-144">По умолчанию, процесс построения не будет воссоздать базу данных при каждом развертывании&#x2014;вместо этого будет сравнить существующую базу данных и самой последней схемы и внесите только необходимые изменения.</span><span class="sxs-lookup"><span data-stu-id="48760-144">By default, the build process will not recreate the database on every deployment&#x2014;instead, it will compare the existing database to the latest schema and make only the changes required.</span></span> <span data-ttu-id="48760-145">Таким образом следует только необходимо сопоставить указанные роли базы данных при первом развертывании решения.</span><span class="sxs-lookup"><span data-stu-id="48760-145">As a result, you should only need to map these database roles the first time you deploy the solution.</span></span>
6. <span data-ttu-id="48760-146">Откройте Internet Explorer и перейдите к URL-адрес приложения диспетчера контактов (например, `http://testweb1:85/ContactManager/`).</span><span class="sxs-lookup"><span data-stu-id="48760-146">Open Internet Explorer and browse to the URL of the Contact Manager application (for example, `http://testweb1:85/ContactManager/`).</span></span>
7. <span data-ttu-id="48760-147">Убедитесь, что приложение работает правильно, и вы можете добавить контакты.</span><span class="sxs-lookup"><span data-stu-id="48760-147">Verify that the application works as expected and you're able to add contacts.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="48760-148">Заключение</span><span class="sxs-lookup"><span data-stu-id="48760-148">Conclusion</span></span>

<span data-ttu-id="48760-149">Создав командный файл, содержащий инструкции MSBuild обеспечивает быстрый и простой способ создания и развертывания решений с несколькими проектами в среде определенное место назначения.</span><span class="sxs-lookup"><span data-stu-id="48760-149">Creating a command file containing your MSBuild instructions provides you with a quick and easy way of building and deploying a multi-project solution to a specific destination environment.</span></span> <span data-ttu-id="48760-150">Если вам нужно повторно развернуть решение в нескольких средах назначения, можно создать несколько командных файлов.</span><span class="sxs-lookup"><span data-stu-id="48760-150">If you need to repeatedly deploy your solution to multiple destination environments, you can create multiple command files.</span></span> <span data-ttu-id="48760-151">В каждый командный файл команда MSBuild создаст один и тот же файл универсальный проект, но он укажет файл различных проекта для конкретной среды.</span><span class="sxs-lookup"><span data-stu-id="48760-151">In each command file, the MSBuild command will build the same universal project file, but it will specify a different environment-specific project file.</span></span> <span data-ttu-id="48760-152">Например командный файл для публикации для разработчика или тестовой среде может содержать эта команда MSBuild:</span><span class="sxs-lookup"><span data-stu-id="48760-152">For example, a command file to publish to a developer or test environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


<span data-ttu-id="48760-153">Командный файл для публикации в промежуточной среде может содержать эта команда MSBuild:</span><span class="sxs-lookup"><span data-stu-id="48760-153">A command file to publish to a staging environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> <span data-ttu-id="48760-154">Рекомендации по настройке файлы проекта для конкретной среды для серверных сред, см. в разделе [настройки свойств развертывания для целевой среды](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span><span class="sxs-lookup"><span data-stu-id="48760-154">For guidance on how to customize the environment-specific project files for your own server environments, see [Configure Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>


<span data-ttu-id="48760-155">Можно также настроить процесс сборки для каждой среды путем переопределения свойств, а также другими параметрами в команде MSBuild.</span><span class="sxs-lookup"><span data-stu-id="48760-155">You can also customize the build process for each environment by overriding properties or setting various other switches in your MSBuild command.</span></span> <span data-ttu-id="48760-156">Дополнительные сведения см. в разделе [Справочник по командной строке MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).</span><span class="sxs-lookup"><span data-stu-id="48760-156">For more information, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="48760-157">[Назад](deploying-database-projects.md)
> [Вперед](manually-installing-web-packages.md)</span><span class="sxs-lookup"><span data-stu-id="48760-157">[Previous](deploying-database-projects.md)
[Next](manually-installing-web-packages.md)</span></span>
