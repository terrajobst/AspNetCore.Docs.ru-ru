---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: "JSON и XML-сериализации в ASP.NET Web API | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2012
ms.topic: article
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: 7aafe4823d3a6090fae4a63f1a66fb2670ecb025
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a>JSON и XML-сериализации в веб-API ASP.NET
====================
по [Mike Wasson](https://github.com/MikeWasson)

В этой статье описывается форматирования данных JSON и XML в ASP.NET Web API.

В ASP.NET Web API *форматирования типа мультимедиа* — это объект, можно:

- Тело сообщения объектов среды CLR для чтения из HTTP
- Запись объектов среды CLR в основной текст сообщения HTTP

Веб-API предоставляет форматирования типа мультимедиа для JSON и XML. Платформа вставляет эти модули форматирования в конвейер по умолчанию. Клиенты могут запросить JSON или XML в заголовке Accept HTTP-запроса.

## <a name="contents"></a>Описание

- [Модуль форматирования типа мультимедиа JSON](#json_media_type_formatter)

    - [Свойства только для чтения](#json_readonly)
    - [Даты](#json_dates)
    - [Отступов](#json_indenting)
    - [Верблюжий](#json_camelcasing)
    - [Анонимные и слабо типизированных объектов](#json_anon)
- [Модуль форматирования типа мультимедиа XML](#xml_media_type_formatter)

    - [Свойства только для чтения](#xml_readonly)
    - [Даты](#xml_dates)
    - [Отступов](#xml_indenting)
    - [Сериализаторы XML для типа параметра](#xml_pertype)
- [Удалить модуль форматирования XML или JSON](#removing_the_json_or_xml_formatter)
- [Обработка циклических ссылок на объекты](#handling_circular_object_references)
- [Тестирование сериализации объектов](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a>Модуль форматирования типа мультимедиа JSON

Форматирование обеспечивается **JsonMediaTypeFormatter** класса. По умолчанию **JsonMediaTypeFormatter** использует [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) библиотеки для выполнения сериализации. Json.NET — это проект открытым исходным кодом сторонних разработчиков.

При желании можно настроить **JsonMediaTypeFormatter** класс, используемый **DataContractJsonSerializer** вместо Json.NET. Чтобы сделать это, установите **UseDataContractJsonSerializer** свойства **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>Сериализация JSON

В этом разделе описываются некоторые конкретные режимы форматирования JSON, используя значение по умолчанию [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) сериализатор. Это не должен быть подробную документацию библиотеки Json.NET; Дополнительные сведения см. в разделе [Json.NET документации](http://james.newtonking.com/projects/json/help/).

#### <a name="what-gets-serialized"></a>Что сериализуется?

По умолчанию все открытые свойства и поля включаются в сериализованной JSON. Чтобы исключить свойство или поле, декорировать его **JsonIgnore** атрибута.

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

Если вы предпочитаете &quot;участие в&quot; подход, добавьте класс с **DataContract** атрибута. Если этот атрибут присутствует, элементы игнорируются, если они не имеют **DataMember**. Можно также использовать **DataMember** для сериализации закрытых членов.

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>Свойства только для чтения

Свойства только для чтения, сериализуются по умолчанию.

<a id="json_dates"></a>
### <a name="dates"></a>даты

По умолчанию Json.NET записывает даты [ISO 8601](http://www.w3.org/TR/NOTE-datetime) формат. Даты в формате UTC (время по Гринвичу) записываются с суффиксом «Z». Даты в формате местного времени включают смещение часового пояса. Пример:

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

По умолчанию Json.NET сохраняет часовой пояс. Это можно переопределить, задав свойство DateTimeZoneHandling:

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

Если вы предпочитаете использовать [формат даты Microsoft JSON](https://msdn.microsoft.com/en-us/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) вместо ISO 8601, задать **DateFormatHandling** свойство параметры сериализатора:

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a>Отступы

Чтобы записать с отступом JSON, задайте **форматирование** параметру **Formatting.Indented**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a>Верблюжий

Чтобы записать имена свойств JSON с верблюжий, без изменения модели данных, задайте **CamelCasePropertyNamesContractResolver** на сериализатор:

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a>Анонимные и слабо типизированных объектов

Метод действия может возвращать анонимный объект и выполнить его сериализацию в JSON. Пример:

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

Текст сообщения ответа будет содержать следующий JSON:

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

Если веб-API получает слабо структурированных объектов JSON от клиентов, может выполнить десериализацию текста запроса для **Newtonsoft.Json.Linq.JObject** типа.

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

Тем не менее обычно лучше использовать строго типизированных объектов данных. Затем не нужно самостоятельно выполнять анализ данных, и воспользоваться преимуществами проверки модели.

XML-сериализатор не поддерживает анонимные типы или **JObject** экземпляров. Если вы используете эти функции для данных JSON, следует удалить модуль форматирования XML из конвейера, как описано далее в этой статье.

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>Модуль форматирования типа мультимедиа XML

Форматирование XML-кода предоставляется **XmlMediaTypeFormatter** класса. По умолчанию **XmlMediaTypeFormatter** использует **DataContractSerializer** классом для выполнения сериализации.

При желании можно настроить **XmlMediaTypeFormatter** использовать **XmlSerializer** вместо **DataContractSerializer**. Чтобы сделать это, установите **UseXmlSerializer** свойства **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

**XmlSerializer** класс поддерживает узкий набор типов по сравнению **DataContractSerializer**, но дает больший контроль над результирующего XML. Рассмотрите возможность использования **XmlSerializer** если должен соответствовать существующей схемы XML.

### <a name="xml-serialization"></a>XML-сериализация

В этом разделе описываются некоторые конкретные режимы модуль форматирования XML с помощью стандартных **DataContractSerializer**.

По умолчанию DataContractSerializer работает следующим образом:

- Все свойства чтения/записи и поля сериализуются. Чтобы исключить свойство или поле, декорировать его **IgnoreDataMember** атрибута.
- Закрытые и защищенные члены не сериализуются.
- Свойства только для чтения не сериализуются. (Тем не менее, содержимое свойства только для чтения коллекция сериализуются.)
- Имена классов и членов записываются в формате XML, так же как и в объявлении класса.
- Пространство имен XML по умолчанию используется.

Если требуется больший контроль над сериализации, вы можете декорировать класс с **DataContract** атрибута. Если этот атрибут присутствует, сериализации класса следующим образом:

- &quot;Обеспечение поддержки&quot; подход: свойства и поля по умолчанию не сериализуются. Для сериализации свойства или поля, декорировать его **DataMember** атрибута.
- Для сериализации закрытому или защищенному члену, декорировать его **DataMember** атрибута.
- Свойства только для чтения не сериализуются.
- Чтобы изменить отображение имени класса в XML, задайте *имя* параметр в **DataContract** атрибута.
- Чтобы изменить способ отображения имени члена в XML, установите *имя* параметр в **DataMember** атрибута.
- Чтобы изменить пространство имен XML, задайте *имен* параметр в **DataContract** класса.

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>Свойства только для чтения

Свойства только для чтения не сериализуются. Если свойство только для чтения имеет закрытый резервное поле, можно пометить закрытое поле с **DataMember** атрибута. Этот подход требует **DataContract** атрибут в классе.

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>даты

Даты, записываются в формате ISO 8601. Например &quot;2012-05-23T20:21:37.9116538Z&quot;.

<a id="xml_indenting"></a>
### <a name="indenting"></a>Отступы

Чтобы записать XML с отступами, задайте **отступ** свойства **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>Сериализаторы XML для типа параметра

Можно задать разные сериализаторы XML для различных типов среды CLR. Например, может потребоваться определенный объект, который требуется **XmlSerializer** для обеспечения обратной совместимости. Можно использовать **XmlSerializer** для данного объекта и продолжать использовать **DataContractSerializer** для других типов.

Чтобы задать XML-сериализатор для определенного типа, вызовите **SetSerializer**.

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

Можно указать **XmlSerializer** или любой объект, который является производным от **XmlObjectSerializer**.

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>Удалить модуль форматирования XML или JSON

Модуль форматирования JSON или XML-форматирования можно удалить из списка модулей форматирования, если вы не хотите использовать их. Основных причин для этого являются:

- Чтобы ограничить ответы web API для определенного типа носителя. Например можно поддерживать только ответов JSON, а удалить модуль форматирования XML.
- Чтобы заменить модуль форматирования по умолчанию пользовательский модуль форматирования. Например можно заменить модуль форматирования JSON с собственную реализацию модуля форматирования JSON.

Ниже показано, как удалить модули форматирования по умолчанию. Вызывать его из вашей **приложения\_запустить** метода, определенного в файле Global.asax.

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>Обработка циклических ссылок на объекты

По умолчанию модули форматирования JSON и XML записать все объекты как значения. Если два свойства ссылаются на один объект или объект того же появляется дважды в коллекцию, модуль форматирования будет сериализовать объект дважды. Это определенной проблемы если граф объекта содержит циклы, поскольку сериализатор вызовет исключение при обнаружении цикл в графе.

Рассмотрим следующие модели объектов и контроллера.

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

Вызвать это действие приведет к модулем форматирования, возникает исключение, которое может достигать ответа кода состояния 500 (Внутренняя ошибка сервера) для клиента.

Для сохранения ссылок на объекты в JSON, добавьте следующий код в **приложения\_запустить** метод в файле Global.asax:

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

Теперь действие контроллера возвращает JSON, который выглядит следующим образом:

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

Обратите внимание, что сериализатор добавляет &quot;$id&quot; свойство для обоих объектов. Кроме того, он обнаруживает, свойство Employee.Department создает цикл, поэтому он заменяет значение ссылки на объект: {&quot;$ref&quot;:&quot;1&quot;}.

> [!NOTE]
> Ссылки на объекты не являются стандартными в формате JSON. Прежде чем использовать эту функцию, учитывайте ли ваши клиенты смогут проанализировать результаты. Возможно, лучше просто удалить циклов с диаграммы. Например связи с из сотрудников отдела не требуется действительно в данном примере.


Для сохранения ссылок на объекты в XML, имеются две возможности. Параметр проще — добавление `[DataContract(IsReference=true)]` класс модели. *IsReference* параметр включает ссылки на объект. Следует помнить, что **DataContract** делает сериализации включить в программу, поэтому необходимо также добавить **DataMember** атрибуты к свойствам:

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

Теперь средство форматирования создает XML, подобный следующее:

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

Вы хотите избежать атрибуты в классе модели, существует ли еще один вариант: Создание нового определенного типа **DataContractSerializer** экземпляр и задайте *ними* для **значение true**  в конструкторе. Затем установите как сериализатор для типа этого экземпляра на модуль форматирования XML тип носителя. Ниже показано, как это сделать:

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>Тестирование сериализации объектов

При разработке веб-API, полезно для тестирования сериализацией объектов данных. Это можно сделать без создания контроллера или вызов действия контроллера.

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
