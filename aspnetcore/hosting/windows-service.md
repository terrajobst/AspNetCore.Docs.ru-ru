---
title: "Узел в службе Windows"
author: tdykstra
description: "Узнайте, как для размещения приложения ASP.NET Core в службе Windows."
keywords: "Размещение ASP.NET Core, службы Windows"
ms.author: tdykstra
manager: wpickett
ms.date: 03/30/2017
ms.topic: article
ms.assetid: d9a65066-d7cb-47df-b046-64629c4d2c6f
ms.technology: aspnet
ms.prod: aspnet-core
uid: hosting/windows-service
ms.openlocfilehash: 5b54c77ff9e019b1d550aa687923077a3e9ba5c2
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/22/2017
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a>Узел приложения ASP.NET Core в службе Windows

По [Tom Dykstra](https://github.com/tdykstra)

Для размещения приложения ASP.NET Core в Windows, если вы не используете IIS рекомендуется выполнять [службы Windows](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications). Таким образом она сможет автоматически начать после перезагрузки и сбои, не ожидая, пока кто-либо выполнить вход.

[Просмотреть или загрузить образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) разделе [дальнейшие действия](#next-steps) разделе инструкции о том, как запустить его.

## <a name="prerequisites"></a>Предварительные требования

* Приложение должно выполняться в среде выполнения .NET framework.  В *.csproj* файла, укажите соответствующие значения для [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) и [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog). Ниже приведен пример:

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  При создании проекта в Visual Studio, используйте **основное приложение ASP.NET (.NET Framework)** шаблона.

* Если приложение будет получать запросы из Интернета (не только из внутренней сети), он должен использовать [WebListener](xref:fundamentals/servers/weblistener) веб-сервер вместо [Kestrel](xref:fundamentals/servers/kestrel).  Для развертываний edge kestrel необходимо использовать со службами IIS.  Дополнительные сведения см. в статье [Использование Kestrel с обратным прокси-сервером](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

## <a name="getting-started"></a>Начало работы

В этом разделе описывается минимальный набор изменений, необходимых для настройки существующего проекта ASP.NET Core для запуска в службе.

* Установка пакета NuGet [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).

* Внесите следующие изменения в `Program.Main`:
  
  * Вызовите `host.RunAsService` вместо `host.Run`.
  
  * Если код вызывает `UseContentRoot`, используйте путь в расположение публикации вместо`Directory.GetCurrentDirectory()` 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* Публикация приложения в папке.

  Используйте [публикации dotnet](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) или [профиль публикации Visual Studio](xref:publishing/web-publishing-vs) , публикует в папку.

* Тестирование путем создания и запуска службы.

  Откройте окно командной строки администратора для использования [sc.exe](https://technet.microsoft.com/library/bb490995) средство командной строки для создания и запуска службы.  
  
  Если имя службы MyService, публикации приложения для `c:\svc`и само приложение называется AspNetCoreService, команды будет выглядеть следующим образом:

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```
  `binPath` Значение — это путь к исполняемому файлу приложения, включая сам имя исполняемого файла.

  ![Окно консоли, создание и запуск примера](windows-service/_static/create-start.png)

  После завершения этих команд, можно просмотреть по тому же пути, которое использовалось для выполнения как консольное приложение (по умолчанию `http://localhost:5000`)

  ![Выполняемые в службе](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a>Укажите способ запуска за пределами службы

Это упрощает тестирование и отладку при запуске вне службы, поэтому обычно добавьте код, вызывающий `host.RunAsService` только при определенных условиях.  Например, можно выполнить как консольное приложение при получении `--console` аргумент командной строки или если присоединен отладчик.

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a>Остановка и запуск событий

Если требуется обрабатывать `OnStarting`, `OnStarted`, и `OnStopping` события, выполните следующие дополнительные изменения:

* Создайте класс, наследующий от класса `WebHostService`.

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* Создание метода расширения для `IWebHost` , передает пользовательскую `WebHostService` для `ServiceBase.Run`.

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* В `Program.Main` изменений вызывал новый метод расширения, вместо `host.RunAsService`.

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

Если пользовательский `WebHostService` коду требуется доступ к службе из внедрения зависимости (например, средства ведения журнала), можно получить его из `Services` свойство `IWebHost`.

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a>Следующие шаги

[Образец приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) , сопровождающий это статья является простой веб-приложения MVC, который был изменен, как показано в предыдущих примерах кода.  Чтобы запустить его в службе, выполните следующие действия:

* Публикация в *c:\svc*.

* Откройте окно администратора.

* Введите следующие команды:

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * В браузере перейдите к http://localhost: 5000, чтобы убедиться, что он работает.

Если приложение не запускается до должным образом при запуске в службе, быстрый способ сделать доступной сообщения об ошибках является для добавления поставщика ведения журнала, такие как [поставщик журнала событий Windows](xref:fundamentals/logging#eventlog).

## <a name="acknowledgments"></a>Подтверждения

В этой статье был записан с помощью источников, которые уже были опубликованы. Минимальное значение и наиболее полезные из них были их:

* [Размещение ASP.NET Core в качестве службы Windows](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [Как разместить основные ASP.NET в службе Windows](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
