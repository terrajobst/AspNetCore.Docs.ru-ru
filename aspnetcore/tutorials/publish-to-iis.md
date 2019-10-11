---
title: Публикация приложения ASP.NET Core в службах IIS
author: guardrex
description: Узнайте, как разместить приложение ASP.NET Core на сервере служб IIS.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/03/2019
uid: tutorials/publish-to-iis
ms.openlocfilehash: 820527cc15f883c906d2fdf1c073d443a5b3b40e
ms.sourcegitcommit: d8b12cc1716ee329d7bd2300e201b61e15d506ac
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/04/2019
ms.locfileid: "71942885"
---
# <a name="publish-an-aspnet-core-app-to-iis"></a><span data-ttu-id="ebc15-103">Публикация приложения ASP.NET Core в службах IIS</span><span class="sxs-lookup"><span data-stu-id="ebc15-103">Publish an ASP.NET Core app to IIS</span></span>

<span data-ttu-id="ebc15-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="ebc15-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ebc15-105">В этом руководстве описывается, как разместить приложение ASP.NET Core на сервере служб IIS.</span><span class="sxs-lookup"><span data-stu-id="ebc15-105">This tutorial shows how to host an ASP.NET Core app on an IIS server.</span></span>

<span data-ttu-id="ebc15-106">В руководстве рассматриваются следующие действия:</span><span class="sxs-lookup"><span data-stu-id="ebc15-106">This tutorial covers the following subjects:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ebc15-107">установка пакета размещения .NET Core в Windows Server;</span><span class="sxs-lookup"><span data-stu-id="ebc15-107">Install the .NET Core Hosting Bundle on Windows Server.</span></span>
> * <span data-ttu-id="ebc15-108">создание сайта служб IIS в диспетчере служб IIS;</span><span class="sxs-lookup"><span data-stu-id="ebc15-108">Create an IIS site in IIS Manager.</span></span>
> * <span data-ttu-id="ebc15-109">развертывание приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ebc15-109">Deploy an ASP.NET Core app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ebc15-110">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="ebc15-110">Prerequisites</span></span>

* <span data-ttu-id="ebc15-111">[Пакет SDK для .NET Core](/dotnet/core/sdk), установленный на компьютере разработки.</span><span class="sxs-lookup"><span data-stu-id="ebc15-111">[.NET Core SDK](/dotnet/core/sdk) installed on the development machine.</span></span>
* <span data-ttu-id="ebc15-112">Сервер Windows Server с настроенной ролью **Веб-сервер (IIS)** .</span><span class="sxs-lookup"><span data-stu-id="ebc15-112">Windows Server configured with the **Web Server (IIS)** server role.</span></span> <span data-ttu-id="ebc15-113">Если сервер не настроен для размещения веб-сайтов со службами IIS, следуйте указаниям в разделе *Настройка служб IIS* статьи <xref:host-and-deploy/iis/index#iis-configuration>, а затем вернитесь к этому руководству.</span><span class="sxs-lookup"><span data-stu-id="ebc15-113">If your server isn't configured to host websites with IIS, follow the guidance in the *IIS configuration* section of the <xref:host-and-deploy/iis/index#iis-configuration> article and then return to this tutorial.</span></span>

> [!WARNING]
> <span data-ttu-id="ebc15-114">**Принципы настройки служб IIS и обеспечения безопасности веб-сайта не рассматриваются в этом руководстве.**</span><span class="sxs-lookup"><span data-stu-id="ebc15-114">**IIS configuration and website security involve concepts that aren't covered by this tutorial.**</span></span> <span data-ttu-id="ebc15-115">Перед размещением рабочих приложений в службах IIS ознакомьтесь с руководством по службам IIS в [документации по Microsoft IIS](https://www.iis.net/) и [статьей о размещении ASP.NET Core с помощью служб IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="ebc15-115">Consult the IIS guidance in the [Microsoft IIS documentation](https://www.iis.net/) and the [ASP.NET Core article on hosting with IIS](xref:host-and-deploy/iis/index) before hosting production apps on IIS.</span></span>
>
> <span data-ttu-id="ebc15-116">Важные сценарии размещения служб IIS, не рассматриваемые в этом руководстве:</span><span class="sxs-lookup"><span data-stu-id="ebc15-116">Important scenarios for IIS hosting not covered by this tutorial include:</span></span>
>
> * [<span data-ttu-id="ebc15-117">Создание куста реестра для защиты данных ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ebc15-117">Creation of a registry hive for ASP.NET Core Data Protection</span></span>](xref:host-and-deploy/iis/index#data-protection)
> * [<span data-ttu-id="ebc15-118">Настройка списка управления доступом (ACL) для пула приложений</span><span class="sxs-lookup"><span data-stu-id="ebc15-118">Configuration of the app pool's Access Control List (ACL)</span></span>](xref:host-and-deploy/iis/index#application-pool-identity)
> * <span data-ttu-id="ebc15-119">Чтобы сосредоточиться на принципах развертывания посредством служб IIS, в этом руководстве приложение развертывается без настройки протокола HTTPS в службах IIS.</span><span class="sxs-lookup"><span data-stu-id="ebc15-119">To focus on IIS deployment concepts, this tutorial deploys an app without HTTPS security configured in IIS.</span></span> <span data-ttu-id="ebc15-120">Дополнительные сведения о размещении приложения с поддержкой протокола HTTPS см. в статьях, посвященных безопасности, в разделе [Дополнительные ресурсы](#additional-resources) этой статьи.</span><span class="sxs-lookup"><span data-stu-id="ebc15-120">For more information on hosting an app enabled for HTTPS protocol, see the security topics in the [Additional resources](#additional-resources) section of this article.</span></span> <span data-ttu-id="ebc15-121">Дополнительные рекомендации по размещению приложений ASP.NET Core приведены в статье <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="ebc15-121">Further guidance for hosting ASP.NET Core apps is provided in the <xref:host-and-deploy/iis/index> article.</span></span>

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="ebc15-122">Установка пакета размещения .NET Core</span><span class="sxs-lookup"><span data-stu-id="ebc15-122">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="ebc15-123">Установите *пакет размещения .NET Core* на сервере служб IIS.</span><span class="sxs-lookup"><span data-stu-id="ebc15-123">Install the *.NET Core Hosting Bundle* on the IIS server.</span></span> <span data-ttu-id="ebc15-124">В составе пакета устанавливаются среда выполнения .NET Core, библиотека .NET Core и [модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="ebc15-124">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="ebc15-125">Модуль позволяет запускать приложения ASP.NET Core под управлением IIS.</span><span class="sxs-lookup"><span data-stu-id="ebc15-125">The module allows ASP.NET Core apps to run behind IIS.</span></span>

<span data-ttu-id="ebc15-126">Скачайте установщик по следующей ссылке:</span><span class="sxs-lookup"><span data-stu-id="ebc15-126">Download the installer using the following link:</span></span>

[<span data-ttu-id="ebc15-127">Текущий установщик пакета размещения .NET Core (прямая загрузка)</span><span class="sxs-lookup"><span data-stu-id="ebc15-127">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

1. <span data-ttu-id="ebc15-128">Запустите установщик на сервере служб IIS.</span><span class="sxs-lookup"><span data-stu-id="ebc15-128">Run the installer on the IIS server.</span></span>

1. <span data-ttu-id="ebc15-129">Перезагрузите сервер или в командой оболочке выполните команду **net stop was /y**, а затем — **net start w3svc**.</span><span class="sxs-lookup"><span data-stu-id="ebc15-129">Restart the server or execute **net stop was /y** followed by **net start w3svc** in a command shell.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="ebc15-130">Создание сайта IIS</span><span class="sxs-lookup"><span data-stu-id="ebc15-130">Create the IIS site</span></span>

1. <span data-ttu-id="ebc15-131">На сервере служб IIS создайте папку, в которой будут храниться файлы и папки опубликованного приложения.</span><span class="sxs-lookup"><span data-stu-id="ebc15-131">On the IIS server, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="ebc15-132">На следующем этапе путь к папке предоставляется IIS как физический путь к приложению.</span><span class="sxs-lookup"><span data-stu-id="ebc15-132">In a following step, the folder's path is provided to IIS as the physical path to the app.</span></span>

1. <span data-ttu-id="ebc15-133">В окне диспетчера IIS на панели **Подключения** разверните узел сервера.</span><span class="sxs-lookup"><span data-stu-id="ebc15-133">In IIS Manager, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="ebc15-134">Щелкните правой кнопкой мыши папку **Сайты**.</span><span class="sxs-lookup"><span data-stu-id="ebc15-134">Right-click the **Sites** folder.</span></span> <span data-ttu-id="ebc15-135">В контекстном меню выберите пункт **Добавить веб-сайт**.</span><span class="sxs-lookup"><span data-stu-id="ebc15-135">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="ebc15-136">Укажите **Имя сайта** и задайте **Физический путь** к созданной папке развертывания приложения.</span><span class="sxs-lookup"><span data-stu-id="ebc15-136">Provide a **Site name** and set the **Physical path** to the app's deployment folder that you created.</span></span> <span data-ttu-id="ebc15-137">Укажите конфигурацию **привязки** и нажмите кнопку **ОК**, чтобы создать веб-сайт.</span><span class="sxs-lookup"><span data-stu-id="ebc15-137">Provide the **Binding** configuration and create the website by selecting **OK**.</span></span>

## <a name="create-an-aspnet-core-razor-pages-app"></a><span data-ttu-id="ebc15-138">Создание приложения Razor Pages ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ebc15-138">Create an ASP.NET Core Razor Pages app</span></span>

<span data-ttu-id="ebc15-139">Следуйте указаниям руководства <xref:getting-started>, чтобы создать приложение Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="ebc15-139">Follow the <xref:getting-started> tutorial to create a Razor Pages app.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="ebc15-140">Публикация и развертывание приложения</span><span class="sxs-lookup"><span data-stu-id="ebc15-140">Publish and deploy the app</span></span>

<span data-ttu-id="ebc15-141">*Публикация приложения* означает создание скомпилированного приложения, которое можно разместить на сервере.</span><span class="sxs-lookup"><span data-stu-id="ebc15-141">*Publish an app* means to produce a compiled app that can be hosted by a server.</span></span> <span data-ttu-id="ebc15-142">*Развертывание приложения* означает перемещение опубликованного приложения в систему размещения.</span><span class="sxs-lookup"><span data-stu-id="ebc15-142">*Deploy an app* means to move the published app to a hosting system.</span></span> <span data-ttu-id="ebc15-143">Публикация обеспечивается [пакетом SDK для .NET Core](/dotnet/core/sdk), а развертывание может производиться различными способами.</span><span class="sxs-lookup"><span data-stu-id="ebc15-143">The publish step is handled by the [.NET Core SDK](/dotnet/core/sdk), while the deployment step can be handled by a variety of approaches.</span></span> <span data-ttu-id="ebc15-144">В этом руководстве применяется развертывание с помощью *папок*, которое состоит из следующих этапов:</span><span class="sxs-lookup"><span data-stu-id="ebc15-144">This tutorial adopts the *folder* deployment approach, where:</span></span>

* <span data-ttu-id="ebc15-145">Приложение публикуется в папке.</span><span class="sxs-lookup"><span data-stu-id="ebc15-145">The app is published to a folder.</span></span>
* <span data-ttu-id="ebc15-146">Содержимое папки перемещается в папку на сайте IIS (**физический путь** к сайту в диспетчере IIS).</span><span class="sxs-lookup"><span data-stu-id="ebc15-146">The folder's contents are moved to the IIS site's folder (the **Physical path** to the site in IIS Manager).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ebc15-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ebc15-147">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="ebc15-148">В **обозревателе решений** щелкните правой кнопкой мыши проект и выберите **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="ebc15-148">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span>
1. <span data-ttu-id="ebc15-149">В диалоговом оке **Выберите целевой объект публикации** выберите вариант публикации **Папка**.</span><span class="sxs-lookup"><span data-stu-id="ebc15-149">In the **Pick a publish target** dialog, select the **Folder** publish option.</span></span>
1. <span data-ttu-id="ebc15-150">Укажите путь к **папке или общей папке**.</span><span class="sxs-lookup"><span data-stu-id="ebc15-150">Set the **Folder or File Share** path.</span></span>
   * <span data-ttu-id="ebc15-151">Если вы создали для сайта IIS папку, доступную на компьютере разработки в качестве сетевой папки, укажите путь к общей папке.</span><span class="sxs-lookup"><span data-stu-id="ebc15-151">If you created a folder for the IIS site that's available on the development machine as a network share, provide the path to the share.</span></span> <span data-ttu-id="ebc15-152">Для публикации в общей папке текущий пользователь должен иметь доступ на запись.</span><span class="sxs-lookup"><span data-stu-id="ebc15-152">The current user must have write access to publish to the share.</span></span>
   * <span data-ttu-id="ebc15-153">Если выполнить развертывание непосредственно в папке сайта IIS на сервере IIS невозможно, опубликуйте приложение в папке на съемном носителе и физически переместите опубликованное приложение в папку сайта IIS на сервере, которая является **физическим путем** сайта в диспетчере IIS.</span><span class="sxs-lookup"><span data-stu-id="ebc15-153">If you're unable to deploy directly to the IIS site folder on the IIS server, publish to a folder on removeable media and physically move the published app to the IIS site folder on the server, which is the site's **Physical path** in IIS Manager.</span></span> <span data-ttu-id="ebc15-154">Переместите содержимое папки *bin/Release/{ЦЕЛЕВАЯ ПЛАТФОРМА}/publish* в папку сайта IIS на сервере, которая является **физическим путем** сайта в диспетчере IIS.</span><span class="sxs-lookup"><span data-stu-id="ebc15-154">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/publish* folder to the IIS site folder on the server, which is the site's **Physical path** in IIS Manager.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ebc15-155">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="ebc15-155">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="ebc15-156">В командной оболочке опубликуйте приложение в конфигурации выпуска, выполнив команду [dotnet publish](/dotnet/core/tools/dotnet-publish):</span><span class="sxs-lookup"><span data-stu-id="ebc15-156">In a command shell, publish the app in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command:</span></span>

   ```dotnetcli
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="ebc15-157">Переместите содержимое папки *bin/Release/{ЦЕЛЕВАЯ ПЛАТФОРМА}/publish* в папку сайта IIS на сервере, которая является **физическим путем** сайта в диспетчере IIS.</span><span class="sxs-lookup"><span data-stu-id="ebc15-157">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/publish* folder to the IIS site folder on the server, which is the site's **Physical path** in IIS Manager.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ebc15-158">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="ebc15-158">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="ebc15-159">В **обозревателе решений** щелкните правой кнопкой мыши проект и выберите пункты **Опубликовать** > **Опубликовать в папку**.</span><span class="sxs-lookup"><span data-stu-id="ebc15-159">Right-click on the project in **Solution** and select **Publish** > **Publish to Folder**.</span></span>
1. <span data-ttu-id="ebc15-160">Укажите путь в поле **Выберите папку**.</span><span class="sxs-lookup"><span data-stu-id="ebc15-160">Set the **Choose a folder** path.</span></span>
   * <span data-ttu-id="ebc15-161">Если вы создали для сайта IIS папку, доступную на компьютере разработки в качестве сетевой папки, укажите путь к общей папке.</span><span class="sxs-lookup"><span data-stu-id="ebc15-161">If you created a folder for the IIS site that's available on the development machine as a network share, provide the path to the share.</span></span> <span data-ttu-id="ebc15-162">Для публикации в общей папке текущий пользователь должен иметь доступ на запись.</span><span class="sxs-lookup"><span data-stu-id="ebc15-162">The current user must have write access to publish to the share.</span></span>
   * <span data-ttu-id="ebc15-163">Если выполнить развертывание непосредственно в папке сайта IIS на сервере IIS невозможно, опубликуйте приложение в папке на съемном носителе и физически переместите опубликованное приложение в папку сайта IIS на сервере, которая является **физическим путем** сайта в диспетчере IIS.</span><span class="sxs-lookup"><span data-stu-id="ebc15-163">If you're unable to deploy directly to the IIS site folder on the IIS server, publish to a folder on removeable media and physically move the published app to the IIS site folder on the server, which is the site's **Physical path** in IIS Manager.</span></span> <span data-ttu-id="ebc15-164">Переместите содержимое папки *bin/Release/{ЦЕЛЕВАЯ ПЛАТФОРМА}/publish* в папку сайта IIS на сервере, которая является **физическим путем** сайта в диспетчере IIS.</span><span class="sxs-lookup"><span data-stu-id="ebc15-164">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/publish* folder to the IIS site folder on the server, which is the site's **Physical path** in IIS Manager.</span></span>

---

## <a name="browse-the-website"></a><span data-ttu-id="ebc15-165">Обзор веб-сайта</span><span class="sxs-lookup"><span data-stu-id="ebc15-165">Browse the website</span></span>

<span data-ttu-id="ebc15-166">Приложение будет доступно в браузере после получения первого запроса.</span><span class="sxs-lookup"><span data-stu-id="ebc15-166">The app is accessible in a browser after it receives the first request.</span></span> <span data-ttu-id="ebc15-167">Выполните запрос к приложению в привязке конечной точки, созданной в диспетчере служб IIS для сайта.</span><span class="sxs-lookup"><span data-stu-id="ebc15-167">Make a request to the app at the endpoint binding that you established in IIS Manager for the site.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ebc15-168">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="ebc15-168">Next steps</span></span>

<span data-ttu-id="ebc15-169">В этом руководстве вы узнали, как:</span><span class="sxs-lookup"><span data-stu-id="ebc15-169">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ebc15-170">установка пакета размещения .NET Core в Windows Server;</span><span class="sxs-lookup"><span data-stu-id="ebc15-170">Install the .NET Core Hosting Bundle on Windows Server.</span></span>
> * <span data-ttu-id="ebc15-171">создание сайта служб IIS в диспетчере служб IIS;</span><span class="sxs-lookup"><span data-stu-id="ebc15-171">Create an IIS site in IIS Manager.</span></span>
> * <span data-ttu-id="ebc15-172">развертывание приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ebc15-172">Deploy an ASP.NET Core app.</span></span>

<span data-ttu-id="ebc15-173">Дополнительные сведения о размещении приложений ASP.NET Core в службах IIS см. в обзорной статье о службах IIS:</span><span class="sxs-lookup"><span data-stu-id="ebc15-173">To learn more about hosting ASP.NET Core apps on IIS, see the IIS Overview article:</span></span>

> [!div class="nextstepaction"]
> <xref:host-and-deploy/iis/index>

## <a name="additional-resources"></a><span data-ttu-id="ebc15-174">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="ebc15-174">Additional resources</span></span>

### <a name="articles-in-the-aspnet-core-documentation-set"></a><span data-ttu-id="ebc15-175">Статьи в наборе документации по ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ebc15-175">Articles in the ASP.NET Core documentation set</span></span>

* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/directory-structure>
* <xref:test/troubleshoot-azure-iis>
* <xref:security/enforcing-ssl>

### <a name="articles-pertaining-to-aspnet-core-app-deployment"></a><span data-ttu-id="ebc15-176">Статьи, относящиеся к развертыванию приложений ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ebc15-176">Articles pertaining to ASP.NET Core app deployment</span></span>

* <xref:tutorials/publish-to-azure-webapp-using-vs>
* <xref:tutorials/publish-to-azure-webapp-using-vscode>
* <xref:host-and-deploy/visual-studio-publish-profiles>
* [<span data-ttu-id="ebc15-177">Публикация веб-приложения в папку с помощью Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="ebc15-177">Publish a Web app to a folder using Visual Studio for Mac</span></span>](/visualstudio/mac/publish-folder)

### <a name="articles-on-iis-https-configuration"></a><span data-ttu-id="ebc15-178">Статьи о конфигурации HTTPS служб IIS</span><span class="sxs-lookup"><span data-stu-id="ebc15-178">Articles on IIS HTTPS configuration</span></span>

* [<span data-ttu-id="ebc15-179">Настройка SSL в диспетчере служб IIS</span><span class="sxs-lookup"><span data-stu-id="ebc15-179">Configuring SSL in IIS Manager</span></span>](/iis/manage/configuring-security/configuring-ssl-in-iis-manager)
* [<span data-ttu-id="ebc15-180">Настройка SSL в службах IIS</span><span class="sxs-lookup"><span data-stu-id="ebc15-180">How to Set Up SSL on IIS</span></span>](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)

### <a name="articles-on-iis-and-windows-server"></a><span data-ttu-id="ebc15-181">Статьи о службах IIS и Windows Server</span><span class="sxs-lookup"><span data-stu-id="ebc15-181">Articles on IIS and Windows Server</span></span>

* [<span data-ttu-id="ebc15-182">Официальный веб-сайт Microsoft IIS</span><span class="sxs-lookup"><span data-stu-id="ebc15-182">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="ebc15-183">Библиотека технического содержимого по Windows Server</span><span class="sxs-lookup"><span data-stu-id="ebc15-183">Windows Server technical content library</span></span>](/windows-server/windows-server)
