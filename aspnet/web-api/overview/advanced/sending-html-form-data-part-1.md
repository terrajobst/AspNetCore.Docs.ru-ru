---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'Отправка данных формы HTML в веб-API ASP.NET: данные формы urlencoded | Документация Майкрософт'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/15/2012
ms.topic: article
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 24410c92df828d4aaaa3b91dd3e9fa14575fd300
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399874"
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a>Отправка данных формы HTML в веб-API ASP.NET: формы urlencoded данных
====================
по [Майк Уоссон](https://github.com/MikeWasson)

## <a name="part-1-form-urlencoded-data"></a>Часть 1: Данные формы urlencoded

В этой статье показано, как для отправки данных формы urlencoded контроллер Web API.

- [Общие сведения о HTML-формы](#overview_of_html_forms)
- [Отправка сложных типов](#sending_complex_types)
- [Отправка данных формы с помощью AJAX](#sending_form_data_via_ajax)
- [Отправка простых типов](#sending_simple_types)

> [!NOTE]
> [Скачивание готового проекта](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a>Общие сведения о HTML-формы

Использование форм HTML GET или POST для отправки данных на сервер. **Метод** атрибут **формы** элемента предоставляет метод HTTP:

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

Метод по умолчанию — GET. Если форма использует GET, формы, в которой данные кодируются в виде строки запроса URI. Если в форме используется POST, данные формы помещается в тексте запроса. Для отправки данных **enctype** атрибут задает формат текста запроса:

| Enctype | Описание: |
| --- | --- |
| application/x-www-form-urlencoded | Данные формы кодируются как пары имя/значение, аналогичную строку запроса URI. Это формат по умолчанию для POST. |
| данные multipart/формы | Данные формы кодируются как составное сообщение MIME. Этот формат следует используйте, если при передаче файла на сервер. |

Часть 1 из этой статье рассматривается x-www-формы-urlencoded формат. [Часть 2](sending-html-form-data-part-2.md) описывает составное сообщение MIME.

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a>Отправка сложных типов

Как правило будет отправлять сложный тип, состоящий из значения, взятые из нескольких элементов управления формы. Рассмотрим следующую модель, представляющий состояние.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

Вот контроллер веб-API, который принимает `Update` объекта с помощью запроса POST.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> Этот контроллер с помощью [маршрутизации на основе действия](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), поэтому в шаблоне маршрута будет &quot;api / {controller} / {action} / {id}&quot;. Клиент будет размещать данные для &quot;/api/updates/complex&quot;.


Теперь давайте напишем HTML-формы для пользователей отправить обновление состояния.

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

Обратите внимание, что **действие** атрибут на форме представляет собой URI наши действия контроллера. Вот какие-либо значения, введенные в форму.

![](sending-html-form-data-part-1/_static/image1.png)

Когда пользователь нажимает кнопку Submit, браузер отправляет запрос HTTP следующего вида:

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

Обратите внимание на то, что текст запроса содержит данные формы, в формате пар "имя значение". Веб-API автоматически преобразует пары "имя значение" в экземпляре `Update` класса.

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a>Отправка данных формы с помощью AJAX

Когда пользователь отправляет форму, браузер выходит за пределы текущей страницы и отображает текст ответного сообщения. Это нормально, когда ответ является HTML-страницы. С помощью веб-API, тем не менее, текст ответа — обычно либо пуст или содержит структурированных данных, таких как JSON. В этом случае более разумно для отправки запросов данных формы, с помощью AJAX, чтобы страницы может обработать ответ.

Ниже показано, как при отправке данных формы, с помощью jQuery.

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

JQuery **отправить** функция заменяет действий формы с помощью новой функции. Это значение переопределяет поведение по умолчанию для кнопки «Отправить». **Сериализации** функция сериализует данные формы в пары "имя значение". Чтобы отправить данные формы на сервер, вызовите `$.post()`.

По завершении запроса `.success()` или `.error()` обработчика отображается соответствующее сообщение для пользователя.

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a>Отправка простых типов

В предыдущих разделах мы отправили сложный тип, который веб-API десериализовать в экземпляр класса модели. Вы также можете отправлять простые типы, такие как строка.

> [!NOTE]
> Перед отправкой простой тип, рекомендуется вместо этого поместив значение сложного типа. Это дает преимущества модели проверки на стороне сервера и позволяет добавлять в модель при необходимости.


Основные шаги для отправки простого типа одинаковы, но есть две небольшие отличия. Во-первых, в контроллере, необходимо снабдить имени параметра **FromBody** атрибута.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

По умолчанию веб-API будет предпринята попытка простых типов из идентификатора URI запроса. **FromBody** атрибут сообщает веб-API для чтения значения из текста запроса.

> [!NOTE]
> Веб-API считывает текст ответа и не более одного раза, только один параметр действия могут поступать из текста запроса. Если вам нужно получить несколько значений из текста запроса, определения сложного типа.


Во-вторых клиенту необходимо отправить значение в следующем формате:

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

В частности часть пары "имя/значение" должен быть пустым для простого типа. Не все браузеры поддерживают это для HTML-формы, но создавать этот формат в скрипте следующим образом:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

Вот примеры формы:

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

И Вот сценарий для отправки значения формы. Единственное отличие от предыдущего сценария — это аргумент, переданный в **блога** функции.

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

Тот же подход можно использовать для отправки массив простых типов:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a>Дополнительные ресурсы

[Часть 2: Отправка файлов и составное сообщение MIME](sending-html-form-data-part-2.md)
