---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
title: Добавление модели (C#) | Документация Майкрософт
author: Rick-Anderson
description: 'Примечание: Обновленную версию этого учебника доступен здесь, использующий ASP.NET MVC 5 и Visual Studio 2013. Это более безопасное и гораздо проще выполнить и демонстрационных версий...'
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 42355b95-5f1f-413e-8d16-14cdfaaefcd8
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: f35e1fec7b3b2a1fc53cf8beb3781a2e2f6c8740
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577130"
---
<a name="adding-a-model-c"></a>Добавление модели (C#)
====================
по [Рик Андерсон]((https://twitter.com/RickAndMSFT))

> Этом учебнике описываются основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, которой является бесплатной версии Microsoft Visual Studio. Перед началом работы убедитесь, что вы установили необходимые компоненты, перечисленные ниже. Все из них можно установить, щелкнув следующую ссылку: [установщика веб-платформы](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Кроме того можно установить по отдельности необходимых компонентов, с помощью следующих ссылок:
> 
> - [Необходимые компоненты для Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Обновление средств ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(среды выполнения и средства поддержки)
> 
> Если вы используете Visual Studio 2010 вместо Visual Web Developer 2010, необходимо установить необходимые компоненты, щелкнув следующую ссылку: [необходимых компонентов Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Проект Visual Web Developer с исходным кодом C# — прилагаются в этом разделе. [Загрузить версию C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Если вы предпочитаете Visual Basic, переключитесь в [версии Visual Basic](../vb/adding-a-model.md) работы с этим руководством.


## <a name="adding-a-model"></a>Добавление модели

В этом разделе вы добавите некоторые классы для управления фильмами в базе данных. Эти классы будут «модель» часть приложения ASP.NET MVC.

Вы используете технологии доступа к данным .NET Framework, известный как Entity Framework для определения и работы с этими классами модели. Поддерживает Entity Framework (часто обозначается как EF), называется парадигмы разработки *Code First*. Во-первых, код позволяет создавать объекты модели путем написания простых классов. (Они также называются классы POCO, от «plain old CLR объекты».) Затем можно создавать базы данных, созданной в режиме реального времени из классов, который позволяет очень простой и быстрой разработки рабочего процесса.

## <a name="adding-model-classes"></a>Добавление классов модели

В **обозревателе решений**, щелкните правой кнопкой мыши *моделей* папку, выберите **добавить**, а затем выберите **класс**.

![](adding-a-model/_static/image1.png)

Имя *класс* «Фильма».

[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)

Добавьте следующие пять свойства `Movie` класса:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Мы будем использовать `Movie` класс для представления фильмов в базе данных. Каждый экземпляр `Movie` объекта будет соответствовать ряду в таблице базы данных, а каждое свойство `Movie` класс сопоставляется со столбцом в таблице.

В этом же файле добавьте следующий `MovieDBContext` класса:

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

`MovieDBContext` Класс представляет контекст базы данных movie Entity Framework, который обрабатывает получение, хранения и обновления `Movie` экземпляров в базе данных класса. `MovieDBContext` Является производным от `DbContext` базовый класс, предоставляемый платформой Entity Framework. Дополнительные сведения о `DbContext` и `DbSet`, см. в разделе [средств повышения производительности платформы Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).

Чтобы иметь возможность ссылаться на `DbContext` и `DbSet`, необходимо добавить следующие `using` инструкция в верхней части файла:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Полный *Movie.cs* файла приведен ниже.

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>Создание строки подключения и работы с SQL Server Compact

`MovieDBContext` Созданный класс обрабатывает задачи подключения к базе данных и сопоставления `Movie` объектов для записи базы данных. Один вопрос, на который вы можете спросить, однако, как указать базу данных, которая подключается к. Это можно сделать путем добавления сведений о соединении в *Web.config* файл приложения.

Откройте корневой каталог приложения *Web.config* файл. (Не *Web.config* файл *представления* папки.) На следующем рисунке показано, как *Web.config* файлы; открыть *Web.config* файл помеченные красными кружками.

![](adding-a-model/_static/image4.png)

### 

Добавьте следующую строку подключения для `<connectionStrings>` элемент в *Web.config* файл.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

В следующем примере показано часть *Web.config* файл с добавить новую строку подключения:

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

Этот небольшой объем кода и XML предоставляет все необходимое для записи для представления и сохранения данных фильмов в базе данных.

Далее предстоит создать новый `MoviesController` класс, который можно использовать для отображения данных фильма и разрешить пользователям создавать новые вхождения фильма.

> [!div class="step-by-step"]
> [Назад](adding-a-view.md)
> [Вперед](accessing-your-models-data-from-a-controller.md)
