---
title: Профили публикации Visual Studio для развертывания приложений ASP.NET Core
author: rick-anderson
description: Узнайте, как создавать профили публикации в Visual Studio и применять их для управления развертыванием приложений ASP.NET Core в разных целевых объектах.
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2019
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: e1e8f99be18d6f395a146bda805f71c46cd0346d
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64889459"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a>Профили публикации Visual Studio для развертывания приложений ASP.NET Core

Авторы: [Саид Ибрагим Хашими](https://github.com/sayedihashimi) (Sayed Ibrahim Hashimi) и [Рик Андерсон](https://twitter.com/RickAndMSFT)

::: moniker range="<= aspnetcore-1.1"

Для версии 1.1 в этом разделе загрузите [профили публикации Visual Studio для развертывания приложений ASP.NET Core (версия 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/VS_Publish_Profiles_1.1.pdf).

::: moniker-end

Этот документ посвящен использованию Visual Studio 2017 или более поздней версии для создания и применения профилей публикации. Профили публикации, созданные с помощью Visual Studio, можно применять в MSBuild и Visual Studio. Инструкции по публикации в Azure см. в статье [Публикация веб-приложения ASP.NET Core в службе приложений Azure с помощью Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs).

Представленный ниже файл проекта был создан с помощью команды `dotnet new mvc`.

::: moniker range=">= aspnetcore-2.2"

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.2</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>
```

::: moniker-end

Атрибут `Sdk` элемента `<Project>` выполняет следующие задачи:

* Импортирует файл свойств из *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* в начало.
* Импортирует целевой файл из *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* в конец.

По умолчанию `MSBuildSDKsPath` (с Visual Studio 2017 Enterprise) находится в папке *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.

Пакет SDK `Microsoft.NET.Sdk.Web` имеет следующие зависимости:

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

Это означает, что он импортирует следующие свойства и целевые объекты:

* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*;
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*;
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*;
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*.

Целевые объекты публикации импортируют набор нужных целевых объектов в зависимости от используемых методов публикации.

Когда MSBuild или Visual Studio загружает проект, выполняются следующие обобщенные действия:

* сборка проекта;
* вычисление файлов для публикации;
* публикация файлов в месте назначения;

## <a name="compute-project-items"></a>вычисление элементов проекта.

После загрузки проекта вычисляются элементы проекта (файлы). Порядок обработки файла определяется атрибутом `item type`. По умолчанию файлы с расширением *.cs* включаются в список элементов `Compile`. Файлы в списке элементов `Compile` компилируются.

Список элементов `Content` содержит файлы, предназначенные для публикации, а также результаты сборки. По умолчанию файлы, соответствующие шаблону `wwwroot/**`, включаются в элемент `Content`. [Стандартная маска](https://gruntjs.com/configuring-tasks#globbing-patterns) `wwwroot/\*\*` соответствует всем файлам в папке *wwwroot* **и** всех ее подпапках. Чтобы явным образом добавить в список публикации конкретный файл, поместите его в *CSPROJ*-файл, как показано в разделе [Включаемые файлы](#include-files).

При нажатии кнопки **Публикация** в Visual Studio или при публикации из командной строки происходит следующее:

* Вычисляются свойства и (или) элементы (файлы, требующие сборки).
* **Только Visual Studio**: пакеты NuGet восстанавливаются. (Восстановление выполняется пользователем в интерфейсе командной строки.)
* Выполняется сборка проекта.
* Вычисляются публикуемые элементы (файлы, требующие публикации).
* Публикуется проект (вычисляемые файлы копируются в место назначения публикации.)

Когда проект ASP.NET Core ссылается на `Microsoft.NET.Sdk.Web` в файле проекта, файл *app_offline.htm* помещается в корневой каталог веб-приложения. Если файл присутствует, модуль ASP.NET Core корректно завершает работу приложения и обслуживает файл *app_offline.htm* во время развертывания. Дополнительные сведения см. в разделе [Справочник по конфигурации модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).

## <a name="basic-command-line-publishing"></a>Простая публикация из командной строки

Публикация из командной строки работает на всех платформах, поддерживаемых .NET Core, и не требует наличия Visual Studio. В приведенных ниже примерах команда [dotnet publish](/dotnet/core/tools/dotnet-publish) выполняется из папки проекта (где хранится файл *CSPROJ*). Если вы не находитесь в папке проекта, укажите путь к файлу проекта явным образом. Например:

```console
dotnet publish C:\Webs\Web1
```

Для создания и публикации веб-приложения выполните следующие команды.

```console
dotnet new mvc
dotnet publish
```

Выходные данные команды [dotnet publish](/dotnet/core/tools/dotnet-publish) выглядят примерно следующим образом:

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

Папка для публикации по умолчанию — `bin\$(Configuration)\netcoreapp<version>\publish`. Для параметра `$(Configuration)` по умолчанию подставляется значение *Debug*. В предыдущем примере `<TargetFramework>` имеет значение `netcoreapp2.0`.

`dotnet publish -h` отображает справку по публикации.

Следующая команда определяет сборку `Release` и папку для публикации.

```console
dotnet publish -c Release -o C:\MyWebs\test
```

Команда [dotnet publish](/dotnet/core/tools/dotnet-publish) вызывает MSBuild, что в свою очередь вызывает целевой объект `Publish`. Все параметры, передаваемые в `dotnet publish`, передаются далее в MSBuild. Параметр `-c` сопоставляется со свойством `Configuration` MSBuild. Параметр `-o` сопоставляется со свойством `OutputPath`.

Свойства MSBuild можно передавать в любом из следующих форматов.

* `p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

Следующая команда публикует сборку `Release` в общую сетевую папку.

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

Общая сетевая папка указывается с помощью символов косой черты (*//r8/*) и работает на всех поддерживаемых платформах .NET Core.

Убедитесь, что приложение, публикуемое для развертывания, не запущено. Во время выполнения приложения файлы в папке *publish* блокируются. Заблокированные файлы скопировать нельзя, так что развертывание в этом случае не произойдет.

## <a name="publish-profiles"></a>Профили публикации

В этом разделе для создания профиля публикации используется Visual Studio 2017 или более поздней версии. После создания профиля публикацию можно выполнять из Visual Studio или из командной строки.

Профили публикации упрощают процесс публикации. Вы можете создать любое количество профилей. Создайте профиль публикации в Visual Studio, выбрав один из следующих методов:

* В обозревателе решений щелкните проект правой кнопкой мыши и выберите команду **Опубликовать**.
* Выберите команду **Опубликовать {имя проекта}** в меню **Сборка**.

Откроется вкладка **Публикация** на странице свойств приложения. Если в проекте нет профиля публикации, откроется следующая страница:

![Вкладка "Публикация" на странице свойств приложения](visual-studio-publish-profiles/_static/az.png)

Выберите **Папка** и укажите путь к папке, в которой будут храниться опубликованные ресурсы. По умолчанию используется папка *bin\Release\PublishOutput*. Нажмите кнопку **Создать профиль**, чтобы завершить процесс.

Когда профиль публикации будет создан, изменения отразятся на вкладке **Публикация**. Новый профиль появится в раскрывающемся списке. Щелкните **Создать новый профиль**, чтобы создать еще один профиль.

![Вкладка "Публикация" на странице свойств приложения с профилем FolderProfile](visual-studio-publish-profiles/_static/create_new.png)

Мастер публикации поддерживает следующие целевые объекты публикации.

* Служба приложений Azure
* Виртуальные машины Azure
* IIS, FTP и т. д. (для любого веб-сервера)
* Папка
* Профиль импорта

Дополнительные сведения см. в статье [Выбор подходящих вариантов публикации](/visualstudio/ide/not-in-toc/web-publish-options).

При создании профиля публикации с помощью Visual Studio создается файл MSBuild *Properties/PublishProfiles/{имя_профиля}.pubxml*. Этот файл MSBuild с расширением *.pubxml* содержит параметры конфигурации публикации. Вы можете изменить этот файл, чтобы настроить процесс сборки и публикации. Этот файл считывается процессом публикации. Особое свойство `<LastUsedBuildConfiguration>` является глобальным и не должно присутствовать ни в одном файле, импортируемом в сборку. Дополнительные сведения см. в статье [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) (MSBuild: настройка свойства конфигурации).

Если публикация выполняется в целевой объект Azure, файл с расширением *.pubxml* содержит идентификатор подписки Azure. При таком типе целевого объекта мы не рекомендуем добавлять этот файл в систему управления версиями. Если публикация выполняется в целевой объект, не относящийся к Azure, файл с расширением *.pubxml* можно безопасно вернуть в систему.

Конфиденциальные данные (например, пароль публикации) шифруются на уровне пользователя или компьютера. Они сохраняются в файле *Properties/PublishProfiles/{имя_профиля}.pubxml.user*. Так как этот файл может содержать конфиденциальные данные, его не следует возвращать в систему управления версиями.

Общие сведения о публикации веб-приложений в ASP.NET Core см. в статье [Размещение и развертывание ASP.NET Core](xref:host-and-deploy/index). Открытый исходный код задач MSBuild и целевых объектов, необходимых для публикации приложения ASP.NET Core, размещен в https://github.com/aspnet/websdk.

`dotnet publish` может использовать профили публикации папки, MSDeploy и [Kudu](https://github.com/projectkudu/kudu/wiki).

Папка (работает на всех платформах):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

MSDeploy (сейчас работает только в Windows, так как MSDeploy не является кроссплатформенным):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

Пакет MSDeploy (сейчас работает только в Windows, так как MSDeploy не является кроссплатформенным):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

В примерах выше **не** передавайте `deployonbuild` в `dotnet publish`.

Дополнительные сведения см. в [документации по Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).

`dotnet publish` поддерживает API-интерфейсы Kudu для публикации в Azure с любой платформы. Публикация с помощью Visual Studio поддерживает API Kudu, но для кроссплатформенной публикации в Azure их поддерживает WebSDK.

Добавьте в папку *Properties/PublishProfiles* профиль публикации со следующим содержимым:

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

Выполните следующую команду, чтобы архивировать содержимое публикации и опубликовать его в Azure с помощью API-интерфейсов Kudu.

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

Для работы с профилем публикации задайте следующие свойства MSBuild.

* `DeployOnBuild=true`
* `PublishProfile={PUBLISH PROFILE}`

Для публикации с использованием профиля *FolderProfile* вы можете выполнить одну из указанных ниже команд.

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Команда [dotnet build](/dotnet/core/tools/dotnet-build) вызывает `msbuild` для сборки и публикации. Вызовы `dotnet build` и `msbuild` эквивалентны, если они передаются в профиле папки. При прямом вызове MSBuild на платформе Windows используется версия MSBuild для .NET Framework. В настоящее время MSDeploy поддерживает публикацию только на компьютерах Windows. Вызов `dotnet build` в других профилях, кроме профиля папки, вызывает MSBuild, который использует в таких профилях MSDeploy. Вызов `dotnet build` в других профилях, кроме профиля папки, вызывает MSBuild (с помощью MSDeploy) и завершается сбоем (даже при работе на платформе Windows). Для публикации профилей, отличных от профиля папки, вызывайте MSBuild напрямую.

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

Обратите внимание на то, что параметр `<LastUsedBuildConfiguration>` имеет значение `Release`. При публикации из Visual Studio свойство конфигурации `<LastUsedBuildConfiguration>` получает значение, которые использовалось при запуске процесса публикации. Свойство конфигурации `<LastUsedBuildConfiguration>` имеет особое значение и его не следует переопределять в импортируемом файле MSBuild. Это свойство можно переопределить из командной строки.

С использованием .NET Core CLI:

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

С использованием MSBuild

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>Публикация конечной точки MSDeploy из командной строки

В следующем примере используется веб-приложение ASP.NET Core, созданное с помощью Visual Studio, с именем *AzureWebApp*. Профиль публикации приложений Azure добавляется с помощью Visual Studio. Дополнительные сведения о том, как создать профиль, см. в разделе [Профили публикации](#publish-profiles).

Чтобы развернуть приложение с помощью профиля публикации, выполните команду `msbuild` из **командной строки разработчика** Visual Studio. Командная строка доступна в папке *Visual Studio* в меню **Пуск** на панели задач Windows. Для удобства можно добавить командную строку в меню **Сервис** в Visual Studio. Дополнительные сведения см. в статье [Командная строка разработчика для Visual Studio](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).

MSBuild использует следующий синтаксис команд.

```console
msbuild {PATH} 
    /p:DeployOnBuild=true 
    /p:PublishProfile={PROFILE} 
    /p:Username={USERNAME} 
    /p:Password={PASSWORD}
```

* {PATH} &ndash; путь к файлу проекта приложения.
* {PROFILE} &ndash; имя профиля публикации.
* {USERNAME} &ndash; имя пользователя MSDeploy. {USERNAME} можно найти в профиле публикации.
* {PASSWORD} &ndash; пароль MSDeploy. Получите {PASSWORD} из файла *{PROFILE}.PublishSettings*. Файл с расширением *.PublishSettings* можно скачать одним из следующих способов:
  * Обозреватель решений. Выберите **Представление** > **Cloud Explorer**. Подключитесь к подписке Azure. Откройте **Службы приложений**. Щелкните правой кнопкой приложение. Выберите **Загрузить профиль публикации**.
  * Портал Azure: щелкните **Скачать профиль публикации** на панели **Обзор** веб-приложения.

В следующем примере используется профиль публикации с именем *AzureWebApp — веб-развертывание*:

```console
msbuild "AzureWebApp.csproj" 
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

Профиль публикации можно также использовать с командой .NET Core CLI [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) из командной строки Windows.

```console
dotnet msbuild "AzureWebApp.csproj"
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

> [!NOTE]
> Команда [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) доступна на всех платформах и может компилировать приложения ASP.NET Core в macOS и Linux. Однако MSBuild в macOS и Linux не может развертывать приложения в Azure или другой конечной точке MSDeploy. MSDeploy доступен только в Windows.

## <a name="set-the-environment"></a>Указание среды

Включите свойство `<EnvironmentName>` в профиле публикации (*PUBXML*) или файле проекта, чтобы задать [среду](xref:fundamentals/environments) приложения:

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

Если вам нужно преобразовать *web.config* (например, задать переменные среды на основе конфигурации, профиля или среды), см. раздел <xref:host-and-deploy/iis/transform-webconfig>.

## <a name="exclude-files"></a>Исключение файлов

При публикации веб-приложений ASP.NET Core включаются артефакты сборки и содержимое папки *wwwroot*. `msbuild` поддерживает [стандартные маски](https://gruntjs.com/configuring-tasks#globbing-patterns). Например, представленный ниже элемент `<Content>` исключит все текстовые файлы (*.txt*) из папки *wwwroot/content* и всех ее подпапок.

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Предложенную выше разметку можно добавить в профиль публикации или в файл *CSPROJ*. При добавлении в файл *.csproj* правило добавляется во все профили публикации в проекте.

Следующий элемент `<MsDeploySkipRules>` исключает все файлы из папки *wwwroot/content*.

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

Элемент `<MsDeploySkipRules>` не удаляет *пропускаемые* целевые объекты с сайта развертывания. Целевые файлы и папки, указанные в элементе `<Content>`, будут удалены с сайта развертывания. Допустим, что в развернутом веб-приложении есть следующие файлы.

* *Views/Home/About1.cshtml*
* *Views/Home/About2.cshtml*
* *Views/Home/About3.cshtml*

Если вы добавите указанные ниже элементы `<MsDeploySkipRules>`, эти файлы не будут удалены с сайта развертывания.

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

Приведенные выше элементы `<MsDeploySkipRules>` запрещают развертывание *пропускаемых* файлов. Эти файлы не будут удалены, если они уже развернуты.

Следующий элемент `<Content>` удаляет целевые файлы на сайте развертывания.

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Применение указанного выше элемента `<Content>` при развертывании из командной строки дает следующий результат:

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

Показанная ниже разметка позволяет включить папку *images*, расположенную за пределами папки проекта, в папку *wwwroot/images* на сайте публикации.

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

Показанная выше разметка может быть добавлена в файл *.csproj* или в профиль публикации. Если добавить ее в файл *CSPROJ*, она будет включаться в каждый профиль публикации этого проекта.

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

Встроенные целевые объекты `BeforePublish` и `AfterPublish` позволяют выполнить целевой объект до или после его публикации. Добавьте следующие элементы в профиль публикации, чтобы сохранять сообщения в консоли, возвращаемые до и после публикации:

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a>Публикация на сервере с использованием сертификата без доверия

Добавьте свойство `<AllowUntrustedCertificate>` со значением `True` в профиль публикации:

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a>Служба Kudu

Чтобы просмотреть файлы в развертывании веб-приложения службы приложений Azure, используйте [службу Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service). Добавьте к имени веб-приложения маркер `scm`. Например:

| URL-адрес                                    | Результат       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | Веб-приложение      |
| `http://mysite.scm.azurewebsites.net/` | Служба Kudu |

Для просмотра, редактирования, удаления и (или) добавления файлов выберите в меню пункт [Консоль отладки](https://github.com/projectkudu/kudu/wiki/Kudu-console).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Веб-развертывание](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) упрощает развертывание веб-приложений и веб-сайтов на серверах IIS.
* [https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): проблемы с файлами и возможности запросов для развертывания.
* [Публикация веб-приложения ASP.NET в виртуальную машину Azure из Visual Studio](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
* <xref:host-and-deploy/iis/transform-webconfig>
