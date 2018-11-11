---
title: Миграция с ASP.NET MVC для ASP.NET Core MVC
author: ardalis
description: Узнайте, как приступить к миграции проекта ASP.NET MVC в ASP.NET Core MVC.
ms.author: riande
ms.date: 03/07/2017
uid: migration/mvc
ms.openlocfilehash: 7c9d927bbd06f96f130d53e946a2963b5804960b
ms.sourcegitcommit: edb9d2d78c9a4d68b397e74ae2aff088b325a143
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/09/2018
ms.locfileid: "51505743"
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="5a889-103">Миграция с ASP.NET MVC для ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="5a889-103">Migrate from ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="5a889-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Дэниэл рот](https://github.com/danroth27), [Стив Смит](https://ardalis.com/), и [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="5a889-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="5a889-105">В этой статье показано, как приступить к миграции проекта ASP.NET MVC для [ASP.NET Core MVC](../mvc/overview.md).</span><span class="sxs-lookup"><span data-stu-id="5a889-105">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="5a889-106">В процессе он выделяет многие действия, которые были изменены с ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5a889-106">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="5a889-107">Миграция с ASP.NET MVC состоит из нескольких этапов и в этой статье рассматриваются начальной настройки, основные контроллеры и представления, статическое содержимое и зависимости на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="5a889-107">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="5a889-108">Дополнительные статьи посвящены перенос конфигурации, а также найти во многих проектах ASP.NET MVC код удостоверений.</span><span class="sxs-lookup"><span data-stu-id="5a889-108">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="5a889-109">Номера версий в примерах может потерять свою актуальность.</span><span class="sxs-lookup"><span data-stu-id="5a889-109">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="5a889-110">Может потребоваться соответствующим образом обновление проектов.</span><span class="sxs-lookup"><span data-stu-id="5a889-110">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="5a889-111">Создание начального проекта ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="5a889-111">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="5a889-112">Чтобы продемонстрировать обновления, мы начнем с создания приложения ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5a889-112">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="5a889-113">Создайте его с именем *WebApp1* так, пространство имен проекта ASP.NET Core, мы создадим в следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="5a889-113">Create it with the name *WebApp1* so the namespace matches the ASP.NET Core project we create in the next step.</span></span>

![Диалоговое окно Visual Studio новый проект](mvc/_static/new-project.png)

![Диалоговое окно создания веб-приложение: шаблон проекта MVC, выбранного в панели шаблонов ASP.NET](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="5a889-116">*Необязательно:* изменить имя решения из *WebApp1* для *Mvc5*.</span><span class="sxs-lookup"><span data-stu-id="5a889-116">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="5a889-117">Visual Studio отображает имя нового решения (*Mvc5*), что упрощает определить этот проект из следующего проекта.</span><span class="sxs-lookup"><span data-stu-id="5a889-117">Visual Studio displays the new solution name (*Mvc5*), which makes it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="5a889-118">Создание проекта ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5a889-118">Create the ASP.NET Core project</span></span>

<span data-ttu-id="5a889-119">Создайте новый *пустой* веб-приложения ASP.NET Core с тем же именем, что и предыдущий проект (*WebApp1*) чтобы совпадали пространства имен в двух проектах.</span><span class="sxs-lookup"><span data-stu-id="5a889-119">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="5a889-120">Наличие то же пространство имен упрощает проще скопировать код между двумя проектами.</span><span class="sxs-lookup"><span data-stu-id="5a889-120">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="5a889-121">Вам придется создать этот проект в каталоге, отличном от предыдущий проект, чтобы использовать то же имя.</span><span class="sxs-lookup"><span data-stu-id="5a889-121">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![Диалоговое окно создания нового проекта](mvc/_static/new_core.png)

![Диалоговое окно создания веб-приложения ASP.NET: шаблон пустого проекта, выбранного в панели шаблоны ASP.NET Core](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="5a889-124">*Необязательно:* создать новое приложение ASP.NET Core с помощью *веб-приложение* шаблона проекта.</span><span class="sxs-lookup"><span data-stu-id="5a889-124">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="5a889-125">Назовите проект *WebApp1*и выберите параметр проверки из **учетные записи отдельных пользователей**.</span><span class="sxs-lookup"><span data-stu-id="5a889-125">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="5a889-126">Переименовать это приложение для *FullAspNetCore*.</span><span class="sxs-lookup"><span data-stu-id="5a889-126">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="5a889-127">Создание этого проекта экономит время при преобразовании.</span><span class="sxs-lookup"><span data-stu-id="5a889-127">Creating this project saves you time in the conversion.</span></span> <span data-ttu-id="5a889-128">Вы можете изучить сгенерированный шаблона код см. в результате или скопировать код для преобразования проекта.</span><span class="sxs-lookup"><span data-stu-id="5a889-128">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="5a889-129">Это также полезно, когда вы перестает отвечать на шаг преобразования, сравниваемый с этим проектом, созданных в шаблоне.</span><span class="sxs-lookup"><span data-stu-id="5a889-129">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="5a889-130">Настройка сайта для использования MVC</span><span class="sxs-lookup"><span data-stu-id="5a889-130">Configure the site to use MVC</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="5a889-131">При нацеливании на .NET Core, [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) указывается по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="5a889-131">When targeting .NET Core, the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) is referenced by default.</span></span> <span data-ttu-id="5a889-132">Этот пакет содержит пакеты, часто используемые пакеты по MVC-приложения.</span><span class="sxs-lookup"><span data-stu-id="5a889-132">This package contains packages commonly used packages by MVC apps.</span></span> <span data-ttu-id="5a889-133">Для работы с .NET Framework, ссылки на пакеты должны быть перечислены по отдельности в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="5a889-133">If targeting .NET Framework, package references must be listed individually in the project file.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <span data-ttu-id="5a889-134">При нацеливании на .NET Core, [метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage) указывается по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="5a889-134">When targeting .NET Core, the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) is referenced by default.</span></span> <span data-ttu-id="5a889-135">Этот пакет содержит пакеты, часто используемые пакеты по MVC-приложения.</span><span class="sxs-lookup"><span data-stu-id="5a889-135">This package contains packages commonly used packages by MVC apps.</span></span> <span data-ttu-id="5a889-136">Для работы с .NET Framework, ссылки на пакеты должны быть перечислены по отдельности в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="5a889-136">If targeting .NET Framework, package references must be listed individually in the project file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* <span data-ttu-id="5a889-137">При нацеливании на .NET Core или .NET Framework, пакетов, часто используемых пакетов по MVC-приложения, перечислены по отдельности в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="5a889-137">When targeting .NET Core or .NET Framework, packages commonly used packages by MVC apps are listed individually in the project file.</span></span>

::: moniker-end

<span data-ttu-id="5a889-138">`Microsoft.AspNetCore.Mvc` — Это платформа ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="5a889-138">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="5a889-139">`Microsoft.AspNetCore.StaticFiles` — Это обработчик статических файлов.</span><span class="sxs-lookup"><span data-stu-id="5a889-139">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="5a889-140">Среда выполнения ASP.NET Core является модульной средой, и вы должны явно согласиться на обслуживать статические файлы (см. в разделе [статические файлы](xref:fundamentals/static-files)).</span><span class="sxs-lookup"><span data-stu-id="5a889-140">The ASP.NET Core runtime is modular, and you must explicitly opt in to serve static files (see [Static files](xref:fundamentals/static-files)).</span></span>

* <span data-ttu-id="5a889-141">Откройте *Startup.cs* и измените код следующим:</span><span class="sxs-lookup"><span data-stu-id="5a889-141">Open the *Startup.cs* file and change the code to match the following:</span></span>

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

<span data-ttu-id="5a889-142">`UseStaticFiles` Метод расширения добавляет обработчик статических файлов.</span><span class="sxs-lookup"><span data-stu-id="5a889-142">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="5a889-143">Как упоминалось ранее, среда выполнения ASP.NET является модульной средой, и вы должны явно согласиться на обслуживать статические файлы.</span><span class="sxs-lookup"><span data-stu-id="5a889-143">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="5a889-144">`UseMvc` Добавляет метод расширения маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="5a889-144">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="5a889-145">Дополнительные сведения см. в разделе [запуск приложения](xref:fundamentals/startup) и [маршрутизации](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="5a889-145">For more information, see [Application Startup](xref:fundamentals/startup) and [Routing](xref:fundamentals/routing).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="5a889-146">Добавьте контроллер и представление</span><span class="sxs-lookup"><span data-stu-id="5a889-146">Add a controller and view</span></span>

<span data-ttu-id="5a889-147">В этом разделе вы добавите это минимальный контроллер и представление для использования в качестве заполнителей для контроллера ASP.NET MVC и представлений, что необходимо перенести в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="5a889-147">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="5a889-148">Добавить *контроллеров* папки.</span><span class="sxs-lookup"><span data-stu-id="5a889-148">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="5a889-149">Добавить **класс контроллера** с именем *HomeController.cs* для *контроллеров* папки.</span><span class="sxs-lookup"><span data-stu-id="5a889-149">Add a **Controller Class** named *HomeController.cs* to the *Controllers* folder.</span></span>

![Диалоговое окно ''Добавление нового элемента''](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="5a889-151">Добавить *представления* папки.</span><span class="sxs-lookup"><span data-stu-id="5a889-151">Add a *Views* folder.</span></span>

* <span data-ttu-id="5a889-152">Добавить *Views/Home* папки.</span><span class="sxs-lookup"><span data-stu-id="5a889-152">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="5a889-153">Добавить **представления Razor** с именем *Index.cshtml* для *Views/Home* папки.</span><span class="sxs-lookup"><span data-stu-id="5a889-153">Add a **Razor View** named *Index.cshtml* to the *Views/Home* folder.</span></span>

![Диалоговое окно ''Добавление нового элемента''](mvc/_static/view.png)

<span data-ttu-id="5a889-155">Ниже показана структура проекта:</span><span class="sxs-lookup"><span data-stu-id="5a889-155">The project structure is shown below:</span></span>

![В обозревателе решений файлов и папок из WebApp1](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="5a889-157">Замените содержимое файла *Views/Home/Index.cshtml* файл со следующими:</span><span class="sxs-lookup"><span data-stu-id="5a889-157">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="5a889-158">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="5a889-158">Run the app.</span></span>

![Веб-приложения в Microsoft Edge](mvc/_static/hello-world.png)

<span data-ttu-id="5a889-160">См. в разделе [контроллеров](xref:mvc/controllers/actions) и [представления](xref:mvc/views/overview) Дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="5a889-160">See [Controllers](xref:mvc/controllers/actions) and [Views](xref:mvc/views/overview) for more information.</span></span>

<span data-ttu-id="5a889-161">Теперь, когда у нас есть минимальный рабочий проект ASP.NET Core, можно начать перенос функциональные возможности из проекта ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5a889-161">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="5a889-162">Нам нужно переместить следующие:</span><span class="sxs-lookup"><span data-stu-id="5a889-162">We need to move the following:</span></span>

* <span data-ttu-id="5a889-163">клиентским содержимым ("CSS", "Шрифты" и "скрипты")</span><span class="sxs-lookup"><span data-stu-id="5a889-163">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="5a889-164">контроллеры</span><span class="sxs-lookup"><span data-stu-id="5a889-164">controllers</span></span>

* <span data-ttu-id="5a889-165">представления</span><span class="sxs-lookup"><span data-stu-id="5a889-165">views</span></span>

* <span data-ttu-id="5a889-166">модели</span><span class="sxs-lookup"><span data-stu-id="5a889-166">models</span></span>

* <span data-ttu-id="5a889-167">Объединение</span><span class="sxs-lookup"><span data-stu-id="5a889-167">bundling</span></span>

* <span data-ttu-id="5a889-168">фильтры</span><span class="sxs-lookup"><span data-stu-id="5a889-168">filters</span></span>

* <span data-ttu-id="5a889-169">Журнал ввода-вывода, Identity (это делается в следующем руководстве.)</span><span class="sxs-lookup"><span data-stu-id="5a889-169">Log in/out, Identity (This is done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="5a889-170">Контроллеры и представления</span><span class="sxs-lookup"><span data-stu-id="5a889-170">Controllers and views</span></span>

* <span data-ttu-id="5a889-171">Копирование каждого из методов из ASP.NET MVC `HomeController` к новому `HomeController`.</span><span class="sxs-lookup"><span data-stu-id="5a889-171">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="5a889-172">Обратите внимание, что в ASP.NET MVC, тип возвращаемого значения метода действия контроллера встроенного шаблона [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); в ASP.NET Core MVC, возвращаемое методы действий `IActionResult` вместо этого.</span><span class="sxs-lookup"><span data-stu-id="5a889-172">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="5a889-173">`ActionResult` реализует `IActionResult`, поэтому нет необходимости, чтобы изменить тип возвращаемого значения своих методов действий.</span><span class="sxs-lookup"><span data-stu-id="5a889-173">`ActionResult` implements `IActionResult`, so there's no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="5a889-174">Копировать *About.cshtml*, *Contact.cshtml*, и *Index.cshtml* файлы представления Razor из проекта ASP.NET MVC в проект ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5a889-174">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="5a889-175">Запустите приложение ASP.NET Core и каждого метода теста.</span><span class="sxs-lookup"><span data-stu-id="5a889-175">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="5a889-176">Мы еще не перенести файл макета или стили еще, поэтому просмотра представления только с содержимым в файлах представления.</span><span class="sxs-lookup"><span data-stu-id="5a889-176">We haven't migrated the layout file or styles yet, so the rendered views only contain the content in the view files.</span></span> <span data-ttu-id="5a889-177">Будут ссылки файла, созданного макета для `About` и `Contact` просматривает, поэтому вы должны вызывать их из браузера (Замените **4492** с номером порта, используемый в проекте).</span><span class="sxs-lookup"><span data-stu-id="5a889-177">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Страница](mvc/_static/contact-page.png)

<span data-ttu-id="5a889-179">Обратите внимание, отсутствие элементов меню и использования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="5a889-179">Note the lack of styling and menu items.</span></span> <span data-ttu-id="5a889-180">Мы исправим это в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="5a889-180">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="5a889-181">Статическое содержимое</span><span class="sxs-lookup"><span data-stu-id="5a889-181">Static content</span></span>

<span data-ttu-id="5a889-182">В предыдущих версиях ASP.NET MVC статическое содержимое было размещено в корне веб-проекта и был смешиваться с файлами на сервере.</span><span class="sxs-lookup"><span data-stu-id="5a889-182">In previous versions of ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="5a889-183">В ASP.NET Core, статическое содержимое размещается в *wwwroot* папки.</span><span class="sxs-lookup"><span data-stu-id="5a889-183">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="5a889-184">Нужно будет скопировать статическое содержимое из старого приложения ASP.NET MVC для *wwwroot* в проекте ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5a889-184">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="5a889-185">В этот пример преобразования:</span><span class="sxs-lookup"><span data-stu-id="5a889-185">In this sample conversion:</span></span>

* <span data-ttu-id="5a889-186">Копировать *favicon.ico* файл из старого проекта MVC *wwwroot* папки в проекте ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5a889-186">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="5a889-187">ASP.NET MVC старый проект использует [Bootstrap](https://getbootstrap.com/) для его стилей и сохраняет файлы начальной загрузки *содержимого* и *сценариев* папки.</span><span class="sxs-lookup"><span data-stu-id="5a889-187">The old ASP.NET MVC project uses [Bootstrap](https://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="5a889-188">Шаблон, который создан старого проекта ASP.NET MVC, ссылается на начальной загрузки в файле макета (*Views/Shared/_Layout.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="5a889-188">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="5a889-189">Можно скопировать *bootstrap.js* и *bootstrap.css* проект, файлы из ASP.NET MVC *wwwroot* папку в новом проекте.</span><span class="sxs-lookup"><span data-stu-id="5a889-189">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project.</span></span> <span data-ttu-id="5a889-190">Вместо этого мы добавим поддержку для начальной загрузки (и другие клиентские библиотеки), с помощью CDN в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="5a889-190">Instead, we'll add support for Bootstrap (and other client-side libraries) using CDNs in the next section.</span></span>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="5a889-191">Перенесите файл макета</span><span class="sxs-lookup"><span data-stu-id="5a889-191">Migrate the layout file</span></span>

* <span data-ttu-id="5a889-192">Копировать *_ViewStart.cshtml* файла из старого проекта ASP.NET MVC *представления* папки в проекте ASP.NET Core *представления* папки.</span><span class="sxs-lookup"><span data-stu-id="5a889-192">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="5a889-193">*_ViewStart.cshtml* файл не был изменен в ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="5a889-193">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="5a889-194">Создание *Views/Shared* папки.</span><span class="sxs-lookup"><span data-stu-id="5a889-194">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="5a889-195">*Необязательно:* копирования *_ViewImports.cshtml* из *FullAspNetCore* проект MVC *представления* папки в проекте ASP.NET Core  *Представления* папки.</span><span class="sxs-lookup"><span data-stu-id="5a889-195">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="5a889-196">Удалите все объявления пространства имен в *_ViewImports.cshtml* файла.</span><span class="sxs-lookup"><span data-stu-id="5a889-196">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="5a889-197">*_ViewImports.cshtml* файл предоставляют пространства имен для всех файлов представлений и переводит [вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="5a889-197">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="5a889-198">Вспомогательные функции тегов используются в файл макета.</span><span class="sxs-lookup"><span data-stu-id="5a889-198">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="5a889-199">*_ViewImports.cshtml* файл появился в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5a889-199">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="5a889-200">Копировать *_Layout.cshtml* файла из старого проекта ASP.NET MVC *Views/Shared* папки в проекте ASP.NET Core *Views/Shared* папки.</span><span class="sxs-lookup"><span data-stu-id="5a889-200">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="5a889-201">Откройте *_Layout.cshtml* файл и внесите следующие изменения (ниже приведен полный код):</span><span class="sxs-lookup"><span data-stu-id="5a889-201">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

* <span data-ttu-id="5a889-202">Замените `@Styles.Render("~/Content/css")` с `<link>` элемент загрузить *bootstrap.css* (см. ниже).</span><span class="sxs-lookup"><span data-stu-id="5a889-202">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

* <span data-ttu-id="5a889-203">Удалите `@Scripts.Render("~/bundles/modernizr")`.</span><span class="sxs-lookup"><span data-stu-id="5a889-203">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

* <span data-ttu-id="5a889-204">Закомментируйте `@Html.Partial("_LoginPartial")` строки (заключить строку с `@*...*@`).</span><span class="sxs-lookup"><span data-stu-id="5a889-204">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="5a889-205">Дополнительные сведения см. в разделе [перенести проверки подлинности и удостоверения в ASP.NET Core](xref:migration/identity)</span><span class="sxs-lookup"><span data-stu-id="5a889-205">For more information see [Migrate Authentication and Identity to ASP.NET Core](xref:migration/identity)</span></span>

* <span data-ttu-id="5a889-206">Замените `@Scripts.Render("~/bundles/jquery")` с `<script>` элемент (см. ниже).</span><span class="sxs-lookup"><span data-stu-id="5a889-206">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

* <span data-ttu-id="5a889-207">Замените `@Scripts.Render("~/bundles/bootstrap")` с `<script>` элемент (см. ниже).</span><span class="sxs-lookup"><span data-stu-id="5a889-207">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below).</span></span>

<span data-ttu-id="5a889-208">Разметка замены для включения Bootstrap CSS:</span><span class="sxs-lookup"><span data-stu-id="5a889-208">The replacement markup for Bootstrap CSS inclusion:</span></span>

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

<span data-ttu-id="5a889-209">Разметка замены для jQuery и включения JavaScript начальной загрузки:</span><span class="sxs-lookup"><span data-stu-id="5a889-209">The replacement markup for jQuery and Bootstrap JavaScript inclusion:</span></span>

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

<span data-ttu-id="5a889-210">Обновленный *_Layout.cshtml* файла приведен ниже:</span><span class="sxs-lookup"><span data-stu-id="5a889-210">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

<span data-ttu-id="5a889-211">Откройте сайт в браузере.</span><span class="sxs-lookup"><span data-stu-id="5a889-211">View the site in the browser.</span></span> <span data-ttu-id="5a889-212">Он должен загружаться корректно, ожидаемый стили на месте.</span><span class="sxs-lookup"><span data-stu-id="5a889-212">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="5a889-213">*Необязательно:* может потребоваться попробуйте использовать файл макета.</span><span class="sxs-lookup"><span data-stu-id="5a889-213">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="5a889-214">Для этого проекта можно скопировать файл макета из *FullAspNetCore* проекта.</span><span class="sxs-lookup"><span data-stu-id="5a889-214">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="5a889-215">Использует файл макета [вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) и другие усовершенствования.</span><span class="sxs-lookup"><span data-stu-id="5a889-215">The new layout file uses [Tag Helpers](xref:mvc/views/tag-helpers/intro) and has other improvements.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="5a889-216">Настроить объединение и Минификация</span><span class="sxs-lookup"><span data-stu-id="5a889-216">Configure bundling and minification</span></span>

<span data-ttu-id="5a889-217">Сведения о способах настройки объединения и минификации, см. в разделе [объединение и Минификация](../client-side/bundling-and-minification.md).</span><span class="sxs-lookup"><span data-stu-id="5a889-217">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solve-http-500-errors"></a><span data-ttu-id="5a889-218">Устранить ошибки HTTP 500</span><span class="sxs-lookup"><span data-stu-id="5a889-218">Solve HTTP 500 errors</span></span>

<span data-ttu-id="5a889-219">Существует множество проблем, которые могут вызвать сообщение об ошибке HTTP 500, которые не содержат сведений о источник проблемы.</span><span class="sxs-lookup"><span data-stu-id="5a889-219">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="5a889-220">Например если *Views/_ViewImports.cshtml* файл содержит пространство имен, который не существует в проекте, то вы получите ошибку HTTP 500.</span><span class="sxs-lookup"><span data-stu-id="5a889-220">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="5a889-221">По умолчанию в приложениях ASP.NET Core `UseDeveloperExceptionPage` расширение добавляется к `IApplicationBuilder` и выполняется, когда конфигурация *разработки*.</span><span class="sxs-lookup"><span data-stu-id="5a889-221">By default in ASP.NET Core apps, the `UseDeveloperExceptionPage` extension is added to the `IApplicationBuilder` and executed when the configuration is *Development*.</span></span> <span data-ttu-id="5a889-222">Это подробно описано в следующем коде:</span><span class="sxs-lookup"><span data-stu-id="5a889-222">This is detailed in the following code:</span></span>

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

<span data-ttu-id="5a889-223">ASP.NET Core преобразует необработанные исключения в веб-приложения в сообщения об ошибках HTTP 500.</span><span class="sxs-lookup"><span data-stu-id="5a889-223">ASP.NET Core converts unhandled exceptions in a web app into HTTP 500 error responses.</span></span> <span data-ttu-id="5a889-224">Как правило сведения об ошибке не включены в эти ответы для предотвращения раскрытия потенциально конфиденциальной информации о сервере.</span><span class="sxs-lookup"><span data-stu-id="5a889-224">Normally, error details aren't included in these responses to prevent disclosure of potentially sensitive information about the server.</span></span> <span data-ttu-id="5a889-225">См. в разделе **с помощью страница исключения для разработчика** в [обработки ошибок](../fundamentals/error-handling.md) Дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="5a889-225">See **Using the Developer Exception Page** in [Handle errors](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5a889-226">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="5a889-226">Additional resources</span></span>

* [<span data-ttu-id="5a889-227">Клиентская разработка</span><span class="sxs-lookup"><span data-stu-id="5a889-227">Client-side development</span></span>](xref:client-side/index)
* [<span data-ttu-id="5a889-228">Вспомогательные функции тегов</span><span class="sxs-lookup"><span data-stu-id="5a889-228">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
