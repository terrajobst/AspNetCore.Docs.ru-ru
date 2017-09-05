---
title: "С помощью Bower в ASP.NET Core"
author: rick-anderson
description: "Управление пакетами стороне клиента с помощью Bower."
keywords: ASP.NET Core, bower
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: df7c43da-280e-4df6-86cb-eecec8f12bfc
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/bower
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 409f7afba8dd7d03b7b9aa27d93ec9167252b965
ms.sourcegitcommit: 4e84d8bf5f404bb77f3d41665cf7e7374fc39142
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/05/2017
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a><span data-ttu-id="a0cd5-104">Управление пакетами стороне клиента с помощью Bower в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a0cd5-104">Manage client-side packages with Bower in ASP.NET Core</span></span>

<span data-ttu-id="a0cd5-105">По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Риса Ноэл](http://blog.falafel.com/author/noel-rice/), и [Скотт Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="a0cd5-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel Rice](http://blog.falafel.com/author/noel-rice/), and [Scott Addie](https://scottaddie.com)</span></span> 

<span data-ttu-id="a0cd5-106">[Bower](https://bower.io/) вызывает саму себя «Диспетчер пакетов для Интернета.»</span><span class="sxs-lookup"><span data-stu-id="a0cd5-106">[Bower](https://bower.io/) calls itself "A package manager for the web."</span></span> <span data-ttu-id="a0cd5-107">В экосистеме .NET он заполняет void влево на невозможность NuGet доставки статического содержимого файлов.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-107">Within the .NET ecosystem, it fills the void left by NuGet’s inability to deliver static content files.</span></span> <span data-ttu-id="a0cd5-108">Для проектов ASP.NET Core эти статические файлы, присущие клиентские библиотеки, такие как [jQuery](http://jquery.com/) и [начальной загрузки](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="a0cd5-108">For ASP.NET Core projects, these static files are inherent to client-side libraries like [jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="a0cd5-109">Для библиотеки .NET, по-прежнему использовать [NuGet](https://nuget.org/) диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-109">For .NET libraries, you still use [NuGet](https://nuget.org/) package manager.</span></span>

<span data-ttu-id="a0cd5-110">Процесс создания новых проектов, созданных с помощью шаблонов проектов ASP.NET Core, Настройка на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-110">New projects created with the ASP.NET Core project templates set up the client-side build process.</span></span> <span data-ttu-id="a0cd5-111">[jQuery](http://jquery.com/) и [начальной загрузки](http://getbootstrap.com/) установлены, и поддерживается Bower.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-111">[jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/) are installed, and Bower is supported.</span></span>

<span data-ttu-id="a0cd5-112">Клиентские пакеты отображаются в *bower.json* файла.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-112">Client-side packages are listed in the *bower.json* file.</span></span> <span data-ttu-id="a0cd5-113">Шаблоны проектов ASP.NET Core настраивает *bower.json* с jQuery, jQuery проверки и начальной загрузки.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-113">The ASP.NET Core project templates configures *bower.json* with jQuery, jQuery validation, and Bootstrap.</span></span>

<span data-ttu-id="a0cd5-114">В этом учебнике мы добавили поддержку для [шрифта здорово](http://fontawesome.io).</span><span class="sxs-lookup"><span data-stu-id="a0cd5-114">In this tutorial, we'll add support for [Font Awesome](http://fontawesome.io).</span></span> <span data-ttu-id="a0cd5-115">Можно установить пакеты bower с **управление пакетами Bower** пользовательского интерфейса или вручную в *bower.json* файла.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-115">Bower packages can be installed with the **Manage Bower Packages** UI or manually in the *bower.json* file.</span></span>

### <a name="installation-via-manage-bower-packages-ui"></a><span data-ttu-id="a0cd5-116">Установка с помощью Bower управление пакеты пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="a0cd5-116">Installation via Manage Bower Packages UI</span></span>

* <span data-ttu-id="a0cd5-117">Создание нового приложения ASP.NET Core Web с **веб-приложения ASP.NET Core (.NET Core)** шаблона.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-117">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="a0cd5-118">Выберите **веб-приложение** и **без проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-118">Select **Web Application** and **No Authentication**.</span></span>

* <span data-ttu-id="a0cd5-119">Щелкните правой кнопкой мыши проект в обозревателе решений и выберите **управление пакетами Bower** (также в главном меню **проекта** > **управление пакетами Bower**).</span><span class="sxs-lookup"><span data-stu-id="a0cd5-119">Right-click the project in Solution Explorer and select **Manage Bower Packages** (alternatively from the main menu, **Project** > **Manage Bower Packages**).</span></span>

* <span data-ttu-id="a0cd5-120">В **Bower: \<имя проекта\>**  окно, перейдите на вкладку «Просмотр» и отфильтруйте список пакетов, введя `font-awesome` в поле поиска:</span><span class="sxs-lookup"><span data-stu-id="a0cd5-120">In the **Bower: \<project name\>** window, click the "Browse" tab, and then filter the packages list by entering `font-awesome` in the search box:</span></span>

 ![Управление пакетами bower](bower/_static/manage-bower-packages.png)

* <span data-ttu-id="a0cd5-122">Убедитесь, что «сохранить изменения в *bower.json*» установлен флажок.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-122">Confirm that the "Save changes to *bower.json*" checkbox is checked.</span></span> <span data-ttu-id="a0cd5-123">Выберите версию в раскрывающемся списке и нажмите кнопку **установить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-123">Select a version from the drop-down list and click the **Install** button.</span></span> <span data-ttu-id="a0cd5-124">**Выходной** в окне отображаются сведения об установке.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-124">The **Output** window shows the installation details.</span></span>

### <a name="manual-installation-in-bowerjson"></a><span data-ttu-id="a0cd5-125">Установка вручную в bower.json</span><span class="sxs-lookup"><span data-stu-id="a0cd5-125">Manual installation in bower.json</span></span>

<span data-ttu-id="a0cd5-126">Откройте *bower.json* и добавьте «шрифта здорово» зависимостям.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-126">Open the *bower.json* file and add "font-awesome" to the dependencies.</span></span> <span data-ttu-id="a0cd5-127">IntelliSense отображает доступные пакеты.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-127">IntelliSense shows the available packages.</span></span> <span data-ttu-id="a0cd5-128">При выборе пакета отображаются доступные версии.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-128">When a package is selected, the available versions are displayed.</span></span> <span data-ttu-id="a0cd5-129">Изображения ниже являются более старыми и не будет соответствовать вы видите.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-129">The images below are older and will not match what you see.</span></span>

![IntelliSense bower пакета обозревателя](bower/_static/add-package.png)

![версия bower IntelliSense](bower/_static/version-intelliSense.png)

<span data-ttu-id="a0cd5-132">Bower использует [семантического управления версиями](http://semver.org/) организовать зависимостей.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-132">Bower uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="a0cd5-133">Семантического управления версиями, также известный как SemVer определяет пакеты с формирование \<основной >.\< дополнительный номер >. \<исправление >.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-133">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme \<major>.\<minor>.\<patch>.</span></span> <span data-ttu-id="a0cd5-134">IntelliSense упрощает семантического управления версиями, отображая только несколько распространенных вариантов.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-134">IntelliSense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="a0cd5-135">Верхний элемент в списке IntelliSense (4.6.3 в приведенном выше примере) считается последняя стабильная версия пакета.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-135">The top item in the IntelliSense list (4.6.3 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="a0cd5-136">Знак вставки (^) соответствует самой последней основной номер версии, а тильда (~) соответствует последнему дополнительному номеру версии.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-136">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span>

<span data-ttu-id="a0cd5-137">Сохранить *bower.json* файла.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-137">Save the *bower.json* file.</span></span> <span data-ttu-id="a0cd5-138">Visual Studio отслеживает *bower.json* изменения в файле.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-138">Visual Studio watches the *bower.json* file for changes.</span></span> <span data-ttu-id="a0cd5-139">При сохранении, *bower установки* выполняется команда.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-139">Upon saving, the *bower install* command is executed.</span></span> <span data-ttu-id="a0cd5-140">См. в окне вывода **Bower или npm** представление для точного выполняемой команды.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-140">See the Output window's **Bower/npm** view for the exact command executed.</span></span>

<span data-ttu-id="a0cd5-141">Откройте *.bowerrc* файл *bower.json*.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-141">Open the *.bowerrc* file under *bower.json*.</span></span> <span data-ttu-id="a0cd5-142">`directory` Свойству *wwwroot/lib* указывающая расположение Bower установит пакет средств.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-142">The `directory` property is set to *wwwroot/lib* which indicates the location Bower will install the package assets.</span></span>

```json
{
 "directory": "wwwroot/lib"
}
```

<span data-ttu-id="a0cd5-143">Поле поиска в обозревателе решений можно использовать для поиска и отображения шрифта здорово пакета.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-143">You can use the search box in Solution Explorer to find and display the font-awesome package.</span></span>

<span data-ttu-id="a0cd5-144">Откройте *одну\_Layout.cshtml* и добавьте здорово шрифта CSS-файл в среде [вспомогательный тег](xref:mvc/views/tag-helpers/intro) для `Development`.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-144">Open the *Views\Shared\_Layout.cshtml* file and add the font-awesome CSS file to the environment [Tag Helper](xref:mvc/views/tag-helpers/intro) for `Development`.</span></span> <span data-ttu-id="a0cd5-145">В обозревателе решений, перетаскивание *шрифта awesome.css* внутри `<environment names="Development">` элемента.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-145">From Solution Explorer, drag and drop *font-awesome.css* inside the `<environment names="Development">` element.</span></span>

<span data-ttu-id="a0cd5-146">[!code-html[Main](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]</span><span class="sxs-lookup"><span data-stu-id="a0cd5-146">[!code-html[Main](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]</span></span>

<span data-ttu-id="a0cd5-147">В рабочем приложении необходимо добавить *шрифта awesome.min.css* помощникам тега среды для `Staging,Production`.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-147">In a production app you would add *font-awesome.min.css* to the environment tag helper for `Staging,Production`.</span></span>

<span data-ttu-id="a0cd5-148">Замените содержимое *Views\Home\About.cshtml* файл Razor с следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="a0cd5-148">Replace the contents of the *Views\Home\About.cshtml* Razor file with the following markup:</span></span>

<span data-ttu-id="a0cd5-149">[!code-html[Main](bower/sample/About.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="a0cd5-149">[!code-html[Main](bower/sample/About.cshtml)]</span></span>

<span data-ttu-id="a0cd5-150">Запустите приложение и перейдите в представление о программе порядок проверки работы пакета здорово шрифта.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-150">Run the app and navigate to the About view to verify the font-awesome package works.</span></span>

## <a name="exploring-the-client-side-build-process"></a><span data-ttu-id="a0cd5-151">Обзор процесса сборки на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="a0cd5-151">Exploring the client-side build process</span></span>

<span data-ttu-id="a0cd5-152">Большинство шаблонов проектов ASP.NET Core уже настроены для использования Bower.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-152">Most ASP.NET Core project templates are already configured to use Bower.</span></span> <span data-ttu-id="a0cd5-153">Это Далее Пошаговое руководство начинается с пустой проект ASP.NET Core и добавляет каждый фрагмент вручную, то можно понять, как Bower используется в проекте.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-153">This next walkthrough starts with an empty ASP.NET Core project and adds each piece manually, so you can get a feel for how Bower is used in a project.</span></span> <span data-ttu-id="a0cd5-154">Вы видите, можно, что происходит в структуре проекта и выполняется вывод каждого изменения конфигурации среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-154">You see can what happens to the project structure and the runtime output as each configuration change is made.</span></span>

<span data-ttu-id="a0cd5-155">Указаны общие шаги для использования процесса построения на стороне клиента с помощью Bower.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-155">The general steps to use the client-side build process with Bower are:</span></span>

* <span data-ttu-id="a0cd5-156">Определите пакеты, используемой в проекте.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-156">Define packages used in your project.</span></span> <!-- once defined, you don't need to download them, VS does -->
* <span data-ttu-id="a0cd5-157">Справочник по пакетов из веб-страниц.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-157">Reference packages from your web pages.</span></span>

### <a name="define-packages"></a><span data-ttu-id="a0cd5-158">Определение пакетов</span><span class="sxs-lookup"><span data-stu-id="a0cd5-158">Define packages</span></span>

<span data-ttu-id="a0cd5-159">После списка пакетов в *bower.json* их загружается файл, Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-159">Once you list packages in the *bower.json* file, Visual Studio will download them.</span></span> <span data-ttu-id="a0cd5-160">В следующем примере используется Bower для загрузки jQuery и начальной загрузки для *wwwroot* папки.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-160">The following example uses Bower to load jQuery and Bootstrap to the *wwwroot* folder.</span></span>

* <span data-ttu-id="a0cd5-161">Создание нового приложения ASP.NET Core Web с **веб-приложения ASP.NET Core (.NET Core)** шаблона.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-161">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="a0cd5-162">Выберите **пустой** шаблон проекта и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-162">Select the **Empty** project template and click **OK**.</span></span>

* <span data-ttu-id="a0cd5-163">В обозревателе решений щелкните правой кнопкой мыши проект > **Добавление нового элемента** и выберите **файла конфигурации для Bower**.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-163">In Solution Explorer, right-click the project > **Add New Item** and select **Bower Configuration File**.</span></span> <span data-ttu-id="a0cd5-164">Примечание: A *.bowerrc* также будет добавлен файл.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-164">Note: A *.bowerrc* file is also added.</span></span>

* <span data-ttu-id="a0cd5-165">Откройте *bower.json*, добавление jquery и начальной загрузки для `dependencies` раздела.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-165">Open *bower.json*, and add jquery and bootstrap to the `dependencies` section.</span></span> <span data-ttu-id="a0cd5-166">Итоговый *bower.json* файла будет выглядеть как в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-166">The resulting *bower.json* file will look like the following example.</span></span> <span data-ttu-id="a0cd5-167">Версии будет меняться со временем и может не совпадать на рисунке ниже.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-167">The versions will change over time and may not match the image below.</span></span>

<span data-ttu-id="a0cd5-168">[!code-json[Main](bower/sample/bower.json?highlight=5,6)]</span><span class="sxs-lookup"><span data-stu-id="a0cd5-168">[!code-json[Main](bower/sample/bower.json?highlight=5,6)]</span></span>

* <span data-ttu-id="a0cd5-169">Сохранить *bower.json* файла.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-169">Save the *bower.json* file.</span></span>

 <span data-ttu-id="a0cd5-170">Проверить проект включает *начальной загрузки* и *jQuery* каталогов в *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-170">Verify the project includes the *bootstrap* and *jQuery* directories in *wwwroot/lib*.</span></span> <span data-ttu-id="a0cd5-171">Bower использует *.bowerrc* файл для установки средств в *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-171">Bower uses the *.bowerrc* file to install the assets in *wwwroot/lib*.</span></span>

 <span data-ttu-id="a0cd5-172">Примечание: «Управление пакетами Bower» пользовательского интерфейса является альтернативой редактирование файлов вручную.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-172">Note: The "Manage Bower Packages" UI provides an alternative to manual file editing.</span></span>

### <a name="enable-static-files"></a><span data-ttu-id="a0cd5-173">Включение статических файлов</span><span class="sxs-lookup"><span data-stu-id="a0cd5-173">Enable static files</span></span>

* <span data-ttu-id="a0cd5-174">Добавить `Microsoft.AspNetCore.StaticFiles` пакет NuGet для проекта.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-174">Add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project.</span></span>
* <span data-ttu-id="a0cd5-175">Включение статических файлов предоставляется с [по промежуточного слоя статических файлов](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions).</span><span class="sxs-lookup"><span data-stu-id="a0cd5-175">Enable static files to be served with the [Static file middleware](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions).</span></span> <span data-ttu-id="a0cd5-176">Добавьте вызов [UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions) для `Configure` метод `Startup`.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-176">Add a call to [UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions) to the `Configure` method of `Startup`.</span></span>

<span data-ttu-id="a0cd5-177">[!code-csharp[Main](bower/sample/Startup.cs?highlight=9)]</span><span class="sxs-lookup"><span data-stu-id="a0cd5-177">[!code-csharp[Main](bower/sample/Startup.cs?highlight=9)]</span></span>

### <a name="reference-packages"></a><span data-ttu-id="a0cd5-178">Справочник по пакетам</span><span class="sxs-lookup"><span data-stu-id="a0cd5-178">Reference packages</span></span>

<span data-ttu-id="a0cd5-179">В этом разделе вы создадите на HTML-странице, чтобы убедиться, что он может обращаться к развернутых пакетов.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-179">In this section, you will create an HTML page to verify it can access the deployed packages.</span></span>

* <span data-ttu-id="a0cd5-180">Добавьте новую HTML-страницу с именем *Index.html* для *wwwroot* папки.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-180">Add a new HTML page named *Index.html* to the *wwwroot* folder.</span></span> <span data-ttu-id="a0cd5-181">Примечание: Необходимо добавить HTML-файла для *wwwroot* папки.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-181">Note: You must add the HTML file to the *wwwroot* folder.</span></span> <span data-ttu-id="a0cd5-182">По умолчанию статического содержимого не может быть выдана за пределами *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-182">By default, static content cannot be served outside *wwwroot*.</span></span> <span data-ttu-id="a0cd5-183">В разделе [работа с файлами статического](xref:fundamentals/static-files) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-183">See [Working with static files](xref:fundamentals/static-files) for more information.</span></span>

 <span data-ttu-id="a0cd5-184">Замените содержимое *Index.html* следующей разметкой:</span><span class="sxs-lookup"><span data-stu-id="a0cd5-184">Replace the contents of *Index.html* with the following markup:</span></span>

<span data-ttu-id="a0cd5-185">[!code-html[Main](bower/sample/Index.html)]</span><span class="sxs-lookup"><span data-stu-id="a0cd5-185">[!code-html[Main](bower/sample/Index.html)]</span></span>

* <span data-ttu-id="a0cd5-186">Запустите приложение и перейдите к `http://localhost:<port>/Index.html`.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-186">Run the app and navigate to `http://localhost:<port>/Index.html`.</span></span> <span data-ttu-id="a0cd5-187">Кроме того, с *Index.html* открыт, нажмите клавишу `Ctrl+Shift+W`.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-187">Alternatively, with *Index.html* opened, press `Ctrl+Shift+W`.</span></span> <span data-ttu-id="a0cd5-188">Проверка применения стилей jumbotron, код jQuery реагирует при нажатии кнопки и что начальной загрузки кнопка меняет состояние.</span><span class="sxs-lookup"><span data-stu-id="a0cd5-188">Verify that the jumbotron styling is applied, the jQuery code responds when the button is clicked, and that the Bootstrap button changes state.</span></span>

 ![Стиль jumbotron](bower/_static/jumbotron.png)
