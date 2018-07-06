---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
title: Новые возможности в Entity Framework 4.0 | Документация Майкрософт
author: tdykstra
description: В этой серии руководств основана на веб-приложение университета Contoso, созданный по началу работы с этой серии руководств Entity Framework 4.0. I...
ms.author: aspnetcontent
ms.date: 01/26/2011
ms.assetid: 393df4a8-b1db-44c4-9db7-2b533ca887d0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
msc.type: authoredcontent
ms.openlocfilehash: 960aaf7aa7c2cff5c51079ecfe50031fa82d5508
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827845"
---
<a name="whats-new-in-the-entity-framework-40"></a>Новые возможности в Entity Framework 4.0
====================
по [том Дайкстра](https://github.com/tdykstra)

> В этой серии руководств основан на веб-приложение университета Contoso, которая создается с [Приступая к работе с платформой Entity Framework](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) серии руководств. Если вы не прошли предыдущих учебных курсах, в качестве отправной точки для этого учебника вы можете [загрузить приложение](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , он был создан. Вы также можете [загрузить приложение](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , созданный путем завершения серии руководств. Если у вас есть вопросы о учебники, их можно разместить [форум ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


В предыдущем руководстве вы узнали некоторые методы для максимально увеличить производительность веб-приложение, использующее Entity Framework. Этом руководстве рассматриваются некоторые из наиболее важных новых функций в версии 4 платформы Entity Framework и ссылки на ресурсы, которые предоставляют более подробное введение все новые функции. Ниже приведены компоненты, выделенным в этом руководстве:

- Связи внешнего ключа.
- Выполнение пользовательских команд SQL.
- Разработка модели.
- Поддержка POCO.

Кроме того, учебник кратко рассматривается *разработки code first*, это функция, которая будет реализована в следующем выпуске платформы Entity Framework.

Чтобы начать работу с учебником, запустите Visual Studio и откройте веб-приложение университета Contoso, работали в предыдущем учебном курсе.

## <a name="foreign-key-associations"></a>Связи внешнего ключа

Версия 3.5 платформы Entity Framework включает свойства навигации, но оно не включило свойства внешнего ключа в модели данных. Например `CourseID` и `StudentID` столбцы `StudentGrade` таблицы будет пропущено `StudentGrade` сущности.

[!["Image01"](what-s-new-in-the-entity-framework-4/_static/image2.png)](what-s-new-in-the-entity-framework-4/_static/image1.png)

Причина этого подхода: что, строго говоря, внешние ключи являются физической реализации и не входящие в концептуальной моделью данных. Однако на практике часто бывает проще работать с сущностями в коде, когда у вас есть прямой доступ к ним.

Для примера как внешних ключей в модели данных можно упростить код, рассмотрим, как вам пришлось бы код *DepartmentsAdd.aspx* страницы без них. В `Department` сущности, `Administrator` свойство представляет собой внешний ключ, который соответствует `PersonID` в `Person` сущности. Чтобы установить связь между нового отдела и его администратором, все нужно было сделать было задано значение для `Administrator` свойство в `ItemInserting` обработчик событий элемента управления с привязкой к данным:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample1.cs)]

Без внешних ключей в модель данных будет обрабатывать `Inserting` событий элемента управления источника данных, а не `ItemInserting` события элемента управления с привязкой к данным, чтобы получить ссылку на самой сущности перед добавлением сущности набора сущностей. При наличии эту ссылку, вы устанавливаете ассоциации с помощью кода, в следующих примерах:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample2.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample3.cs)]

Как вы видите в группы Entity Framework [записи блога в связи внешнего ключа](https://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx), других случаях, где сложности кода отличается гораздо больше. В соответствии с потребностями из тех, кто предпочитает смириться с детали реализации, в концептуальной модели для обеспечения более простой код, Entity Framework теперь предоставляет возможность в модели данных, в том числе внешних ключей.

В терминологии Entity Framework, если включить внешние ключи в модели данных вы используете *ассоциации внешних ключей*, и если вы исключили внешние ключи, вы используете *независимых сопоставлений*.

## <a name="executing-user-defined-sql-commands"></a>Выполнение команд SQL, определяемые пользователем

В более ранних версиях Entity Framework невозможно было легко создать собственные команды SQL в режиме реального времени и выполнять их. Платформа Entity Framework динамически созданных команд SQL для вас либо было необходимо создать хранимую процедуру и импортируйте его как функцию. Добавляет версии 4 `ExecuteStoreQuery` и `ExecuteStoreCommand` методы `ObjectContext` класс, который облегчает можно передать любой запрос непосредственно к базе данных.

Предположим, что администраторам университета Suppose необходимо иметь возможность выполнять массовые изменения в базе данных без необходимости проходить через процесс создания хранимой процедуры и их импорта в модель данных. Их первый запрос — для страницы, которая позволяет изменить число зачетных баллов для каждого курса в базе данных. На веб-странице, они хотят иметь возможность введите число, которое используется для умножения значения каждого `Course` строки `Credits` столбца.

Создание новой страницы, которая использует *Site.Master* главную страницу и назовите его *UpdateCredits.aspx*. Затем добавьте следующую разметку, чтобы `Content` управления с именем `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample4.aspx)]

Эта разметка создает `TextBox` элемента управления, в котором пользователь может ввести значение множителя `Button` управления нажмите кнопку, чтобы выполнить команду и `Label` управления указывает, число затронутых строк.

Откройте *UpdateCredits.aspx.cs*и добавьте следующие `using` инструкции и обработчик для кнопки `Click` событий:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample5.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample6.cs)]

Этот код выполняет SQL-запрос `Update` команды, используя значение в текстовое поле и использует метку для отображения числа обработанных строк. Перед выполнением страницы запуска *Courses.aspx* страницу, чтобы получить некоторые данные изображения «до».

[![Image02](what-s-new-in-the-entity-framework-4/_static/image4.png)](what-s-new-in-the-entity-framework-4/_static/image3.png)

Запустите *UpdateCredits.aspx*, введите «10» в качестве множитель и нажмите кнопку **Execute**.

[![Image03](what-s-new-in-the-entity-framework-4/_static/image6.png)](what-s-new-in-the-entity-framework-4/_static/image5.png)

Запустите *Courses.aspx* страницу, чтобы проверить измененные данные.

[![Image04](what-s-new-in-the-entity-framework-4/_static/image8.png)](what-s-new-in-the-entity-framework-4/_static/image7.png)

(Если вы хотите задать число кредитов к исходным значениям в *UpdateCredits.aspx.cs* изменить `Credits * {0}` для `Credits / {0}` и повторно запустить эту страницу, введя 10, что и делитель.)

Дополнительные сведения о выполнении запросов, которые определяют в коде, см. в разделе [как: непосредственно выполнять команды от источника данных](https://msdn.microsoft.com/library/ee358769.aspx).

## <a name="model-first-development"></a>Разработка модели

В этих пошаговых руководствах вы сначала создать базу данных и затем создается модель данных на основе структуры базы данных. В Entity Framework 4 можно запустить с моделью данных, вместо этого и создать базу данных, на основе структуры модели данных. Если вы создаете приложение, для которого еще не существует базы данных, основанная на модели подход позволяет создавать сущностей и связей, которые имеют смысл концептуально для приложения, не заботясь о деталях физической реализации . (Это справедливо только на начальных стадиях разработки, однако. Со временем база данных будет создан и рабочих данных в нем, и создать его заново из модели будет практические; на этом этапе вы будете вернемся к рассмотрению сначала база данных.)

В этом разделе руководства будет создание простой модели данных и создать базу данных из него.

В **обозревателе решений**, щелкните правой кнопкой мыши *DAL* папку и выберите **Добавление нового элемента**. В **Добавление нового элемента** диалогового **установленные шаблоны** выберите **данных** , а затем выберите **ADO.NET Entity Data Model** шаблона . Назовите новый файл *AlumniAssociationModel.edmx* и нажмите кнопку **добавить**.

[![Image06](what-s-new-in-the-entity-framework-4/_static/image10.png)](what-s-new-in-the-entity-framework-4/_static/image9.png)

Будет запущен мастер модели EDM. В **Выбор содержимого модели** выберите **пустую модель** и нажмите кнопку **Готово**.

[![Image07](what-s-new-in-the-entity-framework-4/_static/image12.png)](what-s-new-in-the-entity-framework-4/_static/image11.png)

**Конструктора моделей EDM** открывается с помощью пустого конструктора. Перетащите **сущности** элемент из **элементов** в область конструктора.

[![Image08](what-s-new-in-the-entity-framework-4/_static/image14.png)](what-s-new-in-the-entity-framework-4/_static/image13.png)

Измените имя сущности с `Entity1` для `Alumnus`, изменить `Id` имя свойства для `AlumnusId`и добавьте новое скалярное свойство с именем `Name`. Добавление новых свойств нажатии клавиши ВВОД после изменения имени `Id` столбца, или щелкните правой кнопкой мыши сущность и выберите **добавьте скалярное свойство**. По умолчанию для новых свойств используется тип `String`, что годится для этой простой демонстрации, но, конечно, вы можете изменить таких вещей, как тип данных в **свойства** окна.

Создайте другой сущности так же, как и назовите его `Donation`. Изменение `Id` свойства `DonationId` и добавление скалярного свойства с именем `DateAndAmount`.

[![Image09](what-s-new-in-the-entity-framework-4/_static/image16.png)](what-s-new-in-the-entity-framework-4/_static/image15.png)

Чтобы добавить ассоциацию между этими двумя сущностями, щелкните правой кнопкой мыши `Alumnus` сущности, выберите **добавить**, а затем выберите **ассоциации**.

[![Image10](what-s-new-in-the-entity-framework-4/_static/image18.png)](what-s-new-in-the-entity-framework-4/_static/image17.png)

Значения по умолчанию в **добавить сопоставление** представляют собой желаемого (один ко многим, включают свойства навигации, включать внешние ключи), поэтому просто нажмите кнопку **ОК**.

[![Image11](what-s-new-in-the-entity-framework-4/_static/image20.png)](what-s-new-in-the-entity-framework-4/_static/image19.png)

Конструктор добавляет линию связи и свойства внешнего ключа.

[![Image12](what-s-new-in-the-entity-framework-4/_static/image22.png)](what-s-new-in-the-entity-framework-4/_static/image21.png)

Теперь все готово для создания базы данных. Щелкните правой кнопкой мыши область конструктора и выберите **создать базу данных из модели**.

[![Image13](what-s-new-in-the-entity-framework-4/_static/image24.png)](what-s-new-in-the-entity-framework-4/_static/image23.png)

Будет запущен мастер создания базы данных. (Если вы видите предупреждения, указывающие, что сущности не сопоставлены, можно пропустить их, на данный момент.)

В **Выбор подключения к данным** действии, нажмите кнопку **новое подключение**.

[![Image14](what-s-new-in-the-entity-framework-4/_static/image26.png)](what-s-new-in-the-entity-framework-4/_static/image25.png)

В **свойства соединения** диалоговое окно, выберите локальный экземпляр SQL Server Express и базе данных имя `AlumniAsssociation`.

[![Image15](what-s-new-in-the-entity-framework-4/_static/image28.png)](what-s-new-in-the-entity-framework-4/_static/image27.png)

Нажмите кнопку **Да** при появлении, если вы хотите создать базу данных. Когда **Выбор подключения к данным** шаг снова отображается, нажмите кнопку **Далее**.

В **Сводка и параметры** действии, нажмите кнопку **Готово**.

[![Image18](what-s-new-in-the-entity-framework-4/_static/image30.png)](what-s-new-in-the-entity-framework-4/_static/image29.png)

Объект *.sql* создается файл с помощью команд языка DDL определения данных, но еще не запущено команды.

[![Image20](what-s-new-in-the-entity-framework-4/_static/image32.png)](what-s-new-in-the-entity-framework-4/_static/image31.png)

Используйте это средство, например **SQL Server Management Studio** для запуска скрипта и создавать таблицы, как это делалось при создании `School` базы данных для [в первом учебнике этой серии руководств, Приступая к работе ](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). (Если вы загрузили базы данных.)

Теперь вы можете использовать `AlumniAssociation` модели данных в веб-страницы так же, вы уже использовали `School` модели. Чтобы испытать эту возможность, добавьте данные в таблицы и создать веб-страницу, которая отображает данные.

С помощью **обозревателя серверов**, добавьте следующие строки к `Alumnus` и `Donation` таблицы.

[![Image21](what-s-new-in-the-entity-framework-4/_static/image34.png)](what-s-new-in-the-entity-framework-4/_static/image33.png)

Создание нового веб-страницы с именем *Alumni.aspx* , использующий *Site.Master* главной страницы. Добавьте следующую разметку для `Content` управления с именем `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample7.aspx)]

Эта разметка создает вложенные `GridView` элементы управления, внешнего один для отображения имен выпускников, а внутреннее для отображения даты пожертвование и суммы.

Откройте *Alumni.aspx.cs*. Добавить `using` инструкции для данных, доступ к слоя и обработчик для внешнего `GridView` элемента управления `RowDataBound` событий:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample8.cs)]

Этот код осуществляет внутренний `GridView` управление с помощью `Donations` свойство навигации текущей строки `Alumnus` сущности.

Откройте страницу.

[![Image22](what-s-new-in-the-entity-framework-4/_static/image36.png)](what-s-new-in-the-entity-framework-4/_static/image35.png)

(Примечание: Эта страница входит в загружаемый проект, но для его работы, вы должны создать базу данных локального экземпляра SQL Server Express; базы данных не будет включено как *.mdf* файл *приложения\_ Данные* папки.)

Дополнительные сведения об использовании моделей в первую очередь службы платформы Entity Framework см. в разделе [Model-First в Entity Framework 4](https://msdn.microsoft.com/data/ff830362.aspx).

## <a name="poco-support"></a>Поддержка POCO

При использовании методологии проблемно ориентированном проектировании, создании классов данных, представляющих данные и поведение, относящийся к предметной области бизнеса. Эти классы должны быть независимыми любой определенной технологии, используемый для хранения (Сохранить) данных. Другими словами, они должны быть *игнорирующих сохраняемость*. Пропуск сохраняемости можно также упростить класс модульного теста, так как проект модульного теста можно использовать технологии сохраняемости наиболее удобно для тестирования. Более ранних версий платформы Entity Framework предлагается ограниченная поддержка сохраняемости, так как наследование от классов сущностей `EntityObject` класса и таким образом включены немало функций сущности с платформой.

Entity Framework 4 появилась возможность использовать классы сущностей, которые не наследуют `EntityObject` класса и, следовательно, игнорирующих сохраняемость. В контексте платформы Entity Framework, такие классы, как это обычно называется *обычные старые объекты CLR* (POCO или POCO). Классы POCO можно написать вручную или вы можете автоматически создавать их в зависимости от существующей модели анализа данных с помощью шаблонов Text Template Transformation Toolkit (T4), предоставляемые платформой Entity Framework.

Дополнительные сведения об использовании POCO в Entity Framework см. следующие ресурсы:

- [Работа с сущностями POCO](https://msdn.microsoft.com/library/dd456853.aspx). Это документ MSDN, в котором приведен обзор POCO, со ссылками на другие документы, которые содержат более подробные сведения.
- [Пошаговое руководство: POCO шаблона для Entity Framework](https://blogs.msdn.com/b/adonet/archive/2010/01/25/walkthrough-poco-template-for-the-entity-framework.aspx) это записи блога от команды разработчиков платформы Entity Framework, со ссылками на другие записи в блогах о POCO.

## <a name="code-first-development"></a>Разработка кода

Поддержка POCO в Entity Framework 4 по-прежнему необходимо создать модель данных и связать ваши классы сущностей модели данных. Следующая версия Entity Framework будет включать функцию, называемую *разработки code first*. Эта функция позволяет использовать Entity Framework с помощью собственных классов POCO без необходимости использовать в конструкторе моделей или файл XML модели данных. (Таким образом, этот параметр также был вызван *только код*; *code first* и *только код* указывают на один и тот же компонент Entity Framework.)

Дополнительные сведения о подходе code first для разработки см. следующие ресурсы:

- [Разработка с помощью Entity Framework 4 Code-First](https://weblogs.asp.net/scottgu/archive/2010/07/16/code-first-development-with-entity-framework-4.aspx). Это записи блога Скотта Гатри Знакомство с разработки code first.
- [Блог группы разработки Entity Framework - учитывает CodeOnly с тегами](https://blogs.msdn.com/b/efdesign/archive/tags/codeonly/)
- [Записи с тегами Code First в блоге группы разработки Entity Framework-](https://blogs.msdn.com/b/efdesign/archive/tags/code+first/)
- [Руководство по MVC Music Store — часть 4: моделей и доступа к данным](../../../../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4.md)
- [Начало работы с MVC 3. часть 4: разработки Code First Entity Framework](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model.md)

Кроме того появилось новое учебное MVC Code-First, создающий приложения как для приложения университета Contoso проецируется публикуемого весной 2011 г. в [https://asp.net/entity-framework/tutorials](../../../../entity-framework.md)

## <a name="more-information"></a>Дополнительные сведения

На этом завершается с обзором, чтобы новые возможности в Entity Framework и это продолжение серии руководств Entity Framework. Дополнительные сведения о новых возможностях в Entity Framework 4, не входящих в здесь см. следующие ресурсы:

- [Новые возможности в ADO.NET](https://msdn.microsoft.com/library/ex6y04yf.aspx) статье MSDN о новых функциях в версии 4 платформы Entity Framework.
- [Объявление о выпуске Entity Framework 4](https://blogs.msdn.com/b/efdesign/archive/2010/04/12/announcing-the-release-of-entity-framework-4.aspx) записи блога группы разработчиков платформы Entity Framework о новых возможностях в версии 4.

> [!div class="step-by-step"]
> [Назад](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
