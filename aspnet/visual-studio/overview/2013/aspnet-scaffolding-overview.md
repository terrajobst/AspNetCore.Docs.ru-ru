---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: Формирование шаблонов ASP.NET в Visual Studio 2013 | Документы Microsoft
author: tfitzmac
description: Формирование шаблонов ASP.NET — это новая функция, которая включается в Visual Studio 2013.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/09/2014
ms.topic: article
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: 425983c1ffff6369276f0723a9947a411a4617eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
ms.locfileid: "26506733"
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a><span data-ttu-id="e6265-103">Формирование шаблонов ASP.NET в Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="e6265-103">ASP.NET Scaffolding in Visual Studio 2013</span></span>
====================
<span data-ttu-id="e6265-104">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e6265-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="e6265-105">Формирование шаблонов ASP.NET — это новая функция, которая включается в Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="e6265-105">ASP.NET Scaffolding is a new feature that is included in Visual Studio 2013.</span></span>


## <a name="overview"></a><span data-ttu-id="e6265-106">Обзор</span><span class="sxs-lookup"><span data-stu-id="e6265-106">Overview</span></span>

<span data-ttu-id="e6265-107">Формирование шаблонов ASP.NET — это платформа создания кода для веб-приложений ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e6265-107">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="e6265-108">Visual Studio 2013 включает генераторы предустановленных кода для проектов MVC и веб-API.</span><span class="sxs-lookup"><span data-stu-id="e6265-108">Visual Studio 2013 includes pre-installed code generators for MVC and Web API projects.</span></span> <span data-ttu-id="e6265-109">Формирование шаблонов добавить в проект, если вы хотите быстро добавить код, который взаимодействует с моделями данных.</span><span class="sxs-lookup"><span data-stu-id="e6265-109">You add scaffolding to your project when you want to quickly add code that interacts with data models.</span></span> <span data-ttu-id="e6265-110">С помощью формирования шаблонов можно уменьшить время при разработке операций стандартные данные в своем проекте.</span><span class="sxs-lookup"><span data-stu-id="e6265-110">Using scaffolding can reduce the amount of time to develop standard data operations in your project.</span></span>

<span data-ttu-id="e6265-111">По умолчанию Visual Studio 2013 не поддерживает создание кода для проекта веб-форм, но можно использовать формирование шаблонов с веб-формы, Добавление зависимостей MVC в проект или Установка расширения.</span><span class="sxs-lookup"><span data-stu-id="e6265-111">By default, Visual Studio 2013 does not support generating code for a Web Forms project, but you can use scaffolding with Web Forms by either adding MVC dependencies to the project or installing an extension.</span></span> <span data-ttu-id="e6265-112">Ниже приведены оба подхода.</span><span class="sxs-lookup"><span data-stu-id="e6265-112">Both approaches are shown below.</span></span>

<span data-ttu-id="e6265-113">Visual Studio 2013 с обновлением 2 (в настоящее время версия-Кандидат) предоставляет возможность расширить формирование шаблонов ASP.NET для соответствия требованиям вашего сценария.</span><span class="sxs-lookup"><span data-stu-id="e6265-113">Visual Studio 2013 Update 2 (currently RC) provides the ability to extend ASP.NET Scaffolding to meet the requirements of your scenario.</span></span> <span data-ttu-id="e6265-114">Эта функциональная возможность можно создать шаблон настраиваемого формирование шаблонов и добавить его в диалоговом окне Добавить новый формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="e6265-114">With this functionality, you can create a customized scaffolding template and add it to Add New Scaffold dialog.</span></span> <span data-ttu-id="e6265-115">В пределах настроенного шаблона укажите код, который создается, когда Добавление элемента формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="e6265-115">Within the customized template, you specify the code that is generated when adding a scaffolded item.</span></span> <span data-ttu-id="e6265-116">Дополнительные сведения см. в разделе [Создание Custom Scaffolder для Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="e6265-116">For more information, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e6265-117">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="e6265-117">Prerequisites</span></span>

<span data-ttu-id="e6265-118">Чтобы использовать формирование шаблонов ASP.NET, необходимо иметь:</span><span class="sxs-lookup"><span data-stu-id="e6265-118">To use ASP.NET Scaffolding, you must have:</span></span>

- <span data-ttu-id="e6265-119">Microsoft Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="e6265-119">Microsoft Visual Studio 2013</span></span>
- <span data-ttu-id="e6265-120">Веб-инструменты разработчиков (часть установки Visual Studio 2013 по умолчанию)</span><span class="sxs-lookup"><span data-stu-id="e6265-120">Web Developer Tools (part of default Visual Studio 2013 installation)</span></span>
- <span data-ttu-id="e6265-121">Веб-платформы ASP.NET и инструменты 2013 (компонент установки Visual Studio 2013 по умолчанию)</span><span class="sxs-lookup"><span data-stu-id="e6265-121">ASP.NET Web Frameworks and Tools 2013 (part of default Visual Studio 2013 installation)</span></span>

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a><span data-ttu-id="e6265-122">Добавление элемента формирования шаблонов для MVC или веб-API</span><span class="sxs-lookup"><span data-stu-id="e6265-122">Add a scaffolded item to MVC or Web API</span></span>

<span data-ttu-id="e6265-123">Чтобы добавить элемент формирования шаблонов, щелкните правой кнопкой мыши проект или папку внутри проекта и выберите **добавить** — **новый элемент формирования шаблонов**, как показано на следующем рисунке.</span><span class="sxs-lookup"><span data-stu-id="e6265-123">To add a scaffold, right-click either the project or a folder within the project, and select **Add** – **New Scaffolded Item**, as shown in the following image.</span></span>

![Добавление элемента формирования шаблонов](aspnet-scaffolding-overview/_static/image1.png)

<span data-ttu-id="e6265-125">Из **Добавление формирования шаблонов** окно, выберите тип формирования шаблонов для добавления.</span><span class="sxs-lookup"><span data-stu-id="e6265-125">From the **Add Scaffold** window, select the type of scaffold to add.</span></span>

![Выберите тип для формирования шаблонов](aspnet-scaffolding-overview/_static/image2.png)

<span data-ttu-id="e6265-127">**Добавить контроллер** окно предоставляет возможность выбрать параметры для создания контроллера, включая, хотите ли вы использовать новые функции асинхронного из Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="e6265-127">The **Add Controller** window gives you the opportunity to select options for generating the controller, including whether you want to use the new async features from Entity Framework 6.</span></span>

![Добавление контроллера](aspnet-scaffolding-overview/_static/image3.png)

<span data-ttu-id="e6265-129">Соответствующими классами и страницы создаются для вашего сценария.</span><span class="sxs-lookup"><span data-stu-id="e6265-129">The relevant classes and pages are created for your scenario.</span></span> <span data-ttu-id="e6265-130">Например ниже приведен контроллер MVC и представления, созданным с помощью формирования шаблонов для модели класс с именем фильмов.</span><span class="sxs-lookup"><span data-stu-id="e6265-130">For example, the following image shows the MVC controller and views that were created through scaffolding for a model class named Movies.</span></span>

![Создаваемых файлов](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a><span data-ttu-id="e6265-132">Добавление элемента формирования шаблонов для веб-форм</span><span class="sxs-lookup"><span data-stu-id="e6265-132">Add a scaffolded item to Web Forms</span></span>

<span data-ttu-id="e6265-133">Чтобы добавить формирование шаблонов, который создает код веб-форм, необходимо установить расширение в Visual Studio или Добавление зависимостей MVC.</span><span class="sxs-lookup"><span data-stu-id="e6265-133">To add scaffolding that generates Web Forms code, you must either install an extension to Visual Studio or add MVC dependencies.</span></span> <span data-ttu-id="e6265-134">Ниже приведены оба подхода, но необходимо выполнить одно из следующих подходов.</span><span class="sxs-lookup"><span data-stu-id="e6265-134">Both approaches are shown below, but you only need to do one of these approaches.</span></span>

### <a name="web-forms-scaffolding-extension"></a><span data-ttu-id="e6265-135">Формирование шаблонов расширения веб-форм</span><span class="sxs-lookup"><span data-stu-id="e6265-135">Web Forms Scaffolding Extension</span></span>

<span data-ttu-id="e6265-136">Можно установить расширения Visual Studio, позволяют использовать формирование шаблонов с проектом веб-форм.</span><span class="sxs-lookup"><span data-stu-id="e6265-136">You can install a Visual Studio extension that enable you to use scaffolding with a Web Forms project.</span></span> <span data-ttu-id="e6265-137">В Visual Studio, выберите **средства** и затем **расширения и обновления**.</span><span class="sxs-lookup"><span data-stu-id="e6265-137">In Visual Studio, select **Tools** and then **Extensions and Updates**.</span></span> <span data-ttu-id="e6265-138">В этом диалоговом окне поиска в коллекции Visual Studio для **формирование шаблонов форм**.</span><span class="sxs-lookup"><span data-stu-id="e6265-138">From this dialog search the Visual Studio Gallery for **Web Forms Scaffolding**.</span></span>

![Установка веб-форм формирование шаблонов](aspnet-scaffolding-overview/_static/image5.png)

<span data-ttu-id="e6265-140">Дополнительные сведения см. в разделе [формирование шаблонов форм](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span><span class="sxs-lookup"><span data-stu-id="e6265-140">For more information, see [Web Forms Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span></span>

### <a name="mvc-dependencies"></a><span data-ttu-id="e6265-141">Зависимостей MVC</span><span class="sxs-lookup"><span data-stu-id="e6265-141">MVC Dependencies</span></span>

<span data-ttu-id="e6265-142">Добавление зависимостей MVC, выберите **добавить** - **новый элемент формирования шаблонов**.</span><span class="sxs-lookup"><span data-stu-id="e6265-142">To add MVC dependencies, select **Add** - **New Scaffolded Item**.</span></span> <span data-ttu-id="e6265-143">В окне Добавление формирования шаблонов выберите **зависимостей MVC**, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="e6265-143">In the Add Scaffold window, select **MVC Dependencies**, as shown below.</span></span>

![Добавление зависимостей MVC](aspnet-scaffolding-overview/_static/image6.png)

<span data-ttu-id="e6265-145">Существует два варианта для формирования шаблонов MVC; Минимальными и полное.</span><span class="sxs-lookup"><span data-stu-id="e6265-145">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="e6265-146">При выборе минимальной только пакеты NuGet и ссылки для ASP.NET MVC добавляются в проект.</span><span class="sxs-lookup"><span data-stu-id="e6265-146">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="e6265-147">При выборе параметра «полная» добавляются минимальные зависимости, а также необходимые файлы содержимого проекта MVC.</span><span class="sxs-lookup"><span data-stu-id="e6265-147">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span> <span data-ttu-id="e6265-148">Чтобы легко использовать формирование шаблонов, выберите полную зависимости.</span><span class="sxs-lookup"><span data-stu-id="e6265-148">To easily use scaffolding, select Full dependencies.</span></span>

![Выберите полный зависимости](aspnet-scaffolding-overview/_static/image7.png)

<span data-ttu-id="e6265-150">После добавления зависимости, вы увидите **readme.txt** файла.</span><span class="sxs-lookup"><span data-stu-id="e6265-150">After adding the dependencies, you will see a **readme.txt** file.</span></span> <span data-ttu-id="e6265-151">Внимательно следуйте инструкциям в этот файл, чтобы убедиться, что проект работает правильно.</span><span class="sxs-lookup"><span data-stu-id="e6265-151">Carefully follow the instructions in this file to ensure that your project works correctly.</span></span>

<span data-ttu-id="e6265-152">После завершения действия в файле readme.txt, можно добавить новый элемент формирования шаблонов, как показано в предыдущем разделе о MVC и веб-API.</span><span class="sxs-lookup"><span data-stu-id="e6265-152">When you have completed the steps in the readme.txt file, you can add a new scaffolded item as shown in the previous section about MVC and Web API.</span></span> <span data-ttu-id="e6265-153">В рамках проекта автоматически создается представления и контроллера будет работать правильно.</span><span class="sxs-lookup"><span data-stu-id="e6265-153">The automatically-generated views and controller will function correctly within your project.</span></span>

## <a name="tutorials"></a><span data-ttu-id="e6265-154">Учебники</span><span class="sxs-lookup"><span data-stu-id="e6265-154">Tutorials</span></span>

<span data-ttu-id="e6265-155">Создание настраиваемого scaffolder — [Создание Custom Scaffolder для Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="e6265-155">To create a customized scaffolder, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

<span data-ttu-id="e6265-156">Созданные файлы, в разделе [Настройка созданные файлы из диалогового окна элемент формирования шаблонов](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span><span class="sxs-lookup"><span data-stu-id="e6265-156">To customize the generated files, see [How to customize the generated files from the New Scaffolded Item dialog](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span></span>

<span data-ttu-id="e6265-157">Пример с помощью формирования шаблонов с **Database First разработки**, в разделе [EF Database First с ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span><span class="sxs-lookup"><span data-stu-id="e6265-157">For an example of using scaffolding with **Database First development**, see [EF Database First with ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span></span>

<span data-ttu-id="e6265-158">Пример с помощью формирования шаблонов в **MVC** проекта см. в разделе [Приступая к работе с ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="e6265-158">For an example of using scaffolding in an **MVC** project, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<span data-ttu-id="e6265-159">Пример с помощью формирования шаблонов в **веб-API** проекта см. в разделе [создать REST API с маршрутизацией атрибутов в веб-API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="e6265-159">For an example of using scaffolding in a **Web API** project, see [Create a REST API with Attribute Routing in Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span></span>
