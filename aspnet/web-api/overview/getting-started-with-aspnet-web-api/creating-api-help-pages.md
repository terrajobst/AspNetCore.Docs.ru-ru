---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: "Создание страниц справки для веб-API ASP.NET | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2013
ms.topic: article
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 18d04492529e96b6c0e14f1d7a30378b4832f4c8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="creating-help-pages-for-aspnet-web-api"></a>Создание страниц справки для веб-API ASP.NET
====================
по [Mike Wasson](https://github.com/MikeWasson)

При создании веб-API, часто бывает полезно создать страницу справки, чтобы другие разработчики знали способ вызова API. Вы создаете всю документацию вручную, но лучше максимально функции автоформирования.

Чтобы облегчить эту задачу, веб-API ASP.NET представляет собой библиотеку для автоматического создания страниц справки во время выполнения.

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a>Создание страниц справки API

Установка [ASP.NET и веб-средств 2012.2 обновления](https://go.microsoft.com/fwlink/?LinkId=282650). Это обновление интегрирует страниц справки в шаблоне проекта веб-API.

Затем создайте новый проект ASP.NET MVC 4 и выберите шаблон проекта веб-API. Шаблон проекта создает контроллер пример API с именем `ValuesController`. Этот шаблон также создает страниц справки API. Все файлы кода для страницы справки, помещаются в папку областей проекта.

![](creating-api-help-pages/_static/image2.png)

При запуске приложения, домашняя страница содержит ссылки на страницу справки по API. На домашней странице относительный путь — / Help.

![](creating-api-help-pages/_static/image3.png)

Эта ссылка будет произведен переход в страницу сводки API.

![](creating-api-help-pages/_static/image4.png)

Представление MVC для этой страницы определяется в Areas/HelpPage/Views/Help/Index.cshtml. Можно изменить эту страницу для изменения макета, введение, заголовок, стили и т. д.

Основная часть страницы — это таблица, API-функций, сгруппированных по контроллера. Записи таблицы создаются динамически с помощью **IApiExplorer** интерфейса. (Мы поговорим об этом интерфейсе несколько более поздней версии.) При добавлении нового контроллера API таблицы автоматически обновляется во время выполнения.

В столбце «API» перечислены метод HTTP и относительного URI. В столбце «Описание» содержит документацию для каждого API-интерфейса. Изначально документация является просто текста заполнителя. В следующем разделе я покажу, как добавить документации из комментариев XML.

Каждый API содержит ссылку на страницу с более подробные сведения, включая пример тексты запросов и ответов.

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a>Добавление страницы справки в существующий проект

Страницы справки можно добавить в существующий проект веб-API с помощью диспетчера пакетов NuGet. Этот параметр полезен в случае запуска из другой проект шаблона, чем шаблон «Веб-API».

Из **средства** последовательно выберите пункты **диспетчер пакетов библиотеки**, а затем выберите **консоль диспетчера пакетов**. В [консоль диспетчера пакетов](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) окно, введите одну из следующих команд:

Для **C#** приложения:`Install-Package Microsoft.AspNet.WebApi.HelpPage`

Для **Visual Basic** приложения:`Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`

Существует два пакета, один для C# и один для Visual Basic. Убедитесь, что используйте тот, который соответствует проекту.

Эта команда устанавливает необходимые сборки и добавляет в представления MVC для страниц справки (расположенный в папке областей или HelpPage). Необходимо вручную добавить ссылку на страницу справки. URI имеет следующий вид/Help. Чтобы создать связь в представлении razor, добавьте следующие строки:

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

Кроме того убедитесь, что для регистрации области. В файле Global.asax, добавьте следующий код в **приложения\_запустить** метод, если он еще не существует:

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a>Добавление документации по API

По умолчанию с помощью страницы имеют заполнитель строки для документации. Можно использовать [комментарии XML-документации](https://msdn.microsoft.com/en-us/library/b2s063f7.aspx) для создания документации. Чтобы включить эту функцию, откройте файл областей HelpPage приложений и\_Start/HelpPageConfig.cs и раскомментируйте следующую строку:

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

Теперь можно включите XML-документацию. В обозревателе решений щелкните правой кнопкой мыши проект и выберите **свойства**. Выберите **построения** страницы.

![](creating-api-help-pages/_static/image6.png)

В разделе **вывода**, проверьте **XML-файл документации**. В поле ввода введите «приложения\_Data/XmlDocument.xml».

![](creating-api-help-pages/_static/image7.png)

Затем откройте код `ValuesController` контроллер API, который определен в /Controllers/ValuesControler.cs. Добавьте несколько комментариев документации методы контроллера. Пример:

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> Совет: Если поместите курсор на строку выше метод и введите три косые черты, Visual Studio автоматически вставляет XML-элементов. Затем можно заполнить пустые значения.


Теперь сборки и снова запустить приложение и перейдите к странице справки. В таблице API появится строк документации.

![](creating-api-help-pages/_static/image8.png)

На странице справки читает строки из XML-файла во время выполнения. (При развертывании приложения, убедитесь, что развертывание XML-файл.)

## <a name="under-the-hood"></a>Взгляд изнутри

Страницы справки строятся на основе **ApiExplorer** класс, который является частью платформы веб-API. **ApiExplorer** класс сырье служит для создания страницы справки. Для каждого API **ApiExplorer** содержит **ApiDescription** , описывающий API. Для этой цели «API» определяется как сочетание метод HTTP и относительного URI. Например ниже приведены некоторые различных API-интерфейсы.

- ПОЛУЧИТЬ /api/Products
- ПОЛУЧИТЬ /api/продукты / {id}
- POST/api/продуктов

Если действие контроллера поддерживает несколько методов HTTP **ApiExplorer** считает каждый метод различных API.

Чтобы скрыть интерфейс API из **ApiExplorer**, добавьте **ApiExplorerSettings** действия и установите атрибут *IgnoreApi* значение true.

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

Этот атрибут также можно добавить к контроллеру, исключаемый контроллера целиком.

Класс ApiExplorer получает строк документации из **IDocumentationProvider** интерфейса. Как показано выше, библиотека страницах справки предоставляет **IDocumentationProvider** из строк XML-документации, который получает документации. Код находится в /Areas/HelpPage/XmlDocumentationProvider.cs. Можно получить документации из другого источника, написав собственные **IDocumentationProvider**. Чтобы подключить, вызовите **SetDocumentationProvider** метод расширения, определенные в **HelpPageConfigurationExtensions**

**ApiExplorer** автоматически вызывает **IDocumentationProvider** интерфейс для получения строк документации для каждого API-интерфейса. Она сохраняет их в **документации** свойство **ApiDescription** и **ApiParameterDescription** объектов.

## <a name="next-steps"></a>Дальнейшие действия

Вы не ограничены страниц справки, показано ниже. На самом деле **ApiExplorer** не только на создание страниц справки. Ло Huang Yao написаны некоторые значительные записи в блогах для получения вы думаете без дополнительной настройки:

- [Добавление простой тестовый клиент страница справки ASP.NET Web API](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [Создание веб-API справки страницы ASP.NET работать над резидентные службы](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [Создание во время разработки справочной страницы (или клиента) для веб-API ASP.NET](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [Дополнительные настройки страницы справки](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
