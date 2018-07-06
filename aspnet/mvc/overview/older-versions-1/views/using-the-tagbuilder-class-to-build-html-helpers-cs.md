---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
title: Используя класс TagBuilder для создания вспомогательных методов HTML (C#) | Документация Майкрософт
author: StephenWalther
description: Стивен Вальтер представлены полезные служебный класс в платформе ASP.NET MVC с именем класс TagBuilder. Можно легко использовать класс TagBuilder для...
ms.author: aspnetcontent
ms.date: 03/02/2009
ms.assetid: 3975a52f-bd15-4edd-8f3d-1df93672515b
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 9990fc7ad8093643a564a5e02ff65264d4a7fe15
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813230"
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-c"></a>Используя класс TagBuilder для создания вспомогательных методов HTML (C#)
====================
по [Стивен Вальтер](https://github.com/StephenWalther)

> Стивен Вальтер представлены полезные служебный класс в платформе ASP.NET MVC с именем класс TagBuilder. Класс TagBuilder для простого создания HTML-теги.


Платформа ASP.NET MVC включает в себя полезные служебный класс с именем класс TagBuilder, который можно использовать при создании вспомогательных методов HTML. Класс TagBuilder, как и предполагает имя класса, позволяет легко создавать HTML-теги. Из этого краткого руководства вам будет предоставлен Обзор класс TagBuilder и вы узнаете, как использовать этот класс при построении простой вспомогательный метод HTML, который отображает HTML &lt;img&gt; теги.

## <a name="overview-of-the-tagbuilder-class"></a>Общие сведения о классе TagBuilder

Класс TagBuilder содержится в пространстве имен System.Web.Mvc. Он имеет пять методов:

- AddCssClass() - позволяет добавлять новый *класс = "»* атрибут к тегу.
- GenerateId() - позволяет добавлять атрибут id для тега. Этот метод автоматически замещающий точки в идентификаторе (по умолчанию, точки заменяются символами подчеркивания)
- MergeAttribute() - предоставляет возможность добавить атрибуты к тегу. Существует несколько перегрузок этого метода.
- SetInnerText() - позволяет задать внутренний текст тега. Внутренний текст — автоматически кодировать в HTML.
- ToString() — обеспечивает возможность визуализации тега. Можно указать, хотите ли вы создать обычный тег, открывающего тега, закрывающий тег или самозакрывающийся тег.
  

Класс TagBuilder имеет четыре важных свойства:

- Атрибуты — представляет все атрибуты тега.
- IdAttributeDotReplacement - представляет символ, используемый методом GenerateId() для замены периодов (по умолчанию — символ подчеркивания).
- InnerHTML - представляет внутреннее содержимое тега. Назначить строку для этого свойства *не* HTML кодирования строки.
- TagName - представляет имя тега.

Эти методы и свойства предоставляют все базовые методы и свойства, которые необходимы для создания HTML-тега. Вам не обязательно использовать класс TagBuilder. Вместо этого можно использовать класс StringBuilder. Тем не менее класс TagBuilder упрощает вашу жизнь немного.

## <a name="creating-an-image-html-helper"></a>Создание вспомогательного метода HTML изображения

При создании экземпляра класса TagBuilder, необходимо передать имя тега, который требуется выполнить сборку в конструктор TagBuilder. Затем можно вызывать методы, такие как методы AddCssClass и MergeAttribute(), чтобы изменить атрибуты тега. Наконец вызывается метод ToString() для отображения тега.

Например в листинге 1 содержит вспомогательный метод HTML изображения. Вспомогательная функция изображения реализуются внутренним образом с помощью TagBuilder, который представляет элемент HTML &lt;img&gt; тега.

**В листинге 1 - Helpers\ImageHelper.cs**

[!code-csharp[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample1.cs)]

Этот класс в листинге 1 содержит два статических перегруженных методов с именем образа. При вызове метода Image(), можно передать объект, представляющий набор атрибутов HTML или нет.

Обратите внимание на то, как метод TagBuilder.MergeAttribute() используется для добавления отдельных атрибутов, таких как атрибут src для TagBuilder. Обратите внимание на то, кроме того, как используется метод TagBuilder.MergeAttributes() добавляемый TagBuilder коллекции атрибутов. Метод MergeAttributes() принимает словарь&lt;строка, объект&gt; параметра. Класс RouteValueDictionary используется для преобразования в объект, представляющий коллекцию атрибутов в словарь&lt;строка, объект&gt;.

После создания вспомогательная функция изображения, можно использовать вспомогательный метод в представлениях ASP.NET MVC так же, как и любой из других стандартных вспомогательные методы HTML. В представлении в листинге 2 используется вспомогательная функция изображения для отображения одного образа Xbox дважды (см. рис. 1). Вспомогательный метод Image() вызывается с и без коллекцию атрибутов HTML.

**В листинге 2 - Home\Index.aspx**

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample2.aspx)]


[![В диалоговом окне нового проекта](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)

**Рис 01**: использование вспомогательного метода образа ([Просмотр полноразмерного изображения](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))


Обратите внимание на то, что необходимо импортировать пространство имен, связанное со вспомогательным методом изображение в верхней части представления Index.aspx. Вспомогательный метод импортируется с следующую директиву:

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample3.aspx)]

> [!div class="step-by-step"]
> [Назад](creating-custom-html-helpers-cs.md)
> [Вперед](creating-page-layouts-with-view-master-pages-cs.md)
