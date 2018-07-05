---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: Развертывание конкретной сборки | Документация Майкрософт
author: jrjlee
description: В этом разделе описывается развертывание веб-пакеты и скрипты базы данных из определенной предыдущей сборки в новое место назначения, такие как промежуточное хранение или производство стандартной среды рабочего...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: 6d55497dbc13133aa9c8b8eaecca0f6915fd9ed0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388228"
---
<a name="deploying-a-specific-build"></a><span data-ttu-id="d2733-103">Развертывание конкретной сборки</span><span class="sxs-lookup"><span data-stu-id="d2733-103">Deploying a Specific Build</span></span>
====================
<span data-ttu-id="d2733-104">по [Джейсон Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="d2733-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="d2733-105">Загрузить PDF-файл</span><span class="sxs-lookup"><span data-stu-id="d2733-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="d2733-106">В этом разделе описывается развертывание веб-пакеты и скрипты базы данных из определенной предыдущей сборки в новое место назначения, например в промежуточной или производственной среде.</span><span class="sxs-lookup"><span data-stu-id="d2733-106">This topic describes how to deploy web packages and database scripts from a specific previous build to a new destination, like a staging or production environment.</span></span>


<span data-ttu-id="d2733-107">Этот раздел является частью серии учебников, исходя из требования к развертыванию enterprise вымышленной компании Fabrikam, Inc. В этой серии руководств используется пример решения&#x2014; [решения диспетчера контактов](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;для представления веб-приложения с более реалистичные уровень сложности, включая приложения ASP.NET MVC 3, Windows Communication Служба Foundation (WCF) и проект базы данных.</span><span class="sxs-lookup"><span data-stu-id="d2733-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="d2733-108">Метод развертывания в основе этих учебников основан на разбиение проекта файл подход, описанный в [основные сведения о файле проекта](../web-deployment-in-the-enterprise/understanding-the-project-file.md), в которой процесс построения и развертывания управляется двумя файлы проекта&#x2014;один содержащая инструкции сборки, которые применяются к каждой целевой среде и содержащего параметры построения и развертывания для конкретной среды.</span><span class="sxs-lookup"><span data-stu-id="d2733-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build and deployment process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="d2733-109">Во время сборки файл проекта для конкретной среды объединяется в файл проекта независимой от среды, чтобы полный набор инструкций построения.</span><span class="sxs-lookup"><span data-stu-id="d2733-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="d2733-110">Общие сведения о задачах</span><span class="sxs-lookup"><span data-stu-id="d2733-110">Task Overview</span></span>

<span data-ttu-id="d2733-111">До сих пор разделы в рамках этого учебного курса с фокусом ввода, о том, как для сборки, упаковки и развертывания веб-приложений и баз данных как часть один шаг или автоматизировать процесс.</span><span class="sxs-lookup"><span data-stu-id="d2733-111">Until now, the topics in this tutorial set have focused on how to build, package, and deploy web applications and databases as part of a single-step or automated process.</span></span> <span data-ttu-id="d2733-112">Однако в некоторых распространенных сценариев, нужно будет выбрать из списка сборок в папке сброса развертываемым ресурсам.</span><span class="sxs-lookup"><span data-stu-id="d2733-112">However, in some common scenarios, you'll want to select the resources that you deploy from a list of builds in a drop folder.</span></span> <span data-ttu-id="d2733-113">Другими словами последней сборке может оказаться сборки, который вы хотите развернуть.</span><span class="sxs-lookup"><span data-stu-id="d2733-113">In other words, the latest build may not be the build you want to deploy.</span></span>

<span data-ttu-id="d2733-114">Рассмотрим сценарий непрерывной интеграции (CI), описанные в предыдущем разделе, [Создание сборки определение что поддерживает развертывания](creating-a-build-definition-that-supports-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="d2733-114">Consider the continuous integration (CI) scenario described in the previous topic, [Creating a Build Definition That Supports Deployment](creating-a-build-definition-that-supports-deployment.md).</span></span> <span data-ttu-id="d2733-115">Вы создали определение сборки в Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="d2733-115">You've created a build definition in Team Foundation Server (TFS) 2010.</span></span> <span data-ttu-id="d2733-116">Каждый раз, чтобы разработчик возвращает код в Team Foundation Server, Team Build будет сборки кода, создание веб-пакеты и скрипты базы данных как часть процесса построения, выполните все модульные тесты и развертывания ресурсов в среде тестирования.</span><span class="sxs-lookup"><span data-stu-id="d2733-116">Every time a developer checks code into TFS, Team Build will build your code, create web packages and database scripts as part of the build process, run any unit tests, and deploy your resources to a test environment.</span></span> <span data-ttu-id="d2733-117">В зависимости от политики хранения, которые были настроены при создании определения построения TFS будет сохранять определенное количество предыдущих сборок.</span><span class="sxs-lookup"><span data-stu-id="d2733-117">Depending on the retention policy you configured when you created the build definition, TFS will retain a certain number of previous builds.</span></span>

![](deploying-a-specific-build/_static/image1.png)

<span data-ttu-id="d2733-118">Теперь предположим, что вы выполнили проверку подлинности и Проверочное тестирование для одного из этих сборок в среде тестирования, вы будете готовы развернуть приложение в промежуточной среде.</span><span class="sxs-lookup"><span data-stu-id="d2733-118">Now, suppose you've performed verification and validation testing against one of these builds in your test environment, and you're ready to deploy your application to a staging environment.</span></span> <span data-ttu-id="d2733-119">В то же время разработчики могут проверили в новом коде.</span><span class="sxs-lookup"><span data-stu-id="d2733-119">In the meantime, developers may have checked in new code.</span></span> <span data-ttu-id="d2733-120">Вы не хотите Перестройте решение и развертывание в промежуточной среде, и вы не хотите развернуть последнюю сборку в промежуточной среде.</span><span class="sxs-lookup"><span data-stu-id="d2733-120">You don't want to rebuild the solution and deploy to the staging environment, and you don't want to deploy the latest build to the staging environment.</span></span> <span data-ttu-id="d2733-121">Вместо этого требуется выполнить развертывание конкретной сборки, вы проверен и проверенных на тестовых серверах.</span><span class="sxs-lookup"><span data-stu-id="d2733-121">Instead, you want to deploy the specific build that you've verified and validated on the test servers.</span></span>

<span data-ttu-id="d2733-122">Для этого необходимо указать Microsoft Build Engine (MSBuild), где можно найти на веб-пакеты и скрипты базы данных, которые созданы для определенной сборки.</span><span class="sxs-lookup"><span data-stu-id="d2733-122">To accomplish this, you need to tell the Microsoft Build Engine (MSBuild) where to find the web packages and database scripts that a specific build generated.</span></span>

## <a name="overriding-the-outputroot-property"></a><span data-ttu-id="d2733-123">Переопределения свойства OutputRoot</span><span class="sxs-lookup"><span data-stu-id="d2733-123">Overriding the OutputRoot Property</span></span>

<span data-ttu-id="d2733-124">В [образец решения](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), *Publish.proj* файле объявляется свойство с именем **OutputRoot**.</span><span class="sxs-lookup"><span data-stu-id="d2733-124">In the [sample solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), the *Publish.proj* file declares a property named **OutputRoot**.</span></span> <span data-ttu-id="d2733-125">Как и предполагает имя, это корневой папке, которая содержит все, что процесс построения создает.</span><span class="sxs-lookup"><span data-stu-id="d2733-125">As the name suggests, this is the root folder that contains everything that the build process generates.</span></span> <span data-ttu-id="d2733-126">В *Publish.proj* файл, вы увидите, **OutputRoot** свойство ссылается на корневой каталог для всех ресурсов развертывания.</span><span class="sxs-lookup"><span data-stu-id="d2733-126">In the *Publish.proj* file, you can see that the **OutputRoot** property refers to the root location for all deployment resources.</span></span>

> [!NOTE]
> <span data-ttu-id="d2733-127">**OutputRoot** — это имя часто используемые свойства.</span><span class="sxs-lookup"><span data-stu-id="d2733-127">**OutputRoot** is a commonly used property name.</span></span> <span data-ttu-id="d2733-128">Файлы Visual C# и Visual Basic проекта также объявлять это свойство для хранения корневой каталог для всех выходных данных сборки.</span><span class="sxs-lookup"><span data-stu-id="d2733-128">Visual C# and Visual Basic project files also declare this property to store the root location for all build outputs.</span></span>


[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]


<span data-ttu-id="d2733-129">Если требуется, чтобы в файле проекта для развертывания веб-пакетов и скриптов в другом расположении базы данных&#x2014;выходные данные предыдущей сборки TFS, такие как&#x2014;вам просто нужно переопределить **OutputRoot** свойство.</span><span class="sxs-lookup"><span data-stu-id="d2733-129">If you want your project file to deploy web packages and database scripts from a different location&#x2014;like the outputs of a previous TFS build&#x2014;you simply need to override the **OutputRoot** property.</span></span> <span data-ttu-id="d2733-130">Следует задать значение свойства в соответствующую папку на сервере Team Build.</span><span class="sxs-lookup"><span data-stu-id="d2733-130">You should set the property value to the relevant build folder on the Team Build server.</span></span> <span data-ttu-id="d2733-131">При запуске MSBuild из командной строки, можно указать значение для **OutputRoot** аргумент командной строки:</span><span class="sxs-lookup"><span data-stu-id="d2733-131">If you were running MSBuild from the command line, you could specify a value for **OutputRoot** as a command-line argument:</span></span>


[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]


<span data-ttu-id="d2733-132">На практике, тем не менее, будет также нужно пропустить **построения** целевой&#x2014;нет смысла в создании решения в том случае, если вы не планируете использовать выходные данные сборки.</span><span class="sxs-lookup"><span data-stu-id="d2733-132">In practice, however, you'd also want to skip the **Build** target&#x2014;there's no point in building your solution if you don't plan to use the build outputs.</span></span> <span data-ttu-id="d2733-133">Это можно сделать путем указания целей, которые вы хотите выполнить из командной строки:</span><span class="sxs-lookup"><span data-stu-id="d2733-133">You could do this by specifying the targets you want to execute from the command line:</span></span>


[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]


<span data-ttu-id="d2733-134">Однако в большинстве случаев необходимо встроить логики развертывания в определении сборки TFS.</span><span class="sxs-lookup"><span data-stu-id="d2733-134">However, in most cases, you'll want to build your deployment logic into a TFS build definition.</span></span> <span data-ttu-id="d2733-135">Это позволяет пользователям с **помещать сборки в очередь** разрешений на активацию развертывания из любой установки Visual Studio с подключением к серверу TFS.</span><span class="sxs-lookup"><span data-stu-id="d2733-135">This enables users with the **Queue builds** permission to trigger the deployment from any Visual Studio installation with a connection to the TFS server.</span></span>

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a><span data-ttu-id="d2733-136">Создание определения сборки для развертывания определенных сборок</span><span class="sxs-lookup"><span data-stu-id="d2733-136">Creating a Build Definition to Deploy Specific Builds</span></span>

<span data-ttu-id="d2733-137">Далее описано, как создать определение сборки, который позволяет пользователям активацию развертывания в промежуточной среде, с помощью одной команды.</span><span class="sxs-lookup"><span data-stu-id="d2733-137">The next procedure describes how to create a build definition that enables users to trigger deployments to a staging environment with a single command.</span></span>

<span data-ttu-id="d2733-138">В этом случае вы не хотите определения построения к фактической сборке ничего&#x2014;необходимо просто выполнение логики развертывания в файле пользовательского проекта.</span><span class="sxs-lookup"><span data-stu-id="d2733-138">In this case, you don't want the build definition to actually build anything&#x2014;you just want it to execute the deployment logic in your custom project file.</span></span> <span data-ttu-id="d2733-139">*Publish.proj* файл включает условную логику, которая пропускает **построения** ориентироваться на базе файла выполняется в Team Build.</span><span class="sxs-lookup"><span data-stu-id="d2733-139">The *Publish.proj* file includes conditional logic that skips the **Build** target if the file is running in Team Build.</span></span> <span data-ttu-id="d2733-140">Это достигается путем оценки встроенных **BuildingInTeamBuild** свойство, которое автоматически присваивается **true** при запуске файла проекта в Team Build.</span><span class="sxs-lookup"><span data-stu-id="d2733-140">It does this by evaluating the built-in **BuildingInTeamBuild** property, which is automatically set to **true** if you run your project file in Team Build.</span></span> <span data-ttu-id="d2733-141">Таким образом можно пропустить процесс построения и просто запустите файл проекта, чтобы развертывать существующую сборку.</span><span class="sxs-lookup"><span data-stu-id="d2733-141">As a result, you can skip the build process and simply run the project file to deploy an existing build.</span></span>

<span data-ttu-id="d2733-142">**Чтобы создать определение сборки, чтобы активировать развертывание вручную**</span><span class="sxs-lookup"><span data-stu-id="d2733-142">**To create a build definition to trigger deployment manually**</span></span>

1. <span data-ttu-id="d2733-143">В Visual Studio 2010 в **Team Explorer** окна, разверните узел командного проекта, щелкните правой кнопкой мыши **построения**, а затем нажмите кнопку **создать определение сборки**.</span><span class="sxs-lookup"><span data-stu-id="d2733-143">In Visual Studio 2010, in the **Team Explorer** window, expand your team project node, right-click **Builds**, and then click **New Build Definition**.</span></span>

    ![](deploying-a-specific-build/_static/image2.png)
2. <span data-ttu-id="d2733-144">На **Общие** вкладке, присвойте имя определения сборки (например, **DeployToStaging**) и необязательное описание.</span><span class="sxs-lookup"><span data-stu-id="d2733-144">On the **General** tab, give the build definition a name (for example, **DeployToStaging**) and an optional description.</span></span>
3. <span data-ttu-id="d2733-145">На **триггера** выберите **вручную — возвраты не запускают новую сборку**.</span><span class="sxs-lookup"><span data-stu-id="d2733-145">On the **Trigger** tab, select **Manual – Check-ins do not trigger a new build**.</span></span>
4. <span data-ttu-id="d2733-146">На **сборки по умолчанию** на вкладке **Копировать выходные данные в следующую папку сброса** введите путь к транзитному универсальными именами (UNC) (например,  **\\TFSBUILD\Drops**).</span><span class="sxs-lookup"><span data-stu-id="d2733-146">On the **Build Defaults** tab, in the **Copy build output to the following drop folder** box, type the Universal Naming Convention (UNC) path of your drop folder (for example, **\\TFSBUILD\Drops**).</span></span>

    ![](deploying-a-specific-build/_static/image3.png)
5. <span data-ttu-id="d2733-147">На **процесс** на вкладке **файл процесса сборки** раскрывающегося списка, оставьте **DefaultTemplate.xaml** выбранного.</span><span class="sxs-lookup"><span data-stu-id="d2733-147">On the **Process** tab, in the **Build process file** dropdown list, leave **DefaultTemplate.xaml** selected.</span></span> <span data-ttu-id="d2733-148">Это один из шаблонов процесса сборки по умолчанию, добавляемых в новые командные проекты.</span><span class="sxs-lookup"><span data-stu-id="d2733-148">This is one of the default build process templates that get added to all new team projects.</span></span>
6. <span data-ttu-id="d2733-149">В **параметры процесса сборки** таблицы, щелкните поле **элементы для построения** строк, а затем нажмите кнопку **кнопку с многоточием** кнопки.</span><span class="sxs-lookup"><span data-stu-id="d2733-149">In the **Build process parameters** table, click in the **Items to Build** row, and then click the **ellipsis** button.</span></span>

    ![](deploying-a-specific-build/_static/image4.png)
7. <span data-ttu-id="d2733-150">В **элементы для построения** диалоговом окне щелкните **добавить**.</span><span class="sxs-lookup"><span data-stu-id="d2733-150">In the **Items to Build** dialog box, click **Add**.</span></span>
8. <span data-ttu-id="d2733-151">В **элементы типа** раскрывающемся списке выберите **файлы проекта MSBuild**.</span><span class="sxs-lookup"><span data-stu-id="d2733-151">In the **Items of type** dropdown list, select **MSBuild Project files**.</span></span>
9. <span data-ttu-id="d2733-152">Перейдите к расположению файла пользовательского проекта с помощью которого можно контролировать процесс развертывания, выберите файл и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="d2733-152">Browse to the location of the custom project file with which you control the deployment process, select the file, and then click **OK**.</span></span>

    ![](deploying-a-specific-build/_static/image5.png)
10. <span data-ttu-id="d2733-153">В **элементы для построения** диалоговом окне щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="d2733-153">In the **Items to Build** dialog box, click **OK**.</span></span>
11. <span data-ttu-id="d2733-154">В **параметры процесса сборки** таблицу, разверните узел **Дополнительно** раздел.</span><span class="sxs-lookup"><span data-stu-id="d2733-154">In the **Build process parameters** table, expand the **Advanced** section.</span></span>
12. <span data-ttu-id="d2733-155">В **Аргументы MSBuild** строк, укажите расположение файла проекта для конкретной среды и добавить заполнитель для расположения папки сборки:</span><span class="sxs-lookup"><span data-stu-id="d2733-155">In the **MSBuild Arguments** row, specify the location of your environment-specific project file and add a placeholder for the location of your build folder:</span></span>

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > <span data-ttu-id="d2733-156">Необходимо переопределить **OutputRoot** значение каждый раз, поставьте построение в очередь.</span><span class="sxs-lookup"><span data-stu-id="d2733-156">You'll need to override the **OutputRoot** value every time you queue a build.</span></span> <span data-ttu-id="d2733-157">Это рассматривается в следующей процедуре.</span><span class="sxs-lookup"><span data-stu-id="d2733-157">This is covered in the next procedure.</span></span>
13. <span data-ttu-id="d2733-158">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="d2733-158">Click **Save**.</span></span>

<span data-ttu-id="d2733-159">Когда вы запускаете сборку, необходимо обновить **OutputRoot** свойство, чтобы она указывала на сборку, необходимо развернуть.</span><span class="sxs-lookup"><span data-stu-id="d2733-159">When you trigger a build, you need to update the **OutputRoot** property to point to the build you want to deploy.</span></span>

<span data-ttu-id="d2733-160">**Для развертывания определенной сборки из определения сборки**</span><span class="sxs-lookup"><span data-stu-id="d2733-160">**To deploy a specific build from a build definition**</span></span>

1. <span data-ttu-id="d2733-161">В **Team Explorer** окно, щелкните правой кнопкой мыши определение построения и нажмите кнопку **поставить новую сборку в**.</span><span class="sxs-lookup"><span data-stu-id="d2733-161">In the **Team Explorer** window, right-click the build definition, and then click **Queue New Build**.</span></span>

    ![](deploying-a-specific-build/_static/image7.png)
2. <span data-ttu-id="d2733-162">В **поставить сборку в очередь** диалоговом окне **параметры** вкладке, разверните узел **Дополнительно** раздел.</span><span class="sxs-lookup"><span data-stu-id="d2733-162">In the **Queue Build** dialog box, on the **Parameters** tab, expand the **Advanced** section.</span></span>
3. <span data-ttu-id="d2733-163">В **Аргументы MSBuild** строк, замените значение **OutputRoot** свойство с расположением папки сборки.</span><span class="sxs-lookup"><span data-stu-id="d2733-163">In the **MSBuild Arguments** row, replace the value of the **OutputRoot** property with the location of your build folder.</span></span> <span data-ttu-id="d2733-164">Пример:</span><span class="sxs-lookup"><span data-stu-id="d2733-164">For example:</span></span>

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > <span data-ttu-id="d2733-165">Не забудьте включить косую черту в конце пути к папке сборки.</span><span class="sxs-lookup"><span data-stu-id="d2733-165">Be sure to include a trailing slash at the end of the path to your build folder.</span></span>
4. <span data-ttu-id="d2733-166">Нажмите кнопку **очереди**.</span><span class="sxs-lookup"><span data-stu-id="d2733-166">Click **Queue**.</span></span>

<span data-ttu-id="d2733-167">Когда построение в очередь, файл проекта будет развертывать скрипты базы данных и web пакеты из папки размещения построений, указанному в **OutputRoot** свойство.</span><span class="sxs-lookup"><span data-stu-id="d2733-167">When you queue the build, the project file will deploy the database scripts and web packages from the build drop folder you specified in the **OutputRoot** property.</span></span>

## <a name="conclusion"></a><span data-ttu-id="d2733-168">Заключение</span><span class="sxs-lookup"><span data-stu-id="d2733-168">Conclusion</span></span>

<span data-ttu-id="d2733-169">В этом разделе описано, как опубликовать развертывания ресурсов, таких как веб-пакеты и скрипты базы данных, из определенной предыдущей сборки с помощью модели развертывания проекта файл разбиения.</span><span class="sxs-lookup"><span data-stu-id="d2733-169">This topic described how to publish deployment resources, like web packages and database scripts, from a specific previous build using the split project file deployment model.</span></span> <span data-ttu-id="d2733-170">Оно описывает способ переопределения **OutputRoot** определение сборки и включения логики развертывания в TFS.</span><span class="sxs-lookup"><span data-stu-id="d2733-170">It explained how to override the **OutputRoot** property and how to incorporate the deployment logic into a TFS build definition.</span></span>

## <a name="further-reading"></a><span data-ttu-id="d2733-171">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="d2733-171">Further Reading</span></span>

<span data-ttu-id="d2733-172">Дополнительные сведения о создании определений сборки см. в разделе [создание базового определения сборки](https://msdn.microsoft.com/library/ms181716.aspx) и [определить процедуру сборки](https://msdn.microsoft.com/library/ms181715.aspx).</span><span class="sxs-lookup"><span data-stu-id="d2733-172">For more information on creating build definitions, see [Create a Basic Build Definition](https://msdn.microsoft.com/library/ms181716.aspx) and [Define Your Build Process](https://msdn.microsoft.com/library/ms181715.aspx).</span></span> <span data-ttu-id="d2733-173">Дополнительные рекомендации по очереди сборок см. в разделе [поставить в очередь сборку](https://msdn.microsoft.com/library/ms181722.aspx).</span><span class="sxs-lookup"><span data-stu-id="d2733-173">For more guidance on queuing builds, see [Queue a Build](https://msdn.microsoft.com/library/ms181722.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d2733-174">[Назад](creating-a-build-definition-that-supports-deployment.md)
> [Вперед](configuring-permissions-for-team-build-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="d2733-174">[Previous](creating-a-build-definition-that-supports-deployment.md)
[Next](configuring-permissions-for-team-build-deployment.md)</span></span>
