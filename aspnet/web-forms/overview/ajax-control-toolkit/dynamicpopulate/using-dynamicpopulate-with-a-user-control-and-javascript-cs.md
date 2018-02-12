---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
title: "С помощью DynamicPopulate с пользовательский элемент управления и JavaScript (C#) | Документы Microsoft"
author: wenz
description: "Элемент управления DynamicPopulate в наборе элементов управления ASP.NET AJAX вызывает веб-службы (или метод страницы) и заполняет результирующее значение в целевой элемент управления на t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 38ac8250-8854-444c-b9ab-8998faa41c5a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 0d98177561b72ffbe05455f785e156f91e450d36
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2018
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-c"></a>С помощью DynamicPopulate с пользовательский элемент управления и JavaScript (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)

> Элемент управления DynamicPopulate в наборе элементов управления ASP.NET AJAX вызывает веб-службы (или метод страницы) и заполняет результирующее значение в целевой элемент управления на странице без обновления страницы. Можно также запустить совокупности, используя настраиваемый код JavaScript на стороне клиента. Однако особое внимание должен быть предприняты при расширителя находится в элементе управления.


## <a name="overview"></a>Обзор

`DynamicPopulate` Элемента управления в наборе элементов управления ASP.NET AJAX вызывает веб-службы (или метод страницы) и заполняет результирующее значение в целевой элемент управления на странице без обновления страницы. Можно также запустить совокупности, используя настраиваемый код JavaScript на стороне клиента. Однако особое внимание должен быть предприняты при расширителя находится в элементе управления.

## <a name="steps"></a>Шаги

Во-первых, необходимо использовать веб-службу ASP.NET, которая реализует метод, вызываемый `DynamicPopulateExtender` элемента управления. Веб-служба реализует метод `getDate()` , ожидает один аргумент типа строки, которая называется `contextKey`, так как `DynamicPopulate` управления отправляет сведения о контексте один фрагмент с каждого вызова веб-службы. Ниже приведен код (файл `DynamicPopulate.cs.asmx`) для загрузки текущей даты в одном из трех форматов:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample1.aspx)]

На следующем шаге необходимо создать новый пользовательский элемент управления (`.ascx` файл), обозначаемого, следующее объявление в первой строке:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample2.aspx)]

Объект &lt; `label` &gt; элемент будет использоваться для отображения данных, поступающих с сервера.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample3.aspx)]

Также файл пользовательского элемента управления, мы будем использовать три переключателей, каждая из которых представляет одно из трех возможных даты форматах, поддерживаемых веб-службы. Когда пользователь щелкает на одном из переключателей, браузер будет выполняться код JavaScript, который выглядит следующим образом:

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample4.ps1)]

Этот код обращается к `DynamicPopulateExtender` (не волноваться по поводу необычной идентификатор пока, это будет рассматриваться позже) и запускает динамического заполнения данными. В контексте текущего переключатель `this.value` ссылается на его значение, которое является `format1`, `format2` или `format3` точно веб-метода ожиданиям.

Единственное, что еще отсутствуют в пользовательский элемент управления является `DynamicPopulateExtender` связывает переключателей в веб-службу управления.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample5.aspx)]

Вы можете отметить, снова необычной идентификатор, используемый в элементе управления: `mcd1$myDate` вместо `myDate`. Ранее, код JavaScript, используемый `mcd1_dpe1` для доступа к `DynamicPopulateExtender` вместо `dpe1`. Эта стратегия именования имеет особые требования при использовании `DynamicPopulateExtender` в пользовательский элемент управления. Кроме того необходимо встроить регулировать пользователя особенным образом для его работы. Создайте новую страницу ASP.NET и зарегистрируйте префикс тега для пользовательского элемента управления, только что реализованный:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample6.aspx)]

Затем следует включить ASP.NET AJAX `ScriptManager` управления на новой странице:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample7.aspx)]

Наконец добавьте пользовательский элемент управления на страницу. Необходимо установить его `ID` атрибута (и `runat="server"`, конечно), но у присваивается определенное имя: `mcd1` так как это префикс, используемый в пользовательский элемент управления для доступа к нему с помощью JavaScript.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample8.aspx)]

Вот и все! Страница работает как ожидалось: пользователь выбирает один из переключателей, элемент управления в наборе средств вызывает веб-службу и отображает текущую дату в нужном формате.


[![Переключатели размещаются в элементе управления пользователя](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)

Переключатели размещаются в пользовательском элементе управления ([Просмотр полноразмерное изображение](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))

>[!div class="step-by-step"]
[Назад](dynamically-populating-a-control-using-javascript-code-cs.md)
[Вперед](dynamically-populating-a-control-vb.md)
