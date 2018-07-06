---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Веб-развертывание ASP.NET с помощью Visual Studio: развертывание обновления кода | Документация Майкрософт'
author: tdykstra
description: В этой серии руководств показано, как развернуть ASP.NET (публикации) веб-приложения, веб-приложениях службы приложений Azure или у стороннего поставщика размещения, Пол...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: c058d4314ea0bed87e22c0e6b3fa09b89b2423d9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833359"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a><span data-ttu-id="961fa-103">Веб-развертывание ASP.NET с помощью Visual Studio: развертывание обновления кода</span><span class="sxs-lookup"><span data-stu-id="961fa-103">ASP.NET Web Deployment using Visual Studio: Deploying a Code Update</span></span>
====================
<span data-ttu-id="961fa-104">по [том Дайкстра](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="961fa-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="961fa-105">Загрузите начальный проект</span><span class="sxs-lookup"><span data-stu-id="961fa-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="961fa-106">В этой серии руководств показано, как развернуть ASP.NET (публикации) веб-приложения для веб-приложениях службы приложений Azure или стороннего поставщика услуг размещения, с помощью Visual Studio 2012 или Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="961fa-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="961fa-107">Сведения об этой серии см. в разделе [в первом учебнике серии](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="961fa-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="961fa-108">Обзор</span><span class="sxs-lookup"><span data-stu-id="961fa-108">Overview</span></span>

<span data-ttu-id="961fa-109">После первоначального развертывания продолжает работу обслуживание и разработка веб-сайт, и вскоре требуется развернуть обновления.</span><span class="sxs-lookup"><span data-stu-id="961fa-109">After the initial deployment, your work of maintaining and developing your web site continues, and before long you will want to deploy an update.</span></span> <span data-ttu-id="961fa-110">В этом руководстве описываются процесс развертывания обновления в код приложения.</span><span class="sxs-lookup"><span data-stu-id="961fa-110">This tutorial takes you through the process of deploying an update to your application code.</span></span> <span data-ttu-id="961fa-111">Обновление, реализации и развертывании в этом руководстве не влечет за собой изменение базы данных; Вы увидите, чем отличается развертывание изменения базы данных в следующем учебном курсе.</span><span class="sxs-lookup"><span data-stu-id="961fa-111">The update that you implement and deploy in this tutorial does not involve a database change; you'll see what's different about deploying a database change in the next tutorial.</span></span>

<span data-ttu-id="961fa-112">Напоминание: Если что-то не работает, как работать с учебником, вы получаете сообщение об ошибке, не забудьте установить флажок [страница устранения неполадок](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="961fa-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="make-a-code-change"></a><span data-ttu-id="961fa-113">Изменения кода</span><span class="sxs-lookup"><span data-stu-id="961fa-113">Make a code change</span></span>

<span data-ttu-id="961fa-114">В качестве простого примера обновления для приложения, вы добавите **преподавателей** странице Список курсов, проводимых выбранным преподавателем.</span><span class="sxs-lookup"><span data-stu-id="961fa-114">As a simple example of an update to your application, you'll add to the **Instructors** page a list of courses taught by the selected instructor.</span></span>

<span data-ttu-id="961fa-115">При запуске **преподавателей** страницы, можно заметить, что существуют **выберите** ссылки в сетке, но они не было присвоено имя, отличное от марки серый цвет фона Включение строк.</span><span class="sxs-lookup"><span data-stu-id="961fa-115">If you run the **Instructors** page, you'll notice that there are **Select** links in the grid, but they don't do anything other than make the row background turn gray.</span></span>

![Странице "Instructors" с выбором](deploying-a-code-update/_static/image1.png)

<span data-ttu-id="961fa-117">Теперь добавьте код, который выполняется при запуске **выберите** ссылку нажатии и отображает список курсов, проводимых выбранным преподавателем.</span><span class="sxs-lookup"><span data-stu-id="961fa-117">Now you'll add code that runs when the **Select** link is clicked and displays a list of courses taught by the selected instructor .</span></span>

1. <span data-ttu-id="961fa-118">В *Instructors.aspx*, добавьте следующую разметку сразу после **ErrorMessageLabel** `Label` управления:</span><span class="sxs-lookup"><span data-stu-id="961fa-118">In *Instructors.aspx*, add the following markup immediately after the **ErrorMessageLabel** `Label` control:</span></span>

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. <span data-ttu-id="961fa-119">Откройте страницу и выберите преподавателя.</span><span class="sxs-lookup"><span data-stu-id="961fa-119">Run the page and select an instructor.</span></span> <span data-ttu-id="961fa-120">Появится список курсов, проводимых этого преподавателя.</span><span class="sxs-lookup"><span data-stu-id="961fa-120">You see a list of courses taught by that instructor.</span></span>

    ![Странице "Instructors" с помощью курсов обучения](deploying-a-code-update/_static/image2.png)
3. <span data-ttu-id="961fa-122">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="961fa-122">Close the browser.</span></span>

## <a name="deploy-the-code-update-to-the-test-environment"></a><span data-ttu-id="961fa-123">Развертывание обновления кода в тестовой среде</span><span class="sxs-lookup"><span data-stu-id="961fa-123">Deploy the code update to the test environment</span></span>

<span data-ttu-id="961fa-124">Перед использованием профилей публикации для развертывания для тестирования, промежуточной и рабочей среде, необходимо изменить параметры публикации базы данных.</span><span class="sxs-lookup"><span data-stu-id="961fa-124">Before you can use your publish profiles to deploy to test, staging, and production, you need to change database publishing options.</span></span> <span data-ttu-id="961fa-125">Больше не нужно выполнять скрипты развертывания grant и данных для базы данных членства.</span><span class="sxs-lookup"><span data-stu-id="961fa-125">You no longer need to run the grant and data deployment scripts for the membership database.</span></span>

1. <span data-ttu-id="961fa-126">Откройте **веб-публикации** мастера, щелкнув правой кнопкой мыши проект ContosoUniversity команду **публикации**.</span><span class="sxs-lookup"><span data-stu-id="961fa-126">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="961fa-127">Нажмите кнопку **теста** профиля в **профиль** стрелку раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="961fa-127">Click the **Test** profile in the **Profile** drop-down list.</span></span>
3. <span data-ttu-id="961fa-128">Нажмите кнопку **параметры** вкладки.</span><span class="sxs-lookup"><span data-stu-id="961fa-128">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="961fa-129">В разделе **DefaultConnection** в **баз данных** снимите **обновления базы данных** "флажок".</span><span class="sxs-lookup"><span data-stu-id="961fa-129">Under **DefaultConnection** in the **Databases** section, clear the **Update database** check box.</span></span>
5. <span data-ttu-id="961fa-130">Нажмите кнопку **профиль** , а затем щелкните **промежуточной** профиля в **профиль** стрелку раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="961fa-130">Click the **Profile** tab, and then click the **Staging** profile in the **Profile** drop-down list.</span></span>
6. <span data-ttu-id="961fa-131">При появлении, если вы хотите сохранить изменения, внесенные в **теста** , установите флажок **Да**.</span><span class="sxs-lookup"><span data-stu-id="961fa-131">When you are asked if you want to save the changes made to the **Test** profile, click **Yes**.</span></span>
7. <span data-ttu-id="961fa-132">Сделать то же изменение в профиле промежуточного хранения.</span><span class="sxs-lookup"><span data-stu-id="961fa-132">Make the same change in the Staging profile.</span></span>
8. <span data-ttu-id="961fa-133">Повторите процедуру, чтобы сделать то же изменение в профиле рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="961fa-133">Repeat the process to make the same change in the Production profile.</span></span>
9. <span data-ttu-id="961fa-134">Закрыть **веб-публикации** мастера.</span><span class="sxs-lookup"><span data-stu-id="961fa-134">Close the **Publish Web** wizard.</span></span>

<span data-ttu-id="961fa-135">Развертывание в тестовую среду — теперь просто выполняется одним щелчком повторной публикации.</span><span class="sxs-lookup"><span data-stu-id="961fa-135">Deploying to the test environment is now a simple matter of running one-click publish again.</span></span> <span data-ttu-id="961fa-136">Чтобы сделать этот процесс быстрее, можно использовать **веб-публикация одним щелкните** панели инструментов.</span><span class="sxs-lookup"><span data-stu-id="961fa-136">To make this process quicker, you can use the **Web One Click Publish** toolbar.</span></span>

1. <span data-ttu-id="961fa-137">В **представление** меню, выберите **панелей инструментов** , а затем выберите **веб-публикация одним щелкните**.</span><span class="sxs-lookup"><span data-stu-id="961fa-137">In the **View** menu, choose **Toolbars** and then select **Web One Click Publish**.</span></span>

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. <span data-ttu-id="961fa-139">В **обозревателе решений**, выберите проект ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="961fa-139">In **Solution Explorer**, select the ContosoUniversity project.</span></span>
3. <span data-ttu-id="961fa-140">**веб-публикация одним щелкните** панели инструментов выберите **теста** профиль публикации, а затем нажмите кнопку **веб-публикации** (значок со стрелками влево и вправо).</span><span class="sxs-lookup"><span data-stu-id="961fa-140">the **Web One Click Publish** toolbar, choose the **Test** publish profile and then click **Publish Web** (the icon with arrows pointing left and right).</span></span>

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. <span data-ttu-id="961fa-142">Visual Studio развертывает обновленное приложение и в браузере автоматически откроется на домашнюю страницу.</span><span class="sxs-lookup"><span data-stu-id="961fa-142">Visual Studio deploys the updated application, and the browser automatically opens to the home page.</span></span>
5. <span data-ttu-id="961fa-143">Откройте страницу Instructors и выберите преподавателя, чтобы проверить, что обновление успешно развернут.</span><span class="sxs-lookup"><span data-stu-id="961fa-143">Run the Instructors page and select an instructor to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="961fa-144">Обычным также выполнить тестирование регрессии (то есть проверка остальной части сайта, чтобы убедиться в том, что новое изменение не нарушили существующие функциональные возможности).</span><span class="sxs-lookup"><span data-stu-id="961fa-144">You would normally also do regression testing (that is, test the rest of the site to make sure that the new change didn't break any existing functionality).</span></span> <span data-ttu-id="961fa-145">Но в этом руководстве можно будет пропустить этот шаг и следовать для развертывания обновления в промежуточной и рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="961fa-145">But for this tutorial you'll skip that step and proceed to deploy the update to staging and production.</span></span>

<span data-ttu-id="961fa-146">При повторном развертывании, веб-развертывания автоматически определяет, какие файлы были изменены, и копирует только измененные файлы на сервер.</span><span class="sxs-lookup"><span data-stu-id="961fa-146">When you redeploy, Web Deploy automatically determines which files have changed and only copies changed files to the server.</span></span> <span data-ttu-id="961fa-147">По умолчанию веб-развертывание использует даты последнего изменения в файлы для определения, какие из них были изменены.</span><span class="sxs-lookup"><span data-stu-id="961fa-147">By default, Web Deploy uses last-changed dates on files to determine which ones have changed.</span></span> <span data-ttu-id="961fa-148">Некоторые системы управления версиями изменения файла даты даже не изменяйте содержимое файла.</span><span class="sxs-lookup"><span data-stu-id="961fa-148">Some source control systems change file dates even when you don't change the file contents.</span></span> <span data-ttu-id="961fa-149">В этом случае можно настроить веб-развертывание для использования контрольных сумм для определения, какие файлы были изменены.</span><span class="sxs-lookup"><span data-stu-id="961fa-149">In that case, you might want to configure Web Deploy to use file checksums to determine which files have changed.</span></span> <span data-ttu-id="961fa-150">Дополнительные сведения см. в разделе [почему все мои файлы повторном развертывании несмотря на то, что я не изменили их?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) в часто задаваемых вопросов развертывания ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="961fa-150">For more information, see [Why do all of my files get redeployed although I didn't change them?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) in the ASP.NET Deployment FAQ.</span></span>

## <a name="take-the-application-offline-during-deployment"></a><span data-ttu-id="961fa-151">Возьмем приложение в автономный режим во время развертывания</span><span class="sxs-lookup"><span data-stu-id="961fa-151">Take the application offline during deployment</span></span>

<span data-ttu-id="961fa-152">Изменение, которое вы развертываете теперь является простое изменение на одной странице.</span><span class="sxs-lookup"><span data-stu-id="961fa-152">The change you're deploying now is a simple change to a single page.</span></span> <span data-ttu-id="961fa-153">Но иногда развертывание более масштабным изменениям, или развертывании изменений кода и базы данных и сайт может работать неправильно, если пользователь запрашивает страницу до завершения развертывания.</span><span class="sxs-lookup"><span data-stu-id="961fa-153">But sometimes you deploy larger changes, or you deploy both code and database changes, and the site might behave incorrectly if a user requests a page before deployment is finished.</span></span> <span data-ttu-id="961fa-154">Чтобы запретить пользователям доступ к сайту во время развертывания, можно использовать *приложения\_offline.htm* файла.</span><span class="sxs-lookup"><span data-stu-id="961fa-154">To prevent users from accessing the site while deployment is in progress, you can use an *app\_offline.htm* file.</span></span> <span data-ttu-id="961fa-155">Если поместить файл с именем *приложения\_offline.htm* в корневой папке приложения, службы IIS автоматически отображает этот файл, а не посредством запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="961fa-155">When you put a file named *app\_offline.htm* in the root folder of your application, IIS automatically displays that file instead of running your application.</span></span> <span data-ttu-id="961fa-156">Поэтому для предотвращения доступа во время развертывания, находиться *приложения\_offline.htm* в корневой папке, запустите процесс развертывания, а затем удалите *приложения\_offline.htm* после успешной развертывание.</span><span class="sxs-lookup"><span data-stu-id="961fa-156">So to prevent access during deployment, you put *app\_offline.htm* in the root folder, run the deployment process, and then remove *app\_offline.htm* after successful deployment.</span></span>

<span data-ttu-id="961fa-157">Можно настроить веб-развертывания для автоматического размещения по умолчанию *приложения\_offline.htm* файл на сервере при запуске развертывания и удалите его после его завершения.</span><span class="sxs-lookup"><span data-stu-id="961fa-157">You can configure Web Deploy to automatically put a default *app\_offline.htm* file on the server when it starts deploying and remove it when it finishes.</span></span> <span data-ttu-id="961fa-158">Что все, что необходимо сделать это добавьте следующий XML-элемент для публикации (.pubxml) профиля:</span><span class="sxs-lookup"><span data-stu-id="961fa-158">To do that all you have to do is add the following XML element to your publish profile (.pubxml) file:</span></span>

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

<span data-ttu-id="961fa-159">В этом руководстве вы узнаете, как создать и использовать пользовательский *приложения\_offline.htm* файла.</span><span class="sxs-lookup"><span data-stu-id="961fa-159">For this tutorial you'll see how to create and use a custom *app\_offline.htm* file.</span></span>

<span data-ttu-id="961fa-160">С помощью *приложения\_offline.htm* на промежуточном сайте не требуется, так как у вас нет пользователей, обращающихся к промежуточный сайт.</span><span class="sxs-lookup"><span data-stu-id="961fa-160">Using *app\_offline.htm* in the staging site isn't required, because you don't have users accessing the staging site.</span></span> <span data-ttu-id="961fa-161">Но рекомендуется использовать промежуточного хранения для все проверки того, который вы планируете развернуть в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="961fa-161">But it's a good practice to use staging to test everything the way you plan to deploy in production.</span></span>

### <a name="create-appofflinehtm"></a><span data-ttu-id="961fa-162">Создание приложения\_offline.htm</span><span class="sxs-lookup"><span data-stu-id="961fa-162">Create app\_offline.htm</span></span>

1. <span data-ttu-id="961fa-163">В **обозревателе решений**, щелкните правой кнопкой мыши решение и нажмите кнопку **добавить**, а затем нажмите кнопку **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="961fa-163">In **Solution Explorer**, right-click the solution and click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="961fa-164">Создание **HTML-страницу** с именем *приложения\_offline.htm* (Удаление последней «l» в *.html* расширение, Visual Studio по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="961fa-164">Create an **HTML Page** named *app\_offline.htm* (delete the final "l" in the *.html* extension that Visual Studio creates by default).</span></span>
3. <span data-ttu-id="961fa-165">Замените разметку шаблона следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="961fa-165">Replace the template markup with the following markup:</span></span>

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. <span data-ttu-id="961fa-166">Сохраните и закройте файл.</span><span class="sxs-lookup"><span data-stu-id="961fa-166">Save and close the file.</span></span>

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a><span data-ttu-id="961fa-167">Копирование приложения\_offline.htm в корневую папку веб-сайта</span><span class="sxs-lookup"><span data-stu-id="961fa-167">Copy app\_offline.htm to the root folder of the web site</span></span>

<span data-ttu-id="961fa-168">Можно использовать любые средства FTP для копирования файлов на веб-сайт.</span><span class="sxs-lookup"><span data-stu-id="961fa-168">You can use any FTP tool to copy files to the web site.</span></span> <span data-ttu-id="961fa-169">[FileZilla](http://filezilla-project.org/) — это популярный инструмент FTP и показан на снимках экрана.</span><span class="sxs-lookup"><span data-stu-id="961fa-169">[FileZilla](http://filezilla-project.org/) is a popular FTP tool and is shown in the screen shots.</span></span>

<span data-ttu-id="961fa-170">Чтобы использовать средство FTP, необходимы три вещи: URL-адрес FTP, имя пользователя и пароль.</span><span class="sxs-lookup"><span data-stu-id="961fa-170">To use an FTP tool, you need three things: the FTP URL, the user name, and the password.</span></span>

<span data-ttu-id="961fa-171">URL-адрес отображается на странице панели мониторинга веб сайта на портале управления Azure и имя пользователя и пароль для FTP-сервера можно найти в *.publishsettings* файл, который вы загрузили ранее.</span><span class="sxs-lookup"><span data-stu-id="961fa-171">The URL is shown on the web site's dashboard page in the Azure Management Portal, and the user name and password for FTP can be found in the *.publishsettings* file that you downloaded earlier.</span></span> <span data-ttu-id="961fa-172">Ниже показано, как получить эти значения.</span><span class="sxs-lookup"><span data-stu-id="961fa-172">The following steps show how to get these values.</span></span>

1. <span data-ttu-id="961fa-173">На портале управления Azure щелкните **веб-сайтов** вкладку, а затем нажмите кнопку промежуточного веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="961fa-173">In the Azure Management Portal, click **Web Sites** tab and then click the staging web site.</span></span>
2. <span data-ttu-id="961fa-174">На **панели мониторинга** странице прокрутите вниз, чтобы найти имя узла FTP в **быстрый обзор** раздел.</span><span class="sxs-lookup"><span data-stu-id="961fa-174">On the **Dashboard** page, scroll down to find the FTP host name in the **Quick Glance** section.</span></span>

    ![Имя узла FTP](deploying-a-code-update/_static/image5.png)
3. <span data-ttu-id="961fa-176">Откройте промежуточной *.publishsettings* файл в блокноте или другом текстовом редакторе.</span><span class="sxs-lookup"><span data-stu-id="961fa-176">Open the staging *.publishsettings* file in Notepad or another text editor.</span></span>
4. <span data-ttu-id="961fa-177">Найти `publishProfile` элемент, для FTP-профиля.</span><span class="sxs-lookup"><span data-stu-id="961fa-177">Find the `publishProfile` element for the FTP profile.</span></span>
5. <span data-ttu-id="961fa-178">Копировать `userName` и `userPWD` значения.</span><span class="sxs-lookup"><span data-stu-id="961fa-178">Copy the `userName` and `userPWD` values.</span></span>

    ![Учетные данные FTP](deploying-a-code-update/_static/image6.png)
6. <span data-ttu-id="961fa-180">Откройте FTP-клиент и войдите в систему URL-адрес FTP.</span><span class="sxs-lookup"><span data-stu-id="961fa-180">Open your FTP tool and log on to the FTP URL.</span></span>
7. <span data-ttu-id="961fa-181">Копировать *приложения\_offline.htm* из папки решения для */site/wwwroot* папку на промежуточном сайте.</span><span class="sxs-lookup"><span data-stu-id="961fa-181">Copy *app\_offline.htm* from the solution folder to the */site/wwwroot* folder in the staging site.</span></span>

    ![Скопируйте app_offline](deploying-a-code-update/_static/image7.png)
8. <span data-ttu-id="961fa-183">Перейдите по URL-АДРЕСУ промежуточный сайт.</span><span class="sxs-lookup"><span data-stu-id="961fa-183">Browse to your staging site's URL.</span></span> <span data-ttu-id="961fa-184">Вы увидите, что *приложения\_offline.htm* страницы будет отображаться вместо домашней страницы.</span><span class="sxs-lookup"><span data-stu-id="961fa-184">You see that the *app\_offline.htm* page is now displayed instead of your home page.</span></span>

    ![App_offline.htm в окне браузера](deploying-a-code-update/_static/image8.png)

<span data-ttu-id="961fa-186">Теперь вы готовы для развертывания в промежуточную среду.</span><span class="sxs-lookup"><span data-stu-id="961fa-186">You are now ready to deploy to staging.</span></span>

## <a name="deploy-the-code-update-to-staging-and-production"></a><span data-ttu-id="961fa-187">Развертывание обновления кода в промежуточной и рабочей средах</span><span class="sxs-lookup"><span data-stu-id="961fa-187">Deploy the code update to staging and production</span></span>

1. <span data-ttu-id="961fa-188">В **веб-публикация одним щелкните** панели инструментов выберите **промежуточной** профиль публикации, а затем нажмите кнопку **веб-публикации**.</span><span class="sxs-lookup"><span data-stu-id="961fa-188">In the **Web One Click Publish** toolbar, choose the **Staging** publish profile and then click **Publish Web**.</span></span>

    <span data-ttu-id="961fa-189">Visual Studio развернет обновленное приложение и откроет в браузере домашнюю страницу.</span><span class="sxs-lookup"><span data-stu-id="961fa-189">Visual Studio deploys the updated application and opens the browser to the site's home page.</span></span> <span data-ttu-id="961fa-190">*Приложения\_offline.htm* отобразится файл.</span><span class="sxs-lookup"><span data-stu-id="961fa-190">The *app\_offline.htm* file is displayed.</span></span> <span data-ttu-id="961fa-191">Перед тестированием для проверки успешного развертывания, необходимо удалить *приложения\_offline.htm* файла.</span><span class="sxs-lookup"><span data-stu-id="961fa-191">Before you can test to verify successful deployment, you must remove the *app\_offline.htm* file.</span></span>
2. <span data-ttu-id="961fa-192">Вернитесь в ваше средство FTP и удалите **приложения\_offline.htm** промежуточного сайта.</span><span class="sxs-lookup"><span data-stu-id="961fa-192">Return to your FTP tool, and delete **app\_offline.htm** from the staging site.</span></span>
3. <span data-ttu-id="961fa-193">В браузере откройте страницу Instructors в промежуточный сайт и выберите преподавателя, чтобы проверить, что обновление успешно развернут.</span><span class="sxs-lookup"><span data-stu-id="961fa-193">In the browser, open the Instructors page in the staging site, and select an instructor to verify that the update was successfully deployed.</span></span>
4. <span data-ttu-id="961fa-194">Выполните ту же процедуру для рабочей среды, как это делалось промежуточного хранения.</span><span class="sxs-lookup"><span data-stu-id="961fa-194">Follow the same procedure for production as you did for staging.</span></span>

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a><span data-ttu-id="961fa-195">Просмотр изменений и развертывание определенных файлов</span><span class="sxs-lookup"><span data-stu-id="961fa-195">Reviewing Changes and Deploying Specific Files</span></span>

<span data-ttu-id="961fa-196">Visual Studio 2012 также дает возможность развертывать отдельные файлы.</span><span class="sxs-lookup"><span data-stu-id="961fa-196">Visual Studio 2012 also gives you the ability to deploy individual files.</span></span> <span data-ttu-id="961fa-197">Для выбранного файла можно просмотреть различия между локальной и развернутой версии, разверните файл в целевой среде или скопируйте файл из целевой среды для локального проекта.</span><span class="sxs-lookup"><span data-stu-id="961fa-197">For a selected file you can view differences between the local version and the deployed version, deploy the file to the destination environment, or copy the file from the destination environment to the local project.</span></span> <span data-ttu-id="961fa-198">В этом разделе руководства вы научитесь использовать эти возможности.</span><span class="sxs-lookup"><span data-stu-id="961fa-198">In this section of the tutorial you see how to use these features.</span></span>

### <a name="make-a-change-to-deploy"></a><span data-ttu-id="961fa-199">Изменения для развертывания</span><span class="sxs-lookup"><span data-stu-id="961fa-199">Make a change to deploy</span></span>

1. <span data-ttu-id="961fa-200">Откройте *Content/Site.css*и найти блок для `body` тега.</span><span class="sxs-lookup"><span data-stu-id="961fa-200">Open *Content/Site.css*, and find the block for the `body` tag.</span></span>
2. <span data-ttu-id="961fa-201">Измените значение `background-color` из `#fff` для `darkblue`.</span><span class="sxs-lookup"><span data-stu-id="961fa-201">Change the value for `background-color` from `#fff` to `darkblue`.</span></span>

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a><span data-ttu-id="961fa-202">Просмотр изменений в окне Просмотр публикации</span><span class="sxs-lookup"><span data-stu-id="961fa-202">View the change in the Publish Preview window</span></span>

<span data-ttu-id="961fa-203">При использовании **веб-публикации** мастер должен опубликовать проект, вы увидите, что изменения будут опубликованы, дважды щелкнув файл в **предварительной версии** окна.</span><span class="sxs-lookup"><span data-stu-id="961fa-203">When you use the **Publish Web** wizard to publish the project, you can see what changes are going to be published by double-clicking the file in the **Preview** window.</span></span>

1. <span data-ttu-id="961fa-204">Щелкните правой кнопкой мыши проект ContosoUniversity и нажмите кнопку **публикации**.</span><span class="sxs-lookup"><span data-stu-id="961fa-204">Right-click the ContosoUniversity project and click **Publish**.</span></span>
2. <span data-ttu-id="961fa-205">Из **профиль** стрелку раскрывающегося списка выберите **теста** профиль публикации.</span><span class="sxs-lookup"><span data-stu-id="961fa-205">From the **Profile** drop-down list, select the **Test** publish profile.</span></span>
3. <span data-ttu-id="961fa-206">Нажмите кнопку **предварительной версии**, а затем нажмите кнопку **начать просмотр**.</span><span class="sxs-lookup"><span data-stu-id="961fa-206">Click **Preview**, and then click **Start Preview**.</span></span>
4. <span data-ttu-id="961fa-207">В **предварительной версии** панель, дважды щелкните **Site.css**.</span><span class="sxs-lookup"><span data-stu-id="961fa-207">In the **Preview** pane, double-click **Site.css**.</span></span>

    ![Дважды щелкните файл Site.css](deploying-a-code-update/_static/image9.png)

    <span data-ttu-id="961fa-209">**Предварительный просмотр изменений** диалоговое окно отображается предварительный просмотр изменений, которые будут развернуты.</span><span class="sxs-lookup"><span data-stu-id="961fa-209">The **Preview changes** dialog shows a preview of the changes that will be deployed.</span></span>

    ![Просмотр изменений в Site.css](deploying-a-code-update/_static/image10.png)

    <span data-ttu-id="961fa-211">Если дважды щелкнуть *Web.config* файл, **Предварительный просмотр изменений** диалогового окна показан результат построения преобразования конфигураций и опубликовать профиль преобразования.</span><span class="sxs-lookup"><span data-stu-id="961fa-211">If you double-click the *Web.config* file, the **Preview changes** dialog shows the effect of your build configuration transformations and publish profile transformations.</span></span> <span data-ttu-id="961fa-212">На этом этапе вы ничего не сделали, приводящие к *Web.config* файла на сервере, чтобы изменить, поэтому вы ожидаете увидеть без изменений.</span><span class="sxs-lookup"><span data-stu-id="961fa-212">At this point you have not done anything that would cause the *Web.config* file on the server to change, so you expect to see no changes.</span></span> <span data-ttu-id="961fa-213">Тем не менее **Предварительный просмотр изменений** окне неправильно отображаются два изменения.</span><span class="sxs-lookup"><span data-stu-id="961fa-213">However, the **Preview changes** window incorrectly shows two changes.</span></span> <span data-ttu-id="961fa-214">Похоже, два XML-элементы будут удалены.</span><span class="sxs-lookup"><span data-stu-id="961fa-214">It looks like two XML elements will be removed.</span></span> <span data-ttu-id="961fa-215">Эти элементы добавляются процессом публикации, при выборе **выполнить Code First Migrations при запуске приложения** для Code First класс контекста.</span><span class="sxs-lookup"><span data-stu-id="961fa-215">These elements are added by the publish process when you select **Execute Code First Migrations on application start** for a Code First context class.</span></span> <span data-ttu-id="961fa-216">Сравнение выполняется в том случае, прежде чем добавить процесс публикации этих элементов, так выглядит так, будто они удаляются несмотря на то, что они не будут удалены.</span><span class="sxs-lookup"><span data-stu-id="961fa-216">The comparison is done before the publish process adds those elements, so it looks like they are being removed although they will not be removed.</span></span> <span data-ttu-id="961fa-217">Эта ошибка будет исправлена в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="961fa-217">This error will be corrected in a future release.</span></span>
5. <span data-ttu-id="961fa-218">Нажмите кнопку **Закрыть**.</span><span class="sxs-lookup"><span data-stu-id="961fa-218">Click **Close**.</span></span>
6. <span data-ttu-id="961fa-219">Нажмите кнопку **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="961fa-219">Click **Publish**.</span></span>
7. <span data-ttu-id="961fa-220">Когда откроется браузер на домашнюю страницу сайта теста, нажмите CTRL + F5, чтобы вызывает жестких обновления, чтобы увидеть эффект от изменения CSS.</span><span class="sxs-lookup"><span data-stu-id="961fa-220">When the browser opens to the home page of the Test site, press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Эффект изменения CSS](deploying-a-code-update/_static/image11.png)
8. <span data-ttu-id="961fa-222">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="961fa-222">Close the browser.</span></span>

### <a name="publish-specific-files-from-solution-explorer"></a><span data-ttu-id="961fa-223">Опубликовать определенные файлы из обозревателя решений</span><span class="sxs-lookup"><span data-stu-id="961fa-223">Publish specific files from Solution Explorer</span></span>

<span data-ttu-id="961fa-224">Предположим, что не нравится синий фон и восстановить исходный цвет.</span><span class="sxs-lookup"><span data-stu-id="961fa-224">Suppose you don't like the blue background and want to revert to the original color.</span></span> <span data-ttu-id="961fa-225">В этом разделе необходимо восстановить исходные параметры путем публикации определенного файла непосредственно из **обозревателе решений**.</span><span class="sxs-lookup"><span data-stu-id="961fa-225">In this section you'll restore the original settings by publishing a specific file directly from **Solution Explorer**.</span></span>

1. <span data-ttu-id="961fa-226">Откройте *Content/Site.css* копирование и восстановление `background-color` значение `#fff`.</span><span class="sxs-lookup"><span data-stu-id="961fa-226">Open *Content/Site.css* and restore the `background-color` setting to `#fff`.</span></span>
2. <span data-ttu-id="961fa-227">В **обозревателе решений**, щелкните правой кнопкой мыши *Content/Site.css* файла.</span><span class="sxs-lookup"><span data-stu-id="961fa-227">In **Solution Explorer**, right-click the *Content/Site.css* file.</span></span>

    <span data-ttu-id="961fa-228">В контекстном меню показаны три параметров публикации.</span><span class="sxs-lookup"><span data-stu-id="961fa-228">The context menu shows three publish options.</span></span>

    ![Публиковать параметры из обозревателя решений](deploying-a-code-update/_static/image12.png)
3. <span data-ttu-id="961fa-230">Нажмите кнопку **Просмотр изменений в Site.css**.</span><span class="sxs-lookup"><span data-stu-id="961fa-230">Click **Preview changes to Site.css**.</span></span>

    <span data-ttu-id="961fa-231">Откроется окно для отображения различий между локального файла и его версию в целевой среде.</span><span class="sxs-lookup"><span data-stu-id="961fa-231">A window opens to show the differences between the local file and the version of it in the destination environment.</span></span>

    ![Diff-Content/Site.css](deploying-a-code-update/_static/image13.png)
4. <span data-ttu-id="961fa-233">В **обозревателе решений**, щелкните правой кнопкой мыши **Site.css** еще раз и нажмите кнопку **публикации Site.css**.</span><span class="sxs-lookup"><span data-stu-id="961fa-233">In **Solution Explorer**, right-click **Site.css** again and click **Publish Site.css**.</span></span>

    <span data-ttu-id="961fa-234">**Действий веб-публикации** окно показывает, что файл был опубликован.</span><span class="sxs-lookup"><span data-stu-id="961fa-234">The **Web Publish Activity** window shows that the file has been published.</span></span>

    ![Окно действия публикации Web](deploying-a-code-update/_static/image14.png)
5. <span data-ttu-id="961fa-236">Откройте в браузере `http://localhost/contosouniversity` URL-адрес и нажмите клавишу CTRL + F5, чтобы вызвать жесткой обновить, чтобы увидеть эффект CSS изменения.</span><span class="sxs-lookup"><span data-stu-id="961fa-236">Open a browser to the `http://localhost/contosouniversity` URL, and then press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Домашняя страница с обычной CSS](deploying-a-code-update/_static/image15.png)
6. <span data-ttu-id="961fa-238">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="961fa-238">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="961fa-239">Сводка</span><span class="sxs-lookup"><span data-stu-id="961fa-239">Summary</span></span>

<span data-ttu-id="961fa-240">Теперь вы увидели несколько способов развертывания обновления приложения, не связанных с изменениями в базе данных, и вы узнали, как просматривать изменения, чтобы проверить, что будет обновляться ожиданиям.</span><span class="sxs-lookup"><span data-stu-id="961fa-240">You've now seen several ways to deploy an application update that does not involve a database change, and you've seen how to preview the changes to verify that what will be updated is what you expect.</span></span> <span data-ttu-id="961fa-241">Теперь есть страницы преподавателей **курсы под руководством** раздел.</span><span class="sxs-lookup"><span data-stu-id="961fa-241">The Instructors page now has a **Courses Taught** section.</span></span>

![Странице "Instructors" с помощью курсов обучения](deploying-a-code-update/_static/image16.png)

<span data-ttu-id="961fa-243">Следующее руководство показано, как развернуть изменения базы данных: вы добавите поле даты рождения, к базе данных и на страницу преподавателей.</span><span class="sxs-lookup"><span data-stu-id="961fa-243">The next tutorial shows you how to deploy a database change: you'll add a birthdate field to the database and to the Instructors page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="961fa-244">[Назад](deploying-to-production.md)
> [Вперед](deploying-a-database-update.md)</span><span class="sxs-lookup"><span data-stu-id="961fa-244">[Previous](deploying-to-production.md)
[Next](deploying-a-database-update.md)</span></span>
