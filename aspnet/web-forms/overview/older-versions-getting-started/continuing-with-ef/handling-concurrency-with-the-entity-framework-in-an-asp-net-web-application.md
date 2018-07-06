---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
title: Обработка параллелизма с помощью Entity Framework 4.0 в 4 веб-приложения ASP.NET | Документация Майкрософт
author: tdykstra
description: В этой серии руководств основана на веб-приложение университета Contoso, созданный по началу работы с этой серии руководств Entity Framework 4.0. I...
ms.author: aspnetcontent
ms.date: 01/26/2011
ms.assetid: a5aa22a6-fb7f-4f41-9c7f-addda151940b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 52c4dbdd5cf2919d0a04a883c1cd48cb56dd4e94
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804550"
---
<a name="handling-concurrency-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Обработка параллелизма с помощью Entity Framework 4.0 в 4 веб-приложения ASP.NET
====================
по [том Дайкстра](https://github.com/tdykstra)

> В этой серии руководств основан на веб-приложение университета Contoso, которая создается с [Приступая к работе с Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) серии руководств. Если вы не прошли предыдущих учебных курсах, в качестве отправной точки для этого учебника вы можете [загрузить приложение](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , он был создан. Вы также можете [загрузить приложение](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , созданный путем завершения серии руководств. Если у вас есть вопросы о учебники, их можно разместить [форум ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


В предыдущем руководстве вы узнали, как для сортировки и фильтрации данных с помощью `ObjectDataSource` управления и Entity Framework. Мы продемонстрируем варианта обработки параллелизма в веб-приложения ASP.NET, использующего Entity Framework. Вы создадите новый веб-страницы, предназначенный для обновления распределения аудиторий преподавателя. Будут обрабатываться проблем параллелизма в этой странице и странице отделов, который был создан ранее.

[![Image06](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

[!["Image01"](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

## <a name="concurrency-conflicts"></a>Конфликты параллелизма

Конфликт параллелизма возникает, когда один пользователь редактирует запись и другой пользователь редактирует ту же запись, прежде чем изменение первого пользователя записывается в базу данных. Если вы не настроили Entity Framework для обнаружения таких конфликтов, обновляющий базу данных, перезаписывает изменения другого пользователя. Во многих приложениях такой риск допустим, и не нужно настроить приложение для обработки конфликтов параллелизма возможно. (Если существует несколько пользователей или несколько обновлений или не является критической, если перезапись некоторых изменений, стоимость реализации параллелизма может перевесить его преимущества.) Если вам не нужно беспокоиться о конфликтах параллелизма, можно пропустить этот учебник Оставшиеся два учебника ряда не зависят от ничего, создаваемых в нем.

### <a name="pessimistic-concurrency-locking"></a>Пессимистичный параллелизм (блокировка)

Если приложению нужно предотвратить случайную потерю данных в сценариях параллелизма, одним из способов сделать это являются блокировки базы данных. Это называется *пессимистичный параллелизм*. Например, перед чтением строки из базы данных вы запрашиваете блокировку для доступа для обновления или только для чтения. Если заблокировать строку для обновления, другие пользователи не могут заблокировать ее для обновления или только для чтения, так как получат копию данных, которые находятся в процессе изменения. Если заблокировать строку только для чтения, другие пользователи также могут заблокировать ее только для чтения, но не для обновления.

Управление блокировками имеет свои недостатки. Оно может оказаться сложным с точки зрения программирования. Требуются значительные ресурсы базы данных управления, и он может привести к снижению производительности как количество пользователей приложения увеличивается (то есть он плохо масштабируется). Поэтому не все системы управления базами данных поддерживают пессимистичный параллелизм. Платформа Entity Framework предоставляет нет встроенной поддержки и здесь не показано, как реализовать его.

### <a name="optimistic-concurrency"></a>Оптимистическая блокировка

Альтернативой пессимистичному параллелизму является *оптимистичного параллелизма*. Оптимистическая блокировка допускает появление конфликтов параллелизма, а затем обрабатывает их соответствующим образом. Например, Джон запускает *Department.aspx* странице щелчков **изменить** ссылку для отдела журнал и сократить **бюджета** сумму из 1,000,000.00 $ $ 125,000.00. (Джон Администрирование конкурирующих отдела и хочет освободить деньги свой отдел).

[![Image07](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Прежде чем Джон выбирает **обновление**, Мария выполняет ту же страницу, щелкает **изменить** ссылку журнал отдела, а затем изменения **Дата начала** поле 1/1 / 1/10/2011 г. 1999 г. (Мария Администрирование отделе журнал и хочет ей дополнительные старшинства).

[![Image08](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

Джон выбирает **обновление** во-первых, затем Мария нажимает кнопку **обновления**. Списки теперь браузер Джейн **бюджета** сумму как $1,000,000.00. Однако это неверно, поскольку было изменено сервером Джон сумму в $125,000.00.

Ниже приведены некоторые действия, которые можно предпринять в этом сценарии:

- Вы можете отслеживать, для какого свойства пользователь изменил и обновил только соответствующие столбцы в базе данных. В этом примере сценария данные не будут потеряны, так как эти два пользователя обновляли разные свойства. Далее время кто-то кафедру журнала, он увидит 1/1/1999 и 125,000.00 долл. США. 

    Это поведение по умолчанию в Entity Framework, и он может значительно снизить количество конфликтов, способных привести к потере данных. Тем не менее это поведение не избежать потери данных, если конкурирующие изменения вносятся в то же свойство сущности. Кроме того это не всегда возможно; При сопоставлении хранимых процедур на тип сущности, все свойства сущности обновляются при внесении изменений к сущности в базе данных.
- Вы можете разрешить изменение Джейн перезаписать изменения. После Мария нажимает кнопку **обновление**, **бюджета** сумма возвращается к 1,000,000.00 долл. США. Такой подход называется *победой клиента* или *сохранением последнего внесенного изменения*. (Значения клиента имеют приоритет над в хранилище данных).
- Можно запретить изменение Джейн Дмитрия в базе данных. Как правило будет отображать сообщение об ошибке, Показать ее текущее состояние данных и предоставить ей повторный ввод изменения, если она по-прежнему хочет сделать их. Может дополнительно автоматизировать процесс путем сохранения ввода и предоставляя ей возможность восстановить его не нужно вводить его повторно. Это называется *победой хранилища*. (Значения в хранилище имеют приоритет над данными, передаваемыми клиентом.)

### <a name="detecting-concurrency-conflicts"></a>Обнаружение конфликтов параллелизма

В Entity Framework, конфликты можно разрешать путем обработки `OptimisticConcurrencyException` создаваемые исключения платформы Entity Framework. Чтобы определить, когда именно нужно выдавать исключения, платформа Entity Framework должна быть в состоянии обнаруживать конфликты. Поэтому нужно соответствующим образом настроить базу данных и модель данных. Ниже приведены некоторые варианты для реализации обнаружения конфликтов:

- В базе данных включают столбец таблицы, который может использоваться для определения того, при изменении строки. Затем можно настроить Entity Framework для включения этого столбца в `Where` предложение SQL `Update` или `Delete` команды.

    Это назначение `Timestamp` столбца в `OfficeAssignment` таблицы.

    [![Image09](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

    Тип данных `Timestamp` столбец также называется `Timestamp`. Тем не менее столбец не содержит как фактически значение даты или времени. Вместо этого значение — это число, увеличивающееся каждый раз при обновлении строки. В `Update` или `Delete` команде `Where` предложение включает в себя исходный `Timestamp` значение. Если обновляемая строка была изменена другим пользователем, значение в `Timestamp` отличается от исходного значения, поэтому `Where` предложение возвращает нет строки для обновления. Когда Entity Framework обнаруживает, что строки не были обновлены в текущем `Update` или `Delete` команды (то есть когда число затронутых строк равно нулю), она интерпретирует это как конфликт параллелизма.
- Настройка Entity Framework для включения исходных значений каждого столбца в таблице в `Where` предложении `Update` и `Delete` команд.

    Как и первый вариант, если что-либо в строке изменилось с момента первого чтения `Where` предложение не возвращает строку для обновления, который Entity Framework интерпретирует как конфликт параллелизма. Этот метод является эффективности с помощью `Timestamp` поле, но может оказаться неэффективным. Для таблиц базы данных со множеством столбцов, может привести к очень больших `Where` предложений, и в веб-приложение может потребовать обрабатывать большой объем состояний. Обслуживание большого объема состояний может повлиять на производительность приложения, так как она требует ресурсов сервера (например, состояния сеанса) или должен быть включен в веб-страницы, сам (например, состояние представления).

В этом руководстве вы добавите обработки конфликтов оптимистичного параллелизма для сущности, у которых свойство отслеживания ошибок ( `Department` сущности) и для объекта, который имеет свойство отслеживания ( `OfficeAssignment` сущности).

## <a name="handling-optimistic-concurrency-without-a-tracking-property"></a>Оптимистичный параллелизм обработки без свойства отслеживания

Для реализации оптимистичного параллелизма для `Department` сущности, которые не обеспечивают отслеживания (`Timestamp`) свойство, будет выполнить следующие задачи:

- Изменение модели данных для включения отслеживания для параллелизма `Department` сущностей.
- В `SchoolRepository` класса, обработки исключений параллелизма в `SaveChanges` метод.
- В *Departments.aspx* странице, обработки исключений параллелизма путем отображения сообщения пользователю предупреждение, что попытка изменения завершились с ошибкой. Пользователь может затем просмотреть текущие значения и повторите изменения, если они по-прежнему требуются.

### <a name="enabling-concurrency-tracking-in-the-data-model"></a>Включение отслеживания в модели данных параллелизма

В Visual Studio откройте веб-приложение университета Contoso, работали в предыдущем учебнике этой серии.

Откройте *SchoolModel.edmx*и в конструкторе моделей, щелкните правой кнопкой мыши `Name` свойство в `Department` сущности, а затем нажмите кнопку **свойства**. В **свойства** измените `ConcurrencyMode` свойства `Fixed`.

[![Image16](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Используйте его для других первичных ключей скалярным свойствам (`Budget`, `StartDate`, и `Administrator`.) (Это невозможно для свойств навигации.) Это указывает, что каждый раз, когда Entity Framework приводит к возникновению ошибки `Update` или `Delete` команду SQL, чтобы обновить `Department` сущности в базе данных, эти столбцы (на основе исходных значений) должны быть включены в `Where` предложение. Если строка не обнаружена при `Update` или `Delete` выполняется команда, Entity Framework будет выдано исключение оптимистичного параллелизма.

Сохраните и закройте модели данных.

### <a name="handling-concurrency-exceptions-in-the-dal"></a>Обработка исключений параллелизма в слое DAL

Откройте *SchoolRepository.cs* и добавьте следующие `using` инструкции для `System.Data` пространство имен:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Добавьте следующий новый `SaveChanges` метод, который обрабатывает исключения оптимистичный параллелизм:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

При возникновении ошибки параллелизма при вызове этого метода, значения свойств объекта в памяти будут заменены текущими значениями в базе данных. Параллелизм исключение создается снова, чтобы веб-страницы могут обрабатывать его.

В `DeleteDepartment` и `UpdateDepartment` методы, замените существующий вызов `context.SaveChanges()` вызовом `SaveChanges()` для вызова нового метода.

### <a name="handling-concurrency-exceptions-in-the-presentation-layer"></a>Обработка исключений параллелизма на уровне представления данных

Откройте *Departments.aspx* и добавьте `OnDeleted="DepartmentsObjectDataSource_Deleted"` атрибут `DepartmentsObjectDataSource` элемента управления. Открывающий тег для элемента управления, теперь будет выглядеть примерно так.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

В `DepartmentsGridView` управления, укажите все столбцы таблицы в `DataKeyNames` атрибута, как показано в следующем примере. Обратите внимание, что это будет создано очень больших представление поля состояния, который является одной из причин, почему с помощью поля отслеживания обычно является предпочтительным способом для отслеживания конфликтов параллелизма.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Откройте *Departments.aspx.cs* и добавьте следующие `using` инструкции для `System.Data` пространство имен:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

Добавьте следующий новый метод, который будет вызывать из элемента управления источником данных `Updated` и `Deleted` обработчики событий для обработки исключений параллелизма:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

Этот код проверяет тип исключения, и если это исключение параллелизма в коде динамически создается `CustomValidator` элемент управления, который в свою очередь отображает сообщение в `ValidationSummary` элемента управления.

Вызовите новый метод из `Updated` обработчик событий, который был добавлен ранее. Кроме того, создайте новый `Deleted` обработчик событий, который вызывает метод же (но ничего не делать):

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

### <a name="testing-optimistic-concurrency-in-the-departments-page"></a>Тестирование оптимистичного параллелизма в странице отделов

Запустите *Departments.aspx* страницы.

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

Нажмите кнопку **изменить** в строку и измените значение в **бюджета** столбца. (Помните, что можно изменить только записи, которые вы создали для этого руководства, так как существующий `School` записей базы данных содержит некоторые недопустимые данные. Запись для экономики отдела является безопасном экспериментировать с.)

[![Image18](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

Откройте новое окно браузера и запустить ее еще раз (копирование URL-адрес из первого окна браузера поле "адрес" в окне второго обозревателя).

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Нажмите кнопку **изменить** в одной строке вы изменили ранее и измените **бюджета** значение на другое значение.

[![Image19](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

В окне второго обозревателя нажмите кнопку **обновления**. **Бюджета** сумма успешно изменено на это новое значение.

[![Image20](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

В первом окне браузера щелкните **обновления**. Обновление не выполнено. **Бюджета** сумма отображается повторно с помощью значения, заданного во втором окне браузера, и вы увидите сообщение об ошибке.

[![Image21](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

## <a name="handling-optimistic-concurrency-using-a-tracking-property"></a>Обработка оптимистичного параллелизма, с помощью свойства отслеживания в

Чтобы обрабатывать оптимистичного параллелизма для сущности, которая имеет свойство отслеживания, будет выполнить следующие задачи:

- Добавлять хранимые процедуры в модель данных для управления `OfficeAssignment` сущностей. (Свойства отслеживания и хранимые процедуры не должны использоваться совместно; они просто группируются вместе здесь для иллюстрации).
- Добавить CRUD-методы DAL и BLL для `OfficeAssignment` сущностями, включая код для обработки исключений оптимистичного параллелизма в слое DAL.
- Создайте веб-страницу office назначения.
- Протестируйте оптимистичного параллелизма в новой веб-страницы.

### <a name="adding-officeassignment-stored-procedures-to-the-data-model"></a>Добавление OfficeAssignment хранимые процедуры в модель данных

Откройте *SchoolModel.edmx* файл в конструкторе моделей, щелкните правой кнопкой мыши область конструктора и нажмите кнопку **обновить модель из базы данных**. В **добавить** вкладке **Выбор объектов базы данных** диалогового окна разверните узел **хранимые процедуры** и выберите три `OfficeAssignment` хранимые процедуры (см. в разделе см. снимок экрана), а затем нажмите кнопку **Готово**. (Эти хранимые процедуры были уже в базе данных вы скачали или создали с помощью сценария.)

[![Image02](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Щелкните правой кнопкой мыши `OfficeAssignment` сущности и выберите **сопоставление хранимых процедур**.

[![Image03](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Задайте **вставить**, **обновление**, и **удалить** функции для использования соответствующих хранимых процедур. Для `OrigTimestamp` параметр `Update` установите **свойство** для `Timestamp` и выберите **использовать исходное значение** параметр.

[![Image04](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image30.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image29.png)

Когда платформа Entity Framework вызывает `UpdateOfficeAssignment` хранимой процедуры, оно будет передавать исходное значение `Timestamp` столбца в `OrigTimestamp` параметра. Хранимая процедура использует этот параметр в его `Where` предложение:

[!code-sql[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.sql)]

Хранимая процедура также устанавливает новое значение `Timestamp` столбца после обновления, чтобы платформа Entity Framework можно хранить `OfficeAssignment` сущность, которая находится в памяти в соответствии с соответствующей строки базы данных.

(Обратите внимание, что хранимую процедуру для удаления назначение кабинета не имеет `OrigTimestamp` параметра. По этой причине Entity Framework не удалось проверить что сущность не изменяется перед его удалением.)

Сохраните и закройте модели данных.

### <a name="adding-officeassignment-methods-to-the-dal"></a>Добавление OfficeAssignment методов DAL

Откройте *ISchoolRepository.cs* и добавьте следующие методы CRUD для `OfficeAssignment` набор сущностей:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Добавьте следующие новые методы для *SchoolRepository.cs*. В `UpdateOfficeAssignment` , вызываемый метод локальной `SaveChanges` вместо метода `context.SaveChanges`.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

В тестовом проекте откройте *MockSchoolRepository.cs* и добавьте следующие `OfficeAssignment` коллекции и методы CRUD к нему. (Фиктивного репозитория должен реализовывать интерфейс репозитория или решение не будет компилироваться.)

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

### <a name="adding-officeassignment-methods-to-the-bll"></a>Добавление OfficeAssignment методов BLL

В главном проекте откройте *SchoolBL.cs* и добавьте следующие методы CRUD для `OfficeAssignment` набора сущностей, к нему:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

## <a name="creating-an-officeassignments-web-page"></a>Создание веб-страницу OfficeAssignments

Создать новый веб-страницу, который использует *Site.Master* главную страницу и назовите его *OfficeAssignments.aspx*. Добавьте следующую разметку для `Content` управления с именем `Content2`:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.aspx)]

Обратите внимание, что в `DataKeyNames` атрибут, разметка определяет `Timestamp` свойства, а также ключ записи (`InstructorID`). Указание свойств в `DataKeyNames` атрибут заставляет элемент управления для их сохранения в состоянии элемента управления (который аналогичен состояния представления), чтобы исходные значения были доступны во время обработки обратной передачи.

Если вы не сохраните `Timestamp` значение, Entity Framework не будет его для `Where` предложение SQL `Update` команды. Поэтому ничего не может найти для обновления. Таким образом, платформа Entity Framework может генерировать исключение оптимистичного параллелизма при каждом `OfficeAssignment` сущность обновляется.

Откройте *OfficeAssignments.aspx.cs* и добавьте следующие `using` инструкции для уровня доступа к данным:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Добавьте следующий `Page_Init` метод, который включает функциональные возможности платформы динамических данных. Также добавьте следующий обработчик для `ObjectDataSource` элемента управления `Updated` событий, чтобы проверить наличие ошибок параллелизма:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

### <a name="testing-optimistic-concurrency-in-the-officeassignments-page"></a>Тестирование на странице OfficeAssignments оптимистичного параллелизма

Запустите *OfficeAssignments.aspx* страницы.

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image32.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image31.png)

Нажмите кнопку **изменить** в строку и измените значение в **расположение** столбца.

[![Image11](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image34.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image33.png)

Откройте новое окно браузера и запустить ее еще раз (копирование URL-адрес из первого окна браузера, в окне второго обозревателя).

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image36.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image35.png)

Нажмите кнопку **изменить** в одной строке вы изменили ранее и измените **расположение** значение на другое значение.

[![Image12](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image38.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image37.png)

В окне второго обозревателя нажмите кнопку **обновления**.

[![Image13](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image40.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image39.png)

Перейдите в первое окно браузера и нажмите кнопку **обновления**.

[![Image15](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image42.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image41.png)

Вы видите сообщение об ошибке и **расположение** значение был обновлен для отображения значения, вы изменили его во втором окне браузера.

## <a name="handling-concurrency-with-the-entitydatasource-control"></a>Обработка параллелизма с помощью элемента управления EntityDataSource

`EntityDataSource` Управления включает встроенную логику, которое распознает параметры параллелизма в модели данных и дескрипторов операций обновления и удаления соответствующим образом. Тем не менее, как и все исключения, необходимо обрабатывать `OptimisticConcurrencyException` исключения самостоятельно, чтобы обеспечить понятное сообщение об ошибке.

Далее вы настроите *Courses.aspx* страницы (которая использует `EntityDataSource` управления) разрешить обновление и удаление операций и отображается сообщение об ошибке в том случае, если происходит конфликт параллелизма. `Course` Сущности не указан параллелизма, отслеживания столбца, чтобы использовать этот же метод, это было сделано `Department` сущности: отслеживать значения всех свойств, не содержащих ключи.

Откройте *SchoolModel.edmx* файла. Для не содержащих ключи свойств `Course` сущности (`Title`, `Credits`, и `DepartmentID`), задайте **режим параллелизма** свойства `Fixed`. Затем сохраните и закройте модели данных.

Откройте *Courses.aspx* странице и внесите следующие изменения:

- В `CoursesEntityDataSource` управления, добавьте `EnableUpdate="true"` и `EnableDelete="true"` атрибуты. Открывающий тег для элемента управления, теперь, аналогичный приведенному ниже:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample16.aspx)]
- В `CoursesGridView` управления, измените `DataKeyNames` значение для атрибута `"CourseID,Title,Credits,DepartmentID"`. Затем добавьте `CommandField` элемент `Columns` элемент, который показывает **изменить** и **удалить** кнопки (`<asp:CommandField ShowEditButton="True" ShowDeleteButton="True" />`). `GridView` Управления теперь аналогичный приведенному ниже:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample17.aspx)]

Откройте страницу и создать такую ситуацию, что и на странице отделов. Запустите страницу в двух окнах браузера, щелкните **изменить** в одной строки в каждом окне и внести другое изменение в каждом из них. Нажмите кнопку **обновление** в одном окне и нажмите кнопку **обновления** в другом окне. При нажатии кнопки **обновления** во второй раз, вы увидите страницу ошибки, полученный в результате параллелизма необработанное исключение.

[![Image22](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image44.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image43.png)

Обработать эту ошибку в очень похоже на способ обработки в режиме `ObjectDataSource` элемента управления. Откройте *Courses.aspx* страницы и в `CoursesEntityDataSource` элемента управления, укажите обработчики для `Deleted` и `Updated` события. Открывающий тег элемента управления, теперь, аналогичный приведенному ниже:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample18.aspx)]

Прежде чем `CoursesGridView` управления, добавьте следующий `ValidationSummary` управления:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample19.aspx)]

В *Courses.aspx.cs*, добавьте `using` инструкции для `System.Data` пространства имен, добавьте метод, который проверяет для параллельной обработки исключений и добавьте обработчики для `EntityDataSource` элемента управления `Updated` и `Deleted`обработчиков. Код будет выглядеть следующим образом:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample20.cs)]

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample21.cs)]

Единственное различие между этим кодом и что вы сделали `ObjectDataSource` управления является то, что в этом случае исключение параллелизма в `Exception` свойства объекта аргумента события, а не в этом исключении `InnerException` свойство.

Запустить эту страницу и снова создайте конфликт параллелизма. Это время вы видите сообщение об ошибке:

[![Image23](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image46.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image45.png)

На этом заканчивается введение в обработку конфликтов параллелизма. Следующее руководство будут представлены рекомендации по повышению производительности в веб-приложение, использующее Entity Framework.

> [!div class="step-by-step"]
> [Назад](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
> [Вперед](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
