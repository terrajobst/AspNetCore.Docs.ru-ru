---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
title: Создание модульных тестов для приложений ASP.NET MVC (C#) | Документы Microsoft
author: StephenWalther
description: Описание способов создания модульных тестов для действий контроллера. Стивен Вальтер в этом учебнике показано, как проверка действия контроллера возвращает parti...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: d3a270b9-d7b1-47f2-8775-fc3beb518b5c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
msc.type: authoredcontent
ms.openlocfilehash: ccd9a1b3aee8379c23c01c5eb7f756a786f6359d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-c"></a>Создание модульных тестов для приложений ASP.NET MVC (C#)
====================
по [Стивен Вальтер](https://github.com/StephenWalther)

[Скачать PDF](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_CS.pdf)

> Описание способов создания модульных тестов для действий контроллера. Стивен Вальтер в этом учебнике показано, как проверка действия контроллера возвращает конкретное представление, возвращает определенного набора данных или другого типа результата действия.


Целью данного учебника является демонстрация как приложения можно создавать модульные тесты для контроллеров в your ASP.NET MVC. Рассматриваются способы построения три разных типа модульных тестов. Вы узнаете, как тестирование представления, возвращенных действия контроллера, способа представления данных, возвращаемых функцией действия контроллера тестирования и проверка ли одного контроллера действия перенаправит вас на второе действие контроллера.

## <a name="creating-the-controller-under-test"></a>Создание контроллера теста

Давайте начнем с создания контроллер, к которому мы собираемся тестирования. Контроллер с именем `ProductController`, содержащихся в листинге 1.

**Листинг 1. `ProductController.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample1.cs)]

`ProductController` Содержит два метода действия с именем `Index()` и `Details()`. Оба метода действия возвращают представления. Обратите внимание, что `Details()` действие принимает параметр с именем идентификатора.

## <a name="testing-the-view-returned-by-a-controller"></a>Тестирование представления возвращенных контроллера

Предположим, что мы хотим проверить ли `ProductController` возвращает правильное представление. Мы хотим, чтобы убедиться, что при `ProductController.Details()` вызове действия возвращается в представлении сведений. Тестовый класс в списке 2 содержит модульный тест для проверки представления, возвращенных `ProductController.Details()` действие.

**Листинг 2. `ProductControllerTest.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample2.cs)]

Этот класс в списке 2 включает метод теста, с именем `TestDetailsView()`. Этот метод содержит три строки кода. В первой строке кода создается новый экземпляр `ProductController` класса. Вторая строка кода вызывает контроллера `Details()` метода действия. Наконец, последняя строка кода проверки того, является ли представление возвращенных `Details()` действии подробного представления.

`ViewResult.ViewName` Свойство представляет имя представления, возвращенный контроллер. Важное предупреждение о тестировании это свойство. Что контроллер можно получить двумя способами. Контроллер может явным образом получить следующим образом:

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample3.cs)]

Кроме того имя представления может выводиться из имя действия контроллера, следующим образом:

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample4.cs)]

Это действие контроллера также возвращает представление с именем `Details`. Имя представления, однако выводится из имени действия. Если вы хотите проверить имя представления, вы должны явно возвращается имя представления из действия контроллера.

Можно выполнить модульный тест в списке 2, либо введя сочетание клавиш **Ctrl-R, А** или щелкнув **выполнить все тесты в решении** кнопку (см. рис. 1). Если тест выполнен, отобразится в окне результатов теста на рис. 2.


[![Выполнить все тесты в решении](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image1.png)

**На рисунке 01**: выполнить все тесты в решении ([Просмотр полноразмерное изображение](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image3.png))


[![Выполнена успешно!](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image4.png)

**На рисунке 02**: успех! ([Просмотр полноразмерное изображение](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image6.png))


## <a name="testing-the-view-data-returned-by-a-controller"></a>Тестирование представления данных возвращенных контроллера

Контроллер MVC передает данные в представлении с помощью так называемых *`View Data`*. Например, предположим, вы хотите отобразить подробные сведения по определенному продукту при вызове `ProductController Details()` действие. В этом случае можно создать экземпляр `Product` класса (определенный в модели) и передать экземпляр для `Details` представление, используя преимущества `View Data`.

Измененный `ProductController` в списке 3 включает в себя обновленные `Details()` действие, которое возвращает продукт.

**Листинг 3. `ProductController.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample5.cs)]

Во-первых, `Details()` действие создает новый экземпляр `Product` класс, представляющий переносного компьютера. Далее, экземпляр `Product` класса передается в качестве второго параметра `View()` метод.

Можно создать модульные тесты для проверки, является ли ожидаемый набор данных содержатся в представлении данных. Платформа модульного тестирования в тестах со списком 4 ли продукта, представляющий переносной компьютер возвращается при вызове `ProductController Details()` метода действия.

**Листинг 4. `ProductControllerTest.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample6.cs)]

В листинге 4 `TestDetailsView()` метод проверяет данные представления, возвращаемые при вызове `Details()` метода. `ViewData` Предоставляется как свойство `ViewResult` возвращенные вызовом `Details()` метод. `ViewData.Model` Свойство содержит продукта, передаются в представление. Тест просто проверяет, имеет ли продукт, содержащиеся в представлении данных имя переносного компьютера.

## <a name="testing-the-action-result-returned-by-a-controller"></a>Тестирование результата действия возвращается с помощью контроллера

Более сложные действия контроллера могут возвращать различные типы результатов действий в зависимости от значения параметров, переданных для действия контроллера. Действие контроллера может возвращать различные типы результатов действий, в том числе `ViewResult`, `RedirectToRouteResult`, или `JsonResult`.

Например, измененные `Details()` действие в листинге 5 возвращает `Details` просмотра при передаче соответствующим продуктом идентификатор действия. Если передается недопустимый продукта идентификатор со значением идентификатора — меньше 1, то вы будете перенаправлены `Index()` действие.

**Листинг 5. `ProductController.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample7.cs)]

Можно проверить поведение `Details()` действие с модульным тестом в 6 со списком. Модульный тест в список 6 проверяет, что вы будете перенаправлены на `Index` просмотра при передаче идентификатора со значением -1 `Details()` метод.

**Перечисление 6. `ProductControllerTest.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample8.cs)]

При вызове `RedirectToAction()` метод действия контроллера, действия контроллера возвращает `RedirectToRouteResult`. Тест проверяет ли `RedirectToRouteResult` будет перенаправлять пользователя на действие контроллера, с именем `Index`.

## <a name="summary"></a>Сводка

В этом учебнике вы узнали, как для создания модульных тестов для действий контроллеров MVC. Во-первых вы узнали, как проверить, возвращается ли правильное представление с помощью действия контроллера. Вы узнали, как использовать `ViewResult.ViewName` свойство для проверки имени представления.

Далее мы рассмотрели, как можно проверить содержимое `View Data`. Вы узнали, как проверить, возвращаются ли подходящее в `View Data` после вызова метода действия контроллера.

Наконец мы рассмотрели, как можно проверить ли различные типы результатов действий возвращаются из действия контроллера. Вы узнали, как проверить ли контроллер возвращает `ViewResult` или `RedirectToRouteResult`.

> [!div class="step-by-step"]
> [Вперед](creating-unit-tests-for-asp-net-mvc-applications-vb.md)
