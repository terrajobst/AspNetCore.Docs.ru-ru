---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: Добавление содержимого в систему управления версиями | Документация Майкрософт
author: jrjlee
description: В этом разделе объясняется, как для добавления содержимого в систему управления версиями в Team Foundation Server (TFS) 2010. В этом примере описывает добавление решений и проектов в проекте team...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: 9fdb2e37f2925273b457157b634865d93e865098
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838375"
---
<a name="adding-content-to-source-control"></a><span data-ttu-id="c29d7-104">Добавление содержимого в системе управления версиями</span><span class="sxs-lookup"><span data-stu-id="c29d7-104">Adding Content to Source Control</span></span>
====================
<span data-ttu-id="c29d7-105">по [Джейсон Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="c29d7-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="c29d7-106">Загрузить PDF-файл</span><span class="sxs-lookup"><span data-stu-id="c29d7-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="c29d7-107">В этом разделе объясняется, как для добавления содержимого в систему управления версиями в Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="c29d7-107">This topic explains how to add content to source control in Team Foundation Server (TFS) 2010.</span></span> <span data-ttu-id="c29d7-108">Описывает добавление решений и проектов к командному проекту в TFS, а также описание способов добавления внешних зависимостей, таких как платформы или сборки в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="c29d7-108">It describes how to add solutions and projects to a team project in TFS, and it explains how to add external dependencies like frameworks or assemblies to source control.</span></span>


<span data-ttu-id="c29d7-109">Этот раздел является частью серии учебников, исходя из требования к развертыванию enterprise вымышленной компании Fabrikam, Inc. В этой серии руководств используется пример решения&#x2014; [решения диспетчера контактов](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;для представления веб-приложения с более реалистичные уровень сложности, включая приложения ASP.NET MVC 3, Windows Communication Служба Foundation (WCF) и проект базы данных.</span><span class="sxs-lookup"><span data-stu-id="c29d7-109">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="c29d7-110">Общие сведения о задачах</span><span class="sxs-lookup"><span data-stu-id="c29d7-110">Task Overview</span></span>

<span data-ttu-id="c29d7-111">В большинстве случаев каждый член команды разработчиков, должны иметь возможность добавить содержимое в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="c29d7-111">In most cases, every member of the developer team should be able to add content to source control.</span></span> <span data-ttu-id="c29d7-112">Чтобы добавить решение в систему управления версиями в Team Foundation Server, необходимо выполнить следующие действия:</span><span class="sxs-lookup"><span data-stu-id="c29d7-112">To add a solution to source control in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="c29d7-113">Подключитесь к командному проекту.</span><span class="sxs-lookup"><span data-stu-id="c29d7-113">Connect to a team project.</span></span>
- <span data-ttu-id="c29d7-114">Сопоставление структуры папки командного проекта на сервере в структуре папок на локальном компьютере.</span><span class="sxs-lookup"><span data-stu-id="c29d7-114">Map the team project folder structure on the server to a folder structure on your local computer.</span></span>
- <span data-ttu-id="c29d7-115">Добавьте решение и его содержимое в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="c29d7-115">Add the solution and its contents to source control.</span></span>
- <span data-ttu-id="c29d7-116">Добавьте любых внешних зависимостей в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="c29d7-116">Add any external dependencies to source control.</span></span>

<span data-ttu-id="c29d7-117">В этом разделе будет показано, как для выполнения этих процедур.</span><span class="sxs-lookup"><span data-stu-id="c29d7-117">This topic will show you how to perform these procedures.</span></span>

<span data-ttu-id="c29d7-118">Задачи и пошаговые руководства в этом разделе предполагается, что вы уже создали новый командный проект TFS для управления содержимым.</span><span class="sxs-lookup"><span data-stu-id="c29d7-118">The tasks and walkthroughs in this topic assume that you've already created a new TFS team project to manage your content.</span></span> <span data-ttu-id="c29d7-119">Дополнительные сведения о создании нового командного проекта см. в разделе [создания командного проекта в Team Foundation Server](creating-a-team-project-in-tfs.md).</span><span class="sxs-lookup"><span data-stu-id="c29d7-119">For more information on creating a new team project, see [Creating a Team Project in TFS](creating-a-team-project-in-tfs.md).</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="c29d7-120">Кто выполняет эти процедуры?</span><span class="sxs-lookup"><span data-stu-id="c29d7-120">Who Performs These Procedures?</span></span>

<span data-ttu-id="c29d7-121">В большинстве случаев каждый член команды разработчиков, сможете добавлять и изменять содержимое в определенным командным проектам.</span><span class="sxs-lookup"><span data-stu-id="c29d7-121">In most cases, every member of the developer team should be able to add and modify content within specific team projects.</span></span>

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a><span data-ttu-id="c29d7-122">Подключение к командному проекту и создать сопоставление папки</span><span class="sxs-lookup"><span data-stu-id="c29d7-122">Connect to a Team Project and Create a Folder Mapping</span></span>

<span data-ttu-id="c29d7-123">Прежде чем добавить любое содержимое в систему управления версиями, необходимо подключиться к командному проекту и создайте сопоставление между структуре папок на сервере и в файловой системе на локальном компьютере.</span><span class="sxs-lookup"><span data-stu-id="c29d7-123">Before you add any content to source control, you need to connect to a team project and create a mapping between the folder structure on the server and the file system on your local machine.</span></span>

<span data-ttu-id="c29d7-124">**Для подключения к командному проекту и сопоставления локальный путь**</span><span class="sxs-lookup"><span data-stu-id="c29d7-124">**To connect to a team project and map a local path**</span></span>

1. <span data-ttu-id="c29d7-125">Откройте Visual Studio 2010 на рабочей станции разработчика.</span><span class="sxs-lookup"><span data-stu-id="c29d7-125">On your developer workstation, open Visual Studio 2010.</span></span>
2. <span data-ttu-id="c29d7-126">В Visual Studio на **Team** меню, щелкните **подключиться к Team Foundation Server**.</span><span class="sxs-lookup"><span data-stu-id="c29d7-126">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c29d7-127">Если вы уже настроили подключение к серверу TFS, можно пропустить шаги 3 – 6.</span><span class="sxs-lookup"><span data-stu-id="c29d7-127">If you have already configured a connection to a TFS server, you can omit steps 3-6.</span></span>
3. <span data-ttu-id="c29d7-128">В **подключение к командному проекту** диалоговом окне щелкните **серверов**.</span><span class="sxs-lookup"><span data-stu-id="c29d7-128">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
4. <span data-ttu-id="c29d7-129">В **Добавление или удаление Team Foundation Server** диалоговом окне щелкните **добавить**.</span><span class="sxs-lookup"><span data-stu-id="c29d7-129">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
5. <span data-ttu-id="c29d7-130">В **добавить Team Foundation Server** диалоговом окне укажите сведения об экземпляре TFS, а затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="c29d7-130">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](adding-content-to-source-control/_static/image1.png)
6. <span data-ttu-id="c29d7-131">В **Добавление или удаление Team Foundation Server** диалоговом окне щелкните **закрыть**.</span><span class="sxs-lookup"><span data-stu-id="c29d7-131">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
7. <span data-ttu-id="c29d7-132">В **подключение к командному проекту** диалоговом окне выберите экземпляр TFS, вы хотите подключиться, выберите команду коллекция проектов, выберите командный проект, вы хотите добавить и нажмите кнопку **Connect**.</span><span class="sxs-lookup"><span data-stu-id="c29d7-132">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection, select the team project you want to add to, and then click **Connect**.</span></span>

    ![](adding-content-to-source-control/_static/image2.png)
8. <span data-ttu-id="c29d7-133">В **Team Explorer** окне разверните узел командного проекта, а затем дважды щелкните **системы управления версиями**.</span><span class="sxs-lookup"><span data-stu-id="c29d7-133">In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image3.png)
9. <span data-ttu-id="c29d7-134">На **обозреватель управления исходным кодом** щелкните **не сопоставлен**.</span><span class="sxs-lookup"><span data-stu-id="c29d7-134">On the **Source Control Explorer** tab, click **Not mapped**.</span></span>

    ![](adding-content-to-source-control/_static/image4.png)
10. <span data-ttu-id="c29d7-135">В **карты** отображаемое в диалоговом окне **локальную папку** поле, перейдите к (или создание) в локальной папке, в качестве корневой папки для командного проекта и нажмите кнопку **карты**.</span><span class="sxs-lookup"><span data-stu-id="c29d7-135">In the **Map** dialog box, in the **Local folder** box, browse to (or create) a local folder to act as the root folder for the team project, and then click **Map**.</span></span>

    ![](adding-content-to-source-control/_static/image5.png)
11. <span data-ttu-id="c29d7-136">Когда появится запрос для загрузки исходных файлов, нажмите кнопку **Да**.</span><span class="sxs-lookup"><span data-stu-id="c29d7-136">When you're prompted to download source files, click **Yes**.</span></span>

    ![](adding-content-to-source-control/_static/image6.png)

<span data-ttu-id="c29d7-137">На этом этапе папке на сервере для командного проекта сопоставлена с локальной папки на рабочей станции разработчика.</span><span class="sxs-lookup"><span data-stu-id="c29d7-137">At this point, you have mapped the server-side folder for the team project to a local folder on your developer workstation.</span></span> <span data-ttu-id="c29d7-138">Все существующее содержимое из командного проекта также загрузки в локальную папку структуру.</span><span class="sxs-lookup"><span data-stu-id="c29d7-138">You've also downloaded any existing content from the team project to your local folder structure.</span></span> <span data-ttu-id="c29d7-139">Теперь можно начать добавлять разделы в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="c29d7-139">You can now start to add your own content to source control.</span></span>

## <a name="add-projects-and-solutions-to-source-control"></a><span data-ttu-id="c29d7-140">Добавьте проекты и решения в систему управления версиями</span><span class="sxs-lookup"><span data-stu-id="c29d7-140">Add Projects and Solutions to Source Control</span></span>

<span data-ttu-id="c29d7-141">Чтобы добавить проекты и решения в систему управления версиями, необходимо сначала переместить их в сопоставленной папки для командного проекта на локальном компьютере.</span><span class="sxs-lookup"><span data-stu-id="c29d7-141">To add projects and solutions to source control, you first need to move them to the mapped folder for the team project on your local machine.</span></span> <span data-ttu-id="c29d7-142">Затем можно проверить в содержимом для синхронизации с сервером добавления.</span><span class="sxs-lookup"><span data-stu-id="c29d7-142">You can then check in the content to synchronize your additions with the server.</span></span>

<span data-ttu-id="c29d7-143">**Добавление проектов в систему управления версиями**</span><span class="sxs-lookup"><span data-stu-id="c29d7-143">**To add projects to source control**</span></span>

1. <span data-ttu-id="c29d7-144">На рабочей станции разработчика переместите проектов и решений в соответствующую папку в структуре сопоставленной папки для командного проекта.</span><span class="sxs-lookup"><span data-stu-id="c29d7-144">On your developer workstation, move your projects and solutions to an appropriate location within the mapped folder structure for the team project.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c29d7-145">Многие организации имеют рекомендуемым способом, каким образом организовывать проекты и решения в системе управления версиями.</span><span class="sxs-lookup"><span data-stu-id="c29d7-145">Many organizations will have a preferred approach to how projects and solutions should be organized in source control.</span></span> <span data-ttu-id="c29d7-146">Рекомендации о том, как структура папок, см. в разделе [How To: Your папок системы управления версиями структуру в Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span><span class="sxs-lookup"><span data-stu-id="c29d7-146">For guidance on how to structure folders, see [How To: Structure Your Source Control Folders in Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span></span>
2. <span data-ttu-id="c29d7-147">Откройте решение в Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="c29d7-147">Open the solution in Visual Studio 2010.</span></span>
3. <span data-ttu-id="c29d7-148">В **обозревателе решений** окно, щелкните правой кнопкой мыши решение и нажмите кнопку **добавить решение в систему управления версиями**.</span><span class="sxs-lookup"><span data-stu-id="c29d7-148">In the **Solution Explorer** window, right-click the solution, and then click **Add Solution to Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > <span data-ttu-id="c29d7-149">В некоторых случаях в зависимости от того, как организации нравится содержимое структуры в TFS может потребоваться добавить проекты в систему управления версиями индивидуально, предоставляя более точный контроль над способ организации исходного кода.</span><span class="sxs-lookup"><span data-stu-id="c29d7-149">In some cases, depending on how your organization likes to structure content in TFS, you may need to add projects to source control individually to provide more fine-grained control over how your source code is organized.</span></span>
4. <span data-ttu-id="c29d7-150">Убедитесь, что **обозреватель управления исходным кодом** вкладка отображает содержимое, добавленные в структуре папки сервера для командного проекта.</span><span class="sxs-lookup"><span data-stu-id="c29d7-150">Verify that the **Source Control Explorer** tab displays the content you've added within the server folder structure for the team project.</span></span>

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > <span data-ttu-id="c29d7-151">**Обозреватель управления исходным кодом** вкладка отображает содержимое с помощью не повторный запрос, так как вы добавили решения сопоставленную папку в локальной файловой системе.</span><span class="sxs-lookup"><span data-stu-id="c29d7-151">The **Source Control Explorer** tab displays your content with no further prompting because you added your solution to a mapped folder on the local file system.</span></span> <span data-ttu-id="c29d7-152">Если решение в Несопоставленное расположение, вам будет предложено указать расположения папок в TFS и локальной файловой системе.</span><span class="sxs-lookup"><span data-stu-id="c29d7-152">If your solution was in an unmapped location, you'd be prompted to specify folder locations in both TFS and your local file system.</span></span>
5. <span data-ttu-id="c29d7-153">На **обозреватель управления исходным кодом** на вкладке **папки** панели, щелкните правой кнопкой мыши командный проект (например, **ContactManager**) и нажмите кнопку **возврата Ожидающие изменения**.</span><span class="sxs-lookup"><span data-stu-id="c29d7-153">On the **Source Control Explorer** tab, in the **Folders** pane, right-click the team project (for example, **ContactManager**), and then click **Check In Pending Changes**.</span></span>
6. <span data-ttu-id="c29d7-154">В **возврата — исходные файлы** диалоговом окне введите комментарий и нажмите кнопку **вернуть**.</span><span class="sxs-lookup"><span data-stu-id="c29d7-154">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

    ![](adding-content-to-source-control/_static/image9.png)

<span data-ttu-id="c29d7-155">На этом этапе вы добавили решения в систему управления версиями в Team Foundation Server.</span><span class="sxs-lookup"><span data-stu-id="c29d7-155">At this point you have added your solution to source control in TFS.</span></span>

## <a name="add-external-dependencies-to-source-control"></a><span data-ttu-id="c29d7-156">Добавление внешних зависимостей в систему управления версиями</span><span class="sxs-lookup"><span data-stu-id="c29d7-156">Add External Dependencies to Source Control</span></span>

<span data-ttu-id="c29d7-157">При добавлении проекта или решения в систему управления версиями, все файлы и папки в проекте или решении также добавляется.</span><span class="sxs-lookup"><span data-stu-id="c29d7-157">When you add a project or solution to source control, any files and folders within your project or solution will also be added.</span></span> <span data-ttu-id="c29d7-158">Тем не менее во многих случаях, проекты и решения также полагаются на внешние зависимости, такие как локальных сборок, для правильной.</span><span class="sxs-lookup"><span data-stu-id="c29d7-158">However, in a lot of cases, projects and solutions also rely on external dependencies, like local assemblies, to function properly.</span></span> <span data-ttu-id="c29d7-159">Необходимо добавить, что таких ресурсов в систему управления версиями, чтобы позволить Team Build и другими членами команды разработчиков успешно сборки кода.</span><span class="sxs-lookup"><span data-stu-id="c29d7-159">You need to add any such resources to source control to let both Team Build and other members of the developer team build your code successfully.</span></span>

<span data-ttu-id="c29d7-160">Например структура папок для примера решения диспетчера контактов включает в себя папку с именем пакетов.</span><span class="sxs-lookup"><span data-stu-id="c29d7-160">For example, the folder structure for the Contact Manager sample solution includes a folder named packages.</span></span> <span data-ttu-id="c29d7-161">Он содержит сборки и различные вспомогательные ресурсы для ADO.NET Entity Framework 4.1.</span><span class="sxs-lookup"><span data-stu-id="c29d7-161">This contains the assembly and various supporting resources for the ADO.NET Entity Framework 4.1.</span></span> <span data-ttu-id="c29d7-162">«Пакеты» не является частью решения диспетчера контактов, но решение не будет успешно построен без него.</span><span class="sxs-lookup"><span data-stu-id="c29d7-162">The packages folder is not part of the Contact Manager solution, but the solution will not build successfully without it.</span></span> <span data-ttu-id="c29d7-163">Чтобы включить Team Build для построения решения, необходимо добавить папку packages в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="c29d7-163">To enable Team Build to build the solution, you need to add the packages folder to source control.</span></span>

> [!NOTE]
> <span data-ttu-id="c29d7-164">Включение папки пакетов является типичным, что происходит при добавлении платформы Entity Framework, то есть одинаковые ресурсы к решению, используя расширения NuGet для Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="c29d7-164">The inclusion of a packages folder is typical of what happens when you add the Entity Framework, or similar resources, to your solution using the NuGet extension for Visual Studio 2010.</span></span>


<span data-ttu-id="c29d7-165">**Для добавления содержимого не входящих в проект в систему управления версиями**</span><span class="sxs-lookup"><span data-stu-id="c29d7-165">**To add non-project content to source control**</span></span>

1. <span data-ttu-id="c29d7-166">Убедитесь, что элементы, которые вы хотите добавить (например, «пакеты») в соответствующем месте в пределах сопоставленную папку в локальной файловой системе.</span><span class="sxs-lookup"><span data-stu-id="c29d7-166">Ensure that the items you want to add (for example, the packages folder) are in an appropriate location within a mapped folder on your local file system.</span></span>
2. <span data-ttu-id="c29d7-167">В Visual Studio 2010 в **Team Explorer** окне разверните узел командного проекта, а затем дважды щелкните **системы управления версиями**.</span><span class="sxs-lookup"><span data-stu-id="c29d7-167">In Visual Studio 2010, In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image10.png)
3. <span data-ttu-id="c29d7-168">На **обозреватель управления исходным кодом** на вкладке **папки** панель, выберите папку, которая содержит элемент или элементы нужно добавить.</span><span class="sxs-lookup"><span data-stu-id="c29d7-168">On the **Source Control Explorer** tab, in the **Folders** pane, select the folder that contains the item or items you want to add.</span></span>
4. <span data-ttu-id="c29d7-169">Нажмите кнопку **добавить элементы в папку** кнопки.</span><span class="sxs-lookup"><span data-stu-id="c29d7-169">Click the **Add Items to Folder** button.</span></span>

    ![](adding-content-to-source-control/_static/image11.png)
5. <span data-ttu-id="c29d7-170">В **добавить в систему управления версиями** диалоговом окне выберите папку или элементы, которые необходимо добавить, а затем нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="c29d7-170">In the **Add to Source Control** dialog box, select the folder or items you want to add, and then click **Next**.</span></span>

    ![](adding-content-to-source-control/_static/image12.png)
6. <span data-ttu-id="c29d7-171">На **Исключенные элементы** выберите все необходимые элементы, которые автоматически исключаются (например, сборки) и нажмите кнопку **элементы, относящиеся к**.</span><span class="sxs-lookup"><span data-stu-id="c29d7-171">On the **Excluded items** tab, select any required items that have been automatically excluded (for example, assemblies), and then click **Include item(s)**.</span></span>

    ![](adding-content-to-source-control/_static/image13.png)
7. <span data-ttu-id="c29d7-172">На **элементы для добавления** убедитесь, что перечислены все файлы, которые нужно включить и нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="c29d7-172">On the **Items to add** tab, verify that all the files you want to include are listed, and then click **Finish**.</span></span>

    ![](adding-content-to-source-control/_static/image14.png)
8. <span data-ttu-id="c29d7-173">В **обозреватель управления исходным кодом** окно, нажмите кнопку **вернуть** кнопки.</span><span class="sxs-lookup"><span data-stu-id="c29d7-173">In the **Source Control Explorer** window, click the **Check In** button.</span></span>

    ![](adding-content-to-source-control/_static/image15.png)
9. <span data-ttu-id="c29d7-174">В **возврата — исходные файлы** диалоговом окне введите комментарий и нажмите кнопку **вернуть**.</span><span class="sxs-lookup"><span data-stu-id="c29d7-174">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

<span data-ttu-id="c29d7-175">На этом этапе вы добавили внешние зависимости для вашего решения в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="c29d7-175">At this point, you have added the external dependencies for your solution to source control.</span></span>

## <a name="conclusion"></a><span data-ttu-id="c29d7-176">Заключение</span><span class="sxs-lookup"><span data-stu-id="c29d7-176">Conclusion</span></span>

<span data-ttu-id="c29d7-177">В этом разделе описано, как подключиться к командному проекту, сопоставить структуру папок и добавить содержимое в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="c29d7-177">This topic described how to connect to a team project, map a folder structure, and add content to source control.</span></span> <span data-ttu-id="c29d7-178">Дополнительные сведения о том, как работа с элементами в системе управления версиями см. в разделе [использование управления версиями](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="c29d7-178">For more information on how to work with items under source control, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

<span data-ttu-id="c29d7-179">Следующий раздел, [Настройка сервера сборки TFS для развертывания веб-](configuring-a-tfs-build-server-for-web-deployment.md), описывается, как подготовить сервер TFS Team Build для создания и развертывания решения.</span><span class="sxs-lookup"><span data-stu-id="c29d7-179">The next topic, [Configuring a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md), describes how to prepare a TFS Team Build server to build and deploy your solution.</span></span>

## <a name="further-reading"></a><span data-ttu-id="c29d7-180">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="c29d7-180">Further Reading</span></span>

<span data-ttu-id="c29d7-181">Более подробную информацию о работе с системой управления версиями в Team Foundation Server см. в разделе [использование управления версиями](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="c29d7-181">For more comprehensive information on working with source control in TFS, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c29d7-182">[Назад](creating-a-team-project-in-tfs.md)
> [Вперед](configuring-a-tfs-build-server-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="c29d7-182">[Previous](creating-a-team-project-in-tfs.md)
[Next](configuring-a-tfs-build-server-for-web-deployment.md)</span></span>
