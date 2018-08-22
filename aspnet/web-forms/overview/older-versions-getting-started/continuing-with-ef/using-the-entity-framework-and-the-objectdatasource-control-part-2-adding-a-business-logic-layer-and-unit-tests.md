---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
title: 'С помощью Entity Framework 4.0 и элемент управления ObjectDataSource, часть 2: Добавление слой бизнес-логики и модульных тестов | Документация Майкрософт'
author: tdykstra
description: В этой серии руководств основана на веб-приложение университета Contoso, созданный по началу работы с этой серии руководств Entity Framework 4.0. I...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: efb0e677-10b8-48dc-93d3-9ba3902dd807
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
msc.type: authoredcontent
ms.openlocfilehash: 6517f037a03bb520ee4f3b3185a255b0eaa9de9a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835717"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests"></a>С помощью Entity Framework 4.0 и элемент управления ObjectDataSource, часть 2: Добавление слой бизнес-логики и модульных тестов
====================
по [том Дайкстра](https://github.com/tdykstra)

> В этой серии руководств основан на веб-приложение университета Contoso, которая создается с [Приступая к работе с Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) серии руководств. Если вы не прошли предыдущих учебных курсах, в качестве отправной точки для этого учебника вы можете [загрузить приложение](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , он был создан. Вы также можете [загрузить приложение](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , созданный путем завершения серии руководств. Если у вас есть вопросы о учебники, их можно разместить [форум ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


В предыдущем руководстве вы создали n уровневые веб-приложения с помощью Entity Framework и `ObjectDataSource` элемента управления. Этот учебник демонстрирует добавление бизнес-логики сохраняя бизнес-логики (BLL) и уровня доступа к данным (DAL) отдельных и показано, как создать автоматизированные модульные тесты для BLL.

В этом руководстве вы выполните следующие задачи:

- Создайте интерфейс репозитория, который объявляет методы доступа к данным, что нужно.
- Реализуйте интерфейс репозитория в класс репозитория.
- Создайте класс бизнес логики, которое вызывает класс репозитория для выполнения функций доступа к данным.
- Подключение `ObjectDataSource` управления класса бизнес логики, а не класс репозитория.
- Создайте проект модульного теста и класс репозитория, который используется коллекциями в памяти для хранения данных.
- Создание модульного теста для бизнес-логики, который требуется добавить к классу бизнес логики, а затем выполнить тест и увидеть, как это ошибкой.
- Реализация бизнес-логику в класс бизнес логики, а затем снова запустите модульного тестирования и см. в разделе, его успешного.

Вы будете работать с *Departments.aspx* и *DepartmentsAdd.aspx* страниц, созданных в предыдущем учебном курсе.

## <a name="creating-a-repository-interface"></a>Создание интерфейса репозитория

Вы начнете с создания интерфейса репозитория.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image1.png)

В *DAL* папки, создайте новый файл класса, назовите его *ISchoolRepository.cs*и замените существующий код следующим кодом:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample1.cs)]

Интерфейс определяет один метод для каждого из CRUD (Создание, чтение, обновление и удаление) методы, которые созданы в класс репозитория.

В `SchoolRepository` в класс *SchoolRepository.cs*, указать, что этот класс реализует `ISchoolRepository` интерфейса:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample2.cs)]

## <a name="creating-a-business-logic-class"></a>Создание класса бизнес логики

Далее создадим класс бизнес логики. Это делается таким образом, можно добавить бизнес-логику, которая будет выполняться по `ObjectDataSource` управления, несмотря на то, что вы не сделаете, еще. Теперь новый класс бизнес логики будет выполнять только те же операции CRUD, что и хранилище.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image3.png)

Создайте новую папку и назовите его *BLL*. (В реальных приложениях, бизнес-логики обычно реализуется как библиотека классов — отдельный проект, но чтобы упростить этот пример, классов BLL будут храниться в папке проекта.)

В *BLL* папки, создайте новый файл класса, назовите его *SchoolBL.cs*и замените существующий код следующим кодом:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample3.cs)]

Этот код создает те же методы CRUD, который вы видели ранее в класс репозитория, но вместо прямого обращения к методы Entity Framework, он вызывает репозитории методы класса.

Класс переменной, содержащей ссылку на класс репозитория определяется как тип интерфейса и код, создающий класс репозитория содержится в два конструктора. Конструктор без параметров будут использоваться `ObjectDataSource` элемента управления. Он создает экземпляр класса `SchoolRepository` класс, который был создан ранее. Другой конструктор позволяет любой код, который создает экземпляр класса бизнес логики, чтобы передать любой объект, реализующий интерфейс репозитория.

CRUD-методы, которые вызывают класс репозитория и два конструктора позволяют использовать с любой серверной части data store, можно выбрать класс бизнес логики. Класс бизнес логики не нужно учитывать как класс, который вызывает сохраняет данные. (Часто это называется *сохраняемости*.) Это упрощает модульное тестирование, поскольку класс бизнес логики могут подключаться к реализацию repository, который использует что-то в качестве простого в памяти `List` коллекций для хранения данных.

> [!NOTE]
> С технической точки зрения, будут по-прежнему не игнорирующих сохраняемость, объекты сущности так, как их все экземпляры из классов, унаследованных от платформы Entity Framework `EntityObject` класса. Для завершения сохраняемости, можно использовать *обычные старые объекты CLR*, или *POCO*, вместо объектов, наследующих от `EntityObject` класса. С помощью POCO выходит за рамки данного руководства. Дополнительные сведения см. в разделе [пригодности для тестирования и Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx) на веб-сайте MSDN.)


Теперь вы можете подключать `ObjectDataSource` элементы управления для бизнес логики вместо класса в репозиторий и убедитесь, что все работает, как и раньше.

В *Departments.aspx* и *DepartmentsAdd.aspx*, замените все вхождения `TypeName="ContosoUniversity.DAL.SchoolRepository"` для `TypeName="ContosoUniversity.BLL.SchoolBL`«. (Четыре экземпляра всего имеется.)

Запустите *Departments.aspx* и *DepartmentsAdd.aspx* страниц, чтобы убедиться, что они по-прежнему работать как раньше.

[!["Image01"](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image5.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image7.png)

## <a name="creating-a-unit-test-project-and-repository-implementation"></a>Создание проекта модульных тестов и реализация репозитория

Добавление нового проекта в решение с помощью **тестовый проект** шаблона и назовите его `ContosoUniversity.Tests`.

В тестовом проекте добавьте ссылку на `System.Data.Entity` и добавьте в проект ссылку на `ContosoUniversity` проекта.

Теперь можно создать класс репозитория, которое будет использоваться с модульными тестами. Хранилище данных для этого репозитория будет находиться в пределах класса.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image9.png)

В тестовом проекте, создайте новый файл класса, назовите его *MockSchoolRepository.cs*и замените существующий код следующим кодом:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample4.cs)]

Этот класс репозитория имеет те же методы CRUD, который напрямую обращается к Entity Framework, но они работают с `List` коллекций в памяти, а не с базой данных. Это упрощает для тестового класса для настройки и проверки модульных тестов для класса бизнес логики.

## <a name="creating-unit-tests"></a>Создание модульных тестов

**Тестирования** шаблон проекта класса заглушки модульных тестов для вас создан, и Ваша следующая задача — для изменения этого класса путем добавления методов модульного тестирования в него для бизнес-логики, который вы хотите добавить к классу бизнес логики.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image11.png)

Университета Contoso любой отдельных преподавателя может быть только администратор одного отдела, и необходимо добавить бизнес-логики для реализации этого правила. Начнем с добавления тесты и тесты, чтобы увидеть его сбой. Затем вы добавите код и повторно запустите тесты, ожидается их успешного выполнения.

Откройте *UnitTest1.cs* файл и добавьте `using` инструкции для бизнес-логики и доступа к данным уровней, которые созданы в проекте ContosoUniversity:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample5.cs)]

Замените `TestMethod1` метод со следующими методами:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample6.cs)]

`CreateSchoolBL` Метод создает экземпляр класса репозитория, который вы создали для проекта, который затем передается в новый экземпляр класса бизнес логики модульного теста. Затем данный метод использует класс бизнес логики для вставки три подразделения, которые можно использовать в методах теста.

Методы теста убедитесь, что класс бизнес логики вызывает исключение, если кто-то пытается вставить новое подразделение с администратор отдела, или если кто-то пытается обновить администратор отдела при установке для него идентификатор человека уже, кто является администратором другого отдела.

Вы не создавали класс исключений, поэтому этот код компилироваться не будет. Чтобы получить его компиляции, щелкните правой кнопкой мыши `DuplicateAdministratorException` и выберите **формирования**, а затем **класс**.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image13.png)

Это создает класс в тестовый проект, который можно удалить после создания класса исключения в основной проект. и реализации бизнес-логики.

Запустите проект теста. Как и ожидалось, тесты будут завершаться.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image15.png)

## <a name="adding-business-logic-to-make-a-test-pass"></a>Добавление бизнес-логики, чтобы сделать тестового прохода

Затем вы реализуете бизнес-логики, в которых невозможно задать в качестве администратора отдела человек, который уже является администратором для другой отдел. Вы выдачи исключения из уровня бизнес логики, а также catch на уровне представления данных, если пользователь редактирует отдел и щелкает **обновления** после выбора человек, который уже назначен администратор. (Можно также удалить из раскрывающегося списка, который уже являются администраторами, прежде чем отображать страницу преподавателей, но здесь предназначена для работы с бизнес-логики).

Начните с создания класса исключения, вы будете вызываемый, когда пользователь пытается сделать администратор отдела более одного преподавателя. В основной проект, создайте новый файл класса в *BLL* папки, назовите его *DuplicateAdministratorException.cs*и замените существующий код следующим кодом:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample7.cs)]

Теперь удалить временный *DuplicateAdministratorException.cs* файла, созданного в проекте теста. Чтобы иметь возможность компиляции.

В главном проекте откройте *SchoolBL.cs* файл и добавьте следующий метод, который содержит логику проверки. (Код ссылается на метод, который будет создано позже).

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample8.cs)]

Назовем этот метод при вставке или обновлении `Department` сущности, чтобы проверить, имеет ли другой отдел уже того же администратора.

Код вызывает метод для поиска в базе данных для `Department` сущность, которая имеет те же `Administrator` значение свойства как сущности вставке или обновлении. Если он найден, код создает исключение. Никакая проверка не является обязательным, если не содержит сущность вставке или обновлении `Administrator` значение и исключение не создается, если метод вызывается во время обновления и `Department` удалось найти сущность `Department` обновляемая сущность.

Вызовите новый метод из `Insert` и `Update` методов:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample9.cs)]

В *ISchoolRepository.cs*, добавьте следующее объявление нового метода доступа к данным:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample10.cs)]

В *SchoolRepository.cs*, добавьте следующий `using` инструкции:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample11.cs)]

В *SchoolRepository.cs*, добавьте следующий новый метод доступа к данным:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample12.cs)]

Этот код извлекает `Department` сущностей, которые имеют указанный администратора. Только один отдел должен найти (если таковые имеются). Тем не менее поскольку без ограничения встроена в базу данных, тип возвращаемого значения является коллекцией в случае, если находятся несколько отделов.

По умолчанию когда контекст объекта извлекает сущности из базы данных, данные о их в диспетчере состояния его объекта. `MergeOption.NoTracking` Параметр указывает, что такое отслеживание не будут использоваться для этого запроса. Это необходимо, поскольку запрос может возвратить точное сущности, для которой вы пытаетесь обновить, и затем вы не смогли бы подключить эту сущность. Например, если изменить подразделение журнал в *Departments.aspx* страницы и оставьте без изменений администратор, этот запрос вернет отделе журнал. Если `NoTracking` не задано, контекст объекта уже существует сущность department журнала в его диспетчер состояния объекта. Затем при присоединении сущности department журнал, который создается заново из состояния представления, контекст объекта создавал исключение, исключение, сообщающее `"An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key"`.

(В качестве альтернативы указанию `MergeOption.NoTracking`, можно создать новый контекст объекта только для этого запроса. Так как новый контекст объекта будет иметь собственный диспетчер состояний объекта, будет не было конфликтов при вызове `Attach` метод. Новый контекст объекта будет обмениваться подключения метаданные и базы данных с исходного контекста объекта, снижения производительности этот альтернативный подход будет минимальным. Подход, показанный здесь, однако повышает `NoTracking` параметр, который вы найдете полезные в других контекстах. `NoTracking` Возможность описана далее в этом руководстве этой серии.)

В тестовом проекте добавьте новый метод доступа к данным для *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample13.cs)]

Этот код использует LINQ для выполнения одно и то же выделение данных, `ContosoUniversity` репозитория проекта использует LINQ to Entities для.

Снова запустите тестовый проект. Результат на этот раз положительный.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image17.png)

## <a name="handling-objectdatasource-exceptions"></a>Обработка исключений ObjectDataSource

В `ContosoUniversity` проекта запустите *Departments.aspx* странице и попробовать изменить администратора отдела другому пользователю, который уже является администратором для другой отдел. (Помните, что можно изменить только подразделения, добавленные с помощью этого учебника, так как базы данных поставляется с недопустимыми данными.) Вы получите следующие страницы ошибки сервера:

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image19.png)

Можно запретить пользователям см. в разделе такого рода страницу ошибки, поэтому необходимо добавить код обработки ошибок. Откройте *Departments.aspx* и указать обработчик `OnUpdated` событие `DepartmentsObjectDataSource`. `ObjectDataSource` Теперь открывающий тег аналогичный приведенному ниже.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample14.aspx)]

В *Departments.aspx.cs*, добавьте следующий `using` инструкции:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample15.cs)]

Добавьте следующий обработчик для `Updated` событий:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample16.cs)]

Если `ObjectDataSource` управления перехватывает исключение при попытке выполнить обновление, он передает исключение в аргументе события (`e`) к этому обработчику. Код в обработчик проверяет, если используется исключение повторяющихся администратора. Если это так, код создает проверяющий элемент управления, который содержит сообщение об ошибке для `ValidationSummary` управления для отображения.

Запустить эту страницу и попытаться сделать пользователя администратором двух отделов еще раз. Это время `ValidationSummary` элемент управления отображает сообщение об ошибке.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image21.png)

Внести аналогичные изменения к *DepartmentsAdd.aspx* страницы. В *DepartmentsAdd.aspx*, указать обработчик `OnInserted` событие `DepartmentsObjectDataSource`. Разметка будет выглядеть следующим образом.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample17.aspx)]

В *DepartmentsAdd.aspx.cs*, добавить эту же `using` инструкции:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample18.cs)]

Добавьте следующий обработчик событий:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample19.cs)]

Теперь можно протестировать *DepartmentsAdd.aspx.cs* страницу, чтобы убедиться, что он также правильно обрабатывает пытается сделать один человек администратор более одного отдела.

На этом завершается введение в реализацию шаблон репозитория для использования `ObjectDataSource` элемента управления с Entity Framework. Дополнительные сведения о шаблон репозитория и пригодности для тестирования, см. в документе MSDN [пригодности для тестирования и Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx).

В следующем учебном курсе будет показано, как добавление функций в приложение сортировки и фильтрации.

> [!div class="step-by-step"]
> [Назад](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)
> [Вперед](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
