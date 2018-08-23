---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Реализация наследования с использованием Entity Framework в приложении ASP.NET MVC (8 из 10) | Документация Майкрософт
author: tdykstra
description: Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 10ee0be62a1d601e323afc423e9022bed56f4f33
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835921"
---
<a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a>Реализация наследования с использованием Entity Framework в приложении ASP.NET MVC (8 из 10)
====================
по [том Дайкстра](https://github.com/tdykstra)

[Скачать завершенный проект](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 4, используя Entity Framework 5 Code First и Visual Studio 2012. Сведения о серии руководств см. в [первом руководстве серии](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Серии руководств можно начать с самого начала или [Загрузите начальный проект для этой главы](building-the-ef5-mvc4-chapter-downloads.md) и начните здесь.
> 
> > [!NOTE] 
> > 
> > Если вы столкнулись с проблемами, не удается устранить, [скачать завершенного глава](building-the-ef5-mvc4-chapter-downloads.md) и попробуйте воспроизвести проблему. Обычно можно найти решение проблемы, сравнивая код, чтобы полный код. Некоторые распространенные ошибки и способы их устранения, см. в разделе [ошибки и способы их устранения.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


В предыдущем учебнике показана обработка исключений параллелизма. В этом учебнике демонстрируется, как реализовать наследование в модели данных.

В объектно ориентированное программирование, чтобы исключить избыточный код можно использовать наследование. В рамках этого учебника вы измените классы `Instructor` и `Student` таким образом, чтобы они были производными от базового класса `Person`, который содержит общие свойства для преподавателей и учащихся, такие как `LastName`. Изменения вносятся в коде, а не на веб-страницах, и автоматически отражаются в базе данных.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Таблица на иерархию и таблица на тип наследования

В объектно ориентированного программирования, можно использовать наследование для упрощения работы с связанных классов. Например `Instructor` и `Student` классы в `School` модели данных совместно используют несколько свойств, что приводит к избыточный код:

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

Предположим, что вам требуется исключить повторяющийся код для свойств, которые являются общими для сущностей `Instructor` и `Student`. Можно создать `Person` базовый класс, который содержит только те общие свойства, а затем сделать `Instructor` и `Student` сущности являются производными от этого базового класса, как показано на следующем рисунке:

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

Структура наследования может быть представлена в базе данных несколькими способами. Имеется `Person` таблицу, содержащую сведения об учащихся и преподавателей в одной таблице. Некоторые столбцы могут относиться только к преподавателям (`HireDate`), некоторые только к учащимся (`EnrollmentDate`), некоторые для обоих (`LastName`, `FirstName`). Как правило, пришлось бы *дискриминатора* представляет столбец, который указывает, какой тип каждой строки. Например, в столбце дискриминатора может указываться значение "Instructor" для преподавателей и "Student" для учащихся.

![Таблицы на hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

Такая модель создания структуры наследования сущностей из одной таблицы базы данных называется *таблица на иерархию* наследования (TPH).

В качестве альтернативы можно создать базу данных, которая будет иметь приближенный к структуре наследования вид. Например, может иметь только поля с именами `Person` таблицы и иметь отдельные `Instructor` и `Student` таблицы с полями даты.

![Таблицы на type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

Такая модель создания таблицы базы данных для каждого класса сущности называется *одна таблица на тип* наследование (TPT).

TPH наследования как правило, обеспечивают более высокую производительность на платформе Entity Framework, чем шаблоны наследования TPT, так как шаблоны TPT может привести к сложные запросы на соединение. В этом учебнике демонстрируется реализация модели наследования "одна таблица на иерархию". Это можно сделать, выполнив следующие действия:

- Создание `Person` класса и измените `Instructor` и `Student` классам наследовать от `Person`.
- Добавьте код сопоставления модели для базы данных в классе контекста базы данных.
- Изменение `InstructorID` и `StudentID` ссылки в проекте для `PersonID`.

## <a name="creating-the-person-class"></a>Создание класса Person

 Примечание: Вы не сможете компиляции проекта после создания классов, указанных ниже, пока вы не обновите контроллеры, которые использует эти классы. 

В *моделей* папке создайте *Person.cs* и замените код шаблона следующим кодом:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

В *Instructor.cs*, являются производными `Instructor` класса из `Person` класса и удалите поля ключа и имени. Код будет выглядеть следующим образом:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Внести аналогичные изменения к *Student.cs*. `Student` Класса будет выглядеть как в следующем примере:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a>Добавление типа сущности Person в модель

В *SchoolContext.cs*, добавьте `DbSet` свойство для `Person` типа сущности:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Это все, что требуется платформе Entity Framework для настройки наследования типа "одна таблица на иерархию". Как можно будет увидеть, когда базы данных не будет создана повторно, он будет иметь `Person` таблицы вместо `Student` и `Instructor` таблицы.

## <a name="changing-instructorid-and-studentid-to-personid"></a>Изменение InstructorID и StudentID на PersonID

В *SchoolContext.cs*, в операторе сопоставления преподавателя курсов измените `MapRightKey("InstructorID")` для `MapRightKey("PersonID")`:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

Это изменение не является необходимым. изменяется только имя столбца InstructorID в таблицы соединения многие ко многим. Если оставить имя, InstructorID приложение по-прежнему будет работать правильно. Ниже приведен полный *SchoolContext.cs*:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

Далее необходимо изменить `InstructorID` для `PersonID` и `StudentID` для `PersonID` в проекте ***за исключением*** в файлах с меткой времени миграции *миграций* папка. Для этого вы сможете найти открывать только файлы, которые должны быть изменены, а затем выполнения глобальных изменений на открытые файлы. Единственным файлом в *миграций* папка, следует изменить *Migrations\Configuration.cs.*

1. > [!IMPORTANT]
   > Перед началом закрытие всех открытых файлов в Visual Studio.
2. Нажмите кнопку **найти и заменить — найти все файлы** в **изменить** меню и выполните поиск всех файлов в проекте, содержащих `InstructorID`.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. Откройте каждый файл в **результаты поиска** окно ***за исключением*** &lt;отметку времени&gt;*\_.cs* переноса файлов в *Миграций* папки, дважды щелкнув одну строку для каждого файла.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. Откройте **заменить в файлах** диалоговое окно и изменение **папка** для **все открытые документы**.
5. Используйте **заменить в файлах** диалогового окна, чтобы изменить все `InstructorID` для `PersonID.`  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. Найти все файлы в проекте, который содержит `StudentID`.
7. Откройте каждый файл в **результаты поиска** окно ***за исключением*** &lt;отметку времени&gt;*\_\*.cs* файлы данных миграции в *миграций* папки, дважды щелкнув одну строку для каждого файла.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. Откройте **заменить в файлах** диалоговое окно и изменение **папка** для **все открытые документы**.
9. Используйте **заменить в файлах** диалогового окна, чтобы изменить все `StudentID` для `PersonID`.   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. Выполните построение проекта.

(Обратите внимание, что этот пример демонстрирует *недостаток* из `classnameID` шаблон именования первичные ключи. Если идентификатор первичные ключи присвоил без префикса имени класса, *не* переименование было бы необходимо сейчас.)

## <a name="create-and-update-a-migrations-file"></a>Создание и изменение файла миграции

В консоли диспетчера пакетов (PMC), введите следующую команду:

`Add-Migration Inheritance`

Запустите `Update-Database` команду в PMC. Команда завершится ошибкой на этом этапе, так как у нас есть миграции не знает как обрабатывать существующие данные. Вы получите следующую ошибку:

*Конфликт инструкции ALTER TABLE с ограничением FOREIGN KEY «FK\_dbo. Отдел\_dbo. Person\_PersonID». Конфликт произошел в базе данных «ContosoUniversity», таблица «dbo. Пользователь», столбец «PersonID».*

Откройте *миграций\&lt; метка времени&gt;\_Inheritance.cs* и замените `Up` метод следующим кодом:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

Запустите `update-database` еще раз выполните команду.

> [!NOTE]
> Это можно получить другие ошибки при переносе данных и внесения изменений схемы. Если вы получаете ошибки миграции не удается устранить, можно будет продолжить руководства, изменив строку подключения в *Web.config* файла или удаление базы данных. Самым простым подходом является переименовать базу данных, в *Web.config* файл. Например, изменить имя базы данных на CU\_тестирования, как показано в следующем примере:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> В новой базе данных нет данных для переноса и `update-database` команда является гораздо большей долей вероятности завершится без ошибок. Инструкции о том, как удалить базу данных, см. в разделе [как удалить базу данных из Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). При этом подходе для продолжения работы с учебником, пропустите шаг развертывания в конце этого руководства, так как развернутого веб-сайта будет та же ошибка, при выполнении миграции автоматически. Если вы хотите устранении ошибки миграции, самый лучший ресурс является одним из форумы Entity Framework или StackOverflow.com.


## <a name="testing"></a>Тестирование

Запустите сайт и попробуйте открыть различные страницы. Все работает так же, как и раньше.

В **обозреватель серверов,** разверните **SchoolContext** и затем **таблиц**, и вы увидите, что **учащихся** и **преподавателя**  таблиц были заменены **Person** таблицы. Разверните **Person** таблицы чтобы увидеть, все столбцы, которые ранее были в наличии **учащихся** и **преподавателя** таблиц.

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Щелкните таблицу Person правой кнопкой мыши и выберите команду **Показать данные таблицы**, чтобы просмотреть столбец дискриминатора.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

На следующей схеме показана структура новой базы данных School:

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a>Сводка

Таблица на иерархию наследования теперь реализовано для `Person`, `Student`, и `Instructor` классы. Дополнительные сведения об этом и других структур наследования см. в разделе [стратегии сопоставление наследования](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) в блоге Morteza Manavi. В следующем учебном курсе вы увидите некоторые способы реализации репозиторий и блок рабочих шаблонов.

Ссылки на другие ресурсы Entity Framework можно найти в [схема содержимого для доступа к данным ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Назад](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Вперед](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
