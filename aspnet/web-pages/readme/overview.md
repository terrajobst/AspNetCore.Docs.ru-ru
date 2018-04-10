---
uid: web-pages/readme/overview
title: Файл WebMatrix Readme | Документы Microsoft
author: rick-anderson
description: WebMatrix и ASP.NET Web Pages (Razor) версии 1.0 Readme
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/06/2011
ms.topic: article
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: c65ee58b8c13b0b4acb6e7c9b631c8235e791506
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/10/2018
---
<a name="webmatrix-readme"></a><span data-ttu-id="01652-103">WebMatrix Readme</span><span class="sxs-lookup"><span data-stu-id="01652-103">WebMatrix Readme</span></span>
====================
<span data-ttu-id="01652-104">13 января 2011 г.</span><span class="sxs-lookup"><span data-stu-id="01652-104">13 January 2011</span></span>

## <a name="contents"></a><span data-ttu-id="01652-105">Описание</span><span class="sxs-lookup"><span data-stu-id="01652-105">Contents</span></span>

> [!NOTE]
> <span data-ttu-id="01652-106">Этот файл readme применяется к версии 1.0 WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="01652-106">This readme applies to the 1.0 release of WebMatrix.</span></span>


- [<span data-ttu-id="01652-107">Обзор набора средств Visual Studio для Unity</span><span class="sxs-lookup"><span data-stu-id="01652-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="01652-108">Установка</span><span class="sxs-lookup"><span data-stu-id="01652-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="01652-109">Сведения о публикации приложений</span><span class="sxs-lookup"><span data-stu-id="01652-109">How to Publish Applications</span></span>](#InstructionsForPublishingApplications)
- [<span data-ttu-id="01652-110">Изменения и проблемы</span><span class="sxs-lookup"><span data-stu-id="01652-110">Changes and Issues</span></span>](#ChangesAndIssues)

    - [<span data-ttu-id="01652-111">Установка WebMatrix 1.0</span><span class="sxs-lookup"><span data-stu-id="01652-111">WebMatrix 1.0 Installation</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="01652-112">Веб-страницы ASP.NET</span><span class="sxs-lookup"><span data-stu-id="01652-112">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="01652-113">WebMatrix</span><span class="sxs-lookup"><span data-stu-id="01652-113">WebMatrix</span></span>](#Known_Issues_WebMatrix)
    - [<span data-ttu-id="01652-114">IIS Express</span><span class="sxs-lookup"><span data-stu-id="01652-114">IIS Express</span></span>](#Known_Issues_IISExpress)
    - [<span data-ttu-id="01652-115">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="01652-115">SQL Server Compact</span></span>](#Known_Issues_SQLServerCompact)
    - [<span data-ttu-id="01652-116">Установка приложений</span><span class="sxs-lookup"><span data-stu-id="01652-116">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="01652-117">Публикация приложений</span><span class="sxs-lookup"><span data-stu-id="01652-117">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
- [<span data-ttu-id="01652-118">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="01652-118">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="01652-119">Обзор</span><span class="sxs-lookup"><span data-stu-id="01652-119">Overview</span></span>

> <span data-ttu-id="01652-120">Microsoft WebMatrix 1.0 представляет собой стек бесплатной разработки, который устанавливается в минутах.</span><span class="sxs-lookup"><span data-stu-id="01652-120">Microsoft WebMatrix 1.0 is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="01652-121">Она интегрирует веб-сервера базы данных и платформами программирования для создания единой, интегрированной среды.</span><span class="sxs-lookup"><span data-stu-id="01652-121">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="01652-122">Вы можете использовать WebMatrix для упрощения разработки кода, тестирования и публикация собственных ASP.NET или PHP, веб-сайт, или вы можете использовать WebMatrix для создания нового веб-сайтов с помощью таких популярных приложений с открытым исходным кодом как DotNetNuke, Umbraco, WordPress и Joomla.</span><span class="sxs-lookup"><span data-stu-id="01652-122">You can use WebMatrix to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="01652-123">WebMatrix использует же мощный веб-сервер, СУБД и платформы среды выполнения веб-сайта в Интернете, что упрощает и ускоряет переход из среды разработки в рабочей среде smooth и удобный.</span><span class="sxs-lookup"><span data-stu-id="01652-123">WebMatrix uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>


<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="01652-124">Установка</span><span class="sxs-lookup"><span data-stu-id="01652-124">Installation</span></span>

> <span data-ttu-id="01652-125">Чтобы установить WebMatrix 1.0, необходимо сначала установить [платформы установщика Microsoft Web 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="01652-125">To install WebMatrix 1.0, you must first install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="01652-126">После установки установщика веб-платформы, его можно использовать для установки WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="01652-126">After you've installed the Web Platform Installer, you can use it to install WebMatrix.</span></span>
> 
> <span data-ttu-id="01652-127">Если во время установки возникают проблемы, см. [Устранение неполадок с Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span><span class="sxs-lookup"><span data-stu-id="01652-127">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a><span data-ttu-id="01652-128">Сведения о публикации приложений</span><span class="sxs-lookup"><span data-stu-id="01652-128">How to Publish Applications</span></span>

> <span data-ttu-id="01652-129">В разделе [пошаговые инструкции для публикации приложений](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="01652-129">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a><span data-ttu-id="01652-130">Изменения и проблемы</span><span class="sxs-lookup"><span data-stu-id="01652-130">Changes and Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a><span data-ttu-id="01652-131">Проблемы с установкой 1.0 WebMatrix</span><span class="sxs-lookup"><span data-stu-id="01652-131">WebMatrix 1.0 Installation Issues</span></span>

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="01652-132">Проблема: WebMatrix 1.0 доступна только на платформах, поддерживающих Microsoft .NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="01652-132">Issue: WebMatrix 1.0 is available only on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="01652-133">.NET Framework версии 4 является обязательным для WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="01652-133">The .NET Framework version 4 is required for WebMatrix.</span></span> <span data-ttu-id="01652-134">В некоторых случаях установщик WebMatrix 1.0 позволит попытается установить на платформу, которая не является частью набора поддерживаемых конфигураций.</span><span class="sxs-lookup"><span data-stu-id="01652-134">In certain cases, the WebMatrix 1.0 installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="01652-135">В частности Windows Vista без обновления SP1 позволит вам начать установку WebMatrix, но компонент .NET Framework 4 не удастся и блокировать установку.</span><span class="sxs-lookup"><span data-stu-id="01652-135">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="01652-136">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="01652-136">**Workaround**</span></span>  
> <span data-ttu-id="01652-137">Установите на поддерживаемых платформах, включая:</span><span class="sxs-lookup"><span data-stu-id="01652-137">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="01652-138">Windows 7</span><span class="sxs-lookup"><span data-stu-id="01652-138">Windows 7</span></span>
> - <span data-ttu-id="01652-139">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="01652-139">Windows Server 2008</span></span>
> - <span data-ttu-id="01652-140">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="01652-140">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="01652-141">Windows Vista с пакетом обновления 1 (SP1) или выше</span><span class="sxs-lookup"><span data-stu-id="01652-141">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="01652-142">Windows XP с пакетом обновления 3 (SP3)</span><span class="sxs-lookup"><span data-stu-id="01652-142">Windows XP SP3</span></span>
> - <span data-ttu-id="01652-143">Windows Server 2003 с пакетом обновления 2 (SP2)</span><span class="sxs-lookup"><span data-stu-id="01652-143">Windows Server 2003 SP2</span></span>


#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="01652-144">Проблема: Не удается установить WebMatrix 1.0, если Microsoft Visual Studio 2008 устанавливается без Microsoft Visual Studio 2008 с пакетом обновления 1</span><span class="sxs-lookup"><span data-stu-id="01652-144">Issue: Cannot install WebMatrix 1.0 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="01652-145">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="01652-145">**Workaround**</span></span>  
> <span data-ttu-id="01652-146">Установка [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) из центра загрузки Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="01652-146">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="01652-147">Проблема: Некоторые сборки для SQL Server Compact 4.0 не установлены в глобальном кэше СБОРОК</span><span class="sxs-lookup"><span data-stu-id="01652-147">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="01652-148">Управляемые сборки для SQL Server Compact 4.0 не помещается в глобальный кэш сборок (GAC), при установке SQL Server Compact 4.0 на 64-разрядном компьютере и что компьютер имеет только профиле .NET Framework 3.5 с пакетом обновления 1 клиента установлен.</span><span class="sxs-lookup"><span data-stu-id="01652-148">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="01652-149">Управляемые сборки, которые не установлены в глобальном кэше СБОРОК являются:</span><span class="sxs-lookup"><span data-stu-id="01652-149">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="01652-150">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span><span class="sxs-lookup"><span data-stu-id="01652-150">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="01652-151">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span><span class="sxs-lookup"><span data-stu-id="01652-151">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="01652-152">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="01652-152">**Workaround**</span></span>  
> <span data-ttu-id="01652-153">Удаление SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="01652-153">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="01652-154">Загрузить и установить полную версию .NET Framework 3.5 с пакетом обновления 1 из следующего расположения:</span><span class="sxs-lookup"><span data-stu-id="01652-154">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="01652-155">Microsoft .NET Framework 3.5 пакетом обновления 1 (полный пакет)</span><span class="sxs-lookup"><span data-stu-id="01652-155">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="01652-156">Затем переустановите SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="01652-156">Then reinstall SQL Server Compact 4.0.</span></span>


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="01652-157">Проблема: Не удается удалить SQL Server Compact с помощью командной строки</span><span class="sxs-lookup"><span data-stu-id="01652-157">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="01652-158">Удаление SQL Server Compact через параметры командной строки не работает в данном выпуске.</span><span class="sxs-lookup"><span data-stu-id="01652-158">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="01652-159">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="01652-159">**Workaround**</span></span>  
> <span data-ttu-id="01652-160">Используйте *программы и компоненты* в панели управления Windows, чтобы удалить Microsoft SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="01652-160">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="01652-161">Веб-страницы ASP.NET</span><span class="sxs-lookup"><span data-stu-id="01652-161">ASP.NET Web Pages</span></span>

<span data-ttu-id="01652-162">Этот раздел документа описывает новые возможности, изменения и известные проблемы с версии 1.0 веб-страниц ASP.NET с синтаксисом Razor.</span><span class="sxs-lookup"><span data-stu-id="01652-162">This section of the document describes new features, changes, and known issues with the 1.0 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="01652-163">Новые возможности</span><span class="sxs-lookup"><span data-stu-id="01652-163">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="01652-164">Изменения</span><span class="sxs-lookup"><span data-stu-id="01652-164">Changes</span></span>](#Changes)
- [<span data-ttu-id="01652-165">Проблемы</span><span class="sxs-lookup"><span data-stu-id="01652-165">Issues</span></span>](#Issues)

#### <a id="NewFeatures"></a>  <span data-ttu-id="01652-166">Новые возможности</span><span class="sxs-lookup"><span data-stu-id="01652-166">New Features</span></span>

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a><span data-ttu-id="01652-167">Новинка: Добавлен параметр конфигурации для отключения диспетчера пакетов</span><span class="sxs-lookup"><span data-stu-id="01652-167">New: Configuration setting added to disable the package manager</span></span>

> <span data-ttu-id="01652-168">Новый `asp:AdminManagerEnabled` ключ доступен для `<appSettings>` элемент в *web.config* файл, который позволяет полностью отключить диспетчер пакетов.</span><span class="sxs-lookup"><span data-stu-id="01652-168">A new `asp:AdminManagerEnabled` key is available for the `<appSettings>` element in the *web.config* file, which lets you completely disable the package manager.</span></span> <span data-ttu-id="01652-169">Значение по умолчанию для этого элемента имеет значение true, это значит, что, если он не включен в *web.config* включен файл диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="01652-169">The default value for this element is true, meaning that if it is not included in the *web.config* file, the package manager is enabled.</span></span> <span data-ttu-id="01652-170">Чтобы отключить диспетчер пакетов, добавьте следующий элемент для *web.config* файл в корневом каталоге веб-сайта:</span><span class="sxs-lookup"><span data-stu-id="01652-170">To disable the package manager, add the following element to the *web.config* file in the root of the website:</span></span>
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a>  <span data-ttu-id="01652-171">Изменения</span><span class="sxs-lookup"><span data-stu-id="01652-171">Changes</span></span>

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a><span data-ttu-id="01652-172">Изменение: переименована в «asp: AdminFolderVirtualPath» ключ «webPages:AdminFolderVirtualPath»</span><span class="sxs-lookup"><span data-stu-id="01652-172">Change: "webPages:AdminFolderVirtualPath" key renamed to "asp:AdminFolderVirtualPath"</span></span>

> <span data-ttu-id="01652-173">`webPages:AdminFolderVirtualPath` Ключ, который может быть добавлен к *web.config* был переименован файл, чтобы указать расположение диспетчер пакетов для использования `asp:` вместо пространства имен `webPages` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="01652-173">The `webPages:AdminFolderVirtualPath` key that can be added to the *web.config* file to specify the location of the package manager has been renamed to use the `asp:` namespace instead of the `webPages` namespace.</span></span> <span data-ttu-id="01652-174">Если используется этот элемент, его необходимо переименовать в файле конфигурации.</span><span class="sxs-lookup"><span data-stu-id="01652-174">If you have used this element, you must rename it in the configuration file.</span></span>


#### <a id="Issues"></a>  <span data-ttu-id="01652-175">Известные проблемы</span><span class="sxs-lookup"><span data-stu-id="01652-175">Known Issues</span></span>

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a><span data-ttu-id="01652-176">Проблема: Паролей авторизованных пользователей, которые больше не распознан</span><span class="sxs-lookup"><span data-stu-id="01652-176">Issue: Passwords for membership users no longer recognized</span></span>

> <span data-ttu-id="01652-177">Для большей безопасности был изменен алгоритм для создания и хранения паролей членства (имя входа).</span><span class="sxs-lookup"><span data-stu-id="01652-177">The algorithm for creating and storing membership (login) passwords has been changed to be more secure.</span></span> <span data-ttu-id="01652-178">В результате пароли, сохраненные для членов (пользователей), созданных в бета-версии ASP.NET Razor не распознаются.</span><span class="sxs-lookup"><span data-stu-id="01652-178">As a result, the passwords stored for members (users) created in Beta versions of ASP.NET Razor will not be recognized.</span></span> 
> 
> <span data-ttu-id="01652-179">**Инструкции по решению** Если сайт еще не были размещены в рабочей среде, удалить записи из базы данных членства.</span><span class="sxs-lookup"><span data-stu-id="01652-179">**Workaround** If the site has not yet been put into production, remove the user records from the membership database.</span></span> <span data-ttu-id="01652-180">Если база данных находится в режиме реального времени, программным образом создать повторно существующие пароли в базе данных членства.</span><span class="sxs-lookup"><span data-stu-id="01652-180">If database is live, programmatically regenerate existing passwords in the membership database.</span></span>


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="01652-181">Проблема: Непредвиденное поведение при использовании пользовательские таблицы для членства</span><span class="sxs-lookup"><span data-stu-id="01652-181">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="01652-182">Чтобы инициализировать поставщик членства для веб-сайта ASP.NET Razor, вызовите `WebSecurity.InitializeDatabaseConnection` метод.</span><span class="sxs-lookup"><span data-stu-id="01652-182">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="01652-183">(В WebMatrix шаблоне начального сайта включает вызов этого метода в * \_AppStart.cshtml* файл.) Если `autoCreateTables` параметр этого метода имеет значение в значение true (по умолчанию, ему присваивается значение true, если в шаблоне начального сайта), и если имя таблицы нераспознанный передается в метод (второй параметр), метод не выдает ошибку.</span><span class="sxs-lookup"><span data-stu-id="01652-183">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="01652-184">Вместо этого он автоматически создадут таблицу.</span><span class="sxs-lookup"><span data-stu-id="01652-184">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="01652-185">Это может вызвать проблемы, если планируется использовать пользовательские таблицы членства, но передает имя неправильной таблицы в `WebSecurity.InitializeDatabaseConnection` метод.</span><span class="sxs-lookup"><span data-stu-id="01652-185">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="01652-186">Поскольку метод по умолчанию вызывает ошибку, если таблицы не существует, и вместо этого он создает новую таблицу, приложение может появиться работает.</span><span class="sxs-lookup"><span data-stu-id="01652-186">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="01652-187">Тем не менее код приложения, который основан на пользовательские таблицы (и поля в нем) со временем может завершиться непредвиденные ошибки.</span><span class="sxs-lookup"><span data-stu-id="01652-187">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="01652-188">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="01652-188">**Workaround**</span></span>  
> <span data-ttu-id="01652-189">Убедитесь, что имя, переданное в `InitializeDatabaseConnection` метод соответствует таблице в базе данных членства профиль пользователя, или убедитесь, что `autoCreateTables` параметр имеет значение false.</span><span class="sxs-lookup"><span data-stu-id="01652-189">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a><span data-ttu-id="01652-190">Проблема: Сообщение об ошибке «модуля администрирования требуется доступ к ~/App\_данных»</span><span class="sxs-lookup"><span data-stu-id="01652-190">Issue: Error message "The Admin Module requires access to ~/App\_Data"</span></span>

> <span data-ttu-id="01652-191">В некоторых случаях для создания пользователей или работы со система членства ASP.NET может привести к странице для отображения ошибки *модуль администрирования требуется доступ к ~/App\_данные*.</span><span class="sxs-lookup"><span data-stu-id="01652-191">Under some circumstances, trying to create users or otherwise work with the ASP.NET membership system can cause the page to display the error *The Admin Module requires access to ~/App\_Data*.</span></span> <span data-ttu-id="01652-192">Это происходит, если учетная запись, под управлением IIS или IIS Express не имеет разрешения на создание и запись в *приложения\_данные* папки в корневом каталоге веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="01652-192">This occurs if the account that IIS or IIS Express is running under does not have permissions to create and write to the *App\_Data* folder under the website root.</span></span> 
> 
> <span data-ttu-id="01652-193">**Инструкции по решению** вручную создать *приложения\_данные* папку веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="01652-193">**Workaround** Manually create an *App\_Data* folder for the website.</span></span> <span data-ttu-id="01652-194">Убедитесь, что учетная запись Windows, то приложение запущено под (как правило, NETWORK SERVICE) имеет разрешения на чтение и запись для корневой папки приложения и для вложенных папок, таких как приложения\_данных.</span><span class="sxs-lookup"><span data-stu-id="01652-194">Then make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as App\_Data.</span></span> <span data-ttu-id="01652-195">Более подробные сведения можно найти в статье базы знаний [проблемы с SQL Server Express пользователя при создании экземпляров и проектами веб-приложений ASP.net](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="01652-195">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="01652-196">Ошибка, проблема: «не удалось сформировать пользовательский экземпляр SQL Server»</span><span class="sxs-lookup"><span data-stu-id="01652-196">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="01652-197">Если выполняется IIS 7.5 в Windows 7 или Windows Server 2008 R2 WebMatrix веб-приложение использует SQL Server Express, может появиться сообщение о том, что SQL Server не удалось получить путь локального приложения пользователя во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="01652-197">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="01652-198">**Инструкции по решению** убедитесь, что учетная запись Windows, то приложение запущено под (как правило, NETWORK SERVICE) имеет разрешения на чтение и запись для корневой папки приложения и вложенные папки например *приложения\_данных*.</span><span class="sxs-lookup"><span data-stu-id="01652-198">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="01652-199">Более подробные сведения можно найти в статье базы знаний [проблемы с SQL Server Express пользователя при создании экземпляров и проектами веб-приложений ASP.net](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="01652-199">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a><span data-ttu-id="01652-200">Проблема: Файлы, содержащие ресурсы диспетчера пакетов или пароли диспетчера пакетов servable в службах IIS 6.0 и более ранних версий</span><span class="sxs-lookup"><span data-stu-id="01652-200">Issue: Files that contains package-manager resources or package-manager passwords are servable under IIS 6.0 and earlier</span></span>

> <span data-ttu-id="01652-201">Если выполняется развертывание приложения было создано с помощью выпуска версии-кандидата 2 ASP.NET Web Pages (Razor), и если приложение содержит *password.txt* или *packagesources.txt* файл */App\_ Данные/admin*, IIS 6.0 будет предоставлять этот файл при поступлении запроса подвергает паролей для вашего экземпляра диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="01652-201">If you deploy an ASP.NET Web Pages (Razor) application that was built using the RC2 release, and if the application contains a *password.txt* or *packagesources.txt* file under */App\_Data/admin*, IIS 6.0 will serve the file if requested, potentially exposing the passwords for your package manager instance.</span></span> 
> 
> <span data-ttu-id="01652-202">**Инструкции по решению** переименование *password.txt* или *packagesources.txt* файл *password.config* или *packagesources.config*. По умолчанию службы IIS 6.0 не обслуживает файлы, имеющие *.config* расширения.</span><span class="sxs-lookup"><span data-stu-id="01652-202">**Workaround** Rename the *password.txt* or *packagesources.txt* file to *password.config* or *packagesources.config*. By default, IIS 6.0 does not serve files that have the *.config* extension.</span></span> <span data-ttu-id="01652-203">(В IIS 7, нет файлов в *приложения\_данные* обслуживаются папки, поэтому необходимо переименовать файлы.)</span><span class="sxs-lookup"><span data-stu-id="01652-203">(In IIS 7, no files in the *App\_Data* folder are served, so you do not need to rename the files.)</span></span>


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a><span data-ttu-id="01652-204">Проблема: Удаление пакеты, установленные с помощью бета-версии 3 не полностью удаляет компоненты пакета</span><span class="sxs-lookup"><span data-stu-id="01652-204">Issue: Uninstalling packages installed using the Beta 3 release does not completely remove package components</span></span>

> <span data-ttu-id="01652-205">Если вы установили пакет с помощью диспетчера пакетов в бета-версии 3 и попытайтесь удалить его с помощью текущего выпуска, пакет не полностью удален.</span><span class="sxs-lookup"><span data-stu-id="01652-205">If you installed a package using the package manager in the Beta 3 release and then try to uninstall it using the current release, the package is not completely uninstalled.</span></span> <span data-ttu-id="01652-206">С помощью диспетчера пакетов **удаления** кнопка удаляет некоторые компоненты, но оставляет код библиотеки пакета и не обновляет *package.config* файла.</span><span class="sxs-lookup"><span data-stu-id="01652-206">Using the package manager's **Uninstall** button removes some components, but leaves the package's library code and does not update the *package.config* file.</span></span>
> 
> <span data-ttu-id="01652-207">**Инструкции по решению** </span><span class="sxs-lookup"><span data-stu-id="01652-207">**Workaround** </span></span>  
> <span data-ttu-id="01652-208">Выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="01652-208">Perform these steps:</span></span>  
> 1. <span data-ttu-id="01652-209">Удалить *приложения\_Data\packages* папки.</span><span class="sxs-lookup"><span data-stu-id="01652-209">Delete the *App\_Data\packages* folder.</span></span> <span data-ttu-id="01652-210">При этом удаляются все пакеты.</span><span class="sxs-lookup"><span data-stu-id="01652-210">This removes all packages.</span></span>   
> 2. <span data-ttu-id="01652-211">Удалить *packages.config* файл в корневом каталоге веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="01652-211">Delete the *packages.config* file in the root of the website.</span></span>


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a><span data-ttu-id="01652-212">Проблема: В Visual Studio вызова диспетчер пакетов веб-переводит приложение автономный режим</span><span class="sxs-lookup"><span data-stu-id="01652-212">Issue: In Visual Studio, invoking the web-based package manager takes the application offline</span></span>

> <span data-ttu-id="01652-213">Если вы работаете в Visual Studio (не WebMatrix) и использовать * \_администратора* функциональные возможности, чтобы запустить диспетчер пакетов Visual Studio переводит приложение в автономный режим и отправляет *приложения\_ OFFLINE.htm* в корневой папке веб-сайта, что прерывает возможность использовать диспетчер пакетов.</span><span class="sxs-lookup"><span data-stu-id="01652-213">If you are working in Visual Studio (not WebMatrix) and use the *\_admin* functionality to start the package manager, Visual Studio takes the application offline and posts the *app\_offline.htm* into the website root, which disrupts your ability to use the package manager.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="01652-214">Несмотря на то, что такое поведение обычно появляются при с помощью интерфейса диспетчера веб-пакета, то же поведение возникает, если добавить, удалить или изменить любые файлы в *приложения\_данные* папки.</span><span class="sxs-lookup"><span data-stu-id="01652-214">Although you would most typically see this behavior when using the web-based package manager interface, the same behavior occurs if you add, remove, or modify any files in the *App\_Data* folder.</span></span>
> 
> <span data-ttu-id="01652-215">**Инструкции по решению** </span><span class="sxs-lookup"><span data-stu-id="01652-215">**Workaround** </span></span>  
> <span data-ttu-id="01652-216">Для работы с пакетами в среде Visual Studio, воспользуйтесь расширение NuGet вместо веб-пакета.</span><span class="sxs-lookup"><span data-stu-id="01652-216">To work with packages in Visual Studio, use the NuGet extension instead of the web-based package manager.</span></span> <span data-ttu-id="01652-217">Сведения см. в разделе [документации по NuGet](https://docs.microsoft.com/nuget/).</span><span class="sxs-lookup"><span data-stu-id="01652-217">For information, see the [NuGet documentation](https://docs.microsoft.com/nuget/).</span></span> <span data-ttu-id="01652-218">При работе с другими файлами в *приложения\_данные* папки, рекомендуется использовать файлы в другом месте, чтобы избежать этой проблемы.</span><span class="sxs-lookup"><span data-stu-id="01652-218">If you are working with other files in the *App\_Data* folder, consider keeping the files elsewhere to avoid this issue.</span></span> <span data-ttu-id="01652-219">Если это невозможно, удалите *приложения\_offline.htm* файл вручную, или подождать, пока узел восстанавливает подключение автоматически (по умолчанию через 30 секунд).</span><span class="sxs-lookup"><span data-stu-id="01652-219">If that's not practical, delete the *app\_offline.htm* file manually or wait until the site comes back online automatically (by default, after 30 seconds).</span></span>


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="01652-220">Проблема: Visual Studio IntelliSense и шаблоны проектов доступны только в ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="01652-220">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="01652-221">Установка веб-страниц ASP.NET не также устанавливает средства для Visual Studio как IntelliSense и шаблоны проектов для приложений веб-страниц ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="01652-221">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="01652-222">**Инструкции по решению** для использования IntelliSense и шаблоны проектов для приложений веб-страниц ASP.NET в Visual Studio, установить ASP.NET MVC 3, версия-Кандидат либо с помощью установщика веб-платформы или [автономного установщика](https://go.microsoft.com/fwlink/?LinkID=191797).</span><span class="sxs-lookup"><span data-stu-id="01652-222">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="01652-223">Проблема: Чтение веб-каналов или другие внешние источники данных через прокси-сервер</span><span class="sxs-lookup"><span data-stu-id="01652-223">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="01652-224">Если на сервере сайта находится за прокси-сервер, может потребоваться настроить сведения о прокси-сервера в *web.config* файл, чтобы иметь возможность считывать данные, поступающие от вне локального узла.</span><span class="sxs-lookup"><span data-stu-id="01652-224">If the server running the site is behind a proxy server, you might need to configure proxy information in the *web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="01652-225">Например, если вы используете `ReCaptcha` вспомогательный, вспомогательное приложение взаимодействует со службой reCAPTCHA, но может быть заблокирован прокси-сервером.</span><span class="sxs-lookup"><span data-stu-id="01652-225">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="01652-226">Аналогично веб-каналов, используемые веб-страницах ASP.NET, такие как веб-канал, используемый диспетчер пакетов, может потребоваться настройка прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="01652-226">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="01652-227">Если возникли проблемы в работе с внешней службы или при работе с пакетом, в веб-канала, помещаются следующие элементы в корневого каталога приложения *web.config* файла:</span><span class="sxs-lookup"><span data-stu-id="01652-227">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *web.config* file:</span></span>
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> <span data-ttu-id="01652-228">Дополнительные сведения о настройке прокси-сервера см. в разделе [ &lt;прокси&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) на сайте MSDN.</span><span class="sxs-lookup"><span data-stu-id="01652-228">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on the MSDN Web site.</span></span>


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="01652-229">Проблема: Удаление .NET Framework версии 4 отключает веб-страниц ASP.NET с синтаксисом Razor</span><span class="sxs-lookup"><span data-stu-id="01652-229">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="01652-230">Если удалить .NET Framework версии 4, а затем переустановите его, веб-страниц ASP.NET с синтаксисом Razor отключена.</span><span class="sxs-lookup"><span data-stu-id="01652-230">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="01652-231">Страницы с *.cshtml* расширения работают правильно.</span><span class="sxs-lookup"><span data-stu-id="01652-231">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="01652-232">Веб-страницы ASP.NET регистрирует сборку в корневой папке машины *web.config* файла и удаление .NET Framework удаляет этот файл.</span><span class="sxs-lookup"><span data-stu-id="01652-232">ASP.NET Web Pages registers an assembly in the machine root *web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="01652-233">Повторная установка .NET Framework устанавливает новую версию файла конфигурации, но не добавляет ссылку для сборки веб-страниц ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="01652-233">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="01652-234">**Инструкции по решению** после повторной установки .NET Framework, переустановите ASP.NET Web Pages с синтаксисом Razor.</span><span class="sxs-lookup"><span data-stu-id="01652-234">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="01652-235">При этом добавляется следующий элемент для *web.config* файл в корне машины, которое обычно находится в следующем расположении:</span><span class="sxs-lookup"><span data-stu-id="01652-235">This adds the following element to the *web.config* file in the machine root, which is typically in the following location:</span></span>  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="01652-236">Проблема: Без расширений URL-адреса не найдены файлы.cshtml/.vbhtml в IIS 7 или IIS 7.5</span><span class="sxs-lookup"><span data-stu-id="01652-236">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="01652-237">В IIS 7 или IIS 7.5, запросы с URL-адреса следующего вида не удается найти страниц, имеющих *.cshtml* или *.vbhtml* расширения:</span><span class="sxs-lookup"><span data-stu-id="01652-237">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> <span data-ttu-id="01652-238">Проблема возникает из-за перезаписи URL-адресов не включена по умолчанию для IIS 7 или IIS 7.5.</span><span class="sxs-lookup"><span data-stu-id="01652-238">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="01652-239">Возможная сценарий существует, вы не видите проблемы при тестировании локально с помощью IIS Express, но возникают при развертывании веб-сайта для размещения веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="01652-239">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="01652-240">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="01652-240">**Workaround**</span></span>
> 
> - <span data-ttu-id="01652-241">Если у вас есть контроль над компьютером сервера, на компьютере сервера установите обновление, описанное в [обновление доступно, что позволяет определенным обработчикам служб IIS 7.0 или IIS 7.5 обрабатывать запросы, URL-адреса не заканчиваться точкой](https://support.microsoft.com/kb/980368).</span><span class="sxs-lookup"><span data-stu-id="01652-241">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="01652-242">Если у вас по управлению компьютером сервера (например, развертывается для размещения веб-сайта), добавьте следующий на веб-сайт *web.config* файла:</span><span class="sxs-lookup"><span data-stu-id="01652-242">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *web.config* file:</span></span> 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="01652-243">Проблема: Развертывание приложения на компьютере, не поддерживает SQL Server Compact установлена</span><span class="sxs-lookup"><span data-stu-id="01652-243">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="01652-244">Приложения, включающие баз данных SQL Server Compact могут выполняться на компьютере, где SQL Server Compact не установлена.</span><span class="sxs-lookup"><span data-stu-id="01652-244">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="01652-245">Microsoft WebMatrix 1.0 автоматически копирует эти двоичные файлы для вас и выполняет соответствующее *web.config* файла преобразования.</span><span class="sxs-lookup"><span data-stu-id="01652-245">Microsoft WebMatrix 1.0 automatically copies these binaries for you and performs the appropriate *web.config* file transforms.</span></span>
> 
> <span data-ttu-id="01652-246">**Инструкции по решению** Если вам нужно скопировать эти файлы и сделать *web.config* изменения файлов вручную, выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="01652-246">**Workaround** If you need to copy these files and make the *web.config* file changes manually, do the following:</span></span>
> 
> 1. <span data-ttu-id="01652-247">Скопируйте сборки ядра базы данных для *Bin* папку (и вложенные папки), приложения на целевом компьютере:</span><span class="sxs-lookup"><span data-stu-id="01652-247">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span>  
> 
>    - <span data-ttu-id="01652-248">Copy *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* </span><span class="sxs-lookup"><span data-stu-id="01652-248">Copy *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* </span></span>  
>        <span data-ttu-id="01652-249">**to** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="01652-249">**to** *\Bin*</span></span>
>    - <span data-ttu-id="01652-250">Копировать <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>для</em></strong>\Bin\x86\*</span><span class="sxs-lookup"><span data-stu-id="01652-250">Copy <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>to</em></strong>\Bin\x86\*</span></span>
>    - <span data-ttu-id="01652-251">Копировать <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>\* <strong>для</strong><em>\Bin\amd64</em></span><span class="sxs-lookup"><span data-stu-id="01652-251">Copy <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>\* <strong>to</strong><em>\Bin\amd64</em></span></span>
> 
> 2. <span data-ttu-id="01652-252">В корневой папке веб-сайта, создайте или откройте *web.config* файла.</span><span class="sxs-lookup"><span data-stu-id="01652-252">In the root folder of the website, create or open a *web.config* file.</span></span> <span data-ttu-id="01652-253">(В версии 1.0 WebMatrix этот тип файлов доступна при нажатии кнопки **все** в **выберите тип файла** диалоговое окно.)</span><span class="sxs-lookup"><span data-stu-id="01652-253">(In WebMatrix 1.0, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="01652-254">Добавьте следующий элемент в качестве дочернего элемента `<configuration>` элемента (не внутри `<system.web>` элемент):</span><span class="sxs-lookup"><span data-stu-id="01652-254">Add the following element as a child of the `<configuration>` element (not inside the `<system.web>` element):</span></span>
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="01652-255">Проблема: «Базы данных» и «WebGrid» вспомогательные методы не работают со средним уровнем доверия в Visual Basic</span><span class="sxs-lookup"><span data-stu-id="01652-255">Issue: "Database" and "WebGrid" helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="01652-256">Если вы используете Visual Basic (создание *.vbhtml* файлы), `Database` и `WebGrid` вспомогательные методы не будет работать, если приложения настроен для использования со средним уровнем доверия.</span><span class="sxs-lookup"><span data-stu-id="01652-256">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="01652-257">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="01652-257">**Workaround**</span></span>  
> <span data-ttu-id="01652-258">При использовании Visual Studio 2010, можно разрешить эту проблему путем установки выпуска пакета обновления 1.</span><span class="sxs-lookup"><span data-stu-id="01652-258">If you use Visual Studio 2010, you can resolve this problem by installing the Service Pack 1 release.</span></span> <span data-ttu-id="01652-259">Пока не станет доступен окончательной версии выпуска пакета обновления 1, можно загрузить бета-версии с пакетом обновления 1 из [Microsoft Visual Studio 2010 с пакетом обновления 1 бета-версию обновления](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) страницы в центре загрузки Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="01652-259">Until the final version of the SP1 release is available, you can download the Beta version of SP1 from the [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) page on the Microsoft Download Center.</span></span>   
>   
> <span data-ttu-id="01652-260">Если это нецелесообразно, или если вы не используете Visual Studio 2010, можно временно задать приложение для использования полного доверия.</span><span class="sxs-lookup"><span data-stu-id="01652-260">If this is not practical, or if you do not use Visual Studio 2010, you can temporarily set the application to use Full Trust.</span></span>


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a><span data-ttu-id="01652-261">Проблема: «ApplicationPart» ресурсы, доступны извне</span><span class="sxs-lookup"><span data-stu-id="01652-261">Issue: "ApplicationPart" resources are externally accessible</span></span>

> <span data-ttu-id="01652-262">Если сборка содержит объекты, является производным от `ApplicationPart` класса ресурсы сборки предоставляемых `ResourceRouteHandler` класса.</span><span class="sxs-lookup"><span data-stu-id="01652-262">If an assembly contains objects that derives from the `ApplicationPart` class, that assembly's resources are exposed by the `ResourceRouteHandler` class.</span></span> <span data-ttu-id="01652-263">Например, рассмотрим следующий URL-адрес:</span><span class="sxs-lookup"><span data-stu-id="01652-263">For example, consider the following URL:</span></span>  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> <span data-ttu-id="01652-264">Этот запрос загружает все строки ресурса в *System.Web.WebPages.Administration.dll* сборки.</span><span class="sxs-lookup"><span data-stu-id="01652-264">This request downloads all of the resource strings in the *System.Web.WebPages.Administration.dll* assembly.</span></span> <span data-ttu-id="01652-265">Загружаются все внедренные ресурсы (даже те, которые не предназначены для обслуживаемых как статическое содержимое).</span><span class="sxs-lookup"><span data-stu-id="01652-265">All of the embedded resources (even those that are not intended to be served as static content) are downloaded.</span></span> <span data-ttu-id="01652-266">Если внедренные ресурсы содержат конфиденциальную информацию, это может представлять угрозу безопасности.</span><span class="sxs-lookup"><span data-stu-id="01652-266">If the embedded resources contain sensitive information, this can represent a security risk.</span></span> 
> 
> <span data-ttu-id="01652-267">**Инструкции по решению** </span><span class="sxs-lookup"><span data-stu-id="01652-267">**Workaround** </span></span>  
> <span data-ttu-id="01652-268">Если вы создаете **ApplicationPart** объектов, убедитесь, что внедренные ресурсы, связанные с **ApplicationPart** сборки объекта не содержат конфиденциальной информации.</span><span class="sxs-lookup"><span data-stu-id="01652-268">If you create an **ApplicationPart** object, make sure that the embedded resources associated with that **ApplicationPart** object's assembly do not contain sensitive information.</span></span>


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a><span data-ttu-id="01652-269">WebMatrix</span><span class="sxs-lookup"><span data-stu-id="01652-269">WebMatrix</span></span>

> [!NOTE]
> <span data-ttu-id="01652-270">Сведения о проблемах при установке для WebMatrix содержатся [проблем установки WebMatrix](#Known_Issues_Installation) ранее в этом документе.</span><span class="sxs-lookup"><span data-stu-id="01652-270">For information about installation issues for WebMatrix, see [WebMatrix Installation Issues](#Known_Issues_Installation) earlier in this document.</span></span>


<span data-ttu-id="01652-271">Документ в этом разделе описаны известные проблемы для среды разработки WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="01652-271">This section of the document describes known issues for the WebMatrix development environment.</span></span>

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a><span data-ttu-id="01652-272">Проблема: Изменения в имени пользователя или пароля в строку подключения базы данных в файле web.config не отражаются в рабочей области баз данных</span><span class="sxs-lookup"><span data-stu-id="01652-272">Issue: Changes in the username or password of a database connection string in a web.config file are not reflected in the Databases workspace</span></span>

> <span data-ttu-id="01652-273">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="01652-273">**Workaround**</span></span>  
> 
> 1. <span data-ttu-id="01652-274">В *web.config* файл, измените имя базы данных в строке подключения (например, добавьте «1» на него).</span><span class="sxs-lookup"><span data-stu-id="01652-274">In the *web.config* file, change the database name in the connection string (for example, add "1" to it).</span></span>
> 2. <span data-ttu-id="01652-275">Сохранить *web.config* файла.</span><span class="sxs-lookup"><span data-stu-id="01652-275">Save the *web.config* file.</span></span>
> 3. <span data-ttu-id="01652-276">Нажмите кнопку **баз данных** и обновления.</span><span class="sxs-lookup"><span data-stu-id="01652-276">Click **Databases** and refresh.</span></span>
> 4. <span data-ttu-id="01652-277">Изменить имя базы данных в строке подключения в *web.config* файл обратно исходное имя базы данных.</span><span class="sxs-lookup"><span data-stu-id="01652-277">Change the database name in the connection string in the *web.config* file back to the original database name.</span></span>
> 5. <span data-ttu-id="01652-278">Сохранить *web.config* файла.</span><span class="sxs-lookup"><span data-stu-id="01652-278">Save the *web.config* file.</span></span>
> 6. <span data-ttu-id="01652-279">Нажмите кнопку **баз данных** и обновления.</span><span class="sxs-lookup"><span data-stu-id="01652-279">Click **Databases** and refresh.</span></span>


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a><span data-ttu-id="01652-280">Проблема: Не удается удалить папки, созданные WebMatrix</span><span class="sxs-lookup"><span data-stu-id="01652-280">Issue: Folders created by WebMatrix cannot be deleted</span></span>

> <span data-ttu-id="01652-281">Если запущен с повышенными правами, WebMatrix (то есть запущен с помощью WebMatrix **Запуск от имени администратора** в Windows), нельзя удалять папки, созданные с WebMatrix с помощью проводника Windows.</span><span class="sxs-lookup"><span data-stu-id="01652-281">If WebMatrix is running using elevated permissions (that is, you started WebMatrix using the **Run as Administrator** option in Windows), folders that are created by WebMatrix cannot be deleted using Windows Explorer.</span></span>
> 
> <span data-ttu-id="01652-282">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="01652-282">**Workaround**</span></span>  
> <span data-ttu-id="01652-283">Запустите проводник, используя повышенные разрешения.</span><span class="sxs-lookup"><span data-stu-id="01652-283">Run Windows Explorer using elevated permissions.</span></span> <span data-ttu-id="01652-284">Выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="01652-284">Follow these steps:</span></span>  
> 
> 1. <span data-ttu-id="01652-285">В Windows, нажмите кнопку **запустить**.</span><span class="sxs-lookup"><span data-stu-id="01652-285">In Windows, click **Start**.</span></span>
> 2. <span data-ttu-id="01652-286">Введите «Проводник» и щелкните правой кнопкой мыши запись для **Проводник**.</span><span class="sxs-lookup"><span data-stu-id="01652-286">Enter "Windows Explorer" and right-click the entry for **Windows Explorer**.</span></span>
> 3. <span data-ttu-id="01652-287">Нажмите кнопку **Запуск от имени администратора**.</span><span class="sxs-lookup"><span data-stu-id="01652-287">Click **Run as Administrator**.</span></span> <span data-ttu-id="01652-288">Папки можно удалить.</span><span class="sxs-lookup"><span data-stu-id="01652-288">You can then delete the folders.</span></span>


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="01652-289">Проблема: Не удалось выполнить определенные задачи, требующие повышения прав WebMatrix 1.0</span><span class="sxs-lookup"><span data-stu-id="01652-289">Issue: WebMatrix 1.0 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="01652-290">WebMatrix 1.0 не может выполнять определенные задачи, требующие повышения прав, таких как установка дополнительных компонентов в следующих ситуациях:</span><span class="sxs-lookup"><span data-stu-id="01652-290">WebMatrix 1.0 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="01652-291">В Windows Vista или Windows 7 вы вошли под учетной записью, у которого нет прав администратора, и управления учетных записей (UAC) отключено.</span><span class="sxs-lookup"><span data-stu-id="01652-291">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="01652-292">Вы используете Microsoft Windows XP или Microsoft Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="01652-292">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="01652-293">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="01652-293">**Workaround**</span></span>  
> <span data-ttu-id="01652-294">Большинство задач в WebMatrix 1.0 необходимы разрешения администратора.</span><span class="sxs-lookup"><span data-stu-id="01652-294">Most tasks in WebMatrix 1.0 do not require administrative permission.</span></span> <span data-ttu-id="01652-295">Для тех, которые можно выполнять операции от имени администратора или выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="01652-295">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="01652-296">В Windows Vista или Windows 7 включите контроль учетных Записей.</span><span class="sxs-lookup"><span data-stu-id="01652-296">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="01652-297">В Windows XP добавьте пользователя в группу безопасности "Администраторы".</span><span class="sxs-lookup"><span data-stu-id="01652-297">On Windows XP, add the user to the Administrators security group.</span></span>


#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="01652-298">Проблема: Отключен «Из галерею веб-узла»</span><span class="sxs-lookup"><span data-stu-id="01652-298">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="01652-299">**Сайта из веб-коллекции** параметр отключен, если не установлен установщик веб-платформы 3.0.</span><span class="sxs-lookup"><span data-stu-id="01652-299">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="01652-300">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="01652-300">**Workaround**</span></span>  
> <span data-ttu-id="01652-301">Установка [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="01652-301">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="01652-302">Проблема: Google Chrome не доступен в качестве параметра запуска</span><span class="sxs-lookup"><span data-stu-id="01652-302">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="01652-303">Google Chrome не отображается в списке браузеров в **запуска** на **Главная** вкладки.</span><span class="sxs-lookup"><span data-stu-id="01652-303">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="01652-304">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="01652-304">**Workaround**</span></span>  
> <span data-ttu-id="01652-305">В некоторых версиях Google Chrome не регистрируют себя правильно с функцией программ по умолчанию в Windows.</span><span class="sxs-lookup"><span data-stu-id="01652-305">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="01652-306">Чтобы избежать этого, Google Chrome, выберите пункт *Настройка и управление Google Chrome* меню, нажмите кнопку *параметры*и нажмите кнопку *марка Google Chrome обозреватель по умолчанию*.</span><span class="sxs-lookup"><span data-stu-id="01652-306">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="01652-307">Проблема: Диалоговое окно «Foreign Key» не допускает ввод первичного ключа</span><span class="sxs-lookup"><span data-stu-id="01652-307">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="01652-308">**Foreign Key** диалоговое окно не позволяет ввести имя первичного ключа из таблицы первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="01652-308">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="01652-309">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="01652-309">**Workaround**</span></span>  
> <span data-ttu-id="01652-310">Это сделано намеренно.</span><span class="sxs-lookup"><span data-stu-id="01652-310">This is intentional.</span></span> <span data-ttu-id="01652-311">Необходимо ввести имя первичного ключа из таблицы первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="01652-311">You do not need to enter the name of the primary key from the primary key table.</span></span>


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a><span data-ttu-id="01652-312">Проблема: Технология IntelliSense не доступна в WebMatrix, Razor синтаксис, C# или Visual Basic</span><span class="sxs-lookup"><span data-stu-id="01652-312">Issue: IntelliSense is not available in WebMatrix for Razor syntax, C#, or Visual Basic</span></span>

> <span data-ttu-id="01652-313">Технология IntelliSense поддерживается в WebMatrix для HTML и CSS.</span><span class="sxs-lookup"><span data-stu-id="01652-313">IntelliSense is supported in WebMatrix for HTML and CSS.</span></span> <span data-ttu-id="01652-314">Тем не менее не доступны для других языков.</span><span class="sxs-lookup"><span data-stu-id="01652-314">However, it is not available for other languages.</span></span> 
> 
> <span data-ttu-id="01652-315">**Инструкции по решению** </span><span class="sxs-lookup"><span data-stu-id="01652-315">**Workaround** </span></span>  
> <span data-ttu-id="01652-316">Отсутствует.</span><span class="sxs-lookup"><span data-stu-id="01652-316">None.</span></span>


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a><span data-ttu-id="01652-317">Проблема: IntelliSense для HTML и CSS предлагает элементы, которые не подходят зависимости от контекста</span><span class="sxs-lookup"><span data-stu-id="01652-317">Issue: IntelliSense for HTML and CSS suggests elements that are not contextually appropriate</span></span>

> <span data-ttu-id="01652-318">IntelliSense для разметки в WebMatrix поддерживает HTML с помощью [схема XHTML 1.0 Transitional](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) и CSS с помощью [схемы CSS 2.1](http://www.w3.org/TR/CSS2/).</span><span class="sxs-lookup"><span data-stu-id="01652-318">IntelliSense for markup in WebMatrix supports HTML using the [XHTML 1.0 Transitional schema](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) and CSS using the [CSS 2.1 schema](http://www.w3.org/TR/CSS2/).</span></span> <span data-ttu-id="01652-319">Так как IntelliSense основана на эти конкретные схемы, определенные теги, атрибутов или свойств может предложенные, еще не подходит для определения текущего страницы или стиль.</span><span class="sxs-lookup"><span data-stu-id="01652-319">Because IntelliSense is based on these specific schemas, certain tags, attributes, or properties might be suggested that are not appropriate for the current page or style definition.</span></span> <span data-ttu-id="01652-320">Для HTML он может также привести к неожиданным предложения в содержимом, которое может быть интерпретировано как неправильного формата XHTML (например, если теги не закрыты).</span><span class="sxs-lookup"><span data-stu-id="01652-320">For HTML, it can also lead to unexpected suggestions in content that might be interpreted as malformed XHTML (for example, when tags are not closed).</span></span> <span data-ttu-id="01652-321">Эта проблема может быть более наглядными, если позиция ввода находится внутри тега неполный; в этом случае IntelliSense может предложить новые открывающие теги или другие неверные оптимизации.</span><span class="sxs-lookup"><span data-stu-id="01652-321">This issue might be more noticeable if the insertion point is inside an incomplete tag; in that case, IntelliSense might suggest new opening tags or offer other incorrect suggestions.</span></span> 
> 
> <span data-ttu-id="01652-322">**Инструкции по решению** </span><span class="sxs-lookup"><span data-stu-id="01652-322">**Workaround** </span></span>  
> <span data-ttu-id="01652-323">Для HTML убедитесь, что вы работаете с правильным форматом, завершенной страниц XHTML.</span><span class="sxs-lookup"><span data-stu-id="01652-323">For HTML, make sure that you are working within a well-formed, complete XHTML page.</span></span> <span data-ttu-id="01652-324">Для CSS решение отсутствует.</span><span class="sxs-lookup"><span data-stu-id="01652-324">For CSS, there is no workaround.</span></span>


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a><span data-ttu-id="01652-325">Проблема: IntelliSense не вызывается во время ввода</span><span class="sxs-lookup"><span data-stu-id="01652-325">Issue: IntelliSense is not invoked while you type</span></span>

> <span data-ttu-id="01652-326">В некоторых случаях IntelliSense не может быть вызван, как HTML и CSS, вводимых в редакторе.</span><span class="sxs-lookup"><span data-stu-id="01652-326">At times, IntelliSense might not be invoked as HTML or CSS is being entered in the editor.</span></span> <span data-ttu-id="01652-327">В частности это может произойти, когда курсор находится непосредственно рядом с другим элементом, или в конце файла.</span><span class="sxs-lookup"><span data-stu-id="01652-327">In particular, this might happen when the insertion point is directly next to another element or at the end of a file.</span></span> 
> 
> <span data-ttu-id="01652-328">**Инструкции по решению** </span><span class="sxs-lookup"><span data-stu-id="01652-328">**Workaround** </span></span>  
> <span data-ttu-id="01652-329">Убедитесь, что имеется вокруг курсора и курсор не в конце файла.</span><span class="sxs-lookup"><span data-stu-id="01652-329">Make sure that there is whitespace around the insertion point and that the insertion point is not at the end of a file.</span></span> <span data-ttu-id="01652-330">IntelliSense также можно вызвать вручную, нажав клавиши Ctrl + Пробел.</span><span class="sxs-lookup"><span data-stu-id="01652-330">You can also invoke IntelliSense manually by pressing Ctrl+Space.</span></span>


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a><span data-ttu-id="01652-331">Проблема: Пользовательский Интерфейс не доступен для отключения IntelliSense</span><span class="sxs-lookup"><span data-stu-id="01652-331">Issue: No UI is available for disabling IntelliSense</span></span>

> <span data-ttu-id="01652-332">WebMatrix 1.0 позволяет без пользовательского интерфейса или жеста отключать IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="01652-332">WebMatrix 1.0 provides no UI or gesture for disabling IntelliSense.</span></span> 
> 
> <span data-ttu-id="01652-333">**Инструкции по решению** </span><span class="sxs-lookup"><span data-stu-id="01652-333">**Workaround** </span></span>  
> <span data-ttu-id="01652-334">Запустите WebMatrix, выполните следующую команду, которая включает параметр, который отключает IntelliSense:</span><span class="sxs-lookup"><span data-stu-id="01652-334">Start WebMatrix using the following command, which includes a switch that disables IntelliSense:</span></span>  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a><span data-ttu-id="01652-335">IIS Express</span><span class="sxs-lookup"><span data-stu-id="01652-335">IIS Express</span></span>

<span data-ttu-id="01652-336">IIS Express имеет свой собственный файл readme, который доступен по следующему АДРЕСУ:</span><span class="sxs-lookup"><span data-stu-id="01652-336">IIS Express has its own readme file, which is available at the following URL:</span></span>

[<span data-ttu-id="01652-337">https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409</span><span class="sxs-lookup"><span data-stu-id="01652-337">https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409</span></span>](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a><span data-ttu-id="01652-338">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="01652-338">SQL Server Compact</span></span>

<span data-ttu-id="01652-339">SQL Server Compact имеет свой собственный файл readme, который доступен по следующему АДРЕСУ:</span><span class="sxs-lookup"><span data-stu-id="01652-339">SQL Server Compact has its own readme file, which is available at the following URL:</span></span>

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

<span data-ttu-id="01652-340">Сведения о проблемы, связанные с установкой SQL Server Compact в составе WebMatrix разделе [проблем установки WebMatrix](#Known_Issues_Installation) ранее в этом документе.</span><span class="sxs-lookup"><span data-stu-id="01652-340">For information about issues that involve installing SQL Server Compact as part of WebMatrix, see [WebMatrix Installation Issues](#Known_Issues_Installation) earlier in this document.</span></span>

### <a id="Known_Issues_Installing_Applications"></a>  <span data-ttu-id="01652-341">Установка приложений</span><span class="sxs-lookup"><span data-stu-id="01652-341">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="01652-342">Проблема: При установке приложения может занять много времени при пользовательской папке "Мои документы" перенаправляется в общую сетевую папку</span><span class="sxs-lookup"><span data-stu-id="01652-342">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="01652-343">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="01652-343">**Workaround**</span></span>  
> <span data-ttu-id="01652-344">Отсутствует.</span><span class="sxs-lookup"><span data-stu-id="01652-344">None.</span></span> <span data-ttu-id="01652-345">Приложение может занять некоторое время, чтобы установить, но устанавливается правильно.</span><span class="sxs-lookup"><span data-stu-id="01652-345">The application might take a while to install, but will install correctly.</span></span>


### <a id="Known_Issues_Publishing_Applications"></a>  <span data-ttu-id="01652-346">Публикация приложений</span><span class="sxs-lookup"><span data-stu-id="01652-346">Publishing Applications</span></span>

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a><span data-ttu-id="01652-347">Проблема: «требуемые разрешения не могут быть получены» ошибка при публикации базы данных SQL Compact</span><span class="sxs-lookup"><span data-stu-id="01652-347">Issue: "Required permissions cannot be acquired" error when publishing a SQL Compact Database</span></span>

> <span data-ttu-id="01652-348">WebMatrix не полностью поддерживает развертывание вспомогательные двоичные файлы для SQL Server Compact на сервере под управлением .NET Framework версии 3.5 с конфигурацией среднего уровня доверия.</span><span class="sxs-lookup"><span data-stu-id="01652-348">WebMatrix does not fully support deploying supporting binaries for SQL Server Compact to a server that is running .NET Framework version 3.5 with a medium trust configuration.</span></span>
> 
> <span data-ttu-id="01652-349">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="01652-349">**Workaround**</span></span>  
> <span data-ttu-id="01652-350">Предпочтительный обходным путем является установка .NET Framework 4 на сервере.</span><span class="sxs-lookup"><span data-stu-id="01652-350">The preferred workaround is to install the .NET Framework 4 on the server.</span></span> <span data-ttu-id="01652-351">Кроме того выполните следующее:</span><span class="sxs-lookup"><span data-stu-id="01652-351">Alternatively, do the following:</span></span>
> 
> 1. <span data-ttu-id="01652-352">Добавьте следующие элементы для `SecurityClasses` статьи *Web\_MediumTrust.config* файла:</span><span class="sxs-lookup"><span data-stu-id="01652-352">Add the following elements to the `SecurityClasses` section in *Web\_MediumTrust.config* file:</span></span>
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. <span data-ttu-id="01652-353">Создать новый набор разрешений *Web\_MediumTrust.config* файл следующие необходимые разрешения:</span><span class="sxs-lookup"><span data-stu-id="01652-353">Create a new permission set in the *Web\_MediumTrust.config* file with the following required permissions:</span></span>
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. <span data-ttu-id="01652-354">Применить набор разрешений для SQL Server Compact, помещая следующие элементы *Web\_MediumTrust.config* файла:</span><span class="sxs-lookup"><span data-stu-id="01652-354">Apply the permission set to SQL Server Compact by putting the following elements in the *Web\_MediumTrust.config* file:</span></span>
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a><span data-ttu-id="01652-355">Проблема: Веб-приложения в галерее и PhpBB отображает ошибку «Служба недоступна» после публикации</span><span class="sxs-lookup"><span data-stu-id="01652-355">Issue: Gallery and PhpBB web applications display a "Service is unavailable" error after publishing</span></span>

> <span data-ttu-id="01652-356">В некоторых случаях публикации приложения приведет к ошибке «служба недоступна».</span><span class="sxs-lookup"><span data-stu-id="01652-356">Under some circumstances, publishing an application causes a "service is unavailable" error.</span></span>
> 
> <span data-ttu-id="01652-357">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="01652-357">**Workaround**</span></span>  
> <span data-ttu-id="01652-358">В WebMatrix, добавьте обратную косую черту (\) в конец имени сервера в **параметры публикации** окна, а затем опубликовать приложение еще раз.</span><span class="sxs-lookup"><span data-stu-id="01652-358">In WebMatrix, add a backslash (\) to the end of the server name in the **Publish Settings** window and then publish the application again.</span></span>


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a><span data-ttu-id="01652-359">Проблема: Moodle макет веб-сайта и ссылки перестают работать после публикации</span><span class="sxs-lookup"><span data-stu-id="01652-359">Issue: Moodle website layout and links are broken after publishing</span></span>

> <span data-ttu-id="01652-360">После публикации приложения Moodle, приложение будет работать неправильно.</span><span class="sxs-lookup"><span data-stu-id="01652-360">After you publish a Moodle application, the application does not work correctly.</span></span>
> 
> <span data-ttu-id="01652-361">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="01652-361">**Workaround**</span></span>  
> <span data-ttu-id="01652-362">В WebMatrix, добавьте косую черту (/) в конец **имя сайта** в **параметры публикации** окна, а затем опубликовать приложение еще раз.</span><span class="sxs-lookup"><span data-stu-id="01652-362">In WebMatrix, add a slash (/) to the end of the **Site Name** field in the **Publish Settings** window and then publish the application again.</span></span>


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a><span data-ttu-id="01652-363">Проблема: Публикация nopCommerce приведет к ошибке базы данных</span><span class="sxs-lookup"><span data-stu-id="01652-363">Issue: Publishing nopCommerce fails with a database error</span></span>

> <span data-ttu-id="01652-364">Публикации nopCommerce завершается ошибкой и сообщает об ошибке базы данных как «вставьте nop\_таблицы журнала не удалось.»</span><span class="sxs-lookup"><span data-stu-id="01652-364">Publishing nopCommerce fails and reports a database error like "Insert into the nop\_log table failed."</span></span>
> 
> <span data-ttu-id="01652-365">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="01652-365">**Workaround**</span></span>  
> 
> 1. <span data-ttu-id="01652-366">В WebMatrix, щелкните **запуска** для запуска nopCommerce локально.</span><span class="sxs-lookup"><span data-stu-id="01652-366">In WebMatrix, click **Run** to launch nopCommerce locally.</span></span>
> 2. <span data-ttu-id="01652-367">Войдите на странице администрирования.</span><span class="sxs-lookup"><span data-stu-id="01652-367">Log into the administration page.</span></span>
> 3. <span data-ttu-id="01652-368">Нажмите кнопку **системы** меню.</span><span class="sxs-lookup"><span data-stu-id="01652-368">Click the **System** menu.</span></span>
> 4. <span data-ttu-id="01652-369">Нажмите кнопку **журнала** параметр.</span><span class="sxs-lookup"><span data-stu-id="01652-369">Click the **Log** option.</span></span>
> 5. <span data-ttu-id="01652-370">Нажмите кнопку **очистить журнал** кнопки.</span><span class="sxs-lookup"><span data-stu-id="01652-370">Click the **Clear Log** button.</span></span>
> 6. <span data-ttu-id="01652-371">Опубликуйте nopCommerce еще раз.</span><span class="sxs-lookup"><span data-stu-id="01652-371">Publish nopCommerce again.</span></span>


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a><span data-ttu-id="01652-372">Проблема: Silverstripe CMS отображается «HTTP 500 PHP FCGI ошибка» при загрузке опубликованного узла</span><span class="sxs-lookup"><span data-stu-id="01652-372">Issue: Silverstripe CMS displays a "HTTP 500 PHP FCGI Error" when you download a published site</span></span>

> <span data-ttu-id="01652-373">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="01652-373">**Workaround**</span></span>  
> <span data-ttu-id="01652-374">После нажатия кнопки **загрузки публикации узла**, пропустить `silverstripe-cache/manifest_main` в **Просмотр публикации**.</span><span class="sxs-lookup"><span data-stu-id="01652-374">After you click **Download published site**, skip `silverstripe-cache/manifest_main` in **Publish Preview**.</span></span> <span data-ttu-id="01652-375">Этот файл используется для кэширования и относится только к каждому компьютеру.</span><span class="sxs-lookup"><span data-stu-id="01652-375">This file is used for caching purposes and is specific to each computer.</span></span>


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a><span data-ttu-id="01652-376">Проблема: Subtext отображает «Ошибка сервера в приложении '/'» при загрузке опубликованного узла</span><span class="sxs-lookup"><span data-stu-id="01652-376">Issue: Subtext displays "Server Error in '/' Application" when you download a published site</span></span>

> <span data-ttu-id="01652-377">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="01652-377">**Workaround**</span></span>  
> <span data-ttu-id="01652-378">Откройте сайт *web.config* и замените идентификатор пользователя и пароль в строке подключения базы данных с учетными данными администратора SQL Server («sa» учетные данные).</span><span class="sxs-lookup"><span data-stu-id="01652-378">Open the site's *web.config* file and replace the user ID and password in the database connection string with the SQL Server administrator credentials (the "sa" credentials).</span></span>
> 
> <span data-ttu-id="01652-379">Кроме того, выполните следующие действия, чтобы предоставить учетной записи пользователя, вы вошли в систему `db_owner` разрешения:</span><span class="sxs-lookup"><span data-stu-id="01652-379">Alternatively, follow these steps in order to give the user account you are logged in with `db_owner` permissions:</span></span>
> 
> 1. <span data-ttu-id="01652-380">Установка SQL Server Management Studio с помощью установщика веб-платформы.</span><span class="sxs-lookup"><span data-stu-id="01652-380">Install SQL Server Management Studio using the Web Platform Installer.</span></span>
> 2. <span data-ttu-id="01652-381">Подключение к локальному экземпляру SQL Server Express (по умолчанию `.\SQLEXPRESS`).</span><span class="sxs-lookup"><span data-stu-id="01652-381">Connect to the local SQL Server Express instance (by default, `.\SQLEXPRESS`).</span></span>
> 3. <span data-ttu-id="01652-382">Нажмите кнопку **баз данных** &gt; *[localSubtextDatabase]* &gt; **безопасности** &gt; **пользователей** &gt; *[localSubtextUser*] (по умолчанию — `subtextuser`], щелкните правой кнопкой мыши и нажмите кнопку **свойства**.</span><span class="sxs-lookup"><span data-stu-id="01652-382">Click **Databases** &gt; *[localSubtextDatabase]* &gt; **Security** &gt; **Users** &gt; *[localSubtextUser*] (default is `subtextuser`], right-click, and click **Properties**.</span></span>
> 4. <span data-ttu-id="01652-383">Выберите **db\_владельца** раздела членство в роли.</span><span class="sxs-lookup"><span data-stu-id="01652-383">Select **db\_owner** in the role membership section.</span></span>


#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="01652-384">Проблема: Сайта может не работать после публикации, если поле «URL-адрес назначения» нет префикса http:// или https://</span><span class="sxs-lookup"><span data-stu-id="01652-384">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="01652-385">В **параметры публикации** диалоговое окно, если URL-адрес назначения не начинается с `http://` или `https://`, сайт может не работать после развертывания.</span><span class="sxs-lookup"><span data-stu-id="01652-385">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="01652-386">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="01652-386">**Workaround**</span></span>  
> <span data-ttu-id="01652-387">Убедитесь, что перед публикацией сайта, URL-адрес назначения в **параметры публикации** диалоговое окно начинается с `http://` или `https://`.</span><span class="sxs-lookup"><span data-stu-id="01652-387">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="01652-388">Проблема: Публикация базы данных MySQL завершается с ошибкой «не удалось опубликовать базу данных.</span><span class="sxs-lookup"><span data-stu-id="01652-388">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="01652-389">Это может произойти, если удаленная база данных не может запустить сценарий.»</span><span class="sxs-lookup"><span data-stu-id="01652-389">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="01652-390">Эта ошибка может возникнуть по ряду причин.</span><span class="sxs-lookup"><span data-stu-id="01652-390">The error can occur for a number of reasons.</span></span> <span data-ttu-id="01652-391">Одна из причин этой ошибки можно увидеть, — если сценарий базы данных содержит символ одиночные кавычки (') и не является база данных MySQL назначения по умолчанию используется кодировка UTF-8.</span><span class="sxs-lookup"><span data-stu-id="01652-391">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="01652-392">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="01652-392">**Workaround**</span></span>  
> <span data-ttu-id="01652-393">Задайте кодировку по умолчанию для удаленной базы данных MySQL в UTF-8.</span><span class="sxs-lookup"><span data-stu-id="01652-393">Set the default character set for the remote MySQL database to UTF-8.</span></span>


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a><span data-ttu-id="01652-394">Проблема: Некоторые ссылки не отображаются в DotNetNuke после публикации или загрузка узла</span><span class="sxs-lookup"><span data-stu-id="01652-394">Issue: Some links are not visible in DotNetNuke after publishing or downloading the site</span></span>

> <span data-ttu-id="01652-395">При публикации или загрузить узла DotNetNuke, может потребоваться очистить кэш, чтобы получить новые ссылки на отобразились на сайте.</span><span class="sxs-lookup"><span data-stu-id="01652-395">If you publish or download a DotNetNuke site, you might need to clear the cache to get the new links to appear on the site.</span></span>
> 
> <span data-ttu-id="01652-396">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="01652-396">**Workaround**</span></span>
> 
> 1. <span data-ttu-id="01652-397">Войдите в систему как «Узел».</span><span class="sxs-lookup"><span data-stu-id="01652-397">Log in as "Host".</span></span>
> 2. <span data-ttu-id="01652-398">Перейдите к меню узел и выберите **параметры узла**.</span><span class="sxs-lookup"><span data-stu-id="01652-398">Go to the host menu and select **Host Settings**.</span></span>
> 3. <span data-ttu-id="01652-399">Прокрутите список вниз и в разделе **Дополнительные параметры**, разверните **параметры производительности**.</span><span class="sxs-lookup"><span data-stu-id="01652-399">Scroll down and under **Advanced Settings**, expand **Performance Settings**.</span></span>
> 4. <span data-ttu-id="01652-400">Нажмите кнопку **очистить кэш** ссылку для страницы.</span><span class="sxs-lookup"><span data-stu-id="01652-400">Click the **Clear Cache** link for pages.</span></span>
> 5. <span data-ttu-id="01652-401">Перейдите к нижней части страницы и перезапустить приложение.</span><span class="sxs-lookup"><span data-stu-id="01652-401">Go to the bottom of the page and restart the application.</span></span>


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a><span data-ttu-id="01652-402">Проблема: Некоторые ссылки в AtomSite не работают после загрузки опубликованного узла</span><span class="sxs-lookup"><span data-stu-id="01652-402">Issue: Some links in AtomSite are broken after you download a published site</span></span>

> <span data-ttu-id="01652-403">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="01652-403">**Workaround**</span></span>  
> <span data-ttu-id="01652-404">В *service.config* файла, *users.config* файла и всех *.xml* файлы, замените строку URL-адреса (например, `http://myhost.com/atomsite`) с локальной (например, `http://localhost:1239`).</span><span class="sxs-lookup"><span data-stu-id="01652-404">In the *service.config* file, *users.config* file, and all *.xml* files, replace the URL string (for example, `http://myhost.com/atomsite`) with the local one (for example, `http://localhost:1239`).</span></span>


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a><span data-ttu-id="01652-405">Проблема: MySQL-приложений, как в WordPress не смогут публиковать и сообщения об ошибке в базе данных</span><span class="sxs-lookup"><span data-stu-id="01652-405">Issue: MySQL-based applications like WordPress fail to publish and report a database error</span></span>

> <span data-ttu-id="01652-406">По умолчанию WebMatrix устанавливается MySQL с кодировкой UTF-8.</span><span class="sxs-lookup"><span data-stu-id="01652-406">By default, WebMatrix installs MySQL with the UTF-8 character set.</span></span> <span data-ttu-id="01652-407">Если установка MySQL самостоятельно, а не кодировку UTF-8 (например, это Latin1), процесс публикации для баз данных может завершиться сбоем.</span><span class="sxs-lookup"><span data-stu-id="01652-407">If you install MySQL on your own, and the character set is not UTF-8 (for example, it is Latin1), the publish process for databases might fail.</span></span>
> 
> <span data-ttu-id="01652-408">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="01652-408">**Workaround**</span></span>
> 
> 1. <span data-ttu-id="01652-409">Измените кодировку, MySQL, кодировка UTF-8.</span><span class="sxs-lookup"><span data-stu-id="01652-409">Change the character set for MySQL to UTF-8.</span></span> <span data-ttu-id="01652-410">(Дополнительные сведения см. в разделе [набор символов сервера и параметры сортировки](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) MySQL веб-сайта.)</span><span class="sxs-lookup"><span data-stu-id="01652-410">(For details, see [Server Character Set and Collation](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) on the MySQL website.)</span></span>
> 2. <span data-ttu-id="01652-411">Переустановите приложение.</span><span class="sxs-lookup"><span data-stu-id="01652-411">Reinstall the application.</span></span>
> 3. <span data-ttu-id="01652-412">Повторно опубликуйте приложение.</span><span class="sxs-lookup"><span data-stu-id="01652-412">Republish the application.</span></span>


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a><span data-ttu-id="01652-413">Проблема: «Загрузки публикации узла» завершается ошибкой для приложений, имеющих установки на основе браузера</span><span class="sxs-lookup"><span data-stu-id="01652-413">Issue: "Download published site" fails for applications that have browser-based setup</span></span>

> <span data-ttu-id="01652-414">Некоторые приложения (например, Kentico CMS) требуется запускать их в браузере для выполнения программы установки после установки, например для создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="01652-414">Some applications (for example, Kentico CMS) require you to launch them in the browser in order to perform post-installation setup such as creating a database.</span></span> <span data-ttu-id="01652-415">При публикации приложения следующим образом без завершения установки на основе браузера, попытка загрузить на одном сайте с удаленного сервера завершится ошибкой.</span><span class="sxs-lookup"><span data-stu-id="01652-415">If you publish an application like this without completing the browser-based setup, attempting to download the same site from a remote server will fail.</span></span>
> 
> <span data-ttu-id="01652-416">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="01652-416">**Workaround**</span></span>  
> <span data-ttu-id="01652-417">Завершите установку с использованием обозревателя перед публикацией узла.</span><span class="sxs-lookup"><span data-stu-id="01652-417">Finish browser-based setup before publishing the site.</span></span>


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a><span data-ttu-id="01652-418">Проблема: «Загрузки публикации узла» приведет к ошибке базы данных для DotNetNuke и Kooboo CMS</span><span class="sxs-lookup"><span data-stu-id="01652-418">Issue: "Download published site" fails with a database error for DotNetNuke and Kooboo CMS</span></span>

> <span data-ttu-id="01652-419">Если при попытке загрузить приложения с сервера, и иметь учетные данные администратора в строку подключения базы данных в **параметры публикации** диалогового окна, может появиться следующая ошибка в журнале публикации:</span><span class="sxs-lookup"><span data-stu-id="01652-419">If you try to download an application from a server and you have administrator credentials in the database connection string in the **Publish Settings** dialog, you might see the following error in the publish log:</span></span>
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> <span data-ttu-id="01652-420">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="01652-420">**Workaround**</span></span>  
> <span data-ttu-id="01652-421">Если это возможно, повторно опубликовать сайт (или опубликовать его) с помощью учетных данных без прав администратора для базы данных.</span><span class="sxs-lookup"><span data-stu-id="01652-421">If practical, republish the site (or have it published) using non-administrator credentials for the database.</span></span>


<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="01652-422">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="01652-422">For More Information</span></span>

<span data-ttu-id="01652-423">Дополнительные сведения о WebMatrix 1.0 см. ниже веб-сайтов:</span><span class="sxs-lookup"><span data-stu-id="01652-423">For more information about WebMatrix 1.0, see the following websites:</span></span>

- [<span data-ttu-id="01652-424">IIS.net</span><span class="sxs-lookup"><span data-stu-id="01652-424">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="01652-425">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="01652-425">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="01652-426">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="01652-426">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

<span data-ttu-id="01652-427">© 2011 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="01652-427">© 2011 Microsoft Corporation.</span></span> <span data-ttu-id="01652-428">Все права защищены.</span><span class="sxs-lookup"><span data-stu-id="01652-428">All Rights Reserved.</span></span> <span data-ttu-id="01652-429">[Условия использования](https://msdn.microsoft.cos/cc300389.aspx).</span><span class="sxs-lookup"><span data-stu-id="01652-429">[Terms of Use](https://msdn.microsoft.cos/cc300389.aspx).</span></span>
