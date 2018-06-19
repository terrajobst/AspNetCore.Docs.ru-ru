---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: Обновление приложения ASP.NET MVC 1.0 для ASP.NET MVC 2 | Документы Microsoft
author: rick-anderson
description: В этом документе описываются оба способа обновления и вручную с помощью мастера приложения ASP.NET MVC 1.0 для ASP.NET MVC 2. В этом документе также доступна для d...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/08/2010
ms.topic: article
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: d1227f0738b2d2a396fed942f32ae5e75579596e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530063"
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a><span data-ttu-id="f743d-104">Обновление приложения ASP.NET MVC 1.0 для ASP.NET MVC 2</span><span class="sxs-lookup"><span data-stu-id="f743d-104">Upgrading an ASP.NET MVC 1.0 Application to ASP.NET MVC 2</span></span>
====================
> <span data-ttu-id="f743d-105">В этом документе описываются оба способа обновления и вручную с помощью мастера приложения ASP.NET MVC 1.0 для ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="f743d-105">This document describes both how to upgrade manually and with a wizard an ASP.NET MVC 1.0 Application to ASP.NET MVC 2.</span></span> <span data-ttu-id="f743d-106">В этом документе также доступен для [загрузки](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span><span class="sxs-lookup"><span data-stu-id="f743d-106">This document is also available for [Download](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span></span>


## <a name="introduction"></a><span data-ttu-id="f743d-107">Вступление</span><span class="sxs-lookup"><span data-stu-id="f743d-107">Introduction</span></span>

<span data-ttu-id="f743d-108">ASP.NET MVC 2 можно установить параллельно с ASP.NET MVC 1.0 на том же сервере.</span><span class="sxs-lookup"><span data-stu-id="f743d-108">ASP.NET MVC 2 can be installed side by side with ASP.NET MVC 1.0 on the same server.</span></span> <span data-ttu-id="f743d-109">Это предоставляет разработчикам гибкость в выборе при обновлении приложения ASP.NET MVC 1.0 для ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="f743d-109">This gives application developers flexibility in choosing when to upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2.</span></span>

<span data-ttu-id="f743d-110">Visual Studio 2010 содержит мастер, обновления существующих проектов ASP.NET MVC 1.0, созданные с помощью Visual Studio 2008 в ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="f743d-110">Visual Studio 2010 includes a wizard that upgrades existing ASP.NET MVC 1.0 projects built with Visual Studio 2008 to ASP.NET MVC 2.</span></span> <span data-ttu-id="f743d-111">Мастер обновления инициируется Открытие проекта ASP.NET MVC 1.0 в Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="f743d-111">The upgrade wizard is initiated by opening an ASP.NET MVC 1.0 project in Visual Studio 2010.</span></span>

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a><span data-ttu-id="f743d-112">Мастер для ASP.NET MVC 1.0 для Visual Studio 2008 SP1 обновления</span><span class="sxs-lookup"><span data-stu-id="f743d-112">Upgrade Wizard for ASP.NET MVC 1.0 on Visual Studio 2008 SP1</span></span>

<span data-ttu-id="f743d-113">Чтобы обновить приложение ASP.NET MVC 1.0 для ASP.NET MVC 2 в Visual Studio 2008 SP1, используйте (неподдерживаемые) MvcAppConverter приложение.</span><span class="sxs-lookup"><span data-stu-id="f743d-113">To upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2 in Visual Studio 2008 SP1, use the (unsupported) MvcAppConverter application.</span></span> <span data-ttu-id="f743d-114">Это приложение можно загрузить со следующего URL:</span><span class="sxs-lookup"><span data-stu-id="f743d-114">You can download this application from the following URL:</span></span>

[<span data-ttu-id="f743d-115">https://go.Microsoft.com/fwlink/?LinkId=185351</span><span class="sxs-lookup"><span data-stu-id="f743d-115">https://go.microsoft.com/fwlink/?LinkID=185351</span></span>](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a><span data-ttu-id="f743d-116">Вручную обновление проекта ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="f743d-116">Manually Upgrading an ASP.NET MVC 1.0 Project</span></span>

<span data-ttu-id="f743d-117">Чтобы вручную обновить существующее приложение ASP.NET MVC 1.0 до версии 2, выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="f743d-117">To manually upgrade an existing ASP.NET MVC 1.0 application to version 2, follow these steps:</span></span>

1. <span data-ttu-id="f743d-118">Создайте резервную копию существующего проекта.</span><span class="sxs-lookup"><span data-stu-id="f743d-118">Make a backup of the existing project.</span></span>
2. <span data-ttu-id="f743d-119">В текстовом редакторе откройте файл проекта (файл с расширением .csproj или .vbproj) и найдите элемент ProjectTypeGuid.</span><span class="sxs-lookup"><span data-stu-id="f743d-119">In a text editor, open the project file (the file with the .csproj or .vbproj file extension) and find the ProjectTypeGuid element.</span></span> <span data-ttu-id="f743d-120">Значением этого элемента замените GUID {603c0e0b-db56-11dc-be95-000d561079b0} с {F85E285D-A4E0-4152-9332-AB1D724D3325}.</span><span class="sxs-lookup"><span data-stu-id="f743d-120">As the value of that element, replace the GUID {603c0e0b-db56-11dc-be95-000d561079b0} with {F85E285D-A4E0-4152-9332-AB1D724D3325}.</span></span> <span data-ttu-id="f743d-121">Когда вы закончите, значение этого элемента должен быть следующим:</span><span class="sxs-lookup"><span data-stu-id="f743d-121">When you are done, the value of that element should be as follows:</span></span> 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. <span data-ttu-id="f743d-122">В корневой папке веб-приложений измените файл Web.config.</span><span class="sxs-lookup"><span data-stu-id="f743d-122">In the Web application root folder, edit the Web.config file.</span></span> <span data-ttu-id="f743d-123">Поиск System.Web.Mvc, версия = 1.0.0.0 и замените все экземпляры System.Web.Mvc, версия = 2.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="f743d-123">Search for System.Web.Mvc, Version=1.0.0.0 and replace all instances with System.Web.Mvc, Version=2.0.0.0.</span></span>
4. <span data-ttu-id="f743d-124">Повторите предыдущий шаг для файла Web.config, расположенный в папке «представления».</span><span class="sxs-lookup"><span data-stu-id="f743d-124">Repeat the previous step for the Web.config file located in the Views folder.</span></span>
5. <span data-ttu-id="f743d-125">Откройте проект с помощью Visual Studio и в **обозревателе решений**, разверните **ссылки** узла.</span><span class="sxs-lookup"><span data-stu-id="f743d-125">Open the project using Visual Studio, and in **Solution Explorer**, expand the **References** node.</span></span> <span data-ttu-id="f743d-126">Удалите ссылку на System.Web.Mvc (которая указывает на сборку версии 1.0).</span><span class="sxs-lookup"><span data-stu-id="f743d-126">Delete the reference to System.Web.Mvc (which points to the version 1.0 assembly).</span></span> <span data-ttu-id="f743d-127">Добавьте ссылку на System.Web.Mvc (v2.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="f743d-127">Add a reference to System.Web.Mvc (v2.0.0.0).</span></span>
6. <span data-ttu-id="f743d-128">Добавьте следующий элемент bindingRedirect в файл Web.config в корневой папке приложения в разделе конфигурации:</span><span class="sxs-lookup"><span data-stu-id="f743d-128">Add the following bindingRedirect element to the Web.config file in the application root under the configuraton section:</span></span>   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. <span data-ttu-id="f743d-129">Создайте новое пустое приложение ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="f743d-129">Create a new empty ASP.NET MVC 2 application.</span></span> <span data-ttu-id="f743d-130">Скопируйте файлы из папки сценариев нового приложения в папку «Скрипты» существующего приложения.</span><span class="sxs-lookup"><span data-stu-id="f743d-130">Copy the files from the Scripts folder of the new application into the Scripts folder of the existing application.</span></span>
8. <span data-ttu-id="f743d-131">Обновление существующего applicationâ€™ s CSS-файл в файле Site.css определений стилей CSS.</span><span class="sxs-lookup"><span data-stu-id="f743d-131">Update the existing applicationâ€™s CSS file with the CSS style definitions in the Site.css file.</span></span>
9. <span data-ttu-id="f743d-132">Скомпилируйте приложение и запустите его.</span><span class="sxs-lookup"><span data-stu-id="f743d-132">Compile the application and run it.</span></span> <span data-ttu-id="f743d-133">Если возникнут ошибки, см. раздел критические изменения [новые возможности в ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) страницы.</span><span class="sxs-lookup"><span data-stu-id="f743d-133">If any errors occur, refer to the Breaking Changes section of the [What's New in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) page.</span></span>
