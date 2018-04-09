---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: Создание действия (VB) | Документы Microsoft
author: microsoft
description: Дополнительные сведения о добавлении нового действия для контроллера ASP.NET MVC. Дополнительные сведения о требованиях для метода для действия.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: c77e4738444c61d60bdd78a50b36f98be41fc271
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="creating-an-action-vb"></a>Создание действия (Visual Basic)
====================
по [Microsoft](https://github.com/microsoft)

> Дополнительные сведения о добавлении нового действия для контроллера ASP.NET MVC. Дополнительные сведения о требованиях для метода для действия.


Целью данного учебника является объясняется, как можно создать новое действие контроллера. Дополнительные сведения о требованиях к метода действия. Вы также узнаете, как предотвратить раскрытие в качестве действия метода.

## <a name="adding-an-action-to-a-controller"></a>Добавление действия к контроллеру

Добавьте новое действие к контроллеру, путем добавления нового метода к контроллеру. Например контроллер в листинге 1 содержит действия с именем Index() и действие с именем SayHello(). Оба метода доступны как действия.

**Листинг 1 - Controllers\HomeController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

Чтобы предоставляться вселенной как действие, метод должны соответствовать определенным требованиям:

- Метод должен быть открытым.
- Метод не может быть статическим.
- Метод не может быть методом расширения.
- Метод не может быть конструктор, метод получения или задания.
- Метод не может иметь открытые универсальные типы.
- Метод не является методом базового класса контроллера.
- Этот метод не может содержать **ref** или **out** параметров.

Обратите внимание, что нет никаких ограничений на тип возвращаемого значения действия контроллера. Действие контроллера может вернуть строку, DateTime, экземпляр класса Random или void. Платформа ASP.NET MVC будет преобразовать любой тип возврата, не результат действия в строку и просмотру строки в браузере.

При добавлении любого метода, который не нарушает эти требования к контроллеру, метод указывается в виде действия контроллера. Здесь следует соблюдать осторожность. Действие контроллера может вызываться все, кто подключен к Интернету. Нет, например, создать действие DeleteMyWebsite() контроллера.

## <a name="preventing-a-public-method-from-being-invoked"></a>Предотвращение открытый метод вызова

Если необходимо создать открытый метод в класс контроллера и вы не хотите предоставлять метод как действия контроллера, то метод может предотвратить вызов с помощью &lt;NonAction&gt; атрибута. Например, контроллер в списке 2 содержит открытый метод с именем CompanySecrets(), дополненному &lt;NonAction&gt; атрибута.

**Листинг 2 - Controllers\WorkController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

При попытке вызвать действие контроллера CompanySecrets(), введя в адресной строке браузера /Work/CompanySecrets вы сможете получить сообщение об ошибке на рис. 1.


[![Вызов метода NonAction](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)

**На рисунке 01**: вызов метода NonAction ([Просмотр полноразмерное изображение](creating-an-action-vb/_static/image2.png))

> [!div class="step-by-step"]
> [Назад](creating-a-controller-vb.md)
> [Вперед](aspnet-mvc-controllers-overview-cs.md)
