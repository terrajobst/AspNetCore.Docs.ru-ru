---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: Создание новых хранимых процедур для адаптеров таблиц типизированного набора данных (Visual Basic) | Документы Microsoft
author: rick-anderson
description: В предыдущих учебниках мы создали инструкций SQL в коде и передаваемые в базу данных, для выполнения инструкций. Альтернативным подходом является использование s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: a5a4a9ba-d18d-489a-a6b0-a3c26d6b0274
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: 3df3623f5575a48a22fb1a2c3bc719975a80b9c6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="creating-new-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>Создание новых хранимых процедур для адаптеров таблиц типизированного набора данных (Visual Basic)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить код](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_67_VB.zip) или [скачать PDF](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial67vb1.pdf)

> В предыдущих учебниках мы создали инструкций SQL в коде и передаваемые в базу данных, для выполнения инструкций. Альтернативный подход — использовать хранимые процедуры, где содержатся предварительно определенные в базе данных SQL инструкции. В этом учебнике рассказано, как адаптера таблицы мастер создаст новые хранимые процедуры для нас.


## <a name="introduction"></a>Вступление

Уровень доступа к данным (DAL) для этих учебников используются типизированного набора данных. Как было сказано в [Создание слой доступа к данным](../introduction/creating-a-data-access-layer-vb.md) учебник, типизированные наборы данных состоят из строго типизированные таблицы данных и адаптеров таблиц. DataTables представляют логические объекты в системе во время интерфейс адаптеры таблиц TableAdapter с основной базы данных для выполнения работы доступа к данным. Сюда входят заполнения таблицы данных, с данными, выполнении запросов, возвращающих скалярные данные, и вставка, обновление и удаление записей из базы данных.

Команды SQL, выполняемой адаптеров таблиц может быть либо нерегламентированных инструкций SQL, например `SELECT columnList FROM TableName`, или хранимых процедур. Адаптеры таблиц TableAdapter в нашей архитектуре использовать специализированные инструкции SQL. Многие разработчики и Администраторы баз данных, однако предпочтение хранимых процедур специализированные инструкции SQL из соображений безопасности, удобства обслуживания и возможность обновления. Другие ardently предпочитают нерегламентированных инструкций SQL для гибкости. В свой собственный рабочий я предпочитать хранимые процедуры, нерегламентированные инструкции SQL, но используется для упрощения в предыдущих учебниках нерегламентированных инструкций SQL.

При определении TableAdapter или добавления новых методов, мастер TableAdapter s упрощает так же, как легко создать новые хранимые процедуры или использовать существующие хранимые процедуры, как и использования нерегламентированных инструкций SQL. В этом учебнике мы рассмотрим, как мастер s TableAdapter автоматическое создание хранимых процедур. В следующем уроке мы рассмотрим способы настройки адаптера таблицы методы s использование существующих или созданные вручную хранимых процедур.

> [!NOTE]
> См. записи блога Роб Говард [пункт используйте хранимые процедуры еще не?](http://grokable.com/2003/11/dont-use-stored-procedures-yet-must-be-suffering-from-nihs-not-invented-here-syndrome/) и [Frans Bouma](https://weblogs.asp.net/fbouma/) запись в блоге s [хранимых процедур, плохо Кэй M?](https://weblogs.asp.net/fbouma/archive/2003/11/18/38178.aspx) для живым языком обсуждений на преимуществ и недостатков использования хранимые процедуры и нерегламентированные SQL.


## <a name="stored-procedure-basics"></a>Основы хранимых процедур

Функции — это конструкция, общие для всех языков программирования. Функция является набор инструкций, которые выполняются при вызове функции. Функции могут принимать входные параметры и при необходимости может вернуть значение. *[Хранимые процедуры](http://en.wikipedia.org/wiki/Stored_procedure)*  являются конструкциями базы данных, которые совместно используют много общего с функциями в языках программирования. Хранимая процедура состоит из набора инструкций T-SQL, которые выполняются при вызове хранимой процедуры. Хранимая процедура может принимать ноль для многих входных параметров и может возвращать скалярные значения выходных параметров или чаще всего результирующие наборы из `SELECT` запросов.

> [!NOTE]
> Хранимые процедуры, часто называют sprocs или SPs.


Хранимые процедуры создаются с помощью [ `CREATE PROCEDURE` ](https://msdn.microsoft.com/library/aa258259(SQL.80).aspx) инструкции T-SQL. Например, следующий скрипт T-SQL создает хранимую процедуру с именем `GetProductsByCategoryID` , принимает один параметр с именем `@CategoryID` и возвращает `ProductID`, `ProductName`, `UnitPrice`, и `Discontinued` поля этих столбцов в `Products` таблицу, которая имеет соответствующего `CategoryID` значение:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

Когда эта хранимая процедура будет создана, ее можно вызвать с помощью следующего синтаксиса:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.sql)]

> [!NOTE]
> В следующем уроке мы рассмотрим создание хранимых процедур через интегрированную среду разработки Visual Studio. В этом учебнике тем не менее, мы собираемся адаптера таблицы мастер автоматического создания хранимых процедур для нас.


Помимо простого возврата данных, хранимых процедурах часто используются для выполнения нескольких команд базы данных в пределах одной транзакции. Хранимая процедура с именем `DeleteCategory`, например, может потребоваться в `@CategoryID` параметра и выполнения двух `DELETE` инструкции: во-первых, один для удаления связанных продуктов, а второй — Удаление указанной категории. Несколько инструкций в хранимой процедуре *не* автоматически переносимый в рамках транзакции. Дополнительные команды T-SQL должны быть выданы для обеспечения хранимой процедуры s несколько команд трактуются как атомарную операцию. Мы рассмотрим создание программы-оболочки команды s хранимой процедуры в области действия транзакции в последующих учебника.

При использовании хранимых процедур в архитектуре, методы доступа к данным s вызова определенной хранимой процедуры вместо выполнения инструкции SQL ad-hoc. Это централизует инструкций SQL, выполняемых (в базе данных) вместо ее определение в архитектуре s приложения. Пожалуй централизацию упрощает поиск, анализ и настроить запросы и обеспечивает гораздо более четкая картина о том, где и как база данных используется.

Дополнительные сведения о хранимой процедуре основы обратитесь к ресурсам в разделе "Дополнительные материалы" в конце этого учебника.

## <a name="step-1-creating-the-advanced-data-access-layer-scenarios-web-pages"></a>Шаг 1: Создание дополнительных данных Access слоя сценарии веб-страниц

Прежде чем начать нашего обсуждения о создании DAL, с помощью хранимых процедур, позволяют s сначала занять некоторое время для создания страниц ASP.NET в данном проекте веб-сайта, которые необходимы для этого и следующего несколько учебников. Начните с добавления в новую папку с именем `AdvancedDAL`. Добавьте следующие страницы ASP.NET в этой папке, убедитесь, что связать каждую страницу с `Site.master` главной страницы:

- `Default.aspx`
- `NewSprocs.aspx`
- `ExistingSprocs.aspx`
- `JOINs.aspx`
- `AddingColumns.aspx`
- `ComputedColumns.aspx`
- `EncryptingConfigSections.aspx`
- `ManagedFunctionsAndSprocs.aspx`


![Добавление страницы ASP.NET учебников сценарии уровень доступа дополнительных данных](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**Рис. 1**: Добавление страницы ASP.NET учебников сценарии уровень доступа дополнительных данных


Как и в других папках, `Default.aspx` в `AdvancedDAL` папки будут перечислены учебники в своем разделе. Помните, что `SectionLevelTutorialListing.ascx` пользовательский элемент управления предоставляет следующие функциональные возможности. Таким образом, добавьте этот пользовательский элемент управления для `Default.aspx` , перетащив его из обозревателя решений на странице s представление конструктора.


[![Добавление Default.aspx SectionLevelTutorialListing.ascx пользовательского элемента управления](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)

**На рисунке 2**: добавление `SectionLevelTutorialListing.ascx` пользовательского элемента управления на `Default.aspx` ([Просмотр полноразмерное изображение](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png))


И, наконец, добавьте эти страницы как операции для `Web.sitemap` файла. В частности, добавьте следующую разметку после работы с данными в пакетном режиме `<siteMapNode>`:


[!code-xml[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.xml)]

После обновления `Web.sitemap`, занять некоторое время для просмотра учебников веб-сайт с помощью браузера. Меню слева теперь включает элементы для расширенных сценариев учебники DAL.


![Карта сайта теперь включает записи учебников DAL расширенные сценарии](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)

**Рис. 3**: карта сайта теперь включает записи учебников DAL расширенные сценарии


## <a name="step-2-configuring-a-tableadapter-to-create-new-stored-procedures"></a>Шаг 2: Настройка адаптера таблицы для создания новых хранимых процедур

Чтобы продемонстрировать создание уровня доступа к данным, который использует хранимые процедуры вместо нерегламентированных инструкций SQL, позволяют s создать новый типизированный набор данных в `~/App_Code/DAL` папку с именем `NorthwindWithSprocs.xsd`. Так как мы выполнили шаг с заходом выполняет этот процесс подробно в предыдущих учебниках, мы продолжится быстро выполнить шаги здесь. Если возникли затруднения или требуется дополнительно пошаговые инструкции с созданием и настройкой типизированного набора данных, обращаться к [Создание слой доступа к данным](../introduction/creating-a-data-access-layer-vb.md) учебника.

Добавить новый набор данных в проект, щелкнув правой кнопкой мыши `DAL` папки, выбрав добавить новый элемент и выбирая шаблон набора данных, как показано на рис. 4.


[![Добавить новый типизированный набор данных в проект с именем NorthwindWithSprocs.xsd](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png)

**Рис. 4**: добавить новый типизированный набор данных в проект с именем `NorthwindWithSprocs.xsd` ([Просмотр полноразмерное изображение](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png))


Это будет создать новый типизированный набор данных, откройте его конструктор, создать новый адаптер таблицы и запустите мастер настройки адаптера таблицы. Мастер настройки адаптера таблицы s сначала появляется запрос на выбор базы данных для работы с. Строка подключения к базе данных "Борей" должны быть перечислены в раскрывающемся списке. Выберите этот параметр и нажмите кнопку Далее.

На этом экране Далее можно выбрать, как адаптер следует обращаться к базе данных. В предыдущих учебниках выбран первый вариант, использовать инструкции SQL. В этом учебнике выберите второй вариант, создать новые хранимые процедуры и нажмите кнопку Далее.


[![Указать TableAdpater, чтобы создать новые хранимые процедуры](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png)

**Рис. 5**: поручить TableAdpater, чтобы создать новые хранимые процедуры ([Просмотр полноразмерное изображение](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png))


Так же, как с помощью нерегламентированных инструкций SQL, на следующем этапе мы предлагается ввести `SELECT` инструкция s основного запроса адаптера таблицы. Но вместо `SELECT` введенных напрямую выполнить запрос на нерегламентированные инструкции s адаптера таблицы мастер может создать хранимую процедуру, которая содержит это `SELECT` запроса.

Используйте следующие `SELECT` запроса для этого адаптера таблицы:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]


[![Введите запрос SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png)

**Рис. 6**: введите `SELECT` запроса ([Просмотр полноразмерное изображение](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png))


> [!NOTE]
> Приведенный выше запрос немного отличается от основной запрос `ProductsTableAdapter` в `Northwind` типизированного набора данных. Помните, что `ProductsTableAdapter` в `Northwind` типизированный набор данных содержит двух коррелированных запросов осуществляется вернуть имя категории и название компании для каждой категории продукта s и поставщика. В следующем подразделе [обновление адаптера таблицы для использования соединения](updating-the-tableadapter-to-use-joins-vb.md) учебнике мы рассмотрим это добавление связанные данные для этого адаптера таблицы.


Теперь пора нажмите кнопку "Дополнительно". Здесь можно указать, ли мастер следует также создавать insert, update и delete инструкции для адаптера таблицы, следует ли использовать оптимистическую блокировку и таблицы данных должны обновляться после операции вставки и обновления. По умолчанию установлен параметр инструкции создать инструкции Insert, Update и Delete. Оставьте этот флажок установлен. В этом учебнике не использовать параметры оптимистичного параллелизма флажок.

При наличии хранимых процедур, автоматически создаваемые с помощью мастера TableAdapter, вероятно, обновления данных параметр таблицы учитывается. Независимо от того, установлен ли этот флажок, полученный insert и update хранимых процедур получить только что вставленных или просто обновить запись, что будет рассмотрено в шаге 3.


![Оставьте инструкции создать инструкции Insert, Update и Delete, выбран параметр](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png)

**Рис. 7**: оставьте инструкции создать инструкции Insert, Update и Delete, выбран параметр


> [!NOTE]
> Если установлен параметр использовать оптимистичного параллелизма, мастер будет добавлять дополнительные условия, `WHERE` предложение, запрет обновления в случае обнаружения изменений в другие поля данных. Обращаться к [реализации оптимистичного параллелизма](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) учебника Дополнительные сведения об использовании функции управления TableAdapter s встроенных оптимистичного параллелизма.


После ввода `SELECT` запроса и подтверждения того, что установлен параметр инструкции создать инструкции Insert, Update и Delete, нажмите кнопку Далее. Это следующем экране, показанном на рисунке 8, запрашивает имен хранимых процедур, которые мастер создаст для выбора, вставки, обновления и удаления данных. Эти хранимые процедуры имена для изменения `Products_Select`, `Products_Insert`, `Products_Update`, и `Products_Delete`.


[![Переименование хранимых процедур](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**Рис. 8**: переименование хранимых процедур ([Просмотр полноразмерное изображение](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))


Чтобы просмотреть код T-SQL, адаптера таблицы мастер будет использовать для создания четырех хранимых процедур, нажмите кнопку Предварительный просмотр скрипта SQL. В диалоговом окне Просмотр сценария SQL можно сохранить скрипт в файл или скопировать его в буфер обмена.


![Просмотреть скрипт SQL, используемый для создания хранимых процедур](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**Рис. 9**: предварительный просмотр скрипта SQL, используемый для создания хранимых процедур


После именования хранимые процедуры, нажмите кнопку рядом с именем TableAdapter s соответствующие методы. Так же, как при помощи инструкций SQL, ad-hoc можно создать методы, которые заполнения существующих DataTable или возвращать новый. Также можно указать, следует ли включать TableAdapter шаблон DB Direct для вставки, обновления и удаления записей. Оставьте все три флажка этот флажок установлен, но переименуйте метод DataTable для возврата `GetProducts` (как показано на рис. 10).


[![Назовите методы Fill и GetProducts](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)

**Рис. 10**: имя методы `Fill` и `GetProducts` ([Просмотр полноразмерное изображение](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png))


Нажмите кнопку Далее, чтобы увидеть сводку действий, которые осуществит мастер. Завершите работу мастера, нажав кнопку "Готово". После завершения работы мастера вы вернетесь к s набора данных конструктор, который должен включать `ProductsDataTable`.


[![Вновь добавленный ProductsDataTable показывает s конструктора наборов данных](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)

**Рис. 11**: s набор данных конструктор показывает, что недавно добавленный `ProductsDataTable` ([Просмотр полноразмерное изображение](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png))


## <a name="step-3-examining-the-newly-created-stored-procedures"></a>Шаг 3: Проверка вновь созданные хранимые процедуры

Мастер TableAdapter, используются на шаге 2, автоматически созданные хранимых процедур для выбора, вставки, обновления и удаления данных. Эти хранимые процедуры могут быть просмотрены или изменены с помощью Visual Studio в обозревателе серверов и детализация папку s хранимые процедуры базы данных. Как показано на рис. 12, базы данных Northwind содержит четыре новые хранимые процедуры: `Products_Delete`, `Products_Insert`, `Products_Select`, и `Products_Update`.


![Четыре хранимые процедуры, созданные на шаге 2 можно найти в папке базы данных s хранимых процедур](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)

**Рис. 12**: четырех хранимых процедур, созданных на шаге 2 можно найти в папке базы данных s хранимых процедур


> [!NOTE]
> Если вы не видите обозревателя серверов, перейдите в меню «Вид» и выберите параметр обозревателя серверов. Если отсутствует продуктах хранимые процедуры, добавляемые в шаге 2, обновите try, щелкнув папку хранимые процедуры и выбрав команду.


Чтобы просмотреть или изменить хранимую процедуру, дважды щелкните его имя в обозревателе серверов или, или щелкните правой кнопкой мыши хранимую процедуру и выберите Открыть. На рисунке 13 показано `Products_Delete` хранимой процедуры, при открытии.


[![Хранимые процедуры можно открыть и изменить из среды Visual Studio](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png)

**Рис. 13**: хранимые процедуры могут быть открыты и изменены из в Visual Studio ([Просмотр полноразмерное изображение](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png))


Содержимое обоих `Products_Delete` и `Products_Select` хранимые процедуры довольно просты. `Products_Insert` И `Products_Update` хранимые процедуры с другой стороны, служат основанием для более тщательного изучения, как выполнить оба `SELECT` инструкции после их `INSERT` и `UPDATE` инструкции. Например, следующий запрос SQL составляют `Products_Insert` хранимой процедуры:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

Хранимая процедура принимает в качестве входных параметров `Products` столбцы, возвращенные `SELECT` запрос, указанный в мастере s адаптера таблицы и эти значения используются в `INSERT` инструкции. После `INSERT` инструкции `SELECT` запроса используется для возврата `Products` значения столбцов (включая `ProductID`) новые записи. Это обновление полезно при добавлении новой записи с помощью шаблона пакета обновления, автоматически обновляет только что добавленном `ProductRow` экземпляров `ProductID` свойства с автоматическим приращением значения, назначенные в базе данных.

Следующий код иллюстрирует эту функцию. Он содержит `ProductsTableAdapter` и `ProductsDataTable` для `NorthwindWithSprocs` типизированного набора данных. Новый продукт добавляется в базу данных путем создания `ProductsRow` экземпляра, задав свои значения и вызове TableAdapter s `Update` метод, передавая `ProductsDataTable`. На внутреннем уровне s TableAdapter `Update` метод перечисляет `ProductsRow` экземпляров в DataTable переданный (в этом примере имеется только один - один мы только что добавили) и выполняет соответствующее вставки, обновления или удаления. В этом случае `Products_Insert` выполняется хранимая процедура, добавляющая новую запись для `Products` таблицу и возвращает сведения об записи добавляются. `ProductsRow` Экземпляра s `ProductID` значение затем обновляется. После `Update` метод завершения, можно обратиться к только что добавленную запись s `ProductID` значение через `ProductsRow` s `ProductID` свойство.


[!code-vb[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.vb)]

`Products_Update` Аналогично хранимой процедуры также `SELECT` инструкции после ее `UPDATE` инструкции.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample7.sql)]

Обратите внимание, что эта хранимая процедура включает в себя два входных параметра для `ProductID`: `@Original_ProductID` и `@ProductID`. Эта функция позволяет использовать сценарии, где первичного ключа могут быть изменены. Например в базу данных служащего, каждая запись может служить номер социального страхования сотрудника s их первичного ключа. Чтобы изменить существующий сотрудника s номер социального страхования, необходимо предоставить новый номер социального страхования и исходным. Для `Products` таблицы, такая функция не требуется, поскольку `ProductID` столбец `IDENTITY` столбца и не может быть изменено. На самом деле `UPDATE` инструкции в `Products_Update` t хранимой процедуры включают `ProductID` столбца в своем списке столбцов. Таким образом, хотя `@Original_ProductID` используется в `UPDATE` инструкцию s `WHERE` предложение, он является избыточным для `Products` таблицы и может быть заменено `@ProductID` параметр. При изменении параметров хранимых процедур s важно методы адаптера таблицы, используемые этой хранимой процедуры также обновляются.

## <a name="step-4-modifying-a-stored-procedure-s-parameters-and-updating-the-tableadapter"></a>Шаг 4: Изменение параметров хранимых процедур s и обновления адаптера таблицы

Поскольку `@Original_ProductID` параметра является избыточным, s позволяют удалить его из `Products_Update` полностью хранимой процедуры. Откройте `Products_Update` хранимой процедуры, удалить `@Original_ProductID` параметра и в `WHERE` предложения `UPDATE` инструкции, изменить имя параметра, используемую из `@Original_ProductID` для `@ProductID`. После внесения этих изменений, T-SQL в хранимой процедуре должен выглядеть следующим образом:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample8.sql)]

Чтобы сохранить эти изменения в базу данных, щелкните значок «сохранить» на панели инструментов или нажмите сочетание клавиш Ctrl + S. На этом этапе `Products_Update` хранимой процедуры не ожидает `@Original_ProductID` входного параметра, но адаптера таблицы настроен для передачи такие параметры. Вы увидите параметры адаптера таблицы будет отправлять в `Products_Update` хранимой процедуры, выбрав адаптера таблицы в конструкторе наборов данных, перейдите в окне «Свойства» и нажав кнопку с многоточием в `UpdateCommand` s `Parameters` коллекции. Откроется диалоговое окно Редактор коллекции параметров, показанное на рис. 14.


![Списки параметров редактор коллекции параметров, используемых передаваемый Products_Update хранимой процедуры](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png)

**Рис. 14**: передаваемый списки параметров редактор коллекции параметров, используемых `Products_Update` хранимой процедуры


Здесь можно удалить этот параметр, просто выбрав `@Original_ProductID` параметр из списка элементов и нажмите кнопку "Удалить".

Кроме того можно обновить параметры для всех методов, щелкнув правой кнопкой мыши адаптера таблицы в конструкторе и выбрав Настройка. Откроется мастер настройки адаптера таблицы, хранимые процедуры, используемые для выбора, вставки, обновления, перечисление и удаление вместе с параметрами хранимых процедур бы ожидать получения. Если щелкнуть раскрывающегося списка обновления можно увидеть `Products_Update` хранимых процедур ожидается входных параметров, который теперь больше не включает в себя `@Original_ProductID` (см. рис. 15). Просто нажмите кнопку "Готово", чтобы автоматически обновить коллекцию параметров, используемых в адаптер таблицы ".


[![Кроме того, можно использовать мастер настройки адаптера таблицы s для обновления коллекции параметров его методов](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**Рис. 15**: также можно нажать s адаптера таблицы мастер настройки, чтобы обновить его коллекции параметров методов ([Просмотр полноразмерное изображение](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))


## <a name="step-5-adding-additional-tableadapter-methods"></a>Шаг 5: Добавление дополнительных адаптеров таблиц методов

Шаг 2 показано при создании нового адаптера таблицы это случается, соответствующие хранимые процедуры автоматически созданы. То же самое значение true, при добавлении дополнительных методов для адаптера таблицы. Чтобы проиллюстрировать это, позволяют добавлять s `GetProductByProductID(productID)` метод `ProductsTableAdapter` созданный на шаге 2. Этот метод принимает в качестве входных данных `ProductID` значением и возвращают сведения о продукте, указанного.

Запустить, щелкнув и выбрав команду Добавить запрос в контекстном меню.


![Добавление запроса адаптера таблицы](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**На рисунке 16**: Добавление запроса адаптера таблицы


Будет запущен мастер настройки запроса адаптера таблицы, который сначала запрашивает как TableAdapter следует обращаться к базе данных. Чтобы создать новую хранимую процедуру, выберите команду Создать новый параметр хранимой процедуры и нажмите кнопку Далее.


[![Выберите команду Создать новый параметр хранимой процедуры](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)

**Рисунок 17**: выберите создать новую хранимую процедуру параметр ([Просмотр полноразмерное изображение](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png))


Следующий экран появляется запрос для определения типа запроса на выполнение, она будет вернуть набор строк или скалярное значение или выполнять `UPDATE`, `INSERT`, или `DELETE` инструкции. Поскольку `GetProductByProductID(productID)` метод будет возвращать строку, оставьте SELECT, возвращающая строки выбран и нажмите Далее.


[![Выберите SELECT, возвращающая строки параметра](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)

**На рисунке 18**: выберите SELECT, возвращающая строки параметра ([Просмотр полноразмерное изображение](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png))


На следующем экране отображается запрос основного s TableAdapter, который просто содержит имя хранимой процедуры (`dbo.Products_Select`). Замените имя хранимой процедуры следующим `SELECT` инструкцию, которая возвращает все поля продукта для указанного продукта:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample9.sql)]


[![Замените имя хранимой процедуры запроса SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)

**На рисунке 19**: замените имя хранимой процедуры с `SELECT` запроса ([Просмотр полноразмерное изображение](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png))


Последующие экрана запросит имя хранимой процедуры, которая будет создана. Введите имя `Products_SelectByProductID` и нажмите кнопку Далее.


[![Имя новой хранимой процедуры Products_SelectByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image45.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**Рис. 20**: имя новой хранимой процедуры `Products_SelectByProductID` ([Просмотр полноразмерное изображение](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image46.png))


Последний шаг мастера позволяет изменить метод имена создан, а также укажите, следует ли использовать заливку шаблон DataTable, возвращают шаблон DataTable или оба. Для этого метода, оставьте оба варианта этот флажок установлен, но переименуйте методы для `FillByProductID` и `GetProductByProductID`. Нажмите кнопку Далее, чтобы Сводка шагов мастера выполнит и нажмите кнопку Готово, чтобы завершить работу мастера.


[![Переименуйте методы TableAdapter s FillByProductID и GetProductByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image48.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image47.png)

**Рисунок 21**: переименование s методы адаптера таблицы для `FillByProductID` и `GetProductByProductID` ([Просмотр полноразмерное изображение](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image49.png))


По завершении работы мастера TableAdapter имеет новый метод, доступный, `GetProductByProductID(productID)` , будет выполняться при вызове `Products_SelectByProductID` хранимую процедуру, созданную ранее. Занять некоторое время для просмотра этой новой хранимой процедуры в обозревателе серверов, детализируя папку хранимые процедуры и открыв `Products_SelectByProductID` (Если вы не видите его, щелкните правой кнопкой мыши папку хранимые процедуры и нажмите кнопку обновления).

Обратите внимание, что `SelectByProductID` хранимой процедуры принимает `@ProductID` как входной параметр и выполняет `SELECT` инструкции, мы ввели в мастере.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample10.sql)]

## <a name="step-6-creating-a-business-logic-layer-class"></a>Шаг 6: Создание класс слой бизнес-логики

На протяжении ряда учебника мы strived для поддержания многоуровневой архитектуры, в котором уровень представления внесены все свои вызовы в слой бизнес-логики (BLL). Чтобы соответствуют этому проектному решению, необходимо сначала создать класс уровень бизнес-ЛОГИКИ нового типизированного набора данных, прежде чем можно обратиться к данные о продуктах от уровня представления.

Создать новый файл класса с именем `ProductsBLLWithSprocs.vb` в `~/App_Code/BLL` папку и добавьте в него следующий код:


[!code-vb[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample11.vb)]

Этот класс имитирует `ProductsBLL` класса от более ранних учебники, но использует семантики `ProductsTableAdapter` и `ProductsDataTable` объектов из `NorthwindWithSprocs` набора данных. Например, вместо того, чтобы `Imports NorthwindTableAdapters` инструкции в начале файла класса как `ProductsBLL` так, `ProductsBLLWithSprocs` класс использует `Imports NorthwindWithSprocsTableAdapters`. Аналогичным образом `ProductsDataTable` и `ProductsRow` объектов, используемых в этом классе начинаются с префикса `NorthwindWithSprocs` пространства имен. `ProductsBLLWithSprocs` Класс предоставляет два данных доступ к методам, `GetProducts` и `GetProductByProductID`, и методы для добавления, обновления и удаления экземпляра одного продукта.

## <a name="step-7-working-with-thenorthwindwithsprocsdataset-from-the-presentation-layer"></a>Шаг 7: Работа с`NorthwindWithSprocs`набор данных, уровень представления

На этом этапе мы создали DAL, который использует хранимые процедуры для доступа и изменения основной базы данных. Мы также создали элементарного МЕТОД с методами для получения всех продуктов или конкретного продукта, а также методы для добавления, обновления и удаления продуктов. Округления этого учебника, s позволяют создавать страницы ASP.NET, которая использует уровень бизнес-ЛОГИКИ s `ProductsBLLWithSprocs` класса для отображения, обновления и удаления записей.

Откройте `NewSprocs.aspx` страницы в `AdvancedDAL` папки и перетащите элемент управления GridView с панели элементов в конструктор, присвоив ему имя `Products`. В GridView выберите s смарт-тег, чтобы привязать его к новой ObjectDataSource с именем `ProductsDataSource`. Настройка ObjectDataSource для использования `ProductsBLLWithSprocs` класса, как показано на рис.


[![Настройка ObjectDataSource с помощью класса ProductsBLLWithSprocs](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image51.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image50.png)

**На рисунке 22**: Настройка ObjectDataSource для использования `ProductsBLLWithSprocs` класса ([Просмотр полноразмерное изображение](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image52.png))


Раскрывающегося списка на вкладке «SELECT» имеет два параметра `GetProducts` и `GetProductByProductID`. Поскольку мы хотим отобразить все продукты в GridView, выберите `GetProducts` метод. Раскрывающиеся списки в каждой из вкладок UPDATE, INSERT и DELETE могут иметь только один метод. Убедитесь, что каждый из этих списков раскрывающийся список его соответствующий метод выбран и нажмите кнопку Готово.

После завершения мастера ObjectDataSource Visual Studio добавит стояли и CheckBoxField GridView для полей данных продукта. Включите редактирование встроенных s GridView и удаление компонентов, проверку Разрешить изменение, а также разрешить удаление параметры присутствуют в смарт-тег.


[![Страница содержит элемент управления GridView с редактирование и удаление включена поддержка](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image54.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image53.png)

**На рисунке 23**: страница содержит GridView, редактирование и удаление включена поддержка ([Просмотр полноразмерное изображение](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image55.png))


Как мы хранять описано в предыдущих занятий, после завершения работы мастера s ObjectDataSource Visual Studio задает `OldValuesParameterFormatString` свойство исходный\_{0}. Его необходимо вернуть значение по умолчанию {0} в порядке для работы функций модификации данных неправильно заданных параметров, ожидается с помощью методов в наш уровень бизнес-ЛОГИКИ. Таким образом, не забудьте задать `OldValuesParameterFormatString` свойство {0} или полностью удалить свойство из декларативного синтаксиса.

После завершения работы мастера настройки источника данных, включение, изменение и удаление поддержки в GridView и возвращение ObjectDataSource s `OldValuesParameterFormatString` свойство в значение по умолчанию вашей страницы s должна выглядеть следующим образом:


[!code-aspx[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample12.aspx)]

На этом этапе мы может освободить GridView, настройка интерфейса редактирования, включая проверку, имеющих `CategoryID` и `SupplierID` столбцы отображаются в виде элементами управления DropDownList и т. д. Мы также добавить клиентского подтверждения кнопка "Удалить", и я рекомендую времени для реализации этих расширений. Так как эти разделы были описаны в предыдущих учебниках, однако будет не материале их еще раз.

Независимо от того, улучшить GridView ли или нет Проверьте основные функции s страницы в браузере. Как показано на рис. 24, на странице перечислены продукты в GridView, предоставляющий построчные редактирование и удаление возможностей.


[![Продукты можно просмотреть, изменить и удалить из GridView](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image57.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image56.png)

**Рис. 24**: продукты можно просматривать, редактирования и удалено из GridView ([Просмотр полноразмерное изображение](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image58.png))


## <a name="summary"></a>Сводка

Адаптеры таблиц TableAdapter типизированного набора данных можно доступ к данным из базы данных с помощью нерегламентированных инструкций SQL или с помощью хранимых процедур. При работе с помощью хранимых процедур, можно использовать либо существующие хранимые процедуры или мастер TableAdapter можно также указать для создания новых хранимых процедур, на основе `SELECT` запроса. В этом учебнике мы изучена способа получения хранимые процедуры, автоматически созданные для нас.

При необходимости хранимые процедуры, которые автоматически созданный позволяет сэкономить время, существует несколько случаев, где хранимая процедура, созданная t мастера соответствовать что будет создан на собственный. Одним из примеров является `Products_Update` хранимой процедуры, которая ожидается оба `@Original_ProductID` и `@ProductID` входных параметров, даже если `@Original_ProductID` параметр была излишней.

Во многих сценариях хранимые процедуры уже может быть создана или нужно создавать их вручную, чтобы иметь степень контроля над командами s хранимой процедуры. В любом случае будет необходимо указать адаптера таблицы, чтобы использовать существующие хранимые процедуры для ее методов. Эта процедура рассматривается в следующем уроке будет показано.

Программирование довольны!

## <a name="further-reading"></a>Дополнительные сведения

Дополнительные сведения по темам, рассматриваемые в этом учебнике см. в следующих ресурсах:

- [Создание и обслуживание хранимых процедур](https://msdn.microsoft.com/library/aa214299(SQL.80).aspx)
- [Извлечение скалярных данных из хранимой процедуры](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)
- [SQL Server хранимые процедуры основы](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)
- [Хранимые процедуры: Обзор](http://www.sqlteam.com/item.asp?ItemID=563)
- [Написание хранимой процедуры](http://www.4guysfromrolla.com/webtech/111499-1.shtml)

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основной рецензент этого учебника было Хилтон Geisenow. Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs.md)
> [Вперед](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
