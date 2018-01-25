---
title: "Миграция из ASP.NET MVC в ASP.NET Core MVC"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/mvc
ms.openlocfilehash: e3220fb32900aac42cf96497964936ad5b375a86
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="migrating-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="b65f3-102">Миграция из ASP.NET MVC в ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="b65f3-102">Migrating From ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="b65f3-103">По [Рик Андерсон](https://twitter.com/RickAndMSFT), [рот Daniel](https://github.com/danroth27), [Стив Смит](https://ardalis.com/), и [Скотт Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="b65f3-103">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="b65f3-104">В этой статье показано, как начать перенос проекта ASP.NET MVC для [ASP.NET Core MVC](../mvc/overview.md).</span><span class="sxs-lookup"><span data-stu-id="b65f3-104">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="b65f3-105">В процессе выделяются многих вещах, которые были изменены с ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b65f3-105">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="b65f3-106">Миграция с ASP.NET MVC состоит из нескольких этапов, и в этой статье рассматриваются начальной настройки, основные контроллеры и представления, статическое содержимое и зависимости на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="b65f3-106">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="b65f3-107">Дополнительные статьи описывается Миграция конфигурации и удостоверения код, который содержится во многих проектах ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b65f3-107">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="b65f3-108">Номера версий в образцы могут устареть.</span><span class="sxs-lookup"><span data-stu-id="b65f3-108">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="b65f3-109">Может потребоваться обновить проекты, соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="b65f3-109">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="b65f3-110">Создание начального проекта ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b65f3-110">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="b65f3-111">Для демонстрации обновления, мы начнем с создания приложения ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b65f3-111">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="b65f3-112">Создать его с именем *WebApp1* , пространство имен будет соответствовать проекта ASP.NET Core, создается в следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="b65f3-112">Create it with the name *WebApp1* so the namespace will match the ASP.NET Core project we create in the next step.</span></span>

![Диалоговое окно Visual Studio новый проект](mvc/_static/new-project.png)

![Диалоговое окно создания веб-приложения: шаблон проекта MVC, выбранного на панели шаблонов ASP.NET](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="b65f3-115">*Необязательно:* изменить имя решения из *WebApp1* для *Mvc5*.</span><span class="sxs-lookup"><span data-stu-id="b65f3-115">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="b65f3-116">Visual Studio будет отображаться имя нового решения (*Mvc5*), который облегчит сообщить этот проект из проекта Далее.</span><span class="sxs-lookup"><span data-stu-id="b65f3-116">Visual Studio will display the new solution name (*Mvc5*), which will make it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="b65f3-117">Создание проекта ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b65f3-117">Create the ASP.NET Core project</span></span>

<span data-ttu-id="b65f3-118">Создайте новый *пустой* веб-приложения ASP.NET Core с тем же именем, что предыдущий проект (*WebApp1*), соответствуют пространства имен в двух проектов.</span><span class="sxs-lookup"><span data-stu-id="b65f3-118">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="b65f3-119">Если то же пространство имен проще скопировать код в двух проектах.</span><span class="sxs-lookup"><span data-stu-id="b65f3-119">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="b65f3-120">Необходимо создать этот проект в каталоге, отличном от предыдущий проект, чтобы использовать то же имя.</span><span class="sxs-lookup"><span data-stu-id="b65f3-120">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![Диалоговое окно создания нового проекта](mvc/_static/new_core.png)

![Диалоговое окно создания веб-приложения ASP.NET: шаблон пустого проекта, выбранного в панели ASP.NET базовых шаблонов](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="b65f3-123">*Необязательно:* Создание нового приложения ASP.NET Core с использованием *веб-приложение* шаблона проекта.</span><span class="sxs-lookup"><span data-stu-id="b65f3-123">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="b65f3-124">Назовите проект *WebApp1*и выберите нужный вариант проверки подлинности из **индивидуальные учетные записи**.</span><span class="sxs-lookup"><span data-stu-id="b65f3-124">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="b65f3-125">Переименование этого приложения *FullAspNetCore*.</span><span class="sxs-lookup"><span data-stu-id="b65f3-125">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="b65f3-126">Создание этого проекта позволит сэкономить время при преобразовании.</span><span class="sxs-lookup"><span data-stu-id="b65f3-126">Creating this project will save you time in the conversion.</span></span> <span data-ttu-id="b65f3-127">Можно взглянуть на код создан шаблон, см. в результате или скопировать код проекта преобразования.</span><span class="sxs-lookup"><span data-stu-id="b65f3-127">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="b65f3-128">Это также полезно, когда возникли затруднения на шаг преобразования для сравнения с проектом, созданных в шаблоне.</span><span class="sxs-lookup"><span data-stu-id="b65f3-128">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="b65f3-129">Настройка сайта для использования MVC</span><span class="sxs-lookup"><span data-stu-id="b65f3-129">Configure the site to use MVC</span></span>

* <span data-ttu-id="b65f3-130">Установка `Microsoft.AspNetCore.Mvc` и `Microsoft.AspNetCore.StaticFiles` пакеты NuGet.</span><span class="sxs-lookup"><span data-stu-id="b65f3-130">Install the `Microsoft.AspNetCore.Mvc` and `Microsoft.AspNetCore.StaticFiles` NuGet packages.</span></span>

  <span data-ttu-id="b65f3-131">`Microsoft.AspNetCore.Mvc`— Это платформа ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="b65f3-131">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="b65f3-132">`Microsoft.AspNetCore.StaticFiles`метод является обработчиком статических файлов.</span><span class="sxs-lookup"><span data-stu-id="b65f3-132">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="b65f3-133">Среда выполнения ASP.NET является модульной средой, и необходимо явно включить обновление через обрабатывать статические файлы (в разделе [работа с статические файлы](../fundamentals/static-files.md)).</span><span class="sxs-lookup"><span data-stu-id="b65f3-133">The ASP.NET runtime is modular, and you must explicitly opt in to serve static files (see [Working with Static Files](../fundamentals/static-files.md)).</span></span>

* <span data-ttu-id="b65f3-134">Откройте *.csproj* файла (щелкните правой кнопкой мыши проект в **обозревателе решений** и выберите **изменить WebApp1.csproj**) и добавьте `PrepareForPublish` целевой:</span><span class="sxs-lookup"><span data-stu-id="b65f3-134">Open the *.csproj* file (right-click the project in **Solution Explorer** and select **Edit WebApp1.csproj**) and add a `PrepareForPublish` target:</span></span>

  [!code-xml[Main](mvc/sample/WebApp1.csproj?range=21-23)]

  <span data-ttu-id="b65f3-135">`PrepareForPublish` Целевой необходим для получения клиентские библиотеки с помощью Bower.</span><span class="sxs-lookup"><span data-stu-id="b65f3-135">The `PrepareForPublish` target is needed for acquiring client-side libraries via Bower.</span></span> <span data-ttu-id="b65f3-136">Мы рассмотрим это более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="b65f3-136">We'll talk about that later.</span></span>

* <span data-ttu-id="b65f3-137">Откройте *файла Startup.cs* и измените код в соответствии со следующим:</span><span class="sxs-lookup"><span data-stu-id="b65f3-137">Open the *Startup.cs* file and change the code to match the following:</span></span>

  [!code-csharp[Main](mvc/sample/Startup.cs?highlight=14,27-34)]

  <span data-ttu-id="b65f3-138">`UseStaticFiles` Метод расширения добавляет обработчик файла статистики.</span><span class="sxs-lookup"><span data-stu-id="b65f3-138">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="b65f3-139">Как упоминалось ранее, среда выполнения ASP.NET является модульной средой, и необходимо явно включить обновление через обрабатывать статические файлы.</span><span class="sxs-lookup"><span data-stu-id="b65f3-139">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="b65f3-140">`UseMvc` Добавляет метод расширения маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="b65f3-140">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="b65f3-141">Дополнительные сведения см. в разделе [запуска приложения](../fundamentals/startup.md) и [маршрутизации](../fundamentals/routing.md).</span><span class="sxs-lookup"><span data-stu-id="b65f3-141">For more information, see [Application Startup](../fundamentals/startup.md) and [Routing](../fundamentals/routing.md).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="b65f3-142">Добавление контроллера и представления</span><span class="sxs-lookup"><span data-stu-id="b65f3-142">Add a controller and view</span></span>

<span data-ttu-id="b65f3-143">В этом разделе вы добавите минимальной контроллер и представление в качестве заполнителей для контроллера ASP.NET MVC и представления, будет перенести в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="b65f3-143">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="b65f3-144">Добавить *контроллеров* папки.</span><span class="sxs-lookup"><span data-stu-id="b65f3-144">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="b65f3-145">Добавить **класс контроллера MVC** с именем *HomeController.cs* для *контроллеров* папки.</span><span class="sxs-lookup"><span data-stu-id="b65f3-145">Add an **MVC controller class** with the name *HomeController.cs* to the *Controllers* folder.</span></span>

![Диалоговое окно ''Добавление нового элемента''](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="b65f3-147">Добавить *представления* папки.</span><span class="sxs-lookup"><span data-stu-id="b65f3-147">Add a *Views* folder.</span></span>

* <span data-ttu-id="b65f3-148">Добавить *представления/домашние* папки.</span><span class="sxs-lookup"><span data-stu-id="b65f3-148">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="b65f3-149">Добавить *Index.cshtml* страница представления MVC для *представления/домашние* папки.</span><span class="sxs-lookup"><span data-stu-id="b65f3-149">Add an *Index.cshtml* MVC view page to the *Views/Home* folder.</span></span>

![Диалоговое окно ''Добавление нового элемента''](mvc/_static/view.png)

<span data-ttu-id="b65f3-151">Структура проекта, показано ниже:</span><span class="sxs-lookup"><span data-stu-id="b65f3-151">The project structure is shown below:</span></span>

![В обозревателе решений файлы и папки с WebApp1](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="b65f3-153">Замените содержимое *Views/Home/Index.cshtml* файл со следующим:</span><span class="sxs-lookup"><span data-stu-id="b65f3-153">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="b65f3-154">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="b65f3-154">Run the app.</span></span>

![Открыть в Microsoft Edge веб-приложения](mvc/_static/hello-world.png)

<span data-ttu-id="b65f3-156">В разделе [контроллеров](../mvc/controllers/index.md) и [представления](../mvc/views/index.md) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="b65f3-156">See [Controllers](../mvc/controllers/index.md) and [Views](../mvc/views/index.md) for more information.</span></span>

<span data-ttu-id="b65f3-157">Теперь, когда у нас есть минимальный рабочий проект ASP.NET Core, мы можем запустить перенос функциональные возможности из проекта ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b65f3-157">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="b65f3-158">Нам потребуется переместить следующее:</span><span class="sxs-lookup"><span data-stu-id="b65f3-158">We will need to move the following:</span></span>

* <span data-ttu-id="b65f3-159">клиентское содержимое (CSS, шрифты и скрипты.)</span><span class="sxs-lookup"><span data-stu-id="b65f3-159">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="b65f3-160">контроллеры</span><span class="sxs-lookup"><span data-stu-id="b65f3-160">controllers</span></span>

* <span data-ttu-id="b65f3-161">представления</span><span class="sxs-lookup"><span data-stu-id="b65f3-161">views</span></span>

* <span data-ttu-id="b65f3-162">модели</span><span class="sxs-lookup"><span data-stu-id="b65f3-162">models</span></span>

* <span data-ttu-id="b65f3-163">Объединение</span><span class="sxs-lookup"><span data-stu-id="b65f3-163">bundling</span></span>

* <span data-ttu-id="b65f3-164">фильтры</span><span class="sxs-lookup"><span data-stu-id="b65f3-164">filters</span></span>

* <span data-ttu-id="b65f3-165">Журнал in/out, identity (это будет сделано в следующем уроке.)</span><span class="sxs-lookup"><span data-stu-id="b65f3-165">Log in/out, identity (This will be done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="b65f3-166">Контроллеры и представления</span><span class="sxs-lookup"><span data-stu-id="b65f3-166">Controllers and views</span></span>

* <span data-ttu-id="b65f3-167">Копирование всех методов из ASP.NET MVC `HomeController` к новому `HomeController`.</span><span class="sxs-lookup"><span data-stu-id="b65f3-167">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="b65f3-168">Обратите внимание, что в ASP.NET MVC, возвращаемый тип метода действия встроенного шаблона контроллера [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); в ASP.NET MVC Core, возвращаемого методами `IActionResult` вместо него.</span><span class="sxs-lookup"><span data-stu-id="b65f3-168">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="b65f3-169">`ActionResult`реализует `IActionResult`, поэтому нет необходимости изменять тип возвращаемого значения методов действия.</span><span class="sxs-lookup"><span data-stu-id="b65f3-169">`ActionResult` implements `IActionResult`, so there's no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="b65f3-170">Копировать *About.cshtml*, *Contact.cshtml*, и *Index.cshtml* Razor Просмотр файлов из проекта ASP.NET MVC в проект ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b65f3-170">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="b65f3-171">Запуск приложения ASP.NET Core и каждый метод теста.</span><span class="sxs-lookup"><span data-stu-id="b65f3-171">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="b65f3-172">Мы еще не перенесены файл макета или стили еще, готовый для просмотра представления будет содержать только содержимое в файлы представления.</span><span class="sxs-lookup"><span data-stu-id="b65f3-172">We haven't migrated the layout file or styles yet, so the rendered views will only contain the content in the view files.</span></span> <span data-ttu-id="b65f3-173">Вы не будете иметь ссылки файла, созданного макета для `About` и `Contact` просматривает, таким образом вы получаете вызывать их из браузера (Замените **4492** с номером порта, используемые в вашем проекте).</span><span class="sxs-lookup"><span data-stu-id="b65f3-173">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Страницы контактов](mvc/_static/contact-page.png)

<span data-ttu-id="b65f3-175">Обратите внимание, отсутствие Стилизация и элементами меню.</span><span class="sxs-lookup"><span data-stu-id="b65f3-175">Note the lack of styling and menu items.</span></span> <span data-ttu-id="b65f3-176">Мы исправим это в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="b65f3-176">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="b65f3-177">Статическое содержимое</span><span class="sxs-lookup"><span data-stu-id="b65f3-177">Static content</span></span>

<span data-ttu-id="b65f3-178">В предыдущих версиях ASP.NET MVC статическое содержимое было размещено из корневого каталога веб-проекта и был перемежаемый файлов на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="b65f3-178">In previous versions of  ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="b65f3-179">В ASP.NET Core статическое содержимое размещается в *wwwroot* папки.</span><span class="sxs-lookup"><span data-stu-id="b65f3-179">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="b65f3-180">Может потребоваться скопировать статическое содержимое из старого приложения ASP.NET MVC для *wwwroot* в папке проекта ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b65f3-180">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="b65f3-181">В данном примере:</span><span class="sxs-lookup"><span data-stu-id="b65f3-181">In this sample conversion:</span></span>

* <span data-ttu-id="b65f3-182">Копировать *favicon.ico* файла из старого проекта MVC *wwwroot* папку в проекте ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b65f3-182">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="b65f3-183">Используется в проекте ASP.NET MVC старый [начальной загрузки](http://getbootstrap.com/) для его задания стиля и хранилищ, программу начальной загрузки файлов в *содержимого* и *сценариев* папки.</span><span class="sxs-lookup"><span data-stu-id="b65f3-183">The old ASP.NET MVC project uses [Bootstrap](http://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="b65f3-184">Шаблон, который создан старого проекта ASP.NET MVC, ссылается на начальной загрузки в файл макета (*Views/Shared/_Layout.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="b65f3-184">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="b65f3-185">Можно скопировать *bootstrap.js* и *bootstrap.css* файлы из ASP.NET MVC проекта для *wwwroot* папки в новый проект, но такой подход не используется улучшенный механизм для управления зависимости на стороне клиента в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b65f3-185">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project, but that approach doesn't use the improved mechanism for managing client-side dependencies in ASP.NET Core.</span></span>

<span data-ttu-id="b65f3-186">В новом проекте, мы добавим поддержку для начальной загрузки (и другие клиентские библиотеки) с помощью [Bower](https://bower.io/):</span><span class="sxs-lookup"><span data-stu-id="b65f3-186">In the new project, we'll add support for Bootstrap (and other client-side libraries) using [Bower](https://bower.io/):</span></span>

* <span data-ttu-id="b65f3-187">Добавить [Bower](https://bower.io/) файл конфигурации с именем *bower.json* с корнем проекта (правой кнопкой мыши проект, а затем **Добавить > новый элемент > файла конфигурации для Bower**).</span><span class="sxs-lookup"><span data-stu-id="b65f3-187">Add a [Bower](https://bower.io/) configuration file named *bower.json* to the project root (Right-click on the project, and then **Add > New Item > Bower Configuration File**).</span></span> <span data-ttu-id="b65f3-188">Добавить [начальной загрузки](http://getbootstrap.com/) и [jQuery](https://jquery.com/) в файл (см. выделенные строки ниже).</span><span class="sxs-lookup"><span data-stu-id="b65f3-188">Add [Bootstrap](http://getbootstrap.com/) and [jQuery](https://jquery.com/) to the file (see the highlighted lines below).</span></span>

  [!code-json[Main](mvc/sample/bower.json?highlight=5-6)]

<span data-ttu-id="b65f3-189">После сохранения файла, Bower автоматически загрузит зависимости, чтобы *wwwroot/lib* папки.</span><span class="sxs-lookup"><span data-stu-id="b65f3-189">Upon saving the file, Bower will automatically download the dependencies to the *wwwroot/lib* folder.</span></span> <span data-ttu-id="b65f3-190">Можно использовать **поиск в обозревателе решений** флажок, чтобы найти путь к папке средств:</span><span class="sxs-lookup"><span data-stu-id="b65f3-190">You can use the **Search Solution Explorer** box to find the path of the assets:</span></span>

![активы jQuery, показаны в результатах поиска обозревателя решений](mvc/_static/search.png)

<span data-ttu-id="b65f3-192">В разделе [управления клиентских пакетов с Bower](../client-side/bower.md) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="b65f3-192">See [Manage Client-Side Packages with Bower](../client-side/bower.md) for more information.</span></span>

<a name="migrate-layout-file"></a>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="b65f3-193">Перенесите файл макета</span><span class="sxs-lookup"><span data-stu-id="b65f3-193">Migrate the layout file</span></span>

* <span data-ttu-id="b65f3-194">Копировать *файл _ViewStart.cshtml* файла из старого проекта ASP.NET MVC *представления* папки в проект ASP.NET Core *представления* папки.</span><span class="sxs-lookup"><span data-stu-id="b65f3-194">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="b65f3-195">*Файл _ViewStart.cshtml* файл не был изменен в ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="b65f3-195">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="b65f3-196">Создание *представления/Общие* папки.</span><span class="sxs-lookup"><span data-stu-id="b65f3-196">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="b65f3-197">*Необязательно:* копирования *_ViewImports.cshtml* из *FullAspNetCore* проекта MVC *представления* папки в ASP.NET Core проекта *Представления* папки.</span><span class="sxs-lookup"><span data-stu-id="b65f3-197">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="b65f3-198">Удалите все объявления пространства имен в *_ViewImports.cshtml* файла.</span><span class="sxs-lookup"><span data-stu-id="b65f3-198">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="b65f3-199">*_ViewImports.cshtml* файла предоставляют пространства имен для всех файлов, представления, после чего [вспомогательных функций тегов](../mvc/views/tag-helpers/index.md).</span><span class="sxs-lookup"><span data-stu-id="b65f3-199">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](../mvc/views/tag-helpers/index.md).</span></span> <span data-ttu-id="b65f3-200">В новом файле макета используются вспомогательных функций тегов.</span><span class="sxs-lookup"><span data-stu-id="b65f3-200">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="b65f3-201">*_ViewImports.cshtml* файл впервые ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b65f3-201">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="b65f3-202">Копировать *_Layout.cshtml* файла из старого проекта ASP.NET MVC *представления/Общие* папки в проект ASP.NET Core *представления/Общие* папки.</span><span class="sxs-lookup"><span data-stu-id="b65f3-202">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="b65f3-203">Откройте *_Layout.cshtml* файл и внесите следующие изменения (ниже приведен полный код):</span><span class="sxs-lookup"><span data-stu-id="b65f3-203">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

   * <span data-ttu-id="b65f3-204">Замените `@Styles.Render("~/Content/css")` с `<link>` элемента, который требуется загрузить *bootstrap.css* (см. ниже).</span><span class="sxs-lookup"><span data-stu-id="b65f3-204">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

   * <span data-ttu-id="b65f3-205">Удалите `@Scripts.Render("~/bundles/modernizr")`.</span><span class="sxs-lookup"><span data-stu-id="b65f3-205">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

   * <span data-ttu-id="b65f3-206">Закомментируйте `@Html.Partial("_LoginPartial")` строки (заключить строку с `@*...*@`).</span><span class="sxs-lookup"><span data-stu-id="b65f3-206">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="b65f3-207">Мы вернемся к нему в будущем учебника.</span><span class="sxs-lookup"><span data-stu-id="b65f3-207">We'll return to it in a future tutorial.</span></span>

   * <span data-ttu-id="b65f3-208">Замените `@Scripts.Render("~/bundles/jquery")` с `<script>` элемент (см. ниже).</span><span class="sxs-lookup"><span data-stu-id="b65f3-208">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

   * <span data-ttu-id="b65f3-209">Замените `@Scripts.Render("~/bundles/bootstrap")` с `<script>` элемент (см. ниже)...</span><span class="sxs-lookup"><span data-stu-id="b65f3-209">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below)..</span></span>

<span data-ttu-id="b65f3-210">Ссылка CSS замены:</span><span class="sxs-lookup"><span data-stu-id="b65f3-210">The replacement CSS link:</span></span>

```html
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
```

<span data-ttu-id="b65f3-211">Теги сценариев замены:</span><span class="sxs-lookup"><span data-stu-id="b65f3-211">The replacement script tags:</span></span>

```html
<script src="~/lib/jquery/dist/jquery.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
```

<span data-ttu-id="b65f3-212">Обновленный *_Layout.cshtml* файла показан ниже:</span><span class="sxs-lookup"><span data-stu-id="b65f3-212">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-html[Main](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7,27,39-40)]

<span data-ttu-id="b65f3-213">Просмотр сайта в браузере.</span><span class="sxs-lookup"><span data-stu-id="b65f3-213">View the site in the browser.</span></span> <span data-ttu-id="b65f3-214">Должны теперь загрузиться правильно, с ожидаемой стили на месте.</span><span class="sxs-lookup"><span data-stu-id="b65f3-214">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="b65f3-215">*Необязательно:* может потребоваться попробуйте использовать новый файл макета.</span><span class="sxs-lookup"><span data-stu-id="b65f3-215">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="b65f3-216">Для этого проекта можно скопировать файл из макета *FullAspNetCore* проекта.</span><span class="sxs-lookup"><span data-stu-id="b65f3-216">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="b65f3-217">В новом файле макета используется [вспомогательных функций тегов](../mvc/views/tag-helpers/index.md) и других усовершенствований.</span><span class="sxs-lookup"><span data-stu-id="b65f3-217">The new layout file uses [Tag Helpers](../mvc/views/tag-helpers/index.md) and has other improvements.</span></span>

## <a name="configure-bundling--minification"></a><span data-ttu-id="b65f3-218">Настройка объединения & Минификации</span><span class="sxs-lookup"><span data-stu-id="b65f3-218">Configure Bundling & Minification</span></span>

<span data-ttu-id="b65f3-219">Сведения о настройке объединение и Минификация см. в разделе [объединении и Минификация](../client-side/bundling-and-minification.md).</span><span class="sxs-lookup"><span data-stu-id="b65f3-219">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solving-http-500-errors"></a><span data-ttu-id="b65f3-220">Ошибки HTTP 500 решения</span><span class="sxs-lookup"><span data-stu-id="b65f3-220">Solving HTTP 500 errors</span></span>

<span data-ttu-id="b65f3-221">Существуют многие проблемы, которые могут вызвать сообщение об ошибке HTTP 500, не содержат сведений о источник проблемы.</span><span class="sxs-lookup"><span data-stu-id="b65f3-221">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="b65f3-222">Например если *Views/_ViewImports.cshtml* файл содержит пространство имен, которые не существуют в проекте, вы получите ошибку HTTP 500.</span><span class="sxs-lookup"><span data-stu-id="b65f3-222">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="b65f3-223">Чтобы получить подробные сведения об ошибке, добавьте следующий код:</span><span class="sxs-lookup"><span data-stu-id="b65f3-223">To get a detailed error message, add the following code:</span></span>

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

<span data-ttu-id="b65f3-224">В разделе **с помощью страницы исключение разработчика** в [обработка ошибок](../fundamentals/error-handling.md) Дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="b65f3-224">See **Using the Developer Exception Page** in [Error Handling](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b65f3-225">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="b65f3-225">Additional Resources</span></span>

* [<span data-ttu-id="b65f3-226">Клиентская разработка</span><span class="sxs-lookup"><span data-stu-id="b65f3-226">Client-Side Development</span></span>](../client-side/index.md)

* [<span data-ttu-id="b65f3-227">Вспомогательные функции тегов</span><span class="sxs-lookup"><span data-stu-id="b65f3-227">Tag Helpers</span></span>](../mvc/views/tag-helpers/index.md)
