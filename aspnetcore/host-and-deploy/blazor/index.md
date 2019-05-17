---
title: Размещение и развертывание Blazor
author: guardrex
description: Узнайте, как размещать и развертывать приложения Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/24/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: c8a65b08582102af9129cf71ac4a108a905e49fc
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2019
ms.locfileid: "65085534"
---
# <a name="host-and-deploy-blazor"></a><span data-ttu-id="85e26-103">Размещение и развертывание Blazor</span><span class="sxs-lookup"><span data-stu-id="85e26-103">Host and deploy Blazor</span></span>

<span data-ttu-id="85e26-104">Авторы: [Люк Лэтем](https://github.com/guardrex), [Рэйнер Стропек](https://www.timecockpit.com) и [Дэниэл Рот](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="85e26-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="85e26-105">Публикация приложения</span><span class="sxs-lookup"><span data-stu-id="85e26-105">Publish the app</span></span>

<span data-ttu-id="85e26-106">Приложения публикуются для развертывания в конфигурации выпуска.</span><span class="sxs-lookup"><span data-stu-id="85e26-106">Apps are published for deployment in Release configuration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="85e26-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="85e26-107">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="85e26-108">На панели навигации выберите **Сборка** > **Опубликовать {приложение}**.</span><span class="sxs-lookup"><span data-stu-id="85e26-108">Select **Build** > **Publish {APPLICATION}** from the navigation bar.</span></span>
1. <span data-ttu-id="85e26-109">Выберите *целевой объект публикации*.</span><span class="sxs-lookup"><span data-stu-id="85e26-109">Select the *publish target*.</span></span> <span data-ttu-id="85e26-110">Чтобы опубликовать объект в локальной среде, выберите **папку**.</span><span class="sxs-lookup"><span data-stu-id="85e26-110">To publish locally, select **Folder**.</span></span>
1. <span data-ttu-id="85e26-111">Оставьте расположение по умолчанию в поле **выбора папки** или укажите другое расположение.</span><span class="sxs-lookup"><span data-stu-id="85e26-111">Accept the default location in the **Choose a folder** field or specify a different location.</span></span> <span data-ttu-id="85e26-112">Нажмите кнопку **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="85e26-112">Select the **Publish** button.</span></span>


# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="85e26-113">Visual Studio Code или .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="85e26-113">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="85e26-114">Используйте команду [dotnet publish](/dotnet/core/tools/dotnet-publish), чтобы опубликовать приложение с конфигурацией выпуска:</span><span class="sxs-lookup"><span data-stu-id="85e26-114">Use the [dotnet publish](/dotnet/core/tools/dotnet-publish) command to publish the app with a Release configuration:</span></span>

```console
dotnet publish -c Release
```

---

<span data-ttu-id="85e26-115">Публикация приложения активирует [восстановление](/dotnet/core/tools/dotnet-restore) зависимостей проекта и выполняет [сборку](/dotnet/core/tools/dotnet-build) проекта, прежде чем создавать ресурсы для развертывания.</span><span class="sxs-lookup"><span data-stu-id="85e26-115">Publishing the app triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="85e26-116">В ходе процесса построения удаляются неиспользуемые методы и сборки, чтобы уменьшить размер скачиваемого приложения и время загрузки.</span><span class="sxs-lookup"><span data-stu-id="85e26-116">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span>

<span data-ttu-id="85e26-117">Приложения Blazor на стороне клиента публикуются в папке */bin/Release/{целевая_платформа}/dist*.</span><span class="sxs-lookup"><span data-stu-id="85e26-117">A Blazor client-side app is published to the */bin/Release/{TARGET FRAMEWORK}/dist* folder.</span></span> <span data-ttu-id="85e26-118">Приложения Blazor на стороне сервера публикуются в папке */bin/Release/{целевая_платформа}/publish*.</span><span class="sxs-lookup"><span data-stu-id="85e26-118">A Blazor server-side app is published to the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span>

<span data-ttu-id="85e26-119">Ресурсы из папки развертываются на веб-сервере.</span><span class="sxs-lookup"><span data-stu-id="85e26-119">The assets in the folder are deployed to the web server.</span></span> <span data-ttu-id="85e26-120">Развертывание может проводиться вручную или автоматизированно в зависимости от используемых средств разработки.</span><span class="sxs-lookup"><span data-stu-id="85e26-120">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="deployment"></a><span data-ttu-id="85e26-121">Развертывание</span><span class="sxs-lookup"><span data-stu-id="85e26-121">Deployment</span></span>

<span data-ttu-id="85e26-122">Инструкции по развертыванию см. в следующих разделах:</span><span class="sxs-lookup"><span data-stu-id="85e26-122">For deployment guidance, see the following topics:</span></span>

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>
