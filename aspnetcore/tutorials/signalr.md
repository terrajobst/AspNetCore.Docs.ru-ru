---
title: Руководство. Начало работы с SignalR в ASP.NET Core
author: tdykstra
description: В этом руководстве создается приложение чата, которое использует SignalR для ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/20/2018
uid: tutorials/signalr
ms.openlocfilehash: db7f31963f6a4280069f1f4f82a547e2879e64bb
ms.sourcegitcommit: d27317c16f113e7c111583042ec7e4c5a26adf6f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/20/2018
ms.locfileid: "41751537"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a>Руководство. Начало работы с SignalR в ASP.NET Core

В этом руководстве описаны основы создания приложения в режиме реального времени с помощью SignalR. Вы научитесь:

> [!div class="checklist"]
> * Создавать веб-приложение, которое использует SignalR в ASP.NET Core.
> * Создавать концентратор SignalR на сервере.
> * Подключаться к концентратору SignalR из клиентов JavaScript.
> * Использовать концентратор для отправки сообщений из любого клиента всем подключенным клиентам.

В итоге вы получите работающее приложение чата:

![Пример приложения SignalR](signalr/_static/signalr-get-started-finished.png)

[Просмотреть или скачать пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([описание скачивания](xref:tutorials/index#how-to-download-a-sample)).

## <a name="prerequisites"></a>Предварительные требования

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Visual Studio 2017 15.7.3 или более поздней версии](https://www.visualstudio.com/downloads/) с рабочей нагрузкой **ASP.NET и веб-разработка**
* [Пакет SDK для .NET Core 2.1.или более поздней версии](https://www.microsoft.com/net/download/all)
* [npm](https://www.npmjs.com/get-npm) (диспетчер пакетов для Node.js, используемый для клиентской библиотеки SignalR JavaScript.)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

* [Visual Studio Code.](https://code.visualstudio.com/download)
* [Пакет SDK для .NET Core 2.1.или более поздней версии](https://www.microsoft.com/net/download/all)
* [C# для Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm) (диспетчер пакетов для Node.js, используемый для клиентской библиотеки SignalR JavaScript.)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

* [Visual Studio для Mac 7.5.4 или более поздней версии](https://www.visualstudio.com/downloads/)
* [Пакет SDK для .NET Core 2.1 или более поздней версии](https://www.microsoft.com/net/download/all) (входит в установку Visual Studio)
* [npm](https://www.npmjs.com/get-npm) (диспетчер пакетов для Node.js, используемый для клиентской библиотеки SignalR JavaScript.)

---

## <a name="create-the-project"></a>Создание проекта

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* В меню выберите **Файл > Создать проект**.

* В диалоговом окне **Новый проект** выберите **Установленные > Visual C# > Веб > Веб-приложение ASP.NET Core**. Назовите проект *SignalRChat*.

  ![Диалоговое окно создания проекта в Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* Выберите **Веб-приложение**, чтобы создать проект, который использует Razor Pages.

* Выберите целевую платформу **.NET Core**, **ASP.NET Core 2.1** и нажмите кнопку **ОК**.

  ![Диалоговое окно создания проекта в Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code/)

* Откройте папку, которую можно использовать для нового проекта.

* Во **встроенном терминале** выполните следующую команду:

   ```console
   dotnet new webapp -o SignalRChat
   ```

   [!INCLUDE[](~/includes/webapp-alias-notice.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

* В меню выберите **Файл > Создать решение**.

* Выберите **.NET Core > Приложение > Веб-приложение ASP.NET Core** (не выбирайте **Веб-приложение ASP.NET Core (MVC)**).

* Выберите **Далее**.

* Присвойте проекту имя *SignalRChat* и нажмите кнопку **Создать**.

---

## <a name="add-the-signalr-client-library"></a>Добавление клиентской библиотеки SignalR

Библиотека сервера SignalR входит в состав [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Но вам нужно получить клиентскую библиотеку JavaScript из npm, диспетчера пакетов Node.js.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* В **консоли диспетчера пакетов** (PMC) перейдите в папку проекта (где расположен файл *SignalRChat.csproj*).

  ```console
  cd SignalRChat
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code/)

2. Перейдите к новой папке проекта.

  ```console
  cd SignalRChat
  ``` 

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

* В **терминале** перейдите в папку проекта (где расположен файл *SignalRChat.csproj*).

---

* Запустите инициализатор npm, чтобы создать файл *package.json*:

  ```console
  npm init -y
  ```

  Выходные данные этой команды выглядят примерно следующим образом:

  ```console
  Wrote to C:\tmp\SignalRChat\package.json:
  {
    "name": "SignalRChat",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1"
    },
    "keywords": [],
    "author": "",
    "license": "ISC"0
  }
  ```

* Установите пакет клиентской библиотеки:

  ```console
  npm install @aspnet/signalr
  ```

  Выходные данные этой команды выглядят примерно следующим образом:

  ```
  npm notice created a lockfile as package-lock.json. You should commit this file.
  npm WARN signalrchat@1.0.0 No description
  npm WARN signalrchat@1.0.0 No repository field.

  + @aspnet/signalr@1.0.2
  added 1 package in 0.98s
  ```

Команда `npm install` загрузила клиентскую библиотеку JavaScript во вложенную папку в разделе *node_modules*. Скопируйте ее оттуда в папку в разделе *wwwroot*, на которую можно ссылаться с веб-страницы приложения чата.

* Создайте папку *signalr* в *wwwroot/lib*.

* Скопируйте файл *signalr.js* из *node_modules/@aspnet/signalr/dist/browser* в новую папку *wwwroot/lib/signalr*.

## <a name="create-the-signalr-hub"></a>Создание концентратора SignalR

[hub](xref:signalr/hubs) — это класс, который служит в качестве конвейера высокого уровня для обработки взаимодействия между клиентом и сервером.

* В папке проекта SignalRChat создайте папку *Hubs*.

* В папке *Hubs* создайте файл *ChatHub.cs* со следующим кодом:

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  Класс `ChatHub` наследует от класса SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub). Класс `Hub` управляет подключениями, группами и обменом сообщениями.

  Метод `SendMessage` может вызываться любым подключенным клиентом. Он отправляет полученное сообщение всем клиентам. Код SignalR является асинхронным, поэтому обеспечивает максимальную масштабируемость.

## <a name="configure-the-project-to-use-signalr"></a>Настройка проекта для использования SignalR

Сервер SignalR необходимо настроить таким образом, чтобы он передавал запросы SignalR в SignalR.

* Добавьте следующий выделенный код в файл *Startup.cs*.

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  В результате SignalR будет добавлен в систему [внедрения зависимостей](xref:fundamentals/dependency-injection) и конвейер [ПО промежуточного слоя](xref:fundamentals/middleware/index).

## <a name="create-the-signalr-client-code"></a>Создание кода клиента SignalR

* Замените содержимое в файле *Pages\Index.cshtml* следующим кодом:

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  Предыдущий код:

  * Создает текстовые поля для имени и текста сообщения и кнопку отправки.
  * Создает список с `id="messagesList"` для отображения сообщений, полученных от концентратора SignalR.
  * Содержит ссылки на скрипты для SignalR и код приложения *chat.js*, который создается в следующем шаге.

* В папке *wwwroot/js* создайте файл *chat.js* со следующим кодом:

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  Предыдущий код:

  * Создает и запускает подключение.
  * Добавляет к кнопке отправки обработчик, который отправляет сообщения в концентратор.
  * Добавляет к объекту подключения обработчик, который получает сообщения из концентратора и добавляет их в список.

## <a name="run-the-app"></a>Запуск приложения

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Нажмите клавиши **CTRL+F5**, чтобы запустить приложение без отладки.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

* Нажмите клавиши **CTRL+F5**, чтобы запустить приложение без отладки.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

* В меню выберите **Запуск > Запуск без отладки**.

---

* Скопируйте URL-адрес из адресной строки, откройте другой экземпляр или вкладку браузера и вставьте URL-адрес в адресную строку.

* Выберите любой браузер, введите имя и сообщение и нажмите кнопку **Отправить**.

  Имя и сообщение отображаются на обеих страницах мгновенно.

  ![Пример приложения SignalR](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> Если приложение не работает, откройте средства разработчика для браузера (F12) и перейдите в консоль. Вы можете увидеть ошибки, связанные с вашим кодом HTML и JavaScript. Предположим, вы поместили *signalr.js* не в ту папку, которую указали. В этом случае ссылка на этот файл не будет работать, и вы увидите сообщение об ошибке 404 в консоли.
> ![ошибка: signalr.js не найден](signalr/_static/f12-console.png)

## <a name="next-steps"></a>Следующие шаги

Если вы хотите, чтобы клиенты могли подключаться к приложению SignalR из разных доменов, необходимо включить общий доступ к ресурсам независимо от источника (CORS). Дополнительные сведения см. в разделе [Общий доступ к ресурсам независимо от источника](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).

Чтобы узнать больше о SignalR, концентраторах и клиентах JavaScript, ознакомьтесь со следующими ресурсами:

* [Введение в SignalR для ASP.NET Core](xref:signalr/introduction)
* [Использование концентраторов в SignalR для ASP.NET Core](xref:signalr/hubs)
* [Клиент ASP.NET Core SignalR JavaScript](xref:signalr/javascript-client)
