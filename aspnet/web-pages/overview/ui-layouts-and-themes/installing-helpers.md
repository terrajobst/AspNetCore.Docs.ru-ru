---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Установка вспомогательного класса в ASP.NET Web Pages сайта (Razor) | Документы Microsoft
author: tfitzmac
description: В этой статье описывается установка модуля поддержки на веб-сайте ASP.NET Web Pages (Razor). Вспомогательный объект является компонентом для повторного использования, который включает код и разметку для каждого...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 766fbb87ae8bcb8917eb8fa7f552c00792242cf6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="0613e-104">Установка вспомогательного класса в веб-страницы (Razor) узла ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0613e-104">Installing a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="0613e-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="0613e-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="0613e-106">В этой статье описывается установка модуля поддержки на веб-сайте ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="0613e-106">This article describes how to install a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="0613e-107">Объект *вспомогательный* повторно используемый компонент, который включает код и разметку, чтобы выполнить задачу, которая может оказаться трудоемкой и сложной.</span><span class="sxs-lookup"><span data-stu-id="0613e-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="0613e-108">Что вы узнаете следующее.</span><span class="sxs-lookup"><span data-stu-id="0613e-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="0613e-109">Как установить помощник в веб-сайт, созданный с помощью WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="0613e-109">How to install a helper in a website created using WebMatrix 3.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="0613e-110">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="0613e-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="0613e-111">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="0613e-111">WebMatrix 3</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="0613e-112">Общие сведения о вспомогательных методов</span><span class="sxs-lookup"><span data-stu-id="0613e-112">Overview of Helpers</span></span>

<span data-ttu-id="0613e-113">Некоторые задачи, которые пользователи часто необходимо сделать на веб-страницах требуют большого объема кода или дополнительных знаний.</span><span class="sxs-lookup"><span data-stu-id="0613e-113">Some tasks that people often want to do on web pages require a lot of code or require extra knowledge.</span></span> <span data-ttu-id="0613e-114">Примеры включают отображать диаграмму для данных; Размещение кнопки Twitter «Следуют» на странице; При отправке сообщения из веб-сайта; Обрезание или изменение размеров изображений; для веб-узла с помощью PayPal.</span><span class="sxs-lookup"><span data-stu-id="0613e-114">Examples include displaying a chart for data; putting a Twitter "Follow" button on a page; sending email from your website; cropping or resizing images; using PayPal for your site.</span></span> <span data-ttu-id="0613e-115">Чтобы облегчить выполните этих видов веб-страниц ASP.NET позволяет использовать *вспомогательные методы*.</span><span class="sxs-lookup"><span data-stu-id="0613e-115">To make it easy to do these kinds of things, ASP.NET Web Pages lets you use *helpers*.</span></span> <span data-ttu-id="0613e-116">Вспомогательные объекты — это компоненты, установить для сайта и которые позволяют вам выполнять типовые задачи, используя только одну-две строки кода Razor.</span><span class="sxs-lookup"><span data-stu-id="0613e-116">Helpers are components that you install for a site and that let you perform typical tasks by using just a line or two of Razor code.</span></span>

<span data-ttu-id="0613e-117">Веб-страницы ASP.NET имеет несколько вспомогательных методов, встроенных в.</span><span class="sxs-lookup"><span data-stu-id="0613e-117">ASP.NET Web Pages has a few helpers built in.</span></span> <span data-ttu-id="0613e-118">Тем не менее многие вспомогательные методы доступны в пакетах (надстройки), которые предоставляются с помощью диспетчера пакетов NuGet.</span><span class="sxs-lookup"><span data-stu-id="0613e-118">However, many helpers are available in packages (add-ins) that are provided using the NuGet package manager.</span></span> <span data-ttu-id="0613e-119">NuGet позволяет выбрать пакет установки, а затем он берет на себя все сведения об установке.</span><span class="sxs-lookup"><span data-stu-id="0613e-119">NuGet lets you select a package to install and then it takes care of all the details of the installation.</span></span>

## <a name="installing-a-helper-in-webmatrix-3"></a><span data-ttu-id="0613e-120">Установка модуля поддержки в WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="0613e-120">Installing a Helper in WebMatrix 3</span></span>

1. <span data-ttu-id="0613e-121">В WebMatrix 3 выберите **NuGet** кнопки.</span><span class="sxs-lookup"><span data-stu-id="0613e-121">In WebMatrix 3, click the **NuGet** button.</span></span>

    ![Диалоговое окно галереи NuGet в WebMatrix](installing-helpers/_static/image1.png)
2. <span data-ttu-id="0613e-123">При этом будет запущен диспетчер пакетов NuGet и отображаются доступные пакеты.</span><span class="sxs-lookup"><span data-stu-id="0613e-123">This launches the NuGet package manager and displays available packages.</span></span> <span data-ttu-id="0613e-124">В поле поиска введите ключевое слово для вспомогательного метода, который вы хотите установить.</span><span class="sxs-lookup"><span data-stu-id="0613e-124">In the search box, enter a keyword for the helper you want to install.</span></span>

    ![Диалоговое окно галереи NuGet в WebMatrix](installing-helpers/_static/image2.png)
3. <span data-ttu-id="0613e-126">Выберите пакет и нажмите кнопку **установить**.</span><span class="sxs-lookup"><span data-stu-id="0613e-126">Select the package and then click **Install**.</span></span> <span data-ttu-id="0613e-127">Нажмите кнопку **Да** при появлении запроса, если вы хотите установить пакет и указать, что вы принимаете условия.</span><span class="sxs-lookup"><span data-stu-id="0613e-127">Click **Yes** when asked if you want to install the package and indicate that you accept the terms.</span></span>

     <span data-ttu-id="0613e-128">Если это первый раз, после установки модуля поддержки NuGet создает папки в веб-сайт для кода, который представляет вспомогательное приложение.</span><span class="sxs-lookup"><span data-stu-id="0613e-128">If this is the first time you've installed a helper, NuGet creates folders in your website for the code that makes up the helper.</span></span>
4. <span data-ttu-id="0613e-129">Чтобы удалить вспомогательный класс, нажмите кнопку **коллекции** , выберите элемент **установленные** вкладку и выберите пакет, который требуется удалить.</span><span class="sxs-lookup"><span data-stu-id="0613e-129">To uninstall a helper, click the **Gallery** button, click the **Installed** tab, and pick the package you want to uninstall.</span></span>

## <a name="installing-the-twitter-helper"></a><span data-ttu-id="0613e-130">Установка поддержки Twitter</span><span class="sxs-lookup"><span data-stu-id="0613e-130">Installing the Twitter helper</span></span>

<span data-ttu-id="0613e-131">Последнюю версию Twitter API несовместим со вспомогательным методом Twitter устанавливаемого через NuGet.</span><span class="sxs-lookup"><span data-stu-id="0613e-131">The latest version of the Twitter API is not compatible with the Twitter helper you install through NuGet.</span></span> <span data-ttu-id="0613e-132">Вместо этого в разделе [вспомогательный модуль Twitter с WebMatrix](twitter-helper.md) сведения о настройке поддержки Twitter в своем проекте.</span><span class="sxs-lookup"><span data-stu-id="0613e-132">Instead, see the [Twitter Helper with WebMatrix](twitter-helper.md) topic for information about how to set up the Twitter helper in your project.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="0613e-133">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="0613e-133">Additional Resources</span></span>


[<span data-ttu-id="0613e-134">Знакомство с приложением ASP.NET Web Pages 2 — основы программирования</span><span class="sxs-lookup"><span data-stu-id="0613e-134">Introducing ASP.NET Web Pages 2 - Programming Basics</span></span>](../getting-started/introducing-razor-syntax-c.md)

[<span data-ttu-id="0613e-135">Вспомогательный модуль в WebMatrix Twitter</span><span class="sxs-lookup"><span data-stu-id="0613e-135">Twitter Helper with WebMatrix</span></span>](twitter-helper.md)
