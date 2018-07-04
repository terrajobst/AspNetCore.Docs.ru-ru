---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: Запуск скриптов Windows PowerShell из файлов проекта MSBuild | Документация Майкрософт
author: jrjlee
description: В этом разделе описывается, как для выполнения сценария Windows PowerShell как часть процесса построения и развертывания. Сценарий можно запустить локально (другими словами, на б...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: ddb658d8a8f224a7c417321df3e17ce0610d2473
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362899"
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a><span data-ttu-id="9ea4a-104">Запуск скриптов Windows PowerShell из файлов проекта MSBuild</span><span class="sxs-lookup"><span data-stu-id="9ea4a-104">Running Windows PowerShell Scripts from MSBuild Project Files</span></span>
====================
<span data-ttu-id="9ea4a-105">по [Джейсон Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="9ea4a-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="9ea4a-106">Загрузить PDF-файл</span><span class="sxs-lookup"><span data-stu-id="9ea4a-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="9ea4a-107">В этом разделе описывается, как для выполнения сценария Windows PowerShell как часть процесса построения и развертывания.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-107">This topic describes how to run a Windows PowerShell script as part of a build and deployment process.</span></span> <span data-ttu-id="9ea4a-108">Можно запустить сценарий локально (другими словами, на сервере сборки) или удаленно, например на веб-сервере назначения или сервер базы данных.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-108">You can run a script locally (in other words, on the build server) or remotely, like on a destination web server or database server.</span></span>
> 
> <span data-ttu-id="9ea4a-109">Существует множество причин, почему может потребоваться выполнить скрипт Windows PowerShell после развертывания.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-109">There are lots of reasons why you might want to run a post-deployment Windows PowerShell script.</span></span> <span data-ttu-id="9ea4a-110">Например, можно сделать следующее:</span><span class="sxs-lookup"><span data-stu-id="9ea4a-110">For example, you might want to:</span></span>
> 
> - <span data-ttu-id="9ea4a-111">Добавьте источник пользовательское событие в реестр.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-111">Add a custom event source to the registry.</span></span>
> - <span data-ttu-id="9ea4a-112">Создайте каталог в файловой системе для передачи файлов.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-112">Generate a file system directory for uploads.</span></span>
> - <span data-ttu-id="9ea4a-113">Очистите каталогов сборки.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-113">Clean up build directories.</span></span>
> - <span data-ttu-id="9ea4a-114">Записи в файл пользовательского журнала.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-114">Write entries to a custom log file.</span></span>
> - <span data-ttu-id="9ea4a-115">Отправка сообщения электронной почты приглашение пользователей подготовленные веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-115">Send emails inviting users to a newly provisioned web application.</span></span>
> - <span data-ttu-id="9ea4a-116">Создайте учетные записи с соответствующими разрешениями.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-116">Create user accounts with the appropriate permissions.</span></span>
> - <span data-ttu-id="9ea4a-117">Настройка репликации между экземплярами SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-117">Configure replication between SQL Server instances.</span></span>
> 
> <span data-ttu-id="9ea4a-118">В этом разделе показано, как запустить сценарии Windows PowerShell, как локально, так и удаленно из пользовательский целевой объект в файл проекта Microsoft Build Engine (MSBuild).</span><span class="sxs-lookup"><span data-stu-id="9ea4a-118">This topic will show you how to run Windows PowerShell scripts both locally and remotely from a custom target in a Microsoft Build Engine (MSBuild) project file.</span></span>


<span data-ttu-id="9ea4a-119">Этот раздел является частью серии учебников, исходя из требования к развертыванию enterprise вымышленной компании Fabrikam, Inc. В этой серии руководств используется пример решения&#x2014; [решения диспетчера контактов](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;для представления веб-приложения с более реалистичные уровень сложности, включая приложения ASP.NET MVC 3, Windows Communication Служба Foundation (WCF) и проект базы данных.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-119">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="9ea4a-120">Метод развертывания в основе этих учебников основан на разбиение проекта файл подход, описанный в [основные сведения о файле проекта](../web-deployment-in-the-enterprise/understanding-the-project-file.md), в которой процесс построения управляется двумя файлы проекта&#x2014;один с Создание инструкции, которые применяются для каждой целевой среде и содержащего параметры построения и развертывания для конкретной среды.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-120">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="9ea4a-121">Во время сборки файл проекта для конкретной среды объединяется в файл проекта независимой от среды, чтобы полный набор инструкций построения.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-121">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="9ea4a-122">Общие сведения о задачах</span><span class="sxs-lookup"><span data-stu-id="9ea4a-122">Task Overview</span></span>

<span data-ttu-id="9ea4a-123">Чтобы запустить сценарий Windows PowerShell как часть процесса автоматической или один шаг развертывания, необходимо выполнить следующие высокоуровневые задачи:</span><span class="sxs-lookup"><span data-stu-id="9ea4a-123">To run a Windows PowerShell script as part of an automated or single-step deployment process, you'll need to complete these high-level tasks:</span></span>

- <span data-ttu-id="9ea4a-124">Добавьте скрипт Windows PowerShell, в решение и в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-124">Add the Windows PowerShell script to your solution and to source control.</span></span>
- <span data-ttu-id="9ea4a-125">Создайте команду, которая вызывает сценарий Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-125">Create a command that invokes your Windows PowerShell script.</span></span>
- <span data-ttu-id="9ea4a-126">Экранировать все зарезервированные символы XML, в команде.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-126">Escape any reserved XML characters in your command.</span></span>
- <span data-ttu-id="9ea4a-127">Создание целевого объекта в пользовательский файл проекта MSBuild и использовать **Exec** задачу для выполнения команды.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-127">Create a target in your custom MSBuild project file and use the **Exec** task to run your command.</span></span>

<span data-ttu-id="9ea4a-128">В этом разделе будет показано, как для выполнения этих процедур.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-128">This topic will show you how to perform these procedures.</span></span> <span data-ttu-id="9ea4a-129">Задачи и пошаговые руководства в этом разделе предполагается, что вы уже знакомы с целевые объекты MSBuild и свойства, и понять, как использовать пользовательский файл проекта MSBuild для управления процессом построения и развертывания.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-129">The tasks and walkthroughs in this topic assume that you're already familiar with MSBuild targets and properties, and that you understand how to use a custom MSBuild project file to drive a build and deployment process.</span></span> <span data-ttu-id="9ea4a-130">Дополнительные сведения см. в разделе [основные сведения о файле проекта](../web-deployment-in-the-enterprise/understanding-the-project-file.md) и [основные сведения о процессе построения](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="9ea4a-130">For more information, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

## <a name="creating-and-adding-windows-powershell-scripts"></a><span data-ttu-id="9ea4a-131">Создание и Добавление скриптов Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="9ea4a-131">Creating and Adding Windows PowerShell Scripts</span></span>

<span data-ttu-id="9ea4a-132">Задачи в этом разделе используется пример сценария Windows PowerShell с именем **LogDeploy.ps1** Чтобы проиллюстрировать процесс для выполнения скриптов с MSBuild.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-132">The tasks in this topic use a sample Windows PowerShell script named **LogDeploy.ps1** to illustrate how to run scripts from MSBuild.</span></span> <span data-ttu-id="9ea4a-133">**LogDeploy.ps1** скрипт содержит простой функции, которая производит запись в одну строку в файл журнала:</span><span class="sxs-lookup"><span data-stu-id="9ea4a-133">The **LogDeploy.ps1** script contains a simple function that writes a single-line entry to a log file:</span></span>


[!code-javascript[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.js)]


<span data-ttu-id="9ea4a-134">**LogDeploy.ps1** скрипт принимает два параметра.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-134">The **LogDeploy.ps1** script accepts two parameters.</span></span> <span data-ttu-id="9ea4a-135">Первый параметр представляет полный путь к файлу журнала, к которому требуется добавить запись, а второй параметр представляет назначение развертывания, который требуется записать в файл журнала.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-135">The first parameter represents the full path to the log file to which you want to add an entry, and the second parameter represents the deployment destination that you want to record in the log file.</span></span> <span data-ttu-id="9ea4a-136">При выполнении сценарий, он добавляет строку в файл журнала в следующем формате:</span><span class="sxs-lookup"><span data-stu-id="9ea4a-136">When you run the script, it adds a line to the log file in this format:</span></span>


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


<span data-ttu-id="9ea4a-137">Чтобы сделать **LogDeploy.ps1** скрипт для MSBuild, вам потребуется:</span><span class="sxs-lookup"><span data-stu-id="9ea4a-137">To make the **LogDeploy.ps1** script available to MSBuild, you need to:</span></span>

- <span data-ttu-id="9ea4a-138">Добавьте скрипт в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-138">Add the script to source control.</span></span>
- <span data-ttu-id="9ea4a-139">Добавьте скрипт в решение в Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-139">Add the script to your solution in Visual Studio 2010.</span></span>

<span data-ttu-id="9ea4a-140">Не нужно развернуть скрипт на ваше содержимое решения, независимо от того, планируете ли вы запустить скрипт на сервере сборки, или на удаленном компьютере.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-140">You don't need to deploy the script with your solution content, regardless of whether you plan to run the script on the build server or on a remote computer.</span></span> <span data-ttu-id="9ea4a-141">Один из вариантов — в том, чтобы добавить скрипт в папку решения.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-141">One option is to add the script to a solution folder.</span></span> <span data-ttu-id="9ea4a-142">В примере диспетчера контактов так как вы хотите использовать скрипт Windows PowerShell как часть процесса развертывания, имеет смысл необходимо добавить скрипт в папку публикации решения.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-142">In the Contact Manager example, because you want to use the Windows PowerShell script as part of the deployment process, it makes sense to add the script to the Publish solution folder.</span></span>

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

<span data-ttu-id="9ea4a-143">Содержимое папки решений копируются серверов сборки в качестве исходного материала.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-143">The contents of solution folders are copied to build servers as source material.</span></span> <span data-ttu-id="9ea4a-144">Тем не менее они образуют нет никаких выходных данных проекта.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-144">However, they form no part of any project output.</span></span>

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a><span data-ttu-id="9ea4a-145">Выполнение скрипта Windows PowerShell на сервере сборки</span><span class="sxs-lookup"><span data-stu-id="9ea4a-145">Executing a Windows PowerShell Script on the Build Server</span></span>

<span data-ttu-id="9ea4a-146">В некоторых сценариях может потребоваться запускать сценарии Windows PowerShell на компьютере, который создает проекты.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-146">In some scenarios, you may want to run Windows PowerShell scripts on the computer that builds your projects.</span></span> <span data-ttu-id="9ea4a-147">Например можно использовать сценарий Windows PowerShell для очистки папки сборки или записи в файл пользовательского журнала.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-147">For example, you might use a Windows PowerShell script to clean up build folders or write entries to a custom log file.</span></span>

<span data-ttu-id="9ea4a-148">С точки зрения синтаксиса выполнив сценарий Windows PowerShell из файла проекта MSBuild совпадает со значением сценария Windows PowerShell из регулярного командной строки.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-148">In terms of syntax, running a Windows PowerShell script from an MSBuild project file is the same as running a Windows PowerShell script from a regular command prompt.</span></span> <span data-ttu-id="9ea4a-149">Необходимо вызвать исполняемый файл powershell.exe и использовать **— команда** коммутатору для обеспечения команды Windows PowerShell для запуска.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-149">You need to invoke the powershell.exe executable and use the **–command** switch to provide the commands you want Windows PowerShell to run.</span></span> <span data-ttu-id="9ea4a-150">(В Windows PowerShell версии 2, можно также использовать **— файл** переключаться).</span><span class="sxs-lookup"><span data-stu-id="9ea4a-150">(In Windows PowerShell v2, you can also use the **–file** switch).</span></span> <span data-ttu-id="9ea4a-151">Команду следует выполнить этот формат:</span><span class="sxs-lookup"><span data-stu-id="9ea4a-151">The command should take this format:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


<span data-ttu-id="9ea4a-152">Пример:</span><span class="sxs-lookup"><span data-stu-id="9ea4a-152">For example:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


<span data-ttu-id="9ea4a-153">Если путь к сценарию содержит пробелы, необходимо заключить в одинарные кавычки, следующий за знаком амперсанда путь к файлу.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-153">If the path to your script includes spaces, you need to enclose the file path in single quotes preceded by an ampersand.</span></span> <span data-ttu-id="9ea4a-154">Нельзя использовать двойные кавычки, так как вы уже использовали их в нужно вложить команду:</span><span class="sxs-lookup"><span data-stu-id="9ea4a-154">You can't use double quotes, because you've already used them to enclose the command:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


<span data-ttu-id="9ea4a-155">При вызове этой команды из MSBuild, существует несколько дополнительных факторов.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-155">There are a few additional considerations when you invoke this command from MSBuild.</span></span> <span data-ttu-id="9ea4a-156">Во-первых, следует включить **— NonInteractive** флажок, чтобы гарантировать, что сценарий выполняется без вмешательства пользователя.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-156">First, you should include the **–NonInteractive** flag to ensure that the script executes quietly.</span></span> <span data-ttu-id="9ea4a-157">Далее следует включить **-ExecutionPolicy** флаг со значением соответствующего аргумента.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-157">Next, you should include the **–ExecutionPolicy** flag with an appropriate argument value.</span></span> <span data-ttu-id="9ea4a-158">Указывает политику выполнения, что Windows PowerShell будет применяться к сценарию и позволяет переопределить политику выполнения по умолчанию, которые могут препятствовать выполнение скрипта.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-158">This specifies the execution policy that Windows PowerShell will apply to your script and allows you to override the default execution policy, which may prevent execution of your script.</span></span> <span data-ttu-id="9ea4a-159">Можно выбрать из значения аргументов:</span><span class="sxs-lookup"><span data-stu-id="9ea4a-159">You can choose from these argument values:</span></span>

- <span data-ttu-id="9ea4a-160">Значение **Unrestricted** позволит Windows PowerShell для выполнения скрипта, независимо от того, подписан ли сценарий.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-160">A value of **Unrestricted** will allow Windows PowerShell to execute your script, regardless of whether the script is signed.</span></span>
- <span data-ttu-id="9ea4a-161">Значение **RemoteSigned** позволит Windows PowerShell для выполнения неподписанных скриптов, которые были созданы на локальном компьютере.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-161">A value of **RemoteSigned** will allow Windows PowerShell to execute unsigned scripts that were created on the local machine.</span></span> <span data-ttu-id="9ea4a-162">Тем не менее должны быть подписаны скрипты, которые были созданы в другом месте.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-162">However, scripts that were created elsewhere must be signed.</span></span> <span data-ttu-id="9ea4a-163">(На практике, вы скорее всего, для создания сценария Windows PowerShell локально на сервере сборки).</span><span class="sxs-lookup"><span data-stu-id="9ea4a-163">(In practice, you're very unlikely to have created a Windows PowerShell script locally on a build server).</span></span>
- <span data-ttu-id="9ea4a-164">Значение **AllSigned** позволит Windows PowerShell для выполнения только подписанные сценарии.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-164">A value of **AllSigned** will allow Windows PowerShell to execute signed scripts only.</span></span>

<span data-ttu-id="9ea4a-165">Политика выполнения по умолчанию — **Restricted**, который предотвращает запуск любых файлов скрипта Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-165">The default execution policy is **Restricted**, which prevents Windows PowerShell from running any script files.</span></span>

<span data-ttu-id="9ea4a-166">Наконец вам потребуется экранировать все зарезервированные символы XML, которые происходят в вашей команды Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-166">Finally, you need to escape any reserved XML characters that occur in your Windows PowerShell command:</span></span>

- <span data-ttu-id="9ea4a-167">Замените одинарные кавычки с  **&amp;apos;**</span><span class="sxs-lookup"><span data-stu-id="9ea4a-167">Replace single quotes with **&amp;apos;**</span></span>
- <span data-ttu-id="9ea4a-168">Замените двойные кавычки с  **&amp;quot;**</span><span class="sxs-lookup"><span data-stu-id="9ea4a-168">Replace double quotes with **&amp;quot;**</span></span>
- <span data-ttu-id="9ea4a-169">Замените амперсанды с  **&amp;amp;**</span><span class="sxs-lookup"><span data-stu-id="9ea4a-169">Replace ampersands with **&amp;amp;**</span></span>

- <span data-ttu-id="9ea4a-170">При внесении этих изменений, команда будет выглядеть примерно следующим образом:</span><span class="sxs-lookup"><span data-stu-id="9ea4a-170">When you make these changes, your command will resemble this:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


<span data-ttu-id="9ea4a-171">В рамках пользовательского файла проекта MSBuild, можно создать новую цель и использовать **Exec** задачи для выполнения этой команды:</span><span class="sxs-lookup"><span data-stu-id="9ea4a-171">Within your custom MSBuild project file, you can create a new target and use the **Exec** task to run this command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


<span data-ttu-id="9ea4a-172">В этом примере обратите внимание, что:</span><span class="sxs-lookup"><span data-stu-id="9ea4a-172">In this example, note that:</span></span>

- <span data-ttu-id="9ea4a-173">Переменные, такие как значения параметров и расположение исполняемого файла Windows PowerShell, обозначаются как свойства MSBuild.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-173">Any variables, like parameter values and the location of the Windows PowerShell executable, are declared as MSBuild properties.</span></span>
- <span data-ttu-id="9ea4a-174">Условия включены для Разрешите пользователям переопределять эти значения из командной строки.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-174">Conditions are included to enable users to override these values from the command line.</span></span>
- <span data-ttu-id="9ea4a-175">**MSDeployComputerName** свойство уже объявленного в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-175">The **MSDeployComputerName** property is declared elsewhere in the project file.</span></span>

<span data-ttu-id="9ea4a-176">При выполнении данного целевого объекта как часть процесса построения, Windows PowerShell выполнения команды и записи в журнал событий в указанный файл.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-176">When you execute this target as part of your build process, Windows PowerShell will run your command and write a log entry to the file you specified.</span></span>

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a><span data-ttu-id="9ea4a-177">Выполнение скрипта Windows PowerShell на удаленном компьютере</span><span class="sxs-lookup"><span data-stu-id="9ea4a-177">Executing a Windows PowerShell Script on a Remote Computer</span></span>

<span data-ttu-id="9ea4a-178">Windows PowerShell можно запускать скрипты на удаленных компьютерах с помощью [удаленное управление Windows](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span><span class="sxs-lookup"><span data-stu-id="9ea4a-178">Windows PowerShell is capable of running scripts on remote computers through [Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span></span> <span data-ttu-id="9ea4a-179">Чтобы сделать это, необходимо использовать [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) командлета.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-179">To do this, you need to use the [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet.</span></span> <span data-ttu-id="9ea4a-180">Это дает возможность выполнения скрипта для одного или нескольких удаленных компьютеров без копирования скрипт на удаленных компьютерах.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-180">This lets you execute your script against one or more remote computers without copying the script to the remote computers.</span></span> <span data-ttu-id="9ea4a-181">Результаты возвращаются на локальный компьютер, из которого запускается скрипт.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-181">Any results are returned to the local computer from which you ran the script.</span></span>

> [!NOTE]
> <span data-ttu-id="9ea4a-182">Прежде чем использовать **Invoke-Command** командлета Windows PowerShell выполнить скрипты на удаленном компьютере, необходимо настроить прослушиватель WinRM для принятия удаленных сообщений.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-182">Before you use the **Invoke-Command** cmdlet to execute Windows PowerShell scripts on a remote computer, you need to configure a WinRM listener to accept remote messages.</span></span> <span data-ttu-id="9ea4a-183">Это можно сделать с помощью команды **winrm quickconfig** на удаленном компьютере.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-183">You can do this by running the command **winrm quickconfig** on the remote computer.</span></span> <span data-ttu-id="9ea4a-184">Дополнительные сведения см. в разделе [Установка и настройка Windows удаленного управления](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="9ea4a-184">For more information, see [Installation and Configuration for Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span></span>


<span data-ttu-id="9ea4a-185">В окне Windows PowerShell, необходимо использовать этот синтаксис для запуска **LogDeploy.ps1** скрипт на удаленном компьютере:</span><span class="sxs-lookup"><span data-stu-id="9ea4a-185">From a Windows PowerShell window, you'd use this syntax to run the **LogDeploy.ps1** script on a remote computer:</span></span>


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> <span data-ttu-id="9ea4a-186">Существует несколько способов использования **Invoke-Command** для выполнения сценария файл, но этот подход является самым простым для предоставления значений параметров и управления ими путей с пробелами.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-186">There are various other ways of using **Invoke-Command** to run a script file, but this approach is the most straightforward when you need to provide parameter values and manage paths with spaces.</span></span>


<span data-ttu-id="9ea4a-187">При запуске из командной строки, необходимо вызвать исполняемый файл Windows PowerShell и используйте **— команда** параметр для предоставления собственных инструкций:</span><span class="sxs-lookup"><span data-stu-id="9ea4a-187">When you run this from a command prompt, you need to invoke the Windows PowerShell executable and use the **–command** parameter to provide your instructions:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


<span data-ttu-id="9ea4a-188">Как ранее, необходимо предоставить некоторые дополнительные параметры и экранировать любые зарезервированные символы XML, при выполнении команды из MSBuild:</span><span class="sxs-lookup"><span data-stu-id="9ea4a-188">As before, you need to provide some additional switches and escape any reserved XML characters when you run the command from MSBuild:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


<span data-ttu-id="9ea4a-189">Наконец, как и раньше, вы можете использовать **Exec** задачу в пользовательский целевой объект MSBuild для выполнения команды:</span><span class="sxs-lookup"><span data-stu-id="9ea4a-189">Finally, as before, you can use the **Exec** task within a custom MSBuild target to execute your command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


<span data-ttu-id="9ea4a-190">При выполнении данного целевого объекта как часть процесса построения, Windows PowerShell будет выполняться сценарий на компьютере, указанном в **– computername** аргумент.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-190">When you execute this target as part of your build process, Windows PowerShell will run your script on the computer you specified in the **–computername** argument.</span></span>

## <a name="conclusion"></a><span data-ttu-id="9ea4a-191">Заключение</span><span class="sxs-lookup"><span data-stu-id="9ea4a-191">Conclusion</span></span>

<span data-ttu-id="9ea4a-192">В этом разделе описано, как запустить сценарий Windows PowerShell из файла проекта MSBuild.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-192">This topic described how to run a Windows PowerShell script from an MSBuild project file.</span></span> <span data-ttu-id="9ea4a-193">Этот подход можно использовать для выполнения сценария Windows PowerShell, локально или на удаленном компьютере, как часть автоматической или один шаг процесса построения и развертывания.</span><span class="sxs-lookup"><span data-stu-id="9ea4a-193">You can use this approach to run a Windows PowerShell script, either locally or on a remote computer, as part of an automated or single-step build and deployment process.</span></span>

## <a name="further-reading"></a><span data-ttu-id="9ea4a-194">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="9ea4a-194">Further Reading</span></span>

<span data-ttu-id="9ea4a-195">Рекомендации о подписи скриптов Windows PowerShell и управление политиками выполнения, см. в разделе [запуск скриптов Windows PowerShell](https://technet.microsoft.com/library/ee176949.aspx).</span><span class="sxs-lookup"><span data-stu-id="9ea4a-195">For guidance on signing Windows PowerShell scripts and managing execution policies, see [Running Windows PowerShell Scripts](https://technet.microsoft.com/library/ee176949.aspx).</span></span> <span data-ttu-id="9ea4a-196">Рекомендации по выполнение команд Windows PowerShell с удаленного компьютера, см. в разделе [выполнение удаленных команд](https://technet.microsoft.com/library/dd819505.aspx).</span><span class="sxs-lookup"><span data-stu-id="9ea4a-196">For guidance on running Windows PowerShell commands from a remote computer, see [Running Remote Commands](https://technet.microsoft.com/library/dd819505.aspx).</span></span>

<span data-ttu-id="9ea4a-197">Дополнительные сведения об использовании настраиваемых файлов проекта MSBuild для управления процессом развертывания, см. в разделе [основные сведения о файле проекта](../web-deployment-in-the-enterprise/understanding-the-project-file.md) и [основные сведения о процессе построения](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="9ea4a-197">For more information on using custom MSBuild project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9ea4a-198">[Назад](taking-web-applications-offline-with-web-deploy.md)
> [Вперед](troubleshooting-the-packaging-process.md)</span><span class="sxs-lookup"><span data-stu-id="9ea4a-198">[Previous](taking-web-applications-offline-with-web-deploy.md)
[Next](troubleshooting-the-packaging-process.md)</span></span>
