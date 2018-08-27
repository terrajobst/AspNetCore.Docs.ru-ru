---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: Настройка решения диспетчера контактов | Документация Майкрософт
author: jrjlee
description: В этом разделе описывается скачивание и настройка решения диспетчера контактов для локального запуска на компьютере разработчика.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: d834e6fbd2b46dca8bd7eeadc1eb68b33e62bb38
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829729"
---
<a name="setting-up-the-contact-manager-solution"></a><span data-ttu-id="43006-103">Настройка решения диспетчера контактов</span><span class="sxs-lookup"><span data-stu-id="43006-103">Setting Up the Contact Manager Solution</span></span>
====================
<span data-ttu-id="43006-104">по [Джейсон Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="43006-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="43006-105">Загрузить PDF-файл</span><span class="sxs-lookup"><span data-stu-id="43006-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="43006-106">В этом разделе описывается скачивание и настройка решения диспетчера контактов для локального запуска на компьютере разработчика.</span><span class="sxs-lookup"><span data-stu-id="43006-106">This topic describes how to download and configure the Contact Manager solution to run locally on a developer workstation.</span></span>


## <a name="system-requirements"></a><span data-ttu-id="43006-107">Требования к системе</span><span class="sxs-lookup"><span data-stu-id="43006-107">System Requirements</span></span>

<span data-ttu-id="43006-108">Для локального запуска решения диспетчера контактов и выполнения других задач, описанных в этом руководстве, необходимо установить это программное обеспечение на рабочей станции разработчика:</span><span class="sxs-lookup"><span data-stu-id="43006-108">To run the Contact Manager solution locally and to perform the other tasks described in this tutorial, you'll need to install this software on your developer workstation:</span></span>

- <span data-ttu-id="43006-109">Visual Studio 2010 с пакетом 1, Premium или Ultimate Edition</span><span class="sxs-lookup"><span data-stu-id="43006-109">Visual Studio 2010 Service Pack 1, Premium or Ultimate Edition</span></span>
- <span data-ttu-id="43006-110">Internet Information Services (IIS) 7.5 Express</span><span class="sxs-lookup"><span data-stu-id="43006-110">Internet Information Services (IIS) 7.5 Express</span></span>
- <span data-ttu-id="43006-111">SQL Server Express 2008 R2</span><span class="sxs-lookup"><span data-stu-id="43006-111">SQL Server Express 2008 R2</span></span>
- <span data-ttu-id="43006-112">IIS средство веб-развертывания (Web Deploy) 2.1 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="43006-112">IIS Web Deployment Tool (Web Deploy) 2.1 or later</span></span>
- <span data-ttu-id="43006-113">ASP.NET 4.0</span><span class="sxs-lookup"><span data-stu-id="43006-113">ASP.NET 4.0</span></span>
- <span data-ttu-id="43006-114">ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="43006-114">ASP.NET MVC 3</span></span>
- <span data-ttu-id="43006-115">.NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="43006-115">.NET Framework 4</span></span>
- <span data-ttu-id="43006-116">.NET Framework 3.5 SP1</span><span class="sxs-lookup"><span data-stu-id="43006-116">.NET Framework 3.5 SP1</span></span>

<span data-ttu-id="43006-117">За исключением Visual Studio 2010, можно загрузить и установить последние версии всех этих продуктов и компонентов с помощью [установщика веб-платформы](https://go.microsoft.com/?linkid=9805118).</span><span class="sxs-lookup"><span data-stu-id="43006-117">With the exception of Visual Studio 2010, you can download and install the latest versions of all of these products and components through the [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span></span>

## <a name="download-and-extract-the-solution"></a><span data-ttu-id="43006-118">Загрузите и извлеките решения</span><span class="sxs-lookup"><span data-stu-id="43006-118">Download and Extract the Solution</span></span>

<span data-ttu-id="43006-119">Можно загрузить пример приложения диспетчера контактов из коллекции кода MSDN [здесь](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).</span><span class="sxs-lookup"><span data-stu-id="43006-119">You can download the Contact Manager sample application from the MSDN Code Gallery [here](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).</span></span>

## <a name="configure-and-run-the-solution"></a><span data-ttu-id="43006-120">Настройка и запуск решения</span><span class="sxs-lookup"><span data-stu-id="43006-120">Configure and Run the Solution</span></span>

<span data-ttu-id="43006-121">Чтобы настроить и запустить решение диспетчера контактов на локальном компьютере, необходимо выполнить следующие действия:</span><span class="sxs-lookup"><span data-stu-id="43006-121">To configure and run the Contact Manager solution on your local machine, you'll need to perform these high-level steps:</span></span>

1. <span data-ttu-id="43006-122">Если вы не сделали, создайте локальной базы данных служб приложения ASP.NET с членством и ролями управления функциями.</span><span class="sxs-lookup"><span data-stu-id="43006-122">If you don't have one already, create a local ASP.NET application services database with the membership and role management features enabled.</span></span>
2. <span data-ttu-id="43006-123">Изменение строк подключения в *web.config* файлы, чтобы он указывал на локальный экземпляр SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="43006-123">Edit connection strings in the *web.config* files to point to your local SQL Server Express instance.</span></span>
3. <span data-ttu-id="43006-124">Запустите решение из Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="43006-124">Run the solution from Visual Studio 2010.</span></span>

<span data-ttu-id="43006-125">В оставшейся части этого раздела предоставляет дополнительные рекомендации о том, как выполнить каждую из этих задач.</span><span class="sxs-lookup"><span data-stu-id="43006-125">The remainder of this section provides more guidance on how to complete each of these tasks.</span></span>

<span data-ttu-id="43006-126">**Для создания базы данных служб приложений**</span><span class="sxs-lookup"><span data-stu-id="43006-126">**To create the application services database**</span></span>

1. <span data-ttu-id="43006-127">Откройте командную строку Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="43006-127">Open a Visual Studio 2010 command prompt.</span></span> <span data-ttu-id="43006-128">Для этого на **запустить** последовательно выберите пункты **все программы**, нажмите кнопку **Microsoft Visual Studio 2010**, нажмите кнопку **средств Visual Studio**, а затем Нажмите кнопку **Командная строка Visual Studio (2010)**.</span><span class="sxs-lookup"><span data-stu-id="43006-128">To do this, on the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, click **Visual Studio Tools**, and then click **Visual Studio Command Prompt (2010)**.</span></span>
2. <span data-ttu-id="43006-129">В командной строке введите следующую команду и нажмите клавишу ВВОД:</span><span class="sxs-lookup"><span data-stu-id="43006-129">At the command prompt, type this command, and then press Enter:</span></span>

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. <span data-ttu-id="43006-130">Используйте **– C** параметр, чтобы указать строку подключения для сервера базы данных.</span><span class="sxs-lookup"><span data-stu-id="43006-130">Use the **–C** switch to specify the connection string for your database server.</span></span>
    2. <span data-ttu-id="43006-131">Используйте **–A** для указания функций, которые вы хотите добавить в базу данных служб приложения.</span><span class="sxs-lookup"><span data-stu-id="43006-131">Use the **–A** switch to specify the application services features you want to add to the database.</span></span> <span data-ttu-id="43006-132">В этом случае **m** указывает, что вы хотите добавить поддержку для поставщика членства и **r** указывает, что вы хотите добавить поддержку диспетчера ролей.</span><span class="sxs-lookup"><span data-stu-id="43006-132">In this case, **m** indicates that you want to add support for the membership provider and **r** indicates that you want to add support for the role manager.</span></span>
    3. <span data-ttu-id="43006-133">Используйте **– d** параметр, чтобы указать имя для вашей базы данных служб приложений.</span><span class="sxs-lookup"><span data-stu-id="43006-133">Use the **–d** switch to specify a name for your application services database.</span></span> <span data-ttu-id="43006-134">Если параметр не задан, программа создаст базу данных с именем по умолчанию **aspnetdb**.</span><span class="sxs-lookup"><span data-stu-id="43006-134">If you omit this switch, the utility will create a database with the default name of **aspnetdb**.</span></span>
3. <span data-ttu-id="43006-135">Когда база данных создана успешно, командной строке будет отображаться подтверждение.</span><span class="sxs-lookup"><span data-stu-id="43006-135">When the database has been created successfully, the command prompt will show a confirmation.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="43006-136">Дополнительные сведения о aspnet\_regsql служебной программы, см. в разделе [средство регистрации ASP.NET SQL Server (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="43006-136">For more information on the aspnet\_regsql utility, see [ASP.NET SQL Server Registration Tool (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span></span>


<span data-ttu-id="43006-137">Следующим шагом является убедитесь, что строки подключения в решение диспетчера контактов указывают на локальном экземпляре SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="43006-137">The next step is to make sure that the connection strings in the Contact Manager solution point to your local instance of SQL Server Express.</span></span>

<span data-ttu-id="43006-138">**Чтобы обновить строки соединения**</span><span class="sxs-lookup"><span data-stu-id="43006-138">**To update the connection strings**</span></span>

1. <span data-ttu-id="43006-139">Откройте решение диспетчера контактов в Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="43006-139">Open the Contact Manager solution in Visual Studio 2010.</span></span>
2. <span data-ttu-id="43006-140">В **обозревателе решений** окне разверните **ContactManager.Mvc** проекта, а затем дважды щелкните **Web.config** узла.</span><span class="sxs-lookup"><span data-stu-id="43006-140">In the **Solution Explorer** window, expand the **ContactManager.Mvc** project, and then double-click the **Web.config** node.</span></span>

    > [!NOTE]
    > <span data-ttu-id="43006-141">ContactManager.Mvc проект включает два *web.config* файлов.</span><span class="sxs-lookup"><span data-stu-id="43006-141">The ContactManager.Mvc project includes two *web.config* files.</span></span> <span data-ttu-id="43006-142">Необходимо изменить файл проекта.</span><span class="sxs-lookup"><span data-stu-id="43006-142">You need to edit the project-level file.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. <span data-ttu-id="43006-143">В **connectionStrings** элемента, убедитесь, что строка подключения с именем **ApplicationServices** указывает локальную базу данных служб приложения ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="43006-143">In the **connectionStrings** element, verify that the connection string named **ApplicationServices** points to your local ASP.NET application services database.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. <span data-ttu-id="43006-144">В **обозревателе решений** окне разверните **ContactManager.Service** проекта, а затем дважды щелкните **Web.config** узла.</span><span class="sxs-lookup"><span data-stu-id="43006-144">In the **Solution Explorer** window, expand the **ContactManager.Service** project, and then double-click the **Web.config** node.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. <span data-ttu-id="43006-145">В **connectionStrings** элемента в строке подключения с именем **ContactManagerContext**, убедитесь, что **источника данных** свойство имеет значение локального экземпляра SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="43006-145">In the **connectionStrings** element, in the connection string named **ContactManagerContext**, verify that the **Data Source** property is set to your local instance of SQL Server Express.</span></span> <span data-ttu-id="43006-146">Не нужно изменять что-нибудь еще в строке подключения.</span><span class="sxs-lookup"><span data-stu-id="43006-146">You don't need to change anything else in the connection string.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. <span data-ttu-id="43006-147">Сохраните все открытые файлы.</span><span class="sxs-lookup"><span data-stu-id="43006-147">Save all open files.</span></span>

<span data-ttu-id="43006-148">Теперь можно все готово к запуску решения диспетчера контактов на локальном компьютере.</span><span class="sxs-lookup"><span data-stu-id="43006-148">You should now be ready to run the Contact Manager solution on your local machine.</span></span>

> [!NOTE]
> <span data-ttu-id="43006-149">Если вы выполните следующие действия без предварительного создания базы данных служб приложений, ASP.NET создаст базу данных первой попытке создать пользователя.</span><span class="sxs-lookup"><span data-stu-id="43006-149">If you follow these steps without first creating an application services database, ASP.NET will create the database the first time you attempt to create a user.</span></span> <span data-ttu-id="43006-150">Тем не менее вручную создав базу данных дает гораздо больший контроль над набора средств служб приложения, которые требуется поддерживать.</span><span class="sxs-lookup"><span data-stu-id="43006-150">However, manually creating the database gives you a lot more control over the application services feature set you want to support.</span></span>


<span data-ttu-id="43006-151">**Чтобы запустить решение диспетчера контактов**</span><span class="sxs-lookup"><span data-stu-id="43006-151">**To run the Contact Manager solution**</span></span>

1. <span data-ttu-id="43006-152">В Visual Studio 2010 нажмите клавишу F5.</span><span class="sxs-lookup"><span data-stu-id="43006-152">In Visual Studio 2010, press F5.</span></span>
2. <span data-ttu-id="43006-153">Internet Explorer запускается и запрашивает URL-адреса приложения Contact Manager ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="43006-153">Internet Explorer starts up and requests the URL of the Contact Manager ASP.NET MVC 3 application.</span></span> <span data-ttu-id="43006-154">По умолчанию приложение отображает **все контакты** страницы.</span><span class="sxs-lookup"><span data-stu-id="43006-154">By default, the application displays the **All Contacts** page.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. <span data-ttu-id="43006-155">Добавить несколько контактов и убедитесь, что приложение работает должным образом.</span><span class="sxs-lookup"><span data-stu-id="43006-155">Add a few contacts, and then verify that the application works as expected.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. <span data-ttu-id="43006-156">Перейдите к `http://localhost:50114/Account/Register` (Настройка URL-адрес, если вы размещаете приложение на другой порт).</span><span class="sxs-lookup"><span data-stu-id="43006-156">Browse to `http://localhost:50114/Account/Register` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="43006-157">Добавьте имя пользователя, адрес электронной почты и пароль и убедитесь, что вы можете зарегистрировать учетную запись успешно.</span><span class="sxs-lookup"><span data-stu-id="43006-157">Add a user name, email address, and password, and verify that you're able to register an account successfully.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. <span data-ttu-id="43006-158">Перейдите к `http://localhost:50114/Account/LogOn` (Настройка URL-адрес, если вы размещаете приложение на другой порт).</span><span class="sxs-lookup"><span data-stu-id="43006-158">Browse to `http://localhost:50114/Account/LogOn` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="43006-159">Убедитесь, что вы можете выполнить вход с использованием учетной записи, которую вы только что создали.</span><span class="sxs-lookup"><span data-stu-id="43006-159">Verify that you're able to log on using the account you just created.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. <span data-ttu-id="43006-160">Закройте Internet Explorer, чтобы остановить отладку.</span><span class="sxs-lookup"><span data-stu-id="43006-160">Close Internet Explorer to stop debugging.</span></span>

## <a name="conclusion"></a><span data-ttu-id="43006-161">Заключение</span><span class="sxs-lookup"><span data-stu-id="43006-161">Conclusion</span></span>

<span data-ttu-id="43006-162">На этом этапе решение диспетчера контактов должен быть полностью настроен для запуска на локальном компьютере.</span><span class="sxs-lookup"><span data-stu-id="43006-162">At this point, the Contact Manager solution should be fully configured to run on your local machine.</span></span> <span data-ttu-id="43006-163">Решение можно использовать как ссылка, при работе через другие подразделы в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="43006-163">You can use the solution as a reference when you work through the other topics in this tutorial.</span></span>

<span data-ttu-id="43006-164">Следующий раздел, [основные сведения о файле проекта](understanding-the-project-file.md), объясняется, как можно использовать пользовательские файлы проекта Microsoft Build Engine (MSBuild) в рамках решения диспетчера контактов для управления процессом развертывания.</span><span class="sxs-lookup"><span data-stu-id="43006-164">The next topic, [Understanding the Project File](understanding-the-project-file.md), explains how you can use the custom Microsoft Build Engine (MSBuild) project files within the Contact Manager solution to control the deployment process.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="43006-165">[Назад](the-contact-manager-solution.md)
> [Вперед](understanding-the-project-file.md)</span><span class="sxs-lookup"><span data-stu-id="43006-165">[Previous](the-contact-manager-solution.md)
[Next](understanding-the-project-file.md)</span></span>
