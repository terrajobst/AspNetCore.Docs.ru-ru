---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Вызов веб-API из клиента .NET (C#) | Документы Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/24/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: a243eeb982ba581e237263c4e31e130d634aff0e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="call-a-web-api-from-a-net-client-c"></a>Вызов веб-API из клиента .NET (C#)
====================
по [Mike Wasson](https://github.com/MikeWasson) и [Рик Андерсон](https://twitter.com/RickAndMSFT)

[Загрузка завершенного проекта](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample)

Этого учебника показано, как вызывать из приложения .NET, веб-API с помощью [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)

В этом учебнике клиентском приложении записываются, которые используют следующие веб-API:

| Действие | Метод HTTP | Относительный URI |
| --- | --- | --- |
| Получение продукта по Идентификатору | GET | /api/products/*id* |
| Создать продукт | ПОМЕСТИТЬ | / api/продуктов |
| Обновления продукта | PUT | /api/products/*id* |
| Удаление продукта | DELETE | /api/products/*id* |

Чтобы узнать, как реализовать этот интерфейс API с веб-API ASP.NET, в разделе [Создание веб-API, поддерживает операции CRUD](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).

Для простоты клиентское приложение в этом учебнике используется консольное приложение Windows. **HttpClient** также поддерживается для приложений Windows Phone и магазина Windows. Дополнительные сведения см. в разделе [код записи веб-API клиента для нескольких платформ с помощью переносимой библиотеки](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a>Создайте консольное приложение

В Visual Studio создайте новое консольное приложение Windows с именем **HttpClientSample** и вставьте следующий код:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

Приведенный выше код является полный клиентского приложения.

`RunAsync` Запуск и блокируется до ее завершения. Большинство **HttpClient** методы являются async, поскольку они выполняют сетевых операций ввода-вывода. Все асинхронные задачи выполняемые в `RunAsync`. Обычно приложения не блокирует основной поток, но это приложение не допускает каких-либо действий.

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a>Установите клиентские API библиотеки Web

Используйте диспетчер пакетов NuGet для установки пакета Web API Client Libraries.

В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**. В пакет Диспетчер консоли (PMC), введите следующую команду:

`Install-Package Microsoft.AspNet.WebApi.Client`

Эта команда добавляет следующие пакеты NuGet в проект:

* Microsoft.AspNet.WebApi.Client
* Newtonsoft.Json

Json.NET — это популярный платформа JSON высокой производительности для .NET.

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a>Добавьте класс модели

Изучите `Product` класса:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

Этот класс соответствует модели данных, используемых веб-API. Можно использовать приложение **HttpClient** для чтения `Product` экземпляр HTTP-ответа. Приложения не нужно писать никакой код для десериализации.

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a>Создание и инициализация HttpClient

Проверки статических **HttpClient** свойства:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

**HttpClient** предназначен для создания экземпляров один раз и использовать повторно в течение жизненного цикла приложения. Следующие причины могут приводить к **SocketException** ошибки:

* Создание нового **HttpClient** экземпляра на запрос.
* Сервер максимально загружен.

Создание нового **HttpClient** экземпляра на запрос может использовать доступные сокеты.

Следующий код инициализирует **HttpClient** экземпляр:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

Предыдущий код:

* Задает базовый URI для HTTP-запросов. Измените номер порта к порту, используемому в приложении сервера. Приложение не будет работать, если не используется порт для серверного приложения.
* Задает заголовок Accept для «application/json». Установка этот заголовок указывает, что сервер для отправки данных в формате JSON.

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a>Отправка запроса GET для извлечения ресурса

Следующий код отправляет запрос GET для продукта:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

**GetAsync** метод отправляет запрос HTTP GET. Если метод завершается, она возвращает **HttpResponseMessage** , содержащий HTTP-ответа. Если код состояния в ответе код успешного завершения, текст ответа содержит представление JSON продукта. Вызовите **ReadAsAsync** для полезных данных JSON для десериализации `Product` экземпляра. **ReadAsAsync** метод является асинхронным, поскольку текст ответа может быть произвольно большое.

**HttpClient** не вызывает исключение, если HTTP-ответ содержит код ошибки. Вместо этого **IsSuccessStatusCode** свойство **false** Если состояние представляет собой код ошибки. Если вы предпочитаете обрабатывать коды ошибок HTTP как исключения, вызовите [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) в объекте отклика. `EnsureSuccessStatusCode` вызывает исключение, если код состояния находится вне диапазона 200&ndash;299. Обратите внимание, что **HttpClient** может создавать исключения по другим причинам &mdash; к примеру, при истечении времени ожидания запроса.

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a>Модули форматирования типа мультимедиа для десериализации

Когда **ReadAsAsync** вызывается без параметров, она использует набор по умолчанию *модули форматирования мультимедиа* для чтения текста ответа. Модули форматирования по умолчанию поддерживает JSON, XML и данные в кодировке форма URL-адрес.

Вместо использования модулей форматирования по умолчанию, необходимо предоставить список модулей форматирования для **ReadAsAsync** метод.  Список модулей форматирования удобно использовать при наличии пользовательский модуль форматирования мультимедиа:

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

Дополнительные сведения см. в разделе [модулей форматирования мультимедиа в ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)

## <a name="sending-a-post-request-to-create-a-resource"></a>Отправка запроса POST для создания ресурса

Следующий код отправляет запрос POST, содержащий `Product` экземпляра в формате JSON:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

**PostAsJsonAsync** метод:

* Сериализует объект в JSON.
* Отправляет полезные данные JSON в запросе POST.

Если запрос выполняется успешно:

* Она должна возвращать ответа 201 (создано).
* Ответ должен содержать созданные ресурсы URL-адрес в заголовке Location.

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a>Отправка запроса PUT для обновления ресурса

Следующий код отправляет запрос PUT для обновления продукта:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

**PutAsJsonAsync** действия метода **PostAsJsonAsync**, за исключением того, что он отправляет запрос PUT вместо POST.

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a>Отправка запроса DELETE, чтобы удалить ресурс

Следующий код отправляет запрос DELETE для удаления продукта:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

Как GET запрос DELETE не имеет текста запроса. Не нужно указать формат JSON или XML с "Удалить".

## <a name="test-the-sample"></a>Тестирование образца

Для проверки клиентского приложения:

1. [Загрузить](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) и запустить приложение сервера. [Указания по скачиванию](https://docs.microsoft.com/aspnet/core/tutorials/#how-to-download-a-sample). Убедитесь, что работа приложения сервера. Для exaxmple `http://localhost:64195/api/products` должен возвращать список продуктов.
2. Задайте базовый универсальный код Ресурса для HTTP-запросов. Измените номер порта к порту, используемому в приложении сервера.
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. Запустите клиентское приложение. Выводятся следующие результаты.

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
