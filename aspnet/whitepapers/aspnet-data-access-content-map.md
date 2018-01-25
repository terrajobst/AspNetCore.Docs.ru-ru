---
uid: whitepapers/aspnet-data-access-content-map
title: "Доступ к данным ASP.NET - рекомендуется использовать ресурсы | Документы Microsoft"
author: rick-anderson
description: "В этом разделе приведены ссылки на ресурсы документации о том, как получить доступ к данным в веб-приложениях ASP.NET, главным образом с помощью платформы Entity Framework и SQL Se..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/25/2013
ms.topic: article
ms.assetid: f8157be1-4ab9-469e-ad3a-0ccc80b56c00
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-data-access-content-map
msc.type: content
ms.openlocfilehash: 16364951544dff33c9cdb8c8e330cc93de192c47
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-data-access---recommended-resources"></a>Доступ к данным ASP.NET - рекомендуется использовать ресурсы
====================
> Этот раздел содержит ссылки на ресурсы документации о том, как получить доступ к данным в веб-приложениях ASP.NET, главным образом с помощью платформы Entity Framework и SQL Server.
> 
> Если вы знаете значительные записи блога [stackoverflow](http://stackoverflow.com) потоком, или любую ссылку, может потребоваться, [отправьте сообщение электронной почты](mailto:aspnetue@microsoft.com?subject=Data Access Content Map) со ссылкой.
> 
> Последние обновленные 4/3/2014


В нем содержатся следующие подразделы:

- [Приступая к работе с доступом к данным в ASP.NET](#gettingstarted)
- [С помощью платформы Entity Framework](#ef)

    - [Сначала с помощью кода Entity Framework](#cf)
    - [С помощью Entity Framework Code First Migrations](#efcfmigrations)
    - [Сначала с помощью базы данных Entity Framework или модели первый (конструкторе EF)](#efdbf)
    - [Загрузка связанных данных в Entity Framework (отложенную загрузку, активная загрузка и явной загрузки)](#efrelateddata)
    - [Оптимизация производительности Entity Framework](#optimizingef)
    - [Обработка параллелизма в приложения Entity Framework](#efconcurrency)
    - [Книги о платформе Entity Framework](#efbooks)
    - [Дополнительные ресурсы Entity Framework](#otherefresources)
- [Привязка данных в ASP.NET Web Forms приложений](#wfdatabinding)

    - [Использование веб-форм привязки модели](#wfmodelbinding)
    - [Использование веб-форм источников данных](#wfdsc)
    - [Использование веб-форм с привязкой к данным элементы управления и выражения привязки данных](#wfdbc)
- [Работа с базами данных SQL Server](#sqlserver)

    - [Работа с базами данных SQL Server Express LocalDB](#sslocaldb)
    - [Работа с базами данных SQL Server Express](#sse)
    - [Работа с базой данных Azure SQL для Windows](#ssdb)
    - [Выбор между SQL Server и базы данных Azure SQL для Windows](#ssdbchoosing)
- [Работа с системами управления базами данных NoSQL](#nosql)
- [С помощью запросов LINQ в приложениях ASP.NET](#linq)
- [С помощью формирования шаблонов платформы динамических данных](#dd)
- [Защита доступа к данным](#securing)
- [Оптимизация производительности доступа к данным](#optimizingdataaccess)
- [Развертывание базы данных](#deploying)
- [Доступ к данным через веб-службы](#webservice)
- [Дополнительные ресурсы](#additional)

<a id="gettingstarted"></a>

## <a name="getting-started-with-data-access-in-aspnet"></a>Приступая к работе с доступом к данным в ASP.NET

- [Параметры хранения данных (Создание реальных облачных приложений в Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md). Глава электронная книга по разработке для облака. Вводит баз данных NoSQL в качестве альтернативы, многие разработчики, знакомые с реляционными базами данных, как правило, забывают. Представлены рекомендации на то, что нужно принять во внимание при выборе реляционных NoSQL или выбор конкретной платформы.
- [Параметры доступа к данным ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx) (MSDN). Введение в руководство о том, как выбрать платформы и получить доступ к методам, которые подходят для вашего сценария и параметры доступа к данным для реляционных баз данных для ASP.NET.
- [Реляционная база данных](http://en.wikipedia.org/wiki/Relational_database). Википедия). Если вы не работали с реляционными базами данных, см. здесь введение в терминах реляционных баз данных и основные понятия. Введение в SQL Server в частности в разделе [работа с базами данных SQL Server](#sqlserver) далее в этом разделе.

<a id="ef"></a>

## <a name="using-the-entity-framework"></a>С помощью платформы Entity Framework

- [Принципы разработки с использованием Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf) (MSDN). Рекомендации по выбору Entity Framework подход к разработке первой базы данных Model First или Code First.

<a id="cf"></a>

### <a name="using-entity-framework-code-first"></a>Сначала с помощью кода Entity Framework
  

Следующие учебники предлагают загружаемые примеры приложений:

- [Приступая к работе с EF 6 MVC 5 с помощью](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Охватывает широкий спектр Entity Framework Code First сценариев, включая миграция и EF 6 такие функции, устойчивость подключений, команда перехвата и async. Это обновленная версия [EF 5 и MVC 4 ряда](../mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). В более ранних цикл включено учебник в репозиторий и шаблонов единицу работы, которые не включается в новый ряд.
- [Введение в ASP.NET MVC 5](../mvc/overview/getting-started/introduction/getting-started.md). Узкий диапазон Entity Framework кода первого сценарии рассматриваются в отличие от задание более подробное введение в MVC функции.
- [Привязка модели и веб-форм](https://go.microsoft.com/fwlink/?LinkId=286117). Использует Code First в приложении Web Forms.
- [Приступая к работе с 4,5 веб-форм ASP.NET](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Введение в веб-форм с некоторыми покрытием кода первым. Использование привязки модели.
- [MVC Music Store](../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1.md). Использует Code First в приложении MVC 3 электронной коммерции, который также реализует членства и авторизации. Версии MVC и система членства ASP.NET (проверка подлинности и авторизация) использовать здесь устарели; Дополнительные последние сведения о членстве ASP.NET см. в разделе [https://asp.net/identity](https://asp.net/identity).

Другие ресурсы:

- [Entity Framework — код сначала для существующей базы данных](https://msdn.microsoft.com/data/jj200620). MSDN. Видео и пошаговое руководство, которое показывает, как использовать Code First с существующей базы данных.
- [Центр разработчиков данных — Entity Framework](https://msdn.microsoft.com/data/ef). MSDN. Руководство по документации по Entity Framework, создается и обслуживается команда Entity Framework см. в разделе [начать](https://msdn.microsoft.com/data/ee712907) ссылку.

См. также [книг о платформе Entity Framework](#efbooks) и [дополнительные ресурсы Entity Framework](#otherefresources) далее в этом разделе.

<a id="efcfmigrations"></a>

### <a name="using-entity-framework-code-first-migrations"></a>С помощью Entity Framework Code First Migrations
  

Большинство учебников Code First, перечисленных выше титульных миграции. Обратитесь к следующим ресурсам.

- [ASP.NET веб-развертывания с помощью Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 2 учебника выпусков, показано, как использовать Code First Migrations для развертывания базы данных.
- [Развертывание приложения безопасного ASP.NET MVC 5 с членством, OAuth и базы данных SQL на Windows Azure веб-сайт](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Microsoft Azure). Как использовать миграции для развертывания данных членства и приложения Azure.
- [Обзор развертывания веб-для Visual Studio и ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment). . В разделе **Настройка развертывания базы данных в Visual Studio** раздел Описание интеграции Code First Migrations в возможности развертывания Visual Studio web.
- [Центр разработчиков данных - Code First Migrations](https://msdn.microsoft.com/data/jj591621) (MSDN). Документация по миграции команды Entity Framework.
- [Миграция серию](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx). Блог EF). Три видео на дополнительные разделы в Code First Migrations.
- [Code First Migrations с узлами веб-страниц ASP.NET](http://www.mikesdotnetting.com/Article/217/Code-First-Migrations-With-ASP.NET-Web-Pages-Sites). Блог Mikesdotnetting). Показано, как использовать Code First миграции с узлом веб-страниц ASP.NET, помещая контекст данных в проекте библиотеки классов Visual Studio.

<a id="efdbf"></a>

### <a name="using-entity-framework-database-first-or-model-first-the-ef-designer"></a>Сначала с помощью базы данных Entity Framework или модели первый (конструкторе EF)

- [Приступая к работе с Entity Framework 6 Database First MVC 5 с помощью](../mvc/overview/getting-started/database-first-development/setting-up-database.md). Выполнение скрипта в обозревателе серверов, чтобы создать базу данных и затем с помощью конструктора Entity Framework для создания модели данных. Показано, как создавать простые CRUD веб-страниц и для других функций, вы можете воспользоваться одним Code First учебников, так как все рабочие процессы EF используется один API DbContext обработки данных.

Следующие ресурсы являются более старыми. Они полезны в том случае, если требуется использовать Entity Framework версии 4.0, и вы хотите использовать элемент управления источником данных для привязки данных в приложении веб-форм.

- [Приступая к работе с платформой Entity Framework 4.0](../web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). Показано, как использовать **EntityDataSource** элемента управления.
- [Продолжение с платформой Entity Framework](../web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)(показано, как использовать **ObjectDataSource** элемента управления. Содержит руководство по обработки параллелизма, руководство по производительности EF и учебник по новые возможности EF 4.0.

<a id="efrelateddata"></a>

### <a name="handling-related-data-in-entity-framework-lazy-loading-eager-loading-and-explicit-loading"></a>Обработка связанных данных в Entity Framework (отложенную загрузку, активная загрузка и явной загрузки)

- [Чтение данных, связанных с платформой Entity Framework в приложении ASP.NET MVC](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md). Во-первых, кода образца приложения MVC. Методов, показанных применимы и для привязки модели веб-форм и первой базы данных рабочего процесса.
- [Центр разработчиков данных — загрузка связанных сущностей](https://msdn.microsoft.com/data/jj574232) (MSDN). Команды Entity Framework документацию по загрузке связанных данных.

<a id="optimizingef"></a>

### <a name="optimizing-entity-framework-performance"></a>Оптимизация производительности платформы Entity Framework

- [Сложные сценарии, Entity Framework для приложений ASP.NET](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Показывает, как вызывать хранимые процедуры или выполнить инструкцию SQL, как отключить обнаружение изменений и отключить проверку при сохранении изменений.
- [Вопросы производительности платформы Entity Framework 5](https://msdn.microsoft.com/data/hh949853) (MSDN).
- [Вопросы производительности (Entity Framework)](https://msdn.microsoft.com/library/cc853327) (MSDN).
- [Максимальную производительность с платформой Entity Framework в веб-приложение ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md). Применяется к Entity Framework 4.0.
- См. также [доступа к данным ASP.NET оптимизация](#optimizingdataaccess) далее в этом разделе.

<a id="efconcurrency"></a>

### <a name="handling-concurrency-in-an-entity-framework-application"></a>Обработка параллелизма в приложения Entity Framework

- [Обработка параллелизма с платформой Entity Framework в приложении ASP.NET MVC](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md). Во-первых, код DbContext API, с помощью образца приложения MVC.
- [Центр разработчиков данных — шаблоны оптимистичного параллелизма](https://msdn.microsoft.cus/data/jj592904) (MSDN). Документация параллелизма команды Entity Framework.
- [Обработка параллелизма с платформой Entity Framework в веб-приложение ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md). Применяется к Entity Framework 4.0. Базы данных сначала ObjectContext API, с помощью примера приложения веб-форм.

<a id="efrepository"></a><a id="efbooks"></a>

### <a name="books-about-the-entity-framework"></a>Книги о платформе Entity Framework

- [Программирование Entity Framework: DbContext](http://shop.oreilly.com/product/0636920022237.do) Джули Лерман и Миллер Роуэном.
- [Программирование Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) Джули Лерман и Миллер Роуэном.

Этих книгах актуальны текущего следующие рекомендуемые методы. Они предоставляют более полный, но для понимания введение в Entity Framework чем те, которые доступны в Интернете. Другой книги [программирования платформы Entity Framework](http://shop.oreilly.com/product/9780596807252.do) с Джули Лерман крупных и более полное, но он является более ранней, и многие из методик, она охватывает больше не рекомендуется для использования платформы Entity Framework. См. также: список книг, рекомендуется группой Entity Framework в [Data Developer Center - электронной](https://msdn.microsoft.com/data/aa937716) на сайте MSDN.

<a id="otherefresources"></a>

### <a name="other-entity-framework-resources"></a>Другие ресурсы Entity Framework

- [Блог команды Entity Framework (ADO.NET)](https://blogs.msdn.com/b/adonet/). Одно из наиболее подходящих ресурсов для самые последние сведения и объявления о новых усовершенствований. Другие блоги, связанные с EF в разделе блоги на [Приступая к работе с Entity Framework](https://msdn.microsoft.com/data/ee712907).
- [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx). В разделе **точек данных** столбец, который часто является о разделы, относящиеся к платформе Entity Framework.

<a id="wfdatabinding"></a>

## <a name="data-binding-in-aspnet-web-forms-applications"></a>Привязка данных в ASP.NET Web Forms приложений

- [Веб-форм ASP.NET параметры доступа к данным](https://msdn.microsoft.com/library/jj822927.aspx) (MSDN)<a id="wfmodelbinding"></a>.

<a id="wfmodelbinding"></a>

### <a name="using-web-forms-model-binding"></a>Использование веб-форм привязки модели

- [Привязка модели и веб-форм](https://go.microsoft.com/fwlink/?LinkId=286117). С помощью EF Code First учебника рядов.
- [Web Forms модель привязки, часть 1: Выбор данных](https://weblogs.asp.net/scottgu/archive/2011/09/06/web-forms-model-binding-part-1-selecting-data-asp-net-vnext-series.aspx) (блог Скотта Гатри). В этих более старые записи в блогах свойство с именем ItemType назывался ModelType, но в противном случае сведения, содержащиеся в них является допустимым.
- [Web Forms модель привязки, часть 2: Фильтрация данных](https://weblogs.asp.net/scottgu/archive/2011/09/12/web-forms-model-binding-part-2-filtering-data-asp-net-vnext-series.aspx) (блог Скотта Гатри).
- [Web Forms модель привязки часть 3: Обновление и проверка](https://weblogs.asp.net/scottgu/archive/2011/10/30/web-forms-model-binding-part-3-updating-and-validation-asp-net-4-5-series.aspx) (блог Скотта Гатри).
- [Привязки модели ASP.NET 4.5 Web Forms](../web-forms/videos/aspnet-web-forms-vnext/aspnet-45-web-forms-model-binding.md). (видео).
- [Модель привязки, часть 1 — при выборе данных](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-1-selecting-data.md) (видео).
- [Модель привязки часть 2 - фильтрация](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-2-filtering.md) (видео).
- [Приступая к работе с ASP.NET 4.5 веб-формы - отображения данных элементов и сведения о](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).

<a id="wfdsc"></a>

### <a name="using-web-forms-data-source-controls"></a>Использование веб-форм источников данных

- [Данные источника веб-сервера управления](https://msdn.microsoft.com/library/ms247258.aspx) (MSDN).
- [Объявляет о выпуске поставщика платформы динамических данных и управления EntityDataSource для Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/02/28/announcing-the-release-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx) (блог веб-разработки Майкрософт).

<a id="wfdbc"></a>

### <a name="using-web-forms-data-bound-controls-and-data-binding-expressions"></a>Использование веб-форм с привязкой к данным элементы управления и выражения привязки данных

- [Привязка модели и веб-форм](https://go.microsoft.com/fwlink/?LinkId=286117). Последовательность учебник, использующий EF Code First.
- [Приступая к работе с ASP.NET 4.5 веб-формы - отображения данных элементов и сведения о](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).
- [Строго типизированные элементы управления данными](https://weblogs.asp.net/scottgu/archive/2011/09/02/strongly-typed-data-controls-asp-net-vnext-series.aspx) (блог Скотта Гатри).
- [Строго типизированные элементы управления данными](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (видео).
- [ASP.NET 4.5 строгих типизированных данных элементов управления Web Forms](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (видео).
- [Элемент управления привязки данных](https://msdn.microsoft.com/library/ms228214.aspx) (MSDN).
- [Общие сведения о выражениях привязки данных](https://msdn.microsoft.com/library/ms178366.aspx) (MSDN). Это страница охватывает только **Eval** и **привязки**; она не была обновлена для включения **элемент** и **BindItem**.

<a id="sqlserver"></a>

## <a name="working-with-sql-server-databases"></a>Работа с базами данных SQL Server

- [Функции базы данных SQL Server](https://msdn.microsoft.com/library/hh230827.aspx) (MSDN). Общие сведения о разнообразных разделы по SQL Server см. в разделе записи под этим параметром в Содержании.
- [Выпуски SQL Server](https://msdn.microsoft.com/library/ms178359.aspx#sqlserver) (MSDN). Сводка Доступные выпуски SQL Server со ссылками на дополнительные сведения о каждой из них.)
- [Строки подключения SQL Server для веб-приложений ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [С помощью SQL Server Compact для веб-приложений ASP.NET](https://msdn.microsoft.com/library/ms247257.aspx) (MSDN).
- [Microsoft SQL Server: Образцы продуктов, базы данных](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Образец базы данных AdventureWorks.
- [Установка образцов баз данных](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). В дополнение к методам, показанный здесь, можно также загрузить один из образца MDF-файлы в приложение\_папки данных веб-проекта преобразования базы данных в LocalDB и создать строку подключения LocalDB. Сведения о том, как это сделать, см. в разделе [как: обновление до LocalDB](https://msdn.microsoft.com/library/hh873188.aspx).

Также в следующих разделах по работе с SQL Server Express и LocalDB, выбор между SQL Server и базы данных SQL.

<a id="sslocaldb"></a>

### <a name="working-with-sql-server-express-localdb-databases"></a>Работа с базами данных SQL Server Express LocalDB

- [SQL Server Express 2012 LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (MSDN). Официальный MSDN введение в LocalDB.
- [Строки подключения SQL Server для веб-приложений ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Как: обновление до LocalDB](https://msdn.microsoft.com/library/hh873188.aspx) (MSDN). Процесс миграции MDF-файла из более ранней версии SQL Server Express LocalDB. Также необходимо пройти этот процесс, при загрузке одного из [образцы баз данных SQL Server 2012](https://go.microsoft.com/fwlink/?linkid=117483).
- [Общие сведения о LocalDB, улучшенная SQL Express](https://go.microsoft.com/fwlink/?LinkId=234375) (блог по SQL Server Express). Содержит дополнительные сведения о причины создания LocalDB не включается в библиотеке MSDN.
- [LocalDB: Где мои базы данных?](https://go.microsoft.com/fwlink/?LinkId=234376) (Блог по SQL Server Express). Сведения о котором создаются файлы базы данных LocalDB.
- [С помощью LocalDB с полнофункциональными службами IIS, часть 1: профиль пользователя](https://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-1-user-profile.aspx) (блог по SQL Server Express). LocalDB не предназначен для работы со службами IIS. Данная последовательность записи в блогах описание проблемы и некоторые способы их устранения.

<a id="sse"></a>

### <a name="working-with-sql-server-express-databases"></a>Работа с базами данных SQL Server Express

- [Строки подключения SQL Server для веб-приложений ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN). При использовании параметра строки подключения AttachDBFileName с SQL Server Express см особенно пользовательский экземпляр части этой страницы.
- [Как стать владельцем вашего локального SQL Server Express 2008](https://blogs.msdn.com/b/sqlexpress/archive/2010/02/23/how-to-take-ownership-of-your-local-sql-server-2008-express.aspx) (блог по SQL Server Express). Распространенной проблемой не могут работать с базами данных SQL Server Express, поскольку вы не являетесь администратором на экземпляре SQL Server Express. По умолчанию только пользователь, выполнивший установил экспресс-выпуск SQL Server является администратором. Этот блог объясняется, как сделать самостоятельно администратором SQL Server Express, если вы являетесь администратором на компьютере.
- [Мое веб-приложение ASP.NET использовать базу данных SQL Server Express в рабочей среде](https://msdn.microsoft.com/library/jj653753.aspx#sql_express_in_production) (MSDN).

<a id="ssdb"></a>

### <a name="working-with-windows-azure-sql-database"></a>Работа с базой данных Azure SQL для Windows

- [Развертывание приложения ASP.NET MVC защиты с членством, OAuth и базы данных SQL для веб-сайта Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data) (сайт Microsoft Azure).
- [Базы данных SQL](https://docs.microsoft.com/azure/sql-database/) (сайт Microsoft Azure). Начало работы учебников и пошаговых руководств.
- [База данных Azure SQL Windows](https://msdn.microsoft.com/library/windowsazure/ee336279.aspx) (MSDN). Узел верхнего уровня оглавление для базы данных SQL в библиотеке MSDN.
- [Windows Azure SQL базы данных вики-сайте TechNet статей индекс](https://social.technet.microsoft.com/wiki/contents/articles/2267.windows-azure-sql-database-technet-wiki-articles-index-en-us.aspx) (веб-сайте Microsoft TechNet).
- [Блок приложения обработки временных сбоев](https://msdn.microsoft.com/library/hh680934(v=PandP.50).aspx). Платформа, которая позволяет обрабатывать сети временных сбоев и ошибок подключения, возникших в результате регулирования. Доступные в пакете NuGet: [библиотеки Enterprise Library 5.0 — блок приложения обработки для временных сбоев](http://nuget.org/packages/EnterpriseLibrary.WindowsAzure.TransientFaultHandling).
- [Приступая к работе с базой данных SQL и Entity Framework](https://msdn.microsoft.com/data/jj556244) (MSDN).
- [Windows Azure Training Kit](https://www.microsoft.com/download/details.aspx?id=8396) (центра загрузки Майкрософт). Включает практические занятия для базы данных SQL.
- [Форум сообщества базы данных Azure SQL Windows](https://social.msdn.microsoft.com/Forums/ssdsgetstarted/threads).
- [Перемещение базы данных Azure SQL Windows](https://msdn.microsoft.com/library/ff803375.aspx) (MSDN). Один раздел полный сценарий конца в конец группой шаблонов и методов Майкрософт. Обложки, почему может потребоваться переместить и миграции с SQL Server к базе данных SQL.
- [Миграция баз данных SQL Server к базе данных Azure SQL Windows](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).
- [Мастер миграции баз данных SQL](http://sqlazuremw.codeplex.com/). Средство с открытым исходным для миграции базы данных и из базы данных SQL.

<a id="ssdbchoosing"></a>

### <a name="choosing-between-sql-server-and-windows-azure-sql-database"></a>Выбор между SQL Server и базы данных Azure SQL для Windows

- [Сравнение SQL Server с базой данных SQL Windows Azure](https://social.technet.microsoft.com/wiki/contents/articles/996.compare-sql-server-with-windows-azure-sql-database-en-us.aspx) (веб-сайте Microsoft TechNet).
- [Перенос данных в базе данных Azure SQL Windows: средства и методы](https://msdn.microsoft.com/library/windowsazure/hh694043.aspx) (MSDN). Содержит разделы, которые сравнивают SQL Server к базе данных SQL и предоставить рекомендации о том, когда следует выполнять перенос из SQL Server к базе данных SQL.
- [Руководство по доставке базы данных SQL Windows Azure](https://social.technet.microsoft.com/wiki/contents/articles/3398.windows-azure-sql-database-delivery-guide-en-us.aspx) (веб-сайте Microsoft TechNet).
- [Ограничения функций SQL Server (база данных Azure SQL для Windows)](https://msdn.microsoft.com/library/windowsazure/ff394115.aspx) (MSDN).
- [Windows табличного хранилища Azure и база данных Azure SQL — сходство и отличия](https://msdn.microsoft.com/library/jj553018.aspx) (MSDN). Для приложения, развертывание в Windows Azure хранилище таблиц Windows Azure может быть вместо базы данных SQL Windows Azure. Этот раздел поможет выбрать между этими вариантами.
- [База данных Azure SQL Windows](https://msdn.microsoft.com/library/windowsazure/ee336279) (MSDN).
- [Рекомендации и ограничения (база данных Azure SQL для Windows)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)

<a id="nosql"></a>

## <a name="working-with-nosql-database-management-systems"></a>Работа с системами управления базами данных NoSQL

- [Данные служб Windows Azure](https://www.windowsazure.com/develop/net/data/) (сайт Microsoft Azure). В разделе [руководство по функциям службы таблиц](https://docs.microsoft.com/azure/) и **больших данных** части страницы.
- [ASP.NET многоуровневые приложения с помощью таблиц хранилища, очередей и больших двоичных объектов](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (сайт Microsoft Azure). End-to-end учебника с загружаемый пример приложения, использующего таблиц NoSQL хранилища Windows Azure.

<a id="linq"></a>

## <a name="using-linq-queries-in-aspnet-applications"></a>С помощью запросов LINQ в приложениях ASP.NET

- [Параметры доступа к данным ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx#linq) (MSDN). Содержит введение в LINQ.
- [Обучающие видеоматериалы по службам LINQ](http://www.misfitgeek.com/windows-client-linq-training-videos-20/) (блог Джо Стэгнер).
- [Форум по ASP.NET поток со ссылками на динамические ресурсы LINQ](https://forums.asp.net/p/1961037/5601994.aspx?Please+suggest+good+books+so+that+one+can+write+and+understand+dynamic+linq).

<a id="dd"></a>

## <a name="using-dynamic-data-scaffolding"></a>С помощью формирования шаблонов платформы динамических данных

- [Шаблоны проектов платформы динамических данных](https://msdn.microsoft.com/library/jj822927.aspx#dynamicdata) (MSDN). Рекомендации по использованию динамических данных проектов.
- [Платформа динамических данных ASP.NET](https://msdn.microsoft.com/library/ee845452.aspx) (MSDN).

<a id="securing"></a>

## <a name="securing-data-access"></a>Защита доступа к данным

- [Защита доступа к данным в ASP.NET](https://msdn.microsoft.com/library/ms178375.aspx) (MSDN).
- [Вопросы безопасности (Entity Framework)](https://msdn.microsoft.com/library/cc716760.aspx) (MSDN).
- [Практическое руководство: Обезопасить строки соединения при использовании элементов управления источниками данных](https://msdn.microsoft.com/library/ms178372.aspx) (MSDN).

<a id="optimizingdataaccess"></a>

## <a name="optimizing-data-access-performance"></a>Оптимизация производительности доступа к данным

- [Общие сведения о производительности ASP.NET](https://msdn.microsoft.com/library/cc668225.aspx) (MSDN).
- [Кэширование ASP.NET](https://msdn.microsoft.com/library/xsbfdd8c.aspx) (MSDN).
- [Повышение производительности ASP.NET](https://msdn.microsoft.com/library/ff647787) (MSDN). Имеется предупреждение «Удалено содержимое» в верхней части этой страницы, но большая часть информации по-прежнему применимо и нет не сравним обновленный ресурс.
- [Повышение производительности SQL Server](https://msdn.microsoft.com/library/ff647793) (MSDN). Комментарий же, как предыдущая ссылка.

См. также [Entity Framework оптимизация производительности](#optimizingef) ранее в этом разделе.

<a id="deploying"></a>

## <a name="deploying-a-database"></a>Развертывание базы данных

- [Развертывание веб-ASP.NET — рекомендуется использовать ресурсы](aspnet-web-deployment-content-map.md) (MSDN).

<a id="webservice"></a>

## <a name="accessing-data-through-a-web-service"></a>Доступ к данным через веб-службы

- [Доступ к данным через веб-службу](https://msdn.microsoft.com/library/ms178359.aspx#webservice) (MSDN). Рекомендации по использованию веб-API и WCF.
- [Приступая к работе с веб-API ASP.NET](../web-api/index.md).
- [Службы данных WCF](https://msdn.microsoft.com/data/bb931106) (MSDN).

<a id="additional"></a>

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Доступ к данным ASP.NET часто задаваемые вопросы о](https://msdn.microsoft.com/library/jj653753.aspx) (MSDN).
- [Веб-форм ASP.NET учебники - данных](../web-forms/overview/data-access/index.md). Большинство этих учебников, относительно старых; Убедитесь, что вы чтение [параметры доступа к данным ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx) и [параметров хранилища данных (облака реальных построение приложений с помощью Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md) первой, чтобы не получать слишком далеко в способ доступа к данным, который неправильно для вашего сценария.
- [Карта содержимого для ASP.NET MVC](../mvc/overview/getting-started/recommended-resources-for-mvc.md).
- [Веб-страницы ASP.NET учебники - данных](../web-pages/overview/data/index.md).
- [Доступ к данным в Visual Studio](https://msdn.microsoft.com/library/wzabh8c4.aspx) (MSDN). Предоставляет список ссылок, аналогичный эта карта содержимого, но действующих в Visual Studio, а не ASP.NET.
