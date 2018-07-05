---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
title: Создание контроллера (C#) | Документация Майкрософт
author: StephenWalther
description: В этом руководстве Стивен Вальтер демонстрирует, как можно добавить контроллер в приложении ASP.NET MVC.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 719d50d4-2305-454c-98b4-bae64937c48f
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
msc.type: authoredcontent
ms.openlocfilehash: 4477418be589af7b9127c7b7373a128a6867bf11
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399442"
---
<a name="creating-a-controller-c"></a>Создание контроллера (C#)
====================
по [Стивен Вальтер](https://github.com/StephenWalther)

> В этом руководстве Стивен Вальтер демонстрирует, как можно добавить контроллер в приложении ASP.NET MVC.


Чтобы объяснить, как можно создать новый ASP.NET MVC контроллеры является целью данного учебника. Вы научитесь создавать контроллеры, как с помощью команды меню добавления контроллера Visual Studio, так и вручную создав файл класса.

### <a name="using-the-add-controller-menu-option"></a>С помощью контроллера добавить команду меню

Самый простой способ создать новый контроллер — щелкните правой кнопкой мыши папку Controllers в окне обозревателя решений Visual Studio и выберите **Add, контроллера** пункт меню (см. рис. 1). Если выбрать этот пункт меню открывает **Добавление контроллера** диалоговое окно (см. рис. 2).


[![В диалоговом окне нового проекта](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)

**Рис 01**: Добавление нового контроллера ([Просмотр полноразмерного изображения](creating-a-controller-cs/_static/image2.png))


[![В диалоговом окне нового проекта](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)

**Рис. 02**: диалоговое окно добавления контроллера ([Просмотр полноразмерного изображения](creating-a-controller-cs/_static/image4.png))


Обратите внимание, что первая часть имени контроллера будет выделен в **Добавление контроллера** диалоговое окно. Имя каждого контроллера должно заканчиваться суффиксом *контроллера*. Например, можно создать контроллер с именем *ProductController* , но не контроллер с именем *продукта*.


Если вы создаете контроллер, который отсутствует *контроллера* суффикса, то вы не сможете заставить контроллер. Не сделать, — я потрачены впустую бесконечные часы мою жизнь после совершения этой ошибки.


**В листинге 1 - Controllers\ProductController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample1.cs)]

Всегда следует создавать контроллеры в папку "контроллеры". В противном случае будет нарушения правил ASP.NET MVC и других разработчиков будет сложнее основные сведения о приложении.

### <a name="scaffolding-action-methods"></a>Методы действия формирования шаблонов

При создании контроллера, у вас есть возможность создать методы действий создания, обновления и сведения автоматически (см. рис. 3). Если выбран этот параметр, создается класс контроллера в листинге 2.


[![Автоматическое создание методов действий](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)

**Рис 03**: автоматическое создание методов действий ([Просмотр полноразмерного изображения](creating-a-controller-cs/_static/image6.png))


**В листинге 2 - Controllers\CustomerController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample2.cs)]

Эти созданные методы являются методы-заглушки. Фактическая логика для создания, обновления и с подробными сведениями для клиента самостоятельно, необходимо добавить. Но методы-заглушки дают хороший отправной точки.

### <a name="creating-a-controller-class"></a>Создание класса контроллера

Контроллер ASP.NET MVC — это класс. При желании можно игнорировать удобный формирования шаблонов контроллера Visual Studio и создать класс контроллера вручную. Выполните следующие действия.

1. Щелкните правой кнопкой мыши папку Controllers и выберите пункт меню **добавить, новый элемент** и выберите **класс** шаблона (см. рис. 4).
2. Назовите новый класс PersonController.cs и нажмите кнопку **добавить** кнопки.
3. Измените полученный файл, класс наследуется от базового класса System.Web.Mvc.Controller класса (см. Листинг 3).


[![Создание нового класса](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)

**Рис. 04**: Создание нового класса ([Просмотр полноразмерного изображения](creating-a-controller-cs/_static/image8.png))


**Листинг 3 - Controllers\PersonController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample3.cs)]

Контроллер в листинге 3 предоставляет одно действие с именем Index(), которое возвращает строку «Hello World!». Вы можете вызвать это действие контроллера, для запуска приложения и запрос URL-адрес следующего вида:

`http://localhost:40071/Person`

> [!NOTE]
> 
> ASP.NET Development Server использует случайный номер порта (например, 40071). При вводе URL-адрес для вызова контроллера, необходимо указать номер порта справа. Можно определить номер порта, наведя указатель мыши на значок для сервера разработки ASP.NET в области уведомлений Windows (правом верхнем углу экрана).
> 
> [!div class="step-by-step"]
> [Назад](adding-dynamic-content-to-a-cached-page-cs.md)
> [Вперед](creating-an-action-cs.md)
