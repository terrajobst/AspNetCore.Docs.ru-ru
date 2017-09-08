---
title: "Внедрение зависимостей в представления"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 80fb9e43-e4db-4af2-b2a8-e1364a712f69
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/dependency-injection
ms.openlocfilehash: 05d64858dd70b45a1e2bb90a86ab3cbdc85264b1
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="dependency-injection-into-views"></a>Внедрение зависимостей в представления

По [Стив Смит](http://ardalis.com)

Поддерживает ASP.NET Core [внедрения зависимостей](xref:fundamentals/dependency-injection) в представления. Это может быть полезно для представления службы, такие как локализации или данные, необходимые только для заполнения представления элементов. Можно попытаться ведение [Разделение областей ответственности](http://deviq.com/separation-of-concerns) между контроллерами и представлениями. Большая часть данных, которые отображаются в представлениях необходимо передать из контроллера.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample)

## <a name="a-simple-example"></a>Простой пример

Служба может вводить в представлении с помощью `@inject` директивы. Можно представить `@inject` как добавление свойства в представлении, а также заполнение свойства с помощью DI.

Синтаксис для `@inject`:`@inject <type> <name>`

Пример `@inject` в действии:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

Это представление отображает список `ToDoItem` экземпляров, а также сводку Общая статистика. Сводка заполняется из внедренного `StatisticsService`. Эта служба регистрируется для внедрения зависимости в `ConfigureServices` в *файла Startup.cs*:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

`StatisticsService` Выполняет некоторые вычисления на наборе `ToDoItem` экземпляров, которые он обращается к через репозитория:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,26)]

Репозитории примеров использует находящуюся в памяти коллекцию. Реализация, показанный выше (который работает на всех данных в памяти) не рекомендуется для больших, удаленный доступ к которым осуществляется наборов данных.

Образец отображаются данные из модели, привязанный к представлению и службы, вставленными в представлении:

![Для просмотра списка число элементов, выполнить элементов, средний приоритет и список задач с их уровни приоритета и логические значения, указывающее, завершения.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a>Заполнение поиск данных

Внедрение представления можно использовать для заполнения параметров в элементы пользовательского интерфейса, например раскрывающиеся списки. Рассмотрим форму профиль пользователя, содержащий параметры для указания пол, состояние и другие параметры. Отрисовка формы с помощью стандартный подход MVC потребует контроллера для запроса службы доступа к данным для каждого из этих наборов параметров и затем заполнение модели или `ViewBag` каждый набор параметров для привязки.

Альтернативный подход внедряет службы непосредственно в представлении, чтобы получить параметры. Это сводит к минимуму объем кода, необходимого для контроллера, переместив этой логики конструкций элемента представления в самого представления. Действие контроллера для отображения в форме редактирования профиля нужно только передать экземпляр профиля в форме:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

HTML-форму, используемую для обновления этих параметров включает в себя раскрывающиеся списки для трех свойств:

![Обновите представление профиля с формой ввода имя, пол, состояние и любимого цвета.](dependency-injection/_static/updateprofile.png)

Эти списки заполняются службы, который был вставлен в представлении:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

`ProfileOptionsService` — Это служба уровня пользовательского интерфейса обеспечивает только данные, необходимые для этой формы:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

>[!TIP]
> Не забудьте регистрации будет запрашивать через внедрение зависимостей в типы `ConfigureServices` метод в *файла Startup.cs*.

## <a name="overriding-services"></a>Переопределение служб

Помимо добавления новых служб, этот метод можно использовать для переопределения добавленного ранее службы на странице. На следующем рисунке показаны все доступные поля на странице, используемый в первом примере:

![Контекстные меню IntelliSense на типизированный Html, компонент, StatsService и URL-адрес поля со списком символа @](dependency-injection/_static/razor-fields.png)

Как видите, поля по умолчанию включают `Html`, `Component`, и `Url` (а также `StatsService` , мы введенного). Если для экземпляра необходимо заменить на собственный вспомогательных методов HTML по умолчанию, это можно легко сделать с помощью `@inject`:

[!code-html[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

Если вы хотите расширить существующие службы, можно просто использовать этот метод при наследовании или упаковки существующую вашу реализацию.

## <a name="see-also"></a>См. также

* Блог Timms Simon: [начало поиск данных в представлении](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)
