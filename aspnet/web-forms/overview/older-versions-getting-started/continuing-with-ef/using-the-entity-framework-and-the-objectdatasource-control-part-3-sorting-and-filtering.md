---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'С помощью Entity Framework 4.0 и элемент управления ObjectDataSource, часть 3: сортировка и фильтрация | Документация Майкрософт'
author: tdykstra
description: В этой серии руководств основана на веб-приложение университета Contoso, созданный по началу работы с этой серии руководств Entity Framework 4.0. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 48d859191877ba951e233f19873d52625fe180ac
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378914"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a>С помощью Entity Framework 4.0 и элемент управления ObjectDataSource, часть 3: сортировка и фильтрация
====================
по [том Дайкстра](https://github.com/tdykstra)

> В этой серии руководств основан на веб-приложение университета Contoso, которая создается с [Приступая к работе с Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) серии руководств. Если вы не прошли предыдущих учебных курсах, в качестве отправной точки для этого учебника вы можете [загрузить приложение](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , он был создан. Вы также можете [загрузить приложение](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , созданный путем завершения серии руководств. Если у вас есть вопросы о учебники, их можно разместить [форум ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


В предыдущем учебном курсе реализации шаблона репозитория в n уровневых веб-приложение, использующее Entity Framework и `ObjectDataSource` элемента управления. Этом руководстве показано, как для выполнения сортировки и фильтрации и обработки сценариев «основной-подробности». Вам предстоит добавить следующие усовершенствования *Departments.aspx* страницы:

- Текстовое поле, чтобы разрешить пользователям выбирать отделов по имени.
- Список курсов для каждого отдела, который отображается в сетке.
- Возможность сортировать, щелкая заголовки столбцов.

[!["Image01"](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)

## <a name="adding-the-ability-to-sort-gridview-columns"></a>Добавление возможности для Сортировка столбцов GridView

Откройте *Departments.aspx* странице и добавить `SortParameterName="sortExpression"` атрибут `ObjectDataSource` управления с именем `DepartmentsObjectDataSource`. (Позже вы создадите `GetDepartments` метод, который принимает параметр с именем `sortExpression`.) Разметку для открывающего тега элемента управления, теперь, аналогичный приведенному ниже.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

Добавить `AllowSorting="true"` атрибута в открывающий тег элемента `GridView` элемента управления. Разметку для открывающего тега элемента управления, теперь, аналогичный приведенному ниже.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

В *Departments.aspx.cs*, задать порядок сортировки по умолчанию, вызвав `GridView` элемента управления `Sort` метода из `Page_Load` метод:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

Код, который сортирует или фильтры можно добавить в класс бизнес-логики или класс репозитория. Если сделать это в класс бизнес-логики, сортировке или фильтрации работы, выполняются после получения данных из базы данных, так как класс бизнес-логики работает с `IEnumerable` объект, возвращаемый в репозитории. Если добавить сортировку и фильтрацию код в класс репозитория и сделано перед выражение LINQ или запроса объектов был преобразован в `IEnumerable` объекта, ваши команды будут передаваться в базу данных для обработки, которая обычно более эффективен. В этом руководстве вы реализуете сортировки и фильтрации способом, который вызывает обработку, которые необходимо выполнить в базе данных, то есть в репозитории.

Чтобы добавить функцию сортировки, необходимо добавить новый метод в интерфейс репозитория и классы репозитория также относительно класс бизнес-логики. В *ISchoolRepository.cs* добавьте новый `GetDepartments` метода, принимающего `sortExpression` параметр, который будет использоваться для сортировки списка отделов, возвращается:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

`sortExpression` Параметр будет задавать столбец для сортировки и направление сортировки.

Добавьте код для нового метода к *SchoolRepository.cs* файла:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

Изменить существующий без параметров `GetDepartments` для вызова нового метода:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

В тестовом проекте, добавьте следующий новый метод для *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

Если вы предполагаете создать модульные тесты, которые зависят от этого метода возвращает отсортированный список, потребовалось бы для сортировки списка перед возвращением. Вы не будете создавать тесты, в этом руководстве описано, поэтому этот метод может просто возвращать неупорядоченного списка отделов.

В *SchoolBL.cs* добавьте следующий новый метод в класс бизнес-логики:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

Этот код передает параметр сортировки в метод репозитория.

Запустите *Departments.aspx* страницы.

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)

Теперь можно щелкнуть заголовок любого столбца для сортировки по этому столбцу. Если столбец уже отсортированы, щелкнув заголовок обращает направление сортировки.

## <a name="adding-a-search-box"></a>Добавление поля поиска

В этом разделе вы Добавьте текстовое поле поиска, связывание его с `ObjectDataSource` управление с помощью параметра управления и добавьте метод в класс бизнес-логики для поддержки фильтрации.

Откройте *Departments.aspx* странице и добавьте следующую разметку между заголовком и первый `ObjectDataSource` управления:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

В `ObjectDataSource` управления с именем `DepartmentsObjectDataSource`, выполните следующие действия:

- Добавить `SelectParameters` элемент для параметра с именем `nameSearchString` , возвращает значение, введенное в `SearchTextBox` элемента управления.
- Изменение `SelectMethod` значение для атрибута `GetDepartmentsByName`. (Вы создадите этот метод более поздней версии.)

Разметка для `ObjectDataSource` управления теперь аналогичный приведенному ниже:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

В *ISchoolRepository.cs*, добавьте `GetDepartmentsByName` метода, принимающего оба `sortExpression` и `nameSearchString` параметры:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

В *SchoolRepository.cs*, добавьте следующий новый метод:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

Этот код использует `Where` метод, чтобы выбрать элементы, которые содержат строку поиска. Если строка поиска пуста, выберет все записи. Обратите внимание, что при указании вместе вызовы методов в одной инструкции следующим образом (`Include`, затем `OrderBy`, затем `Where`), `Where` метод всегда должен быть последним.

Изменить существующий `GetDepartments` метода, принимающего `sortExpression` параметр для вызова нового метода:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

В *MockSchoolRepository.cs* в тестовом проекте, добавьте следующий новый метод:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

В *SchoolBL.cs*, добавьте следующий новый метод:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

Запустите *Departments.aspx* странице и введите строку поиска, чтобы убедиться в том, что логика выбора работает. Оставьте пустым текстовое поле и повторите поиск, чтобы убедиться в том, что возвращаются все записи.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)

## <a name="adding-a-details-column-for-each-grid-row"></a>Добавление столбца сведений для каждой строки сетки

После этого вы хотите просмотреть все курсы для каждого отдела, отображаемый в правую ячейку сетки. Чтобы сделать это, вы используете вложенный `GridView` управления и выполните привязку данных его данные из `Courses` свойство навигации `Department` сущности.

Откройте *Departments.aspx* и в разметке для `GridView` управления, указания обработчика для `RowDataBound` событий. Разметку для открывающего тега элемента управления, теперь, аналогичный приведенному ниже.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

Добавьте новый `TemplateField` после элемента `Administrator` поле шаблона:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

Эта разметка создает вложенный `GridView` элемента управления, показывающего номером и названием курса из списка курсов. Его не указать источник данных, так как вы будете databind его в коде в `RowDataBound` обработчика.

Откройте *Departments.aspx.cs* и добавьте следующий обработчик для `RowDataBound` событий:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

Этот код получает `Department` сущности из аргументов события, преобразует `Courses` свойство навигации, чтобы `List` коллекции, а также осуществляет вложенного `GridView` в коллекцию.

Откройте *SchoolRepository.cs* файла и укажите упреждающую загрузку для `Courses` свойство навигации, вызвав `Include` метод в запросе объект, созданный в `GetDepartmentsByName` метод. `return` Инструкции в `GetDepartmentsByName` метод теперь похож на приведенный ниже.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

Откройте страницу. В дополнение к фильтрации и сортировки возможность, добавленный ранее элемент управления GridView теперь отображаются сведения о вложенных курс для каждого отдела.

[!["Image01"](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)

На этом завершается введение в сценарии сортировки, фильтрации и «основной-подробности». В следующем руководстве вы увидите, как обеспечивать параллелизм.

> [!div class="step-by-step"]
> [Назад](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [Вперед](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
