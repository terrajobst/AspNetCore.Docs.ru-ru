---
title: Управление пакетами стороне клиента с помощью Bower в ASP.NET Core
author: rick-anderson
description: Управление пакетами стороне клиента с помощью Bower.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/14/2017
uid: client-side/bower
ms.openlocfilehash: 23f3dcd06f012f3cf8d9509280b91c4bd1dc84e1
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272521"
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a><span data-ttu-id="95711-103">Управление пакетами стороне клиента с помощью Bower в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="95711-103">Manage client-side packages with Bower in ASP.NET Core</span></span>

<span data-ttu-id="95711-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Риса Ноэл](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), и [Скотт Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="95711-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), and [Scott Addie](https://scottaddie.com)</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="95711-105">Сохранение Bower, его программы обслуживания рекомендуется с помощью другого решения.</span><span class="sxs-lookup"><span data-stu-id="95711-105">While Bower is maintained, its maintainers recommend using a different solution.</span></span> <span data-ttu-id="95711-106">[Диспетчер библиотек](https://blogs.msdn.microsoft.com/webdev/2018/04/18/what-happened-to-bower/) (LibMan сокращенно) является новой системы управления содержимым статический клиентский Visual Studio (Visual Studio требуется выделить 15,8 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="95711-106">[Library Manager](https://blogs.msdn.microsoft.com/webdev/2018/04/18/what-happened-to-bower/) (LibMan for short) is Visual Studio's new client-side static content management system (Visual Studio 15.8 or later).</span></span> <span data-ttu-id="95711-107">Дополнительные сведения см. в разделе [диспетчер библиотек: клиентские Диспетчер содержимого для веб-приложений](https://blogs.msdn.microsoft.com/webdev/2018/04/17/library-manager-client-side-content-manager-for-web-apps/).</span><span class="sxs-lookup"><span data-stu-id="95711-107">For more information, see [Library Manager: Client-side content manager for web apps](https://blogs.msdn.microsoft.com/webdev/2018/04/17/library-manager-client-side-content-manager-for-web-apps/).</span></span> <span data-ttu-id="95711-108">В Visual Studio до версии 15,5 поддерживается bower.</span><span class="sxs-lookup"><span data-stu-id="95711-108">Bower is supported in Visual Studio through version 15.5.</span></span>
>
> <span data-ttu-id="95711-109">Yarn с Webpack — популярный альтернатива которой [инструкции по миграции](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) доступны.</span><span class="sxs-lookup"><span data-stu-id="95711-109">Yarn with Webpack is one popular alternative for which [migration instructions](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) are available.</span></span> 

<span data-ttu-id="95711-110">[Bower](https://bower.io/) вызывает саму себя «Диспетчер пакетов для Интернета».</span><span class="sxs-lookup"><span data-stu-id="95711-110">[Bower](https://bower.io/) calls itself "A package manager for the web".</span></span> <span data-ttu-id="95711-111">В экосистеме .NET он заполняет void влево на невозможность NuGet доставки статического содержимого файлов.</span><span class="sxs-lookup"><span data-stu-id="95711-111">Within the .NET ecosystem, it fills the void left by NuGet's inability to deliver static content files.</span></span> <span data-ttu-id="95711-112">Для проектов ASP.NET Core эти статические файлы, присущие клиентские библиотеки, такие как [jQuery](http://jquery.com/) и [начальной загрузки](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="95711-112">For ASP.NET Core projects, these static files are inherent to client-side libraries like [jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="95711-113">Для библиотеки .NET, по-прежнему использовать [NuGet](https://www.nuget.org/) диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="95711-113">For .NET libraries, you still use [NuGet](https://www.nuget.org/) package manager.</span></span>

<span data-ttu-id="95711-114">Процесс создания новых проектов, созданных с помощью шаблонов проектов ASP.NET Core, Настройка на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="95711-114">New projects created with the ASP.NET Core project templates set up the client-side build process.</span></span> <span data-ttu-id="95711-115">[jQuery](http://jquery.com/) и [начальной загрузки](http://getbootstrap.com/) установлены, и поддерживается Bower.</span><span class="sxs-lookup"><span data-stu-id="95711-115">[jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/) are installed, and Bower is supported.</span></span>

<span data-ttu-id="95711-116">Клиентские пакеты отображаются в *bower.json* файла.</span><span class="sxs-lookup"><span data-stu-id="95711-116">Client-side packages are listed in the *bower.json* file.</span></span> <span data-ttu-id="95711-117">Шаблоны проектов ASP.NET Core настраивает *bower.json* с jQuery, jQuery проверки и начальной загрузки.</span><span class="sxs-lookup"><span data-stu-id="95711-117">The ASP.NET Core project templates configures *bower.json* with jQuery, jQuery validation, and Bootstrap.</span></span>

<span data-ttu-id="95711-118">В этом учебнике мы добавили поддержку для [шрифта здорово](http://fontawesome.io).</span><span class="sxs-lookup"><span data-stu-id="95711-118">In this tutorial, we'll add support for [Font Awesome](http://fontawesome.io).</span></span> <span data-ttu-id="95711-119">Можно установить пакеты bower с **управление пакетами Bower** пользовательского интерфейса или вручную в *bower.json* файла.</span><span class="sxs-lookup"><span data-stu-id="95711-119">Bower packages can be installed with the **Manage Bower Packages** UI or manually in the *bower.json* file.</span></span>

### <a name="installation-via-manage-bower-packages-ui"></a><span data-ttu-id="95711-120">Установка с помощью Bower управление пакеты пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="95711-120">Installation via Manage Bower Packages UI</span></span>

* <span data-ttu-id="95711-121">Создание нового приложения ASP.NET Core Web с **веб-приложения ASP.NET Core (.NET Core)** шаблона.</span><span class="sxs-lookup"><span data-stu-id="95711-121">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="95711-122">Выберите **веб-приложение** и **без проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="95711-122">Select **Web Application** and **No Authentication**.</span></span>

* <span data-ttu-id="95711-123">Щелкните правой кнопкой мыши проект в обозревателе решений и выберите **управление пакетами Bower** (также в главном меню **проекта** > **управление пакетами Bower**).</span><span class="sxs-lookup"><span data-stu-id="95711-123">Right-click the project in Solution Explorer and select **Manage Bower Packages** (alternatively from the main menu, **Project** > **Manage Bower Packages**).</span></span>

* <span data-ttu-id="95711-124">В **Bower: \<имя проекта\>**  окно, перейдите на вкладку «Просмотр» и отфильтруйте список пакетов, введя `font-awesome` в поле поиска:</span><span class="sxs-lookup"><span data-stu-id="95711-124">In the **Bower: \<project name\>** window, click the "Browse" tab, and then filter the packages list by entering `font-awesome` in the search box:</span></span>

  ![Управление пакетами bower](bower/_static/manage-bower-packages.png)

* <span data-ttu-id="95711-126">Убедитесь, что «сохранить изменения в *bower.json*» установлен флажок.</span><span class="sxs-lookup"><span data-stu-id="95711-126">Confirm that the "Save changes to *bower.json*" checkbox is checked.</span></span> <span data-ttu-id="95711-127">Выберите версию в раскрывающемся списке и нажмите кнопку **установить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="95711-127">Select a version from the drop-down list and click the **Install** button.</span></span> <span data-ttu-id="95711-128">**Выходной** в окне отображаются сведения об установке.</span><span class="sxs-lookup"><span data-stu-id="95711-128">The **Output** window shows the installation details.</span></span>

### <a name="manual-installation-in-bowerjson"></a><span data-ttu-id="95711-129">Установка вручную в bower.json</span><span class="sxs-lookup"><span data-stu-id="95711-129">Manual installation in bower.json</span></span>

<span data-ttu-id="95711-130">Откройте *bower.json* и добавьте «шрифта здорово» зависимостям.</span><span class="sxs-lookup"><span data-stu-id="95711-130">Open the *bower.json* file and add "font-awesome" to the dependencies.</span></span> <span data-ttu-id="95711-131">IntelliSense отображает доступные пакеты.</span><span class="sxs-lookup"><span data-stu-id="95711-131">IntelliSense shows the available packages.</span></span> <span data-ttu-id="95711-132">При выборе пакета отображаются доступные версии.</span><span class="sxs-lookup"><span data-stu-id="95711-132">When a package is selected, the available versions are displayed.</span></span> <span data-ttu-id="95711-133">Изображения ниже являются более старыми и могут не соответствовать вы видите.</span><span class="sxs-lookup"><span data-stu-id="95711-133">The images below are older and won't match what you see.</span></span>

![IntelliSense bower пакета обозревателя](bower/_static/add-package.png)

![версия bower IntelliSense](bower/_static/version-intelliSense.png)

<span data-ttu-id="95711-136">Bower использует [семантического управления версиями](http://semver.org/) организовать зависимостей.</span><span class="sxs-lookup"><span data-stu-id="95711-136">Bower uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="95711-137">Семантического управления версиями, также известный как SemVer определяет пакеты с формирование \<основной >.\< дополнительный номер >. \<исправление >.</span><span class="sxs-lookup"><span data-stu-id="95711-137">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme \<major>.\<minor>.\<patch>.</span></span> <span data-ttu-id="95711-138">IntelliSense упрощает семантического управления версиями, отображая только несколько распространенных вариантов.</span><span class="sxs-lookup"><span data-stu-id="95711-138">IntelliSense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="95711-139">Верхний элемент в списке IntelliSense (4.6.3 в приведенном выше примере) считается последняя стабильная версия пакета.</span><span class="sxs-lookup"><span data-stu-id="95711-139">The top item in the IntelliSense list (4.6.3 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="95711-140">Знак вставки (^) соответствует самой последней основной номер версии, а тильда (~) соответствует последнему дополнительному номеру версии.</span><span class="sxs-lookup"><span data-stu-id="95711-140">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span>

<span data-ttu-id="95711-141">Сохранить *bower.json* файла.</span><span class="sxs-lookup"><span data-stu-id="95711-141">Save the *bower.json* file.</span></span> <span data-ttu-id="95711-142">Visual Studio отслеживает *bower.json* изменения в файле.</span><span class="sxs-lookup"><span data-stu-id="95711-142">Visual Studio watches the *bower.json* file for changes.</span></span> <span data-ttu-id="95711-143">При сохранении, *bower установки* выполняется команда.</span><span class="sxs-lookup"><span data-stu-id="95711-143">Upon saving, the *bower install* command is executed.</span></span> <span data-ttu-id="95711-144">См. в окне вывода **Bower или npm** представление для точного выполняемой команды.</span><span class="sxs-lookup"><span data-stu-id="95711-144">See the Output window's **Bower/npm** view for the exact command executed.</span></span>

<span data-ttu-id="95711-145">Откройте *.bowerrc* файл *bower.json*.</span><span class="sxs-lookup"><span data-stu-id="95711-145">Open the *.bowerrc* file under *bower.json*.</span></span> <span data-ttu-id="95711-146">`directory` Свойству *wwwroot/lib* указывающая расположение Bower установит пакет средств.</span><span class="sxs-lookup"><span data-stu-id="95711-146">The `directory` property is set to *wwwroot/lib* which indicates the location Bower will install the package assets.</span></span>

```json
{
 "directory": "wwwroot/lib"
}
```

<span data-ttu-id="95711-147">Поле поиска в обозревателе решений можно использовать для поиска и отображения шрифта здорово пакета.</span><span class="sxs-lookup"><span data-stu-id="95711-147">You can use the search box in Solution Explorer to find and display the font-awesome package.</span></span>

<span data-ttu-id="95711-148">Откройте *одну\_Layout.cshtml* и добавьте здорово шрифта CSS-файл в среде [вспомогательный тег](xref:mvc/views/tag-helpers/intro) для `Development`.</span><span class="sxs-lookup"><span data-stu-id="95711-148">Open the *Views\Shared\_Layout.cshtml* file and add the font-awesome CSS file to the environment [Tag Helper](xref:mvc/views/tag-helpers/intro) for `Development`.</span></span> <span data-ttu-id="95711-149">В обозревателе решений, перетаскивание *шрифта awesome.css* внутри `<environment names="Development">` элемента.</span><span class="sxs-lookup"><span data-stu-id="95711-149">From Solution Explorer, drag and drop *font-awesome.css* inside the `<environment names="Development">` element.</span></span>

[!code-html[](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

<span data-ttu-id="95711-150">В рабочем приложении необходимо добавить *шрифта awesome.min.css* помощникам тега среды для `Staging,Production`.</span><span class="sxs-lookup"><span data-stu-id="95711-150">In a production app you would add *font-awesome.min.css* to the environment tag helper for `Staging,Production`.</span></span>

<span data-ttu-id="95711-151">Замените содержимое *Views\Home\About.cshtml* файл Razor с следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="95711-151">Replace the contents of the *Views\Home\About.cshtml* Razor file with the following markup:</span></span>

[!code-html[](bower/sample/About.cshtml)]

<span data-ttu-id="95711-152">Запустите приложение и перейдите в представление о программе порядок проверки работы пакета здорово шрифта.</span><span class="sxs-lookup"><span data-stu-id="95711-152">Run the app and navigate to the About view to verify the font-awesome package works.</span></span>

## <a name="exploring-the-client-side-build-process"></a><span data-ttu-id="95711-153">Обзор процесса сборки на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="95711-153">Exploring the client-side build process</span></span>

<span data-ttu-id="95711-154">Большинство шаблонов проектов ASP.NET Core уже настроены для использования Bower.</span><span class="sxs-lookup"><span data-stu-id="95711-154">Most ASP.NET Core project templates are already configured to use Bower.</span></span> <span data-ttu-id="95711-155">Это Далее Пошаговое руководство начинается с пустой проект ASP.NET Core и добавляет каждый фрагмент вручную, то можно понять, как Bower используется в проекте.</span><span class="sxs-lookup"><span data-stu-id="95711-155">This next walkthrough starts with an empty ASP.NET Core project and adds each piece manually, so you can get a feel for how Bower is used in a project.</span></span> <span data-ttu-id="95711-156">Вы увидите, что происходит в структуре проекта и среду выполнения, в выходные данные, что для каждого изменения конфигурации.</span><span class="sxs-lookup"><span data-stu-id="95711-156">You can see what happens to the project structure and the runtime output as each configuration change is made.</span></span>

<span data-ttu-id="95711-157">Указаны общие шаги для использования процесса построения на стороне клиента с помощью Bower.</span><span class="sxs-lookup"><span data-stu-id="95711-157">The general steps to use the client-side build process with Bower are:</span></span>

* <span data-ttu-id="95711-158">Определите пакеты, используемой в проекте.</span><span class="sxs-lookup"><span data-stu-id="95711-158">Define packages used in your project.</span></span> <!-- once defined, you don't need to download them, VS does -->
* <span data-ttu-id="95711-159">Справочник по пакетов из веб-страниц.</span><span class="sxs-lookup"><span data-stu-id="95711-159">Reference packages from your web pages.</span></span>

### <a name="define-packages"></a><span data-ttu-id="95711-160">Определение пакетов</span><span class="sxs-lookup"><span data-stu-id="95711-160">Define packages</span></span>

<span data-ttu-id="95711-161">После списка пакетов в *bower.json* их загружается файл, Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="95711-161">Once you list packages in the *bower.json* file, Visual Studio will download them.</span></span> <span data-ttu-id="95711-162">В следующем примере используется Bower для загрузки jQuery и начальной загрузки для *wwwroot* папки.</span><span class="sxs-lookup"><span data-stu-id="95711-162">The following example uses Bower to load jQuery and Bootstrap to the *wwwroot* folder.</span></span>

* <span data-ttu-id="95711-163">Создание нового приложения ASP.NET Core Web с **веб-приложения ASP.NET Core (.NET Core)** шаблона.</span><span class="sxs-lookup"><span data-stu-id="95711-163">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="95711-164">Выберите **пустой** шаблон проекта и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="95711-164">Select the **Empty** project template and click **OK**.</span></span>

* <span data-ttu-id="95711-165">В обозревателе решений щелкните правой кнопкой мыши проект > **Добавление нового элемента** и выберите **файла конфигурации для Bower**.</span><span class="sxs-lookup"><span data-stu-id="95711-165">In Solution Explorer, right-click the project > **Add New Item** and select **Bower Configuration File**.</span></span> <span data-ttu-id="95711-166">Примечание: A *.bowerrc* также будет добавлен файл.</span><span class="sxs-lookup"><span data-stu-id="95711-166">Note: A *.bowerrc* file is also added.</span></span>

* <span data-ttu-id="95711-167">Откройте *bower.json*, добавление jquery и начальной загрузки для `dependencies` раздела.</span><span class="sxs-lookup"><span data-stu-id="95711-167">Open *bower.json*, and add jquery and bootstrap to the `dependencies` section.</span></span> <span data-ttu-id="95711-168">Итоговый *bower.json* файла будет выглядеть как в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="95711-168">The resulting *bower.json* file will look like the following example.</span></span> <span data-ttu-id="95711-169">Версии будет меняться со временем и может не совпадать на рисунке ниже.</span><span class="sxs-lookup"><span data-stu-id="95711-169">The versions will change over time and may not match the image below.</span></span>

[!code-json[](bower/sample/bower.json?highlight=5,6)]

* <span data-ttu-id="95711-170">Сохранить *bower.json* файла.</span><span class="sxs-lookup"><span data-stu-id="95711-170">Save the *bower.json* file.</span></span>

  <span data-ttu-id="95711-171">Проверить проект включает *начальной загрузки* и *jQuery* каталогов в *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="95711-171">Verify the project includes the *bootstrap* and *jQuery* directories in *wwwroot/lib*.</span></span> <span data-ttu-id="95711-172">Bower использует *.bowerrc* файл для установки средств в *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="95711-172">Bower uses the *.bowerrc* file to install the assets in *wwwroot/lib*.</span></span>

  <span data-ttu-id="95711-173">Примечание: «Управление пакетами Bower» пользовательского интерфейса является альтернативой редактирование файлов вручную.</span><span class="sxs-lookup"><span data-stu-id="95711-173">Note: The "Manage Bower Packages" UI provides an alternative to manual file editing.</span></span>

### <a name="enable-static-files"></a><span data-ttu-id="95711-174">Включение статических файлов</span><span class="sxs-lookup"><span data-stu-id="95711-174">Enable static files</span></span>

* <span data-ttu-id="95711-175">Добавить `Microsoft.AspNetCore.StaticFiles` пакет NuGet для проекта.</span><span class="sxs-lookup"><span data-stu-id="95711-175">Add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project.</span></span>
* <span data-ttu-id="95711-176">Включение статических файлов предоставляется с [по промежуточного слоя статических файлов](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions).</span><span class="sxs-lookup"><span data-stu-id="95711-176">Enable static files to be served with the [Static file middleware](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions).</span></span> <span data-ttu-id="95711-177">Добавьте вызов [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions) для `Configure` метод `Startup`.</span><span class="sxs-lookup"><span data-stu-id="95711-177">Add a call to [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions) to the `Configure` method of `Startup`.</span></span>

[!code-csharp[](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a><span data-ttu-id="95711-178">Справочник по пакетам</span><span class="sxs-lookup"><span data-stu-id="95711-178">Reference packages</span></span>

<span data-ttu-id="95711-179">В этом разделе вы создадите на HTML-странице, чтобы убедиться, что он может обращаться к развернутых пакетов.</span><span class="sxs-lookup"><span data-stu-id="95711-179">In this section, you will create an HTML page to verify it can access the deployed packages.</span></span>

* <span data-ttu-id="95711-180">Добавьте новую HTML-страницу с именем *Index.html* для *wwwroot* папки.</span><span class="sxs-lookup"><span data-stu-id="95711-180">Add a new HTML page named *Index.html* to the *wwwroot* folder.</span></span> <span data-ttu-id="95711-181">Примечание: Необходимо добавить HTML-файла для *wwwroot* папки.</span><span class="sxs-lookup"><span data-stu-id="95711-181">Note: You must add the HTML file to the *wwwroot* folder.</span></span> <span data-ttu-id="95711-182">По умолчанию статического содержимого не может быть выдана за пределами *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="95711-182">By default, static content cannot be served outside *wwwroot*.</span></span> <span data-ttu-id="95711-183">В разделе [статические файлы](xref:fundamentals/static-files) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="95711-183">See [Static files](xref:fundamentals/static-files) for more information.</span></span>

  <span data-ttu-id="95711-184">Замените содержимое *Index.html* следующей разметкой:</span><span class="sxs-lookup"><span data-stu-id="95711-184">Replace the contents of *Index.html* with the following markup:</span></span>

[!code-html[](bower/sample/Index.html)]

* <span data-ttu-id="95711-185">Запустите приложение и перейдите к `http://localhost:<port>/Index.html`.</span><span class="sxs-lookup"><span data-stu-id="95711-185">Run the app and navigate to `http://localhost:<port>/Index.html`.</span></span> <span data-ttu-id="95711-186">Кроме того, с *Index.html* открыт, нажмите клавишу `Ctrl+Shift+W`.</span><span class="sxs-lookup"><span data-stu-id="95711-186">Alternatively, with *Index.html* opened, press `Ctrl+Shift+W`.</span></span> <span data-ttu-id="95711-187">Проверка применения стилей jumbotron, код jQuery реагирует при нажатии кнопки и что начальной загрузки кнопка меняет состояние.</span><span class="sxs-lookup"><span data-stu-id="95711-187">Verify that the jumbotron styling is applied, the jQuery code responds when the button is clicked, and that the Bootstrap button changes state.</span></span>

  ![Стиль jumbotron](bower/_static/jumbotron.png)
