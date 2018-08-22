---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
title: Начало работы с Entity Framework 4.0 Database First и ASP.NET 4 Web Forms — часть 2 | Документация Майкрософт
author: tdykstra
description: Пример веб-приложение университета Contoso демонстрирует создание приложений веб-форм ASP.NET, с помощью Entity Framework. Пример приложения является...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: fb63a326-a4ae-4b0c-a4f5-412327197216
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
msc.type: authoredcontent
ms.openlocfilehash: a0b4acca93deee4fb514fa1bc3e2bd13490cf10e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828493"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-2"></a>Начало работы с Entity Framework 4.0 Database First и ASP.NET 4 Web Forms — часть 2
====================
по [том Дайкстра](https://github.com/tdykstra)

> Пример веб-приложение университета Contoso демонстрирует создание приложения веб-форм ASP.NET, используя Entity Framework 4.0 и Visual Studio 2010. Сведения об этой серии руководств см. в разделе [в первом учебнике серии](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="the-entitydatasource-control"></a>Элемент управления EntityDataSource

В предыдущем руководстве вы создали веб-сайт, базу данных и модель данных. В этом руководстве вы работаете с `EntityDataSource` элемента управления, предоставляемых ASP.NET для упрощения работы с моделью Entity Framework. Вы создадите `GridView` управления для отображения и редактирования данных об учащихся `DetailsView` управления для добавления новых студентов и `DropDownList` управления для выбора подразделения (которая будет использоваться в дальнейшем для отображения связанных курсов).

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image1.png)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image3.png)

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image5.png)

Обратите внимание, что в этом приложении вы не добавления проверки входных данных на страницы, обновить базу данных и обработка ошибок не будет настолько надежно, как потребовался бы в рабочем приложении. Который отслеживает руководства, посвященные Entity Framework и предотвращает получение слишком много времени. Дополнительные сведения о том, как добавить эти функции в приложение, см. в разделе [проверка пользовательского ввода в ASP.NET Web Pages](https://msdn.microsoft.com/library/7kh55542.aspx) и [обработка ошибок в страницах ASP.NET и приложений](https://msdn.microsoft.com/library/w16865z6.aspx).

## <a name="adding-and-configuring-the-entitydatasource-control"></a>Добавление и настройка элемента управления EntityDataSource

Сначала вы создадите, настроив `EntityDataSource` управления для чтения `Person` сущностей из `People` набора сущностей.

Убедитесь, что у вас есть Visual Studio открыта, и что вы работаете с проектом, созданной в первой части. Если проект еще не создан, с момента создания модели данных или с момента последнего изменения, внесенные в него, сборку проекта прямо сейчас. Изменения в модель данных не становятся доступными в конструктор до построения проекта.

Создайте новый веб-страницы с помощью **веб-форма, использующая главную страницу** шаблона и назовите его *Students.aspx*.

[![Image23](the-entity-framework-and-aspnet-getting-started-part-2/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image7.png)

Укажите *Site.Master* как Главная страница. Все страницы, созданные для этих учебников будет использовать эту главную страницу.

[![Image24](the-entity-framework-and-aspnet-getting-started-part-2/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image9.png)

В **источника** просматривать, добавлять `h2` заголовок для `Content` управления с именем `Content2`, как показано в следующем примере:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample1.aspx)]

Из **данных** вкладке **элементов**, перетащите `EntityDataSource` к странице, поместите его под заголовком и измените идентификатор на `StudentsEntityDataSource`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample2.aspx)]

Переключиться в режим **разработки** просмотра, щелкните смарт-тег элемента управления источником данных и нажмите кнопку **Настройка источника данных** для запуска **Настройка источника данных** мастера.

[!["Image01"](the-entity-framework-and-aspnet-getting-started-part-2/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image11.png)

В **Настройка ObjectContext** шага мастера, выберите **SchoolEntities** как значение для **именованное соединение**и выберите **SchoolEntities**как **DefaultContainerName** значение. Затем нажмите кнопку **Далее**.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-2/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image13.png)

Примечание: Если на этом этапе появится следующее диалоговое окно, необходимо выполнить сборку проекта перед продолжением.

[![Image25](the-entity-framework-and-aspnet-getting-started-part-2/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image15.png)

В **Настройка отбора данных** выберите **людей** как значение для **EntitySetName**. В разделе **выберите**, убедитесь, что **выберите объект** ll флажок. Затем выберите параметры для включения обновления и удаления. Когда все будет готово, нажмите кнопку **Готово**.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-2/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image17.png)

## <a name="configuring-database-rules-to-allow-deletion"></a>Настройка правил базы данных, чтобы разрешить удаление

Вы создадите страницу, которая позволяет пользователям удалить учащихся из `Person` таблицу, которая содержит три связи с другими таблицами (`Course`, `StudentGrade`, и `OfficeAssignment`). По умолчанию базы данных будет не позволяют удалить строку в `Person` при наличии связанных строк в одной из других таблиц. Можно вручную удалить связанные строки во-первых, или можно настроить базу данных, чтобы удалить их автоматически при удалении `Person` строки. Для записей учащихся в этом руководстве вы настроите базы данных, чтобы автоматически удалять связанные данные. Поскольку учащиеся могут связанные с ней строки только в `StudentGrade` таблицы, необходимо настроить только один из трех связи.

Если вы используете *файл School.mdf* файл, загруженный из проекта, который переходит к этому учебнику, этот раздел можно пропустить, так как эти изменения конфигурации, уже выполнены. Если база данных создана путем выполнения сценария, необходимо Настройте базу данных, выполнив следующие процедуры.

В **обозревателя серверов**, открытие диаграммы базы данных, созданной в первой части. Щелкните правой кнопкой мыши связь между `Person` и `StudentGrade` (линии между таблицами), а затем выберите **свойства**.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-2/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image19.png)

В **свойства** окне разверните **спецификация INSERT и UPDATE** и задайте **DeleteRule** свойства **Cascade**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-2/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image21.png)

Сохраните и закройте диаграмму. При появлении запроса ли вы хотите обновить базу данных, нажмите кнопку **Да**.

Чтобы убедиться в том, что модель сохраняет сущности, находящиеся в памяти, в соответствии с действиями базы данных, необходимо задать соответствующие правила в модели данных. Откройте *SchoolModel.edmx*, щелкните правой кнопкой мыши линию связи между `Person` и `StudentGrade`, а затем выберите **свойства**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-2/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image23.png)

В **свойства** окне **End1 OnDelete** для **Cascade**.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-2/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image25.png)

Сохраните и закройте *SchoolModel.edmx* файл, а затем перестройте проект.

В общем случае при изменениях базы данных, есть несколько вариантов для как синхронизировать модель:

- Для некоторых видов изменений (например, добавление или обновление таблицы, представления или хранимые процедуры), щелкните правой кнопкой мыши в конструкторе и выберите пункт **обновить модель из базы данных** чтобы конструктора внесите изменения автоматически.
- Повторное создание модели данных.
- Выполнять ручные обновления следующего вида.

В этом случае могут восстановлены модели или обновлении таблицы, затрагиваемый изменением связи, но затем пришлось бы сделать имя поля (из `FirstName` для `FirstMidName`).

## <a name="using-a-gridview-control-to-read-and-update-entities"></a>С помощью элемента управления GridView для чтения и обновления сущностей

В этом разделе вы используете `GridView` элемента управления для отображения, обновить или удалить учащихся.

Откройте или переключитесь на *Students.aspx* и переключиться в режим **разработки** представления. Из **данных** вкладке **элементов**, перетащите `GridView` управления справа от `EntityDataSource` управления, назовите его `StudentsGridView`, щелкните смарт-тег и выберите  **StudentsEntityDataSource** как источник данных.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-2/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image27.png)

Нажмите кнопку **обновить схему** (щелкните **Да** при появлении запроса на подтверждение), затем нажмите кнопку **Enable Paging**, **включить сортировку**, **Включить редактирование**, и **Включение удаления**.

Нажмите кнопку **изменить столбцы**.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-2/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image29.png)

В **выбранные поля** поле, удалите **PersonID**, **LastName**, и **HireDate**. Вы обычно не отображать ключа записи для пользователей, дата приема на работу, не имеет значения для учащихся и будет находиться обе части имени в одно поле, поэтому требуется только одно поле имени.)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-2/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image31.png)

Выберите **FirstMidName** поле и нажмите кнопку **Преобразуйте это поле в TemplateField**.

Сделайте то же самое **EnrollmentDate**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-2/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image33.png)

Нажмите кнопку **ОК** и перейдите в **источника** представления. Оставшиеся изменения будет проще выполнить непосредственно в разметке. `GridView` Управления разметки сейчас выглядит как в следующем примере.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample3.aspx)]

В первом столбце, после поле команды — это поле шаблона, который в настоящее время отображается имя. Измените разметку для этого шаблона поля, как показано в следующем примере:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample4.aspx)]

В режиме отображения двух `Label` элементы управления отображают имя и Фамилия. В режиме редактирования два текстовых поля, приведенными в можно изменить имя и Фамилия. Как и в `Label` элементов управления в режим отображения, использовать `Bind` и `Eval` выражениях точно так, как и для ASP.NET источников данных, которые подключаются непосредственно к базам данных. Единственная разница в том, что при указании свойства сущности, а не столбцы базы данных.

Последний столбец является шаблон поле, отображающее даты регистрации. Измените разметку для этого поля, как показано в следующем примере:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample5.aspx)]

В обоих отображения и изменения режима, строка формата «{0, d}» приводит к даты для отображения в формате «краткий формат даты». (Компьютер может быть настроен для отображения этот формат отличается от изображения на экране, в этом учебнике.)

Обратите внимание на то, что в каждом из этих полей шаблона, конструктор использовать `Bind` выражение по умолчанию, но вы изменили, чтобы `Eval` выражение в `ItemTemplate` элементов. `Bind` Выражение делает данные доступными в `GridView` свойства элемента управления в случае, если вам требуется доступ к данным в коде. На этой странице не нужен доступ к этим данным в коде, чтобы можно было использовать `Eval`, который является более эффективным. Дополнительные сведения см. в разделе [получения данных из данных элементов управления](https://weblogs.asp.net/davidfowler/archive/2008/12/12/getting-your-data-out-of-the-data-controls.aspx).

## <a name="revising-entitydatasource-control-markup-to-improve-performance"></a>Измените разметку элемента управления EntityDataSource для повышения производительности

В разметке для `EntityDataSource` управления, удалите `ConnectionString` и `DefaultContainerName` атрибуты и замените их значением `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` атрибута. Это изменение, следует убедиться в том случае, каждый раз при создании `EntityDataSource` управления, если не нужно использовать соединение, которое отличается от той, которая жестко задано в классе контекста объекта. С помощью `ContextTypeName` атрибут предоставляет следующие преимущества:

- Повышенная производительность. Когда `EntityDataSource` управления инициализирует модель данных с помощью `ConnectionString` и `DefaultContainerName` атрибуты, он выполняет дополнительную работу, чтобы загрузить метаданные при каждом запросе. Это не является обязательным, если указать `ContextTypeName` атрибута.
- Отложенная загрузка включена по умолчанию в классах контекста объектов (таких как `SchoolEntities` в этом руководстве) в Entity Framework 4.0. Это означает, что свойства навигации будут загружены с взаимосвязанными данными автоматически прямо в том случае, когда она нужна. Отложенная загрузка описан более подробно далее в этом руководстве.
- Все настройки, которые вы применили к класс контекста объекта (в этом случае `SchoolEntities` класс) будут доступны для элементов управления, использующих `EntityDataSource` элемента управления. Настройка класса контекста объекта довольно сложная тема, которая не рассматривается в этой серии руководств. Дополнительные сведения см. в разделе [расширение Entity Framework сгенерированным типам](https://msdn.microsoft.com/library/dd456844.aspx).

Разметка теперь будет выглядеть примерно так (порядок свойств могут отличаться):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample6.aspx)]

`EnableFlattening` Атрибут ссылается на функцию, которая была нужна в более ранних версиях Entity Framework, так как столбцы внешнего ключа не были доступны в виде свойств. Текущая версия дает возможность использовать *ассоциации внешних ключей*, означающее свойств внешнего ключа предоставляются для всех, за исключением многие ко многим ассоциаций. Если сущности нет свойств внешнего ключа и нет [сложные типы](https://msdn.microsoft.com/library/bb738472.aspx), можно оставить этот атрибут, значение `False`. Не удаляйте атрибута из разметки, так как значение по умолчанию — `True`. Дополнительные сведения см. в разделе [выравнивание объектов (EntityDataSource)](https://msdn.microsoft.com/library/ee404746.aspx).

Запустить эту страницу, и вы увидите список учащихся и сотрудников (выполнить фильтрацию для просто учащихся в следующем учебном курсе). Имя и фамилия будут отображаться.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-2/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image35.png)

Для сортировки списка, щелкните имя столбца.

Нажмите кнопку **изменить** в любой строке. Текстовые поля отображаются, где можно изменить имя и Фамилия.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-2/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image37.png)

**Удалить** работает кнопка также. Для строки, Дата регистрации и строка не отображается, нажмите кнопку "Удалить". (Строки без даты регистрации представляют преподавателей и может появиться ошибка ссылочной целостности. В следующем учебном курсе вы будете фильтровать этот список, чтобы включить учащихся, точно так же.)

## <a name="displaying-data-from-a-navigation-property"></a>Отображение данных из свойства навигации

Теперь предположим, что нужно знать, сколько курсы в регистрации каждого учащегося. Платформа Entity Framework предоставляет эту информацию в `StudentGrades` свойство навигации `Person` сущности. Поскольку структуры базы данных не допускает учащихся к регистрации в курс без необходимости оценку назначена, в этом руководстве можно предположить, что наличие строки в `StudentGrade` строки таблицы, которая связана с курсом совпадает со значением зарегистрированных в этом курсе. ( `Courses` Свойство навигации является свойством только для преподавателей.)

При использовании `ContextTypeName` атрибут `EntityDataSource` элемента управления, Entity Framework автоматически извлекает данные для свойства навигации при доступе к этому свойству. Это называется *отложенная загрузка*. Тем не менее это может быть неэффективным, за счет отдельный вызов к базе данных, необходима дополнительная информация каждый раз. Если необходимо получить данные из свойства навигации для каждой сущности, возвращенные `EntityDataSource` элемента управления, это более эффективно извлекать данные вместе с самой сущности за один вызов к базе данных. Это называется *Безотложная загрузка*, и укажите безотложную загрузку для свойства навигации, задав `Include` свойство `EntityDataSource` элемента управления.

В *Students.aspx*, вы хотите отобразить число курсов для каждого учащегося, поэтому Безотложная загрузка является лучшим выбором. Если вы отображение всех учащихся, но отображаются количество курсов только для некоторые из них (что потребовало бы написания кода вместе с разметкой), отложенная загрузка может быть лучшим выбором.

Откройте или переключитесь на *Students.aspx*, переключитесь в **разработки** представление, выберите `StudentsEntityDataSource`и в **свойства** задайте **Include**свойства **StudentGrades**. (Если вы хотите получить несколько свойств навигации, можно указать их имена, разделенные запятыми, например, **StudentGrades, курсы**.)

[![Image19](the-entity-framework-and-aspnet-getting-started-part-2/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image39.png)

Переключиться в режим **источника** представления. В `StudentsGridView` управления после последнего `asp:TemplateField` элемента, добавьте новое поле шаблона:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample7.aspx)]

В `Eval` выражения могут ссылаться на свойство навигации `StudentGrades`. Так как это свойство содержит коллекцию, он имеет `Count` свойство, которое можно использовать для отображения число курсов, в которых зарегистрирован студент. В этом руководстве вы увидите, как отобразить данные из свойства навигации, которые содержат отдельные объекты, а не коллекции. (Обратите внимание, что нельзя использовать `BoundField` элементов для отображения данных из свойства навигации.)

Откройте страницу, и теперь вы видите, сколько курсы регистрации учащихся в.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image41.png)

## <a name="using-a-detailsview-control-to-insert-entities"></a>С помощью элемента управления DetailsView для вставки сущности

Следующим шагом является создание страницы, `DetailsView` элемент управления, который позволит добавить новые учащиеся. Закройте браузер, а затем создайте новый веб-страницы с помощью *Site.Master* главной страницы. Присвойте странице имя *StudentsAdd.aspx*, а затем переключиться в **источника** представления.

Добавьте следующую разметку, чтобы заменить существующую разметку для `Content` управления с именем `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample8.aspx)]

Эта разметка создает `EntityDataSource` элемента управления, которая похожа на созданном в *Students.aspx*, за исключением того, он позволяет вставки. Как и в `GridView` управления, привязанных полей `DetailsView` управления кодируются, именно так, как они будут для элемента управления данными, который подключается непосредственно к базе данных, за исключением того, что они ссылаются на свойства сущности. В этом случае `DetailsView` элемент управления используется только для вставки строк, поэтому выбран режим по умолчанию `Insert`.

Откройте страницу и добавить нового учащегося.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image43.png)

Ничего не происходит после вставки нового учащегося, но в том случае, если выполнить *Students.aspx*, вы увидите данные нового учащегося.

## <a name="displaying-data-in-a-drop-down-list"></a>Отображение данных в раскрывающемся списке

В следующих шагах будет databind `DropDownList` управления к сущности, заданные с помощью `EntityDataSource` элемента управления. В этой части руководства не большая с помощью этого списка. В последующих частях, вы используете списка для пользователям предоставляется возможность выбрать подразделение для отображения курсов, связанные с подразделением.

Создание нового веб-страницы с именем *Courses.aspx*. В **источника** просматривать, добавлять заголовок, чтобы `Content` элемент управления, который называется `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample9.aspx)]

В **разработки** просматривать, добавлять `EntityDataSource` на страницу элемент управления как раньше, но в этом случае имя `DepartmentsEntityDataSource`. Выберите **отделов** как **EntitySetName** значение и выберите только **DepartmentID** и **имя** свойства.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-2/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image45.png)

Из **стандартный** вкладке **элементов**, перетащите `DropDownList` на страницу элемент управления, назовите его `DepartmentsDropDownList`, щелкните смарт-тег и выберите **Выбор источника данных** для Запуск **мастер настройки источника данных**.

[![Image16](the-entity-framework-and-aspnet-getting-started-part-2/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image47.png)

В **Выбор источника данных** выберите **DepartmentsEntityDataSource** как источник данных, нажмите кнопку **обновить схему**, а затем выберите **имя** как поле данных для отображения и **DepartmentID** как поле значений данных. Нажмите кнопку **ОК**.

[![Image17](the-entity-framework-and-aspnet-getting-started-part-2/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image49.png)

Метод, используемый databind элемента управления, используя Entity Framework совпадает с другими данными ASP.NET элементов управления источниками, за исключением того, вы при указании сущности и свойства сущности.

Переключиться в режим **источника** просмотра и добавления «выбрать отдел:» непосредственно перед `DropDownList` элемента управления.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample10.aspx)]

Напоминаем, измените разметку для `EntityDataSource` управления на этом этапе, заменив `ConnectionString` и `DefaultContainerName` атрибуты с помощью `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` атрибута. Часто лучше дождаться, когда вы создали элемент управления с привязкой к данным, связанного с элементом управления источником данных перед изменением `EntityDataSource` -разметке элементов управления, так как после внесения изменений, конструктор не предоставляет с **обновления Схемы** параметр в элементе управления с привязкой к данным.

Откройте страницу, и в раскрывающемся списке можно выбрать отдел.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image51.png)

На этом завершается введение в использование `EntityDataSource` элемента управления. Работа с этот элемент управления обычно не отличается от работы с другими данными ASP.NET элементов управления источниками, за исключением того, что вы ссылаетесь, сущности и свойства, а не таблиц и столбцов. Единственное исключение — если вы хотите получить доступ к свойствам навигации. В следующем учебном курсе вы увидите, что синтаксис, используемом с `EntityDataSource` управления также может отличаться от других элементов управления источниками данных, если фильтрация, группирование и упорядочивания данных.

> [!div class="step-by-step"]
> [Назад](the-entity-framework-and-aspnet-getting-started-part-1.md)
> [Вперед](the-entity-framework-and-aspnet-getting-started-part-3.md)
