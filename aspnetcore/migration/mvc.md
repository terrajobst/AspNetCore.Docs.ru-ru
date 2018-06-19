---
title: Перенести из ASP.NET MVC в ASP.NET Core MVC
author: ardalis
description: Узнайте, как начать перенос проекта ASP.NET MVC в ASP.NET Core MVC.
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/mvc
ms.openlocfilehash: b8c913c0a6f47a1c993d508f9baae54981327957
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/08/2018
ms.locfileid: "33851031"
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="72ead-103">Перенести из ASP.NET MVC в ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="72ead-103">Migrate from ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="72ead-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT), [рот Daniel](https://github.com/danroth27), [Стив Смит](https://ardalis.com/), и [Скотт Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="72ead-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="72ead-105">В этой статье показано, как начать перенос проекта ASP.NET MVC для [ASP.NET Core MVC](../mvc/overview.md).</span><span class="sxs-lookup"><span data-stu-id="72ead-105">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="72ead-106">В процессе выделяются многих вещах, которые были изменены с ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="72ead-106">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="72ead-107">Миграция с ASP.NET MVC состоит из нескольких этапов, и в этой статье рассматриваются начальной настройки, основные контроллеры и представления, статическое содержимое и зависимости на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="72ead-107">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="72ead-108">Дополнительные статьи описывается Миграция конфигурации и удостоверения код, который содержится во многих проектах ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="72ead-108">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="72ead-109">Номера версий в образцы могут устареть.</span><span class="sxs-lookup"><span data-stu-id="72ead-109">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="72ead-110">Может потребоваться обновить проекты, соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="72ead-110">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="72ead-111">Создание начального проекта ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="72ead-111">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="72ead-112">Для демонстрации обновления, мы начнем с создания приложения ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="72ead-112">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="72ead-113">Создать его с именем *WebApp1* , пространство имен совпадает с ASP.NET Core проект создается в следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="72ead-113">Create it with the name *WebApp1* so the namespace matches the ASP.NET Core project we create in the next step.</span></span>

![Диалоговое окно Visual Studio новый проект](mvc/_static/new-project.png)

![Диалоговое окно создания веб-приложения: шаблон проекта MVC, выбранного на панели шаблонов ASP.NET](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="72ead-116">*Необязательно:* изменить имя решения из *WebApp1* для *Mvc5*.</span><span class="sxs-lookup"><span data-stu-id="72ead-116">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="72ead-117">Visual Studio отображает имя нового решения (*Mvc5*), упрощая сообщить этот проект из проекта Далее.</span><span class="sxs-lookup"><span data-stu-id="72ead-117">Visual Studio displays the new solution name (*Mvc5*), which makes it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="72ead-118">Создание проекта ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="72ead-118">Create the ASP.NET Core project</span></span>

<span data-ttu-id="72ead-119">Создайте новый *пустой* веб-приложения ASP.NET Core с тем же именем, что предыдущий проект (*WebApp1*), соответствуют пространства имен в двух проектов.</span><span class="sxs-lookup"><span data-stu-id="72ead-119">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="72ead-120">Если то же пространство имен проще скопировать код в двух проектах.</span><span class="sxs-lookup"><span data-stu-id="72ead-120">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="72ead-121">Необходимо создать этот проект в каталоге, отличном от предыдущий проект, чтобы использовать то же имя.</span><span class="sxs-lookup"><span data-stu-id="72ead-121">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![Диалоговое окно создания нового проекта](mvc/_static/new_core.png)

![Диалоговое окно создания веб-приложения ASP.NET: шаблон пустого проекта, выбранного в панели ASP.NET базовых шаблонов](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="72ead-124">*Необязательно:* Создание нового приложения ASP.NET Core с использованием *веб-приложение* шаблона проекта.</span><span class="sxs-lookup"><span data-stu-id="72ead-124">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="72ead-125">Назовите проект *WebApp1*и выберите нужный вариант проверки подлинности из **индивидуальные учетные записи**.</span><span class="sxs-lookup"><span data-stu-id="72ead-125">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="72ead-126">Переименование этого приложения *FullAspNetCore*.</span><span class="sxs-lookup"><span data-stu-id="72ead-126">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="72ead-127">Создание этого проекта экономит время при преобразовании.</span><span class="sxs-lookup"><span data-stu-id="72ead-127">Creating this project saves you time in the conversion.</span></span> <span data-ttu-id="72ead-128">Можно взглянуть на код создан шаблон, см. в результате или скопировать код проекта преобразования.</span><span class="sxs-lookup"><span data-stu-id="72ead-128">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="72ead-129">Это также полезно, когда возникли затруднения на шаг преобразования для сравнения с проектом, созданных в шаблоне.</span><span class="sxs-lookup"><span data-stu-id="72ead-129">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="72ead-130">Настройка сайта для использования MVC</span><span class="sxs-lookup"><span data-stu-id="72ead-130">Configure the site to use MVC</span></span>

* <span data-ttu-id="72ead-131">При разработке для .NET Core, ASP.NET Core metapackage добавляется в проект с именем `Microsoft.AspNetCore.All` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="72ead-131">When targeting .NET Core, the ASP.NET Core metapackage is added to the project, called `Microsoft.AspNetCore.All` by default.</span></span> <span data-ttu-id="72ead-132">Этот пакет содержит пакеты, такие как `Microsoft.AspNetCore.Mvc` и `Microsoft.AspNetCore.StaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="72ead-132">This package contains packages like `Microsoft.AspNetCore.Mvc` and `Microsoft.AspNetCore.StaticFiles`.</span></span> <span data-ttu-id="72ead-133">Если для различных версий платформы .NET Framework, ссылки на пакет необходимо быть перечислены по отдельности в файле \*.csproj.</span><span class="sxs-lookup"><span data-stu-id="72ead-133">If targeting .NET Framework, package references need to be listed individually in the \*.csproj file.</span></span>

<span data-ttu-id="72ead-134">`Microsoft.AspNetCore.Mvc` — Это платформа ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="72ead-134">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="72ead-135">`Microsoft.AspNetCore.StaticFiles` метод является обработчиком статических файлов.</span><span class="sxs-lookup"><span data-stu-id="72ead-135">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="72ead-136">Среда выполнения ASP.NET Core является модульной средой, и необходимо явно включить обновление через обрабатывать статические файлы (в разделе [статические файлы](xref:fundamentals/static-files)).</span><span class="sxs-lookup"><span data-stu-id="72ead-136">The ASP.NET Core runtime is modular, and you must explicitly opt in to serve static files (see [Static files](xref:fundamentals/static-files)).</span></span>

* <span data-ttu-id="72ead-137">Откройте *файла Startup.cs* и измените код в соответствии со следующим:</span><span class="sxs-lookup"><span data-stu-id="72ead-137">Open the *Startup.cs* file and change the code to match the following:</span></span>

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

<span data-ttu-id="72ead-138">`UseStaticFiles` Метод расширения добавляет обработчик файла статистики.</span><span class="sxs-lookup"><span data-stu-id="72ead-138">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="72ead-139">Как упоминалось ранее, среда выполнения ASP.NET является модульной средой, и необходимо явно включить обновление через обрабатывать статические файлы.</span><span class="sxs-lookup"><span data-stu-id="72ead-139">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="72ead-140">`UseMvc` Добавляет метод расширения маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="72ead-140">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="72ead-141">Дополнительные сведения см. в разделе [запуска приложения](xref:fundamentals/startup) и [маршрутизации](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="72ead-141">For more information, see [Application Startup](xref:fundamentals/startup) and [Routing](xref:fundamentals/routing).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="72ead-142">Добавление контроллера и представления</span><span class="sxs-lookup"><span data-stu-id="72ead-142">Add a controller and view</span></span>

<span data-ttu-id="72ead-143">В этом разделе вы добавите минимальной контроллер и представление в качестве заполнителей для контроллера ASP.NET MVC и представления, будет перенести в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="72ead-143">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="72ead-144">Добавить *контроллеров* папки.</span><span class="sxs-lookup"><span data-stu-id="72ead-144">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="72ead-145">Добавить **класс контроллера** с именем *HomeController.cs* для *контроллеров* папки.</span><span class="sxs-lookup"><span data-stu-id="72ead-145">Add a **Controller Class** named *HomeController.cs* to the *Controllers* folder.</span></span>

![Диалоговое окно ''Добавление нового элемента''](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="72ead-147">Добавить *представления* папки.</span><span class="sxs-lookup"><span data-stu-id="72ead-147">Add a *Views* folder.</span></span>

* <span data-ttu-id="72ead-148">Добавить *представления/домашние* папки.</span><span class="sxs-lookup"><span data-stu-id="72ead-148">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="72ead-149">Добавить **представления Razor** с именем *Index.cshtml* для *представления/домашние* папки.</span><span class="sxs-lookup"><span data-stu-id="72ead-149">Add a **Razor View** named *Index.cshtml* to the *Views/Home* folder.</span></span>

![Диалоговое окно ''Добавление нового элемента''](mvc/_static/view.png)

<span data-ttu-id="72ead-151">Структура проекта, показано ниже:</span><span class="sxs-lookup"><span data-stu-id="72ead-151">The project structure is shown below:</span></span>

![В обозревателе решений файлы и папки с WebApp1](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="72ead-153">Замените содержимое *Views/Home/Index.cshtml* файл со следующим:</span><span class="sxs-lookup"><span data-stu-id="72ead-153">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="72ead-154">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="72ead-154">Run the app.</span></span>

![Открыть в Microsoft Edge веб-приложения](mvc/_static/hello-world.png)

<span data-ttu-id="72ead-156">В разделе [контроллеров](xref:mvc/controllers/actions) и [представления](xref:mvc/views/overview) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="72ead-156">See [Controllers](xref:mvc/controllers/actions) and [Views](xref:mvc/views/overview) for more information.</span></span>

<span data-ttu-id="72ead-157">Теперь, когда у нас есть минимальный рабочий проект ASP.NET Core, мы можем запустить перенос функциональные возможности из проекта ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="72ead-157">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="72ead-158">Необходимо переместить следующее:</span><span class="sxs-lookup"><span data-stu-id="72ead-158">We need to move the following:</span></span>

* <span data-ttu-id="72ead-159">клиентское содержимое (CSS, шрифты и скрипты.)</span><span class="sxs-lookup"><span data-stu-id="72ead-159">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="72ead-160">контроллеры</span><span class="sxs-lookup"><span data-stu-id="72ead-160">controllers</span></span>

* <span data-ttu-id="72ead-161">представления</span><span class="sxs-lookup"><span data-stu-id="72ead-161">views</span></span>

* <span data-ttu-id="72ead-162">модели</span><span class="sxs-lookup"><span data-stu-id="72ead-162">models</span></span>

* <span data-ttu-id="72ead-163">Объединение</span><span class="sxs-lookup"><span data-stu-id="72ead-163">bundling</span></span>

* <span data-ttu-id="72ead-164">фильтры</span><span class="sxs-lookup"><span data-stu-id="72ead-164">filters</span></span>

* <span data-ttu-id="72ead-165">Журнал in/out, Identity (это выполняется в следующем уроке.)</span><span class="sxs-lookup"><span data-stu-id="72ead-165">Log in/out, Identity (This is done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="72ead-166">Контроллеры и представления</span><span class="sxs-lookup"><span data-stu-id="72ead-166">Controllers and views</span></span>

* <span data-ttu-id="72ead-167">Копирование всех методов из ASP.NET MVC `HomeController` к новому `HomeController`.</span><span class="sxs-lookup"><span data-stu-id="72ead-167">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="72ead-168">Обратите внимание, что в ASP.NET MVC, возвращаемый тип метода действия встроенного шаблона контроллера [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); в ASP.NET MVC Core, возвращаемого методами `IActionResult` вместо него.</span><span class="sxs-lookup"><span data-stu-id="72ead-168">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="72ead-169">`ActionResult` реализует `IActionResult`, поэтому нет необходимости изменять тип возвращаемого значения методов действия.</span><span class="sxs-lookup"><span data-stu-id="72ead-169">`ActionResult` implements `IActionResult`, so there's no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="72ead-170">Копировать *About.cshtml*, *Contact.cshtml*, и *Index.cshtml* Razor Просмотр файлов из проекта ASP.NET MVC в проект ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="72ead-170">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="72ead-171">Запуск приложения ASP.NET Core и каждый метод теста.</span><span class="sxs-lookup"><span data-stu-id="72ead-171">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="72ead-172">Мы еще не перенесены файл макета или стили, готовый для просмотра представления только с содержимым в просмотр файлов.</span><span class="sxs-lookup"><span data-stu-id="72ead-172">We haven't migrated the layout file or styles yet, so the rendered views only contain the content in the view files.</span></span> <span data-ttu-id="72ead-173">Вы не будете иметь ссылки файла, созданного макета для `About` и `Contact` просматривает, таким образом вы получаете вызывать их из браузера (Замените **4492** с номером порта, используемые в вашем проекте).</span><span class="sxs-lookup"><span data-stu-id="72ead-173">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Страницы контактов](mvc/_static/contact-page.png)

<span data-ttu-id="72ead-175">Обратите внимание, отсутствие Стилизация и элементами меню.</span><span class="sxs-lookup"><span data-stu-id="72ead-175">Note the lack of styling and menu items.</span></span> <span data-ttu-id="72ead-176">Мы исправим это в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="72ead-176">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="72ead-177">Статическое содержимое</span><span class="sxs-lookup"><span data-stu-id="72ead-177">Static content</span></span>

<span data-ttu-id="72ead-178">В предыдущих версиях ASP.NET MVC статическое содержимое было размещено из корневого каталога веб-проекта и был перемежаемый файлов на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="72ead-178">In previous versions of ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="72ead-179">В ASP.NET Core статическое содержимое размещается в *wwwroot* папки.</span><span class="sxs-lookup"><span data-stu-id="72ead-179">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="72ead-180">Может потребоваться скопировать статическое содержимое из старого приложения ASP.NET MVC для *wwwroot* в папке проекта ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="72ead-180">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="72ead-181">В данном примере:</span><span class="sxs-lookup"><span data-stu-id="72ead-181">In this sample conversion:</span></span>

* <span data-ttu-id="72ead-182">Копировать *favicon.ico* файла из старого проекта MVC *wwwroot* папку в проекте ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="72ead-182">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="72ead-183">Используется в проекте ASP.NET MVC старый [начальной загрузки](https://getbootstrap.com/) для его задания стиля и хранилищ, программу начальной загрузки файлов в *содержимого* и *сценариев* папки.</span><span class="sxs-lookup"><span data-stu-id="72ead-183">The old ASP.NET MVC project uses [Bootstrap](https://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="72ead-184">Шаблон, который создан старого проекта ASP.NET MVC, ссылается на начальной загрузки в файл макета (*Views/Shared/_Layout.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="72ead-184">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="72ead-185">Можно скопировать *bootstrap.js* и *bootstrap.css* файлы из ASP.NET MVC проекта для *wwwroot* папки в новом проекте.</span><span class="sxs-lookup"><span data-stu-id="72ead-185">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project.</span></span> <span data-ttu-id="72ead-186">Вместо этого мы добавим поддержку для начальной загрузки (и другие клиентские библиотеки), с помощью CDN в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="72ead-186">Instead, we'll add support for Bootstrap (and other client-side libraries) using CDNs in the next section.</span></span>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="72ead-187">Перенесите файл макета</span><span class="sxs-lookup"><span data-stu-id="72ead-187">Migrate the layout file</span></span>

* <span data-ttu-id="72ead-188">Копировать *файл _ViewStart.cshtml* файла из старого проекта ASP.NET MVC *представления* папки в проект ASP.NET Core *представления* папки.</span><span class="sxs-lookup"><span data-stu-id="72ead-188">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="72ead-189">*Файл _ViewStart.cshtml* файл не был изменен в ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="72ead-189">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="72ead-190">Создание *представления/Общие* папки.</span><span class="sxs-lookup"><span data-stu-id="72ead-190">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="72ead-191">*Необязательно:* копирования *_ViewImports.cshtml* из *FullAspNetCore* проекта MVC *представления* папки в проект ASP.NET Core  *Представления* папки.</span><span class="sxs-lookup"><span data-stu-id="72ead-191">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="72ead-192">Удалите все объявления пространства имен в *_ViewImports.cshtml* файла.</span><span class="sxs-lookup"><span data-stu-id="72ead-192">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="72ead-193">*_ViewImports.cshtml* файла предоставляют пространства имен для всех файлов, представления, после чего [вспомогательных функций тегов](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="72ead-193">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="72ead-194">В новом файле макета используются вспомогательных функций тегов.</span><span class="sxs-lookup"><span data-stu-id="72ead-194">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="72ead-195">*_ViewImports.cshtml* файл впервые ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="72ead-195">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="72ead-196">Копировать *_Layout.cshtml* файла из старого проекта ASP.NET MVC *представления/Общие* папки в проект ASP.NET Core *представления/Общие* папки.</span><span class="sxs-lookup"><span data-stu-id="72ead-196">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="72ead-197">Откройте *_Layout.cshtml* файл и внесите следующие изменения (ниже приведен полный код):</span><span class="sxs-lookup"><span data-stu-id="72ead-197">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

* <span data-ttu-id="72ead-198">Замените `@Styles.Render("~/Content/css")` с `<link>` элемента, который требуется загрузить *bootstrap.css* (см. ниже).</span><span class="sxs-lookup"><span data-stu-id="72ead-198">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

* <span data-ttu-id="72ead-199">Удалите `@Scripts.Render("~/bundles/modernizr")`.</span><span class="sxs-lookup"><span data-stu-id="72ead-199">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

* <span data-ttu-id="72ead-200">Закомментируйте `@Html.Partial("_LoginPartial")` строки (заключить строку с `@*...*@`).</span><span class="sxs-lookup"><span data-stu-id="72ead-200">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="72ead-201">Мы вернемся к нему в будущем учебника.</span><span class="sxs-lookup"><span data-stu-id="72ead-201">We'll return to it in a future tutorial.</span></span>

* <span data-ttu-id="72ead-202">Замените `@Scripts.Render("~/bundles/jquery")` с `<script>` элемент (см. ниже).</span><span class="sxs-lookup"><span data-stu-id="72ead-202">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

* <span data-ttu-id="72ead-203">Замените `@Scripts.Render("~/bundles/bootstrap")` с `<script>` элемент (см. ниже).</span><span class="sxs-lookup"><span data-stu-id="72ead-203">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below).</span></span>

<span data-ttu-id="72ead-204">Разметка замены для включения начальной загрузки CSS:</span><span class="sxs-lookup"><span data-stu-id="72ead-204">The replacement markup for Bootstrap CSS inclusion:</span></span>

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

<span data-ttu-id="72ead-205">Разметка замены для включения начальной загрузки JavaScript и jQuery:</span><span class="sxs-lookup"><span data-stu-id="72ead-205">The replacement markup for jQuery and Bootstrap JavaScript inclusion:</span></span>

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

<span data-ttu-id="72ead-206">Обновленный *_Layout.cshtml* файла показан ниже:</span><span class="sxs-lookup"><span data-stu-id="72ead-206">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

<span data-ttu-id="72ead-207">Просмотр сайта в браузере.</span><span class="sxs-lookup"><span data-stu-id="72ead-207">View the site in the browser.</span></span> <span data-ttu-id="72ead-208">Должны теперь загрузиться правильно, с ожидаемой стили на месте.</span><span class="sxs-lookup"><span data-stu-id="72ead-208">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="72ead-209">*Необязательно:* может потребоваться попробуйте использовать новый файл макета.</span><span class="sxs-lookup"><span data-stu-id="72ead-209">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="72ead-210">Для этого проекта можно скопировать файл из макета *FullAspNetCore* проекта.</span><span class="sxs-lookup"><span data-stu-id="72ead-210">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="72ead-211">В новом файле макета используется [вспомогательных функций тегов](xref:mvc/views/tag-helpers/intro) и других усовершенствований.</span><span class="sxs-lookup"><span data-stu-id="72ead-211">The new layout file uses [Tag Helpers](xref:mvc/views/tag-helpers/intro) and has other improvements.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="72ead-212">Настроить объединение и Минификация</span><span class="sxs-lookup"><span data-stu-id="72ead-212">Configure bundling and minification</span></span>

<span data-ttu-id="72ead-213">Сведения о настройке объединение и Минификация см. в разделе [объединении и Минификация](../client-side/bundling-and-minification.md).</span><span class="sxs-lookup"><span data-stu-id="72ead-213">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solve-http-500-errors"></a><span data-ttu-id="72ead-214">Устранить ошибки HTTP 500</span><span class="sxs-lookup"><span data-stu-id="72ead-214">Solve HTTP 500 errors</span></span>

<span data-ttu-id="72ead-215">Существуют многие проблемы, которые могут вызвать сообщение об ошибке HTTP 500, не содержат сведений о источник проблемы.</span><span class="sxs-lookup"><span data-stu-id="72ead-215">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="72ead-216">Например если *Views/_ViewImports.cshtml* файл содержит пространство имен, которые не существуют в проекте, вы получите ошибку HTTP 500.</span><span class="sxs-lookup"><span data-stu-id="72ead-216">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="72ead-217">По умолчанию в приложениях ASP.NET Core `UseDeveloperExceptionPage` добавляется расширение `IApplicationBuilder` и выполняется после настройки *разработки*.</span><span class="sxs-lookup"><span data-stu-id="72ead-217">By default in ASP.NET Core apps, the `UseDeveloperExceptionPage` extension is added to the `IApplicationBuilder` and executed when the configuration is *Development*.</span></span> <span data-ttu-id="72ead-218">Это подробно описывается в следующем коде:</span><span class="sxs-lookup"><span data-stu-id="72ead-218">This is detailed in the following code:</span></span>

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

<span data-ttu-id="72ead-219">ASP.NET Core преобразует необработанные исключения в веб-приложения в сообщения об ошибках HTTP 500.</span><span class="sxs-lookup"><span data-stu-id="72ead-219">ASP.NET Core converts unhandled exceptions in a web app into HTTP 500 error responses.</span></span> <span data-ttu-id="72ead-220">Как правило сведения об ошибке не включены в эти ответы, чтобы предотвратить раскрытие конфиденциальных сведений о сервере.</span><span class="sxs-lookup"><span data-stu-id="72ead-220">Normally, error details aren't included in these responses to prevent disclosure of potentially sensitive information about the server.</span></span> <span data-ttu-id="72ead-221">В разделе **с помощью страницы исключение разработчика** в [обработки ошибок](../fundamentals/error-handling.md) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="72ead-221">See **Using the Developer Exception Page** in [Handle errors](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="72ead-222">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="72ead-222">Additional resources</span></span>

* [<span data-ttu-id="72ead-223">Клиентская разработка</span><span class="sxs-lookup"><span data-stu-id="72ead-223">Client-side development</span></span>](xref:client-side/index)
* [<span data-ttu-id="72ead-224">Вспомогательные функции тегов</span><span class="sxs-lookup"><span data-stu-id="72ead-224">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
