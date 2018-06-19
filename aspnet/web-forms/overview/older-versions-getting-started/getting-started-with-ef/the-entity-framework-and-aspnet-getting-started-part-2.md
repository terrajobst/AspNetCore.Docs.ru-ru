---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
title: Приступая к работе с базой данных Entity Framework 4.0 сначала и ASP.NET 4 Web Forms — часть 2 | Документы Microsoft
author: tdykstra
description: Contoso университета примера веб-приложения показано, как для создания приложений веб-форм ASP.NET, использующий Entity Framework. Это образец приложения...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: fb63a326-a4ae-4b0c-a4f5-412327197216
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
msc.type: authoredcontent
ms.openlocfilehash: a6c95a92aa77e2bb73aa513a207e0469d1aedbd2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890558"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-2"></a>Приступая к работе с базой данных Entity Framework 4.0 сначала и ASP.NET 4 Web Forms — часть 2
====================
по [Tom Dykstra](https://github.com/tdykstra)

> Contoso университета примера веб-приложения показано, как для создания приложений веб-форм ASP.NET, с помощью Entity Framework 4.0 и Visual Studio 2010. Сведения о учебника серии см [в первом учебнике ряда](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="the-entitydatasource-control"></a>Элемент управления EntityDataSource

В предыдущем учебнике вы создали веб-сайт, базы данных и модели данных. В этом учебнике работают с `EntityDataSource` элемента управления, ASP.NET предоставляет для упрощения работы с модели данных Entity Framework. Вы создадите `GridView` управления для отображения и редактирования данных студента `DetailsView` управления для добавления новых студентов и `DropDownList` управления для выбора подразделения (которая будет использоваться позже для отображения связанных курсов).

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image1.png)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image3.png)

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image5.png)

Обратите внимание, что в этом приложении вы не будет добавляться проверки входных данных на страницы, которые используются для обновления базы данных и обработка ошибок не будет настолько надежно, как будут необходимы в реальном приложении. Содержит учебник, основное внимание уделено Entity Framework и предотвращает получение слишком много времени. Дополнительные сведения о том, как добавить эти функции в приложение см. в разделе [проверка введенных данных в веб-страниц ASP.NET](https://msdn.microsoft.com/library/7kh55542.aspx) и [обработка ошибок на страницах ASP.NET и приложений](https://msdn.microsoft.com/library/w16865z6.aspx).

## <a name="adding-and-configuring-the-entitydatasource-control"></a>Добавление и настройка элемента управления EntityDataSource

Сначала вы создадите Настройка `EntityDataSource` управления для чтения `Person` сущностей из `People` набора сущностей.

Убедитесь, что у вас есть откройте Visual Studio и при работе с проектом, созданные в части 1. Если проект еще не создана, с момента создания модели данных или с момента последнего изменения, внесенные в него, сборку проекта прямо сейчас. Изменения в модель данных, недоступны в конструктор до построения проекта.

Создайте новый веб-страницы с помощью **веб-форма, использующая главную страницу** шаблона и назовите его *Students.aspx*.

[![Image23](the-entity-framework-and-aspnet-getting-started-part-2/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image7.png)

Укажите *Site.Master* как Главная страница. Все страницы, созданные для этих учебников будет использовать эту главную страницу.

[![Image24](the-entity-framework-and-aspnet-getting-started-part-2/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image9.png)

В **источника** просматривать `h2` заголовок для `Content` управления с именем `Content2`, как показано в следующем примере:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample1.aspx)]

Из **данные** вкладке **элементов**, перетащите `EntityDataSource` на страницу элемент управления, поместите его под заголовком и измените идентификатор на `StudentsEntityDataSource`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample2.aspx)]

Переключитесь в **разработки** просмотра, щелкните смарт-тег элемента управления источником данных и нажмите кнопку **Настройка источника данных** для запуска **Настройка источника данных** мастера.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-2/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image11.png)

В **Настройка ObjectContext** шаг мастера, выберите **SchoolEntities** как значение для **подключения с именем**и выберите **SchoolEntities**как **DefaultContainerName** значение. Затем нажмите кнопку **Далее**.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-2/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image13.png)

Примечание: Если на этом этапе вы получаете следующее диалоговое окно, необходимо построить проект, прежде чем продолжить.

[![Image25](the-entity-framework-and-aspnet-getting-started-part-2/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image15.png)

В **Настройка выбора данных** шаг, выберите **людей** как значение для **EntitySetName**. В разделе **выберите**, убедитесь, что **выберите A** ll установлен флажок. Выберите параметры для включения обновления и удаления. Когда закончите, нажмите кнопку **Готово**.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-2/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image17.png)

## <a name="configuring-database-rules-to-allow-deletion"></a>Настройка правил базы данных для разрешения удаления

Вы создадите страницу, которая позволяет пользователям удалять студентов `Person` таблицы, которая имеет три связи с другими таблицами (`Course`, `StudentGrade`, и `OfficeAssignment`). По умолчанию базы данных не позволяет удалять строки в `Person` при наличии связанными строками в одной из других таблиц. Можно вручную удалить связанные строки сначала или можно настроить базу данных для их удаления автоматически при удалении `Person` строки. Для записи студентов в этом учебнике следует настроить базы данных автоматически удалять связанные данные. Поскольку учащихся могут связанные с ней строки только в `StudentGrade` таблицы, необходимо настроить только один из трех связи.

Если вы используете *файл School.mdf* файл, загруженный из проекта, который переходит к этому учебнику, этот раздел можно пропустить, так как эти изменения конфигурации, уже выполнены. При создании базы данных с помощью скрипта, необходимо Настройте базу данных, выполнив следующие процедуры.

В **обозревателя серверов**, открытие диаграммы базы данных, созданной в части 1. Щелкните правой кнопкой мыши связь между `Person` и `StudentGrade` (линии между таблицами), а затем выберите **свойства**.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-2/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image19.png)

В **свойства** окна, разверните **спецификация INSERT и UPDATE** и задайте **DeleteRule** свойства **Cascade**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-2/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image21.png)

Сохраните и закройте диаграмму. Ответ на вопрос, хотите ли вы обновить базу данных, нажмите кнопку **Да**.

Чтобы убедиться в том, что модель отслеживает сущности, находящиеся в памяти, в соответствии с действиями базы данных, необходимо задать соответствующие правила в модели данных. Откройте *SchoolModel.edmx*, щелкните правой кнопкой мыши линию связи между `Person` и `StudentGrade`, а затем выберите **свойства**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-2/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image23.png)

В **свойства** задайте **End1 OnDelete** для **Cascade**.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-2/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image25.png)

Сохраните и закройте *SchoolModel.edmx* файла, а затем перестройте проект.

В общем случае при изменении базы данных, у вас есть несколько способов синхронизации модель:

- Для некоторых видов изменений (например, добавление или обновление таблицы, представления или хранимые процедуры), щелкните правой кнопкой мыши в конструкторе и выберите пункт **обновление модели из базы данных** чтобы конструктора сделать изменения автоматически.
- Повторное создание модели данных.
- Выполните вручную обновления, похожее на следующее.

В этом случае могут восстановлены модели или обновлении таблицы, затронутых изменение отношения, но затем пришлось бы сделать имя поля (из `FirstName` для `FirstMidName`).

## <a name="using-a-gridview-control-to-read-and-update-entities"></a>С помощью элемента управления GridView для чтения и обновления сущностей

В этом разделе мы используем `GridView` элемента управления для отображения, изменения или удаления учащихся.

Откройте или переключитесь на *Students.aspx* и переключитесь в **разработки** представления. Из **данные** вкладке **элементов**, перетащите `GridView` управления справа от `EntityDataSource` управления, назовите его `StudentsGridView`, щелкните смарт-тег, а затем выберите  **StudentsEntityDataSource** как источник данных.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-2/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image27.png)

Нажмите кнопку **обновить схему** (щелкните **Да** при появлении запроса на подтверждение), затем нажмите кнопку **включить разбиение на страницы**, **включить сортировку**, **Включить редактирование**, и **включить удаление**.

Нажмите кнопку **Правка столбцов**.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-2/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image29.png)

В **выбранные поля** удалите **PersonID**, **LastName**, и **HireDate**. Вы обычно отображают ключа записи для пользователей, не применимо к учащихся Дата приема на работу и будет размещаться обе части имени в одно поле, поэтому требуется только одно поле имя.)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-2/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image31.png)

Выберите **FirstMidName** и нажмите кнопку **преобразовать это поле в TemplateField**.

Сделайте то же самое **EnrollmentDate**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-2/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image33.png)

Нажмите кнопку **ОК** и переходе к **источника** представления. Оставшиеся изменения будет проще выполнить непосредственно в разметке. `GridView` Управления разметки сейчас выглядит как в следующем примере.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample3.aspx)]

Первый столбец после поле команды — это поле шаблона, который в текущий момент отображает имя. Измените разметку для этого шаблона поля, как в следующем примере:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample4.aspx)]

В режиме отображения двух `Label` отображают имя и фамилию. В режиме редактирования два текстовых поля предоставляются для изменения имени и фамилии. Как и в `Label` элементов управления в режим отображения, использовать `Bind` и `Eval` выражениях точно так, как и для управления источника данных ASP.NET, подключаться непосредственно к базам данных. Отличается только что задании свойства сущности, а не столбцы базы данных.

Последний столбец является шаблон поле, отображающее даты регистрации. Измените разметку для этого поля должен выглядеть как в следующем примере:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample5.aspx)]

В обоих отображения и изменения режима, строка формата «{0, d}» вызывает даты для отображения в формате «Краткая дата». (Компьютер могут настраиваться для отображения этот формат отличается от изображения на экране, показанными в данном руководстве.)

Обратите внимание, что в каждом из этих полей шаблона, конструктор использовать `Bind` выражение по умолчанию, но вы изменили его `Eval` выражения в `ItemTemplate` элементов. `Bind` Выражение делает данные доступными в `GridView` свойств элемента управления, при необходимости доступа к данным в коде. На этой странице не требуется доступ к этим данным в коде, чтобы можно было использовать `Eval`, является более эффективным. Дополнительные сведения см. в разделе [получения данных из элементов управления данными](https://weblogs.asp.net/davidfowler/archive/2008/12/12/getting-your-data-out-of-the-data-controls.aspx).

## <a name="revising-entitydatasource-control-markup-to-improve-performance"></a>Измените разметку элемента управления EntityDataSource для повышения производительности

В разметке для `EntityDataSource` управления, удалите `ConnectionString` и `DefaultContainerName` атрибуты и заменяет их значением `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` атрибута. Это изменение необходимо каждый раз при создании `EntityDataSource` управления, если не требуется использовать подключение, которое отличается от той, которая жестко запрограммированы в класс контекста объекта. С помощью `ContextTypeName` атрибут предоставляет следующие преимущества:

- Повышенная производительность. Когда `EntityDataSource` элемент управления инициализирует модель данных с помощью `ConnectionString` и `DefaultContainerName` атрибуты, он выполняет дополнительную работу, чтобы загрузить метаданные при каждом запросе. Это не является обязательным, если указать `ContextTypeName` атрибута.
- Отложенная загрузка включена по умолчанию в классах созданный объект контекста (такие как `SchoolEntities` в этом учебнике) в Entity Framework 4.0. Это означает, что свойства навигации, загружаются со связанными данными автоматически прямо в том случае, когда он необходим. Отложенная загрузка является более подробно далее в этом учебнике.
- Все настройки, примененные класс контекста объекта (в этом случае `SchoolEntities` класс) будет доступен для элементов управления, использующих `EntityDataSource` элемента управления. Настройка класс контекста объекта является довольно сложная тема, которой не соответствует этой серии учебника. Дополнительные сведения см. в разделе [расширение Entity Framework сгенерированным типам](https://msdn.microsoft.com/library/dd456844.aspx).

Разметка теперь будет выглядеть примерно так (порядок свойств может отличаться):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample6.aspx)]

`EnableFlattening` Атрибут ссылается на функцию, которая была требуется в более ранних версиях платформы Entity Framework, поскольку столбцы внешнего ключа не были доступны как свойства сущности. Текущая версия дает возможность использовать *внешнего ключа ассоциации*, означающее свойства внешнего ключа, предоставляются для всех, за исключением многие ко многим ассоциаций. Если сущностей имеет свойства внешнего ключа и нет [сложные типы](https://msdn.microsoft.com/library/bb738472.aspx), можно оставить этот атрибут в качестве `False`. Не удаляйте атрибут из разметки, так как значение по умолчанию — `True`. Дополнительные сведения см. в разделе [выравнивание объектов (EntityDataSource)](https://msdn.microsoft.com/library/ee404746.aspx).

Запустите страницу, и вы увидите список студентов и сотрудники (выполнить фильтрацию для учащихся точно так же, в следующем уроке). Имя и фамилия будут отображаться оба.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-2/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image35.png)

Для сортировки списка, щелкните имя столбца.

Нажмите кнопку **изменить** в любой строке. Текстовые поля отображаются, где можно изменить имя и фамилию.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-2/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image37.png)

**Удалить** работает также кнопка. Нажмите кнопку Удалить строки, Дата регистрации и исчезает строки. (Строки без даты регистрации представляют инструкторов и может получить ошибку ссылочной целостности. В следующем учебнике вам предстоит фильтровать этот список, чтобы включать только учащихся.)

## <a name="displaying-data-from-a-navigation-property"></a>Отображение данных из свойства навигации

Теперь предположим, что нужно знать, сколько курсы студентах зарегистрировано в. Платформа Entity Framework предоставляет эти данные в `StudentGrades` свойство навигации `Person` сущности. Так как структура базы данных не допускает студент регистрации курса без необходимости назначенный оценку, в этом учебнике предполагается, имеет строку в `StudentGrade` строки таблицы, которая связана с курсом совпадает со значением регистрируется в курсе. ( `Courses` Свойство навигации предназначен только для инструкторов.)

При использовании `ContextTypeName` атрибут `EntityDataSource` элемента управления, Entity Framework автоматически извлекает данные для свойства навигации при доступе к этому свойству. Это называется *отложенную загрузку*. Тем не менее это может быть неэффективным, поскольку приводит к отдельный вызов базы данных, которую требуется каждого Дополнительные сведения о времени. Если необходимо получить данные из свойства навигации для каждой сущности, возвращенных `EntityDataSource` элемента управления, это более эффективно извлекать данные вместе с самой сущности в рамках одного вызова к базе данных. Это называется *упреждающую*, и укажите упреждающую для свойства навигации, задав `Include` свойства `EntityDataSource` элемента управления.

В *Students.aspx*, требуется показывает количество курсы за каждый учащийся, поэтому упреждающую — лучший выбор. Если были отображение всех студентов, но число курсы только для некоторых из них (что потребует написания кода в дополнение к разметке), отложенная загрузка может быть лучшим выбором.

Откройте или переключитесь на *Students.aspx*, переключитесь в **разработки** представление, выберите `StudentsEntityDataSource`и в **свойства** задайте **Include**свойства **StudentGrades**. (Если требуется получить несколько свойств навигации, можно указать их имена, разделенные запятыми, например, **StudentGrades, курсы**.)

[![Image19](the-entity-framework-and-aspnet-getting-started-part-2/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image39.png)

Переключитесь в **источника** представления. В `StudentsGridView` управления после последнего `asp:TemplateField` элемента, добавьте в новый шаблон поле:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample7.aspx)]

В `Eval` выражения, можно ссылаться на свойство навигации `StudentGrades`. Поскольку это свойство содержит коллекцию, у нее есть `Count` свойство, которое можно использовать для отображения числа курсов, в которых зарегистрирован студент. В этом руководстве вы увидите, как отображать данные из свойства навигации, которые содержат отдельные элементы, а не коллекции. (Обратите внимание, что нельзя использовать `BoundField` элементы для отображения данных из свойства навигации.)

Запустите страницу и появиться сколько курсы студента регистрации в.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image41.png)

## <a name="using-a-detailsview-control-to-insert-entities"></a>С помощью элемента управления DetailsView для вставки сущностей

Следующим шагом является создание страницы, `DetailsView` элемент управления, который позволит вам добавить новых студентов. Закройте браузер, а затем создать новый веб-страницы с помощью *Site.Master* главной страницы. Назовите страницу *StudentsAdd.aspx*, а затем переключиться в **источника** представления.

Добавьте следующую разметку, чтобы заменить существующую разметку для `Content` управления с именем `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample8.aspx)]

Эта разметка создает `EntityDataSource` управления, похожий на тот, который был создан в *Students.aspx*, за исключением того, она позволяет вставки. Как и в `GridView` управления, привязанных полей `DetailsView` управления кодируются в точности так, как они будут иметь для элемента управления данными, который подключается непосредственно к базе данных, за исключением того, что они ссылаются на свойства сущности. В этом случае `DetailsView` управления используется только для вставки строк, поэтому выбирается режим по умолчанию `Insert`.

Запустите страницу и Добавление нового студента.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image43.png)

Ничего не произойдет после вставки нового студента, но если выполнить *Students.aspx*, вы увидите сведения о новом студента.

## <a name="displaying-data-in-a-drop-down-list"></a>Отображение данных в раскрывающемся списке

В следующих шагах вы будете databind `DropDownList` управления к сущности, заданные с помощью `EntityDataSource` элемента управления. В этой части руководства не будет не очень много операций с этим списком. В последующих частей, мы используем список дать пользователям возможность выбирать подразделения для отображения курсов, связанные с подразделением.

Создать новый веб-страницу с именем *Courses.aspx*. В **источника** просмотреть, добавить заголовок, чтобы `Content` управления с именем `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample9.aspx)]

В **разработки** просматривать `EntityDataSource` на страницу элемент управления как вы делали раньше, но в этот раз присвойте ей имя `DepartmentsEntityDataSource`. Выберите **отделы** как **EntitySetName** значение и выберите только **DepartmentID** и **имя** свойства.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-2/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image45.png)

Из **Стандартная** вкладке **элементов**, перетащите `DropDownList` на страницу элемент управления, назовите его `DepartmentsDropDownList`, щелкните смарт-тег и выберите **Выбор источника данных** для Запуск **мастер конфигурации источника данных**.

[![Image16](the-entity-framework-and-aspnet-getting-started-part-2/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image47.png)

В **выберите источник данных** шаг, выберите **DepartmentsEntityDataSource** как источник данных, нажмите кнопку **обновить схему**, а затем выберите **имя** как поле данных для отображения и **DepartmentID** как поле значений данных. Нажмите кнопку **ОК**.

[![Image17](the-entity-framework-and-aspnet-getting-started-part-2/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image49.png)

Метод, используемый для привязки элемента управления, использующий Entity Framework совпадает с другими данными ASP.NET указано управления версиями только сущности и свойства сущности.

Переключитесь в **источника** просмотра и добавления «выбрать отдел:» непосредственно перед `DropDownList` элемента управления.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample10.aspx)]

Напоминаем, измените разметку для `EntityDataSource` управления на этом этапе, заменив `ConnectionString` и `DefaultContainerName` атрибуты с помощью `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` атрибута. Часто лучше Подождите, пока после создания элемента управления с привязкой к данным, связанного с элементом управления источником данных перед изменением `EntityDataSource` управления разметки, так как после внесения изменений, конструктор не даст вам **обновления Схемы** параметр в элементе управления с привязкой к данным.

Запустите страницу и отдел можно выбрать из раскрывающегося списка.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image51.png)

На этом завершается сведения об использовании `EntityDataSource` элемента управления. Работа с этим элементом управления — обычно не отличается от работы с другими данными ASP.NET управления версиями, за исключением того, можно ссылаться на сущности и свойства, а не таблиц и столбцов. Единственное исключение — если вы хотите получить доступ к свойствам навигации. В следующем уроке вы увидите, что синтаксис можно использовать с `EntityDataSource` управления также могут отличаться от других элементов управления источниками данных, при фильтрации, группировки и данные о заказах.

> [!div class="step-by-step"]
> [Назад](the-entity-framework-and-aspnet-getting-started-part-1.md)
> [Вперед](the-entity-framework-and-aspnet-getting-started-part-3.md)
