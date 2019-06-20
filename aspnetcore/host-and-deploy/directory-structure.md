---
title: Структура каталогов ASP.NET Core
author: guardrex
description: Сведения о структуре каталогов опубликованных приложений ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 06/17/2019
uid: host-and-deploy/directory-structure
ms.openlocfilehash: f1df047decc7a0a6b7dcee57a690c55eea428b05
ms.sourcegitcommit: 28a2874765cefe9eaa068dceb989a978ba2096aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/17/2019
ms.locfileid: "67166970"
---
# <a name="aspnet-core-directory-structure"></a><span data-ttu-id="90add-103">Структура каталогов ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="90add-103">ASP.NET Core directory structure</span></span>

<span data-ttu-id="90add-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="90add-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="90add-105">Каталог *публикации* содержит развертываемые ресурсы приложения, созданные командной [dotnet publish](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="90add-105">The *publish* directory contains the app's deployable assets produced by the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="90add-106">Каталог содержит следующее.</span><span class="sxs-lookup"><span data-stu-id="90add-106">The directory contains:</span></span>

* <span data-ttu-id="90add-107">Файлы приложения.</span><span class="sxs-lookup"><span data-stu-id="90add-107">Application files</span></span>
* <span data-ttu-id="90add-108">Файлы конфигурации.</span><span class="sxs-lookup"><span data-stu-id="90add-108">Configuration files</span></span>
* <span data-ttu-id="90add-109">Статические ресурсы.</span><span class="sxs-lookup"><span data-stu-id="90add-109">Static assets</span></span>
* <span data-ttu-id="90add-110">Пакеты</span><span class="sxs-lookup"><span data-stu-id="90add-110">Packages</span></span>
* <span data-ttu-id="90add-111">Среда выполнения ([только автономное развертывание](/dotnet/core/deploying/#self-contained-deployments-scd))</span><span class="sxs-lookup"><span data-stu-id="90add-111">A runtime ([self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) only)</span></span>

| <span data-ttu-id="90add-112">Тип приложения</span><span class="sxs-lookup"><span data-stu-id="90add-112">App Type</span></span> | <span data-ttu-id="90add-113">Структура каталогов</span><span class="sxs-lookup"><span data-stu-id="90add-113">Directory Structure</span></span> |
| -------- | ------------------- |
| [<span data-ttu-id="90add-114">Развертывание, зависящее от платформы</span><span class="sxs-lookup"><span data-stu-id="90add-114">Framework-dependent Deployment</span></span>](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li><span data-ttu-id="90add-115">publish&dagger;</span><span class="sxs-lookup"><span data-stu-id="90add-115">publish&dagger;</span></span><ul><li><span data-ttu-id="90add-116">Views&dagger; (в приложениях MVC, если представления не компилируются заранее)</span><span class="sxs-lookup"><span data-stu-id="90add-116">Views&dagger; (MVC apps; if views aren't precompiled)</span></span></li><li><span data-ttu-id="90add-117">Pages&dagger; (в приложениях MVC или Razor Pages, если страницы не компилируются заранее)</span><span class="sxs-lookup"><span data-stu-id="90add-117">Pages&dagger; (MVC or Razor Pages apps; if pages aren't precompiled)</span></span></li><li><span data-ttu-id="90add-118">wwwroot&dagger;</span><span class="sxs-lookup"><span data-stu-id="90add-118">wwwroot&dagger;</span></span></li><li><span data-ttu-id="90add-119">\*\.DLL-файлы</span><span class="sxs-lookup"><span data-stu-id="90add-119">\*\.dll files</span></span></li><li><span data-ttu-id="90add-120">{имя_сборки}.deps.json</span><span class="sxs-lookup"><span data-stu-id="90add-120">{ASSEMBLY NAME}.deps.json</span></span></li><li><span data-ttu-id="90add-121">{имя_сборки}.dll</span><span class="sxs-lookup"><span data-stu-id="90add-121">{ASSEMBLY NAME}.dll</span></span></li><li><span data-ttu-id="90add-122">{имя_сборки}.pdb</span><span class="sxs-lookup"><span data-stu-id="90add-122">{ASSEMBLY NAME}.pdb</span></span></li><li><span data-ttu-id="90add-123">{имя_сборки}.Views.dll</span><span class="sxs-lookup"><span data-stu-id="90add-123">{ASSEMBLY NAME}.Views.dll</span></span></li><li><span data-ttu-id="90add-124">{имя_сборки}.Views.pdb</span><span class="sxs-lookup"><span data-stu-id="90add-124">{ASSEMBLY NAME}.Views.pdb</span></span></li><li><span data-ttu-id="90add-125">{имя_сборки}.runtimeconfig.json</span><span class="sxs-lookup"><span data-stu-id="90add-125">{ASSEMBLY NAME}.runtimeconfig.json</span></span></li><li><span data-ttu-id="90add-126">web.config (в развертываниях IIS)</span><span class="sxs-lookup"><span data-stu-id="90add-126">web.config (IIS deployments)</span></span></li></ul></li></ul> |
| [<span data-ttu-id="90add-127">Автономное развертывание</span><span class="sxs-lookup"><span data-stu-id="90add-127">Self-contained Deployment</span></span>](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li><span data-ttu-id="90add-128">publish&dagger;</span><span class="sxs-lookup"><span data-stu-id="90add-128">publish&dagger;</span></span><ul><li><span data-ttu-id="90add-129">Views&dagger; (в приложениях MVC, если представления не компилируются заранее)</span><span class="sxs-lookup"><span data-stu-id="90add-129">Views&dagger; (MVC apps; if views aren't precompiled)</span></span></li><li><span data-ttu-id="90add-130">Pages&dagger; (в приложениях MVC или Razor Pages, если страницы не компилируются заранее)</span><span class="sxs-lookup"><span data-stu-id="90add-130">Pages&dagger; (MVC or Razor Pages apps; if pages aren't precompiled)</span></span></li><li><span data-ttu-id="90add-131">wwwroot&dagger;</span><span class="sxs-lookup"><span data-stu-id="90add-131">wwwroot&dagger;</span></span></li><li><span data-ttu-id="90add-132">\*DLL-файлы</span><span class="sxs-lookup"><span data-stu-id="90add-132">\*.dll files</span></span></li><li><span data-ttu-id="90add-133">{имя_сборки}.deps.json</span><span class="sxs-lookup"><span data-stu-id="90add-133">{ASSEMBLY NAME}.deps.json</span></span></li><li><span data-ttu-id="90add-134">{имя_сборки}.dll</span><span class="sxs-lookup"><span data-stu-id="90add-134">{ASSEMBLY NAME}.dll</span></span></li><li><span data-ttu-id="90add-135">{имя_сборки}.exe</span><span class="sxs-lookup"><span data-stu-id="90add-135">{ASSEMBLY NAME}.exe</span></span></li><li><span data-ttu-id="90add-136">{имя_сборки}.pdb</span><span class="sxs-lookup"><span data-stu-id="90add-136">{ASSEMBLY NAME}.pdb</span></span></li><li><span data-ttu-id="90add-137">{имя_сборки}.Views.dll</span><span class="sxs-lookup"><span data-stu-id="90add-137">{ASSEMBLY NAME}.Views.dll</span></span></li><li><span data-ttu-id="90add-138">{имя_сборки}.Views.pdb</span><span class="sxs-lookup"><span data-stu-id="90add-138">{ASSEMBLY NAME}.Views.pdb</span></span></li><li><span data-ttu-id="90add-139">{имя_сборки}.runtimeconfig.json</span><span class="sxs-lookup"><span data-stu-id="90add-139">{ASSEMBLY NAME}.runtimeconfig.json</span></span></li><li><span data-ttu-id="90add-140">web.config (в развертываниях IIS)</span><span class="sxs-lookup"><span data-stu-id="90add-140">web.config (IIS deployments)</span></span></li></ul></li></ul> |

<span data-ttu-id="90add-141">&dagger;Обозначает каталог</span><span class="sxs-lookup"><span data-stu-id="90add-141">&dagger;Indicates a directory</span></span>

<span data-ttu-id="90add-142">Каталог *публикации* представляет *корневой путь содержимого* для развертывания, который также называется *путь к базовой папке приложения*.</span><span class="sxs-lookup"><span data-stu-id="90add-142">The *publish* directory represents the *content root path*, also called the *application base path*, of the deployment.</span></span> <span data-ttu-id="90add-143">Независимо от того, какое имя присвоено каталогу *публикации*, развернутого на сервере приложения, именно это расположение обозначает физический путь к размещенному приложению на этом сервере.</span><span class="sxs-lookup"><span data-stu-id="90add-143">Whatever name is given to the *publish* directory of the deployed app on the server, its location serves as the server's physical path to the hosted app.</span></span>

<span data-ttu-id="90add-144">Каталог *wwwroot*, если таковой имеется, содержит только статические активы.</span><span class="sxs-lookup"><span data-stu-id="90add-144">The *wwwroot* directory, if present, only contains static assets.</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="90add-145">Создание папки *Logs* полезно для [ведения расширенного журнала отладки модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span><span class="sxs-lookup"><span data-stu-id="90add-145">Creating a *Logs* folder is useful for [ASP.NET Core Module enhanced debug logging](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="90add-146">Папки, указанные в пути к значению `<handlerSetting>`, не создаются автоматически и должны заранее существовать в развертывании, чтобы модуль мог осуществлять запись в журнал отладки.</span><span class="sxs-lookup"><span data-stu-id="90add-146">Folders in the path provided to the `<handlerSetting>` value aren't created by the module automatically and should pre-exist in the deployment to allow the module to write the debug log.</span></span>

<span data-ttu-id="90add-147">Каталог *Logs* для развертывания можно создать одним из двух указанных далее методов.</span><span class="sxs-lookup"><span data-stu-id="90add-147">A *Logs* directory can be created for the deployment using one of the following two approaches:</span></span>

* <span data-ttu-id="90add-148">Добавьте в файл проекта элемент `<Target>` следующего содержания:</span><span class="sxs-lookup"><span data-stu-id="90add-148">Add the following `<Target>` element to the project file:</span></span>

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

   <span data-ttu-id="90add-149">Элемент `<MakeDir>` создает пустую папку *Logs* в публикуемых выходных данных.</span><span class="sxs-lookup"><span data-stu-id="90add-149">The `<MakeDir>` element creates an empty *Logs* folder in the published output.</span></span> <span data-ttu-id="90add-150">Этот элемент использует свойство `PublishDir`, чтобы определить целевое расположение для создания папки.</span><span class="sxs-lookup"><span data-stu-id="90add-150">The element uses the `PublishDir` property to determine the target location for creating the folder.</span></span> <span data-ttu-id="90add-151">Несколько методов развертывания, например веб-развертывание, пропускают пустые папки во время развертывания.</span><span class="sxs-lookup"><span data-stu-id="90add-151">Several deployment methods, such as Web Deploy, skip empty folders during deployment.</span></span> <span data-ttu-id="90add-152">Элемент `<WriteLinesToFile>` создает файл в папке *Logs*, чтобы гарантировать ее развертывание на целевом сервере.</span><span class="sxs-lookup"><span data-stu-id="90add-152">The `<WriteLinesToFile>` element generates a file in the *Logs* folder, which guarantees deployment of the folder to the server.</span></span> <span data-ttu-id="90add-153">Создание папки с помощью этого подхода завершается ошибкой, если рабочий процесс не имеет прав на запись в целевую папку.</span><span class="sxs-lookup"><span data-stu-id="90add-153">Folder creation using this approach fails if the worker process doesn't have write access to the target folder.</span></span>

* <span data-ttu-id="90add-154">Самостоятельно создайте физическую папку *Logs* на сервере в каталоге развертывания.</span><span class="sxs-lookup"><span data-stu-id="90add-154">Physically create the *Logs* directory on the server in the deployment.</span></span>

<span data-ttu-id="90add-155">Для каталога развертывания нужны права на чтение и выполнение.</span><span class="sxs-lookup"><span data-stu-id="90add-155">The deployment directory requires Read/Execute permissions.</span></span> <span data-ttu-id="90add-156">Для каталога *Logs* нужны права на чтение и запись.</span><span class="sxs-lookup"><span data-stu-id="90add-156">The *Logs* directory requires Read/Write permissions.</span></span> <span data-ttu-id="90add-157">Для дополнительных каталогов, в которые записываются файлы, нужны права на чтение и запись.</span><span class="sxs-lookup"><span data-stu-id="90add-157">Additional directories where files are written require Read/Write permissions.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="90add-158">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="90add-158">Additional resources</span></span>

* [<span data-ttu-id="90add-159">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="90add-159">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* [<span data-ttu-id="90add-160">Развертывание приложений .NET Core</span><span class="sxs-lookup"><span data-stu-id="90add-160">.NET Core application deployment</span></span>](/dotnet/core/deploying/)
* [<span data-ttu-id="90add-161">Целевые платформы</span><span class="sxs-lookup"><span data-stu-id="90add-161">Target frameworks</span></span>](/dotnet/standard/frameworks)
* [<span data-ttu-id="90add-162">Каталог относительных идентификаторов в .NET Core</span><span class="sxs-lookup"><span data-stu-id="90add-162">.NET Core RID Catalog</span></span>](/dotnet/core/rid-catalog)
