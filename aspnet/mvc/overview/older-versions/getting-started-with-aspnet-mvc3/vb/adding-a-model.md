---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: "Добавление модели (VB) | Документы Microsoft"
author: Rick-Anderson
description: "Этот учебник поможет узнать основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, являющийся..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: efc18dd71e29d12dacc6cf84a1d3c7f7e92f520d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-model-vb"></a>Добавление модели (Visual Basic)
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
> Проект Visual Web Developer с VB.NET исходный код доступен по следующему адресу. [Загрузить версию VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Если вы предпочитаете C#, переключитесь в [C# версии](../cs/adding-a-model.md) этого учебника.


## <a name="adding-a-model"></a>Добавление модели

В этом разделе вы добавите некоторые классы для управления фильмов в базе данных. Эти классы будет «модель» часть приложения ASP.NET MVC.

Мы воспользуемся технология доступа к данным .NET Framework платформа Entity Framework для определения и работы с этими классами модели. Поддерживает платформы Entity Framework (часто обозначается как EF), который называется парадигмы разработки *Code First*. Код сначала позволяет создавать объекты модели путем написания простых классов. (Они также известны как классов POCO, от «plain old CLR объектов».) Затем можно установить базу данных создан автоматически из классов, который позволяет очень простой и быстрой разработки рабочего процесса.

## <a name="adding-model-classes"></a>Добавление классов модели

В **обозревателе решений**, щелкните правой кнопкой мыши *моделей* выберите **добавить**и выберите **класса**.

![](adding-a-model/_static/image1.png)

Имя класса «Фильма».

Добавьте следующие пять свойства `Movie` класса:

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

Мы будем использовать `Movie` класс для представления фильмов в базе данных. Каждый экземпляр `Movie` объекта будет соответствовать строки в таблицу базы данных и каждое свойство `Movie` класса сопоставляются со столбцом в таблице.

В том же файле добавьте следующий `MovieDBContext` класса:

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

`MovieDBContext` Класс представляет контекст базы данных Entity Framework фильм, который обрабатывает выборка, хранения и обновления `Movie` класса экземпляров в базе данных. `MovieDBContext` Является производным от `DbContext` базовый класс, предоставляемый платформой Entity Framework. Дополнительные сведения о `DbContext` и `DbSet`, в разделе [средств повышения производительности платформы Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).

Чтобы иметь возможность ссылаться на `DbContext` и `DbSet`, необходимо добавить следующие `imports` инструкции в верхней части файла:

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

Полный *Movie.vb* файла показан ниже.

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>Создание строки подключения и работе с SQL Server Compact

`MovieDBContext` Созданный класс обрабатывает задачу подключения к базе данных и сопоставления `Movie` объектов для записи базы данных. Один вопрос, который может возникнуть вопрос, является, как указать базу данных, в которой будут подключаться. Который предстоит выполнить путем добавления сведений о соединении в *Web.config* файл приложения.

Откройте корневой каталог приложения *Web.config* файла. (Не *Web.config* файла в *представления* папки.) На рисунке ниже Показать оба *Web.config* файлов; откройте *Web.config* файл обведен красным цветом.

![](adding-a-model/_static/image2.png)

## 

Добавьте следующую строку подключения для `<connectionStrings>` элемент в *Web.config* файла.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

В следующем примере показано часть *Web.config* файл с добавить новую строку подключения:

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

Этот небольшой объем кода и XML предоставляет все необходимое для записи для представления и хранить в базе данных фильма.

Далее мы создадим новый `MoviesController` класс, который можно использовать для отображения данных фильма и позволяют пользователям создавать новые вхождения фильма.

>[!div class="step-by-step"]
[Назад](adding-a-view.md)
[Вперед](accessing-your-models-data-from-a-controller.md)
