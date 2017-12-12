---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: "Справочник по API концентраторов ASP.NET SignalR - клиент .NET (C#) | Документы Microsoft"
author: pfletcher
description: "В этом документе содержатся вводные сведения с помощью API концентраторов SignalR версии 2 в таких клиентов .NET, магазин Windows (WinRT), WPF, Silverlight и недостатки..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: a03c8c42622a768d706acf5ac1f23b37a830d426
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-signalr-hubs-api-guide---net-client-c"></a>Справочник по API концентраторов ASP.NET SignalR - клиент .NET (C#)
====================
по [Флетчера Патрик](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

> В этом документе содержатся вводные сведения с помощью API концентраторов SignalR версии 2 в таких клиентов .NET, магазин Windows (WinRT), WPF, Silverlight и консольных приложений.
> 
> API концентраторов SignalR позволяет вносить удаленные вызовы процедур (RPC) с сервера на подключенных клиентов и от клиентов к серверу. В серверном коде определять методы, которые могут быть вызваны клиентов и вызова методов, которые выполняются на клиенте. В клиентском коде определяют методы, которые могут быть вызваны с сервера и вызывать методы, которые выполняются на сервере. SignalR обеспечивает всех коммуникаций клиент сервер для вас.
> 
> SignalR также предлагает API нижнего уровня, который называется постоянные подключения. Общие сведения о SignalR, концентраторов и постоянные подключения или учебник, в котором показано, как построить полное приложение SignalR см. [SignalR — начало работы](../getting-started/index.md).
> 
> ## <a name="software-versions-used-in-this-topic"></a>Версии программного обеспечения, используемого в этом разделе
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR версии 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Предыдущие версии этого раздела
> 
> Сведения о более ранних версий SignalR см. в разделе [более ранних версий SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Вопросы и комментарии
> 
> Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить в комментарии в нижней части страницы. Если у вас есть вопросы, которые не связаны непосредственно для работы с учебником, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Обзор

Этот документ содержит следующие разделы.

- [Установка клиента](#clientsetup)
- [Как установить соединение](#establishconnection)

    - [Соединения между доменами от клиентов Silverlight](#slcrossdomain)
- [Настройка соединения](#configureconnection)

    - [Как задать максимальное число одновременных подключений клиентов WPF](#maxconnections)
    - [Как задать параметры строки запроса](#querystring)
    - [Как задать метод транспорта](#transport)
    - [Как задать заголовки HTTP](#httpheaders)
    - [Практическое задание сертификатов клиентов](#clientcertificate)
- [Создание прокси-сервера концентратора](#proxy)
- [Как определить на стороне клиента, сервер может вызывать методы](#callclient)

    - [Методы без параметров](#clientmethodswithoutparms)
    - [Методы с параметрами, указание типов параметров](#clientmethodswithparmtypes)
    - [Методы с параметрами, указав динамических объектов для параметров](#clientmethodswithdynamparms)
    - [Удаление обработчика](#removehandler)
- [Способ вызова методов сервера от клиента](#callserver)
- [Как обрабатывать события времени жизни соединений](#connectionlifetime)
- [Способ обработки ошибок](#handleerrors)
- [Как включить ведение журнала на стороне клиента](#logging)
- [WPF, Silverlight и образцы кода приложения консоли для методов клиента, сервер может вызывать](#wpfsl)

Для клиентских проектов .NET образца см. следующие ресурсы:

- [Андрей armenta / SignalR образцы](https://github.com/gustavo-armenta/SignalR-Samples) на GitHub.com (примеры приложений WinRT, Silverlight, консоль).
- [DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) на GitHub.com (например, WPF).
- [SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) на GitHub.com (пример консольного приложения).

Документацию по программированию сервера или клиентов JavaScript см. следующие ресурсы:

- [Справочник по API концентраторов SignalR - сервера](hubs-api-guide-server.md)
- [Справочник по API концентраторов SignalR - клиент JavaScript](hubs-api-guide-javascript-client.md)

Ссылки на разделы справки по API, до версии .NET 4.5 API-интерфейса. Если вы используете .NET 4, см. раздел [версии .NET 4 разделов API](https://msdn.microsoft.com/en-us/library/jj891075(v=vs.100).aspx).

<a id="clientsetup"></a>

## <a name="client-setup"></a>Установка клиента

Установка [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) пакет NuGet (не [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) пакета). Данный пакет поддерживает WinRT, Silverlight, WPF, консольное приложение и клиенты Windows Phone для .NET 4 и .NET 4.5.

Версия SignalR, у вас есть на стороне клиента отличается от версии, у вас есть на сервере, SignalR часто является возможность адаптироваться к разницу. Например на сервер под управлением SignalR версии 2 будет поддерживать клиентов, имеющих 1.1.x установлен, а также клиенты, имеющие версии 2 установлены. Если разница между версию на сервере и на клиентском компьютере слишком велико, или если клиент является более новой версии сервера, создает SignalR `InvalidOperationException` исключения, когда клиент пытается установить соединение. Сообщение об ошибке «`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`».

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Как установить соединение

Перед установкой соединения необходимо создать `HubConnection` объекта и создать учетную запись-посредник. Чтобы установить соединение, вызовите `Start` метод `HubConnection` объекта.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> Для клиентов JavaScript необходимо зарегистрировать по крайней мере один обработчик событий перед вызовом `Start` метод, чтобы установить соединение. Это не требуется для клиентов .NET. Для клиентов JavaScript, созданный код прокси автоматически создает учетных записей-посредников для всех концентраторов, которые существуют на сервере, и регистрация обработчика — это как можно указать, какие концентраторов клиент планирует использовать. Но для клиента .NET создать прокси-серверы концентратора вручную, поэтому SignalR предполагается, что вы используете любой концентратора, создать прокси для.


В примере кода используется значение по умолчанию «/ signalr» URL-адрес для подключения к службе SignalR. Сведения о том, как задать другой базовый URL-адрес см. в разделе [API концентраторов руководство - Server - /signalr URL-адрес для ASP.NET SignalR](hubs-api-guide-server.md#signalrurl).

`Start` Метод выполняется асинхронно. Чтобы убедиться в том, последующие строки кода, которые не выполняются до после установки подключения, используйте `await` в асинхронном методе ASP.NET 4.5 или `.Wait()` в синхронный метод. Не используйте `.Wait()` в клиенте WinRT.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a>Соединения между доменами от клиентов Silverlight

Сведения о том, как разрешить междоменные подключения от клиентов Silverlight см. в разделе [внесения службы через границы домена](https://msdn.microsoft.com/en-us/library/cc197955(v=vs.95).aspx).

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Настройка соединения

Перед установкой соединения можно указать любой из следующих параметров:

- Ограничение одновременных подключений.
- Параметры строки запроса.
- Метод транспорта.
- Заголовки HTTP.
- Сертификаты клиентов.

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a>Как задать максимальное число одновременных подключений клиентов WPF

В WPF клиентов может потребоваться увеличить максимальное количество одновременных подключений, значение по умолчанию 2. Рекомендуемое значение — 10.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

Дополнительные сведения см. в разделе [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/en-us/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Как задать параметры строки запроса

Если вы хотите отправлять данные на сервер при подключении клиента, можно добавить параметры строки запроса для объекта соединения. В следующем примере показано, как параметра строки запроса в клиентском коде.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

Приведенный ниже показано, как прочитать параметр строки запроса в серверном коде.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Как задать метод транспорта

В составе процесса подключения клиента SignalR обычно запрашивает у сервера для определения наиболее транспорта, который поддерживается с сервера и клиента. Если вы уже знаете, какие транспорта, который вы хотите использовать, вы можете обойти этот процесс согласования. Чтобы указать метод транспорта, передайте объект транспорта к методу Start. Приведенный ниже показано, как задать метод транспорта в клиентском коде.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

[Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/en-us/library/jj918090(v=vs.111).aspx) пространство имен включает следующие классы, которые можно использовать для указания транспорта.

- [LongPollingTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)
- [ServerSentEventsTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)
- [WebSocketTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (доступен только в том случае, если сервер и клиент используют .NET 4.5.)
- [AutoTransport](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (автоматически выбирает лучший транспорта, поддерживаемые клиентом и сервером. Это стандартный транспорт. Это в для передачи `Start` метод действует так же, как в объекте, не передавая.)

ForeverFrame транспорта не включены в этот список, поскольку он используется только в браузерах.

Сведения о проверке метод транспорта в серверном коде см. в разделе [ASP.NET руководство по API концентраторов SignalR - Server - способы получения сведений о клиенте из контекстного свойства](hubs-api-guide-server.md#contextproperty). Дополнительные сведения о транспортов и в случае ошибки см. в разделе [Общие сведения о SignalR - транспорта и в случае ошибки](../getting-started/introduction-to-signalr.md#transports).

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a>Как задать заголовки HTTP

Чтобы задать заголовки HTTP, используйте `Headers` свойство для объекта connection. Следующий пример демонстрирует добавление заголовка HTTP.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a>Практическое задание сертификатов клиентов

Чтобы добавить сертификаты клиентов, используйте `AddClientCertificate` метод для объекта connection.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a>Создание прокси-сервера концентратора

Для определения методов на стороне клиента, можно вызвать концентратор с сервера, а также вызывать методы концентратора на сервере, создайте прокси-сервер для концентратора путем вызова `CreateHubProxy` для объекта connection. Строка передается в `CreateHubProxy` является именем класса концентратора или имя, указанное в `HubName` атрибута, если она использовалась на сервере. Совпадение имен регистр не учитывается.

**Класс концентратора на сервере**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

**Создайте клиентский прокси для класса концентратора**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

Если вы устанавливаете класса концентратора `HubName` атрибут, используйте его.

**Класс концентратора на сервере**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

**Создайте клиентский прокси для класса концентратора**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

При вызове метода `HubConnection.CreateHubProxy` несколько раз с одинаковым `hubName`, будут получены в кэше же `IHubProxy` объекта.

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Как определить на стороне клиента, сервер может вызывать методы

Чтобы определить метод, который можно вызвать сервера, использовать прокси-сервер `On` метода для регистрации обработчика событий.

Метод совпадение имен регистр не учитывается. Например `Clients.All.UpdateStockPrice` на сервере будет выполняться `updateStockPrice`, `updatestockprice`, или `UpdateStockPrice` на стороне клиента.

Разные клиентские платформы имеют различные требования к написание кода в метод для обновления пользовательского интерфейса. В примерах предназначены для клиентов WinRT (.NET для магазина Windows). WPF, Silverlight и консольного приложения примеры приведены в [отдельный раздел далее в этом разделе](#wpfsl).

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a>Методы без параметров

Если метод обработка не имеют параметров, используйте перегруженный неуниверсальных `On` метод:

**Серверный код вызова клиентского метода без параметров**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

**WinRT клиентский код для метода вызвана с сервера без параметров ([примеры WPF и Silverlight далее в этом разделе](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Методы с параметрами, указывающих типы параметров

Если метод обработка имеет параметры, укажите типы параметров как универсальные типы `On` метод. Существуют перегрузки универсального `On` метод, чтобы указать параметры до 8 (4 на Windows Phone 7). В следующем примере отправляется один параметр `UpdateStockPrice` метода.

**Вызов клиентского метода с параметром серверного кода**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

**Класс Stock, используемое для параметра**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

**WinRT клиентский код для метода вызвана с сервера с параметром ([примеры WPF и Silverlight далее в этом разделе](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Методы с параметрами, указав динамических объектов для параметров

В качестве альтернативы для задания параметров, универсальных типов `On` метод, параметры можно указать в качестве динамических объектов:

**Вызов клиентского метода с параметром серверного кода**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

**Класс Stock, используемое для параметра**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

**WinRT клиентский код для метода вызвана с сервера с параметром, с помощью динамического объекта для параметра ([примеры WPF и Silverlight далее в этом разделе](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a>Удаление обработчика

Чтобы удалить обработчик, вызовите его `Dispose` метод.

**Клиентский код для метода с названием с сервера**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

**Клиентский код для удаления обработчика**

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Способ вызова методов сервера от клиента

Чтобы вызвать метод на сервере, используйте `Invoke` метод для прокси-сервера концентратора.

Если метод сервера не имеет возвращаемого значения, используйте перегруженный неуниверсальных `Invoke` метод.

**Серверный код для метода, который не имеет возвращаемого значения**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

**Код клиента, вызов метода, который не имеет возвращаемого значения**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

Если возвращаемое значение метода сервера, задать возвращаемый тип как универсальный тип `Invoke` метод.

**Серверный код для метода, который имеет возвращаемое значение и принимает параметр сложный тип**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

**Биржевая класс, используемый для параметра и возвращаемого значения**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

**Код клиента, вызов метода, который имеет возвращаемое значение и принимает параметр сложного типа в методе async ASP.NET 4.5**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

**Код клиента, вызов метода, который имеет возвращаемое значение и принимает сложного параметра типа, в синхронный метод**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

`Invoke` Метод выполняется асинхронно и возвращает `Task` объекта. Если не указать `await` или `.Wait()`, следующая строка кода будет выполняться до завершения выполнения метода, который можно вызывать.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Как обрабатывать события времени жизни соединений

SignalR обеспечивает следующее подключение события времени жизни, которые можно обрабатывать:

- `Received`: Возникает при получении данных для соединения. Предоставляет полученных данных.
- `ConnectionSlow`: Возникает, если клиент обнаруживает подключение медленное или часто удаление.
- `Reconnecting`: Возникает в случае транспорта начинает повторное подключение.
- `Reconnected`: Возникает, когда была повторно присоединена транспорта.
- `StateChanged`: Возникает при изменении состояния подключения. Предоставляет состояния старое и новое состояние. Сведения о подключения содержатся значения состояния [перечисление ConnectionState](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).
- `Closed`: Возникает, когда произойдет отключение соединения.

Например, если вы хотите отображать предупреждающие сообщения для ошибок, которые не являются неустранимыми, но нарушить временное подключение, таких как низкая или слишком частой удаление соединения, обрабатывают `ConnectionSlow` событий.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

Дополнительные сведения см. в разделе [понимание и обработка события времени жизни соединений в SignalR](handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Способ обработки ошибок

Если подробные сообщения об ошибках на сервере не включен явно, объект исключения, которое возвращает SignalR после возникновения ошибки содержит минимум информации об ошибке. Например, если вызов `newContosoChatMessage` завершается ошибкой, содержит сообщение об ошибке в объекте ошибки «`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`» отправки подробных сообщений об ошибках на клиентах в рабочей среде не рекомендуется по соображениям безопасности, но если вы хотите включить подробные сообщения об ошибках для устранения неполадок, используйте следующий код на сервере.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

Для обработки ошибок, которые вызывает SignalR, можно добавить обработчик `Error` событий для объекта connection.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

Для обработки ошибок из вызовов методов, необходимо заключите код в блок try-catch.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Как включить ведение журнала на стороне клиента

Чтобы включить ведение журнала на стороне клиента, задайте `TraceLevel` и `TraceWriter` свойства для объекта connection.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a>WPF, Silverlight и образцы кода приложения консоли для методов клиента, сервер может вызывать

Примеры кода, приведенный выше, для определения методов клиента, сервер может вызывать применяются к клиентам WinRT. В следующих примерах показано эквивалентный код для WPF, Silverlight и консоли приложений-клиентов.

### <a name="methods-without-parameters"></a>Методы без параметров

**WPF клиентский код для метода вызвана с сервера без параметров**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

**Код клиента Silverlight для метода вызвана с сервера без параметров**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

**Код клиента приложения консоли для метода вызвана с сервера без параметров**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Методы с параметрами, указывающих типы параметров

**WPF клиентский код для метода с названием с сервера с параметром**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

**Код клиента Silverlight для вызвана с сервера с параметром метода**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

**Код клиента приложения консоли для метода вызвана с сервера с параметром**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Методы с параметрами, указав динамических объектов для параметров

**WPF клиентский код для метода с названием с сервера с параметром, с помощью динамического объекта для параметра**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

**Код клиента Silverlight для вызвана с сервера с параметром, с помощью динамического объекта для параметра метода**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

**Код клиента приложения консоли для метода вызвана с сервера с параметром, с помощью динамического объекта для параметра**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
