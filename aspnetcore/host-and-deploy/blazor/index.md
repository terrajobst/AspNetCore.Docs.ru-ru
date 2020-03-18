---
title: Размещение и развертывание ASP.NET Core Blazor
author: guardrex
description: Узнайте, как размещать и развертывать приложения Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/11/2020
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/index
ms.openlocfilehash: ddf70da29a82d462422c1bdf74ff45b92bb10b56
ms.sourcegitcommit: 5bdc54162d7dea8d9fa54ac3055678db23586af1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/17/2020
ms.locfileid: "79434269"
---
# <a name="host-and-deploy-aspnet-core-blazor"></a><span data-ttu-id="a1de2-103">Размещение и развертывание ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="a1de2-103">Host and deploy ASP.NET Core Blazor</span></span>

<span data-ttu-id="a1de2-104">Авторы: [Люк Лэтем](https://github.com/guardrex), [Рэйнер Стропек](https://www.timecockpit.com) и [Дэниэл Рот](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="a1de2-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="publish-the-app"></a><span data-ttu-id="a1de2-105">Публикация приложения</span><span class="sxs-lookup"><span data-stu-id="a1de2-105">Publish the app</span></span>

<span data-ttu-id="a1de2-106">Приложения публикуются для развертывания в конфигурации выпуска.</span><span class="sxs-lookup"><span data-stu-id="a1de2-106">Apps are published for deployment in Release configuration.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a1de2-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a1de2-107">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="a1de2-108">На панели навигации выберите **Сборка** > **Опубликовать {приложение}** .</span><span class="sxs-lookup"><span data-stu-id="a1de2-108">Select **Build** > **Publish {APPLICATION}** from the navigation bar.</span></span>
1. <span data-ttu-id="a1de2-109">Выберите *целевой объект публикации*.</span><span class="sxs-lookup"><span data-stu-id="a1de2-109">Select the *publish target*.</span></span> <span data-ttu-id="a1de2-110">Чтобы опубликовать объект в локальной среде, выберите **папку**.</span><span class="sxs-lookup"><span data-stu-id="a1de2-110">To publish locally, select **Folder**.</span></span>
1. <span data-ttu-id="a1de2-111">Оставьте расположение по умолчанию в поле **выбора папки** или укажите другое расположение.</span><span class="sxs-lookup"><span data-stu-id="a1de2-111">Accept the default location in the **Choose a folder** field or specify a different location.</span></span> <span data-ttu-id="a1de2-112">Нажмите кнопку **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="a1de2-112">Select the **Publish** button.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="a1de2-113">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="a1de2-113">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a1de2-114">Используйте команду [dotnet publish](/dotnet/core/tools/dotnet-publish), чтобы опубликовать приложение с конфигурацией выпуска:</span><span class="sxs-lookup"><span data-stu-id="a1de2-114">Use the [dotnet publish](/dotnet/core/tools/dotnet-publish) command to publish the app with a Release configuration:</span></span>

```dotnetcli
dotnet publish -c Release
```

---

<span data-ttu-id="a1de2-115">Публикация приложения активирует [восстановление](/dotnet/core/tools/dotnet-restore) зависимостей проекта и выполняет [сборку](/dotnet/core/tools/dotnet-build) проекта, прежде чем создавать ресурсы для развертывания.</span><span class="sxs-lookup"><span data-stu-id="a1de2-115">Publishing the app triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="a1de2-116">В ходе процесса построения удаляются неиспользуемые методы и сборки, чтобы уменьшить размер скачиваемого приложения и время загрузки.</span><span class="sxs-lookup"><span data-stu-id="a1de2-116">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span>

<span data-ttu-id="a1de2-117">Расположения публикации.</span><span class="sxs-lookup"><span data-stu-id="a1de2-117">Publish locations:</span></span>

* Blazor<span data-ttu-id="a1de2-118"> WebAssembly</span><span class="sxs-lookup"><span data-stu-id="a1de2-118"> WebAssembly</span></span>
  * <span data-ttu-id="a1de2-119">Изолированное. Приложение публикуется в папке */bin/Release/{целевая_платформа}/publish/wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="a1de2-119">Standalone &ndash; The app is published into the */bin/Release/{TARGET FRAMEWORK}/publish/wwwroot* folder.</span></span> <span data-ttu-id="a1de2-120">Чтобы развернуть приложение как статический сайт, скопируйте содержимое папки *wwwroot* на узел статического сайта.</span><span class="sxs-lookup"><span data-stu-id="a1de2-120">To deploy the app as a static site, copy the contents of the *wwwroot* folder to the static site host.</span></span>
  * <span data-ttu-id="a1de2-121">Размещенное. Приложение клиента Blazor WebAssembly публикуется в папке */bin/Release/{целевая_платформа}/publish/wwwroot* для серверного приложения вместе с другими статическими веб-ресурсами серверного приложения.</span><span class="sxs-lookup"><span data-stu-id="a1de2-121">Hosted &ndash; The client Blazor WebAssembly app is published into the */bin/Release/{TARGET FRAMEWORK}/publish/wwwroot* folder of the server app, along with any other static web assets of the server app.</span></span> <span data-ttu-id="a1de2-122">Разверните содержимое папки *publish* на узле.</span><span class="sxs-lookup"><span data-stu-id="a1de2-122">Deploy the contents of the *publish* folder to the host.</span></span>
* <span data-ttu-id="a1de2-123">Сервер Blazor. Приложение публикуется в папке */bin/Release/{целевая_платформа}/publish*.</span><span class="sxs-lookup"><span data-stu-id="a1de2-123">Blazor Server &ndash; The app is published into the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span> <span data-ttu-id="a1de2-124">Разверните содержимое папки *publish* на узле.</span><span class="sxs-lookup"><span data-stu-id="a1de2-124">Deploy the contents of the *publish* folder to the host.</span></span>

<span data-ttu-id="a1de2-125">Ресурсы из папки развертываются на веб-сервере.</span><span class="sxs-lookup"><span data-stu-id="a1de2-125">The assets in the folder are deployed to the web server.</span></span> <span data-ttu-id="a1de2-126">Развертывание может проводиться вручную или автоматизированно в зависимости от используемых средств разработки.</span><span class="sxs-lookup"><span data-stu-id="a1de2-126">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="app-base-path"></a><span data-ttu-id="a1de2-127">Базовый путь приложения</span><span class="sxs-lookup"><span data-stu-id="a1de2-127">App base path</span></span>

<span data-ttu-id="a1de2-128">*Базовый путь приложения*  — это корневой URL-путь приложения.</span><span class="sxs-lookup"><span data-stu-id="a1de2-128">The *app base path* is the app's root URL path.</span></span> <span data-ttu-id="a1de2-129">Рассмотрим следующее приложение ASP.NET Core и подприложение Blazor:</span><span class="sxs-lookup"><span data-stu-id="a1de2-129">Consider the following ASP.NET Core app and Blazor sub-app:</span></span>

* <span data-ttu-id="a1de2-130">Приложение ASP.NET Core называется `MyApp`:</span><span class="sxs-lookup"><span data-stu-id="a1de2-130">The ASP.NET Core app is named `MyApp`:</span></span>
  * <span data-ttu-id="a1de2-131">Физическое расположение приложения: *d:/MyApp*.</span><span class="sxs-lookup"><span data-stu-id="a1de2-131">The app physically resides at *d:/MyApp*.</span></span>
  * <span data-ttu-id="a1de2-132">запросы принимаются по адресу: `https://www.contoso.com/{MYAPP RESOURCE}`.</span><span class="sxs-lookup"><span data-stu-id="a1de2-132">Requests are received at `https://www.contoso.com/{MYAPP RESOURCE}`.</span></span>
* <span data-ttu-id="a1de2-133">Приложение Blazor с именем `CoolApp` является подприложением `MyApp`:</span><span class="sxs-lookup"><span data-stu-id="a1de2-133">A Blazor app named `CoolApp` is a sub-app of `MyApp`:</span></span>
  * <span data-ttu-id="a1de2-134">Физическое расположение подприложения — *d:/MyApp/CoolApp*.</span><span class="sxs-lookup"><span data-stu-id="a1de2-134">The sub-app physically resides at *d:/MyApp/CoolApp*.</span></span>
  * <span data-ttu-id="a1de2-135">запросы принимаются по адресу: `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`.</span><span class="sxs-lookup"><span data-stu-id="a1de2-135">Requests are received at `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`.</span></span>

<span data-ttu-id="a1de2-136">Если для `CoolApp` не указана дополнительная конфигурация, дочернее приложение в этом сценарии не имеет сведений о своем местоположении на сервере.</span><span class="sxs-lookup"><span data-stu-id="a1de2-136">Without specifying additional configuration for `CoolApp`, the sub-app in this scenario has no knowledge of where it resides on the server.</span></span> <span data-ttu-id="a1de2-137">Например, приложение не может создавать правильные относительные URL-адреса к своим ресурсам, если ему неизвестно, что оно находится по относительному пути URL `/CoolApp/`.</span><span class="sxs-lookup"><span data-stu-id="a1de2-137">For example, the app can't construct correct relative URLs to its resources without knowing that it resides at the relative URL path `/CoolApp/`.</span></span>

<span data-ttu-id="a1de2-138">Чтобы указать конфигурацию для базового пути к приложению Blazor`https://www.contoso.com/CoolApp/`, в качестве значения атрибута `href` тега `<base>` задается относительный корневой путь в файле *Pages/_Host.cshtml* (сервер Blazor) или в файле *wwwroot/index.html* (Blazor WebAssembly):</span><span class="sxs-lookup"><span data-stu-id="a1de2-138">To provide configuration for the Blazor app's base path of `https://www.contoso.com/CoolApp/`, the `<base>` tag's `href` attribute is set to the relative root path in the *Pages/_Host.cshtml* file (Blazor Server) or *wwwroot/index.html* file (Blazor WebAssembly):</span></span>

```html
<base href="/CoolApp/">
```

<span data-ttu-id="a1de2-139">Серверные приложения Blazor дополнительно устанавливают базовый путь на стороне сервера путем вызова <xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*> в конвейере запросов приложения `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="a1de2-139">Blazor Server apps additionally set the server-side base path by calling <xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*> in the app's request pipeline of `Startup.Configure`:</span></span>

```csharp
app.UsePathBase("/CoolApp");
```

<span data-ttu-id="a1de2-140">При указании относительного пути URL компонент, который не находится в корневом каталоге, может создавать URL-адреса относительно корневого пути приложения.</span><span class="sxs-lookup"><span data-stu-id="a1de2-140">By providing the relative URL path, a component that isn't in the root directory can construct URLs relative to the app's root path.</span></span> <span data-ttu-id="a1de2-141">Компоненты, которые существуют на разных уровнях структуры каталогов, могут создавать ссылки на другие ресурсы во всех местах приложения.</span><span class="sxs-lookup"><span data-stu-id="a1de2-141">Components at different levels of the directory structure can build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="a1de2-142">Базовый путь к приложению также используется для перехвата выбранных гиперссылок, когда целевой объект ссылки `href` находится в пределах URI-пространства базового пути к приложению.</span><span class="sxs-lookup"><span data-stu-id="a1de2-142">The app base path is also used to intercept selected hyperlinks where the `href` target of the link is within the app base path URI space.</span></span> <span data-ttu-id="a1de2-143">Маршрутизатор Blazor обрабатывает внутреннюю навигацию.</span><span class="sxs-lookup"><span data-stu-id="a1de2-143">The Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="a1de2-144">Во многих сценариях размещения относительный путь URL к приложению является корнем приложения.</span><span class="sxs-lookup"><span data-stu-id="a1de2-144">In many hosting scenarios, the relative URL path to the app is the root of the app.</span></span> <span data-ttu-id="a1de2-145">В таких случаях относительный базовый путь URL к приложению представляет собой косую черту (`<base href="/" />`), что является конфигурацией по умолчанию для приложения Blazor.</span><span class="sxs-lookup"><span data-stu-id="a1de2-145">In these cases, the app's relative URL base path is a forward slash (`<base href="/" />`), which is the default configuration for a Blazor app.</span></span> <span data-ttu-id="a1de2-146">В других сценариях размещения, таких как страницы GitHub и подприложения IIS, базовому пути приложения должно быть присвоено значение относительного пути URL к серверу для приложения.</span><span class="sxs-lookup"><span data-stu-id="a1de2-146">In other hosting scenarios, such as GitHub Pages and IIS sub-apps, the app base path must be set to the server's relative URL path of the app.</span></span>

<span data-ttu-id="a1de2-147">Чтобы задать базовый путь к приложению, измените тег `<base>` в элементах `<head>` тега файла *Pages/_Host.cshtml* (сервер Blazor) или файла *wwwroot/index.html* (Blazor WebAssembly).</span><span class="sxs-lookup"><span data-stu-id="a1de2-147">To set the app's base path, update the `<base>` tag within the `<head>` tag elements of the *Pages/_Host.cshtml* file (Blazor Server) or *wwwroot/index.html* file (Blazor WebAssembly).</span></span> <span data-ttu-id="a1de2-148">Задайте значение атрибута `href` как `/{RELATIVE URL PATH}/` (косая черта в конце обязательна), где `{RELATIVE URL PATH}` — полный относительный путь URL приложения.</span><span class="sxs-lookup"><span data-stu-id="a1de2-148">Set the `href` attribute value to `/{RELATIVE URL PATH}/` (the trailing slash is required), where `{RELATIVE URL PATH}` is the app's full relative URL path.</span></span>

<span data-ttu-id="a1de2-149">Приложение Blazor WebAssembly с некорневым относительным путем URL (например, `<base href="/CoolApp/">`) не сможет найти свои ресурсы *при локальном запуске*.</span><span class="sxs-lookup"><span data-stu-id="a1de2-149">For an Blazor WebAssembly app with a non-root relative URL path (for example, `<base href="/CoolApp/">`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="a1de2-150">Для решения этой проблемы во время локальной разработки и тестирования можно предоставить аргумент *базового пути*, который соответствует значению `href` тега `<base>` во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="a1de2-150">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span> <span data-ttu-id="a1de2-151">Не добавляйте в конце косую черту.</span><span class="sxs-lookup"><span data-stu-id="a1de2-151">Don't include a trailing slash.</span></span> <span data-ttu-id="a1de2-152">Для передачи аргумента базового пути при локальном запуске приложения выполните из каталога приложения команду `dotnet run` с параметром `--pathbase`:</span><span class="sxs-lookup"><span data-stu-id="a1de2-152">To pass the path base argument when running the app locally, execute the `dotnet run` command from the app's directory with the `--pathbase` option:</span></span>

```dotnetcli
dotnet run --pathbase=/{RELATIVE URL PATH (no trailing slash)}
```

<span data-ttu-id="a1de2-153">Для приложения Blazor WebAssembly с относительным путем URL `/CoolApp/` (`<base href="/CoolApp/">`) команда имеет следующий вид:</span><span class="sxs-lookup"><span data-stu-id="a1de2-153">For a Blazor WebAssembly app with a relative URL path of `/CoolApp/` (`<base href="/CoolApp/">`), the command is:</span></span>

```dotnetcli
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="a1de2-154">Приложение Blazor WebAssembly отвечает локально по адресу `http://localhost:port/CoolApp`.</span><span class="sxs-lookup"><span data-stu-id="a1de2-154">The Blazor WebAssembly app responds locally at `http://localhost:port/CoolApp`.</span></span>

## <a name="deployment"></a><span data-ttu-id="a1de2-155">Развертывание</span><span class="sxs-lookup"><span data-stu-id="a1de2-155">Deployment</span></span>

<span data-ttu-id="a1de2-156">Инструкции по развертыванию см. в следующих разделах:</span><span class="sxs-lookup"><span data-stu-id="a1de2-156">For deployment guidance, see the following topics:</span></span>

* <xref:host-and-deploy/blazor/webassembly>
* <xref:host-and-deploy/blazor/server>
