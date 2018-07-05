---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Вызов веб-API из клиента .NET (C#) | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/24/2017
ms.topic: article
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 434515692a53c0939652b643b080cea9f2102564
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370922"
---
<a name="call-a-web-api-from-a-net-client-c"></a>Вызов веб-API из клиента .NET (C#)
====================
по [Майк Уоссон](https://github.com/MikeWasson) и [Рик Андерсон](https://twitter.com/RickAndMSFT)

[Скачать завершенный проект](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample). [Указания по скачиванию](/aspnet/core/tutorials/#how-to-download-a-sample). 

Этом руководстве показано, как вызывать веб-API из приложения .NET с помощью [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)

В этом руководстве клиентского приложения записываются, которые используют следующие веб-API:

| Действие | Метод HTTP | Относительный URI |
| --- | --- | --- |
| Получить продукт по Идентификатору | GET | /API/продукты/*идентификатор* |
| Создать продукт | ПОМЕСТИТЬ | / api/продуктов |
| Обновления продукта | PUT | /API/продукты/*идентификатор* |
| Удалить продукт | DELETE | /API/продукты/*идентификатор* |

Чтобы узнать, как реализовать этот интерфейс API с веб-API ASP.NET, см. в разделе [Создание веб-API, поддерживает операции CRUD](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).

Для простоты клиентское приложение в этом руководстве используется консольное приложение Windows. **HttpClient** также поддерживается для приложений Windows Phone и Windows Store. Дополнительные сведения см. в разделе [код записи веб-API клиента для нескольких платформ с помощью переносимых библиотек](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a>Создание консольного приложения

В Visual Studio создайте новое консольное приложение Windows с именем **HttpClientSample** и вставьте в него следующий код:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

Приведенный выше код является полный клиентским приложением.

`RunAsync` запуски и блокируется до его завершения. Большинство **HttpClient** методы являются async, поскольку они выполняют подсистемы ввода/вывода. Все асинхронные задачи выполняемые в `RunAsync`. Обычно приложения не блокирует основной поток, но это приложение не допускает каких-либо взаимодействий.

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a>Установка библиотеки Web API клиента

С помощью диспетчера пакетов NuGet для установки пакета Web API клиентских библиотек.

В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**. В консоли диспетчера пакетов (PMC), введите следующую команду:

`Install-Package Microsoft.AspNet.WebApi.Client`

Предыдущая команда добавляет в проект следующие пакеты NuGet:

* Microsoft.AspNet.WebApi.Client
* Newtonsoft.Json

Json.NET — это популярная платформа JSON высокой производительности для .NET.

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a>Добавление класса модели

Изучите `Product` класса:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

Этот класс соответствует модели данных, используемых веб-API. Приложение может использовать **HttpClient** для чтения `Product` экземпляр HTTP-ответа. Приложение не нужно писать код десериализации.

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a>Создание и инициализация HttpClient

Изучите статический **HttpClient** свойство:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

**HttpClient** будет должен создаваться один раз и повторно использоваться на протяжении всего жизненного цикла приложения. Можно привести следующие условия **SocketException** ошибок:

* Создание нового **HttpClient** экземпляра на запрос.
* Сервер в условиях большой нагрузки.

Создание нового **HttpClient** экземпляра на запрос может исчерпать все доступные сокеты.

В следующем примере кода инициализирует **HttpClient** экземпляр:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

Предыдущий код:

* Задает базовый URI для HTTP-запросов. Измените номер порта к порту, используемому в серверное приложение. Если не используется порт для серверного приложения для работы приложения.
* Задает заголовок Accept «application/json». Параметр этот заголовок указывает, что сервер для отправки данных в формате JSON.

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a>Отправка запроса GET для извлечения ресурса

Следующий код отправляет запрос GET для продукта:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

**GetAsync** метод отправляет запрос HTTP GET. Если метод завершается, она возвращает **HttpResponseMessage** , содержащий HTTP-ответа. Если код состояния в ответе код успешного выполнения, текст ответа содержит представление JSON продукта. Вызовите **ReadAsAsync** для полезных данных JSON для десериализации `Product` экземпляра. **ReadAsAsync** метод является асинхронным, поскольку текст ответа может быть произвольно большим.

**HttpClient** не выдает исключение при HTTP-ответа содержит код ошибки. Вместо этого **IsSuccessStatusCode** свойство **false** Если состояние — код ошибки. Если вы предпочитаете обрабатывать коды ошибок HTTP как исключения, вызвать [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) объекта response. `EnsureSuccessStatusCode` создает исключение, если код состояния находится вне диапазона 200&ndash;299. Обратите внимание, что **HttpClient** может создавать исключения по другим причинам &mdash; к примеру, если тайм-аута запроса.

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a>Модули форматирования типа мультимедиа для десериализации

Когда **ReadAsAsync** вызывается без параметров, он использует набор по умолчанию *модули форматирования мультимедиа* прочитать текст ответа. Модули форматирования по умолчанию поддерживает JSON, XML и данные в форме кодировке к URL-адрес.

Вместо того чтобы использовать модули форматирования по умолчанию, можно предоставить список модулей форматирования для **ReadAsAsync** метод.  Использование список модулей форматирования полезно в том случае, если у вас есть модуль форматирования типа мультимедиа.

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

Дополнительные сведения см. в разделе [модули форматирования мультимедиа в ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)

## <a name="sending-a-post-request-to-create-a-resource"></a>Отправить запрос POST для создания ресурса

Следующий код отправляет запрос POST, содержащий `Product` экземпляра в формате JSON:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

**PostAsJsonAsync** метод:

* Сериализует объект в формат JSON.
* Отправляет полезные данные JSON в запросе POST.

Если запрос выполнен успешно:

* Он должен вернуть ответ 201 (создано).
* Ответ должен содержать URL-адрес созданные ресурсы в заголовке Location.

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a>Отправка запроса PUT для обновления ресурса

Следующий код отправляет запрос PUT для обновления продукта:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

**PutAsJsonAsync** метод работает подобно **PostAsJsonAsync**, за исключением того, что он отправляет запрос PUT вместо POST.

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a>Отправляя запрос DELETE для удаления ресурса

Следующий код отправляет запрос DELETE для удаления продукта:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

Например GET запрос DELETE не имеет текста запроса. Не нужно указать формат JSON или XML с помощью удаления.

## <a name="test-the-sample"></a>Тестирование образца

Чтобы проверить его:

1. [Скачайте](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) и запуск приложения сервера. [Указания по скачиванию](/aspnet/core/tutorials/#how-to-download-a-sample). Убедитесь, что работает серверное приложение. Для exaxmple `http://localhost:64195/api/products` должен возвращать список продуктов.
2. Задайте базовый URI для HTTP-запросов. Измените номер порта к порту, используемому в серверное приложение.
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. Запустите клиентское приложение. Выводятся следующие результаты.

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
