---
title: Начало работы с SignalR в ASP.NET Core
author: tdykstra
description: В этом руководстве создается приложение с помощью SignalR для ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: 83be28b30cf06eeea37e8d76b3f6444ffd9a10e8
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095495"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a>Начало работы с SignalR в ASP.NET Core

В этом руководстве описаны основы создания приложения в режиме реального времени с помощью SignalR для ASP.NET Core.

   ![Решение](signalr/_static/signalr-get-started-finished.png)

Ниже перечислены рассматриваемые в данном руководстве задачи разработки SignalR:

> [!div class="checklist"]
> * Создание SignalR в веб-приложении ASP.NET Core.
> * Создание хаба SignalR для передачи содержимого клиентам.
> * Изменение класса `Startup` и настройка приложения.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Предварительные требования

Установите следующее программное обеспечение:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Пакет SDK для .NET Core 2.1.или более поздней версии](https://www.microsoft.com/net/download/all)
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.7.3 или более поздней версии с рабочей нагрузкой **ASP.NET и веб-разработка**
* [npm](https://www.npmjs.com/get-npm) (диспетчер пакетов для Node.js)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

* [Пакет SDK для .NET Core 2.1.или более поздней версии](https://www.microsoft.com/net/download/all)
* [Visual Studio Code.](https://code.visualstudio.com/download)
* [C# для Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm) (диспетчер пакетов для Node.js)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>Создайте проект ASP.NET Core, в котором размещен клиент и сервер SignalR

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. Перейдите в меню **Файл** > **Создать проект** и выберите **Веб-приложение ASP.NET Core**. Назовите проект *SignalRChat*.

   ![Диалоговое окно создания проекта в Visual Studio](signalr/_static/signalr-new-project-dialog.png)

2. Выберите **Веб-приложение**, чтобы создать проект с помощью Razor Pages. Нажмите **ОК**. Убедитесь, что в списке платформ выбрана **ASP.NET Core 2.1**, хотя SignalR выполняется и в предыдущих версиях .NET.

   ![Диалоговое окно создания проекта в Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

Visual Studio включает пакет `Microsoft.AspNetCore.SignalR`, содержащий библиотеки сервера в шаблоне **Веб-приложение ASP.NET Core**. Однако необходимо установить клиентскую библиотеку JavaScript для SignalR с помощью *npm*.

3. Выполните следующие команды в окне **Консоль диспетчера пакетов** в корневой папке проекта:

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. Создайте папку с именем signalr в папке *wwwroot/lib* в проекте. Скопируйте файл *signalr.js* из *node_modules\\@aspnet\signalr\dist\browser* в эту папку.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code/)

1. Во **встроенном терминале** выполните следующую команду:

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. Установите клиентскую библиотеку JavaScript с помощью *npm*.

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. Создайте новую папку с именем "signalr" в папке *lib* в проекте. Скопируйте файл *signalr.js* из *node_modules\\@aspnet\signalr\dist\browser* в эту папку.

---

## <a name="create-the-signalr-hub"></a>Создание хаба SignalR

Хаб является классом, который служит конвейером высокого уровня для вызова методов между клиентом и сервером.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. Добавьте класс в проект, перейдя в меню **Файл** > **Создать** > **Файл** и выбрав **Класс Visual C#**. Присвойте классу имя `ChatHub`, а файлу имя *ChatHub.cs*.

2. Наследуйте от `Microsoft.AspNetCore.SignalR.Hub`. Класс `Hub` содержит свойства и события для управления подключениями и группами, а также отправки и получения данных.

3. Создайте метод `SendMessage`, который отправляет сообщение всем подключенным клиентам чата. Заметьте, что он возвращает [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), так как SignalR является асинхронным. Асинхронный код лучше масштабируется.

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code/)

1. Откройте папку *SignalRChat* в Visual Studio Code.

2. Добавьте класс в проект, выбрав **Файл** > **Создать файл**. Присвойте классу имя `ChatHub`, а файлу имя *ChatHub.cs*.

3. Наследуйте от `Microsoft.AspNetCore.SignalR.Hub`. Класс `Hub` содержит свойства и события для управления подключениями и группами, а также отправки и получения данных для клиентов.

4. Добавьте в класс метод `SendMessage`. Метод `SendMessage` отправляет сообщение всем подключенным клиентам чата. Заметьте, что он возвращает [Task](/dotnet/api/system.threading.tasks.task), так как SignalR является асинхронным. Асинхронный код лучше масштабируется.

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

-----

## <a name="configure-the-project-to-use-signalr"></a>Настройка проекта для использования SignalR

Сервер SignalR необходимо настроить таким образом, чтобы он передавал запросы SignalR.

1. Чтобы настроить проект SignalR, измените метод `Startup.ConfigureServices` проекта.

   `services.AddSignalR` открывает системе [внедрения зависимостей](xref:fundamentals/dependency-injection) доступ к службам SignalR.

1. Настройте маршруты к хабам с помощью `UseSignalR` в методе `Configure`. Метод `app.UseSignalR` добавляет SignalR в конвейер [ПО промежуточного слоя](xref:fundamentals/middleware/index).

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a>Создание кода клиента SignalR

1. Добавьте файл JavaScript с именем *chat.js* в папку *wwwroot\js*. Добавьте в этот файл следующий код:

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. Замените содержимое в файле *Pages\Index.cshtml* следующим кодом:

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   Предыдущий HTML отображает поля имени и сообщения и кнопку отправки. Обратите внимание на ссылки на скрипты в нижней части: ссылка на SignalR и *chat.js*.

## <a name="run-the-app"></a>Запуск приложения

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Выберите **Отладка** > **Запуск без отладки**, чтобы запустить браузер и загрузить веб-сайт локально. Скопируйте URL-адрес из адресной строки.

1. Откройте другой экземпляр браузера (любого) и вставьте URL-адрес в адресную строку.

1. Выберите любой браузер, введите имя и сообщение и нажмите кнопку **Отправить**. Имя и сообщение отображаются на обеих страницах мгновенно.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

1. Нажмите клавишу **отладки** (F5), чтобы выполнить сборку программы и запустить ее. Запуск программы открывает окно браузера.

1. Откройте другое окно браузера и локально загрузите веб-сайт.

1. Выберите любой браузер, введите имя и сообщение и нажмите кнопку **Отправить**. Имя и сообщение отображаются на обеих страницах мгновенно.

---

  ![Решение](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a>Связанные ресурсы

[Введение в ASP.NET Core SignalR](xref:signalr/introduction)
