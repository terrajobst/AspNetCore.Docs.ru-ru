---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: "Создание действия (C#) | Документы Microsoft"
author: microsoft
description: "Дополнительные сведения о добавлении нового действия для контроллера ASP.NET MVC. Дополнительные сведения о требованиях для метода для действия."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: 8b751dc7e34951be33e7c27a3429c383a3e1e1c7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="creating-an-action-c"></a>Создание действия (C#)
====================
по [Microsoft](https://github.com/microsoft)

> Дополнительные сведения о добавлении нового действия для контроллера ASP.NET MVC. Дополнительные сведения о требованиях для метода для действия.


Целью данного учебника является объясняется, как можно создать новое действие контроллера. Дополнительные сведения о требованиях к метода действия. Вы также узнаете, как предотвратить раскрытие в качестве действия метода.

## <a name="adding-an-action-to-a-controller"></a>Добавление действия к контроллеру

Добавьте новое действие к контроллеру, путем добавления нового метода к контроллеру. Например контроллер в листинге 1 содержит действия с именем Index() и действие с именем SayHello(). Оба метода доступны как действия.

**Листинг 1 - Controllers\HomeController.cs**

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

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

Если необходимо создать открытый метод в класс контроллера, и вы не хотите предоставлять метод как действие контроллера можно предотвратить метод вызова с помощью атрибута [NonAction]. Например контроллер в списке 2 содержит открытый метод с именем CompanySecrets(), отмеченный атрибутом [NonAction].

**Листинг 2 - Controllers\WorkController.cs**

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

При попытке вызвать действие контроллера CompanySecrets(), введя в адресной строке браузера /Work/CompanySecrets вы сможете получить сообщение об ошибке на рис. 1.


[![Вызов метода NonAction](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)

**На рисунке 01**: вызов метода NonAction ([Просмотр полноразмерное изображение](creating-an-action-cs/_static/image2.png))

>[!div class="step-by-step"]
[Назад](creating-a-controller-cs.md)
[Вперед](asp-net-mvc-routing-overview-vb.md)
