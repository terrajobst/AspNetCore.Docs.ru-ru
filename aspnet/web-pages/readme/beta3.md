---
uid: web-pages/readme/beta3
title: Веб-матрицы и ASP.NET Web Pages (Razor) о бета-версии 3 версии | Документы Microsoft
author: rick-anderson
description: Web Matrix и ASP.NET Web страницы (Razor) о бета-версии 3 выпуска
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/10/2011
ms.topic: article
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: 5ef7a6f44758cf94fc19d6fbab3cc4b7bce8e8e5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a><span data-ttu-id="8351b-103">Web Matrix и ASP.NET Web страницы (Razor) о бета-версии 3 выпуска</span><span class="sxs-lookup"><span data-stu-id="8351b-103">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>
====================
> <span data-ttu-id="8351b-104">Web Matrix и ASP.NET Web страницы (Razor) о бета-версии 3 выпуска</span><span class="sxs-lookup"><span data-stu-id="8351b-104">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>

<span data-ttu-id="8351b-105">9 ноября 2010 г.</span><span class="sxs-lookup"><span data-stu-id="8351b-105">9 November 2010</span></span>

## <a name="contents"></a><span data-ttu-id="8351b-106">Описание</span><span class="sxs-lookup"><span data-stu-id="8351b-106">Contents</span></span>

- [<span data-ttu-id="8351b-107">Обзор набора средств Visual Studio для Unity</span><span class="sxs-lookup"><span data-stu-id="8351b-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="8351b-108">Установка</span><span class="sxs-lookup"><span data-stu-id="8351b-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="8351b-109">Новые возможности, изменения и известные проблемы в бета-версии 3</span><span class="sxs-lookup"><span data-stu-id="8351b-109">New Features, Changes, and Known Issues in the Beta 3 release</span></span>](#Known_Issues)

    - [<span data-ttu-id="8351b-110">Проблемы с установкой WebMatrix</span><span class="sxs-lookup"><span data-stu-id="8351b-110">WebMatrix Installation Issues</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="8351b-111">Веб-страницы ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8351b-111">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="8351b-112">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="8351b-112">SQL Server Compact</span></span>](#Known_Issues_SQL_Server_Compact)
    - [<span data-ttu-id="8351b-113">Установка приложений</span><span class="sxs-lookup"><span data-stu-id="8351b-113">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="8351b-114">Публикация приложений</span><span class="sxs-lookup"><span data-stu-id="8351b-114">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
    - [<span data-ttu-id="8351b-115">Другие проблемы</span><span class="sxs-lookup"><span data-stu-id="8351b-115">Other Issues</span></span>](#Known_Issues_Other_Issues)
- [<span data-ttu-id="8351b-116">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="8351b-116">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="8351b-117">Обзор</span><span class="sxs-lookup"><span data-stu-id="8351b-117">Overview</span></span>

> <span data-ttu-id="8351b-118">Бета-версия Microsoft WebMatrix представляет собой стек бесплатной разработки, который устанавливается в минутах.</span><span class="sxs-lookup"><span data-stu-id="8351b-118">Microsoft WebMatrix Beta is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="8351b-119">Она интегрирует веб-сервера базы данных и платформами программирования для создания единой, интегрированной среды.</span><span class="sxs-lookup"><span data-stu-id="8351b-119">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="8351b-120">Можно использовать для упрощения разработки кода, тестирования и публикация собственных ASP.NET или PHP, веб-сайт WebMatrix бета-версии или бета-версии WebMatrix можно использовать для создания нового веб-сайтов с помощью таких популярных приложений с открытым исходным кодом как DotNetNuke, Umbraco, WordPress и Joomla.</span><span class="sxs-lookup"><span data-stu-id="8351b-120">You can use WebMatrix Beta to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix Beta to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="8351b-121">Бета-версии WebMatrix использует же мощный веб-сервер, СУБД и платформы среды выполнения веб-сайта в Интернете, что упрощает и ускоряет переход из среды разработки в рабочей среде smooth и удобный.</span><span class="sxs-lookup"><span data-stu-id="8351b-121">WebMatrix Beta uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>


<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="8351b-122">Установка</span><span class="sxs-lookup"><span data-stu-id="8351b-122">Installation</span></span>

> <span data-ttu-id="8351b-123">Чтобы установить WebMatrix бета-версии 3, используйте [платформы установщика Microsoft Web 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="8351b-123">To install WebMatrix Beta 3, you use [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="8351b-124">После установки установщика веб-платформы, его можно использовать для установки WebMatrix бета-версии 3.</span><span class="sxs-lookup"><span data-stu-id="8351b-124">After you've installed the Web Platform Installer, you can use it to install WebMatrix Beta 3.</span></span>
> 
> <span data-ttu-id="8351b-125">Если во время установки возникают проблемы, см. [Устранение неполадок с Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span><span class="sxs-lookup"><span data-stu-id="8351b-125">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a><span data-ttu-id="8351b-126">Инструкции по публикации приложения</span><span class="sxs-lookup"><span data-stu-id="8351b-126">Instructions for Publishing Applications</span></span>

> <span data-ttu-id="8351b-127">В разделе [пошаговые инструкции для публикации приложений](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="8351b-127">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a><span data-ttu-id="8351b-128">Новые возможности, изменения, andKnown проблемы</span><span class="sxs-lookup"><span data-stu-id="8351b-128">New Features, Changes, andKnown Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a><span data-ttu-id="8351b-129">WebMatrix бета-версии 3</span><span class="sxs-lookup"><span data-stu-id="8351b-129">WebMatrix Beta 3 Installation</span></span>

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="8351b-130">Проблема: WebMatrix бета-версия 3 доступна только на платформах, поддерживающих Microsoft .NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="8351b-130">Issue: WebMatrix Beta 3 is only available on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="8351b-131">.NET Framework версии 4 является обязательным для WebMatrix бета-версии.</span><span class="sxs-lookup"><span data-stu-id="8351b-131">The .NET Framework version 4 is required for WebMatrix Beta.</span></span> <span data-ttu-id="8351b-132">В некоторых случаях установщик бета-версии WebMatrix позволит попытается установить на платформу, которая не является частью набора поддерживаемых конфигураций.</span><span class="sxs-lookup"><span data-stu-id="8351b-132">In certain cases, the WebMatrix Beta installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="8351b-133">В частности Windows Vista без обновления SP1 позволит вам начать установку WebMatrix бета-версии, но компонент .NET Framework 4 не удастся и блокировать установку.</span><span class="sxs-lookup"><span data-stu-id="8351b-133">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix Beta, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="8351b-134">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="8351b-134">**Workaround**</span></span>  
> <span data-ttu-id="8351b-135">Установите на поддерживаемых платформах, включая:</span><span class="sxs-lookup"><span data-stu-id="8351b-135">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="8351b-136">Windows 7</span><span class="sxs-lookup"><span data-stu-id="8351b-136">Windows 7</span></span>
> - <span data-ttu-id="8351b-137">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="8351b-137">Windows Server 2008</span></span>
> - <span data-ttu-id="8351b-138">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="8351b-138">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="8351b-139">Windows Vista с пакетом обновления 1 (SP1) или выше</span><span class="sxs-lookup"><span data-stu-id="8351b-139">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="8351b-140">Windows XP с пакетом обновления 3 (SP3)</span><span class="sxs-lookup"><span data-stu-id="8351b-140">Windows XP SP3</span></span>
> - <span data-ttu-id="8351b-141">Windows Server 2003 с пакетом обновления 2 (SP2)</span><span class="sxs-lookup"><span data-stu-id="8351b-141">Windows Server 2003 SP2</span></span>


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="8351b-142">Проблема: Не удается установить WebMatrix бета-версии 3, если Microsoft Visual Studio 2008 устанавливается без Microsoft Visual Studio 2008 с пакетом обновления 1</span><span class="sxs-lookup"><span data-stu-id="8351b-142">Issue: Cannot install WebMatrix Beta 3 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="8351b-143">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="8351b-143">**Workaround**</span></span>  
> <span data-ttu-id="8351b-144">Установка [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) из центра загрузки Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="8351b-144">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="8351b-145">Проблема: Некоторые сборки для SQL Server Compact 4.0 не установлены в глобальном кэше СБОРОК</span><span class="sxs-lookup"><span data-stu-id="8351b-145">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="8351b-146">Управляемые сборки для SQL Server Compact 4.0 не помещается в глобальный кэш сборок (GAC), при установке SQL Server Compact 4.0 на 64-разрядном компьютере и что компьютер имеет только профиле .NET Framework 3.5 с пакетом обновления 1 клиента установлен.</span><span class="sxs-lookup"><span data-stu-id="8351b-146">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="8351b-147">Управляемые сборки, которые не установлены в глобальном кэше СБОРОК являются:</span><span class="sxs-lookup"><span data-stu-id="8351b-147">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="8351b-148">*System.Data.SqlServerCe.dll* (поставщика ADO.NET)</span><span class="sxs-lookup"><span data-stu-id="8351b-148">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="8351b-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)</span><span class="sxs-lookup"><span data-stu-id="8351b-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="8351b-150">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="8351b-150">**Workaround**</span></span>  
> <span data-ttu-id="8351b-151">Удаление SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="8351b-151">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="8351b-152">Загрузить и установить полную версию .NET Framework 3.5 с пакетом обновления 1 из следующего расположения:</span><span class="sxs-lookup"><span data-stu-id="8351b-152">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="8351b-153">Microsoft .NET Framework 3.5 пакетом обновления 1 (полный пакет)</span><span class="sxs-lookup"><span data-stu-id="8351b-153">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="8351b-154">Затем переустановите SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="8351b-154">Then reinstall SQL Server Compact 4.0.</span></span>


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="8351b-155">Проблема: Не удается удалить SQL Server Compact с помощью командной строки</span><span class="sxs-lookup"><span data-stu-id="8351b-155">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="8351b-156">Удаление SQL Server Compact через параметры командной строки не работает в данном выпуске.</span><span class="sxs-lookup"><span data-stu-id="8351b-156">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="8351b-157">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="8351b-157">**Workaround**</span></span>  
> <span data-ttu-id="8351b-158">Используйте *программы и компоненты* в панели управления Windows, чтобы удалить Microsoft SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="8351b-158">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="8351b-159">Веб-страницы ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8351b-159">ASP.NET Web Pages</span></span>

<span data-ttu-id="8351b-160">Этот раздел документа описывает новые возможности, изменения и известные проблемы с бета-версии 3 веб-страниц ASP.NET с синтаксисом Razor.</span><span class="sxs-lookup"><span data-stu-id="8351b-160">This section of the document describes new features, changes, and known issues with the Beta 3 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="8351b-161">Новые возможности</span><span class="sxs-lookup"><span data-stu-id="8351b-161">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="8351b-162">Изменения</span><span class="sxs-lookup"><span data-stu-id="8351b-162">Changes</span></span>](#Changes)
- [<span data-ttu-id="8351b-163">Проблемы</span><span class="sxs-lookup"><span data-stu-id="8351b-163">Issues</span></span>](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="8351b-164">Новые возможности в бета-версии 3 для веб-страниц ASP.NET с синтаксисом Razor</span><span class="sxs-lookup"><span data-stu-id="8351b-164">New Features in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a><span data-ttu-id="8351b-165">Новинка! «Html.Raw» метод выводит разметку без кодировки</span><span class="sxs-lookup"><span data-stu-id="8351b-165">New: "Html.Raw" method renders unencoded markup</span></span>

> <span data-ttu-id="8351b-166">Новый `Html.Raw` метод позволяет отобразить HTML-разметку как разметку вместо отображения закодированную выходную.</span><span class="sxs-lookup"><span data-stu-id="8351b-166">The new `Html.Raw` method lets you render HTML markup as markup instead of rendering encoded output.</span></span> <span data-ttu-id="8351b-167">(По умолчанию ASP.NET Razor кодирует строки перед их отображением.) Синтаксис выглядит следующим образом.</span><span class="sxs-lookup"><span data-stu-id="8351b-167">(By default, ASP.NET Razor encodes strings before rendering them.) The syntax is:</span></span>
> 
> `Html.Raw(value)`
> 
> <span data-ttu-id="8351b-168">В следующем примере показано использование `Html.Raw`.</span><span class="sxs-lookup"><span data-stu-id="8351b-168">The following example shows how to use `Html.Raw`:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="8351b-169">Изменения в бета-версии 3 для веб-страниц ASP.NET с синтаксисом Razor</span><span class="sxs-lookup"><span data-stu-id="8351b-169">Changes in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="change-hrefattribute-method-removed"></a><span data-ttu-id="8351b-170">Изменения: Удалить метод «HrefAttribute»</span><span class="sxs-lookup"><span data-stu-id="8351b-170">Change: "HrefAttribute" method removed</span></span>

> <span data-ttu-id="8351b-171">`HrefAttribute` Метод `WebPage` класс был удален.</span><span class="sxs-lookup"><span data-stu-id="8351b-171">The `HrefAttribute` method of the `WebPage` class has been removed.</span></span> <span data-ttu-id="8351b-172">Этот вспомогательный была использована для преобразования небезопасных символов в URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="8351b-172">This helper was used to encode unsafe characters in URLs.</span></span> <span data-ttu-id="8351b-173">Он больше не требуется, так как ASP.NET Razor автоматически кодирует строки.</span><span class="sxs-lookup"><span data-stu-id="8351b-173">It is no longer required because ASP.NET Razor automatically encodes strings.</span></span> <span data-ttu-id="8351b-174">(Используйте новую `Html.Raw` метод для обработки строк без кодировки.)</span><span class="sxs-lookup"><span data-stu-id="8351b-174">(Use the new `Html.Raw` method to render unencoded strings.)</span></span>


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a><span data-ttu-id="8351b-175">Изменение: Синтаксис для декларативной "@helper" Изменить вспомогательные методы</span><span class="sxs-lookup"><span data-stu-id="8351b-175">Change: Syntax for declarative "@helper" helpers changed</span></span>

> <span data-ttu-id="8351b-176">В бета-версии 3, ASP.NET изменяет анализ вспомогательные методы, которые создаются с помощью `@helper` синтаксиса.</span><span class="sxs-lookup"><span data-stu-id="8351b-176">In the Beta 3 release, ASP.NET changes how it parses helpers that are created using the `@helper` syntax.</span></span> <span data-ttu-id="8351b-177">По сути `@helper` синтаксис теперь анализируется как блок кода, а не как блок разметку, которая может включать код.</span><span class="sxs-lookup"><span data-stu-id="8351b-177">In essence, the `@helper` syntax is now parsed as a code block instead of as a block of markup that can include code.</span></span> <span data-ttu-id="8351b-178">Таким образом, код внутри модуля поддержки не нужно заключать в `@{ }` блоков.</span><span class="sxs-lookup"><span data-stu-id="8351b-178">Therefore, code inside the helper does not need to be enclosed in `@{ }` blocks.</span></span> <span data-ttu-id="8351b-179">И наоборот, должно быть явно включено, в элементы HTML или ASP.NET Razor разметки внутри модуля поддержки `<text></text>` тегов.</span><span class="sxs-lookup"><span data-stu-id="8351b-179">Conversely, markup inside the helper has to be explicitly included in HTML elements or in ASP.NET Razor `<text></text>` tags.</span></span>
> 
> <span data-ttu-id="8351b-180">Например, следующая `@helper` синтаксис работает в бета-версии 3:</span><span class="sxs-lookup"><span data-stu-id="8351b-180">For example, the following `@helper` syntax works in the Beta 3 release:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> <span data-ttu-id="8351b-181">В бета-версии 3 необходимо изменить этого вспомогательного объекта должен выглядеть как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="8351b-181">In the Beta 3 release, this helper must be changed to look like the following example:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> <span data-ttu-id="8351b-182">Обратите внимание, что `@{ }` символы во вспомогательном методе исходный код больше не используется.</span><span class="sxs-lookup"><span data-stu-id="8351b-182">Notice that the `@{ }` characters around the initial code in the helper is no longer used.</span></span> <span data-ttu-id="8351b-183">Это так, как содержимое помощников рассматривается как блок кода по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="8351b-183">This is because the contents of the helpers are treated as a code block by default.</span></span> <span data-ttu-id="8351b-184">Вспомогательный объект отображает разметку, начинающуюся с открывающей `<a>` тег.</span><span class="sxs-lookup"><span data-stu-id="8351b-184">The helper renders markup, which starts with the opening `<a>` tag.</span></span> <span data-ttu-id="8351b-185">Если вспомогательный объект должен представлять обычного текста или меток, которые будут отсутствовать закрывающий тег (например, `<meta>` тегов), должен быть содержимое для отображения в `<text></text>` тегов.</span><span class="sxs-lookup"><span data-stu-id="8351b-185">If the helper must render plain text or tags that do not include a closing tag (for example, `<meta>` tags), the content to be rendered must be in `<text></text>` tags.</span></span>


#### <a name="change-webpagecontexthttpcontext-removed"></a><span data-ttu-id="8351b-186">Изменений: Удаление «WebPageContext.HttpContext»</span><span class="sxs-lookup"><span data-stu-id="8351b-186">Change: "WebPageContext.HttpContext" removed</span></span>

> <span data-ttu-id="8351b-187">`WebPageContext.HttpContext` Было удалено.</span><span class="sxs-lookup"><span data-stu-id="8351b-187">The `WebPageContext.HttpContext` property has been removed.</span></span> <span data-ttu-id="8351b-188">Взамен рекомендуется использовать `HttpContext.Current`.</span><span class="sxs-lookup"><span data-stu-id="8351b-188">Use `HttpContext.Current` instead.</span></span> <span data-ttu-id="8351b-189">( `WebPageContext.HttpContext` Это просто оболочке свойство.)</span><span class="sxs-lookup"><span data-stu-id="8351b-189">(The `WebPageContext.HttpContext` property simply wrapped this.)</span></span>


#### <a name="change-facebook-helper-moved-to-new-package"></a><span data-ttu-id="8351b-190">Изменение: Переместить в новый пакет вспомогательный «Facebook»</span><span class="sxs-lookup"><span data-stu-id="8351b-190">Change: "Facebook" helper moved to new package</span></span>

> <span data-ttu-id="8351b-191">`Facebook` Вспомогательный был перемещен в *Facebook.Helper* библиотеку, которая включает в себя `Facebook` вспомогательный метод и дополнительные функциональные возможности.</span><span class="sxs-lookup"><span data-stu-id="8351b-191">The `Facebook` helper has been moved to the *Facebook.Helper* library, which includes the `Facebook` helper and additional functionality.</span></span> <span data-ttu-id="8351b-192">Необходимо установить эту библиотеку как отдельный пакет, как описано в «Установка вспомогательных методов с помощью диспетчера пакетов» учебника [Приступая к работе со страницами ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202889).</span><span class="sxs-lookup"><span data-stu-id="8351b-192">You must install this library as a separate package, as described in "Installing Helpers with Package Manager" in the tutorial [Getting Started with ASP.NET Pages](https://go.microsoft.com/fwlink/?LinkId=202889).</span></span>


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a><span data-ttu-id="8351b-193">Изменение: Членство, роли и безопасность типов перемещает новую сборку</span><span class="sxs-lookup"><span data-stu-id="8351b-193">Change: Membership, Role, and Security types moves to new assembly</span></span>

> <span data-ttu-id="8351b-194">Следующие типы были перемещены в `WebMatrix.WebData` сборки:</span><span class="sxs-lookup"><span data-stu-id="8351b-194">The following types were moved to the `WebMatrix.WebData` assembly:</span></span>
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a><span data-ttu-id="8351b-195">: Класс «TagBuilder» была перемещена на сборку System.Web.WebPages.dll</span><span class="sxs-lookup"><span data-stu-id="8351b-195">Change: "TagBuilder" class moved to System.Web.WebPages.dll assembly</span></span>

> <span data-ttu-id="8351b-196">`TagBuilde` r класса был перемещен в сборку System.Web.WebPages.dll.</span><span class="sxs-lookup"><span data-stu-id="8351b-196">The `TagBuilde` r class has been moved to the System.Web.WebPages.dll assembly.</span></span> <span data-ttu-id="8351b-197">До настоящего времени в сборке, которая входила в состав платформы ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8351b-197">Previously, this was in an assembly that was part of ASP.NET MVC.</span></span> <span data-ttu-id="8351b-198">Это изменение означает, что не понадобится устанавливать ASP.NET MVC для использования `TagBuilder` класса.</span><span class="sxs-lookup"><span data-stu-id="8351b-198">This change means that you do not have to install ASP.NET MVC in order to use the `TagBuilder` class.</span></span>
> 
> <span data-ttu-id="8351b-199">Однако по-прежнему в класс является `System.Web.Mvc` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="8351b-199">However, the class is still in the `System.Web.Mvc` namespace.</span></span> <span data-ttu-id="8351b-200">Чтобы использовать `TagBuilder` класса (например, в пользовательских ASP.NET Razor вспомогательный) необходимо сослаться на пространство имен (например, путем добавления `@using System.Web.Mvc` в код).</span><span class="sxs-lookup"><span data-stu-id="8351b-200">In order to use the `TagBuilder` class (for example, in a custom ASP.NET Razor helper), you must reference the namespace (for example, by adding `@using System.Web.Mvc` to your code).</span></span>


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a><span data-ttu-id="8351b-201">Изменение: Запрос синтаксис проверки изменены; Удалить класс «Проверка»</span><span class="sxs-lookup"><span data-stu-id="8351b-201">Change: Request validation syntax changed; "Validation" class removed</span></span>

> <span data-ttu-id="8351b-202">В бета-версии 3, чтобы отключить проверку для отдельных полей или набор полей, можно вызвать `Validation.Exclude` метод, передавая имя или имена полей, которые необходимо исключить из проверки.</span><span class="sxs-lookup"><span data-stu-id="8351b-202">In the Beta 3 release, to disable validation for an individual field or set of fields, you can call the `Validation.Exclude` method, passing in the name or names of the fields to exclude from validation.</span></span> <span data-ttu-id="8351b-203">Новый синтаксис доступен в бета-версии 3 для обхода проверки.</span><span class="sxs-lookup"><span data-stu-id="8351b-203">A new syntax is available in the Beta 3 release for bypassing validation.</span></span> <span data-ttu-id="8351b-204">`Validation` Метод, используемый в бета-версии 3 были удалены.</span><span class="sxs-lookup"><span data-stu-id="8351b-204">The `Validation` method used in Beta 3 has been removed.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="8351b-205">Если не отключить проверку запросов, если пользователь попытается отправить HTML-разметка (например, с помощью редактора форматированного текста на странице), веб-сайт сообщит об ошибке *от клиентаОбнаруженопотенциальноопасноезначениеRequest.Form*и входные данные пользователя не принят.</span><span class="sxs-lookup"><span data-stu-id="8351b-205">If you do not disable request validation, if users try to upload HTML markup (for example, by using a rich text editor on a page), the website will report an error like *A potentially dangerous Request.Form value was detected from the client* and the user input is not accepted.</span></span> <span data-ttu-id="8351b-206">Если отключить проверку запросов, необходимо вручную проверить ввод данных пользователем, чтобы убедиться в том, что он не содержит потенциально опасной разметки или с помощью примерно [Microsoft версии 4.0 библиотеки скриптов узла защиты между](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span><span class="sxs-lookup"><span data-stu-id="8351b-206">If you disable request validation, you must manually check user input to make sure that it does not contain potentially dangerous markup or script using something like the [Microsoft Anti-Cross Site Scripting Library V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span></span>
> 
> 
> <span data-ttu-id="8351b-207">Чтобы отключить автоматическую проверку запросов, вызовите `Request.Unvalidated` метод, передав ему имя поля или другому объекту post, который вы хотите обойти проверку запросов для.</span><span class="sxs-lookup"><span data-stu-id="8351b-207">To disable automatic request validation, call the `Request.Unvalidated` method, passing it the name of the field or other post object that you want to bypass request validation for.</span></span> <span data-ttu-id="8351b-208">Этот метод можно использовать для обхода проверки для всех элементов в `Form`, `QueryString`, `Cookies`, и `ServerVariables` коллекции.</span><span class="sxs-lookup"><span data-stu-id="8351b-208">You can use this method to bypass validation for any items in the `Form`, `QueryString`, `Cookies`, and `ServerVariables` collections.</span></span> <span data-ttu-id="8351b-209">В следующих примерах показано использование `Unvalidated` метода:</span><span class="sxs-lookup"><span data-stu-id="8351b-209">The following examples show how to use the `Unvalidated` method:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="8351b-210">Известные проблемы для веб-страницы ASP.NET с синтаксисом Razor</span><span class="sxs-lookup"><span data-stu-id="8351b-210">Known Issues for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="8351b-211">Проблема: Непредвиденное поведение при использовании пользовательские таблицы для членства</span><span class="sxs-lookup"><span data-stu-id="8351b-211">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="8351b-212">Чтобы инициализировать поставщик членства для веб-сайта ASP.NET Razor, вызовите `WebSecurity.InitializeDatabaseConnection` метод.</span><span class="sxs-lookup"><span data-stu-id="8351b-212">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="8351b-213">(В WebMatrix шаблоне начального сайта включает вызов этого метода в  *\_AppStart.cshtml* файл.) Если `autoCreateTables` параметр этого метода имеет значение в значение true (по умолчанию, ему присваивается значение true, если в шаблоне начального сайта), и если имя таблицы нераспознанный передается в метод (второй параметр), метод не выдает ошибку.</span><span class="sxs-lookup"><span data-stu-id="8351b-213">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="8351b-214">Вместо этого он автоматически создадут таблицу.</span><span class="sxs-lookup"><span data-stu-id="8351b-214">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="8351b-215">Это может вызвать проблемы, если планируется использовать пользовательские таблицы членства, но передает имя неправильной таблицы в `WebSecurity.InitializeDatabaseConnection` метод.</span><span class="sxs-lookup"><span data-stu-id="8351b-215">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="8351b-216">Поскольку метод по умолчанию вызывает ошибку, если таблицы не существует, и вместо этого он создает новую таблицу, приложение может появиться работает.</span><span class="sxs-lookup"><span data-stu-id="8351b-216">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="8351b-217">Тем не менее код приложения, который основан на пользовательские таблицы (и поля в нем) со временем может завершиться непредвиденные ошибки.</span><span class="sxs-lookup"><span data-stu-id="8351b-217">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="8351b-218">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="8351b-218">**Workaround**</span></span>  
> <span data-ttu-id="8351b-219">Убедитесь, что имя, переданное в `InitializeDatabaseConnection` метод соответствует таблице в базе данных членства профиль пользователя, или убедитесь, что `autoCreateTables` параметр имеет значение false.</span><span class="sxs-lookup"><span data-stu-id="8351b-219">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="8351b-220">Ошибка, проблема: «не удалось сформировать пользовательский экземпляр SQL Server»</span><span class="sxs-lookup"><span data-stu-id="8351b-220">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="8351b-221">Если выполняется IIS 7.5 в Windows 7 или Windows Server 2008 R2 WebMatrix веб-приложение использует SQL Server Express, может появиться сообщение о том, что SQL Server не удалось получить путь локального приложения пользователя во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="8351b-221">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="8351b-222">**Инструкции по решению** убедитесь, что учетная запись Windows, то приложение запущено под (как правило, NETWORK SERVICE) имеет разрешения на чтение и запись для корневой папки приложения и вложенные папки например *приложения\_данных*.</span><span class="sxs-lookup"><span data-stu-id="8351b-222">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="8351b-223">Более подробные сведения можно найти в статье базы знаний [проблемы с SQL Server Express пользователя при создании экземпляров и проектами веб-приложений ASP.net](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="8351b-223">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a><span data-ttu-id="8351b-224">Проблема: В Visual Studio, пространства имен для пользовательских сборок (DLL) не импортируются автоматически</span><span class="sxs-lookup"><span data-stu-id="8351b-224">Issue: In Visual Studio, namespaces for custom assemblies (DLLs) are not imported automatically</span></span>

> <span data-ttu-id="8351b-225">Если вы используете пользовательские сборки в проект в Visual Studio, пространства имен, объявленные в эти сборки не импортируются автоматически во время разработки.</span><span class="sxs-lookup"><span data-stu-id="8351b-225">If you use custom assemblies in a project in Visual Studio, the namespaces declared in those assemblies are not automatically imported at design time.</span></span> <span data-ttu-id="8351b-226">В результате ссылки на пользовательские типы могут не распознаваться во время разработки, помечаются как не указано в Visual Studio (с помощью «волнистая линия»).</span><span class="sxs-lookup"><span data-stu-id="8351b-226">As a result, references to custom types might not be recognized at design time and are marked as not recognized in Visual Studio (using a "squiggle").</span></span> <span data-ttu-id="8351b-227">Это происходит только во время разработки в Visual Studio. само приложение работает правильно.</span><span class="sxs-lookup"><span data-stu-id="8351b-227">This problem occurs only at design time in Visual Studio; the application itself runs properly.</span></span>
> 
> <span data-ttu-id="8351b-228">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="8351b-228">**Workaround**</span></span>  
> <span data-ttu-id="8351b-229">Включить `using` инструкции (`imports` в Visual Basic), ссылается на сущности, которые не распознаются во время разработки.</span><span class="sxs-lookup"><span data-stu-id="8351b-229">Include a `using` statement (`imports` in Visual Basic) that references the entities that are not recognized at design time.</span></span>


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="8351b-230">Проблема: Visual Studio IntelliSense и шаблоны проектов доступны только в ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="8351b-230">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="8351b-231">Установка веб-страниц ASP.NET не также устанавливает средства для Visual Studio как IntelliSense и шаблоны проектов для приложений веб-страниц ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8351b-231">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="8351b-232">**Инструкции по решению** для использования IntelliSense и шаблоны проектов для приложений веб-страниц ASP.NET в Visual Studio, установить ASP.NET MVC 3, версия-Кандидат либо с помощью установщика веб-платформы или [автономного установщика](https://go.microsoft.com/fwlink/?LinkID=191797).</span><span class="sxs-lookup"><span data-stu-id="8351b-232">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a><span data-ttu-id="8351b-233">Проблема: «&lt;вспомогательный&gt; не удается найти класс» ошибка</span><span class="sxs-lookup"><span data-stu-id="8351b-233">Issue: "&lt;helper&gt; class cannot be found" error</span></span>

> <span data-ttu-id="8351b-234">После обновления до бета-версии 3, может появится сообщение об ошибке, вспомогательный класс (например, `Facebook` класс) не удается найти.</span><span class="sxs-lookup"><span data-stu-id="8351b-234">After you upgrade to Beta 3, you might see an error that a helper class (for example, the `Facebook` class) cannot not be found.</span></span> <span data-ttu-id="8351b-235">Начиная с версии Beta 2 и продолжить в бета-версии 3, были перемещены вспомогательные методы для пакетов, необходимо явно установить.</span><span class="sxs-lookup"><span data-stu-id="8351b-235">Starting in Beta 2 and continuing in Beta 3, helpers have been moved to packages that you must explicitly install.</span></span> <span data-ttu-id="8351b-236">Существующие узлы не будут обновлены до включать эти пакеты; Сюда входят узлы в *\My Documents\IISExpress* или *\My Documents\My веб-сайтов* папки.</span><span class="sxs-lookup"><span data-stu-id="8351b-236">Existing sites are not upgraded to include these packages; this includes sites in the *\My Documents\IISExpress* or *\My Documents\My Web Sites* folders.</span></span> <span data-ttu-id="8351b-237">В частности, вы увидите эту ошибку при использовании сайта по умолчанию в *личных узлов* (WebSite1), которая содержит ссылку на `Twitter` вспомогательный.</span><span class="sxs-lookup"><span data-stu-id="8351b-237">In particular, you will see this error if you use the default site in *My Sites* (WebSite1), which includes a reference to the `Twitter` helper.</span></span>
> 
> <span data-ttu-id="8351b-238">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="8351b-238">**Workaround**</span></span>  
> <span data-ttu-id="8351b-239">Закомментируйте вызовы все вспомогательные объекты на сайте, запустите  *\_администратора* и установить пакет или пакеты, содержащие вспомогательные методы, которые вы хотите использовать.</span><span class="sxs-lookup"><span data-stu-id="8351b-239">Comment out calls to any helpers in the site, run the *\_Admin* page, and install the package or packages that include the helpers that you want to use.</span></span> <span data-ttu-id="8351b-240">После установки пакета вы можете раскомментировать строки, которые ссылаются вспомогательных методов.</span><span class="sxs-lookup"><span data-stu-id="8351b-240">After you've installed the package, you can uncomment the lines that reference helpers.</span></span>


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a><span data-ttu-id="8351b-241">Проблема: Развертывание сборок бета-версии 3 ASP.NET Razor в папку Bin может не работать для размещения сайтов</span><span class="sxs-lookup"><span data-stu-id="8351b-241">Issue: Deploying Beta 3 ASP.NET Razor assemblies to the Bin folder might not work on hosting sites</span></span>

> <span data-ttu-id="8351b-242">При развертывании на веб-сайт веб-страниц ASP.NET на сайте, и в том случае, если развертывание сборок ASP.NET Razor бета-версии 3 на сайт *Bin* папки, могут возникнуть ошибки, включая следующие:</span><span class="sxs-lookup"><span data-stu-id="8351b-242">If you deploy an ASP.NET Web Pages website to a hosting site, and if you deploy the ASP.NET Razor Beta 3 assemblies to the site's *Bin* folder, you might experience errors, including the following:</span></span>
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> <span data-ttu-id="8351b-243">Это может произойти, если поставщик услуг размещения установил сборки ASP.NET Web Pages бета-версии 1 в кэш глобальной приложения сервера (GAC).</span><span class="sxs-lookup"><span data-stu-id="8351b-243">This can happen if the hosting provider has installed the ASP.NET Web Pages Beta 1 assemblies into the server's global application cache (GAC).</span></span> <span data-ttu-id="8351b-244">Сборки в глобальном кэше СБОРОК получить более высокий приоритет над сборок, установленных локально в *Bin* папки.</span><span class="sxs-lookup"><span data-stu-id="8351b-244">Assemblies in the GAC get precedence over assemblies installed locally in the *Bin* folder.</span></span>
> 
> <span data-ttu-id="8351b-245">**Инструкции по решению** свяжитесь с поставщиком услуг размещения для подтверждения того, вы видите ошибки из-за конфликта версий поставщика сборок и указанное имя.</span><span class="sxs-lookup"><span data-stu-id="8351b-245">**Workaround** Contact your hosting provider to confirm that the errors you are seeing are due to a conflict between the provider's versions of the assemblies and yours.</span></span> <span data-ttu-id="8351b-246">В этом случае запрос обновления, поставщик услуг размещения сборок в глобальном кэше СБОРОК на сервере.</span><span class="sxs-lookup"><span data-stu-id="8351b-246">If so, request that the hosting provider update the assemblies in the server's GAC.</span></span>


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="8351b-247">Проблема: Чтение веб-каналов или другие внешние источники данных через прокси-сервер</span><span class="sxs-lookup"><span data-stu-id="8351b-247">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="8351b-248">Если на сервере сайта находится за прокси-сервер, может потребоваться настроить сведения о прокси-сервера в *Web.config* файл, чтобы иметь возможность считывать данные, поступающие от вне локального узла.</span><span class="sxs-lookup"><span data-stu-id="8351b-248">If the server running the site is behind a proxy server, you might need to configure proxy information in the *Web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="8351b-249">Например, если вы используете `ReCaptcha` вспомогательный, вспомогательное приложение взаимодействует со службой reCAPTCHA, но может быть заблокирован прокси-сервером.</span><span class="sxs-lookup"><span data-stu-id="8351b-249">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="8351b-250">Аналогично веб-каналов, используемые веб-страницах ASP.NET, такие как веб-канал, используемый диспетчер пакетов, может потребоваться настройка прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="8351b-250">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="8351b-251">Если возникли проблемы в работе с внешней службы или при работе с пакетом, в веб-канала, помещаются следующие элементы в корневого каталога приложения *Web.config* файла:</span><span class="sxs-lookup"><span data-stu-id="8351b-251">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *Web.config* file:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> <span data-ttu-id="8351b-252">Дополнительные сведения о настройке прокси-сервера см. в разделе [ &lt;прокси&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) на сайте MSDN.</span><span class="sxs-lookup"><span data-stu-id="8351b-252">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on the MSDN Web site.</span></span>


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a><span data-ttu-id="8351b-253">Проблема: Ошибка «Не удается загрузить Microsoft.Web.Infrastructure.dll»</span><span class="sxs-lookup"><span data-stu-id="8351b-253">Issue: "Microsoft.Web.Infrastructure.dll cannot be loaded" error</span></span>

> <span data-ttu-id="8351b-254">Если вы ранее установили версию бета-версии 1 веб-страниц ASP.NET с синтаксисом Razor, а затем установить версию бета-версии 3, все соответствующие сборки будут установлены в глобальном кэше СБОРОК, за исключением *Microsoft.Web.Infrastructure.dll*.</span><span class="sxs-lookup"><span data-stu-id="8351b-254">If you previously installed the Beta 1 version of ASP.NET Web Pages with Razor syntax and then install the Beta 3 version, all appropriate assemblies are installed in the GAC except *Microsoft.Web.Infrastructure.dll*.</span></span> <span data-ttu-id="8351b-255">Как следствие, при запуске страницы ASP.NET Razor появится сообщение об ошибке, которое указывает, что *Microsoft.Web.Infrastructure.dll* не может быть загружена.</span><span class="sxs-lookup"><span data-stu-id="8351b-255">As a consequence, when you run ASP.NET Razor pages, you see an error that indicates that *Microsoft.Web.Infrastructure.dll* could not be loaded.</span></span>
> 
> <span data-ttu-id="8351b-256">Эта проблема не возникает, если вы загрузили бета-версии 3 на чистом компьютере.</span><span class="sxs-lookup"><span data-stu-id="8351b-256">This issue does not occur if you loaded the Beta 3 release on a clean computer.</span></span>
> 
> <span data-ttu-id="8351b-257">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="8351b-257">**Workaround**</span></span>  
> <span data-ttu-id="8351b-258">На панели управления удалите веб-страниц ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8351b-258">In Control Panel, uninstall ASP.NET Web Pages.</span></span> <span data-ttu-id="8351b-259">Затем переустановите бета-версии 3.</span><span class="sxs-lookup"><span data-stu-id="8351b-259">Then reinstall the Beta 3 release.</span></span>


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="8351b-260">Проблема: Удаление .NET Framework версии 4 отключает веб-страниц ASP.NET с синтаксисом Razor</span><span class="sxs-lookup"><span data-stu-id="8351b-260">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="8351b-261">Если удалить .NET Framework версии 4, а затем переустановите его, веб-страниц ASP.NET с синтаксисом Razor отключена.</span><span class="sxs-lookup"><span data-stu-id="8351b-261">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="8351b-262">Страницы с *.cshtml* расширения работают правильно.</span><span class="sxs-lookup"><span data-stu-id="8351b-262">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="8351b-263">Веб-страницы ASP.NET регистрирует сборку в корневой папке машины *Web.config* файла и удаление .NET Framework удаляет этот файл.</span><span class="sxs-lookup"><span data-stu-id="8351b-263">ASP.NET Web Pages registers an assembly in the machine root *Web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="8351b-264">Повторная установка .NET Framework устанавливает новую версию файла конфигурации, но не добавляет ссылку для сборки веб-страниц ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8351b-264">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="8351b-265">**Инструкции по решению** после повторной установки .NET Framework, переустановите ASP.NET Web Pages с синтаксисом Razor.</span><span class="sxs-lookup"><span data-stu-id="8351b-265">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="8351b-266">При этом добавляется следующий элемент для *Web.config* файл в корне машины, которое обычно находится в следующем расположении:</span><span class="sxs-lookup"><span data-stu-id="8351b-266">This adds the following element to the *Web.config* file in the machine root, which is typically in the following location:</span></span>  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a><span data-ttu-id="8351b-267">Проблема: Приложения, развернутые ранее с помощью ASP.NET сборок в папке Bin возникать ошибки</span><span class="sxs-lookup"><span data-stu-id="8351b-267">Issue: Applications previously deployed with ASP.NET assemblies in the Bin folder experience errors</span></span>

> <span data-ttu-id="8351b-268">Во время развертывания, копии сборок веб-страниц ASP.NET (например, *Microsoft.WebPages.dll*) для *Bin* папку веб-сайта на сервере.</span><span class="sxs-lookup"><span data-stu-id="8351b-268">During deployment, copies of the ASP.NET Web Pages assemblies (for example, *Microsoft.WebPages.dll*) to the *Bin* folder of the website on the server.</span></span> <span data-ttu-id="8351b-269">(Это может произойти автоматически во время развертывания или потому, что разработчик явно копии сборок.) Тем не менее если установлена бета-версии 3, ошибки происходит, например ошибки, которые не удается найти определенных типов.</span><span class="sxs-lookup"><span data-stu-id="8351b-269">(This might have happened automatically during deployment or because the developer explicitly copied the assemblies.) However, when the Beta 3 release is installed, errors occurs, such as errors that certain types cannot be found.</span></span> <span data-ttu-id="8351b-270">Это происходит, поскольку несколько типов веб-страниц ASP.NET были перемещены в разных пространствах имен для бета-версии 3.</span><span class="sxs-lookup"><span data-stu-id="8351b-270">This occurs because a number of ASP.NET Web Pages types were moved into different namespaces for the Beta 3 release.</span></span>
> 
> <span data-ttu-id="8351b-271">**Инструкции по решению** </span><span class="sxs-lookup"><span data-stu-id="8351b-271">**Workaround** </span></span>  
> <span data-ttu-id="8351b-272">Очистить *Bin* папке развернутого приложения, скопировать новые сборки в папку (или повторного развертывания приложения), а затем перезапустите приложение.</span><span class="sxs-lookup"><span data-stu-id="8351b-272">Clear the *Bin* folder of the deployed application, copy the new assemblies to the folder (or redeploy the application), and then restart the application.</span></span>


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="8351b-273">Проблема: Без расширений URL-адреса не найдены файлы.cshtml/.vbhtml в IIS 7 или IIS 7.5</span><span class="sxs-lookup"><span data-stu-id="8351b-273">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="8351b-274">В IIS 7 или IIS 7.5, запросы с URL-адреса следующего вида не удается найти страниц, имеющих *.cshtml* или *.vbhtml* расширения:</span><span class="sxs-lookup"><span data-stu-id="8351b-274">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> <span data-ttu-id="8351b-275">Проблема возникает из-за перезаписи URL-адресов не включена по умолчанию для IIS 7 или IIS 7.5.</span><span class="sxs-lookup"><span data-stu-id="8351b-275">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="8351b-276">Возможная сценарий существует, вы не видите проблемы при тестировании локально с помощью IIS Express, но возникают при развертывании веб-сайта для размещения веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="8351b-276">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="8351b-277">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="8351b-277">**Workaround**</span></span>
> 
> - <span data-ttu-id="8351b-278">Если у вас есть контроль над компьютером сервера, на компьютере сервера установите обновление, описанное в [обновление доступно, что позволяет определенным обработчикам служб IIS 7.0 или IIS 7.5 обрабатывать запросы, URL-адреса не заканчиваться точкой](https://support.microsoft.com/kb/980368).</span><span class="sxs-lookup"><span data-stu-id="8351b-278">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="8351b-279">Если у вас по управлению компьютером сервера (например, развертывается для размещения веб-сайта), добавьте следующий на веб-сайт *Web.config* файла:</span><span class="sxs-lookup"><span data-stu-id="8351b-279">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *Web.config* file:</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a><span data-ttu-id="8351b-280">Проблема: С помощью проекта веб-приложения или страницы ASP.NET MVC и веб-ASP.NET в одном приложении</span><span class="sxs-lookup"><span data-stu-id="8351b-280">Issue: Using Web Application Project or ASP.NET MVC and ASP.NET Web pages in the same application</span></span>

> <span data-ttu-id="8351b-281">При использовании веб-страниц ASP.NET в проект веб-приложения или в приложении ASP.NET MVC, может появится сообщение об ошибке, *WebPageHttpApplication* не удается найти.</span><span class="sxs-lookup"><span data-stu-id="8351b-281">If you were using ASP.NET Web Pages in a Web Application project or ASP.NET MVC application, you might see an error that *WebPageHttpApplication* cannot be found.</span></span>
> 
> <span data-ttu-id="8351b-282">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="8351b-282">**Workaround**</span></span>  
> <span data-ttu-id="8351b-283">При появлении этой ошибки измените базовый класс, от которого наследует приложения.</span><span class="sxs-lookup"><span data-stu-id="8351b-283">If you get this error, change the base class from which the application derives.</span></span> <span data-ttu-id="8351b-284">В *Global.asax* файл, измените следующую строку:</span><span class="sxs-lookup"><span data-stu-id="8351b-284">In the *Global.asax* file, change the following line:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> <span data-ttu-id="8351b-285">на эту:</span><span class="sxs-lookup"><span data-stu-id="8351b-285">To this:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> <span data-ttu-id="8351b-286">Это фактически отменяет изменения, которая была введена в выпуске бета-версии 1 веб-страниц ASP.NET с синтаксисом Razor.</span><span class="sxs-lookup"><span data-stu-id="8351b-286">This in effect reverses a change that was introduced for the Beta 1 release of ASP.NET Web Pages with Razor syntax.</span></span>


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="8351b-287">Проблема: Развертывание приложения на компьютере, не поддерживает SQL Server Compact установлена</span><span class="sxs-lookup"><span data-stu-id="8351b-287">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="8351b-288">Приложения, включающие баз данных SQL Server Compact могут выполняться на компьютере, где SQL Server Compact не установлена.</span><span class="sxs-lookup"><span data-stu-id="8351b-288">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="8351b-289">Microsoft WebMatrix бета-версии 3 автоматически копирует эти двоичные файлы для вас и выполняет соответствующее *Web.config* файла преобразования.</span><span class="sxs-lookup"><span data-stu-id="8351b-289">Microsoft WebMatrix Beta 3 automatically copies these binaries for you and performs the appropriate *Web.config* file transforms.</span></span>
> 
> <span data-ttu-id="8351b-290">**Инструкции по решению** Если вам нужно скопировать эти файлы и сделать *Web.config* изменения файлов вручную, выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="8351b-290">**Workaround** If you need to copy these files and make the *Web.config* file changes manually, do the following :</span></span>
> 
> 1. <span data-ttu-id="8351b-291">Скопируйте сборки ядра базы данных для *Bin* папку (и вложенные папки), приложения на целевом компьютере:</span><span class="sxs-lookup"><span data-stu-id="8351b-291">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span> 
> 
>     - <span data-ttu-id="8351b-292">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **to** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="8351b-292">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **to** *\Bin*</span></span>
>     - <span data-ttu-id="8351b-293">Копировать *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\*\* **для** *\Bin\x86*</span><span class="sxs-lookup"><span data-stu-id="8351b-293">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\*\* **to** *\Bin\x86*</span></span>
>     - <span data-ttu-id="8351b-294">Копировать *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* **для** *\Bin\amd64*</span><span class="sxs-lookup"><span data-stu-id="8351b-294">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* **to** *\Bin\amd64*</span></span>
> 2. <span data-ttu-id="8351b-295">В корневой папке веб-сайта, создайте или откройте *Web.config* файла.</span><span class="sxs-lookup"><span data-stu-id="8351b-295">In the root folder of the website, create or open a *Web.config* file.</span></span> <span data-ttu-id="8351b-296">(В WebMatrix бета-версии 3, этот тип файлов доступна при выборе **все** в **выберите тип файла** диалоговое окно.)</span><span class="sxs-lookup"><span data-stu-id="8351b-296">(In WebMatrix Beta 3, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="8351b-297">Добавьте следующий элемент в качестве дочернего элемента **&lt;конфигурации&gt;** элемента (не внутри **&lt;system.web&gt;** элемент):</span><span class="sxs-lookup"><span data-stu-id="8351b-297">Add the following element as a child of the **&lt;configuration&gt;** element (not inside the **&lt;system.web&gt;** element):</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="8351b-298">Проблема: Базы данных и WebGrid вспомогательные функции не работают со средним уровнем доверия в Visual Basic</span><span class="sxs-lookup"><span data-stu-id="8351b-298">Issue: Database and WebGrid helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="8351b-299">Если вы используете Visual Basic (создание *.vbhtml* файлы), `Database` и `WebGrid` вспомогательные методы не будет работать, если приложения настроен для использования со средним уровнем доверия.</span><span class="sxs-lookup"><span data-stu-id="8351b-299">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="8351b-300">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="8351b-300">**Workaround**</span></span>  
> <span data-ttu-id="8351b-301">Временно задайте приложение для использования полного доверия.</span><span class="sxs-lookup"><span data-stu-id="8351b-301">Temporarily set the application to use Full Trust.</span></span>

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a><span data-ttu-id="8351b-302">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="8351b-302">SQL Server Compact</span></span>

#### <a name="issue-encrypt-property-is-not-recognized"></a><span data-ttu-id="8351b-303">Проблема: Свойство «Encrypt» не распознан</span><span class="sxs-lookup"><span data-stu-id="8351b-303">Issue: "Encrypt" property is not recognized</span></span>

> <span data-ttu-id="8351b-304">SQL Server Compact 4.0 не распознает `Encrypt` свойство `SqlCeConnection` класса.</span><span class="sxs-lookup"><span data-stu-id="8351b-304">SQL Server Compact 4.0 does not recognize the `Encrypt` property of the `SqlCeConnection` class.</span></span> <span data-ttu-id="8351b-305">Это свойство не следует использовать для шифрования файлов базы данных.</span><span class="sxs-lookup"><span data-stu-id="8351b-305">You should not use this property to encrypt database files.</span></span> <span data-ttu-id="8351b-306">`Encrypt` Свойство устарела в SQL Server Compact 3.5 выпуска и сохранен только для обратной совместимости.</span><span class="sxs-lookup"><span data-stu-id="8351b-306">The `Encrypt` property was deprecated in SQL Server Compact 3.5 release and was retained only for backward compatibility.</span></span> 
> 
> <span data-ttu-id="8351b-307">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="8351b-307">**Workaround**</span></span>  
> <span data-ttu-id="8351b-308">Используйте `Encryption Mode` свойство `SqlCeConnection` класса для шифрования файлов базы данных SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="8351b-308">Use the `Encryption Mode` property of the `SqlCeConnection` class to encrypt SQL Server Compact 4.0 database files.</span></span> <span data-ttu-id="8351b-309">В следующем примере показано, как создание зашифрованные базы данных SQL Server Compact 4.0 с помощью `Encryption Mode` свойство:</span><span class="sxs-lookup"><span data-stu-id="8351b-309">The following example shows how to create an encrypted SQL Server Compact 4.0 database using the `Encryption Mode` property:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> <span data-ttu-id="8351b-310">Смена режима шифрования существующей базы данных SQL Server Compact 4.0, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="8351b-310">To change the encryption mode of an existing SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> <span data-ttu-id="8351b-311">Шифрование незашифрованной базы данных SQL Server Compact 4.0, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="8351b-311">To encrypt an unencrypted SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a><span data-ttu-id="8351b-312">Проблема: Необходимы библиотеки времени выполнения Microsoft Visual C++ 2008</span><span class="sxs-lookup"><span data-stu-id="8351b-312">Issue: Microsoft Visual C++ 2008 runtime libraries are required</span></span>

> <span data-ttu-id="8351b-313">Собственные библиотеки DLL из SQL Server Compact 4.0 требуется Microsoft Visual C++ 2008 Runtime Libraries (x 86, IA64 и x 64) с пакетом обновления 1.</span><span class="sxs-lookup"><span data-stu-id="8351b-313">The native DLLs of SQL Server Compact 4.0 need the Microsoft Visual C++ 2008 Runtime Libraries (x86, IA64, and x64), Service Pack 1.</span></span>
> 
> <span data-ttu-id="8351b-314">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="8351b-314">**Workaround**</span></span>  
> <span data-ttu-id="8351b-315">Установите .NET Framework 3.5 SP1.</span><span class="sxs-lookup"><span data-stu-id="8351b-315">Install the .NET Framework 3.5 SP1.</span></span> <span data-ttu-id="8351b-316">Также устанавливает SP1 библиотеки среды выполнения Visual C++ 2008.</span><span class="sxs-lookup"><span data-stu-id="8351b-316">This also installs the Visual C++ 2008 Runtime Libraries SP1.</span></span> <span data-ttu-id="8351b-317">Эти библиотеки можно загрузить из следующей папки:</span><span class="sxs-lookup"><span data-stu-id="8351b-317">You can download the libraries from the following location:</span></span>   
>   
> [<span data-ttu-id="8351b-318">Обновления безопасности ATL для распространяемого пакета Microsoft Visual C++ 2008 с пакетом обновления 1</span><span class="sxs-lookup"><span data-stu-id="8351b-318">Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL Security Update</span></span>](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> <span data-ttu-id="8351b-319">Обратите внимание, что установка .NET Framework 2.0, 3.0, или 4 не *не* установить SP1 библиотеки среды выполнения Visual C++ 2008.</span><span class="sxs-lookup"><span data-stu-id="8351b-319">Note that installing the .NET Framework 2.0, 3.0, or 4 does *not* install the Visual C++ 2008 Runtime Libraries SP1.</span></span>


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a><span data-ttu-id="8351b-320">Проблема: Если до установки .NET Framework на компьютере установлен SQL Server Compact, его неизменяемое имя поставщика не зарегистрирован в файле machine.config платформы .NET Framework</span><span class="sxs-lookup"><span data-stu-id="8351b-320">Issue: If SQL Server Compact is installed prior to installing .NET Framework on the computer, its provider invariant name is not registered in the .NET Framework machine.config file</span></span>

> <span data-ttu-id="8351b-321">SQL Server Compact может устанавливаться на компьютер, где не установлена платформа .NET Framework, так как SQL Server Compact требуется .NET framework.</span><span class="sxs-lookup"><span data-stu-id="8351b-321">SQL Server Compact can be installed on a machine that does not have .NET Framework installed because SQL Server Compact does require the .NET framework.</span></span> <span data-ttu-id="8351b-322">Если ни .NET Framework версии 3.5 и 4 не установлен перед установкой SQL Server Compact, Compact установки SQL Server не регистрирует его неизменяемое имя поставщика в *machine.config* файла.</span><span class="sxs-lookup"><span data-stu-id="8351b-322">If neither .NET Framework version 3.5 nor 4 is installed before you install SQL Server Compact, the SQL Server Compact Setup does not register its provider invariant name in the *machine.config* file.</span></span> <span data-ttu-id="8351b-323">Все приложения, основанные на SQL Server Compact запись в *machine.config* файл не удастся.</span><span class="sxs-lookup"><span data-stu-id="8351b-323">Any application that relies on the SQL Server Compact entry in the *machine.config* file will fail.</span></span> <span data-ttu-id="8351b-324">Неизменяемое имя регистрационной записи в *machine.config* выглядит как следующий пример:</span><span class="sxs-lookup"><span data-stu-id="8351b-324">The invariant name registration entry in *machine.config* looks like the following example:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> <span data-ttu-id="8351b-325">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="8351b-325">**Workaround**</span></span>  
> <span data-ttu-id="8351b-326">Удаление SQL Server Compact 4.0 CTP-версия 1.</span><span class="sxs-lookup"><span data-stu-id="8351b-326">Uninstall SQL Server Compact 4.0 CTP1.</span></span> <span data-ttu-id="8351b-327">Загрузите и установите полной версии платформы .NET Framework из следующего расположения:</span><span class="sxs-lookup"><span data-stu-id="8351b-327">Download and install the full versions of the .NET Framework from the following location:</span></span>
> 
> [<span data-ttu-id="8351b-328">Microsoft .NET Framework 3.5 пакетом обновления 1 (полный пакет)</span><span class="sxs-lookup"><span data-stu-id="8351b-328">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [<span data-ttu-id="8351b-329">Microsoft .NET Framework 4.0 и выше (полный пакет)</span><span class="sxs-lookup"><span data-stu-id="8351b-329">Microsoft .NET Framework 4.0 Release (Full Package)</span></span>](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> <span data-ttu-id="8351b-330">Затем переустановите [SQL Server Compact 4.0 CTP-версия 1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span><span class="sxs-lookup"><span data-stu-id="8351b-330">Then reinstall [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span></span>


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a><span data-ttu-id="8351b-331">Установка приложений</span><span class="sxs-lookup"><span data-stu-id="8351b-331">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="8351b-332">Проблема: При установке приложения может занять много времени при пользовательской папке "Мои документы" перенаправляется в общую сетевую папку</span><span class="sxs-lookup"><span data-stu-id="8351b-332">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="8351b-333">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="8351b-333">**Workaround**</span></span>  
> <span data-ttu-id="8351b-334">Отсутствует.</span><span class="sxs-lookup"><span data-stu-id="8351b-334">None.</span></span> <span data-ttu-id="8351b-335">Приложение может занять некоторое время, чтобы установить, но устанавливается правильно.</span><span class="sxs-lookup"><span data-stu-id="8351b-335">The application might take a while to install, but will install correctly.</span></span>


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a><span data-ttu-id="8351b-336">Публикация приложений</span><span class="sxs-lookup"><span data-stu-id="8351b-336">Publishing Applications</span></span>

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="8351b-337">Проблема: Сайта может не работать после публикации, если поле «URL-адрес назначения» нет префикса http:// или https://</span><span class="sxs-lookup"><span data-stu-id="8351b-337">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="8351b-338">В **параметры публикации** диалоговое окно, если URL-адрес назначения не начинается с `http://` или `https://`, сайт может не работать после развертывания.</span><span class="sxs-lookup"><span data-stu-id="8351b-338">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="8351b-339">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="8351b-339">**Workaround**</span></span>  
> <span data-ttu-id="8351b-340">Убедитесь, что перед публикацией сайта, URL-адрес назначения в **параметры публикации** диалоговое окно начинается с `http://` или `https://`.</span><span class="sxs-lookup"><span data-stu-id="8351b-340">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="8351b-341">Проблема: Публикация базы данных MySQL завершается с ошибкой «не удалось опубликовать базу данных.</span><span class="sxs-lookup"><span data-stu-id="8351b-341">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="8351b-342">Это может произойти, если удаленная база данных не может запустить сценарий.»</span><span class="sxs-lookup"><span data-stu-id="8351b-342">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="8351b-343">Эта ошибка может возникнуть по ряду причин.</span><span class="sxs-lookup"><span data-stu-id="8351b-343">The error can occur for a number of reasons.</span></span> <span data-ttu-id="8351b-344">Одна из причин этой ошибки можно увидеть, — если сценарий базы данных содержит символ одиночные кавычки (') и не является база данных MySQL назначения по умолчанию используется кодировка UTF-8.</span><span class="sxs-lookup"><span data-stu-id="8351b-344">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="8351b-345">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="8351b-345">**Workaround**</span></span>  
> <span data-ttu-id="8351b-346">Задайте кодировку по умолчанию для удаленной базы данных MySQL в UTF-8.</span><span class="sxs-lookup"><span data-stu-id="8351b-346">Set the default character set for the remote MySQL database to UTF-8.</span></span>


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a><span data-ttu-id="8351b-347">Другие проблемы</span><span class="sxs-lookup"><span data-stu-id="8351b-347">Other Issues</span></span>

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a><span data-ttu-id="8351b-348">Проблема: Поиска и фильтрации не работает в отчетах для Group By: тип проблемы</span><span class="sxs-lookup"><span data-stu-id="8351b-348">Issue: Search/Filter does not work in Reports for Group By: Issue Type</span></span>

> <span data-ttu-id="8351b-349">При запуске отчета для сайта, если ввести текст в *фильтр по URL-адрес* и нажмите кнопку *поиска*, ничего не происходит.</span><span class="sxs-lookup"><span data-stu-id="8351b-349">When you run a report for a site, if you enter text in the *Filter by URL* box and click *Search*, nothing happens.</span></span> <span data-ttu-id="8351b-350">Это, поскольку этот элемент управления не работает при *Group By* присваивается состояние отчета *тип проблемы*, используемого по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="8351b-350">This is because this control is not functional while the *Group By* state of the report is set to *Issue Type*, which is the default.</span></span>
> 
> <span data-ttu-id="8351b-351">**Инструкции по решению** в *Group By* вкладку ленты, нажмите кнопку *URL-адрес* для группировки записей по их URL-адрес источника.</span><span class="sxs-lookup"><span data-stu-id="8351b-351">**Workaround** In the *Group By* tab of ribbon, click *URL* to group the entries by their source URL.</span></span> <span data-ttu-id="8351b-352">Текстовое поле и кнопку для фильтрации записей функциональна в этом состоянии.</span><span class="sxs-lookup"><span data-stu-id="8351b-352">The text box and button to filter the entries are functional while in this state.</span></span>


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a><span data-ttu-id="8351b-353">Проблема: Приложений WCF не удастся выполнить с IIS Express</span><span class="sxs-lookup"><span data-stu-id="8351b-353">Issue: WCF applications fail to run with IIS Express</span></span>

> <span data-ttu-id="8351b-354">Просмотр приложения WCF приводит к ошибке, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="8351b-354">Browsing to a WCF application results in an error like the following one:</span></span>
> 
> <span data-ttu-id="8351b-355">*Не удалось загрузить файл или сборку ' Microsoft.Web.Administration, Version = 7.0.0.0, но, язык и региональные параметры = neutral, PublicKeyToken = 31bf3856ad364e35 "или одну из ее зависимостей. Не удается найти указанный файл.*</span><span class="sxs-lookup"><span data-stu-id="8351b-355">*Could not load file or assembly 'Microsoft.Web.Administration, Version=7.0.0.0, Culture=neutral,PublicKeyToken=31bf3856ad364e35' or one of its dependencies. The system cannot find the file specified.*</span></span>
> 
> <span data-ttu-id="8351b-356">Это происходит, поскольку версия IIS Express бета-версия не поддерживает WCF по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="8351b-356">This occurs because IIS Express Beta release doesn't support WCF by default.</span></span>
> 
> <span data-ttu-id="8351b-357">**Инструкции по решению** , воспользуйтесь одним из следующих решений (инструкции по решению #2 требуется Microsoft Windows Vista или более поздней версии):</span><span class="sxs-lookup"><span data-stu-id="8351b-357">**Workaround** Use any one of the following workarounds (workaround #2 requires Microsoft Windows Vista or higher):</span></span>
> 
> 
> 1. <span data-ttu-id="8351b-358">Копировать *Microsoft.Web.dll* и *Microsoft.Web.Administration.dll* сборки из расположения установки WebMatrix для *bin* каталог WCF приложение.</span><span class="sxs-lookup"><span data-stu-id="8351b-358">Copy the *Microsoft.Web.dll* and *Microsoft.Web.Administration.dll* assemblies from the WebMatrix installation location to the *bin* directory of the WCF application.</span></span> <span data-ttu-id="8351b-359">По умолчанию, WebMatrix устанавливается в *Microsoft WebMatrix* вложенной папке системы *Program Files* папки.</span><span class="sxs-lookup"><span data-stu-id="8351b-359">By default, WebMatrix is installed in the *Microsoft WebMatrix* subfolder under the system's *Program Files* folder.</span></span>
> 2. <span data-ttu-id="8351b-360">В Microsoft Windows Vista или более поздней версии, создать символьную ссылку на сборки в *bin* каталог, используя следующие команды.</span><span class="sxs-lookup"><span data-stu-id="8351b-360">On Microsoft Windows Vista or higher, create a symlink to the assemblies in the *bin* directory using the following commands.</span></span> <span data-ttu-id="8351b-361">(Этот подход имеет то преимущество, что он не создает копию сборок).</span><span class="sxs-lookup"><span data-stu-id="8351b-361">(This approach has the advantage that it does not create a copy of the assemblies.)</span></span>
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. <span data-ttu-id="8351b-362">Установите две сборки в глобальном кэше СБОРОК.</span><span class="sxs-lookup"><span data-stu-id="8351b-362">Install the two assemblies in the GAC.</span></span> <span data-ttu-id="8351b-363">С повышенными привилегиями выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="8351b-363">From an elevated prompt, run the following commands:</span></span>
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="8351b-364">Проблема: Не удалось выполнить определенные задачи, требующие повышения прав WebMatrix бета-версия 3</span><span class="sxs-lookup"><span data-stu-id="8351b-364">Issue: WebMatrix Beta 3 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="8351b-365">WebMatrix бета-версии 3 не может выполнять определенные задачи, требующие повышения прав, таких как установка дополнительных компонентов в следующих ситуациях:</span><span class="sxs-lookup"><span data-stu-id="8351b-365">WebMatrix Beta 3 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="8351b-366">В Windows Vista или Windows 7 вы вошли под учетной записью, у которого нет прав администратора, и управления учетных записей (UAC) отключено.</span><span class="sxs-lookup"><span data-stu-id="8351b-366">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="8351b-367">Вы используете Microsoft Windows XP или Microsoft Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="8351b-367">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="8351b-368">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="8351b-368">**Workaround**</span></span>  
> <span data-ttu-id="8351b-369">Большинство задач в бета-версии 3 WebMatrix необходимы разрешения администратора.</span><span class="sxs-lookup"><span data-stu-id="8351b-369">Most tasks in WebMatrix Beta 3 do not require administrative permission.</span></span> <span data-ttu-id="8351b-370">Для тех, которые можно выполнять операции от имени администратора или выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="8351b-370">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="8351b-371">В Windows Vista или Windows 7 включите контроль учетных Записей.</span><span class="sxs-lookup"><span data-stu-id="8351b-371">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="8351b-372">В Windows XP добавьте пользователя в группу безопасности "Администраторы".</span><span class="sxs-lookup"><span data-stu-id="8351b-372">On Windows XP, add the user to the Administrators security group.</span></span>


#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="8351b-373">Проблема: Отключен «Из галерею веб-узла»</span><span class="sxs-lookup"><span data-stu-id="8351b-373">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="8351b-374">**Сайта из веб-коллекции** параметр отключен, если не установлен установщик веб-платформы 3.0.</span><span class="sxs-lookup"><span data-stu-id="8351b-374">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="8351b-375">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="8351b-375">**Workaround**</span></span>  
> <span data-ttu-id="8351b-376">Установка [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="8351b-376">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a><span data-ttu-id="8351b-377">Проблема: В Windows Server 2003, IIS Express не запускается для обычных пользователей</span><span class="sxs-lookup"><span data-stu-id="8351b-377">Issue: On Windows Server 2003, IIS Express does not start for a non-administrative user</span></span>

> <span data-ttu-id="8351b-378">В Windows Server 2003 при запуске страницы или запустить IIS Express, IIS Express не запускается.</span><span class="sxs-lookup"><span data-stu-id="8351b-378">On Windows Server 2003, when you launch a page or start IIS Express, IIS Express does not start.</span></span> <span data-ttu-id="8351b-379">Для веб-страниц выводится сообщение об ошибке, указывающее, что приложение запущено пользователем без прав администратора.</span><span class="sxs-lookup"><span data-stu-id="8351b-379">For Web pages, an error is displayed that indicates that the application has been started by a non-administrative user.</span></span>
> 
> <span data-ttu-id="8351b-380">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="8351b-380">**Workaround**</span></span>  
> <span data-ttu-id="8351b-381">Пользователь с правами администратора для запуска WebMatrix бета-версии 3.</span><span class="sxs-lookup"><span data-stu-id="8351b-381">Start WebMatrix Beta 3 as an administrative user.</span></span> <span data-ttu-id="8351b-382">Дополнительные сведения см. в следующей статье базы знаний:</span><span class="sxs-lookup"><span data-stu-id="8351b-382">For more details, see the following KnowledgeBase article:</span></span>  
>   
> [<span data-ttu-id="8351b-383">Приложение, которое запускается пользователем без прав администратора не может прослушивать HTTP-трафика компьютера, на котором выполняется приложение в Windows Vista, Windows Server 2003 или Windows XP.</span><span class="sxs-lookup"><span data-stu-id="8351b-383">An application that is started by a non-administrative user cannot listen to the HTTP traffic of the computer on which the application is running in Windows Vista, Windows Server 2003, or Windows XP.</span></span>](https://support.microsoft.com/kb/939786)


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="8351b-384">Проблема: Google Chrome не доступен в качестве параметра запуска</span><span class="sxs-lookup"><span data-stu-id="8351b-384">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="8351b-385">Google Chrome не отображается в списке браузеров в **запуска** на **Главная** вкладки.</span><span class="sxs-lookup"><span data-stu-id="8351b-385">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="8351b-386">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="8351b-386">**Workaround**</span></span>  
> <span data-ttu-id="8351b-387">В некоторых версиях Google Chrome не регистрируют себя правильно с функцией программ по умолчанию в Windows.</span><span class="sxs-lookup"><span data-stu-id="8351b-387">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="8351b-388">Чтобы избежать этого, Google Chrome, выберите пункт *Настройка и управление Google Chrome* меню, нажмите кнопку *параметры*и нажмите кнопку *марка Google Chrome обозреватель по умолчанию*.</span><span class="sxs-lookup"><span data-stu-id="8351b-388">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="8351b-389">Проблема: Диалоговое окно «Foreign Key» не допускает ввод первичного ключа</span><span class="sxs-lookup"><span data-stu-id="8351b-389">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="8351b-390">**Foreign Key** диалоговое окно не позволяет ввести имя первичного ключа из таблицы первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="8351b-390">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="8351b-391">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="8351b-391">**Workaround**</span></span>  
> <span data-ttu-id="8351b-392">Это сделано намеренно.</span><span class="sxs-lookup"><span data-stu-id="8351b-392">This is intentional.</span></span> <span data-ttu-id="8351b-393">Необходимо ввести имя первичного ключа из таблицы первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="8351b-393">You do not need to enter the name of the primary key from the primary key table.</span></span>


#### <a name="issue-the-relationships-button-is-disabled"></a><span data-ttu-id="8351b-394">Проблема: Кнопка «Связей» отключена</span><span class="sxs-lookup"><span data-stu-id="8351b-394">Issue: The "Relationships" button is disabled</span></span>

> <span data-ttu-id="8351b-395">**Связи** под заголовком **таблицы** вкладке **баз данных** рабочей отключена для баз данных SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="8351b-395">The **Relationships** button under the **Table** tab in the **Databases** workspace is disabled for SQL Server Compact databases.</span></span>
> 
> <span data-ttu-id="8351b-396">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="8351b-396">**Workaround**</span></span>  
> <span data-ttu-id="8351b-397">Отсутствует.</span><span class="sxs-lookup"><span data-stu-id="8351b-397">None.</span></span> <span data-ttu-id="8351b-398">SQL Server Compact не поддерживает связи между таблицами.</span><span class="sxs-lookup"><span data-stu-id="8351b-398">SQL Server Compact does not support relationships between tables.</span></span>


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a><span data-ttu-id="8351b-399">Проблема: Параметризованные SQL-запросы исключений</span><span class="sxs-lookup"><span data-stu-id="8351b-399">Issue: Parameterized SQL queries throw exceptions</span></span>

> <span data-ttu-id="8351b-400">В SQL Server Compact 4.0, если не указать тип данных таких как `SqlDbType` или `DbType` для параметров в параметризованных запросах, создается исключение при выполнении запроса.</span><span class="sxs-lookup"><span data-stu-id="8351b-400">In SQL Server Compact 4.0, if you do not specify a data type such as `SqlDbType` or `DbType` for parameters in parameterized queries, an exception is thrown when the query runs.</span></span>
> 
> <span data-ttu-id="8351b-401">**Workaround**</span><span class="sxs-lookup"><span data-stu-id="8351b-401">**Workaround**</span></span>  
> <span data-ttu-id="8351b-402">Явно задать тип данных для параметров, таких как `SqlDbType` или `DbType`.</span><span class="sxs-lookup"><span data-stu-id="8351b-402">Explicitly set the data type for parameters such as `SqlDbType` or `DbType`.</span></span> <span data-ttu-id="8351b-403">Это очень важно в случае с типами данных больших двоичных ОБЪЕКТОВ (`image` и `ntext`).</span><span class="sxs-lookup"><span data-stu-id="8351b-403">This is critical in the case of BLOB data types (`image` and `ntext`).</span></span> <span data-ttu-id="8351b-404">Используйте код, аналогичный следующему:</span><span class="sxs-lookup"><span data-stu-id="8351b-404">Use code like the following:</span></span>
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="8351b-405">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="8351b-405">For More Information</span></span>

<span data-ttu-id="8351b-406">Дополнительные сведения о WebMatrix бета-версии 3 см. ниже веб-сайтов:</span><span class="sxs-lookup"><span data-stu-id="8351b-406">For more information about WebMatrix Beta 3, see the following websites:</span></span>

- [<span data-ttu-id="8351b-407">IIS.net</span><span class="sxs-lookup"><span data-stu-id="8351b-407">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="8351b-408">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8351b-408">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="8351b-409">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="8351b-409">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

* * *

<span data-ttu-id="8351b-410">© Корпорация Майкрософт, 2010.</span><span class="sxs-lookup"><span data-stu-id="8351b-410">© 2010 Microsoft Corporation.</span></span> <span data-ttu-id="8351b-411">Все права защищены.</span><span class="sxs-lookup"><span data-stu-id="8351b-411">All Rights Reserved.</span></span> <span data-ttu-id="8351b-412">[Условия использования](https://msdn.microsoft.cos/cc300389.aspx).</span><span class="sxs-lookup"><span data-stu-id="8351b-412">[Terms of Use](https://msdn.microsoft.cos/cc300389.aspx).</span></span>
