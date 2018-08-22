---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Модули форматирования мультимедиа в ASP.NET Web API 2 | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 7b7ba2fb3f1bba0447e700c84a017266cba305e6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829853"
---
<a name="media-formatters-in-aspnet-web-api-2"></a>Модули форматирования мультимедиа в ASP.NET Web API 2
====================
по [Майк Уоссон](https://github.com/MikeWasson)

Этом руководстве показано, как обеспечить поддержку дополнительных носителей форматов в веб-API ASP.NET.

## <a name="internet-media-types"></a>Типов мультимедиа для Интернета

Тип носителя, также называемый MIME-тип, определяет формат фрагмента данных. В HTTP типы мультимедиа описывают формат тела сообщения. Тип носителя состоит из двух строк, тип и подтип. Пример:

- text/html
- изображение/png
- application/json

Если сообщение HTTP содержит тело сущности, заголовка Content-Type формат тела сообщения. Это говорит получателю, как анализировать содержимое тела сообщения.

Например если ответ HTTP содержит изображения в формате PNG, ответ может содержать следующие заголовки.

[!code-console[Main](media-formatters/samples/sample1.cmd)]

Когда клиент отправляет сообщение-запрос, он может включать заголовок Accept. Заголовок Accept говорит, что сервер мультимедиа типы клиент хочет с сервера. Пример:

[!code-console[Main](media-formatters/samples/sample2.cmd)]

Этот заголовок указывает, что сервер, необходимый клиенту HTML, XHTML или XML.

Тип носителя определяет, как веб-API сериализует и десериализует текст сообщения HTTP. Веб-API имеет встроенную поддержку XML, JSON, BSON и формы urlencoded данных и может поддерживать дополнительные носители, написав *форматирования мультимедиа*.

Чтобы создать модуль форматирования мультимедиа, являются производными от одного из этих классов:

- [MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx). Этот класс использует асинхронное чтение и запись методы.
- [BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx). Этот класс является производным от **MediaTypeFormatter** , но использует sychronous методы чтения и записи.

Наследование от **BufferedMediaTypeFormatter** проще использовать, так как нет асинхронного кода, но это также значит, можно блокировать вызывающий поток ввода-вывода.

## <a name="example-creating-a-csv-media-formatter"></a>Пример: Создание CSV-ФАЙЛ форматирования мультимедиа

В следующем примере показано форматирования типа мультимедиа, который может сериализовать объект Product в формат с разделителями-запятыми (CSV). В этом примере используется тип продукта, определенный в этом руководстве [Создание веб-API, поддерживает операции CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md). Вот определение объекта продукта:

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

Чтобы реализовать модуль форматирования CSV, определите класс, который является производным от **BufferedMediaTypeFormater**:

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

Добавьте в конструктор, типы объектов, которые поддерживает модуль форматирования. В этом примере модуль форматирования поддерживает устройств одного типа, &quot;text/csv&quot;:

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

Переопределить **CanWriteType** метод, чтобы указать, какие типы форматирования можно сериализовать:

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

В этом примере модуль форматирования может сериализовать единый `Product` объекты, а также коллекции `Product` объектов.

Аналогичным образом переопределить **методов CanReadType** метод, чтобы указать, какие типы модуль форматирования может десериализовать. В этом примере модуль форматирования не поддерживает десериализацию, поэтому метод просто возвращает **false**.

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

Наконец, переопределение **WriteToStream** метод. Этот метод сериализует тип с помощью записи в поток. Если модуль форматирования поддерживает десериализацию, также переопределить **ReadFromStream** метод.

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a>Добавление форматирования мультимедиа в конвейер веб-API

Чтобы добавить тип мультимедиа модуля форматирования в конвейер веб-API, используйте **модули форматирования** свойство **HttpConfiguration** объекта.

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a>Кодировки символов

Кроме того модуль форматирования мультимедиа может поддерживать несколько кодировок, например UTF-8 или ISO-8859-1.

В конструкторе, добавьте один или несколько [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) типов **SupportedEncodings** коллекции. Поместите первый кодировку по умолчанию.

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

В **WriteToStream** и **ReadFromStream** вызывать методы, [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) выберите предпочитаемую кодировку символов. Этот метод соответствует заголовки запроса со списком поддерживаемых кодировок. Используйте возвращенный **кодировка** при чтении или записи из потока:

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
