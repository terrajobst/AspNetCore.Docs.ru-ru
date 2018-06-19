---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: Поддержка BSON в ASP.NET Web API 2.1 | Документы Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 53ad705fad6d2225cecca4d73355bd6ebfcf56d5
ms.sourcegitcommit: 459cb3289741a3f46325e605a617dc926ee0563d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/22/2018
ms.locfileid: "27984587"
---
<a name="bson-support-in-aspnet-web-api-21"></a>Поддержка BSON в ASP.NET Web API 2.1
====================
по [Mike Wasson](https://github.com/MikeWasson)

Веб-API 2.1 появилась поддержка BSON. В этом разделе показано, как использовать BSON в контроллере веб-API (на стороне сервера) или в клиентском приложении .NET.

## <a name="what-is-bson"></a>Что такое BSON?

[BSON](http://bsonspec.org/) — это формат двоичной сериализации. «BSON» означает «Двоичный JSON», но очень по-разному сериализация BSON и JSON. BSON — «JSON по принципу», так как объекты в виде пар имя значение, аналогично JSON. В отличие от JSON числовые типы данных хранятся в виде байтов, а не строк

BSON была разработана для упрощенная, легко просматривать и быстро для шифрования или расшифровки.

- BSON сопоставим по размер в JSON. В зависимости от данных BSON полезных данных может быть меньше или больше полезные данные JSON. Для сериализации двоичных данных, таких как файл изображения BSON меньше, чем JSON, так как двоичные данные не кодировке base64.
- Документы BSON являются удобной для быстрого просмотра, так как элементы начинаются с префикса длины поля, поэтому средство синтаксического анализа можно пропустить элементы без их декодирования.
- Эффективны, так как числовые типы данных хранятся в виде числа, строки не кодирования и декодирования.

Собственных клиентов, например клиентские приложения .NET, можно использовать преимущества BSON вместо текстовые форматы, такие как JSON или XML. Для обозревателя клиента, скорее всего вы заходите с JSON, поскольку JavaScript может напрямую обрабатывать полезные данные JSON.

К счастью, использует веб-API [содержимого согласования](content-negotiation.md), поэтому API может поддерживать оба формата и позволяют выбрать клиента.

## <a name="enabling-bson-on-the-server"></a>Включение BSON на сервере

Добавьте в конфигурацию веб-API **BsonMediaTypeFormatter** в коллекцию модулей форматирования.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

Теперь если клиент запрашивает «application/bson», веб-API будет использовать модуль форматирования BSON.

Чтобы связать BSON с другими типами мультимедиа, добавьте их в коллекцию SupportedMediaTypes. Следующий код добавляет «application/vnd.contoso» поддерживаемые типы носителей:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a>Пример сеанса HTTP

В этом примере мы будем использовать следующие классы модели, а также простой контроллер веб-API:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

Клиент может отправить следующий запрос HTTP:

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

Ниже представлен ответ:

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

Здесь я заменить двоичные данные с &quot;.&quot; символов. На следующем снимке экрана из Fiddler показаны необработанные шестнадцатеричные значения.

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a>Использование BSON с HttpClient

Приложения .NET клиенты могут использовать форматирования BSON **HttpClient**. Дополнительные сведения о **HttpClient**, в разделе [вызов Web API из клиента .NET](../advanced/calling-a-web-api-from-a-net-client.md).

Следующий код отправляет запрос GET, которая принимает BSON и затем выполняет десериализацию полезных данных BSON в ответе.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

Чтобы запросить BSON с сервера, задайте заголовок Accept для «application/bson»:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

Чтобы десериализовать текст ответа, используйте **BsonMediaTypeFormatter**. Этот модуль форматирования не в коллекцию модулей форматирования по умолчанию, поэтому необходимо указать его при чтении текста ответа:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

В следующем примере показано, как отправить запрос POST, содержащий BSON.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

Большая часть кода совпадает со значением предыдущего примера. Однако в **PostAsync** метод, укажите **BsonMediaTypeFormatter** как модуль форматирования:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a>Сериализация верхнего уровня типы-примитивы

Каждый документ BSON приведен список пар «ключ значение». Спецификация BSON определяет синтаксис для сериализации одного необработанное значение, например целое число или строка.

Чтобы обойти это ограничение, **BsonMediaTypeFormatter** рассматривает как особый случай типов-примитивов. До сериализации, он преобразует значение в виде пары ключ значение с ключом «Значение». Например предположим, что контроллер API возвращает целое число:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

Перед началом сериализации, BSON форматирования преобразует этот следующая пара ключ значение:

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

При попытке десериализации модуль форматирования преобразует данные обратно в исходное значение. Тем не менее клиенты, использующие разные средства синтаксического анализа BSON потребуется этот случай, если веб-API Возвращает необработанные значения. В общем случае следует возврат структурированных данных, а не необработанных значений.

## <a name="additional-resources"></a>Дополнительные ресурсы

[Веб-API BSON пример](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[Модули форматирования мультимедиа](media-formatters.md)
