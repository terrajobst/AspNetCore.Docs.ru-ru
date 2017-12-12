---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Реализация наследования с использованием Entity Framework 6 в приложении ASP.NET MVC 5 (11, 12) | Документы Microsoft"
author: tdykstra
description: "Contoso университета примера веб-приложения показано, как создавать приложения ASP.NET MVC 5 с помощью Entity Framework 6 Code First и Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: e6ee3f9c055a15b13c27f94675006b9a7e804f1b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="implementing-inheritance-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-11-of-12"></a>Реализация наследования с использованием Entity Framework 6 в приложении ASP.NET MVC 5 (11, 12)
====================
По [Tom Dykstra](https://github.com/tdykstra)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) или [скачать PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso университета примера веб-приложения показано, как создавать приложения ASP.NET MVC 5 с помощью Entity Framework 6 Code First и Visual Studio 2013. Сведения о учебника серии см [в первом учебнике ряда](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


В предыдущем учебнике обработки исключений параллелизма. Этот учебник будет показано, как реализовать наследование в модели данных.

В объектно ориентированного программирования, можно использовать [наследования](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) для упрощения [повторное использование кода](http://en.wikipedia.org/wiki/Code_reuse). В этом учебнике мы изменим `Instructor` и `Student` классов, чтобы они являются производными от `Person` базового класса, который содержит свойства, такие как `LastName` , которые являются общими для преподавателей и учащихся. Не добавить или изменить веб-страницы, но мы изменим некоторую часть кода, и эти изменения будут автоматически отражаться в базе данных.

## <a name="options-for-mapping-inheritance-to-database-tables"></a>Параметры для сопоставления наследования с таблицами базы данных

`Instructor` И `Student` классы в `School` модель данных имеет несколько свойств, которые идентичны:

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

Предположим, что нужно исключить избыточный код для свойств, которые являются общими для `Instructor` и `Student` сущности. Или вы хотите создать службу, имена форматов без интересуясь ли имя, откуда инструктор или студент. Можно создать `Person` базового класса, который содержит только те общие свойства, а затем сделать `Instructor` и `Student` сущности являются производными от базового класса, как показано на следующем рисунке:

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Существует несколько способов эта структура наследования можно представить в базе данных. У вас может быть `Person` таблицу, содержащую сведения о учащихся и инструкторов в одной таблице. Некоторые столбцы могут применяются только к инструкторов (`HireDate`), некоторые только для учащихся (`EnrollmentDate`), некоторые для обоих (`LastName`, `FirstName`). Как правило, будет иметь *дискриминатора* представляет столбец, указывающий, какой тип каждой строки. Например столбец дискриминатора установлена «Инструктора» для инструкторов и «Студентов» для учащихся.

![Таблицы на hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Эта модель создания структура наследования сущностей из одной таблицы базы данных называется *таблица на иерархию* наследование (TPH).

Альтернативой является база данных больше похожи на структуру наследования. Например, может содержать только имя поля `Person` таблицы и иметь отдельный `Instructor` и `Student` таблицы с полями даты.

![Таблицы на type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Этот шаблон создания таблицы базы данных для каждого класса сущности вызывается *одна таблица на тип* наследования (TPT).

Еще другой вариант — Сопоставьте все типы неабстрактного для отдельных таблиц. Все свойства класса, включая унаследованные свойства сопоставляются со столбцами и соответствующая таблица. Этот шаблон вызывается наследования таблицы каждой конкретной реализацией класса (TPC). Если реализован TPC наследования для `Person`, `Student`, и `Instructor` классы, как показано выше, `Student` и `Instructor` таблицы будет выглядеть после реализации наследования, чем раньше не отличается.

TPC и схемы наследования TPH обычно работы с большей производительностью на платформе Entity Framework, чем TPT схемы наследования, поскольку шаблоны TPT может привести к сложного соединения запросов.

Этот учебник демонстрирует реализацию TPH наследования. TPH является шаблон наследование по умолчанию в платформе Entity Framework, поэтому все, нужно сделать — создать `Person` класса, измените `Instructor` и `Student` классы являются производными от `Person`, добавьте новый класс `DbContext`и создать Миграция. (Сведения о способах реализации других шаблонах наследования. в разделе [сопоставление наследования таблица на тип (TPT)](https://msdn.microsoft.com/en-us/data/jj591617#2.5) и [сопоставление наследования таблицы каждой конкретной реализацией класса Отправляемых](https://msdn.microsoft.com/en-us/data/jj591617#2.6) в MSDN Entity Framework документации.)

## <a name="create-the-person-class"></a>Создание класса Person

В *моделей* папке создайте *Person.cs* и замените код шаблона с помощью следующего кода:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>Учащихся и инструкторов классы наследуют от пользователя сделать

В *Instructor.cs*, являются производными `Instructor` класса из `Person` класса и удалять поля ключа и имени. Код будет выглядеть как в следующем примере:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Внести аналогичные изменения в *Student.cs*. `Student` Класс будет выглядеть как в следующем примере:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-the-person-entity-type-to-the-model"></a>Добавить в модель сущность типа Person

В *SchoolContext.cs*, добавьте `DbSet` свойство `Person` типа сущности:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Это все, что платформа Entity Framework требуется для настройки таблица на иерархию наследования. Как вы увидите, при обновлении базы данных, он будет иметь `Person` таблицы вместо `Student` и `Instructor` таблицы.

## <a name="create-and-update-a-migrations-file"></a>Создание и редактирование файла миграции

В пакет Диспетчер консоли (PMC), введите следующую команду:

`Add-Migration Inheritance`

Запустите `Update-Database` в PMC команду. Команда не будет выполнена на этом этапе у существующих данных, миграция не знает, как обрабатывать. Вы получаете сообщение об ошибке, как показано ниже:

> *Невозможно удалить объект "dbo. Инструктора ", так как он ссылается ограничение FOREIGN KEY.*


Откройте *миграций\&lt; отметка времени&gt;\_Inheritance.cs* и замените `Up` метод следующим кодом:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

Этот код отвечает за следующие задачи обновления базы данных:

- Удаление ограничения внешнего ключа и индексы, которые указывают на таблицу учащихся.
- Переименовывает таблицу в качестве лица инструктора и вносит изменения, необходимые для хранения данных студента:

    - Добавляет EnrollmentDate допускает значения NULL для учащихся.
    - Добавляет столбец дискриминатора для указания, является ли строка для учащихся или инструктор.
    - Делает HireDate допускает значение NULL, поскольку студента строк не будет иметь дат приема на работу.
    - Добавление временного поля, которое будет использоваться для обновления внешние ключи, указывающие на учащихся. При копировании студентов в таблице Person, где они могут получить новые значения первичных ключей.
- Копирует данные из таблицы студента в таблицу Person. В этом случае учащихся получить назначения нового значения первичного ключа.
- Устраняет значения внешнего ключа, указывающие на учащихся.
- Повторно создает ограничения внешнего ключа и индексы, теперь указывает таблицы Person.

(Если вы использовали GUID вместо целое число в качестве первичного ключа типа, студента значения первичного ключа не пришлось изменять и удалось некоторые из них была пропущена.)

Запустите `update-database` еще раз.

(В рабочей среде можно будет выполнить соответствующие изменения метода вниз в случае, если когда-либо было необходимо использовать, чтобы вернуться к предыдущей версии базы данных. В этом учебнике не будет использоваться метод вниз.)

> [!NOTE]
> Можно получить другие ошибки при переносе данных и внесения изменений схемы. Если возникают ошибки при миграции не удается устранить, можно будет продолжить учебника, изменив строку подключения в *Web.config* файлов или при удалении базы данных. Самым простым подходом является переименовать базу данных, в *Web.config* файла. Например измените имя базы данных ContosoUniversity2, как показано в следующем примере:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
> 
> С помощью новой базы данных нет данных для переноса и `update-database` команда гораздо больше шансов завершиться без ошибок. Инструкции по удалению базы данных см. в разделе [как удалить базу данных из Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). При использовании этого подхода, чтобы продолжить работу с учебником, пропустите шаг развертывания в конце этого учебника, или развернуть для нового сайта и базы данных. При развертывании обновления на том же сайте, в которой развертывание уже EF получите та же ошибка при выполнении миграции автоматически. Если вы хотите устранении ошибки миграции, лучший ресурс является одним из форумов Entity Framework или StackOverflow.com.


## <a name="testing"></a>Тестирование

Запустите сайт и попробуйте различные страницы. Все, что работает так же, как раньше.

В **обозревателя серверов** разверните **Connections\SchoolContext данные** и затем **таблиц**, и вы увидите, что **студента** и **Инструктора** таблицы были заменены **лицо** таблицы. Разверните **лицо** таблица отобразится все столбцы, которые используются в наличии **студента** и **инструктора** таблиц.

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Щелкните правой кнопкой мыши таблицу Person и нажмите кнопку **Показать таблицу данных** в столбце дискриминатора.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

На следующей схеме показана структура новой базы данных School:

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a>Развертывание в Azure

В этом разделе необходимо выполнить дополнительный **развертыванию приложения в Azure** статьи [часть 3, сортировку, фильтрацию и разбиение по страницам](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) из этого учебника ряда. Если у вас есть ошибки миграции, которые можно разрешить путем удаления базы данных в локальном проекте, пропустите этот шаг; Создание нового сайта и базы данных и развернуть в новой среде.

1. В Visual Studio щелкните правой кнопкой мыши проект в **обозревателе решений** и выберите **публикации** в контекстном меню.  
  
    ![Публикация в контекстном меню проекта](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)
2. Нажмите кнопку **Опубликовать**.  
  
    ![publish](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)  
  
 Веб-приложение открывается в браузере по умолчанию.
3. Протестируйте приложение, чтобы проверить его работы.

    При первом запуске страницы, который обращается к базе данных, платформа Entity Framework запускает все миграций `Up` методы, необходимые для перевода базы данных в актуальном состоянии с текущей моделью данных.

## <a name="summary"></a>Сводка

Таблица на иерархию наследования для реализации `Person`, `Student`, и `Instructor` классы. Дополнительные сведения об этом и других структур наследования см. в разделе [шаблон наследования TPT](https://msdn.microsoft.com/en-us/data/jj618293) и [шаблон наследования TPH](https://msdn.microsoft.com/en-us/data/jj618292) на сайте MSDN. В следующем уроке вы увидите, как обрабатывать разнообразные относительно сложных сценариев Entity Framework.

Ссылки на другие ресурсы Entity Framework можно найти в [доступа к данным ASP.NET - рекомендуется использовать ресурсы](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Назад](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Вперед](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
