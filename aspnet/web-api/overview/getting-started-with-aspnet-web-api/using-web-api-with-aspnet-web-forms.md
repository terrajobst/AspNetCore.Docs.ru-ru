---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: "Использование веб-API с веб-форм ASP.NET | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2012
ms.topic: article
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: af918671a8bcc97a0050ea033ccd14dd96416e73
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/01/2017
---
<a name="using-web-api-with-aspnet-web-forms"></a>Использование веб-API с веб-форм ASP.NET
====================
по [Mike Wasson](https://github.com/MikeWasson)

Несмотря на то, что веб-API ASP.NET упаковывается вместе с ASP.NET MVC, его можно легко добавить веб-API в традиционных приложений для веб-форм ASP.NET. Этот учебник поможет выполнить действия.

## <a name="overview"></a>Обзор

Чтобы использовать веб-API в приложении Web Forms, существует два основных этапа.

- Добавить контроллер веб-API, который является производным от **ApiController** класса.
- Добавление таблицы маршрутов для **приложения\_запустить** метод.

## <a name="create-a-web-forms-project"></a>Создайте проект веб-форм

Запустите Visual Studio и выберите **новый проект** из **запустить** страницы. Или из **файл** последовательно выберите пункты **New** и затем **проекта**.

В **шаблоны** выберите **установленные шаблоны** и разверните **Visual C#** узла. В разделе **Visual C#**выберите **Web**. В списке шаблонов проектов выберите **приложениях ASP.NET Web Forms**. Введите имя для проекта и нажмите кнопку **ОК**.

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a>Создание модели и контроллера

В этом учебнике используются те же классы модели и контроллера, как [Приступая к работе](tutorial-your-first-web-api.md) учебника.

Сначала добавьте класс модели. В **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите **Добавление класса**. Имя класса продуктов и добавьте реализацию следующего:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

Добавьте контроллер веб-API в проект., А *контроллера* — это объект, который обрабатывает запросы HTTP для веб-API.

В **обозревателе решений**, щелкните правой кнопкой мыши проект. Выберите **Добавление нового элемента**.

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

В разделе **установленные шаблоны**, разверните **Visual C#** и выберите **Web**. В списке шаблонов выберите **класс контроллера Web API**. Имя контроллера «ProductsController» и нажмите кнопку **добавить**.

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

**Добавление нового элемента** мастер создаст файл с именем ProductsController.cs. Удалите методы, которые включены в мастере и добавьте следующие методы:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

Дополнительные сведения о коде в этом контроллере см. в разделе [Приступая к работе](tutorial-your-first-web-api.md) учебника.

## <a name="add-routing-information"></a>Добавить сведения о маршрутизации

Далее мы добавим маршрута URI таким образом, идентификаторы URI формы &quot;/api/продукты/&quot; направляются к контроллеру.

В **обозревателе решений**, дважды щелкните файл Global.asax для открытия файла кода Global.asax.cs. Добавьте следующие **с помощью** инструкции.

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

Затем добавьте следующий код в **приложения\_запустить** метод:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

Дополнительные сведения о маршрутизации таблицах см. в разделе [маршрутизации в ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="add-client-side-ajax"></a>Добавить AJAX на стороне клиента

Это все, что нужно для создания веб-API, в которой клиенты имеют доступ. Теперь добавим HTML-страницу, которая используется для вызова API jQuery.

Убедитесь, что на главной странице (например, *Site.Master*) включает в себя `ContentPlaceHolder` с `ID="HeadContent"`:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

Откройте файл Default.aspx. Замените стандартного текста, который находится в области основного содержимого, как показано.

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

Добавьте ссылку на исходный файл jQuery в `HeaderContent` раздела:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

Примечание: Можно легко добавить ссылку на скрипт, перетащив его из **обозревателе решений** в окне редактора кода.

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

За тега скрипта jQuery добавьте следующий блок сценария:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

При загрузке документа, этот сценарий делает запрос AJAX для &quot;api и продуктов&quot;. Запрос возвращает список продуктов в формате JSON. Сценарий добавляет сведения о продукте в таблицу HTML.

При запуске приложения, он должен выглядеть следующим образом:

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
