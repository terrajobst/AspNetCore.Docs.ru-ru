---
title: ASP.NET SignalR JavaScript ядра клиента
author: rachelappel
description: Обзор клиента ASP.NET Core SignalR JavaScript.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/06/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/javascript-client
ms.openlocfilehash: cca1a523bd536f4365e54c196f6c9024779d5d76
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2018
---
# <a name="aspnet-core-signalr-javascript-client"></a>ASP.NET SignalR JavaScript ядра клиента

Автор: [Рэйчел Аппель](http://twitter.com/rachelappel) (Rachel Appel)

[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

Клиентская библиотека ASP.NET Core SignalR JavaScript позволяет разработчикам вызывать код концентратора на стороне сервера.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-client-package"></a>Установить пакет клиента SignalR

Клиентская библиотека SignalR JavaScript доставляется в виде [npm](https://www.npmjs.com/) пакета. Если вы используете Visual Studio, запустите `npm install` из **консоль диспетчера пакетов** при работе в корневой папке. Для кода Visual Studio, выполните команду из **интеграции терминалов**.

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

Содержимое пакета в устанавливает Npm *node_modules\\ @aspnet\signalr\dist\browser*  папки. Создайте новую папку с именем *signalr* под *wwwroot\\lib* папки. Копировать *signalr.js* файл *wwwroot\lib\signalr* папки.

## <a name="use-the-signalr-javascript-client"></a>Использование клиента SignalR JavaScript

Ссылки в клиента SignalR JavaScript `<script>` элемента.

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>Подключения к концентратору

Следующий код создает и запускает подключение. Имя концентратора без учета регистра.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=1-2,18)]

### <a name="cross-origin-connections"></a>Независимо от источника подключения

Как правило браузеры загрузить подключений из того же домена, что запрошенной страницы. Однако существуют случаи, когда требуется соединение с другим доменом.

Для предотвращения чтения конфиденциальных данных с другого сайта вредоносный сайт [подключения независимо от источника](xref:security/cors) по умолчанию отключены. Чтобы разрешить запрос независимо от источника, включить его в `Startup` класса.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-34,55)]

## <a name="call-hub-methods-from-client"></a>Вызов методов концентратора из клиента

JavaScript клиенты вызывать открытые методы концентраторы, с помощью `connection.invoke`. `invoke` Метод принимает два аргумента:

* Имя метода концентратора. В следующем примере имеет имя концентратора `SendMessage`.
* Все аргументы, заданные в метод концентратора. В следующем примере имя аргумента является `message`.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=14)]

## <a name="call-client-methods-from-hub"></a>Вызовите методы клиента от концентратора

Чтобы получать сообщения от концентратора, определение метода с помощью `connection.on` метод.

* Имя метода клиента JavaScript. В следующем примере имя метода является `ReceiveMessage`.
* Метод передает аргументы концентратора. В следующем примере значение аргумента равно `message`.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=4-9)]

Приведенный выше код в `connection.on` выполняется, когда серверный код вызывает ее с помощью `SendAsync` метод.

[!code-javascript[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR определяют, какой метод клиента для вызова путем сопоставления имени метода и определенные аргументы в `SendAsync` и `connection.on`.

> [!NOTE]
> Рекомендуется вызывать `connection.start` после `connection.on` , зарегистрированных обработчиков до получения сообщения.

## <a name="error-handling-and-logging"></a>Обработка ошибок и ведение журнала

Цепочка `catch` метод в конец `connection.start` метод для обработки ошибок на стороне клиента. Используйте `console.error` для ошибок вывода на консоль в браузере.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=18)]

Настройка трассировки журнала клиентской части, передав средства ведения журнала и тип события в журнале, когда устанавливается соединение. Выводятся сообщения с уровнем указанного журнала и более поздних версий. Существуют следующие уровни доступных журнала.

* `signalR.LogLevel.Error` : Сообщения об ошибке. Журналы `Error` только сообщения.
* `signalR.LogLevel.Warning` : Предупреждение сообщения о потенциальных ошибок. Журналы `Warning`, и `Error` сообщений.
* `signalR.LogLevel.Information` : Сообщения о состоянии без ошибок. Журналы `Information`, `Warning`, и `Error` сообщений.
* `signalR.LogLevel.Trace` : Сообщения трассировки. Заносит в журнал все содержимое, включая данных, перемещаемая между концентратором и клиента.

Передайте средство ведения журнала подключения для начала ведения журнала. Средства разработчика браузера обычно содержат консоли, где отображаются сообщения.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=1-2)]

## <a name="related-resources"></a>Связанные ресурсы

* [Концентраторы SignalR в ASP.NET Core](xref:signalr/hubs)
* [Включить запросы независимо от источника (CORS) в ASP.NET Core](xref:security/cors)