---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Использование Entity Framework 4.0 и управления ObjectDataSource, часть 3: сортировка и фильтрация | Документы Microsoft'
author: tdykstra
description: Этот учебник ряд строится на веб-приложение Contoso университета, созданный Приступая к работе с рядами учебника Entity Framework 4.0. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: e412d3ad98a37931e7190a4909cb09fa2abfb3d0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a>Использование Entity Framework 4.0 и управления ObjectDataSource, часть 3: сортировка и фильтрация
====================
по [Tom Dykstra](https://github.com/tdykstra)

> Этот учебник ряд основан на веб-приложение Contoso университета, созданный [Приступая к работе с Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) учебника рядов. Если не была завершена ранее учебники, в качестве отправной точки для этого учебника вы можете [загрузить приложение](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , будет создана. Вы также можете [загрузить приложение](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , создаваемый для завершения учебника ряда. Если у вас есть вопросы о учебники, их можно разместить [форум по ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


В предыдущем учебнике реализован шаблон репозитория в n уровневые веб-приложения, использующего платформу Entity Framework и `ObjectDataSource` элемента управления. Этого учебника показано, как выполнять сортировку и фильтрацию и обработать основной-подробности. Вам предстоит добавить следующие усовершенствования для *Departments.aspx* страницы:

- Текстовое поле, чтобы разрешить пользователям выбирать подразделения по имени.
- Список курсов для каждого отдела, который отображается в сетке.
- Возможность сортировать, щелкая заголовки столбцов.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)

## <a name="adding-the-ability-to-sort-gridview-columns"></a>Добавление возможности сортировки столбцов GridView

Откройте *Departments.aspx* и добавьте `SortParameterName="sortExpression"` атрибут `ObjectDataSource` управления с именем `DepartmentsObjectDataSource`. (Позже вы создадите `GetDepartments` метод, который принимает параметр с именем `sortExpression`.) Разметку для открывающего тега элемента управления теперь приведенной в следующем примере.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

Добавить `AllowSorting="true"` для открывающего тега элемента атрибута `GridView` элемента управления. Разметку для открывающего тега элемента управления теперь приведенной в следующем примере.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

В *Departments.aspx.cs*, задать порядок сортировки по умолчанию, вызвав `GridView` элемента управления `Sort` метод `Page_Load` метод:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

Можно добавить код, который сортирует или фильтрует бизнес логики класса или класса репозитория. Если сделать это в классе логику, сортировке или фильтрации рабочих выполняются после извлечения данных из базы данных, так как логика классе работает с `IEnumerable` объект, возвращаемый в репозиторий. Если добавить сортировку и фильтрацию кода в классе репозитория и можно сделать перед выражение LINQ или запроса объектов был преобразован в `IEnumerable` объекта команды будут передаваться в базу данных для обработки, который обычно более эффективно. В этом учебнике будет реализован сортировки и фильтрации, в результате которого вызывает обработку, которые необходимо выполнить в базе данных, то есть в репозитории.

Чтобы добавить функцию сортировки, необходимо добавить новый метод в репозиторий интерфейс и классы репозитория также относительно бизнес логики класса. В *ISchoolRepository.cs* файл, добавьте новый `GetDepartments` метода, принимающего `sortExpression` параметр, который будет использоваться для сортировки списка отделов, возвращается:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

`sortExpression` Параметр будет задавать столбец для сортировки и направление сортировки.

Добавьте код для нового метода для *SchoolRepository.cs* файла:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

Изменение существующих параметров `GetDepartments` для вызова нового метода:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

В тестовом проекте, добавьте следующий новый метод *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

Если планируется создать модульные тесты, зависят от этого метода возврат отсортированного списка, необходимо отсортировать список перед возвращением. Вы не будет создавать тесты так, в этом учебнике, этот метод может возвращать только неупорядоченного списка отделов.

В *SchoolBL.cs* файл, добавьте следующий новый метод в классе логики:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

Этот код передает параметр сортировки методу репозитория.

Запустите *Departments.aspx* страницы.

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)

Теперь можно щелкнуть любой заголовок столбца для сортировки по этому столбцу. Если столбец уже отсортированы, щелкнув заголовок обращает направление сортировки.

## <a name="adding-a-search-box"></a>Добавление поля поиска

В этом разделе предстоит добавить текстовое поле поиска, связывание его с `ObjectDataSource` управлять с помощью параметра управления и добавить метод в классе логику для поддержки фильтрации.

Откройте *Departments.aspx* и добавьте следующую разметку между заголовком и первым `ObjectDataSource` управления:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

В `ObjectDataSource` управления с именем `DepartmentsObjectDataSource`, выполните следующие действия:

- Добавить `SelectParameters` элемент для параметра с именем `nameSearchString` , возвращает значение, введенное в `SearchTextBox` элемента управления.
- Изменение `SelectMethod` значение для атрибута `GetDepartmentsByName`. (Вы создадите этот метод более поздней версии.)

Разметка для `ObjectDataSource` теперь элемент управления похож на приведенный ниже:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

В *ISchoolRepository.cs*, добавьте `GetDepartmentsByName` метода, принимающего оба `sortExpression` и `nameSearchString` параметры:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

В *SchoolRepository.cs*, добавьте следующий новый метод:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

Этот код использует `Where` метод, чтобы выбрать элементы, которые содержат строку поиска. Если строка поиска пуста, будет выбрано все записи. Обратите внимание, что при указании вызовы методов вместе в одной инструкции следующим образом (`Include`, затем `OrderBy`, затем `Where`), `Where` метод всегда должен быть последним.

Изменить существующий `GetDepartments` метода, принимающего `sortExpression` параметров для вызова нового метода:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

В *MockSchoolRepository.cs* в тестовом проекте, добавьте следующий новый метод:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

В *SchoolBL.cs*, добавьте следующий новый метод:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

Запустите *Departments.aspx* страницы и введите строку поиска, чтобы убедиться в том, что работает логику выбора. Оставьте поле пустым и повторите поиск, чтобы убедиться в том, что возвращаются все записи.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)

## <a name="adding-a-details-column-for-each-grid-row"></a>Добавление столбца сведений для каждой строки сетки

Теперь можно увидеть все курсов для каждого отдела, отображаемых в правую ячейку сетки. Для этого потребуется использовать вложенную `GridView` управления и привязки его к данным из `Courses` свойство навигации `Department` сущности.

Откройте *Departments.aspx* и разметки `GridView` управления, указать обработчик для `RowDataBound` события. Разметку для открывающего тега элемента управления теперь приведенной в следующем примере.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

Добавьте новый `TemplateField` элемента после `Administrator` поле шаблона:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

Эта разметка создает вложенную `GridView` управления, отображающий номер и название списка курсов. Не указывает источник данных, так как вы будете databind его в коде в `RowDataBound` обработчика.

Откройте *Departments.aspx.cs* и добавьте следующий обработчик `RowDataBound` событий:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

Этот код получает `Department` преобразует сущности из аргументов события `Courses` свойство навигации для `List` коллекции, а также осуществляет вложенного `GridView` в коллекцию.

Откройте *SchoolRepository.cs* файла и укажите упреждающую для `Courses` свойство навигации, вызвав `Include` метод в запросе объекта, создаваемого в `GetDepartmentsByName` метод. `return` Инструкции в `GetDepartmentsByName` метод теперь приведенной в следующем примере.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

Запустите страницу. Помимо фильтрации и сортировки возможностей, который был добавлен ранее элемента управления GridView теперь отображаются сведения о вложенных курса для каждого отдела.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)

Введение в сценарии сортировки, фильтрации и подробный завершена. В следующем уроке вы увидите, как обрабатывать параллелизма.

> [!div class="step-by-step"]
> [Назад](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [Вперед](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
