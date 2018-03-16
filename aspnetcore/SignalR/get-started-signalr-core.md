---
title: "Приступая к работе с SignalR в ASP.NET Core"
author: rachelappel
description: "В этом учебнике создается приложение с помощью SignalR для ASP.NET Core."
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/16/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started-signalr-core
ms.openlocfilehash: 139da5a2d0dadf51fece94b7c54ccd531e0ae8c2
ms.sourcegitcommit: 6548a3dd0cd1e3e92ac2310dee757ddad9fd6456
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/16/2018
---
# <a name="tutorial-get-started-with-signalr-for-aspnet-core"></a>Учебник: Приступая к работе с SignalR для ASP.NET Core

Автор: [Рэйчел Аппель](https://twitter.com/rachelappel) (Rachel Appel)

В этом учебнике показано основы построения в режиме реального времени приложения с помощью SignalR для ASP.NET Core.

   ![Решение](get-started-signalr-core/_static/signalr-get-started-finished.png)

В этом учебнике показано следующие задачи разработки SignalR:

> [!div class="checklist"]
> * Создайте SignalR на веб-приложения ASP.NET Core.
> * Создание концентратора SignalR для передачи содержимого клиентам.
> * Изменить `Startup` класс и настройки приложения.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

# <a name="prerequisites"></a>Предварительные требования

Установите следующее программное обеспечение:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET core 2.1.0 предварительного просмотра 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) или более поздней версии
* [Visual Studio 2017 г](https://www.visualstudio.com/downloads/) 15.6 или более поздней версии с **ASP.NET и веб-разработки** рабочей нагрузки
* [npm](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

* [.NET core 2.1.0 предварительного просмотра 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) или более поздней версии
* [Visual Studio Code.](https://code.visualstudio.com/download) 
* [C# для кода Visual Studio](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>Создайте проект ASP.NET Core, на котором размещена SignalR клиента и сервера

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Используйте **файл** > **новый проект** меню и выберите **веб-приложения ASP.NET Core**. Назовите проект *SignalRChat*.

  ![Диалоговое окно нового проекта в Visual Studio](get-started-signalr-core/_static/signalr-new-project-dialog.png)

2. Выберите **веб-приложения** для создания проекта с помощью страниц Razor. Выберите **ОК**. Убедитесь, что **ASP.NET Core 2.1** выбирается из селектора framework, то выполняется SignalR в предыдущих версиях .NET.

  ![Диалоговое окно нового проекта в Visual Studio](get-started-signalr-core/_static/signalr-new-project-choose-type.png)

3. Щелкните правой кнопкой мыши проект в **обозревателе решений** > **добавить** > **новый элемент** > **файл конфигурации npm** . Назовите файл *package.json*.

4. Выполните следующую команду **консоль диспетчера пакетов** окна, в корне проекта:

    ```console
      npm install @aspnet/signalr
    ```
5. Копировать *signalr.js* файл из *node_modules\\ @aspnet\signalr\dist\browser*  для *wwwroot\lib* в папке проекта.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

1. Из **интеграции терминалов**, выполните следующую команду:
 
    ```console
      dotnet new razor -o SignalRChat
    ```

2. Установите библиотеку клиента JavaScript с помощью *npm*.

    ```
      npm init -y
      npm install @aspnet/signalr
    ```

-----

## <a name="create-the-signalr-hub"></a>Создание концентратора SignalR

Концентратор является класс, который служит в качестве высокого уровня конвейера, который позволяет клиенту и серверу для вызова методов друг от друга.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Добавить класс в проект, выбрав **файл** > **New** > **файл** и выбрав **классов Visual C#**.

1. Наследовать от `Microsoft.AspNetCore.SignalR.Hub`. `Hub` Класс содержит свойства и события для управления подключения и группы, а также отправки и получения данных.

1. Создание `SendMessage` метод, который отправляет сообщение всем клиентам, подключенным чат. Обратите внимание на то, она возвращает [задачи](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task(v=vs.110).aspx), так как SignalR является асинхронным. Лучше масштабируется асинхронного кода.

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

1. Откройте *SignalRChat* папки в коде Visual Studio.

1. Добавить класс в проект, выбрав **файл** > **новый файл** в меню.

1. Наследовать от `Microsoft.AspNetCore.SignalR.Hub`. `Hub` Класс содержит свойства и события для управления подключения и группы, а также отправки и получения данных для клиентов.

1. Добавьте в класс метод `SendMessage`. `SendMessage` Метод отправляет сообщение всем клиентам, подключенным чат. Обратите внимание на то, она возвращает [задачи](/dotnet/api/system.threading.tasks.task), так как SignalR является асинхронным. Лучше масштабируется асинхронного кода.

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

-----

## <a name="configure-the-project-to-use-signalr"></a>Настроить проект для использования SignalR

SignalR сервера необходимо настроить, чтобы он доверял передачи запросов для SignalR.

1. Чтобы настроить проект SignalR, работать с проектом `Startup.ConfigureServices` метод.

  `services.AddSignalR` Добавляет в составе SignalR [по промежуточного слоя](xref:fundamentals/middleware/index) конвейера.

1. Настроить маршруты для концентраторов с помощью `UseSignalR`.

  [!code-csharp[Startup](get-started-signalr-core/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a>Создайте код клиента SignalR

1. Замените содержимое в *Pages\Index.cshtml* следующим кодом:

  [!code-cshtml[Index](get-started-signalr-core/sample/Pages/Index.cshtml)]

  Предыдущий HTML отображает имя и поля сообщения и кнопка отправки. Обратите внимание, ссылки на скрипты в нижней: ссылку на SignalR и *chat.js*.

1. Добавьте файл JavaScript с именем *chat.js*в *wwwroot\js* папки. Добавьте в этот файл следующий код:

  [!code-javascript[Index](get-started-signalr-core/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a>Запуск приложения

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Выберите **отладки** > **Запуск без отладки** запускает браузер и загрузить веб-сайт локально. Скопируйте URL-адрес из адресной строки.

1. Откройте другой экземпляр браузера (любой браузер) и вставьте URL-адрес в адресной строке.

1. Выберите любой браузер, введите имя и сообщение и нажмите кнопку **отправки** кнопки. Имя и сообщения отображаются на обеих страницах мгновенно.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

1. Нажмите клавишу **отладки** (F5), чтобы выполнить сборку программы и запустить ее. Запуск программы открывает окно браузера.

1. Открытие другого окна браузера и загрузить локально в веб-сайта.

1. Выберите любой браузер, введите имя и сообщение и нажмите кнопку **отправки** кнопки. Имя и сообщения отображаются на обеих страницах мгновенно.

-----

  ![Решение](get-started-signalr-core/_static/signalr-get-started-finished.png)