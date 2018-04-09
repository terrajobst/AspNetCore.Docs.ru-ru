---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: Запуск скриптов Windows PowerShell из файлов проекта MSBuild | Документы Microsoft
author: jrjlee
description: В этом разделе описывается выполнения сценария Windows PowerShell как часть процесса построения и развертывания. Сценарий можно запустить локально (другими словами, на б...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: c8ef22cfbba7b3b85944ea4c49f3183e5a6aafbb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a><span data-ttu-id="be4e6-104">Запуск скриптов Windows PowerShell из файлов проекта MSBuild</span><span class="sxs-lookup"><span data-stu-id="be4e6-104">Running Windows PowerShell Scripts from MSBuild Project Files</span></span>
====================
<span data-ttu-id="be4e6-105">по [Джейсон Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="be4e6-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="be4e6-106">Скачать PDF</span><span class="sxs-lookup"><span data-stu-id="be4e6-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="be4e6-107">В этом разделе описывается выполнения сценария Windows PowerShell как часть процесса построения и развертывания.</span><span class="sxs-lookup"><span data-stu-id="be4e6-107">This topic describes how to run a Windows PowerShell script as part of a build and deployment process.</span></span> <span data-ttu-id="be4e6-108">Можно запустить сценарий локально (другими словами, на сервере сборки) или удаленно, как и на веб-сервере назначения или сервер базы данных.</span><span class="sxs-lookup"><span data-stu-id="be4e6-108">You can run a script locally (in other words, on the build server) or remotely, like on a destination web server or database server.</span></span>
> 
> <span data-ttu-id="be4e6-109">Существует множество причин, почему может потребоваться выполнить сценарий Windows PowerShell после развертывания.</span><span class="sxs-lookup"><span data-stu-id="be4e6-109">There are lots of reasons why you might want to run a post-deployment Windows PowerShell script.</span></span> <span data-ttu-id="be4e6-110">Например, можно сделать следующее:</span><span class="sxs-lookup"><span data-stu-id="be4e6-110">For example, you might want to:</span></span>
> 
> - <span data-ttu-id="be4e6-111">Добавление источника пользовательское событие в реестр.</span><span class="sxs-lookup"><span data-stu-id="be4e6-111">Add a custom event source to the registry.</span></span>
> - <span data-ttu-id="be4e6-112">Создайте каталог файловой системы для передачи файлов.</span><span class="sxs-lookup"><span data-stu-id="be4e6-112">Generate a file system directory for uploads.</span></span>
> - <span data-ttu-id="be4e6-113">Очистите каталоги сборки.</span><span class="sxs-lookup"><span data-stu-id="be4e6-113">Clean up build directories.</span></span>
> - <span data-ttu-id="be4e6-114">Записи в пользовательский файл журнала.</span><span class="sxs-lookup"><span data-stu-id="be4e6-114">Write entries to a custom log file.</span></span>
> - <span data-ttu-id="be4e6-115">Отправка сообщения электронной почты приглашение пользователей для нового веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="be4e6-115">Send emails inviting users to a newly provisioned web application.</span></span>
> - <span data-ttu-id="be4e6-116">Создайте учетные записи с соответствующими разрешениями.</span><span class="sxs-lookup"><span data-stu-id="be4e6-116">Create user accounts with the appropriate permissions.</span></span>
> - <span data-ttu-id="be4e6-117">Настройка репликации между экземплярами SQL Server.</span><span class="sxs-lookup"><span data-stu-id="be4e6-117">Configure replication between SQL Server instances.</span></span>
> 
> <span data-ttu-id="be4e6-118">В этом разделе показано, как запускать скрипты Windows PowerShell локально и удаленно из пользовательского целевого объекта в файле проекта Microsoft Build Engine (MSBuild).</span><span class="sxs-lookup"><span data-stu-id="be4e6-118">This topic will show you how to run Windows PowerShell scripts both locally and remotely from a custom target in a Microsoft Build Engine (MSBuild) project file.</span></span>


<span data-ttu-id="be4e6-119">Этот раздел входит в состав серию учебников, исходя из требования к развертыванию enterprise вымышленная компания Fabrikam, Inc. Этот учебник ряд использует образец решения&#x2014; [решения диспетчера контактов](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;для представления веб-приложения с реалистичных уровень сложности, включая приложения ASP.NET MVC 3, Windows Communication Служба Foundation (WCF) и проект базы данных.</span><span class="sxs-lookup"><span data-stu-id="be4e6-119">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="be4e6-120">Метод развертывания, в основе этих учебников основан на разбиение проекта файл подход, описанный в [основные сведения о файле проекта](../web-deployment-in-the-enterprise/understanding-the-project-file.md), в которой процесс построения управляется двух файлов проекта&#x2014;один с построение инструкции, которые применяются для каждой целевой среде и, содержащий параметры построения и развертывания конкретной среды.</span><span class="sxs-lookup"><span data-stu-id="be4e6-120">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="be4e6-121">Во время построения файла проекта среды объединяется в файл проекта зависит от среды, образуют полный набор инструкций построения.</span><span class="sxs-lookup"><span data-stu-id="be4e6-121">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="be4e6-122">Общие сведения о задаче</span><span class="sxs-lookup"><span data-stu-id="be4e6-122">Task Overview</span></span>

<span data-ttu-id="be4e6-123">Чтобы запустить сценарий Windows PowerShell в рамках процесса автоматической или один шаг развертывания, необходимо выполнить следующие высокоуровневые задачи:</span><span class="sxs-lookup"><span data-stu-id="be4e6-123">To run a Windows PowerShell script as part of an automated or single-step deployment process, you'll need to complete these high-level tasks:</span></span>

- <span data-ttu-id="be4e6-124">Добавление в сценарий Windows PowerShell в решение и системы управления версиями.</span><span class="sxs-lookup"><span data-stu-id="be4e6-124">Add the Windows PowerShell script to your solution and to source control.</span></span>
- <span data-ttu-id="be4e6-125">Создание команды, который вызывает сценарий Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="be4e6-125">Create a command that invokes your Windows PowerShell script.</span></span>
- <span data-ttu-id="be4e6-126">Экранировать все зарезервированные символы XML, в команде.</span><span class="sxs-lookup"><span data-stu-id="be4e6-126">Escape any reserved XML characters in your command.</span></span>
- <span data-ttu-id="be4e6-127">Создание целевого объекта в файле пользовательского проекта MSBuild и использовать **Exec** задачи для запуска вашей команды.</span><span class="sxs-lookup"><span data-stu-id="be4e6-127">Create a target in your custom MSBuild project file and use the **Exec** task to run your command.</span></span>

<span data-ttu-id="be4e6-128">В этом разделе будет показано, как для выполнения этих процедур.</span><span class="sxs-lookup"><span data-stu-id="be4e6-128">This topic will show you how to perform these procedures.</span></span> <span data-ttu-id="be4e6-129">Задачи и пошаговые руководства в этом разделе предполагается, что вы уже знакомы с целевые объекты MSBuild и свойства и понимания принципов использования пользовательского файла проекта MSBuild для управления процессом построения и развертывания.</span><span class="sxs-lookup"><span data-stu-id="be4e6-129">The tasks and walkthroughs in this topic assume that you're already familiar with MSBuild targets and properties, and that you understand how to use a custom MSBuild project file to drive a build and deployment process.</span></span> <span data-ttu-id="be4e6-130">Дополнительные сведения см. в разделе [основные сведения о файле проекта](../web-deployment-in-the-enterprise/understanding-the-project-file.md) и [основные сведения о процессе построения](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="be4e6-130">For more information, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

## <a name="creating-and-adding-windows-powershell-scripts"></a><span data-ttu-id="be4e6-131">Создание и Добавление скриптов Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="be4e6-131">Creating and Adding Windows PowerShell Scripts</span></span>

<span data-ttu-id="be4e6-132">Задачи в этом разделе используется пример сценария Windows PowerShell с именем **LogDeploy.ps1** продемонстрировать запуска скриптов из MSBuild.</span><span class="sxs-lookup"><span data-stu-id="be4e6-132">The tasks in this topic use a sample Windows PowerShell script named **LogDeploy.ps1** to illustrate how to run scripts from MSBuild.</span></span> <span data-ttu-id="be4e6-133">**LogDeploy.ps1** сценарий содержит простую функцию, которая производит запись в одну строку в файл журнала:</span><span class="sxs-lookup"><span data-stu-id="be4e6-133">The **LogDeploy.ps1** script contains a simple function that writes a single-line entry to a log file:</span></span>


[!code-javascript[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.js)]


<span data-ttu-id="be4e6-134">**LogDeploy.ps1** сценарий принимает два параметра.</span><span class="sxs-lookup"><span data-stu-id="be4e6-134">The **LogDeploy.ps1** script accepts two parameters.</span></span> <span data-ttu-id="be4e6-135">Первый параметр представляет полный путь к файлу журнала, в который требуется добавить запись, а второй параметр представляет назначение развертывания, который требуется записать в файл журнала.</span><span class="sxs-lookup"><span data-stu-id="be4e6-135">The first parameter represents the full path to the log file to which you want to add an entry, and the second parameter represents the deployment destination that you want to record in the log file.</span></span> <span data-ttu-id="be4e6-136">При запуске сценария он добавляет строки в файл журнала в формате:</span><span class="sxs-lookup"><span data-stu-id="be4e6-136">When you run the script, it adds a line to the log file in this format:</span></span>


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


<span data-ttu-id="be4e6-137">Чтобы сделать **LogDeploy.ps1** скрипты для MSBuild, необходимо:</span><span class="sxs-lookup"><span data-stu-id="be4e6-137">To make the **LogDeploy.ps1** script available to MSBuild, you need to:</span></span>

- <span data-ttu-id="be4e6-138">Добавьте скрипт в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="be4e6-138">Add the script to source control.</span></span>
- <span data-ttu-id="be4e6-139">Добавьте скрипт в решение в Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="be4e6-139">Add the script to your solution in Visual Studio 2010.</span></span>

<span data-ttu-id="be4e6-140">Развертывание сценария с контентом решения, независимо от того, планируется ли запустить сценарий на сервере построения или на удаленном компьютере не требуется.</span><span class="sxs-lookup"><span data-stu-id="be4e6-140">You don't need to deploy the script with your solution content, regardless of whether you plan to run the script on the build server or on a remote computer.</span></span> <span data-ttu-id="be4e6-141">Один из вариантов является добавление скрипта в папку решения.</span><span class="sxs-lookup"><span data-stu-id="be4e6-141">One option is to add the script to a solution folder.</span></span> <span data-ttu-id="be4e6-142">В примере диспетчера контактов так как вы хотите использовать скрипт Windows PowerShell как часть процесса развертывания, имеет смысл необходимо добавить скрипт в папку публикации решения.</span><span class="sxs-lookup"><span data-stu-id="be4e6-142">In the Contact Manager example, because you want to use the Windows PowerShell script as part of the deployment process, it makes sense to add the script to the Publish solution folder.</span></span>

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

<span data-ttu-id="be4e6-143">Содержимое папки решений копируются сборки серверов, что и исходный материал.</span><span class="sxs-lookup"><span data-stu-id="be4e6-143">The contents of solution folders are copied to build servers as source material.</span></span> <span data-ttu-id="be4e6-144">Тем не менее они образуют никакая часть выходных данных проекта.</span><span class="sxs-lookup"><span data-stu-id="be4e6-144">However, they form no part of any project output.</span></span>

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a><span data-ttu-id="be4e6-145">Выполнение скрипта Windows PowerShell на сервере сборки</span><span class="sxs-lookup"><span data-stu-id="be4e6-145">Executing a Windows PowerShell Script on the Build Server</span></span>

<span data-ttu-id="be4e6-146">В некоторых сценариях может потребоваться запускать скрипты Windows PowerShell на компьютере, который создает проекты.</span><span class="sxs-lookup"><span data-stu-id="be4e6-146">In some scenarios, you may want to run Windows PowerShell scripts on the computer that builds your projects.</span></span> <span data-ttu-id="be4e6-147">Например сценарий Windows PowerShell может использовать для очистки папки сборки или записи в собственный файл журнала.</span><span class="sxs-lookup"><span data-stu-id="be4e6-147">For example, you might use a Windows PowerShell script to clean up build folders or write entries to a custom log file.</span></span>

<span data-ttu-id="be4e6-148">С точки зрения синтаксиса выполнив сценарий Windows PowerShell из файла проекта MSBuild совпадает со значением выполнения сценария Windows PowerShell из командной строки регулярного.</span><span class="sxs-lookup"><span data-stu-id="be4e6-148">In terms of syntax, running a Windows PowerShell script from an MSBuild project file is the same as running a Windows PowerShell script from a regular command prompt.</span></span> <span data-ttu-id="be4e6-149">Необходимо вызвать powershell.exe исполняемый файл и использовать **— команда** коммутатору для обеспечения нужные команды Windows PowerShell для выполнения.</span><span class="sxs-lookup"><span data-stu-id="be4e6-149">You need to invoke the powershell.exe executable and use the **–command** switch to provide the commands you want Windows PowerShell to run.</span></span> <span data-ttu-id="be4e6-150">(В Windows PowerShell v2, можно также использовать **— файл** переключения).</span><span class="sxs-lookup"><span data-stu-id="be4e6-150">(In Windows PowerShell v2, you can also use the **–file** switch).</span></span> <span data-ttu-id="be4e6-151">Этот формат должна выполнить команда:</span><span class="sxs-lookup"><span data-stu-id="be4e6-151">The command should take this format:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


<span data-ttu-id="be4e6-152">Пример:</span><span class="sxs-lookup"><span data-stu-id="be4e6-152">For example:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


<span data-ttu-id="be4e6-153">Если путь к скрипту содержит пробелы, необходимо заключить в одинарные кавычки, следующий за знаком амперсанда путь к файлу.</span><span class="sxs-lookup"><span data-stu-id="be4e6-153">If the path to your script includes spaces, you need to enclose the file path in single quotes preceded by an ampersand.</span></span> <span data-ttu-id="be4e6-154">Нельзя использовать двойными кавычками, так как вы уже использовали их следует заключать в команде:</span><span class="sxs-lookup"><span data-stu-id="be4e6-154">You can't use double quotes, because you've already used them to enclose the command:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


<span data-ttu-id="be4e6-155">При вызове этой команды из MSBuild, существуют несколько моментов.</span><span class="sxs-lookup"><span data-stu-id="be4e6-155">There are a few additional considerations when you invoke this command from MSBuild.</span></span> <span data-ttu-id="be4e6-156">Во-первых, необходимо включить **— NonInteractive** флаг, чтобы убедиться, что сценарий выполняется без вмешательства пользователя.</span><span class="sxs-lookup"><span data-stu-id="be4e6-156">First, you should include the **–NonInteractive** flag to ensure that the script executes quietly.</span></span> <span data-ttu-id="be4e6-157">Далее следует включать **-ExecutionPolicy** флаг со значением соответствующего аргумента.</span><span class="sxs-lookup"><span data-stu-id="be4e6-157">Next, you should include the **–ExecutionPolicy** flag with an appropriate argument value.</span></span> <span data-ttu-id="be4e6-158">Указывает политику выполнения будет применяться к сценарию и позволяет переопределить политику выполнения по умолчанию, которое может помешать выполнения сценария Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="be4e6-158">This specifies the execution policy that Windows PowerShell will apply to your script and allows you to override the default execution policy, which may prevent execution of your script.</span></span> <span data-ttu-id="be4e6-159">Можно выбрать один из этих значений аргумента:</span><span class="sxs-lookup"><span data-stu-id="be4e6-159">You can choose from these argument values:</span></span>

- <span data-ttu-id="be4e6-160">Значение **Unrestricted** позволит Windows PowerShell для выполнения сценария, независимо от того, имеет ли подпись скрипт.</span><span class="sxs-lookup"><span data-stu-id="be4e6-160">A value of **Unrestricted** will allow Windows PowerShell to execute your script, regardless of whether the script is signed.</span></span>
- <span data-ttu-id="be4e6-161">Значение **RemoteSigned** позволит Windows PowerShell для выполнения неподписанных скриптов, которые были созданы на локальном компьютере.</span><span class="sxs-lookup"><span data-stu-id="be4e6-161">A value of **RemoteSigned** will allow Windows PowerShell to execute unsigned scripts that were created on the local machine.</span></span> <span data-ttu-id="be4e6-162">Тем не менее должны быть подписаны скрипты, которые были созданы в другом месте.</span><span class="sxs-lookup"><span data-stu-id="be4e6-162">However, scripts that were created elsewhere must be signed.</span></span> <span data-ttu-id="be4e6-163">(На практике, вы создали сценарий Windows PowerShell локально на сервере сборки крайне маловероятно).</span><span class="sxs-lookup"><span data-stu-id="be4e6-163">(In practice, you're very unlikely to have created a Windows PowerShell script locally on a build server).</span></span>
- <span data-ttu-id="be4e6-164">Значение **AllSigned** позволит Windows PowerShell для выполнения только подписанные скриптов.</span><span class="sxs-lookup"><span data-stu-id="be4e6-164">A value of **AllSigned** will allow Windows PowerShell to execute signed scripts only.</span></span>

<span data-ttu-id="be4e6-165">Политика выполнения по умолчанию — **Restricted**, который предотвращает запуск любых файлов скрипта Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="be4e6-165">The default execution policy is **Restricted**, which prevents Windows PowerShell from running any script files.</span></span>

<span data-ttu-id="be4e6-166">Наконец необходимо экранировать все зарезервированные символы XML, которые происходят в вашей команды Windows PowerShell:</span><span class="sxs-lookup"><span data-stu-id="be4e6-166">Finally, you need to escape any reserved XML characters that occur in your Windows PowerShell command:</span></span>

- <span data-ttu-id="be4e6-167">Замените одинарные кавычки с  **&amp;apos;**</span><span class="sxs-lookup"><span data-stu-id="be4e6-167">Replace single quotes with **&amp;apos;**</span></span>
- <span data-ttu-id="be4e6-168">Замените двойные кавычки с  **&amp;quot;**</span><span class="sxs-lookup"><span data-stu-id="be4e6-168">Replace double quotes with **&amp;quot;**</span></span>
- <span data-ttu-id="be4e6-169">Замените амперсанды с  **&amp;amp;**</span><span class="sxs-lookup"><span data-stu-id="be4e6-169">Replace ampersands with **&amp;amp;**</span></span>

- <span data-ttu-id="be4e6-170">При внесении этих изменений, команда будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="be4e6-170">When you make these changes, your command will resemble this:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


<span data-ttu-id="be4e6-171">В рамках пользовательского файла проекта MSBuild, можно создать новую цель и использовать **Exec** задачи для выполнения этой команды:</span><span class="sxs-lookup"><span data-stu-id="be4e6-171">Within your custom MSBuild project file, you can create a new target and use the **Exec** task to run this command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


<span data-ttu-id="be4e6-172">В этом примере обратите внимание, что:</span><span class="sxs-lookup"><span data-stu-id="be4e6-172">In this example, note that:</span></span>

- <span data-ttu-id="be4e6-173">Переменные, такие как значения параметров и расположение исполняемого файла Windows PowerShell, объявленные как свойства MSBuild.</span><span class="sxs-lookup"><span data-stu-id="be4e6-173">Any variables, like parameter values and the location of the Windows PowerShell executable, are declared as MSBuild properties.</span></span>
- <span data-ttu-id="be4e6-174">Условия включаются, чтобы пользователь мог переопределить эти значения из командной строки.</span><span class="sxs-lookup"><span data-stu-id="be4e6-174">Conditions are included to enable users to override these values from the command line.</span></span>
- <span data-ttu-id="be4e6-175">**MSDeployComputerName** свойство объявлено в другом месте в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="be4e6-175">The **MSDeployComputerName** property is declared elsewhere in the project file.</span></span>

<span data-ttu-id="be4e6-176">При выполнении этот целевой объект как часть процесса построения, Windows PowerShell запустите команду и создать запись журнала в указанный файл.</span><span class="sxs-lookup"><span data-stu-id="be4e6-176">When you execute this target as part of your build process, Windows PowerShell will run your command and write a log entry to the file you specified.</span></span>

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a><span data-ttu-id="be4e6-177">Выполнение скрипта Windows PowerShell на удаленном компьютере</span><span class="sxs-lookup"><span data-stu-id="be4e6-177">Executing a Windows PowerShell Script on a Remote Computer</span></span>

<span data-ttu-id="be4e6-178">Windows PowerShell — могут выполняться скрипты в удаленных компьютеров с помощью [удаленное управление Windows](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span><span class="sxs-lookup"><span data-stu-id="be4e6-178">Windows PowerShell is capable of running scripts on remote computers through [Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span></span> <span data-ttu-id="be4e6-179">Чтобы сделать это, необходимо использовать [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) командлета.</span><span class="sxs-lookup"><span data-stu-id="be4e6-179">To do this, you need to use the [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet.</span></span> <span data-ttu-id="be4e6-180">Это дает возможность выполнения сценария с одной или нескольких удаленных компьютерах без копирования скрипт на удаленных компьютерах.</span><span class="sxs-lookup"><span data-stu-id="be4e6-180">This lets you execute your script against one or more remote computers without copying the script to the remote computers.</span></span> <span data-ttu-id="be4e6-181">Результаты возвращаются на локальный компьютер, из которого запускается скрипт.</span><span class="sxs-lookup"><span data-stu-id="be4e6-181">Any results are returned to the local computer from which you ran the script.</span></span>

> [!NOTE]
> <span data-ttu-id="be4e6-182">Прежде чем использовать **Invoke-Command** командлета Windows PowerShell выполнение скриптов на удаленном компьютере, необходимо настроить прослушиватель WinRM для принятия удаленных сообщений.</span><span class="sxs-lookup"><span data-stu-id="be4e6-182">Before you use the **Invoke-Command** cmdlet to execute Windows PowerShell scripts on a remote computer, you need to configure a WinRM listener to accept remote messages.</span></span> <span data-ttu-id="be4e6-183">Это можно сделать, выполнив команду **winrm quickconfig** на удаленном компьютере.</span><span class="sxs-lookup"><span data-stu-id="be4e6-183">You can do this by running the command **winrm quickconfig** on the remote computer.</span></span> <span data-ttu-id="be4e6-184">Дополнительные сведения см. в разделе [установки и конфигурации для удаленного управления Windows](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="be4e6-184">For more information, see [Installation and Configuration for Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span></span>


<span data-ttu-id="be4e6-185">В окне Windows PowerShell для запуска необходимо выполнить этот синтаксис **LogDeploy.ps1** сценария на удаленном компьютере:</span><span class="sxs-lookup"><span data-stu-id="be4e6-185">From a Windows PowerShell window, you'd use this syntax to run the **LogDeploy.ps1** script on a remote computer:</span></span>


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> <span data-ttu-id="be4e6-186">Существует несколько способов использования **Invoke-Command** для выполнения сценария файл, но этот подход является наиболее простым для указания значений параметров и управления пути с пробелами.</span><span class="sxs-lookup"><span data-stu-id="be4e6-186">There are various other ways of using **Invoke-Command** to run a script file, but this approach is the most straightforward when you need to provide parameter values and manage paths with spaces.</span></span>


<span data-ttu-id="be4e6-187">При запуске из командной строки, необходимо вызвать исполняемый файл Windows PowerShell и использовать **— команда** параметр для указания:</span><span class="sxs-lookup"><span data-stu-id="be4e6-187">When you run this from a command prompt, you need to invoke the Windows PowerShell executable and use the **–command** parameter to provide your instructions:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


<span data-ttu-id="be4e6-188">Как и прежде, необходимо предоставить некоторые дополнительные ключи и экранировать любые зарезервированные символы XML, при выполнении команды из MSBuild:</span><span class="sxs-lookup"><span data-stu-id="be4e6-188">As before, you need to provide some additional switches and escape any reserved XML characters when you run the command from MSBuild:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


<span data-ttu-id="be4e6-189">Наконец, как и раньше, можно использовать **Exec** задачу в целевой объект пользовательские MSBuild для выполнения команды:</span><span class="sxs-lookup"><span data-stu-id="be4e6-189">Finally, as before, you can use the **Exec** task within a custom MSBuild target to execute your command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


<span data-ttu-id="be4e6-190">При выполнении этот целевой объект как часть процесса построения, Windows PowerShell ваш скрипт будет выполняться на компьютере, указанном в **– computername** аргумент.</span><span class="sxs-lookup"><span data-stu-id="be4e6-190">When you execute this target as part of your build process, Windows PowerShell will run your script on the computer you specified in the **–computername** argument.</span></span>

## <a name="conclusion"></a><span data-ttu-id="be4e6-191">Заключение</span><span class="sxs-lookup"><span data-stu-id="be4e6-191">Conclusion</span></span>

<span data-ttu-id="be4e6-192">В этом разделе описано, как запустить сценарий Windows PowerShell из файла проекта MSBuild.</span><span class="sxs-lookup"><span data-stu-id="be4e6-192">This topic described how to run a Windows PowerShell script from an MSBuild project file.</span></span> <span data-ttu-id="be4e6-193">Этот подход можно использовать для выполнения сценария Windows PowerShell, локально или на удаленном компьютере, как часть автоматической или один шаг процесса построения и развертывания.</span><span class="sxs-lookup"><span data-stu-id="be4e6-193">You can use this approach to run a Windows PowerShell script, either locally or on a remote computer, as part of an automated or single-step build and deployment process.</span></span>

## <a name="further-reading"></a><span data-ttu-id="be4e6-194">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="be4e6-194">Further Reading</span></span>

<span data-ttu-id="be4e6-195">Сведения о подписывании скриптов Windows PowerShell и управление политиками выполнения. в разделе [запуск скриптов Windows PowerShell](https://technet.microsoft.com/library/ee176949.aspx).</span><span class="sxs-lookup"><span data-stu-id="be4e6-195">For guidance on signing Windows PowerShell scripts and managing execution policies, see [Running Windows PowerShell Scripts](https://technet.microsoft.com/library/ee176949.aspx).</span></span> <span data-ttu-id="be4e6-196">Рекомендации по выполнение команд Windows PowerShell с удаленного компьютера см. в разделе [выполнение удаленных команд](https://technet.microsoft.com/library/dd819505.aspx).</span><span class="sxs-lookup"><span data-stu-id="be4e6-196">For guidance on running Windows PowerShell commands from a remote computer, see [Running Remote Commands](https://technet.microsoft.com/library/dd819505.aspx).</span></span>

<span data-ttu-id="be4e6-197">Дополнительные сведения об использовании пользовательские файлы проекта MSBuild для управления процессом развертывания см. в разделе [основные сведения о файле проекта](../web-deployment-in-the-enterprise/understanding-the-project-file.md) и [основные сведения о процессе построения](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="be4e6-197">For more information on using custom MSBuild project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="be4e6-198">[Назад](taking-web-applications-offline-with-web-deploy.md)
> [Вперед](troubleshooting-the-packaging-process.md)</span><span class="sxs-lookup"><span data-stu-id="be4e6-198">[Previous](taking-web-applications-offline-with-web-deploy.md)
[Next](troubleshooting-the-packaging-process.md)</span></span>
