---
title: Размещение и развертывание Blazor
author: guardrex
description: Узнайте, как размещать и развертывать приложения Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: a7739e2b240d7fd6c85e68105892c802228ebeb5
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614590"
---
# <a name="host-and-deploy-blazor"></a><span data-ttu-id="7529f-103">Размещение и развертывание Blazor</span><span class="sxs-lookup"><span data-stu-id="7529f-103">Host and deploy Blazor</span></span>

<span data-ttu-id="7529f-104">Авторы: [Люк Лэтем](https://github.com/guardrex), [Рэйнер Стропек](https://www.timecockpit.com) и [Дэниэл Рот](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="7529f-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="7529f-105">Публикация приложения</span><span class="sxs-lookup"><span data-stu-id="7529f-105">Publish the app</span></span>

<span data-ttu-id="7529f-106">Приложения публикуются для развертывания в конфигурации выпуска при помощи команды [dotnet publish](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="7529f-106">Apps are published for deployment in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="7529f-107">Интегрированная среда разработки (IDE) может обрабатывать выполнение команды `dotnet publish` автоматически с помощью своих встроенных возможностей публикации, поэтому может быть необязательно вручную выполнять команду из командной строки в зависимости от используемых средств разработки.</span><span class="sxs-lookup"><span data-stu-id="7529f-107">An integrated development environment (IDE) may handle executing the `dotnet publish` command automatically using its built-in publishing features, so it might not be necessary to manually execute the command from a command prompt depending on the development tools in use.</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="7529f-108">`dotnet publish` активирует [восстановление](/dotnet/core/tools/dotnet-restore) зависимостей проекта и проводит [сборку](/dotnet/core/tools/dotnet-build) проекта, прежде чем создавать ресурсы для развертывания.</span><span class="sxs-lookup"><span data-stu-id="7529f-108">`dotnet publish` triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="7529f-109">В ходе процесса построения удаляются неиспользуемые методы и сборки, чтобы уменьшить размер скачиваемого приложения и время загрузки.</span><span class="sxs-lookup"><span data-stu-id="7529f-109">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span> <span data-ttu-id="7529f-110">Развертывание создается в папке */bin/Release/{целевая_платформа}/publish*.</span><span class="sxs-lookup"><span data-stu-id="7529f-110">The deployment is created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span>

<span data-ttu-id="7529f-111">Ресурсы в папке *publish* развертываются на веб-сервере.</span><span class="sxs-lookup"><span data-stu-id="7529f-111">The assets in the *publish* folder are deployed to the web server.</span></span> <span data-ttu-id="7529f-112">Развертывание может проводиться вручную или автоматизированно в зависимости от используемых средств разработки.</span><span class="sxs-lookup"><span data-stu-id="7529f-112">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="deployment"></a><span data-ttu-id="7529f-113">Развертывание</span><span class="sxs-lookup"><span data-stu-id="7529f-113">Deployment</span></span>

<span data-ttu-id="7529f-114">Инструкции по развертыванию см. в следующих разделах:</span><span class="sxs-lookup"><span data-stu-id="7529f-114">For deployment guidance, see the following topics:</span></span>

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>
