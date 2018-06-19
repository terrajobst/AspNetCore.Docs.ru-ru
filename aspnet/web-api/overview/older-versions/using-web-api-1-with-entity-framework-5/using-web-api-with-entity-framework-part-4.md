---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'Часть 4: Добавление представления Admin | Документы Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: cbf42f1dbd744d7b85dde7d2dcd99a13c6208a13
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879534"
---
<a name="part-4-adding-an-admin-view"></a>Часть 4: Добавление представления администрирования
====================
по [Mike Wasson](https://github.com/MikeWasson)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a>Добавление представления администрирования

Теперь мы будем включения на стороне клиента и добавление страницы, можно использовать данные из контроллера администратора. Страница позволит создать, изменить или удалить продукты, отправляя запросы AJAX к контроллеру.

В обозревателе решений разверните папку Controllers и откройте файл с именем HomeController.cs. Этот файл содержит контроллер MVC. Добавьте метод с именем `Admin`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

**HttpRouteUrl** метод создает URI веб-API, и мы сохраните его в пакет представления для использования в дальнейшем.

Затем поместите курсор текста внутри `Admin` метод действия, а затем щелкните правой кнопкой мыши и выберите **добавить представление**. Данная команда откроет **добавить представление** диалогового окна.

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

В **добавить представление** диалога, имя представления «Admin». Установите флажок **создать строго типизированное представление**. В разделе **класс модели**, выберите «Product (ProductStore.Models)». Оставьте все параметры как значения по умолчанию.

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

Щелкнув **добавить** добавляет в файл с именем Admin.cshtml в области представления/домашние. Откройте этот файл и добавьте следующий код HTML. Следующий код HTML определяет структуру страницы, но еще подключено не добавляет новых функций.

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a>Создать ссылку на страницу администрирования

В обозревателе решений разверните папку Views, а затем разверните папку Shared. Откройте файл с именем \_Layout.cshtml. Найдите **ul** элемент с id = «menu» и ссылку действия для представления администрирования:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> В образце проекта внесены несколько других финальных изменений, например замены строки «Ваша эмблема здесь». Они не влияют на функциональные возможности приложения. Можно загрузить проект и сравните файлы.


Запустите приложение и щелкните ссылку «Admin», которая появляется в верхней части домашней страницы. Страницы администрирования должна выглядеть следующим образом:

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

Право, страницы не выполняет никаких действий. В следующем разделе мы будем использовать Knockout.js для создания динамического пользовательского интерфейса.

## <a name="add-authorization"></a>Добавьте авторизацию

Страницы администрирования в настоящее время доступен любому пользователю получить на узле. Позволяет изменить этот параметр, чтобы ограничить разрешения для администраторов.

Начните с добавления роли «Администратор» и пользователя-администратора. В обозревателе решений разверните папку «фильтры» и откройте файл с именем InitializeSimpleMembershipAttribute.cs. Найдите `SimpleMembershipInitializer` конструктор. После вызова **WebSecurity.InitializeDatabaseConnection**, добавьте следующий код:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

Это скорую руку способ добавления роли «Администратор» и создайте пользователя для роли.

В обозревателе решений разверните папку Controllers, а затем откройте файл HomeController.cs. Добавить **авторизовать** атрибут `Admin` метод.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

Откройте файл AdminController.cs и добавьте **авторизовать** атрибут ко всему `AdminController` класса.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> MVC и веб-API, как определить **авторизовать** атрибуты в разных пространствах имен. MVC использует **System.Web.Mvc.AuthorizeAttribute**, а веб-API использует **System.Web.Http.AuthorizeAttribute**.


Теперь только администраторы могут просматривать страницы администрирования. Кроме того при отправке запроса HTTP к контроллеру Admin, запрос должен содержать файл cookie проверки подлинности. В противном случае сервер отправляет ответ HTTP 401 (не санкционировано). Вы увидите это на Fiddler, отправив запрос GET к `http://localhost:*port*/api/admin`.

> [!div class="step-by-step"]
> [Назад](using-web-api-with-entity-framework-part-3.md)
> [Вперед](using-web-api-with-entity-framework-part-5.md)
