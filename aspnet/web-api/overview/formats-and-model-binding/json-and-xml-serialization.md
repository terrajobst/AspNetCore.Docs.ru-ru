---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: Сериализация JSON и XML в веб-API ASP.NET | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2012
ms.topic: article
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: e7fbcd41d64651255763c7629f0232788dcb3d30
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400925"
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a>Сериализация JSON и XML в веб-API ASP.NET
====================
по [Майк Уоссон](https://github.com/MikeWasson)

В этой статье описывается модули форматирования JSON и XML в ASP.NET Web API.

В веб-API ASP.NET *форматирования типа мультимедиа* — это объект, который может:

- Тело сообщения объектов среды CLR для чтения из HTTP
- Запись объектов среды CLR в основную часть сообщения HTTP

Веб-API предоставляет модулей форматирования типа мультимедиа для JSON и XML. Платформа вставляет эти модули форматирования в конвейер по умолчанию. Клиенты могут запрашивать JSON или XML в заголовке Accept HTTP-запроса.

## <a name="contents"></a>Описание

- [Модуль форматирования типа мультимедиа JSON](#json_media_type_formatter)

    - [Свойства только для чтения](#json_readonly)
    - [Даты](#json_dates)
    - [Отступов](#json_indenting)
    - [Смешанный регистр знаков](#json_camelcasing)
    - [Анонимные и слабо типизированных объектов](#json_anon)
- [Модуль форматирования типа мультимедиа XML](#xml_media_type_formatter)

    - [Свойства только для чтения](#xml_readonly)
    - [Даты](#xml_dates)
    - [Отступов](#xml_indenting)
    - [Сериализаторы XML для типа параметра](#xml_pertype)
- [Модуль форматирования XML или JSON](#removing_the_json_or_xml_formatter)
- [Обработка циклические ссылки объекта](#handling_circular_object_references)
- [Тестирование сериализация объектов](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a>Модуль форматирования типа мультимедиа JSON

Форматирование обеспечивается **JsonMediaTypeFormatter** класса. По умолчанию **JsonMediaTypeFormatter** использует [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) библиотеку для выполнения сериализации. Json.NET — это проект с открытым кодом независимых производителей.

При желании можно настроить **JsonMediaTypeFormatter** класс для использования **DataContractJsonSerializer** вместо Json.NET. Чтобы сделать это, установите **UseDataContractJsonSerializer** свойства **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>Сериализация JSON

В этом разделе описываются некоторые конкретные режимы модуль форматирования JSON, используя значение по умолчанию [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) сериализатор. Это не должно быть полную документацию по библиотеке Json.NET. Дополнительные сведения см. в разделе [документации по Json.NET](http://james.newtonking.com/projects/json/help/).

#### <a name="what-gets-serialized"></a>Что был сериализован?

По умолчанию все открытые свойства и поля включаются в сериализованном формате JSON. Чтобы опустить свойство или поле, декорировать его **JsonIgnore** атрибута.

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

Если вы предпочитаете &quot;согласиться&quot; подход, установите для класса **DataContract** атрибута. Если этот атрибут присутствует, элементы учитываются, если они не имеют **DataMember**. Можно также использовать **DataMember** для сериализации закрытых членов.

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>Свойства только для чтения

Только для чтения свойства сериализуются по умолчанию.

<a id="json_dates"></a>
### <a name="dates"></a>Даты

По умолчанию Json.NET записывает даты [ISO 8601](http://www.w3.org/TR/NOTE-datetime) формат. Даты в формате UTC (время по Гринвичу) записываются с суффиксом «Z». Даты в формате местного времени включают смещение часового пояса. Пример:

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

По умолчанию Json.NET сохраняет часовой пояс. Это можно переопределить, задав свойство DateTimeZoneHandling:

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

Если вы предпочитаете использовать [формат даты Microsoft JSON](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) вместо ISO 8601, задать **DateFormatHandling** свойство параметры сериализатора:

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a>Отступы

Чтобы записать с отступом JSON, задайте **форматирование** присвоить **Formatting.Indented**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a>Смешанный регистр знаков

Чтобы записать имена свойств JSON с смешанный регистр знаков, без изменения модели данных, задайте **CamelCasePropertyNamesContractResolver** на сериализатора:

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a>Анонимные и слабо типизированных объектов

Метод действия может вернуть анонимный объект и выполнить его сериализацию в JSON. Пример:

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

Текст ответа будет содержать следующий JSON:

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

Если веб-API получает слабо структурированных объектов JSON от клиентов, может выполнить десериализацию текста запроса для **Newtonsoft.Json.Linq.JObject** типа.

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

Тем не менее обычно лучше использовать строго типизированных объектов данных. Затем нужно проанализировать данные самостоятельно, и вы получаете преимущества проверки модели.

XML-сериализатор не поддерживает анонимные типы или **JObject** экземпляров. Если вы используете эти функции для данных JSON, следует удалить модуль форматирования XML из конвейера, как описано далее в этой статье.

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>Модуль форматирования типа мультимедиа XML

Форматирование XML-кода обеспечивается **XmlMediaTypeFormatter** класса. По умолчанию **XmlMediaTypeFormatter** использует **DataContractSerializer** классом для выполнения сериализации.

При желании можно настроить **XmlMediaTypeFormatter** использовать **XmlSerializer** вместо **DataContractSerializer**. Чтобы сделать это, установите **UseXmlSerializer** свойства **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

**XmlSerializer** класс поддерживает более узкий набор типов, чем **DataContractSerializer**, но дает больший контроль над получаемый код XML. Рассмотрите возможность использования **XmlSerializer** Если вам необходимо соответствовать существующей схемы XML.

### <a name="xml-serialization"></a>XML-сериализация

В этом разделе описываются некоторые конкретные поведения данного модуля форматирования XML, используя значение по умолчанию **DataContractSerializer**.

По умолчанию DataContractSerializer ведет себя следующим образом:

- Сериализуются все свойства чтения/записи и поля. Чтобы опустить свойство или поле, декорировать его **IgnoreDataMember** атрибута.
- Закрытые и защищенные члены не сериализуются.
- Свойства только для чтения не сериализуются. (Тем не менее, содержимое свойства только для чтения коллекции сериализуются.)
- Имена классов и членов записываются в формате XML, так же, как в объявлении класса.
- Пространство имен XML по умолчанию используется.

Если вам требуется больший контроль над сериализацией, вы можете декорировать класс с **DataContract** атрибута. Если этот атрибут присутствует, класс сериализуется следующим образом:

- &quot;Согласиться&quot; подход: поля и свойства не сериализуются по умолчанию. Чтобы сериализовать свойство или поле, декорировать его **DataMember** атрибута.
- Чтобы сериализовать закрытому или защищенному члену, декорировать его **DataMember** атрибута.
- Свойства только для чтения не сериализуются.
- Чтобы изменить, как отображается имя класса в XML, установите *имя* параметр в **DataContract** атрибута.
- Чтобы изменить, как имя члена отображается в XML, установите *имя* параметр в **DataMember** атрибута.
- Чтобы изменить пространство имен XML, установите *пространства имен* параметр в **DataContract** класса.

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>Свойства только для чтения

Свойства только для чтения не сериализуются. Если свойство только для чтения имеет закрытое резервное поле, можно пометить закрытое поле с **DataMember** атрибута. Этот подход требует **DataContract** атрибут класса.

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>Даты

Даты записываются в формате ISO 8601. Например &quot;2012-05-23T20:21:37.9116538Z&quot;.

<a id="xml_indenting"></a>
### <a name="indenting"></a>Отступы

Чтобы записать Отступами, задайте **отступ** свойства **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>Сериализаторы XML для типа параметра

Можно задать разные сериализаторы XML для различных типов среды CLR. Например, возможно, объекта данных, который требует **XmlSerializer** для обеспечения обратной совместимости. Можно использовать **XmlSerializer** для данного объекта и продолжать использовать **DataContractSerializer** для других типов.

Чтобы задать XML-сериализатор для определенного типа, вызовите **SetSerializer**.

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

Можно указать **XmlSerializer** или любой объект, который является производным от **XmlObjectSerializer**.

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>Модуль форматирования XML или JSON

Модуль форматирования JSON или модуль форматирования XML можно удалить из списка модулей форматирования, если вы не хотите их использовать. Ниже приведены основные причины этого.

- Чтобы ограничить ваши ответы веб-API с определенным типом носителя. Например можно на поддержку только для ответов JSON и удалить модуль форматирования XML.
- Чтобы заменить модуль форматирования по умолчанию пользовательского модуля форматирования. Например можно заменить модуль форматирования JSON с собственную реализацию модуля форматирования JSON.

Ниже показано, как удалить модули форматирования по умолчанию. Вызывать его из вашей **приложения\_запустить** метод, определенный в файле Global.asax.

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>Обработка циклические ссылки объекта

По умолчанию модули форматирования JSON и XML запись всех объектов в качестве значения. Если два свойства относятся к одному объекту, или тот же объект в коллекцию дважды, модуль форматирования будет сериализовать объект дважды. Это обусловлено конкретной проблемы, если объект графа содержит циклы, сериализатор приведет к возникновению исключения при обнаружении цикл в графе.

Рассмотрим следующие объектные модели и контроллера.

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

Вызов это действие приводит к модулю форматирования генерации исключения, который преобразуется ответа кода состояния 500 (Внутренняя ошибка сервера) для клиента.

Для сохранения ссылок на объекты в формате JSON, добавьте следующий код, чтобы **приложения\_запустить** метод в файле Global.asax:

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

Теперь действие контроллера возвращает JSON, который выглядит следующим образом:

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

Обратите внимание, что сериализатор добавляет &quot;$id&quot; свойства обоих объектов. Кроме того, он обнаруживает, что свойство Employee.Department создает цикл, поэтому он заменяет значение ссылки на объект: {&quot;$ref&quot;:&quot;1&quot;}.

> [!NOTE]
> Ссылки на объекты не являются стандартными в формате JSON. Прежде чем использовать эту функцию, учитывать ли ваши клиенты смогут проанализировать результаты. Возможно, лучше просто удалить циклов с диаграммы. Например в ссылке из сотрудников отдела в этом примере требуется не действительно.


Для сохранения ссылок на объекты в формате XML, можно двумя способами. Более простой способ — добавить `[DataContract(IsReference=true)]` к классу модели. *IsReference* параметр включает ссылки на объекты. Помните, что **DataContract** делает сериализации согласиться, поэтому также необходимо будет добавить **DataMember** атрибуты к свойствам:

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

Теперь модуль форматирования создает код XML, аналогичный следующее:

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

Если вы хотите избежать атрибуты в классе модели, есть еще один вариант: создайте новый конкретного типа **DataContractSerializer** экземпляр и задайте *פכאדמל* для **true**  в конструкторе. Установите этот экземпляр как сериализатор для типа модуля форматирования типа мультимедиа XML. Следующий код показывает, как это сделать:

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>Тестирование сериализация объектов

При разработке веб-API, полезно проверить, как будет сериализовать объекты данных. Это можно сделать без создания контроллера или вызов действия контроллера.

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
