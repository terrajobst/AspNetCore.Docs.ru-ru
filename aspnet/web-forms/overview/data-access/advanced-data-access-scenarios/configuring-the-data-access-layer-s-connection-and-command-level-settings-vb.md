---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-vb
title: Настройка параметров подключения и уровня команды на уровня доступа к данным (Visual Basic) | Документация Майкрософт
author: rick-anderson
description: TableAdapters в типизированный набор DataSet автоматически выполняет подключение к базе данных, выполнения команд и Заполнение DataTable с результатами...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: d57dfa2b-d627-45cb-b5b1-abbf3159d770
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-vb
msc.type: authoredcontent
ms.openlocfilehash: 85ec20795b337d5457becc5182b148b03e1554fa
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402071"
---
<a name="configuring-the-data-access-layers-connection--and-command-level-settings-vb"></a>Настройка параметров подключения и уровня команды на уровня доступа к данным (VB)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачать код](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_72_VB.zip) или [скачать PDF](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/datatutorial72vb1.pdf)

> TableAdapters в типизированный набор DataSet автоматически выполнит подключение к базе данных, выполнения команд и Заполнение DataTable с результатами. Существуют случаи, однако когда требуется следить за этими сведениями, сами и в этом руководстве мы узнаем, как получить доступ к параметрам уровня подключения и команды базы данных в TableAdapter.


## <a name="introduction"></a>Вступление

В этой серии руководств мы использовали типизированных наборов DataSet для реализации уровня доступа к данным и бизнес-объектов наших многоуровневой архитектуры. Как уже говорилось в [руководства по использованию](../introduction/creating-a-data-access-layer-vb.md), s типизированный набор DataSet, DataTables служат в качестве репозиториев данных, тогда как адаптеры таблиц выступать в роли оболочки для взаимодействия с базой данных для извлечения и изменения базовых данных. TableAdapters инкапсулируют сложности, применяемыми при работе с базой данных и избавляет от необходимости писать код, чтобы соединиться с базой данных, команда или заполнения результаты в таблицу данных.

Бывают случаи, тем не менее, когда нам требуется burrow вглубь дикой адаптера таблицы и писать код, который работает непосредственно с объектами ADO.NET. В [перенос изменений базы данных в транзакции](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) руководства, например, мы добавили методы TableAdapter для начало, фиксация и откат транзакций в ADO.NET. Эти методы использовать внутренний, созданная `SqlTransaction` объект, который был назначен TableAdapter s `SqlCommand` объектов.

В этом руководстве мы рассмотрим, как получить доступ к параметрам уровня подключения и команды базы данных в TableAdapter. В частности, мы добавим возможность `ProductsTableAdapter` которое обеспечивает доступ к основной строки подключения и параметры времени ожидания команды.

## <a name="working-with-data-using-adonet"></a>Работа с данными, с помощью ADO.NET

Microsoft .NET Framework содержит множество классов, предназначенных специально для работы с данными. Эти классы, найденных в [ `System.Data` пространства имен](https://msdn.microsoft.com/library/system.data.aspx), называются *ADO.NET* классы. Некоторые классы ADO.NET считать привязаны к конкретному *поставщик данных*. Поставщик данных можно считать канала связи, который разрешает отправку информации об между классами ADO.NET и хранилище данных. Существуют обобщенный поставщики, например, OleDb и ODBC, а также поставщиков, которые специально разработаны для определенной базы данных системы. Например хотя можно подключиться к базе данных Microsoft SQL Server, используя зарегистрированный поставщик OleDb, Поставщик SqlClient гораздо эффективнее, так как она была разработана и оптимизирована для SQL Server.

При программном доступе к данным, обычно используется следующий шаблон:

1. Установите подключение к базе данных.
2. Команду.
3. Для `SELECT` запросов, работать с результирующие записи.

Существуют отдельные классы ADO.NET по выполнению каждого из следующих действий. Чтобы подключиться к базе данных с помощью поставщика SqlClient, например, использовать [ `SqlConnection` класс](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(VS.80).aspx). Проблемы `INSERT`, `UPDATE`, `DELETE`, или `SELECT` базы данных, используйте команду [ `SqlCommand` класс](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.aspx).

За исключением [перенос изменений базы данных в транзакции](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) учебнике мы не сталкивались писать ADO.NET низкого уровня код сами, поскольку TableAdapters автоматически созданный код включает функции, необходимые для соединиться с базой данных, команд, получения данных и заполнить эти данные в DataTables. Тем не менее могут возникнуть ситуации, когда нам требуется, чтобы настроить эти параметры низкого уровня. На следующих шагах мы рассмотрим, как в объекты ADO.NET, внутренним классом TableAdapters.

## <a name="step-1-examining-with-the-connection-property"></a>Шаг 1: Проверка с помощью свойства соединения

Каждый класс TableAdapter имеет `Connection` свойство, которое указывает подключение к базе данных. Этот тип данных свойства s и `ConnectionString` значение определяется выбранные в мастере настройки адаптера таблицы. Помните, что если сначала мы добавим TableAdapter к типизированному набору DataSet этот мастер предложит нам для базы данных источника (см. рис. 1). Выберите в раскрывающемся списке на первом этапе включает этих баз данных, указанных в файле конфигурации, а также других баз данных в обозревателе серверов s подключения к данным. Если базы данных, которую мы хотим использовать отсутствует в раскрывающемся списке, подключение к новой базе данных можно задать, щелкнув кнопку новое соединение и предоставляя сведения о соединении, необходимые.


[![На первом шаге мастера настройки адаптера таблицы](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image2.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image1.png)

**Рис. 1**: первый шаг мастера настройки адаптера таблицы ([Просмотр полноразмерного изображения](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image3.png))


Let s Отвлекитесь и проверьте код для TableAdapter s `Connection` свойство. Как отмечалось в [создание уровня доступа к данным](../introduction/creating-a-data-access-layer-vb.md) руководстве TableAdapter автоматически созданный код можно просмотреть, перейдя в окне просмотра классов, детализация соответствующий класс и затем дважды щелкнув имя члена.

Перейдите в окно представления классов, перейдя в меню "Вид" и выбрав представление классов (или введите сочетание клавиш Ctrl + Shift + C). В верхней части окна представления классов, выполнить детализацию для `NorthwindTableAdapters` пространства имен и выберите `ProductsTableAdapter` класса. При этом отобразится `ProductsTableAdapter` s членов в нижней половине представления класса, как показано на рис. 2. Дважды щелкните `Connection` свойство, чтобы увидеть его код.


![Дважды щелкните Свойства соединения в окне просмотра классов, чтобы просмотреть его автоматически созданный код](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image4.png)

**Рис. 2**: дважды щелкните Свойства соединения в окне просмотра классов, чтобы просмотреть его автоматически созданный код


TableAdapter s `Connection` свойства и другие относящиеся к соединению код следующим:


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample1.vb)]

При создании экземпляра класса TableAdapter, переменную-член `_connection` равен `Nothing`. Когда `Connection` обращении к свойству, он сначала проверяет `_connection` переменную-член, который был создан экземпляр. Если нет, `InitConnection` вызывается метод, который создает экземпляр `_connection` и задает его `ConnectionString` присваивается значение строки подключения, указанные в первом шаге мастера s настройки адаптера таблицы.

`Connection` Свойства также могут быть назначены `SqlConnection` объекта. Это связывает новый `SqlConnection` объекта с каждым из TableAdapter s `SqlCommand` объектов.

## <a name="step-2-exposing-connection-level-settings"></a>Шаг 2: Предоставление настройки уровня соединения

Сведения о подключении должны остаются инкапсулированный в TableAdapter и не быть доступны другие слои в архитектуре приложения. Однако возможны сценарии при сведения уровня соединения s TableAdapter должен быть доступен или настраиваемые для запроса, пользователя или страницы ASP.NET.

Позволяют расширить s `ProductsTableAdapter` в `Northwind` набор данных для включения `ConnectionString` свойство, которое может использоваться с уровня бизнес-логики для чтения или измените строку подключения, используемого TableAdapter.

> [!NOTE]
> Объект *строку подключения* является строка, указывающая подключение к базе данных, таких как службу, чтобы использовать расположение базы данных, учетные данные проверки подлинности и другие параметры, относящиеся к базе данных. Список шаблонов строк подключения, используемые широкий набор хранилищ данных и поставщики, см. в разделе [ConnectionStrings.com](http://www.connectionstrings.com/).


Как уже говорилось в [создание уровня доступа к данным](../introduction/creating-a-data-access-layer-vb.md) руководстве типизированный набор DataSet s автоматически генерируемых классов можно расширить посредством использования частичных классов. Во-первых, создайте новую вложенную папку в проекте с именем `ConnectionAndCommandSettings` под `~/App_Code/DAL` папки.


![Добавить вложенную папку с именем ConnectionAndCommandSettings](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image5.png)

**Рис. 3**: добавить вложенную папку с именем `ConnectionAndCommandSettings`


Добавьте новый файл класса с именем `ProductsTableAdapter.ConnectionAndCommandSettings.vb` и введите следующий код:


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample2.vb)]

Этот частичный класс добавляет `Public` свойство с именем `ConnectionString` для `ProductsTableAdapter` класс, который позволяет любой уровень считывать или обновлять строку подключения для TableAdapter s Базовое соединение.

Этот частичный класс для создания (и сохранен), откройте `ProductsBLL` класса. Перейдите к одному из существующих методов и введите `Adapter` и затем нажмите клавишу периода для вызова IntelliSense. Вы должны увидеть новый `ConnectionString` свойство, доступное в IntelliSense, это означает, что можно программно считывать или изменить это значение из BLL.

## <a name="exposing-the-entire-connection-object"></a>Предоставление объекта всего подключения

Этот частичный класс содержит только одно свойство объекта базового соединения: `ConnectionString`. Если вы хотите сделать всего подключения объект доступным за границы TableAdapter, можно также изменить `Connection` свойство s уровень защиты. Автоматически созданный код, который мы рассмотрели в шаге 1 показали, что TableAdapter s `Connection` свойство помечается как `Friend`, это означает, что он может осуществляться только с помощью классов в той же сборке. Это можно изменить, однако с помощью адаптера таблицы s `ConnectionModifier` свойство.

Откройте `Northwind` набора данных, щелкните `ProductsTableAdatper` в конструкторе и перейдите в окно свойств. Вы увидите следующие сведения `ConnectionModifier` присвоено значение по умолчанию `Assembly`. Чтобы сделать `Connection` доступны за пределами сборки s типизированный набор DataSet, изменение свойства `ConnectionModifier` свойства `Public`.


[![Уровень доступности s свойства подключения можно настроить через свойство ConnectionModifier](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image7.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image6.png)

**Рис. 4**: `Connection` свойства специальных возможностей можно настроить уровень s через `ConnectionModifier` свойство ([Просмотр полноразмерного изображения](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image8.png))


Сохранить набор данных и затем вернуться к `ProductsBLL` класса. Как ранее, перейдите к одному из существующих методов и введите `Adapter` и затем нажмите клавишу периода для вызова IntelliSense. Список должен включать `Connection` свойство, это означает, что теперь программно читать или назначить любые настройки уровня соединения из BLL.

## <a name="step-3-examining-the-command-related-properties"></a>Шаг 3: Проверка свойств, связанных с командой

Адаптер таблицы состоит из основного запроса, который, по умолчанию создан `INSERT`, `UPDATE`, и `DELETE` инструкций. Этот основной запрос s `INSERT`, `UPDATE`, и `DELETE` инструкции выполняются в коде адаптера таблицы s как объект адаптера данных ADO.NET с помощью `Adapter` свойство. Как и в его `Connection` свойство, `Adapter` тип данных свойства s определяется поставщик данных, используемый. Так как в этих учебниках используется поставщик SqlClient, `Adapter` свойство имеет тип [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter(VS.80).aspx).

TableAdapter s `Adapter` свойство имеет три свойства типа `SqlCommand` , используемого для проблемы `INSERT`, `UPDATE`, и `DELETE` инструкции:

- `InsertCommand`
- `UpdateCommand`
- `DeleteCommand`

Объект `SqlCommand` объект отвечает за отправку определенного запроса к базе данных и содержит свойства, например: [ `CommandText` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtext.aspx), который содержит специальный оператор SQL или хранимая процедура, выполняющаяся; и [ `Parameters` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.parameters.aspx), который является коллекцией `SqlParameter` объектов. Как мы видели в [создание уровня доступа к данным](../introduction/creating-a-data-access-layer-vb.md) руководстве эти команды объектов можно настроить с помощью окна «Свойства».

В дополнение к его основного запроса адаптера таблицы может включать ряд методов, переменных, при вызове диспетчеризации указанной команды в базу данных. Объект command основного запроса s и командных объектов для всех дополнительных методов, хранятся в TableAdapter s `CommandCollection` свойство.

Let s Отвлекитесь и просмотрите код, сгенерированный `ProductsTableAdapter` в `Northwind` набор данных для этих двух свойств и их вспомогательные переменные-члены и вспомогательные методы:


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample3.vb)]

Код для `Adapter` и `CommandCollection` свойства точно имитирует, `Connection` свойство. Существуют переменные-члены, которые содержат объекты, используемые в свойствах. Свойства `Get` методы доступа начните с проверки для получения соответствующей переменной члена `Nothing`. Если Да, метод инициализации вызывается в том случае, который создает экземпляр переменной-члена и назначает основные свойства, связанных с командой.

## <a name="step-4-exposing-command-level-settings"></a>Шаг 4: Предоставление параметров уровня команды

В идеале сведения на уровне команды должны оставаться инкапсулированный в пределах уровня доступа к данным. Эта информация понадобится на других уровнях архитектуры, тем не менее, его могут предоставляться через разделяемый класс, так же, как с помощью настройки уровня соединения.

Поскольку TableAdapter имеет только один `Connection` свойство, код для предоставления настройки уровня соединения вполне очевиден. Вещей немного более сложны, при изменении параметров уровня команды, поскольку TableAdapter может иметь несколько объектов команды - `InsertCommand`, `UpdateCommand`, и `DeleteCommand`, вместе с переменное число командные объекты в `CommandCollection` свойство. При обновлении параметров уровня команды, эти параметры необходимо распространить на все объекты команды.

Например представьте факт определенных запросов в адаптер таблицы, затраченное на выполнение дополнительной продолжительное время. При использовании адаптера таблицы для выполнения любого из этих запросов, нам может понадобиться увеличить объект команды s [ `CommandTimeout` свойство](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtimeout.aspx). Это свойство указывает количество секунд ожидания выполнения команды и значение по умолчанию — 30.

Чтобы разрешить `CommandTimeout` свойство отрегулировать BLL, добавьте следующий `Public` метод `ProductsDataTable` используйте разделяемого класса, созданный на шаге 2 (`ProductsTableAdapter.ConnectionAndCommandSettings.vb`):


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample4.vb)]

Этот метод может вызываться из BLL или уровень представления данных, чтобы задать время ожидания команды для всех команд проблем управляет этот экземпляр адаптера таблицы.

> [!NOTE]
> `Adapter` И `CommandCollection` свойства отмечены как `Private`, то есть они возможен только из кода в TableAdapter. В отличие от `Connection` свойства, эти модификаторы доступа не могут быть изменены. Таким образом, если вам нужно предоставлять свойства уровня команды на другие уровни в архитектуре необходимо использовать разделяемый класс подход, описанный выше, для предоставления `Public` метод или свойство, которое считывает или записывает `Private` объектов команд.


## <a name="summary"></a>Сводка

TableAdapters в типизированный набор DataSet служат для инкапсуляции сведений о доступе к данным и сложности. Использование TableAdapter, мы не нужно беспокоиться о написании кода ADO.NET для соединения с базой данных, команда или заполнения результаты в таблицу данных. Он все обрабатывается автоматически для нас.

Тем не менее могут возникнуть ситуации, когда нам требуется для настройки низкоуровневых особенностей ADO.NET, например изменение строки подключения или значения времени ожидания соединения или команды по умолчанию. TableAdapter создан `Connection`, `Adapter`, и `CommandCollection` свойства, но они могут быть `Friend` или `Private`, по умолчанию. Это внутренние сведения, которые могут предоставляться путем расширения TableAdapter, используя разделяемые классы для включения `Public` методов или свойств. Кроме того, s TableAdapter `Connection` модификатор доступа свойства можно настроить с помощью адаптера таблицы s `ConnectionModifier` свойство.

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. Burnadette Leigh, S ren Lauritsen Алексей Терезой Мерфи и Хилтон Geisenow стали Лиз Шалок в этом руководстве. Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](working-with-computed-columns-vb.md)
> [Вперед](protecting-connection-strings-and-other-configuration-information-vb.md)
