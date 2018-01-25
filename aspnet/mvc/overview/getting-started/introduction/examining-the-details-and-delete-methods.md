---
uid: mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
title: "Изучение сведений и методы удаления | Документы Microsoft"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: f1d2a916-626c-4a54-8df4-77e6b9fff355
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: a4e2b075497e08334183519bf8942e4af6f7a727
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="examining-the-details-and-delete-methods"></a>Изучение сведений и методы удаления
====================
По [Рик Андерсон](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

В этой части учебника вы изучим автоматически созданный `Details` и `Delete` методы.

## <a name="examining-the-details-and-delete-methods"></a>Изучение сведений и методы удаления

Откройте `Movie` контроллера и изучить `Details` метод.

![](examining-the-details-and-delete-methods/_static/image1.png)

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample1.cs)]

Ядро формирование шаблонов MVC, созданный данным методом действия добавляется комментарий, показывающая HTTP-запроса, который вызывает метод. В данном случае это `GET` запроса, состоящая из трех сегментов URL-адрес `Movies` контроллера, `Details` метод и `ID` значение.

Код сначала упрощает поиск данных с помощью `Find` метод. Является важным средством обеспечения безопасности встроены в метод, что код проверяет `Find` метод обнаружил фильм, код попытается выполнять никаких действий с ним. Например, злоумышленник может привести к ошибкам в веб-узел, изменив созданный ссылок на URL-адрес `http://localhost:xxxx/Movies/Details/1` примерно в `http://localhost:xxxx/Movies/Details/12345` (или любое другое значение, которое не представляет фактический фильма). Если не были проверены null фильм, null фильма приведет к ошибки базы данных.

Просмотрите методы `Delete` и `DeleteConfirmed`.

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample2.cs?highlight=17)]

Обратите внимание, что `HTTP Get``Delete` метода не удаляет указанный фильм, он возвращает представления фильма, где можно отправить (`HttpPost`) удаления... Выполнение операции удаления в ответ на запрос GET (или выполнение операции редактирования, создания или любой другой операции, изменяющей данные) открывает брешь в системе безопасности. Дополнительные сведения об этом см. запись в блоге Стивен Вальтер [46 совет # ASP.NET MVC — не использовать удалить ссылки, так как они создают бреши в системе безопасности](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

Метод `HttpPost`, который удаляет данные, называется `DeleteConfirmed`, поэтому метод HTTP POST обладает уникальной сигнатурой или именем. Ниже приведены сигнатуры двух методов:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample3.cs)]

Требуется, чтобы в среде CLR перегруженные методы имели уникальную сигнатуру параметров (то же имя метода, но другой список параметров). Однако здесь требуется два метода Delete — один для GET--и один для POST, имеют одинаковую сигнатуру параметра. (Они оба должны принимать целочисленное значение в качестве параметра.)

Чтобы отсортировать эту возможность, можно сделать несколько факторов. Одна — предоставить методы разные имена. Именно это было представлено в предыдущем примере механизма формирования шаблонов. Однако в этом случае возникает небольшая проблема: ASP.NET сопоставляет сегменты URL-адреса с методами действий по имени, а при переименовании метода, как правило, маршрутизация не сможет найти этот метод. Решение показано в примере, а именно: в метод `DeleteConfirmed` следует добавить атрибут `ActionName("Delete")`. Это фактически выполняет сопоставление по системе маршрутизации, URL-адрес, включающий */Delete/* для POST запрос найдет `DeleteConfirmed` метод.

Другой распространенный способ избежать проблемы с методами, которые имеют одинаковые имена и сигнатуры является искусственно изменить сигнатуру метода POST для включения неиспользуемый параметр. Например, некоторые разработчики добавить тип параметра `FormCollection` , передаваемого в метод POST, а затем просто не использовать параметр:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="summary"></a>Сводка

Теперь у вас есть полное приложение ASP.NET MVC, сохраняет данные в локальной базе данных базы данных. Можно создать, чтение, обновление, удаление и поиск фильмов.

![](examining-the-details-and-delete-methods/_static/image2.png)

## <a name="next-steps"></a>Следующие шаги

После построения и тестирования веб-приложения, следующим шагом является предоставление другим пользователям для использования в Интернете. Чтобы сделать это, необходимо развернуть его на веб-поставщик услуг размещения. Корпорация Майкрософт предлагает бесплатные веб-размещения для до 10 веб-сайтов в [освободить пробной учетной записи Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Я рекомендую вы выполните my учебника [развернуть приложение для защиты ASP.NET MVC с членством, OAuth и базы данных SQL на Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Отлично учебник — Tom Dykstra промежуточного уровня [Создание модели данных Entity Framework для приложения ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). [StackOverflow](http://stackoverflow.com/help) и [форумы по ASP.NET MVC](https://forums.asp.net/1146.aspx) являются лучшие помещает задавать вопросы. Выполните [мне](https://twitter.com/RickAndMSFT) в twitter, поэтому можно получить обновления на Мои последние учебники.

Отзыв приветствия.

— [Рик Андерсон](https://blogs.msdn.com/rickAndy) twitter:[@RickAndMSFT](https://twitter.com/RickAndMSFT)  
— [Скотт Хансельман](http://www.hanselman.com/blog/) twitter:[@shanselman](https://twitter.com/shanselman)

>[!div class="step-by-step"]
[Назад](adding-validation.md)
