---
title: Использование нескольких сред в ASP.NET Core
author: rick-anderson
description: Сведения об управлении поведением приложений в разных средах в приложениях ASP.NET Core.
ms.author: riande
ms.date: 07/03/2018
uid: fundamentals/environments
ms.openlocfilehash: de3c3fd5a2f0e49366d9d5b4e992d0247bcab0e5
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577527"
---
# <a name="use-multiple-environments-in-aspnet-core"></a>Использование нескольких сред в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

ASP.NET Core настраивает поведение приложения в зависимости от среды выполнения с помощью переменной среды.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="environments"></a>Среды

ASP.NET Core считывает переменную среды `ASPNETCORE_ENVIRONMENT` при запуске приложения и сохраняет ее значение в свойстве [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname). Переменной `ASPNETCORE_ENVIRONMENT` можно присвоить любое значение, но платформа поддерживает [три значения](/dotnet/api/microsoft.aspnetcore.hosting.environmentname): [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging) и [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production). Если переменная `ASPNETCORE_ENVIRONMENT` не указана, используется значение по умолчанию `Production`.

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

Предыдущий код:

* Вызывает [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage), если `ASPNETCORE_ENVIRONMENT` имеет значение `Development`.
* Вызывает [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler), если переменная `ASPNETCORE_ENVIRONMENT` имеет одно из следующих значений:

    * `Staging`
    * `Production`
    * `Staging_2`

[Вспомогательная функция тега среды](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) использует значение `IHostingEnvironment.EnvironmentName` для включения или исключения разметки в элементе:

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

В ОС Windows и macOS регистр символов в переменных среды и их значениях не учитывается. В ОС Linux в переменных среды и их значениях **регистр символов по умолчанию учитывается**.

### <a name="development"></a>Разработка

В среде разработки могут быть включены функции, которые не должны быть доступны в рабочей среде. Например, шаблоны ASP.NET Core включают в среде разработки [страницу со сведениями об исключении для разработчика](xref:fundamentals/error-handling#the-developer-exception-page).

Среду для локального компьютера разработки можно задать в файле *Properties\launchSettings.json* проекта. Значения среды, заданные в файле *launchSettings.json*, переопределяют значения, заданные в системной среде.

В следующем коде JSON показаны три профиля из файла *launchSettings.json*:

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:54339/",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      }
    },
    "EnvironmentsSample": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:54340/"
    },
    "Kestrel Staging": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:51997/"
    }
  }
}
```

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> Свойство `applicationUrl` в *launchSettings.json* может задать список URL-адресов сервера. Для разделения URL-адресов в списке используется точка с запятой:
>
> ```json
> "EnvironmentsSample": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```

::: moniker-end

Когда приложение запускается с помощью команды [dotnet run](/dotnet/core/tools/dotnet-run), используется первый профиль с атрибутом `"commandName": "Project"`. Значение `commandName` определяет запускаемый веб-сервер. `commandName` может иметь одно из следующих значений:

* IIS Express
* IIS
* Project (запускается Kestrel)

Когда приложение запускается с помощью команды [dotnet run](/dotnet/core/tools/dotnet-run), выполняются указанные ниже действия:

* Считывается файл *launchSettings.json*, если он доступен. Параметры `environmentVariables` в файле *launchSettings.json* переопределяют переменные среды.
* Отображается среда размещения.

Ниже представлены выходные данные приложения, запущенного с помощью команды [dotnet run](/dotnet/core/tools/dotnet-run):

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

Вкладка **Отладка** в свойствах проекта Visual Studio предоставляет графический пользовательский интерфейс для изменения файла *launchSettings.json*.

![Задание переменных среды в свойствах проекта](environments/_static/project-properties-debug.png)

Для вступления в силу изменений, внесенных в профили проекта, может потребоваться перезапуск веб-сервера. Чтобы сервер Kestrel обнаружил изменения, внесенные в среду, его необходимо перезапустить.

> [!WARNING]
> В файле *launchSettings.json* не должны храниться секреты. Для хранения секретов во время разработки в локальной среде можно использовать [средство Secret Manager](xref:security/app-secrets).

При использовании [Visual Studio Code](https://code.visualstudio.com/) переменные среды можно задавать в файле *.vscode/launch.json*. В следующем примере показано, как присвоить среде значение `Development`:

```json
{
   "version": "0.2.0",
   "configurations": [
        {
            "name": ".NET Core Launch (web)",

            ... additional VS Code configuration settings ...

            "env": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    ]
}
```

Файл *.vscode/launch.json* в проекте не читается при запуске приложения с помощью команды `dotnet run` так же, как файл *Properties/launchSettings.json*. При запуске приложения в среде разработки без файла *launchSettings.json* укажите среду с помощью переменной среды или задайте аргумент командной строки как команду `dotnet run`.

### <a name="production"></a>Производство

Конфигурация рабочей среды должна обеспечивать максимальный уровень безопасности, производительности и надежности приложений. Некоторые общие параметры, отличные от разработки:

* кэширование;
* ресурсы на стороне клиента объединяются в пакеты, уплотняются и могут предоставляться из сети CDN;
* страницы с сообщениями об ошибках диагностики отключены;
* включены страницы с понятными пользователям сообщениями об ошибках;
* включены средства ведения журналов и мониторинга в рабочей среде, например [Application Insights](/azure/application-insights/app-insights-asp-net-core).

## <a name="set-the-environment"></a>Указание среды

Часто бывает полезным указать определенную среду для тестирования. Если среда не указана, по умолчанию используется среда `Production`, в которой большинство функций отладки отключено. Способ указания среды зависит от операционной системы.

### <a name="azure-app-service"></a>Служба приложений Azure

Чтобы установить среду в [службе приложений Azure](https://azure.microsoft.com/services/app-service/), выполните следующие действия:

1. Выберите приложение из колонки **Службы приложений**.
1. В группе **ПАРАМЕТРЫ** выберите колонку **Параметры приложения**.
1. В области **Параметры приложения** выберите **Добавить новый параметр**.
1. В поле **Введите имя** укажите `ASPNETCORE_ENVIRONMENT`. В поле **Введите значение** укажите среду (например, `Staging`).
1. Установите флажок **Параметр слота**, если нужно, чтобы параметр среды оставался в текущем слоте при замене слота развертывания. Дополнительные сведения см. в разделе [Документация по Azure. Какие параметры заменяются?](/azure/app-service/web-sites-staged-publishing).
1. Нажмите **Сохранить** в верхней части колонки.

Служба приложений Azure автоматически перезапускает приложение после добавления, изменения или удаления параметра приложения (переменной среды) на портале Azure.

### <a name="windows"></a>Windows

Если приложение запускается с помощью команды [dotnet run](/dotnet/core/tools/dotnet-run), то, чтобы задать переменную `ASPNETCORE_ENVIRONMENT` для текущего сеанса, используйте следующие команды:

**Командная строка**

```console
set ASPNETCORE_ENVIRONMENT=Development
```

**PowerShell**

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Эти команды действуют только для текущего окна. Когда окно закрывается, для параметра `ASPNETCORE_ENVIRONMENT` восстанавливается значение по умолчанию или значение, заданное на компьютере.

Чтобы задать это значение в Windows на глобальном уровне, используйте один из следующих подходов.

* Откройте **Панель управления** > **Система** > **Дополнительные параметры системы** и добавьте или измените значение `ASPNETCORE_ENVIRONMENT`:

  ![Дополнительные параметры системы](environments/_static/systemsetting_environment.png)

  ![Переменная среды ASPNET Core](environments/_static/windows_aspnetcore_environment.png)

* Откройте командную строку администратора и выполните команду `setx` либо откройте командную строку администратора PowerShell и используйте `[Environment]::SetEnvironmentVariable`:

  **Командная строка**

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  Параметр `/M` указывает на установку переменной среды на уровне системы. Если параметр `/M` не используется, переменная среды задается для учетной записи пользователя.

  **PowerShell**

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  Значение параметра `Machine` указывает на установку переменной среды на уровне системы. При изменении значения параметра на `User` переменная среды задается для учетной записи пользователя.

Если переменная среды `ASPNETCORE_ENVIRONMENT` задана глобально, она действует для `dotnet run` в любом окне командной строки, открываемом после установки значения.

**web.config**

Сведения об установке переменной среды `ASPNETCORE_ENVIRONMENT` в файле *web.config* см. в разделе *Настройка переменной среды* в <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>. Если переменная среды `ASPNETCORE_ENVIRONMENT` задана в файле *web.config*, ее значение переопределяет значение на уровне системы.

**Пул приложений IIS**

Чтобы задать переменную среды `ASPNETCORE_ENVIRONMENT` для приложения, выполняющегося в изолированном пуле приложений (такая возможность поддерживается в службах IIS 10.0 и более поздних версий), см. подраздел, посвященный команде *AppCmd.exe*, в разделе [Переменные среды &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe). Если переменная среды `ASPNETCORE_ENVIRONMENT` задана для пула приложений, ее значение переопределяет значение на уровне системы.

> [!IMPORTANT]
> При размещении приложения в службах IIS и добавлении или изменении переменной среды `ASPNETCORE_ENVIRONMENT` используйте один из следующих подходов по применению нового значения в приложении.
>
> * Из командной строки выполните команду `net stop was /y`, за которой следует `net start w3svc`.
> * Перезапустите сервер.

### <a name="macos"></a>macOS

Задать текущую среду в macOS можно в командной строке при запуске приложения:

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

Также можно задать среду с помощью команды `export` до запуска приложения:

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

Переменные среды на уровне компьютера задаются в файле *BASHRC* или *BASH_PROFILE*. Измените файл в любом текстовом редакторе. Добавьте следующий оператор:

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a>Linux

В дистрибутивах Linux используйте команду `export` в командной строке для значений переменных на уровне сеанса или в файле *BASH_PROFILE* для значений среды на уровне компьютера.

### <a name="configuration-by-environment"></a>Конфигурация для разных сред

Для загрузки конфигурации среды мы рекомендуем:

* Файлы *appsettings* (*appsettings.&lt;<Environment>&gt;.json). См. раздел [Конфигурация: поставщик конфигурации файлов](xref:fundamentals/configuration/index#file-configuration-provider).
* Переменные среды (заданные в каждой системе, где размещено приложение). См. разделы [Конфигурация: поставщик конфигурации файлов](xref:fundamentals/configuration/index#file-configuration-provider) и [Безопасное хранение секретов приложения во время разработки: переменные среды](xref:security/app-secrets#environment-variables).
* Менеджер секретов (только в среде разработки). См. раздел <xref:security/app-secrets>.

## <a name="environment-based-startup-class-and-methods"></a>Класс Startup и его методы для разных сред

### <a name="startup-class-conventions"></a>Соглашения о классе Startup

При запуске приложения ASP.NET Core [класс Startup](xref:fundamentals/startup) выполняет его начальную загрузку. Приложение может определять отдельные классы `Startup` для различных сред (например, `StartupDevelopment`), при этом подходящий класс `Startup` выбирается во время выполнения. Класс, у которого суффикс имени соответствует текущей среде, получает приоритет. Если соответствующий класс `Startup{EnvironmentName}` не найден, используется класс `Startup`.

Чтобы реализовать классы `Startup` на основе среды, создайте класс `Startup{EnvironmentName}` для каждой используемой среды и резервный класс `Startup`:

```csharp
// Startup class to use in the Development environment
public class StartupDevelopment
{
    public void ConfigureServices(IServiceCollection services)
    {
        ...
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        ...
    }
}

// Startup class to use in the Production environment
public class StartupProduction
{
    public void ConfigureServices(IServiceCollection services)
    {
        ...
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        ...
    }
}

// Fallback Startup class
// Selected if the environment doesn't match a Startup{EnvironmentName} class
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        ...
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        ...
    }
}
```

Используйте перегрузку [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup), которая принимает имя сборки:

::: moniker range=">= aspnetcore-2.1"

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args)
{
    var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

    return WebHost.CreateDefaultBuilder(args)
        .UseStartup(assemblyName);
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```csharp
public static void Main(string[] args)
{
    CreateWebHost(args).Run();
}

public static IWebHost CreateWebHost(string[] args)
{
    var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

    return WebHost.CreateDefaultBuilder(args)
        .UseStartup(assemblyName)
        .Build();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

        var host = new WebHostBuilder()
            .UseStartup(assemblyName)
            .Build();

        host.Run();
    }
}
```

::: moniker-end

### <a name="startup-method-conventions"></a>Соглашения о методах Startup

Методы [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) и [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) поддерживают версии для конкретных сред в формате `Configure<EnvironmentName>` и `Configure<EnvironmentName>Services`:

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,51)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
* [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)
