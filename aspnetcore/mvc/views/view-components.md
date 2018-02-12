---
title: "Компоненты представлений"
author: rick-anderson
description: "Компоненты представлений предназначены для случаев, когда имеется многоразовая логика отрисовки."
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-components
ms.openlocfilehash: 27e77b8fa032c2b5be753a27db748b7499e27105
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="view-components"></a>Компоненты представлений

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="introducing-view-components"></a>Общие сведения о компонентах представлений

Недавно появившиеся в ASP.NET Core MVC компоненты представлений похожи на частичные представления, но при этом гораздо функциональнее. Компоненты представлений не используют привязку модели и зависят только от данных, предоставляемых при их вызове. Компонент представлений:

* Выполняет отрисовку фрагмента, а не всего отклика.
* Включает те же преимущества разделения функций и тестирования, которые доступны между контроллером и представлением.
* Может иметь параметры и бизнес-логику.
* Обычно вызывается со страницы макета.

Компоненты представлений предназначены для случаев, когда имеется многоразовая логика отрисовки, которая слишком сложна для частичного представления, например:

* Динамические меню навигации
* Облако тегов (где выполняется запрос к базе данных)
* Панель входа
* Корзина для покупок
* Недавно опубликованные статьи
* Содержимое боковой панели в типичном блоге
* Панель входа, которая должна отображаться на каждой странице и содержит ссылки для выхода или входа в зависимости от состояния входа пользователя

Компонент представления состоит из двух частей: класса (обычно производного от [ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) и возвращаемого результата (обычно это представление). Как и контроллеры, компонент представления может быть объектом POCO, но большинству разработчиков потребуются преимущества методов и свойств, доступные при наследовании от `ViewComponent`.

## <a name="creating-a-view-component"></a>Создание компонента представления

Этот раздел содержит общие требования к созданию компонента представления. Далее в этой статье мы подробно рассмотрим каждый шаг и создадим компонент представления.

### <a name="the-view-component-class"></a>Класс компонентов представлений

Класс компонентов представлений можно создать одним из следующих способов:

* Наследование от *ViewComponent*
* Декорирование класса с помощью атрибута `[ViewComponent]` или наследование от класса с помощью атрибута `[ViewComponent]`
* Создание класса, где имя заканчивается суффиксом *ViewComponent*

Как и контроллеры, компоненты представлений должны быть открытыми, невложенными и неабстрактными классами. Имя компонента представления — это имя класса с удаленным суффиксом "ViewComponent". Его можно также можно задать явно с помощью свойства `ViewComponentAttribute.Name`.

Класс компонентов представлений:

* Полностью поддерживает [внедрение зависимостей](../../fundamentals/dependency-injection.md) в конструкторе.

* Не участвует в жизненном цикле контроллера, поэтому в компоненте представления невозможно использовать [фильтры](../controllers/filters.md).

### <a name="view-component-methods"></a>Методы компонентов представлений

Компонент представления задает свою логику в методе `InvokeAsync`, возвращающем `IViewComponentResult`. Параметры берутся непосредственно из вызова компонента представления, а не из привязки модели. Компонент представления никогда не обрабатывает запрос напрямую. Как правило, компонент представления инициализирует модель и передает ее в представление, вызвав метод `View`. Если кратко, методы компонентов представлений:

* Определяют метод `InvokeAsync`, возвращающий `IViewComponentResult`.
* Обычно инициализируют модель и передают ее в представление, вызвав метод `ViewComponent` `View`.
* Получают параметры из вызывающего метода, а не HTTP, так как привязка модели отсутствует.
* Недоступны непосредственно в качестве конечной точки HTTP, вызываются из кода (обычно в представлении). Компонент представления никогда не обрабатывает запрос.
* Перегружаются по сигнатуре, а не сведениям из текущего HTTP-запроса.

### <a name="view-search-path"></a>Путь поиска представления

Среда выполнения ищет представление по следующим путям:

   * Views/\<имя_контроллера>/Components/\<имя_компонента_представления>/\<имя_представления>
   * Views/Shared/Components/\<имя_компонента_представления>/\<имя_представления>

По умолчанию для компонента представления используется имя *Default*, то есть файл представления обычно называется *Default.cshtml*. При создании результата компонента представления или при вызове метода `View` можно указать другое имя представления.

Мы рекомендуем назвать файл представления *Default.cshtml* и использовать путь *Views/Shared/Components/\<имя_компонента_представления>/\<имя_представления>*. Компонент представления `PriorityList` в этом примере использует представление компонента представления *Views/Shared/Components/PriorityList/Default.cshtml*.

## <a name="invoking-a-view-component"></a>Вызов компонента представления

Чтобы использовать компонент представления, вызовите следующий код внутри представления:

```cshtml
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

Эти параметры передаются в метод `InvokeAsync`. Разрабатываемый в этой статье компонент представления `PriorityList` вызывается из файла представления *Views/Todo/Index.cshtml*. Следующий код вызывает метод `InvokeAsync` с двумя параметрами:

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a>Вызов компонента представления в качестве вспомогательной функции тегов

Для ASP.NET Core 1.1 и более поздних версий можно вызвать компонент представления в качестве [вспомогательной функции тегов](xref:mvc/views/tag-helpers/intro):

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

Параметры методов и классов, указанные в стиле Pascal, для вспомогательных функций тегов преобразуются в [кебаб-стиль нижнего регистра](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101). Вспомогательная функция тегов для вызова компонента представления использует элемент `<vc></vc>`. Компонент представления задан следующим образом:

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

Примечание. Для использования компонента представления в качестве вспомогательной функции тегов нужно зарегистрировать сборку, содержащую этот компонент представления, с помощью директивы `@addTagHelper`. Например, если компонент представления находится в сборке с именем "MyWebApp", добавьте в файл `_ViewImports.cshtml` следующую директиву:

```cshtml
@addTagHelper *, MyWebApp
```

Зарегистрировать компонент представления в качестве вспомогательной функции тегов можно в любом файле, который ссылается на этот компонент представления. Раздел [Управление областью вспомогательной функции тегов](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) содержит дополнительные сведения о том, как зарегистрировать вспомогательные функции тегов.

Метод `InvokeAsync`, используемый в этом учебнике:

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

В разметке вспомогательной функции тегов:

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

В предыдущем примере компонент представления `PriorityList` становится `priority-list`. Параметры передаются в компонент представления как атрибуты в кебаб-стиле нижнего регистра.

### <a name="invoking-a-view-component-directly-from-a-controller"></a>Вызов компонента представления непосредственно из контроллера

Компоненты представлений обычно вызываются из представления, но их можно вызывать непосредственно из метода контроллера. Если компоненты представлений не определяют конечные точки, например контроллеры, можно легко реализовать действие контроллера, возвращающее содержимое `ViewComponentResult`.

В этом примере компонент представления вызывается непосредственно из контроллера:

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a>Пошаговое руководство. Создание простого компонента представления

[Скачайте](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) начальный код и выполните его сборку и тестирование. Это простой проект с контроллером `Todo`, который отображает список элементов *дел*.

![Список дел](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a>Добавление класса ViewComponent

Создайте папку *ViewComponents* и добавьте следующий класс `PriorityListViewComponent`:

[!code-csharp[Main](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

Примечания к коду:

* Классы компонентов представлений могут находиться в **любой** папке проекта.
* Так как имя класса PriorityList**ViewComponent** заканчивается суффиксом **ViewComponent**, среда выполнения использует строку "PriorityList" при ссылке на компонент класса из представления. Я подробнее расскажу об этом чуть позже.
* Атрибут `[ViewComponent]` может изменить имя, используемое для ссылки на компонент представления. Например, мы могли назвать класс `XYZ` и применить атрибут `ViewComponent`:

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* Приведенный выше атрибут `[ViewComponent]` предписывает средству выбора компонентов представлений использовать имя `PriorityList` при поиске представлений, связанных с данным компонентом, и строку "PriorityList" при ссылке на компонент класса из представления. Я подробнее расскажу об этом чуть позже.
* Компонент использует [внедрение зависимостей](../../fundamentals/dependency-injection.md) для предоставления контекста данных.
* `InvokeAsync` предоставляет метод, который возможно вызывать из представления и который может принять произвольное число аргументов.
* Метод `InvokeAsync` возвращает набор элементов `ToDo`, удовлетворяющих параметрам `isDone` и `maxPriority`.

### <a name="create-the-view-component-razor-view"></a>Создание представления Razor для компонента представления

* Создайте папку *Views/Shared/Components*. Она **должна**  называться *Components*.

* Создайте папку *Views/Shared/Components/PriorityList*. Ее имя должно соответствовать имени класса представлений компонентов или имени класса без суффикса (если мы следовали соглашению и использовали суффикс *ViewComponent* в имени класса). Если вы использовали атрибут `ViewComponent`, имя класса должно соответствовать его обозначению.

* Создайте представление Razor *Views/Shared/Components/PriorityList/Default.cshtml*: [!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]
    
   Представление Razor принимает список `TodoItem` и отображает их. Если метод `InvokeAsync` компонента представления не передает имя представления (как в нашем примере), по соглашению используется имя *Default*. Далее в этом учебнике я покажу, как передать имя представления. Чтобы переопределить стиль по умолчанию для определенного контроллера, добавьте представление в папку соответствующего контроллера (например, *Views/Todo/Components/PriorityList/Default.cshtml)*.
    
    Если компонент представления связан с конкретным контроллером, его можно добавить в папку этого контроллера (*Views/Todo/Components/PriorityList/Default.cshtml*).

* Добавьте `div`, содержащий вызов компонента списка приоритетов, в конец файла *Views/Todo/index.cshtml*:

    [!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

`@await Component.InvokeAsync` в разметке показывает синтаксис для вызова компонентов представлений. Первым аргументом является имя компонента, который требуется вызвать. Последующие параметры передаются в компонент. `InvokeAsync` может занять произвольное число аргументов.

Проверьте работу приложения. Следующий рисунок показывает список дел и элементы с приоритетом:

![список дел и элементы с приоритетом](view-components/_static/pi.png)

Кроме того, компонент представления можно вызвать непосредственно из контроллера:

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![элементы с приоритетом из действия IndexVC](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a>Указание имени представления

Сложному представлению компонента в некоторых условиях может потребоваться указать нестандартное представление. Ниже показано, как указать представление "PVC" из метода `InvokeAsync`. Обновите метод `InvokeAsync` в классе `PriorityListViewComponent`.

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

Скопируйте файл *Views/Shared/Components/PriorityList/Default.cshtml* в представление *Views/Shared/Components/PriorityList/PVC.cshtml*. Добавьте заголовок, чтобы указать используемое представление PVC.

[!code-cshtml[Main](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

Измените *Views/TodoList/Index.cshtml*:

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

Запустите приложение и проверьте представление PVC.

![Компонент представления с приоритетом](view-components/_static/pvc.png)

Если представление PVC не отображается, убедитесь, что вы вызываете компонент представления с приоритетом 4 или выше.

### <a name="examine-the-view-path"></a>Проверка пути к представлению

* Задайте для параметра приоритета значение 3 или меньше, чтобы представление с приоритетом не возвращалось.
* Временно переименуйте *Views/Todo/Components/PriorityList/Default.cshtml* в *1Default.cshtml*.
* Проверьте приложение, при этом происходит следующая ошибка:

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' wasn't found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* Скопируйте *Views/Todo/Components/PriorityList/1Default.cshtml* в *Views/Shared/Components/PriorityList/Default.cshtml*.
* Добавьте разметку в представление компонента представления дел *Shared*, чтобы указать, что это представление из папки *Shared*.
* Протестируйте представление компонента **Shared**.

![Выходные данные дел с представлением компонента Shared](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a>Предотвращение появления неочевидных строк

Чтобы обеспечить безопасность во время компиляции, можно заменить жестко заданное имя компонента представления на имя класса. Создайте компонент представления без суффикса "ViewComponent":

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

Добавьте инструкцию `using` в файл представления Razor и используйте оператор `nameof`:

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,33-)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Внедрение зависимостей в представления](xref:mvc/views/dependency-injection)
