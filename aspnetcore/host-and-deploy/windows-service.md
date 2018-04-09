---
title: Узел ASP.NET Core в службе Windows
author: tdykstra
description: Узнайте, как для размещения приложения ASP.NET Core в службе Windows.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: b0b27f274de1ca88b20bf582127132527b553ce0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Узел ASP.NET Core в службе Windows

Автор: [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra)

Рекомендуется размещать приложения ASP.NET Core в Windows без с помощью служб IIS для запуска в [службы Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Когда в качестве службы Windows, приложение может автоматически запуск после перезагрузки и аварийно завершает работу без необходимости вмешательства человека.

[Просмотреть или скачать пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([описание скачивания](xref:tutorials/index#how-to-download-a-sample)). Инструкции по запуску примера приложения, см. пример *README.md* файла.

## <a name="prerequisites"></a>Предварительные требования

* Приложение должно выполняться в среде выполнения .NET Framework. В *.csproj* файла, укажите соответствующие значения для [TargetFramework](/nuget/schema/target-frameworks) и [RuntimeIdentifier](/dotnet/articles/core/rid-catalog). Ниже приведен пример:

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  При создании проекта в Visual Studio, используйте **основное приложение ASP.NET (.NET Framework)** шаблона.

* Если приложение получает запросы из Интернета (не только из внутренней сети), он должен использовать [HTTP.sys](xref:fundamentals/servers/httpsys) веб-сервера (ранее называвшейся [WebListener](xref:fundamentals/servers/weblistener) для приложений ASP.NET Core 1.x) вместо [Kestrel](xref:fundamentals/servers/kestrel). IIS рекомендуется для использования в качестве обратного прокси-сервера с Kestrel для развертываний edge. Дополнительные сведения см. в статье [Использование Kestrel с обратным прокси-сервером](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

## <a name="get-started"></a>Начало работы

В этом разделе описывается минимальный набор изменений, необходимых для настройки существующего проекта ASP.NET Core для запуска в службе.

1. Установка пакета NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).

2. Внесите следующие изменения в `Program.Main`:

   * Вызовите `host.RunAsService` вместо `host.Run`.

   * Если код вызывает `UseContentRoot`, используйте путь в расположение публикации вместо `Directory.GetCurrentDirectory()`.

   #### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   #### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   * * *

3. Публикация приложения в папке. Используйте [публикации dotnet](/dotnet/articles/core/tools/dotnet-publish) или [профиль публикации Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) , публикует в папку.

4. Тестирование путем создания и запуска службы.

   Откройте командную строку с правами администратора для использования [sc.exe](https://technet.microsoft.com/library/bb490995) средство командной строки для создания и запуска службы. Если служба называется MyService, опубликованы `c:\svc`, и с именем AspNetCoreService, команды, являются:

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   `binPath` Значение — это путь к исполняемому файлу приложения, включая имя исполняемого файла.

   ![Окно консоли, создание и запуск примера](windows-service/_static/create-start.png)

   После завершения этих команд, перейдите к тому же пути, что при запуске в качестве консольного приложения (по умолчанию `http://localhost:5000`):

   ![Выполняемые в службе](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a>Укажите способ запуска за пределами службы

Проще тестирования и отладки при работе вне службы, поэтому обычно добавьте код, вызывающий `RunAsService` только при определенных условиях. Например, приложение может выполняться как консольное приложение с `--console` аргумент командной строки или при присоединении отладчика:

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

* * *
## <a name="handle-stopping-and-starting-events"></a>Остановка и запуск событий

Для обработки `OnStarting`, `OnStarted`, и `OnStopping` события, выполните следующие дополнительные изменения:

1. Создайте класс, производный от `WebHostService`:

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. Создание метода расширения для `IWebHost` , проходящего пользовательский `WebHostService` для `ServiceBase.Run`:

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. В `Program.Main`, вызовите новый метод расширения, `RunAsCustomService`, а не `RunAsService`:

   #### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   #### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   * * *
Если пользовательский `WebHostService` кода требует службы из внедрения зависимости (например, средства ведения журнала), получить его из `Services` свойства `IWebHost`:

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Прокси-сервер и сценарии подсистемы балансировки нагрузки

Служб, взаимодействовать с запросы из Интернета или корпоративной сети и через прокси или Подсистема балансировки нагрузки может потребовать дополнительной настройки. Дополнительные сведения см. в разделе [Настройка ASP.NET Core для работы с прокси-серверы и подсистем балансировки нагрузки](xref:host-and-deploy/proxy-load-balancer).

## <a name="acknowledgments"></a>Благодарности

В этой статье было написано с помощью опубликованных источников:

* [Размещение ASP.NET Core в качестве службы Windows](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [Как разместить основные ASP.NET в службе Windows](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
