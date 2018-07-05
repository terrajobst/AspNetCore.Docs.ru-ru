---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: What резервных, если развертывание | Документация Майкрософт
author: jrjlee
description: В этом разделе описывается выполнение «что если» (или simulated) развертываний с помощью средства веб-развертывания (Web Deploy) Internet Information Services (IIS) и V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: f50053c3b231a1d174df51ce7aa4c6013192bca8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378103"
---
<a name="performing-a-what-if-deployment"></a><span data-ttu-id="dc670-103">Выполнение развертывания «What If»</span><span class="sxs-lookup"><span data-stu-id="dc670-103">Performing a "What If" Deployment</span></span>
====================
<span data-ttu-id="dc670-104">по [Джейсон Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="dc670-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="dc670-105">Загрузить PDF-файл</span><span class="sxs-lookup"><span data-stu-id="dc670-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="dc670-106">В этом разделе описывается выполнение «что если» (или simulated) развертываний с помощью средства веб-развертывания (Web Deploy) Internet Information Services (IIS) и средство VSDBCMD.</span><span class="sxs-lookup"><span data-stu-id="dc670-106">This topic describes how to perform "what if" (or simulated) deployments using the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) and VSDBCMD.</span></span> <span data-ttu-id="dc670-107">Это позволяет определить последствия логики развертывания в конкретной целевой среде до фактического развертывания приложения.</span><span class="sxs-lookup"><span data-stu-id="dc670-107">This lets you determine the effects of your deployment logic on a particular target environment before you actually deploy your application.</span></span>


<span data-ttu-id="dc670-108">Этот раздел является частью серии учебников, исходя из требования к развертыванию enterprise вымышленной компании Fabrikam, Inc. В этой серии руководств используется пример решения&#x2014; [решения диспетчера контактов](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;для представления веб-приложения с более реалистичные уровень сложности, включая приложения ASP.NET MVC 3, Windows Communication Служба Foundation (WCF) и проект базы данных.</span><span class="sxs-lookup"><span data-stu-id="dc670-108">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="dc670-109">Метод развертывания в основе этих учебников основан на разбиение проекта файл подход, описанный в [основные сведения о файле проекта](../web-deployment-in-the-enterprise/understanding-the-project-file.md), в которой процесс построения и развертывания управляется двумя файлы проекта&#x2014;один содержащая инструкции сборки, которые применяются к каждой целевой среде и содержащего параметры построения и развертывания для конкретной среды.</span><span class="sxs-lookup"><span data-stu-id="dc670-109">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build and deployment process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="dc670-110">Во время сборки файл проекта для конкретной среды объединяется в файл проекта независимой от среды, чтобы полный набор инструкций построения.</span><span class="sxs-lookup"><span data-stu-id="dc670-110">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="performing-a-what-if-deployment-for-web-packages"></a><span data-ttu-id="dc670-111">Выполнение развертывания «What If» для веб-пакетов</span><span class="sxs-lookup"><span data-stu-id="dc670-111">Performing a "What If" Deployment for Web Packages</span></span>

<span data-ttu-id="dc670-112">Веб-развертывание включает в себя функции, которая позволяет выполнять развертывание в «что если» (или пробная версия) режиме.</span><span class="sxs-lookup"><span data-stu-id="dc670-112">Web Deploy includes functionality that lets you perform deployments in "what if" (or trial) mode.</span></span> <span data-ttu-id="dc670-113">При развертывании артефактов в режиме «что если», веб-развертывания создает файл журнала, как если бы вы выполнили развертывание, но она ничего не меняет фактически на целевом сервере.</span><span class="sxs-lookup"><span data-stu-id="dc670-113">When you deploy artifacts in "what if" mode, Web Deploy generates a log file as if you had performed the deployment, but it doesn't actually change anything on the destination server.</span></span> <span data-ttu-id="dc670-114">Просмотрев файл журнала может помочь понять, как повлияет развертывания на конечном сервере, в частности:</span><span class="sxs-lookup"><span data-stu-id="dc670-114">Reviewing the log file can help you to understand what impact your deployment will have on the destination server, in particular:</span></span>

- <span data-ttu-id="dc670-115">Будут добавляться новые.</span><span class="sxs-lookup"><span data-stu-id="dc670-115">What will get added.</span></span>
- <span data-ttu-id="dc670-116">Что будет обновляться.</span><span class="sxs-lookup"><span data-stu-id="dc670-116">What will get updated.</span></span>
- <span data-ttu-id="dc670-117">Что будут удалены.</span><span class="sxs-lookup"><span data-stu-id="dc670-117">What will get deleted.</span></span>

<span data-ttu-id="dc670-118">Так как развертывания «what if» ничего не меняет, фактически на конечном сервере, что он не всегда можно сделать прогноз ли развертывание успешно завершится.</span><span class="sxs-lookup"><span data-stu-id="dc670-118">Because a "what if" deployment doesn't actually change anything on the destination server, what it can't always do is predict whether a deployment will succeed.</span></span>

<span data-ttu-id="dc670-119">Как описано в разделе [развертывание веб-пакеты](../web-deployment-in-the-enterprise/deploying-web-packages.md), можно развернуть веб-пакетов с помощью веб-развертывания в двух случаях&#x2014;с помощью программы командной строки MSDeploy.exe, напрямую или путем запуска *. deploy.cmd* файла что процесс построения создает.</span><span class="sxs-lookup"><span data-stu-id="dc670-119">As described in [Deploying Web Packages](../web-deployment-in-the-enterprise/deploying-web-packages.md), you can deploy web packages using Web Deploy in two ways&#x2014;by using the MSDeploy.exe command-line utility directly or by running the *.deploy.cmd* file that the build process generates.</span></span>

<span data-ttu-id="dc670-120">Если вы используете MSDeploy.exe напрямую, можно выполнить развертывание «what if» путем добавления **whatif** флаг в команду.</span><span class="sxs-lookup"><span data-stu-id="dc670-120">If you're using MSDeploy.exe directly, you can run a "what if" deployment by adding the **–whatif** flag to your command.</span></span> <span data-ttu-id="dc670-121">Например чтобы оценить, что произойдет, если пакет был развернут в ContactManager.Mvc.zip в промежуточной среде, MSDeploy команда должна выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="dc670-121">For example, to evaluate what would happen if you deployed the ContactManager.Mvc.zip package to a staging environment, the MSDeploy command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]


<span data-ttu-id="dc670-122">Когда вы будете удовлетворены результатами развертывания «what if», можно удалить **whatif** флаг выполнения динамической развертывания.</span><span class="sxs-lookup"><span data-stu-id="dc670-122">When you're satisfied with the results of your "what if" deployment, you can remove the **–whatif** flag to run a live deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="dc670-123">Дополнительные сведения о параметрах командной строки для MSDeploy.exe, см. в разделе [веб-развертывание параметров операции](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="dc670-123">For more information on command-line options for MSDeploy.exe, see [Web Deploy Operation Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span></span>


<span data-ttu-id="dc670-124">Если вы используете *. deploy.cmd* файл, вы можете запустить развертывания «what if», включая **/t** флаг флаг (режиме пробной версии), а не **/y** флаг («Да» или режим обновления) в команда.</span><span class="sxs-lookup"><span data-stu-id="dc670-124">If you're using the *.deploy.cmd* file, you can run a "what if" deployment by including the **/t** flag (trial mode) flag instead of the **/y** flag ("yes," or update mode) in your command.</span></span> <span data-ttu-id="dc670-125">Например, чтобы оценить, что произойдет, если пакет был развернут в ContactManager.Mvc.zip, выполнив *. deploy.cmd* файл, команда должна выглядеть примерно следующим образом:</span><span class="sxs-lookup"><span data-stu-id="dc670-125">For example, to evaluate what would happen if you deployed the ContactManager.Mvc.zip package by running the *.deploy.cmd* file, your command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]


<span data-ttu-id="dc670-126">Когда вы будете удовлетворены результатами развертывания «в режиме пробной версии», вы можете заменить **/t** флаг с **/y** флаг выполнения динамической развертывания:</span><span class="sxs-lookup"><span data-stu-id="dc670-126">When you're satisfied with the results of your "trial mode" deployment, you can replace the **/t** flag with a **/y** flag to run a live deployment:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="dc670-127">Дополнительные сведения о параметрах командной строки для *. deploy.cmd* файлы, см. в разделе [как: установка, развертывание пакета с помощью файла deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx).</span><span class="sxs-lookup"><span data-stu-id="dc670-127">For more information on command-line options for *.deploy.cmd* files, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span> <span data-ttu-id="dc670-128">При запуске *. deploy.cmd* файл без указания всех флагов, в командной строке отображается список доступных флагов.</span><span class="sxs-lookup"><span data-stu-id="dc670-128">If you run the *.deploy.cmd* file without specifying any flags, the command prompt will display a list of available flags.</span></span>


## <a name="performing-a-what-if-deployment-for-databases"></a><span data-ttu-id="dc670-129">Выполнение развертывания «What If» для баз данных</span><span class="sxs-lookup"><span data-stu-id="dc670-129">Performing a "What If" Deployment for Databases</span></span>

<span data-ttu-id="dc670-130">В этом разделе предполагается, что вы используете служебную программу VSDBCMD для выполнения развертывания добавочный, на основе схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="dc670-130">This section assumes that you're using the VSDBCMD utility to perform incremental, schema-based database deployment.</span></span> <span data-ttu-id="dc670-131">Этот подход описан более подробно в [развертывание проектов баз данных](../web-deployment-in-the-enterprise/deploying-database-projects.md).</span><span class="sxs-lookup"><span data-stu-id="dc670-131">This approach is described in more detail in [Deploying Database Projects](../web-deployment-in-the-enterprise/deploying-database-projects.md).</span></span> <span data-ttu-id="dc670-132">Мы рекомендуем ознакомиться с этим разделом, прежде чем применять описанные здесь принципы.</span><span class="sxs-lookup"><span data-stu-id="dc670-132">We recommend that you familiarize yourself with this topic before you apply the concepts described here.</span></span>

<span data-ttu-id="dc670-133">При использовании VSDBCMD в **развернуть** режиме, можно использовать **/dd** (или **/DeployToDatabase**) флаг, определяющий ли средство VSDBCMD фактически развертывает базы данных или просто приводит к возникновению ошибки скрипт развертывания.</span><span class="sxs-lookup"><span data-stu-id="dc670-133">When you use VSDBCMD in **Deploy** mode, you can use the **/dd** (or **/DeployToDatabase**) flag to control whether VSDBCMD actually deploys the database or just generates a deployment script.</span></span> <span data-ttu-id="dc670-134">Если вы развертываете DBSCHEMA-файл, это поведение:</span><span class="sxs-lookup"><span data-stu-id="dc670-134">If you're deploying a .dbschema file, this is the behavior:</span></span>

- <span data-ttu-id="dc670-135">Если указать **/dd+** или **/dd**, средство VSDBCMD создаст скрипт развертывания и развертывание базы данных.</span><span class="sxs-lookup"><span data-stu-id="dc670-135">If you specify **/dd+** or **/dd**, VSDBCMD will generate a deployment script and deploy the database.</span></span>
- <span data-ttu-id="dc670-136">Если указать **/dd-** или ключ пропущен, средство VSDBCMD создаст только сценарий развертывания.</span><span class="sxs-lookup"><span data-stu-id="dc670-136">If you specify **/dd-** or omit the switch, VSDBCMD will generate a deployment script only.</span></span>

> [!NOTE]
> <span data-ttu-id="dc670-137">Если вы развертываете DEPLOYMANIFEST-файл, а не DBSCHEMA-файл, поведение **/dd** коммутатора гораздо более сложен.</span><span class="sxs-lookup"><span data-stu-id="dc670-137">If you're deploying a .deploymanifest file rather than a .dbschema file, the behavior of the **/dd** switch is a lot more complicated.</span></span> <span data-ttu-id="dc670-138">По сути, средство VSDBCMD будет игнорировать значение **/dd** переключение, если DEPLOYMANIFEST-файл включает в себя **DeployToDatabase** элемент со значением **True**.</span><span class="sxs-lookup"><span data-stu-id="dc670-138">Essentially, VSDBCMD will ignore the value of the **/dd** switch if the .deploymanifest file includes a **DeployToDatabase** element with a value of **True**.</span></span> <span data-ttu-id="dc670-139">[Развертывание проектов базы данных](../web-deployment-in-the-enterprise/deploying-database-projects.md) описывает это поведение в полном объеме.</span><span class="sxs-lookup"><span data-stu-id="dc670-139">[Deploying Database Projects](../web-deployment-in-the-enterprise/deploying-database-projects.md) describes this behavior in full.</span></span>


<span data-ttu-id="dc670-140">Например, чтобы создать скрипт развертывания для **ContactManager** базы данных без фактического развертывания базы данных, средство VSDBCMD команда должна выглядеть примерно следующим образом:</span><span class="sxs-lookup"><span data-stu-id="dc670-140">For example, to generate a deployment script for the **ContactManager** database without actually deploying the database, your VSDBCMD command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]


<span data-ttu-id="dc670-141">Средство VSDBCMD это средство развертывания разностную копию базы данных, и таким образом скрипт развертывания создается динамически для хранения всех команд SQL, необходимые для обновления текущей базы данных, если таковой существует, к указанной схеме.</span><span class="sxs-lookup"><span data-stu-id="dc670-141">VSDBCMD is a differential database deployment tool, and as such the deployment script is dynamically generated to contain all the SQL commands necessary to update the current database, if one exists, to the specified schema.</span></span> <span data-ttu-id="dc670-142">Просмотр скрипта развертывания — удобный способ определить, какое влияние развертывания окажет на текущей базы данных и данных, содержащихся в нем.</span><span class="sxs-lookup"><span data-stu-id="dc670-142">Reviewing the deployment script is a useful way to determine what impact your deployment will have on the current database and the data it contains.</span></span> <span data-ttu-id="dc670-143">Например можно определить:</span><span class="sxs-lookup"><span data-stu-id="dc670-143">For example, you might want to determine:</span></span>

- <span data-ttu-id="dc670-144">Будут ли удалены все имеющиеся таблицы, и это приведет к потере данных.</span><span class="sxs-lookup"><span data-stu-id="dc670-144">Whether any existing tables will be removed, and whether that will result in data loss.</span></span>
- <span data-ttu-id="dc670-145">Ли порядок операций несет риск потери данных, например, если вы разбиения или слияния таблиц.</span><span class="sxs-lookup"><span data-stu-id="dc670-145">Whether the order of operations carries a risk of data loss, for example, if you're splitting or merging tables.</span></span>

<span data-ttu-id="dc670-146">Если вас все устраивает, с помощью сценария развертывания, можно повторить VSDBCMD с **/dd+** флаг для внесения изменений.</span><span class="sxs-lookup"><span data-stu-id="dc670-146">If you're happy with the deployment script, you can repeat the VSDBCMD with a **/dd+** flag to make the changes.</span></span> <span data-ttu-id="dc670-147">Кроме того можно изменить скрипт развертывания в соответствии с потребностями и затем выполнить его вручную на сервере базы данных.</span><span class="sxs-lookup"><span data-stu-id="dc670-147">Alternatively, you can edit the deployment script to meet your requirements and then execute it manually on the database server.</span></span>

## <a name="integrating-what-if-functionality-into-custom-project-files"></a><span data-ttu-id="dc670-148">Интеграция «What If» функциональные возможности в файлах пользовательских проектов</span><span class="sxs-lookup"><span data-stu-id="dc670-148">Integrating "What If" Functionality into Custom Project Files</span></span>

<span data-ttu-id="dc670-149">В более сложных сценариев развертывания, появится желание использовать пользовательский файл проекта Microsoft Build Engine (MSBuild) для инкапсуляции логики сборки и развертывания, как описано в разделе [основные сведения о файле проекта](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span><span class="sxs-lookup"><span data-stu-id="dc670-149">In more complex deployment scenarios, you'll want to use a custom Microsoft Build Engine (MSBuild) project file to encapsulate your build and deployment logic, as described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span></span> <span data-ttu-id="dc670-150">Например, в [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) образец решения, *Publish.proj* файла:</span><span class="sxs-lookup"><span data-stu-id="dc670-150">For example, in the [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) sample solution, the *Publish.proj* file:</span></span>

- <span data-ttu-id="dc670-151">Выполняет сборку решения.</span><span class="sxs-lookup"><span data-stu-id="dc670-151">Builds the solution.</span></span>
- <span data-ttu-id="dc670-152">Использует веб-развертывания для упаковки и развертывания приложения ContactManager.Mvc.</span><span class="sxs-lookup"><span data-stu-id="dc670-152">Uses Web Deploy to package and deploy the ContactManager.Mvc application.</span></span>
- <span data-ttu-id="dc670-153">Использует веб-развертывания для упаковки и развертывания приложения ContactManager.Service.</span><span class="sxs-lookup"><span data-stu-id="dc670-153">Uses Web Deploy to package and deploy the ContactManager.Service application.</span></span>
- <span data-ttu-id="dc670-154">Развертывает **ContactManager** базы данных.</span><span class="sxs-lookup"><span data-stu-id="dc670-154">Deploys the **ContactManager** database.</span></span>

<span data-ttu-id="dc670-155">При интеграции развертывания нескольких веб-пакетов и/или баз данных в один шаг процесс таким образом, вы также можете выполнять все развертывание в режиме «что если».</span><span class="sxs-lookup"><span data-stu-id="dc670-155">When you integrate the deployment of multiple web packages and/or databases into a single-step process in this way, you may also want the option of performing the entire deployment in a "what if" mode.</span></span>

<span data-ttu-id="dc670-156">*Publish.proj* файл демонстрирует, как это можно сделать.</span><span class="sxs-lookup"><span data-stu-id="dc670-156">The *Publish.proj* file demonstrates how you can do this.</span></span> <span data-ttu-id="dc670-157">Во-первых вам потребуется создать свойство для хранения значения «what if».</span><span class="sxs-lookup"><span data-stu-id="dc670-157">First, you need to create a property to store the "what if" value:</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]


<span data-ttu-id="dc670-158">В этом случае вы создали свойство с именем **WhatIf** со значением по умолчанию **false**.</span><span class="sxs-lookup"><span data-stu-id="dc670-158">In this case, you've created a property named **WhatIf** with a default value of **false**.</span></span> <span data-ttu-id="dc670-159">Пользователи могут переопределить это значение свойства значение **true** в параметре командной строки, как вы вскоре увидите.</span><span class="sxs-lookup"><span data-stu-id="dc670-159">Users can override this value by setting the property to **true** in a command-line parameter, as you'll see shortly.</span></span>

<span data-ttu-id="dc670-160">Следующий этап заключается в параметризовать любой веб-развертывания и средство VSDBCMD команды, чтобы отразить флаги **WhatIf** значение свойства.</span><span class="sxs-lookup"><span data-stu-id="dc670-160">The next stage is to parameterize any Web Deploy and VSDBCMD commands so that the flags reflect the **WhatIf** property value.</span></span> <span data-ttu-id="dc670-161">Например, следующий целевой объект (из *Publish.proj* файл и упрощенное) выполняется *. deploy.cmd* файла развернуть веб-пакет.</span><span class="sxs-lookup"><span data-stu-id="dc670-161">For example, the next target (taken from the *Publish.proj* file and simplified) runs the *.deploy.cmd* file to deploy a web package.</span></span> <span data-ttu-id="dc670-162">По умолчанию, команда включает **/Y** switch («Да» или режим обновления).</span><span class="sxs-lookup"><span data-stu-id="dc670-162">By default, the command includes a **/Y** switch ("yes," or update mode).</span></span> <span data-ttu-id="dc670-163">Если **WhatIf** присваивается **true**, оно заменяется **/T** switch (пробная версия или «что если» режим).</span><span class="sxs-lookup"><span data-stu-id="dc670-163">If **WhatIf** is set to **true**, this is replaced by a **/T** switch (trial, or "what if" mode).</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]


<span data-ttu-id="dc670-164">Аналогичным образом следующий целевой объект использует служебную программу VSDBCMD развертывание базы данных.</span><span class="sxs-lookup"><span data-stu-id="dc670-164">Similarly, the next target uses the VSDBCMD utility to deploy a database.</span></span> <span data-ttu-id="dc670-165">По умолчанию **/dd** ключ не включен.</span><span class="sxs-lookup"><span data-stu-id="dc670-165">By default, a **/dd** switch is not included.</span></span> <span data-ttu-id="dc670-166">Это означает, что VSDBCMD создаст скрипт развертывания, но не выполняет развертывание базы данных&#x2014;другими словами, «что, если» сценарий.</span><span class="sxs-lookup"><span data-stu-id="dc670-166">This means that VSDBCMD will generate a deployment script but will not deploy the database&#x2014;in other words, a "what if" scenario.</span></span> <span data-ttu-id="dc670-167">Если **WhatIf** не задано значение **true**, **/dd** добавлен переключатель и средство VSDBCMD развертываются в базе данных.</span><span class="sxs-lookup"><span data-stu-id="dc670-167">If the **WhatIf** property is not set to **true**, a **/dd** switch is added and VSDBCMD will deploy the database.</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]


<span data-ttu-id="dc670-168">Можно использовать тот же подход можно параметризовать все используемые команды в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="dc670-168">You can use the same approach to parameterize all the relevant commands in your project file.</span></span> <span data-ttu-id="dc670-169">Если вы хотите выполнить развертывание «what if», затем можно просто предоставить **WhatIf** значение свойства из командной строки:</span><span class="sxs-lookup"><span data-stu-id="dc670-169">When you want to run a "what if" deployment, you can then simply provide a **WhatIf** property value from the command line:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]


<span data-ttu-id="dc670-170">Таким образом можно запустить развертывания «what if» для всех компонентов проекта за один шаг.</span><span class="sxs-lookup"><span data-stu-id="dc670-170">In this way, you can run a "what if" deployment for all your project components in a single step.</span></span>

## <a name="conclusion"></a><span data-ttu-id="dc670-171">Заключение</span><span class="sxs-lookup"><span data-stu-id="dc670-171">Conclusion</span></span>

<span data-ttu-id="dc670-172">В этом разделе описано, как запускать «what if» развертывание с помощью Web Deploy, средство VSDBCMD и MSBuild.</span><span class="sxs-lookup"><span data-stu-id="dc670-172">This topic described how to run "what if" deployments using Web Deploy, VSDBCMD, and MSBuild.</span></span> <span data-ttu-id="dc670-173">Развертывание «what if» позволяет оценить влияние предложенное развертывания, прежде чем фактически вносить любые изменения в целевой среде.</span><span class="sxs-lookup"><span data-stu-id="dc670-173">A "what if" deployment lets you evaluate the impact of a proposed deployment before you actually make any changes to the destination environment.</span></span>

## <a name="further-reading"></a><span data-ttu-id="dc670-174">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="dc670-174">Further Reading</span></span>

<span data-ttu-id="dc670-175">Дополнительные сведения о синтаксисе командной строки на веб-развертывания, см. в разделе [веб-развертывание параметров операции](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="dc670-175">For more information on Web Deploy command-line syntax, see [Web Deploy Operation Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span></span> <span data-ttu-id="dc670-176">Руководство по параметрам командной строки, при использовании *. deploy.cmd* файл, см. в разделе [как: установка, развертывание пакета с помощью файла deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx).</span><span class="sxs-lookup"><span data-stu-id="dc670-176">For guidance on command-line options when you use the *.deploy.cmd* file, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span> <span data-ttu-id="dc670-177">Рекомендации о синтаксисе командной строки VSDBCMD, см. в разделе [Справочник по командной строки VSDBCMD. EXE (развертывание и импорт схемы)](https://msdn.microsoft.com/library/dd193283.aspx).</span><span class="sxs-lookup"><span data-stu-id="dc670-177">For guidance on VSDBCMD command-line syntax, see [Command-Line Reference for VSDBCMD.EXE (Deployment and Schema Import)](https://msdn.microsoft.com/library/dd193283.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="dc670-178">[Назад](advanced-enterprise-web-deployment.md)
> [Вперед](customizing-database-deployments-for-multiple-environments.md)</span><span class="sxs-lookup"><span data-stu-id="dc670-178">[Previous](advanced-enterprise-web-deployment.md)
[Next](customizing-database-deployments-for-multiple-environments.md)</span></span>
