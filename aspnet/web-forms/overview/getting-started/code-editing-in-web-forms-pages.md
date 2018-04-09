---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: Редактирование кода веб-форм ASP.NET в Visual Studio 2013 | Документы Microsoft
author: Erikre
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 79b10df04432490d6338dadb8f7ddd36192beb3e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a><span data-ttu-id="0ae31-102">Код редактирования веб-форм ASP.NET в Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="0ae31-102">Code Editing ASP.NET Web Forms in Visual Studio 2013</span></span>
====================
<span data-ttu-id="0ae31-103">по [Эрик Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="0ae31-103">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="0ae31-104">Во многих страницах веб-форма ASP.NET код пишется в Visual Basic, C# или другого языка.</span><span class="sxs-lookup"><span data-stu-id="0ae31-104">In many ASP.NET Web Form pages, you write code in Visual Basic, C#, or another language.</span></span> <span data-ttu-id="0ae31-105">Редактор кода в Visual Studio может помочь быстро написать код и избежать ошибок.</span><span class="sxs-lookup"><span data-stu-id="0ae31-105">The code editor in Visual Studio can help you write code quickly while helping you avoid errors.</span></span> <span data-ttu-id="0ae31-106">Кроме того редактор предоставляет способ создания многократно используемого кода позволяет снизить объем работы, которую необходимо выполнить.</span><span class="sxs-lookup"><span data-stu-id="0ae31-106">In addition, the editor provides ways for you to create reusable code to help reduce the amount of work you need to do.</span></span>

<span data-ttu-id="0ae31-107">В этом пошаговом руководстве показаны различные возможности в редакторе кода Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0ae31-107">This walkthrough illustrates various features of the Visual Studio code editor.</span></span>

<span data-ttu-id="0ae31-108">В этом пошаговом руководстве описаны следующие процедуры.</span><span class="sxs-lookup"><span data-stu-id="0ae31-108">During this walkthrough, you will learn how to:</span></span>

- <span data-ttu-id="0ae31-109">Правильный встроенные ошибки в коде.</span><span class="sxs-lookup"><span data-stu-id="0ae31-109">Correct inline coding errors.</span></span>
- <span data-ttu-id="0ae31-110">Оптимизировать и переименовывать код.</span><span class="sxs-lookup"><span data-stu-id="0ae31-110">Refactor and rename code.</span></span>
- <span data-ttu-id="0ae31-111">Переименуйте переменные и объекты.</span><span class="sxs-lookup"><span data-stu-id="0ae31-111">Rename variables and objects.</span></span>
- <span data-ttu-id="0ae31-112">Вставка фрагментов кода.</span><span class="sxs-lookup"><span data-stu-id="0ae31-112">Insert code snippets.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0ae31-113">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="0ae31-113">Prerequisites</span></span>


<span data-ttu-id="0ae31-114">Для выполнения данного пошагового руководства требуется:</span><span class="sxs-lookup"><span data-stu-id="0ae31-114">In order to complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="0ae31-115">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) или [Microsoft Visual Studio Express 2013 для Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="0ae31-115">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="0ae31-116">Платформа .NET Framework устанавливается автоматически.</span><span class="sxs-lookup"><span data-stu-id="0ae31-116">The .NET Framework is installed automatically.</span></span> 

    > [!NOTE] 
    > 
    > <span data-ttu-id="0ae31-117">Microsoft Visual Studio 2013 и Microsoft Visual Studio Express 2013 для Web будет часто называться Visual Studio на протяжении этого учебника ряда.</span><span class="sxs-lookup"><span data-stu-id="0ae31-117">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>  
    >   
    > <span data-ttu-id="0ae31-118">При использовании Visual Studio это пошаговое руководство предполагает, что выбрана **веб-разработки** набор параметров при первом запуске Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0ae31-118">If you are using Visual Studio, this walkthrough assumes that you selected the **Web Development** collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="0ae31-119">Дополнительные сведения см. в разделе [как: выберите параметры среды веб-разработки](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="0ae31-119">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>

  <span data-ttu-id="0ae31-120">Введение в Visual Studio и ASP.NET, в разделе [Создание базовой страницы веб-форм ASP.NET 4.5 в Visual Studio 2013](creating-a-basic-web-forms-page.md).</span><span class="sxs-lookup"><span data-stu-id="0ae31-120">For an introduction to Visual Studio and ASP.NET, see [Creating a basic ASP.NET 4.5 Web Forms page in Visual Studio 2013](creating-a-basic-web-forms-page.md).</span></span>   
 

## <a name="creating-a-web-application-project-and-a-page"></a><span data-ttu-id="0ae31-121">Создание проекта веб-приложения и страницы</span><span class="sxs-lookup"><span data-stu-id="0ae31-121">Creating a Web application project and a Page</span></span>

<a id="sectionToggle0"></a>

<span data-ttu-id="0ae31-122">В этой части пошагового руководства будет создать проект веб-приложения и добавление новой страницы.</span><span class="sxs-lookup"><span data-stu-id="0ae31-122">In this part of the walkthrough, you will create a Web application project and add a new page to it.</span></span>

### <a name="to-create-a-web-application-project"></a><span data-ttu-id="0ae31-123">Чтобы создать проект веб-приложения</span><span class="sxs-lookup"><span data-stu-id="0ae31-123">To create a Web application project</span></span>

1. <span data-ttu-id="0ae31-124">Откройте Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0ae31-124">Open Microsoft Visual Studio.</span></span>
2. <span data-ttu-id="0ae31-125">В меню **Файл** выберите **Создать проект**.</span><span class="sxs-lookup"><span data-stu-id="0ae31-125">On the **File** menu, select **New Project**.</span></span>  
    <span data-ttu-id="0ae31-126">![Меню "файл"](code-editing-in-web-forms-pages/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0ae31-126">![File Menu](code-editing-in-web-forms-pages/_static/image1.png)</span></span>

    <span data-ttu-id="0ae31-127">Откроется диалоговое окно **Новый проект** .</span><span class="sxs-lookup"><span data-stu-id="0ae31-127">The **New Project** dialog box appears.</span></span>
3. <span data-ttu-id="0ae31-128">Выберите **шаблоны**  - &gt; **Visual C#**  - &gt; **Web** группа шаблонов в левой части экрана.</span><span class="sxs-lookup"><span data-stu-id="0ae31-128">Select the **Templates** -&gt; **Visual C#** -&gt; **Web** templates group on the left.</span></span>
4. <span data-ttu-id="0ae31-129">Выберите **веб-приложение ASP.NET** шаблона в центральном столбце.</span><span class="sxs-lookup"><span data-stu-id="0ae31-129">Choose the **ASP.NET Web Application** template in the center column.</span></span>
5. <span data-ttu-id="0ae31-130">Присвойте проекту имя ***BasicWebApp*** и нажмите кнопку **ОК** кнопки.</span><span class="sxs-lookup"><span data-stu-id="0ae31-130">Name your project ***BasicWebApp*** and click the **OK** button.</span></span>   
<span data-ttu-id="0ae31-131">![Диалоговое окно нового проекта](code-editing-in-web-forms-pages/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="0ae31-131">![New Project dialog box](code-editing-in-web-forms-pages/_static/image2.png)</span></span>
6. <span data-ttu-id="0ae31-132">Затем выберите **Web Forms** шаблона и нажмите кнопку **ОК** кнопку, чтобы создать проект.</span><span class="sxs-lookup"><span data-stu-id="0ae31-132">Next, select the **Web Forms** template and click the **OK** button to create the project.</span></span>  
<span data-ttu-id="0ae31-133">![Диалоговое окно Новый проект ASP.NET](code-editing-in-web-forms-pages/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="0ae31-133">![New ASP.NET Project dialog box](code-editing-in-web-forms-pages/_static/image3.png)</span></span>  

    <span data-ttu-id="0ae31-134">Visual Studio создает новый проект, включающий стандартные функциональные возможности на основе шаблона веб-форм.</span><span class="sxs-lookup"><span data-stu-id="0ae31-134">Visual Studio creates a new project that includes prebuilt functionality based on the Web Forms template.</span></span>


## <a name="creating-a-new-aspnet-web-forms-page"></a><span data-ttu-id="0ae31-135">Создание нового страница веб-форм ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0ae31-135">Creating a new ASP.NET Web Forms Page</span></span>


<span data-ttu-id="0ae31-136">При создании нового приложения веб-форм с помощью **веб-приложение ASP.NET** шаблон проекта Visual Studio добавляет страницы ASP.NET (страницы веб-форм) с именем *Default.aspx*, также как несколько файлов и папки.</span><span class="sxs-lookup"><span data-stu-id="0ae31-136">When you create a new Web Forms application using the **ASP.NET Web Application** project template, Visual Studio adds an ASP.NET page (Web Forms page) named *Default.aspx*, as well as several other files and folders.</span></span> <span data-ttu-id="0ae31-137">Можно использовать *Default.aspx* страницу в качестве домашней страницы для веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="0ae31-137">You can use the *Default.aspx* page as the home page for your Web application.</span></span> <span data-ttu-id="0ae31-138">Однако в данном пошаговом руководстве будет создать и работать с новой страницы.</span><span class="sxs-lookup"><span data-stu-id="0ae31-138">However, for this walkthrough, you will create and work with a new page.</span></span>

### <a name="to-add-a-page-to-the-web-application"></a><span data-ttu-id="0ae31-139">Добавление страницы в веб-приложение</span><span class="sxs-lookup"><span data-stu-id="0ae31-139">To add a page to the Web application</span></span>


1. <span data-ttu-id="0ae31-140">В **обозревателе решений**, щелкните правой кнопкой мыши имя веб-приложения (в этом учебнике, имя приложения — **BasicWebSite**), а затем нажмите кнопку **добавить**  - &gt; **Новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="0ae31-140">In **Solution Explorer**, right-click the Web application name (in this tutorial the application name is **BasicWebSite**), and then click **Add** -&gt; **New Item**.</span></span>   
<span data-ttu-id="0ae31-141">Откроется диалоговое окно **Добавление нового элемента**.</span><span class="sxs-lookup"><span data-stu-id="0ae31-141">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="0ae31-142">Выберите **Visual C#**  - &gt; **Web** группа шаблонов в левой части экрана.</span><span class="sxs-lookup"><span data-stu-id="0ae31-142">Select the **Visual C#** -&gt; **Web** templates group on the left.</span></span> <span data-ttu-id="0ae31-143">Выберите **веб-форму** из середины списка и назовите его *FirstWebPage.aspx*.</span><span class="sxs-lookup"><span data-stu-id="0ae31-143">Then, select **Web Form** from the middle list and name it *FirstWebPage.aspx*.</span></span>   
    <span data-ttu-id="0ae31-144">![Добавить новый элемент-диалоговое окно](code-editing-in-web-forms-pages/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="0ae31-144">![Add New Item dialog box](code-editing-in-web-forms-pages/_static/image4.png)</span></span>
3. <span data-ttu-id="0ae31-145">Нажмите кнопку **добавить** Добавление страницы веб-формы в проект.</span><span class="sxs-lookup"><span data-stu-id="0ae31-145">Click **Add** to add the Web Forms page to your project.</span></span>  
 <span data-ttu-id="0ae31-146">Visual Studio создаст новую страницу и открывает его.</span><span class="sxs-lookup"><span data-stu-id="0ae31-146">Visual Studio creates the new page and opens it.</span></span>
4. <span data-ttu-id="0ae31-147">Затем задайте этой новой странице в качестве стартовой страницы по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0ae31-147">Next, set this new page as the default startup page.</span></span> <span data-ttu-id="0ae31-148">В **обозревателе решений**, щелкните правой кнопкой мыши новую страницу с именем *FirstWebPage.aspx* и выберите **задать в качестве начальной страницы**.</span><span class="sxs-lookup"><span data-stu-id="0ae31-148">In **Solution Explorer**, right-click the new page named *FirstWebPage.aspx* and select **Set As Start Page**.</span></span> <span data-ttu-id="0ae31-149">При очередном запуске этого приложения, чтобы проверить ход работы, автоматически появится эта новая страница в браузере.</span><span class="sxs-lookup"><span data-stu-id="0ae31-149">The next time you run this application to test our progress, you will automatically see this new page in the browser.</span></span>


## <a name="correcting-inline-coding-errors"></a><span data-ttu-id="0ae31-150">Исправление ошибки в коде встроенной</span><span class="sxs-lookup"><span data-stu-id="0ae31-150">Correcting Inline Coding Errors</span></span>


<span data-ttu-id="0ae31-151">Редактор кода в Visual Studio помогает избежать ошибок при написании кода и при внесении ошибки редактор кода позволяет исправить ошибку.</span><span class="sxs-lookup"><span data-stu-id="0ae31-151">The code editor in Visual Studio helps you to avoid errors as you write code, and if you have made an error, the code editor helps you to correct the error.</span></span> <span data-ttu-id="0ae31-152">В этой части пошагового руководства вы напишете строку кода, иллюстрирующие возможности исправления ошибок в редакторе.</span><span class="sxs-lookup"><span data-stu-id="0ae31-152">In this part of the walkthrough, you will write a line of code that illustrate the error correction features in the editor.</span></span>

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a><span data-ttu-id="0ae31-153">Исправление ошибок простого кода в Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ae31-153">To correct simple coding errors in Visual Studio</span></span>


1. <span data-ttu-id="0ae31-154">В **разработки** дважды щелкните пустую страницу, чтобы создать обработчик для **нагрузки** событий для страницы.</span><span class="sxs-lookup"><span data-stu-id="0ae31-154">In **Design** view, double-click the blank page to create a handler for the **Load** event for the page.</span></span>   
   <span data-ttu-id="0ae31-155">Обработчик событий используется только как место для написания кода.</span><span class="sxs-lookup"><span data-stu-id="0ae31-155">You are using the event handler only as a place to write some code.</span></span>
2. <span data-ttu-id="0ae31-156">В обработчике, введите следующую строку, которая содержит ошибку и нажмите клавишу **ввод**:</span><span class="sxs-lookup"><span data-stu-id="0ae31-156">Inside the handler, type the following line that contains an error and press **ENTER**:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

   <span data-ttu-id="0ae31-157">При нажатии клавиши **ввод**, редактор кода помещает зеленый и красное подчеркивание (часто вызывать &quot;волнистая&quot; строки) в группе областей в коде, имеющие проблемы.</span><span class="sxs-lookup"><span data-stu-id="0ae31-157">When you press **ENTER**, the code editor places green and red underlines (commonly call &quot;squiggly&quot; lines) under areas of the code that have issues.</span></span> <span data-ttu-id="0ae31-158">Предупреждение о зеленой линией.</span><span class="sxs-lookup"><span data-stu-id="0ae31-158">A green underline indicates a warning.</span></span> <span data-ttu-id="0ae31-159">Красная линия указывает, необходимо исправить ошибки.</span><span class="sxs-lookup"><span data-stu-id="0ae31-159">A red underline indicates an error that you must fix.</span></span> 

    <span data-ttu-id="0ae31-160">Наведите указатель мыши на `myStr` чтобы увидеть подсказку, информирующее о предупреждении.</span><span class="sxs-lookup"><span data-stu-id="0ae31-160">Hold the mouse pointer over `myStr` to see a tooltip that tells you about the warning.</span></span> <span data-ttu-id="0ae31-161">Кроме того можно навести указатель мыши на красной линией, чтобы просмотреть сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="0ae31-161">Also, hold your mouse pointer over the red underline to see the error message.</span></span>

    <span data-ttu-id="0ae31-162">Ниже приведен код с подчеркивание.</span><span class="sxs-lookup"><span data-stu-id="0ae31-162">The following image shows the code with the underlines.</span></span>

    <span data-ttu-id="0ae31-163">![Текст приветствия в режиме конструктора](code-editing-in-web-forms-pages/_static/image5.png "текст приветствия в режиме конструктора")</span><span class="sxs-lookup"><span data-stu-id="0ae31-163">![Welcome text in Design view](code-editing-in-web-forms-pages/_static/image5.png "Welcome text in Design view")</span></span>  
   <span data-ttu-id="0ae31-164">Ошибка должна быть исправлена путем добавления запятой `;` до конца строки.</span><span class="sxs-lookup"><span data-stu-id="0ae31-164">The error must be fixed by adding a semicolon `;` to the end of the line.</span></span> <span data-ttu-id="0ae31-165">Предупреждение просто уведомляет вы не использовали `myStr` еще переменной.</span><span class="sxs-lookup"><span data-stu-id="0ae31-165">The warning simply notifies you that you haven't used the `myStr` variable yet.</span></span>  

    > [!NOTE] 
    > 
    > <span data-ttu-id="0ae31-166">Просмотреть текущий код форматирования параметров в Visual Studio, выбрав **средства**  - &gt; **параметры**  - &gt; **шрифты и Цвета**.</span><span class="sxs-lookup"><span data-stu-id="0ae31-166">You view your current code formatting settings in Visual Studio by selecting **Tools** -&gt; **Options** -&gt; **Fonts and Colors**.</span></span>


## <a name="refactoring-and-renaming"></a><span data-ttu-id="0ae31-167">Оптимизация кода и переименование</span><span class="sxs-lookup"><span data-stu-id="0ae31-167">Refactoring and Renaming</span></span>

<span data-ttu-id="0ae31-168">Рефакторинг — программным методом, включающим реструктуризацию кода, чтобы сделать проще для понимания и обслуживания, сохраняя его функциональность.</span><span class="sxs-lookup"><span data-stu-id="0ae31-168">Refactoring is a software methodology that involves restructuring your code to make it easier to understand and to maintain, while preserving its functionality.</span></span> <span data-ttu-id="0ae31-169">Простой пример может быть писать код в обработчик событий для получения данных из базы данных.</span><span class="sxs-lookup"><span data-stu-id="0ae31-169">A simple example might be that you write code in an event handler to get data from a database.</span></span> <span data-ttu-id="0ae31-170">При разработке на странице, вы обнаружите, требуется доступ к данным из нескольких разных обработчиков.</span><span class="sxs-lookup"><span data-stu-id="0ae31-170">As you develop your page, you discover that you need to access the data from several different handlers.</span></span> <span data-ttu-id="0ae31-171">Таким образом путем создания метода доступа к данным на странице и вставка вызовы метода в обработчиках рефакторинга кода страницы.</span><span class="sxs-lookup"><span data-stu-id="0ae31-171">Therefore, you refactor the page's code by creating a data-access method in the page and inserting calls to the method in the handlers.</span></span>

<span data-ttu-id="0ae31-172">Редактор кода включает средства, которые помогают выполнять различные задачи оптимизации кода.</span><span class="sxs-lookup"><span data-stu-id="0ae31-172">The code editor includes tools to help you perform various refactoring tasks.</span></span> <span data-ttu-id="0ae31-173">В этом пошаговом руководстве будет работать два метода оптимизации кода: переименование переменных, а также методы извлечения.</span><span class="sxs-lookup"><span data-stu-id="0ae31-173">In this walkthrough, you will work with two refactoring techniques: renaming variables and extracting methods.</span></span> <span data-ttu-id="0ae31-174">Другие параметры оптимизации кода включают инкапсуляцию полей, превращение локальных переменных в параметры метода и управление параметрами метода.</span><span class="sxs-lookup"><span data-stu-id="0ae31-174">Other refactoring options include encapsulating fields, promoting local variables to method parameters, and managing method parameters.</span></span> <span data-ttu-id="0ae31-175">Доступность этих параметров оптимизации зависит от местоположения в коде.</span><span class="sxs-lookup"><span data-stu-id="0ae31-175">The availability of these refactoring options depends on the location in the code.</span></span>

### <a name="refactoring-code"></a><span data-ttu-id="0ae31-176">Рефакторинг кода</span><span class="sxs-lookup"><span data-stu-id="0ae31-176">Refactoring Code</span></span>

<span data-ttu-id="0ae31-177">Обычным сценарием оптимизации кода является создание (извлечение) метода из кода, который находится внутри другого элемента, например, метода.</span><span class="sxs-lookup"><span data-stu-id="0ae31-177">A common refactoring scenario is to create (extract) a method from code that is inside another member, such as a method.</span></span> <span data-ttu-id="0ae31-178">Это уменьшает размер исходного элемента и делает извлеченного кода для повторного использования.</span><span class="sxs-lookup"><span data-stu-id="0ae31-178">This reduces the size of the original member and makes the extracted code reusable.</span></span>

<span data-ttu-id="0ae31-179">В этой части пошагового руководства будет написан простой код и затем извлечь метод из него.</span><span class="sxs-lookup"><span data-stu-id="0ae31-179">In this part of the walkthrough, you will write some simple code, and then extract a method from it.</span></span> <span data-ttu-id="0ae31-180">Оптимизация кода поддерживается для C#, поэтому вы создадите страницы, который используется в качестве языка программирования C#.</span><span class="sxs-lookup"><span data-stu-id="0ae31-180">Refactoring is supported for C#, so you will create a page that uses C# as its programming language.</span></span>

### <a name="to-extract-a-method-in-a-c-page"></a><span data-ttu-id="0ae31-181">Извлечение метода на странице C#</span><span class="sxs-lookup"><span data-stu-id="0ae31-181">To extract a method in a C# page</span></span>

1. <span data-ttu-id="0ae31-182">Переключитесь в **разработки** представления.</span><span class="sxs-lookup"><span data-stu-id="0ae31-182">Switch to **Design** view.</span></span>
2. <span data-ttu-id="0ae31-183">В **элементов**, из **Стандартная** вкладки, перетащите [кнопку](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) на страницу.</span><span class="sxs-lookup"><span data-stu-id="0ae31-183">In the **Toolbox**, from the **Standard** tab, drag a [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) control onto the page.</span></span>
3. <span data-ttu-id="0ae31-184">Дважды щелкните **кнопку** управления, чтобы создать обработчик для его [нажмите кнопку](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) события, а затем добавьте следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="0ae31-184">Double-click the **Button** control to create a handler for its [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) event, and then add the following highlighted code:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

   <span data-ttu-id="0ae31-185">Код создает **ArrayList** использует цикл для загрузки его со значениями объекта и затем использует другой цикл для отображения содержимого **ArrayList** объекта.</span><span class="sxs-lookup"><span data-stu-id="0ae31-185">The code creates an **ArrayList** object, uses a loop to load it with values, and then uses another loop to display the contents of the **ArrayList** object.</span></span>
4. <span data-ttu-id="0ae31-186">Нажмите клавишу **CTRL + F5** для запуска страницы и нажмите кнопку **кнопку** чтобы убедиться в том, что можно увидеть следующие результаты:</span><span class="sxs-lookup"><span data-stu-id="0ae31-186">Press **CTRL+F5** to run the page, and then click the **button** to make sure that you see the following output:</span></span>   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. <span data-ttu-id="0ae31-187">Вернуться в редактор кода, а затем выберите следующие строки в обработчике событий.</span><span class="sxs-lookup"><span data-stu-id="0ae31-187">Return to the code editor, and then select the following lines in the event handler.</span></span>   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. <span data-ttu-id="0ae31-188">Правой кнопкой мыши щелкните **рефакторинг**, а затем выберите **извлечение метода**.</span><span class="sxs-lookup"><span data-stu-id="0ae31-188">Right-click the selection, click **Refactor**, and then choose **Extract Method**.</span></span> 

    <span data-ttu-id="0ae31-189">**Извлечение метода** откроется диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="0ae31-189">The **Extract Method** dialog box appears.</span></span>
7. <span data-ttu-id="0ae31-190">В **имя нового метода** введите **DisplayArray**, а затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="0ae31-190">In the **New Method Name** box, type **DisplayArray**, and then click **OK**.</span></span> 

    <span data-ttu-id="0ae31-191">Редактор кода создаст новый метод с именем `DisplayArray`и помещает в новый метод в вызов **щелкните** обработчика, где изначально находился цикл.</span><span class="sxs-lookup"><span data-stu-id="0ae31-191">The code editor creates a new method named `DisplayArray`, and puts a call to the new method in the **Click** handler where the loop was originally.</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. <span data-ttu-id="0ae31-192">Нажмите клавишу **CTRL + F5** снова запустить страницу, а затем нажмите кнопку **кнопку**.</span><span class="sxs-lookup"><span data-stu-id="0ae31-192">Press **CTRL+F5** to run the page again, and click the **button**.</span></span>

    <span data-ttu-id="0ae31-193">Страница работает так же, как раньше.</span><span class="sxs-lookup"><span data-stu-id="0ae31-193">The page functions the same as it did before.</span></span> <span data-ttu-id="0ae31-194">`DisplayArray` Метод теперь может быть вызов из любого места в классе страницы.</span><span class="sxs-lookup"><span data-stu-id="0ae31-194">The `DisplayArray` method can now be call from anywhere in the page class.</span></span>

## <a name="renaming-variables"></a><span data-ttu-id="0ae31-195">Переименование переменных</span><span class="sxs-lookup"><span data-stu-id="0ae31-195">Renaming Variables</span></span>

<span data-ttu-id="0ae31-196">При работе с переменными, а также объекты, можно переименовать их после уже заданы в коде.</span><span class="sxs-lookup"><span data-stu-id="0ae31-196">When you work with variables, as well as objects, you might want to rename them after they are already referenced in your code.</span></span> <span data-ttu-id="0ae31-197">Тем не менее переименование, переменные и объекты может вызвать код нарушиться, если будет пропущено переименование одной из ссылок.</span><span class="sxs-lookup"><span data-stu-id="0ae31-197">However, renaming variables and objects can cause the code to break if you miss renaming one of the references.</span></span> <span data-ttu-id="0ae31-198">Таким образом можно использовать оптимизацию кода для выполнения переименования.</span><span class="sxs-lookup"><span data-stu-id="0ae31-198">Therefore, you can use refactoring to perform the renaming.</span></span>

### <a name="to-use-refactoring-to-rename-a-variable"></a><span data-ttu-id="0ae31-199">Использование рефакторинга для переименования переменной</span><span class="sxs-lookup"><span data-stu-id="0ae31-199">To use refactoring to rename a variable</span></span>


1. <span data-ttu-id="0ae31-200">В **щелкните** обработчика событий, найдите следующую строку:</span><span class="sxs-lookup"><span data-stu-id="0ae31-200">In the **Click** event handler, locate the following line:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. <span data-ttu-id="0ae31-201">Щелкните правой кнопкой мыши имя переменной `alist`, выберите **рефакторинг**, а затем выберите **переименование**.</span><span class="sxs-lookup"><span data-stu-id="0ae31-201">Right-click the variable name `alist`, choose **Refactor**, and then choose **Rename**.</span></span>

    <span data-ttu-id="0ae31-202">**Переименование** откроется диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="0ae31-202">The **Rename** dialog box appears.</span></span>
3. <span data-ttu-id="0ae31-203">В **новое имя** введите **ArrayList1** и убедитесь, что **Предварительный просмотр изменений ссылок** установлен флажок.</span><span class="sxs-lookup"><span data-stu-id="0ae31-203">In the **New name** box, type **ArrayList1** and make sure the **Preview reference changes** checkbox has been selected.</span></span> <span data-ttu-id="0ae31-204">Затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="0ae31-204">Then click **OK**.</span></span>

    <span data-ttu-id="0ae31-205">**Просмотр изменений** диалоговое окно отображается, а отображается дерево, содержащее все ссылки на переменную, которая выполняется переименование.</span><span class="sxs-lookup"><span data-stu-id="0ae31-205">The **Preview Changes** dialog box appears, and displays a tree that contains all references to the variable that you are renaming.</span></span>
4. <span data-ttu-id="0ae31-206">Нажмите кнопку **применить** закрыть **Просмотр изменений** диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="0ae31-206">Click **Apply** to close the **Preview Changes** dialog box.</span></span>

    <span data-ttu-id="0ae31-207">Переменные, которые ссылаются специально для выбранного экземпляра будут переименованы.</span><span class="sxs-lookup"><span data-stu-id="0ae31-207">The variables that refer specifically to the instance that you selected are renamed.</span></span> <span data-ttu-id="0ae31-208">Обратите внимание, что переменная `alist` в следующей строке, не переименовывается.</span><span class="sxs-lookup"><span data-stu-id="0ae31-208">Note, however, that the variable `alist` in the following line is not renamed.</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    <span data-ttu-id="0ae31-209">Переменная `alist` в этой строке не переименован, поскольку он не представляет то же значение, что и переменная `alist` , был переименован.</span><span class="sxs-lookup"><span data-stu-id="0ae31-209">The variable `alist` in this line is not renamed because it does not represent the same value as the variable `alist` that you renamed.</span></span> <span data-ttu-id="0ae31-210">Переменная `alist` в `DisplayArray` объявление является локальной переменной для этого метода.</span><span class="sxs-lookup"><span data-stu-id="0ae31-210">The variable `alist` in the `DisplayArray` declaration is a local variable for that method.</span></span> <span data-ttu-id="0ae31-211">Это показывает, что с помощью рефакторинга для переименования переменные отличается от простого действия поиска и замены в редакторе. Рефакторинг переименовывает переменные с переменной, которой он работает с учетом семантики.</span><span class="sxs-lookup"><span data-stu-id="0ae31-211">This illustrates that using refactoring to rename variables is different than simply performing a find-and-replace action in the editor; refactoring renames variables with knowledge of the semantics of the variable that it is working with.</span></span>


## <a name="inserting-snippets"></a><span data-ttu-id="0ae31-212">Вставка фрагментов кода</span><span class="sxs-lookup"><span data-stu-id="0ae31-212">Inserting Snippets</span></span>

<span data-ttu-id="0ae31-213">Поскольку существует много задач кодирования, которые часто возникает необходимость выполнить разработчики веб-форм, редактор кода предоставляет библиотеку фрагментов, или блоков предварительно написанного кода.</span><span class="sxs-lookup"><span data-stu-id="0ae31-213">Because there are many coding tasks that Web Forms developers frequently need to perform, the code editor provides a library of snippets, or blocks of prewritten code.</span></span> <span data-ttu-id="0ae31-214">Эти фрагменты кода можно вставить в страницу.</span><span class="sxs-lookup"><span data-stu-id="0ae31-214">You can insert these snippets into your page.</span></span>

<span data-ttu-id="0ae31-215">Каждый язык, используемый в Visual Studio имеет небольшие различия в способе вставки фрагментов кода.</span><span class="sxs-lookup"><span data-stu-id="0ae31-215">Each language that you use in Visual Studio has slight differences in the way you insert code snippets.</span></span> <span data-ttu-id="0ae31-216">Сведения о вставке фрагментов см. в разделе [фрагментов кода Visual Basic](https://msdn.microsoft.com/library/18yz4be4.aspx).</span><span class="sxs-lookup"><span data-stu-id="0ae31-216">For information about inserting snippets, see [Visual Basic IntelliSense Code Snippets](https://msdn.microsoft.com/library/18yz4be4.aspx).</span></span> <span data-ttu-id="0ae31-217">Сведения о вставке фрагментов кода в Visual C# см. в разделе [фрагменты кода Visual C#](https://msdn.microsoft.com/library/z41h7fat.aspx).</span><span class="sxs-lookup"><span data-stu-id="0ae31-217">For information about inserting snippets in Visual C#, see [Visual C# Code Snippets](https://msdn.microsoft.com/library/z41h7fat.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="0ae31-218">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="0ae31-218">Next Steps</span></span>

<span data-ttu-id="0ae31-219">В этом пошаговом руководстве показаны базовые возможности редактора кода Visual Studio 2010 для исправления ошибок в коде, рефакторинга кода, переименование переменных и вставки фрагментов кода в код.</span><span class="sxs-lookup"><span data-stu-id="0ae31-219">This walkthrough has illustrated the basic features of the Visual Studio 2010 code editor for correcting errors in your code, refactoring code, renaming variables, and inserting code snippets into your code.</span></span> <span data-ttu-id="0ae31-220">Дополнительные возможности в редакторе можно сделать и ускорить разработку приложений.</span><span class="sxs-lookup"><span data-stu-id="0ae31-220">Additional features in the editor can make application development fast and easy.</span></span> <span data-ttu-id="0ae31-221">Например, можно сделать следующее:</span><span class="sxs-lookup"><span data-stu-id="0ae31-221">For example, you might want to:</span></span>

- <span data-ttu-id="0ae31-222">Дополнительные сведения о возможностях IntelliSense, таких как изменение параметров IntelliSense, управление фрагментами кода и поиск фрагментов кода в Интернете.</span><span class="sxs-lookup"><span data-stu-id="0ae31-222">Learn more about the features of IntelliSense, such as modifying IntelliSense options, managing code snippets, and searching for code snippets online.</span></span> <span data-ttu-id="0ae31-223">Дополнительные сведения см. в статье [Using IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx) (Использование IntelliSense).</span><span class="sxs-lookup"><span data-stu-id="0ae31-223">For more information, see [Using IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx).</span></span>
- <span data-ttu-id="0ae31-224">Научитесь создавать собственные фрагменты кода.</span><span class="sxs-lookup"><span data-stu-id="0ae31-224">Learn how to create your own code snippets.</span></span> <span data-ttu-id="0ae31-225">Дополнительные сведения см. в разделе [Создание и использование фрагментов кода IntelliSense](https://msdn.microsoft.com/library/ms165392.aspx)</span><span class="sxs-lookup"><span data-stu-id="0ae31-225">For more information, see [Creating and Using IntelliSense Code Snippets](https://msdn.microsoft.com/library/ms165392.aspx)</span></span>
- <span data-ttu-id="0ae31-226">Дополнительные сведения о функциях языка Visual Basic фрагменты кода IntelliSense, таких как настройка фрагментов и устранение неполадок.</span><span class="sxs-lookup"><span data-stu-id="0ae31-226">Learn more about the Visual Basic-specific features of IntelliSense code snippets, such as customizing the snippets and troubleshooting.</span></span> <span data-ttu-id="0ae31-227">Дополнительные сведения см. в разделе [фрагменты кода IntelliSense в Visual Basic](https://msdn.microsoft.com/library/18yz4be4.aspx)</span><span class="sxs-lookup"><span data-stu-id="0ae31-227">For more information, see [Visual Basic IntelliSense Code Snippets](https://msdn.microsoft.com/library/18yz4be4.aspx)</span></span>
- <span data-ttu-id="0ae31-228">Дополнительные сведения о C#-отдельных функций IntelliSense, таких как рефакторинга и фрагментов кода.</span><span class="sxs-lookup"><span data-stu-id="0ae31-228">Learn more about the C#-specific features of IntelliSense, such as refactoring and code snippets.</span></span> <span data-ttu-id="0ae31-229">Дополнительные сведения см. в разделе [IntelliSense для Visual C#](https://msdn.microsoft.com/library/43f44291.aspx).</span><span class="sxs-lookup"><span data-stu-id="0ae31-229">For more information, see [Visual C# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).</span></span>
