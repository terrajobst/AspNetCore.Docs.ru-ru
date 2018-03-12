---
title: "Приступая к работе с SignalR в ASP.NET Core"
author: rachelappel
ms.author: rachelap
description: "В этом учебнике создается приложение с помощью SignalR для ASP.NET Core."
manager: wpickett
ms.date: 03/06/2018
ms.topic: tutorial
ms.technology: dotnet-signalr
ms.prod: aspnet-core
ms.custom: mvc
uid: signalr/get-started-signalr-core
ms.openlocfilehash: 4afb9785fc3d0f472226a745537acbc77adefb4c
ms.sourcegitcommit: 9622bdc6326c28c3322c70000468a80ef21ad376
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2018
---
# <a name="tutorial-get-started-with-signalr-for-aspnet-core"></a>Учебник: Приступая к работе с SignalR для ASP.NET Core

Автор: [Рэйчел Аппель](https://twitter.com/rachelappel) (Rachel Appel)

В этом учебнике показано основы построения в режиме реального времени приложения с помощью SignalR для ASP.NET Core.

   ![Решение](get-started-signalr-core/_static/signalr-get-started-finished.png)

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

В этом учебнике показано следующие задачи разработки SignalR:

> [!div class="checklist"]
> * Создайте веб-приложение ASP.NET Core.
> * Создание концентратора SignalR для передачи содержимого клиентам.
> * Используйте библиотеку SignalR JavaScript для отправки сообщений и отобразить обновления от концентратора.

## <a name="prerequisites"></a>Предварительные требования

Установите следующее программное обеспечение:

* [.NET core 2.1.0 предварительного просмотра 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) или более поздней версии
* [Visual Studio 2017 г](https://www.visualstudio.com/downloads/) 15.6 или более поздней версии с рабочей нагрузкой ASP.NET и веб-разработки
* [npm](https://www.npmjs.com/get-npm)

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>Создайте проект ASP.NET Core, на котором размещена SignalR клиента и сервера

1. Используйте **файл** > **новый проект** меню и выберите **веб-приложения ASP.NET Core**. Задайте для проекта имя `SignalRChat`.

  ![Диалоговое окно нового проекта в Visual Studio](get-started-signalr-core/_static/signalr-new-project-dialog.png)

2. Выберите **веб-приложения** для создания проекта с помощью страниц Razor. Выберите **ОК**. Убедитесь, что **ASP.NET Core 2.1** выбирается из селектора framework, то выполняется SignalR в предыдущих версиях .NET.

  ![Диалоговое окно нового проекта в Visual Studio](get-started-signalr-core/_static/signalr-new-project-choose-type.png)

  Библиотеки, в которых размещена SignalR серверный код, включаются в шаблоне проекта. Установка клиентского JavaScript отдельно с [npm](https://www.npmjs.com/).

  ```console
   npm install @aspnet/signalr
  ```

3. Копировать *signalr.js* из *node_modules\\ @aspnet\signalr\dist\browser*  для *wwwroot\lib* в папке проекта.

## <a name="create-the-signalr-hub"></a>Создание концентратора SignalR

Концентратор является класс, который служит в качестве высокого уровня конвейера, который позволяет клиенту и серверу для вызова методов друг от друга.

1. Добавить класс в проект, выбрав **файл** > **New** > **файл** и выбрав **классов Visual C#**. 

1. Наследовать от `Microsoft.AspNetCore.SignalR.Hub`. `Hub` Класс содержит свойства и события для управления подключения и группы, а также отправки и получения данных.

1. Создание `Send` метод, который отправляет сообщение всем клиентам, подключенным чат. Обратите внимание на то, она возвращает `Task`, так как SignalR является асинхронным. Лучше масштабируется асинхронного кода.

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

## <a name="configure-the-project-to-use-signalr"></a>Настроить проект для использования SignalR

SignalR сервера необходимо настроить, чтобы он доверял передачи запросов для SignalR.

1. Чтобы настроить проект SignalR, измените `ConfigureServices` метод приложения `Startup` класса путем вставки вызов `services.AddSignalR`.

  `services.AddSignalR` Добавляет в составе SignalR [по промежуточного слоя ASP.NET Core](xref:fundamentals/middleware/index) конвейера.

1. Настроить маршруты для концентраторов с помощью `UseSignalR`.

  [!code-csharp[Startup](get-started-signalr-core/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a>Создайте код клиента SignalR

1. Замените содержимое в *Pages\Index.cshtml* следующим кодом:

  [!code-cshtml[Index](get-started-signalr-core/sample/Pages/Index.cshtml)]

  Предыдущий HTML отображает имя и поля сообщения и кнопка отправки. Обратите внимание, ссылки на скрипты в нижней: ссылку на SignalR и *chat.js*.

1. Добавьте файл JavaScript, чтобы *wwwroot\js* папку с именем *chat.js* и добавьте в него следующий код:

  [!code-javascript[Index](get-started-signalr-core/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a>Запуск приложения

1. Выберите **отладки** > **Запуск без отладки** запускает браузер и загрузить веб-сайт локально. Скопируйте URL-адрес из адресной строки.

1. Откройте другой экземпляр браузера (любой браузер) и вставьте URL-адрес в адресной строке.

1. Выберите любой браузер, введите имя и сообщение и нажмите кнопку **отправки** кнопки. Имя и сообщения отображаются на обеих страницах мгновенно.

  ![Решение](get-started-signalr-core/_static/signalr-get-started-finished.png)
