---
title: Используйте шаблон проекта реагирование на них с помощью ASP.NET Core
author: SteveSandersonMS
description: Сведения о начале работы с использованием шаблона проекта ASP.NET Core одной страницы приложений (SPA) для создания-реагирование на них приложение и реагирование на них.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react
ms.openlocfilehash: 4dcfef2bbb99873a9d716a4942f39123944f495c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
---
# <a name="use-the-react-project-template-with-aspnet-core"></a><span data-ttu-id="cf75a-103">Используйте шаблон проекта реагирование на них с помощью ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cf75a-103">Use the React project template with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="cf75a-104">В этой документации не о шаблоне проекта реагирование на них включена в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="cf75a-104">This documentation isn't about the React project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="cf75a-105">Дело новым шаблоном реагирование на них, к которому можно обновить вручную.</span><span class="sxs-lookup"><span data-stu-id="cf75a-105">It's about the newer React template to which you can update manually.</span></span> <span data-ttu-id="cf75a-106">Шаблон по умолчанию включены в ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="cf75a-106">The template is included in ASP.NET Core 2.1 by default.</span></span>

<span data-ttu-id="cf75a-107">Обновленный шаблон проекта реагирование на них предоставляет удобную начальную точку для ASP.NET Core приложений с помощью реагирование на них и [создать реагирование на них приложении](https://github.com/facebookincubator/create-react-app) (CRA) соглашения для реализации полнофункционального клиентского пользовательского интерфейса (UI).</span><span class="sxs-lookup"><span data-stu-id="cf75a-107">The updated React project template provides a convenient starting point for ASP.NET Core apps using React and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="cf75a-108">Шаблон соответствует Создание проекта ASP.NET Core в качестве внутренних API и в стандартном проекте CRA реагирования для работы в пользовательском Интерфейсе, а с удобством размещение их в проект одного приложения, можно выполнять построение и публикуются как единое целое.</span><span class="sxs-lookup"><span data-stu-id="cf75a-108">The template is equivalent to creating both an ASP.NET Core project to act as an API backend, and a standard CRA React project to act as a UI, but with the convenience of hosting both in a single app project that can be built and published as a single unit.</span></span>

## <a name="create-a-new-app"></a><span data-ttu-id="cf75a-109">Создание нового приложения</span><span class="sxs-lookup"><span data-stu-id="cf75a-109">Create a new app</span></span>

<span data-ttu-id="cf75a-110">При использовании ASP.NET Core 2.0, убедитесь, вы уже [установить обновленный шаблон проекта реагирование на них](xref:spa/index#installation).</span><span class="sxs-lookup"><span data-stu-id="cf75a-110">If using ASP.NET Core 2.0, ensure you've [installed the updated React project template](xref:spa/index#installation).</span></span> <span data-ttu-id="cf75a-111">Если у вас есть ASP.NET Core 2.1, нет необходимости для ее установки.</span><span class="sxs-lookup"><span data-stu-id="cf75a-111">If you have ASP.NET Core 2.1, there's no need to install it.</span></span>

<span data-ttu-id="cf75a-112">Создание нового проекта из командной строки с помощью команды `dotnet new react` в пустой каталог.</span><span class="sxs-lookup"><span data-stu-id="cf75a-112">Create a new project from a command prompt using the command `dotnet new react` in an empty directory.</span></span> <span data-ttu-id="cf75a-113">Например, следующие команды создают приложения в *Мои новое приложение* каталога и перейти к этому каталогу:</span><span class="sxs-lookup"><span data-stu-id="cf75a-113">For example, the following commands create the app in a *my-new-app* directory and switch to that directory:</span></span>

```console
dotnet new react -o my-new-app
cd my-new-app
```

<span data-ttu-id="cf75a-114">Запустите приложение из Visual Studio или .NET Core CLI:</span><span class="sxs-lookup"><span data-stu-id="cf75a-114">Run the app from either Visual Studio or the .NET Core CLI:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cf75a-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cf75a-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="cf75a-116">Откройте созданный *.csproj* файл и запустить приложение в обычном режиме оттуда.</span><span class="sxs-lookup"><span data-stu-id="cf75a-116">Open the generated *.csproj* file, and run the app as normal from there.</span></span>

<span data-ttu-id="cf75a-117">Процесс построения восстанавливает npm зависимостей при первом выполнении может занять несколько минут.</span><span class="sxs-lookup"><span data-stu-id="cf75a-117">The build process restores npm dependencies on the first run, which can take several minutes.</span></span> <span data-ttu-id="cf75a-118">Последующие сборки выполняется гораздо быстрее.</span><span class="sxs-lookup"><span data-stu-id="cf75a-118">Subsequent builds are much faster.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="cf75a-119">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="cf75a-119">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="cf75a-120">Убедитесь в наличии переменной среды `ASPNETCORE_Environment` со значением `Development`.</span><span class="sxs-lookup"><span data-stu-id="cf75a-120">Ensure you have an environment variable called `ASPNETCORE_Environment` with value of `Development`.</span></span> <span data-ttu-id="cf75a-121">В Windows (в запросах не PowerShell) запускаются `SET ASPNETCORE_Environment=Development`.</span><span class="sxs-lookup"><span data-stu-id="cf75a-121">On Windows (in non-PowerShell prompts), run `SET ASPNETCORE_Environment=Development`.</span></span> <span data-ttu-id="cf75a-122">В Linux или macOS, запустите `export ASPNETCORE_Environment=Development`.</span><span class="sxs-lookup"><span data-stu-id="cf75a-122">On Linux or macOS, run `export ASPNETCORE_Environment=Development`.</span></span>

<span data-ttu-id="cf75a-123">Запустите [dotnet построения](/dotnet/core/tools/dotnet-build) для проверки приложения правильно ли выполняется сборка.</span><span class="sxs-lookup"><span data-stu-id="cf75a-123">Run [dotnet build](/dotnet/core/tools/dotnet-build) to verify your app builds correctly.</span></span> <span data-ttu-id="cf75a-124">Во время первого выполнения процесса построения восстанавливает npm зависимости, что может занять несколько минут.</span><span class="sxs-lookup"><span data-stu-id="cf75a-124">On the first run, the build process restores npm dependencies, which can take several minutes.</span></span> <span data-ttu-id="cf75a-125">Последующие сборки выполняется гораздо быстрее.</span><span class="sxs-lookup"><span data-stu-id="cf75a-125">Subsequent builds are much faster.</span></span>

<span data-ttu-id="cf75a-126">Запустите [dotnet запуска](/dotnet/core/tools/dotnet-run) для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="cf75a-126">Run [dotnet run](/dotnet/core/tools/dotnet-run) to start the app.</span></span>

---

<span data-ttu-id="cf75a-127">Шаблон проекта создает приложение ASP.NET Core и реагирование на них приложение.</span><span class="sxs-lookup"><span data-stu-id="cf75a-127">The project template creates an ASP.NET Core app and a React app.</span></span> <span data-ttu-id="cf75a-128">Приложения ASP.NET Core предназначен для использования для доступа к данным, авторизации и других проблем на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="cf75a-128">The ASP.NET Core app is intended to be used for data access, authorization, and other server-side concerns.</span></span> <span data-ttu-id="cf75a-129">Реагирование на них приложения, размещенные в *ClientApp* подкаталог, предназначен для использования все проблемы пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="cf75a-129">The React app, residing in the *ClientApp* subdirectory, is intended to be used for all UI concerns.</span></span>

## <a name="add-pages-images-styles-modules-etc"></a><span data-ttu-id="cf75a-130">Добавление страниц, изображений, стили, модули, и т. д.</span><span class="sxs-lookup"><span data-stu-id="cf75a-130">Add pages, images, styles, modules, etc.</span></span>

<span data-ttu-id="cf75a-131">*ClientApp* каталог представляет собой стандартный приложение CRA реагирования.</span><span class="sxs-lookup"><span data-stu-id="cf75a-131">The *ClientApp* directory is a standard CRA React app.</span></span> <span data-ttu-id="cf75a-132">В разделе официальные [CRA документации](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="cf75a-132">See the official [CRA documentation](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) for more information.</span></span>

<span data-ttu-id="cf75a-133">Существуют небольшие различия между реагирование на них приложений, созданных с использованием этого шаблона и созданные CRA самостоятельно. Однако в приложении возможности не изменились.</span><span class="sxs-lookup"><span data-stu-id="cf75a-133">There are slight differences between the React app created by this template and the one created by CRA itself; however, the app's capabilities are unchanged.</span></span> <span data-ttu-id="cf75a-134">Приложение, созданное с помощью шаблона содержит [начальной загрузки](https://getbootstrap.com/)-макета и простой пример маршрутизации на основе.</span><span class="sxs-lookup"><span data-stu-id="cf75a-134">The app created by the template contains a [Bootstrap](https://getbootstrap.com/)-based layout and a basic routing example.</span></span>

## <a name="install-npm-packages"></a><span data-ttu-id="cf75a-135">Установка пакетов npm</span><span class="sxs-lookup"><span data-stu-id="cf75a-135">Install npm packages</span></span>

<span data-ttu-id="cf75a-136">Для установки пакета npm сторонних разработчиков, используйте в командной *ClientApp* подкаталог.</span><span class="sxs-lookup"><span data-stu-id="cf75a-136">To install third-party npm packages, use a command prompt in the *ClientApp* subdirectory.</span></span> <span data-ttu-id="cf75a-137">Пример:</span><span class="sxs-lookup"><span data-stu-id="cf75a-137">For example:</span></span>

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a><span data-ttu-id="cf75a-138">Публикация и развертывание</span><span class="sxs-lookup"><span data-stu-id="cf75a-138">Publish and deploy</span></span>

<span data-ttu-id="cf75a-139">В разработке приложение запускается в режиме, оптимизированный для удобства разработчиков.</span><span class="sxs-lookup"><span data-stu-id="cf75a-139">In development, the app runs in a mode optimized for developer convenience.</span></span> <span data-ttu-id="cf75a-140">Например JavaScript пакеты содержат исходное сопоставление, (что, когда отладка, вы можете просмотреть исходный код).</span><span class="sxs-lookup"><span data-stu-id="cf75a-140">For example, JavaScript bundles include source maps (so that when debugging, you can see your original source code).</span></span> <span data-ttu-id="cf75a-141">Приложение отслеживает JavaScript, HTML и CSS в файл изменения на диске и автоматически перекомпилирует и перезагружает, когда она видит эти файлы изменения.</span><span class="sxs-lookup"><span data-stu-id="cf75a-141">The app watches JavaScript, HTML, and CSS file changes on disk and automatically recompiles and reloads when it sees those files change.</span></span>

<span data-ttu-id="cf75a-142">В рабочей среде обслуживать версии приложения, оптимизированный для производительности.</span><span class="sxs-lookup"><span data-stu-id="cf75a-142">In production, serve a version of your app that's optimized for performance.</span></span> <span data-ttu-id="cf75a-143">Это настраивается происходит автоматически.</span><span class="sxs-lookup"><span data-stu-id="cf75a-143">This is configured to happen automatically.</span></span> <span data-ttu-id="cf75a-144">При публикации, конфигурации построения выдает уменьшенный, transpiled построения клиентского кода.</span><span class="sxs-lookup"><span data-stu-id="cf75a-144">When you publish, the build configuration emits a minified, transpiled build of your client-side code.</span></span> <span data-ttu-id="cf75a-145">В отличие от сборки разработки рабочей сборки не требует Node.js установлен на сервере.</span><span class="sxs-lookup"><span data-stu-id="cf75a-145">Unlike the development build, the production build doesn't require Node.js to be installed on the server.</span></span>

<span data-ttu-id="cf75a-146">Можно использовать стандартный [методы размещения и развертывания ASP.NET Core](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="cf75a-146">You can use standard [ASP.NET Core hosting and deployment methods](xref:host-and-deploy/index).</span></span>

## <a name="run-the-cra-server-independently"></a><span data-ttu-id="cf75a-147">Запустите сервер CRA независимо друг от друга</span><span class="sxs-lookup"><span data-stu-id="cf75a-147">Run the CRA server independently</span></span>

<span data-ttu-id="cf75a-148">Проект настроен для запуска собственного экземпляра сервера разработки CRA в фоновом режиме при запуске приложения ASP.NET Core в режиме разработки.</span><span class="sxs-lookup"><span data-stu-id="cf75a-148">The project is configured to start its own instance of the CRA development server in the background when the ASP.NET Core app starts in development mode.</span></span> <span data-ttu-id="cf75a-149">Это удобно, так как это означает, что не нужно вручную запустить отдельный сервер.</span><span class="sxs-lookup"><span data-stu-id="cf75a-149">This is convenient because it means you don't have to run a separate server manually.</span></span>

<span data-ttu-id="cf75a-150">Недостаток этой настройки по умолчанию не существует.</span><span class="sxs-lookup"><span data-stu-id="cf75a-150">There's a drawback to this default setup.</span></span> <span data-ttu-id="cf75a-151">Каждый раз при изменении кода на C# и ASP.NET основные приложения необходима перезагрузка, CRA сервер перезагрузится.</span><span class="sxs-lookup"><span data-stu-id="cf75a-151">Each time you modify your C# code and your ASP.NET Core app needs to restart, the CRA server restarts.</span></span> <span data-ttu-id="cf75a-152">Чтобы запустить резервное копирование необходимы через несколько секунд.</span><span class="sxs-lookup"><span data-stu-id="cf75a-152">A few seconds are required to start back up.</span></span> <span data-ttu-id="cf75a-153">Если вы вносите частые изменения кода C# и не хотите ждать CRA к перезагрузке сервера, запустите сервера CRA извне, независимо от процесса ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cf75a-153">If you're making frequent C# code edits and don't want to wait for the CRA server to restart, run the CRA server externally, independently of the ASP.NET Core process.</span></span> <span data-ttu-id="cf75a-154">Для этого:</span><span class="sxs-lookup"><span data-stu-id="cf75a-154">To do so:</span></span>

1. <span data-ttu-id="cf75a-155">В командной строке перейдите в *ClientApp* подкаталог и запустить на сервере разработки CRA:</span><span class="sxs-lookup"><span data-stu-id="cf75a-155">In a command prompt, switch to the *ClientApp* subdirectory, and launch the CRA development server:</span></span>

    ```console
    cd ClientApp
    npm start
    ```

2. <span data-ttu-id="cf75a-156">Измените приложения ASP.NET Core к использованию экземпляра сервера внешней CRA вместо запуска одного свои собственные.</span><span class="sxs-lookup"><span data-stu-id="cf75a-156">Modify your ASP.NET Core app to use the external CRA server instance instead of launching one of its own.</span></span> <span data-ttu-id="cf75a-157">В вашей *запуска* класса, замените `spa.UseReactDevelopmentServer` вызова со следующими:</span><span class="sxs-lookup"><span data-stu-id="cf75a-157">In your *Startup* class, replace the `spa.UseReactDevelopmentServer` invocation with the following:</span></span>

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

<span data-ttu-id="cf75a-158">При запуске приложения ASP.NET Core, этого не будет запущен сервер CRA.</span><span class="sxs-lookup"><span data-stu-id="cf75a-158">When you start your ASP.NET Core app, it won't launch a CRA server.</span></span> <span data-ttu-id="cf75a-159">Вместо него используется экземпляр, который можно запустить вручную.</span><span class="sxs-lookup"><span data-stu-id="cf75a-159">The instance you started manually is used instead.</span></span> <span data-ttu-id="cf75a-160">Это позволяет запустить и перезапустить быстрее.</span><span class="sxs-lookup"><span data-stu-id="cf75a-160">This enables it to start and restart faster.</span></span> <span data-ttu-id="cf75a-161">Реагирование на них приложения для перестроения каждый раз больше не ожидает.</span><span class="sxs-lookup"><span data-stu-id="cf75a-161">It's no longer waiting for your React app to rebuild each time.</span></span>
