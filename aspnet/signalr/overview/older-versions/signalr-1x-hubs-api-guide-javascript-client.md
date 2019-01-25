---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
title: SignalR 1.x по API концентраторов — клиента JavaScript | Документация Майкрософт
author: bradygaster
description: Этот документ представляет собой введение по API концентраторов SignalR версии 1.1 в клиентах JavaScript, таких как браузеры и Store Windows (WinJS) рабоче...
ms.author: bradyg
ms.date: 04/17/2013
ms.assetid: dcd4593b-1118-418a-af71-d12ff33fb36d
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: eb40648ca06adcceaa613ba86abfcf7459369c7e
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836757"
---
<a name="signalr-1x-hubs-api-guide---javascript-client"></a>SignalR 1.x по API концентраторов — клиента JavaScript
====================
по [Флетчера Патрик](https://github.com/pfletcher), [том Дайкстра](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Этот документ содержит вводные сведения по API концентраторов SignalR версии 1.1 в клиентах JavaScript, таких как браузеры и приложения Windows Store (WinJS).
> 
> API концентраторов SignalR позволяет вам выбрать удаленные вызовы процедур (RPC), с сервера подключенным клиентам и от клиентов к серверу. В серверном коде определяют методы, которые могут быть вызваны клиентов и вызывать методы, которые выполняются на клиенте. В клиентском коде определяют методы, которые могут вызываться с сервера и вызывать методы, которые выполняются на сервере. SignalR берет на себя все необходимое для вас клиент сервер.
> 
> SignalR также предлагает API низкого уровня, вызывается постоянные подключения. Введение в SignalR, концентраторы и постоянные подключения, или в этом учебнике показано, как создать полное приложение SignalR, см. в разделе [SignalR — Приступая к работе](../getting-started/index.md).


## <a name="overview"></a>Обзор

Этот документ содержит следующие разделы.

- [Созданный прокси и что он делает для вас](#genproxy)

    - [Когда следует использовать созданный прокси](#cantusegenproxy)
- [Установка клиента](#clientsetup)

    - [Способ создания ссылок динамически создаваемого прокси-сервера](#dynamicproxy)
    - [Как создать физический файл для SignalR созданный прокси-сервера](#manualproxy)
- [Как установить соединение](#establishconnection)

    - [$. connection.hub совпадает с объектом этого $.hubConnection() создает](#connequivalence)
    - [Асинхронное выполнение метода start](#asyncstart)
- [Как установить подключение между доменами](#crossdomain)
- [Настройка подключения](#configureconnection)

    - [Как указать параметры строки запроса](#querystring)
    - [Как указать метод транспорта](#transport)
- [Как получить учетную запись-посредник для класса концентратора](#getproxy)
- [Как определить методы на клиенте, который сервер может обратиться](#callclient)
- [Как вызывать методы сервера от клиента](#callserver)
- [Способ обработки событий времени существования подключений](#connectionlifetime)
- [Способ обработки ошибок](#handleerrors)
- [Как включить ведение журнала на стороне клиента](#logging)

Документацию по программированию сервера или клиентов .NET, ознакомьтесь со следующими ресурсами:

- [Руководство по API концентраторов SignalR - сервер](../guide-to-the-api/hubs-api-guide-server.md)
- [Руководство по API концентраторов SignalR — клиент .NET](../guide-to-the-api/hubs-api-guide-net-client.md)

Приведены ссылки на разделы, справочник по API .NET 4.5 версия API. Если вы используете .NET 4, см. в разделе [версия .NET 4 разделов API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a>Созданный прокси и что он делает для вас

Можно запрограммировать клиента JavaScript для взаимодействия со службой SignalR с или без прокси-сервер, созданный SignalR для вас. Прокси-сервер может сделать для вас является упростить синтаксис кода можно использовать для подключения, методы записи, которые сервер вызывает метод, и вызывать методы на сервере.

При написании кода для вызова методов сервера, созданного прокси позволяет использовать синтаксис, который выглядит так, будто выполнялись локальной функции: можно написать `serverMethod(arg1, arg2)` вместо `invoke('serverMethod', arg1, arg2)`. Если вы введете имя метода сервера синтаксис созданного прокси позволяет ошибку на стороне клиента будут понятны и окно интерпретации. И если вручную создать файл, который определяет прокси-серверы, можно также обеспечить поддержку IntelliSense для написания кода, который вызывает методы сервера.

Например предположим, что у вас есть следующий класс концентратора на сервере:

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

В следующих примерах кода показано, как выглядит код JavaScript для вызова `NewContosoChatMessage` метод на сервере, а также получать вызовы `addContosoChatMessageToPage` метод с сервера.

**С помощью созданного прокси**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

**Без созданный прокси-сервера**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a>Когда следует использовать созданный прокси

Если вы хотите зарегистрировать несколько обработчиков событий для метод клиента, сервер вызывает метод, нельзя использовать созданный прокси. В противном случае вы можете использовать созданный прокси или не зависимости от используемого языка программирования. Если вы решили не использовать его, нет необходимости ссылаться на «/ концентраторов signalr» URL-адрес в `script` элемент в коде клиента.

<a id="clientsetup"></a>

## <a name="client-setup"></a>Установка клиента

Клиент JavaScript требуются ссылки на jQuery и JavaScript core SignalR. 1.6.4 или основных более поздних версий, например 1.7.2, 1.8.2 или 1.9.1, требуется версия jQuery. Если вы решили использовать созданный прокси, необходимо также ссылку на прокси-сервера SignalR созданный файл JavaScript. В следующем примере показано, как может выглядеть ссылки на странице HTML, который использует созданный прокси.

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample4.html)]

Эти ссылки должны быть включены в следующем порядке: jQuery, SignalR core после этого и прокси-серверы SignalR Фамилия.

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a>Способ создания ссылок динамически создаваемого прокси-сервера

В предыдущем примере ссылка на прокси-сервер создается SignalR — динамически созданный код JavaScript, не к какому файлу. SignalR создает код JavaScript для прокси-сервера в режиме реального времени и обслуживает его клиенту в ответ на URL-адрес «/ signalr/концентраторы». Если вы указали другой базовый URL-адрес для подключений SignalR на сервере в вашей `MapHubs` , URL-адрес динамически создаваемого файла посредника является пользовательский URL-адрес с «/ концентраторы» добавленным к нему.

> [!NOTE]
> Для клиентов Windows 8 (Windows Store) JavaScript используйте физической посредника вместо того, динамически создаваемый. Дополнительные сведения см. в разделе [прокси, созданного как создать физический файл для SignalR](#manualproxy) далее в этом разделе.


В представлении ASP.NET MVC 4 Razor используйте тильда для обращения к корневой папке приложения в файл справки прокси-сервера:

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample5.html)]

Дополнительные сведения об использовании SignalR в MVC 4, см. в разделе [начало работы с SignalR и MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).

В представлении Razor в ASP.NET MVC 3, используйте `Url.Content` для прокси-сервера файл справки:

[!code-cshtml[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample6.cshtml)]

В приложении веб-форм ASP.NET использовать `ResolveClientUrl` для ваших прокси-серверов, ссылка на файл, или зарегистрировать его с помощью ScriptManager, с помощью приложения корневой путь (начинающийся с тильды):

[!code-aspx[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample7.aspx)]

Как правило используйте тот же метод для указания URL-адреса с «/ signalr/концентраторы», используемого для файлов CSS или JavaScript. При указании URL-адрес без использования тильда в некоторых сценариях приложение будет работать правильно при тестирования в Visual Studio, используя IIS Express, но будут завершаться сообщение об ошибке 404, при развертывании на полноценный сервер IIS. Дополнительные сведения см. в разделе **разрешении ссылок на ресурсы корневого уровня** в [веб-серверов в Visual Studio для веб-проектов ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx) на сайте MSDN.

При запуске веб-проекта в Visual Studio 2012 в режиме отладки, и если вы используете Internet Explorer как браузер, вы увидите файл прокси в **обозревателе решений** под **документы скриптов**, как показано на на рисунке.

![Файл созданного прокси JavaScript в обозревателе решений](signalr-1x-hubs-api-guide-javascript-client/_static/image1.png)

Чтобы просмотреть содержимое файла, дважды щелкните **концентраторов**. Если вы не используете Visual Studio 2012 и Internet Explorer, или если вы не в режиме отладки, содержимое файла можно получить, перейдя по адресу «/ signalR/концентраторы» URL-адрес. Например, если ваш сайт работает в `http://localhost:56699`, перейдите в меню `http://localhost:56699/SignalR/hubs` в браузере.

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a>Как создать физический файл для SignalR созданный прокси-сервера

В качестве альтернативы на динамически созданный прокси-сервер можно создать физический файл, содержащий код прокси-сервера и ссылку на этот файл. Вы можете это сделать, для контроля над кэшированием или объединение поведение, или для получения IntelliSense при написании кода вызовы методов сервера.

Чтобы создать прокси-файл, выполните следующие действия.

1. Установка [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) пакет NuGet.
2. Откройте командную строку и перейдите к *средства* папку, содержащую файл SignalR.exe. В папку средств находится по следующему адресу:

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.1.0.1\tools`
3. Введите следующую команду:

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    Путь к вашей *.dll* обычно *bin* папку в папке проекта.

    Эта команда создает файл с именем *server.js* в той же папке, что *signalr.exe*.
4. Поместите *server.js* в соответствующую папку в проекте, переименуйте его в зависимости от приложения и добавьте ссылку на него вместо ссылки на «/ концентраторов signalr».

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Как установить соединение

Чтобы установить подключение, необходимо создать объект подключения, создать учетную запись-посредник и регистрировать обработчики событий для методов, которые могут вызываться с сервера. При настройке прокси-сервера и обработчики событий, установить соединение, вызвав `start` метод.

Если вы используете созданный прокси, не нужно создать объект подключения в собственном коде, так как созданный код прокси делает это автоматически.

<a id="nogenconnection"></a>

**Установить подключение (созданный прокси-сервер)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

**Установить соединение (без созданный прокси-сервера)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

В примере кода используется значение по умолчанию «/ signalr» URL-адрес для подключения к службе SignalR. Сведения о том, как указать другой базовый URL-адрес, см. в разделе [ASP.NET руководство по API концентраторов SignalR - Server - URL-адрес /signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).

> [!NOTE]
> Обычно вы регистрировать обработчики событий перед вызовом `start` метод, чтобы установить соединение. Если вы хотите зарегистрировать некоторые обработчики событий после подключения к, это можно сделать, но необходимо зарегистрировать по крайней мере один из вашей обработчик или обработчики событий перед вызовом `start` метод. Одной из причин этого является может существовать множество центров в приложении, что вы не захотите активировать `OnConnected` событий на каждый центр, если только вы собираетесь использовать для одного из них. Когда подключение будет установлено, наличие клиентский метод на прокси-сервера концентратора является указывающий SignalR для активации `OnConnected` событий. Если не зарегистрировать все обработчики событий, перед вызовом `start` метод, можно вызывать методы на концентраторе, но центра `OnConnected` не будет вызван метод и методы клиента не будет вызываться с сервера.


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a>$. connection.hub совпадает с объектом этого $.hubConnection() создает

Как вы видите, в примерах, при использовании созданного прокси `$.connection.hub` ссылается на объект подключения. Это тот же объект, которую можно получить, вызвав `$.hubConnection()` , если не используется созданный прокси. Созданный код прокси создает подключение, выполнив следующую инструкцию:

![Создание подключения в создаваемого файла посредника](signalr-1x-hubs-api-guide-javascript-client/_static/image3.png)

Если вы используете созданный прокси-сервер, можно делать все с `$.connection.hub` , выполняемых с помощью объекта соединения при вы не используете созданный прокси.

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a>Асинхронное выполнение метода start

`start` Метод выполняется асинхронно. Он возвращает [объект jQuery отложенный](http://api.jquery.com/category/deferred-object/), что означает, что можно добавить функции обратного вызова, вызывая методы, такие как `pipe`, `done`, и `fail`. Если у вас есть код, который должен выполняться после установки подключения, например вызов метода сервера, помещать код в функцию обратного вызова, или вызывать из функции обратного вызова. `.done` Метод обратного вызова выполняется после установления соединения, и после любой код, при наличии вашей `OnConnected` завершении метода обработчика событий на сервере.

Если поместить оператор «Теперь подключен» из предыдущего примера в следующей строке кода после `start` вызова метода (не в `.done` обратного вызова), `console.log` строка будет иметь прежде, чем подключение будет установлено, как показано в следующем Пример:

![Неправильный способ написания кода, который выполняется после установки подключения](signalr-1x-hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a>Как установить подключение между доменами

Обычно если браузер загружает страницу из `http://contoso.com`, подключении SignalR находится в том же домене, в `http://contoso.com/signalr`. Если страницы от `http://contoso.com` подключается к `http://fabrikam.com/signalr`, то есть соединение между доменами. По соображениям безопасности подключений между доменами отключены по умолчанию. Для установления соединения между доменами, убедитесь, что междоменные соединения разрешены на сервере и укажите URL-АДРЕСЕ соединения при создании объекта подключения. SignalR подходящей технологии для подключения будет использоваться между доменами, такие как [JSONP](http://en.wikipedia.org/wiki/JSONP) или [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).

На сервере, включите соединения между доменами, выбрав соответствующий параметр, при вызове `MapHubs` метод.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample10.cs?highlight=2)]

На стороне клиента укажите URL-адрес при создании объекта подключения (без созданный прокси-сервера) или перед вызовом метода start (с помощью созданного прокси).

**Код клиента, который указывает соединение между доменами (с помощью созданного прокси-сервер)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample11.js?highlight=1)]

**Код клиента, который указывает соединение между доменами (без созданный прокси-сервера)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

При использовании `$.hubConnection` конструктор, не обязательно включать `signalr` в URL-АДРЕСЕ, поскольку она добавляется автоматически (если не указано `useDefaultUrl` как `false`).

Можно создать несколько подключений к другим конечным точкам.

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample13.js)]

> [!NOTE] 
> 
> - Не устанавливайте `jQuery.support.cors` значение true в коде.
> 
>     ![Не присвоено значение true jQuery.support.cors](signalr-1x-hubs-api-guide-javascript-client/_static/image7.png)
> 
>     SignalR обрабатывает использование JSONP или CORS. Параметр `jQuery.support.cors` в значение true отключает JSONP, так как вызывает SignalR предположить браузером CORS.
> - При подключении к URL-адрес localhost, Internet Explorer 10 не будет считать соединение между доменами, поэтому приложение будет работать локально с помощью Internet Explorer 10 даже если вы не включили междоменных подключений на сервере.
> - Сведения об использовании подключений между доменами с Internet Explorer 9, см. в разделе [цепочке обсуждений StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).
> - Сведения об использовании подключений между доменами с Chrome, см. в разделе [цепочке обсуждений StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).
> - В примере кода используется значение по умолчанию «/ signalr» URL-адрес для подключения к службе SignalR. Сведения о том, как указать другой базовый URL-адрес, см. в разделе [ASP.NET руководство по API концентраторов SignalR - Server - URL-адрес /signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Настройка подключения

Перед установкой соединения можно указать параметры строки запроса или указать метод транспорта.

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Как указать параметры строки запроса

Если вы хотите отправлять данные на сервер при подключении клиента, можно добавить параметры строки запроса на объект подключения. В следующих примерах для параметра строки запроса в клиентском коде.

**Задайте значения строки запроса перед вызовом метода start (с помощью созданного прокси-сервер)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample14.js?highlight=1)]

**Задайте значения строки запроса перед вызовом метода start (без созданный прокси-сервера)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample15.js?highlight=2)]

Приведенный ниже показано, как прочитать параметр строки запроса в серверном коде.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample16.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Как указать метод транспорта

В рамках процесса подключения клиента SignalR обычно согласовывает с сервером, чтобы определить лучший транспорт, который поддерживается с сервера и клиента. Если вы уже знаете, какой транспорт, который вы хотите использовать, вы можете обойти этот процесс согласования, указав метод транспорта, при вызове `start` метод.

**Код клиента, который задает метод передачи (с помощью созданного прокси-сервер)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

**Код клиента, который указывает метод транспорта (без созданный прокси-сервера)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

Кроме того можно указать несколько методов транспорта, в том порядке, в котором SignalR испытать их:

**Код клиента, который определяет пользовательский транспорт резервный схемы (с помощью созданного прокси-сервер)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample19.js?highlight=1)]

**Код клиента, который определяет резервный схемы пользовательского транспорта (без созданный прокси-сервера)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample20.js?highlight=2)]

Для указания метода транспортировки, можно использовать следующие значения:

- «webSockets»
- «foreverFrame»
- «serverSentEvents»
- «longPolling»

Как узнать, какой метод транспорта используется соединением, в следующих примерах.

**Код клиента, который отображает метод транспорта, используемый в соединении (с помощью созданного прокси-сервер)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample21.js?highlight=2)]

**Код клиента, который отображает метод транспорта, используемый в соединении (без созданный прокси-сервера)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample22.js?highlight=3)]

Сведения о том, как проверить метод транспорта в серверном коде, см. в разделе [ASP.NET руководство по API концентраторов SignalR - сервер — как для получения сведений о клиенте из контекстного свойства](../guide-to-the-api/hubs-api-guide-server.md#contextproperty). Дополнительные сведения о транспортах и в случае ошибки, см. в разделе [введение в SignalR - транспорта и в случае ошибки](../getting-started/introduction-to-signalr.md#transports).

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a>Как получить учетную запись-посредник для класса концентратора

Каждый объект соединения, который создается инкапсулирует сведения о подключении к службе SignalR, которая содержит один или несколько классов концентратора. Для взаимодействия с классом концентратора, использовать прокси-объект созданные пользователем (Если вы не используете созданный прокси-сервера) или которой будет создан автоматически.

На клиентском компьютере имя прокси-сервера — это версия стиле Camel, имени класса концентратора. SignalR автоматически делает это изменение, чтобы код JavaScript может соответствовать соглашениям JavaScript.

**Класс концентратора на сервере**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

**Получить ссылку на созданный прокси клиента для центра**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample24.js?highlight=1)]

**Создание прокси клиента для классу Hub (без созданный прокси-сервера)**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample25.cs?highlight=1)]

Если вы устанавливаете центр класса с `HubName` атрибута, используйте точное имя без Смена регистра.

**Класс концентратора на сервере с атрибутом HubName**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

**Получить ссылку на созданный прокси клиента для центра**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample27.js?highlight=1)]

**Создание прокси клиента для классу Hub (без созданный прокси-сервера)**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample28.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Как определить методы на клиенте, который сервер может обратиться

Чтобы определить метод, который сервер может обратиться с помощью центра, добавьте обработчик событий для прокси-сервера концентратора с помощью `client` свойства созданного прокси-сервера, или вызова `on` метод, если вы не используете созданный прокси. Параметры могут быть сложными объектами.

Добавьте обработчик событий, перед вызовом метода `start` метод, чтобы установить соединение. (Если вы хотите добавить обработчики событий после вызова метода `start` метод, см. в примечании [как для установления соединения](#establishconnection) выше в этом и используйте синтаксис, показанный для определения метода без использования созданного прокси.)

Совпадение имен метод не учитывает регистр. Например `Clients.All.addContosoChatMessageToPage` будет выполняться на сервере `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, или `addcontosochatmessagetopage` на стороне клиента.

**Определите метод на клиенте (с помощью созданного прокси-сервер)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample29.js?highlight=2)]

**Альтернативный способ определить метод для клиента (с помощью созданного прокси-сервер)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample30.js?highlight=1-2)]

**Определите метод на клиенте (без созданный прокси-сервера, или при добавлении после вызова метода start)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample31.js?highlight=3)]

**Код сервера, который вызывает метод клиента**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample32.cs?highlight=5)]

Следующие примеры включают сложный объект как параметр метода.

**Определите метод на клиенте, который принимает сложный объект (с помощью созданного прокси-сервер)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample33.js?highlight=2-3)]

**Определите метод на клиенте, который принимает сложный объект (без созданный прокси-сервера)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample34.js?highlight=3-4)]

**Код сервера, который определяет сложный объект**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample35.cs?highlight=1)]

**Код сервера, который вызывает метод клиента, используя сложный объект**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample36.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Как вызывать методы сервера от клиента

Для вызова серверного метода от клиента, используйте `server` свойства созданного прокси-сервера или `invoke` метод на прокси-концентратора, если вы не используете созданный прокси. Возвращаемое значение или параметры могут быть сложными объектами.

Передать в нижнем регистре версии имя метода концентратора. SignalR автоматически делает это изменение, чтобы код JavaScript может соответствовать соглашениям JavaScript.

В следующих примерах показано, как вызвать метод сервера, который не имеет возвращаемого значения и способ вызова метода сервера, который имеет возвращаемое значение.

**Метод сервера без атрибута HubMethodName**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample37.cs?highlight=3)]

**Код сервера, который определяет сложный объект, переданный в параметре**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample38.cs)]

**Клиентский код, который вызывает метод сервера (с помощью созданного прокси-сервер)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample39.js?highlight=1)]

**Клиентский код, который вызывает метод сервера (без созданный прокси-сервера)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

Если декорировать метод концентратора с `HubMethodName` атрибута, используйте его без Смена регистра.

**Метод сервера** с атрибутом HubMethodName

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample41.cs?highlight=3)]

**Клиентский код, который вызывает метод сервера (с помощью созданного прокси-сервер)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample42.js?highlight=1)]

**Клиентский код, который вызывает метод сервера (без созданный прокси-сервера)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample43.js?highlight=1)]

Приведенные выше примеры показывают, как вызвать метод сервера, который не возвращает никакого значения. Следующие примеры показывают, как вызвать метод сервера, который имеет возвращаемое значение.

**Серверный код для метода, который имеет возвращаемое значение**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample44.cs?highlight=3)]

**Класс Stock, используемый для** возвращаемое значение

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample45.cs?highlight=1)]

**Клиентский код, который вызывает метод сервера (с помощью созданного прокси-сервер)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample46.js?highlight=2,4-5)]

**Клиентский код, который вызывает метод сервера (без созданный прокси-сервера)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample47.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Способ обработки событий времени существования подключений

SignalR обеспечивает следующее подключение события времени жизни, которые можно обработать:

- `starting`: Возникает перед любые данные, отправляются через соединение.
- `received`: Вызывается, когда все данные получаются через соединение. Предоставляет полученных данных.
- `connectionSlow`: Вызывается, когда клиент обнаруживает подключение медленно или часто удаление.
- `reconnecting`: Вызывается, когда используемому транспорту начинает повторное подключение.
- `reconnected`: Вызывается, когда была повторно присоединена используемому транспорту.
- `stateChanged`: Возникает при изменении состояния подключения. Предоставляет состояние старое и новое состояние (подключение, подключено, повторное подключение или Disconnected).
- `disconnected`: Вызывается, когда произойдет отключение соединения.

Например, если вы хотите отображать предупреждающие сообщения, при наличии проблем подключения, которые могут привести к значительным задержкам, обрабатывать `connectionSlow` событий.

**Обработать событие connectionSlow (с помощью созданного прокси-сервер)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample48.js?highlight=1)]

**Обработать событие connectionSlow (без созданный прокси-сервера)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample49.js?highlight=2)]

Дополнительные сведения см. в разделе [понимание и обработка событий времени существования подключений в SignalR](index.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Способ обработки ошибок

Клиент SignalR JavaScript предоставляет `error` , которые можно добавить обработчик для события. Метод fail также можно добавить обработчик для ошибки, возникающие в результате вызова метода сервера.

Если подробные сообщения об ошибках на сервере не включен явно, объект исключения, которое возвращает SignalR после возникновения ошибки содержит минимум информации об ошибке. Например, если вызов `newContosoChatMessage` завершается ошибкой, содержит сообщение об ошибке в объекте ошибки "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Отправка подробных сообщений об ошибках клиентов в рабочей среде не рекомендуется по соображениям безопасности, но если вы хотите включить подробные сообщения об ошибках для устранения неполадок, используйте следующий код на сервере.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample50.cs?highlight=2)]

В следующем примере показано, как добавить обработчик для события ошибки.

**Добавить обработчик ошибок (с помощью созданного прокси-сервер)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample51.js?highlight=1)]

**Добавить обработчик ошибок (без созданный прокси-сервера)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

В следующем примере показано, как обработать ошибку из вызова метода.

**Обработать ошибку из вызова метода (с помощью созданного прокси-сервер)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample53.js?highlight=2)]

**Обработать ошибку из вызова метода (без созданный прокси-сервера)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

В случае сбоя вызова метода `error` также события, поэтому в коде `error` метод обработчика и в `.fail` выполняется метод обратного вызова.

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Как включить ведение журнала на стороне клиента

Чтобы включить ведение журнала на стороне клиента для подключения, задайте `logging` свойство в объекте подключения, перед вызовом метода `start` метод, чтобы установить соединение.

**Включить ведение журнала (с помощью созданного прокси-сервер)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample55.js?highlight=1)]

**Включение ведения журнала (без созданный прокси-сервера)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample56.js?highlight=2)]

Чтобы просмотреть журналы, откройте средства разработчика браузера и перейдите на вкладку консоли. Учебник, в котором показаны пошаговые инструкции и экрана снимков, которые показывают, как это сделать, см. в разделе [передача сообщений с сервера с помощью Signalr - включить ведение журнала](index.md).
