---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'Веб-развертывание ASP.NET с помощью Visual Studio: развертывание из командной строки | Документация Майкрософт'
author: tdykstra
description: В этой серии руководств показано, как развернуть ASP.NET (публикации) веб-приложения, веб-приложениях службы приложений Azure или у стороннего поставщика размещения, Пол...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: f079beabccd253ff13c19d3192181ddbdf00b3bf
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830971"
---
<a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a><span data-ttu-id="2923d-103">Веб-развертывание ASP.NET с помощью Visual Studio: развертывание из командной строки</span><span class="sxs-lookup"><span data-stu-id="2923d-103">ASP.NET Web Deployment using Visual Studio: Command Line Deployment</span></span>
====================
<span data-ttu-id="2923d-104">по [том Дайкстра](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="2923d-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="2923d-105">Загрузите начальный проект</span><span class="sxs-lookup"><span data-stu-id="2923d-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="2923d-106">В этой серии руководств показано, как развернуть ASP.NET (публикации) веб-приложения для веб-приложениях службы приложений Azure или стороннего поставщика услуг размещения, с помощью Visual Studio 2012 или Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="2923d-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="2923d-107">Сведения об этой серии см. в разделе [в первом учебнике серии](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2923d-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="2923d-108">Обзор</span><span class="sxs-lookup"><span data-stu-id="2923d-108">Overview</span></span>

<span data-ttu-id="2923d-109">Этом руководстве показано, как вызвать веб-Visual Studio опубликовать конвейера из командной строки.</span><span class="sxs-lookup"><span data-stu-id="2923d-109">This tutorial shows you how to invoke the Visual Studio web publish pipeline from the command line.</span></span> <span data-ttu-id="2923d-110">Это полезно для сценариев, где вы хотите [автоматизации процесса развертывания](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) вместо делать это вручную в Visual Studio, обычно с помощью [источника системы управления версиями кода](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span><span class="sxs-lookup"><span data-stu-id="2923d-110">This is useful for scenarios where you want to [automate the deployment process](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) instead of doing it manually in Visual Studio, typically by using a [source code version control system](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span></span>

## <a name="make-a-change-to-deploy"></a><span data-ttu-id="2923d-111">Изменения для развертывания</span><span class="sxs-lookup"><span data-stu-id="2923d-111">Make a change to deploy</span></span>

<span data-ttu-id="2923d-112">В настоящее время на страницу About код шаблона.</span><span class="sxs-lookup"><span data-stu-id="2923d-112">Currently the About page displays the template code.</span></span>

![Страница с кодом шаблона](command-line-deployment/_static/image1.png)

<span data-ttu-id="2923d-114">Вы замените, код, который отображает сводку систему зачисления учащихся.</span><span class="sxs-lookup"><span data-stu-id="2923d-114">You'll replace that with code that displays a summary of student enrollment.</span></span>

<span data-ttu-id="2923d-115">Откройте *About.aspx* странице, удалить все это разметка внутри `MainContent` `Content` элемент и вставьте следующую разметку, вместо него:</span><span class="sxs-lookup"><span data-stu-id="2923d-115">Open the *About.aspx* page, delete all of the markup inside the `MainContent` `Content` element, and insert the following markup in its place:</span></span>

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

<span data-ttu-id="2923d-116">Запустите проект и выберите **о** страницы.</span><span class="sxs-lookup"><span data-stu-id="2923d-116">Run the project and select the **About** page.</span></span>

![Страница About](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a><span data-ttu-id="2923d-118">Развертывание для тестирования с помощью командной строки</span><span class="sxs-lookup"><span data-stu-id="2923d-118">Deploy to Test by using the command line</span></span>

<span data-ttu-id="2923d-119">Вы не развертывать еще одно изменение базы данных, так отключить развертывание базы данных dbDacFx для базы данных aspnet ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="2923d-119">You won't be deploying another database change, so disable dbDacFx database deployment for the aspnet-ContosoUniversity database.</span></span> <span data-ttu-id="2923d-120">Откройте **веб-публикации** мастера и в каждом из трех профилей, снимите флажок публикации **обновления базы данных** флажок на **параметры** вкладку.</span><span class="sxs-lookup"><span data-stu-id="2923d-120">Open the **Publish Web** wizard, and in each of the three publish profiles, clear the **Update Database** check box on the **Settings** tab.</span></span>

<span data-ttu-id="2923d-121">В Windows 8 начальная страница, поиск **Командная строка разработчика для VS2012**.</span><span class="sxs-lookup"><span data-stu-id="2923d-121">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**.</span></span>

<span data-ttu-id="2923d-122">Щелкните правой кнопкой мыши значок **Командная строка разработчика для VS2012** и нажмите кнопку **Запуск от имени администратора**.</span><span class="sxs-lookup"><span data-stu-id="2923d-122">Right-click the icon for **Developer Command Prompt for VS2012** and click **Run as administrator**.</span></span>

<span data-ttu-id="2923d-123">Введите следующую команду в командной строке, заменив путь к файлу решения и путь к файлу решения:</span><span class="sxs-lookup"><span data-stu-id="2923d-123">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file:</span></span>

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

<span data-ttu-id="2923d-124">MSBuild выполняет сборку решения и его развертывание в тестовую среду.</span><span class="sxs-lookup"><span data-stu-id="2923d-124">MSBuild builds the solution and deploys it to the test environment.</span></span>

![Данные, выводимые в командной строке](command-line-deployment/_static/image3.png)

<span data-ttu-id="2923d-126">Откройте браузер и перейдите к `http://localhost/ContosoUniversity`, затем нажмите кнопку **о** страницу, чтобы убедиться, что развертывание выполнено успешно.</span><span class="sxs-lookup"><span data-stu-id="2923d-126">Open a browser and go to `http://localhost/ContosoUniversity`, then click the **About** page to verify that the deployment was successful.</span></span>

<span data-ttu-id="2923d-127">Если вы еще не создали учащихся в тесте, вы увидите пустую страницу в разделе **статистики учащихся текст** заголовок.</span><span class="sxs-lookup"><span data-stu-id="2923d-127">If you haven't created any students in test, you'll see an empty page under the **Student Body Statistics** heading.</span></span> <span data-ttu-id="2923d-128">Перейдите к **учащихся** щелкните **добавить учащихся**и добавить некоторые студенты, а затем вернитесь к **о** страницу для просмотра статистики учащихся.</span><span class="sxs-lookup"><span data-stu-id="2923d-128">Go to the **Students** page, click **Add Student**, and add some students, and then return to the **About** page to see student statistics.</span></span>

![О странице в тестовой среде](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a><span data-ttu-id="2923d-130">Параметры ключа командной строки</span><span class="sxs-lookup"><span data-stu-id="2923d-130">Key command line options</span></span>

<span data-ttu-id="2923d-131">Команда, введенный путь к файлу решения и два свойства, передаваемые в MSBuild:</span><span class="sxs-lookup"><span data-stu-id="2923d-131">The command that you entered passed the solution file path and two properties to MSBuild:</span></span>

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a><span data-ttu-id="2923d-132">Развертывание решения и развертывание отдельных проектов</span><span class="sxs-lookup"><span data-stu-id="2923d-132">Deploying the solution versus deploying individual projects</span></span>

<span data-ttu-id="2923d-133">Задание файла решения приводит все проекты в решении для построения.</span><span class="sxs-lookup"><span data-stu-id="2923d-133">Specifying the solution file causes all projects in the solution to be built.</span></span> <span data-ttu-id="2923d-134">Если у вас есть несколько веб-проектов в решении, применяются следующие правила MSBuild:</span><span class="sxs-lookup"><span data-stu-id="2923d-134">If you have multiple web projects in the solution, the following MSBuild behavior applies:</span></span>

- <span data-ttu-id="2923d-135">Свойства, указываемые в командной строке, передаются в каждый проект.</span><span class="sxs-lookup"><span data-stu-id="2923d-135">The properties that you specify on the command line are passed to every project.</span></span> <span data-ttu-id="2923d-136">Следовательно каждый веб-проекта необходим профиль публикации с вами именем.</span><span class="sxs-lookup"><span data-stu-id="2923d-136">Therefore, each web project must have a publish profile with the name that you specify.</span></span> <span data-ttu-id="2923d-137">Если указать `/p:PublishProfile=Test`, каждого веб-проекта должен быть профиль публикации с именем *теста*.</span><span class="sxs-lookup"><span data-stu-id="2923d-137">If you specify `/p:PublishProfile=Test`, each web project must have a publish profile named *Test*.</span></span>
- <span data-ttu-id="2923d-138">Один проект может публикацию успешно в том случае, если другой даже не выполняет сборку.</span><span class="sxs-lookup"><span data-stu-id="2923d-138">You might successfully publish one project when another one doesn't even build.</span></span> <span data-ttu-id="2923d-139">Дополнительные сведения см. в разделе обсуждений stackoverflow [MSBuild завершается сбоем с два пакета](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span><span class="sxs-lookup"><span data-stu-id="2923d-139">For more information, see the stackoverflow thread [MSBuild fails with two packages](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span></span>

<span data-ttu-id="2923d-140">При выборе отдельного проекта вместо решения, необходимо добавить параметр, указывающий, данная версия Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2923d-140">If you specify an individual project instead of a solution, you have to add a parameter that specifies the Visual Studio version.</span></span> <span data-ttu-id="2923d-141">Если вы используете Visual Studio 2012 командной строки будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="2923d-141">If you are using Visual Studio 2012 the command line would be similar to the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

<span data-ttu-id="2923d-142">Номер версии для Visual Studio 2010 — 10,0.</span><span class="sxs-lookup"><span data-stu-id="2923d-142">The version number for Visual Studio 2010 is 10.0.</span></span> <span data-ttu-id="2923d-143">Дополнительные сведения см. в разделе [совместимость проекта Visual Studio и VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) в блоге Саид Хашими.</span><span class="sxs-lookup"><span data-stu-id="2923d-143">For more information, see [Visual Studio project compatability and VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) on Sayed Hashimi's blog.</span></span>

### <a name="specifying-the-publish-profile"></a><span data-ttu-id="2923d-144">Указание профиля публикации</span><span class="sxs-lookup"><span data-stu-id="2923d-144">Specifying the publish profile</span></span>

<span data-ttu-id="2923d-145">Профиль публикации можно указать по имени или полный путь к *.pubxml* файл, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="2923d-145">You can specify the publish profile by name or by the full path to the *.pubxml* file, as shown in the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a><span data-ttu-id="2923d-146">Веб-публикация методов, поддерживаемых для публикации командной строки</span><span class="sxs-lookup"><span data-stu-id="2923d-146">Web publish methods supported for command-line publishing</span></span>

<span data-ttu-id="2923d-147">Три публикации методы поддерживаются для публикация из командной строки:</span><span class="sxs-lookup"><span data-stu-id="2923d-147">Three publish methods are supported for command line publishing:</span></span>

- <span data-ttu-id="2923d-148">`MSDeploy` -Публикация с помощью веб-развертывания.</span><span class="sxs-lookup"><span data-stu-id="2923d-148">`MSDeploy` - Publish by using Web Deploy.</span></span>
- <span data-ttu-id="2923d-149">`Package` -Опубликуйте, создав веб-пакета развертывания.</span><span class="sxs-lookup"><span data-stu-id="2923d-149">`Package` - Publish by creating a Web Deploy Package.</span></span> <span data-ttu-id="2923d-150">Необходимо отдельно установить пакет с помощью команды MSBuild, который его создал.</span><span class="sxs-lookup"><span data-stu-id="2923d-150">You have to install the package separately from the MSBuild command that creates it.</span></span>
- <span data-ttu-id="2923d-151">`FileSystem` -Публикации путем копирования файлов в указанную папку.</span><span class="sxs-lookup"><span data-stu-id="2923d-151">`FileSystem` - Publish by copying files to a specified folder.</span></span>

### <a name="specifying-the-build-configuration-and-platform"></a><span data-ttu-id="2923d-152">Указание конфигурации сборки и платформы</span><span class="sxs-lookup"><span data-stu-id="2923d-152">Specifying the build configuration and platform</span></span>

<span data-ttu-id="2923d-153">Необходимо задать значение конфигурации сборки и платформы, в Visual Studio или в командной строке.</span><span class="sxs-lookup"><span data-stu-id="2923d-153">The build configuration and platform must be set in Visual Studio or on the command line.</span></span> <span data-ttu-id="2923d-154">Профили публикации включают свойства, которые называются `LastUsedBuildConfiguration` и `LastUsedPlatform`, но эти свойства невозможно установить, чтобы определить, как проект будет собран.</span><span class="sxs-lookup"><span data-stu-id="2923d-154">The publish profiles include properties that are named `LastUsedBuildConfiguration` and `LastUsedPlatform`, but you can't set these properties in order to determine how the project is built.</span></span> <span data-ttu-id="2923d-155">Дополнительные сведения см. в разделе [MSBuild: Настройка свойства конфигурации](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) в блоге Саид Хашими.</span><span class="sxs-lookup"><span data-stu-id="2923d-155">For more information, see [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) on Sayed Hashimi's blog.</span></span>

## <a name="deploy-to-staging"></a><span data-ttu-id="2923d-156">Развертывание в промежуточное хранилище</span><span class="sxs-lookup"><span data-stu-id="2923d-156">Deploy to staging</span></span>

<span data-ttu-id="2923d-157">Для развертывания в Azure, необходимо добавить пароль в командную строку.</span><span class="sxs-lookup"><span data-stu-id="2923d-157">To deploy to Azure, you must add the password to the command line.</span></span> <span data-ttu-id="2923d-158">Если пароль был сохранен в профиле публикации в Visual Studio, он был сохранен в зашифрованном виде в вашей *. pubxml.user* файл.</span><span class="sxs-lookup"><span data-stu-id="2923d-158">If you saved the password in the publish profile in Visual Studio, it was stored in encrypted form in the your *.pubxml.user* file.</span></span> <span data-ttu-id="2923d-159">Этот файл не осуществляется MSBuild при выполнении развертывания командной строки, поэтому вам нужно передать пароль в параметре командной строки.</span><span class="sxs-lookup"><span data-stu-id="2923d-159">That file is not accessed by MSBuild when you do a command line deployment, so you have to pass in the password in a command line parameter.</span></span>

1. <span data-ttu-id="2923d-160">Скопируйте пароль, вам понадобятся из *.publishsettings* файл, который вы скачали ранее для промежуточного веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="2923d-160">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the staging web site.</span></span> <span data-ttu-id="2923d-161">Пароль — это значение `userPWD` атрибут для веб-развертывания `publishProfile` элемент.</span><span class="sxs-lookup"><span data-stu-id="2923d-161">The password is the value of the `userPWD` attribute for the Web Deploy `publishProfile` element.</span></span>

    ![Веб-развертывание пароль](command-line-deployment/_static/image5.png)
2. <span data-ttu-id="2923d-163">В Windows 8 начальная страница, поиск **Командная строка разработчика для VS2012**и щелкните значок, чтобы открыть окно командной строки.</span><span class="sxs-lookup"><span data-stu-id="2923d-163">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**, and click the icon to open the command prompt.</span></span> <span data-ttu-id="2923d-164">(Не нужно открыть его от имени администратора это время, так как не развертывании в службах IIS на локальном компьютере.)</span><span class="sxs-lookup"><span data-stu-id="2923d-164">(You don't have to open it as administrator this time because you aren't deploying to IIS on the local computer.)</span></span>
3. <span data-ttu-id="2923d-165">Введите следующую команду в командной строке, заменив путь к файлу решения и путь к файлу решения и пароль с помощью пароля:</span><span class="sxs-lookup"><span data-stu-id="2923d-165">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    <span data-ttu-id="2923d-166">Обратите внимание на то, что эта команда включает дополнительный параметр: `/p:AllowUntrustedCertificate=true`.</span><span class="sxs-lookup"><span data-stu-id="2923d-166">Notice that this command line includes an extra parameter: `/p:AllowUntrustedCertificate=true`.</span></span> <span data-ttu-id="2923d-167">Так как этот учебник написан, `AllowUntrustedCertificate` свойство должно иметь значение, при публикации в Azure из командной строки.</span><span class="sxs-lookup"><span data-stu-id="2923d-167">As this tutorial is being written, the `AllowUntrustedCertificate` property must be set when you publish to Azure from the command line.</span></span> <span data-ttu-id="2923d-168">Когда будет выпущена для исправления этой ошибки, не нужно этого параметра.</span><span class="sxs-lookup"><span data-stu-id="2923d-168">When the fix for this bug is released, you won't need that parameter.</span></span>
4. <span data-ttu-id="2923d-169">Откройте браузер и перейдите к URL-адрес промежуточный сайт и нажмите кнопку **о** страницу, чтобы убедиться, что развертывание выполнено успешно.</span><span class="sxs-lookup"><span data-stu-id="2923d-169">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

    <span data-ttu-id="2923d-170">Как было показано ранее для тестовой среды, может потребоваться создать некоторые студенты для просмотра статистики на **о** страницы.</span><span class="sxs-lookup"><span data-stu-id="2923d-170">As you saw earlier for the test environment, you might have to create some students to see statistics on the **About** page.</span></span>

## <a name="deploy-to-production"></a><span data-ttu-id="2923d-171">Развертывание в рабочей среде</span><span class="sxs-lookup"><span data-stu-id="2923d-171">Deploy to production</span></span>

<span data-ttu-id="2923d-172">Процесс развертывания в рабочей среде похож на процесс для промежуточного хранения.</span><span class="sxs-lookup"><span data-stu-id="2923d-172">The process for deploying to production is similar to the process for staging.</span></span>

1. <span data-ttu-id="2923d-173">Скопируйте пароль, вам понадобятся из *.publishsettings* файл, загруженный ранее для рабочих веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="2923d-173">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the production web site.</span></span>
2. <span data-ttu-id="2923d-174">Откройте **Командная строка разработчика для VS2012**.</span><span class="sxs-lookup"><span data-stu-id="2923d-174">Open **Developer Command Prompt for VS2012**.</span></span>
3. <span data-ttu-id="2923d-175">Введите следующую команду в командной строке, заменив путь к файлу решения и путь к файлу решения и пароль с помощью пароля:</span><span class="sxs-lookup"><span data-stu-id="2923d-175">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    <span data-ttu-id="2923d-176">На реальном рабочем узле, если было также изменение базы данных, обычно копируются *приложения\_offline.htm* файл на сайт перед развертыванием и удалить его после успешного развертывания.</span><span class="sxs-lookup"><span data-stu-id="2923d-176">For a real production site, if there was also a database change, you would typically copy the *app\_offline.htm* file to the site before deployment and delete it after successful deployment.</span></span>
4. <span data-ttu-id="2923d-177">Откройте браузер и перейдите к URL-адрес промежуточный сайт и нажмите кнопку **о** страницу, чтобы убедиться, что развертывание выполнено успешно.</span><span class="sxs-lookup"><span data-stu-id="2923d-177">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

## <a name="summary"></a><span data-ttu-id="2923d-178">Сводка</span><span class="sxs-lookup"><span data-stu-id="2923d-178">Summary</span></span>

<span data-ttu-id="2923d-179">Теперь вы развернули обновления приложения с помощью командной строки.</span><span class="sxs-lookup"><span data-stu-id="2923d-179">You have now deployed an application update by using the command line.</span></span>

![О странице в тестовой среде](command-line-deployment/_static/image6.png)

<span data-ttu-id="2923d-181">В следующем руководстве, вы увидите пример расширения Интернета конвейера публикации.</span><span class="sxs-lookup"><span data-stu-id="2923d-181">In the next tutorial, you will see an example of how to extend the web publish pipeline.</span></span> <span data-ttu-id="2923d-182">Пример будет показано, как для развертывания файлов, которые не находятся в проекте.</span><span class="sxs-lookup"><span data-stu-id="2923d-182">The example will show you how to deploy files that are not included in the project.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2923d-183">[Назад](deploying-a-database-update.md)
> [Вперед](deploying-extra-files.md)</span><span class="sxs-lookup"><span data-stu-id="2923d-183">[Previous](deploying-a-database-update.md)
[Next](deploying-extra-files.md)</span></span>
