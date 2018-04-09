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
ms.openlocfilehash: e249be06726b307a1c41a525a132f7e0ab8b50ee
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="c6299-103">Перенести из ASP.NET MVC в ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="c6299-103">Migrate from ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="c6299-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT), [рот Daniel](https://github.com/danroth27), [Стив Смит](https://ardalis.com/), и [Скотт Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="c6299-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="c6299-105">В этой статье показано, как начать перенос проекта ASP.NET MVC для [ASP.NET Core MVC](../mvc/overview.md).</span><span class="sxs-lookup"><span data-stu-id="c6299-105">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="c6299-106">В процессе выделяются многих вещах, которые были изменены с ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c6299-106">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="c6299-107">Миграция с ASP.NET MVC состоит из нескольких этапов, и в этой статье рассматриваются начальной настройки, основные контроллеры и представления, статическое содержимое и зависимости на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="c6299-107">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="c6299-108">Дополнительные статьи описывается Миграция конфигурации и удостоверения код, который содержится во многих проектах ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c6299-108">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="c6299-109">Номера версий в образцы могут устареть.</span><span class="sxs-lookup"><span data-stu-id="c6299-109">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="c6299-110">Может потребоваться обновить проекты, соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="c6299-110">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="c6299-111">Создание начального проекта ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="c6299-111">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="c6299-112">Для демонстрации обновления, мы начнем с создания приложения ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c6299-112">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="c6299-113">Создать его с именем *WebApp1* , пространство имен будет соответствовать проекта ASP.NET Core, создается в следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="c6299-113">Create it with the name *WebApp1* so the namespace will match the ASP.NET Core project we create in the next step.</span></span>

![Диалоговое окно Visual Studio новый проект](mvc/_static/new-project.png)

![Диалоговое окно создания веб-приложения: шаблон проекта MVC, выбранного на панели шаблонов ASP.NET](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="c6299-116">*Необязательно:* изменить имя решения из *WebApp1* для *Mvc5*.</span><span class="sxs-lookup"><span data-stu-id="c6299-116">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="c6299-117">Visual Studio будет отображаться имя нового решения (*Mvc5*), который облегчит сообщить этот проект из проекта Далее.</span><span class="sxs-lookup"><span data-stu-id="c6299-117">Visual Studio will display the new solution name (*Mvc5*), which will make it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="c6299-118">Создание проекта ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c6299-118">Create the ASP.NET Core project</span></span>

<span data-ttu-id="c6299-119">Создайте новый *пустой* веб-приложения ASP.NET Core с тем же именем, что предыдущий проект (*WebApp1*), соответствуют пространства имен в двух проектов.</span><span class="sxs-lookup"><span data-stu-id="c6299-119">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="c6299-120">Если то же пространство имен проще скопировать код в двух проектах.</span><span class="sxs-lookup"><span data-stu-id="c6299-120">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="c6299-121">Необходимо создать этот проект в каталоге, отличном от предыдущий проект, чтобы использовать то же имя.</span><span class="sxs-lookup"><span data-stu-id="c6299-121">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![Диалоговое окно создания нового проекта](mvc/_static/new_core.png)

![Диалоговое окно создания веб-приложения ASP.NET: шаблон пустого проекта, выбранного в панели ASP.NET базовых шаблонов](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="c6299-124">*Необязательно:* Создание нового приложения ASP.NET Core с использованием *веб-приложение* шаблона проекта.</span><span class="sxs-lookup"><span data-stu-id="c6299-124">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="c6299-125">Назовите проект *WebApp1*и выберите нужный вариант проверки подлинности из **индивидуальные учетные записи**.</span><span class="sxs-lookup"><span data-stu-id="c6299-125">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="c6299-126">Переименование этого приложения *FullAspNetCore*.</span><span class="sxs-lookup"><span data-stu-id="c6299-126">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="c6299-127">Создание этого проекта позволит сэкономить время при преобразовании.</span><span class="sxs-lookup"><span data-stu-id="c6299-127">Creating this project will save you time in the conversion.</span></span> <span data-ttu-id="c6299-128">Можно взглянуть на код создан шаблон, см. в результате или скопировать код проекта преобразования.</span><span class="sxs-lookup"><span data-stu-id="c6299-128">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="c6299-129">Это также полезно, когда возникли затруднения на шаг преобразования для сравнения с проектом, созданных в шаблоне.</span><span class="sxs-lookup"><span data-stu-id="c6299-129">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="c6299-130">Настройка сайта для использования MVC</span><span class="sxs-lookup"><span data-stu-id="c6299-130">Configure the site to use MVC</span></span>

* <span data-ttu-id="c6299-131">Установка `Microsoft.AspNetCore.Mvc` и `Microsoft.AspNetCore.StaticFiles` пакеты NuGet.</span><span class="sxs-lookup"><span data-stu-id="c6299-131">Install the `Microsoft.AspNetCore.Mvc` and `Microsoft.AspNetCore.StaticFiles` NuGet packages.</span></span>

  <span data-ttu-id="c6299-132">`Microsoft.AspNetCore.Mvc` — Это платформа ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="c6299-132">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="c6299-133">`Microsoft.AspNetCore.StaticFiles` метод является обработчиком статических файлов.</span><span class="sxs-lookup"><span data-stu-id="c6299-133">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="c6299-134">Среда выполнения ASP.NET является модульной средой, и необходимо явно включить обновление через обрабатывать статические файлы (в разделе [работать с статические файлы](../fundamentals/static-files.md)).</span><span class="sxs-lookup"><span data-stu-id="c6299-134">The ASP.NET runtime is modular, and you must explicitly opt in to serve static files (see [Work with static files](../fundamentals/static-files.md)).</span></span>

* <span data-ttu-id="c6299-135">Откройте *.csproj* файла (щелкните правой кнопкой мыши проект в **обозревателе решений** и выберите **изменить WebApp1.csproj**) и добавьте `PrepareForPublish` целевой:</span><span class="sxs-lookup"><span data-stu-id="c6299-135">Open the *.csproj* file (right-click the project in **Solution Explorer** and select **Edit WebApp1.csproj**) and add a `PrepareForPublish` target:</span></span>

  [!code-xml[](mvc/sample/WebApp1.csproj?range=21-23)]

  <span data-ttu-id="c6299-136">`PrepareForPublish` Целевой необходим для получения клиентские библиотеки с помощью Bower.</span><span class="sxs-lookup"><span data-stu-id="c6299-136">The `PrepareForPublish` target is needed for acquiring client-side libraries via Bower.</span></span> <span data-ttu-id="c6299-137">Мы рассмотрим это более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="c6299-137">We'll talk about that later.</span></span>

* <span data-ttu-id="c6299-138">Откройте *файла Startup.cs* и измените код в соответствии со следующим:</span><span class="sxs-lookup"><span data-stu-id="c6299-138">Open the *Startup.cs* file and change the code to match the following:</span></span>

  [!code-csharp[](mvc/sample/Startup.cs?highlight=14,27-34)]

  <span data-ttu-id="c6299-139">`UseStaticFiles` Метод расширения добавляет обработчик файла статистики.</span><span class="sxs-lookup"><span data-stu-id="c6299-139">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="c6299-140">Как упоминалось ранее, среда выполнения ASP.NET является модульной средой, и необходимо явно включить обновление через обрабатывать статические файлы.</span><span class="sxs-lookup"><span data-stu-id="c6299-140">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="c6299-141">`UseMvc` Добавляет метод расширения маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="c6299-141">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="c6299-142">Дополнительные сведения см. в разделе [запуска приложения](../fundamentals/startup.md) и [маршрутизации](../fundamentals/routing.md).</span><span class="sxs-lookup"><span data-stu-id="c6299-142">For more information, see [Application Startup](../fundamentals/startup.md) and [Routing](../fundamentals/routing.md).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="c6299-143">Добавление контроллера и представления</span><span class="sxs-lookup"><span data-stu-id="c6299-143">Add a controller and view</span></span>

<span data-ttu-id="c6299-144">В этом разделе вы добавите минимальной контроллер и представление в качестве заполнителей для контроллера ASP.NET MVC и представления, будет перенести в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="c6299-144">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="c6299-145">Добавить *контроллеров* папки.</span><span class="sxs-lookup"><span data-stu-id="c6299-145">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="c6299-146">Добавить **класс контроллера MVC** с именем *HomeController.cs* для *контроллеров* папки.</span><span class="sxs-lookup"><span data-stu-id="c6299-146">Add an **MVC controller class** with the name *HomeController.cs* to the *Controllers* folder.</span></span>

![Диалоговое окно ''Добавление нового элемента''](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="c6299-148">Добавить *представления* папки.</span><span class="sxs-lookup"><span data-stu-id="c6299-148">Add a *Views* folder.</span></span>

* <span data-ttu-id="c6299-149">Добавить *представления/домашние* папки.</span><span class="sxs-lookup"><span data-stu-id="c6299-149">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="c6299-150">Добавить *Index.cshtml* страница представления MVC для *представления/домашние* папки.</span><span class="sxs-lookup"><span data-stu-id="c6299-150">Add an *Index.cshtml* MVC view page to the *Views/Home* folder.</span></span>

![Диалоговое окно ''Добавление нового элемента''](mvc/_static/view.png)

<span data-ttu-id="c6299-152">Структура проекта, показано ниже:</span><span class="sxs-lookup"><span data-stu-id="c6299-152">The project structure is shown below:</span></span>

![В обозревателе решений файлы и папки с WebApp1](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="c6299-154">Замените содержимое *Views/Home/Index.cshtml* файл со следующим:</span><span class="sxs-lookup"><span data-stu-id="c6299-154">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="c6299-155">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="c6299-155">Run the app.</span></span>

![Открыть в Microsoft Edge веб-приложения](mvc/_static/hello-world.png)

<span data-ttu-id="c6299-157">В разделе [контроллеров](xref:mvc/controllers/actions) и [представления](xref:mvc/views/overview) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="c6299-157">See [Controllers](xref:mvc/controllers/actions) and [Views](xref:mvc/views/overview) for more information.</span></span>

<span data-ttu-id="c6299-158">Теперь, когда у нас есть минимальный рабочий проект ASP.NET Core, мы можем запустить перенос функциональные возможности из проекта ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c6299-158">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="c6299-159">Нам потребуется переместить следующее:</span><span class="sxs-lookup"><span data-stu-id="c6299-159">We will need to move the following:</span></span>

* <span data-ttu-id="c6299-160">клиентское содержимое (CSS, шрифты и скрипты.)</span><span class="sxs-lookup"><span data-stu-id="c6299-160">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="c6299-161">контроллеры</span><span class="sxs-lookup"><span data-stu-id="c6299-161">controllers</span></span>

* <span data-ttu-id="c6299-162">представления</span><span class="sxs-lookup"><span data-stu-id="c6299-162">views</span></span>

* <span data-ttu-id="c6299-163">модели</span><span class="sxs-lookup"><span data-stu-id="c6299-163">models</span></span>

* <span data-ttu-id="c6299-164">Объединение</span><span class="sxs-lookup"><span data-stu-id="c6299-164">bundling</span></span>

* <span data-ttu-id="c6299-165">фильтры</span><span class="sxs-lookup"><span data-stu-id="c6299-165">filters</span></span>

* <span data-ttu-id="c6299-166">Журнал in/out, identity (это будет сделано в следующем уроке.)</span><span class="sxs-lookup"><span data-stu-id="c6299-166">Log in/out, identity (This will be done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="c6299-167">Контроллеры и представления</span><span class="sxs-lookup"><span data-stu-id="c6299-167">Controllers and views</span></span>

* <span data-ttu-id="c6299-168">Копирование всех методов из ASP.NET MVC `HomeController` к новому `HomeController`.</span><span class="sxs-lookup"><span data-stu-id="c6299-168">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="c6299-169">Обратите внимание, что в ASP.NET MVC, возвращаемый тип метода действия встроенного шаблона контроллера [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); в ASP.NET MVC Core, возвращаемого методами `IActionResult` вместо него.</span><span class="sxs-lookup"><span data-stu-id="c6299-169">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="c6299-170">`ActionResult` реализует `IActionResult`, поэтому нет необходимости изменять тип возвращаемого значения методов действия.</span><span class="sxs-lookup"><span data-stu-id="c6299-170">`ActionResult` implements `IActionResult`, so there's no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="c6299-171">Копировать *About.cshtml*, *Contact.cshtml*, и *Index.cshtml* Razor Просмотр файлов из проекта ASP.NET MVC в проект ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c6299-171">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="c6299-172">Запуск приложения ASP.NET Core и каждый метод теста.</span><span class="sxs-lookup"><span data-stu-id="c6299-172">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="c6299-173">Мы еще не перенесены файл макета или стили еще, готовый для просмотра представления будет содержать только содержимое в файлы представления.</span><span class="sxs-lookup"><span data-stu-id="c6299-173">We haven't migrated the layout file or styles yet, so the rendered views will only contain the content in the view files.</span></span> <span data-ttu-id="c6299-174">Вы не будете иметь ссылки файла, созданного макета для `About` и `Contact` просматривает, таким образом вы получаете вызывать их из браузера (Замените **4492** с номером порта, используемые в вашем проекте).</span><span class="sxs-lookup"><span data-stu-id="c6299-174">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Страницы контактов](mvc/_static/contact-page.png)

<span data-ttu-id="c6299-176">Обратите внимание, отсутствие Стилизация и элементами меню.</span><span class="sxs-lookup"><span data-stu-id="c6299-176">Note the lack of styling and menu items.</span></span> <span data-ttu-id="c6299-177">Мы исправим это в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="c6299-177">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="c6299-178">Статическое содержимое</span><span class="sxs-lookup"><span data-stu-id="c6299-178">Static content</span></span>

<span data-ttu-id="c6299-179">В предыдущих версиях ASP.NET MVC статическое содержимое было размещено из корневого каталога веб-проекта и был перемежаемый файлов на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="c6299-179">In previous versions of  ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="c6299-180">В ASP.NET Core статическое содержимое размещается в *wwwroot* папки.</span><span class="sxs-lookup"><span data-stu-id="c6299-180">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="c6299-181">Может потребоваться скопировать статическое содержимое из старого приложения ASP.NET MVC для *wwwroot* в папке проекта ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c6299-181">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="c6299-182">В данном примере:</span><span class="sxs-lookup"><span data-stu-id="c6299-182">In this sample conversion:</span></span>

* <span data-ttu-id="c6299-183">Копировать *favicon.ico* файла из старого проекта MVC *wwwroot* папку в проекте ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c6299-183">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="c6299-184">Используется в проекте ASP.NET MVC старый [начальной загрузки](http://getbootstrap.com/) для его задания стиля и хранилищ, программу начальной загрузки файлов в *содержимого* и *сценариев* папки.</span><span class="sxs-lookup"><span data-stu-id="c6299-184">The old ASP.NET MVC project uses [Bootstrap](http://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="c6299-185">Шаблон, который создан старого проекта ASP.NET MVC, ссылается на начальной загрузки в файл макета (*Views/Shared/_Layout.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="c6299-185">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="c6299-186">Можно скопировать *bootstrap.js* и *bootstrap.css* файлы из ASP.NET MVC проекта для *wwwroot* папки в новый проект, но такой подход не используется улучшенный механизм для управления зависимости на стороне клиента в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c6299-186">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project, but that approach doesn't use the improved mechanism for managing client-side dependencies in ASP.NET Core.</span></span>

<span data-ttu-id="c6299-187">В новом проекте, мы добавим поддержку для начальной загрузки (и другие клиентские библиотеки) с помощью [Bower](https://bower.io/):</span><span class="sxs-lookup"><span data-stu-id="c6299-187">In the new project, we'll add support for Bootstrap (and other client-side libraries) using [Bower](https://bower.io/):</span></span>

* <span data-ttu-id="c6299-188">Добавить [Bower](https://bower.io/) файл конфигурации с именем *bower.json* с корнем проекта (правой кнопкой мыши проект, а затем **Добавить > новый элемент > файла конфигурации для Bower**).</span><span class="sxs-lookup"><span data-stu-id="c6299-188">Add a [Bower](https://bower.io/) configuration file named *bower.json* to the project root (Right-click on the project, and then **Add > New Item > Bower Configuration File**).</span></span> <span data-ttu-id="c6299-189">Добавить [начальной загрузки](http://getbootstrap.com/) и [jQuery](https://jquery.com/) в файл (см. выделенные строки ниже).</span><span class="sxs-lookup"><span data-stu-id="c6299-189">Add [Bootstrap](http://getbootstrap.com/) and [jQuery](https://jquery.com/) to the file (see the highlighted lines below).</span></span>

  [!code-json[](mvc/sample/bower.json?highlight=5-6)]

<span data-ttu-id="c6299-190">После сохранения файла, Bower автоматически загрузит зависимости, чтобы *wwwroot/lib* папки.</span><span class="sxs-lookup"><span data-stu-id="c6299-190">Upon saving the file, Bower will automatically download the dependencies to the *wwwroot/lib* folder.</span></span> <span data-ttu-id="c6299-191">Можно использовать **поиск в обозревателе решений** флажок, чтобы найти путь к папке средств:</span><span class="sxs-lookup"><span data-stu-id="c6299-191">You can use the **Search Solution Explorer** box to find the path of the assets:</span></span>

![активы jQuery, показаны в результатах поиска обозревателя решений](mvc/_static/search.png)

<span data-ttu-id="c6299-193">В разделе [управления клиентских пакетов с Bower](../client-side/bower.md) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="c6299-193">See [Manage Client-Side Packages with Bower](../client-side/bower.md) for more information.</span></span>

<a name="migrate-layout-file"></a>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="c6299-194">Перенесите файл макета</span><span class="sxs-lookup"><span data-stu-id="c6299-194">Migrate the layout file</span></span>

* <span data-ttu-id="c6299-195">Копировать *файл _ViewStart.cshtml* файла из старого проекта ASP.NET MVC *представления* папки в проект ASP.NET Core *представления* папки.</span><span class="sxs-lookup"><span data-stu-id="c6299-195">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="c6299-196">*Файл _ViewStart.cshtml* файл не был изменен в ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="c6299-196">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="c6299-197">Создание *представления/Общие* папки.</span><span class="sxs-lookup"><span data-stu-id="c6299-197">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="c6299-198">*Необязательно:* копирования *_ViewImports.cshtml* из *FullAspNetCore* проекта MVC *представления* папки в проект ASP.NET Core  *Представления* папки.</span><span class="sxs-lookup"><span data-stu-id="c6299-198">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="c6299-199">Удалите все объявления пространства имен в *_ViewImports.cshtml* файла.</span><span class="sxs-lookup"><span data-stu-id="c6299-199">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="c6299-200">*_ViewImports.cshtml* файла предоставляют пространства имен для всех файлов, представления, после чего [вспомогательных функций тегов](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="c6299-200">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="c6299-201">В новом файле макета используются вспомогательных функций тегов.</span><span class="sxs-lookup"><span data-stu-id="c6299-201">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="c6299-202">*_ViewImports.cshtml* файл впервые ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c6299-202">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="c6299-203">Копировать *_Layout.cshtml* файла из старого проекта ASP.NET MVC *представления/Общие* папки в проект ASP.NET Core *представления/Общие* папки.</span><span class="sxs-lookup"><span data-stu-id="c6299-203">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="c6299-204">Откройте *_Layout.cshtml* файл и внесите следующие изменения (ниже приведен полный код):</span><span class="sxs-lookup"><span data-stu-id="c6299-204">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

   * <span data-ttu-id="c6299-205">Замените `@Styles.Render("~/Content/css")` с `<link>` элемента, который требуется загрузить *bootstrap.css* (см. ниже).</span><span class="sxs-lookup"><span data-stu-id="c6299-205">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

   * <span data-ttu-id="c6299-206">Удалите `@Scripts.Render("~/bundles/modernizr")`.</span><span class="sxs-lookup"><span data-stu-id="c6299-206">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

   * <span data-ttu-id="c6299-207">Закомментируйте `@Html.Partial("_LoginPartial")` строки (заключить строку с `@*...*@`).</span><span class="sxs-lookup"><span data-stu-id="c6299-207">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="c6299-208">Мы вернемся к нему в будущем учебника.</span><span class="sxs-lookup"><span data-stu-id="c6299-208">We'll return to it in a future tutorial.</span></span>

   * <span data-ttu-id="c6299-209">Замените `@Scripts.Render("~/bundles/jquery")` с `<script>` элемент (см. ниже).</span><span class="sxs-lookup"><span data-stu-id="c6299-209">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

   * <span data-ttu-id="c6299-210">Замените `@Scripts.Render("~/bundles/bootstrap")` с `<script>` элемент (см. ниже)...</span><span class="sxs-lookup"><span data-stu-id="c6299-210">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below)..</span></span>

<span data-ttu-id="c6299-211">Ссылка CSS замены:</span><span class="sxs-lookup"><span data-stu-id="c6299-211">The replacement CSS link:</span></span>

```html
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
```

<span data-ttu-id="c6299-212">Теги сценариев замены:</span><span class="sxs-lookup"><span data-stu-id="c6299-212">The replacement script tags:</span></span>

```html
<script src="~/lib/jquery/dist/jquery.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
```

<span data-ttu-id="c6299-213">Обновленный *_Layout.cshtml* файла показан ниже:</span><span class="sxs-lookup"><span data-stu-id="c6299-213">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-html[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7,27,39-40)]

<span data-ttu-id="c6299-214">Просмотр сайта в браузере.</span><span class="sxs-lookup"><span data-stu-id="c6299-214">View the site in the browser.</span></span> <span data-ttu-id="c6299-215">Должны теперь загрузиться правильно, с ожидаемой стили на месте.</span><span class="sxs-lookup"><span data-stu-id="c6299-215">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="c6299-216">*Необязательно:* может потребоваться попробуйте использовать новый файл макета.</span><span class="sxs-lookup"><span data-stu-id="c6299-216">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="c6299-217">Для этого проекта можно скопировать файл из макета *FullAspNetCore* проекта.</span><span class="sxs-lookup"><span data-stu-id="c6299-217">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="c6299-218">В новом файле макета используется [вспомогательных функций тегов](xref:mvc/views/tag-helpers/intro) и других усовершенствований.</span><span class="sxs-lookup"><span data-stu-id="c6299-218">The new layout file uses [Tag Helpers](xref:mvc/views/tag-helpers/intro) and has other improvements.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="c6299-219">Настроить объединение и Минификация</span><span class="sxs-lookup"><span data-stu-id="c6299-219">Configure bundling and minification</span></span>

<span data-ttu-id="c6299-220">Сведения о настройке объединение и Минификация см. в разделе [объединении и Минификация](../client-side/bundling-and-minification.md).</span><span class="sxs-lookup"><span data-stu-id="c6299-220">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solving-http-500-errors"></a><span data-ttu-id="c6299-221">Ошибки HTTP 500 решения</span><span class="sxs-lookup"><span data-stu-id="c6299-221">Solving HTTP 500 errors</span></span>

<span data-ttu-id="c6299-222">Существуют многие проблемы, которые могут вызвать сообщение об ошибке HTTP 500, не содержат сведений о источник проблемы.</span><span class="sxs-lookup"><span data-stu-id="c6299-222">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="c6299-223">Например если *Views/_ViewImports.cshtml* файл содержит пространство имен, которые не существуют в проекте, вы получите ошибку HTTP 500.</span><span class="sxs-lookup"><span data-stu-id="c6299-223">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="c6299-224">Чтобы получить подробные сведения об ошибке, добавьте следующий код:</span><span class="sxs-lookup"><span data-stu-id="c6299-224">To get a detailed error message, add the following code:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    if (env.IsDevelopment())
    {
         app.UseDeveloperExceptionPage();
    }

    app.UseStaticFiles();

    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

<span data-ttu-id="c6299-225">В разделе **с помощью страницы исключение разработчика** в [обработки ошибок](../fundamentals/error-handling.md) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="c6299-225">See **Using the Developer Exception Page** in [Handle errors](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c6299-226">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="c6299-226">Additional resources</span></span>

* [<span data-ttu-id="c6299-227">Клиентская разработка</span><span class="sxs-lookup"><span data-stu-id="c6299-227">Client-Side Development</span></span>](xref:client-side/index)
* [<span data-ttu-id="c6299-228">Вспомогательные функции тегов</span><span class="sxs-lookup"><span data-stu-id="c6299-228">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
