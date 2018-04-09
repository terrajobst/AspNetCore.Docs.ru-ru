---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
title: С помощью класса TagBuilder можно построить вспомогательные методы HTML (C#) | Документы Microsoft
author: StephenWalther
description: Стивен Вальтер приведены полезные служебный класс в платформе ASP.NET MVC с именем класса TagBuilder. Легко можно использовать класс TagBuilder...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 3975a52f-bd15-4edd-8f3d-1df93672515b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 6c0e8e4e3a733f2cc8690dc85e3006bce6c661d2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-c"></a>С помощью класса TagBuilder можно построить вспомогательные методы HTML (C#)
====================
по [Стивен Вальтер](https://github.com/StephenWalther)

> Стивен Вальтер приведены полезные служебный класс в платформе ASP.NET MVC с именем класса TagBuilder. Класс TagBuilder можно использовать для облегчения создания HTML-теги.


Платформа ASP.NET MVC включает полезные служебный класс с именем класса TagBuilder, который можно использовать при создании вспомогательных методов HTML. Класс TagBuilder, как видно из имени класса, можно легко создавать HTML-теги. На этом краткий учебник предоставляются общие сведения о классе TagBuilder и вы узнаете, как использовать этот класс при создании простого вспомогательный метод HTML, который прорисовывает HTML- &lt;img&gt; тегов.

## <a name="overview-of-the-tagbuilder-class"></a>Обзор класса TagBuilder

Класс TagBuilder содержится в пространстве имен System.Web.Mvc. Он имеет пять методов:

- AddCssClass() - позволяет добавлять новый *класс = "»* атрибут к тегу.
- GenerateId() - позволяет добавить атрибут id тега. Этот метод автоматически замещающий точки в идентификаторе (по умолчанию точки заменяются символами подчеркивания)
- MergeAttribute() - позволяет добавить атрибуты к тегу. Существует несколько перегрузок данного метода.
- SetInnerText() - позволяет задать внутренний текст тега. Внутренний текст не кодировка HTML.
- ToString() - позволяет отображать тег. Можно указать, хотите ли вы создать обычный тег, открывающего тега, закрывающего тега или самозакрывающегося тега.
  

Класс TagBuilder имеет четыре важных свойства:

- Атрибуты — представляет все атрибуты тега.
- IdAttributeDotReplacement - представляет символ, используемый методом GenerateId() для замены периодов (по умолчанию — символ подчеркивания).
- InnerHTML - представляет внутреннее содержимое тега. Присвоение этому свойству строки *не* HTML-кодирования строки.
- TagName - представляет имя тега.

Эти методы и свойства предоставляют все основные методы и свойства, необходимые для построения тег HTML. Действительно не требуется использовать класс TagBuilder. Вместо этого можно использовать класс StringBuilder. Однако класс TagBuilder упрощает жизнь немного.

## <a name="creating-an-image-html-helper"></a>Создание вспомогательного метода HTML изображения

При создании экземпляра класса TagBuilder, необходимо передать имя тега, который требуется выполнить сборку в конструктор TagBuilder. Затем можно вызывать методы, как методы AddCssClass и MergeAttribute(), чтобы изменить атрибуты тега. Наконец вызвать метод ToString() для визуализации тега.

Например список 1 содержит вспомогательный метод HTML изображения. Реализуется внутри поддержки изображений с TagBuilder, который представляет элемент HTML &lt;img&gt; тег.

**Листинг 1 - Helpers\ImageHelper.cs**

[!code-csharp[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample1.cs)]

Класс в список 1 содержит два статических перегруженные методы, с именем образа. При вызове метода Image(), можно передать объект, представляющий набор атрибутов HTML или нет.

Обратите внимание на то, как метод TagBuilder.MergeAttribute() используется для добавления отдельных атрибутов, таких как атрибут src для TagBuilder. Обратите внимание на то, кроме того, как используется метод TagBuilder.MergeAttributes() Добавление TagBuilder коллекцию атрибутов. Метод MergeAttributes() принимает словарь&lt;строка, объект&gt; параметра. Класс RouteValueDictionary используется для преобразования в объект, представляющий коллекцию атрибутов в словарь&lt;строка, объект&gt;.

После создания поддержки изображений можно использовать вспомогательный объект в представлений ASP.NET MVC так же, как любые другие стандартные вспомогательные методы HTML. Представление в списке 2 использует поддержки изображений для отображения один и тот же образ Xbox дважды (см. рис. 1). Вспомогательный объект Image() вызывается как с и без коллекции атрибутов HTML.

**Листинг 2 - Home\Index.aspx**

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample2.aspx)]


[![Диалоговое окно нового проекта](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)

**На рисунке 01**: использование вспомогательного метода изображения ([Просмотр полноразмерное изображение](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))


Обратите внимание, что необходимо импортировать пространство имен, связанное со вспомогательным методом изображения в верхней части представления Index.aspx. Вспомогательный объект импортируется с следующую директиву:

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample3.aspx)]

> [!div class="step-by-step"]
> [Назад](creating-custom-html-helpers-cs.md)
> [Вперед](creating-page-layouts-with-view-master-pages-cs.md)
