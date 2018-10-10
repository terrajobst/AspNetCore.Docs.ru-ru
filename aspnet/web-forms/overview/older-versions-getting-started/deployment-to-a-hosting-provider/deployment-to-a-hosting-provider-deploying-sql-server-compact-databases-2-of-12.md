---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
title: 'Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: развертывание Compact баз данных SQL Server - 2 12 | Документация Майкрософт'
author: tdykstra
description: Этой серии руководств показано, как развернуть (публикации) ASP.NET проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: c3c76516-4c48-4153-bd03-d70e3a3edbb0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
msc.type: authoredcontent
ms.openlocfilehash: 9b3d47c3c8fe5f0b37f1d45e19341df3f91a5bb0
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911192"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-sql-server-compact-databases---2-of-12"></a>Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: развертывание Compact баз данных SQL Server - 2 12
====================
по [том Дайкстра](https://github.com/tdykstra)

[Загрузите начальный проект](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Этой серии руководств показано, как развернуть (публикации) ASP.NET проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Studio 2012 RC или Visual Studio Express 2012 RC для Web. Также можно использовать Visual Studio 2010, если установить обновление веб-публикации. Введение в серии, см. в разделе [в первом учебнике серии](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Учебник, в котором показаны компоненты развертывания, появившиеся после выпуска версии-КАНДИДАТА Visual Studio 2012, содержит сведения о развертывании выпусков SQL Server, отличных от SQL Server Compact и содержит сведения о развертывании веб-приложения службы приложений Azure, см. в разделе [веб-развертывание ASP.NET с помощью Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Обзор

Этом руководстве показано, как настроить две базы данных SQL Server Compact и компонент database engine для развертывания.

Для доступа к базе данных приложения университета Contoso требуется следующее программное обеспечение, которые должны быть развернуты вместе с приложением, так как он не включен в .NET Framework:

- [SQL Server Compact](https://www.microsoft.com/sqlserver/en/us/editions/compact.aspx) (ядро СУБД).
- [Универсальные поставщики ASP.NET](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (включить систему членства ASP.NET для использования SQL Server Compact)
- [Платформа Entity Framework 5.0](https://msdn.microsoft.com/library/gg696172(d=lightweight,v=vs.103).aspx)(Code First с Миграциями).

Структуру базы данных и некоторые (но не все) данных в двух приложения баз данных также должны быть развернуты. Как правило при разработке приложения, введите тестовые данные в базу данных, не требуется для развертывания на действующем сайте. Тем не менее можно также ввести некоторые данные рабочей среде, которые вы хотите развернуть. В этом руководстве вы настроите проект университета Contoso таким образом, чтобы при развертывании включаются необходимое программное обеспечение и правильные данные.

Напоминание: Если что-то не работает, как работать с учебником, вы получаете сообщение об ошибке, не забудьте установить флажок [страница устранения неполадок](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="sql-server-compact-versus-sql-server-express"></a>SQL Server Compact и SQL Server Express

В примере приложения используется SQL Server Compact 4.0. Этот компонент database engine — это относительно новый вариант для веб-сайтов; более ранних версиях SQL Server Compact не работают в среде веб-размещения. SQL Server Compact предоставляет несколько преимуществ по сравнению с более распространенный сценарий разработки с помощью SQL Server Express и развертывания для полной версии SQL Server. В зависимости от выбранного поставщика услуг размещения SQL Server Compact может быть дешевле развернуть, так как некоторые поставщики могут потребовать дополнительного для поддержки всей базы данных SQL Server. Нет без дополнительной платы для SQL Server Compact, так как в самом ядре базы данных можно развернуть как часть веб-приложения.

Однако также следует знать о его ограничениях. SQL Server Compact не поддерживает хранимые процедуры, триггеры, представления или репликации. (Полный список компонентов SQL Server, которые не поддерживаются в SQL Server Compact, см. в разделе [различия между SQL Server Compact и SQL Server](https://msdn.microsoft.com/library/bb896140.aspx).) Кроме того некоторые из средств, которые можно использовать для управления схем и данных в базах данных SQL Server и SQL Server Express не работают с SQL Server Compact. Например нельзя использовать SQL Server Management Studio или SQL Server Data Tools в Visual Studio с базами данных SQL Server Compact. У вас другие параметры для работы с базами данных SQL Server Compact:

- Можно использовать обозреватель серверов в Visual Studio, которая предоставляет функциональные возможности управления ограниченной базы данных для SQL Server Compact.
- Можно использовать функцию обработки базы данных [WebMatrix](https://www.microsoft.com/web/webmatrix/), которая имеет больше возможностей, чем обозревателя серверов.
- Вы можете использовать относительно полнофункциональный независимых производителей или средства с открытым кодом, такие как [SQL Server Compact элементов](https://github.com/ErikEJ/SqlCeToolbox) и [SQL Compact данных и схемы сценарий, служебной программы](https://github.com/ErikEJ/SqlCeToolbox).
- Можно разработать и запускать собственные сценарии DDL (языка определения данных), работать со схемой базы данных.

Можно начать с SQL Server Compact, а затем обновить позже по мере развития потребностей. В последующих учебниках серии показано, как перенести из SQL Server Compact, SQL Server Express и SQL Server. Тем не менее если вы создаете новое приложение и понадобятся в ближайшем будущем SQL Server, вероятно, лучше всего начать с SQL Server или SQL Server Express.

## <a name="configuring-the-sql-server-compact-database-engine-for-deployment"></a>Настройка СУБД SQL Server Compact для развертывания

Программное обеспечение, необходимое для доступа к данным в приложении университета Contoso был добавлен, установив следующие пакеты NuGet:

- [SqlServerCompact](http://nuget.org/List/Packages/SqlServerCompact)
- [System.Web.Providers](http://nuget.org/List/Packages/System.Web.Providers) (универсальные поставщики ASP.NET)
- [EntityFramework](http://nuget.org/List/Packages/EntityFramework)
- [EntityFramework.SqlServerCompact](http://nuget.org/List/Packages/EntityFramework.sqlservercompact)

Эти ссылки указывают на текущие версии этих пакетов, которые могут быть более новой версии установленных в начальный проект, загруженный в этом руководстве. Для развертывания у поставщика услуг размещения убедитесь, что использовать Entity Framework 5.0 или более поздней версии. Более ранние версии Code First Migrations необходим полный уровень доверия, и во многих поставщиков услуг размещения приложения выполняются в со средним уровнем доверия. Дополнительные сведения о со средним уровнем доверия, см. в разделе [развертывание в IIS в качестве тестовой среды](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) руководства.

Установка пакета NuGet обычно берет на себя все сведения, необходимые для развертывания этого программного обеспечения с приложением. В некоторых случаях это включает в себя задачи, например, изменение файла Web.config и скриптов PowerShell, которые выполняются при построении решения. **Если вы хотите добавить поддержку для любой из этих функций (например, SQL Server Compact и Entity Framework) без использования NuGet, убедитесь, что вы знаете, что установка пакета NuGet делает, то есть те же действия можно выполнять вручную.**

Есть одно исключение, где NuGet не принимает себя все, что нужно сделать, чтобы обеспечить успешное развертывание. Пакет SqlServerCompact NuGet добавляет postbuild-скрипт в проект, который копирует сборки в машинном коде для *x86* и *amd64* во вложенных папках проекта *bin* Папка, но он не включает эти папки в проекте. Таким образом веб-развертывание не будет копировать их на целевой веб-сайт Если вы вручную не включены в проект. (Это происходит из конфигурации развертывания по умолчанию, — это еще один вариант, который не используется в этих учебниках, чтобы изменить этот параметр, такое поведение. — Параметр, который можно изменить **только файлы, необходимые для запуска приложения** под **элементы, развертываемые** на **пакета и публикация веб-** вкладке **проекта Свойства** окна. Изменение этого параметра обычно не рекомендуется, так как это может привести развертывание много дополнительных файлов в рабочую среду, чем это требуется существует. Дополнительные сведения о альтернативы, см. в разделе [Настройка свойств проекта](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md) руководства.)

Постройте проект, а затем в **обозревателе решений** щелкните **Показать все файлы** Если вы еще не сделано. Также может потребоваться щелкнуть **обновить**.

![Solution_Explorer_Show_All_Files](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image1.png)

Разверните **bin** просмотрите **amd64** и **x86** папки и выберите эти папки, щелкните правой кнопкой мыши и выберите **включить в проект**.

![amd64_and_x86_in_Solution_Explorer.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image2.png)

Значки папок изменится, что папке был включен в проект.

![Solution_Explorer_amd64_included.PNG](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image3.png)

## <a name="configuring-code-first-migrations-for-application-database-deployment"></a>Настройка Code First Migrations для развертывания базы данных приложения

При развертывании базы данных приложения, обычно не развертывается просто базы данных разработки со всеми данными в рабочей среде, так как значительная часть данных в нем есть возможно только для целей тестирования. Например учащихся в тестовую базу данных, называются вымышленной. С другой стороны часто невозможно развернуть только структуру базы данных с данными, в его вообще. Некоторые данные в тестовой базе данных может быть реальными данными и должен присутствовать при пользователи начнут использовать приложения. Например базы данных возможно, таблица, содержащая допустимые корпоративного класса значения или названия отделов реальных.

Чтобы смоделировать этот сценарий, нужно настроить метод первой миграции начального кода, который вставляет в базе данных только данные, которые вы хотите отставать в рабочей среде. Этот метод начальное значение не Вставка тестовых данных, так как она будет запущена в рабочей среде после Code First создает базу данных в рабочей среде.

В более ранних версиях Code First до миграции выпуска, было обычным методы начальное значение для вставки тестовых данных, кроме того, так как при каждом изменении модели во время разработки базы данных должны были быть полностью удаляется и создается заново с нуля. С помощью Code First Migrations тестовых данных сохраняется после изменения базы данных, включая тестовые данные в методе начальное значение не требуется. Проект, который вы скачали метод до миграции, в том числе все данные в метод заполнения инициализатор класса. В этом руководстве вы отключите инициализатор класса и выполняет миграцию. Затем вы обновите метод заполнения в классе конфигурации миграции, чтобы он вставляет только те данные, которые требуется вставить в рабочей среде.

На следующей схеме показана схема базы данных приложения:

[![School_database_diagram](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image4.png)

Для этих учебников будет предполагается, что `Student` и `Enrollment` таблицы должно быть пустым, при первом развертывании сайта. Другие таблицы содержат данные, должно быть предварительно загружено при активации приложения.

Так как вы будете использовать Code First Migrations, больше не нужно использовать **DropCreateDatabaseIfModelChanges** Code First инициализатор. Код для этого инициализатора находится в файле SchoolInitializer.cs в проекте ContosoUniversity.DAL. Параметр в **appSettings** в файле Web.config приводит к этом инициализатор для выполнения всякий раз, когда приложение пытается получить доступ к базе данных в первый раз:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample1.xml?highlight=3)]

Откройте файл Web.config приложения и удалите элемент, указывающий Code First инициализатор класса из элемента appSettings. Элемент appSettings теперь выглядит следующим образом:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample2.xml)]

> [!NOTE]
> Еще один способ указать класс инициализатора является выполняется путем вызова `Database.SetInitializer` в `Application_Start` метод в *Global.asax* файл. При включении миграции в проекте, который использует этот метод для указания инициализатор, удалите эту строку кода.

Затем включите Code First Migrations.

Первым делом следует убедиться, что проект ContosoUniversity задан в качестве запускаемого проекта. В **обозревателе решений**, щелкните правой кнопкой мыши проект ContosoUniversity и выберите **Назначить запускаемым проектом**. Code First Migrations будет выглядеть в Автозагружаемый проект, чтобы найти строку подключения к базе данных.

Из **средства** меню, щелкните **диспетчер пакетов NuGet** и затем **консоль диспетчера пакетов**.

![Selecting_Package_Manager_Console](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image6.png)

В верхней части **консоль диспетчера пакетов** окне выберите в качестве проекта по умолчанию, а затем at ContosoUniversity.DAL `PM>` командной строке введите «enable-migrations».

![Enable-migrations_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image7.png)

Эта команда создает *Configuration.cs* файл в новом *миграций* папки в проекте ContosoUniversity.DAL.

![Migrations_folder_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image8.png)

Вы выбрали проект DAL, так как «enable-migrations» команда должна выполняться в проект, содержащий Code First класс контекста. Когда этот класс находится в проекте библиотеки классов, Code First Migrations ищет строку подключения базы данных в Автозагружаемый проект для решения. В решении "ContosoUniversity" веб-проекта задано как автозагружаемый проект. (Если была, вы не хотите назначить этот проект, который содержит строку подключения в качестве запускаемого проекта в Visual Studio, можно указать запускаемого проекта в команду PowerShell. Чтобы просмотреть синтаксис команды для команды enable-migrations, можно ввести команду «get-help enable-migrations».)

Откройте файл Configuration.cs и замените комментарии в `Seed` метод следующим кодом:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample3.cs)]

Ссылки на `List` имеют красной волнистой линией под их, так как у вас нет `using` еще оператор для пространства имен. Щелкните правой кнопкой мыши один из экземпляров `List` и нажмите кнопку **устранить**, а затем нажмите кнопку **using System.Collections.Generic**.

![Разрешить с помощью инструкции](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image9.png)

Следующий код добавляет этот выбор меню `using` инструкций в верхней части файла.

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample4.cs)]

> [!NOTE]
> Добавление кода для `Seed` метод является одним из многих способов, которые можно вставлять основных данных в базу данных. Альтернативным вариантом является добавление кода для `Up` и `Down` методы каждого класса миграции. `Up` И `Down` методы содержат код, который реализует изменения в базе данных. Вы увидите примеры из них в [развертывание обновления базы данных](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) руководства.
> 
> Можно также написать код, который выполняет инструкции SQL с помощью `Sql` метод. Например, если имеется Добавление столбца бюджета таблица Department определена и необходимо инициализировать все бюджеты отделов для 1 000,00 долл. США в ходе переноса, удалось добавить соответствовать строки кода для `Up` метод миграции:
> 
> `Sql("UPDATE Department SET Budget = 1000");`
> 
> В этом примере, отображаемое в этом руководстве используется `AddOrUpdate` метод в `Seed` метод Code First Migrations `Configuration` класса. Code First Migrations вызовы `Seed` метод после каждой миграции и этот метод обновляет строки, которые уже были вставлены или вставляет их, если они еще не существует. `AddOrUpdate` Метод может оказаться лучшим выбором для вашего сценария. Дополнительные сведения см. в разделе [Будьте внимательны с помощью метода AddOrUpdate 4.3 EF](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) блога Джули Лерман.


Нажмите клавиши CTRL + SHIFT + B для сборки проекта.

Следующим шагом является создание `DbMigration` класс для первоначальной миграции. Вы хотите ее для создания новой базы данных, поэтому вам нужно удалить базу данных, который уже существует. Базы данных SQL Server Compact содержатся в *.sdf* файлы в *приложения\_данных* папки. В **обозревателе решений**, разверните *приложения\_данных* в проект ContosoUniversity, чтобы увидеть две базы данных SQL Server Compact, который представлен *.sdf*файлы.

Щелкните правой кнопкой мыши *School.sdf* файл и нажмите кнопку **удалить**.

![sdf_files_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image10.png)

В **консоль диспетчера пакетов** окно, введите команду «add-migration Initial» для создания первоначальной миграции и назовите его «Начальный».

![добавить migration_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image11.png)

Code First Migrations создает еще один файл класса в *миграций* папку, а этот класс содержит код, создающий схему базы данных.

В **консоль диспетчера пакетов**, введите команду «update-database» для создания базы данных и запуска **начальное значение** метод.

![Обновление database_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image12.png)

(Если вы сообщение об ошибке, указывающее таблицу, уже существует и не может быть создан, то, скорее всего вы запустили приложения после удаления базы данных и до выполнения `update-database`. Изменяйте, удалить *School.sdf* файл еще раз и повторите попытку `update-database` команды.)

Запустите приложение. Теперь на страницу учащихся пуст, но на странице преподавателей содержит преподавателей. Это, что вы получаете в рабочей среде после развертывания приложения.

![Empty_Students_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image13.png)

![Instructors_page_after_initial_migration](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image14.png)

Теперь проект готов для развертывания *School* базы данных.

## <a name="creating-a-membership-database-for-deployment"></a>Создайте базу данных членства для развертывания

Приложение университета Contoso использует проверки подлинности системы и форм членства ASP.NET для проверки подлинности и авторизации пользователей. Одной из его страниц доступен только для администраторов. Чтобы просмотреть эту страницу, запустите приложение и выберите **обновления кредиты** из всплывающее меню в разделе **курсы**. Приложение отображает **вход** странице, так как только администраторы могут использовать **обновления кредиты** страницы.

[![Log_in_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image15.png)

Войдите в систему как «admin», с помощью пароля «Pas$ w0rd» (Обратите внимание, что нуль вместо буквы «o» в «w0rd»). После входа в систему **обновления кредиты** откроется страница.

[![Update_Credits_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image17.png)

При развертывании узла в первый раз, довольно часто, чтобы исключить большинство или все учетные записи пользователей, созданные для тестирования. В этом случае используется для развертывания учетной записи администратора и учетные записи пользователей отсутствуют. Вместо того чтобы вручную удалить тестовые учетные записи, вы создадите новую базу данных членства, имеет только один администратор учетной записи пользователя, необходимо использовать в рабочей среде.

> [!NOTE]
> Базы данных членства сохраняет хэш паролей учетных записей. Чтобы выполнить развертывание учетных записей с одного компьютера на другой, необходимо убедиться в том, что процедуры хэширования не создавать разных хэшей на конечном сервере, чем на исходном компьютере. Они будут создают хэшам же при использовании универсальные поставщики ASP.NET, до тех пор, пока вы не меняли алгоритм по умолчанию. По умолчанию алгоритм HMACSHA256 и сохраняется в **проверки** атрибут **[machineKey](https://msdn.microsoft.com/library/w8h3skw9.aspx)** в файле Web.config.


Базы данных членства не обслуживается Code First Migrations, и нет нет автоматического инициализатора, заполняющий базу данных с помощью тестовых учетных записей, (как для базы данных School). Таким образом для хранения данных тестирования доступны вы будете создается копия тестовую базу данных перед созданием нового.

В **обозревателе решений**, переименуйте *aspnet.sdf* файл в *приложения\_данных* папку для *aspnet Dev.sdf*. (Не сделать копию, просто переименуйте его, вы создадите новую базу данных позже.)

В **обозревателе решений**, убедитесь, что выбран веб-проекта (ContosoUniversity, не ContosoUniversity.DAL). Затем в **проекта** меню, выберите **Конфигурация ASP.NET** для запуска **Web Site Administration Tool**(WAT).

Выберите **безопасности** вкладки.

[![WAT_Security_tab](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image19.png)

Нажмите кнопку **Создание или управление ролями** и добавьте **администратора** роли.

[![WAT_Create_New_Role](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image21.png)

Вернитесь к **безопасности** щелкните **Create User**, и добавить пользователя «admin» в качестве администратора. Прежде чем нажимать кнопку **Create User** кнопку **Create User** убедитесь, что выбран **администратора** "флажок". Пароль, используемый в этом руководстве — «Pas$ w0rd», и можно ввести любой адрес электронной почты.

[![WAT_Create_User](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image23.png)

Закройте браузер. В **обозревателе решений**, нажмите кнопку "Обновить", чтобы увидеть новые *aspnet.sdf* файла.

![New_aspnet.sdf_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image25.png)

Щелкните правой кнопкой мыши **aspnet.sdf** и выберите **включить в проект**.

## <a name="distinguishing-development-from-production-databases"></a>Отличительный признак разработки из рабочих баз данных

В этом разделе вы переименуете баз данных, чтобы версии разработки School Dev.sdf и aspnet Dev.sdf и рабочей версии являются School Prod.sdf и aspnet Prod.sdf. Это не является обязательным, но это поэтому поможет обеспечить получение путать тестовой и рабочей версии баз данных.

В **обозревателе решений**, нажмите кнопку **обновить** и разверните приложение\_папка данных см. в разделе базы данных School, который был создан ранее, щелкните его правой кнопкой мыши и выберите **включить в проект** .

![Including_School.sdf_in_project](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image26.png)

Переименуйте *aspnet.sdf* для *aspnet Prod.sdf*.

Переименуйте *School.sdf* для *School-Dev.sdf*.

При запуске приложения в Visual Studio, вы не хотите использовать *-Prod* версий файлов базы данных, вы хотите использовать *- Dev* версий. Таким образом, вам необходимо изменить строку подключения в файле Web.config, чтобы они указывали на *- Dev* версии баз данных. (Вы еще не создали файл Prod.sdf учебного заведения, но это нормально, так как Code First создаст эту базу данных в рабочей среде первый раз при запуске приложения существует).

Откройте файл Web.config приложения и найдите строки подключения:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample5.xml)]

Измените «aspnet.sdf» на «aspnet-Dev.sdf» и измените «School.sdf» на «School-Dev.sdf»:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample6.xml?highlight=4-5)]

Ядро базы данных SQL Server Compact и обе базы данных теперь готовы к развертыванию. В следующем учебнике настраивается автоматически *Web.config* преобразования файла для параметров, которые должны быть разными в средах разработки, тестирования и эксплуатации. (Перечислены параметры, которые необходимо изменить строки подключения, но необходимо настроить эти изменения позже при создании профиля публикации).

## <a name="more-information"></a>Дополнительные сведения

Дополнительные сведения о NuGet см. в разделе [Управление библиотеками проектов с помощью NuGet](https://msdn.microsoft.com/magazine/hh547106.aspx) и [документации по NuGet](http://docs.nuget.org/docs/start-here/overview). Если вы не хотите использовать NuGet, вам потребуется научиться анализировать пакет NuGet, чтобы определить, какие действия происходят при установке. (Например, он может настроить *Web.config* преобразования, настроить сценарии PowerShell для выполнения во время сборки и т. д.) Дополнительные сведения о том, как NuGet работает, см. в разделе особенно [создания и публикации пакета](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) и [файл конфигурации и преобразования исходного кода](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Назад](deployment-to-a-hosting-provider-introduction-1-of-12.md)
> [Вперед](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
