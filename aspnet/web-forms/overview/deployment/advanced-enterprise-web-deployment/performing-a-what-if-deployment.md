---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: Выполнение то, что если развертывание | Документы Microsoft
author: jrjlee
description: В этом разделе описываются способы решения «что если» (или имитируемые) развертываний с помощью инструмента веб-развертывания (Web Deploy) Internet Information Services (IIS) и V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: c1a13f38c8e629bcd615190b00104109e25fb289
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879989"
---
<a name="performing-a-what-if-deployment"></a><span data-ttu-id="5ad1a-103">Для выполнения развертывания «Что, если»</span><span class="sxs-lookup"><span data-stu-id="5ad1a-103">Performing a "What If" Deployment</span></span>
====================
<span data-ttu-id="5ad1a-104">по [Джейсон Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="5ad1a-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="5ad1a-105">Скачать PDF</span><span class="sxs-lookup"><span data-stu-id="5ad1a-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="5ad1a-106">В этом разделе описывается выполнение «что, если» (или имитируемые) развертываний с помощью инструмента веб-развертывания (Web Deploy) Internet Information Services (IIS) и VSDBCMD.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-106">This topic describes how to perform "what if" (or simulated) deployments using the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) and VSDBCMD.</span></span> <span data-ttu-id="5ad1a-107">Это позволяет определить последствия логику развертывания в среде с конкретной цели, до фактического развертывания приложения.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-107">This lets you determine the effects of your deployment logic on a particular target environment before you actually deploy your application.</span></span>


<span data-ttu-id="5ad1a-108">Этот раздел входит в состав серию учебников, исходя из требования к развертыванию enterprise вымышленная компания Fabrikam, Inc. Этот учебник ряд использует образец решения&#x2014; [решения диспетчера контактов](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;для представления веб-приложения с реалистичных уровень сложности, включая приложения ASP.NET MVC 3, Windows Communication Служба Foundation (WCF) и проект базы данных.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-108">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="5ad1a-109">Метод развертывания, в основе этих учебников основан на разбиение проекта файл подход, описанный в [основные сведения о файле проекта](../web-deployment-in-the-enterprise/understanding-the-project-file.md), в которой процесс построения и развертывания управляется двух файлов проекта&#x2014;один содержащий инструкции сборки, которые применяются для каждой целевой среде и, содержащий параметры построения и развертывания конкретной среды.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-109">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build and deployment process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="5ad1a-110">Во время построения файла проекта среды объединяется в файл проекта зависит от среды, образуют полный набор инструкций построения.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-110">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="performing-a-what-if-deployment-for-web-packages"></a><span data-ttu-id="5ad1a-111">Выполнение развертывания «Что, если» для веб-пакеты</span><span class="sxs-lookup"><span data-stu-id="5ad1a-111">Performing a "What If" Deployment for Web Packages</span></span>

<span data-ttu-id="5ad1a-112">Веб-развертывания включает функциональность, которая позволяет выполнять развертывание в «что, если» (или пробной версии) режиме.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-112">Web Deploy includes functionality that lets you perform deployments in "what if" (or trial) mode.</span></span> <span data-ttu-id="5ad1a-113">При развертывании артефактов в режиме «что, если» веб-развертывания создает файл журнала, как если бы вы выполнили развертывание, но он не фактически вносит изменения на сервере назначения.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-113">When you deploy artifacts in "what if" mode, Web Deploy generates a log file as if you had performed the deployment, but it doesn't actually change anything on the destination server.</span></span> <span data-ttu-id="5ad1a-114">Просмотр файла журнала может помочь понять, как повлияет развертывания на конечном сервере, в частности:</span><span class="sxs-lookup"><span data-stu-id="5ad1a-114">Reviewing the log file can help you to understand what impact your deployment will have on the destination server, in particular:</span></span>

- <span data-ttu-id="5ad1a-115">Что будет добавятся.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-115">What will get added.</span></span>
- <span data-ttu-id="5ad1a-116">Что будет обновляться.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-116">What will get updated.</span></span>
- <span data-ttu-id="5ad1a-117">Что будут удалены.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-117">What will get deleted.</span></span>

<span data-ttu-id="5ad1a-118">Из-за развертывания «что, если» не фактически вносит изменения на сервере назначения, что он не всегда можно сделать прогноз, является ли развертывание успешно завершится.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-118">Because a "what if" deployment doesn't actually change anything on the destination server, what it can't always do is predict whether a deployment will succeed.</span></span>

<span data-ttu-id="5ad1a-119">Как описано в [веб-развертывание пакетов](../web-deployment-in-the-enterprise/deploying-web-packages.md), можно развернуть веб-пакетов с помощью веб-развертывания двумя способами&#x2014;с помощью программы командной строки MSDeploy.exe напрямую или путем запуска *. deploy.cmd* файла что процесс построения создает.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-119">As described in [Deploying Web Packages](../web-deployment-in-the-enterprise/deploying-web-packages.md), you can deploy web packages using Web Deploy in two ways&#x2014;by using the MSDeploy.exe command-line utility directly or by running the *.deploy.cmd* file that the build process generates.</span></span>

<span data-ttu-id="5ad1a-120">Если вы используете MSDeploy.exe напрямую, можно выполнить развертывание «что, если» путем добавления **– whatif** флаг в команду.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-120">If you're using MSDeploy.exe directly, you can run a "what if" deployment by adding the **–whatif** flag to your command.</span></span> <span data-ttu-id="5ad1a-121">Например чтобы оценить, что может произойти, если пакет был развернут на ContactManager.Mvc.zip в промежуточной среде, MSDeploy команда должна выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="5ad1a-121">For example, to evaluate what would happen if you deployed the ContactManager.Mvc.zip package to a staging environment, the MSDeploy command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]


<span data-ttu-id="5ad1a-122">Если вы удовлетворены результатами развертывания «что, если», можно удалить **– whatif** флаг выполнения динамической развертывания.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-122">When you're satisfied with the results of your "what if" deployment, you can remove the **–whatif** flag to run a live deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="5ad1a-123">Дополнительные сведения о параметрах командной строки для MSDeploy.exe см. в разделе [веб-развертывание параметров операции](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="5ad1a-123">For more information on command-line options for MSDeploy.exe, see [Web Deploy Operation Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span></span>


<span data-ttu-id="5ad1a-124">Если вы используете *. deploy.cmd* файл, можно запустить развертывания «что, если», включая **/t** флаг флаг (пробный режим), а не **/y** флаг («Да» или режим обновления) в в команду.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-124">If you're using the *.deploy.cmd* file, you can run a "what if" deployment by including the **/t** flag (trial mode) flag instead of the **/y** flag ("yes," or update mode) in your command.</span></span> <span data-ttu-id="5ad1a-125">Например, чтобы оценить, что может произойти, если пакет был развернут на ContactManager.Mvc.zip, запустив *. deploy.cmd* файл, команда должна выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="5ad1a-125">For example, to evaluate what would happen if you deployed the ContactManager.Mvc.zip package by running the *.deploy.cmd* file, your command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]


<span data-ttu-id="5ad1a-126">Если вы удовлетворены результатами развертывания «в режиме пробной версии», можно заменить **/t** флаг с **/y** флаг выполнения динамической развертывания:</span><span class="sxs-lookup"><span data-stu-id="5ad1a-126">When you're satisfied with the results of your "trial mode" deployment, you can replace the **/t** flag with a **/y** flag to run a live deployment:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="5ad1a-127">Дополнительные сведения о параметрах командной строки для *. deploy.cmd* файлов, в разделе [как: установка, развертывание пакета с помощью deploy.cmd файл](https://msdn.microsoft.com/library/ff356104.aspx).</span><span class="sxs-lookup"><span data-stu-id="5ad1a-127">For more information on command-line options for *.deploy.cmd* files, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span> <span data-ttu-id="5ad1a-128">При запуске *. deploy.cmd* файл без указания всевозможные флаги, отображает список доступных флагов командной строки.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-128">If you run the *.deploy.cmd* file without specifying any flags, the command prompt will display a list of available flags.</span></span>


## <a name="performing-a-what-if-deployment-for-databases"></a><span data-ttu-id="5ad1a-129">Выполнение развертывания «Что, если» для баз данных</span><span class="sxs-lookup"><span data-stu-id="5ad1a-129">Performing a "What If" Deployment for Databases</span></span>

<span data-ttu-id="5ad1a-130">В этом разделе предполагается, что вы используете программу VSDBCMD выполнить развертывание добавочных, на основе схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-130">This section assumes that you're using the VSDBCMD utility to perform incremental, schema-based database deployment.</span></span> <span data-ttu-id="5ad1a-131">Этот подход является более подробно в [развертывание проектов базы данных](../web-deployment-in-the-enterprise/deploying-database-projects.md).</span><span class="sxs-lookup"><span data-stu-id="5ad1a-131">This approach is described in more detail in [Deploying Database Projects](../web-deployment-in-the-enterprise/deploying-database-projects.md).</span></span> <span data-ttu-id="5ad1a-132">Рекомендуется ознакомиться с этим разделом перед применением описанные здесь принципы.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-132">We recommend that you familiarize yourself with this topic before you apply the concepts described here.</span></span>

<span data-ttu-id="5ad1a-133">При использовании VSDBCMD в **развернуть** режиме, можно использовать **/dd** (или **/DeployToDatabase**) флаг управления VSDBCMD фактически выполняет развертывание базы данных или просто приводит к возникновению ошибки скрипт развертывания.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-133">When you use VSDBCMD in **Deploy** mode, you can use the **/dd** (or **/DeployToDatabase**) flag to control whether VSDBCMD actually deploys the database or just generates a deployment script.</span></span> <span data-ttu-id="5ad1a-134">Если вы развертываете DBSCHEMA-файл, это поведение:</span><span class="sxs-lookup"><span data-stu-id="5ad1a-134">If you're deploying a .dbschema file, this is the behavior:</span></span>

- <span data-ttu-id="5ad1a-135">При указании **/dd+** или **/dd**, VSDBCMD создаст скрипт развертывания и развертывания базы данных.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-135">If you specify **/dd+** or **/dd**, VSDBCMD will generate a deployment script and deploy the database.</span></span>
- <span data-ttu-id="5ad1a-136">При указании **/dd-** или ключ пропущен, VSDBCMD создаст только скрипт развертывания.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-136">If you specify **/dd-** or omit the switch, VSDBCMD will generate a deployment script only.</span></span>

> [!NOTE]
> <span data-ttu-id="5ad1a-137">Если вы развертываете DEPLOYMANIFEST-файл, а не файл DBSCHEMA поведение **/dd** коммутатора гораздо более сложен.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-137">If you're deploying a .deploymanifest file rather than a .dbschema file, the behavior of the **/dd** switch is a lot more complicated.</span></span> <span data-ttu-id="5ad1a-138">По существу, VSDBCMD будет игнорировать значение **/dd** перейти при DEPLOYMANIFEST-файл включает **DeployToDatabase** элемент со значением **True**.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-138">Essentially, VSDBCMD will ignore the value of the **/dd** switch if the .deploymanifest file includes a **DeployToDatabase** element with a value of **True**.</span></span> <span data-ttu-id="5ad1a-139">[Развертывание проектов базы данных](../web-deployment-in-the-enterprise/deploying-database-projects.md) описывает это поведение в полном объеме.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-139">[Deploying Database Projects](../web-deployment-in-the-enterprise/deploying-database-projects.md) describes this behavior in full.</span></span>


<span data-ttu-id="5ad1a-140">Например, чтобы создать скрипт развертывания для **ContactManager** базы данных без фактического развертывания базы данных в команду VSDBCMD должен выглядеть примерно следующим образом:</span><span class="sxs-lookup"><span data-stu-id="5ad1a-140">For example, to generate a deployment script for the **ContactManager** database without actually deploying the database, your VSDBCMD command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]


<span data-ttu-id="5ad1a-141">VSDBCMD это средство развертывания разностную копию базы данных, и таким образом динамически создается скрипт развертывания для хранения всех команд SQL, необходимые для обновления текущей базы данных, если таковая существует, для указанной схемы.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-141">VSDBCMD is a differential database deployment tool, and as such the deployment script is dynamically generated to contain all the SQL commands necessary to update the current database, if one exists, to the specified schema.</span></span> <span data-ttu-id="5ad1a-142">Просмотр скрипта развертывания является удобным способом для определения того, какое влияние развертывания будет иметь в текущей базе данных и данных, содержащихся в нем.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-142">Reviewing the deployment script is a useful way to determine what impact your deployment will have on the current database and the data it contains.</span></span> <span data-ttu-id="5ad1a-143">Например может потребоваться указать:</span><span class="sxs-lookup"><span data-stu-id="5ad1a-143">For example, you might want to determine:</span></span>

- <span data-ttu-id="5ad1a-144">Будут ли удалены все существующие таблицы и, приведет к потере данных.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-144">Whether any existing tables will be removed, and whether that will result in data loss.</span></span>
- <span data-ttu-id="5ad1a-145">Порядок операций несет риск потери данных, например, если вы разбиения или слияния таблиц.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-145">Whether the order of operations carries a risk of data loss, for example, if you're splitting or merging tables.</span></span>

<span data-ttu-id="5ad1a-146">Если вы довольны скрипт развертывания, можно повторить VSDBCMD с **/dd+** флаг для внесения изменений.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-146">If you're happy with the deployment script, you can repeat the VSDBCMD with a **/dd+** flag to make the changes.</span></span> <span data-ttu-id="5ad1a-147">Кроме того можно изменить скрипт развертывания для удовлетворения требований и выполнить его вручную на сервере базы данных.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-147">Alternatively, you can edit the deployment script to meet your requirements and then execute it manually on the database server.</span></span>

## <a name="integrating-what-if-functionality-into-custom-project-files"></a><span data-ttu-id="5ad1a-148">Интеграция «Что, если» функциональные возможности в файлы проектов</span><span class="sxs-lookup"><span data-stu-id="5ad1a-148">Integrating "What If" Functionality into Custom Project Files</span></span>

<span data-ttu-id="5ad1a-149">Для более сложных сценариев развертывания, необходимо использовать пользовательский файл проекта Microsoft Build Engine (MSBuild) для инкапсуляции логики построения и развертывания, как описано в [основные сведения о файле проекта](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span><span class="sxs-lookup"><span data-stu-id="5ad1a-149">In more complex deployment scenarios, you'll want to use a custom Microsoft Build Engine (MSBuild) project file to encapsulate your build and deployment logic, as described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span></span> <span data-ttu-id="5ad1a-150">Например, в [диспетчера контактов](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) образец решения, *Publish.proj* файла:</span><span class="sxs-lookup"><span data-stu-id="5ad1a-150">For example, in the [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) sample solution, the *Publish.proj* file:</span></span>

- <span data-ttu-id="5ad1a-151">Построение решения.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-151">Builds the solution.</span></span>
- <span data-ttu-id="5ad1a-152">Использует веб-развертывания для упаковки и развертывания приложения ContactManager.Mvc.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-152">Uses Web Deploy to package and deploy the ContactManager.Mvc application.</span></span>
- <span data-ttu-id="5ad1a-153">Использует веб-развертывания для упаковки и развертывания приложения ContactManager.Service.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-153">Uses Web Deploy to package and deploy the ContactManager.Service application.</span></span>
- <span data-ttu-id="5ad1a-154">Развертывает **ContactManager** базы данных.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-154">Deploys the **ContactManager** database.</span></span>

<span data-ttu-id="5ad1a-155">При интеграции развертывание нескольких базы данных или веб-пакетов в один шаг процесс таким образом, можно также выполнять всего развертывания в режиме «что, если».</span><span class="sxs-lookup"><span data-stu-id="5ad1a-155">When you integrate the deployment of multiple web packages and/or databases into a single-step process in this way, you may also want the option of performing the entire deployment in a "what if" mode.</span></span>

<span data-ttu-id="5ad1a-156">*Publish.proj* файл демонстрирует, как это сделать.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-156">The *Publish.proj* file demonstrates how you can do this.</span></span> <span data-ttu-id="5ad1a-157">Во-первых необходимо создать свойство для хранения значения «что, если»:</span><span class="sxs-lookup"><span data-stu-id="5ad1a-157">First, you need to create a property to store the "what if" value:</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]


<span data-ttu-id="5ad1a-158">В этом случае вы создали свойство с именем **WhatIf** со значением по умолчанию **false**.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-158">In this case, you've created a property named **WhatIf** with a default value of **false**.</span></span> <span data-ttu-id="5ad1a-159">Переопределить значение этого свойства значение **true** в параметре командной строки, как вы увидите.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-159">Users can override this value by setting the property to **true** in a command-line parameter, as you'll see shortly.</span></span>

<span data-ttu-id="5ad1a-160">Следующий этап — параметризацию любой веб-развертывания, VSDBCMD команд, чтобы отразить флаги **WhatIf** значение свойства.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-160">The next stage is to parameterize any Web Deploy and VSDBCMD commands so that the flags reflect the **WhatIf** property value.</span></span> <span data-ttu-id="5ad1a-161">Например, следующий целевой объект (из *Publish.proj* файла и упрощенное) выполняется *. deploy.cmd* файл для развертывания веб-пакета.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-161">For example, the next target (taken from the *Publish.proj* file and simplified) runs the *.deploy.cmd* file to deploy a web package.</span></span> <span data-ttu-id="5ad1a-162">По умолчанию команда включает **/Y** switch («Да» или режим обновления).</span><span class="sxs-lookup"><span data-stu-id="5ad1a-162">By default, the command includes a **/Y** switch ("yes," or update mode).</span></span> <span data-ttu-id="5ad1a-163">Если **WhatIf** равно **true**, оно заменяется **/t** switch (пробной версии или режим «что, если»).</span><span class="sxs-lookup"><span data-stu-id="5ad1a-163">If **WhatIf** is set to **true**, this is replaced by a **/T** switch (trial, or "what if" mode).</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]


<span data-ttu-id="5ad1a-164">Аналогичным образом следующий целевой объект использует служебную программу VSDBCMD для развертывания базы данных.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-164">Similarly, the next target uses the VSDBCMD utility to deploy a database.</span></span> <span data-ttu-id="5ad1a-165">По умолчанию **/dd** ключ не включен.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-165">By default, a **/dd** switch is not included.</span></span> <span data-ttu-id="5ad1a-166">Это означает, что VSDBCMD будет создать скрипт развертывания, но не будет выполнять развертывание базы данных&#x2014;другими словами, «что, если» сценарий.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-166">This means that VSDBCMD will generate a deployment script but will not deploy the database&#x2014;in other words, a "what if" scenario.</span></span> <span data-ttu-id="5ad1a-167">Если **WhatIf** не задано значение **true**, **/dd** добавлен переключатель и VSDBCMD будет выполнять развертывание базы данных.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-167">If the **WhatIf** property is not set to **true**, a **/dd** switch is added and VSDBCMD will deploy the database.</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]


<span data-ttu-id="5ad1a-168">Можно использовать тот же подход можно параметризовать все соответствующие команды в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-168">You can use the same approach to parameterize all the relevant commands in your project file.</span></span> <span data-ttu-id="5ad1a-169">Если вы хотите выполнить развертывание «что, если», затем можно просто предоставить **WhatIf** значение свойства из командной строки:</span><span class="sxs-lookup"><span data-stu-id="5ad1a-169">When you want to run a "what if" deployment, you can then simply provide a **WhatIf** property value from the command line:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]


<span data-ttu-id="5ad1a-170">Таким образом можно запустить для всех компонентов проекта за один шаг развертывания «что, если».</span><span class="sxs-lookup"><span data-stu-id="5ad1a-170">In this way, you can run a "what if" deployment for all your project components in a single step.</span></span>

## <a name="conclusion"></a><span data-ttu-id="5ad1a-171">Заключение</span><span class="sxs-lookup"><span data-stu-id="5ad1a-171">Conclusion</span></span>

<span data-ttu-id="5ad1a-172">В этом разделе описано, как запустить «что, если» развертываний, с помощью веб-развертывания, VSDBCMD и MSBuild.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-172">This topic described how to run "what if" deployments using Web Deploy, VSDBCMD, and MSBuild.</span></span> <span data-ttu-id="5ad1a-173">Развертывание «что, если» позволяет оценить влияние предложенных развертывания, прежде чем фактически вы внесли изменения в целевой среде.</span><span class="sxs-lookup"><span data-stu-id="5ad1a-173">A "what if" deployment lets you evaluate the impact of a proposed deployment before you actually make any changes to the destination environment.</span></span>

## <a name="further-reading"></a><span data-ttu-id="5ad1a-174">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="5ad1a-174">Further Reading</span></span>

<span data-ttu-id="5ad1a-175">Дополнительные сведения о синтаксисе командной строки на веб-развертывания см. в разделе [веб-развертывание параметров операции](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="5ad1a-175">For more information on Web Deploy command-line syntax, see [Web Deploy Operation Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx).</span></span> <span data-ttu-id="5ad1a-176">Рекомендации по параметрам командной строки при использовании *. deploy.cmd* файла см. в разделе [как: установка, развертывание пакета с помощью deploy.cmd файл](https://msdn.microsoft.com/library/ff356104.aspx).</span><span class="sxs-lookup"><span data-stu-id="5ad1a-176">For guidance on command-line options when you use the *.deploy.cmd* file, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/library/ff356104.aspx).</span></span> <span data-ttu-id="5ad1a-177">Рекомендации по VSDBCMD синтаксис командной строки см. в разделе [Справочник по командной строке для VSDBCMD. EXE (развертывание и импорт схемы)](https://msdn.microsoft.com/library/dd193283.aspx).</span><span class="sxs-lookup"><span data-stu-id="5ad1a-177">For guidance on VSDBCMD command-line syntax, see [Command-Line Reference for VSDBCMD.EXE (Deployment and Schema Import)](https://msdn.microsoft.com/library/dd193283.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5ad1a-178">[Назад](advanced-enterprise-web-deployment.md)
> [Вперед](customizing-database-deployments-for-multiple-environments.md)</span><span class="sxs-lookup"><span data-stu-id="5ad1a-178">[Previous](advanced-enterprise-web-deployment.md)
[Next](customizing-database-deployments-for-multiple-environments.md)</span></span>
