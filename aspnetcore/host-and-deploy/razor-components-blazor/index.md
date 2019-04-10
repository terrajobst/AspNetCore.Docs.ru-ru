---
title: Размещение и развертывание компонентов Razor и Blazor
author: guardrex
description: Размещение и развертывание компонентов Razor и приложений Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: host-and-deploy/razor-components-blazor/index
ms.openlocfilehash: eeaa27bef584523b77d8b47000b310fe0cac7341
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/08/2019
ms.locfileid: "59070311"
---
# <a name="host-and-deploy-razor-components-and-blazor"></a><span data-ttu-id="5a455-103">Размещение и развертывание компонентов Razor и Blazor</span><span class="sxs-lookup"><span data-stu-id="5a455-103">Host and deploy Razor Components and Blazor</span></span>

<span data-ttu-id="5a455-104">Авторы: [Люк Лэтем](https://github.com/guardrex), [Рэйнер Стропек](https://www.timecockpit.com) и [Дэниэл Рот](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="5a455-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="5a455-105">Публикация приложения</span><span class="sxs-lookup"><span data-stu-id="5a455-105">Publish the app</span></span>

<span data-ttu-id="5a455-106">Приложения публикуются для развертывания в конфигурации выпуска при помощи команды [dotnet publish](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="5a455-106">Apps are published for deployment in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="5a455-107">Интегрированная среда разработки (IDE) может обрабатывать выполнение команды `dotnet publish` автоматически с помощью своих встроенных возможностей публикации, поэтому может быть необязательно вручную выполнять команду из командной строки в зависимости от используемых средств разработки.</span><span class="sxs-lookup"><span data-stu-id="5a455-107">An integrated development environment (IDE) may handle executing the `dotnet publish` command automatically using its built-in publishing features, so it might not be necessary to manually execute the command from a command prompt depending on the development tools in use.</span></span>

```console
dotnet publish -c Release
```

`dotnet publish` <span data-ttu-id="5a455-108">активирует [восстановление](/dotnet/core/tools/dotnet-restore) зависимостей проекта и выполняет [сборку](/dotnet/core/tools/dotnet-build) проекта, прежде чем создавать ресурсы для развертывания.</span><span class="sxs-lookup"><span data-stu-id="5a455-108">triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="5a455-109">В ходе процесса построения удаляются неиспользуемые методы и сборки, чтобы уменьшить размер скачиваемого приложения и время загрузки.</span><span class="sxs-lookup"><span data-stu-id="5a455-109">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span> <span data-ttu-id="5a455-110">Развертывание создается в папке */bin/Release/{целевая_платформа}/publish*.</span><span class="sxs-lookup"><span data-stu-id="5a455-110">The deployment is created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span>

<span data-ttu-id="5a455-111">Ресурсы в папке *publish* развертываются на веб-сервере.</span><span class="sxs-lookup"><span data-stu-id="5a455-111">The assets in the *publish* folder are deployed to the web server.</span></span> <span data-ttu-id="5a455-112">Развертывание может проводиться вручную или автоматизированно в зависимости от используемых средств разработки.</span><span class="sxs-lookup"><span data-stu-id="5a455-112">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="deployment"></a><span data-ttu-id="5a455-113">Развертывание</span><span class="sxs-lookup"><span data-stu-id="5a455-113">Deployment</span></span>

<span data-ttu-id="5a455-114">Инструкции по развертыванию см. в следующих разделах:</span><span class="sxs-lookup"><span data-stu-id="5a455-114">For deployment guidance, see the following topics:</span></span>

* [<span data-ttu-id="5a455-115">Компоненты Blazor на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="5a455-115">Client-side Blazor</span></span>](xref:host-and-deploy/razor-components-blazor/blazor)
* [<span data-ttu-id="5a455-116">Компоненты ASP.NET Core Razor на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="5a455-116">Server-side ASP.NET Core Razor Components</span></span>](xref:host-and-deploy/razor-components-blazor/razor-components)
