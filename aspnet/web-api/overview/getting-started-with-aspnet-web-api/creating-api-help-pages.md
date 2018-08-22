---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Создание страниц справки для веб-API ASP.NET | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: riande
ms.date: 04/01/2013
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 2d8758034ca4339ed7e9699cf2f2643bfab87ba4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837578"
---
<a name="creating-help-pages-for-aspnet-web-api"></a>Создание страниц справки для веб-API ASP.NET
====================
по [Майк Уоссон](https://github.com/MikeWasson)

При создании веб-API, часто бывает полезно создать страницу справки, чтобы другие разработчики будете знать, как вызвать API. Можно создать всю документацию вручную, но лучше автоматически заполнять максимально.

Чтобы облегчить эту задачу, веб-API ASP.NET предоставляет библиотеку для автоматического создания страниц справки во время выполнения.

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a>Создание страниц справки API

Установка [ASP.NET и веб-инструменты 2012.2 обновления](https://go.microsoft.com/fwlink/?LinkId=282650). Это обновление страницы справки интегрированы в шаблоне проекта веб-API.

Затем создайте новый проект ASP.NET MVC 4 и выберите шаблон проекта веб-API. Шаблон проекта создает контроллеру пример API с именем `ValuesController`. Этот шаблон также создает страницы справки API. Все файлы кода для страницы справки, помещаются в папку областей проекта.

![](creating-api-help-pages/_static/image2.png)

При запуске приложения на домашней странице содержит ссылку на страницу справки API. На домашней странице относительный путь — / Help.

![](creating-api-help-pages/_static/image3.png)

Эту ссылку, откроется страница сводки API.

![](creating-api-help-pages/_static/image4.png)

Представление MVC для этой страницы определяется в Areas/HelpPage/Views/Help/Index.cshtml. Вы можете изменить эту страницу для изменения макета, введение, title, стили и т. д.

Основную часть страницы — это таблица, API-интерфейсов, сгруппированные по контроллера. Записи таблицы создаются динамически, с помощью **IApiExplorer** интерфейс. (Я вкратце расскажу об этом интерфейсе позже.) При добавлении нового контроллера API, таблица автоматически обновляется во время выполнения.

В столбце «API» перечислены метод HTTP и относительного URI. Столбец «Description» содержит документацию для каждого API. Изначально документация является просто текст заполнителя. В следующем разделе я покажу, как добавить документацию из комментариев XML.

Каждый API содержит ссылку на страницу с более подробные сведения, включая пример текста запросов и ответов.

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a>Добавление страницы справки в существующий проект

Страницы справки можно добавить в существующий проект веб-API с помощью диспетчера пакетов NuGet. Этот параметр полезен, можно начать с шаблона другому проекту от шаблона «Веб-API».

Из **средства** меню, выберите **диспетчер пакетов библиотеки**, а затем выберите **консоль диспетчера пакетов**. В [консоль диспетчера пакетов](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) окно, введите одну из следующих команд:

Для **C#** приложения: `Install-Package Microsoft.AspNet.WebApi.HelpPage`

Для **Visual Basic** приложения: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`

Существует два пакета, для C# и один для Visual Basic. Убедитесь в том, что использовать ту, которая соответствует проекту.

Эта команда устанавливает необходимые сборки и добавляет в представления MVC для страниц справки (расположенный в папке области/HelpPage). Необходимо вручную добавить ссылку на страницу справки. Данный URI является/Help. Чтобы создать связь в представлении razor, добавьте следующие строки:

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

Кроме того убедитесь, что для регистрации области. В файле Global.asax, добавьте следующий код, чтобы **приложения\_запустить** метод, если он еще не существует:

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a>Добавление документации по API

По умолчанию с помощью страницы имеют заполнитель строки для документации. Можно использовать [комментарии XML-документации](https://msdn.microsoft.com/library/b2s063f7.aspx) документации. Чтобы включить эту функцию, откройте файл областей, приложения или HelpPage\_Start/HelpPageConfig.cs и раскомментируйте следующую строку:

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

Теперь можно включите XML-документации. В обозревателе решений щелкните правой кнопкой мыши проект и выберите **свойства**. Выберите **построения** страницы.

![](creating-api-help-pages/_static/image6.png)

В разделе **вывода**, проверьте **XML-файл документации**. В поле ввода введите «приложение\_Data/XmlDocument.xml».

![](creating-api-help-pages/_static/image7.png)

Затем откройте код `ValuesController` контроллер API, который определен в /Controllers/ValuesControler.cs. Добавьте комментарии документации методов контроллера. Пример:

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> Совет: Если вы поместите курсор в строке над методом и введите три косые черты, Visual Studio автоматически вставляет XML-элементов. Затем вы можете заполнить пустые поля.


Теперь сборки и снова запустить приложение и перейдите на страницы справки. Строками документации появятся в таблице API.

![](creating-api-help-pages/_static/image8.png)

На страницу справки читает строки из XML-файла во время выполнения. (При развертывании приложения, убедитесь, что развертывание XML-файле.)

## <a name="under-the-hood"></a>Взгляд изнутри

Страницы справки строятся на основе **ApiExplorer** класс, который является частью платформы веб-API. **ApiExplorer** класс предоставляет исходный материал для создания страницы справки. Для каждого API **ApiExplorer** содержит **ApiDescription** , описывающий API. Для этой цели «API» определяется как сочетание метод HTTP и относительного URI. Например ниже приведены некоторые разные интерфейсы API.

- ПОЛУЧИТЬ /api/Products
- GET/API/продукты / {id}
- POST/api/продуктов

Если действие контроллера поддерживает несколько методов HTTP, **ApiExplorer** считает каждый метод различных API.

Чтобы скрыть интерфейс API из **ApiExplorer**, добавьте **ApiExplorerSettings** атрибута в действие и задайте *IgnoreApi* значение true.

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

Этот атрибут также можно добавить в контроллер, чтобы исключить всего контроллера.

Класс ApiExplorer получает строк документации из **IDocumentationProvider** интерфейс. Как вы уже видели, библиотека страницы справки по предоставляет **IDocumentationProvider** , возвращает документацию из строк XML-документации. Код находится в /Areas/HelpPage/XmlDocumentationProvider.cs. Можно получить документацию из другого источника, написав собственный **IDocumentationProvider**. Для привязывания, вызовите **SetDocumentationProvider** метод расширения, определенные в **HelpPageConfigurationExtensions**

**ApiExplorer** автоматически вызывает **IDocumentationProvider** интерфейса для получения строк документации для каждого API. Она сохраняет их в **документации** свойство **ApiDescription** и **ApiParameterDescription** объектов.

## <a name="next-steps"></a>Следующие шаги

Можно не только на страницы справки, показано ниже. На самом деле **ApiExplorer** не ограничивается Создание страниц справки. Лин Хаун ЯО написал, что некоторые полезные сообщения в блогах читатель задумается без дополнительной настройки:

- [Добавление простой тестовый клиент в страницу справки ASP.NET Web API](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [Создание веб-API справки страницы ASP.NET работают на резидентные службы](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [Создание во время разработки справки страницы (или клиента) для веб-API ASP.NET](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [Дополнительные настройки страница справки](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
