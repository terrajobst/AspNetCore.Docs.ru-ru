---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: Создание командного проекта в Team Foundation Server | Документы Microsoft
author: jrjlee
description: В этом разделе описывается создание нового командного проекта в Team Foundation Server (TFS) 2010.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: 79c069a601c0eafd84ae142241895428052acd29
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-team-project-in-tfs"></a><span data-ttu-id="b0523-103">Создание командного проекта в Team Foundation Server</span><span class="sxs-lookup"><span data-stu-id="b0523-103">Creating a Team Project in TFS</span></span>
====================
<span data-ttu-id="b0523-104">по [Джейсон Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="b0523-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="b0523-105">Скачать PDF</span><span class="sxs-lookup"><span data-stu-id="b0523-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="b0523-106">В этом разделе описывается создание нового командного проекта в Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="b0523-106">This topic describes how to create a new team project in Team Foundation Server (TFS) 2010.</span></span>


<span data-ttu-id="b0523-107">Этот раздел входит в состав серию учебников, исходя из требования к развертыванию enterprise вымышленная компания Fabrikam, Inc. Этот учебник ряд использует образец решения&#x2014; [решения диспетчера контактов](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;для представления веб-приложения с реалистичных уровень сложности, включая приложения ASP.NET MVC 3, Windows Communication Служба Foundation (WCF) и проект базы данных.</span><span class="sxs-lookup"><span data-stu-id="b0523-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="b0523-108">Общие сведения о задаче</span><span class="sxs-lookup"><span data-stu-id="b0523-108">Task Overview</span></span>

<span data-ttu-id="b0523-109">Подготовить и использовать новый командный проект в TFS, необходимо выполнить следующие общие действия:</span><span class="sxs-lookup"><span data-stu-id="b0523-109">To provision and use a new team project in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="b0523-110">Предоставить разрешения пользователю, который будет создан новый командный проект.</span><span class="sxs-lookup"><span data-stu-id="b0523-110">Grant permissions to the user who will create the new team project.</span></span>
- <span data-ttu-id="b0523-111">Создайте командный проект.</span><span class="sxs-lookup"><span data-stu-id="b0523-111">Create the team project.</span></span>
- <span data-ttu-id="b0523-112">Предоставить разрешения членам команды, которые будут работать в проекте.</span><span class="sxs-lookup"><span data-stu-id="b0523-112">Grant permissions to the team members who will work on the project.</span></span>
- <span data-ttu-id="b0523-113">Проверка некоторое содержимое.</span><span class="sxs-lookup"><span data-stu-id="b0523-113">Check in some content.</span></span>

<span data-ttu-id="b0523-114">В этом разделе будет показано, как для выполнения этих процедур, и будет идентифицировать пользователей и роли задания, которые могут отвечать за каждой процедуры.</span><span class="sxs-lookup"><span data-stu-id="b0523-114">This topic will show you how to perform these procedures, and it will identify the users and job roles that are likely to be responsible for each procedure.</span></span> <span data-ttu-id="b0523-115">Имейте в виду, что, в зависимости от структуры организации, каждая из этих задач может быть ответственность за другое лицо.</span><span class="sxs-lookup"><span data-stu-id="b0523-115">Be aware that, depending on the structure of your organization, each of these tasks may be the responsibility of a different person.</span></span>

<span data-ttu-id="b0523-116">Задачи и пошаговых инструкций в этом разделе предполагается, что установлен и настроен TFS, и вы создали коллекцию командных проектов в рамках процесса настройки.</span><span class="sxs-lookup"><span data-stu-id="b0523-116">The tasks and walkthroughs in this topic assume that you've installed and configured TFS, and that you've created a team project collection as part of the configuration process.</span></span> <span data-ttu-id="b0523-117">Дополнительные сведения об этих предположений и общие сведения о сценарии см. раздел [Настройка сервера сборки TFS для развертывания веб-](configuring-a-tfs-build-server-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="b0523-117">For more information on these assumptions, and for more general background information on the scenario, see [Configure a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md).</span></span>

## <a name="grant-permissions-to-the-team-project-creator"></a><span data-ttu-id="b0523-118">Предоставление разрешений создатель командного проекта</span><span class="sxs-lookup"><span data-stu-id="b0523-118">Grant Permissions to the Team Project Creator</span></span>

<span data-ttu-id="b0523-119">Для создания нового командного проекта, потребуются следующие разрешения:</span><span class="sxs-lookup"><span data-stu-id="b0523-119">In order to create a new team project, you need these permissions:</span></span>

- <span data-ttu-id="b0523-120">Необходимо иметь **создавать новые проекты** разрешение на уровне приложений TFS.</span><span class="sxs-lookup"><span data-stu-id="b0523-120">You must have the **Create new projects** permission on the TFS application tier.</span></span> <span data-ttu-id="b0523-121">Обычно предоставить это разрешение, добавив пользователям **Администраторы коллекции проектов** группы TFS.</span><span class="sxs-lookup"><span data-stu-id="b0523-121">You typically grant this permission by adding users to the **Project Collection Administrators** TFS group.</span></span> <span data-ttu-id="b0523-122">**Администраторы Team Foundation** глобальной группы также включает это разрешение.</span><span class="sxs-lookup"><span data-stu-id="b0523-122">The **Team Foundation Administrators** global group also includes this permission.</span></span>
- <span data-ttu-id="b0523-123">Необходимо иметь разрешение на создание новых сайтов командных внутри коллекции сайтов SharePoint, который соответствует коллекции командного проекта TFS.</span><span class="sxs-lookup"><span data-stu-id="b0523-123">You must have permission to create new team sites within the SharePoint site collection that corresponds to the TFS team project collection.</span></span> <span data-ttu-id="b0523-124">Обычно его можно предоставить путем добавления пользователя в группу SharePoint с **полный доступ** права на SharePoint, семейство веб-сайтов.</span><span class="sxs-lookup"><span data-stu-id="b0523-124">You typically grant this permission by adding the user to a SharePoint group with **Full Control** rights on the SharePoint site collection.</span></span>
- <span data-ttu-id="b0523-125">При использовании функции SQL Server Reporting Services, необходимо быть членом **Диспетчер содержимого Team Foundation** роли в службах Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="b0523-125">If you're using SQL Server Reporting Services features, you must be a member of the **Team Foundation Content Manager** role in Reporting Services.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="b0523-126">Кто выполняет эти процедуры?</span><span class="sxs-lookup"><span data-stu-id="b0523-126">Who Performs These Procedures?</span></span>

<span data-ttu-id="b0523-127">Как правило у пользователя или группы, являющегося администратором развертывания TFS также выполняет эти процедуры.</span><span class="sxs-lookup"><span data-stu-id="b0523-127">Typically, the person or group who administers the TFS deployment also performs these procedures.</span></span>

<span data-ttu-id="b0523-128">Поскольку это привилегированным набор разрешений, новые командные проекты обычно создаются небольшое подмножество пользователей с ответственность за администрирование развертывания TFS.</span><span class="sxs-lookup"><span data-stu-id="b0523-128">Because this is a highly privileged set of permissions, new team projects are typically created by a small subset of users with responsibility for administering a TFS deployment.</span></span> <span data-ttu-id="b0523-129">Разработчики не обычно предоставляются разрешения на создание нового командного проекта.</span><span class="sxs-lookup"><span data-stu-id="b0523-129">Developers will not usually be granted the permissions required to create new team projects.</span></span>

### <a name="grant-permissions-in-tfs"></a><span data-ttu-id="b0523-130">Предоставление разрешений в Team Foundation Server</span><span class="sxs-lookup"><span data-stu-id="b0523-130">Grant Permissions in TFS</span></span>

<span data-ttu-id="b0523-131">Если вы хотите разрешить пользователям создавать новые командные проекты, первая задача высокого уровня является добавление пользователю **Администраторы коллекции проектов** для коллекции командных проектов.</span><span class="sxs-lookup"><span data-stu-id="b0523-131">If you want to enable a user to create new team projects, the first high-level task is to add the user to the **Project Collection Administrators** group for the team project collection.</span></span>

<span data-ttu-id="b0523-132">**Чтобы добавить пользователя в группу администраторов коллекции проектов**</span><span class="sxs-lookup"><span data-stu-id="b0523-132">**To add a user to the Project Collection Administrators group**</span></span>

1. <span data-ttu-id="b0523-133">На сервере TFS на **запустить** последовательно выберите пункты **все программы**, нажмите кнопку **Microsoft Team Foundation Server 2010**, а затем нажмите кнопку **Team Foundation Консоль администрирования**.</span><span class="sxs-lookup"><span data-stu-id="b0523-133">On the TFS server, on the **Start** menu, point to **All Programs**, click **Microsoft Team Foundation Server 2010**, and then click **Team Foundation Administration Console**.</span></span>
2. <span data-ttu-id="b0523-134">В дереве навигации, разверните **уровня приложения**, а затем нажмите кнопку **коллекций командных проектов**.</span><span class="sxs-lookup"><span data-stu-id="b0523-134">In the navigation tree view, expand **Application Tier**, and then click **Team Project Collections**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. <span data-ttu-id="b0523-135">В **коллекций командных проектов** выберите коллекцию, которым требуется управлять командных проектов.</span><span class="sxs-lookup"><span data-stu-id="b0523-135">In the **Team Project Collections** pane, select the team project collection you want to manage.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. <span data-ttu-id="b0523-136">На **Общие** щелкните **членство в группе**.</span><span class="sxs-lookup"><span data-stu-id="b0523-136">On the **General** tab, click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. <span data-ttu-id="b0523-137">В **глобальных групп** выберите **Администраторы коллекции проектов** группы, а затем нажмите кнопку **свойства**.</span><span class="sxs-lookup"><span data-stu-id="b0523-137">In the **Global Groups** dialog box, select the **Project Collection Administrators** group, and then click **Properties**.</span></span>
6. <span data-ttu-id="b0523-138">В **свойства группы Team Foundation Server** выберите **пользователя или группы Windows**, а затем нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="b0523-138">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. <span data-ttu-id="b0523-139">В **Выбор пользователей, компьютеров или групп** диалоговое окно, введите имя пользователя, необходимо иметь возможность создавать новые командные проекты, щелкните **проверить имена**, а затем нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="b0523-139">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to be able to create new team projects, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. <span data-ttu-id="b0523-140">В **свойства группы Team Foundation Server** диалоговое окно, нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="b0523-140">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
9. <span data-ttu-id="b0523-141">В **глобальных групп** диалоговое окно, нажмите кнопку **закрыть**.</span><span class="sxs-lookup"><span data-stu-id="b0523-141">In the **Global Groups** dialog box, click **Close**.</span></span>

### <a name="grant-permissions-in-sharepoint-services"></a><span data-ttu-id="b0523-142">Предоставление разрешений в службах SharePoint</span><span class="sxs-lookup"><span data-stu-id="b0523-142">Grant Permissions in SharePoint Services</span></span>

<span data-ttu-id="b0523-143">Далее необходимо предоставить пользователю соответствующие разрешения для создания новых сайтов командных коллекции веб-сайта SharePoint, который соответствует коллекции командного проекта TFS.</span><span class="sxs-lookup"><span data-stu-id="b0523-143">Next, you need to give the user permission to create new team sites in the SharePoint site collection that corresponds to your TFS team project collection.</span></span>

<span data-ttu-id="b0523-144">**Чтобы предоставить разрешения на полный доступ для коллекции сайтов SharePoint**</span><span class="sxs-lookup"><span data-stu-id="b0523-144">**To grant Full Control permissions on the SharePoint site collection**</span></span>

1. <span data-ttu-id="b0523-145">В консоли администрирования Team Foundation Server на **коллекций командных проектов** выберите коллекции командных проектов, которым требуется управлять.</span><span class="sxs-lookup"><span data-stu-id="b0523-145">In the Team Foundation Server Administration Console, on the **Team Project Collections** page, select the team project collection you want to manage.</span></span>
2. <span data-ttu-id="b0523-146">На **сайт SharePoint** запомните значение **текущее местоположение сайта по умолчанию** URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="b0523-146">On the **SharePoint Site** tab, note the value of the **Current Default Site Location** URL.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. <span data-ttu-id="b0523-147">Откройте Internet Explorer, а затем перейдите по URL-адресу, указанных в шаге 2.</span><span class="sxs-lookup"><span data-stu-id="b0523-147">Open Internet Explorer, and then go to the URL you noted in step 2.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b0523-148">Если вы не вошли в Windows как пользователь, создавший коллекции командных проектов, необходимо выполнить вход в SharePoint от имени этого пользователя для продолжения.</span><span class="sxs-lookup"><span data-stu-id="b0523-148">If you're not logged on to Windows as the user who created the team project collection, you'll need to sign in to SharePoint as this user in order to continue.</span></span>
4. <span data-ttu-id="b0523-149">На **действия сайта** меню, нажмите кнопку **параметры сайта**.</span><span class="sxs-lookup"><span data-stu-id="b0523-149">On the **Site Actions** menu, click **Site Settings**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. <span data-ttu-id="b0523-150">На **параметры сайта** в разделе **пользователей и разрешений**, нажмите кнопку **пользователей и групп**.</span><span class="sxs-lookup"><span data-stu-id="b0523-150">On the **Site Settings** page, under **Users and Permissions**, click **People and groups**.</span></span>
6. <span data-ttu-id="b0523-151">На панели навигации слева щелкните элемент **группы**.</span><span class="sxs-lookup"><span data-stu-id="b0523-151">In the left navigation panel, click **Groups**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. <span data-ttu-id="b0523-152">На **пользователи и группы: все группы** щелкните **настроить группы для этого сайта**.</span><span class="sxs-lookup"><span data-stu-id="b0523-152">On the **People and Groups: All Groups** page, click **Set Up Groups for this Site**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > <span data-ttu-id="b0523-153">Может появиться <strong>HTTP 404 не найдено</strong> ошибка из-за двойной ошибки кодирования HTTP.</span><span class="sxs-lookup"><span data-stu-id="b0523-153">You may receive an <strong>HTTP 404 Not Found</strong> error due to a double HTTP encoding bug.</span></span> <span data-ttu-id="b0523-154">В этом случае замените URL-адрес с этим:</span><span class="sxs-lookup"><span data-stu-id="b0523-154">If this occurs, replace the URL with this:</span></span>   
   > <span data-ttu-id="b0523-155">[<em>URL-адрес коллекции сайта</em>] /\_layouts/permsetup.aspx</span><span class="sxs-lookup"><span data-stu-id="b0523-155">[<em>site collection URL</em>]/\_layouts/permsetup.aspx</span></span>  
   > <span data-ttu-id="b0523-156">Пример:</span><span class="sxs-lookup"><span data-stu-id="b0523-156">For example:</span></span>  
   > <span data-ttu-id="b0523-157">http://tfs/sites/Fabrikam%20Web%20Projects/\_layouts/permsetup.aspx</span><span class="sxs-lookup"><span data-stu-id="b0523-157">http://tfs/sites/Fabrikam%20Web%20Projects/\_layouts/permsetup.aspx</span></span>
8. <span data-ttu-id="b0523-158">На **настроить группы для этого сайта** страницы, добавьте пользователя, который будет создавать командные проекты для **владельцев** группы, а затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="b0523-158">On the **Set Up Groups for this Site** page, add the user who will create team projects to the **Owners** group, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image10.png)

<span data-ttu-id="b0523-159">Дополнительные сведения о включении пользователям создавать новые командные проекты в коллекции командных проектов см. в разделе [Установка прав администратора для коллекций командных проектов](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="b0523-159">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span>

## <a name="create-a-new-team-project-and-add-users"></a><span data-ttu-id="b0523-160">Создание нового командного проекта и добавление пользователей</span><span class="sxs-lookup"><span data-stu-id="b0523-160">Create a New Team Project and Add Users</span></span>

<span data-ttu-id="b0523-161">При наличии необходимых разрешений можно использовать **Team Explorer** окна в Visual Studio 2010 для создания нового командного проекта.</span><span class="sxs-lookup"><span data-stu-id="b0523-161">Once you have the necessary permissions, you can use the **Team Explorer** window in Visual Studio 2010 to create a new team project.</span></span> <span data-ttu-id="b0523-162">Такой подход предоставляет мастер, который собирает всю необходимую информацию и выполняет необходимые задачи в TFS, SharePoint и SQL Server Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="b0523-162">This approach provides a wizard that collects all the required information and performs the necessary tasks in TFS, SharePoint, and SQL Server Reporting Services.</span></span> <span data-ttu-id="b0523-163">Также необходимо предоставить разрешения на новый командный проект, чтобы члены команды разработчиков, чтобы включить их для добавления и изменения содержимого.</span><span class="sxs-lookup"><span data-stu-id="b0523-163">You'll also need to grant permissions on the new team project to members of the developer team, to enable them to add and modify content.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="b0523-164">Кто выполняет эти процедуры?</span><span class="sxs-lookup"><span data-stu-id="b0523-164">Who Performs These Procedures?</span></span>

<span data-ttu-id="b0523-165">Обычно администратор TFS или руководитель группы разработчиков, выполняет эти процедуры.</span><span class="sxs-lookup"><span data-stu-id="b0523-165">Usually either a TFS administrator or a developer team leader performs these procedures.</span></span>

### <a name="create-a-new-team-project"></a><span data-ttu-id="b0523-166">Создание нового командного проекта</span><span class="sxs-lookup"><span data-stu-id="b0523-166">Create a New Team Project</span></span>

<span data-ttu-id="b0523-167">Далее описано, как для создания нового командного проекта в Team Foundation Server 2010.</span><span class="sxs-lookup"><span data-stu-id="b0523-167">The next procedure describes how to create a new team project in TFS 2010.</span></span>

<span data-ttu-id="b0523-168">**Для создания нового командного проекта**</span><span class="sxs-lookup"><span data-stu-id="b0523-168">**To create a new team project**</span></span>

1. <span data-ttu-id="b0523-169">На **запустить** последовательно выберите пункты **все программы**, нажмите кнопку **Microsoft Visual Studio 2010**, щелкните правой кнопкой мыши **Microsoft Visual Studio 2010**, затем **Запуск от имени администратора**.</span><span class="sxs-lookup"><span data-stu-id="b0523-169">On the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, right-click **Microsoft Visual Studio 2010**, and then click **Run as administrator**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b0523-170">Если не запустить Visual Studio 2010 с правами администратора, мастер нового командного проекта на последний шаг не удастся.</span><span class="sxs-lookup"><span data-stu-id="b0523-170">If you don't run Visual Studio 2010 as an administrator, the New Team Project Wizard will fail on the last step.</span></span>
2. <span data-ttu-id="b0523-171">Если **контроль учетных записей пользователей** диалоговое окно, нажмите кнопку **Да**.</span><span class="sxs-lookup"><span data-stu-id="b0523-171">If the **User Account Control** dialog box appears, click **Yes**.</span></span>
3. <span data-ttu-id="b0523-172">В Visual Studio на **команды** меню, нажмите кнопку **подключиться к Team Foundation Server**.</span><span class="sxs-lookup"><span data-stu-id="b0523-172">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b0523-173">Если вы уже настроили соединение с сервером TFS, можно пропустить шаги 4 – 7.</span><span class="sxs-lookup"><span data-stu-id="b0523-173">If you have already configured a connection to a TFS server, you can omit steps 4-7.</span></span>
4. <span data-ttu-id="b0523-174">В **подключение к командному проекту** диалоговое окно, нажмите кнопку **серверов**.</span><span class="sxs-lookup"><span data-stu-id="b0523-174">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
5. <span data-ttu-id="b0523-175">В **Добавление или удаление Team Foundation Server** диалоговое окно, нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="b0523-175">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
6. <span data-ttu-id="b0523-176">В **Добавление Team Foundation Server** диалоговое окно «» укажите сведения об экземпляре TFS и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="b0523-176">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. <span data-ttu-id="b0523-177">В **Добавление или удаление Team Foundation Server** диалоговое окно, нажмите кнопку **закрыть**.</span><span class="sxs-lookup"><span data-stu-id="b0523-177">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
8. <span data-ttu-id="b0523-178">В **подключение к командному проекту** установите флажок экземпляр TFS, необходимо подключиться, выберите команду проект коллекции, которые необходимо добавить, а затем нажмите кнопку **Connect**.</span><span class="sxs-lookup"><span data-stu-id="b0523-178">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection you want to add to, and then click **Connect**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. <span data-ttu-id="b0523-179">В **Team Explorer** окно, щелкните правой кнопкой мыши команда коллекции проектов, а затем нажмите кнопку **нового командного проекта**.</span><span class="sxs-lookup"><span data-stu-id="b0523-179">In the **Team Explorer** window, right-click the team project collection, and then click **New Team Project**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. <span data-ttu-id="b0523-180">В **нового командного проекта** диалоговое окно, введите имя и описание для командного проекта и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="b0523-180">In the **New Team Project** dialog box, provide a name and a description for the team project, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b0523-181">Если командный проект содержит пробелы, могут возникнуть некоторые проблемы при выполнении использовать средство Internet Information Services (IIS) веб-развертывания (Web Deploy) для развертывания пакетов из выходной путь.</span><span class="sxs-lookup"><span data-stu-id="b0523-181">If your team project includes spaces, you may face some issues when you come to use the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to deploy packages from the output path.</span></span> <span data-ttu-id="b0523-182">Пробелы в пути можно сделать гораздо труднее, для выполнения команд веб-развертывания.</span><span class="sxs-lookup"><span data-stu-id="b0523-182">Spaces in the path can make it a lot more difficult to run Web Deploy commands.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. <span data-ttu-id="b0523-183">На **Выбор шаблона процесса** выберите шаблон процесса, который вы хотите использовать для управления процессом разработки и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="b0523-183">On the **Select a Process Template** page, select the process template that you want to use to manage the development process, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b0523-184">Дополнительные сведения о шаблонах процессов TFS. в разделе [шаблонов процессов и средств](https://msdn.microsoft.com/vstudio/aa718795).</span><span class="sxs-lookup"><span data-stu-id="b0523-184">For more information on process templates for TFS, see [Process Templates and Tools](https://msdn.microsoft.com/vstudio/aa718795).</span></span>
12. <span data-ttu-id="b0523-185">На **параметры сайта группы** оставьте параметры по умолчанию без изменений и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="b0523-185">On the **Team Site Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
13. <span data-ttu-id="b0523-186">Этот параметр создает или определяет сайт группы SharePoint, который связан с командным проектом TFS.</span><span class="sxs-lookup"><span data-stu-id="b0523-186">This setting creates, or identifies, a SharePoint team site that is associated with the TFS team project.</span></span> <span data-ttu-id="b0523-187">Команды разработчиков могут использовать этот сайт для управления документации, участвовать в цепочки обсуждений, создавать вики-страницы и выполнять различные задачи, которые не связаны с кодом.</span><span class="sxs-lookup"><span data-stu-id="b0523-187">Your development team can use this site to manage documentation, participate in discussion threads, create wiki pages, and perform various other tasks that are not related to code.</span></span> <span data-ttu-id="b0523-188">Дополнительные сведения см. в разделе [взаимодействий между продуктов SharePoint и Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span><span class="sxs-lookup"><span data-stu-id="b0523-188">For more information, see [Interactions Between SharePoint Products and Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span></span>
14. <span data-ttu-id="b0523-189">На **укажите параметры системы управления версиями** оставьте параметры по умолчанию без изменений и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="b0523-189">On the **Specify Source Control Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
15. <span data-ttu-id="b0523-190">Этот параметр определяет или создает папку в иерархии папок TFS, который будет выступать в качестве корневой папки для содержимого.</span><span class="sxs-lookup"><span data-stu-id="b0523-190">This setting identifies or creates the location in the TFS folder hierarchy that will act as a root folder for your content.</span></span>
16. <span data-ttu-id="b0523-191">На **подтверждение настроек командного проекта** щелкните **Готово**.</span><span class="sxs-lookup"><span data-stu-id="b0523-191">On the **Confirm Team Project Settings** page, click **Finish**.</span></span>
17. <span data-ttu-id="b0523-192">Когда новый командный проект создан успешно, на **командный проект создан** щелкните **закрыть**.</span><span class="sxs-lookup"><span data-stu-id="b0523-192">When the new team project is successfully created, on the **Team Project Created** page, click **Close**.</span></span>

### <a name="add-users-to-a-team-project"></a><span data-ttu-id="b0523-193">Добавление пользователей в командный проект</span><span class="sxs-lookup"><span data-stu-id="b0523-193">Add Users to a Team Project</span></span>

<span data-ttu-id="b0523-194">После создания нового командного проекта можно предоставить разрешения для пользователей, чтобы включить их начать добавление и совместной работы с содержимым.</span><span class="sxs-lookup"><span data-stu-id="b0523-194">Now that you've created the new team project, you can grant permissions to users to enable them to start adding and collaborating on content.</span></span>

<span data-ttu-id="b0523-195">**Чтобы добавить пользователей в командный проект**</span><span class="sxs-lookup"><span data-stu-id="b0523-195">**To add users to a team project**</span></span>

1. <span data-ttu-id="b0523-196">В Visual Studio 2010 в **Team Explorer** , щелкните правой кнопкой мыши командный проект, подведите указатель к **параметры командного проекта**, а затем нажмите кнопку **членство в группе**.</span><span class="sxs-lookup"><span data-stu-id="b0523-196">In Visual Studio 2010, in the **Team Explorer** window, right-click the team project, point to **Team Project Settings**, and then click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. <span data-ttu-id="b0523-197">Чтобы дать пользователю возможность добавлять, изменять и удалять кода в системе управления версиями, необходимо добавить его к **участники** группы.</span><span class="sxs-lookup"><span data-stu-id="b0523-197">To enable a user to add, modify, and remove code under source control, you need to add him or her to the **Contributors** group.</span></span>
3. <span data-ttu-id="b0523-198">В **группы проекта** выберите **участники** группы, а затем нажмите кнопку **свойства**.</span><span class="sxs-lookup"><span data-stu-id="b0523-198">In the **Project Groups** dialog box, select the **Contributors** group, and then click **Properties**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. <span data-ttu-id="b0523-199">В **свойства группы Team Foundation Server** выберите **пользователя или группы Windows**, а затем нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="b0523-199">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. <span data-ttu-id="b0523-200">В **Выбор пользователей, компьютеров или групп** диалоговое окно, введите имя пользователя, которого нужно добавить в командный проект, нажмите кнопку **проверить имена**, а затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="b0523-200">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to add to the team project, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. <span data-ttu-id="b0523-201">В **свойства группы Team Foundation Server** диалоговое окно, нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="b0523-201">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
7. <span data-ttu-id="b0523-202">В **группы проекта** диалоговое окно, нажмите кнопку **закрыть**.</span><span class="sxs-lookup"><span data-stu-id="b0523-202">In the **Project Groups** dialog box, click **Close**.</span></span>

## <a name="conclusion"></a><span data-ttu-id="b0523-203">Заключение</span><span class="sxs-lookup"><span data-stu-id="b0523-203">Conclusion</span></span>

<span data-ttu-id="b0523-204">На этом этапе новый командный проект готов к использованию и команды разработчиков можно начать добавление содержимого и совместная работа в процессе разработки.</span><span class="sxs-lookup"><span data-stu-id="b0523-204">At this point, your new team project is ready to use, and your developer team can start adding content and collaborating on the development process.</span></span>

<span data-ttu-id="b0523-205">Следующий раздел [добавления содержимого в систему управления версиями](adding-content-to-source-control.md), Описание способов добавления содержимого в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="b0523-205">The next topic, [Adding Content to Source Control](adding-content-to-source-control.md), describes how to add content to source control.</span></span>

## <a name="further-reading"></a><span data-ttu-id="b0523-206">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="b0523-206">Further Reading</span></span>

<span data-ttu-id="b0523-207">Более широкой рекомендации по созданию командных проектов в TFS. в разделе [создания командного проекта](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="b0523-207">For broader guidance on creating team projects in TFS, see [Create a Team Project](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span></span> <span data-ttu-id="b0523-208">Дополнительные сведения о включении пользователям создавать новые командные проекты в коллекции командных проектов см. в разделе [Установка прав администратора для коллекций командных проектов](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="b0523-208">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span> <span data-ttu-id="b0523-209">Дополнительные сведения о добавлении пользователей в командные проекты, см. в разделе [Добавление пользователей в командные проекты](https://msdn.microsoft.com/library/bb558971.aspx).</span><span class="sxs-lookup"><span data-stu-id="b0523-209">For more information on adding users to team projects, see [Add Users to Team Projects](https://msdn.microsoft.com/library/bb558971.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b0523-210">[Назад](configuring-team-foundation-server-for-web-deployment.md)
> [Вперед](adding-content-to-source-control.md)</span><span class="sxs-lookup"><span data-stu-id="b0523-210">[Previous](configuring-team-foundation-server-for-web-deployment.md)
[Next](adding-content-to-source-control.md)</span></span>
