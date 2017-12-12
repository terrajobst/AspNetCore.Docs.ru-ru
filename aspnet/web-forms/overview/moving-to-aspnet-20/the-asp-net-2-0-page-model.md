---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: "Модель ASP.NET 2.0 страницы | Документы Microsoft"
author: microsoft
description: "В ASP.NET 1.x, разработчики были выбирать между моделью встроенный код и код модели кода. Кода могут быть реализованы с помощью либо attr Src..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: e008f197cf08bec81c560018f2d42306598f9e6d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="the-aspnet-20-page-model"></a><span data-ttu-id="fb6e3-104">Модель 2.0 страницы ASP.NET</span><span class="sxs-lookup"><span data-stu-id="fb6e3-104">The ASP.NET 2.0 Page Model</span></span>
====================
<span data-ttu-id="fb6e3-105">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="fb6e3-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="fb6e3-106">В ASP.NET 1.x, разработчики были выбирать между моделью встроенный код и код модели кода.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-106">In ASP.NET 1.x, developers had a choice between an inline code model and a code-behind code model.</span></span> <span data-ttu-id="fb6e3-107">Кода может быть реализован с помощью атрибута Src или атрибут CodeBehind @Page директивы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-107">Code-behind could be implemented using either the Src attribute or the CodeBehind attribute of the @Page directive.</span></span> <span data-ttu-id="fb6e3-108">В ASP.NET 2.0 разработчики по-прежнему есть выбор между встроенного кода и кода, но были внесены значительные улучшения модели кода.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-108">In ASP.NET 2.0, developers still have a choice between inline code and code-behind, but there have been significant enhancements to the code-behind model.</span></span>


<span data-ttu-id="fb6e3-109">В ASP.NET 1.x, разработчики были выбирать между моделью встроенный код и код модели кода.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-109">In ASP.NET 1.x, developers had a choice between an inline code model and a code-behind code model.</span></span> <span data-ttu-id="fb6e3-110">Кода может быть реализован с помощью атрибута Src или атрибут CodeBehind @Page директивы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-110">Code-behind could be implemented using either the Src attribute or the CodeBehind attribute of the @Page directive.</span></span> <span data-ttu-id="fb6e3-111">В ASP.NET 2.0 разработчики по-прежнему есть выбор между встроенного кода и кода, но были внесены значительные улучшения модели кода.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-111">In ASP.NET 2.0, developers still have a choice between inline code and code-behind, but there have been significant enhancements to the code-behind model.</span></span>

## <a name="improvements-in-the-code-behind-model"></a><span data-ttu-id="fb6e3-112">Усовершенствования в модели кода</span><span class="sxs-lookup"><span data-stu-id="fb6e3-112">Improvements in the Code-Behind Model</span></span>

<span data-ttu-id="fb6e3-113">Чтобы полностью понять изменения в модели кода в ASP.NET 2.0, все усилия, чтобы быстро просмотреть модели, как он существовал в ASP.NET 1.x.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-113">In order to fully understand the changes in the code-behind model in ASP.NET 2.0, its best to quickly review the model as it existed in ASP.NET 1.x.</span></span>

## <a name="the-code-behind-model-in-aspnet-1x"></a><span data-ttu-id="fb6e3-114">Модель фонового кода в ASP.NET 1.x</span><span class="sxs-lookup"><span data-stu-id="fb6e3-114">The Code-Behind Model in ASP.NET 1.x</span></span>

<span data-ttu-id="fb6e3-115">В ASP.NET 1.x, модель фонового кода включал ASPX-файл (Webform) и файл кода, содержащий программный код.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-115">In ASP.NET 1.x, the code-behind model consisted of an ASPX file (the Webform) and a code-behind file containing programming code.</span></span> <span data-ttu-id="fb6e3-116">Оба файла были подключены с помощью @Page директив в ASPX-файле.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-116">The two files were connected using the @Page directive in the ASPX file.</span></span> <span data-ttu-id="fb6e3-117">Каждый элемент управления на странице ASPX имели соответствующее объявление в файле кода как переменная экземпляра.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-117">Each control on the ASPX page had a corresponding declaration in the code-behind file as an instance variable.</span></span> <span data-ttu-id="fb6e3-118">Файл кода программной также содержится код для привязки событий и созданный код, необходимый для конструктора Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-118">The code-behind file also contained code for event binding and generated code necessary for the Visual Studio designer.</span></span> <span data-ttu-id="fb6e3-119">Эта модель работала, довольно хорошо, но поскольку каждый элемент ASPX-страницы ASP.NET требуется соответствующий код в файл кода, была не true разделение кода и содержимое.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-119">This model worked fairly well, but because every ASP.NET element in the ASPX page required corresponding code in the code-behind file, there was no true separation of code and content.</span></span> <span data-ttu-id="fb6e3-120">Например если конструктор добавлен новый серверный элемент управления ASPX-файл вне Visual Studio IDE, приложение будет нарушена из-за отсутствия объявления для этого элемента управления в файле кода.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-120">For example, if a designer added a new server control to an ASPX file outside of the Visual Studio IDE, the application would break due to the absence of a declaration for that control in the code-behind file.</span></span>

## <a name="the-code-behind-model-in-aspnet-20"></a><span data-ttu-id="fb6e3-121">Модель фонового кода ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="fb6e3-121">The Code-Behind Model in ASP.NET 2.0</span></span>

<span data-ttu-id="fb6e3-122">ASP.NET 2.0 значительно улучшает работу этой модели.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-122">ASP.NET 2.0 greatly improves upon this model.</span></span> <span data-ttu-id="fb6e3-123">В ASP.NET 2.0 кода реализуется с использованием нового *разделяемые классы* в ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-123">In ASP.NET 2.0, code-behind is implemented using the new *partial classes* provided in ASP.NET 2.0.</span></span> <span data-ttu-id="fb6e3-124">Класс кода программной части в ASP.NET 2.0 — definied как разделяемый класс, это означает, что он содержит только часть определения класса.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-124">The code-behind class in ASP.NET 2.0 is definied as a partial class meaning that it contains only part of the class definition.</span></span> <span data-ttu-id="fb6e3-125">Оставшаяся часть определения класса динамически созданные с помощью страницы ASPX, во время выполнения или при предкомпилированного веб-сайта ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-125">The remaining part of the class definition is dynamically generated by ASP.NET 2.0 using the ASPX page at runtime or when the Web site is precompiled.</span></span> <span data-ttu-id="fb6e3-126">Связь между файл кода программной части и страницы ASPX по-прежнему устанавливается с помощью директивы @ Page.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-126">The link between the code-behind file and the ASPX page is still established using the @ Page directive.</span></span> <span data-ttu-id="fb6e3-127">Тем не менее а не атрибут CodeBehind или Src ASP.NET 2.0 теперь использует атрибута CodeFile.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-127">However, instead of a CodeBehind or Src attribute, ASP.NET 2.0 now uses the CodeFile attribute.</span></span> <span data-ttu-id="fb6e3-128">Атрибут Inherits также используется для указания имени класса для страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-128">The Inherits attribute is also used to specify the class name for the page.</span></span>

<span data-ttu-id="fb6e3-129">Типичная директива @ Page может выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="fb6e3-129">A typical @ Page directive might look like this:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

<span data-ttu-id="fb6e3-130">Определение обычной класса в файле кода ASP.NET 2.0 может выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="fb6e3-130">A typical class definition in an ASP.NET 2.0 code-behind file might look like this:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="fb6e3-131">C# и Visual Basic — только управляемые языки, которые поддерживают разделяемые классы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-131">C# and Visual Basic are the only managed languages that currently support partial classes.</span></span> <span data-ttu-id="fb6e3-132">Таким образом разработчики, использующие J# нельзя будет использовать модель фонового кода ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-132">Therefore, developers using J# will not be able to use the code-behind model in ASP.NET 2.0.</span></span>


<span data-ttu-id="fb6e3-133">Новая модель расширяет возможности модели кода, так как разработчики получают файлы кода, которые содержат код, который они создали.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-133">The new model enhances the code-behind model because developers will now have code files that contain only the code that they have created.</span></span> <span data-ttu-id="fb6e3-134">Он также предоставляет true разделение кода и содержимое из-за объявления переменных не экземпляр в файле кода.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-134">It also provides for a true separation of code and content because there are no instance variable declarations in the code-behind file.</span></span>

> [!NOTE]
> <span data-ttu-id="fb6e3-135">Так как разделяемый класс для страницы ASPX, где происходит событие привязки разработчики Visual Basic и добиться увеличения спад производительности с ключевым словом дескрипторов в коде для привязки событий.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-135">Because the partial class for the ASPX page is where event binding takes place, Visual Basic developers can realize a slight performance increase by using the Handles keyword in code-behind to bind events.</span></span> <span data-ttu-id="fb6e3-136">C# имеется нет ключевого слова-эквивалент.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-136">C# has no equivalent keyword.</span></span>


## <a name="new--page-directive-attributes"></a><span data-ttu-id="fb6e3-137">Новые атрибуты директива @ Page</span><span class="sxs-lookup"><span data-stu-id="fb6e3-137">New @ Page Directive Attributes</span></span>

<span data-ttu-id="fb6e3-138">Директива @ Page ASP.NET 2.0 добавляет много новых атрибутов.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-138">ASP.NET 2.0 adds many new attributes to the @ Page directive.</span></span> <span data-ttu-id="fb6e3-139">В ASP.NET 2.0 появились следующие атрибуты.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-139">The following attributes are new in ASP.NET 2.0.</span></span>

## <a name="async"></a><span data-ttu-id="fb6e3-140">Async</span><span class="sxs-lookup"><span data-stu-id="fb6e3-140">Async</span></span>

<span data-ttu-id="fb6e3-141">Асинхронный атрибут позволяет настроить страница может выполняться асинхронно.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-141">The Async attribute allows you to configure page to be executed asynchronously.</span></span> <span data-ttu-id="fb6e3-142">Также охватывает асинхронные страницы позже в этом модуле.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-142">Well cover asynchronous pages later in this module.</span></span>

## <a name="asynctimeout"></a><span data-ttu-id="fb6e3-143">AsyncTimeout</span><span class="sxs-lookup"><span data-stu-id="fb6e3-143">AsyncTimeout</span></span>

<span data-ttu-id="fb6e3-144">Указанное время ожидания для асинхронных страниц.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-144">Specified the timeout for asynchronous pages.</span></span> <span data-ttu-id="fb6e3-145">Значение по умолчанию составляет 45 секунд.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-145">The default is 45 seconds.</span></span>

## <a name="codefile"></a><span data-ttu-id="fb6e3-146">CodeFile</span><span class="sxs-lookup"><span data-stu-id="fb6e3-146">CodeFile</span></span>

<span data-ttu-id="fb6e3-147">Атрибута CodeFile является заменой атрибут CodeBehind в Visual Studio 2002/2003.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-147">The CodeFile attribute is the replacement for the CodeBehind attribute in Visual Studio 2002/2003.</span></span>

### <a name="codefilebaseclass"></a><span data-ttu-id="fb6e3-148">CodeFileBaseClass</span><span class="sxs-lookup"><span data-stu-id="fb6e3-148">CodeFileBaseClass</span></span>

<span data-ttu-id="fb6e3-149">Атрибут CodeFileBaseClass используется в случаях, где требуется несколько страниц для наследования от одного базового класса.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-149">The CodeFileBaseClass attribute is used in cases where you want multiple pages to derive from a single base class.</span></span> <span data-ttu-id="fb6e3-150">Из-за реализации разделяемые классы в ASP.NET, без этого атрибута базового класса, который использует общие общих полей для ссылки на элементы управления, объявленные в страницу ASPX будет работать неправильно из-за ASP. Модуль компиляции сетями автоматически создаст новые члены, основан на элементах управления на странице.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-150">Because of the implementation of partial classes in ASP.NET, without this attribute, a base class that uses shared common fields to reference controls declared in an ASPX page would not work properly because ASP.NETs compilation engine will automatically create new members based on controls in the page.</span></span> <span data-ttu-id="fb6e3-151">Таким образом, если требуется общий базовый класс для двух или нескольких страниц ASP.NET, необходимо определить в атрибуте CodeFileBaseClass задать базовый класс и затем создайте каждой страницы класс, производный от этого базового класса.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-151">Therefore, if you want a common base class for two or more pages in ASP.NET, you will need to define specify your base class in the CodeFileBaseClass attribute and then derive each pages class from that base class.</span></span> <span data-ttu-id="fb6e3-152">Атрибута CodeFile также является обязательным при использовании этого атрибута.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-152">The CodeFile attribute is also required when this attribute is used.</span></span>

## <a name="compilationmode"></a><span data-ttu-id="fb6e3-153">compilationMode</span><span class="sxs-lookup"><span data-stu-id="fb6e3-153">CompilationMode</span></span>

<span data-ttu-id="fb6e3-154">Этот атрибут можно задать свойство CompilationMode страницы ASPX.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-154">This attribute allows you to set the CompilationMode property of the ASPX page.</span></span> <span data-ttu-id="fb6e3-155">Свойство CompilationMode является перечислением, содержащий значения **всегда**, **автоматически**, и **никогда**.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-155">The CompilationMode property is an enumeration containing the values **Always**, **Auto**, and **Never**.</span></span> <span data-ttu-id="fb6e3-156">Значение по умолчанию — **всегда**.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-156">The default is **Always**.</span></span> <span data-ttu-id="fb6e3-157">**Автоматически** параметр будет препятствовать динамически по возможности компиляции страницы ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-157">The **Auto** setting will prevent ASP.NET from dynamically compiling the page if possible.</span></span> <span data-ttu-id="fb6e3-158">За исключением страниц от динамической компиляции повышает производительность.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-158">Excluding pages from dynamic compilation increases performance.</span></span> <span data-ttu-id="fb6e3-159">Тем не менее если страница, на которой исключается содержит этот код, который должен быть скомпилирован, выдается ошибка при просмотре страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-159">However, if a page that is excluded contains that code that must be compiled, an error will be thrown when the page is browsed.</span></span>

## <a name="enableeventvalidation"></a><span data-ttu-id="fb6e3-160">EnableEventValidation</span><span class="sxs-lookup"><span data-stu-id="fb6e3-160">EnableEventValidation</span></span>

<span data-ttu-id="fb6e3-161">Этот атрибут указывает, проверяется ли события обратной передачи и обратного вызова.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-161">This attribute specifies whether or not postback and callback events are validated.</span></span> <span data-ttu-id="fb6e3-162">Если эта функция включена, аргументы для обратной передачи или обратного вызова события проверяются, чтобы убедиться, что они были получены из серверного элемента управления, их первоначального формирования.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-162">When this is enabled, arguments to postback or callback events are checked to ensure that they originated from the server control that originally rendered them.</span></span>

## <a name="enabletheming"></a><span data-ttu-id="fb6e3-163">EnableTheming</span><span class="sxs-lookup"><span data-stu-id="fb6e3-163">EnableTheming</span></span>

<span data-ttu-id="fb6e3-164">Этот атрибут указывает, используются ли темы ASP.NET на странице.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-164">This attribute specifies whether or not ASP.NET themes are used on a page.</span></span> <span data-ttu-id="fb6e3-165">Значение по умолчанию — **false**.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-165">The default is **false**.</span></span> <span data-ttu-id="fb6e3-166">Темы ASP.NET охваченных [10 модуль](profiles-themes-and-web-parts.md).</span><span class="sxs-lookup"><span data-stu-id="fb6e3-166">ASP.NET themes are covered in [Module 10](profiles-themes-and-web-parts.md).</span></span>

## <a name="linepragmas"></a><span data-ttu-id="fb6e3-167">LinePragmas</span><span class="sxs-lookup"><span data-stu-id="fb6e3-167">LinePragmas</span></span>

<span data-ttu-id="fb6e3-168">Этот атрибут указывает, следует ли добавить директивы pragma во время компиляции.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-168">This attribute specifies whether line pragmas should be added during compilation.</span></span> <span data-ttu-id="fb6e3-169">Директивы pragma, параметры, используемые в отладчиках для выделения определенных разделов кода.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-169">Line pragmas are options used by debuggers to mark specific sections of code.</span></span>

## <a name="maintainscrollpositiononpostback"></a><span data-ttu-id="fb6e3-170">MaintainScrollPositionOnPostback</span><span class="sxs-lookup"><span data-stu-id="fb6e3-170">MaintainScrollPositionOnPostback</span></span>

<span data-ttu-id="fb6e3-171">Этот атрибут указывает, вставляется ли JavaScript в страницы для обеспечения позиции прокрутки между обратными передачами.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-171">This attribute specifies whether or not JavaScript is injected into the page in order to maintain scroll position between postbacks.</span></span> <span data-ttu-id="fb6e3-172">Этот атрибут является **false** по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-172">This attribute is **false** by default.</span></span>

<span data-ttu-id="fb6e3-173">Если этот атрибут имеет **true**, ASP.NET добавит &lt;сценарий&gt; блок при обратной передаче, выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="fb6e3-173">When this attribute is **true**, ASP.NET will add a &lt;script&gt; block on postback that looks like this:</span></span>

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

<span data-ttu-id="fb6e3-174">Обратите внимание, что для этого блока сценария src WebResource.axd.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-174">Note that the src for this script block is WebResource.axd.</span></span> <span data-ttu-id="fb6e3-175">Этот ресурс не физический путь.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-175">This resource is not a physical path.</span></span> <span data-ttu-id="fb6e3-176">Получив этот сценарий, ASP.NET динамически создает скрипт.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-176">When this script is requested, ASP.NET dynamically builds the script.</span></span>

### <a name="masterpagefile"></a><span data-ttu-id="fb6e3-177">MasterPageFile</span><span class="sxs-lookup"><span data-stu-id="fb6e3-177">MasterPageFile</span></span>

<span data-ttu-id="fb6e3-178">Этот атрибут задает файл главной страницы для текущей страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-178">This attribute specifies the master page file for the current page.</span></span> <span data-ttu-id="fb6e3-179">Путь может быть относительным или абсолютным.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-179">The path can be relative or absolute.</span></span> <span data-ttu-id="fb6e3-180">Главные страницы охваченных [4 модуля](master-pages.md).</span><span class="sxs-lookup"><span data-stu-id="fb6e3-180">Master pages are covered in [Module 4](master-pages.md).</span></span>

## <a name="stylesheettheme"></a><span data-ttu-id="fb6e3-181">StyleSheetTheme</span><span class="sxs-lookup"><span data-stu-id="fb6e3-181">StyleSheetTheme</span></span>

<span data-ttu-id="fb6e3-182">Этот атрибут позволяет переопределять свойства внешнего вида пользовательского интерфейса, определенные темы ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-182">This attribute allows you to override user-interface appearance properties defined by an ASP.NET 2.0 theme.</span></span> <span data-ttu-id="fb6e3-183">Темы рассматриваются в [10 модуль](profiles-themes-and-web-parts.md).</span><span class="sxs-lookup"><span data-stu-id="fb6e3-183">Themes are covered in [Module 10](profiles-themes-and-web-parts.md).</span></span>

## <a name="theme"></a><span data-ttu-id="fb6e3-184">Тема</span><span class="sxs-lookup"><span data-stu-id="fb6e3-184">Theme</span></span>

<span data-ttu-id="fb6e3-185">Определяет тему для страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-185">Specifies the theme for the page.</span></span> <span data-ttu-id="fb6e3-186">Если значение не указано для атрибута StyleSheetTheme, атрибут Theme переопределяет все стили, примененные к элементу управления на странице.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-186">If a value is not specified for the StyleSheetTheme attribute, the Theme attribute overrides all styles applied to controls on the page.</span></span>

## <a name="title"></a><span data-ttu-id="fb6e3-187">Заголовок</span><span class="sxs-lookup"><span data-stu-id="fb6e3-187">Title</span></span>

<span data-ttu-id="fb6e3-188">Задание заголовка для страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-188">Sets the title for the page.</span></span> <span data-ttu-id="fb6e3-189">Указанное здесь значение будет отображаться в &lt;заголовок&gt; элемент отображаемой страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-189">The value specified here will appear in the &lt;title&gt; element of the rendered page.</span></span>

### <a name="viewstateencryptionmode"></a><span data-ttu-id="fb6e3-190">ViewStateEncryptionMode</span><span class="sxs-lookup"><span data-stu-id="fb6e3-190">ViewStateEncryptionMode</span></span>

<span data-ttu-id="fb6e3-191">Задает значение перечисления ViewStateEncryptionMode.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-191">Sets the value for the ViewStateEncryptionMode enumeration.</span></span> <span data-ttu-id="fb6e3-192">Доступные значения: **всегда**, **автоматически**, и **никогда**.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-192">The available values are **Always**, **Auto**, and **Never**.</span></span> <span data-ttu-id="fb6e3-193">Значение по умолчанию — **автоматически**. Если этот атрибут имеет значение в значение **автоматически**, шифруется viewstate элемента управления запрашивает его путем вызова **RegisterRequiresViewStateEncryption** метод.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-193">The default value is **Auto**. When this attribute is set to a value of **Auto**, viewstate is encrypted is a control requests it by calling the **RegisterRequiresViewStateEncryption** method.</span></span>

## <a name="setting-public-property-values-via-the--page-directive"></a><span data-ttu-id="fb6e3-194">Задание значения открытых свойств через директива @ Page</span><span class="sxs-lookup"><span data-stu-id="fb6e3-194">Setting Public Property Values via the @ Page Directive</span></span>

<span data-ttu-id="fb6e3-195">Другой новой функции директива @ Page в ASP.NET 2.0 — возможность задать начальное значение общих свойств базового класса.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-195">Another new capability of the @ Page directive in ASP.NET 2.0 is the ability to set the initial value of public properties of a base class.</span></span> <span data-ttu-id="fb6e3-196">Предположим, что имеется открытое свойство вызвать, например **SomeText** в базового класса и d, как она будет инициализирована **Hello** при загрузке страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-196">Suppose, for example, that you have a public property called **SomeText** in your base class and you d like it to be initialized to **Hello** when a page is loaded.</span></span> <span data-ttu-id="fb6e3-197">Это можно сделать, просто задав значение в директиве @ Page следующим образом:</span><span class="sxs-lookup"><span data-stu-id="fb6e3-197">You can accomplish this by simply setting the value in the @ Page directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

<span data-ttu-id="fb6e3-198">**SomeText** атрибута директивы @ Page задает начальное значение SomeText свойства базового класса, чтобы *Hello!*.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-198">The **SomeText** attribute of the @ Page directive sets the initial value of the SomeText property in the base class to *Hello!*.</span></span> <span data-ttu-id="fb6e3-199">Видео ниже приведен пример начальное значение открытого свойства в базовом классе с помощью директивы @ Page.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-199">The video below is a walkthrough of setting the initial value of a public property in a base class using the @ Page directive.</span></span>


![](the-asp-net-2-0-page-model/_static/image1.png)


[<span data-ttu-id="fb6e3-200">Откройте весь экран</span><span class="sxs-lookup"><span data-stu-id="fb6e3-200">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/setprop1.wmv)


## <a name="new-public-properties-of-the-page-class"></a><span data-ttu-id="fb6e3-201">Новые свойства открытого класса страницы</span><span class="sxs-lookup"><span data-stu-id="fb6e3-201">New Public Properties of the Page Class</span></span>

<span data-ttu-id="fb6e3-202">В ASP.NET 2.0 появились следующие открытые свойства.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-202">The following public properties are new in ASP.NET 2.0.</span></span>

## <a name="apprelativetemplatesourcedirectory"></a><span data-ttu-id="fb6e3-203">AppRelativeTemplateSourceDirectory</span><span class="sxs-lookup"><span data-stu-id="fb6e3-203">AppRelativeTemplateSourceDirectory</span></span>

<span data-ttu-id="fb6e3-204">Возвращает путь относительно приложения для страницы или элемента управления.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-204">Returns the application-relative path to the page or control.</span></span> <span data-ttu-id="fb6e3-205">Например, для страницы, расположенный в http://app/folder/page.aspx, свойство возвращает ~ / folder /.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-205">For example, for a page located at http://app/folder/page.aspx, the property returns ~/folder/.</span></span>

## <a name="apprelativevirtualpath"></a><span data-ttu-id="fb6e3-206">AppRelativeVirtualPath</span><span class="sxs-lookup"><span data-stu-id="fb6e3-206">AppRelativeVirtualPath</span></span>

<span data-ttu-id="fb6e3-207">Возвращает относительный виртуальный путь к каталогу страницы или элемента управления.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-207">Returns the relative virtual directory path to the page or control.</span></span> <span data-ttu-id="fb6e3-208">Например для страницы, расположенный в http://app/folder/page.aspx, свойство возвращает ~ / folder/page.aspx.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-208">For example for a page located at http://app/folder/page.aspx, the property returns ~/folder/page.aspx.</span></span>

## <a name="asynctimeout"></a><span data-ttu-id="fb6e3-209">AsyncTimeout</span><span class="sxs-lookup"><span data-stu-id="fb6e3-209">AsyncTimeout</span></span>

<span data-ttu-id="fb6e3-210">Возвращает или задает время ожидания, используемое для обработки асинхронной страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-210">Gets or sets the timeout used for asynchronous page handling.</span></span> <span data-ttu-id="fb6e3-211">(Асинхронные страницы будет рассматриваться далее в этом модуле.)</span><span class="sxs-lookup"><span data-stu-id="fb6e3-211">(Asynchronous pages will be covered later in this module.)</span></span>

## <a name="clientquerystring"></a><span data-ttu-id="fb6e3-212">ClientQueryString</span><span class="sxs-lookup"><span data-stu-id="fb6e3-212">ClientQueryString</span></span>

<span data-ttu-id="fb6e3-213">Свойство только для чтения, которое возвращает часть строки запроса запрошенного URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-213">A read-only property that returns the query string portion of the requested URL.</span></span> <span data-ttu-id="fb6e3-214">Это значение является URL-кодированием.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-214">This value is URL encoded.</span></span> <span data-ttu-id="fb6e3-215">Метод UrlDecode класса HttpServerUtility для декодирования.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-215">You can use the UrlDecode method of the HttpServerUtility class to decode it.</span></span>

## <a name="clientscript"></a><span data-ttu-id="fb6e3-216">ClientScript</span><span class="sxs-lookup"><span data-stu-id="fb6e3-216">ClientScript</span></span>

<span data-ttu-id="fb6e3-217">Это свойство возвращает объект ClientScriptManager, который может использоваться для управления ASP.NETs вывод клиентского скрипта.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-217">This property returns a ClientScriptManager object that can be used to manage ASP.NETs emission of client-side script.</span></span> <span data-ttu-id="fb6e3-218">(Класс ClientScriptManager рассматривается далее в этом модуле.)</span><span class="sxs-lookup"><span data-stu-id="fb6e3-218">(The ClientScriptManager class is covered later in this module.)</span></span>

## <a name="enableeventvalidation"></a><span data-ttu-id="fb6e3-219">EnableEventValidation</span><span class="sxs-lookup"><span data-stu-id="fb6e3-219">EnableEventValidation</span></span>

<span data-ttu-id="fb6e3-220">Это свойство определяет, включена ли проверка событий для события обратной передачи и обратного вызова.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-220">This property controls whether or not event validation is enabled for postback and callback events.</span></span> <span data-ttu-id="fb6e3-221">При включении, чтобы убедиться, что они были получены из серверного элемента управления, их первоначального формирования проверяются аргументы для обратной передачи или обратного вызова события.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-221">When enabled, arguments to postback or callback events are verified to ensure that they originated from the server control that originally rendered them.</span></span>

## <a name="enabletheming"></a><span data-ttu-id="fb6e3-222">EnableTheming</span><span class="sxs-lookup"><span data-stu-id="fb6e3-222">EnableTheming</span></span>

<span data-ttu-id="fb6e3-223">Это свойство Возвращает или задает логическое значение, указывающее, применяется ли темы ASP.NET 2.0 на страницу.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-223">This property gets or sets a Boolean that specifies whether or not an ASP.NET 2.0 theme applies to the page.</span></span>

## <a name="form"></a><span data-ttu-id="fb6e3-224">Form</span><span class="sxs-lookup"><span data-stu-id="fb6e3-224">Form</span></span>

<span data-ttu-id="fb6e3-225">Это свойство возвращает HTML-форму на странице ASPX, как объект HtmlForm.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-225">This property returns the HTML form on the ASPX page as an HtmlForm object.</span></span>

## <a name="header"></a><span data-ttu-id="fb6e3-226">Header</span><span class="sxs-lookup"><span data-stu-id="fb6e3-226">Header</span></span>

<span data-ttu-id="fb6e3-227">Это свойство возвращает ссылку на объект HtmlHead, содержащий верхний колонтитул страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-227">This property returns a reference to an HtmlHead object that contains the page header.</span></span> <span data-ttu-id="fb6e3-228">Возвращаемый объект HtmlHead можно использовать для получения или задания таблицы стилей, мета-теги, и т. д.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-228">You can use the returned HtmlHead object to get/set style sheets, Meta tags, etc.</span></span>

## <a name="idseparator"></a><span data-ttu-id="fb6e3-229">IdSeparator</span><span class="sxs-lookup"><span data-stu-id="fb6e3-229">IdSeparator</span></span>

<span data-ttu-id="fb6e3-230">Это свойство только для чтения получает символ, используемый для разделения идентификаторов элементов управления, когда ASP.NET создает уникальный идентификатор для элементов управления на странице.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-230">This read-only property gets the character that is used to separate control identifiers when ASP.NET is building a unique ID for controls on a page.</span></span> <span data-ttu-id="fb6e3-231">Он не предназначен для использования непосредственно в вашем коде.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-231">It is not intended to be used directly from your code.</span></span>

## <a name="isasync"></a><span data-ttu-id="fb6e3-232">IsAsync</span><span class="sxs-lookup"><span data-stu-id="fb6e3-232">IsAsync</span></span>

<span data-ttu-id="fb6e3-233">Это свойство позволяет асинхронные страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-233">This property allows for asynchronous pages.</span></span> <span data-ttu-id="fb6e3-234">Асинхронные страницы рассматриваются далее в этом модуле.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-234">Asynchronous pages are discussed later in this module.</span></span>

## <a name="iscallback"></a><span data-ttu-id="fb6e3-235">IsCallback</span><span class="sxs-lookup"><span data-stu-id="fb6e3-235">IsCallback</span></span>

<span data-ttu-id="fb6e3-236">Это свойство только для чтения возвращает **true** Если страница является результат обратного вызова.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-236">This read-only property returns **true** if the page is the result of a call back.</span></span> <span data-ttu-id="fb6e3-237">Обратных звонков, рассматриваются далее в этом модуле.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-237">Call backs are discussed later in this module.</span></span>

## <a name="iscrosspagepostback"></a><span data-ttu-id="fb6e3-238">IsCrossPagePostBack</span><span class="sxs-lookup"><span data-stu-id="fb6e3-238">IsCrossPagePostBack</span></span>

<span data-ttu-id="fb6e3-239">Это свойство только для чтения возвращает **true** Если страница является частью обратной передаче между страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-239">This read-only property returns **true** if the page is part of a cross-page postback.</span></span> <span data-ttu-id="fb6e3-240">Обратную передачу страницы между рассматриваются далее в этом модуле.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-240">Cross-page postbacks are covered later in this module.</span></span>

## <a name="items"></a><span data-ttu-id="fb6e3-241">Элементы</span><span class="sxs-lookup"><span data-stu-id="fb6e3-241">Items</span></span>

<span data-ttu-id="fb6e3-242">Возвращает ссылку на экземпляр IDictionary, содержащий все объекты, хранящиеся в контексте страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-242">Returns a reference to an IDictionary instance that contains all objects stored in the pages context.</span></span> <span data-ttu-id="fb6e3-243">Можно добавить элементы к этому объекту IDictionary и они будут доступны для вас в течение времени существования контекста.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-243">You can add items to this IDictionary object and they will be available to you throughout the lifetime of the context.</span></span>

## <a name="maintainscrollpositiononpostback"></a><span data-ttu-id="fb6e3-244">MaintainScrollPositionOnPostBack</span><span class="sxs-lookup"><span data-stu-id="fb6e3-244">MaintainScrollPositionOnPostBack</span></span>

<span data-ttu-id="fb6e3-245">Это свойство управляет ли ASP.NET создает JavaScript, который обслуживается прокручиваются страницы позицию в браузере после обратной передаче.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-245">This property controls whether or not ASP.NET emits JavaScript that maintains the pages scroll position in the browser after a postback occurs.</span></span> <span data-ttu-id="fb6e3-246">(Подробные сведения этого свойства были описаны ранее в этом модуле.)</span><span class="sxs-lookup"><span data-stu-id="fb6e3-246">(Details of this property were discussed earlier in this module.)</span></span>

## <a name="master"></a><span data-ttu-id="fb6e3-247">Образец</span><span class="sxs-lookup"><span data-stu-id="fb6e3-247">Master</span></span>

<span data-ttu-id="fb6e3-248">Это свойство только для чтения возвращает ссылку на экземпляр MasterPage к этой странице, к которым был применен главной страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-248">This read-only property returns a reference to the MasterPage instance for a page to which a master page has been applied.</span></span>

## <a name="masterpagefile"></a><span data-ttu-id="fb6e3-249">MasterPageFile</span><span class="sxs-lookup"><span data-stu-id="fb6e3-249">MasterPageFile</span></span>

<span data-ttu-id="fb6e3-250">Возвращает или задает имя файла главной страницы для страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-250">Gets or sets the master page filename for the page.</span></span> <span data-ttu-id="fb6e3-251">Это свойство можно задать только в методе PreInit.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-251">This property can only be set in the PreInit method.</span></span>

## <a name="maxpagestatefieldlength"></a><span data-ttu-id="fb6e3-252">MaxPageStateFieldLength</span><span class="sxs-lookup"><span data-stu-id="fb6e3-252">MaxPageStateFieldLength</span></span>

<span data-ttu-id="fb6e3-253">Это свойство Возвращает или задает максимальную длину для состояния страницы в байтах.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-253">This property gets or sets the maximum length for the pages state in bytes.</span></span> <span data-ttu-id="fb6e3-254">Если свойство задано на положительное число, состояние представления страницы будет выделить несколько скрытых полей, чтобы он не превышает число байтов, указанного в.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-254">If the property is set to a positive number, the pages view state will be broken up into multiple hidden fields so that it doesnt exceed the number of bytes specified.</span></span> <span data-ttu-id="fb6e3-255">Если свойство имеет отрицательное значение, состояние представления будет не разбивается на фрагменты.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-255">If the property is a negative number, the view state will not be broken into chunks.</span></span>

## <a name="pageadapter"></a><span data-ttu-id="fb6e3-256">PageAdapter</span><span class="sxs-lookup"><span data-stu-id="fb6e3-256">PageAdapter</span></span>

<span data-ttu-id="fb6e3-257">Возвращает ссылку на объект PageAdapter, изменяет страницы для запрашивающего браузера.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-257">Returns a reference to the PageAdapter object that modifies the page for the requesting browser.</span></span>

## <a name="previouspage"></a><span data-ttu-id="fb6e3-258">PreviousPage</span><span class="sxs-lookup"><span data-stu-id="fb6e3-258">PreviousPage</span></span>

<span data-ttu-id="fb6e3-259">Возвращает ссылку на предыдущую страницу в случаях Server.Transfer или обратной передаче между страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-259">Returns a reference to the previous page in cases of a Server.Transfer or a cross-page postback.</span></span>

## <a name="skinid"></a><span data-ttu-id="fb6e3-260">SkinID</span><span class="sxs-lookup"><span data-stu-id="fb6e3-260">SkinID</span></span>

<span data-ttu-id="fb6e3-261">Указывает ASP.NET 2.0 обложку, применяемую к странице.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-261">Specifies the ASP.NET 2.0 skin to apply to the page.</span></span>

## <a name="stylesheettheme"></a><span data-ttu-id="fb6e3-262">StyleSheetTheme</span><span class="sxs-lookup"><span data-stu-id="fb6e3-262">StyleSheetTheme</span></span>

<span data-ttu-id="fb6e3-263">Это свойство Возвращает или задает таблицу стилей, примененной к странице.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-263">This property gets or sets the style sheet that is applied to a page.</span></span>

## <a name="templatecontrol"></a><span data-ttu-id="fb6e3-264">TemplateControl</span><span class="sxs-lookup"><span data-stu-id="fb6e3-264">TemplateControl</span></span>

<span data-ttu-id="fb6e3-265">Возвращает ссылку на содержащего элемента управления для страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-265">Returns a reference to the containing control for the page.</span></span>

## <a name="theme"></a><span data-ttu-id="fb6e3-266">Тема</span><span class="sxs-lookup"><span data-stu-id="fb6e3-266">Theme</span></span>

<span data-ttu-id="fb6e3-267">Возвращает или задает имя темы ASP.NET 2.0, применяемой к странице.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-267">Gets or sets the name of the ASP.NET 2.0 theme applied to the page.</span></span> <span data-ttu-id="fb6e3-268">Это значение должно быть установлено до PreInit метод.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-268">This value must be set prior to the PreInit method.</span></span>

## <a name="title"></a><span data-ttu-id="fb6e3-269">Заголовок</span><span class="sxs-lookup"><span data-stu-id="fb6e3-269">Title</span></span>

<span data-ttu-id="fb6e3-270">Это свойство Возвращает или задает заголовок для страницы, полученный из заголовка страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-270">This property gets or sets the title for the page as obtained from the pages header.</span></span>

## <a name="viewstateencryptionmode"></a><span data-ttu-id="fb6e3-271">ViewStateEncryptionMode</span><span class="sxs-lookup"><span data-stu-id="fb6e3-271">ViewStateEncryptionMode</span></span>

<span data-ttu-id="fb6e3-272">Возвращает или задает ViewStateEncryptionMode страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-272">Gets or sets the ViewStateEncryptionMode of the page.</span></span> <span data-ttu-id="fb6e3-273">См. Подробное описание этого свойства ранее в этом модуле.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-273">See a detailed discussion of this property earlier in this module.</span></span>

## <a name="new-protected-properties-of-the-page-class"></a><span data-ttu-id="fb6e3-274">Новые свойства защищенного класса страницы</span><span class="sxs-lookup"><span data-stu-id="fb6e3-274">New Protected Properties of the Page Class</span></span>

<span data-ttu-id="fb6e3-275">Ниже приведены новые защищенные свойства класса страницы в ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-275">The following are the new protected properties of the Page class in ASP.NET 2.0.</span></span>

## <a name="adapter"></a><span data-ttu-id="fb6e3-276">Адаптер</span><span class="sxs-lookup"><span data-stu-id="fb6e3-276">Adapter</span></span>

<span data-ttu-id="fb6e3-277">Возвращает ссылку на ControlAdapter, отображающий страницу на устройстве, которые запросили его.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-277">Returns a reference to the ControlAdapter that renders the page on the device that requested it.</span></span>

## <a name="asyncmode"></a><span data-ttu-id="fb6e3-278">AsyncMode</span><span class="sxs-lookup"><span data-stu-id="fb6e3-278">AsyncMode</span></span>

<span data-ttu-id="fb6e3-279">Это свойство указывает, обрабатывается ли страница асинхронно.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-279">This property indicates whether or not the page is processed asynchronously.</span></span> <span data-ttu-id="fb6e3-280">Он предназначен для использования средой выполнения, а не непосредственно в коде.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-280">It is intended for use by the runtime and not directly in code.</span></span>

## <a name="clientidseparator"></a><span data-ttu-id="fb6e3-281">ClientIDSeparator</span><span class="sxs-lookup"><span data-stu-id="fb6e3-281">ClientIDSeparator</span></span>

<span data-ttu-id="fb6e3-282">Это свойство возвращает символ, используемый в качестве разделителя, при создании уникального клиента идентификаторы для элементов управления.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-282">This property returns the character used as a separator when creating unique client IDs for controls.</span></span> <span data-ttu-id="fb6e3-283">Он предназначен для использования средой выполнения, а не непосредственно в коде.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-283">It is intended for use by the runtime and not directly in code.</span></span>

## <a name="pagestatepersister"></a><span data-ttu-id="fb6e3-284">PageStatePersister</span><span class="sxs-lookup"><span data-stu-id="fb6e3-284">PageStatePersister</span></span>

<span data-ttu-id="fb6e3-285">Это свойство возвращает объект PageStatePersister для страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-285">This property returns the PageStatePersister object for the page.</span></span> <span data-ttu-id="fb6e3-286">Это свойство используется преимущественно разработчиками элементов управления ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-286">This property is primarily used by ASP.NET control developers.</span></span>

## <a name="uniquefilepathsuffix"></a><span data-ttu-id="fb6e3-287">UniqueFilePathSuffix</span><span class="sxs-lookup"><span data-stu-id="fb6e3-287">UniqueFilePathSuffix</span></span>

<span data-ttu-id="fb6e3-288">Это свойство возвращает уникальный suffic, добавляемое к пути файла для кэширование браузеров.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-288">This property returns a unique suffic that is appended to the file path for caching browsers.</span></span> <span data-ttu-id="fb6e3-289">Значение по умолчанию — \_ \_ufps = и 6 цифр.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-289">The default value is \_\_ufps= and a 6-digit number.</span></span>

## <a name="new-public-methods-for-the-page-class"></a><span data-ttu-id="fb6e3-290">Новые общие методы для класса страницы</span><span class="sxs-lookup"><span data-stu-id="fb6e3-290">New Public Methods for the Page Class</span></span>

<span data-ttu-id="fb6e3-291">Для класса страницы в ASP.NET 2.0 появились следующие открытые методы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-291">The following public methods are new to the Page class in ASP.NET 2.0.</span></span>

## <a name="addonprerendercompleteasync"></a><span data-ttu-id="fb6e3-292">Метода AddOnPreRenderCompleteAsync</span><span class="sxs-lookup"><span data-stu-id="fb6e3-292">AddOnPreRenderCompleteAsync</span></span>

<span data-ttu-id="fb6e3-293">Этот метод регистрирует делегатов обработчиков событий для выполнения асинхронной страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-293">This method registers event handler delegates for asynchronous page execution.</span></span> <span data-ttu-id="fb6e3-294">Асинхронные страницы рассматриваются далее в этом модуле.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-294">Asynchronous pages are discussed later in this module.</span></span>

## <a name="applystylesheetskin"></a><span data-ttu-id="fb6e3-295">ApplyStyleSheetSkin</span><span class="sxs-lookup"><span data-stu-id="fb6e3-295">ApplyStyleSheetSkin</span></span>

<span data-ttu-id="fb6e3-296">Свойства в таблице стилей страницы применяется к странице.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-296">Applies the properties in a pages style sheet to the page.</span></span>

## <a name="executeregisteredasynctasks"></a><span data-ttu-id="fb6e3-297">ExecuteRegisteredAsyncTasks</span><span class="sxs-lookup"><span data-stu-id="fb6e3-297">ExecuteRegisteredAsyncTasks</span></span>

<span data-ttu-id="fb6e3-298">Этот метод начинается асинхронной задачи.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-298">This method beings an asynchronous task.</span></span>

### <a name="getvalidators"></a><span data-ttu-id="fb6e3-299">GetValidators</span><span class="sxs-lookup"><span data-stu-id="fb6e3-299">GetValidators</span></span>

<span data-ttu-id="fb6e3-300">Возвращает коллекцию проверяющих элементов управления для указанной группе или группе проверки по умолчанию, если он не задан.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-300">Returns a collection of validators for the specified validation group or the default validation group if none is specified.</span></span>

## <a name="registerasynctask"></a><span data-ttu-id="fb6e3-301">Метод RegisterAsyncTask</span><span class="sxs-lookup"><span data-stu-id="fb6e3-301">RegisterAsyncTask</span></span>

<span data-ttu-id="fb6e3-302">Этот метод регистрирует новый асинхронной задачи.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-302">This method registers a new async task.</span></span> <span data-ttu-id="fb6e3-303">Асинхронные страницы рассматриваются далее в этом модуле.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-303">Asynchronous pages are covered later in this module.</span></span>

## <a name="registerrequirescontrolstate"></a><span data-ttu-id="fb6e3-304">RegisterRequiresControlState</span><span class="sxs-lookup"><span data-stu-id="fb6e3-304">RegisterRequiresControlState</span></span>

<span data-ttu-id="fb6e3-305">Этот метод сообщает ASP.NET, что состояние элемента управления страницы должен быть сохранен.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-305">This method tells ASP.NET that the pages control state must be persisted.</span></span>

## <a name="registerrequiresviewstateencryption"></a><span data-ttu-id="fb6e3-306">RegisterRequiresViewStateEncryption</span><span class="sxs-lookup"><span data-stu-id="fb6e3-306">RegisterRequiresViewStateEncryption</span></span>

<span data-ttu-id="fb6e3-307">Этот метод сообщает ASP.NET, что viewstate страниц требует шифрования.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-307">This method tells ASP.NET that the pages viewstate requires encryption.</span></span>

## <a name="resolveclienturl"></a><span data-ttu-id="fb6e3-308">ResolveClientUrl</span><span class="sxs-lookup"><span data-stu-id="fb6e3-308">ResolveClientUrl</span></span>

<span data-ttu-id="fb6e3-309">Возвращает относительный URL-адрес, который может использоваться для клиентских запросов для изображений и т. д.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-309">Returns a relative URL that can be used for client requests for images, etc.</span></span>

## <a name="setfocus"></a><span data-ttu-id="fb6e3-310">Функция SetFocus</span><span class="sxs-lookup"><span data-stu-id="fb6e3-310">SetFocus</span></span>

<span data-ttu-id="fb6e3-311">Этот метод будет установить фокус на элемент управления, который указывается при начальной загрузке страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-311">This method will set the focus to the control that is specified when the page is initially loaded.</span></span>

## <a name="unregisterrequirescontrolstate"></a><span data-ttu-id="fb6e3-312">UnregisterRequiresControlState</span><span class="sxs-lookup"><span data-stu-id="fb6e3-312">UnregisterRequiresControlState</span></span>

<span data-ttu-id="fb6e3-313">Этот метод будет отменена регистрация элемент управления, который передается как больше не требует сохранения состояния управления.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-313">This method will unregister the control that is passed to it as no longer requiring control state persistence.</span></span>

## <a name="changes-to-the-page-lifecycle"></a><span data-ttu-id="fb6e3-314">Изменения в жизненном цикле страницы</span><span class="sxs-lookup"><span data-stu-id="fb6e3-314">Changes to the Page Lifecycle</span></span>

<span data-ttu-id="fb6e3-315">Жизненного цикла страницы ASP.NET 2.0 не существенно изменилось, но есть несколько новых методов, которые следует обратить внимание.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-315">The page lifecycle in ASP.NET 2.0 hasnt changed dramatically, but there are some new methods that you should be aware of.</span></span> <span data-ttu-id="fb6e3-316">Ниже приведены жизненного цикла страницы ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-316">The ASP.NET 2.0 page lifecycle is outlined below.</span></span>

## <a name="preinit-new-in-aspnet-20"></a><span data-ttu-id="fb6e3-317">PreInit (введено в ASP.NET 2.0)</span><span class="sxs-lookup"><span data-stu-id="fb6e3-317">PreInit (New in ASP.NET 2.0)</span></span>

<span data-ttu-id="fb6e3-318">Событие PreInit — самый ранний этап жизненного цикла, разработчик может получить доступ.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-318">The PreInit event is the earliest stage in the lifecycle that a developer can access.</span></span> <span data-ttu-id="fb6e3-319">Добавление данного события позволяет программно изменять темы ASP.NET 2.0, главные страницы, доступ к свойствам профиля ASP.NET 2.0 и т. д. Если вы находитесь в обратной передачи состояния, необходимо понимать, что Viewstate имеет еще не были применены к элементам управления на этом этапе жизненного цикла.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-319">The addition of this event makes it possible to programmatically change ASP.NET 2.0 themes, master pages, access properties for an ASP.NET 2.0 profile, etc. If you are in a postback state, its important to realize that Viewstate has not yet been applied to controls at this point in the lifecycle.</span></span> <span data-ttu-id="fb6e3-320">Таким образом Если разработчик изменяет свойство элемента управления на этом этапе, он скорее всего перезаписывается позднее в жизненном цикле страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-320">Therefore, if a developer changes a property of a control at this stage, it will likely be overwritten later in the pages lifecycle.</span></span>

## <a name="init"></a><span data-ttu-id="fb6e3-321">Init</span><span class="sxs-lookup"><span data-stu-id="fb6e3-321">Init</span></span>

<span data-ttu-id="fb6e3-322">События Init не отличается от ASP.NET 1.x.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-322">The Init event has not changed from ASP.NET 1.x.</span></span> <span data-ttu-id="fb6e3-323">Это будет место для чтения или инициализации свойств элементов управления на странице.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-323">This is where you would want to read or initialize properties of controls on your page.</span></span> <span data-ttu-id="fb6e3-324">В этой рабочей области, главные страницы, темы и т.д. уже применяется к странице.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-324">At this stage, master pages, themes, etc. are already applied to the page.</span></span>

## <a name="initcomplete-new-in-20"></a><span data-ttu-id="fb6e3-325">InitComplete (введено в версии 2.0)</span><span class="sxs-lookup"><span data-stu-id="fb6e3-325">InitComplete (New in 2.0)</span></span>

<span data-ttu-id="fb6e3-326">Событие InitComplete вызывается в конце этапа инициализации страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-326">The InitComplete event is called at the end of the pages initialization stage.</span></span> <span data-ttu-id="fb6e3-327">На этом этапе жизненного цикла, доступны элементы управления на странице, но их состояние еще не заполнены.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-327">At this point in the lifecycle, you can access controls on the page, but their state has not yet been populated.</span></span>

## <a name="preload-new-in-20"></a><span data-ttu-id="fb6e3-328">Предварительная загрузка (новое в версии 2.0)</span><span class="sxs-lookup"><span data-stu-id="fb6e3-328">PreLoad (New in 2.0)</span></span>

<span data-ttu-id="fb6e3-329">Это событие вызывается после применения всех данных обратной передачи и перед страницы\_нагрузки.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-329">This event is called after all postback data has been applied and just prior to Page\_Load.</span></span>

## <a name="load"></a><span data-ttu-id="fb6e3-330">Load</span><span class="sxs-lookup"><span data-stu-id="fb6e3-330">Load</span></span>

<span data-ttu-id="fb6e3-331">Событие загрузки не отличается от ASP.NET 1.x.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-331">The Load event has not changed from ASP.NET 1.x.</span></span>

## <a name="loadcomplete-new-in-20"></a><span data-ttu-id="fb6e3-332">LoadComplete (введено в версии 2.0)</span><span class="sxs-lookup"><span data-stu-id="fb6e3-332">LoadComplete (New in 2.0)</span></span>

<span data-ttu-id="fb6e3-333">Событие LoadComplete — последнего события в стадии загрузки страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-333">The LoadComplete event is the last event in the pages load stage.</span></span> <span data-ttu-id="fb6e3-334">На этом этапе все данные обратной передачи и viewstate была применена к странице.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-334">At this stage, all postback and viewstate data has been applied to the page.</span></span>

## <a name="prerender"></a><span data-ttu-id="fb6e3-335">PreRender</span><span class="sxs-lookup"><span data-stu-id="fb6e3-335">PreRender</span></span>

<span data-ttu-id="fb6e3-336">При желании для viewstate правильно поддерживаемое для элементов управления, которые добавляются к странице динамически события PreRender является последняя возможность добавлять их.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-336">If you would like for viewstate to be properly maintained for controls that are added to the page dynamically, the PreRender event is the last opportunity to add them.</span></span>

## <a name="prerendercomplete-new-in-20"></a><span data-ttu-id="fb6e3-337">PreRenderComplete (введено в версии 2.0)</span><span class="sxs-lookup"><span data-stu-id="fb6e3-337">PreRenderComplete (New in 2.0)</span></span>

<span data-ttu-id="fb6e3-338">На этапе PreRenderComplete были добавлены все элементы управления на страницу и страница готова для отображения.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-338">At the PreRenderComplete stage, all controls have been added to the page and the page is ready to be rendered.</span></span> <span data-ttu-id="fb6e3-339">События PreRenderComplete является последнее событие, возникающее перед сохранением viewstate страниц.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-339">The PreRenderComplete event is the last event raised before the pages viewstate is saved.</span></span>

## <a name="savestatecomplete-new-in-20"></a><span data-ttu-id="fb6e3-340">SaveStateComplete (введено в версии 2.0)</span><span class="sxs-lookup"><span data-stu-id="fb6e3-340">SaveStateComplete (New in 2.0)</span></span>

<span data-ttu-id="fb6e3-341">Событие SaveStateComplete вызывается сразу после сохранения все страницы viewstate и состояния элемента управления.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-341">The SaveStateComplete event is called immediately after all page viewstate and control state has been saved.</span></span> <span data-ttu-id="fb6e3-342">Это последнее событие, прежде чем фактически отображении страницы в браузере.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-342">This is the last event before the page is actually rendered to the browser.</span></span>

## <a name="render"></a><span data-ttu-id="fb6e3-343">Прорисовка</span><span class="sxs-lookup"><span data-stu-id="fb6e3-343">Render</span></span>

<span data-ttu-id="fb6e3-344">Метод Render не была изменена с момента ASP.NET 1.x.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-344">The Render method has not changed since ASP.NET 1.x.</span></span> <span data-ttu-id="fb6e3-345">Это где HtmlTextWriter инициализируется и отображении страницы в браузере.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-345">This is where the HtmlTextWriter is initialized and the page is rendered to the browser.</span></span>

## <a name="cross-page-postback-in-aspnet-20"></a><span data-ttu-id="fb6e3-346">Обратной передачи страницы между в ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="fb6e3-346">Cross-Page Postback in ASP.NET 2.0</span></span>

<span data-ttu-id="fb6e3-347">В ASP.NET 1.x, обратных передач требовались для учета на одной странице.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-347">In ASP.NET 1.x, postbacks were required to post to the same page.</span></span> <span data-ttu-id="fb6e3-348">Обратную передачу между страницы, не допускалось.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-348">Cross-page postbacks were not allowed.</span></span> <span data-ttu-id="fb6e3-349">ASP.NET 2.0 добавляет возможность выполнять обратную передачу на другую страницу в интерфейсе IButtonControl.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-349">ASP.NET 2.0 adds the ability to post back to a different page via the IButtonControl interface.</span></span> <span data-ttu-id="fb6e3-350">Любой элемент управления, реализующий интерфейс записи IButtonControl (кнопка LinkButton и ImageButton помимо пользовательские элементы управления сторонних разработчиков) могут воспользоваться преимуществами этой новой функциональной возможности, посредством использования атрибута PostBackUrl.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-350">Any control that implements the new IButtonControl interface (Button, LinkButton, and ImageButton in addition to third-party custom controls) can take advantage of this new functionality via the use of the PostBackUrl attribute.</span></span> <span data-ttu-id="fb6e3-351">В следующем коде показано элемент управления Button, отправляет второй страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-351">The following code shows a Button control that posts back to a second page.</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

<span data-ttu-id="fb6e3-352">При отправке страницы обратно, инициирующего обратной передачи страницы доступна через свойство PreviousPage на второй странице.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-352">When the page is posted back, the Page that initiates the postback is accessible via the PreviousPage property on the second page.</span></span> <span data-ttu-id="fb6e3-353">Эта функциональная возможность реализуется через новый веб-форма\_DoPostBackWithOptions клиентской функции, если элемент управления выполняет обратную передачу другой страницы ASP.NET 2.0 отображает для страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-353">This functionality is implemented via the new WebForm\_DoPostBackWithOptions client-side function that ASP.NET 2.0 renders to the page when a control posts back to a different page.</span></span> <span data-ttu-id="fb6e3-354">Новый дескриптор WebResource.axd, который порождает сценарий клиенту предоставляется этой функции JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-354">This JavaScript function is provided by the new WebResource.axd handler which emits script to the client.</span></span>

<span data-ttu-id="fb6e3-355">Видео ниже приведен пример страницы между обратной передачи.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-355">The video below is a walkthrough of a cross-page postback.</span></span>


![](the-asp-net-2-0-page-model/_static/image2.png)


[<span data-ttu-id="fb6e3-356">Откройте весь экран</span><span class="sxs-lookup"><span data-stu-id="fb6e3-356">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/xpage1.wmv)


## <a name="more-details-on-cross-page-postbacks"></a><span data-ttu-id="fb6e3-357">Дополнительные сведения об обратных передач между страницы</span><span class="sxs-lookup"><span data-stu-id="fb6e3-357">More Details on Cross-Page Postbacks</span></span>

### <a name="viewstate"></a><span data-ttu-id="fb6e3-358">Состояние просмотра</span><span class="sxs-lookup"><span data-stu-id="fb6e3-358">Viewstate</span></span>

<span data-ttu-id="fb6e3-359">Может запрос самостоятельно уже сведения о том, что происходит с первой страницы в сценарии обратной передачи страницы между в viewstate.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-359">You may have asked yourself already about what happens to the viewstate from the first page in a cross-page postback scenario.</span></span> <span data-ttu-id="fb6e3-360">В конце концов любой элемент управления, который не реализует IPostBackDataHandler таким образом сохранять свое состояние через состояние просмотра, чтобы иметь доступ к свойствам этого элемента управления на второй странице обратной передачи страницы между, необходимо иметь доступ к состояние отображения для страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-360">After all, any control that does not implement IPostBackDataHandler will persist its state via viewstate, so to have access to the properties of that control on the second page of a cross-page postback, you must have access to the viewstate for the page.</span></span> <span data-ttu-id="fb6e3-361">ASP.NET 2.0 берет на себя этот сценарий с помощью нового скрытого поля на второй странице называется \_ \_PREVIOUSPAGE.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-361">ASP.NET 2.0 takes care of this scenario using a new hidden field in the second page called \_\_PREVIOUSPAGE.</span></span> <span data-ttu-id="fb6e3-362">\_ \_PREVIOUSPAGE поле формы содержит viewstate для первой страницы, чтобы на второй странице можно получить доступ к свойствам всех элементов управления.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-362">The \_\_PREVIOUSPAGE form field contains the viewstate for the first page so that you can have access to the properties of all controls in the second page.</span></span>

### <a name="circumventing-findcontrol"></a><span data-ttu-id="fb6e3-363">Обход FindControl</span><span class="sxs-lookup"><span data-stu-id="fb6e3-363">Circumventing FindControl</span></span>

<span data-ttu-id="fb6e3-364">В видеоруководство обратной передачи страницы между я использовал метод FindControl для получения ссылки на элемент управления TextBox на первой странице.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-364">In the video walkthrough of a cross-page postback, I used the FindControl method to get a reference to the TextBox control on the first page.</span></span> <span data-ttu-id="fb6e3-365">Этот метод хорошо подходит для этой цели, но метод FindControl расходуется, которому требуется написать дополнительный код.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-365">That method works well for that purpose, but FindControl is expensive and it requires writing additional code.</span></span> <span data-ttu-id="fb6e3-366">К счастью ASP.NET 2.0 представляет собой альтернативу FindControl для этой цели, который будет работать в разных сценариях.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-366">Fortunately, ASP.NET 2.0 provides an alternative to FindControl for this purpose that will work in many scenarios.</span></span> <span data-ttu-id="fb6e3-367">Директива PreviousPageType позволяет иметь строго типизированную ссылку на предыдущую страницу с помощью TypeName или VirtualPath атрибута.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-367">The PreviousPageType directive allows you to have a strongly-typed reference to the previous page by using either the TypeName or the VirtualPath attribute.</span></span> <span data-ttu-id="fb6e3-368">Атрибут TypeName позволяет указать тип предыдущей страницы, хотя атрибут VirtualPath позволяет ссылаться на предыдущую страницу, с помощью виртуального пути.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-368">The TypeName attribute allows you to specify the type of the previous page while the VirtualPath attribute allows you to refer to the previous page using a virtual path.</span></span> <span data-ttu-id="fb6e3-369">После выбора директива PreviousPageType, необходимо предоставить элементы управления, т. д., к которому вы хотите разрешить доступ с помощью открытые свойства.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-369">After you've set the PreviousPageType directive, you must then expose the controls, etc. to which you want to allow access using public properties.</span></span>

## <a name="lab-1-cross-page-postback"></a><span data-ttu-id="fb6e3-370">Обратной передачи страницы между Практическое занятие 1</span><span class="sxs-lookup"><span data-stu-id="fb6e3-370">Lab 1 Cross-Page Postback</span></span>

<span data-ttu-id="fb6e3-371">В этой лабораторной работы вы создадите приложение, использующее новый кросс-обратной передачи функциональных возможностей страницы ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-371">In this lab, you will create an application that uses the new cross-page postback functionality of ASP.NET 2.0.</span></span>

1. <span data-ttu-id="fb6e3-372">Откройте Visual Studio 2005 и создайте новый веб-сайте ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-372">Open Visual Studio 2005 and create a new ASP.NET Web site.</span></span>
2. <span data-ttu-id="fb6e3-373">Добавьте новый веб-форма вызывается page2.aspx.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-373">Add a new Webform called page2.aspx.</span></span>
3. <span data-ttu-id="fb6e3-374">Откройте страницу Default.aspx в представлении конструктора и добавьте элемент управления Button и элемент управления TextBox.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-374">Open the Default.aspx in Design view and add a Button control and a TextBox control.</span></span> 

    1. <span data-ttu-id="fb6e3-375">Присвойте кнопке Идентификатором **кнопка '' Отправить** и текстовое поле элемента управления с Идентификатором **UserName**.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-375">Give the Button control an ID of **SubmitButton** and the TextBox control an ID of **UserName**.</span></span>
    2. <span data-ttu-id="fb6e3-376">Значение свойства PostBackUrl кнопки page2.aspx.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-376">Set the PostBackUrl property of the Button to page2.aspx.</span></span>
4. <span data-ttu-id="fb6e3-377">Откройте page2.aspx в режиме исходного кода.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-377">Open page2.aspx in Source view.</span></span>
5. <span data-ttu-id="fb6e3-378">Добавьте директиву @ PreviousPageType, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="fb6e3-378">Add a @ PreviousPageType directive as shown below:</span></span>
6. <span data-ttu-id="fb6e3-379">Добавьте следующий код к странице\_загрузку page2.aspx элемента кода:</span><span class="sxs-lookup"><span data-stu-id="fb6e3-379">Add the following code to the Page\_Load of page2.aspx's code-behind:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. <span data-ttu-id="fb6e3-380">Постройте проект, щелкнув меню "Построение" в сборке.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-380">Build the project by clicking on Build on the Build menu.</span></span>
8. <span data-ttu-id="fb6e3-381">Добавьте следующий код, чтобы код программной части для Default.aspx:</span><span class="sxs-lookup"><span data-stu-id="fb6e3-381">Add the following code to the code-behind for Default.aspx:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. <span data-ttu-id="fb6e3-382">Изменить страницу\_нагрузки в page2.aspx следующее:</span><span class="sxs-lookup"><span data-stu-id="fb6e3-382">Change the Page\_Load in page2.aspx to the following:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. <span data-ttu-id="fb6e3-383">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-383">Build the project.</span></span>
11. <span data-ttu-id="fb6e3-384">Запустите проект.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-384">Run the project.</span></span>
12. <span data-ttu-id="fb6e3-385">Введите имя в текстовое поле и нажмите кнопку.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-385">Enter your name in the TextBox and click the button.</span></span>
13. <span data-ttu-id="fb6e3-386">Что такое результат?</span><span class="sxs-lookup"><span data-stu-id="fb6e3-386">What is the result?</span></span>

## <a name="asynchronous-pages-in-aspnet-20"></a><span data-ttu-id="fb6e3-387">Асинхронные страницы в ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="fb6e3-387">Asynchronous Pages in ASP.NET 2.0</span></span>

<span data-ttu-id="fb6e3-388">Многие конфликты при работе в ASP.NET вызываются задержки внешних вызовов (например, вызовы службы или базы данных Web), задержка ввода-ВЫВОДА файла, и т. д. При выполнении запроса для приложения ASP.NET, ASP.NET использует один из рабочих потоков для обслуживания этого запроса.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-388">Many contention problems in ASP.NET are caused by latency of external calls (such as Web service or database calls), file IO latency, etc. When a request is made against an ASP.NET application, ASP.NET uses one of its worker threads to service that request.</span></span> <span data-ttu-id="fb6e3-389">Этот запрос является владельцем этого потока до завершения запроса и отправки ответа.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-389">That request owns that thread until the request is complete and the response has been sent.</span></span> <span data-ttu-id="fb6e3-390">ASP.NET 2.0 предназначено для устранения проблемы задержки с помощью таких проблем путем добавления возможность асинхронного выполнения страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-390">ASP.NET 2.0 seeks to resolve latency issues with these types of issues by adding the capability to execute pages asynchronously.</span></span> <span data-ttu-id="fb6e3-391">Означает, что рабочий поток можно запустить запрос и передать дополнительные выполнение другому потоку, тем самым быстро возвращение пул доступных потоков.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-391">That means that a worker thread can start the request and then hand off additional execution to another thread, thereby returning to the available thread pool quickly.</span></span> <span data-ttu-id="fb6e3-392">После завершения операции ввода-ВЫВОДА для файла, вызов базы данных, т. д. получен новый поток из пула потоков для завершения запроса.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-392">When the file IO, database call, etc. has completed, a new thread is obtained from the thread pool to finish the request.</span></span>

<span data-ttu-id="fb6e3-393">Первым шагом в создании асинхронного выполнения страницы является установка **Async** атрибута директивы страницы следующим образом:</span><span class="sxs-lookup"><span data-stu-id="fb6e3-393">The first step in making a page execute asynchronously is to set the **Async** attribute of the page directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

<span data-ttu-id="fb6e3-394">Этот атрибут сообщает ASP.NET для реализации IHttpAsyncHandler для страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-394">This attribute tells ASP.NET to implement the IHttpAsyncHandler for the page.</span></span>

<span data-ttu-id="fb6e3-395">Следующим шагом является вызов метода метода AddOnPreRenderCompleteAsync в определенный момент в жизненном цикле страницы до PreRender.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-395">The next step is to call the AddOnPreRenderCompleteAsync method at a point in the lifecycle of the page prior to PreRender.</span></span> <span data-ttu-id="fb6e3-396">(Этот метод обычно вызывается в страницу\_нагрузки.) Метод метода AddOnPreRenderCompleteAsync принимает два параметра; BeginEventHandler и EndEventHandler.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-396">(This method is typically called in Page\_Load.) The AddOnPreRenderCompleteAsync method takes two parameters; a BeginEventHandler and an EndEventHandler.</span></span> <span data-ttu-id="fb6e3-397">BeginEventHandler возвращает значение IAsyncResult, который затем передается как параметр EndEventHandler.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-397">The BeginEventHandler returns an IAsyncResult which is then passed as a parameter to the EndEventHandler.</span></span>

<span data-ttu-id="fb6e3-398">Видео, ниже приведен пример запроса асинхронной страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-398">The video below is a walkthrough of an asynchronous page request.</span></span>


![](the-asp-net-2-0-page-model/_static/image3.png)


[<span data-ttu-id="fb6e3-399">Откройте весь экран</span><span class="sxs-lookup"><span data-stu-id="fb6e3-399">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/async1.wmv)


> [!NOTE]
> <span data-ttu-id="fb6e3-400">До завершения EndEventHandler страницей async не отобразить в обозревателе.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-400">An async page does not render to the browser until the EndEventHandler has completed.</span></span> <span data-ttu-id="fb6e3-401">Несомненно, но некоторым разработчикам будет рассматривать запросов асинхронного как аналог асинхронные обратные вызовы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-401">No doubt but that some developers will think of async requests as being similar to async callbacks.</span></span> <span data-ttu-id="fb6e3-402">Очень важно помнить, что они не являются.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-402">It's important to realize that they are not.</span></span> <span data-ttu-id="fb6e3-403">Преимущество для асинхронных запросов заключается в том, что первый рабочий поток могут быть возвращены в пул потоков для обслуживания новых запросов, снижая таким образом состязания из-за привязки ввода-ВЫВОДА и т. д.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-403">The benefit to asynchronous requests is that the first worker thread can be returned to the thread pool to service new requests, thereby reducing contention due to being IO bound, etc.</span></span>


## <a name="script-callbacks-in-aspnet-20"></a><span data-ttu-id="fb6e3-404">Обратные вызовы из сценария в ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="fb6e3-404">Script Callbacks in ASP.NET 2.0</span></span>

<span data-ttu-id="fb6e3-405">Веб-разработчики всегда поиск способов предотвратить мерцание, связанные с обратным вызовом.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-405">Web developers have always looked for ways to prevent the flickering associated with a callback.</span></span> <span data-ttu-id="fb6e3-406">В ASP.NET 1.x, SmartNavigation был наиболее распространенный способ избежать мерцание, но SmartNavigation причиной проблемы для разработчиков, некоторые из-за сложности реализации на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-406">In ASP.NET 1.x, SmartNavigation was the most common method for avoiding flickering, but SmartNavigation caused problems for some developers because of the complexity of its implementation on the client.</span></span> <span data-ttu-id="fb6e3-407">ASP.NET 2.0 решает эту проблему с обратные вызовы из сценария.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-407">ASP.NET 2.0 addresses this issue with script callbacks.</span></span> <span data-ttu-id="fb6e3-408">Обратные вызовы из сценария используйте XMLHttp для запросов к веб-сервере через JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-408">Script callbacks utilize XMLHttp to make requests against the Web server via JavaScript.</span></span> <span data-ttu-id="fb6e3-409">XMLHttp запрос возвращает XML-данных, можно управлять с помощью обозревателя DOM.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-409">The XMLHttp request returns XML data that can then be manipulated via the browser's DOM.</span></span> <span data-ttu-id="fb6e3-410">XMLHttp код является скрытым от пользователя, новый дескриптор WebResource.axd.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-410">XMLHttp code is hidden from the user by the new WebResource.axd handler.</span></span>

<span data-ttu-id="fb6e3-411">Существуют несколько шагов, которые необходимы для настройки обратный вызов сценария в ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-411">There are several steps that are necessary in order to configure a script callback in ASP.NET 2.0.</span></span>

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a><span data-ttu-id="fb6e3-412">Шаг 1: Реализация интерфейса ICallbackEventHandler</span><span class="sxs-lookup"><span data-stu-id="fb6e3-412">Step 1 : Implement the ICallbackEventHandler Interface</span></span>

<span data-ttu-id="fb6e3-413">Чтобы ASP.NET, чтобы распознать страницы участвовать в обратный вызов сценария необходимо реализовать интерфейс ICallbackEventHandler.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-413">In order for ASP.NET to recognize your page as participating in a script callback, you must implement the ICallbackEventHandler interface.</span></span> <span data-ttu-id="fb6e3-414">Это можно сделать в файле кода следующим образом:</span><span class="sxs-lookup"><span data-stu-id="fb6e3-414">You can do this in your code-behind file like so:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

<span data-ttu-id="fb6e3-415">Также это можно сделать с помощью like директиву @ Implements так:</span><span class="sxs-lookup"><span data-stu-id="fb6e3-415">You can also do this using the @ Implements directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

<span data-ttu-id="fb6e3-416">Обычно используется директива @ Implements при использовании встроенного кода ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-416">You would typically use the @ Implements directive when using inline ASP.NET code.</span></span>

## <a name="step-2--call-getcallbackeventreference"></a><span data-ttu-id="fb6e3-417">Шаг 2: Вызов GetCallbackEventReference</span><span class="sxs-lookup"><span data-stu-id="fb6e3-417">Step 2 : Call GetCallbackEventReference</span></span>

<span data-ttu-id="fb6e3-418">Как упоминалось ранее, вызов XMLHttp инкапсулируется в дескриптор WebResource.axd.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-418">As mentioned previously, the XMLHttp call is encapsulated in the WebResource.axd handler.</span></span> <span data-ttu-id="fb6e3-419">При отображении страницы ASP.NET будет добавить вызов веб-форма\_DoCallback клиентский скрипт, который предоставляется WebResource.axd.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-419">When your page is rendered, ASP.NET will add a call to WebForm\_DoCallback, a client script that is provided by WebResource.axd.</span></span> <span data-ttu-id="fb6e3-420">Веб-форма\_DoCallback функция заменяет \_ \_doPostBack функции для обратного вызова.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-420">The WebForm\_DoCallback function replaces the \_\_doPostBack function for a callback.</span></span> <span data-ttu-id="fb6e3-421">Следует помнить, что \_ \_doPostBack программным путем отправки формы на странице.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-421">Remember that \_\_doPostBack programmatically submits the form on the page.</span></span> <span data-ttu-id="fb6e3-422">В сценарии обратного вызова, необходимо предотвратить обратной передачи, поэтому \_ \_doPostBack не будет достаточно.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-422">In a callback scenario, you want to prevent a postback, so \_\_doPostBack will not suffice.</span></span>

> [!NOTE]
> <span data-ttu-id="fb6e3-423">\_\_doPostBack будет отображаться в клиентском сценарии обратного вызова скрипта к странице.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-423">\_\_doPostBack is still rendered to the page in a client script callback scenario.</span></span> <span data-ttu-id="fb6e3-424">Однако он не используется для обратного вызова.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-424">However, it's not used for the callback.</span></span>


<span data-ttu-id="fb6e3-425">Аргументы для веб-форма\_DoCallback клиентской функции предоставляются через серверные функции GetCallbackEventReference, который обычно называется странице\_нагрузки.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-425">The arguments for the WebForm\_DoCallback client-side function are provided via the server-side function GetCallbackEventReference which would normally be called in Page\_Load.</span></span> <span data-ttu-id="fb6e3-426">Типичный вызов GetCallbackEventReference может выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="fb6e3-426">A typical call to GetCallbackEventReference might look like this:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> <span data-ttu-id="fb6e3-427">В этом случае cm является экземпляром ClientScriptManager.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-427">In this case, cm is an instance of ClientScriptManager.</span></span> <span data-ttu-id="fb6e3-428">Класс ClientScriptManager будет рассматриваться далее в этом модуле.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-428">The ClientScriptManager class will be covered later in this module.</span></span>


<span data-ttu-id="fb6e3-429">Существует несколько перегруженных версий GetCallbackEventReference.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-429">There are several overloaded versions of GetCallbackEventReference.</span></span> <span data-ttu-id="fb6e3-430">В этом случае аргументы являются следующим образом:</span><span class="sxs-lookup"><span data-stu-id="fb6e3-430">In this case, the arguments are as follows:</span></span>

`this`

<span data-ttu-id="fb6e3-431">Ссылка на элемент управления, где вызывается GetCallbackEventReference.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-431">A reference to the control where GetCallbackEventReference is being called.</span></span> <span data-ttu-id="fb6e3-432">В этом случае это сама страница.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-432">In this case, it's the page itself.</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

<span data-ttu-id="fb6e3-433">Строковый аргумент, который будет передаваться из клиентского кода серверного события.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-433">A string argument that will be passed from the client-side code to the server-side event.</span></span> <span data-ttu-id="fb6e3-434">В этом случае обмена мгновенными сообщениями при передаче значения из раскрывающегося списка вызывается ddlCompany.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-434">In this case, Im passing the value of a dropdown called ddlCompany.</span></span>

`ShowCompanyName`

<span data-ttu-id="fb6e3-435">Имя клиентской функции, которая будет принимать возвращаемое значение (в форме строки) из события обратного вызова на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-435">The name of the client-side function that will accept the return value (as string) from the server-side callback event.</span></span> <span data-ttu-id="fb6e3-436">Эта функция будет вызываться только в случае успешного обратного вызова на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-436">This function will only be called when the server-side callback is successful.</span></span> <span data-ttu-id="fb6e3-437">Таким образом для обеспечения надежности, обычно рекомендуется использовать перегруженную версию GetCallbackEventReference, принимающий аргумент дополнительной строки, указав имя клиентской функции для выполнения в случае ошибки.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-437">Therefore, for the sake of robustness, it is generally recommended to use the overloaded version of GetCallbackEventReference that takes an additional string argument specifying the name of a client-side function to execute in the event of an error.</span></span>

`null`

<span data-ttu-id="fb6e3-438">Строка, представляющая клиентской функции, инициированное перед выполнением обратного вызова на сервере.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-438">A string representing a client-side function that it initiated before the callback to the server.</span></span> <span data-ttu-id="fb6e3-439">В этом случае нет нет таких сценариев, поэтому аргумент равен null.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-439">In this case, there is no such script, so the argument is null.</span></span>

`true`

<span data-ttu-id="fb6e3-440">Логическое значение, определяющее, асинхронно поведения обратного вызова или нет.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-440">A Boolean specifying whether or not to conduct the callback asynchronously.</span></span>

<span data-ttu-id="fb6e3-441">Вызов веб-форма\_DoCallback на клиенте будет передавать эти аргументы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-441">The call to WebForm\_DoCallback on the client will pass these arguments.</span></span> <span data-ttu-id="fb6e3-442">Таким образом, эта страница отображается на клиентском компьютере, этот код будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="fb6e3-442">Therefore, when this page is rendered on the client, that code will look like so:</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

<span data-ttu-id="fb6e3-443">Обратите внимание на сигнатуру функции на стороне клиента немного по-разному.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-443">Notice that the signature of the function on the client is a bit different.</span></span> <span data-ttu-id="fb6e3-444">Клиентские функции передает 5 строк и логическое значение.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-444">The client-side function passes 5 strings and a Boolean.</span></span> <span data-ttu-id="fb6e3-445">Дополнительная строка, (который имеет значение null, в приведенном выше примере) содержит клиентской функции, который будет обрабатывать ошибки из обратного вызова на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-445">The additional string (which is null in the above example) contains the client-side function that will handle any errors from the server-side callback.</span></span>

## <a name="step-3--hook-the-client-side-control-event"></a><span data-ttu-id="fb6e3-446">Шаг 3.-Обработчик событий клиентский элемент управления</span><span class="sxs-lookup"><span data-stu-id="fb6e3-446">Step 3 : Hook the Client-Side Control Event</span></span>

<span data-ttu-id="fb6e3-447">Обратите внимание, что возвращаемое значение выше GetCallbackEventReference был назначен переменной типа string.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-447">Notice that the return value of GetCallbackEventReference above was assigned to a string variable.</span></span> <span data-ttu-id="fb6e3-448">Эта строка используется для обработки событий на стороне клиента для элемента управления, который инициирует обратный вызов.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-448">That string is used to hook a client-side event for the control that initiates the callback.</span></span> <span data-ttu-id="fb6e3-449">В этом примере обратный вызов инициируется раскрывающийся список на странице, поэтому требуется подключаться к *OnChange* события.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-449">In this example, the callback is initiated by a dropdown on the page, so I want to hook the *OnChange* event.</span></span>

<span data-ttu-id="fb6e3-450">Для обработки событий на стороне клиента, просто добавьте обработчик разметки клиентские следующим образом:</span><span class="sxs-lookup"><span data-stu-id="fb6e3-450">To hook the client-side event, simply add a handler to the client-side markup as follows:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

<span data-ttu-id="fb6e3-451">Помните, что *cbRef* является возвращаемым значением из вызова GetCallbackEventReference.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-451">Recall that *cbRef* is the return value from the call to GetCallbackEventReference.</span></span> <span data-ttu-id="fb6e3-452">Он содержит вызов веб-форма\_DoCallback, показанном выше.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-452">It contains the call to WebForm\_DoCallback that was shown above.</span></span>

## <a name="step-4--register-the-client-side-script"></a><span data-ttu-id="fb6e3-453">Шаг 4: Регистрация клиентского скрипта</span><span class="sxs-lookup"><span data-stu-id="fb6e3-453">Step 4 : Register the Client-Side Script</span></span>

<span data-ttu-id="fb6e3-454">Напомним, что вызов GetCallbackEventReference указано, что клиентского скрипта с именем **ShowCompanyName** будет выполняться после успешного завершения обратного вызова на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-454">Recall that the call to GetCallbackEventReference specified that a client-side script called **ShowCompanyName** would be executed when the server-side callback succeeds.</span></span> <span data-ttu-id="fb6e3-455">Этот скрипт должен быть добавлен на страницу с помощью экземпляра ClientScriptManager.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-455">That script needs to be added to the page using a ClientScriptManager instance.</span></span> <span data-ttu-id="fb6e3-456">(Класс ClientScriptManager будет dicussed далее в этом модуле.) Как сделать так:</span><span class="sxs-lookup"><span data-stu-id="fb6e3-456">(The ClientScriptManager class will be dicussed later in this module.) You do that like so:</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a><span data-ttu-id="fb6e3-457">Шаг 5: Вызовите методы интерфейса ICallbackEventHandler</span><span class="sxs-lookup"><span data-stu-id="fb6e3-457">Step 5 : Call the Methods of the ICallbackEventHandler Interface</span></span>

<span data-ttu-id="fb6e3-458">ICallbackEventHandler содержит два метода, которые необходимо реализовать в коде.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-458">The ICallbackEventHandler contains two methods that you need to implement in your code.</span></span> <span data-ttu-id="fb6e3-459">Они являются **RaiseCallbackEvent** и **GetCallbackEvent**.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-459">They are **RaiseCallbackEvent** and **GetCallbackEvent**.</span></span>

<span data-ttu-id="fb6e3-460">**RaiseCallbackEvent** принимает строку в качестве аргумента и возвращает значение nothing.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-460">**RaiseCallbackEvent** takes a string as an argument and returns nothing.</span></span> <span data-ttu-id="fb6e3-461">Аргумент строки передается из вызова клиентского веб-форма\_DoCallback.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-461">The string argument is passed from the client-side call to WebForm\_DoCallback.</span></span> <span data-ttu-id="fb6e3-462">В этом случае это значение является *значение* атрибут называется ddlCompany раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-462">In this case, that value is the *value* attribute of the dropdown called ddlCompany.</span></span> <span data-ttu-id="fb6e3-463">Ваш серверный код следует разместить в методе RaiseCallbackEvent.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-463">Your server-side code should be placed in the RaiseCallbackEvent method.</span></span> <span data-ttu-id="fb6e3-464">Например если обратного вызова совершает WebRequest от внешнего ресурса, этот код должно находиться в RaiseCallbackEvent.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-464">For example, if your callback is making a WebRequest against an external resource, that code should be placed in RaiseCallbackEvent.</span></span>

<span data-ttu-id="fb6e3-465">**GetCallbackEvent** отвечает за обработку возврата обратного вызова клиента.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-465">**GetCallbackEvent** is responsible for processing the return of the callback to the client.</span></span> <span data-ttu-id="fb6e3-466">Он не принимает аргументы и возвращает строку.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-466">It takes no arguments and returns a string.</span></span> <span data-ttu-id="fb6e3-467">Строка, она возвращает будет передан в качестве аргумента клиентской функции, в этом случае *ShowCompanyName*.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-467">The string that it returns will be passed as an argument to the client-side function, in this case *ShowCompanyName*.</span></span>

<span data-ttu-id="fb6e3-468">После завершения действия вы готовы выполнить обратный вызов сценария в ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-468">Once you have completed the above steps, you are ready to perform a script callback in ASP.NET 2.0.</span></span>


![](the-asp-net-2-0-page-model/_static/image4.png)


[<span data-ttu-id="fb6e3-469">Откройте весь экран</span><span class="sxs-lookup"><span data-stu-id="fb6e3-469">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/callback1.wmv)


<span data-ttu-id="fb6e3-470">Обратные вызовы из сценария в ASP.NET поддерживаются в любого браузера, поддерживающего вызовах XMLHttp.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-470">Script callbacks in ASP.NET are supported in any browser that supports making XMLHttp calls.</span></span> <span data-ttu-id="fb6e3-471">Включает все современные браузеры используется сегодня.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-471">That includes all of the modern browsers in use today.</span></span> <span data-ttu-id="fb6e3-472">Internet Explorer использует объект XMLHttp ActiveX, а другие современных браузеров (в том числе предстоящих Internet Explorer 7) используйте встроенный объект XMLHttp.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-472">Internet Explorer uses the XMLHttp ActiveX object while other modern browsers (including the upcoming IE 7) use an intrinsic XMLHttp object.</span></span> <span data-ttu-id="fb6e3-473">Для программного определения, если браузер поддерживает обратных вызовов, можно использовать **Request.Browser.SupportCallback** свойство.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-473">To programmatically determine if a browser supports callbacks, you can use the **Request.Browser.SupportCallback** property.</span></span> <span data-ttu-id="fb6e3-474">Это свойство будет возвращать **true** Если запрашивающему клиенту поддерживает обратные вызовы из сценария.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-474">This property will return **true** if the requesting client supports script callbacks.</span></span>

## <a name="working-with-client-script-in-aspnet-20"></a><span data-ttu-id="fb6e3-475">Работа с клиентским скриптом в ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="fb6e3-475">Working with Client Script in ASP.NET 2.0</span></span>

<span data-ttu-id="fb6e3-476">Клиентские скрипты в ASP.NET 2.0 управляются посредством использования ClientScriptManager класса.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-476">Client scripts in ASP.NET 2.0 are managed via the use of the ClientScriptManager class.</span></span> <span data-ttu-id="fb6e3-477">Класс ClientScriptManager хранит информацию о клиентских скриптов, используя тип и имя.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-477">The ClientScriptManager class keeps track of client scripts using a type and a name.</span></span> <span data-ttu-id="fb6e3-478">Это предотвращает тот же скрипт, программно вставляемого на странице более одного раза.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-478">This prevents the same script from being programmatically inserted on a page more than once.</span></span>

> [!NOTE]
> <span data-ttu-id="fb6e3-479">После скрипт был успешно зарегистрирован на странице, все последующие попытки зарегистрировать тот же скрипт просто приведет к скрипт не регистрируется повторно.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-479">After a script has been successfully registered on a page, any subsequent attempt to register the same script will simply result in the script not being registered a second time.</span></span> <span data-ttu-id="fb6e3-480">Повторяющиеся сценарии не будут добавлены и исключение не возникает.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-480">No duplicate scripts are added and no exception occurs.</span></span> <span data-ttu-id="fb6e3-481">Чтобы избежать ненужных вычислений, существуют методы, которые можно использовать для определения, скрипт зарегистрирован ли, чтобы не пытайтесь зарегистрировать его несколько раз.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-481">To avoid unnecessary computation, there are methods that you can use to determine if a script is already registered so that you do not attempt to register it more than once.</span></span>


<span data-ttu-id="fb6e3-482">Методы ClientScriptManager должны быть знакомы разработчикам все текущего ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="fb6e3-482">The methods of the ClientScriptManager should be familiar to all current ASP.NET developers:</span></span>

## <a name="registerclientscriptblock"></a><span data-ttu-id="fb6e3-483">RegisterClientScriptBlock</span><span class="sxs-lookup"><span data-stu-id="fb6e3-483">RegisterClientScriptBlock</span></span>

<span data-ttu-id="fb6e3-484">Этот метод добавляет скрипт к началу отображаемой страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-484">This method adds a script to the top of the rendered page.</span></span> <span data-ttu-id="fb6e3-485">Это полезно для добавления функций, которые будут вызываться явным образом на клиенте.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-485">This is useful for adding functions that will be explicitly called on the client.</span></span>

<span data-ttu-id="fb6e3-486">Существует две перегруженные версии этого метода.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-486">There are two overloaded versions of this method.</span></span> <span data-ttu-id="fb6e3-487">Три из четырех аргументов являются общими для них.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-487">Three of four arguments are common among them.</span></span> <span data-ttu-id="fb6e3-488">Они приведены ниже.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-488">They are:</span></span>

`type (string)`

<span data-ttu-id="fb6e3-489">***Тип*** аргумент указывает тип для скрипта.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-489">The ***type*** argument identifies a type for the script.</span></span> <span data-ttu-id="fb6e3-490">Обычно рекомендуется использовать тип страницы (это. GetType()) для типа.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-490">It is generally a good idea to use the page's type (this.GetType()) for the type.</span></span>

`key (string)`

<span data-ttu-id="fb6e3-491">***Ключ*** аргумент — это определяемые пользователем ключ для скрипта.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-491">The ***key*** argument is a user-defined key for the script.</span></span> <span data-ttu-id="fb6e3-492">Это должно быть уникальным для каждого сценария.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-492">This should be unique for each script.</span></span> <span data-ttu-id="fb6e3-493">Если попытаться добавить скрипт с один и тот же ключ и тип уже добавленных сценария, он не будет добавлен.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-493">If you attempt to add a script with the same key and type of an already added script, it will not be added.</span></span>

`script (string)`

<span data-ttu-id="fb6e3-494">***Сценарий*** аргумент — строка, содержащая фактический сценарий для добавления.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-494">The ***script*** argument is a string containing the actual script to add.</span></span> <span data-ttu-id="fb6e3-495">Рекомендуется использовать для создания скрипта и затем с помощью метода ToString() на StringBuilder присваивать StringBuilder ***сценарий*** аргумент.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-495">It's recommended that you use a StringBuilder to create the script and then use the ToString() method on the StringBuilder to assign the ***script*** argument.</span></span>

<span data-ttu-id="fb6e3-496">При использовании перегруженных RegisterClientScriptBlock, которая принимает только три аргумента, должен включать элементы сценария (&lt;сценарий&gt; и &lt;/script&gt;) в скрипте.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-496">If you use the overloaded RegisterClientScriptBlock that only takes three arguments, you must include script elements (&lt;script&gt; and &lt;/script&gt;) in your script.</span></span>

<span data-ttu-id="fb6e3-497">Вы можете использовать перегрузку, принимающую четвертый аргумент RegisterClientScriptBlock.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-497">You may choose to use the overload of RegisterClientScriptBlock that takes a fourth argument.</span></span> <span data-ttu-id="fb6e3-498">Четвертый аргумент — логическое значение, которое указывает, должна ли ASP.NET добавлять элементы сценария для вас.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-498">The fourth argument is a Boolean that specifies whether or not ASP.NET should add script elements for you.</span></span> <span data-ttu-id="fb6e3-499">Если этот аргумент является **true**, сценарий не должно содержать элементов скрипта явным образом.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-499">If this argument is **true**, your script should not include the script elements explicitly.</span></span>

<span data-ttu-id="fb6e3-500">Метод IsClientScriptBlockRegistered используется для определения, если скрипт уже был зарегистрирован.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-500">Use the IsClientScriptBlockRegistered method to determine if a script has already been registered.</span></span> <span data-ttu-id="fb6e3-501">Это позволяет избежать попытка повторно зарегистрировать скрипт, который уже был зарегистрирован.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-501">This allows you to avoid an attempt to re-register a script that has already been registered.</span></span>

### <a name="registerclientscriptinclude-new-in-20"></a><span data-ttu-id="fb6e3-502">RegisterClientScriptInclude (введено в версии 2.0)</span><span class="sxs-lookup"><span data-stu-id="fb6e3-502">RegisterClientScriptInclude (New in 2.0)</span></span>

<span data-ttu-id="fb6e3-503">Тег RegisterClientScriptInclude создает блок скрипта, ссылки на внешний файл сценария.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-503">The RegisterClientScriptInclude tag creates a script block that links to an external script file.</span></span> <span data-ttu-id="fb6e3-504">Он имеет две перегрузки.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-504">It has two overloads.</span></span> <span data-ttu-id="fb6e3-505">Одна принимает ключ и URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-505">One takes a key and a URL.</span></span> <span data-ttu-id="fb6e3-506">Второй добавляет третий аргумент, задающий тип.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-506">The second adds a third argument specifying the type.</span></span>

<span data-ttu-id="fb6e3-507">Например следующий код приводит к возникновению ошибки блок сценария, ссылающийся на jsfunctions.js в корневой папке сценариев приложения:</span><span class="sxs-lookup"><span data-stu-id="fb6e3-507">For example, the following code generates a script block that links to jsfunctions.js in the root of the scripts folder of the application:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

<span data-ttu-id="fb6e3-508">Этот код выводит следующий код в отображаемой странице:</span><span class="sxs-lookup"><span data-stu-id="fb6e3-508">This code produces the following code in the rendered page:</span></span>

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> <span data-ttu-id="fb6e3-509">Блок сценария отображается в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-509">The script block is rendered at the bottom of the page.</span></span>


<span data-ttu-id="fb6e3-510">Метод IsClientScriptIncludeRegistered используется для определения, если скрипт уже был зарегистрирован.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-510">Use the IsClientScriptIncludeRegistered method to determine if a script has already been registered.</span></span> <span data-ttu-id="fb6e3-511">Это позволяет избежать попытка повторно зарегистрировать скрипт.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-511">This allows you to avoid an attempt to re-register a script.</span></span>

## <a name="registerstartupscript"></a><span data-ttu-id="fb6e3-512">RegisterStartupScript</span><span class="sxs-lookup"><span data-stu-id="fb6e3-512">RegisterStartupScript</span></span>

<span data-ttu-id="fb6e3-513">Метод RegisterStartupScript принимает те же аргументы, что и метод RegisterClientScriptBlock.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-513">The RegisterStartupScript method takes the same arguments as the RegisterClientScriptBlock method.</span></span> <span data-ttu-id="fb6e3-514">Сценарий зарегистрирована RegisterStartupScript выполняет после загрузки страницы, но до загрузки клиентского события.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-514">A script registered with RegisterStartupScript executes after the page loads but before the OnLoad client-side event.</span></span> <span data-ttu-id="fb6e3-515">В 1.X, скрипты, зарегистрированные с RegisterStartupScript были размещены непосредственно перед закрывающим тегом &lt;/form&gt; тег пока скрипты, зарегистрированные с RegisterClientScriptBlock были размещены сразу же после открывающего &lt;формы&gt; тег.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-515">In 1.X, scripts registered with RegisterStartupScript were placed just before the closing &lt;/form&gt; tag while scripts registered with RegisterClientScriptBlock were placed immediately after the opening &lt;form&gt; tag.</span></span> <span data-ttu-id="fb6e3-516">В ASP.NET 2.0 и размещаются непосредственно перед закрывающим тегом &lt;/form&gt; тег.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-516">In ASP.NET 2.0, both are placed immediately before the closing &lt;/form&gt; tag.</span></span>

> [!NOTE]
> <span data-ttu-id="fb6e3-517">Зарегистрировать функцию с RegisterStartupScript, эта функция не будет выполняться до явным образом вызвать его в код на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-517">If you register a function with RegisterStartupScript, that function will not execute until you explicitly call it in client-side code.</span></span>


<span data-ttu-id="fb6e3-518">Метод IsStartupScriptRegistered используется для определения, если скрипт уже был зарегистрирован и избежать попытка повторно зарегистрировать скрипт.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-518">Use the IsStartupScriptRegistered method to determine if a script has already been registered and avoid an attempt to re-register a script.</span></span>

## <a name="other-clientscriptmanager-methods"></a><span data-ttu-id="fb6e3-519">Другие методы ClientScriptManager</span><span class="sxs-lookup"><span data-stu-id="fb6e3-519">Other ClientScriptManager Methods</span></span>

<span data-ttu-id="fb6e3-520">Ниже приведены некоторые полезные методы класса ClientScriptManager.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-520">Here are some of the other useful methods of the ClientScriptManager class.</span></span>

| <span data-ttu-id="fb6e3-521">**GetCallbackEventReference**</span><span class="sxs-lookup"><span data-stu-id="fb6e3-521">**GetCallbackEventReference**</span></span> | <span data-ttu-id="fb6e3-522">См. выше в этом модуле обратные вызовы из сценария.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-522">See script callbacks earlier in this module.</span></span> |
| --- | --- |
| <span data-ttu-id="fb6e3-523">**GetPostBackClientHyperlink**</span><span class="sxs-lookup"><span data-stu-id="fb6e3-523">**GetPostBackClientHyperlink**</span></span> | <span data-ttu-id="fb6e3-524">Возвращает ссылку JavaScript (javascript:&lt;вызовите&gt;) может использоваться для отправки обратно из событий на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-524">Gets a JavaScript reference (javascript:&lt;call&gt;) that can be used to post back from a client-side event.</span></span> |
| <span data-ttu-id="fb6e3-525">**GetPostBackEventReference**</span><span class="sxs-lookup"><span data-stu-id="fb6e3-525">**GetPostBackEventReference**</span></span> | <span data-ttu-id="fb6e3-526">Возвращает строку, который может использоваться для запуска отправки обратно клиенту.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-526">Gets a string that can be used to initiate a post back from the client.</span></span> |
| <span data-ttu-id="fb6e3-527">**GetWebResourceUrl**</span><span class="sxs-lookup"><span data-stu-id="fb6e3-527">**GetWebResourceUrl**</span></span> | <span data-ttu-id="fb6e3-528">Возвращает URL-адрес ресурса, внедренного в сборку.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-528">Returns a URL to a resource that is embedded in an assembly.</span></span> <span data-ttu-id="fb6e3-529">Необходимо использовать в сочетании с **RegisterClientScriptResource**.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-529">Must be used in conjunction with **RegisterClientScriptResource**.</span></span> |
| <span data-ttu-id="fb6e3-530">**RegisterClientScriptResource**</span><span class="sxs-lookup"><span data-stu-id="fb6e3-530">**RegisterClientScriptResource**</span></span> | <span data-ttu-id="fb6e3-531">Регистрирует страницу веб-ресурса.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-531">Registers a Web resource with the page.</span></span> <span data-ttu-id="fb6e3-532">Это встроенный в сборку и может быть обработано новый дескриптор WebResource.axd ресурсов.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-532">These are resources embedded in an assembly and handled by the new WebResource.axd handler.</span></span> |
| <span data-ttu-id="fb6e3-533">**RegisterHiddenField**</span><span class="sxs-lookup"><span data-stu-id="fb6e3-533">**RegisterHiddenField**</span></span> | <span data-ttu-id="fb6e3-534">Регистрирует скрытое поле формы со страницей.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-534">Registers a hidden form field with the page.</span></span> |
| <span data-ttu-id="fb6e3-535">**RegisterOnSubmitStatement**</span><span class="sxs-lookup"><span data-stu-id="fb6e3-535">**RegisterOnSubmitStatement**</span></span> | <span data-ttu-id="fb6e3-536">Регистрирует клиентский код, который выполняется при отправке формы HTML.</span><span class="sxs-lookup"><span data-stu-id="fb6e3-536">Registers client-side code that executes when the HTML form is submitted.</span></span> |
