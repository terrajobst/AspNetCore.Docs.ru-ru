---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
title: "Новые возможности платформы Entity Framework 4.0 | Документы Microsoft"
author: tdykstra
description: "Этот учебник ряд строится на веб-приложение Contoso университета, созданный Приступая к работе с рядами учебника Entity Framework 4.0. Я..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 393df4a8-b1db-44c4-9db7-2b533ca887d0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
msc.type: authoredcontent
ms.openlocfilehash: 4c89ca004ad4c9d731868e868cf6723aa4ed625d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-the-entity-framework-40"></a>Новые возможности платформы Entity Framework 4.0
====================
По [Tom Dykstra](https://github.com/tdykstra)

> Этот учебник ряд основан на веб-приложение Contoso университета, созданный [Приступая к работе с платформой Entity Framework](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) учебника рядов. Если не была завершена ранее учебники, в качестве отправной точки для этого учебника вы можете [загрузить приложение](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , будет создана. Вы также можете [загрузить приложение](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , создаваемый для завершения учебника ряда. Если у вас есть вопросы о учебники, их можно разместить [форум по ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


В предыдущем учебнике вы увидели некоторые методы максимальную производительность веб-приложения, использующего платформу Entity Framework. Этот учебник рассматриваются некоторые из наиболее важных новых функций в версии 4 платформы Entity Framework и ссылки на ресурсы, которые содержатся более полные сведения о всех новых функций. Ниже приведены компоненты, выделяются в этом учебнике:

- Связи внешнего ключа.
- Выполнение пользовательской команды SQL.
- Разработка модели.
- Поддержка POCO.

Кроме того, учебник кратко вынуждает *Разработка кода сначала*, функция, которая поступает в следующей версии платформы Entity Framework.

Чтобы начать работу с учебником, запустите Visual Studio и откройте веб-приложение Contoso университета, велась работа в предыдущем учебнике.

## <a name="foreign-key-associations"></a>Связи внешнего ключа

Версия 3.5 платформы Entity Framework включает свойства навигации, но не включает свойства внешнего ключа в модели данных. Например `CourseID` и `StudentID` столбцы `StudentGrade` таблицы будет опущена `StudentGrade` сущности.

[!["Image01"](what-s-new-in-the-entity-framework-4/_static/image2.png)](what-s-new-in-the-entity-framework-4/_static/image1.png)

Причина этого подхода: что, строго говоря, внешние ключи являются физической реализации и не входящие в концептуальной модели. Однако на практике часто бывает удобнее работать с сущностями в коде при наличии прямой доступ к ним.

Для примера как внешних ключей в модели данных можно упростить код, рассмотрим, как вам пришлось бы код *DepartmentsAdd.aspx* страницы без них. В `Department` сущности, `Administrator` свойство является внешним ключом, который соответствует `PersonID` в `Person` сущности. Чтобы установить связь между новое подразделение и имени его администратора, все было нужно было задано значение для `Administrator` свойства `ItemInserting` обработчик события элемента управления с привязкой к данным:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample1.cs)]

Без внешних ключей в модель данных будет обрабатываться `Inserting` события элемента управления источника данных, а не `ItemInserting` события элемента управления с привязкой к данным, чтобы получить ссылку на самой сущности перед добавлением сущности набора сущностей. При наличии эту ссылку, вы устанавливаете связь как с помощью кода в следующих примерах:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample2.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample3.cs)]

Как видно в команде Entity Framework [на связи внешнего ключа в блоге](https://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx), других случаях, где гораздо больше разница в сложности кода. В соответствии с потребностями те из них, которые предпочитают live с сведения о реализации в концептуальной модели для обеспечения более простой код, Entity Framework теперь предоставляет возможность включения внешних ключей в модель данных.

В терминологии Entity Framework, при включении внешних ключей в модель данных вы используете *внешнего ключа ассоциации*, и если вы исключите внешних ключей, вы используете *независимые сопоставления*.

## <a name="executing-user-defined-sql-commands"></a>Выполнение команд SQL, определяемые пользователем

В более ранних версиях платформы Entity Framework возникла не простой способ создания собственных команд SQL на лету и их выполнения. Entity Framework динамически созданных команд SQL для вас, либо необходимо создать хранимую процедуру и импортировать его в качестве функции. Добавляет версии 4 `ExecuteStoreQuery` и `ExecuteStoreCommand` методы `ObjectContext` класса, чтобы упростить для передачи любого запроса непосредственно в базе данных.

Предположим, что администраторы университета Contoso требуется возможность выполнения массовых изменений в базе данных без необходимости прохождения через процесс создания хранимой процедуры и импортировав их в модель данных. Их первый запрос — для страницы, которая позволяет изменить число кредиты для всех курсов в базе данных. На веб-странице, они хотят иметь возможность вводить номер используется для умножения значения каждого `Course` строки `Credits` столбца.

Создание новой страницы, использующей *Site.Master* главную страницу и назовите его *UpdateCredits.aspx*. Затем добавьте следующую разметку, чтобы `Content` управления с именем `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample4.aspx)]

Эта разметка создает `TextBox` управления, в котором можно ввести значение множителя, `Button` управления нажмите кнопку, чтобы выполнить команду и `Label` управления, указывающие количество затронутых строк.

Откройте *UpdateCredits.aspx.cs*и добавьте следующий `using` инструкции и обработчик для кнопки `Click` событий:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample5.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample6.cs)]

Этот код выполняет код SQL `Update` команды, используя значение в текстовом поле и отображает количество затронутых строк с помощью метки. Прежде чем запустить страницу, запустите *Courses.aspx* страницы, чтобы получить изображение «до» некоторые данные.

[![Image02](what-s-new-in-the-entity-framework-4/_static/image4.png)](what-s-new-in-the-entity-framework-4/_static/image3.png)

Запустите *UpdateCredits.aspx*, введите «10» в качестве множитель и нажмите кнопку **Execute**.

[![Image03](what-s-new-in-the-entity-framework-4/_static/image6.png)](what-s-new-in-the-entity-framework-4/_static/image5.png)

Запустите *Courses.aspx* страницу, чтобы проверить измененные данные.

[![Image04](what-s-new-in-the-entity-framework-4/_static/image8.png)](what-s-new-in-the-entity-framework-4/_static/image7.png)

(Если вы хотите задать число кредиты обратно к исходным значениям в *UpdateCredits.aspx.cs* изменить `Credits * {0}` для `Credits / {0}` и повторно запустите страницу, введя 10 в качестве делителя.)

Дополнительные сведения о выполнении запросов, которые указываются в коде см. в разделе [как: непосредственно выполнение команды для источника данных](https://msdn.microsoft.com/en-us/library/ee358769.aspx).

## <a name="model-first-development"></a>Разработка модели

В этих пошаговых руководствах сначала создать базу данных и затем создается модель данных, основанная на структуре базы данных. В Entity Framework 4 можно начать с моделью данных, вместо и создать базу данных, основанная на структуре модели данных. Если вы создаете приложение, для которого еще не существует базы данных, подход «сначала модель» позволяет создавать сущности и связи, которые имеют смысл концептуально для приложения, не заботясь о деталях физической реализации . (Это справедливо только через начальной стадии разработки, однако. Со временем базы данных будет создана и будет содержать производственных данных и создать его заново из модели будет практические; на этом этапе вы будете обратно в базы данных первый подход.)

В этом разделе учебника будет создать простую модель данных и создать базу данных из него.

В **обозревателе решений**, щелкните правой кнопкой мыши *DAL* папку и выберите **Добавление нового элемента**. В **Добавление нового элемента** диалогового **установленные шаблоны** выберите **данные** , а затем выберите **ADO.NET Entity Data Model** шаблона . Присвойте новому файлу *AlumniAssociationModel.edmx* и нажмите кнопку **добавить**.

[![Image06](what-s-new-in-the-entity-framework-4/_static/image10.png)](what-s-new-in-the-entity-framework-4/_static/image9.png)

Будет запущен мастер моделей EDM. В **Выбор содержимого модели** шаг, выберите **пустую модель** и нажмите кнопку **Готово**.

[![Image07](what-s-new-in-the-entity-framework-4/_static/image12.png)](what-s-new-in-the-entity-framework-4/_static/image11.png)

**Конструктора моделей EDM** открывается с помощью пустого конструктора. Перетащите **сущности** элемента из **элементов** на поверхность разработки.

[![Image08](what-s-new-in-the-entity-framework-4/_static/image14.png)](what-s-new-in-the-entity-framework-4/_static/image13.png)

Изменить имя сущности с `Entity1` для `Alumnus`, изменить `Id` имя свойства для `AlumnusId`и добавить новое скалярное свойство с именем `Name`. Для добавления новых свойств можно нажать клавишу ВВОД после изменения имени `Id` столбец, или щелкните правой кнопкой мыши сущность и выберите **добавить скалярное свойство**. Значение по умолчанию для новых свойств — `String`, который подходит для этого простого примера, но Конечно, вы можете изменить таких вещей, как тип данных в **свойства** окна.

Создайте другой сущности так же, как и назовите его `Donation`. Изменение `Id` свойства `DonationId` и добавьте скалярное свойство с именем `DateAndAmount`.

[![Image09](what-s-new-in-the-entity-framework-4/_static/image16.png)](what-s-new-in-the-entity-framework-4/_static/image15.png)

Чтобы добавить ассоциацию между этими двумя сущностями, щелкните правой кнопкой мыши `Alumnus` сущности, выберите **добавить**и выберите **ассоциации**.

[![Image10](what-s-new-in-the-entity-framework-4/_static/image18.png)](what-s-new-in-the-entity-framework-4/_static/image17.png)

Значения по умолчанию в **добавить сопоставление** диалоговое окно, выберите (один ко многим, включают свойства навигации, содержать внешние ключи), поэтому просто нажмите кнопку **ОК**.

[![Image11](what-s-new-in-the-entity-framework-4/_static/image20.png)](what-s-new-in-the-entity-framework-4/_static/image19.png)

Конструктор добавляет линия связи и свойства внешнего ключа.

[![Image12](what-s-new-in-the-entity-framework-4/_static/image22.png)](what-s-new-in-the-entity-framework-4/_static/image21.png)

Теперь все готово для создания базы данных. Щелкните правой кнопкой мыши область конструктора и выберите **формирования базы данных из модели**.

[![Image13](what-s-new-in-the-entity-framework-4/_static/image24.png)](what-s-new-in-the-entity-framework-4/_static/image23.png)

Будет запущен мастер формирования базы данных. (При появлении предупреждения, указывающие, что сущности не сопоставлены, можно пропустить те, на данный момент.)

В **Выбор подключения к данным** шаг, нажмите кнопку **новое соединение**.

[![Image14](what-s-new-in-the-entity-framework-4/_static/image26.png)](what-s-new-in-the-entity-framework-4/_static/image25.png)

В **свойства соединения** диалогового окна выберите локальный экземпляр SQL Server Express и базе данных имя `AlumniAsssociation`.

[![Image15](what-s-new-in-the-entity-framework-4/_static/image28.png)](what-s-new-in-the-entity-framework-4/_static/image27.png)

Нажмите кнопку **Да** при появляется запрос, следует ли создать базу данных. Когда **Выбор подключения к данным** шаг отображается еще раз, нажмите кнопку **Далее**.

В **сводные данные и параметры** шаг, нажмите кнопку **Готово**.

[![Image18](what-s-new-in-the-entity-framework-4/_static/image30.png)](what-s-new-in-the-entity-framework-4/_static/image29.png)

Объект *.sql* создается файл с командами данных definition language (DDL), но еще не запущено команды.

[![Image20](what-s-new-in-the-entity-framework-4/_static/image32.png)](what-s-new-in-the-entity-framework-4/_static/image31.png)

Используйте это средство, например **SQL Server Management Studio** для запуска скрипта и создавать таблицы, как это делалось при создании `School` базы данных для [в первом учебнике учебника рядов, Приступая к работе ](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). (Если вы загрузили базы данных.)

Теперь вы можете использовать `AlumniAssociation` модели данных в веб-страницы так же, как вы уже использовали `School` модели. Чтобы протестировать эту возможность, добавьте данные в таблицы и создать веб-страницу, на которой отображаются данные.

С помощью **обозревателя серверов**, добавить следующие строки для `Alumnus` и `Donation` таблицы.

[![Image21](what-s-new-in-the-entity-framework-4/_static/image34.png)](what-s-new-in-the-entity-framework-4/_static/image33.png)

Создать новый веб-страницу с именем *Alumni.aspx* , использующий *Site.Master* главной страницы. Добавьте следующую разметку, чтобы `Content` управления с именем `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample7.aspx)]

Эта разметка создает вложенные `GridView` элементов управления, внешние для отображения имен выпускникам и внутреннее отображение пожертвование даты и суммы.

Откройте *Alumni.aspx.cs*. Добавить `using` инструкции для данных, доступ к слоя и обработчик для внешнего `GridView` элемента управления `RowDataBound` событий:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample8.cs)]

Этот код осуществляет внутренний `GridView` управлять с помощью `Donations` свойство навигации текущей строки `Alumnus` сущности.

Запустите страницу.

[![Image22](what-s-new-in-the-entity-framework-4/_static/image36.png)](what-s-new-in-the-entity-framework-4/_static/image35.png)

(Примечание: это будет включена в загружаемый проект, но для его работы, вы должны создать базу данных локального экземпляра SQL Server Express; базы данных не будет включено как *.mdf* файла в *приложения\_ Данные* папки.)

Дополнительные сведения об использовании моделей в первую очередь платформы Entity Framework см. в разделе [моделей в первую очередь в Entity Framework 4](https://msdn.microsoft.com/en-us/data/ff830362.aspx).

## <a name="poco-support"></a>Поддержка POCO

При использовании методологии разработки на основе домена, при создании классов данных, представляющих данные и поведение, относящиеся к бизнес-среде. Эти классы должен быть независимым от любого конкретного технологии, используемой для хранения (Сохранить) данных. Другими словами, они должны быть *сохраняемость*. Сохраняемости можно также упростить класс модульного теста, так как проект модульного теста можно использовать технологии сохраняемости наиболее удобным для тестирования. Более ранних версий платформы Entity Framework предлагаемых ограниченная поддержка сохраняемости, так как наследование из классов сущностей `EntityObject` класса и таким образом включены значительные возможности Entity Framework функции.

Entity Framework 4 появилась возможность использовать классы сущностей, которые не наследуют `EntityObject` класса и, следовательно, игнорирующих сохраняемость. В контексте платформы Entity Framework, классов, как это обычно называется *plain old CLR объекты* (POCO или POCO). Можно написать классов POCO вручную, или можно автоматически создать их на основе существующей модели данных, с помощью преобразования текстовых шаблонов (T4) шаблонов, предоставляемых платформой Entity Framework.

Дополнительные сведения об использовании POCO платформы Entity Framework см. следующие ресурсы:

- [Работа с сущностями POCO](https://msdn.microsoft.com/en-us/library/dd456853.aspx). Это документ MSDN, Обзор POCO, со ссылками на другие документы, которые более подробные сведения.
- [Пошаговое руководство: POCO шаблона для платформы Entity Framework](https://blogs.msdn.com/b/adonet/archive/2010/01/25/walkthrough-poco-template-for-the-entity-framework.aspx) это записи блога от команды разработчиков платформы Entity Framework, со ссылками на другие сообщения в блогах о POCO.

## <a name="code-first-development"></a>Разработка кода

Поддержка POCO в Entity Framework 4, по-прежнему требуется создание модели данных и связывать классы сущностей в модели данных. В следующем выпуске платформы Entity Framework будет включать называемую *Разработка кода сначала*. Эта функция позволяет использовать Entity Framework с помощью классов POCO без необходимости использовать конструктор моделей данных или XML-файл модели данных. (Таким образом, этот параметр также был вызван *код только*; *сначала код* и *код только* ссылаются на один и тот же компонент платформы Entity Framework.)

Дополнительные сведения об использовании сначала код подход к разработке см. следующие ресурсы:

- [Разработка с использованием платформы Entity Framework 4 сначала код](https://weblogs.asp.net/scottgu/archive/2010/07/16/code-first-development-with-entity-framework-4.aspx). Это блога Скотта Гатри Знакомство с приложением сначала код разработки.
- [Блог группы разработки Entity Framework - учитывает CodeOnly с тегами](https://blogs.msdn.com/b/efdesign/archive/tags/codeonly/)
- [Блог группы разработки Entity Framework - учитывает с тегами Code First](https://blogs.msdn.com/b/efdesign/archive/tags/code+first/)
- [MVC Music Store учебник - 4 частей: модели и доступа к данным](../../../../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4.md)
- [Приступая к работе с MVC 3 — часть 4: разработку на основе кода Entity Framework](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model.md)

Кроме того, появилось новое учебное MVC сначала код, создающий приложение аналогично приложение Contoso университета проецируется публикацию в spring 2011 на [https://asp.net/entity-framework/tutorials](../../../../entity-framework.md)

## <a name="more-information"></a>Дополнительные сведения

Это завершает Обзор, чтобы новые возможности платформы Entity Framework и продолжение этого учебника ряда Entity Framework. Дополнительные сведения о новых возможностях в платформе Entity Framework 4, которые здесь не описаны см. следующие ресурсы:

- [Новые возможности ADO.NET](https://msdn.microsoft.com/en-us/library/ex6y04yf.aspx) раздел библиотеки MSDN о новых функциях в версии 4 платформы Entity Framework.
- [Представляем выпуск Entity Framework 4](https://blogs.msdn.com/b/efdesign/archive/2010/04/12/announcing-the-release-of-entity-framework-4.aspx) сообщение в блоге команды разработчиков платформа Entity Framework новые возможности в версии 4.

>[!div class="step-by-step"]
[Назад](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
