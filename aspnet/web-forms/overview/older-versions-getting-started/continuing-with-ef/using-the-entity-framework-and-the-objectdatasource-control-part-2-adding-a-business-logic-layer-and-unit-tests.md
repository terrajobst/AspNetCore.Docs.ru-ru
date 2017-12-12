---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
title: "Использование Entity Framework 4.0 и управления ObjectDataSource, часть 2: Добавление слой бизнес-логики и модульные тесты | Документы Microsoft"
author: tdykstra
description: "Этот учебник ряд строится на веб-приложение Contoso университета, созданный Приступая к работе с рядами учебника Entity Framework 4.0. Я..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: efb0e677-10b8-48dc-93d3-9ba3902dd807
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
msc.type: authoredcontent
ms.openlocfilehash: 0440f807c7baa7b92e5f05590eca9cc237b5aef9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests"></a>Использование Entity Framework 4.0 и управления ObjectDataSource, часть 2: Добавление слой бизнес-логики и модульные тесты
====================
По [Tom Dykstra](https://github.com/tdykstra)

> Этот учебник ряд основан на веб-приложение Contoso университета, созданный [Приступая к работе с Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) учебника рядов. Если не была завершена ранее учебники, в качестве отправной точки для этого учебника вы можете [загрузить приложение](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , будет создана. Вы также можете [загрузить приложение](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , создаваемый для завершения учебника ряда. Если у вас есть вопросы о учебники, их можно разместить [форум по ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


В предыдущем учебнике вы создали n уровневые веб-приложения с помощью платформы Entity Framework и `ObjectDataSource` элемента управления. Этот учебник демонстрирует добавление бизнес-логики сохраняя бизнес-логики (уровень бизнес-ЛОГИКИ) и уровень доступа к данным (DAL) отдельных и показано, как создать автоматических модульных тестов для МЕТОДА.

В этом учебнике предстоит выполнить следующие задачи:

- Создайте интерфейс репозитория, который объявляет методы доступа к данным, что нужно.
- Реализуйте интерфейс репозитория в классе репозитория.
- Создайте класс бизнес логики, который вызывает класс репозитория для выполнения функций доступа к данным.
- Подключение `ObjectDataSource` класса бизнес логики, а не в репозитории класс элемента управления.
- Создайте проект модульного теста и репозитория класс, который использует коллекции в памяти для хранилища данных.
- Создание модульного теста для бизнес-логики, который вы хотите добавить класс бизнес логики, а затем выполнить тест и просмотреть его сбой.
- Реализация бизнес-логики в классе бизнес логики, а затем снова запустите модульного тестирования и тест статье.

Вы будете работать с *Departments.aspx* и *DepartmentsAdd.aspx* страниц, созданных в предыдущем учебнике.

## <a name="creating-a-repository-interface"></a>Создание интерфейса репозитория

Сначала путем создания интерфейса репозитория.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image1.png)

В *DAL* папки, создайте новый файл класса, назовите его *ISchoolRepository.cs*и замените существующий код следующим кодом:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample1.cs)]

Интерфейс определяет один метод для каждого CRUD (Создание, чтение, обновление и удаление) методы, которые вы создали в классе репозитория.

В `SchoolRepository` класса в *SchoolRepository.cs*, указать, что этот класс реализует `ISchoolRepository` интерфейс:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample2.cs)]

## <a name="creating-a-business-logic-class"></a>Создание класса бизнес логики

Затем вы создадите класс бизнес логики. Это делается, позволяют добавлять бизнес-логику, которая будет выполнена по `ObjectDataSource` управления, несмотря на то, что это не будет, пока. Теперь новый класс бизнес логики будет выполнять только те же операции CRUD, что и хранилище.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image3.png)

Создайте новую папку и назовите его *уровень бизнес-ЛОГИКИ*. (В реальном приложении, уровень бизнес логики обычно реализуется как библиотека классов — отдельный проект, но чтобы упростить этот пример, уровень бизнес-ЛОГИКИ классы будут храниться в папке проекта.)

В *уровень бизнес-ЛОГИКИ* папки, создайте новый файл класса, назовите его *SchoolBL.cs*и замените существующий код следующим кодом:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample3.cs)]

Этот код создает те же методы CRUD, который вы видели ранее в классе репозитория, но вместо прямого доступа к методы Entity Framework, она вызывает репозитории методы класса.

Класс переменной, содержащей ссылку на класс репозитория определяется как тип интерфейса и код, который создает экземпляр класса репозитория содержится в двух конструкторов. Конструктор без параметров будут использоваться `ObjectDataSource` элемента управления. Он создает экземпляр `SchoolRepository` класса, которое было создано ранее. Другой конструктор позволяет любой код, который создает экземпляр класса бизнес логики, чтобы передать любой объект, реализующий интерфейс репозитория.

Методы CRUD, которые вызывают класс репозитория и два конструктора позволяют с хранилищем независимо от внутренних данных, можно выбрать с помощью класса бизнес логики. Класс бизнес логики необходимо помните о том, как класс, он вызывает хранит данные. (Это часто называется *сохраняемости*.) Это облегчает модульного тестирования, поскольку класс бизнес логики могут подключаться к реализации репозитория, что-нибудь используется в качестве простого как в памяти `List` коллекции для хранения данных.

> [!NOTE]
> С технической точки зрения будут по-прежнему не игнорирующие сохраняемость, объекты сущности так, как их все экземпляры из классов, наследующих Entity Framework `EntityObject` класса. Для завершения сохраняемости, можно использовать *старые объекты CLR*, или *POCO*, вместо объектов, наследующих от `EntityObject` класса. С помощью POCO выходит за рамки данного руководства. Дополнительные сведения см. в разделе [возможностей тестирования и Entity Framework 4.0](https://msdn.microsoft.com/en-us/library/ff714955.aspx) на веб-сайте MSDN.)


Теперь можно подключиться `ObjectDataSource` элементы управления бизнес логики вместо класса в репозиторий и проверьте, что все работает, как раньше.

В *Departments.aspx* и *DepartmentsAdd.aspx*, изменить все вхождения `TypeName="ContosoUniversity.DAL.SchoolRepository"` для `TypeName="ContosoUniversity.BLL.SchoolBL`». (Отсутствуют четыре экземпляра во всех).

Запустите *Departments.aspx* и *DepartmentsAdd.aspx* страницы, чтобы убедиться, что они по-прежнему работать как раньше.

[!["Image01"](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image5.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image7.png)

## <a name="creating-a-unit-test-project-and-repository-implementation"></a>Создание проекта модульных тестов и реализация репозитория

Добавить новый проект в решении с помощью **тестовый проект** шаблона и назовите его `ContosoUniversity.Tests`.

В тестовом проекте добавьте ссылку на `System.Data.Entity` и добавьте ссылку на проект `ContosoUniversity` проекта.

Теперь можно создать класс репозитория, который будет использоваться с модульными тестами. Хранилище данных для этого репозитория будут внутри класса.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image9.png)

В тестовом проекте, создайте новый файл класса, назовите его *MockSchoolRepository.cs*и замените существующий код следующим кодом:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample4.cs)]

Этот класс репозиторий имеет те же методы CRUD, которая получает непосредственный доступ к платформе Entity Framework, но они работают с `List` коллекций в памяти, а не с базой данных. Это упростит класс теста для настройки и проверки модульных тестов для класса бизнес логики.

## <a name="creating-unit-tests"></a>Создание модульных тестов

**Тестирования** шаблон проекта создается класс заглушки модульных тестов, а следующая задача — для изменения этого класса путем добавления методов модульного теста для бизнес-логики, который требуется добавить в класс бизнес логики.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image11.png)

Университета Contoso все отдельные инструктора можно только администратор одному отделу, и необходимо добавить бизнес-логики, чтобы применить это правило. Начнем с добавления тесты и тесты, чтобы увидеть их ошибкой. Затем мы добавим код и тесты, повторно ожидаемой их передачи.

Откройте *UnitTest1.cs* и добавьте `using` инструкции для бизнес-логики и доступа к данным уровней, созданных в проекте ContosoUniversity:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample5.cs)]

Замените `TestMethod1` метод со следующими методами:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample6.cs)]

`CreateSchoolBL` Метод создает экземпляр класса репозитория, созданной для проекта, который затем передается в новый экземпляр класса бизнес логики модульного теста. Затем метод использует класс бизнес логики для вставки три подразделения, которые можно использовать в методах теста.

Методы теста убедитесь, что класс бизнес логики создает исключение, если кто-то пытается вставить новое подразделение с администратор отдела, или если кто-то пытается обновить администратор отдела, присвоив ему значение идентификатора пользователя кто уже администратор другой отдел.

Вы еще не созданы класс исключений, поэтому этот код не будет компилироваться. Чтобы получить его компиляции, щелкните правой кнопкой мыши `DuplicateAdministratorException` и выберите **формирования**, а затем **класса**.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image13.png)

Это создает класс в тестовом проекте, который можно удалить после создания класса исключения в главном проекте. и реализации бизнес-логики.

Запустите тестовый проект. Как и ожидалось, тест не пройден.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image15.png)

## <a name="adding-business-logic-to-make-a-test-pass"></a>Добавление бизнес-логики, чтобы выполнить проход теста

Далее необходимо реализовать бизнес-логику, в которых невозможно установить как администратор отдела тем, кто уже является администратором другого отдела. Будет вызывать исключение из бизнес-логики и перехватывать на уровне представления данных, если пользователь изменяет отдел и щелкает **обновление** после выбора тем, кто уже является администратором. (Можно также удалить инструкторов из раскрывающегося списка, который уже являются администраторами, до отображения страницы, но здесь предназначена для работы с бизнес-логики).

Начните с создания класса exception, вы будете вызываемый, когда пользователь пытается инструктора администратор более одного отдела. В главном проекте, создайте новый файл класса в *уровень бизнес-ЛОГИКИ* папки, назовите его *DuplicateAdministratorException.cs*и замените существующий код следующим кодом:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample7.cs)]

Теперь удалить временный *DuplicateAdministratorException.cs* файла, созданного в тестовом проекте, чтобы иметь возможность компиляции.

В главном проекте откройте *SchoolBL.cs* и добавьте следующий метод, который содержит логику проверки. (Код ссылается на метод, который вы создадите в более поздней версии).

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample8.cs)]

Этот метод будет вызван при вставке или обновлении `Department` сущности, чтобы проверить, имеет ли другой отдел уже же администратором.

Код вызывает метод для поиска в базе данных для `Department` сущность, которая имеет те же `Administrator` вставке или обновлении значения свойства сущности. Если он найден, код создает исключение. Никакая проверка не является обязательным, если не содержит сущность вставленных или обновленных `Administrator` значение и исключение не возникает, если метод вызывается во время обновления и `Department` удалось найти сущность `Department` обновляемой сущности.

Вызовите новый метод из `Insert` и `Update` методов:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample9.cs)]

В *ISchoolRepository.cs*, добавьте следующее объявление для нового метода доступа к данным:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample10.cs)]

В *SchoolRepository.cs*, добавьте следующий `using` инструкции:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample11.cs)]

В *SchoolRepository.cs*, добавьте следующий новый метод доступа к данным:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample12.cs)]

Этот код извлекает `Department` сущностей, которые имеют указанный администратора. Только один отдел показанному (если таковые имеются). Тем не менее поскольку без ограничения встроено в базе данных, тип возвращаемого значения является коллекцией в случае, если найдены нескольких отделов.

По умолчанию когда контекст объекта извлекает сущностей из базы данных, он сохраняет сведения о их в его диспетчер состояния объекта. `MergeOption.NoTracking` Параметр указывает, что это отслеживания не будет выполняться для этого запроса. Это необходимо, поскольку запрос может вернуть точное сущности, которую вы пытаетесь обновить, и затем вы не сможет присоединить сущность. Например, если изменить в отделе журнал *Departments.aspx* страницы и оставьте без изменений администратор, этот запрос вернет отдел журнала. Если `NoTracking` не задано, контекст объекта уже существует сущность department журнала в его диспетчер состояния объекта. Затем при присоединении сущность department журнала, он создается заново из состояния представления, контекст объекта вызывает исключение, сообщающее `"An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key"`.

(В качестве альтернативы для указания `MergeOption.NoTracking`, можно создать новый контекст объекта только для этого запроса. Так как новый контекст объекта будет иметь свой собственный диспетчер состояния объекта, то бы не было конфликтов при вызове `Attach` метода. Новый контекст объекта используют подключения метаданных и базы данных с исходного контекста объекта, поэтому потери производительности, этот вариант будет минимальным. Подход, показанный здесь, однако повышает `NoTracking` вариант, вы найдете полезные в других контекстах. `NoTracking` Параметр рассматривается далее в этом руководстве этой серии.)

В тестовом проекте добавьте новый метод доступа к данным *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample13.cs)]

Этот код использует LINQ для выполнения одно и то же выделение данных, `ContosoUniversity` LINQ to Entities для используется хранилище проекта.

Снова запустите тестовый проект. Результат на этот раз положительный.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image17.png)

## <a name="handling-objectdatasource-exceptions"></a>Обработка исключений ObjectDataSource

В `ContosoUniversity` проект, выполнить *Departments.aspx* страницы и попробовать изменить администратор отдела тем, кто уже является администратором другого отдела. (Помните, что можно изменить только отделов, добавленные с помощью этого учебника, так как базы данных поставляется с недопустимыми данными). Можно получить на следующей странице ошибка сервера:

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image19.png)

Требуется запретить пользователям видеть такого рода страницу ошибки, поэтому необходимо добавить код обработки ошибок. Откройте *Departments.aspx* и указать обработчик `OnUpdated` событие `DepartmentsObjectDataSource`. `ObjectDataSource` Теперь открывающий тег приведенной в следующем примере.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample14.aspx)]

В *Departments.aspx.cs*, добавьте следующий `using` инструкции:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample15.cs)]

Добавьте следующий обработчик `Updated` событий:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample16.cs)]

Если `ObjectDataSource` управления перехватывает исключение при попытке выполнить обновление, он передает исключение аргумента события (`e`) в этот обработчик. Код в обработчике проверяет, если используется исключение повторяющихся администратора. Если это так, код создает проверяющий элемент управления, который содержит сообщение об ошибке для `ValidationSummary` элемента управления для отображения.

Запустите страницу и попытаться сделать пользователя администратором двух отделов еще раз. Это время `ValidationSummary` элемент управления отображает сообщение об ошибке.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image21.png)

Внести аналогичные изменения в *DepartmentsAdd.aspx* страницы. В *DepartmentsAdd.aspx*, указать обработчик `OnInserted` событие `DepartmentsObjectDataSource`. Разметка будет выглядеть примерно так.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample17.aspx)]

В *DepartmentsAdd.aspx.cs*, добавить тот же `using` инструкции:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample18.cs)]

Добавьте следующий обработчик событий:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample19.cs)]

Теперь можно проверить *DepartmentsAdd.aspx.cs* страницу, чтобы убедиться, что также правильно обрабатывает пытается сделать администратор отдела несколько человек.

На этом завершается введение в реализации шаблон репозитория для использования `ObjectDataSource` управления с платформой Entity Framework. Дополнительные сведения о шаблон репозитория и тестирования см. в документе MSDN [возможностей тестирования и Entity Framework 4.0](https://msdn.microsoft.com/en-us/library/ff714955.aspx).

В этом руководстве вы увидите, как добавить, сортировку и фильтрацию функциональные возможности для приложения.

>[!div class="step-by-step"]
[Назад](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)
[Вперед](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
