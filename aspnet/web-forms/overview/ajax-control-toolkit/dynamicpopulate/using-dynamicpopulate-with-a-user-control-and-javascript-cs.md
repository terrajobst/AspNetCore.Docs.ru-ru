---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
title: Использование DynamicPopulate с пользовательского элемента управления и JavaScript (C#) | Документация Майкрософт
author: wenz
description: Элемент управления DynamicPopulate в ASP.NET AJAX Control Toolkit вызывает веб-службы (или метод страницы) и заполняет результирующее значение в целевой элемент управления на t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 38ac8250-8854-444c-b9ab-8998faa41c5a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 110f6dd05d038438bc061d3ee907a5e2da8968c6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828604"
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-c"></a>Использование DynamicPopulate с пользовательского элемента управления и JavaScript (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)

> Элемент управления DynamicPopulate в ASP.NET AJAX Control Toolkit вызывает веб-службы (или метод страницы) и заполняет результирующее значение в целевой элемент управления на странице без обновления страницы. Можно также активировать заполнение, с помощью пользовательского кода JavaScript на стороне клиента. Тем не менее, выполняемый при расширителя находится в пользовательский элемент управления имеет особое внимание.


## <a name="overview"></a>Обзор

`DynamicPopulate` Элемента управления в ASP.NET AJAX Control Toolkit вызывает веб-службы (или метод страницы) и заполняет результирующее значение в целевой элемент управления на странице без обновления страницы. Можно также активировать заполнение, с помощью пользовательского кода JavaScript на стороне клиента. Тем не менее, выполняемый при расширителя находится в пользовательский элемент управления имеет особое внимание.

## <a name="steps"></a>Шаги

Во-первых, необходимо, чтобы к веб-службе ASP.NET, который реализует метод, вызываемый `DynamicPopulateExtender` элемента управления. Веб-служба реализует метод `getDate()` , ожидает один аргумент типа строки, называемой `contextKey`, так как `DynamicPopulate` элемент управления отправляет понимается набор сведений о контексте с каждый вызов веб-службы. Ниже приведен код (файл `DynamicPopulate.cs.asmx`) который получает текущую дату в одном из трех форматов:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample1.aspx)]

На следующем шаге, создайте новый пользовательский элемент управления (`.ascx` файл), обозначаемого, следующее объявление в первой строке:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample2.aspx)]

Объект &lt; `label` &gt; элемент будет использоваться для отображения данных, поступающих с сервера.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample3.aspx)]

Также в файле параметров пользователя, мы будем использовать три переключателей, каждый из которых представляет одно из трех возможных даты форматов, поддерживаемых веб-службы. Когда пользователь щелкает один из переключателей, браузер выполнит код JavaScript, который выглядит следующим образом:

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample4.ps1)]

Этот код обращается к `DynamicPopulateExtender` (не волнуйтесь о странный код еще, это будет рассматриваться позже) и активирует динамического заполнения данными. В контексте текущего переключатель `this.value` ссылается на его значение, являющееся `format1`, `format2` или `format3` точно что ожидает веб-метода.

Единственное, что пока отсутствует в пользовательский элемент управления является `DynamicPopulateExtender` элемента управления, которые связывают переключателей с веб-службы.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample5.aspx)]

Еще раз вы также могли заметить странный код, используемый в элементе управления: `mcd1$myDate` вместо `myDate`. Ранее, код JavaScript, используемый `mcd1_dpe1` для доступа к `DynamicPopulateExtender` вместо `dpe1`. Эта стратегия именования имеет особые требования при использовании `DynamicPopulateExtender` внутри пользовательского элемента управления. Кроме того необходимо вставить регулировать пользователя определенным образом заставить их работать. Создать новую страницу ASP.NET и зарегистрировать префикс тега для пользовательского элемента управления, который вы только что реализовали:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample6.aspx)]

Затем включите ASP.NET AJAX `ScriptManager` управления на новой странице:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample7.aspx)]

Наконец добавьте пользовательский элемент управления на страницу. Необходимо установить его `ID` атрибут (и `runat="server"`, конечно), но необходимо также присвоено определенное имя: `mcd1` так как это префикс, используемый в пользовательский элемент управления для доступа к нему с помощью JavaScript.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample8.aspx)]

Вот и все! Страницы ведет себя так, как ожидалось: пользователь выбирает один из переключателей, элемент управления в наборе средств вызывает веб-службы и отображает текущую дату в нужном формате.


[![Переключатели находятся в пользовательский элемент управления](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)

Переключатели находятся в пользовательский элемент управления ([Просмотр полноразмерного изображения](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](dynamically-populating-a-control-using-javascript-code-cs.md)
> [Вперед](dynamically-populating-a-control-vb.md)
