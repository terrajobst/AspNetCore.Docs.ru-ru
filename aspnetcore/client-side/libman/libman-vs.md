---
title: Использование LibMan с ASP.NET Core в Visual Studio
author: scottaddie
description: Сведения об использовании LibMan в проекте ASP.NET Core с помощью Visual Studio.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/20/2018
uid: client-side/libman/libman-vs
ms.openlocfilehash: b44769f1d0925f38523d6570858de17f37e32c2b
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/22/2018
ms.locfileid: "41909936"
---
# <a name="use-libman-with-aspnet-core-in-visual-studio"></a><span data-ttu-id="322bb-103">Использование LibMan с ASP.NET Core в Visual Studio</span><span class="sxs-lookup"><span data-stu-id="322bb-103">Use LibMan with ASP.NET Core in Visual Studio</span></span>

<span data-ttu-id="322bb-104">Автор: [Скотт Адди](https://twitter.com/Scott_Addie) (Scott Addie)</span><span class="sxs-lookup"><span data-stu-id="322bb-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="322bb-105">Visual Studio имеет встроенную поддержку [LibMan](xref:client-side/libman/index) в проектах ASP.NET Core, включая:</span><span class="sxs-lookup"><span data-stu-id="322bb-105">Visual Studio has built-in support for [LibMan](xref:client-side/libman/index) in ASP.NET Core projects, including:</span></span>

* <span data-ttu-id="322bb-106">Поддержка по настройке и выполнению операции восстановления LibMan в сборки.</span><span class="sxs-lookup"><span data-stu-id="322bb-106">Support for configuring and running LibMan restore operations on build.</span></span>
* <span data-ttu-id="322bb-107">Пункты меню для активации LibMan восстановление и очистка операций.</span><span class="sxs-lookup"><span data-stu-id="322bb-107">Menu items for triggering LibMan restore and clean operations.</span></span>
* <span data-ttu-id="322bb-108">Диалоговое окно поиска для поиска библиотек и добавления файлов в проект.</span><span class="sxs-lookup"><span data-stu-id="322bb-108">Search dialog for finding libraries and adding the files to a project.</span></span>
* <span data-ttu-id="322bb-109">Для редактирования *libman.json*&mdash;LibMan файл манифеста.</span><span class="sxs-lookup"><span data-stu-id="322bb-109">Editing support for *libman.json*&mdash;the LibMan manifest file.</span></span>

<span data-ttu-id="322bb-110">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/libman/samples/) [(способ загрузки)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="322bb-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/libman/samples/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="322bb-111">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="322bb-111">Prerequisites</span></span>

* <span data-ttu-id="322bb-112">Visual Studio 2017 версии 15,8 или более поздней версии с **ASP.NET и веб-разработка** рабочей нагрузки</span><span class="sxs-lookup"><span data-stu-id="322bb-112">Visual Studio 2017 version 15.8 or later with the **ASP.NET and web development** workload</span></span>

## <a name="add-library-files"></a><span data-ttu-id="322bb-113">Добавьте файлы библиотеки</span><span class="sxs-lookup"><span data-stu-id="322bb-113">Add library files</span></span>

<span data-ttu-id="322bb-114">Файлы библиотеки могут добавляться в проект ASP.NET Core двумя разными способами:</span><span class="sxs-lookup"><span data-stu-id="322bb-114">Library files can be added to an ASP.NET Core project in two different ways:</span></span>

1. [<span data-ttu-id="322bb-115">Используйте диалоговое окно Добавление библиотеки на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="322bb-115">Use the Add Client-Side Library dialog</span></span>](#use-the-add-client-side-library-dialog)
1. [<span data-ttu-id="322bb-116">Вручную настроить LibMan записи файла манифеста</span><span class="sxs-lookup"><span data-stu-id="322bb-116">Manually configure LibMan manifest file entries</span></span>](#manually-configure-libman-manifest-file-entries)

### <a name="use-the-add-client-side-library-dialog"></a><span data-ttu-id="322bb-117">Используйте диалоговое окно Добавление библиотеки на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="322bb-117">Use the Add Client-Side Library dialog</span></span>

<span data-ttu-id="322bb-118">Выполните следующие действия, чтобы установить клиентскую библиотеку.</span><span class="sxs-lookup"><span data-stu-id="322bb-118">Follow these steps to install a client-side library:</span></span>

* <span data-ttu-id="322bb-119">В **обозревателе решений**, щелкните правой кнопкой мыши папку проекта, в которой должны добавляться файлы.</span><span class="sxs-lookup"><span data-stu-id="322bb-119">In **Solution Explorer**, right-click the project folder in which the files should be added.</span></span> <span data-ttu-id="322bb-120">Выберите **добавить** > **клиентская библиотека**.</span><span class="sxs-lookup"><span data-stu-id="322bb-120">Choose **Add** > **Client-Side Library**.</span></span> <span data-ttu-id="322bb-121">**Добавьте клиентскую библиотеку** откроется диалоговое окно:</span><span class="sxs-lookup"><span data-stu-id="322bb-121">The **Add Client-Side Library** dialog appears:</span></span>

  ![Клиентская библиотека диалоговое окно добавления](_static/add-library-dialog.png)

* <span data-ttu-id="322bb-123">Выберите поставщика библиотеки из **поставщика** раскрывающийся список.</span><span class="sxs-lookup"><span data-stu-id="322bb-123">Select the library provider from the **Provider** drop down.</span></span> <span data-ttu-id="322bb-124">CDNJS является поставщиком по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="322bb-124">CDNJS is the default provider.</span></span>
* <span data-ttu-id="322bb-125">Введите имя библиотеки, необходимо получить в **библиотеки** текстовое поле.</span><span class="sxs-lookup"><span data-stu-id="322bb-125">Type the library name to fetch in the **Library** text box.</span></span> <span data-ttu-id="322bb-126">Технология IntelliSense предоставляет список библиотек, начиная с заданного текста.</span><span class="sxs-lookup"><span data-stu-id="322bb-126">IntelliSense provides a list of libraries beginning with the provided text.</span></span>
* <span data-ttu-id="322bb-127">В списке IntelliSense, выберите библиотеку.</span><span class="sxs-lookup"><span data-stu-id="322bb-127">Select the library from the IntelliSense list.</span></span> <span data-ttu-id="322bb-128">Обратите внимание, что имеет суффикс имени библиотеки `@` символов и последняя стабильная версия известные с выбранным поставщиком.</span><span class="sxs-lookup"><span data-stu-id="322bb-128">Notice the library name is suffixed with the `@` symbol and the latest stable version known to the selected provider.</span></span>
* <span data-ttu-id="322bb-129">Решите, какие файлы следует включать:</span><span class="sxs-lookup"><span data-stu-id="322bb-129">Decide which files to include:</span></span>
  * <span data-ttu-id="322bb-130">Выберите **Включение всех файлов библиотеки** переключатель, чтобы включить все файлы библиотеки.</span><span class="sxs-lookup"><span data-stu-id="322bb-130">Select the **Include all library files** radio button to include all of the library's files.</span></span>
  * <span data-ttu-id="322bb-131">Выберите **выбрать определенные файлы** переключатель, чтобы включить подмножество файлов библиотеки.</span><span class="sxs-lookup"><span data-stu-id="322bb-131">Select the **Choose specific files** radio button to include a subset of the library's files.</span></span> <span data-ttu-id="322bb-132">Если выбран переключатель, дерево селектор файлов включен.</span><span class="sxs-lookup"><span data-stu-id="322bb-132">When the radio button is selected, the file selector tree is enabled.</span></span> <span data-ttu-id="322bb-133">Установите флажки слева от имен файлов для загрузки.</span><span class="sxs-lookup"><span data-stu-id="322bb-133">Check the boxes to the left of the file names to download.</span></span>
* <span data-ttu-id="322bb-134">Укажите папку для хранения файлов в проект **целевое расположение** текстовое поле.</span><span class="sxs-lookup"><span data-stu-id="322bb-134">Specify the project folder for storing the files in the **Target Location** text box.</span></span> <span data-ttu-id="322bb-135">Как рекомендации храните каждую библиотеку в отдельной папке.</span><span class="sxs-lookup"><span data-stu-id="322bb-135">As a recommendation, store each library in a separate folder.</span></span>

  <span data-ttu-id="322bb-136">Предложенные **целевое расположение** папки зависит от расположения, из которого запускается диалогового окна:</span><span class="sxs-lookup"><span data-stu-id="322bb-136">The suggested **Target Location** folder is based on the location from which the dialog launched:</span></span>

  * <span data-ttu-id="322bb-137">Если запускается из корневой папки проекта:</span><span class="sxs-lookup"><span data-stu-id="322bb-137">If launched from the project root:</span></span>
    * <span data-ttu-id="322bb-138">*wwwroot/lib* используется, если *wwwroot* существует.</span><span class="sxs-lookup"><span data-stu-id="322bb-138">*wwwroot/lib* is used if *wwwroot* exists.</span></span>
    * <span data-ttu-id="322bb-139">*LIB* используется, если *wwwroot* не существует.</span><span class="sxs-lookup"><span data-stu-id="322bb-139">*lib* is used if *wwwroot* doesn't exist.</span></span>
  * <span data-ttu-id="322bb-140">Если запускается из папки проекта используется имя соответствующей папки.</span><span class="sxs-lookup"><span data-stu-id="322bb-140">If launched from a project folder, the corresponding folder name is used.</span></span>

  <span data-ttu-id="322bb-141">Предложение папка имеет суффикс имени библиотеки.</span><span class="sxs-lookup"><span data-stu-id="322bb-141">The folder suggestion is suffixed with the library name.</span></span> <span data-ttu-id="322bb-142">В следующей таблице показаны варианты папку при установке jQuery в проекта Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="322bb-142">The following table illustrates folder suggestions when installing jQuery in a Razor Pages project.</span></span>
  
  |<span data-ttu-id="322bb-143">Место запуска</span><span class="sxs-lookup"><span data-stu-id="322bb-143">Launch location</span></span>                           |<span data-ttu-id="322bb-144">Предлагаемые папки</span><span class="sxs-lookup"><span data-stu-id="322bb-144">Suggested folder</span></span>      |
  |------------------------------------------|----------------------|
  |<span data-ttu-id="322bb-145">корневой каталог проекта (если *wwwroot* существует)</span><span class="sxs-lookup"><span data-stu-id="322bb-145">project root (if *wwwroot* exists)</span></span>        |<span data-ttu-id="322bb-146">*wwwroot/lib/jquery /*</span><span class="sxs-lookup"><span data-stu-id="322bb-146">*wwwroot/lib/jquery/*</span></span> |
  |<span data-ttu-id="322bb-147">корневой каталог проекта (если *wwwroot* не существует)</span><span class="sxs-lookup"><span data-stu-id="322bb-147">project root (if *wwwroot* doesn't exist)</span></span> |<span data-ttu-id="322bb-148">*LIB/jquery /*</span><span class="sxs-lookup"><span data-stu-id="322bb-148">*lib/jquery/*</span></span>         |
  |<span data-ttu-id="322bb-149">*Страницы* папку в проекте</span><span class="sxs-lookup"><span data-stu-id="322bb-149">*Pages* folder in project</span></span>                 |<span data-ttu-id="322bb-150">*Страниц/jquery /*</span><span class="sxs-lookup"><span data-stu-id="322bb-150">*Pages/jquery/*</span></span>       |

* <span data-ttu-id="322bb-151">Нажмите кнопку **установить** кнопку, чтобы загрузить файлы, каждой конфигурации в *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="322bb-151">Click the **Install** button to download the files, per the configuration in *libman.json*.</span></span>
* <span data-ttu-id="322bb-152">Просмотрите **диспетчер библиотек** веб-канала из **вывода** окно для сведения об установке.</span><span class="sxs-lookup"><span data-stu-id="322bb-152">Review the **Library Manager** feed of the **Output** window for installation details.</span></span> <span data-ttu-id="322bb-153">Пример:</span><span class="sxs-lookup"><span data-stu-id="322bb-153">For example:</span></span>

  ```console
  Restore operation started...
  Restoring libraries for project LibManSample
  Restoring library jquery@3.3.1... (LibManSample)
  wwwroot/lib/jquery/jquery.min.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.min.map written to destination (LibManSample)
  Restore operation completed
  1 libraries restored in 2.32 seconds
  ```

### <a name="manually-configure-libman-manifest-file-entries"></a><span data-ttu-id="322bb-154">Вручную настроить LibMan записи файла манифеста</span><span class="sxs-lookup"><span data-stu-id="322bb-154">Manually configure LibMan manifest file entries</span></span>

<span data-ttu-id="322bb-155">Все операции LibMan в Visual Studio на основе содержимого манифеста LibMan корневой каталог проекта (*libman.json*).</span><span class="sxs-lookup"><span data-stu-id="322bb-155">All LibMan operations in Visual Studio are based on the content of the project root's LibMan manifest (*libman.json*).</span></span> <span data-ttu-id="322bb-156">Можно вручную изменить *libman.json* настроить файлы библиотек проекта.</span><span class="sxs-lookup"><span data-stu-id="322bb-156">You can manually edit *libman.json* to configure library files for the project.</span></span> <span data-ttu-id="322bb-157">Visual Studio восстанавливает все файлы библиотек, один раз *libman.json* сохраняется.</span><span class="sxs-lookup"><span data-stu-id="322bb-157">Visual Studio restores all library files once *libman.json* is saved.</span></span>

<span data-ttu-id="322bb-158">Чтобы открыть *libman.json* для редактирования, имеются следующие варианты:</span><span class="sxs-lookup"><span data-stu-id="322bb-158">To open *libman.json* for editing, the following options exist:</span></span>

* <span data-ttu-id="322bb-159">Дважды щелкните *libman.json* файл **обозревателе решений**.</span><span class="sxs-lookup"><span data-stu-id="322bb-159">Double-click the *libman.json* file in **Solution Explorer**.</span></span>
* <span data-ttu-id="322bb-160">Щелкните правой кнопкой мыши проект в **обозревателе решений** и выберите **управление клиентские библиотеки**.</span><span class="sxs-lookup"><span data-stu-id="322bb-160">Right-click the project in **Solution Explorer** and select **Manage Client-Side Libraries**.</span></span> <span data-ttu-id="322bb-161">**&#8224;**</span><span class="sxs-lookup"><span data-stu-id="322bb-161">**&#8224;**</span></span>
* <span data-ttu-id="322bb-162">Выберите **управление клиентские библиотеки** из Visual Studio **проекта** меню.</span><span class="sxs-lookup"><span data-stu-id="322bb-162">Select **Manage Client-Side Libraries** from the Visual Studio **Project** menu.</span></span> <span data-ttu-id="322bb-163">**&#8224;**</span><span class="sxs-lookup"><span data-stu-id="322bb-163">**&#8224;**</span></span>

<span data-ttu-id="322bb-164">**&#8224;** Если *libman.json* файл в корневую папку проекта еще не существует, он будет создан с содержимым шаблонов элемента по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="322bb-164">**&#8224;** If the *libman.json* file doesn't already exist in the project root, it will be created with the default item template content.</span></span>

<span data-ttu-id="322bb-165">Visual Studio предлагает широкие возможности JSON, поддержка, такие как Раскраска редактирования, форматирование, IntelliSense и проверки схемы.</span><span class="sxs-lookup"><span data-stu-id="322bb-165">Visual Studio offers rich JSON editing support such as colorization, formatting, IntelliSense, and schema validation.</span></span> <span data-ttu-id="322bb-166">Схема JSON манифесте LibMan находится по адресу [ http://json.schemastore.org/libman ](http://json.schemastore.org/libman).</span><span class="sxs-lookup"><span data-stu-id="322bb-166">The LibMan manifest's JSON schema is found at [http://json.schemastore.org/libman](http://json.schemastore.org/libman).</span></span>

<span data-ttu-id="322bb-167">В следующем файле манифеста, LibMan извлекает файлы в конфигурацию, определенную в `libraries` свойство.</span><span class="sxs-lookup"><span data-stu-id="322bb-167">With the following manifest file, LibMan retrieves files per the configuration defined in the `libraries` property.</span></span> <span data-ttu-id="322bb-168">Объяснение объектных литералов, определенные в `libraries` ниже:</span><span class="sxs-lookup"><span data-stu-id="322bb-168">An explanation of the object literals defined within `libraries` follows:</span></span>

* <span data-ttu-id="322bb-169">Подмножество [jQuery](https://jquery.com/) версии 3.3.1 извлекается из CDNJS поставщика.</span><span class="sxs-lookup"><span data-stu-id="322bb-169">A subset of [jQuery](https://jquery.com/) version 3.3.1 is retrieved from the CDNJS provider.</span></span> <span data-ttu-id="322bb-170">Подмножество определяется в `files` свойство&mdash;*jquery.min.js*, *jquery.js*, и *jquery.min.map*.</span><span class="sxs-lookup"><span data-stu-id="322bb-170">The subset is defined in the `files` property&mdash;*jquery.min.js*, *jquery.js*, and *jquery.min.map*.</span></span> <span data-ttu-id="322bb-171">Файлы помещаются в проекте *wwwroot/lib/jquery* папки.</span><span class="sxs-lookup"><span data-stu-id="322bb-171">The files are placed in the project's *wwwroot/lib/jquery* folder.</span></span>
* <span data-ttu-id="322bb-172">Полностью [начальной загрузки](https://getbootstrap.com/) и помещает его в версии 4.1.3 *wwwroot/lib/bootstrap* папки.</span><span class="sxs-lookup"><span data-stu-id="322bb-172">The entirety of [Bootstrap](https://getbootstrap.com/) version 4.1.3 is retrieved and placed in a *wwwroot/lib/bootstrap* folder.</span></span> <span data-ttu-id="322bb-173">Объектный литерал `provider` переопределения свойств `defaultProvider` значение свойства.</span><span class="sxs-lookup"><span data-stu-id="322bb-173">The object literal's `provider` property overrides the `defaultProvider` property value.</span></span> <span data-ttu-id="322bb-174">LibMan извлекает начальной загрузки файлов от поставщика unpkg.</span><span class="sxs-lookup"><span data-stu-id="322bb-174">LibMan retrieves the Bootstrap files from the unpkg provider.</span></span>
* <span data-ttu-id="322bb-175">Подмножество [Lodash](https://lodash.com/) был утвержден, управляющий текст внутри организации.</span><span class="sxs-lookup"><span data-stu-id="322bb-175">A subset of [Lodash](https://lodash.com/) was approved by a governing body within the organization.</span></span> <span data-ttu-id="322bb-176">*Lodash.js* и *lodash.min.js* файлы загружаются из локальной файловой системы в *C:\\tmp\\*.</span><span class="sxs-lookup"><span data-stu-id="322bb-176">The *lodash.js* and *lodash.min.js* files are retrieved from the local file system at *C:\\tmp\\*.</span></span> <span data-ttu-id="322bb-177">Файлы копируются в проект *wwwroot/lib/lodash* папки.</span><span class="sxs-lookup"><span data-stu-id="322bb-177">The files are copied to the project's *wwwroot/lib/lodash* folder.</span></span>

[!code-json[](samples/LibManSample/libman.json)]

> [!NOTE]
> <span data-ttu-id="322bb-178">LibMan поддерживает только одну версию каждой библиотеки из каждого поставщика.</span><span class="sxs-lookup"><span data-stu-id="322bb-178">LibMan only supports one version of each library from each provider.</span></span> <span data-ttu-id="322bb-179">*Libman.json* файла не выполняется проверка схемы, если он содержит две библиотеки с тем же именем библиотеки для данного поставщика.</span><span class="sxs-lookup"><span data-stu-id="322bb-179">The *libman.json* file fails schema validation if it contains two libraries with the same library name for a given provider.</span></span>

## <a name="restore-library-files"></a><span data-ttu-id="322bb-180">Восстановить файлы библиотеки</span><span class="sxs-lookup"><span data-stu-id="322bb-180">Restore library files</span></span>

<span data-ttu-id="322bb-181">Чтобы восстановить файлы библиотеки в Visual Studio, должен иметь допустимый *libman.json* файл в корневом каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="322bb-181">To restore library files from within Visual Studio, there must be a valid *libman.json* file in the project root.</span></span> <span data-ttu-id="322bb-182">Восстановленные файлы помещаются в проект в расположении, указанном для каждой библиотеки.</span><span class="sxs-lookup"><span data-stu-id="322bb-182">Restored files are placed in the project at the location specified for each library.</span></span>

<span data-ttu-id="322bb-183">Можно восстановить файлы библиотеки в проекте ASP.NET Core, двумя способами:</span><span class="sxs-lookup"><span data-stu-id="322bb-183">Library files can be restored in an ASP.NET Core project in two ways:</span></span>

1. [<span data-ttu-id="322bb-184">Восстановление файлов во время сборки</span><span class="sxs-lookup"><span data-stu-id="322bb-184">Restore files during build</span></span>](#restore-files-during-build)
1. [<span data-ttu-id="322bb-185">Восстановить файлы вручную</span><span class="sxs-lookup"><span data-stu-id="322bb-185">Restore files manually</span></span>](#restore-files-manually)

### <a name="restore-files-during-build"></a><span data-ttu-id="322bb-186">Восстановление файлов во время сборки</span><span class="sxs-lookup"><span data-stu-id="322bb-186">Restore files during build</span></span>

<span data-ttu-id="322bb-187">LibMan можно восстановить файлы библиотеки, определенные как часть процесса построения.</span><span class="sxs-lookup"><span data-stu-id="322bb-187">LibMan can restore the defined library files as part of the build process.</span></span> <span data-ttu-id="322bb-188">По умолчанию *восстановления при построении* поведение можно отключить.</span><span class="sxs-lookup"><span data-stu-id="322bb-188">By default, the *restore-on-build* behavior is disabled.</span></span>

<span data-ttu-id="322bb-189">Чтобы включить и проверить поведение восстановления при построении.</span><span class="sxs-lookup"><span data-stu-id="322bb-189">To enable and test the restore-on-build behavior:</span></span>

* <span data-ttu-id="322bb-190">Щелкните правой кнопкой мыши *libman.json* в **обозревателе решений** и выберите **включить восстановление клиентские библиотеки для построения** в контекстном меню.</span><span class="sxs-lookup"><span data-stu-id="322bb-190">Right-click *libman.json* in **Solution Explorer** and select **Enable Restore Client-Side Libraries on Build** from the context menu.</span></span>
* <span data-ttu-id="322bb-191">Нажмите кнопку **Да** кнопки, когда будет предложено установить пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="322bb-191">Click the **Yes** button when prompted to install a NuGet package.</span></span> <span data-ttu-id="322bb-192">[Microsoft.Web.LibraryManager.Build](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) NuGet-пакет добавляется в проект:</span><span class="sxs-lookup"><span data-stu-id="322bb-192">The [Microsoft.Web.LibraryManager.Build](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) NuGet package is added to the project:</span></span>

  [!code-xml[](samples/LibManSample/LibManSample.csproj?name=snippet_RestoreOnBuildPackage)]

* <span data-ttu-id="322bb-193">Постройте проект, чтобы убедиться, что происходит восстановление файлов LibMan.</span><span class="sxs-lookup"><span data-stu-id="322bb-193">Build the project to confirm LibMan file restoration occurs.</span></span> <span data-ttu-id="322bb-194">`Microsoft.Web.LibraryManager.Build` Пакета внедряет целевой объект MSBuild, который выполняется LibMan во время операции построения проекта.</span><span class="sxs-lookup"><span data-stu-id="322bb-194">The `Microsoft.Web.LibraryManager.Build` package injects an MSBuild target that runs LibMan during the project's build operation.</span></span>
* <span data-ttu-id="322bb-195">Просмотрите **построения** веб-канала из **вывода** окно журнала действий LibMan:</span><span class="sxs-lookup"><span data-stu-id="322bb-195">Review the **Build** feed of the **Output** window for a LibMan activity log:</span></span>

  ```console
  1>------ Build started: Project: LibManSample, Configuration: Debug Any CPU ------
  1>
  1>Restore operation started...
  1>Restoring library jquery@3.3.1...
  1>Restoring library bootstrap@4.1.3...
  1>
  1>2 libraries restored in 10.66 seconds
  1>LibManSample -> C:\LibManSample\bin\Debug\netcoreapp2.1\LibManSample.dll
  ========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
  ```

<span data-ttu-id="322bb-196">При включении восстановления при построении поведение *libman.json* отображает контекстное меню **отключить восстановление клиентские библиотеки для построения** параметр.</span><span class="sxs-lookup"><span data-stu-id="322bb-196">When the restore-on-build behavior is enabled, the *libman.json* context menu displays a **Disable Restore Client-Side Libraries on Build** option.</span></span> <span data-ttu-id="322bb-197">Выбор этого варианта удаляет `Microsoft.Web.LibraryManager.Build` ссылка из файла проекта пакет.</span><span class="sxs-lookup"><span data-stu-id="322bb-197">Selecting this option removes the `Microsoft.Web.LibraryManager.Build` package reference from the project file.</span></span> <span data-ttu-id="322bb-198">Следовательно клиентские библиотеки больше не будут восстановлены при каждом построении.</span><span class="sxs-lookup"><span data-stu-id="322bb-198">Consequently, the client-side libraries are no longer restored on each build.</span></span>

<span data-ttu-id="322bb-199">Независимо от настройки восстановления на build, можно восстановить вручную в любое время с *libman.json* контекстного меню.</span><span class="sxs-lookup"><span data-stu-id="322bb-199">Regardless of the restore-on-build setting, you can manually restore at any time from the *libman.json* context menu.</span></span> <span data-ttu-id="322bb-200">Дополнительные сведения см. в разделе [восстановить файлы вручную](#restore-files-manually).</span><span class="sxs-lookup"><span data-stu-id="322bb-200">For more information, see [Restore files manually](#restore-files-manually).</span></span>

### <a name="restore-files-manually"></a><span data-ttu-id="322bb-201">Восстановить файлы вручную</span><span class="sxs-lookup"><span data-stu-id="322bb-201">Restore files manually</span></span>

<span data-ttu-id="322bb-202">Чтобы вручную восстановить файлы библиотеки:</span><span class="sxs-lookup"><span data-stu-id="322bb-202">To manually restore library files:</span></span>

* <span data-ttu-id="322bb-203">Для всех проектов в решении:</span><span class="sxs-lookup"><span data-stu-id="322bb-203">For all projects in the solution:</span></span>
  * <span data-ttu-id="322bb-204">Щелкните правой кнопкой мыши имя решения в **обозревателе решений**.</span><span class="sxs-lookup"><span data-stu-id="322bb-204">Right-click the solution name in **Solution Explorer**.</span></span>
  * <span data-ttu-id="322bb-205">Выберите **восстановить клиентские библиотеки** параметр.</span><span class="sxs-lookup"><span data-stu-id="322bb-205">Select the **Restore Client-Side Libraries** option.</span></span>
* <span data-ttu-id="322bb-206">Для конкретного проекта:</span><span class="sxs-lookup"><span data-stu-id="322bb-206">For a specific project:</span></span>
  * <span data-ttu-id="322bb-207">Щелкните правой кнопкой мыши *libman.json* файл **обозревателе решений**.</span><span class="sxs-lookup"><span data-stu-id="322bb-207">Right-click the *libman.json* file in **Solution Explorer**.</span></span>
  * <span data-ttu-id="322bb-208">Выберите **восстановить клиентские библиотеки** параметр.</span><span class="sxs-lookup"><span data-stu-id="322bb-208">Select the **Restore Client-Side Libraries** option.</span></span>

<span data-ttu-id="322bb-209">Во время выполнения операции восстановления:</span><span class="sxs-lookup"><span data-stu-id="322bb-209">While the restore operation is running:</span></span>

* <span data-ttu-id="322bb-210">Значок центра состояния задач (TSC) в строке состояния Visual Studio будет анимироваться и будет считывать *запуска операции восстановления*.</span><span class="sxs-lookup"><span data-stu-id="322bb-210">The Task Status Center (TSC) icon on the Visual Studio status bar will be animated and will read *Restore operation started*.</span></span> <span data-ttu-id="322bb-211">При выборе значка открывается со списком известных фоновых задач всплывающей подсказки.</span><span class="sxs-lookup"><span data-stu-id="322bb-211">Clicking the icon opens a tooltip listing the known background tasks.</span></span>
* <span data-ttu-id="322bb-212">Сообщения будут отправляться в строке состояния и **диспетчер библиотек** веб-канала из **вывода** окна.</span><span class="sxs-lookup"><span data-stu-id="322bb-212">Messages will be sent to the status bar and the **Library Manager** feed of the **Output** window.</span></span> <span data-ttu-id="322bb-213">Пример:</span><span class="sxs-lookup"><span data-stu-id="322bb-213">For example:</span></span>

  ```console
  Restore operation started...
  Restoring libraries for project LibManSample
  Restoring library jquery@3.3.1... (LibManSample)
  wwwroot/lib/jquery/jquery.min.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.min.map written to destination (LibManSample)
  Restore operation completed
  1 libraries restored in 2.32 seconds
  ```

## <a name="delete-library-files"></a><span data-ttu-id="322bb-214">Удалить файлы библиотеки</span><span class="sxs-lookup"><span data-stu-id="322bb-214">Delete library files</span></span>

<span data-ttu-id="322bb-215">Для выполнения *чистой* операцию, которая удаляет файлы библиотек, ранее восстановлена в Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="322bb-215">To perform the *clean* operation, which deletes library files previously restored in Visual Studio:</span></span>

* <span data-ttu-id="322bb-216">Щелкните правой кнопкой мыши *libman.json* файл **обозревателе решений**.</span><span class="sxs-lookup"><span data-stu-id="322bb-216">Right-click the *libman.json* file in **Solution Explorer**.</span></span>
* <span data-ttu-id="322bb-217">Выберите **чистой клиентские библиотеки** параметр.</span><span class="sxs-lookup"><span data-stu-id="322bb-217">Select the **Clean Client-Side Libraries** option.</span></span>

<span data-ttu-id="322bb-218">Во избежание непреднамеренного удаления файлов не является библиотекой операции очистки не удаляет каталоги целиком.</span><span class="sxs-lookup"><span data-stu-id="322bb-218">To prevent unintentional removal of non-library files, the clean operation doesn't delete whole directories.</span></span> <span data-ttu-id="322bb-219">Удаляет только файлы, которые были включены в предыдущее восстановление.</span><span class="sxs-lookup"><span data-stu-id="322bb-219">It only removes files that were included in the previous restore.</span></span>

<span data-ttu-id="322bb-220">Во время выполнения операции очистки:</span><span class="sxs-lookup"><span data-stu-id="322bb-220">While the clean operation is running:</span></span>

* <span data-ttu-id="322bb-221">Значок TSC в строке состояния Visual Studio будет анимироваться и будет считывать *начала операции библиотеки клиента*.</span><span class="sxs-lookup"><span data-stu-id="322bb-221">The TSC icon on the Visual Studio status bar will be animated and will read *Client libraries operation started*.</span></span> <span data-ttu-id="322bb-222">При выборе значка открывается со списком известных фоновых задач всплывающей подсказки.</span><span class="sxs-lookup"><span data-stu-id="322bb-222">Clicking the icon opens a tooltip listing the known background tasks.</span></span>
* <span data-ttu-id="322bb-223">Сообщения отправляются в строке состояния и **диспетчер библиотек** веб-канала из **вывода** окна.</span><span class="sxs-lookup"><span data-stu-id="322bb-223">Messages are sent to the status bar and the **Library Manager** feed of the **Output** window.</span></span> <span data-ttu-id="322bb-224">Пример:</span><span class="sxs-lookup"><span data-stu-id="322bb-224">For example:</span></span>

```console
Clean libraries operation started...
Clean libraries operation completed
2 libraries were successfully deleted in 1.91 secs
```

<span data-ttu-id="322bb-225">Операции очистки удаляет только файлы из проекта.</span><span class="sxs-lookup"><span data-stu-id="322bb-225">The clean operation only deletes files from the project.</span></span> <span data-ttu-id="322bb-226">Файлы библиотек остаются в кэше для ускорения извлечения для операции восстановления в будущем.</span><span class="sxs-lookup"><span data-stu-id="322bb-226">Library files stay in the cache for faster retrieval on future restore operations.</span></span> <span data-ttu-id="322bb-227">Чтобы управлять файлы библиотек, хранящиеся в кэше на локальном компьютере, используйте LibMan интерфейса командной строки.</span><span class="sxs-lookup"><span data-stu-id="322bb-227">To manage library files stored in the local machine's cache, use the LibMan CLI.</span></span>

## <a name="uninstall-library-files"></a><span data-ttu-id="322bb-228">Удалите файлы библиотеки</span><span class="sxs-lookup"><span data-stu-id="322bb-228">Uninstall library files</span></span>

<span data-ttu-id="322bb-229">Чтобы удалить файлы библиотеки:</span><span class="sxs-lookup"><span data-stu-id="322bb-229">To uninstall library files:</span></span>

* <span data-ttu-id="322bb-230">Откройте *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="322bb-230">Open *libman.json*.</span></span>
* <span data-ttu-id="322bb-231">Поместите курсор внутри соответствующего `libraries` литерала объекта.</span><span class="sxs-lookup"><span data-stu-id="322bb-231">Position the caret inside the corresponding `libraries` object literal.</span></span>
* <span data-ttu-id="322bb-232">Щелкните значок лампочки, который отображается в левом поле и выберите **удаления \<library_name > @\<library_version >**:</span><span class="sxs-lookup"><span data-stu-id="322bb-232">Click the light bulb icon that appears in the left margin, and select **Uninstall \<library_name>@\<library_version>**:</span></span>

  ![Удаление библиотеки контекстного меню](_static/uninstall-menu-option.png)

<span data-ttu-id="322bb-234">Кроме того, можно вручную изменить и сохранить манифест LibMan (*libman.json*).</span><span class="sxs-lookup"><span data-stu-id="322bb-234">Alternatively, you can manually edit and save the LibMan manifest (*libman.json*).</span></span> <span data-ttu-id="322bb-235">[Операции восстановления](#restore-library-files) выполняется при сохранении файла.</span><span class="sxs-lookup"><span data-stu-id="322bb-235">The [restore operation](#restore-library-files) runs when the file is saved.</span></span> <span data-ttu-id="322bb-236">Файлы библиотек, которые больше не определяются в *libman.json* удаляются из проекта.</span><span class="sxs-lookup"><span data-stu-id="322bb-236">Library files that are no longer defined in *libman.json* are removed from the project.</span></span>

## <a name="update-library-version"></a><span data-ttu-id="322bb-237">Обновить версию библиотеки</span><span class="sxs-lookup"><span data-stu-id="322bb-237">Update library version</span></span>

<span data-ttu-id="322bb-238">Чтобы проверить версию обновленной библиотеки:</span><span class="sxs-lookup"><span data-stu-id="322bb-238">To check for an updated library version:</span></span>

* <span data-ttu-id="322bb-239">Откройте *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="322bb-239">Open *libman.json*.</span></span>
* <span data-ttu-id="322bb-240">Поместите курсор внутри соответствующего `libraries` литерала объекта.</span><span class="sxs-lookup"><span data-stu-id="322bb-240">Position the caret inside the corresponding `libraries` object literal.</span></span>
* <span data-ttu-id="322bb-241">Щелкните значок лампочки, который отображается в левом поле.</span><span class="sxs-lookup"><span data-stu-id="322bb-241">Click the light bulb icon that appears in the left margin.</span></span> <span data-ttu-id="322bb-242">Наведите указатель мыши **проверять наличие обновлений**.</span><span class="sxs-lookup"><span data-stu-id="322bb-242">Hover over **Check for updates**.</span></span>

<span data-ttu-id="322bb-243">LibMan проверяет наличие более новой версией, установленной версии библиотеки.</span><span class="sxs-lookup"><span data-stu-id="322bb-243">LibMan checks for a library version newer than the version installed.</span></span> <span data-ttu-id="322bb-244">Возможны следующие результаты.</span><span class="sxs-lookup"><span data-stu-id="322bb-244">The following outcomes can occur:</span></span>

* <span data-ttu-id="322bb-245">Объект **обновления не найдены** сообщение появляется, если уже установлена последняя версия.</span><span class="sxs-lookup"><span data-stu-id="322bb-245">A **No updates found** message is displayed if the latest version is already installed.</span></span>
* <span data-ttu-id="322bb-246">Последняя стабильная версия отображается, если не установлен.</span><span class="sxs-lookup"><span data-stu-id="322bb-246">The latest stable version is displayed if not already installed.</span></span>

  ![Проверить наличие обновлений в контекстном меню](_static/update-menu-option.png)

* <span data-ttu-id="322bb-248">При наличии предварительной версии, новее, чем установленная версия отображается предварительного выпуска.</span><span class="sxs-lookup"><span data-stu-id="322bb-248">If a pre-release newer than the installed version is available, the pre-release is displayed.</span></span>

<span data-ttu-id="322bb-249">Чтобы понизить до более ранней версии библиотеки, вручную измените *libman.json* файла.</span><span class="sxs-lookup"><span data-stu-id="322bb-249">To downgrade to an older library version, manually edit the *libman.json* file.</span></span> <span data-ttu-id="322bb-250">При сохранении файла LibMan [операции восстановления](#restore-library-files):</span><span class="sxs-lookup"><span data-stu-id="322bb-250">When the file is saved, the LibMan [restore operation](#restore-library-files):</span></span>

* <span data-ttu-id="322bb-251">Удаляет избыточные файлов из предыдущей версии.</span><span class="sxs-lookup"><span data-stu-id="322bb-251">Removes redundant files from the previous version.</span></span>
* <span data-ttu-id="322bb-252">Добавление новых и обновленных файлов из новой версии.</span><span class="sxs-lookup"><span data-stu-id="322bb-252">Adds new and updated files from the new version.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="322bb-253">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="322bb-253">Additional resources</span></span>

* [<span data-ttu-id="322bb-254">Репозиторий LibMan GitHub</span><span class="sxs-lookup"><span data-stu-id="322bb-254">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
