---
title: Клиент .NET SignalR ASP.NET Core
author: tdykstra
description: Сведения о клиенте .NET, ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/07/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 970888a410b2486a20f98ce77a8674f8ec357f50
ms.sourcegitcommit: 028ad28c546de706ace98066c76774de33e4ad20
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/08/2018
ms.locfileid: "39655256"
---
# <a name="aspnet-core-signalr-net-client"></a>Клиент .NET SignalR ASP.NET Core

Автор: [Рэйчел Аппель](http://twitter.com/rachelappel) (Rachel Appel)

Клиент ASP.NET Core SignalR .NET можно использовать в приложениях Xamarin, WPF, Windows Forms, консоли и .NET Core. Как и [клиента JavaScript](xref:signalr/javascript-client), клиент .NET позволяет получать и отправлять и получать сообщения в концентратор в режиме реального времени.

> [!NOTE]
> Xamarin имеет особые требования для версии Visual Studio. Дополнительные сведения см. в разделе [клиентом SignalR 2.1.1 в Xamarin](https://github.com/aspnet/Announcements/issues/305).

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

В образце кода в этой статье показана приложения WPF, использующее клиент ASP.NET Core SignalR .NET.

## <a name="install-the-signalr-net-client-package"></a>Установка пакета клиента SignalR .NET

`Microsoft.AspNetCore.SignalR.Client` Пакет требуется для клиентов .NET для подключения к концентраторов SignalR. Чтобы установить клиентскую библиотеку, выполните следующую команду **консоль диспетчера пакетов** окна:

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>Подключение к концентратору

Чтобы установить подключение, создайте `HubConnectionBuilder` и вызвать `Build`. URL-адрес концентратора, протокола, тип транспорта, уровень ведения журнала, заголовки и другие параметры можно настроить при создании подключения. Настройте все необходимые параметры, вставляя `HubConnectionBuilder` методы в `Build`. Установить подключение с `StartAsync`.

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a>Вызов методов концентратора из клиента

`InvokeAsync` вызывает методы концентратора. Передайте имя метода концентратора и все аргументы, заданные в методе концентратора, чтобы `InvokeAsync`. SignalR является асинхронным, поэтому используйте `async` и `await` при выполнении вызовов.

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a>Вызывать методы клиента от концентратора

Определите методы концентратора вызовы с использованием `connection.On` после сборки, но прежде чем выполнять подключение.

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

Предыдущий код в `connection.On` выполняется, когда серверный код вызывает ее с помощью `SendAsync` метод.

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a>Обработка ошибок и ведение журнала

Обработка ошибок с помощью оператора try-catch. Проверьте `Exception` объектом, чтобы определить правильное действие, предпринимаемое после возникновения ошибки.

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Центры](xref:signalr/hubs)
* [Клиент JavaScript](xref:signalr/javascript-client)
* [Публикация в Azure](xref:signalr/publish-to-azure-web-app)
