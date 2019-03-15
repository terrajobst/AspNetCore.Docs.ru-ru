---
title: Клиент .NET SignalR ASP.NET Core
author: bradygaster
description: Сведения о клиенте .NET, ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 03/14/2019
uid: signalr/dotnet-client
ms.openlocfilehash: a03abef53aa44f0a1016b8f72d8e3a7af2f9bed1
ms.sourcegitcommit: d913bca90373c07f89b1d1df01af5fc01fc908ef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/14/2019
ms.locfileid: "57978308"
---
# <a name="aspnet-core-signalr-net-client"></a>Клиент .NET SignalR ASP.NET Core

Клиентская библиотека ASP.NET Core SignalR .NET позволяет взаимодействовать с концентраторами SignalR из приложений .NET.

> [!NOTE]
> Xamarin имеет особые требования для версии Visual Studio. Дополнительные сведения см. в разделе [клиентом SignalR 2.1.1 в Xamarin](https://github.com/aspnet/Announcements/issues/305).

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([как скачивать](xref:index#how-to-download-a-sample))

В образце кода в этой статье показана приложения WPF, использующее клиент ASP.NET Core SignalR .NET.

## <a name="install-the-signalr-net-client-package"></a>Установка пакета клиента SignalR .NET

`Microsoft.AspNetCore.SignalR.Client` Пакет требуется для клиентов .NET для подключения к концентраторов SignalR. Чтобы установить клиентскую библиотеку, выполните следующую команду **консоль диспетчера пакетов** окна:

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>Подключение к концентратору

Чтобы установить подключение, создайте `HubConnectionBuilder` и вызвать `Build`. URL-адрес концентратора, протокола, тип транспорта, уровень ведения журнала, заголовки и другие параметры можно настроить при создании подключения. Настройте все необходимые параметры, вставляя `HubConnectionBuilder` методы в `Build`. Установить подключение с `StartAsync`.

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a>Дескриптор была потеряна связь

Используйте <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> событий реагировать на потерю соединения. Например можно автоматизировать повторное подключение.

`Closed` Событие требует делегат, который возвращает `Task`, что позволяет асинхронного кода для выполнения без использования `async void`. Для удовлетворения сигнатуре делегата в `Closed` обработчик событий, который выполняется синхронно, возвращают `Task.CompletedTask`:

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

Основная причина для поддержки асинхронных является, поэтому вы можете перезапустить подключение. Подключения — это действие async.

В `Closed` обработчик, который перезапускает соединения, рассмотрим некоторые случайные задержки предотвратить перегрузку сервера, как показано в следующем примере:

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a>Вызов методов концентратора из клиента

`InvokeAsync` вызывает методы концентратора. Передайте имя метода концентратора и все аргументы, заданные в методе концентратора, чтобы `InvokeAsync`. SignalR является асинхронным, поэтому используйте `async` и `await` при выполнении вызовов.

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

> [!NOTE]
> Если вы используете службу Azure SignalR в *бессерверной режим*, нельзя вызывать методы концентратора клиента. Дополнительные сведения см. в разделе [документации по службе SignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).

## <a name="call-client-methods-from-hub"></a>Вызывать методы клиента от концентратора

Определите методы концентратора вызовы с использованием `connection.On` после сборки, но прежде чем выполнять подключение.

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

Предыдущий код в `connection.On` выполняется, когда серверный код вызывает ее с помощью `SendAsync` метод.

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a>Обработка ошибок и ведение журнала

Обработка ошибок с помощью оператора try-catch. Проверьте `Exception` объектом, чтобы определить правильное действие, предпринимаемое после возникновения ошибки.

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Центры](xref:signalr/hubs)
* [Клиент JavaScript](xref:signalr/javascript-client)
* [Публикация в Azure](xref:signalr/publish-to-azure-web-app)
* [SignalR бессерверной документация по службе Azure](/azure/azure-signalr/signalr-concept-serverless-development-config)
