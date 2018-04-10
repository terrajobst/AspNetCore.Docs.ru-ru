---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-cs
title: Создание хранимых процедур и определяемых пользователем функций с помощью управляемого кода (C#) | Документы Microsoft
author: rick-anderson
description: Microsoft SQL Server 2005 интегрируется с .NET Common Language Runtime на предоставление разработчикам возможности создания объектов базы данных с помощью управляемого кода. Этот учебник...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 213eea41-1ab4-4371-8b24-1a1a66c515de
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 5a860c8ab6ad7ff04de2175900491d532db782d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="creating-stored-procedures-and-user-defined-functions-with-managed-code-c"></a>Создание хранимой процедуры и определяемые пользователем функции с помощью управляемого кода (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить код](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_75_CS.zip) или [скачать PDF](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/datatutorial75cs1.pdf)

> Microsoft SQL Server 2005 интегрируется с .NET Common Language Runtime на предоставление разработчикам возможности создания объектов базы данных с помощью управляемого кода. Этот учебник демонстрирует создание управляемых хранимых процедур и управляемых определяемых пользователем функций в коде Visual Basic или C#. Также мы узнаем, как эти выпуски Visual Studio предоставляют возможность отладки таких управляемых объектов базы данных.


## <a name="introduction"></a>Вступление

Использование базы данных, таких как s Microsoft SQL Server 2005 [Transact-Structured языка запросов (T-SQL)](http://en.wikipedia.org/wiki/Transact-SQL) для вставки, изменения и получения данных. Большинство баз данных включают конструкции для группирования нескольких инструкций SQL, которые затем могут быть выполнены как единое единого и повторного использования. Хранимые процедуры являются одним из примеров. Другая — *определяемые пользователем функции*(UDF), это конструкция, будут рассмотрены более подробно в шаге 9.

По существу SQL предназначен для работы с наборами данных. `SELECT`, `UPDATE`, И `DELETE` инструкций по своей природе применяются ко всем записям в соответствующей таблице и ограничено только объемом их `WHERE` предложения. Тем не менее имеют множество функций языка, для работы с одной записи за раз и для работы с данными, скалярные. [`CURSOR` s](http://www.sqlteam.com/item.asp?ItemID=553) позволяют быть в цикле по одному за раз набор записей. Строковые функции обработки как `LEFT`, `CHARINDEX`, и `PATINDEX` работают со скалярными значениями. SQL также включает операторах потока управления, такие как `IF` и `WHILE`.

До Microsoft SQL Server 2005 хранимые процедуры и определяемые пользователем функции могут определяться только как коллекция инструкций T-SQL. SQL Server 2005, однако был разработан для обеспечения интеграции с [Common Language Runtime (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), передаваемый в среде выполнения все сборки .NET. Следовательно хранимые процедуры и определяемые пользователем функции в базе данных SQL Server 2005 могут создаваться с помощью управляемого кода. То есть как метод в классе C# можно создать хранимой процедуры или определяемой пользователем функции. Это позволяет эти хранимые процедуры и определяемые пользователем функции, чтобы использовать функциональные возможности в платформе .NET Framework и из пользовательских классов.

В данном руководстве, мы рассмотрим, как создавать управляемые хранимые процедуры и определяемые пользователем функции и как интегрировать их в базе данных "Борей". S позволяют начать работу!

> [!NOTE]
> Управляемые объекты базы данных обеспечивают ряд преимуществ по сравнению с аналогичными функциями SQL. Набор операторов языка и вы знакомы, а также возможность повторного использования существующего кода и логику перечислены основные преимущества. Но управляемых объектов базы данных могут привести к снижению эффективности при работе с наборами данных, которые не включают в себя много процедурной логики. Более подробные сведения о преимуществах использования управляемого кода или T-SQL, см. [преимущества использования управляемого кода для создания объектов баз данных](https://msdn.microsoft.com/library/k2e1fb36(VS.80).aspx).


## <a name="step-1-moving-the-northwind-database-out-ofappdata"></a>Шаг 1: Перемещение базы данных "Борей" Out of`App_Data`

Все наши руководства сих использовали файл базы данных Microsoft SQL Server 2005 Express Edition в s приложения web `App_Data` папки. Размещение базы данных в `App_Data` упрощает распространение и под управлением этих учебников, как все файлы находились в одном каталоге и необходимые без дополнительных действий по настройке для тестирования в учебнике.

В этом учебнике, однако позволяют s перемещать базы данных Northwind из `App_Data` и зарегистрируйте его явным образом в экземпляре базы данных SQL Server 2005 Express Edition. Хотя мы выполнить действия с базой данных в этом учебнике `App_Data` папки, число шагов выполняются гораздо проще, явная регистрация базы данных с экземпляром базы данных SQL Server 2005 Express Edition.

Загрузка для этого учебника имеет две базы данных файлы - `NORTHWND.MDF` и `NORTHWND_log.LDF` - помещается в папку с именем `DataFiles`. Если вы следуете вместе с собственной реализации учебники, закройте Visual Studio и переместить `NORTHWND.MDF` и `NORTHWND_log.LDF` файлы на веб-сайте s `App_Data` к папке вне веб-сайта. Когда файлы базы данных были перемещены в другую папку необходимо зарегистрировать базу данных "Борей" с экземпляром базы данных SQL Server 2005 Express Edition. Это можно сделать из SQL Server Management Studio. При наличии не - Express Edition SQL Server 2005 на компьютере установлена затем скорее всего уже установлен в Management Studio. Если вы только SQL Server 2005, экспресс-выпуск на компьютере затем занять некоторое время, чтобы загрузить и установить [Microsoft SQL Server Management Studio Express](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Запустите SQL Server Management Studio. Как показано на рис. 1, предлагая какой сервер для подключения к запускает среду Management Studio. Введите localhost\SQLExpress для имени сервера, выберите в раскрывающемся списке проверки подлинности проверку подлинности Windows и щелкните "Подключить".


![Подключитесь к экземпляру соответствующей базы данных](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image1.png)

**Рис. 1**: подключитесь к экземпляру соответствующей базы данных


После подключения можно удалить окно обозревателя объектов будут отображаться сведения об экземпляре базы данных SQL Server 2005 Express Edition, включая базы данных, сведения о безопасности, параметры управления и т. д.

Нам нужно присоединить базу данных "Борей" в `DataFiles` папку (или везде, где возможно была перемещена) к экземпляру базы данных SQL Server 2005 Express Edition. Щелкните правой кнопкой мыши папку базы данных и выберите в контекстном меню параметр присоединения. Откроется диалоговое окно Присоединение баз данных. Нажмите кнопку "Добавить", детализировать углублением соответствующий `NORTHWND.MDF` файл и нажмите кнопку ОК. На этом этапе экран должен выглядеть как на рис.


[![Подключитесь к экземпляру соответствующей базы данных](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image3.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image2.png)

**На рисунке 2**: подключение к соответствующему экземпляру базы данных ([Просмотр полноразмерное изображение](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image4.png))


> [!NOTE]
> При подключении к экземпляру SQL Server 2005, экспресс-выпуск с помощью среды Management Studio в диалоговом окне Присоединение баз данных не позволяют для детализации углублением каталогам профиля пользователя, например Мои документы. Поэтому убедитесь, что поместить `NORTHWND.MDF` и `NORTHWND_log.LDF` файлы в каталоге, не написанный пользователем профиля.


Нажмите кнопку ОК, чтобы присоединить базу данных. В диалоговом окне Присоединение баз данных будет закрыт и обозревателя объектов теперь должны быть перечислены только что присоединенные базы данных. Скорее всего, Northwind, база данных имеет имя, например `9FE54661B32FDD967F51D71D0D5145CC_LINE ARTICLES\DATATUTORIALS\VOLUME 3\CSHARP\73\ASPNET_DATA_TUTORIAL_75_CS\APP_DATA\NORTHWND.MDF`. Переименуйте базу данных Northwind, щелкнув правой кнопкой мыши в базе данных и выбрав переименование.


![Переименовать базу данных Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image5.png)

**Рис. 3**: переименовать базу данных Northwind


## <a name="step-2-creating-a-new-solution-and-sql-server-project-in-visual-studio"></a>Шаг 2: Создание нового решения и проекта SQL Server в Visual Studio

Для создания управляемых хранимых процедур или определяемых пользователем функций в SQL Server 2005 мы напишем хранимых процедур и определяемых пользователем Функций логики в код C# в классе. После написания кода необходимо скомпилировать этот класс в сборке ( `.dll` файл), Зарегистрируйте сборку с базой данных SQL Server, а затем создайте хранимую процедуру или определяемую пользователем Функцию объекта в базе данных, указывающий на соответствующий метод в сборка. Эти шаги можно будет выполняться вручную. Мы создания кода в любой текстовый редактор, ее компиляция из командной строки, с помощью компилятора C# ([`csc.exe`](https://msdn.microsoft.com/library/ms379563(vs.80).aspx)), зарегистрируйте его в базу данных при помощи [ `CREATE ASSEMBLY` ](https://msdn.microsoft.com/library/ms189524.aspx) команду или из среды управления Studio и добавьте хранимой процедуры или определяемой пользователем функции объект аналогичен процедуре. К счастью Professional и команды системы версий Visual Studio включают тип проекта SQL Server, который автоматизирует следующие задачи. В этом учебнике мы рассмотрим с помощью типа проекта SQL Server для создания управляемой хранимой процедуры и определяемой пользователем функции.

> [!NOTE]
> Если вы используете Visual Web Developer или стандартный выпуск Visual Studio, необходимо вместо этого использовать ручной. Шаг 13 приведены подробные инструкции для выполнения этих шагов вручную. Я рекомендуем вам ознакомиться шаги 2 до 12 перед чтением шаг 13, так как эти действия включают важные инструкции по настройке SQL Server, должны быть применены независимо от того, какие версии Visual Studio.


Сначала откройте Visual Studio. В меню «Файл» выберите новый проект, чтобы открыть диалоговое окно нового проекта (см. рис. 4). Детализировать углублением типа проекта базы данных, а затем выберите из списка справа в списке шаблонов для создания нового проекта SQL Server. Я решил имя этого проекта `ManagedDatabaseConstructs` и поместить его в решение с именем `Tutorial75`.


[![Создайте новый проект SQL Server](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image7.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image6.png)

**Рис. 4**: создайте новый проект SQL Server ([Просмотр полноразмерное изображение](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image8.png))


В диалоговом окне нового проекта для создания решения и проекта SQL Server, нажмите кнопку ОК.

Проекта SQL Server привязан к определенной базе данных. Следовательно после создания нового проекта SQL Server мы сразу же просят указать эти сведения. Рис. 5 показано диалоговое окно нового ссылку на базу данных, который указан для указания базы данных Northwind, мы, зарегистрированные в экземпляре базы данных SQL Server 2005 Express Edition обратно на шаге 1.


![Связать проект SQL Server с базой данных "Борей"](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image9.png)

**Рис. 5**: связать проект SQL Server с базой данных "Борей"


Для отладки управляемых хранимых процедур и определяемых пользователем функций, мы создадим в этот проект, необходимо включить поддержку для подключения отладки SQL/CLR. Каждый раз, когда связывание проекта SQL Server с новой базы данных (как мы делали на рис. 5), Visual Studio предлагает нам необходимо включить отладку SQL/CLR для соединения (см. рис. 6). Нажмите кнопку Да.


![Включить отладку SQL/CLR](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image10.png)

**Рис. 6**: включить отладку SQL/CLR


На этом этапе в решение добавлен новый проект SQL Server. Он содержит папку с именем `Test Scripts` с файл с именем `Test.sql`, который используется для отладки управляемых объектов базы данных создана в проекте. Мы рассмотрим отладки в шаг 12.

Теперь можно добавить новые управляемые хранимые процедуры и определяемые пользователем функции в этот проект, но прежде чем мы сообщим сначала включают нашей существующего веб-приложения в решении. Меню "файл" выберите пункт Добавить и выберите существующий веб-сайт. Перейдите к папке соответствующего веб-сайта и нажмите кнопку ОК. Как показано на рис. 7, это приведет к обновлению решение для включения двух проектов: веб-сайт и `ManagedDatabaseConstructs` проект SQL Server.


![В обозревателе решений, теперь включает два проекта](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image11.png)

**Рис. 7**: В обозревателе решений, теперь включает два проекта


`NORTHWNDConnectionString` Значение в `Web.config` в настоящее время ссылается `NORTHWND.MDF` файла в `App_Data` папки. Поскольку мы удалили эту базу данных с `App_Data` и явно зарегистрированы в экземпляре базы данных SQL Server 2005 Express Edition, необходимо соответствующим образом обновить `NORTHWNDConnectionString` значение. Откройте `Web.config` файл на веб-сайт и измените `NORTHWNDConnectionString` значение, чтобы считывает строку подключения: `Data Source=localhost\SQLExpress;Initial Catalog=Northwind;Integrated Security=True`. После этого изменения вашего `<connectionStrings>` раздела `Web.config` должен выглядеть следующим образом:


[!code-xml[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample1.xml)]

> [!NOTE]
> Как было сказано в [предыдущего учебника](debugging-stored-procedures-cs.md), при отладке объект SQL Server из клиентского приложения, например на веб-сайт ASP.NET, следует отключить пулы соединений. Строка подключения, показанный выше отключает использование пулов соединений ( `Pooling=false` ). Если вы не планируете отладки управляемых хранимых процедур и определяемых пользователем функций из веб-узла ASP.NET, активировать использование пула подключений.


## <a name="step-3-creating-a-managed-stored-procedure"></a>Шаг 3: Создание управляемой хранимой процедуры

Чтобы добавить управляемая хранимая процедура в базе данных Northwind, необходимо сначала создать хранимую процедуру как метод в проект SQL Server. В обозревателе решений щелкните правой кнопкой мыши `ManagedDatabaseConstructs` имя проекта и выберите команду Добавить новый элемент. Откроется диалоговое окно Добавить новый элемент, который содержит список типов управляемых объектов базы данных, можно добавить в проект. Как показано на рис. 8, в нем хранимые процедуры и определяемые пользователем функции, помимо прочего.

Разрешить s начните с добавления хранимой процедуры, которая просто возвращает все продукты, которые больше не поддерживаются. Присвойте новому файлу хранимой процедуры `GetDiscontinuedProducts.cs`.


[![Добавить новую хранимую процедуру с именем GetDiscontinuedProducts.cs](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image13.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image12.png)

**Рис. 8**: добавить новые хранимые процедуры с именем `GetDiscontinuedProducts.cs` ([Просмотр полноразмерное изображение](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image14.png))


Это создаст новый файл класса C# со следующим содержимым:


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample2.cs)]

Обратите внимание, что хранимая процедура реализуется в виде `static` метода в `partial` файл класса с именем `StoredProcedures`. Кроме того `GetDiscontinuedProducts` метод оформлен `SqlProcedure attribute`, который помечает метод как хранимую процедуру.

В следующем коде создается `SqlCommand` и устанавливает его `CommandText` для `SELECT` запрос, возвращающий все столбцы из `Products` таблица для продуктов, `Discontinued` поля равно 1. Затем выполняется команда и отправляет результаты в клиентское приложение. Добавьте следующий код в `GetDiscontinuedProducts` метод.


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample3.cs)]

Все управляемые объекты базы данных имеют доступ к [ `SqlContext` объекта](https://msdn.microsoft.com/library/ms131108.aspx) , представляющий контекст вызывающего объекта. `SqlContext` Предоставляет доступ к [ `SqlPipe` объекта](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.aspx) через его [ `Pipe` свойства](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlcontext.pipe.aspx). Это `SqlPipe` объект используется переносящий информации между базой данных SQL Server и вызывающему приложению. Как и предполагает его имя, [ `ExecuteAndSend` метод](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.executeandsend.aspx) выполняет переданное `SqlCommand` объекта и отправляет результаты обратно в клиентское приложение.

> [!NOTE]
> Управляемые объекты базы данных лучше всего подходят для хранимых процедур и определяемых пользователем функций, использующих процедурная логика, а не логику, основанную на наборе. Процедурная логика предполагает работа с наборами данных на основе по строкам или работе со скалярными значениями. `GetDiscontinuedProducts` Мы только что создали, тем не менее, метод включает в себя без процедурной логики. Таким образом он будет реализовываться в идеале как T-SQL, хранимая процедура. Оно реализовано как управляемая хранимая процедура для демонстрации действия, необходимые для создания и развертывания управляемых хранимых процедур.


## <a name="step-4-deploying-the-managed-stored-procedure"></a>Шаг 4: Развертывание управляемая хранимая процедура

Этот код завершения мы готовы к развертыванию его к базе данных Northwind. Развертывание проекта SQL Server код компилируется в сборку, регистрирует сборку с базой данных и создает соответствующие объекты в базе данных, связывая их соответствующие методы в сборке. Точный набор задач, выполняемых с помощью параметра развертывания точнее выраженная в шаг 13. Щелкните правой кнопкой мыши `ManagedDatabaseConstructs` проекта имя в обозревателе решений и выберите вариант развертывания. Тем не менее, развертывание завершается неудачей из-за ошибки: неправильный синтаксис около «Внешний». Необходимо установить уровень совместимости текущей базы данных на более высокое значение, чтобы включить эту функцию. В разделе справки для хранимой процедуры `sp_dbcmptlevel`.

Это сообщение об ошибке возникает при попытке зарегистрировать сборку в базе данных Northwind. Чтобы зарегистрировать сборку в базе данных SQL Server 2005, уровень совместимости базы данных s должно быть присвоено значение 90. По умолчанию новых баз данных SQL Server 2005 с уровнем совместимости 90. Однако базы данных, созданные с помощью Microsoft SQL Server 2000 иметь уровень совместимости по умолчанию 80. Момента базы данных Northwind изначально базы данных Microsoft SQL Server 2000, ее уровень совместимости установлено значение 80 и поэтому должно быть увеличено 90 для регистрации управляемых объектов базы данных.

Чтобы обновить уровень совместимости базы данных s, откройте окно нового запроса в среде Management Studio и введите:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample4.sql)]

Щелкните значок «выполнить» на панели инструментов можно выполнить этот запрос.


[![Обновить уровень совместимости базы данных "Борей" s](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image16.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image15.png)

**Рис. 9**: обновление s уровень совместимости базы данных Northwind ([Просмотр полноразмерное изображение](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image17.png))


После обновления уровень совместимости, повторное развертывание проекта SQL Server. Это время развертывания должна завершиться без ошибок.

Вернуться к SQL Server Management Studio, щелкните правой кнопкой мыши в базе данных "Борей" в обозревателе объектов и нажмите кнопку обновления. Далее углубляться в папке «программирование», а затем разверните папку сборки. Как показано на рис. 10, базы данных Northwind теперь включает сборки, создаваемой `ManagedDatabaseConstructs` проекта.


![Сборка ManagedDatabaseConstructs — теперь зарегистрирован в базе данных Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image18.png)

**Рис. 10**: `ManagedDatabaseConstructs` сборки — теперь зарегистрирован в базе данных Northwind


Также разверните папку хранимые процедуры. Что вы увидите хранимую процедуру с именем `GetDiscontinuedProducts`. Эта хранимая процедура была создана, процесс развертывания и указывает на `GetDiscontinuedProducts` метод в `ManagedDatabaseConstructs` сборки. Когда `GetDiscontinuedProducts` хранимая процедура выполняется, он, в свою очередь, выполняет `GetDiscontinuedProducts` метод. Так как это управляемая хранимая процедура не может быть изменен через среду Management Studio (поэтому значок замка рядом с именем хранимой процедуры).


![Хранимая процедура GetDiscontinuedProducts, перечисленные в папке хранимых процедур](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image19.png)

**Рис. 11**: `GetDiscontinuedProducts` хранимой процедуры, перечисленные в папке хранимых процедур


По-прежнему необходимо преодолеть перед управляемой хранимой процедуры можно вызывать одно препятствие: база данных настроена для предотвращения выполнения управляемого кода. Проверить это, откройте новое окно запроса и выполнение `GetDiscontinuedProducts` хранимой процедуры. Вы получите следующее сообщение об ошибке: выполнение пользовательского кода в .NET Framework отключено. Включите параметр конфигурации clr enabled.

Для проверки параметров s базы данных Northwind, введите и выполните команду `exec sp_configure` в окне запроса. Это показывает, что среда clr включена настройка установлено значение 0.


[![Включена среда clr установлено в настоящее время установлен в 0](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image21.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image20.png)

**Рис. 12**: включена среда clr установлено в настоящее время установлен в 0 ([Просмотр полноразмерное изображение](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image22.png))


Обратите внимание, что каждый параметр конфигурации на рис. 12 четырех указанных: минимальное и максимальные значения конфигурации и выполнения значений. Чтобы обновить значение конфигурации для параметра включена среда clr, выполните следующую команду:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample5.sql)]

При повторном запуске `exec sp_configure` вы увидите, что приведенная выше инструкция обновляется значение конфигурации включена среда clr параметр s 1, а, это значение установлено в 0. Для этого изменения конфигурации вступили в силу необходимо выполнить [ `RECONFIGURE` команда](https://msdn.microsoft.com/library/ms176069.aspx), задающий рабочее значение со значением в текущей конфигурации. Просто введите `RECONFIGURE` в окно запроса и щелкните значок «выполнить» на панели инструментов. При запуске `exec sp_configure` теперь должна отображается значение 1 для параметра включена среда clr s конфигурации и выполнения значения.

После завершения настройки включена среда clr, мы готовы к выполнению управляемого `GetDiscontinuedProducts` хранимой процедуры. В окне запроса введите и выполните следующую команду `exec` `GetDiscontinuedProducts`. В результате вызова хранимой процедуры на соответствующий управляемый код в `GetDiscontinuedProducts` для выполнения метода. Этот код выдает `SELECT` запрос для возврата всех продуктов, которые не поддерживаются и возвращает эти данные вызывающему приложению, — в этом экземпляре SQL Server Management Studio. Среда Management Studio получает этих результатов и отображает их в окне «результаты».


[![GetDiscontinuedProducts, хранимая процедура возвращает все неподдерживаемые продукты](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image24.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image23.png)

**Рис. 13**: `GetDiscontinuedProducts` хранимой процедуры возвращает все неподдерживаемые продукты ([Просмотр полноразмерное изображение](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image25.png))


## <a name="step-5-creating-managed-stored-procedures-that-accept-input-parameters"></a>Шаг 5: Создание управляемых хранимых процедур, которые принимают входные параметры

Многие из запросов и хранимых процедур, мы создали в этих учебниках используется *параметры*. Например, в [создание новых хранимых процедур для типизированного набора данных s адаптеры таблиц TableAdapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) учебника мы создали хранимую процедуру с именем `GetProductsByCategoryID` , приняты входной параметр с именем `@CategoryID`. Хранимая процедура затем возвращаются все продукты, `CategoryID` поле, которое соответствует значению предоставленного `@CategoryID` параметра.

Для создания управляемой хранимой процедуры, которая принимает входные параметры, достаточно укажите эти параметры в определении метода s. Чтобы проиллюстрировать это, s позволяют добавить другой управляемая хранимая процедура для `ManagedDatabaseConstructs` проект с именем `GetProductsWithPriceLessThan`. Это управляемая хранимая процедура принимает входной параметр указания цены и вернет все продукты, `UnitPrice` поля меньше, чем значение параметра s.

Чтобы добавить новую хранимую процедуру в проект, щелкните правой кнопкой мыши `ManagedDatabaseConstructs` имя проекта и добавить новую хранимую процедуру. Назовите файл `GetProductsWithPriceLessThan.cs`. Как мы видели в шаге 3, это создаст новый файл класса C# с помощью метода с именем `GetProductsWithPriceLessThan` помещен в `partial` класса `StoredProcedures`.

Обновление `GetProductsWithPriceLessThan` определение метода s, чтобы он принимал [ `SqlMoney` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.aspx) входной параметр с именем `price` и писать код для выполнения и возвращает результаты запроса:


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample6.cs)]

`GetProductsWithPriceLessThan` Определение метода s и код напоминает определение и кода `GetDiscontinuedProducts` метод, созданный на шаге 3. Единственными отличиями являются, `GetProductsWithPriceLessThan` метод принимает в качестве входного параметра (`price`), `SqlCommand` s запроса включает параметр (`@MaxPrice`), и добавляется параметр `SqlCommand` s `Parameters` собираются и присвоено значение `price` переменной.

После добавления этого кода, повторное развертывание проекта SQL Server. После этого вернитесь к SQL Server Management Studio и обновите папку хранимые процедуры. Вы увидите новую запись, `GetProductsWithPriceLessThan`. В окне запроса введите и выполните следующую команду `exec GetProductsWithPriceLessThan 25`, который будет список всех продуктов меньше 25 долл. США, как показано на рис. 14.


[![Отображаются продукты в 25 долл. США](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image27.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image26.png)

**Рис. 14**: отображаются продукты в 25 долл. США ([Просмотр полноразмерное изображение](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image28.png))


## <a name="step-6-calling-the-managed-stored-procedure-from-the-data-access-layer"></a>Шаг 6: Вызов управляемой хранимой процедуры из уровня доступа к данным

На этом этапе мы добавили `GetDiscontinuedProducts` и `GetProductsWithPriceLessThan` управляемых хранимых процедур для `ManagedDatabaseConstructs` проекта и зарегистрирована их в базе данных "Борей" SQL Server. Мы также вызывается эти управляемые хранимые процедуры из SQL Server Management Studio (см. Рисунок 13 и 14). Чтобы наши ASP.NET приложения для использования этих управляемых хранимых процедур, тем не менее, необходимо добавить их для доступа к данным и бизнес-логики уровни в архитектуре. На этом этапе мы добавим два новых метода для `ProductsTableAdapter` в `NorthwindWithSprocs` типизированного набора данных, который изначально был создан в [создание новых хранимых процедур для типизированного набора данных s адаптеры таблиц TableAdapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) учебника. В шаге 7 мы добавим соответствующие методы для МЕТОДА.

Откройте `NorthwindWithSprocs` типизированного набора данных в Visual Studio и начала, добавив новый метод `ProductsTableAdapter` с именем `GetDiscontinuedProducts`. Чтобы добавить новый метод адаптера таблицы, щелкните правой кнопкой мыши на имени s адаптера таблицы в конструкторе и выберите Добавить запрос в контекстном меню.

> [!NOTE]
> Поскольку мы перешли из базы данных Northwind `App_Data` папки к экземпляру базы данных SQL Server 2005 Express Edition, крайне важно, чтобы отразить это изменение обновляться соответствующая строка подключения в файле Web.config. На шаге 2 мы рассмотрели обновление `NORTHWNDConnectionString` значение в `Web.config`. Если вы забыли для немедленного обновления, вы увидите сообщение об ошибке, не удалось добавить запрос. Не удается найти подключение `NORTHWNDConnectionString` для объекта `Web.config` в диалоговом окне при попытке добавить новый метод в TableAdapter. Чтобы устранить эту ошибку, нажмите кнопку ОК, а затем перейдите к `Web.config` и обновить `NORTHWNDConnectionString` значение, как описано в шаге 2. Попробуйте повторно добавить метод в TableAdapter. Это время, он должен работать без ошибок.


Добавление нового метода будет запущен мастер настройки запроса адаптера таблицы, который мы использовали много раз за последние учебники. Сначала появляется запрос для указания того, как адаптер следует обращаться к базе данных: посредством нерегламентированных инструкций SQL или с помощью новой или существующей хранимой процедуры. Так как мы уже создали и зарегистрировали `GetDiscontinuedProducts` управляемая хранимая процедура с базой данных, выберите использовать существующие хранимые процедуры параметр и нажмите кнопку Далее.


[![Выберите использование существующей хранимой процедуры, параметр](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image30.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image29.png)

**Рис. 15**: выберите, использовать существующие хранимой процедуры параметр ([Просмотр полноразмерное изображение](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image31.png))


Далее нам запрашивает хранимой процедуры, который будет вызывать метод. Выберите `GetDiscontinuedProducts` управляемая хранимая процедура из раскрывающегося списка и нажмите кнопку Далее.


[![Выберите GetDiscontinuedProducts управляемой хранимой процедуры](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image33.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image32.png)

**На рисунке 16**: выберите `GetDiscontinuedProducts` управляемых хранимых процедур ([Просмотр полноразмерное изображение](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image34.png))


Затем, нас просят указать, возвращает ли хранимая процедура строки, единственное значение или ничего. Поскольку `GetDiscontinuedProducts` возвращает набор строк неподдерживаемые продукты, выберите первый вариант (табличные данные) и нажмите кнопку Далее.


[![Выберите параметр табличных данных](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image36.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image35.png)

**Рисунок 17**: выберите параметр табличных данных ([Просмотр полноразмерное изображение](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image37.png))


Экран окончательного мастер позволяет указать шаблонов доступа к данным, который используется и имена методов, полученный. Оставьте флажки проверки и имя методы `FillByDiscontinued` и `GetDiscontinuedProducts`. Нажмите кнопку Готово, чтобы завершить работу мастера.


[![Имя методы FillByDiscontinued и GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image39.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image38.png)

**На рисунке 18**: имя методы `FillByDiscontinued` и `GetDiscontinuedProducts` ([Просмотр полноразмерное изображение](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image40.png))


Повторите эти шаги для создания метода с именем `FillByPriceLessThan` и `GetProductsWithPriceLessThan` в `ProductsTableAdapter` для `GetProductsWithPriceLessThan` управляемая хранимая процедура.

На рисунке 19 показано снимка экрана в конструкторе наборов данных после добавления методов в `ProductsTableAdapter` для `GetDiscontinuedProducts` и `GetProductsWithPriceLessThan` управляемых хранимых процедур.


[![ProductsTableAdapter включает в себя новые методы, добавленные в этом шаге](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image42.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image41.png)

**На рисунке 19**: `ProductsTableAdapter` включает в себя новые методы добавлены на этом шаге ([Просмотр полноразмерное изображение](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image43.png))


## <a name="step-7-adding-corresponding-methods-to-the-business-logic-layer"></a>Шаг 7: Добавление соответствующих методов уровня бизнес-логики

Теперь, когда мы обновили уровня доступа к данным, чтобы включить методы для вызова управляемых хранимых процедур, добавленные в шагах 4 и 5, необходимо добавить соответствующие методы для бизнес-логики. Добавьте следующие два метода для `ProductsBLLWithSprocs` класса:


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample7.cs)]

Оба метода просто вызвать соответствующий метод DAL и возвращается `ProductsDataTable` экземпляра. `DataObjectMethodAttribute` Разметки выше каждый метод вызывает эти методы, которые будут включены в раскрывающемся списке на вкладке ВЫБЕРИТЕ мастер настройки источника данных ObjectDataSource s.

## <a name="step-8-invoking-the-managed-stored-procedures-from-the-presentation-layer"></a>Шаг 8: Вызов управляемых хранимых процедур из уровня представления

Бизнес-логики и уровней доступа к данным дополнить для поддержки вызова `GetDiscontinuedProducts` и `GetProductsWithPriceLessThan` управляемых хранимых процедур могут отображаться этих хранимых процедур результаты с помощью страницы ASP.NET.

Откройте `ManagedFunctionsAndSprocs.aspx` страницы в `AdvancedDAL` папки и из панели элементов перетащите элемент управления GridView в конструктор. Набор GridView s `ID` свойства `DiscontinuedProducts` и его смарт-теге, привязать его к новой ObjectDataSource с именем `DiscontinuedProductsDataSource`. Настройка ObjectDataSource для извлечения данных из `ProductsBLLWithSprocs` класса s `GetDiscontinuedProducts` метод.


[![Настройка ObjectDataSource с помощью класса ProductsBLLWithSprocs](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image45.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image44.png)

**Рис. 20**: Настройка ObjectDataSource для использования `ProductsBLLWithSprocs` класса ([Просмотр полноразмерное изображение](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image46.png))


[![Выберите из раскрывающегося списка на вкладке ВЫБЕРИТЕ метод GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image48.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image47.png)

**Рисунок 21**: выберите `GetDiscontinuedProducts` метода из раскрывающегося списка на вкладке ВЫБЕРИТЕ ([Просмотр полноразмерное изображение](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image49.png))


Поскольку в этой сетке будут использованы для только что отображение сведений о продукте, задать раскрывающихся списков в UPDATE, INSERT и удаление вкладок (нет) и нажмите кнопку Готово.

После завершения работы мастера, Visual Studio автоматически добавит BoundField или CheckBoxField для каждого поля данных в `ProductsDataTable`. Теперь пора удалить все эти поля за исключением `ProductName` и `Discontinued`, на котором точки к GridView и ObjectDataSource s должна выглядеть следующим образом:


[!code-aspx[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample8.aspx)]

Теперь пора просматривать эту страницу через браузер. При просмотре, вызовы ObjectDataSource `ProductsBLLWithSprocs` класса s `GetDiscontinuedProducts` метод. Как мы видели в шаге 7, этот метод вызывает DAL s `ProductsDataTable` класса s `GetDiscontinuedProducts` метод, который вызывает `GetDiscontinuedProducts` хранимой процедуры. Эта хранимая процедура является управляемой хранимой процедуры и выполняет код, созданный на шаге 3, возвращая неподдерживаемые продукты.

Результаты, возвращенные управляемая хранимая процедура упаковано в `ProductsDataTable` DAL предоставляет и затем возвращаются для МЕТОДА, который возвращает их уровень представления, где они привязаны к GridView и отображаются. Как и ожидалось, в сетке приведен список этих продуктов, которые больше не поддерживаются.


[![Неподдерживаемые продукты отображаются](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image51.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image50.png)

**На рисунке 22**: указаны неподдерживаемые продукты ([Просмотр полноразмерное изображение](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image52.png))


Для выполнения задания Добавьте текстовое поле и другой GridView на страницу. У этого GridView отображения продуктов меньше суммы, введенной в текстовое поле, вызвав `ProductsBLLWithSprocs` класса s `GetProductsWithPriceLessThan` метод.

## <a name="step-9-creating-and-calling-t-sql-udfs"></a>Шаг 9: Создание и вызов определяемых пользователем функций T-SQL

Определяемые пользователем функции или определяемые пользователем функции, — это объекты базы данных точно соответствуют семантику функций в языках программирования. Как и функция в C# определяемые пользователем функции можно включить переменное число входных параметров и возвращает значение определенного типа. Определяемая пользователем Функция может возвращать либо скалярное данных — строка, целое число и т. д. - или табличных данных. Разрешить s краткое взгляните на определяемые пользователем функции, начиная с определяемой пользователем функции, которая возвращает скалярный тип данных обоих типов.

Следующие определяемая пользователем Функция вычисляет оценочное значение инвентаризации для определенного продукта. Это делается путем ведения три входных параметра - `UnitPrice`, `UnitsInStock`, и `Discontinued` значений для конкретного продукта — и возвращает значение типа `money`. Предполагаемый объем запасов вычисляется путем умножения `UnitPrice` по `UnitsInStock`. Неподдерживаемые элементы уменьшается вдвое это значение.


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample9.sql)]

После добавления этой определяемой пользователем функции в базе данных его можно найти через среду Management Studio разверните папку программирования, а затем функции, а затем скалярные функции. Он может использоваться в `SELECT` запрос следующим образом:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample10.sql)]

Я добавил `udf_ComputeInventoryValue` определяемых пользователем Функций в базу данных "Борей". На рисунке 23 показан результат выполнения указанных выше `SELECT` запрос, если просмотреть с помощью среды Management Studio. Также Обратите внимание, что определяемая пользователем Функция находится в списке папку скалярные функции в обозревателе объектов.


[![Каждый продукт s значений запасов, перечислен](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image54.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image53.png)

**На рисунке 23**: s каждого продукта перечислены значения запасов ([Просмотр полноразмерное изображение](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image55.png))


Определяемые пользователем функции также может возвращать табличных данных. Например можно создать определяемую пользователем Функцию, возвращаются продукты, относящиеся к определенной категории:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample11.sql)]

`udf_GetProductsByCategoryID` Определяемая пользователем Функция принимает `@CategoryID` входного параметра и возвращает результаты указанного `SELECT` запроса. После создания этой определяемой пользователем функции можно ссылаться в `FROM` (или `JOIN`) предложения `SELECT` запроса. Следующий пример возвращает `ProductID`, `ProductName`, и `CategoryID` значения для каждой напитков.


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample12.sql)]

Я добавил `udf_GetProductsByCategoryID` определяемых пользователем Функций в базу данных "Борей". Рис. 24 показан результат выполнения указанных выше `SELECT` запрос, если просмотреть с помощью среды Management Studio. Определяемые пользователем функции, возвращающие табличные данные можно найти в папке возвращающие табличное значение функции s обозревателя объектов.


[![ProductID, ProductName и CategoryID отображаются для каждого напитков](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image57.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image56.png)

**Рис. 24**: `ProductID`, `ProductName`, и `CategoryID` отображаются для каждого напитков ([Просмотр полноразмерное изображение](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image58.png))


> [!NOTE]
> Дополнительные сведения о создании и использовании определяемых пользователем функций извлечь [введение в определяемые пользователем функции](http://www.sqlteam.com/item.asp?ItemID=1955). Можно также выбрать [преимущества и функции Drawbacks of User-Defined](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1).


## <a name="step-10-creating-a-managed-udf"></a>Шаг 10: Создание управляемых определяемых пользователем Функций

`udf_ComputeInventoryValue` И `udf_GetProductsByCategoryID` определяемых пользователем функций, созданных в приведенных выше примерах являются объектами базы данных T-SQL. SQL Server 2005 также поддерживает управляемые определяемые пользователем функции, которые могут добавляться в `ManagedDatabaseConstructs` проект как управляемых хранимых процедур из шагов 3 и 5. Для выполнения этого шага позволяют реализовать s `udf_ComputeInventoryValue` определяемой пользователем функции в управляемом коде.

Для добавления управляемых определяемых пользователем Функций для `ManagedDatabaseConstructs` проекта, щелкните правой кнопкой мыши имя проекта в обозревателе решений и выберите команду Добавить новый элемент. Выберите шаблон, определяемые пользователем в диалоговом окне Добавление нового элемента и присвойте новому файлу определяемой пользователем функции `udf_ComputeInventoryValue_Managed.cs`.


[![Добавьте в проект ManagedDatabaseConstructs новый управляемый определяемой пользователем функции.](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image60.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image59.png)

**Рис. 25**: Добавление новых управляемых определяемой пользователем функции для `ManagedDatabaseConstructs` проекта ([Просмотр полноразмерное изображение](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image61.png))


Создает шаблон пользовательской функции `partial` класс с именем `UserDefinedFunctions` с методом, имя которого совпадает с именем файла s класса (`udf_ComputeInventoryValue_Managed`, в данном экземпляре). Этот метод, отмеченный с помощью [ `SqlFunction` атрибут](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlfunctionattribute.aspx), какие флаги методу в качестве управляемого определяемой пользователем функции.


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample13.cs)]

`udf_ComputeInventoryValue` Метод в настоящее время возвращает [ `SqlString` объекта](https://msdn.microsoft.com/library/system.data.sqltypes.sqlstring.aspx) и не принимает все входные параметры. Необходимо обновить определение метода, чтобы он принимает три входных параметра - `UnitPrice`, `UnitsInStock`, и `Discontinued` — и возвращает `SqlMoney` объекта. Логика для вычисления значения инвентаризации идентична в T-SQL `udf_ComputeInventoryValue` определяемой пользователем функции.


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample14.cs)]

Обратите внимание, что входные параметры метода s определяемой пользователем функции имеют соответствующие типы SQL: `SqlMoney` для `UnitPrice` поля, [ `SqlInt16` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlint16.aspx) для `UnitsInStock`, и [ `SqlBoolean` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlboolean.aspx) для `Discontinued`. Эти типы данных зависит от типов, определенных в `Products` таблицы: `UnitPrice` столбец имеет тип `money`, `UnitsInStock` столбец типа `smallint`и `Discontinued` столбец типа `bit`.

Код сначала создает `SqlMoney` экземпляр с именем `inventoryValue` , которому присваивается значение 0. `Products` Позволяет таблицы для базы данных `NULL` значения в `UnitsInPrice` и `UnitsInStock` столбцов. Таким образом, нам нужно первая проверка содержат эти значения `NULL` s, что мы делаем через `SqlMoney` объект s [ `IsNull` свойства](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.isnull.aspx). Если оба `UnitPrice` и `UnitsInStock` содержат отличных`NULL` значения, а затем мы вычислений `inventoryValue` быть произведение двух. Затем, если `Discontinued` имеет значение true, а затем мы сократить значение.

> [!NOTE]
> `SqlMoney` Только обеспечивает два `SqlMoney` экземпляров будет умножено друг с другом. Он не допускает `SqlMoney` экземпляр будет умножено на числовой литерал с плавающей запятой. Таким образом чтобы сократить его `inventoryValue` мы умножьте его на новый `SqlMoney` экземпляру, который имеет значение 0,5.


## <a name="step-11-deploying-the-managed-udf"></a>Шаг 11: Развертывание управляемых определяемой пользователем функции.

После создания управляемой функции UDF мы готовы развернуть его в базе данных Northwind. Как было показано в шаге 4, управляемых объектов проекта SQL Server будут развернуты, щелкнув правой кнопкой мыши имя проекта в обозревателе решений и выбрав параметр развернуть в контекстном меню.

После развертывания проекта, вернитесь в SQL Server Management Studio и обновите содержимое папки, скалярные функции. Теперь должны отображаться две записи:

- `dbo.udf_ComputeInventoryValue` -T-SQL определяемая пользователем Функция создана на шаге 9, и
- `dbo.udf ComputeInventoryValue_Managed` -управляемого определяемая пользователем Функция создана в шаге 10, который только что была развернута.

Чтобы протестировать этот управляемый определяемой пользователем функции, выполните следующий запрос в среде Management Studio:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample15.sql)]

Эта команда использует управляемый `udf ComputeInventoryValue_Managed` определяемой пользователем функции, а не T-SQL, `udf_ComputeInventoryValue` определяемой пользователем функции, но выходные данные одинаково. Ссылаться на рисунке 23, чтобы просмотреть снимок экрана s выходные данные определяемой пользователем функции.

## <a name="step-12-debugging-the-managed-database-objects"></a>Шаг 12: Отладки управляемых объектов базы данных

В [отладки хранимых процедур](debugging-stored-procedures-cs.md) учебника мы рассмотрели трех параметров для отладки SQL Server с помощью Visual Studio: прямой отладки базы данных, отладку приложения и отладка из проекта SQL Server. Управлять базы данных объекты не может быть отлажен через прямой отладки базы данных, но их можно отлаживать из клиентского приложения, а также непосредственно из проекта SQL Server. В порядке для отладки Однако базы данных SQL Server 2005 необходимо разрешить отладку SQL/CLR. Помните, что при первом создании `ManagedDatabaseConstructs` проекта Visual Studio просили ли мы хотели включить отладку (см. рис. 6 в шаге 2) SQL/CLR. Этот параметр можно изменить, щелкнув в окне обозревателя сервера базы данных.


![Убедитесь, что база данных допускает отладку SQL/CLR](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image62.png)

**Рис. 26**: Убедитесь, что база данных допускает отладку SQL/CLR


Предположим, что нам нужно отладить `GetProductsWithPriceLessThan` управляемая хранимая процедура. Начнем можно установить точку останова в коде `GetProductsWithPriceLessThan` метод.


[![Установите точку останова в методе GetProductsWithPriceLessThan](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image64.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image63.png)

**Рис. 27**: установите точку останова в `GetProductsWithPriceLessThan` метод ([Просмотр полноразмерное изображение](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image65.png))


Разрешить s сначала посмотрим отладки управляемых объектов базы данных из проекта SQL Server. Так как в нашем решении входят два проекта — `ManagedDatabaseConstructs` проект SQL Server вместе с веб-узел - для отладки из проекта SQL Server, необходимо указать Visual Studio для запуска `ManagedDatabaseConstructs` проект SQL Server, когда мы начинаем отладки. Щелкните правой кнопкой мыши `ManagedDatabaseConstructs` проекта в обозревателе решений и выберите Назначить запускаемым проектом параметр в контекстном меню.

Когда `ManagedDatabaseConstructs` запускается из отладчика, он выполняет инструкции SQL в проект `Test.sql` файл, который находится в `Test Scripts` папки. Например, чтобы проверить `GetProductsWithPriceLessThan` управляемая хранимая процедура замены существующего `Test.sql` файл содержимого с помощью следующей инструкции, которая вызывает `GetProductsWithPriceLessThan` управляемая хранимая процедура, передав `@CategoryID` значение 14.95:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample16.sql)]

Когда вы хранить ввести приведенного выше сценария в `Test.sql`, начать отладку, перейдите в меню "Отладка" и выбрав команду Начать отладку или нажимать клавишу F5 или зеленый значок воспроизведения на панели инструментов. Будут построены проекты в решении, развертывания управляемых объектов базы данных в базе данных "Борей" и затем выполнить `Test.sql` сценария. На этом этапе будет достигнута точка останова, и мы можно пошагово проходить `GetProductsWithPriceLessThan` метод, просмотрите значения входных параметров и т. д.


[![В методе GetProductsWithPriceLessThan точки останова](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image67.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image66.png)

**Рис. 28**: точки останова в `GetProductsWithPriceLessThan` щелчка метод ([Просмотр полноразмерное изображение](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image68.png))


Чтобы объект базы данных SQL для отладки через клиентское приложение крайне важно, базы данных были настроены для поддержки отладки приложений. Щелкните правой кнопкой мыши базу данных в обозревателе сервера и убедитесь, что установлен параметр отладки приложения. Кроме того нам нужно настроить приложение ASP.NET для интеграции с помощью отладчика SQL и для отключения пула соединений. Эти шаги были подробно обсуждаются в шаге 2 [отладки хранимых процедур](debugging-stored-procedures-cs.md) учебника.

После настройки приложения ASP.NET и базы данных, задать веб-сайта ASP.NET в качестве запускаемого проекта и начните отладку. При посещении страницы, которая вызывает один из управляемых объектов, которые точкой останова, приложение будет остановлено и управления будут поддерживаться отладчику, где можно производить шаг через код, как показано на рисунке 28.

## <a name="step-13-manually-compiling-and-deploying-managed-database-objects"></a>Шаг 13: Вручную компиляция и развертывание управляемых объектов базы данных

Проекты SQL Server позволяют легко создавать, компиляции и развертывания управляемых объектов базы данных. К сожалению проекты SQL Server доступны только в выпусках Professional и команды системы Visual Studio. Если используется Visual Web Developer или Standard Edition для Visual Studio и хотите использовать управляемые объекты базы данных, необходимо вручную создать и развернуть их. Включает в себя четыре этапа:

1. Создайте файл, содержащий исходный код для управляемого объекта базы данных,
2. Компиляция в сборку, объект
3. Зарегистрируйте сборку с базой данных SQL Server 2005 и
4. Создайте объект базы данных в SQL Server, который указывает на соответствующий метод в сборке.

Показаны эти задачи, позволяя создать новую s управляемого хранимой процедуры, возвращающей этих продуктов, `UnitPrice` больше, чем указанное значение. Создать новый файл на компьютере с именем `GetProductsWithPriceGreaterThan.cs` и введите следующий код в файл (можно использовать Visual Studio, «Блокнот» или любой текстовый редактор для этого):


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample17.cs)]

Данный пример кода является почти идентична `GetProductsWithPriceLessThan` метод, созданный на шаге 5. Единственными отличиями являются имена методов, `WHERE` предложения и имя параметра, используемого в запросе. В `GetProductsWithPriceLessThan` метода `WHERE` предложение чтения: `WHERE UnitPrice < @MaxPrice`. Здесь в `GetProductsWithPriceGreaterThan`, мы используем: `WHERE UnitPrice > @MinPrice` .

Необходимо скомпилировать этот класс в сборке. В командной строке перейдите в каталог, где был сохранен `GetProductsWithPriceGreaterThan.cs` файл и использовать компилятор C# (`csc.exe`) чтобы скомпилировать файл класса в сборке:


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample18.cmd)]

Если к папке, содержащей `csc.exe` в не в системе s `PATH`, необходимо полностью указывать его путь `%WINDOWS%\Microsoft.NET\Framework\version\`, следующим образом:


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample19.cmd)]


[![Скомпилируйте GetProductsWithPriceGreaterThan.cs в сборку](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image70.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image69.png)

**Рис. 29**: компиляция `GetProductsWithPriceGreaterThan.cs` в сборку ([Просмотр полноразмерное изображение](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image71.png))


`/t` Флаг указывает, что файл класса C# должен быть скомпилирован в DLL (а не исполняемый файл). `/out` Флаг указывает имя результирующей сборки.

> [!NOTE]
> Вместо компиляции `GetProductsWithPriceGreaterThan.cs` файл класса с помощью командной строки, можно также использовать [Visual C#, экспресс-выпуск](https://msdn.microsoft.com/vstudio/express/visualcsharp/) или создать отдельный проект библиотеки классов в Visual Studio Standard Edition. S ren Алексей Lauritsen окна предоставил такой проект Visual C#, экспресс-выпуск с кодом `GetProductsWithPriceGreaterThan` хранимой процедуры и два управляемых хранимых процедур и определяемая пользователем Функция создан в шаге 3, 5 и 10. S ren s проект также содержит команды T-SQL, необходимо, чтобы добавить соответствующие объекты базы данных.


Код компилируется в сборку мы готовы к регистрации сборки в базе данных SQL Server 2005. Это может быть выполнено с помощью T-SQL, с помощью команды `CREATE ASSEMBLY`, или с помощью SQL Server Management Studio. Разрешить s фокус использование среды Management Studio.

В среде Management Studio разверните папку программирования в базе данных Northwind. Одна из ее подпапке — сборок. Чтобы вручную добавить сборку в базу данных, щелкните правой кнопкой мыши в папке «сборки» и выберите новую сборку в контекстном меню. Это появляется новый сборки диалоговое окно (см. рисунок 30). Нажмите кнопку обзора, выберите `ManuallyCreatedDBObjects.dll` сборки мы просто компилируется и нажмите кнопку ОК, чтобы добавить сборку в базе данных. Вы не увидите `ManuallyCreatedDBObjects.dll` в обозревателе объектов.


[![Добавить сборку ManuallyCreatedDBObjects.dll в базе данных](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image73.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image72.png)

**Рис. 30**: добавление `ManuallyCreatedDBObjects.dll` сборки в базу данных ([Просмотр полноразмерное изображение](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image74.png))


![ManuallyCreatedDBObjects.dll отображается в обозревателе объектов](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image75.png)

**Рис. 31**: `ManuallyCreatedDBObjects.dll` отображается в обозревателе объектов


Хотя мы добавили сборку в базе данных Northwind, нам нужно еще связать хранимую процедуру с `GetProductsWithPriceGreaterThan` метод в сборке. Для этого откройте новое окно запроса и выполните следующий сценарий:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample20.sql)]

Это создает новую хранимую процедуру в базе данных Northwind с именем `GetProductsWithPriceGreaterThan` и связывает его с помощью управляемого метода `GetProductsWithPriceGreaterThan` (который находится в классе `StoredProcedures`, который находится в сборке `ManuallyCreatedDBObjects`).

После выполнения приведенного выше скрипта, обновите папку хранимые процедуры в обозревателе объектов. Вы увидите новую запись хранимой процедуры - `GetProductsWithPriceGreaterThan` -значком блокировки рядом с ним. Чтобы протестировать эту хранимую процедуру, введите и выполните следующий скрипт в окне запроса:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample21.sql)]

Как показано на рис. 32, приведенная выше команда отображает сведения для этих продуктов с `UnitPrice` больше $24.95.


[![ManuallyCreatedDBObjects.dll отображается в обозревателе объектов](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image77.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image76.png)

**Рис. 32**: `ManuallyCreatedDBObjects.dll` отображается в обозревателе объектов ([Просмотр полноразмерное изображение](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image78.png))


## <a name="summary"></a>Сводка

Microsoft SQL Server 2005 обеспечивает интеграцию с Common Language Runtime (CLR), что позволяет объектов базы данных создается с использованием управляемого кода. Ранее эти объекты базы данных можно было создавать только с помощью T-SQL, но теперь можно создать эти объекты с помощью программирования языками, такими как C# .NET. В данном руководстве, мы создали два управляемых хранимых процедур и управляемой определяемые пользователем функции.

Visual Studio s тип проекта SQL Server упрощает создание, компиляция и развертывание управляемых объектов базы данных. Кроме того она предлагает широкие возможности поддержки отладки. Однако типы проектов SQL Server доступны только в выпусках Professional и команды системы Visual Studio. Для тех, кто с помощью Visual Web Developer или Standard Edition для Visual Studio, создание, компиляции и этапы развертывания необходимо выполнить вручную, как мы видели в шаг 13.

Программирование довольны!

## <a name="further-reading"></a>Дополнительные сведения

Дополнительные сведения по темам, рассматриваемые в этом учебнике см. в следующих ресурсах:

- [Преимущества и недостатки использования определяемых пользователем функций](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1)
- [Создание объектов SQL Server 2005 в управляемом коде](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Создание триггеров с помощью управляемого кода в SQL Server 2005](http://www.15seconds.com/issue/041006.htm)
- [Практическое руководство: Создание и запуск CLR SQL Server хранимой процедуры](https://msdn.microsoft.com/library/5czye81z(VS.80).aspx)
- [Практическое руководство: Создание и запуск определяемой пользователем функции CLR SQL Server](https://msdn.microsoft.com/library/w2kae45k(VS.80).aspx)
- [Практическое руководство: Изменение `Test.sql` сценария, выполняемого объекты SQL](https://msdn.microsoft.com/library/ms233682(VS.80).aspx)
- [Введение в пользователя пользователем функции](http://www.sqlteam.com/item.asp?ItemID=1955)
- [Управляемый код и SQL Server 2005 (видео)](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Справочник по Transact-SQL](https://msdn.microsoft.com/library/aa299742(SQL.80).aspx)
- [Пошаговое руководство: Создание хранимой процедуры в управляемом коде](https://msdn.microsoft.com/library/zxsa8hkf(VS.80).aspx)

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основной рецензент этого учебника было S ren Алексей Lauritsen. Кроме этой статьи, S ren также создан проект Visual C#, экспресс-выпуск, включенному в настоящую загрузку статьи s для компиляции управляемых объектов базы данных вручную. Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](debugging-stored-procedures-cs.md)
> [Вперед](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
