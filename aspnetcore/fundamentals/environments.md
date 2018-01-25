---
title: "Работа с несколькими средами в ASP.NET Core"
author: rick-anderson
description: "Узнайте, как ASP.NET Core предоставляет поддержку для управления поведением приложения в нескольких средах."
ms.author: riande
manager: wpickett
ms.date: 12/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: 60a1543ce11d08490e6df0eb84f980672ecfe672
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="working-with-multiple-environments"></a>Работа с несколькими средами

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

ASP.NET Core обеспечивает поддержку установки поведение приложения во время выполнения с переменными среды.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="environments"></a>Среды

ASP.NET Core считывает переменной среды `ASPNETCORE_ENVIRONMENT` при запуске приложения и сохраняет это значение в [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName). `ASPNETCORE_ENVIRONMENT`можно задать любое значение, но [три значения](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) поддерживаются платформой: [разработки](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [промежуточной](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), и [рабочей](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0). Если `ASPNETCORE_ENVIRONMENT` не задано, то по умолчанию `Production`.

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet)]

Предыдущий код:

* Вызовы [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) и [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) при `ASPNETCORE_ENVIRONMENT` равно `Development`.
* Вызовы [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) при значение `ASPNETCORE_ENVIRONMENT` задано одно из следующих:

    * `Staging`
    * `Production`
    * `Staging_2`

[Вспомогательный тега среды ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) использует значение `IHostingEnvironment.EnvironmentName` для включения или исключения разметки в элементе:

[!code-html[Main](environments/sample/WebApp1/Pages/About.cshtml)]

Обратите внимание: В Windows и macOS, переменные среды и значения не учитывается регистр. Переменные среды для Linux и значения являются **регистра** по умолчанию.

### <a name="development"></a>Разработка

В среде разработки можно включить функции, которые не должны предоставляться в рабочей среде. Например, включить шаблоны ASP.NET Core [страницы разработчик исключений](xref:fundamentals/error-handling#the-developer-exception-page) в среде разработки.

Среда для локального компьютера может быть задано в *Properties\launchSettings.json* файла проекта. Задание значений среды в *launchSettings.json* переопределяют значения, заданные в среде системы.

Следующий код XML показывает три профиля из *launchSettings.json* файла:

[!code-xml[Main](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

При запуске приложения с `dotnet run`, первый профиль с `"commandName": "Project"` будет использоваться. Значение `commandName` указывает веб-сервера для запуска. `commandName`может принимать одно из:

* IIS Express
* IIS
* Проект (которое запускает Kestrel)

При запуске приложения с `dotnet run`:

* *launchSettings.json* считывается если он доступен. `environmentVariables`параметры в *launchSettings.json* переопределение переменных среды.
* Среда размещения отображается.


Ниже показано приложение к работе с `dotnet run`:
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

Visual Studio **отладки** вкладка предоставляет графический интерфейс пользователя для редактирования *launchSettings.json* файла:

![Переменные среды параметр свойства проекта](environments/_static/project-properties-debug.png)

Изменения, внесенные в проект профилей могут не вступают в силу только после перезапуска веб-сервера. Необходимо перезапустить kestrel, прежде чем он обнаруживает изменения, внесенные в своей среде.

>[!WARNING]
> *launchSettings.json* не следует хранить секреты. [Секрет диспетчера](xref:security/app-secrets) может использоваться для хранения конфиденциальных данных для локальной разработки.

### <a name="production"></a>Производство

В рабочей среде должны быть настроены для повышения безопасности, производительности и надежности приложений. Ниже перечислены некоторые общие параметры, которые, возможно, в производственную среду, будут отличаться от разработки.

* Кэширование.
* Клиентские ресурсы объединяются в один пакет, уменьшено и потенциально полученном из CDN.
* Страницы диагностики ошибок отключена.
* Страницы ошибок включено.
* Ведение журнала производства и мониторинг включен. Например [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/).

## <a name="setting-the-environment"></a>Настройка среды

Часто полезно задать конкретную среду для тестирования. Если среда не настроена, то по умолчанию `Production` отключающее большинство функций отладки.

Метод для настройки среды, зависит от операционной системы.

### <a name="azure"></a>Azure

Для службы приложения Azure:

* Выберите **параметры приложения** колонку.
* Добавить ключ и значение в **параметры приложения**.


### <a name="windows"></a>Windows
Чтобы задать `ASPNETCORE_ENVIRONMENT` для текущего сеанса, если приложение запускается с помощью `dotnet run`, используются следующие команды

**командная строка;**
```
set ASPNETCORE_ENVIRONMENT=Development
```
**PowerShell**
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Эти команды вступают в силу только для текущего окна. При закрытии окна параметр ASPNETCORE_ENVIRONMENT возвращается по умолчанию или значение машины. Чтобы установить значение глобально при открытии Windows **панели управления** > **системы** > **расширенных параметров системы** и добавление или изменение `ASPNETCORE_ENVIRONMENT` значение.

![Дополнительные свойства системы](environments/_static/systemsetting_environment.png)

![Переменная среды ASPNET Core](environments/_static/windows_aspnetcore_environment.png)


**web.config**

В разделе *задание переменных среды* раздел [Справочник по конфигурации модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) раздела.

**На пул приложений IIS**

Для установки переменных среды для отдельных приложений, выполняющихся в изолированной пулы приложений (поддерживается в IIS 10.0 +), в разделе *команды AppCmd.exe* раздел [переменных среды \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) раздела.

### <a name="macos"></a>MacOS
Установка текущей среды для macOS можно сделать в строки при запуске приложения;

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
или с помощью `export` задайте его перед запуском приложения.

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
Переменные среды уровня машины задаются *.bashrc* или *.bash_profile* файла. Измените файл, используя любой текстовый редактор и добавьте следующий оператор.

```
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a>Linux
Дистрибутивы Linux, используйте `export` команды из командной строки для параметров переменных на основе сеансов и *bash_profile* файла параметров уровня среды компьютера.

### <a name="configuration-by-environment"></a>Конфигурация для разных сред

В разделе [конфигурации средой](xref:fundamentals/configuration/index#configuration-by-environment) для получения дополнительной информации.

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a>Класс запуска и методы на основе среды

При запуске приложения ASP.NET Core [класс запуска](xref:fundamentals/startup) загружает приложение. Если класс `Startup{EnvironmentName}` существует, что будет вызван для этого `EnvironmentName`:

[!code-csharp[Main](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

Примечание: Вызов [WebHostBuilder.UseStartup<TStartup> ](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) переопределяет разделы конфигурации.

[Настройка](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) и [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) поддержки конкретных версий среды формы `Configure{EnvironmentName}` и `Configure{EnvironmentName}Services`:

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Запуск приложения](xref:fundamentals/startup)
* [Конфигурация](xref:fundamentals/configuration/index)
* [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
