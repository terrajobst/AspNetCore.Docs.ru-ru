---
uid: mvc/overview/releases/mvc51-release-notes
title: Новые возможности в ASP.NET MVC 5.1 | Документы Microsoft
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: be10486c9fd39738f44cdda4fedb409058017601
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/10/2018
ms.locfileid: "26504053"
---
<a name="whats-new-in-aspnet-mvc-51"></a><span data-ttu-id="959ac-102">Новые возможности в ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="959ac-102">What's New in ASP.NET MVC 5.1</span></span>
====================
<span data-ttu-id="959ac-103">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="959ac-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="959ac-104">В этом разделе описываются новые возможности в ASP.NET Web MVC 5.1.</span><span class="sxs-lookup"><span data-stu-id="959ac-104">This topic describes what's new for ASP.NET Web MVC 5.1.</span></span>

- [<span data-ttu-id="959ac-105">Требования к программному обеспечению</span><span class="sxs-lookup"><span data-stu-id="959ac-105">Software Requirements</span></span>](#SoftwareRequirements)
- [<span data-ttu-id="959ac-106">Скачать</span><span class="sxs-lookup"><span data-stu-id="959ac-106">Download</span></span>](#download)
- [<span data-ttu-id="959ac-107">Документация</span><span class="sxs-lookup"><span data-stu-id="959ac-107">Documentation</span></span>](#documentation)
- [<span data-ttu-id="959ac-108">Новые возможности в ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="959ac-108">New Features in ASP.NET MVC 5.1</span></span>](#new-features)

    - [<span data-ttu-id="959ac-109">Атрибут улучшения маршрутизации</span><span class="sxs-lookup"><span data-stu-id="959ac-109">Attribute routing improvements</span></span>](#AttributeRouting)
    - [<span data-ttu-id="959ac-110">Шаблоны редактора начальной загрузки поддерживаются</span><span class="sxs-lookup"><span data-stu-id="959ac-110">Bootstrap support for editor templates</span></span>](#Bootstrap)
    - [<span data-ttu-id="959ac-111">Поддержка перечислений в представлениях</span><span class="sxs-lookup"><span data-stu-id="959ac-111">Enum support in views</span></span>](#Enum)
    - [<span data-ttu-id="959ac-112">Ненавязчивой проверки для MinLength и MaxLength атрибутов</span><span class="sxs-lookup"><span data-stu-id="959ac-112">Unobtrusive validation for MinLength/MaxLength Attributes</span></span>](#Unobtrusive)
    - [<span data-ttu-id="959ac-113">Поддержка контекст «this» в ненавязчивого Ajax</span><span class="sxs-lookup"><span data-stu-id="959ac-113">Supporting the ‘this' context in Unobtrusive Ajax</span></span>](#thisContext)
- <span data-ttu-id="959ac-114">[Известные проблемы и критические изменения](#KnownBreakingChanges)- [исправления ошибок](#bug-fixes)</span><span class="sxs-lookup"><span data-stu-id="959ac-114">[Known Issues and Breaking Changes](#KnownBreakingChanges)- [Bug Fixes](#bug-fixes)</span></span>

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="959ac-115">Требования к программному обеспечению</span><span class="sxs-lookup"><span data-stu-id="959ac-115">Software Requirements</span></span>

- <span data-ttu-id="959ac-116">Visual Studio 2012: Загрузите [ASP.NET и веб-средств 2013.1 для Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).</span><span class="sxs-lookup"><span data-stu-id="959ac-116">Visual Studio 2012: Download [ASP.NET and Web Tools 2013.1 for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).</span></span>
- <span data-ttu-id="959ac-117">Visual Studio 2013: Загрузите [Visual Studio 2013 с обновлением 1](https://go.microsoft.com/fwlink/?LinkId=390064).</span><span class="sxs-lookup"><span data-stu-id="959ac-117">Visual Studio 2013: Download [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064).</span></span> <span data-ttu-id="959ac-118">Это обновление требуется для изменения представления Razor ASP.NET MVC 5.1.</span><span class="sxs-lookup"><span data-stu-id="959ac-118">This update is needed for editing ASP.NET MVC 5.1 Razor Views.</span></span>

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="959ac-119">Скачать</span><span class="sxs-lookup"><span data-stu-id="959ac-119">Download</span></span>

<span data-ttu-id="959ac-120">Компоненты среды выполнения выпущены в виде пакетов NuGet при галереи NuGet.</span><span class="sxs-lookup"><span data-stu-id="959ac-120">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="959ac-121">Выполните все пакеты среды выполнения [семантического управления версиями](http://semver.org/) спецификации.</span><span class="sxs-lookup"><span data-stu-id="959ac-121">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="959ac-122">Последнюю версию пакета ASP.NET MVC 5.1 RTM имеет следующую версию: «5.1.2».</span><span class="sxs-lookup"><span data-stu-id="959ac-122">The latest ASP.NET MVC 5.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="959ac-123">Можно установить или обновить эти пакеты через [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span><span class="sxs-lookup"><span data-stu-id="959ac-123">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span></span> <span data-ttu-id="959ac-124">Этот выпуск также включает соответствующие локализованные пакеты в NuGet.</span><span class="sxs-lookup"><span data-stu-id="959ac-124">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="959ac-125">Можно установить или обновить выпущенных пакетов NuGet, используя консоль диспетчера пакетов NuGet:</span><span class="sxs-lookup"><span data-stu-id="959ac-125">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="959ac-126">Документация</span><span class="sxs-lookup"><span data-stu-id="959ac-126">Documentation</span></span>

<span data-ttu-id="959ac-127">Учебники и другие сведения о ASP.NET MVC 5.1 RTM доступны на веб-сайт ASP.NET ( https://www.asp.net).</span><span class="sxs-lookup"><span data-stu-id="959ac-127">Tutorials and other information about ASP.NET MVC 5.1 RTM are available from the ASP.NET web site ( https://www.asp.net).</span></span> 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a><span data-ttu-id="959ac-128">Новые возможности в ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="959ac-128">New Features in ASP.NET MVC 5.1</span></span>

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a><span data-ttu-id="959ac-129">Атрибут улучшения маршрутизации</span><span class="sxs-lookup"><span data-stu-id="959ac-129">Attribute routing improvements</span></span>

 <span data-ttu-id="959ac-130">Выбор маршрута основан атрибут, маршрутизация теперь поддерживает ограничения, Включение управления версиями и заголовок.</span><span class="sxs-lookup"><span data-stu-id="959ac-130">Attribute routing now supports constraints, enabling versioning and header based route selection.</span></span> <span data-ttu-id="959ac-131">Многие аспекты маршруты атрибута теперь настраиваются через `IDirectRouteFactory` интерфейс и `RouteFactoryAttribute` класса.</span><span class="sxs-lookup"><span data-stu-id="959ac-131">Many aspects of attribute routes are now customizable via the `IDirectRouteFactory` interface and `RouteFactoryAttribute` class.</span></span> <span data-ttu-id="959ac-132">Префикс маршрута теперь можно расширять посредством `IRoutePrefix` интерфейс и `RoutePrefixAttribute` класса.</span><span class="sxs-lookup"><span data-stu-id="959ac-132">The route prefix is now extensible via the `IRoutePrefix` interface and `RoutePrefixAttribute` class.</span></span> 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a><span data-ttu-id="959ac-133">Поддержка перечислений в представлениях</span><span class="sxs-lookup"><span data-stu-id="959ac-133">Enum support in views</span></span>

1. <span data-ttu-id="959ac-134">Новый `@Html.EnumDropDownListFor()` вспомогательные методы.</span><span class="sxs-lookup"><span data-stu-id="959ac-134">New `@Html.EnumDropDownListFor()` helper methods.</span></span> <span data-ttu-id="959ac-135">Эту возможность можно использовать как и большинство вспомогательных методов HTML помнить, что выражение должно возвращать [перечисления](https://msdn.microsoft.com/en-us/library/cc138362.aspx) типа или [Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) где `T` является [перечисления](https://msdn.microsoft.com/en-us/library/cc138362.aspx) типа.</span><span class="sxs-lookup"><span data-stu-id="959ac-135">These should be used like most of the HTML helpers with the caveat that the expression must evaluate to an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type or a [Nullable&lt;T&gt;](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) where `T` is an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type.</span></span> <span data-ttu-id="959ac-136">Используйте `EnumHelper.IsValidForEnumHelper()` для проверки этих требований.</span><span class="sxs-lookup"><span data-stu-id="959ac-136">Use `EnumHelper.IsValidForEnumHelper()` to check these requirements.</span></span>
2. <span data-ttu-id="959ac-137">Новый `EnumHelper.GetSelectList()` методы, которые возвращают `IList<SelectListItem>`.</span><span class="sxs-lookup"><span data-stu-id="959ac-137">New `EnumHelper.GetSelectList()` methods which return an `IList<SelectListItem>`.</span></span> <span data-ttu-id="959ac-138">Это полезно, когда необходимость в управлении список выбора до вызова метода, например, `@Html.DropDownListFor()`, или если требуется отобразить имена которого `@Html.EnumDropDownListFor()` показано.</span><span class="sxs-lookup"><span data-stu-id="959ac-138">This is useful when you need to manipulate a select list prior to calling, for example, `@Html.DropDownListFor()`, or when you wish to display the names which `@Html.EnumDropDownListFor()` shows.</span></span>

<span data-ttu-id="959ac-139">В следующем коде показано эти API-интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="959ac-139">The following code shows these APIs.</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

<span data-ttu-id="959ac-140">Полный пример [здесь](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).</span><span class="sxs-lookup"><span data-stu-id="959ac-140">You can see a complete example [here](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).</span></span>

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a><span data-ttu-id="959ac-141">Шаблоны редактора начальной загрузки поддерживаются</span><span class="sxs-lookup"><span data-stu-id="959ac-141">Bootstrap support for editor templates</span></span>

<span data-ttu-id="959ac-142">Теперь допускается передача в атрибутах HTML в [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) как [анонимный объект](https://msdn.microsoft.com/en-us/library/bb397696.aspx).</span><span class="sxs-lookup"><span data-stu-id="959ac-142">We now allow passing in HTML attributes in [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) as an [anonymous object](https://msdn.microsoft.com/en-us/library/bb397696.aspx).</span></span>

<span data-ttu-id="959ac-143">Пример:</span><span class="sxs-lookup"><span data-stu-id="959ac-143">For example:</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a><span data-ttu-id="959ac-144">Ненавязчивой проверки MinLengthAttribute и MaxLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="959ac-144">Unobtrusive validation for MinLengthAttribute and MaxLengthAttribute</span></span>

<span data-ttu-id="959ac-145">Проверки на стороне клиента для строковых и массива типов теперь поддерживается для свойства декорированных [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) и [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) атрибуты.</span><span class="sxs-lookup"><span data-stu-id="959ac-145">Client-side validation for string and array types will now be supported for properties decorated with the [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) and [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) attributes.</span></span>

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a><span data-ttu-id="959ac-146">Поддержка контекст «this» в ненавязчивого Ajax</span><span class="sxs-lookup"><span data-stu-id="959ac-146">Supporting the ‘this' context in Unobtrusive Ajax</span></span>

<span data-ttu-id="959ac-147">Функции обратного вызова (`OnBegin, OnComplete, OnFailure, OnSuccess`) теперь можно найти элемент вызова через `this` контекста.</span><span class="sxs-lookup"><span data-stu-id="959ac-147">The callback functions (`OnBegin, OnComplete, OnFailure, OnSuccess`) will now be able to locate the invoking element via the `this` context.</span></span> <span data-ttu-id="959ac-148">Пример:</span><span class="sxs-lookup"><span data-stu-id="959ac-148">For example:</span></span>

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="959ac-149">Известные проблемы и критические изменения</span><span class="sxs-lookup"><span data-stu-id="959ac-149">Known Issues and Breaking Changes</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="959ac-150">Атрибут маршрутизации</span><span class="sxs-lookup"><span data-stu-id="959ac-150">Attribute Routing</span></span>

<span data-ttu-id="959ac-151">Неоднозначности в маршрутизации соответствует атрибута теперь выдает ошибку вместо выбора первого совпадения.</span><span class="sxs-lookup"><span data-stu-id="959ac-151">Ambiguities in attribute routing matches will now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="959ac-152">Маршруты атрибутов не могут использовать `{controller}` параметра и с помощью `{action}` параметр маршрутов разместить на действия.</span><span class="sxs-lookup"><span data-stu-id="959ac-152">Attribute routes are prohibited from using the `{controller}` parameter, and from using the `{action}` parameter on routes placed on actions.</span></span> <span data-ttu-id="959ac-153">Использует эти параметры большой вероятностью приведет к неоднозначности.</span><span class="sxs-lookup"><span data-stu-id="959ac-153">Uses of these parameters would very likely lead to ambiguities.</span></span> 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="959ac-154">API MVC или веб-формирования шаблонов в проект 5.1 пакетов приводит к получению 5.0 пакетов для те, которые уже не существуют в проекте</span><span class="sxs-lookup"><span data-stu-id="959ac-154">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="959ac-155">Обновление пакетов NuGet для ASP.NET MVC 5.1 RTM не обновляет средства Visual Studio, такие как формирование шаблонов ASP.NET или шаблон проекта веб-приложения ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="959ac-155">Updating NuGet packages for ASP.NET MVC 5.1 RTM does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="959ac-156">Они используют предыдущую версию пакета среды выполнения ASP.NET (5.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="959ac-156">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="959ac-157">В результате формирование шаблонов ASP.NET установит предыдущую версию (5.0.0.0) необходимые пакеты, если они отсутствуют доступные в проектах.</span><span class="sxs-lookup"><span data-stu-id="959ac-157">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="959ac-158">Формирование шаблонов ASP.NET в Visual Studio 2013 RTM или обновлением 1 не перезаписывает последние пакеты в проекте.</span><span class="sxs-lookup"><span data-stu-id="959ac-158">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="959ac-159">Если вы используете формирование шаблонов ASP.NET после обновления пакеты проектов Web API 2.1 или ASP.NET MVC 5.1, убедитесь, что версии веб-API и ASP.NET MVC, согласованы.</span><span class="sxs-lookup"><span data-stu-id="959ac-159">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span> 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a><span data-ttu-id="959ac-160">Выделение синтаксиса для представлений Razor в Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="959ac-160">Syntax Highlighting for Razor Views in Visual Studio 2013</span></span>

<span data-ttu-id="959ac-161">При обновлении до ASP.NET MVC 5.1 RTM без обновления Visual Studio 2013, не будет Поддержка редактора Visual Studio для выделения синтаксиса при изменении представлений Razor.</span><span class="sxs-lookup"><span data-stu-id="959ac-161">If you update to ASP.NET MVC 5.1 RTM without updating Visual Studio 2013, you will not get Visual Studio editor support for syntax highlighting while editing the Razor views.</span></span> <span data-ttu-id="959ac-162">Будет необходимо обновить Visual Studio 2013 для получения этой поддержки.</span><span class="sxs-lookup"><span data-stu-id="959ac-162">You will need to update Visual Studio 2013 to get this support.</span></span> 

### <a name="type-renames"></a><span data-ttu-id="959ac-163">Переименование типа</span><span class="sxs-lookup"><span data-stu-id="959ac-163">Type Renames</span></span>

<span data-ttu-id="959ac-164">Некоторые типы, используемые для расширения маршрутизации атрибута будут переименованы в 5.1 RTM.</span><span class="sxs-lookup"><span data-stu-id="959ac-164">Some of the types used for attribute routing extensibility are renamed in 5.1 RTM.</span></span>

| <span data-ttu-id="959ac-165">**Старое имя типа (5.1 версия-Кандидат)**</span><span class="sxs-lookup"><span data-stu-id="959ac-165">**Old Type Name (5.1 RC)**</span></span> | <span data-ttu-id="959ac-166">**Введите новое имя (5.1 RTM)**</span><span class="sxs-lookup"><span data-stu-id="959ac-166">**New Type Name (5.1 RTM)**</span></span> |
| --- | --- |
| <span data-ttu-id="959ac-167">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="959ac-167">IDirectRouteProvider</span></span> | <span data-ttu-id="959ac-168">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="959ac-168">IDirectRouteFactory</span></span> |
| <span data-ttu-id="959ac-169">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="959ac-169">RouteProviderAttribute</span></span> | <span data-ttu-id="959ac-170">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="959ac-170">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="959ac-171">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="959ac-171">DirectRouteProviderContext</span></span> | <span data-ttu-id="959ac-172">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="959ac-172">DirectRouteFactoryContext</span></span> |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="959ac-173">Исправления ошибок</span><span class="sxs-lookup"><span data-stu-id="959ac-173">Bug Fixes</span></span>

<span data-ttu-id="959ac-174">Этот выпуск также включает несколько исправлений ошибок.</span><span class="sxs-lookup"><span data-stu-id="959ac-174">This release also includes several bug fixes.</span></span> <span data-ttu-id="959ac-175">Можно найти полный список здесь:</span><span class="sxs-lookup"><span data-stu-id="959ac-175">You can find the complete list here:</span></span>

- [<span data-ttu-id="959ac-176">5.1.0 пакет</span><span class="sxs-lookup"><span data-stu-id="959ac-176">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="959ac-177">5.1.1 пакета</span><span class="sxs-lookup"><span data-stu-id="959ac-177">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<span data-ttu-id="959ac-178">5.1.2 пакет содержит обновления IntelliSense, но без исправления ошибок.</span><span class="sxs-lookup"><span data-stu-id="959ac-178">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
