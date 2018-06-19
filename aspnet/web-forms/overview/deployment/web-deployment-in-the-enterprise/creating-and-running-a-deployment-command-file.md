---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Создание и запуск развертывания файл команды | Документы Microsoft
author: jrjlee
description: Описывается, как построить командный файл, который позволит вам выполнить развертывание, с помощью Microsoft Build Engine (MSBuild) файлы проектов как один шаг...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: e5fb034a67bc9f2ea549af269eae51a49acc4d98
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891182"
---
<a name="creating-and-running-a-deployment-command-file"></a><span data-ttu-id="73a51-103">Создание и запуск командный файл развертывания</span><span class="sxs-lookup"><span data-stu-id="73a51-103">Creating and Running a Deployment Command File</span></span>
====================
<span data-ttu-id="73a51-104">по [Джейсон Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="73a51-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="73a51-105">Скачать PDF</span><span class="sxs-lookup"><span data-stu-id="73a51-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="73a51-106">В этом разделе описывается создание командный файл, который позволит вам выполнения с помощью Microsoft Build Engine (MSBuild) файлы проектов как один шаг, repeatable процесс развертывания.</span><span class="sxs-lookup"><span data-stu-id="73a51-106">This topic describes how to build a command file that will let you run a deployment using Microsoft Build Engine (MSBuild) project files as a single-step, repeatable process.</span></span>


<span data-ttu-id="73a51-107">Этот раздел входит в состав серию учебников, исходя из требования к развертыванию enterprise вымышленная компания Fabrikam, Inc. Этот учебник ряд использует образец решения&#x2014; [диспетчера контактов](the-contact-manager-solution.md) решения&#x2014;для представления веб-приложения с реалистичных уровень сложности, включая приложения ASP.NET MVC 3, Windows Communication Служба Foundation (WCF) и проект базы данных.</span><span class="sxs-lookup"><span data-stu-id="73a51-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager](the-contact-manager-solution.md) solution&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="73a51-108">Метод развертывания, в основе этих учебников основан на разбиение проекта файл подход, описанный в [основные сведения о процессе построения](understanding-the-build-process.md), в которой процесс построения управляется двух файлов проекта&#x2014;один с построение инструкции, которые применяются для каждой целевой среде и, содержащий параметры построения и развертывания конкретной среды.</span><span class="sxs-lookup"><span data-stu-id="73a51-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Build Process](understanding-the-build-process.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="73a51-109">Во время построения файла проекта среды объединяется в файл проекта зависит от среды, образуют полный набор инструкций построения.</span><span class="sxs-lookup"><span data-stu-id="73a51-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="process-overview"></a><span data-ttu-id="73a51-110">Общие сведения о процессе</span><span class="sxs-lookup"><span data-stu-id="73a51-110">Process Overview</span></span>

<span data-ttu-id="73a51-111">В этом разделе вы узнаете, как создать и запустить командный файл, который использует эти файлы проекта для развертывания repeatable целевой среде.</span><span class="sxs-lookup"><span data-stu-id="73a51-111">In this topic, you'll learn how to create and run a command file that uses these project files to perform a repeatable deployment to your target environment.</span></span> <span data-ttu-id="73a51-112">По существу, командный файл просто должен содержать команду MSBuild:</span><span class="sxs-lookup"><span data-stu-id="73a51-112">Essentially, the command file simply needs to contain an MSBuild command that:</span></span>

- <span data-ttu-id="73a51-113">Сообщает MSBuild для независимой от среды выполнения *Publish.proj* файла.</span><span class="sxs-lookup"><span data-stu-id="73a51-113">Tells MSBuild to execute the environment-agnostic *Publish.proj* file.</span></span>
- <span data-ttu-id="73a51-114">Сообщает *Publish.proj* файл, файл, который содержит параметры проекта для конкретной среды и где его найти.</span><span class="sxs-lookup"><span data-stu-id="73a51-114">Tells the *Publish.proj* file which file contains the environment-specific project settings and where to find it.</span></span>

## <a name="create-an-msbuild-command"></a><span data-ttu-id="73a51-115">Создайте команду MSBuild</span><span class="sxs-lookup"><span data-stu-id="73a51-115">Create an MSBuild Command</span></span>

<span data-ttu-id="73a51-116">Как описано в [основные сведения о процессе построения](understanding-the-build-process.md), файл проекта среды&#x2014;, *Env Dev.proj*&#x2014;предназначена для импорта в независимой от среды *Publish.proj* файл во время построения.</span><span class="sxs-lookup"><span data-stu-id="73a51-116">As described in [Understanding the Build Process](understanding-the-build-process.md), the environment-specific project file&#x2014;for example, *Env-Dev.proj*&#x2014;is designed to be imported into the environment-agnostic *Publish.proj* file at build time.</span></span> <span data-ttu-id="73a51-117">Вместе эти файлы предоставляют полный набор инструкций, которые задают MSBuild для построения и развертывания решения.</span><span class="sxs-lookup"><span data-stu-id="73a51-117">Together, these two files provide a complete set of instructions that tell MSBuild how to build and deploy your solution.</span></span>

<span data-ttu-id="73a51-118">*Publish.proj* файле используется **импорта** элемента, который требуется импортировать файл проекта для конкретной среды.</span><span class="sxs-lookup"><span data-stu-id="73a51-118">The *Publish.proj* file uses an **Import** element to import the environment-specific project file.</span></span>


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


<span data-ttu-id="73a51-119">Таким образом при использовании MSBuild.exe для построения и развертывания решений диспетчера контактов, вы должны:</span><span class="sxs-lookup"><span data-stu-id="73a51-119">As such, when you use MSBuild.exe to build and deploy the Contact Manager solution, you need to:</span></span>

- <span data-ttu-id="73a51-120">Проведение MSBuild.exe *Publish.proj* файла.</span><span class="sxs-lookup"><span data-stu-id="73a51-120">Run MSBuild.exe on the *Publish.proj* file.</span></span>
- <span data-ttu-id="73a51-121">Укажите расположение файла проекта для конкретной среды, указав параметр командной строки с именем **TargetEnvPropsFile**.</span><span class="sxs-lookup"><span data-stu-id="73a51-121">Specify the location of the environment-specific project file by supplying a command-line parameter named **TargetEnvPropsFile**.</span></span>

<span data-ttu-id="73a51-122">Чтобы сделать это, команде MSBuild должна выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="73a51-122">To do this, your MSBuild command should resemble this:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


<span data-ttu-id="73a51-123">Здесь это простой шаг, чтобы переместить repeatable, один шаг развертывания.</span><span class="sxs-lookup"><span data-stu-id="73a51-123">From here, it's a simple step to move to a repeatable, single-step deployment.</span></span> <span data-ttu-id="73a51-124">Вам нужно всего Добавление MSBuild команду cmd-файл.</span><span class="sxs-lookup"><span data-stu-id="73a51-124">All you need to do is to add your MSBuild command to a .cmd file.</span></span> <span data-ttu-id="73a51-125">В решении диспетчера контактов, в папку публикации содержит файл с именем *Dev.cmd публикации* , который выполняет именно это.</span><span class="sxs-lookup"><span data-stu-id="73a51-125">In the Contact Manager solution, the Publish folder includes a file named *Publish-Dev.cmd* that does exactly this.</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="73a51-126">**/Fl** служебная программа MSBuild для создания файла журнала с именем *msbuild.log* в рабочем каталоге, в котором был вызван MSBuild.exe.</span><span class="sxs-lookup"><span data-stu-id="73a51-126">The **/fl** switch instructs MSBuild to create a log file named *msbuild.log* in the working directory in which MSBuild.exe was invoked.</span></span>


<span data-ttu-id="73a51-127">Для развертывания или повторного развертывания решения диспетчера контактов, вам нужно лишь запустить *Dev.cmd публикации* файла.</span><span class="sxs-lookup"><span data-stu-id="73a51-127">To deploy or redeploy the Contact Manager solution, all you need to do is run the *Publish-Dev.cmd* file.</span></span> <span data-ttu-id="73a51-128">При запуске файла будет MSBuild:</span><span class="sxs-lookup"><span data-stu-id="73a51-128">When you run the file, MSBuild will:</span></span>

- <span data-ttu-id="73a51-129">Построение всех проектов в решении.</span><span class="sxs-lookup"><span data-stu-id="73a51-129">Build all the projects in the solution.</span></span>
- <span data-ttu-id="73a51-130">Создание развертывания веб-пакетов для проектов веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="73a51-130">Generate deployable web packages for the web application projects.</span></span>
- <span data-ttu-id="73a51-131">Создавать файлы расширением DBSCHEMA и .deploymanifest для проектов баз данных.</span><span class="sxs-lookup"><span data-stu-id="73a51-131">Generate .dbschema and .deploymanifest files for the database projects.</span></span>
- <span data-ttu-id="73a51-132">Развертывание веб-пакеты на веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="73a51-132">Deploy the web packages to the web server.</span></span>
- <span data-ttu-id="73a51-133">Развертывание базы данных на сервере базы данных.</span><span class="sxs-lookup"><span data-stu-id="73a51-133">Deploy the database to the database server.</span></span>

## <a name="run-the-deployment"></a><span data-ttu-id="73a51-134">Выполните развертывание</span><span class="sxs-lookup"><span data-stu-id="73a51-134">Run the Deployment</span></span>

<span data-ttu-id="73a51-135">При создании командного файла для целевой среде необходимо для завершения всего развертывания, просто запустив файл.</span><span class="sxs-lookup"><span data-stu-id="73a51-135">When you've created a command file for your target environment, you should be able to complete the entire deployment by simply running the file.</span></span>

<span data-ttu-id="73a51-136">**Чтобы развернуть решение диспетчера контактов для тестовой среды**</span><span class="sxs-lookup"><span data-stu-id="73a51-136">**To deploy the Contact Manager solution to your test environment**</span></span>

1. <span data-ttu-id="73a51-137">На компьютере разработчика откройте проводник Windows и перейдите к расположению *Dev.cmd публикации* файла.</span><span class="sxs-lookup"><span data-stu-id="73a51-137">On your developer workstation, open Windows Explorer, and then browse to the location of the *Publish-Dev.cmd* file.</span></span>
2. <span data-ttu-id="73a51-138">Дважды щелкните файл для его запуска.</span><span class="sxs-lookup"><span data-stu-id="73a51-138">Double-click the file to run it.</span></span>
3. <span data-ttu-id="73a51-139">Если **открыть файл — предупреждение системы безопасности** диалоговое окно, нажмите кнопку **запуска**.</span><span class="sxs-lookup"><span data-stu-id="73a51-139">If an **Open File – Security Warning** dialog box appears, click **Run**.</span></span>
4. <span data-ttu-id="73a51-140">Если настроить параметры конфигурации и тестовые серверы правильно, окно командной строки будет отображаться **построение выполнено успешно** сообщение после завершения обработки файлов проекта MSBuild.</span><span class="sxs-lookup"><span data-stu-id="73a51-140">If your configuration settings and test servers are set up correctly, the Command Prompt window will show a **Build succeeded** message when MSBuild has finished processing the project files.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. <span data-ttu-id="73a51-141">Если это первый раз, после развертывания решения в этой среде, необходимо добавить тест web server учетную запись компьютера для **db\_datawriter** и **db\_datareader**роли на **ContactManager** базы данных.</span><span class="sxs-lookup"><span data-stu-id="73a51-141">If this is the first time you've deployed the solution to this environment, you'll need to add the test web server machine account to the **db\_datawriter** and **db\_datareader** roles on the **ContactManager** database.</span></span> <span data-ttu-id="73a51-142">Эта процедура описана в [Настройка сервера базы данных для развертывания веб-публикаций](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="73a51-142">This procedure is described in [Configure a Database Server for Web Deploy Publishing](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="73a51-143">Необходимо назначить эти разрешения, при создании базы данных.</span><span class="sxs-lookup"><span data-stu-id="73a51-143">You only need to assign these permissions when you create the database.</span></span> <span data-ttu-id="73a51-144">По умолчанию процесс построения не будет воссоздать базу данных на каждом развертывании&#x2014;вместо этого будет сравнить существующей базы данных до самой последней схемы и сделать только необходимые изменения.</span><span class="sxs-lookup"><span data-stu-id="73a51-144">By default, the build process will not recreate the database on every deployment&#x2014;instead, it will compare the existing database to the latest schema and make only the changes required.</span></span> <span data-ttu-id="73a51-145">Поэтому следует только необходимо сопоставить этих ролей базы данных в первый раз при развертывании решения.</span><span class="sxs-lookup"><span data-stu-id="73a51-145">As a result, you should only need to map these database roles the first time you deploy the solution.</span></span>
6. <span data-ttu-id="73a51-146">Откройте Internet Explorer и перейдите к URL-адрес диспетчера контактов приложения (например, `http://testweb1:85/ContactManager/`).</span><span class="sxs-lookup"><span data-stu-id="73a51-146">Open Internet Explorer and browse to the URL of the Contact Manager application (for example, `http://testweb1:85/ContactManager/`).</span></span>
7. <span data-ttu-id="73a51-147">Убедитесь, что приложение работает правильно, и вы можете добавлять контакты.</span><span class="sxs-lookup"><span data-stu-id="73a51-147">Verify that the application works as expected and you're able to add contacts.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="73a51-148">Заключение</span><span class="sxs-lookup"><span data-stu-id="73a51-148">Conclusion</span></span>

<span data-ttu-id="73a51-149">Создав командный файл, содержащий инструкции MSBuild обеспечивает быстрый и простой способ построения и развертывания многопроектное решение в среде определенное место назначения.</span><span class="sxs-lookup"><span data-stu-id="73a51-149">Creating a command file containing your MSBuild instructions provides you with a quick and easy way of building and deploying a multi-project solution to a specific destination environment.</span></span> <span data-ttu-id="73a51-150">Если необходимо повторно развернуть решение в различных средах назначения, можно создать несколько командных файлов.</span><span class="sxs-lookup"><span data-stu-id="73a51-150">If you need to repeatedly deploy your solution to multiple destination environments, you can create multiple command files.</span></span> <span data-ttu-id="73a51-151">В каждый командный файл команды MSBuild создаст один и тот же файл универсального проекта, но он будет указать файл проекта другой среды.</span><span class="sxs-lookup"><span data-stu-id="73a51-151">In each command file, the MSBuild command will build the same universal project file, but it will specify a different environment-specific project file.</span></span> <span data-ttu-id="73a51-152">Например командный файл для публикации разработчика или тестовой среде может содержать эта команда MSBuild.</span><span class="sxs-lookup"><span data-stu-id="73a51-152">For example, a command file to publish to a developer or test environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


<span data-ttu-id="73a51-153">Командный файл для публикации в среде промежуточного хранения может содержать эта команда MSBuild.</span><span class="sxs-lookup"><span data-stu-id="73a51-153">A command file to publish to a staging environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> <span data-ttu-id="73a51-154">Рекомендации по настройке файлы проекта, относящихся к среде для серверных сред см. в разделе [настройки свойств развертывания для целевой среды](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span><span class="sxs-lookup"><span data-stu-id="73a51-154">For guidance on how to customize the environment-specific project files for your own server environments, see [Configure Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>


<span data-ttu-id="73a51-155">Можно также настроить процесс построения для каждой среды путем переопределения свойств, а также другими параметрами в команде MSBuild.</span><span class="sxs-lookup"><span data-stu-id="73a51-155">You can also customize the build process for each environment by overriding properties or setting various other switches in your MSBuild command.</span></span> <span data-ttu-id="73a51-156">Дополнительные сведения см. в разделе [Справочник по командной строке MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).</span><span class="sxs-lookup"><span data-stu-id="73a51-156">For more information, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="73a51-157">[Назад](deploying-database-projects.md)
> [Вперед](manually-installing-web-packages.md)</span><span class="sxs-lookup"><span data-stu-id="73a51-157">[Previous](deploying-database-projects.md)
[Next](manually-installing-web-packages.md)</span></span>
