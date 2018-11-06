---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Создание и использование вспомогательного метода в веб-ASP.NET страниц узла (Razor) | Документация Майкрософт
author: Rick-Anderson
description: В этой статье описывается создание вспомогательного метода в на веб-сайте ASP.NET Web Pages (Razor). Помощник представляет собой многократно используемый компонент включает код и разметку для производительности...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 3b54ba8a35a354eb0562a47866cd8b5dcb66bc86
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021512"
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="f244b-104">Создание и использование вспомогательного метода на сайте ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="f244b-104">Creating and Using a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="f244b-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f244b-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f244b-106">В этой статье описывается создание вспомогательного метода в на веб-сайте ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="f244b-106">This article describes how to create a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="f244b-107">Объект *вспомогательный* является многоразовым компонентом, который включает код и разметку, чтобы выполнить задачу, которая может быть утомительным или сложной.</span><span class="sxs-lookup"><span data-stu-id="f244b-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="f244b-108">**Вы узнаете, как:**</span><span class="sxs-lookup"><span data-stu-id="f244b-108">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="f244b-109">Как создать и использовать простую вспомогательную.</span><span class="sxs-lookup"><span data-stu-id="f244b-109">How to create and use a simple helper.</span></span>
> 
> <span data-ttu-id="f244b-110">Ниже приведены функции ASP.NET, представленные в этой статье.</span><span class="sxs-lookup"><span data-stu-id="f244b-110">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="f244b-111">`@helper` Синтаксис.</span><span class="sxs-lookup"><span data-stu-id="f244b-111">The `@helper` syntax.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f244b-112">Версии программного обеспечения, используемые в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="f244b-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f244b-113">Веб-страниц ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="f244b-113">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="f244b-114">Этот учебник также работает с ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="f244b-114">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="f244b-115">Общие сведения о вспомогательных функций</span><span class="sxs-lookup"><span data-stu-id="f244b-115">Overview of Helpers</span></span>

<span data-ttu-id="f244b-116">Если вам нужно выполнять те же задачи на разных страницах сайта, можно использовать вспомогательный объект.</span><span class="sxs-lookup"><span data-stu-id="f244b-116">If you need to perform the same tasks on different pages in your site, you can use a helper.</span></span> <span data-ttu-id="f244b-117">Веб-страниц ASP.NET включает в себя ряд вспомогательных методов, и существуют многие другие, которые можно загрузить и установить.</span><span class="sxs-lookup"><span data-stu-id="f244b-117">ASP.NET Web Pages includes a number of helpers, and there are many more that you can download and install.</span></span> <span data-ttu-id="f244b-118">(Список встроенных вспомогательных функций в веб-страниц ASP.NET, перечисленные в [краткий справочник по API ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202907).) Если ни один из существующих вспомогательных методов соответствует вашим потребностям, можно создать собственный вспомогательный.</span><span class="sxs-lookup"><span data-stu-id="f244b-118">(A list of the built-in helpers in ASP.NET Web Pages is listed in the [ASP.NET API Quick Reference](https://go.microsoft.com/fwlink/?LinkId=202907).) If none of the existing helpers meet your needs, you can create your own helper.</span></span>

<span data-ttu-id="f244b-119">Вспомогательный объект позволяет использовать общие блока кода на нескольких страницах.</span><span class="sxs-lookup"><span data-stu-id="f244b-119">A helper lets you use a common block of code across multiple pages.</span></span> <span data-ttu-id="f244b-120">Предположим, что на странице часто требуется создать элемент заметки, который задается помимо обычные абзацы.</span><span class="sxs-lookup"><span data-stu-id="f244b-120">Suppose that in your page you often want to create a note item that's set apart from normal paragraphs.</span></span> <span data-ttu-id="f244b-121">Возможно, создается ли заметка как `<div>` элемент, имеет стиль окно с границей.</span><span class="sxs-lookup"><span data-stu-id="f244b-121">Perhaps the note is created as a `<div>` element that's styled as a box with a border.</span></span> <span data-ttu-id="f244b-122">Вместо добавления этой же разметки на страницу каждый раз будет отображаться примечание, вы можете упаковать разметку как вспомогательное средство.</span><span class="sxs-lookup"><span data-stu-id="f244b-122">Rather than add this same markup to a page every time you want to display a note, you can package the markup as a helper.</span></span> <span data-ttu-id="f244b-123">Затем можно вставить примечание с помощью одной строки кода в любом месте он вам нужен.</span><span class="sxs-lookup"><span data-stu-id="f244b-123">You can then insert the note with a single line of code anywhere you need it.</span></span>

<span data-ttu-id="f244b-124">Использование вспомогательного метода, как это делает код в каждой из страниц, проще и удобнее для чтения.</span><span class="sxs-lookup"><span data-stu-id="f244b-124">Using a helper like this makes the code in each of your pages simpler and easier to read.</span></span> <span data-ttu-id="f244b-125">Он также упрощает для поддержания работы сайта, поскольку если вам нужно изменить вид заметки, можно изменить разметку в одном месте.</span><span class="sxs-lookup"><span data-stu-id="f244b-125">It also makes it easier to maintain your site, because if you need to change how the notes look, you can change the markup in one place.</span></span>

## <a name="creating-a-helper"></a><span data-ttu-id="f244b-126">Создание вспомогательного метода</span><span class="sxs-lookup"><span data-stu-id="f244b-126">Creating a Helper</span></span>

<span data-ttu-id="f244b-127">Эта процедура показано, как создать помощник, который создает Обратите внимание, как описано выше.</span><span class="sxs-lookup"><span data-stu-id="f244b-127">This procedure shows you how to create the helper that creates the note, as just described.</span></span> <span data-ttu-id="f244b-128">Это простой пример, но может содержать пользовательских вспомогательных разметки и кода ASP.NET, который требуется.</span><span class="sxs-lookup"><span data-stu-id="f244b-128">This is a simple example, but the custom helper can include any markup and ASP.NET code that you need.</span></span>

1. <span data-ttu-id="f244b-129">В корневой папке веб-сайта, создайте папку с именем *приложения\_кода*.</span><span class="sxs-lookup"><span data-stu-id="f244b-129">In the root folder of the website, create a folder named *App\_Code*.</span></span> <span data-ttu-id="f244b-130">Это имя зарезервировано в ASP.NET, в котором можно разместить код для компонентов, таких как вспомогательные функции.</span><span class="sxs-lookup"><span data-stu-id="f244b-130">This is a reserved folder name in ASP.NET where you can put code for components like helpers.</span></span>
2. <span data-ttu-id="f244b-131">В *приложения\_кода* создайте новую папку *.cshtml* файл и назовите его *MyHelpers.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f244b-131">In the *App\_Code* folder create a new *.cshtml* file and name it *MyHelpers.cshtml*.</span></span>
3. <span data-ttu-id="f244b-132">Замените существующее содержимое следующим:</span><span class="sxs-lookup"><span data-stu-id="f244b-132">Replace the existing content with the following:</span></span>

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="f244b-133">Код использует `@helper` синтаксис для объявления нового вспомогательный объект с именем `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="f244b-133">The code uses the `@helper` syntax to declare a new helper named `MakeNote`.</span></span> <span data-ttu-id="f244b-134">Этого конкретного вспомогательного объекта позволяет передавать параметр с именем `content` , может содержать сочетание текста и разметки.</span><span class="sxs-lookup"><span data-stu-id="f244b-134">This particular helper lets you pass a parameter named `content` that can contain a combination of text and markup.</span></span> <span data-ttu-id="f244b-135">Вспомогательное приложение вставляет строку в тексте Примечание с помощью `@content` переменной.</span><span class="sxs-lookup"><span data-stu-id="f244b-135">The helper inserts the string into the note body using the `@content` variable.</span></span>

    <span data-ttu-id="f244b-136">Обратите внимание на то, что этот файл имеет имя *MyHelpers.cshtml*, но вспомогательное приложение называется `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="f244b-136">Notice that the file is named *MyHelpers.cshtml*, but the helper is named `MakeNote`.</span></span> <span data-ttu-id="f244b-137">Несколько настраиваемых вспомогательных методов можно поместить в один файл.</span><span class="sxs-lookup"><span data-stu-id="f244b-137">You can put multiple custom helpers into a single file.</span></span>
4. <span data-ttu-id="f244b-138">Сохраните и закройте файл.</span><span class="sxs-lookup"><span data-stu-id="f244b-138">Save and close the file.</span></span>

## <a name="using-the-helper-in-a-page"></a><span data-ttu-id="f244b-139">Использование вспомогательного метода на странице</span><span class="sxs-lookup"><span data-stu-id="f244b-139">Using the Helper in a Page</span></span>

1. <span data-ttu-id="f244b-140">В корневой папке создайте новый пустой файл с именем *TestHelper.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f244b-140">In the root folder, create a new blank file called *TestHelper.cshtml*.</span></span>
2. <span data-ttu-id="f244b-141">Добавьте в файл следующий код:</span><span class="sxs-lookup"><span data-stu-id="f244b-141">Add the following code to the file:</span></span>

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    <span data-ttu-id="f244b-142">Чтобы вызвать вспомогательный метод, вы создали, используйте `@` следуют вспомогательного приложения где, точка, имя файла, а затем имя вспомогательного.</span><span class="sxs-lookup"><span data-stu-id="f244b-142">To call the helper you created, use `@` followed by the file name where the helper is, a dot, and then the helper name.</span></span> <span data-ttu-id="f244b-143">(При наличии нескольких папок в *приложения\_кода* папки, можно использовать синтаксис `@FolderName.FileName.HelperName` для вызова вспомогательное приложение в любой папке уровень вложенности).</span><span class="sxs-lookup"><span data-stu-id="f244b-143">(If you had multiple folders in the *App\_Code* folder, you could use the syntax `@FolderName.FileName.HelperName` to call your helper within any nested folder level).</span></span> <span data-ttu-id="f244b-144">Текст, добавляемый в кавычки в круглых скобках является текста для отображения вспомогательное приложение как часть примечания в веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="f244b-144">The text that you add in quotation marks within the parentheses is the text that the helper will display as part of the note in the web page.</span></span>
3. <span data-ttu-id="f244b-145">Сохраните страницу и запустите его в браузере.</span><span class="sxs-lookup"><span data-stu-id="f244b-145">Save the page and run it in a browser.</span></span> <span data-ttu-id="f244b-146">Вспомогательная функция создает элемент Заметки справа где вызывается вспомогательный метод: между двумя абзацами.</span><span class="sxs-lookup"><span data-stu-id="f244b-146">The helper generates the note item right where you called the helper: between the two paragraphs.</span></span>

    ![Снимок экрана, показывающий страницу в браузере, и способ вспомогательный метод создания разметки, который помещает рамку вокруг указанного текста.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a><span data-ttu-id="f244b-148">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="f244b-148">Additional Resources</span></span>


<span data-ttu-id="f244b-149">[Меню по горизонтали как вспомогательное средство Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span><span class="sxs-lookup"><span data-stu-id="f244b-149">[Horizontal menu as a Razor helper](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span></span> <span data-ttu-id="f244b-150">Этой записи блога, эти протоколы Майк показано, как создать меню по горизонтали как вспомогательное средство, с помощью разметки, CSS и код.</span><span class="sxs-lookup"><span data-stu-id="f244b-150">This blog entry by Mike Pope shows how to create a horizontal menu as a helper using markup, CSS, and code.</span></span>

<span data-ttu-id="f244b-151">[Использование HTML5 в ASP.NET Web Pages вспомогательные функции для WebMatrix и ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span><span class="sxs-lookup"><span data-stu-id="f244b-151">[Leveraging HTML5 in ASP.NET Web Pages Helpers for WebMatrix and ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span></span> <span data-ttu-id="f244b-152">Этой записи блога, что Sam показывает вспомогательный объект, который выполняет визуализацию HTML5 `Canvas` элемент.</span><span class="sxs-lookup"><span data-stu-id="f244b-152">This blog entry by Sam Abraham shows a helper that renders an HTML5 `Canvas` element.</span></span>

<span data-ttu-id="f244b-153">[Разница между @Helpers и @Functions в WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span><span class="sxs-lookup"><span data-stu-id="f244b-153">[The Difference Between @Helpers and @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span></span> <span data-ttu-id="f244b-154">Описание этой записи блога, Майк Бринд `@helper` синтаксис и `@function` синтаксис и когда следует использовать каждый.</span><span class="sxs-lookup"><span data-stu-id="f244b-154">This blog entry by Mike Brind describes `@helper` syntax and `@function` syntax and when to use each.</span></span>
