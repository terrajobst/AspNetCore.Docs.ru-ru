---
title: Размещение и развертывание ASP.NET Core Blazor
author: guardrex
description: Узнайте, как размещать и развертывать приложения Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 9ae0ca406138215c14b59ee395c1f6541ed9dfc9
ms.sourcegitcommit: e5a74f882c14eaa0e5639ff082355e130559ba83
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/20/2019
ms.locfileid: "71168241"
---
# <a name="host-and-deploy-aspnet-core-blazor"></a><span data-ttu-id="ebe92-103">Размещение и развертывание ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="ebe92-103">Host and deploy ASP.NET Core Blazor</span></span>

<span data-ttu-id="ebe92-104">Авторы: [Люк Лэтем](https://github.com/guardrex), [Рэйнер Стропек](https://www.timecockpit.com) и [Дэниэл Рот](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="ebe92-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="publish-the-app"></a><span data-ttu-id="ebe92-105">Публикация приложения</span><span class="sxs-lookup"><span data-stu-id="ebe92-105">Publish the app</span></span>

<span data-ttu-id="ebe92-106">Приложения публикуются для развертывания в конфигурации выпуска.</span><span class="sxs-lookup"><span data-stu-id="ebe92-106">Apps are published for deployment in Release configuration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ebe92-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ebe92-107">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="ebe92-108">На панели навигации выберите **Сборка** > **Опубликовать {приложение}** .</span><span class="sxs-lookup"><span data-stu-id="ebe92-108">Select **Build** > **Publish {APPLICATION}** from the navigation bar.</span></span>
1. <span data-ttu-id="ebe92-109">Выберите *целевой объект публикации*.</span><span class="sxs-lookup"><span data-stu-id="ebe92-109">Select the *publish target*.</span></span> <span data-ttu-id="ebe92-110">Чтобы опубликовать объект в локальной среде, выберите **папку**.</span><span class="sxs-lookup"><span data-stu-id="ebe92-110">To publish locally, select **Folder**.</span></span>
1. <span data-ttu-id="ebe92-111">Оставьте расположение по умолчанию в поле **выбора папки** или укажите другое расположение.</span><span class="sxs-lookup"><span data-stu-id="ebe92-111">Accept the default location in the **Choose a folder** field or specify a different location.</span></span> <span data-ttu-id="ebe92-112">Нажмите кнопку **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="ebe92-112">Select the **Publish** button.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ebe92-113">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="ebe92-113">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="ebe92-114">Используйте команду [dotnet publish](/dotnet/core/tools/dotnet-publish), чтобы опубликовать приложение с конфигурацией выпуска:</span><span class="sxs-lookup"><span data-stu-id="ebe92-114">Use the [dotnet publish](/dotnet/core/tools/dotnet-publish) command to publish the app with a Release configuration:</span></span>

```dotnetcli
dotnet publish -c Release
```

---

<span data-ttu-id="ebe92-115">Публикация приложения активирует [восстановление](/dotnet/core/tools/dotnet-restore) зависимостей проекта и выполняет [сборку](/dotnet/core/tools/dotnet-build) проекта, прежде чем создавать ресурсы для развертывания.</span><span class="sxs-lookup"><span data-stu-id="ebe92-115">Publishing the app triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="ebe92-116">В ходе процесса построения удаляются неиспользуемые методы и сборки, чтобы уменьшить размер скачиваемого приложения и время загрузки.</span><span class="sxs-lookup"><span data-stu-id="ebe92-116">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span>

<span data-ttu-id="ebe92-117">Приложения Blazor WebAssembly публикуются в папке */bin/Release/{целевая_платформа}publish/{имя_сборки}/dist*.</span><span class="sxs-lookup"><span data-stu-id="ebe92-117">A Blazor WebAssembly app is published to the */bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist* folder.</span></span> <span data-ttu-id="ebe92-118">Серверные приложения Blazor публикуются в папке */bin/Release/{целевая_платформа}/publish*.</span><span class="sxs-lookup"><span data-stu-id="ebe92-118">A Blazor Server app is published to the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span>

<span data-ttu-id="ebe92-119">Ресурсы из папки развертываются на веб-сервере.</span><span class="sxs-lookup"><span data-stu-id="ebe92-119">The assets in the folder are deployed to the web server.</span></span> <span data-ttu-id="ebe92-120">Развертывание может проводиться вручную или автоматизированно в зависимости от используемых средств разработки.</span><span class="sxs-lookup"><span data-stu-id="ebe92-120">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="app-base-path"></a><span data-ttu-id="ebe92-121">Базовый путь приложения</span><span class="sxs-lookup"><span data-stu-id="ebe92-121">App base path</span></span>

<span data-ttu-id="ebe92-122">*Базовый путь приложения*  — это корневой URL-путь приложения.</span><span class="sxs-lookup"><span data-stu-id="ebe92-122">The *app base path* is the app's root URL path.</span></span> <span data-ttu-id="ebe92-123">Рассмотрим приведенные далее основное приложение и приложение Blazor.</span><span class="sxs-lookup"><span data-stu-id="ebe92-123">Consider the following main app and Blazor app:</span></span>

* <span data-ttu-id="ebe92-124">Основному приложению задано имя `MyApp`:</span><span class="sxs-lookup"><span data-stu-id="ebe92-124">The main app is called `MyApp`:</span></span>
  * <span data-ttu-id="ebe92-125">физическое расположение приложения — *d:\\MyApp*;</span><span class="sxs-lookup"><span data-stu-id="ebe92-125">The app physically resides at *d:\\MyApp*.</span></span>
  * <span data-ttu-id="ebe92-126">запросы принимаются по адресу: `https://www.contoso.com/{MYAPP RESOURCE}`.</span><span class="sxs-lookup"><span data-stu-id="ebe92-126">Requests are received at `https://www.contoso.com/{MYAPP RESOURCE}`.</span></span>
* <span data-ttu-id="ebe92-127">Приложению Blazor задано имя `CoolApp`. Оно является дочерним приложением `MyApp`:</span><span class="sxs-lookup"><span data-stu-id="ebe92-127">A Blazor app called `CoolApp` is a sub-app of `MyApp`:</span></span>
  * <span data-ttu-id="ebe92-128">физическое расположение дочернего приложения — *d:\\MyApp\\CoolApp*;</span><span class="sxs-lookup"><span data-stu-id="ebe92-128">The sub-app physically resides at *d:\\MyApp\\CoolApp*.</span></span>
  * <span data-ttu-id="ebe92-129">запросы принимаются по адресу: `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`.</span><span class="sxs-lookup"><span data-stu-id="ebe92-129">Requests are received at `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`.</span></span>

<span data-ttu-id="ebe92-130">Если для `CoolApp` не указана дополнительная конфигурация, дочернее приложение в этом сценарии не имеет сведений о своем местоположении на сервере.</span><span class="sxs-lookup"><span data-stu-id="ebe92-130">Without specifying additional configuration for `CoolApp`, the sub-app in this scenario has no knowledge of where it resides on the server.</span></span> <span data-ttu-id="ebe92-131">Например, приложение не может создавать правильные относительные URL-адреса к своим ресурсам, если ему неизвестно, что оно находится по относительному пути URL `/CoolApp/`.</span><span class="sxs-lookup"><span data-stu-id="ebe92-131">For example, the app can't construct correct relative URLs to its resources without knowing that it resides at the relative URL path `/CoolApp/`.</span></span>

<span data-ttu-id="ebe92-132">Чтобы указать конфигурацию для базового пути приложения Blazor (`https://www.contoso.com/CoolApp/`), в качестве значения атрибута `href` тега `<base>` задается относительный корневой путь в файле *wwwroot/index.html*.</span><span class="sxs-lookup"><span data-stu-id="ebe92-132">To provide configuration for the Blazor app's base path of `https://www.contoso.com/CoolApp/`, the `<base>` tag's `href` attribute is set to the relative root path in the *wwwroot/index.html* file:</span></span>

```html
<base href="/CoolApp/">
```

<span data-ttu-id="ebe92-133">При указании относительного пути URL компонент, который не находится в корневом каталоге, может создавать URL-адреса относительно корневого пути приложения.</span><span class="sxs-lookup"><span data-stu-id="ebe92-133">By providing the relative URL path, a component that isn't in the root directory can construct URLs relative to the app's root path.</span></span> <span data-ttu-id="ebe92-134">Компоненты, которые существуют на разных уровнях структуры каталогов, могут создавать ссылки на другие ресурсы во всех местах приложения.</span><span class="sxs-lookup"><span data-stu-id="ebe92-134">Components at different levels of the directory structure can build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="ebe92-135">Основной путь приложения также используется для перехвата щелчка гиперссылок, когда `href` целевой объект ссылки находится в пределах URI-пространства базового пути приложения&mdash;; здесь маршрутизатор Blazor отвечает за внутреннюю навигацию.</span><span class="sxs-lookup"><span data-stu-id="ebe92-135">The app base path is also used to intercept hyperlink clicks where the `href` target of the link is within the app base path URI space&mdash;the Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="ebe92-136">Во многих сценариях размещения относительный путь URL к приложению является корнем приложения.</span><span class="sxs-lookup"><span data-stu-id="ebe92-136">In many hosting scenarios, the relative URL path to the app is the root of the app.</span></span> <span data-ttu-id="ebe92-137">В таких случаях относительный базовый путь URL приложения является косой чертой (`<base href="/" />`), что служит конфигурацией по умолчанию для приложения Blazor.</span><span class="sxs-lookup"><span data-stu-id="ebe92-137">In these cases, the app's relative URL base path is a forward slash (`<base href="/" />`), which is the default configuration for a Blazor app.</span></span> <span data-ttu-id="ebe92-138">В других сценариях размещения, таких как страницы GitHub и дочерние приложения IIS, базовому пути приложения должно быть присвоено значение относительного пути URL к приложению.</span><span class="sxs-lookup"><span data-stu-id="ebe92-138">In other hosting scenarios, such as GitHub Pages and IIS sub-apps, the app base path must be set to the server's relative URL path to the app.</span></span>

<span data-ttu-id="ebe92-139">Чтобы задать базовый путь приложения, измените тег `<base>` в элементах тега `<head>` файла *wwwroot/index.html*.</span><span class="sxs-lookup"><span data-stu-id="ebe92-139">To set the app's base path, update the `<base>` tag within the `<head>` tag elements of the *wwwroot/index.html* file.</span></span> <span data-ttu-id="ebe92-140">Задайте значение атрибута `href` как `/{RELATIVE URL PATH}/` (косая черта в конце обязательна), где `{RELATIVE URL PATH}` — полный относительный путь URL приложения.</span><span class="sxs-lookup"><span data-stu-id="ebe92-140">Set the `href` attribute value to `/{RELATIVE URL PATH}/` (the trailing slash is required), where `{RELATIVE URL PATH}` is the app's full relative URL path.</span></span>

<span data-ttu-id="ebe92-141">Приложение с некорневым относительным путем URL (например, `<base href="/CoolApp/">`) не сможет найти свои ресурсы *при локальном запуске*.</span><span class="sxs-lookup"><span data-stu-id="ebe92-141">For an app with a non-root relative URL path (for example, `<base href="/CoolApp/">`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="ebe92-142">Для решения этой проблемы во время локальной разработки и тестирования можно предоставить аргумент *базового пути*, который соответствует значению `href` тега `<base>` во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="ebe92-142">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span> <span data-ttu-id="ebe92-143">Для передачи аргумента базового пути при локальном запуске приложения выполните из каталога приложения команду `dotnet run` с параметром `--pathbase`:</span><span class="sxs-lookup"><span data-stu-id="ebe92-143">To pass the path base argument when running the app locally, execute the `dotnet run` command from the app's directory with the `--pathbase` option:</span></span>

```dotnetcli
dotnet run --pathbase=/{RELATIVE URL PATH (no trailing slash)}
```

<span data-ttu-id="ebe92-144">Для приложения с относительным путем URL `/CoolApp/` (`<base href="/CoolApp/">`) команда имеет следующий вид:</span><span class="sxs-lookup"><span data-stu-id="ebe92-144">For an app with a relative URL path of `/CoolApp/` (`<base href="/CoolApp/">`), the command is:</span></span>

```dotnetcli
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="ebe92-145">Приложение отвечает локально по `http://localhost:port/CoolApp`.</span><span class="sxs-lookup"><span data-stu-id="ebe92-145">The app responds locally at `http://localhost:port/CoolApp`.</span></span>

## <a name="deployment"></a><span data-ttu-id="ebe92-146">Развертывание</span><span class="sxs-lookup"><span data-stu-id="ebe92-146">Deployment</span></span>

<span data-ttu-id="ebe92-147">Инструкции по развертыванию см. в следующих разделах:</span><span class="sxs-lookup"><span data-stu-id="ebe92-147">For deployment guidance, see the following topics:</span></span>

* <xref:host-and-deploy/blazor/webassembly>
* <xref:host-and-deploy/blazor/server>
