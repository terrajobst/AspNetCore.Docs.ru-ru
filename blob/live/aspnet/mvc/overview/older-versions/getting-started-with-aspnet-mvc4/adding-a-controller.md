---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: "Добавление контроллера | Документы Microsoft"
author: Rick-Anderson
description: "Примечание: Обновленную версию этого учебника доступен здесь, использующий ASP.NET MVC 5 и Visual Studio 2013. Это более безопасный, гораздо проще выполните и демонстрационных..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 69af91401e51470fbc0b67103345325201b06723
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-controller"></a><span data-ttu-id="0ceac-104">Добавление контроллера</span><span class="sxs-lookup"><span data-stu-id="0ceac-104">Adding a Controller</span></span>
====================
<span data-ttu-id="0ceac-105">По [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="0ceac-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="0ceac-106">Доступна обновленная версия этого учебника [здесь](../../getting-started/introduction/getting-started.md) , с использованием ASP.NET MVC 5 и Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="0ceac-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="0ceac-107">Он является более безопасны, выполните гораздо проще и показаны дополнительные возможности.</span><span class="sxs-lookup"><span data-stu-id="0ceac-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="0ceac-108">Обозначает MVC *model-view-controller*.</span><span class="sxs-lookup"><span data-stu-id="0ceac-108">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="0ceac-109">MVC представляет собой шаблон для разработки приложений, которые хорошо архитектурой, тестирования и простые в обслуживании.</span><span class="sxs-lookup"><span data-stu-id="0ceac-109">MVC is a pattern for developing applications that are well architected, testable and easy to maintain.</span></span> <span data-ttu-id="0ceac-110">Содержать MVC-приложений:</span><span class="sxs-lookup"><span data-stu-id="0ceac-110">MVC-based applications contain:</span></span>

- <span data-ttu-id="0ceac-111">**M** odels: классы, представляющие данные приложения, которые используют логику проверки для применения бизнес-правил для этих данных.</span><span class="sxs-lookup"><span data-stu-id="0ceac-111">**M** odels: Classes that represent the data of the application and that use validation logic to enforce business rules for that data.</span></span>
- <span data-ttu-id="0ceac-112">**V** iews: файлы шаблонов, которые приложение использует для динамического создания HTML-ответы.</span><span class="sxs-lookup"><span data-stu-id="0ceac-112">**V** iews: Template files that your application uses to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="0ceac-113">**C** ontrollers: классы, которые обрабатывают входящие запросы браузера, получения данных модели, а затем укажите шаблонов представлений, которые возвращают ответа в браузер.</span><span class="sxs-lookup"><span data-stu-id="0ceac-113">**C** ontrollers: Classes that handle incoming browser requests, retrieve model data, and then specify view templates that return a response to the browser.</span></span>

<span data-ttu-id="0ceac-114">Мы быть, охватывающие все эти компоненты этого учебника ряда и показано, как их использовать для построения приложения.</span><span class="sxs-lookup"><span data-stu-id="0ceac-114">We'll be covering all these concepts in this tutorial series and show you how to use them to build an application.</span></span>

<span data-ttu-id="0ceac-115">Давайте начнем с создания класса контроллера.</span><span class="sxs-lookup"><span data-stu-id="0ceac-115">Let's begin by creating a controller class.</span></span> <span data-ttu-id="0ceac-116">В **обозревателе решений**, щелкните правой кнопкой мыши *контроллеров* папки, а затем выберите **добавить контроллер**.</span><span class="sxs-lookup"><span data-stu-id="0ceac-116">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span>

![](adding-a-controller/_static/image1.png)

<span data-ttu-id="0ceac-117">Имя вашего нового контроллера &quot;HelloWorldController&quot;.</span><span class="sxs-lookup"><span data-stu-id="0ceac-117">Name your new controller &quot;HelloWorldController&quot;.</span></span> <span data-ttu-id="0ceac-118">Оставить как шаблон по умолчанию **контроллер MVC пустой** и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="0ceac-118">Leave the default template as **Empty MVC controller** and click **Add**.</span></span>

![Добавление контроллера](adding-a-controller/_static/image2.png)

<span data-ttu-id="0ceac-120">Обратите внимание на **обозревателе решений** что файл был создан именованный *HelloWorldController.cs*.</span><span class="sxs-lookup"><span data-stu-id="0ceac-120">Notice in **Solution Explorer** that a new file has been created named *HelloWorldController.cs*.</span></span> <span data-ttu-id="0ceac-121">Файл открыт в среде IDE.</span><span class="sxs-lookup"><span data-stu-id="0ceac-121">The file is open in the IDE.</span></span>

![](adding-a-controller/_static/image3.png)

<span data-ttu-id="0ceac-122">Замените содержимое файла следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="0ceac-122">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

<span data-ttu-id="0ceac-123">Методы контроллера возвратит строку HTML-кода в качестве примера.</span><span class="sxs-lookup"><span data-stu-id="0ceac-123">The controller methods will return a string of HTML as an example.</span></span> <span data-ttu-id="0ceac-124">Контроллер называется `HelloWorldController` и первый способ называется `Index`.</span><span class="sxs-lookup"><span data-stu-id="0ceac-124">The controller is named `HelloWorldController` and the first method above is named `Index`.</span></span> <span data-ttu-id="0ceac-125">Давайте вызывать его из браузера.</span><span class="sxs-lookup"><span data-stu-id="0ceac-125">Let's invoke it from a browser.</span></span> <span data-ttu-id="0ceac-126">Запустите приложение (нажмите клавишу F5 или Ctrl + F5).</span><span class="sxs-lookup"><span data-stu-id="0ceac-126">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="0ceac-127">В браузере, добавьте &quot;HelloWorld&quot; пути в адресной строке.</span><span class="sxs-lookup"><span data-stu-id="0ceac-127">In the browser, append &quot;HelloWorld&quot; to the path in the address bar.</span></span> <span data-ttu-id="0ceac-128">(Например, на рисунке ниже, он является `http://localhost:1234/HelloWorld.`) будет выглядеть страница в браузере, а следующий снимок экрана.</span><span class="sxs-lookup"><span data-stu-id="0ceac-128">(For example, in the illustration below, it's `http://localhost:1234/HelloWorld.`) The page in the browser will look like the following screenshot.</span></span> <span data-ttu-id="0ceac-129">В методе выше код непосредственно вернул строку.</span><span class="sxs-lookup"><span data-stu-id="0ceac-129">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="0ceac-130">Вы сообщили система просто возвращается HTML-ФАЙЛ, как и!</span><span class="sxs-lookup"><span data-stu-id="0ceac-130">You told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image4.png)

<span data-ttu-id="0ceac-131">ASP.NET MVC вызывает другой контроллер классы (и другие методы действия в них), в зависимости от входящего URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="0ceac-131">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="0ceac-132">Логику маршрутизации URL-адрес по умолчанию используется ASP.NET MVC использует формат следующим образом, чтобы определить, какой код для вызова:</span><span class="sxs-lookup"><span data-stu-id="0ceac-132">The default URL routing logic used by ASP.NET MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="0ceac-133">Первая часть URL-адреса определяет класс контроллера для выполнения.</span><span class="sxs-lookup"><span data-stu-id="0ceac-133">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="0ceac-134">Поэтому */HelloWorld* сопоставляется `HelloWorldController` класса.</span><span class="sxs-lookup"><span data-stu-id="0ceac-134">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="0ceac-135">Во второй части URL-адрес определяет на класс для выполнения метода действия.</span><span class="sxs-lookup"><span data-stu-id="0ceac-135">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="0ceac-136">Поэтому */HelloWorld/индекс* вызовет `Index` метод `HelloWorldController` класса для выполнения.</span><span class="sxs-lookup"><span data-stu-id="0ceac-136">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="0ceac-137">Обратите внимание, что нам пришлось перейдите к */HelloWorld* и `Index` метод использовался по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0ceac-137">Notice that we only had to browse to */HelloWorld* and the `Index` method was used by default.</span></span> <span data-ttu-id="0ceac-138">Это так, как метод с именем `Index` метод по умолчанию, который будет вызываться на контроллере, если тип не задан явно.</span><span class="sxs-lookup"><span data-stu-id="0ceac-138">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span>

<span data-ttu-id="0ceac-139">Перейдите по адресу `http://localhost:xxxx/HelloWorld/Welcome`.</span><span class="sxs-lookup"><span data-stu-id="0ceac-139">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="0ceac-140">`Welcome` Метод выполняется и возвращает строку, которая &quot;это метод приветствия действие... &quot;.</span><span class="sxs-lookup"><span data-stu-id="0ceac-140">The `Welcome` method runs and returns the string &quot;This is the Welcome action method...&quot;.</span></span> <span data-ttu-id="0ceac-141">Сопоставление по умолчанию MVC `/[Controller]/[ActionName]/[Parameters]`.</span><span class="sxs-lookup"><span data-stu-id="0ceac-141">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="0ceac-142">Для этого URL-адреса заданы контроллер `HelloWorld` и метод действия `Welcome`.</span><span class="sxs-lookup"><span data-stu-id="0ceac-142">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="0ceac-143">Часть URL-адреса `[Parameters]` на данный момент еще не использовалась.</span><span class="sxs-lookup"><span data-stu-id="0ceac-143">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="0ceac-144">Давайте немного изменить этот пример, чтобы некоторые сведения о параметрах можно передать URL-адреса контроллера (например, */HelloWorld/приветствия? имя = Скотт&amp;numtimes = 4*).</span><span class="sxs-lookup"><span data-stu-id="0ceac-144">Let's modify the example slightly so that you can pass some parameter information from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="0ceac-145">Изменение вашей `Welcome` метод, чтобы включить два параметра, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="0ceac-145">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="0ceac-146">Обратите внимание, что код использует функцию C# необязательный параметр указывает, что `numTimes` параметр по умолчанию 1, если не передаются значения для этого параметра.</span><span class="sxs-lookup"><span data-stu-id="0ceac-146">Note that the code uses the C# optional-parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

<span data-ttu-id="0ceac-147">Запустите приложение и откройте пример URL-адрес (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`.</span><span class="sxs-lookup"><span data-stu-id="0ceac-147">Run your application and browse to the example URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`.</span></span> <span data-ttu-id="0ceac-148">Можно попробовать разные значения для `name` и `numtimes` в URL-АДРЕСЕ.</span><span class="sxs-lookup"><span data-stu-id="0ceac-148">You can try different values for `name` and `numtimes` in the URL.</span></span> <span data-ttu-id="0ceac-149">[Система привязки модели ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) автоматически сопоставляет именованные параметры из строки запроса в адресной строке для параметров в методе.</span><span class="sxs-lookup"><span data-stu-id="0ceac-149">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters from the query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="0ceac-150">В обоих этих примерах контроллер предоставляет &quot;VC&quot; часть MVC — то есть, представления и контроллера работы.</span><span class="sxs-lookup"><span data-stu-id="0ceac-150">In both these examples the controller has been doing the &quot;VC&quot; portion of MVC — that is, the view and controller work.</span></span> <span data-ttu-id="0ceac-151">Контроллер вернул HTML напрямую.</span><span class="sxs-lookup"><span data-stu-id="0ceac-151">The controller is returning HTML directly.</span></span> <span data-ttu-id="0ceac-152">Обычно вы не хотите контроллеров, возвращение HTML напрямую, так как становится очень сложно в код.</span><span class="sxs-lookup"><span data-stu-id="0ceac-152">Ordinarily you don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="0ceac-153">Вместо этого мы обычно используем отдельное представление файла шаблона для создания HTML-ответа.</span><span class="sxs-lookup"><span data-stu-id="0ceac-153">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="0ceac-154">Давайте посмотрим, далее в том, как можно это сделать.</span><span class="sxs-lookup"><span data-stu-id="0ceac-154">Let's look next at how we can do this.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="0ceac-155">[Назад](intro-to-aspnet-mvc-4.md)
[Вперед](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="0ceac-155">[Previous](intro-to-aspnet-mvc-4.md)
[Next](adding-a-view.md)</span></span>
