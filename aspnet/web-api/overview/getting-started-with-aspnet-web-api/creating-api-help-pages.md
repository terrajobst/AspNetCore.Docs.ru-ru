---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Создание страниц справки для веб-API ASP.NET | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: riande
ms.date: 04/01/2013
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: c081064a32151a71fc4f3ea407e0c48a1539432a
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913129"
---
<a name="creating-help-pages-for-aspnet-web-api"></a><span data-ttu-id="e44b7-102">Создание страниц справки для веб-API ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e44b7-102">Creating Help Pages for ASP.NET Web API</span></span>
====================
<span data-ttu-id="e44b7-103">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e44b7-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="e44b7-104">При создании веб-API, часто бывает полезно создать страницу справки, чтобы другие разработчики будете знать, как вызвать API.</span><span class="sxs-lookup"><span data-stu-id="e44b7-104">When you create a web API, it is often useful to create a help page, so that other developers will know how to call your API.</span></span> <span data-ttu-id="e44b7-105">Можно создать всю документацию вручную, но лучше автоматически заполнять максимально.</span><span class="sxs-lookup"><span data-stu-id="e44b7-105">You could create all of the documentation manually, but it is better to autogenerate as much as possible.</span></span>

<span data-ttu-id="e44b7-106">Чтобы облегчить эту задачу, веб-API ASP.NET предоставляет библиотеку для автоматического создания страниц справки во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="e44b7-106">To make this task easier, ASP.NET Web API provides a library for auto-generating help pages at run time.</span></span>

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a><span data-ttu-id="e44b7-107">Создание страниц справки API</span><span class="sxs-lookup"><span data-stu-id="e44b7-107">Creating API Help Pages</span></span>

<span data-ttu-id="e44b7-108">Установка [ASP.NET и веб-инструменты 2012.2 обновления](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="e44b7-108">Install [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="e44b7-109">Это обновление страницы справки интегрированы в шаблоне проекта веб-API.</span><span class="sxs-lookup"><span data-stu-id="e44b7-109">This update integrates help pages into the Web API project template.</span></span>

<span data-ttu-id="e44b7-110">Затем создайте новый проект ASP.NET MVC 4 и выберите шаблон проекта веб-API.</span><span class="sxs-lookup"><span data-stu-id="e44b7-110">Next, create a new ASP.NET MVC 4 project and select the Web API project template.</span></span> <span data-ttu-id="e44b7-111">Шаблон проекта создает контроллеру пример API с именем `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="e44b7-111">The project template creates an example API controller named `ValuesController`.</span></span> <span data-ttu-id="e44b7-112">Этот шаблон также создает страницы справки API.</span><span class="sxs-lookup"><span data-stu-id="e44b7-112">The template also creates the API help pages.</span></span> <span data-ttu-id="e44b7-113">Все файлы кода для страницы справки, помещаются в папку областей проекта.</span><span class="sxs-lookup"><span data-stu-id="e44b7-113">All of the code files for the help page are placed in the Areas folder of the project.</span></span>

![](creating-api-help-pages/_static/image2.png)

<span data-ttu-id="e44b7-114">При запуске приложения на домашней странице содержит ссылку на страницу справки API.</span><span class="sxs-lookup"><span data-stu-id="e44b7-114">When you run the application, the home page contains a link to the API help page.</span></span> <span data-ttu-id="e44b7-115">На домашней странице относительный путь — / Help.</span><span class="sxs-lookup"><span data-stu-id="e44b7-115">From the home page, the relative path is /Help.</span></span>

![](creating-api-help-pages/_static/image3.png)

<span data-ttu-id="e44b7-116">Эту ссылку, откроется страница сводки API.</span><span class="sxs-lookup"><span data-stu-id="e44b7-116">This link brings you to an API summary page.</span></span>

![](creating-api-help-pages/_static/image4.png)

<span data-ttu-id="e44b7-117">Представление MVC для этой страницы определяется в Areas/HelpPage/Views/Help/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="e44b7-117">The MVC view for this page is defined in Areas/HelpPage/Views/Help/Index.cshtml.</span></span> <span data-ttu-id="e44b7-118">Вы можете изменить эту страницу для изменения макета, введение, title, стили и т. д.</span><span class="sxs-lookup"><span data-stu-id="e44b7-118">You can edit this page to modify the layout, introduction, title, styles, and so forth.</span></span>

<span data-ttu-id="e44b7-119">Основную часть страницы — это таблица, API-интерфейсов, сгруппированные по контроллера.</span><span class="sxs-lookup"><span data-stu-id="e44b7-119">The main part of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="e44b7-120">Записи таблицы создаются динамически, с помощью **IApiExplorer** интерфейс.</span><span class="sxs-lookup"><span data-stu-id="e44b7-120">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="e44b7-121">(Я вкратце расскажу об этом интерфейсе позже.) При добавлении нового контроллера API, таблица автоматически обновляется во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="e44b7-121">(I'll talk more about this interface later.) If you add a new API controller, the table is automatically updated at run time.</span></span>

<span data-ttu-id="e44b7-122">В столбце «API» перечислены метод HTTP и относительного URI.</span><span class="sxs-lookup"><span data-stu-id="e44b7-122">The "API" column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="e44b7-123">Столбец «Description» содержит документацию для каждого API.</span><span class="sxs-lookup"><span data-stu-id="e44b7-123">The "Description" column contains documentation for each API.</span></span> <span data-ttu-id="e44b7-124">Изначально документация является просто текст заполнителя.</span><span class="sxs-lookup"><span data-stu-id="e44b7-124">Initially, the documentation is just placeholder text.</span></span> <span data-ttu-id="e44b7-125">В следующем разделе я покажу, как добавить документацию из комментариев XML.</span><span class="sxs-lookup"><span data-stu-id="e44b7-125">In the next section, I'll show you how to add documentation from XML comments.</span></span>

<span data-ttu-id="e44b7-126">Каждый API содержит ссылку на страницу с более подробные сведения, включая пример текста запросов и ответов.</span><span class="sxs-lookup"><span data-stu-id="e44b7-126">Each API has a link to a page with more detailed information, including example request and response bodies.</span></span>

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a><span data-ttu-id="e44b7-127">Добавление страницы справки в существующий проект</span><span class="sxs-lookup"><span data-stu-id="e44b7-127">Adding Help Pages to an Existing Project</span></span>

<span data-ttu-id="e44b7-128">Страницы справки можно добавить в существующий проект веб-API с помощью диспетчера пакетов NuGet.</span><span class="sxs-lookup"><span data-stu-id="e44b7-128">You can add help pages to an existing Web API project by using NuGet Package Manager.</span></span> <span data-ttu-id="e44b7-129">Этот параметр полезен, можно начать с шаблона другому проекту от шаблона «Веб-API».</span><span class="sxs-lookup"><span data-stu-id="e44b7-129">This option is useful you start from a different project template than the "Web API" template.</span></span>

<span data-ttu-id="e44b7-130">Из **средства** меню, выберите **диспетчер пакетов NuGet**, а затем выберите **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="e44b7-130">From the **Tools** menu, select **NuGet Package Manager**, and then select **Package Manager Console**.</span></span> <span data-ttu-id="e44b7-131">В [консоль диспетчера пакетов](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) окно, введите одну из следующих команд:</span><span class="sxs-lookup"><span data-stu-id="e44b7-131">In the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) window, type one of the following commands:</span></span>

<span data-ttu-id="e44b7-132">Для **C#** приложения: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span><span class="sxs-lookup"><span data-stu-id="e44b7-132">For a **C#** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span></span>

<span data-ttu-id="e44b7-133">Для **Visual Basic** приложения: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span><span class="sxs-lookup"><span data-stu-id="e44b7-133">For a **Visual Basic** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span></span>

<span data-ttu-id="e44b7-134">Существует два пакета, для C# и один для Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="e44b7-134">There are two packages, one for C# and one for Visual Basic.</span></span> <span data-ttu-id="e44b7-135">Убедитесь в том, что использовать ту, которая соответствует проекту.</span><span class="sxs-lookup"><span data-stu-id="e44b7-135">Make sure to use the one that matches your project.</span></span>

<span data-ttu-id="e44b7-136">Эта команда устанавливает необходимые сборки и добавляет в представления MVC для страниц справки (расположенный в папке области/HelpPage).</span><span class="sxs-lookup"><span data-stu-id="e44b7-136">This command installs the necessary assemblies and adds the MVC views for the help pages (located in the Areas/HelpPage folder).</span></span> <span data-ttu-id="e44b7-137">Необходимо вручную добавить ссылку на страницу справки.</span><span class="sxs-lookup"><span data-stu-id="e44b7-137">You'll need to manually add a link to the Help page.</span></span> <span data-ttu-id="e44b7-138">Данный URI является/Help.</span><span class="sxs-lookup"><span data-stu-id="e44b7-138">The URI is /Help.</span></span> <span data-ttu-id="e44b7-139">Чтобы создать связь в представлении razor, добавьте следующие строки:</span><span class="sxs-lookup"><span data-stu-id="e44b7-139">To create a link in a razor view, add the following:</span></span>

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

<span data-ttu-id="e44b7-140">Кроме того убедитесь, что для регистрации области.</span><span class="sxs-lookup"><span data-stu-id="e44b7-140">Also, make sure to register areas.</span></span> <span data-ttu-id="e44b7-141">В файле Global.asax, добавьте следующий код, чтобы **приложения\_запустить** метод, если он еще не существует:</span><span class="sxs-lookup"><span data-stu-id="e44b7-141">In the Global.asax file, add the following code to the **Application\_Start** method, if it is not there already:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a><span data-ttu-id="e44b7-142">Добавление документации по API</span><span class="sxs-lookup"><span data-stu-id="e44b7-142">Adding API Documentation</span></span>

<span data-ttu-id="e44b7-143">По умолчанию с помощью страницы имеют заполнитель строки для документации.</span><span class="sxs-lookup"><span data-stu-id="e44b7-143">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="e44b7-144">Можно использовать [комментарии XML-документации](https://msdn.microsoft.com/library/b2s063f7.aspx) документации.</span><span class="sxs-lookup"><span data-stu-id="e44b7-144">You can use [XML documentation comments](https://msdn.microsoft.com/library/b2s063f7.aspx) to create the documentation.</span></span> <span data-ttu-id="e44b7-145">Чтобы включить эту функцию, откройте файл областей, приложения или HelpPage\_Start/HelpPageConfig.cs и раскомментируйте следующую строку:</span><span class="sxs-lookup"><span data-stu-id="e44b7-145">To enable this feature, open the file Areas/HelpPage/App\_Start/HelpPageConfig.cs and uncomment the following line:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

<span data-ttu-id="e44b7-146">Теперь можно включите XML-документации.</span><span class="sxs-lookup"><span data-stu-id="e44b7-146">Now enable XML documentation.</span></span> <span data-ttu-id="e44b7-147">В обозревателе решений щелкните правой кнопкой мыши проект и выберите **свойства**.</span><span class="sxs-lookup"><span data-stu-id="e44b7-147">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="e44b7-148">Выберите **построения** страницы.</span><span class="sxs-lookup"><span data-stu-id="e44b7-148">Select the **Build** page.</span></span>

![](creating-api-help-pages/_static/image6.png)

<span data-ttu-id="e44b7-149">В разделе **вывода**, проверьте **XML-файл документации**.</span><span class="sxs-lookup"><span data-stu-id="e44b7-149">Under **Output**, check **XML documentation file**.</span></span> <span data-ttu-id="e44b7-150">В поле ввода введите «приложение\_Data/XmlDocument.xml».</span><span class="sxs-lookup"><span data-stu-id="e44b7-150">In the edit box, type "App\_Data/XmlDocument.xml".</span></span>

![](creating-api-help-pages/_static/image7.png)

<span data-ttu-id="e44b7-151">Затем откройте код `ValuesController` контроллер API, который определен в /Controllers/ValuesControler.cs.</span><span class="sxs-lookup"><span data-stu-id="e44b7-151">Next, open the code for the `ValuesController` API controller, which is defined in /Controllers/ValuesControler.cs.</span></span> <span data-ttu-id="e44b7-152">Добавьте комментарии документации методов контроллера.</span><span class="sxs-lookup"><span data-stu-id="e44b7-152">Add some documentation comments to the controller methods.</span></span> <span data-ttu-id="e44b7-153">Пример:</span><span class="sxs-lookup"><span data-stu-id="e44b7-153">For example:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="e44b7-154">Совет: Если вы поместите курсор в строке над методом и введите три косые черты, Visual Studio автоматически вставляет XML-элементов.</span><span class="sxs-lookup"><span data-stu-id="e44b7-154">Tip: If you position the caret on the line above the method and type three forward slashes, Visual Studio automatically inserts the XML elements.</span></span> <span data-ttu-id="e44b7-155">Затем вы можете заполнить пустые поля.</span><span class="sxs-lookup"><span data-stu-id="e44b7-155">Then you can fill in the blanks.</span></span>


<span data-ttu-id="e44b7-156">Теперь сборки и снова запустить приложение и перейдите на страницы справки.</span><span class="sxs-lookup"><span data-stu-id="e44b7-156">Now build and run the application again, and navigate to the help pages.</span></span> <span data-ttu-id="e44b7-157">Строками документации появятся в таблице API.</span><span class="sxs-lookup"><span data-stu-id="e44b7-157">The documentation strings should appear in the API table.</span></span>

![](creating-api-help-pages/_static/image8.png)

<span data-ttu-id="e44b7-158">На страницу справки читает строки из XML-файла во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="e44b7-158">The help page reads the strings from the XML file at run time.</span></span> <span data-ttu-id="e44b7-159">(При развертывании приложения, убедитесь, что развертывание XML-файле.)</span><span class="sxs-lookup"><span data-stu-id="e44b7-159">(When you deploy the application, make sure to deploy the XML file.)</span></span>

## <a name="under-the-hood"></a><span data-ttu-id="e44b7-160">Взгляд изнутри</span><span class="sxs-lookup"><span data-stu-id="e44b7-160">Under the Hood</span></span>

<span data-ttu-id="e44b7-161">Страницы справки строятся на основе **ApiExplorer** класс, который является частью платформы веб-API.</span><span class="sxs-lookup"><span data-stu-id="e44b7-161">The help pages are built on top of the **ApiExplorer** class, which is part of the Web API framework.</span></span> <span data-ttu-id="e44b7-162">**ApiExplorer** класс предоставляет исходный материал для создания страницы справки.</span><span class="sxs-lookup"><span data-stu-id="e44b7-162">The **ApiExplorer** class provides the raw material for creating a help page.</span></span> <span data-ttu-id="e44b7-163">Для каждого API **ApiExplorer** содержит **ApiDescription** , описывающий API.</span><span class="sxs-lookup"><span data-stu-id="e44b7-163">For each API, **ApiExplorer** contains an **ApiDescription** that describes the API.</span></span> <span data-ttu-id="e44b7-164">Для этой цели «API» определяется как сочетание метод HTTP и относительного URI.</span><span class="sxs-lookup"><span data-stu-id="e44b7-164">For this purpose, an "API" is defined as the combination of HTTP method and relative URI.</span></span> <span data-ttu-id="e44b7-165">Например ниже приведены некоторые разные интерфейсы API.</span><span class="sxs-lookup"><span data-stu-id="e44b7-165">For example, here are some distinct APIs:</span></span>

- <span data-ttu-id="e44b7-166">ПОЛУЧИТЬ /api/Products</span><span class="sxs-lookup"><span data-stu-id="e44b7-166">GET /api/Products</span></span>
- <span data-ttu-id="e44b7-167">GET/API/продукты / {id}</span><span class="sxs-lookup"><span data-stu-id="e44b7-167">GET /api/Products/{id}</span></span>
- <span data-ttu-id="e44b7-168">POST/api/продуктов</span><span class="sxs-lookup"><span data-stu-id="e44b7-168">POST /api/Products</span></span>

<span data-ttu-id="e44b7-169">Если действие контроллера поддерживает несколько методов HTTP, **ApiExplorer** считает каждый метод различных API.</span><span class="sxs-lookup"><span data-stu-id="e44b7-169">If a controller action supports multiple HTTP methods, the **ApiExplorer** treats each method as a distinct API.</span></span>

<span data-ttu-id="e44b7-170">Чтобы скрыть интерфейс API из **ApiExplorer**, добавьте **ApiExplorerSettings** атрибута в действие и задайте *IgnoreApi* значение true.</span><span class="sxs-lookup"><span data-stu-id="e44b7-170">To hide an API from the **ApiExplorer**, add the **ApiExplorerSettings** attribute to the action and set *IgnoreApi* to true.</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

<span data-ttu-id="e44b7-171">Этот атрибут также можно добавить в контроллер, чтобы исключить всего контроллера.</span><span class="sxs-lookup"><span data-stu-id="e44b7-171">You can also add this attribute to the controller, to exclude the entire controller.</span></span>

<span data-ttu-id="e44b7-172">Класс ApiExplorer получает строк документации из **IDocumentationProvider** интерфейс.</span><span class="sxs-lookup"><span data-stu-id="e44b7-172">The ApiExplorer class gets documentation strings from the **IDocumentationProvider** interface.</span></span> <span data-ttu-id="e44b7-173">Как вы уже видели, библиотека страницы справки по предоставляет **IDocumentationProvider** , возвращает документацию из строк XML-документации.</span><span class="sxs-lookup"><span data-stu-id="e44b7-173">As you saw earlier, the Help Pages library provides an **IDocumentationProvider** that gets documentation from XML documentation strings.</span></span> <span data-ttu-id="e44b7-174">Код находится в /Areas/HelpPage/XmlDocumentationProvider.cs.</span><span class="sxs-lookup"><span data-stu-id="e44b7-174">The code is located in /Areas/HelpPage/XmlDocumentationProvider.cs.</span></span> <span data-ttu-id="e44b7-175">Можно получить документацию из другого источника, написав собственный **IDocumentationProvider**.</span><span class="sxs-lookup"><span data-stu-id="e44b7-175">You can get documentation from another source by writing your own **IDocumentationProvider**.</span></span> <span data-ttu-id="e44b7-176">Для привязывания, вызовите **SetDocumentationProvider** метод расширения, определенные в **HelpPageConfigurationExtensions**</span><span class="sxs-lookup"><span data-stu-id="e44b7-176">To wire it up, call the **SetDocumentationProvider** extension method, defined in **HelpPageConfigurationExtensions**</span></span>

<span data-ttu-id="e44b7-177">**ApiExplorer** автоматически вызывает **IDocumentationProvider** интерфейса для получения строк документации для каждого API.</span><span class="sxs-lookup"><span data-stu-id="e44b7-177">**ApiExplorer** automatically calls into the **IDocumentationProvider** interface to get documentation strings for each API.</span></span> <span data-ttu-id="e44b7-178">Она сохраняет их в **документации** свойство **ApiDescription** и **ApiParameterDescription** объектов.</span><span class="sxs-lookup"><span data-stu-id="e44b7-178">It stores them in the **Documentation** property of the **ApiDescription** and **ApiParameterDescription** objects.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e44b7-179">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="e44b7-179">Next Steps</span></span>

<span data-ttu-id="e44b7-180">Можно не только на страницы справки, показано ниже.</span><span class="sxs-lookup"><span data-stu-id="e44b7-180">You aren't limited to the help pages shown here.</span></span> <span data-ttu-id="e44b7-181">На самом деле **ApiExplorer** не ограничивается Создание страниц справки.</span><span class="sxs-lookup"><span data-stu-id="e44b7-181">In fact, **ApiExplorer** is not limited to creating help pages.</span></span> <span data-ttu-id="e44b7-182">Лин Хаун ЯО написал, что некоторые полезные сообщения в блогах читатель задумается без дополнительной настройки:</span><span class="sxs-lookup"><span data-stu-id="e44b7-182">Yao Huang Lin has written some great blog posts to get you thinking out of the box:</span></span>

- [<span data-ttu-id="e44b7-183">Добавление простой тестовый клиент в страницу справки ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="e44b7-183">Adding a simple Test Client to ASP.NET Web API Help Page</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [<span data-ttu-id="e44b7-184">Создание веб-API справки страницы ASP.NET работают на резидентные службы</span><span class="sxs-lookup"><span data-stu-id="e44b7-184">Making ASP.NET Web API Help Page work on self-hosted services</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [<span data-ttu-id="e44b7-185">Создание во время разработки справки страницы (или клиента) для веб-API ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e44b7-185">Design-time generation of help page (or client) for ASP.NET Web API</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [<span data-ttu-id="e44b7-186">Дополнительные настройки страница справки</span><span class="sxs-lookup"><span data-stu-id="e44b7-186">Advanced Help Page customizations</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
