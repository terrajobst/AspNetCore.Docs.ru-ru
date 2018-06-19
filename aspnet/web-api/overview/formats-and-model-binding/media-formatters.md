---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Модули форматирования мультимедиа в ASP.NET Web API 2 | Документы Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 1cb1c7e0f832a0a0160276fbd41facc017e2ae3e
ms.sourcegitcommit: 50d40c83fa641d283c097f986dde5341ebe1b44c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/22/2018
ms.locfileid: "34452604"
---
<a name="media-formatters-in-aspnet-web-api-2"></a>Модули форматирования мультимедиа в ASP.NET Web API 2
====================
по [Mike Wasson](https://github.com/MikeWasson)

Этот учебник демонстрирует поддерживают дополнительные мультимедийные форматы в веб-API ASP.NET.

## <a name="internet-media-types"></a>Типы носителей Интернета

Тип носителя, также называемый MIME-тип, определяет формат фрагмента данных. В HTTP типы носителей описания формата текста сообщения. Тип носителя состоит из двух строк, тип и подтип. Пример:

- text/html
- изображение или png
- application/json

Если сообщение HTTP содержит тело сущности, заголовка Content-Type указывает формат текста сообщения. Узнает получатель синтаксического анализа содержимого тела сообщения.

Например если HTTP-ответа содержит изображения в формате PNG, ответ могут возникнуть следующие заголовки.

[!code-console[Main](media-formatters/samples/sample1.cmd)]

Когда клиент отправляет сообщение-запрос, может включать заголовок Accept. Заголовок Accept сообщает, что сервер мультимедиа типы клиент хочет с сервера. Пример:

[!code-console[Main](media-formatters/samples/sample2.cmd)]

Этот заголовок указывает, что сервер, что клиенту требуется HTML, XHTML или XML.

Тип носителя определяет, как веб-API сериализует и десериализует тело сообщения HTTP. Веб-API имеет встроенную поддержку XML, JSON, BSON и формы urlencoded данных и может поддерживать дополнительные носители, написав *форматирования мультимедиа*.

Чтобы создать модуль форматирования мультимедиа, являются производными от одного из этих классов:

- [MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx). Этот класс использует асинхронное чтение и запись методы.
- [BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx). Этот класс является производным от **MediaTypeFormatter** , но использует sychronous методы чтения и записи.

Наследование от **BufferedMediaTypeFormatter** проще, так как нет асинхронного кода, но это также означает, что вызывающий поток можно заблокировать во время ввода-вывода.

## <a name="example-creating-a-csv-media-formatter"></a>Пример: Создание модуль форматирования мультимедиа CSV

В следующем примере показано форматирования типа мультимедиа, который может сериализовать объект продукта в формате с разделителями-запятыми (CSV). В этом примере используется тип продукта, определенные в этом учебнике [Создание веб-API, поддерживает операции CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md). Вот определение объекта продукта.

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

Чтобы реализовать модуль форматирования CSV, определите класс, производный от **BufferedMediaTypeFormater**:

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

В конструкторе добавьте типами мультимедиа, которые поддерживает модуль форматирования. В этом примере модуль форматирования поддерживает устройств одного типа &quot;текст/csv&quot;:

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

Переопределить **CanWriteType** можно сериализовать метод, чтобы указать, какие типы форматирования:

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

В этом примере можно сериализовать один модуль форматирования `Product` объектов и коллекций `Product` объектов.

Аналогичным образом переопределить **CanReadType** метод, чтобы указать, какие типы форматирования может десериализовать. В этом примере модуль форматирования не поддерживает десериализации, поэтому метод просто возвращает **false**.

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

Наконец, переопределения **WriteToStream** метод. Этот метод сериализует тип путем записи в поток. Если модуль форматирования поддерживает десериализации, также переопределить **ReadFromStream** метод.

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a>Добавление форматирования мультимедиа в конвейер веб-API

Чтобы добавить тип мультимедиа модуля форматирования в конвейер веб-API, используйте **модули форматирования** свойство **HttpConfiguration** объекта.

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a>Кодировки символов

При необходимости форматирования мультимедиа может поддерживать несколько кодировок, например UTF-8 или ISO 8859-1.

В конструкторе, добавьте один или несколько [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) типы **SupportedEncodings** коллекции. Поместите первый кодировку по умолчанию.

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

В **WriteToStream** и **ReadFromStream** вызывать методы, [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) для выбора предпочтительной кодировки. Этот метод совпадает со списком поддерживаемых кодировок заголовки запроса. Используйте возвращенный **кодировка** при чтении или записи из потока:

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
