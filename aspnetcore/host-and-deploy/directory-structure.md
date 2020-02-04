---
title: Структура каталогов ASP.NET Core
author: guardrex
description: Сведения о структуре каталогов опубликованных приложений ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 01/28/2020
uid: host-and-deploy/directory-structure
ms.openlocfilehash: ba5cb96dfdcdca10034299e3bbe662ce056af791
ms.sourcegitcommit: fe41cff0b99f3920b727286944e5b652ca301640
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2020
ms.locfileid: "76870270"
---
# <a name="aspnet-core-directory-structure"></a><span data-ttu-id="400db-103">Структура каталогов ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="400db-103">ASP.NET Core directory structure</span></span>

<span data-ttu-id="400db-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="400db-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="400db-105">Каталог *публикации* содержит развертываемые ресурсы приложения, созданные командной [dotnet publish](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="400db-105">The *publish* directory contains the app's deployable assets produced by the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="400db-106">Каталог содержит следующее.</span><span class="sxs-lookup"><span data-stu-id="400db-106">The directory contains:</span></span>

* <span data-ttu-id="400db-107">Файлы приложения.</span><span class="sxs-lookup"><span data-stu-id="400db-107">Application files</span></span>
* <span data-ttu-id="400db-108">Файлы конфигурации.</span><span class="sxs-lookup"><span data-stu-id="400db-108">Configuration files</span></span>
* <span data-ttu-id="400db-109">Статические ресурсы.</span><span class="sxs-lookup"><span data-stu-id="400db-109">Static assets</span></span>
* <span data-ttu-id="400db-110">Пакеты</span><span class="sxs-lookup"><span data-stu-id="400db-110">Packages</span></span>
* <span data-ttu-id="400db-111">Среда выполнения ([только автономное развертывание](/dotnet/core/deploying/#self-contained-deployments-scd))</span><span class="sxs-lookup"><span data-stu-id="400db-111">A runtime ([self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) only)</span></span>

| <span data-ttu-id="400db-112">Тип приложения</span><span class="sxs-lookup"><span data-stu-id="400db-112">App Type</span></span> | <span data-ttu-id="400db-113">Структура каталогов</span><span class="sxs-lookup"><span data-stu-id="400db-113">Directory Structure</span></span> |
| -------- | ------------------- |
| [<span data-ttu-id="400db-114">Исполняемый файл, зависящий от платформы (FDE)</span><span class="sxs-lookup"><span data-stu-id="400db-114">Framework-dependent Executable (FDE)</span></span>](/dotnet/core/deploying/#framework-dependent-executables-fde) | <ul><li><span data-ttu-id="400db-115">publish&dagger;</span><span class="sxs-lookup"><span data-stu-id="400db-115">publish&dagger;</span></span><ul><li><span data-ttu-id="400db-116">Views&dagger; в приложениях MVC, если представления не компилируются заранее</span><span class="sxs-lookup"><span data-stu-id="400db-116">Views&dagger; MVC apps; if views aren't precompiled</span></span></li><li><span data-ttu-id="400db-117">Pages&dagger; в приложениях MVC или Razor Pages, если страницы не компилируются заранее</span><span class="sxs-lookup"><span data-stu-id="400db-117">Pages&dagger; MVC or Razor Pages apps, if pages aren't precompiled</span></span></li><li><span data-ttu-id="400db-118">wwwroot&dagger;</span><span class="sxs-lookup"><span data-stu-id="400db-118">wwwroot&dagger;</span></span></li><li><span data-ttu-id="400db-119">*.dll files</li><li>{ASSEMBLY NAME}.deps.json</li><li>{ASSEMBLY NAME}.dll</li><li>{ASSEMBLY NAME}{.EXTENSION} (расширение *EXE* на Windows, без расширения на macOS или Linux)</li><li>{ASSEMBLY NAME}.pdb</li><li>{ASSEMBLY NAME}.Views.dll</li><li>{ASSEMBLY NAME}.Views.pdb</li><li>{ASSEMBLY NAME}.runtimeconfig.json</li><li>web.config (IIS deployments)</li><li>createdump ([служебная программа createdump на Linux](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/xplat-minidump-generation.md#configurationpolicy))</li><li>* .so (общая библиотека объектов Linux)</span><span class="sxs-lookup"><span data-stu-id="400db-119">*.dll files</li><li>{ASSEMBLY NAME}.deps.json</li><li>{ASSEMBLY NAME}.dll</li><li>{ASSEMBLY NAME}{.EXTENSION} *.exe* extension on Windows, no extension on macOS or Linux</li><li>{ASSEMBLY NAME}.pdb</li><li>{ASSEMBLY NAME}.Views.dll</li><li>{ASSEMBLY NAME}.Views.pdb</li><li>{ASSEMBLY NAME}.runtimeconfig.json</li><li>web.config (IIS deployments)</li><li>createdump ([Linux createdump utility](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/xplat-minidump-generation.md#configurationpolicy))</li><li>*.so (Linux shared object library)</span></span></li><li><span data-ttu-id="400db-120">*.a (архив macOS)</li><li>* .dylib (динамическая библиотека macOS)</span><span class="sxs-lookup"><span data-stu-id="400db-120">*.a (macOS archive)</li><li>*.dylib (macOS dynamic library)</span></span></li></ul></li></ul> |
| [<span data-ttu-id="400db-121">Автономное развертывание (SCD)</span><span class="sxs-lookup"><span data-stu-id="400db-121">Self-contained Deployment (SCD)</span></span>](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li><span data-ttu-id="400db-122">publish&dagger;</span><span class="sxs-lookup"><span data-stu-id="400db-122">publish&dagger;</span></span><ul><li><span data-ttu-id="400db-123">Views&dagger; в приложениях MVC, если представления не компилируются заранее</span><span class="sxs-lookup"><span data-stu-id="400db-123">Views&dagger; MVC apps, if views aren't precompiled</span></span></li><li><span data-ttu-id="400db-124">Pages&dagger; в приложениях MVC или Razor Pages, если страницы не компилируются заранее</span><span class="sxs-lookup"><span data-stu-id="400db-124">Pages&dagger; MVC or Razor Pages apps, if pages aren't precompiled</span></span></li><li><span data-ttu-id="400db-125">wwwroot&dagger;</span><span class="sxs-lookup"><span data-stu-id="400db-125">wwwroot&dagger;</span></span></li><li><span data-ttu-id="400db-126">DLL-файлы</span><span class="sxs-lookup"><span data-stu-id="400db-126">\*.dll files</span></span></li><li><span data-ttu-id="400db-127">{имя_сборки}.deps.json</span><span class="sxs-lookup"><span data-stu-id="400db-127">{ASSEMBLY NAME}.deps.json</span></span></li><li><span data-ttu-id="400db-128">{имя_сборки}.dll</span><span class="sxs-lookup"><span data-stu-id="400db-128">{ASSEMBLY NAME}.dll</span></span></li><li><span data-ttu-id="400db-129">{имя_сборки}.exe</span><span class="sxs-lookup"><span data-stu-id="400db-129">{ASSEMBLY NAME}.exe</span></span></li><li><span data-ttu-id="400db-130">{имя_сборки}.pdb</span><span class="sxs-lookup"><span data-stu-id="400db-130">{ASSEMBLY NAME}.pdb</span></span></li><li><span data-ttu-id="400db-131">{имя_сборки}.Views.dll</span><span class="sxs-lookup"><span data-stu-id="400db-131">{ASSEMBLY NAME}.Views.dll</span></span></li><li><span data-ttu-id="400db-132">{имя_сборки}.Views.pdb</span><span class="sxs-lookup"><span data-stu-id="400db-132">{ASSEMBLY NAME}.Views.pdb</span></span></li><li><span data-ttu-id="400db-133">{имя_сборки}.runtimeconfig.json</span><span class="sxs-lookup"><span data-stu-id="400db-133">{ASSEMBLY NAME}.runtimeconfig.json</span></span></li><li><span data-ttu-id="400db-134">web.config (в развертываниях IIS)</span><span class="sxs-lookup"><span data-stu-id="400db-134">web.config (IIS deployments)</span></span></li></ul></li></ul> |

<span data-ttu-id="400db-135">&dagger;Обозначает каталог</span><span class="sxs-lookup"><span data-stu-id="400db-135">&dagger;Indicates a directory</span></span>

<span data-ttu-id="400db-136">Каталог *публикации* представляет *корневой путь содержимого* для развертывания, который также называется *путь к базовой папке приложения*.</span><span class="sxs-lookup"><span data-stu-id="400db-136">The *publish* directory represents the *content root path*, also called the *application base path*, of the deployment.</span></span> <span data-ttu-id="400db-137">Независимо от того, какое имя присвоено каталогу *публикации*, развернутого на сервере приложения, именно это расположение обозначает физический путь к размещенному приложению на этом сервере.</span><span class="sxs-lookup"><span data-stu-id="400db-137">Whatever name is given to the *publish* directory of the deployed app on the server, its location serves as the server's physical path to the hosted app.</span></span>

<span data-ttu-id="400db-138">Каталог *wwwroot*, если таковой имеется, содержит только статические активы.</span><span class="sxs-lookup"><span data-stu-id="400db-138">The *wwwroot* directory, if present, only contains static assets.</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="400db-139">Создание папки *Logs* полезно для [ведения расширенного журнала отладки модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span><span class="sxs-lookup"><span data-stu-id="400db-139">Creating a *Logs* folder is useful for [ASP.NET Core Module enhanced debug logging](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="400db-140">Папки, указанные в пути к значению `<handlerSetting>`, не создаются автоматически и должны заранее существовать в развертывании, чтобы модуль мог осуществлять запись в журнал отладки.</span><span class="sxs-lookup"><span data-stu-id="400db-140">Folders in the path provided to the `<handlerSetting>` value aren't created by the module automatically and should pre-exist in the deployment to allow the module to write the debug log.</span></span>

<span data-ttu-id="400db-141">Каталог *Logs* для развертывания можно создать одним из двух указанных далее методов.</span><span class="sxs-lookup"><span data-stu-id="400db-141">A *Logs* directory can be created for the deployment using one of the following two approaches:</span></span>

* <span data-ttu-id="400db-142">Добавьте в файл проекта элемент `<Target>` следующего содержания:</span><span class="sxs-lookup"><span data-stu-id="400db-142">Add the following `<Target>` element to the project file:</span></span>

   ```xml
   <Target Name="CreateLogsFolder" AfterTargets="Publish">
     <MakeDir Directories="$(PublishDir)Logs" 
              Condition="!Exists('$(PublishDir)Logs')" />
     <WriteLinesToFile File="$(PublishDir)Logs\.log" 
                       Lines="Generated file" 
                       Overwrite="True" 
                       Condition="!Exists('$(PublishDir)Logs\.log')" />
   </Target>
   ```

   <span data-ttu-id="400db-143">Элемент `<MakeDir>` создает пустую папку *Logs* в публикуемых выходных данных.</span><span class="sxs-lookup"><span data-stu-id="400db-143">The `<MakeDir>` element creates an empty *Logs* folder in the published output.</span></span> <span data-ttu-id="400db-144">Этот элемент использует свойство `PublishDir`, чтобы определить целевое расположение для создания папки.</span><span class="sxs-lookup"><span data-stu-id="400db-144">The element uses the `PublishDir` property to determine the target location for creating the folder.</span></span> <span data-ttu-id="400db-145">Несколько методов развертывания, например веб-развертывание, пропускают пустые папки во время развертывания.</span><span class="sxs-lookup"><span data-stu-id="400db-145">Several deployment methods, such as Web Deploy, skip empty folders during deployment.</span></span> <span data-ttu-id="400db-146">Элемент `<WriteLinesToFile>` создает файл в папке *Logs*, чтобы гарантировать ее развертывание на целевом сервере.</span><span class="sxs-lookup"><span data-stu-id="400db-146">The `<WriteLinesToFile>` element generates a file in the *Logs* folder, which guarantees deployment of the folder to the server.</span></span> <span data-ttu-id="400db-147">Создание папки с помощью этого подхода завершается ошибкой, если рабочий процесс не имеет прав на запись в целевую папку.</span><span class="sxs-lookup"><span data-stu-id="400db-147">Folder creation using this approach fails if the worker process doesn't have write access to the target folder.</span></span>

* <span data-ttu-id="400db-148">Самостоятельно создайте физическую папку *Logs* на сервере в каталоге развертывания.</span><span class="sxs-lookup"><span data-stu-id="400db-148">Physically create the *Logs* directory on the server in the deployment.</span></span>

<span data-ttu-id="400db-149">Для каталога развертывания нужны права на чтение и выполнение.</span><span class="sxs-lookup"><span data-stu-id="400db-149">The deployment directory requires Read/Execute permissions.</span></span> <span data-ttu-id="400db-150">Для каталога *Logs* нужны права на чтение и запись.</span><span class="sxs-lookup"><span data-stu-id="400db-150">The *Logs* directory requires Read/Write permissions.</span></span> <span data-ttu-id="400db-151">Для дополнительных каталогов, в которые записываются файлы, нужны права на чтение и запись.</span><span class="sxs-lookup"><span data-stu-id="400db-151">Additional directories where files are written require Read/Write permissions.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="400db-152">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="400db-152">Additional resources</span></span>

* [<span data-ttu-id="400db-153">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="400db-153">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* [<span data-ttu-id="400db-154">Развертывание приложений .NET Core</span><span class="sxs-lookup"><span data-stu-id="400db-154">.NET Core application deployment</span></span>](/dotnet/core/deploying/)
* [<span data-ttu-id="400db-155">Целевые платформы</span><span class="sxs-lookup"><span data-stu-id="400db-155">Target frameworks</span></span>](/dotnet/standard/frameworks)
* [<span data-ttu-id="400db-156">Каталог относительных идентификаторов в .NET Core</span><span class="sxs-lookup"><span data-stu-id="400db-156">.NET Core RID Catalog</span></span>](/dotnet/core/rid-catalog)
