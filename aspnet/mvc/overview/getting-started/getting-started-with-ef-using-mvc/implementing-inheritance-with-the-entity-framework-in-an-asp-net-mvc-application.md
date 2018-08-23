---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Реализация наследования с использованием Entity Framework 6 в приложении ASP.NET MVC 5 (11 из 12) | Документация Майкрософт
author: tdykstra
description: Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 5, используя Entity Framework 6 Code First и Visual Studio...
ms.author: riande
ms.date: 11/07/2014
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9bfe860761a18cb4a69d33336d64a169e3c834f9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838853"
---
<a name="implementing-inheritance-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-11-of-12"></a>Реализация наследования с использованием Entity Framework 6 в приложении ASP.NET MVC 5 (11 из 12)
====================
по [том Дайкстра](https://github.com/tdykstra)

[Скачать завершенный проект](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) или [скачать PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 5, используя Entity Framework 6 Code First и Visual Studio 2013. Сведения о серии руководств см. в [первом руководстве серии](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


В предыдущем учебнике показана обработка исключений параллелизма. В этом учебнике демонстрируется, как реализовать наследование в модели данных.

В объектно ориентированного программирования, можно использовать [наследования](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) для упрощения [повторное использование кода](http://en.wikipedia.org/wiki/Code_reuse). В рамках этого учебника вы измените классы `Instructor` и `Student` таким образом, чтобы они были производными от базового класса `Person`, который содержит общие свойства для преподавателей и учащихся, такие как `LastName`. Изменения вносятся в коде, а не на веб-страницах, и автоматически отражаются в базе данных.

## <a name="options-for-mapping-inheritance-to-database-tables"></a>Варианты сопоставления наследования для таблиц базы данных

`Instructor` И `Student` классы в `School` модели данных имеют несколько свойств, которые идентичны:

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

Предположим, что вам требуется исключить повторяющийся код для свойств, которые являются общими для сущностей `Instructor` и `Student`. Кроме того, вам может потребоваться написать службу, которая может форматировать имена как преподавателей, так и учащихся. Можно создать `Person` базовый класс, который содержит только те общие свойства, а затем сделать `Instructor` и `Student` сущности являются производными от этого базового класса, как показано на следующем рисунке:

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Структура наследования может быть представлена в базе данных несколькими способами. Имеется `Person` таблицу, содержащую сведения об учащихся и преподавателей в одной таблице. Некоторые столбцы могут относиться только к преподавателям (`HireDate`), некоторые только к учащимся (`EnrollmentDate`), некоторые для обоих (`LastName`, `FirstName`). Как правило, пришлось бы *дискриминатора* представляет столбец, который указывает, какой тип каждой строки. Например, в столбце дискриминатора может указываться значение "Instructor" для преподавателей и "Student" для учащихся.

![Таблицы на hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Такая модель создания структуры наследования сущностей из одной таблицы базы данных называется *таблица на иерархию* наследования (TPH).

В качестве альтернативы можно создать базу данных, которая будет иметь приближенный к структуре наследования вид. Например, может иметь только поля с именами `Person` таблицы и иметь отдельные `Instructor` и `Student` таблицы с полями даты.

![Таблицы на type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Такая модель создания таблицы базы данных для каждого класса сущности называется *одна таблица на тип* наследование (TPT).

Кроме того, можно сопоставить все не являющиеся абстрактными типы с отдельными таблицами. Все свойства класса, включая унаследованные, сопоставляются со столбцами в соответствующей таблице. Такая модель называется наследованием типа "одна таблица на конкретный класс". Если вы реализовали наследование TPC для `Person`, `Student`, и `Instructor` классы, как показано выше, `Student` и `Instructor` таблицы будет выглядеть после реализации наследования, чем раньше не отличается.

TPC и шаблоны наследование TPH как правило, обеспечивают более высокую производительность на платформе Entity Framework, чем шаблоны наследования TPT, так как шаблоны TPT может привести к сложные запросы на соединение.

В этом учебнике демонстрируется реализация модели наследования "одна таблица на иерархию". TPH является шаблон наследования по умолчанию в Entity Framework, поэтому все, что необходимо сделать это создание `Person` измените `Instructor` и `Student` классам наследовать от `Person`, добавьте новый класс `DbContext`и создание Миграция. (Сведения о том, как реализовать другие схемы наследования, см. в разделе [сопоставление наследования таблица на тип (TPT)](https://msdn.microsoft.com/data/jj591617#2.5) и [таблица на конкретный класс (TPC) наследование сопоставления](https://msdn.microsoft.com/data/jj591617#2.6) в MSDN Документация по Entity Framework.)

## <a name="create-the-person-class"></a>Создание класса Person

В *моделей* папке создайте *Person.cs* и замените код шаблона следующим кодом:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>Создание классов Student и Instructor, унаследованных от класса Person

В *Instructor.cs*, являются производными `Instructor` класса из `Person` класса и удалите поля ключа и имени. Код будет выглядеть следующим образом:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Внести аналогичные изменения к *Student.cs*. `Student` Класса будет выглядеть как в следующем примере:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-the-person-entity-type-to-the-model"></a>Добавить тип сущности Person в модель

В *SchoolContext.cs*, добавьте `DbSet` свойство для `Person` типа сущности:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Это все, что требуется платформе Entity Framework для настройки наследования типа "одна таблица на иерархию". Как вы увидите, при обновлении базы данных, он будет иметь `Person` таблицы вместо `Student` и `Instructor` таблицы.

## <a name="create-and-update-a-migrations-file"></a>Создание и изменение файла миграции

В консоли диспетчера пакетов (PMC), введите следующую команду:

`Add-Migration Inheritance`

Запустите `Update-Database` команду в PMC. Команда завершится ошибкой на этом этапе, так как у нас есть миграции не знает как обрабатывать существующие данные. Вы получаете сообщение об ошибке, аналогичный приведенному ниже:

> *Невозможно удалить объект "dbo. Преподаватель "так как он ссылается ограничение FOREIGN KEY.*


Откройте *миграций\&lt; метка времени&gt;\_Inheritance.cs* и замените `Up` метод следующим кодом:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

Этот код выполняет следующие задачи по обновлению базы данных:

- Удаляет ограничения внешнего ключа и индексы, которые указывают на таблицу Student.
- Переименовывает таблицу Instructor в Person и вносит изменения, необходимые для сохранения в ней данных из таблицы Student:

    - Добавляет допускающий значения NULL тип EnrollmentDate для учащихся.
    - Добавляет столбец дискриминатора, который указывает, представляет ли строка учащегося или преподавателя.
    - Изменяет тип HireDate на допускающий значения NULL, поскольку в строках для учащихся не будет указываться дата приема на работу.
    - Добавляет временное поле, которое будет использоваться для обновления внешних ключей, указывающих на учащихся. При копировании записей учащихся в таблицу Person они получат новые значения первичного ключа.
- Копирует данные из таблицы Student в таблицу Person. При этом записям учащихся назначаются новые значения первичного ключа.
- Исправляет значения внешнего ключа, которые указывают на учащихся.
- Повторно создает ограничения внешнего ключа и индексы, которые после этого указывают на таблицу Person.

(Если вместо целочисленного типа первичного ключа используется GUID, значения первичного ключа для записей учащихся изменять не потребуется и некоторые из этих действий можно пропустить.)

Запустите `update-database` еще раз выполните команду.

(В рабочей системе вы вносите соответствующие изменения для метода вниз в случае, если вам когда-либо приходилось использовать, чтобы вернуться к предыдущей версии базы данных. В этом руководстве вы не используете метод вниз.)

> [!NOTE]
> Это можно получить другие ошибки при переносе данных и внесения изменений схемы. Если вы получаете ошибки миграции не удается устранить, можно будет продолжить руководства, изменив строку подключения в *Web.config* файла или путем удаления базы данных. Самым простым подходом является переименовать базу данных, в *Web.config* файл. Например измените имя базы данных на ContosoUniversity2, как показано в следующем примере:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
> 
> В новой базе данных нет данных для переноса и `update-database` команда является гораздо большей долей вероятности завершится без ошибок. Инструкции о том, как удалить базу данных, см. в разделе [как удалить базу данных из Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). При использовании этого подхода для продолжения работы с учебником пропустить шаг развертывания в конце этого руководства, или развернуть для нового сайта и базы данных. Если вы развернете обновление на тот же сайт, на которой развертывание уже, EF получите та же ошибка, при выполнении миграции автоматически. Если вы хотите устранении ошибки миграции, самый лучший ресурс является одним из форумы Entity Framework или StackOverflow.com.


## <a name="testing"></a>Тестирование

Запустите сайт и попробуйте открыть различные страницы. Все работает так же, как и раньше.

В **обозреватель серверов,** разверните **Connections\SchoolContext данных** и затем **таблиц**, и вы увидите, что **учащихся** и **Преподавателя** таблиц были заменены **Person** таблицы. Разверните **Person** таблицы чтобы увидеть, все столбцы, которые ранее были в наличии **учащихся** и **преподавателя** таблиц.

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Щелкните таблицу Person правой кнопкой мыши и выберите команду **Показать данные таблицы**, чтобы просмотреть столбец дискриминатора.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

На следующей схеме показана структура новой базы данных School:

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a>Развертывание в Azure

В этом разделе, необходимо завершить необязательный **развертыванию приложения в Azure** статьи [части 3, сортировку, фильтрацию и разбиение по страницам](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) этой серии руководств. Если у вас есть ошибки миграции, которые можно разрешить путем удаления базы данных в локальном проекте, пропустите этот шаг; или создайте новый сайт и базы данных, а развертывание в новую среду.

1. В Visual Studio щелкните правой кнопкой мыши проект в **обозревателе решений** и выберите **публикации** в контекстном меню.  
  
    ![Публикация в контекстном меню проекта](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)
2. Нажмите кнопку **Опубликовать**.  
  
    ![publish](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)  
  
   Веб-приложение откроется в браузере по умолчанию.
3. Протестируйте приложение, чтобы проверить его работы.

    При первом запуске страницы, который обращается к базе данных, платформа Entity Framework выполняет все миграции `Up` методы, необходимые для приведения базы данных в актуальном состоянии с текущей моделью данных.

## <a name="summary"></a>Сводка

Вы реализовали наследование типа "одна таблица на иерархию" для классов `Person`, `Student` и `Instructor`. Дополнительные сведения об этом и других структур наследования см. в разделе [модель наследования TPT](https://msdn.microsoft.com/data/jj618293) и [шаблон наследование TPH](https://msdn.microsoft.com/data/jj618292) на сайте MSDN. В рамках следующего учебника вы узнаете, как работать в нескольких сценариях Entity Framework с расширенными возможностями.

Ссылки на другие ресурсы Entity Framework можно найти в [доступ к данным ASP.NET — рекомендуемые ресурсы](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Назад](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Вперед](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
