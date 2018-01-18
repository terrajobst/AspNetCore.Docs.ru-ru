---
title: "Используйте шаблон проекта реагирование на них"
author: SteveSandersonMS
description: "Сведения о начале работы с использованием шаблона проекта ASP.NET Core одностраничного приложения (SPA) версии кандидата для реагирование на них и создать реагирование на них приложений."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/06/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react
ms.openlocfilehash: a63d81633a0f37d24ad5e05de293e3c41004eba1
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2018
---
# <a name="use-the-react-project-template-release-candidate"></a><span data-ttu-id="1586c-103">Используйте шаблон проекта реагировать (версия-кандидат)</span><span class="sxs-lookup"><span data-stu-id="1586c-103">Use the React project template (release candidate)</span></span>

> [!NOTE]
> <span data-ttu-id="1586c-104">В этой документации не о шаблоне проекта выпущено реагирование на них.</span><span class="sxs-lookup"><span data-stu-id="1586c-104">This documentation is not about the released React project template.</span></span> <span data-ttu-id="1586c-105">**Данная документация является о версии-кандидате шаблона реагирование на них.**</span><span class="sxs-lookup"><span data-stu-id="1586c-105">**This documentation is about the release candidate of the React template.**</span></span> <span data-ttu-id="1586c-106">Мы надеемся, что для отправки в начале 2018 выпущенной версии.</span><span class="sxs-lookup"><span data-stu-id="1586c-106">We hope to ship the released version in early 2018.</span></span>

<span data-ttu-id="1586c-107">Обновленный шаблон проекта реагирование на них предоставляет удобную начальную точку для ASP.NET Core приложений с помощью реагирование на них и [создать реагирование на них приложении](https://github.com/facebookincubator/create-react-app) (CRA) соглашения для реализации полнофункционального клиентского пользовательского интерфейса (UI).</span><span class="sxs-lookup"><span data-stu-id="1586c-107">The updated React project template provides a convenient starting point for ASP.NET Core apps using React and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="1586c-108">Шаблон соответствует Создание проекта ASP.NET Core в качестве внутренних API и в стандартном проекте CRA реагирования для работы в пользовательском Интерфейсе, а с удобством размещение их в проект одного приложения, можно выполнять построение и публикуются как единое целое.</span><span class="sxs-lookup"><span data-stu-id="1586c-108">The template is equivalent to creating both an ASP.NET Core project to act as an API backend, and a standard CRA React project to act as a UI, but with the convenience of hosting both in a single app project that can be built and published as a single unit.</span></span>

## <a name="create-a-new-app"></a><span data-ttu-id="1586c-109">Создание нового приложения</span><span class="sxs-lookup"><span data-stu-id="1586c-109">Create a new app</span></span>

<span data-ttu-id="1586c-110">Чтобы начать, убедитесь, вы уже [установить обновленный шаблон проекта реагирование на них](xref:spa/index#installation).</span><span class="sxs-lookup"><span data-stu-id="1586c-110">To get started, ensure you've [installed the updated React project template](xref:spa/index#installation).</span></span> <span data-ttu-id="1586c-111">Эти инструкции не относятся к предыдущей шаблон проекта реагирование на них, включенные в .NET Core 2.0.x SDK.</span><span class="sxs-lookup"><span data-stu-id="1586c-111">These instructions don't apply to the previous React project template included in the .NET Core 2.0.x SDK.</span></span>

<span data-ttu-id="1586c-112">Создание нового проекта из командной строки с помощью команды `dotnet new react` в пустой каталог.</span><span class="sxs-lookup"><span data-stu-id="1586c-112">Create a new project from a command prompt using the command `dotnet new react` in an empty directory.</span></span> <span data-ttu-id="1586c-113">Например, следующие команды создают приложения в *Мои новое приложение* каталога и перейти к этому каталогу:</span><span class="sxs-lookup"><span data-stu-id="1586c-113">For example, the following commands create the app in a *my-new-app* directory and switch to that directory:</span></span>

```console
dotnet new -o my-new-app
cd my-new-app
```

<span data-ttu-id="1586c-114">Запустите приложение из Visual Studio или .NET Core CLI:</span><span class="sxs-lookup"><span data-stu-id="1586c-114">Run the app from either Visual Studio or the .NET Core CLI:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1586c-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1586c-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1586c-116">Откройте созданный *.csproj* файл и запустить приложение в обычном режиме оттуда.</span><span class="sxs-lookup"><span data-stu-id="1586c-116">Open the generated *.csproj* file, and run the app as normal from there.</span></span>

<span data-ttu-id="1586c-117">Процесс построения восстанавливает npm зависимостей при первом выполнении может занять несколько минут.</span><span class="sxs-lookup"><span data-stu-id="1586c-117">The build process restores npm dependencies on the first run, which can take several minutes.</span></span> <span data-ttu-id="1586c-118">Последующие сборки выполняется гораздо быстрее.</span><span class="sxs-lookup"><span data-stu-id="1586c-118">Subsequent builds are much faster.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1586c-119">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="1586c-119">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1586c-120">Убедитесь в наличии переменной среды `ASPNETCORE_Environment` со значением `Development`.</span><span class="sxs-lookup"><span data-stu-id="1586c-120">Ensure you have an environment variable called `ASPNETCORE_Environment` with value of `Development`.</span></span> <span data-ttu-id="1586c-121">В Windows (в запросах не PowerShell) запускаются `SET ASPNETCORE_Environment=Development`.</span><span class="sxs-lookup"><span data-stu-id="1586c-121">On Windows (in non-PowerShell prompts), run `SET ASPNETCORE_Environment=Development`.</span></span> <span data-ttu-id="1586c-122">В Linux или macOS, запустите `export ASPNETCORE_Environment=Development`.</span><span class="sxs-lookup"><span data-stu-id="1586c-122">On Linux or macOS, run `export ASPNETCORE_Environment=Development`.</span></span>

<span data-ttu-id="1586c-123">Запустите `dotnet build` для проверки приложения правильно ли выполняется сборка.</span><span class="sxs-lookup"><span data-stu-id="1586c-123">Run `dotnet build` to verify your app builds correctly.</span></span> <span data-ttu-id="1586c-124">Во время первого выполнения процесса построения восстанавливает npm зависимости, что может занять несколько минут.</span><span class="sxs-lookup"><span data-stu-id="1586c-124">On the first run, the build process restores npm dependencies, which can take several minutes.</span></span> <span data-ttu-id="1586c-125">Последующие сборки выполняется гораздо быстрее.</span><span class="sxs-lookup"><span data-stu-id="1586c-125">Subsequent builds are much faster.</span></span>

<span data-ttu-id="1586c-126">Запустите `dotnet run` для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="1586c-126">Run `dotnet run` to start the app.</span></span>

---

<span data-ttu-id="1586c-127">Шаблон проекта создает приложение ASP.NET Core и реагирование на них приложение.</span><span class="sxs-lookup"><span data-stu-id="1586c-127">The project template creates an ASP.NET Core app and a React app.</span></span> <span data-ttu-id="1586c-128">Приложения ASP.NET Core предназначен для использования для доступа к данным, авторизации и других проблем на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="1586c-128">The ASP.NET Core app is intended to be used for data access, authorization, and other server-side concerns.</span></span> <span data-ttu-id="1586c-129">Реагирование на них приложения, размещенные в *ClientApp* подкаталог, предназначен для использования все проблемы пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="1586c-129">The React app, residing in the *ClientApp* subdirectory, is intended to be used for all UI concerns.</span></span>

## <a name="add-pages-images-styles-modules-etc"></a><span data-ttu-id="1586c-130">Добавление страниц, изображений, стили, модули, и т. д.</span><span class="sxs-lookup"><span data-stu-id="1586c-130">Add pages, images, styles, modules, etc.</span></span>

<span data-ttu-id="1586c-131">*ClientApp* каталог представляет собой стандартный приложение CRA реагирования.</span><span class="sxs-lookup"><span data-stu-id="1586c-131">The *ClientApp* directory is a standard CRA React app.</span></span> <span data-ttu-id="1586c-132">В разделе официальные [CRA документации](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="1586c-132">See the official [CRA documentation](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) for more information.</span></span>

<span data-ttu-id="1586c-133">Существуют небольшие различия между реагирование на них приложений, созданных с использованием этого шаблона и созданные CRA самостоятельно. Однако в приложении возможности не изменились.</span><span class="sxs-lookup"><span data-stu-id="1586c-133">There are slight differences between the React app created by this template and the one created by CRA itself; however, the app's capabilities are unchanged.</span></span> <span data-ttu-id="1586c-134">Приложение, созданное с помощью шаблона содержит [начальной загрузки](https://getbootstrap.com/)-макета и простой пример маршрутизации на основе.</span><span class="sxs-lookup"><span data-stu-id="1586c-134">The app created by the template contains a [Bootstrap](https://getbootstrap.com/)-based layout and a basic routing example.</span></span>

## <a name="install-npm-packages"></a><span data-ttu-id="1586c-135">Установка пакета npm</span><span class="sxs-lookup"><span data-stu-id="1586c-135">Install npm packages</span></span>

<span data-ttu-id="1586c-136">Для установки пакета npm сторонних разработчиков, используйте в командной *ClientApp* подкаталог.</span><span class="sxs-lookup"><span data-stu-id="1586c-136">To install third-party npm packages, use a command prompt in the *ClientApp* subdirectory.</span></span> <span data-ttu-id="1586c-137">Пример:</span><span class="sxs-lookup"><span data-stu-id="1586c-137">For example:</span></span>

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a><span data-ttu-id="1586c-138">Публикация и развертывание</span><span class="sxs-lookup"><span data-stu-id="1586c-138">Publish and deploy</span></span>

<span data-ttu-id="1586c-139">В разработке приложение запускается в режиме, оптимизированный для удобства разработчиков.</span><span class="sxs-lookup"><span data-stu-id="1586c-139">In development, the app runs in a mode optimized for developer convenience.</span></span> <span data-ttu-id="1586c-140">Например JavaScript пакеты содержат исходное сопоставление, (что, когда отладка, вы можете просмотреть исходный код).</span><span class="sxs-lookup"><span data-stu-id="1586c-140">For example, JavaScript bundles include source maps (so that when debugging, you can see your original source code).</span></span> <span data-ttu-id="1586c-141">Приложение отслеживает JavaScript, HTML и CSS в файл изменения на диске и автоматически перекомпилирует и перезагружает, когда она видит эти файлы изменения.</span><span class="sxs-lookup"><span data-stu-id="1586c-141">The app watches JavaScript, HTML, and CSS file changes on disk and automatically recompiles and reloads when it sees those files change.</span></span>

<span data-ttu-id="1586c-142">В рабочей среде обслуживать версии приложения, оптимизированный для производительности.</span><span class="sxs-lookup"><span data-stu-id="1586c-142">In production, serve a version of your app that is optimized for performance.</span></span> <span data-ttu-id="1586c-143">Это настраивается происходит автоматически.</span><span class="sxs-lookup"><span data-stu-id="1586c-143">This is configured to happen automatically.</span></span> <span data-ttu-id="1586c-144">При публикации, конфигурации построения выдает уменьшенный, transpiled построения клиентского кода.</span><span class="sxs-lookup"><span data-stu-id="1586c-144">When you publish, the build configuration emits a minified, transpiled build of your client-side code.</span></span> <span data-ttu-id="1586c-145">В отличие от сборки разработки рабочей сборки не требует Node.js установлен на сервере.</span><span class="sxs-lookup"><span data-stu-id="1586c-145">Unlike the development build, the production build doesn't require Node.js to be installed on the server.</span></span>

<span data-ttu-id="1586c-146">Можно использовать стандартный [методы размещения и развертывания ASP.NET Core](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="1586c-146">You can use standard [ASP.NET Core hosting and deployment methods](xref:host-and-deploy/index).</span></span>

## <a name="run-the-cra-server-independently"></a><span data-ttu-id="1586c-147">Запустите сервер CRA независимо друг от друга</span><span class="sxs-lookup"><span data-stu-id="1586c-147">Run the CRA server independently</span></span>

<span data-ttu-id="1586c-148">Проект настроен для запуска собственного экземпляра сервера разработки CRA в фоновом режиме при запуске приложения ASP.NET Core в режиме разработки.</span><span class="sxs-lookup"><span data-stu-id="1586c-148">The project is configured to start its own instance of the CRA development server in the background when the ASP.NET Core app starts in development mode.</span></span> <span data-ttu-id="1586c-149">Это удобно, так как это означает, что не нужно вручную запустить отдельный сервер.</span><span class="sxs-lookup"><span data-stu-id="1586c-149">This is convenient because it means you don't have to run a separate server manually.</span></span>

<span data-ttu-id="1586c-150">Недостаток этой настройки по умолчанию не существует.</span><span class="sxs-lookup"><span data-stu-id="1586c-150">There is a drawback to this default setup.</span></span> <span data-ttu-id="1586c-151">Каждый раз при изменении кода на C# и ASP.NET основные приложения необходима перезагрузка, CRA сервер перезагрузится.</span><span class="sxs-lookup"><span data-stu-id="1586c-151">Each time you modify your C# code and your ASP.NET Core app needs to restart, the CRA server restarts.</span></span> <span data-ttu-id="1586c-152">Чтобы запустить резервное копирование необходимы через несколько секунд.</span><span class="sxs-lookup"><span data-stu-id="1586c-152">A few seconds are required to start back up.</span></span> <span data-ttu-id="1586c-153">Если вы вносите частые изменения кода C# и не хотите ждать CRA к перезагрузке сервера, запустите сервера CRA извне, независимо от процесса ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1586c-153">If you're making frequent C# code edits and don't want to wait for the CRA server to restart, run the CRA server externally, independently of the ASP.NET Core process.</span></span> <span data-ttu-id="1586c-154">Для этого:</span><span class="sxs-lookup"><span data-stu-id="1586c-154">To do so:</span></span>

1. <span data-ttu-id="1586c-155">В командной строке перейдите в *ClientApp* подкаталог и запустить на сервере разработки CRA:</span><span class="sxs-lookup"><span data-stu-id="1586c-155">In a command prompt, switch to the *ClientApp* subdirectory, and launch the CRA development server:</span></span>

    ```console
    cd ClientApp
    npm start
    ```

2. <span data-ttu-id="1586c-156">Измените приложения ASP.NET Core к использованию экземпляра сервера внешней CRA вместо запуска одного свои собственные.</span><span class="sxs-lookup"><span data-stu-id="1586c-156">Modify your ASP.NET Core app to use the external CRA server instance instead of launching one of its own.</span></span> <span data-ttu-id="1586c-157">В вашей *запуска* класса, замените `spa.UseReactDevelopmentServer` вызова со следующими:</span><span class="sxs-lookup"><span data-stu-id="1586c-157">In your *Startup* class, replace the `spa.UseReactDevelopmentServer` invocation with the following:</span></span>

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

<span data-ttu-id="1586c-158">При запуске приложения ASP.NET Core, этого не будет запущен сервер CRA.</span><span class="sxs-lookup"><span data-stu-id="1586c-158">When you start your ASP.NET Core app, it won't launch a CRA server.</span></span> <span data-ttu-id="1586c-159">Вместо него используется экземпляр, который можно запустить вручную.</span><span class="sxs-lookup"><span data-stu-id="1586c-159">The instance you started manually is used instead.</span></span> <span data-ttu-id="1586c-160">Это позволяет запустить и перезапустить быстрее.</span><span class="sxs-lookup"><span data-stu-id="1586c-160">This enables it to start and restart faster.</span></span> <span data-ttu-id="1586c-161">Реагирование на них приложения для перестроения каждый раз больше не ожидает.</span><span class="sxs-lookup"><span data-stu-id="1586c-161">It's no longer waiting for your React app to rebuild each time.</span></span>
