---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: Поддержка BSON в веб-API 2.1 ASP.NET | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 7c5e763d92295a83b3431e9ec6e305b07f8a64d2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370498"
---
<a name="bson-support-in-aspnet-web-api-21"></a>Поддержка BSON в веб-API 2.1 ASP.NET
====================
по [Майк Уоссон](https://github.com/MikeWasson)

Веб-API 2.1 введена поддержка BSON. В этом разделе показано, как использовать BSON в контроллере веб-API (на стороне сервера) и в клиентском приложении .NET.

## <a name="what-is-bson"></a>Что такое BSON?

[BSON](http://bsonspec.org/) — это формат двоичной сериализации. «BSON» означает «Двоичный JSON», но BSON и JSON сериализуются совсем по-другому. BSON — «Похожих на JSON-», так как объекты отображаются в виде пары имя значение, аналогичную JSON. В отличие от JSON числовых типов данных, хранятся в виде байтов, не строки

BSON была разработана как упрощенная, легко просматривать и быстро для шифрования или расшифровки.

- BSON можно сравнить размер, в формат JSON. В зависимости от данных полезные данные BSON могут занять меньше или больше, чем полезные данные JSON. Для сериализации двоичных данных, таких как файл изображения, BSON меньше, чем JSON, так как двоичные данные не кодировке base64.
- BSON документы являются удобной для быстрого просмотра, так как элементы должны иметь префикс длины поля, средство синтаксического анализа можно пропустить элементы без их декодирование.
- Эффективны, так как числовые типы данных хранятся в виде числа, строки не кодирования и декодирования.

Собственные клиенты, такие как клиентские приложения .NET, можно использовать преимущества BSON вместо текстовых форматов, например JSON или XML. Для обозревателя клиента возможно, необходимо отказаться от использования JSON, так как JavaScript, напрямую, можно преобразовать в полезных данных JSON.

К счастью, веб-API использует [согласования содержимого](content-negotiation.md), поэтому можно поддерживать оба формата и дать клиенту выбрать API.

## <a name="enabling-bson-on-the-server"></a>Включение BSON на сервере

Добавьте в конфигурацию веб-API, **BsonMediaTypeFormatter** в коллекцию модулей форматирования.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

Теперь если клиент запрашивает «application/bson», веб-API будет использовать модуль форматирования BSON.

Чтобы связать BSON с другими типами мультимедиа, их необходимо добавьте в коллекцию SupportedMediaTypes. Следующий код добавляет «application/vnd.contoso» поддерживаемые типы носителей:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a>Пример HTTP-сеанса

В этом примере мы будем использовать класс модели, а также простой контроллер веб-API:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

Клиент может отправить следующий запрос HTTP:

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

Ниже представлен ответ:

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

Здесь я заменил двоичные данные с &quot;.&quot; символов. На следующем снимке экрана из Fiddler показаны необработанные шестнадцатеричные значения.

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a>С помощью BSON с HttpClient

Приложения клиенты .NET могут использовать модуль форматирования BSON **HttpClient**. Дополнительные сведения о **HttpClient**, см. в разделе [вызова Web API из клиента .NET](../advanced/calling-a-web-api-from-a-net-client.md).

Следующий код отправляет запрос GET, который принимает BSON и затем десериализует полезные данные BSON в ответе.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

Чтобы запросить BSON с сервера, задайте заголовок Accept для «application/bson»:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

Чтобы десериализовать текст ответа, используйте **BsonMediaTypeFormatter**. Этот модуль форматирования не в коллекции модулей форматирования по умолчанию, поэтому вам нужно будет указать его при чтении текста ответа:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

Далее примере показано, как отправлять запрос POST, содержащий BSON.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

Большая часть этого кода совпадает со значением предыдущего примера. Однако в **PostAsync** метод, укажите **BsonMediaTypeFormatter** как модуль форматирования:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a>Сериализация верхнего уровня типов-примитивов

Каждый документ BSON приведен список пар "ключ значение". Спецификация BSON определяет синтаксис для сериализации необработанные единственное значение, например целое число или строка.

Чтобы обойти это ограничение, **BsonMediaTypeFormatter** рассматривает типы-примитивы как особый случай. Перед сериализацией, он преобразует значение в паре ключ/значение с ключом «Value». Например предположим, что контроллер API возвращает целое число:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

Перед сериализацией, модуль форматирования BSON преобразует это следующую пару "ключ значение":

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

При десериализации, модуль форматирования преобразует данные обратно в исходное значение. Тем не менее клиенты, использующие разные средства синтаксического анализа BSON потребуется в этом случае веб-API возвращает исходные значения. В общем случае следует рассмотрите возможность возврата структурированных данных, а не исходные значения.

## <a name="additional-resources"></a>Дополнительные ресурсы

[Веб-API BSON пример](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[Модули форматирования мультимедиа](media-formatters.md)
