---
title: "Использование средств Visual Studio для Docker с ASP.NET Core"
author: spboyer
description: "Сведения о том, как упаковать в контейнер приложение ASP.NET Core с помощью средств Visual Studio 2017 и Docker для Windows."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/12/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: b2a3c369a22d50fcefdb96914f5bf84bfafab7cb
ms.sourcegitcommit: 6fa546140575b3eb279eabae12d9acad966f70e0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a><span data-ttu-id="9ac57-103">Использование средств Visual Studio для Docker с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9ac57-103">Visual Studio Tools for Docker with ASP.NET Core</span></span>

<span data-ttu-id="9ac57-104">[Visual Studio 2017 г](https://www.visualstudio.com/) поддерживает построение, отладка и запуск приложений, предназначенных для .NET Core контейнерного ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9ac57-104">[Visual Studio 2017](https://www.visualstudio.com/) supports building, debugging, and running containerized ASP.NET Core apps targeting .NET Core.</span></span> <span data-ttu-id="9ac57-105">Поддерживаются контейнеры Windows и Linux.</span><span class="sxs-lookup"><span data-stu-id="9ac57-105">Both Windows and Linux containers are supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9ac57-106">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="9ac57-106">Prerequisites</span></span>

* <span data-ttu-id="9ac57-107">[Visual Studio 2017](https://www.visualstudio.com/) с рабочей нагрузкой **Кроссплатформенная разработка .NET Core**</span><span class="sxs-lookup"><span data-stu-id="9ac57-107">[Visual Studio 2017](https://www.visualstudio.com/) with the **.NET Core cross-platform development** workload</span></span>
* [<span data-ttu-id="9ac57-108">Docker для Windows</span><span class="sxs-lookup"><span data-stu-id="9ac57-108">Docker for Windows</span></span>](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a><span data-ttu-id="9ac57-109">Установка и настройка</span><span class="sxs-lookup"><span data-stu-id="9ac57-109">Installation and setup</span></span>

<span data-ttu-id="9ac57-110">Перед установкой Docker ознакомьтесь с разделом [Docker для Windows: что следует знать перед установкой](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install), а затем установите [Docker для Windows](https://docs.docker.com/docker-for-windows/install/).</span><span class="sxs-lookup"><span data-stu-id="9ac57-110">For Docker installation, review the information at [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) and install [Docker For Windows](https://docs.docker.com/docker-for-windows/install/).</span></span>

<span data-ttu-id="9ac57-111">Нужно настроить **[Общие диски](https://docs.docker.com/docker-for-windows/#shared-drives)** в Docker для Windows, чтобы обеспечить поддержку сопоставления тома и отладки.</span><span class="sxs-lookup"><span data-stu-id="9ac57-111">**[Shared Drives](https://docs.docker.com/docker-for-windows/#shared-drives)** in Docker for Windows must be configured to support volume mapping and debugging.</span></span> <span data-ttu-id="9ac57-112">Щелкните правой кнопкой мыши значок области уведомлений Docker, выберите **параметры...** и выберите **общих дисков**.</span><span class="sxs-lookup"><span data-stu-id="9ac57-112">Right-click the System Tray's Docker icon, select **Settings...**, and select **Shared Drives**.</span></span> <span data-ttu-id="9ac57-113">Выберите диск, где хранятся файлы в Docker.</span><span class="sxs-lookup"><span data-stu-id="9ac57-113">Select the drive where Docker stores files.</span></span> <span data-ttu-id="9ac57-114">Выберите **применить**.</span><span class="sxs-lookup"><span data-stu-id="9ac57-114">Select **Apply**.</span></span>

![Общие диски](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> <span data-ttu-id="9ac57-116">Версии Visual Studio 2017 г 15.6 и более поздние версии приглашение по **общих дисков** не настроены.</span><span class="sxs-lookup"><span data-stu-id="9ac57-116">Visual Studio 2017 versions 15.6 and later prompt when **Shared Drives** aren't configured.</span></span>

## <a name="add-docker-support-to-an-app"></a><span data-ttu-id="9ac57-117">Добавление поддержки Docker в приложение</span><span class="sxs-lookup"><span data-stu-id="9ac57-117">Add Docker support to an app</span></span>

<span data-ttu-id="9ac57-118">Чтобы добавить поддержку Docker в проекте ASP.NET Core, проекта должны быть предназначены для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="9ac57-118">In order to add Docker support to an ASP.NET Core project, the project must target .NET Core.</span></span> <span data-ttu-id="9ac57-119">Поддерживаются контейнерами Windows и Linux.</span><span class="sxs-lookup"><span data-stu-id="9ac57-119">Both Linux and Windows containers are supported.</span></span>

<span data-ttu-id="9ac57-120">При добавлении поддержки Docker в проект, выберите Windows или Linux контейнера.</span><span class="sxs-lookup"><span data-stu-id="9ac57-120">When adding Docker support to a project, choose either a Windows or a Linux container.</span></span> <span data-ttu-id="9ac57-121">Узел Docker должен работать на контейнерах такого же типа.</span><span class="sxs-lookup"><span data-stu-id="9ac57-121">The Docker host must be running the same container type.</span></span> <span data-ttu-id="9ac57-122">Чтобы изменить тип контейнера для работающего экземпляра Docker, щелкните правой кнопкой мыши значок Docker в области уведомлений и выберите **Переключение на контейнеры Windows** или **Переключение на контейнеры Linux**.</span><span class="sxs-lookup"><span data-stu-id="9ac57-122">To change the container type in the running Docker instance, right-click the System Tray's Docker icon and choose **Switch to Windows containers...** or **Switch to Linux containers...**.</span></span>

### <a name="new-app"></a><span data-ttu-id="9ac57-123">Новое приложение</span><span class="sxs-lookup"><span data-stu-id="9ac57-123">New app</span></span>

<span data-ttu-id="9ac57-124">При создании приложения с помощью шаблонов проектов **веб-приложения ASP.NET Core** установите флажок **Enable Docker Support** (Включение поддержки Docker):</span><span class="sxs-lookup"><span data-stu-id="9ac57-124">When creating a new app with the **ASP.NET Core Web Application** project templates, select the **Enable Docker Support** checkbox:</span></span>

![Флажок "Enable Docker Support" (Включение поддержки Docker)](visual-studio-tools-for-docker/_static/enable-docker-support-checkbox.png)

<span data-ttu-id="9ac57-126">Если требуемая версия .NET framework — .NET Core **ОС** раскрывающегося списка позволяет выбирать тип контейнера.</span><span class="sxs-lookup"><span data-stu-id="9ac57-126">If the target framework is .NET Core, the **OS** drop-down allows for the selection of a container type.</span></span>

### <a name="existing-app"></a><span data-ttu-id="9ac57-127">Существующее приложение</span><span class="sxs-lookup"><span data-stu-id="9ac57-127">Existing app</span></span>

<span data-ttu-id="9ac57-128">Средства Visual Studio для Docker не поддерживают добавление Docker в существующий проект ASP.NET Core, ориентированный на .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="9ac57-128">The Visual Studio Tools for Docker don't support adding Docker to an existing ASP.NET Core project targeting .NET Framework.</span></span> <span data-ttu-id="9ac57-129">Для проектов ASP.NET Core, ориентированных на .NET Core, добавить поддержку Docker через средства можно двумя способами.</span><span class="sxs-lookup"><span data-stu-id="9ac57-129">For ASP.NET Core projects targeting .NET Core, there are two options for adding Docker support via the tooling.</span></span> <span data-ttu-id="9ac57-130">Откройте проект в Visual Studio и выберите один из следующих параметров:</span><span class="sxs-lookup"><span data-stu-id="9ac57-130">Open the project in Visual Studio, and choose one of the following options:</span></span>

* <span data-ttu-id="9ac57-131">Выберите пункт **Поддержка Docker** в меню **Проект**.</span><span class="sxs-lookup"><span data-stu-id="9ac57-131">Select **Docker Support** from the **Project** menu.</span></span>
* <span data-ttu-id="9ac57-132">В обозревателе решений щелкните проект правой кнопкой мыши и выберите пункт **Добавить** > **Поддержка Docker**.</span><span class="sxs-lookup"><span data-stu-id="9ac57-132">Right-click the project in Solution Explorer and select **Add** > **Docker Support**.</span></span>

## <a name="docker-assets-overview"></a><span data-ttu-id="9ac57-133">Обзор ресурсов Docker</span><span class="sxs-lookup"><span data-stu-id="9ac57-133">Docker assets overview</span></span>

<span data-ttu-id="9ac57-134">Средства Visual Studio для Docker добавляют в решение проект *docker-compose* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="9ac57-134">The Visual Studio Tools for Docker add a *docker-compose* project to the solution, containing the following:</span></span>

* <span data-ttu-id="9ac57-135">*.dockerignore*. Содержит список шаблонов файлов и каталогов для исключения при создании контекста сборки.</span><span class="sxs-lookup"><span data-stu-id="9ac57-135">*.dockerignore*: Contains a list of file and directory patterns to exclude when generating a build context.</span></span>
* <span data-ttu-id="9ac57-136">*docker-compose.yml*. Базовый файл [Docker Compose](https://docs.docker.com/compose/overview/), который служит для определения коллекции образов, сборка и запуск которых будут выполняться с помощью команд `docker-compose build` и `docker-compose run`, соответственно.</span><span class="sxs-lookup"><span data-stu-id="9ac57-136">*docker-compose.yml*: The base [Docker Compose](https://docs.docker.com/compose/overview/) file used to define the collection of images to be built and run with `docker-compose build` and `docker-compose run`, respectively.</span></span>
* <span data-ttu-id="9ac57-137">*docker compose.override.yml*. Дополнительный файл, считываемый Docker Compose и содержащий переопределения конфигурации для служб.</span><span class="sxs-lookup"><span data-stu-id="9ac57-137">*docker-compose.override.yml*: An optional file, read by Docker Compose, containing configuration overrides for services.</span></span> <span data-ttu-id="9ac57-138">Visual Studio выполняет `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` для объединения этих файлов.</span><span class="sxs-lookup"><span data-stu-id="9ac57-138">Visual Studio executes `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` to merge these files.</span></span>

<span data-ttu-id="9ac57-139">*Dockerfile* с инструкциями по созданию окончательного образа Docker добавляется в корень проекта.</span><span class="sxs-lookup"><span data-stu-id="9ac57-139">A *Dockerfile*, the recipe for creating a final Docker image, is added to the project root.</span></span> <span data-ttu-id="9ac57-140">См. [справочник по Dockerfile](https://docs.docker.com/engine/reference/builder/) для получения сведений о других доступных в нем командах.</span><span class="sxs-lookup"><span data-stu-id="9ac57-140">Refer to [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for an understanding of the commands within it.</span></span> <span data-ttu-id="9ac57-141">Этот конкретный *Dockerfile* использует [многоэтапную сборку](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) из четырех раздельных именованных этапов:</span><span class="sxs-lookup"><span data-stu-id="9ac57-141">This particular *Dockerfile* uses a [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) containing four distinct, named build stages:</span></span>

[!code-dockerfile[](visual-studio-tools-for-docker/samples/HelloDockerTools/HelloDockerTools/Dockerfile?highlight=1,5,14,17)]

<span data-ttu-id="9ac57-142">*Dockerfile* основан на образе [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore).</span><span class="sxs-lookup"><span data-stu-id="9ac57-142">The *Dockerfile* is based on the [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) image.</span></span> <span data-ttu-id="9ac57-143">Этот базовый образ включает пакеты NuGet платформы ASP.NET Core, которые были предварительно скомпилированы JIT-компилятором для повышения производительности запуска.</span><span class="sxs-lookup"><span data-stu-id="9ac57-143">This base image includes the ASP.NET Core NuGet packages, which have been pre-jitted to improve startup performance.</span></span>

<span data-ttu-id="9ac57-144">*Docker compose.yml* файл содержит имя образа, который создается при выполнении проекта:</span><span class="sxs-lookup"><span data-stu-id="9ac57-144">The *docker-compose.yml* file contains the name of the image that's created when the project runs:</span></span>

[!code-yaml[](visual-studio-tools-for-docker/samples/HelloDockerTools/docker-compose.yml?highlight=5)]

<span data-ttu-id="9ac57-145">В предыдущем примере `image: hellodockertools` создает образ `hellodockertools:dev`, когда приложение выполняется в режиме **отладки**.</span><span class="sxs-lookup"><span data-stu-id="9ac57-145">In the preceding example, `image: hellodockertools` generates the image `hellodockertools:dev` when the app runs in **Debug** mode.</span></span> <span data-ttu-id="9ac57-146">Образ `hellodockertools:latest` создается, когда приложение запускается в режиме **выпуска**.</span><span class="sxs-lookup"><span data-stu-id="9ac57-146">The `hellodockertools:latest` image is generated when the app runs in **Release** mode.</span></span>

<span data-ttu-id="9ac57-147">Префикс имени изображения с [Docker Hub](https://hub.docker.com/) имя пользователя (например, `dockerhubusername/hellodockertools`), если изображение будет помещено в реестре.</span><span class="sxs-lookup"><span data-stu-id="9ac57-147">Prefix the image name with the [Docker Hub](https://hub.docker.com/) username (for example, `dockerhubusername/hellodockertools`) if the image will be pushed to the registry.</span></span> <span data-ttu-id="9ac57-148">Кроме того, изменить имя изображения, чтобы включить URL-адрес частного реестра (например, `privateregistry.domain.com/hellodockertools`) в зависимости от конфигурации.</span><span class="sxs-lookup"><span data-stu-id="9ac57-148">Alternatively, change the image name to include the private registry URL (for example, `privateregistry.domain.com/hellodockertools`) depending on the configuration.</span></span>

## <a name="debug"></a><span data-ttu-id="9ac57-149">Отладка</span><span class="sxs-lookup"><span data-stu-id="9ac57-149">Debug</span></span>

<span data-ttu-id="9ac57-150">Выберите пункт **Docker** в раскрывающемся списке отладки на панели инструментов, чтобы начать отладку приложения.</span><span class="sxs-lookup"><span data-stu-id="9ac57-150">Select **Docker** from the debug drop-down in the toolbar, and start debugging the app.</span></span> <span data-ttu-id="9ac57-151">Представление **Docker** окна **Выходные данные** показывает следующие выполненные действия:</span><span class="sxs-lookup"><span data-stu-id="9ac57-151">The **Docker** view of the **Output** window shows the following actions taking place:</span></span>

* <span data-ttu-id="9ac57-152">*Microsoft/aspnetcore* получена образа среды выполнения (если еще не в кэше).</span><span class="sxs-lookup"><span data-stu-id="9ac57-152">The *microsoft/aspnetcore* runtime image is acquired (if not already in the cache).</span></span>
* <span data-ttu-id="9ac57-153">*Aspnetcore/microsoft построения* компиляции и публикации изображение получается (если еще не в кэше).</span><span class="sxs-lookup"><span data-stu-id="9ac57-153">The *microsoft/aspnetcore-build* compile/publish image is acquired (if not already in the cache).</span></span>
* <span data-ttu-id="9ac57-154">Переменной среды *ASPNETCORE_ENVIRONMENT* присвоено значение `Development` внутри контейнера.</span><span class="sxs-lookup"><span data-stu-id="9ac57-154">The *ASPNETCORE_ENVIRONMENT* environment variable is set to `Development` within the container.</span></span>
* <span data-ttu-id="9ac57-155">Порт 80 открыт и сопоставлен с динамически назначаемым портом для localhost.</span><span class="sxs-lookup"><span data-stu-id="9ac57-155">Port 80 is exposed and mapped to a dynamically-assigned port for localhost.</span></span> <span data-ttu-id="9ac57-156">Порт определяется узлом Docker и может запрашиваться с помощью команды `docker ps`.</span><span class="sxs-lookup"><span data-stu-id="9ac57-156">The port is determined by the Docker host and can be queried with the `docker ps` command.</span></span>
* <span data-ttu-id="9ac57-157">Приложения копируется в контейнер.</span><span class="sxs-lookup"><span data-stu-id="9ac57-157">The app is copied to the container.</span></span>
* <span data-ttu-id="9ac57-158">Браузер по умолчанию запускается с помощью отладчика, присоединенных к контейнеру с помощью динамически назначаемый порт.</span><span class="sxs-lookup"><span data-stu-id="9ac57-158">The default browser is launched with the debugger attached to the container using the dynamically-assigned port.</span></span> 

<span data-ttu-id="9ac57-159">Полученный образ Docker — *разработки* образа приложения с *microsoft/aspnetcore* изображения как базового образа.</span><span class="sxs-lookup"><span data-stu-id="9ac57-159">The resulting Docker image is the *dev* image of the app with the *microsoft/aspnetcore* images as the base image.</span></span> <span data-ttu-id="9ac57-160">Выполните команду `docker images` в окне **Консоль диспетчера пакетов** (PMC).</span><span class="sxs-lookup"><span data-stu-id="9ac57-160">Run the `docker images` command in the **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="9ac57-161">Отображаются изображения на компьютере.</span><span class="sxs-lookup"><span data-stu-id="9ac57-161">The images on the machine are displayed:</span></span>

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                f8f9d6c923e2        About an hour ago   391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        15 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> <span data-ttu-id="9ac57-162">Изображение разработки не имеет содержимое приложения, как **отладки** конфигурации используют монтирование томов для обеспечения взаимодействия с интерактивным.</span><span class="sxs-lookup"><span data-stu-id="9ac57-162">The dev image lacks the app contents, as **Debug** configurations use volume mounting to provide the iterative experience.</span></span> <span data-ttu-id="9ac57-163">Чтобы отправить образ, используйте конфигурацию **выпуска**.</span><span class="sxs-lookup"><span data-stu-id="9ac57-163">To push an image, use the **Release** configuration.</span></span>

<span data-ttu-id="9ac57-164">Выполните команду `docker ps` в PMC.</span><span class="sxs-lookup"><span data-stu-id="9ac57-164">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="9ac57-165">Обратите внимание, что приложение выполняется с помощью контейнера:</span><span class="sxs-lookup"><span data-stu-id="9ac57-165">Notice the app is running using the container:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a><span data-ttu-id="9ac57-166">Изменение и продолжение</span><span class="sxs-lookup"><span data-stu-id="9ac57-166">Edit and continue</span></span>

<span data-ttu-id="9ac57-167">Изменения статических полей и представлений Razor применяются автоматически без необходимости выполнять этап компиляции.</span><span class="sxs-lookup"><span data-stu-id="9ac57-167">Changes to static files and Razor views are automatically updated without the need for a compilation step.</span></span> <span data-ttu-id="9ac57-168">Внесите изменение, сохраните его и перезагрузите страницу в браузере, чтобы увидеть обновление.</span><span class="sxs-lookup"><span data-stu-id="9ac57-168">Make the change, save, and refresh the browser to view the update.</span></span>  

<span data-ttu-id="9ac57-169">Для внесения изменений в файлы кода требуются компиляция и перезапуск Kestrel в контейнере.</span><span class="sxs-lookup"><span data-stu-id="9ac57-169">Modifications to code files requires compiling and a restart of Kestrel within the container.</span></span> <span data-ttu-id="9ac57-170">После внесения изменения нажмите клавиши CTRL+F5, чтобы выполнить процесс и запустить приложение в контейнере.</span><span class="sxs-lookup"><span data-stu-id="9ac57-170">After making the change, use CTRL + F5 to perform the process and start the app within the container.</span></span> <span data-ttu-id="9ac57-171">Контейнер Docker не перестроен или остановлена.</span><span class="sxs-lookup"><span data-stu-id="9ac57-171">The Docker container isn't rebuilt or stopped.</span></span> <span data-ttu-id="9ac57-172">Выполните команду `docker ps` в PMC.</span><span class="sxs-lookup"><span data-stu-id="9ac57-172">Run the `docker ps` command in PMC.</span></span> <span data-ttu-id="9ac57-173">Обратите внимание, что исходный контейнер все еще выполняется, как и 10 минут назад:</span><span class="sxs-lookup"><span data-stu-id="9ac57-173">Notice the original container is still running as of 10 minutes ago:</span></span>

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a><span data-ttu-id="9ac57-174">Публикация образов Docker</span><span class="sxs-lookup"><span data-stu-id="9ac57-174">Publish Docker images</span></span>

<span data-ttu-id="9ac57-175">После завершения цикла разработки и отладки приложения, Visual Studio Tools для Docker помочь при создании образа производственного приложения.</span><span class="sxs-lookup"><span data-stu-id="9ac57-175">Once the develop and debug cycle of the app is completed, the Visual Studio Tools for Docker assist in creating the production image of the app.</span></span> <span data-ttu-id="9ac57-176">Выберите в раскрывающемся списке конфигурации значение **Выпуск** и выполните сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="9ac57-176">Change the configuration drop-down to **Release** and build the app.</span></span> <span data-ttu-id="9ac57-177">Инструментарий создает изображение с *последние* тег, который можно отправить частного реестра или Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="9ac57-177">The tooling produces the image with the *latest* tag, which can be pushed to the private registry or Docker Hub.</span></span> 

<span data-ttu-id="9ac57-178">Выполните команду `docker images` PMC, чтобы просмотреть список образов:</span><span class="sxs-lookup"><span data-stu-id="9ac57-178">Run the `docker images` command in PMC to see the list of images:</span></span>

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                4cb1fca533f0        19 seconds ago      391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        16 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> <span data-ttu-id="9ac57-179">Команда `docker images` возвращает промежуточные образы с именами репозитория и тегами, обозначенными как *\<none>* (не указаны выше).</span><span class="sxs-lookup"><span data-stu-id="9ac57-179">The `docker images` command returns intermediary images with repository names and tags identified as *\<none>* (not listed above).</span></span> <span data-ttu-id="9ac57-180">Эти неименованные образы создаются *Dockerfile* [многоэтапной сборки](https://docs.docker.com/engine/userguide/eng-image/multistage-build/).</span><span class="sxs-lookup"><span data-stu-id="9ac57-180">These unnamed images are produced by the [multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile*.</span></span> <span data-ttu-id="9ac57-181">Они повышают эффективность сборки окончательного образа &mdash; при изменениях перестраиваются только необходимые слои.</span><span class="sxs-lookup"><span data-stu-id="9ac57-181">They improve the efficiency of building the final image&mdash;only the necessary layers are rebuilt when changes occur.</span></span> <span data-ttu-id="9ac57-182">Когда промежуточных образов больше не нужны, удалите их с помощью [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) команды.</span><span class="sxs-lookup"><span data-stu-id="9ac57-182">When the intermediary images are no longer needed, delete them using the [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) command.</span></span>

<span data-ttu-id="9ac57-183">Рабочий образ или образ выпуска может быть меньше по размеру, чем образ *dev*.</span><span class="sxs-lookup"><span data-stu-id="9ac57-183">There may be an expectation for the production or release image to be smaller in size by comparison to the *dev* image.</span></span> <span data-ttu-id="9ac57-184">В результате сопоставления по тома отладчика и приложения были запущены на локальном компьютере, а не внутри контейнера.</span><span class="sxs-lookup"><span data-stu-id="9ac57-184">Because of the volume mapping, the debugger and app were running from the local machine and not within the container.</span></span> <span data-ttu-id="9ac57-185">Образ с тегом *latest* упаковал код приложения, необходимый для запуска приложения на хост-компьютере.</span><span class="sxs-lookup"><span data-stu-id="9ac57-185">The *latest* image has packaged the necessary app code to run the app on a host machine.</span></span> <span data-ttu-id="9ac57-186">Таким образом разностный файл имеет размер кода приложения.</span><span class="sxs-lookup"><span data-stu-id="9ac57-186">Therefore, the delta is the size of the app code.</span></span>
