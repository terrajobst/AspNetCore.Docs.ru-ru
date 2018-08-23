---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-cs
title: Создание хранимых процедур и определяемых пользователем функций с помощью управляемого кода (C#) | Документация Майкрософт
author: rick-anderson
description: Microsoft SQL Server 2005 интегрируется с .NET среда CLR позволяет разработчикам для создания объектов базы данных с помощью управляемого кода. Этот учебник...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: 213eea41-1ab4-4371-8b24-1a1a66c515de
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-cs
msc.type: authoredcontent
ms.openlocfilehash: fb4a867d5868e8000fcd10130401a9e169b6f49f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835531"
---
<a name="creating-stored-procedures-and-user-defined-functions-with-managed-code-c"></a>Создание хранимых процедур и определяемых пользователем функций с управляемым кодом (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачать код](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_75_CS.zip) или [скачать PDF](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/datatutorial75cs1.pdf)

> Microsoft SQL Server 2005 интегрируется с .NET среда CLR позволяет разработчикам для создания объектов базы данных с помощью управляемого кода. Этом руководстве демонстрируется создание управляемых хранимых процедур и управляемых определяемых пользователем функций в коде Visual Basic или C#. Мы также видим, как эти выпуски Visual Studio предоставляют возможность отладки таких управляемых объектов базы данных.


## <a name="introduction"></a>Вступление

Использование базы данных, например s Microsoft SQL Server 2005 [Transact-Structured язык запросов (T-SQL)](http://en.wikipedia.org/wiki/Transact-SQL) для вставки, изменения и извлечения данных. Большинство систем базы данных включают конструкции для группирования нескольких инструкций SQL, которые затем могут быть выполнены как единое единый, многократно используемых. Хранимые процедуры являются одним из примеров. Другой — *определяемые пользователем функции*(UDF), это конструкция, будут рассмотрены более подробно на шаге 9.

По существу SQL предназначен для работы с наборами данных. `SELECT`, `UPDATE`, И `DELETE` инструкций по своей природе применяются ко всем записям в соответствующей таблице и ограничиваются только их `WHERE` предложения. Еще есть множество функций языка, для работы с одной записи за раз, а также для управления скалярных данных. [`CURSOR` s](http://www.sqlteam.com/item.asp?ItemID=553) разрешить для набора записей, их следует зациклить по одной за раз. Строковые функции обработки, таких как `LEFT`, `CHARINDEX`, и `PATINDEX` работают с скалярных данных. SQL также включает в себя операторах потока управления, такие как `IF` и `WHILE`.

До Microsoft SQL Server 2005 хранимые процедуры и определяемые пользователем функции могли быть заданы только как коллекция инструкций T-SQL. SQL Server 2005, однако была разработана для обеспечения интеграции с [Common Language Runtime (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), который является среда выполнения для всех сборок .NET. Следовательно хранимые процедуры и определяемые пользователем функции в базе данных SQL Server 2005 можно создать с помощью управляемого кода. То есть можно создать хранимую процедуру или определяемую пользователем Функцию как метод в класс C#. Благодаря этому эти хранимые процедуры и определяемые пользователем функции использовать функции в .NET Framework и из пользовательских классов.

В этом учебном курсе мы рассмотрим создание управляемых хранимых процедур и определяемых пользователем функций и как интегрировать их в базе данных "Борей". Позвольте s приступить к работе!

> [!NOTE]
> Управляемые объекты базы данных предоставляют некоторые преимущества по сравнению с аналогичными функциями SQL. Основными преимуществами являются набора операторов языков и навыки работы и возможность повторного использования существующего кода и логики. Но управляемых объектов базы данных может привести к снижению эффективности при работе с наборами данных, не включающие много процедурной логики. Более глубокое обсуждение преимущества использования управляемого кода и T-SQL, ознакомьтесь с [преимущества использования управляемого кода для создания объектов баз данных](https://msdn.microsoft.com/library/k2e1fb36(VS.80).aspx).


## <a name="step-1-moving-the-northwind-database-out-ofappdata"></a>Шаг 1: Перемещение базы данных "Борей" Out of`App_Data`

Всех наших учебниках до сих использовали файл базы данных Microsoft SQL Server 2005 Express Edition в s приложения web `App_Data` папки. Размещение базы данных в `App_Data` упрощенное распространение и под управлением этих учебников, так как все файлы находились в одном каталоге и требуется не дополнительные действия по настройке для тестирования в руководстве.

Для этого руководства, однако позволяют s перемещать базы данных Northwind из `App_Data` и явно зарегистрировать его с экземпляром базы данных SQL Server 2005 Express Edition. Хотя мы можно выполнить действия в этом руководстве с базой данных в `App_Data` папки, некоторые действия выполняются гораздо проще, явная регистрация базы данных с экземпляром базы данных SQL Server 2005 Express Edition.

Загрузки, в этом руководстве содержатся файлы двух баз данных - `NORTHWND.MDF` и `NORTHWND_log.LDF` - помещено в папку с именем `DataFiles`. Если вы работаете в собственной реализации учебники, закройте Visual Studio и переместите `NORTHWND.MDF` и `NORTHWND_log.LDF` файлы на веб-сайте s `App_Data` папке за пределами веб-сайта. Когда файлы базы данных были перемещены в другую папку, нам нужно зарегистрировать базу данных "Борей" с экземпляром базы данных SQL Server 2005 Express Edition. Это можно сделать из SQL Server Management Studio. При наличии не - экспресс выпуск SQL Server 2005 на компьютере установлена затем скорее всего уже установлен в Management Studio. Если вы только на компьютере установлен SQL Server 2005 Express Edition, Отвлекитесь и загрузите и установите [Microsoft SQL Server Management Studio Express](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Запустите SQL Server Management Studio. Как показано на рис. 1, запросив какой сервер для подключения к запускает Management Studio. Введите localhost\SQLExpress для имени сервера, выберите проверку подлинности Windows в раскрывающемся списке проверки подлинности и нажмите кнопку Connect.


![Соединиться с экземпляром соответствующей базе данных](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image1.png)

**Рис. 1**: соединиться с экземпляром соответствующей базе данных


После подключения вы ve окно обозревателя объектов будут отображаться сведения об экземпляре базы данных SQL Server 2005 Express Edition, включая базы данных, сведения о безопасности, возможности управления и т. д.

Нам нужно присоединить базу данных "Борей" в `DataFiles` папке (или везде, где вы можете ее перенесли) к экземпляру базы данных SQL Server 2005 Express Edition. Щелкните правой кнопкой мыши на папку базы данных и выберите в контекстном меню параметр присоединения. Откроется диалоговое окно Присоединение баз данных. Нажмите кнопку "Добавить", детализировать углублением соответствующий `NORTHWND.MDF` файла и нажмите кнопку ОК. На этом этапе экран должен выглядеть как показано на рис. 2.


[![Соединиться с экземпляром соответствующей базе данных](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image3.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image2.png)

**Рис. 2**: подключитесь к соответствующему экземпляру базы данных ([Просмотр полноразмерного изображения](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image4.png))


> [!NOTE]
> При подключении к экземпляру SQL Server 2005, экспресс-выпуск с помощью Management Studio в диалоговом окне Присоединение баз данных не позволяет детализировать каталогов профиля пользователя, например «Мои документы». Поэтому убедитесь, что поместить `NORTHWND.MDF` и `NORTHWND_log.LDF` файлов в каталоге профиля не написанный пользователем.


Нажмите кнопку "ОК" присоединить базу данных. Присоединение баз данных диалоговое окно закроется, и обозревателя объектов теперь должно отобразиться только что присоединенные базы данных. Скорее всего "Борей", база данных имеет имя, например `9FE54661B32FDD967F51D71D0D5145CC_LINE ARTICLES\DATATUTORIALS\VOLUME 3\CSHARP\73\ASPNET_DATA_TUTORIAL_75_CS\APP_DATA\NORTHWND.MDF`. Переименуйте базу данных в Northwind, щелкнув правой кнопкой мыши, в базе данных и выбрав Rename.


![Переименовать базу данных "Борей"](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image5.png)

**Рис. 3**: присвоить базе данных "Борей"


## <a name="step-2-creating-a-new-solution-and-sql-server-project-in-visual-studio"></a>Шаг 2: Создание нового решения и проекта SQL Server в Visual Studio

Для создания управляемых хранимых процедур или определяемых пользователем функций в SQL Server 2005 мы напишем хранимых процедур и определяемых пользователем Функций логики как код C# в классе. После написания кода необходимо скомпилировать этот класс в сборку ( `.dll` файл), Зарегистрируйте сборку с базой данных SQL Server и затем создания хранимой процедуры или определяемой пользователем функции объекта в базе данных, который указывает на соответствующий метод в сборка. Эти действия можно будет выполняться вручную. Мы можно создать код в любой текстовый редактор, скомпилировать его из командной строки, используя компилятор C# ([`csc.exe`](https://msdn.microsoft.com/library/ms379563(vs.80).aspx)), зарегистрируйте его в базу данных при помощи [ `CREATE ASSEMBLY` ](https://msdn.microsoft.com/library/ms189524.aspx) команду или из системы управления Studio и добавление хранимой процедуры или определяемой пользователем функции объекту, используя аналогичные средства. К счастью Professional и Team Systems версиях Visual Studio включают тип проекта SQL Server, который позволяет автоматизировать выполнение этих задач. В этом руководстве мы рассмотрим использование типа проекта SQL Server для создания управляемой хранимой процедуры и определяемой пользователем функции.

> [!NOTE]
> Если вы используете Visual Web Developer или стандартный выпуск Visual Studio, необходимо вместо этого используйте ручной. Шаг 13 приведены подробные инструкции по выполнению этих шагов вручную. Я рекомендую прочитать шаги 2 – 12 перед чтением шаг 13, так как они включают важные инструкции по настройке SQL Server, которые должны применяться независимо от того, какие версии Visual Studio вы используете.


Сначала откройте Visual Studio. В меню «Файл» выберите новый проект, чтобы открыть диалоговое окно Новый проект (см. рис. 4). Детализировать углублением тип проекта базы данных, а затем на основе шаблонов, перечисленных в правой части, выберите Создание нового проекта SQL Server. Я решил этого проекта имя `ManagedDatabaseConstructs` и поместить его в решение с именем `Tutorial75`.


[![Создайте новый проект SQL Server](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image7.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image6.png)

**Рис. 4**: создайте новый проект SQL Server ([Просмотр полноразмерного изображения](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image8.png))


Нажмите кнопку "ОК" в диалоговом окне нового проекта, чтобы создать решение и проект SQL Server.

Проекта SQL Server привязывается к определенной базе данных. Следовательно после создания нового проекта SQL Server мы сразу же будет предложено указать эту информацию. Рис. 5 показано диалоговое окно новой ссылки на базу данных, который указан для указания базы данных Northwind, который мы зарегистрировали в экземпляре базы данных SQL Server 2005 Express Edition обратно на шаге 1.


![Связать проект SQL Server с базой данных "Борей"](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image9.png)

**Рис. 5**: связать проект SQL Server с базой данных "Борей"


Для отладки управляемых хранимых процедур и определяемых пользователем функций, мы создадим в этом проекте, необходимо включить поддержку отладки для соединения SQL/CLR. При связывании проекта SQL Server с новой базы данных (как мы делали это на рис. 5), Visual Studio спрашивает нас, если нам нужно включить отладку SQL/CLR на соединение (см. рис. 6). Нажмите "Да".


![Включить отладку SQL/CLR](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image10.png)

**Рис. 6**: включить отладку SQL/CLR


На этом этапе в решение добавлен новый проект SQL Server. Он содержит папку с именем `Test Scripts` -файл с именем `Test.sql`, который используется для отладки управляемых объектов базы данных созданы в проекте. Мы рассмотрим отладки в шаг 12.

Теперь можно добавить новые управляемые хранимые процедуры и определяемые пользователем функции в этот проект, но прежде чем мы позволим s сначала включения нашего существующего веб-приложения в решение. Меню "файл" выберите пункт Добавить и выберите существующий веб-сайт. Перейдите к папке соответствующего веб-сайта и нажмите кнопку ОК. Как показано на рис. 7, это приведет к обновлению в решение два проекта: веб-сайт и `ManagedDatabaseConstructs` проект SQL Server.


![В обозревателе решений теперь включает два проекта](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image11.png)

**Рис. 7**: В обозревателе решений теперь включает два проекта


`NORTHWNDConnectionString` Значение в `Web.config` в настоящее время ссылается `NORTHWND.MDF` файл `App_Data` папки. Поскольку мы удалили эту базу данных с `App_Data` и явно зарегистрированы в экземпляре базы данных SQL Server 2005 Express Edition, необходимо соответствующим образом обновить `NORTHWNDConnectionString` значение. Откройте `Web.config` файл на веб-сайта и измените `NORTHWNDConnectionString` значение таким образом, чтобы считывает строку подключения: `Data Source=localhost\SQLExpress;Initial Catalog=Northwind;Integrated Security=True`. После этого изменения вашей `<connectionStrings>` статьи `Web.config` должен выглядеть следующим образом:


[!code-xml[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample1.xml)]

> [!NOTE]
> Как уже говорилось в [предыдущем учебном курсе](debugging-stored-procedures-cs.md), при отладке объект SQL Server из клиентского приложения, например на веб-сайте ASP.NET, необходимо отключить пулы соединений. Строка подключения, показанный выше отключает использование пулов соединений ( `Pooling=false` ). Если вы не планируете отладки управляемых хранимых процедур и определяемых пользователем функций с веб-сайта ASP.NET, включения пула подключений.


## <a name="step-3-creating-a-managed-stored-procedure"></a>Шаг 3: Создание управляемой хранимой процедуры

Чтобы добавить управляемой хранимой процедуры в базе данных Northwind, сначала необходимо создать хранимую процедуру как метод в проект SQL Server. В обозревателе решений щелкните правой кнопкой мыши `ManagedDatabaseConstructs` имя проекта и выберите Добавление нового элемента. Откроется диалоговое окно Добавление нового элемента, в которой перечислены типы управляемых объектов базы данных, которые могут быть добавлены в проект. Как показано на рис. 8, это включает в себя хранимые процедуры и определяемые пользователем функции, помимо прочего.

Позвольте s начните с добавления хранимую процедуру, которая просто возвращает все продукты, которые больше не поддерживаются. Назовите новый файл хранимой процедуры `GetDiscontinuedProducts.cs`.


[![Добавить новую хранимую процедуру с именем GetDiscontinuedProducts.cs](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image13.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image12.png)

**Рис. 8**: Добавление новой хранимой процедуры с именем `GetDiscontinuedProducts.cs` ([Просмотр полноразмерного изображения](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image14.png))


Это создаст новый файл класса C# со следующим содержимым:


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample2.cs)]

Обратите внимание, что хранимая процедура реализуется как `static` метода в `partial` файл класса с именем `StoredProcedures`. Кроме того `GetDiscontinuedProducts` метод дополняется `SqlProcedure attribute`, который помечает метод как хранимую процедуру.

Следующий код создает `SqlCommand` и устанавливает его `CommandText` для `SELECT` запрос, возвращающий все столбцы из `Products` таблица для продуктов, `Discontinued` поля равно 1. Затем он выполняет команду и отправляет результаты обратно в клиентское приложение. Добавьте следующий код в `GetDiscontinuedProducts` метод.


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample3.cs)]

Все управляемые объекты базы данных имеют доступ к [ `SqlContext` объект](https://msdn.microsoft.com/library/ms131108.aspx) , представляющий контекст вызывающего объекта. `SqlContext` Предоставляет доступ к [ `SqlPipe` объект](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.aspx) через его [ `Pipe` свойство](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlcontext.pipe.aspx). Это `SqlPipe` объект используется для межпроцессного сведения между базой данных SQL Server и вызывающему приложению. Как понятно из названия, [ `ExecuteAndSend` метод](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.executeandsend.aspx) выполняет переданное `SqlCommand` объект и отправляет результаты обратно в клиентское приложение.

> [!NOTE]
> Управляемые объекты базы данных лучше всего подходят для хранимых процедур и определяемых пользователем функций, использующих процедурной логики, а не логику, основанную на наборе. Процедурная логика включает в себя, работа с наборами данных на основе row-by-row или работе со скалярными значениями. `GetDiscontinuedProducts` Мы только что создали, тем не менее, метод включает в себя без процедурной логики. Таким образом он будет в идеале реализован в виде T-SQL, хранимая процедура. Она реализуется как управляемый хранимую процедуру, чтобы выполнить действия, необходимые для создания и развертывания управляемых хранимых процедур.


## <a name="step-4-deploying-the-managed-stored-procedure"></a>Шаг 4: Развертывание управляемой хранимой процедуры

Этот код завершения мы готовы развернуть его в базе данных "Борей". Развертывание проекта SQL Server компилирует код в сборку, регистрирует сборку с базой данных и создает соответствующие объекты в базе данных, связывая их в соответствующие методы в сборке. Точный набор задач, выполняемых с помощью параметра развертывания более точно указано на шаге 13. Щелкните правой кнопкой мыши `ManagedDatabaseConstructs` проекта имя в обозревателе решений и выберите параметр "Развернуть". Тем не менее, приведет к сбою со следующей ошибкой: неправильный синтаксис около «Внешний». Необходимо задать уровень совместимости текущей базы данных на более высокое значение, чтобы включить эту функцию. См. в разделе справки для хранимой процедуры `sp_dbcmptlevel`.

Это сообщение об ошибке возникает при попытке зарегистрировать сборку в базе данных "Борей". Чтобы зарегистрировать сборку с базой данных SQL Server 2005, уровень совместимости базы данных s должно быть присвоено значение 90. По умолчанию новых баз данных SQL Server 2005 с уровнем совместимости 90. Тем не менее базы данных, созданных с помощью Microsoft SQL Server 2000 имеют уровень совместимости по умолчанию 80. Так как базы данных Northwind, изначально была база данных Microsoft SQL Server 2000, ее уровень совместимости 80 в настоящий момент установлена и таким образом, необходимо увеличить до 90 для регистрации управляемых объектов базы данных.

Чтобы обновить уровень совместимости базы данных s, откройте окно нового запроса в среде Management Studio и введите:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample4.sql)]

Щелкните значок "Execute" на панели инструментов для запуска приведенного выше запроса.


[![Измените уровень совместимости базы данных "Борей" s](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image16.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image15.png)

**Рис. 9**: обновления s уровень совместимости базы данных "Борей" ([Просмотр полноразмерного изображения](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image17.png))


После обновления уровень совместимости, повторное развертывание проекта SQL Server. Это время развертывания должна завершиться без ошибок.

Вернуться к SQL Server Management Studio, щелкните правой кнопкой мыши, в базе данных "Борей" в обозревателе объектов и выберите "Обновить". Затем углубиться в папке «программирование» и затем разверните папку сборки. Как показано на рис. 10, в базе данных "Борей" теперь включает сборку, созданную `ManagedDatabaseConstructs` проекта.


![Сборка ManagedDatabaseConstructs является Регистрация выполнена с базой данных "Борей"](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image18.png)

**Рис. 10**: `ManagedDatabaseConstructs` сборка является Регистрация выполнена с базой данных "Борей"


Также разверните папку хранимые процедуры. Вы увидите следующие сведения хранимую процедуру с именем `GetDiscontinuedProducts`. Эта хранимая процедура была создана, процесс развертывания и указывает на `GetDiscontinuedProducts` метод в `ManagedDatabaseConstructs` сборки. Когда `GetDiscontinuedProducts` хранимая процедура выполняется, он, в свою очередь, выполняет `GetDiscontinuedProducts` метод. Так как это управляемая хранимая процедура не может быть изменен в среде Management Studio (поэтому значок замка рядом с именем хранимой процедуры).


![Хранимая процедура GetDiscontinuedProducts, перечисленные в папке хранимых процедур](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image19.png)

**Рис. 11**: `GetDiscontinuedProducts` хранимой процедуры, перечисленные в папке хранимых процедур


По-прежнему необходимо преодолеть, прежде чем управляемой хранимой процедуры можно вызывать одно препятствие: база данных настроена для предотвращения выполнения управляемого кода. Проверить это, откройте новое окно запроса и выполнение `GetDiscontinuedProducts` хранимой процедуры. Вы получите следующее сообщение об ошибке: выполнение пользовательского кода в .NET Framework отключено. Включите параметр конфигурации clr enabled.

Чтобы проверить сведения о конфигурации s базы данных "Борей", введите и выполните команду `exec sp_configure` в окне запроса. Это показывает, что среда clr включена задание в настоящее время имеет значение 0.


[![Включена среда clr параметр установлен в настоящее время для 0](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image21.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image20.png)

**Рис. 12**: включена среда clr установлено в настоящее время значение 0 ([Просмотр полноразмерного изображения](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image22.png))


Обратите внимание, что каждый параметр конфигурации, на рис. 12 четыре значения, перечисленные с ней: минимальное и максимальные значения и конфигурации и выполнения значениями. Чтобы обновить значение конфигурации для параметра включена среда clr, выполните следующую команду:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample5.sql)]

При повторном запуске `exec sp_configure` вы увидите, что приведенная выше инструкция обновлено значение конфигурации среда clr включена параметр s до 1, но что это значение по-прежнему имеет значение 0. Для этого изменения конфигурации вступили в силу потребуется выполнить [ `RECONFIGURE` команда](https://msdn.microsoft.com/library/ms176069.aspx), задающий значения текущему значению конфигурации. Просто введите `RECONFIGURE` в окно запроса и щелкните значок "Выполнить" на панели инструментов. При запуске `exec sp_configure` теперь должна отображается значение 1 в параметре конфигурации среда clr включена параметр s и выполните значения.

После завершения настройки включена среда clr, мы готовы к выполнению управляемого `GetDiscontinuedProducts` хранимой процедуры. В окне запроса введите и выполните команду `exec` `GetDiscontinuedProducts`. Вызов хранимой процедуры вызывает соответствующий управляемый код в `GetDiscontinuedProducts` для выполнения метода. Этот код выдает `SELECT` запрос, чтобы возвратить все продукты, которые не поддерживаются и возвращает данные вызывающему приложению, которая находится на этот экземпляр SQL Server Management Studio. Management Studio получает эти результаты и отображает их в окне результатов.


[![GetDiscontinuedProducts, хранимая процедура возвращает все снятых с производства продуктов](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image24.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image23.png)

**Рис. 13**: `GetDiscontinuedProducts` хранимой процедуры возвращает все неподдерживаемые продукты ([Просмотр полноразмерного изображения](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image25.png))


## <a name="step-5-creating-managed-stored-procedures-that-accept-input-parameters"></a>Шаг 5: Создание управляемых хранимых процедур, которые принимают входные параметры

Многие запросы и хранимые процедуры, мы создали в данных учебных курсах использовали *параметры*. Например, в [создание новых хранимых процедур для адаптеров таблиц TableAdapter типизированного набора DataSet s](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) руководстве мы создали хранимую процедуру с именем `GetProductsByCategoryID` , принят входной параметр с именем `@CategoryID`. Хранимая процедура, то возвращается все продукты, `CategoryID` поля соответствуют значению предоставленного `@CategoryID` параметра.

Для создания управляемой хранимой процедуры, которая принимает входные параметры, просто укажите эти параметры в определении метода s. Чтобы проиллюстрировать это, s позволяют добавить другой управляемой хранимой процедуры для `ManagedDatabaseConstructs` проект с именем `GetProductsWithPriceLessThan`. Это управляемая хранимая процедура принимает входной параметр указания цены и вернет все продукты, `UnitPrice` поля меньше, чем значение параметра s.

Чтобы добавить новую хранимую процедуру в проект, щелкните правой кнопкой мыши `ManagedDatabaseConstructs` имя проекта и добавить новую хранимую процедуру. Назовите файл `GetProductsWithPriceLessThan.cs`. Как мы видели в шаге 3, это создаст новый файл класса C# с помощью метода с именем `GetProductsWithPriceLessThan` в `partial` класс `StoredProcedures`.

Обновление `GetProductsWithPriceLessThan` определение метода s, чтобы он принимал [ `SqlMoney` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.aspx) входной параметр с именем `price` и написать код для выполнения и возврата результатов запроса:


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample6.cs)]

`GetProductsWithPriceLessThan` Определение метода s и код сильно напоминает определение и код `GetDiscontinuedProducts` метод, созданный на шаге 3. Единственные отличия —, `GetProductsWithPriceLessThan` метод принимает в качестве входного параметра (`price`), `SqlCommand` s запроса включает параметр (`@MaxPrice`), и добавляется параметр `SqlCommand` s `Parameters` собираются и присвоено значение `price` переменной.

После добавления этого кода, повторное развертывание проекта SQL Server. Затем вернитесь в SQL Server Management Studio и обновите папку хранимые процедуры. Вы увидите новую запись, `GetProductsWithPriceLessThan`. В окне запросов введите и выполните команду `exec GetProductsWithPriceLessThan 25`, который будет список всех продуктов меньше 25 долл. США, как показано на рис. 14.


[![Отображаются продукты в 25 долл. США](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image27.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image26.png)

**Рис. 14**: отображаются продукты в 25 долл. США ([Просмотр полноразмерного изображения](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image28.png))


## <a name="step-6-calling-the-managed-stored-procedure-from-the-data-access-layer"></a>Шаг 6: Вызов управляемой хранимой процедуры из уровня доступа к данным

На этом этапе мы добавили `GetDiscontinuedProducts` и `GetProductsWithPriceLessThan` управляемых хранимых процедур для `ManagedDatabaseConstructs` проекта и зарегистрировали их с базой данных "Борей" SQL Server. Мы также вызывать эти управляемые хранимые процедуры из SQL Server Management Studio (см. рис. 13 и 14). В порядке для ASP.NET приложения, чтобы использовать эти управляемые хранимые процедуры, тем не менее, необходимо добавить их для доступа к данным и бизнес-логики уровни в архитектуре. На этом этапе мы добавим два новых метода для `ProductsTableAdapter` в `NorthwindWithSprocs` типизированный набор DataSet, в который был изначально создан на [создание новых хранимых процедур для адаптеров таблиц TableAdapter типизированного набора DataSet s](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) руководства. На шаге 7 мы добавим соответствующие методы BLL.

Откройте `NorthwindWithSprocs` типизированного набора DataSet в Visual Studio и начните новый метод для `ProductsTableAdapter` с именем `GetDiscontinuedProducts`. Чтобы добавить новый метод адаптера таблицы, щелкните правой кнопкой мыши на имени s адаптера таблицы в конструкторе и выберите параметр Добавить запрос в контекстном меню.

> [!NOTE]
> Так как мы перешли из базы данных Northwind `App_Data` папку к экземпляру базы данных SQL Server 2005 Express Edition, крайне важно, соответствующую строку подключения в файле Web.config обновлен, чтобы отразить это изменение. На шаге 2 мы обсуждали обновление `NORTHWNDConnectionString` значение в `Web.config`. Если вы забыли сделать это обновление, вы увидите сообщение об ошибке, не удалось добавить запрос. Не удается найти подключение `NORTHWNDConnectionString` для объекта `Web.config` в диалоговом окне при попытке добавить новый метод в TableAdapter. Чтобы устранить эту ошибку, нажмите кнопку ОК, а затем перейдите к `Web.config` и обновить `NORTHWNDConnectionString` значение, как описано на шаге 2. Попробуйте повторно добавить метод в TableAdapter. Это время, он должен работать без ошибок.


Добавление нового метода запускает мастер настройки запроса TableAdapter, который мы использовали много раз в предыдущих учебных курсах. Первым шагом является запрос для указания способа TableAdapter должен доступа к базе данных: через специальный оператор SQL или с помощью новой или существующей хранимой процедуры. Так как мы уже создан и зарегистрирован `GetDiscontinuedProducts` управляемой хранимой процедуры с базой данных, выберите использовать существующие хранимые процедуры параметр и нажмите "Далее".


[![Выберите использование существующей хранимой процедуры, параметр](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image30.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image29.png)

**Рис. 15**: выберите, использовать существующую хранимую процедуру параметр ([Просмотр полноразмерного изображения](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image31.png))


Появится запрос на указание для хранимой процедуры, который будет вызывать метод. Выберите `GetDiscontinuedProducts` управляемая хранимая процедура в раскрывающемся списке и нажмите "Далее".


[![Выберите GetDiscontinuedProducts управляемой хранимой процедуры](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image33.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image32.png)

**Рис. 16**: выберите `GetDiscontinuedProducts` управляемых хранимых процедур ([Просмотр полноразмерного изображения](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image34.png))


Мы затем следует указать, является ли хранимая процедура возвращает строк, одно значение либо значение nothing. Так как `GetDiscontinuedProducts` возвращает набор строк снятый с производства продукт, выберите первый вариант (табличных данных) и нажмите кнопку Далее.


[![Выберите параметр табличных данных](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image36.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image35.png)

**Рис. 17**: выберите параметр табличных данных ([Просмотр полноразмерного изображения](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image37.png))


Окончательный экран можно указать шаблоны доступа к данным, который используется и имена итоговый методов. Оставьте как флажков, так и имя методы `FillByDiscontinued` и `GetDiscontinuedProducts`. Нажмите кнопку Готово, чтобы завершить работу мастера.


[![Имя FillByDiscontinued методы и GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image39.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image38.png)

**Рис. 18**: назовите методы `FillByDiscontinued` и `GetDiscontinuedProducts` ([Просмотр полноразмерного изображения](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image40.png))


Повторите эти шаги для создания методов с именами `FillByPriceLessThan` и `GetProductsWithPriceLessThan` в `ProductsTableAdapter` для `GetProductsWithPriceLessThan` управляемая хранимая процедура.

Рис. 19 показана копия экрана конструктора наборов данных после добавления методы, которые `ProductsTableAdapter` для `GetDiscontinuedProducts` и `GetProductsWithPriceLessThan` управляемых хранимых процедур.


[![ProductsTableAdapter включает новые методы, добавленные на этом шаге](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image42.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image41.png)

**Рис. 19**: `ProductsTableAdapter` включает новый добавлены методы на этом шаге ([Просмотр полноразмерного изображения](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image43.png))


## <a name="step-7-adding-corresponding-methods-to-the-business-logic-layer"></a>Шаг 7: Добавление соответствующих методов уровня бизнес-логики

Теперь, когда мы обновили уровень доступа к данным и включает методы для вызова управляемых хранимых процедур, добавленные в шагах 4 и 5, необходимо добавить соответствующие методы для уровня бизнес-логики. Добавьте следующие два метода в `ProductsBLLWithSprocs` класса:


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample7.cs)]

Оба метода, просто вызовите соответствующий метод DAL и возвращается `ProductsDataTable` экземпляра. `DataObjectMethodAttribute` Разметки над каждым методом вызывает эти методы, которые будут включены в раскрывающемся списке на вкладке "ВЫБЕРИТЕ" мастер настройки источников данных s ObjectDataSource.

## <a name="step-8-invoking-the-managed-stored-procedures-from-the-presentation-layer"></a>Шаг 8: Вызов управляемых хранимых процедур из уровня представления данных

С бизнес-логики и уровни доступа к данным непроверенной поддержка вызова `GetDiscontinuedProducts` и `GetProductsWithPriceLessThan` управляемые хранимые процедуры могут отображаться эти хранимые процедуры результаты с помощью страницы ASP.NET.

Откройте `ManagedFunctionsAndSprocs.aspx` странице в `AdvancedDAL` папке и из панели элементов перетащите элемент управления GridView в конструктор. Набор GridView s `ID` свойства `DiscontinuedProducts` и его смарт-теге, привязать его к элементу управления ObjectDataSource с именем `DiscontinuedProductsDataSource`. Настройте элемент ObjectDataSource для извлечения данных из `ProductsBLLWithSprocs` класс s `GetDiscontinuedProducts` метод.


[![Настройка ObjectDataSource на использование класса ProductsBLLWithSprocs](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image45.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image44.png)

**Рис. 20**: Настройка ObjectDataSource для использования `ProductsBLLWithSprocs` класс ([Просмотр полноразмерного изображения](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image46.png))


[![Выберите метод GetDiscontinuedProducts из раскрывающегося списка на вкладке "ВЫБЕРИТЕ"](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image48.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image47.png)

**Рис. 21**: выберите `GetDiscontinuedProducts` метода из раскрывающегося списка на вкладке "ВЫБЕРИТЕ" ([Просмотр полноразмерного изображения](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image49.png))


Так как эта сетка будет использоваться для только что отображение сведений о продукте, установите раскрывающиеся списки в UPDATE, INSERT и удаление вкладок (нет) и нажмите кнопку Готово.

После завершения работы мастера, Visual Studio автоматически добавит BoundField или CheckBoxField для каждого поля данных в `ProductsDataTable`. Отвлекитесь и удалить все эти поля, за исключением `ProductName` и `Discontinued`, на этой стадии вашей GridView и ObjectDataSource s должна выглядеть следующим образом:


[!code-aspx[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample8.aspx)]

Отвлекитесь и просмотреть эту страницу через обозреватель. При просмотре, элемент управления ObjectDataSource вызывает `ProductsBLLWithSprocs` класс s `GetDiscontinuedProducts` метод. Как мы видели в шаге 7, этот метод вызывает DAL s `ProductsDataTable` класс s `GetDiscontinuedProducts` метод, который вызывает `GetDiscontinuedProducts` хранимой процедуры. Эта хранимая процедура — это управляемая хранимая процедура и выполняет код, созданный на шаге 3, возвращая снятых с производства продуктов.

Результаты, возвращенные управляемой хранимой процедуры упаковываются в `ProductsDataTable` с DAL и затем вернулось к BLL, который затем возвращает их на уровень презентации, где они привязываются к GridView и отображаются. Как и ожидалось, в сетке приведен список продуктов, которых больше не поддерживаются.


[![Неподдерживаемые продукты, перечисленные](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image51.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image50.png)

**Рис. 22**: неподдерживаемые продукты перечислены ([Просмотр полноразмерного изображения](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image52.png))


Для практического занятия Добавьте текстовое поле и другой GridView на страницу. У этого GridView отображения продуктов меньше, чем количество, введенный в текстовое поле, вызвав `ProductsBLLWithSprocs` класс s `GetProductsWithPriceLessThan` метод.

## <a name="step-9-creating-and-calling-t-sql-udfs"></a>Шаг 9: Создание и вызов определяемых пользователем функций T-SQL

Определяемые пользователем функции или определяемые пользователем функции, — это объекты базы данных точно соответствуют семантику функции в языках программирования. Как и функция в C# определяемые пользователем функции можно включить переменное число входных параметров и возвращает значение определенного типа. Определяемая пользователем Функция может возвращать либо скалярное данных — строка, целое число и т. д. - или табличных данных. Позволяют быстро просматривать в обоих типов, определяемых пользователем функций, начиная с Определяемой пользователем функции, возвращающий тип скалярных данных s.

Следующие определяемая пользователем Функция Вычисляет расчетное значение инвентаризации для определенного продукта. Это делается с учетом три входных параметра - `UnitPrice`, `UnitsInStock`, и `Discontinued` значений для конкретного продукта — и возвращает значение типа `money`. Вычисляет расчетное значение запасов путем умножения `UnitPrice` по `UnitsInStock`. Неподдерживаемые элементы это значение уменьшается вдвое.


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample9.sql)]

После добавления эту определяемую пользователем Функцию к базе данных, его можно найти через среду Management Studio разверните папку программирования, а затем функции, а затем скалярные функции. Он может использоваться в `SELECT` запрос следующим образом:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample10.sql)]

Я добавил `udf_ComputeInventoryValue` определяемой пользователем функции в базе данных "Борей"; Рис. 23 показан результат выполнения указанных выше `SELECT` запроса при просмотре через Management Studio. Обратите внимание на то, что определяемая пользователем Функция содержится в списке папку скалярные функции в обозревателе объектов.


[![Каждый продукт s инвентаризации значения указан в списке](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image54.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image53.png)

**Рис. 23**: s каждого продукта перечислены значения инвентаризации ([Просмотр полноразмерного изображения](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image55.png))


Определяемые пользователем функции могут также возвращать табличные данные. Например мы можем создать определяемую пользователем Функцию, которое возвращает продукты, принадлежащие к определенной категории:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample11.sql)]

`udf_GetProductsByCategoryID` Определяемая пользователем Функция принимает `@CategoryID` входной параметр и возвращает результаты заданного `SELECT` запроса. После создания эту определяемую пользователем Функцию можно ссылаться в `FROM` (или `JOIN`) предложение `SELECT` запроса. Следующий пример возвращает `ProductID`, `ProductName`, и `CategoryID` значения для каждого из напитков.


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample12.sql)]

Я добавил `udf_GetProductsByCategoryID` определяемой пользователем функции в базе данных "Борей"; Рис. 24 показан результат выполнения указанных выше `SELECT` запроса при просмотре через Management Studio. Определяемые пользователем функции, возвращающие табличные данные можно найти в папке возвращающие табличное значение функции s обозревателя объектов.


[![ProductID, ProductName и CategoryID указаны для каждого напитков](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image57.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image56.png)

**Рис. 24**: `ProductID`, `ProductName`, и `CategoryID` перечисленные для каждого напитки ([Просмотр полноразмерного изображения](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image58.png))


> [!NOTE]
> Дополнительные сведения о создании и использовании определяемых пользователем функций извлечения [введение в определяемые пользователем функции](http://www.sqlteam.com/item.asp?ItemID=1955). Ознакомьтесь также с [преимуществ и функций Drawbacks of User-Defined](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1).


## <a name="step-10-creating-a-managed-udf"></a>Шаг 10: Создание управляемой функции UDF

`udf_ComputeInventoryValue` И `udf_GetProductsByCategoryID` определяемых пользователем функций, созданных в приведенных выше примерах являются объектами базы данных T-SQL. SQL Server 2005 также поддерживает управляемые определяемые пользователем функции, которые могут добавляться к `ManagedDatabaseConstructs` проект так же, как управляемых хранимых процедур из шаги 3 и 5. Для выполнения этого шага позволяют реализовать s `udf_ComputeInventoryValue` определяемой пользователем функции в управляемом коде.

Чтобы добавить управляемой функции UDF для `ManagedDatabaseConstructs` проекта, щелкните правой кнопкой мыши имя проекта в обозревателе решений и выберите Добавление нового элемента. Выберите шаблон, определяемые пользователем в диалоговом окне Add New Item и назовите новый файл определяемой пользователем функции `udf_ComputeInventoryValue_Managed.cs`.


[![Добавьте в проект ManagedDatabaseConstructs управляемой функции UDF](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image60.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image59.png)

**Рис. 25**: Добавление новых управляемых определяемой пользователем функции для `ManagedDatabaseConstructs` проекта ([Просмотр полноразмерного изображения](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image61.png))


Создает шаблон пользовательской функции `partial` класс с именем `UserDefinedFunctions` с помощью метода, имя которого совпадает имя класса файл s (`udf_ComputeInventoryValue_Managed`, в данном экземпляре). Этот метод дополнен с помощью [ `SqlFunction` атрибут](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlfunctionattribute.aspx), который помечает метод как управляемой функции UDF.


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample13.cs)]

`udf_ComputeInventoryValue` Сейчас, метод возвращает [ `SqlString` объект](https://msdn.microsoft.com/library/system.data.sqltypes.sqlstring.aspx) , которая не принимает входные параметры. Необходимо обновить определение метода, чтобы он принимает три входных параметра - `UnitPrice`, `UnitsInStock`, и `Discontinued` — и возвращает `SqlMoney` объекта. Логика для вычисления значения инвентаризации идентична в T-SQL `udf_ComputeInventoryValue` определяемой пользователем функции.


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample14.cs)]

Обратите внимание, что входные параметры метода s определяемой пользователем функции являются соответствующих им типов SQL: `SqlMoney` для `UnitPrice` поле [ `SqlInt16` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlint16.aspx) для `UnitsInStock`, и [ `SqlBoolean` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlboolean.aspx) для `Discontinued`. Эти типы данных зависит от типов, определенных в `Products` таблицы: `UnitPrice` столбец имеет тип `money`, `UnitsInStock` столбец типа `smallint`и `Discontinued` столбец типа `bit`.

Код начинается с создания `SqlMoney` экземпляр с именем `inventoryValue` , присваивается значение 0. `Products` Table допускает для базы данных `NULL` значения в `UnitsInPrice` и `UnitsInStock` столбцов. Таким образом, нам нужно для первой проверки см. в разделе, если эти значения содержат `NULL` s, что мы делаем через `SqlMoney` объект s [ `IsNull` свойство](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.isnull.aspx). Если оба `UnitPrice` и `UnitsInStock` отличные`NULL` значения, а затем мы вычисляем `inventoryValue` быть произведение двух. Затем, если `Discontinued` имеет значение true, а затем мы эмпирическое значение.

> [!NOTE]
> `SqlMoney` Объект только позволяет два `SqlMoney` экземпляры будут участвовать в умножении друг с другом. Не поддерживает `SqlMoney` экземпляра будет умножено числового литерала с плавающей запятой. Таким образом чтобы сократить `inventoryValue` мы умножьте его на новый `SqlMoney` экземпляр, который имеет значение 0,5.


## <a name="step-11-deploying-the-managed-udf"></a>Шаг 11: Развертывание управляемой функции UDF

После создания управляемой функции UDF мы готовы развернуть его в базе данных "Борей". Как мы видели в шаге 4, управляемых объектов проекта SQL Server развертываются, щелкнув правой кнопкой мыши имя проекта в обозревателе решений и выбрав параметр "Развернуть" в контекстном меню.

После развертывания проекта, вернитесь в SQL Server Management Studio и обновите папку скалярные функции. Теперь вы увидите две записи:

- `dbo.udf_ComputeInventoryValue` -T-SQL определяемая пользователем Функция создан на шаге 9, и
- `dbo.udf ComputeInventoryValue_Managed` -управляемой функции UDF создан на шаге 10, которая только что была развернута.

Чтобы протестировать эту управляемых определяемую пользователем Функцию, выполните следующий запрос в Management Studio:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample15.sql)]

Эта команда использует управляемый `udf ComputeInventoryValue_Managed` определяемой пользователем функции, вместо T-SQL, `udf_ComputeInventoryValue` определяемой пользователем функции, но результат одинаков. См. рисунок 23, чтобы просмотреть снимок экрана выходных данных s определяемой пользователем функции.

## <a name="step-12-debugging-the-managed-database-objects"></a>Шаг 12: Отладки управляемых объектов базы данных

В [отладки хранимых процедур](debugging-stored-procedures-cs.md) учебнике мы рассмотрели три параметры для отладки SQL Server с помощью Visual Studio: прямая отладка базы данных, отладку приложения и отладка проекта SQL Server. Управляемая база данных, объекты нельзя отлаживать с помощью прямой отладки базы данных, но можно отлаживать, из клиентского приложения и непосредственно из проекта SQL Server. Для отладки, однако база данных SQL Server 2005 должны позволять отладку SQL/CLR. Помните, что при первом создании `ManagedDatabaseConstructs` проекта Visual Studio обращались к нам ли мы хотели бы включить отладку (см. рис. 6 в шаге 2) SQL/CLR. Этот параметр можно изменить, щелкнув правой кнопкой мыши, в базе данных из окна обозревателя серверов.


![Убедитесь, что базы данных разрешает отладку SQL/CLR](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image62.png)

**Рис. 26**: Убедитесь, что базы данных разрешает отладку SQL/CLR


Представьте, что мы хотели бы Отладка `GetProductsWithPriceLessThan` управляемая хранимая процедура. Начнем, установив точку останова в коде `GetProductsWithPriceLessThan` метод.


[![Установите точку останова в методе GetProductsWithPriceLessThan](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image64.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image63.png)

**Рис. 27**: установите точку останова в `GetProductsWithPriceLessThan` метод ([Просмотр полноразмерного изображения](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image65.png))


Позвольте s сначала посмотрим отладки управляемых объектов базы данных из проекта SQL Server. Так как наше решение содержит два проекта — `ManagedDatabaseConstructs` проект SQL Server вместе с нашего веб-сайта — Чтобы выполнять отладку из проекта SQL Server, необходимо указать Visual Studio для запуска `ManagedDatabaseConstructs` проект SQL Server, когда мы приступите к отладке. Щелкните правой кнопкой мыши `ManagedDatabaseConstructs` проекта в обозревателе решений и выбрать набор в качестве запускаемого проекта в контекстном меню.

Когда `ManagedDatabaseConstructs` запускается из отладчика, он выполняет инструкции SQL в проект `Test.sql` файл, который находится в `Test Scripts` папку. Например, чтобы проверить `GetProductsWithPriceLessThan` управляемая хранимая процедура замены существующего `Test.sql` файл содержимого с помощью следующей инструкцией, которая вызывает `GetProductsWithPriceLessThan` управляемая хранимая процедура, передавая `@CategoryID` значение 14.95:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample16.sql)]

Когда вы ve ввести приведенный выше сценарий в `Test.sql`, начать отладку, перейдя в меню "Отладка" и выбрав команду Начать отладку, или, нажав клавишу F5 или зеленый значок воспроизведения на панели инструментов. Это будет создавать проекты в решении, развертывание управляемых объектов базы данных в базе данных "Борей" и затем выполните `Test.sql` скрипта. На этом этапе будет достигнута точка останова, и мы можете шаг за шагом `GetProductsWithPriceLessThan` метода анализа значений входных параметров и т. д.


[![В методе GetProductsWithPriceLessThan точки останова](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image67.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image66.png)

**Рис. 28**: точки останова в `GetProductsWithPriceLessThan` метод был нажат ([Просмотр полноразмерного изображения](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image68.png))


Чтобы объект базы данных SQL для отладки через клиентское приложение крайне важно, что база данных быть настроена для поддержки отладки приложения. Щелкните правой кнопкой мыши, в базе данных в обозревателе сервера и убедитесь, что установлен параметр отладки приложения. Кроме того нам нужно настроить приложение ASP.NET для интеграции с SQL-отладчика и для выключения пула подключений. Эти шаги были подробно рассматривается в шаге 2 [отладки хранимых процедур](debugging-stored-procedures-cs.md) руководства.

После настройки приложения ASP.NET и базы данных, веб-сайта ASP.NET в качестве запускаемого проекта и начните отладку. При посещении страницы, которая вызывает один из управляемых объектов, представляющих точкой останова, приложение будет остановлено, и элемент управления будет вернуть к отладчику, где в пошаговом режиме код как показано на рис. 28.

## <a name="step-13-manually-compiling-and-deploying-managed-database-objects"></a>Шаг 13: Вручную компиляция и развертывание управляемых объектов базы данных

Проекты SQL Server позволяют легко создавать, компиляции и развертывания управляемых объектов базы данных. К сожалению проекты SQL Server доступны только в выпусках Professional и Team системы Visual Studio. Если вы используете Visual Web Developer или стандартный выпуск Visual Studio и хотите использовать управляемые объекты базы данных, необходимо будет вручную создавать и развертывать их. Это включает в себя четыре шага:

1. Создайте файл, содержащий исходный код для управляемого объекта базы данных,
2. Скомпилировать в сборку, объект
3. Зарегистрировать сборку в базе данных SQL Server 2005, и
4. Создайте объект базы данных в SQL Server, который указывает на соответствующий метод в сборке.

Чтобы проиллюстрировать эти задачи, позволяют создать новую s управляемых хранимой процедуры, возвращающей этих продуктов, `UnitPrice` больше, чем указанное значение. Создайте новый файл на компьютере с именем `GetProductsWithPriceGreaterThan.cs` и введите следующий код в файл (можно использовать любой текстовый редактор, Блокнот или Visual Studio для выполнения этой задачи):


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample17.cs)]

Этот код почти идентичен `GetProductsWithPriceLessThan` метод, созданный на шаге 5. Единственные отличия — имена методов, `WHERE` предложение, а также имя параметра, используемого в запросе. Вернитесь в `GetProductsWithPriceLessThan` метод, `WHERE` предложение чтение: `WHERE UnitPrice < @MaxPrice`. Здесь в `GetProductsWithPriceGreaterThan`, мы используем: `WHERE UnitPrice > @MinPrice` .

Теперь нам нужно скомпилировать этот класс в сборку. В командной строке перейдите в каталог, где был сохранен `GetProductsWithPriceGreaterThan.cs` файл и использовать компилятор C# (`csc.exe`) для компиляции в сборку файла класса:


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample18.cmd)]

Если в папку, содержащую `csc.exe` в не в системе s `PATH`, будет необходимо полностью указать ссылку пути, `%WINDOWS%\Microsoft.NET\Framework\version\`, следующим образом:


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample19.cmd)]


[![Скомпилируйте GetProductsWithPriceGreaterThan.cs в сборку](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image70.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image69.png)

**Рис. 29**: компиляция `GetProductsWithPriceGreaterThan.cs` в для сборки ([Просмотр полноразмерного изображения](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image71.png))


`/t` Флаг указывает, что файл C#, класс должен быть скомпилирован в DLL (а не исполняемый файл). `/out` Флаг имя результирующей сборки.

> [!NOTE]
> Вместо компиляции `GetProductsWithPriceGreaterThan.cs` файл класса с помощью командной строки, можно также использовать [Visual C# Express Edition](https://msdn.microsoft.com/vstudio/express/visualcsharp/) или создать отдельный проект библиотеки классов в Visual Studio Standard Edition. S ren Lauritsen Алексей любезно предоставила такого проекта Visual C# Express Edition с кодом для `GetProductsWithPriceGreaterThan` хранимой процедуры и два управляемых хранимых процедур и определяемых пользователем Функций создан в шаге 3, 5 и 10. S ren s проект также содержит команды T-SQL, которые необходимо добавить соответствующие объекты базы данных.


С кодом, скомпилированным в сборку мы готовы к регистрации сборки в базе данных SQL Server 2005. Это можно сделать через T-SQL, с помощью команды `CREATE ASSEMBLY`, или с помощью SQL Server Management Studio. Позвольте s фокус среде Management Studio.

В среде Management Studio разверните папку программирования в базе данных "Борей". Одна из ее подпапке — сборок. Чтобы вручную добавить новую сборку в базу данных, щелкните правой кнопкой мыши в папке «сборки» и выберите новую сборку в контекстном меню. В этом отображаются новые сборки диалоговое окно (см. рис. 30). Нажмите кнопку обзора, выберите `ManuallyCreatedDBObjects.dll` сборки, мы только что скомпилированные и нажмите кнопку ОК, чтобы добавить сборку в базу данных. Вы не увидите `ManuallyCreatedDBObjects.dll` в обозревателе объектов.


[![Добавить сборку ManuallyCreatedDBObjects.dll базы данных](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image73.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image72.png)

**Рис. 30**: добавление `ManuallyCreatedDBObjects.dll` сборки в базу данных ([Просмотр полноразмерного изображения](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image74.png))


![ManuallyCreatedDBObjects.dll отображается в обозревателе объектов](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image75.png)

**Рис. 31**: `ManuallyCreatedDBObjects.dll` отображается в обозревателе объектов


Хотя мы добавили сборки в базу данных Northwind, у нас есть еще позволяет связать хранимую процедуру с `GetProductsWithPriceGreaterThan` метода в сборке. Для этого откройте новое окно запроса и выполните следующий сценарий:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample20.sql)]

Это создает новую хранимую процедуру в базе данных Northwind с именем `GetProductsWithPriceGreaterThan` и связывает его с помощью управляемого метода `GetProductsWithPriceGreaterThan` (который находится в классе `StoredProcedures`, который находится в сборке `ManuallyCreatedDBObjects`).

После выполнения приведенного выше скрипта, обновите папку хранимые процедуры в обозревателе объектов. Вы увидите новую запись хранимой процедуры - `GetProductsWithPriceGreaterThan` -значком блокировки рядом с ним. Чтобы протестировать эту хранимую процедуру, введите и выполните следующий скрипт в окне запроса:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample21.sql)]

Как показано на рис. 32, приведенная выше команда отображает сведения по этим продуктам с `UnitPrice` больше, чем 24,95 долларов США.


[![ManuallyCreatedDBObjects.dll отображается в обозревателе объектов](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image77.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image76.png)

**Рис. 32**: `ManuallyCreatedDBObjects.dll` отображается в обозревателе объектов ([Просмотр полноразмерного изображения](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image78.png))


## <a name="summary"></a>Сводка

Microsoft SQL Server 2005 обеспечивает интеграцию с Common Language Runtime (CLR), который позволяет объектам базы данных создается с использованием управляемого кода. Ранее эти объекты базы данных можно было создавать только с помощью T-SQL, но теперь можно создать эти объекты с помощью языков программирования, как C# .NET. В этом учебном курсе мы создали два управляемых хранимых процедур и управляемой определяемые пользователем функции.

Visual Studio s тип проекта SQL Server упрощает создания, компиляции и развертывания управляемых объектов базы данных. Кроме того она предлагает широкие возможности поддержки отладки. Тем не менее типы проектов SQL Server доступны только в выпусках Professional и Team системы Visual Studio. Для тех, с помощью Visual Web Developer или Standard Edition из Visual Studio, создания, компиляции и шаги по развертыванию необходимо выполнить вручную, как мы видели в шаге 13.

Счастливого вам программирования!

## <a name="further-reading"></a>Дополнительные сведения

Дополнительные сведения по темам, обсуждавшимся в этом руководстве см. в следующих ресурсах:

- [Преимущества и недостатки использования определяемых пользователем функций](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1)
- [Создание объектов SQL Server 2005 в управляемом коде](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Создание триггеров, с помощью управляемого кода в SQL Server 2005](http://www.15seconds.com/issue/041006.htm)
- [Практическое руководство: Создание и запуск CLR SQL Server хранимой процедуры](https://msdn.microsoft.com/library/5czye81z(VS.80).aspx)
- [Практическое руководство: Создание и запуск определяемые пользователем функции CLR SQL Server](https://msdn.microsoft.com/library/w2kae45k(VS.80).aspx)
- [Практическое руководство: Изменение `Test.sql` сценария, выполняемого объекты SQL](https://msdn.microsoft.com/library/ms233682(VS.80).aspx)
- [Введение в работу пользователя определенных функций](http://www.sqlteam.com/item.asp?ItemID=1955)
- [Управляемый код и SQL Server 2005 (видео)](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Справочник по Transact-SQL](https://msdn.microsoft.com/library/aa299742(SQL.80).aspx)
- [Пошаговое руководство: Создание хранимой процедуры в управляемом коде](https://msdn.microsoft.com/library/zxsa8hkf(VS.80).aspx)

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. Основной рецензент этого учебного был S ren Алексей Lauritsen. В дополнение к рецензирование этой статьи, S ren также создать проект Visual C# Express Edition, состава данной загрузки статьи s для ручной компиляции управляемых объектов базы данных. Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](debugging-stored-procedures-cs.md)
> [Вперед](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
