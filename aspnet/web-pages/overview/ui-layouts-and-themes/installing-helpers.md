---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Установка вспомогательного метода в веб-ASP.NET страниц узла (Razor) | Документация Майкрософт
author: tfitzmac
description: В этой статье описывается установка вспомогательного метода в на веб-сайте ASP.NET Web Pages (Razor). Помощник представляет собой многократно используемый компонент включает код и разметку для каждого...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 38290fd47355e7893eddd1f867f47b113b54ca7e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37361810"
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="61230-104">Установка вспомогательного метода на сайте ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="61230-104">Installing a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="61230-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="61230-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="61230-106">В этой статье описывается установка вспомогательного метода в на веб-сайте ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="61230-106">This article describes how to install a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="61230-107">Объект *вспомогательный* является многоразовым компонентом, который включает код и разметку, чтобы выполнить задачу, которая может быть утомительным или сложной.</span><span class="sxs-lookup"><span data-stu-id="61230-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="61230-108">Вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="61230-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="61230-109">Как установить помощник на веб-сайт, созданный с помощью WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="61230-109">How to install a helper in a website created using WebMatrix 3.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="61230-110">Версии программного обеспечения, используемые в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="61230-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="61230-111">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="61230-111">WebMatrix 3</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="61230-112">Общие сведения о вспомогательных функций</span><span class="sxs-lookup"><span data-stu-id="61230-112">Overview of Helpers</span></span>

<span data-ttu-id="61230-113">Некоторые задачи, которые пользователи часто стремятся сделать на веб-страницах, требуют большого объема кода или требует дополнительных знаний.</span><span class="sxs-lookup"><span data-stu-id="61230-113">Some tasks that people often want to do on web pages require a lot of code or require extra knowledge.</span></span> <span data-ttu-id="61230-114">Примеры включают отображение диаграммы для данных; Размещение кнопки «Отслеживать» Twitter на странице; При отправке сообщения с веб-сайта; Обрезание или изменение размеров изображений; с помощью PayPal для вашего сайта.</span><span class="sxs-lookup"><span data-stu-id="61230-114">Examples include displaying a chart for data; putting a Twitter "Follow" button on a page; sending email from your website; cropping or resizing images; using PayPal for your site.</span></span> <span data-ttu-id="61230-115">Чтобы упростить процесс для выполнения таких задач, веб-страниц ASP.NET позволяет использовать *вспомогательные функции*.</span><span class="sxs-lookup"><span data-stu-id="61230-115">To make it easy to do these kinds of things, ASP.NET Web Pages lets you use *helpers*.</span></span> <span data-ttu-id="61230-116">Вспомогательные функции, что устанавливаемые компоненты для сайта и которые позволяют вам выполнять типовые задачи, используя лишь одну-две строки кода Razor.</span><span class="sxs-lookup"><span data-stu-id="61230-116">Helpers are components that you install for a site and that let you perform typical tasks by using just a line or two of Razor code.</span></span>

<span data-ttu-id="61230-117">Веб-страницы ASP.NET имеет несколько вспомогательных функций, встроенных.</span><span class="sxs-lookup"><span data-stu-id="61230-117">ASP.NET Web Pages has a few helpers built in.</span></span> <span data-ttu-id="61230-118">Тем не менее многие вспомогательные функции доступны в пакетах (надстройки), которые предоставляются с помощью диспетчера пакетов NuGet.</span><span class="sxs-lookup"><span data-stu-id="61230-118">However, many helpers are available in packages (add-ins) that are provided using the NuGet package manager.</span></span> <span data-ttu-id="61230-119">NuGet позволяет выбрать пакет установки, а затем она отвечает за все аспекты установки.</span><span class="sxs-lookup"><span data-stu-id="61230-119">NuGet lets you select a package to install and then it takes care of all the details of the installation.</span></span>

## <a name="installing-a-helper-in-webmatrix-3"></a><span data-ttu-id="61230-120">Установка вспомогательного метода в WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="61230-120">Installing a Helper in WebMatrix 3</span></span>

1. <span data-ttu-id="61230-121">В WebMatrix 3 щелкните **NuGet** кнопки.</span><span class="sxs-lookup"><span data-stu-id="61230-121">In WebMatrix 3, click the **NuGet** button.</span></span>

    ![Коллекция NuGet диалоговое окно, в WebMatrix](installing-helpers/_static/image1.png)
2. <span data-ttu-id="61230-123">Это запускает диспетчер пакетов NuGet и отображает доступные пакеты.</span><span class="sxs-lookup"><span data-stu-id="61230-123">This launches the NuGet package manager and displays available packages.</span></span> <span data-ttu-id="61230-124">В поле поиска введите ключевое слово для вспомогательного метода, который вы хотите установить.</span><span class="sxs-lookup"><span data-stu-id="61230-124">In the search box, enter a keyword for the helper you want to install.</span></span>

    ![Коллекция NuGet диалоговое окно, в WebMatrix](installing-helpers/_static/image2.png)
3. <span data-ttu-id="61230-126">Выберите пакет и нажмите кнопку **установить**.</span><span class="sxs-lookup"><span data-stu-id="61230-126">Select the package and then click **Install**.</span></span> <span data-ttu-id="61230-127">Нажмите кнопку **Да** при запросе, если вы хотите установить пакет и указывают, что вы принимаете условия.</span><span class="sxs-lookup"><span data-stu-id="61230-127">Click **Yes** when asked if you want to install the package and indicate that you accept the terms.</span></span>

     <span data-ttu-id="61230-128">Если это первый раз, вы установили вспомогательный объект, NuGet создает папки в веб-сайт для кода, составляющего вспомогательное приложение.</span><span class="sxs-lookup"><span data-stu-id="61230-128">If this is the first time you've installed a helper, NuGet creates folders in your website for the code that makes up the helper.</span></span>
4. <span data-ttu-id="61230-129">Чтобы удалить вспомогательный объект, нажмите кнопку **коллекции** , выберите элемент **установленные** вкладку и выберите пакет, который требуется удалить.</span><span class="sxs-lookup"><span data-stu-id="61230-129">To uninstall a helper, click the **Gallery** button, click the **Installed** tab, and pick the package you want to uninstall.</span></span>

## <a name="installing-the-twitter-helper"></a><span data-ttu-id="61230-130">Установка вспомогательного приложения Twitter</span><span class="sxs-lookup"><span data-stu-id="61230-130">Installing the Twitter helper</span></span>

<span data-ttu-id="61230-131">Последнюю версию Twitter API несовместим со вспомогательной функцией Twitter, устанавливаемым через NuGet.</span><span class="sxs-lookup"><span data-stu-id="61230-131">The latest version of the Twitter API is not compatible with the Twitter helper you install through NuGet.</span></span> <span data-ttu-id="61230-132">Вместо этого см. в разделе [вспомогательный модуль Twitter с помощью WebMatrix](twitter-helper.md) сведения о настройке вспомогательного приложения Twitter в проекте.</span><span class="sxs-lookup"><span data-stu-id="61230-132">Instead, see the [Twitter Helper with WebMatrix](twitter-helper.md) topic for information about how to set up the Twitter helper in your project.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="61230-133">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="61230-133">Additional Resources</span></span>


[<span data-ttu-id="61230-134">Введение в ASP.NET Web Pages 2 — основы программирования</span><span class="sxs-lookup"><span data-stu-id="61230-134">Introducing ASP.NET Web Pages 2 - Programming Basics</span></span>](../getting-started/introducing-razor-syntax-c.md)

[<span data-ttu-id="61230-135">Вспомогательное приложение Twitter с помощью WebMatrix</span><span class="sxs-lookup"><span data-stu-id="61230-135">Twitter Helper with WebMatrix</span></span>](twitter-helper.md)
