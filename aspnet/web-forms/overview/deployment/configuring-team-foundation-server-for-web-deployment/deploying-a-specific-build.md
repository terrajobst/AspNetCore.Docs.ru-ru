---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: "Развертывание конкретной сборки | Документы Microsoft"
author: jrjlee
description: "В этом разделе описывается развертывание веб-пакетов и скриптов базы данных из конкретного предыдущей сборки в новое место назначения, например промежуточной или производственной enviro..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: afac083c96c1396ad60275fcb55a0ec9c4c0bd44
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="deploying-a-specific-build"></a><span data-ttu-id="ede06-103">Развертывание конкретной сборки</span><span class="sxs-lookup"><span data-stu-id="ede06-103">Deploying a Specific Build</span></span>
====================
<span data-ttu-id="ede06-104">по [Джейсон Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="ede06-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="ede06-105">Скачать PDF</span><span class="sxs-lookup"><span data-stu-id="ede06-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="ede06-106">В этом разделе описывается развертывание веб-пакетов и скриптов базы данных из конкретного предыдущей сборки в новое место назначения, как в промежуточной или производственной среде.</span><span class="sxs-lookup"><span data-stu-id="ede06-106">This topic describes how to deploy web packages and database scripts from a specific previous build to a new destination, like a staging or production environment.</span></span>


<span data-ttu-id="ede06-107">Этот раздел входит в состав серию учебников, исходя из требования к развертыванию enterprise вымышленная компания Fabrikam, Inc. Этот учебник ряд использует образец решения & #x 2014; [решения диспетчера контактов](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; для представления веб-приложения с реалистичных уровень сложности, включая приложения ASP.NET MVC 3, Windows Службы Communication Foundation (WCF) и проект базы данных.</span><span class="sxs-lookup"><span data-stu-id="ede06-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="ede06-108">Метод развертывания, в основе этих учебников основан на разбиение проекта файл подход, описанный в [основные сведения о файле проекта](../web-deployment-in-the-enterprise/understanding-the-project-file.md), в которой управляет процессом построения и развертывания двух файлов проекта & #x 2014; o NE, содержащий инструкции сборки, которые применяются для каждой целевой среде и, содержащий параметры построения и развертывания конкретной среды.</span><span class="sxs-lookup"><span data-stu-id="ede06-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build and deployment process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="ede06-109">Во время построения файла проекта среды объединяется в файл проекта зависит от среды, образуют полный набор инструкций построения.</span><span class="sxs-lookup"><span data-stu-id="ede06-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="ede06-110">Общие сведения о задаче</span><span class="sxs-lookup"><span data-stu-id="ede06-110">Task Overview</span></span>

<span data-ttu-id="ede06-111">До этого момента в разделах этого учебника набора основное внимание уделено как для сборки, упаковки и развертывания веб-приложений и баз данных в рамках одного шага или автоматизировать процесс.</span><span class="sxs-lookup"><span data-stu-id="ede06-111">Until now, the topics in this tutorial set have focused on how to build, package, and deploy web applications and databases as part of a single-step or automated process.</span></span> <span data-ttu-id="ede06-112">Однако в некоторых распространенных сценариях, может потребоваться выбрать ресурсы, развертываемые из списка сборок в папке сброса.</span><span class="sxs-lookup"><span data-stu-id="ede06-112">However, in some common scenarios, you'll want to select the resources that you deploy from a list of builds in a drop folder.</span></span> <span data-ttu-id="ede06-113">Другими словами последняя сборка может оказаться сборки, которые вы хотите развернуть.</span><span class="sxs-lookup"><span data-stu-id="ede06-113">In other words, the latest build may not be the build you want to deploy.</span></span>

<span data-ttu-id="ede06-114">Рассмотрим сценарий непрерывной интеграции (CI), описанные в предыдущем разделе, [Создание сборки определение, поддерживает развертывания](creating-a-build-definition-that-supports-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="ede06-114">Consider the continuous integration (CI) scenario described in the previous topic, [Creating a Build Definition That Supports Deployment](creating-a-build-definition-that-supports-deployment.md).</span></span> <span data-ttu-id="ede06-115">Вы создали определение сборки в Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="ede06-115">You've created a build definition in Team Foundation Server (TFS) 2010.</span></span> <span data-ttu-id="ede06-116">Каждый раз, когда разработчик вернет код в TFS, Team Build будет построения кода, создания веб-пакетов и скриптов базы данных как часть процесса построения, запустите модульные тесты и развертывание ресурсов в тестовой среде.</span><span class="sxs-lookup"><span data-stu-id="ede06-116">Every time a developer checks code into TFS, Team Build will build your code, create web packages and database scripts as part of the build process, run any unit tests, and deploy your resources to a test environment.</span></span> <span data-ttu-id="ede06-117">В зависимости от политики хранения, настроенных при создании определения построения TFS, сохранят определенное число предыдущих сборок.</span><span class="sxs-lookup"><span data-stu-id="ede06-117">Depending on the retention policy you configured when you created the build definition, TFS will retain a certain number of previous builds.</span></span>

![](deploying-a-specific-build/_static/image1.png)

<span data-ttu-id="ede06-118">Теперь предположим, что вы выполнили проверку и Проверочное тестирование для одного из этих сборок в среде тестирования, вы будете готовы развернуть приложение в промежуточной среде.</span><span class="sxs-lookup"><span data-stu-id="ede06-118">Now, suppose you've performed verification and validation testing against one of these builds in your test environment, and you're ready to deploy your application to a staging environment.</span></span> <span data-ttu-id="ede06-119">В то же время разработчики могут иметь возвращенные нового кода.</span><span class="sxs-lookup"><span data-stu-id="ede06-119">In the meantime, developers may have checked in new code.</span></span> <span data-ttu-id="ede06-120">Вы не хотите Перестроить решение и развертывание в промежуточной среде, и вы не хотите развернуть последнюю сборку в промежуточной среде.</span><span class="sxs-lookup"><span data-stu-id="ede06-120">You don't want to rebuild the solution and deploy to the staging environment, and you don't want to deploy the latest build to the staging environment.</span></span> <span data-ttu-id="ede06-121">Вместо этого вы хотите развернуть определенной сборки, были проверены и проверены на тестовых серверах.</span><span class="sxs-lookup"><span data-stu-id="ede06-121">Instead, you want to deploy the specific build that you've verified and validated on the test servers.</span></span>

<span data-ttu-id="ede06-122">Для выполнения этой задачи необходимо указать Microsoft Build Engine (MSBuild), где можно найти веб-пакетов и скриптов баз данных, созданных для конкретного построения.</span><span class="sxs-lookup"><span data-stu-id="ede06-122">To accomplish this, you need to tell the Microsoft Build Engine (MSBuild) where to find the web packages and database scripts that a specific build generated.</span></span>

## <a name="overriding-the-outputroot-property"></a><span data-ttu-id="ede06-123">Переопределения свойства OutputRoot</span><span class="sxs-lookup"><span data-stu-id="ede06-123">Overriding the OutputRoot Property</span></span>

<span data-ttu-id="ede06-124">В [образец решения](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), *Publish.proj* файл объявляет свойство с именем **OutputRoot**.</span><span class="sxs-lookup"><span data-stu-id="ede06-124">In the [sample solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), the *Publish.proj* file declares a property named **OutputRoot**.</span></span> <span data-ttu-id="ede06-125">Как видно из имени, это корневой папке, которая содержит все, что процесс построения создает.</span><span class="sxs-lookup"><span data-stu-id="ede06-125">As the name suggests, this is the root folder that contains everything that the build process generates.</span></span> <span data-ttu-id="ede06-126">В *Publish.proj* файл, можно увидеть, **OutputRoot** свойство ссылается на корневой каталог для всех ресурсов развертывания.</span><span class="sxs-lookup"><span data-stu-id="ede06-126">In the *Publish.proj* file, you can see that the **OutputRoot** property refers to the root location for all deployment resources.</span></span>

> [!NOTE]
> <span data-ttu-id="ede06-127">**OutputRoot** — это имя часто используемые свойства.</span><span class="sxs-lookup"><span data-stu-id="ede06-127">**OutputRoot** is a commonly used property name.</span></span> <span data-ttu-id="ede06-128">Visual C# и Visual Basic файлы проекта также объявлять это свойство для хранения на корневой каталог для всех выходных данных сборки.</span><span class="sxs-lookup"><span data-stu-id="ede06-128">Visual C# and Visual Basic project files also declare this property to store the root location for all build outputs.</span></span>


[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]


<span data-ttu-id="ede06-129">Если требуется, чтобы в файле проекта для развертывания веб-пакетов и скриптов базы данных в другое расположение и #x 2014; например выходные данные предыдущей сборки TFS & #x 2014; необходимо переопределить **OutputRoot** свойство.</span><span class="sxs-lookup"><span data-stu-id="ede06-129">If you want your project file to deploy web packages and database scripts from a different location&#x2014;like the outputs of a previous TFS build&#x2014;you simply need to override the **OutputRoot** property.</span></span> <span data-ttu-id="ede06-130">Следует задать значение свойства в соответствующую папку на сервере Team Build.</span><span class="sxs-lookup"><span data-stu-id="ede06-130">You should set the property value to the relevant build folder on the Team Build server.</span></span> <span data-ttu-id="ede06-131">При запуске MSBuild из командной строки, можно указать значение для **OutputRoot** аргумент командной строки:</span><span class="sxs-lookup"><span data-stu-id="ede06-131">If you were running MSBuild from the command line, you could specify a value for **OutputRoot** as a command-line argument:</span></span>


[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]


<span data-ttu-id="ede06-132">На практике, однако также требуется пропустить **построения** целевая & #x 2014; нет смысла при построении решения, если вы не планируете использовать выходные данные сборки.</span><span class="sxs-lookup"><span data-stu-id="ede06-132">In practice, however, you'd also want to skip the **Build** target&#x2014;there's no point in building your solution if you don't plan to use the build outputs.</span></span> <span data-ttu-id="ede06-133">Это можно сделать путем указания целевых объектов, которые необходимо выполнить из командной строки:</span><span class="sxs-lookup"><span data-stu-id="ede06-133">You could do this by specifying the targets you want to execute from the command line:</span></span>


[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]


<span data-ttu-id="ede06-134">Однако в большинстве случаев имеет смысл встраивать логику развертывания в определение сборки TFS.</span><span class="sxs-lookup"><span data-stu-id="ede06-134">However, in most cases, you'll want to build your deployment logic into a TFS build definition.</span></span> <span data-ttu-id="ede06-135">Это позволяет пользователям с **построений в очередь** разрешение для инициации развертывания из любой установки Visual Studio с подключением к серверу TFS.</span><span class="sxs-lookup"><span data-stu-id="ede06-135">This enables users with the **Queue builds** permission to trigger the deployment from any Visual Studio installation with a connection to the TFS server.</span></span>

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a><span data-ttu-id="ede06-136">Создание определения построения для развертывания сборки</span><span class="sxs-lookup"><span data-stu-id="ede06-136">Creating a Build Definition to Deploy Specific Builds</span></span>

<span data-ttu-id="ede06-137">Далее описано, как создание определения построения, позволяющий пользователям триггера развертывания в промежуточной среде, с помощью одной команды.</span><span class="sxs-lookup"><span data-stu-id="ede06-137">The next procedure describes how to create a build definition that enables users to trigger deployments to a staging environment with a single command.</span></span>

<span data-ttu-id="ede06-138">В этом случае необходимо исключить из определения построения, чтобы построить все & #x 2014; можно просто нужно выполнение логики развертывания в файле пользовательского проекта.</span><span class="sxs-lookup"><span data-stu-id="ede06-138">In this case, you don't want the build definition to actually build anything&#x2014;you just want it to execute the deployment logic in your custom project file.</span></span> <span data-ttu-id="ede06-139">*Publish.proj* файл включает условную логику, которая пропускает **построения** целевой, если файл работает в Team Build.</span><span class="sxs-lookup"><span data-stu-id="ede06-139">The *Publish.proj* file includes conditional logic that skips the **Build** target if the file is running in Team Build.</span></span> <span data-ttu-id="ede06-140">Это делается путем оценки встроенных **BuildingInTeamBuild** свойство, которое будет автоматически присвоено **true** при запуске файла проекта в Team Build.</span><span class="sxs-lookup"><span data-stu-id="ede06-140">It does this by evaluating the built-in **BuildingInTeamBuild** property, which is automatically set to **true** if you run your project file in Team Build.</span></span> <span data-ttu-id="ede06-141">В результате можно пропустить процесс построения и просто запустите файл проекта, чтобы развертывание существующей сборки.</span><span class="sxs-lookup"><span data-stu-id="ede06-141">As a result, you can skip the build process and simply run the project file to deploy an existing build.</span></span>

<span data-ttu-id="ede06-142">**Создание определения построения для запуска развертывания вручную**</span><span class="sxs-lookup"><span data-stu-id="ede06-142">**To create a build definition to trigger deployment manually**</span></span>

1. <span data-ttu-id="ede06-143">В Visual Studio 2010 в **Team Explorer** окна, разверните узел командного проекта, щелкните правой кнопкой мыши **строит**, а затем нажмите кнопку **создать определение построения**.</span><span class="sxs-lookup"><span data-stu-id="ede06-143">In Visual Studio 2010, in the **Team Explorer** window, expand your team project node, right-click **Builds**, and then click **New Build Definition**.</span></span>

    ![](deploying-a-specific-build/_static/image2.png)
2. <span data-ttu-id="ede06-144">На **Общие** вкладки, присвойте имя определения сборки (например, **DeployToStaging**) и необязательное описание.</span><span class="sxs-lookup"><span data-stu-id="ede06-144">On the **General** tab, give the build definition a name (for example, **DeployToStaging**) and an optional description.</span></span>
3. <span data-ttu-id="ede06-145">На **триггер** выберите **ручной — возвраты не запускают новую сборку**.</span><span class="sxs-lookup"><span data-stu-id="ede06-145">On the **Trigger** tab, select **Manual – Check-ins do not trigger a new build**.</span></span>
4. <span data-ttu-id="ede06-146">На **сборки по умолчанию** вкладке **Копировать выходные данные построения в следующую папку сброса** введите путь к папке сброса соглашения об универсальных именах (UNC) (например,  **\\TFSBUILD\Drops**).</span><span class="sxs-lookup"><span data-stu-id="ede06-146">On the **Build Defaults** tab, in the **Copy build output to the following drop folder** box, type the Universal Naming Convention (UNC) path of your drop folder (for example, **\\TFSBUILD\Drops**).</span></span>

    ![](deploying-a-specific-build/_static/image3.png)
5. <span data-ttu-id="ede06-147">На **процесс** вкладке **файл процесса построения** раскрывающегося списка, оставьте **DefaultTemplate.xaml** выбранных.</span><span class="sxs-lookup"><span data-stu-id="ede06-147">On the **Process** tab, in the **Build process file** dropdown list, leave **DefaultTemplate.xaml** selected.</span></span> <span data-ttu-id="ede06-148">Это один из шаблонов процесса сборки по умолчанию, которые добавляются в новые командные проекты.</span><span class="sxs-lookup"><span data-stu-id="ede06-148">This is one of the default build process templates that get added to all new team projects.</span></span>
6. <span data-ttu-id="ede06-149">В **параметры процесса построения** таблицы, нажмите кнопку в **элементы для построения** , а затем нажмите **многоточие** кнопки.</span><span class="sxs-lookup"><span data-stu-id="ede06-149">In the **Build process parameters** table, click in the **Items to Build** row, and then click the **ellipsis** button.</span></span>

    ![](deploying-a-specific-build/_static/image4.png)
7. <span data-ttu-id="ede06-150">В **элементы для построения** диалоговое окно, нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="ede06-150">In the **Items to Build** dialog box, click **Add**.</span></span>
8. <span data-ttu-id="ede06-151">В **элементы типа** раскрывающегося списка выберите **файлы проекта MSBuild**.</span><span class="sxs-lookup"><span data-stu-id="ede06-151">In the **Items of type** dropdown list, select **MSBuild Project files**.</span></span>
9. <span data-ttu-id="ede06-152">Перейдите к нужному файлу пользовательского проекта с помощью которого контролировать процесс развертывания, выберите файл и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="ede06-152">Browse to the location of the custom project file with which you control the deployment process, select the file, and then click **OK**.</span></span>

    ![](deploying-a-specific-build/_static/image5.png)
10. <span data-ttu-id="ede06-153">В **элементы для построения** диалоговое окно, нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="ede06-153">In the **Items to Build** dialog box, click **OK**.</span></span>
11. <span data-ttu-id="ede06-154">В **параметры процесса построения** таблица, разверните **Дополнительно** раздела.</span><span class="sxs-lookup"><span data-stu-id="ede06-154">In the **Build process parameters** table, expand the **Advanced** section.</span></span>
12. <span data-ttu-id="ede06-155">В **Аргументы MSBuild** строк, укажите расположение файла проекта в определенной среде и добавить заполнитель для расположения папки сборки:</span><span class="sxs-lookup"><span data-stu-id="ede06-155">In the **MSBuild Arguments** row, specify the location of your environment-specific project file and add a placeholder for the location of your build folder:</span></span>

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > <span data-ttu-id="ede06-156">Необходимо переопределить **OutputRoot** значение каждый раз при запуске построения.</span><span class="sxs-lookup"><span data-stu-id="ede06-156">You'll need to override the **OutputRoot** value every time you queue a build.</span></span> <span data-ttu-id="ede06-157">Это рассматривается в следующей процедуре.</span><span class="sxs-lookup"><span data-stu-id="ede06-157">This is covered in the next procedure.</span></span>
13. <span data-ttu-id="ede06-158">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="ede06-158">Click **Save**.</span></span>

<span data-ttu-id="ede06-159">Если запустить сборку, необходимо обновить **OutputRoot** свойство для указания сборки, которую требуется развернуть.</span><span class="sxs-lookup"><span data-stu-id="ede06-159">When you trigger a build, you need to update the **OutputRoot** property to point to the build you want to deploy.</span></span>

<span data-ttu-id="ede06-160">**Для развертывания конкретной сборки из определения построения**</span><span class="sxs-lookup"><span data-stu-id="ede06-160">**To deploy a specific build from a build definition**</span></span>

1. <span data-ttu-id="ede06-161">В **Team Explorer** окно, щелкните правой кнопкой мыши определение построения и нажмите кнопку **новую сборку в очередь**.</span><span class="sxs-lookup"><span data-stu-id="ede06-161">In the **Team Explorer** window, right-click the build definition, and then click **Queue New Build**.</span></span>

    ![](deploying-a-specific-build/_static/image7.png)
2. <span data-ttu-id="ede06-162">В **поставить в очередь построение** диалогового **параметры** , раскройте **Дополнительно** раздела.</span><span class="sxs-lookup"><span data-stu-id="ede06-162">In the **Queue Build** dialog box, on the **Parameters** tab, expand the **Advanced** section.</span></span>
3. <span data-ttu-id="ede06-163">В **Аргументы MSBuild** строк, замените значение **OutputRoot** свойство с расположением папки сборки.</span><span class="sxs-lookup"><span data-stu-id="ede06-163">In the **MSBuild Arguments** row, replace the value of the **OutputRoot** property with the location of your build folder.</span></span> <span data-ttu-id="ede06-164">Пример:</span><span class="sxs-lookup"><span data-stu-id="ede06-164">For example:</span></span>

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > <span data-ttu-id="ede06-165">Не забудьте добавить косую черту в конце пути к папке построения.</span><span class="sxs-lookup"><span data-stu-id="ede06-165">Be sure to include a trailing slash at the end of the path to your build folder.</span></span>
4. <span data-ttu-id="ede06-166">Нажмите кнопку **очереди**.</span><span class="sxs-lookup"><span data-stu-id="ede06-166">Click **Queue**.</span></span>

<span data-ttu-id="ede06-167">Когда построение в очередь, файл проекта будут развернуты скриптов базы данных и web пакеты из папки размещения построений вы указали в **OutputRoot** свойство.</span><span class="sxs-lookup"><span data-stu-id="ede06-167">When you queue the build, the project file will deploy the database scripts and web packages from the build drop folder you specified in the **OutputRoot** property.</span></span>

## <a name="conclusion"></a><span data-ttu-id="ede06-168">Заключение</span><span class="sxs-lookup"><span data-stu-id="ede06-168">Conclusion</span></span>

<span data-ttu-id="ede06-169">В этом разделе описано, как опубликовать развертывания ресурсов, таких как веб-пакетов и скриптов базы данных, из предыдущей конкретной сборки с помощью модели развертывания проекта файл разбиения.</span><span class="sxs-lookup"><span data-stu-id="ede06-169">This topic described how to publish deployment resources, like web packages and database scripts, from a specific previous build using the split project file deployment model.</span></span> <span data-ttu-id="ede06-170">Оно описано, как переопределить **OutputRoot** определение построения и встраивать логику развертывания в TFS.</span><span class="sxs-lookup"><span data-stu-id="ede06-170">It explained how to override the **OutputRoot** property and how to incorporate the deployment logic into a TFS build definition.</span></span>

## <a name="further-reading"></a><span data-ttu-id="ede06-171">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="ede06-171">Further Reading</span></span>

<span data-ttu-id="ede06-172">Дополнительные сведения о создании определений сборки см. в разделе [создание базового определения сборки](https://msdn.microsoft.com/en-us/library/ms181716.aspx) и [определить процедуру сборки](https://msdn.microsoft.com/en-us/library/ms181715.aspx).</span><span class="sxs-lookup"><span data-stu-id="ede06-172">For more information on creating build definitions, see [Create a Basic Build Definition](https://msdn.microsoft.com/en-us/library/ms181716.aspx) and [Define Your Build Process](https://msdn.microsoft.com/en-us/library/ms181715.aspx).</span></span> <span data-ttu-id="ede06-173">Дополнительные рекомендации по очереди сборок см. в разделе [очередь сборку](https://msdn.microsoft.com/en-us/library/ms181722.aspx).</span><span class="sxs-lookup"><span data-stu-id="ede06-173">For more guidance on queuing builds, see [Queue a Build](https://msdn.microsoft.com/en-us/library/ms181722.aspx).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ede06-174">[Назад](creating-a-build-definition-that-supports-deployment.md)
[Вперед](configuring-permissions-for-team-build-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="ede06-174">[Previous](creating-a-build-definition-that-supports-deployment.md)
[Next](configuring-permissions-for-team-build-deployment.md)</span></span>
