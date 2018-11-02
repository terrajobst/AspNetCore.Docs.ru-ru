---
title: Начало работы с SignalR ASP.NET Core
author: tdykstra
description: В этом руководстве создается приложение чата, которое использует SignalR для ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: tutorials/signalr
ms.openlocfilehash: c059ace7ebe0e65ecb3ac068677d65ae148322a0
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207684"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a>Руководство. Начало работы с SignalR ASP.NET Core

В этом руководстве описаны основы создания приложения в режиме реального времени с помощью SignalR. Вы научитесь:

> [!div class="checklist"]
> * Создайте веб-проект.
> * добавлять клиентскую библиотеку SignalR;
> * создавать концентратор SignalR;
> * настраивать проект для использования SignalR;
> * Добавлять код для отправки сообщений из любого клиента всем подключенным клиентам.

В итоге вы получите работающее приложение чата:

![Пример приложения SignalR](signalr/_static/signalr-get-started-finished.png)

[Просмотреть или скачать пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([описание скачивания](xref:index#how-to-download-a-sample)).

## <a name="prerequisites"></a>Предварительные требования

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Visual Studio 2017 15.8 или более поздней версии](https://www.visualstudio.com/downloads/) с рабочей нагрузкой **ASP.NET и веб-разработка**
* [Пакет SDK для .NET Core 2.1.или более поздней версии](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

* [Visual Studio Code.](https://code.visualstudio.com/download)
* [Пакет SDK для .NET Core 2.1.или более поздней версии](https://www.microsoft.com/net/download/all)
* [C# для Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

* [Visual Studio для Mac 7.5.4 или более поздней версии](https://www.visualstudio.com/downloads/)
* [Пакет SDK для .NET Core 2.1 или более поздней версии](https://www.microsoft.com/net/download/all) (входит в установку Visual Studio)

---

## <a name="create-a-web-project"></a>Создайте веб-проект.

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

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

* В меню выберите **Файл > Создать решение**.

* Выберите **.NET Core > Приложение > Веб-приложение ASP.NET Core** (не выбирайте **Веб-приложение ASP.NET Core (MVC)**).

* Выберите **Далее**.

* Присвойте проекту имя *SignalRChat* и нажмите кнопку **Создать**.

---

## <a name="add-the-signalr-client-library"></a>Добавление клиентской библиотеки SignalR

Библиотека сервера SignalR входит в состав метапакета `Microsoft.AspNetCore.App`. Клиентская библиотека JavaScript не добавляется в проект автоматически. В рамках этого руководства вы будете использовать диспетчер библиотек (LibMan), чтобы получить клиентскую библиотеку из *unpkg*. unpkg — это сеть доставки содержимого, которая позволяет доставить любое содержимое из npm (диспетчера пакетов Node.js).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* В **обозревателе решений** щелкните проект правой кнопкой мыши и выберите **Добавить** > **Client-Side Library** (Клиентская библиотека).

* В диалоговом окне **Add Client-Side Library** (Добавить клиентскую библиотеку) для параметра **Поставщик** выберите **unpkg**. 

* В поле **Библиотека** введите `@aspnet/signalr@1` и выберите последнюю версию, но не предварительную.

  ![Диалоговое окно добавления клиентской библиотеки — выбор библиотеки](signalr/_static/libman1.png)

* Щелкните **Choose specific files** (Выбрать определенные файлы), разверните папку *dist/browser* и выберите *signalr.js* и *signalr.min.js*.

* В поле **Целевое расположение** укажите *wwwroot/lib/signalr/* и нажмите кнопку **Установить**.

  ![Диалоговое окно добавления клиентской библиотеки — выбор файлов и назначения](signalr/_static/libman2.png)

  LibMan создает папку *wwwroot/lib/signalr* и копирует в нее выбранные файлы.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code/)

* Во **встроенном терминале** выполните следующую команду, чтобы установить LibMan.

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* Перейдите в папку проекта (где расположен файл *SignalRChat.csproj*).

* Выполните приведенную ниже команду, чтобы получить клиентскую библиотеку SignalR с помощью LibMan. Возможно, придется подождать несколько секунд, прежде чем появятся выходные данные.

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  В параметрах указываются следующие свойства:
  * использование поставщика unpkg;
  * копирование файлов в назначение *wwwroot/lib/signalr*;
  * копирование только выбранных файлов.

  Выходные данные выглядят примерно так, как в следующем примере:

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

* В **терминале** выполните следующую команду, чтобы установить LibMan.

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* Перейдите в папку проекта (где расположен файл *SignalRChat.csproj*).

* Выполните приведенную ниже команду, чтобы получить клиентскую библиотеку SignalR с помощью LibMan.

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  В параметрах указываются следующие свойства:
  * использование поставщика unpkg;
  * копирование файлов в назначение *wwwroot/lib/signalr*;
  * копирование только выбранных файлов.

  Выходные данные выглядят примерно так, как в следующем примере:

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a>Создание концентратора SignalR

*hub* — это класс, который служит в качестве конвейера высокого уровня для обработки взаимодействия между клиентом и сервером.

* В папке проекта SignalRChat создайте папку *Hubs*.

* В папке *Hubs* создайте файл *ChatHub.cs* со следующим кодом:

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  Класс `ChatHub` наследует от класса `Hub` SignalR. Класс `Hub` управляет подключениями, группами и обменом сообщениями.

  Метод `SendMessage` может вызываться любым подключенным клиентом. Он отправляет полученное сообщение всем клиентам. Код SignalR является асинхронным, поэтому обеспечивает максимальную масштабируемость.

## <a name="configure-signalr"></a>Настройка SignalR

Сервер SignalR необходимо настроить таким образом, чтобы он передавал запросы SignalR в SignalR.

* Добавьте следующий выделенный код в файл *Startup.cs*.

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  В результате SignalR будет добавлен в систему внедрения зависимостей ASP.NET Core и конвейер ПО промежуточного слоя.

## <a name="add-signalr-client-code"></a>Добавление кода клиента SignalR

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

В этом руководстве вы узнали, как:

> [!div class="checklist"]
> * создавать проект веб-приложения;
> * добавлять клиентскую библиотеку SignalR;
> * создавать концентратор SignalR;
> * настраивать проект для использования SignalR;
> * добавлять код, использующий концентратор для отправки сообщений из любого клиента всем подключенным клиентам.

Дополнительные сведения о SignalR см. во введении:

> [!div class="nextstepaction"]
> [Введение в ASP.NET Core SignalR](xref:signalr/introduction)
