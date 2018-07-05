---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: 'Отправка данных формы HTML в веб-API ASP.NET: Отправка файлов и составное сообщение MIME | Документация Майкрософт'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: 7c1c85f462141daf747e23aa4215d47f2d263140
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386712"
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a>Отправка данных формы HTML в веб-API ASP.NET: Отправка файлов и составное сообщение MIME
====================
по [Майк Уоссон](https://github.com/MikeWasson)

## <a name="part-2-file-upload-and-multipart-mime"></a>Часть 2: Отправка файлов и составное сообщение MIME

Этом руководстве показано, как для передачи файлов в веб-API. Также описывается обработка составных данных MIME.

> [!NOTE]
> [Скачивание готового проекта](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).


Ниже приведен пример HTML-форму для передачи файла:

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

Эта форма содержит элемент управления для ввода текста и элемент управления ввода файла. Когда форма содержит элемент управления ввода файла **enctype** атрибут всегда должен иметь &quot;multipart/данные формы&quot;, которое указывает, что формы будут отправляться как составное сообщение MIME.

Формат составного сообщения MIME будет легче понять, взглянув на пример запроса:

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

Это сообщение состоит из двух *частей*, один для каждого элемента управления. Часть границы обозначены строки, начинающиеся с дефисами.

> [!NOTE]
> Часть границ включает компонент случайных (&quot;41184676334&quot;) чтобы убедиться, что строка границ не отображается случайно внутри части сообщения.


Каждая часть сообщения содержит один или несколько заголовков, следуют содержимого части.

- Заголовок Content-Disposition содержит имя элемента управления. Для файлов он также содержит имя файла.
- Заголовок Content-Type описывает данные в части. Если этот заголовок указан, значение по умолчанию — text/plain.

В предыдущем примере пользователь отправил файл с именем GrandCanyon.jpg, с типом содержимого image/jpeg; значение текстового ввода &quot;отпуск&quot;.

## <a name="file-upload"></a>Отправка файла

Теперь давайте взглянем на контроллер веб-API, который считывает файлы из составного сообщения MIME. Контроллер будет считывать файлы асинхронно. Веб-API поддерживает асинхронные операции, с помощью [модель программирования на основе задач](https://msdn.microsoft.com/library/dd460693.aspx). Во-первых, вот код, если вы ориентируетесь на .NET Framework 4.5, которая поддерживает **async** и **await** ключевые слова.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

Обратите внимание на то, что действие контроллера не принимает никаких параметров. Том, что мы можем обработать текст запроса внутри действия без вызова форматирования типа мультимедиа.

**IsMultipartContent** метод проверяет, содержит ли запрос составное сообщение MIME. В противном случае контроллер возвращает код состояния HTTP 415 (неподдерживаемый тип носителя).

**MultipartFormDataStreamProvider** класс — это вспомогательный объект, выделяет файловых потоков для отправленных файлов. Чтобы прочитать составного сообщения MIME, вызовите **ReadAsMultipartAsync** метод. Этот метод извлекает все части сообщения и записывает их в потоков, предоставляемых **MultipartFormDataStreamProvider**.

По завершении выполнения метода можно получить сведения о файлах из **FileData** свойство, которое является коллекцией из **MultipartFileData** объектов.

- **MultipartFileData.FileName** — это имя локального файла на сервере, где был сохранен файл.
- **MultipartFileData.Headers** содержит часть заголовка (*не* заголовка запроса). Это можно использовать для доступа к содержимому\_заголовки Disposition и Content-Type.

Как и предполагает имя, **ReadAsMultipartAsync** — это асинхронный метод. Для выполнения работы, после завершения работы метода, используйте [задача продолжения](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) или **await** ключевое слово (.NET 4.5).

Вот версия .NET Framework 4.0 предыдущего примера кода:

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a>Чтение данных элемента управления формы

HTML-форма, я показал ранее было ввода текста.

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

Можно получить значение элемента управления из **FormData** свойство **MultipartFormDataStreamProvider**.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

**FormData** — **NameValueCollection** , содержащий пары имя/значение для элементов управления формы. Коллекция может содержать повторяющиеся ключи. Рассмотрим следующую форму:

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

Текст запроса может выглядеть следующим образом:

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

В этом случае **FormData** коллекции будет содержать следующие пары "ключ значение":

- поездки: приема-передачи
- параметры: nonstop
- параметры: даты
- рабочее место: окно
