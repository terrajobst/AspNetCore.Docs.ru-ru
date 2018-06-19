---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: Добавление содержимого в систему управления версиями | Документы Microsoft
author: jrjlee
description: В этом разделе объясняется, как добавлять содержимое в систему управления версиями в Team Foundation Server (TFS) 2010. Он описывает, как добавление решений и проектов в проекте группы...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: c9c3a506d2745a6793661453a293732429d3e46e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890438"
---
<a name="adding-content-to-source-control"></a><span data-ttu-id="49351-104">Добавление содержимого в систему управления версиями</span><span class="sxs-lookup"><span data-stu-id="49351-104">Adding Content to Source Control</span></span>
====================
<span data-ttu-id="49351-105">по [Джейсон Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="49351-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="49351-106">Скачать PDF</span><span class="sxs-lookup"><span data-stu-id="49351-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="49351-107">В этом разделе объясняется, как добавлять содержимое в систему управления версиями в Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="49351-107">This topic explains how to add content to source control in Team Foundation Server (TFS) 2010.</span></span> <span data-ttu-id="49351-108">Описывает добавление решений и проектов к командному проекту в TFS, а также описание способов добавления внешние зависимости, такие как .NET Framework или сборках в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="49351-108">It describes how to add solutions and projects to a team project in TFS, and it explains how to add external dependencies like frameworks or assemblies to source control.</span></span>


<span data-ttu-id="49351-109">Этот раздел входит в состав серию учебников, исходя из требования к развертыванию enterprise вымышленная компания Fabrikam, Inc. Этот учебник ряд использует образец решения&#x2014; [решения диспетчера контактов](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;для представления веб-приложения с реалистичных уровень сложности, включая приложения ASP.NET MVC 3, Windows Communication Служба Foundation (WCF) и проект базы данных.</span><span class="sxs-lookup"><span data-stu-id="49351-109">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="49351-110">Общие сведения о задаче</span><span class="sxs-lookup"><span data-stu-id="49351-110">Task Overview</span></span>

<span data-ttu-id="49351-111">В большинстве случаев каждый член команды разработчиков должен иметь возможность добавления содержимого в системе управления версиями.</span><span class="sxs-lookup"><span data-stu-id="49351-111">In most cases, every member of the developer team should be able to add content to source control.</span></span> <span data-ttu-id="49351-112">Чтобы добавить решение в систему управления версиями в Team Foundation Server, необходимо выполнить следующие общие действия:</span><span class="sxs-lookup"><span data-stu-id="49351-112">To add a solution to source control in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="49351-113">Подключитесь к командному проекту.</span><span class="sxs-lookup"><span data-stu-id="49351-113">Connect to a team project.</span></span>
- <span data-ttu-id="49351-114">Сопоставьте структура папки командного проекта на сервере в структуре папок на локальном компьютере.</span><span class="sxs-lookup"><span data-stu-id="49351-114">Map the team project folder structure on the server to a folder structure on your local computer.</span></span>
- <span data-ttu-id="49351-115">Добавьте решение и его содержимое в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="49351-115">Add the solution and its contents to source control.</span></span>
- <span data-ttu-id="49351-116">Добавьте все внешние зависимости для системы управления версиями.</span><span class="sxs-lookup"><span data-stu-id="49351-116">Add any external dependencies to source control.</span></span>

<span data-ttu-id="49351-117">В этом разделе будет показано, как для выполнения этих процедур.</span><span class="sxs-lookup"><span data-stu-id="49351-117">This topic will show you how to perform these procedures.</span></span>

<span data-ttu-id="49351-118">Задачи и пошаговые руководства в этом разделе предполагается, уже создали новый командный проект TFS для управления содержимым.</span><span class="sxs-lookup"><span data-stu-id="49351-118">The tasks and walkthroughs in this topic assume that you've already created a new TFS team project to manage your content.</span></span> <span data-ttu-id="49351-119">Дополнительные сведения о создании нового командного проекта см. в разделе [создания командного проекта в Team Foundation Server](creating-a-team-project-in-tfs.md).</span><span class="sxs-lookup"><span data-stu-id="49351-119">For more information on creating a new team project, see [Creating a Team Project in TFS](creating-a-team-project-in-tfs.md).</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="49351-120">Кто выполняет эти процедуры?</span><span class="sxs-lookup"><span data-stu-id="49351-120">Who Performs These Procedures?</span></span>

<span data-ttu-id="49351-121">В большинстве случаев каждый член команды разработчиков должна быть возможность добавления и изменения содержимого определенным командным проектам.</span><span class="sxs-lookup"><span data-stu-id="49351-121">In most cases, every member of the developer team should be able to add and modify content within specific team projects.</span></span>

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a><span data-ttu-id="49351-122">Подключитесь к командному проекту и создать сопоставление папки</span><span class="sxs-lookup"><span data-stu-id="49351-122">Connect to a Team Project and Create a Folder Mapping</span></span>

<span data-ttu-id="49351-123">Прежде чем добавлять содержимое в систему управления версиями, необходимо подключиться к командному проекту и создание сопоставления между структуру папок на сервере и файловой системы на локальном компьютере.</span><span class="sxs-lookup"><span data-stu-id="49351-123">Before you add any content to source control, you need to connect to a team project and create a mapping between the folder structure on the server and the file system on your local machine.</span></span>

<span data-ttu-id="49351-124">**Чтобы подключиться к командному проекту и сопоставить локальный путь**</span><span class="sxs-lookup"><span data-stu-id="49351-124">**To connect to a team project and map a local path**</span></span>

1. <span data-ttu-id="49351-125">На компьютере разработчика откройте Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="49351-125">On your developer workstation, open Visual Studio 2010.</span></span>
2. <span data-ttu-id="49351-126">В Visual Studio на **команды** меню, нажмите кнопку **подключиться к Team Foundation Server**.</span><span class="sxs-lookup"><span data-stu-id="49351-126">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="49351-127">Если вы уже настроили соединение с сервером TFS, можно пропустить шаги 3 – 6.</span><span class="sxs-lookup"><span data-stu-id="49351-127">If you have already configured a connection to a TFS server, you can omit steps 3-6.</span></span>
3. <span data-ttu-id="49351-128">В **подключение к командному проекту** диалоговое окно, нажмите кнопку **серверов**.</span><span class="sxs-lookup"><span data-stu-id="49351-128">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
4. <span data-ttu-id="49351-129">В **Добавление или удаление Team Foundation Server** диалоговое окно, нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="49351-129">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
5. <span data-ttu-id="49351-130">В **Добавление Team Foundation Server** диалоговое окно «» укажите сведения об экземпляре TFS и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="49351-130">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](adding-content-to-source-control/_static/image1.png)
6. <span data-ttu-id="49351-131">В **Добавление или удаление Team Foundation Server** диалоговое окно, нажмите кнопку **закрыть**.</span><span class="sxs-lookup"><span data-stu-id="49351-131">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
7. <span data-ttu-id="49351-132">В **подключение к командному проекту** диалоговое окно, выберите экземпляр TFS, необходимо подключиться, выберите команду проект коллекции, выберите командный проект, необходимо добавить и выберите команду **Connect**.</span><span class="sxs-lookup"><span data-stu-id="49351-132">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection, select the team project you want to add to, and then click **Connect**.</span></span>

    ![](adding-content-to-source-control/_static/image2.png)
8. <span data-ttu-id="49351-133">В **Team Explorer** окна, разверните узел командного проекта, а затем дважды щелкните **системы управления версиями**.</span><span class="sxs-lookup"><span data-stu-id="49351-133">In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image3.png)
9. <span data-ttu-id="49351-134">На **обозревателя управления исходным кодом** щелкните **не сопоставлен**.</span><span class="sxs-lookup"><span data-stu-id="49351-134">On the **Source Control Explorer** tab, click **Not mapped**.</span></span>

    ![](adding-content-to-source-control/_static/image4.png)
10. <span data-ttu-id="49351-135">В **карты** диалогового **локальную папку** поле, перейдите к (или создайте) в локальной папке выступать в качестве корневой папки командного проекта, а затем нажмите кнопку **карты**.</span><span class="sxs-lookup"><span data-stu-id="49351-135">In the **Map** dialog box, in the **Local folder** box, browse to (or create) a local folder to act as the root folder for the team project, and then click **Map**.</span></span>

    ![](adding-content-to-source-control/_static/image5.png)
11. <span data-ttu-id="49351-136">Когда появится запрос для загрузки исходных файлов, нажмите кнопку **Да**.</span><span class="sxs-lookup"><span data-stu-id="49351-136">When you're prompted to download source files, click **Yes**.</span></span>

    ![](adding-content-to-source-control/_static/image6.png)

<span data-ttu-id="49351-137">На этом этапе серверную папку для командного проекта, сопоставленной с локальной папке на компьютере разработчика.</span><span class="sxs-lookup"><span data-stu-id="49351-137">At this point, you have mapped the server-side folder for the team project to a local folder on your developer workstation.</span></span> <span data-ttu-id="49351-138">Все существующее содержимое также загружен из командного проекта в локальной папке структуру.</span><span class="sxs-lookup"><span data-stu-id="49351-138">You've also downloaded any existing content from the team project to your local folder structure.</span></span> <span data-ttu-id="49351-139">Теперь можно приступить к добавьте свое содержимое в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="49351-139">You can now start to add your own content to source control.</span></span>

## <a name="add-projects-and-solutions-to-source-control"></a><span data-ttu-id="49351-140">Добавления проектов и решений в систему управления версиями</span><span class="sxs-lookup"><span data-stu-id="49351-140">Add Projects and Solutions to Source Control</span></span>

<span data-ttu-id="49351-141">Чтобы добавить проекты и решения в систему управления версиями, необходимо сначала переместить их в сопоставленной папке для командного проекта на локальном компьютере.</span><span class="sxs-lookup"><span data-stu-id="49351-141">To add projects and solutions to source control, you first need to move them to the mapped folder for the team project on your local machine.</span></span> <span data-ttu-id="49351-142">Затем можно проверить в содержимом для синхронизации с сервером добавления.</span><span class="sxs-lookup"><span data-stu-id="49351-142">You can then check in the content to synchronize your additions with the server.</span></span>

<span data-ttu-id="49351-143">**Добавление проектов в систему управления версиями**</span><span class="sxs-lookup"><span data-stu-id="49351-143">**To add projects to source control**</span></span>

1. <span data-ttu-id="49351-144">На рабочей станции разработчика переместите проектов и решений в соответствующую папку в структуре сопоставленную папку для командного проекта.</span><span class="sxs-lookup"><span data-stu-id="49351-144">On your developer workstation, move your projects and solutions to an appropriate location within the mapped folder structure for the team project.</span></span>

    > [!NOTE]
    > <span data-ttu-id="49351-145">Многие организации будут иметь рекомендуемым способом, как проекты и решения должна быть организована в системе управления версиями.</span><span class="sxs-lookup"><span data-stu-id="49351-145">Many organizations will have a preferred approach to how projects and solutions should be organized in source control.</span></span> <span data-ttu-id="49351-146">Руководство по структуре папок см. в разделе [How To: вашей папки системы управления версиями структуру в Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span><span class="sxs-lookup"><span data-stu-id="49351-146">For guidance on how to structure folders, see [How To: Structure Your Source Control Folders in Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span></span>
2. <span data-ttu-id="49351-147">Откройте решение в Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="49351-147">Open the solution in Visual Studio 2010.</span></span>
3. <span data-ttu-id="49351-148">В **обозревателе решений** щелкните правой кнопкой мыши решение и выберите **добавить решение в систему управления версиями**.</span><span class="sxs-lookup"><span data-stu-id="49351-148">In the **Solution Explorer** window, right-click the solution, and then click **Add Solution to Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > <span data-ttu-id="49351-149">В некоторых случаях в зависимости от того, как ваша организация нравится содержимое структуры в TFS необходимо добавлять проекты к системе управления версиями по отдельности для обеспечения более точный контроль над организация исходного кода.</span><span class="sxs-lookup"><span data-stu-id="49351-149">In some cases, depending on how your organization likes to structure content in TFS, you may need to add projects to source control individually to provide more fine-grained control over how your source code is organized.</span></span>
4. <span data-ttu-id="49351-150">Убедитесь, что **обозревателя управления исходным кодом** вкладка отображает содержимое, добавленные в структуры папок сервера для командного проекта.</span><span class="sxs-lookup"><span data-stu-id="49351-150">Verify that the **Source Control Explorer** tab displays the content you've added within the server folder structure for the team project.</span></span>

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > <span data-ttu-id="49351-151">**Обозревателя управления исходным кодом** вкладка отображает содержимое с какой-либо дальнейшей запроса из-за добавления решения сопоставленную папку в локальной файловой системе.</span><span class="sxs-lookup"><span data-stu-id="49351-151">The **Source Control Explorer** tab displays your content with no further prompting because you added your solution to a mapped folder on the local file system.</span></span> <span data-ttu-id="49351-152">Если решение в Несопоставленное расположение, вам будет предложено указать папки в TFS, так и в локальной файловой системе.</span><span class="sxs-lookup"><span data-stu-id="49351-152">If your solution was in an unmapped location, you'd be prompted to specify folder locations in both TFS and your local file system.</span></span>
5. <span data-ttu-id="49351-153">На **обозревателя управления исходным кодом** вкладку в **папки** области, щелкните правой кнопкой мыши командный проект (например, **ContactManager**) и нажмите кнопку **возврат Ожидающие изменения**.</span><span class="sxs-lookup"><span data-stu-id="49351-153">On the **Source Control Explorer** tab, in the **Folders** pane, right-click the team project (for example, **ContactManager**), and then click **Check In Pending Changes**.</span></span>
6. <span data-ttu-id="49351-154">В **возврата – исходные файлы** диалоговое окно, введите комментарий и нажмите кнопку **вернуть**.</span><span class="sxs-lookup"><span data-stu-id="49351-154">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

    ![](adding-content-to-source-control/_static/image9.png)

<span data-ttu-id="49351-155">На этом этапе вы добавили решения в систему управления версиями в Team Foundation Server.</span><span class="sxs-lookup"><span data-stu-id="49351-155">At this point you have added your solution to source control in TFS.</span></span>

## <a name="add-external-dependencies-to-source-control"></a><span data-ttu-id="49351-156">Добавление внешних зависимостей в систему управления версиями</span><span class="sxs-lookup"><span data-stu-id="49351-156">Add External Dependencies to Source Control</span></span>

<span data-ttu-id="49351-157">При добавлении проекта или решения в систему управления версиями, все файлы и папки, проекта или решения, также добавляется.</span><span class="sxs-lookup"><span data-stu-id="49351-157">When you add a project or solution to source control, any files and folders within your project or solution will also be added.</span></span> <span data-ttu-id="49351-158">В большинстве случаев проекты и решения также полагайтесь на внешних зависимостей, например локальных сборок, для правильной работы функций.</span><span class="sxs-lookup"><span data-stu-id="49351-158">However, in a lot of cases, projects and solutions also rely on external dependencies, like local assemblies, to function properly.</span></span> <span data-ttu-id="49351-159">Необходимо добавить такие ресурсы в систему управления версиями, чтобы позволить Team Build и другими членами команды разработчиков сборки кода успешно.</span><span class="sxs-lookup"><span data-stu-id="49351-159">You need to add any such resources to source control to let both Team Build and other members of the developer team build your code successfully.</span></span>

<span data-ttu-id="49351-160">Например структуру папок для диспетчера контактов образец решения включает в себя папку с именем пакетов.</span><span class="sxs-lookup"><span data-stu-id="49351-160">For example, the folder structure for the Contact Manager sample solution includes a folder named packages.</span></span> <span data-ttu-id="49351-161">Она содержит различные вспомогательные ресурсы и сборки для ADO.NET Entity Framework 4.1.</span><span class="sxs-lookup"><span data-stu-id="49351-161">This contains the assembly and various supporting resources for the ADO.NET Entity Framework 4.1.</span></span> <span data-ttu-id="49351-162">«Пакеты» не является частью решения диспетчера контактов, но решение не будет успешно построен без него.</span><span class="sxs-lookup"><span data-stu-id="49351-162">The packages folder is not part of the Contact Manager solution, but the solution will not build successfully without it.</span></span> <span data-ttu-id="49351-163">Чтобы включить Team Build для построения решения, необходимо добавить папку пакеты в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="49351-163">To enable Team Build to build the solution, you need to add the packages folder to source control.</span></span>

> [!NOTE]
> <span data-ttu-id="49351-164">Включение папки пакеты типично того, что происходит при добавлении платформы Entity Framework или сходные ресурсы к решению, используя расширение NuGet для Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="49351-164">The inclusion of a packages folder is typical of what happens when you add the Entity Framework, or similar resources, to your solution using the NuGet extension for Visual Studio 2010.</span></span>


<span data-ttu-id="49351-165">**Для добавления содержимого не проекта в системе управления версиями**</span><span class="sxs-lookup"><span data-stu-id="49351-165">**To add non-project content to source control**</span></span>

1. <span data-ttu-id="49351-166">Убедитесь, что элементы, которые требуется добавить (например, «пакеты») в соответствующем расположении в сопоставленной папке в локальной файловой системе.</span><span class="sxs-lookup"><span data-stu-id="49351-166">Ensure that the items you want to add (for example, the packages folder) are in an appropriate location within a mapped folder on your local file system.</span></span>
2. <span data-ttu-id="49351-167">В Visual Studio 2010 в **Team Explorer** окна, разверните узел командного проекта, а затем дважды щелкните **системы управления версиями**.</span><span class="sxs-lookup"><span data-stu-id="49351-167">In Visual Studio 2010, In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image10.png)
3. <span data-ttu-id="49351-168">На **обозревателя управления исходным кодом** вкладке **папки** выберите папку, которая содержит элемент или элементы вы хотите добавить.</span><span class="sxs-lookup"><span data-stu-id="49351-168">On the **Source Control Explorer** tab, in the **Folders** pane, select the folder that contains the item or items you want to add.</span></span>
4. <span data-ttu-id="49351-169">Нажмите кнопку **добавить элементы в папку** кнопки.</span><span class="sxs-lookup"><span data-stu-id="49351-169">Click the **Add Items to Folder** button.</span></span>

    ![](adding-content-to-source-control/_static/image11.png)
5. <span data-ttu-id="49351-170">В **добавить в систему управления версиями** диалогового окна выберите папку или элементы, которые необходимо добавить, а затем нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="49351-170">In the **Add to Source Control** dialog box, select the folder or items you want to add, and then click **Next**.</span></span>

    ![](adding-content-to-source-control/_static/image12.png)
6. <span data-ttu-id="49351-171">На **Исключенные элементы** выберите все нужные элементы автоматически исключаются (например, для сборок), а затем нажмите кнопку **элементы, относящиеся к**.</span><span class="sxs-lookup"><span data-stu-id="49351-171">On the **Excluded items** tab, select any required items that have been automatically excluded (for example, assemblies), and then click **Include item(s)**.</span></span>

    ![](adding-content-to-source-control/_static/image13.png)
7. <span data-ttu-id="49351-172">На **элементы для добавления** убедитесь, что перечислены все файлы, необходимо включить и нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="49351-172">On the **Items to add** tab, verify that all the files you want to include are listed, and then click **Finish**.</span></span>

    ![](adding-content-to-source-control/_static/image14.png)
8. <span data-ttu-id="49351-173">В **обозревателя управления исходным кодом** окно, нажмите кнопку **вернуть** кнопки.</span><span class="sxs-lookup"><span data-stu-id="49351-173">In the **Source Control Explorer** window, click the **Check In** button.</span></span>

    ![](adding-content-to-source-control/_static/image15.png)
9. <span data-ttu-id="49351-174">В **возврата – исходные файлы** диалоговое окно, введите комментарий и нажмите кнопку **вернуть**.</span><span class="sxs-lookup"><span data-stu-id="49351-174">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

<span data-ttu-id="49351-175">На этом этапе вы добавили внешние зависимости для вашего решения в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="49351-175">At this point, you have added the external dependencies for your solution to source control.</span></span>

## <a name="conclusion"></a><span data-ttu-id="49351-176">Заключение</span><span class="sxs-lookup"><span data-stu-id="49351-176">Conclusion</span></span>

<span data-ttu-id="49351-177">В этом разделе описано, как подключиться к командному проекту, сопоставление структуру папок и добавления содержимого в системе управления версиями.</span><span class="sxs-lookup"><span data-stu-id="49351-177">This topic described how to connect to a team project, map a folder structure, and add content to source control.</span></span> <span data-ttu-id="49351-178">Дополнительные сведения о том, как работать с элементами в системе управления версиями см. в разделе [использование управления версиями](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="49351-178">For more information on how to work with items under source control, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

<span data-ttu-id="49351-179">Следующий раздел [Настройка сервера сборки TFS для развертывания веб-](configuring-a-tfs-build-server-for-web-deployment.md), описывает, как подготовить сервер TFS Team Build для построения и развертывания решения.</span><span class="sxs-lookup"><span data-stu-id="49351-179">The next topic, [Configuring a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md), describes how to prepare a TFS Team Build server to build and deploy your solution.</span></span>

## <a name="further-reading"></a><span data-ttu-id="49351-180">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="49351-180">Further Reading</span></span>

<span data-ttu-id="49351-181">Более подробные сведения о работе с системой управления версиями в Team Foundation Server см. в разделе [использование управления версиями](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="49351-181">For more comprehensive information on working with source control in TFS, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="49351-182">[Назад](creating-a-team-project-in-tfs.md)
> [Вперед](configuring-a-tfs-build-server-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="49351-182">[Previous](creating-a-team-project-in-tfs.md)
[Next](configuring-a-tfs-build-server-for-web-deployment.md)</span></span>
