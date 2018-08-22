---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
title: Динамическое заполнение элемента управления, с помощью кода JavaScript (C#) | Документация Майкрософт
author: wenz
description: Элемент управления DynamicPopulate в ASP.NET AJAX Control Toolkit вызывает веб-службы (или метод страницы) и заполняет результирующее значение в целевой элемент управления на t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: cc4c2def-e88c-4456-ae8b-a6ae0ff8cc2d
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
msc.type: authoredcontent
ms.openlocfilehash: e8b49f41f132cc31ca458ce0af3b74dbb54f225e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836971"
---
<a name="dynamically-populating-a-control-using-javascript-code-c"></a>Динамическое заполнение элемента управления, с помощью кода JavaScript (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)

> Элемент управления DynamicPopulate в ASP.NET AJAX Control Toolkit вызывает веб-службы (или метод страницы) и заполняет результирующее значение в целевой элемент управления на странице без обновления страницы. Можно также активировать заполнение, с помощью пользовательского кода JavaScript на стороне клиента.


## <a name="overview"></a>Обзор

`DynamicPopulate` Элемента управления в ASP.NET AJAX Control Toolkit вызывает веб-службы (или метод страницы) и заполняет результирующее значение в целевой элемент управления на странице без обновления страницы. Можно также активировать заполнение, с помощью пользовательского кода JavaScript на стороне клиента.

## <a name="steps"></a>Шаги

Во-первых, необходимо, чтобы к веб-службе ASP.NET, который реализует метод, вызываемый `DynamicPopulateExtender` элемента управления. Веб-служба реализует метод `getDate()` , ожидает один аргумент типа строки, называемой `contextKey`, так как `DynamicPopulate` элемент управления отправляет понимается набор сведений о контексте с каждый вызов веб-службы. Ниже приведен код (файл `DynamicPopulate.cs.asmx`) который получает текущую дату в одном из трех форматов:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample1.aspx)]

На следующем шаге создайте новый сайт ASP.NET и начать с элементом управления ASP.NET AJAX ScriptManager:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample2.aspx)]

Затем добавьте элемент управления label (например с помощью HTML-элемент управления с тем же именем, или `<asp:Label />` веб-элемент управления) где позже будет показано, результатом вызова веб-службы.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample3.aspx)]

Далее включите `DynamicPopulateExtender` управление и предоставляют данные веб-службы, целевой элемент управления, но не имя элемента управления, который запускает заполнение, это будет сделано позже с помощью пользовательским кодом JavaScript!

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample4.aspx)]

Теперь в компонент JavaScript. `$find()` Функции, определенные в библиотеке AJAX для ASP.NET, такие как возвращает ссылку на стороне сервера объекты ASP.NET AJAX Control Toolkit `DynamicPopulateExtender`. В текущем файле `$find("dpe")` возвращает ссылку на `DynamicPopulateExtender` элемента управления на странице. Он предоставляет метод, называемый `populate()` чего начнется процесс динамического заполнения. `populate()` Метод требует один аргумент: контекстный ключ, который будет использоваться в качестве аргумента `getDate()` веб-метод. Например `$find("dpe").populate("format1")` заполнения метку с текущей датой в формате месяц день год.

Чтобы сделать пример более гибкий, пользователь может выбрать один из нескольких форматов даты. Для каждой из них отображается типа "переключатель". Один раз когда пользователь щелкает переключатель, код JavaScript динамически заполняет метку с выбранным форматом даты. Ниже приведены эти переключатели.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample5.aspx)]

Обратите внимание, что в контексте типа "переключатель", выражение JavaScript `this.value` ссылается на значение «текущий», которая может быть точно те же данные `getDate()` метод может работать с.


[![Нажмите кнопку извлекает дату с сервера, в указанном формате](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)

Нажмите кнопку извлекает дату с сервера, в формате, заданном ([Просмотр полноразмерного изображения](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](dynamically-populating-a-control-cs.md)
> [Вперед](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
