---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: "ASP.NET веб-развертывания с помощью Visual Studio: развертывание обновления кода | Документы Microsoft"
author: tdykstra
description: "Этот учебник ряд показано развертывание ASP.NET (публикации) веб-приложения для веб-приложениях службы приложений Azure или стороннего поставщика услуг размещения, Пол..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: f6861c702c1ccb19e5a4eee484a622079e205f86
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a><span data-ttu-id="70417-103">ASP.NET веб-развертывания с помощью Visual Studio: развертывание обновления кода</span><span class="sxs-lookup"><span data-stu-id="70417-103">ASP.NET Web Deployment using Visual Studio: Deploying a Code Update</span></span>
====================
<span data-ttu-id="70417-104">По [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="70417-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="70417-105">Загрузите начальный проект</span><span class="sxs-lookup"><span data-stu-id="70417-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="70417-106">Этот учебник ряд показано развертывание ASP.NET (публикации) веб-приложения для веб-приложениях службы приложений Azure или стороннего поставщика услуг размещения, с помощью Visual Studio 2012 или Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="70417-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="70417-107">Сведения о последовательности см. в разделе [в первом учебнике ряда](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="70417-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="70417-108">Обзор</span><span class="sxs-lookup"><span data-stu-id="70417-108">Overview</span></span>

<span data-ttu-id="70417-109">После первоначального развертывания продолжает работу обслуживание и разработка веб-узла и вскоре требуется развернуть обновления.</span><span class="sxs-lookup"><span data-stu-id="70417-109">After the initial deployment, your work of maintaining and developing your web site continues, and before long you will want to deploy an update.</span></span> <span data-ttu-id="70417-110">Этот учебник поможет выполнить процесс развертывания обновления в код приложения.</span><span class="sxs-lookup"><span data-stu-id="70417-110">This tutorial takes you through the process of deploying an update to your application code.</span></span> <span data-ttu-id="70417-111">Обновление, реализации и развертывания в этом учебнике не включает изменения базы данных; Вы увидите отличия в развертывании изменение базы данных в следующем уроке.</span><span class="sxs-lookup"><span data-stu-id="70417-111">The update that you implement and deploy in this tutorial does not involve a database change; you'll see what's different about deploying a database change in the next tutorial.</span></span>

<span data-ttu-id="70417-112">Напоминание: Если вы получаете сообщение об ошибке или что-то не работает, как пройти учебник, не забудьте проверить [страницу устранения неполадок](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="70417-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="make-a-code-change"></a><span data-ttu-id="70417-113">Изменения кода</span><span class="sxs-lookup"><span data-stu-id="70417-113">Make a code change</span></span>

<span data-ttu-id="70417-114">В качестве простого примера обновления в приложение, мы добавим **инструкторов** страница списка выбранных инструктора при изучении курсов.</span><span class="sxs-lookup"><span data-stu-id="70417-114">As a simple example of an update to your application, you'll add to the **Instructors** page a list of courses taught by the selected instructor.</span></span>

<span data-ttu-id="70417-115">При запуске **инструкторов** страницы, вы заметите, что существуют **выберите** ссылки в сетке, но они не совершают ничего, кроме убедитесь серый включить фон строки.</span><span class="sxs-lookup"><span data-stu-id="70417-115">If you run the **Instructors** page, you'll notice that there are **Select** links in the grid, but they don't do anything other than make the row background turn gray.</span></span>

![Страница инструкторов с выделением](deploying-a-code-update/_static/image1.png)

<span data-ttu-id="70417-117">Теперь добавьте код, выполняемый при **выберите** ссылка нажатии и отображает список выбранных инструктора при изучении курсов.</span><span class="sxs-lookup"><span data-stu-id="70417-117">Now you'll add code that runs when the **Select** link is clicked and displays a list of courses taught by the selected instructor .</span></span>

1. <span data-ttu-id="70417-118">В *Instructors.aspx*, добавьте следующую разметку сразу после **ErrorMessageLabel** `Label` управления:</span><span class="sxs-lookup"><span data-stu-id="70417-118">In *Instructors.aspx*, add the following markup immediately after the **ErrorMessageLabel** `Label` control:</span></span>

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. <span data-ttu-id="70417-119">Запустите страницу и выберите инструктор.</span><span class="sxs-lookup"><span data-stu-id="70417-119">Run the page and select an instructor.</span></span> <span data-ttu-id="70417-120">Можно просмотреть список при изучении этого инструктора курсов.</span><span class="sxs-lookup"><span data-stu-id="70417-120">You see a list of courses taught by that instructor.</span></span>

    ![Страница инструкторов с курсов обучения](deploying-a-code-update/_static/image2.png)
3. <span data-ttu-id="70417-122">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="70417-122">Close the browser.</span></span>

## <a name="deploy-the-code-update-to-the-test-environment"></a><span data-ttu-id="70417-123">Развернуть обновление кода в среде тестирования</span><span class="sxs-lookup"><span data-stu-id="70417-123">Deploy the code update to the test environment</span></span>

<span data-ttu-id="70417-124">Перед использованием профилей публикации для развертывания тестов, промежуточной и производственной, необходимо изменить параметры базы данных публикации.</span><span class="sxs-lookup"><span data-stu-id="70417-124">Before you can use your publish profiles to deploy to test, staging, and production, you need to change database publishing options.</span></span> <span data-ttu-id="70417-125">Больше не требуется запуск скриптов развертывания grant и данных для базы данных членства.</span><span class="sxs-lookup"><span data-stu-id="70417-125">You no longer need to run the grant and data deployment scripts for the membership database.</span></span>

1. <span data-ttu-id="70417-126">Откройте **веб-публикация** мастера, щелкнув правой кнопкой мыши проект ContosoUniversity команду **публикации**.</span><span class="sxs-lookup"><span data-stu-id="70417-126">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="70417-127">Нажмите кнопку **тест** профиле в **профиль** раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="70417-127">Click the **Test** profile in the **Profile** drop-down list.</span></span>
3. <span data-ttu-id="70417-128">Нажмите кнопку **параметры** вкладки.</span><span class="sxs-lookup"><span data-stu-id="70417-128">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="70417-129">В разделе **DefaultConnection** в **баз данных** снимите **обновление базы данных** флажок.</span><span class="sxs-lookup"><span data-stu-id="70417-129">Under **DefaultConnection** in the **Databases** section, clear the **Update database** check box.</span></span>
5. <span data-ttu-id="70417-130">Нажмите кнопку **профиль** , а затем щелкните **промежуточной** профиле в **профиль** раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="70417-130">Click the **Profile** tab, and then click the **Staging** profile in the **Profile** drop-down list.</span></span>
6. <span data-ttu-id="70417-131">Когда будет предложено, если вы хотите сохранить изменения, внесенные в **тест** , установите флажок **Да**.</span><span class="sxs-lookup"><span data-stu-id="70417-131">When you are asked if you want to save the changes made to the **Test** profile, click **Yes**.</span></span>
7. <span data-ttu-id="70417-132">Внести такое же изменение в профиле промежуточного хранения.</span><span class="sxs-lookup"><span data-stu-id="70417-132">Make the same change in the Staging profile.</span></span>
8. <span data-ttu-id="70417-133">Повторите процедуру, чтобы внести такое же изменение в профиле рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="70417-133">Repeat the process to make the same change in the Production profile.</span></span>
9. <span data-ttu-id="70417-134">Закрыть **веб-публикация** мастера.</span><span class="sxs-lookup"><span data-stu-id="70417-134">Close the **Publish Web** wizard.</span></span>

<span data-ttu-id="70417-135">Развертывание в среде тестирования — теперь это простой механизм выполнения одним щелчком повторной публикации.</span><span class="sxs-lookup"><span data-stu-id="70417-135">Deploying to the test environment is now a simple matter of running one-click publish again.</span></span> <span data-ttu-id="70417-136">Чтобы сделать этот процесс быстрее, можно использовать **веб-публикация одним щелкните** инструментов.</span><span class="sxs-lookup"><span data-stu-id="70417-136">To make this process quicker, you can use the **Web One Click Publish** toolbar.</span></span>

1. <span data-ttu-id="70417-137">В **представление** меню, выберите **панели инструментов** , а затем выберите **веб-публикация одним щелкните**.</span><span class="sxs-lookup"><span data-stu-id="70417-137">In the **View** menu, choose **Toolbars** and then select **Web One Click Publish**.</span></span>

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. <span data-ttu-id="70417-139">В **обозревателе решений**, выберите проект ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="70417-139">In **Solution Explorer**, select the ContosoUniversity project.</span></span>
3. <span data-ttu-id="70417-140">**веб-публикация одним щелкните** инструментов, выберите **тест** профиль публикации, а затем нажмите кнопку **веб-публикация** (значок со стрелками влево и вправо).</span><span class="sxs-lookup"><span data-stu-id="70417-140">the **Web One Click Publish** toolbar, choose the **Test** publish profile and then click **Publish Web** (the icon with arrows pointing left and right).</span></span>

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. <span data-ttu-id="70417-142">Visual Studio развертывает обновленное приложение, и автоматически откроется браузер на домашнюю страницу.</span><span class="sxs-lookup"><span data-stu-id="70417-142">Visual Studio deploys the updated application, and the browser automatically opens to the home page.</span></span>
5. <span data-ttu-id="70417-143">Запустите страницу инструкторов и выберите инструктор, чтобы убедиться, что обновление было успешно развернуто.</span><span class="sxs-lookup"><span data-stu-id="70417-143">Run the Instructors page and select an instructor to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="70417-144">Обычным также выполнить тест регрессии (то есть тестирование остальной части сайта, чтобы убедиться в том, что новое изменение не разбить существующие функциональные возможности).</span><span class="sxs-lookup"><span data-stu-id="70417-144">You would normally also do regression testing (that is, test the rest of the site to make sure that the new change didn't break any existing functionality).</span></span> <span data-ttu-id="70417-145">Однако в этом учебнике будет пропустить этот шаг и перейдите к развертывания обновления в промежуточной и производственной сред.</span><span class="sxs-lookup"><span data-stu-id="70417-145">But for this tutorial you'll skip that step and proceed to deploy the update to staging and production.</span></span>

<span data-ttu-id="70417-146">При повторном развертывании, веб-развертывания автоматически определяет, какие файлы были изменены, и копирует только измененные файлы на сервер.</span><span class="sxs-lookup"><span data-stu-id="70417-146">When you redeploy, Web Deploy automatically determines which files have changed and only copies changed files to the server.</span></span> <span data-ttu-id="70417-147">По умолчанию веб-развертывание использует даты последнего изменения файлов, чтобы определить, какие из них были изменены.</span><span class="sxs-lookup"><span data-stu-id="70417-147">By default, Web Deploy uses last-changed dates on files to determine which ones have changed.</span></span> <span data-ttu-id="70417-148">Некоторые системы управления версиями файлов даты изменить даже при изменении содержимого файла не.</span><span class="sxs-lookup"><span data-stu-id="70417-148">Some source control systems change file dates even when you don't change the file contents.</span></span> <span data-ttu-id="70417-149">В этом случае можно настроить веб-развертывания для использования контрольных сумм для определения, какие файлы были изменены.</span><span class="sxs-lookup"><span data-stu-id="70417-149">In that case, you might want to configure Web Deploy to use file checksums to determine which files have changed.</span></span> <span data-ttu-id="70417-150">Дополнительные сведения см. в разделе [почему все мои файлы получить повторного развертывания несмотря на то, что их не удалось изменить?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) в часто задаваемые вопросы по ASP.NET развертывания.</span><span class="sxs-lookup"><span data-stu-id="70417-150">For more information, see [Why do all of my files get redeployed although I didn't change them?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) in the ASP.NET Deployment FAQ.</span></span>

## <a name="take-the-application-offline-during-deployment"></a><span data-ttu-id="70417-151">Использовать приложение в автономный режим во время развертывания</span><span class="sxs-lookup"><span data-stu-id="70417-151">Take the application offline during deployment</span></span>

<span data-ttu-id="70417-152">Изменение, которое выполняется развертывание теперь является простое изменение на одной странице.</span><span class="sxs-lookup"><span data-stu-id="70417-152">The change you're deploying now is a simple change to a single page.</span></span> <span data-ttu-id="70417-153">Но иногда развертывание большего изменения или развертывании изменений кода и баз данных и сайте может работать неправильно, если пользователь запрашивает страницу до завершения развертывания.</span><span class="sxs-lookup"><span data-stu-id="70417-153">But sometimes you deploy larger changes, or you deploy both code and database changes, and the site might behave incorrectly if a user requests a page before deployment is finished.</span></span> <span data-ttu-id="70417-154">Чтобы запретить пользователям доступа к узлу, во время развертывания, можно использовать *приложения\_offline.htm* файла.</span><span class="sxs-lookup"><span data-stu-id="70417-154">To prevent users from accessing the site while deployment is in progress, you can use an *app\_offline.htm* file.</span></span> <span data-ttu-id="70417-155">При переводе в файл с именем *приложения\_offline.htm* в корневой папке приложения, службы IIS автоматически отображает этот файл вместо запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="70417-155">When you put a file named *app\_offline.htm* in the root folder of your application, IIS automatically displays that file instead of running your application.</span></span> <span data-ttu-id="70417-156">Поэтому для предотвращения доступа во время развертывания, поместить *приложения\_offline.htm* в корневой папке запуска процесса развертывания, а затем удалите *приложения\_offline.htm* после успешно развертывание.</span><span class="sxs-lookup"><span data-stu-id="70417-156">So to prevent access during deployment, you put *app\_offline.htm* in the root folder, run the deployment process, and then remove *app\_offline.htm* after successful deployment.</span></span>

<span data-ttu-id="70417-157">Можно настроить веб-развертывания для автоматического размещения по умолчанию *приложения\_offline.htm* файла на сервере при запуске развертывание и удалите его при завершении.</span><span class="sxs-lookup"><span data-stu-id="70417-157">You can configure Web Deploy to automatically put a default *app\_offline.htm* file on the server when it starts deploying and remove it when it finishes.</span></span> <span data-ttu-id="70417-158">Делать все, что нужно сделать — это добавить следующий XML-элемент в публикации (.pubxml) профиля:</span><span class="sxs-lookup"><span data-stu-id="70417-158">To do that all you have to do is add the following XML element to your publish profile (.pubxml) file:</span></span>

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

<span data-ttu-id="70417-159">В этом учебнике будет показано, как создавать и использовать пользовательский *приложения\_offline.htm* файла.</span><span class="sxs-lookup"><span data-stu-id="70417-159">For this tutorial you'll see how to create and use a custom *app\_offline.htm* file.</span></span>

<span data-ttu-id="70417-160">С помощью *приложения\_offline.htm* в промежуточный сайт не требуется, так как нет пользователей, обращающихся к промежуточный сайт.</span><span class="sxs-lookup"><span data-stu-id="70417-160">Using *app\_offline.htm* in the staging site isn't required, because you don't have users accessing the staging site.</span></span> <span data-ttu-id="70417-161">Однако рекомендуется использовать промежуточного хранения для все проверки того, который необходимо выполнить развертывание в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="70417-161">But it's a good practice to use staging to test everything the way you plan to deploy in production.</span></span>

### <a name="create-appofflinehtm"></a><span data-ttu-id="70417-162">Создание приложения\_offline.htm</span><span class="sxs-lookup"><span data-stu-id="70417-162">Create app\_offline.htm</span></span>

1. <span data-ttu-id="70417-163">В **обозревателе решений**, щелкните правой кнопкой мыши решение и выберите **добавить**, а затем нажмите кнопку **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="70417-163">In **Solution Explorer**, right-click the solution and click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="70417-164">Создание **HTML-страницу** с именем *приложения\_offline.htm* (удаление конечного символа «l» в *.html* расширения, Visual Studio по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="70417-164">Create an **HTML Page** named *app\_offline.htm* (delete the final "l" in the *.html* extension that Visual Studio creates by default).</span></span>
3. <span data-ttu-id="70417-165">Замените существующую разметку шаблона следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="70417-165">Replace the template markup with the following markup:</span></span>

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. <span data-ttu-id="70417-166">Сохраните и закройте файл.</span><span class="sxs-lookup"><span data-stu-id="70417-166">Save and close the file.</span></span>

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a><span data-ttu-id="70417-167">Копия приложения\_offline.htm в корневую папку веб-сайта</span><span class="sxs-lookup"><span data-stu-id="70417-167">Copy app\_offline.htm to the root folder of the web site</span></span>

<span data-ttu-id="70417-168">Можно использовать любое средство FTP для копирования файлов на веб-сайт.</span><span class="sxs-lookup"><span data-stu-id="70417-168">You can use any FTP tool to copy files to the web site.</span></span> <span data-ttu-id="70417-169">[FileZilla](http://filezilla-project.org/) популярным средством FTP и отображается в снимках экрана.</span><span class="sxs-lookup"><span data-stu-id="70417-169">[FileZilla](http://filezilla-project.org/) is a popular FTP tool and is shown in the screen shots.</span></span>

<span data-ttu-id="70417-170">Чтобы использовать средство FTP, необходимы три вещи: URL-адрес FTP, имя пользователя и пароль.</span><span class="sxs-lookup"><span data-stu-id="70417-170">To use an FTP tool, you need three things: the FTP URL, the user name, and the password.</span></span>

<span data-ttu-id="70417-171">URL-адрес отображается на странице панели мониторинга веб-сайта на портале управления Azure, и имя пользователя и пароль для FTP-сервера можно найти в *.publishsettings* ранее загруженный файл.</span><span class="sxs-lookup"><span data-stu-id="70417-171">The URL is shown on the web site's dashboard page in the Azure Management Portal, and the user name and password for FTP can be found in the *.publishsettings* file that you downloaded earlier.</span></span> <span data-ttu-id="70417-172">Ниже показано, как получить эти значения.</span><span class="sxs-lookup"><span data-stu-id="70417-172">The following steps show how to get these values.</span></span>

1. <span data-ttu-id="70417-173">На портале управления Azure щелкните **веб-сайтов** и нажмите кнопку промежуточного веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="70417-173">In the Azure Management Portal, click **Web Sites** tab and then click the staging web site.</span></span>
2. <span data-ttu-id="70417-174">На **мониторинга** страницы, прокрутите вниз, чтобы найти имя узла FTP в **быстрый обзор** раздела.</span><span class="sxs-lookup"><span data-stu-id="70417-174">On the **Dashboard** page, scroll down to find the FTP host name in the **Quick Glance** section.</span></span>

    ![Имя узла FTP](deploying-a-code-update/_static/image5.png)
3. <span data-ttu-id="70417-176">Откройте промежуточной *.publishsettings* файл в блокноте или другом текстовом редакторе.</span><span class="sxs-lookup"><span data-stu-id="70417-176">Open the staging *.publishsettings* file in Notepad or another text editor.</span></span>
4. <span data-ttu-id="70417-177">Найти `publishProfile` элемент для профиль FTP.</span><span class="sxs-lookup"><span data-stu-id="70417-177">Find the `publishProfile` element for the FTP profile.</span></span>
5. <span data-ttu-id="70417-178">Копировать `userName` и `userPWD` значения.</span><span class="sxs-lookup"><span data-stu-id="70417-178">Copy the `userName` and `userPWD` values.</span></span>

    ![Учетные данные FTP](deploying-a-code-update/_static/image6.png)
6. <span data-ttu-id="70417-180">Открытие средства FTP и выполните вход на URL-адрес FTP.</span><span class="sxs-lookup"><span data-stu-id="70417-180">Open your FTP tool and log on to the FTP URL.</span></span>
7. <span data-ttu-id="70417-181">Копировать *приложения\_offline.htm* из папки решения для */сайт/wwwroot* папку промежуточного сайта.</span><span class="sxs-lookup"><span data-stu-id="70417-181">Copy *app\_offline.htm* from the solution folder to the */site/wwwroot* folder in the staging site.</span></span>

    ![Скопируйте app_offline](deploying-a-code-update/_static/image7.png)
8. <span data-ttu-id="70417-183">Перейдите по URL-АДРЕСУ промежуточный сайт.</span><span class="sxs-lookup"><span data-stu-id="70417-183">Browse to your staging site's URL.</span></span> <span data-ttu-id="70417-184">Вы увидите, что *приложения\_offline.htm* страницы будет отображаться вместо домашней страницы.</span><span class="sxs-lookup"><span data-stu-id="70417-184">You see that the *app\_offline.htm* page is now displayed instead of your home page.</span></span>

    ![App_offline.htm в окне браузера](deploying-a-code-update/_static/image8.png)

<span data-ttu-id="70417-186">Теперь вы готовы к развертыванию для промежуточного хранения.</span><span class="sxs-lookup"><span data-stu-id="70417-186">You are now ready to deploy to staging.</span></span>

## <a name="deploy-the-code-update-to-staging-and-production"></a><span data-ttu-id="70417-187">Развернуть обновление кода для промежуточной и производственной сред</span><span class="sxs-lookup"><span data-stu-id="70417-187">Deploy the code update to staging and production</span></span>

1. <span data-ttu-id="70417-188">В **веб-публикация одним щелкните** инструментов, выберите **промежуточной** профиль публикации, а затем нажмите кнопку **веб-публикация**.</span><span class="sxs-lookup"><span data-stu-id="70417-188">In the **Web One Click Publish** toolbar, choose the **Staging** publish profile and then click **Publish Web**.</span></span>

    <span data-ttu-id="70417-189">Visual Studio развертывает обновленное приложение и открывает в браузере домашнюю страницу.</span><span class="sxs-lookup"><span data-stu-id="70417-189">Visual Studio deploys the updated application and opens the browser to the site's home page.</span></span> <span data-ttu-id="70417-190">*Приложения\_offline.htm* отображается файл.</span><span class="sxs-lookup"><span data-stu-id="70417-190">The *app\_offline.htm* file is displayed.</span></span> <span data-ttu-id="70417-191">Перед началом тестирования для проверки успешного развертывания, необходимо удалить *приложения\_offline.htm* файла.</span><span class="sxs-lookup"><span data-stu-id="70417-191">Before you can test to verify successful deployment, you must remove the *app\_offline.htm* file.</span></span>
2. <span data-ttu-id="70417-192">Вернитесь в средстве FTP и удалите **приложения\_offline.htm** из промежуточный сайт.</span><span class="sxs-lookup"><span data-stu-id="70417-192">Return to your FTP tool, and delete **app\_offline.htm** from the staging site.</span></span>
3. <span data-ttu-id="70417-193">В браузере откройте страницу инструкторов в промежуточный сайт и выберите инструктор, чтобы убедиться, что обновление было успешно развернуто.</span><span class="sxs-lookup"><span data-stu-id="70417-193">In the browser, open the Instructors page in the staging site, and select an instructor to verify that the update was successfully deployed.</span></span>
4. <span data-ttu-id="70417-194">Выполните ту же процедуру для рабочей среды, как и для промежуточного хранения.</span><span class="sxs-lookup"><span data-stu-id="70417-194">Follow the same procedure for production as you did for staging.</span></span>

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a><span data-ttu-id="70417-195">Просмотр изменений и развертывании определенные файлы</span><span class="sxs-lookup"><span data-stu-id="70417-195">Reviewing Changes and Deploying Specific Files</span></span>

<span data-ttu-id="70417-196">Visual Studio 2012 также предоставляет возможность развертывания отдельных файлов.</span><span class="sxs-lookup"><span data-stu-id="70417-196">Visual Studio 2012 also gives you the ability to deploy individual files.</span></span> <span data-ttu-id="70417-197">Для выбранного файла можно просмотреть различия между локальной версией и развернутой версии, развертывание файла в целевой среде или скопировать файл с целевой среде для локального проекта.</span><span class="sxs-lookup"><span data-stu-id="70417-197">For a selected file you can view differences between the local version and the deployed version, deploy the file to the destination environment, or copy the file from the destination environment to the local project.</span></span> <span data-ttu-id="70417-198">В этом разделе учебника вы научитесь использовать эти функции.</span><span class="sxs-lookup"><span data-stu-id="70417-198">In this section of the tutorial you see how to use these features.</span></span>

### <a name="make-a-change-to-deploy"></a><span data-ttu-id="70417-199">Изменения для развертывания</span><span class="sxs-lookup"><span data-stu-id="70417-199">Make a change to deploy</span></span>

1. <span data-ttu-id="70417-200">Откройте *Content/Site.css*и найти блок для `body` тег.</span><span class="sxs-lookup"><span data-stu-id="70417-200">Open *Content/Site.css*, and find the block for the `body` tag.</span></span>
2. <span data-ttu-id="70417-201">Измените значение `background-color` из `#fff` для `darkblue`.</span><span class="sxs-lookup"><span data-stu-id="70417-201">Change the value for `background-color` from `#fff` to `darkblue`.</span></span>

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a><span data-ttu-id="70417-202">Просмотр изменений в окне Просмотр публикации</span><span class="sxs-lookup"><span data-stu-id="70417-202">View the change in the Publish Preview window</span></span>

<span data-ttu-id="70417-203">При использовании **веб-публикация** мастера для публикации проекта, вы увидите, что изменения будут опубликованы, дважды щелкнув файл в **предварительного просмотра** окна.</span><span class="sxs-lookup"><span data-stu-id="70417-203">When you use the **Publish Web** wizard to publish the project, you can see what changes are going to be published by double-clicking the file in the **Preview** window.</span></span>

1. <span data-ttu-id="70417-204">Щелкните правой кнопкой мыши проект ContosoUniversity и нажмите кнопку **публикации**.</span><span class="sxs-lookup"><span data-stu-id="70417-204">Right-click the ContosoUniversity project and click **Publish**.</span></span>
2. <span data-ttu-id="70417-205">Из **профиль** раскрывающемся списке выберите **тест** профиль публикации.</span><span class="sxs-lookup"><span data-stu-id="70417-205">From the **Profile** drop-down list, select the **Test** publish profile.</span></span>
3. <span data-ttu-id="70417-206">Нажмите кнопку **предварительного просмотра**, а затем нажмите кнопку **начать просмотр**.</span><span class="sxs-lookup"><span data-stu-id="70417-206">Click **Preview**, and then click **Start Preview**.</span></span>
4. <span data-ttu-id="70417-207">В **предварительного просмотра** панели, дважды щелкните **Site.css**.</span><span class="sxs-lookup"><span data-stu-id="70417-207">In the **Preview** pane, double-click **Site.css**.</span></span>

    ![Дважды щелкните файл Site.css](deploying-a-code-update/_static/image9.png)

    <span data-ttu-id="70417-209">**Предварительного просмотра изменений** диалогового окна отображается предварительный просмотр изменений, которые будут развернуты.</span><span class="sxs-lookup"><span data-stu-id="70417-209">The **Preview changes** dialog shows a preview of the changes that will be deployed.</span></span>

    ![Просмотр изменений в Site.css](deploying-a-code-update/_static/image10.png)

    <span data-ttu-id="70417-211">Если дважды щелкнуть *Web.config* файла, **предварительного просмотра изменений** диалогового окна показан результат сборки конфигурации преобразования и преобразования профиля публикации.</span><span class="sxs-lookup"><span data-stu-id="70417-211">If you double-click the *Web.config* file, the **Preview changes** dialog shows the effect of your build configuration transformations and publish profile transformations.</span></span> <span data-ttu-id="70417-212">На этом этапе вы еще не все, что приведет к *Web.config* файла на сервере, чтобы изменить, таким образом, чтобы ожидать без изменений.</span><span class="sxs-lookup"><span data-stu-id="70417-212">At this point you have not done anything that would cause the *Web.config* file on the server to change, so you expect to see no changes.</span></span> <span data-ttu-id="70417-213">Тем не менее **предварительного просмотра изменений** окне неправильно отображаются два изменения.</span><span class="sxs-lookup"><span data-stu-id="70417-213">However, the **Preview changes** window incorrectly shows two changes.</span></span> <span data-ttu-id="70417-214">Похоже, два XML-элементы будут удалены.</span><span class="sxs-lookup"><span data-stu-id="70417-214">It looks like two XML elements will be removed.</span></span> <span data-ttu-id="70417-215">Эти элементы добавляются в процессе публикации, при выборе **выполнять миграции Code First при запуске приложения** Code First класса контекста.</span><span class="sxs-lookup"><span data-stu-id="70417-215">These elements are added by the publish process when you select **Execute Code First Migrations on application start** for a Code First context class.</span></span> <span data-ttu-id="70417-216">Сравнение выполняется перед добавляет в процессе публикации этих элементов, так выглядит так, будто они удаляются несмотря на то, что они не будут удалены.</span><span class="sxs-lookup"><span data-stu-id="70417-216">The comparison is done before the publish process adds those elements, so it looks like they are being removed although they will not be removed.</span></span> <span data-ttu-id="70417-217">Эта ошибка будет исправлена в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="70417-217">This error will be corrected in a future release.</span></span>
5. <span data-ttu-id="70417-218">Нажмите кнопку **Закрыть**.</span><span class="sxs-lookup"><span data-stu-id="70417-218">Click **Close**.</span></span>
6. <span data-ttu-id="70417-219">Нажмите кнопку **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="70417-219">Click **Publish**.</span></span>
7. <span data-ttu-id="70417-220">При открытии обозревателя на домашнюю страницу сайта теста, нажмите клавиши CTRL + F5, чтобы приведут к жестких обновлению, чтобы увидеть эффект от изменения CSS.</span><span class="sxs-lookup"><span data-stu-id="70417-220">When the browser opens to the home page of the Test site, press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Эффект изменения CSS](deploying-a-code-update/_static/image11.png)
8. <span data-ttu-id="70417-222">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="70417-222">Close the browser.</span></span>

### <a name="publish-specific-files-from-solution-explorer"></a><span data-ttu-id="70417-223">Опубликовать определенные файлы из обозревателя решений</span><span class="sxs-lookup"><span data-stu-id="70417-223">Publish specific files from Solution Explorer</span></span>

<span data-ttu-id="70417-224">Предположим, что вы не синим фоном, например чтобы восстановить исходное значение цвета.</span><span class="sxs-lookup"><span data-stu-id="70417-224">Suppose you don't like the blue background and want to revert to the original color.</span></span> <span data-ttu-id="70417-225">В этом разделе необходимо восстановить исходные настройки путем публикации определенного файла непосредственно из **обозревателе решений**.</span><span class="sxs-lookup"><span data-stu-id="70417-225">In this section you'll restore the original settings by publishing a specific file directly from **Solution Explorer**.</span></span>

1. <span data-ttu-id="70417-226">Откройте *Content/Site.css* и восстановление `background-color` параметру `#fff`.</span><span class="sxs-lookup"><span data-stu-id="70417-226">Open *Content/Site.css* and restore the `background-color` setting to `#fff`.</span></span>
2. <span data-ttu-id="70417-227">В **обозревателе решений**, щелкните правой кнопкой мыши *Content/Site.css* файла.</span><span class="sxs-lookup"><span data-stu-id="70417-227">In **Solution Explorer**, right-click the *Content/Site.css* file.</span></span>

    <span data-ttu-id="70417-228">В контекстном меню показывает три параметров публикации.</span><span class="sxs-lookup"><span data-stu-id="70417-228">The context menu shows three publish options.</span></span>

    ![Публиковать параметры из обозревателя решений](deploying-a-code-update/_static/image12.png)
3. <span data-ttu-id="70417-230">Нажмите кнопку **Просмотр изменений в Site.css**.</span><span class="sxs-lookup"><span data-stu-id="70417-230">Click **Preview changes to Site.css**.</span></span>

    <span data-ttu-id="70417-231">Откроется окно, показывающих различия между локального файла и его версии в целевой среде.</span><span class="sxs-lookup"><span data-stu-id="70417-231">A window opens to show the differences between the local file and the version of it in the destination environment.</span></span>

    ![Diff-Content/Site.css](deploying-a-code-update/_static/image13.png)
4. <span data-ttu-id="70417-233">В **обозревателе решений**, щелкните правой кнопкой мыши **Site.css** еще раз и нажмите кнопку **публикации Site.css**.</span><span class="sxs-lookup"><span data-stu-id="70417-233">In **Solution Explorer**, right-click **Site.css** again and click **Publish Site.css**.</span></span>

    <span data-ttu-id="70417-234">**Действия публикации Web** в окне отображаются опубликованные файл.</span><span class="sxs-lookup"><span data-stu-id="70417-234">The **Web Publish Activity** window shows that the file has been published.</span></span>

    ![Действие публикации окне](deploying-a-code-update/_static/image14.png)
5. <span data-ttu-id="70417-236">Откройте в браузере `http://localhost/contosouniversity` URL-адрес и нажмите клавишу CTRL + F5, чтобы вызвать жесткой обновления, чтобы выявить эффект от CSS изменения.</span><span class="sxs-lookup"><span data-stu-id="70417-236">Open a browser to the `http://localhost/contosouniversity` URL, and then press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Домашняя страница с обычной CSS](deploying-a-code-update/_static/image15.png)
6. <span data-ttu-id="70417-238">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="70417-238">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="70417-239">Сводка</span><span class="sxs-lookup"><span data-stu-id="70417-239">Summary</span></span>

<span data-ttu-id="70417-240">Теперь вы увидели несколько способов развертывания обновления приложения, не связанных с изменениями в базе данных, и вы знаете, как для предварительного просмотра изменения, убедитесь, что будут обновлены ожидаемых.</span><span class="sxs-lookup"><span data-stu-id="70417-240">You've now seen several ways to deploy an application update that does not involve a database change, and you've seen how to preview the changes to verify that what will be updated is what you expect.</span></span> <span data-ttu-id="70417-241">Страница инструкторов теперь имеет **курсов обучения** раздела.</span><span class="sxs-lookup"><span data-stu-id="70417-241">The Instructors page now has a **Courses Taught** section.</span></span>

![Страница инструкторов с курсов обучения](deploying-a-code-update/_static/image16.png)

<span data-ttu-id="70417-243">Далее учебника показано, как развернуть изменение базы данных: для базы данных и странице инструкторов добавите поля Дата рождения.</span><span class="sxs-lookup"><span data-stu-id="70417-243">The next tutorial shows you how to deploy a database change: you'll add a birthdate field to the database and to the Instructors page.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="70417-244">[Назад](deploying-to-production.md)
[Вперед](deploying-a-database-update.md)</span><span class="sxs-lookup"><span data-stu-id="70417-244">[Previous](deploying-to-production.md)
[Next](deploying-a-database-update.md)</span></span>
