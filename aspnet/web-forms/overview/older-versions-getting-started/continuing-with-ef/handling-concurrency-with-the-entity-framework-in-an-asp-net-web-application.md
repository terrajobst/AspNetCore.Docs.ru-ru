---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
title: Обработка параллелизма с платформой Entity Framework 4.0 в 4 веб-приложение ASP.NET | Документы Microsoft
author: tdykstra
description: Этот учебник ряд строится на веб-приложение Contoso университета, созданный Приступая к работе с рядами учебника Entity Framework 4.0. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: a5aa22a6-fb7f-4f41-9c7f-addda151940b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: f40695270006e4f8b0c9ad8e94049e5239f06e63
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="handling-concurrency-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Обработка параллелизма с платформой Entity Framework 4.0 в 4 веб-приложение ASP.NET
====================
по [Tom Dykstra](https://github.com/tdykstra)

> Этот учебник ряд основан на веб-приложение Contoso университета, созданный [Приступая к работе с Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) учебника рядов. Если не была завершена ранее учебники, в качестве отправной точки для этого учебника вы можете [загрузить приложение](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , будет создана. Вы также можете [загрузить приложение](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , создаваемый для завершения учебника ряда. Если у вас есть вопросы о учебники, их можно разместить [форум по ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


В предыдущем учебнике вы узнали, как для сортировки и фильтрации данных с помощью `ObjectDataSource` управления и Entity Framework. Этот учебник показывает параметры для обработки параллелизма в веб-приложение ASP.NET, использующего платформу Entity Framework. Вы создадите новый веб-страницы, предназначенный для обновления инструктора аудиторий. Будет обрабатывать проблем параллелизма на этой странице и на странице отделов, которое было создано ранее.

[![Image06](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

[![Image01](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

## <a name="concurrency-conflicts"></a>Конфликты параллелизма

Конфликт параллелизма возникает, когда один пользователь изменяет запись и другой пользователь изменяет ту же запись, прежде чем изменения первого пользователя записывается в базу данных. Если не настроить Entity Framework для обнаружения таких конфликтов, кто обновляет базу данных, перезаписывает изменения другого пользователя. Во многих приложениях этот риск является допустимым и не нужно настроить приложение для обработки конфликтов параллелизма возможно. (Если существует несколько пользователей или несколько обновлений или не действительно являются критическими, если некоторые изменения будут перезаписаны, стоимость программирования для параллелизма может перевесить преимущества.) Если не нужно беспокоиться о конфликтах параллелизма, можно пропустить этот учебник; Оставшиеся два учебника ряда не зависят от ничего, создаваемые в данном файле.

### <a name="pessimistic-concurrency-locking"></a>Пессимистическая блокировка (блокировки)

Если приложению нужно предотвратить случайную потерю данных в сценариях параллелизма, одним из способов сделать это являются блокировки базы данных. Это называется *пессимистичный параллелизм*. Например, перед чтением строки из базы данных вы запрашиваете блокировку для доступа для обновления или только для чтения. Если заблокировать строку для обновления, другие пользователи не могут заблокировать ее для обновления или только для чтения, так как получат копию данных, которые находятся в процессе изменения. Если заблокировать строку только для чтения, другие пользователи также могут заблокировать ее только для чтения, но не для обновления.

Управление блокировками имеет свои недостатки. Оно может оказаться сложным с точки зрения программирования. Она требует значительные ресурсы базы данных управления, и он может вызвать снижение производительности как число пользователей приложения увеличивает (то есть, он плохо масштабируется). Поэтому не все системы управления базами данных поддерживают пессимистичный параллелизм. Платформа Entity Framework предоставляет нет встроенная поддержка и учебнике не показано, как для его реализации.

### <a name="optimistic-concurrency"></a>Оптимистическая блокировка

Вместо него следует использовать для пессимистичный параллелизм *оптимистичного параллелизма*. Оптимистическая блокировка допускает появление конфликтов параллелизма, а затем обрабатывает их соответствующим образом. Например, Джон запускает *Department.aspx* страницы щелчков **изменить** связать подразделения, журнал и уменьшает **бюджета** сумму из 1,000,000.00 $ $ 125,000.00. (Джон Администрирование конкурирующих отдел и хочет освободить money его собственному отдела).

[![Image07](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Прежде чем Джон выбирает **обновление**, Мария выполняет ту же страницу, щелкает **изменить** ссылку журнала отдела, а затем изменения **Дата начала** поля из 1/10/2011 1/1 / 1999 г. (Мария Администрирование отдел журнала и хочет предоставить ей дополнительные стажа.)

[![Image08](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

Джон выбирает **обновление** сначала Мария щелкает **обновление**. Списки теперь браузер Джейн **бюджета** как $1,000,000.00, однако это неверно, поскольку размер был изменен, Джон на 125,000.00 $.

Ниже перечислены некоторые действия, которые можно выполнять в этом сценарии.

- Вы можете отслеживать, для какого свойства пользователь изменил и обновил только соответствующие столбцы в базе данных. В этом примере сценария данные не будут потеряны, так как эти два пользователя обновляли разные свойства. Далее время кто-то просматривает журнал отдела, появится 1/1/1999 и 125,000.00 $. 

    Это поведение по умолчанию в платформе Entity Framework, и он может существенно уменьшить количество конфликтов, которые могут привести к потере данных. Однако такое поведение не избежать потери данных, если конкурирующих изменений в то же свойство сущности. Кроме того это не всегда возможно; При сопоставлении хранимых процедур на тип сущности, все свойства сущности обновляются при внесении изменений в сущности в базе данных.
- Можно перезаписать изменения Джона изменение Джейн. После нажатия кнопки Мария **обновление**, **бюджета** сумма возвращается к 1,000,000.00 $. Такой подход называется *победой клиента* или *сохранением последнего внесенного изменения*. (Значения клиента имеют приоритет над возможности хранилища данных).
- Можно запретить изменение Джейн обновляется в базе данных. Как правило следует отобразить сообщение об ошибке, отображения его текущего состояния данных и разрешить ей введите своих изменений, если она по-прежнему хочет сделать их. Дополнительно может автоматизировать процесс путем сохранения своих входных данных и что ей дает возможность восстановить его без повторного ввода его. Это называется *победой хранилища*. (Значения в хранилище имеют приоритет над данными, передаваемыми клиентом.)

### <a name="detecting-concurrency-conflicts"></a>Обнаружение конфликтов параллелизма

В Entity Framework, может разрешить конфликт, обработка `OptimisticConcurrencyException` исключения, которые выдает Entity Framework. Чтобы определить, когда именно нужно выдавать исключения, платформа Entity Framework должна быть в состоянии обнаруживать конфликты. Поэтому нужно соответствующим образом настроить базу данных и модель данных. Ниже приведены некоторые варианты для реализации обнаружения конфликтов:

- В базе данных включают столбец таблицы, который может использоваться для определения момента изменения строки. Затем можно настроить для включения этого столбца в Entity Framework `Where` предложение SQL `Update` или `Delete` команд.

    Это назначение `Timestamp` столбца в `OfficeAssignment` таблицы.

    [![Image09](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

    Тип данных `Timestamp` столбца называется также `Timestamp`. Однако столбец фактически не содержит значение даты или времени. Вместо этого значение — это число, увеличивающееся каждый раз, когда строка обновляется. В `Update` или `Delete` команды `Where` предложение включает в себя исходный `Timestamp` значение. Если были изменены другим пользователем, значение в обновляемой строке `Timestamp` отличается от исходного значения, поэтому `Where` предложение возвращает нет строки для обновления. Когда Entity Framework находит, что строки не были обновлены в текущем `Update` или `Delete` команды (то есть, когда количество задействованных строк равно нулю), он интерпретирует как конфликт параллелизма.
- Настройка включения исходных значений для каждого из столбцов в таблице в платформе Entity Framework `Where` предложения `Update` и `Delete` команд.

    Как и первый вариант, если что-либо в строке изменилась с момента прочтите строки `Where` предложение не возвращает строки для обновления, который Entity Framework интерпретирует как конфликт параллелизма. Этот метод является эффективности с помощью `Timestamp` поле, но может оказаться неэффективным. Для таблиц базы данных, имеющие много столбцов, это может привести к очень больших `Where` предложений, и в веб-приложение может потребовать необходимо хранить большие объемы состояния. Обслуживание больших объемов состояния может повлиять на производительность приложения, так как он требует ресурсов сервера (например, состояния сеанса) или должен быть включен в веб-странице сам (например, состояние представления).

В этом учебнике вы добавите обработки конфликтов оптимистичного параллелизма для сущности, которая не имеет свойство отслеживания ошибок ( `Department` сущности) и для объекта, который имеет свойство отслеживания ( `OfficeAssignment` сущности).

## <a name="handling-optimistic-concurrency-without-a-tracking-property"></a>Оптимистический параллелизм обработки без свойства отслеживания

Для реализации оптимистичного параллелизма для `Department` сущности, которая не имеет отслеживания (`Timestamp`) свойства, выполняются следующие задачи:

- Измените модель данных для включения отслеживания для параллелизма `Department` сущностей.
- В `SchoolRepository` класса обработки исключений параллелизма в `SaveChanges` метод.
- В *Departments.aspx* страницы, обработки исключений параллелизма путем отображения сообщения пользователю предупреждение, что попытка не был изменен. Пользователь может затем просмотреть текущие значения и повторите изменения, если они по-прежнему требуются.

### <a name="enabling-concurrency-tracking-in-the-data-model"></a>Включение отслеживания в модели данных параллелизма

В Visual Studio откройте веб-приложение Contoso университета, велась работа в предыдущем учебнике этой серии.

Откройте *SchoolModel.edmx*и в конструкторе моделей щелкните правой кнопкой мыши `Name` свойство в `Department` сущности, а затем нажмите кнопку **свойства**. В **свойства** измените `ConcurrencyMode` свойства `Fixed`.

[![Image16](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Сделайте то же самое для других свойств скалярные первичного ключа (`Budget`, `StartDate`, и `Administrator`.) (Вы не поддерживается для свойств навигации.) Указывает, что каждый раз, когда платформа Entity Framework приводит к возникновению ошибки `Update` или `Delete` команда SQL для обновления `Department` сущности в базе данных (на основе исходных значений) эти столбцы должны быть включены в `Where` предложения. Если строка не обнаружена при `Update` или `Delete` выполняется команда, Entity Framework будет создано исключение оптимистичного параллелизма.

Сохраните и закройте модели данных.

### <a name="handling-concurrency-exceptions-in-the-dal"></a>Обработка исключений с параллелизмом в DAL

Откройте *SchoolRepository.cs* и добавьте следующее `using` инструкции для `System.Data` пространство имен:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Добавьте следующий `SaveChanges` метод, который обрабатывает исключения оптимистической блокировки:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

При возникновении ошибки параллелизма при вызове этого метода, значения свойств объекта в памяти заменяются значениями базы данных. Параллелизм создается исключение, чтобы веб-страницы могут обрабатывать ее.

В `DeleteDepartment` и `UpdateDepartment` методы, замените существующий вызов `context.SaveChanges()` с помощью вызова `SaveChanges()` для вызова нового метода.

### <a name="handling-concurrency-exceptions-in-the-presentation-layer"></a>Обработка исключений параллельного доступа на уровне представления данных

Откройте *Departments.aspx* и добавьте `OnDeleted="DepartmentsObjectDataSource_Deleted"` атрибут `DepartmentsObjectDataSource` элемента управления. Открывающий тег для элемента управления теперь будет выглядеть примерно так.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

В `DepartmentsGridView` контроля, указания всех столбцов таблицы в `DataKeyNames` атрибута, как показано в следующем примере. Обратите внимание, что это будет создано очень больших представление поля состояния, это одна из причин, поэтому с помощью поля отслеживания обычно является предпочтительным способом для отслеживания конфликтов параллелизма.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Откройте *Departments.aspx.cs* и добавьте следующее `using` инструкции для `System.Data` пространство имен:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

Добавьте следующий новый метод, который будет вызываться из элемента управления источником данных `Updated` и `Deleted` обработчики событий для обработки исключений, параллелизма:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

Этот код проверяет тип исключения, и если это исключение параллелизма в коде создается динамически `CustomValidator` управления, который в свою очередь отображает сообщение в `ValidationSummary` элемента управления.

Вызовите новый метод из `Updated` обработчик событий, который был добавлен ранее. Кроме того, создайте новый `Deleted` обработчика событий, который вызывает тот же метод (но не выполняет никаких действий else):

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

### <a name="testing-optimistic-concurrency-in-the-departments-page"></a>Тестирование оптимистичного параллелизма в отделах страницы

Запустите *Departments.aspx* страницы.

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

Нажмите кнопку **изменить** в строку и измените значение в **бюджета** столбца. (Помните, что можно изменить только записей, созданных для этого учебника, поскольку существующий `School` записи базы данных содержат неверные данные. Запись для подразделения экономику является безопасным для экспериментов с.)

[![Image18](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

Откройте новое окно браузера и запустите страницу еще раз (Копировать URL-адрес в поле адреса первое окно браузера второе окно браузера).

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Нажмите кнопку **изменить** так же можно изменить ранее и измените **бюджета** нечто иное значение.

[![Image19](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Во втором окне браузера щелкните **обновление**. **Бюджета** сумма успешно изменена для этого нового значения.

[![Image20](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

В первом окне браузера щелкните **обновление**. Обновление не выполнено. **Бюджета** сумма отображается повторно с использованием значения, заданного параметром второе окно браузера и отображается сообщение об ошибке.

[![Image21](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

## <a name="handling-optimistic-concurrency-using-a-tracking-property"></a>Обработка с помощью свойства отслеживания оптимистичного параллелизма

Чтобы обработать оптимистичного параллелизма для сущности, которая имеет свойство отслеживания, выполняются следующие задачи.

- Добавлять хранимые процедуры в модель данных для управления `OfficeAssignment` сущностей. (Свойства отслеживания и хранимые процедуры не должны использоваться совместно, они просто группируются вместе здесь для иллюстрации.)
- Добавьте методы CRUD DAL, а также МЕТОД для `OfficeAssignment` сущностям, включая код для обработки исключений оптимистичного параллелизма в DAL.
- Создайте веб-страницу назначения office.
- Проверьте оптимистичного параллелизма в новую веб-страницу.

### <a name="adding-officeassignment-stored-procedures-to-the-data-model"></a>Добавление OfficeAssignment хранимых процедур в модель данных

Откройте *SchoolModel.edmx* файл в конструкторе моделей, щелкните правой кнопкой мыши область конструктора и щелкните **обновление модели из базы данных**. В **добавить** вкладке **Выбор объектов базы данных** диалогового окна разверните **хранимых процедур** и выберите три `OfficeAssignment` хранимые процедуры (см. Следующий снимок экрана), а затем нажмите кнопку **Готово**. (Эти хранимые процедуры уже есть в базе данных при загрузке или создан с помощью сценария.)

[![Image02](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Щелкните правой кнопкой мыши `OfficeAssignment` сущность и выберите **сопоставление хранимых процедур**.

[![Image03](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Задать **вставить**, **обновление**, и **удалить** функции для использования соответствующих хранимых процедур. Для `OrigTimestamp` параметр `Update` функцию, задайте **свойство** для `Timestamp` и выберите **использовать исходное значение** параметр.

[![Image04](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image30.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image29.png)

Если Entity Framework вызывает `UpdateOfficeAssignment` хранимая процедура пройдет исходное значение `Timestamp` столбца в `OrigTimestamp` параметра. Хранимая процедура использует этот параметр в его `Where` предложения:

[!code-sql[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.sql)]

Хранимая процедура также устанавливает новое значение `Timestamp` столбца после обновления, чтобы платформа Entity Framework, можно сохранить `OfficeAssignment` сущность, которая находится в памяти в соответствии с соответствующей строки базы данных.

(Обратите внимание, что хранимую процедуру для удаления назначения office не имеет `OrigTimestamp` параметра. По этой причине Entity Framework не удалось проверить сущности, которые не изменяется перед его удалением.)

Сохраните и закройте модели данных.

### <a name="adding-officeassignment-methods-to-the-dal"></a>Добавление методов OfficeAssignment DAL

Откройте *ISchoolRepository.cs* и добавьте следующие методы CRUD `OfficeAssignment` набора сущностей:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Добавьте следующие новые методы для *SchoolRepository.cs*. В `UpdateOfficeAssignment` , вызываемый метод локальной `SaveChanges` вместо метода `context.SaveChanges`.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

В тестовом проекте откройте *MockSchoolRepository.cs* и добавьте следующее `OfficeAssignment` коллекции и методы CRUD. (Макета репозитория должен реализовывать интерфейс репозитория или решение не может быть скомпилирован.)

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

### <a name="adding-officeassignment-methods-to-the-bll"></a>Добавление методов OfficeAssignment МЕТОДА

В главном проекте откройте *SchoolBL.cs* и добавьте следующие методы CRUD `OfficeAssignment` набора сущностей, к нему:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

## <a name="creating-an-officeassignments-web-page"></a>Создание веб-страницу OfficeAssignments

Создать новую веб-страницу, которая использует *Site.Master* главную страницу и назовите его *OfficeAssignments.aspx*. Добавьте следующую разметку, чтобы `Content` управления с именем `Content2`:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.aspx)]

Обратите внимание, что в `DataKeyNames` атрибут задает разметку `Timestamp` свойства, а также ключ записи (`InstructorID`). Задание свойств в `DataKeyNames` атрибут заставляет элемент управления сохраняет их в состояние элемента управления (Это похоже на состояние просмотра), чтобы исходные значения были доступны во время обработки обратной передачи.

Если не был сохранен `Timestamp` значение, Entity Framework будет отсутствовать в `Where` предложение SQL `Update` команды. Поэтому ничего не может найти для обновления. В результате Entity Framework может генерировать исключение оптимистичного параллелизма при каждом `OfficeAssignment` обновления сущности.

Откройте *OfficeAssignments.aspx.cs* и добавьте следующее `using` инструкции для доступа к данным:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Добавьте следующие `Page_Init` метод, который включает функциональные возможности платформы динамических данных. Также добавьте следующий обработчик для `ObjectDataSource` элемента управления `Updated` событий для проверки ошибок параллелизма:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

### <a name="testing-optimistic-concurrency-in-the-officeassignments-page"></a>Тестирование на странице OfficeAssignments оптимистичного параллелизма

Запустите *OfficeAssignments.aspx* страницы.

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image32.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image31.png)

Нажмите кнопку **изменить** в строку и измените значение в **расположение** столбца.

[![Image11](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image34.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image33.png)

Откройте новое окно браузера и запустите страницу еще раз (Копировать URL-адрес из первого окна браузера второе окно браузера).

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image36.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image35.png)

Нажмите кнопку **изменить** так же можно изменить ранее и измените **расположение** нечто иное значение.

[![Image12](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image38.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image37.png)

Во втором окне браузера щелкните **обновление**.

[![Image13](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image40.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image39.png)

Перейдите в первое окно браузера и нажмите кнопку **обновление**.

[![Image15](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image42.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image41.png)

Отображается сообщение об ошибке и **расположение** значение был обновлен для отображения значения его для изменения во второе окно браузера.

## <a name="handling-concurrency-with-the-entitydatasource-control"></a>Обработка параллелизма с помощью элемента управления EntityDataSource

`EntityDataSource` Управления включает встроенную логику, который распознает параметры параллелизма в модели данных и дескрипторы операций обновления и удаления соответствующим образом. Тем не менее, как и все исключения, необходимо обрабатывать `OptimisticConcurrencyException` исключения самостоятельно, чтобы обеспечить понятное сообщение.

Далее следует настроить *Courses.aspx* страницы (которая использует `EntityDataSource` управления) позволяют обновление и удаление операций и для отображения сообщения об ошибке, если возникает конфликт параллелизма. `Course` Сущность не имеет параллелизма, отслеживание столбцов, поэтому будет использовать тот же метод, как и на `Department` сущности: отслеживания значений все неключевые свойства.

Откройте *SchoolModel.edmx* файла. Для свойств неключевыми `Course` сущности (`Title`, `Credits`, и `DepartmentID`), установите **режим параллелизма** свойства `Fixed`. Затем сохраните и закройте модели данных.

Откройте *Courses.aspx* страницы и внесите следующие изменения:

- В `CoursesEntityDataSource` управления, добавьте `EnableUpdate="true"` и `EnableDelete="true"` атрибуты. Открывающий тег для этого элемента управления теперь приведенной в следующем примере:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample16.aspx)]
- В `CoursesGridView` контроля, изменить `DataKeyNames` значение для атрибута `"CourseID,Title,Credits,DepartmentID"`. Затем добавьте `CommandField` элемент `Columns` элемент, который показывает **изменить** и **удалить** кнопок (`<asp:CommandField ShowEditButton="True" ShowDeleteButton="True" />`). `GridView` Теперь элемент управления похож на приведенный ниже:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample17.aspx)]

Запустите страницу и создать такую ситуацию, как это делалось ранее на странице отделов. Запустите страницу в двух окнах браузера, щелкните **изменить** в одной строки в каждом окне и сделать другое изменение в каждом из них. Нажмите кнопку **обновление** в одном окне, а затем выберите **обновление** в другом окне. При нажатии кнопки **обновление** во второй раз, отображается страница ошибки, полученной в результате параллелизма необработанное исключение.

[![Image22](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image44.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image43.png)

Обрабатывать ошибки в очень похожа на обработку в режиме `ObjectDataSource` элемента управления. Откройте *Courses.aspx* страницы и в `CoursesEntityDataSource` управления, укажите обработчики для `Deleted` и `Updated` события. Открывающий тег элемента управления теперь приведенной в следующем примере:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample18.aspx)]

Прежде чем `CoursesGridView` управления, добавьте следующий `ValidationSummary` управления:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample19.aspx)]

В *Courses.aspx.cs*, добавьте `using` инструкции для `System.Data` пространства имен, добавьте метод, который проверяет, для исключений параллелизма и добавьте обработчики для `EntityDataSource` элемента управления `Updated` и `Deleted`обработчиков. Код будет выглядеть следующим образом:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample20.cs)]

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample21.cs)]

Единственное различие между этим кодом и что вы сделали `ObjectDataSource` управления является в этом случае исключение параллелизма в `Exception` свойство объекта аргументы события, а не в это исключение `InnerException` свойство.

Запустите страницу и снова создать конфликт параллелизма. Это время отображается сообщение об ошибке:

[![Image23](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image46.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image45.png)

На этом заканчивается введение в обработку конфликтов параллелизма. Следующее руководство будут представлены рекомендации по повышению производительности веб-приложения, использующего платформу Entity Framework.

> [!div class="step-by-step"]
> [Назад](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
> [Вперед](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
