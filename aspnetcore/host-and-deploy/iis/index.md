---
title: "Размещение ASP.NET Core в Windows со службами IIS"
author: guardrex
description: "Сведения о размещении приложений ASP.NET Core в службах Windows Server Internet Information Services (IIS)."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/iis/index
ms.openlocfilehash: 18c7448ad79891d04eca1e939a0aeeabe417bde8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="host-aspnet-core-on-windows-with-iis"></a>Размещение ASP.NET Core в Windows со службами IIS

Авторы: [Люк Лэтэм](https://github.com/guardrex) (Luke Latham) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

## <a name="supported-operating-systems"></a>Поддерживаемые операционные системы

Поддерживаются следующие операционные системы:

* Windows 7 и более поздних версий;
* Windows Server 2008 R2 и более поздних версий.

&#8224;В принципе, конфигурация IIS, описываемая в этом документе, также применима при размещении приложений ASP.NET Core в службах IIS Nano Server, но более точные инструкции см. в статье [ASP.NET Core со службами IIS в Nano Server](xref:tutorials/nano-server).

[Сервер HTTP.sys](xref:fundamentals/servers/httpsys) (который ранее назывался [WebListener](xref:fundamentals/servers/weblistener)) не будет работать в конфигурации обратного прокси-сервера со службами IIS. Используйте [сервер Kestrel](xref:fundamentals/servers/kestrel).

## <a name="iis-configuration"></a>Конфигурация IIS

Включите роль **Веб-сервер (IIS)** и создайте службы роли.

### <a name="windows-desktop-operating-systems"></a>Операционные системы Windows для настольных компьютеров

Последовательно выберите **Панель управления** > **Программы** > **Программы и компоненты** > **Включение или отключение компонентов Windows** (в левой части экрана). Откройте группу **Службы IIS**, а затем — **Средства управления веб-сайтом**. Установите флажок **Консоль управления IIS**. Установите флажок **Службы Интернета**. В группе **Службы Интернета** оставьте компоненты по умолчанию или настройте компоненты IIS.

![Группы "Консоль управления IIS" и "Службы Интернета" выбраны в окне "Компоненты Windows".](index/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a>Операционные системы семейства Windows Server

В серверной операционной системе используйте мастер **добавления ролей и компонентов**, который можно вызвать в меню **Управление** или по ссылке в **диспетчере сервера**. На этапе **Роли сервера** установите флажок **Веб-сервер (IIS)**.

![Роль "Веб-сервер (IIS)" выбрана на странице "Выбор ролей сервера".](index/_static/server-roles-ws2016.png)

На этапе **Службы роли** выберите нужные службы роли IIS или оставьте службы по умолчанию.

![Службы роли по умолчанию, выбранные на странице "Выбор служб ролей".](index/_static/role-services-ws2016.png)

Пройдите этап **Подтверждение**, чтобы установить роль и службы веб-сервера. После установки роли "Веб-сервер (IIS)" перезагружать сервер или службы IIS не требуется.

## <a name="install-the-net-core-windows-server-hosting-bundle"></a>Установка пакета размещения .NET Core для Windows Server

1. Установите [пакет размещения .NET Core для Windows Server](https://aka.ms/dotnetcore-2-windowshosting) в размещающей системе. В составе пакета устанавливаются среда выполнения .NET Core, библиотека .NET Core и [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module). Модуль создает обратный прокси-сервер между службами IIS и сервером Kestrel. Если система не подключена к Интернету, перед установкой пакета размещения .NET Core для Windows Server получите и установите [Распространяемый компонент Microsoft Visual C++ 2015](https://www.microsoft.com/download/details.aspx?id=53840).

2. Перезапустите систему или выполните в командной строке команду **net stop was /y**, а затем команду **net start w3svc**, чтобы изменение системной переменной PATH вступило в силу.

> [!NOTE]
> Сведения об общей конфигурации IIS см. в разделе [Модуль ASP.NET Core с общей конфигурацией IIS](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a>Установка веб-развертывания при публикации с помощью Visual Studio

При развертывании приложений на серверах с помощью [веб-развертывания](/iis/publish/using-web-deploy/introduction-to-web-deploy) установите на сервере последнюю версию веб-развертывания. Чтобы установить веб-развертывание, можно использовать [установщик веб-платформы (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) или получить установщик непосредственно в [Центре загрузки Майкрософт](https://www.microsoft.com/download/details.aspx?id=43717). Предпочтительный метод — использовать WebPI. WebPI обеспечивает автономную установку и настройку поставщиков размещения.

## <a name="application-configuration"></a>Настройка приложения

### <a name="enabling-the-iisintegration-components"></a>Включение компонентов IISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Обычно *Program.cs* вызывает [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), чтобы начать настройку узла. `CreateDefaultBuilder` настраивает [Kestrel](xref:fundamentals/servers/kestrel) в качестве веб-сервера и активирует интеграцию IIS, задавая базовый путь и порт для [модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module):

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Включите в зависимости приложения зависимость от пакета [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/). Включите в приложение ПО промежуточного слоя для интеграции IIS, добавив метод расширения *UseIISIntegration* в *WebHostBuilder*.

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

`UseKestrel` и `UseIISIntegration` являются обязательными элементами. Код, вызывающий `UseIISIntegration`, не влияет на переносимость кода. Если приложение запускается не в IIS (например, запускается непосредственно в Kestrel), то в `UseIISIntegration` нет операций.

---

Дополнительные сведения о размещении см. в разделе [Размещение в ASP.NET Core](xref:fundamentals/hosting).

### <a name="iis-options"></a>Параметры служб IIS

Чтобы настроить параметры служб IIS, включите конфигурацию служб для `IISOptions` в `ConfigureServices`.

```csharp
services.Configure<IISOptions>(options => 
{
    ...
});
```

| Параметр                         | По умолчанию | Параметр |
| ------------------------------ | ------- | ------- |
| `AutomaticAuthentication`      | `true`  | Если значение — `true`, ПО промежуточного слоя для проверки подлинности задаст значение `HttpContext.User` и будет отвечать на общие запросы. Если значение — `false`, ПО промежуточного слоя для проверки подлинности только предоставит идентификатор (`HttpContext.User`) и будет отвечать только на явные запросы от `AuthenticationScheme`. Для работы `AutomaticAuthentication` необходимо включить в службах IIS проверку подлинности Windows. |
| `AuthenticationDisplayName`    | `null`  | Задает отображаемое имя для пользователей на страницах входа. |
| `ForwardClientCertificate`     | `true`  | Если значение — `true` и если присутствует заголовок запроса `MS-ASPNETCORE-CLIENTCERT`, происходит заполнение `HttpContext.Connection.ClientCertificate`. |

### <a name="webconfig"></a>web.config.

Основным назначением файла *Web.config* является настройка [модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module). Он может также предоставлять дополнительные параметры конфигурации IIS. За создание, преобразования и публикацию *web.config* отвечает веб-пакет SDK для .NET Core (`Microsoft.NET.Sdk.Web`). Пакет SDK устанавливается поверх файла проекта (`<Project Sdk="Microsoft.NET.Sdk.Web">`). Чтобы пакет SDK не преобразовывал файл *web.config*, добавьте в файл проекта свойство **\<IsTransformWebConfigDisabled>** со значением `true`:

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

Если в проекте есть файл *web.config*, он преобразуется с использованием соответствующих аргументов *processPath* и *arguments* для настройки [модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) и переносится в [опубликованные выходные данные](xref:host-and-deploy/directory-structure). Преобразование не изменяет параметры конфигурации служб IIS, включенные в файл.

### <a name="webconfig-location"></a>расположение web.config

Приложения .NET Core размещаются с помощью обратного прокси-сервера между службами IIS и сервером Kestrel. Для создания обратного прокси-сервера файл *web.config* должен находиться по корневому пути к содержимому развернутого приложения (как правило, это основной путь приложения). Это физический путь к веб-сайту, указанный в службах IIS. Файл *web.config* требуется в корне приложения, чтобы разрешить публикацию нескольких приложений с помощью веб-развертывания.

По физическому пути приложения находятся важные файлы, включая такие вложенные папки, как *\<имя_сборки>.runtimeconfig.json*, *\<имя_сборки>.xml* (комментарии к документам XML) и *\<имя_сборки>.deps.json*. Когда имеющийся файл *web.config* настраивает сайт, службы IIS препятствует обслуживанию этих конфиденциальных файлов. **Поэтому важно следить за тем, чтобы файл *web.config* не был случайно переименован или удален из развертывания**.

## <a name="create-the-iis-website"></a>Создание веб-сайта IIS

1. В целевой системе IIS создайте папку для опубликованных папок и файлов приложения, которые описываются в статье [Структура каталогов](xref:host-and-deploy/directory-structure).

2. В папке создайте папку *logs* для хранения журналов StdOut, если включено их ведение. Если приложение развертывается с папкой *logs* в полезных данных, пропустите этот шаг. Инструкции о том, как MSBuild создает папку *logs*, см. в разделе [Структура каталога](xref:host-and-deploy/directory-structure).

3. В **диспетчере IIS** создайте веб-сайт. Укажите имя в поле **Имя сайта** и задайте значение в поле **Физический путь** для созданной папки развертывания приложения. Укажите конфигурацию **привязки** и создайте веб-сайт.

4. Для пула приложений выберите **Без управляемого кода**. ASP.NET Core выполняется в отдельном процессе и управляет средой выполнения.

5. Откройте окно **Добавить веб-сайт**.

   ![В контекстном меню узла "Сайты" выберите пункт "Добавить веб-сайт".](index/_static/add-website-context-menu-ws2016.png)

6. Настройте веб-сайт.

   ![В окне "Добавить веб-сайт" укажите имя сайта, физический путь и имя узла.](index/_static/add-website-ws2016.png)

7. На панели **Пулы приложений** откройте окно **Изменение пула приложений**, щелкнув пул приложений веб-сайта правой кнопкой мыши и выбрав в контекстном меню пункт **Основные параметры...**

   ![В контекстном меню пула приложений выберите пункт "Основные настройки".](index/_static/apppools-basic-settings-ws2016.png)

8. Для параметра **Версия среды CLR .NET** выберите значение **Без управляемого кода**.

   ![Выберите "Без управляемого кода" для параметра "Версия среды CLR .NET".](index/_static/edit-apppool-ws2016.png)
     
    Примечание. Задавать для параметра **Версия среды CLR .NET** значение **Без управляемого кода** необязательно. Для ASP.NET Core не требуется загрузка классической среды CLR.

9. Убедитесь в том, что удостоверение модели процесса имеет соответствующие разрешения.

   При изменении удостоверения по умолчанию для пула приложений (**Модель процесса** > **Удостоверение**) с **ApplicationPoolIdentity** на другое, убедитесь, что у нового удостоверения есть соответствующие разрешения для доступа к папке приложения, базе данных и другим необходимым ресурсам.
   
## <a name="deploy-the-app"></a>Развертывание приложения

Разверните приложение в папке, созданной в целевой системе IIS. Рекомендуемый механизм развертывания — [веб-развертывание](/iis/publish/using-web-deploy/introduction-to-web-deploy).

Убедитесь в том, что приложение, публикуемое для развертывания, не запущено. Во время выполнения приложения файлы в папке *publish* блокируются. Заблокированные файлы не могут быть перезаписаны. Чтобы освободить заблокированные файлы в развертывании, остановите пул приложений.

* Вручную в диспетчере служб IIS на сервере.
* С помощью веб-развертывания и ссылки на `Microsoft.NET.Sdk.Web` в файле проекта. Файл *app_offline.htm* помещается в корень каталога веб-приложения. Если файл присутствует, модуль ASP.NET Core корректно завершает работу приложения и обслуживает файл *app_offline.htm* во время развертывания. Дополнительные сведения см. в разделе [Справочник по конфигурации модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module#appofflinehtm).
* Используйте PowerShell для остановки и перезапуска пула приложений (требуется PowerShell 5 или более поздняя версия).
  ```PowerShell
  $webAppPoolName = 'APP_POOL_NAME'

  # Stop the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
      Stop-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Stopped
  }

  # Provide script commands here to deploy the app

  # Restart the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
      Start-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Started
  }
  ```

### <a name="web-deploy-with-visual-studio"></a>Веб-развертывание с помощью Visual Studio

Сведения о создании профиля публикации для веб-развертывания см. в статье [Профили публикации Visual Studio для развертывания приложений ASP.NET Core](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles). Если поставщик услуг размещения предоставляет профиль публикации или поддерживает его создание, скачайте этот профиль и импортируйте его с помощью диалогового окна **Публикация** в Visual Studio.

![Диалоговое окно "Публикация"](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a>Веб-развертывание вне Visual Studio

[Веб-развертывание](/iis/publish/using-web-deploy/introduction-to-web-deploy) можно также использовать вне Visual Studio с помощью командной строки. Дополнительные сведения см. в статье [Средство веб-развертывания](/iis/publish/using-web-deploy/use-the-web-deployment-tool).

### <a name="alternatives-to-web-deploy"></a>Альтернативы веб-развертыванию

Используйте любой из нескольких методов для перемещения приложения в систему размещения, такую как Xcopy, Robocopy или PowerShell. Пользователи Visual Studio могут применять [образцы публикации](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).

## <a name="browse-the-website"></a>Обзор веб-сайта

![Начальная страница IIS, загруженная в браузере Microsoft Edge.](index/_static/browsewebsite.png)

## <a name="data-protection"></a>Защита данных

Защита данных используется некоторым программным обеспечением промежуточного слоя ASP.NET, в том числе применяемым для проверки подлинности. Даже если API защиты данных не вызываются из пользовательского кода, защиту данных следует настроить с помощью скрипта развертывания или в пользовательском коде для создания постоянного хранилища ключей. Если защита данных не настроена, ключи хранятся в памяти и удаляются при перезапуске приложения.

Если набор ключей хранится в памяти, то при перезапуске приложения происходит следующее.

* Все токены проверки подлинности на основе форм становятся недействительными. 
* При выполнении следующего запроса пользователю требуется выполнить вход снова. 
* Любые данные, защищенные с помощью набора ключей, больше не могут быть расшифрованы.

Чтобы настроить защиту данных в службах IIS, следует использовать **один** из описанных ниже подходов.

### <a name="create-a-data-protection-registry-hive"></a>Создание куста реестра для защиты данных

Ключи защиты данных, используемые приложениями ASP.NET, хранятся в кустах реестра, являющихся внешними для приложений. Чтобы сохранять эти ключи для определенного приложения, нужно создать куст реестра для пула приложений, к которому относится данное приложение.

При автономной установке служб IIS, не поддерживающей веб-ферму, можно выполнить [скрипт PowerShell Provision-AutoGenKeys.ps1 для защиты данных](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) применительно к каждому пулу приложений, в который входит приложение ASP.NET Core. Этот скрипт создает специальный раздел в реестре HKLM, который будет доступен только для учетной записи рабочего процесса. Неактивные ключи шифруются с помощью API защиты данных с ключом компьютера.

В веб-ферме приложение можно настроить так, чтобы для хранения набора ключей защиты данных использовался UNC-путь. По умолчанию ключи защиты данных не шифруются. Разрешения на доступ к файлам в общей папке должны быть предоставлены только учетной записи Windows, от имени которой выполняется приложение. Помимо этого, для защиты неактивных ключей можно использовать сертификат X509. Рассмотрите возможность реализации механизма, позволяющего пользователям отправлять сертификаты: поместить сертификаты в хранилище доверенных сертификатов пользователя и обеспечивать их доступность на всех компьютерах, где выполняется приложение. Подробные сведения см. в разделе [Настройка защиты данных](xref:security/data-protection/configuration/overview).

### <a name="configure-the-iis-application-pool-to-load-the-user-profile"></a>Настройка пула приложений IIS для загрузки профиля пользователя

Этот параметр находится на странице **Дополнительные параметры** пула приложений в разделе **Модель процесса**. Задайте для параметра "Загрузить профиль пользователя" значение `True`. В результате ключи сохраняются в каталоге профиля пользователя и защищаются посредством API защиты данных с использованием ключа на уровне учетной записи пользователя, применяемой для пула приложений.

### <a name="use-the-file-system-as-a-key-ring-store"></a>Использование файловой системы в качестве хранилища набора ключей

Измените код приложения так, чтобы [в качестве хранилища набора ключей использовалась файловая система](xref:security/data-protection/configuration/overview). Используйте сертификат X509 для защиты набора ключей, причем это должен быть доверенный сертификат. Если сертификат является самозаверяющим, поместите его в доверенное корневое хранилище.

При использовании служб IIS в веб-ферме:

* используйте общую папку, которая доступна всем компьютерам;
* разверните сертификат X509 на каждом компьютере; настройте [защиту данных в коде](xref:security/data-protection/configuration/overview).

### <a name="set-a-machine-wide-policy-for-data-protection"></a>Задание политики защиты данных на уровне компьютера

Система защиты данных обеспечивает ограниченную поддержку задания [политики по умолчанию на уровне компьютера](xref:security/data-protection/configuration/machine-wide-policy) для всех приложений, использующих интерфейсы API защиты данных. Дополнительные сведения см. в документации по [защите данных](xref:security/data-protection/index).

## <a name="configuration-of-sub-applications"></a>Настройка дочерних приложений

Дочерние приложения, добавленные в корневое приложение, не должны включать в себя модуль ASP.NET Core в качестве обработчика. Если модуль добавляется в качестве обработчика в файл *web.config* дочернего приложения, то при попытке работы с этим приложением выводится ошибка 500.19 (внутренняя ошибка сервера) с указанием некорректного файла конфигурации. В приведенном ниже примере показано содержимое опубликованного файла *web.config* для дочернего приложения ASP.NET Core.

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

При размещении дочернего приложения не на основе ASP.NET Core в приложении ASP.NET Core, нужно явным образом удалить унаследованный обработчик из файла *web.config* дочернего приложения.

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore"/>
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

Дополнительные сведения о настройке модуля ASP.NET Core см. в статье [Введение в модуль ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) и в [справочнике по настройке модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module).

## <a name="configuration-of-iis-with-webconfig"></a>Настройка служб IIS с помощью файла web.config

Раздел **\<system.webServer>** файла *web.config* действует для тех компонентов IIS, которые относятся к конфигурации прокси-сервера. Если в службах IIS на уровне системы настроено динамическое сжатие, для приложения этот параметр можно отключить с помощью элемента **\<urlCompression>** в файле *web.config* приложения. Дополнительные сведения см. в [справочнике по настройке \<system.webServer>](https://docs.microsoft.com/iis/configuration/system.webServer/), [справочнике по настройке модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module) и статье [Использование модулей IIS с ASP.NET Core](xref:host-and-deploy/iis/modules). Если нужно задать переменные среды для отдельных приложений, выполняющихся в изолированных пулах приложений (такая возможность поддерживается в службах IIS 10.0 и более поздних версий), см. подраздел, посвященный *команде AppCmd.exe*, в разделе [Переменные среды \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) справочной документации по службам IIS.

## <a name="configuration-sections-of-webconfig"></a>Разделы конфигурации файла web.config

Разделы конфигурации приложений ASP.NET в *web.config* не используются в приложениях ASP.NET Core для конфигурации.

* **\<system.web>**
* **\<appSettings>**
* **\<connectionStrings >**
* **\<location>**

Для настройки приложений ASP.NET Core используются другие поставщики конфигураций. Дополнительные сведения см. в разделе [Конфигурация](xref:fundamentals/configuration/index).

## <a name="application-pools"></a>Пулы приложений

Если в одной системе размещается несколько веб-сайтов, приложения следует изолировать друг от друга, запуская каждое из них в отдельном пуле приложений. В диалоговом окне **Добавить веб-сайт** служб IIS такая конфигурация задана по умолчанию. Если указано **имя сайта**, оно автоматически переносится в текстовое поле **Пул приложений**. При добавлении веб-сайта создается пул приложений с именем сайта.

## <a name="application-pool-identity"></a>Удостоверение пула приложений

Учетная запись удостоверения пула приложений позволяет запускать приложение от имени уникальной учетной записи, не создавая домены или локальные учетные записи и не управляя ими. В службах IIS 8.0 и более поздних версий рабочий процесс администратора IIS (WAS) создает виртуальную учетную запись с именем нового пула приложений и по умолчанию выполняет рабочие процессы пула приложений с этой учетной записью. В консоли управления IIS в окне **Дополнительные параметры** для пула приложений должно быть выбрано **Удостоверение** **ApplicationPoolIdentity**.

![Диалоговое окно дополнительных параметров для пула приложений](index/_static/apppool-identity.png)

Процесс управления IIS создает в системе безопасности Windows безопасный идентификатор с именем пула приложений. С помощью этого удостоверения можно защищать ресурсы, но оно не представляет реальную учетную запись пользователя и не отображается в консоли управления Windows пользователя.

Если рабочему процессу IIS требуется предоставить доступ к приложению с повышенными правами, измените список управления доступом (ACL) для каталога, содержащего приложение.

1. Откройте проводник и перейдите к каталогу.

2. Щелкните каталог правой кнопкой мыши и выберите пункт **Свойства**.

3. На вкладке **Безопасность** нажмите кнопку **Изменить**, а затем — кнопку **Добавить**.

4. Нажмите кнопку **Размещение** и выберите систему.

5. В текстовом поле **Введите имена выбираемых объектов** введите **IIS AppPool\DefaultAppPool**.

   ![Диалоговое окно "Выбор пользователей или групп" для папки приложения](index/_static/select-users-or-groups-1.png)

6. Нажмите кнопку **Проверить имена**. Нажмите кнопку **ОК**.

   ![Диалоговое окно "Выбор пользователей или групп" для папки приложения](index/_static/select-users-or-groups-2.png)

Эту задачу можно также выполнить с помощью командной строки, используя средство **ICACLS**.

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Устранение неполадок ASP.NET Core в службах IIS](xref:host-and-deploy/iis/troubleshoot)
* [Справочник по общим ошибкам для службы приложений Azure и служб IIS с ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Введение в модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Справочник по конфигурации модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Использование модулей IIS с ASP.NET Core](xref:host-and-deploy/iis/modules)
* [Введение в ASP.NET Core](../index.md)
* [Официальный веб-сайт Microsoft IIS](https://www.iis.net/)
* [Библиотека Microsoft TechNet: Windows Server](https://docs.microsoft.com/windows-server/windows-server-versions)
