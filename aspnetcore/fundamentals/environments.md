---
title: Использование нескольких сред в ASP.NET Core
author: rick-anderson
description: Сведения о поддержке управления поведением приложений в разных средах в ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 12/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/environments
ms.openlocfilehash: 2c8441db527203aeea516073dae3bc335c335565
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/07/2018
ms.locfileid: "33840961"
---
# <a name="use-multiple-environments-in-aspnet-core"></a>Использование нескольких сред в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

ASP.NET Core дает возможность настраивать поведение приложения во время выполнения с помощью переменных среды.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="environments"></a>Среды

ASP.NET Core считывает переменную среды `ASPNETCORE_ENVIRONMENT` при запуске приложения и сохраняет ее значение в свойстве [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName). Переменной `ASPNETCORE_ENVIRONMENT` можно присвоить любое значение, но платформа поддерживает [три значения](/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0): [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0) и [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0). Если переменная `ASPNETCORE_ENVIRONMENT` не задана, по умолчанию используется значение `Production`.

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet)]

Предыдущий код:

* Вызывает [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) и [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_), если переменная `ASPNETCORE_ENVIRONMENT` имеет значение `Development`.
* Вызывает [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_), если переменная `ASPNETCORE_ENVIRONMENT` имеет одно из следующих значений:

    * `Staging`
    * `Production`
    * `Staging_2`

[Вспомогательная функция тега среды](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) использует значение `IHostingEnvironment.EnvironmentName` для включения или исключения разметки в элементе:

[!code-html[](environments/sample/WebApp1/Pages/About.cshtml)]

Примечание. В ОС Windows и macOS регистр символов в переменных среды и их значениях не учитывается. В ОС Linux в переменных среды и их значениях **регистр символов по умолчанию учитывается**.

### <a name="development"></a>Разработка

В среде разработки могут быть включены функции, которые не должны быть доступны в рабочей среде. Например, шаблоны ASP.NET Core включают в среде разработки [страницу со сведениями об исключении для разработчика](xref:fundamentals/error-handling#the-developer-exception-page).

Среду для локального компьютера разработки можно задать в файле *Properties\launchSettings.json* проекта. Значения среды, заданные в файле *launchSettings.json*, переопределяют значения, заданные в системной среде.

В следующем коде JSON показаны три профиля из файла *launchSettings.json*:

[!code-json[](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

::: moniker range=">= aspnetcore-2.1"
> [!NOTE]
> Свойство `applicationUrl` в *launchSettings.json* может задать список URL-адресов сервера. Для разделения URL-адресов в списке используется точка с запятой:
>
> ```json
> "WebApplication1": {
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
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

Вкладка **Отладка** в Visual Studio предоставляет графический пользовательский интерфейс для изменения файла *launchSettings.json*:

![Задание переменных среды в свойствах проекта](environments/_static/project-properties-debug.png)

Для вступления в силу изменений, внесенных в профили проекта, может потребоваться перезапуск веб-сервера. Чтобы сервер Kestrel обнаружил изменения, внесенные в среду, его необходимо перезапустить.

>[!WARNING]
> В файле *launchSettings.json* не должны храниться секреты. Для хранения секретов во время разработки в локальной среде можно использовать [средство Secret Manager](xref:security/app-secrets).

### <a name="production"></a>Производство

Конфигурация рабочей среды должна обеспечивать максимальный уровень безопасности, производительности и надежности приложений. Некоторые общие параметры, отличные от разработки:

* кэширование;
* ресурсы на стороне клиента объединяются в пакеты, уплотняются и могут предоставляться из сети CDN;
* страницы с сообщениями об ошибках диагностики отключены;
* включены страницы с понятными пользователям сообщениями об ошибках;
* включены средства ведения журналов и мониторинга в рабочей среде, например [Application Insights](/azure/application-insights/app-insights-asp-net-core).

## <a name="setting-the-environment"></a>Указание среды

Часто бывает полезным указать определенную среду для тестирования. Если среда не указана, по умолчанию используется среда `Production`, в которой большинство функций отладки отключено.

Способ указания среды зависит от операционной системы.

### <a name="azure"></a>Azure

Для службы приложений Azure:

* Выберите колонку **Параметры приложения**.
* Добавьте ключ и значение в поле **Параметры приложения**.


### <a name="windows"></a>Windows
Если приложение запускается с помощью команды [dotnet run](/dotnet/core/tools/dotnet-run), то, чтобы задать переменную `ASPNETCORE_ENVIRONMENT` для текущего сеанса, используйте следующие команды

**командная строка;**
```
set ASPNETCORE_ENVIRONMENT=Development
```
**PowerShell**
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Эти команды действуют только для текущего окна. Когда окно закрывается, для переменной ASPNETCORE_ENVIRONMENT восстанавливается значение по умолчанию или значение, заданное на компьютере. Чтобы задать это значение в Windows на глобальном уровне, откройте **Панель управления** > **Система** > **Дополнительные параметры системы** и добавьте или удалите значение `ASPNETCORE_ENVIRONMENT`.

![Дополнительные параметры системы](environments/_static/systemsetting_environment.png)

![Переменная среды ASPNET Core](environments/_static/windows_aspnetcore_environment.png)


**web.config**

См. раздел *Задание переменных среды* в статье [Справочник по настройке модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).

**Пул приложений IIS**

Чтобы задать переменные среды для отдельных приложений, выполняющихся в изолированных пулах приложений (такая возможность поддерживается в службах IIS 10.0 и более поздних версий), см. подраздел, посвященный *команде AppCmd.exe*, в разделе [Переменные среды \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe).

### <a name="macos"></a>macOS
Задать текущую среду в macOS можно в командной строке при запуске приложения;

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
или с помощью команды `export` перед запуском приложения.

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
Переменные среды на уровне компьютера задаются в файле *BASHRC* или *BASH_PROFILE*. Измените этот файл в любом текстовом редакторе, добавив следующий оператор.

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a>Linux
В дистрибутивах Linux используйте команду `export` в командной строке для значений переменных на уровне сеанса или в файле *BASH_PROFILE* для значений среды на уровне компьютера.

### <a name="configuration-by-environment"></a>Конфигурация для разных сред

Дополнительные сведения см. в разделе [Конфигурация для разных сред](xref:fundamentals/configuration/index#configuration-by-environment).

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a>Класс Startup и его методы для разных сред

При запуске приложения ASP.NET Core [класс Startup](xref:fundamentals/startup) выполняет его начальную загрузку. Если класс `Startup{EnvironmentName}` существует, он вызывается для данной среды `EnvironmentName`:

[!code-csharp[](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

Примечание. Вызов [WebHostBuilder.UseStartup<TStartup>](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) переопределяет разделы конфигурации.

Методы [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) и [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) поддерживают версии для конкретных сред в формате `Configure{EnvironmentName}` и `Configure{EnvironmentName}Services`:

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Запуск приложения](xref:fundamentals/startup)
* [Конфигурация](xref:fundamentals/configuration/index)
* [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
