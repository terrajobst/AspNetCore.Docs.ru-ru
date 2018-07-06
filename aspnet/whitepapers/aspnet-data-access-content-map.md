---
uid: whitepapers/aspnet-data-access-content-map
title: Доступ к данным ASP.NET — рекомендуемые ресурсы | Документация Майкрософт
author: rick-anderson
description: Этот раздел содержит ссылки на документацию о том, как получить доступ к данным в приложениях ASP.NET, главным образом с помощью Entity Framework и SQL Se...
ms.author: aspnetcontent
ms.date: 07/25/2013
ms.assetid: f8157be1-4ab9-469e-ad3a-0ccc80b56c00
msc.legacyurl: /whitepapers/aspnet-data-access-content-map
msc.type: content
ms.openlocfilehash: fb0cea94d82cc8f59ec56a5445ee84d38325995e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832504"
---
<a name="aspnet-data-access---recommended-resources"></a>Доступ к данным ASP.NET — рекомендуемые ресурсы
====================
> Этот раздел содержит ссылки на документацию о том, как получить доступ к данным в приложениях ASP.NET, главным образом с помощью Entity Framework и SQL Server.
> 
> Если известно, учета, замечательный блог [stackoverflow](http://stackoverflow.com) потока или любую ссылку, которая будет полезна, [отправьте нам сообщение электронной почты](mailto:aspnetue@microsoft.com?subject=Data Access Content Map) со ссылкой.
> 
> Последние обновленные 4/3/2014


В нем содержатся следующие подразделы:

- [Приступая к работе с доступом к данным в ASP.NET](#gettingstarted)
- [С помощью Entity Framework](#ef)

    - [Сначала с помощью кода Entity Framework](#cf)
    - [С помощью Entity Framework Code First Migrations](#efcfmigrations)
    - [Сначала с помощью Entity Framework Database или Model First (конструктор EF)](#efdbf)
    - [Загрузка связанных данных в Entity Framework (отложенной загрузки, упреждающая загрузка и явная загрузка)](#efrelateddata)
    - [Оптимизация производительности Entity Framework](#optimizingef)
    - [Обработка параллелизма в приложении Entity Framework](#efconcurrency)
    - [Книги о Entity Framework](#efbooks)
    - [Дополнительные ресурсы Entity Framework](#otherefresources)
- [Привязка данных в ASP.NET Web Forms приложений](#wfdatabinding)

    - [С помощью веб-форм привязки модели](#wfmodelbinding)
    - [С помощью веб-форм элементов управления источниками данных](#wfdsc)
    - [С помощью веб-форм, элементов управления с привязкой данных и выражения привязки данных](#wfdbc)
- [Работа с базами данных SQL Server](#sqlserver)

    - [Работа с базами данных SQL Server Express LocalDB](#sslocaldb)
    - [Работа с базами данных SQL Server Express](#sse)
    - [Работа с базой данных Azure SQL для Windows](#ssdb)
    - [Выбор между SQL Server и база данных Azure SQL для Windows](#ssdbchoosing)
- [Работа с системами управления базами данных NoSQL](#nosql)
- [С помощью запросов LINQ в приложениях ASP.NET](#linq)
- [С помощью формирования шаблонов платформы динамических данных](#dd)
- [Защита доступа к данным](#securing)
- [Оптимизация скорость доступа к данным](#optimizingdataaccess)
- [Развертывание базы данных](#deploying)
- [Доступ к данным через веб-службы](#webservice)
- [Дополнительные ресурсы](#additional)

<a id="gettingstarted"></a>

## <a name="getting-started-with-data-access-in-aspnet"></a>Приступая к работе с доступом к данным в ASP.NET

- [Варианты хранения данных (Создание реальных облачных приложений с помощью Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md). Глава электронной книги о разработке для облака. Представляет базы данных NoSQL в качестве альтернативы, многие разработчики, знакомые с реляционными базами данных после них обычно опускаются. Представлены рекомендации, о чем надо подумать при выборе реляционной или NoSQL или выбора конкретной платформы.
- [Параметры доступа к данным ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx) (MSDN). Введение в параметры доступа к данным для реляционных баз данных для ASP.NET и рекомендации о том, как выбрать платформы и получить доступ к методам, которые подходят для вашего сценария.
- [Реляционная база данных](http://en.wikipedia.org/wiki/Relational_database). Википедии). Если вы еще не работали с реляционными базами данных, см. на странице Общие сведения о терминологии реляционных баз данных и основные понятия. В частности введение в SQL Server см. в разделе [работы с базами данных SQL Server](#sqlserver) далее в этом разделе.

<a id="ef"></a>

## <a name="using-the-entity-framework"></a>С помощью Entity Framework

- [Принципы разработки с использованием Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf) (MSDN). Рекомендации по выбору Entity Framework разработки подход Database First, Code First или Model First.

<a id="cf"></a>

### <a name="using-entity-framework-code-first"></a>Сначала с помощью кода Entity Framework
  

Следующие учебники предложения загружаемые примеры приложений:

- [Начало работы с EF 6 с помощью MVC 5](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Рассматриваются такие устойчивость подключения, перехват команд и асинхронные функции широкий спектр Entity Framework Code First сценариев, включая миграции и EF 6. Это обновленная версия [EF 5 / MVC 4 серии](../mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). В более ранних цикл включено учебник в репозиторий и шаблоны единицу работы, которые не включены в новой серии.
- [Введение в ASP.NET MVC 5](../mvc/overview/getting-started/introduction/getting-started.md). Описаны уже первого сценария диапазон из кода Entity Framework, но не более полную задания Знакомство с возможностями MVC.
- [Привязка модели и Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Использует Code First в приложении Web Forms.
- [Приступая к работе с ASP.NET 4.5 веб-форм](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Введение в веб-формы с помощью некоторых покрытия Code First. Используется привязка модели.
- [MVC Music Store](../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1.md). Использует Code First в приложении MVC 3 электронной коммерции, который также реализует членство и авторизация. Версия MVC и система членства ASP.NET (проверка подлинности и авторизации) используемый здесь устарели; Дополнительные последние сведения о членстве ASP.NET, см. в разделе [ https://asp.net/identity ](https://asp.net/identity).

Другие ресурсы:

- [Entity Framework — Code First для существующей базы данных](https://msdn.microsoft.com/data/jj200620). MSDN. Видео и пошаговое руководство, которое показывает, как использовать Code First с существующей базы данных.
- [Центр разработчиков данных — Entity Framework](https://msdn.microsoft.com/data/ef). MSDN. Руководство по документации по Entity Framework, который создается и обслуживается команды Entity Framework, см. в разделе [приступить к работе](https://msdn.microsoft.com/data/ee712907) ссылку.

См. также [книг о платформе Entity Framework](#efbooks) и [дополнительные ресурсы Entity Framework](#otherefresources) далее в этом разделе.

<a id="efcfmigrations"></a>

### <a name="using-entity-framework-code-first-migrations"></a>С помощью Entity Framework Code First Migrations
  

Большая часть Code First руководствах, перечисленных выше титульной миграций. Также ознакомьтесь со следующими ресурсами.

- [Веб-развертывание ASP.NET с помощью Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). часть 2 серии руководств, показывающий, как использовать Code First Migrations для развертывания базы данных.
- [Развертывание безопасного приложения ASP.NET MVC 5 с членством, OAuth и базой данных SQL на Windows Azure веб-сайт](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Microsoft Azure). Как использовать миграции для развертывания членства и данных приложения в Azure.
- [Веб-Общие сведения о развертывании для Visual Studio и ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment). См. в разделе **Настройка развертывания базы данных в Visual Studio** разделе Описание интеграции Code First Migrations в функции развертывания веб Visual Studio.
- [Центр разработчиков данных — Code First Migrations](https://msdn.microsoft.com/data/jj591621) (MSDN). Документация по миграции группы Entity Framework.
- [Миграция серию](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx). Блог EF). Три видео на дополнительные разделы в Code First Migrations.
- [Code First Migrations с узлами веб-страниц ASP.NET](http://www.mikesdotnetting.com/Article/217/Code-First-Migrations-With-ASP.NET-Web-Pages-Sites). Блог Mikesdotnetting). В этой статье демонстрируется использование Code First migrations узел веб-страниц ASP.NET, поместив контекст данных в проекте библиотеки классов Visual Studio.

<a id="efdbf"></a>

### <a name="using-entity-framework-database-first-or-model-first-the-ef-designer"></a>Сначала с помощью Entity Framework Database или Model First (конструктор EF)

- [Начало работы с Entity Framework 6 Database First с помощью MVC 5](../mvc/overview/getting-started/database-first-development/setting-up-database.md). Выполнение скрипта в обозревателе серверов, чтобы создать базу данных и затем с помощью конструктора Entity Framework для создания модели данных. Показано, как создание простой CRUD веб-страниц и для других типов функций, вы можете воспользоваться одним из руководств, Code First, так как все рабочие процессы EF используют один и тот же DbContext API обработки данных.

Следующие ресурсы являются более старыми. Они полезны в том случае, если вы хотите использовать версии 4.0 платформы Entity Framework, и вы хотите использовать элемент управления источником данных для привязки данных в приложении Web Forms.

- [Начало работы с Entity Framework 4.0](../web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). Демонстрируется использование **EntityDataSource** элемента управления.
- [Продолжить с Entity Framework](../web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)(показано, как использовать **ObjectDataSource** элемента управления. Содержит руководство по обработки параллелизма, учебник по EF производительности и руководство по новые возможности в EF 4.0.

<a id="efrelateddata"></a>

### <a name="handling-related-data-in-entity-framework-lazy-loading-eager-loading-and-explicit-loading"></a>Обработка связанных данных в Entity Framework (отложенной загрузки, упреждающая загрузка и явная загрузка)

- [Чтение связанных данных с Entity Framework в приложении ASP.NET MVC](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md). Во-первых, код примера приложения MVC. Методы, представленные применяются также для привязки модели веб-форм и Database First рабочего процесса.
- [Центр разработчиков данных — загрузка связанных сущностей](https://msdn.microsoft.com/data/jj574232) (MSDN). Данные, относящиеся к документации группы Entity Framework о загрузке.

<a id="optimizingef"></a>

### <a name="optimizing-entity-framework-performance"></a>Оптимизация производительности платформы Entity Framework

- [Расширенные сценарии Entity Framework для приложений ASP.NET](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Показывает, как на выполнение инструкций SQL или свои собственные хранимые процедуры, как отключить отслеживание изменений и как отключить проверку при сохранении изменений.
- [Рекомендации по ускорению Entity Framework 5](https://msdn.microsoft.com/data/hh949853) (MSDN).
- [Сведения о производительности (Entity Framework)](https://msdn.microsoft.com/library/cc853327) (MSDN).
- [Повышение производительности с помощью Entity Framework в веб-приложения ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md). Применяется к Entity Framework 4.0.
- См. также [доступ к данным ASP.NET, оптимизация](#optimizingdataaccess) далее в этом разделе.

<a id="efconcurrency"></a>

### <a name="handling-concurrency-in-an-entity-framework-application"></a>Обработка параллелизма в приложении Entity Framework

- [Обработка параллелизма с помощью Entity Framework в приложении ASP.NET MVC](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md). Во-первых, код DbContext API, с помощью примера приложения MVC.
- [Центр разработчиков данных — шаблоны оптимистичного параллелизма](https://msdn.microsoft.cus/data/jj592904) (MSDN). Документация параллелизма команда Entity Framework.
- [Обработка параллелизма с помощью Entity Framework в веб-приложения ASP.NET](../web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md). Применяется к Entity Framework 4.0. Базы данных во-первых, API ObjectContext, используя пример приложения веб-форм.

<a id="efrepository"></a><a id="efbooks"></a>

### <a name="books-about-the-entity-framework"></a>Книги о Entity Framework

- [Программирование Entity Framework: DbContext](http://shop.oreilly.com/product/0636920022237.do) , Джули Лерман и Роуэн Миллер.
- [Программирование Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) , Джули Лерман и Роуэн Миллер.

Обе эти книги актуальны с текущей рекомендуемые методы. Они предоставляют более полный, но для понимания введение в Entity Framework чем те, которые доступны в Интернете. Еще одна книга [Programming Entity Framework](http://shop.oreilly.com/product/9780596807252.do) с Джули Лерман крупных и более комплексное, но является более ранней, и многие из методик, он охватывает больше не рекомендуется использовать Entity Framework. См. также список книг, рекомендуется группой Entity Framework в [Центр разработчиков данных — книг](https://msdn.microsoft.com/data/aa937716) на сайте MSDN.

<a id="otherefresources"></a>

### <a name="other-entity-framework-resources"></a>Другие ресурсы Entity Framework

- [Блог группы Entity Framework (ADO.NET)](https://blogs.msdn.com/b/adonet/). Одно из лучших ресурсов самые последние сведения и объявления о новых улучшений. Другие блоги, связанные с EF см. в разделе блоги по [начало работы с Entity Framework](https://msdn.microsoft.com/data/ee712907).
- [Журнал MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx). См. в разделе **точек данных** столбец, который часто является темы, связанные с платформой Entity Framework.

<a id="wfdatabinding"></a>

## <a name="data-binding-in-aspnet-web-forms-applications"></a>Привязка данных в ASP.NET Web Forms приложений

- [Веб-форм ASP.NET параметры доступа к данным](https://msdn.microsoft.com/library/jj822927.aspx) (MSDN)<a id="wfmodelbinding"></a>.

<a id="wfmodelbinding"></a>

### <a name="using-web-forms-model-binding"></a>С помощью веб-форм привязки модели

- [Привязка модели и Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). С помощью EF Code First серии руководств.
- [Web Forms привязка моделей, часть 1: Выбор данных](https://weblogs.asp.net/scottgu/archive/2011/09/06/web-forms-model-binding-part-1-selecting-data-asp-net-vnext-series.aspx) (блог Скотта Гатри). В эти старые записи блога свойство, которое в настоящее время называется ItemType назывался ModelType, но в противном случае сведения, содержащиеся в них является допустимым.
- [Web Forms привязка моделей, часть 2: Фильтрация данных](https://weblogs.asp.net/scottgu/archive/2011/09/12/web-forms-model-binding-part-2-filtering-data-asp-net-vnext-series.aspx) (блог Скотта Гатри).
- [Web Forms привязка моделей, часть 3: Обновление и проверка](https://weblogs.asp.net/scottgu/archive/2011/10/30/web-forms-model-binding-part-3-updating-and-validation-asp-net-4-5-series.aspx) (блог Скотта Гатри).
- [Привязка модели 4,5 веб-форм ASP.NET](../web-forms/videos/aspnet-web-forms-vnext/aspnet-45-web-forms-model-binding.md). (видео).
- [Привязка моделей, часть 1 - выборка данных](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-1-selecting-data.md) (видео).
- [Привязка моделей, часть 2 - фильтрация](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-2-filtering.md) (видео).
- [Приступая к работе с ASP.NET 4.5 веб-форм - отображения данных элементов и подробностей](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).

<a id="wfdsc"></a>

### <a name="using-web-forms-data-source-controls"></a>С помощью веб-форм элементов управления источниками данных

- [Данные источника управления](https://msdn.microsoft.com/library/ms247258.aspx) (MSDN).
- [Состоялся выпуск поставщика динамических данных и управления EntityDataSource для Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/02/28/announcing-the-release-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx) (блог веб-разработки Майкрософт).

<a id="wfdbc"></a>

### <a name="using-web-forms-data-bound-controls-and-data-binding-expressions"></a>С помощью веб-форм, элементов управления с привязкой данных и выражения привязки данных

- [Привязка модели и Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Серии руководств, использующий EF Code First.
- [Приступая к работе с ASP.NET 4.5 веб-форм - отображения данных элементов и подробностей](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).
- [Строго типизированные элементы управления данными](https://weblogs.asp.net/scottgu/archive/2011/09/02/strongly-typed-data-controls-asp-net-vnext-series.aspx) (блог Скотта Гатри).
- [Строго типизированные элементы управления данными](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (видео).
- [ASP.NET 4.5 строгих типизированных данных элементов управления Web Forms](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (видео).
- [Элемент управления привязкой к данным](https://msdn.microsoft.com/library/ms228214.aspx) (MSDN).
- [Общие сведения о выражениях привязки данных](https://msdn.microsoft.com/library/ms178366.aspx) (MSDN). На этой странице рассматриваются только **Eval** и **привязать**; он не был обновлен для включения **элемент** и **BindItem**.

<a id="sqlserver"></a>

## <a name="working-with-sql-server-databases"></a>Работа с базами данных SQL Server

- [Функции базы данных SQL Server](https://msdn.microsoft.com/library/hh230827.aspx) (MSDN). Общие сведения о самых разнообразных разделы по SQL Server см. в записях в разделе в Оглавлении.
- [Выпуски SQL Server](https://msdn.microsoft.com/library/ms178359.aspx#sqlserver) (MSDN). Сводка Доступные выпуски SQL Server, со ссылками на дополнительные сведения о каждом из них.)
- [Строки подключения SQL Server для веб-приложений ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [С помощью SQL Server Compact для веб-приложений ASP.NET](https://msdn.microsoft.com/library/ms247257.aspx) (MSDN).
- [Microsoft SQL Server: Образцы продуктов, базы данных](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Образцы баз данных AdventureWorks.
- [Установка образцов баз данных](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Помимо методов, показанных здесь, вы также можете скачать одно из MDF-файлов образца в приложение\_папка данных веб-проекта, преобразования базы данных в LocalDB и создать строку подключения LocalDB. Сведения о том, как это сделать, см. в разделе [как: обновление до LocalDB](https://msdn.microsoft.com/library/hh873188.aspx).

Также в следующих разделах по работе с SQL Server Express и LocalDB, выбор между SQL Server и базы данных SQL.

<a id="sslocaldb"></a>

### <a name="working-with-sql-server-express-localdb-databases"></a>Работа с базами данных SQL Server Express LocalDB

- [SQL Server Express 2012 LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (MSDN). Официальный MSDN Общие сведения о LocalDB.
- [Строки подключения SQL Server для веб-приложений ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Практическое: обновление до LocalDB](https://msdn.microsoft.com/library/hh873188.aspx) (MSDN). Как перенести MDF-файла из более ранней версии SQL Server Express LocalDB. У вас также есть пройти этот процесс, если необходимо загрузить один из [образцы баз данных SQL Server 2012](https://go.microsoft.com/fwlink/?linkid=117483).
- [Общие сведения о LocalDB, улучшенная Express SQL](https://go.microsoft.com/fwlink/?LinkId=234375) (блог SQL Server Express). Содержит дополнительные сведения о причины создания LocalDB не включается в MSDN.
- [LocalDB: Where is My Database?](https://go.microsoft.com/fwlink/?LinkId=234376) (Блог SQL Server Express). Сведения о котором создаются файлы базы данных LocalDB.
- [Использование LocalDB с полнофункциональными службами IIS, часть 1: профиль пользователя](https://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-1-user-profile.aspx) (блог SQL Server Express). LocalDB не предназначен для работы со службами IIS. Этой серии записей блога описываются проблемы и некоторые обходные пути.

<a id="sse"></a>

### <a name="working-with-sql-server-express-databases"></a>Работа с базами данных SQL Server Express

- [Строки подключения SQL Server для веб-приложений ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN). Если вы используете строку подключения AttachDBFileName с SQL Server Express, см. в разделе особенно разделе пользовательский экземпляр части этой страницы.
- [Как стать владельцем вашего локального SQL Server Express 2008](https://blogs.msdn.com/b/sqlexpress/archive/2010/02/23/how-to-take-ownership-of-your-local-sql-server-2008-express.aspx) (блог SQL Server Express). Распространенной проблемой не могут работать с базами данных SQL Server Express, так как вы не являетесь администратором на экземпляре SQL Server Express. По умолчанию только пользователь, выполнивший установку, SQL Server Express является администратором. В этом блоге объясняется, как сделать самостоятельно администратором SQL Server Express, если вы являетесь администратором на компьютере.
- [Мое веб-приложение ASP.NET использовать базу данных SQL Server Express в рабочей среде](https://msdn.microsoft.com/library/jj653753.aspx#sql_express_in_production) (MSDN).

<a id="ssdb"></a>

### <a name="working-with-windows-azure-sql-database"></a>Работа с базой данных Azure SQL для Windows

- [Развертывание приложения Secure ASP.NET MVC с членством, OAuth и базой данных SQL для веб-сайта Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data) (сайт Microsoft Azure).
- [Базы данных SQL](https://docs.microsoft.com/azure/sql-database/) (сайт Microsoft Azure). Получение, учебники и пошаговые руководства.
- [База данных Azure SQL Windows](https://msdn.microsoft.com/library/windowsazure/ee336279.aspx) (MSDN). Узел верхнего уровня оглавление для базы данных SQL в библиотеке MSDN.
- [Windows Azure SQL базы данных TechNet Wiki статьи индекс](https://social.technet.microsoft.com/wiki/contents/articles/2267.windows-azure-sql-database-technet-wiki-articles-index-en-us.aspx) (сайте Microsoft TechNet).
- [Блок приложения для обработки временных сбоев](https://msdn.microsoft.com/library/hh680934(v=PandP.50).aspx). Это платформа, которая позволяет обрабатывать временных сетевых сбоев и ошибок подключения, возникших в результате регулирования. В пакете NuGet: [библиотеки Enterprise Library 5.0 — блок приложения обработки для временных сбоев](http://nuget.org/packages/EnterpriseLibrary.WindowsAzure.TransientFaultHandling).
- [Приступая к работе с базой данных SQL и платформа Entity Framework](https://msdn.microsoft.com/data/jj556244) (MSDN).
- [Пакет Windows Azure Training Kit](https://www.microsoft.com/download/details.aspx?id=8396) (Центр загрузки Майкрософт). Включает в себя практические занятия для базы данных SQL.
- [Форум сообщества Windows Azure SQL Database](https://social.msdn.microsoft.com/Forums/ssdsgetstarted/threads).
- [Перемещение в базу данных Azure SQL Windows](https://msdn.microsoft.com/library/ff803375.aspx) (MSDN). Один главе комплексный сценарий end-to-end группой Microsoft Patterns and Practices. Рассматривается, почему может потребоваться перенести и о миграции из SQL Server базу данных SQL.
- [Миграция баз данных SQL Server в базу данных Azure SQL Windows](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).
- [Мастер миграции баз данных SQL](http://sqlazuremw.codeplex.com/). Средство с открытым исходным по переносу баз данных и из базы данных SQL.

<a id="ssdbchoosing"></a>

### <a name="choosing-between-sql-server-and-windows-azure-sql-database"></a>Выбор между SQL Server и база данных Azure SQL для Windows

- [Сравнение SQL Server с базой данных SQL Windows Azure](https://social.technet.microsoft.com/wiki/contents/articles/996.compare-sql-server-with-windows-azure-sql-database-en-us.aspx) (сайте Microsoft TechNet).
- [Перенос данных в базе данных Azure SQL в Windows: средства и методы](https://msdn.microsoft.com/library/windowsazure/hh694043.aspx) (MSDN). Содержит разделы, сравнение SQL Server в базу данных SQL и советы о том, когда для переноса из SQL Server в базу данных SQL.
- [Руководство по доставке базы данных SQL Windows Azure](https://social.technet.microsoft.com/wiki/contents/articles/3398.windows-azure-sql-database-delivery-guide-en-us.aspx) (сайте Microsoft TechNet).
- [Ограничения функций SQL Server (база данных Azure SQL для Windows)](https://msdn.microsoft.com/library/windowsazure/ff394115.aspx) (MSDN).
- [Windows Azure Table Storage и база данных Azure SQL — сходства и различия](https://msdn.microsoft.com/library/jj553018.aspx) (MSDN). Для приложения, развертывание в Windows Azure хранилище таблиц Windows Azure может быть альтернативой базы данных SQL Windows Azure. Этот раздел поможет вам выбрать между этими альтернативами.
- [База данных Azure SQL Windows](https://msdn.microsoft.com/library/windowsazure/ee336279) (MSDN).
- [Рекомендации и ограничения (база данных Azure SQL для Windows)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)

<a id="nosql"></a>

## <a name="working-with-nosql-database-management-systems"></a>Работа с системами управления базами данных NoSQL

- [Данные служб Windows Azure](https://www.windowsazure.com/develop/net/data/) (сайт Microsoft Azure). См. в разделе [специализированной службы таблиц](https://docs.microsoft.com/azure/) и **больших данных** раздел страницы.
- [ASP.NET многоуровневого приложения с помощью хранилища таблиц, очередей и больших двоичных объектов](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (сайт Microsoft Azure). End-to-end учебнику, загружаемый пример приложения, использующего таблиц NoSQL службы хранилища Windows Azure.

<a id="linq"></a>

## <a name="using-linq-queries-in-aspnet-applications"></a>С помощью запросов LINQ в приложениях ASP.NET

- [Параметры доступа к данным ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx#linq) (MSDN). Содержит общие сведения о LINQ.
- [Обучающие видеоматериалы по LINQ](http://www.misfitgeek.com/windows-client-linq-training-videos-20/) (Joe Stagner блог).
- [Обсуждение форума ASP.NET со ссылками на динамические ресурсы LINQ](https://forums.asp.net/p/1961037/5601994.aspx?Please+suggest+good+books+so+that+one+can+write+and+understand+dynamic+linq).

<a id="dd"></a>

## <a name="using-dynamic-data-scaffolding"></a>С помощью формирования шаблонов платформы динамических данных

- [Шаблоны проектов платформы динамических данных](https://msdn.microsoft.com/library/jj822927.aspx#dynamicdata) (MSDN). Рекомендации по использованию динамических данных проектов.
- [Платформа динамических данных ASP.NET](https://msdn.microsoft.com/library/ee845452.aspx) (MSDN).

<a id="securing"></a>

## <a name="securing-data-access"></a>Защита доступа к данным

- [Защита доступа к данным в ASP.NET](https://msdn.microsoft.com/library/ms178375.aspx) (MSDN).
- [Рекомендации по безопасности (платформа Entity Framework)](https://msdn.microsoft.com/library/cc716760.aspx) (MSDN).
- [Практическое руководство: Защита строк подключения, при использовании элементов управления источниками данных](https://msdn.microsoft.com/library/ms178372.aspx) (MSDN).

<a id="optimizingdataaccess"></a>

## <a name="optimizing-data-access-performance"></a>Оптимизация скорость доступа к данным

- [Общие сведения о производительности ASP.NET](https://msdn.microsoft.com/library/cc668225.aspx) (MSDN).
- [Кэширование ASP.NET](https://msdn.microsoft.com/library/xsbfdd8c.aspx) (MSDN).
- [Повышение производительности ASP.NET](https://msdn.microsoft.com/library/ff647787) (MSDN). Предупреждение «Устаревшее содержимое» в верхней части этой страницы, но большая часть информации по-прежнему относится и отсутствует сопоставимых обновленный ресурс.
- [Повышение производительности SQL Server](https://msdn.microsoft.com/library/ff647793) (MSDN). Одинаковые комментарии как предыдущая ссылка.

См. также [Entity Framework, оптимизация производительности](#optimizingef) ранее в этом разделе.

<a id="deploying"></a>

## <a name="deploying-a-database"></a>Развертывание базы данных

- [Развертывание веб-ASP.NET — рекомендуемые ресурсы](aspnet-web-deployment-content-map.md) (MSDN).

<a id="webservice"></a>

## <a name="accessing-data-through-a-web-service"></a>Доступ к данным через веб-службы

- [Доступ к данным через веб-службу](https://msdn.microsoft.com/library/ms178359.aspx#webservice) (MSDN). Рекомендации по использованию веб-API и WCF.
- [Приступая к работе с веб-API ASP.NET](../web-api/index.md).
- [Службы WCF Data Services](https://msdn.microsoft.com/data/bb931106) (MSDN).

<a id="additional"></a>

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Доступ к данным ASP.NET часто задаваемые вопросы о](https://msdn.microsoft.com/library/jj653753.aspx) (MSDN).
- [Веб-форм ASP.NET учебники - данных](../web-forms/overview/data-access/index.md). Эти учебники большинство относительно старых; Обязательно прочтите статью [параметры доступа к данным ASP.NET](https://msdn.microsoft.com/library/ms178359.aspx) и [варианты хранения данных (Создание облачных реального мира, приложений с помощью Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md) первой, чтобы не получить слишком далеко в метод доступа к данным, который неправильно для вашего сценария.
- [Карта содержимого для ASP.NET MVC](../mvc/overview/getting-started/recommended-resources-for-mvc.md).
- [Веб-страницы ASP.NET учебники - данных](../web-pages/overview/data/index.md).
- [Доступ к данным в Visual Studio](https://msdn.microsoft.com/library/wzabh8c4.aspx) (MSDN). Предоставляет список ссылок, аналогичный эта карта содержимого, но с упором на Visual Studio, а не ASP.NET.
