---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: Устранение неполадок в процессе упаковки | Документы Microsoft
author: jrjlee
description: В этом разделе описывается, как собирать подробные сведения о процессе обработки пакетов с помощью свойства EnablePackageProcessLoggingAndAssert м...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 816ab77c44b52c6449a139475f2ef8546bd38071
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892690"
---
<a name="troubleshooting-the-packaging-process"></a><span data-ttu-id="57676-103">Устранение неполадок в процессе упаковки</span><span class="sxs-lookup"><span data-stu-id="57676-103">Troubleshooting the Packaging Process</span></span>
====================
<span data-ttu-id="57676-104">по [Джейсон Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="57676-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="57676-105">Скачать PDF</span><span class="sxs-lookup"><span data-stu-id="57676-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="57676-106">В этом разделе описывается, как собирать подробные сведения о процессе обработки пакетов с помощью **EnablePackageProcessLoggingAndAssert** свойства в Microsoft Build Engine (MSBuild).</span><span class="sxs-lookup"><span data-stu-id="57676-106">This topic describes how you can collect detailed information about the packaging process by using the **EnablePackageProcessLoggingAndAssert** property in the Microsoft Build Engine (MSBuild).</span></span>
> 
> <span data-ttu-id="57676-107">При задании **EnablePackageProcessLoggingAndAssert** свойства **true**, будет MSBuild:</span><span class="sxs-lookup"><span data-stu-id="57676-107">When you set the **EnablePackageProcessLoggingAndAssert** property to **true**, MSBuild will:</span></span>
> 
> - <span data-ttu-id="57676-108">Добавление дополнительных сведений о процессе упаковки журналов построения.</span><span class="sxs-lookup"><span data-stu-id="57676-108">Add additional information about the packaging process to the build logs.</span></span>
> - <span data-ttu-id="57676-109">Например, журнал ошибок при определенных условиях, при обнаружении повторяющихся файлов в списке пакетов.</span><span class="sxs-lookup"><span data-stu-id="57676-109">Log errors under certain conditions, for example, if duplicate files are found in the packaging list.</span></span>
> - <span data-ttu-id="57676-110">Создайте каталог журнала в *ProjectName*\_пакет папки и использовать его для записи сведений об упаковке файлов.</span><span class="sxs-lookup"><span data-stu-id="57676-110">Create a Log directory in the *ProjectName*\_Package folder and use it to record information about the files you're packaging.</span></span>
> 
> <span data-ttu-id="57676-111">Если вашего веб-развертывания пакеты не содержат файлы, которые предполагается, что произошел сбой обработки пакетов, можно использовать эти сведения для устранения неполадок процесса и ориентира где дела идут неправильный.</span><span class="sxs-lookup"><span data-stu-id="57676-111">If the packaging process is failing, or your web deployment packages don't contain the files that you expect, you can use this information to troubleshoot the process and pinpoint where things are going wrong.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="57676-112">**EnablePackageProcessLoggingAndAssert** свойство работает только при сборке проекта с помощью **отладки** конфигурации.</span><span class="sxs-lookup"><span data-stu-id="57676-112">The **EnablePackageProcessLoggingAndAssert** property only works if you build your project using the **Debug** configuration.</span></span> <span data-ttu-id="57676-113">Свойство учитывается в других конфигурациях.</span><span class="sxs-lookup"><span data-stu-id="57676-113">The property is ignored in other configurations.</span></span>


<span data-ttu-id="57676-114">Этот раздел входит в состав серию учебников, исходя из требования к развертыванию enterprise вымышленная компания Fabrikam, Inc. Этот учебник ряд использует образец решения&#x2014; [решения диспетчера контактов](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;для представления веб-приложения с реалистичных уровень сложности, включая приложения ASP.NET MVC 3, Windows Communication Служба Foundation (WCF) и проект базы данных.</span><span class="sxs-lookup"><span data-stu-id="57676-114">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="57676-115">Метод развертывания, в основе этих учебников основан на разбиение проекта файл подход, описанный в [основные сведения о файле проекта](../web-deployment-in-the-enterprise/understanding-the-project-file.md), в которой процесс построения управляется двух файлов проекта&#x2014;один с построение инструкции, которые применяются для каждой целевой среде и, содержащий параметры построения и развертывания конкретной среды.</span><span class="sxs-lookup"><span data-stu-id="57676-115">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="57676-116">Во время построения файла проекта среды объединяется в файл проекта зависит от среды, образуют полный набор инструкций построения.</span><span class="sxs-lookup"><span data-stu-id="57676-116">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a><span data-ttu-id="57676-117">Основные сведения о свойстве EnablePackageProcessLoggingAndAssert</span><span class="sxs-lookup"><span data-stu-id="57676-117">Understanding the EnablePackageProcessLoggingAndAssert Property</span></span>

<span data-ttu-id="57676-118">[Построение и упаковки проектов веб-приложений](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) описано, как конвейер публикации Web (WPP) предоставляет набор целевых объектов MSBuild, расширяющие функциональные возможности MSBuild и включить его для интеграции веб-служб Internet Information Services (IIS) Средства развертывания (Web Deploy).</span><span class="sxs-lookup"><span data-stu-id="57676-118">[Building and Packaging Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) described how the Web Publishing Pipeline (WPP) provides a set of MSBuild targets that extend the functionality of MSBuild and enable it to integrate with the Internet Information Services (IIS) Web Deployment Tool (Web Deploy).</span></span> <span data-ttu-id="57676-119">При создании пакета в проект веб-приложения, вызов WPP целевых объектов.</span><span class="sxs-lookup"><span data-stu-id="57676-119">When you package a web application project, you're invoking WPP targets.</span></span>

<span data-ttu-id="57676-120">Большое количество этих целевых объектов WPP включать условную логику, осуществляющий запись в журнал дополнительные при **EnablePackageProcessLoggingAndAssert** свойству **true**.</span><span class="sxs-lookup"><span data-stu-id="57676-120">Lots of these WPP targets include conditional logic that logs additional information when the **EnablePackageProcessLoggingAndAssert** property is set to **true**.</span></span> <span data-ttu-id="57676-121">Например, при просмотре **пакета** целевым объектом, можно увидеть, он создает каталог журнала и выводит список файлов в текстовый файл, если **EnablePackageProcessLoggingAndAssert** равен **true**.</span><span class="sxs-lookup"><span data-stu-id="57676-121">For example, if you review the **Package** target, you can see that it creates an additional log directory and writes a list of files to a text file if **EnablePackageProcessLoggingAndAssert** is equal to **true**.</span></span>


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]


> [!NOTE]
> <span data-ttu-id="57676-122">Целевые объекты WPP определяются в *Microsoft.Web.Publishing.targets* файл в папке % PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web.</span><span class="sxs-lookup"><span data-stu-id="57676-122">The WPP targets are defined in the *Microsoft.Web.Publishing.targets* file in the %PROGRAMFILES(x86)%\MSBuild\Microsoft\VisualStudio\v10.0\Web folder.</span></span> <span data-ttu-id="57676-123">Можно открыть этот файл и просмотреть целевые объекты в Visual Studio 2010 или редакторе XML.</span><span class="sxs-lookup"><span data-stu-id="57676-123">You can open this file and review the targets in Visual Studio 2010 or any XML editor.</span></span> <span data-ttu-id="57676-124">Следует изменить содержимое файла.</span><span class="sxs-lookup"><span data-stu-id="57676-124">Take care not to modify the contents of the file.</span></span>


## <a name="enabling-the-additional-logging"></a><span data-ttu-id="57676-125">Включение дополнительных ведения журнала</span><span class="sxs-lookup"><span data-stu-id="57676-125">Enabling the Additional Logging</span></span>

<span data-ttu-id="57676-126">Можно задать значение для **EnablePackageProcessLoggingAndAssert** свойство различными способами, в зависимости от способа создания проекта.</span><span class="sxs-lookup"><span data-stu-id="57676-126">You can supply a value for the **EnablePackageProcessLoggingAndAssert** property in various ways, depending on how you build your project.</span></span>

<span data-ttu-id="57676-127">При создании проекта из командной строки, можно задать значение для **EnablePackageProcessLoggingAndAssert** свойство в качестве аргумента командной строки:</span><span class="sxs-lookup"><span data-stu-id="57676-127">If you build your project from the command line, you can supply a value for the **EnablePackageProcessLoggingAndAssert** property as a command-line argument:</span></span>


[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]


<span data-ttu-id="57676-128">Если вы используете пользовательском файле проекта для построения проектов, можно включить **EnablePackageProcessLoggingAndAssert** значение в **свойства** атрибут **MSBuild**задачи:</span><span class="sxs-lookup"><span data-stu-id="57676-128">If you're using a custom project file to build your projects, you can include the **EnablePackageProcessLoggingAndAssert** value in the **Properties** attribute of the **MSBuild** task:</span></span>


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]


<span data-ttu-id="57676-129">Если вы используете определение сборки Team Foundation Server (TFS) для построения проектов, можно задать значение для **EnablePackageProcessLoggingAndAssert** свойство в **Аргументы MSBuild** строки:![](troubleshooting-the-packaging-process/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="57676-129">If you're using a Team Foundation Server (TFS) build definition to build your projects, you can supply a value for the **EnablePackageProcessLoggingAndAssert** property in the **MSBuild Arguments** row:![](troubleshooting-the-packaging-process/_static/image1.png)</span></span>

> [!NOTE]
> <span data-ttu-id="57676-130">Дополнительные сведения о создании и настройке определений сборки см. в разделе [Создание сборки определение, поддерживает развертывания](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="57676-130">For more information on creating and configuring build definitions, see [Creating a Build Definition That Supports Deployment](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).</span></span>


<span data-ttu-id="57676-131">Кроме того, если вы хотите включить пакета в каждой сборки, можно изменить файл проекта для проекта веб-приложения задать **EnablePackageProcessLoggingAndAssert** свойства **true**.</span><span class="sxs-lookup"><span data-stu-id="57676-131">Alternatively, if you want to include the package in every build, you can modify the project file for your web application project to set the **EnablePackageProcessLoggingAndAssert** property to **true**.</span></span> <span data-ttu-id="57676-132">Свойства следует добавить к первому **PropertyGroup** элемент в файле с расширением CSPROJ или VBPROJ-файлу.</span><span class="sxs-lookup"><span data-stu-id="57676-132">You should add the property to the first **PropertyGroup** element within your .csproj or .vbproj file.</span></span>


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]


## <a name="reviewing-the-log-files"></a><span data-ttu-id="57676-133">Просмотр файлов журналов</span><span class="sxs-lookup"><span data-stu-id="57676-133">Reviewing the Log Files</span></span>

<span data-ttu-id="57676-134">После построения и упаковки проекта веб-приложения с **EnablePackageProcessLoggingAndAssert** значение **true**, MSBuild создает дополнительную папку с именем входа *ProjectName* \_Папки пакета.</span><span class="sxs-lookup"><span data-stu-id="57676-134">When you build and package a web application project with **EnablePackageProcessLoggingAndAssert** set to **true**, MSBuild creates an additional folder named Log in the *ProjectName*\_Package folder.</span></span> <span data-ttu-id="57676-135">Папка журнала содержит различные файлы.</span><span class="sxs-lookup"><span data-stu-id="57676-135">The Log folder contains various files:</span></span>

![](troubleshooting-the-packaging-process/_static/image2.png)

<span data-ttu-id="57676-136">Список файлов, которые вы видите будут различаться в зависимости от действий в проекте и процесс сборки.</span><span class="sxs-lookup"><span data-stu-id="57676-136">The list of files that you see will vary according to the things in your project and your build process.</span></span> <span data-ttu-id="57676-137">Тем не менее эти файлы обычно используются для записи списка файлов, которые собирает WPP пакетов на различных этапах процесса:</span><span class="sxs-lookup"><span data-stu-id="57676-137">However, these files are typically used to record the list of files that the WPP is collecting for packaging, at various stages of the process:</span></span>

- <span data-ttu-id="57676-138">*PreExcludePipelineCollectFilesPhaseFileList.txt* файла список файлов, которые собирает MSBuild упаковки, прежде чем будут удалены все файлы, указанные для исключения.</span><span class="sxs-lookup"><span data-stu-id="57676-138">The *PreExcludePipelineCollectFilesPhaseFileList.txt* file lists the files that MSBuild collects for packaging before any files that are specified for exclusion are removed.</span></span>
- <span data-ttu-id="57676-139">*AfterExcludeFilesFilesList.txt* файл содержит список измененных файлов после удаляются все файлы, указанные для исключения.</span><span class="sxs-lookup"><span data-stu-id="57676-139">The *AfterExcludeFilesFilesList.txt* file contains the modified file list after any files that are specified for exclusion are removed.</span></span>

    > [!NOTE]
    > <span data-ttu-id="57676-140">Дополнительные сведения об исключении файлов и папок из обработки пакетов см. в разделе [за исключением файлов и папок из развертывания](excluding-files-and-folders-from-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="57676-140">For more information on excluding files and folders from the packaging process, see [Excluding Files and Folders from Deployment](excluding-files-and-folders-from-deployment.md).</span></span>
- <span data-ttu-id="57676-141">*AfterTransformWebConfig.txt* файла список файлов, собираемых для упаковки после любого *Web.config* преобразования были выполнены.</span><span class="sxs-lookup"><span data-stu-id="57676-141">The *AfterTransformWebConfig.txt* file lists the files collected for packaging after any *Web.config* transforms have been performed.</span></span> <span data-ttu-id="57676-142">В этом списке, все зависящие от конфигурации *Web.config* преобразования файлов, таких как *Web.Debug.config* и *Web.Release.config*, исключаются из списка файлов для Упаковка.</span><span class="sxs-lookup"><span data-stu-id="57676-142">In this list, any configuration-specific *Web.config* transform files, like *Web.Debug.config* and *Web.Release.config*, are excluded from the list of files for packaging.</span></span> <span data-ttu-id="57676-143">Преобразовать один *Web.config* включается на их место.</span><span class="sxs-lookup"><span data-stu-id="57676-143">A single transformed *Web.config* is included in their place.</span></span>
- <span data-ttu-id="57676-144">*PostAutoParameterizationWebConfigConnectionStrings.txt* файл содержит список файлов после строки подключения в *Web.config* файла были параметризованы.</span><span class="sxs-lookup"><span data-stu-id="57676-144">The *PostAutoParameterizationWebConfigConnectionStrings.txt* file contains the list of files after the connection strings in the *Web.config* file have been parameterized.</span></span> <span data-ttu-id="57676-145">Это процесс, который позволяет заменить строки подключения нужные параметры для целевой среды при развертывании пакета.</span><span class="sxs-lookup"><span data-stu-id="57676-145">This is the process that lets you replace your connection strings with the right settings for your target environment when you deploy the package.</span></span>
- <span data-ttu-id="57676-146">*Prepackage.txt* -файл содержит окончательные перед построением список файлов, которые будут включены в пакет.</span><span class="sxs-lookup"><span data-stu-id="57676-146">The *Prepackage.txt* file contains the finalized pre-build list of files to be included in the package.</span></span>

> [!NOTE]
> <span data-ttu-id="57676-147">Имена дополнительных файлов журналов обычно соответствуют WPP целевых объектов.</span><span class="sxs-lookup"><span data-stu-id="57676-147">The names of the additional log files typically correspond to WPP targets.</span></span> <span data-ttu-id="57676-148">Эти целевые объекты можно просмотреть с помощью проверки *Microsoft.Web.Publishing.targets* файл в папке % PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web.</span><span class="sxs-lookup"><span data-stu-id="57676-148">You can review these targets by examining the *Microsoft.Web.Publishing.targets* file in the %PROGRAMFILES(x86)%\MSBuild\Microsoft\VisualStudio\v10.0\Web folder.</span></span>


<span data-ttu-id="57676-149">Если содержимое веб-пакет не соответствуют ожиданию, просмотр этих файлов может быть удобным способом для определения, на какой точке в процесс действия пошло не так.</span><span class="sxs-lookup"><span data-stu-id="57676-149">If the contents of your web package aren't what you expected, reviewing these files can be a useful way to identify at what point in the process things went wrong.</span></span>

## <a name="conclusion"></a><span data-ttu-id="57676-150">Заключение</span><span class="sxs-lookup"><span data-stu-id="57676-150">Conclusion</span></span>

<span data-ttu-id="57676-151">В этом разделе описано, как использовать **EnablePackageProcessLoggingAndAssert** свойств в MSBuild для устранения неполадок в процессе обработки пакетов.</span><span class="sxs-lookup"><span data-stu-id="57676-151">This topic described how you can use the **EnablePackageProcessLoggingAndAssert** property in MSBuild to troubleshoot the packaging process.</span></span> <span data-ttu-id="57676-152">Оно описано различными способами, в котором можно указать значение свойства для процесса сборки и ее описанных дополнительную информацию, которая регистрируется, если значение свойства **true**.</span><span class="sxs-lookup"><span data-stu-id="57676-152">It explained the different ways in which you can supply the property value to the build process, and it described the additional information that is recorded when you set the property to **true**.</span></span>

## <a name="further-reading"></a><span data-ttu-id="57676-153">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="57676-153">Further Reading</span></span>

<span data-ttu-id="57676-154">Дополнительные сведения об использовании пользовательские файлы проекта MSBuild для управления процессом развертывания см. в разделе [основные сведения о файле проекта](../web-deployment-in-the-enterprise/understanding-the-project-file.md) и [основные сведения о процессе построения](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="57676-154">For more information on using custom MSBuild project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span> <span data-ttu-id="57676-155">Дополнительные сведения о WPP и управлении его упаковку в разделе [построения и упаковки проектов веб-приложений](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).</span><span class="sxs-lookup"><span data-stu-id="57676-155">For more information on the WPP and how it manages the packaging process, see [Building and Packaging Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).</span></span> <span data-ttu-id="57676-156">Рекомендации о том, как исключить определенные файлы и папки из веб-развертывания пакетов см. в разделе [за исключением файлов и папок из развертывания](excluding-files-and-folders-from-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="57676-156">For guidance on how to exclude specific files and folders from web deployment packages, see [Excluding Files and Folders from Deployment](excluding-files-and-folders-from-deployment.md).</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="57676-157">Назад</span><span class="sxs-lookup"><span data-stu-id="57676-157">Previous</span></span>](running-windows-powershell-scripts-from-msbuild-project-files.md)
