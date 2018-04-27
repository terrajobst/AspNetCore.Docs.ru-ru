---
title: Visual Studio профилей публикации для развертывания приложения ASP.NET Core
author: rick-anderson
description: Сведения о создании профилей публикации в Visual Studio и использовать их для управления ASP.NET Core развертывания приложений на различных целевых объектов.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 3dc858793cd4ddb2630d05a5084f4b7caeaa30eb
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a>Visual Studio профилей публикации для развертывания приложения ASP.NET Core

Авторы: [Саид Ибрагим Хашими](https://github.com/sayedihashimi) (Sayed Ibrahim Hashimi) и [Рик Андерсон](https://twitter.com/RickAndMSFT)

В этом документе описывается использование Visual Studio 2017 г. Создание и использование профилей публикации. Профили публикации, созданные с помощью Visual Studio, можно применять в MSBuild и Visual Studio 2017. Инструкции по публикации в Azure см. в статье [Публикация веб-приложения ASP.NET Core в службе приложений Azure с помощью Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs).

Следующий файл проекта был создан с помощью команды `dotnet new mvc`:

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

`<Project>` Элемента `Sdk` атрибута выполняет следующие задачи:

* Импортирует файл свойств из *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* в начале.
* Импортирует целевой файл из *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* в конец.

По умолчанию `MSBuildSDKsPath` (с Visual Studio 2017 Enterprise) находится в папке *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.

`Microsoft.NET.Sdk.Web` Зависит от пакета SDK:

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

Что вызывает следующие свойства и целевые объекты для импорта:

* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*

Опубликуйте право набор целевых объектов в зависимости от способа публикации используется Импорт целевых объектов.

При загрузке проекта Visual Studio или MSBuild выполняются следующие высокоуровневые действия:

* сборка проекта;
* вычисление файлов для публикации;
* публикация файлов в месте назначения;

## <a name="compute-project-items"></a>вычисление элементов проекта.

После загрузки проекта вычисляются элементы проекта (файлы). Порядок обработки файла определяется атрибутом `item type`. По умолчанию файлы с расширением *.cs* включаются в список элементов `Compile`. Файлы в списке элементов `Compile` компилируются.

`Content` Список элементов содержит файлы, которые публикуются в дополнение к выходных данных сборки. По умолчанию файлы, соответствующие шаблону `wwwroot/**` включаются в `Content` элемента. `wwwroot/\*\*` [Шаблон глобализацию](https://gruntjs.com/configuring-tasks#globbing-patterns) обозначает все файлы в *wwwroot* папки **и** вложенные папки. Чтобы явно добавить файл к списку публикации, добавьте файл непосредственно в *.csproj* файла, как показано в [включаемых файлов](#include-files).

При выборе **публикации** кнопки в Visual Studio или при публикации из командной строки:

* Вычисляются свойства и (или) элементы (файлы, требующие сборки).
* **Visual Studio только**: пакеты NuGet восстанавливаются. (Восстановление выполняется пользователем в интерфейсе командной строки.)
* Выполняется сборка проекта.
* Вычисляются публикуемые элементы (файлы, требующие публикации).
* Публикация проекта (вычисляемый файлы копируются в место назначения публикации).

Когда ссылается на проект ASP.NET Core `Microsoft.NET.Sdk.Web` в файле проекта *app_offline.htm* файл помещается в корне каталога веб-приложения. Если файл присутствует, модуль ASP.NET Core корректно завершает работу приложения и обслуживает файл *app_offline.htm* во время развертывания. Дополнительные сведения см. в разделе [Справочник по конфигурации модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).

## <a name="basic-command-line-publishing"></a>Основные публикации командной строки

Публикация командной строки работает на всех платформах, поддерживаемых .NET Core и не требует Visual Studio. В примере, показанном ниже [публикации dotnet](/dotnet/core/tools/dotnet-publish) команду следует запускать из каталога проекта (который содержит *.csproj* файла). В противном случае в папке проекта явным образом передать в путь к файлу проекта. Пример:

```console
dotnet publish C:\Webs\Web1
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

[Публикации dotnet](/dotnet/core/tools/dotnet-publish) команды выходные данные выглядят примерно следующим:

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

Папка для публикации по умолчанию — `bin\$(Configuration)\netcoreapp<version>\publish`. Значение по умолчанию для `$(Configuration)` — *отладки*. В предыдущем примере `<TargetFramework>` — `netcoreapp2.0`.

`dotnet publish -h` отображает справку по публикации.

Следующая команда определяет сборку `Release` и папку для публикации.

```console
dotnet publish -c Release -o C:\MyWebs\test
```

[Публикации dotnet](/dotnet/core/tools/dotnet-publish) команда вызывает MSBuild, которая вызывает `Publish` цели. Любые параметры, передаваемые в `dotnet publish` передаются в MSBuild. Параметр `-c` сопоставляется со свойством `Configuration` MSBuild. Параметр `-o` сопоставляется со свойством `OutputPath`.

Свойства MSBuild могут передаваться с помощью любого из следующих форматов:

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

Следующая команда публикует сборку `Release` в общую сетевую папку.

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

Общая сетевая папка указывается с помощью символов косой черты (*//r8/*) и работает на всех поддерживаемых платформах .NET Core.

Убедитесь, что приложение, публикуемое для развертывания, не запущено. Во время выполнения приложения файлы в папке *publish* блокируются. Заблокированные файлы скопировать нельзя, так что развертывание в этом случае не произойдет.

## <a name="publish-profiles"></a>Профили публикации

В этом разделе использует 2017 г. Visual Studio для создания профиля публикации. После создания публикации из Visual Studio или в командной строке доступна.

Публикация профилей можно упростить процесс публикации и могут иметь любое количество профилей. Создание профиля публикации в Visual Studio, выбрав один из следующих путей:

* Щелкните правой кнопкой мыши проект в обозревателе решений и выберите **публикации**.
* Выберите **публикации &lt;project_name&gt;**  из **построения** меню.

**Публикации** отображается на странице приложения производственные мощности. Если в проекте отсутствует профиль публикации, отображается со следующей страницы:

![Вкладка публикации на странице приложения емкости](visual-studio-publish-profiles/_static/az.png)

Когда **папки** — флажок установлен, укажите путь к папке для хранения опубликованных ресурсов. По умолчанию используется папка *bin\Release\PublishOutput*. Нажмите кнопку **создать профиль** кнопки завершения.

После создания профиля публикации **публикации** вкладке изменения. В раскрывающемся списке отображается только что созданный профиль. Нажмите кнопку **Создание нового профиля** для создания другого нового профиля.

![Вкладка публикации на странице приложения производственные мощности, показывающая FolderProfile](visual-studio-publish-profiles/_static/create_new.png)

Мастер публикации поддерживает следующие целевые объекты публикации.

* Служба приложений Azure
* Виртуальные машины Azure
* Службы IIS, FTP, т. д. (для любого веб-сервера)
* Папка
* Импорт профиля

Дополнительные сведения см. в разделе [какие параметры публикации оптимальной](/visualstudio/ide/not-in-toc/web-publish-options).

При создании профиля публикации с помощью Visual Studio *свойства/PublishProfiles/&lt;profile_name&gt;.pubxml* создается файл MSBuild. *.Pubxml* файл файла MSBuild, который содержит настройки конфигурации публикации. Этот файл можно изменить для настройки процесса построения и публикации процесс. Этот файл считывается процессом публикации. `<LastUsedBuildConfiguration>` является специальной, так как он является глобальным и не должны быть в любом файле, который импортируется в сборке. В разделе [MSBuild: как задать свойство конфигурации](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) для получения дополнительной информации.

При публикации в целевом объекте Azure, *.pubxml* файл содержит ваш идентификатор подписки Azure. С этим типом целевого добавлении этого файла в систему управления версиями не рекомендуется. При публикации на целевой объект не Azure, можно безопасно выполнить возврат *.pubxml* файла.

Конфиденциальные данные (например, пароль публикации) шифруются на уровне пользователя или компьютера. Он хранится в *свойства/PublishProfiles/&lt;profile_name&gt;. pubxml.user* файл. Так как этот файл можно хранить конфиденциальные данные, его не должно проверяться в системе управления версиями.

Общие сведения о том, как опубликовать веб-приложения ASP.NET Core. в разделе [узла и развернуть](xref:host-and-deploy/index). Задачи MSBuild и целевые объекты, необходимые для публикации приложения ASP.NET Core открытым исходным кодом на https://github.com/aspnet/websdk.

`dotnet publish` можно использовать папку MSDeploy, и [Kudu](https://github.com/projectkudu/kudu/wiki) профили публикации:

Папка (кросс-платформенное):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

MSDeploy (в настоящее время работает только в Windows, поскольку MSDeploy не кросс платформенных):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

MSDeploy пакета (в настоящее время работает только в Windows, поскольку MSDeploy не кросс платформенных):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

В предыдущем образцы **не** передачи `deployonbuild` для `dotnet publish`.

Дополнительные сведения см. в разделе [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).

`dotnet publish` поддерживает Kudu API-интерфейсы для публикации в Azure с любой платформы. Visual Studio опубликовать поддерживает Kudu API-интерфейсов, но поддерживается WebSDK для кросс платформенной публикации в Azure.

Добавить профиль публикации *свойства/PublishProfiles* папку со следующим содержимым:

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

Выполните следующую команду, чтобы архивировать содержимое публикации, а затем опубликовать его в Azure с помощью API-интерфейсы Kudu:

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

Для работы с профилем публикации задайте следующие свойства MSBuild.

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

При публикации с использованием профиля с именем *FolderProfile*, можно выполнить одну из приведенных ниже команд:

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

При вызове [построения dotnet](/dotnet/core/tools/dotnet-build), он вызывает `msbuild` для выполнения сборки и процесса публикации. Вызов любого `dotnet build` или `msbuild` эквивалентно при передаче в папке профиля. При вызове MSBuild непосредственно в Windows, .NET Framework версии MSBuild используется. В настоящее время MSDeploy поддерживает публикацию только на компьютерах Windows. Вызов `dotnet build` в других профилях, кроме профиля папки, вызывает MSBuild, который использует в таких профилях MSDeploy. Вызов `dotnet build` в других профилях, кроме профиля папки, вызывает MSBuild (с помощью MSDeploy) и завершается сбоем (даже при работе на платформе Windows). Для публикации профилей, отличных от профиля папки, вызывайте MSBuild напрямую.

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

Обратите внимание на то, что параметр `<LastUsedBuildConfiguration>` имеет значение `Release`. При публикации из Visual Studio свойство конфигурации `<LastUsedBuildConfiguration>` получает значение, которые использовалось при запуске процесса публикации. `<LastUsedBuildConfiguration>` Свойство конфигурации является особенным и не должен переопределяться из импортированного файла MSBuild. Это свойство можно переопределить с помощью командной строки.

С помощью .NET Core CLI:

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

С использованием MSBuild

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>Публикация конечной точки MSDeploy из командной строки

Публикация может выполняться с помощью .NET Core CLI или MSBuild. Команда `dotnet publish` выполняется в контексте .NET Core. `msbuild` Команде требуется .NET Framework, которая ограничивает его среды Windows.

Простейший способ публикации с помощью MSDeploy — это сначала создать профиль публикации в Visual Studio 2017, а затем применить этот профиль из командной строки.

В следующем примере создается веб-приложение ASP.NET Core (с помощью `dotnet new mvc`), и профиль публикации Azure добавляется с помощью Visual Studio.

Запустите `msbuild` из **Командная строка разработчика для VS 2017 г**. Командная строка разработчика содержит правильные *msbuild.exe* в пути с некоторым набором переменных MSBuild.

MSBuild использует следующий синтаксис.

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

Получить `Password` из  *\<имя публикации >. PublishSettings* файл. Загрузить *. PublishSettings* файл либо из:

* Обозреватель решений: Щелкните правой кнопкой мыши на веб-приложения и выберите **загрузить профиль публикации**.
* Портал Azure: щелкните **получение профиля публикации** на веб-приложения **Обзор** панель.

`Username` можно найти в профиле публикации.

В следующем примере используется *Web11112 - веб-развертывания* профиль публикации:

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a>Исключить файлы

При публикации веб-приложений ASP.NET Core включаются артефакты сборки и содержимое папки *wwwroot*. `msbuild` поддерживает [стандартные маски](https://gruntjs.com/configuring-tasks#globbing-patterns). Например, следующая `<Content>` элемент исключает весь текст (*.txt*) файлов из *wwwroot/content* и всех ее вложенных папок.

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Предыдущей разметки можно добавить профиль публикации или *.csproj* файла. При добавлении в файл *.csproj* правило добавляется во все профили публикации в проекте.

Следующие `<MsDeploySkipRules>` элемент исключает все файлы из *wwwroot/содержимого* папки:

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` не удалять *пропустить* целевых объектов с сайта развертывания. `<Content>` целевые файлы и папки будут удалены с сайта развертывания. Например предположим, что развернутого веб-приложения имели следующие файлы:

* *Views/Home/About1.cshtml*
* *Views/Home/About2.cshtml*
* *Views/Home/About3.cshtml*

Если следующие `<MsDeploySkipRules>` добавляются элементы, эти файлы не будут удалены на сайте развертывания.

```xml
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

Приведенный выше `<MsDeploySkipRules>` элементы предотвратить *пропущен* файлов, развертывание. Его не удалить эти файлы, как только они развернуты.

Следующие `<Content>` элемент удаляет целевых файлов на сайте развертывания:

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

С помощью развертывания из командной строки с предыдущим периодом `<Content>` элемент дает следующие результаты:

```console
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

## <a name="include-files"></a>Включаемые файлы

Включает следующую разметку *изображения* папку вне каталога проекта к *wwwroot/images* папку сайта публикации:

```xml
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

## <a name="run-a-target-before-or-after-publishing"></a>Выполнение целевого объекта до или после публикации

Встроенная `BeforePublish` и `AfterPublish` целей выполнение целевого файла до или после назначения публикации. Добавьте следующие элементы профиля публикации для журнала сообщений консоли до и после публикации:

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a>Опубликовать на сервере, использующий Недоверенный сертификат

Добавить `<AllowUntrustedCertificate>` свойство со значением `True` для профиля публикации:

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a>Служба Kudu

Чтобы просмотреть файлы в развертывании приложения веб-службы приложения Azure, используйте [Kudu службы](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service). Добавление `scm` токен, имя веб-приложения. Пример:

| URL-адрес                                    | Результат       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | Веб-приложение      |
| `http://mysite.scm.azurewebsites.net/` | Kudu службы |

Выберите [консоли отладки](https://github.com/projectkudu/kudu/wiki/Kudu-console) пункт меню, чтобы просмотреть, изменить, удалить или добавить файлы.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Веб-развертывание](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) упрощает развертывание веб-приложений и веб-сайтов на серверах IIS.
* [https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): Файл, проблем и запросов возможности развертывания.
