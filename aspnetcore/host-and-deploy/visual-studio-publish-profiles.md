---
title: "Visual Studio профилей публикации для развертывания приложения ASP.NET Core"
author: rick-anderson
description: "Узнайте, как для создания профилей публикации для приложения ASP.NET Core в Visual Studio."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 09/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 1f403447c39db4ebfe3dafda591602f0dc9db8c3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a>Visual Studio профилей публикации для развертывания приложения ASP.NET Core

Авторы: [Саид Ибрагим Хашими](https://github.com/sayedihashimi) (Sayed Ibrahim Hashimi) и [Рик Андерсон](https://twitter.com/RickAndMSFT)

В этой статье рассказывается об использовании Visual Studio 2017 для создания профилей публикации. Профили публикации, созданные с помощью Visual Studio, можно применять в MSBuild и Visual Studio 2017. Статья содержит сведения о процессе публикации. Инструкции по публикации в Azure см. в статье [Публикация веб-приложения ASP.NET Core в службе приложений Azure с помощью Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs).

Представленный ниже файл *.csproj* был создан с помощью команды `dotnet new mvc`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
  </ItemGroup>

  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
  </ItemGroup>

</Project>
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.5" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.6" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.3" />
  </ItemGroup>

</Project>
```

---

Атрибут `Sdk` в элементе `<Project>` (первая строка) показанной выше разметки выполняет следующие действия.

* Импортирует файл свойств из *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* в начале.
* Импортирует целевой файл из *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* в конец.

По умолчанию `MSBuildSDKsPath` (с Visual Studio 2017 Enterprise) находится в папке *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.

`Microsoft.NET.Sdk.Web` зависит от следующих компонентов.

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

Что вызывает следующие свойства и целевые объекты для импорта:

* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets

Опубликуйте право набор целевых объектов в зависимости от способа публикации используется Импорт целевых объектов.

Когда MSBuild или Visual Studio загружает проект, на высоком уровне выполняются следующие действия:

* сборка проекта;
* вычисление файлов для публикации;
* публикация файлов в месте назначения;

### <a name="compute-project-items"></a>вычисление элементов проекта.

После загрузки проекта вычисляются элементы проекта (файлы). Порядок обработки файла определяется атрибутом `item type`. По умолчанию файлы с расширением *.cs* включаются в список элементов `Compile`. Файлы в списке элементов `Compile` компилируются.

`Content` Список элементов содержит файлы, которые публикуются в дополнение к выходных данных сборки. По умолчанию файлы, соответствующие шаблону `wwwroot/**` включаются в `Content` элемента. [wwwroot /\* \* -это шаблон глобализацию](https://gruntjs.com/configuring-tasks#globbing-patterns) указывает все файлы в *wwwroot* папки **и** вложенные папки. Чтобы напрямую добавить в список публикации отдельный файл, можно добавить этот файл в файл *CSPROJ*, как показано в разделе [Включение файлов](#including-files).

При выборе **публикации** кнопки в Visual Studio или при публикации из командной строки:

* Вычисляются свойства и (или) элементы (файлы, требующие сборки).
* Только в Visual Studio: восстанавливаются пакеты NuGet. (Восстановление выполняется пользователем в интерфейсе командной строки.)
* Выполняется сборка проекта.
* Вычисляются публикуемые элементы (файлы, требующие публикации).
* Проект публикуется. (Вычисляемые файлы копируются в место назначения публикации.)

Когда ссылается на проект ASP.NET Core `Microsoft.NET.Sdk.Web` в файле проекта *app_offline.htm* файл помещается в корне каталога веб-приложения. Если файл присутствует, модуль ASP.NET Core корректно завершает работу приложения и обслуживает файл *app_offline.htm* во время развертывания. Дополнительные сведения см. в разделе [Справочник по конфигурации модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module#appofflinehtm).

## <a name="basic-command-line-publishing"></a>Основные публикации командной строки

Публикация командной строки работает на всех платформах .NET Core поддерживается и не требует Visual Studio. В приведенных ниже примерах команда `dotnet publish` выполняется из папки проекта (содержащей файл *.csproj*). В противном случае в папке проекта явным образом передать в путь к файлу проекта. Пример:

```console
dotnet publish c:/webs/web1
```

Для создания и публикации веб-приложения выполните следующие команды.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

Команда `dotnet publish` выдает результаты следующего вида.

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

Папка для публикации по умолчанию — `bin\$(Configuration)\netcoreapp<version>\publish`. Значение параметра `$(Configuration)` по умолчанию — Debug (Отладка). В приведенном выше примере `<TargetFramework>` — это `netcoreapp2.0`.

`dotnet publish -h` отображает справку по публикации.

Следующая команда определяет сборку `Release` и папку для публикации.

```console
dotnet publish -c Release -o C:/MyWebs/test
```

`dotnet publish` Команда вызывает MSBuild, который вызывает `Publish` цели. Любые параметры, передаваемые в `dotnet publish` передаются в MSBuild. Параметр `-c` сопоставляется со свойством `Configuration` MSBuild. Параметр `-o` сопоставляется со свойством `OutputPath`.

Свойства MSBuild могут передаваться с помощью любого из следующих форматов:

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

Следующая команда публикует сборку `Release` в общую сетевую папку.

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

Общая сетевая папка указывается с помощью символов косой черты (*//r8/*) и работает на всех поддерживаемых платформах .NET Core.

Убедитесь, что приложение, публикуемое для развертывания, не запущено. Во время выполнения приложения файлы в папке *publish* блокируются. Заблокированные файлы скопировать нельзя, так что развертывание в этом случае не произойдет.

## <a name="publish-profiles"></a>Профили публикации

В этом разделе для создания профилей публикации используется Visual Studio 2017 и более поздних версий. После создания публикации из Visual Studio или в командной строке доступна.

Профили публикации позволяют упростить процесс публикации. Несколько профилей публикации может существовать. Чтобы создать профиль публикации в Visual Studio, щелкните правой кнопкой мыши проект в обозревателе решений и выберите **публикации**. Можно также выбрать **публикации \<имя проекта >** из меню "Построение". Откроется вкладка **Публикации** на странице свойств приложения. Если проект не содержит профиль публикации, откроется следующая страница.

![Выбрана вкладка публикации страницы емкости приложения отображение Azure, IIS, FTB, папка с Azure. Здесь же представлены флажки, позволяющие создать новый профиль или выбрать уже существующий.](visual-studio-publish-profiles/_static/az.png)

Если выбран раздел **Папка**, кнопка **Опубликовать** создает профиль публикации в папку и выполняет публикацию.

![Вкладка **Публикация** на странице свойств приложения с разделами "Azure", "IIS, FTB" и "Папка"](visual-studio-publish-profiles/_static/pub1.png)

После создания профиля публикации **публикации** изменений и выберите **Создание нового профиля** для создания нового профиля.

![Вкладка **Публикация** на странице свойств приложения с созданным на предыдущем шаге профилем FolderProfile и ссылкой "Создать профиль".](visual-studio-publish-profiles/_static/create_new.png)

Мастер публикации поддерживает следующие целевые объекты публикации.

* Служба приложений Microsoft Azure
* IIS, FTP и т. д (для любого веб-сервера)
* Папка
* Импорт профиля (допускает Импорт профиля).
* Виртуальные машины Microsoft Azure

Дополнительные сведения см. в статье [Выбор подходящих вариантов публикации](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options).

При создании профиля публикации с помощью Visual Studio *свойства/PublishProfiles/\<имя публикации > .pubxml* создается файл MSBuild. Это файл с расширением *.pubxml* представляет собой файл MSBuild, содержащий настройки конфигурации публикации. Этот файл можно изменить для настройки процесса построения и публикации процесс. Этот файл считывается процессом публикации. `<LastUsedBuildConfiguration>`является специальной, так как он является глобальным и не должны быть в любом файле, который импортируется в сборке. Дополнительные сведения см. в статье [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) (MSBuild: настройка свойства конфигурации).
*.Pubxml* файл не следует возвращен в систему управления версиями, так как он зависит от *.user* файла. Файл *.user* ни в коем случае не должен возвращаться в систему управления версиями, так как может содержать конфиденциальные сведения и быть действительным только для одного пользователя и компьютера.

Конфиденциальные данные (такие как пароль публикации) шифруются на уровне пользователя или компьютера и хранятся в файле *Properties/PublishProfiles/\<имя публикации>.pubxml.user*. Так как этот файл может содержать конфиденциальные данные, он **не** должен возвращаться в систему управления версиями.

Общие сведения о том, как опубликовать веб-приложения ASP.NET Core. в разделе [узла и развернуть](index.md). [Размещать и развертывать](index.md) является проекта с открытым кодом на https://github.com/aspnet/websdk.

`dotnet publish`можно использовать папку MSDeploy, и [KUDU](https://github.com/projectkudu/kudu/wiki) профили публикации:
 
Папка (кросс-платформенное):`dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`

MSDeploy (в настоящее время работает только в windows, поскольку MSDeploy не кросс платформенных):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`

MSDeploy пакета (в настоящее время работает только в windows, поскольку MSDeploy не кросс платформенных):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`

В примерах выше **не** передачи `deployonbuild` для `dotnet publish`.

Дополнительные сведения см. в разделе [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).

`dotnet publish` поддерживает API KUDU для публикации в Azure с любой платформы. Visual Studio опубликовать поддерживает KUDU API-интерфейсов, но поддерживается websdk для перекрестного plat публикации в Azure.

Добавьте профиль публикации в папку *Properties/PublishProfiles* со следующим содержимым:

```xml
<Project>
  <PropertyGroup>
    <PublishProtocol>Kudu</PublishProtocol>
    <PublishSiteName>nodewebapp</PublishSiteName>
    <UserName>username</UserName>
    <Password>password</Password>
  </PropertyGroup>
</Project>
```

Выполните следующую команду моментально настройки публикации содержимого и его публикация в Azure с помощью API-интерфейсы KUDU:

`dotnet publish /p:PublishProfile=Azure /p:Configuration=Release`

Для работы с профилем публикации задайте следующие свойства MSBuild.

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

При публикации с использованием профиля с именем *FolderProfile*, можно выполнить одну из приведенных ниже команд:

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

При вызове `dotnet build`, он вызывает `msbuild` для выполнения сборки и процесса публикации. Вызов `dotnet build` или `msbuild` эквивалентно по существу при передаче в папке профиля. При вызове MSBuild непосредственно в Windows, .NET Framework версии MSBuild используется. В настоящее время MSDeploy поддерживает публикацию только на компьютерах Windows. Вызов `dotnet build` в других профилях, кроме профиля папки, вызывает MSBuild, который использует в таких профилях MSDeploy. Вызов `dotnet build` в других профилях, кроме профиля папки, вызывает MSBuild (с помощью MSDeploy) и завершается сбоем (даже при работе на платформе Windows). Для публикации профилей, отличных от профиля папки, вызывайте MSBuild напрямую.

Следующий профиль публикации папки был создан в Visual Studio и публикуется в общей сетевой папке:

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework>netcoreapp1.1</PublishFramework>
    <ProjectGuid>c30c453c-312e-40c4-aec9-394a145dee0b</ProjectGuid>
    <publishUrl>\\r8\Release\AdminWeb</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
</Project>
```

Обратите внимание на то, что параметр `<LastUsedBuildConfiguration>` имеет значение `Release`. При публикации из Visual Studio свойство конфигурации `<LastUsedBuildConfiguration>` получает значение, которые использовалось при запуске процесса публикации. `<LastUsedBuildConfiguration>` Свойство конфигурации является особенным и не должен переопределяться из импортированного файла MSBuild. Это свойство можно переопределить с помощью командной строки. Пример:

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

С использованием MSBuild

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>Публикация конечной точки MSDeploy из командной строки

Как упоминалось ранее, публикация может выполняться с помощью `dotnet publish` или `msbuild` команды. Команда `dotnet publish` выполняется в контексте .NET Core. `msbuild` Команды требуется платформа .NET framework и поэтому ограничена среды Windows.

Простейший способ публикации с помощью MSDeploy — это сначала создать профиль публикации в Visual Studio 2017, а затем применить этот профиль из командной строки.

В следующем примере создается веб-приложение ASP.NET Core (с помощью `dotnet new mvc`), и профиль публикации Azure добавляется с помощью Visual Studio.

Запустите `msbuild` из **Командная строка разработчика для VS 2017 г**. Командная строка разработчика содержит правильные *msbuild.exe* в пути с некоторым набором переменных MSBuild.

MSBuild использует следующий синтаксис.

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>`

Получить `Password` из  *\<имя публикации >. PublishSettings* файл. Загрузить *. PublishSettings* файл либо из:

* Обозреватель решений: Щелкните правой кнопкой мыши на веб-приложения и выберите **загрузить профиль публикации**.
* Портал управления Azure: Выберите **получение профиля публикации** из колонки веб-приложения.

`Username` можно найти в профиле публикации.

В следующем примере используется профиль публикации "Web11112 - Web Deploy".

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a>Исключение файлов

При публикации веб-приложений ASP.NET Core включаются артефакты сборки и содержимое папки *wwwroot*. `msbuild` поддерживает [стандартные маски](https://gruntjs.com/configuring-tasks#globbing-patterns). Например, следующая `<Content>` элемент разметки исключает весь текст (*.txt*) файлов из *wwwroot/content* и всех ее вложенных папок.

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Показанная выше разметка может быть добавлена в профиль публикации или в файл *.csproj*. При добавлении в файл *.csproj* правило добавляется во все профили публикации в проекте.

Представленная ниже разметка элемента `<MsDeploySkipRules>` исключает все файлы из папки *wwwroot/content*.

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>`не удалять *пропустить* целевых объектов с сайта развертывания. `<Content>`целевые файлы и папки будут удалены с сайта развертывания. Например предположим, что развернутого веб-приложения имели следующие файлы:

* *Views/Home/About1.cshtml*
* *Views/Home/About2.cshtml*
* *Views/Home/About3.cshtml*

Если следующие `<MsDeploySkipRules>` добавляется разметки, эти файлы не удалены на сайте развертывания.

``` xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About1.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About2.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About3.cshtml</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` Предотвращает разметки, показанный выше *пропущен* файлы из процесса depoyed, но не удаляет эти файлы, как только они развернуты.

Следующие `<Content>` разметки удаляет целевых файлов на сайте развертывания:

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

С помощью развертывания из командной строки с `<Content>` разметки выше результатов в выходные данные, аналогичные следующим:

``` console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\netcoreapp1.1\PubTmp\Web1.SourceManifest.
  xml) to Destination: auto().
  Deleting file (Web11112\Views\Home\About1.cshtml).
  Deleting file (Web11112\Views\Home\About2.cshtml).
  Deleting file (Web11112\Views\Home\About3.cshtml).
  Updating file (Web11112\web.config).
  Updating file (Web11112\Web1.deps.json).
  Updating file (Web11112\Web1.dll).
  Updating file (Web11112\Web1.pdb).
  Updating file (Web11112\Web1.runtimeconfig.json).
  Successfully executed Web deployment task.
  Publish Succeeded.
Done Building Project "C:\Webs\Web1\Web1.csproj" (default targets).
```

## <a name="including-files"></a>Включение файлов

Включает следующую разметку *изображения* папку вне каталога проекта к *wwwroot/images* папку сайта публикации:

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

Показанная выше разметка может быть добавлена в файл *.csproj* или в профиль публикации. Если добавляется в *.csproj* файл, он включен в каждый профиль публикации в проекте.

Разметка, выделенная в приведенном ниже примере, показывает, как.

* Скопировать файл из папки, не связанной с проектом, в папку *wwwroot*.
* Исключить папку *wwwroot\Content*.
* Исключить файл *Views\Home\About2.cshtml*.

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework />
    <ProjectGuid>afa9f185-7ce0-4935-9da1-ab676229d68a</ProjectGuid>
    <publishUrl>bin\Release\PublishOutput</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
  <ItemGroup>
    <ResolvedFileToPublish Include="..\ReadMe2.MD">
      <RelativePath>wwwroot\ReadMe2.MD</RelativePath>
    </ResolvedFileToPublish>

    <Content Update="wwwroot\Content\**\*" CopyToPublishDirectory="Never" />
    <Content Update="Views\Home\About2.cshtml" CopyToPublishDirectory="Never" />

  </ItemGroup>
</Project>
```

Дополнительные примеры развертывания см. в файле [WebSDK Readme](https://github.com/aspnet/websdk).

### <a name="run-a-target-before-or-after-publishing"></a>Выполнение целевого объекта до или после публикации

Встроенная `BeforePublish` и `AfterPublish` целевых объектов можно использовать для выполнения целевого объекта до или после назначения публикации. В профиль публикации можно добавить показанную ниже разметку — в этом случае сообщения будут добавлены в вывод консоли до или после публикации.

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a>Служба Kudu

Чтобы просмотреть файлы в службе приложений Azure развертывание веб-приложения, используйте [Kudu службы](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service). Добавление `scm` маркера к имени веб-приложения. Пример:

| URL-адрес                                    | Результат      |
| -------------------------------------- | ----------- |
| `http://mysite.azurewebsites.net/`     | Веб-приложение     |
| `http://mysite.scm.azurewebsites.net/` | Служба Kudu |

Для просмотра, редактирования, удаления и добавления файлов выберите в меню пункт [Консоль отладки](https://github.com/projectkudu/kudu/wiki/Kudu-console).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Веб-развертывание](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) упрощает развертывание веб-приложений и веб-сайтов на серверах IIS.
* [https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): проблемы с файлами и особенности запросов для развертывания.
