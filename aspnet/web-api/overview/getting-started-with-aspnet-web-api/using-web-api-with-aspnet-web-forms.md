---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Использование веб-API с помощью веб-форм ASP.NET | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 04/03/2012
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: c9da38d290a812e94ed9473386ba6b897447dac5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840892"
---
<a name="using-web-api-with-aspnet-web-forms"></a>Использование веб-API с помощью веб-форм ASP.NET
====================
по [Майк Уоссон](https://github.com/MikeWasson)

Несмотря на то, что веб-API ASP.NET поставляется в комплекте с ASP.NET MVC, это легко реализовать веб-API, на традиционное приложение веб-форм ASP.NET. Этот учебник поможет выполнить действия.

## <a name="overview"></a>Обзор

Чтобы использовать веб-API в приложении Web Forms, существует два основных этапа:

- Добавить контроллер веб-API, который является производным от **ApiController** класса.
- Добавление таблицы маршрутов для **приложения\_запустить** метод.

## <a name="create-a-web-forms-project"></a>Создать проект веб-форм

Запустите Visual Studio и выберите **новый проект** из **запустить** страницы. Или с **файл** меню, выберите **New** и затем **проекта**.

В **шаблоны** области выберите **установленные шаблоны** и разверните **Visual C#** узла. В разделе **Visual C#** выберите **Web**. В списке шаблонов проектов выберите **приложения веб-форм ASP.NET**. Введите имя для проекта и нажмите кнопку **ОК**.

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a>Создание модели и контроллера

В этом учебнике используется те же классы модели и контроллера, как [Приступая к работе](tutorial-your-first-web-api.md) руководства.

Во-первых добавьте класс модели. В **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите **Добавление класса**. Имя класса продуктов и добавьте следующую реализацию:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

Добавьте контроллер Web API в проект., объект *контроллера* — объект, который обрабатывает HTTP-запросы для веб-API.

В **обозревателе решений**, щелкните правой кнопкой мыши проект. Выберите **Добавление нового элемента**.

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

В разделе **установленные шаблоны**, разверните **Visual C#** и выберите **Web**. Выберите из списка шаблонов, **класс контроллера веб-API**. Имя контроллера «ProductsController» и нажмите кнопку **добавить**.

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

**Добавление нового элемента** мастер создаст файл с именем ProductsController.cs. Удалить методы, которые включены в мастере и добавьте следующие методы:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

Дополнительные сведения о коде в этом контроллере, см. в разделе [Приступая к работе](tutorial-your-first-web-api.md) руководства.

## <a name="add-routing-information"></a>Добавить сведения о маршрутизации

Далее мы добавим URI маршрут таким образом, URI-адреса формы &quot;/API/продукты/&quot; направляются к контроллеру.

В **обозревателе решений**, дважды щелкните файл Global.asax, чтобы открыть файл с выделенным кодом Global.asax.cs. Добавьте следующий **с помощью** инструкции.

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

Затем добавьте следующий код, чтобы **приложения\_запустить** метод:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

Дополнительные сведения о таблицы маршрутизации, см. в разделе [маршрутизации в ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="add-client-side-ajax"></a>Добавление AJAX на стороне клиента

Это все необходимое для создания веб-API, в которой клиенты могут обращаться. Теперь давайте добавим страницу HTML, который использует jQuery для вызова API.

Убедитесь, что главной странице (например, *Site.Master*) включает в себя `ContentPlaceHolder` с `ID="HeadContent"`:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

Откройте файл Default.aspx. Замените стандартный текст, который находится в области основного содержимого, как показано:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

Затем добавьте ссылку на исходный файл jQuery в `HeaderContent` разделе:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

Примечание: Можно легко добавить ссылку на сценарий, перетащив файл из **обозревателе решений** в окне редактора кода.

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

Под jQuery тег сценария добавьте следующий блок сценария:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

При загрузке документа, выполняющий запрос AJAX к &quot;api/products&quot;. Запрос возвращает список продуктов в формате JSON. Сценарий добавляет сведения о продукте в таблице HTML.

При запуске приложения, он должен выглядеть следующим образом:

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
