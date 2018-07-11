---
title: Клиент ASP.NET Core SignalR JavaScript
author: rachelappel
description: Общие сведения о клиенте ASP.NET Core SignalR JavaScript.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/javascript-client
ms.openlocfilehash: d0eba401b3152d510b9db092169e83f6da2a0523
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938048"
---
# <a name="aspnet-core-signalr-javascript-client"></a>Клиент ASP.NET Core SignalR JavaScript

Автор: [Рэйчел Аппель](http://twitter.com/rachelappel) (Rachel Appel)

Клиентская библиотека ASP.NET Core SignalR JavaScript позволяет разработчикам вызывать код концентратора на стороне сервера.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-client-package"></a>Установка пакета клиента SignalR

Клиентская библиотека SignalR JavaScript предоставляется в виде [npm](https://www.npmjs.com/) пакета. Если вы используете Visual Studio, запустите `npm install` из **консоль диспетчера пакетов** при работе в корневой папке. Для Visual Studio Code, выполните команду из **интегрированный терминал**.

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

Npm устанавливает содержимое пакета в *node_modules\\ @aspnet\signalr\dist\browser*  папки. Создайте новую папку с именем *signalr* под *wwwroot\\lib* папки. Копировать *signalr.js* файл *wwwroot\lib\signalr* папки.

## <a name="use-the-signalr-javascript-client"></a>Использование клиента SignalR JavaScript

Сослаться на клиентский SignalR JavaScript в `<script>` элемент.

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>Подключение к концентратору

Следующий код создает и запускает подключение. Имя концентратора регистр не учитывается.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a>Независимо от источника соединения

Как правило браузеры нагрузки подключений из тому же домену, что запрошенной страницы. Тем не менее существуют случаи, когда требуется соединение с другим доменом.

Для предотвращения чтения конфиденциальных данных с другого сайта, вредоносный сайт [подключения независимо от источника](xref:security/cors) отключены по умолчанию. Чтобы разрешить запрос независимо от источника, включить его в `Startup` класса.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>Вызов методов концентратора из клиента

Клиенты JavaScript вызывать открытые методы в концентраторах, с помощью `connection.invoke`. `invoke` Метод принимает два аргумента:

* Имя метода концентратора. В следующем примере имеет имя центра `SendMessage`.
* Все аргументы, определенный в методе концентратора. В следующем примере, является имя аргумента `message`.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a>Вызывать методы клиента от концентратора

Для получения сообщений от концентратора, определение метода с помощью `connection.on` метод.

* Имя метода клиента JavaScript. В следующем примере, является имя метода `ReceiveMessage`.
* Аргументы концентратор передает в метод. В следующем примере значение аргумента равно `message`.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

Предыдущий код в `connection.on` выполняется, когда серверный код вызывает ее с помощью `SendAsync` метод.

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR определяют, какой метод клиента для вызова, сопоставляя имя метода и аргументы, определенные в `SendAsync` и `connection.on`.

> [!NOTE]
> Рекомендуется, вызовите `connection.start` после `connection.on` обработчики зарегистрированные до получения сообщения.

## <a name="error-handling-and-logging"></a>Обработка ошибок и ведение журнала

Цепочка `catch` метод в конец `connection.start` метод для обработки ошибок на стороне клиента. Используйте `console.error` для ошибок вывода для консоли браузера.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

Настройка трассировки журнала на стороне клиента, передав средства ведения журнала и тип события в журнале, когда устанавливается соединение. Выводятся сообщения с уровнем журнала и более поздних версий. Ниже перечислены уровни доступных журналов:

* `signalR.LogLevel.Error` : Сообщения об ошибке. Журналы `Error` только сообщения.
* `signalR.LogLevel.Warning` : Предупреждение сообщения об ошибках. Журналы `Warning`, и `Error` сообщений.
* `signalR.LogLevel.Information` : Сообщения о состоянии без ошибок. Журналы `Information`, `Warning`, и `Error` сообщений.
* `signalR.LogLevel.Trace` : Сообщения трассировки. В журнал все события, в том числе данных, перемещаемая между центром и клиентом.

Используйте `configureLogging` метод `HubConnectionBuilder` настроить уровень ведения журнала. Сообщения записываются в консоли браузера.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="related-resources"></a>Связанные ресурсы

* [Центры](xref:signalr/hubs)
* [Клиент .NET](xref:signalr/dotnet-client)
* [Публикация в Azure](xref:signalr/publish-to-azure-web-app)
* [Включение запросов о происхождении (CORS) в ASP.NET Core](xref:security/cors)
