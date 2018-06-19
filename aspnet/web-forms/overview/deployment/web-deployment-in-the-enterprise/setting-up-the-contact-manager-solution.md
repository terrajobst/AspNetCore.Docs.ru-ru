---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: Настройка диспетчера контактов решения | Документы Microsoft
author: jrjlee
description: В этом разделе описывается загрузка и настройка решения диспетчера контактов для локального выполнения на рабочей станции разработчика.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: e8fb24f5b2d96d864d1aa6bc0f78644773de00ab
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30881816"
---
<a name="setting-up-the-contact-manager-solution"></a><span data-ttu-id="20953-103">Настройка решения диспетчера контактов</span><span class="sxs-lookup"><span data-stu-id="20953-103">Setting Up the Contact Manager Solution</span></span>
====================
<span data-ttu-id="20953-104">по [Джейсон Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="20953-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="20953-105">Скачать PDF</span><span class="sxs-lookup"><span data-stu-id="20953-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="20953-106">В этом разделе описывается загрузка и настройка решения диспетчера контактов для локального выполнения на рабочей станции разработчика.</span><span class="sxs-lookup"><span data-stu-id="20953-106">This topic describes how to download and configure the Contact Manager solution to run locally on a developer workstation.</span></span>


## <a name="system-requirements"></a><span data-ttu-id="20953-107">Требования к системе</span><span class="sxs-lookup"><span data-stu-id="20953-107">System Requirements</span></span>

<span data-ttu-id="20953-108">Для локального запуска решения диспетчера контактов и выполнять другие задачи, описанные в этом учебнике, необходимо будет установить это программное обеспечение на рабочей станции разработчика:</span><span class="sxs-lookup"><span data-stu-id="20953-108">To run the Contact Manager solution locally and to perform the other tasks described in this tutorial, you'll need to install this software on your developer workstation:</span></span>

- <span data-ttu-id="20953-109">Visual Studio 2010 с пакетом 1, Premium или Ultimate Edition</span><span class="sxs-lookup"><span data-stu-id="20953-109">Visual Studio 2010 Service Pack 1, Premium or Ultimate Edition</span></span>
- <span data-ttu-id="20953-110">Internet Information Services (IIS) 7.5 Express</span><span class="sxs-lookup"><span data-stu-id="20953-110">Internet Information Services (IIS) 7.5 Express</span></span>
- <span data-ttu-id="20953-111">SQL Server Express 2008 R2</span><span class="sxs-lookup"><span data-stu-id="20953-111">SQL Server Express 2008 R2</span></span>
- <span data-ttu-id="20953-112">IIS средство веб-развертывания (Web Deploy) 2.1 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="20953-112">IIS Web Deployment Tool (Web Deploy) 2.1 or later</span></span>
- <span data-ttu-id="20953-113">ASP.NET 4.0</span><span class="sxs-lookup"><span data-stu-id="20953-113">ASP.NET 4.0</span></span>
- <span data-ttu-id="20953-114">ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="20953-114">ASP.NET MVC 3</span></span>
- <span data-ttu-id="20953-115">.NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="20953-115">.NET Framework 4</span></span>
- <span data-ttu-id="20953-116">.NET Framework 3.5 SP1</span><span class="sxs-lookup"><span data-stu-id="20953-116">.NET Framework 3.5 SP1</span></span>

<span data-ttu-id="20953-117">За исключением Visual Studio 2010 можно загрузить и установить последние версии всех этих продуктов и компонентов с помощью [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span><span class="sxs-lookup"><span data-stu-id="20953-117">With the exception of Visual Studio 2010, you can download and install the latest versions of all of these products and components through the [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span></span>

## <a name="download-and-extract-the-solution"></a><span data-ttu-id="20953-118">Загрузите и извлеките решения</span><span class="sxs-lookup"><span data-stu-id="20953-118">Download and Extract the Solution</span></span>

<span data-ttu-id="20953-119">Можно загрузить пример приложения диспетчера контактов из коллекции кода MSDN [здесь](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).</span><span class="sxs-lookup"><span data-stu-id="20953-119">You can download the Contact Manager sample application from the MSDN Code Gallery [here](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).</span></span>

## <a name="configure-and-run-the-solution"></a><span data-ttu-id="20953-120">Настройка и запуск решения</span><span class="sxs-lookup"><span data-stu-id="20953-120">Configure and Run the Solution</span></span>

<span data-ttu-id="20953-121">Для настройки и запуска диспетчера контактов решения на локальном компьютере, необходимо выполнить следующие общие действия:</span><span class="sxs-lookup"><span data-stu-id="20953-121">To configure and run the Contact Manager solution on your local machine, you'll need to perform these high-level steps:</span></span>

1. <span data-ttu-id="20953-122">Если у еще его нет, создание локальной базы данных служб приложения ASP.NET с помощью функции управления членством и ролями, доступные.</span><span class="sxs-lookup"><span data-stu-id="20953-122">If you don't have one already, create a local ASP.NET application services database with the membership and role management features enabled.</span></span>
2. <span data-ttu-id="20953-123">Изменение строк подключения в *web.config* файлы, чтобы она указывала на локальном экземпляре SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="20953-123">Edit connection strings in the *web.config* files to point to your local SQL Server Express instance.</span></span>
3. <span data-ttu-id="20953-124">Запустите решение из Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="20953-124">Run the solution from Visual Studio 2010.</span></span>

<span data-ttu-id="20953-125">Оставшаяся часть этого подраздела предоставляет дополнительные рекомендации о том, как выполнить каждую из этих задач.</span><span class="sxs-lookup"><span data-stu-id="20953-125">The remainder of this section provides more guidance on how to complete each of these tasks.</span></span>

<span data-ttu-id="20953-126">**Для создания базы данных служб приложений**</span><span class="sxs-lookup"><span data-stu-id="20953-126">**To create the application services database**</span></span>

1. <span data-ttu-id="20953-127">Откройте командную строку Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="20953-127">Open a Visual Studio 2010 command prompt.</span></span> <span data-ttu-id="20953-128">Для этого на **запустить** последовательно выберите пункты **все программы**, нажмите кнопку **Microsoft Visual Studio 2010**, нажмите кнопку **набора средств Visual Studio**, а затем Нажмите кнопку **командной строки Visual Studio (2010)**.</span><span class="sxs-lookup"><span data-stu-id="20953-128">To do this, on the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, click **Visual Studio Tools**, and then click **Visual Studio Command Prompt (2010)**.</span></span>
2. <span data-ttu-id="20953-129">В командной строке введите следующую команду и нажмите клавишу ВВОД:</span><span class="sxs-lookup"><span data-stu-id="20953-129">At the command prompt, type this command, and then press Enter:</span></span>

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. <span data-ttu-id="20953-130">Используйте **-C** коммутатора, чтобы указать строку подключения для сервера базы данных.</span><span class="sxs-lookup"><span data-stu-id="20953-130">Use the **–C** switch to specify the connection string for your database server.</span></span>
    2. <span data-ttu-id="20953-131">Используйте **–A** для указания функций, которые требуется добавить в базу данных служб приложения.</span><span class="sxs-lookup"><span data-stu-id="20953-131">Use the **–A** switch to specify the application services features you want to add to the database.</span></span> <span data-ttu-id="20953-132">В этом случае **m** указывает, что для добавления поддержки поставщик членства и **r** указывает, что вы хотите добавить поддержку для диспетчера ролей.</span><span class="sxs-lookup"><span data-stu-id="20953-132">In this case, **m** indicates that you want to add support for the membership provider and **r** indicates that you want to add support for the role manager.</span></span>
    3. <span data-ttu-id="20953-133">Используйте **– d** коммутатора, чтобы указать имя для базы данных приложения службы.</span><span class="sxs-lookup"><span data-stu-id="20953-133">Use the **–d** switch to specify a name for your application services database.</span></span> <span data-ttu-id="20953-134">Если параметр не задан, программа создаст базу данных с именем по умолчанию **aspnetdb**.</span><span class="sxs-lookup"><span data-stu-id="20953-134">If you omit this switch, the utility will create a database with the default name of **aspnetdb**.</span></span>
3. <span data-ttu-id="20953-135">Если база данных успешно создана, командной строки будут выведены подтверждения.</span><span class="sxs-lookup"><span data-stu-id="20953-135">When the database has been created successfully, the command prompt will show a confirmation.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="20953-136">Дополнительные сведения о aspnet\_regsql программы, в разделе [средство регистрации ASP.NET SQL Server (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="20953-136">For more information on the aspnet\_regsql utility, see [ASP.NET SQL Server Registration Tool (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span></span>


<span data-ttu-id="20953-137">Следующим шагом является убедитесь в том, что строки подключения в решении диспетчера контактов указывают на локальном экземпляре SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="20953-137">The next step is to make sure that the connection strings in the Contact Manager solution point to your local instance of SQL Server Express.</span></span>

<span data-ttu-id="20953-138">**Чтобы обновить строки соединения**</span><span class="sxs-lookup"><span data-stu-id="20953-138">**To update the connection strings**</span></span>

1. <span data-ttu-id="20953-139">Откройте Диспетчер контактов решение в Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="20953-139">Open the Contact Manager solution in Visual Studio 2010.</span></span>
2. <span data-ttu-id="20953-140">В **обозревателе решений** окна, разверните **ContactManager.Mvc** проекта, а затем дважды щелкните **Web.config** узла.</span><span class="sxs-lookup"><span data-stu-id="20953-140">In the **Solution Explorer** window, expand the **ContactManager.Mvc** project, and then double-click the **Web.config** node.</span></span>

    > [!NOTE]
    > <span data-ttu-id="20953-141">ContactManager.Mvc проект содержит две *web.config* файлов.</span><span class="sxs-lookup"><span data-stu-id="20953-141">The ContactManager.Mvc project includes two *web.config* files.</span></span> <span data-ttu-id="20953-142">Необходимо изменить файл проекта.</span><span class="sxs-lookup"><span data-stu-id="20953-142">You need to edit the project-level file.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. <span data-ttu-id="20953-143">В **connectionStrings** элемент, убедитесь, что строка подключения с именем **ApplicationServices** указывает на локальной базы данных служб приложения ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="20953-143">In the **connectionStrings** element, verify that the connection string named **ApplicationServices** points to your local ASP.NET application services database.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. <span data-ttu-id="20953-144">В **обозревателе решений** окна, разверните **ContactManager.Service** проекта, а затем дважды щелкните **Web.config** узла.</span><span class="sxs-lookup"><span data-stu-id="20953-144">In the **Solution Explorer** window, expand the **ContactManager.Service** project, and then double-click the **Web.config** node.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. <span data-ttu-id="20953-145">В **connectionStrings** элемент в строке подключения с именем **ContactManagerContext**, убедитесь, что **источника данных** свойству локального экземпляра SQL Экспресс-выпуск сервера.</span><span class="sxs-lookup"><span data-stu-id="20953-145">In the **connectionStrings** element, in the connection string named **ContactManagerContext**, verify that the **Data Source** property is set to your local instance of SQL Server Express.</span></span> <span data-ttu-id="20953-146">Нет необходимости изменять что-либо еще в строке подключения.</span><span class="sxs-lookup"><span data-stu-id="20953-146">You don't need to change anything else in the connection string.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. <span data-ttu-id="20953-147">Сохраните все открытые файлы.</span><span class="sxs-lookup"><span data-stu-id="20953-147">Save all open files.</span></span>

<span data-ttu-id="20953-148">Теперь можно запустить диспетчер контактов решение на локальном компьютере.</span><span class="sxs-lookup"><span data-stu-id="20953-148">You should now be ready to run the Contact Manager solution on your local machine.</span></span>

> [!NOTE]
> <span data-ttu-id="20953-149">Если без предварительного создания базы данных служб приложения, выполните следующие действия, ASP.NET создаст базу данных при первом создании пользователя.</span><span class="sxs-lookup"><span data-stu-id="20953-149">If you follow these steps without first creating an application services database, ASP.NET will create the database the first time you attempt to create a user.</span></span> <span data-ttu-id="20953-150">Тем не менее вручную создав базу данных предоставляет гораздо больший контроль над набора средств служб приложения, которые требуется поддерживать.</span><span class="sxs-lookup"><span data-stu-id="20953-150">However, manually creating the database gives you a lot more control over the application services feature set you want to support.</span></span>


<span data-ttu-id="20953-151">**Чтобы запустить решение диспетчера контактов**</span><span class="sxs-lookup"><span data-stu-id="20953-151">**To run the Contact Manager solution**</span></span>

1. <span data-ttu-id="20953-152">В Visual Studio 2010 нажмите клавишу F5.</span><span class="sxs-lookup"><span data-stu-id="20953-152">In Visual Studio 2010, press F5.</span></span>
2. <span data-ttu-id="20953-153">Internet Explorer запускается и запрашивает URL-адрес приложения, обратитесь в службу диспетчера ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="20953-153">Internet Explorer starts up and requests the URL of the Contact Manager ASP.NET MVC 3 application.</span></span> <span data-ttu-id="20953-154">По умолчанию приложение отображает **все контакты** страницы.</span><span class="sxs-lookup"><span data-stu-id="20953-154">By default, the application displays the **All Contacts** page.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. <span data-ttu-id="20953-155">Добавьте несколько контактов и убедитесь, что приложение работает правильно.</span><span class="sxs-lookup"><span data-stu-id="20953-155">Add a few contacts, and then verify that the application works as expected.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. <span data-ttu-id="20953-156">Перейдите к `http://localhost:50114/Account/Register` (Если вы используете приложение на другой порт, измените URL-адрес).</span><span class="sxs-lookup"><span data-stu-id="20953-156">Browse to `http://localhost:50114/Account/Register` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="20953-157">Добавьте имя пользователя, адрес электронной почты и пароль и убедитесь, что вы можете зарегистрировать учетную запись успешно.</span><span class="sxs-lookup"><span data-stu-id="20953-157">Add a user name, email address, and password, and verify that you're able to register an account successfully.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. <span data-ttu-id="20953-158">Перейдите к `http://localhost:50114/Account/LogOn` (Если вы используете приложение на другой порт, измените URL-адрес).</span><span class="sxs-lookup"><span data-stu-id="20953-158">Browse to `http://localhost:50114/Account/LogOn` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="20953-159">Убедитесь, что вы можете выполнить вход с помощью учетной записи, которую вы только что создали.</span><span class="sxs-lookup"><span data-stu-id="20953-159">Verify that you're able to log on using the account you just created.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. <span data-ttu-id="20953-160">Закройте Internet Explorer, чтобы остановить отладку.</span><span class="sxs-lookup"><span data-stu-id="20953-160">Close Internet Explorer to stop debugging.</span></span>

## <a name="conclusion"></a><span data-ttu-id="20953-161">Заключение</span><span class="sxs-lookup"><span data-stu-id="20953-161">Conclusion</span></span>

<span data-ttu-id="20953-162">На этом этапе диспетчера контактов решения должны быть полностью настроены для запуска на локальном компьютере.</span><span class="sxs-lookup"><span data-stu-id="20953-162">At this point, the Contact Manager solution should be fully configured to run on your local machine.</span></span> <span data-ttu-id="20953-163">При работе через другие подразделы в этом учебнике, можно использовать решение как ссылка.</span><span class="sxs-lookup"><span data-stu-id="20953-163">You can use the solution as a reference when you work through the other topics in this tutorial.</span></span>

<span data-ttu-id="20953-164">Следующий раздел [основные сведения о файле проекта](understanding-the-project-file.md), объясняется, как можно использовать пользовательские файлы проекта Microsoft Build Engine (MSBuild) в рамках решения диспетчера контактов для управления процессом развертывания.</span><span class="sxs-lookup"><span data-stu-id="20953-164">The next topic, [Understanding the Project File](understanding-the-project-file.md), explains how you can use the custom Microsoft Build Engine (MSBuild) project files within the Contact Manager solution to control the deployment process.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="20953-165">[Назад](the-contact-manager-solution.md)
> [Вперед](understanding-the-project-file.md)</span><span class="sxs-lookup"><span data-stu-id="20953-165">[Previous](the-contact-manager-solution.md)
[Next](understanding-the-project-file.md)</span></span>
