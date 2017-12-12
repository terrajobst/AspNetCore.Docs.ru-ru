---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: "Создание и использование вспомогательного класса в ASP.NET Web Pages (Razor) сайта | Документы Microsoft"
author: tfitzmac
description: "В этой статье описывается создание вспомогательного класса в на веб-сайт ASP.NET Web Pages (Razor). Вспомогательный объект является компонентом для повторного использования, который включает код и разметку для производительности..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 5d0c1ae09d8fbc91ff76cd4045d439abafee7736
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="5cb2d-104">Создание и использование вспомогательный объект в сайт ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="5cb2d-104">Creating and Using a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="5cb2d-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5cb2d-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="5cb2d-106">В этой статье описывается создание вспомогательного класса в на веб-сайт ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="5cb2d-106">This article describes how to create a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="5cb2d-107">Объект *вспомогательный* повторно используемый компонент, который включает код и разметку, чтобы выполнить задачу, которая может оказаться трудоемкой и сложной.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="5cb2d-108">**Что вы узнаете следующее.**</span><span class="sxs-lookup"><span data-stu-id="5cb2d-108">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="5cb2d-109">Как создать и использовать простой вспомогательный.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-109">How to create and use a simple helper.</span></span>
> 
> <span data-ttu-id="5cb2d-110">Существуют следующие функции ASP.NET, представленные в статье:</span><span class="sxs-lookup"><span data-stu-id="5cb2d-110">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="5cb2d-111">`@helper` Синтаксиса.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-111">The `@helper` syntax.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="5cb2d-112">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="5cb2d-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="5cb2d-113">Веб-страниц ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="5cb2d-113">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="5cb2d-114">Этот учебник также работает с веб-страницы ASP.NET 2.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-114">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="5cb2d-115">Общие сведения о вспомогательных методов</span><span class="sxs-lookup"><span data-stu-id="5cb2d-115">Overview of Helpers</span></span>

<span data-ttu-id="5cb2d-116">Если необходимо выполнять те же задачи на разных страницах веб-узла, можно использовать вспомогательный класс.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-116">If you need to perform the same tasks on different pages in your site, you can use a helper.</span></span> <span data-ttu-id="5cb2d-117">Веб-страниц ASP.NET включает ряд вспомогательных методов, и существует много других, которые можно загрузить и установить.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-117">ASP.NET Web Pages includes a number of helpers, and there are many more that you can download and install.</span></span> <span data-ttu-id="5cb2d-118">(Список встроенных вспомогательных методов веб-страницах ASP.NET, перечислен в [краткий справочник по API ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202907).) Если ни один из существующих вспомогательные методы отвечают вашим требованиям, можно создать собственный вспомогательный.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-118">(A list of the built-in helpers in ASP.NET Web Pages is listed in the [ASP.NET API Quick Reference](https://go.microsoft.com/fwlink/?LinkId=202907).) If none of the existing helpers meet your needs, you can create your own helper.</span></span>

<span data-ttu-id="5cb2d-119">Вспомогательный класс позволяет использовать общий блок кода на нескольких страницах.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-119">A helper lets you use a common block of code across multiple pages.</span></span> <span data-ttu-id="5cb2d-120">Предположим, что на странице часто требуется создать элемент заметки, которая устанавливается отдельно от обычные абзацы.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-120">Suppose that in your page you often want to create a note item that's set apart from normal paragraphs.</span></span> <span data-ttu-id="5cb2d-121">Возможно Примечание создается как `<div>` элемент, имеет стиль поле с границей.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-121">Perhaps the note is created as a `<div>` element that's styled as a box with a border.</span></span> <span data-ttu-id="5cb2d-122">А не добавить этот же разметки страницы каждый раз будет отображаться заметки, можно упаковать разметку в качестве вспомогательного класса.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-122">Rather than add this same markup to a page every time you want to display a note, you can package the markup as a helper.</span></span> <span data-ttu-id="5cb2d-123">Можно вставить примечание с единой строки кода в любом месте он нужен.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-123">You can then insert the note with a single line of code anywhere you need it.</span></span>

<span data-ttu-id="5cb2d-124">Использование вспомогательного метода, как это делает код в каждой из страниц, проще и удобнее для чтения.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-124">Using a helper like this makes the code in each of your pages simpler and easier to read.</span></span> <span data-ttu-id="5cb2d-125">Он также упрощает для поддержания работы сайта, если вам нужно изменить вид заметки, можно изменять разметку в одном месте.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-125">It also makes it easier to maintain your site, because if you need to change how the notes look, you can change the markup in one place.</span></span>

## <a name="creating-a-helper"></a><span data-ttu-id="5cb2d-126">Создание вспомогательного метода</span><span class="sxs-lookup"><span data-stu-id="5cb2d-126">Creating a Helper</span></span>

<span data-ttu-id="5cb2d-127">Эта процедура показано, как создать помощник, который создает Обратите внимание, как описано выше.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-127">This procedure shows you how to create the helper that creates the note, as just described.</span></span> <span data-ttu-id="5cb2d-128">Это простой пример, но пользовательских вспомогательных могут включать любые разметки и кода ASP.NET, которая может потребоваться.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-128">This is a simple example, but the custom helper can include any markup and ASP.NET code that you need.</span></span>

1. <span data-ttu-id="5cb2d-129">В корневой папке веб-сайта, создайте папку с именем *приложения\_кода*.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-129">In the root folder of the website, create a folder named *App\_Code*.</span></span> <span data-ttu-id="5cb2d-130">Это имя зарезервировано в ASP.NET, можно поместить код для компонентов, как вспомогательные методы.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-130">This is a reserved folder name in ASP.NET where you can put code for components like helpers.</span></span>
2. <span data-ttu-id="5cb2d-131">В *приложения\_кода* создайте новую папку *.cshtml* файл и назовите его *MyHelpers.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-131">In the *App\_Code* folder create a new *.cshtml* file and name it *MyHelpers.cshtml*.</span></span>
3. <span data-ttu-id="5cb2d-132">Замените существующее содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="5cb2d-132">Replace the existing content with the following:</span></span>

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="5cb2d-133">Код использует `@helper` синтаксис для объявления нового вспомогательный класс с именем `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-133">The code uses the `@helper` syntax to declare a new helper named `MakeNote`.</span></span> <span data-ttu-id="5cb2d-134">Этот вспомогательный определенного позволяет передавать параметр с именем `content` , может содержать сочетание текста и разметки.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-134">This particular helper lets you pass a parameter named `content` that can contain a combination of text and markup.</span></span> <span data-ttu-id="5cb2d-135">Вспомогательное приложение вставляет строку в текст заметки с помощью `@content` переменной.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-135">The helper inserts the string into the note body using the `@content` variable.</span></span>

    <span data-ttu-id="5cb2d-136">Обратите внимание, что этот файл имеет имя *MyHelpers.cshtml*, но вспомогательное приложение называется `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-136">Notice that the file is named *MyHelpers.cshtml*, but the helper is named `MakeNote`.</span></span> <span data-ttu-id="5cb2d-137">Можно поместить несколько пользовательских вспомогательных методов в один файл.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-137">You can put multiple custom helpers into a single file.</span></span>
4. <span data-ttu-id="5cb2d-138">Сохраните и закройте файл.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-138">Save and close the file.</span></span>

## <a name="using-the-helper-in-a-page"></a><span data-ttu-id="5cb2d-139">Использование вспомогательного метода на странице</span><span class="sxs-lookup"><span data-stu-id="5cb2d-139">Using the Helper in a Page</span></span>

1. <span data-ttu-id="5cb2d-140">В корневой папке создайте новый пустой файл с именем *TestHelper.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-140">In the root folder, create a new blank file called *TestHelper.cshtml*.</span></span>
2. <span data-ttu-id="5cb2d-141">Добавьте в файл следующий код:</span><span class="sxs-lookup"><span data-stu-id="5cb2d-141">Add the following code to the file:</span></span>

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    <span data-ttu-id="5cb2d-142">Чтобы вызвать вспомогательный объект был создан, используйте `@` следуют имя файла, где — вспомогательный объект точкой и имя поддержки.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-142">To call the helper you created, use `@` followed by the file name where the helper is, a dot, and then the helper name.</span></span> <span data-ttu-id="5cb2d-143">(Если имеется несколько папок *приложения\_кода* папки, можно использовать синтаксис `@FolderName.FileName.HelperName` для вызова вспомогательное приложение в любой папке уровень вложенности).</span><span class="sxs-lookup"><span data-stu-id="5cb2d-143">(If you had multiple folders in the *App\_Code* folder, you could use the syntax `@FolderName.FileName.HelperName` to call your helper within any nested folder level).</span></span> <span data-ttu-id="5cb2d-144">Текст, добавляемый в кавычки в круглых скобках представляет собой текст, вспомогательное приложение будет отображаться как часть в примечании к веб-странице.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-144">The text that you add in quotation marks within the parentheses is the text that the helper will display as part of the note in the web page.</span></span>
3. <span data-ttu-id="5cb2d-145">Сохраните страницу и запустите его в браузере.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-145">Save the page and run it in a browser.</span></span> <span data-ttu-id="5cb2d-146">Вспомогательное приложение создает элемент Заметки справа где вызывается вспомогательный объект: между двумя абзацами.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-146">The helper generates the note item right where you called the helper: between the two paragraphs.</span></span>

    ![Снимок экрана со страницей в браузере, и как вспомогательный объект создаются разметку, которая помещает поле с указанным текстом.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a><span data-ttu-id="5cb2d-148">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="5cb2d-148">Additional Resources</span></span>


<span data-ttu-id="5cb2d-149">[Горизонтальная меню как вспомогательный Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span><span class="sxs-lookup"><span data-stu-id="5cb2d-149">[Horizontal menu as a Razor helper](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span></span> <span data-ttu-id="5cb2d-150">Запись в блоге по Майк эти протоколы демонстрируется создание горизонтальной меню как вспомогательный объект с помощью разметки, CSS и кода.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-150">This blog entry by Mike Pope shows how to create a horizontal menu as a helper using markup, CSS, and code.</span></span>

<span data-ttu-id="5cb2d-151">[Использование HTML5 в ASP.NET Web Pages вспомогательные методы для WebMatrix и ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span><span class="sxs-lookup"><span data-stu-id="5cb2d-151">[Leveraging HTML5 in ASP.NET Web Pages Helpers for WebMatrix and ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span></span> <span data-ttu-id="5cb2d-152">Вспомогательный класс, который выполняет визуализацию HTML5 показывает запись в блоге, что Sam `Canvas` элемента.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-152">This blog entry by Sam Abraham shows a helper that renders an HTML5 `Canvas` element.</span></span>

<span data-ttu-id="5cb2d-153">[Разница между @Helpers и @Functions в WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span><span class="sxs-lookup"><span data-stu-id="5cb2d-153">[The Difference Between @Helpers and @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span></span> <span data-ttu-id="5cb2d-154">Описывает, Майк Бринд данной записи блога `@helper` синтаксис и `@function` синтаксис и когда следует использовать каждое.</span><span class="sxs-lookup"><span data-stu-id="5cb2d-154">This blog entry by Mike Brind describes `@helper` syntax and `@function` syntax and when to use each.</span></span>
