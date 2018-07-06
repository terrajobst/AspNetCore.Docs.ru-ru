---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/improving-the-details-and-delete-methods
title: Улучшение методов Details и Delete (C#) | Документация Майкрософт
author: Rick-Anderson
description: Этом учебнике описываются основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, который является...
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: 3f42edd9-c5b8-4712-9055-970f7d38e350
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/improving-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 17789aa9e9388f3b05ece953cfc0f50ffa510f52
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803776"
---
<a name="improving-the-details-and-delete-methods-c"></a>Улучшение методов Details и Delete (C#)
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Обновленную версию этого учебника доступен [здесь](../../../getting-started/introduction/getting-started.md) , использующий ASP.NET MVC 5 и Visual Studio 2013. Она более безопасные, гораздо проще следовать и показаны дополнительные возможности.
> 
> 
> Этом учебнике описываются основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, которой является бесплатной версии Microsoft Visual Studio. Перед началом работы убедитесь, что вы установили необходимые компоненты, перечисленные ниже. Все из них можно установить, щелкнув следующую ссылку: [установщика веб-платформы](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Кроме того можно установить по отдельности необходимых компонентов, с помощью следующих ссылок:
> 
> - [Необходимые компоненты для Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Обновление средств ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(среды выполнения и средства поддержки)
> 
> Если вы используете Visual Studio 2010 вместо Visual Web Developer 2010, необходимо установить необходимые компоненты, щелкнув следующую ссылку: [необходимых компонентов Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Проект Visual Web Developer с исходным кодом C# — прилагаются в этом разделе. [Загрузить версию C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Если вы предпочитаете Visual Basic, переключитесь в [версии Visual Basic](../vb/intro-to-aspnet-mvc-3.md) работы с этим руководством.


В этой части руководства, вносятся некоторые улучшения, чтобы автоматически созданный `Details` и `Delete` методы. Эти изменения не требуются, но с несколько небольших бит кода, вы можете легко улучшить приложение.

## <a name="improving-the-details-and-delete-methods"></a>Улучшение методов Details и Delete

Когда сформированного `Movie` контроллера, ASP.NET MVC созданный код, что работало отлично, но, можно сделать более надежным, с помощью лишь небольшие изменения.

Откройте `Movie` контроллера и изменение `Details` метода, возвращая `HttpNotFound` после фильм не найдены. Следует также изменить `Details` метод, чтобы задать значение по умолчанию для идентификатора, который передается в него. (Внесенные аналогичные изменения к `Edit` метод в [часть 6](examining-the-edit-methods-and-edit-view.md) данного руководства.) Тем не менее, необходимо изменить тип возвращаемого значения `Details` метода из `ViewResult` для `ActionResult`, так как `HttpNotFound` метод не возвращает `ViewResult` объекта. В следующем примере показан измененный `Details` метод.

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample1.cs)]

Код сначала облегчает поиск данных с помощью `Find` метод. Это функция важный элемент обеспечения безопасности, который мы создали в метод является то, что код проверяет, `Find` метод обнаружил фильм, прежде чем код пытается выполнить любые действия с его. Например, злоумышленник может внести ошибки на сайт путем изменения созданного ссылками из URL-адрес `http://localhost:xxxx/Movies/Details/1` в нечто подобное `http://localhost:xxxx/Movies/Details/12345` (или любое другое значение, которое не представляет фактический фильм). Если этого не сделать проверку на наличие фильма со значением null, это может привести к ошибке базы данных.

Аналогичным образом измените `Delete` и `DeleteConfirmed` методы для задавать значение по умолчанию для параметра ID и возврата `HttpNotFound` после фильм не найдены. Обновленный `Delete` методы в `Movie` контроллера, показаны ниже.

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample2.cs)]

Обратите внимание, что `Delete` метод данные не удаляются. Выполнение операции удаления в ответ на запрос GET (или выполнение операции редактирования, создания или любой другой операции, изменяющей данные) открывает брешь в системе безопасности. Дополнительные сведения об этом см. запись в блоге Стивен Вальтер [46 совет # ASP.NET MVC — не использовать ссылки, удалить, так как они создают бреши в системе безопасности](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

Метод `HttpPost`, который удаляет данные, называется `DeleteConfirmed`, поэтому метод HTTP POST обладает уникальной сигнатурой или именем. Ниже приведены сигнатуры двух методов:

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample3.cs)]

Среда CLR (CLR) требует перегруженные методы имели уникальную сигнатуру (тем же именем, другой список параметров). Однако здесь необходимы два метода Delete — один для GET--и один для POST, оба из которых требуют такой же сигнатурой. (Они оба должны принимать целочисленное значение в качестве параметра.)

Чтобы отсортировать этот выходной, можно сделать несколько вещей. Одна — предоставить методы разные имена. Это именно это было сделано он в предыдущем примере. Однако в этом случае возникает небольшая проблема: ASP.NET сопоставляет сегменты URL-адреса с методами действий по имени, а при переименовании метода, как правило, маршрутизация не сможет найти этот метод. Решение показано в примере, а именно: в метод `DeleteConfirmed` следует добавить атрибут `ActionName("Delete")`. Это фактически выполняет сопоставление для системы маршрутизации, таким образом, чтобы URL-адрес, который включает в себя <em>/Delete/</em>для отправки запроса будет найти `DeleteConfirmed` метод.

Другой способ устранения неполадки методов с одинаковыми именами и сигнатурами является искусственное изменение сигнатуры метода POST для включения неиспользуемый параметр. Например, некоторые разработчики добавить тип параметра `FormCollection` , передаваемый в метод POST и затем просто не использовать параметр:

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="wrapping-up"></a>Подводя итоги

Теперь у вас есть полное приложение ASP.NET MVC, которое хранит данные в базу данных SQL Server Compact. Можно создать, чтение, обновление, удаление и поиск фильмов.

![](improving-the-details-and-delete-methods/_static/image1.png)

Этот основам вас к работе, делая контроллеров, связав их с представлениями и передача вокруг жестко заданные данные. Затем вы создали и разработана модель данных. Entity Framework Code First создали базу данных из модели данных на лету, и система формирования шаблонов ASP.NET MVC автоматически создается действие методов и представлений для основных операций CRUD. Затем вы добавили форму поиска, которые позволяют пользователям найти в базе данных. Можно изменить базу данных будет содержать новый столбец данных, а затем обновлен две страницы для создания и отображения новых данных. Вы добавили проверки, помечая модели данных с атрибутами из `DataAnnotations` пространства имен. Итоговый проверки выполняется на клиенте и на сервере.

Если вы хотите развернуть приложение, полезно для предварительного тестирования приложения на локальном сервере IIS 7. Это можно использовать [установщика веб-платформы](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=ASPNET;) ссылку, чтобы включить параметр IIS для приложений ASP.NET. Перейдите по следующим ссылкам развертывания:

- [Карта содержимого развертывания ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx)
- [Включение IIS 7.x](https://blogs.msdn.com/b/rickandy/archive/2011/03/14/enabling-iis-7-x-on-windows-7-vista-sp1-windows-2008-windows-2008-r2.aspx)
- [Проекты развертывания веб-приложений](https://msdn.microsoft.com/library/dd394698.aspx)

Я теперь рекомендуем перейти к нашей промежуточного уровня [Создание модели данных Entity Framework для приложения ASP.NET MVC](../../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) и [MVC Music Store](../../mvc-music-store/mvc-music-store-part-1.md) учебники, чтобы изучить [ASP.NET статьи на сайте MSDN](https://msdn.microsoft.com/library/gg416514(VS.98).aspx)и ознакомьтесь с множеством видеоролики и ресурсы на [ https://asp.net/mvc ](https://asp.net/mvc) Чтобы получить дополнительные сведения об ASP.NET MVC! [Форумах ASP.NET MVC](https://forums.asp.net/1146.aspx) — отличное место для вопросов.

Желаем удачи!

— Скотт Хансельман ([ http://hanselman.com ](http://hanselman.com) и [ @shanselman ](http://twitter.com/shanselman) в Twitter) и Рик Андерсон [blogs.msdn.com/rickAndy](https://blogs.msdn.com/rickAndy)

> [!div class="step-by-step"]
> [Назад](adding-validation-to-the-model.md)
