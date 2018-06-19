---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
title: Добавление модели (C#) | Документы Microsoft
author: Rick-Anderson
description: 'Примечание: Обновленную версию этого учебника доступен здесь, использующий ASP.NET MVC 5 и Visual Studio 2013. Это более безопасный, гораздо проще выполните и демонстрационных...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 42355b95-5f1f-413e-8d16-14cdfaaefcd8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a7f9206ddd43b4a3b4f96240ee48b9414450da22
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879352"
---
<a name="adding-a-model-c"></a>Добавление модели (C#)
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

> Этот учебник поможет узнать основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, которой — это бесплатная версия Microsoft Visual Studio. Прежде чем начать, убедитесь, что вы установили необходимые компоненты, перечисленные ниже. Все из них можно установить, щелкнув по следующей ссылке: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Кроме того можно установить отдельно предварительные требования, используя следующие ссылки:
> 
> - [Необходимые компоненты Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Обновление средств ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(среда выполнения + средства поддержки)
> 
> Если вы используете Visual Studio 2010 вместо Visual Web Developer 2010, необходимо установить необходимые компоненты, щелкнув по следующей ссылке: [необходимых компонентов Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Проект Visual Web Developer с исходного кода C# доступна по следующему адресу. [Загрузка версии C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Если используется Visual Basic, перейдите в [Visual Basic-версии](../vb/adding-a-model.md) этого учебника.


## <a name="adding-a-model"></a>Добавление модели

В этом разделе вы добавите некоторые классы для управления фильмов в базе данных. Эти классы будет «модель» часть приложения ASP.NET MVC.

Мы воспользуемся технология доступа к данным .NET Framework платформа Entity Framework для определения и работы с этими классами модели. Поддерживает платформы Entity Framework (часто обозначается как EF), который называется парадигмы разработки *Code First*. Код сначала позволяет создавать объекты модели путем написания простых классов. (Они также известны как классов POCO, от «plain old CLR объектов».) Затем можно установить базу данных создан автоматически из классов, который позволяет очень простой и быстрой разработки рабочего процесса.

## <a name="adding-model-classes"></a>Добавление классов модели

В **обозревателе решений**, щелкните правой кнопкой мыши *моделей* выберите **добавить**и выберите **класса**.

![](adding-a-model/_static/image1.png)

Имя *класс* «Фильма».

[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)

Добавьте следующие пять свойства `Movie` класса:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Мы будем использовать `Movie` класс для представления фильмов в базе данных. Каждый экземпляр `Movie` объекта будет соответствовать строки в таблицу базы данных и каждое свойство `Movie` класса сопоставляются со столбцом в таблице.

В том же файле добавьте следующий `MovieDBContext` класса:

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

`MovieDBContext` Класс представляет контекст базы данных Entity Framework фильм, который обрабатывает выборка, хранения и обновления `Movie` класса экземпляров в базе данных. `MovieDBContext` Является производным от `DbContext` базовый класс, предоставляемый платформой Entity Framework. Дополнительные сведения о `DbContext` и `DbSet`, в разделе [средств повышения производительности платформы Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).

Чтобы иметь возможность ссылаться на `DbContext` и `DbSet`, необходимо добавить следующие `using` инструкции в верхней части файла:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Полный *Movie.cs* файла показан ниже.

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>Создание строки подключения и работе с SQL Server Compact

`MovieDBContext` Созданный класс обрабатывает задачу подключения к базе данных и сопоставления `Movie` объектов для записи базы данных. Один вопрос, который может возникнуть вопрос, является, как указать базу данных, в которой будут подключаться. Который предстоит выполнить путем добавления сведений о соединении в *Web.config* файл приложения.

Откройте корневой каталог приложения *Web.config* файла. (Не *Web.config* файла в *представления* папки.) На рисунке ниже Показать оба *Web.config* файлов; откройте *Web.config* файл обведен красным цветом.

![](adding-a-model/_static/image4.png)

### 

Добавьте следующую строку подключения для `<connectionStrings>` элемент в *Web.config* файла.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

В следующем примере показано часть *Web.config* файл с добавить новую строку подключения:

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

Этот небольшой объем кода и XML предоставляет все необходимое для записи для представления и хранить в базе данных фильма.

Далее мы создадим новый `MoviesController` класс, который можно использовать для отображения данных фильма и позволяют пользователям создавать новые вхождения фильма.

> [!div class="step-by-step"]
> [Назад](adding-a-view.md)
> [Вперед](accessing-your-models-data-from-a-controller.md)
