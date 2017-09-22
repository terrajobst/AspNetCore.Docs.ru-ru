---
title: "Пользовательские модули форматирования в ASP.NET Core MVC веб-API"
author: tdykstra
description: "Узнайте, как создавать и использовать пользовательские модули форматирования для веб-API в ASP.NET Core."
keywords: "ASP.NET Core веб-api, пользовательские модули форматирования"
ms.author: tdykstra
manager: wpickett
ms.date: 02/08/2017
ms.topic: article
ms.assetid: 1fb6fdc2-e199-4469-9012-b909d1913422
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/custom-formatters
ms.openlocfilehash: 5e665abe10fd7444c3fd5f20cfeca3ef0a5f79d3
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/22/2017
---
# <a name="custom-formatters-in-aspnet-core-mvc-web-apis"></a>Пользовательские модули форматирования в ASP.NET Core MVC веб-API

По [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core MVC имеет встроенную поддержку для обмена данными в веб-API с помощью формата JSON, XML или обычного текста. В этой статье показано, как добавить поддержку дополнительных форматов, создав пользовательские модули форматирования.

[Просмотреть или загрузить пример из GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample).

## <a name="when-to-use-custom-formatters"></a>Когда следует использовать пользовательские модули форматирования

Использовать пользовательский модуль форматирования [содержимого согласования](xref:mvc/models/formatting) процесса для поддержки типа содержимого, которое не поддерживается в встроенные модули форматирования (JSON, XML и обычный текст).

Например, если некоторые из клиентов для веб-API может обрабатывать [Protobuf](https://github.com/google/protobuf) формат, может потребоваться использовать Protobuf этими клиентами, поскольку он является более эффективным.  Либо можно использовать веб-API для отправки имена и адреса в [vCard](https://wikipedia.org/wiki/VCard) формат, широко распространенный формат для обмена контактных данных. Пример приложения, описываемых в этой статье реализует простой vCard модуль форматирования.

## <a name="overview-of-how-to-use-a-custom-formatter"></a>Общие сведения о том, как использовать пользовательский модуль форматирования

Ниже приведены шаги, чтобы создать и использовать пользовательский модуль форматирования.

* Создайте класс модуля форматирования выходных данных, если необходимо сериализовать данные для отправки клиенту.
* Создание класса модуль форматирования входных данных, если вы хотите выполнить десериализацию данных, полученных от клиента. 
* Добавить экземпляры к модули форматирования `InputFormatters` и `OutputFormatters` коллекций в [MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).

В следующих разделах руководства и примеры кода для каждого из этих действий.

## <a name="how-to-create-a-custom-formatter-class"></a>Как создать класс пользовательский модуль форматирования

Чтобы создать модуль форматирования:

* Создайте класс, производный от соответствующего базового класса.
* Укажите допустимый носитель типов и кодировок в конструкторе.
* Переопределить `CanReadType` / `CanWriteType` методы
* Переопределить `ReadRequestBodyAsync` / `WriteResponseBodyAsync` методы
  
### <a name="derive-from-the-appropriate-base-class"></a>Является производным от соответствующего базового класса

Для текстовых типов носителей (например, vCard), являются производными от [TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) или [TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) базового класса.

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

Двоичные типы являются производными от [InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) или [OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter) базового класса.

### <a name="specify-valid-media-types-and-encodings"></a>Укажите допустимый носитель типы и кодировки

В конструкторе, укажите допустимый носитель типов и кодировок, добавив `SupportedMediaTypes` и `SupportedEncodings` коллекции.

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

> [!NOTE]  
> Внедрение зависимостей конструктор в класс модуля форматирования не поддерживается. Например средство ведения журнала не удается получить, добавив параметр ведения журнала в конструктор. Для доступа к службам, необходимо использовать объект context, переданного в методы. Пример кода [ниже](#read-write) показано, как это сделать.

### <a name="override-canreadtypecanwritetype"></a>Переопределить CanReadType/CanWriteType 

Укажите тип можно десериализовать в или сериализации из путем переопределения `CanReadType` или `CanWriteType` методы. Например, можно только будет создание vCard текст из `Contact` и наоборот.

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

#### <a name="the-canwriteresult-method"></a>Метод CanWriteResult

В некоторых сценариях необходимо переопределить `CanWriteResult` вместо `CanWriteType`. Используйте `CanWriteResult` , если выполняются следующие условия:

  * Метод действия возвращает класс модели.
  * Существуют производные классы, которые могут возвращаться во время выполнения.
  * Необходимо знать во время выполнения, который производным классом, возвращаемого действием.  

Предположим, что подпись метода действие возвращает `Person` типа, но он может возвращать `Student` или `Instructor` тип, производный от `Person`. Если требуется, чтобы модуль форматирования для обработки только `Student` объектов, проверьте тип [объекта](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) в контекст объект, предоставляемый для `CanWriteResult` метод. Обратите внимание, что нет необходимости использовать `CanWriteResult` при возвращении операции `IActionResult`; в этом случае `CanWriteType` метод получает тип среды выполнения.

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a>Переопределить ReadRequestBodyAsync/WriteResponseBodyAsync 

Выполняют реальную работу десериализации или сериализации в `ReadRequestBodyAsync` или `WriteResponseBodyAsync`.  Выделенные строки в следующем примере показано, как получить службы из контейнера внедрения зависимостей (их не удается получить из параметров конструктора).

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a>Настройка MVC, чтобы использовать пользовательский модуль форматирования
 
Чтобы использовать пользовательский модуль форматирования, добавьте экземпляр класса модуля форматирования для `InputFormatters` или `OutputFormatters` коллекции.

[!code-csharp[Main](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

Модули форматирования, вычисляются в порядке, в каком они добавлены. Первый из них имеет приоритет. 

## <a name="next-steps"></a>Следующие шаги

В разделе [образец приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample), который реализует простой vCard входных данных и модули форматирования выходных данных.  Приложение считывает и записывает визитные карточки, которые выглядят как в следующем примере:

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

Чтобы увидеть vCard выходных данных, запустите приложение и отправка запроса Get с Accept заголовок «текст/vcard» для `http://localhost:63313/api/contacts/` (при запуске из Visual Studio) или `http://localhost:5000/api/contacts/` (при запуске из командной строки).

Чтобы добавить ее в памяти коллекцию контактов, отправьте запрос Post в URL, с заголовка Content-Type «text/vcard» и текст vCard в тексте, отформатированных как в приведенном выше примере.
