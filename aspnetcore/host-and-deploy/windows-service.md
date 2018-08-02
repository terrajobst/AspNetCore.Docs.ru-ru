---
title: Размещение ASP.NET Core в службе Windows
author: guardrex
description: Узнайте, как разместить приложение ASP.NET Core в службе Windows.
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 4fd0cc881eff3b1bbdfdf51e223d0fd42051c31d
ms.sourcegitcommit: 516d0645c35ea784a3ae807be087ae70446a46ee
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/27/2018
ms.locfileid: "39320743"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Размещение ASP.NET Core в службе Windows

Авторы: [Люк Латэм](https://github.com/guardrex) (Luke Latham) и [Tom Dykstra](https://github.com/tdykstra) (Том Дайкстра)

Приложение ASP.NET Core можно разместить в Windows без использования IIS в качестве [службы Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Размещенное в службе Windows приложение может автоматически запускаться после перезагрузок и аварийных завершений работы, не требуя вмешательства оператора.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="get-started"></a>Начало работы

Чтобы настроить существующий проект ASP.NET Core для запуска в службе, требуется следующий минимальный набор изменений:

1. В файле проекта:

   1. Подтвердите наличие идентификатора среды выполнения или добавьте его в **\<PropertyGroup >**, где содержится требуемая версия платформы:

      ::: moniker range=">= aspnetcore-2.1"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="= aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="< aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp1.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

   1. Добавьте ссылку на пакет для [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).

1. Внесите следующие изменения в `Program.Main`:

   * Вызовите [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) вместо `host.Run`.

   * Вызовите [UseContentRoot](xref:fundamentals/host/web-host#content-root) и используйте путь к расположению публикации приложения вместо `Directory.GetCurrentDirectory()`.

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,12)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. Опубликуйте приложение. Используйте команду [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) или [профиль публикации Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles).

   Чтобы опубликовать пример приложения из командной строки, выполните следующую команду в окне консоли из папки проекта:

   ```console
   dotnet publish --configuration Release
   ```

1. Используйте программу командной строки [sc.exe](https://technet.microsoft.com/library/bb490995), чтобы создать службу. Значение `binPath` обозначает путь к исполняемому файлу приложения, включая имя самого файла. **Требуется пробел между знаком равенства и кавычками, с которых начинается путь.**

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   Для создания службы, опубликованной в папке проекта, используйте путь к папке *publish*. В следующем примере служба:

   * называется **MyService**;
   * публикуется в папке *c:\\my_services\\AspNetCoreService\\bin\\Release\\&lt;TARGET_FRAMEWORK&gt;\\publish*;
   * представлена исполняемым файлом приложения с именем *AspNetCoreService.exe*.

   Откройте командную оболочку с правами администратора и выполните следующую команду:

   ```console
   sc create MyService binPath= "c:\my_services\AspNetCoreService\bin\Release\<TARGET_FRAMEWORK>\publish\AspNetCoreService.exe"
   ```
   
   > [!IMPORTANT]
   > Убедитесь, что между аргументом `binPath=` и его значением есть пробел.
   
   Чтобы опубликовать и запустить службу из другой папки:
   
   1. Используйте параметр [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) команды `dotnet publish`.
   1. Создайте службу с помощью команды `sc.exe`, используя путь к папке выходных данных. Включите имя исполняемого файла службы в путь, передаваемый в `binPath`.

1. Запустите службу с помощью команды `sc start <SERVICE_NAME>`.

   Чтобы запустить пример службы приложения, используйте следующую команду:

   ```console
   sc start MyService
   ```

   Команде потребуется несколько секунд, чтобы запустить службу.

1. Команду `sc query <SERVICE_NAME>` можно использовать для проверки состояния службы:

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   Чтобы проверить состояние примера службы приложения, используйте следующую команду:

   ```console
   sc query MyService
   ```

1. Если служба находится в состоянии `RUNNING` и является веб-приложением, найдите приложение по его пути (по умолчанию `http://localhost:5000`, который перенаправляет на `https://localhost:5001` при использовании [ПО промежуточного слоя перенаправления на HTTPS](xref:security/enforcing-ssl)).

   Чтобы получить пример службы приложений, найдите приложение по адресу `http://localhost:5000`.

1. Остановите службу с помощью команды `sc stop <SERVICE_NAME>`.

   Чтобы остановить пример службы приложения, используйте следующую команду:

   ```console
   sc stop MyService
   ```

1. После небольшой задержки для остановки службы удалите службу с помощью команды `sc delete <SERVICE_NAME>`.

   Проверьте состояние примера службы приложений:

   ```console
   sc query MyService
   ```

   Когда пример службы приложений находится в состоянии `STOPPED`, используйте следующую команду для удаления примера службы приложений:

   ```console
   sc delete MyService
   ```

## <a name="provide-a-way-to-run-outside-of-a-service"></a>Обеспечение возможности запуска за пределами службы

Тестирование и отладку проще выполнять при работе вне службы. Поэтому разработчики обычно добавляют код, который вызывает `RunAsService` только при соблюдении определенных условий. Например, приложение может выполняться как консольное приложение с аргументом командной строки `--console` или при присоединении отладчика:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

Так как для конфигурации ASP.NET Core требуется пара имя-значение для аргументов командной строки, параметр `--console` удаляется до передачи аргументов в [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).

> [!NOTE]
> `isService` не передается из `Main` в `CreateWebHostBuilder`, так как для правильного [тестирования интеграции](xref:test/integration-tests) сигнатура `CreateWebHostBuilder` должна иметь значение `CreateWebHostBuilder(string[])`.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a>Обработка событий остановки и запуска

Для обработки событий [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) и [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) внесите следующие дополнительные изменения:

1. Создайте класс, производный от [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. Создайте метод расширения для [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost), который передает пользовательский параметр `WebHostService` в [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. В `Program.Main` измените вызов [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) на вызов нового метода расширения `RunAsCustomService`:

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=14)]

   > [!NOTE]
   > `isService` не передается из `Main` в `CreateWebHostBuilder`, так как для правильного [тестирования интеграции](xref:test/integration-tests) сигнатура `CreateWebHostBuilder` должна иметь значение `CreateWebHostBuilder(string[])`.

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

Если пользовательский код `WebHostService` обращается к службе путем внедрения зависимости (например, к средству ведения журнала), ее можно получить из свойства [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services):

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Сценарии использования прокси-сервера и подсистемы балансировки нагрузки

Для служб, которые взаимодействуют с запросами из Интернета или корпоративной сети и размещаются за прокси-сервером или подсистемой балансировки нагрузки, может потребоваться дополнительная настройка. Дополнительные сведения см. в разделе <xref:host-and-deploy/proxy-load-balancer>.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Конфигурация конечных точек Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (включает конфигурацию HTTPS и поддержку SNI)
* <xref:fundamentals/host/web-host>
