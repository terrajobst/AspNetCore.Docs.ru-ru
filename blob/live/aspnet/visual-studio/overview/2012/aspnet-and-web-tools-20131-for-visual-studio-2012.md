---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: "Заметки о выпуске для ASP.NET и веб-инструменты 2013.1 для Visual Studio 2012 | Документы Microsoft"
author: microsoft
description: "Этот документ описывает выпуска ASP.NET и Web Tools 2013.1 для Visual Studio 2012."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2013
ms.topic: article
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 1e4ee8eb4901305bf6a8c9c5b949dc4ee10290e5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="c987b-103">Заметки о выпуске для ASP.NET и веб-инструменты 2013.1 для Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="c987b-103">Release Notes for ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>
====================
<span data-ttu-id="c987b-104">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c987b-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="c987b-105">Этот документ описывает выпуска ASP.NET и Web Tools 2013.1 для Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="c987b-105">This document describes the release of ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>


## <a name="contents"></a><span data-ttu-id="c987b-106">Описание</span><span class="sxs-lookup"><span data-stu-id="c987b-106">Contents</span></span>

- [<span data-ttu-id="c987b-107">Замечания по установке</span><span class="sxs-lookup"><span data-stu-id="c987b-107">Installation Notes</span></span>](#install)
- [<span data-ttu-id="c987b-108">Требования к программному обеспечению</span><span class="sxs-lookup"><span data-stu-id="c987b-108">Software Requirements</span></span>](#requirements)
- <span data-ttu-id="c987b-109">Новые возможности в ASP.NET и веб-инструменты 2013.1 для Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="c987b-109">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

    - [<span data-ttu-id="c987b-110">Начальной загрузки</span><span class="sxs-lookup"><span data-stu-id="c987b-110">Bootstrap</span></span>](#bootstrap)
    - [<span data-ttu-id="c987b-111">Шаблоны</span><span class="sxs-lookup"><span data-stu-id="c987b-111">Templates</span></span>](#templates)

        - [<span data-ttu-id="c987b-112">Шаблон ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="c987b-112">ASP.NET MVC 5 template</span></span>](#mvc5template)
        - [<span data-ttu-id="c987b-113">Шаблон ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="c987b-113">ASP.NET Web API 2 template</span></span>](#apitemplate)
        - [<span data-ttu-id="c987b-114">Шаблоны элементов</span><span class="sxs-lookup"><span data-stu-id="c987b-114">Item Templates</span></span>](#itemtemplate)
    - [<span data-ttu-id="c987b-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="c987b-115">Entity Framework 6</span></span>](#ef6)
    - [<span data-ttu-id="c987b-116">Формирование шаблонов ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c987b-116">ASP.NET Scaffolding</span></span>](#scaffold)
    - [<span data-ttu-id="c987b-117">Редактор Razor</span><span class="sxs-lookup"><span data-stu-id="c987b-117">Razor Editor</span></span>](#razor)
    - [<span data-ttu-id="c987b-118">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="c987b-118">NuGet 2.7</span></span>](#nuget)
- <span data-ttu-id="c987b-119">Известные проблемы и критические изменения</span><span class="sxs-lookup"><span data-stu-id="c987b-119">Known Issues and Breaking Changes</span></span>

    - [<span data-ttu-id="c987b-120">Формирование шаблонов ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c987b-120">ASP.NET Scaffolding</span></span>](#issuescaffolding)

        - [<span data-ttu-id="c987b-121">MVC и API формирование шаблонов-HTTP 404 не найдено ошибок</span><span class="sxs-lookup"><span data-stu-id="c987b-121">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>](#404issue)
        - [<span data-ttu-id="c987b-122">Visual Studio Express 2012 для Web перестает работать после добавления элемента формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="c987b-122">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>](#expressissue)
    - [<span data-ttu-id="c987b-123">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="c987b-123">ASP.NET Razor 3</span></span>](#issuerazor)

        - [<span data-ttu-id="c987b-124">Просмотр файл cshtml просмотр с помощью или F5 приводит к возникновению ошибки сервера</span><span class="sxs-lookup"><span data-stu-id="c987b-124">Viewing cshtml file with Browse With or F5 causes a server error</span></span>](#browseissue)
        - [<span data-ttu-id="c987b-125">Перезапись URL-адреса и Tilde(~)</span><span class="sxs-lookup"><span data-stu-id="c987b-125">Url Rewrite and Tilde(~)</span></span>](#rewriteissue)
    - [<span data-ttu-id="c987b-126">Шаблоны</span><span class="sxs-lookup"><span data-stu-id="c987b-126">Templates</span></span>](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a><span data-ttu-id="c987b-127">Замечания по установке</span><span class="sxs-lookup"><span data-stu-id="c987b-127">Installation Notes</span></span>

<span data-ttu-id="c987b-128">[Установка](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET и веб-средств 2013.1 для Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="c987b-128">[Install](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="requirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="c987b-129">Требования к программному обеспечению</span><span class="sxs-lookup"><span data-stu-id="c987b-129">Software Requirements</span></span>

<span data-ttu-id="c987b-130">Необходимо иметь Visual Studio 2012 или Visual Studio Express 2012 для Web.</span><span class="sxs-lookup"><span data-stu-id="c987b-130">You must have either Visual Studio 2012 or Visual Studio Express 2012 for Web.</span></span>

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="c987b-131">Новые возможности в ASP.NET и веб-инструменты 2013.1 для Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="c987b-131">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

<a id="bootstrap"></a>
### <a name="bootstrap"></a><span data-ttu-id="c987b-132">начальной загрузки</span><span class="sxs-lookup"><span data-stu-id="c987b-132">Bootstrap</span></span>

<span data-ttu-id="c987b-133">При формировании на основе скаффолдинга контроллеров MVC 5 и представлений, использует разметка для представления [начальной загрузки](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="c987b-133">When you scaffold MVC 5 controllers and views, the markup for the views uses [Bootstrap](http://getbootstrap.com/).</span></span>

<a id="templates"></a>
### <a name="templates"></a><span data-ttu-id="c987b-134">Шаблоны</span><span class="sxs-lookup"><span data-stu-id="c987b-134">Templates</span></span>

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a><span data-ttu-id="c987b-135">Шаблон ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="c987b-135">ASP.NET MVC 5 template</span></span>

<span data-ttu-id="c987b-136">Мы добавили новый шаблон MVC 5.</span><span class="sxs-lookup"><span data-stu-id="c987b-136">We added a new MVC 5 template.</span></span> <span data-ttu-id="c987b-137">Он ссылается на последнюю пакеты MVC 5 NuGet и формирование шаблонов можно использовать для добавления контроллеров и представлений.</span><span class="sxs-lookup"><span data-stu-id="c987b-137">It references the latest MVC 5 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a><span data-ttu-id="c987b-138">Шаблон ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="c987b-138">ASP.NET Web API 2 template</span></span>

<span data-ttu-id="c987b-139">Мы добавили новый шаблон веб-API 2.</span><span class="sxs-lookup"><span data-stu-id="c987b-139">We added a new Web API 2 template.</span></span> <span data-ttu-id="c987b-140">Он ссылается на последнюю пакеты Web API 2 NuGet и формирование шаблонов можно использовать для добавления контроллеров и представлений.</span><span class="sxs-lookup"><span data-stu-id="c987b-140">It references the latest Web API 2 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="itemtemplate"></a>
#### <a name="item-templates"></a><span data-ttu-id="c987b-141">Шаблоны элементов</span><span class="sxs-lookup"><span data-stu-id="c987b-141">Item Templates</span></span>

<span data-ttu-id="c987b-142">Мы добавили новые шаблоны элементов для представления MVC 5, веб-страницы (Razor 3) и веб-API 2 контроллеров.</span><span class="sxs-lookup"><span data-stu-id="c987b-142">We added new item templates for MVC 5 views, Web Pages (Razor 3), and Web API 2 controllers.</span></span> <span data-ttu-id="c987b-143">Они установлены связанные пакеты NuGet в проект при добавлении новых элементов.</span><span class="sxs-lookup"><span data-stu-id="c987b-143">They install the related NuGet packages to the project while adding new items.</span></span>

<a id="ef6"></a>
### <a name="entity-framework-6"></a><span data-ttu-id="c987b-144">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="c987b-144">Entity Framework 6</span></span>

<span data-ttu-id="c987b-145">При формировании на основе скаффолдинга контроллер MVC или веб-API, использующий Entity Framework, мы используем Framework 6.</span><span class="sxs-lookup"><span data-stu-id="c987b-145">When you scaffold an MVC or Web API controller using Entity Framework, we use Framework 6.</span></span> <span data-ttu-id="c987b-146">Дополнительные сведения об Entity Framework см. в разделе [истории версий Entity Framework](https://msdn.com/data/jj574253).</span><span class="sxs-lookup"><span data-stu-id="c987b-146">For more information about Entity Framework, see the [Entity Framework Version History](https://msdn.com/data/jj574253).</span></span>

<span data-ttu-id="c987b-147">Можно также загрузить и установить средства платформы Entity Framework 6 для Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="c987b-147">You can also download and install the Entity Framework 6 Tools for Visual Studio 2012.</span></span> <span data-ttu-id="c987b-148">В разделе [получение платформы Entity Framework](https://msdn.com/data/ee712906#tooling).</span><span class="sxs-lookup"><span data-stu-id="c987b-148">See the [Get Entity Framework](https://msdn.com/data/ee712906#tooling).</span></span>

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="c987b-149">Формирование шаблонов ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c987b-149">ASP.NET Scaffolding</span></span>

<span data-ttu-id="c987b-150">Формирование шаблонов ASP.NET — это платформа создания кода для веб-приложений ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c987b-150">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="c987b-151">Он позволяет легко добавить стандартный код в проект, который взаимодействует с моделью данных.</span><span class="sxs-lookup"><span data-stu-id="c987b-151">It makes it easy to add boilerplate code to your project that interacts with a data model.</span></span>

<span data-ttu-id="c987b-152">В предыдущих версиях Visual Studio формирование шаблонов был ограничен проектов ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c987b-152">In previous versions of Visual Studio, scaffolding was limited to ASP.NET MVC projects.</span></span> <span data-ttu-id="c987b-153">Благодаря этому обновлению теперь можно использовать формирование шаблонов для любого проекта ASP.NET, включая веб-форм.</span><span class="sxs-lookup"><span data-stu-id="c987b-153">With this update, you can now use scaffolding for any ASP.NET project, including Web Forms.</span></span> <span data-ttu-id="c987b-154">Это обновление не поддерживает создание страниц для проекта веб-форм, но по-прежнему можно использовать формирование шаблонов с веб-формами путем добавления зависимостей MVC в проект.</span><span class="sxs-lookup"><span data-stu-id="c987b-154">This update does not support generating pages for a Web Forms project, but you can still use scaffolding with Web Forms by adding MVC dependencies to the project.</span></span> <span data-ttu-id="c987b-155">Поддержка создания страниц для веб-формы будет добавлена в будущем обновлении.</span><span class="sxs-lookup"><span data-stu-id="c987b-155">Support for generating pages for Web Forms will be added in a future update.</span></span>

<span data-ttu-id="c987b-156">При использовании формирования шаблонов мы убедитесь, что все требуемые зависимости устанавливаются в проекте.</span><span class="sxs-lookup"><span data-stu-id="c987b-156">When using scaffolding, we ensure that all required dependencies are installed in the project.</span></span> <span data-ttu-id="c987b-157">Например если для создания проекта веб-форм ASP.NET и затем использовать формирование шаблонов для добавления контроллера Web API, необходимые пакеты NuGet и ссылки добавляются в проект автоматически.</span><span class="sxs-lookup"><span data-stu-id="c987b-157">For example, if you start with an ASP.NET Web Forms project and then use scaffolding to add a Web API Controller, the required NuGet packages and references are added to your project automatically.</span></span>

<span data-ttu-id="c987b-158">Чтобы добавить формирование шаблонов MVC в проект веб-форм, добавьте **новый элемент формирования шаблонов** и выберите **зависимостей MVC 5** в диалоговом окне.</span><span class="sxs-lookup"><span data-stu-id="c987b-158">To add MVC scaffolding to a Web Forms project, add a **New Scaffolded Item** and select **MVC 5 Dependencies** in the dialog window.</span></span> <span data-ttu-id="c987b-159">Существует два варианта для формирования шаблонов MVC; Минимальными и полное.</span><span class="sxs-lookup"><span data-stu-id="c987b-159">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="c987b-160">При выборе минимальной только пакеты NuGet и ссылки для ASP.NET MVC добавляются в проект.</span><span class="sxs-lookup"><span data-stu-id="c987b-160">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="c987b-161">При выборе параметра «полная» добавляются минимальные зависимости, а также необходимые файлы содержимого проекта MVC.</span><span class="sxs-lookup"><span data-stu-id="c987b-161">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span>

<span data-ttu-id="c987b-162">Поддержка асинхронных контроллеров формирование шаблонов использует новые функции асинхронного с Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="c987b-162">Support for scaffolding async controllers uses the new async features from Entity Framework 6.</span></span>

<span data-ttu-id="c987b-163">Дополнительные сведения и учебники см. в разделе [Обзор формирование шаблонов ASP.NET](../2013/aspnet-scaffolding-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c987b-163">For more information and tutorials, see [ASP.NET Scaffolding Overview](../2013/aspnet-scaffolding-overview.md).</span></span> <span data-ttu-id="c987b-164">В следующих учебниках описано формирование шаблонов в Visual Studio 2013, но они также применимы к ASP.NET и 2013.1 Web Tools для Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="c987b-164">These tutorials show scaffolding with Visual Studio 2013, but they are also applicable to ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="razor"></a>
### <a name="razor-editor"></a><span data-ttu-id="c987b-165">Редактор Razor</span><span class="sxs-lookup"><span data-stu-id="c987b-165">Razor Editor</span></span>

<span data-ttu-id="c987b-166">Это обновление Visual Studio 2012 теперь поддерживает Razor 3 средства редактирования.</span><span class="sxs-lookup"><span data-stu-id="c987b-166">With this update, Visual Studio 2012 now supports Razor 3 tooling/editing.</span></span>

<a id="nuget"></a>
### <a name="nuget-27"></a><span data-ttu-id="c987b-167">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="c987b-167">NuGet 2.7</span></span>

<span data-ttu-id="c987b-168">NuGet 2.7 включает широкий набор новых функций, которые описаны в статье [заметки о выпуске 2.7 NuGet](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span><span class="sxs-lookup"><span data-stu-id="c987b-168">NuGet 2.7 includes a rich set of new features which are described in detail at [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span></span>

<span data-ttu-id="c987b-169">Эта версия NuGet устраняет необходимость для пользователей, чтобы явно разрешить NuGet восстановления отсутствующих пакетов.</span><span class="sxs-lookup"><span data-stu-id="c987b-169">This version of NuGet removes the need for users to explicitly allow NuGet to restore missing packages.</span></span> <span data-ttu-id="c987b-170">При установке NuGet 2.7, пользователи неявно даете согласие на автоматическое восстановление отсутствующих пакетов.</span><span class="sxs-lookup"><span data-stu-id="c987b-170">When installing NuGet 2.7, users implicitly consent to automatically restoring missing packages.</span></span> <span data-ttu-id="c987b-171">Пользователей можно явным образом отказаться восстановление пакета NuGet параметры в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c987b-171">Users can explicitly opt out of package restoration through the NuGet settings in Visual Studio.</span></span> <span data-ttu-id="c987b-172">Это изменение упрощает работу восстановления пакета.</span><span class="sxs-lookup"><span data-stu-id="c987b-172">This change simplifies how package restoration works.</span></span>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="c987b-173">Известные проблемы и критические изменения</span><span class="sxs-lookup"><span data-stu-id="c987b-173">Known Issues and Breaking Changes</span></span>

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="c987b-174">Формирование шаблонов ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c987b-174">ASP.NET Scaffolding</span></span>

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a><span data-ttu-id="c987b-175">MVC и API формирование шаблонов-HTTP 404 не найдено ошибок</span><span class="sxs-lookup"><span data-stu-id="c987b-175">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>

<span data-ttu-id="c987b-176">Если возникнет сообщение об ошибке при добавлении элемента формирования шаблонов в проект, возможно, проект может остаться в несогласованном состоянии.</span><span class="sxs-lookup"><span data-stu-id="c987b-176">If you encounter an error when adding a scaffolded item to a project, it is possible your project will be left in an inconsistent state.</span></span> <span data-ttu-id="c987b-177">Некоторые изменения, внесенные быть формирование шаблонов будет выполнен откат, но другие изменения, например установленные пакеты NuGet, не будет выполнен откат.</span><span class="sxs-lookup"><span data-stu-id="c987b-177">Some of the changes made be scaffolding will be rolled back but other changes, such as the installed NuGet packages, will not be rolled back.</span></span> <span data-ttu-id="c987b-178">Если откат изменения конфигурации маршрутизации, пользователи получат ошибку HTTP 404 при навигации для формирования шаблонов элементов.</span><span class="sxs-lookup"><span data-stu-id="c987b-178">If the routing configuration changes are rolled back, users will receive an HTTP 404 error when navigating to scaffolded items.</span></span>

<span data-ttu-id="c987b-179">Чтобы устранить эту ошибку для MVC, добавить новый элемент формирования шаблонов и выберите зависимостей MVC 5 (минимум или полный).</span><span class="sxs-lookup"><span data-stu-id="c987b-179">To fix this error for MVC, add a new scaffolded item and select MVC 5 Dependencies (either Minimal or Full).</span></span> <span data-ttu-id="c987b-180">Этот процесс будет добавить все необходимые изменения в проект.</span><span class="sxs-lookup"><span data-stu-id="c987b-180">This process will add all of the required changes to your project.</span></span>

<span data-ttu-id="c987b-181">Чтобы устранить эту ошибку для веб-API:</span><span class="sxs-lookup"><span data-stu-id="c987b-181">To fix this error for Web API:</span></span>

1. <span data-ttu-id="c987b-182">Добавьте следующий класс WebApiConfig в проект.</span><span class="sxs-lookup"><span data-stu-id="c987b-182">Add the following WebApiConfig class to your project.</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. <span data-ttu-id="c987b-183">Настройка WebApiConfig.Register в приложении\_Start-метод в файле Global.asax следующим образом:</span><span class="sxs-lookup"><span data-stu-id="c987b-183">Configure WebApiConfig.Register in the Application\_Start method in Global.asax as follows:</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a><span data-ttu-id="c987b-184">Visual Studio Express 2012 для Web перестает работать после добавления элемента формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="c987b-184">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>

<span data-ttu-id="c987b-185">Если Visual Studio Express 2012 для Web перестает работать после добавления элемента формирования шаблонов с Entity Framework (например, Web API 2 контроллер с действиями, использующий Entity Framework), это может означать, что Visual Studio Express не удается загрузить сборки образа в машинном коде в зависимости от System.Web.Extensions.</span><span class="sxs-lookup"><span data-stu-id="c987b-185">If Visual Studio Express 2012 for Web stops working after adding scaffolded item with Entity Framework (such as Web API 2 Controller with actions, using Entity Framework), it is possible that Visual Studio Express failed to load the native image of an assembly dependent on System.Web.Extensions.</span></span>

<span data-ttu-id="c987b-186">Чтобы устранить эту проблему, настройте Visual Studio Express для работы с образа MSIL System.Web.Extensions:</span><span class="sxs-lookup"><span data-stu-id="c987b-186">To correct this problem, configure Visual Studio Express to work with the MSIL image of System.Web.Extensions:</span></span>

1. <span data-ttu-id="c987b-187">Откройте командную строку с правами администратора.</span><span class="sxs-lookup"><span data-stu-id="c987b-187">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="c987b-188">Перейдите к 11.0\Common7\IDE %ProgramFiles%\Microsoft Visual Studio или 11.0\Common7\IDE % ProgramFiles(x86) %\Microsoft Visual Studio (для 64-разрядной Windows).</span><span class="sxs-lookup"><span data-stu-id="c987b-188">Go to %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE or %ProgramFiles(x86)%\Microsoft Visual Studio 11.0\Common7\IDE (for 64 bit Windows).</span></span>
3. <span data-ttu-id="c987b-189">Откройте VWDExpress.exe.config в текстовом редакторе.</span><span class="sxs-lookup"><span data-stu-id="c987b-189">Open VWDExpress.exe.config in a text editor.</span></span>
4. <span data-ttu-id="c987b-190">Добавьте следующую строку в списке &lt;конфигурации&gt;/&lt;среды выполнения&gt; элемента:</span><span class="sxs-lookup"><span data-stu-id="c987b-190">Add the following line under the &lt;configuration&gt;/&lt;runtime&gt; element:</span></span>  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. <span data-ttu-id="c987b-191">Перезапустите Visual Studio Express 2012 для Web.</span><span class="sxs-lookup"><span data-stu-id="c987b-191">Restart Visual Studio Express 2012 for Web.</span></span>

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a><span data-ttu-id="c987b-192">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="c987b-192">ASP.NET Razor 3</span></span>

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a><span data-ttu-id="c987b-193">Просмотр withBrowse файл cshtml WithorF5causes ошибка сервера</span><span class="sxs-lookup"><span data-stu-id="c987b-193">Viewing cshtml file withBrowse WithorF5causes a server error</span></span>

<span data-ttu-id="c987b-194">При создании проекта MVC 5 в Visual Studio 2012 (или откройте файл в проект Visual Studio 2012 MVC 5, созданный в Visual Studio 2013) и попытаться просмотреть файл cshtml с помощью просмотр с помощью либо нажмите клавишу F5, вы получите сообщение об ошибке с состояния — **ошибка сервера в Приложение «/»**.</span><span class="sxs-lookup"><span data-stu-id="c987b-194">When you create an MVC 5 project in Visual Studio 2012 (or open in Visual Studio 2012 an MVC 5 project that was created in Visual Studio 2013) and attempt to view a cshtml file by using Browse With or F5, you will receive an error that states - **Server Error in '/' Application**.</span></span> <span data-ttu-id="c987b-195">Пытается перейти к серверу`http://localhost:XXXX/Views/../XXXX.cshtml`</span><span class="sxs-lookup"><span data-stu-id="c987b-195">The server attempts to navigate to `http://localhost:XXXX/Views/../XXXX.cshtml`</span></span>

<span data-ttu-id="c987b-196">Чтобы устранить эту проблему, измените **действие при запуске** в проекте для **определенную страницу**.</span><span class="sxs-lookup"><span data-stu-id="c987b-196">To resolve this issue, change the **Start Action** setting in your project to **Specific Page**.</span></span> <span data-ttu-id="c987b-197">Необходимо указать значение для страницы.</span><span class="sxs-lookup"><span data-stu-id="c987b-197">You do not need to provide a value for the page.</span></span>

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

<span data-ttu-id="c987b-198">После этого изменения выбора F5 переходит в корневую папку приложения (`http://localhost:XXXX`).</span><span class="sxs-lookup"><span data-stu-id="c987b-198">After making this change, selecting F5 navigates to the root of your application (`http://localhost:XXXX`).</span></span> <span data-ttu-id="c987b-199">Такое поведение не так же, как для проектов MVC 5 в Visual Studio 2013, где **текущей страницы** параметр запускает откройте страницу.</span><span class="sxs-lookup"><span data-stu-id="c987b-199">This behavior is not the same as the behavior for MVC 5 projects in Visual Studio 2013, where the **Current Page** setting launches the open page.</span></span>

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a><span data-ttu-id="c987b-200">Перезапись URL-адреса и Tilde(~)</span><span class="sxs-lookup"><span data-stu-id="c987b-200">Url Rewrite and Tilde(~)</span></span>

<span data-ttu-id="c987b-201">После обновления до ASP.NET Razor 3 или MVC ASP.NET 5, нотации tilde(~) может перестать работать правильно при использовании операции перезаписи URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="c987b-201">After upgrading to ASP.NET Razor 3 or ASP.NET MVC 5, the tilde(~) notation may no longer work correctly if you are using URL rewrites.</span></span> <span data-ttu-id="c987b-202">Перезапись URL-адреса влияет нотация tilde(~) в HTML-элементов, например &lt;A /&gt;, &lt;СЦЕНАРИЯ /&gt;, &lt;ССЫЛКУ и&gt;, и поэтому больше не сопоставляет тильда в корневой каталог.</span><span class="sxs-lookup"><span data-stu-id="c987b-202">The URL rewrite affects the tilde(~) notation in HTML elements such as &lt;A/&gt;, &lt;SCRIPT/&gt;, &lt;LINK/&gt;, and as a result the tilde no longer maps to the root directory.</span></span>

<span data-ttu-id="c987b-203">Например, если переписывать запросы для **asp.net/content** для **asp.net**, атрибут href в &lt;A href = «~/content/» /&gt; разрешается в **/content/ содержимое и** вместо  **/** .</span><span class="sxs-lookup"><span data-stu-id="c987b-203">For example, if you rewrite requests for **asp.net/content** to **asp.net**, the href attribute in &lt;A href="~/content/"/&gt; resolves to **/content/content/** instead of **/**.</span></span> <span data-ttu-id="c987b-204">Чтобы отменить это изменение, можно задать **IIS\_WasUrlRewritten** контекста, значение false в каждой веб-страницы или в **приложения\_BeginRequest** в файле Global.asax.</span><span class="sxs-lookup"><span data-stu-id="c987b-204">To suppress this change, you can set the **IIS\_WasUrlRewritten** context to false in each Web Page or in **Application\_BeginRequest** in Global.asax.</span></span>

<a id="templateissue"></a>
### <a name="templates"></a><span data-ttu-id="c987b-205">Шаблоны</span><span class="sxs-lookup"><span data-stu-id="c987b-205">Templates</span></span>

<span data-ttu-id="c987b-206">При создании ASP.NET MVC проектов с помощью Visual Studio 2012 на Windows 8.1 или Windows Server 2012 R2, Visual Studio отображает сообщение об ошибке, указывающее, «Настройка Web [url] для ASP.NET 4.5 не удалось.»</span><span class="sxs-lookup"><span data-stu-id="c987b-206">When you create ASP.NET MVC projects with Visual Studio 2012 on Windows 8.1 or Windows Server 2012 R2, Visual Studio displays an error message that states "Configuring Web [url] for ASP.NET 4.5 failed."</span></span>

![Ошибка конфигурации](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

<span data-ttu-id="c987b-208">Эта ошибка возникает, поскольку Visual Studio 2012 не поддерживает компонент ASP.NET 4.5, установленные в этих версиях Windows.</span><span class="sxs-lookup"><span data-stu-id="c987b-208">You see this error because Visual Studio 2012 does not enable the ASP.NET 4.5 feature when it is installed on those releases of Windows.</span></span> <span data-ttu-id="c987b-209">Чтобы включить ASP.NET 4.5, выполните действия, описанные в [Включение или отключение компонентов](https://windows.microsoft.com/en-us/windows-8/turn-windows-features-on-off).</span><span class="sxs-lookup"><span data-stu-id="c987b-209">To enable ASP.NET 4.5, perform the steps described in [Turn Windows features on or off](https://windows.microsoft.com/en-us/windows-8/turn-windows-features-on-off).</span></span>

![Включение и отключение компонентов Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

<span data-ttu-id="c987b-211">Кроме того можно включить ASP.NET 4.5 через командную строку.</span><span class="sxs-lookup"><span data-stu-id="c987b-211">Alternatively, you can enable ASP.NET 4.5 through the command line.</span></span>

1. <span data-ttu-id="c987b-212">Откройте командную строку с правами администратора.</span><span class="sxs-lookup"><span data-stu-id="c987b-212">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="c987b-213">Выполните следующую команду, чтобы включить ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="c987b-213">Run the following command to enable ASP.NET 4.5.</span></span>  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
