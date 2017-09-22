---
title: "Просмотр компонентов"
author: rick-anderson
description: "Просмотр компонентов предполагается, что в любом у логики отрисовки для повторного использования."
keywords: "ASP.NET Core, просмотр компонентов частичного представления"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: ab4705b7-59d7-4f31-bc97-ea7f292fe926
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-components
ms.openlocfilehash: bb8a889c66ec9ca0c0aec7b4a4184d7c19858d78
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/22/2017
---
# <a name="view-components"></a>Просмотр компонентов

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample)

## <a name="introducing-view-components"></a>Знакомство с приложением Просмотр компонентов

Новое в ASP.NET Core MVC, просмотр компонентов похожи на частичного представления, но они являются более мощными. Просмотр компонентов не использовать привязки модели и только зависят от данных, которые предоставляют при вызове в него. Компонент представления:

* Подготавливает к просмотру фрагмента, а не весь ответ
* Включает в себя же разделения задач и преимущества тестирования найден между контроллером и представлением
* Может иметь параметры и бизнес-логики
* Обычно вызывается из макета страницы

Просмотр компонентов предназначены в любом, что у вас есть логики отрисовки для повторного использования, слишком сложен для частичного представления, такие как:

* Меню динамических переходов
* Облако тегов (где запрос к базе данных)
* Панель имени входа
* Корзина для покупок
* Недавно опубликованные статьи
* Боковая панель содержимого в типичных блога
* Имя входа панель, в которой будут отображены на каждой странице и Показать ссылки выйти из системы или журнала, в зависимости от журнала в состояние пользователя

Компонент представления состоит из двух частей: класс (обычно получается из [ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) и в результате возвращается (обычно представление). Подобно контроллеров, представление компонента может быть POCO, но большинство разработчиков будет нужно воспользоваться преимуществами методов и свойств, доступных путем наследования от `ViewComponent`.

## <a name="creating-a-view-component"></a>Создание представления компонента

Этот раздел содержит основные требования, чтобы создать представление компонента. Далее в этой статье мы будем проверьте каждый шаг подробно и создать представление компонента.

### <a name="the-view-component-class"></a>Класс компонента представления

Класс компонента представления могут создаваться с помощью любого из следующих:

* Наследование от *ViewComponent*
* Декорирования класс с `[ViewComponent]` атрибута или производные от класса с `[ViewComponent]` атрибута
* Создание класса, где имя должно заканчиваться суффиксом *ViewComponent*

Как и контроллеров Просмотр компонентов должна быть public, не являющегося вложенным и неабстрактного классы. Имя компонента представление — это имя класса с суффиксом «ViewComponent» удалены. Его можно также явно указать с помощью `ViewComponentAttribute.Name` свойство.

Класс компонента представления:

* Полностью поддерживает конструктор [внедрения зависимости](../../fundamentals/dependency-injection.md)

* Не участвуют в жизненном цикле контроллер, то есть нельзя использовать [фильтры](../controllers/filters.md) в компоненте представления

### <a name="view-component-methods"></a>Представление методов компонента

Компонент представления задает его логику в `InvokeAsync` метод, возвращающий `IViewComponentResult`. Параметры берутся непосредственно из вызова компонента представления, не из привязки модели. Никогда не компонент представления непосредственно обрабатывает запрос. Как правило, представление компонента инициализирует модель и передает его в представление, вызвав `View` метод. Таким образом просмотрите методов компонента:

* Определение `InvokeAsync` метод, возвращающий`IViewComponentResult`
* Обычно инициализирует модель и передает его в представление, вызвав `ViewComponent` `View` метод
* Параметры берутся из вызывающего метода, не HTTP, нет привязки модели
* Не доступны непосредственно как конечную точку HTTP, они вызываются из кода приложения (обычно в представление). Компонент представления никогда не обрабатывается запрос
* Перегружено подписи, а не все данные из текущего HTTP-запроса.

### <a name="view-search-path"></a>Путь к представлению поиска

Среда выполнения ищет в представлении в следующие пути:

   * Представления или\<controller_name > /Components/\<view_component_name > /\<view_name >
   * Представления, компонентов/Общие/\<view_component_name > /\<view_name >

Представление является именем по умолчанию для компонента представление *по умолчанию*, то есть файл представления обычно называться *Default.cshtml*. Можно указать другое имя представления, при создании компонента результат представления или при вызове `View` метода.

Мы рекомендуем, назовите файл представления *Default.cshtml* и использовать *компонентов/представления/Общие/\<view_component_name > /\<view_name >* пути. `PriorityList` Использует компонент представления, используемые в данном примере *Views/Shared/Components/PriorityList/Default.cshtml* для этого представления view.

## <a name="invoking-a-view-component"></a>Вызов компонента представления

Чтобы использовать компонент представления, следует вызвать следующий внутри представления:

```html
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

Параметры передаются `InvokeAsync` метод. `PriorityList` Из вызывается компонент представления, разработанные в статье *Views/Todo/Index.cshtml* файл представления. В следующих `InvokeAsync` метод вызывается с двумя параметрами:

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a>Вызов компонента представления как вспомогательный тега

Для ASP.NET Core 1.1 и более поздних версиях можно вызвать компонент представления как [вспомогательный тег](xref:mvc/views/tag-helpers/intro):

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

Преобразуется в стиле Pascal класса и метода параметры для вспомогательных функций тегов их [нижний регистр kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101). Вспомогательный объект тег для вызова компонента представление использует `<vc></vc>` элемента. Представления компонента указывается следующим образом:

```html
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

Примечание: Для использования в качестве тега вспомогательный компонент представления, необходимо зарегистрировать сборку, содержащую компонент представления с помощью `@addTagHelper` директивы. Например, если представление компонент находится в сборке с именем «MyWebApp», добавьте следующую директиву для `_ViewImports.cshtml` файла:

```csharp
@addTagHelper *, MyWebApp
```

Компонент представления можно зарегистрировать как вспомогательный тег в любой файл, который ссылается на компонент представления. В разделе [управление область вспомогательный тега](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) Дополнительные сведения о том, как зарегистрировать вспомогательных функций тегов.

`InvokeAsync` Метод, используемый в этом учебнике:

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

В разметке вспомогательный тег:

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

В примере выше `PriorityList` становится компонент представления `priority-list`. Параметры в компонент представления, передаются как атрибуты в нижний регистр kebab.

### <a name="invoking-a-view-component-directly-from-a-controller"></a>Вызов компонента представление непосредственно из контроллера

Просмотр компонентов обычно вызываются из представления, но их можно вызывать непосредственно из метода контроллера. Хотя Просмотр компонентов следует определять конечные точки, например контроллеров, можно легко реализовать действия контроллера, который возвращает содержимое `ViewComponentResult`.

В этом примере компонент представления вызывается непосредственно из контроллера:

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a>Пошаговое руководство: Создание простого представления компонента

[Загрузить](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), построения и тестирования начальный код. Это простой проект с `Todo` контроллер, который отображает список *Todo* элементов.

![Список ToDos](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a>Добавьте класс ViewComponent

Создание *ViewComponents* папки и добавьте следующее `PriorityListViewComponent` класса:

[!code-csharp[Main](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

Примечания к коду.

* Представление классов компонентов может содержаться в **любой** папку в проекте.
* Так как класс именем PriorityList**ViewComponent** заканчивается суффиксом **ViewComponent**, среда выполнения будет использовать строку «PriorityList» при ссылке на класс компонента из представления. Я объясню, более подробно далее.
* `[ViewComponent]` Атрибута можно изменить имя, используемое для ссылки на компонент представления. Например, может с именем класса `XYZ` и применяется `ViewComponent` атрибута:

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* `[ViewComponent]` Атрибут выше сообщает Выбор компонентов представления для использования имени `PriorityList` при поиске представлений, связанных с компонентом и строка «PriorityList» для ссылки на класс компонента из представления. Я объясню, более подробно далее.
* Компонент использует [внедрения зависимостей](../../fundamentals/dependency-injection.md) для предоставления контекста данных.
* `InvokeAsync`предоставляет метод, который может вызываться из представления, который может занять произвольное число аргументов.
* `InvokeAsync` Метод возвращает набор `ToDo` элементы, которые удовлетворяют `isDone` и `maxPriority` параметров.

### <a name="create-the-view-component-razor-view"></a>Создание представления view компонента Razor

* Создание *компонентовпредставления/Общие/* папки. Эта папка **должен** называться *компонентов*.

* Создание *представления/общие/компоненты или PriorityList* папки. Этот имя папки должно соответствовать имени класса представления компонента или имя класса, за вычетом суффикс (если мы следовали правилам и использовать *ViewComponent* суффикса в имени класса). Если вы использовали `ViewComponent` атрибут, имя класса пришлось бы соответствует обозначение атрибута.

* Создание *Views/Shared/Components/PriorityList/Default.cshtml* представления Razor:[!code-html[Main](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]
    
   Представления Razor принимает список `TodoItem` и отображает их. Если компонент представления `InvokeAsync` метод не передает имя представления (как в нашем примере) *по умолчанию* используется для имени представления по соглашению. Далее в этом учебнике я покажу, как передать имя представления. Чтобы переопределить стили по умолчанию для определенного контроллера, добавления представления к папке контроллера представления (например *Views/Todo/Components/PriorityList/Default.cshtml)*.
    
    Если компонент представления зависит от конкретного контроллера, его можно добавить в папку конкретного контроллера (*Views/Todo/Components/PriorityList/Default.cshtml*).

* Добавить `div` содержащем вызов компонент списка приоритет в нижнюю часть *Views/Todo/index.cshtml* файла:

    [!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

Разметка `@await Component.InvokeAsync` показан синтаксис для вызова компонентов представления. Первым аргументом является имя компонента, который нужно вызвать или вызова. Последующие аргументы передаются в компонент. `InvokeAsync`может занять произвольное число аргументов.

Тестирование приложения. На следующем рисунке показана ToDo list и элементы с приоритетом:

![элементы списка и приоритет TODO](view-components/_static/pi.png)

Компонент представления также можно вызывать непосредственно из контроллера:

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![элементы с приоритетом от IndexVC действия](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a>Указание имени представления

Сложные представление компонента может потребоваться указать представления не по умолчанию при некоторых условиях. Ниже показано, как указание представления «PVC» из `InvokeAsync` метод. Обновление `InvokeAsync` метод `PriorityListViewComponent` класса.

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

Копировать *Views/Shared/Components/PriorityList/Default.cshtml* файл, чтобы представление с именем *Views/Shared/Components/PriorityList/PVC.cshtml*. Добавьте заголовок, чтобы указать, что представление PVC используется.

[!code-html[Main](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

Обновление *Views/TodoList/Index.cshtml*:

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

Запустите приложение и проверить PVC представление.

![Компонент представления приоритет](view-components/_static/pvc.png)

Если представление PVC не отображается, убедитесь, что вы вызываете компонент представления с приоритетом 4 или более поздней версии.

### <a name="examine-the-view-path"></a>Проверьте путь к представлению

* Измените параметр priority три или менее, чтобы представление priority не возвращается.
* Временно переименуйте *Views/Todo/Components/PriorityList/Default.cshtml* для *1Default.cshtml*.
* Тестирование приложения, вы получите следующую ошибку:

   <!-- literal_block {"ids": [], "xml:space": "preserve"} -->

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' was not found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* Копировать *Views/Todo/Components/PriorityList/1Default.cshtml* для *Views/Shared/Components/PriorityList/Default.cshtml*.
* Добавить некоторую разметку для *Shared* Todo представление компонентное представление для указания представления — от *Shared* папки.
* Тест **Shared** компонентное представление.

![Вывод ToDo с Shared компонентное представление](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a>Предотвращение magic строк

Следует компилировать безопасности времени можно заменить имя компонента жестко представление с именем класса. Создайте представление компонента без суффикса «ViewComponent»:

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

Добавить `using` инструкцию, чтобы ваш Razor просмотра файла, а `nameof` оператор:

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,33-)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Внедрение зависимостей в представления](dependency-injection.md)
