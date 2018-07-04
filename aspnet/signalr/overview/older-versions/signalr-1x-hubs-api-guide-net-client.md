---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
title: Руководство по API концентраторов ASP.NET SignalR — клиент .NET (SignalR 1.x) | Документация Майкрософт
author: pfletcher
description: Этот документ представляет собой введение по API концентраторов SignalR версии 2 в таких клиентов .NET, Windows Store (WinRT), WPF, Silverlight и «против»...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: c334adc3-d6dc-44f3-9f06-f7634475aad3
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 948384298366ba87effeaaff55f38e4edc7be940
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389538"
---
<a name="aspnet-signalr-hubs-api-guide---net-client-signalr-1x"></a>Руководство по API концентраторов ASP.NET SignalR — клиент .NET (SignalR 1.x)
====================
по [Флетчера Патрик](https://github.com/pfletcher), [том Дайкстра](https://github.com/tdykstra)

> Этот документ содержит вводные сведения по API концентраторов SignalR версии 2 в таких клиентов .NET, Windows Store (WinRT), WPF, Silverlight и консольных приложений.
> 
> API концентраторов SignalR позволяет вам выбрать удаленные вызовы процедур (RPC), с сервера подключенным клиентам и от клиентов к серверу. В серверном коде определяют методы, которые могут быть вызваны клиентов и вызывать методы, которые выполняются на клиенте. В клиентском коде определяют методы, которые могут вызываться с сервера и вызывать методы, которые выполняются на сервере. SignalR берет на себя все необходимое для вас клиент сервер.
> 
> SignalR также предлагает API низкого уровня, вызывается постоянные подключения. Введение в SignalR, концентраторы и постоянные подключения, или в этом учебнике показано, как создать полное приложение SignalR, см. в разделе [SignalR — Приступая к работе](../getting-started/index.md).


## <a name="overview"></a>Обзор

Этот документ содержит следующие разделы.

- [Установка клиента](#clientsetup)
- [Как установить соединение](#establishconnection)

    - [Междоменные подключения от клиентов Silverlight](#slcrossdomain)
- [Настройка подключения](#configureconnection)

    - [Как задать максимальное число одновременных подключений клиентов WPF](#maxconnections)
    - [Как указать параметры строки запроса](#querystring)
    - [Как указать метод транспорта](#transport)
    - [Как указать заголовки HTTP](#httpheaders)
    - [Практическое задание сертификатов клиентов](#clientcertificate)
- [Как создать прокси-сервера концентратора](#proxy)
- [Как определить методы на клиенте, который сервер может обратиться](#callclient)

    - [Методы без параметров](#clientmethodswithoutparms)
    - [Методы с параметрами, указав типы параметров](#clientmethodswithparmtypes)
    - [Методы с параметрами, указав динамические объекты для параметров](#clientmethodswithdynamparms)
    - [Как удалить обработчик](#removehandler)
- [Как вызывать методы сервера от клиента](#callserver)
- [Способ обработки событий времени существования подключений](#connectionlifetime)
- [Способ обработки ошибок](#handleerrors)
- [Как включить ведение журнала на стороне клиента](#logging)
- [WPF, Silverlight и образцов кода консольных приложений для клиентских методов, которые могут вызывать сервера](#wpfsl)

Для образцов проектов клиента .NET ознакомьтесь со следующими ресурсами:

- [Андрей armenta / SignalR — примеры](https://github.com/gustavo-armenta/SignalR-Samples) на сайте GitHub.com (примеры приложений WinRT, Silverlight, консоли).
- [DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) на сайте GitHub.com (например, WPF).
- [SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) на сайте GitHub.com (например, приложение консоли).

Документацию по программированию сервера или клиентов JavaScript, ознакомьтесь со следующими ресурсами:

- [Руководство по API концентраторов SignalR - сервер](../guide-to-the-api/hubs-api-guide-server.md)
- [Руководство по API концентраторов SignalR — клиент JavaScript](../guide-to-the-api/hubs-api-guide-javascript-client.md)

Приведены ссылки на разделы, справочник по API .NET 4.5 версия API. Если вы используете .NET 4, см. в разделе [версия .NET 4 разделов API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="clientsetup"></a>

## <a name="client-setup"></a>Установка клиента

Установка [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) пакет NuGet (не [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) пакета). Этот пакет поддерживает WinRT, Silverlight, WPF, консольного приложения и клиенты Windows Phone, для .NET 4 и .NET 4.5.

Если версия SignalR, у вас на стороне клиента отличается от версии, у вас есть на сервере, SignalR часто имеет возможность адаптироваться к разницу. Например при выпуске SignalR версии 2.0, а также установки, на сервере, сервер будет поддерживать клиентов, имеющих 1.1.x, а также клиенты, имеющие 2.0 установлена. Если разница между версию на сервере и на клиентском компьютере слишком велико, SignalR вызывает `InvalidOperationException` исключение, когда клиент пытается установить соединение. Сообщение об ошибке "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`«.

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Как установить соединение

Чтобы установить соединение, то необходимо создать `HubConnection` объект и создать учетную запись-посредник. Чтобы установить подключение, вызов `Start` метод `HubConnection` объекта.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> Для клиентов JavaScript, вам необходимо зарегистрировать по крайней мере один обработчик событий, перед вызовом `Start` метод, чтобы установить соединение. Это не является обязательным для клиентов .NET. Для клиентов JavaScript, созданный код прокси автоматически создает прокси-серверы для всех концентраторов, которые существуют на сервере, и регистрация обработчика — указывает, какие центры клиент планирует использовать. Но для клиента .NET создать прокси-серверы центра вручную, поэтому SignalR предполагается, что вы будете использовать любой центр, создать прокси для.


В примере кода используется значение по умолчанию «/ signalr» URL-адрес для подключения к службе SignalR. Сведения о том, как указать другой базовый URL-адрес, см. в разделе [ASP.NET руководство по API концентраторов SignalR - Server - URL-адрес /signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).

`Start` Метод выполняется асинхронно. Чтобы убедиться, что последующие строки кода, которые не выполняются до после установления соединения, используйте `await` в асинхронном методе ASP.NET 4.5 или `.Wait()` в синхронный метод. Не используйте `.Wait()` в клиенте WinRT.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

Класс `HubConnection` является потокобезопасным.

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a>Междоменные подключения от клиентов Silverlight

Сведения о включении междоменные подключения от клиентов Silverlight, см. в разделе [внесения службы через границы домена](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Настройка подключения

Перед установкой соединения можно указать любой из следующих вариантов:

- Ограничение одновременных подключений.
- Параметры строки запроса.
- Метод транспорта.
- Заголовки HTTP.
- Сертификаты клиента.

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a>Как задать максимальное число одновременных подключений клиентов WPF

В WPF клиентов может потребоваться увеличить максимальное число одновременных подключений со значения по умолчанию 2. Рекомендуемое значение — 10.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

Дополнительные сведения см. в разделе [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Как указать параметры строки запроса

Если вы хотите отправлять данные на сервер при подключении клиента, можно добавить параметры строки запроса на объект подключения. В следующем примере показано, как параметра строки запроса в клиентском коде.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample5.cs)]

Приведенный ниже показано, как прочитать параметр строки запроса в серверном коде.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Как указать метод транспорта

В рамках процесса подключения клиента SignalR обычно согласовывает с сервером, чтобы определить лучший транспорт, который поддерживается с сервера и клиента. Если вы уже знаете, какой транспорт, который вы хотите использовать, вы можете обойти этот процесс согласования. Чтобы указать метод транспорта, передайте объект транспорта к методу Start. Приведенный ниже показано, как указать метод транспорта в клиентском коде.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

[Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) пространство имен включает следующие классы, которые можно использовать для указания транспортного уровня.

- [LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)
- [ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)
- [WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (доступен только в том случае, когда сервер и клиент использовать .NET 4.5).
- [AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (автоматически выбирает лучший транспорт, который поддерживается клиентом и сервером. Это транспорта по умолчанию. Передача ее в `Start` метод имеет тот же эффект, что не передавая никаких действий.)

ForeverFrame транспорта не включен в этот список, так как он используется только в браузерах.

Сведения о том, как проверить метод транспорта в серверном коде, см. в разделе [ASP.NET руководство по API концентраторов SignalR - сервер — как для получения сведений о клиенте из контекстного свойства](../guide-to-the-api/hubs-api-guide-server.md#contextproperty). Дополнительные сведения о транспортах и в случае ошибки, см. в разделе [введение в SignalR - транспорта и в случае ошибки](../getting-started/introduction-to-signalr.md#transports).

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a>Как указать заголовки HTTP

Чтобы задать заголовки HTTP, используйте `Headers` свойство для объекта connection. Приведенный ниже показано, как добавить заголовок HTTP.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a>Практическое задание сертификатов клиентов

Чтобы добавить сертификаты клиента, используйте `AddClientCertificate` метод для объекта connection.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a>Как создать прокси-сервера концентратора

Чтобы определить методы на клиенте, который можно вызвать концентратор с сервера, а также вызывать методы концентратора на сервере, создать прокси-сервер для центра, вызвав `CreateHubProxy` для объекта connection. Строка передается в `CreateHubProxy` — это имя класса концентратора или имя, указанное в `HubName` атрибут, если план, который использовался на сервере. Сопоставление имен не учитывает регистр.

**Класс концентратора на сервере**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

**Создайте клиентский прокси для класса концентратора**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

Если вы устанавливаете центр класса с `HubName` атрибута, используйте его.

**Класс концентратора на сервере**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample12.cs)]

**Создайте клиентский прокси для класса концентратора**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

Прокси-объект является потокобезопасным. На самом деле, если вы вызываете `HubConnection.CreateHubProxy` несколько раз с тем же `hubName`, будут получены такие же кэшируются `IHubProxy` объекта.

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Как определить методы на клиенте, который сервер может обратиться

Чтобы определить метод, который сервер может обратиться, использовать прокси-сервер `On` метод, чтобы зарегистрировать обработчик событий.

Совпадение имен метод не учитывает регистр. Например `Clients.All.UpdateStockPrice` будет выполняться на сервере `updateStockPrice`, `updatestockprice`, или `UpdateStockPrice` на стороне клиента.

Разные клиентские платформы имеют различные требования к как написать код метода для обновления пользовательского интерфейса. Примеры, приведенные предназначены для клиентов WinRT (Windows Store .NET). WPF, Silverlight и примеры приложений консоли, указанные в [отдельном разделе этой статьи](#wpfsl).

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a>Методы без параметров

Если метод обработка не имеет параметров, используйте перегрузку метода нестандартную `On` метод:

**Серверный код вызова клиентского метода без параметров**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

**WinRT клиентский код для метода вызывается с сервера без параметров ([см. в разделе WPF и Silverlight примеры далее в этом разделе](#wpfsl))**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Методы с параметрами, указав типы параметров

Если метод обработка имеет параметры, укажите типы параметров, как универсальные типы `On` метод. Существуют универсальные перегрузки `On` метод, чтобы можно было указать параметры до 8 (4 на Windows Phone 7). В следующем примере передается один параметр `UpdateStockPrice` метод.

**Серверный код вызова клиентского метода с параметром**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

**Класс Stock, используемое для параметра**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample17.cs)]

**WinRT клиентский код для метода вызывается с сервера с параметром ([см. в разделе WPF и Silverlight примеры далее в этом разделе](#wpfsl))**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Методы с параметрами, указав динамические объекты для параметров

В качестве альтернативы для задания параметров, универсальных типов `On` метод, параметры можно указать в качестве динамических объектов:

**Серверный код вызова клиентского метода с параметром**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

**Класс Stock, используемое для параметра**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample20.cs)]

**WinRT клиентский код для метода вызывается с сервера с параметром, с помощью динамического объекта для параметра ([см. в разделе WPF и Silverlight примеры далее в этом разделе](#wpfsl))**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a>Как удалить обработчик

Чтобы удалить обработчик, вызовите его `Dispose` метод.

**Клиентский код для метод, вызываемый из сервера**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

**Клиентский код для удаления обработчика**

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Как вызывать методы сервера от клиента

Чтобы вызвать метод на сервере, используйте `Invoke` метод на прокси-сервера концентратора.

Если сервер метод не возвращает никакого значения, воспользуйтесь перегрузкой неуниверсальных `Invoke` метод.

**Серверный код для метода, который не имеет возвращаемого значения**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

**Код клиента, вызов метода, который не имеет возвращаемого значения**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

Если метод сервера имеет возвращаемое значение, укажите тип возвращаемого значения как универсальный тип `Invoke` метод.

**Серверный код для метода, который имеет возвращаемое значение и принимает параметр сложного типа**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

**Класс Stock, используемый для параметра и возвращаемого значения**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample27.cs)]

**Клиентский код, вызвав метод, который имеет возвращаемое значение и принимает параметр сложного типа, в асинхронном методе ASP.NET 4.5**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

**Клиентский код, вызвав метод, который имеет возвращаемое значение и принимает параметр сложного типа, синхронный метод**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

`Invoke` Метод выполняется асинхронно и возвращает `Task` объекта. Если вы не укажете `await` или `.Wait()`, следующая строка кода будет выполняться до завершения выполнения метода, который вы вызываете.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Способ обработки событий времени существования подключений

SignalR обеспечивает следующее подключение события времени жизни, которые можно обработать:

- `Received`: Возникает при получении данных для соединения. Предоставляет полученных данных.
- `ConnectionSlow`: Возникает, когда клиент обнаруживает подключение медленно или часто удаление.
- `Reconnecting`: Возникает, когда используемому транспорту начинает повторное подключение.
- `Reconnected`: Возникает, когда была повторно присоединена используемому транспорту.
- `StateChanged`: Возникает при изменении состояния подключения. Предоставляет состояние старое и новое состояние. Сведения о подключении, значения состояния см. в разделе [перечисление ConnectionState](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).
- `Closed`: Возникает, когда произойдет отключение соединения.

Например, если вы хотите отображать предупреждающие сообщения для ошибок, которые не являются очень существенными, но вызывают проблемы с промежуточными соединениями, такие как медленная работа или часто удаление соединения, обрабатывать `ConnectionSlow` событий.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample30.cs)]

Дополнительные сведения см. в разделе [понимание и обработка событий времени существования подключений в SignalR](../guide-to-the-api/handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Способ обработки ошибок

Если подробные сообщения об ошибках на сервере не включен явно, объект исключения, которое возвращает SignalR после возникновения ошибки содержит минимум информации об ошибке. Например, если вызов `newContosoChatMessage` завершается ошибкой, содержит сообщение об ошибке в объекте ошибки "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Отправка подробных сообщений об ошибках клиентов в рабочей среде не рекомендуется по соображениям безопасности, но если вы хотите включить подробные сообщения об ошибках для устранения неполадок, используйте следующий код на сервере.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

Для обработки ошибок, которые вызывает SignalR, можно добавить обработчик для `Error` событие для объекта подключения.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample32.cs)]

Для обработки ошибок из вызовов методов, поместите код в блок try-catch.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Как включить ведение журнала на стороне клиента

Чтобы включить ведение журнала на стороне клиента, задайте `TraceLevel` и `TraceWriter` свойства в объекте подключения.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a>WPF, Silverlight и образцов кода консольных приложений для клиентских методов, которые могут вызывать сервера

Примеры кода, показанный ранее, для определения методов клиента, которые могут вызывать сервера применяются к клиентам WinRT. В следующих примерах показано эквивалентный код для WPF, Silverlight и консоли приложений-клиентов.

### <a name="methods-without-parameters"></a>Методы без параметров

**WPF клиентский код для метода с сервера без параметров**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

**Код клиента Silverlight для метод, вызываемый с сервера без параметров**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

**Код клиента приложения консоли для метода вызывается с сервера без параметров**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Методы с параметрами, указав типы параметров

**Код клиента WPF для метод, вызываемый из сервера с параметром**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

**Код клиента Silverlight для метод, вызываемый из сервера с параметром**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

**Код клиента приложения консоли для метода вызывается с сервера с параметром**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Методы с параметрами, указав динамические объекты для параметров

**Код клиента WPF для метод, вызываемый из сервера с параметром, с помощью динамического объекта для параметра**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

**Код клиента Silverlight для метод, вызываемый из сервера с параметром, с помощью динамического объекта для параметра**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

**Код клиента приложения консоли для метода вызывается с сервера с параметром, с помощью динамического объекта для параметра**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
