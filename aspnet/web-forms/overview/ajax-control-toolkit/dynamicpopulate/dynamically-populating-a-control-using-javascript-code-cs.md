---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
title: Динамически заполнение элемента управления с помощью кода JavaScript (C#) | Документы Microsoft
author: wenz
description: Элемент управления DynamicPopulate в наборе элементов управления ASP.NET AJAX вызывает веб-службы (или метод страницы) и заполняет результирующее значение в целевой элемент управления на t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: cc4c2def-e88c-4456-ae8b-a6ae0ff8cc2d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
msc.type: authoredcontent
ms.openlocfilehash: c9fd11ea0348eb7fe9a7634f7b26031339146828
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="dynamically-populating-a-control-using-javascript-code-c"></a>Динамически заполнение элемента управления с помощью кода JavaScript (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)

> Элемент управления DynamicPopulate в наборе элементов управления ASP.NET AJAX вызывает веб-службы (или метод страницы) и заполняет результирующее значение в целевой элемент управления на странице без обновления страницы. Можно также запустить совокупности, используя настраиваемый код JavaScript на стороне клиента.


## <a name="overview"></a>Обзор

`DynamicPopulate` Элемента управления в наборе элементов управления ASP.NET AJAX вызывает веб-службы (или метод страницы) и заполняет результирующее значение в целевой элемент управления на странице без обновления страницы. Можно также запустить совокупности, используя настраиваемый код JavaScript на стороне клиента.

## <a name="steps"></a>Шаги

Во-первых, необходимо использовать веб-службу ASP.NET, которая реализует метод, вызываемый `DynamicPopulateExtender` элемента управления. Веб-служба реализует метод `getDate()` , ожидает один аргумент типа строки, которая называется `contextKey`, так как `DynamicPopulate` управления отправляет сведения о контексте один фрагмент с каждого вызова веб-службы. Ниже приведен код (файл `DynamicPopulate.cs.asmx`) для загрузки текущей даты в одном из трех форматов:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample1.aspx)]

На следующем шаге создайте новый сайт ASP.NET и начать с элементом управления ASP.NET AJAX ScriptManager:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample2.aspx)]

Добавьте элемент управления label (например с помощью HTML-элемент управления с таким же именем или `<asp:Label />` веб-элемента управления) позже которого отображается результат вызова веб-службы.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample3.aspx)]

Теперь включают `DynamicPopulateExtender` управление и предоставляют информации веб-службы, целевой элемент управления, но не имя элемента управления, который запускает это будет сделано позже на заполнение с использованием JavaScript пользовательские!

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample4.aspx)]

Теперь в компонент JavaScript. `$find()` Функции, определенные библиотека ASP.NET AJAX, возвращает ссылку на объекты серверных элементов управления ASP.NET AJAX, такие как `DynamicPopulateExtender`. В текущем файле `$find("dpe")` возвращает ссылку на версию `DynamicPopulateExtender` управления на странице. Он предоставляет метод, называемый `populate()` которое запускает процесс динамического заполнения. `populate()` Метод требует один аргумент: ключ контекста, который будет использоваться в качестве аргумента для `getDate()` веб-метода. Например `$find("dpe").populate("format1")` заполнит метку с текущей даты в формате месяц день год.

Чтобы сделать более гибкое образца, пользователь может выбрать один из нескольких форматов даты. Переключатель отображается для каждой из них. Один раз когда пользователь щелкает переключатель, код JavaScript динамически заполняет метку с выбранным форматом даты. Ниже приведены эти переключатели.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample5.aspx)]

Обратите внимание, что в контексте типа "переключатель", выражение JavaScript `this.value` ссылается на значение «текущий», которая может быть точно те же данные `getDate()` метод может работать с.


[![Нажмите кнопку извлекает дату с сервера в формате, заданном](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)

Нажмите кнопку извлекает дату с сервера в формате, заданном ([Просмотр полноразмерное изображение](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](dynamically-populating-a-control-cs.md)
> [Вперед](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
