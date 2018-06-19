---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'Отправка данных формы HTML в веб-API ASP.NET: данные формы urlencoded | Документы Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/15/2012
ms.topic: article
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 0ed339c4f9d5854ab5a21cdd077a4d494987101f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
ms.locfileid: "26506943"
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a>Отправка данных формы HTML в веб-API ASP.NET: данные формы urlencoded.
====================
по [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-1-form-urlencoded-data"></a>Часть 1: Данные формы urlencoded

В этой статье показано, как для отправки данных формы urlencoded контроллер веб-API.

- [Общие сведения об HTML-формы](#overview_of_html_forms)
- [Отправка сложных типов](#sending_complex_types)
- [Отправка данных по AJAX](#sending_form_data_via_ajax)
- [Отправка простых типов](#sending_simple_types)

> [!NOTE]
> [Загрузка завершенного проекта](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a>Общие сведения об HTML-формы

Использование форм HTML GET или POST для отправки данных на сервер. **Метод** атрибут **формы** элемента предоставляет метод HTTP:

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

Метод по умолчанию — GET. Если форма использует GET, формы, в которой данные кодируются в виде строки запроса URI. Если в форме используется POST, данные формы помещается в тексте запроса. Для отправки данных **enctype** атрибут указывает формат текста запроса:

| Enctype | Описание |
| --- | --- |
| application/x-www-form-urlencoded | Данные формы кодируются как пары имя значение, аналогично строку запроса URI. Это формат по умолчанию для POST. |
| данные multipart/формы | Данные формы кодируются как составного сообщения MIME. Этот формат следует используйте при отправке файла на сервер. |

Часть 1 из этой статьи рассматривается x-www-формы-urlencoded формата. [Часть 2](sending-html-form-data-part-2.md) описывает составного MIME.

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a>Отправка сложных типов

Как правило будет отправлять сложный тип, состоящий из значениями, взятыми из нескольких элементов управления формы. Рассмотрим следующую модель, представляющий обновление состояния:

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

Вот контроллер веб-API, который принимает `Update` объекта с помощью POST.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> Использует этот контроллер [маршрутизацию на основе действия](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), поэтому шаблон маршрута &quot;api / {controller} / {action} / {id}&quot;. Клиент будет отправлять данные в &quot;/api/updates/complex&quot;.


Теперь напишем HTML-формы для пользователям отправлять обновления состояния.

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

Обратите внимание, что **действия** атрибута формы представляет собой URI наши действия контроллера. Вот некоторые значения, введенные в форму.

![](sending-html-form-data-part-1/_static/image1.png)

Когда пользователь щелкает отправки, браузер отправляет запрос HTTP следующего вида:

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

Обратите внимание, что текст запроса содержит данные формы, в формате пар «имя значение». Веб-API автоматически преобразует пары «имя значение» в экземпляре `Update` класса.

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a>Отправка данных по AJAX

Когда пользователь отправляет форму, браузер выходит из текущей страницы и отображает текст сообщения ответа. Это нормально, когда ответ является HTML-страницы. С веб-API, однако текст ответа обычно является либо пуст или содержит структурированные данные, такие как JSON. В этом случае смысл для отправки данных формы, с помощью AJAX для запросов, чтобы страницу можно обработать ответ.

Ниже показано, как при отправке данных формы с использованием библиотеки jQuery.

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

JQuery **отправить** функция заменяет действий формы с помощью новой функции. Это переопределяет поведение по умолчанию для кнопки «Отправить». **Сериализации** функция сериализует данные формы в пары "имя значение". Чтобы отправить данные формы на сервер, вызовите `$.post()`.

По завершении выполнения запроса `.success()` или `.error()` обработчик отображается соответствующее сообщение для пользователя.

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a>Отправка простых типов

В предыдущих разделах мы отправили сложный тип, который десериализуется экземпляром класса модели веб-API. Можно также отправлять простые типы, такие как строка.

> [!NOTE]
> Перед отправкой простого типа, рассмотрите возможность вместо упаковки значение в сложном типе. Это дает преимущества проверки модели на стороне сервера и упрощает для расширения модели при необходимости.


Основные шаги для отправки простого типа одинаковы, но два незначительных отличий. Во-первых, в контроллере, необходимо дополнить имя параметра с **FromBody** атрибута.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

По умолчанию веб-API предпринимает попытку получения простых типов от URI запроса. **FromBody** атрибут сообщает веб-API для чтения значения из текста запроса.

> [!NOTE]
> Веб-API считывает текст ответа и не более одного раза, только один параметр действия могут поступать из текста запроса. Если вам нужно получить несколько значений из текста запроса, определите сложный тип.


Во-вторых клиенту необходимо отправить значение в следующем формате:

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

В частности часть пары имя/значение должно быть пустым для простого типа. Не все браузеры поддерживают это для HTML-форм, но создать этот формат в скрипте следующим образом:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

Ниже приведен пример формы.

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

А вот сценарий, чтобы отправить значение формы. Единственное отличие от предыдущего сценария — это аргумент, переданный в **учет** функции.

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

Тот же подход можно использовать для отправки массив простых типов:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a>Дополнительные ресурсы

[Часть 2: Отправка файла и составного MIME](sending-html-form-data-part-2.md)
