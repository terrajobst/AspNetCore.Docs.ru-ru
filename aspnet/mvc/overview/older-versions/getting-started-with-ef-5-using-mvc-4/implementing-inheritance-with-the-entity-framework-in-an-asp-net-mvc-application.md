---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Реализация наследования с использованием платформы Entity Framework в приложении ASP.NET MVC (8, 10) | Документы Microsoft
author: tdykstra
description: Contoso университета примера веб-приложения демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: ee088f841bdb68f4806b0b62be7d379b9eab9f8c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a>Реализация наследования с использованием платформы Entity Framework в приложении ASP.NET MVC (8, 10)
====================
по [Tom Dykstra](https://github.com/tdykstra)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso университета примера веб-приложения демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio 2012. Сведения о серии руководств см. в [первом руководстве серии](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Учебник рядов можно запустить с самого начала или [загрузить начальный проект для этой главы](building-the-ef5-mvc4-chapter-downloads.md) и начните здесь.
> 
> > [!NOTE] 
> > 
> > Если возникли проблемы, не удается устранить, [загрузить завершенного главе](building-the-ef5-mvc4-chapter-downloads.md) и попробуйте воспроизвести проблему. Обычно можно найти решение проблемы путем сравнения код для завершения кода. Некоторые распространенные ошибки и способы их устранения см. в разделе [ошибок и способы их устранения.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


В предыдущем учебнике обработки исключений параллелизма. В этом учебнике демонстрируется, как реализовать наследование в модели данных.

В объектно ориентированное программирование, чтобы исключить избыточный код можно использовать наследование. В рамках этого учебника вы измените классы `Instructor` и `Student` таким образом, чтобы они были производными от базового класса `Person`, который содержит общие свойства для преподавателей и учащихся, такие как `LastName`. Изменения вносятся в коде, а не на веб-страницах, и автоматически отражаются в базе данных.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Таблица иерархию и таблица на тип наследования

В объектно ориентированного программирования, можно использовать наследование для упрощения работы с связанных классов. Например `Instructor` и `Student` классы в `School` модели данных совместно использовать несколько свойств, что приводит к избыточный код:

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

Предположим, что вам требуется исключить повторяющийся код для свойств, которые являются общими для сущностей `Instructor` и `Student`. Можно создать `Person` базового класса, который содержит только те общие свойства, а затем сделать `Instructor` и `Student` сущности являются производными от базового класса, как показано на следующем рисунке:

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

Структура наследования может быть представлена в базе данных несколькими способами. У вас может быть `Person` таблицу, содержащую сведения о учащихся и инструкторов в одной таблице. Некоторые столбцы могут применяются только к инструкторов (`HireDate`), некоторые только для учащихся (`EnrollmentDate`), некоторые для обоих (`LastName`, `FirstName`). Как правило, будет иметь *дискриминатора* представляет столбец, указывающий, какой тип каждой строки. Например, в столбце дискриминатора может указываться значение "Instructor" для преподавателей и "Student" для учащихся.

![Таблицы на hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

Эта модель создания структура наследования сущностей из одной таблицы базы данных называется *таблица на иерархию* наследование (TPH).

В качестве альтернативы можно создать базу данных, которая будет иметь приближенный к структуре наследования вид. Например, может содержать только имя поля `Person` таблицы и иметь отдельный `Instructor` и `Student` таблицы с полями даты.

![Таблицы на type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

Этот шаблон создания таблицы базы данных для каждого класса сущности вызывается *одна таблица на тип* наследования (TPT).

TPH схемы наследования обычно работы с большей производительностью в платформе Entity Framework, чем TPT схемы наследования, поскольку шаблоны TPT может привести к запросы сложного соединения. В этом учебнике демонстрируется реализация модели наследования "одна таблица на иерархию". Вам предстоит выполнить, выполнив следующие действия.

- Создание `Person` и измените их `Instructor` и `Student` классы являются производными от `Person`.
- Добавьте код сопоставления модели в базу данных для класса контекста базы данных.
- Изменение `InstructorID` и `StudentID` ссылки в проекте для `PersonID`.

## <a name="creating-the-person-class"></a>Создание класса Person

 Примечание: Невозможно скомпилировать проект после создания классов ниже, пока вы не обновите контроллеры, которые использует эти классы. 

В *моделей* папке создайте *Person.cs* и замените код шаблона с помощью следующего кода:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

В *Instructor.cs*, являются производными `Instructor` класса из `Person` класса и удалять поля ключа и имени. Код будет выглядеть следующим образом:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Внести аналогичные изменения в *Student.cs*. `Student` Класс будет выглядеть как в следующем примере:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a>Добавление типа Person сущности в модель

В *SchoolContext.cs*, добавьте `DbSet` свойство `Person` типа сущности:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Это все, что требуется платформе Entity Framework для настройки наследования типа "одна таблица на иерархию". Как можно будет увидеть, когда база данных создается заново, он будет иметь `Person` таблицы вместо `Student` и `Instructor` таблицы.

## <a name="changing-instructorid-and-studentid-to-personid"></a>Изменение InstructorID и идентификатором StudentID на PersonID

В *SchoolContext.cs*, в операторе сопоставления курса инструктора изменить `MapRightKey("InstructorID")` для `MapRightKey("PersonID")`:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

Это изменение не требуется; изменяется только имя столбца InstructorID в многие ко многим соединяемой таблице. Если оставить имя InstructorID, приложение по-прежнему будет работать неправильно. Ниже приведен полный *SchoolContext.cs*:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

Далее необходимо изменить `InstructorID` для `PersonID` и `StudentID` для `PersonID` в проекте ***за исключением*** в файлах с меткой времени миграции *миграций* папка. Для этого вы будете найти открывать только файлы, которые должны быть изменены, а затем провести Глобальное изменение открытых файлов. Единственным файлом в *миграций* папка, следует изменить *Migrations\Configuration.cs.*

1. > [!IMPORTANT]
   > Перед началом закрытие всех открытых файлов в Visual Studio.
2. Нажмите кнопку **найти и заменить — найти все файлы** в **изменить** меню и выполните поиск всех файлов в проекте, содержащих `InstructorID`.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. Откройте каждый файл в **результаты поиска** окна ***за исключением*** &lt;отметку времени&gt;*\_.cs* переноса файлов в *Миграций* папки, дважды щелкнув одну строку для каждого файла.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. Откройте **заменить в файлах** диалоговое окно и изменение **папка** для **все открытые документы**.
5. Используйте **заменить в файлах** диалоговое окно для изменения всех `InstructorID` для `PersonID.`  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. Найти все файлы в проекте, который содержит `StudentID`.
7. Откройте каждый файл в **результаты поиска** окна ***за исключением*** &lt;отметку времени&gt;*\_\*.cs* миграции файлов в *миграций* папки, дважды щелкнув одну строку для каждого файла.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. Откройте **заменить в файлах** диалоговое окно и изменение **папка** для **все открытые документы**.
9. Используйте **заменить в файлах** диалоговое окно для изменения всех `StudentID` для `PersonID`.   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. Выполните построение проекта.

(Обратите внимание, что этот пример демонстрирует *недостаток* из `classnameID` шаблон именования первичные ключи. Если бы код первичных ключей без префикса имени класса *не* переименование потребовалась бы теперь.)

## <a name="create-and-update-a-migrations-file"></a>Создание и редактирование файла миграции

В пакет Диспетчер консоли (PMC), введите следующую команду:

`Add-Migration Inheritance`

Запустите `Update-Database` в PMC команду. Команда не будет выполнена на этом этапе у существующих данных, миграция не знает, как обрабатывать. Появиться следующая ошибка:

*Конфликт инструкции ALTER TABLE с ограничением FOREIGN KEY «FK\_dbo. Отдел\_dbo. Лицо\_PersonID». Конфликт произошел в таблицу в базе данных «ContosoUniversity», «dbo. Пользователь», столбец «PersonID».*

Откройте *миграций\&lt; отметка времени&gt;\_Inheritance.cs* и замените `Up` метод следующим кодом:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

Запустите `update-database` еще раз.

> [!NOTE]
> Можно получить другие ошибки при переносе данных и внесения изменений схемы. Если возникают ошибки при миграции не удается устранить, можно будет продолжить учебника, изменив строку подключения в *Web.config* файла или удаление базы данных. Самым простым подходом является переименовать базу данных, в *Web.config* файла. Например, измените имя базы данных на CU\_тестирования, как показано в следующем примере:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> С помощью новой базы данных нет данных для переноса и `update-database` команда гораздо больше шансов завершиться без ошибок. Инструкции по удалению базы данных см. в разделе [как удалить базу данных из Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). При использовании этого подхода, чтобы продолжить работу с учебником, пропустите шаг развертывания в конце этого учебника, поскольку развернутого сайта будет получена та же ошибка возникла при выполнении миграции автоматически. Если вы хотите устранении ошибки миграции, лучший ресурс является одним из форумов Entity Framework или StackOverflow.com.


## <a name="testing"></a>Тестирование

Запустите сайт и попробуйте различные страницы. Все работает так же, как и раньше.

В **обозревателя серверов** разверните **SchoolContext** и затем **таблиц**, и вы увидите, что **студента** и **инструктора**  таблицы были заменены **лицо** таблицы. Разверните **лицо** таблица отобразится все столбцы, которые используются в наличии **студента** и **инструктора** таблиц.

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Щелкните таблицу Person правой кнопкой мыши и выберите команду **Показать данные таблицы**, чтобы просмотреть столбец дискриминатора.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

На следующей схеме показана структура новой базы данных School:

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a>Сводка

Таблица на иерархию наследования теперь реализована для `Person`, `Student`, и `Instructor` классы. Дополнительные сведения об этом и других структур наследования см. в разделе [стратегии сопоставление наследования](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) Morteza Manavi блога. В следующем уроке вы увидите некоторые способы реализации хранилища и единицы шаблонов рабочих элементов.

Ссылки на другие ресурсы Entity Framework можно найти в [Карта содержимого для доступа к данным ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Назад](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Вперед](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
