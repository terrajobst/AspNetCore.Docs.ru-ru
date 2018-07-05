---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: Создание командного проекта в Team Foundation Server | Документация Майкрософт
author: jrjlee
description: В этом разделе описывается создание нового командного проекта в Team Foundation Server (TFS) 2010.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: 2c3b30cac408f47d7d15ae7456f0744219506c85
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397565"
---
<a name="creating-a-team-project-in-tfs"></a><span data-ttu-id="9a593-103">Создание командного проекта в Team Foundation Server</span><span class="sxs-lookup"><span data-stu-id="9a593-103">Creating a Team Project in TFS</span></span>
====================
<span data-ttu-id="9a593-104">по [Джейсон Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="9a593-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="9a593-105">Загрузить PDF-файл</span><span class="sxs-lookup"><span data-stu-id="9a593-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="9a593-106">В этом разделе описывается создание нового командного проекта в Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="9a593-106">This topic describes how to create a new team project in Team Foundation Server (TFS) 2010.</span></span>


<span data-ttu-id="9a593-107">Этот раздел является частью серии учебников, исходя из требования к развертыванию enterprise вымышленной компании Fabrikam, Inc. В этой серии руководств используется пример решения&#x2014; [решения диспетчера контактов](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;для представления веб-приложения с более реалистичные уровень сложности, включая приложения ASP.NET MVC 3, Windows Communication Служба Foundation (WCF) и проект базы данных.</span><span class="sxs-lookup"><span data-stu-id="9a593-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="9a593-108">Общие сведения о задачах</span><span class="sxs-lookup"><span data-stu-id="9a593-108">Task Overview</span></span>

<span data-ttu-id="9a593-109">Чтобы подготовить и использовать новый командный проект в TFS, необходимо выполнить следующие действия:</span><span class="sxs-lookup"><span data-stu-id="9a593-109">To provision and use a new team project in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="9a593-110">Предоставить разрешения пользователю, которому будет создание нового командного проекта.</span><span class="sxs-lookup"><span data-stu-id="9a593-110">Grant permissions to the user who will create the new team project.</span></span>
- <span data-ttu-id="9a593-111">Создайте командный проект.</span><span class="sxs-lookup"><span data-stu-id="9a593-111">Create the team project.</span></span>
- <span data-ttu-id="9a593-112">Предоставить разрешения членам команды, которые будут работать над проектом.</span><span class="sxs-lookup"><span data-stu-id="9a593-112">Grant permissions to the team members who will work on the project.</span></span>
- <span data-ttu-id="9a593-113">Проверьте в некоторое содержимое.</span><span class="sxs-lookup"><span data-stu-id="9a593-113">Check in some content.</span></span>

<span data-ttu-id="9a593-114">В этом разделе показано, как для выполнения этих процедур, и будет выполнен поиск пользователей и роли заданий, которые могут отвечать за каждой процедуры.</span><span class="sxs-lookup"><span data-stu-id="9a593-114">This topic will show you how to perform these procedures, and it will identify the users and job roles that are likely to be responsible for each procedure.</span></span> <span data-ttu-id="9a593-115">Имейте в виду, что, в зависимости от структуры вашей организации, каждый из этих задач может быть ответственность за другое лицо.</span><span class="sxs-lookup"><span data-stu-id="9a593-115">Be aware that, depending on the structure of your organization, each of these tasks may be the responsibility of a different person.</span></span>

<span data-ttu-id="9a593-116">Задачи и пошаговые руководства в этом разделе предполагается, что вы установили и настроили TFS и что вы создали коллекцию командных проектов как часть процесса настройки.</span><span class="sxs-lookup"><span data-stu-id="9a593-116">The tasks and walkthroughs in this topic assume that you've installed and configured TFS, and that you've created a team project collection as part of the configuration process.</span></span> <span data-ttu-id="9a593-117">Дополнительные сведения на следующих предположениях и дополнительные общие сведения о сценарии, см. в разделе [настроить сервер сборки TFS для развертывания веб-](configuring-a-tfs-build-server-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="9a593-117">For more information on these assumptions, and for more general background information on the scenario, see [Configure a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md).</span></span>

## <a name="grant-permissions-to-the-team-project-creator"></a><span data-ttu-id="9a593-118">Предоставление разрешений для создатель командного проекта</span><span class="sxs-lookup"><span data-stu-id="9a593-118">Grant Permissions to the Team Project Creator</span></span>

<span data-ttu-id="9a593-119">Для создания нового командного проекта, потребуются следующие разрешения:</span><span class="sxs-lookup"><span data-stu-id="9a593-119">In order to create a new team project, you need these permissions:</span></span>

- <span data-ttu-id="9a593-120">Необходимо иметь **создавать новые проекты** разрешение на уровне приложений TFS.</span><span class="sxs-lookup"><span data-stu-id="9a593-120">You must have the **Create new projects** permission on the TFS application tier.</span></span> <span data-ttu-id="9a593-121">Обычно предоставить это разрешение, добавив пользователям **Администраторы коллекции проектов** группу TFS.</span><span class="sxs-lookup"><span data-stu-id="9a593-121">You typically grant this permission by adding users to the **Project Collection Administrators** TFS group.</span></span> <span data-ttu-id="9a593-122">**Администраторы Team Foundation** глобальной группы также включает это разрешение.</span><span class="sxs-lookup"><span data-stu-id="9a593-122">The **Team Foundation Administrators** global group also includes this permission.</span></span>
- <span data-ttu-id="9a593-123">Необходимо иметь разрешение на создание новых сайтов групп внутри семейства узлов SharePoint, соответствующий коллекции командных проектов TFS.</span><span class="sxs-lookup"><span data-stu-id="9a593-123">You must have permission to create new team sites within the SharePoint site collection that corresponds to the TFS team project collection.</span></span> <span data-ttu-id="9a593-124">Обычно предоставьте это разрешение, при добавлении пользователя в группу SharePoint с **полный контроль** семейство веб-сайтов права на сайте SharePoint.</span><span class="sxs-lookup"><span data-stu-id="9a593-124">You typically grant this permission by adding the user to a SharePoint group with **Full Control** rights on the SharePoint site collection.</span></span>
- <span data-ttu-id="9a593-125">Если вы используете SQL Server Reporting Services функции, необходимо быть членом **Диспетчер содержимого Team Foundation** роли в службах Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="9a593-125">If you're using SQL Server Reporting Services features, you must be a member of the **Team Foundation Content Manager** role in Reporting Services.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="9a593-126">Кто выполняет эти процедуры?</span><span class="sxs-lookup"><span data-stu-id="9a593-126">Who Performs These Procedures?</span></span>

<span data-ttu-id="9a593-127">Как правило пользователь или группа, являющегося администратором развертывания TFS также выполняет эти процедуры.</span><span class="sxs-lookup"><span data-stu-id="9a593-127">Typically, the person or group who administers the TFS deployment also performs these procedures.</span></span>

<span data-ttu-id="9a593-128">Так как это более высоким уровнем привилегий набора разрешений, новые командные проекты обычно создаются с небольшой набор пользователей, ответственных за администрирование развертывания TFS.</span><span class="sxs-lookup"><span data-stu-id="9a593-128">Because this is a highly privileged set of permissions, new team projects are typically created by a small subset of users with responsibility for administering a TFS deployment.</span></span> <span data-ttu-id="9a593-129">Разработчики не обычно получают разрешения, необходимые для создания новых командных проектов.</span><span class="sxs-lookup"><span data-stu-id="9a593-129">Developers will not usually be granted the permissions required to create new team projects.</span></span>

### <a name="grant-permissions-in-tfs"></a><span data-ttu-id="9a593-130">GRANT, предоставление разрешений в Team Foundation Server</span><span class="sxs-lookup"><span data-stu-id="9a593-130">Grant Permissions in TFS</span></span>

<span data-ttu-id="9a593-131">Если вы хотите разрешить пользователю создавать командные проекты, первой высокого уровня задачей является добавление пользователю **Администраторы коллекции проектов** группы для коллекции командных проектов.</span><span class="sxs-lookup"><span data-stu-id="9a593-131">If you want to enable a user to create new team projects, the first high-level task is to add the user to the **Project Collection Administrators** group for the team project collection.</span></span>

<span data-ttu-id="9a593-132">**Чтобы добавить пользователя в группу администраторов коллекции проектов**</span><span class="sxs-lookup"><span data-stu-id="9a593-132">**To add a user to the Project Collection Administrators group**</span></span>

1. <span data-ttu-id="9a593-133">На сервере TFS на **запустить** последовательно выберите пункты **все программы**, нажмите кнопку **Microsoft Team Foundation Server 2010**, а затем нажмите кнопку **Team Foundation Консоль администрирования**.</span><span class="sxs-lookup"><span data-stu-id="9a593-133">On the TFS server, on the **Start** menu, point to **All Programs**, click **Microsoft Team Foundation Server 2010**, and then click **Team Foundation Administration Console**.</span></span>
2. <span data-ttu-id="9a593-134">В представлении в виде дерева навигации разверните **уровень приложений**, а затем нажмите кнопку **коллекций командных проектов**.</span><span class="sxs-lookup"><span data-stu-id="9a593-134">In the navigation tree view, expand **Application Tier**, and then click **Team Project Collections**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. <span data-ttu-id="9a593-135">В **коллекций командных проектов** панели, выберите коллекцию, вы хотите управлять командных проектов.</span><span class="sxs-lookup"><span data-stu-id="9a593-135">In the **Team Project Collections** pane, select the team project collection you want to manage.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. <span data-ttu-id="9a593-136">На **Общие** щелкните **членство в группе**.</span><span class="sxs-lookup"><span data-stu-id="9a593-136">On the **General** tab, click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. <span data-ttu-id="9a593-137">В **глобальные группы** выберите **Администраторы коллекции проектов** группу и нажмите **свойства**.</span><span class="sxs-lookup"><span data-stu-id="9a593-137">In the **Global Groups** dialog box, select the **Project Collection Administrators** group, and then click **Properties**.</span></span>
6. <span data-ttu-id="9a593-138">В **свойства группы Team Foundation Server** диалоговом окне выберите **Windows пользователь или группа**, а затем нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="9a593-138">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. <span data-ttu-id="9a593-139">В **Выбор пользователей, компьютеров или групп** диалоговое окно, введите имя пользователя, который вы хотите иметь возможность создавать командные проекты, щелкните **проверить имена**, а затем нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="9a593-139">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to be able to create new team projects, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. <span data-ttu-id="9a593-140">В **свойства группы Team Foundation Server** диалоговом окне щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="9a593-140">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
9. <span data-ttu-id="9a593-141">В **глобальные группы** диалоговом окне щелкните **закрыть**.</span><span class="sxs-lookup"><span data-stu-id="9a593-141">In the **Global Groups** dialog box, click **Close**.</span></span>

### <a name="grant-permissions-in-sharepoint-services"></a><span data-ttu-id="9a593-142">Предоставление разрешений в службах SharePoint</span><span class="sxs-lookup"><span data-stu-id="9a593-142">Grant Permissions in SharePoint Services</span></span>

<span data-ttu-id="9a593-143">Далее необходимо предоставить пользователю разрешение на создание новых сайтов групп в коллекции сайтов SharePoint, соответствующий вашей коллекции командных проектов TFS.</span><span class="sxs-lookup"><span data-stu-id="9a593-143">Next, you need to give the user permission to create new team sites in the SharePoint site collection that corresponds to your TFS team project collection.</span></span>

<span data-ttu-id="9a593-144">**Чтобы предоставить разрешения полного доступа для коллекции сайтов SharePoint**</span><span class="sxs-lookup"><span data-stu-id="9a593-144">**To grant Full Control permissions on the SharePoint site collection**</span></span>

1. <span data-ttu-id="9a593-145">В консоли администрирования Team Foundation Server на **коллекций командных проектов** выберите коллекции командных проектов, которым требуется управлять.</span><span class="sxs-lookup"><span data-stu-id="9a593-145">In the Team Foundation Server Administration Console, on the **Team Project Collections** page, select the team project collection you want to manage.</span></span>
2. <span data-ttu-id="9a593-146">На **сайт SharePoint** вкладке, обратите внимание на значение **текущее местоположение сайта по умолчанию** URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="9a593-146">On the **SharePoint Site** tab, note the value of the **Current Default Site Location** URL.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. <span data-ttu-id="9a593-147">Откройте Internet Explorer, а затем перейдите на URL-адрес, вы записали на шаге 2.</span><span class="sxs-lookup"><span data-stu-id="9a593-147">Open Internet Explorer, and then go to the URL you noted in step 2.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9a593-148">Если не выполнен вход на для Windows от имени пользователя, создавшего коллекцию командных проектов, необходимо войти в SharePoint от имени этого пользователя для продолжения.</span><span class="sxs-lookup"><span data-stu-id="9a593-148">If you're not logged on to Windows as the user who created the team project collection, you'll need to sign in to SharePoint as this user in order to continue.</span></span>
4. <span data-ttu-id="9a593-149">На **действия сайта** меню, щелкните **параметры сайта**.</span><span class="sxs-lookup"><span data-stu-id="9a593-149">On the **Site Actions** menu, click **Site Settings**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. <span data-ttu-id="9a593-150">На **параметры сайта** раздела **пользователями и разрешениями**, нажмите кнопку **пользователи и группы**.</span><span class="sxs-lookup"><span data-stu-id="9a593-150">On the **Site Settings** page, under **Users and Permissions**, click **People and groups**.</span></span>
6. <span data-ttu-id="9a593-151">На панели навигации слева щелкните элемент **группы**.</span><span class="sxs-lookup"><span data-stu-id="9a593-151">In the left navigation panel, click **Groups**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. <span data-ttu-id="9a593-152">На **пользователи и группы: все группы** щелкните **настроить группы для этого сайта**.</span><span class="sxs-lookup"><span data-stu-id="9a593-152">On the **People and Groups: All Groups** page, click **Set Up Groups for this Site**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > <span data-ttu-id="9a593-153">Может появиться <strong>HTTP 404 не найдено</strong> ошибка из-за ошибки кодирования double HTTP.</span><span class="sxs-lookup"><span data-stu-id="9a593-153">You may receive an <strong>HTTP 404 Not Found</strong> error due to a double HTTP encoding bug.</span></span> <span data-ttu-id="9a593-154">В этом случае замените URL-адрес с этим:</span><span class="sxs-lookup"><span data-stu-id="9a593-154">If this occurs, replace the URL with this:</span></span>   
   > <span data-ttu-id="9a593-155">`[site_collection_URL]/_layouts/permsetup.aspx` Например:</span><span class="sxs-lookup"><span data-stu-id="9a593-155">`[site_collection_URL]/_layouts/permsetup.aspx` For example:</span></span>  
   > `http://tfs/sites/Fabrikam%20Web%20Projects/_layouts/permsetup.aspx` 
8. <span data-ttu-id="9a593-156">На **настроить группы для этого сайта** странице, пользователь, который будет создавать командные проекты, чтобы добавить **владельцев** группу и нажмите **ОК**.</span><span class="sxs-lookup"><span data-stu-id="9a593-156">On the **Set Up Groups for this Site** page, add the user who will create team projects to the **Owners** group, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image10.png)

<span data-ttu-id="9a593-157">Дополнительные сведения о включении пользователям создавать командные проекты в коллекции командных проектов см. в разделе [Установка прав администратора для коллекций командных проектов](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="9a593-157">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span>

## <a name="create-a-new-team-project-and-add-users"></a><span data-ttu-id="9a593-158">Создание нового командного проекта и добавление пользователей</span><span class="sxs-lookup"><span data-stu-id="9a593-158">Create a New Team Project and Add Users</span></span>

<span data-ttu-id="9a593-159">При наличии необходимых разрешений можно использовать **Team Explorer** окно в Visual Studio 2010 для создания нового командного проекта.</span><span class="sxs-lookup"><span data-stu-id="9a593-159">Once you have the necessary permissions, you can use the **Team Explorer** window in Visual Studio 2010 to create a new team project.</span></span> <span data-ttu-id="9a593-160">Такой подход предоставляет мастер, который собирает все необходимые данные и выполняет необходимые задачи в TFS, SharePoint и SQL Server Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="9a593-160">This approach provides a wizard that collects all the required information and performs the necessary tasks in TFS, SharePoint, and SQL Server Reporting Services.</span></span> <span data-ttu-id="9a593-161">Также необходимо предоставить разрешения для нового командного проекта участникам команды разработчиков, чтобы включить их для добавления и изменения содержимого.</span><span class="sxs-lookup"><span data-stu-id="9a593-161">You'll also need to grant permissions on the new team project to members of the developer team, to enable them to add and modify content.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="9a593-162">Кто выполняет эти процедуры?</span><span class="sxs-lookup"><span data-stu-id="9a593-162">Who Performs These Procedures?</span></span>

<span data-ttu-id="9a593-163">Обычно администратор TFS или руководитель группы разработчиков выполнение этих процедур.</span><span class="sxs-lookup"><span data-stu-id="9a593-163">Usually either a TFS administrator or a developer team leader performs these procedures.</span></span>

### <a name="create-a-new-team-project"></a><span data-ttu-id="9a593-164">Создание нового командного проекта</span><span class="sxs-lookup"><span data-stu-id="9a593-164">Create a New Team Project</span></span>

<span data-ttu-id="9a593-165">Далее описано, как создать новый командный проект в Team Foundation Server 2010.</span><span class="sxs-lookup"><span data-stu-id="9a593-165">The next procedure describes how to create a new team project in TFS 2010.</span></span>

<span data-ttu-id="9a593-166">**Для создания нового командного проекта**</span><span class="sxs-lookup"><span data-stu-id="9a593-166">**To create a new team project**</span></span>

1. <span data-ttu-id="9a593-167">На **запустить** последовательно выберите пункты **все программы**, нажмите кнопку **Microsoft Visual Studio 2010**, щелкните правой кнопкой мыши **Microsoft Visual Studio 2010**, а затем нажмите кнопку **Запуск от имени администратора**.</span><span class="sxs-lookup"><span data-stu-id="9a593-167">On the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, right-click **Microsoft Visual Studio 2010**, and then click **Run as administrator**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9a593-168">Если не выполнить Visual Studio 2010 с правами администратора, на последний шаг не удастся мастеру создания командного проекта.</span><span class="sxs-lookup"><span data-stu-id="9a593-168">If you don't run Visual Studio 2010 as an administrator, the New Team Project Wizard will fail on the last step.</span></span>
2. <span data-ttu-id="9a593-169">Если **контроль учетных записей пользователей** появится диалоговое окно, нажмите кнопку **Да**.</span><span class="sxs-lookup"><span data-stu-id="9a593-169">If the **User Account Control** dialog box appears, click **Yes**.</span></span>
3. <span data-ttu-id="9a593-170">В Visual Studio на **Team** меню, щелкните **подключиться к Team Foundation Server**.</span><span class="sxs-lookup"><span data-stu-id="9a593-170">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9a593-171">Если вы уже настроили подключение к серверу TFS, можно пропустить шаги 4 – 7.</span><span class="sxs-lookup"><span data-stu-id="9a593-171">If you have already configured a connection to a TFS server, you can omit steps 4-7.</span></span>
4. <span data-ttu-id="9a593-172">В **подключение к командному проекту** диалоговом окне щелкните **серверов**.</span><span class="sxs-lookup"><span data-stu-id="9a593-172">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
5. <span data-ttu-id="9a593-173">В **Добавление или удаление Team Foundation Server** диалоговом окне щелкните **добавить**.</span><span class="sxs-lookup"><span data-stu-id="9a593-173">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
6. <span data-ttu-id="9a593-174">В **добавить Team Foundation Server** диалоговом окне укажите сведения об экземпляре TFS, а затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="9a593-174">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. <span data-ttu-id="9a593-175">В **Добавление или удаление Team Foundation Server** диалоговом окне щелкните **закрыть**.</span><span class="sxs-lookup"><span data-stu-id="9a593-175">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
8. <span data-ttu-id="9a593-176">В **подключение к командному проекту** диалоговом окне выберите коллекцию, вы хотите добавить и нажмите кнопку проектов экземпляре TFS, вы хотите подключиться, выберите команду **Connect**.</span><span class="sxs-lookup"><span data-stu-id="9a593-176">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection you want to add to, and then click **Connect**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. <span data-ttu-id="9a593-177">В **Team Explorer** окно, щелкните правой кнопкой мыши командного проекта коллекции, а затем нажмите кнопку **нового командного проекта**.</span><span class="sxs-lookup"><span data-stu-id="9a593-177">In the **Team Explorer** window, right-click the team project collection, and then click **New Team Project**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. <span data-ttu-id="9a593-178">В **нового командного проекта** диалоговом окне укажите имя и описание для командного проекта и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="9a593-178">In the **New Team Project** dialog box, provide a name and a description for the team project, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9a593-179">Если командный проект содержит пробелы, вы можете столкнуться некоторые проблемы при переходе используйте средство Internet Information Services (IIS) веб-развертывания (веб-развертывание) для развертывания пакетов из выходной путь.</span><span class="sxs-lookup"><span data-stu-id="9a593-179">If your team project includes spaces, you may face some issues when you come to use the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to deploy packages from the output path.</span></span> <span data-ttu-id="9a593-180">Пробелы в пути можно сделать гораздо труднее — для выполнения команд на веб-развертывания.</span><span class="sxs-lookup"><span data-stu-id="9a593-180">Spaces in the path can make it a lot more difficult to run Web Deploy commands.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. <span data-ttu-id="9a593-181">На **Выбор шаблона процесса** выберите шаблон процесса, который вы хотите использовать для управления процессом разработки и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="9a593-181">On the **Select a Process Template** page, select the process template that you want to use to manage the development process, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9a593-182">Дополнительные сведения о шаблонах процессов для TFS, см. в разделе [шаблоны процессов и инструменты](https://msdn.microsoft.com/vstudio/aa718795).</span><span class="sxs-lookup"><span data-stu-id="9a593-182">For more information on process templates for TFS, see [Process Templates and Tools](https://msdn.microsoft.com/vstudio/aa718795).</span></span>
12. <span data-ttu-id="9a593-183">На **параметры сайта группы** странице, оставьте без изменений параметры по умолчанию и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="9a593-183">On the **Team Site Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
13. <span data-ttu-id="9a593-184">Этот параметр создает или определяет, сайт группы SharePoint, который связан с командным проектом TFS.</span><span class="sxs-lookup"><span data-stu-id="9a593-184">This setting creates, or identifies, a SharePoint team site that is associated with the TFS team project.</span></span> <span data-ttu-id="9a593-185">Ваша команда разработчиков может использовать этот сайт для управления документацией, участвовать в беседах, создание вики-страниц и выполнять различные задачи, которые не связаны с кодом.</span><span class="sxs-lookup"><span data-stu-id="9a593-185">Your development team can use this site to manage documentation, participate in discussion threads, create wiki pages, and perform various other tasks that are not related to code.</span></span> <span data-ttu-id="9a593-186">Дополнительные сведения см. в разделе [взаимодействий между продуктов SharePoint и Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span><span class="sxs-lookup"><span data-stu-id="9a593-186">For more information, see [Interactions Between SharePoint Products and Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span></span>
14. <span data-ttu-id="9a593-187">На **укажите параметры системы управления версиями** странице, оставьте без изменений параметры по умолчанию и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="9a593-187">On the **Specify Source Control Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
15. <span data-ttu-id="9a593-188">Этот параметр определяет или создает расположение в иерархии папок TFS, который будет выступать в качестве корневой папки для содержимого.</span><span class="sxs-lookup"><span data-stu-id="9a593-188">This setting identifies or creates the location in the TFS folder hierarchy that will act as a root folder for your content.</span></span>
16. <span data-ttu-id="9a593-189">На **подтверждение настроек командного проекта** щелкните **Готово**.</span><span class="sxs-lookup"><span data-stu-id="9a593-189">On the **Confirm Team Project Settings** page, click **Finish**.</span></span>
17. <span data-ttu-id="9a593-190">Когда новый командный проект создан успешно, на **командный проект создан** щелкните **закрыть**.</span><span class="sxs-lookup"><span data-stu-id="9a593-190">When the new team project is successfully created, on the **Team Project Created** page, click **Close**.</span></span>

### <a name="add-users-to-a-team-project"></a><span data-ttu-id="9a593-191">Добавление пользователей в командный проект</span><span class="sxs-lookup"><span data-stu-id="9a593-191">Add Users to a Team Project</span></span>

<span data-ttu-id="9a593-192">Теперь, после создания нового командного проекта, можно предоставить разрешения для пользователей, чтобы включить их, чтобы начать добавление и совместная работа над содержимым.</span><span class="sxs-lookup"><span data-stu-id="9a593-192">Now that you've created the new team project, you can grant permissions to users to enable them to start adding and collaborating on content.</span></span>

<span data-ttu-id="9a593-193">**Чтобы добавить пользователей в командный проект**</span><span class="sxs-lookup"><span data-stu-id="9a593-193">**To add users to a team project**</span></span>

1. <span data-ttu-id="9a593-194">В Visual Studio 2010 в **Team Explorer** окно, щелкните правой кнопкой мыши командный проект, выберите пункт **параметры командного проекта**, а затем нажмите кнопку **членство в группе**.</span><span class="sxs-lookup"><span data-stu-id="9a593-194">In Visual Studio 2010, in the **Team Explorer** window, right-click the team project, point to **Team Project Settings**, and then click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. <span data-ttu-id="9a593-195">Чтобы пользователи могут добавлять, изменять и удалять код, в системе управления версиями, необходимо добавить его к **участники** группы.</span><span class="sxs-lookup"><span data-stu-id="9a593-195">To enable a user to add, modify, and remove code under source control, you need to add him or her to the **Contributors** group.</span></span>
3. <span data-ttu-id="9a593-196">В **группы проекта** выберите **участники** группу и нажмите **свойства**.</span><span class="sxs-lookup"><span data-stu-id="9a593-196">In the **Project Groups** dialog box, select the **Contributors** group, and then click **Properties**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. <span data-ttu-id="9a593-197">В **свойства группы Team Foundation Server** диалоговом окне выберите **Windows пользователь или группа**, а затем нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="9a593-197">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. <span data-ttu-id="9a593-198">В **Выбор пользователей, компьютеров или групп** диалоговое окно, введите имя пользователя, который вы хотите добавить в командный проект, нажмите кнопку **проверить имена**, а затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="9a593-198">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to add to the team project, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. <span data-ttu-id="9a593-199">В **свойства группы Team Foundation Server** диалоговом окне щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="9a593-199">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
7. <span data-ttu-id="9a593-200">В **группы проекта** диалоговом окне щелкните **закрыть**.</span><span class="sxs-lookup"><span data-stu-id="9a593-200">In the **Project Groups** dialog box, click **Close**.</span></span>

## <a name="conclusion"></a><span data-ttu-id="9a593-201">Заключение</span><span class="sxs-lookup"><span data-stu-id="9a593-201">Conclusion</span></span>

<span data-ttu-id="9a593-202">На этом этапе новый командный проект готов к использованию, и ваша команда разработчиков можно начать добавление содержимого и совместная работа на процесс разработки.</span><span class="sxs-lookup"><span data-stu-id="9a593-202">At this point, your new team project is ready to use, and your developer team can start adding content and collaborating on the development process.</span></span>

<span data-ttu-id="9a593-203">Следующий раздел, [Добавление содержимого в системе управления версиями](adding-content-to-source-control.md), описывает способы добавления содержимого в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="9a593-203">The next topic, [Adding Content to Source Control](adding-content-to-source-control.md), describes how to add content to source control.</span></span>

## <a name="further-reading"></a><span data-ttu-id="9a593-204">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="9a593-204">Further Reading</span></span>

<span data-ttu-id="9a593-205">Более широкие рекомендации по созданию командных проектов в TFS см. в разделе [создать командный проект](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="9a593-205">For broader guidance on creating team projects in TFS, see [Create a Team Project](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span></span> <span data-ttu-id="9a593-206">Дополнительные сведения о включении пользователям создавать командные проекты в коллекции командных проектов см. в разделе [Установка прав администратора для коллекций командных проектов](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="9a593-206">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span> <span data-ttu-id="9a593-207">Дополнительные сведения о добавлении пользователей в командные проекты, см. в разделе [Добавление пользователей в командные проекты](https://msdn.microsoft.com/library/bb558971.aspx).</span><span class="sxs-lookup"><span data-stu-id="9a593-207">For more information on adding users to team projects, see [Add Users to Team Projects](https://msdn.microsoft.com/library/bb558971.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9a593-208">[Назад](configuring-team-foundation-server-for-web-deployment.md)
> [Вперед](adding-content-to-source-control.md)</span><span class="sxs-lookup"><span data-stu-id="9a593-208">[Previous](configuring-team-foundation-server-for-web-deployment.md)
[Next](adding-content-to-source-control.md)</span></span>
