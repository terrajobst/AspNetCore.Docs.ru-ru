---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: "Усовершенствования в Visual Studio 2005 | Документы Microsoft"
author: microsoft
description: "Visual Studio 2005 предоставляет разработчики веб-приложений с большом количестве улучшений и усовершенствований для веб-проектов."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: aafc59980e807677d6023110d324365ce92bb5fc
ms.sourcegitcommit: d8aa1d314891e981460b5e5c912afb730adbb3ad
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/05/2018
---
<a name="improvements-in-visual-studio-2005"></a><span data-ttu-id="6eea2-103">Усовершенствования в Visual Studio 2005</span><span class="sxs-lookup"><span data-stu-id="6eea2-103">Improvements in Visual Studio 2005</span></span>
====================
<span data-ttu-id="6eea2-104">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="6eea2-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="6eea2-105">Visual Studio 2005 предоставляет разработчики веб-приложений с большом количестве улучшений и усовершенствований для веб-проектов.</span><span class="sxs-lookup"><span data-stu-id="6eea2-105">Visual Studio 2005 provides Web application developers with a long list of improvements and enhancements to Web projects.</span></span>


<span data-ttu-id="6eea2-106">Visual Studio 2005 предоставляет разработчики веб-приложений с большом количестве улучшений и усовершенствований для веб-проектов.</span><span class="sxs-lookup"><span data-stu-id="6eea2-106">Visual Studio 2005 provides Web application developers with a long list of improvements and enhancements to Web projects.</span></span> <span data-ttu-id="6eea2-107">Также производительно, как Visual Studio .NET 2002 и 2003, было много жалобы в способе обработки веб-проектов.</span><span class="sxs-lookup"><span data-stu-id="6eea2-107">As powerful as Visual Studio .NET 2002 and 2003 are, there were many complaints in the way that Web projects were handled.</span></span> <span data-ttu-id="6eea2-108">Visual Studio 2005 добавляет значительное количество новых компонентов для решения этих проблем.</span><span class="sxs-lookup"><span data-stu-id="6eea2-108">Visual Studio 2005 adds a significant number of new features in order to address these complaints.</span></span> <span data-ttu-id="6eea2-109">Для тех, которые предпочитают способ обработки компиляции веб-приложений в Visual Studio .NET 2003, в разделе [проектов веб-приложений](https://go.microsoft.com/fwlink/?LinkId=57870).</span><span class="sxs-lookup"><span data-stu-id="6eea2-109">For those who prefer the way that Visual Studio .NET 2003 handled compilation of Web applications, see [Web Application Projects](https://go.microsoft.com/fwlink/?LinkId=57870).</span></span>

<span data-ttu-id="6eea2-110">В этот модуль также рассматриваются методы создания веб-проекта, управления и разработки.</span><span class="sxs-lookup"><span data-stu-id="6eea2-110">In this module, well cover improvements in Web project creation, management, and development.</span></span> <span data-ttu-id="6eea2-111">В более поздней версии модуля также охватывает улучшения в построении веб-проектов и их развертывания.</span><span class="sxs-lookup"><span data-stu-id="6eea2-111">In a later module, well cover improvements in building Web projects and deploying them.</span></span>

## <a name="frontpage-server-extensions"></a><span data-ttu-id="6eea2-112">Серверные расширения FrontPage</span><span class="sxs-lookup"><span data-stu-id="6eea2-112">FrontPage Server Extensions</span></span>

<span data-ttu-id="6eea2-113">Visual Studio .NET 2002 и 2003 необходимые серверные расширения FrontPage на поле для создания или построения веб-проектов.</span><span class="sxs-lookup"><span data-stu-id="6eea2-113">Visual Studio .NET 2002 and 2003 required FrontPage Server Extensions on the box in order to create or build Web projects.</span></span> <span data-ttu-id="6eea2-114">Разработчики у Выбор между двумя режимами различный уровень доступа (режим доступа серверных расширений FrontPage или файл), оба используются серверные расширения FrontPage для выполнения задач, таких как установка корневой каталог приложения в IIS, и т. д.</span><span class="sxs-lookup"><span data-stu-id="6eea2-114">Developers did have a choice between two different access modes (FrontPage Server Extensions or File access mode), both used FrontPage Server Extensions to perform tasks such as setting the application root in IIS, etc.</span></span>

<span data-ttu-id="6eea2-115">Visual Studio 2005 устраняет зависимость от серверных расширений FrontPage для локальных проектов.</span><span class="sxs-lookup"><span data-stu-id="6eea2-115">Visual Studio 2005 removes the reliance on FrontPage Server Extensions for local projects.</span></span> <span data-ttu-id="6eea2-116">Visual Studio 2005 теперь осуществляет доступ к метабазе IIS напрямую, а не с помощью серверных расширений FrontPage.</span><span class="sxs-lookup"><span data-stu-id="6eea2-116">Visual Studio 2005 now accesses the IIS metabase directly instead of using the FrontPage Server Extensions.</span></span> <span data-ttu-id="6eea2-117">Visual Studio 2005 также добавляет поддержку для FTP, позволяющий проект удаленного доступа без использования серверных расширений FrontPage.</span><span class="sxs-lookup"><span data-stu-id="6eea2-117">Visual Studio 2005 also adds support for FTP which allows for remote project access without requiring FrontPage Server Extensions.</span></span>

<span data-ttu-id="6eea2-118">Для разработчиков, которым требуется использовать в своих проектах серверные расширения FrontPage параметр по-прежнему доступен.</span><span class="sxs-lookup"><span data-stu-id="6eea2-118">For those developers who want to use FrontPage Server Extensions in their projects, the option is still available.</span></span> <span data-ttu-id="6eea2-119">Тем не менее основываясь на надежный отзывах членов сообщества разработчиков ASP.NET, это не является обязательным.</span><span class="sxs-lookup"><span data-stu-id="6eea2-119">However, based upon strong feedback from the ASP.NET developer community, it is not a requirement.</span></span>

> [!NOTE]
> <span data-ttu-id="6eea2-120">Серверные расширения FrontPage по-прежнему необходимы для создания удаленного проекта, открытия и т. д.</span><span class="sxs-lookup"><span data-stu-id="6eea2-120">FrontPage Server Extensions are still required for remote project creation, opening, etc.</span></span>


## <a name="aspnet-development-server"></a><span data-ttu-id="6eea2-121">Сервер ASP.NET Development Server</span><span class="sxs-lookup"><span data-stu-id="6eea2-121">ASP.NET Development Server</span></span>

<span data-ttu-id="6eea2-122">Visual Studio 2005 поставляется с нового веб-сервер, который вызывается ASP.NET Development Server.</span><span class="sxs-lookup"><span data-stu-id="6eea2-122">Visual Studio 2005 ships with a new Web server called ASP.NET Development Server.</span></span> <span data-ttu-id="6eea2-123">(Этот веб-сервер был ранее известен как Cassini.)</span><span class="sxs-lookup"><span data-stu-id="6eea2-123">(This Web server was previously known as Cassini.)</span></span>

<span data-ttu-id="6eea2-124">Существует несколько преимуществ ASP.NET Development Server.</span><span class="sxs-lookup"><span data-stu-id="6eea2-124">There are several benefits of the ASP.NET Development Server.</span></span>

- <span data-ttu-id="6eea2-125">Теперь возможна, являющиеся администраторами для разработки и отладки для веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="6eea2-125">It is now possible for non-Administrators to develop and debug against a Web server.</span></span>
- <span data-ttu-id="6eea2-126">ASP.NET Development Server динамически сопоставляет виртуальных каталогов в любое место в файловой системе, что обеспечивает расположения проектов по гибкой.</span><span class="sxs-lookup"><span data-stu-id="6eea2-126">The ASP.NET Development Server dynamically maps virtual directories to any location in the file system allowing for flexible project locations.</span></span>
- <span data-ttu-id="6eea2-127">Пользователей в Windows XP Professional, которые уже используются службы IIS теперь можно создать новый веб-приложений, которые не влияют на структуру файла или папки из своего по умолчанию веб-сайта в службах IIS.</span><span class="sxs-lookup"><span data-stu-id="6eea2-127">Users on Windows XP Professional who are already using IIS will now be able to create new Web applications that will not affect the file or folder structure of their Default Web Site in IIS.</span></span>

<span data-ttu-id="6eea2-128">Чтобы воспользоваться преимуществами ASP.NET Development Server требуется никакая специальная настройка.</span><span class="sxs-lookup"><span data-stu-id="6eea2-128">No special configuration is required to take advantage of the ASP.NET Development Server.</span></span> <span data-ttu-id="6eea2-129">Если отлаживается или просматривать веб-проекта, который размещен в файловой системе, Visual Studio 2005 автоматически запускает экземпляр сервера разработки ASP.NET на случайный порт для обслуживания запроса.</span><span class="sxs-lookup"><span data-stu-id="6eea2-129">When a Web project that is hosted on the file system is debugged or browsed, Visual Studio 2005 will automatically start an instance of the ASP.NET Development Server on a random port to service the request.</span></span>

<span data-ttu-id="6eea2-130">Дополнительные сведения будут рассматриваться с ASP.NET Development Server, далее в этом модуле.</span><span class="sxs-lookup"><span data-stu-id="6eea2-130">More information will be covered on the ASP.NET Development Server later in this module.</span></span>

## <a name="improved-file-management"></a><span data-ttu-id="6eea2-131">Управление файлами</span><span class="sxs-lookup"><span data-stu-id="6eea2-131">Improved File Management</span></span>

<span data-ttu-id="6eea2-132">В Visual Studio 2002 и 2003 файл проекта (.vbproj для VB.NET) и .csproj для C# хранятся сведения для всех файлов в веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="6eea2-132">In Visual Studio 2002 and 2003, a project file (.vbproj for VB.NET and .csproj for C#) stored information on all files in the Web application.</span></span> <span data-ttu-id="6eea2-133">Отображение обозревателя решений основана на сведения о файлах в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="6eea2-133">The Solution Explorer display is based upon the file information in the project file.</span></span> <span data-ttu-id="6eea2-134">По этой причине обозреватель решений будет часто отображаются неверные сведения в случаях, где использовались внешних редакторов.</span><span class="sxs-lookup"><span data-stu-id="6eea2-134">Because of this, the Solution Explorer would often display inaccurate information in cases where external editors were used.</span></span> <span data-ttu-id="6eea2-135">Visual Studio 2002 и 2003 будет часто перезаписывать изменения файла или отображает самую последнюю версию файлов.</span><span class="sxs-lookup"><span data-stu-id="6eea2-135">Visual Studio 2002 and 2003 would often overwrite file changes or not display the most recent version of files.</span></span>

<span data-ttu-id="6eea2-136">Visual Studio 2005 немедленно выполняет в этом файле проекта.</span><span class="sxs-lookup"><span data-stu-id="6eea2-136">Visual Studio 2005 does away with the project file.</span></span> <span data-ttu-id="6eea2-137">Вместо этого она считывает данные файлов и папок непосредственно с диска, приведет к точное отображение файлов в проекте.</span><span class="sxs-lookup"><span data-stu-id="6eea2-137">Instead, it reads the file and folder information directly from the disk, resulting in an accurate display of the files in your project.</span></span> <span data-ttu-id="6eea2-138">Поскольку папки ссылки в Visual Studio 2002 и 2003 не представляет фактический папки веб-приложения, Visual Studio 2005 также удаляет папку ссылки из обозревателя решений.</span><span class="sxs-lookup"><span data-stu-id="6eea2-138">Because the References folder in Visual Studio 2002 and 2003 does not represent an actual folder in your Web application, Visual Studio 2005 also removes the References folder from Solution Explorer.</span></span> <span data-ttu-id="6eea2-139">Для доступа к ссылок проекта в Visual Studio 2005, следует использовать страницы свойств для проекта.</span><span class="sxs-lookup"><span data-stu-id="6eea2-139">To access the references for your project in Visual Studio 2005, you should use the Property pages for the project.</span></span>

## <a name="creating-web-projects"></a><span data-ttu-id="6eea2-140">Создание веб-проектов</span><span class="sxs-lookup"><span data-stu-id="6eea2-140">Creating Web Projects</span></span>

<span data-ttu-id="6eea2-141">Веб-разработчики имеют множество новых возможностей для создания проекта в Visual Studio 2005.</span><span class="sxs-lookup"><span data-stu-id="6eea2-141">Web developers have many new options available for project creation in Visual Studio 2005.</span></span> <span data-ttu-id="6eea2-142">Веб-сайты могут теперь можно создать в любом месте в файловой системе и можно отлаживать или просматривать с помощью ASP.NET Development Server.</span><span class="sxs-lookup"><span data-stu-id="6eea2-142">Web sites can now be created anywhere in the file system and can then be debugged or browsed using the new ASP.NET Development Server.</span></span> <span data-ttu-id="6eea2-143">Разработчики также могут создавать новые веб-сайты, с помощью протокола FTP.</span><span class="sxs-lookup"><span data-stu-id="6eea2-143">Developers can also create new Web sites using FTP.</span></span>

<span data-ttu-id="6eea2-144">Щелкните здесь, чтобы просмотреть пошаговое видео создания веб-проектов в Visual Studio 2005.</span><span class="sxs-lookup"><span data-stu-id="6eea2-144">Click here to view a video walkthrough of creating Web projects in Visual Studio 2005.</span></span>


![](improvements-in-visual-studio-2005/_static/image1.png)


[<span data-ttu-id="6eea2-145">Откройте весь экран</span><span class="sxs-lookup"><span data-stu-id="6eea2-145">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)


### <a name="file-system-projects"></a><span data-ttu-id="6eea2-146">Файл системы проектов</span><span class="sxs-lookup"><span data-stu-id="6eea2-146">File System Projects</span></span>

<span data-ttu-id="6eea2-147">Как было показано в этом примере видео, вы можете создать веб-сайт на локальном компьютере или в удаленном расположении через общую папку файловой системы.</span><span class="sxs-lookup"><span data-stu-id="6eea2-147">As you saw in the video walkthrough, you can choose to create Web sites on the file system either on the local machine or on a remote location via a file share.</span></span> <span data-ttu-id="6eea2-148">Веб-сайты, созданные в файловой системе просматривать и отлаживать с помощью сервера разработки ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6eea2-148">Web sites that are created on the file system are browsed and debugged using the ASP.NET Development Server.</span></span>

> [!NOTE]
> <span data-ttu-id="6eea2-149">ASP.NET Development Server может вызвать некоторую путаницу для клиентов.</span><span class="sxs-lookup"><span data-stu-id="6eea2-149">The ASP.NET Development Server may cause some confusion for customers.</span></span> <span data-ttu-id="6eea2-150">Если веб-проект создается в файловой системе, в структуре каталогов IISs (т. е. c:/inetpub/wwwroot), веб-сайт будет по-прежнему просмотреть с помощью сервера разработки ASP.NET при запуске из Visual Studio 2005.</span><span class="sxs-lookup"><span data-stu-id="6eea2-150">If a Web project is created on the file system in IISs directory structure (i.e. c:/inetpub/wwwroot), the Web site will still be browsed via the ASP.NET Development Server when launched from within Visual Studio 2005.</span></span> <span data-ttu-id="6eea2-151">Таким образом любой конфигурации IIS (т. е. методы проверки подлинности) неприменим.</span><span class="sxs-lookup"><span data-stu-id="6eea2-151">Therefore, any IIS configuration (i.e. authentication methods) is not applicable.</span></span>


<span data-ttu-id="6eea2-152">Веб-проекта по умолчанию также удаляет много дополнительных накладных расходов, включает только страницу Default.aspx, default.cs файл и папка приложения/_Data.</span><span class="sxs-lookup"><span data-stu-id="6eea2-152">The default web project also removes a lot of the overhead by only includes a Default.aspx page, default.cs file, and an App/_Data folder.</span></span> <span data-ttu-id="6eea2-153">При необходимости добавляются web.config и специальных папок (т. е. приложение/_code).</span><span class="sxs-lookup"><span data-stu-id="6eea2-153">The web.config and special folders (i.e. app/_code) are added as they are needed.</span></span> <span data-ttu-id="6eea2-154">Веб-проект включает только файлы и папки, которые необходимы.</span><span class="sxs-lookup"><span data-stu-id="6eea2-154">Your web project only includes the files and folders that you need.</span></span>

### <a name="http-projects"></a><span data-ttu-id="6eea2-155">Проекты HTTP</span><span class="sxs-lookup"><span data-stu-id="6eea2-155">HTTP Projects</span></span>

<span data-ttu-id="6eea2-156">Проекты HTTP может быть проектов, созданных на локальном веб-узла IIS или на удаленном веб-сайте.</span><span class="sxs-lookup"><span data-stu-id="6eea2-156">HTTP projects can either be projects that are created on a local IIS Web site or on a remote Web site.</span></span> <span data-ttu-id="6eea2-157">Расположение проекта по умолчанию является `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="6eea2-157">The default project location is `http://localhost`.</span></span> <span data-ttu-id="6eea2-158">Если нажать кнопку "Обзор", существует два варианта HTTP: IIS на локальном и удаленном сайте.</span><span class="sxs-lookup"><span data-stu-id="6eea2-158">If you click the Browse button, there are two HTTP options: Local IIS and Remote Site.</span></span> <span data-ttu-id="6eea2-159">Основным различием в этих двух параметров — метод, в котором информация веб-сайта отображается в диалоговом окне выберите расположение и в том, как файлы копируются на веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="6eea2-159">The main difference in these two options is the method in which the web site information is displayed in the Choose Location dialog and in how the files are copied to the Web server.</span></span>

<span data-ttu-id="6eea2-160">Параметр локальный сервер IIS считывает данные сайта из метабазы на локальном компьютере и файлы будут скопированы в файловой системе.</span><span class="sxs-lookup"><span data-stu-id="6eea2-160">The Local IIS option reads the site information from the metabase on the local machine and files are copied using the file system.</span></span> <span data-ttu-id="6eea2-161">Параметр удаленного сайта использует серверные расширения FrontPage и сведения о сайте и файлы копируются с помощью протокола HTTP и вызовов RPC расширениями сервера FrontPage.</span><span class="sxs-lookup"><span data-stu-id="6eea2-161">The Remote Site option uses the FrontPage Server Extensions and the site information and files are copied using HTTP and FrontPage Server Extensions RPC calls.</span></span>

> [!NOTE]
> <span data-ttu-id="6eea2-162">Файл vs###/_tmp.htm и get/_aspx/_ver.aspx больше не используется для определения сведений о версии.</span><span class="sxs-lookup"><span data-stu-id="6eea2-162">The vs###/_tmp.htm file and get/_aspx/_ver.aspx are no longer used to determine version information.</span></span>


<span data-ttu-id="6eea2-163">Параметр HTTP по умолчанию — локальный сервер IIS.</span><span class="sxs-lookup"><span data-stu-id="6eea2-163">The default HTTP option is Local IIS.</span></span> <span data-ttu-id="6eea2-164">Этот параметр считывает метабазу IIS, чтобы определить, какие сайты доступны и расположение, в котором для создания содержимого.</span><span class="sxs-lookup"><span data-stu-id="6eea2-164">This option reads the IIS Metabase to determine which sites are available and the location in which to create content.</span></span> <span data-ttu-id="6eea2-165">Можно выбрать другую папку или виртуальный каталог, выбрав его в представлении в виде дерева.</span><span class="sxs-lookup"><span data-stu-id="6eea2-165">You can select a different folder or virtual directory by selecting it in the tree view.</span></span> <span data-ttu-id="6eea2-166">Вы можно также создать новый виртуальный каталог, пометить папки как приложения, а также удалять существующие виртуальные каталоги в этом окне.</span><span class="sxs-lookup"><span data-stu-id="6eea2-166">You can also create a new virtual directory, mark folders as applications, as well as delete existing virtual directories from this dialog box.</span></span>


![Выберите диалоговое окно размещения](improvements-in-visual-studio-2005/_static/image1.gif)

<span data-ttu-id="6eea2-168">**Рис. 1**: выберите диалоговое окно размещения</span><span class="sxs-lookup"><span data-stu-id="6eea2-168">**Figure 1**: The Choose Location Dialog</span></span>


<span data-ttu-id="6eea2-169">В отличие от более ранних версиях Visual Studio, если установить флажок **использование протокола SSL** флажок и SSL-сертификат не соответствует URL-адрес при переходе, откроется диалоговое окно Предупреждение системы безопасности, запрос, если бы Например продолжить.</span><span class="sxs-lookup"><span data-stu-id="6eea2-169">Unlike in earlier versions of Visual Studio, if you check the **Use Secure Sockets Layer** checkbox and the SSL certificate does not match the URL you are browsing, you will be presented with a Security Alert dialog asking you if you would like to proceed.</span></span> <span data-ttu-id="6eea2-170">С помощью Visual Studio .NET 2003, если сертификат не имеет соответствующего один, Создание проекта приведет к сбою.</span><span class="sxs-lookup"><span data-stu-id="6eea2-170">Using Visual Studio .NET 2003, if the certificate was not a matching one, creating the project would fail.</span></span>


![Безопасность предупреждений о SSL-сертификата](improvements-in-visual-studio-2005/_static/image2.gif)

<span data-ttu-id="6eea2-172">**На рисунке 2**: предупреждение системы безопасности о SSL-сертификата</span><span class="sxs-lookup"><span data-stu-id="6eea2-172">**Figure 2**: Security Alert Regarding SSL Certificate</span></span>


### <a name="note-on-host-headers"></a><span data-ttu-id="6eea2-173">Обратите внимание на заголовков узлов</span><span class="sxs-lookup"><span data-stu-id="6eea2-173">Note on Host Headers</span></span>

<span data-ttu-id="6eea2-174">При создании веб-приложения на узле, привязанный к конкретный IP-адрес, необходимо будет проверить, настроен ли заголовок узла.</span><span class="sxs-lookup"><span data-stu-id="6eea2-174">If you are creating a Web application on a site bound to a specific IP, you will need to ensure that a host header is configured.</span></span> <span data-ttu-id="6eea2-175">В противном случае Visual Studio будет создан на сайте по адресу `http://localhost`, но IP-адрес не будут разрешаться правильно при сайта просматривать или отладки из интегрированной.</span><span class="sxs-lookup"><span data-stu-id="6eea2-175">Otherwise, Visual Studio will create the site at `http://localhost`, but the IP address will not resolve correctly when the site is browsed or debugged from within the IDE.</span></span>

<span data-ttu-id="6eea2-176">Если выбран параметр удаленного сайта, диалоговое окно изменяет позволяют вводить URL-адрес назначения для нового веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="6eea2-176">If you select the Remote Site option, the dialog changes to allow you to enter the destination URL for the new Web site.</span></span> <span data-ttu-id="6eea2-177">Этот URL-адрес должен быть на сервере, где включены серверные расширения FrontPage.</span><span class="sxs-lookup"><span data-stu-id="6eea2-177">This URL must be on a server that has the FrontPage Server Extensions enabled.</span></span> <span data-ttu-id="6eea2-178">Если вы хотите работать с локального веб-сервере с помощью серверных расширений FrontPage, можно использовать параметр удаленного сайта и указать локальный URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="6eea2-178">If you want to work with your local Web server using the FrontPage Server Extensions, you can use the Remote Site option and specify a local URL.</span></span>


![Создание веб-сайта на удаленном сервере](improvements-in-visual-studio-2005/_static/image1.jpg)

<span data-ttu-id="6eea2-180">**Рис. 3**: Создание веб-сайта на удаленном сервере</span><span class="sxs-lookup"><span data-stu-id="6eea2-180">**Figure 3**: Creating a Web Site on a Remote Server</span></span>


<span data-ttu-id="6eea2-181">Если при создании приложения на удаленном сайте через SSL, SSL-сертификат не соответствует, диалоговое окно подтверждения немного отличается от диалоговое окно отображается при использовании параметра локальный сервер IIS.</span><span class="sxs-lookup"><span data-stu-id="6eea2-181">When creating an application on a remote site via SSL, if the SSL certificate does not match, the confirmation dialog is slightly different than the dialog displayed when using the Local IIS option.</span></span>


![Появление предупреждения системы безопасности удаленного сайта](improvements-in-visual-studio-2005/_static/image3.gif)

<span data-ttu-id="6eea2-183">**Рис. 4**: Появление предупреждения системы безопасности удаленного сайта</span><span class="sxs-lookup"><span data-stu-id="6eea2-183">**Figure 4**: The Remote Site Security Alert</span></span>


<a id="_Toc116100243"></a>

#### <a name="ftp"></a><span data-ttu-id="6eea2-184">FTP</span><span class="sxs-lookup"><span data-stu-id="6eea2-184">FTP</span></span>

<span data-ttu-id="6eea2-185">Visual Studio 2005 предоставляет возможность создавать веб-сайты по протоколу FTP.</span><span class="sxs-lookup"><span data-stu-id="6eea2-185">Visual Studio 2005 introduces the option to create Web sites via FTP.</span></span> <span data-ttu-id="6eea2-186">При использовании этого параметра интегрированной среды разработки локально создает файлы в папке temp пользователей, а затем использует FTP для перемещения файлов с FTP-ресурса.</span><span class="sxs-lookup"><span data-stu-id="6eea2-186">When you use this option, the IDE creates the files locally in the users temp folder and then uses FTP to move the files to the FTP location.</span></span>

> [!NOTE]
> <span data-ttu-id="6eea2-187">Расположение временной папки — c:/Documents and Settings /&lt;пользователя&gt;/локальные параметры/Temp/VWDWebCache/&lt;сервера&gt;/_&lt;имя приложения&gt;</span><span class="sxs-lookup"><span data-stu-id="6eea2-187">The temp folder location is c:/Documents and Settings/&lt;User&gt;/Local Settings/Temp/VWDWebCache/&lt;Server&gt;/_&lt;application name&gt;</span></span>


<span data-ttu-id="6eea2-188">При использовании параметра FTP, откроется диалоговое окно Выбор расположения.</span><span class="sxs-lookup"><span data-stu-id="6eea2-188">When using the FTP option, you will be presented with a Choose Location dialog.</span></span> <span data-ttu-id="6eea2-189">Введите необходимые сведения о подключении FTP в этом диалоговом окне, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="6eea2-189">You enter the required FTP connection information into this dialog as shown below.</span></span>


![Выберите расположение для FTP-сервера](improvements-in-visual-studio-2005/_static/image2.jpg)

<span data-ttu-id="6eea2-191">**Рис. 5**: выберите расположение для FTP-сервера</span><span class="sxs-lookup"><span data-stu-id="6eea2-191">**Figure 5**: The Choose Location Dialog for FTP</span></span>


## <a name="lab-setup-ftp-site-and-create-a-project"></a><span data-ttu-id="6eea2-192">Лаборатории: Настройка FTP сайта, а также создание проекта</span><span class="sxs-lookup"><span data-stu-id="6eea2-192">Lab: Setup FTP site and create a project</span></span>

<span data-ttu-id="6eea2-193">Следующие шаги настройки FTP-сайта, чтобы у пользователя есть только их можно передать по протоколу FTP расположение.</span><span class="sxs-lookup"><span data-stu-id="6eea2-193">The following steps configure the FTP site so that a user has a location that only they can upload to via FTP.</span></span>

### <a name="install-the-ftp-service"></a><span data-ttu-id="6eea2-194">Установить службу FTP</span><span class="sxs-lookup"><span data-stu-id="6eea2-194">Install the FTP Service</span></span>

1. <span data-ttu-id="6eea2-195">Установка и удаление программ, выберите компонентов Windows</span><span class="sxs-lookup"><span data-stu-id="6eea2-195">Open Add Remove Programs, select Add/Remove Windows Components</span></span>
2. <span data-ttu-id="6eea2-196">Выберите Internet Information Services (сервер приложений в Windows 2003) и нажмите кнопку **сведения**.</span><span class="sxs-lookup"><span data-stu-id="6eea2-196">Select Internet Information Services (Application Server on Windows 2003) and click **Details**.</span></span>
3. <span data-ttu-id="6eea2-197">Проверьте **службы протокола передачи файлов (FTP)** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="6eea2-197">Check **File Transfer Protocol (FTP) Service** and click **OK**.</span></span>
4. <span data-ttu-id="6eea2-198">Нажмите кнопку **Далее** для установки службы FTP.</span><span class="sxs-lookup"><span data-stu-id="6eea2-198">Click **Next** to install the FTP service.</span></span>

### <a name="create-a-new-folder-for-content"></a><span data-ttu-id="6eea2-199">Создайте новую папку для содержимого</span><span class="sxs-lookup"><span data-stu-id="6eea2-199">Create a New Folder for Content</span></span>

1. <span data-ttu-id="6eea2-200">В проводнике Windows создайте новую папку с именем **User1** внутри c:/inetpub/wwwroot.</span><span class="sxs-lookup"><span data-stu-id="6eea2-200">In Windows Explorer, create a new folder called **User1** inside of c:/inetpub/wwwroot.</span></span>

#### <a name="configure-folders-and-permissions-on-folders"></a><span data-ttu-id="6eea2-201">Настройка папки и разрешения для папок.</span><span class="sxs-lookup"><span data-stu-id="6eea2-201">Configure folders and permissions on folders.</span></span>

1. <span data-ttu-id="6eea2-202">Откройте оснастку служб IIS с помощью средств администрирования.</span><span class="sxs-lookup"><span data-stu-id="6eea2-202">Open the Internet Information Services snap-in from Administrative Tools.</span></span> <span data-ttu-id="6eea2-203">Теперь вы можете папка FTP-узлы в узле имени компьютера.</span><span class="sxs-lookup"><span data-stu-id="6eea2-203">You will now have an FTP Sites folder under the computer name node.</span></span>
2. <span data-ttu-id="6eea2-204">Разверните **FTP-узлов**.</span><span class="sxs-lookup"><span data-stu-id="6eea2-204">Expand **FTP Sites**.</span></span>
3. <span data-ttu-id="6eea2-205">Щелкните правой кнопкой мыши **FTP-узел по умолчанию**выберите **New**, затем **виртуальный каталог**, нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="6eea2-205">Right-click the **Default FTP Site**, select **New**, then **Virtual Directory**, then click **Next**.</span></span>
4. <span data-ttu-id="6eea2-206">Введите **User1** имя виртуального каталога и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="6eea2-206">Enter **User1** for the virtual directory name and click **Next**.</span></span>
5. <span data-ttu-id="6eea2-207">Введите **c:/inetpub/wwwroot/User1** путь и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="6eea2-207">Enter **c:/inetpub/wwwroot/User1** for the path and click **Next**.</span></span>
6. <span data-ttu-id="6eea2-208">Нажмите кнопку **Далее** и затем **Готово** для завершения работы мастера.</span><span class="sxs-lookup"><span data-stu-id="6eea2-208">Click **Next** and then **Finish** to complete the wizard.</span></span>
7. <span data-ttu-id="6eea2-209">Щелкните правой кнопкой мыши **User1** виртуальный каталог на FTP-узел по умолчанию и выберите **свойства**.</span><span class="sxs-lookup"><span data-stu-id="6eea2-209">Right-click the **User1** virtual directory under Default FTP Site and select **Properties**.</span></span>
8. <span data-ttu-id="6eea2-210">Проверьте **записи** флажок и нажмите кнопку **ОК** чтобы закрыть диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="6eea2-210">Check the **Write** checkbox and click **OK** to close the dialog.</span></span>
9. <span data-ttu-id="6eea2-211">Щелкните правой кнопкой мыши **FTP-узел по умолчанию** и выберите **свойства**.</span><span class="sxs-lookup"><span data-stu-id="6eea2-211">Right-click **Default FTP Site** and select **Properties**.</span></span>
10. <span data-ttu-id="6eea2-212">На **учетные записи безопасности** , установите флажок **Разрешить анонимные подключения**.</span><span class="sxs-lookup"><span data-stu-id="6eea2-212">On the **Security Accounts** tab, uncheck **Allow Anonymous Connections**.</span></span>
11. <span data-ttu-id="6eea2-213">Нажмите кнопку **Да** в диалоговом окне с вопросом, следует продолжить.</span><span class="sxs-lookup"><span data-stu-id="6eea2-213">Click **Yes** in the dialog asking if you want to continue.</span></span>
12. <span data-ttu-id="6eea2-214">Нажмите кнопку **ОК** чтобы закрыть диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="6eea2-214">Click **OK** to close the dialog.</span></span>
13. <span data-ttu-id="6eea2-215">Разверните **веб-сайт по умолчанию** под **веб-сайтов** узла.</span><span class="sxs-lookup"><span data-stu-id="6eea2-215">Expand the **Default Web Site** under the **Web Sites** node.</span></span>
14. <span data-ttu-id="6eea2-216">Щелкните правой кнопкой мыши **User1** каталог и выберите **свойства**</span><span class="sxs-lookup"><span data-stu-id="6eea2-216">Right-click the **User1** directory and select **Properties**</span></span>
15. <span data-ttu-id="6eea2-217">В **параметры приложения** щелкните **создать** пометить папки как приложение.</span><span class="sxs-lookup"><span data-stu-id="6eea2-217">In the **Application Settings** section, click **Create** to mark the folder as an application.</span></span>
16. <span data-ttu-id="6eea2-218">Нажмите кнопку **ОК** чтобы закрыть диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="6eea2-218">Click **OK** to close the dialog.</span></span>
17. <span data-ttu-id="6eea2-219">Закройте оснастку служб IIS.</span><span class="sxs-lookup"><span data-stu-id="6eea2-219">Close the Internet Information Services snap-in.</span></span>

### <a name="create-web-project"></a><span data-ttu-id="6eea2-220">Создание веб-проекта</span><span class="sxs-lookup"><span data-stu-id="6eea2-220">Create web project</span></span>

1. <span data-ttu-id="6eea2-221">Откройте Visual Studio 2005.</span><span class="sxs-lookup"><span data-stu-id="6eea2-221">Open Visual Studio 2005.</span></span>
2. <span data-ttu-id="6eea2-222">Из **файл** последовательно выберите пункты **новый веб-сайт**.</span><span class="sxs-lookup"><span data-stu-id="6eea2-222">From the **File** menu, select **New Web Site**.</span></span>
3. <span data-ttu-id="6eea2-223">В **расположение** раскрывающийся список, выберите **FTP**.</span><span class="sxs-lookup"><span data-stu-id="6eea2-223">In the **Location** dropdown, select **FTP**.</span></span>
4. <span data-ttu-id="6eea2-224">Нажмите кнопку **Обзор**.</span><span class="sxs-lookup"><span data-stu-id="6eea2-224">Click **Browse**.</span></span>
5. <span data-ttu-id="6eea2-225">Введите **localhost** в **сервера** текстового поля.</span><span class="sxs-lookup"><span data-stu-id="6eea2-225">Enter **localhost** in the **Server** textbox.</span></span>
6. <span data-ttu-id="6eea2-226">Введите **User1** в текстовом поле каталога.</span><span class="sxs-lookup"><span data-stu-id="6eea2-226">Enter **User1** in the Directory textbox.</span></span>
7. <span data-ttu-id="6eea2-227">Нажмите кнопку **откройте**.</span><span class="sxs-lookup"><span data-stu-id="6eea2-227">Click **Open**.</span></span> <span data-ttu-id="6eea2-228">В диалоговом окне Новый веб-сайт будет введен адрес FTP.</span><span class="sxs-lookup"><span data-stu-id="6eea2-228">The FTP location will be entered into the New Web Site dialog.</span></span>
8. <span data-ttu-id="6eea2-229">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="6eea2-229">Click **OK**.</span></span>
9. <span data-ttu-id="6eea2-230">Снимите флажок **анонимный вход** в диалоговом окне вход в FTP, введите свои учетные данные и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="6eea2-230">Uncheck **Anonymous log on** in the FTP Log On dialog, enter your credentials, and click **OK**.</span></span>
10. <span data-ttu-id="6eea2-231">Что такое URL-адрес для проекта?</span><span class="sxs-lookup"><span data-stu-id="6eea2-231">What is the URL for the project?</span></span> <span data-ttu-id="6eea2-232">(URL-адрес для проекта будет отображаться в обозревателе решений.)</span><span class="sxs-lookup"><span data-stu-id="6eea2-232">(The URL for the project will be displayed in Solution Explorer.)</span></span>
11. <span data-ttu-id="6eea2-233">Из **построения** последовательно выберите пункты **построить веб-узел** или **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="6eea2-233">From the **Build** menu, select **Build Web Site** or **Build Solution**.</span></span>
12. <span data-ttu-id="6eea2-234">Щелкните правой кнопкой мыши на Default.aspx в обозревателе решений и выберите **Просмотр в браузере**.</span><span class="sxs-lookup"><span data-stu-id="6eea2-234">Right-click on Default.aspx in Solution Explorer and select **View in Browser**.</span></span>
13. <span data-ttu-id="6eea2-235">В диалоговом окне требуется URL-адрес веб-сайта, введите `http://localhost/user1` URL-адрес и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="6eea2-235">In the Web Site URL Required dialog, enter `http://localhost/user1` for the URL and click **OK**.</span></span>

> [!NOTE]
> <span data-ttu-id="6eea2-236">Если возникает ошибка, указывающими на невозможность загрузки /_Default тип, убедитесь, что запущены ASP.NET 2.0 на веб-сайта и не более ранней версии.</span><span class="sxs-lookup"><span data-stu-id="6eea2-236">If you get a error indicating an inability to load the type /_Default, make sure that you are running ASP.NET 2.0 on your Web site and not an earlier version.</span></span> <span data-ttu-id="6eea2-237">Это можно сделать на вкладке ASP.NET в службах IIS.</span><span class="sxs-lookup"><span data-stu-id="6eea2-237">You can do that from the ASP.NET tab in Internet Information Services.</span></span>


## <a name="opening-web-projects"></a><span data-ttu-id="6eea2-238">Открытие веб-проектов</span><span class="sxs-lookup"><span data-stu-id="6eea2-238">Opening Web Projects</span></span>

<span data-ttu-id="6eea2-239">Открытие веб-проектов похоже на создание проектов.</span><span class="sxs-lookup"><span data-stu-id="6eea2-239">Opening Web projects is similar to creating projects.</span></span> <span data-ttu-id="6eea2-240">Следующие разделы указывают области следить out для при работе в Интегрированной среде разработки.</span><span class="sxs-lookup"><span data-stu-id="6eea2-240">The following sections call out areas to keep an eye out for while working within the IDE.</span></span> <span data-ttu-id="6eea2-241">Он также рассматривается работа с веб-проектов с помощью HTTP и FTP.</span><span class="sxs-lookup"><span data-stu-id="6eea2-241">It also covers working with Web projects using HTTP and FTP.</span></span>

<span data-ttu-id="6eea2-242">Открыть веб-проект, выберите в меню "файл" Открыть веб-сайт.</span><span class="sxs-lookup"><span data-stu-id="6eea2-242">To open a Web project, select Open Web Site from the File menu.</span></span> <span data-ttu-id="6eea2-243">Появится с то же диалоговое окно Выбор расположения, описанные ранее и у вас есть же четыре возможности, доступные: файловая система, локальный сервер IIS, FTP и удаленный узел.</span><span class="sxs-lookup"><span data-stu-id="6eea2-243">You will be prompted with the same Choose Location dialog covered previously and you have the same four options available to you: File System, Local IIS, FTP, and Remote Site.</span></span>

<a id="_Toc116100245"></a>

## <a name="file-system"></a><span data-ttu-id="6eea2-244">Файловая система</span><span class="sxs-lookup"><span data-stu-id="6eea2-244">File System</span></span>

<span data-ttu-id="6eea2-245">Как указывалось ранее в этом модуле, Visual Studio больше не использует файл проекта.</span><span class="sxs-lookup"><span data-stu-id="6eea2-245">As indicated previously in this module, Visual Studio no longer uses a project file.</span></span> <span data-ttu-id="6eea2-246">Таким образом Если открыть веб-сайт из файловой системы, фактически имеется возможность выбрать любую папку, к которой необходимо, даже если папка не был создан как веб-проекта изначально в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6eea2-246">Therefore, if you choose to open a Web site from the file system, you actually have the option of choosing any folder that you wish, even if the folder you choose was not created as a Web project initially in Visual Studio.</span></span> <span data-ttu-id="6eea2-247">Например вы можете открыть папку "Мои документы" как веб-сайт и Visual Studio к счастью открыть его и отображение файлов, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="6eea2-247">For example, you can choose to open the My Documents folder as a Web site and Visual Studio will happily open it and display your files as shown below.</span></span>


![Мои документы, открытые как веб-сайта](improvements-in-visual-studio-2005/_static/image3.jpg)

<span data-ttu-id="6eea2-249">**Рис. 6**: *Мои документы* открывается как веб-сайта</span><span class="sxs-lookup"><span data-stu-id="6eea2-249">**Figure 6**: *My Documents* Opened As a Web Site</span></span>


<span data-ttu-id="6eea2-250">Поскольку Visual Studio создает только дополнительные файлы и папки, при необходимости, дополнительные файлы или папки добавляются в нужное место при открытии.</span><span class="sxs-lookup"><span data-stu-id="6eea2-250">Because Visual Studio only creates additional files and folders when necessary, no additional files or folders are added to the location you open.</span></span> <span data-ttu-id="6eea2-251">Побочным эффектом данной архитектуры является, что предотвращает вложенности веб-сайтов в файловой системе.</span><span class="sxs-lookup"><span data-stu-id="6eea2-251">A side-effect of this architecture is that it prevents you from nesting Web sites on the file system.</span></span> <span data-ttu-id="6eea2-252">Например рассмотрим следующую структуру каталогов.</span><span class="sxs-lookup"><span data-stu-id="6eea2-252">For example, consider the following directory structure.</span></span>

<span data-ttu-id="6eea2-253">Веб-проект в C:/Мой_веб</span><span class="sxs-lookup"><span data-stu-id="6eea2-253">Web project at C:/MyWebSite</span></span>

<span data-ttu-id="6eea2-254">Другой веб-проект, в C:/Мой_веб-Nested.</span><span class="sxs-lookup"><span data-stu-id="6eea2-254">Another web project at C:/MyWebSite/Nested</span></span>

<span data-ttu-id="6eea2-255">При открытии веб-сайте c:/Мой_веб вложенная папка будет отображаться как вложенная папка этого приложения.</span><span class="sxs-lookup"><span data-stu-id="6eea2-255">When you open the Web site at c:/MyWebSite, the Nested folder will appear as a sub-folder of that application.</span></span>

<a id="_Toc116100246"></a>

## <a name="http"></a><span data-ttu-id="6eea2-256">HTTP</span><span class="sxs-lookup"><span data-stu-id="6eea2-256">HTTP</span></span>

<span data-ttu-id="6eea2-257">При открытии веб-сайтах через HTTP, параметры доступны для чтения в метабазе IIS (локальный сервер IIS) или с помощью серверных расширений FrontPage (удаленный узел). Если имеются вложенные веб-приложений, они отображаются также со значком, который идентифицирует их как приложения.</span><span class="sxs-lookup"><span data-stu-id="6eea2-257">When opening Web sites via HTTP, settings are read either from the IIS metabase (Local IIS) or using FrontPage Server Extensions (Remote Site.) If there are nested web applications, these are displayed as well with an icon identifying them as an application.</span></span> <span data-ttu-id="6eea2-258">Если вы знакомы с работой веб-приложений в Microsoft FrontPage в Visual Studio 2005 аналогичен.</span><span class="sxs-lookup"><span data-stu-id="6eea2-258">If you are familiar with working with web applications in FrontPage, the behavior in Visual Studio 2005 is similar.</span></span>

<span data-ttu-id="6eea2-259">Несмотря на то, что Visual Studio будет отображен значок для приложений, которые будут размещены под ней приложение, которое в настоящее время открыт в среде IDE, он не позволяет вам развернуть их, чтобы просмотреть их содержимое.</span><span class="sxs-lookup"><span data-stu-id="6eea2-259">Even though Visual Studio will display an icon for applications that are nested beneath the application that is currently opened within the IDE, it will not allow you to expand them to see their content.</span></span> <span data-ttu-id="6eea2-260">Тем не менее, можно дважды щелкните на них, чтобы открыть их.</span><span class="sxs-lookup"><span data-stu-id="6eea2-260">You can, however, double-click on them to open them.</span></span> <span data-ttu-id="6eea2-261">При этом вы откроется диалоговое окно, предлагающее либо открыть веб-приложение (и замените открытого решения), или добавить веб-приложения в текущем решении.</span><span class="sxs-lookup"><span data-stu-id="6eea2-261">When you do, you will be presented with a dialog prompting you to either open the web application (and replace the currently open solution) or add the Web application to your current solution.</span></span>


![Дважды щелкните значок вложенного приложения представляется с помощью этого диалогового окна](improvements-in-visual-studio-2005/_static/image4.jpg)

<span data-ttu-id="6eea2-263">**Рис. 7**: дважды щелкните значок вложенного приложения представляется с помощью этого диалогового окна</span><span class="sxs-lookup"><span data-stu-id="6eea2-263">**Figure 7**: Double-clicking a nested application icon presents you with this dialog</span></span>


<a id="_Toc116100247"></a>

## <a name="ftp-site"></a><span data-ttu-id="6eea2-264">FTP-сайт</span><span class="sxs-lookup"><span data-stu-id="6eea2-264">FTP Site</span></span>

<span data-ttu-id="6eea2-265">При открытии узлу по протоколу FTP, файлы всех копируются на локальный компьютер к временной папке.</span><span class="sxs-lookup"><span data-stu-id="6eea2-265">When you open a site via FTP, the files are all copied locally to your temp folder.</span></span> <span data-ttu-id="6eea2-266">Полный путь к расположению локального хранилища отображается в панели «Свойства» для проекта и создается в следующем формате.</span><span class="sxs-lookup"><span data-stu-id="6eea2-266">The full path for the local storage location is displayed in the Properties pane for the project and is created using the following format.</span></span>

<span data-ttu-id="6eea2-267">C:/Documents and Settings /&lt;пользователя&gt;/локальные параметры/Temp/VWDWebCache/&lt;сервера&gt;/_&lt;имя приложения&gt;</span><span class="sxs-lookup"><span data-stu-id="6eea2-267">C:/Documents and Settings/&lt;User&gt;/Local Settings/Temp/VWDWebCache/&lt;Server&gt;/_&lt;application name&gt;</span></span>

<span data-ttu-id="6eea2-268">При использовании FTP, Visual Studio будет необходимо указать базовый URL-адрес проекта, чтобы вы могли просмотреть, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="6eea2-268">When using FTP, Visual Studio will need to specify the base URL for your project so that you can browse it as shown below.</span></span> <span data-ttu-id="6eea2-269">Если базовый URL-адрес не указан, Visual Studio запросит его при первом просмотре страницы в веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="6eea2-269">If you do not specify a base URL, Visual Studio will ask you for it the first time you attempt to browse a page in the Web site.</span></span>


![Указание базового URL-адреса для FTP-узлов](improvements-in-visual-studio-2005/_static/image5.jpg)

<span data-ttu-id="6eea2-271">**Рис. 8**: указание базового URL-адреса для FTP-узлов</span><span class="sxs-lookup"><span data-stu-id="6eea2-271">**Figure 8**: Specifying a Base URL for FTP Sites</span></span>


## <a name="improvements-in-compilation"></a><span data-ttu-id="6eea2-272">Усовершенствования в компиляции</span><span class="sxs-lookup"><span data-stu-id="6eea2-272">Improvements in Compilation</span></span>

<span data-ttu-id="6eea2-273">Работа с веб-приложений в Visual Studio 2005 — заметно быстрее, чем в предыдущих версиях.</span><span class="sxs-lookup"><span data-stu-id="6eea2-273">Working with Web applications in Visual Studio 2005 is noticeably faster than previous versions.</span></span> <span data-ttu-id="6eea2-274">Причиной этого является в небольшой частью нет изменений в архитектуре компиляции.</span><span class="sxs-lookup"><span data-stu-id="6eea2-274">This is due in no small part to the changes in compilation architecture.</span></span>

<span data-ttu-id="6eea2-275">В Visual Studio 2002 и 2003 веб-приложений были скомпилированы в один основную сборку, которая находится в папке/Bin.</span><span class="sxs-lookup"><span data-stu-id="6eea2-275">In Visual Studio 2002 and 2003, Web applications were compiled into one primary assembly residing in the /bin folder.</span></span> <span data-ttu-id="6eea2-276">В Visual Studio 2005 был добавлен в папку приложения и _Code.</span><span class="sxs-lookup"><span data-stu-id="6eea2-276">In Visual Studio 2005, an App/_Code folder was added.</span></span> <span data-ttu-id="6eea2-277">Классы и любой другой код без пользовательского интерфейса добавляются в папку приложения и _Code.</span><span class="sxs-lookup"><span data-stu-id="6eea2-277">Classes and other non-UI code are added to the App/_Code folder.</span></span> <span data-ttu-id="6eea2-278">Если Visual Studio создает проект, все файлы в папке приложения или _Code компилируются в единый файл App/_Code.dll.</span><span class="sxs-lookup"><span data-stu-id="6eea2-278">When Visual Studio builds the project, all files in the App/_Code folder are compiled into a single App/_Code.dll file.</span></span> <span data-ttu-id="6eea2-279">Результат этого изменения является намного быстрее, чем в предыдущих версиях последующие сборки.</span><span class="sxs-lookup"><span data-stu-id="6eea2-279">The result of this change is that subsequent builds are much faster than in previous versions.</span></span>

> [!NOTE]
> <span data-ttu-id="6eea2-280">Программу командной строки MSBuild может также использоваться для создания приложений ASP.NET Web.</span><span class="sxs-lookup"><span data-stu-id="6eea2-280">The MSBuild command line utility can also be used to build ASP.NET Web applications.</span></span> <span data-ttu-id="6eea2-281">Это средство будет рассматриваться в модуль 9.</span><span class="sxs-lookup"><span data-stu-id="6eea2-281">That tool will be covered in module 9.</span></span>


<span data-ttu-id="6eea2-282">Другое усовершенствование компиляции является параметром новое построение в меню Построение.</span><span class="sxs-lookup"><span data-stu-id="6eea2-282">Another compilation enhancement is the new Build Page option on the Build menu.</span></span> <span data-ttu-id="6eea2-283">Эта функция позволяет разработчику Перестроить только текущей страницы (вместе с, курс и зависимостей), чтобы изменения могут компилироваться быстрее.</span><span class="sxs-lookup"><span data-stu-id="6eea2-283">This feature allows a developer to rebuild only the current page (along with, of course, and dependencies) so that changes can be compiled more quickly.</span></span> <span data-ttu-id="6eea2-284">Поскольку C# не предлагает фоновая компиляция по требованию для целей обновления IntelliSense, т. д., они поможет нам эта функция так, как появится возможность для IntelliSense, чтобы быстро обновить просто перестроения на одной странице.</span><span class="sxs-lookup"><span data-stu-id="6eea2-284">Because C# does not offer background compilation for purposes of updating IntelliSense, etc., they will benefit immensely from this feature because it will allow for IntelliSense to be updated quickly by simply rebuilding a single page.</span></span>

<span data-ttu-id="6eea2-285">Свойства построения для проекта позволяют настраивать тип построения, которое происходит перед выполнением стартовой страницы.</span><span class="sxs-lookup"><span data-stu-id="6eea2-285">The Build properties for a project allow you to configure the type of build that occurs before the startup page is executed.</span></span> <span data-ttu-id="6eea2-286">Разработчики могут выбрать построение только текущую страницу, чтобы быстрее отладку приложений после изменений кода можно запустить Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6eea2-286">Developers can choose to only build the current page so that Visual Studio can start debugging applications more quickly after code changes.</span></span>


![Действие построения начало страницы](improvements-in-visual-studio-2005/_static/image6.jpg)

<span data-ttu-id="6eea2-288">**Рис. 9**: действие построения начало страницы</span><span class="sxs-lookup"><span data-stu-id="6eea2-288">**Figure 9**: The Build Page Start Action</span></span>


<span data-ttu-id="6eea2-289">Другое большое усовершенствование Visual Studio и архитектура ASP.NET в области редактирования и продолжить.</span><span class="sxs-lookup"><span data-stu-id="6eea2-289">Another great enhancement to Visual Studio and the ASP.NET architecture is in the area of edit and continue.</span></span> <span data-ttu-id="6eea2-290">В Visual Studio 2005 разработчики могут начать отладку проекта и внести изменения кода в проекте без отсоединения отладчика.</span><span class="sxs-lookup"><span data-stu-id="6eea2-290">In Visual Studio 2005, developers can start debugging a project and make code changes on the project without detaching the debugger.</span></span> <span data-ttu-id="6eea2-291">На самом деле буквально, можно начать отладку проекта, добавьте новый класс, добавьте код для этого класса, добавьте код на странице, которая создает новый экземпляр этого класса и выполнить метод класса, все без отсоединения отладчика.</span><span class="sxs-lookup"><span data-stu-id="6eea2-291">In fact, you can literally start debugging a project, add a new class, add code to that class, add code to your page that creates a new instance of that class and execute a method of the class, all without detaching the debugger.</span></span> <span data-ttu-id="6eea2-292">Новый код выполняется практически так же легко, как обновить браузер!</span><span class="sxs-lookup"><span data-stu-id="6eea2-292">Executing the new code is literally as easy as refreshing the browser!</span></span>

<span data-ttu-id="6eea2-293">Щелкните здесь, чтобы просмотреть пошаговое видеоруководство правки и продолжения компонентов в Visual Studio 2005.</span><span class="sxs-lookup"><span data-stu-id="6eea2-293">Click here to see a video walkthrough of the edit and continue feature in Visual Studio 2005.</span></span>


![](improvements-in-visual-studio-2005/_static/image2.png)


[<span data-ttu-id="6eea2-294">Откройте весь экран</span><span class="sxs-lookup"><span data-stu-id="6eea2-294">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)


<span data-ttu-id="6eea2-295">Надежные изменить и продолжить функциональные возможности в ASP.NET 2.0 и Visual Studio 2005 происходит из-за изменения для приложений ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6eea2-295">The robust edit and continue functionality in ASP.NET 2.0 and Visual Studio 2005 is due to an architectural change for ASP.NET applications.</span></span> <span data-ttu-id="6eea2-296">В ASP.NET 1.x, приложения, созданные в Visual Studio 2002/2003 были скомпилированы в основную сборку, которая была сохранена в папке/Bin.</span><span class="sxs-lookup"><span data-stu-id="6eea2-296">In ASP.NET 1.x, applications created in Visual Studio 2002/2003 were compiled into a primary assembly that was stored in the /bin folder.</span></span> <span data-ttu-id="6eea2-297">Все классы, страницы, т. д. для компиляции приложения в этом одну DLL.</span><span class="sxs-lookup"><span data-stu-id="6eea2-297">All classes, pages, etc. for the application were compiled into that one DLL.</span></span> <span data-ttu-id="6eea2-298">Затем во время выполнения ASP.NET следует компилировать все элементы управления, разметку и код ASP.NET на страницах и скопируйте эти библиотеки DLL в папке ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6eea2-298">Then at runtime, ASP.NET would compile all of the controls, markup, and ASP.NET code within pages and copy those DLLs into the ASP.NET temporary folder.</span></span>

<span data-ttu-id="6eea2-299">В Visual Studio 2005 с помощью ASP.NET 2.0, две компиляции моделей структуры выше (один для Visual Studio) и один для ASP.NET во время выполнения были объединены в одну общую модель компиляции.</span><span class="sxs-lookup"><span data-stu-id="6eea2-299">In Visual Studio 2005 using ASP.NET 2.0, the two compilation models outline above (one for Visual Studio and one for ASP.NET at runtime) have been merged into one common compilation model.</span></span> <span data-ttu-id="6eea2-300">Это означает, что все ошибки компиляции теперь перехватываются во время стадии разработки, а не во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="6eea2-300">That means that all compilation issues are now caught during the development stage instead of at runtime.</span></span> <span data-ttu-id="6eea2-301">Она также позволяет для конструктора и поддержку технологии IntelliSense для функции, такие как пользовательские элементы управления и главные страницы.</span><span class="sxs-lookup"><span data-stu-id="6eea2-301">It also allows for designer and IntelliSense support for features such as user controls and master pages.</span></span>

<span data-ttu-id="6eea2-302">Щелкните здесь, чтобы просмотреть пошаговое видео поддержку конструктора для пользовательских элементов управления.</span><span class="sxs-lookup"><span data-stu-id="6eea2-302">Click here to see a video walkthrough of designer support for user controls.</span></span>


![](improvements-in-visual-studio-2005/_static/image3.png)


[<span data-ttu-id="6eea2-303">Откройте весь экран</span><span class="sxs-lookup"><span data-stu-id="6eea2-303">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)


> [!NOTE]
> <span data-ttu-id="6eea2-304">Когда пользовательский элемент управления удаляется из страницы, @Register директива остается в разметке и должны быть удалены вручную во избежание ошибок синтаксического анализа, если пользовательский элемент управления удаляется с веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="6eea2-304">When a user control is removed from a page, the @Register directive remains in the markup and should be removed manually in order to avoid parser errors if the user control is deleted from the Web site.</span></span>


<span data-ttu-id="6eea2-305">Другой улучшения в модели компиляции Visual Studio — это функция публикации веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="6eea2-305">Another improvement in the Visual Studio compilation model is the Publish Web Site feature.</span></span> <span data-ttu-id="6eea2-306">Так как функция публикации выполняет предварительную компиляцию веб-сайта, разработчики получили доступ добавлены производительность не компилируя ничего по требованию.</span><span class="sxs-lookup"><span data-stu-id="6eea2-306">Because the Publish feature precompiles the Web site, developers can enjoy the added performance of not having to compile anything on demand.</span></span> <span data-ttu-id="6eea2-307">Он также выполняет предварительную компиляцию всех исходного кода в папке приложения или _Code в библиотеку DLL, чтобы исходный код должен быть развернут.</span><span class="sxs-lookup"><span data-stu-id="6eea2-307">It also precompiles all source code in the App/_Code folder into a DLL so that no source code has to be deployed.</span></span>


![Диалоговое окно публикации веб-сайта](improvements-in-visual-studio-2005/_static/image7.jpg)

<span data-ttu-id="6eea2-309">**Рис. 10**: диалоговое окно публикации веб-сайта</span><span class="sxs-lookup"><span data-stu-id="6eea2-309">**Figure 10**: The Publish Web Site Dialog</span></span>


> [!NOTE]
> <span data-ttu-id="6eea2-310">Программа aspnet/_compile.exe может также использоваться для предварительной компиляции веб-приложения ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6eea2-310">The aspnet/_compile.exe utility can also be used to pre-compile an ASP.NET Web application.</span></span> <span data-ttu-id="6eea2-311">Это средство будет рассматриваться в модуль 9.</span><span class="sxs-lookup"><span data-stu-id="6eea2-311">That tool will be covered in module 9.</span></span>


<span data-ttu-id="6eea2-312">При публикации веб-сайта, предварительно скомпилированные файлы хранятся в папке «Temporary ASP.NET Files», как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="6eea2-312">When you Publish a Web site, the precompiled files are stored in the Temporary ASP.NET Files folder as shown below.</span></span> <span data-ttu-id="6eea2-313">Файлы с *.compiled* расширение файла являются XML-файлы, которые Определение зависимостей для определенных библиотек DLL.</span><span class="sxs-lookup"><span data-stu-id="6eea2-313">Files with a *.compiled* file extension are XML files that define dependencies for particular DLLs.</span></span> <span data-ttu-id="6eea2-314">Все элементы управления веб-форма или пользователь компилируются в случайные библиотеки, которые начинаются с *приложения или_веб-_*.</span><span class="sxs-lookup"><span data-stu-id="6eea2-314">Any Webform or user controls are compiled into random DLLs that begin with *App/_Web/_*.</span></span>

<span data-ttu-id="6eea2-315">Если оставить *разрешить это предварительно скомпилированному сайту быть обновляемым* флажок установлен, разметки внутри вашей веб-форм и пользовательских элементов управления не будет предварительно компилируется в DLL, что позволит вносить изменения после развертывания.</span><span class="sxs-lookup"><span data-stu-id="6eea2-315">If you leave the *Allow this precompiled site to be updatable* checkbox checked, markup inside of your Webforms and user controls will not be pre-compiled into a DLL allowing you to make changes after deployment.</span></span> <span data-ttu-id="6eea2-316">Если вы хотите использовать для блокировки разметку, чтобы не допускаются изменения развернутым содержимым, снимите этот флажок.</span><span class="sxs-lookup"><span data-stu-id="6eea2-316">If you would prefer to lock down the markup so that changes to the deployed content are not allowed, uncheck this box.</span></span>

<span data-ttu-id="6eea2-317">*Использовать фиксированное именование и одной сборки страницы* флажок позволяет отключить пакетную компиляцию, чтобы каждая страница компилируется в сборку с именем фиксированной.</span><span class="sxs-lookup"><span data-stu-id="6eea2-317">The *Use fixed naming and single page assemblies* checkbox allows you to disable batch compilation so that each page is compiled into a fixed-named assembly.</span></span> <span data-ttu-id="6eea2-318">Если оставить этот флажок снят флажок позволяет воспользоваться преимуществами пакетной компиляции.</span><span class="sxs-lookup"><span data-stu-id="6eea2-318">Leaving this box unchecked allows you to take advantage of batch compilation.</span></span>

<span data-ttu-id="6eea2-319">*Включить строгое именование в предварительно скомпилированные сборки* флажок позволяет строгое имя предварительно скомпилированных сборок.</span><span class="sxs-lookup"><span data-stu-id="6eea2-319">The *Enable strong naming on precompiled assemblies* checkbox allows you to strong-name your precompiled assemblies.</span></span>

> [!NOTE]
> <span data-ttu-id="6eea2-320">В ASP.NET 1.x, сборки со строгими именами должен быть установлен в глобальный кэш сборок (GAC).</span><span class="sxs-lookup"><span data-stu-id="6eea2-320">In ASP.NET 1.x, strong-named assemblies had to be installed into the Global Assembly Cache (GAC).</span></span> <span data-ttu-id="6eea2-321">В ASP.NET 2.0 вы не требуются для установки сборки со строгими именами в глобальный кэш СБОРОК.</span><span class="sxs-lookup"><span data-stu-id="6eea2-321">In ASP.NET 2.0, you are not required to install strong-named assemblies into the GAC.</span></span>


![Файлы предварительно скомпилированного приложения ASP.NET](improvements-in-visual-studio-2005/_static/image8.jpg)

<span data-ttu-id="6eea2-323">**Рис. 11**: файлы предварительно скомпилированного приложения ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6eea2-323">**Figure 11**: An ASP.NET Applications Pre-Compiled Files</span></span>


> [!NOTE]
> <span data-ttu-id="6eea2-324">В приложении выше отсутствовал файл web.config.</span><span class="sxs-lookup"><span data-stu-id="6eea2-324">In the application above, there was no web.config file.</span></span> <span data-ttu-id="6eea2-325">Если был установлен, он может быть вызвана *PrecompiledApp.config* после публикации веб-сайта сайта процесса.</span><span class="sxs-lookup"><span data-stu-id="6eea2-325">If there had been, it would have been called *PrecompiledApp.config* after the Publish Web site process.</span></span>


## <a name="improvements-in-deployment"></a><span data-ttu-id="6eea2-326">Усовершенствования в развертывании</span><span class="sxs-lookup"><span data-stu-id="6eea2-326">Improvements in Deployment</span></span>

<span data-ttu-id="6eea2-327">Как с помощью Visual Studio 2002 и 2003, Visual Studio 2005 предоставляет возможность Копировать проект.</span><span class="sxs-lookup"><span data-stu-id="6eea2-327">As with Visual Studio 2002 and 2003, Visual Studio 2005 offers a Copy Project feature.</span></span> <span data-ttu-id="6eea2-328">Однако компонент была усилена в Visual Studio 2005 и теперь называется копирования веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="6eea2-328">However, the feature has been beefed up in Visual Studio 2005 and is now called Copy Web Site.</span></span>

<span data-ttu-id="6eea2-329">Диалоговое окно копирования веб-сайта делится на левой и правой части.</span><span class="sxs-lookup"><span data-stu-id="6eea2-329">The Copy Web Site dialog is split into a left frame and a right frame.</span></span> <span data-ttu-id="6eea2-330">Левая часть называется исходный веб-сайт а рамки справа удаленного веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="6eea2-330">The left frame is called the Source Web Site and the right frame is called the Remote Web Site.</span></span> <span data-ttu-id="6eea2-331">Единственное, что может запутать некоторым разработчикам является отображается в правой части сайта не обязательно удаленного узла.</span><span class="sxs-lookup"><span data-stu-id="6eea2-331">One thing that may confuse some developers is that the site displayed in the right frame is not necessarily a remote site.</span></span> <span data-ttu-id="6eea2-332">Это может быть сайта в локальной файловой системе или на локальном экземпляре служб IIS.</span><span class="sxs-lookup"><span data-stu-id="6eea2-332">It could be a site on the local file system or on the local instance of IIS.</span></span> <span data-ttu-id="6eea2-333">Кроме того, сайта, отображаемое в левом окне не обязательно исходного веб-сайта, так как диалоговое окно позволяет публиковать на удаленный веб-сайте *для* исходного веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="6eea2-333">Additionally, the site displayed in the left frame is not necessarily the source Web site because the dialog allows you to publish from the remote Web site *to* the source Web site.</span></span>

<span data-ttu-id="6eea2-334">При копировании проекта на удаленном веб-сайт, узел должен иметь серверные расширения FrontPage, установленными на нем.</span><span class="sxs-lookup"><span data-stu-id="6eea2-334">If you are copying a project to a remote Web site, that site must have the FrontPage Server Extensions installed on it.</span></span> <span data-ttu-id="6eea2-335">Если этого не произошло, необходимо подключиться с помощью протокола FTP.</span><span class="sxs-lookup"><span data-stu-id="6eea2-335">If it does not, you will need to connect using FTP.</span></span> <span data-ttu-id="6eea2-336">С другой стороны при копировании проекта на локальном экземпляре IIS серверные расширения FrontPage не требуются.</span><span class="sxs-lookup"><span data-stu-id="6eea2-336">On the other hand, if you are copying a project to the local IIS instance, FrontPage Server Extensions are not required.</span></span>

> [!NOTE]
> <span data-ttu-id="6eea2-337">Если при попытке создать новый веб-сайт на локальном экземпляре служб IIS и установлены серверные расширения FrontPage 2002, вы получите сообщение об ошибке, сообщающее, что создание веб-сайтов не поддерживается на сервере SharePoint.</span><span class="sxs-lookup"><span data-stu-id="6eea2-337">If you try to create a new Web site on the local IIS instance and the FrontPage 2002 Server Extensions are installed, you will get an error message telling you that creating Web sites is not supported on a SharePoint server.</span></span> <span data-ttu-id="6eea2-338">В этом случае вы можете серверных расширений FrontPage 2000 установка или удаление серверные расширения FrontPage.</span><span class="sxs-lookup"><span data-stu-id="6eea2-338">In that case, you have the option of installing the FrontPage 2000 Server Extensions or removing the FrontPage Server Extensions.</span></span>


<span data-ttu-id="6eea2-339">Щелкните здесь для видеоруководство средства копирования веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="6eea2-339">Click here for a video walkthrough of the Copy Web Site feature.</span></span>


![](improvements-in-visual-studio-2005/_static/image4.png)


[<span data-ttu-id="6eea2-340">Откройте весь экран</span><span class="sxs-lookup"><span data-stu-id="6eea2-340">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/copysite1.wmv)


## <a name="improvements-in-debugging"></a><span data-ttu-id="6eea2-341">Усовершенствования в отладке</span><span class="sxs-lookup"><span data-stu-id="6eea2-341">Improvements in Debugging</span></span>

<span data-ttu-id="6eea2-342">Существует четыре важных улучшений в отладке в Visual Studio 2005.</span><span class="sxs-lookup"><span data-stu-id="6eea2-342">There are four key improvements in debugging in Visual Studio 2005.</span></span>

- <span data-ttu-id="6eea2-343">Отладка локально как администратор возможна без дополнительной настройки.</span><span class="sxs-lookup"><span data-stu-id="6eea2-343">Debugging locally as a non-administrator is possible out of the box.</span></span>
- <span data-ttu-id="6eea2-344">Атрибут Debug для элемента компиляции теперь по умолчанию — false.</span><span class="sxs-lookup"><span data-stu-id="6eea2-344">The Debug attribute for the Compilation element is now false by default.</span></span>
- <span data-ttu-id="6eea2-345">Удаленная отладка и настройка проще, чем до.</span><span class="sxs-lookup"><span data-stu-id="6eea2-345">Remote debugging setup and configuration is easier than before.</span></span>
- <span data-ttu-id="6eea2-346">Теперь можно отлаживать веб-сайт открывается с помощью расположения FTP.</span><span class="sxs-lookup"><span data-stu-id="6eea2-346">You can now debug a Web site opened via an FTP location.</span></span>

## <a name="debugging-as-a-non-administrator"></a><span data-ttu-id="6eea2-347">Отладка как без прав администратора</span><span class="sxs-lookup"><span data-stu-id="6eea2-347">Debugging as a Non-Administrator</span></span>

<span data-ttu-id="6eea2-348">Добавление сервера разработки ASP.NET позволяет не только администраторам легко отлаживать приложения ASP.NET сразу после установки.</span><span class="sxs-lookup"><span data-stu-id="6eea2-348">The addition of the ASP.NET Development Server allows non-administrators to easily debug ASP.NET applications right out of the box.</span></span> <span data-ttu-id="6eea2-349">При отладке приложения ASP.NET, работающего в локальной файловой системе Visual Studio запускается сервер разработки ASP.NET в контексте пользователя, выполнившего вход.</span><span class="sxs-lookup"><span data-stu-id="6eea2-349">When an ASP.NET application running on the local file system is debugged, Visual Studio launches the ASP.NET Development Server under the context of the logged-on user.</span></span> <span data-ttu-id="6eea2-350">Этому пользователю можно отлаживать приложения без дополнительной настройки.</span><span class="sxs-lookup"><span data-stu-id="6eea2-350">That user can then debug that application without any additional configuration.</span></span>

## <a name="debug-is-false-by-default"></a><span data-ttu-id="6eea2-351">Отладка имеет значение False по умолчанию</span><span class="sxs-lookup"><span data-stu-id="6eea2-351">Debug is False by Default</span></span>

<span data-ttu-id="6eea2-352">В ASP.NET 1.x, *отладки* атрибута в *компиляции* было присвоено значение элемента в файле web.config *true* по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6eea2-352">In ASP.NET 1.x, the *debug* attribute in the *compilation* element of the web.config file was set to *true* by default.</span></span> <span data-ttu-id="6eea2-353">Он всегда рекомендации, разработчики присвоить этому атрибуту значение *false* перед развертыванием приложения в рабочей среде, но так как большинство разработчиков не полностью неясны последствия оставить значение атрибута отладки значение true, они просто оставить ее в качестве-является.</span><span class="sxs-lookup"><span data-stu-id="6eea2-353">It has always been recommended that developers set this attribute to *false* before deploying an application to production, but because most developers don't fully understand the consequences of leaving the debug attribute set to true, they simply left it as-is.</span></span>

<span data-ttu-id="6eea2-354">Наиболее серьезные проблемы с входящим атрибут debug, равным true — отключает ASP.NETs пакетной компиляции модели.</span><span class="sxs-lookup"><span data-stu-id="6eea2-354">The most severe problem with having the debug attribute set to true is that it disables ASP.NETs batch compilation model.</span></span> <span data-ttu-id="6eea2-355">Таким образом каждая страница компилируется в отдельной библиотеке DLL.</span><span class="sxs-lookup"><span data-stu-id="6eea2-355">Therefore, each page is compiled into a separate DLL.</span></span> <span data-ttu-id="6eea2-356">Если веб-приложение состоит из тысячи страниц (не знал по любым способом), что означает несколько тысяч небольших библиотек DLL создается этим приложением.</span><span class="sxs-lookup"><span data-stu-id="6eea2-356">If a Web application consists of thousands of pages (not unheard of by any means), that means several thousand small DLLs will be created by that application.</span></span> <span data-ttu-id="6eea2-357">Хотя эти библиотеки DLL, небольшой размер, они не загружались в любой определенного расположения в памяти.</span><span class="sxs-lookup"><span data-stu-id="6eea2-357">While these DLLs are small in size, they are not loaded into any particular location in memory.</span></span> <span data-ttu-id="6eea2-358">Таким образом они привести к фрагментации в системной памяти и могут участвовать в OutOfMemoryException вхождений.</span><span class="sxs-lookup"><span data-stu-id="6eea2-358">Therefore, they cause fragmentation in system memory and can contribute to OutOfMemoryException occurrences.</span></span>

<span data-ttu-id="6eea2-359">В ASP.NET 2.0 атрибут debug присвоено значение false по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6eea2-359">In ASP.NET 2.0, the debug attribute is set to false by default.</span></span> <span data-ttu-id="6eea2-360">Как вы уже видели, когда разработчик выполняет отладку приложения ASP.NET в Visual Studio 2005, ему будет предложено добавить файл web.config с включенной отладкой.</span><span class="sxs-lookup"><span data-stu-id="6eea2-360">As you have already seen, when a developer debugs an ASP.NET application in Visual Studio 2005, they are prompted to add a web.config file with debugging enabled.</span></span> <span data-ttu-id="6eea2-361">Это приводит к же недостатки, которые присутствовали в ASP.NET 1.x, но теперь разработчик четко предупреждается о том, что атрибут следует восстановить false перед перемещением приложения в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="6eea2-361">Doing so incurs the same drawbacks that were present in ASP.NET 1.x, but now the developer is clearly warned that the attribute should be reset to false before moving the application to production.</span></span>

## <a name="remote-debugging-setup-and-configuration"></a><span data-ttu-id="6eea2-362">Установка и настройка удаленной отладки</span><span class="sxs-lookup"><span data-stu-id="6eea2-362">Remote Debugging Setup and Configuration</span></span>

<span data-ttu-id="6eea2-363">В Visual Studio 2002 и 2003, удаленная отладка полагается на диспетчер отладки (mdm.exe) и vs7jit.exe процесса.</span><span class="sxs-lookup"><span data-stu-id="6eea2-363">In Visual Studio 2002/2003, remote debugging relied on the Machine Debug Manager (mdm.exe) and the vs7jit.exe process.</span></span> <span data-ttu-id="6eea2-364">Из-за этого Устранение неполадок удаленной отладки была часто черный прямоугольник для клиентов и часто не было гораздо лучше для службой технической поддержки.</span><span class="sxs-lookup"><span data-stu-id="6eea2-364">Because of that, troubleshooting remote debugging problems was often a black box for customers and it was often not much better for PSS.</span></span>

<span data-ttu-id="6eea2-365">Visual Studio 2005 устраняет зависимость от mdm.exe и vs7jit.exe процессы.</span><span class="sxs-lookup"><span data-stu-id="6eea2-365">Visual Studio 2005 removes the reliance on the mdm.exe and vs7jit.exe processes.</span></span> <span data-ttu-id="6eea2-366">Вместо этого он теперь использует службу монитор удаленной отладки (msvsmon.exe).</span><span class="sxs-lookup"><span data-stu-id="6eea2-366">Instead, it now uses the Remote Debug Monitor service (msvsmon.exe.)</span></span>

<span data-ttu-id="6eea2-367">Требования для удаленной отладки в Visual Studio 2005 достаточно прост.</span><span class="sxs-lookup"><span data-stu-id="6eea2-367">The requirement for debugging in Visual Studio 2005 remotely is quite simple.</span></span> <span data-ttu-id="6eea2-368">Необходимо запустить msvsmon.exe на удаленном сервере до отладки.</span><span class="sxs-lookup"><span data-stu-id="6eea2-368">You need to run msvsmon.exe on the remote server prior to debugging.</span></span> <span data-ttu-id="6eea2-369">Монитор удаленной отладки можно установить на компакт-диске Visual Studio или просто запустите msvsmon.exe из общей папки без установки на всех веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="6eea2-369">You can install the Remote Debug Monitor from the Visual Studio CD or you can simply run msvsmon.exe from a share without installing anything at all on the Web server.</span></span>

<span data-ttu-id="6eea2-370">При запуске msvsmon.exe, вполне вероятно, что он выдаст ошибку, о портах, блокируются для удаленной отладки.</span><span class="sxs-lookup"><span data-stu-id="6eea2-370">When you run msvsmon.exe, it is likely that it will complain about ports being blocked for remote debugging.</span></span> <span data-ttu-id="6eea2-371">К счастью можно легко разблокировать порты справа в диалоговом окне предупреждение, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="6eea2-371">Fortunately, you can easily unblock the ports from right within the warning dialog as shown below.</span></span>


![Уведомление о том, что брандмауэр Windows блокирует удаленную отладку](improvements-in-visual-studio-2005/_static/image9.jpg)

<span data-ttu-id="6eea2-373">**Рис. 12**: — уведомление, что брандмауэр Windows блокирует удаленную отладку</span><span class="sxs-lookup"><span data-stu-id="6eea2-373">**Figure 12**: Notification that Windows Firewall is Blocking Remote Debugging</span></span>


<span data-ttu-id="6eea2-374">После разблокировать порты, необходимые для отладки появится монитор удаленной отладки, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="6eea2-374">Once you have unblocked the ports necessary for debugging, you will see the Remote Debugging Monitor as shown below.</span></span> <span data-ttu-id="6eea2-375">Из этого интерфейса можно отслеживать подключения и изменить легко отладки разрешений.</span><span class="sxs-lookup"><span data-stu-id="6eea2-375">From this interface, you can monitor connections and change debugging permissions easily.</span></span>


![Монитор удаленной отладки](improvements-in-visual-studio-2005/_static/image10.jpg)

<span data-ttu-id="6eea2-377">**Рис. 13**: монитор удаленной отладки</span><span class="sxs-lookup"><span data-stu-id="6eea2-377">**Figure 13**: The Remote Debugging Monitor</span></span>


<span data-ttu-id="6eea2-378">Можно также удаленно отлаживать веб-приложения, открывается с помощью FTP.</span><span class="sxs-lookup"><span data-stu-id="6eea2-378">It is also possible to remotely debug a Web application opened via FTP.</span></span> <span data-ttu-id="6eea2-379">Действия являются такие же, как описанные ранее.</span><span class="sxs-lookup"><span data-stu-id="6eea2-379">The steps are the same as those previously covered.</span></span> <span data-ttu-id="6eea2-380">Тем не менее необходимо указать базовый URL-адрес для просмотра проектов FTP, как описано ранее в этом модуле.</span><span class="sxs-lookup"><span data-stu-id="6eea2-380">However, you will need to specify a base URL for browsing the FTP project as outlined earlier in this module.</span></span>

## <a name="lab-2"></a><span data-ttu-id="6eea2-381">В лаборатории 2</span><span class="sxs-lookup"><span data-stu-id="6eea2-381">Lab 2</span></span>

## <a name="remote-debugging-with-visual-studio-2005"></a><span data-ttu-id="6eea2-382">Удаленная отладка в Visual Studio 2005</span><span class="sxs-lookup"><span data-stu-id="6eea2-382">Remote Debugging with Visual Studio 2005</span></span>

<span data-ttu-id="6eea2-383">Эта лаборатория поможет вам выполнить удаленную отладку с помощью Visual Studio 2005.</span><span class="sxs-lookup"><span data-stu-id="6eea2-383">This lab will walk you through remote debugging with Visual Studio 2005.</span></span>

<span data-ttu-id="6eea2-384">Щелкните здесь для пошаговое видеоруководство для этого занятия.</span><span class="sxs-lookup"><span data-stu-id="6eea2-384">Click here for a video walkthrough of this lab.</span></span>


![](improvements-in-visual-studio-2005/_static/image5.png)


[<span data-ttu-id="6eea2-385">Откройте весь экран</span><span class="sxs-lookup"><span data-stu-id="6eea2-385">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/remdebug1.wmv)


<span data-ttu-id="6eea2-386">Этой лаборатории необходимо иметь две машины с одного выполнения Visual Studio 2005 и других запущенных служб IIS 5 или выше.</span><span class="sxs-lookup"><span data-stu-id="6eea2-386">This lab requires you to have two machines, one running Visual Studio 2005 and the other running IIS 5 or greater.</span></span>

1. <span data-ttu-id="6eea2-387">Откройте Visual Studio 2005 и создайте новый веб-сайт на удаленном сервере.</span><span class="sxs-lookup"><span data-stu-id="6eea2-387">Open Visual Studio 2005 and create a new Web site on the remote server.</span></span>

> [!NOTE]
> <span data-ttu-id="6eea2-388">Можно создать веб-сайта на удаленном экземпляре служб IIS или по протоколу FTP.</span><span class="sxs-lookup"><span data-stu-id="6eea2-388">You can create the Web site on a remote IIS instance or via FTP.</span></span>


1. <span data-ttu-id="6eea2-389">С удаленного веб-сервера найдите msvsmon.exe на компьютере разработчика, используя UNC-путь и его выполнение.</span><span class="sxs-lookup"><span data-stu-id="6eea2-389">From the remote Web server, locate msvsmon.exe on the development machine using a UNC path and execute it.</span></span>  
 <span data-ttu-id="6eea2-390">По умолчанию msvsmon.exe находится //server/c$/Program файлы или Microsoft Visual Studio 8/Common7/IDE/удаленного отладчика или x86.</span><span class="sxs-lookup"><span data-stu-id="6eea2-390">The default location for msvsmon.exe is //server/c$/Program Files/Microsoft Visual Studio 8/Common7/IDE/Remote Debugger/x86.</span></span>
2. <span data-ttu-id="6eea2-391">Если будет предложено открыть порты для удаленной отладки, сделайте это.</span><span class="sxs-lookup"><span data-stu-id="6eea2-391">If prompted to unblock ports for remote debugging, do so.</span></span>
3. <span data-ttu-id="6eea2-392">С компьютера разработки откройте выделенным кодом Default.aspx и установите точку останова в методе _подсистема веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="6eea2-392">From the development machine, open the code-behind for Default.aspx and set a breakpoint in the Page/_Load method.</span></span>
4. <span data-ttu-id="6eea2-393">Начните отладку с компьютера разработки.</span><span class="sxs-lookup"><span data-stu-id="6eea2-393">Start debugging from the development machine.</span></span>

<span data-ttu-id="6eea2-394">Следует попадет на точке останова, как ожидалось.</span><span class="sxs-lookup"><span data-stu-id="6eea2-394">You should hit the breakpoint as expected.</span></span>

## <a name="aspnet-development-server"></a><span data-ttu-id="6eea2-395">Сервер ASP.NET Development Server</span><span class="sxs-lookup"><span data-stu-id="6eea2-395">ASP.NET Development Server</span></span>

<span data-ttu-id="6eea2-396">Как уже описано weve Visual Studio 2005 поставляется с веб-сервер, называется ASP.NET Development Server.</span><span class="sxs-lookup"><span data-stu-id="6eea2-396">As weve already discussed, Visual Studio 2005 ships with a Web server called the ASP.NET Development Server.</span></span> <span data-ttu-id="6eea2-397">(ASP.NET Development Server иногда называют Cassini.) Этот веб-сервер является удобный способ для просмотра и отладки приложений, работающих в файловой системе.</span><span class="sxs-lookup"><span data-stu-id="6eea2-397">(The ASP.NET Development Server is sometimes referred to as Cassini.) This Web server is a convenient means to browse and debug Web applications running on the file system.</span></span>

<span data-ttu-id="6eea2-398">ASP.NET Development Server является ограниченным веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="6eea2-398">The ASP.NET Development Server is a restricted Web server.</span></span> <span data-ttu-id="6eea2-399">Запрещены удаленные соединения, он не допускает запросы из любого пользователя, отличного от пользователя, запустившего веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="6eea2-399">It does not allow remote connections, it does not allow any requests from any user other than the user who started the Web server.</span></span> <span data-ttu-id="6eea2-400">Он также имеет возможность предоставления ASP-страниц.</span><span class="sxs-lookup"><span data-stu-id="6eea2-400">It also does not have the capability of serving ASP pages.</span></span> <span data-ttu-id="6eea2-401">Обслуживаются только ASP.NET ресурсов и HTML (включая изображений, файлов CSS, т. д.).</span><span class="sxs-lookup"><span data-stu-id="6eea2-401">Only ASP.NET resources and HTML resources (including images, CSS files, etc.) are served.</span></span>

<span data-ttu-id="6eea2-402">ASP.NET Development Server может быть вызвано из командной строки, запустив файл WebDev.WebServer.exe, расположенный в папке c:/Windows/Microsoft.NET/Framework/v2.0./ */*  /  */*/\*.</span><span class="sxs-lookup"><span data-stu-id="6eea2-402">The ASP.NET Development Server can be launched via the command line by running the WebDev.WebServer.exe file located at c:/Windows/Microsoft.NET/Framework/v2.0./*/*/*/*/\*.</span></span> <span data-ttu-id="6eea2-403">Следующее диалоговое окно отображает параметры, которые доступны.</span><span class="sxs-lookup"><span data-stu-id="6eea2-403">The following dialog displays the parameters that are available.</span></span>


![](improvements-in-visual-studio-2005/_static/image11.jpg)

<span data-ttu-id="6eea2-404">**Рис. 14**</span><span class="sxs-lookup"><span data-stu-id="6eea2-404">**Figure 14**</span></span>


> [!NOTE]
> <span data-ttu-id="6eea2-405">ASP.NET Development Server не поддерживается при запуске явным образом через командную строку.</span><span class="sxs-lookup"><span data-stu-id="6eea2-405">The ASP.NET Development Server is not supported when launched explicitly via the command line.</span></span>
