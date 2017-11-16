---
title: "Создание профилей публикации для Visual Studio и MSBuild"
author: rick-anderson
description: "Описание веб-публикации в Visual Studio."
keywords: "ASP.NET Core, веб-публикация, публикация, msbuild, веб-развертывание, публикации dotnet, Visual Studio 2017"
ms.author: riande
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.assetid: 0377a02d-8fda-47a5-929a-24a16e1d2c93
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/web-publishing-vs
ms.openlocfilehash: f010f9d90165ce4d6718fe1440e600985f21a01d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="create-publish-profiles-for-visual-studio-and-msbuild-to-deploy-aspnet-core-apps"></a>Создание профилей публикации для Visual Studio и MSBuild с целью развертывания приложений ASP.NET Core

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
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.1" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.2" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.1" />
  </ItemGroup>

</Project>
```

---

Атрибут `Sdk` в элементе `<Project>` (первая строка) показанной выше разметки выполняет следующие действия.

* Импортирует файл `props` из *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* в начало.
* Импортирует целевой файл из *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* в конец.

По умолчанию `MSBuildSDKsPath` (с Visual Studio 2017 Enterprise) находится в папке *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.

`Microsoft.NET.Sdk.Web` зависит от следующих компонентов.

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

В связи с чем импортируются следующие файлы `props` и `targets`.

* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets

Целевые объекты публикации, в свою очередь, также импортируют соответствующий набор целевых объектов в зависимости от используемых методов публикации.

Когда MSBuild или Visual Studio загружает проект, на высоком уровне выполняются следующие действия:

* сборка проекта;
* вычисление файлов для публикации;
* публикация файлов в месте назначения;

### <a name="compute-project-items"></a>вычисление элементов проекта.

После загрузки проекта вычисляются элементы проекта (файлы). Порядок обработки файла определяется атрибутом `item type`. По умолчанию файлы с расширением *.cs* включаются в список элементов `Compile`. Файлы в списке элементов `Compile` компилируются.

Список элементов `Content` содержит предназначенные для публикации файлы и результаты сборки. По умолчанию файлы, соответствующие шаблону wwwroot/**, включаются в элемент `Content`. [wwwroot/** — это стандартная маска](https://gruntjs.com/configuring-tasks#globbing-patterns), которая охватывает все файлы в папке *wwwroot* **и** ее подпапках. Чтобы напрямую добавить в список публикации отдельный файл, можно добавить этот файл в файл *CSPROJ*, как показано в разделе [Включение файлов](#including-files).

При нажатии кнопки **Публикация** в Visual Studio или публикации из командной строки происходит следующее.

- Вычисляются свойства и (или) элементы (файлы, требующие сборки).
- Только в Visual Studio: восстанавливаются пакеты NuGet.  (Восстановление выполняется пользователем в интерфейсе командной строки.)
- Выполняется сборка проекта.
- Вычисляются публикуемые элементы (файлы, требующие публикации).
- Проект публикуется. (Вычисляемые файлы копируются в место назначения публикации.)

## <a name="basic-command-line-publishing"></a>Простая публикация из командной строки

Этот метод работает на всех поддерживаемых платформах .NET Core и не требует Visual Studio. В приведенных ниже примерах команда `dotnet publish` выполняется из папки проекта (содержащей файл *.csproj*). Если вы не находитесь в папке проекта, путь к файлу проекта можно передать явным образом. Пример:

```console
dotnet publish  c:/webs/web1
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

--------------

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

Команда `dotnet publish` вызывает метод `MSBuild`, который, в свою очередь, вызывает целевой объект `Publish`. Все параметры, передаваемые в `dotnet publish`, передаются в `MSBuild`. Параметр `-c` сопоставляется со свойством `Configuration` MSBuild. Параметр `-o` сопоставляется со свойством `OutputPath`.

Свойства MSBuild можно передавать в любом из следующих форматов.

- ` p:<NAME>=<VALUE>`
- `/p:<NAME>=<VALUE>`

Следующая команда публикует сборку `Release` в общую сетевую папку.

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

Общая сетевая папка указывается с помощью символов косой черты (*//r8/*) и работает на всех поддерживаемых платформах .NET Core.

Убедитесь, что приложение, публикуемое для развертывания, не запущено. Во время выполнения приложения файлы в папке *publish* блокируются. Заблокированные файлы скопировать нельзя, так что развертывание в этом случае не произойдет.

## <a name="publish-profiles"></a>Профили публикации

В этом разделе для создания профилей публикации используется Visual Studio 2017 и более поздних версий. После создания профилей публикацию можно произвести из Visual Studio или из командной строки.

Профили публикации позволяют упростить процесс публикации. Таких профилей может быть несколько. Чтобы создать профиль публикации в Visual Studio, щелкните проект в обозревателе решений правой кнопкой мыши и выберите команду **Опубликовать**. Другой вариант: открыть меню сборки и выбрать команду **Опубликовать     \<имя проекта >**. Откроется вкладка **Публикации** на странице свойств приложения. Если проект не содержит профиль публикации, откроется следующая страница.

![Вкладка **Публикация** на странице свойств приложения с разделами "Azure", "IIS, FTB" и "Папка" и выбранным разделом Azure Здесь же представлены флажки, позволяющие создать новый профиль или выбрать уже существующий.](web-publishing-vs/_static/az.png)

Если выбран раздел **Папка**, кнопка **Опубликовать** создает профиль публикации в папку и выполняет публикацию.

![Вкладка **Публикация** на странице свойств приложения с разделами "Azure", "IIS, FTB" и "Папка"](web-publishing-vs/_static/pub1.png)

После того, как профиль публикации будет создан, вкладка **Публикация** изменит вид, и вы сможете выбрать пункт **Создать профиль**, чтобы создать новый профиль.

![Вкладка **Публикация** на странице свойств приложения с созданным на предыдущем шаге профилем FolderProfile и ссылкой "Создать профиль".](web-publishing-vs/_static/create_new.png)

Мастер публикации поддерживает следующие целевые объекты публикации.

- Служба приложений Microsoft Azure
- IIS, FTP и т. д (для любого веб-сервера)
- Папка
- Импорт профиля (позволяет импортировать профиль)
- Виртуальные машины Microsoft Azure

Дополнительные сведения см. в статье [Выбор подходящих вариантов публикации](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options).

При создании профиля публикации с помощью Visual Studio создается файл MSBuild *Properties/PublishProfiles/\<имя публикации>.pubxml*. Это файл с расширением *.pubxml* представляет собой файл MSBuild, содержащий настройки конфигурации публикации. Процесс сборки и публикации можно настроить, скорректировав этот файл. Этот файл считывается процессом публикации. Свойство `<LastUsedBuildConfiguration>` отличается тем, что является глобальным и не должно присутствовать ни в одном файле, импортируемом в сборку. Дополнительные сведения см. в статье [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) (MSBuild: настройка свойства конфигурации).
Файл с расширением *.pubxml* не должен возвращаться в систему управления версиями, так как зависит от файла *.user*. Файл *.user* ни в коем случае не должен возвращаться в систему управления версиями, так как может содержать конфиденциальные сведения и быть действительным только для одного пользователя и компьютера.

Конфиденциальные данные (такие как пароль публикации) шифруются на уровне пользователя или компьютера и хранятся в файле *Properties/PublishProfiles/\<имя публикации>.pubxml.user*. Так как этот файл может содержать конфиденциальные данные, он **не** должен возвращаться в систему управления версиями.

Общие сведения о том, как опубликовать веб-приложение в ASP.NET Core, см. в статье [Публикация и развертывание](index.md). [Публикация и развертывание](index.md) — это проект с открытым кодом на сайте https://github.com/aspnet/websdk.

 `dotnet publish` может использовать профили публикации папки, Msdeploy и [KUDU](https://github.com/projectkudu/kudu/wiki):
 
Папка (работает на всех платформах): `dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`

Msdeploy (сейчас работает только в Windows, так как эта команда не поддерживается на всех платформах): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`

Пакет Msdeploy (сейчас работает только в Windows, так как Msdeploy не поддерживается на всех платформах): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`

В примерах выше **не** передавайте `deployonbuild` в `dotnet publish`.

Дополнительные сведения см. в [документации по пакету Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).

`dotnet publish` поддерживает API KUDU для публикации в Azure с любой платформы. Публикация с помощью Visual Studio не поддерживает API KUDU, но эти API поддерживаются websdk для кроссплатформенной публикации в Azure.

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

Выполните следующую команду, чтобы архивировать содержимое публикации и опубликовать его в Azure с помощью API-интерфейсов KUDU.

`dotnet publish /p:PublishProfile=Azure /p:Configuration=Release`

Для работы с профилем публикации задайте следующие свойства MSBuild.

- `DeployOnBuild=true`
- `PublishProfile=<Publish profile name>`

Например, при публикации с использованием профиля *FolderProfile* можно выполнить одну из указанных ниже команд.

- `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
- `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

При вызове `dotnet build` он вызывает `msbuild` для выполнения сборки и публикации. Вызов `dotnet build` или `msbuild` по сути работает как передача профиля папки. Вызывая MSBuild в Windows напрямую, вы получаете версию MSBuild .NET Framework.  В настоящее время MSDeploy поддерживает публикацию только на компьютерах Windows. Вызов `dotnet build` в других профилях, кроме профиля папки, вызывает MSBuild, который использует в таких профилях MSDeploy. Вызов `dotnet build` в других профилях, кроме профиля папки, вызывает MSBuild (с помощью MSDeploy) и завершается сбоем (даже при работе на платформе Windows). Для публикации профилей, отличных от профиля папки, вызывайте MSBuild напрямую.

Следующий профиль публикации папки был создан в Visual Studio и публикуется в общей сетевой папке:

[!code-xml[Main](web-publishing-vs/sample/FolderProfile.pubxml?highlight=5,9,11,18)]

Обратите внимание на то, что параметр `<LastUsedBuildConfiguration>` имеет значение `Release`.  При публикации из Visual Studio свойство конфигурации `<LastUsedBuildConfiguration>` получает значение, которые использовалось при запуске процесса публикации. Свойство конфигурации `<LastUsedBuildConfiguration>` имеет особое значение и не должно переопределяться в импортированном файле MSBuild. Переопределить это свойство можно из командной строки. Пример:

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

С использованием MSBuild

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>Публикация конечной точки MSDeploy из командной строки

Как уже говорилось, для публикации можно использовать команды `dotnet publish` и `msbuild`. Команда `dotnet publish` выполняется в контексте .NET Core. Команда `msbuild` требует платформы .NET Framework и работает только в средах Windows.

Простейший способ публикации с помощью MSDeploy — это сначала создать профиль публикации в Visual Studio 2017, а затем применить этот профиль из командной строки.

В следующем примере создается веб-приложение ASP.NET Core (с помощью команды `dotnet new mvc`), и добавляется профиль публикации Azure с помощью Visual Studio.

Команда `msbuild` выполняется в **командной строке разработчика для VS 2017**. Командная строка разработчика задаст соответствующий файл *msbuild.exe* в пути и некоторые переменные MSBuild.

MSBuild использует следующий синтаксис.

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile>  /p:Username=<USERNAME> /p:Password=<PASSWORD>`

Значение `Password` можно получить из файла *\<Имя публикации>.PublishSettings*. Файл *.PublishSettings* можно загрузить следующим образом.

- Обозреватель решений: щелкните веб-приложения правой кнопкой мыши и выберите пункт **Загрузить профиль публикации**.
- Портал управления Azure: выберите пункт **Получить профиль публикации** в колонке "Веб-приложение".

`Username` можно найти в профиле публикации.

В следующем примере используется профиль публикации "Web11112 - Web Deploy".

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a>Исключение файлов

При публикации веб-приложений ASP.NET Core включаются артефакты сборки и содержимое папки *wwwroot*. `msbuild` поддерживает [стандартные маски](https://gruntjs.com/configuring-tasks#globbing-patterns). Например, представленная ниже разметка элемента `<Content>` исключит все текстовые файлы (*.txt*) из папки *wwwroot/content* и всех ее подпапок.

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
    <AbsolutePath>wwwroot\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

Элемент `<MsDeploySkipRules>` не удаляет с сайта развертывания *пропускаемые* целевые объекты. С сайта развертывания будут удалены файлы и папки, указанные в элементе `<Content>`. Допустим, например, что вы развернули веб-приложение со следующими файлами.

- *Views/Home/About1.cshtml*
- *Views/Home/About2.cshtml*
- *Views/Home/About3.cshtml*

Если при этом вы добавили указанную ниже разметку `<MsDeploySkipRules>`, эти файлы не будут удалены с сайта развертывания.

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

Показанная выше разметка `<MsDeploySkipRules>` предотвращает развертывание *пропускаемых* файлов, но не удаляет эти файлы после развертывания.

Следующая разметка `<Content>` удалит целевые файлы на сайте развертывания.

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Развертывание из командной строки с указанной выше разметкой `<Content>` даст результаты следующего вида.

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

Показанная ниже разметка позволяет включить папку *изображений*, расположенную не в папке проекта, в папку *wwwroot/images* на сайте публикации.

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

Показанная выше разметка может быть добавлена в файл *.csproj* или в профиль публикации. При добавлении в файл *.csproj* она будет включаться в каждый профиль публикации в проекте.

Разметка, выделенная в приведенном ниже примере, показывает, как.

* Скопировать файл из папки, не связанной с проектом, в папку *wwwroot*.
* Исключить папку *wwwroot\Content*.
* Исключить файл *Views\Home\About2.cshtml*.

[!code-xml[Main](web-publishing-vs/sample/FolderProfile2.pubxml?highlight=21-29)]

Дополнительные примеры развертывания см. в файле [WebSDK Readme](https://github.com/aspnet/websdk).

### <a name="run-a-target-before-or-after-publishing"></a>Выполнение целевого объекта до или после публикации

Встроенные целевые объекты `BeforePublish` и `AfterPublish` можно использовать для выполнения целевого объекта до или после его публикации. В профиль публикации можно добавить показанную ниже разметку — в этом случае сообщения будут добавлены в вывод консоли до или после публикации.

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a>Служба Kudu

Просматривать файлы веб-приложения Azure можно в [службе Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service). Для этого добавьте к имени веб-приложения маркер `scm`. Пример:

| URL-адрес               | Результат|
| ----------------- | ------------ |
| `http://mysite.azurewebsites.net/` | Веб-приложение |
| `http://mysite.scm.azurewebsites.net/` | Служба Kudu |

Для просмотра, редактирования, удаления и добавления файлов выберите в меню пункт [Консоль отладки](https://github.com/projectkudu/kudu/wiki/Kudu-console).

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Веб-развертывание](https://www.iis.net/downloads/microsoft/web-deploy) (msdeploy) упрощает развертывание веб-приложений и веб-сайтов на серверах IIS.

- [https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): проблемы с файлами и особенности запросов для развертывания.
