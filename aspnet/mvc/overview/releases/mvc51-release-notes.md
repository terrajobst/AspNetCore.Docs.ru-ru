---
uid: mvc/overview/releases/mvc51-release-notes
title: Новые возможности в ASP.NET MVC 5.1 | Документация Майкрософт
author: microsoft
description: ''
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 6cdf5ae42457ab10e30693a1b77b4aee65570be5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836986"
---
<a name="whats-new-in-aspnet-mvc-51"></a><span data-ttu-id="d8949-102">Новые возможности в ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="d8949-102">What's New in ASP.NET MVC 5.1</span></span>
====================
<span data-ttu-id="d8949-103">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d8949-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="d8949-104">В этом разделе описываются новые возможности ASP.NET Web MVC 5.1.</span><span class="sxs-lookup"><span data-stu-id="d8949-104">This topic describes what's new for ASP.NET Web MVC 5.1.</span></span>

- [<span data-ttu-id="d8949-105">Требования к программному обеспечению</span><span class="sxs-lookup"><span data-stu-id="d8949-105">Software Requirements</span></span>](#SoftwareRequirements)
- [<span data-ttu-id="d8949-106">Скачать</span><span class="sxs-lookup"><span data-stu-id="d8949-106">Download</span></span>](#download)
- [<span data-ttu-id="d8949-107">Документация</span><span class="sxs-lookup"><span data-stu-id="d8949-107">Documentation</span></span>](#documentation)
- [<span data-ttu-id="d8949-108">Новые возможности в ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="d8949-108">New Features in ASP.NET MVC 5.1</span></span>](#new-features)

    - [<span data-ttu-id="d8949-109">Атрибут улучшения маршрутизации</span><span class="sxs-lookup"><span data-stu-id="d8949-109">Attribute routing improvements</span></span>](#AttributeRouting)
    - [<span data-ttu-id="d8949-110">Поддержка начальной загрузки для редактора шаблонов</span><span class="sxs-lookup"><span data-stu-id="d8949-110">Bootstrap support for editor templates</span></span>](#Bootstrap)
    - [<span data-ttu-id="d8949-111">Поддержке нумераций в представлениях</span><span class="sxs-lookup"><span data-stu-id="d8949-111">Enum support in views</span></span>](#Enum)
    - [<span data-ttu-id="d8949-112">Малозаметная проверка для MinLength и MaxLength атрибутов</span><span class="sxs-lookup"><span data-stu-id="d8949-112">Unobtrusive validation for MinLength/MaxLength Attributes</span></span>](#Unobtrusive)
    - [<span data-ttu-id="d8949-113">Поддержка «this» контекст в ненавязчивого Ajax</span><span class="sxs-lookup"><span data-stu-id="d8949-113">Supporting the ‘this' context in Unobtrusive Ajax</span></span>](#thisContext)
- <span data-ttu-id="d8949-114">[Известные проблемы и критические изменения](#KnownBreakingChanges)- [исправления ошибок](#bug-fixes)</span><span class="sxs-lookup"><span data-stu-id="d8949-114">[Known Issues and Breaking Changes](#KnownBreakingChanges)- [Bug Fixes](#bug-fixes)</span></span>

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="d8949-115">Требования к программному обеспечению</span><span class="sxs-lookup"><span data-stu-id="d8949-115">Software Requirements</span></span>

- <span data-ttu-id="d8949-116">Visual Studio 2012: Скачайте [ASP.NET и веб-инструменты 2013.1 для Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).</span><span class="sxs-lookup"><span data-stu-id="d8949-116">Visual Studio 2012: Download [ASP.NET and Web Tools 2013.1 for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).</span></span>
- <span data-ttu-id="d8949-117">Visual Studio 2013: Загрузки [Visual Studio 2013 с обновлением 1](https://go.microsoft.com/fwlink/?LinkId=390064).</span><span class="sxs-lookup"><span data-stu-id="d8949-117">Visual Studio 2013: Download [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064).</span></span> <span data-ttu-id="d8949-118">Это обновление требуется для изменения представления Razor ASP.NET MVC 5.1.</span><span class="sxs-lookup"><span data-stu-id="d8949-118">This update is needed for editing ASP.NET MVC 5.1 Razor Views.</span></span>

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="d8949-119">Скачать</span><span class="sxs-lookup"><span data-stu-id="d8949-119">Download</span></span>

<span data-ttu-id="d8949-120">Компоненты среды выполнения выпускаются в виде пакетов NuGet из коллекции NuGet.</span><span class="sxs-lookup"><span data-stu-id="d8949-120">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="d8949-121">Выполните все пакеты среды выполнения [семантического управления версиями](http://semver.org/) спецификации.</span><span class="sxs-lookup"><span data-stu-id="d8949-121">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="d8949-122">Последнюю версию пакета ASP.NET MVC 5.1 RTM имеет следующую версию: «5.1.2».</span><span class="sxs-lookup"><span data-stu-id="d8949-122">The latest ASP.NET MVC 5.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="d8949-123">Вы можете установить или обновить эти пакеты с помощью [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span><span class="sxs-lookup"><span data-stu-id="d8949-123">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span></span> <span data-ttu-id="d8949-124">Этот выпуск также включает соответствующие локализованные пакеты в NuGet.</span><span class="sxs-lookup"><span data-stu-id="d8949-124">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="d8949-125">Можно установить или обновить на выпущенных пакетов NuGet с помощью консоли диспетчера пакетов NuGet:</span><span class="sxs-lookup"><span data-stu-id="d8949-125">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="d8949-126">Документация</span><span class="sxs-lookup"><span data-stu-id="d8949-126">Documentation</span></span>

<span data-ttu-id="d8949-127">Учебники и другие сведения о ASP.NET MVC 5.1 RTM доступны на веб-сайте ASP.NET ( https://www.asp.net).</span><span class="sxs-lookup"><span data-stu-id="d8949-127">Tutorials and other information about ASP.NET MVC 5.1 RTM are available from the ASP.NET web site ( https://www.asp.net).</span></span> 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a><span data-ttu-id="d8949-128">Новые возможности в ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="d8949-128">New Features in ASP.NET MVC 5.1</span></span>

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a><span data-ttu-id="d8949-129">Атрибут улучшения маршрутизации</span><span class="sxs-lookup"><span data-stu-id="d8949-129">Attribute routing improvements</span></span>

 <span data-ttu-id="d8949-130">Выбор маршрута на основе атрибутов, маршрутизация теперь поддерживает ограничения, обеспечивая управление версиями и заголовок.</span><span class="sxs-lookup"><span data-stu-id="d8949-130">Attribute routing now supports constraints, enabling versioning and header based route selection.</span></span> <span data-ttu-id="d8949-131">Многие аспекты маршруты с атрибутами теперь настраиваются с помощью `IDirectRouteFactory` интерфейс и `RouteFactoryAttribute` класса.</span><span class="sxs-lookup"><span data-stu-id="d8949-131">Many aspects of attribute routes are now customizable via the `IDirectRouteFactory` interface and `RouteFactoryAttribute` class.</span></span> <span data-ttu-id="d8949-132">Префикс маршрута теперь можно расширять посредством `IRoutePrefix` интерфейс и `RoutePrefixAttribute` класса.</span><span class="sxs-lookup"><span data-stu-id="d8949-132">The route prefix is now extensible via the `IRoutePrefix` interface and `RoutePrefixAttribute` class.</span></span> 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a><span data-ttu-id="d8949-133">Поддержке нумераций в представлениях</span><span class="sxs-lookup"><span data-stu-id="d8949-133">Enum support in views</span></span>

1. <span data-ttu-id="d8949-134">Новый `@Html.EnumDropDownListFor()` вспомогательные методы.</span><span class="sxs-lookup"><span data-stu-id="d8949-134">New `@Html.EnumDropDownListFor()` helper methods.</span></span> <span data-ttu-id="d8949-135">Они должны использоваться как и большинство вспомогательных методов HTML, если выражение должно возвращать [перечисления](https://msdn.microsoft.com/en-us/library/cc138362.aspx) тип или [Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) где `T` является [перечисления](https://msdn.microsoft.com/en-us/library/cc138362.aspx) типа.</span><span class="sxs-lookup"><span data-stu-id="d8949-135">These should be used like most of the HTML helpers with the caveat that the expression must evaluate to an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type or a [Nullable&lt;T&gt;](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) where `T` is an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type.</span></span> <span data-ttu-id="d8949-136">Используйте `EnumHelper.IsValidForEnumHelper()` для проверки этих требований.</span><span class="sxs-lookup"><span data-stu-id="d8949-136">Use `EnumHelper.IsValidForEnumHelper()` to check these requirements.</span></span>
2. <span data-ttu-id="d8949-137">Новый `EnumHelper.GetSelectList()` методы, которые возвращают `IList<SelectListItem>`.</span><span class="sxs-lookup"><span data-stu-id="d8949-137">New `EnumHelper.GetSelectList()` methods which return an `IList<SelectListItem>`.</span></span> <span data-ttu-id="d8949-138">Это полезно, когда необходимость в управлении в списке выбора, перед вызовом функции, например, `@Html.DropDownListFor()`, или если требуется отобразить имена которого `@Html.EnumDropDownListFor()` показано.</span><span class="sxs-lookup"><span data-stu-id="d8949-138">This is useful when you need to manipulate a select list prior to calling, for example, `@Html.DropDownListFor()`, or when you wish to display the names which `@Html.EnumDropDownListFor()` shows.</span></span>

<span data-ttu-id="d8949-139">В следующем коде показано эти API-интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="d8949-139">The following code shows these APIs.</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

<span data-ttu-id="d8949-140">Полный пример можно просмотреть [здесь](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).</span><span class="sxs-lookup"><span data-stu-id="d8949-140">You can see a complete example [here](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).</span></span>

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a><span data-ttu-id="d8949-141">Поддержка начальной загрузки для редактора шаблонов</span><span class="sxs-lookup"><span data-stu-id="d8949-141">Bootstrap support for editor templates</span></span>

<span data-ttu-id="d8949-142">Теперь мы разрешаем передачи в атрибутах HTML в [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) как [анонимный объект](https://msdn.microsoft.com/en-us/library/bb397696.aspx).</span><span class="sxs-lookup"><span data-stu-id="d8949-142">We now allow passing in HTML attributes in [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) as an [anonymous object](https://msdn.microsoft.com/en-us/library/bb397696.aspx).</span></span>

<span data-ttu-id="d8949-143">Пример:</span><span class="sxs-lookup"><span data-stu-id="d8949-143">For example:</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a><span data-ttu-id="d8949-144">Малозаметная проверка MinLengthAttribute и MaxLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="d8949-144">Unobtrusive validation for MinLengthAttribute and MaxLengthAttribute</span></span>

<span data-ttu-id="d8949-145">Проверка на стороне клиента для строковых и массива типов будут поддерживаться, теперь для свойства с атрибутом [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) и [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) атрибуты.</span><span class="sxs-lookup"><span data-stu-id="d8949-145">Client-side validation for string and array types will now be supported for properties decorated with the [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) and [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) attributes.</span></span>

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a><span data-ttu-id="d8949-146">Поддержка «this» контекст в ненавязчивого Ajax</span><span class="sxs-lookup"><span data-stu-id="d8949-146">Supporting the ‘this' context in Unobtrusive Ajax</span></span>

<span data-ttu-id="d8949-147">В функции обратного вызова (`OnBegin, OnComplete, OnFailure, OnSuccess`) теперь будут иметь возможность найти элемент вызов через `this` контекста.</span><span class="sxs-lookup"><span data-stu-id="d8949-147">The callback functions (`OnBegin, OnComplete, OnFailure, OnSuccess`) will now be able to locate the invoking element via the `this` context.</span></span> <span data-ttu-id="d8949-148">Пример:</span><span class="sxs-lookup"><span data-stu-id="d8949-148">For example:</span></span>

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="d8949-149">Известные проблемы и критические изменения</span><span class="sxs-lookup"><span data-stu-id="d8949-149">Known Issues and Breaking Changes</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="d8949-150">Маршрутизация с помощью атрибутов</span><span class="sxs-lookup"><span data-stu-id="d8949-150">Attribute Routing</span></span>

<span data-ttu-id="d8949-151">Неоднозначности в маршрутизации соответствует атрибута теперь сообщает ошибку вместо выбора первого совпадения.</span><span class="sxs-lookup"><span data-stu-id="d8949-151">Ambiguities in attribute routing matches will now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="d8949-152">Маршруты с атрибутами, запрещается использовать `{controller}` параметра и обратно с помощью `{action}` параметр на маршрутах уделено действия.</span><span class="sxs-lookup"><span data-stu-id="d8949-152">Attribute routes are prohibited from using the `{controller}` parameter, and from using the `{action}` parameter on routes placed on actions.</span></span> <span data-ttu-id="d8949-153">Сценарии использования этих параметров большой вероятностью приведет к неоднозначности.</span><span class="sxs-lookup"><span data-stu-id="d8949-153">Uses of these parameters would very likely lead to ambiguities.</span></span> 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="d8949-154">Формирование шаблонов MVC и веб-API в проект с помощью 5.1 результатов пакеты в 5.0 пакеты для тех, которые еще не существуют в проекте</span><span class="sxs-lookup"><span data-stu-id="d8949-154">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="d8949-155">Обновление пакетов NuGet для ASP.NET MVC 5.1 RTM-версии не обновить инструменты Visual Studio, такие как формирование шаблонов ASP.NET или шаблон проекта веб-приложения ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d8949-155">Updating NuGet packages for ASP.NET MVC 5.1 RTM does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="d8949-156">Они используют предыдущую версию пакетов среды выполнения ASP.NET (5.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="d8949-156">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="d8949-157">Таким образом формирование шаблонов ASP.NET установит предыдущую версию (5.0.0.0) необходимые пакеты, если они еще не доступны в проектах.</span><span class="sxs-lookup"><span data-stu-id="d8949-157">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="d8949-158">Тем не менее формирование шаблонов ASP.NET в Visual Studio 2013 RTM или с обновлением 1 не перезаписывает последних пакетов в проектах.</span><span class="sxs-lookup"><span data-stu-id="d8949-158">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="d8949-159">Если вы используете формирование шаблонов ASP.NET после обновления пакеты проектов Web API 2.1 или ASP.NET MVC 5.1, убедитесь, что в версиях веб-API и ASP.NET MVC являются согласованными.</span><span class="sxs-lookup"><span data-stu-id="d8949-159">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span> 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a><span data-ttu-id="d8949-160">Выделение синтаксиса для представления Razor в Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="d8949-160">Syntax Highlighting for Razor Views in Visual Studio 2013</span></span>

<span data-ttu-id="d8949-161">Если вы обновляете до ASP.NET MVC 5.1 RTM-версии без обновления Visual Studio 2013, не будет поддержки редактора Visual Studio для выделения синтаксиса во время редактирования представлениями Razor.</span><span class="sxs-lookup"><span data-stu-id="d8949-161">If you update to ASP.NET MVC 5.1 RTM without updating Visual Studio 2013, you will not get Visual Studio editor support for syntax highlighting while editing the Razor views.</span></span> <span data-ttu-id="d8949-162">Необходимо будет обновить Visual Studio 2013 для получения этой поддержки.</span><span class="sxs-lookup"><span data-stu-id="d8949-162">You will need to update Visual Studio 2013 to get this support.</span></span> 

### <a name="type-renames"></a><span data-ttu-id="d8949-163">Переименование типа</span><span class="sxs-lookup"><span data-stu-id="d8949-163">Type Renames</span></span>

<span data-ttu-id="d8949-164">Некоторые типы, используемые для расширения маршрутизации атрибут переименовываются в окончательной версии 5.1.</span><span class="sxs-lookup"><span data-stu-id="d8949-164">Some of the types used for attribute routing extensibility are renamed in 5.1 RTM.</span></span>

| <span data-ttu-id="d8949-165">**Старое имя типа (5.1 версия-Кандидат)**</span><span class="sxs-lookup"><span data-stu-id="d8949-165">**Old Type Name (5.1 RC)**</span></span> | <span data-ttu-id="d8949-166">**Имя нового типа (5.1 RTM)**</span><span class="sxs-lookup"><span data-stu-id="d8949-166">**New Type Name (5.1 RTM)**</span></span> |
| --- | --- |
| <span data-ttu-id="d8949-167">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="d8949-167">IDirectRouteProvider</span></span> | <span data-ttu-id="d8949-168">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="d8949-168">IDirectRouteFactory</span></span> |
| <span data-ttu-id="d8949-169">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="d8949-169">RouteProviderAttribute</span></span> | <span data-ttu-id="d8949-170">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="d8949-170">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="d8949-171">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="d8949-171">DirectRouteProviderContext</span></span> | <span data-ttu-id="d8949-172">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="d8949-172">DirectRouteFactoryContext</span></span> |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="d8949-173">Исправления ошибок</span><span class="sxs-lookup"><span data-stu-id="d8949-173">Bug Fixes</span></span>

<span data-ttu-id="d8949-174">Этот выпуск также включает несколько исправлений ошибок.</span><span class="sxs-lookup"><span data-stu-id="d8949-174">This release also includes several bug fixes.</span></span> <span data-ttu-id="d8949-175">Можно найти полный список здесь:</span><span class="sxs-lookup"><span data-stu-id="d8949-175">You can find the complete list here:</span></span>

- [<span data-ttu-id="d8949-176">5.1.0 пакет</span><span class="sxs-lookup"><span data-stu-id="d8949-176">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="d8949-177">5.1.1 пакет</span><span class="sxs-lookup"><span data-stu-id="d8949-177">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<span data-ttu-id="d8949-178">5.1.2 пакет содержит обновления IntelliSense, но не исправления ошибок.</span><span class="sxs-lookup"><span data-stu-id="d8949-178">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
