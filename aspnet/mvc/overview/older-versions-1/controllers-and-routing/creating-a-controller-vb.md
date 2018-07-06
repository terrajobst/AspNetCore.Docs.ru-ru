---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
title: Создание контроллера (VB) | Документация Майкрософт
author: StephenWalther
description: В этом руководстве Стивен Вальтер демонстрирует, как можно добавить контроллер в приложении ASP.NET MVC.
ms.author: aspnetcontent
ms.date: 03/02/2009
ms.assetid: 204b7e86-f560-4611-8adb-785b33e777b9
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
msc.type: authoredcontent
ms.openlocfilehash: b24aeabc6fd4192f44f83e0c0b2150e87ef6f501
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833067"
---
<a name="creating-a-controller-vb"></a>Создание контроллера (VB)
====================
по [Стивен Вальтер](https://github.com/StephenWalther)

> В этом руководстве Стивен Вальтер демонстрирует, как можно добавить контроллер в приложении ASP.NET MVC.


Чтобы объяснить, как можно создать новый ASP.NET MVC контроллеры является целью данного учебника. Вы научитесь создавать контроллеры, как с помощью команды меню добавления контроллера Visual Studio, так и вручную создав файл класса.

### <a name="using-the-add-controller-menu-option"></a>С помощью контроллера добавить команду меню

Самый простой способ создать новый контроллер — щелкните правой кнопкой мыши папку Controllers в окне обозревателя решений Visual Studio и выберите **Add, контроллера** пункт меню (см. рис. 1). Если выбрать этот пункт меню открывает **Добавление контроллера** диалоговое окно (см. рис. 2).


[![В диалоговом окне нового проекта](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)

**Рис 01**: Добавление нового контроллера ([Просмотр полноразмерного изображения](creating-a-controller-vb/_static/image2.png))


[![В диалоговом окне нового проекта](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)

**Рис. 02**: диалоговое окно добавления контроллера ([Просмотр полноразмерного изображения](creating-a-controller-vb/_static/image4.png))


Обратите внимание, что первая часть имени контроллера будет выделен в **Добавление контроллера** диалоговое окно. Имя каждого контроллера должно заканчиваться суффиксом *контроллера*. Например, можно создать контроллер с именем *ProductController* , но не контроллер с именем *продукта*.


Если вы создаете контроллер, который отсутствует *контроллера* суффикса, то вы не сможете заставить контроллер. Не сделать, — я потрачены впустую бесконечные часы мою жизнь после совершения этой ошибки.


**В листинге 1 - Controllers\ProductController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample1.vb)]

Всегда следует создавать контроллеры в папку "контроллеры". В противном случае будет нарушения правил ASP.NET MVC и других разработчиков будет сложнее основные сведения о приложении.

### <a name="scaffolding-action-methods"></a>Методы действия формирования шаблонов

При создании контроллера, у вас есть возможность создать методы действий создания, обновления и сведения автоматически (см. рис. 3). Если выбран этот параметр, создается класс контроллера в листинге 2.


[![Автоматическое создание методов действий](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)

**Рис 03**: автоматическое создание методов действий ([Просмотр полноразмерного изображения](creating-a-controller-vb/_static/image6.png))


**В листинге 2 - Controllers\CustomerController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample2.vb)]

Эти созданные методы являются методы-заглушки. Фактическая логика для создания, обновления и с подробными сведениями для клиента самостоятельно, необходимо добавить. Но методы-заглушки дают хороший отправной точки.

### <a name="creating-a-controller-class"></a>Создание класса контроллера

Контроллер ASP.NET MVC — это класс. При желании можно игнорировать удобный формирования шаблонов контроллера Visual Studio и создать класс контроллера вручную. Выполните следующие действия.

1. Щелкните правой кнопкой мыши папку Controllers и выберите пункт меню **добавить, новый элемент** и выберите **класс** шаблона (см. рис. 4).
2. Назовите новый класс PersonController.vb и нажмите кнопку **добавить** кнопки.
3. Измените полученный файл, класс наследуется от базового класса System.Web.Mvc.Controller класса (см. Листинг 3).


[![Создание нового класса](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)

**Рис. 04**: Создание нового класса ([Просмотр полноразмерного изображения](creating-a-controller-vb/_static/image8.png))


**Листинг 3 - Controllers\PersonController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample3.vb)]

Контроллер в листинге 3 предоставляет одно действие с именем Index(), которое возвращает строку «Hello World!». Вы можете вызвать это действие контроллера, для запуска приложения и запрос URL-адрес следующего вида:

`http://localhost:40071/Person`

> [!NOTE]
> 
> ASP.NET Development Server использует случайный номер порта (например, 40071). При вводе URL-адрес для вызова контроллера, необходимо указать номер порта справа. Можно определить номер порта, наведя указатель мыши на значок для сервера разработки ASP.NET в области уведомлений Windows (правом верхнем углу экрана).
> 
> [!div class="step-by-step"]
> [Назад](adding-dynamic-content-to-a-cached-page-vb.md)
> [Вперед](creating-an-action-vb.md)
