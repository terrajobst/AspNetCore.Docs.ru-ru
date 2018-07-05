---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
title: Динамическое заполнение элемента управления (C#) | Документация Майкрософт
author: wenz
description: Элемент управления DynamicPopulate в ASP.NET AJAX Control Toolkit вызывает веб-службы (или метод страницы) и заполняет результирующее значение в целевой элемент управления на t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e1fec43e-1daf-49d2-b0c7-7f1b930455cc
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 560bcf261c7c621b130f4c3585d5d3df2f5eaf27
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37392932"
---
<a name="dynamically-populating-a-control-c"></a>Динамическое заполнение элемента управления (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)

> Элемент управления DynamicPopulate в ASP.NET AJAX Control Toolkit вызывает веб-службы (или метод страницы) и заполняет результирующее значение в целевой элемент управления на странице без обновления страницы.


## <a name="overview"></a>Обзор

`DynamicPopulate` Элемента управления в ASP.NET AJAX Control Toolkit вызывает веб-службы (или метод страницы) и заполняет результирующее значение в целевой элемент управления на странице без обновления страницы. Этом руководстве показано, как выполнить такую настройку.

## <a name="steps"></a>Шаги

Во-первых, необходимо, чтобы к веб-службе ASP.NET, который реализует метод, вызываемый `DynamicPopulate`. Класс веб-службы требует `ScriptService` атрибут, который определен в `Microsoft.Web.Script.Services`; в противном случае AJAX для ASP.NET не удается создать клиентский прокси JavaScript для веб-службы, который в свою очередь, необходим `DynamicPopulate`.

Веб-метода должен ожидать, что один аргумент типа строки, называемой `contextKey`, так как `DynamicPopulate` элемент управления отправляет понимается набор сведений о контексте с каждый вызов веб-службы. Следующая веб-служба возвращает текущую дату в формате, представленный `contextKey` аргумент:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample1.aspx)]

Веб-служба затем сохраняется как `DynamicPopulate.cs.asmx`. Кроме того, вы можете реализовать `getDate()` метод как метод страницы в фактические страницы ASP.NET с `DynamicPopulate` элемента управления.

На следующем шаге создайте новый файл ASP.NET. Как всегда, первым шагом является включение `ScriptManager` на текущей странице загрузки библиотеки ASP.NET AJAX и разработку набора средств элементов управления:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample2.aspx)]

Затем добавьте элемент управления label (например с помощью HTML-элемент управления с тем же именем, или &lt; `asp:Label`  / &gt; веб-элемент управления) где позже будет показано, результатом вызова веб-службы.

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample3.aspx)]

Кнопка HTML (как элемент управления HTML, так как мы не требуют обратную передачу на сервер) будет использоваться для запуска динамического заполнения:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample4.aspx)]

Наконец, мы должны `DynamicPopulateExtender` управления связывание всех компонентов. Устанавливается следующие атрибуты (помимо очевидными, `ID` и `runat` = `"server"`):

- `TargetControlID` куда поместить результат из вызова веб-службы
- `ServicePath` путь к веб-службы (опустить, если вы хотите использовать метод страницы)
- `ServiceMethod` Имя веб-метода или метода страницы
- `ContextKey` сведения о контексте, который должны отправляться веб-службы
- `PopulateTriggerControlID` элемент, который запускает вызов веб-службы
- `ClearContentsDuringUpdate` следует ли очистить целевого элемента во время вызова веб-службы

Как вы видите, элемент управления требуются некоторые сведения, но размещением всего подряд в месте довольно просто. Далее приведена разметка для `DynamicPopulateExtender` элемента управления в текущем сценарии:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample5.aspx)]

Откройте страницу ASP.NET в браузере и нажмите кнопку ""; Вы получите текущую дату в формате месяц день год.


[![Нажмите кнопку извлекает дату с сервера](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)

Нажмите кнопку извлекает дату с сервера ([Просмотр полноразмерного изображения](dynamically-populating-a-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Вперед](dynamically-populating-a-control-using-javascript-code-cs.md)
