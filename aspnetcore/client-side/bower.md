---
title: Управление пакетами на стороне клиента с помощью Bower в ASP.NET Core
author: rick-anderson
description: Управление пакетами на стороне клиента с помощью Bower.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 08/09/2018
uid: client-side/bower
ms.openlocfilehash: 1292b75bdfe1cea56d9c8bc282b0dd90fc945a33
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67813530"
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a><span data-ttu-id="06be8-103">Управление пакетами на стороне клиента с помощью Bower в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="06be8-103">Manage client-side packages with Bower in ASP.NET Core</span></span>

<span data-ttu-id="06be8-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Райс Ноэл](https://twitter.com/noelrice1), и [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="06be8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel Rice](https://twitter.com/noelrice1), and [Scott Addie](https://scottaddie.com)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="06be8-105">Сохранение Bower, его издатели рекомендуем использовать другое решение.</span><span class="sxs-lookup"><span data-stu-id="06be8-105">While Bower is maintained, its maintainers recommend using a different solution.</span></span> <span data-ttu-id="06be8-106">[Диспетчер библиотек](https://blogs.msdn.microsoft.com/webdev/2018/04/18/what-happened-to-bower/) (LibMan для краткости) — средство приобретения новых клиентская библиотека для Visual Studio (Visual Studio требуется выделить 15,8 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="06be8-106">[Library Manager](https://blogs.msdn.microsoft.com/webdev/2018/04/18/what-happened-to-bower/) (LibMan for short) is Visual Studio's new client-side library acquisition tool (Visual Studio 15.8 or later).</span></span> <span data-ttu-id="06be8-107">Дополнительные сведения см. в разделе <xref:client-side/libman/index>.</span><span class="sxs-lookup"><span data-stu-id="06be8-107">For more information, see <xref:client-side/libman/index>.</span></span> <span data-ttu-id="06be8-108">Bower поддерживается в Visual Studio с помощью версии 15.5.</span><span class="sxs-lookup"><span data-stu-id="06be8-108">Bower is supported in Visual Studio through version 15.5.</span></span>
>
> <span data-ttu-id="06be8-109">Yarn с Webpack является альтернативой популярных, для которого [инструкции по миграции](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) доступны.</span><span class="sxs-lookup"><span data-stu-id="06be8-109">Yarn with Webpack is one popular alternative for which [migration instructions](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) are available.</span></span>

<span data-ttu-id="06be8-110">[Bower](https://bower.io/) вызывает саму себя «Диспетчер пакетов для веб-».</span><span class="sxs-lookup"><span data-stu-id="06be8-110">[Bower](https://bower.io/) calls itself "A package manager for the web".</span></span> <span data-ttu-id="06be8-111">В экосистеме .NET он заполняет этот пробел влево на невозможность NuGet для доставки статического содержимого файлов.</span><span class="sxs-lookup"><span data-stu-id="06be8-111">Within the .NET ecosystem, it fills the void left by NuGet's inability to deliver static content files.</span></span> <span data-ttu-id="06be8-112">Для проектов ASP.NET Core, статическими файлами принадлежат клиентские библиотеки, такие как [jQuery](https://jquery.com/) и [Bootstrap](https://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="06be8-112">For ASP.NET Core projects, these static files are inherent to client-side libraries like [jQuery](https://jquery.com/) and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="06be8-113">Для библиотеки .NET, вы по-прежнему использовать [NuGet](https://www.nuget.org/) диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="06be8-113">For .NET libraries, you still use [NuGet](https://www.nuget.org/) package manager.</span></span>

<span data-ttu-id="06be8-114">Процесс сборки новые проекты, созданные с помощью шаблонов проекта ASP.NET Core, Настройка на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="06be8-114">New projects created with the ASP.NET Core project templates set up the client-side build process.</span></span> <span data-ttu-id="06be8-115">[jQuery](https://jquery.com/) и [Bootstrap](https://getbootstrap.com/) установлены, и поддерживается Bower.</span><span class="sxs-lookup"><span data-stu-id="06be8-115">[jQuery](https://jquery.com/) and [Bootstrap](https://getbootstrap.com/) are installed, and Bower is supported.</span></span>

<span data-ttu-id="06be8-116">Клиентские пакеты отображаются в *bower.json* файла.</span><span class="sxs-lookup"><span data-stu-id="06be8-116">Client-side packages are listed in the *bower.json* file.</span></span> <span data-ttu-id="06be8-117">Шаблоны проектов ASP.NET Core настраивает *bower.json* с jQuery, Bootstrap и проверки jQuery.</span><span class="sxs-lookup"><span data-stu-id="06be8-117">The ASP.NET Core project templates configures *bower.json* with jQuery, jQuery validation, and Bootstrap.</span></span>

<span data-ttu-id="06be8-118">В этом руководстве мы добавим поддержку [Font Awesome](http://fontawesome.io).</span><span class="sxs-lookup"><span data-stu-id="06be8-118">In this tutorial, we'll add support for [Font Awesome](http://fontawesome.io).</span></span> <span data-ttu-id="06be8-119">Можно установить пакеты bower с **управление пакетами Bower** пользовательского интерфейса или вручную в *bower.json* файла.</span><span class="sxs-lookup"><span data-stu-id="06be8-119">Bower packages can be installed with the **Manage Bower Packages** UI or manually in the *bower.json* file.</span></span>

### <a name="installation-via-manage-bower-packages-ui"></a><span data-ttu-id="06be8-120">Установка с помощью управление пакетами Bower пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="06be8-120">Installation via Manage Bower Packages UI</span></span>

* <span data-ttu-id="06be8-121">Создание нового приложения ASP.NET Core Web с **веб-приложение ASP.NET Core (.NET Core)** шаблона.</span><span class="sxs-lookup"><span data-stu-id="06be8-121">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="06be8-122">Выберите **веб-приложение** и **без проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="06be8-122">Select **Web Application** and **No Authentication**.</span></span>

* <span data-ttu-id="06be8-123">Щелкните правой кнопкой мыши проект в обозревателе решений и выберите **управление пакетами Bower** (также в главном меню **проекта** > **управление пакетами Bower**).</span><span class="sxs-lookup"><span data-stu-id="06be8-123">Right-click the project in Solution Explorer and select **Manage Bower Packages** (alternatively from the main menu, **Project** > **Manage Bower Packages**).</span></span>

* <span data-ttu-id="06be8-124">В **Bower: \<имя_проекта\>**  окно, перейдите на вкладку «Обзор» и отфильтруйте список пакетов, введя `font-awesome` в поле поиска:</span><span class="sxs-lookup"><span data-stu-id="06be8-124">In the **Bower: \<project name\>** window, click the "Browse" tab, and then filter the packages list by entering `font-awesome` in the search box:</span></span>

  ![Управление пакетами bower](bower/_static/manage-bower-packages.png)

* <span data-ttu-id="06be8-126">Убедитесь, что «сохранить изменения в *bower.json*"будет установлен флажок.</span><span class="sxs-lookup"><span data-stu-id="06be8-126">Confirm that the "Save changes to *bower.json*" check box is checked.</span></span> <span data-ttu-id="06be8-127">Выберите версию в раскрывающемся списке и нажмите кнопку **установить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="06be8-127">Select a version from the drop-down list and click the **Install** button.</span></span> <span data-ttu-id="06be8-128">**Вывода** окне отображаются сведения об установке.</span><span class="sxs-lookup"><span data-stu-id="06be8-128">The **Output** window shows the installation details.</span></span>

### <a name="manual-installation-in-bowerjson"></a><span data-ttu-id="06be8-129">Установка вручную в bower.json</span><span class="sxs-lookup"><span data-stu-id="06be8-129">Manual installation in bower.json</span></span>

<span data-ttu-id="06be8-130">Откройте *bower.json* и добавьте зависимости «font awesome».</span><span class="sxs-lookup"><span data-stu-id="06be8-130">Open the *bower.json* file and add "font-awesome" to the dependencies.</span></span> <span data-ttu-id="06be8-131">IntelliSense отображает доступные пакеты.</span><span class="sxs-lookup"><span data-stu-id="06be8-131">IntelliSense shows the available packages.</span></span> <span data-ttu-id="06be8-132">При выборе пакета отображаются доступные версии.</span><span class="sxs-lookup"><span data-stu-id="06be8-132">When a package is selected, the available versions are displayed.</span></span> <span data-ttu-id="06be8-133">Приведенные ниже изображения являются более старыми и не будет соответствовать результату.</span><span class="sxs-lookup"><span data-stu-id="06be8-133">The images below are older and won't match what you see.</span></span>

![IntelliSense из обозреватель пакетов bower](bower/_static/add-package.png)

![версия bower IntelliSense](bower/_static/version-intelliSense.png)

<span data-ttu-id="06be8-136">Bower использует [семантического управления версиями](https://semver.org/) для упорядочения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="06be8-136">Bower uses [semantic versioning](https://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="06be8-137">Семантическое управление версиями, также известный как SemVer, определяет пакеты с формирование \<основных >.\< дополнительный номер >. \<patch >.</span><span class="sxs-lookup"><span data-stu-id="06be8-137">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme \<major>.\<minor>.\<patch>.</span></span> <span data-ttu-id="06be8-138">IntelliSense упрощает семантического управления версиями, отображая только несколько распространенных вариантов действий.</span><span class="sxs-lookup"><span data-stu-id="06be8-138">IntelliSense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="06be8-139">Верхний элемент в списке IntelliSense (4.6.3 в приведенном выше примере) считается последняя стабильная версия пакета.</span><span class="sxs-lookup"><span data-stu-id="06be8-139">The top item in the IntelliSense list (4.6.3 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="06be8-140">Символ крышки (^) соответствует самой последней основной версии и тильда (~) соответствует самой последней дополнительный номер версии.</span><span class="sxs-lookup"><span data-stu-id="06be8-140">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span>

<span data-ttu-id="06be8-141">Сохранить *bower.json* файла.</span><span class="sxs-lookup"><span data-stu-id="06be8-141">Save the *bower.json* file.</span></span> <span data-ttu-id="06be8-142">Visual Studio отслеживает *bower.json* файл для изменения.</span><span class="sxs-lookup"><span data-stu-id="06be8-142">Visual Studio watches the *bower.json* file for changes.</span></span> <span data-ttu-id="06be8-143">При сохранении, *bower install* выполняется команда.</span><span class="sxs-lookup"><span data-stu-id="06be8-143">Upon saving, the *bower install* command is executed.</span></span> <span data-ttu-id="06be8-144">См. в окне вывода **Bower и npm** представление для точного выполняемой команды.</span><span class="sxs-lookup"><span data-stu-id="06be8-144">See the Output window's **Bower/npm** view for the exact command executed.</span></span>

<span data-ttu-id="06be8-145">Откройте *.bowerrc* файл *bower.json*.</span><span class="sxs-lookup"><span data-stu-id="06be8-145">Open the *.bowerrc* file under *bower.json*.</span></span> <span data-ttu-id="06be8-146">`directory` Свойству *wwwroot/lib* , указывающее расположение Bower установит пакет средств.</span><span class="sxs-lookup"><span data-stu-id="06be8-146">The `directory` property is set to *wwwroot/lib* which indicates the location Bower will install the package assets.</span></span>

```json
{
 "directory": "wwwroot/lib"
}
```

<span data-ttu-id="06be8-147">Поле поиска в обозревателе решений можно использовать для поиска и отображения font awesome пакета.</span><span class="sxs-lookup"><span data-stu-id="06be8-147">You can use the search box in Solution Explorer to find and display the font-awesome package.</span></span>

<span data-ttu-id="06be8-148">Откройте *Views\Shared\_Layout.cshtml* и добавьте font awesome CSS-файл в среде [вспомогательная функция тега](xref:mvc/views/tag-helpers/intro) для `Development`.</span><span class="sxs-lookup"><span data-stu-id="06be8-148">Open the *Views\Shared\_Layout.cshtml* file and add the font-awesome CSS file to the environment [Tag Helper](xref:mvc/views/tag-helpers/intro) for `Development`.</span></span> <span data-ttu-id="06be8-149">Из обозревателя решений перетащите *шрифта awesome.css* внутри `<environment names="Development">` элемент.</span><span class="sxs-lookup"><span data-stu-id="06be8-149">From Solution Explorer, drag and drop *font-awesome.css* inside the `<environment names="Development">` element.</span></span>

[!code-html[](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

<span data-ttu-id="06be8-150">В рабочем приложении необходимо добавить *шрифта awesome.min.css* чтобы вспомогательная функция тега среды для `Staging,Production`.</span><span class="sxs-lookup"><span data-stu-id="06be8-150">In a production app you would add *font-awesome.min.css* to the environment tag helper for `Staging,Production`.</span></span>

<span data-ttu-id="06be8-151">Замените содержимое файла *Views\Home\About.cshtml* файл Razor, используя следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="06be8-151">Replace the contents of the *Views\Home\About.cshtml* Razor file with the following markup:</span></span>

[!code-html[](bower/sample/About.cshtml)]

<span data-ttu-id="06be8-152">Запустите приложение и перейдите к представлению About, чтобы убедитесь, что работает font awesome пакета.</span><span class="sxs-lookup"><span data-stu-id="06be8-152">Run the app and navigate to the About view to verify the font-awesome package works.</span></span>

## <a name="exploring-the-client-side-build-process"></a><span data-ttu-id="06be8-153">Изучение процесса сборки на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="06be8-153">Exploring the client-side build process</span></span>

<span data-ttu-id="06be8-154">Большинство шаблоны проектов ASP.NET Core уже настроены на использование Bower.</span><span class="sxs-lookup"><span data-stu-id="06be8-154">Most ASP.NET Core project templates are already configured to use Bower.</span></span> <span data-ttu-id="06be8-155">Этот следующего пошагового руководства начинается с пустого проекта ASP.NET Core и добавляет каждый фрагмент вручную, поэтому вы можете почувствовать, как Bower используется в проекте.</span><span class="sxs-lookup"><span data-stu-id="06be8-155">This next walkthrough starts with an empty ASP.NET Core project and adds each piece manually, so you can get a feel for how Bower is used in a project.</span></span> <span data-ttu-id="06be8-156">Вы увидите, что происходит с структуре проекта и выходные данные каждого изменения конфигурации среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="06be8-156">You can see what happens to the project structure and the runtime output as each configuration change is made.</span></span>

<span data-ttu-id="06be8-157">Приведены общие шаги, используемые процесса сборки на стороне клиента с помощью Bower.</span><span class="sxs-lookup"><span data-stu-id="06be8-157">The general steps to use the client-side build process with Bower are:</span></span>

* <span data-ttu-id="06be8-158">Определение пакетов, используемых в проекте.</span><span class="sxs-lookup"><span data-stu-id="06be8-158">Define packages used in your project.</span></span> <!-- once defined, you don't need to download them, VS does -->
* <span data-ttu-id="06be8-159">Справочник по пакетов из веб-страниц.</span><span class="sxs-lookup"><span data-stu-id="06be8-159">Reference packages from your web pages.</span></span>

### <a name="define-packages"></a><span data-ttu-id="06be8-160">Определение пакетов</span><span class="sxs-lookup"><span data-stu-id="06be8-160">Define packages</span></span>

<span data-ttu-id="06be8-161">После перечисления пакетов в *bower.json* файла, Visual Studio будут загружать их.</span><span class="sxs-lookup"><span data-stu-id="06be8-161">Once you list packages in the *bower.json* file, Visual Studio will download them.</span></span> <span data-ttu-id="06be8-162">В следующем примере используется Bower для загрузки, jQuery и начальной загрузки для *wwwroot* папки.</span><span class="sxs-lookup"><span data-stu-id="06be8-162">The following example uses Bower to load jQuery and Bootstrap to the *wwwroot* folder.</span></span>

* <span data-ttu-id="06be8-163">Создание нового приложения ASP.NET Core Web с **веб-приложение ASP.NET Core (.NET Core)** шаблона.</span><span class="sxs-lookup"><span data-stu-id="06be8-163">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="06be8-164">Выберите **пустой** шаблон проекта и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="06be8-164">Select the **Empty** project template and click **OK**.</span></span>

* <span data-ttu-id="06be8-165">В обозревателе решений щелкните правой кнопкой мыши проект > **Добавление нового элемента** и выберите **файл конфигурации Bower**.</span><span class="sxs-lookup"><span data-stu-id="06be8-165">In Solution Explorer, right-click the project > **Add New Item** and select **Bower Configuration File**.</span></span> <span data-ttu-id="06be8-166">Примечание. Объект *.bowerrc* добавляется еще и файл.</span><span class="sxs-lookup"><span data-stu-id="06be8-166">Note: A *.bowerrc* file is also added.</span></span>

* <span data-ttu-id="06be8-167">Откройте *bower.json*и добавьте jquery и начальной загрузки для `dependencies` раздел.</span><span class="sxs-lookup"><span data-stu-id="06be8-167">Open *bower.json*, and add jquery and bootstrap to the `dependencies` section.</span></span> <span data-ttu-id="06be8-168">Полученный в результате *bower.json* файла будет выглядеть как в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="06be8-168">The resulting *bower.json* file will look like the following example.</span></span> <span data-ttu-id="06be8-169">Версии будут меняться со временем и могут не соответствовать на следующем рисунке.</span><span class="sxs-lookup"><span data-stu-id="06be8-169">The versions will change over time and may not match the image below.</span></span>

[!code-json[](bower/sample/bower.json?highlight=5,6)]

* <span data-ttu-id="06be8-170">Сохранить *bower.json* файла.</span><span class="sxs-lookup"><span data-stu-id="06be8-170">Save the *bower.json* file.</span></span>

  <span data-ttu-id="06be8-171">Проверить проект включает *начальной загрузки* и *jQuery* каталогов в *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="06be8-171">Verify the project includes the *bootstrap* and *jQuery* directories in *wwwroot/lib*.</span></span> <span data-ttu-id="06be8-172">Bower использует *.bowerrc* файл, чтобы установить для активов *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="06be8-172">Bower uses the *.bowerrc* file to install the assets in *wwwroot/lib*.</span></span>

  <span data-ttu-id="06be8-173">Примечание. Пользовательский Интерфейс «Управление пакетами Bower» представляет собой альтернативу редактирование файлов вручную.</span><span class="sxs-lookup"><span data-stu-id="06be8-173">Note: The "Manage Bower Packages" UI provides an alternative to manual file editing.</span></span>

### <a name="enable-static-files"></a><span data-ttu-id="06be8-174">Включение статических файлов</span><span class="sxs-lookup"><span data-stu-id="06be8-174">Enable static files</span></span>

* <span data-ttu-id="06be8-175">Добавление `Microsoft.AspNetCore.StaticFiles` пакет NuGet в проект.</span><span class="sxs-lookup"><span data-stu-id="06be8-175">Add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project.</span></span>
* <span data-ttu-id="06be8-176">Включить статические файлы обслуживаются с [по промежуточного слоя статических файлов](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions).</span><span class="sxs-lookup"><span data-stu-id="06be8-176">Enable static files to be served with the [Static file middleware](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions).</span></span> <span data-ttu-id="06be8-177">Добавьте вызов [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions) для `Configure` метод `Startup`.</span><span class="sxs-lookup"><span data-stu-id="06be8-177">Add a call to [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions) to the `Configure` method of `Startup`.</span></span>

[!code-csharp[](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a><span data-ttu-id="06be8-178">Справочник по пакетов</span><span class="sxs-lookup"><span data-stu-id="06be8-178">Reference packages</span></span>

<span data-ttu-id="06be8-179">В этом разделе вы создадите страницу HTML, чтобы убедиться, что он может обращаться к развернутых пакетов.</span><span class="sxs-lookup"><span data-stu-id="06be8-179">In this section, you will create an HTML page to verify it can access the deployed packages.</span></span>

* <span data-ttu-id="06be8-180">Добавьте новую страницу HTML с именем *Index.html* для *wwwroot* папки.</span><span class="sxs-lookup"><span data-stu-id="06be8-180">Add a new HTML page named *Index.html* to the *wwwroot* folder.</span></span> <span data-ttu-id="06be8-181">Примечание. Необходимо добавить HTML-файл для *wwwroot* папки.</span><span class="sxs-lookup"><span data-stu-id="06be8-181">Note: You must add the HTML file to the *wwwroot* folder.</span></span> <span data-ttu-id="06be8-182">По умолчанию невозможно обслужить статическое содержимое за пределами *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="06be8-182">By default, static content cannot be served outside *wwwroot*.</span></span> <span data-ttu-id="06be8-183">См. в разделе [статические файлы](xref:fundamentals/static-files) Дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="06be8-183">See [Static files](xref:fundamentals/static-files) for more information.</span></span>

  <span data-ttu-id="06be8-184">Замените содержимое файла *Index.html* следующей разметкой:</span><span class="sxs-lookup"><span data-stu-id="06be8-184">Replace the contents of *Index.html* with the following markup:</span></span>

[!code-html[](bower/sample/Index.html)]

* <span data-ttu-id="06be8-185">Запустите приложение и перейдите к `http://localhost:<port>/Index.html`.</span><span class="sxs-lookup"><span data-stu-id="06be8-185">Run the app and navigate to `http://localhost:<port>/Index.html`.</span></span> <span data-ttu-id="06be8-186">Кроме того, с помощью *Index.html* открыт, нажмите клавишу `Ctrl+Shift+W`.</span><span class="sxs-lookup"><span data-stu-id="06be8-186">Alternatively, with *Index.html* opened, press `Ctrl+Shift+W`.</span></span> <span data-ttu-id="06be8-187">Убедитесь, что применяется в стилях jumbotron, кодом jQuery реагирует при нажатии кнопки и что начальной загрузки кнопки изменяет состояние.</span><span class="sxs-lookup"><span data-stu-id="06be8-187">Verify that the jumbotron styling is applied, the jQuery code responds when the button is clicked, and that the Bootstrap button changes state.</span></span>

  ![jumbotron стиль.](bower/_static/jumbotron.png)
