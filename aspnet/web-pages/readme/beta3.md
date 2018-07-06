---
uid: web-pages/readme/beta3
title: Web Matrix и ASP.NET Web Pages (Razor) о бета-версии 3 выпуска | Документация Майкрософт
author: rick-anderson
description: Web Matrix и ASP.NET Web Pages (Razor) о бета-версии 3 выпуска
ms.author: aspnetcontent
ms.date: 01/10/2011
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: 16b324e555b5450fc1e05c63e7e19985a2d02b89
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831836"
---
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a><span data-ttu-id="cccab-103">Web Matrix и ASP.NET Web Pages (Razor) о бета-версии 3 выпуска</span><span class="sxs-lookup"><span data-stu-id="cccab-103">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>
====================
> <span data-ttu-id="cccab-104">Web Matrix и ASP.NET Web Pages (Razor) о бета-версии 3 выпуска</span><span class="sxs-lookup"><span data-stu-id="cccab-104">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>

<span data-ttu-id="cccab-105">9 ноября 2010 г.</span><span class="sxs-lookup"><span data-stu-id="cccab-105">9 November 2010</span></span>

## <a name="contents"></a><span data-ttu-id="cccab-106">Описание</span><span class="sxs-lookup"><span data-stu-id="cccab-106">Contents</span></span>

- [<span data-ttu-id="cccab-107">Обзор набора средств Visual Studio для Unity</span><span class="sxs-lookup"><span data-stu-id="cccab-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="cccab-108">Установка</span><span class="sxs-lookup"><span data-stu-id="cccab-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="cccab-109">Новые возможности, изменения и известные проблемы в бета-версии 3</span><span class="sxs-lookup"><span data-stu-id="cccab-109">New Features, Changes, and Known Issues in the Beta 3 release</span></span>](#Known_Issues)

    - [<span data-ttu-id="cccab-110">Проблемы установки WebMatrix</span><span class="sxs-lookup"><span data-stu-id="cccab-110">WebMatrix Installation Issues</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="cccab-111">Веб-страницы ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cccab-111">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="cccab-112">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="cccab-112">SQL Server Compact</span></span>](#Known_Issues_SQL_Server_Compact)
    - [<span data-ttu-id="cccab-113">Установка приложений</span><span class="sxs-lookup"><span data-stu-id="cccab-113">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="cccab-114">Публикация приложений</span><span class="sxs-lookup"><span data-stu-id="cccab-114">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
    - [<span data-ttu-id="cccab-115">Другие проблемы</span><span class="sxs-lookup"><span data-stu-id="cccab-115">Other Issues</span></span>](#Known_Issues_Other_Issues)
- [<span data-ttu-id="cccab-116">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="cccab-116">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="cccab-117">Обзор</span><span class="sxs-lookup"><span data-stu-id="cccab-117">Overview</span></span>

> <span data-ttu-id="cccab-118">Бета-версия Microsoft WebMatrix представляет собой стек разработки бесплатного, которая устанавливается за считанные минуты.</span><span class="sxs-lookup"><span data-stu-id="cccab-118">Microsoft WebMatrix Beta is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="cccab-119">Веб-сервера он интегрируется с базой данных и платформами программирования для создания единой, интегрированной среды.</span><span class="sxs-lookup"><span data-stu-id="cccab-119">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="cccab-120">WebMatrix бета-версии можно использовать для упрощения разработки кода, тестирования и публикации собственного ASP.NET или PHP, веб-сайта или бета-версии WebMatrix можно использовать для создания нового веб-сайтов с помощью популярных приложений с открытым кодом как DotNetNuke, Umbraco, WordPress и Joomla.</span><span class="sxs-lookup"><span data-stu-id="cccab-120">You can use WebMatrix Beta to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix Beta to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="cccab-121">Бета-версии WebMatrix использует же мощный веб-сервер, СУБД и платформы среду, которая будет выполняться веб-сайта в Интернете, что упрощает переход от разработки в рабочей среде и ускоряет.</span><span class="sxs-lookup"><span data-stu-id="cccab-121">WebMatrix Beta uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>


<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="cccab-122">Установка</span><span class="sxs-lookup"><span data-stu-id="cccab-122">Installation</span></span>

> <span data-ttu-id="cccab-123">Чтобы установить WebMatrix бета-версии 3, используйте [веб-платформы Microsoft Installer-3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="cccab-123">To install WebMatrix Beta 3, you use [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="cccab-124">После установки установщика веб-платформы, его можно использовать для установки WebMatrix бета-версии 3.</span><span class="sxs-lookup"><span data-stu-id="cccab-124">After you've installed the Web Platform Installer, you can use it to install WebMatrix Beta 3.</span></span>
> 
> <span data-ttu-id="cccab-125">При возникновении проблем во время установки, см. [Устранение неполадок с Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span><span class="sxs-lookup"><span data-stu-id="cccab-125">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a><span data-ttu-id="cccab-126">Инструкции для публикации приложений</span><span class="sxs-lookup"><span data-stu-id="cccab-126">Instructions for Publishing Applications</span></span>

> <span data-ttu-id="cccab-127">См. в разделе [пошаговые инструкции для публикации приложений](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="cccab-127">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a><span data-ttu-id="cccab-128">Новые возможности, изменения, andKnown проблемы</span><span class="sxs-lookup"><span data-stu-id="cccab-128">New Features, Changes, andKnown Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a><span data-ttu-id="cccab-129">Установка WebMatrix бета-версия 3</span><span class="sxs-lookup"><span data-stu-id="cccab-129">WebMatrix Beta 3 Installation</span></span>

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="cccab-130">Проблема: WebMatrix бета-версия 3 доступна только на платформах, поддерживающих Microsoft .NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="cccab-130">Issue: WebMatrix Beta 3 is only available on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="cccab-131">В .NET Framework версии 4 является обязательным для WebMatrix бета-версии.</span><span class="sxs-lookup"><span data-stu-id="cccab-131">The .NET Framework version 4 is required for WebMatrix Beta.</span></span> <span data-ttu-id="cccab-132">В некоторых случаях установщик бета-версии WebMatrix позволит установке на платформу, которая не является частью набора поддерживаемых конфигураций.</span><span class="sxs-lookup"><span data-stu-id="cccab-132">In certain cases, the WebMatrix Beta installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="cccab-133">В частности Windows Vista без обновления SP1 позволяет начать установку WebMatrix бета-версии, но компонент .NET Framework 4 не удастся и блокировать установку.</span><span class="sxs-lookup"><span data-stu-id="cccab-133">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix Beta, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="cccab-134">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="cccab-134">**Workaround**</span></span>  
> <span data-ttu-id="cccab-135">Установите на поддерживаемых платформах, включая:</span><span class="sxs-lookup"><span data-stu-id="cccab-135">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="cccab-136">Windows 7</span><span class="sxs-lookup"><span data-stu-id="cccab-136">Windows 7</span></span>
> - <span data-ttu-id="cccab-137">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="cccab-137">Windows Server 2008</span></span>
> - <span data-ttu-id="cccab-138">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="cccab-138">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="cccab-139">Windows Vista с пакетом обновления 1 (SP1) или выше</span><span class="sxs-lookup"><span data-stu-id="cccab-139">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="cccab-140">Windows XP с пакетом обновления 3 (SP3)</span><span class="sxs-lookup"><span data-stu-id="cccab-140">Windows XP SP3</span></span>
> - <span data-ttu-id="cccab-141">Windows Server 2003 с пакетом обновления 2 (SP2)</span><span class="sxs-lookup"><span data-stu-id="cccab-141">Windows Server 2003 SP2</span></span>


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="cccab-142">Проблема: Не удается установить WebMatrix бета-версии 3, если Microsoft Visual Studio 2008 установлено без Microsoft Visual Studio 2008 с пакетом обновления 1</span><span class="sxs-lookup"><span data-stu-id="cccab-142">Issue: Cannot install WebMatrix Beta 3 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="cccab-143">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="cccab-143">**Workaround**</span></span>  
> <span data-ttu-id="cccab-144">Установка [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) из центра загрузки Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="cccab-144">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="cccab-145">Проблема: Некоторые сборки для SQL Server Compact 4.0 не установлены в глобальном кэше СБОРОК</span><span class="sxs-lookup"><span data-stu-id="cccab-145">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="cccab-146">Управляемые сборки для SQL Server Compact 4.0 не помещается в глобальный кэш сборок (GAC), при установке SQL Server Compact 4.0 на компьютере с 64-разрядной, и на компьютере установлена только профиль .NET Framework 3.5 с пакетом обновления 1 клиента установлен.</span><span class="sxs-lookup"><span data-stu-id="cccab-146">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="cccab-147">Управляемые сборки, которые не установлены в глобальном кэше СБОРОК являются:</span><span class="sxs-lookup"><span data-stu-id="cccab-147">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="cccab-148">*System.Data.SqlServerCe.dll* (поставщик ADO.NET)</span><span class="sxs-lookup"><span data-stu-id="cccab-148">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="cccab-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)</span><span class="sxs-lookup"><span data-stu-id="cccab-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="cccab-150">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="cccab-150">**Workaround**</span></span>  
> <span data-ttu-id="cccab-151">Удаление SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="cccab-151">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="cccab-152">Скачайте и установите полную версию .NET Framework 3.5 SP1 из следующего расположения:</span><span class="sxs-lookup"><span data-stu-id="cccab-152">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="cccab-153">Microsoft .NET Framework 3.5 пакетом обновления 1 (полный пакет)</span><span class="sxs-lookup"><span data-stu-id="cccab-153">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="cccab-154">Затем переустановите SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="cccab-154">Then reinstall SQL Server Compact 4.0.</span></span>


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="cccab-155">Проблема: Не удается удалить SQL Server Compact с помощью командной строки</span><span class="sxs-lookup"><span data-stu-id="cccab-155">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="cccab-156">Удаление SQL Server Compact через параметры командной строки не работает в этом выпуске.</span><span class="sxs-lookup"><span data-stu-id="cccab-156">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="cccab-157">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="cccab-157">**Workaround**</span></span>  
> <span data-ttu-id="cccab-158">Используйте *программы и компоненты* на панели управления Windows, чтобы удалить Microsoft SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="cccab-158">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="cccab-159">Веб-страницы ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cccab-159">ASP.NET Web Pages</span></span>

<span data-ttu-id="cccab-160">Этот раздел документа описывает новые возможности, изменения и известные проблемы с бета-версии 3 веб-страниц ASP.NET с синтаксисом Razor.</span><span class="sxs-lookup"><span data-stu-id="cccab-160">This section of the document describes new features, changes, and known issues with the Beta 3 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="cccab-161">Новые возможности</span><span class="sxs-lookup"><span data-stu-id="cccab-161">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="cccab-162">Изменения</span><span class="sxs-lookup"><span data-stu-id="cccab-162">Changes</span></span>](#Changes)
- [<span data-ttu-id="cccab-163">Проблемы</span><span class="sxs-lookup"><span data-stu-id="cccab-163">Issues</span></span>](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="cccab-164">Новые возможности бета-версии 3 для веб-страниц ASP.NET с синтаксисом Razor</span><span class="sxs-lookup"><span data-stu-id="cccab-164">New Features in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a><span data-ttu-id="cccab-165">Новинка: «Html.Raw» метод выводит разметку без кодировки</span><span class="sxs-lookup"><span data-stu-id="cccab-165">New: "Html.Raw" method renders unencoded markup</span></span>

> <span data-ttu-id="cccab-166">Новый `Html.Raw` метод позволяет отображать разметку HTML в виде разметки вместо отображения закодированную выходную.</span><span class="sxs-lookup"><span data-stu-id="cccab-166">The new `Html.Raw` method lets you render HTML markup as markup instead of rendering encoded output.</span></span> <span data-ttu-id="cccab-167">(По умолчанию ASP.NET Razor кодирует строки перед их отображением.) Синтаксис выглядит следующим образом.</span><span class="sxs-lookup"><span data-stu-id="cccab-167">(By default, ASP.NET Razor encodes strings before rendering them.) The syntax is:</span></span>
> 
> `Html.Raw(value)`
> 
> <span data-ttu-id="cccab-168">В следующем примере показано использование `Html.Raw`.</span><span class="sxs-lookup"><span data-stu-id="cccab-168">The following example shows how to use `Html.Raw`:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="cccab-169">Изменения в бета-версии 3 для веб-страниц ASP.NET с синтаксисом Razor</span><span class="sxs-lookup"><span data-stu-id="cccab-169">Changes in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="change-hrefattribute-method-removed"></a><span data-ttu-id="cccab-170">Изменение: Удален метод «HrefAttribute»</span><span class="sxs-lookup"><span data-stu-id="cccab-170">Change: "HrefAttribute" method removed</span></span>

> <span data-ttu-id="cccab-171">`HrefAttribute` Метод `WebPage` класс был удален.</span><span class="sxs-lookup"><span data-stu-id="cccab-171">The `HrefAttribute` method of the `WebPage` class has been removed.</span></span> <span data-ttu-id="cccab-172">Этот вспомогательный был использован для кодирования небезопасные символы URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="cccab-172">This helper was used to encode unsafe characters in URLs.</span></span> <span data-ttu-id="cccab-173">Он больше не требуется, так как ASP.NET Razor автоматически кодирует строки.</span><span class="sxs-lookup"><span data-stu-id="cccab-173">It is no longer required because ASP.NET Razor automatically encodes strings.</span></span> <span data-ttu-id="cccab-174">(Использовать новый `Html.Raw` метод для обработки строк без кодировки.)</span><span class="sxs-lookup"><span data-stu-id="cccab-174">(Use the new `Html.Raw` method to render unencoded strings.)</span></span>


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a><span data-ttu-id="cccab-175">Изменение: Синтаксис для декларативной "@helper" Изменить вспомогательные функции</span><span class="sxs-lookup"><span data-stu-id="cccab-175">Change: Syntax for declarative "@helper" helpers changed</span></span>

> <span data-ttu-id="cccab-176">В выпуске бета-версии 3, ASP.NET изменяет анализе вспомогательные функции, которые создаются с помощью `@helper` синтаксис.</span><span class="sxs-lookup"><span data-stu-id="cccab-176">In the Beta 3 release, ASP.NET changes how it parses helpers that are created using the `@helper` syntax.</span></span> <span data-ttu-id="cccab-177">По сути `@helper` синтаксис теперь анализируется как блок кода, а не как блок разметки, которая может включать код.</span><span class="sxs-lookup"><span data-stu-id="cccab-177">In essence, the `@helper` syntax is now parsed as a code block instead of as a block of markup that can include code.</span></span> <span data-ttu-id="cccab-178">Таким образом, код внутри модуля поддержки не нужно заключать в `@{ }` блоков.</span><span class="sxs-lookup"><span data-stu-id="cccab-178">Therefore, code inside the helper does not need to be enclosed in `@{ }` blocks.</span></span> <span data-ttu-id="cccab-179">И наоборот, это разметка внутри модуля поддержки должно быть явно включено, в элементы HTML или ASP.NET Razor `<text></text>` теги.</span><span class="sxs-lookup"><span data-stu-id="cccab-179">Conversely, markup inside the helper has to be explicitly included in HTML elements or in ASP.NET Razor `<text></text>` tags.</span></span>
> 
> <span data-ttu-id="cccab-180">Например, следующая `@helper` работает в выпуске бета-версии 3:</span><span class="sxs-lookup"><span data-stu-id="cccab-180">For example, the following `@helper` syntax works in the Beta 3 release:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> <span data-ttu-id="cccab-181">В выпуске бета-версии 3 этого вспомогательного объекта необходимо изменить таким образом, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="cccab-181">In the Beta 3 release, this helper must be changed to look like the following example:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> <span data-ttu-id="cccab-182">Обратите внимание, что `@{ }` символы исходный код во вспомогательном методе больше не используется.</span><span class="sxs-lookup"><span data-stu-id="cccab-182">Notice that the `@{ }` characters around the initial code in the helper is no longer used.</span></span> <span data-ttu-id="cccab-183">Это обусловлено содержимое из вспомогательных методов, обрабатываются как блок кода по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="cccab-183">This is because the contents of the helpers are treated as a code block by default.</span></span> <span data-ttu-id="cccab-184">Вспомогательное приложение выводит разметку, начинающуюся с открывающей `<a>` тега.</span><span class="sxs-lookup"><span data-stu-id="cccab-184">The helper renders markup, which starts with the opening `<a>` tag.</span></span> <span data-ttu-id="cccab-185">Если вспомогательное приложение необходимо подготовить обычного текста или меток, которые будут отсутствовать закрывающий тег (например, `<meta>` тегов), визуализируемое содержимое должно быть в `<text></text>` теги.</span><span class="sxs-lookup"><span data-stu-id="cccab-185">If the helper must render plain text or tags that do not include a closing tag (for example, `<meta>` tags), the content to be rendered must be in `<text></text>` tags.</span></span>


#### <a name="change-webpagecontexthttpcontext-removed"></a><span data-ttu-id="cccab-186">Изменений: Удаление «WebPageContext.HttpContext»</span><span class="sxs-lookup"><span data-stu-id="cccab-186">Change: "WebPageContext.HttpContext" removed</span></span>

> <span data-ttu-id="cccab-187">`WebPageContext.HttpContext` Свойство был удален.</span><span class="sxs-lookup"><span data-stu-id="cccab-187">The `WebPageContext.HttpContext` property has been removed.</span></span> <span data-ttu-id="cccab-188">Взамен рекомендуется использовать `HttpContext.Current`.</span><span class="sxs-lookup"><span data-stu-id="cccab-188">Use `HttpContext.Current` instead.</span></span> <span data-ttu-id="cccab-189">( `WebPageContext.HttpContext` Свойство просто обернул это.)</span><span class="sxs-lookup"><span data-stu-id="cccab-189">(The `WebPageContext.HttpContext` property simply wrapped this.)</span></span>


#### <a name="change-facebook-helper-moved-to-new-package"></a><span data-ttu-id="cccab-190">Изменение: Переместить в новый пакет вспомогательный «Facebook»</span><span class="sxs-lookup"><span data-stu-id="cccab-190">Change: "Facebook" helper moved to new package</span></span>

> <span data-ttu-id="cccab-191">`Facebook` Вспомогательный был перемещен в *Facebook.Helper* библиотеки, которая содержит `Facebook` вспомогательный метод и дополнительные функции.</span><span class="sxs-lookup"><span data-stu-id="cccab-191">The `Facebook` helper has been moved to the *Facebook.Helper* library, which includes the `Facebook` helper and additional functionality.</span></span> <span data-ttu-id="cccab-192">Необходимо установить эту библиотеку как отдельный пакет, как описано в «Установка вспомогательных функций с помощью диспетчера пакетов» в этом руководстве [Приступая к работе со страницами ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202889).</span><span class="sxs-lookup"><span data-stu-id="cccab-192">You must install this library as a separate package, as described in "Installing Helpers with Package Manager" in the tutorial [Getting Started with ASP.NET Pages](https://go.microsoft.com/fwlink/?LinkId=202889).</span></span>


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a><span data-ttu-id="cccab-193">Изменение: Типы членства, ролей и безопасности перемещает новую сборку</span><span class="sxs-lookup"><span data-stu-id="cccab-193">Change: Membership, Role, and Security types moves to new assembly</span></span>

> <span data-ttu-id="cccab-194">Следующие типы были перемещены `WebMatrix.WebData` сборки:</span><span class="sxs-lookup"><span data-stu-id="cccab-194">The following types were moved to the `WebMatrix.WebData` assembly:</span></span>
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a><span data-ttu-id="cccab-195">: Класс «TagBuilder» перемещена на сборку System.Web.WebPages.dll</span><span class="sxs-lookup"><span data-stu-id="cccab-195">Change: "TagBuilder" class moved to System.Web.WebPages.dll assembly</span></span>

> <span data-ttu-id="cccab-196">`TagBuilde` r класс был перемещен в сборку System.Web.WebPages.dll.</span><span class="sxs-lookup"><span data-stu-id="cccab-196">The `TagBuilde` r class has been moved to the System.Web.WebPages.dll assembly.</span></span> <span data-ttu-id="cccab-197">Ранее это было в сборке, который был частью ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="cccab-197">Previously, this was in an assembly that was part of ASP.NET MVC.</span></span> <span data-ttu-id="cccab-198">Это изменение означает, что не нужно установить ASP.NET MVC, чтобы использовать `TagBuilder` класса.</span><span class="sxs-lookup"><span data-stu-id="cccab-198">This change means that you do not have to install ASP.NET MVC in order to use the `TagBuilder` class.</span></span>
> 
> <span data-ttu-id="cccab-199">Тем не менее, класс является по-прежнему в `System.Web.Mvc` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="cccab-199">However, the class is still in the `System.Web.Mvc` namespace.</span></span> <span data-ttu-id="cccab-200">Чтобы использовать `TagBuilder` класса (например, в пользовательских ASP.NET Razor вспомогательный объект), необходимо сослаться на пространство имен (например, путем добавления `@using System.Web.Mvc` в код).</span><span class="sxs-lookup"><span data-stu-id="cccab-200">In order to use the `TagBuilder` class (for example, in a custom ASP.NET Razor helper), you must reference the namespace (for example, by adding `@using System.Web.Mvc` to your code).</span></span>


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a><span data-ttu-id="cccab-201">Изменение: Запрос проверка синтаксиса изменены; Удален класс «Проверка»</span><span class="sxs-lookup"><span data-stu-id="cccab-201">Change: Request validation syntax changed; "Validation" class removed</span></span>

> <span data-ttu-id="cccab-202">В выпуске бета-версии 3, чтобы отключить проверку для отдельных полей или набор полей, можно вызвать `Validation.Exclude` метод, передавая имя или имена полей для исключения из проверки.</span><span class="sxs-lookup"><span data-stu-id="cccab-202">In the Beta 3 release, to disable validation for an individual field or set of fields, you can call the `Validation.Exclude` method, passing in the name or names of the fields to exclude from validation.</span></span> <span data-ttu-id="cccab-203">Новый синтаксис доступен в бета-версии 3 для обхода проверки.</span><span class="sxs-lookup"><span data-stu-id="cccab-203">A new syntax is available in the Beta 3 release for bypassing validation.</span></span> <span data-ttu-id="cccab-204">`Validation` Метод, используемый в бета-версии 3 были удалены.</span><span class="sxs-lookup"><span data-stu-id="cccab-204">The `Validation` method used in Beta 3 has been removed.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="cccab-205">Если не отключить проверку запросов, если пользователь попытается отправить HTML-разметка (например, с помощью редактора форматированного текста на странице), веб-сайт сообщит об ошибке, например *было обнаружено потенциально опасных значения Request.Form от клиента*и входные данные пользователя не принят.</span><span class="sxs-lookup"><span data-stu-id="cccab-205">If you do not disable request validation, if users try to upload HTML markup (for example, by using a rich text editor on a page), the website will report an error like *A potentially dangerous Request.Form value was detected from the client* and the user input is not accepted.</span></span> <span data-ttu-id="cccab-206">Если вы отключите проверку запросов, необходимо самостоятельно записать после ввода, чтобы убедиться в том, что оно не должно содержать потенциально опасные разметки или скрипт с помощью примерно [Microsoft версии 4.0 библиотеки сценариев сайта защиты между](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span><span class="sxs-lookup"><span data-stu-id="cccab-206">If you disable request validation, you must manually check user input to make sure that it does not contain potentially dangerous markup or script using something like the [Microsoft Anti-Cross Site Scripting Library V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span></span>
> 
> 
> <span data-ttu-id="cccab-207">Чтобы отключить автоматическую проверку запросов, вызовите `Request.Unvalidated` метод, передав ему имя поля или другой объект post, который вы хотите пропустить проверку запроса для.</span><span class="sxs-lookup"><span data-stu-id="cccab-207">To disable automatic request validation, call the `Request.Unvalidated` method, passing it the name of the field or other post object that you want to bypass request validation for.</span></span> <span data-ttu-id="cccab-208">Этот метод можно использовать для обхода проверки для всех элементов в `Form`, `QueryString`, `Cookies`, и `ServerVariables` коллекций.</span><span class="sxs-lookup"><span data-stu-id="cccab-208">You can use this method to bypass validation for any items in the `Form`, `QueryString`, `Cookies`, and `ServerVariables` collections.</span></span> <span data-ttu-id="cccab-209">Следующие примеры показывают, как использовать `Unvalidated` метод:</span><span class="sxs-lookup"><span data-stu-id="cccab-209">The following examples show how to use the `Unvalidated` method:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="cccab-210">Известные проблемы веб-страниц ASP.NET с синтаксисом Razor</span><span class="sxs-lookup"><span data-stu-id="cccab-210">Known Issues for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="cccab-211">Проблема: Непредвиденное поведение при использовании пользовательского таблицы членства</span><span class="sxs-lookup"><span data-stu-id="cccab-211">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="cccab-212">Чтобы инициализировать поставщик членства для на веб-сайте ASP.NET Razor, вызовите `WebSecurity.InitializeDatabaseConnection` метод.</span><span class="sxs-lookup"><span data-stu-id="cccab-212">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="cccab-213">(В WebMatrix шаблоне начального сайта включает вызов этого метода в  *\_AppStart.cshtml* файл.) Если `autoCreateTables` задан параметр этого метода значение true (по умолчанию ему присваивается значение true, если в шаблоне начального сайта), и если нераспознанный имя_таблицы передается в метод (второй параметр), метод выдает ошибку.</span><span class="sxs-lookup"><span data-stu-id="cccab-213">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="cccab-214">Вместо этого он автоматически создадут таблицу.</span><span class="sxs-lookup"><span data-stu-id="cccab-214">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="cccab-215">Это может стать проблемой, если вы планируете использовать пользовательские таблицы членства, но передать имя таблицы не так, чтобы `WebSecurity.InitializeDatabaseConnection` метод.</span><span class="sxs-lookup"><span data-stu-id="cccab-215">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="cccab-216">Поскольку метод не по умолчанию создается ошибка, если таблицы не существует, а вместо этого он создает новую таблицу, приложение может показаться работать.</span><span class="sxs-lookup"><span data-stu-id="cccab-216">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="cccab-217">Тем не менее код приложения, который основан на пользовательские таблицы (и поля в нем) со временем может завершаться непредвиденные ошибки.</span><span class="sxs-lookup"><span data-stu-id="cccab-217">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="cccab-218">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="cccab-218">**Workaround**</span></span>  
> <span data-ttu-id="cccab-219">Убедитесь, что имя, переданное в `InitializeDatabaseConnection` метод совпадений, профиль пользователя из таблицы в базе данных членства, или убедитесь, что `autoCreateTables` параметр имеет значение false.</span><span class="sxs-lookup"><span data-stu-id="cccab-219">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="cccab-220">Ошибка, проблема: «не удалось сформировать пользовательский экземпляр SQL Server»</span><span class="sxs-lookup"><span data-stu-id="cccab-220">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="cccab-221">Если приложение WebMatrix использует SQL Server Express и работает IIS 7.5 на Windows 7 или Windows Server 2008 R2, вы увидите ошибку, указывающую, что SQL Server не удалось получить путь локального приложения пользователя во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="cccab-221">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="cccab-222">**Инструкции по решению** убедитесь в том, что учетная запись Windows, то приложение запущено под (как правило, NETWORK SERVICE) имеет разрешения на чтение и запись для корневой папки приложения и вложенные папки например *приложения\_данных*.</span><span class="sxs-lookup"><span data-stu-id="cccab-222">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="cccab-223">Более подробные сведения см. в статье базы знаний [проблемы с SQL Server Express пользователя при создании экземпляров и ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="cccab-223">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a><span data-ttu-id="cccab-224">Проблема: В Visual Studio, пространства имен для пользовательских сборок (DLL) не импортируются автоматически</span><span class="sxs-lookup"><span data-stu-id="cccab-224">Issue: In Visual Studio, namespaces for custom assemblies (DLLs) are not imported automatically</span></span>

> <span data-ttu-id="cccab-225">Если вы используете пользовательские сборки в проекте в Visual Studio, пространства имен, объявленные в эти сборки не импортируются автоматически во время разработки.</span><span class="sxs-lookup"><span data-stu-id="cccab-225">If you use custom assemblies in a project in Visual Studio, the namespaces declared in those assemblies are not automatically imported at design time.</span></span> <span data-ttu-id="cccab-226">Таким образом ссылки на пользовательские типы могут не распознаваться во время разработки, помечаются как не указано в Visual Studio (с помощью «волнистая линия»).</span><span class="sxs-lookup"><span data-stu-id="cccab-226">As a result, references to custom types might not be recognized at design time and are marked as not recognized in Visual Studio (using a "squiggle").</span></span> <span data-ttu-id="cccab-227">Это происходит только во время разработки в Visual Studio. само приложение работает правильно.</span><span class="sxs-lookup"><span data-stu-id="cccab-227">This problem occurs only at design time in Visual Studio; the application itself runs properly.</span></span>
> 
> <span data-ttu-id="cccab-228">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="cccab-228">**Workaround**</span></span>  
> <span data-ttu-id="cccab-229">Включить `using` инструкции (`imports` в Visual Basic), ссылающийся на сущности, которые не распознаются во время разработки.</span><span class="sxs-lookup"><span data-stu-id="cccab-229">Include a `using` statement (`imports` in Visual Basic) that references the entities that are not recognized at design time.</span></span>


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="cccab-230">Проблема: Visual Studio IntelliSense и шаблоны проектов доступны только в ASP.NET MVC версии 3</span><span class="sxs-lookup"><span data-stu-id="cccab-230">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="cccab-231">Установка веб-страниц ASP.NET не также устанавливает средства для Visual Studio такие как IntelliSense и шаблоны проектов для приложений ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="cccab-231">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="cccab-232">**Инструкции по решению** использовать IntelliSense и шаблоны проектов для приложений веб-страниц ASP.NET в Visual Studio, установить версия-Кандидат ASP.NET MVC 3 с помощью установщика веб-платформы или [автономный установщик](https://go.microsoft.com/fwlink/?LinkID=191797).</span><span class="sxs-lookup"><span data-stu-id="cccab-232">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a><span data-ttu-id="cccab-233">Проблема: "&lt;вспомогательный&gt; не удалось найти класс» ошибка</span><span class="sxs-lookup"><span data-stu-id="cccab-233">Issue: "&lt;helper&gt; class cannot be found" error</span></span>

> <span data-ttu-id="cccab-234">После обновления до бета-версии 3, может появиться ошибка, вспомогательный класс (например, `Facebook` класс) не удается найти.</span><span class="sxs-lookup"><span data-stu-id="cccab-234">After you upgrade to Beta 3, you might see an error that a helper class (for example, the `Facebook` class) cannot not be found.</span></span> <span data-ttu-id="cccab-235">Начиная с бета-версии 2 и продолжая в бета-версии 3, были перемещены для пакетов, необходимо явно установить вспомогательные функции.</span><span class="sxs-lookup"><span data-stu-id="cccab-235">Starting in Beta 2 and continuing in Beta 3, helpers have been moved to packages that you must explicitly install.</span></span> <span data-ttu-id="cccab-236">Существующие узлы не будут обновлены до включения этих пакетов; Сюда входят сайта в *\My Documents\IISExpress* или *\My Documents\My веб-сайтов* папки.</span><span class="sxs-lookup"><span data-stu-id="cccab-236">Existing sites are not upgraded to include these packages; this includes sites in the *\My Documents\IISExpress* or *\My Documents\My Web Sites* folders.</span></span> <span data-ttu-id="cccab-237">В частности, вы увидите эту ошибку при использовании сайта по умолчанию в *My Sites* (WebSite1), который включает ссылку на `Twitter` вспомогательный.</span><span class="sxs-lookup"><span data-stu-id="cccab-237">In particular, you will see this error if you use the default site in *My Sites* (WebSite1), which includes a reference to the `Twitter` helper.</span></span>
> 
> <span data-ttu-id="cccab-238">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="cccab-238">**Workaround**</span></span>  
> <span data-ttu-id="cccab-239">Закомментируйте вызовы все вспомогательные функции в сайт, запустите  *\_администратора* и установить пакет или пакеты, содержащие вспомогательные методы, которые вы хотите использовать.</span><span class="sxs-lookup"><span data-stu-id="cccab-239">Comment out calls to any helpers in the site, run the *\_Admin* page, and install the package or packages that include the helpers that you want to use.</span></span> <span data-ttu-id="cccab-240">После установки пакета, Раскомментируйте строки, которые ссылаются на вспомогательные функции.</span><span class="sxs-lookup"><span data-stu-id="cccab-240">After you've installed the package, you can uncomment the lines that reference helpers.</span></span>


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a><span data-ttu-id="cccab-241">Проблема: Развертывание сборок бета-версии 3 ASP.NET Razor в папку Bin может не работать для размещения сайтов</span><span class="sxs-lookup"><span data-stu-id="cccab-241">Issue: Deploying Beta 3 ASP.NET Razor assemblies to the Bin folder might not work on hosting sites</span></span>

> <span data-ttu-id="cccab-242">При развертывании на веб-сайте веб-страниц ASP.NET для размещения сайта, и в том случае, если развернуть сборки ASP.NET Razor бета-версии 3 на сайт *Bin* папки, могут возникнуть ошибки, включая следующие:</span><span class="sxs-lookup"><span data-stu-id="cccab-242">If you deploy an ASP.NET Web Pages website to a hosting site, and if you deploy the ASP.NET Razor Beta 3 assemblies to the site's *Bin* folder, you might experience errors, including the following:</span></span>
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> <span data-ttu-id="cccab-243">Это может произойти, если поставщик установил сборки ASP.NET Web Pages бета-версии 1 в кэш сервера глобального приложения (GAC).</span><span class="sxs-lookup"><span data-stu-id="cccab-243">This can happen if the hosting provider has installed the ASP.NET Web Pages Beta 1 assemblies into the server's global application cache (GAC).</span></span> <span data-ttu-id="cccab-244">Сборки в глобальном кэше СБОРОК высокий приоритет по сравнению сборок, установленных локально в *Bin* папки.</span><span class="sxs-lookup"><span data-stu-id="cccab-244">Assemblies in the GAC get precedence over assemblies installed locally in the *Bin* folder.</span></span>
> 
> <span data-ttu-id="cccab-245">**Инструкции по решению** обратитесь к поставщику услуг размещения, убедитесь, что ошибки возникают из-за конфликта версий поставщика сборок и выбираете.</span><span class="sxs-lookup"><span data-stu-id="cccab-245">**Workaround** Contact your hosting provider to confirm that the errors you are seeing are due to a conflict between the provider's versions of the assemblies and yours.</span></span> <span data-ttu-id="cccab-246">Если Да, запрашивают поставщика услуг размещения об обновлении сборки в глобальном кэше СБОРОК на сервере.</span><span class="sxs-lookup"><span data-stu-id="cccab-246">If so, request that the hosting provider update the assemblies in the server's GAC.</span></span>


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="cccab-247">Проблема: Чтение веб-каналы или другие внешние источники данных через прокси-сервер</span><span class="sxs-lookup"><span data-stu-id="cccab-247">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="cccab-248">Если на сервер сайта находится за прокси-сервер, может потребоваться настроить сведения прокси-сервера в *Web.config* файл, чтобы иметь возможность считывать данные, поступающие от за пределами вашего сайта.</span><span class="sxs-lookup"><span data-stu-id="cccab-248">If the server running the site is behind a proxy server, you might need to configure proxy information in the *Web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="cccab-249">Например, если вы используете `ReCaptcha` helper, вспомогательное приложение взаимодействует со службой reCAPTCHA, но может быть заблокирован прокси-сервером.</span><span class="sxs-lookup"><span data-stu-id="cccab-249">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="cccab-250">Аналогично веб-каналы, которые используются в веб-страниц в ASP.NET, такие как веб-канал, используемый диспетчером пакетов, может потребоваться настройка прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="cccab-250">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="cccab-251">Если возникают проблемы в работе с внешней службы или работе с веб-канал пакетов, поместите следующие элементы в корень приложения *Web.config* файла:</span><span class="sxs-lookup"><span data-stu-id="cccab-251">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *Web.config* file:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> <span data-ttu-id="cccab-252">Дополнительные сведения о настройке прокси-сервер, см. в разделе [ &lt;прокси&gt; (сетевые параметры)](https://msdn.microsoft.com/library/sa91de1e.aspx) на сайте MSDN.</span><span class="sxs-lookup"><span data-stu-id="cccab-252">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on the MSDN Web site.</span></span>


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a><span data-ttu-id="cccab-253">Проблема: Ошибка «Не удается загрузить Microsoft.Web.Infrastructure.dll»</span><span class="sxs-lookup"><span data-stu-id="cccab-253">Issue: "Microsoft.Web.Infrastructure.dll cannot be loaded" error</span></span>

> <span data-ttu-id="cccab-254">Если вы ранее установили бета-версии 1 веб-страниц ASP.NET с синтаксисом Razor и затем установить версию бета-версии 3, все соответствующие сборки установлены в глобальном кэше СБОРОК, за исключением *Microsoft.Web.Infrastructure.dll*.</span><span class="sxs-lookup"><span data-stu-id="cccab-254">If you previously installed the Beta 1 version of ASP.NET Web Pages with Razor syntax and then install the Beta 3 version, all appropriate assemblies are installed in the GAC except *Microsoft.Web.Infrastructure.dll*.</span></span> <span data-ttu-id="cccab-255">Как следствие, при запуске ASP.NET Razor pages, появится сообщение об ошибке, которое указывает, что *Microsoft.Web.Infrastructure.dll* не может быть загружен.</span><span class="sxs-lookup"><span data-stu-id="cccab-255">As a consequence, when you run ASP.NET Razor pages, you see an error that indicates that *Microsoft.Web.Infrastructure.dll* could not be loaded.</span></span>
> 
> <span data-ttu-id="cccab-256">Эта проблема не возникает, если вы загрузили выпуск бета-версии 3 на чистом компьютере.</span><span class="sxs-lookup"><span data-stu-id="cccab-256">This issue does not occur if you loaded the Beta 3 release on a clean computer.</span></span>
> 
> <span data-ttu-id="cccab-257">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="cccab-257">**Workaround**</span></span>  
> <span data-ttu-id="cccab-258">В панели управления удалите веб-страниц ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="cccab-258">In Control Panel, uninstall ASP.NET Web Pages.</span></span> <span data-ttu-id="cccab-259">Затем переустановите бета-версии 3.</span><span class="sxs-lookup"><span data-stu-id="cccab-259">Then reinstall the Beta 3 release.</span></span>


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="cccab-260">Проблема: Удаление .NET Framework версии 4 отключает веб-страниц ASP.NET с синтаксисом Razor</span><span class="sxs-lookup"><span data-stu-id="cccab-260">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="cccab-261">Если удалить .NET Framework версии 4 и переустановите его, веб-страниц ASP.NET с синтаксисом Razor будет отключено.</span><span class="sxs-lookup"><span data-stu-id="cccab-261">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="cccab-262">Страницы с *.cshtml* расширения не выполняются правильно.</span><span class="sxs-lookup"><span data-stu-id="cccab-262">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="cccab-263">Веб-страницы ASP.NET регистрирует сборку в корневом каталоге компьютера *Web.config* файла и удаление .NET Framework удаляет этот файл.</span><span class="sxs-lookup"><span data-stu-id="cccab-263">ASP.NET Web Pages registers an assembly in the machine root *Web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="cccab-264">Повторная установка .NET Framework устанавливает новую версию файла конфигурации, но не добавляет ссылки для сборки веб-страниц ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="cccab-264">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="cccab-265">**Инструкции по решению** после повторной установки .NET Framework, переустановите ASP.NET Web Pages с синтаксисом Razor.</span><span class="sxs-lookup"><span data-stu-id="cccab-265">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="cccab-266">Это добавляет следующий элемент *Web.config* файл в корневой раздел компьютера, который обычно находится в следующем расположении:</span><span class="sxs-lookup"><span data-stu-id="cccab-266">This adds the following element to the *Web.config* file in the machine root, which is typically in the following location:</span></span>  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a><span data-ttu-id="cccab-267">Проблема: Ранее развернутое с использованием ASP.NET сборок в папке Bin приложения возникать ошибки</span><span class="sxs-lookup"><span data-stu-id="cccab-267">Issue: Applications previously deployed with ASP.NET assemblies in the Bin folder experience errors</span></span>

> <span data-ttu-id="cccab-268">Во время развертывания, копии сборок веб-страниц ASP.NET (например, *Microsoft.WebPages.dll*) для *Bin* папку веб-сайта на сервере.</span><span class="sxs-lookup"><span data-stu-id="cccab-268">During deployment, copies of the ASP.NET Web Pages assemblies (for example, *Microsoft.WebPages.dll*) to the *Bin* folder of the website on the server.</span></span> <span data-ttu-id="cccab-269">(Это может произойти автоматически во время развертывания или потому, что разработчик явно копии сборок.) Тем не менее при установке бета-версии 3, ошибки происходит, например ошибки, которые не удается найти определенные типы.</span><span class="sxs-lookup"><span data-stu-id="cccab-269">(This might have happened automatically during deployment or because the developer explicitly copied the assemblies.) However, when the Beta 3 release is installed, errors occurs, such as errors that certain types cannot be found.</span></span> <span data-ttu-id="cccab-270">Это происходит, поскольку несколько типов веб-страниц ASP.NET были перемещены в разных пространствах имен для бета-версии 3.</span><span class="sxs-lookup"><span data-stu-id="cccab-270">This occurs because a number of ASP.NET Web Pages types were moved into different namespaces for the Beta 3 release.</span></span>
> 
> <span data-ttu-id="cccab-271">**Инструкции по решению** </span><span class="sxs-lookup"><span data-stu-id="cccab-271">**Workaround** </span></span>  
> <span data-ttu-id="cccab-272">Очистить *Bin* папке развернутого приложения, скопируйте новые сборки в папку (или повторного развертывания приложения), а затем перезапустите приложение.</span><span class="sxs-lookup"><span data-stu-id="cccab-272">Clear the *Bin* folder of the deployed application, copy the new assemblies to the folder (or redeploy the application), and then restart the application.</span></span>


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="cccab-273">Проблема: URL-адреса без расширений приложения не найти файлы.cshtml/.vbhtml на IIS 7 или IIS 7.5</span><span class="sxs-lookup"><span data-stu-id="cccab-273">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="cccab-274">В IIS 7 или IIS 7.5, запросы на URL-адрес, как показано ниже не смогут найти страниц, включающих *.cshtml* или *.vbhtml* расширения:</span><span class="sxs-lookup"><span data-stu-id="cccab-274">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> <span data-ttu-id="cccab-275">Проблема возникает, так как переопределение URL-адресов не включена по умолчанию для IIS 7 или IIS 7.5.</span><span class="sxs-lookup"><span data-stu-id="cccab-275">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="cccab-276">Возможная ситуация не возникает проблема, при тестировании локально с помощью IIS Express, что возникают при развертывании веб-сайта для размещения веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="cccab-276">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="cccab-277">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="cccab-277">**Workaround**</span></span>
> 
> - <span data-ttu-id="cccab-278">Если у вас есть контроль над на компьютере сервера, на компьютере сервера установите обновление, описанное в [обновление доступно, что позволяет определенным обработчикам служб IIS 7.0 или IIS 7.5 обрабатывать запросы URL-адреса которых не оканчиваются точкой](https://support.microsoft.com/kb/980368).</span><span class="sxs-lookup"><span data-stu-id="cccab-278">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="cccab-279">Если у вас нет контроля над на компьютере сервера (например, развертывается для размещения веб-сайта), добавьте следующий код веб сайта *Web.config* файла:</span><span class="sxs-lookup"><span data-stu-id="cccab-279">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *Web.config* file:</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a><span data-ttu-id="cccab-280">Проблема: С помощью проекта веб-приложения или ASP.NET MVC и ASP.NET Web pages в одном приложении</span><span class="sxs-lookup"><span data-stu-id="cccab-280">Issue: Using Web Application Project or ASP.NET MVC and ASP.NET Web pages in the same application</span></span>

> <span data-ttu-id="cccab-281">При использовании веб-страниц ASP.NET в проекте веб-приложения или приложения ASP.NET MVC, может появиться ошибка, *WebPageHttpApplication* не удается найти.</span><span class="sxs-lookup"><span data-stu-id="cccab-281">If you were using ASP.NET Web Pages in a Web Application project or ASP.NET MVC application, you might see an error that *WebPageHttpApplication* cannot be found.</span></span>
> 
> <span data-ttu-id="cccab-282">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="cccab-282">**Workaround**</span></span>  
> <span data-ttu-id="cccab-283">Если вы получаете эту ошибку, измените базовый класс, от которого наследует приложения.</span><span class="sxs-lookup"><span data-stu-id="cccab-283">If you get this error, change the base class from which the application derives.</span></span> <span data-ttu-id="cccab-284">В *Global.asax* измените следующую строку:</span><span class="sxs-lookup"><span data-stu-id="cccab-284">In the *Global.asax* file, change the following line:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> <span data-ttu-id="cccab-285">На эту:</span><span class="sxs-lookup"><span data-stu-id="cccab-285">To this:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> <span data-ttu-id="cccab-286">Это фактически отменяет изменения, которая была введена для выпуска бета-версии 1 веб-страниц ASP.NET с синтаксисом Razor.</span><span class="sxs-lookup"><span data-stu-id="cccab-286">This in effect reverses a change that was introduced for the Beta 1 release of ASP.NET Web Pages with Razor syntax.</span></span>


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="cccab-287">Проблема: Развертывание приложения на компьютере, не SQL Server Compact установлена</span><span class="sxs-lookup"><span data-stu-id="cccab-287">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="cccab-288">Приложения, включающие базы данных SQL Server Compact можно запустить на компьютере, где SQL Server Compact не установлено.</span><span class="sxs-lookup"><span data-stu-id="cccab-288">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="cccab-289">Бета-версии 3 Microsoft WebMatrix автоматически копирует эти двоичные файлы для вас и выполняет соответствующий *Web.config* файл преобразования.</span><span class="sxs-lookup"><span data-stu-id="cccab-289">Microsoft WebMatrix Beta 3 automatically copies these binaries for you and performs the appropriate *Web.config* file transforms.</span></span>
> 
> <span data-ttu-id="cccab-290">**Инструкции по решению** Если вам нужно скопировать эти файлы и сделать *Web.config* изменения файлов вручную, выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="cccab-290">**Workaround** If you need to copy these files and make the *Web.config* file changes manually, do the following :</span></span>
> 
> 1. <span data-ttu-id="cccab-291">Скопируйте сборки ядра базы данных для *Bin* папки (и вложенных папок), приложения на целевом компьютере:</span><span class="sxs-lookup"><span data-stu-id="cccab-291">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span> 
> 
>     - <span data-ttu-id="cccab-292">Копировать *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **для** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="cccab-292">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **to** *\Bin*</span></span>
>     - <span data-ttu-id="cccab-293">Копировать *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\*\* **для** *\Bin\x86*</span><span class="sxs-lookup"><span data-stu-id="cccab-293">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\*\* **to** *\Bin\x86*</span></span>
>     - <span data-ttu-id="cccab-294">Копировать *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* **для** *\Bin\amd64*</span><span class="sxs-lookup"><span data-stu-id="cccab-294">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* **to** *\Bin\amd64*</span></span>
> 2. <span data-ttu-id="cccab-295">В корневой папке веб-сайта, создайте или откройте *Web.config* файл.</span><span class="sxs-lookup"><span data-stu-id="cccab-295">In the root folder of the website, create or open a *Web.config* file.</span></span> <span data-ttu-id="cccab-296">(В WebMatrix бета-версии 3, этот тип файлов доступна при нажатии кнопки **все** в **выберите тип файла** диалоговое окно.)</span><span class="sxs-lookup"><span data-stu-id="cccab-296">(In WebMatrix Beta 3, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="cccab-297">Добавьте следующий элемент в качестве дочернего элемента **&lt;конфигурации&gt;** элемент (не внутри **&lt;system.web&gt;** элемента):</span><span class="sxs-lookup"><span data-stu-id="cccab-297">Add the following element as a child of the **&lt;configuration&gt;** element (not inside the **&lt;system.web&gt;** element):</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="cccab-298">Проблема: Базы данных и WebGrid вспомогательные функции не работают в со средним уровнем доверия в Visual Basic</span><span class="sxs-lookup"><span data-stu-id="cccab-298">Issue: Database and WebGrid helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="cccab-299">Если вы используете Visual Basic (создание *.vbhtml* файлы), `Database` и `WebGrid` вспомогательные функции не будут работать, если приложение настроено для использования со средним уровнем доверия.</span><span class="sxs-lookup"><span data-stu-id="cccab-299">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="cccab-300">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="cccab-300">**Workaround**</span></span>  
> <span data-ttu-id="cccab-301">Временно настройте для приложения для использования полного доверия.</span><span class="sxs-lookup"><span data-stu-id="cccab-301">Temporarily set the application to use Full Trust.</span></span>

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a><span data-ttu-id="cccab-302">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="cccab-302">SQL Server Compact</span></span>

#### <a name="issue-encrypt-property-is-not-recognized"></a><span data-ttu-id="cccab-303">Проблема: Свойство «Шифрование» не распознан</span><span class="sxs-lookup"><span data-stu-id="cccab-303">Issue: "Encrypt" property is not recognized</span></span>

> <span data-ttu-id="cccab-304">SQL Server Compact 4.0 не распознает `Encrypt` свойство `SqlCeConnection` класса.</span><span class="sxs-lookup"><span data-stu-id="cccab-304">SQL Server Compact 4.0 does not recognize the `Encrypt` property of the `SqlCeConnection` class.</span></span> <span data-ttu-id="cccab-305">Это свойство не следует использовать для шифрования файлов базы данных.</span><span class="sxs-lookup"><span data-stu-id="cccab-305">You should not use this property to encrypt database files.</span></span> <span data-ttu-id="cccab-306">`Encrypt` Свойство устарела в SQL Server Compact 3.5 выпуск и сохранен только для обратной совместимости.</span><span class="sxs-lookup"><span data-stu-id="cccab-306">The `Encrypt` property was deprecated in SQL Server Compact 3.5 release and was retained only for backward compatibility.</span></span> 
> 
> <span data-ttu-id="cccab-307">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="cccab-307">**Workaround**</span></span>  
> <span data-ttu-id="cccab-308">Используйте `Encryption Mode` свойство `SqlCeConnection` класса для шифрования файлов базы данных SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="cccab-308">Use the `Encryption Mode` property of the `SqlCeConnection` class to encrypt SQL Server Compact 4.0 database files.</span></span> <span data-ttu-id="cccab-309">В следующем примере показано, как создать зашифрованные базы данных SQL Server Compact 4.0 с помощью `Encryption Mode` свойство:</span><span class="sxs-lookup"><span data-stu-id="cccab-309">The following example shows how to create an encrypted SQL Server Compact 4.0 database using the `Encryption Mode` property:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> <span data-ttu-id="cccab-310">Чтобы изменить режим шифрования существующей базы данных SQL Server Compact 4.0, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="cccab-310">To change the encryption mode of an existing SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> <span data-ttu-id="cccab-311">Чтобы выполнить шифрование незашифрованной базы данных SQL Server Compact 4.0, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="cccab-311">To encrypt an unencrypted SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a><span data-ttu-id="cccab-312">Проблема: Необходимы библиотеки времени выполнения Microsoft Visual C++ 2008</span><span class="sxs-lookup"><span data-stu-id="cccab-312">Issue: Microsoft Visual C++ 2008 runtime libraries are required</span></span>

> <span data-ttu-id="cccab-313">Собственные библиотеки DLL из SQL Server Compact 4.0, потребуется Microsoft Visual C++ 2008 Runtime Libraries (x 86, IA64 и x 64), пакетом обновления 1.</span><span class="sxs-lookup"><span data-stu-id="cccab-313">The native DLLs of SQL Server Compact 4.0 need the Microsoft Visual C++ 2008 Runtime Libraries (x86, IA64, and x64), Service Pack 1.</span></span>
> 
> <span data-ttu-id="cccab-314">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="cccab-314">**Workaround**</span></span>  
> <span data-ttu-id="cccab-315">Установите .NET Framework 3.5 SP1.</span><span class="sxs-lookup"><span data-stu-id="cccab-315">Install the .NET Framework 3.5 SP1.</span></span> <span data-ttu-id="cccab-316">При этом также устанавливаются SP1 библиотеки среды выполнения Visual C++ 2008.</span><span class="sxs-lookup"><span data-stu-id="cccab-316">This also installs the Visual C++ 2008 Runtime Libraries SP1.</span></span> <span data-ttu-id="cccab-317">Эти библиотеки можно загрузить из следующего расположения:</span><span class="sxs-lookup"><span data-stu-id="cccab-317">You can download the libraries from the following location:</span></span>   
>   
> [<span data-ttu-id="cccab-318">Обновление безопасности ATL для распространяемого пакета Microsoft Visual C++ 2008 с пакетом обновления 1</span><span class="sxs-lookup"><span data-stu-id="cccab-318">Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL Security Update</span></span>](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> <span data-ttu-id="cccab-319">Обратите внимание, что установка .NET Framework 2.0, 3.0, или 4 не *не* установить SP1 библиотеки среды выполнения Visual C++ 2008.</span><span class="sxs-lookup"><span data-stu-id="cccab-319">Note that installing the .NET Framework 2.0, 3.0, or 4 does *not* install the Visual C++ 2008 Runtime Libraries SP1.</span></span>


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a><span data-ttu-id="cccab-320">Проблема: Если перед установкой .NET Framework на компьютере установлен SQL Server Compact, его неизменяемое имя поставщика не зарегистрирован в файле machine.config .NET Framework</span><span class="sxs-lookup"><span data-stu-id="cccab-320">Issue: If SQL Server Compact is installed prior to installing .NET Framework on the computer, its provider invariant name is not registered in the .NET Framework machine.config file</span></span>

> <span data-ttu-id="cccab-321">SQL Server Compact можно установить на компьютере, который не поддерживает .NET Framework, установить, так как SQL Server Compact требуется .NET framework.</span><span class="sxs-lookup"><span data-stu-id="cccab-321">SQL Server Compact can be installed on a machine that does not have .NET Framework installed because SQL Server Compact does require the .NET framework.</span></span> <span data-ttu-id="cccab-322">Если ни .NET Framework версии 3.5 и 4 не установлена, перед установкой SQL Server Compact, Compact установки SQL Server не регистрирует его неизменяемое имя поставщика в *machine.config* файл.</span><span class="sxs-lookup"><span data-stu-id="cccab-322">If neither .NET Framework version 3.5 nor 4 is installed before you install SQL Server Compact, the SQL Server Compact Setup does not register its provider invariant name in the *machine.config* file.</span></span> <span data-ttu-id="cccab-323">Любое приложение, которое использует SQL Server Compact записи в *machine.config* файла завершится ошибкой.</span><span class="sxs-lookup"><span data-stu-id="cccab-323">Any application that relies on the SQL Server Compact entry in the *machine.config* file will fail.</span></span> <span data-ttu-id="cccab-324">Неизменяемое имя регистрационной записи в *machine.config* выглядит как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="cccab-324">The invariant name registration entry in *machine.config* looks like the following example:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> <span data-ttu-id="cccab-325">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="cccab-325">**Workaround**</span></span>  
> <span data-ttu-id="cccab-326">Удаление SQL Server Compact 4.0 CTP-версия 1.</span><span class="sxs-lookup"><span data-stu-id="cccab-326">Uninstall SQL Server Compact 4.0 CTP1.</span></span> <span data-ttu-id="cccab-327">Скачайте и установите полные версии платформы .NET Framework из следующего расположения:</span><span class="sxs-lookup"><span data-stu-id="cccab-327">Download and install the full versions of the .NET Framework from the following location:</span></span>
> 
> [<span data-ttu-id="cccab-328">Microsoft .NET Framework 3.5 пакетом обновления 1 (полный пакет)</span><span class="sxs-lookup"><span data-stu-id="cccab-328">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [<span data-ttu-id="cccab-329">Выпуск Microsoft .NET Framework 4.0 (полный пакет)</span><span class="sxs-lookup"><span data-stu-id="cccab-329">Microsoft .NET Framework 4.0 Release (Full Package)</span></span>](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> <span data-ttu-id="cccab-330">Затем переустановите [SQL Server Compact 4.0 CTP-версия 1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span><span class="sxs-lookup"><span data-stu-id="cccab-330">Then reinstall [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span></span>


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a><span data-ttu-id="cccab-331">Установка приложений</span><span class="sxs-lookup"><span data-stu-id="cccab-331">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="cccab-332">Проблема: При установке приложения может занять много времени, если папки «Мои документы» перенаправляется в общую сетевую папку</span><span class="sxs-lookup"><span data-stu-id="cccab-332">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="cccab-333">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="cccab-333">**Workaround**</span></span>  
> <span data-ttu-id="cccab-334">Отсутствует.</span><span class="sxs-lookup"><span data-stu-id="cccab-334">None.</span></span> <span data-ttu-id="cccab-335">Приложение может занять некоторое время, чтобы установить, но правильную установку.</span><span class="sxs-lookup"><span data-stu-id="cccab-335">The application might take a while to install, but will install correctly.</span></span>


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a><span data-ttu-id="cccab-336">Публикация приложений</span><span class="sxs-lookup"><span data-stu-id="cccab-336">Publishing Applications</span></span>

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="cccab-337">Проблема: Сайта может не работать после публикации, если поле «URL-адрес назначения» нет префикса http:// или https://</span><span class="sxs-lookup"><span data-stu-id="cccab-337">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="cccab-338">В **параметров публикации** диалоговое окно, если URL-адрес назначения не начинается с `http://` или `https://`, сайт может не работать после развертывания.</span><span class="sxs-lookup"><span data-stu-id="cccab-338">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="cccab-339">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="cccab-339">**Workaround**</span></span>  
> <span data-ttu-id="cccab-340">Убедитесь, что прежде чем опубликовать сайт, URL-адрес назначения в **параметры публикации** диалоговое окно начинается с `http://` или `https://`.</span><span class="sxs-lookup"><span data-stu-id="cccab-340">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="cccab-341">Проблема: Публикация базы данных MySQL завершается с ошибкой «не удалось опубликовать базу данных.</span><span class="sxs-lookup"><span data-stu-id="cccab-341">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="cccab-342">Это может произойти, если удаленная база данных нельзя запустить сценарий.»</span><span class="sxs-lookup"><span data-stu-id="cccab-342">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="cccab-343">Эта ошибка может возникнуть по ряду причин.</span><span class="sxs-lookup"><span data-stu-id="cccab-343">The error can occur for a number of reasons.</span></span> <span data-ttu-id="cccab-344">Одна из причин, можно увидеть, эта ошибка — Если скрипт базы данных одинарная кавычка (') и не может базы данных MySQL назначения по умолчанию используется кодировка UTF-8.</span><span class="sxs-lookup"><span data-stu-id="cccab-344">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="cccab-345">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="cccab-345">**Workaround**</span></span>  
> <span data-ttu-id="cccab-346">Задайте кодировку по умолчанию для удаленной базы данных MySQL в UTF-8.</span><span class="sxs-lookup"><span data-stu-id="cccab-346">Set the default character set for the remote MySQL database to UTF-8.</span></span>


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a><span data-ttu-id="cccab-347">Другие проблемы</span><span class="sxs-lookup"><span data-stu-id="cccab-347">Other Issues</span></span>

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a><span data-ttu-id="cccab-348">Проблема: Поиска и фильтрации не работает в отчетах для Group By: тип проблемы</span><span class="sxs-lookup"><span data-stu-id="cccab-348">Issue: Search/Filter does not work in Reports for Group By: Issue Type</span></span>

> <span data-ttu-id="cccab-349">При запуске отчета для узла, когда вы вводите текст в *фильтровать по URL-адрес* поле и нажмите кнопку *поиска*, ничего не происходит.</span><span class="sxs-lookup"><span data-stu-id="cccab-349">When you run a report for a site, if you enter text in the *Filter by URL* box and click *Search*, nothing happens.</span></span> <span data-ttu-id="cccab-350">Это, поскольку этот элемент управления не работает при *Group By* присваивается состояние отчета *тип проблемы*, который используется по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="cccab-350">This is because this control is not functional while the *Group By* state of the report is set to *Issue Type*, which is the default.</span></span>
> 
> <span data-ttu-id="cccab-351">**Инструкции по решению** в *Group By* вкладку ленты, нажмите кнопку *URL-адрес* для группировки записей по их URL-адрес источника.</span><span class="sxs-lookup"><span data-stu-id="cccab-351">**Workaround** In the *Group By* tab of ribbon, click *URL* to group the entries by their source URL.</span></span> <span data-ttu-id="cccab-352">Текстовое поле и кнопку для фильтрации записей в этом состоянии функциональна.</span><span class="sxs-lookup"><span data-stu-id="cccab-352">The text box and button to filter the entries are functional while in this state.</span></span>


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a><span data-ttu-id="cccab-353">Проблема: WCF приложения перестают работать с IIS Express</span><span class="sxs-lookup"><span data-stu-id="cccab-353">Issue: WCF applications fail to run with IIS Express</span></span>

> <span data-ttu-id="cccab-354">Перейдя к приложения WCF вызовет ошибку, аналогичный приведенному ниже:</span><span class="sxs-lookup"><span data-stu-id="cccab-354">Browsing to a WCF application results in an error like the following one:</span></span>
> 
> <span data-ttu-id="cccab-355">*Не удалось загрузить файл или сборку "Microsoft.Web.Administration, Version = 7.0.0.0, язык и региональные параметры = neutral, PublicKeyToken = 31bf3856ad364e35" или одну из ее зависимостей. Не удается найти указанный файл.*</span><span class="sxs-lookup"><span data-stu-id="cccab-355">*Could not load file or assembly 'Microsoft.Web.Administration, Version=7.0.0.0, Culture=neutral,PublicKeyToken=31bf3856ad364e35' or one of its dependencies. The system cannot find the file specified.*</span></span>
> 
> <span data-ttu-id="cccab-356">Это происходит, потому что выпуска IIS Express бета-версия не поддерживает WCF по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="cccab-356">This occurs because IIS Express Beta release doesn't support WCF by default.</span></span>
> 
> <span data-ttu-id="cccab-357">**Инструкции по решению** , воспользуйтесь одним из следующих решений (инструкции по решению #2 требуется Microsoft Windows Vista или более поздней версии):</span><span class="sxs-lookup"><span data-stu-id="cccab-357">**Workaround** Use any one of the following workarounds (workaround #2 requires Microsoft Windows Vista or higher):</span></span>
> 
> 
> 1. <span data-ttu-id="cccab-358">Копировать *Microsoft.Web.dll* и *файл Microsoft.Web.Administration.dll* сборки из папки установки WebMatrix, чтобы *bin* каталог WCF приложение.</span><span class="sxs-lookup"><span data-stu-id="cccab-358">Copy the *Microsoft.Web.dll* and *Microsoft.Web.Administration.dll* assemblies from the WebMatrix installation location to the *bin* directory of the WCF application.</span></span> <span data-ttu-id="cccab-359">По умолчанию, WebMatrix устанавливается в *Microsoft WebMatrix* вложенной папке системы *Program Files* папки.</span><span class="sxs-lookup"><span data-stu-id="cccab-359">By default, WebMatrix is installed in the *Microsoft WebMatrix* subfolder under the system's *Program Files* folder.</span></span>
> 2. <span data-ttu-id="cccab-360">В Microsoft Windows Vista или более поздней версии, создайте символьную ссылку на сборки в *bin* каталог, используя следующие команды.</span><span class="sxs-lookup"><span data-stu-id="cccab-360">On Microsoft Windows Vista or higher, create a symlink to the assemblies in the *bin* directory using the following commands.</span></span> <span data-ttu-id="cccab-361">(Данный подход имеет преимущество, что он не создает копию сборки).</span><span class="sxs-lookup"><span data-stu-id="cccab-361">(This approach has the advantage that it does not create a copy of the assemblies.)</span></span>
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. <span data-ttu-id="cccab-362">Установите две сборки в глобальном кэше СБОРОК.</span><span class="sxs-lookup"><span data-stu-id="cccab-362">Install the two assemblies in the GAC.</span></span> <span data-ttu-id="cccab-363">С повышенными привилегиями выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="cccab-363">From an elevated prompt, run the following commands:</span></span>
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="cccab-364">Проблема: Не удается выполнить определенные задачи, требующие повышения прав WebMatrix бета-версия 3</span><span class="sxs-lookup"><span data-stu-id="cccab-364">Issue: WebMatrix Beta 3 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="cccab-365">WebMatrix бета-версии 3 не может выполнить определенные задачи, требующие повышения прав, таких как установка дополнительных компонентов в следующих ситуациях:</span><span class="sxs-lookup"><span data-stu-id="cccab-365">WebMatrix Beta 3 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="cccab-366">В Windows Vista или Windows 7 вы выполняете вход с использованием учетной записи, которая не имеет прав администратора и контроля учетных записей (UAC) отключено.</span><span class="sxs-lookup"><span data-stu-id="cccab-366">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="cccab-367">При использовании Microsoft Windows XP или Microsoft Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="cccab-367">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="cccab-368">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="cccab-368">**Workaround**</span></span>  
> <span data-ttu-id="cccab-369">Большинство задач в WebMatrix бета-версии 3 не требуют прав администратора.</span><span class="sxs-lookup"><span data-stu-id="cccab-369">Most tasks in WebMatrix Beta 3 do not require administrative permission.</span></span> <span data-ttu-id="cccab-370">Для тех, которые можно выполнить операцию с правами администратора или выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="cccab-370">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="cccab-371">В Windows Vista или Windows 7 включите контроль учетных Записей.</span><span class="sxs-lookup"><span data-stu-id="cccab-371">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="cccab-372">В Windows XP добавьте пользователя в группу безопасности администраторов.</span><span class="sxs-lookup"><span data-stu-id="cccab-372">On Windows XP, add the user to the Administrators security group.</span></span>


#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="cccab-373">Проблема: «Site from Web Gallery» отключена</span><span class="sxs-lookup"><span data-stu-id="cccab-373">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="cccab-374">**Site from Web Gallery** параметр отключен, если не установлен установщик веб-платформы 3.0.</span><span class="sxs-lookup"><span data-stu-id="cccab-374">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="cccab-375">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="cccab-375">**Workaround**</span></span>  
> <span data-ttu-id="cccab-376">Установка [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="cccab-376">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a><span data-ttu-id="cccab-377">Проблема: В Windows Server 2003, IIS Express не запускается для обычных пользователей</span><span class="sxs-lookup"><span data-stu-id="cccab-377">Issue: On Windows Server 2003, IIS Express does not start for a non-administrative user</span></span>

> <span data-ttu-id="cccab-378">В Windows Server 2003 при открывает страницу или запуске IIS Express и IIS Express не запускается.</span><span class="sxs-lookup"><span data-stu-id="cccab-378">On Windows Server 2003, when you launch a page or start IIS Express, IIS Express does not start.</span></span> <span data-ttu-id="cccab-379">Для веб-страниц выводится сообщение об ошибке, указывающее, что приложения был запущен пользователем без прав администратора.</span><span class="sxs-lookup"><span data-stu-id="cccab-379">For Web pages, an error is displayed that indicates that the application has been started by a non-administrative user.</span></span>
> 
> <span data-ttu-id="cccab-380">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="cccab-380">**Workaround**</span></span>  
> <span data-ttu-id="cccab-381">Запуск WebMatrix бета-версии 3, пользователь с правами администратора.</span><span class="sxs-lookup"><span data-stu-id="cccab-381">Start WebMatrix Beta 3 as an administrative user.</span></span> <span data-ttu-id="cccab-382">Дополнительные сведения см. в следующей статье базы знаний:</span><span class="sxs-lookup"><span data-stu-id="cccab-382">For more details, see the following KnowledgeBase article:</span></span>  
>   
> [<span data-ttu-id="cccab-383">Приложение, которое запускается с пользователь без прав администратора не может прослушивать HTTP-трафика компьютера, на котором приложение выполняется в Windows Vista, Windows Server 2003 или Windows XP.</span><span class="sxs-lookup"><span data-stu-id="cccab-383">An application that is started by a non-administrative user cannot listen to the HTTP traffic of the computer on which the application is running in Windows Vista, Windows Server 2003, or Windows XP.</span></span>](https://support.microsoft.com/kb/939786)


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="cccab-384">Проблема: Google Chrome не поддерживается в случае выполнения</span><span class="sxs-lookup"><span data-stu-id="cccab-384">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="cccab-385">Google Chrome не отображается в списке браузеров в разделе **запуска** на **Главная** вкладки.</span><span class="sxs-lookup"><span data-stu-id="cccab-385">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="cccab-386">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="cccab-386">**Workaround**</span></span>  
> <span data-ttu-id="cccab-387">В некоторых версиях Google Chrome не регистрируют себя правильно с функцией программы по умолчанию в Windows.</span><span class="sxs-lookup"><span data-stu-id="cccab-387">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="cccab-388">Обойти это ограничение, Google Chrome, выберите пункт *Настройка и управление Google Chrome* меню, щелкните *параметры*, а затем нажмите кнопку *марки Google Chrome обозревателем по умолчанию*.</span><span class="sxs-lookup"><span data-stu-id="cccab-388">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="cccab-389">Проблема: Диалоговое окно «Foreign Key» не позволяет ввести первичный ключ</span><span class="sxs-lookup"><span data-stu-id="cccab-389">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="cccab-390">**Foreign Key** диалоговое окно не позволяет ввести имя первичного ключа из таблицы первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="cccab-390">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="cccab-391">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="cccab-391">**Workaround**</span></span>  
> <span data-ttu-id="cccab-392">Это сделано намеренно.</span><span class="sxs-lookup"><span data-stu-id="cccab-392">This is intentional.</span></span> <span data-ttu-id="cccab-393">Необходимо ввести имя первичного ключа из таблицы первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="cccab-393">You do not need to enter the name of the primary key from the primary key table.</span></span>


#### <a name="issue-the-relationships-button-is-disabled"></a><span data-ttu-id="cccab-394">Проблема: Кнопка «Связи» отключена</span><span class="sxs-lookup"><span data-stu-id="cccab-394">Issue: The "Relationships" button is disabled</span></span>

> <span data-ttu-id="cccab-395">**Связи** под кнопкой **таблицы** вкладке **баз данных** рабочей области отключен для базы данных SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="cccab-395">The **Relationships** button under the **Table** tab in the **Databases** workspace is disabled for SQL Server Compact databases.</span></span>
> 
> <span data-ttu-id="cccab-396">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="cccab-396">**Workaround**</span></span>  
> <span data-ttu-id="cccab-397">Отсутствует.</span><span class="sxs-lookup"><span data-stu-id="cccab-397">None.</span></span> <span data-ttu-id="cccab-398">SQL Server Compact не поддерживает связи между таблицами.</span><span class="sxs-lookup"><span data-stu-id="cccab-398">SQL Server Compact does not support relationships between tables.</span></span>


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a><span data-ttu-id="cccab-399">Проблема: Параметризованные SQL-запросы исключения</span><span class="sxs-lookup"><span data-stu-id="cccab-399">Issue: Parameterized SQL queries throw exceptions</span></span>

> <span data-ttu-id="cccab-400">В SQL Server Compact 4.0, если вы не укажете тип данных таких как `SqlDbType` или `DbType` для параметров в параметризованных запросах, создается исключение при выполнении запроса.</span><span class="sxs-lookup"><span data-stu-id="cccab-400">In SQL Server Compact 4.0, if you do not specify a data type such as `SqlDbType` or `DbType` for parameters in parameterized queries, an exception is thrown when the query runs.</span></span>
> 
> <span data-ttu-id="cccab-401">**Инструкции по решению**</span><span class="sxs-lookup"><span data-stu-id="cccab-401">**Workaround**</span></span>  
> <span data-ttu-id="cccab-402">Явно задать тип данных для параметров, таких как `SqlDbType` или `DbType`.</span><span class="sxs-lookup"><span data-stu-id="cccab-402">Explicitly set the data type for parameters such as `SqlDbType` or `DbType`.</span></span> <span data-ttu-id="cccab-403">Это крайне важно в случае с типами данных больших двоичных ОБЪЕКТОВ (`image` и `ntext`).</span><span class="sxs-lookup"><span data-stu-id="cccab-403">This is critical in the case of BLOB data types (`image` and `ntext`).</span></span> <span data-ttu-id="cccab-404">Используйте код, аналогичный следующему:</span><span class="sxs-lookup"><span data-stu-id="cccab-404">Use code like the following:</span></span>
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="cccab-405">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="cccab-405">For More Information</span></span>

<span data-ttu-id="cccab-406">Дополнительные сведения о WebMatrix бета-версии 3 см. в разделе следующих веб-сайтах:</span><span class="sxs-lookup"><span data-stu-id="cccab-406">For more information about WebMatrix Beta 3, see the following websites:</span></span>

- [<span data-ttu-id="cccab-407">IIS.net</span><span class="sxs-lookup"><span data-stu-id="cccab-407">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="cccab-408">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cccab-408">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="cccab-409">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="cccab-409">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

* * *

<span data-ttu-id="cccab-410">© 2010 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="cccab-410">© 2010 Microsoft Corporation.</span></span> <span data-ttu-id="cccab-411">Все права защищены.</span><span class="sxs-lookup"><span data-stu-id="cccab-411">All Rights Reserved.</span></span> <span data-ttu-id="cccab-412">[Условия использования](https://msdn.microsoft.cos/cc300389.aspx).</span><span class="sxs-lookup"><span data-stu-id="cccab-412">[Terms of Use](https://msdn.microsoft.cos/cc300389.aspx).</span></span>
