---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
title: Создание хранимых процедур для адаптеров таблиц TableAdapter типизированного DataSet (C#) | Документация Майкрософт
author: rick-anderson
description: В предыдущих учебных курсах мы создали инструкций SQL в коде и передаваемые в базу данных, для выполнения инструкций. Альтернативным подходом является использование s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 751282ca-5870-4d66-84e4-6cefae23eb4a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
msc.type: authoredcontent
ms.openlocfilehash: c17cc0bd6cb97ddf21e2f8d6b177b7f97a28575d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368775"
---
<a name="creating-new-stored-procedures-for-the-typed-datasets-tableadapters-c"></a>Создание хранимых процедур для адаптеров таблиц TableAdapter типизированного DataSet (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачать код](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_67_CS.zip) или [скачать PDF](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/datatutorial67cs1.pdf)

> В предыдущих учебных курсах мы создали инструкций SQL в коде и передаваемые в базу данных, для выполнения инструкций. Альтернативный подход — использовать хранимые процедуры, где инструкций SQL предварительно определены в базе данных. В этом руководстве мы Учимся адаптера таблицы мастер создаст новые хранимые процедуры для нас.


## <a name="introduction"></a>Вступление

Уровень доступа к данным (DAL) для этих учебников использующего типизированные наборы DataSet. Как уже говорилось в [создание уровня доступа к данным](../introduction/creating-a-data-access-layer-cs.md) руководстве типизированные наборы данных состоят из строго типизированных таблиц DataTable и адаптеров таблиц. DataTables представляют логические сущности в системе, а интерфейс адаптеров таблиц TableAdapter с основной базы данных для выполнения работы доступа данных. Сюда входят заполнение DataTables с данными, выполнении запросов, возвращающих скалярные данные, и вставка, обновление и удаление записей из базы данных.

Команды SQL, выполняемая адаптеры таблиц может быть либо специализированные инструкции SQL, например `SELECT columnList FROM TableName`, или хранимых процедур. TableAdapters в архитектуре с помощью инструкций SQL ad-hoc. Многие разработчики и Администраторы баз данных, тем не менее, предпочтение отдается хранимых процедур специализированные инструкции SQL для повышения безопасности, удобства поддержки и обновления. Другие ardently предпочитают специализированные инструкции SQL для гибкости. На своем опыте я предпочитать хранимых процедур и нерегламентированных инструкций SQL, но решили использовать специализированные инструкции SQL для упрощения предыдущих учебных курсах.

При определении TableAdapter или добавления новых методов, мастер TableAdapter s позволяет так же, как легко создать новые хранимые процедуры или использовать существующие хранимые процедуры, как в случае использования инструкций SQL ad-hoc. В этом руководстве мы рассмотрим, как автоматическое создание хранимых процедур мастер s адаптера таблицы. В следующем учебном курсе мы рассмотрим способы настройки s методов класса TableAdapter для использования существующего или созданного вручную хранимых процедур.

> [!NOTE]
> См. записи блога Роб Говард [не используйте хранимые процедуры еще?](http://grokable.com/2003/11/dont-use-stored-procedures-yet-must-be-suffering-from-nihs-not-invented-here-syndrome/) и [Frans Bouma](https://weblogs.asp.net/fbouma/) запись в блоге s [хранимые процедуры, плохо, Кэй M?](https://weblogs.asp.net/fbouma/archive/2003/11/18/38178.aspx) для формах спор на преимущества и недостатки хранимые процедуры и ad-hoc SQL.


## <a name="stored-procedure-basics"></a>Основы хранимых процедур

Функции — это конструкция, общими для всех языков программирования. Функция — это набор инструкций, которые выполняются при вызове функции. Функции могут принимать входные параметры и при необходимости может вернуть значение. *[Хранимые процедуры](http://en.wikipedia.org/wiki/Stored_procedure)*  являются конструкциями базы данных, которые во многом сходны с функциями в языках программирования. Хранимая процедура состоит из набора инструкций T-SQL, которые выполняются при вызове хранимой процедуры. Хранимая процедура может принимать от 0 до много входных параметров и может возвращать скалярные значения выходных параметров или чаще всего результирующие наборы из `SELECT` запросов.

> [!NOTE]
> Хранимые процедуры, часто называются sprocs или SPs.


Хранимые процедуры создаются с помощью [ `CREATE PROCEDURE` ](https://msdn.microsoft.com/library/aa258259(SQL.80).aspx) инструкцию T-SQL. Например, следующий сценарий T-SQL создает хранимую процедуру с именем `GetProductsByCategoryID` , принимает один параметр с именем `@CategoryID` и возвращает `ProductID`, `ProductName`, `UnitPrice`, и `Discontinued` поля этих столбцов в `Products` таблицу, которая имеет соответствующую `CategoryID` значение:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample1.sql)]

Когда эта хранимая процедура будет создана, можно вызвать, используя следующий синтаксис:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample2.sql)]

> [!NOTE]
> В следующем учебном курсе мы рассмотрим создание хранимых процедур через Visual Studio IDE. Для этого руководства тем не менее, мы собираемся разрешить мастеру TableAdapter автоматически создавать хранимые процедуры для нас.


Помимо простого возврата данных, хранимых процедурах часто используются для выполнения нескольких команд базы данных в пределах одной транзакции. Хранимая процедура с именем `DeleteCategory`, например, может пройти через `@CategoryID` параметра и выполнения двух `DELETE` инструкций: во-первых, один для удаления связанных продуктов, а вторая — Удаление указанной категории. Несколько инструкций в хранимой процедуре *не* автоматически переносимый в пределах транзакции. Дополнительные команды T-SQL должны выдаваться для обеспечения хранимая процедура преобразования, несколько команд, обрабатываются в виде атомарной операции. Узнаете, как программы-оболочки для хранимой процедуры s команды в пределах области транзакции, в следующем руководстве.

При использовании хранимых процедур в рамках архитектуры, методы доступа к данным s вызова определенной хранимой процедуры вместо выполнения инструкции SQL ad-hoc. Это позволяет централизовать расположение инструкций SQL, выполняемых (в базе данных) вместо того, чтобы ее определение в архитектуру приложения. Такая Централизация пожалуй упрощает поиск, анализ и настроить запросы и обеспечивает гораздо более четкая картина о том, где и как база данных используется.

Дополнительные сведения о хранимой процедуры основы можно найти в источниках, в разделе «Дополнительные материалы» в конце этого руководства.

## <a name="step-1-creating-the-advanced-data-access-layer-scenarios-web-pages"></a>Шаг 1: Создание дополнительных данных доступ уровня сценарии веб-страниц

Прежде чем приступать к нашего обсуждения в слой DAL с использованием хранимых процедур, позвольте s уделим несколько минут созданию страниц ASP.NET в нашем проекте веб-сайта, которые необходимы для этого и нескольких следующих учебных курсах. Начните с добавления новой папки с именем `AdvancedDAL`. Добавьте следующие страницы ASP.NET в этой папке, не забывая связывать каждую с `Site.master` главной страницы:

- `Default.aspx`
- `NewSprocs.aspx`
- `ExistingSprocs.aspx`
- `JOINs.aspx`
- `AddingColumns.aspx`
- `ComputedColumns.aspx`
- `EncryptingConfigSections.aspx`
- `ManagedFunctionsAndSprocs.aspx`


![Добавление страниц ASP.NET учебников сценарии уровень доступа расширенные данные](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image1.png)

**Рис. 1**: Добавление страниц ASP.NET учебников сценарии уровень доступа расширенные данные


Как и в других папках, `Default.aspx` в `AdvancedDAL` папку перечислит учебные курсы в своем разделе. Помните, что `SectionLevelTutorialListing.ascx` пользовательский элемент управления предоставляет следующие функциональные возможности. Поэтому добавьте данный пользовательский элемент управления для `Default.aspx` , перетащив его из обозревателя решений на странице s режиме конструктора.


[![Добавление элемента управления Sectionleveltutoriallisting.ascx к странице Default.aspx](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image3.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image2.png)

**Рис. 2**: добавление `SectionLevelTutorialListing.ascx` для пользовательского элемента управления `Default.aspx` ([Просмотр полноразмерного изображения](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image4.png))


Наконец, добавьте эти страницы как записи для `Web.sitemap` файла. В частности, добавьте следующую разметку после работы с данными в пакетном режиме `<siteMapNode>`:


[!code-xml[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample3.xml)]

После обновления `Web.sitemap`, Отвлекитесь и просмотрите учебный веб-узел в обозревателе. В меню слева теперь есть элементы для Дополнительные руководства сценарии DAL.


![Карта узла теперь включают записи учебников сценариев расширенной DAL](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image5.png)

**Рис. 3**: карта узла теперь включают записи учебников сценариев расширенной DAL


## <a name="step-2-configuring-a-tableadapter-to-create-new-stored-procedures"></a>Шаг 2: Настройка использования TableAdapter для создания новых хранимых процедур

Чтобы продемонстрировать, создание уровня доступа к данным, который использует хранимые процедуры вместо специализированные инструкции SQL, let s создадим новый типизированный набор DataSet в `~/App_Code/DAL` папку с именем `NorthwindWithSprocs.xsd`. Так как мы выполнили шаг с заходом этот процесс подробно в предыдущих учебных курсах, мы будем использовать быстро выполнить шаги ниже. Если при возникновении затруднений или требуется дополнительно пошаговые инструкции с созданием и настройкой типизированный набор DataSet, обращаться к [создание уровня доступа к данным](../introduction/creating-a-data-access-layer-cs.md) руководства.

Добавить новый набор данных в проект, щелкнув правой кнопкой мыши `DAL` папки, выбрав Add New Item и выбрав шаблон набора данных, как показано на рис. 4.


[![Добавить новый типизированный набор данных в проект с именем NorthwindWithSprocs.xsd](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image7.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image6.png)

**Рис. 4**: добавить новый типизированный набор DataSet проекта с именем `NorthwindWithSprocs.xsd` ([Просмотр полноразмерного изображения](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image8.png))


Это будет создать новый типизированный набор DataSet, откройте его конструктор, создать новый адаптер и запустите мастер настройки TableAdapter. Мастер настройки адаптера таблицы s сначала появляется запрос на выбор базы данных для работы с. В раскрывающемся списке должна быть указана строка подключения к базе данных "Борей". Установите этот флажок и нажмите кнопку Далее.

На этом экране Далее мы можем выбрать, каким образом TableAdapter должен обращаться к базе данных. В предыдущих учебных курсах мы выбрали первый вариант, использовать инструкции SQL. Для этого руководства выберите второй вариант, создать новые хранимые процедуры и нажмите кнопку Далее.


[![Указать TableAdpater, чтобы создать новые хранимые процедуры](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image10.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image9.png)

**Рис. 5**: указать TableAdpater, чтобы создать новые хранимые процедуры ([Просмотр полноразмерного изображения](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image11.png))


Так же, как с помощью нерегламентированных инструкций SQL, на следующем шаге мы запрашиваются `SELECT` инструкции для основного запроса адаптера таблицы s. Но вместо использования `SELECT` инструкция ввести, чтобы выполнить запрос ad-hoc напрямую, мастер TableAdapter s создает хранимую процедуру, которая содержит это `SELECT` запроса.

Используйте следующую команду `SELECT` запроса для этого адаптера таблицы:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample4.sql)]


[![Введите запрос SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image13.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image12.png)

**Рис. 6**: введите `SELECT` запроса ([Просмотр полноразмерного изображения](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image14.png))


> [!NOTE]
> Приведенный выше запрос несколько отличается от основной запрос `ProductsTableAdapter` в `Northwind` типизированного набора DataSet. Помните, что `ProductsTableAdapter` в `Northwind` типизированный набор DataSet включает в себя две коррелированные вложенные запросы, чтобы вернуть имя категории и название компании для каждой категории продукта s и поставщика. В следующем подразделе [обновление TableAdapter для использования соединяет](updating-the-tableadapter-to-use-joins-cs.md) руководстве мы рассмотрим добавление связанных данных для этого адаптера таблицы.


Отвлекитесь и нажмите кнопку "Дополнительно". Здесь можно указать, ли мастер должен также создавать insert, update и delete инструкции для TableAdapter, следует ли использовать оптимистичный параллелизм и таблицы данных должны обновляться после операций вставки и обновления. По умолчанию установлен параметр инструкции создать Insert, Update и Delete. Оставьте этот флажок установленным. Для этого руководства используйте оптимистичный параллелизм параметров оставьте флажок снят.

При наличии хранимых процедур, автоматически создаваемые с помощью мастера TableAdapter, похоже, что обновления параметр таблицы данных учитывается. Независимо от того, является ли этот флажок установлен, полученный insert и update хранимых процедур получить только что вставленное или только что обновленную запись, как мы увидим в шаге 3.


![Оставьте инструкций создать Insert, Update и Delete, выбран параметр](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image15.png)

**Рис. 7**: оставьте инструкций создать Insert, Update и Delete, выбран параметр


> [!NOTE]
> Если установлен параметр Использовать оптимистичный параллелизм, мастер будет добавлять дополнительные условия, `WHERE` предложение, запрет обновления при отсутствии изменений в других полях данных. Вернуться к [реализации оптимистичного параллелизма](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) Дополнительные сведения об использовании функции управления TableAdapter s встроенные оптимистичного параллелизма.


После ввода `SELECT` запроса и подтверждения, что установлен инструкций создать Insert, Update и Delete, нажмите кнопку Далее. Этот следующем экране, показанном на рис. 8 предлагает ввести имена хранимых процедур, которые мастер создаст для выбора, вставки, обновления и удаления данных. Эти хранимые процедуры имена для изменения `Products_Select`, `Products_Insert`, `Products_Update`, и `Products_Delete`.


[![Переименование хранимых процедур](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image17.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image16.png)

**Рис. 8**: переименование хранимых процедур ([Просмотр полноразмерного изображения](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image18.png))


Чтобы просмотреть код T-SQL, используемый мастером TableAdapter для создания четырех хранимых процедур, нажмите кнопку Просмотр скрипта SQL. В диалоговом окне Просмотр скрипта SQL можно сохранить скрипт в файл или скопируйте его в буфер обмена.


![Просмотреть скрипт SQL, используемый для создания хранимых процедур](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image19.png)

**Рис. 9**: предварительный просмотр скрипта SQL, используемый для создания хранимых процедур


После именования хранимые процедуры, нажмите кнопку рядом с именем TableAdapter s соответствующие методы. Так же, как при помощи инструкций SQL, ad-hoc мы можем создавать методы, которые заполнить объект DataTable существующие или вернуть его. Также можно указать, следует ли включать TableAdapter непосредственного шаблона DB для вставки, обновления и удаления записей. Оставьте все три флажков, но переименовать метод DataTable для возврата `GetProducts` (как показано на рис. 10).


[![Назовите методы Fill и GetProducts](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image21.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image20.png)

**Рис. 10**: назовите методы `Fill` и `GetProducts` ([Просмотр полноразмерного изображения](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image22.png))


Нажмите кнопку Далее, чтобы просмотреть сводку действий, которые выполнит мастер. Следуйте указаниям мастера, нажав кнопку "Готово". По завершении работы мастера вы вернетесь к s набора данных конструктор, который должен включать `ProductsDataTable`.


[![Конструктор наборов данных s показывает вновь добавленный ProductsDataTable](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image24.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image23.png)

**Рис. 11**: s набора данных конструктор показывает новые добавленные `ProductsDataTable` ([Просмотр полноразмерного изображения](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image25.png))


## <a name="step-3-examining-the-newly-created-stored-procedures"></a>Шаг 3: Проверка вновь созданные хранимые процедуры

Мастеру TableAdapter, который используется на шаге 2, автоматически созданные хранимые процедуры для выбора, вставки, обновления и удаления данных. Эти хранимые процедуры можно просматривать или изменены с помощью Visual Studio, перейдя в проводник по серверам и перейти к папке базы данных s хранимых процедур. Как показано на рис. 12, база данных "Борей" содержит четыре новые хранимые процедуры: `Products_Delete`, `Products_Insert`, `Products_Select`, и `Products_Update`.


![Четыре хранимые процедуры, созданной на шаге 2 можно найти в папке хранимые процедуры базы данных s](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image26.png)

**Рис. 12**: четыре хранимые процедуры, созданной на шаге 2 можно найти в папке хранимые процедуры базы данных s


> [!NOTE]
> Если вы не видите обозреватель серверов, перейдите к меню "Вид" и выберите параметр обозревателя серверов. Если вы не видите продуктах хранимых процедур, добавленных из шага 2, обновите try, щелкнув папку хранимые процедуры и выбрав команду.


Чтобы просмотреть или изменить хранимую процедуру, дважды щелкните его имя в обозревателе серверов или, или, щелкните правой кнопкой мыши на хранимую процедуру и выберите Open. Рис. 13 показан `Products_Delete` хранимой процедуры, в том случае, когда открывается.


[![Хранимые процедуры можно открыть и изменить из среды Visual Studio](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image28.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image27.png)

**Рис. 13**: хранимые процедуры могут быть открыты и изменены из в Visual Studio ([Просмотр полноразмерного изображения](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image29.png))


Содержимое обоих `Products_Delete` и `Products_Select` хранимых процедур, довольно просты. `Products_Insert` И `Products_Update` хранимые процедуры, с другой стороны, гарантирует более тщательного изучения, так как они оба выполняют `SELECT` инструкции после их `INSERT` и `UPDATE` инструкций. Например, следующий запрос SQL составляет `Products_Insert` хранимой процедуры:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample5.sql)]

Хранимая процедура принимает в качестве входных параметров `Products` столбцы, возвращенные `SELECT` запрос, указанный в мастере адаптера таблицы s и эти значения используются в `INSERT` инструкции. Следуя `INSERT` инструкции `SELECT` запроса используется для возврата `Products` значения столбцов (включая `ProductID`) из вновь добавленной записи. Это обновление полезно при добавлении новой записи с помощью шаблона пакетного обновления, автоматически обновляет только что добавленного `ProductRow` экземпляров `ProductID` свойства автоматически измененные инкрементно значения присваивается базой данных.

Следующий код иллюстрирует эту функцию. Он содержит `ProductsTableAdapter` и `ProductsDataTable` для `NorthwindWithSprocs` типизированного набора DataSet. Нового продукта добавляется в базу данных путем создания `ProductsRow` экземпляра, задав его значения и вызове TableAdapter s `Update` метод, передавая `ProductsDataTable`. На внутреннем уровне s TableAdapter `Update` метод перечисляет `ProductsRow` экземпляров в DataTable переданный (в этом примере имеется только один - один мы только что добавили) и выполняет соответствующий вставки, обновления или удаления. В этом случае `Products_Insert` хранимой процедуры, который добавляет новую запись для `Products` таблицы и возвращает сведения о записи новых. `ProductsRow` Экземпляра s `ProductID` значение затем обновляется. После `Update` метод завершения, можно обратиться к только что добавленную запись s `ProductID` значение через `ProductsRow` s `ProductID` свойство.


[!code-csharp[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample6.cs)]

`Products_Update` Хранимой процедуры аналогично также `SELECT` инструкции после ее `UPDATE` инструкции.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample7.sql)]

Обратите внимание, что эта хранимая процедура включает два входных параметра для `ProductID`: `@Original_ProductID` и `@ProductID`. Эта функция позволяет для сценариев, где могут быть изменены первичный ключ. Например в базе данных сотрудников, каждая запись может использовать номер социального страхования сотрудника s как их первичного ключа. Чтобы изменить существующий сотрудника s номер социального страхования, необходимо одновременно указывать новый номер социального страхования и исходного. Для `Products` таблицы, такая функциональность не требуется, поскольку `ProductID` столбец `IDENTITY` столбца и никогда не должен изменяться. На самом деле `UPDATE` инструкции в `Products_Update` t хранимой процедуры включают `ProductID` столбца в списке столбцов. Таким образом, хотя `@Original_ProductID` используется в `UPDATE` инструкцию s `WHERE` предложение, он является избыточным для `Products` таблицы и может быть заменен `@ProductID` параметра. При изменении параметров хранимых процедур s очень важно, что метод или методы адаптера таблицы, использующие этой хранимой процедуры также обновляются.

## <a name="step-4-modifying-a-stored-procedure-s-parameters-and-updating-the-tableadapter"></a>Шаг 4: Изменение параметров хранимых процедур s и обновления адаптера таблицы

Так как `@Original_ProductID` параметра является излишним, позволяют s удалите ее из `Products_Update` полностью хранимой процедуры. Откройте `Products_Update` хранимой процедуры, удалить `@Original_ProductID` параметра и в `WHERE` предложении `UPDATE` инструкции, изменить имя параметра, используемую из `@Original_ProductID` для `@ProductID`. После внесения этих изменений, T-SQL в хранимой процедуре должен выглядеть следующим образом:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample8.sql)]

Чтобы сохранить эти изменения в базу данных, щелкните значок "Сохранить" на панели инструментов или нажмите сочетание клавиш Ctrl + S. На этом этапе `Products_Update` хранимой процедуры не ожидает `@Original_ProductID` входного параметра, но TableAdapter настроен для передачи такие параметры. Вы увидите параметры, будет отправлять TableAdapter `Products_Update` хранимую процедуру путем выбора адаптера таблицы в конструкторе наборов данных, открыть окно "Свойства" и нажав кнопку с многоточием в `UpdateCommand` s `Parameters` коллекции. Откроется редактор коллекции параметров диалоговое окно, показанное на рис. 14.


![Параметры, используемые списки редактор коллекции параметров, передаваемый Products_Update хранимой процедуры](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image30.png)

**Рис. 14**: параметры, используемые списки редактор коллекции параметров, передаваемый `Products_Update` хранимой процедуры


Этот параметр можно удалить отсюда, просто выбрав `@Original_ProductID` параметр из списка членов и нажав кнопку "Удалить".

Кроме того можно обновить параметры, используемые для всех методов, щелкнув в конструкторе и выбрав Настройка. Откроется мастер настройки адаптера таблицы, список хранимые процедуры, используемые для выбора, вставка, обновление и удаление, а также параметры хранимых процедур вы должны получить. Если щелкнуть стрелку раскрывающегося списка обновления можно увидеть `Products_Update` хранимых процедур ожидается входных параметров, который теперь больше не включает в себя `@Original_ProductID` (см. рис. 15). Просто нажмите кнопку "Готово", чтобы автоматически обновить коллекцию параметров, используемого TableAdapter ".


[![Также можно использовать мастер настройки TableAdapter s для обновления коллекции параметров его методов](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image32.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image31.png)

**Рис. 15**: также можно нажать s адаптера таблицы мастер настройки, чтобы обновить коллекцию параметров его методов ([Просмотр полноразмерного изображения](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image33.png))


## <a name="step-5-adding-additional-tableadapter-methods"></a>Шаг 5: Добавление дополнительных TableAdapter методов

Шаг 2 показано при создании нового адаптера таблицы несложно имеют соответствующие хранимые процедуры автоматически. То же самое значение true, добавляя дополнительные методы адаптера таблицы. Чтобы проиллюстрировать это, позволяют добавить s `GetProductByProductID(productID)` метод `ProductsTableAdapter` создана на шаге 2. Этот метод принимает в качестве входных данных `ProductID` значение и получения подробной информации о указанного продукта.

Запустить, щелкнув правой кнопкой мыши в TableAdapter и выбрав добавить запрос в контекстном меню.


![Добавить новый запрос в адаптер таблицы](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image34.png)

**Рис. 16**: Добавление нового запроса адаптера таблицы


Будет запущен мастер настройки запроса TableAdapter, который сначала запросит как TableAdapter должен обращаться к базе данных. Чтобы создать новую хранимую процедуру, вариант создания новой хранимой процедуры и нажмите кнопку Далее.


[![Выберите создать новую хранимую процедуру параметр](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image36.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image35.png)

**Рис. 17**: выберите создать новую хранимую процедуру параметр ([Просмотр полноразмерного изображения](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image37.png))


Далее запрашивает идентифицировать тип запроса для выполнения, будет возвращать набор строк или скалярное значение, или выполните `UPDATE`, `INSERT`, или `DELETE` инструкции. Так как `GetProductByProductID(productID)` метод будет возвращать строку, оставьте SELECT, возвращающий строки выбран и нажмите "Далее".


[![Выберите SELECT, возвращающая строки параметр](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image39.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image38.png)

**Рис. 18**: выберите SELECT, возвращающая строки параметра ([Просмотр полноразмерного изображения](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image40.png))


На следующем экране отображается TableAdapter s основного запроса, который просто содержит имя хранимой процедуры (`dbo.Products_Select`). Замените имя хранимой процедуры следующим `SELECT` инструкцию, которая возвращает все поля продуктов для указанного продукта:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample9.sql)]


[![Замените имя хранимой процедуры с помощью запроса SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image42.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image41.png)

**Рис. 19**: замените имя хранимой процедуры с `SELECT` запроса ([Просмотр полноразмерного изображения](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image43.png))


Последующие экрана запросит имя хранимой процедуры, которая будет создана. Введите имя `Products_SelectByProductID` и нажмите кнопку Далее.


[![Имя новой хранимой процедуры Products_SelectByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image45.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image44.png)

**Рис. 20**: имя новая хранимая процедура `Products_SelectByProductID` ([Просмотр полноразмерного изображения](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image46.png))


Последний шаг мастера позволяет изменить метод имена создан, а также указать, следует ли использовать заливку шаблон DataTable, возврата шаблон DataTable или оба. Этот метод, оставьте оба варианта флажок установлен, но переименовать методы, которые `FillByProductID` и `GetProductByProductID`. Нажмите кнопку Далее Сводная информация о том, мастер выполнит и нажмите кнопку Готово, чтобы завершить работу мастера.


[![Переименуйте s методов класса TableAdapter в на FillByProductID и GetProductByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image48.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image47.png)

**Рис. 21**: переименовать s методов класса TableAdapter для `FillByProductID` и `GetProductByProductID` ([Просмотр полноразмерного изображения](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image49.png))


После завершения работы мастера, TableAdapter содержит новый метод, доступный, `GetProductByProductID(productID)` , будет выполняться при вызове `Products_SelectByProductID` хранимую процедуру, созданную ранее. Отвлекитесь и просмотреть этот новую хранимую процедуру в обозревателе серверов, детализируя папку хранимые процедуры и открыв `Products_SelectByProductID` (Если вы не видите его, щелкните правой кнопкой мыши на папку хранимые процедуры и выберите "Обновить").

Обратите внимание, что `SelectByProductID` хранимая процедура принимает `@ProductID` как входной параметр и выполняет `SELECT` оператор, который мы ввели в мастере.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample10.sql)]

## <a name="step-6-creating-a-business-logic-layer-class"></a>Шаг 6: Создание класс слоя бизнес-логики

В этой серии руководств мы strived для поддержания многоуровневой архитектуры, в котором уровень представления данных выполнены все его вызовы в слой бизнес-логики (BLL). Чтобы соответствовать этому проектному, необходимо сначала создать класс BLL для новый типизированный набор DataSet, чтобы мы могли обращаться к данные о продуктах от слоя представления.

Создайте новый файл класса с именем `ProductsBLLWithSprocs.cs` в `~/App_Code/BLL` папку и добавьте в него следующий код:


[!code-csharp[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample11.cs)]

Этот класс имитирует `ProductsBLL` класса семантику из предыдущих учебных курсах, но использует `ProductsTableAdapter` и `ProductsDataTable` объектов из `NorthwindWithSprocs` набора данных. Например, вместо `using NorthwindTableAdapters` инструкцию в начало файла класса как `ProductsBLL` делает, `ProductsBLLWithSprocs` класс использует `using NorthwindWithSprocsTableAdapters`. Аналогичным образом `ProductsDataTable` и `ProductsRow` объекты, используемые в этом классе начинаются с префикса `NorthwindWithSprocs` пространства имен. `ProductsBLLWithSprocs` Класс предоставляет методы, доступ к данным с двумя `GetProducts` и `GetProductByProductID`, и методы для добавления, обновления и удаления экземпляра одного продукта.

## <a name="step-7-working-with-thenorthwindwithsprocsdataset-from-the-presentation-layer"></a>Шаг 7: Работа с`NorthwindWithSprocs`набора данных от слоя представления

На этом этапе мы создали DAL, который использует хранимые процедуры для доступа и изменения основной базы данных. Также мы создавали элементарные BLL с помощью методов для получения всех продуктов или конкретного продукта, а также методы для добавления, обновления и удаления продуктов. Округление в этом руководстве, позволяют s создать страницу ASP.NET, использующего BLL s `ProductsBLLWithSprocs` класс для отображения, обновления и удаления записей.

Откройте `NewSprocs.aspx` странице в `AdvancedDAL` папки и перетащите элемент управления GridView с панели инструментов в конструктор, назовите его `Products`. В GridView выберите s смарт-тег, чтобы привязать его к элементу управления ObjectDataSource с именем `ProductsDataSource`. Настройка ObjectDataSource на использование `ProductsBLLWithSprocs` класса, как показано на рис. 22.


[![Настройка ObjectDataSource на использование класса ProductsBLLWithSprocs](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image51.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image50.png)

**Рис. 22**: Настройка ObjectDataSource для использования `ProductsBLLWithSprocs` класс ([Просмотр полноразмерного изображения](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image52.png))


Стрелку раскрывающегося списка на вкладке "ВЫБЕРИТЕ" доступны два варианта `GetProducts` и `GetProductByProductID`. Поскольку мы хотим отобразить все продукты в GridView, выберите `GetProducts` метод. Раскрывающиеся списки в каждой из вкладок UPDATE, INSERT и DELETE могут иметь только один метод. Убедитесь, что каждый из этих списков раскрывающегося списка его соответствующий метод выбран и нажмите кнопку Готово.

После завершения мастера ObjectDataSource, Visual Studio добавит поля BoundFields и по полю CheckBoxField GridView для полей данных продукта. Включите встроенные редактирования GridView s и удаление компонентов разрешить редактирование и разрешить удаление параметры смарт-тега.


[![Страница содержит элемент управления GridView, редактирования и удаления включена поддержка](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image54.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image53.png)

**Рис. 23**: страница содержит элемент управления GridView с редактирование и удаление включена поддержка ([Просмотр полноразмерного изображения](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image55.png))


Как мы ve описано в предыдущих руководствах по завершении работы мастера ObjectDataSource s, Visual Studio наборы `OldValuesParameterFormatString` свойство с первоначальным\_{0}. Это имя должно использоваться значение по умолчанию {0} в порядке для возможности изменения данных для правильной работы заданных параметров, ожидается с помощью методов в BLL. Таким образом, не забудьте задать `OldValuesParameterFormatString` свойства {0} или полностью удалить свойство из декларативного синтаксиса.

После завершения работы мастера настройки источников данных, включение, изменение и удаление поддержки в GridView и возвращение ObjectDataSource s `OldValuesParameterFormatString` свойства к значению по умолчанию вашей страницы s должна выглядеть следующим образом:


[!code-aspx[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample12.aspx)]

На этом этапе мы может освободить GridView путем настройки интерфейса правки, включая проверку, наличие `CategoryID` и `SupplierID` столбцы отображаются в виде элементов управления DropDownList и т. д. Мы также добавить клиентского подтверждения при удалении «удалить», и я рекомендую уделить время для реализации этих улучшений. Поскольку эти темы уже описаны в предыдущих учебных курсах, однако не покрывается их снова здесь.

Независимо от того, ли улучшить GridView, или нет проверьте страницу s основные функции в браузере. Как показано на рис. 24, на странице перечислены продукты, в элементе управления GridView, предоставляющий построчные редактирования и удаления возможности.


[![Продукты можно просмотреть, изменить и удалить из GridView](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image57.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image56.png)

**Рис. 24**: продуктов можно просмотреть, редактирования и удален из GridView ([Просмотр полноразмерного изображения](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image58.png))


## <a name="summary"></a>Сводка

TableAdapters в типизированный набор DataSet может доступ к данным из базы данных, используя специализированные инструкции SQL или с помощью хранимых процедур. При работа с хранимыми процедурами, можно использовать либо существующие хранимые процедуры или мастер TableAdapter может получить указание для создания новых хранимых процедур, на основе `SELECT` запроса. В этом руководстве мы изучили, как у хранимой процедуры, автоматически создан для нас.

При необходимости хранимые процедуры, которые автоматически генерируемым помогает сэкономить время, существует несколько случаев, где хранимая процедура, созданная функцией t мастер соответствуют что мы бы создали самостоятельно. Одним из примеров является `Products_Update` хранимые процедуры, которая ожидается оба `@Original_ProductID` и `@ProductID` входных параметров, даже если `@Original_ProductID` параметр была излишней.

Во многих ситуациях хранимые процедуры уже может быть создана или нужно создавать их вручную, чтобы иметь степень контроля над команды s хранимой процедуры. В любом случае будет необходимо указать адаптер TableAdapter будет использовать существующие хранимые процедуры для своих методов. Как выполнить эту задачу в следующем учебном курсе будет показано.

Счастливого вам программирования!

## <a name="further-reading"></a>Дополнительные сведения

Дополнительные сведения по темам, обсуждавшимся в этом руководстве см. в следующих ресурсах:

- [Создание и обслуживание хранимых процедур](https://msdn.microsoft.com/library/aa214299(SQL.80).aspx)
- [Извлечение скалярных данных из хранимой процедуры](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)
- [Хранимая процедура основы SQL Server](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)
- [Хранимые процедуры: Обзор](http://www.sqlteam.com/item.asp?ItemID=563)
- [Написание хранимой процедуры](http://www.4guysfromrolla.com/webtech/111499-1.shtml)

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. Основной рецензент этого учебного был Hilton Geisenow. Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Вперед](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
