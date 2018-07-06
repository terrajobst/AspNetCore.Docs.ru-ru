---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Добавление модели | Документация Майкрософт
author: Rick-Anderson
description: 'Примечание: Обновленную версию этого учебника доступен здесь, использующий ASP.NET MVC 5 и Visual Studio 2013. Это более безопасное и гораздо проще выполнить и демонстрационных версий...'
ms.author: aspnetcontent
ms.date: 08/28/2012
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: e68b6d41c7880fb849bbefbedd4c4e25c6f47b0e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837763"
---
<a name="adding-a-model"></a>Добавление модели
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Обновленную версию этого учебника доступен [здесь](../../getting-started/introduction/getting-started.md) , использующий ASP.NET MVC 5 и Visual Studio 2013. Она более безопасные, гораздо проще следовать и показаны дополнительные возможности.


В этом разделе вы добавите некоторые классы для управления фильмами в базе данных. Эти классы будут представлять &quot;модели&quot; частью приложения ASP.NET MVC.

Вы используете .NET Framework технологий доступа к данным, известный как [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) определить и работать с этими классами модели. Поддерживает Entity Framework (часто обозначается как EF), называется парадигмы разработки *Code First*. Во-первых, код позволяет создавать объекты модели путем написания простых классов. (Они также называются классами POCO из &quot;обычные старые объекты CLR.&quot;) Затем можно создавать базы данных, созданной в режиме реального времени из классов, который позволяет очень простой и быстрой разработки рабочего процесса.

## <a name="adding-model-classes"></a>Добавление классов модели

В **обозревателе решений**, щелкните правой кнопкой мыши *моделей* папку, выберите **добавить**, а затем выберите **класс**.

![](adding-a-model/_static/image1.png)

Введите *класс* имя &quot;фильма&quot;.

Добавьте следующие пять свойства `Movie` класса:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Мы будем использовать `Movie` класс для представления фильмов в базе данных. Каждый экземпляр `Movie` объекта будет соответствовать ряду в таблице базы данных, а каждое свойство `Movie` класс сопоставляется со столбцом в таблице.

В этом же файле добавьте следующий `MovieDBContext` класса:

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

`MovieDBContext` Класс представляет контекст базы данных movie Entity Framework, который обрабатывает получение, хранения и обновления `Movie` экземпляров в базе данных класса. `MovieDBContext` Является производным от `DbContext` базовый класс, предоставляемый платформой Entity Framework.

Чтобы иметь возможность ссылаться на `DbContext` и `DbSet`, необходимо добавить следующие `using` инструкция в верхней части файла:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Полный *Movie.cs* файла приведен ниже. (Несколько с помощью инструкций, которые не требуется будут удалены.)

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Создание строки подключения и работа с SQL Server LocalDB

`MovieDBContext` Созданный класс обрабатывает задачи подключения к базе данных и сопоставления `Movie` объектов для записи базы данных. Один вопрос, на который вы можете спросить, однако, как указать базу данных, которая подключается к. Это можно сделать путем добавления сведений о соединении в *Web.config* файл приложения.

Откройте корневой каталог приложения *Web.config* файл. (Не *Web.config* файл *представления* папки.) Откройте *Web.config* файл, выделены красным.

![](adding-a-model/_static/image2.png)

Добавьте следующую строку подключения для `<connectionStrings>` элемент в *Web.config* файл.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

В следующем примере показано часть *Web.config* файл с добавить новую строку подключения:

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

Этот небольшой объем кода и XML предоставляет все необходимое для записи для представления и сохранения данных фильмов в базе данных.

Далее предстоит создать новый `MoviesController` класс, который можно использовать для отображения данных фильма и разрешить пользователям создавать новые вхождения фильма.

> [!div class="step-by-step"]
> [Назад](adding-a-view.md)
> [Вперед](accessing-your-models-data-from-a-controller.md)
