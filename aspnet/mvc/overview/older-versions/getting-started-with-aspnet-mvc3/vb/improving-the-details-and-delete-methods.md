---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/improving-the-details-and-delete-methods
title: "Улучшение сведения и методы удаления (VB) | Документы Microsoft"
author: Rick-Anderson
description: "Этот учебник поможет узнать основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, являющийся..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: c5c14ef0-c128-4dc1-8c01-7f0fdb09e411
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/improving-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: e057d9f106aaa8afbe521d8185a06dfbf48e46fb
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="improving-the-details-and-delete-methods-vb"></a>Улучшение сведения и методы удаления (Visual Basic)
====================
По [Рик Андерсон](https://github.com/Rick-Anderson)

> Этот учебник поможет узнать основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, которой — это бесплатная версия Microsoft Visual Studio. Прежде чем начать, убедитесь, что вы установили необходимые компоненты, перечисленные ниже. Все из них можно установить, щелкнув по следующей ссылке: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Кроме того можно установить отдельно предварительные требования, используя следующие ссылки:
> 
> - [Необходимые компоненты Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Обновление средств ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(среда выполнения + средства поддержки)
> 
> Если вы используете Visual Studio 2010 вместо Visual Web Developer 2010, необходимо установить необходимые компоненты, щелкнув по следующей ссылке: [необходимых компонентов Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Проект Visual Web Developer с VB.NET исходный код доступен по следующему адресу. [Загрузить версию VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Если вы предпочитаете C#, переключитесь в [C# версии](../cs/improving-the-details-and-delete-methods.md) этого учебника.


В этой части руководства вносятся некоторые улучшения для автоматически создаваемых `Details` и `Delete` методы. Эти изменения не требуются, но с небольшое небольшие отрывки кода, можно легко улучшить приложение.

## <a name="improving-the-details-and-delete-methods"></a>Улучшение сведения и методы удаления

После формирования шаблонов `Movie` контроллера, ASP.NET MVC созданный код, работает отлично, но, можно сделать более надежными с только что небольшие изменения.

Откройте `Movie` контроллера и изменения `Details` метода, возвращая `HttpNotFound` при фильма не найдены. Следует также изменить `Details` метод, чтобы задать значение по умолчанию для идентификатора, который передается в него. (Внесены соответствующие изменения для `Edit` метод в [часть 6](examining-the-edit-methods-and-edit-view.md) данного руководства.) Тем не менее, необходимо изменить тип возвращаемого значения `Details` метод `ViewResult` для `ActionResult`, так как `HttpNotFound` метод не возвращает `ViewResult` объекта. В следующем примере показано измененной `Details` метод.

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample1.vb)]

Код сначала упрощает поиск данных с помощью `Find` метод. Является важным средством обеспечения безопасности, мы создали в метод, что код проверяет, что `Find` метод обнаружил фильм, код попытается выполнять никаких действий с ним. Например, злоумышленник может привести к ошибкам в веб-узел, изменив созданный ссылок на URL-адрес `http://localhost:xxxx/Movies/Details/1` примерно в `http://localhost:xxxx/Movies/Details/12345` (или любое другое значение, которое не представляет фактический фильма). В противном случае проверка null фильма в итоге ошибки базы данных.

Аналогичным образом измените `Delete` и `DeleteConfirmed` методы задание значения по умолчанию для параметра ID, а также для возврата `HttpNotFound` при фильма не найдены. Обновленный `Delete` методы в `Movie` контроллера, показаны ниже.

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample2.vb)]

Обратите внимание, что `Delete` метода не удаляет данные. Выполнение операции удаления в ответ на запрос GET (или выполнение операции редактирования, создания или любой другой операции, изменяющей данные) открывает брешь в системе безопасности. Дополнительные сведения об этом см. запись в блоге Стивен Вальтер [46 совет # ASP.NET MVC — не использовать удалить ссылки, так как они создают бреши в системе безопасности](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

Метод `HttpPost`, который удаляет данные, называется `DeleteConfirmed`, поэтому метод HTTP POST обладает уникальной сигнатурой или именем. Ниже приведены сигнатуры двух методов:

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample3.vb)]

Общеязыковая среда выполнения (CLR) требует перегруженные методы, имели уникальная сигнатура (таким же именем, другой список параметров). Однако здесь требуется два метода Delete — один для GET--и один для POST требуются такой же сигнатурой. (Они оба должны принимать целочисленное значение в качестве параметра.)

Чтобы отсортировать эту возможность, можно сделать несколько факторов. Одна — предоставить методы разные имена. Это мы сделали он предшествующий пример. Однако в этом случае возникает небольшая проблема: ASP.NET сопоставляет сегменты URL-адреса с методами действий по имени, а при переименовании метода, как правило, маршрутизация не сможет найти этот метод. Решение показано в примере, а именно: в метод `DeleteConfirmed` следует добавить атрибут `ActionName("Delete")`. Это фактически выполняет сопоставление по системе маршрутизации, URL-адрес, включающий */Delete/*для POST запрос найдет `DeleteConfirmed` метод.

Другой способ избежать проблемы с методами, которые имеют одинаковые имена и сигнатуры — искусственно изменить сигнатуру метода POST для включения неиспользуемый параметр. Например, некоторые разработчики добавить тип параметра `FormCollection` , передаваемого в метод POST, а затем просто не использовать параметр:

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample4.vb)]

## <a name="wrapping-up"></a>Резюме

Теперь у вас есть полное приложение ASP.NET MVC, который хранит данные в базе данных SQL Server Compact. Можно создать, чтение, обновление, удаление и поиск фильмов.

![](improving-the-details-and-delete-methods/_static/image1.png)

Это основам получен работы, делая контроллеров, связывая их с представлениями и передача вокруг жестко данных. Затем вы создаются и изменяются модели данных. Entity Framework Code First создана база данных из модели данных на лету, и автоматически созданные системой формирование шаблонов ASP.NET MVC, методы действий и представления для основные операции CRUD. Затем вы добавили форму поиска, которые позволяют пользователям при поиске в базе данных. Изменения базы данных, чтобы включить новый столбец данных и затем обновлены две страницы для создания и отображения новых данных. Добавить проверку, помечая модели данных с атрибутами `DataAnnotations` пространства имен. Полученный проверки выполняется на клиенте и на сервере.

Если вы хотите развернуть приложение, полезно для тестирования приложения на локальном сервере IIS 7. Эту функцию можно использовать [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=ASPNET;) ссылку, чтобы включить параметр IIS для приложений ASP.NET. См. по следующим ссылкам развертывания:

- [Карта содержимого развертывания ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx)
- [Включение IIS 7.x](https://blogs.msdn.com/b/rickandy/archive/2011/03/14/enabling-iis-7-x-on-windows-7-vista-sp1-windows-2008-windows-2008-r2.aspx)
- [Развертывание проектов веб-приложений](https://msdn.microsoft.com/library/dd394698.aspx)

Читателю теперь можно переходить к нашей промежуточного уровня [Создание модели данных Entity Framework для приложения ASP.NET MVC](../../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) и [MVC Music Store](../../mvc-music-store/mvc-music-store-part-1.md) учебники для изучения [ASP.NET статьи в библиотеке MSDN](https://msdn.microsoft.com/library/gg416514(VS.98).aspx)и многие видеоролики и ресурсы на [https://asp.net/mvc](https://asp.net/mvc) , чтобы получить дополнительные сведения о ASP.NET MVC! [Форумы по ASP.NET MVC](https://forums.asp.net/1146.aspx) — это отличное место для вопросов.

Желаем удачи!

— Скотт Хансельман ([http://hanselman.com](http://hanselman.com) и [ @shanselman ](http://twitter.com/shanselman) в Twitter) и Рик Андерсон [blogs.msdn.com/rickAndy](https://blogs.msdn.com/rickAndy)

>[!div class="step-by-step"]
[Назад](adding-validation-to-the-model.md)
