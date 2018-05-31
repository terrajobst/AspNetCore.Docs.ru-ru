---
title: Размещение ASP.NET Core в службе Windows
author: rick-anderson
description: Узнайте, как разместить приложение ASP.NET Core в службе Windows.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: 29f83ee585c73aeb57a09f70ea8e28650c05ce69
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/14/2018
ms.locfileid: "34153533"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Размещение ASP.NET Core в службе Windows

Автор: [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra)

Чтобы разместить приложение ASP.NET Core в Windows, не используя IIS, рекомендуем выполнить его в [службе Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Размещенное в службе Windows приложение может автоматически запускаться после перезагрузок и аварийных завершений работы, не требуя вмешательства оператора.

[Просмотреть или скачать пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([описание скачивания](xref:tutorials/index#how-to-download-a-sample)). Инструкции по запуску примера приложения содержатся в файле *README.md* для этого примера.

## <a name="prerequisites"></a>Предварительные требования

* Приложение должно работать в среде выполнения .NET Framework. В файле *.csproj* укажите соответствующие значения для параметров [TargetFramework](/nuget/schema/target-frameworks) (Целевая среда выполнения) и [RuntimeIdentifier](/dotnet/articles/core/rid-catalog) (Идентификатор среды выполнения). Ниже приведен пример:

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  При создании нового проекта в Visual Studio используйте шаблон **приложения ASP.NET Core (.NET Framework)**.

* Если это приложение получает запросы из Интернета (а не только из внутренней сети), для него потребуется веб-сервер [HTTP.sys](xref:fundamentals/servers/httpsys) (который ранее назывался [WebListener](xref:fundamentals/servers/weblistener) для приложений ASP.NET Core 1.x), а не [Kestrel](xref:fundamentals/servers/kestrel). Рекомендуем применять IIS в качестве обратного прокси-сервера для развертываний Kestrel на пограничных серверах. Дополнительные сведения см. в статье [Использование Kestrel с обратным прокси-сервером](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

## <a name="get-started"></a>Начало работы

В этом разделе описан минимальный набор изменений, позволяющий настроить существующий проект ASP.NET Core для запуска в службе.

1. Установите пакет NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).

2. Внесите следующие изменения в `Program.Main`:

   * Вызовите метод `host.RunAsService` вместо `host.Run`.

   * Если в коде есть вызовы `UseContentRoot`, используйте в них путь к расположению публикации вместо `Directory.GetCurrentDirectory()`.

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   ---

3. Опубликуйте приложение в папке. Используйте команду [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) или [профиль публикации Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles), выполняющий публикацию в папку.

4. Выполните проверку, создав и запустив службу.

   Откройте оболочку командной строки с правами администратора и запустите в ней программу командной строки [sc.exe](https://technet.microsoft.com/library/bb490995) для создания и запуска службы. Например, если служба называется MyService и опубликована в `c:\svc` с именем AspNetCoreService, для ее запуска используются следующие команды:

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   Значение `binPath` обозначает путь к исполняемому файлу приложения, включая имя самого файла.

   ![Пример создания и запуска в окне консоли](windows-service/_static/create-start.png)

   Когда эти команд будут выполнены, перейдите по тому же пути, что и при запуске консольного приложения (по умолчанию это `http://localhost:5000`):

   ![Выполнение в качестве службы](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a>Обеспечение возможности запуска за пределами службы

Тестирование и отладку проще выполнять при работе вне службы. Поэтому разработчики обычно добавляют код, который вызывает `RunAsService` только при соблюдении определенных условий. Например, приложение может выполняться как консольное приложение с аргументом командной строки `--console` или при присоединении отладчика:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

---

## <a name="handle-stopping-and-starting-events"></a>Обработка событий остановки и запуска

Чтобы правильно обрабатывать события `OnStarting`, `OnStarted` и `OnStopping`, внесите дополнительные изменения.

1. Создайте класс, производный от класса `WebHostService`.

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. Создайте метод расширения для `IWebHost`, который передает пользовательский параметр `WebHostService` в `ServiceBase.Run`:

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. В `Program.Main` измените вызов `RunAsService` на вызов нового метода расширения `RunAsCustomService`:

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   ---

Если пользовательский код `WebHostService` обращается к службе путем введения зависимости (например, к средству ведения журнала), ее можно получить из свойства `Services` объекта `IWebHost`:

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Сценарии использования прокси-сервера и подсистемы балансировки нагрузки

Для служб, которые взаимодействуют с запросами из Интернета или корпоративной сети и размещаются за прокси-сервером или подсистемой балансировки нагрузки, может потребоваться дополнительная настройка. Дополнительные сведения см. в разделе [Настройка ASP.NET Core для работы с прокси-серверами и подсистемами балансировки нагрузки](xref:host-and-deploy/proxy-load-balancer).

## <a name="acknowledgments"></a>Благодарности

Эта статья создана с использованием данных из других опубликованных источников:

* [Hosting ASP.NET Core as Windows service](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074) (Размещение ASP.NET Core в качестве службы Windows)
* [How to host your ASP.NET Core in a Windows Service](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/) (Как разместить ASP.NET Core в службе Windows)
