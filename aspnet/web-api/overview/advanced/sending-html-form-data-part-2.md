---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: "Отправка данных формы HTML в веб-API ASP.NET: файл передачи и составного MIME | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: 331d0e520a1fd8ec84aecd09a9c9e6d286c5893b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a>Отправка данных формы HTML в веб-API ASP.NET: файл передачи и составного MIME
====================
по [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-2-file-upload-and-multipart-mime"></a>Часть 2: Отправка файла и составного MIME

Этот учебник показывает, как для передачи файлов в веб-API. Также описывается обработка составных данных MIME.

> [!NOTE]
> [Загрузка завершенного проекта](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).


Ниже приведен пример формы HTML для передачи файла:

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

Эта форма содержит управления для ввода текста и элемент управления входного файла. Когда форма содержит элемент управления ввода файла **enctype** атрибут всегда должен иметь &quot;multipart/формы data&quot;, которое указывает, что формы будут отправляться как составного сообщения MIME.

Формат составного сообщения MIME можно легко понять, просмотрев пример запроса:

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

Это сообщение состоит из двух *частей*, один для каждого элемента управления. Часть границы обозначаются строки, начинающиеся с тире.

> [!NOTE]
> Граница часть включает в себя случайных компонент (&quot;41184676334&quot;) для того что строка границ не отображалось случайно внутри части сообщения.


Каждая часть сообщения содержит один или несколько заголовков, следуют содержимое части.

- Заголовок Content-Disposition включает в себя имя элемента управления. Для файлов он также содержит имя файла.
- Заголовок Content-Type описывает данные в части. Если этот заголовок указан, значение по умолчанию — text/plain.

В предыдущем примере пользователь загрузить файл с именем GrandCanyon.jpg, с типом содержимого image/jpeg; значение текстового ввода &quot;отпуск&quot;.

## <a name="file-upload"></a>Отправка файла

Теперь давайте взглянем на контроллер веб-API, который считывает файлы из составного сообщения MIME. Контроллер будет читать файлы асинхронно. Веб-API поддерживает асинхронные операции с помощью [модель программирования на основе задач](https://msdn.microsoft.com/library/dd460693.aspx). Во-первых, вот код, если вы используете .NET Framework 4.5, который поддерживает **async** и **await** ключевые слова.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

Обратите внимание, что действие контроллера не принимает никаких параметров. Это, так как мы можем обработать тело запроса внутри действия, без вызова форматирования типа мультимедиа.

**IsMultipartContent** метод проверяет, содержит ли запрос составного сообщения MIME. В противном случае возвращает код состояния HTTP 415 (неподдерживаемый тип носителя).

**MultipartFormDataStreamProvider** класс представляет вспомогательный объект, который выделяет для отправляемых файлов файлового потока. Для считывания составного сообщения MIME следует вызвать **ReadAsMultipartAsync** метод. Этот метод извлекает все части сообщения и записывает их в потоков, предоставляемых **MultipartFormDataStreamProvider**.

При завершении метода, можно получить сведения о файлах из **FileData** свойство, которое представляет собой коллекцию из **MultipartFileData** объектов.

- **MultipartFileData.FileName** — это имя локального файла на сервере, где был сохранен файл.
- **MultipartFileData.Headers** содержит часть заголовка (*не* в заголовке запроса). Это можно использовать для доступа к содержимому\_заголовки расстановки и Content-Type.

Как видно из имени, **ReadAsMultipartAsync** является асинхронным методом. Для выполнения работы, после завершения работы метода, используйте [задача продолжения](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) или **await** ключевое слово (.NET 4.5).

Ниже приведена версия .NET Framework 4.0 приведенного выше кода:

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a>Чтение данных элемента управления формы

HTML-формы, показанного выше было управления для ввода текста.

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

Можно получить значение из элемента управления **FormData** свойство **MultipartFormDataStreamProvider**.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

**FormData** — **NameValueCollection** , содержащий пары имя значение для элементов управления формы. Коллекция может содержать повторяющиеся ключи. Рассмотрим следующую форму:

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

Текст запроса может выглядеть следующим образом:

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

В этом случае **FormData** коллекция будет содержать следующие пары ключ значение:

- маршрут: приема-передачи
- параметры: nonstop
- параметры: дат
- рабочее место: окна
