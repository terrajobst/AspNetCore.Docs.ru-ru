---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
title: 'Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: развертывание Compact баз данных SQL Server - 2 12 | Документы Microsoft'
author: tdykstra
description: Ряд учебниках показано развертывание ASP.NET (публикации) проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: c3c76516-4c48-4153-bd03-d70e3a3edbb0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
msc.type: authoredcontent
ms.openlocfilehash: 7e2d430bd8e07ed7d97d11a00c61d90beeac005f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-sql-server-compact-databases---2-of-12"></a>Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: развертывание Compact баз данных SQL Server - 2 12
====================
по [Tom Dykstra](https://github.com/tdykstra)

[Загрузите начальный проект](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Ряд учебниках показано развертывание ASP.NET (публикации) проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Studio 2012 RC или Visual Studio Express 2012 RC для Web. Также можно использовать Visual Studio 2010, при установке обновления публикации Web. Введение ряда см. в разделе [в первом учебнике ряда](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Учебник, в котором показаны возможности развертывания, появившиеся после выпуска версии-КАНДИДАТА Visual Studio 2012, показано, как развернуть выпусках SQL Server, отличных от SQL Server Compact и показано, как для развертывания на веб-приложениях службы приложений Azure, в разделе [развертывания веб-приложения ASP.NET с помощью Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Обзор

Этого учебника показано, как настроить две базы данных SQL Server Compact и компонент database engine для развертывания.

Для доступа к базе данных приложение Contoso университета требуется следующее программное обеспечение, должны быть развернуты вместе с приложением, так как он не включен в .NET Framework:

- [SQL Server Compact](https://www.microsoft.com/sqlserver/en/us/editions/compact.aspx) (компонент database engine).
- [Универсальные поставщики ASP.NET](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (включить систему членства ASP.NET для использования SQL Server Compact)
- [Entity Framework 5.0](https://msdn.microsoft.com/library/gg696172(d=lightweight,v=vs.103).aspx)(Code First с миграции).

Структура базы данных, а также некоторые (но не всех) данных в двух приложения баз данных также должны быть развернуты. Как правило при разработке приложения, введите тестовые данные в базу данных, не требуется для развертывания на действующем сайте. Тем не менее можно также ввести некоторые данные производства, которые вы хотите развернуть. В этом учебнике следует настроить проект Contoso университета, чтобы необходимое программное обеспечение и правильные данные включаются при развертывании.

Напоминание: Если вы получаете сообщение об ошибке или что-то не работает, как пройти учебник, не забудьте проверить [страницу устранения неполадок](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="sql-server-compact-versus-sql-server-express"></a>SQL Server Compact и SQL Server Express

В примере приложения используется SQL Server Compact 4.0. Этот компонент database engine — относительно новый параметр для веб-сайтов; более ранних версиях SQL Server Compact не работают в среде веб-размещения. SQL Server Compact обеспечивает ряд преимуществ по сравнению с более распространенным разработки с SQL Server Express и развертывания для всего сервера SQL. В зависимости от выбранного поставщика услуг размещения SQL Server Compact может быть недорого развернуть, так как некоторые поставщики могут потребовать дополнительного для поддержки полной базы данных SQL Server. Нет без дополнительной платы для SQL Server Compact, поскольку сам компонент database engine можно развернуть как часть веб-приложения.

Тем не менее также следует знать о ее ограничениях. SQL Server Compact не поддерживает хранимые процедуры, триггеры, представления или репликации. (Полный список функций SQL Server, которые не поддерживаются в SQL Server Compact см. в разделе [различия между SQL Server Compact и SQL Server](https://msdn.microsoft.com/library/bb896140.aspx).) Кроме того некоторые средства, которые можно использовать для управления схемы и данные в базах данных SQL Server и SQL Server Express не поддерживают работу с SQL Server Compact. Например нельзя использовать SQL Server Management Studio или SQL Server Data Tools в Visual Studio с базами данных SQL Server Compact. Имеются другие параметры для работы с базами данных SQL Server Compact.

- Можно использовать обозреватель серверов в Visual Studio предлагает функциональные возможности обработки ограниченного базы данных для SQL Server Compact.
- Можно использовать функцию обработки базы данных [WebMatrix](https://www.microsoft.com/web/webmatrix/), которая имеет больше возможностей, чем обозревателя серверов.
- Можно использовать относительно полнофункциональный сторонних разработчиков или открыть источник средства, такие как [SQL Server Compact элементов](https://github.com/ErikEJ/SqlCeToolbox) и [SQL Compact данных и схемы скрипта программы](https://github.com/ErikEJ/SqlCeToolbox).
- Можно писать и выполнять собственные сценарии DDL (языка описания данных) для управления схему базы данных.

Можно начать с SQL Server Compact, а затем обновить позже, в случае изменения потребностей. Следующих занятиях рассматривается в этой серии показывают, как выполнить миграцию из SQL Server Compact для SQL Server Express и SQL Server. Тем не менее если вы создаете новое приложение и планируется в ближайшем будущем нужен сервер SQL Server, следует начать с SQL Server или SQL Server Express.

## <a name="configuring-the-sql-server-compact-database-engine-for-deployment"></a>Настройка SQL Server Compact Database Engine для развертывания

Программное обеспечение, необходимое для доступа к данным в приложении университета Contoso был добавлен, установив следующие пакеты NuGet:

- [SqlServerCompact](http://nuget.org/List/Packages/SqlServerCompact)
- [System.Web.Providers](http://nuget.org/List/Packages/System.Web.Providers) (универсальные поставщики ASP.NET)
- [EntityFramework](http://nuget.org/List/Packages/EntityFramework)
- [EntityFramework.SqlServerCompact](http://nuget.org/List/Packages/EntityFramework.sqlservercompact)

Эти ссылки указывают на текущие версии этих пакетов, которые могут быть более новой версии установленных в начальный проект, загруженный в этом учебнике. Для развертывания у поставщика услуг размещения убедитесь, что использовать Entity Framework 5.0 или более поздней версии. Code First Migrations более ранних версий требуют полного доверия и запуска приложения многие поставщики услуг размещения со средним уровнем доверия. Дополнительные сведения о со средним уровнем доверия см. в разделе [развертывание в IIS в тестовой среде](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) учебника.

Установка пакета NuGet обычно обеспечивает все сведения, необходимые для развертывания этого программного обеспечения с приложением. В некоторых случаях выполняется задач, таких как изменение файла Web.config и добавление сценарии PowerShell, которые выполняются при каждой сборке решения. **Если вы хотите добавить поддержку для любой из этих функций (например, SQL Server Compact и Entity Framework) без использования NuGet, убедитесь, что необходимо знать, что установка пакета NuGet делает, которая может выполнять те же действия вручную.**

Есть одно исключение, в котором NuGet не принимает следить за все, что нужно сделать, чтобы обеспечить успешное развертывание. Пакет SqlServerCompact NuGet после построения скрипт добавляется в проект, который копирует сборки в машинном коде для *x86* и *amd64* во вложенных папках проекта *bin* Папка, но скрипт не включает эти папки в проекте. В результате веб-развертывание не будет копировать их на веб-узел назначения Если вы вручную не включены в проект. (Это поведение результатов из конфигурации развертывания по умолчанию, имеет другой параметр, который не будет использовать в этих учебниках, чтобы изменить параметр, контролирующий поведение. — Параметр, который можно изменить **только файлы, необходимые для запуска приложения** под **развертываемые элементы** на **Пакет/Публикация веб-сайта** вкладка **проекта Свойства** окна. Изменение этого параметра обычно не рекомендуется, так как это может привести к развертывание много дополнительных файлов в рабочую среду, чем это требуется существует. Дополнительные сведения об альтернативных вариантов см. в разделе [Настройка свойств проекта](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md) учебник.)

Выполните построение проекта, а затем в **обозревателе решений** щелкните **Показать все файлы** Если вы еще не выполнена. Может потребоваться нажать кнопку **обновление**.

![Solution_Explorer_Show_All_Files](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image1.png)

Разверните **bin** папку, чтобы просмотреть **amd64** и **x86** папки и выберите те папки, щелкните правой кнопкой мыши и выберите **включить в проект**.

![amd64_and_x86_in_Solution_Explorer.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image2.png)

Значки папок изменится, что папка был включен в проект.

![Solution_Explorer_amd64_included.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image3.png)

## <a name="configuring-code-first-migrations-for-application-database-deployment"></a>Настройка миграции Code First для развертывания базы данных приложения

При развертывании базы данных приложения, обычно не развертываются просто базы данных разработки все данные в них в рабочей среде, так как большая часть данных в ней, возможно, присутствует только для целей тестирования. Например учащихся в тестовую базу данных, называются вымышленной. С другой стороны часто невозможно развернуть только структуру базы данных с данными, в его вообще. Некоторые данные в тестовой базы данных может быть реальными данными и должны присутствовать при пользователи начнут использовать приложение. Например вашей базе данных может быть таблица, содержащая допустимый уровень значения или имена реальных отдела.

Для имитации этом распространенном сценарии, следует настроить код первого миграций, начальное значение метод, который вставляет в базе данных только данные, которые должны быть существует в рабочей среде. Этот метод инициализации не вставить тестовых данных, так как будет выполняться в рабочей среде после Code First создает базу данных в рабочей среде.

В более ранних версиях Code First до выпуска миграции было часто такие методы начальное значение для вставки тестовых данных, кроме того, поскольку при каждом изменении модели во время разработки базы данных было полностью удалить и повторно создать с нуля. С помощью Code First Migrations тестовых данных сохраняется после изменения базы данных, включая тестовых данных в методе начальное значение не требуется. Проект, который вы загрузили метод до миграции, в том числе все данные в методе начальное значение инициализатор класса. В этом учебнике будет отключить инициализатор класса и включить миграцию. Затем обновите Seed-метод в классе конфигурации миграции, чтобы он вставляет только данные, которые нужно вставить в рабочей среде.

На следующей схеме показана схема базы данных приложения:

[![School_database_diagram](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image4.png)

Для этих учебников вы Предположим, что `Student` и `Enrollment` таблицы должно быть пустым, когда узел сначала развертывается. Другие таблицы содержат данные, должен ли предварительная загрузка приложения станет активным.

Поскольку вы будете использовать Code First Migrations, больше не нужно использовать **DropCreateDatabaseIfModelChanges** Code First инициализатора. Код для этого инициализатора находится в файле SchoolInitializer.cs в проекте ContosoUniversity.DAL. Параметр в **appSettings** этого инициализатор для выполнения каждый раз, когда приложение пытается получить доступ к базе данных в первый раз вызывает элемента в файле Web.config:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample1.xml?highlight=3)]

Откройте файл Web.config приложения и удалить элемент, который задает Code First инициализатор класса из элемента appSettings. Элемент appSettings теперь выглядит следующим образом:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample2.xml)]

> [!NOTE]
> Другой способ указать инициализатор класса является выполняется вызовом метода `Database.SetInitializer` в `Application_Start` метод в *Global.asax* файла. При включении миграции в проекте, который использует этот метод для указания инициализатор, удалите эту строку кода.


Затем включите Code First Migrations.

Первым шагом является убедитесь в том, что проект ContosoUniversity задано как автозагружаемый проект. В **обозревателе решений**, щелкните правой кнопкой мыши проект ContosoUniversity и выберите **Назначить запускаемым проектом**. Code First Migrations просмотрит Автозагружаемый проект, чтобы найти строку подключения базы данных.

Из **средства** меню, нажмите кнопку **диспетчер пакетов библиотеки** и затем **консоль диспетчера пакетов**.

![Selecting_Package_Manager_Console](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image6.png)

В верхней части **консоль диспетчера пакетов** окна выберите ContosoUniversity.DAL в качестве проекта по умолчанию, а затем at `PM>` строке введите «enable-migrations».

![включить migrations_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image7.png)

Эта команда создает *Configuration.cs* файл в новом *миграций* папку в проекте ContosoUniversity.DAL.

![Migrations_folder_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image8.png)

Вы выбрали проект DAL, так как команда «enable-migrations» должен выполняться в проект, который содержит код первый класс контекста. После этого класса в проект библиотеки классов Code First Migrations ищет строку подключения базы данных в запускаемым проектом решения. Веб-проекта в решении ContosoUniversity был задан как запускаемый проект. (Если было не требуется для обозначения проект, который содержит строку подключения как автозагружаемый проект в Visual Studio, можно указать запускаемым проектом в команде PowerShell. Чтобы просмотреть синтаксис команды для команду enable-migrations, можно ввести команду «get-help enable-migrations».)

Откройте файл Configuration.cs и замените комментарии в `Seed` метод следующим кодом:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample3.cs)]

Ссылки на `List` имеют красная волнистая линия под их, так как отсутствует `using` инструкции еще для пространства имен. Щелкните правой кнопкой мыши один из экземпляров `List` и нажмите кнопку **Разрешить**, а затем нажмите кнопку **using System.Collections.Generic**.

![Разрешить с помощью инструкции](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image9.png)

Следующий код добавляет этот выбор меню `using` инструкции в верхней части файла.

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample4.cs)]

> [!NOTE]
> Добавление кода для `Seed` метода — один из многих способов, которые можно вставить основных данных в базу данных. Альтернативным вариантом является добавление кода для `Up` и `Down` методов класса каждой миграции. `Up` И `Down` методы содержат код, который реализует изменения в базе данных. Вы увидите их в примеры [развертывание обновления базы данных](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) учебника.
> 
> Также можно написать код, который выполняет инструкции SQL с помощью `Sql` метод. Например, если добавление столбца бюджета таблица Department, необходимо инициализировать все бюджеты отделов для 1000,00 долларов в процессе миграции удалось добавить соответствовать строку кода для `Up` метод для этой миграции:
> 
> `Sql("UPDATE Department SET Budget = 1000");`
> 
> В этом примере, отображаемое в этом учебнике используется `AddOrUpdate` метод в `Seed` метод Code First Migrations `Configuration` класса. Code First Migrations вызовы `Seed` метод после каждой миграции и этот метод обновляет строки, которые уже были вставлены или вставляет их, если они еще не существуют. `AddOrUpdate` Метод может оказаться самым подходящим для вашего сценария. Дополнительные сведения см. в разделе [Будьте внимательны с EF 4.3 AddOrUpdate метод](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) Джули Лерман блога.


Нажмите клавиши CTRL-SHIFT-B для сборки проекта.

Следующим шагом является создание `DbMigration` класс для первоначальной миграции. Вы хотите завершения миграции, чтобы создать новую базу данных, их необходимо удалить базу данных, которая уже существует. Базы данных SQL Server Compact находятся в *.sdf* файлы в *приложения\_данные* папки. В **обозревателе решений**, разверните *приложения\_данные* в ContosoUniversity проекта, чтобы просмотреть эти две базы данных SQL Server Compact, который представлен *.sdf*файлов.

Щелкните правой кнопкой мыши *School.sdf* файла и нажмите кнопку **удалить**.

![sdf_files_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image10.png)

В **консоль диспетчера пакетов** окно, введите команду «Добавить миграции начальным» для создания первоначальной миграции и назовите его «Начальный».

![добавить migration_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image11.png)

Code First Migrations создает еще один файл класса в *миграций* , а этот класс содержит код, создающий схему базы данных.

В **консоль диспетчера пакетов**, введите команду «update-database» для создания базы данных и выполнения **начальное значение** метод.

![update-database_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image12.png)

(Если появляется ошибка указывает таблицу, уже существует и не может быть создан, возможно, будет потому, что приложение запущено после удаления базы данных и до выполнения `update-database`. Удалить in that Case, *School.sdf* файл еще раз и повторите `update-database` команд.)

Запустите приложение. Теперь учащихся страница пуста, но страница инструкторов содержит инструкторов. Это то, что вы получите в рабочей среде после развертывания приложения.

![Empty_Students_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image13.png)

![Instructors_page_after_initial_migration](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image14.png)

Теперь проект готов к развертыванию *School* базы данных.

## <a name="creating-a-membership-database-for-deployment"></a>Создание базы данных членства для развертывания

Приложение университета Contoso использует проверки подлинности системы и форм членства ASP.NET для проверки подлинности и авторизации пользователей. Один из его страницы доступна только для администраторов. Чтобы просмотреть эту страницу, запустите приложение и выберите **обновление кредиты** из всплывающее меню в разделе **курсы**. Приложение отображает **входа в** страницы, так как только администраторы могут использовать **обновление кредиты** страницы.

[![Log_in_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image15.png)

Войдите в систему как «admin», с помощью пароля «Pas$ w0rd» (Обратите внимание, номер ноль вместо букву «o» в «w0rd»). После входа в **обновление кредиты** -страница.

[![Update_Credits_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image17.png)

При развертывании узла в первый раз, чаще всего для исключения большинство или все учетные записи пользователей, созданные для тестирования. В этом случае вы развернете учетную запись администратора и учетные записи пользователей отсутствуют. Вместо того чтобы вручную удалить тестовые учетные записи, вы создадите новую базу данных членства, имеет только один администратор учетной записи пользователя, необходимых в рабочей среде.

> [!NOTE]
> База данных членства сохраняет хэш паролей учетных записей. Чтобы развернуть учетные записи с одного компьютера на другой, необходимо убедиться, что хэширования подпрограммы не создавать разные хэши на конечном сервере не на исходном компьютере. Генераторы будут получать одинаковые хэш-коды при использовании универсальные поставщики ASP.NET до тех пор, пока не будет изменять алгоритм по умолчанию. По умолчанию алгоритм HMACSHA256 и определен в **проверки** атрибут **[machineKey](https://msdn.microsoft.com/library/w8h3skw9.aspx)** в файле Web.config.


База данных членства не обслуживается Code First Migrations, и нет без автоматического инициализатора, заполняющего базы данных с учетными записями теста (как для базы данных School). Таким образом чтобы сохранить данные теста, доступные вносятся копию тестовую базу данных перед созданием нового.

В **обозревателе решений**, переименуйте *aspnet.sdf* файла в *приложения\_данные* папку для *aspnet Dev.sdf*. (Не копировать, просто переименуйте его, вы создадите новую базу данных позже.)

В **обозревателе решений**, убедитесь, что веб-проекта (ContosoUniversity, не ContosoUniversity.DAL). Затем в **проекта** последовательно выберите пункты **конфигурации ASP.NET** для запуска **Web Site Administration Tool**(WAT).

Выберите **безопасности** вкладки.

[![WAT_Security_tab](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image19.png)

Нажмите кнопку **Создание или управление ролями** и добавьте **администратора** роли.

[![WAT_Create_New_Role](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image21.png)

Вернитесь к **безопасности** щелкните **Create User**, и добавить пользователя «администратор» в качестве администратора. Прежде чем нажимать кнопку **Create User** кнопку **Create User** убедитесь, что выбрана **администратора** флажок. Пароль, используемый в этом учебнике будет «Pas$ w0rd», и можно ввести любой адрес электронной почты.

[![WAT_Create_User](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image23.png)

Закройте браузер. В **обозревателе решений**, нажмите кнопку «Обновить», чтобы увидеть вновь *aspnet.sdf* файла.

![New_aspnet.sdf_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image25.png)

Щелкните правой кнопкой мыши **aspnet.sdf** и выберите **включить в проект**.

## <a name="distinguishing-development-from-production-databases"></a>Отличительный признак разработки из производственной базы данных

В этом разделе вы переименуете баз данных, чтобы версии разработки являются School Dev.sdf aspnet Dev.sdf и версиях School Prod.sdf и и aspnet Prod.sdf. Это не является обязательным, но делать так поможет предотвратить получение путать и тестовой или рабочей версии базы данных.

В **обозревателе решений**, нажмите кнопку **обновление** и разверните приложение\_папка данных для базы данных School, созданный ранее в разделе, щелкните его правой кнопкой мыши и выберите **включить в проект** .

![Including_School.sdf_in_project](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image26.png)

Переименуйте *aspnet.sdf* для *aspnet Prod.sdf*.

Переименуйте *School.sdf* для *School-Dev.sdf*.

При запуске приложения в Visual Studio, вы не хотите использовать *-Prod* версий файлов базы данных, необходимо использовать *- Dev* версии. Таким образом, вам необходимо изменить строки подключения в файле Web.config, чтобы они указывали на *- Dev* версии баз данных. (Еще не создал файл School Prod.sdf, но это нормально, так как Code First создаст этой базы данных в производственной первый раз при запуске приложения существует).

Откройте файл Web.config приложения и найдите строки подключения:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample5.xml)]

Измените «aspnet.sdf» на «aspnet Dev.sdf» и измените «School.sdf» на «School Dev.sdf»:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample6.xml?highlight=4-5)]

Ядро базы данных SQL Server Compact и обе базы данных теперь готовы к развертыванию. В этом руководстве настраиваются автоматически *Web.config* файл преобразования для параметров, которые должны быть разными в средах разработки, тестирования и эксплуатации. (Вместе с параметрами, которые должны быть изменены представляют собой строки подключения, но необходимо настроить эти изменения позже при создании профиля публикации).

## <a name="more-information"></a>Дополнительные сведения

Дополнительные сведения о NuGet см. в разделе [Управление библиотеками проектов с помощью NuGet](https://msdn.microsoft.com/magazine/hh547106.aspx) и [документации по NuGet](http://docs.nuget.org/docs/start-here/overview). Если вы не хотите использовать NuGet, необходимо узнать, как анализировать пакет NuGet, чтобы определить, что делает при установке. (Например, она может настроить *Web.config* преобразования, настраивать сценарии PowerShell для выполнения во время сборки и т. д.) Дополнительные сведения о работе NuGet см. в разделе особенно [создания и публикации пакета](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) и [файла конфигурации и преобразования кода источника](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Назад](deployment-to-a-hosting-provider-introduction-1-of-12.md)
> [Вперед](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
